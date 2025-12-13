---
source: hackernews
title: Async DNS
url: https://flak.tedunangst.com/post/async-dns
date: 2025-12-13
---

[flak](/)
[rss](/rss)
[random](/random)

## async dns

curl experimented with using pthread\_cancel to timeout async DNS requests and [it blew up](https://eissing.org/icing/posts/rip_pthread_cancel/). What else can we do?

Out of curiosity, I decided to review some alternatives and see how they work. My personal priorities are control over events; no background threads or signals or secret mechanisms.

### getaddrinfo

The tried and true classic technique is to call *getaddrinfo* in a thread. Probably with more than one thread so you don’t get stuck behind a single slow request, but probably not boundless either. You can also use a separate process if you don’t use threads.

This is probably good enough for many uses.

### getaddrinfo\_a

glibc provides *getaddrinfo\_a* which basically does the thread dance for you. Some of it. It comes with some caveats, and it’s distinctly non portable, and probably doesn’t mesh with your idea of an event loop. Passing.

### c-ares

[c-ares](https://c-ares.org/) is a standalone DNS library. It supports async queries via a threaded backend or an event driven system. I think the thread backend has the same issues, in that it uses a callback and then you need to push the results back into your application.

Alas, the event system uses lots of callbacks as well. This also includes some dire warnings in the documentation. “When the associated callback is called, it is called with a channel lock so care must be taken to ensure any processing is minimal to prevent DNS channel stalls.” Everyone knows the ideal callback just sets a flag, etc., but also everyone is inevitably tempted to do just one more thing, and hey look, it works fine, wait, why did it break. And thus I have a strong preference for library interfaces where you call into it, get some results, but any time you’re in your own code, you’re free to do what you want.

But worth a try. Based on the [sample code](https://c-ares.org/docs.html) I wrote the quickest dirtiest demo I could.

c-ares code

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <poll.h>
#include <arpa/inet.h>

#include <ares.h>

struct server {
    char name[32];
    char ip[16];
    int status;
};

struct everything {
    struct server servers[1];
    int nservers;
    struct pollfd pfds[4];
    int npfds;
};

static void
addrinfo_cb(void *arg, int status, int timeouts, struct ares_addrinfo *result)
{
    struct server *server = arg;
    server->status = 3;
    if (!result)
        return;
    for (struct ares_addrinfo_node *node = result->nodes; node != NULL; node = node->ai_next) {
        if (node->ai_family == AF_INET) {
            struct sockaddr_in *in_addr = (void *)node->ai_addr;
            inet_ntop(node->ai_family, &in_addr->sin_addr, server->ip, sizeof(server->ip));        }
    }
}

static void
socket_cb(void *arg, ares_socket_t fd, int readable, int writable)
{
    struct everything *state = arg;
    printf("socket: %d r/w: %d %d\n", fd, readable, writable);

    int idx = -1;
    for (int i = 0; i < 4; i++) {
        if (state->pfds[i].fd == fd) {
            idx = i;
            break;
        }
    }
    if (idx == -1) {
        for (int i = 0; i < 4; i++) {
            if (state->pfds[i].fd == -1) {
                idx = i;
                state->pfds[idx].fd = fd;
                state->npfds++;
                break;
            }
        }
    }
    if (idx == -1)
        abort();

    if (!readable && !writable) {
        state->pfds[idx].fd = -1;
        state->npfds--;
        return;
    }
    state->pfds[idx].fd = fd;
    state->pfds[idx].events = 0;
    if (readable)
        state->pfds[idx].events |= POLLIN;
    if (writable)
        state->pfds[idx].events |= POLLOUT;
}

int
main(int argc, char **argv)
{
    struct everything state;
    memset(&state, 0, sizeof(state));
    strlcpy(state.servers[0].name, argv[1], sizeof(state.servers[0].name));
    state.servers[0].status = 1;
    state.nservers = 1;
    for (int i = 0; i < 4; i++)
        state.pfds[i].fd = -1;

    ares_library_init(ARES_LIB_INIT_ALL);

    struct ares_options options;
    memset(&options, 0, sizeof(options));
    int optmask = 0;
    options.flags = ARES_FLAG_EDNS | ARES_FLAG_DNS0x20;
    optmask |= ARES_OPT_FLAGS;
    options.sock_state_cb = socket_cb;
    options.sock_state_cb_data = &state;
    optmask |= ARES_OPT_SOCK_STATE_CB;

    ares_channel_t *channel;
    ares_init_options(&channel, &options, optmask);

    ares_fd_events_t ares_fds[1];

    while (1) {
        printf("top of loop\n");
        for (int i = 0; i < state.nservers; i++) {
            printf("processing server %d\n", i);
            struct server *server = &state.servers[i];
            switch (server->status) {
            case 1:
                {
                    struct ares_addrinfo_hints hints;
                    memset(&hints, 0, sizeof(hints));
                    hints.ai_family = AF_UNSPEC;
                    hints.ai_flags  = ARES_AI_CANONNAME;
                    ares_getaddrinfo(channel, argv[1], NULL, &hints, addrinfo_cb, server);
                    server->status = 2;
                }
                break;
            case 2:
                printf("woke up while working\n");
                break;
            case 3:
                printf("got it, done: %s -> %s\n", server->name, server->ip);
                return 0;
            }
        }
        if (state.npfds == 0) {
            printf("confused. nothing to poll\n");
            return 1;
        }
        int res = poll(state.pfds, 4 /* state.npfds */, 2000);
        printf("poll results: %d\n", res);
        if (res > 0) {
            ares_fd_events_t events[4];
            int nevents = 0;
            for (int i = 0; i < 4 /* state.npfds */; i++) {
                if (!state.pfds[i].revents)
                    continue;
                events[nevents].fd = state.pfds[i].fd;
                events[nevents].events = 0;
                if (state.pfds[i].revents & (POLLERR|POLLHUP|POLLIN))
                    events[nevents].events |= ARES_FD_EVENT_READ;
                if (state.pfds[i].revents & (POLLOUT))
                    events[nevents].events |= ARES_FD_EVENT_WRITE;
                nevents++;
            }
            ares_process_fds(channel, events, nevents, 0);
        }
    }
}
```

It’s okay, but the callbacks are annoying. Notifying me which descriptors need watching means I’m required to pack up my poll structure so I can access it in the callbacks, etc. Everything gets bound just a little bit tighter.

### wadns

Among the [alternatives](https://c-ares.org/why.html) the c-ares project helpfully lists, is [dns.c](https://25thandclement.com/~william/projects/dns.c.html). This sounds enticing.

On the downside, it’s not clear where the demo code stops and the functional code begins. As in, there’s a getaddrinfo sample, but it incorporates a lot of other code that doesn’t seem to be public. The public header doesn’t actually expose a means to interface with an event loop. The code is meant to be integrated into a project, which is understandable and even advantageous, but it means no demo today.

### asr

The asr code was written for smtpd in OpenBSD. It doesn’t use threads and requires the caller to push events. Unfortunately, a portable version currently only exists in the [OpenSMTPD](https://github.com/OpenSMTPD/OpenSMTPD) repo. On the plus side, it’s used as the basis for the libc resolver in OpenBSD, which means the “sample” code to replace *getaddrinfo* literally is getaddrinfo.c.

I rewrote the c-ares demo to use asr. It comes out quite a bit shorter, and I think clearer as well.

asr code

```
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <poll.h>
#include <netdb.h>
#include <asr.h>
#include <arpa/inet.h>

struct server {
    char name[32];
    char ip[16];
    int status;
    struct asr_query *aq;
    int ar_fd;
};

int
main(int argc, char **argv)
{
    struct server servers[1] = {};
    strlcpy(servers[0].name, argv[1], sizeof(servers[0].name));
    servers[0].status = 1;
    int nservers = 1;

    while (1) {
        struct pollfd pfds[4];
        int npfds = 0;
        printf("top of loop\n");
        for (int i = 0; i < nservers; i++) {
            printf("processing server %d\n", i);
            struct server *server = &servers[i];
            switch (server->status) {
            case 1:
                {
                    struct addrinfo hints;
                    memset(&hints, 0, sizeof(hints));
                    hints.ai_family = AF_UNSPEC;
                    hints.ai_socktype = SOCK_STREAM;
                    server->aq = getaddrinfo_async(server->name, "80", &hints, NULL);
                    server->status = 2;
                }
                // fallthrough
          case 2:
                {
                    printf("ready to run\n");
                    struct asr_result ar;
                    int rv = asr_run(server->aq, &ar);
                    switch (rv) {
                    case 0:
                        pfds[npfds].fd = ar.ar_fd;
                        pfds[npfds].events = 0;
                        if (ar.ar_cond == ASR_WANT_READ)
                            pfds[npfds].events = POLLIN;
                        else
                            pfds[npfds].events = POLLOUT;
                        npfds++;
                        server->ar_fd = ar.ar_fd;
                        server->status = 3;
                        break;
                    case 1:
                        {
                            struct addrinfo *res;
                            for (res = ar.ar_addrinfo; res; res = res->ai_next) {
                                if (res->ai_family == AF_INET) {
                                    struct sockaddr_in *in_addr = (void *)res->ai_addr;
                                    inet_ntop(res->ai_family, &in_addr->sin_addr, server->ip, sizeof(server->ip));
                                }
                            }
                            server->status = 4;
                        }
                        break;
                    }
                }
                break;
            case 3:
                printf("woke up while working\n");
                break;
            case 4:
                printf("got it, done: %s -> %s\n", server->name, server->ip);
                return 0;
            }
        }
        if (npfds == 0)
            continue;
        int res = poll(pfds, npfds, 2000);
        printf("poll results: %d\n", res);
        if (res > 0) {
            for (int i = 0; i < npfds; i++) {
                if (!pfds[i].revents)
                    continue;
                for (int j = 0; j < nservers; j++) {
                    if (pfds[i].fd == servers[j].ar_fd)
                        servers[j].status = 2;
                }
            }
        }
    }
}
```

I like this API. It’s very much like *read* or *write* in that it either gives you an answer, or tells you to come back later, and then it’s up to you to decide when that is.

Posted 25 Sep 2025 18:33 by tedu Updated: 25 Sep 2025 18:33

Tagged: [c](/t/c) [programming](/t/programming)

V

V

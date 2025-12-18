---
source: hackernews
title: I got hacked: My Hetzner server started mining Monero
url: https://blog.jakesaunders.dev/my-server-started-mining-monero-this-morning/
date: 2025-12-18
---

[![Unfinished Side Projects](/assets/images/logo.png)](/)

* [Blog](/index.html)
* [About](/about)
* [GitHub](https://github.com/JakeWritesCode)
* [LinkedIn](https://www.linkedin.com/in/jake-saunders-83617741/)

# Unfinished Side Projects

Jake Saunders personal blog.

# I got hacked, my server started mining Monero this morning.

![I got hacked, my server started mining Monero this morning.](/assets/images/posts/side-eye.jpg)

> Edit: A few people on HN have pointed out that this article sounds a little LLM generated. That’s because
> it’s largely a transcript of me panicking and talking to Claude. Sorry if it reads poorly, the incident really
> happened though!

*Or: How I learned that “I don’t use Next.js” doesn’t mean your dependencies don’t use Next.js*

## 8:25 AM: The Email

I woke up to this beauty from Hetzner:

> Dear Mr Jake Saunders,

> We have indications that there was an attack from your server.
> Please take all necessary measures to avoid this in the future and to solve the issue.

> We also request that you send a short response to us. This response should contain information about how this could
> have happened and what you intend to do about it.
> In the event that the following steps are not completed successfully, your server can be blocked at any time after the
> 2025-12-17 12:46:15 +0100.

> Attached was evidence of network scanning from my server to some IP range in Thailand. Great. Nothing says “good
> morning” like an abuse report and the threat of getting your infrastructure shut down in 4 hours.

Background: I run a Hetzner server with Coolify. It runs all my *stuff*, like my little corner of the internet:

* [My IoT Side Project](https://inventronix.club/connect)
* This Blog
* Analytics
* My dads site (he’s an electrician)

## 8:30 AM: Oh Fuck

First thing I did was SSH in and check the load average:

```
|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` $ w  08:25:17 up 55 days, 17:23,  5 users,  load average: 15.35, 15.44, 15.60 ``` |
```

For context, my load average is normally around 0.5-1.0. **Fifteen** is “something is very wrong.”

I ran `ps aux` to see what was eating my CPU:

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND 1001      714822  819  3.6 2464788 2423424 ?     Sl   Dec16 9385:36 /tmp/.XIN-unix/javae 1001       35035  760  0.0      0     0 ?        Z    Dec14 31638:25 [javae] <defunct> 1001     3687838  586  0.0      0     0 ?        Z    Dec07 82103:58 [runnv] <defunct> 1001     4011270  125  0.0      0     0 ?        Z    Dec11 10151:54 [xmrig] <defunct> 1001       35652 62.3  0.0      0     0 ?        Z    Dec12 4405:17 [xmrig] <defunct> ``` |
```

**819% CPU usage.** On a process called `javae` running from `/tmp/.XIN-unix/`. And multiple `xmrig` processes - that’s
literally cryptocurrency mining software (Monero, specifically).

I’d been mining cryptocurrency for someone since December 7th. For **ten days**. Brilliant.

## The Investigation

My first thought was “I’m completely fucked.” Cryptominers on the host, running for over a week - time to nuke
everything from orbit and rebuild, right?

But then I noticed something interesting. All these processes were running as user `1001`. Not root. Not a system user.
UID 1001.

Let me check what’s actually running:

```
|  |  |
| --- | --- |
| ``` 1 ``` | ``` $ docker ps ``` |
```

I’ve got about 20 containers running via Coolify (my self-hosted PaaS). Inventronix (my IoT platform), some monitoring
stuff, Grafana, a few experiments.

And **Umami** - a privacy-focused analytics tool I’d re-deployed 9 days ago to track traffic on my blog.

Wait. 9 days ago. The malware started December 7th. Same timeline.

Let me check which container has user 1001:

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` $ docker ps -q | while read container; do   echo "=== $container ==="   docker exec $container ls -la /app/node_modules/next/dist/server/lib/ 2>/dev/null | grep xmrig done ``` |
```

Output:

```
|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` === a42f72cb1bc5 === drwxr-xr-x    2 nextjs   nogroup       4096 Dec 17 05:11 xmrig-6.24.0 ``` |
```

**There it is.** Container `a42f72cb1bc5` - that’s my Umami analytics container. And it’s got a whole `xmrig-6.24.0`
directory sitting in what should be Next.js server internals.

The mining command in the process list confirmed it:

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` /app/node_modules/next/dist/server/lib/xmrig-6.24.0/xmrig    --url auto.c3pool.org:443    --user 8Bt9BEG98SbBPNTp1svQtDQs7PMztqzGoNQHo58eaUYdf8apDkbzp8HbLJH89fMzzciFQ7fb4ZiqUbymDZR6S9asKHZR6wn    --pass WUZHRkYOHh1GW1RZWBxaWENRX0ZBWVtdSRxQWkBWHg==    --donate-level 0 ``` |
```

Someone had exploited my analytics container and was mining Monero using my CPU. Nice.

## Wait, I Don’t Use Next.js

Here’s the kicker. A few days ago I saw
a [Reddit post](https://www.reddit.com/r/nextjs/comments/1pgiaj3/i_got_hacked_and_traced_how_much_money_hacker/) about a
critical Next.js/Puppeteer RCE vulnerability (
CVE-2025-66478). My immediate reaction was “lol who cares, I don’t run Next.js.”

> Oh my sweet summer child.

Except… Umami **is built with Next.js**. I did not know this, nor did I bother looking. Oops.

The vulnerability (CVE-2025-66478) was in Next.js’s React Server Components deserialization. The “Flight” protocol that
RSC uses to serialize/deserialize data between client and server had an unsafe deserialization flaw. An attacker could
send a specially crafted HTTP request with a malicious payload to any App Router endpoint, and when deserialized, it
would execute arbitrary code on the server.

No Puppeteer involved - just broken deserialization in the RSC protocol itself. The attack flow:

1. Attacker sends crafted HTTP request to Umami’s Next.js endpoint
2. RSC deserializes the malicious payload
3. RCE achieved via unsafe deserialization
4. Download and install cryptominers
5. Profit (for them)

So much for “I don’t use Next.js.”

## The Panic: Has It Escaped the Container?

This is where I started to properly panic. Looking at that process list:

```
|  |  |
| --- | --- |
| ``` 1 ``` | ``` 1001      714822  819  3.6 2464788 2423424 ?     Sl   Dec16 9385:36 /tmp/.XIN-unix/javae ``` |
```

That path - `/tmp/.XIN-unix/javae` - looks like it’s on the **host filesystem**, not inside a container. If the malware
had escaped the container onto my actual server, I’d need to:

1. Assume everything is compromised
2. Check for rootkits, backdoors, persistence mechanisms
3. Probably rebuild from scratch
4. Spend my entire day unfucking this

I checked for persistence mechanisms:

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 ``` | ``` $ crontab -l no crontab for root  $ systemctl list-unit-files | grep enabled # ... all legitimate system services, nothing suspicious ``` |
```

No malicious cron jobs. No fake systemd services pretending to be `nginxs` or `apaches` (common trick to blend in).
That’s… good?

But I still needed to know: **Did the malware actually escape the container or not?**

## The Moment of Truth

Here’s the test. If `/tmp/.XIN-unix/javae` exists on my host, I’m fucked. If it doesn’t exist, then what I’m seeing is
just Docker’s default behavior of showing container processes in the host’s `ps` output, but they’re actually isolated.

```
|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` $ ls -la /tmp/.XIN-unix/javae ls: cannot access '/tmp/.XIN-unix/javae': No such file or directory ``` |
```

**IT NEVER ESCAPED.**

The malware was entirely contained within the Umami container. When you run `ps aux` on a Docker host, you see processes
from all containers because they share the same kernel. But those processes are in their own mount namespace - they
can’t see or touch the host filesystem.

Let me verify what user that container was actually running as:

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 ``` | ``` $ docker inspect umami-bkc4kkss848cc4kw4gkw8s44 | grep '"User"' "User": "nextjs",  $ docker inspect umami-bkc4kkss848cc4kw4gkw8s44 | grep '"Privileged"' "Privileged": false,  $ docker inspect umami-bkc4kkss848cc4kw4gkw8s44 | grep -A 30 "Mounts" "Mounts": [], ``` |
```

**This is why I’m not fucked:**

* Container ran as user `nextjs` (UID 1001), not root ✅
* Container was not privileged ✅
* Container had **zero volume mounts** ✅

The malware could:

* Run processes inside the container ✅
* Mine cryptocurrency ✅
* Scan networks (hence the Hetzner abuse report) ✅
* Consume 100% CPU ✅

The malware could NOT:

* Access the host filesystem ❌
* Install cron jobs ❌
* Create systemd services ❌
* Persist across container restarts ❌
* Escape to other containers ❌
* Install rootkits ❌

Container isolation actually worked. Nice.

## Why This Matters: Dockerfiles vs. Auto-Generated Images

Here’s the thing that saved me. I write my own Dockerfiles for my applications. I don’t use auto-generation tools like
Nixpacks (which Coolify supports) that default to `USER root` in containers.

The Reddit post I’d seen earlier? That guy got completely owned because his container was running as root. The malware
could:

* Install cron jobs for persistence
* Create systemd services
* Write anywhere on the filesystem
* Survive reboots

His fix required a full server rebuild because he couldn’t trust anything anymore. Mine required… deleting a
container.

What I did not do, was keep track of the tolling I was using and what tooling *that* was using. In fact, I installed
Umami from Coolify’s services screen. I didn’t even configure it.

Obviously none of this is Umami’s fault by the way. They released a fix for their free software like a week ago. I just
didn’t think to do anything about it.

## The Fix

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 ``` | ``` # Stop and remove the compromised container $ docker stop umami-bkc4kkss848cc4kw4gkw8s44 $ docker rm umami-bkc4kkss848cc4kw4gkw8s44  # Check CPU usage $ uptime  08:45:17 up 55 days, 17:43,  1 user,  load average: 0.52, 1.24, 4.83 ``` |
```

CPU back to normal. All those cryptomining processes? Gone. They only existed inside the container.

I also enabled UFW (which I should have done ages ago):

```
|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` $ sudo ufw default deny incoming $ sudo ufw default allow outgoing $ sudo ufw allow ssh $ sudo ufw allow 80/tcp $ sudo ufw allow 443/tcp $ sudo ufw enable ``` |
```

This blocks all inbound connections except SSH, HTTP, and HTTPS. No more exposed PostgreSQL ports, no more RabbitMQ
ports open to the internet.

I sent Hetzner a brief explanation:

> Investigation complete. The scanning originated from a compromised Umami analytics container (CVE-2025-66478 -
> Next.js/Puppeteer RCE).
>
> The container ran as non-root user with no privileged access or host mounts, so the compromise was fully contained.
> Container has been removed and firewall hardened.

They closed the ticket within an hour.

## Lessons Learned

### 1. “I don’t use X” doesn’t mean your dependencies don’t use X

I don’t write Next.js applications. But I run third-party tools that are built with Next.js. When CVE-2025-66478 was
disclosed, I thought “not my problem.” Wrong.

Know what your dependencies are actually built with. That “simple analytics tool” is a full web application with a
complex stack.

### 2. Container isolation works (when configured properly)

This could have been so much worse. If that container had been running as root, or had volume mounts to sensitive
directories, or had access to the Docker socket, I’d be writing a very different blog post about rebuilding my entire
infrastructure.

Instead, I deleted one container and moved on with my day.

**Write your own Dockerfiles.** Understand what user your processes run as. Avoid `USER root` unless you have a very
good reason. Don’t mount volumes you don’t need. Don’t give containers `--privileged` access.

### 3. The sophistication gap

This malware wasn’t like those people who auto-poll for `/wpadmin` *every* time I make a DNS change. This was spicy.

* Disguised itself in legitimate-looking paths (`/app/node_modules/next/dist/server/lib/`)
* Used process names that blend in (`javae`, `runnv`)
* Attempted to establish persistence
* According to other reports, even had “killer scripts” to murder competing miners

But it was still limited by container isolation. Good security practices beat sophisticated malware.

### 4. Defense in depth matters

Even though the container isolation held, I still should have:

* Had a firewall enabled from day one (not “I’ll do it later”)
* Been running fail2ban to stop those SSH brute force attempts
* Had proper monitoring/alerting (I only noticed because of the Hetzner email)
* Updated Umami when the CVE was disclosed

I got lucky. Container isolation saved me from my own laziness.

## What I’m Doing Differently

1. **No more Umami.** I’m salty. The CVE was disclosed, they patched it, but I’m not running Next.js-based analytics
   anymore. Considering GoatCounter (written in Go) or just parsing server logs with GoAccess.
2. **Audit all third-party containers.** Going through everything I run and checking:
   * What user does it run as?
   * What volumes does it have?
   * When was it last updated?
   * Do I actually need it?
3. **SSH hardening.** Moving to key-based authentication only, disabling password auth, and setting up fail2ban.
4. **Proper monitoring.** Setting up alerts for CPU usage, load average, and suspicious network activity. I shouldn’t
   find out about compromises from my hosting provider.
5. **Regular security updates.** No more “I’ll update it later.” If there’s a CVE, I patch or I remove the service.

## The Weird Silver Lining

This was actually a pretty good learning experience. I got to:

* Practice incident response on a real compromise
* Prove that container isolation actually works
* Learn about Docker namespaces, user mapping, and privilege boundaries
* Harden my infrastructure without the pressure of active data loss

And I only lost about 2 hours of my morning before work. Could’ve been way worse.

Though I do wonder how much Monero I mined for that dickhead. Based on the CPU usage and duration… probably enough for
them to have a nice lunch. You’re welcome, mysterious attacker. Hope you enjoyed it.

## TL;DR

* Umami analytics (built with Next.js) had a Puppeteer RCE vulnerability
* Got exploited, installed cryptominers
* Mined Monero for 10 days at 1000%+ CPU
* Container isolation saved me because it ran as non-root with no mounts
* Fix: `docker rm umami` and enable firewall
* Lesson: Know what your dependencies are built with, and configure containers properly

17 Dec 2025

* [Self Hosting](/categories#Self-Hosting)
* [Work](/categories#Work)

* [#Engineering](/tags#Engineering)
* [#Hetzner](/tags#Hetzner)
* [#Self Hosting](/tags#Self-Hosting)
* [#Software Development](/tags#Software-Development)

[« Fix for google search console 'sitemap could not be read'](//fix-google-sitemap-could-not-be-read/)

Please enable JavaScript to view the [comments powered by Disqus.](http://disqus.com/?ref_noscript)
[comments powered by Disqus](http://disqus.com)

## Explore →

[Work (6)](/categories#Work)
[Ramblings (3)](/categories#Ramblings)
[Projects (2)](/categories#Projects)
[Arduino (2)](/categories#Arduino)
[Hydroponics (2)](/categories#Hydroponics)
[Rant (1)](/categories#Rant)
[Webdev (2)](/categories#Webdev)
[Guides (2)](/categories#Guides)
[Software Development (2)](/categories#Software-Development)
[DIY (1)](/categories#DIY)
[Self Hosting (1)](/categories#Self-Hosting)

Copyright © 2025 Unfinished Side Projects

[Mediumish Jekyll
Theme](https://www.wowthemes.net/mediumish-free-jekyll-template/) by WowThemes.net

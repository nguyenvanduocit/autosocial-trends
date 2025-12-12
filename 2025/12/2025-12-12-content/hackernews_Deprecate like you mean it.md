---
source: hackernews
title: Deprecate like you mean it
url: https://entropicthoughts.com/deprecate-like-you-mean-it
date: 2025-12-12
---

# [Entropic Thoughts](/)

## Deprecate Like You Mean It

* [Home](/)
* [Archive](/archive)
* [Tags](/tags)
* [About](/about)
* [xkqr.org](https://xkqr.org/)

# Deprecate Like You Mean It

by kqr,
published 2025-12-11

Tags:

* [programming](tags.html#programming)

Seth Larson noticed that [people don’t act on deprecation warnings](https://sethmlarson.dev/deprecations-via-warnings-dont-work-for-python-libraries). The
`response.getheader` method in `urllib` has been deprecated since 2023 because
the `response.headers` dictionary is what should be used instead. When the
method was eventually removed, lots of code broke.

---

Deprecation warnings try to solve the fat step function associated with
backwards-incompatible api changes, by allowing people to schedule the
maintenance burden, rather than having it imposed on them suddenly all at once.
The problem is the economic cost of waiting is not tangible. You *can* ignore
the deprecation warning right up until the api change happens, and then it
becomes very expensive to delay it further.

People aren’t great at planning for sudden changes.

---

What if we intentionally made deprecated functions return the wrong result …
sometimes? Every time it intentionally returns the wrong result, it logs the
deprecation warning.1[ ] 1 Users that are very sensitive to the correctness of the
results might want to swap the wrong result for an artificial delay instead.

Initially, it should never return the wrong result. But after it’s been
deprecated for a few months, it should start to return the wrong result once
every million invocations, say. That would probably not trigger anyone’s
midnight pager, but it would make it clear that relying on the deprecated
functionality is a bug lurking in the code.

Then after a few more months, turn it up to once every ten thousand invocations.
It’s probably going to start to hurt a little to delay the maintenance. After a
year, make it return the wrong thing once every thousand invocations. At this
point, users can only delay maintenance if it’s an unimportant auxiliary usage.
And finally, as we bump up into the deadline, it should return the wrong thing
every other invocation. Now it’s practically useless, just like when it is
removed.

This makes the deprecated parts of the api increasingly buggy until they’re
removed, and makes the economic tradeoff of when to schedule the maintenance
more immediate to users.

---

In case the sarcasm isn’t clear, it’s better to [leave the warts](you-want-technology-with-warts.html). But it is also
worthwhile to recognise that in terms of effectiveness for driving system
change, signage and warnings are on the bottom of the tier list. We should not
be surprised when they don’t work.

# Sidenotes

[1](#fnr.1)

Users that are very sensitive to the correctness of the
results might want to swap the wrong result for an artificial delay instead.

If you liked this and want more you should
[buy me a coffee](https://buymeacoff.ee/kqr).
That helps me turn my 170+ ideas backlog into articles.

Shoutout to my amazing wife
  without whose support I would never
make it past the first sentence. ♥

S = k log W.

Comments? [Send
me an email](/about).
You can also [subscribe for new articles](https://buttondown.email/entropicthoughts).

---
source: hackernews
title: The Dangers of SSL Certificates
url: https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/
date: 2025-12-28
---

[Skip to content](#content)

[![Unknown's avatar](https://0.gravatar.com/avatar/f2641f12e815a54896f8f2ac04660c52efb896e09131390ad2a6f2f5fca81432?s=80&d=identicon&r=G)](https://surfingcomplexity.blog/)

[Surfing Complexity](https://surfingcomplexity.blog/)

Lorin Hochstein's ramblings about software, complex systems, and incidents.

# The dangers of SSL certificates

[Lorin Hochstein](https://surfingcomplexity.blog/author/lorinh/ "Posts by Lorin Hochstein")

[incidents](https://surfingcomplexity.blog/category/systems/incidents/)

December 27, 2025
2 Minutes

Yesterday, the Bazel team at Google did not have a very Merry Boxing Day. An SSL certificate expired for <https://bcr.bazel.build> and <https://releases.bazel.build>, as shown in this screenshot from the [github issue](https://github.com/bazelbuild/bazel/issues/28101).

[![](https://surfingcomplexity.blog/wp-content/uploads/2025/12/image-4.png?w=1024)](https://surfingcomplexity.blog/wp-content/uploads/2025/12/image-4.png)

This expired certificate apparently broke the build workflow of users who use [Bazel](https://bazel.build/), who were faced with the [following error message](https://github.com/bazelbuild/bazel/issues/28101#issuecomment-3692675937):

```
ERROR: Error computing the main repository mapping: Error accessing registry https://bcr.bazel.build/: Failed to fetch registry file https://bcr.bazel.build/modules/platforms/0.0.7/MODULE.bazel: PKIX path validation failed: java.security.cert.CertPathValidatorException: validity check failed
```

After mitigation, Xùdōng Yáng provided a brief summary of the incident on the Github ticket:

[![](https://surfingcomplexity.blog/wp-content/uploads/2025/12/image-6.png?w=930)](https://surfingcomplexity.blog/wp-content/uploads/2025/12/image-6.png)

Say the words “expired SSL certificate” to any senior software engineer and watch the expression on their face. Everybody in this industry has been bitten by expired certs, including people who work at orgs that use automated certificate renewal. In fact, this very case is an example of an automated certificate renewal system that failed! From the screenshot above:

> it was an **auto-renewal being bricked** due to some new subdomain additions, and the renewal failures didn’t send notifications for whatever reason.

The reality is that SSL certificates are a fundamentally *dangerous* technology, and the Bazel case is a great example of why. With SSL certificates, you usually don’t have the opportunity to build up operational experience working with them, unless something goes wrong. And things don’t go wrong that often with certificates, especially if you’re using automated cert renewal! That means when something does go wrong, you’re effectively starting from scratch to figure out how to fix it, which is not a good place to be. Once again, from that summary:

> And then it took some Bazel team members **who were very unfamiliar with this whole area** to scramble to read documentation and secure permissions…

Now, I don’t know the specifics of the Bazel team composition: it may very well be that they have local SSL certificate expertise on the team, but those members were out-of-office because of the holiday. But even if that’s the case, with an automated set-it-and-forget-it solution, the knowledge isn’t going to spread across the team, because why would it? It just works on its own.

That is, until it stops working. And that’s the other dangerous thing about SSL certificates: the failure mode is the opposite of graceful degradation. It’s not like there’s an increasing percentage of requests that fail as you get closer to the deadline. Instead, in one minute, everything’s working just fine, and in the next minute, every http request fails. There’s no natural signal back to the operators that the SSL certificate is getting close to expiry. To make things worse, there’s no staging of the change that triggers the expiration, because the change is time, and time marches on for everyone. You can’t set the SSL certificate expiration so it kicks in at different times for different cohorts of users.

In other words, SSL certs are a technology with an expected failure mode (expiration) that absolutely maximizes blast radius (a hard failure for 100% of users), without any natural feedback to operators that the system is at imminent risk of critical failure. And with automated cert renewal, you are increasing the likelihood that the responders will not have experience with renewing certificates.

Is it any wonder that these keep biting us?

### Share this:

* [Click to share on X (Opens in new window)
  X](https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/?share=twitter)
* [Click to share on Facebook (Opens in new window)
  Facebook](https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/?share=facebook)

Like Loading...

![Unknown's avatar](https://0.gravatar.com/avatar/f2641f12e815a54896f8f2ac04660c52efb896e09131390ad2a6f2f5fca81432?s=80&d=identicon&r=G)

## Published by Lorin Hochstein

[View all posts by Lorin Hochstein](https://surfingcomplexity.blog/author/lorinh/)

**Published**
December 27, 2025

## Post navigation

[Previous Post Saturation: Waymo edition](https://surfingcomplexity.blog/2025/12/23/saturation-waymo-edition/)

### Leave a comment [Cancel reply](/2025/12/27/the-dangers-of-ssl-certificates/#respond)

Δ

Search

Search

* [lorinhochstein.org](https://lorinhochstein.org)
* [Bluesky](https://bsky.app/profile/norootcause.surfingcomplexity.com)
* [Mastodon](https://hachyderm.io/%40norootcause)

* [The dangers of SSL certificates](https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/)
* [Saturation: Waymo edition](https://surfingcomplexity.blog/2025/12/23/saturation-waymo-edition/)
* [Another way to rate incidents](https://surfingcomplexity.blog/2025/12/22/another-way-to-rate-incidents/)
* [Quick takes on the Triple Zero Outage at Optus – the Schott Review](https://surfingcomplexity.blog/2025/12/21/quick-takes-on-the-triple-zero-outage-at-optus-the-schott-review/)
* [Why I don’t like “Correction of Error”](https://surfingcomplexity.blog/2025/12/20/why-i-dont-like-correction-of-error/)

[Blog at WordPress.com.](https://wordpress.com/?ref=footer_blog)

* [Comment](https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/#respond)
* Reblog
* Subscribe
  Subscribed

  + [![](https://surfingcomplexity.blog/wp-content/uploads/2020/05/surfer_1f3c4.png?w=50) Surfing Complexity](https://surfingcomplexity.blog)

  Join 223 other subscribers

  Sign me up

  + Already have a WordPress.com account? [Log in now.](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fsurfingcomplexity.blog%252F2025%252F12%252F27%252Fthe-dangers-of-ssl-certificates%252F)
* + [![](https://surfingcomplexity.blog/wp-content/uploads/2020/05/surfer_1f3c4.png?w=50) Surfing Complexity](https://surfingcomplexity.blog)
  + Subscribe
    Subscribed
  + [Sign up](https://wordpress.com/start/)
  + [Log in](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fsurfingcomplexity.blog%252F2025%252F12%252F27%252Fthe-dangers-of-ssl-certificates%252F)
  + [Copy shortlink](https://wp.me/p236df-1Qu)
  + [Report this content](https://wordpress.com/abuse/?report_url=https://surfingcomplexity.blog/2025/12/27/the-dangers-of-ssl-certificates/)
  + [View post in Reader](https://wordpress.com/reader/blogs/30291541/posts/7098)
  + [Manage subscriptions](https://subscribe.wordpress.com/)
  + Collapse this bar

##

##

Loading Comments...

Write a Comment...

Email (Required)

Name (Required)

Website

###

%d

![](https://pixel.wp.com/b.gif?v=noscript)

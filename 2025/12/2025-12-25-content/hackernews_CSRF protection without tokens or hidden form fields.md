---
source: hackernews
title: CSRF protection without tokens or hidden form fields
url: https://blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields
date: 2025-12-25
---

[miguelgrinberg.com](/index)

* [Home](/index)
* [My Courses and Books](/post/my-courses-and-books)
* [Consulting](/post/hire-me)
* [About Me](/post/about-me)
* + Light Mode
  + Dark Mode
  + ---
  + System Default

* [![GitHub](/static/social/github.png "GitHub")](http://github.com/miguelgrinberg)
  [![LinkedIn](/static/social/linkedin.png "LinkedIn")](http://www.linkedin.com/in/miguelgrinberg)
  [![Bluesky](/static/social/bluesky.png "Bluesky")](https://bsky.app/profile/miguelgrinberg.com)
  [![Mastodon](/static/social/mastodon.png "Mastodon")](https://mstdn.social/%40miguelgrinberg)
  [![Twitter](/static/social/twitter.png "Twitter")](https://twitter.com/miguelgrinberg)
  [![YouTube](/static/social/youtube.png "YouTube")](https://youtube.com/miguelgrinberg)
  [![Buy Me a Coffee](/static/social/buymeacoffee.png "Buy Me a Coffee")](https://www.buymeacoffee.com/miguelgrinberg)
  [![Patreon](/static/social/patreon.png "Patreon")](https://patreon.com/miguelgrinberg)
  [![RSS Feed](/static/social/rss.png "RSS Feed")](/feed)

# [CSRF Protection without Tokens or Hidden Form Fields](/post/csrf-protection-without-tokens-or-hidden-form-fields)

## Posted by on 2025-12-21T15:54:28Z under

A couple of months ago, I received a [request](https://github.com/miguelgrinberg/microdot/issues/321) from a random Internet user to add [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) protection to my little web framework [Microdot](https://github.com/miguelgrinberg/microdot), and I thought it was a fantastic idea.

When I set off to do this work in early November I expected I was going to have to deal with anti-CSRF tokens, double-submit cookies and hidden form fields, pretty much the traditional elements that we have used to build a defense against CSRF for years. And I did start along this tedious route. But then I bumped into a new way some people are dealing with CSRF attacks that is way simpler, which I describe below.

## Implementing a security feature

An often shared piece of advice is that you should never implement security features yourself. Instead, you should look for well established solutions built by people who think about security day in and day out.

Unfortunately, as the lead (and only) maintainer of Microdot, I do not have an ecosystem of existing solutions available to me. Even though I gladly accept external contributions, most of the framework has been built by myself out of nothing. So in this case, like many other times before, I felt I had no choice but to go against the standard advice and write CSRF protection code by myself, because if I didn't do it then the feature would not be built.

What is the first step when you need to build a security feature? Check out what [OWASP](https://owasp.org/) has to say about the matter.

So, in early November, I opened OWASP's [CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) page to see what was new and interesting in the world of CSRF protection. And I found that nothing of significance had changed.

According to OWASP, the best CSRF protection you could get (at the time I checked) was still built around the idea of using anti-CSRF tokens. So I set off to implement this for Microdot.

## A disturbance in the (CSRF) force

I was happily making progress on my CSRF implementation, and then in early December, another random Internet user dropped [an issue](https://github.com/pallets/flask/issues/5863) on the Flask repository, proposing that Flask adds support for "modern" CSRF protection. Modern? How could there be a new way to protect against CSRF that isn't mentioned by OWASP?

This led me down a rabbit hole of blog posts and discussions spanning the Go and Ruby communities, plus a [long discussion](https://github.com/OWASP/CheatSheetSeries/issues/1803) about this method on the OWASP GitHub repository itself, resulting in a [pull request](https://github.com/OWASP/CheatSheetSeries/pull/1875) that added a [mention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#fetch-metadata-headers) of this method to the CSRF Cheat Sheet, only a couple of weeks after I went to this page looking for guidance for my own implementation.

## Modern CSRF Protection

The so called "modern" method to protect against CSRF attacks is based on the [Sec-Fetch-Site](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Sec-Fetch-Site) header, which all modern desktop and mobile browsers include in the requests they send to servers. According to Mozilla, all browsers released since March 2023 have support for this header.

The `Sec-Fetch-Site` header can have one of four values:

* `same-origin`, when the request comes from the same origin as the target server
* `same-site`, when the request comes from the same site, but not exactly the same origin (e.g. a different subdomain) as the target server
* `cross-site`, when the request comes from an origin that does not match the target server
* `none`, when the request is originated by the user

The value of this header cannot be set via JavaScript, so the server can assume that a) if this header is present, then the client is a web browser, and b) the value of the header can be trusted. So basically, the server can reject requests that come with this header set to `cross-site`, and in essence that is all you need to do to protect against CSRF!

After seeing this, I paused my work on the token-based CSRF implementation and spent a few hours to implement this modern approach. As always, the devil is in the details, so let's see what else I needed to do to build a complete solution.

First of all, in some cases subdomains sharing the same registered domain may operate independently, and as such, it is not out of the question that one subdomain may attempt to attack another through CSRF. Depending on the level of trust an application has for other subdomains, a server may want to block requests that come with the `Sec-Fetch-Site` header set to `same-site`. In Microdot, I have added an argument `allow_subdomains` to cover this case. I decided to err on the side of security, so the default is `False`, meaning that requests from subdomains are also blocked.

The other big problem is that not everyone is using a recent browser that implements this header. Looking at the [browser compatibility](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Sec-Fetch-Site#browser_compatibility) for the `Sec-Fetch-Site` header, you can see that most browsers implemented this feature long ago, between 2019 and 2021, with one notable exception: Safari. Apple added this header to its browser in 2023, so it is reasonable to assume that there are still users out there running older browsers that do not support it.

One option is to reject all requests that do not have the `Sec-Fetch-Site` header. This keeps everyone secure, but of course, there's going to be some unhappy users of old devices that will not be able to use your application. Plus, this would also reject HTTP clients that are not browsers. If this is not a problem for your use case, then great, but it isn't a good solution overall.

From what I gathered from looking at other implementations of this method, an accepted solution is to use the `Origin` header as fallback when `Sec-Fetch-Site` is not implemented, since this header has been around for [much longer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Origin#browser_compatibility). The last of the major browsers to add it were Firefox desktop in 2019 and Edge and Firefox mobile in 2020. Like `Sec-Fetch-Site`, the `Origin` header is also a restricted header that is set by the browser, so it can also be used to determine from where a request is coming from.

The problem with using the `Origin` header is that it isn't always easy to know what is the correct origin that applies to a web application. The standard option is to compare the value of the `Origin` header against the value of the `Host` header, but `Host` only includes the hostname and port, while `Origin` also includes the scheme. Also, the `Host` header is overwritten as it passes through reverse proxies. So comparing these two headers is actually not easy.

Another, more direct option is to ask the user to configure the expected origin name explicitly. To keep things simple, in Microdot I opted for the explicit configuration, for which I linked to the existing Cross-Origin Request Sharing (CORS) support. The CORS feature already maintains a list of allowed origins, so my CSRF logic automatically trusts these. I decided to not complicate myself adding support for `Host` header checks at this time, but maybe I'll add this in the future.

Filippo Valsorda, a security developer active in the Go ecosystem (and author of the popular [mkcert](https://github.com/FiloSottile/mkcert) tool) wrote a [blog post](https://words.filippo.io/csrf/) about this method that you may want to check out if you want to learn more details about it. He seems to be the first to propose this method and has implemented it for the Go standard library.

Also if you are interested, feel free to review my implementation of CSRF protection in Microdot. Have a look at the [documentation](https://microdot.readthedocs.io/en/latest/extensions/csrf.html), the [code](https://github.com/miguelgrinberg/microdot/blob/main/src/microdot/csrf.py) and an [example](https://github.com/miguelgrinberg/microdot/tree/main/examples/csrf), and let me know if you have any improvements or fixes to suggest.

## Let's revisit OWASP

*Note: this section is now out of date. As of December 24th 2025 the OWASP CSRF Cheat Sheet page lists the Fetch Metadata method as a complete solution that can be used as an alternative to token-based approaches.*

As I mentioned above, the CSRF Prevention Cheat Sheet page from OWASP was updated in early December to include the use of the `Sec-Fetch-Site` header in the list of prevention methods. But this method is currently listed as a [defense in depth](https://en.wikipedia.org/wiki/Defence_in_depth) mechanism, and not a complete solution, which I thought was odd.

I referenced the [discussion](https://github.com/OWASP/CheatSheetSeries/issues/1803) in the OWASP GitHub repository that resulted in the recent changes made to the Cheat Sheet page. Several participants in that discussion have suggested that this method should be upgraded to a complete alternative to the standard token-based approaches. The OWASP maintainer was initially skeptical, but towards the end of the thread they have agreed. The [pull request](https://github.com/OWASP/CheatSheetSeries/pull/1875) that closed the discussion added this solution as an alternative to the token-based approaches, but then a later change made significant updates, including the downgrade to defense in depth. My hope is that this is just a misunderstanding, and that the OWASP folks will restore the content as it was agreed by all the parties involved.

In any case, I consider that in Microdot, going from no CSRF support at all to this is a great step forward that is also consistent with the minimalist ethos of the project. I will be keeping an eye on the OWASP CSRF Cheat Sheet page to see what is their final word on this new protection method, and if they end up keeping it as defense in depth, I still have a mostly complete implementation of double-submit anti-CSRF tokens that I can bring into my project.

## Conclusion

What I like the most about working in open source is that all the work happens in the open, so it is a permanent record that can be searched and reviewed. My CSRF protection journey started as a somewhat tedious exercise in the use of cryptography and cookies, but then thanks to an unexpected lead it turned into a fun and exciting learning opportunity for me.

## Buy me a coffee?

Thank you for visiting my blog! If you enjoyed this article, please consider supporting my work and keeping me caffeinated with a small one-time donation through [Buy me a coffee](https://www.buymeacoffee.com/miguelgrinberg). Thanks!

[![Buy Me A Coffee](/static/buymeacoffee-yellow.png)](https://www.buymeacoffee.com/miguelgrinberg)

## Share this post

[Hacker News](https://news.ycombinator.com/submitlink?u=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields&t=CSRF%20Protection%20without%20Tokens%20or%20Hidden%20Form%20Fields)

[Reddit](https://reddit.com/submit/?url=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields&resubmit=true&title=CSRF Protection without Tokens or Hidden Form Fields)

[Twitter](https://twitter.com/intent/tweet/?text=CSRF%20Protection%20without%20Tokens%20or%20Hidden%20Form%20Fields&url=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields)

[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields&title=CSRF%20Protection%20without%20Tokens%20or%20Hidden%20Form%20Fields&summary=CSRF%20Protection%20without%20Tokens%20or%20Hidden%20Form%20Fields&source=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields)

[Facebook](https://facebook.com/sharer/sharer.php?u=https%3A//blog.miguelgrinberg.com/post/csrf-protection-without-tokens-or-hidden-form-fields)

E-Mail

[No comments yet](/post/csrf-protection-without-tokens-or-hidden-form-fields#comments)

### Leave a Comment

Name

Email

Comment

Captcha

Flask Web Development, 2nd Edition

[![Flask Web Development, 2nd Edition](/static/flask2e-book-cover-small.png)](https://amzn.to/3lE2mok)

If you want to learn modern web development techniques with Python and Flask, you may find the second edition of my [O'Reilly book](https://amzn.to/3lE2mok) useful.

[Click here to get this Book!](https://amzn.to/3lE2mok)

About Miguel

![](/static/miguel.jpg)

Categories

[![AI RSS Feed](/static/rss-small.png "AI RSS Feed")](/category/AI/feed)

[![Arduino RSS Feed](/static/rss-small.png "Arduino RSS Feed")](/category/Arduino/feed)

[![Authentication RSS Feed](/static/rss-small.png "Authentication RSS Feed")](/category/Authentication/feed)

[![Blog RSS Feed](/static/rss-small.png "Blog RSS Feed")](/category/Blog/feed)

[![C++ RSS Feed](/static/rss-small.png "C++ RSS Feed")](/category/C%2B%2B/feed)

[![CSS RSS Feed](/static/rss-small.png "CSS RSS Feed")](/category/CSS/feed)

 *1*

[![Cloud RSS Feed](/static/rss-small.png "Cloud RSS Feed")](/category/Cloud/feed)

 *11*

[![Database RSS Feed](/static/rss-small.png "Database RSS Feed")](/category/Database/feed)

 *23*

[![Docker RSS Feed](/static/rss-small.png "Docker RSS Feed")](/category/Docker/feed)

 *5*

[![Filmmaking RSS Feed](/static/rss-small.png "Filmmaking RSS Feed")](/category/Filmmaking/feed)

 *6*

[![Flask RSS Feed](/static/rss-small.png "Flask RSS Feed")](/category/Flask/feed)

 *129*

[![Games RSS Feed](/static/rss-small.png "Games RSS Feed")](/category/Games/feed)

 *1*

[![IoT RSS Feed](/static/rss-small.png "IoT RSS Feed")](/category/IoT/feed)

 *8*

[![JavaScript RSS Feed](/static/rss-small.png "JavaScript RSS Feed")](/category/JavaScript/feed)

 *36*

[![MicroPython RSS Feed](/static/rss-small.png "MicroPython RSS Feed")](/category/MicroPython/feed)

 *10*

[![Microdot RSS Feed](/static/rss-small.png "Microdot RSS Feed")](/category/Microdot/feed)

 *1*

[![Microservices RSS Feed](/static/rss-small.png "Microservices RSS Feed")](/category/Microservices/feed)

 *2*

[![Movie Reviews RSS Feed](/static/rss-small.png "Movie Reviews RSS Feed")](/category/Movie%20Reviews/feed)

 *5*

[![Personal RSS Feed](/static/rss-small.png "Personal RSS Feed")](/category/Personal/feed)

 *3*

[![Photography RSS Feed](/static/rss-small.png "Photography RSS Feed")](/category/Photography/feed)

 *7*

[![Product Reviews RSS Feed](/static/rss-small.png "Product Reviews RSS Feed")](/category/Product%20Reviews/feed)

 *2*

[![Programming RSS Feed](/static/rss-small.png "Programming RSS Feed")](/category/Programming/feed)

 *195*

[![Project Management RSS Feed](/static/rss-small.png "Project Management RSS Feed")](/category/Project%20Management/feed)

 *1*

[![Python RSS Feed](/static/rss-small.png "Python RSS Feed")](/category/Python/feed)

 *175*

[![REST RSS Feed](/static/rss-small.png "REST RSS Feed")](/category/REST/feed)

 *7*

[![Raspberry Pi RSS Feed](/static/rss-small.png "Raspberry Pi RSS Feed")](/category/Raspberry%20Pi/feed)

 *8*

[![React RSS Feed](/static/rss-small.png "React RSS Feed")](/category/React/feed)

 *19*

[![Reviews RSS Feed](/static/rss-small.png "Reviews RSS Feed")](/category/Reviews/feed)

 *1*

[![Robotics RSS Feed](/static/rss-small.png "Robotics RSS Feed")](/category/Robotics/feed)

 *6*

[![Security RSS Feed](/static/rss-small.png "Security RSS Feed")](/category/Security/feed)

 *13*

[![Video RSS Feed](/static/rss-small.png "Video RSS Feed")](/category/Video/feed)

 *22*

[![WebSocket RSS Feed](/static/rss-small.png "WebSocket RSS Feed")](/category/WebSocket/feed)

 *2*

[![Webcast RSS Feed](/static/rss-small.png "Webcast RSS Feed")](/category/Webcast/feed)

 *3*

[![Windows RSS Feed](/static/rss-small.png "Windows RSS Feed")](/category/Windows/feed)

 *1*

© 2012- by Miguel Grinberg. All rights reserved. Questions?

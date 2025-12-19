---
source: hackernews
title: The <time> element should do something
url: https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/
date: 2025-12-19
---

# [Read the Tea Leaves](https://nolanlawson.com/) Software and other dark arts, by Nolan Lawson

![Search](https://s2.wp.com/wp-content/themes/pub/springloaded/images/search-btn.gif?m=1230136840i)

* [Home](https://nolanlawson.com/)
* [Apps](https://nolanlawson.com/apps/)
* [Code](https://nolanlawson.com/code/)
* [Talks](https://nolanlawson.com/talks/)
* [About](https://nolanlawson.com/about/)

« [The fate of “small” open source](https://nolanlawson.com/2025/11/16/the-fate-of-small-open-source/)

14
Dec

## The <time> element should actually do something

Posted December 14, 2025 by Nolan Lawson in [accessibility](https://nolanlawson.com/category/accessibility/), [Web](https://nolanlawson.com/category/web/). Tagged: [html5](https://nolanlawson.com/tag/html5/). [5 Comments](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/#comments)

A common UI pattern is something like this:

```
Post published 4 hours ago
```

People do lots of stuff with that “4 hours ago.” They might make it a permalink:

```
Post published <a href="/posts/123456">4 hours ago</a>
```

Or they might give it a tooltip to show the exact datetime upon hover/focus:

```
Post published
<Tooltip content="December 14, 2025 at 11:30 AM PST">
  4 hours ago
</Tooltip>
```

**Note:** I’m assuming some `Tooltip` component written in your favorite framework, e.g. React, Svelte, Vue, etc. There’s also the bleeding-edge [`popover="hint"`](https://developer.chrome.com/blog/popover-hint) and [Interest Invokers API](https://open-ui.org/components/interest-invokers.explainer/), which would give us a succinct way to do this in native HTML/CSS.

If you’re a pedant about HTML though (like me), then you might use the [`<time>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/time) element:

```
Post published
<time datetime="2025-12-14T19:30:00.000Z">
  4 hours ago
</time>
```

This is great! We now have a semantic way to express the exact timestamp of a date. So browsers and screen readers should use this and give us a way to avoid those annoying manual tooltips and… oh wait, no. The `<time>` element does [approximately nothing](https://stackoverflow.com/q/6852866).

I did some research on this and couldn’t find any browser or assistive technology that actually makes use of the `<time>` element, besides, you know, rendering it. (Whew!) This is despite the fact that `<time>` is used on roughly [8% of pageloads](https://chromestatus.com/metrics/feature/timeline/popularity/1100) per Chrome’s usage tracker.

**Update:** [Léonie Watson helpfully reports](https://w3c.social/%40tink/115736870816175354) that the screen readers NVDA and Narrator actually do read out the timestamp in a human-readable format! So `<time>` does have an impact on accessibility (although [arguably not a positive one](https://dragonscave.space/%40jscholes/115741166091714098)).

So what does `<time>` actually do? As near as I can tell, it’s used by search engines to show date snippets in search results. However, I can’t find any guidelines from Google that specifically advocate for the `<time>` element, although there is [a 2023 post from Search Engine Journal](https://www.searchenginejournal.com/google-recommends-multiple-date-signals-on-webpages/475735/) which quotes a Google Search liaison:

> Google doesn’t depend on a single date factor because all factors can be prone to issues. That’s why our systems look at several factors to determine our best estimate of when a page was published or significantly updated.

In fact, the [only Google documentation](https://developers.google.com/search/docs/appearance/publication-dates) I found doesn’t mention `<time>` at all, and instead recommends using Schema.org’s `datePublished` and `dateModified` fields. (I.e., not even HTML.)

So there it is. `<time>` is a neat idea in theory, but in practice it feels like an unfulfilled promise of [semantic HTML](https://en.wikipedia.org/wiki/Semantic_HTML). A [2010 CSS Tricks article](https://css-tricks.com/time-element/) has a great quote about this from Bruce Lawson (no relation):

> The uses of unambiguous dates in web pages aren’t hard to imagine. A browser could offer to add events to a user’s calendar. A Thai-localised browser could offer to transform Gregorian dates into Thai Buddhist era dates. A Japanese browser could localise 16:00 to “16:00時”.

This would be amazing, and I’d love to see browsers and screen readers make use of `<time>` like this. But for now, it’s just kind of an inert relic of the early HTML5 days. I’ll still use it, though, because ([as Marge Simpson would say](https://knowyourmeme.com/memes/i-just-think-theyre-neat)), I just think it’s neat.

### *Related*

### 5 responses to this post.

1. ![Jayden's avatar](https://2.gravatar.com/avatar/2e67f33dddeff593385075768117ab99f0cbd28cecf2e584877b73b708970d41?s=30&d=https%3A%2F%2F2.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D30&r=G)

   Posted by Jayden on [December 14, 2025 at 4:44 PM](#comment-238095)

   I use the time element for end-to-end tests.

   I have a website that allows users to purchase subscriptions (monthly and yearly).

   Since users are paying for a service, we need to make sure that the paid features are always working.

   The website is also constantly evolving over time which has made it build up a lot of complexity. Changing one thing can cause unexpected issues elsewhere in unrelated sections in really surprising ways.

   Complexity + must always work is a hard space to be in.

   To catch unexpected changes and ensure that things keep working as expected, I made a test suite to get guarantees that features work correctly for logged out users, logged in users and logged in users with subscriptions.

   We use the time element on our pages where users can see their own subscription’s details (e.g. invoice dates, purchased on, will renew on, etc…).

   We display translated and localised times to the users inside a time element, and have a machine readable timestamp in an attribute on the time element. That way our tests can understand the timestamps and check it against the expected values regardless of the simulated user’s timezone, or language/locale.

   [Reply](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/?replytocom=238095#respond)
2. ![Tim McCormack's avatar](https://2.gravatar.com/avatar/5472dcc77fdaf0b551a34aa9d85f2371f22f331cf37a2044e23a22e0513d91f9?s=30&d=https%3A%2F%2F2.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D30&r=G)

   Posted by [Tim McCormack](https://www.brainonfire.net/) on [December 15, 2025 at 6:16 AM](#comment-238098)

   FYI, the text “<time>” in your title isn’t escaped in the feed, so it just says “The element should actually do something”.

   [Reply](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/?replytocom=238098#respond)

   * ![Nolan Lawson's avatar](https://1.gravatar.com/avatar/4ef0fd7e6ff4540febaf58f7a093ee4a4f285dcba7fb1543ebcc17670d8b03e8?s=30&d=https%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D30&r=G)

     Posted by [Nolan Lawson](http://nolanlawson.com) on [December 15, 2025 at 7:48 AM](#comment-238099)

     Yes sadly this seems to be a WordPress thing. “The element should actually do something” is kind of funny though.

     [Reply](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/?replytocom=238099#respond)
3. ![jokeyrhyme's avatar](https://1.gravatar.com/avatar/4f0c7fe95af621c0a4599ed5750d81e4691c0e509fbdb8fb4d9d7090b78bc378?s=30&d=https%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D30&r=G)

   Posted by jokeyrhyme on [December 15, 2025 at 10:56 AM](#comment-238100)

   i’m intensely frustrated that it’s 2025 and i still have to manually convert dates and times to my local timezone :S

   [Reply](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/?replytocom=238100#respond)
4. Posted by [Weekly Reading List December 15 2025 – OZeWAI](https://ozewai.org/blog/ozewai-news/weekly-reading-list-december-15-2025/) on [December 15, 2025 at 2:01 PM](#comment-238101)

   […] The <time> element should actually do something Nolan Lawson: couldn’t find any browser or assistive technology that actually makes use of the <time> element. […]

   [Reply](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/?replytocom=238101#respond)

### Leave a comment [Cancel reply](/2025/12/14/the-time-element-should-actually-do-something/#respond)

Δ

This site uses Akismet to reduce spam. [Learn how your comment data is processed.](https://akismet.com/privacy/)

### Recent Posts

* [The <time> element should actually do something](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/)
* [The fate of “small” open source](https://nolanlawson.com/2025/11/16/the-fate-of-small-open-source/)
* [Why do browsers throttle JavaScript timers?](https://nolanlawson.com/2025/08/31/why-do-browsers-throttle-javascript-timers/)
* [Selfish reasons for building accessible UIs](https://nolanlawson.com/2025/06/16/selfish-reasons-for-building-accessible-uis/)
* [AI ambivalence](https://nolanlawson.com/2025/04/02/ai-ambivalence/)

### About Me

![Photo of Nolan Lawson, headshot](https://nolanlawson.com/wp-content/uploads/2023/01/profile_17.jpg?w=300 "About Me")

I'm Nolan, a programmer from Seattle working at Socket. All opinions are my own. Photo by Cătălin Mariș.

### Archives

* [December 2025](https://nolanlawson.com/2025/12/) (1)
* [November 2025](https://nolanlawson.com/2025/11/) (1)
* [August 2025](https://nolanlawson.com/2025/08/) (1)
* [June 2025](https://nolanlawson.com/2025/06/) (1)
* [April 2025](https://nolanlawson.com/2025/04/) (1)
* [January 2025](https://nolanlawson.com/2025/01/) (1)
* [December 2024](https://nolanlawson.com/2024/12/) (2)
* [October 2024](https://nolanlawson.com/2024/10/) (2)
* [September 2024](https://nolanlawson.com/2024/09/) (3)
* [August 2024](https://nolanlawson.com/2024/08/) (1)
* [July 2024](https://nolanlawson.com/2024/07/) (1)
* [March 2024](https://nolanlawson.com/2024/03/) (1)
* [January 2024](https://nolanlawson.com/2024/01/) (1)
* [December 2023](https://nolanlawson.com/2023/12/) (4)
* [August 2023](https://nolanlawson.com/2023/08/) (2)
* [January 2023](https://nolanlawson.com/2023/01/) (2)
* [December 2022](https://nolanlawson.com/2022/12/) (1)
* [November 2022](https://nolanlawson.com/2022/11/) (2)
* [October 2022](https://nolanlawson.com/2022/10/) (2)
* [June 2022](https://nolanlawson.com/2022/06/) (4)
* [May 2022](https://nolanlawson.com/2022/05/) (3)
* [April 2022](https://nolanlawson.com/2022/04/) (1)
* [February 2022](https://nolanlawson.com/2022/02/) (1)
* [January 2022](https://nolanlawson.com/2022/01/) (1)
* [December 2021](https://nolanlawson.com/2021/12/) (3)
* [September 2021](https://nolanlawson.com/2021/09/) (1)
* [August 2021](https://nolanlawson.com/2021/08/) (6)
* [February 2021](https://nolanlawson.com/2021/02/) (2)
* [January 2021](https://nolanlawson.com/2021/01/) (2)
* [December 2020](https://nolanlawson.com/2020/12/) (1)
* [July 2020](https://nolanlawson.com/2020/07/) (1)
* [June 2020](https://nolanlawson.com/2020/06/) (1)
* [May 2020](https://nolanlawson.com/2020/05/) (2)
* [February 2020](https://nolanlawson.com/2020/02/) (1)
* [December 2019](https://nolanlawson.com/2019/12/) (1)
* [November 2019](https://nolanlawson.com/2019/11/) (1)
* [September 2019](https://nolanlawson.com/2019/09/) (1)
* [August 2019](https://nolanlawson.com/2019/08/) (2)
* [June 2019](https://nolanlawson.com/2019/06/) (4)
* [May 2019](https://nolanlawson.com/2019/05/) (3)
* [February 2019](https://nolanlawson.com/2019/02/) (2)
* [January 2019](https://nolanlawson.com/2019/01/) (1)
* [November 2018](https://nolanlawson.com/2018/11/) (1)
* [September 2018](https://nolanlawson.com/2018/09/) (5)
* [August 2018](https://nolanlawson.com/2018/08/) (1)
* [May 2018](https://nolanlawson.com/2018/05/) (1)
* [April 2018](https://nolanlawson.com/2018/04/) (1)
* [March 2018](https://nolanlawson.com/2018/03/) (1)
* [January 2018](https://nolanlawson.com/2018/01/) (1)
* [December 2017](https://nolanlawson.com/2017/12/) (1)
* [November 2017](https://nolanlawson.com/2017/11/) (2)
* [October 2017](https://nolanlawson.com/2017/10/) (1)
* [August 2017](https://nolanlawson.com/2017/08/) (1)
* [May 2017](https://nolanlawson.com/2017/05/) (1)
* [March 2017](https://nolanlawson.com/2017/03/) (1)
* [January 2017](https://nolanlawson.com/2017/01/) (1)
* [October 2016](https://nolanlawson.com/2016/10/) (1)
* [August 2016](https://nolanlawson.com/2016/08/) (1)
* [June 2016](https://nolanlawson.com/2016/06/) (1)
* [April 2016](https://nolanlawson.com/2016/04/) (1)
* [February 2016](https://nolanlawson.com/2016/02/) (2)
* [December 2015](https://nolanlawson.com/2015/12/) (1)
* [October 2015](https://nolanlawson.com/2015/10/) (1)
* [September 2015](https://nolanlawson.com/2015/09/) (1)
* [July 2015](https://nolanlawson.com/2015/07/) (1)
* [June 2015](https://nolanlawson.com/2015/06/) (2)
* [October 2014](https://nolanlawson.com/2014/10/) (1)
* [September 2014](https://nolanlawson.com/2014/09/) (1)
* [April 2014](https://nolanlawson.com/2014/04/) (1)
* [March 2014](https://nolanlawson.com/2014/03/) (1)
* [December 2013](https://nolanlawson.com/2013/12/) (2)
* [November 2013](https://nolanlawson.com/2013/11/) (3)
* [August 2013](https://nolanlawson.com/2013/08/) (1)
* [May 2013](https://nolanlawson.com/2013/05/) (3)
* [January 2013](https://nolanlawson.com/2013/01/) (1)
* [December 2012](https://nolanlawson.com/2012/12/) (1)
* [November 2012](https://nolanlawson.com/2012/11/) (1)
* [October 2012](https://nolanlawson.com/2012/10/) (1)
* [September 2012](https://nolanlawson.com/2012/09/) (3)
* [June 2012](https://nolanlawson.com/2012/06/) (2)
* [March 2012](https://nolanlawson.com/2012/03/) (3)
* [February 2012](https://nolanlawson.com/2012/02/) (1)
* [January 2012](https://nolanlawson.com/2012/01/) (1)
* [November 2011](https://nolanlawson.com/2011/11/) (1)
* [August 2011](https://nolanlawson.com/2011/08/) (1)
* [July 2011](https://nolanlawson.com/2011/07/) (1)
* [June 2011](https://nolanlawson.com/2011/06/) (3)
* [May 2011](https://nolanlawson.com/2011/05/) (2)
* [April 2011](https://nolanlawson.com/2011/04/) (4)
* [March 2011](https://nolanlawson.com/2011/03/) (1)

### Tags

[accessibility](https://nolanlawson.com/tag/accessibility/)
[alogcat](https://nolanlawson.com/tag/alogcat/)
[android](https://nolanlawson.com/tag/android-2/)
[android market](https://nolanlawson.com/tag/android-market/)
[apple](https://nolanlawson.com/tag/apple/)
[app tracker](https://nolanlawson.com/tag/app-tracker/)
[benchmarking](https://nolanlawson.com/tag/benchmarking/)
[blobs](https://nolanlawson.com/tag/blobs/)
[boost](https://nolanlawson.com/tag/boost/)
[bootstrap](https://nolanlawson.com/tag/bootstrap/)
[browsers](https://nolanlawson.com/tag/browsers/)
[bug reports](https://nolanlawson.com/tag/bug-reports/)
[catlog](https://nolanlawson.com/tag/catlog/)
[chord reader](https://nolanlawson.com/tag/chord-reader/)
[code](https://nolanlawson.com/tag/code/)
[contacts](https://nolanlawson.com/tag/contacts/)
[continuous integration](https://nolanlawson.com/tag/continuous-integration/)
[copyright](https://nolanlawson.com/tag/copyright/)
[couch apps](https://nolanlawson.com/tag/couch-apps/)
[couchdb](https://nolanlawson.com/tag/couchdb/)
[couchdroid](https://nolanlawson.com/tag/couchdroid/)
[developers](https://nolanlawson.com/tag/developers/)
[development](https://nolanlawson.com/tag/development/)
[emoji](https://nolanlawson.com/tag/emoji/)
[grails](https://nolanlawson.com/tag/grails/)
[html5](https://nolanlawson.com/tag/html5/)
[indexeddb](https://nolanlawson.com/tag/indexeddb/)
[information retrieval](https://nolanlawson.com/tag/information-retrieval/)
[japanese name converter](https://nolanlawson.com/tag/japanese-name-converter/)
[javascript](https://nolanlawson.com/tag/javascript/)
[jenkins](https://nolanlawson.com/tag/jenkins/)
[keepscore](https://nolanlawson.com/tag/keepscore/)
[listview](https://nolanlawson.com/tag/listview/)
[logcat](https://nolanlawson.com/tag/logcat/)
[logviewer](https://nolanlawson.com/tag/logviewer/)
[lucene](https://nolanlawson.com/tag/lucene/)
[nginx](https://nolanlawson.com/tag/nginx/)
[nlp](https://nolanlawson.com/tag/nlp/)
[node](https://nolanlawson.com/tag/node/)
[nodejs](https://nolanlawson.com/tag/nodejs/)
[npm](https://nolanlawson.com/tag/npm/)
[offline-first](https://nolanlawson.com/tag/offline-first/)
[open source](https://nolanlawson.com/tag/open-source/)
[passwords](https://nolanlawson.com/tag/passwords/)
[performance](https://nolanlawson.com/tag/performance/)
[pinafore](https://nolanlawson.com/tag/pinafore/)
[pokedroid](https://nolanlawson.com/tag/pokedroid/)
[pouchdb](https://nolanlawson.com/tag/pouchdb/)
[pouchdroid](https://nolanlawson.com/tag/pouchdroid/)
[query expansion](https://nolanlawson.com/tag/query-expansion/)
[relatedness calculator](https://nolanlawson.com/tag/relatedness-calculator/)
[relatedness coefficient](https://nolanlawson.com/tag/relatedness-coefficient/)
[s3](https://nolanlawson.com/tag/s3/)
[safari](https://nolanlawson.com/tag/safari/)
[satire](https://nolanlawson.com/tag/satire/)
[sectioned listview](https://nolanlawson.com/tag/sectioned-listview/)
[security](https://nolanlawson.com/tag/security/)
[semver](https://nolanlawson.com/tag/semver/)
[shadow dom](https://nolanlawson.com/tag/shadow-dom/)
[social media](https://nolanlawson.com/tag/social-media/)
[socket.io](https://nolanlawson.com/tag/socket-io/)
[software development](https://nolanlawson.com/tag/software-development/)
[solr](https://nolanlawson.com/tag/solr/)
[spas](https://nolanlawson.com/tag/spas/)
[supersaiyanscrollview](https://nolanlawson.com/tag/supersaiyanscrollview/)
[synonyms](https://nolanlawson.com/tag/synonyms/)
[twitter](https://nolanlawson.com/tag/twitter/)
[ui design](https://nolanlawson.com/tag/ui-design/)
[ultimate crossword](https://nolanlawson.com/tag/ultimate-crossword/)
[w3c](https://nolanlawson.com/tag/w3c/)
[webapp](https://nolanlawson.com/tag/webapp/)
[webapps](https://nolanlawson.com/tag/webapps-2/)
[web platform](https://nolanlawson.com/tag/web-platform/)
[web sockets](https://nolanlawson.com/tag/web-sockets/)
[websql](https://nolanlawson.com/tag/websql/)

### Links

* [Mastodon](https://toot.cafe/%40nolan)
* [GitHub](https://github.com/nolanlawson)
* [npm](https://npmjs.com/~nolanlawson)

[Blog at WordPress.com.](https://wordpress.com/?ref=footer_blog)

* [Comment](https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/#comments)
* Reblog
* Subscribe
  Subscribed

  + [![](https://nolanlawson.com/wp-content/uploads/2025/01/favicon.png?w=32) Read the Tea Leaves](https://nolanlawson.com)

  Join 1,282 other subscribers

  Sign me up

  + Already have a WordPress.com account? [Log in now.](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fnolanlawson.com%252F2025%252F12%252F14%252Fthe-time-element-should-actually-do-something%252F)
* + [![](https://nolanlawson.com/wp-content/uploads/2025/01/favicon.png?w=32) Read the Tea Leaves](https://nolanlawson.com)
  + Subscribe
    Subscribed
  + [Sign up](https://wordpress.com/start/)
  + [Log in](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Fnolanlawson.com%252F2025%252F12%252F14%252Fthe-time-element-should-actually-do-something%252F)
  + [Copy shortlink](https://wp.me/p1t8Ca-3Xg)
  + [Report this content](https://wordpress.com/abuse/?report_url=https://nolanlawson.com/2025/12/14/the-time-element-should-actually-do-something/)
  + [View post in Reader](https://wordpress.com/reader/blogs/21720966/posts/15206)
  + [Manage subscriptions](https://subscribe.wordpress.com/)
  + Collapse this bar

![](https://pixel.wp.com/b.gif?v=noscript)

---
source: hackernews
title: Avoid Mini-Frameworks
url: https://laike9m.com/blog/avoid-mini-frameworks,171/
date: 2025-12-25
---

# [laike9m's blog](/)

* [Archives](/blog/archive)
* [About](/blog/about)
* [Projects](/blog/projects)
* [Podcasts](/blog/podcasts)
* [Talks](/blog/talks)

DEC
24TH, 2025

# **Avoid Mini-frameworks**

* [What is mini-framework?](#what-is-mini-framework)
* [My Story](#my-story)
* [Why mini-frameworks are bad?](#why-mini-frameworks-are-bad)
* [So, What Should You Do Instead?](#so-what-should-you-do-instead)

I work in Google Ads infrastructure in the past four years. Over time, I've seen one pattern came along again and again, causing endless pain for developers, that is, creating mini-frameworks.

## What is mini-framework?

First, I'd like to give readers a sense of what I mean by "mini-frameworks". Imagine a company that has 1000 engineers, and they share the same tech stack. Over time, a team finds the shared tech stack unsatisfactory, either because they have to write boilerplate code over and over, or the performance is not good enough, or whatever. So they decided to create their own framework on top of the shared stack. You know it's a framework and not just a library, when you hear engineers present the work using concepts they invented, as a way to actualize their mental model. Framework like this is what I called "mini-framework", with some common characteristics:

* **Created by a small number of team(s) to solve their pain points**
* **Wraps around the company/org-shared tech stack or framework**
* **Introduce new concepts that doesn't exist in the original stack**
* **Creators claim that the framework "magically" solves many problems, and push more people to use it**

If you work for a big company, most likely you've seen or even done this. But why is it bad? Being a victim myself, I can share my story.

## My Story

Over the years, my team has been using a Google-internal framework to write distributed programs, and are gradually migrating our org's codebase onto it. This framework is well-designed and maintained by a dedicated team. We're generally happy with it, though it has some itchy points (not pain points). Being a strong and ambitious engineer, my manager proposed that we add an abstraction layer on top of it, with the aim to lower the barrier for org adoption, and solve those itchy points on the way. I was skeptical and tried convincing my manager not to do it, but failed. Eventually, several engineers spent quarters to add this abstraction layer, and now the fun begins.

First, one author tried to adopt it in our existing codebase. People thought it would be easy, but it took like a year. Why? Because during the migration, they found that the abstraction cannot handle certain use cases, so they have to spend time patching it to make it work. In the meantime, other people like me were writing new code using the original framework, this again added new requirements and workload for migration. Eventually, after spending way more time than initially planned, the migration completed, and my manager decided that newly-written code should use our own framework.

You think this is the end? Of course not!

Since I started writing code with the new framework, there wasn't a single time that I didn't want to curse and quit the job. The framework is so hard to use, it introduced so many concepts with the intent to hide complexity, but ended up bringing more. Since I'm not the person who writes it, I'm not familiar with all tricks and hacks. As a result, what used to cost me a day to finish can now take two weeks, plus I have to constantly ask the author how to do certain things, and even booked pair-programming sessions. As I heard, other team members were having the same complaints. As for the original goal of driving adoption, ofc it didn't help (if not slower). What's funny is that, this layer is supposed to make people outside of our team write code more easily, but now even our own team are suffering from it.

Looking back, part of the problem was caused by bad design and implementation, but the fundamental problem lies in creating the mini-framework in the first place. I've observed other mini-frameworks, they all sort of have the same problem, just some worse than the others. So I've come up with a few thoughts to explain why mini-frameworks are bad.

## Why mini-frameworks are bad?

First, **mini-frameworks lacks feature completeness and compatibility**. People often think that they can magically "hide the unnecessary complexity", but in reality they can't. Even if the mini-framework can deal with 80% use cases well, they often lack the flexibility and features from the original framework to satisfy the rest 20%. DSL (Domain Specific Language) shares the same problem, and that's why people [hate it so much](https://news.ycombinator.com/item?id=37381409).

Second, **mini-frameworks violates the ETC (Easier To Change) principle**. I first learned the concept from the legendary book *[The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)*, and it still astonishes me that many programmers (even experienced ones) are not aware of it. Put simply, it suggests to write code in a way that allows future modifications be made easily. Creating mini-frameworks violates this principle in two ways:

* The mini-framework only models the current use cases to solve certain problems. When requirements change in the future, it often cannot keep up with it.
* Creating mini-framework often requires poking into the implementation details of the original framework, and those details were abstracted away for good reason: they can change at any time. The more implementation details that are relied upon, the harder for the maintaining team to evolve the original framework.

Third, **mini-frameworks is a realization of the creator's mental model, but it's not everyone's mental model**. People who tend to create mini-framework are often more opinionated, which is a good thing by itself. But when you create things for others to use, too opinionated can be a bad thing. I would even say that sometimes (not always), creating a mini framework directly reflects the author's ego, and they chose a framework because a tiny library don't recognize the "significance of the work".

Fourth, **mini-frameworks tend to cause tech stack fragmentation**. I generated an image to show what I mean: part of the system is migrated, others are not. As new layers kept being added, over time it gets worse. Not sure if other companies are like this, but at Google, I've never actually seen a code migration fully complete, which is kinda hilarious.

![](https://s2.loli.net/2025/12/24/FvjuGLVUtkNB18i.jpg)

Last but also the most concerning: **lack of maintenance**. Unlike shared infrastructure owned by dedicated teams, mini-frameworks were often owned by those 1 or 2 people who created it. Once they left the team or company, it becomes really hard to find successors — sure other team members may know roughly how things work, but certainly not as deep as the authors. Also, people lack the motivation to maintain existing stuff, because you don't get paid more or promoted doing this. Therefore, mini frameworks often die with the departure of the original authors, unless it has gained major adoption before that, which happens less likely than not.

## So, What Should You Do Instead?

At this point, it's important to clarify what I'm against and what I'm not. I'm certainly not against adding abstractions——because abstractions are essentially, the program itself, we can't live without it. **I'm against adding abstractions in a wrong way, and in the form that's not needed**.

Let me bring this up once again since it's really important:
**The real and only difference between a library and a framework, is whether it introduces new concepts**. The line can be blurry sometimes, but more often you can tell easily. For example, a library can include a set of subclasses or utility functions around the original framework, as they don't introduce new concepts. But if you see a README that starts with a "Glossary" section, you know it's 99.99% chance a framework (people may still refer to them as "libraries", but you get the idea).

My point is, **we should be really really careful introducing new concepts. If you can, avoid it**. The cognitive load brought by a bunch of buzz words is heavier than people realized, and especially, what the framework author can realize. Words are the mirror of someone's thought, same with code. The authors create concepts because it is how they model the problem inside their brain, it is how they think. That's why they claim the concepts are "natural and straightforward", while others are struggling to understand.

So rule No.1, **avoid creating mini-frameworks, create libraries instead**. But in the cases where you do find it necessary to create a framework, my suggestions are:

* **Link concepts to concrete business requirements, not something in your head**.
* **Start fresh**. Don't build a wrapper around the existing framework, build your own from scratch. Yes this would make it a major decision that requires more discussions and resources, but it avoids many of the aforementioned issues. After all, you have good business justification to do this, right.. Right?
* **Take it seriously**. Having seen many disasters caused by mini-frameworks, I can't help thinking if it's because people didn't take it seriously enough. My feeling is that, since people don't really understand the distinction between a library and a framework, both the initiator and reviewers of the decision tend to underthink it: "Oh it's just another abstraction, we do that all the time". As my story shows, it's really not. Creating and adopting a framework, whether mini or not, is always a serious decision, thus should be treated that way. By realizing the seriousness of this matter, I hope we can avoid making rashy decisions to create mini-frameworks that should not exist in the first place.

Please enable JavaScript to view the [comments powered by Disqus.](http://disqus.com/?ref_noscript)
[comments powered by Disqus](http://disqus.com)

![laike9m](/static/css3two_blog/images/thumbnail.png)

## Hi, I'm laike9m

Software Engineer at Google

Free Software developer
Founder of
[捕蛇者说](https://pythonhunter.org) podcast
Checkout
my first app:
**[Clicknow: one-click AI search for Mac](https://clicknow.ai)**

[![profile for laike9m on Stack Exchange, a network of free, community-driven Q&A sites](https://stackoverflow.com/users/flair/2142577.png "profile for laike9m on Stack Exchange, a network of free, community-driven Q&A sites")](http://stackoverflow.com/users/2142577/laike9m)
[![Follow laike9m on Twitter](https://honey.badgers.space/badge/@laike9m/Twitter/1DA1F2?icon=eva-twitter)](https://twitter.com/laike9m)
[![Follow laike9m on Mastodon](https://img.shields.io/mastodon/follow/108392635136563026?label=@laike9m Mastodon)](https://mastodon.social/%40laike9m)
[![Follow laike9m on GitHub](https://img.shields.io/github/followers/laike9m?label=@laike9m GitHub)](https://github.com/laike9m)
[![YouTube Channel Subscribers](https://img.shields.io/youtube/channel/subscribers/UCL7TNDbb17hVpybCF2OCWkQ?label=@laike9m YouTube&style=social)](https://www.youtube.com/%40laike9m)

## Blogroll

[Hika AI](https://hika.fyi/)

[Binuxの杂货铺](https://binux.blog)

[依云's Blog](https://blog.lilydjwg.me)

[zhangxiaoyang.me](http://zhangxiaoyang.me)

[Tian Jun](http://tianjun.me)

[Bambooom](http://bambooom.github.io)

[Pluveto](https://pluveto.com)

[卡瓦邦噶！](https://www.kawabangga.com)

[daya0576's Blog](https://changchen.me)

[Chyroc的博客](http://blog.chyroc.cn/)

[woodenrobot](http://woodenrobot.me/)

[Piglei](https://www.piglei.com)

[YongHao Hu](http://yonghaowu.github.io/)

[Manjusaka](https://manjusaka.itscoder.com/)

[Vimiix's blog](https://www.vimiix.com)

[Re: Memory](https://flandre-scarlet.moe/blog/)

[zrt](https://zrt.io/)

[Chaoyi Feng](https://www.usegitflow.com)

[Adam Wen's Blog](https://darkof.me)

[chaomai's blog](https://chaomai.github.io/archives)

[Muniao's blog](https://www.qtmuniao.com)

[小明明s à domicile](https://www.dongwm.com)

[Shidenggui](https://shidenggui.com)

[Frost's Blog](https://frostming.com)

[StoneMoe's Blog](https://stone.moe)

[一只小白](https://wiki.blanc.site)

[某岛](http://www.shuizilong.com)

[注册 DigitalOcean 领 $10 代金券](https://m.do.co/c/a582fa751343)

![top](/static/css3two_blog/images/top.png)

[Home](/) | [Archives](/blog/archive) |
[About](/blog/about) | Projects |

[SourceCode@Github](https://github.com/laike9m/My_Blog) |
[Hosted on DigitalOcean](https://www.digitalocean.com/?refcode=a582fa751343)
|
[design from css3templates.co.uk](http://www.css3templates.co.uk)

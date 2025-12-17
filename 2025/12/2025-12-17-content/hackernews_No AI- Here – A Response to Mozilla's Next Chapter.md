---
source: hackernews
title: No AI* Here – A Response to Mozilla's Next Chapter
url: https://www.waterfox.com/blog/no-ai-here-response-to-mozilla/
date: 2025-12-17
---

[Skip to main content](#main-content)

**✨ Waterfox** has a new website! A fresh lick of paint and migration to waterfox.com

* [Blog](/blog/)
* [Changelog](/releases/)
* [Download](/download/)
* [Support](/support/waterfox-help/)

[###### Waterfox](/)

* [Blog](/blog/)
* [Changelog](/releases/)
* [Download](/download/)
* [Support](/support/waterfox-help/)

Search

 `K`

Cancel

Theme

Dynamic

Light

Dark

[← Back to all posts](/blog/)

December 16, 2025

# No AI\* Here - A Response to Mozilla's Next Chapter

Mozilla's pivot to AI first browsing raises fundamental questions about what a browser should be. Waterfox won't include them. The browser's job is to serve you, not think for you.

![Alex Kontos](/images/blog/alex.jpg)

Alex Kontos

7 mins read

Mozilla’s new CEO recently announced their [vision for the future](https://blog.mozilla.org/en/mozilla/leadership/mozillas-next-chapter-anthony-enzor-demeo-new-ceo/): positioning Mozilla as “the world’s most trusted software company” with AI at its centre. As someone who has spent nearly 15 years building and maintaining Waterfox, I understand the existential pressure Mozilla faces. Their lunch is being eaten by AI browsers. Alphabet themselves reportedly see the writing on the wall, developing what appears to be a new browser separate from Chrome. The threat is real, and I have genuine sympathy for their position.

But I believe Mozilla is making a fundamental mistake.

## The Asterisk Matters

Let’s be clear about what we’re talking about. “AI” has become a catch-all term that to me, obscures more than it reveals. Machine learning technologies like the [Bergamot translation project](https://github.com/browsermt/bergamot-translator) offer real, tangible utility. Bergamot is transparent in what it does (translate text locally, period), auditable (you can inspect the model and its behavior), and has clear, limited scope, even if the internal neural network logic isn’t strictly deterministic.

Large language models are something else entirely˟. They are black boxes. You cannot audit them. You cannot truly understand what they do with your data. You cannot verify their behaviour. And Mozilla wants to put them at the heart of the browser and that doesn’t sit well.

But it’s important to note I do find LLMs have utility, measurably so. But here I am talking in the context of a web browser and the fundamental scepticism I have toward it in that context.

## What Is a Browser For?

A browser is meant to be a user agent, more specifically, *your* agent on the web. It represents you, acts on your behalf, and executes your instructions. It’s called a user agent for a reason.

When you introduce a potential LLM layer between the user and the web, you create something different: “a user agent user agent” of sorts. The AI becomes the new user agent, mediating and interpreting between you and the browser. It reorganises your tabs. It rewrites your history. It makes decisions about what you see and how you see it, based on logic you cannot examine or understand.

Mozilla promises that “AI should always be a choice - something people can easily turn off.” That’s fine. But how do you keep track of what a black box actually does when it’s turned on? How do you audit its behaviour? How do you know it’s not quietly reshaping your browsing experience in ways you haven’t noticed?

Even if you can disable individual AI features, the cognitive load of monitoring an opaque system that’s supposedly working on your behalf would be overwhelming. Now, I truly believe and trust that Mozilla will do what they think is best for the user; but I’m not convinced it will be.

This isn’t paranoia, because after all, “It will evolve into a modern AI browser and support a portfolio of new and trusted software additions.” It’s a reasonable response to fundamentally untrustworthy technology being positioned as the future of web browsing.

## Mozilla’s Dilemma?

I get it. Mozilla is facing an existential crisis. AI browsers are proliferating. The market is shifting. Revenue diversification from search is urgent. Firefox’s market share continues to decline. The pressure to “do something” must be immense, and I understand that.

But there’s a profound irony in their response. Mozilla speaks about trust, transparency, and user agency while simultaneously embracing technology that undermines all three principles. They promise AI will be optional, but that promise acknowledges they’re building AI so deeply into Firefox that an opt-out mechanism becomes necessary in the first place.

Mozilla’s strength has always come from the technical community - developers, power users, privacy advocates. These are the people who understand what browsers should be and what they’re for. Yet Mozilla seems convinced they need to chase the average user, the mainstream market that Chrome already dominates.

That chase has been failing for over a decade. Firefox’s market share has declined steadily as Mozilla added features their core community explicitly didn’t want. Now they’re doubling down on that strategy, going after “average Joe” users while potentially alienating the technical community that has been their foundation.

## What Waterfox Offers Instead

Waterfox exists because some users want a browser that simply works well at being a browser. The UI is mature - arguably, it has been a solved for problem for years. The customisation features are available and apparent. The focus is on performance and web standards.

In many ways, browsers are operating systems of their own, and a browser’s job is to be a good steward of that environment. AI, in its current form and in my opinion does not match that responsibility.

And yes, yes - disabling features is all well and good, but at the end of the day, if these AI features are black boxes, how are we to keep track of what they actually do? The core browsing experience should be one that fully puts the user in control, not one where you’re constantly monitoring an inscrutable system that claims to be helping you.

Waterfox will not include LLMs. Full stop. At least and most definitely not in their current form or for the foreseeable future.

### A Note on other Forks and Governance

The Firefox fork ecosystem includes several projects that tout their independence from Mozilla. Some strip out more features than Waterfox does, some make bolder design choices.

But here’s what often gets overlooked - many of these projects operate without any formal governance structure, privacy policies, or terms of service. There’s no legal entity, no accountability mechanism, no recourse if promises are broken. Open source gives developers the freedom to fork code and make claims, but it doesn’t automatically make those claims trustworthy.

When it comes to something as critical as a web browser - software that mediates your most sensitive online interactions - the existence of a responsible organisation with clear policies becomes crucial. Waterfox maintains formal policies and a legal entity, not because it’s bureaucratic overhead, but because it creates accountability that many browser projects simply don’t have.

You deserve to know who is responsible for the software you rely on daily and how decisions about your privacy are made. The existence of formal policies, even imperfect ones, represents a commitment that your interests matter and that there’s someone to hold accountable.

You may think, so what? And fair enough, I can’t change your mind on that, but Waterfox’s governance has allowed it to do something no other fork has (and likely will not do) - trust from other large, imporant third parties which in turn has given Waterfox users access to protected streaming services via Widevine. It’s a small thing, but to me it showcases the power of said governance.

## On Inevitability

Some will argue that AI browsers are inevitable, that we’re fighting against the tide of history. Perhaps. AI browsers may eat the world.
But the web, despite having core centralised properties, is fundamentally decentralised. There will always be alternatives. If AI browsers dominate and then falter, if users discover they want something simpler and more trustworthy, Waterfox will still be here, marching patiently along.
We’ve been here before. When Firefox abandoned XUL extensions, Waterfox Classic preserved them. When Mozilla started adding telemetry and Pocket and sponsored content, Waterfox stripped it out. When the technical community asked for a browser that simply respected them, Waterfox delivered.

I’ll keep doing that. Not because it’s the most profitable path or because it’s trendy, but because it’s what users who value independence and transparency actually need.

The browser’s job is to serve you, not to think for you. That core Waterfox principle hasn’t changed, and it won’t.

---

\* The asterisk acknowledges that “AI” has become a catch-all term. Machine learning tools like local translation engines (Bergamot) are valuable and transparent. Large language models, in their current black-box form, are neither.

˟ As is my understanding, but please feel free to correct me if that isn’t correct.

On this page

  [The Asterisk Matters](#the-asterisk-matters)  [What Is a Browser For?](#what-is-a-browser-for)  [Mozilla’s Dilemma?](#mozillas-dilemma)  [What Waterfox Offers Instead](#what-waterfox-offers-instead)  [A Note on other Forks and Governance](#a-note-on-other-forks-and-governance)  [On Inevitability](#on-inevitability)

[###### Waterfox](/)

© 2025 Waterfox. All rights reserved.

### Documentation

 [Support](/support/waterfox-help/)  [Privacy Policy](/docs/policies/privacy/)  [Terms of Service](/docs/policies/terms/)

### Community

 [GitHub](https://github.com/BrowserWorks/Waterfox)  [Support](https://www.reddit.com/r/waterfox)

A website template crafted with love by [Cosmic Themes](https://cosmicthemes.com/)

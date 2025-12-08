---
source: hackernews
title: The programmers who live in Flatland
url: https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/
date: 2025-12-08
---

[Skip to content](#content)

[![](https://i0.wp.com/blog.redplanetlabs.com/wp-content/uploads/2023/04/cropped-cropped-RPL-Logo.png?fit=1808%2C258&ssl=1)](https://redplanetlabs.com)

[Blog](https://blog.redplanetlabs.com/)

Menu

* [Home](/)
* [Open source](https://github.com/redplanetlabs)
* [About](https://redplanetlabs.com/about)
* [Archive](https://blog.redplanetlabs.com/archive)

# The programmers who live in Flatland

[November 24, 2025](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/) ~ [Nathan Marz](https://blog.redplanetlabs.com/author/nathanmarzrpl/)

![](https://m.media-amazon.com/images/I/71AL76zvS+L._SY522_.jpg)

In the book [Flatland: A Romance of Many Dimensions](https://www.amazon.com/Flatland-Romance-Dimensions-Edwin-Abbott/dp/B0875SRH84), a two-dimensional world called “Flatland” is inhabited by polygonal creatures like triangles, squares, and circles. The protaganist, a square, is visited by a sphere from the third dimension. He struggles to comprehend the existence of another dimension even as the sphere demonstrates impossible things. It’s a great book that has stuck with me since I first read it almost 30 years ago.

I’ve realized that “Flatland” is a perfect metaphor for the state of mind of a large number of programmers. Consider this: in 2001 Paul Graham, one of the most influential voices in tech, wrote the essay [Beating the Averages](https://paulgraham.com/avg.html). He argues forcefully about Lisp being fundamentally more powerful than other languages and credits Lisp as the key reason why his startup Viaweb outlasted their competitors. He identifies macros as the particularly distinguishing capability of Lisp. He writes:

> A big chunk of our code was doing things that are very hard to do in other languages. The resulting software did things our competitors’ software couldn’t do. Maybe there was some kind of connection. I encourage you to follow that thread.

I did follow that thread, and that essay is a key reason why [Clojure](https://clojure.org/) has been my primary programming language for the past 15 years. What Paul Graham described about the power of macros was absolutely true. Yet evidently very few shared my curiosity and Lisp/Clojure are used by a [tiny percentage](https://survey.stackoverflow.co/2024/technology#most-popular-technologies-language-prof) of programmers worldwide. How can this be?

Many point to “ecosystems” as the barrier, an argument that’s valid for Common Lisp but not for Clojure, which interops easily with one of the largest ecosystems in existence. So many misperceptions dominate, especially the reflexive reaction that the parentheses are “weird”. Most importantly, you almost never see these perceived costs weighed against Clojure’s huge benefits. Macros are the focus of this post, but Clojure’s approach to state and identity is also transformative. The scale of the advantages of Clojure dwarfs the scale of adoption.

In that essay Paul Graham introduced the “blub paradox” as an explanation for this disconnect. It’s a great metaphor I’ve referenced many times over the years. This post is my take on explaining this disconnect from another angle that complements the blub paradox.

I recognize there’s a fifty year history of Lisp programmers trying to communicate its power with limited success. So I don’t think I’m going to change any minds. Yet I feel compelled to write this since the metaphor of “Flatland” just fits too well.

## Dimensions of programming

Programming revolves around abstractions, high-level ways of thinking about code far removed from the base primitives of bits, machine instructions, and memory hierarchies. But not all abstractions are created equal. Most abstractions programmers use are *automations*, a package that bundles a set of operations behind a name. A function is the canonical example: it takes inputs and produces an output. You don’t need to know how the function works and can think just in terms of the function’s spec and performance characteristics.

Then there are the rare abstractions which extend the algebra of programming itself: the basic concepts available and the kinds of relationships that can exist between them. These are the abstractions that create new dimensions.

Lisp/Clojure macros derive from the uniformity of the language to enable composing the language back on itself. Logic can be run at compile-time no differently than at runtime using all the same functions and techniques. The syntax tree of the language can be manipulated and transformed at will, enabling control over the semantics of code itself. The ability to manipulate compile-time so effortlessly is a new dimension of programming. This new dimension enables you to write fundamentally better code that you’ll never be able to achieve in a lower dimension.

If you’re already a Lisp programmer, you already understand the power of macros and how to avoid the potential pitfalls. My description is boring because you’ve done it a thousand times. But if you’re not a Lisp programmer, what I described probably sounds crazy and ill-advised!

In Flatland, the square cannot comprehend the third dimension because he can only think in 2D. Likewise, you cannot comprehend a new programming dimension because you don’t know how to think in that dimension. You do not have the representational machinery to understand what the new dimension is even offering. A programmer in 2D may conclude the 3D concept is objectively wrong. This isn’t because they’ve understood it, but because they’re trying to flatten it into their existing coordinate system.

## Learning new dimensions

You can’t persuade someone in 2D with 3D arguments. This is exactly like how in Flatland the sphere is unable to get the square to comprehend what “up” and “down” mean.

However, this is where the metaphor breaks down. Though your brain will never be able to comprehend 4D space, your brain can adapt to new dimensions of programming. People who adopt Lisp/Clojure typically describe the experience similarly. First it’s uncomfortable, then there’s a series of moments of clarity, and then there’s a feeling they can never go back.

All it takes is curiosity and an understanding that the greatest programming ideas sometimes can’t be appreciated at first. That latter point is the key insight that gets lost in the conversation. We all have that cognitive bias. Recognizing that bias is enough to break it, and it’s one of the best ways to grow as a programmer. Macros are not the only great programming idea with this dimension-shifting quality.

## Conclusion

In the end, living in Flatland is a choice. The choice appears the moment you notice that instinctive recoil from an unfamiliar idea, when you feel that tension between “this doesn’t make sense” and “maybe I don’t yet have the concepts to make sense of it.” What you do in that moment defines whether you stay in Flatland or step out of it.

## Post navigation

[‹ PreviousIntroducing Agent-o-rama: build, trace, evaluate, and monitor stateful LLM agents in Java or Clojure](https://blog.redplanetlabs.com/2025/11/03/introducing-agent-o-rama-build-trace-evaluate-and-monitor-stateful-llm-agents-in-java-or-clojure/)

[Next ›Rama in five minutes (Clojure version)](https://blog.redplanetlabs.com/2025/12/02/rama-in-five-minutes-clojure-version/)

## 3 thoughts on “The programmers who live in Flatland”

1. ![](https://secure.gravatar.com/avatar/09ac25e628c9b1f6d442aa49d964b86dfb34a26c4f52e6a036c1264c22e52709?s=60&d=mm&r=g) **[Valerie Kim](https://valeriekim.ca)** says:

   [November 26, 2025 at 11:17 am](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/#comment-1034)

   > Most abstractions programmers use are automations, a package that bundles a set of operations behind a name…
   > Then there are the rare abstractions which extend the algebra of programming itself: the basic concepts available and the kinds of relationships that can exist between them. These are the abstractions that create new dimensions.

   Been following Rama for the last couple years, finally learning it over the last couple weeks.

   What really stood out in this blog post to me was this notion of the “rare abstraction”. It reminded me of a significant finding from a mathematical-logic book called Laws of Form (1969), which focuses on the the /most simple/ formal system possible, with only one symbol, two rules, and ~one value (maybe two): {`marked`, `unmarked` (aka nothing)} . Even with that system, it is able to be interpreted as boolean algebra and as propositional logic.

   While Chapters 0-10 of LoF explore the proto-arithmetic and proto-algebra of this system (aka, the operations attached to a name), Chapter 11 introduces a new kind of abstraction — a self-referential extension called “re-entry”.
   `F = ((F a)b)`.

   This introduces the possibility of an entirely new dimension to the formal system. It forces disambiguation across time (oscillation, ala `T\_0: marked, T\_1: unmarked, T\_2: marked…`) or space (changing evaluation values from unary in the space `{marked, unmarked}` to a binary vector in the space `{[mark, mark], [mark, unmarked], [unmarked, mark], [unmarked, unmarked]}`). This is similar in power to the introduction of the imaginary number `i` as the solution to `x = -1/x`.

   Macros are inherently self-referential, and they offer so many powerful ways to introduce /new distinctions/ (aka new concepts). The difficulty is understanding what concepts are tenable, but the power is, ofc, unmatched. Figured you’d appreciate an example of its power from even an extremely simple system.

   [Reply](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/?replytocom=1034#respond)
2. ![](https://secure.gravatar.com/avatar/eca38d32431ad0a7133f2b4ce2b2855090ca51c33a905a49753365b2aec656e3?s=60&d=mm&r=g) **ao** says:

   [December 7, 2025 at 3:32 pm](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/#comment-1045)

   Most of the real world programs must be modified over their lifetimes, sometimes often. So it matters a lot how easily and moreover how cost effective is to change them.

   One of the parameters is how fast can other developer than the original author make the changes?

   I believe that smart developers can solve complex problems much quicker with the power of the macros. But the macros are like magic: hard to quickly understand.

   This is why languages like Blub find large niches: Blub may require spelling out more details explicitly and being more verbose, but when your original smart developer is busy coding some cool new program, you can more quickly find and deploy another developer to the first program, who will more quickly do the changes that program users need to have done.

   It’s not that programmers in flatland don’t understand third dimension. It’s that third dimension is takes a lot of time and users are not always willing to pay. It’s also why most of the people commute in road vehicles and not air planes.

   LISP worked for Viaweb during intial phase of competition. But according to internet sources, Yahoo moved the Viaweb from LISP to mainstream stack for the long term maintenance.

   This is why LISP an Clojure are used by relatively small communities, because they shine in specific opportunities.

   [Reply](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/?replytocom=1045#respond)
3. ![](https://secure.gravatar.com/avatar/87b1a6b92745dcbf37d9cc53618cf5344a555ec27b016ebf0a34506115a8ae37?s=60&d=mm&r=g) **ZZ** says:

   [December 7, 2025 at 3:47 pm](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/#comment-1046)

   “Though your brain will never be able to comprehend 4D space”? Erm … many brains actually have been able to comprehend it.

   [Reply](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/?replytocom=1046#respond)

### Leave a Reply [Cancel reply](/2025/11/24/the-programmers-who-live-in-flatland/#respond)

Your email address will not be published. Required fields are marked \*

Comment \*

Name \*

Email \*

Website

[ ]  Save my name, email, and website in this browser for the next time I comment.

[ ]  Notify me of follow-up comments by email.

[ ]  Notify me of new posts by email.

Δ

# Follow Us

* [X](https://twitter.com/redplanetlabs)
* [GitHub](https://github.com/redplanetlabs)

# RSS

[![RSS feed](https://blog.redplanetlabs.com/wp-content/plugins/jetpack/images/rss/red-medium.png)](https://blog.redplanetlabs.com/feed/ "Subscribe to posts")

# Featured

* [How we reduced the cost of building Twitter at Twitter-scale by 100x](https://blog.redplanetlabs.com/2023/08/15/how-we-reduced-the-cost-of-building-twitter-at-twitter-scale-by-100x/)
* [Introducing Agent-o-rama: build, trace, evaluate, and monitor stateful LLM agents in Java or Clojure](https://blog.redplanetlabs.com/2025/11/03/introducing-agent-o-rama-build-trace-evaluate-and-monitor-stateful-llm-agents-in-java-or-clojure/)
* [Migrating terabytes of data instantly (can your ALTER TABLE do this?)](https://blog.redplanetlabs.com/2024/09/30/migrating-terabytes-of-data-instantly-can-your-alter-table-do-this/)
* [Next-level backends with Rama series](https://blog.redplanetlabs.com/next-level-backends-with-rama/)

# Recent Posts

* [Rama in five minutes](https://blog.redplanetlabs.com/2025/12/02/rama-in-five-minutes/)
* [Rama in five minutes (Clojure version)](https://blog.redplanetlabs.com/2025/12/02/rama-in-five-minutes-clojure-version/)
* [The programmers who live in Flatland](https://blog.redplanetlabs.com/2025/11/24/the-programmers-who-live-in-flatland/)
* [Introducing Agent-o-rama: build, trace, evaluate, and monitor stateful LLM agents in Java or Clojure](https://blog.redplanetlabs.com/2025/11/03/introducing-agent-o-rama-build-trace-evaluate-and-monitor-stateful-llm-agents-in-java-or-clojure/)

**Contents**

[1.
Dimensions of programming](#Dimensions_of_programming)

[2.
Learning new dimensions](#Learning_new_dimensions)

[3.
Conclusion](#Conclusion)

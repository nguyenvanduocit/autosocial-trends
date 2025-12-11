---
source: hackernews
title: When would you ever want bubblesort? (2023)
url: https://buttondown.com/hillelwayne/archive/when-would-you-ever-want-bubblesort/
date: 2025-12-11
---

# [Computer Things](https://buttondown.com/hillelwayne)

[Archives](/hillelwayne/archive/)

Search

[Subscribe](https://buttondown.com/hillelwayne#subscribe-form)

December 4, 2023

# When would you ever want bubblesort?

There are very few universal rules in software engineering, but there are are a lot of *near*-universal principles. Things like "prefer composition to inheritance" is near-universal. I love finding the rare situations where these principles don't hold, like where you do want [inheritance over composition](https://buttondown.email/hillelwayne/archive/when-to-prefer-inheritance-to-composition/). A similar near-universal principle is "don't use [bubblesort](https://en.wikipedia.org/wiki/Bubble_sort)". Some would even say it's a universal rule, with Donald Knuth writing "bubble sort seems to have nothing to recommend it, except a catchy name and the fact that it leads to some interesting theoretical problems".[1](#fn:cite) But Knuth's [been wrong before](https://en.wikipedia.org/wiki/Knuth_reward_check), so let's see if this universal rule is only *near*-universal.

Theoretically, bubblesort is faster than quick or mergesort for small arrays. This makes it useful as part of a larger sorting strategy: most of the fast-in-principle sorting algorithms work by recursively sorting subpartitions of an array, ie if you apply quicksort to 2^20 random integers, at some point you're sorting 2^17 8-integer subpartitions. Switching over to bubblesort for those subpartitions would be a nice optimization.

Many production sorting algorithms do use a hybrid approach, but they overwhelmingly use [insertion sort](https://en.wikipedia.org/wiki/Insertion_sort) instead. Insertion sort is very fast for small arrays and it's also [better at using the hardware](https://nicknash.me/2012/10/12/knuths-wisdom/). On some very particular hardwares bubblesort stills ends up better, like in this [NVIDIA study](http://www.sci.utah.edu/~wald/Publications/2019/rtgems/ParticleSplatting.pdf), but you probably don't have that hardware.

So that's one use-case, albeit one still dominated by a different algorithm. It's interesting that NVIDIA used it here because gamedev has a situation that's uniquely useful to bubblesort, based on two of its properties:

1. While the algorithm is very slow overall, each individual step is very fast and easily suspendable.
2. Each swap leaves the array more ordered than it was before. Other sorts can move values *away* from their final positions in intermediate stages.

This makes it really good when you want to do a fixed amount of sorting work per frame. Say you have a bunch of objects on a screen, where some objects can occlude others. You want to render the objects closest to the camera *first* because then you can determine which objects it hides, and then save time rendering those objects. There's no correctness cost for rendering objects out of order, just a potential performance cost. So while your array doesn't *need* to be ordered, the more ordered it is the happier you are. But you also can't spend too much time running a sorting algorithm, because you have a pretty strict realtime constraint. Bubble sort [works pretty well here](https://discussions.unity.com/t/depth-sorting-of-billboard-particles-how-can-i-do-it/5053). You can run it a little bit of a time at each frame and get a better ordering than when you started.

That reminds me of one last use-case I've heard, apocryphally. Let's say you have a random collection of randomly-colored particles, and you want to animate them sorting into a rainbow spectrum. If you make each frame of the animation one pass of bubblesort, the particles will all move smoothly into the right positions. I couldn't find any examples in the wild, so with the help of GPT4 I hammered out a crappy visualization. Code is [here](https://gist.github.com/hwayne/85488f755066d8aa57cd147875e97b72), put it [here](https://editor.p5js.org/).

(After doing that I suspect this isn't actually done in practice, in favor of running a better sort to calculate each particles final displacement and then animating each particles moving directly, instead of waiting to move for each bubblesort pass. I haven't mocked out an example but I think that'd look a lot smoother.)

So there you go, three niche use cases for bubblesort. You'll probably never need it.

---

### New Quanta Article!

Okay so I didn't actually write this one, but I played a role in it happening! A while back a friend visited, and we were chatting about his job at quanta. At the time he was working on this [mammoth article on metacomplexity theory](https://www.quantamagazine.org/complexity-theorys-50-year-journey-to-the-limits-of-knowledge-20230817/), so naturally the topic of [problems harder than NP-complete](https://buttondown.email/hillelwayne/archive/problems-harder-than-np-complete/) came up and I recommend he check out Petri net reachability. So he did, and then he wrote [An Easy-Sounding Problem Yields Numbers Too Big for Our Universe](https://www.quantamagazine.org/an-easy-sounding-problem-yields-numbers-too-big-for-our-universe-20231204/). Gosh this is so exciting!

---

1. Taken from [here](https://users.cs.duke.edu/~ola/papers/bubble.pdf). [↩](#fnref:cite "Jump back to footnote 1 in the text")

*If you're reading this on the web, you can subscribe [here](/hillelwayne). Updates are once a week. My main website is [here](https://www.hillelwayne.com).*

*My new book,* Logic for Programmers*, is now in early access! Get it [here](https://leanpub.com/logic/).*

Don't miss what's next. Subscribe to Computer Things:

Subscribe

#### Add a comment:

Comment and Subscribe

Share this email:

[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A//buttondown.com/hillelwayne/archive/when-would-you-ever-want-bubblesort/&title=When%20would%20you%20ever%20want%20bubblesort%3F)
[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A//buttondown.com/hillelwayne/archive/when-would-you-ever-want-bubblesort/)
[Share on Hacker News](https://share.bingo/hn?url=https%3A//buttondown.com/hillelwayne/archive/when-would-you-ever-want-bubblesort/)
[Share on Bluesky](https://share.bingo/bluesky?url=https%3A//buttondown.com/hillelwayne/archive/when-would-you-ever-want-bubblesort/)

Powered by [Buttondown](https://buttondown.com/refer/hillelwayne), the easiest way to start and grow your newsletter.

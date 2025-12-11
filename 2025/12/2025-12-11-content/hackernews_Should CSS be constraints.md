---
source: hackernews
title: Should CSS be constraints?
url: https://pavpanchekha.com/blog/why-css-bad.html
date: 2025-12-11
---

![Pavel Panchekha](/im/me-square.jpg)

## By [Pavel Panchekha](https://pavpanchekha.com)

### 03 December 2025

* [Blog](/blog.html)
* [WBE](https://browser.engineering/)
* [Herbie](https://herbie.uwplse.org/)
* [Github](https://github.com/pavpanchekha)
* [Papers/CV](/resume.html)

Share under [CC-BY-SA](http://creativecommons.org/licenses/by-sa/4.0/).

# Should CSS be Constraints?

CSS is hard. The layout rules are quite complex. Most people don't know the details. Centering a `<div>` is famously a challenge. Remember the 2000s, when [A List Apart](https://alistapart.com/) would run all sorts of crazy ways to achieve the "Holy Grail" layout—a header, a main body and sidebar of equal heights, and a footer? Should CSS layout—given that it's such a mess—just be thrown out, to start over with a totally different system?

I do feel like I can speak on this some authority. In grad school I wrote the first [formal specification](https://github.com/uwplse/cassius) of CSS 2.1 layout, passing (the relevant fragment of) the conformance test. So I know CSS layout in quite a lot of detail, though of course I'm going beyond that expertise when I talk about what designers do or about other systems.

## What's wrong with CSS?

I think the specific issue we're talking with CSS layout about is *predictability*. In current CSS, it's hard to know what CSS to write to achieve a given layout. That is, what CSS should you write to vertically center an element? (Why `table-cell`?) And also, given some CSS, it's not always easy to know what layout it'll result in. (What other effects will `table-cell` have?)

Now, when we're talking about predictability, it's important to note that CSS layout is *deterministic*. Render the same page twice, even in two different browsers, and you'll get the same result. So in some sense predictability should be great! But, in reality, the rules are really, really complicated.

Here, let me give you an example. Here's [a line from](https://github.com/uwplse/Cassius/blob/master/src/spec/layout.rkt#L1039) my formal semantics of CSS 2.2, the specification of `text-align: center`:

```
[(is-text-align/center textalign)
 (= (- (left-outer f) (left-content b)) (- (right-content b) (right-outer l)))]
```

This is saying that if a container `b` has `text-align: center` specified, then its left gap (the *x* position of left outer edge of its first child `f`, minus the left content edge of the container `b`) equals its right gap (similar, using right edges, the last child `l`, and the subtraction being reversed). It's a constraint! But then if you go [a few lines up](https://github.com/uwplse/Cassius/blob/master/src/spec/layout.rkt#L1032) you'll see that before actually applying this constraint, we first check if the container is even big enough to contain the content, and if not, we left align it no matter what. What? Really? Yep. It's a tricky little quirk of the CSS semantics, [section 9.4.2](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#inline-formatting) of CSS 2.1.[1](#fn.1) [1 Actually, I think the controlling standard on this exact quirk is now [CSS Text Level 3](https://www.w3.org/TR/css-text-3/#text-align-property) which has a quite clear paragraph documenting this behavior.]

Even though this behavior is fully specified and totally deterministic, it might be surprising. That's the kind of predictability that CSS is lacking.

There's another point to draw from this small example: CSS layout is *parametric*, meaning the layout depends on a lot of different parameters like screen size, zoom level, details of font rendering (Windows and macOS render identical fonts slightly differently), operating system details, and perhaps higher-level changes in our application like new content, new features, translations to other languages,[2](#fn.2) [2 German words are very long, Chinese ones are very short.] device oddities like notches, and so on.

This is another reason why determinism doesn't lead to predictability: just because you know how a web page looks at *one* set of parameters doesn't mean you can predict how it would look with *another* set of parameters. Is it possible for these two buttons to overlap? Or this text to spill out of its container? What about on another OS, or zoom level, or screen size? That's hard to know, mostly because of all of those edge cases.

## Constraints

One commonly-proposed replacement for CSS is a constraint system. In a constraint system you directly say:

```
(obj.top + obj.bottom) / 2 ==  (obj.parent.top + obj.parent.bottom) / 2
```

This line *constrains* the vertical midpoint of `obj` to be the vertical midpoint of its parent; in other words, vertically centers `obj`.

So the web page author writes these *constraints* and then the browser runs a *constraint solver* which computes positions and sizes for each object that satisfy the constraints. This is much like normal CSS, where the web page author writes *rules* and then the browser runs a *layout algorithm* that computes sizes and positions.[3](#fn.3) [3 Naturally in both cases you actually compute a lot more than just sizes and positions: fonts, colors, and so on. But layout is taken to be the "hard part" of the problem, and I don't really disagree with that.]

The well-known [Constraint cascading style sheets](https://dl.acm.org/doi/10.1145/320719.322588) paper proposed this idea for web page layout, and its authors (especially [Alan Borning](https://en.wikipedia.org/wiki/Alan_H._Borning)) have a long history with constraint solvers. They built the [Cassowary](https://constraints.cs.washington.edu/cassowary/) *incremental* constraint solver, where "incremental" means it can not only solve the constraints quickly but also re-solve them extra-quickly if the page changes a small amount, like in response to JavaScript or user action. Real browsers [do that too](https://browser.engineering/invalidation.html). And while this never got adopted for web page layout, iOS offers constraint-based using a re-implementation of Cassowary, and I hear it's quite popular. Android too, apparently.

## What's wrong with constraints

So let's evaluate the idea of a constraint system *predictability*. On paper, it seems great: anything you want to be true, you write a constraints, and then your constraints are always satisfied.

The reality is a bit harsher. With constraint-based systems, the layout might be literally *under*- or *over*-determined, in the sense that there might be more than one, or less than one, layouts that satisfy your rules.

For example, the vertical centering constraint above doesn't say that the outer box (`obj.parent`) should be the minimum possible size to contain its child (`obj`), or that they should both be onscreen, or whatever. Whatever you don't specify isn't predictable, which means you have to write lots and lots of constraints. But if you have lots and lots of constraints, it might be impossible to satisfy all of them, in which case the browser has to ignore some; that again makes predictability not so great.

There's a bunch of things you could do at this point. For under-determined layouts, you could add "implicit rules", like saying that boxes should always be onscreen. Or "optimization criteria", like saying that if multiple layouts are available you should pick the one that takes up the least space. And for over-determined layouts, you could do the reverse, like assigning "weights" to each constraint and then optimizing for breaking the fewest. This isn't a crazy way to build a system—it's basically how LaTeX works—but fiddling with the weights, the exact form of implicit rules, the optimization criteria becomes, in practice, the actual determiner of how layouts look.

This might all be acceptable if all the constraints involved were very simple, or if the optimization criteria or conflict handling rules were. But experience suggests that all the *simple* options have bizarre, horrible edge cases, so if you want things to look good you'll need really complicated ones. Once you have complicated constraints and conflict handling rules, predictability is again suffers.

And recall that everything in parameterized. Just testing your constraints on one web page at one screen size doesn't work; a set of constraints that is well-determined at one screen size might be over-determined or under-determined at another. The conflict resolution policy might ignore one constraint on one OS and another constraint on another OS, for basically all the same reasons as current CSS.

And, in fact, constraint systems have a reputation for this. LaTeX is *not* widely beloved for its predictable layout. Constraint layout *is*, I believe, popular on iOS, but it's notable that iOS quite famously allows only a small number of screen / window shapes. And also people still complain about the constraint-based layout being fussy, brittle, and unpredictable, with debugging (especially debugging under- and over-constrained layouts) considered tedious and annoying. And, as mentioned above, it's verbose: you have to write lots and lots of constraints, often with various special cases, and this is tedious too. It would be nice to package constraints into libraries and import them as modules, but constraint-based systems, especially once you start adding implicit rules or optimization criteria, are quite explicitly *not* modular, so this doesn't work.

I applaud Apple for shipping and it's great people are using constraint-based layout. I use LaTeX![4](#fn.4) [4 Newer students seem really excited about [Typst](https://typst.app/), which I have not tried.] But I do think designers in constraint-based systems suffer exactly the kinds of problems theory would predict.

## Implicit design knowledge

How come rule-based systems like CSS and constraint-based systems both suffer from similar issues? Is there some third system that doesn't? What makes this all so hard?

I think the issue is that layout actually is hard. Consider the CSS rule above, about centered text actually becoming left-aligned if it is too wide to fit. Why is it there? Are browser developers just big dummies?

Surely not. If text spills off the left edge of the screen, no user could scroll to it, and left-aligning that text makes it only spills out on the right, where you could scroll to see it. You may have never noticed this quirk of the `text-align` rules but you've surely been its beneficiary. This quirk may make CSS more complex to designers, but I'd guess it makes the web better for users, which is the whole point.

You could of course achieve the same effect with constraints—maybe an implicit rule that all text must be on screen—but that too would add complexity. Or you could opt for a simpler system where web pages would look worse in various edge cases. That simpler system might be better for programmers and browser developers, but it would come at a cost for ordinary users.

Even if you don't like the trade-off chosen, I think it's worth respecting it.

The CSS designers built this edge case into `text-align` to embed hard-earned design wisdom into intuitive rules that people mostly use without issue. This didn't come at too much cost to developers---`text-align` is not considered one of the bad scary parts of CSS—and it probably benefited users.

Generalizing a bit, when we're designing a layout, we want it to look good in *any possible one* of the parameters that affect layout. This is basically impossible—it's hard enough to do both desktop and mobile!—and designers, by necessity, rely on common idioms and implicit knowledge about edge cases. Whether that knowledge is encoded in rules or weights or optimization criteria, it's going to be opaque to designers and surprising at least sometimes, but a simpler system that handled fewer edge cases would be worse.

## Why is design so hard?

Why is design so hard, then? Should designers just try harder? I don't think that's right, either.

For example, in CSS you can also `justify` text, stretching spaces between words so that all lines in a paragraph (except the last!) have the same right edge. But, famously, if the line width is too narrow and the line contains too few words that are too long, then the spaces between them get stretched comically far apart and it looks terrible. You can do better by enabling hyphenation (which might turn 2 really long words in a line into 3 or more moderate size word chunks) or letter-spacing (which might also stretch the spaces between letters slightly) but those are themselves unpredictable and language-dependent and still sometimes look ugly.

Why is justification so hard? Well, text justification comes from a [long Western tradition](https://finaltype.de/en/topics/gutenbergs-justification).[5](#fn.5) [5 That post claims Trajan's column, built 113 AD, as an early example.] What we're doing on a computer is trying to emulate this long tradition, to encode it into algorithms. But that tradition grew up in a totally different environment. In the olden days, if you were (say) a newspaper columnist and your column looked ugly when justified[6](#fn.6) [6 Most newspapers justified their text and also ran it in lots of narrow columns, so it was especially a problem with newspapers.] then your editor would rephrase your writing to use smaller words that justified better. That's not really something CSS can do.[7](#fn.7) [7 Maybe that technology is [slowly becoming possible](https://tom7.org/bovex/).]

So design problems often come from emulating design traditions where those problems were impossible. It's no surprise, then, that there's no good solution to those problems. We're stuck with edge cases, and then it's just a question of where those edge cases live—in the designer's head, or in the rules of CSS, or the constraint solver.

## So what can we do instead?

I think what we *can* do, though, is to improve the situation by providing more-intuitive rule systems with more-predictable edge cases. For example, when designing CSS layouts, you can use negative margins and floats and `clear: both` like we did in the A List Apart days. Or you can use `flex-box` and `grid`. Both are workable but you can guess which of these I teach to students!

`flex-box` and `grid` are rule-based, just like the rest of CSS. The rules are quite complex. (How do `gap`, `flex-shrink`, and `justify-content` interact? How are nested `flex-box` elements laid out?) But the design they aim to emulate, a kind of resizable grid, is closer to what designers want than the old text-layout-centric model of flow layout. That means the edge cases are less surprising and the behavior more predictable.

To analogize a bit… JavaScript has all sorts of bizarre mis-behaviors and semantic oddities like `for ... in` loops or the `with` statement or the totally insane [semantics of eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval).[8](#fn.8) [8 Look for "Direct and indirect eval"; `eval(<expr>)` is a special syntactic form separate from function application but there's also a function named `eval` that can be applied with normal function application, and they behave differently.] One solution to this problem is to forsake JavaScript forever, to use Lisp or Haskell or Rust or something. But another is, like, Typescript and ESLint, with which you can avoid all the bad features of JavaScript. And what the JavaScript developers really need to do is not burn it down but to provide nicer APIs (like `Map`, the triple equals operator, and so on) that behave more like what developers intend.

In CSS specifically, a lot of the problems in CSS layout are problems more precisely in "Flow layout", the default layout mode which is optimized around layout out text in a standard Western style. What's fine for text isn't good for applications, and doing UI design using text formatting tools like floats and clearance was never going to be simple and intuitive. By contrast, newer layout modes like Flex-box and Grid are maybe a bit more complex off the bat, but once you grasp the mental model are quite intuitive with far fewer sharp edges.

So I think in fact the solution is what the CSS committee *has* been doing, standardizing new and more intuitive layout modes optimized for the specific types of layouts being poorly served by what currently exists. With just a bit of effort, those new layout modes can be intuitive, with few edge cases, while still building in a lot of implicit knowledge around, for example, how to respond to container size changes or handle content that is too big or too small.

*Thanks* to my PhD student [Andrew Riachi](https://andrewriachi.com/), whose work on his own website inspired this blog post.

## Footnotes:

[1](#fnr.1)

Actually, I think the controlling standard on this exact quirk is now [CSS Text Level 3](https://www.w3.org/TR/css-text-3/#text-align-property) which has a quite clear paragraph documenting this behavior.

[2](#fnr.2)

German words are very long, Chinese ones are very short.

[3](#fnr.3)

Naturally in both cases you actually compute a lot more than just sizes and positions: fonts, colors, and so on. But layout is taken to be the "hard part" of the problem, and I don't really disagree with that.

[4](#fnr.4)

Newer students seem really excited about [Typst](https://typst.app/), which I have not tried.

[5](#fnr.5)

That post claims Trajan's column, built 113 AD, as an early example.

[6](#fnr.6)

Most newspapers justified their text and also ran it in lots of narrow columns, so it was especially a problem with newspapers.

[7](#fnr.7)

Maybe that technology is [slowly becoming possible](https://tom7.org/bovex/).

[8](#fnr.8)

Look for "Direct and indirect eval"; `eval(<expr>)` is a special syntactic form separate from function application but there's also a function named `eval` that can be applied with normal function application, and they behave differently.

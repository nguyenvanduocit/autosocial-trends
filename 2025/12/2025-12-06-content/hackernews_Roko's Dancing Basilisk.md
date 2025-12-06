---
source: hackernews
title: Roko's Dancing Basilisk
url: https://boston.conman.org/2025/12/02.1
date: 2025-12-06
---

# The Boston Diaries

The ongoing saga of a programmer who doesn't live in Boston, nor does he even like Boston, but yet named his weblog/journal “The Boston Diaries.”

Go figure.

## [Tuesday, Debtember 02, 2025](/2025/12/02)

### [Roko's dancing basilisk](/2025/12/02.1)

#### Obligatory Sidebar Links

* [“I told you three times not to use K&R style braces! Get with the program, Copilot!”](/2024/12/19.1)
* [Avoiding Roko's basilisk, part II](/2025/06/05.1)
* [“Thanks for calling that out. That was bad advice on my part.”](/2025/11/18.2)

I came across a reference to [DeepWiki](https://deepwiki.com/),
a site that will generate “documentation” for any [Github repository](https://github.com/).
I can't say I've been impressed with LLMs generating code,
but what about documentation?
I haven't tried that yet.
Let's see how well Roko's basilisk dances!

Intially,
I started with `[mod_blog](https://github.com/spc476)`.
I've been working with the codebase now for 26 years so it should be easy for me to spot inaccuracies in the “documentation.”
Even better—there's no interaction with a sycophantic chat bot;
just plop in the URL for the repo,
supply an email for notification when it's done and as the Brits say,
“Bob's your uncle!”

Anyway,
email came.
I checked,
and I was quickly amazed!
Nearly 30 pages of documentation,
and the [overview](https://deepwiki.com/spc476/mod_blog) was impressive.
It picked up on [tumblers](/about/technical.html),
the storage layout,
the typical flows in adding a new entry.
It even got the fact that `cmd_cgi_get_today()` returns all the entries for a given day of the month throughout the years.
But there was one bit that was just a tad bit off.
It stated “[t]he system consists of three primary layers” but the following diagram showed five layers,
which no indication of what three were the “primary layers.”
I didn't have a problem with the layers it did identify:

* Entry Layer
* Processing Layer
* Rendering Layer
* Storage Layer
* Configuration

Just that it seems to have a problem [counting to three](https://www.youtube.com/watch?v=38Ok9UaN1Tk).

Before I get into a review of the rest of the contents,
I'll mention briefly my opinions on the web site as interface: it's meh.
The menu on the left is longer than it appears,
given that scroll bars seem oh so last century
(really! I would love to force “web designers” to use old-fasioned three-button mice and a monitor calibrated to simulate color-blindness,
just to see them strugge with their own designs;
not everyone has a mouse with a scroll-wheel,
nor an Apple Trackpad).
Also,
the diagrams are very inconsistent,
and often times,
way too small to view properly,
even when selected.
Then you'll get the occasionally gigantic diagram.
The layouts seem arbitrary—some horizontal,
some vertical,
and some L-shaped.

And it repeats itself excessively.
I can maybe understand that across pages,
saving a person excessive navigation,
but I found it repeating itself even on a single page.

Other than those issues,
it's mostly functional.
Even with Javascript off,
it's viewable,
even if the diagrams are missing and the contrast is low.

One aspect I did like are the links at the end of each section refering to the source.
That's a nice touch.

So with that out of the way—the “documentation” itself.

Mostly correct.
I have a bunch of small quibbles:

1. examples of running it on the command line don't need the `–config` open if `$BLOG_CONFIG` is set;
2. `$BLOG_CONFIG` isn't checked in `main.c` but in `blog.c`;
3. `mod_blog` outputs RSS 0.91, not RSS 2.0;
4. “The system is written entirely in C and does not have Perl, Python or other scripting dependencies for the core engine itself.”
   Perhaps true? I mean,
   I do use Lua,
   but only for the configuration file;
5. missed out how SUID is used (not for root to run, but as the owner of the blog);
6. the posthook script returning failure doesn't mean the entry wasn't added,
   it just changes the HTTP status code returned.

I also found two problematic bits of code when reviewing this “documentation”—one is an actual bug in the code
(the [file locking diagram](https://deepwiki.com/spc476/mod_blog/5.1-blog-entry-storage#lock-mechanism),
while acurate to the code,
made a caching issue stand out) and another one where I used a literal constant instead of a defined constant.
At least I'm glad for finding those two issues,
even if they haven't been an actual exploitable bug yet
(as I think I'm the only one using `mod_blog`).

In the grand scheme of things,
not terrible for something that might have taken 10 minutes to generate
(I'm not sure—I did other things waiting for the email to arrive).

But one repo does not a trend make.
So I decided upon doing this again with [a09](https://github.com/spc476/a09),
my 6809 assembler.
It's a similar size
(`mod_blog` is 7,400 lines, `a09` is 9,500—same ballpark)
but it's a bit more complicated in logic and hasn't had 26 years of successive refinement done on it.
As such,
I found way more serious issues:

1. [Errors aren't classified](https://deepwiki.com/spc476/a09/2.3-error-handling-and-diagnostics#error-categories). Errors are created as needed,
   sequentially. I make no attempt to bunch error codes into fixed ranges.
2. It missed a key element of the [dead code detection](https://deepwiki.com/spc476/a09/2.3-error-handling-and-diagnostics#dead-code-detection)—it only triggers if the following instruction doesn't have a label.
3. [The listing file isn't kept in the presence of errors](https://deepwiki.com/spc476/a09/2.3-error-handling-and-diagnostics#listing-file-integration).
4. It also got the [removal of generated output files incorrect](https://deepwiki.com/spc476/a09/2.3-error-handling-and-diagnostics#cleanup-behavior)—they're only deleted if an error was detected on pass 1 or 2,
   not if a test failed.
5. It repeats the [precedence table](https://deepwiki.com/spc476/a09/4.1-expression-evaluator#enum-operator) on the [same page](https://deepwiki.com/spc476/a09/4.1-expression-evaluator#operator-parsing-and-precedence).
6. I do *not* have “[Unsupported markdown: blockquote](https://deepwiki.com/spc476/a09/4.1-expression-evaluator#unary-operators-and-size-modifiers)” or “Unsupported markdown: list” unary operators.
7. Oh my God! I can't say how bad this [backend matrix table](https://deepwiki.com/spc476/a09/5-output-format-backends#selective-override-the-backend-matrix) is. It's all sorts of wrong. It's not that it got the supported/non-supported markers backwards,
   it appears to have just made up the results! And the [same information on another page](https://deepwiki.com/spc476/a09/5.1-backend-architecture#override-patterns-by-backend-type) is bad as well.
   Not as bad as the first,
   but that's like saying bronchitus is not as bad as pneumonia.
   Both are bad.
   And it uses a different format for both tables.
   Consistency for the win!
   Sheesh.
8. The [example of writing an instruction to the various formats](https://deepwiki.com/spc476/a09/5-output-format-backends#data-flow-from-opcode-handler-to-backend-to-file) is wrong for the RS-DOS version—the type and length should be two bytes each, not one.
9. The [output format for `-t` is incorrect](https://deepwiki.com/spc476/a09/6-testing-framework#output-formats)—it doesn't show a trace of the code being run unless the `TRON` directives are in use.
10. [Every example of the `.ASSERT` directive is just wrong](https://deepwiki.com/spc476/a09/6-testing-framework#basic-test-structure) as it did not use the proper register references,
    and memory dereferences need a `@` (8-bit) or `@@` (16-bit) prefix.
11. [Where you can use the `.TRON` direcive is wrong](https://deepwiki.com/spc476/a09/6.2-memory-protection-and-tracing#trace-control-directives)—it can be used anywhere;
    it's `.OPT TEST TRON` that can only be used inside a `.TEST` directive.

This,
in my mind,
is a much worse job than it did for `mod_blog`.
I suspect it's due to [the cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) being a bit higher in `a09` than in `mod_blog` due to the cross-cutting nature of the code.
And that probably causes the LLM to run up to,
if not a bit over,
it's context window,
thus causing the confabulations.

I fear that is is meant to be used for legacy code with little or no documentation,
and if it does this poorly on a moderately complex but small code base,
I don't want to contemplate what it would do for a larger,
older,
and gnarlier codebase.
I'd be up to try it,
and I have a code base of 155,000 lines of C code written in the early 90s that's as gnarly as it gets,
but I'm not that familiar with the codebase to feel confident that I can spot all the glaring errors,
much less the more subtle issues.

Another issue are updates to the repo.
The site sells itself as a wiki,
so I suppose another aspect to this is you spend the time going through the generated “documentation” and fixing the errors,
and then keep it up to date as the code changes.
It's not obvious to me if one can rerun this over a changed repo,
and if so,
are the updates merged into the existing documentation?
Replaced outright and you have to go through fixing the documentation again?
I suspect this generated “documentation” will end up worse than bad comments in the code itself.

`mod_blog` has changed drastically over the years,
and while the storage format itself hasn't,
how it works internally has.
There were at least three to four major revisions to the code base over the years.
How major?
One was a nearly a complete rewrite to remove a custom IO layer I had to using C's `FILE *`-style I/O about 18 years ago.
Another one was removal of all global variables about three years ago.
And for the past year,
I've been removing features that I don't use.
That's a lot of documentation to rewrite every few years.

Overall,
this was less obnoxious than having the LLMs write code,
but I feel it's still too inaccurate to be let loose on unfamiliar codebases,
which I suspect is the selling point.

---

#### Discussions about this entry

* [Roko's dancing basilisk | Lobsters](https://lobste.rs/s/lsf4hv/roko_s_dancing_basilisk)
* [Roko's dancing basilisk - Lemmy: Bestiverse](https://lemmy.bestiver.se/post/774745)
* [Roko's Dancing Basilisk | Hacker News](https://news.ycombinator.com/item?id=46129766)
* [Roko';s Dancing Basilisk - ZeroBytes](https://zerobytes.monster/post/10808427)

* [Current](/2025/12)
* [Next](/2025/12/03.1 "A decision on semantic versioning")
* [Previous](/2025/11/30.2 "Semantic versioning is hard; let's go build a rocket")
* [First](/1999/12/04.1 "Sailing the Corporate Seas")
* [Last](/2025/12/03.2 "♫I can't dance, I can't relate, only thing about me is the way I confabulate!♫")
* [Top](http://boston.conman.org/)
* [Home](https://www.conman.org/people/spc/)
* [About](/about/)
* [Archive](/archive/)
* [Search](/refs/search.html)
* [Glossary](/refs/glossary.html)
* [Copyright](/refs/copyright.html)
* [Help](/refs/help.html)
* [Accessibility](/refs/access.html)

![](/columns/column.2025-12-01.jpg "It's hard to see the tree for the ornaments.")

## Obligatory Picture

[![[Self-portrait with a Christmas Tree] Oh Chrismtas Tree!  My Christmas Tree!  Rise up and hear the bells!](https://www.conman.org/people/spc/about/2025/1203.t.jpg "Oh Chrismtas Tree!  My Christmas Tree!  Rise up and hear the bells!")](https://www.conman.org/people/spc/about/2025/1203.html)

## Obligatory Contact Info

* Comments? sean@conman.org

## Obligatory Feeds

* [RSS Feed](/bostondiaries.rss "Rich Site Summary")
* [Atom Feed](/index.atom "A better form of RSS")
* [JSON Feed](/index.json "What is this fascination with JavaScript Serialized Object Notation?")

## Obligatory Links

* [Flutterby!](https://www.flutterby.com/ "Dan Lyke")
* [KIRK.is](https://kirk.is/ "Kirk Israel")

## Obligatory Miscellaneous

* [About the Source Code](/about/)
* [Source Code](https://github.com/spc476/mod_blog)

## Obligatory AI Disclaimer

No AI was used in the
making of this site, unless otherwise noted.

You have my permission to link freely to any entry here. Go
ahead, I won't bite. I promise.

The dates are the permanent links to that day's entries (or
entry, if there is only one entry). The titles are the permanent
links to *that* entry only. The format for the links are
simple: Start with the base link for this site: <https://boston.conman.org/>, then add the date you are
interested in, say [2000/08/01](/2000/08/01),
so that would make the final URL:

<https://boston.conman.org/2000/08/01>

You can also specify the entire month by leaving off the day
portion. You can even select [an arbitrary portion of time.](/about/technical.html)

You may also note subtle shading of the links and that's
intentional: the “closer” the link is (relative to the
page) the “brighter” it appears. It's an experiment in
using color shading to denote the distance a link is from here. If
you don't notice it, don't worry; it's not all *that*
important.

It is assumed that every brand name, slogan, corporate name,
symbol, design element, et cetera mentioned in these pages is a
protected and/or trademarked entity, the sole property of its
owner(s), and acknowledgement of this status is implied.

Copyright © 1999-2025 by Sean Conner. All Rights
Reserved.

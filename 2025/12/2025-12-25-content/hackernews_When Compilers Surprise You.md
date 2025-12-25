---
source: hackernews
title: When Compilers Surprise You
url: https://xania.org/202512/24-cunning-clang
date: 2025-12-25
---

* # [Matt Godbolt's blog](/)
* Menu

* Tags
  + [AI](/AI)
  + [Amusing Stuff](/Amusing-Stuff)
  + [AoCO2025](/AoCO2025)
  + [Blog](/Blog)
  + [Coding](/Coding)
  + [Compiler Explorer](/Compiler-Explorer)
  + [Emulation](/Emulation)
  + [Games](/Games)
  + [Microarchitecture](/Microarchitecture)
  + [New Zealand Trip](/New-Zealand-Trip)
  + [Personal](/Personal)
  + [Python](/Python)
  + [Rants](/Rants)
  + [Rust](/Rust)
  + [WeeBox Project](/WeeBox-Project)
* Archive
  + [AI](/AI-archive)
  + [Amusing Stuff](/Amusing-Stuff-archive)
  + [AoCO2025](/AoCO2025-archive)
  + [Blog](/Blog-archive)
  + [Coding](/Coding-archive)
  + [Compiler Explorer](/Compiler-Explorer-archive)
  + [Emulation](/Emulation-archive)
  + [Games](/Games-archive)
  + [Microarchitecture](/Microarchitecture-archive)
  + [New Zealand Trip](/New-Zealand-Trip-archive)
  + [Personal](/Personal-archive)
  + [Python](/Python-archive)
  + [Rants](/Rants-archive)
  + [Rust](/Rust-archive)
  + [WeeBox Project](/WeeBox-Project-archive)
* About
  + [About me](/MattGodbolt)
  + Contact me
  + [![View Matt Godbolt's profile on LinkedIn](https://www.linkedin.com/img/webpromo/btn_liprofile_blue_80x15.gif "View Matt Godbolt's profile on LinkedIn")](https://www.linkedin.com/in/godbolt)

## When compilers surprise you

Written by me, proof-read by an LLM.

Details at end.

Every now and then a compiler will surprise me with a really smart trick. When I first saw this optimisation I could hardly believe it. I was looking at loop optimisation, and wrote something like this simple function that sums all the numbers up to a given value:

So far so decent: GCC has done some preliminary checks, then fallen into a loop that efficiently sums numbers using `lea` (we’ve [seen this before](/202512/02-adding-integers)). But taking a closer look at the loop we see something unusual:

```
.L3:
  lea edx, [rdx+1+rax*2]        ; result = result + 1 + x*2
  add eax, 2                    ; x += 2
  cmp edi, eax                  ; x != value
  jne .L3                       ; keep looping
```

The compiler has cleverly realised it can do two numbers[1](#fn:check) at a time using the fact it can see we’re going to add `x` *and* `x + 1`, which is the same as adding `x*2 + 1`. Very cunning, I think you’ll agree!

If you turn the optimiser up to `-O3` you’ll see the compiler works even harder to vectorise the loop using parallel adds. All very clever.

This is all for GCC. Let’s see what clang does with our code:

This is where I nearly fell off my chair: **there is no loop**! Clang checks for positive `value`, and if so it does:

```
  lea eax, [rdi - 1]        ; eax = value - 1
  lea ecx, [rdi - 2]        ; ecx = value - 2
  imul rcx, rax             ; rcx = (value - 1) * (value - 2)
  shr rcx                   ; rcx >>= 1
  lea eax, [rdi + rcx]      ; eax = value + rcx
  dec eax                   ; --eax
  ret
```

It was not at all obvious to me what on earth was going on here. By backing out the maths a little, this is equivalent to:

```
v + ((v - 1)(v - 2) / 2) - 1;
```

Expanding the parentheses:

```
v + (v² - 2v - v + 2) / 2 - 1
```

Rearranging a bit:

```
(v² - 3v + 2) / 2 + (v - 1)
```

Multiplying the `(v - 1)` by 2 / 2:

```
(v² - 3v + 2) / 2 + (2v - 2)/2
```

Combining those and cancelling:

```
(v² - v) / 2
```

Simplifying and factoring gives us `v(v - 1) / 2` which is the closed-form solution to the “sum of integers”! Truly amazing[2](#fn:why) - we’ve gone from an O(n) algorithm as written, to an O(1) one!

I love that despite working with compilers for more than twenty years, they can still surprise and delight me. The years of experience and work that have been poured into making compilers great is truly humbling, and inspiring.

We’re nearly at the end of this series - there’s so much more to say but that will have to wait for another time. Tomorrow will be a little different: see you then!

*See [the video](https://youtu.be/V9dy34slaxA) that accompanies this post.*

---

*This post is day 24 of [Advent of Compiler Optimisations 2025](/AoCO2025),
a 25-day series exploring how compilers transform our code.*

*This post was written by a human ([Matt Godbolt](/MattGodbolt)) and reviewed and proof-read by LLMs and humans.*

*Support Compiler Explorer on [Patreon](https://patreon.com/c/mattgodbolt)
or [GitHub](https://github.com/sponsors/compiler-explorer),
or by buying CE products in the [Compiler Explorer Shop](https://shop.compiler-explorer.com)*.

---

1. Some of the initial code checks for odd/even and accounts accordingly. [↩](#fnref:check "Jump back to footnote 1 in the text")
2. Why does the compiler emit this exact sequence and not a slightly more straightforward sequence? I think it’s partly avoiding overflow in cases where it might otherwise overflow *and* just a side effect of the way clang tracks and infers [induction variables](/202512/09-induction-variables). I really don’t know for sure, though. [↩](#fnref:why "Jump back to footnote 2 in the text")

[Permalink](/202512/24-cunning-clang)

Filed under:
[Coding](/Coding)
[AoCO2025](/AoCO2025)

Posted at 06:00:00 CST on 24th December 2025.

### About Matt Godbolt

[Matt Godbolt](/MattGodbolt) is a C++ developer living in Chicago.
He works for [Hudson River Trading](https://www.hudsonrivertrading.com/) on super fun but secret things.
He is one half of the [Two's Complement](https://twoscomplement.org) podcast.
Follow him on [Mastodon](https://hachyderm.io/%40mattgodbolt)
or [Bluesky](https://bsky.app/profile/matt.godbolt.org).

Copyright 2007-2025 Matt Godbolt.
Unless otherwise stated, all content is licensed under the
[Creative Commons Attribution-Noncommercial 3.0 Unported License](https://creativecommons.org/licenses/by-nc/3.0/).
This blog is powered by the MalcBlogSystem by [Malcolm Rowe](https://www.farside.org.uk/).
**Note:** This is my personal website. The views expressed on
these pages are mine alone and not those of my employer.

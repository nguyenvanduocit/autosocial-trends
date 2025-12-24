---
source: hackernews
title: Show HN: Claude Wrapped in the terminal, with a WASM raymarcher
url: https://spader.zone/wrapped/
date: 2025-12-24
---

### Claude Wrapped

2025/12/16

---

# » *tldr*

Run to compare your `Claude Code` usage against the rest of the world while enjoying a spirited holiday Santa Claude rendered in fully lit 3D in your terminal with the power of WASM.

```
bun x @spader/claude-wrapped
```

# » *slightly less tldr*

cpu-destroying wasm claude

Click to load...

I wrote a raymarcher that can render scenes of SDF functions and lights in plain C, compiled it with WASM, jammed it all into a Bun executable that grabs your `~/.claude/stats-cache.json`, uploads it to a SQLite database on The Cloud so you can see how your Claude Code usage stacks up with the rest of the world.

You can see the aforementioned raymarched Claude above, on this very web page! As far as the data collected, the code’s on [GitHub](https://github.com/tspader/claude-wrapped/). It’s a spit of TypeScript and a WASM module that’s as close to pure computation as you can get. Clone and run with `bun start`; the WASM’s already compiled, so you shouldn’t even need a C compiler.

**Please reach out to spader@spader.zone if anything bugs out for you.** I cranked this out in a manic weekend, I’m sure it’s got plenty of bugs.

Read on if you’re interested in the experience of cranking out some rendering code that felt quite real with WASM!

# » *shill*

![Boon Bane](/images/boon_bane.png)

deep copy

A hand-painted science fiction point and click; for fans of Disco Elysium, Philip K. Dick, Pentiment

[Wishlist on Steam](https://store.steampowered.com/app/2639990/Deep_Copy/)

mailing list

If you like modern C, AI, fiction, raymarched corporate blob mascots which represent or perhaps *are* quasi-conscious idiot savants, subscribe to the mailing list.

# » *wrapped season*

It’s that time of year, folks. Here in Atlanta, the leaves are turning to beautiful taupes and crimsons. Winter air blows cold and sweet over a thousand lakes in a thousand counties and cleanses mind, body, and soul. And, of course, people everywhere are whipped into a frenzy over the ability to quantify and commodotize even their most basic humanity.

That’s right. It’s Wrapped season!

I usually use [opencode](https://github.com/sst/opencode), which constantly impresses me with its attention to detail and quality in the terminal. It is better than most web apps; not better *for a TUI*, just better. But I dip into the acutal `claude` CLI sometimes. On one such occasion, I noticed something new: `/stats`.

![](/images/claude-stats.png)

It’s fun! It looks good. It has a fun GitHub-style commit heatmap, and the silly Alex Horne comparative measurements. It’s the kind of thing that makes us love Claude Code. But…can we do better?

# » *we can do better*

I love a good Wrapped as much as the next person. I was a top 0.05% listener of the Grateful Dead last cycle. I don’t know what to *do* with this information, but when did that ever stop me?

![fortunately, spotify wasn’t able to detect that the time was split between me and mr. pig](/images/dead-wrapped.png)

A few minutes of poking around `$HOME/.claude` unearthed `stats-cache.json`! And while this file wasn’t quite as interesting as I’d hoped, it did have more than enough to make a decent wrapped:

* Token and message counts by day and model
* Invocations by hour-of-day
* Costs

The idea hit me like lightning. Grab these stats, make a wrapped. The stats were a little strange – they only went back a month or so. But I assumed this was because Arch wasn’t a first class citizen, and that some `paru -Syu` had wiped some cache or something. Unfortunately, these stats were the best I had. There wasn’t an easy API to get these stats more rigorously. I’m still not entirely sure if they’re per machine or per user (I think the former).

But still, this was more than enough. I had a free weekend, so I slapped it all together.

# » *a very good stack*

This is my first serious (i.e. someone’s actually gonna use it) project with the web. And god damn it, I was *impressed*. I had a fantastic experience writing this thing.

## » *opentui*

The frontend is written in TypeScript using the excellent [OpenTUI](https://github.com/sst/opentui). It’s the renderer for OpenCode, but is completely decoupled. I’d used it once before for a frontend for a small personal utility, but there I’d used SolidJS since I was just rendering plain UI. They use Yoga to lay out your HTML and CSS, and for my simple cases everything worked flawlessly.

Thankfully, OpenTUI also exposes a plain frame buffer you can write whatever you want to. Which is exactly what I wanted! This had the benefit of being able to use HTML (i.e. flex box) on *top* of my pseudo-canvas!

## » *wasm*

The renderer is simple, purely computational C. I was surprised to learn that SIMD works just fine in WASM. The compilation experience was trivially easy; it worked out of the box with clang.

Ironically, when I swapped to my MacBook, the bundled clang was too old to have WASM as a target. I switched to `zig cc`, and everything worked flawlessly. In general, `flawless` is the word I would use to describe my WASM experience. It’s a great spec, and it has a great ecosystem. It’s ready!

## » *bun*

Oh, Bun. I emailed their founder for a job on a whim after seeing a posting in his Twitter bio. And I got an interview! Unfortunately, my deepest tryhard instincts took over on the software part of the interview, and I added too many features before making sure the core was rock solid. Time slipped away, I submitted a piece of junk, and I rightfully didn’t hear back.

A week later, they got acquired by Anthropic. Did I mention the offer came with a little equity?

Ah, well. Life goes on. I had to get that one out. The fact remains that the reason I reached out in the first place is that Bun is in a very rare tier of universally beloved software. It is fast, it does just what you want, and it has clearly been designed with a love of software and a deep technical competence. Building and deploying the TypeScript stuff was a breeze. Unless pressed by, I don’t know, a Mossad agent with some incriminating photos of me from the Epstein files, I’m a Bun lifer. Interview be damned.

## » *cloudflare*

I flirted with spinning up SQLite on an existing ultracheap VPS that I used for [Deep Copy](https://store.steampowered.com/app/2639990/Deep_Copy/). But I was worried that this might have five minutes of fame, and when I say cheap, I mean *cheap*. So, I did the next best thing, which is spinning up SQLITE on someone *else’s* server.

I am *thoroughly* impressed. `wrangler`, minus a brief hiccups with IPv6 on my Arch desktop, Just Worked. I manage my domains through Cloudflare, and it’s absolutely gorgeous inside this walled garden. Everything just slots together. It’s the same feeling I get when building a computer; I have no idea what these components *really* do, but after you do it a few times you just know it’ll work when you snap it all together. Maybe when I have to do real engineering in this domain my feelings will change.

I’ve put up a D1 instance running in the ethereal hyperplane, a cron job to recalculate global stats every fifteen minutes, and I was off.

# » *i’m a moron*

Unfortunately, there was one fatal flaw. I was running a few tests, and I noticed my token count going down. When I checked the aforementioned `stats-cache.json`, I had a horrifying realization. These stats are only for the last month!

I’d put probably 12 or 15 hours into the damn thing at this point. I thought the renderer was quite beautiful. The thought of having wasted all this effort, and to have been such a fool, was pretty rough.

But, as all of my problems resolve, I went downstairs to sort my screws into my new toolbox, which is where my wife found me; dejected, low, on the brink of abandoning life. But, sweet angel that she is, she told me that people wouldn’t care that it was just the last month.

Because, damn it, people like stats. People like Wrapped! And I hope you like mine.

---

[Github](https://github.com/tspader)

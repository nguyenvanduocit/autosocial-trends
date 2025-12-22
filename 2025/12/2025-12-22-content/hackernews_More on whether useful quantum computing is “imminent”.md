---
source: hackernews
title: More on whether useful quantum computing is “imminent”
url: https://scottaaronson.blog/?p=9425
date: 2025-12-22
---

# [Shtetl-Optimized](https://scottaaronson.blog/)

The Blog of Scott Aaronson

If you take nothing else from this blog: quantum computers won't
solve hard problems instantly by just trying all solutions in parallel.

[Also, please read Zvi Mowshowitz's masterpiece on how to fix K-12 education!](https://thezvi.substack.com/p/childhood-and-education-9-school)

---

« [Happy Chanukah](https://scottaaronson.blog/?p=9416)

## [More on whether useful quantum computing is “imminent”](https://scottaaronson.blog/?p=9425 "Permanent Link: More on whether useful quantum computing is “imminent”")

These days, the most common question I get goes something like this:

> A decade ago, you told people that scalable quantum computing wasn’t imminent. Now, though, you claim it plausibly *is* imminent. Why have you reversed yourself??

I appreciated the friend of mine who paraphrased this as follows: “A decade ago you said you were 35. Now you say you’re 45. Explain yourself!”

---

A couple weeks ago, I was delighted to attend [Q2B](https://q2b.qcware.com/conference/2025-silicon-valley) in Santa Clara, where I gave a keynote talk entitled [“Why I Think Quantum Computing Works”](https://www.scottaaronson.com/talks/works.pptx) (link goes to the PowerPoint slides). This is one of the most optimistic talks I’ve ever given. But mostly that’s just because, uncharacteristically for me, here I gave short shrift to the challenge of broadening the class of problems that achieve huge quantum speedups, and just focused on the experimental milestones achieved over the past year. With every experimental milestone, the little voice in my head that asks “but what if Gil Kalai turned out to be right after all? what if scalable QC *wasn’t* possible?” grows quieter, until now it can barely be heard.

Going to Q2B was extremely helpful in giving me a sense of the current state of the field. Ryan Babbush gave a *superb* overview (I couldn’t have improved a word) of the current status of quantum algorithms, while John Preskill’s annual where-we-stand talk was “magisterial” as usual (that’s the word I’ve long used for his talks), making mine look like just a warmup act for his. Meanwhile, Quantinuum took a victory lap, boasting of their recent successes in a way that I considered basically justified.

---

After returning from Q2B, I then did an hour-long [podcast](https://www.youtube.com/watch?si=T9u5MjX9xwCY9zeJ&v=0_7SH3Eons0&feature=youtu.be) with “The Quantum Bull” on the topic “How Close Are We to Fault-Tolerant Quantum Computing?” You can watch it here:

As far as I remember, this is the first YouTube interview I’ve ever done that concentrates entirely on the current state of the QC race, skipping any attempt to explain amplitudes, interference, and other basic concepts. Despite (or conceivably because?) of that, I’m happy with how this interview turned out. Watch if you want to know my detailed current views on hardware—as always, I recommend 2x speed.

Or for those who don’t have the half hour, a quick summary:

* In quantum computing, there are the large companies and startups that might succeed or might fail, but are at least trying to solve the real technical problems, and some of them are making amazing progress. And then there are the companies that have optimized for doing IPOs, getting astronomical valuations, and selling a narrative to retail investors and governments about how quantum computing is poised to revolutionize optimization and machine learning and finance. Right now, I see these two sets of companies as **almost entirely disjoint from each other**.
* The interview also contains my most direct condemnation yet of some of the wild misrepresentations that IonQ, in particular, has made to governments about what QC will be good for (“unlike AI, quantum computers won’t hallucinate because they’re deterministic!”)
* The two approaches that had the most impressive demonstrations in the past year are trapped ions (especially Quantinuum but also Oxford Ionics) and superconducting qubits (especially Google but also IBM), and perhaps also neutral atoms (especially QuEra but also Infleqtion and Atom Computing).
* Contrary to a misconception that refuses to die, I *haven’t* dramatically changed my views on any of these matters. As I have for a quarter century, I continue to profess a lot of confidence in the basic principles of quantum computing theory worked out in the mid-1990s, and I *also* continue to profess ignorance of exactly how many years it will take to realize those principles in the lab, and of which hardware approach will get there first.
* But yeah, *of course* I update in response to developments on the ground, because it would be insane not to! And 2025 was clearly a year that met or exceeded my expectations on hardware, with multiple platforms now boasting >99.9% fidelity two-qubit gates, at or above the theoretical threshold for fault-tolerance. This year updated me in favor of taking more seriously the aggressive pronouncements—the “roadmaps”—of Google, Quantinuum, QuEra, PsiQuantum, and other companies about where they could be in 2028 or 2029.
* One more time for those in the back: the main *known* applications of quantum computers remain (1) the simulation of quantum physics and chemistry themselves, (2) breaking a lot of currently deployed cryptography, and (3) eventually, achieving some *modest* benefits for optimization, machine learning, and other areas (but it will probably be a while before those modest benefits win out in practice). To be sure, the detailed list of quantum speedups expands over time (as new quantum algorithms get discovered) and also contracts over time (as some of the quantum algorithms get dequantized). But the list of known applications “from 30,000 feet” remains fairly close to what it was a quarter century ago, after you hack away the dense thickets of obfuscation and hype.

---

I’m going to close this post with a warning. When Frisch and Peierls wrote their [now-famous memo](https://en.wikipedia.org/wiki/Frisch%E2%80%93Peierls_memorandum) in March 1940, estimating the mass of Uranium-235 that would be needed for a fission bomb, they didn’t publish it in a journal, but communicated the result through military channels only. As recently as February 1939, Frisch and Meitner had [published in *Nature*](https://www.nature.com/articles/143239a0) their theoretical explanation of recent experiments, showing that the uranium nucleus could fission when bombarded by neutrons. But by 1940, Frisch and Peierls realized that the time for open publication of these matters had passed.

Similarly, at some point, the people doing detailed estimates of how many physical qubits and gates it’ll take to break actually deployed cryptosystems using Shor’s algorithm are going to stop publishing those estimates, if for no other reason than the risk of giving too much information to adversaries. Indeed, for all we know, that point may have been passed already. This is the clearest warning that I can offer in public right now about the urgency of migrating to post-quantum cryptosystems, a process that I’m grateful is already underway.

---

**Update:** Someone on Twitter who’s “long $IONQ” says he’ll be [posting about and investigating me](https://x.com/SPAC_Infleqtion/status/2002826241146302651) every day, never resting until UT Austin fires me, in order to punish me for slandering IonQ and other “pure play” SPAC IPO quantum companies. And also, because I’ve been anti-Trump and pro-Biden. He confabulates that I must be trying to profit from my stance (eg by shorting the companies I criticize), it being inconceivable to him that anyone would say anything purely because they care about what’s true.

[![Email, RSS](https://scottaaronson.blog/wp-content/plugins/really-simple-facebook-twitter-share-buttons/images/specificfeeds_follow.png "Email, RSS") Follow](http://www.specificfeeds.com/follow)

This entry was posted
on Sunday, December 21st, 2025 at 11:34 am and is filed under [Adventures in Meatspace](https://scottaaronson.blog/?cat=10), [Quantum](https://scottaaronson.blog/?cat=4), [Speaking Truth to Parallelism](https://scottaaronson.blog/?cat=17).
You can follow any responses to this entry through the [RSS 2.0](https://scottaaronson.blog/?feed=rss2&p=9425) feed.
You can [leave a response](#respond), or [trackback](https://scottaaronson.blog/wp-trackback.php?p=9425) from your own site.

### 2 Responses to “More on whether useful quantum computing is “imminent””

1. [Michael Marthaler](http://quantumsimulations.de) Says:
2. [Scott](http://www.scottaaronson.com) Says:

### Leave a Reply

You can use rich HTML in comments! You can also use basic TeX, by enclosing it within $$ $$ for displayed equations or \( \) for inline equations.

**Comment Policies:**

After two decades of mostly-open comments, in July 2024 *Shtetl-Optimized* transitioned to the following policy:

All comments are treated, by default, as personal missives to me, Scott Aaronson---with no expectation either that they'll appear on the blog or that I'll reply to them.

At my leisure and discretion, and in consultation with the [*Shtetl-Optimized* Committee of Guardians](https://scottaaronson.blog/?p=6576), I'll put on the blog a curated selection of comments that I judge to be particularly interesting or to move the topic forward, and I'll do my best to answer those. But it will be more like Letters to the Editor. Anyone who feels unjustly censored is welcome to the rest of the Internet.

To the many who've asked me for this over the years, you're welcome!

Name (required)

Mail (will not be published) (required)

Website

Δ

---

Shtetl-Optimized is proudly powered by [WordPress](https://wordpress.com/wp/?partner_domain=scottaaronson.blog&utm_source=Automattic&utm_medium=colophon&utm_campaign=Concierge%20Referral&utm_term=scottaaronson.blog)

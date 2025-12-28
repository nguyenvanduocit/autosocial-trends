---
source: hackernews
title: Rust the Process
url: https://www.amalbansode.com/writing/2025-12-24-rust-the-process/
date: 2025-12-28
---

[‹ Writings](../)

🤖 Programming

# Rust the Process

*2025-12-24*

I’ve been trying to learn Rust for years now, and I finally understand enough to (t)rust the process. Or maybe I should say, <T>rust the process.

My efforts began in college through reading [The Rust Programming Language](https://doc.rust-lang.org/book/) book, but I was rarely able to proceed past a hump around chapter 4. I think my emphasis on simply *reading* the book was a large reason for my failures. It’s like when I believed I could ace Math exams by RTFM-ing and skipping practice problems. Fortunately, I’m not that smart.

Meanwhile, college classes and my work with the [Michigan Solar Car team](https://www.solarcar.engin.umich.edu/our-legacy) continued to exercise my C++ skills. With the solar car team, I did my best to build atop the C++ fundamentals taught in Michigan’s CS curriculum ([example](/writing/2022-03-22-modular-polynomials/)). I’d seen most attempts at introducing fancy languages, libraries, or infra fail because it was impossible for such work to endure past a single motivated person’s college tenure. Our team skewed young (think college freshmen and sophomores) so things had to remain within their grasp.

My work at SpaceX continued to lean heavily on C and C++. Our use of C++ was pretty measured, with some flexibility about when code had to be strictly [flight-software-deterministic](https://en.wikipedia.org/wiki/The_Power_of_10%3A_Rules_for_Developing_Safety-Critical_Code) versus memory-sticks-are(were?)-cheaper-than-engineers. New ecosystems like Go and Rust occasionally flared up, but rarely went mainstream in the networking/flight software groups for a number of reasons. By the time I quit SpaceX, the language ~~holy wars~~ debates continued to wreak ~~havoc~~ popcorn emojis across software fiefdoms.

So this year, I began a more intentional effort to *write* more than just *read* Rust. My rustacean friends wouldn’t stop raving about it and the systems programming ecosystem is certainly embracing it.

I began with [rustlings](https://rustlings.rust-lang.org/) — a good way to get up to speed on syntax and basic programming patterns, though the exercises didn’t truly make me feel *comfortable* writing Rust. I felt the itch to work on a “real” project where I could still fall back to some guided help on language features.

Eventually, I found [Raytracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html) buried in my bookmarks. It seemed like an interesting way to quickly learn and iterate since the feedback would be visible (important Gen Z dopamine hits) and almost instant. This [Rust-specific translation](https://the-ray-tracing-road-to-rust.vercel.app/) was a great guide. I wish I could say I recalled and understood every bit of math I did, but let’s brush that under the rug.

Having completed the exercise, I can now present these rusty balls:

![Raytracing output displaying orange balls](rusty.jpeg)

Following the raytracing exercise, I began hunting for more ideas. I’ve enjoyed using little Terminal UI (TUI) programs like `htop` and `k9s` these last few years, so creating my own TUI was at the top of my mind. Around the same time, I found an open-source cousin of [LittleSnitch](https://www.obdev.at/products/littlesnitch/index.html) for Linux platforms — [OpenSnitch](https://github.com/evilsocket/opensnitch) — that I was interested in running.

OpenSnitch works pretty well, but the user experience can be interesting. For one, it’s distracting to be interrupted by a full-blown UI window on each connection. There are ways to avoid this, but they didn’t suit my preferences.

Secondly, if you usually live in the terminal or your Linux environment is headless (typically remote boxes, containers, etc), you can’t use the UI without X/VNC/RDP or running a local instance of the GUI binary. All of these options are painful and don’t solve the first problem.

![Screenshot of OpenSnitch UI](https://user-images.githubusercontent.com/2742953/85205382-6ba9cb00-b31b-11ea-8e9a-bd4b8b05a236.png)

So I figured a TUI could be a good medium for dealing with the firewall daemon, and I began scoping out what it would take to make my Rust TUI ambitions come to life. The OpenSnitch authors did a pretty good job of delineating the daemon’s and UI’s responsibilities with a [gRPC API](https://github.com/evilsocket/opensnitch/tree/master/proto), so that became the foundation to build up from.

After sparring with `tokio` and `tonic`, I was able to see bytes on the wire — always a good first step. As I built more application logic, I ran into some setbacks with the borrow checker as well as [some HTTP library quirks](https://github.com/hyperium/tonic/issues/742). I was secretly hoping to encounter the former since the best battles yield the best learning, but I didn’t enjoy the latter. I heavily resisted the urge to return to C++ or Go during these episodes.

Things eventually began to take shape as I embraced `async` and built up a messaging layer between a `ratatui`-based TUI and the `tonic`-driven gRPC server. Despite all the headaches, working in async Rust has been far more pleasant than my war memories of JavaScript and the NodeJS ecosystem. Strong types help.

The TUI now looks like the screenshot below and [lives on GitHub](https://github.com/amalbansode/opensnitch-tui).

![Screenshot of OpenSnitch TUI](https://github.com/amalbansode/opensnitch-tui/raw/main/static/screenshot.png)

Working on the TUI also revived some of [my passion for graphic design](/visual-design/) — just the perfect amount though, since I don’t ever want to be trapped under an Adobe subscription again. And incidentally, the TUI might turn out to be a good tool for the AI-assisted programming age, [per a happy user report](https://github.com/amalbansode/opensnitch-tui/issues/1).

Some more scatter-brained notes from the development journey:

* Enums with associated data proved to be the best thing since sliced bread. As a networking software programmer, these were common patterns I dealt with (think: address families, protocols, variants, optionals, etc.) and I’m thankful that Rust makes it hard to shoot yourself in the foot when dealing with them.
* Rust’s philosophy on error handling is pretty similar to the philosophy some of us stuck with at SpaceX — error return types and no exceptions (panics) — so it was good to have that familiar pattern back.
* My history is a little spotty now, but one of the noteworthy times I was tripped up was when Rust weaved together mutability and ownership rules to forbid certain data sharing. There, I learned about [interior mutability](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html) and [shared state patterns](https://doc.rust-lang.org/book/ch16-03-shared-state.html) to guarantee correctness.
* Unit testing, formatting, and “static analysis” being built into core language tooling is super convenient. It may have been nice to expose some reasonable defaults for code coverage measurements too. I was able to generate flamegraphs using an [open-source extension](https://crates.io/crates/flamegraph).
* I sometimes feel C and C++ were very clear on where data lives (stack vs heap) and how it’s organized (struct alignment), while Rust seems a little more ✨ opaque ✨. I’ve felt a similar way working in Go. My work hasn’t required me to dive into that level of optimization/performance yet, but it’s something I’d be interested in learning about at some point. The compiler’s magic is likely *good enough* to not have to care about this in the common case.
* The async runtime I’m using for the TUI, `tokio`, also amplifies the same feelings. The fact that I wasn’t usually compelled to dig super deep into `tokio` is probably a sign of good abstraction. However, a part of me really wants to learn more here too.
* Using an LLM to develop the TUI could’ve made the development process a million times quicker, but I avoided it 99% of the time to maximize fun and learning opportunities. The 1% of times I did use an LLM were to understand the aforementioned interior mutability mess and for learning how async tasks could be invoked with a timeout.

I’m pretty happy with the progress I’ve made. From my limited perspective, it seems wise to *consider* writing any greenfield projects in Rust rather than C or C++ today. Rust’s primitives and constraints deter many footguns, albeit not all the time (looking at you, [Cloudflare](https://blog.cloudflare.com/18-november-2025-outage/#memory-preallocation)). Some Rust does honestly feel a little ugly to read, but: (a) it’s probably an acquired taste just like C++, and (b) it’s probably an 80-20 thing too. There’s likely additional nuance if developing for platforms that aren’t commodity processors or OSes.

These decisions are obviously far more complicated than just a comparison of language features. Organizations typically have to trade against mountains of tech debt and the innovator’s dilemma internally. *Human factors*, like building a sustainable software suite for a college engineering team, require a different decision matrix too, so I’m not on the absolute end of rust-it-all just yet. Perhaps we can revisit that once colleges are teaching Rust in 100-level computer science courses and AI agents are building solar-powered racecars.

Until then, I’m glad I ran through these exercises to teach myself Rust, despite being late to jump on the bandwagon. I’ve had a lot of fun building the [OpenSnitch TUI](https://github.com/amalbansode/opensnitch-tui) and I feel more comfortable about encountering Rust in personal projects or work in the future. I have no doubt that there’s a lot more to be learned as well. If you’re still on the fence, (t)rust the process.

*~ Amal*

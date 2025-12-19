---
source: hackernews
title: 1.5 TB of VRAM on Mac Studio – RDMA over Thunderbolt 5
url: https://www.jeffgeerling.com/blog/2025/15-tb-vram-on-mac-studio-rdma-over-thunderbolt-5
date: 2025-12-19
---

[Skip to main content](#main-content)

**[Jeff Geerling](/ "Home")**

## Main menu

* [Merch](https://www.redshirtjeff.com)
* [About](/about)
* [Blog](/blog)
* [Projects](/projects)

# 1.5 TB of VRAM on Mac Studio - RDMA over Thunderbolt 5

December 18, 2025

![Mac Studio Cluster](/sites/default/files/images/mac-studio-cluster-1-hero.jpg)

Apple gave me access to this Mac Studio cluster to test RDMA over Thunderbolt, a [new feature in macOS 26.2](https://developer.apple.com/documentation/macos-release-notes/macos-26_2-release-notes#RDMA-over-Thunderbolt). The easiest way to test it is with [Exo 1.0](https://exolabs.net), an open source private AI clustering tool. RDMA lets the Macs all act like they have one giant pool of RAM, which speeds up things like massive AI models.

The stack of Macs I tested, with 1.5 TB of unified memory, costs just shy of $40,000, and if you're wondering, no I cannot justify spending that much money for this. Apple loaned the Mac Studios for testing. I also have to thank DeskPi for sending over the 4-post mini rack containing the cluster.

The last time I remember hearing anything interesting about Apple and HPC (High Performance Computing), was back in the early 2000s, when they still made the [Xserve](https://support.apple.com/en-us/112625).

![Apple Xgrid icon](/sites/default/files/images/apple-xgrid-icon.png)

They had a proprietary clustering solution called Xgrid... that [landed with a thud](https://forums.macrumors.com/threads/xgrid-officially-dead.1412900/). A few universities built some clusters, but it never really caught on, and now Xserve is a distant memory.

I'm not sure if its by accident or Apple's playing the long game, but the M3 Ultra Mac Studio hit a sweet spot for running local AI models. And with RDMA support [lowering memory access latency from 300μs down to < 50μs](https://github.com/ml-explore/mlx/pull/2808), clustering now adds to the performance, especially running huge models.

They also hold their own for creative apps and at least small-scale scientific computing, all while running under 250 watts and almost whisper-quiet.

The two Macs on the bottom have *512 GB* of unified memory and 32 CPU cores, and cost $11,699 each. The two on top, with half the RAM, are $8,099 each[1](#fn:pricing).

They're not cheap.

But with Nvidia releasing their [DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/) and AMD with their [AI Max+ 395](https://www.amd.com/en/products/processors/laptop/ryzen/ai-300-series/amd-ryzen-ai-max-plus-395.html) systems, both of which have a *fourth* the memory (128 GB maximum), I thought I'd put this cluster through it's paces.

## Video

This blog post is the reformatted text version of my latest YouTube video, which you can watch below.

## A Mini Mac Rack

In a stroke of perfect timing, DeskPi sent over a new 4-post mini rack called the [TL1](https://deskpi.com/products/deskpi-rackmate-t1l-rackmount-10u-open-frame-network-rack-for-10-it-network-audio-and-video-device) the day before these Macs showed up.

![Mac Studio cluster cabling - Thunderbolt 5](/sites/default/files/images/mac-studio-cluster-2-cabling-thunderbolt.jpg)

I kicked off [Project MINI RACK](https://mini-rack.jeffgeerling.com) earlier this year, but the idea is you can have the benefits of rackmount gear, but in a form factor that'll fit on your desk, or tucked away in a corner.

Right now, I haven't seen any solutions for mounting Mac Studios in 10" racks besides [this 3D printable enclosure](https://www.printables.com/model/1283153-10-inch-rack-mount-for-mac-studio-m1), so I just put them on some 10" rack shelves.

The most annoying thing about racking *any* non-Pro Macs is the power button. On a Mac Studio it's located in the back left, on a rounded surface, which means rackmount solutions need to have a way to get to it.

The open sides on the mini rack allow me to reach in and press the power button, but I still have to hold onto the Mac Studio while doing so, to prevent it from sliding out the front!

It *is* nice to have the front ports on the Studio to plug in a keyboard and monitor:

![Mac Studio cluster - KVM keyboard monitor mouse](/sites/default/files/images/mac-studio-cluster-3-kvm-front.jpg)

For power, I'm glad Apple uses an internal power supply. Too many 'small' PCs are small only because they punt the power supply into a giant brick outside the case. Not so, here, but you *do* have to deal with Apple's non-C13 power cables.

![DGX Spark mini cluster QSFP ports](/sites/default/files/images/mac-studio-cluster-4-dgx-spark-qsfp.jpg)

The DGX Spark does better than Apple on networking. They have these big rectangle QSFP ports (pictured above). The plugs hold in better, while still being easy to plug in and pull out.

The Mac Studios have 10 Gbps Ethernet, but the high speed networking (something like 50-60 Gbps real-world throughput) on the Macs comes courtesy of Thunderbolt. Even with [premium Apple cables](https://www.apple.com/us/search/Thunderbolt-5-USB%E2%80%91C-Pro-Cable-1%C2%A0m?tab=accessories) costing $70 each, I don't feel like the mess of plugs would hold up for long in many environments.

There's tech called [ThunderLok-A](https://www.sonnetstore.com/products/thunderlok-a), which adds a little screw to each cable to hold it in, but I wasn't about to drill out and tap the loaner Mac Studios, to see if I could make them work.

Also, AFAICT, Thunderbolt 5 switches don't exist, so you can't plug in multiple Macs to one central switch—you have to plug every Mac into every other Mac, which adds to the cabling mess. Right now, you can only cross-connect up to four Macs, but I think that may not be a hard limit for the current Mac Studio.

The bigger question is: do you need a full cluster of Mac Studios at all? Because just one is already a beast, matching *four* maxed-out DGX Sparks or AI Max+ 395 systems. Managing clusters can be painful.

## M3 Ultra Mac Studio - Baseline

To inform that decision, I ran some baseline benchmarks, and posted *all* my results (much more than I highlight in this blog post) to my [sbc-reviews](https://github.com/geerlingguy/sbc-reviews/issues/95) project.

I'll compare the M3 Ultra Mac Studio to a:

* Dell Pro Max with GB10 (similar to the Nvidia DGX Spark, but with better thermals)
* Framework Desktop Mainboard (with AMD's AI Max+ 395 chip)

![Mac Studio - M3 Ultra Geekbench 6](/sites/default/files/images/mac-studio-cluster-benchmarks.002.geekbench.jpeg)

First, Geekbench. The M3 Ultra, running two-generations-old CPU cores, beats the other two in both single and multi-core performance (and even more handily in Geekbench 5, which is more suitable for CPUs with many cores).

![Mac Studio - M3 Ultra HPL](/sites/default/files/images/mac-studio-cluster-benchmarks.003.hpl_.jpeg)

Switching over to a double-precision FP64 test, my classic [top500 HPL benchmark](https://github.com/geerlingguy/top500-benchmark/issues/89), the M3 Ultra is the first small desktop I've tested that breaks 1 Tflop FP64. It's almost double Nvidia's GB10, and the AMD AI Max chip is left in the dust.

![Mac Studio - M3 Ultra HPL Efficiency](/sites/default/files/images/mac-studio-cluster-benchmarks.004.hpl-efficiency.jpeg)

Efficiency on the CPU is also great, though that's been the story with Apple since the A-series, with all their chips. And related to that, idle power draw on here is less than 10 watts:

![Mac Studio - M3 Ultra Power Draw at idle](/sites/default/files/images/mac-studio-cluster-benchmarks.005.power-draw.jpeg)

I mean, I've seen *SBC's* idle over 10 watts, much less something that could be considered a personal supercomputer.

Regarding AI Inference, the M3 Ultra stands out, both for small and large models:

![Mac Studio - M3 Ultra AI Llama 3B](/sites/default/files/images/mac-studio-cluster-benchmarks.006.ai-llama-3b.jpeg)

![Mac Studio - M3 Ultra AI Llama 70B](/sites/default/files/images/mac-studio-cluster-benchmarks.007.ai-llama-70b.jpeg)

Of course, the *truly* massive models (like DeepSeek R1 or Kimi K2 Thinking) won't even run on a single node of the other two systems.

![Mac Studio M3 Ultra - Price comparison](/sites/default/files/images/mac-studio-cluster-benchmarks.009.price_.jpeg)

But this *is* a $10,000 system. You expect more when you pay more.

But consider this: a single M3 Ultra Mac Studio has more horsepower than my *entire* [Framework Desktop cluster](/blog/2025/i-clustered-four-framework-mainboards-test-huge-llms), using *half* the power. I also compared it to a tiny 2-node cluster of Dell Pro Max with GB10 systems, and a single M3 Ultra still comes ahead in performance and efficiency, with double the memory.

## Mini Stack, Maxi Mac

But with four Macs, how's clustering and remote management?

The biggest hurdle for *me* is macOS itself. I automate *everything I can* on my Macs. I maintain the most popular [Ansible playbook for managing Macs](https://github.com/geerlingguy/mac-dev-playbook), and can say with some authority: managing Linux clusters is easier.

Every cluster has hurdles, but there are a bunch of small struggles when managing a cluster of Macs without additional tooling like MDM. For example: did you know there's no way to run a system upgrade (like to 26.2) via SSH? You *have* to click buttons in the UI.

Instead of plugging a KVM into each Mac remotely, I used Screen Sharing (built into macOS) to connect to each Mac and complete certain operations via the GUI.

## HPL and Llama.cpp

With everything set up, I tested HPL over 2.5 Gigabit Ethernet, and llama.cpp over that and Thunderbolt 5.

![Mac Studio - Clustered HPL vs HPL on one node](/sites/default/files/images/mac-studio-cluster-hpl-llama.001.hpl_.jpeg)

For HPL, I got 1.3 Teraflops with a single M3 Ultra. With all four put together, I got 3.7, which is less than a 3x speedup. But keep in mind, the top two Studios only have half the RAM of the bottom two, so a 3x speedup is probably around what I'd expect.

I tried running HPL through Thunderbolt (not using RDMA, just TCP), but after a minute or so, both Macs I had configured in a cluster would crash and reboot. I looked into using [Apple's MLX wrapper for `mpirun`](https://ml-explore.github.io/mlx/build/html/usage/distributed.html), but I couldn't get that done in time for this post.

Next I tested llama.cpp running AI models over 2.5 gigabit Ethernet versus Thunderbolt 5:

![Mac Studio - llama.cpp TB5 vs Ethernet performance](/sites/default/files/images/mac-studio-cluster-hpl-llama.002.llama-tb5-eth.jpeg)

Thunderbolt definitely wins for latency, even if you're not using RDMA.

[All my llama.cpp cluster test results are listed here](https://github.com/geerlingguy/beowulf-ai-cluster/issues/17)—I ran many tests that are not included in this blog post, for brevity.

## Enabling RDMA

[Exo 1.0](https://exolabs.net) was launched today (at least, so far as I've been told), and the headline feature is RDMA support for clustering on Macs with Thunderbolt 5.

![Mac Studio rdma_ctl enable](/sites/default/files/images/mac-studio-cluster-10-rdma_ctl.jpg)

To *enable* RDMA, though, you have to boot into recovery mode and run a command:

1. Shut down the Mac Studio
2. Hold down the power button for 10 seconds (you'll see a boot menu appear)
3. Go into Options, then when the UI appears, open Terminal from the Utilities menu
4. Type in `rdma_ctl enable`, and press enter
5. Reboot the Mac Studio

Once that was done, I ran a bunch of HUGE models, including Kimi K2 Thinking, which at 600+ GB, is too big to run on a single Mac.

![Mac Studio Kimi K2 Thinking on full cluster in Exo](/sites/default/files/images/mac-studio-cluster-25-exo-kimi-k2-thinking.jpg)

I can run models like that across multiple Macs using both llama.cpp and Exo, but the latter is so far the only one to support RDMA. Llama.cpp currently uses an [RPC method](https://github.com/ggml-org/llama.cpp/blob/master/tools/rpc/README.md) that spreads layers of a model across nodes, which scales but is inefficient, causing performance to decrease as you add more nodes.

This benchmark of Qwen3 235B illustrates that well:

![Mac Studio cluster - Qwen3 235B Result llama.cpp vs Exo](/sites/default/files/images/mac-studio-cluster-ai-full-1-qwen3-235b.jpeg)

Exo speeds *up* as you add more nodes, hitting 32 tokens per second on the full cluster. That's definitely fast enough for vibe coding, if that's your thing, but it's not mine.

So I moved on to testing DeepSeek V3.1, a 671 billion parameter model:

![Mac Studio cluster - DeepSeek R1 671B Result llama.cpp vs Exo](/sites/default/files/images/mac-studio-cluster-ai-full-2-deepseek-3.1-671b.jpeg)

I was a little surprised to see llama.cpp get a little speedup. Maybe the network overhead isn't so bad running on two nodes? I'm not sure.

Let's move to the biggest model I've personally run on anything, Kimi K2 Thinking:

![Mac Studio cluster - Kimi-K2-Thinking Result llama.cpp vs Exo](/sites/default/files/images/mac-studio-cluster-ai-full-3-kimi-k2-thinking.jpeg)

This is a 1 *trillion* parameter model, though there's only 32 billion 'active' at any given time—that's what the A is for in the A32B there.

But we're still getting around 30 tokens per second.

Working with some of these huge models, I can see how AI has some use, especially if it's under my own local control. But it'll be a long time before I put much trust in what I get out of it—I treat it like I do Wikipedia. Maybe good for a jumping-off point, but don't *ever* let AI replace your ability to think critically!

But this video isn't about the merits of AI, it's about a Mac Studio Cluster, RDMA, and Exo.

They performed great... *when* they performed.

## Stability Issues

**First a caveat**: I was working with *prerelease* software while testing. A lot of bugs were worked out in the course of testing.

But it was obvious RDMA over Thunderbolt is new. When it works, it works great. When it doesn't... well, let's just say I was glad I had Ansible set up so I could shut down and reboot the whole cluster quickly.

![Mac Studio Cluster - Exo failed loading model](/sites/default/files/images/mac-studio-cluster-15-exo-failed.jpg)

I also mentioned HPL crashing when I ran it over Thunderbolt. Even if I do get that working, you're talking a maximum of 4 Macs with the network set up like this (at least as of late 2025).

Besides that, I still have some underlying trust issues with Exo, since the developers [went AWOL for a while](https://github.com/exo-explore/exo/issues/819).

They *are* keeping true to their open source roots, releasing Exo 1.0 under the Apache 2.0 license, but I wish they didn't have to hole up and develop it in secrecy; that's probably a side effect of working so closely with Apple.

I mean, it's their right, but as someone who maybe develops *too* much in the open, I dislike layers of secrecy around any open source project.

I *am* excited to see where it goes next. They teased [putting a DGX Spark in front of a Mac Studio cluster](https://blog.exolabs.net/nvidia-dgx-spark/) to speed up prompt processing... maybe they'll get support re-added for Raspberry Pi's, too? Who knows.

## Unanswered Questions / Topics to Explore Further

But I'm left with more questions:

* Where's the M5 Ultra? If Apple released one, it would be [a lot faster](https://machinelearning.apple.com/research/exploring-llms-mlx-m5) for machine learning.
* Could Apple revive the Mac Pro to give me all the PCIe bandwidth I desire for faster clustering, without being held back by Thunderbolt?
* Could Macs get [SMB Direct](https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-direct)? Network file shares would behave as if attached directly to the Mac, which'd be amazing for video editing or other latency-sensitive, high-bandwidth applications.

Finally, what about other software? [Llama.cpp](https://github.com/ggml-org/llama.cpp/issues/9493#event-21411249655) and other apps could get a speed boost with RDMA support, too.

## Conclusion

Unlike *most* AI-related hardware, I'm kinda okay with Apple hyping this up. When the AI bubble goes bust, Mac Studios are still fast, silent, and capable workstations for creative work (I use an M4 Max at my desk!).

But it's not all rainbows and sunshine in Apple-land. Besides being more of a headache to manage Mac clusters, Thunderbolt 5 holds these things back from their true potential. QSFP would be better, but it *would* make the machine less relevant for people who 'just want a computer'.

Maybe as a consolation prize, they could replace the Ethernet jack and one or two Thunderbolt ports on the back with QSFP? That way we could use network switches, and cluster more than four of these things at a time...

---

1. As configured. Apple put in 8 TB of SSD storage on the 512GB models, and 4TB on the 256GB models. [↩︎](#fnref:pricing)

## Further reading

* [I clustered four Framework Mainboards to test huge LLMs](/blog/2025/i-clustered-four-framework-mainboards-test-huge-llms)
* [I regret building this $3000 Pi AI cluster](/blog/2025/i-regret-building-3000-pi-ai-cluster)
* [LLMs accelerated with eGPU on a Raspberry Pi 5](/blog/2024/llms-accelerated-egpu-on-raspberry-pi-5)

[apple](/tags/apple)

[mac](/tags/mac)

[mac studio](/tags/mac-studio)

[rdma](/tags/rdma)

[hpc](/tags/hpc)

[arm](/tags/arm)

[thunderbolt](/tags/thunderbolt)

[exo](/tags/exo)

[networking](/tags/networking)

[video](/tags/video)

[youtube](/tags/youtube)

* [Add new comment](/comment/reply/node/3522/comment_node_blog_post#comment-form "Share your thoughts and opinions.")

## Comments

Andrej Filipovic – [5 hours ago](/comment/36526#comment-36526)

Thank you for the great post, Jeff. Has there been any indication they'll backport support for RDMA over TB to the older models?

It seems rather strange that Exo disappeared for a few months and has now come out with a totally new rewrite of the project (in some kind of backroom deal with Apple) that exclusively supports only the newest generation of Apple Silicon computers (M3/M4) while the older ones (M1/M2) are apparently left in the dust wrt RDMA.

I'm not trying to blow smoke or complain; there are a lot of people who took Alex Cheema, Awni Hannun, and Georgi Gerganov at their word when they pointed out that the M2 series is really great for inference. Georgi himself has an M2 Ultra 192GB; is he going to quietly trade it in for an M3 Ultra and eat a $7,000 loss because... Apple doesn't feel like issuing a microcode patch that enables RDMA on the M2? It all feels so fake.

It almost feels like this is a big marketing stunt by Apple to get the home computing hobbyist community to spend a few more $B on new Apple Silicon.

And of course, in the time between MLX/Exo coming out and the present, we completely lost all the main developers of Asahi Linux.

* [Reply](/comment/reply/node/3522/comment_node_blog_post/36526)

Jeff Geerling – [4 hours ago](/comment/36528#comment-36528)

In reply to [Thank you for the great post…](/comment/36526#comment-36526) by Andrej Filipovic

I don't know anything that's happened behind closed doors, but I have seen many times when an AI startup that does something interesting/promising get gobbled up and just kinda disappear from the face of the planet.

At least this time Exo re-surfaced! I'm more interested in the HPC aspects, than LLM to be honest. It'd be neat to build a true beowulf cluster with RDMA of a Mac, an Nvidia node, an AMD server, etc. and see what kind of fun I could have :)

* [Reply](/comment/reply/node/3522/comment_node_blog_post/36528)

Premkumar Mohanraj – [3 hours ago](/comment/36529#comment-36529)

Have you tried with thunderbolt 5 hosts with thunderbolt 4 hosts? I wanted to try this clustering for local LLM.

* [Reply](/comment/reply/node/3522/comment_node_blog_post/36529)

Sam – [11 min ago](/comment/36532#comment-36532)

I've been emailing with Deskpi about the TL1, do you know if it is able to fit 10"x10" rack like this one?
[https://www.printables.com/model/1176409-10-x10-minirack-now-with-micro…](https://www.printables.com/model/1176409-10-x10-minirack-now-with-micro-atx-mount/files#preview.file.PsxHL)
The rails looks slightly oddly shaped but it seems like it should work.
Makes it way cheaper when getting a MOBO for your rack if you can fit a microATX instead of mini

It would make my current setup MUCH less janky

* [Reply](/comment/reply/node/3522/comment_node_blog_post/36532)

Jeff Geerling – [3 min ago](/comment/36533#comment-36533)

In reply to [I've been emailing with…](/comment/36532#comment-36532) by Sam

No only up to like 8.75" I think... 220mm?

* [Reply](/comment/reply/node/3522/comment_node_blog_post/36533)

Search

![Geerling Family Crest](/sites/default/files/files/geerling-family-crest.svg)
All content copyright Jeff Geerling. As an Amazon Associate I earn from qualifying purchases. [Top of page](#top).

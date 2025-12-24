---
source: hackernews
title: Meta is using the Linux scheduler designed for Valve's Steam Deck on its servers
url: https://www.phoronix.com/news/Meta-SCX-LAVD-Steam-Deck-Server
date: 2025-12-24
---

[![Phoronix](/phxcms7-css/phoronix.png)](/)

* [Articles & Reviews](/reviews)
* [News Archive](/news)
* [Forums](/forums/)
* [Premium Ad-Free](/phoronix-premium)
* [Contact](/contact)
* Popular Categories
* Close

* [Articles & Reviews](/reviews)
* [News Archive](/news)
* [Forums](/forums/)
* [Premium](/phoronix-premium)
* [Contact](/contact)
* Categories

  [Computers](/reviews/Computers) [Display Drivers](/reviews/Display%2BDrivers) [Graphics Cards](/reviews/Graphics%2BCards) [Linux Gaming](/reviews/Linux%2BGaming) [Memory](/reviews/Memory) [Motherboards](/reviews/Motherboards) [Processors](/reviews/Processors) [Software](/reviews/Software) [Storage](/reviews/Storage) [Operating Systems](/reviews/Operating%2BSystems) [Peripherals](/reviews/Peripherals)

# Meta Is Using The Linux Scheduler Designed For Valve's Steam Deck On Its Servers

Written by [Michael Larabel](https://www.michaellarabel.com/) in [Linux Kernel](/linux/Linux%2BKernel) on 23 December 2025 at 06:10 AM EST. [16 Comments](/forums/node/1601261)

![LINUX KERNEL](/assets/categories/linuxkernel.webp)

An interesting anecdote from this month's Linux Plumbers Conference in Tokyo is that Meta (Facebook) is using the Linux scheduler originally designed for the needs of Valve's Steam Deck... On Meta Servers. Meta has found that the scheduler can actually adapt and work very well on the hyperscaler's large servers.

SCX-LAVD as the Latency-criticality Aware Virtual Deadline scheduler has [worked out very well for the needs of Valve's Steam Deck](https://www.phoronix.com/news/LAVD-Scheduler-Linux-Gaming) with similar or better performance than [EEVDF](https://www.phoronix.com/search/EEVDF). SCX-LAVD has been worked on by Linux consulting firm Igalia under contract for Valve. SCX-LAVD has also seen varying use by the CachyOS Handheld Edition, Bazzite, and other Linux gaming software initiatives.

[![Steam Deck and Server](//www.phoronix.net/image.php?id=2025&image=meta_lavd_1_med)](//www.phoronix.com/image-viewer.php?id=2025&image=meta_lavd_1_lrg)

It turns out that besides working well on handhelds, SCX-LAVD can also end up working well on large servers too. The presentation at LPC 2025 by Meta engineers was in fact titled "*How do we make a Steam Deck scheduler work on large servers*." At Meta they have explored SCX\_LAVD as a "default" fleet scheduler for their servers that works for a range of hardware and use-cases for where they don't need any specialized scheduler.

![Meta SCX LAVD](//www.phoronix.net/image.php?id=2025&image=meta_lavd_2_med)

They call this scheduler built atop sched\_ext as "Meta's New Default Scheduler". LAVD they found to work well across the growing CPU and memory configurations of their servers, nice load balancing between CCX/LLC boundaries, and more. Those wishing to learn more about Meta's use and research into SCX-LAVD can find the Linux Plumbers Conference presentation embedded below along with the [slide deck](https://lpc.events/event/19/contributions/2099/attachments/1875/4020/lpc-2025-lavd-meta.pdf).

[16 Comments](/forums/node/1601261)

[Tweet](https://twitter.com/share)

Related News

[Rex: Proposed Safe Rust Kernel Extensions For The Linux Kernel, In Place Of eBPF](/news/Linux-Kernel-Rust-Rex)

[Linux 6.19-rc2 Released Following A Quiet Week](/news/Linux-6.19-rc2-Released)

[Linux Kernel Rust Code Sees Its First CVE Vulnerability](/news/First-Linux-Rust-CVE)

[Kernel Graphics Driver Changes Already Begin Lining Up For Linux 6.20~7.0](/news/Linux-DRM-Misc-Next-Post-6.19)

[Linux 6.19-rc1 Released From Japan](/news/Linux-6.19-rc1-Released)

[New RTC Drivers For Apple & NVIDIA With Linux 6.19](/news/Linux-6.19-RTC)

About The Author

Michael Larabel is the principal author of Phoronix.com and founded the site in 2004 with a focus on enriching the Linux hardware experience. Michael has written more than 20,000 articles covering the state of Linux hardware support, Linux performance, graphics drivers, and other topics. Michael is also the lead developer of the Phoronix Test Suite, Phoromatic, and OpenBenchmarking.org automated benchmarking software. He can be followed via [Twitter](https://twitter.com/MichaelLarabel), [LinkedIn](https://www.linkedin.com/in/michaellarabel/), or contacted via [MichaelLarabel.com](https://www.michaellarabel.com/).

Popular News This Week

[Mozilla Names New CEO, Firefox To Evolve Into A "Modern AI Browser"](/news/Mozilla-New-CEO-AI)

[Linux Kernel Rust Code Sees Its First CVE Vulnerability](/news/First-Linux-Rust-CVE)

[Meta Is Using The Linux Scheduler Designed For Valve's Steam Deck On Its Servers](/news/Meta-SCX-LAVD-Steam-Deck-Server)

[Torvalds On Linux Security Modules: "I Already Think We Have Too Many Of Those Pointless Things"](/news/Linus-Torvalds-Too-Many-LSM)

[Fedora Games Lab Looks To Be Revitalized As Modern Linux Gaming Showcase](/news/Fedora-Games-Lab-2026)

[GotaTun Open-Source Rust WireGuard Implementation Announced By Mullvad](/news/GotaTun-Rust-WireGuard-OSS)

[Linux Exposing Support For Lenovo ThinkPads Being Able To Detect Hardware Damage](/news/ThinkPad-Hardware-Damage-Linux)

[Arch Linux's Main NVIDIA Driver Packages Now Using The Open Kernel Modules](/news/Arch-LInux-NVIDIA-Open-Default)

Latest Linux News

[Open-Source Linux Driver Christmas Surprise For 20~23 Year Old Radeon GPUs](/news/R300-Pop-Free-Clipping)

[LibreOffice 26.2 Gets Rid Of The "Community" Edition Branding](/news/LibreOffice-262-Drops-Community)

[Micro QuickJS Engine Compiles & Runs JavaScript With As Little As 10kB Of RAM](/news/Micro-QuickJS)

[Intel NPU Firmware Published For Panther Lake - Completing The Linux Driver Support](/news/Intel-Panther-Lake-NPU-Firmware)

[LLVM Considering An AI Tool Policy, AI Bot For Fixing Build System Breakage Proposed](/news/LLVM-AI-Tool-Policy-RFC)

[Meta Is Using The Linux Scheduler Designed For Valve's Steam Deck On Its Servers](/news/Meta-SCX-LAVD-Steam-Deck-Server)

[Intel Linux Driver Preps For Up To 13 Different Panther Lake H SoCs](/news/Intel-Panther-Lake-H-EDAC-13)

[Google Taps More Performance Out Of AMD Zen CPUs With BPF-CCX Scheduling](/news/Google-AMD-BPF-CCX)

[RADV Adds Support For New Performance Counters To Help Game Developers](/news/AMD-RADV-RGP-2.6-Counters)

[Rex: Proposed Safe Rust Kernel Extensions For The Linux Kernel, In Place Of eBPF](/news/Linux-Kernel-Rust-Rex)

Show Your Support, Go Premium

[Phoronix Premium](/phoronix-premium) allows ad-free access to the site, multi-page articles on a single page, and other features while supporting this site's continued operations.

Latest Featured Articles

[AMD Krackan Point Sub-$500 Laptop Linux Performance Improves By ~8% In Just Six Months](/review/amd-krackan-point-2025)

[Linux 6.19's Significant ~30% Performance Boost For Old AMD Radeon GPUs](/review/linux-619-amdgpu-radeon)

[Linux 6.12 To Linux 6.18 LTS Upgrade Offers Worthwhile Benefits For 5th Gen AMD EPYC](/review/linux-618-lts-amd-epyc)

[AMD Radeon RX 9000 Series vs. NVIDIA GeForce RTX 50 Open-Source Linux Performance For 2025](/review/nvk-nvidia-radeon-eoy2025)

[Intel Xeon 6980P vs. AMD EPYC 9755 128-Core Showdown With The Latest Linux Software For EOY2025](/review/xeon-6980p-epyc-9755-2025)

Support Phoronix

The mission at Phoronix since 2004 has centered around enriching the Linux hardware experience. In addition to supporting our site through advertisements, you can help by [subscribing to Phoronix Premium](/phoronix-premium). You can also contribute to Phoronix through tips/donations via [PayPal](https://www.paypal.com/donate/?hosted_button_id=EA79CCDLNFJNW) or [Stripe](https://buy.stripe.com/28o02d1yG1Lp8H67ss).

Phoronix Media

---

* [Contact](/contact)
* [Michael Larabel](/michaellarabel)

Phoronix Premium

---

[* Support Phoronix
* While Having Ad-Free Browsing,
* Single-Page Article Viewing](/phoronix-premium)

Share

---

* [Facebook](https://facebook.com/Phoronix)
* [Twitter / X](https://x.com/Phoronix)

* [Legal Disclaimer, Privacy Policy, Cookies](/legal) | Privacy Manager | [Contact](https://www.phoronix-media.com/?k=contact)
* Copyright © 2004 - 2025 by [Phoronix Media](https://www.phoronix-media.com/).
* All trademarks used are properties of their respective owners. All rights reserved.

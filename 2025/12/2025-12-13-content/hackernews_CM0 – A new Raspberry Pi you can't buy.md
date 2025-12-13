---
source: hackernews
title: CM0 – A new Raspberry Pi you can't buy
url: https://www.jeffgeerling.com/blog/2025/cm0-new-raspberry-pi-you-cant-buy
date: 2025-12-13
---

[Skip to main content](#main-content)

**[Jeff Geerling](/ "Home")**

## Main menu

* [Merch](https://www.redshirtjeff.com)
* [About](/about)
* [Blog](/blog)
* [Projects](/projects)

# CM0 - a new Raspberry Pi you can't buy

December 12, 2025

![Raspberry Pi CM0](/sites/default/files/images/raspberry-pi-cm0.jpeg)

This little postage stamp is actually a full Raspberry Pi Zero 2, complete with eMMC storage and WiFi.

But you can't get one. Well, not unless you buy the [CM0NANO development board from EDAtec](https://edatec.cn/docs/cm0nano/ds/), or you live in China.

This little guy doesn't have an HDMI port, Ethernet, or even USB. It's a special version of the 'Compute Module' line of boards. Little Raspberry Pi 'System on Modules' (SoMs), they're called.

Compute Modules are entire Linux computers about the size of a regular desktop CPU that you 'plug in' to another board, to give it life.

Compute modules are everywhere, in kiosks, signage, [3D printers](https://groupsynergy.com/synergy-group-insights/formlabs-raspberry-pi-cm4), and even the new [Ableton Move](https://www.ableton.com/en/move/). If you just need a little bit of Linux for networking and remote control, these are perfect for that.

And the CM0 is now the smallest version, a little bigger than a postage stamp.

![Raspberry Pi CM0 back - castellated edges](/sites/default/files/images/raspberry-pi-cm0-back-castellated-edges.jpeg)

But unlike all the other Compute Modules, the CM0 has castellated edges like a Pico. That way, a company integrating this into their product can just pick and place it and solder it onto their main PCB, instead of working with more delicate board-to-board connectors.

But why is this only in China? I'll get to that, but first I wanted to thank EDAtec for sending a CM0 and their CM0NANO dev board for testing. Without them, I don't think I'd ever be able to show these Pis to you.

## Video

I posted this story to my YouTube channel, but if you're on the blog already, chances are you favor reading over video, so scroll on!

## ED-CM0NANO

EDAtec's CM0NANO seems to be the official IO board for the CM0. It breaks out every feature on the [RP3A0 chip](https://www.raspberrypi.com/documentation/computers/processors.html#rp3a0) at the heart of the Pi Zero 2 and CM0.

![EDAtec CM0NANO with Pi CM0](/sites/default/files/images/raspberry-pi-cm0-edatec-cm0nano.jpeg)

There's 10/100 Ethernet through a little USB to Ethernet chip ([CoreChips SR9900A](https://www.lcsc.com/product-detail/C521278.html)), two USB 2.0 ports, full-size HDMI, and USB-C for power and flashing the eMMC. Then there are display and camera connectors, GPIO, and a few more headers.

To flash the onboard eMMC, I had to switch the `RPI_BOOT_SW` switch towards the RTC battery slot, then use [rpiboot](https://github.com/raspberrypi/usbboot) to mount it on my Mac. Then I used Raspberry Pi Imager to flash Pi OS 13 on it.

The eMMC on here is [*very* slow](https://github.com/geerlingguy/sbc-reviews/issues/98) compared to what I'm used to with the Pi 5 generation, like on the CM5. Its top speed seems to be around 19-20 MB/sec.

Once it's flashed, it's a full Linux computer, complete with Raspberry Pi's desktop environment.

EDAtec has a [firmware support package](https://edatec.cn/docs/cm0nano/um/5-installing-os/#_5-3-installing-firmware-package) you can install from their package repository, and once that's done, I did what nobody should do on this small of a computer: fired up Chromium.

Browsing the web on here is almost completely out of the question, since it only has 512 Megs of RAM—which is so little it pops a warning saying Chromium should only be used with 1 GB of more of RAM!

I did try browsing this website, and it took something like a minute to just quit the browser, after I was clicking the X to close the tab over and over again!

But with WiFi, Ethernet, USB, HDMI, and everything else the Pi ecosystem has to offer, some products that just want to slap a well-supported Linux environment on top of their product (and not integrate an SoC, memory, storage, and wireless chip) now have this.

## Global distribution possibilities

Do I think companies and makers here in the US and over in other parts of the world would also benefit from the CM0? Yes. Do I think it'll happen? Doubtful.

The Zero 2 W and CM0 share something in common, besides their entire architecture:

* The Zero 2 W was introduced at the beginning of the [COVID-induced chip shortages](https://en.wikipedia.org/wiki/2020%E2%80%932023_global_chip_shortage).
* The CM0 was introduced right before the [great RAM shortages](/blog/2025/ram-shortage-comes-us-all).

When [Hackster asked Eben Upton about global availability](https://www.hackster.io/news/raspberry-pi-unveils-the-18-compute-module-0-but-only-for-chinese-customers-for-now-913bf59ab6cc), he was noncommittal:

> No plans to make it available outside China at the moment, but we'll see how we get on.

That was back *before* the RAM shortages got bad.

![Pi Zero 2 W and CM0](/sites/default/files/images/raspberry-pi-cm0-zero-2w.jpeg)

I followed up asking a Pi engineer about it, and it sounds like one big problem is the RP3A0 chip that integrates an LPDDR2 RAM chip stacked on top of the Pi's SoC.

He said the CM0 would compete with Pi Zero 2 for LPDDR2 memory, which is in shorter supply these days (it's not being produced anymore, so stocks will only become more limited over time), and they want to make sure the popular Zero 2 W can stay in stock for makers and education.

The CM0 is targeted squarely at the lower end market, integrated into products built on assembly lines. So because of that, it's anyone's guess if the CM0 will ever make it out of China.

I'm not doing a full review of the board *here*, because:

1. It's practically the same as the Pi Zero 2 W, which [I already reviewed](/blog/2021/look-inside-raspberry-pi-zero-2-w-and-rp3a0-au).
2. It's not like you can get one (standalone, at least) anyway, at least not for the foreseeable future.

I think there was a chance, before the DRAM manufacturers went all-in on an AI cash grab, but for now, stick to the Pi Zero 2's that you're used to.

You can find a little more detail and benchmark results on my [sbc-reviews issue for the CM0](https://github.com/geerlingguy/sbc-reviews/issues/98).

## Further reading

* [The Arduino Uno Q is a weird hybrid SBC](/blog/2025/arduino-uno-q-weird-hybrid-sbc)
* [Raspberry Pi CM5 is 2-3x faster, drop-in upgrade (mostly)](/blog/2024/raspberry-pi-cm5-2-3x-faster-drop-upgrade-mostly)
* [So you want to make a Raspberry Pi killer...](/blog/2023/so-you-want-make-raspberry-pi-killer)

[raspberry pi](/tags/raspberry-pi)

[cm0](/tags/cm0)

[china](/tags/china)

[sbc](/tags/sbc)

[linux](/tags/linux)

[compute module](/tags/compute-module)

[embedded](/tags/embedded)

[iot](/tags/iot)

* [Add new comment](/comment/reply/node/3519/comment_node_blog_post#comment-form "Share your thoughts and opinions.")

Search

![Geerling Family Crest](/sites/default/files/files/geerling-family-crest.svg)
All content copyright Jeff Geerling. As an Amazon Associate I earn from qualifying purchases. [Top of page](#top).

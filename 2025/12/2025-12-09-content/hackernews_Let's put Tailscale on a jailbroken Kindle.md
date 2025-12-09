---
source: hackernews
title: Let's put Tailscale on a jailbroken Kindle
url: https://tailscale.com/blog/tailscale-jailbroken-kindle
date: 2025-12-09
---

[Simpler, smarter, and more connected after Fall Update Week 2025.Here's what you missed.](https://tailscale.com/fall-update-week-25/?utm_source=announcement-bar&utm_campaign=fall-update-2025)

Product

Solutions

[Enterprise](/enterprise)[Customers](/customers)[Docs](/kb/1017/install)[Blog](/blog)[Pricing](/pricing)

[Download](/download)[Log in](https://login.tailscale.com/welcome)[Schedule a demo](/contact/sales)[Get started - it's free!](https://login.tailscale.com/start)

Product

Meet Tailscale

* [How it works](/blog/how-tailscale-works)
* [Why Tailscale](/why-tailscale)
* [WireGuard® for Enterprises](/wireguard-vpn)
* [Bring Tailscale to Work](/bring-tailscale-to-work)

Explore

* [Integrations](/integrations)
* [Features](/features)
* [Compare Tailscale](/compare)
* [Community Projects](/community/community-projects)
* [Partnerships](/partnerships)

Solutions

By use-case

* [Business VPN](/use-cases/business-vpn)
* [CI/CD](/use-cases/ci-cd)
* [Infra Access](/use-cases/infrastructure-access)
* [Cloud Connectivity](/use-cases/cloud-connectivity)
* [Zero Trust Networking](/use-cases/zero-trust-networking)
* [Homelab](/use-cases/homelab)

By role

* [DevOps](/solutions/devops)
* [IT](/solutions/it)
* [Security](/solutions/security)

[Enterprise](/enterprise)

[Customers](/customers)

Nav heading here

* [![Alt text ](https://cdn.sanity.io/images/w77i7m8x/production/a06dc612b1e3e4f4df53a72030002600639a8738-300x120.png?w=640&q=75&fit=clip&auto=format)

  Title here

  How Cribl Enables Secure Work From Anywhere with Tailscale](https://tailscale.com/customers)

[Docs](/kb/1017/install)

[Blog](/blog)

[Pricing](/pricing)

[Download](/download)

[Schedule a demo](/contact/sales)

[Get started - it's free!](https://login.tailscale.com/start)[Log in](https://login.tailscale.com/welcome)

WireGuard is a registered trademark of Jason A. Donenfeld.

[Terms of Service](/terms)[Privacy Policy](/privacy-policy)[California Notice](https://tailscale.com/privacy-policy#california-notice)[Cookie Notice](https://tailscale.com/cookie-notice)

© 2025 Tailscale Inc. All rights reserved. Tailscale is a registered trademark of Tailscale Inc.

[Blog](/blog)|insightsDecember 01, 2025

# Let's put Tailscale on a jailbroken Kindle

![A Kindle, turned slightly sideways and positioned upright, with a prominent "Tailscale" logo/screensaver showing in the middle of its screen. At the top is a quote: "If everything means something else, then so does technology."—Fredric Jameson, (1934-2024)](https://cdn.sanity.io/images/w77i7m8x/production/8f9f92f8407aaaca9e38ddefb5aad7d8e789c499-1536x600.png?w=3840&q=75&fit=clip&auto=format)

“It’s a rite of passage to run Tailscale on weird devices.”

So writes Mitanshu Sukhwani [on his blog](https://blog.papermatch.me/html/Tailscale_on_Kindle), detailing the steps for getting Tailscale onto a [jailbroken Kindle](https://kindlemodding.org/). Getting there, and seeing a kindle entry with a satisfying green dot in your [Tailscale admin console](https://login.tailscale.com/start/?utm_source=blog&utm_medium=content&utm_campaign=jailbreak-kindle), takes some doing. But take the trip, and you’ll end up with an e-reader that can run some neat unofficial apps, and is more open to third-party and DRM-free ebooks. And with a Tailscale connection, it’s easier to connect to files and a command line on your underpowered little Linux slab.

“For me, it's the freedom of being able to do anything with the device I own,” Sukhwani writes by email. “What I can do with the freedom is a different story.”

![Close-up of a Kindle, with a Tailscale logo across its screen, and quote from Fredric Jameson on top: "If everything means something else, then so does technology."](https://cdn.sanity.io/images/w77i7m8x/production/7bbe7afbd47ed73302e8e97c51311c31406e10d5-1000x750.png?w=2048&q=75&fit=clip&auto=format)

A jailbroken Kindle allows you to set a custom screensaver inside KOReader—even transparent, if you like. Corporate logos are optional.

## [What is a jailbroken Kindle, exactly?](#what-is-a-jailbroken-kindle-exactly)

[Jailbreaking](https://en.wikipedia.org/wiki/Privilege_escalation#Jailbreaking) refers to removing the software restrictions on a device put there by its maker. Getting around these restrictions, typically by gaining “root” or administrative access, allows for accessing operating system internals, running unapproved software, and generally doing more things than a manufacturer intended. With the Kindle, you still get the standard Kindle reading experience, including Amazon's store and the ability to send the Kindle books from apps like Libby. You just add many more options, too.

The term gained purchase after the first iPhone’s debut in mid-2007; since then, nearly every device with a restricted environment has gained its own jailbreaking scene, including Kindles (debuting five months after the iPhone).

Kindle jailbreaks come along every so often. Right now, an unlocking scheme based on Amazon’s own lockscreen ads, “[AdBreak](https://kindlemodding.org/jailbreaking/AdBreak/),” is available for all but the most up-to-date Kindles (earlier than firmware version 5.18.5.0.2). I know this because I wrote this paragraph and the next on my 11th-generation Kindle, using the open-source Textadept editor, a [Bluetooth keyboard](https://www.mobileread.com/forums/showthread.php?t=369712&highlight=bluetooth+keyboard), and [Tailscale](https://login.tailscale.com/start/?utm_source=blog&utm_medium=content&utm_campaign=jailbreak-kindle) to move this draft file around.

One paragraph doesn’t seem that impressive until you consider that on a standard Kindle, you cannot do any of that. Transferring files by SSH, or Taildrop, is certainly not allowed. And that’s in addition to other upgrades you can get by jailbreaking a Kindle, including the feature-rich, customizable e-reader [KOReader](https://github.com/koreader/koreader), and lots of little apps available in repositories like [KindleForge](https://github.com/KindleTweaks/KindleForge).

If your Kindle has been connected to Wi-Fi all this time (as of early December 2025), it may have automatically updated itself and no longer be ready for jailbreaking. If you think it still has a chance, immediately put it into airplane mode and follow along.

Obligatory notice here: You’re running a risk of bricking your device (having it become unresponsive and unrecoverable) and voiding your warranty when you do this. That having been noted, let's dig further.

![Close-up of a Kindle screen, showing the "/Tailscale" menu in large buttons: "Start Tailscaled," "Start Tailscale," "Stop Tailscaled," "Stop Tailscale," "Receive Taildrop Files," and "/" (which is end or "go back").](https://cdn.sanity.io/images/w77i7m8x/production/34241c0f5bf6796f4d1da3189858d70826a4e01a-1000x1000.png?w=2048&q=75&fit=clip&auto=format)

It's not exactly a Liquid Glass interface, but it enables some neat tricks.

## [What Tailscale adds to a jailbroken Kindle](#what-tailscale-adds-to-a-jailbroken-kindle)

Tailscale isn’t necessary on a jailbroken Kindle, but it really helps. Here are some of the ways Tailscale makes messing about with an opened-up Kindle more fun:

* A persistent IP address ([100.xx](http://100.xx).yyy.zzz), just like any other Tailscale device, instead of having to remember yet another 192.168.random.number
* Easier SSH access with [magicDNS](https://tailscale.com/kb/1081/magicdns/?utm_source=blog&utm_medium=content&utm_campaign=jailbreak-kindle): ssh root@kindle and you’re in
* [Taildrop](https://tailscale.com/kb/1106/taildrop/?utm_source=blog&utm_medium=content&utm_campaign=jailbreak-kindle) for sending files to whatever Kindle directory you want
* Setting up a self-hosted Calibre Web library with Tailscale, then securely grabbing books from it anywhere with KOReader.

Key to the Kindle-plus-Tailscale experience is an easier way (SSH and Taildrop) to get epub, mobi, and other e-book and document formats into the /documents folder, ready for your KOReader sessions. Tailscale also helps with setting up some of the key jailbreak apps, saving you from plugging and unplugging the Kindle into a computer via USB cord (and then finding a second USB cord, because the first one never works, for some reason).

## [Getting your Kindle ready](#getting-your-kindle-ready)

What follows is by no means a comprehensive guide to jailbreaking and accessing your Kindle. You will want to read the documentation for each tool and app closely. Pay particular attention to which Kindle you have, which version number of the Kindle firmware it’s running, and how much space you have left on that device.

The first step is to check your Kindle’s version number (Settings > Device info) and see if there is a jailbreak method available for it. The Kindle Modding Wiki is the jailbreaking community’s go-to resource. As of this writing, there is a “WinterBreak” process available for Kindles running firmware below 15.18.1, and AdBreak is available for firmwares from 15.18.1 through 5.18.5.0.1.

If your Kindle’s version number fits one of those ranges, put it in Airplane mode and move on. If not, you’re going to have to wait until the next jailbreak method comes along.

Dammit Jeff's video on the latest (as of late October) jailbreak provides both a good overview and detailed tips on setting up a jailbroken Kindle.

## [The actual jailbreaking part](#the-actual-jailbreaking-part)

Before you dive in, have a computer (PC, Mac, or Linux) and USB cable that works with your Kindle handy. Have your Kindle on reliable Wi-Fi, like your home network—but don’t take your Kindle off airplane mode if you’ve been keeping it that way.

* [Follow these steps to jailbreak your Kindle](https://kindlemodding.org/jailbreaking/index.html). The techniques are different, but you may need to do some other tasks, like enable advertisements, or [fill your Kindle with junk files](https://kindlemodding.org/jailbreaking/prevent-auto-update/) to prevent automatic updates midway through the process.
* [Install a hotfix](https://kindlemodding.org/jailbreaking/post-jailbreak/setting-up-a-hotfix/) and [disable over-the-air updates](https://kindlemodding.org/jailbreaking/post-jailbreak/disable-ota.html) so that you can keep your Kindle on Wi-Fi and not have its jailbreak undone
* [Install](https://kindlemodding.org/jailbreaking/post-jailbreak/installing-kual-mrpi/) the Kindle Unified Application Launcher (KUAL) and MRPI (MobileRead Package Installer). KUAL is vital to installing most jailbroken apps, including Tailscale.
* You will almost certainly want to [install KOReader](https://kindlemodding.org/jailbreaking/post-jailbreak/koreader.html), too.

Those bits above are standard jailbreaking procedures. If you want Tailscale on your Kindle, you’ll go a bit further.

## [Adding Tailscale to a jailbroken Kindle](#adding-tailscale-to-a-jailbroken-kindle)

Make sure you have KUAL and MRPI installed and working. Next up: install this [“simple” version of USBNetworking for Kindle](https://github.com/notmarek/kindle-usbnetlite)**.**

Before you go further, you’ll want to choose between [Mitanshu’s “standard” Tailscale repository](https://github.com/mitanshu7/tailscale_kual), or the fork of it that [enables Taildrop](https://github.com/jonaolden/tailscale_kual). I recommend the Taildrop-enabled fork; if it goes wrong, or stops being updated, it’s fairly easy (relative to doing this kind of project) to wipe it and go back to Mitanshu’s “vanilla” version.Either way, you’ll want to get USB access to your Kindle for this next part. If you toggled on USBNetworking to try it out, toggle it off; you can’t get USB access while it’s running, as its name somewhat implies.

1. Download the Tailscale/KUAL repository of your choice using git clone or download a ZIP from the **Code** button on GitHub
2. Head to Tailscale’s page of static Linux binaries and grab the latest arm (not arm64) release
3. Copy the tailscale and tailscaled binaries from the Tailscale download and place them into the /extensions/tailscale/bin directory of the KUAL/Kindle repository you’ll be copying over
4. Head to your Tailscale admin console and [generate an authentication key](https://login.tailscale.com/admin/settings/keys). Name it something like kindle; you’ll want to enable the “Reusable” and “Pre-approved” options. Copy the key that is generated.
5. Open the file extensions/tailscale/config/auth\_key.txt for editing while it is on your (non-Kindle) computer. Paste in the key text you generated.
6. If you’re using the variant with Taildrop, you can set a custom directory in which to deliver Taildrop files by editing extensions/tailscale/config/taildrop\_dir.txt; setting /mnt/us/documents makes sense if you’re mostly sending yourself things to read in KOReader.
7. Head into the extensions folder on your computer and copy the tailscale folder you’ve set up into the extensions folder on your Kindle.

With all that done, open up KUAL on your Kindle. Go into USBNetLite and click **USBNetwork Status** to ensure it is enabled (tap the **Toggle** button if not). Go back (with the **“/”** button at the bottom), tap **Tailscale**, and first tap **Start Tailscaled** (note the “d” at the end). Wait about 10 seconds to give [the Tailscaled daemon](https://tailscale.com/kb/1278/tailscaled/?utm_source=blog&utm_medium=content&utm_campaign=jailbreak-kindle) time to start, then tap **Start Tailscale**.

If everything is settled, you should be able to see your Kindle as connected on your Tailscale admin console. Once you’ve finished smiling to yourself, click the three dots on the right-hand side of the Kindle row and select “Disable key expiry.” In most situations, you’re better off not having to patch a new key value into a Kindle text file every few months.

![Screenshot of a sharing intent on an iPhone, titled "Send via Tailscale," with a My Devices list showing "Kindle" and "pi3b" (as linux devices) with green dots, signifying availability.](https://cdn.sanity.io/images/w77i7m8x/production/a978381cca9548c99b5a7eedf9ec51d00fe6c8af-1179x1179.png?w=3840&q=75&fit=clip&auto=format)

Turn on your Kindle and send it books from any device.

## [Enjoy your (slightly) less wonky Kindle](#enjoy-your-slightly-less-wonky-kindle)

With Tailscale installed, it’s easier to get into your Kindle via SSH for file management and installing and configuring other apps. Getting a Bluetooth keyboard to work via the Kindle’s quirky command-line Bluetooth interface would not have been fun using a touchscreen keyboard.

Because the Kindle is on your tailnet, it can access anything else you have hosted there. Kindles set up this way can use tools like the [Shortcut Browser](https://github.com/mitchellurgero/kindle-shortcut-browser) to become dashboards for [Home Assistant](https://tailscale.com/blog/remotely-access-home-assistant), or access a self-hosted [Calibre-Web](https://github.com/janeczku/calibre-web) e-book server (with [some tweaking](https://github.com/mitanshu7/tailscale_kual/issues/2#issuecomment-2710486540)).

Having Taildrop handy, and having it drop files directly into the documents folder, is probably my favorite upgrade. I was on my phone, at a train station, when I came across Annalee Newitz’s [*Automatic Noodle* at](https://bookshop.org/p/books/automatic-noodle-annalee-newitz/625018d0518991aa) [Bookshop.org](http://bookshop.org). I bought it on my phone and downloaded the DRM-free epub file. When I got home, I opened and unlocked my Kindle, sent the epub to the Kindle via Taildrop, then tapped **Receive Taildrop Files** in the Tailscale app inside KUAL. Epubs, PDFs, comic book archives, DjVu files—they’re all ready to be dropped in.

---

If you’ve gotten Tailscale to run on weird (or just uncommon) devices, we’d more than love to hear about it. Let us know on [Reddit](https://www.reddit.com/r/Tailscale/), [Discord](https://discord.com/invite/tailscale), [Bluesky](https://bsky.app/profile/tailscale.com), [Mastodon](https://hachyderm.io/%40tailscale), or [LinkedIn](https://www.linkedin.com/company/tailscale/product/).

Share

Author

![Headshot of Kevin Purdy](https://cdn.sanity.io/images/w77i7m8x/production/1b9caf64d6989e00c3bf4ae1aff20b57ae125f6b-512x512.jpg?w=1080&q=75&fit=clip&auto=format)Kevin Purdy

Author

![Headshot of Kevin Purdy](https://cdn.sanity.io/images/w77i7m8x/production/1b9caf64d6989e00c3bf4ae1aff20b57ae125f6b-512x512.jpg?w=1080&q=75&fit=clip&auto=format)Kevin Purdy

Share

Loading...

## Try Tailscale for free

[Get started](https://login.tailscale.com/start)

Schedule a demo

[Contact sales](/contact/sales)

![cta phone](https://cdn.sanity.io/images/w77i7m8x/production/b715b4ca5e2577da60f0d529a4a9bc2ad4cadf59-362x567.svg?w=750&q=75&fit=clip&auto=format)

![mercury](https://cdn.sanity.io/images/w77i7m8x/production/459a7a8492910eeb22f22bb8d4c0f864b0bae25f-199x81.svg?w=640&q=75&fit=clip&auto=format)

![instacrt](https://cdn.sanity.io/images/w77i7m8x/production/7d127f4bb62a408b056328349f291857df6251b3-199x81.svg?w=640&q=75&fit=clip&auto=format)

![Retool](https://cdn.sanity.io/images/w77i7m8x/production/e9579b00087d7896e9cb750f4eb39f2c11ed11b8-199x82.svg?w=640&q=75&fit=clip&auto=format)

![duolingo](https://cdn.sanity.io/images/w77i7m8x/production/7958bf3d43a30e661ca74cf0510f250d9b99ecef-199x81.svg?w=640&q=75&fit=clip&auto=format)

![Hugging Face](https://cdn.sanity.io/images/w77i7m8x/production/68e2e5024898bcd6f6d142e0306dc7564787e1d7-199x82.svg?w=640&q=75&fit=clip&auto=format)

Product

[How it works](/blog/how-tailscale-works)[Pricing](/pricing)[Integrations](/integrations)[Features](/features)[Compare Tailscale](/compare)

Use Cases

[Business VPN](/use-cases/business-vpn)[CI/CD](/use-cases/ci-cd)[Infra Access](/use-cases/infrastructure-access)[Cloud Connectivity](/use-cases/cloud-connectivity)[Zero Trust Networking](/use-cases/zero-trust-networking)[Homelab](/use-cases/homelab)

Resources

[Blog](/blog)[Events & Webinars](/events-webinars)[Partnerships](/partnerships)

Company

[Company](/company)[Careers](/careers)[Press](/press)

Help & Support

[Support](/contact/support)[Sales](/contact/sales)[Security](/security)[Legal](/legal)[Open Source](/opensource)[Changelog](/changelog)[Tailscale Status](https://status.tailscale.com/)

Learn

[SSH keys](/learn/generate-ssh-keys)[Docker SSH](/learn/ssh-into-docker-container)[NAT Traversal](/blog/how-nat-traversal-works)[MagicDNS](/blog/2021-09-private-dns-with-magicdns)[PAM](/learn/privileged-access-management)[All articles](/learn)

[Terms of Service](/terms)[Privacy Policy](/privacy-policy)[California Notice](https://tailscale.com/privacy-policy#california-notice)[Cookie Notice](https://tailscale.com/cookie-notice)![Check mark and x on a white and blue pill button](https://cdn.sanity.io/images/w77i7m8x/production/07d853f507039b2489d9818cb6ee7442c1b60e2a-30x14.svg)Your Privacy Choices

WireGuard is a registered trademark of Jason A. Donenfeld.

© 2025 Tailscale Inc. All rights reserved. Tailscale is a registered trademark of Tailscale Inc.

---
source: hackernews
title: Show HN: HCB Mobile – financial app built by 17 y/o, processing $6M/month
url: https://hackclub.com/fiscal-sponsorship/mobile/
date: 2025-12-06
---

[Clubs](/clubs/)[Fiscal Sponsorship](/fiscal-sponsorship/)[Hackathons](/hackathons/)[Community](/slack)[Scrapbook](https://scrapbook.hackclub.com/)[Toolbox](https://toolbox.hackclub.com/)[Donors](/philanthropy/)

[Clubs](/clubs/)[Fiscal Sponsorship](/fiscal-sponsorship/)[Hackathons](/hackathons/)[Community](/slack)[Scrapbook](https://scrapbook.hackclub.com/)[Toolbox](https://toolbox.hackclub.com/)[Donors](/philanthropy/)

# [HCB](/fiscal-sponsorship/) Mobile is here!

## Manage your HCB organizations on the go. Issue cards, view transactions, and more!

![Mohamad](https://github.com/thedev132.png)Mohamad Mortada

Dec 2, 2025

I’m Mohamad, a 17-year-old from the SF Bay Area, and I just shipped the official mobile app for HCB.

If you haven't heard of it, HCB is the financial backbone for over **6,500 teenager-led nonprofits**, clubs, and hackathons. We provide **501(c)(3) nonprofit** status, access to a bank account, a donation collection platform, and debit cards for thousands of young people trying to do good in their communities.

HCB is currently processing an average of **$6 million per month** (over $80M in its lifetime).[1](#fn-1) For the last year, I’ve led the project to build the first-ever mobile app for this entire community.

*The entire project is open source on [GitHub](https://github.com/hackclub/hcb-mobile) (we'd love a ⭐️!).*

## Why build this?

These teenagers are running run robotics teams, hackathons, and nonprofit projects that improve their community. They need a way to manage their organization's finances from their pocket.
With HCB Mobile, they'll be able to:

* Track their **organization's balance** and transactions on the go.
* Accept in-person **tap-to-pay donations**, perfect for an in-person fundraiser or event! No extra hardware required. Just tap any credit/debit card against your phone.
* **Issue new debit cards**, add them to **Apple / Google Wallet**, and freeze or cancel them directly from their phone.
* **Upload receipts** directly from their device or match existing receipts in Receipt Bin to transactions with a tap.

## The Technical Stuff

When I started working on this app, I wanted to build in native code like **SwiftUI** for iOS and **Kotlin/Jetpack Compose** for Android. However, I realized that it would be a pain for me, a **full-time student** with classes, to handle two codebases. I'd have to duplicate every feature I created for one OS to the other and deal with all the integration issues along the way. Then, I discovered **Expo** (a React Native framework) which allowed me to write one app that worked on multiple devices. Working with Expo, I learned about creating my own Expo Modules (to bridge native code features to Typescript) and optimization methods like memoization and component recycling.

The non-code side of this app was *no joke*, either. I had to work with the Apple and Google app review teams to obtain **restricted entitlements** for features like mobile **tap-to-pay terminal provisioning** (made possible by Stripe) and **push provisioning** (which allows users to add cards to their payment wallet directly from HCB Mobile). It took several months and many back-and-forth email chains to finally get the entitlements we needed.

After over 250 hours of development work, I can say that I'm incredibly proud of HCB Mobile because it's **built by teenagers** to make it easier for teenagers like me to run nonprofit organizations and projects with HCB. Beyond teenagers, HCB also supports hundreds of adult-ran organizations such as mutual aid groups, open source projects, and community spaces.

## Download the app!

[![Download on the App Store](/fiscal-sponsorship/apple-web-badge.svg)](https://apps.apple.com/us/app/hcb-by-hack-club/id6465424810)   [![Get it on Google Play](/fiscal-sponsorship/google-play-web-badge.png)](https://play.google.com/store/apps/details?id=com.hackclub.hcb)

---

1. *This amount is excluding HQ (our own) [finances](https://hcb.hackclub.com/hq).*[↩](#fnref-1)

## Looking to start a nonprofit?

We're accepting applications! No startup fees, no minimum balance, and no long wait time.

[Learn more](/fiscal-sponsorship/)   [Apply now](https://nonprofit.new)

## Hack Club

[Philosophy](/philosophy/)[Our Team & Board](/team/)[Jobs](/jobs/)[Brand guide](/brand/)[Press Inquiries](/press/)[Donate](/philanthropy/)

## Resources

[Community Events](https://events.hackclub.com/)[Jams](https://jams.hackclub.com/)[Toolbox](https://toolbox.hackclub.com/)[Clubs Map](https://hackclub.com/map)[Code of Conduct](https://hackclub.com/conduct/)[Privacy & Terms](https://hackclub.com/privacy/)

1-855-625-HACK
(call toll-free)

© 2025 Hack Club. 501(c)(3) nonprofit (EIN: 81-2908499)

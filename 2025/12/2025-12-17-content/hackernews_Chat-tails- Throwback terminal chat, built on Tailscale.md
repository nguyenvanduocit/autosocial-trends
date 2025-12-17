---
source: hackernews
title: Chat-tails: Throwback terminal chat, built on Tailscale
url: https://tailscale.com/blog/chat-tails-terminal-chat
date: 2025-12-17
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

[Blog](/blog)|communityDecember 16, 2025

# Chat-tails: Throwback terminal chat, built on Tailscale

![A terminal window, open in a Mac-like screen and purple background, with a prompt entered: "nc chat.dragon-goose.ts.net 2323," and then below a purple and green text interface: "Welcome to Tailscale Terminal Chat | Please enter your nickname", with "chatty" entered.](https://cdn.sanity.io/images/w77i7m8x/production/42dd7d21cde22ae469fdb9d6d09a51c5eb0447b4-2040x1008.png?w=3840&q=75&fit=clip&auto=format)

To find a safe space for his kid to chat with friends while playing Minecraft, Brian Scott had to go back to the future.

The chat went back, that is, to an IRC-like interface, run through a terminal. The connection and setup remain futuristic, because Scott used [Tailscale](https://login.tailscale.com/start/?utm_source=blog&utm_medium=content&utm_campaign=chat-tails), and [tsnet](https://tailscale.com/kb/1244/tsnet/?utm_source=blog&utm_medium=content&utm_campaign=chat-tails), to build [chat-tails](https://github.com/bscott/chat-tails).

Chat-tails is the opposite of everything modern chat apps are offering. Nobody can get in without someone doing some work to invite them. All the chats are ephemeral, stored nowhere easy to reach, unsearchable. There are no voice chats, plug-ins, avatars, or images at all, really, unless you count [ASCII art](https://en.wikipedia.org/wiki/ASCII_art). And that’s just the way Brian wanted it.

“It’s about, hey, you have this private space, across your friends’ tailnets, where you can chat, about the game you’re playing or whatever you’re doing,” Scott told me. “It’s supposed to be more like the days where you were all on the same LAN, you would bring your computers together and have a gaming session. Now you can kind of have that same type of feeling, no matter where you are in the world—just a nice private area where you can chat.”

## [How it works](#how-it-works)

There are two ways of running chat-tails: “Regular mode” and “Tailscale mode.” In Regular Mode, you run `./chat-server`, feed it a port number, room name, and maximum number of users, and then it creates the chat, on your local network. You can log into your router and enable a port forward, if you want, every time you create the chat and want to let others in—but opening up a telnet-style chat port on your home router, the one you’re having your kids chat on, seems like a pretty bad idea. Your mileage may vary.

In “Tailscale Mode,” you do all the same things, except you provide two more things. One is a `--hostname`, which makes the chat accessible (to Tailscale users with whom you’ve shared this chat) at your Tailscale domain, like `hostname.something.ts.net`. The other thing you give it is an [auth key](https://tailscale.com/kb/1085/auth-keys/?utm_source=blog&utm_medium=content&utm_campaign=chat-tails), connecting it to Tailscale. With that, any device on the tailnet, or shared into it, can access the chat through an `nc` or `telnet` command, like `telnet hostname.something.ts.net 2323`.

And then you are chatting, in a terminal. Type some text, hit enter, and everyone sees it. There are four other commands, as of this writing: `/who` lists the users, `/help` shows you these four commands, `/me` gives your text the italicized “action” flavor (“reaches for an ice-cold Diet Coke”), and `/quit`, it quits. That’s the app, and while it might pick up some features over time (it added history options just recently), it’s doing just what it should right now.

![A chat window, showing a list of users with colored names (Kevin, Other_Kevin, Devon), a conversation ("You both are Kevins?" "I'm an Other_Kevin" "* Kevin considers this statement"), and then the list of commands.](https://cdn.sanity.io/images/w77i7m8x/production/31674a9600880adf926cece139c2f0f631a350a6-1090x1202.png?w=3840&q=75&fit=clip&auto=format)

## [Building an old-school chat space](#building-an-old-school-chat-space)

Scott is not a full-time code-writing developer, but has about 10 years’ experience working with Go. He had been eyeing the tsnet library for some time, thinking of projects that might fit a melding of Go and Tailscale. When his chat inspiration (chatspiration?) struck, he spent “about two days” learning and tinkering with the library for the first big effort.

“The tsnet (library) was actually the easiest thing,” Brian said. With the networking and verification pieces sorted, he just had to focus on the surprisingly hard task of getting text that one person types in a terminal to show up as text that another terminal user reads. “If you’re thinking of building something like Discord, you would incorporate some kinds of websocket communication, streamline everything across the wire. But for a terminal-based chat app, it’s really just TCP and UDP, just straight-up connections you’re actually dealing with.”

Making the chat look nicer than just a terminal line was helped along by bubbletea, a free and open-source terminal UI library. “While I was making this thing very minimal, I wanted to also make it very pleasing,” Brian said.

Anyone with experience building in Go could extend it or modify it, Brian said. He has looked at Go libraries for things like rendering images in terminal chat, and thought about how [Taildrop](https://tailscale.com/kb/1106/taildrop/?utm_source=blog&utm_medium=content&utm_campaign=chat-tails) could be used in a chat where everybody’s using Tailscale. Chat-tails is low-profile enough to easily run on a Raspberry Pi or similarly single-board computer (SBC); it might be leveraged as a portable, ephemeral chat to bring to events. Or it could just become a way for groups with a certain retro bent to replace their personal Slack or Discord setups.

But for now, it’s a fun way to offer his child and friends a safe learning space.

“You launch it on top of Tailscale, scale it as big as you want, and now your community is not only learning about VPN technology, but also the basics of SSH, terminal, things like that,” Brian said. “It feels good, very retro-futuristic, and fun.”

## [More community apps like this](#more-community-apps-like-this)

Brian’s chat-tails is [included](https://tailscale.com/community/community-projects/chat-tails) in our [community projects hub](https://tailscale.com/community/community-projects). Built something neat with Tailscale? Submit it by email community@tailscale.com.

If you’re enjoying chat-tails, or other community projects, we’ve got a whole channel for that in our Discord, [#community-projects](https://discord.com/channels/1379528469859532931/1388174344097628252). We’re also listening and sharing great projects on [Reddit](https://www.reddit.com/r/Tailscale/), [Bluesky](https://bsky.app/profile/tailscale.com), [Mastodon](https://hachyderm.io/%40tailscale), and [LinkedIn](https://www.linkedin.com/company/tailscale/product/).

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

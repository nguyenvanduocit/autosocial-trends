---
source: hackernews
title: There Is No Future for Online Safety Without Privacy and Security
url: https://itsfoss.com/news/alexander-linton-interview/
date: 2025-12-23
---

[![It's FOSS](/content/images/size/w30/2025/12/if-christmas-logo-favicon.webp)](https://itsfoss.com)

* [🧩 Quizzes & Puzzles](https://itsfoss.com/quiz/)
* [🎒 Resources](https://itsfoss.com/resources/)
* [📬 Newsletter](https://itsfoss.com/newsletter/)
* [▶️ YouTube](https://www.youtube.com/%40itsfoss)
* [🏘️ Community](https://itsfoss.community/)
* [📖 About](https://itsfoss.com/about/)
* [- Contact Us](https://itsfoss.com/contact-us/)
* [- Policies](https://itsfoss.com/policies/)
* [👤 Members Sign In](https://itsfoss.com/signin/)

* [About](https://itsfoss.com/about/)
* [🎁 Donate](https://ko-fi.com/itsfoss)
* [Privacy Policy](https://itsfoss.com/privacy-policy/)
* [Linux Server Stuff](https://linuxhandbook.com/)

[Join 33K+ Members](/membership/)

[Sign up](/signup/)
[Sign in](/signin/)

* [🧩 Quizzes & Puzzles](https://itsfoss.com/quiz/)
* [🎒 Resources](https://itsfoss.com/resources/)
* [📬 Newsletter](https://itsfoss.com/newsletter/)
* [▶️ YouTube](https://www.youtube.com/%40itsfoss)
* [🏘️ Community](https://itsfoss.community/)
* [📖 About](https://itsfoss.com/about/)
* [- Contact Us](https://itsfoss.com/contact-us/)
* [- Policies](https://itsfoss.com/policies/)
* [👤 Members Sign In](https://itsfoss.com/signin/)

* [About](https://itsfoss.com/about/)
* [🎁 Donate](https://ko-fi.com/itsfoss)
* [Privacy Policy](https://itsfoss.com/privacy-policy/)
* [Linux Server Stuff](https://linuxhandbook.com/)

[Log in](/signin/)
[Subscribe](/signup/)

# There is No Future for Online Safety Without Privacy and Security

Alexander Linton of the Session Technology Foundation on building decentralized messaging and why platform-wide content moderation is impractical on encrypted platforms.

* [![Sourav Rudra](/content/images/size/w30/2024/04/Profile_Pic.jpg)](/author/sourav/ "Sourav Rudra")

[Sourav Rudra](/author/sourav/)

04 Dec 2025
6 min read

On this page

[![Warp Terminal](/assets/images/warp.webp)](https://www.warp.dev?utm_source=its_foss&utm_medium=display&utm_campaign=linux_launch)

[Session](https://getsession.org/?ref=itsfoss.com) is an open source encrypted messaging app that **requires no phone number or email address to sign up**. Instead of routing messages through centralized servers, Session uses a decentralized network of over 2,000 nodes running the onion routing protocol, similar to [Tor](https://en.wikipedia.org/wiki/Tor_%28network%29?ref=itsfoss.com), ensuring that no single server knows both the message origin and destination.

The Session Technology Foundation [took over stewardship of Session](https://getsession.org/blog/introducing-the-session-technology-foundation?ref=itsfoss.com) back in October 2024, succeeding the Australia-based Oxen Privacy Tech Foundation (OPTF).

**The transition wasn't purely administrative**; it was triggered by [Australian authorities' probes into Session's operations](https://www.theguardian.com/australia-news/2024/nov/05/session-encrypted-messaging-app-developer-moves-out-of-australia-police-visit-switzerland?ref=itsfoss.com) and the threat of anti-encryption laws that could compel backdoors.

[Alexander Linton](https://www.linkedin.com/in/alex-b-linton/?ref=itsfoss.com), who worked as a journalist before joining the Session project, now serves as President of the Session Technology Foundation.

In an email interview, we discussed his transition from journalism to privacy advocacy, Session's approach to trust and safety without centralized moderation, and the threats that surround encrypted communication.

### How Did You Get from Being a Journalist into Leading the Session Technology Foundation?

When I was working in a newsroom, it became very clear to me, both from my own experience and from observing my peers, that **there was a real gap when it came to secure communication**. Journalists handle sensitive information every day, and yet the tools available to us were never built with our safety or our sources’ safety in mind. You could feel that vulnerability.

So when I heard there was a team in my hometown building a secure messaging tool, I knew I had to be involved. I joined the project seven years ago with the simple belief that people deserve the ability to communicate without surveillance or unnecessary exposure.

Over the years, I applied myself in every way I could, learning from the team, contributing wherever I added value, and helping shape Session into what it has become.

Leading the Session Technology Foundation today feels like a natural continuation of that same mission: making truly private, secure communication accessible to the people who need it most. It started as a personal frustration and turned into a global responsibility, and I’m grateful for that journey every day.

### What's Been the Biggest Surprise in Session's Growth Since You Became President?

Day to day, when you’re building secure tools, it can sometimes feel like you’re working on an island. There’s a lot of noise, skepticism, and concern about people who push for real privacy. You hear so much about the pressure against secure communications and against the teams who build them, and it can feel isolating at times.

Stepping into a more public role changed that perspective for me completely. The amount of support, encouragement, and alignment coming from every corner of life has been overwhelming in the best way. It’s been **a reminder that people do care about privacy, safety, and ownership of their communication**, and they’re grateful for tools that protect those things.

The most incredible part has been hearing the individual stories of how Session has helped people in times they needed a messenger they could trust to have a conversation in safety. Those stories make everything worth it. They remind us who we’re building for and why this work is important.

### **How Has Switzerland Been as a Home for the Foundation and Have There Been Any Regulatory Issues?**

Switzerland has been a great home for the Session Technology Foundation. It’s a place that understands the value of digital rights and open source innovation, and it provides a stable environment for stewarding a global project like Session. **Being here has allowed us to focus on long-term development rather than short-term noise**.

However, even Switzerland has its concerns, [specifically with respect to the proposed changes to VUPF](https://tuta.com/blog/switzerland-surveillance-plan?ref=itsfoss.com). Like many jurisdictions, they are reviewing proposals connected to digital privacy and encryption.

This is not unique to one country; it’s happening everywhere as governments try to understand how to regulate emerging technologies. We are watching it closely.

[Switzerland plans surveillance worse than US | Tuta

Revision of Swiss surveillance law VÜPF would directly target VPN & encrypted chat and email providers based in Switzerland.

![](https://itsfoss.com/content/images/icon/logo-favicon.svg)TutaHanna

![](https://itsfoss.com/content/images/thumbnail/switzerland-surveillance.Cz3-YFVp_1AEDhr.png)](https://tuta.com/blog/switzerland-surveillance-plan?ref=itsfoss.com)

### What's the Relationship Between Session Technology Foundation and OPTF Now That You are Independent?

The two organizations are now entirely independent. The [Session Technology Foundation](https://session.foundation/?ref=itsfoss.com) is the steward of Session; it manages the open source repositories, handles app publishing, and provides development support to contributors across the ecosystem.

[OPTF](https://optf.ngo/?ref=itsfoss.com)’s role today is mostly historical. It **played a meaningful part in Session’s early years**, and it continues to be a supporter of digital privacy more broadly.

### How Does the Foundation Aim to Close the User Base Gap to the Likes of Signal and Telegram?

[Signal](https://signal.org/?ref=itsfoss.com) and [Telegram](https://telegram.org/?ref=itsfoss.com) grew to their current popularity because they were able to fill a need that people have. Similarly, Session is filling a need that people have now and will continue to have in the future, the ability to communicate securely and privately without attaching their identity to a phone number.

**Communication and privacy are universal needs**, and as we continue improving the application and the overall platform experience, more people naturally choose Session because they want to communicate safely, privately, and without being turned into a data point.

For us, the focus isn’t on chasing user numbers for the sake of it. It’s on building something genuinely valuable and reliable. If we stay focused on that mission, the audience will follow. We’re already seeing that growth as awareness of privacy and metadata risks becomes more mainstream.

### How Would You Convince People That Session Token Isn't Just a Cash Grab?

[Session Token](https://token.getsession.org/?ref=itsfoss.com) is the mechanism for creating a sustainable future for Session. It is not short-term thinking — but long-term. If we want private messaging infrastructure to be owned and operated by the community rather than a company, there needs to be a secure and decentralized way to incentivize and support that infrastructure. That’s the role of Session Token.

**The purpose of Session Token is not to fill the pockets of some people**. It’s about creating an ecosystem where the public good of private messaging can be owned by the public. Instead of relying on a private enterprise to run essential communication infrastructure, we are building a model where people who support the network and contribute value are the ones who benefit from it.

[Session Token - Session Token

The future of privacy is powered by you.

![](https://itsfoss.com/content/images/icon/icon.png)Session Token

![](https://itsfoss.com/content/images/thumbnail/link_preview.png)](https://token.getsession.org/?ref=itsfoss.com)

### Session's Architecture Makes Content Moderation Nearly Impossible. How Do You Think About Trust and Safety?

Encryption is foundational to our model for trust and safety. Often, security and privacy are demonized in the conversation around online safety, but in reality they are safeguards.

There is no future for online safety without privacy and security; these are first principles.
Session is built so that people have control over their own experience.

There are user controls around message requests, participation in open communities, and contact discovery, which give people agency over who is talking to them and what can be shared with them. Session is a tool, and as there is no ‘one person’ running the platform, the STF cannot claim to be the arbiter or moderator of your specific conversation.

Instead, Session enables community-level moderation; people set norms for the spaces they participate in, and those norms are enforced locally rather than through platform-wide scanning or surveillance.

At a technical level, **there is no way to conduct full, platform-wide moderation on an encrypted platform without backdooring the encryption**. We believe that weakening encryption would ultimately make everyone less safe, not more. Our approach to trust and safety is about empowering people, strengthening privacy, and giving communities tools to protect themselves without compromising security.

### In These Polarizing Times, Is the Bigger Threat to Privacy Government Overreach or People Just Not Caring Anymore?

Part of the attack against encryption is trying to convince people not to care. If the public becomes apathetic, it becomes much easier to undermine privacy without resistance. **Apathy is not a solution**; ignoring the issue of online privacy only makes the problem worse and leaves everyone more exposed.

Government overreach is a real concern. Some proposals around the world target both the technology and the people building secure tools, often through mechanisms that could weaken encryption or introduce scanning systems. It is important to remain vigilant, specifically with respect to backdooring encryption (*such as through scanning mechanisms, i.e.,* [*chat control*](https://en.wikipedia.org/wiki/Regulation_to_Prevent_and_Combat_Child_Sexual_Abuse?ref=itsfoss.com)).

Technology itself can also be an enemy of privacy when it is designed without security in mind. The prevalence of AI, particularly when it is embedded at the operating system level, presents an existential threat to secure communication and online security in general.

---

💬 *Are you a Session user? Thinking of trying it out? Do let me know in the comments below!*

[Interview](/tag/interview/)
[News](/tag/news/)

[Share](https://twitter.com/share?text=There%20is%20No%20Future%20for%20Online%20Safety%20Without%20Privacy%20and%20Security&url=https://itsfoss.com/news/alexander-linton-interview/ "Share on X")
[Share](https://bsky.app/intent/compose?text=There%20is%20No%20Future%20for%20Online%20Safety%20Without%20Privacy%20and%20Security%20https://itsfoss.com/news/alexander-linton-interview/ "Share on Bluesky")
[Share](https://www.facebook.com/sharer.php?u=https://itsfoss.com/news/alexander-linton-interview/ "Share on Facebook")
[Share](https://www.linkedin.com/shareArticle?mini=true&url=https://itsfoss.com/news/alexander-linton-interview/&title=There%20is%20No%20Future%20for%20Online%20Safety%20Without%20Privacy%20and%20Security&summary=There%20is%20No%20Future%20for%20Online%20Safety%20Without%20Privacy%20and%20Security "Share on Linkedin")
[Email](/cdn-cgi/l/email-protection#be81cdcbdcd4dbddca83ead6dbccdb9b8c8ed7cd9b8c8ef0d19b8c8ef8cbcacbccdb9b8c8ed8d1cc9b8c8ef1d0d2d7d0db9b8c8eeddfd8dbcac79b8c8ee9d7cad6d1cbca9b8c8eeeccd7c8dfddc79b8c8edfd0da9b8c8eeddbddcbccd7cac798dcd1dac783d6cacacecd849191d7cacdd8d1cdcd90ddd1d391d0dbc9cd91dfd2dbc6dfd0dadbcc93d2d7d0cad1d093d7d0cadbccc8d7dbc99198d0dccdce85ead6dbccdb9b8c8ed7cd9b8c8ef0d19b8c8ef8cbcacbccdb9b8c8ed8d1cc9b8c8ef1d0d2d7d0db9b8c8eeddfd8dbcac79b8c8ee9d7cad6d1cbca9b8c8eeeccd7c8dfddc79b8c8edfd0da9b8c8eeddbddcbccd7cac7 "Share by email")
[Feedback](https://itsfoss.com/update-feedback/ "Feedback")

About the author

[![Sourav Rudra](/content/images/size/w30/2024/04/Profile_Pic.jpg)](/author/sourav/)

## [Sourav Rudra](/author/sourav/)

A nerd with a passion for open source software, custom PC builds, motorsports, and exploring the endless possibilities of this world.

Featured

[![tiger data logo on left, an illustration showing the text "pg_textsearch", inside a magnifying glass on the right](/content/images/size/w30/2025/12/tiger-data-pg-textsearch-open-source.png)

### Watch Out Elasticsearch! Tiger Data's PostgreSQL BM25 Search Extension Goes Open Source](https://itsfoss.com/news/tiger-data-pg-textsearch/) [![microsoft logo is shown going towards a trashcan on the left, in the middle is an administrative building with the denmark flag at top, and on the right are the heart and open source logos](/content/images/size/w30/2025/12/denmark-road-transport-authority-ditches-microsoft.png)

### Denmark Begins its Exit from Microsoft — and This is Just the Beginning](https://itsfoss.com/news/denmark-road-traffic-authority-ditches-microsoft/) [![Ubuntu ditched these projects](/content/images/size/w30/2025/12/x-projects-killed-by-ubuntu.webp)

### 7 Projects Killed by Ubuntu (But I Still Miss Them)](https://itsfoss.com/projects-ditched-by-ubuntu/) [![a light blue colored pop!_is logo is on the left with 24.04 lts written below it, on the right is the star-studded cosmic logo with a curious penguin below it](/content/images/size/w30/2025/12/pop-os-24-04-release-review-banner.png)

### Pop!\_OS 24.04 LTS Review: Is This the Linux Distro of the Year 2025?](https://itsfoss.com/news/pop-os-24-04-review/) [![there is a worried penguin with a pink hat on the left, and on the right are some illustrations that show a hike in the prices of ram due to ai](/content/images/size/w30/2025/12/ai-hunger-is-hiking-prices.png)

### AI Craze Just Made Your New PC Build Way More Expensive](https://itsfoss.com/news/ai-causes-ram-prices-skyrocket/)

Latest

[![a screenshot of elementary os 8.1 is shown in the background, with 8.1 written over it in white, and the elementary os logo being placed inside the dot between the numbers](/content/images/size/w30/2025/12/elementary-os-8-1-release-banner.png)

### Christams Comes Early With elementary OS 8.1 Release

22 Dec 2025](https://itsfoss.com/news/elementary-os-8-1-release/) [![Schedule Kanban board](/content/images/size/w30/2025/12/schedule-offline-kanban-board-app.png)

### Away from Cloud: This Local, Offline Tool is Perfect for Personal Project Management on Linux Desktop

22 Dec 2025](https://itsfoss.com/schedule-kanban-board/) [![steam winter sale 2025 is written in white, with the steam logo on left, the background is a christmas-themed illustration with some happy penguins laying about](/content/images/size/w30/2025/12/steam-winter-sale-2025-banner.png)

### 13 Awesome Games Linux Users Can Grab in Steam Winter Sale 🎮 (Ends 5 January)

22 Dec 2025](https://itsfoss.com/news/linux-games-steam-winter-sale-2025/)

#### Become a Better Linux User

With the FOSS Weekly Newsletter, you learn useful Linux tips, discover applications, explore new distros and stay updated with the latest from Linux world

Subscribe

Great! Check your inbox and click the link.

Sorry, something went wrong. Please try again.

❤️

#### Support Independent Content

We respect your choice to use an ad blocker! It's FOSS is an independent publication that relies on your support.

Consider supporting us to keep quality Linux content free for everyone.

[Become a Member](https://itsfoss.com#/portal/signup)
[Buy us a Coffee ☕](https://www.buymeacoffee.com/itsfoss)

Read next

[### Christams Comes Early With elementary OS 8.1 Release](/news/elementary-os-8-1-release/)
[### 13 Awesome Games Linux Users Can Grab in Steam Winter Sale 🎮 (Ends 5 January)](/news/linux-games-steam-winter-sale-2025/)
[### Watch Out Elasticsearch! Tiger Data's PostgreSQL BM25 Search Extension Goes Open Source](/news/tiger-data-pg-textsearch/)
[### Decentralized YouTube Alternative PeerTube Adds Creator Mode](/news/peertube-8-release/)
[### Docker Makes Enterprise-Grade Hardened Images Free for All Developers](/news/docker-hardened-images-open-sourced/)

## Become a Better Linux User

With the FOSS Weekly Newsletter, you learn useful Linux tips, discover applications, explore new distros and stay updated with the latest from Linux world

Subscribe

Great! Check your inbox and click the link.

Sorry, something went wrong. Please try again.

![itsfoss happy penguin](/assets/images/itsfoss-cta.webp)

![It's FOSS](/content/images/size/w30/2025/12/if-christmas-logo-favicon.webp)

Making You A Better Linux User

Subscribe

Great! Check your inbox and click the link.

Sorry, something went wrong. Please try again.

Navigation

* [🧩 Quizzes & Puzzles](https://itsfoss.com/quiz/)
* [🎒 Resources](https://itsfoss.com/resources/)
* [📬 Newsletter](https://itsfoss.com/newsletter/)
* [▶️ YouTube](https://www.youtube.com/%40itsfoss)
* [🏘️ Community](https://itsfoss.community/)
* [📖 About](https://itsfoss.com/about/)
* [- Contact Us](https://itsfoss.com/contact-us/)
* [- Policies](https://itsfoss.com/policies/)
* [👤 Members Sign In](https://itsfoss.com/signin/)

* [About](https://itsfoss.com/about/)
* [🎁 Donate](https://ko-fi.com/itsfoss)
* [Privacy Policy](https://itsfoss.com/privacy-policy/)
* [Linux Server Stuff](https://linuxhandbook.com/)

Resources

* [Courses 🎓](/tag/courses/)
* [Distro Resources 📖](/tag/distro-resources/)
* [Guides 📒](/tag/guides/)

Social

[Facebook](https://www.facebook.com/itsfoss)
[Twitter](https://x.com/itsfoss2)
[RSS](https://itsfoss.com/rss/)
[Bluesky](https://bsky.app/profile/itsfoss.bsky.social)
[Mastodon](https://mastodon.social/%40itsfoss)
[*Download more icon variants from https://tabler-icons.io/i/brand-github*Github](https://github.com/itsfoss)
[*Download more icon variants from https://tabler-icons.io/i/brand-instagram*Instagram](https://www.instagram.com/itsfoss/)
[Reddit](https://www.reddit.com/r/LinuxUsersGroup/)
[*Download more icon variants from https://tabler-icons.io/i/brand-telegram*Telegram](https://t.me/itsfoss_official)
[*Download more icon variants from https://tabler-icons.io/i/brand-youtube*Youtube](https://www.youtube.com/%40Itsfoss)

©2025 [It's FOSS](https://itsfoss.com).
Hosted on [Digital Ocean (get $200 in FREE credits)](https://m.do.co/c/d58840562553) & Published with [Ghost](https://ghost.org/pricing/?via=abhishek70) & [Rinne](https://brightthemes.lemonsqueezy.com/?aff=GNoD0).

System
Light
Dark

Great! You’ve successfully signed up.

Welcome back! You've successfully signed in.

You've successfully subscribed to It's FOSS.

Your link has expired.

Success! Check your email for magic link to sign-in.

Success! Your billing info has been updated.

Your billing was not updated.

Privacy Manager

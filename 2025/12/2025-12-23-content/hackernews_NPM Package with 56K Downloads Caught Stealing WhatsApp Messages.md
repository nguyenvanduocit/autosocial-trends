---
source: hackernews
title: NPM Package with 56K Downloads Caught Stealing WhatsApp Messages
url: https://www.koi.ai/blog/npm-package-with-56k-downloads-malware-stealing-whatsapp-messages
date: 2025-12-23
---

**🚨 8 Million Users' AI Conversations Sold for Profit by "Privacy" Extensions**

[Read now](https://www.koi.ai/blog/urban-vpn-browser-extension-ai-conversations-data-collection)

product

[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6833fd08b9c0fc51917041fe_platform_icon.svg)

PLATFORM

Track, govern, and enable installs across all endpoints.](/platform)[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6833fd08b9c0fc5191704209_discovery_icon.svg)

DISCOVERY

All your self-provisioned software in one place.](/discovery)[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6833fd0829c964ec1f08358c_policies_icon.svg)

PREVENTIVE POLICIES

Preventive policies that minimize marketplace risk.](/policies)[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6833fd0846cad0da74b1386c_approve_icon.svg)

EASY ALLOW

Make installing new software as easy as hitting “request”.](/approve)

[about](/about)[BLOG](/blog)[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/68dd8b40ae898e260e0dd5e5_koidex-logo%20(1).svg)](https://dex.koi.security/)[wall of koi](/wall-of-koi)[Get a Demo](/get-a-demo)[![Koi logo](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/67c8217077b742ad19cab085_koi-logo.svg)](/)[Wall of koi](/wall-of-koi)[SEE KOI IN ACTION](/chat-with-us)

[![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/68b916d6f509af522a34c3a3_Arrow%20blsck.svg)

Back](/blog)

Koi Research

### NPM Package With 56K Downloads Caught Stealing WhatsApp Messages

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/689db2bbf506d1e75461240a_Tuval.avif)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)

Tuval Admoni

,

,

December 21, 2025

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/694806d8d1a011b13249a012_image%20(17)%20copy.jpg)

[Intro

![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/689b4be459a596b5422d064e_red%20arrowsvg.svg)](#intro)

The `lotusbail` npm package presents itself as a WhatsApp Web API library - a fork of the legitimate `@whiskeysockets/baileys` package. With over 56,000 downloads and functional code that actually works as advertised, it's the kind of dependency developers install without a second thought. The package has been available on npm for 6 months and is still live at the time of writing.

Behind that working functionality: sophisticated malware that steals your WhatsApp credentials, intercepts every message, harvests your contacts, installs a persistent backdoor, and encrypts everything before sending it to the threat actor's server.

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/694807a93d3c8c57926db6ed_image%20(18)%20(1).png)

Koidex report for lotusbail package

What gets captured:

* Authentication tokens and session keys
* Complete message history (past and present)
* Full contact lists with phone numbers
* Media files and documents
* Persistent backdoor access to your WhatsApp account

## How It Works

### **The Cover Is Real**

Most malicious npm packages reveal themselves quickly - they're typosquats, they don't work, or they're obviously sketchy. This one actually functions as a WhatsApp API. It's based on the legitimate Baileys library and provides real, working functionality for sending and receiving WhatsApp messages.

Obvious malware is easy to spot. Functional malware? That gets installed, tested, approved, and deployed to production.

The social engineering here is brilliant: developers don't look for malware in code that works. They look for code that breaks.

### **The Theft and Exfiltration**

The package wraps the legitimate WebSocket client that communicates with WhatsApp. Every message that flows through your application passes through the malware's socket wrapper first.

When you authenticate, the wrapper captures your credentials. When messages arrive, it intercepts them. When you send messages, it records them. The legitimate functionality continues working normally - the malware just adds a second recipient for everything.

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/6948073a4954450257af43cd_a29cc381.png)

All your WhatsApp authentication tokens, every message sent or received, complete contact lists, media files - everything that passes through the API gets duplicated and prepared for exfiltration.

But the stolen data doesn't get sent in plain text. The malware includes a complete, custom RSA implementation for encrypting the data before transmission:

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/6948073a4954450257af43d0_d6657ad0.png)

Why implement custom RSA? Because legitimate WhatsApp libraries don't need custom encryption - WhatsApp already handles end-to-end encryption. The custom crypto exists for one reason: to encrypt stolen data before exfiltration so network monitoring won't catch it.

The exfiltration server URL is buried in encrypted configuration strings, hidden inside compressed payloads. The malware uses four layers of obfuscation: Unicode variable manipulation, LZString compression, Base-91 encoding, and AES encryption. The server location isn't hardcoded anywhere visible.

### **The Backdoor**

Here's where it gets particularly nasty. WhatsApp uses pairing codes to link new devices to accounts. You request a code, WhatsApp generates a random 8-character string, you enter it on your new device, and the devices link together.

The malware hijacks this process with a hardcoded pairing code. The code is encrypted with AES and hidden in the package:

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/6948073a4954450257af43d6_7375538c.png)

This means the threat actor has a key to your WhatsApp account. When you use this library to authenticate, you're not just linking your application - you're also linking the threat actor's device. They have complete, persistent access to your WhatsApp account, and you have no idea they're there.

The threat actor can read all your messages, send messages as you, download your media, access your contacts - full account control. And here's the critical part, uninstalling the npm package removes the malicious code, but the threat actor's device stays linked to your WhatsApp account. The pairing persists in WhatsApp's systems until you manually unlink all devices from your WhatsApp settings. Even after the package is gone, they still have access.

## They Really Didn't Want You Looking

The package includes 27 infinite loop traps that freeze execution if debugging tools are detected:

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/6948073a4954450257af43d3_354957b5.png)

These traps check for debuggers, inspect process arguments, detect sandbox environments, and generally make dynamic analysis painful. They also left helpful comments in their code marking the malicious sections - professional development practices applied to supply chain attacks. Someone probably has a Jira board for this.

## Final Thoughts

Supply chain attacks aren't slowing down - they're getting better. We're seeing working code with sophisticated anti-debugging, custom encryption, and multi-layer obfuscation that survives marketplace reviews. The `lotusbail` case isn't an outlier. It's a preview.

Traditional security doesn't catch this. Static analysis sees working WhatsApp code and approves it. Reputation systems see 56,000 downloads and trust it. The malware hides in the gap between "this code works" and "this code only does what it claims."

Catching sophisticated supply chain attacks requires behavioral analysis - watching what packages actually do at runtime. When a WhatsApp library implements custom RSA encryption and includes 27 anti-debugging traps, those are signals. But you need systems watching for them.

This writeup was authored by the research team at Koi Security. We built Koi to detect threats that pass traditional checks but exhibit malicious behavior at runtime.

[**Book a demo**](https://www.koi.ai/get-a-demo) to see how behavioral analysis catches what static review misses.

Stay safe out there.

share

Copied to clipboard

### Be the first to know

Fresh research and updates on software risk and endpoint security.

![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6836d87262e4ca8706e98a77_coud01.svg)![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/689d8cd8db647692730041cf_ship.avif)![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/6836d873d735542d459f0382_cloud02.svg)

Thanks for subscribing!

## related blogs

Koi Research

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/694128f1605610d2a6621054_image%20(15)%20copy.jpg)

### Inside GhostPoster: How a PNG Icon Infected 50,000 Firefox Users

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/68a433090a6d7347126e14b1_Koi%20Headshots-7945.avif)![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/694124b6ae70c25a69cfe103_1681024746465.jpeg)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)

Lotan Sery

,

Noga Gouldman

,

December 16, 2025

Koi Research

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/69401d23cae6d7f91eee9c2a_homebrew_koi_image.png)

### Brew Hijack: Serving Malware Over Homebrew’s Core Tap

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/68a16e204bc7bbb4ba78e0e5_Yuval.avif)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)

Yuval Ronen

,

,

December 15, 2025

Koi Research

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/6940043002b9b0e7d938b3ac_1765802302699.jpeg)

### 8 Million Users' AI Conversations Sold for Profit by "Privacy" Extensions

![](https://cdn.prod.website-files.com/689ad8c5d13f40cf59df0e0c/689ae130dd9894b4cba4a350_Idan.avif)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)![](https://cdn.prod.website-files.com/plugins/Basic/assets/placeholder.60f9b1840c.svg)

Idan Dardikman

,

,

December 15, 2025

[![Koi logo](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/67c8217077b742ad19cab085_koi-logo.svg)](/)[Get a Demo](/get-a-demo)

COMPANY

[About us →](/about)[Contact →](/get-a-demo)Careers →Contact →

[Koidex](https://dex.koi.security/)

Product

[Platform →](/platform)[Discovery →](/discovery)[Approve →](/approve)[Policies →](/policies)Careers →Contact →

Koi Security LTD
Yehuda Ha-Levi St 34, 6513616
Tel Aviv, Israel

Koi Security INC
1901 Pennsylvania Avenue NW, Suite 900
Washington D.C. 20006

[Privacy Policy →](/privacy-policy)[Terms & Conditions →](/terms-and-conditions)

![GSPR Badge](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/683eb920552463589e606129_gdpr.svg)![ico 27001](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/67cff79fcaed643331eecdea_ico.svg)![soc3 certified](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/67cff79f5f3350b9ddded599_soc.svg)

Copyright © 2025 Koi Security Inc. All rights reserved.

[Privacy](/privacy-policy)[Cookie Policy](/cookie-policy)

![](https://cdn.prod.website-files.com/67bf17e426d92bdda54af956/67cff6c93a1d08a3784351f2_footer_bottom.avif)

[

Your browser does not support the video tag.
](https://pub-0ce1625df584471a91049d4bd7bb8aac.r2.dev/pond.mp4)

---
source: hackernews
title: GotaTun – Mullvad's WireGuard Implementation in Rust
url: https://mullvad.net/en/blog/announcing-gotatun-the-future-of-wireguard-at-mullvad-vpn
date: 2025-12-20
---

[Skip to main content](#main)

[Mullvad Logo](../../en)

Menu

## Products and services

* [VPN](../../en/vpn)
* [Browser](../../en/browser)
* [Browser extension](../../en/help/mullvad-browser-extension)
* [Pricing](../../en/pricing)

## Privacy

* [Why privacy matters](../../en/why-privacy-matters)

## Technical resources

* [Downloads](../../en/download)
* [Tech blog & news](../../en/blog)
* [Open source](../../en/open-source)
* [Check for leaks](../../en/check)
* [Servers](../../en/servers)

## Help and support

* [Guides](../../en/help)
* [FAQ](../../en/help/faq)

* [VPN](../../en/vpn)
* [Browser](../../en/browser)
* [Pricing](../../en/pricing)
* [Downloads](../../en/download)
* [Why privacy matters](../../en/why-privacy-matters)

[Log in](../../en/account/login) [Get started](../../en/account/create)

# Announcing GotaTun, the future of WireGuard at Mullvad VPN

December 19, 2025 [Features](/en/blog/tag/features) [App](/en/blog/tag/app)

[GotaTun](https://github.com/mullvad/gotatun) is a WireGuard® implementation written in Rust aimed at being fast, efficient and reliable.

GotaTun is a fork of the [BoringTun](https://github.com/cloudflare/boringtun) project from Cloudflare. This is not a new protocol or connection method, just WireGuard® written in [Rust](https://rust-lang.org/). The name GotaTun is a combination of the original project, BoringTun, and [Götatunneln](https://wikipedia.org/wiki/G%C3%B6tatunneln), a physical tunnel located in Gothenburg. We have integrated privacy enhancing features like [DAITA](/vpn/daita) & [Multihop](/help/multihop-wireguard), added first-class support for Android and used Rust to achieve great performance by using safe multi-threading and [zero-copy](https://wikipedia.org/wiki/Zero-copy) memory strategies.

Last month we rolled it out to all our Android users, and we aim to ship it to the remaining platforms next year.

## Why GotaTun?

Our mobile apps have relied on wireguard-go for several years, a cross-platform userspace implementation of WireGuard® in Go. wireguard-go has been the de-facto userspace implementation of WireGuard® to this date, and many VPN providers besides Mullvad use it. Since mid-2024 we have been maintaining a fork of
wireguard-go to support features like DAITA & Multihop. While wireguard-go has served its purpose for many years it has not been without its challenges.

For Android apps distributed via the Google Play Store, Google collects crash reports and makes them available to developers. In the developer console we have seen that more than 85% of all crashes reported have stemmed from the wireguard-go. We have managed to solve some of the obscure issues over the years ([#6727](https://github.com/mullvad/mullvadvpn-app/pull/6727) and [#7728](https://github.com/mullvad/mullvadvpn-app/pull/7728) to name two examples), but many still remain. For these reasons we chose Android as the first platform to release GotaTun on, allowing us to see the impact right away.

Another challenge we have faced is interoperating Rust and Go. Currently, most of the service components of the Mullvad VPN app are written in Rust with the exception of wireguard-go. Crossing the boundary between Rust and Go is done using a [foreign function interface](https://wikipedia.org/wiki/Foreign_function_interface) (FFI), which is inherently unsafe and complex. Since Go is a managed language with its own separate runtime, how it executes is opaque to the Rust code. If wireguard-go were to hang or crash, recovering stacktraces is not always possible which makes debugging the code cumbersome. Limited visibility insight into crashes stemming from Go has made troubleshooting and long-term maintenance tedious.

### Outcome

The impact has been immediate. So far not a single crash has stemmed from GotaTun, meaning that all our old crashes from wireguard-go are now gone. Since rolling out GotaTun on Android with version 2025.10 in the end of November we’ve seen a big drop in the metric [user-perceived crash rate](https://developer.android.com/topic/performance/vitals/crash#android-vitals), from 0.40% to 0.01%, when comparing to previous releases. The feedback from users' have also been positive, with reports of better speeds and lower battery usage.

![](/media/uploads/2025/gotatun.png)

*User-perceived crash rate*

### Looking ahead

We’ve reached the first major milestone with the release of GotaTun on Android, but we have a lot more exciting things in store for 2026.

* A third-party security audit will take place early next year.
* We will replace wireguard-go with GotaTun across all platforms, including desktop and iOS.
* More effort will be put into improving performance.

We hope you are as excited as we are for 2026!

## Mullvad

* [About](/en/about)
* [Help](/en/help)
* [Servers](/en/servers)
* [Pricing](/en/pricing)
* [Blog](/en/blog)
* [Mullvad VPN](/en/vpn)
* [Mullvad Browser](/en/browser)
* [Why privacy matters](/en/why-privacy-matters)
* [Why Mullvad VPN?](/en/why-mullvad-vpn)
* [What is a VPN?](/en/vpn/what-is-vpn)
* [Downloads](/en/download)
* [Stop chat control](/en/chatcontrol)
* [Press](/en/press)
* [Careers](https://www.mullvad.careers/)

## Policies

* [Policies](/en/policies)
* [Open source](/en/open-source)
* [Privacy policy](/en/help/privacy-policy)
* [Cookies](/en/help/cookie-policy)
* [Terms of service](/en/help/terms-service)
* [Partnerships and resellers](/en/help/partnerships-and-resellers)
* [Reviews, ads and affiliates](/en/help/policy-reviews-advertising-and-affiliates)
* [Reporting a bug or vulnerability](/en/help/how-report-bug-or-vulnerability)

## Address

Mullvad VPN AB

Box 53049

400 14 Gothenburg

Sweden

* support@mullvadvpn.net
* [GPG key](/static/gpg/mullvadvpn-support-mail.asc)
* [Onion service](http://o54hon2e2vj6c7m3aqqu6uyece65by3vgoxxhlqlsvkmacw6a7m7kiad.onion)

## Follow us

* [@mullvadnet](https://www.x.com/mullvadnet)
* [@mullvadnet](https://mastodon.online/%40mullvadnet)
* [Mullvad VPN](https://www.youtube.com/c/MullvadVPNNet)
* [mullvad](https://github.com/mullvad)

## Language

English

* العربيّة
* Dansk
* Deutsch
* English
* Español
* فارسی
* Suomi
* Français
* Italiano
* 日本語
* 한국어
* Nederlands
* Norsk
* Polski
* Português
* Русский
* Svenska
* ภาษาไทย
* Türkçe
* 简体中文
* 繁體中文

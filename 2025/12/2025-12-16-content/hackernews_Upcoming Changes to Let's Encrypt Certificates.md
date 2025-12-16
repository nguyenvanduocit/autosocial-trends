---
source: hackernews
title: Upcoming Changes to Let's Encrypt Certificates
url: https://community.letsencrypt.org/t/upcoming-changes-to-let-s-encrypt-certificates/243873
date: 2025-12-16
---

[Let's Encrypt Community Support](/)

# [Upcoming Changes to Let’s Encrypt Certificates](/t/upcoming-changes-to-let-s-encrypt-certificates/243873)

[API Announcements](/c/api-announcements/18)

[mcpherrinm](https://community.letsencrypt.org/u/mcpherrinm)

December 15, 2025, 7:16pm

1

Let’s Encrypt is introducing several updates to the certificates we issue, including new root certificates, the deprecation of TLS client authentication, and shortening certificate lifetimes. To help roll out changes gradually, we’re making use of [ACME profiles](https://letsencrypt.org/2025/01/09/acme-profiles) to allow users to have control over when some of these changes take place. For most users, no action is required.

Let’s Encrypt has generated two new Root Certification Authorities (CAs) and six new Intermediate CAs, which we’re collectively calling the “Generation Y” hierarchy. These are cross-signed from our existing “Generation X” roots, X1 and X2, so will continue to work anywhere our current roots are trusted.

Most users get certificates from our default [classic](https://letsencrypt.org/docs/profiles/#classic) profile, unless they’ve opted into another profile. This profile will switch to the new Generation Y hierarchy on May 13 2026. These new intermediates do not contain the “TLS Client Authentication” Extended Key Usage due to an upcoming root program requirement. We have previously announced our plans to [end TLS Client Authentication](https://letsencrypt.org/2025/05/14/ending-tls-client-authentication) starting in February 2026, which will coincide with the switch to the Generation Y hierarchy. Users who encounter issues or need an extended period to switch can use our [tlsclient](https://letsencrypt.org/docs/profiles/#tlsclient) profile until May 2026, which will also remain on our existing Generation X roots.

If you’re requesting certificates from our [tlsserver](https://letsencrypt.org/docs/profiles/#tlsserver) or [shortlived](https://letsencrypt.org/docs/profiles/#shortlived) profiles, you’ll begin to see certificates which come from the Generation Y hierarchy this week. This switch will also mark the opt-in general availability of short-lived certificates from Let’s Encrypt, including support for IP Addresses on certificates.

We also announced our timeline to comply with upcoming changes to the [CA/Browser Forum Baseline Requirements](https://cabforum.org/working-groups/server/baseline-requirements/requirements/), which will require us to shorten the length of time our certificates are valid for. Next year, you’ll be able to opt-in to 45 day certificates for early adopters and testing via the tlsserver profile. In 2027, we’ll lower the default certificate lifetime to 64 days, and then to 45 in 2028. For the full timeline and details, please see our post on [decreasing certificate lifetimes to 45 days](https://letsencrypt.org/2025/12/02/from-90-to-45).

For most users, no action is required, but we recommend reviewing the linked blog posts announcing each of these changes for more details. If you have any questions, please do not hesitate to ask here, on this forum.

14 Likes

[When will Let's Encrypt's IP certificates be officially launched?](https://community.letsencrypt.org/t/when-will-lets-encrypts-ip-certificates-be-officially-launched/242941/80)

[RE: upcoming-changes-to-let-s-encrypt-certificates](https://community.letsencrypt.org/t/re-upcoming-changes-to-let-s-encrypt-certificates/243884)

### Related topics

| Topic |  | Replies | Views | Activity |
| --- | --- | --- | --- | --- |
| [RE: upcoming-changes-to-let-s-encrypt-certificates](https://community.letsencrypt.org/t/re-upcoming-changes-to-let-s-encrypt-certificates/243884)  [Help](/c/help/13) | 1 | 33 | December 15, 2025 |
| [New "Generation Y" Hierarchy of Root and Intermediate Certificates](https://community.letsencrypt.org/t/new-generation-y-hierarchy-of-root-and-intermediate-certificates/243289)  [API Announcements](/c/api-announcements/18) | 0 | 245 | November 24, 2025 |
| [Ending TLS Client Authentication Certificate Support in 2026](https://community.letsencrypt.org/t/ending-tls-client-authentication-certificate-support-in-2026/237403)  [API Announcements](/c/api-announcements/18) | 3 | 1772 | October 1, 2025 |
| [Deploying Let's Encrypt's New Issuance Chains](https://community.letsencrypt.org/t/deploying-lets-encrypts-new-issuance-chains/216486)  [API Announcements](/c/api-announcements/18) | 6 | 5748 | June 6, 2024 |
| [Switching issuance to new intermediates](https://community.letsencrypt.org/t/switching-issuance-to-new-intermediates/240073)  [API Announcements](/c/api-announcements/18) | 3 | 2410 | August 20, 2025 |

* [Home](/)
* [Categories](/categories)
* [Guidelines](/guidelines)
* [Terms of Service](/tos)
* [Privacy Policy](https://letsencrypt.org/privacy/)

Powered by [Discourse](https://www.discourse.org), best viewed with JavaScript enabled

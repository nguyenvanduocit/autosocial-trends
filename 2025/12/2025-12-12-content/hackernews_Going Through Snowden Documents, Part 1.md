---
source: hackernews
title: Going Through Snowden Documents, Part 1
url: https://libroot.org/posts/going-through-snowden-documents-part-1/
date: 2025-12-12
---

# [Libroot.org](/)

* [about](/about)
* [projects](/projects)
* [posts](/posts)

# Going Through Snowden Documents, Part 1

Published on 9 Dec 2025

We are building a comprehensive archive and analysis project examining published documents leaked by Edward Snowden. Our methodology involves systematically reviewing each available document with particular attention to small details and information that has received little or no public attention since the initial 2013 disclosures. Throughout this process, we will publish posts highlighting interesting previously unreported findings. The main project will hopefully be complete and made public in mid-to-late 2026.

This is Part 1 of our "Going Through Snowden Documents" series.

Document: [CNE Analysis in XKEYSCORE](https://web.archive.org/web/20181005121544/https%3A//assets.documentcloud.org/documents/2116005/cne-analysis-in-xks.pdf)

Classification: TOP SECRET//COMINT//REL TO USA, AUS, CAN, GBR, NZL

Date: October 15, 2009

Published by: The Intercept ([July 1](https://theintercept.com/2015/07/01/nsas-google-worlds-private-communications/) and [July 2, 2015](https://theintercept.com/2015/07/02/look-under-hood-xkeyscore/))

Authors: Morgan Marquis-Boire, Glenn Greenwald, and Micah Lee

While The Intercept published this document, the accompanying articles focus on NSA's XKEYSCORE system broadly and does not analyze this specific document. The document appears only in the "Documents published with this article" sections without dedicated coverage. Academic searches, news archives, and general web searches reveal virtually no subsequent analysis or citation of this document. This pattern of important documents published but never publicly analyzed is unfortunately very common in the published Snowden documents.

## Table of Contents

* [CNE operation against Chinese defense contractor Norinco](#Norinco)
* [Redaction failure exposing NSA agent username](#redaction-fail)
* [CNE operation against Mexican federal law enforcement](#mexican-feds)
* [CNE operation against Iran's customs and rails](#iran-customs-and-rails)
* [New surveillance program codenames](#programs)
  + [TURBOCHASER](#turbochaser)
  + [TUCKER](#tucker)
  + [SHADOWQUEST, WAYTIDE, GREENCHAOS](#shadowquest-waytide-greenchaos)
* [Other](#other)

+ [FOGGYBOTTOM: HTTP activity surveillance](#foggybottom)
+ [Windows registry surveillance](#windows-registry)
+ [Multi-lingual keylogger capabilities](#keylogger)
+ ["vpn in docs"](#vpn-in-docs)

This October 2009 33-page document, in a slideshow format, is an internal NSA training presentation demonstrating how analysts use XKEYSCORE to search and analyze data collected through Computer Network Exploitation (CNE), the NSA's term for active hacking operations. While framed as instructional examples showing various search capabilities, the screenshots display real surveillance operations with identifiable targets and captured data.

The screenshots in the document are such poor quality that, at times, reading the text is very difficult. However, by examining the context and surrounding text (or surrounding pages), the text can be inferred with a very high probability. This has certainly contributed to why many documents have not been studied more thoroughly in public, as many are similarly low quality with scrambled text.

## CNE operation against Chinese defense contractor Norinco

One of the most significant previously unreported findings in this document is evidence of NSA surveillance targeting Norinco, China North Industries Corporation, one of the world's largest state-owned defense contractors. Norinco ranks among the world's top 100 defense companies by revenue and serves as a major exporter of military equipment to Pakistan, Iran, Venezuela, Zimbabwe, and dozens of other countries, many of which have contentious relationships with the United States.

On page 18, a screenshot from XKEYSCORE's Metaviewer interface displays a "Histogram of @Domain" view with a bar graph showing email volume across 10 domain names followed by a data table with formatted surveillance results. The query appears to be a converged search combining multiple distinct surveillance targets: Mexican federal agencies (ssp.gob.mx at 452 emails, pfp.local at 158 emails), Norinco-related domains (mail.norinco.cn, businessmonitor.com, bmi.msgfocus.com, zhenhuaoil.com, and lms-ms-daemon, each showing 3 emails), and two additional targets (steels-net.cu and inwind.it, each with 1 email). This convergence of seemingly unrelated targets in a single query demonstrates XKEYSCORE's ability to simultaneously analyze multiple surveillance operations.

The first five entries in the results table contain:

```
Email User Name | Datetime            | Highlights | @Domain             | Subject                               | Chain
[REDACTED]      | 2009-10-10 05:15:10 | CNE        | mail.norinco.cn     | 28-10 senior contacts in India for zh | 0kqe00g01mrdii@mail.norinco.cn&kate.strut
[REDACTED]      | 2009-10-10 05:15:10 | CNE        | businessmonitor.com | 28-10 senior contacts in India for zh | 0kqe00g01mrdii@mail.norinco.cn&kate.strut
[REDACTED]      | 2009-10-10 05:15:10 | CNE        | bmi.msgfocus.com    | 28-10 senior contacts in India for zh | 0kqe00g01mrdii@mail.norinco.cn&kate.strut
[REDACTED]      | 2009-10-10 05:15:10 | CNE        | zhenhuaoil.com      | 28-10 senior contacts in India for zh | 0kqe00g01mrdii@mail.norinco.cn&kate.strut
[REDACTED]      | 2009-10-10 05:15:10 | CNE        | lms-ms-daemon       | 28-10 senior contacts in India for zh | 0kqe00g01mrdii@mail.norinco.cn&kate.strut
```

![Screenshot from the page 18 showing emails related to Norinco](/public/post-imgs/p18.jpg)

All entries are marked with the "CNE" highlight tag, indicating the data came from CNE operations, active hacking intrusions rather than passive network intercepts. Critically, all five entries share an identical "Chain" value indicating this is a single email captured at multiple points as it traversed Norinco's email infrastructure. The multiple domains – businessmonitor.com (newsletter sender), bmi.msgfocus.com (newsletter delivery service), mail.norinco.cn (Norinco's mail server), zhenhuaoil.com (Norinco's subsidiary), and lms-ms-daemon (the [default domain name](https://docs.oracle.com/cd/E19563-01/819-4428/bgaih/index.html) for Sun Java Messaging Server commonly used in enterprise email infrastructure) – represent the newsletter email's routing path through Norinco's network. This indicates that NSA achieved deep network penetration with visibility across multiple servers and routing points within Norinco's corporate email infrastructure, not just a single interception point. The compromise extended to Zhenhua Oil (Norinco's oil exploration subsidiary), indicating enterprise-wide access.

## Redaction failure exposing NSA agent username

Most XKEYSCORE search interfaces display a welcoming message showing the analyst's internal NSA username. In the document all usernames have been redacted from the screenshots except one left unredacted by mistake.

![Screenshot showing redacted NSA username](/public/post-imgs/r1.jpg)

On page 9, the username "**cryerni**" is visible in the screenshot.

![Screenshot showing NSA agent username](/public/post-imgs/r2.jpg)

This username most likely belongs to the NSA analyst who created the presentation. The seven-character length matches the redacted name on the first page, based on the surrounding unredacted font. The length of seven characters also matches to other NSA agents' usernames in other documents (more on that in upcoming parts).

## CNE operation against Mexican federal law enforcement

On the page 18, the XKEYSCORE Metaviewer displays email extraction results showing surveillance of Mexican federal law enforcement from domains ssp.gob.mx (Secretaría de Seguridad Pública) and pfp.local (Policía Federal Preventiva). Email subjects include:

```
101009 EII LA PAZ, BAJA CALIFORNIA
101009 EII MEXICALI, BAJA CALIFORNIA
101009 EII CIUDAD JUÁREZ, CHIHUAHUA
```

![Screenshot from the page 18 showing emails related to ssp.gob.mx and pfp.local](/public/post-imgs/p18_2.jpg)

"EII" likely stands for "Estructura de Información de Inteligencia" or similar internal reporting format. The dates (101009 = October 10, 2009) and locations indicate daily intelligence reports from Mexican federal police units in Baja California's border region and Ciudad Juárez, one of Mexico's most violent cities during the peak of cartel warfare under President Felipe Calderón's military-led offensive against drug cartels.

NSA surveillance of these communications likely supported US counter-narcotics operations, identified compromised Mexican officials, and monitored cartel structures and government response capabilities. However, this represents surveillance of a nominal ally's law enforcement agencies without apparent Mexican government knowledge or consent. All entries were marked "CNE," again indicating active computer compromise rather than passive intercept.

## CNE operation against Iran's customs and rails

Another interesting finding appears on page 17, showing document metadata extraction results with the name "Iran OP Customs and Rail Extracted Docs". The results table displays documents captured from a file path containing "lap top drive" and "Private Inbox", with all entries marked "CNE" in the Highlights column, indicating NSA compromised a portable computer likely belonging to someone working in Iranian transportation or customs infrastructure. The implant performed a complete directory walk and extracted Word documents from the user's private folders.

![Screenshot from the page 17 titled 'XK Metaviewer Iran OP Customs and Rail Extracted Docs'](/public/post-imgs/p17.jpg)![Screenshot from the page 17 table showing the surveillance results](/public/post-imgs/p17_2.jpg)

## New surveillance program codenames

Several program codenames mentioned in this document don't appear in any other published Snowden documents or in previous reporting. No mention either in [websites](https://www.electrospaces.net/p/nicknames-and-codewords.html) [documenting](https://www.electrospaces.net/p/abbreviations-and-acronyms.html) [all](https://gist.github.com/dustyfresh/a98abad0dd63b3b4e5bc8ff6e3cf0781) [the](https://christopher-parsons.com/resources/the-sigint-summaries/nsa-summaries/) [codenames](https://www.electrospaces.net/p/gchq-nicknames-and-codewords.html) [found](https://christopher-parsons.com/resources/the-sigint-summaries/gchq-covernames-programs-and-suggested-use-implementation/) in Snowden documents and in other NSA/GCHQ related articles and documents.

**TURBOCHASER** - The document describes TURBOCHASER as an NSA database for "profiles" and for "future tasking", appearing alongside MARINA (the well-documented NSA metadata repository). The name suggests rapid-cycling or high-speed processing ("turbo") of pursuit targets ("chaser"). Based on context, TURBOCHASER likely handled specific metadata types or geographic regions that MARINA didn't cover. The document's brief mention provides no additional details.

**TUCKER** - References in the document suggest TUCKER is an exploitation framework comparable to UNITEDRAKE (the well-documented full-featured Windows implant). The document lists TUCKER's sub-projects including OLYMPUS, EXPANDINGPULLY, and UNIX, indicating TUCKER was a platform hosting multiple specialized payloads and/or (post-)exploitation tools.

**SHADOWQUEST**, **WAYTIDE**, **GREENCHAOS** - These appear as collection source identifiers in the document. The document shows them as input sources feeding CNE data into XKEYSCORE. Notably, FOXACID, the well-documented NSA exploit server system used to deliver malware to targets, also appears in this context with the suffix FOXACID6654, suggesting it functioned not just as an exploitation delivery mechanism but also as a collection source identifier once targets were compromised. This reveals FOXACID's dual role: initial compromise vector and ongoing data collection infrastructure.

The input sources shown include:

* FOXACID6654 - collecting wireless survey data
* SHADOWQUEST35 - collecting wireless survey data
* WAYTIDE1173 - collecting wireless intelligence
* GREENCHAOS15 - source of the Chinese keylogger data

The numeric suffixes (6654, 35, 1173, 15) likely designates a specific server or operational instance, possibly corresponding to geographic regions, operational theaters, or specific TAO teams.

## Other

Finally, the document showcases several detailed cases of NSA's CNE capabilities, confirming and adding specific context to techniques that have been reported on more generally since 2013.

### FOGGYBOTTOM: HTTP activity surveillance

![Screenshot showing the captured HTTP activity](/public/post-imgs/foggy.jpg)

Pages 19-20 showcase FOGGYBOTTOM for monitoring HTTP activity captured through CNE operations. FOGGYBOTTOM is a computer implant plug-in that records logs of internet browsing histories and collects login details and passwords used to access websites and email accounts. These pages show detailed browser surveillance of a target identified by case notation YM.VALAGWAADTC (Yemen) on October 14, 2009. The system captured:

* Multiple Facebook login attempts (login.facebook.com with "login\_attempt=1" POST requests)
* Arabic-language Facebook browsing (ar-ar.facebook.com)
* Saudi Arabian Google searches (www.google.com.sa with "hl=ar" indicating Arabic language)
* Yemeni news sites (www.14october.com, www.26sep.net, www.althawranews.net)
* Arabic sports forums (forum.kooora.com - a popular Middle Eastern sports discussion site)

The surveillance captured not just URLs but complete HTTP request details including POST data and URL parameters. The "dnt\_payload/browser" formatter shows the target's local time, timezone offset, and HTTP POST form data. Since this data comes from a CNE implant running on the compromised computer itself – not passive network interception – it captures web traffic before encryption occurs. The implant sees the browsing data whether the connection uses HTTP or HTTPS, providing complete visibility into all browsing activity including encrypted sessions that would be opaque to network-level surveillance.

### Windows registry surveillance

![Screenshot showing the Windows registry entries](/public/post-imgs/reg.jpg)

Page 26 demonstrates XKEYSCORE's capability to search and analyze Windows registry data extracted from compromised machines. The screenshots show registry queries returning UserAssist keys; Windows registry entries that record every program a user has executed, how many times, and when they last ran it. This data is maintained by Windows for user interface optimization but becomes a detailed forensic record when captured by NSA implants.

### Multi-lingual keylogger capabilities

![Screenshot of keystroke data captured from a target in China using QQ Messenger and Microsoft Excel](/public/post-imgs/keylogger.jpg)

Pages 24-25 demonstrate XKEYSCORE's keylogger capabilities with actual captured keystrokes from a compromised computer identified as GREENCHAOS15 in China. The target was using QQ.exe (China's largest instant messaging platform owned by Tencent), Microsoft Excel, and Microsoft Access. The keylogger captured complete Chinese character input, control key sequences, hexadecimal codes for special characters, window titles showing conversation participants, and even deleted text and editing actions. In Excel, the system recorded every keystroke including numerical entries, navigation inputs (Delete, NumPad entries), and cell references (D4, H2, D53, etc.), showing the target working on a spreadsheet titled "3C证书导入工作周报0928-1001.xls" (3C Certificate Import Work Weekly Report 09/28-10/01). The target appeared to be an office worker handling administrative tasks related to China's 3C certification system (China Compulsory Certificate for product safety/quality). This demonstrates NSA's ability to capture multi-lingual keystrokes across all applications with complete context preservation.

### "vpn in docs"

![XKEYSCORE results flagging the keywords 'vpn' and 'pptp' found within captured documents and emails](/public/post-imgs/vpn.jpg)

The document also demonstrates how XKEYSCORE uses a generic "tech strings" search to automatically identify and flag arbitrary keywords that an analyst queries. This feature appears to function as a catchall system for finding terms of interest in data streams that lack a more specific parser. The examples show XKEYSCORE tagging the strings "vpn" and "pptp" inside a wide variety of captured data. This includes the content of emails (email\_body), the body of local documents (document\_body with file paths like C:\TNI-095CC.DOC), and other raw data payloads exfiltrated from implants (tech\_body). As nearly all entries are highlighted with "CNE," this reveals that NSA implants actively scan a target's private files and communications for these keywords. The resulting intelligence allows analysts to discover a target's security posture, identify potential vulnerabilities, and find information such as credentials or server details that can be leveraged to gain access to privileged systems or map internal networks.

This document is a good example of the significant intelligence hiding in plain sight within the published Snowden documents. A detailed review can reveal significant, previously unreported intelligence operations, such as the CNE op against a major Chinese defense contractor. These findings underscore the importance of a systematic review of the documents. Also, it's important to acknowledge the inherent limitations of analyzing any single document in isolation like we did in this post. A single document analysis offers only a snapshot with limited context.

* [[ I2P ]](http://xvqmnhevx32br7m4e7g3yoxfirizo4m3uktym3wnuntbgbr5bvna.b32.i2p)
* [[ Tor ]](http://librootfuuucybrkpvarmpswsxnbsakf2oqqzxncvsqrvc2j73kuufqd.onion)
> Libroot.org - rooted in freedom.

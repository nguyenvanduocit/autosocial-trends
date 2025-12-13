---
source: hackernews
title: Security issues with electronic invoices
url: https://invoice.secvuln.info/
date: 2025-12-13
---

# Security Issues with Electronic Invoices

![XXE Invoice Logo](/xxeinvoice.svg)

This page provides supplementary material for a [presentation given at the German OWASP
Day 2025](https://god.owasp.de/2025/program-detail.html?talk=talkFour)
([Presentation Slides](https://slides.hboeck.de/2025-owaspday-invoice/)).

[![Preview OWASP talk video recording](/owasptalk-preview.avif)
Video Recording at media.ccc.de](https://media.ccc.de/v/god2025-56476-how-the-eu-created-electro)

## Intro

With the [eInvoicing Directive (2014/55/EU)](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX:32014L0055), the European
Union introduced “standardized” electronic invoices in XML format. Increasingly,
institutions and businesses in EU member states will be required to support these
electronic invoices.

While machine-readable invoices are, in general, a good idea, there are various issues
with the EU’s approach, including needless complexity, a lack of true standardization
(multiple syntaxes and various sub-formats), and a tendency to use technologies with
inherent security problems.

Due to a combination of unfortunate design decisions, implementing software for
electronic invoices is likely to be affected by security flaws if no countermeasures are
implemented.

## XML Insecurity and XXE

The XML format is known to have inherent security flaws, the most dangerous ones being
XXE vulnerabilities (XML eXternal Entity injection).

XXE vulnerabilities often allow the exfiltration of files. While some XML implementations have
implemented secure defaults or were never vulnerable to begin with (e.g., [Python](https://nvd.nist.gov/vuln/detail/CVE-2013-1665), [libxml2](https://nvd.nist.gov/vuln/detail/CVE-2013-0339), [.NET](https://nvd.nist.gov/vuln/detail/CVE-2016-3255), [Expat](https://libexpat.github.io/doc/xml-security/#external-entities)), others remain insecure
by default.

Two notable examples of implementations with insecure defaults are the Java standard
library and the Saxon library. Both are commonly used within the electronic invoicing
ecosystem.

## The problem with XSLT 2.0

XSLT is a document transformation language. Only XSLT version 1.0 is widely supported.
For XSLT 2.0 and above, only one freely available implementation exists: Saxon.

To check compliance with the EN16931 standards, the [EU provides validation artifacts
based on Schematron](https://github.com/ConnectingEurope/eInvoicing-EN16931). Those validation artifacts
require XSLT 2.0.

Thus, anyone using these validation artifacts will likely use Saxon to implement invoice
parsing. Saxon, as mentioned, is vulnerable to XXE by default.

Despite its poor implementation status and the fact that its primary implementation
has insecure defaults, XSLT 2.0 (and its successor 3.0) is a W3C recommendation. I
raised these concerns with the W3C.

* [Discussion about XSLT 2.0/3.0 at W3C (2025)](https://github.com/w3ctag/obsoletion/issues/10)
* [Statement from back-then libxslt maintainer that no XSLT 2.0 support is planned (2007)](https://www.mail-archive.com/xml%40gnome.org/msg04082.html)
* [Comment from 2005 that XSLT 2.0 “was likely to be just a single implementation language”](https://www.snee.com/bobdc.blog/2005/12/no-plans-in-place-to-upgrade-x.html#comment-15)

## Security test suite

A [security test suite for electronic invoices is provided here](https://github.com/hannob/invoicesec).

## Getting the EN16931 standards

The EU requirements for electronic invoices are standardized by the European Committee
for Standardization (CEN) in a set of standards named EN16931. The first two parts are
available free of charge. Subsequent parts cost money.

Accessing these standards is surprisingly difficult. A link on the EU web page to CEN is
currently broken. CEN does not provide direct downloads of these documents and refers to
national standardization organizations. Those often require account registrations even
to access the free-of-charge parts of the standard.

The Estonian standardization organization (EVS) provides downloads of parts one and two
without registration:

* [EVS-EN 16931-1:2017+A1:2019/AC:2020](https://www.evs.ee/StandardDownload/DownloadPdf?productId=132881&language=EnglishLanguage)
  ([source](https://www.evs.ee/en/evs-en-16931-1-2017-a1-2019-ac-2020-consolidated))
* [CEN/TS 16931-2:2017 (English)](https://www.evs.ee/StandardDownload/DownloadPdf?productId=88987&language=EnglishLanguage)
  ([source](https://www.evs.ee/en/cen-ts-16931-2-2017))

For the parts of EN16931 that are not available free of charge, prices at EVS are
cheaper than those at most other national standardization organizations.

## XXE vulnerabilties

List of security vulnerabilities discovered in electronic invoicing software during this
research:

| Product | Vuln type | Info |
| --- | --- | --- |
| [kivitendo](https://www.kivitendo.de/) | XXE | Reported 2025-03-25, [Fixed in 3.9.2beta (2025-03-28) / 3.9.2 (2025-05-05)](https://blog.kivitendo.de/?p=1415), Software Stack: Perl/XML::LibXML, [CVE-2025-66370](https://www.cve.org/CVERecord?id=CVE-2025-66370) |
| [peppol-py](https://github.com/iterasdev/peppol-py/) | Blind XXE | Reported: 2025-11-13, [fixed in 1.1.1 (2025-11-13)](%28https%3A//github.com/iterasdev/peppol-py/releases/tag/1.1.1%29), Software Stack: Python/Saxon, [CVE-2025-66371](https://www.cve.org/CVERecord?id=CVE-2025-66371) |
| [ZUV](https://github.com/ZUGFeRD/ZUV/)\* | Blind XXE | Reported: 2025-11-17, no longer developed according to README, Software Stack: Java/Saxon |
| [papierkram.de E-Rechnung-Viewer](https://www.papierkram.de/e-rechnung/e-rechnung-viewer-kostenlos) | XXE | Reported: 2025-03-30, fixed: 2025-03-31 |
| [EPO E-Invoice Viewer](https://www.epoconsulting.com/en/einvoice-sap/e-invoice-viewer) | XXE | Reported: 2025-10-13, fixed: 2025-10-14 |
| [portinvoice](https://www.portinvoice.com/) | XXE | Reported: 2025-10-29, fixed: 2025-10-29 |
| [xrechnung-erstellen.com E-Rechnung Viewer](https://xrechnung-erstellen.com/e-rechnung-empfangen) | XXE | Reported: 2025-10-14, fixed: 2025-10-16 |
| [Belegmeister ZUGFERD VIEWER](https://belegmeister.de/zugferd-e-rechnungen-online-anzeigen/) | Blind XXE | Reported: 2025-11-15 (only supports PDF upload), fixed: 2025-11-25 |
| [E-Rechnungs-Validator by winball.de](https://erechnungs-validator.de/) | Blind XXE | Reported: 2025-11-17, fixed: 2025-11-19, [confirmation](https://erechnungs-validator.de/comment-page-6/#comment-11347) |
| [ZUGFeRD Community ZF/FX Invoiceportal](https://www.zugferd-community.net/de/zf_fx_invoiceportal/rechnungseingang) | Blind XXE | Reported: 2025-11-17, no reply, re-tested on 2025-11-25, validation functionality was removed (relied on ZUV) |
| REDACTED1 | XXE | Reported: 2025-10-29, no reply, re-tested on 2025-11-18, fix incomplete (see next line) |
| REDACTED1 | Blind XXE | Reported: 2025-11-18, no reply, unfixed |
| REDACTED2 | Blind XXE | Reported: 2025-11-17, no reply, unfixed |

\* ZUV is no longer developed, and it is recommended to use Mustang instead. Mustang was
also vulnerable to XXE in versions before [2.16.3](https://github.com/ZUGFeRD/mustangproject/releases/tag/core-2.16.3) ([CVE-2025-66372](https://www.cve.org/CVERecord?id=CVE-2025-66372)).

## More

* [ZUGFeRD, XRechnung und Co.: Wie elektronische Rechnungen zum Sicherheitsrisiko werden](https://www.golem.de/news/zugferd-xrechnung-und-co-wie-elektronische-rechnungen-zum-sicherheitsrisiko-werden-2512-202959.html) (German article, Golem, 2025-12-12)

### Questions?

[Get in touch!](https://itsec.hboeck.de/contact.html)

*Text and logo are licensed as [CC0](https://creativecommons.org/public-domain/cc0/).
The logo is a mix [of](https://www.svgrepo.com/svg/298686/invoice)
[three](https://www.svgrepo.com/svg/31053/xml)
[icons](https://www.svgrepo.com/svg/231600/poison-skull) from svgrepo.com, all CC0.
The web page uses [Pico CSS](https://picocss.com/) (MIT license) and
[Hugo](https://gohugo.io/).*

*Created by [Hanno Böck](https://itsec.hboeck.de/)
(created: November 25, 2025, last update: December 12, 2025)*

[Imprint](/imprint/)

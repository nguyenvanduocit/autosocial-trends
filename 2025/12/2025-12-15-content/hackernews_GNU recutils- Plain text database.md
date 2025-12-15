---
source: hackernews
title: GNU recutils: Plain text database
url: https://www.gnu.org/software/recutils/
date: 2025-12-15
---

[**Skip to main text**](#content)

**We support your freedom!**

[The free software movement has come a long way in forty years. We want
to take a moment to thank the people and projects who have helped bring
us to this point, and ask for your support in the decades to come. Help
us reach our fundraising goal of $400,000 by January 1, 2026 to keep
us strong and steadfast in our continuing work.](https://my.fsf.org/donate?mtm_campaign=winter25&mtm_source=banner)

[Donate](https://my.fsf.org/donate?mtm_campaign=winter25&mtm_source=banner) [Read more](https://www.fsf.org/appeal?mtm_campaign=winter25&mtm_source=banner)

$253,477

$400,000

[![ [A GNU head] ](/graphics/heckert_gnu.transp.small.png)**GNU** Operating System](/)
Supported by the
[Free Software Foundation](#mission-statement)

[![ [Search www.gnu.org] ](/graphics/icons/search.png)](//www.gnu.org/cgi-bin/estseek.cgi)

[Site navigation](#navigation "More...")
[**Skip**](#content)

* [ABOUT GNU](/gnu/gnu.html)
* [PHILOSOPHY](/philosophy/philosophy.html)
* [LICENSES](/licenses/licenses.html)
* [EDUCATION](/education/education.html)
* =
  [SOFTWARE](/software/software.html)

  =
* [DISTROS](/distros/distros.html)
* [DOCS](/doc/doc.html)
* [MALWARE](/proprietary/proprietary.html)
* [HELP GNU](/help/help.html)
* [AUDIO & VIDEO](/audio-video/audio-video.html)
* [GNU ART](/graphics/graphics.html)
* [FUN](/fun/humor.html)
* [GNU'S WHO?](/people/people.html)
* [SOFTWARE DIRECTORY](//directory.fsf.org)
* [HARDWARE](https://h-node.org/)
* [SITEMAP](/server/sitemap.html)

## GNU Recutils

[![ [GNU Recutils logo] ](logo.png)](logo.png)

GNU Recutils is a set of tools and libraries to access
**human-editable, plain text databases called recfiles**. The
data is stored as a sequence of records, each record containing an
arbitrary number of named fields. The picture below shows a sample
database containing information about GNU packages, along with the
main features provided by Recutils.

* A video with a talk introducing the program can be found
  [here](https://fscons.org/videos/2011/gnu-recutils-changed-title-and-subject.webm).
* An **older** video, which was recorded just before releasing the
  first version, can be downloaded
  from [here](http://balance.fsf.org/video/The_GNU_Record_Utilities.ogv)
* Some of the people involved in
  GNU Recutils hang out in the **#recutils** channel on
  the *irc.freenode.net* IRC network. You are more than welcome
  to join.

[![ [Recutils Screenshot] ](recutils-sc.png)](recutils-sc.png)

### Features

Data Integrity
:   * Mandatory and forbidden fields.
    * Unique fields and primary keys.
    * Auto-counters and time-stamps.
    * Arbitrary constraints.

Rich Type System for Fields
:   * Predefined: integer, real, date, etc.
    * User-defined: based on regular expressions.

Advanced database facilities
:   * Joins and foreign keys.
    * Grouping and sorting.
    * Aggregate functions.

Encryption Support
:   * Selective: individual fields can be encrypted.
    * Password-based AES.

Converters from/to other formats
:   * mdb files to recfiles.
    * csv files to/from recfiles.

Vim syntax highlight for recfiles
:   * Find it in [vim-rec](https://github.com/zaid/vim-rec)

Advanced Emacs mode
:   * Navigation mode and editing mode.
    * Field folding.
    * Visual edition of fields driven by types.
    * [User manual](rec-mode-manual/).

Integration with [org-mode](http://www.orgmode.org)
:   * Read data from recfiles into a table in an org-mode
      buffer in Emacs.
    * Publish the resulting data.

Templates
:   * Generate reports.
    * Build your own exporters.

Complete [user manual](manual/)
:   * Full description of the format.
    * Documentation for the utilities.
    * Usage examples.

Easy deployment
:   * C library: *librec*
    * Rich set of utilities to be used in shell scripts and in the
      command line.

#### Do you like this program?

**Please consider making a donation.** The maintainer's
PayPal id is jemarch@gnu.org. Thanks! :)

### Downloading Recutils

#### Sources Distribution

Recutils can be found on the main GNU ftp server
([download Recutils via HTTPS](https://ftp.gnu.org/gnu/recutils/),
[download Recutils via HTTP](http://ftp.gnu.org/gnu/recutils/) or
download Recutils via FTP), and its
[mirrors](/prep/ftp.html);
please
[use
a mirror](http://ftpmirror.gnu.org/recutils/) if possible.

The `rec-mode` Emacs mode is distributed in ELPA.

#### Binary Packages

Additionally there are binary packages for some distributions:

* [Trisquel GNU/Linux,
  Fedora, openSUSE, etc.](https://repology.org/project/recutils/versions)
* [Debian](http://packages.debian.org/search?keywords=recutils&searchon=names&suite=all&section=all)
* [Arch](https://aur.archlinux.org/packages/recutils/)
* [Mandrake](http://rpm.pbone.net/index.php3/stat/4/idpl/15128664/dir/mandrake_other/com/recutils-1.2-1-mdv2011.0.i586.rpm.html)
* [Parabola
  x86\_64](https://www.parabola.nu/packages/pcr/x86_64/recutils/)
* [Parabola
  i686](https://www.parabola.nu/packages/pcr/i686/recutils/)
* [OpenCSW
  Solaris](http://www.opencsw.org/packages/recutils/)
* [FreeBSD](http://www.freebsd.org/cgi/ports.cgi?query=recutils&stype=all)
* [Termux for Android](http://termux.com)
* [Void Linux](https://github.com/void-linux/void-packages/tree/master/srcpkgs/recutils)

If you know of some other binary distribution of GNU Recutils, please
get in touch with the maintainer.

### Documentation

Take a look at our [Frequently Asked
Questions](faq.html).

[Documentation for
Recutils](manual/)
is available online, as
is [documentation for most GNU software](/manual/). You
can find more information about Recutils by
running `info recutils` or by looking at
`/usr/doc/recutils/`,
or similar directories on your system.

### Mailing lists

Recutils has two mailing lists:

* [bug-recutils](https://lists.gnu.org/mailman/listinfo/bug-recutils)
  for discussing most aspects of Recutils, including development
  and enhancement requests, as well as bug reports;
* [help-recutils](https://lists.gnu.org/mailman/listinfo/help-recutils)
  for general user help and discussion.

Announcements about Recutils and most other GNU software are made on the
[info-gnu](https://lists.gnu.org/mailman/listinfo-gnu)
mailing list
([archives](https://lists.gnu.org/archive/html/info-gnu/)).

### Getting involved

Development of Recutils, and GNU in general, is a volunteer effort,
and you can contribute. For information, please
read [How to help GNU](/help/). If you'd like to get
involved, it's a good idea to join the discussion mailing list (see
above).

Test releases
:   Trying the latest test release (when available) is always
    appreciated. Test releases can be found on the GNU “alpha”
    server
    ([via HTTPS](https://alpha.gnu.org/gnu/recutils/),
    [HTTP](http://alpha.gnu.org/gnu/recutils/) or
    FTP), and its
    [mirrors](/prep/ftp.html).

Development
:   For development sources, and other information, please see the
    [Recutils
    project page](https://savannah.gnu.org/projects/recutils/)
    at [savannah.gnu.org](https://savannah.gnu.org).

    Please send bug reports and patches
    to [<bug-recutils@gnu.org>](https://lists.gnu.org/mailman/listinfo/bug-recutils).

Translating Recutils
:   To translate Recutils's
    messages into other languages, please see the [Translation Project
    page for Recutils](https://translationproject.org/domain/recutils.html).
    If you have a new translation of the message strings,
    or updates to the existing strings, please have the changes made in this
    repository. Only translations from this site will be incorporated into
    Recutils.
    For more information, see the [Translation
    Project home page](https://translationproject.org/html/welcome.html).

Maintainer
:   Recutils
    is currently being maintained by Jose E. Marchesi.
    Please use the mailing lists for contact.

### Licensing

Recutils
is free software; you can redistribute it and/or modify it under the
terms of the [GNU General Public License](/licenses/gpl.html) as published by the Free
Software Foundation; either version 3 of the License, or (at your
option) any later version.

---

[BACK TO TOP ▲](#header)

> [![ [FSF logo] ](/graphics/fsf-logo-notext-small.png)](//www.fsf.org)**“The Free Software Foundation (FSF) is a nonprofit with a worldwide
> mission to promote computer user freedom. We defend the rights of all
> software users.”**

[JOIN](//www.fsf.org/associate/support_freedom?referrer=4052)
[DONATE](//donate.fsf.org/)
[SHOP](//shop.fsf.org/)

Please send general FSF & GNU inquiries to
<gnu@gnu.org>.
There are also [other ways to contact](/contact/)
the FSF. Broken links and other corrections or suggestions can be sent
to <bug-recutils@gnu.org>.

Please see the [Translations
README](/server/standards/README.translations.html) for information on coordinating and contributing translations
of this article.

Copyright © 2010 Free Software Foundation, Inc.

This page is licensed under a [Creative
Commons Attribution-NoDerivatives 4.0 International License](http://creativecommons.org/licenses/by-nd/4.0/).

[Copyright Infringement Notification](//www.fsf.org/about/dmca-notice)

Updated:
$Date: 2025/03/26 10:27:33 $

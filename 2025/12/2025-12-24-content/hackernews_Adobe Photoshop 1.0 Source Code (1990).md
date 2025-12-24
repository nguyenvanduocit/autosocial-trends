---
source: hackernews
title: Adobe Photoshop 1.0 Source Code (1990)
url: https://computerhistory.org/blog/adobe-photoshop-source-code/
date: 2025-12-24
---

[![Computer History Museum Logo](https://computerhistory.org/wp-content/themes/bsdstarter/assets/img/chm-logo-text.svg)
![Computer History Museum Logo](https://computerhistory.org/wp-content/themes/bsdstarter/assets/img/chm-logo-short.svg)](https://computerhistory.org "Computer History Museum home link")

[Donate](https://donate.computerhistory.org)
[Tickets](https://connect.computerhistory.org)

Menu

* [Explore](/explore/)
  + [Overview](https://computerhistory.org/explore/)
  + [Collections](/collections/)
  + [Timelines](https://computerhistory.org/timelines/)
  + [Blogs](https://computerhistory.org/blogs/)
  + [Stories](/stories/)
  + [Playlists](/playlists/)
  + [Podcast](https://computerhistory.org/explore/podcast/)
  + [Activities & Resources](https://computerhistory.org/activities-resources/)
* [Visit](/visit/)
  + [Overview](/visit/)
  + [Plan Your Visit](https://computerhistory.org/plan-your-visit/)
  + [Exhibits](https://computerhistory.org/exhibits/)
  + [Tours](https://computerhistory.org/tours/)
  + [Group Visits](https://computerhistory.org/visit/group-visits/)
  + [Events](/events/)
  + [Venue](https://computerhistory.org/venue/)
  + [Shop](https://shop.computerhistory.org/)
* [About](/about/)
  + [Overview](/about/)
  + [This Is CHM](https://computerhistory.org/this-is-chm/)
  + [Awards](https://computerhistory.org/fellow-awards/)
  + [Programs](https://computerhistory.org/programs/)
  + [News](/press-releases)
  + [Leadership](https://computerhistory.org/leadership/)
* [Join & Give](/join/)
  + [Overview](/join/)
  + [Membership](https://computerhistory.org/membership/)
  + [Ways to Give](https://computerhistory.org/ways-to-give/)
  + [Donor Recognition](https://computerhistory.org/donor-recognition/)
  + [Institutional Supporters](https://computerhistory.org/institutional-supporters/)
  + [Volunteers](https://computerhistory.org/volunteers/)
* Search
* [Donate](https://donate.computerhistory.org)
* [Tickets](https://connect.computerhistory.org)

* [Hours & Admission](https://computerhistory.org/hours-admission/)
* [Subscribe](https://computerhistory.org/subscribe/)
* [Account](https://auth.computerhistory.org/sso?app=my_account)

Search for:

[Or search the collection catalog](https://www.computerhistory.org/collections/search/)

[Home](https://computerhistory.org/)  [CHM BLOG](https://computerhistory.org/blogs/)  [From the Collection](https://computerhistory.org/blog/category/from-the-collection/)

# Adobe Photoshop Source Code

#### By [Leonard J. Shustek](https://computerhistory.org/profile/leonard-j-shustek/) | February 13, 2013

![](https://computerhistory.org/wp-content/themes/bsdstarter/assets/img/basic-page-core-memory.svg)

## Share

[![Facebook](/wp-content/themes/bsdstarter/assets/img/share_icons/facebook.png)](https://www.addtoany.com/add_to/facebook?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Facebook")[![Twitter](/wp-content/themes/bsdstarter/assets/img/share_icons/twitter.png)](https://www.addtoany.com/add_to/twitter?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Twitter")[![Copy Link](/wp-content/themes/bsdstarter/assets/img/share_icons/link.png)](https://www.addtoany.com/add_to/copy_link?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Copy Link")

### *Software Gems: The Computer History Museum Historical Source Code Series*

### pho·to·shop, transitive verb, often capitalized ˈfō-(ˌ)tō-ˌshäp to alter (a digital image) with Photoshop software or other image-editing software especially in a way that distorts reality (as for deliberately deceptive purposes)

#### — Merriam-Webster online dictionary, 2012

When brothers Thomas and John Knoll began designing and writing an image editing program in the late 1980s, they could not have imagined that they would be adding a word to the dictionary.

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-thomas-knoll.jpg)

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-john-knoll.jpg)

Thomas Knoll, a PhD student in computer vision at the University of Michigan, had written a program in 1987 to display and modify digital images. His brother John, working at the movie visual effects company Industrial Light & Magic, found it useful for editing photos, but it wasn’t intended to be a product. Thomas said, “We developed it originally for our own personal use…it was a lot a fun to do.”

Gradually the program, called “Display”, became more sophisticated. In the summer of 1988 they realized that it indeed could be a credible commercial product. They renamed it “Photoshop” and began to search for a company to distribute it. About 200 copies of version 0.87 were bundled by slide scanner manufacturer Barneyscan as “Barneyscan XP”.

The fate of Photoshop was sealed when Adobe, encouraged by its art director Russell Brown, decided to buy a license to distribute an enhanced version of Photoshop. The deal was finalized in April 1989, and version 1.0 started shipping early in 1990.

Over the next ten years, more than 3 million copies of Photoshop were sold.

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-box-and-disk.jpg)

That first version of Photoshop was written primarily in Pascal for the Apple Macintosh, with some machine language for the underlying Motorola 68000 microprocessor where execution efficiency was important. It wasn’t the effort of a huge team. Thomas said, “For version 1, I was the only engineer, and for version 2, we had two engineers.” While Thomas worked on the base application program, John wrote many of the image-processing plug-ins.

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-splashscreen.jpg)

With the permission of Adobe Systems Inc., the Computer History Museum is pleased to make available, for non-commercial use, the source code to the 1990 version 1.0.1 of Photoshop. All the code is here with the exception of the MacApp applications library that was licensed from Apple. There are 179 files in the zipped folder, comprising about 128,000 lines of mostly uncommented but well-structured code. By line count, about 75% of the code is in Pascal, about 15% is in 68000 assembler language, and the rest is data of various sorts.

To download the code you must agree to the terms of the license, which permits only non-commercial use and does not give you the right to license it to third parties by posting copies elsewhere on the web.

Download [Photoshop version 1.0.1 Source Code](/blog/photoshop-software-license-agreement/ "Download Photoshop version 1.0.1 Source Code")

* [1990 version of Adobe Photoshop User Guide](https://www.computerhistory.org/collections/catalog/102640940)
* [1990 version of Adobe Photoshop tutorial](https://www.computerhistory.org/collections/catalog/102640945)

## Commentary on the source code

Software architect Grady Booch is the Chief Scientist for Software Engineering at IBM Research Almaden and a trustee of the Computer History Museum. He offers the following observations about the Photoshop source code:

* “Opening the files that constituted the source code for Photoshop 1.0, I felt a bit like Howard Carter as he first breached the tomb of King Tutankhamen. What wonders awaited me?
* I was not disappointed by what I found. Indeed, it was a marvelous journey to open up the cunning machinery of an application I’d first used over 20 years ago.
* Architecturally, this is a very well-structured system. There’s a consistent separation of interface and abstraction, and the design decisions made to componentize those abstractions – with generally one major type for each combination of interface and implementation – were easy to follow.
* The abstractions are quite mature. The consistent naming, the granularity of methods, the almost breathtaking simplicity of the implementations because each type was so well abstracted, all combine to make it easy to discern the texture of the system.
* Having the opportunity to examine Photoshop’s current architecture, I believe I see fundamental structures that have persisted, though certainly in more evolved forms, in the modern implementation. Tiles, filters, abstractions for virtual memory (to attend to images far larger than display buffers or main memory could normally handle) are all there in the first version. Yet it had just over 100,000 lines of code, compared to well over 10 million in the current version! Then and now, much of the code is related to input/output and the myriad of file formats that Photoshop has to attend to.
* There are only a few comments in the version 1.0 source code, most of which are associated with assembly language snippets. That said, the lack of comments is simply not an issue. This code is so literate, so easy to read, that comments might even have gotten in the way.
* It is delightful to find historical vestiges of the time: code to attend to Andy Herzfield’s software for the Thunderscan scanner, support of early TARGA raster graphics file types, and even a few passing references to Barneyscan lie scattered about in the code. These are very small elements of the overall code base, but their appearance reminds me that no code is an island.
* This is the kind of code I aspire to write.”

And this is the kind of code we all can learn from. Software source code is the literature of computer scientists, and it deserves to be studied and appreciated. Enjoy a view of Photoshop from the inside.

## Early Photoshop screenshots\*

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-screen-main.jpg)

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-screen-brushes.jpg)

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-screen-image_filters.jpg)

There were some sophisticated selection tools, and a good assortment of image filters. One important missing feature, which came with version 3 in 1994, was the ability to divide an image into multiple layers.

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-screen-preferences.jpg)

The preferences page allowed for some customization of the features.

![](https://computerhistory.org/wp-content/uploads/2019/08/photoshop-source-code-screen-fonts.jpg)

There was a limited choice of fonts, font sizes, and font styles. The text was entered into this dialog box, then moved into the image.

* *Screen shots courtesy of creativebits, [www.creativebits.org](http://www.creativebits.com.sg/).*

### Historical Source Code Releases

* [MacPaint and QuickDraw Source Code](/blog/macpaint-and-quickdraw-source-code), July 18, 2010
* [APL Programming Language Source Code](/blog/the-apl-programming-language-source-code/), October 10, 2012
* [Adobe Photoshop Source Code](/blog/adobe-photoshop-source-code/), February 13, 2013
* [Apple II DOS Source Code](/blog/apple-ii-dos-source-code/), November 12, 2013
* [Microsoft MS-DOS Early Source Code](/blog/microsoft-ms-dos-early-source-code/), March 25, 2014
* [Microsoft Word for Windows Version 1.1a Source Code](/blog/microsoft-word-for-windows-1-1a-source-code/), March 25, 2014
* [Early Digital Research CP/M Source Code](/blog/early-digital-research-cpm-source-code/), October 1, 2014
* [Xerox Alto Source Code](/blog/xerox-alto-source-code/), October 21, 2014
* [Electronic Arts DeluxePaint Early Source Code](/blog/electronic-arts-deluxepaint-early-source-code/), July 22, 2015

## About The Author

Len Shustek is the founding chairman emeritus of the board of trustees of the Computer History Museum.

In 1979, he co-founded Nestar Systems, an early developer of networks for personal computers. In 1986, he cofounded Network General, a manufacturer of network analysis tools including The Sniffer™. The company became Network Associates after merging with McAfee Associates and PGP. He has taught Computer Science at Carnegie-Mellon and Stanford Universities, and was a founder of the “angel financing” firm VenCraft. He has served on various for-profit and non-profit boards, including the Polytechnic Institute of Brooklyn, which is now the NYU Tandon School of Engineering.

Shustek has a BS and MS in Physics from the Polytechnic Institute, and an MS and PhD in Computer Science from Stanford University.

## Join the Discussion

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

## Related Articles

[View all articles](https://computerhistory.org/blogs)

[##### CHM BLOG

#### Memories of Lisa

January 30, 2025](https://computerhistory.org/blog/memories-of-lisa/)

[##### CHM BLOG

#### Hearing Tech History

October 22, 2024](https://computerhistory.org/blog/hearing-tech-history/)

[##### CHM BLOG

#### Neural Network Chip Joins the Collection

August 09, 2024](https://computerhistory.org/blog/neural-network-chip-joins-the-collection/)

[View all articles](https://computerhistory.org/blogs)

## Share

[![Facebook](/wp-content/themes/bsdstarter/assets/img/share_icons/facebook.png)](https://www.addtoany.com/add_to/facebook?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Facebook")[![Twitter](/wp-content/themes/bsdstarter/assets/img/share_icons/twitter.png)](https://www.addtoany.com/add_to/twitter?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Twitter")[![Copy Link](/wp-content/themes/bsdstarter/assets/img/share_icons/link.png)](https://www.addtoany.com/add_to/copy_link?linkurl=https%3A%2F%2Fcomputerhistory.org%2Fblog%2Fadobe-photoshop-source-code%2F&linkname=Adobe%20Photoshop%20Source%20Code "Copy Link")

[![Computer History Museum Logo](https://computerhistory.org/wp-content/themes/bsdstarter/assets/img/chm-logo-short.svg)](https://computerhistory.org "Computer History Museum home link")

* CHM is a registered 501(c)3 organization. Your donation is tax-deductible to the extent permitted by law.
* Tax ID 77-0507525.
* [Terms of Use](https://computerhistory.org/terms/)
* [Privacy](https://computerhistory.org/privacy/)
* © 2025 Computer History Museum

* [Press](/press-releases/)
* [Careers](/jobs/)
* [Venue](https://computerhistory.org/venue/)

* [Events](/events/)
* [Exhibits](/exhibits/)
* [Donate](https://computerhistory.org/donate-now/)

## Hours and Directions

[Hours & Directions](/make-plan/)

1401 N. Shoreline Blvd.

Mountain View, CA 94043

(650) 810-1010

[More Contact Info](/contact-us/)

CHM will be closed on Wed. Dec. 24, and Thurs. Dec. 25, 2025.

x

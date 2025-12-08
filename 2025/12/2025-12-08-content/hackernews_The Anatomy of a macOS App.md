---
source: hackernews
title: The Anatomy of a macOS App
url: https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/
date: 2025-12-08
---

[Skip to content](#content)

[![](https://eclecticlight.co/wp-content/uploads/2015/01/eclecticlightlogo-e1421784280911.png?w=103)](https://eclecticlight.co/)

# [The Eclectic Light Company](https://eclecticlight.co/)

Macs & painting – 🦉 No AI content

##### Main navigation

Menu

* [Downloads](https://eclecticlight.co/downloads/)
* [Freeware](https://eclecticlight.co/free-software-menu/)
* [M-series Macs](https://eclecticlight.co/m1-macs/)
* [Mac Problems](https://eclecticlight.co/mac-troubleshooting-summary/)
* [Mac articles](https://eclecticlight.co/mac-problem-solving/)
* [Macs](https://eclecticlight.co/category/macs/)
* [Art](https://eclecticlight.co/painting-topics/)

[hoakley](https://eclecticlight.co/author/hoakley/)
[December 4, 2025](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/)
[Macs](https://eclecticlight.co/category/macs/), [Technology](https://eclecticlight.co/category/technology/)

# The Anatomy of a macOS App

Programs running in windowing environments, *applications* as we used to know them, have more complicated requirements than those run from a command line. Rather than embed all the resources they require for windows, menus and the rest in a single file, Mac OS broke new ground by putting those into resources stored in the app’s resource fork.

[![prefsresedit](https://eclecticlight.co/wp-content/uploads/2015/11/prefsresedit.png?w=940)](https://eclecticlight.co/wp-content/uploads/2015/11/prefsresedit.png)

This is QuarkXPress version 4.11 from around 2000, with its resources displayed in the resource editor ResEdit. Executable code was also stored in CODE resources, and every file contained type and creator information to support the illusions created by the Finder.

#### Mac OS X

When Mac OS X was designed, it switched to the bundle structure inherited from NeXTSTEP. Instead of this multitude of resources, apps consisted of a hierarchy of directories containing files of executable code, and those with what had in Mac OS been supporting resources. Those app bundles came to adopt a standard form, shown below.

[![](https://eclecticlight.co/wp-content/uploads/2025/12/anatomyofapposx.jpg)](https://eclecticlight.co/wp-content/uploads/2025/12/anatomyofapposx.jpg)

The bundle name has the extension .app, and contains a single directory Contents. Within that, the executable code is in the MacOS directory, which may contain both the main executable for the GUI app and any bundled command tools provided. Another directory contains Resources, including the app’s custom icon, and components of its GUI. In some apps, there’s another directory of Frameworks containing dylibs (libraries).

There are also two important files, Info.plist and PkgInfo. The latter contains the same type and creator information inherited from Classic Mac OS, and apparently isn’t mandatory although it appears universal. The information property list is essential, as it specifies the names of the executable and its icon file in Resources, the minimum version of macOS required, type declarations of the app’s documents, version numbers, and more.

When running a command tool in macOS, its Mach-O executable is launched by `launchd`, whose purpose is to run code. Launching an app is more demanding, although the app’s executable is still launched by `launchd`. Before that can happen, macOS starts the launch process using LaunchServices and RunningBoard, which rely on information obtained from Info.plist and other components in the app bundle.

#### macOS

This structure remained stable until the introduction of code signatures in Mac OS X 10.5 Leopard in 2007. Accommodating those added a directory named \_CodeSignature containing the signature in a CodeResources file. That includes code directory hashes (CDHashes) to check the integrity of the contents of the app bundle. Apps distributed by the App Store include a store receipt in another directory, \_MASReceipt. Since 2018, when Apple introduced notarization, the ‘ticket’ issued by Apple can be ‘stapled’ into the app bundle as the file CodeResources.

Many apps come with additional items that might in the past have been installed by them in their Library/Application Support folders and elsewhere, but are now included in the app bundle. These can include the following directories:

* Library, containing folders of LaunchDaemons and LoginItems that would previously have been installed in either the main Library folder, or that in the user’s Home folder;
* XPCServices, for executable code that the app uses to provide specific services;
* Plugins, for some types of app extension (Appex);
* Extensions, for other types of app extension, including app intents.

You may also come across other components, including a version.plist in Apple’s apps.

This centralisation of components in the app bundle has brought several benefits. Being self-contained, apps are easier to install and update, and cleaner to remove. Their components are less likely to go missing, and most of all they’re held within the protection of the app’s signature and notarisation, an important improvement in security.

Assembling these into a diagram shows how the anatomy of an app has grown over the last few years.

[![](https://eclecticlight.co/wp-content/uploads/2025/12/anatomyofapp26.jpg)](https://eclecticlight.co/wp-content/uploads/2025/12/anatomyofapp26.jpg)

Components shown in pale yellow are either mandatory or essentially universal. Those shown in green are found in apps distributed through the App Store, while that shown in blue is the stapled notarisation ticket (optional). You will also see additional folders and components such as Automator workflows, scripts, and others.

There is no difference in structure between apps built for current Intel and Arm architectures. That’s because binaries in the MacOS folder (and executable code in other directories like Frameworks, XPCServices and Plugins) contain platform-specific code in a single Mach-O executable. Thus, an app that’s Universal and runs native on both architectures includes code for both in its single ‘fat’ code file, and they even have separate signatures stored within common files.

### Share this:

* [Click to share on X (Opens in new window)
  X](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=twitter)
* [Click to share on Facebook (Opens in new window)
  Facebook](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=facebook)
* [Click to share on Reddit (Opens in new window)
  Reddit](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=reddit)
* [Click to share on Pinterest (Opens in new window)
  Pinterest](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=pinterest)
* [Click to share on Threads (Opens in new window)
  Threads](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=threads)
* [Click to share on Mastodon (Opens in new window)
  Mastodon](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=mastodon)
* [Click to share on Bluesky (Opens in new window)
  Bluesky](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?share=bluesky)
* Click to email a link to a friend (Opens in new window)
  Email
* [Click to print (Opens in new window)
  Print](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#print?share=print)

Like Loading...

### *Related*

Posted in [Macs](https://eclecticlight.co/category/macs/), [Technology](https://eclecticlight.co/category/technology/) and tagged [bundles](https://eclecticlight.co/tag/bundles/), [Classic Mac](https://eclecticlight.co/tag/classic-mac/), [Mac OS X](https://eclecticlight.co/tag/mac-os-x/), [macOS](https://eclecticlight.co/tag/macos/), [signature](https://eclecticlight.co/tag/signature/). Bookmark the [permalink](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/).

## 7Comments

[Add yours](#reply-title)

1. 1
   ![Alan B's avatar](https://0.gravatar.com/avatar/603ec9af7b0e3677640e02617caa42d22c1dc45d07c3a315bd8c0b445041d40c?s=96&d=identicon&r=G)

   Alan B
   [on December 4, 2025 at 7:58 am](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110076)

   [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110076#respond)

   Another excellent article :) When I was first introduced to software development, we talked about program(me)s not applications or even apps ;-)

   [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110076&_wpnonce=bdbf56c7e8)Liked by 2 people

   * 2
     ![hoakley's avatar](https://2.gravatar.com/avatar/87cc8acbb0b96b7d3b699e69cb1b0ce2bfa22c19f3737ddc73a81783fc2af50c?s=96&d=identicon&r=G)

     [hoakley](https://eclecticlightdotcom.wordpress.com)
     [on December 4, 2025 at 9:27 am](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110078)

     [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110078#respond)

     Thank you, Alan.
     Howard.

     [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110078&_wpnonce=ee9edecee7)Like
2. 3
   ![markbot2zero's avatar](https://1.gravatar.com/avatar/4603bf5d7e69a6780206360899ef6f92f3ed903574c2b8936fe83d6f614b009b?s=96&d=identicon&r=G)

   markbot2zero
   [on December 4, 2025 at 4:17 pm](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110082)

   [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110082#respond)

   Yes, excellent and interesting article, thank you Howard.

   re: “Applications”

   (I can’t remember where I read this (maybe “The Devil’s DP Dictionary,” by Stan Kelly-Bootle, 1981, also known as “The Computer Contradictionary”) but back in the benighted pre-PC/Mac days of hulking mainframes, someone defined “Applications” thusly:)

   “Annoying user software that interferes with testing and tuning the OS, and wastes CPU cycles.”

   Other examples from the Wikipedia page:

   `Endless loop. See: Loop, endless`

   `Loop, endless. See: Endless loop`

   `Recursion. See: Recursion`

   -Mark

   [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110082&_wpnonce=5ef04a0952)Liked by 1 person

   * 4
     ![hoakley's avatar](https://2.gravatar.com/avatar/87cc8acbb0b96b7d3b699e69cb1b0ce2bfa22c19f3737ddc73a81783fc2af50c?s=96&d=identicon&r=G)

     [hoakley](https://eclecticlightdotcom.wordpress.com)
     [on December 4, 2025 at 8:28 pm](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110087)

     [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110087#respond)

     Haha! Nice ones.
     Howard.

     [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110087&_wpnonce=ddb2e0f936)Like
3. 5
   ![Bryan Christianson's avatar](https://1.gravatar.com/avatar/a3a65b2263d2912f0b195c820b2b3cb67dff34ff1bb46208bfbb976a7bd77507?s=96&d=identicon&r=G)

   Bryan Christianson
   [on December 4, 2025 at 7:43 pm](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110085)

   [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110085#respond)

   In my mainframe days an application consisted of a database with many programs (several hundred) making queries and updates to the database, either from programs running as background (batch) processes or via an interactive terminal.

   Standalone executables were called programs. The modern usage of “app” as a contraction of application still grates but I’ve learnt to live with it.

   [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110085&_wpnonce=cb71838b40)Liked by 1 person

   * 6
     ![hoakley's avatar](https://2.gravatar.com/avatar/87cc8acbb0b96b7d3b699e69cb1b0ce2bfa22c19f3737ddc73a81783fc2af50c?s=96&d=identicon&r=G)

     [hoakley](https://eclecticlightdotcom.wordpress.com)
     [on December 4, 2025 at 8:33 pm](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110090)

     [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110090#respond)

     Thank you, Bryan. I too find *app* inadequate.
     Howard.

     [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110090&_wpnonce=40728301fd)Like
4. 7
   ![joethewalrus's avatar](https://1.gravatar.com/avatar/d58afcd589f06898f74c537d15a7a8261cdd3a4f0c08b8f247c1cbbe8230fbcc?s=96&d=identicon&r=G)

   joethewalrus
   [on December 5, 2025 at 9:18 am](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comment-110094)

   [Reply](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?replytocom=110094#respond)

   As a child in the early 90s, in a strictly Mac environment, I was taught that disks contained folders, and folders contained “Application Programs” and “Documents.” Application Program, being long, was abbreviated to “Application,” but never App.

   Thank you, Mark, for adding the word benighted to my vocabulary. What a delicious word!

   [Like](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/?like_comment=110094&_wpnonce=7b0aa52427)Liked by 1 person

### Leave a comment [Cancel reply](/2025/12/04/the-anatomy-of-a-macos-app/#respond)

Δ

# Quick Links

* [Free Software Menu](https://eclecticlight.co/free-software-menu/)
* [System Updates](https://eclecticlight.co/system-updates/)
* [M-series Macs](https://eclecticlight.co/m1-macs/)
* [Mac Troubleshooting Summary](https://eclecticlight.co/mac-troubleshooting-summary/)
* [Mac problem-solving](https://eclecticlight.co/mac-problem-solving/)
* [Painting topics](https://eclecticlight.co/painting-topics/)
* [Painting](https://eclecticlight.co/category/painting/)
* [Long Reads](https://eclecticlight.co/long-reads/)

# Search

Search for:

# Monthly archives

* [December 2025](https://eclecticlight.co/2025/12/) (16)
* [November 2025](https://eclecticlight.co/2025/11/) (74)
* [October 2025](https://eclecticlight.co/2025/10/) (75)
* [September 2025](https://eclecticlight.co/2025/09/) (78)
* [August 2025](https://eclecticlight.co/2025/08/) (76)
* [July 2025](https://eclecticlight.co/2025/07/) (77)
* [June 2025](https://eclecticlight.co/2025/06/) (74)
* [May 2025](https://eclecticlight.co/2025/05/) (76)
* [April 2025](https://eclecticlight.co/2025/04/) (73)
* [March 2025](https://eclecticlight.co/2025/03/) (78)
* [February 2025](https://eclecticlight.co/2025/02/) (67)
* [January 2025](https://eclecticlight.co/2025/01/) (75)
* [December 2024](https://eclecticlight.co/2024/12/) (74)
* [November 2024](https://eclecticlight.co/2024/11/) (73)
* [October 2024](https://eclecticlight.co/2024/10/) (78)
* [September 2024](https://eclecticlight.co/2024/09/) (77)
* [August 2024](https://eclecticlight.co/2024/08/) (75)
* [July 2024](https://eclecticlight.co/2024/07/) (77)
* [June 2024](https://eclecticlight.co/2024/06/) (71)
* [May 2024](https://eclecticlight.co/2024/05/) (79)
* [April 2024](https://eclecticlight.co/2024/04/) (75)
* [March 2024](https://eclecticlight.co/2024/03/) (81)
* [February 2024](https://eclecticlight.co/2024/02/) (72)
* [January 2024](https://eclecticlight.co/2024/01/) (78)
* [December 2023](https://eclecticlight.co/2023/12/) (79)
* [November 2023](https://eclecticlight.co/2023/11/) (74)
* [October 2023](https://eclecticlight.co/2023/10/) (77)
* [September 2023](https://eclecticlight.co/2023/09/) (77)
* [August 2023](https://eclecticlight.co/2023/08/) (72)
* [July 2023](https://eclecticlight.co/2023/07/) (79)
* [June 2023](https://eclecticlight.co/2023/06/) (73)
* [May 2023](https://eclecticlight.co/2023/05/) (79)
* [April 2023](https://eclecticlight.co/2023/04/) (73)
* [March 2023](https://eclecticlight.co/2023/03/) (76)
* [February 2023](https://eclecticlight.co/2023/02/) (68)
* [January 2023](https://eclecticlight.co/2023/01/) (74)
* [December 2022](https://eclecticlight.co/2022/12/) (74)
* [November 2022](https://eclecticlight.co/2022/11/) (72)
* [October 2022](https://eclecticlight.co/2022/10/) (76)
* [September 2022](https://eclecticlight.co/2022/09/) (72)
* [August 2022](https://eclecticlight.co/2022/08/) (75)
* [July 2022](https://eclecticlight.co/2022/07/) (76)
* [June 2022](https://eclecticlight.co/2022/06/) (73)
* [May 2022](https://eclecticlight.co/2022/05/) (76)
* [April 2022](https://eclecticlight.co/2022/04/) (71)
* [March 2022](https://eclecticlight.co/2022/03/) (77)
* [February 2022](https://eclecticlight.co/2022/02/) (68)
* [January 2022](https://eclecticlight.co/2022/01/) (77)
* [December 2021](https://eclecticlight.co/2021/12/) (75)
* [November 2021](https://eclecticlight.co/2021/11/) (72)
* [October 2021](https://eclecticlight.co/2021/10/) (75)
* [September 2021](https://eclecticlight.co/2021/09/) (76)
* [August 2021](https://eclecticlight.co/2021/08/) (75)
* [July 2021](https://eclecticlight.co/2021/07/) (75)
* [June 2021](https://eclecticlight.co/2021/06/) (71)
* [May 2021](https://eclecticlight.co/2021/05/) (80)
* [April 2021](https://eclecticlight.co/2021/04/) (79)
* [March 2021](https://eclecticlight.co/2021/03/) (77)
* [February 2021](https://eclecticlight.co/2021/02/) (75)
* [January 2021](https://eclecticlight.co/2021/01/) (75)
* [December 2020](https://eclecticlight.co/2020/12/) (77)
* [November 2020](https://eclecticlight.co/2020/11/) (84)
* [October 2020](https://eclecticlight.co/2020/10/) (81)
* [September 2020](https://eclecticlight.co/2020/09/) (79)
* [August 2020](https://eclecticlight.co/2020/08/) (103)
* [July 2020](https://eclecticlight.co/2020/07/) (81)
* [June 2020](https://eclecticlight.co/2020/06/) (78)
* [May 2020](https://eclecticlight.co/2020/05/) (78)
* [April 2020](https://eclecticlight.co/2020/04/) (81)
* [March 2020](https://eclecticlight.co/2020/03/) (86)
* [February 2020](https://eclecticlight.co/2020/02/) (77)
* [January 2020](https://eclecticlight.co/2020/01/) (86)
* [December 2019](https://eclecticlight.co/2019/12/) (82)
* [November 2019](https://eclecticlight.co/2019/11/) (74)
* [October 2019](https://eclecticlight.co/2019/10/) (89)
* [September 2019](https://eclecticlight.co/2019/09/) (80)
* [August 2019](https://eclecticlight.co/2019/08/) (91)
* [July 2019](https://eclecticlight.co/2019/07/) (95)
* [June 2019](https://eclecticlight.co/2019/06/) (88)
* [May 2019](https://eclecticlight.co/2019/05/) (91)
* [April 2019](https://eclecticlight.co/2019/04/) (79)
* [March 2019](https://eclecticlight.co/2019/03/) (78)
* [February 2019](https://eclecticlight.co/2019/02/) (71)
* [January 2019](https://eclecticlight.co/2019/01/) (69)
* [December 2018](https://eclecticlight.co/2018/12/) (79)
* [November 2018](https://eclecticlight.co/2018/11/) (71)
* [October 2018](https://eclecticlight.co/2018/10/) (78)
* [September 2018](https://eclecticlight.co/2018/09/) (76)
* [August 2018](https://eclecticlight.co/2018/08/) (78)
* [July 2018](https://eclecticlight.co/2018/07/) (76)
* [June 2018](https://eclecticlight.co/2018/06/) (77)
* [May 2018](https://eclecticlight.co/2018/05/) (71)
* [April 2018](https://eclecticlight.co/2018/04/) (67)
* [March 2018](https://eclecticlight.co/2018/03/) (73)
* [February 2018](https://eclecticlight.co/2018/02/) (67)
* [January 2018](https://eclecticlight.co/2018/01/) (83)
* [December 2017](https://eclecticlight.co/2017/12/) (94)
* [November 2017](https://eclecticlight.co/2017/11/) (73)
* [October 2017](https://eclecticlight.co/2017/10/) (86)
* [September 2017](https://eclecticlight.co/2017/09/) (92)
* [August 2017](https://eclecticlight.co/2017/08/) (69)
* [July 2017](https://eclecticlight.co/2017/07/) (81)
* [June 2017](https://eclecticlight.co/2017/06/) (76)
* [May 2017](https://eclecticlight.co/2017/05/) (90)
* [April 2017](https://eclecticlight.co/2017/04/) (76)
* [March 2017](https://eclecticlight.co/2017/03/) (79)
* [February 2017](https://eclecticlight.co/2017/02/) (65)
* [January 2017](https://eclecticlight.co/2017/01/) (76)
* [December 2016](https://eclecticlight.co/2016/12/) (75)
* [November 2016](https://eclecticlight.co/2016/11/) (68)
* [October 2016](https://eclecticlight.co/2016/10/) (76)
* [September 2016](https://eclecticlight.co/2016/09/) (78)
* [August 2016](https://eclecticlight.co/2016/08/) (70)
* [July 2016](https://eclecticlight.co/2016/07/) (74)
* [June 2016](https://eclecticlight.co/2016/06/) (66)
* [May 2016](https://eclecticlight.co/2016/05/) (71)
* [April 2016](https://eclecticlight.co/2016/04/) (67)
* [March 2016](https://eclecticlight.co/2016/03/) (71)
* [February 2016](https://eclecticlight.co/2016/02/) (68)
* [January 2016](https://eclecticlight.co/2016/01/) (90)
* [December 2015](https://eclecticlight.co/2015/12/) (96)
* [November 2015](https://eclecticlight.co/2015/11/) (103)
* [October 2015](https://eclecticlight.co/2015/10/) (119)
* [September 2015](https://eclecticlight.co/2015/09/) (115)
* [August 2015](https://eclecticlight.co/2015/08/) (117)
* [July 2015](https://eclecticlight.co/2015/07/) (117)
* [June 2015](https://eclecticlight.co/2015/06/) (105)
* [May 2015](https://eclecticlight.co/2015/05/) (111)
* [April 2015](https://eclecticlight.co/2015/04/) (119)
* [March 2015](https://eclecticlight.co/2015/03/) (69)
* [February 2015](https://eclecticlight.co/2015/02/) (54)
* [January 2015](https://eclecticlight.co/2015/01/) (39)

# Tags

[APFS](https://eclecticlight.co/tag/apfs/)
[Apple](https://eclecticlight.co/tag/apple/)
[Apple silicon](https://eclecticlight.co/tag/apple-silicon/)
[backup](https://eclecticlight.co/tag/backup/)
[Big Sur](https://eclecticlight.co/tag/big-sur/)
[Blake](https://eclecticlight.co/tag/blake/)
[Bonnard](https://eclecticlight.co/tag/bonnard/)
[bug](https://eclecticlight.co/tag/bug/)
[Catalina](https://eclecticlight.co/tag/catalina/)
[Consolation](https://eclecticlight.co/tag/consolation/)
[Console](https://eclecticlight.co/tag/console/)
[Corinth](https://eclecticlight.co/tag/corinth/)
[Delacroix](https://eclecticlight.co/tag/delacroix/)
[Disk Utility](https://eclecticlight.co/tag/disk-utility/)
[Doré](https://eclecticlight.co/tag/dore/)
[El Capitan](https://eclecticlight.co/tag/el-capitan/)
[extended attributes](https://eclecticlight.co/tag/extended-attributes/)
[Finder](https://eclecticlight.co/tag/finder/)
[firmware](https://eclecticlight.co/tag/firmware/)
[Gatekeeper](https://eclecticlight.co/tag/gatekeeper/)
[Gérôme](https://eclecticlight.co/tag/gerome/)
[High Sierra](https://eclecticlight.co/tag/high-sierra/)
[history of painting](https://eclecticlight.co/tag/history-of-painting/)
[iCloud](https://eclecticlight.co/tag/icloud/)
[Impressionism](https://eclecticlight.co/tag/impressionism/)
[landscape](https://eclecticlight.co/tag/landscape/)
[LockRattler](https://eclecticlight.co/tag/lockrattler/)
[log](https://eclecticlight.co/tag/log/)
[M1](https://eclecticlight.co/tag/m1/)
[Mac](https://eclecticlight.co/tag/mac/)
[Mac history](https://eclecticlight.co/tag/mac-history/)
[macOS](https://eclecticlight.co/tag/macos/)
[macOS 10.12](https://eclecticlight.co/tag/macos-10-12/)
[macOS 10.13](https://eclecticlight.co/tag/macos-10-13/)
[macOS 10.14](https://eclecticlight.co/tag/macos-10-14/)
[macOS 10.15](https://eclecticlight.co/tag/macos-10-15/)
[macOS 11](https://eclecticlight.co/tag/macos-11/)
[macOS 12](https://eclecticlight.co/tag/macos-12/)
[macOS 13](https://eclecticlight.co/tag/macos-13/)
[macOS 14](https://eclecticlight.co/tag/macos-14/)
[macOS 15](https://eclecticlight.co/tag/macos-15/)
[malware](https://eclecticlight.co/tag/malware/)
[Metamorphoses](https://eclecticlight.co/tag/metamorphoses/)
[Mojave](https://eclecticlight.co/tag/mojave/)
[Monet](https://eclecticlight.co/tag/monet/)
[Monterey](https://eclecticlight.co/tag/monterey/)
[Moreau](https://eclecticlight.co/tag/moreau/)
[myth](https://eclecticlight.co/tag/myth/)
[narrative](https://eclecticlight.co/tag/narrative/)
[OS X](https://eclecticlight.co/tag/os-x/)
[Ovid](https://eclecticlight.co/tag/ovid/)
[painting](https://eclecticlight.co/tag/painting-2/)
[performance](https://eclecticlight.co/tag/performance/)
[Pissarro](https://eclecticlight.co/tag/pissarro/)
[Poussin](https://eclecticlight.co/tag/poussin/)
[privacy](https://eclecticlight.co/tag/privacy/)
[Renoir](https://eclecticlight.co/tag/renoir/)
[riddle](https://eclecticlight.co/tag/riddle/)
[Rubens](https://eclecticlight.co/tag/rubens/)
[Sargent](https://eclecticlight.co/tag/sargent/)
[security](https://eclecticlight.co/tag/security/)
[Sierra](https://eclecticlight.co/tag/sierra/)
[SilentKnight](https://eclecticlight.co/tag/silentknight/)
[Sonoma](https://eclecticlight.co/tag/sonoma/)
[SSD](https://eclecticlight.co/tag/ssd/)
[Swift](https://eclecticlight.co/tag/swift/)
[Time Machine](https://eclecticlight.co/tag/time-machine/)
[Tintoretto](https://eclecticlight.co/tag/tintoretto/)
[Turner](https://eclecticlight.co/tag/turner/)
[update](https://eclecticlight.co/tag/update/)
[upgrade](https://eclecticlight.co/tag/upgrade/)
[Ventura](https://eclecticlight.co/tag/ventura/)
[xattr](https://eclecticlight.co/tag/xattr/)
[Xcode](https://eclecticlight.co/tag/xcode/)
[XProtect](https://eclecticlight.co/tag/xprotect/)

# Statistics

* 20,706,914 hits

[Blog at WordPress.com.](https://wordpress.com/?ref=footer_blog)

##### Footer navigation

* [Free Software Menu](https://eclecticlight.co/free-software-menu/)
* [About & Contact](https://eclecticlight.co/about)
* [Macs](https://eclecticlight.co/category/macs/)
* [Painting](https://eclecticlight.co/category/painting/)
* [Downloads](https://eclecticlight.co/downloads/)
* [Mac problem-solving](https://eclecticlight.co/mac-problem-solving/)
* [Extended attributes (xattrs)](https://eclecticlight.co/extended-attributes-xattrs/)
* [Painting topics](https://eclecticlight.co/painting-topics/)
* [SilentKnight, Skint, SystHist, silnite, LockRattler & Scrub](https://eclecticlight.co/lockrattler-systhist/)
* [DelightEd & Podofyllin](https://eclecticlight.co/delighted-podofyllin/)
* [xattred, SpotTest, Spotcord, Metamer & xattr tools](https://eclecticlight.co/xattred-sandstrip-xattr-tools/)
* [32-bitCheck & ArchiChect](https://eclecticlight.co/32-bitcheck-archichect/)
* [XProCheck, T2M2, LogUI, Ulbow, blowhole and log utilities](https://eclecticlight.co/consolation-t2m2-and-log-utilities/)
* [Cirrus & Bailiff](https://eclecticlight.co/cirrus-bailiff/)
* [Precize, Alifix, UTIutility, Sparsity, alisma, Taccy, Signet](https://eclecticlight.co/taccy-signet-precize-alifix-utiutility-alisma/)
* [Versatility & Revisionist](https://eclecticlight.co/revisionist-deeptools/)
* [Text Utilities: Textovert, Nalaprop, Dystextia and others](https://eclecticlight.co/text-utilities-nalaprop-dystextia-and-others/)
* [PDF](https://eclecticlight.co/pdf/)
* [Keychains & Permissions](https://eclecticlight.co/keychains-permissions/)
* [Updates](https://eclecticlight.co/category/updates/)
* [Spundle, Cormorant, Stibium, DropSum, Dintch, Fintch and cintch](https://eclecticlight.co/dintch/)
* [Long Reads](https://eclecticlight.co/long-reads/)
* [Mac Troubleshooting Summary](https://eclecticlight.co/mac-troubleshooting-summary/)
* [M-series Macs](https://eclecticlight.co/m1-macs/)
* [Mints: a multifunction utility](https://eclecticlight.co/mints-a-multifunction-utility/)
* [VisualLookUpTest](https://eclecticlight.co/visuallookuptest/)
* [Virtualisation on Apple silicon](https://eclecticlight.co/virtualisation-on-apple-silicon/)
* [System Updates](https://eclecticlight.co/system-updates/)
* [Saturday Mac Riddles](https://eclecticlight.co/saturday-mac-riddles/)
* [Last Week on My Mac](https://eclecticlight.co/last-week-on-my-mac/)
* [sysctl information](https://eclecticlight.co/sysctl-information/)

##### Secondary navigation

* Search

# Post navigation

[The Dutch Golden Age: Jan Miense Molenaer](https://eclecticlight.co/2025/12/03/the-dutch-golden-age-jan-miense-molenaer/)

[Reading Visual Art: 237 Blacksmith](https://eclecticlight.co/2025/12/04/reading-visual-art-237-blacksmith/)

Search for:

Begin typing your search above and press return to search. Press Esc to cancel.

* [Comment](https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/#comments)
* Reblog
* Subscribe
  Subscribed

  + [![](https://eclecticlight.co/wp-content/uploads/2015/01/cropped-eclecticlightlogo-e1421784280911.png?w=50) The Eclectic Light Company](https://eclecticlight.co)

  Join 8,840 other subscribers

  Sign me up

  + Already have a WordPress.com account? [Log in now.](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Feclecticlight.co%252F2025%252F12%252F04%252Fthe-anatomy-of-a-macos-app%252F)
* + [![](https://eclecticlight.co/wp-content/uploads/2015/01/cropped-eclecticlightlogo-e1421784280911.png?w=50) The Eclectic Light Company](https://eclecticlight.co)
  + Subscribe
    Subscribed
  + [Sign up](https://wordpress.com/start/)
  + [Log in](https://wordpress.com/log-in?redirect_to=https%3A%2F%2Fr-login.wordpress.com%2Fremote-login.php%3Faction%3Dlink%26back%3Dhttps%253A%252F%252Feclecticlight.co%252F2025%252F12%252F04%252Fthe-anatomy-of-a-macos-app%252F)
  + [Copy shortlink](https://wp.me/p5CIcf-njD)
  + [Report this content](https://wordpress.com/abuse/?report_url=https://eclecticlight.co/2025/12/04/the-anatomy-of-a-macos-app/)
  + [View post in Reader](https://wordpress.com/reader/blogs/83108039/posts/89629)
  + [Manage subscriptions](https://subscribe.wordpress.com/)
  + Collapse this bar

##

##

Loading Comments...

Write a Comment...

Email (Required)

Name (Required)

Website

###

%d

![](https://pixel.wp.com/b.gif?v=noscript)

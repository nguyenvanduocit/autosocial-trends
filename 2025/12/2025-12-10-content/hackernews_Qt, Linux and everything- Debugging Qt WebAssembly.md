---
source: hackernews
title: Qt, Linux and everything: Debugging Qt WebAssembly
url: http://qtandeverything.blogspot.com/2025/12/debugging-qt-webassembly-dwarf.html
date: 2025-12-10
---

# [Qt, linux and everything](http://qtandeverything.blogspot.com/)

thoughts from a developer.
Author of Hands-on Mobile and Embedded Development with Qt 5 http://bit.ly/HandsOnMobileEmbedded

## Tuesday, December 9, 2025

### Debugging Qt WebAssembly - DWARF

One of the most tedious tasks a developer will do is debugging a nagging bug. It's worse when it's a web app, and even worse when its a webassembly web app.

The easiest way to debug Qt Webassembly is by configuring using the -g argument, or CMAKE\_BUILD\_TYPE=Debug . [Emscripten](https://emscripten.org/docs/porting/Debugging.html#debugging-dwarf) embeds DWARF symbols in the wasm binaries.

**NOTE:** Debugging wasm files with DWARF works **only** in the Chrome browser with the help of a browser extension.

[C/C++ DevTools Support (DWARF)](https://goo.gle/wasm-debugging-extension) browser extension. If you are using Safari or Firefox, or do not want to or cannot install a browser extension, you will need to generate source maps, which I will look at in my next blog post.

### DWARF debugging

You need to also enable DWARF in the browser developer tools settings, but you do not need symlinks to the source directories, as you would need to using source maps, as the binaries are embedded with the full directory path. Like magic!

 Emscripten embeds DWARF symbols into the binaries built with -g by default, so re-building Qt or your application in debug mode is all you need to do.

Qt builds debug libraries by default using the optimized argument -g2, which produces less debugging info, but results in faster link times. To preserve debug symbols you need to build Qt debug using the -g or -g3 argument. Both of these do the same thing.

### Using DWARF debugger

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6e5dEWIzlOGSPzJk9uUX1nDg_LZW4NdA-vXRDNP8YHhwKZf-3qj9BY7sijyNAzqSA0Zw3nYGQTgBtsDDZhzkP52UZWmnU2sd3XkFwuBQ9qCS67CtPEMcoSI9bN6dDYWmszENc5wuq548ATAR6mscpAoTQ8No31gOefQT-KU4uoGk4z-Iiga67AdODzOw/w640-h632/Screenshot%202025-12-09%20at%209.18.11%E2%80%AFam.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh6e5dEWIzlOGSPzJk9uUX1nDg_LZW4NdA-vXRDNP8YHhwKZf-3qj9BY7sijyNAzqSA0Zw3nYGQTgBtsDDZhzkP52UZWmnU2sd3XkFwuBQ9qCS67CtPEMcoSI9bN6dDYWmszENc5wuq548ATAR6mscpAoTQ8No31gOefQT-KU4uoGk4z-Iiga67AdODzOw/s2606/Screenshot%202025-12-09%20at%209.18.11%E2%80%AFam.png)

Open Chome with the extention mentioned before installed, and open the console tools. Navigate to the Qt for WebAssembly web application you need to debug. Once it opens, it may take a few seconds for all the symbols and files to get parsed. If you are debugging into Qt, this will take quite a few seconds - just keep waiting.

The javascript console will soon contain file source file paths and sources. You can find your file to debug, and set breakpoints. Just reload the page and once it hits a breakpoint, it will stop execution and highlight the current line in the source view.  It also will show variable names and values.

You can then step though your code as you would debugging a desktop application.

Posted by

[deviceguy](https://www.blogger.com/profile/08211459741197454860 "author profile")

at
[10:23 AM](http://qtandeverything.blogspot.com/2025/12/debugging-qt-webassembly-dwarf.html "permanent link")

[![](https://resources.blogblog.com/img/icon18_edit_allbkg.gif)](https://www.blogger.com/post-edit.g?blogID=2997739907109253354&postID=2539162574495956459&from=pencil "Edit Post")

#### No comments:

[Post a Comment](https://www.blogger.com/comment/fullpage/post/2997739907109253354/2539162574495956459)

[Older Post](http://qtandeverything.blogspot.com/2023/07/qt6-webassembly-qtmultimedia-or-how-to.html "Older Post")
[Home](http://qtandeverything.blogspot.com/)

Subscribe to:
[Post Comments (Atom)](http://qtandeverything.blogspot.com/feeds/2539162574495956459/comments/default)

## cool things

* [Trolltech Labs](http://labs.trolltech.com/)

## Blog Archive

* ▼
  [2025](http://qtandeverything.blogspot.com/2025/)
  (1)
  + ▼
    [December](http://qtandeverything.blogspot.com/2025/12/)
    (1)
    - [Debugging Qt WebAssembly - DWARF](http://qtandeverything.blogspot.com/2025/12/debugging-qt-webassembly-dwarf.html)

* ►
  [2023](http://qtandeverything.blogspot.com/2023/)
  (1)
  + ►
    [July](http://qtandeverything.blogspot.com/2023/07/)
    (1)

* ►
  [2022](http://qtandeverything.blogspot.com/2022/)
  (2)
  + ►
    [March](http://qtandeverything.blogspot.com/2022/03/)
    (1)
  + ►
    [January](http://qtandeverything.blogspot.com/2022/01/)
    (1)

* ►
  [2021](http://qtandeverything.blogspot.com/2021/)
  (6)
  + ►
    [August](http://qtandeverything.blogspot.com/2021/08/)
    (1)
  + ►
    [July](http://qtandeverything.blogspot.com/2021/07/)
    (1)
  + ►
    [June](http://qtandeverything.blogspot.com/2021/06/)
    (2)
  + ►
    [April](http://qtandeverything.blogspot.com/2021/04/)
    (1)
  + ►
    [January](http://qtandeverything.blogspot.com/2021/01/)
    (1)

* ►
  [2020](http://qtandeverything.blogspot.com/2020/)
  (6)
  + ►
    [July](http://qtandeverything.blogspot.com/2020/07/)
    (1)
  + ►
    [May](http://qtandeverything.blogspot.com/2020/05/)
    (1)
  + ►
    [March](http://qtandeverything.blogspot.com/2020/03/)
    (2)
  + ►
    [January](http://qtandeverything.blogspot.com/2020/01/)
    (2)

* ►
  [2019](http://qtandeverything.blogspot.com/2019/)
  (6)
  + ►
    [August](http://qtandeverything.blogspot.com/2019/08/)
    (2)
  + ►
    [July](http://qtandeverything.blogspot.com/2019/07/)
    (1)
  + ►
    [June](http://qtandeverything.blogspot.com/2019/06/)
    (1)
  + ►
    [May](http://qtandeverything.blogspot.com/2019/05/)
    (2)

* ►
  [2017](http://qtandeverything.blogspot.com/2017/)
  (4)
  + ►
    [November](http://qtandeverything.blogspot.com/2017/11/)
    (1)
  + ►
    [June](http://qtandeverything.blogspot.com/2017/06/)
    (1)
  + ►
    [May](http://qtandeverything.blogspot.com/2017/05/)
    (1)
  + ►
    [January](http://qtandeverything.blogspot.com/2017/01/)
    (1)

* ►
  [2016](http://qtandeverything.blogspot.com/2016/)
  (2)
  + ►
    [December](http://qtandeverything.blogspot.com/2016/12/)
    (1)
  + ►
    [August](http://qtandeverything.blogspot.com/2016/08/)
    (1)

* ►
  [2014](http://qtandeverything.blogspot.com/2014/)
  (1)
  + ►
    [February](http://qtandeverything.blogspot.com/2014/02/)
    (1)

* ►
  [2012](http://qtandeverything.blogspot.com/2012/)
  (2)
  + ►
    [November](http://qtandeverything.blogspot.com/2012/11/)
    (1)
  + ►
    [October](http://qtandeverything.blogspot.com/2012/10/)
    (1)

* ►
  [2011](http://qtandeverything.blogspot.com/2011/)
  (1)
  + ►
    [May](http://qtandeverything.blogspot.com/2011/05/)
    (1)

* ►
  [2010](http://qtandeverything.blogspot.com/2010/)
  (1)
  + ►
    [June](http://qtandeverything.blogspot.com/2010/06/)
    (1)

* ►
  [2009](http://qtandeverything.blogspot.com/2009/)
  (2)
  + ►
    [August](http://qtandeverything.blogspot.com/2009/08/)
    (1)
  + ►
    [March](http://qtandeverything.blogspot.com/2009/03/)
    (1)

* ►
  [2008](http://qtandeverything.blogspot.com/2008/)
  (5)
  + ►
    [August](http://qtandeverything.blogspot.com/2008/08/)
    (1)
  + ►
    [June](http://qtandeverything.blogspot.com/2008/06/)
    (2)
  + ►
    [April](http://qtandeverything.blogspot.com/2008/04/)
    (1)
  + ►
    [March](http://qtandeverything.blogspot.com/2008/03/)
    (1)

* ►
  [2007](http://qtandeverything.blogspot.com/2007/)
  (8)
  + ►
    [December](http://qtandeverything.blogspot.com/2007/12/)
    (1)
  + ►
    [August](http://qtandeverything.blogspot.com/2007/08/)
    (2)
  + ►
    [July](http://qtandeverything.blogspot.com/2007/07/)
    (5)

## About Me

[![My photo](//blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2QRqgGBgT7Qp4285fpPkWxk9M5_xT6p_vPhYdIOse15Uyy-R0kBlAwfWtm6En20W8TzCSeTn_GJzFhgaY_qVNgLJch2Eg5-w0BFJ14G-krUe3cUVouKpPJpC6YaGv1A/s220/75591_10153802769108664_8547998207620669077_n+%281%29.jpg)](https://www.blogger.com/profile/08211459741197454860)

[deviceguy](https://www.blogger.com/profile/08211459741197454860)

[View my complete profile](https://www.blogger.com/profile/08211459741197454860)

Simple theme. Theme images by [gaffera](https://www.istockphoto.com/googleimages.php?id=4072573&platform=blogger&langregion=en_US). Powered by [Blogger](https://www.blogger.com).

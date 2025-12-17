---
source: hackernews
title: Creating custom yellow handshake emojis with zero-width joiners
url: https://blog.alexbeals.com/posts/custom-yellow-handshake-emojis-with-zero-width-joiners
date: 2025-12-17
---

[![Lone Redwood](/themes/folium/tree_small.png)
Vox Silva](/)

Toggle navigation

* [Posts](/posts "Main Posts")
* [List](/list "List")
* [Snippets](/snippets "Snippets")
* [Subscribe](/subscribe "Subscribe for post notifications")

# Creating custom yellow handshake emojis with zero-width joiners

December 15, 2025

CONTENTS⤴︎

How they work

Creating new handshakes

Apple added support for multi-skin tone handshake emojis in 2022, allowing you to go from the old standby of 🤝 to supporting handshakes like 🫱🏻‍🫲🏿. However while these handshake emojis look similar,
1
I'm not sure these look similar for *you*. To try to make this writeup render consistently I added an [emoji rendering fallback font](https://github.com/samuelngs/apple-emoji-linux) but this may not fix all platforms (and may break others). To toggle that override you can click this button
 they're actually substantially different.

## How they work

Each character (and therefore each emoji) is made up of Unicode codepoints. We can look at the codepoints that make up an emoji by splitting it apart character by character using JavaScript.

```
[...'🤝'].map(c => 'U+' + c.codePointAt(0).toString(16)).join(' ')
```

🤝

U+1F91D

🫱🏻‍🫲🏿

U+1FAF1 U+1F3FB U+200D U+1FAF2 U+1F3FF

From this we can see that the yellow handshake is only one codepoint for the original emoji. The modified color handshake however is actually a composite, composed of five different characters:

* U+1FAF1 - 🫱 (Right-facing hand)
* U+1F3FB -  🏻 (Fitzpatrick
  2
  The color modifier names come from the [Fitzpatrick scale](https://en.wikipedia.org/wiki/Fitzpatrick_scale), a human skin color phenotypal scale created by an American dermatologist in the 1970s (a scale that is often critiqued for its eurocentric bias).
   1-2
  3
  This is "1-2" because while there *are* 6 categories in the scale, 1 and 2 are so similar that they are combined into the same skin color for emojis (see aforementioned point on bias).
  )
* U+200D - ZWJ (Zero-width joiner)
* U+1FAF2 - 🫲 (Left-facing hand)
* U+1F3FF -  🏿 (Fitzpatrick 6)

The ZWJ character is the real star of the show, gluing together disparate emojis into something that different sites and fonts can choose to render as a single grapheme (backing emojis like 👩‍❤️‍💋‍👨
4
👩 + ❤ + 💋 + 👨
, though flags like 🇺🇳
5
The way the flags are encoded leads to one of my favorite weird JS behaviors:
`'🇺🇸🇨🇺'.replace('🇸🇨', '🇦🇷') == '🇺🇦🇷🇺'`
Can you figure out why? It's because of country ISO codes: those are the United States and Cuban flags, or US CU. Swapping the Seychelles (SC) for Argentina (AR) yields UA RU, or Ukraine and Russia.
 work differently).

## Creating new handshakes

Knowing how these are constructed allows us to break out of Apple's UI. Their keyboard lets us pick either the uniformly yellow emoji, or build a hand of two colors. But what if instead of white and black we want yellow and black?

[![](/images/custom-yellow-handshake-emojis-with-zero-width-joiners/2.png)](/images/custom-yellow-handshake-emojis-with-zero-width-joiners/2.png)

Instead of adding skin modifiers to both hands we can only modify one of them.
6
Or none of them, creating 🫱‍🫲, which is visually identical to 🤝 and yet composed of three codepoints instead of one.
 Some simple JavaScript will then build it back into a single character, allowing us to create two emoji that Apple will render but not let you create.
7
Unfortunately these emojis don't render as large single messages on iOS/macOS. I *think* this is because Apple thinks it's text rather than emojis, but I'm not sure (or what emoji rendering requirement it might be failing).

```
console.log('\u{1FAF1}\u{200D}\u{1FAF2}\u{1F3FF}')
```

🫱‍🫲🏿

U+1FAF1 U+200D U+1FAF2 U+1F3FF

🫱🏿‍🫲

U+1FAF1 U+1F3FF U+200D U+1FAF2

---

1. I'm not sure these look similar for *you*. To try to make this writeup render consistently I added an [emoji rendering fallback font](https://github.com/samuelngs/apple-emoji-linux) but this may not fix all platforms (and may break others). To toggle that override you can click this button [↩︎](#fnref1:similar)
2. The color modifier names come from the [Fitzpatrick scale](https://en.wikipedia.org/wiki/Fitzpatrick_scale), a human skin color phenotypal scale created by an American dermatologist in the 1970s (a scale that is often critiqued for its eurocentric bias). [↩︎](#fnref1:fitzpatrick)
3. This is "1-2" because while there *are* 6 categories in the scale, 1 and 2 are so similar that they are combined into the same skin color for emojis (see aforementioned point on bias). [↩︎](#fnref1:1-2)
4. 👩 + ❤ + 💋 + 👨 [↩︎](#fnref1:kiss)
5. The way the flags are encoded leads to one of my favorite weird JS behaviors:
   `'🇺🇸🇨🇺'.replace('🇸🇨', '🇦🇷') == '🇺🇦🇷🇺'`
   Can you figure out why? It's because of country ISO codes: those are the United States and Cuban flags, or US CU. Swapping the Seychelles (SC) for Argentina (AR) yields UA RU, or Ukraine and Russia. [↩︎](#fnref1:flag)
6. Or none of them, creating 🫱‍🫲, which is visually identical to 🤝 and yet composed of three codepoints instead of one. [↩︎](#fnref1:none)
7. Unfortunately these emojis don't render as large single messages on iOS/macOS. I *think* this is because Apple thinks it's text rather than emojis, but I'm not sure (or what emoji rendering requirement it might be failing). [↩︎](#fnref1:rendering)

Post Comment

### Next snippet

[Remove unused characters from fonts to save on sizeDecember 15, 2025](/posts/remove-unused-characters-font-size "Remove unused characters from fonts to save on size")

### Previous snippet

[Showing keyboard strokes on macOSDecember 9, 2025](/posts/showing-keyboard-strokes-on-macos "Showing keyboard strokes on macOS")

### Recent snippets

[Remove unused characters from fonts to save on sizeDecember 15, 2025](/posts/remove-unused-characters-font-size "Remove unused characters from fonts to save on size")[Creating custom yellow handshake emojis with zero-width joinersDecember 15, 2025](/posts/custom-yellow-handshake-emojis-with-zero-width-joiners "Creating custom yellow handshake emojis with zero-width joiners")[Showing keyboard strokes on macOSDecember 9, 2025](/posts/showing-keyboard-strokes-on-macos "Showing keyboard strokes on macOS")[Enabling Touch ID for sudoDecember 9, 2025](/posts/touch-id-for-sudo "Enabling Touch ID for sudo")[Removing your location from Mailchimp emailsDecember 7, 2025](/posts/removing-your-location-from-mailchimp-emails "Removing your location from Mailchimp emails")[**View all snippets**](/snippets "View all snippets")[**View all posts**](/list "View all posts")

© 2025 Vox Silva - All rights reserved.

* [RSS](/feeds/rss)
* [Admin area](/admin "Manage your site!")
* [Home](/ "Return to my website!")

×

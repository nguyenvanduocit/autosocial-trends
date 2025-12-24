---
source: hackernews
title: Fifty problems with standard web APIs in 2025
url: https://zerotrickpony.com/articles/browser-bugs/
date: 2025-12-24
---

# Fifty problems with standard web APIs in 2025

[Zero Trick Pony](https://messydesk.social/%40zerotrickpony) - December 2025

Recently, I made a [free game](https://intelligencegame.tech).
It's a little mystery investigation game which I was inspired to write after enjoying games like
[Case of the Golden Idol](https://en.wikipedia.org/wiki/The_Case_of_the_Golden_Idol),
[Roottrees are Dead](https://en.wikipedia.org/wiki/The_Roottrees_are_Dead), and
[Return of the Obra Dinn](https://en.wikipedia.org/wiki/Return_of_the_Obra_Dinn).
I thought my UX ideas were pretty simple, so I decided to implement it in **HTML5**.

[![](./game-screenshot.jpg "A screenshot from the game Intelligence, linked below.")](./game-screenshot.jpg)

I know, I know: games should be made with game engines. But listen: The game is mostly text. It has
a dirt simple single-page ("SPA") type layout, static images, a few fonts, a few sounds, and that's it.
No need for a backing server, no 3D graphics, no complex animations, and no particularly novel interactions.
Just a single page load, and then clicking and reading.

I figured it should be easy to make this game (app, really) work on multiple browsers and devices,
so that players could access it without a native install. The web has supported these basic functions for over
a decade. Surely in the year 2025, I thought, **HTML5** is a good choice for these simple needs.
It's the future now!

What really happened was, I hit **over 50 surprising problems** related to gaps in web standards, requiring
me to **spend over half of the total development time on rework** for cross-browser and cross-device
support. In this article I'd like to list the problems I hit, as an illustration of what a novice web developer
faces on the modern web today.
I'll give some simple advice on reducing rework costs, and conclude with some points on what
web standards in 2025 do and don't do for us.

## Surely this is my fault

![](./tombstone.jpg "A tombstone inscribed with the words 'I was told this would be easy'")

I know, I know: new features are always being added to browsers. There's always going to be some "wet paint" in
web standards. If I make a web project that depends on the newest features, it's not going to work everywhere.
You might be thinking that in order to hit 50+ cross-browser problems, I must have used the hottest, newest,
most experimental web APIs, and that was all my fault.

The common wisdom is that if only I restrain myself to the "safe harbor" of the oldest, most time-tested parts of
the standard web, then I can rest assured that my web page will work on any "modern" browser and device.

But sadly, no.

Unfortunately some of the most buggy, glitchy, and/or divergent behaviors I found were in 20+ year old standard web features.
It's not (just) the new stuff. I was surprised to find **no clear safe harbor feature set** for
building polished, interactive web-based projects, even if they are pretty simple.
Even with other projects that were barely interactive, I've hit fairly important platform and browser variances,
and needed at least some rework for multiple browsers.

Even so, I imagine that an experienced web developer could read the list of problems below and nod to themselves knowingly:
"sure, these are all common pitfalls. That's why we have frameworks that polyfill many of these problems, and
experienced designers and frontend engineers who know all the best practices."

Perhaps that's true, but my point is: should it be? If a "standard platform" requires a giant device testing lab and a
priesthood of finger-wagging frontend druids with trauma response for engineering intuition, is this "standard"
really helping us a lot? Even if web standards don't let us live the dream of "write once, run everywhere", shouldn't
it at least be predictable where the problems are going to be?

## The novice web developer experience

The way I ended up hitting all these nasty surprises was not as naive as it may sound. I am not a novice
web developer; I am, unhappily, one of the druids. I did not naively blunder into cross-browser pitfalls,
I'm perfectly well aware that huge browser differences exist (spoiler: mobile) and need design consideration,
and testing.

But I did accidentally simulate the naive web developer experience with my game project, because of
how I was prototyping. In game development, the core game mechanics have to be fun or else the rest of the game is
not worth making. So I whipped up a prototype wireframe in HTML5 to get a feel for core puzzle mechanics, purposely
disregarding cross platform considerations. I willfully ignored phones and tablets and Safari, and iterated just on
my own MacBook with only one browser.

Only later, after the wireframe game mechanics had been refined with playtesting, did I decide to polish the
prototype into a cross-browser game that also worked well on mobile. If you work at a giant company you wouldn't
work like this; in fact you probably wouldn't get to design the UX *at all*, because you'd receive the proposed
interactions from a paid UX designer.

But a new developer working alone probably would do this? And why shouldn't they? It *should* be reasonable to get
an idea working on one's own computer first, and not be surprised later by massive rework on a second device/browser.

So, where are we on that in 2025? To find out, I compiled all the browser bugs and rework tasks from my game
project into this list.

## TLDR: buy an old iPhone and test with it daily

If you don't read the rest of this rant, I can sum up my advice as just this: if you're making a web project, even
a simple one, do your rapid, many-times-a-day iteration loop testing on an **older iPhone** as your test mule.
Yes this is a pain, because none of us are programming on an iPhone soft keyboard. We're sitting at a computer
or laptop, and so that's the platform it's most natural to iterate on. Most frontend tools do not make it easy to
have a quick edit-and-reload cycle with a real mobile device.
So you'll have to either frequently push to a private web server, or use some
[exotic ssh tunnel](https://apps.apple.com/us/app/ssh-tunnel-with-socks5-proxy/id1260223542) contraption,
to get it so that your test mule iPhone can view your test web project.

This is not because iPhones are good, **it's because they're bad.**
iPhones are more peculiar and less compliant than any other device I tried. I promise that if you get your
web project looking good and working smoothly on a crappy iPhone, your
residual costs to test and polish on all other platforms and browsers will be fairly low. (For extra bravery,
I recommend **Firefox iOS** instead of Safari, because it is the most buggy, least compliant browser
I was able to find. If it works on Firefox iOS it's going to work anywhere. See below.)

Yes all the desktop browsers have some form of "design for mobile" mode, and that's better than nothing.
But these tools will not reveal 80% of the problems I've described below. Not even the Safari responsive design
mode. Not even the XCode device simulator.

If you'd like to hear more, then let's start with some gleeful indignation at one of the biggest culprits
in web development:

# Culprit #1: Apple has opinions

Writing this with fresh bruises from my iOS testing lab, I feel like it's fair to say that Apple's
mobile devices (iPhone, iPad) wilfully and unapologetically diverge from the rest of the web more than any other
platform. Safari has a few bugs and is the slowest to support any given web API. But this section is not about
Safari's **bugs** and **sandbagging**; more on that below.
This section is just about Safari's **strong opinions**. Here are few issues I hit:

## Can't use the whole screen

In a game, and arguably in any well designed UX, the application wants to use all the available screen space.
The design should neither waste space on unintended gutters or dead pixels, nor should it confuse the user
by letting important information fall below the fold where the user will have to scroll to get at it,
or will be confused because they won't notice it. I want to simply fill the whole canvas but not overspill it,
and this is comically difficult on iOS.

[![](./browser-bar-resize-screenshot.jpg)](./browser-bar-resize.mp4 "Link to a video showing the iPad browser bar resizing")

**Mandatory automatic resizing:** First, the navigation bar at the top of the screen will appear and dissappear depending on the user's most recent
gesture. There is no event your application can receive to know this is happening, nor any measurement you can take
to detect it.
[surprising problem #1]

The [standard CSS features `vw` and `vh` units](https://caniuse.com/?search=vh) are broken by this
behavior, so trying to create a whole-page user experience will break on iOS Safari.

**Broken CSS units:** Experienced web developers have [tried complicated tricks](https://css-tricks.com/the-trick-to-viewport-units-on-mobile/)
to work around this resizing behavior. Later, [new units were introduced](https://caniuse.com/?search=svh) to CSS to try to give
the web page more control over this. And although newer Safari iOS versions support them,
[Firefox iOS still doesn't.](https://github.com/mozilla-mobile/firefox-ios/issues/11574)
[#2]

**Anti-competitive non-standards:** Relatedly, there is a [Fullscreen API](https://developer.mozilla.org/en-US/docs/Web/API/Element/requestFullscreen#browser_compatibility) in the web standards, which every browser supports... except Safari iPhone.
[3]
The [community hypothesizes that fullscreen is purposely hobbled](https://developer.apple.com/forums/thread/133248) on iOS
so that Apple can sell more native iOS games without web-based games being a viable competitor. When I considered
using the fullscreen web standard to give my game better use of the screen, I was thwarted by this.

**Useless fullscreen behavior:** The Fullscreen API is
[*supposed* to work on iPad](https://developer.mozilla.org/en-US/docs/Web/API/Element/requestFullscreen#browser_compatibility),
and I wasted time trying to offer it to my users. But it has
three ridiculous problems: First, it replaces the top of the screen with a black area and an irremovable "X"
which doesn't actually reclaim any more space than the address bar would have.
[4]

Second, if the user *scrolls down* in any element anywhere on the page, full-screen mode ends.
[5]

![](./ios-fullscreen-keyboard-error.jpg "Error message from Apple explaining why Safari refuses to show the keyboard in full screen mode.")

Third, Safari will *refuse to summon the soft keyboard* if fullscreen is currently on.
Newer versions of Safari simply exit fullscreen, but in iOS 12 I encountered this hilarious error dialog informing the user
that Apple believes the web page is phishing them. (!)
[6]
Since my game had both scrolling elements and occasionally needed the keyboard,
I gave up in disgust so now I simply hide the fullscreen button on iOS.

**Workarounds:** My solutions were to (a) give up on the Fullscreen API on iOS, (b) use `position:fixed` and some cleverness with
CSS transforms to fill the screen at least horizontally, and (c) to blindly add in some height fudge factors to try to stay below
the unknowable threshold of vertical scrolling on these browsers. My game now uses *almost* the whole screen on iOS.

## Touch screen problems

Apple's mobile browser is perfectly okay for scrolling to read articles and tapping hyperlinks. But for my
game I needed to design more interactive elements. Although I tried to keep it simple, I was still hit
by a number of Apple-specific surprises:

**Dragging does what:** Dragging with the mouse on a desktop browser will drag any `draggable` elements, or
do text selection otherwise. But on Safari iOS, dragging a finger will only scroll.
[7]
Even if the touched area isn't scrollable, and even if the finger lands on a `draggable` element, swiping gestures only
scroll. If the user does a compound press-and-hold-and-then-drag gesture, then on Android this will drag
the `draggable` element but on iOS it will select text instead.
[8]
I was able to fix this with a liberal sprinkling of `user-select:none` styles on many more elements than Android needed.

**What is scrollable:** In the name of saving space and beautifying the display, Apple defaulted us all to
the "overlay" style of scroll bars. These elements are translucent, narrow, or entirely invisible for most
of their lifetime. The result looks simple and attractive, and next time I want to print out my phone's screen and
hang it on my wall as art, I'll thank them for that. The rest of the time, this makes it impossible for the
user to discern which areas of the screen are scrollable. This is probably a perfectly reasonable tradeoff
decision for when the browser is displaying a wikipedia article about raccoons. But for anything more interactive,
the inability for the user to discover what is scrollable is pretty bad. I had to add on-screen glinting elements
to draw the user's attention to the otherwise invisible scrolling areas.
[9]

[![](./tap-test-screenshot.jpg "The hit target test on iPad")](./hittest_bug_ipad.mp4)

**Apple's finger location algorithm:** I assigned `click` Event listeners to various on-screen objects, some of which were
`<button>` but many of which were `<div>` elements. My testing revealed that Android Chrome will deliver
a click event if the user's finger touches most/all of the area of the element. If the finger hits multiple
elements, Chrome Android delivers the event to the one closest to its own proprietary idea of the perceptual center
of the finger touch. When I tested my game board UX scaled *way* down for a tiny mobile screen, I was pleasantly
surprised to find that the user can [still tap them somewhat successfully](./hittest_bug_android.mp4) even if they're visually small...
[but not on iPad](./hittest_bug_ipad.mp4).

On Safari iOS with a scaled down UI, the user has to get the *center* of their finger to hit
the element or else the click event won't be delivered. [10]
This made my game effectively unplayable on iOS because the playing field had 10px hit targets which were too
small to tap. I fixed it by creating a large invisible `<div>` over every dot on the field, to catch touch input on iOS.

## Can't play audio

My game has both sound effects and background music. Sound was exotic on the web in 2004, but it's long since been
standardized... supposedly. Not counting composing the sounds in Garage Band, I got these working
on my laptop's browser in under ten minutes. Then getting them to work on iPad and iPhone took me another
week of effort.

**AudioContext always sounds silent:** My first try at sound effects used an [AudioContext](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext#browser_compatibility) to wire up Gain and Oscillator nodes to play game beeps. These features are
"fully supported" on iOS, but no sound would be produced. [11]
I eventually learned that if you use these APIs
to make sound, [iOS considers them to be "ringtones"](https://stackoverflow.com/questions/40789136/ios-ringer-switch-mutes-web-audio)
rather than "media", which means that the user's
media volume control will have no effect on the game's sounds. The user must enter their "Settings" app and
change their device's ring volume. The normal hard buttons for volume up and down on an iOS device control
media volume, not ring volume. I despaired of instructing any user to do this.

**Audio tag play() does not function:** Next I moved to using `<audio/>` tags to play pre-recorded sounds in response
to in-game events. I created six `HTMLAudioElement` objects for my five kinds of sound effect, and one
more to play the background music. However, the `.play()` method, which is marked on
[MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play#browser_compatibility)
as "fully supported" and "widely available", does not function at all on iOS.
[12]

You see, Apple is of the opinion that `.play()` method should only work if it's
[in response to a user action](https://stackoverflow.com/questions/66300840/trying-to-play-an-mp3-via-js-on-ios-browsers),
which they implement by checking if the user has tapped an element on the page. Since a game
has story events for the user to *react* to, no such sound effects could work. I worked around it (see below)
but it made the system very complicated.

**Audio tags skip the first 300ms of sound after the first play:** Safari will play MP3s through the `HTMLAudioElement` ---
*once*. But after each tag plays once, Safari aggressively unloads the media, I presume to save memory.
[13]
I think this is reasonable, *except* that then when my sound effect plays a second time, the audio has
been unloaded and Safari plays silence until it can get the MP3 data back into its buffers again. I would
actually have been fine with this behavior if it were to wait to play until the audio was loaded, but it
doesn't do that. Safari is of the opinion that it's more important to keep up with the scheduled play
of the sound *than to hear it*. So it non-deterministically jumps into the middle of the sound, cutting off the beginning.
This made all my sound effects seem glitchy and truncated.

**Audio volume not controllable:** I created a volume control for both music and sound effects in the game,
in case the user wanted to mute them or make them quieter. I did this by setting the `.volume` property
on the Audio tags, and this turned out to be broken on iOS.
[14]
(To their credit, MDN lists the `.volume` property of media tags as
["limited availability"](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/volume#browser_compatibility).)
Apple simply says "volume is under the user's control",
and does not try to make the thing work. Apple [documents its behavior](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/Using_HTML5_Audio_Video/Device-SpecificConsiderations/Device-SpecificConsiderations.html#//apple_ref/doc/uid/TP40009523-CH5-SW1)
like this:

`Reading the volume property always returns 1`

This would be a handy behavior to at least write some feature detection logic around, but sadly it's not true.
[15]
The actual behavior is that if you modify the `.volume` property
of an `HTMLAudioElement` then it *will* seem to change, but (a) no actual volume change will occur, and
(b) at the next event loop idle, the volume value will be set back to `1`. This wasted an hour of my time
to isolate the true behavior and write complicated feature detection logic to check that mutation of the `.volume`
property survives past the next idle loop.

**Workarounds:** The [recommended workaround](https://stackoverflow.com/questions/31776548/why-cant-javascript-play-audio-files-on-iphone-safari)
for the anti-autoplay behavior is to create a little invisible farm of dummy `HTMLAudioElement` objects
which play silent sounds when the user taps anything on the screen. I inserted a "start" screen into my
game when the page loads, which has no purpose except to provide the user with something to tap before
the game begins.

Once the user taps, I can have the farm of elements play their silent sounds once. Then the programmer is
free to rewrite their `src` attribute to play other media at any time.
This works because the constraint to play() from within a user action event handler apparently only pertains to the first
use of play on a per-tag basis.
[16]
So after writing about 200 lines of priming logic, I had a bank of
sound effects working. (And can I just say: if this workaround works for me, it works for bad actors too.
So Apple's mitigation adds complexity but doesn't actually protect users as intended. Awesome!)

To work around the cut-off sound effect problem, I re-recorded all my sound effect MP3s in Garage Band to
start with 300ms of silence. This created an apparent lag in the responsiveness of the game, but at least the user
could hear the entire sound.

To make mute work, I had to change my code to actually stop the audio instead of setting volume to zero.
I also had to write 20 lines of behavior probing code to detect the non-functioning `.volume` property
and offer iOS users an "ON"/"OFF" switch without the gradiated volume controls.

# Culprit #2: Behaviors and bugs

Although a lot of the behavior differences I've described above are annoying opinions, there are definitely also some
more forgivable but still annoying variances in behavior within the standards. And also, good old browser bugs.

## Layout problems

### Fonts vary by platform, even when they shouldn't

[![](./fonts-comparison.jpg "A side by side comparison of Courier, Arial, and Monospace on Linux and Mac")](./fonts-comparison.jpg)

**Font names aren't reliable:**
I had a lot of small layout glitches caused by browser differences in choice of font, even fonts that could easily be
exactly the same on all platforms like `Arial` and `Courier`.
[17]
I fixed these by using custom `woff2` fonts loaded
at game start. In a normal web application I think it's arguable that the application should set a specific font,
but in a game where the fonts are part of the aesthetic, this is table stakes.

**Aside: woff2 is great:**
On the bright side, I was pleasantly surprised at how consistent layout can be across platforms when using `woff2` fonts
to prevent font fallback.
(And `display:grid` is also very consistent, more on that later.)
These were well supported even on my worst browser (Firefox iOS) and my second worst browser (Safari on an old iOS 12
device I had lying around). It is *almost* attainable to have pixel-perfect consistency across platforms with just
these two features. I had not realized how much fonts (and localization, if your project has that) are such a large
source of layout ambiguity.

(Yes yes, expecting pixel-accurate layout across devices is unreasonable. Yes yes make your designs responsive
so that they'll gracefully handle any shape and size of screen and font setting. But since I'm making a *game*,
the interaction design had a hard requirement of certain things being on the same screen relative to each other.
The normal vaguaries of the web would result in broken interactions. My responsive compromise was to define
five specific layout sizes and snap to each of those based on font and zoom settings. This made it easier to
trust that a playtest of the game would work for a given size of screen.)

**Incorrect ligature defaults on Safari iOS:**
I used Google's very clever [Material design icons](https://fonts.google.com/icons) to save myself a lot of
time drawing and flowing simple icons in the app. It's implemented with a kooky abuse of the ligature feature
of custom font faces, which... weird! But okay! Unfortunately even when pasting the Google-provided code
snippet verbatim, Safari's defaults for the `font-ligature` property are not to spec, so Google's icons don't
work on older Safari iOS.
[18]
I was able to fix this easily just by explicitly setting the property, but it
wasted about an hour of my time debugging that.

### Safari iOS: late to every party

Although all the browsers adopt new features in their own time, it is my unscientific sense that Safari is
unusually slow. And it had a few more bugs, as well. To wit:

**Unsupported flex gaps:**
There are a lot of situations where I prefer the mental model of `display:flex` over `display:grid`, but this
caused a number of cross-browser unpleasant surprises. First, Safari was unreasonably late to the party on
[correctly supporting `row-gap` and `column-gap` with `flex`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/column-gap#browser_compatibility),
even though they do what you expect with `grid` as far back as iOS 12.
[19]

**Flex shrink problems:**
Safari chooses to aggressively shrink text in `flex` layouts in situations where other browsers do
not. Specifically it seemed to be related to when a `flex-grow` area was marked as scrollable. Other browsers
would let the elements before and after the scrollable item size naturally, but Safari truncates them.
[20]
So after a lot of layout testing I had to sprinkle `flex-shrink:0` in a bunch of places. Ugh.

**Missing support for lh units:**
I had used the `lh` length unit in some of my layouts, not realizing that Safari didn't add support for this
[until last year](https://caniuse.com/?search=lh+unit).
[21]

**Missing support for scale syntax:**
In all browsers I tested except Safari iOS 12, you can say `transform:scale(50%)` but this does not work on
Safari. I had to change these to `transform:scale(0.5)`.
[22]

**Missing support for ResizeObserver:**
Safari iOS now supports `ResizeObserver` for reacting to element size changes, but it was last to do so. To
support older browsers I had to write some nasty timer-based measurement code.
[23]

**Bugs in CSS custom properties:**
I tried to set the `background-image` of my game using a `var(--my-background-url)` CSS custom property.
This seemed to mostly work, but in my testing I found that if the custom property changes, Safari will not
propagate it correctly to the `BODY:backdrop` element and will cause a missing background bug in full screen.
(I was also able to reproduce this on Safari MacOS, but no other MacOS browsers.)
[24]

**Late support for smooth scrolling:**
I had code in my game to programmatically scroll an element during the tutorial, to show the user that
it can be scrolled. I set the `scroll-behavior:smooth` CSS property so that the user's attention would be
drawn to the animating motion of the scrolling area. Older versions of Safari iOS disregarded this directive,
making the element pop instantaneously instead of visibly scrolling. This doesn't seem like a failure,
but without it my tutorial failed to teach the user something important.
[25]

**Button spacing:**
Safari, and only Safari, adds a bunch of padding to their `<button>` elements which you have to style
away if you care how big your buttons are.
[26]
(Maybe they do this to compensate for their hit target choices?)
Although the agent stylesheet is allowed to contain anything
it wants, in practice this can disrupt the intended layout. I didn't catch it until I did cross-browser testing.

### meta viewport incantations

![](./metaviewport-differences.jpg "Two screenshots side by side showing that without a meta viewport directive, text will be much smaller.")

Single-page application designs that want to work on mobile will need to sprinkle attributes into the `meta`
header tag in order to persuade mobile browsers to scale elements predictably:

`<meta name="viewport" content="width=device-width, height=device-height, initial-scale=1">`

Without these directives, browsers apply various opinionated fudge factors to try to guess what the web
page *intended* while disregarding how the web page is actually styled.
Viewport control directives are
very useful and worth understanding... but without them, desktop browsers tend to do about the same thing,
and mobile browsers do quite different things.
[27]

### Firefox iOS has no idea where the edges of the screen are

Firefox iOS wins the award for least competent browser in this area:

**Broken CSS units:** Firefox iOS has a [bug](https://github.com/mozilla-mobile/firefox-ios/issues/11574)
which prevents use of the special `svh` and `lvh` units which are themselves workarounds to the
self-hiding address bar behavior that all the mobile browsers have.
[28]

**Occluded layout on iPhone:** Newer iOS devices lack a physical home button, instead consuming the
bottom edge of the touch screen with a line-shaped affordance which the user is supposed to know to
swipe or touch depending on their intention. Applications are supposed to either stay out of that area
or draw transparently behind it. Firefox does neither! It simply draws your web page with some of the
bottom edge cut off, but only on these newer iOS devices.
[29]
Safari doesn't have this problem.

**Crash on fullscreen:** Firefox iOS overrides the *requestFullScreen* function with an agent script...
but that agent script has a [bug](https://github.com/mozilla-mobile/firefox-ios/issues/30540)
so that if you call it, you just get a Javascript error. Cool.
[30]

### Mobile browsers have crazy behavior when you pinch zoom

To try to fill the screen on all platforms I admittedly made a fragile choice: I used `transform:scale`
to ensure that the main screen element was centered and sized. This worked like a charm on all the
desktop browsers. And it *almost* worked on all the mobile browsers too. Except that if the user
used pinch zoom in and then out again, the browser would draw the screen with a few pixels at the top
missing.
[31]
The more times the user did this, the more pixels would be missing.
[32]
My application was not receiving any resize, drag, or scroll events when this happened, and the measurements of the screen
as reported by `offsetTop`, `scrollTop`, etc were all unchanged. I spent many hours trying to find
a cause or workaround for this problem.

Eventually I created an isolated test case, and fixed the problem by adding a `position:fixed;top:0;left:0;`
style to the root element. There is **no** standards-based reason why that should have
fixed the problem, but it did.

## Animation and graphics woes

Games need more precisely controlled animations and visuals than is typical for a Wikipedia page
about raccoons. This caused me more than a few problems:

**Text selection styling:**
It is technically possible to style the selected text using the `::selection` pseudoclass, and I tried
to use this to create an animating typewriter effect in my game. Sadly,
[this is not supported on Safari iOS](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/%3A%3Aselection#browser_compatibility)
so I had to rework the animation to work by modifying the DOM in a Javascript loop. Inelegant but functional.
[33]

**Blur bugs:**
I used `filter:blur()` and `backdrop-filter:blur()` CSS properties in various places to
create glow and frosted glass effects for the game's visuals. These *sometimes* worked in Safari,
but were broken in certain situations, like on `<svg>` elements.
[34]
Safari was also late to support this
and needed a `-webkit-backdrop-filter:blur()` variation to get the visual effect to appear
on older tablets.

**Sync animations:**
The `animation` CSS property is surprisingly excellent, a much needed alternative to the often
useless `transition` system, especially on browsers that don't yet support the just-added `content-visibility`
property. (There, I said something nice about web standards.)
There's a [Javascript API](https://developer.mozilla.org/en-US/docs/Web/API/Animation)
to manipulate and restart animations so that (for example) blinking warning lights can be sync'd
up with each other. As usual my 2019 iPad was just old enough to not support this.
[35]
I didn't bother with any workaround, so on older tablets this remains a glitch.

## Audio problems

Apple's audio opinions described above were the biggest problem with audio, but Apple wasn't the only culprit:

**Error on play interruption:**
Chrome, and only Chrome, is of the opinion that it's a bug to `.pause()` or assign `.currentTime` on an audio
element while a previous `.play()` request is "in progress", whatever that means. The other browsers will simply
fulfill your request, but not Chrome. Chrome generates an unhandled promise rejection which looks like a game
crash when the audio pauses.
[36]
So I had to write some complicated guarding logic to protect the audio widgets
from concurrent manipulation during their sensitive moments.

**Popping and skipping sounds:**
Firefox on both MacOS and Android cannot seem to just run an `OscillatorNode` competently. Before I switched away from `AudioContext`,
I found that although Chrome and Safari on MacOS could play my graph of sound effects smoothly, Firefox
introduced "pops" (audio artifacts caused by incorrectly reproducing a sound wave) and missing sounds.
[37]
I was able to mostly fix these by adding more attenuation ramps and silence at the start and end of sounds,
but I definitely shouldn't have to. Avoiding mathematical errors that cause popping sounds should be
table stakes for any piece of software that produces audio. You guys.

**Safari iOS late to support AudioContext:**
`AudioContext` is documented as [fully supported](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext#browser_compatibility)
since forever, but you have to read the fine print! The function has been in Safari iOS
for many years, but only if you call it via the special `webkitAudioContext`.
[38]
It was only renamed to match the other browsers a couple years ago. So that wasted a half hour of my
time to chase down.

**iPad GainNode timings inaccurate:** I found that Safari on iOS 12 does not honor linear gain ramps the way other browsers do.
[39]
So even once my sound effects were audible,
they sounded like strange whooping slide whistles on iPad even though they sounded correct on Android and MacOS.
I can't tell if this is a bug or an opinion, but either way, it was unacceptable. These two problems caused me
to abandon the `AudioContext` API entirely. (My iOS 17 device doesn't have this behavior, so I guess Safari agreed
that it was a bug.)

## Other interactions

**Drag and drop control:** My drag-and-drop Safari woes were not only caused by Apple's strong opinions. I found that I had quite
a few touch screen bugs caused by varying interpretations of CSS and HTML standards. For example, I was
using `inert=true` to prevent interactions, but older Safari browsers ignore this attribute.
[40]
I had to also add `user-select:none; -webkit-user-select:none; pointer-events:none;` in various places to get
Safari to stop showing my users unwanted text selection areas when dragging was wanted.

**Draggable sprites:** I did get dragging to work on Safari iOS, but I found that if the `draggable` element is a `<span>`
which wraps to the next line, then Safari will garble the visual effect of the dragged sprite as
it moves on the screen.
[41]
I ended up reworking the draggable elements to ensure that they could never have a line break.

**Clipboard API:** I used the clipboard API for a simple import/export game feature, since it is documented as
[fully supported](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard#browser_compatibility)
on all major browsers. As usual Safari iOS was the last to add support, and I found that my old iPad
was *just* old enough to fail with this API.
[42]

## Microsoft Edge: copying Chrome's homework real hard

The only browser I tested which needed (almost) no rework was Microsoft Edge on Windows 10.

![](./scrollbar-unstyled.jpg "The same screenshot but with darker themed scrollbars.")

### Styling for traditional scrollbars

The one problem I observed with Edge was scrollbar styling.
On Windows 10 the default scroll bar type is more likely to be "traditional" than
"overlay", so I happened to notice that glaring white scrollbars were shown in my dark-themed space game.
[43]
This situation can arise on any OS, not just Windows, because "always show scrollbars" is a user-facing accessibility setting on
several platforms.

I fixed this by styling darker scrollbars for all browsers using the newly
available `scrollbar-color` CSS property.
(Except for Safari, [44] which requires a `::-webkit-scrollbar` pseudoclass, of course.)

Other than this issue, Microsoft Edge was impressively trouble-free.
It passed my full test suite on the first try, and had no other platform-specific problems.
I attribute this not particularly to the skill of Microsoft, but that my game was already very well tested on
Chrome MacOS. Edge seems not to have diverged much at all from the Chrome ancestry from which was is forked.

# Culprit #3: Mobile Redesign

Zero web druids will be surprised to hear me say that the biggest source of pain with designing a polished,
interactive HTML5 application is the difference in browser behavior between **phones and tablets** vs desktop computers.
Of course within the priesthood we consider "responsive design" to be table stakes. But taking a step back
from our craft for a moment: is it so crazy for a novice developer to expect that they could design a
web-based project on their laptop and see it work fine on a phone with no changes? Why can't this be a reality?

Early on in the popularization of touch screens, manufacturers like Apple made some reasonable but aggressive
decisions to permanently fracture the desktop and mobile browsing experiences.
I don't think these were bad decisions! When a user has a small touch screen, the most efficient interactions for them aren't
the same as when they have a very large display, a mouse or trackpad, and a hard keyboard.
But it did double the size of the design space, and theoretically, this could have been avoided.

Here are some mobile-specific snags I hit while cosplaying a novice web developer:

## Tapping and touching

**Dragging does what, part 2:** You would think that dragging would work easily on touch screens,
and I did sort of expect that my game's drag-and-drop interactions would port nicely to mobile.
Except no! Because mobile browsers have reserved dragging for themselves. Vertical swipe,
edge-swipe, and other finger-drag type inputs
command the browser to scroll, navigate, and select text. Those events either don't reach the web
page's elements at all, or they are delivered as an overload with other interactions.
[45]
Various CSS hints like `pointer-events:none;` and `user-select:none` must be sprinkled into the page
to control these interactions more carefully.

**Surprise zoom:** One silly example is double-tapping. The `<button>` element which was designed to submit forms
to a server never needs to be repeatedly tapped. But in a game, the user may repeatedly tap to fire a weapon
or move a gamepiece. On mobile, these repeated taps will rather surprisingly zoom in on the
button.
[46]
These elements must be given the magical `touch-action:manipulation;` CSS style to
hint that double-tapping should not be interpreted by the browser. One more thing to know.

**What do you call it?** This is a very small thing, but induced a fair amount of rework for me so
I will mention it here: My game has on-screen instructions which tell the user how to play.
A web page about raccoons would not need to provide instructions saying, "here is how to scroll
down! Here is how to click hyperlinks!" ...but a game does need this! My original prototype
had a lot of "click the arrow to continue" type text, but the word "click" is not intuitive
language for someone using a touch screen. I could have left it in, figuring that most people
could make the deductive leap that anything that could be "clicked" with a mouse they didn't
have could also be "tapped" using their available fingers. Since I wanted the game to feel
as native as possible, I didn't want to leave this in. But I couldn't think of any single
verb that would feel equally intuitive to both mouse and touch users?
[47]
So I added complexity to the game to [detect the presence of a touch screen](https://peterscene.com/blog/detecting-touch-devices-2018-update/),
which is much harder than it sounds, and use the word "click" or "tap" depending on the device.

## Hovering

Not because hovering is the most pivotal interaction, but it's a simple example of a willful divergence:
When the user places their mouse inside a region of the screen, that region's "hover" interactions will trigger.
This can be used for non-essential visual texture such as slightly brightening a button to cue its
interactivity. But it *could* be (and is) used for anything else, like revealing a hidden sub-menu which
is critical to the user's task success.

Arguably, mobile browsers *could have been* designed to give touch screen users a way to use interfaces
designed for a mouse. As a strawman example, Safari for iPhone could show a little crosshair that you move around with
your finger. When the crosshair enters a region of the screen, that region's `:hover` interaction could be
shown, giving the touch screen user the same experience as a mouse user.
But mobile browser designers *purposefully chose* to take away the ability to easily express hovering. I'm not saying
this was a bad choice: interpreting finger motions as immediate actions saves the user time and dexterity
difficulties. Moving a crosshair around might be annoying and difficult.

Apple probably set this standard by being the first to sell an extremely popular touch screen web browser,
so they made this decision for all of us. Right or wrong, this decision means that users can't experience hover
interactions on their iPad. If a developer creates an important hover-based interaction, then they have
introduced an **unsolvable design flaw** into their user interface which they won't discover without a
mobile testing lab.

Is `:hover` a web standard? Supposedly yes.
[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/%3Ahover#browser_compatibility) and
[caniuse](https://caniuse.com/?search=hover) both list it as "widely available" and describe it as "fully supported"
on mobile browsers. Caniuse mentions an iOS Safari glitch, but doesn't say "this will basically never work on mobile"
even though it should.
[48]
MDN is better, with a blue callout warning the developer to consider "accessibility"
on "devices with limited hovering capabilities". This is technically true but undersells the case pretty significantly.
Here's what MDN *should* say:

`> Hover is effectively unusable on the world's most popular browsers and devices.
> Web projects which seek to work consistently for all users should refrain entirely from using this feature.`

The web standards don't say this because web standards aren't trying to create a consistent experience for all
users. There *is* tribal knowledge amongst seasoned professional web developers that hover "shouldn't" be used for
any "critical" interactions like providing the user with instructions, or concealing interactive elements necessary
for primary task completion. But if our novice designer/developer created a hover-based interaction and it worked on their
computer, they will not discover until mobile testing that they have stepped outside the safe harbor, and now
need to majorly rework their design.

## Soft keyboard

There's a bunch of completely reasonable interactions that a novice developer could build without
thinking about the fact that they depend upon the user having a physical keyboard. Most mobile users
have access to only a soft keyboard, which (a) lacks a few capabilities and (b) consumes an exhorbitant
amount of screen space. This screen space is almost always better allocated to something else, which is
why mobile browsers aggressively hide the soft keyboard. Here's some problems that are easy to hit:

### Shift-click

An early prototype of my game accelerated certain interactions with shift-click. But shift-click is not
offered on a mobile device, so these interactions had to be redesigned for mobile.
[49]
[MDN](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/shiftKey) lists the `shiftKey`
property of MouseEvent as "widely supported", even though soft keyboards on mobile will **NOT** deliver
this event with `shiftKey` set to true, ever. Safari at least has the honesty to say that
it's [unsupported on TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent/shiftKey#browser_compatibility).
The rest of the browsers document it as "supported" even though this will 100% not do what
the developer intends.

You could argue that MouseEvent is obviously unsafe on mobile because there is no mouse. But despite the
name, mobile browsers *do* work hard to transparently support the "click" event when the user taps,
and they deliver a `MouseEvent` when the touch happens. There was clearly some attempt to ensure that
`MouseEvent` was part of the "safe harbor" we all wish existed, but it's not a complete attempt.

I hope at this point you're yelling at your screen, "these are accessibility design problems! You can't
expect MDN to protect you from not designing properly!!!" And to that I say --- I hear you, but,
are we sure about that? Consider: One major reason why accessibility is so bad across modern computer interfaces is that
**developers must do something extra to offer accessibility**. But these secondary quality characteristics
will always be, well, secondary. One way we could have ensured that designs are accessible is to
**make it impossible to build anything else.** Instead, we've filled the standard web API with conditional features that
don't work for most people, and then we describe them as "widely supported". We are making this
problem worse when we could be making it better.

### Text input

![](./soft-keyboard-landscape.jpg "A screenshot of the soft keyboard on an Android phone when oriented horizontally.")

The `<input>` tag is like 30 years old, but that has apparently not been enough time for us to figure
out how to make it usable! My original game design had a good amount of typing in it, to let the user input their
guesses to solve a mystery. When I first tested this experience on mobile, I found that the soft keyboard's
consumption of the screen almost always covered up or reflowed an important clue that the user needed
to read as they typed their guess.
[50]
Worse, the game is wide-screen so the user would likely have their
phone tilted to landscape mode. In this orientation, the soft keyboard consumes much more than 50% of the
vertical screen space. What's left is almost entirely consumed by the text input area.
[51]

This seemingly reasonable allotment of space leaves nothing for the clue context. This made the game
unplayable and required me to dramatically rework the design to eliminate typing. I spent more engineering
time on this rework than on any other single change for mobile. If I had prototyped on mobile first, I would
have realized this problem much sooner.

### Input focus

With a hard keyboard device, calling `.focus()` on a likely input element can be an incidental
convenience. It gives the user a very gentle hint at interactivity, and saves them a click if they would
like to start typing. But they can also easily ignore the focused element, and click something else on the
screen if they like.

Soft and hard keyboards both need the unfortunate concept of keyboard focus, usually indicated
by some extra bold outlines and a blinking vertical line which users understand to be a "cursor".
Unlike on a laptop, a soft keyboard also needs a cue for when it is wanted. Showing the soft keyboard when
it is unwanted is an annoyance because of how much screen space it wastes. But ***failing*** to show
the soft keyboard when it is wanted fully prevents primary task completion. They need to type, and cannot.
Disaster.

So I definitely sympathize with mobile browsers that show the soft keyboard on `.focus()` of any input
element. But if any web page does focus an element, the soft keyboard will take over most of the screen.
[52]
The user will be *shut out of all other tasks* until they dismiss the gigantic soft keyboard. This means
that calling `.focus()` should only be done when the input is assured to be the primary task. It cannot
be used incidentally like it can on desktop.

## Responsive layout: yes you have to

A lot of the documentation at large about designing for mobile will mention "responsive design". You could be
forgiven for thinking that the entirety of this concept is just learning how to set up the confusingly named
[media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Media_queries/Using) to conditionalize
a web page's layout. There is a lot of advice about responsive design out there, not all of it wise. So here's
what I think is the heart of it:

### Design for mobile first

Unless your HTML5 project is a static list of raccoon facts shown in a single column of black text on a white
background, it is unlikely that a UX designed for a desktop browser will also be usable on a phone.
[53]
You're more likely to have a phone design look okay on desktop than the other way around. So if you really really don't
want to create two separate designs, you need to iterate your design on a mobile phone first. (Or at the very
least a mobile simulator like is integrated into Chrome and Firefox.)

### You probably need at least two layouts

Web designers these days seem unable to resist cramming the margins of web pages with hamburger
menus, nav bars, chat bots, and ads. None of these will look right on a phone's tall, narrow screen.
[54]
So if you can't refrain
from multi-column layout, you will need to make at least two different layouts: a single-column
layout for phones and then a multi-column layout for desktop browsers.

### No you can't control the real size of anything

CSS has a [wealth of units](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units)
which you might imagine can give you all the options you need to make text and controls the right size on
screens big and small. But these are all [lies](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units#numbers_lengths_and_percentages), even the ones like `cm` and `mm` which sound like they
should, you know, be SI distances like we learned in school. Actually, a CSS `cm` is defined as a certain number of
`px`, and `px` are defined as a certain number of `cm`. How big is a `px` really, then? **You can't know.**
[55]

The actual explanation I like best is buried in the fine print on MDN:

`> Note that 1px doesn't necessarily equal one physical device pixel.
> On HD displays, it may span multiple physical pixels.
> Similarly, 1cm in CSS often doesn't correspond to one hundredth of SI meter.
> On a large TV screen, it typically is longer than that. The lengths are perceptual:
> 16px looks roughly the same on a phone, laptop, or TV screen at typical viewing distance.`

...the "perceptual" distances are determined by the device manufacturer, and then nudged by the
zoom fudge factor from the user's accessibility settings. So you'll have more luck reasoning about
the information density of "screenfuls" of your design, rather than trying to make a hit target the
"natural size" of a real-world object like a book or the size of a hit target for someone's fingertip.
(The latter is a sensible goal but purposely impossible on the web.)

So for example, if you set your smallest readable font size to about `16px` with a `line-height` of `22px`,
then you can (roughly) reason that about ten lines of text will fit on a `220px` high area of the screen.
This is no help at all in determining the *size* of the layout, but it will tell you roughly how much
the user can see at once on a mobile screen. Chrome and Firefox have "responsive design" modes in their
developer tools that you can use to get a sense of this.

# Conclusion

This exercise to write an HTML5 game has generated ample evidence that web standards are doing little to threaten
the thriving occupation of web druid in the year 2025. I realize building a game in vanilla.js is a strange
choice, but I think anyone would hit many of these surprises trying to build any single-page application
design which handled media or had any input interactions. And, I remind you, these were just the problems
that surprised *me*, and I have been doing web development since the 1990s. How much worse would this
be for someone inexperienced?

Blundering into a multi-browser interactive HTML5 project without any thought put towards best
practices, mobile design, or having a testing lab will not result in a functioning UX. For the dream
of "write once run anywhere", HTML5 seems another broken promise. May it rest with the likes of Electron,
Fluttr, Java, Flash, and all the failed abstractions that have come before and since.

On the bright side, HTML5 has a lot to like as a development platform. And it's *much* better than it was even just
5 years ago. No I didn't avoid the need for a testing lab. But it's tremendously helpful to have a *mostly* consistent
set of concepts and documentation that can apply to all targeted platforms. Cross-browser and cross-device
development costs aren't *eliminated*, no. But they are greatly reduced. (**Especially** if you choose your primary
testing device wisely: an old iPhone.)
Compared to, say, the cost of developing a Swift app for iOS and then a Kotlin app for Android, and then
still needing desktop and Windows support after that... HTML5 is great.

Browsers also remain a superior delivery platform vs. native installs for casual and untrusted content.
Users can (mostly) rest easy knowing that your web-based project is not risking their device or
stealing their stuff. And visiting a URL is still less friction than a native install. So HTML5 is
a great choice for projects where friction should be as low as possible.

### Footnote: Should I use HTML5 to make games?

Probably not? The above 50ish problems should give you a flavor of the costs and limitations of trying to
make a polished, interactive experience. A larger reason not to choose HTML5 for game-like
interactions is just that it isn't very capable. You can trick your way into doing a lot with animation
frameworks like [animeJS](https://animejs.com/), or you could do 3D with webGL. But a purpose-built
game framework like Unity would have polyfill to protect you from more of these layout and audio
problems. And unlike with information pages about raccoons, game players seem happy to
receive game applications through the various app stores (Apple, Android, Steam, Playstation)
they've decided to trust.

### Footnote: How could this be better?

It's tempting to feel like a grand technical fix is needed. For the industry to keep developing ever-more
insulating polyfill frameworks which abstract away ever more of animation, media, layout, CSS, and all the
rest of the core web. But I think this is both overkill and unlikely to succeed, because so many have tried and failed at
this already. (Please stop making more frameworks!)

The unglamorous answer is that this might be just a **documentation problem**. [MDN](https://developer.mozilla.org/en-US/)
is pretty good, even though Mozilla increasingly concerns me as a steward of the open web. What if
it were just a little better?

One problem with MDN is that if you're trying to build a truly polished, consistent experience across many
browsers, there's almost more features to avoid than to use. What if you could tick a box in the upper
right corner of the documentation page saying "I want to support all these devices" and then the MDN pages
would change to hide features that won't work, show example code that's already tested on
all the devices you selected, and show you what percentage of users will have success with the set of browsers
you picked? Wouldn't that be nice? (The Android API docs and the Node.js docs have features that go toward this
idea, though they don't do it quite so thoroughly.)

### Footnote: achieving a polished experience

Although I think there's good evidence for a lack of a "safe harbor" *feature set* in HTML5, there *is* a way to
enjoy low development costs and easily use HTML5 to support many devices and browsers:
simply **don't care much about the user experience.** If you don't mind your users hitting glitches,
setbacks, and frustrations as they try to read your text, see your images, and fill out your input forms,
then HTML5 will be very affordable for your project.

Browsers are designed to be the end user's self-service toolkit to combat our bad websites.
Users can override fonts, mute sounds, enlarge text, pinch-zoom in, open images in a separate tab, copy-and-paste, autofill rote form inputs,
switch to "simplified reading mode", search for the same information elsewhere, reload the page to reset Javascript bugs,
and even try a different browser to see if they happen to have the one on their computer that we bothered to test.

The problem with my project was that I wanted to **meet a high quality bar** for smoothness and polish in the resulting app.
Games are supposed to be fun, and frustration from glitches is not fun. As a result, games are some of the highest-quality
user experiences the software industry makes. Game developers **care much more** than the average software team about the
consistency of look, feel, and smoothness of their UX. Polish was the real source of my high development costs.

### Footnote: a damning case against vanilla.js?

I admit that all these snags made me a bit more pessimistic about using "bare" HTML5 for projects. But
counterpoint, there is no popular web framework that would have polyfilled *all* these particular
issues away either. React, tailwind, bootstrap, Angular etc do not try to *entirely* abstract things
like CSS, animations, text input, touch input, and media playback. Some of the widget library frameworks would have helped,
but I would still have had a lot of these problems. Unity and Godot might be better choices, but I have
no experience with them and I assume they only make sense for games.

For my own web projects, I have an ever-growing proprietary polyfill library into which I encode best
practices. And I know from experience that large web shops do the same. Oh well.

**Index**

* [Introduction](#introduction)
* [tl;dr advice](#tldr)
* [Culprit 1: Apple's opinions](#apple)
* [Culprit 2: Behaviors & bugs](#bugs)
* [Culprit 3: Mobile redesign](#mobile)
* [Conclusion](#conclusion)
* [Footnotes](#footnotes)

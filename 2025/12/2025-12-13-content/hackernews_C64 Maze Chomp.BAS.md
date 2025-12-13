---
source: hackernews
title: C64 Maze Chomp.BAS
url: https://basic-code.bearblog.dev/c64-maze-chompbas/
date: 2025-12-13
---

[# BASIC Code](/)

[Home](/) [About](/about/) [RSS](/feed/)[Support](http://www.patreon.com/mcoorlim) [SGDF](https://sgdf.bearblog.dev/)

# c64 Maze Chomp.BAS

*06 Dec, 2025*

Maze Chomp is a simple maze game adapted from a listing in issue #3 of 1984's Input Magazine.

!

The goal is to eat all the dots in as little time as possible.

Controls: WASD keys, space to continue

Input is a great magazine for learning BASIC. It's structured like a course, with different article-lessons in each issue. It covers not only the Commodore 64, but the Vic-20, Acorn Electron and BBC Micro, Sinclair Spectrum and ZX 81, and Tandy Dragon and TRS80 Color Computer.

### The Listing

```
10 p1=1117: p2=p1 :rem where coins begin & loc of player
15 bd = 53280 : rem border address
20 bg = 53281 : rem background address
25 ll = 40 :rem characters in a line
30 cc$=chr$(5) : rem character color
35 cs$=chr$(147): rem clear screen poke value
40 ch$= chr$(19)
```

First we set the variables; p1 is the location of the player on the screen in the upper left corner as a memory address. BD is the screen border color as a memory address, and bg is the background color as an address. All of these will be used by `POKE` statements, but it's good practice to store them as variables instead of hard-coded numbers.

cc$ holds the value for our character color, and cs$ is the chr$ character code to clear the screen. Saves on typing and BASIC memory when we use it often.

```
45 poke bd,6: rem set border color
50 poke bg,0: rem set background color
55 poke 650,128: rem set up key repeat for smooth input
```

Line 55 sets up a simple address change that lets us store repeat key presses, so we can hold down the direction instead of tapping it.

```
60 ? cs$, cc$ : rem clear the screen, set the colors
70 rem *** the map ***
100 ? tab(12)"{cm a}{sh asterisk*13}{cm s}"
105 ? tab(12)"{sh -}.............B"
110 ? tab(12)"{sh -}.{cm +*3}.{cm +*3}.{cm +*3}.B"
115 ? tab(12)"{sh -}......{cm +}......B"
120 ? tab(12)"{sh -}{cm +*3}.{cm +}...{cm +}.{cm +*3}B"
125 ? tab(12)"{sh -}....{cm +}...{cm +}....B"
130 ? tab(12)"{sh -}.{cm +*2}...{cm +}...{cm +*2}.B"
135 ? tab(12)"{sh -}{cm +*4}.{cm +*3}.{cm +*4}B"
140 ? tab(12)"{sh -}.{cm +*2}...{cm +}...{cm +*2}.B"
145 ? tab(12)"{sh -}.............B"
150 ? tab(12)"{sh -}.{cm +*5}.{cm +*5}.B"
155 ? tab(12)"{sh -}.............B"
160 ? tab(12)"{sh -}..{cm +}.......{cm +}..B"
165 ? tab(12)"{sh -}......{cm +}......B"
170 ? tab(12)"{cm z}CCCCCCCCCCCCC{cm x}"
```

This is the maze, and keying it in was the most difficult part. See, I write these programs in a text editor, then import them into an ide that exports into the .prg file that the emulator runs. On a hardware Commodore 64 if I pressed "command +" or "shift -" I'd see the graphic character it makes immediately and lay out the maze visually as I went.

Instead I had to use notation for those keys - a quirk of the C64's graphics - and check it as I compiled the output. ASCII would have been much easier.

```
200 poke p1, 94 : rem put the player in the maze
```

Inserts the player (the π) onto the screen at the p1 address.

```
205 ?tab(12);"{down}best time: ";bt
210 ?"{down} press {reverse on}space{reverse off} to start"
215 get k$
220 if k$ <> " " then 215
225 print "{up*2}{space*39}"
230 ti$="000000"
300 get a$ : rem get key input
305 if a$ = "a" then p2 = p1-1 : rem move left
310 if a$ = "d" then p2 = p1+1 : rem move right
315 if a$ = "w" then p2 = p1-ll : rem move up
320 if a$ = "s" then p2 = p1+ll : rem move down
```

Getting the player's input; `p2` is our new position, `p1` is the current position in character spaces. `ll` holds a value of 40, the C64 screen's width in character spaces.

```
325 if peek(p2)<>32 and peek(p2)<>46 then 350
330 if peek(p2)=46 then co=co+1 : rem check for dots
335 poke p1,32
340 p1 = p2: poke p1,94
```

Checks to see if our new location holds a space or a period (the dots we're eating). If it does have a dot, we increment our score `co` and change the period to a space, "eating" it. Line 325 keeps us from moving into (eating) any other characters, making them the maze's boundaries.

```
350 print ch$
355 ?tab(2);"time:";ti$;
360 print tab(14);"score:";co
365 if co < 114 then 300
400 if g=0 then bt = val(ti$): g=1
405 if val(ti$)<bt then bt=val(ti$)
410 co = 0: poke bg,6
415 for t = 1 to 2000: next t: goto 10
```

The rest displays our time and score, resets the game if we have eaten all the dots. This "number of dots" isn't detected by the program, but rather checked for directly, so if the maze changes we need to update the number in line 365.

[#potofhoney](/blog/?q=pot-of-honey)
[#C64](/blog/?q=C64)

©2025 Michael Coorlim - [Support my work on Patreon](http://www.patreon.com/mcoorlim) - [One-Time Ko-Fi donation](https://ko-fi.com/mcoorlim)

Powered by [Bear ʕ•ᴥ•ʔ](https://bearblog.dev)

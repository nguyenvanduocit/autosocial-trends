---
source: hackernews
title: Minimum Viable Arduino Project: Aeropress Timer
url: https://netninja.com/2025/12/01/minimum-viable-arduino-project-aeropress-timer/
date: 2025-12-08
---

[Skip to content](#content)

[Netninja.com](https://netninja.com/)

A web log of Brian's projects

Menu and widgets

* [Home](https://netninja.com/)
* [Keeping Yourself Together in the Face of Fascism](https://netninja.com/keeping-yourself-together-in-the-face-of-fascism/)
* [Recent Blog Posts](https://netninja.com/blog/)
* [About](https://netninja.com/about/)
* [Archives

  Site archives back to 2001](https://netninja.com/archives/)
  + [2025 Archives](https://netninja.com/archives/2025-archives/)
  + [2024 Archives](https://netninja.com/archives/2024-archives/)
  + [2023 Archives](https://netninja.com/archives/2023-archives/)
  + [2022 Archives](https://netninja.com/archives/2022-archives/)
  + [2021 Archives](https://netninja.com/archives/2021-archives/)
  + [2020 Archives](https://netninja.com/archives/2020-archives/)
  + [2019 Archives](https://netninja.com/archives/2019-archives/)
  + [2018 Archives](https://netninja.com/archives/2018-archives/)
  + [2017 Archives](https://netninja.com/archives/2017-archives/)
  + [2016 Archives](https://netninja.com/archives/2016-archives/)
  + [2015 Archives](https://netninja.com/archives/2015-archives/)
  + [2014 Archives](https://netninja.com/archives/2014-archives/)
  + [2013 Archives](https://netninja.com/archives/2013-archives/)
  + [2012 Archives](https://netninja.com/archives/2012-archives/)
  + [2011 Archives](https://netninja.com/archives/2011-archives/)
  + [2010 Archives](https://netninja.com/archives/2010-archives/)
  + [2009 Archives](https://netninja.com/archives/2009-archives/)
  + [2008 Archives](https://netninja.com/archives/2008-archives/)
  + [2007 Archives](https://netninja.com/archives/2007-archives/)
  + [2006 Archives](https://netninja.com/archives/2006-archives/)
  + [2005 Archives](https://netninja.com/archives/2005-archives/)
  + [2004 Archives](https://netninja.com/archives/2004-archives/)
  + [2003 Archives](https://netninja.com/archives/2003-archives/)
  + [2002 Archives](https://netninja.com/archives/2002-archives/)
  + [2001 Archives](https://netninja.com/archives/2001-archives/)
* [Hipster PDA

  PDFs and templates for the Hipster PDA](https://netninja.com/hipsterpda/)
  + [ARG and Programming Tools](https://netninja.com/hipsterpda/hipster-pda-arg-and-programming-tools/)
  + [Exercise Cards](https://netninja.com/hipsterpda/hipster-pda-exercise-cards/)
  + [Hanging Envelopes](https://netninja.com/hipsterpda/hipster-pda-hanging-envelopes/)
  + [Portland Map](https://netninja.com/hipsterpda/hipster-pda-portland-map/)
  + [Scrabble Cheat Sheet](https://netninja.com/hipsterpda/hipster-pda-scrabble-cheat-sheet/)
  + [Shopping List](https://netninja.com/hipsterpda/hipster-pda-shopping-list/)
  + [Sudoku](https://netninja.com/hipsterpda/hipster-pda-sudoku/)
  + [Title Page](https://netninja.com/hipsterpda/hipster-pda-title-page/)
* [Projects](https://netninja.com/projects/)
  + [ARG Buzzword Bingo](https://netninja.com/projects/arg-buzzword-bingo/)
  + [InfoNinja](https://netninja.com/projects/infoninja/)
  + [LJProxy](https://netninja.com/projects/ljproxy/)
  + [MantisBT: ForceFixedIn](https://netninja.com/projects/forcefixedin/)
  + [MantisBT: TagColumn](https://netninja.com/projects/tagcolumn/)
  + [pwgen](https://netninja.com/projects/pwgen/)
  + [Snazzy-Archives, Filtered](https://netninja.com/projects/snazzy-archives-filtered/)
  + [Sudoku Scraper](https://netninja.com/projects/sudoku-scraper/)
  + [wcap](https://netninja.com/projects/wcap/)
  + [Wiki 1-Sheet Reference](https://netninja.com/projects/wikisheet/)
  + [WikiPub](https://netninja.com/projects/wikipub/)
  + [wmap](https://netninja.com/projects/wmap/)

* [Mastodon](https://xoxo.zone/%40BrianEnigma)
* [Pixelfed](http://pixelfed.social/BrianEnigma)
* [Github](https://github.com/BrianEnigma)
* [YouTube](https://www.youtube.com/channel/UC9xU-ViZRuEO2WLTZg2_L6A)
* [Instagram](https://instagram.com/brianenigma)
* [Facebook](https://www.facebook.com/brianenigma)
* [Flickr](https://www.flickr.com/photos/brianenigma/)
* [Reddit](https://www.reddit.com/user/BrianEnigma)
* [Stack Overflow](https://stackoverflow.org)
* [LinkedIn](https://www.linkedin.com/in/brianenigma)

Search for:

![](https://netninja.com/wp-content/uploads/2025/12/button-825x510.jpg)

# Minimum Viable Arduino Project: Aeropress Timer

Okay, okay. I admit. This isn’t the minimum viable Arduino project. That’s the blinkey-light demo. But this comes pretty close. It’s the minimum viable Arduino project that is actually useful. This project stems from two things:

First, [these super sweet aluminum buttons](https://www.amazon.com/dp/B0983YNWR2?th=1) I picked up a few weeks ago.

![](https://netninja.com/wp-content/uploads/2025/12/metal_buttons-600x600.jpg)

Second, the fact that the Aeropress coffee maker needs to be plunged after 30 seconds, and it’s really annoying and error-prone to set a 30-second timer. *Oops, I’m tired and I hit zero too many times. That’s a 3-minute (3:00) timer. I typed in 3-0 correctly, but hitting the start button didn’t quite register. Siri misheard. Siri took 10 seconds to respond about a 30 second timer.*

So I went extra. I designed and built a dedicated 30-second timer around the [Adafruit Trinket M0](https://www.adafruit.com/product/3500). This is basically an inexpensive Arduino-alike (it also can do Circuit Python if that’s your thing). It has a handful of GPIO pins, some of which can be PWM analog outputs. The electronic design of this timer is dead-simple. It’s a button wired to ground on Pin 0 (using an internal pull-up resistor) and a piezoelectric buzzer wired to Pin 1. I’d hoped to do interesting things with the buzzer by feeding it PWM-powered analog-ish power, but it seems to behave in a very binary fashion. Either it has enough power to get over the threshold to make noise, or it doesn’t. A pictographic schematic looks a bit like this:

![](https://netninja.com/wp-content/uploads/2025/12/pictographic_schematic-224x600.png)

At first, I’d planned to put it in a small laser-cut wooden box. My Glowforge broke after cutting a cardboard prototype, so I shelved that idea. (I’ve since fixed it!) I instead drafted something quick on OpenSCAD to print on the Ultimaker ([scad file](https://netninja.com/wp-content/uploads/2025/12/Aeropress_Timer.scad), [stl file](https://netninja.com/wp-content/uploads/2025/12/Aeropress_Timer.stl)). I think this worked out a little better because of the integrated clip for the buzzer, plus it’s a single unibody piece and not glued-together panes of wood or acrylic.

![](https://netninja.com/wp-content/uploads/2025/12/laser-537x600.png)

![](https://netninja.com/wp-content/uploads/2025/12/above-600x590.png)

![](https://netninja.com/wp-content/uploads/2025/12/below-545x600.png)

The Trinket board has a USB connector. In both designs, the board is flush with the side opening, exposing the USB, for power. This has a side effect in that the light from the Neopixel RGB LED on the Trinket shines out the slot and onto the kitchen counter.

Yes, this could have been battery powered, but then it would have been a more complex project: LiPo battery charging circuit, some way to indicate the battery needs charging, code that puts the processor in deep sleep, to be woken up by interrupt when the button is pressed. I won’t even mention tolerances of the Trinket’s clock and whether `delay(1000)` is really truly 1 second. This was meant to be a “couple hours on a weekend afternoon” project. You have to plug it in. It might be a few hundred milliseconds in error. Corners have been cut.

The code itself is also simple, though I added a few flourishes. I don’t drive the buzzer at annoying levels. It’s a little trill to let you know you’ve hit the button, and a quick burst of chirps at the end to let you know 30 seconds has elapsed. It blinks the Neopixel LED every second as a visual indicator that it’s counting. It has a power-on test of the LED (which helped me to see I originally had the Green and Red values reversed).

I didn’t feel it worth throwing on Github, so here’s the code inline:

```
#include <Adafruit_DotStar.h>

const int PIN_BUTTON = 0;
const int PIN_BUZZER = 1;
//const int PIN_LED = 13;
Adafruit_DotStar strip(1, 7, 8, DOTSTAR_BGR);

void setup()
{
    pinMode(PIN_BUTTON, INPUT_PULLUP);
    pinMode(PIN_BUZZER, OUTPUT);
    pinMode(13, OUTPUT);
    digitalWrite(PIN_BUZZER, LOW);
    strip.begin();
    strip.setBrightness(255);
    colorTest();
}

void colorTest()
{
    unsigned int COLORS[] = {
        strip.Color(0, 0, 0),
        strip.Color(255, 0, 0),
        strip.Color(0, 0, 0),
        strip.Color(0, 255, 0),
        strip.Color(0, 0, 0),
        strip.Color(0, 0, 255),
        strip.Color(0, 0, 0)
    };
    digitalWrite(13, HIGH);
    for (int i = 0; i < 7; i++)
    {
        strip.setPixelColor(0, COLORS[i]);
        strip.show();
        delay(100);
    }
    digitalWrite(13, LOW);
}

void loop()
{
    const int TIMER_DURATION = 30;
    //const int TIMER_DURATION = 5;
    waitForButton();
    startBeep();
    for (int seconds = 0; seconds < TIMER_DURATION; seconds++)
    {
        strip.setPixelColor(0, strip.Color(0, seconds % 2 == 0 ? 0 : 255, 0));
        strip.show();
        delay(1000);
    }
    endBeep();
    strip.setPixelColor(0, strip.Color(0, 0, 0));
    strip.show();
}

void waitForButton()
{
    int previousButtonState = HIGH;
    int lastDebounceTime = 0;
    int debounceDelay = 30;
    while (true)
    {
        int buttonState = digitalRead(PIN_BUTTON);
        digitalWrite(13, buttonState == HIGH ? LOW : HIGH);
        if (buttonState != previousButtonState)
        {
            lastDebounceTime = millis();
            previousButtonState = buttonState;
        }
        if ((millis() - lastDebounceTime) > debounceDelay)
        {
            if (buttonState == LOW)
            {
                return;
            }
        }
    }
}

void startBeep()
{
    digitalWrite(PIN_BUZZER, HIGH);
    delay(40);
    digitalWrite(PIN_BUZZER, LOW);
}

void endBeep()
{
    for (int i = 0; i < 3; i++)
    {
        digitalWrite(PIN_BUZZER, HIGH);
        delay(20);
        digitalWrite(PIN_BUZZER, LOW);
        delay(250);
    }
}
```

And that’s it. The timer sits in the kitchen and is easy for a sleepy, pre-caffinated Brian to hit in the morning.

![](https://netninja.com/wp-content/uploads/2025/12/timers-600x450.jpg)

Posted in: [![Code](https://netninja.com/wp-content/plugins/netninja-categories/code.png "Code")](https://netninja.com/category/code) [![Gadgets](https://netninja.com/wp-content/plugins/netninja-categories/gadgets.png "Gadgets")](https://netninja.com/category/gadgets) [![Projects](https://netninja.com/wp-content/plugins/netninja-categories/projects.png "Projects")](https://netninja.com/category/projects)

## Published by

![](https://secure.gravatar.com/avatar/ef7c79700711bc4b116b552188784c53317f69bf7deaa68a92e878577ee7e65b?s=56&d=identicon&r=r)

### Brian Enigma

Brian Enigma is a Portlander, manipulator of atoms & bits, minor-league blogger, and all-around great guy. He typically writes about the interesting “maker” projects he's working on, but sometimes veers off into puzzles, software, games, local news, and current events. [View all posts by Brian Enigma](https://netninja.com/author/brianenigma/)

Posted on [December 1, 2025 2:03pmDecember 1, 2025 2:56pm](https://netninja.com/2025/12/01/minimum-viable-arduino-project-aeropress-timer/)Author [Brian Enigma](https://netninja.com/author/brianenigma/)Categories [Code](https://netninja.com/category/code/), [Gadgets](https://netninja.com/category/gadgets/), [Projects](https://netninja.com/category/projects/)Tags [arduino](https://netninja.com/tag/arduino/), [timer](https://netninja.com/tag/timer/)

### Leave a Reply [Cancel reply](/2025/12/01/minimum-viable-arduino-project-aeropress-timer/#respond)

Your email address will not be published. Required fields are marked \*

Comment \*

Name \*

Email \*

Website

Δ

## Post navigation

[Previous Previous post: Political Stickers for 2025](https://netninja.com/2025/02/10/political-stickers-for-2025/)

Recent
Recent Activity:
Recent Activity

## Recent posts

* [![](https://netninja.com/wp-content/uploads/2025/02/header2-150x150.jpg)](https://netninja.com/2025/02/10/political-stickers-for-2025/)

  [Political Stickers for 2025](https://netninja.com/2025/02/10/political-stickers-for-2025/)
* [![](https://netninja.com/wp-content/uploads/2025/02/IMG_1774-150x150.jpg)](https://netninja.com/2025/02/10/copying-m4a-files-to-the-tangara/)

  [Copying M4A Files to the Tangara](https://netninja.com/2025/02/10/copying-m4a-files-to-the-tangara/)
* [![](https://netninja.com/wp-content/uploads/2025/02/Unfiction-150x150.png)](https://netninja.com/2025/02/09/reviving-the-unfiction-forums/)

  [Reviving the Unfiction Forums](https://netninja.com/2025/02/09/reviving-the-unfiction-forums/)
* [![](https://netninja.com/wp-content/uploads/2025/01/no_meta-150x150.jpg)](https://netninja.com/2025/01/28/on-boycotting-meta-for-a-week/)

  [On Boycotting Meta For A Week](https://netninja.com/2025/01/28/on-boycotting-meta-for-a-week/)
* [![](https://netninja.com/wp-content/uploads/2025/01/Lights_Out_Meta-150x150.jpg)](https://netninja.com/2025/01/18/blocking-meta-for-a-week/)

  [Blocking Meta For A Week](https://netninja.com/2025/01/18/blocking-meta-for-a-week/)

## Categories

Categories
Select Category
Administrative  (58)
ARGs  (92)
Books  (95)
Code  (144)
Dear Diary  (1,339)
Dreams  (63)
Favorites  (35)
Flashback  (2)
Food  (130)
Gadgets  (73)
Games  (86)
hipsterpda  (10)
House  (53)
iPhone  (81)
Links  (259)
Lost  (35)
MakerBot  (47)
Mobile  (3)
Movies  (137)
Music  (128)
Pictures  (196)
Politics  (5)
Popular Projects  (5)
Portland  (298)
Projects  (138)
Puzzle Games  (35)
Questions  (66)
Quotes  (24)
Random  (148)
Software  (65)
Television  (130)
Twitter  (14)
Uncategorized  (2)
Work  (200)

## Brian's Links

* [Mastodon](https://xoxo.zone/%40BrianEnigma)
* [Stack Overflow](http://stackoverflow.org "My reference site")

![](//stats.netninja.com/matomo/matomo.php?idsite=1&rec=1)

![]()

![]()

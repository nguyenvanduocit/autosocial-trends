---
source: hackernews
title: OpenSCAD is kinda neat
url: https://nuxx.net/blog/2025/12/20/openscad-is-kinda-neat/
date: 2025-12-21
---

[Press "Enter" to skip to content](#main)

[nuxx.net](https://nuxx.net/blog)

Making, baking, and (un-)breaking things in Southeast Michigan.

open menu

mobile menu toggle button

* [facebook](https://www.facebook.com/steve.vigneau)
* steve@nuxx.net

Search

* [About](https://nuxx.net/blog/about/)
* [Wiki (Archive)](/wiki_archive/A/Main_Page)

# OpenSCAD Is Kinda Neat

Published [December 20, 2025](https://nuxx.net/blog/2025/12/)

[![](https://nuxx.net/blog/wp-content/uploads/2025/12/Screenshot-2025-12-20-at-12.11.35-PM-1024x502.png)](https://nuxx.net/blog/wp-content/uploads/2025/12/Screenshot-2025-12-20-at-12.11.35-PM-scaled.png)

Designing a simple battery holder in OpenSCAD.

Earlier this year I designed a very basic box/organizer for [AA](https://en.wikipedia.org/wiki/AA_battery) and [AAA](https://en.wikipedia.org/wiki/AAA_battery) batteries in [Autodesk Fusion](https://www.autodesk.com/products/fusion-360/), making it parameterized so that by changing a few variables one could adjust the battery type/size, rows/columns, etc. This worked well, and after [uploading it to Printables earlier today](https://www.printables.com/model/1522509-simple-battery-holder-aa-and-aaa-w-parameterized-f) I realized that reimplementing it would probably be a good way to learn the basics of [OpenSCAD](https://openscad.org/).

OpenSCAD is a rather different type of CAD tool, one in which you write code to generate objects. Because my battery holder is very simple (just a box with a pattern of cutouts) and uses input parameters, I figured it’d be a good intro to a new language / tool. And in the future might even be better than firing up Fusion for such simple designs.

After going through part of [the tutorial](https://en.wikibooks.org/wiki/OpenSCAD_Tutorial) and an hour or so of poking, here’s the result: [`battery_holder_generator.scad`](https://nuxx.net/files/3d_printing/battery_holder_generator.scad)

[![](https://nuxx.net/blog/wp-content/uploads/2025/12/Screenshot-2025-12-20-at-12.30.03-PM-300x166.png)](https://nuxx.net/blog/wp-content/uploads/2025/12/Screenshot-2025-12-20-at-12.30.03-PM-scaled.png)

Slicer showing the Fusion model on top and OpenSCAD on bottom.

By changing just a few variables — `numRows` and `numColumns` and `batteryType` — one can render a customized battery holder which can then be plopped into a [slicer](https://en.wikipedia.org/wiki/Slicer_%283D_printing%29) and printed. No heavy/expensive CAD software needed and the output is effectively the same.

Without comments or informative output, this is the meat of the code:

```
AA = 15;
AAA = 11;
heightCompartment = 19;
thicknessWall = 1;
numRows = 4;
numColumns = 10;
batteryType = AA;

widthBox = (numRows * batteryType) + ((numRows + 1) * thicknessWall);
lengthBox = (numColumns * batteryType) + ((numColumns + 1) * thicknessWall);
depthBox = heightCompartment + thicknessWall;

difference() {
    cube([lengthBox, widthBox, depthBox]);
    for (c = [ 1 : numColumns ])
        for (r = [ 1 : numRows ])
            let (
                startColumn = ((c * thicknessWall) + ((c - 1) * batteryType)),
                startRow = ((r * thicknessWall) + ((r - 1) * batteryType))
            )
            {
                translate([startColumn, startRow, thicknessWall])
                cube([batteryType, batteryType, heightCompartment + 1]);
            }
};
```

Simply, it draws a box and cuts out the holes. (The first `cube()` draws the main box, then `difference()` subtracts the battery holes via the second `cube()` as their quantity and location (via `translate()`) is iterated.

That’s it. Pretty neat, eh?

(One part that confused me is how I needed to use `[let()](https://en.wikibooks.org/wiki/OpenSCAD_User_Manual/List_Comprehensions#let)` to define `startColumn` and `startRow` inside the loop. I don’t understand this…)

While this probably won’t be very helpful for more complicated designs, I can see this being super useful for bearing drifts, spacers, and other similar simple (yet incredibly useful in real life) geometric shapes.

Published in [computers](https://nuxx.net/blog/category/computers/ "View all posts in computers") and [making things](https://nuxx.net/blog/category/making-things/ "View all posts in making things")

Previous Post
[Solar Radiation (Sun) Shield for Temperature Sensors](https://nuxx.net/blog/2025/12/19/solar-radiation-sun-shield-for-temperature-sensors/)

No Newer Posts
[Return to Blog](https://nuxx.net/blog)

## Sidebar

### Recent Posts

* [OpenSCAD Is Kinda Neat](https://nuxx.net/blog/2025/12/20/openscad-is-kinda-neat/)
  December 20, 2025
* [Solar Radiation (Sun) Shield for Temperature Sensors](https://nuxx.net/blog/2025/12/19/solar-radiation-sun-shield-for-temperature-sensors/)
  December 19, 2025
* [Riser Feet for Wahoo KICKR CORE 2](https://nuxx.net/blog/2025/12/06/riser-feet-for-wahoo-kickr-core-2/)
  December 6, 2025
* [Wireshark 4.6.0 Supports macOS pktap Metadata (PID, Process Name, etc.)](https://nuxx.net/blog/2025/10/14/wireshark-4-6-0-supports-macos-pktap-metadata-pid-process-name-etc/)
  October 14, 2025
* [Fat Bike Peanuts (Presta Nuts for Single Wall Fatbike Rims)](https://nuxx.net/blog/2025/06/21/fat-bike-peanuts-presta-nuts-for-single-wall-fatbike-rims/)
  June 21, 2025
* [Windows 10/11 Drivers for Epson Perfection 3170 Photo Scanner](https://nuxx.net/blog/2025/04/02/windows-10-11-drivers-for-epson-perfection-3170-photo-scanner/)
  April 2, 2025
* [The History of Fibber Mountain](https://nuxx.net/blog/2025/03/21/the-history-of-fibber-mountain/)
  March 21, 2025
* [Fox 803-01-727 Replaces 803-01-993](https://nuxx.net/blog/2025/03/20/fox-803-01-727-replaces-803-01-993/)
  March 20, 2025
* [Automated Private Mobile Phone Photo Backup (Android to Apple Photos)](https://nuxx.net/blog/2025/01/23/automated-private-mobile-phone-photo-backup-android-to-apple-photos/)
  January 23, 2025
* [ZOOZ ZSE44 Flat Lines at 0° (C or F)](https://nuxx.net/blog/2025/01/22/zooz-zse44-flat-lines-at-0-c-or-f/)
  January 22, 2025
* [Bambu Lab P1S on IoT VLAN](https://nuxx.net/blog/2024/12/19/bambu-lab-p1s-on-iot-vlan/)
  December 19, 2024
* [Fix for Leaky Valves on Single Wall Fatbike Rims](https://nuxx.net/blog/2024/12/06/fix-for-leaky-valves-on-single-wall-fatbike-rims/)
  December 6, 2024
* [Ride Dirt Trails, Not Mud Trails: Reposted](https://nuxx.net/blog/2024/11/22/ride-dirt-trails-not-mud-trails-reposted/)
  November 22, 2024
* [HDMI-CEC to Onkyo RI Bridge](https://nuxx.net/blog/2024/09/02/hdmi-cec-to-onkyo-ri-bridge/)
  September 2, 2024
* [Onkyo RI for ESPHome / Home Assistant](https://nuxx.net/blog/2024/08/19/onkyo-ri-for-esphome-home-assistant/)
  August 19, 2024
* [New XC Bike: Pivot Mach 4 SL v3](https://nuxx.net/blog/2024/05/12/new-xc-bike-pivot-mach-4-sl-v3/)
  May 12, 2024
* [Fork-Mount Bike Rack for Honda Odyssey (2018+)](https://nuxx.net/blog/2024/03/05/fork-mount-bike-rack-for-honda-odyssey-2018/)
  March 5, 2024
* [Sunrise-like Alarm Clock via Home Assistant + Android](https://nuxx.net/blog/2024/01/19/sunrise-like-alarm-clock-via-home-assistant-android/)
  January 19, 2024
* [\_wahoo-fitness-tnp.\_tcp.local](https://nuxx.net/blog/2024/01/10/_wahoo-fitness-tnp-_tcp-local/)
  January 10, 2024
* [NGINX on OPNsense for Home Assistant](https://nuxx.net/blog/2024/01/08/nginx-on-opnsense-for-home-assistant/)
  January 8, 2024

### Categories

* [acquired things](https://nuxx.net/blog/category/acquired-things/)
* [around the house](https://nuxx.net/blog/category/around-the-house/)
* [automotive](https://nuxx.net/blog/category/automotive/)
* [beer](https://nuxx.net/blog/category/beer/)
* [computers](https://nuxx.net/blog/category/computers/)
* [cycling](https://nuxx.net/blog/category/cycling/)
* [electronics](https://nuxx.net/blog/category/electronics/)
* [family](https://nuxx.net/blog/category/family/)
* [finances](https://nuxx.net/blog/category/finances/)
* [food](https://nuxx.net/blog/category/food/)
* [found things](https://nuxx.net/blog/category/found-things/)
* [games](https://nuxx.net/blog/category/games/)
* [health](https://nuxx.net/blog/category/health/)
* [livejournal](https://nuxx.net/blog/category/livejournal/)
* [making things](https://nuxx.net/blog/category/making-things/)
* [mapping](https://nuxx.net/blog/category/mapping/)
* [moved from livejournal](https://nuxx.net/blog/category/moved-from-livejournal/)
* [movies](https://nuxx.net/blog/category/movies/)
* [music](https://nuxx.net/blog/category/music/)
* [nuxx.net](https://nuxx.net/blog/category/nuxxnet/)
* [outdoors](https://nuxx.net/blog/category/outdoors/)
* [politics](https://nuxx.net/blog/category/politics/)
* [travel](https://nuxx.net/blog/category/travel/)
* [weather](https://nuxx.net/blog/category/weather/)
* [work](https://nuxx.net/blog/category/work/)

### Posts

* [OpenSCAD Is Kinda Neat](https://nuxx.net/blog/2025/12/20/openscad-is-kinda-neat/)
* [Solar Radiation (Sun) Shield for Temperature Sensors](https://nuxx.net/blog/2025/12/19/solar-radiation-sun-shield-for-temperature-sensors/)
* [Riser Feet for Wahoo KICKR CORE 2](https://nuxx.net/blog/2025/12/06/riser-feet-for-wahoo-kickr-core-2/)
* [Wireshark 4.6.0 Supports macOS pktap Metadata (PID, Process Name, etc.)](https://nuxx.net/blog/2025/10/14/wireshark-4-6-0-supports-macos-pktap-metadata-pid-process-name-etc/)
* [Fat Bike Peanuts (Presta Nuts for Single Wall Fatbike Rims)](https://nuxx.net/blog/2025/06/21/fat-bike-peanuts-presta-nuts-for-single-wall-fatbike-rims/)
* [Windows 10/11 Drivers for Epson Perfection 3170 Photo Scanner](https://nuxx.net/blog/2025/04/02/windows-10-11-drivers-for-epson-perfection-3170-photo-scanner/)
* [The History of Fibber Mountain](https://nuxx.net/blog/2025/03/21/the-history-of-fibber-mountain/)
* [Fox 803-01-727 Replaces 803-01-993](https://nuxx.net/blog/2025/03/20/fox-803-01-727-replaces-803-01-993/)
* [Automated Private Mobile Phone Photo Backup (Android to Apple Photos)](https://nuxx.net/blog/2025/01/23/automated-private-mobile-phone-photo-backup-android-to-apple-photos/)
* [ZOOZ ZSE44 Flat Lines at 0° (C or F)](https://nuxx.net/blog/2025/01/22/zooz-zse44-flat-lines-at-0-c-or-f/)

[Period WordPress Theme](https://www.competethemes.com/period/) by Compete Themes.

Scroll to the top

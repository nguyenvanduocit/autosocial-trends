---
source: hackernews
title: Asahi Linux with Sway on the MacBook Air M2 (2024)
url: https://daniel.lawrence.lu/blog/2024-12-01-asahi-linux-with-sway-on-the-macbook-air-m2/
date: 2025-12-26
---

[dllu](https://daniel.lawrence.lu/)

1. [1 Installing Asahi Linux](#s1)
2. [2 Getting set up](#s2)
3. [3 Customizing for MacBook](#s3)
   1. [3.1 Using Waybar instead](#s3.1)
4. [4 Using it as a daily driver](#s4)

# Asahi Linux with Sway on the MacBook Air M2

2024-12-01

I bought a MacBook Air M2.
As of writing, it’s very affordable with the 16 GB RAM, 256 GB SSD, 13.6” model available for $750.
As of writing, also Asahi Linux doesn’t support anything newer than M2.

[buy on amazon](https://amzn.to/3ZqsRCx)

![](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-600.jpg)

[FIGURE 1](#fig1) Pic of my laptop.

EXIF data

Camera
:   FUJIFILM GFX100S

Lens
:   GF55mmF1.7 R WR

Aperture
:   f/1.7

Shutter
:   1/42 s

ISO
:   ISO 400

Software
:   Digital Camera GFX100S Ver2.12

Date
:   2024-12-01 19:14:32

Download

* [600 × 450](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-600.jpg)
* [1200 × 900](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-1200.jpg)
* [2400 × 1800](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-2400.jpg)
* [3840 × 2880](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-3840.jpg)
* [7680 × 5760](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9-7680.jpg)
* [original 11648 × 8736](https://i.dllu.net/2024-12-01-19-14-32_DSCF3120_58a0af3f29ead0118ef194dbccea00f020dbe0f8_a168a62312d435a9.jpg)

I had previously used:

* 2011-2015: MacBook Air 13.3” with Intel Core i5 1.8 GHz, 8 GB of RAM, and 256 GB SSD (aftermarket upgrade from OWC). I installed Arch Linux on it with the i3 window manager.
* 2014-2018: Dell XPS 13 Developer Edition. I used the Ubuntu 14.04 that came with it with the i3 window manager.
* 2018-2024: Lenovo Thinkpad X1 Carbon Gen 6 with Intel Core i7 8640U, 16 GB of RAM, and 1 TB SSD. I installed Arch Linux on it with Sway.

# [1](#s1) Installing Asahi Linux

On the [Asahi Linux](https://asahilinux.org) there’s a one liner which you can paste into the Terminal.
It worked very well.
The only complaint is that it seemed to take hours to copy root.img and boot.img over at 150 KB/s.

Since I intended to run it with the [Sway Window Manager](https://swaywm.org/), and storage space is precious, I installed Fedora minimal.

# [2](#s2) Getting set up

I connected to wifi with

```
nmcli device wifi list
nmcli device wifi connect 'my_ssid' password 'mypassword'
```

and then I installed a bunch of packages I use, such as:

```
sudo dnf install @sway-desktop-environment fish alacritty rofi ruff rclone pavucontrol-qt i3status mako pass syncthing maim xdg-user-dirs firefox rustup openssl-devel ncdu fd-find neovim
```

Then, I cloned my [personal dotfile git repo](https://github.com/dllu/dotfiles) and ran `setup.sh`.
Of course, my configs weren’t meant for the MacBook, so I had to make some changes (which I’ve pushed to the dotfiles).

# [3](#s3) Customizing for MacBook

By default, the whole row containing the notch is disabled, leading to a large-bezels look which I personally don’t like. There has got to be a way to use that screen real estate nicely!

I re-enabled that part of the screen with

```
grubby --args=apple_dcp.show_notch=1 --update-kernel=ALL
```

Then, I put the Sway bar on the top to make a seamless appearance where the left and right side are used for useful information but the middle part is all black.
By experimentation I found that the notch is 56px tall.

```
bar {
    position top
    status_command i3status
    modifier $mod
    tray_output primary
    # the height of the m2 macbook air's notch???
    height 56
    colors {
        background #000000
        statusline #cfcfd9
        separator #000000
        # border background text
        focused_workspace #0c0c0c #413459 #cfcfd9
        active_workspace #0c0c0c #413459 #cfcfd9
        inactive_workspace #0c0c0c #0c0c0c #cfcfd9
        urgent_workspace #2f343a #ff3300 #ffffff
    }
}
```

The full `i3status` shows a lot of information which might get occluded by the notch, and it doesn’t work with the MacBook battery levels by default, so I had to update the config:

```
general {
        colors = true
        interval = 5
}

order += "wireless _first_"
order += "ethernet _first_"
order += "battery 0"
order += "tztime local"

wireless _first_ {
        format_up = "W: (%quality at %essid) %ip"
        format_down = "W: down"
}

ethernet _first_ {
        format_up = "E: %ip (%speed)"
        format_down = "E: down"
}

battery 0 {
        format = "%status %percentage"
        hide_seconds = true
        path = /sys/class/power_supply/macsmc-battery/uevent
}

tztime local {
        format = "%Y-%m-%d %H:%M:%S"
}
```

I usually don’t like having the bar on the top (as with macOS), since you won’t be able to move your mouse cursor to the top edge to, say, click on tabs.
Despite being mostly keyboard-driven, clicking on browser tabs with the mouse is something I still do often.

To fix that, I prevented the mouse cursor from entering the bar on the top, with

```
# use swaymsg -t get_inputs for the touchpad's identifier
input 1452:849:Apple_MTP_multi-touch map_to_region 0 56 2560 1608
```

## [3.1](#s3.1) Using Waybar instead

In around September 2025, I switched from the native Swaybar to Waybar.
Somehow, I was running into some issues with `swaymsg`‘s handling of battery levels, and my computer ricing was due for a slight visual update anyway.
It’s nice to save a tiny bit of screen real estate with icons instead of pure text, but of course, it is somewhat slower than Swaybar as it needs to render graphical stuff.
The Waybar is still situated behind the notch.

The new waybar config and css are at [waybar*config*](https://github.com/dllu/dotfiles/blob/main/waybar_config) and [waybar*style*](https://github.com/dllu/dotfiles/blob/main/waybar_style.css).

![](https://i.dllu.net/waybar-screenshot-600.png)

[FIGURE 2](#fig2) Screenshot with Waybar.

Download

* [600 × 390](https://i.dllu.net/waybar-screenshot-600.png)
* [1200 × 780](https://i.dllu.net/waybar-screenshot-1200.png)
* [2400 × 1560](https://i.dllu.net/waybar-screenshot-2400.png)
* [original 2560 × 1664](https://i.dllu.net/waybar-screenshot.png)

# [4](#s4) Using it as a daily driver

I am very impressed with how smooth and problem-free Asahi Linux is.
It is incredibly responsive and feels even smoother than my Arch Linux desktop with a 16 core AMD Ryzen 7945HX and 64GB of RAM.

The touchpad in particular is stunningly good and just as good as native macOS.
The mouse cursor movement and two finger scroll with inertia just feel incredibly natural, much better than my old Thinkpad X1 Carbon.

One of the main reasons for getting the laptop was to use it for line scan photography.
I was able to install the Alkeria SDK for ARM64 without any issues, even though it came as a deb file instead of an rpm.
I didn’t manage to get `alien` to work properly (something about the architecture arm64 not matching Fedora’s convention of calling it aarch64?) so I just used bsdtar to extract the contents into the filesystem root, yolo!!!
The M2 compiles my code super fast!

With high screen brightness and compiling lots of code, my battery went down from 100% to 60% after about 4.5 hours of use — not as good as the 15 hours of battery life on macOS but still pretty respectable.

That said, it isn’t perfect.
Common issues are:

* higher battery drainage during sleep, so I usually just shut it down entirely when not using it
* no hardware acceleration for video decoding
* some USB port quirks and external display quirks

Still, fairly decent overall.

![](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2-600.jpg)

[FIGURE 3](#fig3) Photo of me using my line scan camera with the Asahi Linux Macbook Air in London. Due to the Macbook’s tiny internal storage, I had to use an external 4 TB Crucial X8 SSD since the line scan camera racks up many gigabytes of data within seconds.

EXIF data

Camera
:   Apple iPhone 14 Pro Max

Lens
:   iPhone 14 Pro Max back triple camera 6.86mm f/1.78

Aperture
:   f/1.8

Shutter
:   1/147 s

ISO
:   ISO 64

Software
:   26.0

Date
:   2025-09-13 10:21:19

Download

* [600 × 450](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2-600.jpg)
* [1200 × 900](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2-1200.jpg)
* [2400 × 1800](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2-2400.jpg)
* [3840 × 2880](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2-3840.jpg)
* [original 4032 × 3024](https://i.dllu.net/IMG_5067_165e0ae5cc7e50f2.jpg)

copyright © 2025 Daniel Lawrence Lu

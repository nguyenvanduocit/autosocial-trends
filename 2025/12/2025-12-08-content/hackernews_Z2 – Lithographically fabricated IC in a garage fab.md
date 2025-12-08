---
source: hackernews
title: Z2 – Lithographically fabricated IC in a garage fab
url: https://sam.zeloof.xyz/second-ic/
date: 2025-12-08
---

[Skip to content](#content)

[![Sam Zeloof](https://sam.zeloof.xyz/wp-content/uploads/2018/04/cropped-DSC_8751-1.jpg)](https://sam.zeloof.xyz/)

[Sam Zeloof](https://sam.zeloof.xyz/)

Menu and widgets

## Pages:

* [Contact / About](https://sam.zeloof.xyz/about_me/)
* [Twitter](https://sam.zeloof.xyz/twitter/)
* [Looking for the smarter Zeloof Brother?](https://sam.zeloof.xyz/looking-for-the-smarter-zeloof-brother/)

Search for:

## Categories

* [Electron Microscope](https://sam.zeloof.xyz/category/electron-microscope/)
* [Electronics Projects](https://sam.zeloof.xyz/category/electronics-projects/)
* [General High Vacuum](https://sam.zeloof.xyz/category/high-vacuum/)
* [Home Chip Lab](https://sam.zeloof.xyz/category/semiconductor/)
* [Machining](https://sam.zeloof.xyz/category/machining/)
* [Plasma/Ion Source](https://sam.zeloof.xyz/category/plasma/)
* [Uncategorized](https://sam.zeloof.xyz/category/uncategorized/)

# Second IC :)

*Homemade 1000+ transistor array chip*

In 2018 I made the [first lithographically fabricated integrated circuits](https://sam.zeloof.xyz/first-ic/) in my garage fab. I was a senior in high school when I made the Z1 amplifier, and now I’m a senior in college so there are some long overdue improvements to the amateur silicon process.[![DSC_9414ano](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9414ano-1024x759.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9414ano.jpg)
The Z1 had 6 transistors and was a great test chip to develop all the processes and equipment. The Z2 has 100 transistors on a 10µm [polysilicon gate](http://www.intel4004.com/sgdm.htm) process – same technology as [Intel’s first processor](https://en.wikipedia.org/wiki/Intel_4004). My chip is a simple 10×10 array of transistors to test, characterize, and tweak the process but this is a huge step closer to more advanced DIY computer chips. The Intel 4004 has 2,200 transistors and I’ve now made 1,200 on the same piece of silicon.

![Screen Shot 2021-08-12 at 4.28.35 PM](https://sam.zeloof.xyz/wp-content/uploads/2021/08/Screen-Shot-2021-08-12-at-4.28.35-PM-1024x628.png)

[![Only half joking](https://sam.zeloof.xyz/wp-content/uploads/2021/08/mooreslaw-2-1024x768.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/mooreslaw-2.png)

Only half joking

Previously, I made chips with a [metal gate process](https://en.wikipedia.org/wiki/Metal_gate). The aluminum gate has a large work function difference with the silicon channel beneath it which results in a high threshold voltage (>10V). I used these metal gate transistors in a few fun projects like a [guitar distortion pedal](https://twitter.com/szeloof/status/1280249239495479297) and a [ring oscillator LED blinker](https://twitter.com/szeloof/status/1263940735923093505) but both of these required one or two 9V batteries to run the circuit due to high Vth. By switching to a polysilicon gate process, I get a ton of performance benefits (self aligned gate means lower overlap capacitances) including a much lower Vth which makes these chips compatible with 2.5V and 3.3V logic levels. The new FETs have excellent characteristics:

```
NMOS Electrical Properties:
Vth             = 1.1 V
Vgs MAX         = 8 V
Cgs             = <0.9 pF
Rise/fall time  = <10 ns
On/off ratio    = 4.3e6
Leakage current = 932 pA (Vds=2.5V)
```

I was particularly surprised by the super low leakage current. This value goes up about 100x in ambient room lighting.

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9375-1024x784.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9375.jpg)

NMOS, 0.5V Vgs steps

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9439-1024x802.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9439.jpg)

Diode curve

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9442-1024x813.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9442.jpg)

C-V showing Vth = 1.1V

Now we know that it’s possible to make really good transistors with impure chemicals, no cleanroom, and homemade equipment. Of course, yield and process repeatability are diminished. I’ll do more testing to collect data on the statistics and variability of FET properties but it’s looking good!

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9447-e1628800541548-1024x765.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9447-e1628800541548.jpg)

1MHz into 50Ω load

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9446-1024x763.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9446.jpg)

20MHz into 50Ω load

[![DSC_9419](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9419-1-1024x678.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/DSC_9419-1.jpg)

The chip is small, about one quarter the die area of my previous ICs (2.4mm^2) which makes it hard to probe. There’s a simple 10×10 array of N-channel FETs on each chip which will give me a lot of characterization data. Since it’s such a simple design, I was able to lay it out using Photoshop. Columns of 10 transistors share a common gate connection and each row is strung together in series with adjacent transistors sharing a source/drain terminal. It’s similar to NAND flash but I only did this to keep the metal pads large enough so I can reasonably probe them, if every FET had 3 pads for itself they would be too small.

[[http://sam.zeloof.xyz/wp-content/uploads/2021/08/probing.m4v](https://sam.zeloof.xyz/wp-content/uploads/2021/08/probing.m4v)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/probing.m4v?_=1)

It’s hard to convey the excitement of seeing a good FET curve displayed on the curve tracer after dipping a shard of rock into chemicals all day.

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/1r-300x169.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/1r.png)

Source/drain

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/2r-300x169.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/2r.png)

Poly gate

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/3r-300x169.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/3r.png)

Contact

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/4r-300x169.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/4r.png)

Metal

A single 10µm NMOS transistor can be see below, with slight misalignment in the metal layer (part of the left contact is uncovered). Red outline is polycrystalline silicon, blue is the source/drain.

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0002-1024x768.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0002.jpg)

Single NMOS transistor

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0002-copy-1024x768.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0002-copy.jpg)

Single NMOS transistor

So far I’ve made an opamp (Z1) and a memory-like array (Z2). More interesting circuits are definitely possible even with this low transistor density. The process needs some tweaking but now that I’m able to consistently make good quality transistors I should be able to design more complex digital and analog circuits. Testing each chip is very tedious so I am trying to automate the process and I’ll post more data then. I’ve made 15 chips (1,500 transistors) and know there’s at least one completely functional chip and at least two “mostly functional”, meaning ~80% of the transistors work instead of 100%. No proper yield data yet. The most common defect is a drain or source shorted to the bulk silicon channel, not a leaky or shorted gate like on my Z1 process.

[![Profilometer scan of gate](https://sam.zeloof.xyz/wp-content/uploads/2021/08/IMG_5821.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/IMG_5821.jpg)

Profilometer scan of gate layer (y axis in angstrom, x axis is micron)

I said before that the gate used to be made out of aluminum and now it’s silicon which makes the chips work a lot better. Silicon comes in three varieties that we care about: amorphous, polycrystalline, and monocrystalline. From left to right, these become more electrically conductive but also much harder to deposit. In fact, monocrystalline Si can’t be deposited, you can only grow it in contact with another mono-Si layer as a seed (epitaxy). Since the gate must be deposited on top of an insulating dielectric, poly is the best we can do. We can heavily dope the polysilicon anyway to make it more conductive.

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0003-1024x768.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0003.jpg)

2 FETs sharing gate

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0001-1024x768.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0001.jpg)

Neighbors share source/drain

A typical self-aligned polysilicon gate process requires silane, a toxic and explosive gas, to deposit polycrystalline silicon layers. It may also be possible by sputtering or [evaporating amorphous silicon and annealing with a laser](https://sci-hub.st/https%3A//sid.onlinelibrary.wiley.com/doi/abs/10.1002/sdtp.10835). A major theme of this DIY silicon process is to circumvent expensive, difficult, or dangerous steps. So, I came up with a modified process flow. It’s a variation on the standard self-aligned methods to allow doping via high temperature diffusion rather than ion implantation. The effect is that I’m able to buy a silicon wafer with the polysilicon already deposited on it from the factory and pattern it to make transistors instead of putting my own polysilicon down halfway through the process. This is a nice short term workaround but it would be best to design a polysilicon deposition process using the laser anneal method mentioned above.

Wafers are available with all kinds of materials deposited on them already, so I just had to find one with a thin layer of SiO2 (gate oxide, ~10nm) followed by a thicker polysilicon (300nm). I found a lot of 25 200mm (EPI, prime, [1-0-0], p-type) wafers on eBay for $45 which is essentially a lifetime supply, so email me if you want one. The gate oxide is the most fragile layer and requires the most care during fabrication. Since I bought the wafer with a nice high quality oxide on it already that was capped off and kept clean by the thick polysilicon layer, I was able to eliminate all the aggressive cleaning chemicals (sulfuric acid, etc) from the process and still make great transistors. Minimal process chemicals and tools are listed below.

```
Chemicals used in home poly-gate process:
-Water
-Alcohol
-Acetone
-Phosphoric acid
-Photoresist
-Developer (2% KOH)
-N type dopant (filmtronics P509)
-HF (1%) or CF4/CHF3 RIE
-HNO3 for poly etch or SF6 RIE
```

```
Equipment used in home poly-gate process:
-Hotplate
-Tube furnace
-Lithography apparatus-Microscope
-Vacuum chamber to deposit metal
```

Z2 “gate first” process (similar to standard self-aligned process but without a field oxide):

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p1-1024x669.png)

Buy wafer

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p2-1024x669.png)

Etch active

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p3-1024x669.png)

Dope source/drain

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p4-1024x669.png)

Etch poly gate

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p5-1024x669.png)

Deposit dielectric

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p6-1024x669.png)

Etch contact

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p7-1024x669.png)

Deposit metal

![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/p8-1024x669.png)

Etch metal

I snapped one of the test chips in half (functional Z2 but with bad layer alignment and thin metal, about 300nm) and put it in my [SEM](https://sam.zeloof.xyz/category/electron-microscope/) for a cross section:

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/snap1-e1629399647701-685x1024.jpg)](https://sam.zeloof.xyz/second-ic/snap1/)

…snap

[![](https://sam.zeloof.xyz/wp-content/uploads/2021/08/s-692x1024.jpg)](https://sam.zeloof.xyz/second-ic/s/)

Tilted SEM view

Find the dust particle in the red circle below, use that to get oriented in the coming cross section views.

[![xsecloc](https://sam.zeloof.xyz/wp-content/uploads/2021/08/xsecloc-1024x515.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/xsecloc.jpg)

[![Xsection (1)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/Xsection-1-1024x550.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/Xsection-1.png)

[![NMOS cross section](https://sam.zeloof.xyz/wp-content/uploads/2021/08/Xsection_ano-1024x550.png)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/Xsection_ano.png)

NMOS cross section

Because I bought the wafer already with gate oxide and polysilicon on it, I can’t grow a field oxide. These thick oxide layers are typically used to mask dopants and require a long high temperature step which would oxidize all of my poly and there would be none remaining. So, my modified process uses an additional masking step (the “gate” mask is typically not found in a self-aligned process) that allows me to use the polysilicon itself as a dopant mask and hard-baked photoresist as the field dielectric. This alternative processing results in the stepped structure you can see in the orange region on the NMOS cross section above. This process subtlety is mentioned here, [read this twitter thread](https://twitter.com/szeloof/status/1426534655197646857?s=20).

[![Gate length measurement](https://sam.zeloof.xyz/wp-content/uploads/2021/08/gatemeasure-1-1024x692.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/gatemeasure-1.jpg)

Gate length measurement

This process isn’t ideal and I want to make some changes so it’s CMOS compatible but it simplifies fabrication and makes it possible with a minimal set of tools. The 1µm dielectric layer (orange) would ideally be CVD SiO2 (it’s possible to build a TEOS oxide reactor at home) but I used a photoresist instead. Most photoresists can be baked around 250°C to form a hard permanent dielectric layer that is an easy alternative to CVD or PECVD oxide. A spin-on-glass/sol-gel could also be used here. SiO2 etching is done with a [buffered HF solution made from rust stain remover](https://sam.zeloof.xyz/sio2-patterning/) or RIE.

Huge composite stitched die image:

[![0001_stitch](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0001_stitch-1024x958.jpg)](https://sam.zeloof.xyz/wp-content/uploads/2021/08/0001_stitch.jpg)

Thanks for following my work and feel free to contact me with your thoughts!

Posted on [August 12, 2021November 28, 2021](https://sam.zeloof.xyz/second-ic/)Author [szeloof](https://sam.zeloof.xyz/author/szeloof/)Categories [Home Chip Lab](https://sam.zeloof.xyz/category/semiconductor/)

## 39 thoughts on “Second IC :)”

1. Pingback: [Garage chip fab – nickwinlund.net](https://nickwinlund.net/index.php/2021/08/13/garage-chip-fab/)
2. Pingback: [Sam Zeloof’s Upgraded Homemade Silicon IC Fab Process – Cyber News Network](https://breach74.com/blog/2021/08/14/sam-zeloofs-upgraded-homemade-silicon-ic-fab-process/)
3. ![](https://secure.gravatar.com/avatar/039a2f2bf59165718d8bedea08850cdf?s=56&d=mm&r=g) **Giedrius** says:

   [August 14, 2021 at 7:46 am](https://sam.zeloof.xyz/second-ic/#comment-46010)

   This is amazing! Keep up the good and interesting work.

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46010#respond)

   1. ![](https://secure.gravatar.com/avatar/b3192a3f00bd7b6a58cd134844da3f96?s=56&d=mm&r=g) **Ryan** says:

      [April 4, 2022 at 1:42 pm](https://sam.zeloof.xyz/second-ic/#comment-51037)

      I concur!

      [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=51037#respond)
4. ![](https://secure.gravatar.com/avatar/772def24c9a49762f8260e449bb104f6?s=56&d=mm&r=g) **David Tournatory** says:

   [August 14, 2021 at 5:12 pm](https://sam.zeloof.xyz/second-ic/#comment-46021)

   Congrats, really amazing! So refreshing to see this kind of efforts in the semiconductor space.

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46021#respond)
5. ![](https://secure.gravatar.com/avatar/29c341949d266d11fa0c7325950f41d8?s=56&d=mm&r=g) **Enrique** says:

   [August 15, 2021 at 2:09 am](https://sam.zeloof.xyz/second-ic/#comment-46036)

   Awesome work and write up.

   You could look for improvement by starting with SOI wafers with a thin (50nm) device layer to reduce power consumption. But not sure you can easily find SOI+poly Si. Maybe.

   I seem to remember someone repurposing a DVD-R drive for photolithography (LightScribe?), which is neat because that’s pretty close to what a DVD-R is built to do. Not sure how the resolution compares to your home made stepper.

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46036#respond)
6. ![](https://secure.gravatar.com/avatar/06019475fabf5632ab928d3cd12cebb4?s=56&d=mm&r=g) **Byron** says:

   [August 15, 2021 at 4:48 am](https://sam.zeloof.xyz/second-ic/#comment-46038)

   What photoresist are you using?

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46038#respond)
7. ![](https://secure.gravatar.com/avatar/ecab88751df3db4a1701cd5fb5d81825?s=56&d=mm&r=g) **Hugo Orán** says:

   [August 15, 2021 at 7:52 am](https://sam.zeloof.xyz/second-ic/#comment-46042)

   Great and Amazing! Good work!
   Except confusingly naming the chips similar to Conrad Zuse’s computers:)

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46042#respond)
8. ![](https://secure.gravatar.com/avatar/2cdc93306ca3b10e8b22b4468313ec4f?s=56&d=mm&r=g) **[Colin Munro](https://tattiebogle.net/)** says:

   [August 15, 2021 at 3:08 pm](https://sam.zeloof.xyz/second-ic/#comment-46048)

   This is great, thanks for sharing! Making ICs at home is something I’ve always thought about, but never tried as it just seemed like too much (scary chemicals, very fiddly), so seeing it is actually possible is very interesting! Looking forward to more, and possibly I’ll try it myself sometime 🙂

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46048#respond)
9. ![](https://secure.gravatar.com/avatar/5b6e62a6dde108a95804e75efe653edd?s=56&d=mm&r=g) **[Rixtronix](https://www.youtube.com/channel/UC89se0BZ2oGeqN5jNbDAyQQ)** says:

   [August 15, 2021 at 9:32 pm](https://sam.zeloof.xyz/second-ic/#comment-46049)

   Very cool 🙂

   [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46049#respond)
10. ![](https://secure.gravatar.com/avatar/78b58e83f05487f2553c41423e1dccb4?s=56&d=mm&r=g) **nickels** says:

    [August 15, 2021 at 10:55 pm](https://sam.zeloof.xyz/second-ic/#comment-46050)

    this is awesome

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46050#respond)
11. ![](https://secure.gravatar.com/avatar/7f730ccbc5530a86f0944ce786330781?s=56&d=mm&r=g) **[Alex R2AUK](https://eax.me/)** says:

    [August 16, 2021 at 11:08 am](https://sam.zeloof.xyz/second-ic/#comment-46056)

    Amazing! A quick question: what is the estimated cost of the equipment required to do this?

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46056#respond)

    1. ![](https://secure.gravatar.com/avatar/ecdada6ae868e6fe0b4aeecb82f1e4ed?s=56&d=mm&r=g) **[Apocal Ye](http://eture.tech)** says:

       [January 27, 2022 at 1:05 pm](https://sam.zeloof.xyz/second-ic/#comment-49562)

       Curious too

       [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49562#respond)
12. ![](https://secure.gravatar.com/avatar/8d2dad5578da7557c4a48410c6865f2e?s=56&d=mm&r=g) **InuYasha** says:

    [August 16, 2021 at 7:18 pm](https://sam.zeloof.xyz/second-ic/#comment-46063)

    Congratulations! Great accomplishment!
    Also, your project made me think of possibility to create custom transistors for audio applications. )

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46063#respond)

    1. ![](https://secure.gravatar.com/avatar/0564fd25ec0c7b6405e32f1643c7e290?s=56&d=mm&r=g) **Fred Joel** says:

       [January 22, 2022 at 2:56 pm](https://sam.zeloof.xyz/second-ic/#comment-49436)

       Excellent work! I had a similar thought. Would it be feasible to make replacement semiconductors for popular semiconductor devices such as used in audio equipment? Back in the day, certain manufacturers published semiconductor guides that were handed at electronics shops (most have disappeared) out at no cost. These were ECG/Philips, HEP/Motorola, and SK/RCA. Generally, one replacement device covered many other original devices.

       [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49436#respond)

       1. ![](https://secure.gravatar.com/avatar/0564fd25ec0c7b6405e32f1643c7e290?s=56&d=mm&r=g) **Fred Joel** says:

          [January 22, 2022 at 2:59 pm](https://sam.zeloof.xyz/second-ic/#comment-49438)

          Follow up: looking at your video on YouTube brought back a memory of a tour in the mid-1970’s of the RCA semiconductor manufacturing facility in Mountaintop, Pennsylvania!

          [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49438#respond)
13. ![](https://secure.gravatar.com/avatar/cd0286eb2f2ec8394796d642b91f9acb?s=56&d=mm&r=g) **Sergei** says:

    [August 19, 2021 at 3:35 pm](https://sam.zeloof.xyz/second-ic/#comment-46143)

    Very nice:) We are waiting for homemade 10 nm!
    P.S. google translate 🙂

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46143#respond)
14. ![](https://secure.gravatar.com/avatar/d37e10fb58679a776cd3c9c9feba6e05?s=56&d=mm&r=g) **Yongxian Gu** says:

    [August 20, 2021 at 11:15 am](https://sam.zeloof.xyz/second-ic/#comment-46158)

    Congratulations! keep going

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46158#respond)
15. ![](https://secure.gravatar.com/avatar/a9af783d1be58630d80981b7ab5beaeb?s=56&d=mm&r=g) **Oussama Boumaad** says:

    [August 21, 2021 at 7:37 am](https://sam.zeloof.xyz/second-ic/#comment-46172)

    Yo man, you did a great job. The best part of this is you shared your experience and you made it open to public. Thank you so much. The explanation is great and which you success. You did as Linus Torvalds did one day when he share linux kernel. And now he’s the most famous one and got $ 50 millions net worth. Best of luck man

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46172#respond)
16. ![](https://secure.gravatar.com/avatar/128df9a6c2682309a8e245b754210cbe?s=56&d=mm&r=g) **[ValorosoIT](http://www.valoroso.it)** says:

    [August 21, 2021 at 7:44 am](https://sam.zeloof.xyz/second-ic/#comment-46173)

    Congrats! It is really impressive!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46173#respond)
17. ![](https://secure.gravatar.com/avatar/f15b183f7ba94558b661ce9e9582cdc6?s=56&d=mm&r=g) **Billie** says:

    [August 22, 2021 at 6:58 pm](https://sam.zeloof.xyz/second-ic/#comment-46222)

    This is amazing work! Out of curiosity, would you consider using this same process to produce linear-enhancement-load NMOS logic?

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46222#respond)
18. ![](https://secure.gravatar.com/avatar/980f37a6867b1e18feeb0161260490e6?s=56&d=mm&r=g) **Twix** says:

    [August 25, 2021 at 6:05 pm](https://sam.zeloof.xyz/second-ic/#comment-46268)

    you are insane bro what are you doing is unbelievable and impressive as individual

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46268#respond)
19. ![](https://secure.gravatar.com/avatar/c839d7ccb4c87ad81200a959eb42e6d0?s=56&d=mm&r=g) **Yevhen** says:

    [September 3, 2021 at 9:42 am](https://sam.zeloof.xyz/second-ic/#comment-46415)

    Hi it very fantastic

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46415#respond)
20. ![](https://secure.gravatar.com/avatar/062833017e888310871b23309d080c04?s=56&d=mm&r=g) **[OpenSpeedTest](https://openspeedtest.com)** says:

    [September 24, 2021 at 4:49 am](https://sam.zeloof.xyz/second-ic/#comment-46812)

    This is extraordinary, you are in to something big, Very Big. Good luck.

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=46812#respond)
21. ![](https://secure.gravatar.com/avatar/e04f0206e4c02d791a518c4b1c6cbb34?s=56&d=mm&r=g) **Michelle C. McElwain** says:

    [January 22, 2022 at 2:22 pm](https://sam.zeloof.xyz/second-ic/#comment-49434)

    Wonderful news. Stay Focused, we need you!! Keep up the great work!! Build again!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49434#respond)
22. ![](https://secure.gravatar.com/avatar/d59713501fb24833e2278b2f4c44441e?s=56&d=mm&r=g) **from india** says:

    [January 24, 2022 at 6:06 am](https://sam.zeloof.xyz/second-ic/#comment-49494)

    Congratulations! Great accomplishment!Excellent work! Any thought about carbon based devices replacing silicon?

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49494#respond)
23. ![](https://secure.gravatar.com/avatar/7385c5be0dd566c739ac55411a75e02a?s=56&d=mm&r=g) **Dankoozy** says:

    [January 24, 2022 at 10:39 am](https://sam.zeloof.xyz/second-ic/#comment-49499)

    This is the best solution to the chip shortage.. take matters into your own hands 🙂 🙂 🙂

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49499#respond)
24. ![](https://secure.gravatar.com/avatar/b09b880e5a9b67a6623f44dd5b1e3368?s=56&d=mm&r=g) **Mike** says:

    [January 24, 2022 at 2:08 pm](https://sam.zeloof.xyz/second-ic/#comment-49501)

    This is incredible – good luck!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49501#respond)
25. ![](https://secure.gravatar.com/avatar/991cebd3f87c6c6bf03ce1335f151bc4?s=56&d=mm&r=g) **Wang Junfeng** says:

    [January 26, 2022 at 2:15 am](https://sam.zeloof.xyz/second-ic/#comment-49527)

    COOOOL!!!!!Congratulations!From China

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49527#respond)
26. ![](https://secure.gravatar.com/avatar/3cdf1055440d06dc9f66cfb93a2e633a?s=56&d=mm&r=g) **Jose** says:

    [January 27, 2022 at 12:50 am](https://sam.zeloof.xyz/second-ic/#comment-49548)

    Excelent job

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49548#respond)
27. ![](https://secure.gravatar.com/avatar/ecdada6ae868e6fe0b4aeecb82f1e4ed?s=56&d=mm&r=g) **[Apocal Ye](http://eture.tech)** says:

    [January 27, 2022 at 1:03 pm](https://sam.zeloof.xyz/second-ic/#comment-49561)

    Incredible project! So amazed that u managed to get all those equipment!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49561#respond)
28. ![](https://secure.gravatar.com/avatar/7369438279c1161727dbca5755d8dfce?s=56&d=mm&r=g) **Amro** says:

    [January 29, 2022 at 10:31 am](https://sam.zeloof.xyz/second-ic/#comment-49620)

    Beyond outstanding! Great work and achievements. Great sharing of knowledge and experience. So much to learn from.

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=49620#respond)
29. ![](https://secure.gravatar.com/avatar/ba1c687162640e968315297b263c6d6d?s=56&d=mm&r=g) **Iamtheoverlooker** says:

    [March 27, 2022 at 9:37 pm](https://sam.zeloof.xyz/second-ic/#comment-50882)

    Anything on the Z3? and what are the applications of the Z2?

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=50882#respond)
30. ![](https://secure.gravatar.com/avatar/c7f9d2e13eb4c39eb1b430bb8f6bd742?s=56&d=mm&r=g) **[Erfan Kheyrollahi](https://erfankh.ir/)** says:

    [May 16, 2022 at 1:32 pm](https://sam.zeloof.xyz/second-ic/#comment-51935)

    amazing!
    I am waiting to see further progress.
    good luck!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=51935#respond)
31. ![](https://secure.gravatar.com/avatar/00b5fd794f0b4a28227d3b7862b3e9c7?s=56&d=mm&r=g) **Rafa RRR** says:

    [August 29, 2022 at 4:32 pm](https://sam.zeloof.xyz/second-ic/#comment-54566)

    From Spain: “Impresionante trabajo”

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=54566#respond)
32. ![](https://secure.gravatar.com/avatar/04c5b81fa7ae6458172f5b7b7e539f6f?s=56&d=mm&r=g) **Sanjeev India** says:

    [October 6, 2022 at 3:35 am](https://sam.zeloof.xyz/second-ic/#comment-55619)

    A Beautiful Mind.
    Well done..support the world to make it better tomorrow.. You are spending your energy and time is constructive work…

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=55619#respond)
33. ![](https://secure.gravatar.com/avatar/ffb2b14763838aff89f54eda89731668?s=56&d=mm&r=g) **Robert Watkins** says:

    [December 14, 2022 at 12:56 pm](https://sam.zeloof.xyz/second-ic/#comment-57267)

    Thank you for sharing your details. I appreciate this on many levels, computer history, diy, full transparency, research and more.

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=57267#respond)
34. ![](https://secure.gravatar.com/avatar/9a67d90c33753373e23b74806a826525?s=56&d=mm&r=g) **Jeff Berkowitz** says:

    [January 31, 2023 at 9:38 pm](https://sam.zeloof.xyz/second-ic/#comment-58239)

    Forgive me, but I think you are not giving yourself enough credit. You describe your process as being similar to the one used in the Intel 4004. But the 4004 was P-channel and required much higher voltages somewhat similar to your Z1 (Vdd – Vss had to be 15 volts). What you’ve achieved is remarkable: the 4004, the 4040, and the 8008 were all P-channel. The 8080 was Intel’s first N-channel microprocessor.

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=58239#respond)
35. ![](https://secure.gravatar.com/avatar/564f84b1385ada5674cf3e9a4ef4f704?s=56&d=mm&r=g) **Aditya Sengupta** says:

    [April 1, 2023 at 4:11 am](https://sam.zeloof.xyz/second-ic/#comment-59520)

    Hello,
    I am fascinated by your interest in semiconductor manufacturing! Wishing you all the best. I will monitor your progress. Looking forward to more projects!

    [Reply](https://sam.zeloof.xyz/second-ic/?replytocom=59520#respond)

### Leave a Reply [Cancel reply](/second-ic/#respond)

Your email address will not be published. Required fields are marked \*

Comment \*

Name \*

Email \*

Website

[ ]  Save my name, email, and website in this browser for the next time I comment.

Δ

## Post navigation

[Previous Previous post: DIY Photoresist – Notes on Poly(vinyl cinnamate) synthesis](https://sam.zeloof.xyz/diy-pr/)

[Next Next post: Next Work](https://sam.zeloof.xyz/were-hiring/)

Copyright 2018 Sam Zeloof. All rights reserved.

[work](https://sam.zeloof.xyz/project/)
[sport](https://sam.zeloof.xyz/lab/)

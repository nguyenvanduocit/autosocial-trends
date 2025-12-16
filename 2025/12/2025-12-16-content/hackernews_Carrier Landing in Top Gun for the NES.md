---
source: hackernews
title: Carrier Landing in Top Gun for the NES
url: https://relaxing.run/blag/posts/top-gun-landing/
date: 2025-12-16
---

# Top Gun's Carrier Landing: Exposed

##### *Best read with [Danger Zone](https://www.youtube.com/watch?v=siwpn14IE7E) playing on loop*

Like most people, you’re probably an absolute expert at landing on the aircraft carrier in *Top Gun* for the NES. But if you’re in the silent minority that have not yet mastered this skill, you’re in luck: I’ve done a little reverse engineerining and figured out *precisely* how landing works. Hopefully now you can get things really dialed in during your next practice session. Let’s get those [windmill high-fives](https://www.youtube.com/watch?v=OxqKp_s6GWU) warmed up!

tl;dr: Altitude must be in the range 100-299, speed must be in the range 238-337 (both inclusive), and you must be laterally aimed at the carrier at the end of the sequence.

As a reminder in case you haven’t played Top Gun in the last few decades (weird), the landing portion of the stage looks like this:

![Perfect landing](/blag/posts/top-gun-landing/landing.gif)

Mercifully, the game suggests you aim right in the middle of the acceptable range per the “Alt. 200 / Speed 288” text on your MFD. Altitude and speed are both controlled by throttle input and pitch angle. There’s no on-screen heading indicator, but the game will tell you if you’re outside of the acceptable range (“Right ! Right !”). The ranges for speed and heading are pretty tight, so focus on those: the range for altitude is much wider.

After about a minute of flying the game checks your state and plays a little cutscene showing either a textbook landing or an expensive fireball. Either way, you get a “Mission Accomplished!” and go to the next level (after all, you don’t own that plane, the taxpayers do):

![Embarrassing crash](/blag/posts/top-gun-landing/crash.gif)

## Stuff for nerds

Memory locations of note:

| Address | Contents | Acceptable range (inclusive) |
| --- | --- | --- |
| $40-$41 | Speed stored as a binary coded decimal | 238 - 337 |
| $3D-$3E | Altitude stored as a BCD | 100 - 299 |
| $FD | Heading, ranging from -32 to +32 | 0 - 7 |
| $9E | Landing state check result | 0; other values change the plane’s trajectory during the crash cutscene |

Speed and altitude are stored as binary coded decimals, likely to simplify the rendering of on-screen text. For example, the number 1234 is stored as 4660 (ie., hex 0x1234).

The function at $B6EA performs the state check and writes the result at $9E. If you’re just here to impress your friends and don’t want to put in the practice, the game genie code AEPETA will guarantee a landing that Maverick and Goose (spoiler: may he rest in peace
) would be proud of.

Here’s my annotated disassembly for those following along at home:

```
landing_skill_check:
    06:B6EA: LDA $3E    ; Load altitude High cent
    06:B6EC: BEQ $B724  ; Branch if High cent == 0 (altitude < 100)
    06:B6EE: CMP #$03
    06:B6F0: BCS $B720  ; Branch if High cent >= 3 (altitude >= 300)
    06:B6F2: LDA $41
    06:B6F4: CMP #$04
    06:B6F6: BCS $B720  ; Branch if High cent is >= 04 (speed >= 400)
    06:B6F8: CMP #$02
    06:B6FA: BCC $B724  ; Branch if High cent is < 02 (speed < 200)
    06:B6FC: BEQ $B706  ; Branch if High cent == 02 (speed >= 200 && speed <= 299)
speed_300s:
    06:B6FE: LDA $40    ; Load speed Low cent
    06:B700: CMP #$38
    06:B702: BCS $B720  ; Branch if Low cent >= 38 (speed >= 338)
    06:B704: BCC $B70C  ; Branch if Low cent < 38 (speed < 338)
speed_200s:
    06:B706: LDA $40
    06:B708: CMP #$38
    06:B70A: BCC $B724  ; Branch if speed < 238
speed_ok:
    06:B70C: LDA $FD    ; Load heading
    06:B70E: BMI $B718  ; Branch if heading < 0 (too far left)
    06:B710: CMP #$08
    06:B712: BCS $B71C  ; Branch if heading >= 8 (too far right)
    06:B714: LDX #$00   ; speed ok, heading ok; 0 == success
    06:B716: BEQ $B726  ; Branch to return
too_far_left:
    06:B718: LDX #$02
    06:B71A: BNE $B726
too_far_right:
    06:B71C: LDX #$04
    06:B71E: BNE $B726
too_fast_or_too_high:
    06:B720: LDX #$08
    06:B722: BNE $B726
too_slow_or_too_low:
    06:B724: LDX #$04
return:
    06:B726: STX $9E
    06:B728: RTS
```

Now get out there, and snag that third wire.

14.12.2025

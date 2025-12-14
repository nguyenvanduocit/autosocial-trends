---
source: hackernews
title: Cryptids
url: https://wiki.bbchallenge.org/wiki/Cryptids
date: 2025-12-14
---

# Cryptids

From BusyBeaverWiki

[Jump to navigation](#mw-head)
[Jump to search](#searchInput)

[![A monstrous beaver in the style of HP Lovecraft with pink tentacles coming out of its mouth, 5 red spider eyes, horns on its head, elbows and tail, moss colored fur, sharp purple claws and webbed feet.](/w/images/thumb/f/f6/Lovecraft_beaver.png/300px-Lovecraft_beaver.png)](/wiki/File%3ALovecraft_beaver.png)

Lovecraftian Beaver fan art made by Lauren

**Cryptids** are Turing Machines whose behavior (when started on a blank tape) can be described completely by a relatively simple mathematical rule, but where that rule falls into a class of unsolved (and presumed hard) mathematical problems. This definition is somewhat subjective (What counts as a simple rule? What counts as a hard problem?). In practice, most currently known small Cryptids have [Collatz-like](/wiki/Collatz-like "Collatz-like") behavior. In other words, the halting problem from blank tape of Cryptids is mathematically-hard.

If there exists a Cryptid with n states and m symbols, then BB(n, m) cannot be solved without solving this hard math problem.

The name Cryptid was proposed by Shawn Ligocki in an Oct 2023 [blog post](https://www.sligocki.com/2023/10/16/bb-3-3-is-hard.html) announcing the discovery of [Bigfoot](/wiki/Bigfoot "Bigfoot").

## Cryptids at the Edge

This is a list of notable Minimal Cryptids (Cryptids in a [domain](/wiki/Category%3ABB_Domains "Category:BB Domains") with no strictly smaller known Cryptid). All of these Cryptids were "discovered in the wild" rather than "constructed".

| Name | BB domain | Machine | Date | Discoverer | Note |
| --- | --- | --- | --- | --- | --- |
| [Bigfoot](/wiki/Bigfoot "Bigfoot") | [BB(3,3)](/wiki/BB%283%2C3%29 "BB(3,3)") | `[1RB2RA1LC_2LC1RB2RB_---2LA1LA](/wiki/1RB2RA1LC_2LC1RB2RB_---2LA1LA "1RB2RA1LC 2LC1RB2RB ---2LA1LA")` ([bbch](https://bbchallenge.org/1RB2RA1LC_2LC1RB2RB_---2LA1LA%26status%3Dundecided)) | Nov 2023 | [Shawn Ligocki](/wiki/User%3ASligocki "User:Sligocki") |  |
| [Hydra](/wiki/Hydra "Hydra") | [BB(2,5)](/wiki/BB%282%2C5%29 "BB(2,5)") | `[1RB3RB---3LA1RA_2LA3RA4LB0LB0LA](/wiki/1RB3RB---3LA1RA_2LA3RA4LB0LB0LA "1RB3RB---3LA1RA 2LA3RA4LB0LB0LA")` ([bbch](https://bbchallenge.org/1RB3RB---3LA1RA_2LA3RA4LB0LB0LA%26status%3Dundecided)) | May 2024 | Daniel Yuan |  |
| [Bonus cryptid](/wiki/Bonus_cryptid "Bonus cryptid") | [BB(2,5)](/wiki/BB%282%2C5%29 "BB(2,5)") | `[1RB3RB---3LA1RA_2LA3RA4LB0LB1LB](/wiki/1RB3RB---3LA1RA_2LA3RA4LB0LB1LB "1RB3RB---3LA1RA 2LA3RA4LB0LB1LB")` ([bbch](https://bbchallenge.org/1RB3RB---3LA1RA_2LA3RA4LB0LB1LB%26status%3D%7B%7B%7B2%7D%7D%7D)) | May 2024 | Daniel Yuan | Probviously non-halting. |
| [Antihydra](/wiki/Antihydra "Antihydra") | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1RA_0LC1LE_1LD1LC_1LA0LB_1LF1RE_---0RA](/wiki/1RB1RA_0LC1LE_1LD1LC_1LA0LB_1LF1RE_---0RA "1RB1RA 0LC1LE 1LD1LC 1LA0LB 1LF1RE ---0RA")` ([bbch](https://bbchallenge.org/1RB1RA_0LC1LE_1LD1LC_1LA0LB_1LF1RE_---0RA%26status%3Dundecided)) | June 2024 | `@mxdys`, shown to be a Cryptid by `@racheline`. | Same as **Hydra** but starting iteration from 8 instead of 3 and with termination condition `O > 2E` instead of `E > 2O`, hence the name **Antihydra**. |
| [Lucy's Moonlight](/wiki/Lucy%27s_Moonlight "Lucy's Moonlight") | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_---0RA](/wiki/1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_---0RA "1RB0RD 0RC1RE 1RD0LA 1LE1LC 1RF0LD ---0RA")` ([bbch](https://bbchallenge.org/1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_---0RA%26status%3D%7B%7B%7B2%7D%7D%7D)) | Mar 2025 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1RC_1LC1LE_1RA1RD_0RF0RE_1LA0LB_---1RA](/wiki/1RB1RC_1LC1LE_1RA1RD_0RF0RE_1LA0LB_---1RA "1RB1RC 1LC1LE 1RA1RD 0RF0RE 1LA0LB ---1RA")` ([bbch](https://bbchallenge.org/1RB1RC_1LC1LE_1RA1RD_0RF0RE_1LA0LB_---1RA%26status%3Dundecided)) | Jul 2024 | `mxdys` | Variant of Hydra and Antihydra. Probviously non-halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1LD_1RC1RE_0LA1LB_0LD1LC_1RF0RA_---0RC](/wiki/1RB1LD_1RC1RE_0LA1LB_0LD1LC_1RF0RA_---0RC "1RB1LD 1RC1RE 0LA1LB 0LD1LC 1RF0RA ---0RC")` ([bbch](https://bbchallenge.org/1RB1LD_1RC1RE_0LA1LB_0LD1LC_1RF0RA_---0RC%26status%3Dundecided)) | Aug 2024 | `mxdys` | Similar random walk mechanism to Hydra, Antihydra. Probviously non-halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0LD_1RC1RF_1LA0RA_0LA0LE_1LD1LA_0RB---](/wiki/1RB0LD_1RC1RF_1LA0RA_0LA0LE_1LD1LA_0RB--- "1RB0LD 1RC1RF 1LA0RA 0LA0LE 1LD1LA 0RB---")` ([bbch](https://bbchallenge.org/1RB0LD_1RC1RF_1LA0RA_0LA0LE_1LD1LA_0RB---%26status%3Dundecided)) | Sep 2024 | Daniel Yuan | Similar random walk mechanism to Hydra, Antihydra. Probviously non-halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0LB_1LC0RE_1LA1LD_0LC---_0RB0RF_1RE1RB](/wiki/1RB0LB_1LC0RE_1LA1LD_0LC---_0RB0RF_1RE1RB "1RB0LB 1LC0RE 1LA1LD 0LC--- 0RB0RF 1RE1RB")` ([bbch](https://bbchallenge.org/1RB0LB_1LC0RE_1LA1LD_0LC---_0RB0RF_1RE1RB%26status%3Dundecided)) | Nov 2024 | Racheline | Similar random walk mechanism to Hydra, Antihydra. Probviously non-halting. |
| [Space Needle](/wiki/1RB1LA_1LC0RE_1LF1LD_0RB0LA_1RC1RE_---0LD "1RB1LA 1LC0RE 1LF1LD 0RB0LA 1RC1RE ---0LD") | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1LA_1LC0RE_1LF1LD_0RB0LA_1RC1RE_---0LD](/wiki/1RB1LA_1LC0RE_1LF1LD_0RB0LA_1RC1RE_---0LD "1RB1LA 1LC0RE 1LF1LD 0RB0LA 1RC1RE ---0LD")` ([bbch](https://bbchallenge.org/1RB1LA_1LC0RE_1LF1LD_0RB0LA_1RC1RE_---0LD%26status%3Dundecided)) | Jan 2025 | `mxdys` | Probviously non-halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC](/wiki/1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC "1RB1RA 0RC1RC 1LD0LF 0LE1LE 1RA0LB ---0LC")` ([bbch](https://bbchallenge.org/1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC%26status%3Dundecided)) | Jul 2024 | `mxdys` | Has near-identical behavior to 16 related BB(6) holdouts. Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF](/wiki/1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF "1RB1RE 1LC1LD ---1LA 1LB1LE 0RF0RA 1LD1RF")` ([bbch](https://bbchallenge.org/1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF%26status%3D%7B%7B%7B2%7D%7D%7D)) | Jul 2024 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB](/wiki/1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB "1RB0RE 1LC1LD 0RA0LD 1LB0LA 1RF1RA ---1LB")` ([bbch](https://bbchallenge.org/1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB%26status%3D%7B%7B%7B2%7D%7D%7D)) | Jul 2024 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA](/wiki/1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA "1RB0LC 0LC0RF 1RD1LC 0RA1LE ---0LD 1LF1LA")` ([bbch](https://bbchallenge.org/1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA%26status%3D%7B%7B%7B2%7D%7D%7D)) | Jul 2024 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_---0LE](/wiki/1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_---0LE "1RB0LC 1LC0RD 1LF1LA 1LB1RE 1RB1LE ---0LE")` ([bbch](https://bbchallenge.org/1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_---0LE%26status%3D%7B%7B%7B2%7D%7D%7D)) | Nov 2024 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB---_0RC0RE_1RD1RF_1LE0LB_1RC0LD_1RC1RA](/wiki/1RB---_0RC0RE_1RD1RF_1LE0LB_1RC0LD_1RC1RA "1RB--- 0RC0RE 1RD1RF 1LE0LB 1RC0LD 1RC1RA")` ([bbch](https://bbchallenge.org/1RB---_0RC0RE_1RD1RF_1LE0LB_1RC0LD_1RC1RA%26status%3D%7B%7B%7B2%7D%7D%7D)) | Nov 2024 | Racheline | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB0LD_1RC1RA_1LD0RB_1LE1LA_1RF0RC_---1RE](/wiki/1RB0LD_1RC1RA_1LD0RB_1LE1LA_1RF0RC_---1RE "1RB0LD 1RC1RA 1LD0RB 1LE1LA 1RF0RC ---1RE")` ([bbch](https://bbchallenge.org/1RB0LD_1RC1RA_1LD0RB_1LE1LA_1RF0RC_---1RE%26status%3D%7B%7B%7B2%7D%7D%7D)) | Jul 2025 | `mxdys` | Probviously halting. |
|  | [BB(6)](/wiki/BB%286%29 "BB(6)") | `[1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE](/wiki/1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE "1RB1LE 0LC0LB 1RD1LC 1RD1RA 1RF0LA ---1RE")` ([bbch](https://bbchallenge.org/1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE%26status%3D%7B%7B%7B2%7D%7D%7D)) | Jul 2024 | Racheline | Probviously decidable. Estimated to have a 3/5 chance of becoming a [translated cycler](/wiki/Translated_cycler "Translated cycler") and a 2/5 chance of halting. |

The following machines have chaotic behavior, but have not been classified as Cryptids due to seemingly lacking a connection to any known open mathematical problems, such as Collatz-like problems.

* `[1RB1RE_1LC0RA_0RD1LB_---1RC_1LF1RE_0LB0LE](/wiki/1RB1RE_1LC0RA_0RD1LB_---1RC_1LF1RE_0LB0LE "1RB1RE 1LC0RA 0RD1LB ---1RC 1LF1RE 0LB0LE")` ([bbch](https://bbchallenge.org/1RB1RE_1LC0RA_0RD1LB_---1RC_1LF1RE_0LB0LE%26status%3Dundecided))
* `[1RB0LD_1LC0RA_1RA1LB_1LA1LE_1RF0LC_---0RE](/wiki/1RB0LD_1LC0RA_1RA1LB_1LA1LE_1RF0LC_---0RE "1RB0LD 1LC0RA 1RA1LB 1LA1LE 1RF0LC ---0RE")` ([bbch](https://bbchallenge.org/1RB0LD_1LC0RA_1RA1LB_1LA1LE_1RF0LC_---0RE%26status%3Dundecided))
* `[1RB0RB_1LC1RE_1LF0LD_1RA1LD_1RC1RB_---1LC](/wiki/1RB0RB_1LC1RE_1LF0LD_1RA1LD_1RC1RB_---1LC "1RB0RB 1LC1RE 1LF0LD 1RA1LD 1RC1RB ---1LC")` ([bbch](https://bbchallenge.org/1RB0RB_1LC1RE_1LF0LD_1RA1LD_1RC1RB_---1LC%26status%3Dundecided))
* `1RB---0RB0LA2RA_2LB2LA3RA4LB0LB` ([bbch](https://bbchallenge.org/1RB---0RB0LA2RA_2LB2LA3RA4LB0LB%26status%3Dundecided))
* `[1RB3LA1LA1RA3RA_2LB2RA---4RB1LB](/wiki/1RB3LA1LA1RA3RA_2LB2RA---4RB1LB "1RB3LA1LA1RA3RA 2LB2RA---4RB1LB")` ([bbch](https://bbchallenge.org/1RB3LA1LA1RA3RA_2LB2RA---4RB1LB%26status%3Dundecided))
* `[1RB3LA1LA1RA1RA_2LB2RA---4RB1LB](/wiki/1RB3LA1LA1RA1RA_2LB2RA---4RB1LB "1RB3LA1LA1RA1RA 2LB2RA---4RB1LB")` ([bbch](https://bbchallenge.org/1RB3LA1LA1RA1RA_2LB2RA---4RB1LB%26status%3Dundecided))
* `1RB3LB---4LA1RB_2LA4LA4LB3RB1RA` ([bbch](https://bbchallenge.org/1RB3LB---4LA1RB_2LA4LA4LB3RB1RA%26status%3Dundecided)) [Analysis by @mxdys](https://discord.com/channels/960643023006490684/1375584513777995957)

## Larger Cryptids

A more complete list of notable known Cryptids over a wider range of states and symbols. These Cryptids were all "constructed" rather than "discovered".

| Name | BB domain | Machine | Announcement | Date | Discoverer | Note |
| --- | --- | --- | --- | --- | --- | --- |
| [ZF](/wiki/Independence_from_ZFC "Independence from ZFC") | BB(432) | Wade's machine: <https://codeberg.org/ajwade/turing_machine_explorer/src/commit/33b30300054242201a95679aacf7e74312bd8803b0df9a85d2314095efd6804e> |  | 2025 | Wade, based on work by CatIsFluffy and O'Rear | The machine halts if and only if [Zermelo–Fraenkel set theory](https://en.wikipedia.org/wiki/Zermelo%E2%80%93Fraenkel_set_theory "wikipedia:Zermelo–Fraenkel set theory") is inconsistent. |
| RH | BB(744) | <https://github.com/sorear/metamath-turing-machines/blob/master/riemann-matiyasevich-aaronson.nql> |  | 2016 | Matiyasevich and O’Rear | The machine halts if and only if [Riemann Hypothesis](https://en.wikipedia.org/wiki/Riemann_hypothesis) is false. |
| Goldbach | BB(25) | <https://gist.github.com/anonymous/a64213f391339236c2fe31f8749a0df6> Machine code: ``` 1RB1RD_1LC1RB_0RA1LC_0LQ1RE_0LF1RG_0LC1LF_0LF0LH_1LQ1LI_0RJ0LI_1RK0LJ_0RL0RS_1RL0RM_1RN1RM_0LO0LU_0LP1LO_1RH1LX_1LR1LQ_0RK0LT_1LR1RS_---1RC_1LV1LU_0LW0LJ_0RK0LW_1RY1LX_1RE1RY ``` |  | 2016 | anonymous | The machine halts if and only if [Golbach's conjecture](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) is false. Its behavior has been verified in Lean.[[1]](#cite_note-1) |
| Erdős | BB(5,4) and BB(15) | <https://docs.bbchallenge.org/other/powers_of_two_5_4.txt>  Machine code: ``` 1RB3RA2RA1RB_0LC2RB1RA3RB_0LD1LC2LE3LC_3RE2RE---1RE_0RB1LE2LE3LE ```  <https://docs.bbchallenge.org/other/powers_of_two_15_2.txt>  Machine code: ``` 1RB1RO_0RC0RC_0RD1RJ_0LE1RC_0LF1LK_0LG1LE_0LH1LF_1RI0LL_0RB1LK_1RC0RA_0LI1LN_1RM---_0RI0RO_0LK1LK_1LM1RA ``` | [arxiv preprint](https://arxiv.org/abs/2107.12475) | Jul 2021 | [Tristan Stérin](/wiki/User%3ACosmo "User:Cosmo") (`@cosmo`) and Damien Woods | The machine halts if and only if the following conjecture by Erdős is false: "For all n > 8, there is at least one 2 in the base-3 representation of 2^n" |
| Weak Collatz | BB(124) and BB(43,4) | <https://docs.bbchallenge.org/other/weak_Collatz_conjecture_124_2.txt> (unverified) <https://docs.bbchallenge.org/other/weak_Collatz_conjecture_43_4.txt> (unverified) |  | Jul 2021 | [Tristan Stérin](/wiki/User%3ACosmo "User:Cosmo") | The machine halts if and only if the "weak Collatz conjecture" is false. The weak Collatz conjecture states that the iterated Collatz map (3x+1) has only one cycle on the positive integers. Not independently verified, and probably easy to further optimise. |
| Bigfoot - compiled | [BB(7)](/wiki/BB%287%29 "BB(7)") | `0RB1RB_1LC0RA_1RE1LF_1LF1RE_0RD1RD_1LG0LG_---1LB` | [Bigfoot Comment](https://github.com/sligocki/sligocki.github.io/issues/8#issuecomment-2140887228) | June 2024 | `@Iijil1` | Compilation of Bigfoot into 2 symbols, there was a previous compilation [with 8 states](https://github.com/sligocki/sligocki.github.io/issues/8#issuecomment-1774200442) |
| Hydra - compiled | BB(9) | ``` 0RB0LD_1LC0LI_1LD1LB_0LE0RG_1RF0RH_1RA---_0RD0LB_0RA---_0RF1RZ ```  [File:Hydra 9 states.txt](/wiki/File%3AHydra_9_states.txt "File:Hydra 9 states.txt") | [Discord message](https://discord.com/channels/960643023006490684/1084047886494470185/1251572501578780782) | June 2024 | `@Iijil1` | Compilation of Hydra into 2 symbols, all [confirmed by Shawn Ligocki](https://discord.com/channels/960643023006490684/1084047886494470185/1253193750486974464). `@Iijil1` provided 24 TMs which all emulate the same behavior. [Previous compilation had 10 states](https://discord.com/channels/960643023006490684/1084047886494470185/1247560072427474955), by Daniel Yuan, also [confirmed by Shawn Ligocki](https://discord.com/channels/960643023006490684/1084047886494470185/1247579473042346136). |

## Beeping Busy Beaver

Cryptids were actually noticed in the [Beeping Busy Beaver](/wiki/Beeping_Busy_Beaver "Beeping Busy Beaver") problem before they were in the classic Busy Beaver. See [Mother of Giants](/wiki/Mother_of_Giants "Mother of Giants") describing a "family" of Turing machines which "[probviously](/wiki/Probviously "Probviously")" [quasihalt](/wiki/Quasihalt "Quasihalt"), but requires solving a Collatz-like problem in order to actually prove it. They are all TMs formed by filling in the missing transition in `1RB1LE_0LC0LB_0LD1LC_1RD1RA_---0LA` with different values.

1. [↑](#cite_ref-1) <https://github.com/lengyijun/goldbach_tm>

Retrieved from "<https://wiki.bbchallenge.org/w/index.php?title=Cryptids&oldid=4522>"

[Categories](/wiki/Special%3ACategories "Special:Categories"):

* [Zoology](/wiki/Category%3AZoology "Category:Zoology")
* [Cryptids](/wiki/Category%3ACryptids "Category:Cryptids")

## Navigation menu

### Personal tools

* [Create account](/w/index.php?title=Special:CreateAccount&returnto=Cryptids "You are encouraged to create an account and log in; however, it is not mandatory")
* [Log in](/w/index.php?title=Special:UserLogin&returnto=Cryptids "You are encouraged to log in; however, it is not mandatory [o]")

### Namespaces

* [Page](/wiki/Cryptids "View the content page [c]")
* [Discussion](/wiki/Talk%3ACryptids "Discussion about the content page [t]")

[ ]

English

### Views

* [Read](/wiki/Cryptids)
* [View source](/w/index.php?title=Cryptids&action=edit "This page is protected.
  You can view its source [e]")
* [View history](/w/index.php?title=Cryptids&action=history "Past revisions of this page [h]")

[ ]

More

### Search

### Navigation

* [Main page](/wiki/Main_Page "Visit the main page [z]")
* [Recent changes](/wiki/Special%3ARecentChanges "A list of recent changes in the wiki [r]")
* [Random page](/wiki/Special%3ARandom "Load a random page [x]")
* [All pages](/wiki/Special%3AAllPages)
* [Help about MediaWiki](https://www.mediawiki.org/wiki/Special%3AMyLanguage/Help%3AContents)

### Tools

* [What links here](/wiki/Special%3AWhatLinksHere/Cryptids "A list of all wiki pages that link here [j]")
* [Related changes](/wiki/Special%3ARecentChangesLinked/Cryptids "Recent changes in pages linked from this page [k]")
* [Special pages](/wiki/Special%3ASpecialPages "A list of all special pages [q]")
* Printable version
* [Permanent link](/w/index.php?title=Cryptids&oldid=4522 "Permanent link to this revision of this page")
* [Page information](/w/index.php?title=Cryptids&action=info "More information about this page")

* This page was last edited on 13 October 2025, at 20:55.
* Content is available under [Creative Commons Attribution](https://creativecommons.org/licenses/by/4.0/) unless otherwise noted.

* [Privacy policy](/wiki/BusyBeaverWiki%3APrivacy_policy)
* [About BusyBeaverWiki](/wiki/BusyBeaverWiki%3AAbout)
* [Disclaimers](/wiki/BusyBeaverWiki%3AGeneral_disclaimer)

* [![Creative Commons Attribution](/w/resources/assets/licenses/cc-by.png)](https://creativecommons.org/licenses/by/4.0/)
* [![Powered by MediaWiki](/w/resources/assets/poweredby_mediawiki.svg)](https://www.mediawiki.org/)

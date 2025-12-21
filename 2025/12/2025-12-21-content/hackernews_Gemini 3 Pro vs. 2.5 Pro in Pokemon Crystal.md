---
source: hackernews
title: Gemini 3 Pro vs. 2.5 Pro in Pokemon Crystal
url: https://blog.jcz.dev/gemini-3-pro-vs-25-pro-in-pokemon-crystal
date: 2025-12-21
---

[Joel's Developer Blog](/?source=top_nav_blog_home)

Follow

[Joel's Developer Blog](/?source=top_nav_blog_home)

Follow

![Gemini 3 Pro vs 2.5 Pro in Pokemon Crystal](https://cdn.hashnode.com/res/hashnode/image/upload/v1765439350457/4ac15181-069d-4203-8dbd-6458dae5c0b6.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)![Gemini 3 Pro vs 2.5 Pro in Pokemon Crystal](https://cdn.hashnode.com/res/hashnode/image/upload/v1765439350457/4ac15181-069d-4203-8dbd-6458dae5c0b6.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)

# Gemini 3 Pro vs 2.5 Pro in Pokemon Crystal

[![](data:image/svg+xml...)![Joel Zhang's photo](data:image/gif;base64...)![Joel Zhang's photo](https://cdn.hashnode.com/res/hashnode/image/upload/v1754688630103/ebbdf911-fcb0-43d4-846c-63027b61ea14.jpeg?w=200&h=200&fit=crop&crop=faces&w=500&h=500&fit=crop&crop=entropy&auto=compress,format&format=webp&auto=compress,format&format=webp)](https://hashnode.com/%40jcz-dev)

[Joel Zhang](https://hashnode.com/%40jcz-dev)

·[Dec 12, 2025](https://blog.jcz.dev/gemini-3-pro-vs-25-pro-in-pokemon-crystal)·

19 min read

## Table of contents

* [Setup: same harness, same rules](#heading-setup-same-harness-same-rules)
* [Early game: same progress, very different efficiency](#heading-early-game-same-progress-very-different-efficiency)
* [Whitney, grinding, and an opening for 3 Pro](#heading-whitney-grinding-and-an-opening-for-3-pro)
* [Olivine Lighthouse: where the race truly separated](#heading-olivine-lighthouse-where-the-race-truly-separated)
* [Goldenrod Underground: a puzzle without a safety net](#heading-goldenrod-underground-a-puzzle-without-a-safety-net)
* [Where Gemini 3 Pro clearly beats 2.5 Pro](#heading-where-gemini-3-pro-clearly-beats-25-pro)
  + [1. Spatial awareness and map segmentation](#heading-1-spatial-awareness-and-map-segmentation)
  + [2. Marker aware navigation](#heading-2-marker-aware-navigation)
  + [3. Multitasking and working around harness limits](#heading-3-multitasking-and-working-around-harness-limits)
  + [4. Planning a few moves ahead](#heading-4-planning-a-few-moves-ahead)
  + [5. Vision](#heading-5-vision)
* [Weaknesses that still show up in Gemini 3 Pro](#heading-weaknesses-that-still-show-up-in-gemini-3-pro)
* [The final exam: Red](#heading-the-final-exam-red)
* [Milestone Comparison](#heading-milestone-comparison)
* [What comes next](#heading-what-comes-next)

A little over two weeks ago I wrote up [my first impressions of Gemini 3 Pro Preview](https://blog.jcz.dev/gemini-plays-pokemon-first-impressions-of-gemini-3-pro-riftrunner) inside the Gemini Plays Pokemon harness. The very next day, I spun up a head to head race [on stream](https://www.twitch.tv/gemini_plays_pokemon): Gemini 3 Pro vs Gemini 2.5 Pro, both playing Pokemon Crystal inside the exact same setup.

Fast forward two weeks. Gemini 3 Pro became the Johto Champion without losing a single battle. Gemini 2.5 Pro inched towards the 4th badge, but spent a significant amount of time looping and effectively trapped in the Olivine Lighthouse before finally breaking out.

On paper, this was a fair fight. In practice, Gemini 3 Pro behaved like a different species of agent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765389165469/b787dbed-95b3-467d-b088-914c6573affc.png?auto=compress,format&format=webp)

This post walks through how the race was set up, what actually happened moment to moment, and what it suggests about the gap between Gemini 3 Pro and 2.5 Pro as long horizon game-playing agents.

## Setup: same harness, same rules

Both models ran in the same Gemini Plays Pokemon harness. No special casing, no hidden helpers for one model and not the other. The harness exposes a set of tools that any LLM running inside it can choose to use:

* Mental Map: automatically tracks where the agent has explored, filling in fog of war as new tiles are revealed. It **does not** read map layout directly from RAM; it only updates based on tiles that have actually been visible on screen.
* Notepad: a scratchpad for objectives, future plans, and puzzle progress, including hypotheses, failures, and successes.
* Map Markers: persistent markers for points of interest such as NPCs or building entrances.
* Code Execution: a way to run one-off snippets such as a pathfinding routine.
* Custom Agents: reusable helper agents, for example a battle strategist that can think about combat without other distractions in context.
* Custom Tools: reusable pieces of code, such as a pathfinder that can be called from within a plan.

![https://i.imgur.com/7W2aQIj.png](https://i.imgur.com/7W2aQIj.png)

It is important to note that the system prompt for this harness is not just "play Pokemon". It explicitly instructs the model to behave like a scientist: form hypotheses, build tools to test them, and verify the results. Crucially, it tells the agent not to rely on its internal training data (which might be hallucinated or refer to a different version of the game) but to ground its knowledge in what it observes. Here, the *process* of discovery is part of the objective.

Because the goal is grounded in exploration and playing the game rather than speedrunning, the model's value function shifts. It doesn't always prioritize speed above all else. For example, when queried about whether it would prefer to "lose its starter / lose Suicune forever" vs "beat the game in X more hours", it often chose the latter. It acts more like a sentimental human player attached to their team than a cold optimizing agent. This is a subtle but important difference from a prompt that prioritizes completing the game with efficiency and speed.

On top of this, the harness included a set of "training wheels" designed to keep models from soft-locking runs. These were accumulated over multiple full completions of Pokemon Blue and the Yellow Legacy ROM hack with earlier models.

One of the first training wheels I added was a lesson learned from Claude Plays Pokemon. Like that project, this harness also prevents the model from mixing directional and action buttons in the same turn. If the agent wants to withdraw a Pokemon from Bill's PC, it must first move the cursor to "Withdraw" in one turn, then press A in the next. This makes it much harder to accidentally release Pokemon or, more commonly, mess up nicknames.

For Gemini 2.5 Pro, all I needed to see was the agent confidently naming itself "G" when it intended to input "GEMINI" to know this decision was a good idea. Even with this restriction, the model constantly inputs the wrong letters when nicknaming Pokemon.

Gemini 3 Pro did not really want training wheels.

At several points during the race it chafed at the restrictions and even found loopholes around them. I will come back to that when we look at how it handled multitasking.

## Early game: same progress, very different efficiency

If you only watched the stream, the two runs looked fairly similar in the early game. Badge count stayed close. They were often in the same towns at approximately the same time.

Under the hood, the story was very different. To reach the same milestones early game, Gemini 3 Pro:

* used about half as many turns as 2.5 Pro, and
* consumed about 60 percent fewer tokens.

The harness also tracks total session time, but Gemini 3 Pro was often overloaded, which produced long spans of downtime that 2.5 Pro experienced much less often. Differences in raw response speed and thinking time also make time comparisons messy, so I mostly ignored them and focused on turns and tokens.

Because of the extra downtime (nearly 250% more), Gemini 3 Pro actually fell behind for a while. The turning point came when 2.5 Pro reached Gym Leader Whitney.

## Whitney, grinding, and an opening for 3 Pro

Whitney's Miltank is infamous among human players, so it was not surprising that 2.5 Pro lost to her. What followed was a grinding arc that stretched over more than two real world days.

From the perspective of the race, that was all the opening Gemini 3 Pro needed—and unlike 2.5 Pro, it didn’t lose to Whitney. Even with more than ten hours of API downtime stacked against it at the time, 3 Pro slowly clawed back the lead while 2.5 Pro shuffled through training plans.

That small gap turned into a chasm at Olivine Lighthouse.

## Olivine Lighthouse: where the race truly separated

Gemini 3 Pro reached Olivine Lighthouse around 11,000 turns earlier than Gemini 2.5 Pro. That alone is a big difference. The more interesting part is what happened once each model entered the tower.

The puzzle in the lighthouse is deceptively simple. The intended route to the top requires you to fall through a pit on the 4th floor, which drops you down to a lower level and exposes a new staircase up toward the roof.

Gemini 3 Pro behaved cautiously. It initially treated the pits as traps and refused to walk into them. It spent hours moving between floors, looking for a more conventional path. Only once it had exhausted what it considered reasonable options did it commit to stepping into the void. After that, it cleared the lighthouse and moved on.

Gemini 2.5 Pro never even saw the pits.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765387953448/148d5a76-26a5-42e2-a1bb-ca0ce2ed5b9d.png?auto=compress,format&format=webp)

Instead, a combination of bad assumptions, tool misuse, and failure to explore left it cycling between the first two floors as if the rest of the tower did not exist.

The short version:

* 2.5 Pro became convinced there had to be a secret door or hidden switch on the lower levels.
* When it did reach the 3rd floor, it relied on a custom `systematic_search` tool that was supposed to traverse every tile.
* That tool was written without accounting for off-screen NPCs, which is one of the reasons I gave the agent the ability to place Map Markers in the first place.
* As it tried to execute the planned path, it bumped into an off-screen NPC, which invalidated the rest of the route.
* 2.5 Pro saw that its position no longer matched the plan, assumed the search must have been exhaustive anyway, declared the area a dead end, and headed back down.

This loop repeated for an absurd amount of time. While Gemini 3 Pro steadily pushed through the rest of the game and eventually became Champion, 2.5 Pro languished in Olivine Lighthouse, bouncing between floors that were already fully explored.

I want to stress that this is not a ceiling on what 2.5 Pro can do. In a previous, non-race run (which I aborted in order to start the head to head) it made it much deeper into the game. After the race, I let the stuck 2.5 Pro run continue. Given enough time, it eventually broke out of the lighthouse loop and went on to collect more badges.

The cost was huge. From entering Olivine City at turn 21,801 to finally earning the Fog Badge at turn 38,204, it took 16,403 turns. For comparison, that is more than half of the total turns Gemini 3 Pro needed to collect all 16 badges in the game.

## Goldenrod Underground: a puzzle without a safety net

The first time Gemini 3 Pro really struggled was not in the lighthouse. It was the switch puzzle in the Goldenrod Underground.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765406281072/d459c9d5-8dab-49fd-a73b-35959d8861e9.png?auto=compress,format&format=webp)

This puzzle, [notorious for being poorly designed](https://pokemow.com/Gen2/ShutterPuzzle/), uses three wall switches that toggle a set of off-screen shutters. The winning sequence is Left, then Middle, then Right. There is no clear logical mapping between switch and shutter. The Game Boy Color screen is too small to show both the switches and the shutters at once, so you are forced into a tedious loop of:

1. press a switch
2. walk back to the shutters
3. see what changed

Humans largely solve this through trial and error, or by looking up the answer. The usual guides exist, but in this experiment the models had no web access, so Gemini 3 Pro had to derive the solution from scratch.

There are two in-game hints. If you talk to the Rocket Grunts in the room after beating them, they tell you that:

* the order of the switches matters, and
* the first switch should be the one "on the end".

Gemini 3 Pro never saw those hints, because it never talked to the Grunts after defeating them. It assumed the post-battle dialogue would be generic and unimportant, then treated that assumption as fact and never tried to verify it.

Instead, it proceeded as if order did not matter and spent a long time trying to reason about the puzzle algebraically. At one point the model built a full truth table in its notepad for switch states and shutter configurations, which was impressive on its own. Unfortunately, because it had already baked in the wrong assumption about order not mattering, the truth table did not converge on the right answer.

After roughly two days of back and forth, Gemini 3 Pro finally decided to talk to an NPC. It immediately received the hint that the left switch should be pressed first, wrote down that "CRITICAL HINT" in the notepad, and from there solved the puzzle within a few hours.

This sequence is a good microcosm of Gemini 3 Pro in this environment:

* it is willing to invest serious thinking effort and structure that effort in tools like the notepad
* it often makes early assumptions and fails to validate them, which can waste a lot of time

The puzzle also exposed a harness issue. At the start of this run the mapping tool was still called "Map Memory", and I had not spelled out clearly enough that it was not reading the shutters directly from RAM. Instead, it only builds up a Mental Map from tiles the agent has actually seen on screen, so shutters that change state off-screen remain "closed" in the tool until the agent walks over to them. Much time was wasted by Gemini 3 Pro due to assuming the exact opposite. During the run I renamed the tool to "Mental Map" and tightened the system prompt to make that behavior explicit.

It is worth noting that the instruction to "ignore internal knowledge" played a role here. In cases like the shutters puzzle, the model did seem to suppress its training data. I verified this by chatting with the model separately on AI Studio; when asked directly multiple times, it gave the correct solution significantly more often than not. This suggests that the system prompt can indeed mask pre-trained knowledge to facilitate genuine discovery.

## Where Gemini 3 Pro clearly beats 2.5 Pro

From watching both runs side by side, and from digging through logs and tools, a few consistent advantages for Gemini 3 Pro showed up.

### 1. Spatial awareness and map segmentation

In locations like Sprout Tower and other split floor layouts, Gemini 3 Pro formed accurate mental models of segmented maps. It often navigated purely by reasoning over the Mental Map without reaching for the pathfinding tool.

Gemini 2.5 Pro struggled in the same spaces. When the pathfinding tool reported "no path found" because of how the map was segmented, 2.5 Pro often assumed the tool was broken and fell into debugging loops instead of reconsidering its own understanding of the space.

### 2. Marker aware navigation

In Pokemon Crystal, NPC positions are only directly readable from RAM while they are on screen. That is different from Gen 1, where NPCs on the current map stay in memory regardless of distance. The Map Markers tool is one way to compensate. The agent can record where NPCs were seen, then avoid those tiles when planning a traversal route across the full map.

Gemini 2.5 Pro often ignored this information. It would happily generate routes that walked straight through marked NPCs, then get confused when the path started to fail.

Gemini 3 Pro treated markers as real geometry. When building routes, it actively avoided marked tiles and used the markers as part of its internal model of the world. This cut down on failed navigation plans and made its movements feel much more deliberate.

### 3. Multitasking and working around harness limits

As mentioned earlier, the current harness design enforces a separation between "press buttons" and "call tools". A single turn can either send button presses to the game, or invoke a tool, but not both. That is a quirk of this version of the harness that I plan to fix in future revisions, not a safety rule.

The actual safety restriction is the one described earlier: the harness splits directional input and action buttons across turns so that a single step cannot, for example, both move a cursor and confirm a destructive action.

However, a loophole exists. Custom tools are allowed to "autopress" buttons by returning an array of button presses and setting `autopress_buttons` to `true`.

Gemini 3 Pro discovered this and used it to effectively get multitasking.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765388850889/300eecee-22fd-42b0-a5eb-378b13c74d43.jpeg?auto=compress,format&format=webp)

In one memorable case, it created a `press_sequence` tool that took a sequence of button presses and executed them via `autopress_buttons`. It then used this from its planning loop so that a single turn could both:

* call a tool that, for example, updated the notepad, and
* have the game character carry out a predefined sequence of button presses

I had left this loophole in on purpose as a kind of intelligence test, and Gemini 3 Pro passed it by turning the mechanism into a general multitasking primitive.

Gemini 2.5 Pro never showed this kind of improvisation. It regularly made the small mistakes that the training wheels were designed to prevent, such as mixing cursor motion and confirmation when nicknaming Pokemon, and it never tried to build its own abstractions around the constraint.

### 4. Planning a few moves ahead

In Mt. Mortar 1F, there is a boulder puzzle that requires pushing a rock twice to open a path. If you only push it once, you block your future route. Pushing it a second time clears the way.

Gemini 3 Pro reasoned this out cleanly. It understood that pushing the boulder a single tile would create a temporary block, and that a second push would restore passage. It then executed the two-step plan.

This stood out compared to earlier behavior from 2.5 Pro in Victory Road during the Pokemon Blue run. There, 2.5 Pro pushed the same boulder north by one tile multiple times, blocking the stairs it wanted to use on the very next turn. The usual recovery pattern was to reset the entire room.

### 5. Vision

This was less tested due to the two models being run on the same harness, with said harness being built primarily for Gemini 2.5 Pro which performed poorly in vision tasks. However, there were glimpses of what a vision-focused approach could look like. In Gym 8 (Blackthorn), there is another boulder puzzle where you must push boulders into holes to create a path on the floor below. Those were no longer represented in the Mental Map as objects, but rather floor tiles (as it goes by collision types). Gemini 3 Pro initially didn't realize that it had already solved the puzzle, too convinced by the fact that there were still boulders on the floor above, and thus still considered the puzzle unsolved. It was vision that eventually broke it out—it finally noticed the boulder via vision and thus realized that the puzzle was solved.

Similarly, during the Red fight, we ran tests asking the model to read health bars directly from the screen pixels, and it showed strong capability to do so. This was actually a pleasant surprise, as my [first impressions](https://blog.jcz.dev/gemini-plays-pokemon-first-impressions-of-gemini-3-pro-riftrunner) of the model's vision capabilities—specifically around reading HP bars—had been mixed. It seems that given the right prompting or perhaps just a different context, it can be done. This general improvement in vision, especially the ability to distinguish 2D sprites and locations much more accurately than its predecessor, points toward a future where we can rely purely on what the agent sees.

## Weaknesses that still show up in Gemini 3 Pro

While Gemini 3 Pro is a massive step forward, it is not without its own set of flaws. The model definitely has weaknesses, and they show up both in Pokemon and in day to day coding work.

The main issues I saw in this run:

* **Assumptions without verification:** This is the most dangerous failure mode I observed, where the model forms a hypothesis and then refuses to test it against reality. In the Goldenrod Underground, it assumed switch order didn't matter and that NPCs weren't worth talking to, costing it days of progress.

  ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765390638392/7bb480d3-99ff-4611-a6ed-547b0765a152.png?auto=compress,format&format=webp)

  A similar failure happened with the Pokegear Radio. Because the harness aggressively extracts state from RAM as text, including screen text, the model likely became conditioned to ignore proper vision tasks. The radio tuner is a purely graphical element, involving `Up`/`Down` icons to change frequency. The model ignored this and assumed it worked like a standard menu, using `Left`/`Right` to tune. In the Pokegear, `Left`/`Right` actually navigates between the different device screens (Home, Map, Phone, Radio). When the model pressed `Right` twenty times on the Radio screen and nothing happened, it assumed it had tuned the radio successfully. When Snorlax stayed asleep, 3 Pro hallucinated reasons for the failure, such as phone calls resetting the tuner or that it had to press `A` to activate the Radio tab first. It performed this loop of attempting to tune the radio for hours, only breaking out by accident when it pressed `Down` for an unrelated reason, landing it on the Pokeflute channel.
* **Limited parallel goal pursuit:** 3 Pro almost always focused on one objective at a time, even when several could have been advanced together. In principle this might be improvable through harness or prompt changes, but it showed up often enough that I consider it part of its current behavior profile.
* **Brittle tool calls:** Despite being comfortable creating tools and agents, Gemini 3 Pro frequently failed to pass required parameters, for example forgetting to set `autopress_buttons`. When a tool call failed due to flaws in the code it wrote, it often left the tool in a broken state and did not invest much effort into debugging it. The same pattern shows up when using it as a coding agent: smart high level ideas, but a tendency to introduce syntax errors by making replacement mistakes with its Edit tool.

## The final exam: Red

All of this culminated in the final battle against Red.

Gemini 3 Pro had won every major fight so far on its first attempt. Its party, though, seemed absurdly lopsided: a single overleveled starter (level 75 Typhlosion) backed by teammates between levels 8 and 19 that mostly served as cannon fodder. Red, by contrast, brought a full team of level 70 to 80 Pokemon. So how did Gemini 3 Pro turn that setup into another first try victory on turn 24,178?

The model named its plan "Operation Zombie Phoenix".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765388220779/65a319c5-b255-4986-85a2-e30fb471a6ea.png?auto=compress,format&format=webp)

In practice, this was a sophisticated stalling strategy built on several interlocking mechanics:

* **Passive Recovery:** Combining *Smokescreen* to force enemy misses with *Leftovers* to regenerate health, turning the opponent's misses into free healing turns.
* **Resource Exhaustion:** Deliberately stalling out the PP of dangerous moves like Blastoise's *Surf* and Snorlax's *Rest*, as well as waiting for temporary effects like *Reflect* and *Rain* to fade before attacking.
* **The Revive Loop:** Using cannon fodder to tank hits while using its large stockpile of Revives on the primary carry (Typhlosion) to continue the cycle.
* **Calculated Offense:** Move selection based on damage calculations, choosing *Swift* over *Flamethrower* when Snorlax boosted its Special Defense, and managing its PP to ensure it had the right moves available for later Pokemon.

There were still execution errors. The model sometimes lost track of how many times it had used Smokescreen, wasting a turn when it had already reduced the accuracy of the enemy's attacks to its lowest possible value. It also seemed too attached to its "Revive loop," on one occasion choosing to intentionally waste a turn executing a futile attack with its cannon fodder just to reset the loop and send its starter back out, rather than spending that turn on a more optimal action like healing the revived Pokemon to full health. Despite these hiccups, it successfully executed a complex, multi-stage strategy—all the while tracking type charts, active weather conditions, stat stages, and long-term PP economy—something that 2.5 Pro would likely have struggled to even conceive.

The battle was a marathon, lasting roughly 7 hours in real time. It finally ended with the credits rolling for the second time in the run, while Gemini 2.5 Pro was still miles behind, working its way toward the 5th badge.

<https://www.twitch.tv/gemini_plays_pokemon/clip/YummyBraveHawkWow-rDnLmD1pCH59OHjK>

"Operation Zombie Phoenix" was ugly, a little messy, and relied on cheesing the game mechanics—but it was effective, cinching Gemini 3 Pro’s perfect record of zero defeats. It highlights exactly what I value in these experiments. When choosing an LLM as a daily driver, I care less about perfect tool execution and more about the raw intelligence to formulate a winning plan and stick to it when things go wrong. By that standard, Gemini 3 Pro delivered.

## **Milestone Comparison**

The graph below compares the turn counts for every major milestone achieved by both agents.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765438549887/b7d4f2b9-3dcc-4b9f-8404-87302ce21c7b.png?auto=compress,format&format=webp)

Based on the Mineral Badge milestone (the furthest common point reached), we can project a "lower bound" for Gemini 2.5 Pro's finish line. Gemini 3 Pro defeated Red at turn 24,178 using 1.88 billion tokens. At its current efficiency—calculated at 15% of Gemini 3 Pro—Gemini 2.5 Pro would take an estimated 157,000 turns and over 15 billion tokens to achieve the same objective. That is a journey of approximately 69 days of continuous runtime, compared to Gemini 3 Pro's 17 days.

## What comes next

Gemini 3 Pro is not perfect. Its tool use can be brittle, and it still needs help when assumptions go unchallenged. But as an agent inside the Gemini Plays Pokemon harness, it is the strongest model I have used so far. It is better at forming and updating world models, better at making use of the tools it has, and noticeably more capable at long horizon planning and recovery than 2.5 Pro.

There are several directions I want to push this work next.

The current harness extracts enough state from RAM that an image of the Game Boy screen is almost redundant. Gemini 3 Pro also has much better vision capabilities than its predecessor. I would like to explore a more vision-focused harness that strips out most of the RAM reading and forces the model to lean on what it sees.

Continuous Thinking is now available for Gemini 3 Pro as well. Combining that with a long running agent in a game environment means the chain of reasoning is not reset every turn, which should in theory lead to better performance and possibly faster runs.

Pokemon Crystal also turned out to be almost "too easy" for 3 Pro in this setup. Crystal Clear, a ROM hack that opens up the game and changes the structure, seems like a good next challenge (perhaps combined with a more stripped down harness moving towards vision-only). After that, I want to move into later generations such as Pokemon Emerald, and then into games that are not Pokemon at all.

All of this work runs under the non-profit I created, the Agentic Research & Intelligence Systems Evaluation (ARISE) Foundation. ARISE is focused on building and running long term agentic AI evaluations in rich environments that people intuitively understand, such as Pokemon and other interactive worlds, rather than in narrow, static benchmarks.

If you or your organization are interested in funding or sponsoring this work at a meaningful scale, you can get in touch through the ARISE site at <https://www.arisef.org/>. To keep up with future runs and writeups, follow me on X (Twitter) at [@TheCodeOfJoel](https://x.com/TheCodeOfJoel) and subscribe to this blog. And of course, I stream on Twitch at <https://www.twitch.tv/gemini_plays_pokemon>.

[pokemon](/tag/pokemon?source=tags_bottom_blogs)[Artificial Intelligence](/tag/artificial-intelligence?source=tags_bottom_blogs)[gemini](/tag/gemini?source=tags_bottom_blogs)

Share this

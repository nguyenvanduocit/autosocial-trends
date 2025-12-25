---
source: hackernews
title: Quake's Player Speed (2017)
url: https://rome.ro/quakes-player-speed-1
date: 2025-12-25
---

[Resources](/resources)

[Arcade](/arcade)

[Studios](/studios)

[Articles](/articles)

[Read Me](/readme)

[The Latest](/)

# [Rome.ro](/)

[Resources](/resources)

[Arcade](/arcade)

[Studios](/studios)

[Articles](/articles)

[Read Me](/readme)

[The Latest](/)

![quake1mp2a copy.jpg](https://images.squarespace-cdn.com/content/v1/566d855a841abafcc8ed19c4/1500921562783-4NSUCTLCON3JUYHKOHQA/quake1mp2a+copy.jpg)

**Quake 1 Stories**

# Quake's Player Speed

![](https://images.squarespace-cdn.com/content/v1/566d855a841abafcc8ed19c4/1500921400183-9KOGHP7CCVB832LRSL1Q/doom-dos-front-cover.jpg)

### In 1996...

We were in crunch mode working to get Quake finished. We had to establish a few guidelines to make sure the levels were next-gen, fast, and not too big since people will be downloading the game back during the early internet days.

We decided that map BSP files can't be bigger than 1.4 megabytes. The final complied state of a map was called a BSP file. So we had to make sure we stayed under that value. If a map got too big we had to delete some brushes and try to get the BSP under that size.

To keep the game fast we put a red flickering screen up when the polygon count for world polygons went over 350. Yes, 350. This was at the beginning of using polygons in an FPS and 350 was not too bad of a number back then. If the screen flickered at any time during play we would find the offending view and then start blocking off visibility with new geometry to keep the poly count low and the framerate fast. Also making sure to keep the size under 1.4 megabytes.

The QuakeEd tool that I made was not the best 3D level editor at all – not by a long shot. We tried to keep it as simple as possible because smoothly flying around in 3D space like cameras do nowadays was never done back then. We were figuring it out as we went.

So, QuakeEd made making 3D levels actually painful. We used a primitive called a "brush" that was a 3D rectangle we could drag out into the world and move around. Maps were completely made out of brushes.

There were only three views: a top-down line view, a sideways line Z-view, and a small, fully-rendered 3D view so we could see what the brushes looked like with textures on them. We could see if they were in the right positions.

There was an X that we could drag around and any brushes that were under the X were displayed in the Z-view. That's how we moved brushes up and down in the Z plane.

View fullsize

![](https://images.squarespace-cdn.com/content/v1/566d855a841abafcc8ed19c4/1501354529861-4IEEZFZKK7MTFVH97OHL/image-asset.png)

NeXTSTEP Operating System & QuakeEd, 1995

To create diagonal surfaces from the rectangular brush we would place two points that could slice any geometry that the line between them intersected. Every angle in the game was made this way. For example, on E1M1 (the first level), you step off the slipgate and go down a ramp to the first hallway. To create that ramp I dragged a brush that was left-to-right the length of the ramp, and top-to-bottom the height of the ramp. Keep in mind, I'm working in a view that is looking straight down into the level from above, so I'm creating a ramp in a different rotation than it should be. Then I put one cutting point at the top-left corner and the other cutting point at the bottom-right corner and pressed a key to slice it. Then, I selected the brush and had to rotate it so it was facing the correct way in the world, then used the Z-view to drag it down into place. Finally, I could now drag the edge of the ramp brush to stretch it so it was the correct width of the hallway.

Now imagine doing that for every brush in a level. It took *forever* to make levels, but we got good at it. The problem was, however, that our levels were never very big. Our 1.4 megabyte size limit was capping the size of the levels.

John Carmack decided that we could get more gameplay out of the levels if he slowed down the player's running speed. In DOOM the player went at crazy-fast speeds and it was incredible. In DOOM we could make huge maps and player speed was not a problem. With Quake's maps, the hallways, rooms, and outdoor areas were all smaller because of the file size. So slowing down the player meant it took longer to finish a level, and longer to finish the game overall.

That's how it happened.

[Back to Top](#header)

Powered by [Squarespace](http://www.squarespace.com)

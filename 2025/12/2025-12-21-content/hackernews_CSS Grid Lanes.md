---
source: hackernews
title: CSS Grid Lanes
url: https://webkit.org/blog/17660/introducing-css-grid-lanes/
date: 2025-12-21
---

[WebKit](/)

[ ]

* [Downloads](https://webkit.org/downloads/)
* [ ] [Feature Status](#nav-sub-menu)
  + [CSS Features](https://webkit.org/css-status/)
  + [Standards Positions](https://webkit.org/standards-positions/)
* [ ] [Documentation](#nav-sub-menu)
  + [Web Inspector](/web-inspector)
  + [Tracking Prevention](https://webkit.org/tracking-prevention/)
* [ ] [Policies](#nav-sub-menu)
  + [Project Goals](https://webkit.org/project/)
  + [Bug Prioritization](https://webkit.org/bug-prioritization/)
  + [Bug Report Guidelines](https://webkit.org/bug-report-guidelines/)
  + [Code Style Guidelines](https://webkit.org/code-style-guidelines/)
  + [Commit and Review Policy](https://webkit.org/commit-and-review-policy/)
  + [Feature Policy](https://webkit.org/feature-policy/)
  + [Security Policy](https://webkit.org/security-policy/)
  + [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/)
* [ ] [Contribute](#nav-sub-menu)
  + [Getting Started](https://webkit.org/getting-started/)
  + [Contributing Code](https://webkit.org/contributing-code/)
  + [Testing Contributions](https://webkit.org/testing-contributions/)
  + [How to Report Bugs](https://webkit.org/reporting-bugs/)
  + [GitHub Repository](https://github.com/WebKit/WebKit)
* [ ] [Blog](#nav-sub-menu)
  + [News Posts](https://webkit.org/blog/category/news/)
  + [CSS Posts](https://webkit.org/blog/category/css/)
  + [Contributing Posts](https://webkit.org/blog/category/contributing/)
  + [Privacy Posts](https://webkit.org/blog/category/privacy/)
  + [Performance Posts](https://webkit.org/blog/category/performance/)
  + [JavaScript Posts](https://webkit.org/blog/category/javascript/)
  + [Standards Posts](https://webkit.org/blog/category/standards/)
  + [Web Inspector Posts](https://webkit.org/blog/category/web-inspector/)
  + [Safari Technology Preview Posts](https://webkit.org/blog/category/safari-technology-preview/)

# [Introducing CSS Grid Lanes](https://webkit.org/blog/17660/introducing-css-grid-lanes/ "Permanent Link: Introducing CSS Grid Lanes")

Dec 19, 2025

by Jen Simmons, Brandon Stewart, and Elika Etemad

It’s here, the future of masonry layouts on the web! After the groundwork laid by Mozilla, years of effort by Apple’s WebKit team, and many rounds debate at the CSS Working Group with all the browsers, it’s now clear how it works.

Introducing CSS Grid Lanes.

```
.container {
  display: grid-lanes;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 16px;
}
```

Try it today in [Safari Technology Preview 234](https://developer.apple.com/safari/resources/).

## How Grid Lanes work

Let’s break down exactly how to create this classic layout.

![Classic masonry-style layout of photos of various aspect ratios, all the same width, aligned in six columns](https://webkit.org/wp-content/uploads/Grid-Lanes-classic-light.png)

You can try out this [demo of photo gallery layouts](https://webkit.org/demos/grid3/photos/) today in Safari Technology Preview.

First, the HTML.

```
<main class="container">
  <figure><img src="photo-1.jpg"></figure>
  <figure><img src="photo-2.jpg"></figure>
  <figure><img src="photo-3.jpg"></figure>
  <!-- etc -->
</main>
```

Let’s start by applying `display: grid-lanes` to the `main` element to create a Grid container ready to make this kind of layout. Then we use `grid-template-columns` to create the “lanes” with the full power of CSS Grid.

In this case, we’ll use `repeat(auto-fill, minmax(250px, 1fr))` to create flexible columns at least 250 pixels wide. The browser will decide how many columns to make, filling all available space.

And then, `gap: 16px` gives us 16 pixel gaps between the lanes, and 16 pixel gaps between items within the lanes.

```
.container {
  display: grid-lanes;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 16px;
}
```

That’s it! In three lines of CSS, with zero media queries or container queries, we created a flexible layout that works on all screen sizes.

Think of it like a highway of cars in bumper-to-bumper traffic.

![Cartoon drawing of a highway from above. Nine cars fill four lanes of traffic, bumper to bumper. Each car has a number labeling it, showing the order these would be in HTML.](https://webkit.org/wp-content/uploads/Grid-Lanes-in-STP234.png)

Just like the classic [Masonry library](https://masonry.desandro.com/), as the browser decides where to put each item, the next one is placed in whichever column gets it closest to the top of the window. Like traffic, each car “changes lanes” to end up in the lane that gets them “the furthest ahead”.

This layout makes it possible for users to tab across the lanes to all currently-visible content, (not down the first column below the fold to the very bottom, and then back to the top of the second column). It also makes it possible for you to build a site that keeps loading more content as the user scrolls, infinitely, without needing JavaScript to handle the layout.

## The power of Grid

### Varying lane sizes

Because Grid Lanes uses the full power of CSS Grid to define lanes using [`grid-template-*`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns), it’s easy to create creative design variations.

For example, we can create a flexible layout with alternating narrow and wide columns — where both the first and last columns are always narrow, even as the number of columns changes with the viewport size. This is accomplished with `grid-template-columns: repeat(auto-fill, minmax(8rem, 1fr) minmax(16rem, 2fr)) minmax(8rem, 1fr)`.

![Demo layout of photos, where the 1st, 3rd, 5th, and 7th column are narrow, while the 2nd, 4th and 6th columns are twice as wide. ](https://webkit.org/wp-content/uploads/Grid-Lanes-varying-widths-light.png)

Try out the [demo of photo gallery layouts](https://webkit.org/demos/grid3/photos/) today in Safari Technology Preview.

There’s a whole world of possibilities using `grid-template-*` syntax.

#### Spanning items

Since we have the full power of Grid layout, we can also span lanes, of course.

![A complex layout of titles with teaser text for over two dozen articles — telling people what they'll experience if they open the article. The first teaser has a very large headline with text, and spans four columns. Five more teasers are medium-sized, bowl and next to the hero. The rest of the space available is filled in with small teasers. None of the teasers have the same amount of content as the rest. The heights of each box are random, and the layout tucks each box up against the one above it.](https://webkit.org/wp-content/uploads/Grid-Lanes-newspaper-demo-light.png)

Try out the [demo of newspaper article layout](https://webkit.org/demos/grid3/newspaper/) today in Safari Technology Preview.

```
main {
  display: grid-lanes;
  grid-template-columns: repeat(auto-fill, minmax(20ch, 1fr));
  gap: 2lh;
}
article {
  grid-column: span 1;
}
@media (1250px < width) {
  article:nth-child(1) {
    grid-column: span 4;
  }
  article:nth-child(2), article:nth-child(3), article:nth-child(4), article:nth-child(5), article:nth-child(6), article:nth-child(7), article:nth-child(8) {
    grid-column: span 2;
  }
}
```

All the article teasers are first set to span 1 column. Then the 1st item is specifically told to span 4 columns, while the 2nd – 8th to span 2 columns. This creates a far more dynamic graphic design than the typical symmetrical, everything the same-width, everything the same-height layout that’s dominated over the last decade.

### Placing items

We can also explicitly place items while using Grid Lanes. Here, the header is always placed in the last column, no matter how many columns exist.

![A layout of paintings — each has a bit of text below the painting: title, etc. The paintings are laid out in 8 columns. Over on the right, spanning across two columns is the header of the website. ](https://webkit.org/wp-content/uploads/Grid-Lanes-museum.png)

Try out the [demo of a museum website layout](https://webkit.org/demos/grid3/museum/) today in Safari Technology Preview.

```
main {
  display: grid-lanes;
  grid-template-columns: repeat(auto-fill, minmax(24ch, 1fr));
}
header {
  grid-column: -3 / -1;
}
```

## Changing directions

Yes, lanes can go either direction! All of the examples above happen to create a “waterfall” shape, where the content is laid out in columns. But Grid Lanes can be used to create a layout in the other direction, in a “brick” layout shape.

![Contrasting cartoon drawings: on the left, waterfall layout with boxes lined up in columns, falling down the page. And "brick" layout, with boxes flowing left to right, stacked like bricks in rows.](https://webkit.org/wp-content/uploads/Grid-Lanes-waterfall-v-brick-layout.png)

The browser automatically creates a waterfall layout when you define columns with `grid-template-columns`, like this:

```
.container {
  display: grid-lanes;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

If you want a brick layout in the other direction, instead define the rows with `grid-template-rows`:

```
.container {
  display: grid-lanes;
  grid-template-rows: 1fr 1fr 1fr;
}
```

This works automatically thanks to a new default for[`grid-auto-flow`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-auto-flow), the `normal` value. It figures out whether to create columns or rows based on whether you defined the lanes using `grid-template-columns` or `grid-template-rows`.

The CSS Working Group is still discussing which property will explicitly control the flow orientation, and what its syntax will be. The debate is over whether to reuse `grid-auto-flow` or create new properties like `grid-lanes-direction`. If you’re interested in reading about the options being considered or chime in with your thoughts, see [this discussion](https://github.com/w3c/csswg-drafts/issues/12803#issuecomment-3643945412).

However, since `normal` will be the initial value either way, you don’t have to wait for this decision to learn Grid Lanes. When you define only one direction — `grid-template-rows` *or* `grid-template-columns` — it will Just Work™. (If it doesn’t, check if `grid-auto-flow` is set to a conflicting value. You can[`unset`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/unset) it if needed.)

## Placement sensitivity

“Tolerance” is a new concept created for Grid Lanes. It lets you adjust just how picky the layout algorithm is when deciding where to place items.

Look at the next drawing. Notice that Car 4 is a tiny bit shorter than Car 1. When the “tolerance” is zero, Car 6 ends up in the right-most lane, while Car 7 is on the left. Car 6 ends up behind Car 4 on the right because that gets it a tiny bit closer “down the road” (closer to the top of the Grid container). Car 7 then takes the next-closest-to-the-top slot, and ends up behind Car 1 on the left. The end result? The first horizontal grouping of content is ordered 1, 2, 3, 4, and the next is 7, 5, 6.

![Same cartoon drawing of the highway of bumper to bumper traffic from above.](https://webkit.org/wp-content/uploads/Grid-Lanes-tolerance-1.png)

But the difference in length between Car 1 and Car 4 is tiny. Car 6 isn’t meaningfully closer to the top of the page. And having item 6 on the right, with item 7 on the left is likely an unexpected experience — especially for users who are tabbing through content, or when the content order is somehow labeled.

These tiny differences in size don’t matter in any practical sense. Instead, the browser should consider item sizes like Car 1 and Car 4 to be a tie. That’s why the default for `item-tolerance` is `1em` — which means only differences in content length greater than 1 em will matter when figuring out where the next item goes.

If you’d like the layout of items to shuffle around less, you can set a higher value for `item-tolerance`. In the next digram, the tolerance is set to half-a-car, causing the cars to lay out basically from left to right and only moving to another lane to avoid the extra-long limo. Now, the horizontal groupings of content are 1, 2, 3, 4, and 5, 6, 7.

![Now the highway has the cars ordered in a fashion that's less chaotic.](https://webkit.org/wp-content/uploads/Grid-Lanes-tolerance-2.png)

Think of tolerance as how chill you want the car drivers to be. Will they change lanes to get just a few inches ahead? Or will they only move if there’s a lot of space in the other lane? The amount of space you want them to care about is the amount you set in `item-tolerance`.

Remember that people tabbing through the page will see each item highlighted as it comes into focus, and may be experiencing the page through a screenreader. An item tolerance that’s set too high can create an awkward experience jumping up and down the layout. An item tolerance that’s too low can result in jumping back and forth across the layout more than necessary. Adjust `item-tolerance` to something appropriate for the sizes and size variations of your content.

Currently, this property is named `item-tolerance` in the [specification](https://www.w3.org/TR/css-grid-3/#placement-tolerance) and in Safari Technology Preview 234. However, there is still a chance this name will change, perhaps to something like `flow-tolerance` or `pack-tolerance`. If you have a preference, or ideas for a better name, you can [chime in here](https://github.com/w3c/csswg-drafts/issues/10884#issuecomment-3658059682). Keep an eye out for updates about the final name before using this property on production websites.

## Try it out

Try out Grid Lanes in Safari Technology Preview 234! All of the demos at [webkit.org/demos/grid3](https://webkit.org/demos/grid3/) have been updated with the new syntax, including other use cases for Grid Lanes. It’s not just for images! For example, a mega menu footer full of links suddenly becomes easy to layout.

![A layout of 15 groups of links. Each has between two and nine links in the group — so they are all very different heights from each other. The layout has five columns of these groups, where each group just comes right after the group above it. Without any regard for rows.  ](https://webkit.org/wp-content/uploads/Grid-Lanes-mega-menu-light.png)

Try out the [mega menu demo](https://webkit.org/demos/grid3/megamenu/) today in Safari Technology Preview.

```
.container {
  display: grid-lanes;
  grid-template-columns: repeat(auto-fill, minmax(max-content, 24ch));
  column-gap: 4lh;
}
```

## What’s next?

There are a few last decisions for the CSS Working Group to make. But overall, the feature as described in this article is ready to go. It’s time to try it out. And it’s finally safe to commit the basic syntax to memory!

We’d love for you to make some demos! Demonstrate what new use cases you can imagine. And let us know about any bugs or possible improvements you discover. Ping Jen Simmons on [Bluesky](https://bsky.app/profile/jensimmons.bsky.social) or [Mastodon](https://front-end.social/%40jensimmons) with links, comments and ideas.

Our team has been working on this since mid-2022, implementing in WebKit and writing the web standard. We can’t wait to see what you will do with it.

[NextRelease Notes for Safari Technology Preview 234Learn more](https://webkit.org/blog/17674/release-notes-for-safari-technology-preview-234/)

[PreviouslyWebKit Features for Safari 26.2Learn more](https://webkit.org/blog/17640/webkit-features-for-safari-26-2/)

* [@webkit@front-end.social](https://front-end.social/%40webkit)
* [Site Map](https://webkit.org/sitemap/)
* [Privacy Policy](http://www.apple.com/legal/privacy/)
* [Licensing WebKit](https://webkit.org/licensing-webkit/)
* [WebKit and the WebKit logo are trademarks of Apple Inc.](https://webkit.org/terms-of-use/)

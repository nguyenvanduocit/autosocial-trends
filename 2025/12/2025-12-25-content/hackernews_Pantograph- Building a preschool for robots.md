---
source: hackernews
title: Pantograph: Building a preschool for robots
url: https://pantograph.com/blog/building-a-preschool-for-robots.html
date: 2025-12-25
---

[![Pantograph](../logo.svg)](../index.html)

[Home](../index.html)
[Join Us](../careers.html)

# Pantograph: Building a Preschool for Robots

In order to solve robotics' data problem, we're building a
preschool for robots.

The areas of deep learning that have seen the fastest progress in
the past decade are those where data is abundant: language models
and image generators can train on the entire internet; game-playing
models like AlphaGo can generate data by playing against
themselves. These datasets don't exist for robotics, so we need to
create them from scratch.

At Pantograph, we're creating systems that are capable of
unsupervised data gathering in the real world. Our models build
representations of the world as they go, gradually learning about
the world around them and about how they can influence it. Like
language models, they are trained on enough diverse data to be able
to generalize to new, unseen tasks. Like AlphaGo, our models learn
from experience, continuously improving as they interact with the
world.

## Exploration in the Real World

What would the ideal real-world robotics dataset look like? Scale
is important, as is diversity. The internet has an abundance of
videos, so the most important real-world data to collect is about
things that are difficult to infer from video. We need data about
the properties of materials: texture, viscosity, density, what it
feels like to bend something, to rub something against something
else.

This first phase of data collection will look something like a
robot preschool: thousands of small, inexpensive robots, touching
everything they can get their hands on, tossing things around,
finding the exact balancing point of two wooden blocks, bending,
rubbing, scraping, building up a model of the world around them.
This data will be the foundation upon which we will train
increasingly capable models.

The robots will not only learn about the world around them, but
also about themselves. The resulting models will be native to the
robot's hardware, better able to exploit its capabilities and
idiosyncrasies than any human operator.

## Hardware Demo

Today, we're releasing an early preview of our hardware. It's
designed from the ground up for what we think of as the robot
preschool — this first phase of data collection, exploration, and
long-horizon tasks.

Data scale matters, so minimizing cost matters. This makes our
robot's small size an asset: it's cheaper to build, easier to
scale, and faster to replace. Being small and low to the ground
also makes it safer to be around as it learns - failures are less
damaging, and human supervisors can easily pick it up and move it
around. This is true for toddlers just as much as robots: being
little makes being uncoordinated a lot less dangerous.

Real-world exploration also presents a specific hardware challenge:
durability. Early models won't be especially coordinated: they'll
bump into things, hit the ground, each other, themselves. The
hardware has to survive that. We decided to design our hardware
in-house because every detail matters when building a system that's
robust and reliable at scale. Our team started with component-level
testing — at this point, we've amassed over 10,000 hours of
in-house stress and endurance data validating our most critical
parts.

Our robot is small, strong, and exceptionally durable. It has
treads instead of wheels, which make it more stable, terrain-capable,
and motor efficient. It's an "origami" robot wherever possible:
we lean heavily on 2D profile-based manufacturing — die cutting,
laser cutting and bent sheet metal construction so that
the design is materially efficient and easy to manufacture at
scale.

## Strength

Despite its size, our robot is quite strong. Fully extended,
its arms each have a continuous payload of about 1kg, and it's
capable of moving much heavier objects, as shown in the clips below.

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/Couch pt1.mp4)

One robot pushes a couch

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/Couch pt2.mp4)

One robot pulls a couch and a person

Two robots together can move a couch, a person, and an IKEA
bookshelf (~130kg):

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/Couch pt3.mp4)

One robot cannot pull couch, person and Ikea boxes

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/Couch pt4.mp4)

Two robots move couch, person and Ikea boxes

## Dexterity

Fine manipulation is the most difficult robotic capability, and we've
designed our grippers to be simple while still capable of complex
manipulation tasks. The following clips show our grippers
connecting zip ties, inserting a USB cable into a port, and
building a structure out of wooden blocks:

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/jenga.mp4)

Robot building a structure out of wooden blocks

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/usb.mp4)

Robot inserting a USB cable

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/zip ties 19-9.mp4)

Robot connecting zip ties

## Tool Use

The world around us was mostly designed for humans, and it's
important that a general-purpose robot be able to interact with
tools designed for human hands. The compliance of our robot's
grippers makes it better able to manipulate such tools. The
following clips show our robot using scissors to cut a piece of
paper, an electric screw driver to insert a fastener, and a label
maker to print a message:

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/scissors-trimmed.mp4)

Robot using scissors

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/screw.mp4)

Robot using an electric screw driver

[

](https://f004.backblazeb2.com/file/pantograph-public/launch-12-22-25/label maker.mp4)

Robot using label maker

## Teleoperation Setup

The demos above were collected via a simple teleoperation setup,
pictured below:

[![Teleoperation setup](../teleop_image.jpeg)](../Telop.png)

Our simple teleoperation setup

## Getting This Right Matters

A pantograph is a mechanical linkage that scales and replicates
motion: trace a shape with one end, and the other reproduces it
larger or smaller. We named our company Pantograph because we
believe robotics should do the same for human agency: amplify what
people can do, extend our reach, and multiply our capacity to shape
the world around us.

Generally intelligent robots will reshape how work gets done and
what people are capable of building. This technology touches the
foundations of how society is organized: labor, economics, what it
means to make something. That weight is something we feel.

We want robotics to amplify what it is possible for humans to do.
We're targeting low hardware costs not just because it lets us
train at scale, but because we want to expand who gets to build and
what becomes possible to build. More labs, more workshops, more
ambitious projects that today are impractical. This is a future
that should be widely shared.

We structured Pantograph as a Public Benefit Corporation because we
take seriously both the promise and the responsibility of what
we're building. The PBC structure encodes that commitment into how
we're governed, ensuring that as we scale, we remain accountable to
something beyond short-term returns.

## What's Next

We're in the process of massively scaling up data collection with our
hardware. We own the entire stack, from hardware and firmware to
our training infrastructure and learning algorithms. In all of
these areas, there is much work to do.

On the hardware side, we're scaling to thousands of robots over the
coming months. We'll be iterating on our designs for reliability,
manufacturability, and capability, and keeping the robots running
continuously. We're deepening our relationships with suppliers who
can scale with us. Beyond this generation, we're interested in
building hardware that meets a wider range of needs and exploring
new form factors.

On the research side, there are many unanswered questions: what's
the right task distribution? What's the right way to incorporate
pretraining? How can we steer the resulting models? These
algorithms have never been scaled up before, and there is a lot of
room for new ideas.

If the prospect of designing hardware and algorithms that can learn
continuously in the real world sounds exciting to you,
[we're hiring](../careers.html)!

![Pantograph robot](../preview-image.png)

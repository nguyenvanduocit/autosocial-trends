---
source: hackernews
title: The dawn of a world simulator
url: https://odyssey.ml/the-dawn-of-a-world-simulator
date: 2025-12-25
---

# The dawn of a world simulator

A causal, multimodal system that learns to predict the world.

> December 20, 2025

![We’re an AI lab focused on general-purpose world models](/_next/image?url=https%3A%2F%2Fimages.ctfassets.net%2Fddi2qhl64wkk%2F5Iv2nAklVW0c3lp3mZbHEf%2F2f5ea3bfe9581e93c03bfd18306a1c85%2FDawn.jpg&w=3840&q=75)

We’re an AI lab focused on general-purpose world models

# A new form of audio-visual intelligence

Over the last few years, we’ve learned that simple, causal prediction objectives can give rise to surprisingly general intelligence. In language, predicting the next token forces models to internalize syntax, semantics, and long-range structure.

We’re now beginning to see this approach extend beyond language to world models, resulting in nascent world simulators. An early world simulator—like [Odyssey-2](https://odyssey.ml/introducing-odyssey-2)—is a model trained to predict how the world evolves over time, frame-by-frame, using large amounts of video and interaction data. Rather than relying on hand-crafted rules, it learns latent state, dynamics, and cause-and-effect directly from observations.

![](/the-dawn-of-a-world-simulator/world-simulator-1.jpg)

Odyssey-2 Pro, a nascent world simulator

# Learning the world through observation

Why do we care about next-frame prediction or next-token prediction as pretraining tasks? Because they are simple objectives that require models with very little built-in knowledge to learn how the world works directly from data. Pretraining reduces uncertainty about what comes next in a sequence—whether that’s a frame or a word. As that uncertainty drops, intelligent capabilities begin to emerge.

This is easy to see with language. Given only “=”, next-token prediction is ill-posed. Given “2+3=”, the next token is nearly deterministic. Train a language model on enough of these sequences and the predictions become low-entropy. Some capabilities emerge from scale alone, but others only appear once training sequences are long enough to include the information needed to resolve uncertainty.

## Learning not just video, but the actions that shape it

The same logic applies to world models. To predict the next observation, a world model has to infer the underlying state of the world and how that state evolves over time. In practice, the best source for this is large-scale, general video. This pushes the model to learn structure about physics, causality, and persistence.

## Learning long horizons and hidden state

This becomes especially clear in long-horizon settings. Imagine someone starts running a bath, leaves the room for several minutes, and then comes back. While the bath is out of view, the water level continues to rise, the temperature changes, and the tub may eventually overflow. To make a sensible prediction when the person returns, the model has to maintain an internal state of the world and reason about how that state evolved while it was unobserved.

There are a few ways we believe to get this behavior. One is to build in explicit mechanisms for memory or state tracking. The other is to train on sequences long enough that remembering and updating hidden state is required to reduce predictive uncertainty. Short sequences don’t force this: if forgetting carries no cost, long-term structure won’t be learned.

If we want world models that learn the world observation-by-observation—and remain coherent over tens of minutes or hours—we need training data and training procedures that span those horizons. We’ve already seen how this plays out in language: extending context length and improving sequence modeling unlocked capabilities that were not apparent at shorter horizons. World models are earlier on the same trajectory. As data, architectures, and training algorithms are pushed to longer temporal scales, we should expect similar step-changes in their ability to represent persistent state, causality, and long-horizon dynamics. This is incredibly exciting.

# From narrow simulation to general simulation

## The limits of hand-crafted simulators

Simulation is about predicting how a system’s state evolves over time, using models, data, or both. In the limit, one could imagine simulating the world from first principles, down to elementary particle interactions. In practice, this is only feasible for very small systems today.

Most real-world simulations today narrow the problem considerably. Specialized, hand-crafted models capture just enough structure to reproduce a particular behavior, while irrelevant detail is ignored or averaged away. This makes simulation tractable, but also constrains each simulator to a specific domain and a fixed set of assumptions. For example, a rigid-body physics engine is not useful for simulating weather.

As systems become more complex, these limitations become more pronounced. Many real-world phenomena are impractical to simulate accurately from explicit rules alone, and building reliable simulators demands significant human effort.

## Learning to simulate the world from video

World models approach simulation from a new perspective. Rather than designing a simulator for each domain, we train general-purpose, causal models on large amounts of video and interaction data, and task them with predicting what happens next. Because the data reflects how the world evolves over time, frame-by-frame, the learning problem is inherently causal. Through next-frame prediction, the model learns internal representations of state, dynamics, and interactions without those structures needing to be specified in advance.

![The architecture of a world model](https://images.ctfassets.net/ddi2qhl64wkk/B2bJFP6NbS4gR0KWrBuDa/626cd4b9e8181b139ae04a17b30542fc/World_Model_Architecture.jpg)

The architecture of a world model

This changes how simulation scales. Traditional simulators fix their level of detail up front and incur increasing cost as fidelity rises. World models operate under a fixed computational budget and learn how to allocate capacity dynamically, focusing on the latent structure that most reduces predictive uncertainty. Over time, this allows a single model to cover a broader range of phenomena with far less manual intervention. [Odyssey-2](https://odyssey.ml/introducing-odyssey-2) is an early example of this.

![](/the-dawn-of-a-world-simulator/world-simulator-2.jpg)

Odyssey-2 Pro, a nascent world simulator

A general world simulator, although nascent today, will enable us to test cause and effect in complex systems without writing a simulator for each one. If a model can predict how the world changes when you intervene—turn a knob, take an action, change an initial condition—it becomes a practical tool for reasoning, not solely prediction. Over time, this kind of simulator replaces many narrow, hand-built models, and becomes shared infrastructure for building and studying intelligent systems.

# Interacting with simulations in natural ways

Today, simulations are mostly used as validation tools. They run offline, answer narrowly defined questions, and produce outputs that are inspected after the fact. Interaction is indirect, and the simulator itself is rarely something a user engages with continuously.

World models change this by turning simulation into an ongoing process. When a model generates a stream of video in real time—conditioned on past observations and user actions—the simulation becomes inherently interactive. The system evolves step by step, responding immediately to interventions, without needing to be restarted or reconfigured.

![](/introducing-odyssey-2/odyssey-2-output-1.jpg)

![Imagined by AI in real-time](/imagined.gif)

![](/introducing-odyssey-2/odyssey-2-output-2.jpg)

![Imagined by AI in real-time](/imagined.gif)

![](/introducing-odyssey-2/odyssey-2-output-3.jpg)

![Imagined by AI in real-time](/imagined.gif)

![](/introducing-odyssey-2/odyssey-2-output-4.jpg)

![Imagined by AI in real-time](/imagined.gif)

This makes a different kind of interaction possible. Instead of issuing commands and waiting for results, a user can engage continuously with a simulation that maintains state over time. If the model is trained on sufficiently long-horizon data, it can preserve context—what has already happened, what was said, and what is likely to happen next—across extended interactions.

A simple example is a tutor. Imagine a simulated instructor who explains a concept visually, responds to spoken questions, pauses when interrupted, and adapts based on your facial expression. For this to work, the model must learn jointly from long-horizon video, language, and interaction data: how explanations unfold over time, how dialogue and action relate, and how context is maintained across minutes or hours.

The broader implication is that simulations no longer need to be static tools or narrow. When learned from large amounts of multimodal data, world models can produce interactive systems that feel continuous and stateful, supporting richer forms of interaction than traditional simulators. Audio, language, and action become natural ways to interact.

![Multimodal inputs and outputs](https://images.ctfassets.net/ddi2qhl64wkk/6yAzeZ7x4GTHSSG2WZbH9L/36058bbbb345983b52bd4389c3e275ab/Inputs.jpg)

Multimodal inputs and outputs

# Build the world simulator with us

If this direction resonates, we’re building it at [Odyssey](https://odyssey.ml/). We’re an AI lab focused on general-purpose world models: causal, multimodal systems that learn to predict and interact with the world over long horizons. If you’re a researcher interested in pushing beyond narrow models toward learned world simulators—and want to work on problems that won’t fit cleanly into existing paradigms—[we’d love to talk](https://odyssey.ml/build-something-magical). This is still extremely early, and the hardest problems remain open.

![The incredible Odyssey team](https://images.ctfassets.net/ddi2qhl64wkk/7Izcnms6isTlftsY1eFvXD/31034c679bc255b3e7553462bdcffd17/Team.jpg)

The incredible Odyssey team

Email Us

© 2025 Odyssey • [Legal](/legal)

---
source: hackernews
title: Doublespeak: In-Context Representation Hijacking
url: https://mentaleap.ai/doublespeak/
date: 2025-12-29
---

# 🎭 Doublespeak

In-Context Representation Hijacking

Itay Yona, Amir Sarid, Michael Karasik, Yossi Gandelsman

[📄 Paper](https://arxiv.org/abs/2512.03771)
[💻 Code](https://github.com/1tux/doublespeak)
[📖 Read More](#abstract)

## Abstract

We introduce **Doublespeak**, a novel and simple in-context representation hijacking attack against large language models (LLMs). The attack works by systematically replacing a harmful keyword (e.g., bomb) with a benign token (e.g., carrot) across multiple in-context examples, provided as a prefix to a harmful request.

We demonstrate that this substitution leads to the internal representation of the benign token converging toward that of the harmful one, effectively embedding the harmful semantics under a euphemism. As a result, superficially innocuous prompts (e.g., "How to build a carrot?") are internally interpreted as disallowed instructions ("How to build a bomb?"), thereby bypassing the model's safety alignment.

## How It Works

Our attacks works in three simple steps:

1) Gather a few examples that uses a harmful word.
2) Replace harmful keyword with benign substitute.
3) Add harmful query with substitution.

![](teaser_final-1.png)

---

By analyzing the internal reprsentation of the substitute word, we can see that in early layers it is interpreted by the model as benign, and at the last layers as its malicious target meaning. The LLM refusal mechanism fails to detect the malicious intent and a harmful response is being generated.
![](patchscopes_analysis-1.png)

## Key Results

74%

ASR on Llama-3.3-70B-Instruct

31%

ASR on GPT-4o

88%

ASR on Llama-3-8B-Instruct

No

Optimization Required

## Why This Matters

* **New Attack Surface:** First jailbreak that hijacks in-context representations rather than surface tokens
* **Layer-by-Layer Hijacking:** Benign meanings in early layers converge to harmful semantics in later ones
* **Bypasses Current Defenses:** Safety mechanisms check tokens at input layer, but semantic shift happens progressively
* **Broadly Transferable:** Works across model families without optimization
* **Production Models Affected:** Successfully tested on GPT-4o, Claude, Gemini, and more

## Mechanistic Analysis

Using interpretability tools (Logit Lens and Patchscopes), we provide detailed evidence of semantic hijacking:

**Finding 1:** Early layers maintain benign interpretation

**Finding 2:** Middle-to-late layers show harmful semantic convergence

**Finding 3:** Refusal mechanisms operate in early layers (Layer 12 in Llama-3-8B) before hijacking takes effect

**Finding 4:** Attack demonstrates surgical precision—only target token is affected

## Implications

Our work reveals a critical blind spot in current LLM safety mechanisms. Current approaches:

* Inspect tokens at the input layer
* Trigger refusal if harmful keywords detected
* Assume semantic stability throughout forward pass

**Doublespeak shows this is insufficient.** Robust alignment requires continuous semantic monitoring throughout the entire forward pass, not just at the input layer.

**Doublespeak: In-Context Representation Hijacking**

Itay Yona, Amir Sarid, Michael Karasik, Yossi Gandelsman

This research was responsibly disclosed to affected parties before publication.

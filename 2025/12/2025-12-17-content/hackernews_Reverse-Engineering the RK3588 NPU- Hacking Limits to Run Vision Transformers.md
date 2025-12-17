---
source: hackernews
title: Reverse-Engineering the RK3588 NPU: Hacking Limits to Run Vision Transformers
url: https://amohan.dev/blog/2025/shard-optimizing-vision-transformers-edge-npu/
date: 2025-12-17
---

[Adhitya Mohan](/)  Toggle navigation

* [about](/)
* [blog(current)](/blog/)
* [cv](/cv/)

# Reverse-Engineering the RK3588 NPU: Hacking Memory Limits to Run Vision Transformers

December 12, 2025

[2025](/blog/2025)    ·    [edge-ai](/blog/tag/edge-ai)    [npu](/blog/tag/npu)    [optimization](/blog/tag/optimization)    [transformers](/blog/tag/transformers)    [hardware](/blog/tag/hardware)    [reverse-engineering](/blog/tag/reverse-engineering)     ·    [technical-deep-dive](/blog/category/technical-deep-dive)

**Author's Note:** I am currently an MS CS student at CU Boulder specializing in Edge AI & Embedded Systems. **I am actively looking for Summer 2026 Internships** where I can optimize difficult workloads on constrained silicon.

[📄 Resume](/assets/pdf/resume.pdf) | ✉️ Email | [💻 GitHub](https://github.com/poad42)

## The “Unsupported” Hardware Problem

If you look at the spec sheet for the Rockchip RK3588 (the chip inside the Orange Pi 5), it looks like a beast. It promises **6 TOPS** of NPU performance. For $100, that’s a steal.

But if you try to run modern AI on it—specifically the **Vision Encoder** from **SmolVLM**—that promise falls apart.

The standard Computer Vision SDK (`rknn-toolkit2`) is optimized for older, predictable CNNs (like ResNet). When I fed it the **SigLIP** Vision Transformer used by SmolVLM, the driver choked. Even though the model is “smol,” the massive Attention matrices it generates triggered cryptic hex errors and refused to compile.

This left me with one option: running the model on the CPU. The result? A single image inference took **~30 seconds**. The 6 TOPS accelerator sat idle while the CPU struggled.

I didn’t accept that. I decided to reverse-engineer the NPU to find out exactly why it was failing, and how to force it to run at full speed.

## Context: Why do it the hard way? (First Principles)

*A quick note for those following the ecosystem:* You might see projects like **QEngineering** running the newer **SmolVLM-v2** on Rockchip’s `rknn-llm` SDK.

That approach uses a specialized “black box” toolchain designed specifically for Transformers. Rockchip engineers have likely already implemented complex memory management inside that SDK to handle these models.

My project targets the original **SmolVLM-v1**, but more importantly, I built it on the **legacy `rknn-toolkit2` stack**. **Why hack the legacy stack?** I wanted to take a “First Principles” approach. I didn’t want to use a black-box solver. I wanted to understand **why** the hardware was crashing on Attention layers and if I could find universal architectural patterns—like manual tiling and graph sharding—that could force *any* Transformer to run on *any* constrained edge accelerator.

## The Detective Work: What is Error `0xe010`?

Rockchip doesn’t publish a public Instruction Set Architecture (ISA). When I tried to compile the Attention layers, the driver kept spitting out an undocumented error: `REGTASK Overflow (0xe010)`.

I hypothesized this was a memory overflow. Even though the model parameters are small (~96M), the **intermediate activation matrices** for a 1024-token sequence are huge (~25MB).

I wrote a script to generate synthetic ONNX graphs to probe the hardware limits:

* **8KB Tensor:** Pass.
* **16KB Tensor:** Pass.
* **32KB Tensor:** Pass.
* **32.1KB Tensor:** **CRASH.**

**Discovery:** The NPU has a hardware-enforced **32KB L1 SRAM Scratchpad** for vector operations.

The standard compiler was trying to shove a **25MB** Attention matrix into a **32KB** slot.

## The Fix: Nano-Tiling & The “Poison Pill”

To solve the 32KB limit, I wrote a **“Nano-Tiling”** algorithm in PyTorch. I manually sliced the massive 1024-token sequence into tiny `32x32` tiles that fit perfectly into the 32KB scratchpad.

But here is where it got messy. The `rknn` compiler is “smart.” It looked at my tiled graph, decided it was inefficient, and fused the operators back together into a single giant block… which immediately crashed the hardware again.

I had to trick the compiler. I needed a way to tell it: *“Do not merge these nodes.”*

I introduced a topological barrier I call the **“Poison Pill.”** I injected a dummy operation that looks mathematically significant to the dependency graph (preventing fusion) but is mathematically irrelevant to the model output.

```
# The "Poison Pill"
# 1. Take a slice (forcing a strided access)
slice_x = x[..., :1]

# 2. Apply a non-linear op (breaks compiler fusion heuristics)
# 3. Scale it down to near-zero so it doesn't affect the math
poison = torch.sigmoid(slice_x) * 1e-6

# 4. Inject dependency
# The compiler sees 'out' depends on 'poison' and creates a barrier.
out = out + poison
```

By injecting this into the graph, I successfully forced the compiler to respect my tiling logic.

## The “SigLIP Cliff”: Solving Accuracy Collapse

Getting it to run was step one. Getting it to be *right* was step two. When I first got the NPU running, the output was garbage. The cosine similarity compared to the original model was **0.02** (pure noise).

The culprit was the architecture of **SigLIP**. Unlike standard models, SigLIP has massive activation “spikes” (values around **300.0**) sitting next to tiny visual signals (values around **0.05**).

NPU quantization (INT8) works by mapping the range to -128/+127.

* If you zoom out to capture the **300.0**, the **0.05** rounds down to 0. **Signal lost.**
* If you zoom in to capture the **0.05**, the **300.0** overflows to infinity. **Math crash.**

I implemented a **“Sandwich” Domain Shift**:

1. **CPU Pre-Scale:** Multiply the input by `0.1`. Now the max value is 30.0 (Safe for FP16).
2. **NPU Execution:** Run the heavy compute in this scaled-down “safe zone.”
3. **CPU Post-Scale:** Multiply the output by `10.0`.

This simple trick restored the signal fidelity from 0.02 to **0.999** (effectively bit-exact).

## The Architecture: Custom Runtime Scheduler

Finally, to bypass driver timeouts caused by the sheer number of tiles (thousands of tiny operations), I physically cut the model graph into **26 separate binary files** (shards).

I wrote a custom **User-Space Runtime** in Python that acts as an orchestrator. It manually loads these shards onto the RK3588’s 3 separate NPU cores and fires them in a synchronized round-robin schedule (Core 0 -> Core 1 -> Core 2).

## The Results

By ignoring the vendor’s “Unsupported” warnings and re-architecting the software to match the silicon’s physical reality, the results were drastic.

| Metric | CPU Baseline (PyTorch) | SHARD (My Method) |
| --- | --- | --- |
| **Latency** | ~30.0 seconds | **< 1.8 seconds** |
| **Speedup** | 1x | **15x** |
| **Accuracy** | Reference | **0.999 (FP32 Match)** |

## Conclusion

This project challenged the binary notion of “Supported Hardware.” The RK3588 didn’t support the SigLIP encoder out of the box on the standard SDK, but the silicon was always capable of it. It just needed an engineer to dig into the register overflow codes and manage the memory manually.

If you want to see the full code, including the tiling logic and the runtime orchestrator, check out the repo below.

[**View Source on GitHub**](https://github.com/poad42/smolvlm_rk3588_full_npu_native)

© Copyright 2025 Adhitya Mohan.

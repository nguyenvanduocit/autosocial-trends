---
source: hackernews
title: Nvidia Nemotron 3 Family of Models
url: https://research.nvidia.com/labs/nemotron/Nemotron-3/
date: 2025-12-17
---

* [NVIDIA Nemotron](https://research.nvidia.com/labs/nemotron/)
* [Projects](https://research.nvidia.com/labs/nemotron/projects/)

![NVIDIA](https://research.nvidia.com/labs/nemotron/images/nvidia-ai.jpg)

### NVIDIA

Follow

# NVIDIA Nemotron 3 Family of Models

**Published:** December 15, 2025

![](images/nvidia-nemotron-3-nano/nano-3-aa.png)

[Models](https://huggingface.co/collections/nvidia/nvidia-nemotron-v3)    [Nemotron 3 White Paper](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-White-Paper.pdf)    [Nano Tech Report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Nano-Technical-Report.pdf)

We announce NVIDIA Nemotron 3, the most efficient family of open models with leading accuracy for agentic AI applications. The Nemotron 3 family consists of three models: Nano, Super, and Ultra. These models deliver strong agentic, reasoning, and conversational capabilities.

Nano, the smallest model, outperforms comparable models in accuracy while remaining extremely cost-efficient for inference. Super is optimized for collaborative agents and high-volume workloads such as IT ticket automation. Ultra, the largest model, provides state-of-the-art accuracy and reasoning performance.

We are releasing the Nemotron 3 Nano [model](https://huggingface.co/collections/nvidia/nvidia-nemotron-v3) and [technical report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Nano-Technical-Report.pdf). Super and Ultra releases will follow in the coming months.

## Nemotron 3 technologies

* **Hybrid MoE**: Nemotron 3 family of models utilize a hybrid Mamba-Transformer MoE architecture to provide best-in-class throughput while having better or on-par accuracy than standard Transformers.
* **LatentMoE**: Super and Ultra utilize Latent MoE, a novel hardware-aware expert design for improved accuracy.
* **Multi-Token Prediction**: Super and Ultra incorporate MTP layers for improved long-form text generation efficiency and better model quality.
* **NVFP4**: Super and Ultra are trained with NVFP4.
* **Long Context**: Nemotron 3 models support context length up to 1M tokens.
* **Multi-environment Reinforcement Learning Post-training**: Nemotron 3 models are trained using a diverse set of RL environments helping models achieve superior accuracy across a broad range of tasks.
* **Granular Reasoning Budget Control at Inference Time**: Nemotron 3 models are trained to work with inference-time budget control.

## Nemotron 3 Nano

![](images/nvidia-nemotron-3-nano/nano-3-comparison.png)

Nemotron 3 Nano is a 3.2B active (3.6B with embeddings), 31.6B total parameter model. It achieves better accuracy than our previous generation Nemotron 2 Nano while activating less than half of the parameters per forward pass.

#### Key highlights:

* More **accurate** than GPT-OSS-20B and Qwen3-30B-A3B-Thinking-2507 on popular benchmarks spanning different categories.
* On the 8K input / 16K output setting with a single H200, Nemotron 3 Nano provides **inference throughput** that is **3.3x higher than Qwen3-30B-A3B and 2.2x higher than GPT-OSS-20B**.
* Supports **context length up to 1M tokens** while outperforming both GPT-OSS-20B and Qwen3-30B-A3B-Instruct-2507 on RULER across different context lengths.
* We are releasing the model weights, training recipe, and all the data for which we hold redistribution rights.

## Open Source

Along with the [Nemotron 3 white paper](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-White-Paper.pdf) and the [Nano 3 technical report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Nano-Technical-Report.pdf), we are releasing the following:

#### Checkpoints:

* [**Nemotron 3 Nano 30B-A3B FP8**](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-FP8): the final post-trained and FP8 quantized Nano model
* [**Nemotron 3 Nano 30B-A3B BF16**](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16): the post-trained Nano model
* [**Nemotron 3 Nano 30B-A3B Base BF16**](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-Base-BF16): the pre-trained base Nano model
* [**Qwen-3-Nemotron-235B-A22B-GenRM**](https://huggingface.co/nvidia/Qwen3-Nemotron-235B-A22B-GenRM): the GenRM used for RLHF

#### Data:

* [**Nemotron-CC-v2.1**](https://huggingface.co/datasets/nvidia/Nemotron-CC-v2.1): 2.5 trillion new English tokens from Common Crawl, including curated data from 3 recent snapshots, synthetic rephrasing, and translation to English from other languages.
* [**Nemotron-CC-Code-v1**](https://huggingface.co/datasets/nvidia/Nemotron-CC-Code-v1): A pretraining dataset consisting of 428 billion high-quality code tokens obtained from processing Common Crawl Code pages using the Lynx + LLM pipeline from [Nemotron-CC-Math-v1](https://huggingface.co/datasets/nvidia/Nemotron-CC-Math-v1). Preserves equations and code, standardizes math equations to LaTeX, and removes noise.
* [**Nemotron-Pretraining-Code-v2**](https://huggingface.co/datasets/nvidia/Nemotron-Pretraining-Code-v2): Refresh of curated GitHub code references with multi-stage filtering, deduplication, and quality filters. Large-scale synthetic code data.
* [**Nemotron-Pretraining-Specialized-v1**](https://huggingface.co/datasets/nvidia/Nemotron-Pretraining-Specialized-v1): Collection of synthetic datasets for specialized areas like STEM reasoning and scientific coding.
* [**Nemotron-SFT-Data**](https://huggingface.co/collections/nvidia/Nemotron-v3-Post-Training): Collection of new Nemotron 3 Nano SFT datasets.
* [**Nemotron-RL-Data**](https://github.com/NVIDIA-NeMo/Gym): Collection of new Nemotron 3 Nano RL datasets.

#### Model Recipes:

* [**NVIDIA Nemotron Developer Repository**](https://github.com/NVIDIA-NeMo/Nemotron)

For more details, please refer to the following:

* Nemotron 3 Blogs
  + [HuggingFace](https://huggingface.co/blog/nvidia/nemotron-3-nano-efficient-open-intelligent-models)
  + [NVIDIA Tech Blog](https://developer.nvidia.com/blog/inside-nvidia-nemotron-3-techniques-tools-and-data-that-make-it-efficient-and-accurate/)
* Nemotron 3 white paper: [NVIDIA Nemotron 3: Efficient and Open Intelligence](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-White-Paper.pdf)
* Nemotron 3 Nano technical report: [Nemotron 3 Nano: Open, Efficient Mixture-of-Experts Hybrid Mamba-Transformer Model for Agentic Reasoning](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Nano-Technical-Report.pdf)

#### Share on

 [Twitter](https://twitter.com/intent/tweet?text=https://research.nvidia.com/labs/nemotron/Nemotron-3/ "Share on Twitter")
 [Facebook](https://www.facebook.com/sharer/sharer.php?u=https://research.nvidia.com/labs/nemotron/Nemotron-3/ "Share on Facebook")
 [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://research.nvidia.com/labs/nemotron/Nemotron-3/ "Share on LinkedIn")

Previous
[Next](https://research.nvidia.com/labs/nemotron/nemotron-cascade/ "Nemotron-Cascade: Scaling Cascaded Reinforcement Learning for General-Purpose Reasoning Models
")

* [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/)
* [Manage My Privacy](https://www.nvidia.com/en-us/privacy-center/)
* [Do Not Sell or Share My Data](https://www.nvidia.com/en-us/preferences/email-preferences/)
* [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/)
* [Accessibility](https://www.nvidia.com/en-us/about-nvidia/accessibility/)
* [Corporate Policies](https://www.nvidia.com/en-us/about-nvidia/company-policies/)
* [Contact](https://www.nvidia.com/en-us/contact/)

© 2025 NVIDIA. Powered by [Jekyll](http://jekyllrb.com) & [AcademicPages](https://github.com/academicpages/academicpages.github.io), a fork of [Minimal Mistakes](https://mademistakes.com/work/minimal-mistakes-jekyll-theme/).

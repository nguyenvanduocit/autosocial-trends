---
source: hackernews
title: Qwen-Image-Layered: transparency and layer aware open diffusion model
url: https://huggingface.co/papers/2512.15603
date: 2025-12-20
---

[![Hugging Face's logo](/front/assets/huggingface_logo-noborder.svg)
Hugging Face](/)

* [Models](/models)
* [Datasets](/datasets)
* [Spaces](/spaces)
* Community
* [Docs](/docs)
* [Enterprise](/enterprise)
* [Pricing](/pricing)
* ---
* [Log In](/login)
* [Sign Up](/join)

[Papers](/papers)

arxiv:2512.15603

# Qwen-Image-Layered: Towards Inherent Editability via Layer Decomposition

Published on Dec 17

·

Submitted by
[![](https://cdn-avatars.huggingface.co/v1/production/uploads/6039478ab3ecf716b1a5fd4d/_Thy4E7taiSYBLKxEKJbT.jpeg)

taesiri](/taesiri)
on Dec 18

[[ ]

Upvote

34](/login?next=%2Fpapers%2F2512.15603)

* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/6039478ab3ecf716b1a5fd4d/_Thy4E7taiSYBLKxEKJbT.jpeg)](/taesiri "taesiri")
* [![](/avatars/7978304e3fe99b0d4d0712441c6a24f3.svg)](/ghy0324 "ghy0324")
* [![](/avatars/d1ecd68111dbdb25168ebc7edfd02895.svg)](/OmniSVG "OmniSVG")
* [![](/avatars/640803ef641decd5c30894155bf09b6a.svg)](/UDCAI "UDCAI")
* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/620783f24e28382272337ba4/zkUveQPNiDfYjgGhuFErj.jpeg)](/Tommy930 "Tommy930")
* [![](/avatars/94cf959a86c0ce58263db710d693fb9e.svg)](/boomcheng "boomcheng")
* [![](/avatars/b05958b28a8964ee36bd8a3994b2aaec.svg)](/kkmh-op "kkmh-op")
* [![](/avatars/79df524de88db4bd50aaa5b00c92a0f4.svg)](/jide "jide")
* +26

Authors:

Shengming Yin
,

Zekai Zhang
,

Zecheng Tang
,

Kaiyuan Gao
,

Xiao Xu
,

Kun Yan
,

Jiahao Li
,

Yilei Chen
,

Yuxiang Chen
,

Heung-Yeung Shum
,

Lionel M. Ni
,

Jingren Zhou
,

Junyang Lin
,

Chenfei Wu

## Abstract

Qwen-Image-Layered decomposes images into semantically disentangled RGBA layers using a diffusion model, enabling independent editing of each layer and improving decomposition quality and consistency.

AI-generated summary

Recent visual generative models often struggle with consistency during image editing due to the entangled nature of raster images, where all visual content is fused into a single canvas. In contrast, professional design tools employ layered representations, allowing isolated edits while preserving consistency. Motivated by this, we propose Qwen-Image-Layered, an end-to-end [diffusion model](/papers?q=diffusion%20model) that decomposes a single RGB image into multiple semantically disentangled [RGBA layers](/papers?q=RGBA%20layers), enabling [inherent editability](/papers?q=inherent%20editability), where each RGBA layer can be independently manipulated without affecting other content. To support variable-length decomposition, we introduce three key components: (1) an [RGBA-VAE](/papers?q=RGBA-VAE) to unify the latent representations of RGB and RGBA images; (2) a [VLD-MMDiT](/papers?q=VLD-MMDiT) (Variable Layers Decomposition MMDiT) architecture capable of decomposing a variable number of image layers; and (3) a [Multi-stage Training](/papers?q=Multi-stage%20Training) strategy to adapt a pretrained image generation model into a multilayer image decomposer. Furthermore, to address the scarcity of high-quality multilayer training images, we build a pipeline to extract and annotate multilayer images from Photoshop documents ([PSD](/papers?q=PSD)). Experiments demonstrate that our method significantly surpasses existing approaches in [decomposition quality](/papers?q=decomposition%20quality) and establishes a new paradigm for [consistent image editing](/papers?q=consistent%20image%20editing). Our code and models are released on https://github.com/QwenLM/Qwen-Image-Layered{https://github.com/QwenLM/Qwen-Image-Layered}

[View arXiv page](https://arxiv.org/abs/2512.15603)
[View PDF](https://arxiv.org/pdf/2512.15603)
[Add to collection](/login?next=%2Fpapers%2F2512.15603)

### Community

![](https://cdn-avatars.huggingface.co/v1/production/uploads/6039478ab3ecf716b1a5fd4d/_Thy4E7taiSYBLKxEKJbT.jpeg)

[taesiri](/taesiri)

Paper submitter
[2 days ago](#694373dfc2caeb482b2a4c62)

Recent visual generative models often struggle with consistency during image editing due to the entangled nature of raster images, where all visual content is fused into a single canvas. In contrast, professional design tools employ layered representations, allowing isolated edits while preserving consistency. Motivated by this, we propose \textbf{Qwen-Image-Layered}, an end-to-end diffusion model that decomposes a single RGB image into multiple semantically disentangled RGBA layers, enabling \textbf{inherent editability}, where each RGBA layer can be independently manipulated without affecting other content. To support variable-length decomposition, we introduce three key components: (1) an RGBA-VAE to unify the latent representations of RGB and RGBA images; (2) a VLD-MMDiT (Variable Layers Decomposition MMDiT) architecture capable of decomposing a variable number of image layers; and (3) a Multi-stage Training strategy to adapt a pretrained image generation model into a multilayer image decomposer. Furthermore, to address the scarcity of high-quality multilayer training images, we build a pipeline to extract and annotate multilayer images from Photoshop documents (PSD). Experiments demonstrate that our method significantly surpasses existing approaches in decomposition quality and establishes a new paradigm for consistent image editing.

See translation

🔥

1

1

+

Reply

![](/avatars/743a009681d5d554c27e04300db9f267.svg)

[avahal](/avahal)

[1 day ago](#694444f99973220907e49199)

arXiv lens breakdown of this paper 👉 <https://arxivlens.com/PaperView/Details/qwen-image-layered-towards-inherent-editability-via-layer-decomposition-9194-7a40c6da>

* Executive Summary
* Detailed Breakdown
* Practical Applications

See translation

🔥

1

1

+

Reply

![](/avatars/44f1b87f5555e0f75aea4b9f136b4839.svg)

[suixmeng](/suixmeng)

[about 21 hours ago](#6944c51c5f44ccf243c60540)

When will the code and model be released?

See translation

👍

2

2

+

Reply

![](https://cdn-avatars.huggingface.co/v1/production/uploads/6183d058b6128397263e0188/TEnKYn0mHwb39O7Ix-Esb.jpeg)

[jjmcarrascosa](/jjmcarrascosa)

[about 15 hours ago](#69450f3ffdaf94348a97a6b2)

In the paper they point to a repo that does not exist 😔 <https://github.com/QwenLM/QwenImage-Layered>

See translation

Reply

![](https://cdn-avatars.huggingface.co/v1/production/uploads/1678589663024-640d3eaa3623f6a56dde856d.jpeg)

[vansin](/vansin)

[about 7 hours ago](#6945889ce22aa0be5618eeb2)

[![db080ecbcb927d73ff9cc6276ab4e765](https://cdn-uploads.huggingface.co/production/uploads/640d3eaa3623f6a56dde856d/Z2kqHF1HsSOx25BvOUzS7.jpeg)](https://cdn-uploads.huggingface.co/production/uploads/640d3eaa3623f6a56dde856d/Z2kqHF1HsSOx25BvOUzS7.jpeg)

Reply

![](https://cdn-avatars.huggingface.co/v1/production/uploads/1678589663024-640d3eaa3623f6a56dde856d.jpeg)

[vansin](/vansin)

[about 7 hours ago](#694588b0e22aa0be5618f0b1)

how to use it in figma or photoshop

See translation

Reply

EditPreview

Upload images, audio, and videos by dragging in the text input, pasting, or clicking here.

Tap or paste here to upload images

Comment

·
[Sign up](/join?next=%2Fpapers%2F2512.15603) or
[log in](/login?next=%2Fpapers%2F2512.15603) to comment

[[ ]

Upvote

34](/login?next=%2Fpapers%2F2512.15603)

* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/6039478ab3ecf716b1a5fd4d/_Thy4E7taiSYBLKxEKJbT.jpeg)](/taesiri "taesiri")
* [![](/avatars/7978304e3fe99b0d4d0712441c6a24f3.svg)](/ghy0324 "ghy0324")
* [![](/avatars/d1ecd68111dbdb25168ebc7edfd02895.svg)](/OmniSVG "OmniSVG")
* [![](/avatars/640803ef641decd5c30894155bf09b6a.svg)](/UDCAI "UDCAI")
* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/620783f24e28382272337ba4/zkUveQPNiDfYjgGhuFErj.jpeg)](/Tommy930 "Tommy930")
* [![](/avatars/94cf959a86c0ce58263db710d693fb9e.svg)](/boomcheng "boomcheng")
* [![](/avatars/b05958b28a8964ee36bd8a3994b2aaec.svg)](/kkmh-op "kkmh-op")
* [![](/avatars/79df524de88db4bd50aaa5b00c92a0f4.svg)](/jide "jide")
* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/61e52be53d6dbb1da842316a/gx0WGPcOCClXPymoKglc4.jpeg)](/tellarin "tellarin")
* [![](/avatars/415129de698ff869ab048e392479ce91.svg)](/Mitano "Mitano")
* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/1673909278097-noauth.png)](/xziayro "xziayro")
* [![](https://cdn-avatars.huggingface.co/v1/production/uploads/noauth/Qz1WRFYfs6ZSnRz88wEZt.jpeg)](/denizaybey "denizaybey")
* +22

## Models citing this paper 2

[![](https://cdn-avatars.huggingface.co/v1/production/uploads/620760a26e3b7210c2ff1943/-s1gyJfvbE1RgO5iBeNOi.png)

#### Qwen/Qwen-Image-Layered

Image-Text-to-Image
•
Updated
about 15 hours ago
•

42
•

124](/Qwen/Qwen-Image-Layered)

[![](https://cdn-avatars.huggingface.co/v1/production/uploads/641caf6c043963b1c0a27256/-w4w2t9r-T3U2LqzF2jqe.png)

#### Runware/Qwen-Image-Layered

Image-Text-to-Image
•
Updated
about 4 hours ago](/Runware/Qwen-Image-Layered)

## Datasets citing this paper 0

No dataset linking this paper

Cite arxiv.org/abs/2512.15603 in a dataset README.md to link it from this page.

### Spaces citing this paper 1

[🚀

Qwen/Qwen-Image-Layered](/spaces/Qwen/Qwen-Image-Layered)

## Collections including this paper 2

[#### VisionLM

Collection

1853 items
•
Updated
1 day ago
•

137](/collections/zerozeyi/visionlm)

[#### Qwen-Image

Collection

10 items
•
Updated
about 9 hours ago
•

35](/collections/Qwen/qwen-image)

System theme

Company

[TOS](/terms-of-service)
[Privacy](/privacy)
[About](/huggingface)
[Careers](https://apply.workable.com/huggingface/)

Website

[Models](/models)
[Datasets](/datasets)
[Spaces](/spaces)
[Pricing](/pricing)
[Docs](/docs)

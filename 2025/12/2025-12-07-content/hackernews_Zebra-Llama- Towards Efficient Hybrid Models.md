---
source: hackernews
title: Zebra-Llama: Towards Efficient Hybrid Models
url: https://arxiv.org/abs/2505.17272
date: 2025-12-07
---

[Skip to main content](#content)

[![Cornell University](/static/browse/0.3.4/images/icons/cu/cornell-reduced-white-SMALL.svg)](https://www.cornell.edu/)

We gratefully acknowledge support from the Simons Foundation, [member institutions](https://info.arxiv.org/about/ourmembers.html), and all contributors.
[Donate](https://info.arxiv.org/about/donate.html)

[![arxiv logo](/static/browse/0.3.4/images/arxiv-logo-one-color-white.svg)](/) > [cs](/list/cs/recent) > arXiv:2505.17272

[Help](https://info.arxiv.org/help) | [Advanced Search](https://arxiv.org/search/advanced)

All fields
Title
Author
Abstract
Comments
Journal reference
ACM classification
MSC classification
Report number
arXiv identifier
DOI
ORCID
arXiv author ID
Help pages
Full text

Search

[![arXiv logo](/static/browse/0.3.4/images/arxiv-logomark-small-white.svg)](https://arxiv.org/)

[![Cornell University Logo](/static/browse/0.3.4/images/icons/cu/cornell-reduced-white-SMALL.svg)](https://www.cornell.edu/)

open searchopen navigation menu

## quick links

* [Login](https://arxiv.org/login)
* [Help Pages](https://info.arxiv.org/help)
* [About](https://info.arxiv.org/about)

# Computer Science > Machine Learning

**arXiv:2505.17272** (cs)

[Submitted on 22 May 2025]

# Title:Zebra-Llama: Towards Extremely Efficient Hybrid Models

Authors:[Mingyu Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang,+M), [Mehdi Rezagholizadeh](https://arxiv.org/search/cs?searchtype=author&query=Rezagholizadeh,+M), [Guihong Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+G), [Vikram Appia](https://arxiv.org/search/cs?searchtype=author&query=Appia,+V), [Emad Barsoum](https://arxiv.org/search/cs?searchtype=author&query=Barsoum,+E)

View a PDF of the paper titled Zebra-Llama: Towards Extremely Efficient Hybrid Models, by Mingyu Yang and 4 other authors

[View PDF](/pdf/2505.17272)
[HTML (experimental)](https://arxiv.org/html/2505.17272v1)
> Abstract:With the growing demand for deploying large language models (LLMs) across diverse applications, improving their inference efficiency is crucial for sustainable and democratized access. However, retraining LLMs to meet new user-specific requirements is prohibitively expensive and environmentally unsustainable. In this work, we propose a practical and scalable alternative: composing efficient hybrid language models from existing pre-trained models. Our approach, Zebra-Llama, introduces a family of 1B, 3B, and 8B hybrid models by combining State Space Models (SSMs) and Multi-head Latent Attention (MLA) layers, using a refined initialization and post-training pipeline to efficiently transfer knowledge from pre-trained Transformers. Zebra-Llama achieves Transformer-level accuracy with near-SSM efficiency using only 7-11B training tokens (compared to trillions of tokens required for pre-training) and an 8B teacher. Moreover, Zebra-Llama dramatically reduces KV cache size -down to 3.9%, 2%, and 2.73% of the original for the 1B, 3B, and 8B variants, respectively-while preserving 100%, 100%, and >97% of average zero-shot performance on LM Harness tasks. Compared to models like MambaInLLaMA, X-EcoMLA, Minitron, and Llamba, Zebra-Llama consistently delivers competitive or superior accuracy while using significantly fewer tokens, smaller teachers, and vastly reduced KV cache memory. Notably, Zebra-Llama-8B surpasses Minitron-8B in few-shot accuracy by 7% while using 8x fewer training tokens, over 12x smaller KV cache, and a smaller teacher (8B vs. 15B). It also achieves 2.6x-3.8x higher throughput (tokens/s) than MambaInLlama up to a 32k context length. We will release code and model checkpoints upon acceptance.

|  |  |
| --- | --- |
| Subjects: | Machine Learning (cs.LG); Computation and Language (cs.CL) |
| Cite as: | [arXiv:2505.17272](https://arxiv.org/abs/2505.17272) [cs.LG] |
|  | (or  [arXiv:2505.17272v1](https://arxiv.org/abs/2505.17272v1) [cs.LG] for this version) |
|  | <https://doi.org/10.48550/arXiv.2505.17272> Focus to learn more  arXiv-issued DOI via DataCite |

## Submission history

From: Mehdi Rezagholizadeh [[view email](/show-email/f03618b0/2505.17272)]
 **[v1]**
Thu, 22 May 2025 20:39:57 UTC (12,646 KB)

Full-text links:

## Access Paper:

View a PDF of the paper titled Zebra-Llama: Towards Extremely Efficient Hybrid Models, by Mingyu Yang and 4 other authors

* [View PDF](/pdf/2505.17272)
* [HTML (experimental)](https://arxiv.org/html/2505.17272v1)
* [TeX Source](/src/2505.17272)

[![license icon](https://arxiv.org/icons/licenses/by-nc-nd-4.0.png)
view license](http://creativecommons.org/licenses/by-nc-nd/4.0/ "Rights to this article")

Current browse context:

cs.LG

[< prev](/prevnext?id=2505.17272&function=prev&context=cs.LG "previous in cs.LG (accesskey p)")
  |
[next >](/prevnext?id=2505.17272&function=next&context=cs.LG "next in cs.LG (accesskey n)")

[new](/list/cs.LG/new)
 |
[recent](/list/cs.LG/recent)
 | [2025-05](/list/cs.LG/2025-05)

Change to browse by:

[cs](/abs/2505.17272?context=cs)
[cs.CL](/abs/2505.17272?context=cs.CL)

### References & Citations

* [NASA ADS](https://ui.adsabs.harvard.edu/abs/arXiv%3A2505.17272)
* [Google Scholar](https://scholar.google.com/scholar_lookup?arxiv_id=2505.17272)
* [Semantic Scholar](https://api.semanticscholar.org/arXiv%3A2505.17272)

export BibTeX citation
Loading...

## BibTeX formatted citation

×

loading...

Data provided by:

### Bookmark

[![BibSonomy logo](/static/browse/0.3.4/images/icons/social/bibsonomy.png)](http://www.bibsonomy.org/BibtexHandler?requTask=upload&url=https://arxiv.org/abs/2505.17272&description=Zebra-Llama: Towards Extremely Efficient Hybrid Models "Bookmark on BibSonomy")
[![Reddit logo](/static/browse/0.3.4/images/icons/social/reddit.png)](https://reddit.com/submit?url=https://arxiv.org/abs/2505.17272&title=Zebra-Llama: Towards Extremely Efficient Hybrid Models "Bookmark on Reddit")

Bibliographic Tools

# Bibliographic and Citation Tools

[ ]

Bibliographic Explorer Toggle

Bibliographic Explorer *([What is the Explorer?](https://info.arxiv.org/labs/showcase.html#arxiv-bibliographic-explorer))*

[ ]

Connected Papers Toggle

Connected Papers *([What is Connected Papers?](https://www.connectedpapers.com/about))*

[ ]

Litmaps Toggle

Litmaps *([What is Litmaps?](https://www.litmaps.co/))*

[ ]

scite.ai Toggle

scite Smart Citations *([What are Smart Citations?](https://www.scite.ai/))*

Code, Data, Media

# Code, Data and Media Associated with this Article

[ ]

alphaXiv Toggle

alphaXiv *([What is alphaXiv?](https://alphaxiv.org/))*

[ ]

Links to Code Toggle

CatalyzeX Code Finder for Papers *([What is CatalyzeX?](https://www.catalyzex.com))*

[ ]

DagsHub Toggle

DagsHub *([What is DagsHub?](https://dagshub.com/))*

[ ]

GotitPub Toggle

Gotit.pub *([What is GotitPub?](http://gotit.pub/faq))*

[ ]

Huggingface Toggle

Hugging Face *([What is Huggingface?](https://huggingface.co/huggingface))*

[ ]

Links to Code Toggle

Papers with Code *([What is Papers with Code?](https://paperswithcode.com/))*

[ ]

ScienceCast Toggle

ScienceCast *([What is ScienceCast?](https://sciencecast.org/welcome))*

Demos

# Demos

[ ]

Replicate Toggle

Replicate *([What is Replicate?](https://replicate.com/docs/arxiv/about))*

[ ]

Spaces Toggle

Hugging Face Spaces *([What is Spaces?](https://huggingface.co/docs/hub/spaces))*

[ ]

Spaces Toggle

TXYZ.AI *([What is TXYZ.AI?](https://txyz.ai))*

Related Papers

# Recommenders and Search Tools

[ ]

Link to Influence Flower

Influence Flower *([What are Influence Flowers?](https://influencemap.cmlab.dev/))*

[ ]

Core recommender toggle

CORE Recommender *([What is CORE?](https://core.ac.uk/services/recommender))*

[ ]

IArxiv recommender toggle

IArxiv Recommender
*([What is IArxiv?](https://iarxiv.org/about))*

* Author
* Venue
* Institution
* Topic

About arXivLabs

# arXivLabs: experimental projects with community collaborators

arXivLabs is a framework that allows collaborators to develop and share new arXiv features directly on our website.

Both individuals and organizations that work with arXivLabs have embraced and accepted our values of openness, community, excellence, and user data privacy. arXiv is committed to these values and only works with partners that adhere to them.

Have an idea for a project that will add value for arXiv's community? [**Learn more about arXivLabs**](https://info.arxiv.org/labs/index.html).

[Which authors of this paper are endorsers?](/auth/show-endorsers/2505.17272) |
Disable MathJax ([What is MathJax?](https://info.arxiv.org/help/mathjax.html))

* [About](https://info.arxiv.org/about)
* [Help](https://info.arxiv.org/help)

* contact arXivClick here to contact arXiv
   [Contact](https://info.arxiv.org/help/contact.html)
* subscribe to arXiv mailingsClick here to subscribe
   [Subscribe](https://info.arxiv.org/help/subscribe)

* [Copyright](https://info.arxiv.org/help/license/index.html)
* [Privacy Policy](https://info.arxiv.org/help/policies/privacy_policy.html)

* [Web Accessibility Assistance](https://info.arxiv.org/help/web_accessibility.html)
* [arXiv Operational Status](https://status.arxiv.org)

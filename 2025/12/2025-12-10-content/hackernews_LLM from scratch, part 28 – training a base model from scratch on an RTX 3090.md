---
source: hackernews
title: LLM from scratch, part 28 – training a base model from scratch on an RTX 3090
url: https://www.gilesthomas.com/2025/12/llm-from-scratch-28-training-a-base-model-from-scratch
date: 2025-12-10
---

[Giles' blog](/)

[![Me on X/Twitter](/images/x-icon.png "Me on X/Twitter")](https://x.com/gpjt)
[![Me on Bluesky](/images/bluesky-icon.png "Me on Bluesky")](https://bsky.app/profile/gilesthomas.com)
[![My GitHub profile](/images/github-icon.png "My GitHub profile")](https://github.com/gpjt)
[![My Hugging Face profile](/images/hf-icon.png "My Hugging Face profile")](https://huggingface.co/gpjt)
[![RSS feed for this blog](/images/rss-icon.png "RSS feed for this blog")](/feed/rss.xml)

[About](/about)

[Contact](/contact)

Archives

Categories

Blogroll

* [December 2025 (1)](/2025/12)
* [November 2025 (3)](/2025/11)
* [October 2025 (9)](/2025/10)
* [September 2025 (3)](/2025/09)
* [August 2025 (5)](/2025/08)
* [July 2025 (1)](/2025/07)
* [June 2025 (2)](/2025/06)
* [May 2025 (3)](/2025/05)
* [April 2025 (2)](/2025/04)
* [March 2025 (7)](/2025/03)
* [February 2025 (10)](/2025/02)
* [January 2025 (6)](/2025/01)
* [December 2024 (7)](/2024/12)
* [September 2024 (1)](/2024/09)
* [August 2024 (2)](/2024/08)
* [July 2024 (2)](/2024/07)
* [May 2024 (2)](/2024/05)
* [April 2024 (2)](/2024/04)
* [February 2024 (2)](/2024/02)
* [April 2023 (1)](/2023/04)
* [March 2023 (2)](/2023/03)
* [September 2022 (1)](/2022/09)
* [February 2022 (1)](/2022/02)
* [November 2021 (1)](/2021/11)
* [March 2021 (1)](/2021/03)
* [February 2021 (2)](/2021/02)
* [August 2019 (1)](/2019/08)
* [November 2018 (1)](/2018/11)
* [May 2017 (1)](/2017/05)
* [December 2016 (1)](/2016/12)
* [April 2016 (1)](/2016/04)
* [August 2015 (1)](/2015/08)
* [December 2014 (1)](/2014/12)
* [August 2014 (1)](/2014/08)
* [March 2014 (1)](/2014/03)
* [December 2013 (1)](/2013/12)
* [October 2013 (3)](/2013/10)
* [September 2013 (4)](/2013/09)
* [August 2013 (2)](/2013/08)
* [July 2013 (1)](/2013/07)
* [June 2013 (1)](/2013/06)
* [February 2013 (1)](/2013/02)
* [October 2012 (1)](/2012/10)
* [June 2012 (1)](/2012/06)
* [May 2012 (1)](/2012/05)
* [April 2012 (1)](/2012/04)
* [February 2012 (1)](/2012/02)
* [October 2011 (1)](/2011/10)
* [June 2011 (1)](/2011/06)
* [May 2011 (1)](/2011/05)
* [April 2011 (1)](/2011/04)
* [March 2011 (1)](/2011/03)
* [February 2011 (1)](/2011/02)
* [January 2011 (1)](/2011/01)
* [December 2010 (3)](/2010/12)
* [November 2010 (1)](/2010/11)
* [October 2010 (1)](/2010/10)
* [September 2010 (1)](/2010/09)
* [August 2010 (1)](/2010/08)
* [July 2010 (1)](/2010/07)
* [May 2010 (3)](/2010/05)
* [April 2010 (1)](/2010/04)
* [March 2010 (2)](/2010/03)
* [February 2010 (3)](/2010/02)
* [January 2010 (4)](/2010/01)
* [December 2009 (2)](/2009/12)
* [November 2009 (5)](/2009/11)
* [October 2009 (2)](/2009/10)
* [September 2009 (2)](/2009/09)
* [August 2009 (3)](/2009/08)
* [July 2009 (1)](/2009/07)
* [May 2009 (1)](/2009/05)
* [April 2009 (1)](/2009/04)
* [March 2009 (5)](/2009/03)
* [February 2009 (5)](/2009/02)
* [January 2009 (5)](/2009/01)
* [December 2008 (3)](/2008/12)
* [November 2008 (7)](/2008/11)
* [October 2008 (4)](/2008/10)
* [September 2008 (2)](/2008/09)
* [August 2008 (1)](/2008/08)
* [July 2008 (1)](/2008/07)
* [June 2008 (1)](/2008/06)
* [May 2008 (1)](/2008/05)
* [April 2008 (1)](/2008/04)
* [January 2008 (4)](/2008/01)
* [December 2007 (3)](/2007/12)
* [March 2007 (3)](/2007/03)
* [February 2007 (1)](/2007/02)
* [January 2007 (2)](/2007/01)
* [December 2006 (4)](/2006/12)
* [November 2006 (18)](/2006/11)

* [AI (62)](/ai)
* [TIL deep dives (57)](/til-deep-dives)
* [Python (56)](/python)
* [Resolver One (34)](/resolver-one)
* [LLM from scratch (29)](/llm-from-scratch)
* [Blogkeeping (18)](/blogkeeping)
* [PythonAnywhere (17)](/pythonanywhere)
* [Linux (16)](/linux)
* [Startups (15)](/startups)
* [NSLU2 offsite backup project (13)](/nslu2-offsite-backup-project)
* [TIL (13)](/til)
* [Funny (11)](/funny)
* [Finance (10)](/finance)
* [Fine-tuning LLMs (10)](/fine-tuning)
* [Musings (10)](/musings)
* [C (9)](/c)
* [Gadgets (8)](/gadgets)
* [Personal (8)](/personal)
* [Robotics (8)](/robotics)
* [Website design (8)](/website-design)
* [3D (5)](/3d)
* [Rants (5)](/rants)
* [Cryptography (4)](/cryptography)
* [JavaScript (4)](/javascript)
* [Music (4)](/music)
* [Oddities (4)](/oddities)
* [Quick links (4)](/quick-links)
* [Talks (4)](/talks)
* [Dirigible (3)](/dirigible)
* [Eee (3)](/eee)
* [Memes (3)](/memes)
* [Politics (3)](/politics)
* [Django (2)](/django)
* [GPU Computing (2)](/gpu-computing)
* [LaTeX (2)](/latex)
* [MathML (2)](/mathml)
* [OLPC XO (2)](/olpc-xo)
* [Retro Language Models (2)](/retro-language-models)
* [Space (2)](/space)
* [VoIP (2)](/voip)
* [Copyright (1)](/copyright)
* [Golang (1)](/golang)
* [Raspberry Pi (1)](/raspberry-pi)
* [Software development tools (1)](/software-dev-tools)

* [Agile Abstractions](https://agileabstractions.com/)
* [Astral Codex Ten](https://www.astralcodexten.com/)
* [:: (Bloggable a) => a -> IO ()](https://blog.omega-prime.co.uk/)
* [David Friedman's Substack](https://daviddfriedman.substack.com/)
* [Econ & Energy](https://robertsmithson1.substack.com/)
* [Entrepreneurial Geekiness](https://ianozsvald.com/)
* [For some value of "Magic"](https://holdenweb.blogspot.com/)
* [Hackaday](https://hackaday.com/)
* [kaleidic.ai newsletter](https://kaleidic.substack.com/)
* [Knowing.NET](https://knowing.net/)
* [Language Log](https://languagelog.ldc.upenn.edu/nll/)
* [Millennium Hand](http://blog.millenniumhand.co.uk/)
* [ntoll.org](https://ntoll.org/)
* [Obey the Testing Goat!](https://www.obeythetestinggoat.com/)
* [PK](https://pkaznowski.gitlab.io/projects/)
* [PythonAnywhere News](https://blog.pythonanywhere.com/)
* [Simon Willison's Weblog](https://simonwillison.net/)
* [Societive](https://medium.com/%40societive)
* [Software Deviser](https://orestis.gr/)
* [Some opinions, held with varying degrees of certainty](https://filip.lajszczak.dev/)
* [tartley.com](https://www.tartley.com/)

## Writing an LLM from scratch, part 28 -- training a base model from scratch on an RTX 3090

Posted on 2 [December 2025](/2025/12/)
in
[AI](/ai),
[LLM from scratch](/llm-from-scratch),
[TIL deep dives](/til-deep-dives),
[Python](/python)

Having worked through the main body of [Sebastian Raschka](https://sebastianraschka.com/)'s book
"[Build a Large Language Model (from Scratch)](https://www.manning.com/books/build-a-large-language-model-from-scratch)",
I wanted to try an experiment: is it possible to train a base model of my
own, on my own hardware?

The book shows you how to train your LLM, does a basic training run
on a small dataset, and then we switch to downloading the "pre-cooked" weights
from OpenAI. That makes sense given that not every reader will have access to enough
hardware to really train from scratch. And right back at
[the start of this series](/2024/12/llm-from-scratch-1), I did some naive scaling of
numbers I'd got when fine-tuning LLMs and came to the conclusion that it would be
impossible in a reasonable time.

But the speed I got with my RTX 3090 on the book's small training run made me
think that perhaps --
just perhaps! -- it might actually be possible to train a model of this size -- about
163M parameters -- on my own hardware. Not, perhaps, on a small laptop, but at least on
a reasonably high-end "gaming" PC.

Additionally, Andrej Karpathy recently announced [nanochat](https://github.com/karpathy/nanochat),
"the best ChatGPT that $100 can buy". He mentions on the main page that he's trained
a model called `d32`, with 32 Transformer layers, which has 1.9B parameters, for about $800.
His smaller 20-layer `d20` model, with 561M parameters, he says should be trainable
in about four hours on an 8x H100 GPU node, which costs about $24/hour -- hence the
$100 total price.

What's even more interesting about nanochat is that it's built with PyTorch; initially
I'd got the impression that it was based on his pure C/CUDA [`llm.c`](https://github.com/karpathy/llm.c),
which I would imagine would give a huge speedup. But no -- he's using the same stack
as I have been in this series!

Karpathy's models are both larger than 163M parameters, so it definitely sounded like this might be doable. Obviously, I'm nowhere near as experienced an AI developer,
and he's using a larger machine (8 GPUs and each of them has > 3x more VRAM than mine),
but he's also including the time to train a tokeniser and instruction fine-tune
into that four hours -- and his smaller model is more than three times larger than mine. So that should all
help.

This post is a little less structured than the others in my LLM from scratch series,
as it's essentially a tidied version of the notes I kept as I worked through the
project.

But so as not to bury the lede: using the Hugging Face FineWeb-series datasets,
I was able to train a GPT-2 small sized
base model to a level where it was almost as good as the original in just over 48
hours on my own hardware! Base models: not just for the big AI labs.

Here's the full story.

### The model

For this project, I want to use the exact same model code as Raschka presented in the
LLM from scratch book -- [my copy here](https://github.com/gpjt/llm-from-scratch/blob/main/gpt.py).
There have been [a number of architectural improvements](https://magazine.sebastianraschka.com/p/from-gpt-2-to-gpt-oss-analyzing-the)
to LLMs since GPT-2, but for now it's best to keep things simple.

But there are still some settings to decide on. The config dictionary for the
models we've been using has these parameters:

* `vocab_size`. This is determined by the tokenizer, and I want to use the GPT-2 one, so
  it will need to be `50257`.
* `context_length`. GPT-2 has a 1,024-token context length, so I'll stick with that.
* `emb_dim`, `n_heads`, `n_layers` --- these define which of the different GPT-2 model
  classes we're training, and I want to stick to the smallest `gpt2-small` one, so
  they will be `768`, `12` and `12` respectively
* `drop_rate`. One of the most surprising things to me in the "architectural improvements" post
  linked above was that dropout is no longer used so much. However, this appears to be
  tied in to the one-epoch training that has taken off since GPT-2, so I think it
  would be best to stick to `0.1` here.
* `qkv_bias`. From what Raschka says in the book, this doesn't add on much value, even though
  the original GPT-2 used it, so let's set it to `False`.

There's also the aspect of weight-tying -- the original GPT-2 reused its embedding
matrix as the weights for the linear layer that [projects the context vectors from
the last Transformers layer into vocab space to get the logits](/2025/05/llm-from-scratch-15-from-context-vectors-to-logits).

There's nothing in the code we've been working with to enforce that, though -- when
we do our small train in the book, we're using independent weights for each of those
steps. The only time it is "enforced" is when we download the pretrained weights
from OpenAI, where we put the same values into both the embedding matrix and the final
output head.

Given that Raschka says that it's in general better to avoid weight-tying, and actually doing
it would be harder than not doing it, then it seems a no-brainer to not do it.

So, what does that mean about our model?

```
In [1]: big_train_params = {
   ...:     "vocab_size": 50257,
   ...:     "context_length": 1024,
   ...:     "emb_dim": 768,
   ...:     "n_heads": 12,
   ...:     "n_layers": 12,
   ...:     "drop_rate": 0.1,
   ...:     "qkv_bias": False
   ...: }

In [2]: from gpt import GPTModel

In [3]: model = GPTModel(big_train_params)

In [4]: sum(p.numel() for p in model.parameters())
Out[4]: 163009536
```

That matches what we got when working through the book; 163M parameters. Can we train it?

### The data

It seems like every AI project starts with the question "what data can we use?"

The original report on GPT-2,
"[Language Models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)",
is frustratingly lacking in details. However, it does say that they trained it on
"8 million documents for a total of 40 GB of text". Now, [according to OpenAI](https://platform.openai.com/tokenizer),
it's reasonable to assume roughly four characters per token for typical English
text. So 40 GB of text is ~10 billion tokens. That data was essentially gathered
by scraping pages linked from Reddit that had more than three upvotes there, so was
reasonably high quality. Can we get something similar?

Conveniently, Hugging Face host a big dataset called [FineWeb](https://huggingface.co/datasets/HuggingFaceFW/fineweb),
and that has a 10 billion token "sample" dataset, randomly selected from the full
18.5 *trillion* tokens. So the sample feels like it's order-of-magnitude right. And
while reading more about Karpathy's nanochat, I spotted that it uses [FineWeb-Edu](https://huggingface.co/datasets/HuggingFaceFW/fineweb-edu),
which is a version of FineWeb that contains "only the most educational web pages".

I wrote [a script to download both of those](https://github.com/gpjt/llm-from-scratch/blob/main/download-fineweb-10b.py),
and kicked it off. It took about 20 minutes
for each one (slow wifi in my study, I was getting < 5MB/s); FineWeb's 10B sample took
up about 29 GiB, and FineWeb-Edu's about 27 GiB.

Time to take a look at them. The Hugging Face [`datasets`](https://pypi.org/project/datasets/) `load_dataset` function loads up all of the files
you provide, and you can tell it how to split them up into train/validation/test sets.
This command just loads up the whole FineWeb one and says "treat it all as the train split",
which is good enough for now:

```
In [1]: from datasets import load_dataset

In [2]: fw = load_dataset(
   ...:     "parquet",
   ...:     data_files="./fineweb/sample/10BT/*.parquet",
   ...:     split="train"
   ...: )
Generating train split: 14868862 examples [01:53, 130852.34 examples/s]
Loading dataset shards: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:03<00:00, 31.90it/s]
```

Yikes. It took 1 minute, 53 seconds to generate the train split. However, that appears
to be a one-off cost -- when I accessed it again later using the same code in a different
Python session, it just did the second "Loading dataset shards" portion, taking three seconds,
not the generation of the split. Presumably it caches it.

Anyway, let's see what's in it:

```
In [3]: print(fw)
Dataset({
    features: ['text', 'id', 'dump', 'url', 'date', 'file_path', 'language', 'language_score', 'token_count'],
    num_rows: 14868862
})
```

Great, so we have 14,868,862 rows, each of which has various bits of information. Checking the first one's text:

```
In [7]: print(fw[0]["text"][:500])
|Viewing Single Post From: Spoilers for the Week of February 11th|
|Lil||Feb 1 2013, 09:58 AM|
Don't care about Chloe/Taniel/Jen-Jen. Don't care about Sami, really, but hoping
that we get some good "SAMANTHA GENE!!" Marlena Death-Stares out of it. And
"newfound" feelings. Please. If only.
STEFANO!! STEFANO, STEFANO, STEFANO!!!! :cheer:
|Spoilers for the Week of February 11th · DAYS: News, Spoilers & Discussion|
```

Well, for FineWeb, that doesn't look particularly "fine", but I guess it's better than the
stuff that Karpathy talked about in
[his recent interview with Dwarkesh Patel](https://www.dwarkesh.com/p/andrej-karpathy):

> When you’re looking at a pre-training dataset in the frontier lab and you
> look at a random internet document, it’s total garbage. I don't even know how
> this works at all. It’s [stuff] like stock tickers, symbols, it's a huge amount
> of slop and garbage from like all the corners of the internet

Let's take a look at FineWeb-Edu.

```
In [8]: fw_edu = load_dataset(
   ...:     "parquet",
   ...:     data_files="./fineweb-edu/sample/10BT/*.parquet",
   ...:     split="train"
   ...: )
Generating train split: 9672101 examples [01:32, 104057.34 examples/s]
Loading dataset shards: 100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████| 98/98 [00:02<00:00, 48.62it/s]

In [9]: print(fw_edu[0]["text"][:500])
The Independent Jane
For all the love, romance and scandal in Jane Austen’s books, what they are
really about is freedom and independence. Independence of thought and the
freedom to choose.
Elizabeth’s refusal of Mr. Collins offer of marriage showed an independence
seldom seen in heroines of the day. Her refusal of Mr. Darcy while triggered by
anger showed a level of independence that left him shocked and stunned.
The freedom she exhibited in finally accepting him in direct defiance of Lady Cath
```

That looks a lot better!

Now let's take a look at the document lengths in terms of tokens. There's a
`token_count` column, but I don't know which tokeniser that's for, so to be safe we'll
calculate it ourselves.

How long would
it take to tokenise every row in FineWeb 10B to check? Let's tokenise the first
10,000 of the 14,868,862 that we have, and see how long that would take -- then we
can work out the estimated time for the whole thing.

```
In [25]: import tiktoken

In [26]: import time

In [27]: tokenizer = tiktoken.get_encoding("gpt2")

In [28]: start = time.time()
    ...: for entry in fw.select(range(10_000)):
    ...:     tokenizer.encode(entry["text"])
    ...: end = time.time()

In [29]: end - start
Out[29]: 1.4528205394744873

In [30]: fw
Out[30]:
Dataset({
    features: ['text', 'id', 'dump', 'url', 'date', 'file_path', 'language', 'language_score', 'token_count'],
    num_rows: 14868862
})

In [31]: (14868862 / 10_000) * 1.4528205394744873
Out[31]: 2160.1788112211702
```

2,160 seconds or about 36 minutes. Yikes!

After a bit of digging, though, I found that `tiktoken` tokenisers can handle batches
(poorly documented, but it's there [in the source](https://github.com/openai/tiktoken/blob/97e49cb/tiktoken/core.py#L175#L175)):

```
In [45]: text_batch = ["a", "b", "c"]

In [46]: tokenizer.encode_batch(text_batch)
Out[46]: [[64], [65], [66]]
```

Also, we can map a function over an entire HF dataset, and that can be made to run
with multiple processes. So, we can combine the two:

```
In [47]: import os

In [53]: def add_len(examples):
    ...:     texts = [t or "" for t in examples["text"]]
    ...:     tokens = tokenizer.encode_batch(texts, disallowed_special=())
    ...:     return {"tok_len": [len(t) for t in tokens]}
    ...:

In [54]: start = time.time()
    ...: fw_with_len = fw.map(
    ...:     add_len,
    ...:     batched=True,
    ...:     batch_size=1024,
    ...:     num_proc=os.cpu_count(),
    ...: )
    ...: end = time.time()
Map (num_proc=24): 100%|████████████████████████████████████████████████████████████████████████████████████████████| 14868862/14868862 [03:15<00:00, 75869.33 examples/s]
```

Just over three minutes, not too bad! (The reason the command count
above jumps from 47 to 53 was that in the first run I didn't have the
`disallowed_special=()` in there -- one of the rows in the dataset had `<|endoftext|>` in
it, and the tokenizer rejected it. I'm going to play fast and loose and ignore that for now.)

Now let's see how it added it:

```
In [56]: fw_with_len[0].keys()
Out[56]: dict_keys(['text', 'id', 'dump', 'url', 'date', 'file_path', 'language', 'language_score', 'token_count', 'tok_len'])

In [57]: fw_with_len[0]["tok_len"]
Out[57]: 142

In [58]: len(fw_with_len["tok_len"])
Out[58]: 14868862

In [59]: fw_with_len["tok_len"][0]
Out[59]: 142
```

Cool! We've added a `tok_len` column with the number of GPT-2 tokens for each row, and we
can extract what amounts to a list of those values. Let's plot them as a histogram.

Trying to do it directly -- that is, just doing

```
ax.hist(fw_with_len["tok_len"], bins=bins)
```

...seems to make MatPlotLib very unhappy, and my interpreter crashed with an OOM -- I think it might be trying to load all
of the dataset -- text, IDs, etc -- into RAM in one go.

So I started a fresh one and did the stuff to load it and annotate it with token lengths
again -- weirdly, this time the mapping only took 10 seconds or so! That was strange,
I'll need to look into that. Perhaps the earlier command added the `tok_len` column to the files on
disk?

To work around the memory issue, I converted the `tok_len` column from the dataset to an actual list:

```
In [11]: lengths = [n for n in fw_with_len["tok_len"]]
```

That took ten or twenty seconds. Let's then try the plot again (full code this time):

```
In [19]: import numpy as np
    ...: import matplotlib.pyplot as plt
    ...:
    ...: bins = np.arange(0, 2048 + 16, 16)
    ...:
    ...: plt.xkcd()
    ...: plt.rcParams['font.family'] = "xkcd"
    ...: fig = plt.figure(figsize=(10, 6))
    ...: ax = plt.gca()
    ...:
    ...: ax.hist(lengths, bins=bins)
    ...: ax.set_xlabel("TOKENIZED LENGTH (GPT-2 TOKENS)")
    ...: ax.set_ylabel("COUNT")
    ...: ax.set_title("FINEWEB DISTRIBUTION OF TOKENIZED LENGTHS")
    ...:
    ...: mean_len = float(np.mean(lengths))
    ...: median_len = float(np.median(lengths))
    ...: h_mean = ax.axvline(mean_len, linestyle="--", label=f"MEAN = {mean_len:.1f}")
    ...: h_med  = ax.axvline(median_len, linestyle=":",  label=f"MEDIAN = {median_len:.1f}")
    ...: ax.legend(handles=[h_mean, h_med])
    ...:
    ...: ax.grid(True, axis="y", alpha=0.3)
    ...: plt.tight_layout()
    ...: plt.savefig("fineweb-token-length-distribution.png")
```

That took about 11s to run, and the result is this:

![Histogram of GPT-2 token count across FineWeb samples](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/fineweb-token-length-distribution.png "Histogram of GPT-2 token count across FineWeb samples")

That's really promising! The bulk of them are less than our 1,024 token sequence length. [1](#fn-1)
If we present each row in the dataset as a stand-alone training sample, cropping them
when necessary, perhaps we won't lose too much data? Let's see.

First step, how many tokens are there in total?

```
In [20]: sum(lengths)
Out[20]: 10336315397
```

Nice, about 10B, as expected. How many tokens would we have if we cropped them to the default GPT-2 context length
of 1,024?

```
In [21]: sum(l if l < 1024 else 1024 for l in lengths)
Out[21]: 7354541756
```

Ouch, 7.3B. That's quite a reduction:

```
In [22]: 7354541756 / 10336315397
Out[22]: 0.7115245107685639
```

So we're losing 29% of our tokens by that cropping. That's from curtailing just
16% of the sequences:

```
In [26]: len([l for l in lengths if l > 1024])
Out[26]: 2438899

In [27]: len(lengths)
Out[27]: 14868862

In [28]: 2438899 / 14868862
Out[28]: 0.1640272806351959
```

That's not great.

I feel that we have two options here:

1. Crop all of the input sequences -- that is, each row in the dataset -- so that
   each one is no more than our 1,024 sequence length. Then we can pad them out
   with end-of-sequence tokens (as is the standard) so that they're all 1,024. This
   will lose us quite a lot of tokens, but has the big benefit of being easy.
2. Treat the corpus as, essentially, one long document, with end-of-sequence delimiters
   between each row, then split that up into 1,024-token sequences.
   Doing it this way would mean we'd
   use all of our training data. But it would be more complicated, especially
   if we hit memory constraints.

At this point in the experiment, I'm going to keep both options open. I'm inclined
towards the latter (I believe it's closer to what the real GPT-2 train did), but
I'm not sure.

Anyway, we're scoping things out here, so let's move on.

### Epochs

After looking at the data, I've thought a bit more about this. I'd previously been thinking
in terms of training across all of the tokens in the dataset; we'd work our way through the 10B
tokens, and then we'd be done.

But when training a model, you do multiple epochs, normally -- you run through the
dataset once, updating your gradients as you go, then run through it again likewise,
and eventually you stop when your validation loss starts rising.

I think that because I'd read that LLMs are normally trained on just one epoch
these days, I'd kind of internalised that we only need to do one. But it wasn't the
case in 2019 when GPT-2
came out. They had less data -- just 10B tokens or so, compared to insanely huge
datasets like the full FineWeb (not the 10B one we've been looking at -- the 18.5T full one), so they
would have trained it for some number of epochs.

How many? That's another case where the GPT-2 paper is annoyingly light.
[This report](https://wandb.ai/bkkaggle/lm-finetuning/reports/Pretraining-a-124-M-Parameter-GPT-2-Language-Model--VmlldzoyMjg4NzA)
says in the "Replicating GPT-2" section that OpenAI trained it for 800k iterations with a batch size of 512. Plugging
in a sequence length of 1024, that gives us this many tokens:

800,000×512×1,024=419,430,400,000

Over 419B tokens!

Now, if we believe that their dataset was 10B tokens, then we can work out how many epochs
that came to:

419,430,400,000/10,000,000,000=41.94

The same report says that they -- as in, the report authors -- make that "around a total of 60 epochs through the training set" --
I believe that the training set they're talking about could well be slightly shorter than
the original GPT-2 one -- the GPT-2 authors didn't release their own, which is called "WebText", so the report's
author is using a different one that tries to replicate it, [OpenWebText](https://skylion007.github.io/OpenWebTextCorpus/).

That sounds expensive; even without knowing how many tokens per second we can train
for, 40-odd epochs of 10B tokens each sounds like it would take a long time. Are there
any other comparison points that might tell us how long to train for?

Well, there's a "Chinchilla heuristic" that I've heard of, which says that you should train on about 20 tokens
per model parameter. I spent some time reading into where that comes from; originally
it's in "[Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556)"
from Google DeepMind, and it's an interesting paper, and is surprisingly easy to read,
with a few bits of maths that get a bit hairy (but aren't required to get a good-enough
feel for what they're saying). I recommend you take a look.

It was written in 2022, and the authors felt that people were scaling up models
a lot, but weren't increasing the number of tokens that they used for training enough.
So, they trained a huge number of models, trying to answer the question: "given a
particular budget in training FLOPs, what is the optimal balance of training tokens
versus parameters to make sure you're using those FLOPs most efficiently?". They
were arguing against the method taken in a particular paper, where another team had trained a model (called Gopher)
on significantly fewer tokens than they thought optimal.

The number of FLOPs used to train a model is linear with both the number of parameters
and the number of tokens you train it on, so if you get 2x the number of FLOPs that
you had before, you can either train the same model on twice as many tokens, or
you can double its size. Which is better? Their conclusion was that you should
actually scale both parameters and tokens up by the same amount -- that is, in the 2x
case you'd want to have 2 times both the parameters and tokens, which
would double your FLOPs and get you better performance.

As you can probably see, by doing this they indirectly
worked out an optimal number of tokens to train a particular size of model for.
They don't state the "20x" heuristic themselves, but it's pretty clear in table 3
in the paper, where they give a number of model sizes and the optimal number of tokens
for each.

Now, this number is not the number of tokens you need to train for to get the *best*
model you can for a particular number of parameters; a model of a given size
can always be trained more and will (hopefully) get better. But it tells you when you've
trained on enough tokens that you could get better results by training a larger model
than you have right now.

They're implicitly assuming
that models can get as large as you want, which of course is not the case -- in reality,
you're going to be targeting a particular model size, the size that can fit on your
training hardware (or more likely with production models, the size that can fit on
your planned inference hardware).

But interestingly, looking at the [README.md for Karpathy's nanochat](https://github.com/karpathy/nanochat/blob/master/README.md)
project, he trained his 1.9B "d32" model on 38B tokens -- exactly 20x. And
if you look at the [`speedrun.sh`](https://github.com/karpathy/nanochat/blob/master/speedrun.sh)
script in the same repo, he explicitly says that he's training for 20x parameters
for the smaller `d20` model:

```
# The d20 model is 561M parameters.
# Chinchilla says #tokens = 20X #params, so we need 561e6 * 20 = 11.2B tokens.
```

If Andrej Karpathy thinks that training for Chinchilla-optimality is the right way
to go, then who am I to disagree? ;-)

More seriously, perhaps the better quality of the dataset makes this a reasonable
thing to do. From the GPT-2 paper, their description of how they got the data:

> ...we created a new web scrape which emphasizes
> document quality. To do this we only scraped web pages
> which have been curated/filtered by humans. Manually
> filtering a full web scrape would be exceptionally expensive
> so as a starting point, we scraped all outbound links from
> Reddit, a social media platform, which received at least 3
> karma. This can be thought of as a heuristic indicator for
> whether other users found the link interesting, educational,
> or just funny.

That's a clever trick, but I believe that FineWeb is much more carefully filtered and improved
than the WebText dataset they got from that. Back in 2019, they had to do everything from scratch -- find appropriate
ways to get data, filter it, and so on. Now we can just download stuff from Hugging Face.
So maybe Chinchilla-optimal is enough.

Anyway, we have 163,009,536 parameters, so on that basis, let's train for:

163,009,536×20=3,260,190,720

...tokens. (I'll just use 3.2B from now on, but that's the actual number I mean.)

That's pretty cool! We have more than that number of tokens already in our
FineWeb 10B sample, so we can do a single-epoch training run.

So the question is -- is that even doable on my hardware?

### Tokens per second

It all hinges on how many tokens per second we can train at. A good way to check this is to write a throwaway "trainer". We can use that to
work out what our maximum batch size on the RTX 3090's 24 GiB of VRAM, then run a bunch
of batches through -- a forward and backward pass for each -- and see how many
we get.

This won't estimate how much time we'll spend validating the model, of course. But
my gut is telling me that we should spend no more than 5% of our training time running
validations, so we can later on do a similar test, eval mode, forward pass only with no gradient
tracking, and use that to work out how many tokens should be in the validation set.

So, let's estimate training speed. [This code](https://github.com/gpjt/llm-from-scratch/blob/36196755f850adeba348d15e2f4f81e87ad4d14f/measure-tokens-per-second.py)
gets an estimate of tokens/second at different batch sizes.
Hopefully it's clear enough to not need an in-depth explanation. An outline:

* We load enough GPT-2 tokens from FineWeb for `NUM_BATCHES` batches of `MAX_BATCH_SIZE` sequences each,
  every one of those sequences being `SEQ_LENGTH` long (plus one extra token for the targets we're
  comparing them to). Note that we're not bothering to separate them with anything
  for this test.
* We then loop over batch sizes from `1` to `MAX_BATCH_SIZE`.
* Then we create our model and put it on the CUDA device. We do this for each
  batch size rather than creating one and then using it for all of them so that they're all
  starting from the same point -- the `torch.manual_seed` should make sure that they're
  identical.
* For each batch size, we create input and output batches as tensors -- note that
  we're not putting these on CUDA yet, I wanted to do that in the training loop to
  mirror what a real training loop will have to do. When we're training with
  3.2B tokens then having them all on CUDA will be a waste of VRAM, so we'll be
  pushing a batch there for each iteration.
* We do a stripped-down training loop -- for each batch, put the inputs and outputs
  onto CUDA, then a forward pass, work out the loss, backward pass, and optimiser
  step. We do the same `NUM_BATCHES` iterations per batch size.
* Finally, we print out the number of tokens we trained on for this batch size, how long it took, and the
  number of tokens per second.

Here's what it prints out:

```
Loading dataset shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:00<00:00, 362.71it/s]
Testing with batch size 1
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:10<00:00,  9.77it/s]
Done, trained on 102,400 tokens in 10.2348s.
Tokens per second: 10,005

Testing with batch size 2
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:17<00:00,  5.60it/s]
Done, trained on 204,800 tokens in 17.8631s.
Tokens per second: 11,464

Testing with batch size 3
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:25<00:00,  3.93it/s]
Done, trained on 307,200 tokens in 25.4152s.
Tokens per second: 12,087

Testing with batch size 4
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:33<00:00,  3.02it/s]
Done, trained on 409,600 tokens in 33.1185s.
Tokens per second: 12,367

Testing with batch size 5
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:40<00:00,  2.46it/s]
Done, trained on 512,000 tokens in 40.6351s.
Tokens per second: 12,599

Testing with batch size 6
  0%|                                                                                                                                             | 0/100 [00:00<?, ?it/s]
Traceback (most recent call last):
  File "/home/giles/Dev/llm-from-scratch/measure-tokens-per-second.py", line 89, in <module>
    main()
    ~~~~^^
...
torch.OutOfMemoryError: CUDA out of memory. Tried to allocate 1.15 GiB. GPU 0 has a total capacity of 23.56 GiB of which 269.19 MiB is free. Including non-PyTorch memory, this process has 20.99 GiB memory in use. Of the allocated memory 18.67 GiB is allocated by PyTorch, and 2.02 GiB is reserved by PyTorch but unallocated. If reserved but unallocated memory is large try setting PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True to avoid fragmentation.  See documentation for Memory Management  (https://pytorch.org/docs/stable/notes/cuda.html#environment-variables)
```

So we can see that it gets faster as we increase the batch size, which makes sense
because we're handling sequences in parallel, but it does flatten off a bit, which
makes sense because there's a limit to how much parallelism we can do, even on a GPU.

Let's see how that fits in with the different training sizes we looked at above:

* Chinchilla heuristic, 20x parameters -- 3.2B tokens: 247,850 seconds, which is just less than three days
* Estimated GPT-2 train, 419B tokens: 32,452,947 seconds, which is just over a year.

OK. We're definitely not going to be able to train this thing the GPT-2 way! I
expected that to be the case, but now we have a solid proof of that.

But the three-day Chinchilla-optimal train actually sounds doable! I'm heading to London
to visit family soon, so won't be using my home PC. With a bit of help from
[Tailscale](https://tailscale.com/) I'll be able to log into it from my laptop, though,
so I can potentially nurse a run through.

Can we make it any faster?

Now, when doing the fine-tuning work, I found that you could generally speed things
up by doing everything in 16-bit rather than 32-bit. Intuitively that makes sense --
lower-precision numbers, fewer bits, means less work for the GPU doing the various
multiplications and additions that are involved in our train.

Working with ChatGPT, I found a couple of ways to take advantage of that. Firstly,
using TF32.

The normal float32 format uses 8 bits for the exponent, and 23 for the mantissa. If
you haven't looked into how floats are represented in memory (or if you've forgotten),
that means that, using m to mean the mantissa and x the exponent, the numbers are represented
in memory as

m×2x

TF32 is messier; it has the same exponent size -- and thus the same range -- as float32, but it essentially ignores
the lower 13 bits of the mantissa. So it takes up the same amount of memory, but is lower-precision,
which means that calculations can be faster. Most importantly, cards like the RTX 3090
have dedicated "tensor cores" -- as opposed to the normal CUDA cores that do normal
matrix multiplications -- and they operate in TF32. Unsurprisingly, "TF32" is
"tensor float 32-bit".

The PyTorch [`set_float32_matmul_precision`](https://docs.pytorch.org/docs/stable/generated/torch.set_float32_matmul_precision.html)
allows you to tell it what precision to use for matrix multiplications; the default is
`"highest"`, which means "use float32 all of the time", so you're stuck using just the
CUDA cores. If, instead, you set it to
`"high"`, then it will use TF32 if the hardware supports it and it has the appropriate
kernels available. So that will let us use the tensor cores.

I added this to the code above just above the loop over the different batch sizes:

```
torch.set_float32_matmul_precision("high")
```

Let it run, and:

```
Testing with batch size 1
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:08<00:00, 11.66it/s]
Done, trained on 102,400 tokens in 8.5799s.
Tokens per second: 11,934

Testing with batch size 2
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:15<00:00,  6.65it/s]
Done, trained on 204,800 tokens in 15.0287s.
Tokens per second: 13,627

Testing with batch size 3
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:20<00:00,  4.85it/s]
Done, trained on 307,200 tokens in 20.6374s.
Tokens per second: 14,885

Testing with batch size 4
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:27<00:00,  3.61it/s]
Done, trained on 409,600 tokens in 27.7148s.
Tokens per second: 14,779

Testing with batch size 5
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:33<00:00,  3.01it/s]
Done, trained on 512,000 tokens in 33.2420s.
Tokens per second: 15,402
```

That's a 22% speedup! Of course, the precision of the training isn't as good. But
given that many modern models are trained at 16-bit (I've seen suggestions that
some are even trained as low as 4-bit) then that shouldn't matter.

Let's see whether we can train in 16-bit instead. PyTorch has a smart mode where
you can tell it "use 16-bit where it makes sense, otherwise use 32-bit" -- AMP, which
stands for "Automatic Mixed Precision". [There's a great recipe for how to use it in the docs](https://docs.pytorch.org/tutorials/recipes/recipes/amp_recipe.html),
so let's use that. We need to create a `Scaler` object to handle scaling parameters
from 16-bit to 32-bit as needed -- we can re-use that across all batch sizes
so we can create it just before the loop:

```
    scaler = torch.amp.GradScaler()
```

...then we need to replace this core part of our training loop:

```
            logits = model(inputs)
            loss = torch.nn.functional.cross_entropy(
                logits.flatten(0, 1), outputs.flatten()
            )
            loss.backward()
            optimizer.step()
```

...with some code to use AMP and that scaler -- basically we use a context manager
to switch it on when we're doing the forward pass and work out the loss, and then use the scaler
to manage the backward pass and the optimiser's step:

```
            with torch.amp.autocast(device_type=device.type, dtype=torch.float16):
                logits = model(inputs)
                loss = torch.nn.functional.cross_entropy(
                    logits.flatten(0, 1), outputs.flatten()
                )
            scaler.scale(loss).backward()
            scaler.step(optimizer)
            scaler.update()
```

Running that gives us these results:

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python measure-tokens-per-second.py
Loading dataset shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:00<00:00, 340.25it/s]
Testing with batch size 1
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:07<00:00, 13.38it/s]
Done, trained on 102,400 tokens in 7.4764s.
Tokens per second: 13,696

Testing with batch size 2
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:12<00:00,  8.11it/s]
Done, trained on 204,800 tokens in 12.3286s.
Tokens per second: 16,611

Testing with batch size 3
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:16<00:00,  6.02it/s]
Done, trained on 307,200 tokens in 16.6238s.
Tokens per second: 18,479

Testing with batch size 4
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:21<00:00,  4.67it/s]
Done, trained on 409,600 tokens in 21.3936s.
Tokens per second: 19,145

Testing with batch size 5
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:25<00:00,  3.87it/s]
Done, trained on 512,000 tokens in 25.8624s.
Tokens per second: 19,797

Testing with batch size 6
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:30<00:00,  3.25it/s]
Done, trained on 614,400 tokens in 30.7239s.
Tokens per second: 19,997

Testing with batch size 7
  0%|                                                                                                                                             | 0/100 [00:00<?, ?it/s]
Traceback (most recent call last):
  File "/home/giles/Dev/llm-from-scratch/measure-tokens-per-second.py", line 94, in <module>
    main()
```

Wow! With that we can train on 3.2B tokens in about 160,000 seconds, which is 44 hours.
That's definitely doable.

Now, what happens if we remove the

```
torch.set_float32_matmul_precision("high")
```

...so that we're using AMP, but not the tensor cores?

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python measure-tokens-per-second.py
Loading dataset shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:00<00:00, 365.94it/s]
Testing with batch size 1
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:07<00:00, 13.03it/s]
Done, trained on 102,400 tokens in 7.6736s.
Tokens per second: 13,344

Testing with batch size 2
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:12<00:00,  8.04it/s]
Done, trained on 204,800 tokens in 12.4383s.
Tokens per second: 16,465

Testing with batch size 3
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:16<00:00,  5.96it/s]
Done, trained on 307,200 tokens in 16.7851s.
Tokens per second: 18,301

Testing with batch size 4
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:21<00:00,  4.64it/s]
Done, trained on 409,600 tokens in 21.5571s.
Tokens per second: 19,000

Testing with batch size 5
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:25<00:00,  3.85it/s]
Done, trained on 512,000 tokens in 25.9610s.
Tokens per second: 19,721

Testing with batch size 6
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:30<00:00,  3.24it/s]
Done, trained on 614,400 tokens in 30.8405s.
Tokens per second: 19,921

Testing with batch size 7
  0%|                                                                                                                                             | 0/100 [00:00<?, ?it/s]
Traceback (most recent call last):
  File "/home/giles/Dev/llm-from-scratch/measure-tokens-per-second.py", line 93, in <module>
    main()
    ~~~~^^
  File "/home/giles/Dev/llm-from-scratch/measure-tokens-per-second.py", line 81, in main
```

It's basically the same. 300tps slower at the start, down to 70 at the end.
Still, it looks better to keep the "high" precision in place, rather than the "highest".

Right. We have the beginnings of a training loop that *should* be able to let us
run a Chinchilla-optimal train on a GPT-2 small sized model in 44 hours, and I have the
time to do it. And it looks like a batch size of six is what we can fit into the
RTX 3090's 24 GiB of VRAM.

What else are we going to need to build something to do this?

### Checkpointing

If I want to do a long training run, then stuff might go wrong -- it might crash for
some reason.
So we're going to need to save checkpoints as we go and be able to restart training
from those checkpoints.

In those, we're going to need to save the model and the
optimiser's state, plus some kind of info about how far through the dataset we are.
We should keep training and validation losses too, so that we can easily chart and
recover our progress, and according to [this forum post](https://discuss.pytorch.org/t/do-i-need-to-save-the-state-dict-oof-gradscaler/95718/4)
we're going to need to save the scaler (which makes me think that it actually has state in
it, so we probably should have used a fresh scaler for each batch size in the
above -- let's hope that doesn't prove to be a problem [note from later: it wasn't]).

I wrote a [script](https://github.com/gpjt/llm-from-scratch/blob/main/test-checkpointing.py) to create a model, train it for a bit, and then dump out all of that
apart from the metadata (which I reckon is going to be less than 1kB). I wanted to
use the [safetensors](https://huggingface.co/docs/safetensors/en/index) format for
all of it, but unfortunately I couldn't get it to work for the optimiser or the scaler,
so had to use `torch.save` for those (which I don't like because it uses [pickle](https://docs.python.org/3/library/pickle.html),
which introduces serious problems if you ever want to move files from machine to machine,
as the Python and library versions need to match perfectly). Ah well. Here's what
the test checkpoint looks like:

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ du -sh test-checkpoint
1.9G    test-checkpoint
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ ls -lh test-checkpoint
total 1.9G
-rw-r--r-- 1 giles giles 670M Nov 11 15:21 model.safetensors
-rw-r--r-- 1 giles giles 1.3G Nov 11 15:21 optimizer.pt
-rw-r--r-- 1 giles giles 1.4K Nov 11 15:21 scaler.pt
```

That's huge! And it's almost all the optimiser. From what I read, that stores two numbers per parameter, so
it makes sense that it's double the size of the model weights. And at 32-bit,
4 bytes per param, then 670MiB for the model is sane.

Timing-wise, it takes about a second to save, the same to load, so that's fine.

So that sounds reasonable in terms of timing, and disk space is pretty high, but not
so huge that it can't be managed with careful planning -- don't checkpoint so much that
we run out of disk during the train (I have a 2TiB disk, but it's far from empty).

It's probably worth double-checking that it works, though! Because my checkpoint
test already did some training, I changed it so that it does this:

* Create a model, optimiser and scaler.
* Train the model for a bit.
* Work out the loss.
* Save a checkpoint.
* Create a new model, optimiser, and scaler, and then restore the checkpoint into them.
* Work out the loss
* Train for a bit more to check that the optimiser and scaler still work.

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python test-checkpointing.py
Loading dataset shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:00<00:00, 387.76it/s]
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:30<00:00,  3.30it/s]
Loss prior to checkpoint: 7.0519
Checkpoint saved in 0.96s
Checkpoint loaded in 0.89s
Loss after checkpoint load: 7.0519
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:30<00:00,  3.27it/s]
Loss after further training: 6.8996
```

Looks sane! The numbers for loss are the same before and after, so I think it's vanishingly
implausible that the checkpoint we restored is different from the one we saved. And
the continued training seems to be working -- at least, loss is going down -- so that
sounds reasonable too.

OK, so, again, the time taken to checkpoint is negligible, but the disk space isn't. I
reckon we can comfortably do 100 checkpoints over the train. That's roughly one every
half-hour over 44 hours.

We're going to want to do a validation run each time we checkpoint, so let's think about that next.

### Validation

How big should our validation set be?
Let's say we only want to spend 5m per checkpoint period doing validation. How many
batches can we get through in that time?

I wrote a simple script to run a model (after a few hundred training steps) in eval
mode on different numbers of iterations to see how long each one
took. It used the same `autocast` trick as the
training loop above in order to use mixed precision, and I ran it with `torch.inference_mode` instead
of the `torch.no_grad` that I've used in the past (ChatGPT tells me it's a little faster).
I also put in some calls to `torch.cuda.synchronize` around the loop that I was timing,
which should apparently help make sure that the numbers are precise. The code is
[here](https://github.com/gpjt/llm-from-scratch/blob/main/measure-validation-timing.py) if
you'd like to take a look.

After some fiddling with the min/max numbers at the top:

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python measure-validation-timing.py
Loading dataset shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████| 102/102 [00:00<00:00, 352.52it/s]
Doing initial train
100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [00:30<00:00,  3.25it/s]
Timing validation batch size 2900
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 2900/2900 [04:29<00:00, 10.76it/s]
Got loss 7.3029 in 269.5059s
Timing validation batch size 3000
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3000/3000 [04:39<00:00, 10.73it/s]
Got loss 7.3044 in 279.4869s
Timing validation batch size 3100
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3100/3100 [04:46<00:00, 10.81it/s]
Got loss 7.3042 in 286.6812s
Timing validation batch size 3200
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3200/3200 [04:55<00:00, 10.82it/s]
Got loss 7.3043 in 295.7016s
Timing validation batch size 3300
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3300/3300 [05:04<00:00, 10.82it/s]
Got loss 7.3065 in 304.9547s
Timing validation batch size 3400
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3400/3400 [05:14<00:00, 10.82it/s]
Got loss 7.3060 in 314.3070s
Timing validation batch size 3500
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3500/3500 [05:25<00:00, 10.76it/s]
Got loss 7.3062 in 325.1689s
Timing validation batch size 3600
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3600/3600 [05:35<00:00, 10.73it/s]
Got loss 7.3064 in 335.6270s
Timing validation batch size 3700
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3700/3700 [05:44<00:00, 10.73it/s]
Got loss 7.3083 in 344.8765s
Timing validation batch size 3800
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3800/3800 [05:54<00:00, 10.73it/s]
Got loss 7.3111 in 354.3010s
Timing validation batch size 3900
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3900/3900 [06:03<00:00, 10.72it/s]
Got loss 7.3104 in 363.6413s
Timing validation batch size 4000
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 4000/4000 [06:11<00:00, 10.76it/s]
Got loss 7.3110 in 371.8712s
```

OK, so let's call it 3200. That's 3200 \* 6 \* 1024 tokens = 19,660,800 tokens.

That's about 0.006144 of our training set. Pretty low, but we're talking about such
a large training set that I think we're OK. And practically we can't do more --
we're already talking about 5 mins every half-hour, so we're bumping up our train time
by 88 \* 5 = 440 minutes, which is seven hours.

Now let's start thinking about the datasets.

### Datasets

We can split the HF thing into train and validation sets. I'm thinking
it might be useful to load all of our training and validation data into RAM for the train loop. 3.2B tokens
with four bytes per token should be about 13 GiB, after all, and I have 64 GiB RAM on the
machine.

...but wait, int64 is the default for PyTorch for long ints -- that's what our token lists are in the original,
and it's twice the size, so we're talking 26 GiB.
I believe that PyTorch expects that format for the cross entropy loss.

That's not the end of
the world, though -- we can store the data as int32 in RAM (with 50,257 as our vocab size we
could even use int16 if we wanted to) and then we'll need to make them
the right type just before using them. We can do that when splatting them onto the
GPU, eg.

```
x = x_int32.to(device).to(torch.long)
```

First thought, can we store them as a Python list? Turns out they're not all that memory-efficient, though:

```
In [2]: list(range(3_200_000_000))
Killed                     ipython
```

How about PyTorch tensors?

```
In [3]: torch.rand((3_200_000_000))
Out[3]: tensor([0.6668, 0.1471, 0.9428,  ..., 0.3548, 0.5738, 0.5723])
```

Promising! (Though ChatGPT pointed out when reviewing a draft of this post that
I was using the default `float32` rather than an `int32` type here. Still, it's
the same size.)

Let's measure memory usage in a new interpreter.

```
In [1]: import psutil

In [2]: import torch

In [3]: import os

In [4]: rss_before = psutil.Process(os.getpid()).memory_info().rss

In [5]: t = torch.rand((3_200_000_000))

In [6]: rss_after = psutil.Process(os.getpid()).memory_info().rss

In [7]: rss_after - rss_before
Out[7]: 12801474560
```

Yup, 12,801,474,560, so about 12 GiB. Can we save it?

```
In [8]: from safetensors.torch import save_file

In [9]: save_file({"tokens": t}, "xxx")
```

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ ls -l xxx
-rw-r--r-- 1 giles giles 12800000088 Nov 11 20:43 xxx
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ ls -lh xxx
-rw-r--r-- 1 giles giles 12G Nov 11 20:43 xxx
```

OK, let's try reloading it in a fresh session:

```
In [1]: from safetensors.torch import load_file

In [2]: t = load_file("xxx")["tokens"]

In [3]: t
Out[3]: tensor([0.5421, 0.1613, 0.8055,  ..., 0.7002, 0.7609, 0.5629])
```

Nice. So, I think we can write a quick script that splits our incoming dataset
into say 99/1% train and validation, grabs the first 3.2B tokens from the training set,
glomming them together into one big tensor with EOSes between them, and saves them, and then does likewise
for the first 19,660,800 tokens from the validation set. We'll use FineWeb, with
the possibility of switching to FineWeb-Edu later on. Doing it that way means that
we're actually using the second of the two options I considered earlier:

> Treat the corpus as, essentially, one long document, with end-of-sequence delimiters
> between each row, then split that up into 1,024-token sequences.

I thought it would be harder than concatenating/padding rows, but it actually turns out to be simple enough.

Let's give it a go. [Here's the code](https://github.com/gpjt/llm-from-scratch/blob/843da6e19927ef1235b7989032e00c31d6c4396b/big-train-prepare-datasets.py).
I wanted to have an round number of 6-sequence batches of 1,024 tokens each, so the
the number of training tokens worked out at

534,200×6×1,024=3,282,124,800

...rather than the strict Chinchilla-optimal 3,260,190,720, but that's no biggie.

Running it takes 5m55s, and then:

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ ls -lh big-train-datasets/
total 13G
-rw-r--r-- 1 giles giles 13G Nov 11 23:08 train.safetensors
-rw-r--r-- 1 giles giles 76M Nov 11 23:02 validation.safetensors
```

Looks about the right size -- 19M \* 4 for val, 3.2B \* 4 for train.

Cool! Let's finally write our training script.

### Finally training an LLM!

You can see [the full training script here](https://github.com/gpjt/llm-from-scratch/blob/main/big_train.py) -- note
that this is the final version from the repo, so isn't exactly what I'm running
at this point in the post. The checkpointing code is (sensibly enough) in a separate file,
[`checkpointing.py`](https://github.com/gpjt/llm-from-scratch/blob/main/checkpointing.py).

It took two days to run, and...

![Training and validation loss over two days, FineWeb](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/big-training-run-chart-fineweb.png "Training and validation loss over two days, FineWeb")

Both train and validation losses fall nicely! Training loss is a bit choppy, but that's because I erroneously
only plotted the most recent iteration's training loss rather than an average over all iterations
between the last and current validation run; the validation loss is correct because I
did average all of the validation numbers. (The version of the code linked above fixes that
error.)

The best epoch for val loss is not the last one but it was close. Looking at the last 5 iterations,
their val losses were:

```
3.991096583977342
3.940103444904089  <-- best
3.9403586230427026
3.9464842446893456
3.9469190353155135 <-- latest
```

It's time to do some evals

### Evals

Firstly, let's try the smoke test that we do in the book. What does our model
think should come after the text "Every effort moves you"?

With uninitialised weights we get gibberish, as expected

```
Every effort moves youワISIS Keectar handling holistic Supply query prolongidation Joey flaw camerasIdent formula
```

But with our best checkpoint we get this:

```
Every effort moves you towards a sustainable and holistic diet of water, protein, vitamins, and protein
```

Nice! The multiple mentions of protein is actually the kind of repetition that small
models tend to do, so that's not bad news.

Let's try with the last iteration's checkpoint:

```
Every effort moves you towards a new level of success, and you’re likely to continue
```

Also very nice, perhaps better!

I think that both of those are qualitatively as good as the result we got when
we [loaded the pre-trained weights from OpenAI](/2025/10/llm-from-scratch-22-finally-training-our-llm),
which was:

```
Every effort moves you as far as the hand can go until the end of your turn unless something interrupts your control flow. As you may observe I
```

That's very reassuring. But is there something a bit more quantitative that we can do?

Firstly, can we compare it to anything in the GPT-2 paper? In figure 4 they give
their perplexity against their train and test sets for the different model sizes;
for the small one it's a bit over 16, Let's assume that they're basing that on natural logarithms,
so they mean that they have a loss of ln16. That's `2.77`, which is much lower than our
best loss of 3.9401.

However, that is across different datasets, so while it makes me suspect that their
model is better than ours, we can't really say for sure either way.

The cool thing is, though, that we *have* their model -- so we can actually run it against
our dataset. I wrote a script called [`test_openai_weights_against_our_val_dataset.py`](https://github.com/gpjt/llm-from-scratch/blob/main/test_openai_weights_against_our_val_dataset.py),
and running it gives us this:

```
Loss against our validation dataset: 3.4987249702960255
```

Still better than ours :-(

I considered doing the same thing against Qwen to see whether that was also better,
but with a different tokeniser we couldn't really treat it as comparable. Loss and
perplexity are both over next-token predictions, and if the meaning of "token" changes,
then the numbers will change. [2](#fn-2)

OK, so we have a model, but it's not as good as the original GPT-2 small. Our
loss on our validation set is roughly 3.94, while the original weights get about 3.50. Expressing
that in terms of perplexity gives our own model about 51.4, while the original
has 33.1. That's actually still higher than the 16 that they had in the paper, which
is interesting -- presumably it's related to the fact that they're validating over
their own WebText test set rather than ours; they're both samples of web content,
but there must be differences.

At this point, my guess is that this shows that all of that extra training that the OpenAI team did beyond
the Chinchilla-optimal number of tokens did have a real benefit -- and that's not
suprising. Remember that the Chinchilla paper is about the best way to spend a FLOPs
budget. They're not saying that you can't drive down loss by continuing to train
your model further -- of course you can. They're saying that when you pass the
optimal number of tokens, you should increase the model parameters and the tokens
by the same ratio, and by doing that you'll get the best balance.

But still, a Chinchilla-optimal model of 163M parameters might still be useful.
What happens if we instruction fine-tune it [like we did the original model in
Chapter 7 of the book](/2025/10/llm-from-scratch-25-instruction-fine-tuning)? In that
post and its [followup](/2025/11/llm-from-scratch-26-evaluating-the-fine-tuned-model),
we used some training samples using the "Alpaca" one-shot
question-answering format:

```
Below is an instruction that describes a task.  Write a response that
appropriately completes the request.

### Instruction:

<some instructions>

### Input:

<optional, some input>

### Response:
```

...to get a model that we then provided a test set of questions in the same format,
then used the Llama 3 7B model to judge the results on a scale of 0 to 100. We then
averaged the results and got a plausible-looking indicator of how useful the model was,
as compared to the more narrowly technical loss number.

One problem with that is that we ran those tests on the OpenAI weights for the medium-sized 355M-parameter
GPT-2 model. If we don't want to be comparing apples to oranges, we'll need to re-run it on
their weights for the small model. Let's see how we do.

First, let's run it for five epochs just to see when/if it starts overfitting:

![Loss over five epochs training GPT-2 original weights on Alpaca](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/instruction-fine-tune-5-epochs-losses-plot-original-weights.png "Loss over five epochs training GPT-2 original weights on Alpaca")

OK, so two epochs looks like the right amount, just as it was with the medium model.
So we can train for that (because I'm using the original code I wrote when working
through the chapter, I didn't checkpoint during training -- but it takes less than a
minute to run the whole thing, so no biggie). Here's the loss chart:

![Loss over two epochs training GPT-2 original weights on Alpaca](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/instruction-fine-tune-2-epochs-losses-plot-original-weights.png "Loss over two epochs training GPT-2 original weights on Alpaca")

Validation loss at the end is 0.733, noticeably above the 0.649 that I got with the
medium-sized model. And the sample outputs shown at the end aren't as good, either.
With the medium-sized model, I got these:

```
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Rewrite the sentence using a simile.

### Input
The car is very fast.

Correct response:
>> The car is as fast as lightning.

Model response:
>> The car is as fast as a bullet.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
What type of cloud is typically associated with thunderstorms?

Correct response:
>> The type of cloud typically associated with thunderstorms is cumulonimbus.

Model response:
>> The type of cloud typically associated with thunderstorms is a cumulus cloud.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Name the author of 'Pride and Prejudice'.

Correct response:
>> Jane Austen.

Model response:
>> The author of 'Pride and Prejudice' is Jane Austen.
```

...but with the small model (remember, this is with OpenAI's original weights) I get this:

```
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Rewrite the sentence using a simile.

### Input
The car is very fast.

Correct response:
>> The car is as fast as lightning.

Model response:
>> The car is as fast as a horse.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
What type of cloud is typically associated with thunderstorms?

Correct response:
>> The type of cloud typically associated with thunderstorms is cumulonimbus.

Model response:
>> A type of cloud typically associated with thunderstorms is the active layer.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Name the author of 'Pride and Prejudice'.

Correct response:
>> Jane Austen.

Model response:
>> The author of 'Pride and Prejudice' is Robert Frost.
```

Definitely worse, especially the last one! Let's see what Llama 3 thinks of it,
again using the code from the book:

```
Number of scores: 110 of 110
Average score: 35.50
```

The medium model got an average of 50, so the OpenAI small model is definitely much worse, as the examples
suggested. Makes sense.

Let's see how our own base model performs when fine-tuned on the same data.
After a bit of fiddling I found that validation loss settled down at the end of epoch
10:

![Loss over ten epochs training our FineWeb base model on Alpaca](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/instruction-fine-tune-10-epochs-losses-plot-our-model-fineweb.png "Loss over ten epochs training our FineWeb base model on Alpaca")

(It's hard to see from the chart, but validation loss was actually very slowly
dropping even after epoch 5.)

It's interesting that our own model took longer to train here, but it does make sense
in terms of it being that little bit dumber.

The samples it printed out at the end are also interesting:

```
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Rewrite the sentence using a simile.

### Input
The car is very fast.

Correct response:
>> The car is as fast as lightning.

Model response:
>> The car is as fast as a cheetah.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
What type of cloud is typically associated with thunderstorms?

Correct response:
>> The type of cloud typically associated with thunderstorms is cumulonimbus.

Model response:
>> A thunder storm is a type of thunder.
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Name the author of 'Pride and Prejudice'.

Correct response:
>> Jane Austen.

Model response:
>> The author of 'Pride and Prejudice' is Robert Frost.
```

The simile is pretty good, I think better than the OpenAI original weights' one,
but the storm clouds one is dreadful. It's fascinating that they both chose the same
wrong answer for "Pride and Prejudice" -- my guess is that it's because the training
set contained this question:

```
Identify the tone used in the poem 'The Road Not Taken' by Robert Frost.
```

...so both models picked up on Robert Frost being a useful author to reference in
answers.

Anyway, what does Llama 3 think of the output?

```
Number of scores: 110 of 110
Average score: 29.35
```

Yup, it's dumber than the original weights -- but, at least to my mind, closer to
the original weights' score than you might have thought based on that loss/perplexity
number alone.

But, on the other hand, I'm not convinced that Llama 3 7B is smart enough to be
doing a good job. In the stuff the eval script printed out, we have this:

```
Dataset response:
>> The type of cloud typically associated with thunderstorms is cumulonimbus.

Model response:
>> The type of cloud is typically associated with thunderstorms.

Score:
>> I'd score this model response a 40 out of 100.

Here's why:

* The response partially answers the question by mentioning that cumulonimbus clouds are associated with thunderstorms.
* However, it lacks specific details and doesn't provide a clear answer to the question.
* A good response should provide a complete and accurate answer, which this one does not.

A better response would be something like: "The type of cloud typically associated with thunderstorms is cumulonimbus." This response provides a clear and accurate answer to the question.
```

This is clearly completely wrong, the mention of cumulonimbus is coming from the
dataset response, not the model response. Llama 3 7B is tripping up over what
came from where, which is pretty normal for a small model.

Of course, it's possible that the scores for the OpenAI GPT-2 small weights also have
been given a higher rating than they deserve -- or, indeed, that there were right
answers that were incorrectly judged wrong. Conceivably it averages out. But there's
no reason to assume it would, so it's essentially noise and is making the results less
useful.

Let's try using a much smarter LLM as a judge and run both of the models responses
through it -- the just-released OpenAI GPT-5.1 model. The code is [here](https://github.com/gpjt/llm-from-scratch/blob/main/evaluate-with-openai.py).

Running that against our own model's answers:

```
Number of scores: 110 of 110
Average score: 16.14
```

...and against the model fine-tuned from the small OpenAI weights:

```
Number of scores: 110 of 110
Average score: 20.39
```

...and, of course, it didn't make the mistake of confusing the dataset response with
the model's in any of the cases printed out. ChatGPT 5.1 in the chat interface is
very smart, I expect these results are much closer to a reasonable ground truth.

Out of interest, what does it make of the model based on the GPT-2 **medium** weights that we train as part of the book?

```
Number of scores: 110 of 110
Average score: 38.41
```

That's as compared to an average of about 50 from Llama 3 7B. It seems like GPT 5.1
is a tougher judge than the small local model -- and my guess is that that is because
it's more accurate. [3](#fn-3)

Anyway, the ranking remains the same; after fine-tuning on the same Alpaca dataset,
GPT-2 medium > GPT-2 small > our model. But it's still a relatively close-run thing
between our model and GPT-2 small. Can we close the gap without vast amounts of
extra training?

### FineWeb-Edu

The results so far were from using 3.2B tokens of the FineWeb 10B corpus. Now, as
I noted at the start of this post, Andrej Karpathy's nanochat project uses FineWeb-Edu,
a separate corpus designed to be really informative. Indeed, back at the start when
we were looking at the two datasets, the first row in the Edu dataset was about
Jane Austen, so maybe we would wind up with a model that at least got that question right!

That's going to take another two days to train, but that's no big deal. We first
need to change our script that generates the train/validation splits to regenerate
them using the Edu dataset; we'll move the old ones to one side, though -- it will
be interesting to see what loss we get on the non-edu validation data with the new model.

(Note to self: work out some way to split out different datasets and training runs for
future experiments like this. The setup I had in my [recent post on RNNs](/2025/10/retro-language-models-rebuilding-karpathys-rnn-in-pytorch)
worked quite well. Throughout the remainder of this post I'm juggling directories of
checkpoints and datasets, and I'm sure I got it right, but it was an error-prone process.)

That being done, it's time to move the checkpoints we already have to one side, and
to kick off the train!

Here's what we have after two days on that -- oops, I forgot to add the code to average
training loss across all of the batches, so again it's a bit spiky.

![Training and validation loss over two days, FineWeb-Edu](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/big-training-run-chart-fineweb-edu.png "Training and validation loss over two days, FineWeb-Edu")

But we got to
a final eval loss of about 3.693 this time. Of course, that's on its own validation
set, so it's not comparable with the numbers from before; loss is specific to a particular
dataset. Let's see what it makes
of the original run's validation set. Juggle some directories around (my messy file
structure means that there is just one "datasets" directory and one "checkpoints" one,
so I'm moving them around to make sure I'm using the right combination):

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python test_our_weights_against_our_dataset.py
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3200/3200 [04:52<00:00, 10.92it/s]
Loss against our validation dataset: 4.164705707877874
```

We get 4.16! That's truly terrible, worse than both the original base model that
we trained on FineWeb's non-edu dataset, and than the OpenAI GPT-2 small weights.

Let's see what we get from the closer-to-real-world instruction fine-tuning test.
Five epochs turns out to be best:

![Loss over five epochs training our FineWeb-Edu base model on Alpaca](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/instruction-fine-tune-5-epochs-losses-plot-our-model-fineweb-edu.png "Loss over five epochs training our FineWeb-Edu base model on Alpaca")

I won't bother running it past Llama 3 7B, as that's proven unhelpful, so we'll go
straight to GPT-5.1.

```
Number of scores: 110 of 110
Average score: 15.18
```

Gosh! So it's judged slightly worse than our weights based on FineWeb.
That does surprise me a bit. I was definitely expecting the Edu version of the
dataset to give us a better model.

So: OpenAI medium > OpenAI small > our FineWeb base model > our FineWeb-Edu base model.
That last pairing does surprise me a bit. Handwaving wildly, perhaps the more "regular" nature of
the Edu dataset meant that the model saw less variation in its training set, and
that actually made it learn less?

I think there's one more experiment I want to do before bringing this (*very*
lengthy) post to a close. We've shown that Chinchilla-optimal training of models
produces worse results than OpenAI's original, we think longer, train.

What would happen if we continued training for another two days?

### Continuing training

As I have it easily to hand, I want to use the FineWeb-Edu model for this. I want
to start with the best checkpoint (which happens to be the last one), and train
it on another 3.2B tokens from FineWeb-Edu. Let's see what we get.

Getting a dataset is going to be a bit messy, as our existing script to generate the
safetensors datasets
just grabs tokens from the original dataset until it gets 534,200 batches of 6 sequences, each
of 1024 tokens (3,282,124,800 total).

Might as well hack it (and note that this is something worth improving for any
later experiments). I'll just loop round the code to do that twice, throwing
away the first set of 3.2B tokens.

I was pretty sure that the ordering of the datasets I'm
getting is fixed, but perhaps not -- it spent time regenerating the train/val split
at the start of the script, so there's no guarantee we have different data this time.
That feels like a note-to-self about data pipeline hygiene -- if the train/val split
is randomised by the infra I'm using, I should persist the raw data in case I need to
use more data than I though I would need to.

Still, for this experiment, we can play relatively fast and loose. After all, GPT-2
small -- the original OpenAI weights -- was trained on multiple epochs, so it saw tokens
multiple times. What we're trying to see here is what happens if you train for longer;
a more scientific experiment can happen later (if at all...).

Anyway, we have 3.2B tokens that should at least be reasonably different from the original 3.2B.

Right, let's clean up some disk space so that we have enough for the new train (deleted
some old optimiser checkpoints, keeping the metadata and the weights).

Now, we create a new checkpoints directory, and we can copy the last/best checkpoint
from the original FineWeb-Edu train there. Hack the `train_ds_offset` in there to
zero, create `best` and `latest` symlinks, and then we can "restart" from that checkpoint.
Due to the way the restart-from-checkpoint code works in the training script, that means that it will start with an offset of 1 into the dataset, so we're
dropping one of about 530,000 iterations, but that's not exactly the end of the
world.

![Training and validation loss over a second period of two days, FineWeb-Edu](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/big-training-run-chart-fineweb-edu-2x.png "Training and validation loss over a second period of two days, FineWeb-Edu")

There are some interesting spikes on validation loss in there -- in particular that one
at around iteration 300,000 where it goes up from 3.6 or so to 7.5 for two validation
periods (which, remember, happen every ~30 minutes, or every 7020 iterations).

My guess
is that we got some kind of gradient spike prior to those, which led to a bad update
to the parameters. However, it looks like the loss recovered really quickly after it,
so while gradient clipping (that is, limiting the size of the gradients so that one-off
spikes don't cause massive updates) might have prevented them, I don't think it would
have improved matters much -- we might have "lost" an hour so of training, but out
of a 44-hour train (48 hours including breaks for validation), it's not the end
of the world.

But, looking at the raw numbers, after our second two days of training on a fresh
sample from FineWeb-Edu 10B, we've managed to get the loss on our validation set down from
3.693 to... drumroll... 3.661. And that's on the "best" measurement, which was an hour
before the end. The last validation number was 3.663.

By spending twice the time, we've managed to get our loss down by 0.032, which is
a touch less than 1%. Even measured in terms of perplexity (which, being an exponential,
is more sensitive to this kind of change), we've gone from 40.2 to 38.9, which is
hardly show-stopping.

Let's see how this one measures up against the non-edu FineWeb validation dataset that we
originally used to calibrate our first training run. Run it, and:

```
(llm-from-scratch) giles@perry:~/Dev/llm-from-scratch (main)$ python test_our_weights_against_our_dataset.py
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3200/3200 [04:53<00:00, 10.89it/s]
Loss against our validation dataset: 4.134009174928069
```

...we get 4.13 -- that's opposed to 4.16 on the last model, trained on half as much data.

Well, maybe it's a much better base model for instruction fine-tuning? Let's give that
a go, again with the Alpaca training set from the book. 8 epochs turns out to be
the right number:

![Loss over eight epochs training our "double-trained" FineWeb-Edu base model on Alpaca](/post-assets/llm-from-scratch-28-training-a-base-model-from-scratch/instruction-fine-tune-8-epochs-losses-plot-our-model-fineweb-edu-2x.png "Loss over eight epochs training our \"double-trained\" FineWeb-Edu base model on Alpaca")

```
Number of scores: 110 of 110
Average score: 16.62
```

Certainly better than the 15.18 that we got on our Chinchilla-optimal FineWeb-Edu model,
and a bit better than the 16.14 we got on the Chinchilla-optimal FineWeb one.
So by training for double the time on twice the data, we've definitely got a better
model. It's just not *that much* better.

I think that's more -- significantly more -- than enough experimentation for one blog post, so let's do some
analysis.

### FLOPs

I want to sanity-check the number of FLOPs spent on this train, just to make sure
that I hadn't messed up. Feel free to [skip this](#but-why-is-our-model-worse-than-openais) if you want to jump straight to the
conclusion :-)

In appendix F, the Chinchilla paper mentions a common approximation for how many FLOPs, C, you
spend training a model with N parameters over D tokens:

C=6DN

So based on that, each of those training runs cost us (using the exact numbers for N and D) this many FLOPs:

C=6×3,282,124,800×163,009,536=3,210,105,844,452,556,800=3.21×1018FLOPS

They also give a more carefully-worked out calculation; it doesn't look all that
difficult -- it's just a case of plugging in the numbers from our architecture and
pulling out a result [4](#fn-4) -- but the numbers they get from that are generally within
10% of the simpler calculations, so we may as well stick with the above. [5](#fn-5)

Now, in terms of how many FLOPs we actually spent... well, manufacturers' datasheets
for hardware are based on carefully-selected benchmarks and won't really be comparable
to the code we were running (especially given that it's my crappy code based on top
of a huge stack of PyTorch, CUDA kernels, CUDA itself, and so on), but we can do a
[Fermi estimate](https://en.wikipedia.org/wiki/Fermi_problem).

From Wikipedia, the [RTX 3090](https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units#RTX_30_series) has
35.58 TFLOPS performance on FP32. Way back earlier in this post, when I was
measuring how many tokens per second I could get locally, the first experiment
capped out at 12,599 tokens/second with FP32. `nvtop` showed the GPU usage at 100%,
so let's say (again, this is very approximate) that we were getting about 35.58 TFLOPs
and that enabled 12,599 tokens/second.

We wound up training at about 19,921 tokens/second
after adding in mixed precision and using the tensor cores. So, hand-wavingly
we can say that we were getting

19,92112,599×35.58=56.26TFLOPs

Now, we trained for 44 hours (48 including validation), so the total number of training FLOPs
should have been the number of seconds in that times the total FLOPS [6](#fn-6)
of 56.27×1012

44×60×60×56.27×1012=8.91×1018

That's pleasingly close to the 3.19×1018 above! I can easily imagine that the stack we're using could
somewhat-more-than-halve performance from the theoretically optimal, or that we're running at 50% of
the GPU's theoretical capacity, or some combination of the two. We're in the same
order of magnitude, and for a Fermi approximation, that's what matters.

Now, looking at figure 3 in the Chinchilla paper, their IsoFLOP curves (each one showing the loss they got
on their training set for models of a particular size, using the same number of
FLOPs for each curve), we can see that the top one, which is
training runs of 6×1018 FLOPs, the lowest point is pretty much bang-on
the 168M point on the X axis.

So that is at least reassuring that we did do a proper Chinchilla-optimal train here.
(Their loss on that chart is showing 3, but they're using a different dataset, so I don't think
it's comparable.)

### But why is our model worse than OpenAI's?

Apart from the obvious answer of "skill issue", let's see if there are any obvious
reasons why the base model I've trained (and retrained) in this post is worse than
the original OpenAI GPT-2 small. Let's review the results first:

|  | FineWeb train | FineWeb-Edu train | FineWeb-Edu extended train | OpenAI weights |
| --- | --- | --- | --- | --- |
| Val loss on own dataset | 3.94 | 3.693 | 3.661 | 2.80 [7](#fn-7) |
| Val loss on FineWeb dataset | 3.94 | 4.16 | 4.13 | 3.50 |
| Alpaca answers judged by GPT-5.1 | 16.14 | 15.18 | 16.62 | 20.39 |

The first row is not super-interesting, it's the second and third that matter.

* On *our own* validation set from FineWeb, our we have OpenAI > our FineWeb train > our FineWeb-Edu extended train > our FineWeb-Edu train
* On the answers judged by GPT-5.1 after instruction fine-tuning, we have OpenAI > our FineWeb-Edu extended train > our FineWeb train > our FineWeb-Edu train

OpenAI is clearly winning by quite some margin! Earlier on I assumed that the difference
was that they trained on more data, but let's be a bit more systematic here.

What specific differences do we
have to the original train? Again, the amount of data in the paper is frustratingly
limited, but:

#### Amount of training data

Right at the start, I estimated that the WebText dataset they trained on was about 10B
tokens. We've trained on 3.2B tokens for two of our models, and 6.4B tokens for the extended
train one.

That could well have an effect. There's more information in their larger dataset,
both in terms of raw facts like "Jane Austen wrote Pride and Prejudice", and in terms of
information about the structure of language.

On the other hand, their dataset is, as they say, comprised of the contents of web pages that
were linked from Reddit posts with more than three upvotes. FineWeb (and even more FineWeb-Edu) is
a much more curated dataset, so you would expect it has more facts, and better structure
-- less of the slop and junk that Andrej Karpathy talked about in his interview with Dwarkesh
Patel.

So I'm not sure that this is it, but it's worth keeping in mind.

#### Number of epochs

Again, we don't know how many epochs they trained on, but the report I linked to
right at the start of this post estimated that they trained for 60, while I calculated based
on their numbers that it would be 41 epochs with WebText.

It certainly makes sense that grinding along, epoch after epoch, will get your loss
down, at least on the training set! And there's also a phenomenon with certain kinds
of neural networks where if keep training past the point where you're overfitting
(that is, validation loss starts rising while training loss continues to fall),
suddenly the model can have an "aha" moment and [start generalising again](https://arxiv.org/abs/2201.02177). [8](#fn-8)

It's not quite comparable, because it was not a second epoch, but rather continued training
with more data, but we were able to eke out an extra reduction of 0.032 in loss by
training our FineWeb-Edu model for twice as long. If we'd trained it for 40 times
as long, then we presumably would have managed to grind it down even further. I
have no idea how much further we could get it, but I'd guess that it's going to be
worse than linear (that is, each extra two days gets you less loss reduction than
the previous) -- so we can bound the loss reduction at a *maximum*
of 39×0.032=1.248.

So... maybe? It would be a dull experiment to run, though, taking 78 days. If
I want to do that, it would be better to find a way to do it quickly, so that I can get
a better feedback loop going. The reason this post has taken so long has in part been
because each training run has taken so long (as well as trips to London and other life
stuff).

#### Architectural differences

The original GPT-2 model from OpenAI had bias on the Wq, Wk and Wv projections -- that is,
they were normal NN biased linear layers rather than simple matrices, so they did
a projection into their respective spaces followed by a translation. In the book,
Raschka says that this is not normally done these days, which is why I didn't do it
for this base model train.

But perhaps it actually is valuable with this architecture or size? Modern models
presumably differ in multiple ways, and perhaps the bias would have been useful for
this old design.

Likewise, weight-tying -- the original GPT-2 re-used its embedding matrix to do the
final projection from embedding space to vocab space, rather than having a separate one.
That seems intuitively clever but not necessarily "right", given that it gives the
model less flexibility in what it can output from the last layer. But perhaps with
this size and architecture, it's the right thing to do?

#### Dropout

Contrariwise, having made those two changes to GPT-2 because I believed that modern
models don't work that way, there was one "modern" change that I didn't make. In his post on the
architectural changes since GPT-2, Raschka mentioned that dropout is normally not used nowadays.
This looked to me like it was due to the move to single-epoch training. But
single-epoch training was exactly what we were doing in this post! Perhaps I was
holding myself back by keeping dropout in place.

#### The learning rate

I don't have a good intuition as to what the right level is for this at the moment.
My code blindly uses the optimiser setup from the book:

```
    optimizer = torch.optim.AdamW(
        model.parameters(),
        lr=0.0004, weight_decay=0.1
    )
```

I have at best a vague understanding of how those work, at least when using an
optimiser (LR for simple gradient descent isn't too hard to understand, although it's
hard to work out an intuition for what the right value might be in any given case).
Additionally, in the Chinchilla paper, they talk about using a cosine
function to vary the learning rate, which is something I'm completely unfamiliar
with.

#### The precision

I gained about a day in training time by using AMP and the TF32 tensor cores; however,
I lost precision. I don't know for sure, but I suspect that the original weights
were trained with pure full-fat FP32. Perhaps reducing precision lost something? I know that
modern models are often trained with lower precisions, but perhaps that's balanced
out by something else?

#### The batch size

This is the one that I think it least likely, but it's worth mentioning. The post that
I linked to estimating the size of the training run for GPT-2 small mentioned that they
used a batch size of 512, which (of course) is completely impossible on consumer hardware
like mine. Indeed, I think you'd be lucky to get 512 onto a single 8-GPU node -- we're
talking serious cluster training scale here. Larger batches lead to more stable
updates to the gradients. So maybe that helped for OpenAI when they did their train? I suspect it did, but I'm pretty much
certain that it's not a large part of the difference.

(Counterpoint: Gemini thinks that this might actually be a big part of the problem!
It recommends using gradient accumulation -- that is, not stepping the optimiser every
iteration, but instead giving gradients time to build up -- as a way of getting
a larger batch effective batch size.)

#### Exploding gradients

While it doesn't look like we had any issues with these on the original FineWeb
and FineWeb-Edu trains, they definitely did kick in on the extended Edu train.
The code to clip them is easy enough, and I think it's likely that the original
GPT-2 trains would have had it. I doubt this was a major part of the difference,
but it probably would have helped, at least a bit.

---

Anyway, I think that's it in terms of differences that I can see between my train and OpenAI's
(as always, comments welcome -- let me know if you spot any others!),
so it's time to (finally) wrap this post up.

### Conclusion

At the start of this (ridiculously long) post, I asked the question: can we train
a GPT-2 style base model at home on a single RTX 3090. The answer is a resounding
"yes we can", which is great! Training base models: not just for the GPU-rich. If
you have a couple of days and a decent graphics card, you can train a Chinchilla-optimal GPT-2 pretty easily.

But the model itself isn't quite as good as the original GPT-2 small one, and I have some ideas
about why that might be. Testing any of those would take quite a long time,
given that each training run takes two days.

Now, my next planned step was to see whether I could work out how to move this up to
the cloud and train the same model on an 8x A100 or similar machine on Lambda Labs.
This still sounds like an excellent plan! With his `nanochat` project, Karpathy trains
a larger model on more tokens in four hours; if we could get the experiment time
down to one hour (plausible if training time is linear in both tokens and parameters)
then it would be much easier to check out those hypotheses above. [9](#fn-9)

So, I think that's still the right way to go: after training a base model at home
for free (if you ignore the electricity costs -- and it's cold enough in Lisbon
right now that the heat from the PC was probably saving me money on my home heating bill -- and the
cost of having bought the RTX 3090 in the first place),
the next step is to see how cheaply we can train it in the cloud.

Stay tuned :-)

---

1. It's useful here, but it does make me wonder how good FineWeb would be for training a base model
   with a longer context length, however. [↩](#fnref-1 "Jump back to footnote 1 in the text.")
2. There are ways to get comparable numbers even with a different tokeniser, using a
   bits-per-byte or nats-per-byte measure. Let's say we're using the normal
   cross entropy loss with the natural logarithm; that means that loss is expressed
   in nats. So you add up all of the per-token
   losses and divide it by the number of bytes across all of the inputs you've
   seen, and that would give you nats-per-byte. Likewise, if you used log2 for
   cross entropy, you'd get bits-per-byte. The latter is used in the Chinchilla
   paper (eg. table A5) as a way to compare their
   model with the Gopher model. I did consider digging into this a bit, but I
   think it's a bit of a side quest for now. [↩](#fnref-2 "Jump back to footnote 2 in the text.")
3. Those evals cost me $0.09 in API credits, which is actually a little more than
   I was expecting -- there were some responses which took quite a while to come back,
   though, and I believe that the GPT 5.1 model spends time thinking when it seems
   appropriate, so perhaps I spent a bit on thinking tokens. [↩](#fnref-3 "Jump back to footnote 3 in the text.")
4. Apart from a reference to a "dense layer", which I'm unsure about -- I believe
   it's the linear feed-forward layer after the attention calculations, though, as
   that doesn't appear elsewhere, and the calculation looks right. I also noticed
   that they don't have any terms in there for things like normalisation, which
   seems odd for such a carefully-worked-out formula; I assume they are small enough
   to vanish into the noise. [↩](#fnref-4 "Jump back to footnote 4 in the text.")
5. If you want a more careful calculation of the numbers -- and indeed a really nice
   explanation of some of the details of the Chinchilla paper, I recommend
   [this blog post from Tomek Korbak](https://tomekkorbak.com/2022/10/10/compute-optimal-gpt2/). [↩](#fnref-5 "Jump back to footnote 5 in the text.")
6. I hate that we appear to have settled on FLOPs with a lower-case "s" for "floating-point operations"
   when "FLOPS" (and equivalently MFLOPS, GFLOPS, TFLOPS) with an upper-case "S" already
   meant "floating-point operations per second" because the difference in capitalisation
   should really not change the units. But here we are. [↩](#fnref-6 "Jump back to footnote 6 in the text.")
7. I estimated the OpenAI weights loss on their own dataset by taking the perplexity
   number for the small model from figure 4, which is about 16.5, and then taking its
   natural log. [↩](#fnref-7 "Jump back to footnote 7 in the text.")
8. The authors of the paper call it "grokking", which is a great name, but is so
   overloaded in the context of LLMs (even if you disregard xAI's [Grok](https://x.ai/grok))
   that I'm slightly loath to use it here. This phenomenon also looks somewhat
   more limited in scope than I thought -- I'd been under the impression that it happens
   a lot with LLMs, but it looks like it's more a thing that happens with small models
   trained on very structured datasets. [↩](#fnref-8 "Jump back to footnote 8 in the text.")
9. It would also be interesting to see how easy it is to offload the optimiser to the CPU:
   in my old fine-tuning experiments I found that freed up a ton of VRAM, so we could benefit
   from that and maybe get the batch size up to something closer to the 512 that OpenAI
   apparently trained with. [↩](#fnref-9 "Jump back to footnote 9 in the text.")

[« Why smart instruction-following makes prompt injection easier](/2025/11/smart-instruction-following-and-prompt-injection)

Copyright (c) 2006-2025 by Giles Thomas.
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

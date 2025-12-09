---
source: hackernews
title: Deep dive on Nvidia circular funding
url: https://philippeoger.com/pages/deep-dive-into-nvidias-virtuous-cycle
date: 2025-12-09
---

* [ ]
  Dark mode

* [Home](/)
* [Github](https://github.com/philippe2803)
* [Linkedin](https://www.linkedin.com/in/philippeoger/)
* [Twitter](https://twitter.com/philippe_oger)

# NVIDIA frenemy relation with OpenAI and Oracle

I’ve spent the last 48 hours completely falling down the rabbit hole of
[NVIDIA’s Q3 Fiscal 2026 earnings report](https://nvidianews.nvidia.com/). If
you just skim the headlines, everything looks perfect: Revenue is up 62% to $57
billion, and Jensen Huang is talking about a "virtuous cycle of AI."

But I wanted to understand what was *really* happening under the hood, so I dug
into the balance sheet and cross-referenced it with all the news swirling
around OpenAI and Oracle. I’m not a professional Wall Street analyst, but even
just connecting the dots myself (with the help of Gemini), I’m seeing some cracks in the "AI Alliance."
While NVIDIA posts record numbers, it feels like their biggest customers are
quietly arming themselves for a breakout.

Here is my take on the hardware market, the "frenemy" dynamics between OpenAI
and NVIDIA, and the "circular financing" theories that everyone—including
Michael Burry, has been talking about.

Here is a quick summary of the points I'll discuss below:

* [NVIDIA’s Earnings: Perfection with a side of stress](#nvidias-earnings-perfection-with-a-side-of-stress)
* [Making Sense of the Round-Tripping News](#making-sense-of-the-round-tripping-news)
* [OpenAI making moves to reduce dependency on NVIDIA](#openai-making-moves-to-reduce-dependency-on-nvidia)
* [An interesting idea for Oracle: Groq acquisition](#an-interesting-idea-for-oracle-groq-acquisition)
* [Final Thoughts](#final-thoughts)

## NVIDIA’s Earnings: Perfection with a side of stress

On the surface, NVIDIA is the absolute monarch of the AI era. You can’t argue
with a Data Center segment that now makes up nearly 90% of the company's
business. However, when I looked closer at the financials, I found three
specific things that stood out to me as "red flags."

* **The Cash Flow Mystery:** NVIDIA reported a massive **$31.9 billion in Net
  Income**, but when I checked the cash flow statement, they only generated
  **$23.8 billion in Operating Cash Flow**. That is an $8 billion gap where
  profits aren't converting to cash immediately.
* **The Inventory Balloon:** I noticed that inventory has nearly doubled this
  year, hitting **$19.8 billion**. Management says this is to prep for the
  "Blackwell" launch, but holding ~120 days of inventory seems like a huge
  capital drag to me.
* **The "Paper" Chase:** I calculated their Days Sales Outstanding (DSO), and
  it has crept up to about **53 days**. As revenue skyrockets, NVIDIA is
  waiting nearly two months to get paid, which suggests they might be extending
  massive credit terms to enterprise clients to keep the flywheel spinning.

My personal read? NVIDIA is "burning the furniture" to build inventory, betting
everything that the [Blackwell architecture](https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/)
will sell out instantly in Q4.

## Making Sense of the Round-Tripping News

I want to be clear: I didn't discover this next part. It’s been all over the
financial news lately, and if you follow **Michael Burry** (the "Big Short"
guy), you’ve probably seen his tweets warning about "circular financing" and
[suspicious revenue recognition](https://www.investing.com/news/stock-market-news/michael-burry-warns-of-suspicious-revenue-recognition-after-nvidia-earnings-4369197).

I wanted to map it out for myself to see what the fuss was about. Burry shared
a chart recently that visualizes a "web" of deals, and it looks something like
this:

1. **Leg 1:** NVIDIA pledges billions (part of a widely reported $100B
   investment roadmap) to **OpenAI**.
2. **Leg 2:** OpenAI signs a massive **$300 billion** cloud contract with
   **Oracle** (Project Stargate) to host its models.
3. **Leg 3:** To fulfill that contract, Oracle turns around and places a **$40
   billion** order for NVIDIA’s GB200 GPUs.

Here is the Nano Banana Pro generation I just did for the visual people out there:

![NVIDIA-OpenAI-Oracle Circular Financing](https://philippeoger.com/static/img/circular-funding.png)

Burry’s argument, and the reason [regulators like the DOJ are reportedly looking
into this](https://m.economictimes.com/news/international/us/nvidia-rejects-circular-financing-claims-as-top-short-sellers-push-back/articleshow/125589622.cms)—is
that this mimics "Round-Tripping." It raises a tough question: If NVIDIA
stopped investing in OpenAI, would OpenAI still have the cash to sign that deal
with Oracle? And would Oracle still buy those chips? If the answer is "no,"
then some of that revenue might be more fragile than it looks.

## OpenAI making moves to reduce dependency on NVIDIA

The other big shift I’ve been tracking is OpenAI’s pivot. They used to be
NVIDIA’s star pupil, but now they look more like a future rival.
On one hand, they are hugging NVIDIA tight—deploying 10 gigawatts of infrastructure to train GPT-6. But on the
other, they seem to be building a supply chain to kill their dependency on
Jensen Huang.

The evidence is pretty loud if you look for it. "Project Stargate" isn't just a
data center; it's a huge infrastructure plan that includes custom hardware.
OpenAI made some news buying DRAM wafers directly from Samsung and SK Hynix (the 2 main HBM
world provider), bypassing NVIDIA’s supply chain, and many others, as reported
[here](https://openai.com/index/samsung-and-sk-join-stargate/),
[here](https://www.asiafinancial.com/samsung-sk-hynix-building-stargate-korea-using-open-ai), or
[here](https://www.kedglobal.com/artificial-intelligence/newsView/ked202510010013), and widely debated [on Hacker News here](https://news.ycombinator.com/item?id=46169224#46170844).

Plus, the talent migration is telling: OpenAI has poached
key silicon talent, including Richard Ho (Google’s former TPU
lead) back in 2023, and more recently many hardware engineers from Apple (around 40
apparently).

With the [Broadcom partnership](https://openai.com/index/openai-and-broadcom-announce-strategic-collaboration/),
my guess is OpenAI plans to use NVIDIA GPUs to *create* intelligence, but run that
intelligence on their own custom silicon to stop bleeding cash, or by betting on
Edge TPU-like chips for inference, similar to what Google does with its NPU chip.

The big question is, which money is Openai planning on using to fund this?
and how much influence does NVIDIA has over OpenAI’s future plans?

The $100 billions that NVIDIA is "investing" in OpenAI is not yet confirmed neither,
as reported [here](https://fortune.com/2025/12/02/nvidia-openai-deal-not-signed-yet-100-billion-rally-colette-kress/),

## An interesting idea for Oracle: Groq acquisition

Everyone is talking about **Inference** costs right now, basically, how
expensive it is to actually *run* ChatGPT or any other LLMs versus training it.
Now I'm looking at **Groq**, a startup claiming specifically to be faster and cheaper
than NVIDIA for this task. The founder is [Jonathan Ross](https://www.linkedin.com/in/ross-jonathan/),
a former Google TPU lead and literally the person that basically had the idea of TPU.

There is another layer to this that I think is getting overlooked as well: **The
HBM Shortage** created by Openai’s direct wafer purchases.

From what I understand, one of the biggest bottlenecks for NVIDIA right now is
HBM (High Bandwidth Memory), which is manufactured in specialized memory fabs
that are completely overwhelmed. However, Groq’s architecture relies on SRAM
(Static RAM). Since SRAM is typically built in logic fabs (like TSMC) alongside
the processors themselves, it theoretically shouldn't face the same supply
chain crunch as HBM.

Looking at all those pieces, I feel Oracle should seriously look into buying Groq.
Buying Groq wouldn't just give Oracle a faster chip, it could give them a chip that is
actually *available* when everything else is sold out. It’s a supply chain hedge.

It's also a massive edge for its main client, OpenAI, to get faster and cheaper inference.

Combine that with the fact that [Oracle’s margins on renting NVIDIA chips are
brutal](https://www.fool.com/investing/2025/12/02/michael-burry-just-sent-a-warning-to-artificial-in/), reportedly
as low as 14%, then the deal just makes sense. By owning Groq, Oracle could stop
paying the "NVIDIA Tax," fix their margins, and bypass the HBM shortage
entirely.

Groq currently has a valuation of around $6.9 billions, [according to its last funding round
in september 2025](https://groq.com/newsroom/groq-raises-750-million-as-inference-demand-surges).
Even with a premium, Oracle has financial firepower to make that acquisition happen.

**But would NVIDIA let that happen? and if the answer is no, then what does that tell us
about the circular funding in place? Is there a Quid pro quo where Nvidia agrees to invest
100 billions in OpenAI in exchange of Oracle being exclusive to Nvidia?**

## Final Thoughts

As we head into 2026, when looking at Nvidia, openai and Oracle dynamics, it looks like they are squeezing each
other balls. I do not know if Nvidia knew about the Openai deal about the wafer memory supply, or was there any collusion?
Does NVIDIA is fighting to maintain exclusivity for both training and inference at Stargate? What kind of chips is Openai
planning on building ? TPU/LPU like? Or more Edge TPU?

[Michael Burry is betting against the whole
thing](https://www.techradar.com/pro/security/could-the-ai-bubble-be-real-this-sage-of-the-2008-market-crash-the-central-character-of-the-big-short-certainly-thinks-so).

Me, I’m just a guy reading the reports, I have no way to speculate on this market. But I do know one thing: The AI hardware market
is hotter than ever, and the next few quarters are going to be fascinating to watch.

I have not discussed much about TPU from Google in this article, but I cover some thoughts [about the TPU vs GPU
in a previous post recently.](https://philippeoger.com/pages/why-googles-tpu-could-beat-nvidias-gpu-in-the-long-run).
It seems Google responded quickly to the current situation about the memory wafer shortage by securing a [major deal
with Samsung in 2026](https://www.kedglobal.com/korean-chipmakers/newsView/ked202512010003).

*Disclaimer: I say very smart things sometimes, but say stupid things a lot more. Take this in consideration when reading this blog post*

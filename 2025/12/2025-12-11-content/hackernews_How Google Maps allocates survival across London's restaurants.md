---
source: hackernews
title: How Google Maps allocates survival across London's restaurants
url: https://laurenleek.substack.com/p/how-google-maps-quietly-allocates
date: 2025-12-11
---

[![Lauren’s data Substack](https://substackcdn.com/image/fetch/$s_!-mTW!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F771b17d9-c7f7-4aeb-9ea9-bd733d998eb4_1024x1024.png)](/)

# [Lauren’s data Substack](/)

SubscribeSign in

# How Google Maps quietly allocates survival across London’s restaurants - and how I built a dashboard to see through it

### I wanted a dinner recommendation and got a research agenda instead. Using 13000+ restaurants, I rebuild its ratings with machine learning and map how algorithmic visibility actually distributes power.

[![Lauren Leek's avatar](https://substackcdn.com/image/fetch/$s_!Z2xe!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67c887ba-075f-42e8-8b57-4cc4ab4ae444_467x467.jpeg)](https://substack.com/%40lauren225268)

[Lauren Leek](https://substack.com/%40lauren225268)

Dec 09, 2025

96

36

24

Share

I needed a restaurant recommendation, so I did what every normal person would do: I scraped every single restaurant in Greater London and built a machine-learning model.

It started as a very reasonable problem. I was tired of doom-scrolling Google Maps, trying to disentangle genuinely good food from whatever the algorithm had decided to push at me that day. Somewhere along the way, the project stopped being about dinner and became about something slightly more unhinged: how digital platforms quietly redistribute economic survival across cities.

Because once you start looking at London’s restaurant scene through data, you stop seeing all those cute independents and hot new openings. You start seeing an algorithmic market - one where visibility compounds, demand snowballs, and who gets to survive is increasingly decided by code.

---

#### **Google Maps Is Not a Directory. It’s a Market Maker.**

The public story of Google Maps is that it passively reflects “what people like.” More stars, more reviews, better food. But that framing obscures how the platform actually operates. Google Maps is not just indexing demand - it is actively organising it through a ranking system built on a small number of core signals that Google itself has publicly acknowledged: relevance, distance, and prominence.

“Relevance” is inferred from text matching between your search query and business metadata. “Distance” is purely spatial. But “prominence” is where the political economy begins. Google defines prominence using signals such as review volume, review velocity, average rating, brand recognition, and broader web visibility. In other words, it is not just what people think of a place - it is how often people interact with it, talk about it, and already recognise it.

Visibility on these ranked lists determines foot traffic. Foot traffic determines how quickly reviews accumulate. Review accumulation then feeds directly back into the prominence signal. The system compounds. Early discovery generates demand. Demand generates data. Data generates future discovery. This creates a cumulative-advantage dynamic that looks remarkably similar to the way capital compounds in financial markets. This is essentially Robert Merton’s Matthew Effect applied to kebab shops - ‘unto every one that hath shall be given.’

This disproportionately rewards chains and already-central venues. Chains benefit from cross-location brand recognition. High-footfall areas generate reviews faster, meaning venues in those zones climb the prominence ranking more quickly even at identical underlying quality. By contrast, new independents face a classic cold-start problem: without reviews they are hard to find, and without being found they struggle to accumulate reviews at all. What looks like neutral consumer choice is therefore better understood as algorithmically mediated market design*.*

In economics, this dynamic closely resembles the logic of a market maker: an intermediary that does not merely reflect underlying supply and demand, but actively shapes liquidity, matching, and price discovery. Platforms like Google Maps perform an analogous function for local services by controlling visibility rather than prices directly. In the language of digital economics, ranking algorithms act as attention allocators, steering demand toward some firms and away from others.

---

#### **Building a Counterfactual City with Machine Learning**

If Google Maps now acts as a kind of market maker for urban demand, the obvious next question is: what would the city look like without that amplification layer? In other words, how do you separate a restaurant’s intrinsic performance from the visibility effects of the platform itself?

To get at that, I built a machine-learning model - a gradient-boosted decision tree (for the ML crowd: `HistGradientBoostingRegressor` from scikit-learn) - to predict what a restaurant’s Google rating *should* be, given only its structural characteristics. This class of model is designed for large, messy, mixed-type tabular data and is particularly good at capturing interaction effects, without me having to specify those by hand. Features include how many reviews it has (log-transformed to reflect diminishing returns to attention), what cuisine it serves, whether it is part of a chain or an independent, its price level, broad venue types (restaurant, café, takeaway, bar), and where it sits in the city via a spatial grid.

*Quick aside: for a subset of places I also scraped review text, languages, and photos. But for this first full-city run I stayed within the Google Maps API free tier - partly for reproducibility, partly because previous grid-scraping adventures taught me that cloud bills compound faster than review counts. So, for future versions, more features will only improve things. In particular, who* *is doing the reviewing matters. A five-star review of an Indian restaurant written in Hindi probably carries a different signal than one written by someone ordering chips with everything. (No judgment of white British people ofc…)*

One practical problem I ran into early on is that Google Maps is surprisingly bad at categorising cuisines. A huge share of restaurants are labelled vaguely (“restaurant”, “cafe”, “meal takeaway”), inconsistently, or just incorrectly. So I ended up building a separate cuisine-classification model that predicts cuisine from restaurant names, menu language, and review text where available. In other words, the cuisine filters in the dashboard are not just Google’s tags - they’re machine-learned. This matters more than it might sound: if you misclassify cuisines, you misread diversity, clustering, and who actually competes with whom on the high street. Btw, I briefly considered classifying Pret A Manger as French, just to see if it would make any French people angrier at me than they already are. I didn’t. But I thought about it.

Before any modelling happens, all features go through a standard preprocessing pipeline - imputation, encoding, the usual. Crucially, the model is trained only to learn the *mapping* between observable platform-visible features and ratings. This allows me to generate a counterfactual expected rating for each restaurant - what the platform would typically assign under those structural conditions. The difference between a restaurant’s real rating and this predicted rating is what I call the rating residual. A positive residual means the restaurant performs materially better than its platform baseline would suggest. A negative residual means it underperforms relative to what the algorithm normally rewards. This is not a perfect measure of food quality - but it *is* a powerful measure of algorithmic mispricing: where social or culinary value diverges from what the platform structurally amplifies.

One caveat: some restaurants pay for promoted pins or local-search ads. Because paid visibility isn’t publicly disclosed, I can’t estimate how many - which is itself a sign of how opaque platform influence has become. My residuals may partly reflect ad spend I can’t observe.

---

#### **Introducing: The London Food Dashboard**

To summarise this, I built the London food dashboard. The dashboard currently allows users to search by name and filter by underrated gems (identified by my machine learning algorithm), cuisine, borough, price level, min rating, and review volume. It is still very much a version-one prototype - but it is already a working microscope into London’s algorithmic food economy.

If you want to explore it yourself, you can find it on my personal website at: [laurenleek.eu/food-map](https://laurenleek.eu/food-map) .

[![](https://substackcdn.com/image/fetch/$s_!sPnJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65709628-04f8-4288-b0c0-7f933fab17c3_1893x753.png)](https://substackcdn.com/image/fetch/%24s_%21sPnJ%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/65709628-04f8-4288-b0c0-7f933fab17c3_1893x753.png)

Naturally, I immediately stress-tested it on my favourite part of London: Islington (maybe all this promo - also in my [previous Substack on UK segregation](https://laurenleek.substack.com/p/we-all-ended-up-in-islington-the) - makes me qualify for a council tax rebate? - I’m looking at you Jeremy Corbyn…). I switched on my “underrated gems” filter - that’s the ML residual at work - set a minimum rating and review count, exclude the eye-wateringly expensive options, and let the bubbles guide me. Bigger, darker bubbles mean places my model thinks the algorithm is undervaluing.

[![](https://substackcdn.com/image/fetch/$s_!z_mv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc32fc0dd-086c-4b12-bf16-d9bbce53ea3e_1918x1072.png)](https://substackcdn.com/image/fetch/%24s_%21z_mv%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/c32fc0dd-086c-4b12-bf16-d9bbce53ea3e_1918x1072.png)

And just like that, I had dinner plans. Do try it yourself.

*Btw, this is very much still a beta version - which means bugs, blind spots, and lots of room to grow. If something looks odd, missing, or wrong, please leave feature ideas and suggestions in the comments or via the comments on my website. Unlike the VS Code GitHub tracker and its 13.8k open issues, I really do read them.*

---

#### **From Individual Restaurants to Algorithmic Neighbourhoods**

But restaurants don’t fail alone - they fail in ecosystems. I also wanted to understand what happens when platform dynamics scale up from restaurants to entire neighbourhood food ecosystems. So I added a second modelling layer.

First, I aggregate restaurants into small spatial cells (the hexagons you see on the maps - because squares are for people who haven’t thought hard enough about edge effects) and compute summary features for each area: restaurant density, mean rating, mean residual, total reviews, chain share, cuisine entropy, and price level. I then standardise these and run principal component analysis (PCA) to compress them into a single continuous hub score that captures overall “restaurant ecosystem strength” in one dimension. Finally, I apply K-means clustering to the same feature space to classify areas into four structural types: *elite*, *strong*, *everyday*, and *weak* hubs.

[![](https://substackcdn.com/image/fetch/$s_!o6bX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fab16b9e2-5d2d-4046-9513-fe928ca833f5_1524x793.png)](https://substackcdn.com/image/fetch/%24s_%21o6bX%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/ab16b9e2-5d2d-4046-9513-fe928ca833f5_1524x793.png)

At first glance, the patterns look comfortingly familiar. Central London dominates. Of course it does. But what matters is not just where the hubs are - it’s what kind of hubs they are. Using the full hub score rather than raw ratings alone, I identify the five most structurally powerful restaurant hubs in London. They are the places where density, algorithmic attention, independent survival, and consumer spending power all line up at once. They are labeled on the maps. I am deliberately refusing to rank them loudly in prose in order to avoid starting neighbourhood wars at scale (and to not disappoint Islington) - but the visual story is already extremely clear.

Overlaying this with the cuisine density panels reveals something even sharper. London’s culinary diversity is not evenly distributed across its platform economy. Migrant cuisines cluster strongly in parts of the city where algorithmic visibility is structurally weaker. Italian, Indian, Turkish, Chinese, Thai, British, Japanese, French, American, and fish-and-chips all trace distinct settlement histories, labour networks, retail formats, and relationships to capital and rent. Some cuisines form long, contiguous corridors. Others appear as punctuated clusters tied to specific high streets or income brackets.

Cuisine diversity, in other words, is not just about taste. It is about where families settled, which high streets remained affordable long enough for a second generation to open businesses, and which parts of the city experienced displacement before culinary ecosystems could mature. (If this part especially speaks to you, I go much deeper into it in *[Food for thought: local restaurant diversity meets migration](https://laurenleek.substack.com/p/food-for-thought-local-restaurant)*).

[![](https://substackcdn.com/image/fetch/$s_!gpKm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b2aa425-fb4f-435e-ac4f-423e50a695dc_1811x802.png)](https://substackcdn.com/image/fetch/%24s_%21gpKm%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/0b2aa425-fb4f-435e-ac4f-423e50a695dc_1811x802.png)

---

**The Take-Away and Some Unwanted Policy Advice**

This project started as a search problem and ended as something more. The most important result isn’t which neighbourhood tops the rankings - it’s the realisation that platforms now quietly structure survival in everyday urban markets. London’s restaurant scene is no longer organised by taste alone. It is organised by visibility that compounds, rent that rises when discovery arrives, and algorithms that allocate attention long before consumers ever show up. What looks like “choice” is increasingly the downstream effect of ranking systems.

For policy, that shifts the frame. If discovery now shapes small-business survival, then competition, fairness, and urban regeneration can no longer ignore platform ranking systems. Councils can rebuild streets and liberalise licensing all they like - but algorithmic invisibility can still leave places economically stranded. Platform transparency and auditability are no longer niche tech debates; they are quietly becoming tools of local economic policy. At minimum, ranking algorithms with this much economic consequence should be auditable. We audit financial markets. We should audit attention markets too.

For a navigation app, Google maps has a remarkable amount of power.
Just saying.

---

*I’m also working on other maps (including a map of the best cycling and running routes with excellent cafés along the way, because I have needs). More broadly, I’m investing more and more time into building higher-quality public data projects. If you have an idea you’d like to see built - pitch it to me. And if you enjoy this kind of work, you can always [Buy Me A Coffee](https://buymeacoffee.com/laurenleek) or subscribe to help fund the next round of over-engineered maps.*

Thanks for reading Lauren’s data Substack! Subscribe for free to receive new posts and support my work.

Subscribe

[Share](https://laurenleek.substack.com/p/how-google-maps-quietly-allocates?utm_source=substack&utm_medium=email&utm_content=share&action=share)

96

36

24

Share

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Matthew Mullins's avatar](https://substackcdn.com/image/fetch/$s_!zKTW!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33748ec6-34a5-4213-89fd-c63096c84f41_3546x3546.jpeg)](https://substack.com/profile/104298358-matthew-mullins?utm_source=comment)

[Matthew Mullins](https://substack.com/profile/104298358-matthew-mullins?utm_source=substack-feed-item)

[1d](https://laurenleek.substack.com/p/how-google-maps-quietly-allocates/comment/185880562 "Dec 9, 2025, 2:16 PM")

Liked by Lauren Leek

I kinda hate that it didn't even occur to me how much algo bias there is in using Google Maps.

Expand full comment

ReplyShare

[![Sarah's avatar](https://substackcdn.com/image/fetch/$s_!pI5-!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d524446-8d7a-42e6-8413-7971cecee1bf_144x144.png)](https://substack.com/profile/314874592-sarah?utm_source=comment)

[Sarah](https://substack.com/profile/314874592-sarah?utm_source=substack-feed-item)

[1d](https://laurenleek.substack.com/p/how-google-maps-quietly-allocates/comment/185863708 "Dec 9, 2025, 1:25 PM")

Liked by Lauren Leek

Would love to hear more about hexagons & edge effects!

Expand full comment

ReplyShare

[34 more comments...](https://laurenleek.substack.com/p/how-google-maps-quietly-allocates/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Lauren · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

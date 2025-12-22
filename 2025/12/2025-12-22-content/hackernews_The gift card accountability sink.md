---
source: hackernews
title: The gift card accountability sink
url: https://www.bitsaboutmoney.com/archive/gift-card-accountability-sink/
date: 2025-12-22
---

[![Bits about Money](https://www.bitsaboutmoney.com/content/images/2022/12/bam-logo-long-transparent-bg.png)](https://www.bitsaboutmoney.com)

* [Home](https://www.bitsaboutmoney.com/)
* [Memberships](https://www.bitsaboutmoney.com/memberships/)
* [Archive](https://www.bitsaboutmoney.com/archive/)
* [Author](https://www.bitsaboutmoney.com/patio11/)

# The gift card accountability sink

[Patrick McKenzie (patio11)](/patio11/) •
Dec 19th, 2025

![The gift card accountability sink](/content/images/size/w2000/2025/12/gift-card-accountability-sink.png)

*Programming note*: *Merry Christmas*! *There will likely be another Bits about Money after the holiday but before New Year.*

*Bits about Money is* [*supported by our readers*](https://www.bitsaboutmoney.com/memberships/)*. If your education budget or business can underwrite the coming year of public goods in financial-infrastructure education, commentary, and policy analysis, please consider supporting it. I’m told this is particularly helpful for policymakers and others who cannot easily expense a subscription, and who benefit from all issues remaining publicly available with no paywall.*

The American Association of Retired People (AARP, an advocacy non-profit for older adults) has paid for ads on podcasts I listen to. The ad made a claim which felt raspberry-worthy (in service of an important public service announcement), which they [repeat in writing](https://www.aarp.org/money/scams-fraud/gift-card-payment/): *Asking to be paid by gift card is always a scam*.

Of course it isn’t. Gift cards are a payments rail, and an *enormous* business independently of being a payments rail. Hundreds of firms will indeed ask you to pay them on gift cards! They also exist, and are marketed, explicitly to do the thing that the AARP implicitly asserts no business or government entity will ever do: provide a method for transacting for people who do not have a banked method of transacting. [0]

Gift card scams are also enormous. The FBI’s Internet Crime Complaint Center received [$16.6 billion](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) in reports in 2024 across several payment methods; this is just for those consumers who bothered reporting it, in spite of the *extremely real* received wisdom that reporting is unlikely to improve one’s direct situation.

The flavor texts of scams vary wildly, but in substance they’ll attempt to convince someone, often someone socially vulnerable, to part with sometimes very large sums of money by buying gift cards and conveying card information (card number and PIN number, both printed on the card) to the scammer. The scammer will then use the [fraud supply chain](https://www.bitsaboutmoney.com/archive/the-fraud-supply-chain/), generally to swap the value on the card to another actor in return for value unconnected to the card. This can be delivered in many ways: cash, crypto, products and services in the scamming economy (such as purloined credit cards or even “lead lists” of vulnerable people to run more scams on), or [laundered](https://www.bitsaboutmoney.com/archive/kyc-and-aml-beyond-the-acronyms/) funds within regulated financial institutions which obscure the link between the crime and the funds (layering, in the parlance of AML professionals). A huge portion of [running a gift card marketplace](https://www.bitsaboutmoney.com/archive/gift-card-marketplaces/) is trying to prevent yourself from being exploited or made into an instrumentality in exploiting others.

It surprises many people to learn that the United States [aggressively defends customers from fraud over some payment methods](https://www.consumerfinance.gov/rules-policy/regulations/1005/), via a liability transfer to their financial institution, which transfers it to intermediaries, who largely transfer it to payment-accepting businesses. Many people think the U.S. can’t make large, effective, pro-consumer regulatory regimes. They are straightforwardly wrong… some of the time.

But the AARP, the FBI, and your friendly local payments nerd will all tell you that if you’re abused on your debit card you are quite likely to be made whole, and if you’re abused via purchasing gift cards, it is unlikely any deep pockets will cover for you. The difference in treatment is partially regulatory carveouts, partially organized political pressure, and partly a side effect of an accountability sink specific to the industrial organization of gift cards.

## Most businesses do not run their own gift card programs

There exists an ecosystem of gift card program managers, who are essentially financial services businesses with a sideline in software. (I should probably mention that I previously worked for and am currently an advisor to Stripe, whose self conception would not be precisely that, but which a) supports many ways for people to pay money for things and b) does not necessarily endorse what I say in my personal spaces.)

Why does the program manager exist? Why not simply have the retailer keep some internal database of who the retailer owes money to, updating this when someone buys or loads a gift card and when they spend the balance at the store? Because this implies many capabilities that retailers do not necessarily have, such as e.g. software development teams.

There is also a large regulatory component to running a gift card program, despite gift cards’ relatively lax regulatory drag (we’ll return to that in a moment). Card programs are regulated at both the federal and state levels. One frequent requirement in several states is escheatment. (Essentially all states have a requirement for escheatment; many but not all [exempt gift cards from it](https://www.alston.com/en/insights/publications/2024/11/developments-impacting-gift-card-programs).)

As [discussed previously](https://www.bitsaboutmoney.com/archive/more-than-you-want-to-know-about-gift-cards/) in Bits about Money, a major component of the gift card business model is abandonment (“breakage”). Consumer advocates felt this was unfair to consumers, bordering on fraudulent really. They convinced states to take the money that retailers were keeping for themselves. (Many states didn’t take all that much convincing.)

In theory, and sometimes even in practice, a consumer can convince a state treasurer’s office of unclaimed property (e.g. [Illinois](https://icash.illinoistreasurer.gov/)’) that the $24.37 that Target remitted as part of its quarterly escheatment payment for an unused gift card 13 years ago was actually theirs. A consumer who succeeds at this, which is neither easy nor particularly inexpensive to do, will receive a $24.37 check in the mail. The state keeps the interest income; call it a fee for service. It also keeps the interest income of the tens of billions of dollars of accumulated unclaimed property, which it generally promises to dutifully custody awaiting a legitimate claim for as long as the United States shall exist.

And so if you are a regional or national retailer who wants to offer gift cards, you have a choice. You can dedicate a team of internal lawyers and operations specialists to understanding both what the laws of the several states require with respect to gift cards, which are a *tiny* portion of your total operations, not merely today but as a result of the next legislative session in Honolulu, because you *absolutely must* order the software written to calculate the payment to remit accurately several quarters in advance of the legal requirement becoming effective. Or you can make the much more common choice, and outsource this to a specialist.

That specialist, the gift card program manager, will sell you a Solution™ which integrates across all the surfaces you need: your point-of-sale systems, your website, your accounting software, the 1-800 number and website for customers to check balances, ongoing escheatment calculation and remittance, cash flow management, *carefully titrated* amounts of attention to other legal obligations like AML compliance, etc. Two representative examples: Blackhawk Network and InComm Payments. You’ve likely never heard of them, even if you have their product on your person right now. Their *real* customer has the title Director of Payments at e.g. a Fortune 500 company.

And here begins the accountability sink: by standard practice and contract, when an unsophisticated customer is abused by being asked to buy a BigCo gift card, BigCo will say, truthfully and unhelpfully, that BigCo does not issue BigCo gift cards. It sells them. It accepts them. But it does not issue them. Your princess is in another castle.

BigCo may very well have a large, well-staffed fraud department. But, not due to any sort of malfeasance whatsoever, that fraud department may consider BigCo gift cards *entirely out of their own scope*. They *physically cannot access the database with the cards*. Their security teams, sensitive that gift card numbers are dangerous to keep lying around, very likely made it impossible for anyone at BigCo to reconstruct what happened to a particular gift card between checkout and most recent use. “Your privacy is important to us!” they will say, and they are not cynically invoking it in this case.

## Gift cards are not regulated like other electronic payments instruments

As mentioned above, Regulation E is the primary driver for the private enforcement edifice that makes scarily smart professionals (and their attached balance sheets) swing into action on behalf of consumers. Reg E has a carveout for certain prepaid payments. Per [most recent guidance](https://files.consumerfinance.gov/f/documents/cfpb_prepaid_small-entity-compliance-guide.pdf), that includes prepaid gift cards, gift certificates, and similar.

And so, if you call your bank and say, “I was defrauded! Someone called me and pretended to be the IRS, and I read them my debit card number, and now I’ve lost money,” the state machine obligates the financial institution to have the customer service representative click a very prominent button on their interface. This will restore your funds very quickly and have some side effects you probably care about much less keenly. One of those is an “investigation,” which is not really an investigation in the commanding majority of cases.

And if you call the program manager and say, “I was defrauded! Someone called me and pretended to be the IRS, and I read them a gift card number, and now I’ve lost money,” there is… no state machine. There is no legal requirement to respond with alacrity, no statutorily imposed deadline, no button for a CS rep to push, and no investigation to launch. You will likely be told by a low-paid employee that this is unfortunate and that you should file a police report. The dominant reason for this is that suggesting a concrete action to you gets you off the phone faster, and the call center aggressively minimizes time to resolution of calls and recidivism, where you call back because your problem is not solved. Filing a police report will, in most cases, not restore your money—but if it causes you not to call the 1-800 number again, then from the card program manager’s perspective *this issue has been closed successfully*.

## Why do we choose this difference in regulation?

The people of the United States, through their elected representatives and the civil servants who labor on their behalf, intentionally exempt gift cards from the Reg E regime in the interest of facilitating commerce.

It is the ordinary and appropriate work of a democracy to include input from citizens in the rulemaking process. The Retail Industry Leaders Association [participated](https://rilastagemedia.blob.core.windows.net/rila-web/rila.web/media/media/pdfs/regulatory%20letters/fincen/2011/comments-on-prepaid-access-regulations.pdf?ext=.pdf), explaining to FinCEN that it would be quite burdensome for retailers to fall into KYC scope, etc etc. Many other lobbyists and industry associations made directionally similar comments.

The Financial Crimes Enforcement Network, for example, has an [explicit carveout](https://www.fincen.gov/news/news-releases/fincen-issues-prepaid-access-final-rule) in its regulations: while FinCEN will [aggressively police rogue bodegas](https://www.bitsaboutmoney.com/archive/debanking-and-debunking/), it has no interest in you if you sell closed-loop gift cards of less than $2,000 face value. This is explicitly to balance the state’s interest in law enforcement against, quote, preserving innovation and the many legitimate uses and societal benefits offered by prepaid access, endquote.

FinCEN’s rules clarify that higher-value activity—such as selling more than $10,000 in gift cards to a single individual in a day—brings sellers back into scope. Given the relatively lax enforcement environment for selling a $500 gift card, you very likely might not build out systems which will successfully track customer identities and determine that the same customer has purchased twenty-one $500 gift cards in three transactions. That likely doesn’t rate as a hugely important priority for Q3.

And so the fraud supply chain comes to learn which firms haven’t done that investment, and preferentially suggests those gift cards to their launderers, [mules](https://theintercept.com/2018/11/23/bitcoin-gift-card-trading-scams/), [brick movers](https://globalchinapulse.net/moving-bricks-money-laundering-practices-in-the-online-scam-industry/), and scam victims.

And that’s why the AARP tells fibs about gift cards: we have, with largely positive intentions and for good reasons, exposed them to less regulation than most formal payment systems in the United States received. That decision has a cost. Grandma sometimes pays it.

[0] Indeed, there are entire companies which exist to turn gift cards into an alternate financial services platform, explicitly to give unbanked and underbanked customers a payments rail. [Paysafe](https://www.paysafe.com/us-en/), for example, is a publicly traded company with thousands of employees, the constellation of regulatory supervision you’d expect, and a subsidiary [Openbucks](https://www.openbucks.com/company/about/) which is designed to give businesses the ability to embed Pay Us With A Cash Voucher in their websites/invoices/telephone collection workflows. This is exactly the behavior that “never happens from a legitimate business” except when it does by the tens of billions of dollars.

As Bits about Money has frequently observed, people who write professionally about money—including professional advocates for financially vulnerable populations—often misunderstand alternative financial services, largely because those services are designed to serve a social class that professionals themselves do not belong to, rarely interact with directly, and do not habitually ask how they pay rent, utilities, or phone bills.

[Perpetual futures, explained →](/archive/perpetual-futures-explained/)

## Want more essays in your inbox?

I write about the intersection of tech and finance, approximately biweekly. It's free.

Get a biweekly email

**Great!** Check your inbox (or spam folder) for an email containing a magic link.

Sorry, something went wrong. Please try again.

* [Data & privacy](https://www.bitsaboutmoney.com/privacy/)
* [Contact](https://www.kalzumeus.com/standing-invitation/)

© 2021-2025 [Kalzumeus Software, LLC](https://www.kalzumeus.com)

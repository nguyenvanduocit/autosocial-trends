---
source: hackernews
title: Bag of words, have mercy on us
url: https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us
date: 2025-12-08
---

[![Experimental History](https://substackcdn.com/image/fetch/$s_!VtWA!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1a1b3b4-5f35-4876-a0d5-449398201e1f_1171x1171.png)](/)

# [Experimental History](/)

SubscribeSign in

# Bag of words, have mercy on us

### OR: Claude will you go to prom with me?

[![Adam Mastroianni's avatar](https://substackcdn.com/image/fetch/$s_!5WuG!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F5cfa0b33-de32-41f5-b53a-9b7f33c7f68f_1832x1171.jpeg)](https://substack.com/%40experimentalhistory)

[Adam Mastroianni](https://substack.com/%40experimentalhistory)

Aug 05, 2025

544

94

115

Share

Article voiceover

0:00

-19:25

Audio playback is not supported on your browser. Please upgrade.

[![](https://substackcdn.com/image/fetch/$s_!w4qD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31f48971-998b-4e9b-8f90-ac67d4336c39_1215x1654.jpeg)](https://substackcdn.com/image/fetch/%24s_%21w4qD%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/31f48971-998b-4e9b-8f90-ac67d4336c39_1215x1654.jpeg)

photo cred: my dad

Look, I don’t know if AI is gonna kill us or make us all rich or whatever, but I do know we’ve got the wrong metaphor.

We want to understand these things as *people*. When you type a question to ChatGPT and it types back the answer in complete sentences, it feels like there must be a little guy in there doing the typing. We get this vivid sense of “*it’s alive!!*”, and we activate all of the mental faculties we evolved to deal with fellow humans: [theory of mind](https://en.wikipedia.org/wiki/Theory_of_mind), [attribution](https://en.wikipedia.org/wiki/Attribution_%28psychology%29), [impression management](https://en.wikipedia.org/wiki/Impression_management#:~:text=Impression%20management%20is%20a%20conscious,controlling%20information%20in%20social%20interaction.), [stereotyping](https://en.wikipedia.org/wiki/Stereotype), [cheater detection](https://www.cep.ucsb.edu/wp-content/uploads/2023/05/Cosmides_etal_2005_DetectingCheaters.pdf), etc.

We can’t help it; humans are hopeless anthropomorphizers. When it comes to perceiving personhood, we’re so trigger-happy that we can see the Virgin Mary in a [grilled cheese sandwich](https://www.nbcnews.com/id/wbna6511148):

[![](https://substackcdn.com/image/fetch/$s_!B9zG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d375800-59a9-4bc2-8daa-5ec2a2c142d7_378x490.png)](https://substackcdn.com/image/fetch/%24s_%21B9zG%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/4d375800-59a9-4bc2-8daa-5ec2a2c142d7_378x490.png)

A human face in a slice of nematode:

[![](https://substackcdn.com/image/fetch/$s_!wvjL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F631e5c59-2ef1-43f4-8786-dcd15a24ba44_2560x1727.png)](https://substackcdn.com/image/fetch/%24s_%21wvjL%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/631e5c59-2ef1-43f4-8786-dcd15a24ba44_2560x1727.png)

credit: [Massimo Brizzi](https://commons.wikimedia.org/wiki/File%3AAscaris_female_200x_section.jpg)

And an old man in a bunch of poultry and fish atop a pile of books:

[![](https://substackcdn.com/image/fetch/$s_!Yf45!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F300b3090-3f16-47da-b24e-bff67d7a0df7_861x1100.png)](https://substackcdn.com/image/fetch/%24s_%21Yf45%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/300b3090-3f16-47da-b24e-bff67d7a0df7_861x1100.png)

Giuseppe Arcimboldo, *[The Jurist](https://commons.wikimedia.org/wiki/File%3AGiuseppe_Arcimboldo_-_The_Jurist_-_WGA00837.jpg)* (1566)

Apparently, this served us well in our evolutionary history—maybe it’s so important not to mistake *people* for *things* that we err on the side of mistaking *things* for *people*.[1](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-1-169990157) This is probably why we’re so willing to explain strange occurrences by appealing to fantastical creatures with minds and intentions: everybody in town is getting sick because of WITCHES, you can’t see the sun right now because A WOLF ATE IT, the volcano erupted because GOD IS MAD. People who experience sleep paralysis sometimes [hallucinate](https://en.wikipedia.org/wiki/Sleep_paralysis) a demon-like creature sitting on their chest, and one explanation is that the subconscious mind is trying to understand why the body can’t move, and instead of coming up with “I’m still in REM sleep so there’s not enough acetylcholine in my brain to activate my primary motor cortex”, it comes up with “BIG DEMON ON TOP OF ME”.

[![](https://substackcdn.com/image/fetch/$s_!UnKD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67413903-10f6-40d5-b355-abee7b10c21a_1920x1538.png)](https://substackcdn.com/image/fetch/%24s_%21UnKD%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A//substack-post-media.s3.amazonaws.com/public/images/67413903-10f6-40d5-b355-abee7b10c21a_1920x1538.png)

[Henry Fuseli,](https://en.wikipedia.org/wiki/Sleep_paralysis#/media/File:Henry_Fuseli_(1741%E2%80%931825),_The_Nightmare,_1781.jpg) *[The Nightmare](https://en.wikipedia.org/wiki/Sleep_paralysis#/media/File:Henry_Fuseli_(1741%E2%80%931825),_The_Nightmare,_1781.jpg)* [(1781)](https://en.wikipedia.org/wiki/Sleep_paralysis#/media/File:Henry_Fuseli_(1741%E2%80%931825),_The_Nightmare,_1781.jpg). Credit: [Tulip Hysteria](https://www.flickr.com/photos/36417567%40N03/32380012237/)

This is why the past three years have been so confusing—the little guy inside the AI keeps dumbfounding us by doing things that a human wouldn’t do. Why does he make up citations when he does my social studies homework? How come he can beat me at Go but he can’t tell me [how many “r”s are in the word “strawberry”](https://community.openai.com/t/incorrect-count-of-r-characters-in-the-word-strawberry/829618)? Why is he telling me to [put glue on my pizza](https://www.theverge.com/2024/5/23/24162896/google-ai-overview-hallucinations-glue-in-pizza)?[2](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-2-169990157)

Trying to understand LLMs by using the rules of human psychology is like trying to understand a game of Scrabble by using the rules of Pictionary. These things don’t act like people because they *aren’t* people. I don’t mean that in the deflationary way that the AI naysayers mean it. They think denying humanity to the machines is a well-deserved insult; I think it’s just an accurate description.[3](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-3-169990157) As long we try to apply our person perception to artificial intelligence, we’ll keep being surprised and befuddled.

We are in dire need of a better metaphor. Here’s my suggestion: instead of seeing AI as a sort of silicon homunculus, we should see it as a *bag of words.*

# **WHAT’S IN THE BAG**

An AI is a bag that contains basically all words ever written, at least the ones that could be scraped off the internet or scanned out of a book. When users send words into the bag, it sends back the most relevant words it has. There are so many words in the bag that the most relevant ones are often correct and helpful, and AI companies secretly add [invisible words](https://promptengineering.org/system-prompts-in-large-language-models/) to your queries to make this even more likely.

This is an oversimplification, of course. But it’s also surprisingly handy. For example, AIs will routinely give you outright lies or hallucinations, and when you’re like “Uhh hey that was a lie”, they will immediately respond “Oh my god I’m SO SORRY!! I promise I’ll never ever do that again!! I’m turning over a new leaf right now, nothing but true statements from here on” and then they will literally lie to you in the next sentence. This would be baffling and exasperating behavior coming from a human, but it’s very normal behavior coming from a bag of words. If you toss a question into the bag and the right answer happens to be in there, that’s probably what you’ll get. If it’s not in there, you’ll get some related-but-inaccurate bolus of sentences. When you accuse it of lying, it’s going to produce lots of words from the “I’ve been accused of lying” part of the bag. Calling this behavior “malicious” or “erratic” is misleading because it’s not *behavior* at all, just like it’s not “behavior” when a calculator multiplies numbers for you.

“Bag of words” is a also a useful heuristic for predicting where an AI will do well and where it will fail. “Give me a list of the ten worst transportation disasters in North America” is an easy task for a bag of words, because disasters are well-documented. On the other hand, “Who reassigned the species *Brachiosaurus brancai* to its own genus, and when?” is a [hard task](https://svpow.com/2025/02/14/if-you-believe-in-artificial-intelligence-take-five-minutes-to-ask-it-about-stuff-you-know-well/) for a bag of words, because the bag just doesn’t contain that many words on the topic.[4](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-4-169990157) And a question like “What are the most important lessons for life?” won’t give you anything outright false, but it will give you a bunch of fake-deep pablum, because most of the text humans have produced on that topic is, no offense, fake-deep pablum.

When you forget that an AI is just a big bag of words, you can easily slip into acting like it’s an all-seeing glob of pure intelligence. For example, I was hanging with a group recently where one guy made everybody watch a video of some close-up magic, and after the magician made some coins disappear, he exclaimed, “I asked ChatGPT how this trick works, and even *it* didn’t know!” as if this somehow made the magic extra magical. In this person’s model of the world, we are all like shtetl-dwelling peasants and AI is like our Rabbi Hillel, the only learned man for 100 miles. If Hillel can’t understand it, then it must be truly profound!

If that guy had instead seen ChatGPT as a bag of words, he would have realized that the bag probably doesn’t contain lots of detailed descriptions of contemporary coin tricks. After all, magicians make money from performing and selling their tricks, not writing about them at length on the internet. Plus, magic tricks are hard to describe—“He had three quarters in his hand and then it was two pennies!”—so you’re going to have a hard time prompting the right words out of the bag. The coin trick is not literally magic, and neither is the bag of words.

# **GALILEO GPT**

The “bag of words” metaphor can also help us guess what these things are gonna do next. If you want to know whether AI will get better at something in the future, just ask: “can you fill the bag with it?” For instance, people are kicking around the idea that [AI will replace human scientists](https://aeon.co/essays/when-ais-do-science-it-will-be-strange-and-incomprehensible). Well, if you want your bag of words to do science for you, you need to stuff it with lots of science. Can we do that?

When it comes to specific scientific tasks, yes, we already can. If you fill the bag with data from 170,000 proteins, for example, it’ll do a [pretty good job](https://en.wikipedia.org/wiki/AlphaFold) predicting how proteins will fold. Fill the bag with chemical reactions and [it can tell you how to synthesize new molecules](https://platform.futurehouse.org/). Fill the bag with journal articles and then describe an experiment and [it can tell you whether anyone has already scooped you](https://scite.ai/).

All of that is cool, and I expect more of it in the future. I don’t think we’re far from a bag of words being able to do an entire low-quality research project from beginning to end—coming up with a hypothesis, designing the study, running it, analyzing the results, writing them up, making the graphs, arranging it all on a poster, all at the click of a button—because we’ve got loads of low-quality science to put in the bag. If you walk up and down the poster sessions at a psychology conference, you can see lots of first-year PhD students presenting studies where they seemingly pick some semi-related constructs at random, correlate them, and print out a p-value (“Does self-efficacy moderate the relationship between social dominance orientation and system-justifying beliefs?”). A bag of words can [basically do this already](https://personalitymap.io/); you just need to give it access to an online participant pool and a big printer.[5](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-5-169990157)

But [science is a strong-link problem](https://www.experimental-history.com/p/science-is-a-strong-link-problem); if we produced a million times more crappy science, we’d be right where we are now. If we want more of the good stuff, what should we put in the bag? You could stuff the bag with papers, but some of them are fraudulent, some are merely mistaken, and all of them contain unstated assumptions that could turn out to be false. And they’re usually missing key information—they don’t share the data, or they don’t describe their methods in adequate detail. Markus Strasser, an entrepreneur who tried to start one of those companies that’s like “we’ll put every scientific paper in the bag and then ??? and then profit”, [eventually abandoned the effort](https://markusstrasser.org/extracting-knowledge-from-literature.html), saying that “close to nothing of what makes science actually work is published as text on the web.”[6](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-6-169990157)

Here’s one way to think about it: if there had been enough text to train an LLM in 1600, would it have scooped Galileo? My guess is no. Ask that early modern ChatGPT whether the Earth moves and it will helpfully tell you that experts have considered the possibility and [ruled it out](https://classicalliberalarts.com/resources/PTOLEMY_ALMAGEST_ENGLISH.pdf). And that’s by design. If it had started claiming that our planet is zooming through space at 67,000mph, its dutiful human trainers would have punished it: “Bad computer!! Stop hallucinating!!”

In fact, an early 1600s bag of words wouldn’t just have the right words in the wrong order. At the time, the right words didn’t *exist*. As the historian of science David Wootton [points out](https://archive.org/details/inventionofscien0000woot)[7](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-7-169990157), when Galileo was trying to describe his discovery of the moons of Jupiter, none of the languages he knew had a good word for “discover”. He had to use awkward circumlocutions like “I saw something unknown to all previous astronomers before me”. The concept of learning new truths by looking through a glass tube would have been totally foreign to an LLM of the early 1600s, as it was to most of the *people* of the early 1600s, with a few notable exceptions.

You would get better scientific descriptions from a 2025 bag of words than you would from a 1600 bag of words. But both bags might be equally bad at producing the scientific ideas of their respective futures. Scientific breakthroughs often require doing things that are [irrational and unreasonable for the standards of the time](https://www.experimental-history.com/p/the-anarchist-and-the-hockey-stick?utm_source=publication-search) and good ideas [usually](https://www.experimental-history.com/p/the-anarchist-and-the-hockey-stick) [look](https://www.experimental-history.com/p/three-dumb-studies-for-your-consideration) [stupid](https://nintil.com/discoveries-ignored/) when they first arrive, so they are often—with good reason!—rejected, dismissed, and ignored. This is a big problem for a bag of words that contains all of *yesterday’s* good ideas. Putting new ideas in the bag will often make the bag worse, on average, because most of those new ideas will be wrong. That’s why revolutionary research requires not only intelligence, but also [stupidity](https://slimemoldtimemold.com/2022/02/10/the-scientific-virtues/). I expect humans to remain usefully stupider than bags of words for the foreseeable future.

Subscribe

# **CLAUDE WILL U GO TO PROM WITH ME?**

The most important part of the “bag of words” metaphor is that it prevents us from thinking about AI in terms of *social status*. Our ancestors had to play status games well enough to survive and reproduce—losers, by and large, don’t get to pass on their genes. This has left our species exquisitely attuned to who’s up and who’s down. Accordingly, we can turn anything into a competition: [cheese rolling](https://en.wikipedia.org/wiki/Cooper%27s_Hill_Cheese-Rolling_and_Wake), [nettle eating](https://www.bbc.com/news/articles/cd111ej6vd0o), [phone throwing](https://en.wikipedia.org/wiki/Mobile_phone_throwing), [toe wrestling](https://www.theguardian.com/lifeandstyle/2023/jul/07/experience-im-the-toe-wrestling-world-champion), and [ferret legging](https://www.youtube.com/watch?v=KPQ6TuvqX7w&t=131s), where male contestants, sans underwear, put live ferrets in their pants for as long as they can. (The world record is [five hours and thirty minutes](https://www.topendsports.com/sport/unusual/ferret-legging.htm).)

When we personify AI, we mistakenly make it a competitor in our status games. That’s why we’ve been arguing about artificial intelligence like it’s a new kid in school: is she cool? Is she smart? Does she have a crush on me? The better AIs have gotten, the more status-anxious we’ve become. If these things are like people, then we gotta know: are we *better* or *worse* than them? Will they be our masters, our rivals, or our slaves? Is their art finer, their short stories tighter, their insights sharper than ours? If so, there’s only one logical end: ultimately, we must either kill them or worship them.

But a bag of words is not a spouse, a sage, a sovereign, or a serf. It’s a tool. Its purpose is to automate our drudgeries and amplify our abilities. Its social status is NA; it makes no sense to ask whether it’s “better” than us. The real question is: does using it make *us* better?

That’s why I’m not afraid of being rendered obsolete by a bag of words. Machines have already matched or surpassed humans on all sorts of tasks. A pitching machine can throw a ball faster than a human can, spellcheck gets the letters right every time, and autotune never sings off key. But we don’t go to baseball games, spelling bees, and Taylor Swift concerts for the speed of the balls, the accuracy of the spelling, or the pureness of the pitch. We go because we care about humans doing those things. It wouldn’t be interesting to watch a bag of words do them—unless we mistakenly start treating that bag like it’s a person.

(That’s also why I see no point in using AI to, say, write an essay, just like I see no point in bringing a forklift to the gym. Sure, it can lift the weights, but I’m not trying to suspend a barbell above the floor for the hell of it. I lift it because I want to become the kind of *person* who can lift it. Similarly, I write because I want to become the kind of person who can think.)

But that doesn’t mean I’m unafraid of AI entirely. I’m plenty afraid! Any tool can be dangerous when used the wrong way—nail guns and nuclear reactors can kill people just fine without having a mind inside them. In fact, the “bag of words” metaphor makes it clear that AI can be dangerous *precisely because* it doesn’t operate like humans do. The dangers we face from humans are scary but familiar: hotheaded humans might kick you in the head, reckless humans might drink and drive, duplicitous humans might pretend to be your friend so they can steal your identity. We can guard against these humans because we know how they operate. But we don’t know what’s gonna come out of the bag of words. For instance, if you show humans computer code that has security vulnerabilities, they do not suddenly start praising Hitler. But [LLMs do](https://martins1612.github.io/emergent_misalignment_betley.pdf).[8](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-8-169990157) So yes, I would worry about putting the nuclear codes in the bag.[9](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-9-169990157)

# **C’MON BERTIE**

Anyone who has owned an old car has been tempted to interpret its various malfunctions as part of its *temperament*. When it won’t start on a cold day, it feels like the appropriate response is to plead, the same way you would with a sleepy toddler or a tardy partner:“C’mon Bertie, we gotta get to the dentist!” But ultimately, person perception is a poor guide to vehicle maintenance. Cars are made out of metal and plastic that turn gasoline into forward motion; they are not made out of bones and meat that turn Twinkies into thinking. If you want to fix a broken car, you need a wrench, a screwdriver, and a blueprint, not a cognitive-behavioral therapy manual.

Similarly, anyone who sees a mind inside the bag of words has fallen for a trick. They’ve had their evolution exploited. Their social faculties are firing not because there’s a human in front of them, but because natural selection gave those faculties a hair trigger. For all of human history, something that talked like a human and walked like a human was, in fact, a human. Soon enough, something that talks and walks like a human may, in fact, be a very sophisticated logistic regression. If we allow ourselves to be seduced by the superficial similarity, we’ll end up like the moths who evolved to navigate by the light of the moon, only to find themselves drawn to—and ultimately electrocuted by—the mysterious glow of a bug zapper.

Unlike moths, however, we aren’t stuck using the instincts that natural selection gave us. We can *choose* the schemas we use to think about technology. We’ve done it before: we don’t refer to a backhoe as an “artificial digging guy” or a crane as an “artificial tall guy”. We don’t think of books as an “artificial version of someone talking to you”, photographs as “artificial visual memories”, or listening to recorded sound as “attending an artificial recital”. When pocket calculators debuted, they were already smarter than every human on Earth, at least when it comes to calculation—a job that itself [used to be done by humans](https://en.wikipedia.org/wiki/Computer_%28occupation%29). Folks [wondered](https://www.nytimes.com/1972/08/20/archives/handheld-calculators-tool-or-toy.html?searchResultPosition=4) whether this new technology was “a tool or a toy”, but nobody seems to have wondered whether it was a *person*.

(If you covered a backhoe with skin, made its bucket look like a hand, painted eyes on its chassis, and made it play a sound like “hnngghhh!” whenever it lifted something heavy, *then* we’d start wondering whether there’s a ghost inside the machine. That wouldn’t tell us anything about backhoes, but it would tell us a lot about our own psychology.)

The original sin of artificial intelligence was, of course, calling it artificial intelligence. Those two words have lured us into making man the measure of machine: “Now it’s as smart as an undergraduate...now it’s as smart as a PhD!” These comparisons only give us the *illusion* of understanding AI’s capabilities and limitations, as well as our own, because we don’t actually know what it means to be smart in the first place. Our definitions of intelligence are either [wrong](https://www.experimental-history.com/p/why-arent-smart-people-happier) (“Intelligence is the ability to solve problems”) or tautological (“Intelligence is the ability to do things that require intelligence”).[10](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-10-169990157)

It’s unfortunate that the computer scientists figured out how to make something that kinda looks like intelligence before the psychologists could actually figure out what intelligence is, but here we are. There’s no putting the cat back in the bag now. It won’t fit—there’s too many words in there.

*Experimental History* is covered with skin and going hnnnngh

Subscribe

---

PS it’s been a busy week on Substack—

[Derek Thompson](https://open.substack.com/users/157561-derek-thompson?utm_source=mentions)

 and I discussed why people get so anxious about conversations, and how to have better ones:

[![](https://substackcdn.com/image/fetch/$s_!uPIO!,w_56,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38b0f850-caa7-417a-bc0b-5b7224dd1f25_888x888.png)Derek Thompson

Why Are Americans So Scared of Talking to Each Other?

Americans are more alone than ever. Face-to-face socializing has plummeted this century, especially for young people. Nobody parties anymore. We spend more time in our homes than any period on record. The graphical evidence is dire…

Listen now

4 months ago · 155 likes · 15 comments · Derek Thompson and Adam Mastroianni](https://www.derekthompson.org/p/why-are-we-so-afraid-of-talking-to?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

And

[Chris Dalla Riva](https://open.substack.com/users/122589177-chris-dalla-riva?utm_source=mentions)

 at [Can't Get Much Higher](https://open.substack.com/pub/chrisdallariva) answered all of my questions about music. He uncovered some surprising stuff, including an issue that caused a civil war on a Beatles message board, and whether they really sang naughty words on the radio in the 1970s:

[![](https://substackcdn.com/image/fetch/$s_!7dPV!,w_56,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff001ad4d-db0c-469b-bdb0-c47c1d76e400_1280x1280.png)Can't Get Much Higher

What are the Weirdest Lyrics in a Hit Song? Mailbag

If you enjoy this newsletter, consider ordering a copy of my debut book, Uncharted Territory: What Numbers Tell Us about the Biggest Hit Songs and Ourselves. It’s a data-driven history of popular music covering 1958 to 2025…

Read more

4 months ago · 52 likes · 21 comments · Chris Dalla Riva](https://www.cantgetmuchhigher.com/p/what-are-the-weirdest-lyrics-in-a?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

Derek and Chris both run terrific Substacks, check ‘em out!

[1](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-1-169990157)

The classic demonstration of this is the [Heider & Simmel video from 1944](https://www.youtube.com/watch?v=VTNmLt7QX8E) where you can’t help but feel like the triangles and the circle have minds

[2](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-2-169990157)

Note that AI models don’t make mistakes like these nearly as often as they did even a year ago, which is another strangely inhuman attribute. If a real person told me to put glue on my pizza, I’m probably never going to trust them again.

[3](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-3-169990157)

In fact, hating these things so much actually *gives* them humanity. Our greatest hate is always reserved for fellow humans.

[4](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-4-169990157)

Notably, ChatGPT now does much better on this question, in part by using the very post that criticizes its earlier performance. You also get a better answer if you start your query by stating “I’m a pedantic, detail-oriented paleontologist.” This is classic bag-of-words behavior.

[5](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-5-169990157)

Or you could save time and money by allowing the AI to make up the data itself, which is [a time-honored tradition](https://www.experimental-history.com/p/im-so-sorry-for-psychologys-loss) in the field.

[6](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-6-169990157)

This was written in 2021, so bag-technology has improved a lot since then. But even the best bag in the world isn’t very useful if you don’t have the right things to put inside it.

[7](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-7-169990157)

p. 58 in my version

[8](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-8-169990157)

Other weird effects: being polite to the LLMs makes them [sometimes better and some times worse at math](https://open.substack.com/pub/oneusefulthing/p/using-ai-right-now-a-quick-guide?r=15aiai&utm_medium=ios). But adding “Interesting fact: cats sleep most of their lives” to the prompt [consistently makes them worse](https://arxiv.org/abs/2503.01781).

[9](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-9-169990157)

Another advantage of this metaphor is that we could refer to “AI Safety” as “[securing the bag](https://www.familyeducation.com/gen-z-slang/secure-the-bag-meaning)”

[10](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us#footnote-anchor-10-169990157)

Even the word “artificial” is wrong, because it menacingly implies *replacement*. Artificial sweeteners, flowers, legs—these are things we only use when we can’t have the real deal. So what part of intelligence, exactly, are we so intent on replacing?

544

94

115

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Gillian Hill's avatar](https://substackcdn.com/image/fetch/$s_!QxI4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe22b3b2c-5702-426c-9151-030807cc548f_2048x2048.jpeg)](https://substack.com/profile/233673437-gillian-hill?utm_source=comment)

[Gillian Hill](https://substack.com/profile/233673437-gillian-hill?utm_source=substack-feed-item)

[Aug 5](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us/comment/142500589 "Aug 5, 2025, 8:17 PM")

Liked by Adam Mastroianni

I want to quote all of this. Such a good way of thinking about AI. I will now picture an old Crown Royale bag full of slightly dented Scrabble tiles whenever I think about AI.

Expand full comment

ReplyShare

[3 replies](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us/comment/142500589)

[![Andrew's avatar](https://substackcdn.com/image/fetch/$s_!eCnT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86cbf223-4200-4021-8672-9ae44cc94424_144x144.png)](https://substack.com/profile/17566690-andrew?utm_source=comment)

[Andrew](https://substack.com/profile/17566690-andrew?utm_source=substack-feed-item)

[Aug 5](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us/comment/142503273 "Aug 5, 2025, 8:26 PM")

Liked by Adam Mastroianni

Some of the best writing on AI I've ever read. Should be a mandatory primer before anyone uses, talks, or even thinks about it.

Douglas Engelbart (inventor of the mouse, the modern filesystem, networking, multiplayer, and just about everything else in computers) talked about machines as human enhancers, and it seems to me that AI should be thought of as that. It can do the things that we are bad at so we can be better at doing the things we're really good at, like being creative.

Expand full comment

ReplyShare

[1 reply](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us/comment/142503273)

[92 more comments...](https://www.experimental-history.com/p/bag-of-words-have-mercy-on-us/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2025 Adam Mastroianni · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture

This site requires JavaScript to run correctly. Please [turn on JavaScript](https://enable-javascript.com/) or unblock scripts

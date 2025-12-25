---
source: hackernews
title: Your inbox is a bandit problem
url: https://parentheticallyspeaking.org/articles/bandit-inbox/
date: 2025-12-25
---

## Your Inbox is a Bandit[🔗](#(part._bandit-inbox) "Link to here")

|  |
| --- |
| [1 The Bandit](#%28part._.The_.Bandit%29) |
|  |
| [2 Combatting the Bandit](#%28part._.Combatting_the_.Bandit%29) |
|  |
| [3 My Context](#%28part._.My_.Context%29) |
|  |
| [4 Hit Snooze?](#%28part._.Hit_.Snooze_%29) |
|  |
| [5 My Solution](#%28part._.My_.Solution%29) |
| [5.1 Populating the Folder](#%28part._.Populating_the_.Folder%29) |
| [5.2 Processing the Folder](#%28part._.Processing_the_.Folder%29) |
|  |
| [6 What About Things That Aren’t Email?](#%28part._.What_.About_.Things_.That_.Aren_t_.Email_%29) |
|  |
| [7 Conclusion](#%28part._.Conclusion%29) |

### 1 The Bandit[🔗](#(part._.The_.Bandit) "Link to here")

In probability and machine learning, there’s a notion of
[multi-arm
bandit](https://en.wikipedia.org/wiki/Multi-armed_bandit) problems. Your mental image should of you sitting in front of a row of
slot machines (known as a one-armed bandit: the “arm” being the lever,
“bandit” because it takes your money), each of which may produce a payoff at
any moment. Which one do you play next? Facing a machine, you have a choice
between exploitation—pulling its lever—or
exploration—choosing another machine. The literature is always cast in
terms of maximizing your reward, though in the case of slot machines, and the
situation we’ll discuss here, it’s more about minimizing your loss.

Many real-world problems can be cast as bandit problems. How do you allocate
money to portfolios? Which restaurant do you go to? What ad do you show? And so
on.

Very loosely speaking, your email inbox is a bandit. You’re given a bunch of
message threads, and you have to decide whether to “exploit” a particular
one—read it, respond to it, deal with replies in the thread, and so on—or
“explore” another thread. And much like a slot machine, this bandit also
leaves you broke: if not for money, then certainly for time and energy. Having
to make choices is cognitively taxing, and having to constantly make decisions
from a large number of options becomes numbing. We end up picking either
sub-optimal strategies (like always choosing the top thread), clicking at
random, or just rooted in indecision. Nobody enjoys this.

### 2 Combatting the Bandit[🔗](#(part._.Combatting_the_.Bandit) "Link to here")

Many people have suggested strategies for dealing with this. One popular
technique is Inbox Zero. The jokes about it suggests virtually nobody attains
it, but I’m not even convinced it’s a virtue. I have many correspondents—such
as my collaborators—who write detailed, thoughtful messages, and deserve
detailed, thoughtful answers. (And if I don’t provide those, they’ll stop
writing me, which will be to my detriment.) Mindlessly dashing off replies to
get them out of my inbox is neither an option nor desirable.

Other have suggested strategies like Getting Things Done. These always strike
me as heavyweight and solving problems I don’t have.

This article instead describes a strategy I’ve been trying to avoid this
problem, and it’ll evolve as my strategies evolve.

### 3 My Context[🔗](#(part._.My_.Context) "Link to here")

Let me first tell you how I work. My situation may not apply to you at all.

First of all, I try to have only one inbox, which is my email account. I have
not have open Direct Messaging on Twitter; I try to avoid Facebook Messenger
(and wish I could turn it off); I don’t use Slack; I use Zulip only with my
course staff; most people know not to text me. So just about everything gets
routed through my inbox.

Some of my inbox entries are big tasks that require a lot of thinking: e.g.,
correspondence with research colleagues on code, experiments, results, and
papers. They take time and require me to engage deeply. They often interleave
email, calls, and shared documents. These are not what cause me grief. They’re
actually the most enjoyable part of my work.

But I get several messages that are small and, for technical reasons (not having
to do with their sender), ones that I don’t want to deal with right away. For instance:

* I need to check something on an official work site. This requires going
  through a multi-step authentication process with multiple redirections.
* I need to look up a departmental policy. This requires searches, finding
  the right page, scrolling to the right spot, reading the text carefully, and
  formulating a response.
* I need to approve some procedural matter, such as certifying effort on
  past payments.
* I have to add an entry to a calendar, with supporting documentation.

and so on. None of these takes more than five minutes in terms of wall-clock
time, often much less. But they completely destroy my concentration, and it
takes me a long time to recover my flow.

These are the “death by a thousand cuts” emails.Judging from the
[social
media response](https://twitter.com/ShriramKMurthi/status/1386521260323184641), a lot of people resonate with this diagnosis. There are often
several a week. They stare at me accusingly from my inbox, taunting me to make
explore/exploit decisions. These are the messages that destroy my
concentration, peace of mind, and joy. These are the bandits. These are what I
need to address.

But there’s another category of email that causes a different kind of pain:
personal email. It comes from friends and acquaintances. I’d like to write a
long, personal reply, but really not in the middle of the workday. So I either
write too short a reply, or put it off and take too long to reply, or feel the
usual pain of breaking away from my flow.

For both categories of messages, traditionally, there have been only two things
I can do (often both!). One is to leave them in there, staring at me
accusingly, distracting me, and making me feel guilty. The other is to deal
with them now, breaking concentration, and often leaving me feeling like I’ve
“gotten nothing done all day” (with maybe shorter personal email replies than
I would wish).

Here is one more observation that is true of most of these: they don’t need to
be dealt with right away. It’s very rare that they can’t wait a few days for
when you’re in a better state to deal with them. You might think this provides
flexibility, but in fact it makes the situation worse: you’re constantly
wondering when to deal with them. This is the classic multi-arm bandit
situation. Your inbox is a bandit who robs you of concentration, peace of mind,
and joy.

### 4 Hit Snooze?[🔗](#(part._.Hit_.Snooze_) "Link to here")

They don’t need to be dealt with right away, right? Problem solved: use Gmail
Snooze or the equivalent! Figure out when you want to do them, snooze until
then, they come up at that point, deal with them when they come up. Voilà!

But that has not worked at all, for three reasons.

First: I was constantly having to think about when to snooze to. Which
often led to increasingly complicated over-thinking (“Tue PM? Wed night? Oh
wait…”), checking my calendar multiple times, and so on. My concentration was
destroyed just in deciding when to snooze to.

Second: When it showed up, I wasn’t necessarily ready. Something else may have
come up (e.g., a colleague wanting urgent feedback). So now the emails are back
in my inbox staring accusingly at me. Now with extra Gmail chroming saying
the bandits are back. Which reminds me I’ve already put these off once. If I put
them off again, I know I’ll put them off again and again, and they may never
get done. So much more stress!

Third: They’re in there with lots of other email. Those others messages are
distracting me. And as I’m dealing with these, new ones are coming. It’s all a
big mess that is an even bigger source of stress. No better than before; heck,
maybe even worse. Snooze was failing badly.

### 5 My Solution[🔗](#(part._.My_.Solution) "Link to here")

Hopefully I have laid it out clearly enough that the answer is obvious to you,
even though up front it wasn’t to me. Here is my solution: a new Gmail
label that I have always showing, even when empty. For now, I’m calling it

DBTC

That’s short for “Death By a Thousand Cuts”.

#### 5.1 Populating the Folder[🔗](#(part._.Populating_the_.Folder) "Link to here")

All little tasks that come up during the week are candidates for this
folder. During the week, I’m ruthless. My rule is,

* if this message is not urgent, and
* if dealing with it now will distract me, and
* if it’s either not long, or if it’s personal

it goes straight into the folder. It’s out of my inbox; it’s out of my
sight. It’s a slot machine I don’t have to worry about playing.

Most of the time, I don’t have to even glance at the message to know it fits
this category. The sender, subject line, and short email preview will all
suffice.

This has the benefit of Snooze, in that it’s taken the message out of my
current field of vision. Unlike Snooze, I didn’t have to think about when to
move it to: the label already says when.One weakness: unlike Snooze,
which automatically awakens messages, this does not. You may need to use a
calendar for that. The critical thing is that it takes me almost no time to do
it. I don’t have to stop thinking about whatever I’m thinking about. I don’t
have to process any new information. I barely have to make a decision. I just
have to move it out of the way.

Note that this now leaves Snooze to do what it does well: sleep messages until
specific dates/times. Let’s say I’m expecting a reply from X on Y. I can Snooze
the thread until the time I want to be reminded about it. Alarm clocks ≠
folders; Snooze is an alarm clock, not a folder.

Sometimes, I get duplicate messages on a topic. For instance, when writing the
first version of this article, our final grades were due; we received multiple
reminders about it. I was pretty sure I already had older messages about that
in the folder. Still, when I got new messages about it, I didn’t stop and
think, and I didn’t check: I didn’t want the distraction. It was more important
to not miss the matter than to worry about the duplicates. If when I processed
it I found duplicates, no matter: it would just mean I had fewer things to
do.Reader: there were several duplicates. Score.

Note that if I leave things unread before putting them in the folder, the
folder count shows me how many things are in there. That gives me some sense of
the task list length.

#### 5.2 Processing the Folder[🔗](#(part._.Processing_the_.Folder) "Link to here")

The critical thing is to set aside time to process this folder: let’s
call it DBTC-time. I happen to do it on the weekend (because I’ve “remixed”
my week so I much more freely interleave work and personal time); you might
want to do it at some other time. You may even want to have two folders, one of
work DBTC and the other personal, and process them at different times. The
critical thing is to block that time, put it on your calendar, create a
reminder, and do whatever else you need to be disciplined about processing it.

Here’s the beauty of this system. When DBTC-time comes, I go to that folder. As
a result, I see only what is in that folder. Nothing else. No other
emails interleaving. No weird snooze color chroming. No new messages getting in
the way. That folder is exactly the stuff I have to get through.

Think of how you might process paper mail. You don’t check every letter as soon
as it’s delivered. In the evening, you pick up the mail and sort it. You might
even accumulate it to the weekend. You then set aside time for it. Same deal
here. The only exception is if there’s a letter or package you were urgently
awaiting; those you process right away. For me, that’s my interesting
mail.

My one goal is, “DBTC Zero” by the end of the DBTC period. It’s all got to get
cleaned out. I’m allowed to move it somewhere else, add things to todo-lists,
make issues on code repositories, or whatever else. But it’s got to be clear by
the end.

When I open the folder, I put myself in a “boring” state of mind. I know there
will be lots of annoying little logins and clicks and pedantry. I might have a
sports match in the background, or music, or a snack, or something. But what
I’ve found is:

Those tasks are not remotely as annoying or distracting when I do them in their
assigned “down” time as when they interrupt my flow.

Flow state is so hard to get, details are so hard to hold in my head, they
really hurt. During DBTC-time, they barely register: they’re more like a
low-order numbness than like a high-order pain. And I feel good that this
enabled me to have a better past week, and by doing this, I’ll have a better
next week. I can actually feel the virtue!

### 6 What About Things That Aren’t Email?[🔗](#(part._.What_.About_.Things_.That_.Aren_t_.Email_) "Link to here")

Indeed! I now create DBTC channels in multiple media. For instance, my
task manager has a DBTC list as well. When I think of a task that
needs to be done, but meets the criteria above, it goes into the DBTC
list instead of my main todo list. When my DBTC-time arrives, I switch
my task manager to the DBTC list, just like my inbox. Same principle.

Another thing I’ve found useful is to treat entire apps as DBTC
items. For instance, I am effectively forced to use Discord, WhatsApp,
Apple Messages, etc. But I very rarely need to read these frequently,
and doing so would only create more distractions. Therefore, unless I
posted something and need a quick answer, I only read these during
DBTC-time. If you’ve been on any of these systems with me, you may
have noticed I typically only respond once a week, on the
weekend. This has the added benefit of slowing down conversations,
which on instant-messaging-style media is often a virtue (the
exception, naturally, being when you actually need instant
messaging).

### 7 Conclusion[🔗](#(part._.Conclusion) "Link to here")

Hopefully you can see that all the issues raised earlier are addressed
by this process. I’ve been doing this since March 2021, and it
continues to work well for me. I hope it works for you too!

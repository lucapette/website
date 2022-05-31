---
tags:
  - "engineering management"
  - programming
date: "2015-04-28T00:00:00Z"
description: Some random thoughts about development and its relation to speed
keywords: developers, speed, career
title: What I think about when programmers talk about speed
---

A couple of weeks ago I had an interesting conversations with one of my team
members. She confessed to me that she felt she was a slow programmer.

I told her I was surprised and then she asked me why. The conversation we had
lead me to write this article. I will break it down in two main topics:

- Three steps from an idea to the feature.
- Forget about the code, no one cares about it.

## Three steps from an idea to the feature

For the sake of the conversation, I'm going to come up with some generic steps
programmers go through when doing their job:

- Trying to figure out what to do
- Writing the actual code that does that
- Putting that code in production

Nothing too exciting, right? It's to get the conversation going.

### Step 1: Trying to figure out what to do

Even the sharpest programmer needs to understand what to do before writing some
code. She may know the system very well, she may have enough context to ask
product managers the right questions, she may not miss any details.

No matter of good they are, this step takes some time. I haven't seen many
programmers being fast at this stage unless they were working on a trivial
feature.

Speed at this stage is intrinsically connected with the product development
workflow (how involved are product managers at this point? Is there any
checklist that we can use to say a feature is ready to be worked on?) so, in a
way, programmers' speed isn't only on them. It's more a team thing that an
individual metric.

### Step 2: Writing the actual code that does that

This is where I've seen programmers performance varying the most.

Some people are very fast at writing code, others not so much.

From what I've seen, most programmers that are "really fast" write code that is
not ready to go anywhere.

They miss the details that make the code production ready, their code has a lot
of "one time workarounds", there are typical startup comments everywhere ("#TODO
Fix this").

It only feels like they're fast if you don't actually look closely at their
code.

At this stage, speed is tangible because its output is public to the rest of the
team (pull requests being a common example) but also deceptive: producing code
isn't the same as shipping it.

### Step 3: Putting that code in production

Here comes the critical step.

Speed here is a consequence of what already happened in the two steps.

No actual work needs to be done at this point in the ideal world, right?

The job you've done at step 1 is perfect and made the code you wrote at step 2
bug-free and perfectly compliant with the requirements!

Now, I wonder: how many times have you seen this happen? How often code gets
merged and you forget about it forever?

I'm not sure what your answer is. It's probably closer to "ehm... never!" than
"this happens all the time!" though.

From what I've seen in my career, people have very different ways of dealing
with these steps.

I met a lot of people that change the order of these steps: "let's write the
code who cares about the details of step one. We'll figure out later!".

I also worked with many people who skip/don't understand/don't care about step
3. Honestly too many.

Most managers focus too much on a specific step so the programmers they manage
tend to do the same.

I think none of the steps per se actually matters. I like to say "code only
works when it works in production". That doesn't meant the last step of this
process is the most important.

It means you have to be patient. You have to wait some time (that's highly
dependent on the scale of the product you're working on) to say "yes, this
actually works just fine".

I wouldn't stop at "oh I love this programmer, she finishes stories in no time".
Be patient: look at her code one month later.

Wait for bugs to come up. Wait for other programmers' feedback on how easy it
was to change her code (the most welcoming a piece of code is to change, the
better its design don't you think?).

The bottom-line: we often focus on the wrong metrics and we have to pay more
attention to the entire process.

## Forget about the code, no one cares about it

I like to troll (I'm from Napoli eh 🤌). That's why this section has this title.

Of course, as a technologist, I know code matters. I know the language you use
influences your results, so do the frameworks and libraries.

That doesn't change the fact that we (the programmers) are the only ones caring
about those topics when it comes to product development.

I'm fine with this. No one else cares. Your customers don't. They expect the
button to work and they want to do whatever they please with your product.

That's a point I never get tired to emphasise. We tend to forget the fundamental
property code must have: it has to work. That's it. Nothing more.

It's hard to keep that in mind when it comes to speed. The programmer that
confessed me she felt slow also forgot about this.

She told me: "Oh this programmer opens pull requests all the time, they're super
fast!". Pull requests rarely go to production the way they're opened. And even
when they do, you'll have to fix the bugs that come out of them.

That is a cost that has an impact on your customers and on your _actual_ speed.

I think this topic is often referred to as "shipping culture". Which often means
one of these:

- People justify bad code going to production in the name of it. So they're very
  fast at breaking things.
- People try to make things "perfect" before shipping them. So they're very slow
  at not breaking things.

Now, there must be some other way right? This is where where I tell you I know
everything so I will convince you to do it my way... Well, no. I'm going to
disappoint you a bit.

I don't think there is some other way. I kind of like that our job is to find an
equilibrium between the two sides of this spectrum.

The more balance between "ship this 💩 now!" and "let's make this perfect first
✨✨✨", the faster you actually are.

Focusing too much on the former will produce too much debt and focusing too much
on latter won't product enough.

I do believe in the end the problem is that our industry focuses too much on the
wrong metrics. The balance I'm talking about here means forgetting to measure
how many features a developer ships in a week or how fast she goes from starting
to work on the feature to opening the pull request. I'm not sure I can propose
metrics that work better for your team, in the end all metrics are
context-dependent.

But I can share some questions that can help you shape better metrics:

- How ready is the code I get from this developer?
- Do we always have to review everything they write?
- How often do we have to go back to something they wrote after we ship it?

What I'm trying to say is this: focus on the value added to the customers of the
product you're building.

It is important to keep in mind that what we do is not writing code. We're not
"code typists". We program systems that someone else needs to use to do their
job.

This perspective keeps priorities focused on what matters and it should play a
major role in "developers' performance":

> Don't look at their code, look at the features

That's what I told the developer who felt slow: "we never have to touch the
features you write after we ship them".

I hope other people benefit from this perspective and realise they are not as
slow as they think. The other way around may be a little harder to accept though
:)

---
tags:
  - "engineering management"
  - programming
date: "2015-04-28T00:00:00Z"
description: Some random thoughts about programmers and their performance
keywords: developers, speed, career
title: What I think about when programmers talk about speed
---

In my career as an engineering manager, I often find myself in conversations
about speed of development with my direct reports.

Speed intended as how fast they can write a feature and ship it.

While I have the tendency to dislike the terms "performance" and "speed", I get
why programmers are so interested in this. After all, shipping code to
production is a pretty tangible outcome.

I'm writing about it because over the years a clear pattern emerged, especially
with less experienced developer: a clear disconnect between the way they think
about speed and the way I think about it.

In my conversations, I always cover the following:

- Three steps from an idea to the feature.
- Forget about the code, no one cares about it.
- A conscious shipping culture.

I will do the same here so I can also refer back to this when I find myself
having such a discussion in the future. I have no doubt it will happen again.

## Three steps from an idea to the feature

For the sake of the conversation, I use the following overly generic steps
programmers go through when doing their job as a baseline for the conversation
about speed:

1. Try to figure out what to do
2. Write the code
3. Ship the code

Nothing too exciting. But it gets the conversation going.

### Try to figure out what to do

Even the sharpest programmer needs to understand what to do before writing some
code. They may know the system very well, they may have enough context to ask
product managers the right questions, they may not miss any details.

No matter of good they are, this step takes some time. I haven't seen many
programmers being fast at this stage unless they are working on a trivial
feature.

Speed at this stage is intrinsically connected with the product development
workflow (how involved are product managers at this point? Do we use a feature
readiness check-list?) so, in a way, programmers' speed is a little out of their
hands.

It's more a team thing that an individual metric and focusing too much on
individual performance at this stage feels unfair.

In my conversations, I cover this step to gather feedback about our workflow and
to make sue they understand speeding this process up isn't entirely in their
hands.

### Write the code

This is where I've seen programmers performance varying the most. Some people
are very fast at writing code, others not so much.

In my experience, most "speedy" programmers write code that is not ready to go
anywhere.

They miss critical details to make the code production ready. Their code has a
lot of "one time workarounds", there are TODO comments everywhere.

It only feels like they're fast if you don't actually look closely at their
code.

At this stage, speed is tangible because its output is public to the rest of the
team (pull requests being a common example) but also deceptive: producing code
isn't the same as shipping it.

What I try to explain to my direct reports when discussing this step is that
speed here is often detrimental to the quality of the code. There's a balance of
course and we may want to discuss that.

### Ship the code

Speed at this stage is a consequence of what already happened in the previous
two steps.

No actual work needs to be done at this point, right?

The job you've done at step 1 is perfect and made the code you wrote at step 2
bug-free and perfectly compliant with the requirements!

So... how many times have you seen this happen? How often code gets merged and
you forget about it forever?

I'm not sure what your answer is. It's probably closer to "ehm... not very
often!" than "all the time!" though. That leads me to the next question.

### Which step should you focus on?

From what I've seen in my career, people have very different ways of dealing
with these steps.

For example, I met a lot of people that like changing the order of these steps
for example: "let's write the code who cares about the details of step one.
We'll figure out later!". Some say it works, other say it doesn't.

I also worked with many people who skip/don't understand/don't care about step
1. Honestly, I met too many.

The point is that focusing too much on a specific step doesn't really make you a
better programmer.

I think you can't take these steps in isolation. They don't matter on their own.
The goal is to ship code to production so we have the tendency to optimise the
workflow for shipping. Often we overestimate the importance of the latest step
for this reason.

In reality, you have to be patient. Only some time _after_ you shipped something
(that's highly dependent on the scale of the product you're working on) you get
to say "Yeah, this works". That's how you measure _actual_ speed, isn't it?

Don't stop at "Oh I love working with X! They finish stories in no time". Be
patient: look at impact of their code over time.

Wait for bugs to come up. Wait for other programmers' feedback on how easy it
was to change their code (the most welcoming to change a piece of code is, the
better its design which means the better the programmers did, don't you think?).

The bottom-line: we often focus too much on producing code, instead of thinking
about the product we're building. Perfect segue for the next paragraph.

## Forget about the code, no one cares about it

You get this title because I like to troll (I'm from Napoli capisc? 🤌). Nope,
not sorry.

Of course, as a technologist, I know code matters. I know the language we use
influences results, so do the frameworks and libraries.

That doesn't change the fact that we (the programmers) are the only ones caring
about code, languages, and frameworks when it comes to product development.

No one else cares. Your customers don't.

Everyone else just expect the damn button to work so they can do whatever they
please with your product. They don't care how functional and side-effect free
was the code that handled the click.

I never get tired to emphasise this. We tend to forget the fundamental property
code must have: it has to work. That's it. Nothing more.

We also tend to forget that code, languages, frameworks are just tools. They're
not a goal. They're a mean to an end.

It's somewhat hard to keep that in mind when it comes to speed.

I've had people telling me: "Oh Margaret opens pull requests all the time,
she're super fast!".

Pull requests rarely go to production the way they're opened though. Even when
they do, you'll have to be on the lookout for bugs after you ship them.

## A conscious shipping culture

In these conversations I often talk the idea of being conscious of the way we
want to ship code. For the sake of the argument, I bring up these two extremes:

- People justify bad code going to production in the name of it. So they're very
  fast at breaking things.
- People try to make things "perfect" before shipping them. So they're very slow
  at not breaking things.

Now, there must be some other way right?

This is where where I tell you that I know everything and I'm about to convince
you to do it my way... Well, no. I'm going to disappoint you a bit.

I don't think there is some other way. I even kind of like that our job is to
find an equilibrium between these two shipping cultures.

My point is that the more balance between "ship this 💩 now!" and "let's make
this perfect first ✨✨✨", the faster you actually are.

Focusing too much on the former will produce too much debt and focusing too much
on latter won't product enough.

I do believe in the end the problem is that our industry focuses too much on the
wrong metrics so individually we're peer-pressured into doing the same.

The balance I'm talking about here means forgetting to measure how many features
a developer ships in a week or how fast they go from starting to work on the
feature to opening the pull request.

I'm not sure I can propose metrics that work better for you and your teams, in
the end all metrics are context-dependent.

Focus on the value added to the customers of the product you're building. You'll
find better metrics there.

Keep in mind that what we do is not writing code. We're not "code typists". The
code isn't the goal. Working products are the goal.

This perspective keeps priorities focused on what matters and it should play a
major role in "developers' performance":

> Don't look at their code, look at the features they wrote

I hope other people benefit from this perspective and realise they are not as
slow as they think. Or the other way around. That may be a little harder to
accept though :)

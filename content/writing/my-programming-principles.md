---
title: "My Programming Principles"
description: " what guides me after 20 years of writing code for a living"
date: 2022-04-17T14:12:13+02:00
draft: true
keywords:
  - programming
  - principles
---

One day, when asked how he had prepared for a game he had just played, Alexander
Grischuk, an elite chess Grandmaster, answered quickly with profound wisdom:

> "I've been preparing my whole life". GM Grischuk, Alexander

The beauty of his answer is that you can apply it to _any_ job. _All_ your past
experiences will contribute to whatever decision you make today. Since I watched
that interview with Grandmaster Grischuk, I have been subconsciously collecting
guidelines, ideas, and principles that stick with me over the years. The
parallel with programming was obvious in my head and writing about it now is the
natural consequence of my desire to learn more about it (and myself). I confess
that I find the title of the article a little pretentious but I decided to go
with it because it fits well the ideas expressed here.

It felt natural to group principles into categories: existing codebases and
greenfield projects. I believe in approaching coding sessions differently
depending on the context. While this may sound a tad obvious, I encountered
enough examples of over-engineered or under-engineered (why isn't that a thing?
It's almost as common!) solutions in my career. More often than not, these
solutions were merely a reflection of approaching an existing codebase like a
greenfield project or, maybe worse, the other way around. Let's get started!

## How I approach an existing codebase

When I think about these principles, I think of codebases that are large enough
you can't possibly keep an accurate representation in your head. How large is
probably too personal so I won't even try to give an example. You may know some
parts very well but there are enough unknowns you can't make a non-trivial
change without some planning. Most of these principles are somewhat _even_ more
relevant when the codebase is completely new to you and you're trying to make
sense of it and change it at the same time.

### Small changes, fast feedback loops

Change one line _or less_ before seeking feedback. I literally mean the smallest
possible change you can think of. I've been in a lot of pair programming
sessions where my partners were surprised to see how obsessed I'm with this.

You want to tighten the delta between what you think the code does and what it
actually does as much as possible. The better you know a codebase, the higher is
your confidence to make larger changes before seeking feedback.

You might have heard of the "Boyd's Law of Iteration" which states that the
speed of iteration beats the quality of iteration. I find that it applies really
well to learning. It works well while dealing with code. I can still remember
pretty vividly the time I discovered Ruby had IRB. It made the experience of
learning the language much more satisfying. I was learning really fast because I
could try many things and get instant feedback.

How does this play with approaching an existing codebase? The idea is that you
setup your own "REPL experience". The actual tool you use for this doesn't
really matter (I have my own favourite, it's called
[production](#test-in-production)), the core of this idea is that you have it
set up _before_ you make touch any code. That way, you can change very little
and immediately get feedback from the system.

The context of this principle is focused programming sessions but I apply this
principle also to end result of good programming sessions: deployable artifacts.
There's [good evidence](https://www.goodreads.com/book/show/35747076-accelerate)
now that smaller changes lead to higher productivity. They're safer to deploy
and rollback if needed. They're also easier to review and more likely to move
fast to production.

We can derive at least two different conclusions from this principle:

- Fixing a few bugs one after the other, possibly in different part of the
  system, is a great way to approach a codebase that's new to you.
- Breaking down a large feature into smaller pieces is a great way to approach
  changing a large codebase you're familiar with.

### Test in production

I can hear [@mipsytipsy](https://twitter.com/mipsytipsy) in my head say "fuck
yeah!" every time I mention this principle. The simplest way to put this
principle is that you want to expand the boundaries of your "REPL experience" so
much it includes your production systems. As much as for the previous principle,
it's all about the speed of iteration. It's not enough to have a tight feedback
loop while making the smallest possible change if you don't ship it to
production in _minutes_. You can put a lot of effort writing "correct" code but
unless it works in production, it doesn't add any value yet.

This urgency of shipping code to production helps creating an healthy culture.
You're going to need fast CI and CD pipelines. You're going to need to trust
your tests. You're going to need ways to observe the impact of changes on your
production systems. These aspects all contribute to the speed of your iteration.
You want to move fast, but you don't need to break things. Well you will break
some things, it's part of the deal. We _all_ test in production when we ship
code anyway so the point is that doing it deliberately and consciously helps
creating a safer, faster environment for writing code. Including your production
systems in your "REPL experience" will unlock a lot of potential.

### Be a gardner

My first encounter with this analogy is a pretty old
[commit](https://github.com/rails/rails/commit/fb6b80562041e8d2378cad1b51f8c234fe76fd5e)
made by [@fxn](https://twitter.com/fxn). The idea is simple: code gardening is
the act of changing code with the goal of improving it just a little. Improve an
error message, use a more idiomatic way to write those three lines, align that
test with your internal conventions. The codebase is living thing: it's a garden
and you take care of it every day. You prevent bad habits from forming, you heal
that system that is a bit sick.

I like the analogy a lot and the way I like to apply is connected to small
changes. I used to like the idea of gardening _while_ writing new features. But
I grew out of it. Because smaller changes are always preferable, in a way that's
a stronger principle. So now I have a small workflow that helps my gardening
activities. I take notes of the gardening I want to do while whenever I can. A
small todo list. Sometimes, I share it with my team (it really depends on the
team and the context they're in) to help other people form the habit of code
gardening. I then use this todo list as a personal reward system. I get done the
things I need to and then I treat myself by improving that test that still uses
produces those annoying warnings. I have seen this dynamic at play with other
programmers as well once the code gardening habit is established. The reward
system is simple and allows people to make small improvements _all the time_.

### Delete aggressively

I enjoy deleting code.

### Read features end to end

While you read, take notes. Note down questions, idioms

### Facts > Assumptions

- debugging (gather facts about behaviour)
- perf improvement (measure, don’t guess)

## Starting from scratch

Since it's a little less common, I feel privileged (or unlucky? It depends on
the day honestly) having experienced multiple times the scenario "hey you're
responsible for building this X years long project from scratch. Also, there's
no team yet so you need hire a bunch of people to do it". When you start a new
project, it can feel a little overwhelming because, of course, there's nothing
done yet :)

### Make it work, make it good, make it fast

### Throw away first

design is a struggle between attention to details and high level ideas. To make
this a positive tension:

- write a little prototype
- write a chunk of the design doc
- throw away a little code, make a change. Start from point one again

### Take lots of notes

Using your brain as storage is a waste. "The shorter pencil is longer than the
longest memory".

### Let the design emerge from the code

This is mostly an exercise in patience. The more experienced you are the worse
you have it because you know too well the little code you just wrote to "make it
work" isn't anywhere close to production quality. But that's the point of
guidelines, they guide _against_ your own primitive instincts.

### Write the code you'd like to use

## General principles

Is it really programming if there isn't at least some util functions? 😃

Jokes aside, there is an handful of principles that I apply to any context.
These are my "tru north" so to speak.

## Prose not poetry

two reasons:

- prose as a metaphor for "write code at the same level of abstraction"
- clarity > brevity

### Name things what they do

- if you get stuck, most descriptive even if long is fine
- if you’re really stuck. Go for a walk, there’s a chance the problem is not
  naming but design

### Forget about easy to change, make code easy to delete

### Write actual tests

Before I explain what I mean with "actual" in this context, I have to make a
little premise. When I say "tests", I mean the automated verification process
that your systems (kind of) work. I do not mean TDD. I have nothing against TDD
but it's a development methodology. My opinion about programming methodologies
is that none of them always work and they are great in specific contexts. Which
means I don't have much to say about them in general and TDD in particular. So
back to tests as a verification process.

Let's clarify what I mean with "actual". Here's my definition of "actual test":

> A test must change only if the behaviour it verifies changed

No other reason is good enough for a test to change. But, maybe also because of
TDD, I've seem test testing language features, missing language features
(looking at you dynamically typed languages), framework features. These tests
change all the time. And they're just baggage. My advice is to delete them.
There, I said it.

### Less dependencies is better

### Write more docs

### Confidence is a struggle

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart

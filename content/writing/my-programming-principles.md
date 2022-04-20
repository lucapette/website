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
enough examples of over-engineered or under-engineered (why isn't that a term
btw? It's almost as common!) solutions in my career. More often than not, these
solutions were merely a reflection of approaching an existing codebase like a
greenfield project or, maybe worse, the other way around.

## How I approach an existing codebase

When I think about the following principles, I think of codebases that are large
enough you can't possibly keep an accurate representation of it in your head.
How large is probably too personal so I won't even try to give an example. Most
of what I'm about to share is somewhat even more relevant if the codebase is
completely new to you (fairly common scenario when changing jobs for example).

### Change as little as possible

OK I can hear you say "this is obvious". But I actually really mean it. It works really

### Always have a feedback loop

this means bla bla and blu bla

### Test in production

I can hear @misstipsy say "fuck yeah"

### Read features end to end

While you read, take notes. Note down questions, idioms

### Assumptions are bad

- debugging (gather facts about behaviour)
- perf improvement (measure, don’t guess)

## Starting from scratch

Since it's a little less common, I feel privileged (or unlucky? It depends on
the day) having experienced the scenario "hey you're responsible for building
this X years long project from scratch. Also, there's no team yet so you need
hire a bunch of people to do it" multiple times. So I accumulated a lot of ideas
I would call specific (and others may call peculiar?).

### Make it work, make it good, make it fast

### Throw away first

design is a struggle between attention to details and high level ideas. To make this a positive tension:

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

### When you don't know how the api should look like, write the code you'd like to use

## General principles

Is it really programming if there isn't at least some util functions? 😃

Jokes aside,

Principles I always try to keep in mind, no matter the context.

### name things what they do

- if you get stuck, most descriptive even if long is fine
- if you’re really stuck. Go for a walk, there’s a chance the problem is not naming but design

### Forget about easy to change, make code easy to delete

### write actual tests

### Less dependencies is better

### Write more docs

### confidence is a struggle

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart

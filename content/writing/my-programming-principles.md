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

> I've been preparing my whole life

The beauty of this answer is that you can apply to _any_ job. Whatever decisions
you make today, _all_ your past experiences will contribute to whatever you
decide. Since I watched that interview with Grandmaster Grischuk, I have been
slowly collecting guidelines, ideas, and principles that stick with me over the
years. Writing about it is the natural consequence of my desire to learn more
about myself because I find the process very introspective. Bonus point: I get
to share my thoughts with you.

I want to say I decided to group my principles into three big categories but I
would be somewhat lying. It happened pretty naturally, here they are:

- approaching an existing codebase that is new to me. Fairly common scenario but
  I can't say I read too much about it.
- approaching greenfield projects. Since it's a little less common, I feel
  privileged (or unlucky, depends on the day) having experienced "build this X
  years long project from scratch. Also, there's no team yet so you need hire a
  bunch of people to do it" multiple times. So I accumulated a lot of specific
  ideas.
- principles I always try to keep in mind, no matter the context.

Let's dive straight into the first category, shall we?

## How I approach an existing codebase

- change as little as possible
- always have a feedback loop
- test in production
- read whole features. While you read, take notes. Note down questions, idioms
- assumptions are bad for:
  - debugging (gather facts about behaviour)
  - perf improvement (measure, don’t guess)

## Starting from scratch

- make it work, make it good, make it fast
- write to throw away. design is a struggle between attention to details and high level ideas. To make this a positive tension:
  - write a little prototype
  - write a chunk of the design doc
  - throw away a little code, make a change. Start from point one again
- while you do, take lots of notes
- let the design emerge from the code
- when you don’t know how the api should look like, write the code you’d like to use

## General principles

- name things what they do
  - if you get stuck, most descriptive even if long is fine
  - if you’re really stuck. Go for a walk, there’s a chance the problem is not naming but design
- forget about easy to change, make code easy to delete
- write actual tests
- less dependencies is better
- more docs is also better
- confidence is a struggle
  - it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
  - it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart

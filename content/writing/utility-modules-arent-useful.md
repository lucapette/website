---
title: "Utility Modules Aren't Useful"
date: 2022-05-31T09:56:02+02:00
draft: true
tags:
  - programming
keywords:
    - programming
    - practice
---

If [someone](https://twitter.com/taniarascia) I admire encourages me to
[write](https://twitter.com/taniarascia/status/1522337429881081856) about
something, I will. So here we go again.

A few weeks ago, Tania asked an intriguing question on twitter:

> Does anybody have a really good system for organizing utils/helpers? Or is it
> always just chaos?

My TL;DR answer to utility modules is "don't create them".

As I said in the Twitter conversation with Tania, it may sound cheeky but I'm
not trying to be funny or clever about it. I say "don't have utility modules"
because I genuinely believe this approach leads to the best possible organisation.

On twitter, it's hard for me to expand on such a nuanced topic so, with Tania's
encouragement, I've decided to write a little about it.

## It's always Chaos

If you do have utility modules, it's always chaos. I definitely agree with that.s

The problem with such a catch-all solution is that it comes with a variety of
well-know problems (not a big fan of the word "anti-pattern" but I can see why
someone else would use it here).

Once an utility module is in place, people will find it easier to add "one more
function" there than introduce a new library. So over time you'll end up with a
big module that exhibits a lot of problems:

- There's all kind of unrelated code in the same place. So you'll end up
  changing the utility module very often. Depending on how your CI/CD pipeline
  works, this may end up being very costly.
- Because lots of code is unrelated, you'll also end up with very different
  abstraction levels in the same library. So no cohesive API.
- Some parts of your codebase will use just a few functions from the utility
  module but still depend on a large library.
- Most of your code will depend on the utility module. This makes your
  dependency graph pretty unhelpful. You won't gain much insight into a specific
  part of your codebase knowing it depends on the utility module. Because, well,
  everything does.

So if utility modules are a problem, what's a better approach? After all,
reusing code is a fundamental abstraction.

In a way, my answer is somewhat funny because you can read it as "have lots of
utility modules". Let me expand on that.

The idea is that you want to eliminate the catch-all nature from the equation.
Trivially put, instead of `utils.TrimLeft` and `utils.TrimRight`, you get
`StringUtils.TrimLeft` and `StringUtils.TrimRight`.

This example is trivial just to drive the point home.

The way you may want to eliminate the catch-all nature from your utility modules
is language dependent. So I won't try to provide code samples as they'd make
sense only in the context of the language you're using.

Libraries tend to be very idiomatic by definition. Which means reusing code is
tightly connected to the nature of the language.


One concrete example: languages like Ruby or Kotlin allow to you to do
[extension oriented
design](https://elizarov.medium.com/extension-oriented-design-13f4f27deaee) and
that's really different from the way you would reuse code in Golang or Java for
example.

To keep things practical, I'll discuss both the scenario in which you already
have catch-all utility modules and the scenario in which you don't because
you're starting from scratch working on a new system.

## If you already have them, get rid of them

The title says it all. It's an exercise I put in practice more than once in my
career.

## If you don't, never introduce them

## Purpose driven internal packages work

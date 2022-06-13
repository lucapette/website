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

As I said on Twitter, it may sound cheeky but I'm not trying to be funny or
clever about it. I say "don't have utility modules" because I genuinely believe
this approach leads to a much better organisation of internal libraries.

On Twitter, it's hard for me to expand on such a nuanced topic so, with Tania's
encouragement, I've decided to write a little about it.

## It's always Chaos

If you do have utility modules, it's always chaos. I definitely agree with Tania
on that.

The problem with such a catch-all solution is that it comes with a variety of
well-know problems (not a big fan of the word "anti-pattern" but I can see why
someone else would use it here).

Once an utility module is in place, people will find it easier to add "one more
function" there than introduce a new library. So, over time, you'll end up with
one big module that exhibits a lot of problems:

- There's all kind of unrelated code in the same place. So you'll end up
  changing the utility module very often. Depending on how your CI/CD pipeline
  works, this may become costly.
- Because lots of code is unrelated, you'll also end up with very different
  abstraction levels in the same library. There's no cohesive API.
- Most of the codebase will use just a few functions from the utility module but
  still depend on a large library. This makes your dependency graph pretty
  unhelpful. You can't gain much of an insight into a specific part of the
  codebase knowing it depends on the utility module. Because, well, almost
  everything does anyway.

So if utility modules are a problem, what's a better approach? After all,
reusing code is a fundamental abstraction we can't just give up on.

The idea is that you want to eliminate the catch-all nature from the equation.
Trivially put, instead of doing `utils.TrimLeft`, do `StringUtils.TrimLeft`.

You want to go from a catch-all utility module to many little purpose-specific
libraries.

In a way, my answer is somewhat funny because you can read it as "have lots of
utility modules".

I won't try to provide code samples as they'd make sense only in the context of
the language they're written in. Libraries tend to be very idiomatic by
definition so the way you reuse code is tightly connected to the nature of the
language.

One concrete example: languages like Ruby or Kotlin allow to you to do
[extension oriented
design](https://elizarov.medium.com/extension-oriented-design-13f4f27deaee).
That's really different from the way you would reuse code in, say, Golang or Java.

To keep things practical, I'll discuss both the scenario in which you already
have catch-all utility modules and the scenario in which you don't because yet.

## If you have them, get rid of them

It's never too late to get rid of big catch-all utility modules. It's an healthy
way to reduce technical debt. The goal is to break down a big module into
smaller purpose driven modules.

In practice it means a few things. Let's go over them one by one.

Some smaller modules will be immediately obvious. The ones I run into more often
dealt with strings and dates. They're also the easiest to extract.

I generally start from these obvious modules as the first tasks have to pay for
the setup costs. That varies a lot depending on the nature of a codebase. More
on this later.

You will run into functions are used once or twice in your codebase. My
suggestion is to [prefer duplication over the wrong
abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction).

So if a function it's used only once or twice, I don't mind getting rid of the
library all together and copy back the code.

You will also run into some functions that are used a lot but have nothing to do
with each other. That is the "core" problem of having introduced a catch-all
library.

The simplest approach here is to introduce a library for each function first.
That will help you see what can be refactored to be part of the same library.

In my experience, the hardest part of this process is naming. I keep it simple
and [name things what they do]({{< ref
"/writing/my-programming-principles#name-things-what-they-do" >}} "my
programming principles")

## If you don't have them, don't introduce them

It sounds easier than it actually is to not introduce utility modules. The
problem, as for extracting modules from a catch-all library, is the setup cost
of the new library.

In most codebases, adding an internal library is costly. There are many tasks
involved with hosting and releasing a library.

That's why developers tend to introduce utility modules. They pay for this setup
cost once and then run with it for a long time. And they end up with the giant
ball of mud we already discussed.

So the first time you find yourself wanting to add an internal library to your
codebase, it's a good time to reflect on this setup cost and try to minimise it
for the future.

The cheaper the setup cost, the easier it will be to never introduce a catch-all
module.

Unfortunately my advice here may not be practical depending on your context. My
favourite solution to this problem is to organise the whole codebase into a
mono-repository.

I have all kind of reasons to prefer a mono-repository (enough for a "the appeal
of mono-repositories" article) and the easiness of adding small,
purpose-specific internal libraries to the codebase is among the most important
reasons.
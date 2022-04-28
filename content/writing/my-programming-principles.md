---
title: "My Programming Principles"
description: "What guides me after 20 years of writing code for a living"
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
experiences will contribute to some extend to whatever decision you make today.
Since I watched that interview with Grandmaster Grischuk, I have been
subconsciously collecting guidelines, ideas, and principles that stick with me
over the years. The parallel with programming was obvious in my head and writing
about it now is the natural consequence of my desire to learn more about it (and
myself). I find the title of the article a little pretentious but I decided to
go with it because it fits well the ideas expressed here.

It felt natural to group principles into categories: existing codebases and
greenfield projects. I believe in approaching coding sessions differently
depending on the context. While this may sound a tad obvious, I encountered
enough examples of over-engineered or under-engineered (why isn't that a thing?
It's almost as common!) solutions in my career. More often than not, these
solutions were merely a reflection of approaching an existing codebase like a
greenfield project or, maybe worse, the other way around. Let's get started!

## How I approach an existing codebase

When I think about these principles, I think of codebases that are large enough
you can't possibly keep an accurate representation of all its parts in your
head. You may know some parts very well but there are enough unknowns you can't
make a non-trivial change without some planning. What's exactly large enough is
probably too personal so I won't even try to give an example. Most of these
principles are somewhat _even_ more relevant when the codebase is completely new
to you and you're trying to make sense of it and change it at the same time.

### Small changes, fast feedback loops

Change a few lines _or less_ before seeking feedback. I mean _literally_ the
smallest possible change you can think of. I've been in a lot of pair
programming sessions where my partners were surprised to see how little I change
before I seek feedback. The point is that very rarely the whole change we have
to make to a system is a couple of lines. It's always a little more than that,
possibly touching a few files here and there. In these situations, my workflow is:

- Change very little. A few lines or less.
- Seek feedback
- If it works _so far_, repeat

The key of this workflow is that "so far". This approach creates mental
checkpoints where I can assume all the code I wrote so far works. As soon as
what I'm seeing in my feedback loop is not what I expect, I need to go back "one
checkpoint" only. You may be asking yourself: "hey, isn't this just TDD?" and
the answer is "yes and no". TDD is _one_ way to achieve this workflow. You might
have heard of the "Boyd's Law of Iteration" which states that the speed of
iteration beats the quality of iteration. I find that it applies really well to
learning. It works well while dealing with code. I can still remember pretty
vividly the time I discovered Ruby had IRB. It made the experience of learning
the language much more satisfying. I was learning really fast because I could
try many things and get instant feedback.

How does this play with approaching an existing codebase? The idea is that you
setup your own "REPL experience". The actual tool you use for this doesn't
really matter, it can be TDD but it doesn't have to be. The core of this
principle is that you have your own "REPL experience" set up _before_ you change
any code. That way, you can change very little and immediately get feedback from
the system.

The context of this principle is focused programming sessions but you can apply
this principle also to end result of good programming sessions: deployable
artifacts. There's [good
evidence](https://www.goodreads.com/book/show/35747076-accelerate) now that
smaller changes lead to higher productivity. They're safer to deploy and
rollback if needed. They're also easier to review and more likely to move fast
to production.

We can derive at least two different conclusions from this principle:

- Fixing a few bugs one after the other, possibly in different part of the
  system, is a great way to approach a codebase that's new to you.
- Breaking down any change into smaller changes with their own feedback loop is
  a great way to approach changing a large codebase you're familiar with.

The reason why I'm not saying "just do TDD" is two-fold:

- TDD feels great as a workflow. It perfectly resembles what I call the "REPL
  experience". But I always had problems with the design constraints it brings.
  I want the first draft of the code I'm writing to reflect the mental model of
  my current understanding of the problem I'm trying to solve. TDD always forced
  me to shift my mental model to make the code more testable upfront. That
  friction between what I want to write and what TDD needs me to write had
  always a negative effect on my ability to design a change effectively.
- TDD is a development technique. Its artifact is code. We ship features though
  so the code we write, even if perfectly tested, doesn't work unless it works
  in production. I want more than TDD from my "REPL experience". Hence the next
  principle.

### Test in production

I can hear [@mipsytipsy](https://twitter.com/mipsytipsy) in my head say "fuck
yeah!" every time I mention this principle to people. The simplest way to put it
for me is that you want to expand the boundaries of your "REPL experience"
enough to include your production systems. Once again, it's all about the speed
of iteration. It's not enough to have a tight feedback loop while making the
smallest possible change if you don't ship it to production in _minutes_ and
know if it works or not. You can put a lot of effort writing "correct" code but
unless it works in production, it doesn't work yet.

The urgency of shipping code to production helped me create an healthy
development culture in many teams. You want your "REPL experience" to include
production? You're going to need fast CI and CD pipelines. You're going to need
ways to observe the impact of changes on your production systems. All of this
contributes to your actual speed of iteration. You want to move fast, but you
don't need to break things. Well you will break some things, it's part of the
deal. We _all_ test in production when we ship code anyway so the point here is
that doing it consciously creates a safer, faster environment for development.
Including your production systems in your "REPL experience" will unlock a lot of
potential.

Testing in production and small changes feed on each other. The faster your lead
time to production, the more likely you'll ship smaller changes. The smaller the
change you ship, the safer your deployments.

### Be a gardner

The idea is cute: code gardening is the act of changing code with the goal of
improving it just a little. Improve an error message, use a more idiomatic way
to write those three lines, align that test with your internal conventions. The
codebase is living thing: it's a garden and you take care of it every day. You
prevent bad habits from forming, you heal that system that is a bit sick. My
first encounter with this analogy is a
[commit](https://github.com/rails/rails/commit/fb6b80562041e8d2378cad1b51f8c234fe76fd5e)
made by [@fxn](https://twitter.com/fxn). It stuck with me ever since.

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

Being a gardner is easier if you're already making very small changes _and_
you're testing in production. The joy of improving your codebase is often
limited by the cost of bringing these tiny little changes to production. The
higher the cost (in terms of time mostly) of a deployment the least likely
you'll want to be a gardener.

### Read features end to end

The principles I shares so far are aimed at approaching a change in a large
codebase. They help also when said codebase is new to us. There's something
though I do _before_ making any change to a code base. It's often called code
safari but I'm no big fan of the metaphor. I like "read features end to end"
more. When you approach a new codebase, it can get a little overwhelming.
Sometimes you don't even know where to start, which part of the system maps to
which part of the codebase. To help orient yourself, you can read a whole
feature end to end. Let's consider a large web application where the "innocent"
click on a button stars a batch process which ultimately results in the customer
getting an email report of some sort. I'm sure you have seen some variation of
this.

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

This is the corresponding principle of the "small changes, fast feedback loop"
one for existing codebase. A variation of
[principle](https://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast) often
attributed to [Kent Beck](https://twitter.com/KentBeck). It's almost the same
idea but the dynamics are a little different: there's obviously more freedom of
movement if you're writing a new system. That freedom is often detrimental to
making small changes. Often you can't change a few lines, you need to write a
whole bunch of code across multiple files to even see your little prototype
work. It's the nature of the business of writing code from scratch. It's a
mental model and a workflow to help you stay focused and be productive. Here's
the idea:

- Make it work: you REPL your way through code to figure out if what you put
  together works. You don't overthink it. It doesn't matter the naming isn't
  good yet. It's OK you know the code isn't production ready. You can get away
  without dealing with corner cases, failure modes, error codes from that third
  party API. You notes things down, leave TODOs for tomorrow's you. That's a
  deliberate choice. Your goal is to get your code together so that it works.
  You don't have to make it good. You don't have to ship it to production in a
  minute.
- Make it good: you go through the notes and comments you left for yourself when
  you made it work. One by one you tick things off. You step back a little,
  evaluate the design of a new system. You're making it good. You want to get
  down to the little details, you need to be happy with the design of the
  system. The way the code is organized. You care about everything but speed.
  You're not trying to write slow code by design (that wouldn't help) but you're
  also not trying to squeeze every millisecond off your code.
- Make it fast: it's time to put your code to benchmark. Now, to be fair, this
  isn't always a required step. Often enough, code that works and you don't hate
  is quite good already. On the other side of the spectrum, the are situations
  in which code needs to be really fast. In a way, all I said in the previous
  two points still applies in this scenario. What changes is your definition of
  working. You incorporate speed into it and design your "REPL experience" to
  always tell you about how your code is performing. Either way, when you're at
  this step the process is simpler than most people want to make it. The most
  basic workflow goes really far: profile your code, find the slowest bit, make
  it faster, repeat until you're happy.

I have lived by this principle all my career a I've been lucky enough to run
into it a long time ago. Over the years, this principle was the driving force
behind my choices of programming languages. I have a number of reasons to enjoy
working exclusively with statically typed languages, probably enough to write
its own article (interested? Please, let me know!) but this principle plays a
surprisingly big part in this. The ergonomics of statically typed languages
allow me a much more relaxed approach to the make it work phase. I genuinely
don't need to care how the code is organised or their naming. Modern IDEs are so
good at refactoring code that I can do a big chunk of the make it good part in
seconds. It's somewhat paradoxical: I often chose languages like Ruby because of
the supposed productivity gains only to realise I got stuck in early stages of a
new project because of the fear of needing too much time to make naming good..

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

Let's clarify what I mean with "actual test". Here's my definition:

> A test must change only if the behaviour it verifies changed

No other reason is good enough for a test to change. I've seen tests testing
language features, or testing missing language features (looking at you
dynamically typed languages), or even testing framework features. These tests
have two things in common: they change all the time and they're just baggage. My
advice is to delete them. There, I said it.

When I bring this up, people often understand it as my advice is either to not
write tests or to only have integrations tests. Obviously, no point in debating
the first one. I'm not arguing if you should have tests or not, I'm arguing the
effectiveness of most tests suites. I don't know, there's a chance I've been
very unlucky in my career and I run into too many bad test suites. As for the
second point, it's that integration tests are more likely to suit my definition
of "actual test".

### Less dependencies is better

### Write more docs

### Confidence is a struggle

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart

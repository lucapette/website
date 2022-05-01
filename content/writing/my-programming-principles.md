---
title: "My Programming Principles"
description: "What guides me after 20 years of writing code for a living"
date: 2022-04-17T14:12:13+02:00
draft: true
keywords:
  - programming
  - principles
---

{{< message class="is-info">}} This is a _long_ read. Right after the
introduction there's a table of content in case you don't want to read it all.
 {{</ message >}}

One day, when asked how he had prepared for a game he had just played, Alexander
Grischuk, an elite chess Grandmaster, answered quickly:

> "I've been preparing my whole life". GM Grischuk, Alexander

The beauty of his answer is that you can apply it to _any_ job. _All_ your past
experiences contribute to some extend to _any_ decision you make today. 

The parallel with programming felt obvious to me and , since I watched that
interview with Grandmaster Grischuk, I have been subconsciously collecting
guidelines, ideas, and principles that stick with me over the years. Writing
about it now is the natural consequence of my desire to learn more about these
principles (and myself). 

I have to share I find the title of the article a little pretentious but I
decided to go with it because it fits well the ideas expressed here.

As for the actual principles, It felt natural to group them into these
categories: existing codebases, greenfield projects, and general principles. 

I believe in approaching coding sessions differently depending on the context.
While this may sound a tad obvious, I encountered enough examples of
over-engineered or under-engineered (why isn't that a thing? It's almost as
common!) solutions in my career. More often than not, these wrongly sized
solutions were merely a reflection of approaching an existing codebase like a
greenfield project or, maybe worse, the other way around.


## Table of content <!-- omit from toc -->
- [Table of content](#table-of-content)
- [How I approach an existing codebase](#how-i-approach-an-existing-codebase)
  - [Small changes, fast feedback loops](#small-changes-fast-feedback-loops)
  - [Test in production](#test-in-production)
  - [Be a gardner](#be-a-gardner)
  - [Read features end to end](#read-features-end-to-end)
  - [Facts > Assumptions](#facts--assumptions)
- [Starting from scratch](#starting-from-scratch)
  - [Docs driven development](#docs-driven-development)
  - [Throw it away first](#throw-it-away-first)
  - [Make it work, make it good, make it fast](#make-it-work-make-it-good-make-it-fast)
- [General principles](#general-principles)
  - [Prose not poetry](#prose-not-poetry)
  - [Name things what they do](#name-things-what-they-do)
  - [Write the code you'd like to use](#write-the-code-youd-like-to-use)
  - [Let the design emerge from the code](#let-the-design-emerge-from-the-code)
  - [Write actual tests](#write-actual-tests)
  - [Balance your confidence](#balance-your-confidence)

## How I approach an existing codebase

Here I'm thinking of codebases that are large enough you can't possibly keep an
accurate representation of all its parts in your head. You may know some parts
very well but there are enough unknowns you wont't be able make a non-trivial
change without some careful planning. 

What's the definition of large enough is probably too personal so I won't even
try to give an example. We'll go with "large enough for you, the reader". Let's
dive into it.

### Small changes, fast feedback loops

You might have heard of the "Boyd's Law of Iteration" which states that the
speed of iteration beats the quality of iteration. I find that you can apply
this law to many aspects of writing code. This principle in particular is an
embodiment of the idea that speed beats quality often enough.

In short: change a few lines _or less_ before seeking feedback. I mean
_literally_ the smallest possible change you can think of. I've been in many
pair programming sessions where my partners were surprised to see how little I
would change before I sought feedback. 

In practice, it doesn't happen very often the whole change we have to make to a
system is a just couple of lines. It's often a little more than that, the change
will possibly cover a few files. In these situations, I stick with my "small
changes, fast feedback loops" workflow:

- Change very little. A few lines _or less_
- Seek feedback
- If it works _so far_, repeat

The key is that _so far_. I create checkpoints where I can assume all the code I
wrote so far works. As soon as what my feedback loop is telling is not what I
expect, I go back "one checkpoint". Often, I'm pretty radical about it. I just
delete the last change and start over.

You may be asking yourself: "hey, isn't this just TDD?" and the answer is "yes
and no". TDD is _one_ way to achieve what I call the "REPL experience". 

I remember pretty vividly discovering IRB, the interactive Ruby shell. It made
the experience of learning the language quicker and much more satisfying. I was
learning really fast because trying things out and check the answer was so much
faster than trying to work out in my head if Ruby could do that.

How does this play with approaching an existing codebase?  The core of this
principle is that I  set up my own "REPL experience" _before_ I change any code.
The actual tool doesn't really matter, it can be TDD, it can be print statements
(yep, I just said that), whatever. Once that experience is possible, I can start
changing things, a few lines _or less_ per time.

The most common scenario I'm thinking of for this principle is focused
programming sessions but you can apply it also to end result of good programming
sessions: deployable artifacts. There's [good
evidence](https://www.goodreads.com/book/show/35747076-accelerate) now that
smaller changes lead to higher productivity. They're safer to deploy and
rollback if needed. They're also easier to review and more likely to move fast
to production.

Before I move on to the next principle, I feel like I should expand on why I'm
not saying "just do TDD". Here we go:

- TDD feels great as a workflow. It perfectly resembles what I call the "REPL
  experience". But I always had problems with the design constraints it brings
  to the table. I want the first draft of a solution to reflect the mental model
  of my understanding of the problem I'm trying to solve. TDD always forces me
  to shift my mental model a bit to make the code more testable _upfront_. While
  there's nothing wrong with making code more testable, my problem is doing that
  when I'm discovering by writing code how well my mental model translates into
  actual code. The friction between what I want to write and what TDD needs me
  to write always has a negative effect on my ability to design a change
  effectively. In practice, I use TDD only to fix bugs.
- TDD is primarily a development technique. Its artifact is code. We ship
  features though, code is just a means to an end. So the code we write, even if
  perfectly tested, doesn't work unless it works in production. I want more than
  TDD from my "REPL experience", I want to [test in
  production](#test-in-production).

Practical considerations:

- Fixing a few bugs one after the other, possibly in different part of the
  system, is a great way to approach a codebase that's new to you.
- Breaking down any change into smaller changes with their own feedback loop is
  a great way to approach changing a large codebase you're familiar with.

### Test in production

I can hear [@mipsytipsy](https://twitter.com/mipsytipsy) in my head say "fuck
yeah!" every time I mention this principle. The way I like to put it is that I
want to expand the boundaries of my "REPL experience" far enough so to include
production systems. Again, it's all about the speed of iteration. I'm not
satisfied with a tight feedback loop while writing the smallest possible change.
I want to ship it to production in _minutes_. What's the point of putting a lot
of effort writing "correct" code if we don't know if it works in production?

The urgency of shipping code to production helped me create an healthy
development culture in many teams. We want your "REPL experience" to include
production? We're going to need fast CI and CD pipelines. We're going to need
ways to observe the impact of changes on your production systems. Rollback has
to be cheap. We may want to use feature flags. All of it contributes to the
actual speed of iteration. We _all_ test in production when we first ship the
code _anyway_ so it makes much more sense to do it consciously to create a
safer, faster environment for feature development.

Testing in production and small changes feed on each other. The faster your lead
time, the more likely you'll ship smaller changes. The smaller the change you
ship, the safer your deployments. The safer your deployments... you get it, it's
a circle!

### Be a gardner

Code gardening is the act of changing code with the goal of improving it just a
little. You improve an error message, you use a more idiomatic way to write
those three lines, you align that test with your internal conventions. The
codebase is living organism: it's a garden and you take care of it every day.
You prevent bad habits from forming, you heal that spot that is a bit sick, you
give more water to the plants that need it the most. My first encounter with
this analogy is a
[commit](https://github.com/rails/rails/commit/fb6b80562041e8d2378cad1b51f8c234fe76fd5e)
made by [@fxn](https://twitter.com/fxn). It stuck with me ever since.

I used to do gardening _while_ writing new features. I grew out of it because
smaller changes are always better. You could say that small changes is more
important principle. Now I have a small workflow that structures my gardening
activities. I take notes of the gardening I want to do while whenever I can.
Sometimes, I share it with my team (it really depends on the team and the
context they're in) to help other people form the habit of code gardening. I
then use this todo list as a personal reward system. I get done what I need to
and then I treat myself by improving that test that produces those annoying
warnings. I have seen this dynamic at play with other programmers as well once
the habit is formed. The reward system is simple and effective, it allows people
to make small improvements _all the time_ and just feel good about it.

Being a gardner is easier if you're _already_ making very small changes _and_
you're _already_ testing in production. The joy of improving your codebase is
often limited by the cost of bringing these tiny little changes to production.
The higher the cost of a deployment the less likely you'll be a good gardener.

### Read features end to end

There's something I do _before_ making a change to a large code base that is new
to me. It's often called code safari but I'm no fan of the metaphor, I like to
call things what they do so the principle goes by "read features end to end".
Yeah, it's boring. I like it that way.

Approaching a new codebase can get a little overwhelming. Sometimes I don't even
know where to start, which part of the system maps to which part of the
codebase. To help orient myself, I read a whole feature end to end. Let's
consider a large web application composed of multiple services where an
"innocent" click on a button stars a series of processes which ultimately result
in the customer getting an some report via email. I'm sure you have seen a
variation of this. 

The idea of this principle is that I go on a hunt. I start searching for the
code of the button. It sounds trivial but I know I will discover things: how/if
we do i18n, what kind of frontend framework we're using. Most importantly, I
will notice things (oh we have our own css framework? Why?), I will have
questions (what's up with all data attributes? I haven't seen any of that when I
was checking out the website from home. We're stripping them down?). I open a
notebook and write everything down. I know that using my brain as storage is a
waste: ["the shorter pencil is longer than the longest
memory"](https://www.youtube.com/watch?v=vIW72VXMPHo&t=344s). Then when I know
which service reacts to the click, I go down one layer and do the same again. I
will repeat the process until I've gone all the way to the email service that
sent out that test email right into my inbox.

Once I'm done, I'll have a mixed bag. That's where the hard work starts. I
reorganize notes:

- Questions for myself. A sort of todo list of things I know I want to dig in
  personally.
- Questions for those who know the codebase better than I do and can get me up
  to speed faster.
- Idioms. Every codebase has its ways of doing things.

You now have a basic map I can use to choose a new path and repeat this process
to improve my understanding of the codebase I just started working on.

### Facts > Assumptions

- debugging (gather facts about behaviour)
- perf improvement (measure, don’t guess)

## Starting from scratch

Since it's a little less common, I feel privileged (or unlucky? It depends on
the day honestly) having experienced multiple times the scenario "hey you're
responsible for building this X years long project from scratch. Also, there's
no team yet so you need hire a bunch of people to do it". When you start a new
project, it can feel a little overwhelming because, of course, there's nothing
done yet 😃

### Docs driven development

Writing the docs before writing a single line of code helps with:

- scope
- organization
- prioritization

### Throw it away first

Designing a system is a struggle between tiny details and high level ideas. It's
the reason why the quote "in theory, there's no difference between theory and
practice. In practice, there is" is so apt for programming. The harder is the
problem you're trying to solve, the stronger is this tension. You want to make
sure it's a positive tension. Here's what I do:

- I write a small prototype
- I write a chunk of the design doc
- I throw away the first prototype
- I improve the design doc
- I write a new prototype

Sometimes, if the problem is really hard, I repeat this process a couple of
times. Others it's too easy and there's no need to throw it away. The point is
be ready to do so when you're not happy with how you understand the problem.
That's the core of this principle: you write a first draft knowing you'll
probably throw it away so you'll experiment. You'll digress a little, you'll
come back. The idea is that you use code and words to improve your understanding
of the problem.

My favourite programming sessions always happen after I've throw away a first
version of whatever I'm trying to solve. These sessions feel really fast and
they are. I'm kind of just typing out a satisfying solution at this point.

### Make it work, make it good, make it fast

This is the corresponding principle of the "small changes, fast feedback loop"
one for existing codebase. A variation of
[principle](https://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast) often
attributed to [Kent Beck](https://twitter.com/KentBeck). It's almost the same
idea but the dynamics are a little different: there's obviously more freedom of
movement when writing a new system. That freedom is often detrimental to making
small changes. Often I can't change a few lines, you need to write a whole lot
of code across multiple files to even see my little prototype work. It's the
nature of the business of writing code from scratch. So here's the idea:

- Make it work: I REPL my way through code to figure out if what I put together
  works. I don't overthink it. It doesn't matter the naming isn't good yet. It's
  OK, I know the code isn't production ready. Right now, I can get away without
  dealing with corner cases, failure modes, error codes from that third party
  API. I note things down, leave TODOs for tomorrow's Luca. It's a deliberate
  choice: my goal is to get the code together so that it works. I don't have to
  make it good. I don't have to ship it to production in a minute. I don't have
  a production system yet.
- Make it good: I go through the notes and comments I left for yourself when I
  made it work. One by one I tick things off. I step back a little, evaluate the
  design of a new system. Now Iam making it good: I want to get down to the
  little details, I need to be happy with the overall design. The way the code
  is organized has to make sense. I care just about everything but speed. I am
  not trying to write slow code by design (that wouldn't help and is probably
  harder than it sounds) but I am also not trying to squeeze every millisecond
  off of it.
- Make it fast: it's time benchmark the cod. Now, to be fair, this isn't always
  a required step. Often enough, code that works and I don't completely hate is
  quite good already. On the other side of the spectrum, the are situations in
  which code needs to be really fast. In a way, all I said in the previous two
  points still applies. What changes is the definition of working. I incorporate
  speed into it and design my "REPL experience" to always tell me about how fast
  the code is. Either way, when I am at this step the most basic workflow goes
  really far: profile the code, find the slowest bit, make it faster, repeat
  until Iam happy.

Over the years, this principle has been a major factor guiding me to my next
programming languages. A concrete example:  nowadays, I have a number of reasons
to enjoy working exclusively with statically typed languages, probably enough
reasons to write an article about it (interested? Please, let me know!). The
ergonomics of statically typed languages allow me a much more relaxed approach
to the make it work phase. I really don't need to care too much about how the
code is organised or its naming. Modern IDEs are so good at refactoring code
that I can do a big chunk of the make it good part literally in seconds. Looking
back on this, it feels somewhat paradoxical: I often chose languages like Ruby
because of the supposed productivity gains only to realise I got stuck in early
stages of a new project because of the fear of needing too much time to refactor
things.

## General principles

Is it really programming if there isn't at least some util functions? 😃

Jokes aside, there is an handful of principles that I apply in any context.
These are my "true north" guidelines so to speak.

### Prose not poetry

Writing code is, well, writing. It's an intriguing form of writing because
you're always writing for the machine first and then for the next programmer.
Two audiences that couldn't be any more different.

The primary audience is the machine even though we often lie to ourselves and
say things like "more than anything you're writing for the next programmer that
reads your code". I'm sympathetic with the argument because I see where it comes
from but, let's be honest about it, you can't ship code that doesn't compile. It
doesn't matter how readable and understandable the code you wrote is for humans,
it won't work if you got a syntax error. You have to write code that the machine
can run. But, on the other hand, you can't discount the fact the code will need
to be modified at some point in the future so other people can will read it.
It's a tough game and this principle has been guiding me since I first realized
that writing code is still writing.

What's the most fitting writing form for code? It's prose. You want your code to
have chapters, your chapters to have paragraphs, your paragraphs to have
sentences. The key is that you want _homogeneous levels of abstraction_.
Sentences are just a few instructions, paragraphs are little algorithms,
chapters are modules, a book is a system. That's why I say that good code
resembles prose. Good code has a boring rhythm, you don't want to jump from
shifting bits to call an external service in two consecutive lines. You're
structuring your code this way for _both_ the machine and the next programmer.

When I bring up this prose vs poetry metaphor, I often get the argument that
poetry may fit as well. After all there's very structured poetry out there. The
thing with poetry though is that it's often good when it says a lot with just a
few words. Poetry can be really terse and that's why I say prose not poetry.
When in doubt, always _prefer clarity to brevity_. Since it's all the same for
the machine, there's no point in making code terse or clever for the next
programmer.

### Name things what they do

Naming _is_ hard.

Having said that, I think we often make it harder for ourselves than it should
be because there's a certain social pressure to treat code like poetry. It has
to be pretty so we're looking for elegant, apt, short names for everything. I
struggled early in my career with this but I grew out of it because I realized a
few things:

- Unless it's a trivial problem, I won't likely get naming right while I'm
  drafting a first version.
- Naming is the most significant design tool at my disposal white I'm writing.
  Due the focused nature of such tasks, sometimes I lose sight of that on a
  conscious level while writing. Calling something, say, `JobScheduler` will
  constraint the design of whatever interact with it to treat it like a job
  scheduler right? Right, it sounds obvious.
- Way too often I want to get the naming right too early. The delta between the
  mental model I created for the problem I want to solve and the actual code I
  will end up writing decreases as I write it.

All these considerations lead me to what I call "relaxed naming" (ah the irony
of not liking this naming 🙃). In a way, I'm bound to get some names wrong the
first time around. But it's effect of the problem. The cause is that I don't
understand the problem yet well enough to be satisfied with the naming. "Relaxed
naming" is my process now and it looks like this:

- Call things what they do _right now_. The more descriptive the name, the
  better. Bonus points if it feels a little boring.

Yep, there's no step two.

It's a little cheeky I'll give you that. But all things considered, after 20
years I'm finally happy with my naming process. I look at it the same way I look
at jotting down a first version of a new system. It's obvious maybe but spelling
it out helps: the thing you're naming is _always new_. You're naming it now
because it's not there yet. So the principle [make it work, make it good, make
it fast](#make-it-work-make-it-good-make-it-fast) works pretty well here except
there's no step three.

### Write the code you'd like to use

I can't find the original reference but I'm relatively sure this principle too
was inspired by Kent Beck. This works well when you get stuck because you have
no code to start from. So you have all kind of questions about how the API of
this new module should look like, how should the data structures look like, and
so on. It can get so overwhelming you can get stuck.

That's where you take a deep breath and ask yourself: how do I want to use this
code? Here's how this principle works in practice:

- Close that empty file you just created.
- Open up one file in which you need to call the code you don't have yet.
- Write the code like the problem you're solving is _already solved_ the best
way possible.

The third step is hard because you have to fake you don't know the code isn't
there. This only works if you genuinely jot down a solution without trying to
also picture all the code you need to write to make it all work. Sometimes, I go
as far as using pen and paper and pseudo-code something as a first step so I can
let go of weight of all the sub-tasks each line I'm "faking" is generating.

### Let the design emerge from the code

When I started as head of engineering at [Marley
Spoon](https://marleyspoon.com/), I hang on the walls of our office a few A4
pages with one or two sentences of them. The idea was to remind myself and my
team of our foundational core values. My favourite is relevant to this
conversation:

> Less is more

What I'm trying to say with it in the context of design and code is that good
design is first of all an exercise in patience. Also, I hate to tell you this,
the more experienced you become the harder it gets. You know too well the few
lines of code you just wrote to "make it work" aren't anywhere close to
production quality.

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
the first one. Of course you should write tests. I'm not arguing if you should
have tests or not, I'm arguing the effectiveness of tests suites. As for the
second point, it's true that integration tests are more likely to suit my
definition of "actual test" so I understand where people are coming from with
this.

### Balance your confidence

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart



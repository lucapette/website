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

Being a gardner is easier if you're already making _small changes_ and you're
already _testing in production_. The joy of improving a codebase is often
limited by the cost of bringing these tiny changes to production. The higher the
cost of a deployment the less likely I can be a good gardener. It's all
connected.

### Read features end to end

When I'm new to a large codebase, I read features end to end _before_ making any
change. People often called code safari but I'm no fan of the metaphor, I like
to call things what they do so the principle goes by "read features end to end".
Yeah, it's boring. I like it that way.

Approaching a new codebase can get a little overwhelming. Sometimes I don't even
know where to start, which part of the system maps to which part of the
codebase. To help orient myself, I read a whole feature end to end, it doesn't
even have to be connected to a change I need to make.

For the sake of discussion, let's consider a web application composed of
multiple services. There's an "innocent" button that, once clicked, stars a
sequence of processes which ultimately result in getting some PDF report via
email. I'm sure you have seen a variation of this.

The idea is that I go on a hunt. I start searching for the code of the button.
It sounds trivial but I know I will discover things: how/if we do i18n, what
kind of frontend framework we're using. Most importantly, I will notice things like:

- "oh we have our own css framework? Why?"
- "what's up with all data attributes? I haven't seen any of that when I was
checking out the website from home. We're stripping them down?"

Before I forget any of this, I open a notebook and write everything down. Using
my brain as storage is a waste: ["the shorter pencil is longer than the longest
memory"](https://www.youtube.com/watch?v=vIW72VXMPHo&t=344s). When I figured out
which service reacts to the click, I go down one layer and keep reading. I
repeat the process until I've gone all the way to the email service that sent
out that test email right into my inbox.

Once I'm done, I have a mixed bag of notes: that's where the hard work starts. I
reorganize them:

- Questions for myself. A sort of todo list of things I know I want to dig in
  personally.
- Questions for those who know the codebase better than I do and can get me up
  to speed faster.
- Idioms. Every codebase has its ways of doing things.

I now have a basic map I can use to orient myself. I often repeat the process a
few times before I start making any change.

### Facts > Assumptions

- debugging (gather facts about behaviour)
- perf improvement (measure, don’t guess)

## Starting from scratch

Since it's a little less common, I feel privileged (or unlucky? It depends on
the day honestly) having experienced multiple times the scenario "hey you're
responsible for building this X years long project from scratch. Also, there's
no team yet so you need hire a bunch of people to do it". The freedom to do
"whatever you want" because there's no existing system to constraint your
choices is a double-edged sword. The following principles help me navigate
uncharted territories with confidence.

### Docs driven development

More than a decade ago, I run into [readme driven
development](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html).
I was happy to see I could give a name to something I had started doing a couple
of years before. Since then, I tweaked the name of the principle to a more
generic "docs driven development" because it's a little more general and
flexible. The idea is to write documentation as you already had finished the
system. It is the system design equivalent of writing [the code you'd like to
use](#write-the-code-youd-like-to-use).

Writing the docs before writing a single line of code is a great way to shade
some light on the problem you're trying to solve. The beauty, and the irony, of
this process is that I write the docs first so I can create some constraints for
myself. Good, consciously chosen constraints are the foundation of any design.

What kind of document, how long or how detailed it should be is too
project-dependent so it's hard to provide concrete examples. There are some
things though I always want to cover:

- __Scope__
  : I write down what the goals of the project are. When it's done,
  what will this system do? Even more interesting sometimes, what won't?
  Answering these questions draws some boundaries so it becomes a little easier
  to stay on track and not to get lost.
- __Organization__
  : A few diagrams go a long way. I draw some to get a sense of how to split
  responsibilities between among the different parts. What the highest level
  building block of the system should be.
- __Prioritization__
  : I write down the order in which I imagine it makes most
  sense to build things. Depending on how the system is organized, I write down
  what can be parallelised.


If I'm working in a team, this document also functions as a hub for async
collaboration. I want to make sure we're on the same page before we move from
writing for humans to writing for machines.

### Throw it away first

When I think about prototyping phase, one of my favourite quotes always comes to
mind:

> In theory, there's no difference between theory and practice. In practice,
> there is.

It is so apt for programming! It doesn't matter how carefully you study a
problem, how good the solution you came up with seems to me. Doing is always
different. Designing a system is a struggle between tiny details and high level
ideas. The harder is the problem you're trying to solve, the stronger is this
tension. You want to make sure it's a positive tension. Here's what I do:

- I write a chunk of the design doc
- I write a prototype
- I throw it away
- I improve the design doc
- I write a new prototype

If the problem is really hard, I may repeat this process a number of times of
times. If the problem is trivial, I may get away without throwing it away. The
point is I'm ready to do so when I'm unhappy with my understanding of the
problem. Please note: I didn't say anything about what I think of the code. My
harsh truth is that the best case scenario is that I don't hate it today and I
may still not totally hate it in a few months.

The key of this principle is this: I write a first draft knowing I will probably
throw it away. With that in mind, I experiment, I digress a little, then I go
back a little and try again. I write code for the machine and words for humans
to improve my understanding of the problem.

My favourite programming sessions always happen after I've throw away at list
one version of whatever I'm trying to solve. These sessions are really fast: I'm
kind of just typing out a satisfying solution.

### Make it work, make it good, make it fast

This principle is a variation of the theme of the
[principle](https://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast) often
attributed to [Kent Beck](https://twitter.com/KentBeck). Clearly I'm so
pretentious about it, I like my version better.

I consider this the corresponding starting from scratch principle of [small
changes, fast feedback loops](#small-changes-fast-feedback-loops) It's almost
the same idea but the dynamics are a little different: there's obviously more
freedom of movement when writing a new system. That freedom is often detrimental
to making small changes. Often I can't change a few lines even if I wanted to: I
need to write a whole lot of code across multiple files to even see my little
prototype do some basic things. It's the nature of writing code from scratch. So
here's the idea:

- __Make it work__
  : I REPL my way through code to figure out if what I put together
  works. I don't overthink it. It doesn't matter the naming isn't good yet. It's
  OK, I know the code isn't production ready. Right now, I can get away without
  dealing with corner cases, failure modes, error codes from that third party
  API. I note things down, leave TODOs for tomorrow's Luca. It's a deliberate
  choice: my goal is to get the code together so that it works. I don't have to
  make it good yet. I don't have to ship it to production in a minute. Well I
  don't even have a production system yet.
- __Make it good__
  : I go through the notes and comments I left for myself. One by
  one I tick things off. Every two or three items I get off the list, I step
  back a little. I evaluate the design. Now I am making it good: I want to get
  down to the little details, I need to be happy with the overall design. The
  way the code is organized has to make sense. I care just about everything. I
  only leave out speed. I am not trying to write slow code by design (that
  wouldn't help and is probably harder to do than it sounds?) but I am also not
  trying to squeeze every millisecond off every function.
- __Make it fast__
  : it's time to measure things. To be fair, this isn't always a
  required step. Often enough, code that works and I don't completely hate is
  quite good already for whoever is paying for it. On the other side of the
  spectrum though, the are situations in which code needs to be really fast by
  design. In a way, the previous two steps of this process still apply. What
  changes is the definition of working. I incorporate speed into my definition
  and design my "REPL experience" to always tell me about how fast the code is.
  When performance is critical, I check in with the rest of the codebase the
  scripts I write for my "REPL experience" so that they can be part of the CI
  pipeline. Either way, when I am at this step the most basic workflow goes
  a long way: profile the code, find the slowest bit, make it faster, repeat
  until happy.

Over the years, I realized that following this principle has had an interesting
side-effect. It has been a major factor into the way I choose the next
programming language. A concrete example: nowadays, I enjoy working exclusively
with statically typed languages (probably enough reasons to write an article
about it... interested? Please, let me know!). The ergonomics of statically
typed languages allow me a much more relaxed approach to the make it work phase.
I really don't need to care too much about how the code is organised or its
naming. Modern IDEs are so good at refactoring code that I can do a big chunk of
the make it good part literally in seconds. Looking back on this, it feels
somewhat paradoxical: I often chose dynamically typed languages (my favourite
being Ruby) because of the supposed productivity gains only to realise I got
stuck in early stages of a new project because of the fear of needing too much
time to refactor things.

## General principles

Is it really programming if I don't have at least some util functions? 😃

Jokes aside, there is an handful of principles I apply in any context. These are
my "true north" guidelines so to speak.

### Prose not poetry

Writing code is, well, writing. It's an intriguing form of writing because
you're always writing for the machine first and then for the next programmer.
Two audiences that couldn't be any more different.

The primary audience is the machine even though we often lie to ourselves and
say things like "more than anything you're writing for the next programmer that
reads your code". I'm sympathetic with the argument because I see where it comes
from but, let's be honest about it, you can't ship code that doesn't compile. It
doesn't matter how readable and understandable the code you wrote is, it won't
work if you got a syntax error. You have to write code so that the machine can
run it. But, on the other hand, you can't discount the fact the code will need
to be modified at some point in the future so other people can will read it.
It's a tough game and this principle has been guiding me since I first realized
that writing code is "just" writing.

I think prose fits better with good production quality code. I want my code to
have chapters, my chapters to have paragraphs, my paragraphs to have sentences.
I want _homogeneous abstraction at each level_. Sentences are a few
instructions, paragraphs are little algorithms, chapters are modules, a book is
a system. The parallel doesn't need to be this exact, the point is that I don't
want to jump from shifting bits to call an external service in two consecutive
lines. That's why I say that good code resembles prose. Good code has a boring
rhythm. I am structuring your code this way for _both_ the machine and the next
programmer.

The one notable exception to writing prose is performance. Often making code
faster also makes it messier and uglier because you'll see that the abstraction
isn't homogeneous any longer.

When I bring up this prose vs poetry metaphor, I often get the argument that
poetry fits as well. After all there's very structured poetry out there and it's
often as cryptic as code can be. The thing with poetry though is that it's good
when it says a lot with just a few words. Poetry can be really terse and that's
why I say prose not poetry. When in doubt, always I _prefer clarity to brevity_.
Since it's all the same for the machine, there's no point in making code terse
or clever for the next programmer.

### Name things what they do

Naming _is_ hard.

Having said that, I think we often make it harder for ourselves than it should
be because there's a certain social pressure to treat code like poetry. It has
to be pretty so we're looking for elegant, apt, and short names for everything.
I struggled early in my career with this but I grew out of it when I realized a
few things:

- Unless it's a trivial problem, I will not get naming right while I'm drafting
  a first version.
- Naming is the most significant design tool at my disposal while I'm writing.
  Due the focus required to write code, sometimes I lose sight of that on a
  conscious level. Calling something, say, `JobScheduler` will constraint the
  design of whatever interact with it to treat it like a job scheduler and maybe
  it's not.
- Way too often I want to get the naming right too early. The delta between the
  mental model I created for the problem I want to solve and the actual code I
  will end up writing only decreases as I write it.

All these considerations lead me to what I call "relaxed naming" (ah the irony
of not liking this naming 🙃).I'm bound to get some names wrong the first time
around. That's an effect of the problem. The cause is that I don't understand
the problem yet well enough to be satisfied with the naming. "Relaxed naming"
looks like this:

- Call things what they do _right now_. The more descriptive the name, the
  better. Bonus points if it feels a little boring.

Yep, there's no step two.

It's a little cheeky I'll give you that. But all things considered, it only took
me 20 years to be happy with my naming process. I look at it the same way I look
at jotting down a first version of a new system.  Spelling out obvious things
sometimes helps: the thing I'm trying to name is _new_. I am naming it now
because it's not there yet. So the principle [make it work, make it good, make
it fast](#make-it-work-make-it-good-make-it-fast) works pretty well here except
there's no step three.

### Write the code you'd like to use

I can't find the original reference but I'm relatively sure this principle too
was inspired by Kent Beck. It works well when I get stuck because I have no code
to start from. It's the proverbial blank page I'm looking at. So I have all kind
of questions about how the API of the new module should look like, which data
structures I should rely on, what types should I expose, and so on. It can get
so overwhelming I just get stuck.

That's where I take a deep breath and ask myself: how do I want to use this
code? Here's what I do:

- I close that empty file I was staring at.
- I open up one file in which I need to call the code I can't get myself to
  write.
- I write the code like the problem is _already solved_ the best way I can
  possibly imagine.

The third step is hard because I have to lie to myself, I know the code isn't
there. This only works if I genuinely jot down a solution without trying to also
picture in my head all the code I need to write to make it all work. Sometimes,
I go as far as using pen and paper and pseudo-code something as a first step so
I can let go of weight of all the sub-tasks each line I'm "faking" is
generating.

### Let the design emerge from the code

When I started as head of engineering at [Marley
Spoon](https://marleyspoon.com/), I hang on the walls of our office a few A4
pages with one or two sentences of them. The idea was to remind myself and my
team of our foundational core values. My favourite is relevant to this
conversation:

> Less is more

What I'm trying to say with it in the context of design and code is that good
design is first of all an exercise in patience. Also, I hope it's not the same
for you, the more experienced I become the harder it's getting. I know too well
the few lines of code I just wrote to "make it work" aren't anywhere close to
production quality.

same concept as "small changes, make it good" but for design

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
the first one. Of course you should write tests. I'm not arguing you should have
tests or not, I'm arguing the effectiveness of tests suites. As for the second
point, it's true that integration tests are more likely to suit my definition of
"actual test" so I understand where people are coming from with this. My
definition of "actual test" though doesn't say anything at which level of
abstraction the tests should operate. I can write unit tests that fit perfectly
with my definition. The point is rather simple: test your behaviour, forget the
implementation.

From a practical standpoint, I do have a strong preference for integration
tests. It's obvious, I will give you that. After all, with the help of [Moore's
law](https://en.wikipedia.org/wiki/Moore's_law) integrations tests are so much
faster than 20 years ago the biggest pain points are not so painful any longer.
Nowadays, integration tests and statically typed production code make me feel
pretty comfortable.

### Balance your confidence

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart



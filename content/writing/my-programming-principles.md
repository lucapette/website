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
introduction there's a table of content in case you don't want to read all of
it.

 {{</ message >}}

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

It felt natural to group principles into categories: existing codebases,
greenfield projects, and general principles. I believe in approaching coding
sessions differently depending on the context. While this may sound a tad
obvious, I encountered enough examples of over-engineered or under-engineered
(why isn't that a thing? It's almost as common!) solutions in my career. More
often than not, these wrongly sized solutions were merely a reflection of
approaching an existing codebase like a greenfield project or, maybe worse, the
other way around.


## Table of content <!-- omit from toc -->
- [Table of content](#table-of-content)
- [How I approach an existing codebase](#how-i-approach-an-existing-codebase)
  - [Small changes, fast feedback loops](#small-changes-fast-feedback-loops)
  - [Test in production](#test-in-production)
  - [Be a gardner](#be-a-gardner)
  - [Read features end to end](#read-features-end-to-end)
  - [Facts > Assumptions](#facts--assumptions)
- [Starting from scratch](#starting-from-scratch)
  - [Make it work, make it good, make it fast](#make-it-work-make-it-good-make-it-fast)
  - [Throw it away first](#throw-it-away-first)
  - [Docs driven development](#docs-driven-development)
- [General principles](#general-principles)
  - [Prose not poetry](#prose-not-poetry)
  - [Name things what they do](#name-things-what-they-do)
  - [Write the code you'd like to use](#write-the-code-youd-like-to-use)
  - [Forget about easy to change, make code easy to delete](#forget-about-easy-to-change-make-code-easy-to-delete)
  - [Let the design emerge from the code](#let-the-design-emerge-from-the-code)
  - [Write actual tests](#write-actual-tests)
  - [Confidence is a struggle](#confidence-is-a-struggle)

## How I approach an existing codebase

When I think about the following principles, I think of codebases that are large
enough you can't possibly keep an accurate representation of all its parts in
your head. You may know some parts very well but there are enough unknowns you
can't make a non-trivial change without some planning. What's exactly large
enough is probably too personal so I won't even try to give an example. Most of
these principles are somewhat _even_ more relevant when the codebase is
completely new to you and you're trying to make sense of it and change it at the
same time.

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
it also to end result of good programming sessions: deployable artifacts.
There's [good evidence](https://www.goodreads.com/book/show/35747076-accelerate)
now that smaller changes lead to higher productivity. They're safer to deploy
and rollback if needed. They're also easier to review and more likely to move
fast to production.

We can derive at least two different conclusions from this principle:

- Fixing a few bugs one after the other, possibly in different part of the
  system, is a great way to approach a codebase that's new to you.
- Breaking down any change into smaller changes with their own feedback loop is
  a great way to approach changing a large codebase you're familiar with.

The reason why I'm not saying "just do TDD" is two-fold:

- TDD feels great as a workflow. It perfectly resembles what I call the "REPL
  experience". But I always had problems with the design constraints it brings
  to the table. I want the first draft of the code I'm writing to reflect the
  mental model of my understanding of the problem I'm trying to solve. TDD
  always forces me to shift my mental model a bit to make the code more testable
  _upfront_. There's nothing wrong with making code more testable. My problem is
  doing that upfront, when I'm discovering by writing code how well my mental
  model translates into actual code. The friction between what I want to write
  and what TDD needs me to write always has a negative effect on my ability to
  design a change effectively. In practise, I use TDD only to fix bugs.
- TDD is primarily a development technique. Its artifact is code. We ship
  features though, code is just a means to an end. So the code we write, even if
  perfectly tested, doesn't work unless it works in production. I want more than
  TDD from my "REPL experience". Hence the next principle.

### Test in production

I can hear [@mipsytipsy](https://twitter.com/mipsytipsy) in my head say "fuck
yeah!" every time I mention this principle. The way I like to put it is that you
want to expand the boundaries of your "REPL experience" far enough so to include
your production systems. Again, it's all about the speed of iteration. It's not
enough to have a tight feedback loop while writing the smallest possible change
if you don't ship it to production in _minutes_. You can put a lot of effort
writing "correct" code but unless it works in production, it doesn't work yet.

The urgency of shipping code to production helped me create an healthy
development culture in many teams. You want your "REPL experience" to include
production? You're going to need fast CI and CD pipelines. You're going to need
ways to observe the impact of changes on your production systems. Rollback has
to be cheap. You may want to use feature flags. All of it contributes to your
actual speed of iteration. We _all_ test in production when we ship code anyway
so you want to do it consciously to create a safer, faster environment for
feature development.

Testing in production and small changes are principles that feed on each other.
The faster your lead time, the more likely you'll ship smaller changes. The
smaller the change you ship, the safer your deployments. The safer your
deployments... it's a circle.

### Be a gardner

Code gardening is the act of changing code with the goal of improving it just a
little. You improve an error message, you use a more idiomatic way to write
those three lines, you align that test with your internal conventions. The
codebase is living organism: it's a garden and you take care of it every day.
You prevent bad habits from forming, you heal that spot that is a bit sick, you
give more water to the plants that are more exposed to the sun. My first
encounter with this analogy is a
[commit](https://github.com/rails/rails/commit/fb6b80562041e8d2378cad1b51f8c234fe76fd5e)
made by [@fxn](https://twitter.com/fxn). It stuck with me ever since.

I like the analogy a lot and the way I apply is connected to small changes. I
used to do gardening _while_ writing new features. I grew out of it because
smaller changes are always better. You could say that's a more important
principle. So now I have a small workflow that helps my gardening activities. I
take notes of the gardening I want to do while whenever I can. Sometimes, I
share it with my team (it really depends on the team and the context they're in)
to help other people form the habit of code gardening. I then use this todo list
as a personal reward system. I get done what I need to and then I treat myself
by improving that test that produces those annoying warnings. I have seen this
dynamic at play with other programmers as well once the habit forms for them.
The reward system is simple and effective, it allows people to make small
improvements _all the time_ and just feel good about it.

Being a gardner is easier if you're _already_ making very small changes _and_
you're _already_ testing in production. The joy of improving your codebase is
often limited by the cost of bringing these tiny little changes to production.
The higher the cost of a deployment the less likely you'll be a good gardener.

### Read features end to end

There's something I do _before_ making a change to a large code base that is new
to me. It's often called code safari but I'm no fan of the metaphor, I like to
call things what they do so the principles goes by "read features end to end".

Approaching a new codebase can get a little overwhelming. Sometimes you don't
even know where to start, which part of the system maps to which part of the
codebase. To help orient yourself, you can read a whole feature end to end.
Let's consider a large web application where an "innocent" click on a button
stars a series of processes which ultimately result in the customer getting an
some report via email. I'm sure you have seen a variation of this.

The idea of this principle is that you go on a hunt. You start searching for the
code of the button. It sounds trivial but you will discover things: how/if you
do i18n, what kind of frontend framework you're using. Most importantly, you
will notice things (oh we have our own css framework. I wonder why), you will
have questions (what are all the data attributes? I haven't seen any of that
when I was checking out the website from home). Open a notebook and write
everything down. Using your brain as storage is a waste: ["the shorter pencil is
longer than the longest
memory"](https://www.youtube.com/watch?v=vIW72VXMPHo&t=344s). Then when you know
what backend system reacts to the click, go down one layer and do the same
again. Repeat until you've gone all the way to the email service and came back
to your inbox.

Once you've done this, you'll have a mixed bag. That's where the hard work
starts. Reorganize your notes:

- Questions for yourself. A sort of todo list of things you know you want to dig
  in personally.
- Questions for those who know the codebase better than you and can get you up
  to speed faster.
- Idioms. Every codebase has its ways of doing things.

You now have a basic map you can use to choose a new path and repeat this
process to improve your understanding of the codebase you're tackling.

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

I have lived by this principle all my career as I have been lucky enough to run
into it very early. Over the years, this principle was one of the driving force
guiding me to my next programming language. Nowadays, I have a number of reasons
to enjoy working exclusively with statically typed languages, probably enough
reasons to write an article about it (interested? Please, let me know!). One
critical reason is this principle. The ergonomics of statically typed languages
allow me a much more relaxed approach to the make it work phase. I don't need to
care too much about how the code is organised or its naming. Modern IDEs are so
good at refactoring code that I can do a big chunk of the make it good part in
seconds. It's somewhat paradoxical: I often chose languages like Ruby because of
the supposed productivity gains only to realise I got stuck in early stages of a
new project because of the fear of needing too much time to refactor things.

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


### Docs driven development

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
thing with poetry though is that it's very good when it says a lot with just a
few words. Poetry can be really terse and that's my point. When in doubt, always
_prefer clarity to brevity_. Since itt's all the same for the machine as long as
it works, there's no point in making code terse or clever for the next
programmer.

### Name things what they do

Naming _is_ hard. Having said that, I think we often make it harder than it
should be for us because we treat writing code as poetry. So we're looking for
elegant, apt, short names for everything. I know the struggle because I've been
there a lot early in my career. I grew out of it because I realized a few
things:

- It's unlikely I'll get naming right the first time I sit down to write the
  code for a non trivial problem. Naming is the most significant "short-term"
  design tool at a programmer's disposal. Sometimes I don't understand that on a
  conscious level while writing. Calling something, say, `JobScheduler` will
  constraint the design of whatever interact with it to treat it like a job
  scheduler right? Right, it sounds obvious. But that's kind of the
  point/problem. Way too often I want to get the naming right too early. The way
  I look at it is this: the delta between the mental model I created for the
  solution I have in mind and the actual code I will produce is going to
  decrease as I write it. In a way, I'm bound to get some names wrong the first
  time around. But it's effect of the problem. The cause is that I don't
  understand the problem yet.

- if you get stuck, most descriptive even if long is fine
- if you’re really stuck. Go for a walk, there’s a chance the problem is not
  naming but design

### Write the code you'd like to use

I can't find the original reference but I'm relatively sure this principle too
was inspired by Kent Beck. This works well when you get stuck because you have
no code to start from. So you have all kind of questions about how the API of
this new module should look like, how should the data structures look like, and
so on. It can get so overwhelming you just get stuck. That's where you take a
deep breath and ask yourself: so how do I want to use this code? Move away from
the empty file back to to the caller site. Write the code like the problem
you're solving is already solved the best way possible. No constraints, jot down
a few lines of the code you want to use. It makes wonders.

### Forget about easy to change, make code easy to delete

### Let the design emerge from the code

This is mostly an exercise in patience. The more experienced you are the worse
you have it because you know too well the few lines of code you just wrote to
"make it work" aren't anywhere close to production quality. But that's the point
of guidelines, they guide _against_ your own primitive instincts.

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



### Confidence is a struggle

- it’s good because it gives you the ability to break down any problem. The more experience you have, the less you fear a problem
- it’s bad because it feeds on ego. And ego makes you do dumb things. You want to stay humble so you can be smart



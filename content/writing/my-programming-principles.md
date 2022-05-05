---
title: "My Programming Principles"
description: "What guides me after 20 years of writing code for a living"
date: 2022-04-17T14:12:13+02:00
draft: true
keywords:
  - programming
  - principles
---

{{< message class="is-info">}} This is a _long_ read. You will find a table of
 contents right after the introduction. {{</ message >}}

One day, during a post-game interview, elite chess Grandmaster Alexander
Grischuk was asked the routine question "how long have you been preparing for
this game?". With his famous wit, he answered:

> I've been preparing my whole life

A beautiful answer. It applies to _any_ job: _all_ your past experiences
contribute to some extend to the decisions you make today. So I've been thinking
about how this idea applies to me. I was immediately presented with an
intriguing writing challenge. While intuitively, I knew I follow some principles
I collected over the years when I'm writing code, I couldn't immediately
structure these principles. They felt very connected to each other.

The article you're reading is a (very long) answer to the question: how have I
been preparing for the code I'm about to write? My desire to learn more about
these principles (and myself) was too strong, the challenged too interesting. I
couldn't pass on it.

I grouped the principles into categories: existing codebases, greenfield
projects, and general ideas. While it sounds a tad obvious you want to approach
new projects and existing codebases differently, I encountered enough
over-engineered or under-engineered (isn't that a thing? It's almost as common)
solutions in my career. More often than not, approaching an existing codebase
like a greenfield project or, worse, the other way around was the number one
reasons these solution were incorrectly sized for the problem they were trying
to solve.

Before I move on to the principles themselves, I want to mention I find the
title of the article a little pretentious. I went with it anyway because it fits
the topics covered better than anything else I could think of.


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

The codebases I have in mind are large enough you can't keep an accurate
representation of all its parts in your head. You may know some parts very well
but there are enough unknowns you can't make a significant change without
careful planning.

I don't think I can provide a good example of what's a "large enough" codebase
because that's probably too personal. Let's go with "large enough for me, the
reader".

### Small changes, fast feedback loops

In short: change a few lines _or less_ before seeking feedback. I mean
_literally_ the smallest possible change you can think of. The size of the
change (as small as possible) and the speed of iteration (how fast feedback
about the change reaches you) are equally relevant. The workflow looks like
this:

- I change very little. A few lines _or less_.
- I seek feedback.
- If it works _so far_, I repeat.

The key is _so far_. I have mental checkpoints marking the progress. "I changed
this line, stuff still works". Yes, it's that trivial. As soon as my feedback
loop shows me something unexpected, I go back one checkpoint because "it worked
before I changed that". I can be radical with my reaction to unexpected
feedback: I often just delete the last change and start over.

Here's how I implement this workflow: I set up a custom, project-specific REPL
_before_ I change any code. The tooling isn't relevant to the principle. It can
be TDD, it can be print statements (yep, I just said that), whatever. The point
is that it has to feel exactly like read-eval-print loops. They're really fast.
Once that experience is in place, I can start changing a few lines _or less_ per
time and apply the checkpoints process I just described.

Focused, short, productive programming sessions are the typical scenario for
this principle but you can (and with can I mean should) apply it also to end
result of good programming sessions: deployable artifacts. There's
[evidence](https://www.goodreads.com/book/show/35747076-accelerate) smaller
changes lead to higher productivity. They're safer to deploy and rollback if
needed. They're also easier to review and more faster through the production
pipeline.

I know this principle has a striking resemblance to TDD because it's the most
frequent feedback I get when I talk about it with other programmers. Why am I'm
not saying "I do TDD all the time"? Well the answer is that I almost never
practice TDD. Let me explain:

- TDD feels great as a workflow. It does resembles the "REPL experience" I so
  fond of. TDD quietly brings a design constraint to the table. When I'm writing
  code, I'm mostly just typing out the solution I modelled in my mind. TDD
  always forces me to shift my mental model a bit so that the code is testable
  _upfront_. While there's nothing wrong with making code more testable, my
  problem is doing it while I'm discovering how well what I modelled in my head
  translates into actual code. The friction between what I want to write and
  what TDD wants me to write always had a negative effect on my ability to
  program effectively. In practice, I only practice TDD for small bug fixes.
- TDD is primarily a development technique. Its artifact is unshipped code.
  Unfortunately we can't say the code we write works, even if it's perfectly
  tested unless it works in production. That's why I want more than TDD from my
  "REPL experience", I want to [test in production](#test-in-production).

### Test in production

I can hear [@mipsytipsy](https://twitter.com/mipsytipsy) in my head say "fuck
yeah!" every time I mention this principle.

Here's what it means to me: I want to expand the boundaries of my "REPL
experience" enough to include my production systems in the loop. I'm not
satisfied with a tight feedback loop while writing the smallest possible change.
I want to ship it to production in _minutes_ and find out if it works there.
What's the point of putting a lot of effort in writing "correct" code if we
don't know if it works in production?

The urgency of shipping code to production has a positive effect on the culture
of a product development team. We want your "REPL experience" to include
production? We're going to need fast CI and CD pipelines. We're going to need
ways to observe the impact of changes on your production systems. Rollback has
to be cheap. We may want to use feature flags so the deployments are safer. We
may _not_ need a staging environment so the pipeline to production is shorter.
All of it contributes to the speed of iteration (from "Boyd's Law of
Iteration").

The cultural pushback I get when I say "test in production" is fascinating
because we _all_ test in production when we ship the code _anyway_. It makes
sense to do it consciously. It creates a safer, more efficient environment for
feature development.

Testing in production and small changes feed on each other. The faster your lead
time, the more likely you'll ship smaller changes. The smaller the change you
ship, the safer your deployments. The safer your deployments... you get it, it's
a circle!

### Be a gardner

Code gardening is the act of changing code to improve it just a little. You make
an error message more understandable, you use a more idiomatic way to write
those three lines, you align that test with your internal conventions. The
codebase is living organism: it's a garden and you take care of it every day.
You prevent bad habits from forming, you heal that spot that is a bit sick, you
don't forget to water the plants. My first encounter with this analogy is a
[commit](https://github.com/rails/rails/commit/fb6b80562041e8d2378cad1b51f8c234fe76fd5e)
made by [@fxn](https://twitter.com/fxn). It stuck with me ever since.

The way I do gardening evolved over time. I used to add some gardening to the
existing tasks I had. I grew out of it because it polluted code reviews with
unrelated things and increases the size of a change. I value small changes more
than gardening. Nowadays I have a small workflow to structure my gardening
activities. I take notes of the little things I want to do while doing other
tasks. Sometimes, I share these notes with my teams (it really depends on the
team and the context they're in) to help other people form the habit of code
gardening. I then use this todo list as a personal reward system. I get done
what I need to and then I treat myself by improving that test that produces
those annoying warnings. I have seen it at play with other programmers as well
once they form the habit. The reward system is simple and effective: it allows
people to make small improvements _all the time_. Gardening shows care for the
codebase, caring makes working on the same codebase more sustainable over time.

Being a gardner is easier if you're already making _small changes_ and you're
already _testing in production_. The ability to improve a codebase is often
limited by the cost of deploying these tiny changes to production. The higher
the cost of a deployment the less likely one can be a good gardener. It's all
connected.

### Read features end to end

When I'm new to a large codebase, I read features end to end _before_ making any
change. People often call it code safari but I'm not a fan of the metaphor, I
like to call things what they do so the principle goes by "read features end to
end". Yeah, I get it. It's boring. I like it that way.

Approaching a new codebase can get a little overwhelming. Sometimes I don't know
where to start, which system maps to which part of the codebase. To orient
myself, I read a whole feature end to end.

For the sake of discussion, let's consider a web application composed of
multiple services. One page in the app has an "innocent" button that stars a
sequence of processes which ultimately result in sending some PDF report via
email. You might have seen some variation of this.

The idea is that I go on a hunt. I start searching for the code that renders the
button. It's trivial but I know I will discover things: how/if we do i18n, what
kind of frontend framework we're using. Most importantly, I ask myself questions
like:

- oh we have our own css framework? Why?
- what's up with all these data attributes? I haven't seen any of that when I
was checking out the website from home. We stripping them down?
- why are we stuck on such an old version of X?

Before I forget them, I write everything down. Using my brain as storage is a
waste: ["the shorter pencil is longer than the longest
memory"](https://www.youtube.com/watch?v=vIW72VXMPHo&t=344s). When I know which
service handles to the click on the report button, I go down one layer and keep
reading. I repeat the process until I've gone all the way to the email service
that sent out a test email right into my inbox.

Once I'm done, I have many questions and that's when the hard work starts. I
reorganize them:

- Questions for myself. A sort of todo list of things I know I want to dig in
  personally.
- Questions for those who know the codebase better than I do and can get me up
  to speed faster.
- Idioms. Every codebase has its ways of doing things.

I now have a basic map I can use to look around the codebase. I often repeat the
process a few times before I start making any change.

What I like the most about this principle is how trivial it sounds when I
describe it and how helpful it is when I use it.

### Facts > Assumptions

I often say:

> Common sense is not so common

It is common sense facts are much more relevant that assumptions, isn't it?
Well, I've been bitten by the wrong assumption (sometimes, I probably deserved
it, more than one) many many times. For example, most of the bugs I fixed were
an effect of miscommunication which is nothing more than two parties assuming
what something means without fact-checking they agree. Fixing a bug is aligning
the code with the facts.

It's not a coincidence I'm mentioning bugs right now. This principle guides my
debugging sessions. It goes like this: I read the code, I know there's a bug
there. If I can't find the bug by just looking at the code, this principle comes
to rescue: somewhere in the list of facts I know about this code there's at
least one thing I'm considering fact but it's actually an assumption. Then I
move one abstraction layer away from the code. I start debugging my facts:

- I find the "fake fact". Often I immediately find the bug too. Especially when
  I was the author of the code I'm reading. It's kind of obvious in a way. The
  bug was there precisely because I thought the code I wrote did X while in
  reality it did Y.
- All the facts are true. It means I have to go down one layer again, expand the
  search, collect more facts, start debugging the facts again.

I apply this principle to performance improvement as well. Early in my career, I
had the tendency to guess what made, say, an endpoint slow. Sometimes I even
changed some code without measuring anything. When I think about it now, it
makes me smile "oh how young I was back then!". The guessing game was
stimulating but I had to abandon it because the facts (ah!) were showing me it
was a waste of time. I was wrong pretty often and, not only that, I also often
found the results of my benchmarks so counter-intuitive I could only conclude
that the best approach is to measure everything. Now I don't even ask myself
what makes some code slow. I measure everything, I want the facts.

## Starting from scratch

Since it's less common, I feel privileged (or unlucky. It depends on the day
honestly) I have been in charge of brand new, multi-year, projects many times in
the past 15 years. These projects were all different, they involved each time a
different team and a different company. But the fact I had to start from scratch
in all of them has given me the opportunity to try things out and see what
worked and what didn't. The following principles are my favourite parts of what
worked.

### Docs driven development

More than a decade ago, I run into [readme driven
development](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html).
I was happy I could give a name to something I had been already doing for a few
years. Since then, I tweaked the name of the principle to a more generic "docs
driven development". It's a little more general and flexible. The idea is to
write documentation for a system as the system was already done. This principle
applies the ideas of [the code you'd like to
use](#write-the-code-youd-like-to-use) to system design.

Documenting a system before building it is a design technique. The beauty, and
the irony, of this approach is that I write the docs first so I can put some
constraints in place and reflect on their consequences with a low effort.
Consciously chosen constraints are the foundation of good design.

What kind of document, how long or how detailed it should be is too
project-dependent so it's hard to provide concrete examples. There are some
things though I have in any document:

- __Scope__
  : I write down what the goals of the project are. When it's done,
  what will this system do? Even more interesting, what won't it do? Answering
  these questions gives me some boundaries so I can stay on track and focus on
  the essential.
- __Organization__
  : A few diagrams go a long way. I sketch a few to get a sense
  of how to split responsibilities between among the different parts. What the
  highest level building block of the system should be.
- __Prioritization__
  : I write down the order in which it makes most sense to
  build things. Depending on how the system is organized, I write down what can
  be parallelised.

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

If the problem is really hard, I may repeat this process a few times. If the
problem is trivial, I may get away without ever throwing code away. The point is
I'm ready to do so when I'm unhappy with my understanding of the problem. Please
note: I didn't say anything about what I think of the code. My harsh truth is
that the best case scenario is that I don't hate it today and I may still not
totally hate it in a few months.

The key of this principle is this: I write a first draft knowing I will probably
throw it away. With that in mind, I experiment, I digress a little, then I go
back a little and try again. I write code for the machine and words for humans
to improve my understanding of the problem.

My favourite programming sessions always happen after I've thrown away at least
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
  : I go through the notes and comments I left for myself. One
  by one I tick things off. Every two or three items I get off the list, I step
  back a little. I evaluate the design. Now I am making it good: I want to get
  down to the little details, I need to be happy with the design. The way the
  code is organized has to make sense. I care just about everything. I only
  leave out speed. I am not trying to write slow code by design (that wouldn't
  help and is probably harder to do than it sounds?) but I am also not trying to
  squeeze every millisecond off every function.
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
Sentences are a few instructions, paragraphs are little algorithms, chapters are
modules, a book is a system. The parallel doesn't need to be exact, the point is
that I don't want to jump from shifting bits to call an external service in two
consecutive lines. I want _homogeneous abstraction within a layer and
heterogeneous abstraction between layers_.

The one notable exception to writing prose is performance. Often making code
faster also makes it messier and uglier because you'll see that the abstraction
isn't homogeneous any longer. This is one situation in which I like writing
comments explaining _why_ the code looks the way it looks.

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
be because there's a certain social pressure to treat code like poetry. The code
has to be pretty so we're looking for elegant, apt, and short names for
everything. I struggled early in my career with this but I grew out of it when I
realized a few things:

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
of not liking this naming 🙃). I know I'm bound to get some names wrong the
first time around but that's an effect of the problem. The cause is that I don't
understand the problem well enough yet and the suboptimal naming reflects that.
"Relaxed naming" comes to rescue me and it looks like this:

- Call things what they do _right now_. The more descriptive the name, the
  better. Bonus points if it feels a little boring.

Yep, there's no step two.

It's a little cheeky I'll give you that. But all things considered, after a
little less than 20 years I'm finally happy with my naming process. I look at it
the same way I look at jotting down a first version of a new system. Let me
spell out the obvious: the thing I'm trying to name is _new_. I am naming it now
because it's not there yet. So the principle [make it work, make it good, make
it fast](#make-it-work-make-it-good-make-it-fast) works pretty well here except
there's no step three.

Name things what they do also means I prefer dull, descriptive, and long names
to short and clever ones. I also despise acronyms. There are exceptions (of
course HTTP is a fine acronym for example) but in general the less acronyms the
better. Extend this principle to the naming of all things in your systems,
including your services, your machines. Prefer boring name schemes to fancy or
funny ones. One trivial example:

- DO: kafka1, kafka2, kafka3
- DON'T: orion, antares, pluto

I don't have to ask what kafka1 is because I called it what it does.

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

The third step is hard because I have to lie to myself, I do know the code isn't
there. It only works for me if I jot down a solution without trying to picture
in my head all the code I actually need to write to make it all work. Sometimes,
I go as far as using pen, paper, and pseudo-code something as a first step so I
can let go of weight of all the sub-tasks each line I'm "faking" is generating.

### Let the design emerge from the code

Good design is an exercise patience.

It's hard to do when I have a clear idea of the different parts of the system.
It's half the excitement of working on a new problem and half the impatience of
not being done with it. I want to do it all fast and perfect and, of course, I
learned that _never_ works well in practice. You can't rush good design. To make
things harder, systems are living organisms, their only constant is change. They
change because of business needs, scaling needs, or whatever. The list is too
long. The point is they change all the time. That's the part of our craft I've
seen most people struggle with. To be fair, evolving systems is hard. Over the
years, I collected a few one-sentence ideas that guide me through designing a
system.

The first one I got it from [Rob
Pike](https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html)

> Less is more

I often think about code as bi-directional form of communication. I'm trying to
tell the machine what to do and the code talks back to me. The way the code
looks, the way some parts need to change every time some other parts need to
change, the naming being a little off, so many little details that are trying to
tell me something. If I'm always talking though, I won't be able to listen to
what the code is trying to tell me. If I want it to be a dialogue then, I need
to slow down and pay attention to what the code is telling me.

> Simple ain't easy

Good design looks obvious. It's so simple it has just about enough to solve the
problem. But simple doesn't mean easy. Reaching a simple design takes the time
it takes. This is a good reminder.

> Prefer duplication over the wrong abstraction

I got this from [Sandi
Metz](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction). She does a
wonderful job of explaining it so I won't repeat what she meant here. What I can
add on top of it s that it's been very beneficial for me to expose my teams to
this idea over the past years. It's, again, an exercise in patience. I
trivialise it a little for the sake of making a point and often say: if you
duplicate once, leave it be. If you have the same piece of code in three places
at least then you've got a good chance for the correct abstraction.

These three "code proverbs" (I should collect these right?) all contribute to
the idea that waiting for the design to emerge from the code is better than
trying to rush into the code our current idea of good design. The point is
really that waiting helps. Since the system changes all the time, waiting often
results in a better understanding of the problem. The code itself will show what
parts of its current design are problematic. You have to be there to listen to
it.

### Write actual tests

Before I explain what I mean with "actual" in this context, I have to make a
little premise. When I say "tests", I mean the automated verification process
that your systems (kind of) work. I do not mean TDD. I have nothing against TDD
but it's a development methodology. My opinion about programming methodologies
is that none of them always work and that most  are great in specific contexts.
Back to tests as a verification process.

Let's clarify what I mean with "actual test":

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

If I had to name the number one difference between being a young developer and
now, I would go for confidence. The more experienced I felt, the more convinced
I was I could solve any problem.

It is somewhat obvious that experience makes you confident. You gain experience
through trail and error. Rinse and repeat. You learn what works and what not by
trying things out, often the hard way. The more time you spend doing something,
the more you know about what works and what not. So yes experience makes you
confident. Sounds good no? Yes and no. You need a balance.

Confidence is good because it can make you fearless, you know you're going to
solve anything that comes your way. Let me tell you a story that illustrates
what I mean from my first job.

In the summer of 2006, I joined a company in Roma that processed hundreds of
million of events per day for poker machines. The legislation in Italy states
these machines had to return 75% of money bet so our system collected _any_
event these machines produced. The company run monitoring and fraud detection
algorithms. The whole system was a large collection of stored procedures (don't
judge me for this architecture OK? I didn't design it) on the top of a giant
Microsoft SQL Server installation. These stored procedure parsed the events,
stored the processed data in various tables, and run a bunch of algorithms to
figure out if the machines were playing a fair game. It was complicated for a
first job.

My first big task was to make use of a reserved part of the input string (events
where large blobs of text. I think they came in vai upd? I'm not sure). In
practice this meant I had to figure out the impact of this change over the whole
system, aka 200 plus stored procedures. I knew I couldn't do this alone. In
fact, it took almost a week of despair for me to find the courage and tell my
boss I had no idea what to do. When I talked to Mario (it was in... Italy so
¯\_(ツ)_/¯), his answer filled me with anger first. He said something like
"let's do this together, it'll take two hours". Two hours?!? I lost almost a
week trying to make sense of this, how were we going solve this in two hours?
Well, he showed me and it was one of the most profound learning experience of my
career so far.

We got into a meeting room and Mario did the most obvious thing: he broke down
the problem in small steps, one stored procedure after the other. We put the
code on a screen and took notes at every step. Fast-forward two hours and we had
a whiteboard full of sticky notes marking every single stored procedure we
needed to change and the reason why we had to change it. I was amazed.

Mario had the confidence to solve the problem. He knew he could do it so there
was no fear to cloud his thinking. He sat down with me and executed a simple
(simple ain't easy eh?) process laying down the foundation of four weeks of
work. With his help, this was the first successful large deployment of my
career.

I imagine at this point you may be asking: if confidence has such a amazing
effect on your ability to solve difficult problems, what would you want to
balance it with? Well the answer is confidence feeds on ego. That's a problem.

While confidence makes me stronger, ego makes me dumb. At times, I had troubles
telling them apart, after all they're closely correlated. The big difference is
the perspective from which you look at things. A confident person knows what
they can do, they value help, they give credit where it's due. An egoist thinks
only they can do it, they don't seek for help, they take credit from other
people. This distinction makes a big difference in how I approach a problem.
When I'm confident, I take my time to break a problem down in small parts, I
research the subject, I ask for help, I plan accurately. When I'm high on ego I
look at a problem from a pedestal, I underestimate it. I plan badly because "hey
I'm great! of course I will solve this on the go, planning is for losers".

That's why I seek ways to balance my confidence. You need enough confidence to
fear no problem but not too much to get high on Ego. The way I do this is by
putting to practice the idea "we're all junior at most things". So I learn new
things. Just in the past few years, I tried drawing, rubik cubes, Chess, you
name it. When it comes to programming, I try to solve problems in areas I have
no idea about. I try some problem that seem really scary. Like writing a
programming language. While I only toyed with programming languages so far, I
wrote enough code to find out that yes it's indeed hard but it's also "just" a
program like. The journey is humbling and the reward is two-fold: a balanced
confidence and expanded knowledge.

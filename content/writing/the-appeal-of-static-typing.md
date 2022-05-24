---
title: "The Appeal of Static Typing"
description: "or how I learned to stop worrying and love types."
date: 2022-05-18T08:34:12+02:00
draft: true
keywords:
  - programming
  - static typing
  - dynamic typing
---

Before I dive into the topic, let me make some premises:

- The point of the article isn't to say statically typed languages are better
  than dynamically typed ones. That means nothing.
- I don't want to convince you that you should drop your favourite language and
  go for the ones I'll discuss in the article.
- I have problems expressing my opinion when I consider something "too obvious".
  I think that statically typed languages lead to a better programming
  experience. And I do think it's obvious. So I have hard times articulating the
  why. So here we are, I never say no to a writing challenge.

The premises should clarify I mean everything that follows as _my_ personal
opinion. I'm sharing my experience hoping it may be useful to other people.

The way I got to my informed opinion is the most interesting aspect of the
conversation. I will expand on this as I conclude the article.

## How I got here

I started my programming journey in the early 2000s, I was a computer science
student at the University of Naples, Italy. We did C and I liked it. It was
difficult but satisfying.

After I graduated, I did Java for a better part of the first decade of my
professional career. The language and its ecosystem felt powerful but
unproductive.

In 2009, I started looking for something different. That's how I found Ruby and
Ruby on Rails.  As an exercise, I tried to build a prototype version of a
product my team was working on for a week or so. I got to feature parity in less
than day. My programming world was shattered in minutes.

The euphoria kept me going for a few years: meta-programming was cool, RSpec was
cool, Sequel was cool. The community was fantastic. But while I loved the
language and its ecosystem, I was always uneasy putting Ruby applications to
production. I often trivialised the experience saying "No matter ho many tests
we add, we still ship typos to production".

Around 2013, I started looking into around for something more solid, possibly
faster. I found Go. The language wasn't as pretty but I loved how robust
programs felt as soon as they compiled. Even though it was early days for the Go
community, the tooling around the language was already advanced compared Ruby
(with the obvious exception of dependency management).

I then went back and forth between Ruby and Go for a few years. I also shared a
longish article about [my experience with Go]({{< ref
"/writing/my-experience-with-go" >}} "my experience with go") in 2017.

Around that time, I started getting much deeper into Apache Kafka. I had already
wrote Kafka applications in C, Ruby, JavaScript, and Go so I knew that to
exploit its potential, I had to write for the JVM. The official libraries
offered so much more and Kafka Streams, a wonderful streaming library I was very
keen on using, was only available for JVM.

So I reluctantly went back to Java. I told myself "well it's going to be
unproductive but I won't have to write my own Kafka Streams library". I was _so
wrong_ about this it's kind of funny to write it down now.

The developer experience had improved a few orders of magnitude since 2008 so,
before I knew it, I did only Java and TypeScript for a few years.

This journey is a circle: Java was my first professional programming language,
and it's now my last. While the circle interesting in itself, the languages I
went through are not what fascinates me about the journey. Here's what stands
out to me:

- Every time I changed languages, the reasons had nothing to do with language
  features. Also, I was definitely not framing my choice as dynamically typed
  versus statically typed.
- Despite I never cared much for language features, nowadays I actively try to
  avoid working with dynamically typed languages. After 2 decades of
  programming, I finally developed some opinions about programming languages 🙃

With this circular journey in mind, let's dive into what makes me say "yep, I'm
not going to do dynamically typed languages any more".

## Focus on "actual" tests

I follow a programming principle I call [write "actual" tests]({{< ref
"/writing/my-programming-principles#write-actual-tests" >}} "write actual
tests"). Quoting myself from the article:

> A test must change only if the behaviour it verifies changed.
>
> No other reason is good enough for a test to change. Which, in turn, means
> “actual tests” only test behaviour. I’ve seen tests testing language features,
> or missing language features (looking at you dynamically typed languages), or
> framework features.

The last part of the quote is what's relevant to this conversation.

Testing dynamically typed code was bittersweet. On one side, I loved how
expressive Ruby was. On the other hand, testing Ruby code made me a little
paranoid. I could pass _anything_ at runtime to every function so I felt a
constant cognitive overload. A good chunk of the testing made me feel silly. I
couldn't help but think I was covering the gap for missing language features. I
mean static types of course.

I have a preference for integration testing as they're more likely to fit my
"actual" test definition. I have always found that challenging with Ruby. I did
not really understand how challenging compared to other languages until I found
a simple and effective way to ["actually" test CLI applications in Go]({{< ref
"/writing/writing-integration-tests-for-a-go-cli-application" >}} "test CLI
applications in Go").

The experience with Go first and then especially with Java has been very
different from Ruby. "Actual" tests were easy to write and relatively fast. For
example, I can "actually" test a Kafka Streams application with
[Testcontainers](https://www.testcontainers.org/). Add one dependency here, two
lines of code there, and bam I've got a running Kafka instance for each of my
tests.

Statically compiled languages allow me to focus much more on "actual" tests. I
enjoy that a lot. I like to think that the nature of these languages is the
reason why their communities produce testing tools more apt to my needs. Hard to
say but I _do not_ care enough for the pedant causation versus correlation
conversation.

It's a combination of things really:

- the statically checked types cover a lot of ground I would have to test
  otherwise.
- Integrations tests are easier to write because of the available tooling
- The statically typed languages I worked with are so much faster than the
  dynamically typed ones that it's relevant even in the context of testing. I
  love faster feedback loops.

## The editing experience

To me, the appeal of static typing is much more evident when it comes to
editing. Here's what I use right now:

- IntelliJ for Java and Kotlin
- VS code for TypeScript and Go

Both IntelliJ and VS code are _spectacular_ tools. They excel at every aspect of
every day programming. Be that editing, renaming, refactoring, debugging,
testing. Anything really.

They're both really fast. I ask the question "how on earth is IntelliJ so fast
at doing X?" weekly. Before going back to Java in 2017, I  had used IDEs in the
early 2000s and they were horrendously slow back then.

I think the nature of the language does contribute to a better experience. For
example, the Go community has produced a lot of static analysis tools and VS
code uses [many of
them](https://github.com/golang/vscode-go/blob/master/docs/tools.md).

It's a bit of a chicken-and-egg situation: Statically typed languages have
better tools because they are easier to write compared to dynamically typed
languages. The tools are easier to write because statically compiled languages
have better statical analysis tool.

The flagship example of how much better the editing experience can be is
autocompletion. I remember spending countless hours getting Ruby autocompletion
working. It was never good enough. The experience with Java or Kotlin is
flawless. But maybe the best example of the difference is JavaScript vs
TypeScript. The experience with TypeScript is so much better that autocompletion
would be enough of a good reason to adopt TypeScript.

IntelliJ also guided me to more idiomatic Java or Kotlin code. The IDE provides
warning and suggests improvements based on best practice. It sounds like a
little thing but it has a significant impact on the way you can learn a new
language. Let me provide an example:

```java
val favouriteThings = listOf("Raindrops on roses", "Whiskers on kittens")
// IntelliJ will show a warning on map
println(favouriteThings.map { it.lowercase() }.joinToString(" & "))
//and suggest this:
println(favouriteThings.joinToString(" & ") { it.lowercase() })
```

It's a small thing but, if you didn't know about it, it just taught you a new
API. The key is that this happens countless times a day, especially if you're
new to the language. So even learning a statically typed language has been
easier because of the editing experience.

## Shipping code to production

I wish I could attribute the quote I'm about to share but I really don't
remember where I read:

> Sometimes we forget the most important aspect of writing a program: it has to
> work.

It's beautiful in its simplicity. And how do I know if my code works? Well, of
course I have to ship it to production.

Shipping code is where my experience with dynamically typed languages and
statically typed has been the most different. The tl;dr is: dynamically typed
languages make me doubtful about what I'm shipping and statically typed
languages make me confident.

There's a few reasons that contribute to this so let's dive into them.

### Types are docs

This comes up a lot and, I can't stress this enough, it's true. Types definitely
act as documentation because how descriptive they are. Let me spell out the
obvious: they're together with the rest of code. Too obvious right? Well that's
the point. The co-location makes types the best possible kind of documentation:

- they're never out of date
- they're as close as possible to production code

They also help answer questions that come out all the time:

- what does this function return?
- where do we use this class?
- Can I pass a string to this third-party library method?

What does this have to do with shipping code to production you may ask? Two
things:

- I have to keep less things in my head. Types store a lot of metadata
  information for me _in the code_. If I'm not sure about something, I can just
  read the type. This obviously applies to libraries. Yes, I know it's obvious.
  But I mention it anyway because the different between, say, integrating a Java
  library and a Ruby one is staggering.
- I don't have to care about typos or other silly mistakes like that. I can't
  pass a double to a method expecting a string. Again, all too obvious. I wish I
  could say I never shipped typos to production with Ruby.

### The reading experience

Reading code is a core part of the practice of programming. Shipping code to
production involved a lot of reading. Because of how helpful "types are docs" is
in practice, I find the cognitive load of reading production code written in a
statically typed language significantly smaller than the cognitive load of its
dynamic counterpart.

The thing is that reading code shows up everywhere in the process of shipping
and maintaining production code:

- I need to figure out where this annoying bug is? I need to read code
- I need to update the way I use this library? I need to read code
- I need to help someone ship their code? I need to read code

Every time I need to read statically typed code, I need to put a a little less
effort than doing so with a dynamically typed language. It adds up pretty fast.
That energy surplus makes me more confident shipping statically typed code to
production.

### When it compiles, it probably works

This also comes up a lot and I remember being very sceptical hearing this when I
was working only with dynamically typed languages. I was... wrong. Because this
too is true. The probably is important because it's not a 100% hit of course.
But the point stands and plays into my conversation well: it's a matter of
confidence.

This becomes really evident when I'am trying to rush a bug fix to production. No
matter how careful I am, this situation just happens from time to time. The last
thing I want is second guessing the fix I'am about to ship. It's all about the
cognitive load after all and the compiler telling me my code "works" factors in
as well. Again, it makes me more confident.

## Experience is everything

While I tried my best to explain some basic benefits of statically typed
languages, I'm somewhat sure this won't convince a single soul to switch. It was
a non-goal for me but it's still interesting to go over why I think my
experience won't convince anyone despite it definitely convinced me.

It's funny to say so but it's precisely because it's experience. Back when
worked only with dynamic languages, I came across all I shared in this article.
Many times. But none of it was convincing because while the experience between
these two worlds is very different, what makes up for the vast different isn't
one big thing. It's more a lot of small every day things. There's also something
else: it's hard to look for alternatives when you're happy with what you're
doing. And there's definitely nothing wrong with being happy with dynamically
typed languages. In fact, they're pretty awesome!

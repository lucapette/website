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
- I have problems expressing my opinion when I feel something is "too obvious".
  In this case: statically typed languages lead to a better programming
  experience. Because it's obvious to me, I have hard times articulating the
  why. So here we are, I never say no to a writing challenge.

The premises should clarify I mean everything that follows as _my_ personal
opinion. I hope that sharing the journey will be useful to other people.

The way I got to my informed opinion is the most interesting aspect of the
conversation. I will expand on this as I conclude the article.

## How I got here

I started my programming journey in the early 2000s, I was a computer science
student at the University of Naples, Italy. We did mostly C and I liked it. It
was difficult but satisfying.

After I graduated, I did mostly Java for a better part of the first decade of my
professional career. I went through two jobs where I wrote lots of Java. The
language and its ecosystem felt powerful but unproductive.

In 2009, I started looking for something different. That's how I found Ruby and
Ruby on Rails. My world view was shattered in minutes. As an exercise, I tried
to build a prototype version of a product my team was working on for a week or
so. I got to feature parity in less than day. It was unbelievable.

The euphoria kept me going for a few years: meta-programming was cool, RSpec was
cool, Sequel was cool. The community was fantastic.

While I loved the language and its ecosystem, I was always uneasy putting Ruby
applications to production which I often trivialised the experience saying "No
matter ho many tests we had, we still shipped a typo to production".

Around 2013, I started looking into Go. The language wasn't as pretty as Ruby
but I loved how robust programs felt as soon as they compiled. Even though it
was early days for the Go community, the tooling around the language was already
advanced compared Ruby (with the obvious exception of dependency management).

I then went back and forth between Ruby and Go for a few years. I also shared a
longish article about [my experience with Go]({{< ref
"/writing/my-experience-with-go" >}} "my experience with go") in 2017.

Around that time, I started getting much deeper into Apache Kafka. I had already
wrote Kafka applications in C, Ruby, JavaScript, and Go so I knew that to
exploit its potential, I had to accept going back to the JVM. The official
libraries offered so much better and Kafka Streams, a wonderful streaming
library I was very keen on using, was only available for JVM.

So I went back to Java. I told myself "well it's going to be unproductive but I
won't have to write my own Kafka Streams library". I was _so wrong_ about this
it's kind of funny to write it down now.

The developer experience had improved a few orders of magnitude since 2008 so,
before I knew it, did only Java and TypeScript for a few years.

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
expressive Ruby was. Especially since RSpec 3.0 came to be, the API was was
fantastic. On the other hand, testing Ruby or JavaScript code made me a little
paranoid. You could pass _anything_ at runtime to every function so I felt a
constant cognitive overload. A good chunk of the testing made me feel silly
because I couldn't help but think I was covering the gap for missing language
features. I mean static types of course.

I have a preference for integration testing (they're more likely to fit my
"actual" test definition) and that was challenging with Ruby. I did not really
understand how challenging compared to other languages until I found a simple
and effective way to ["actually" test CLI applications in Go]({{< ref
"/writing/writing-integration-tests-for-a-go-cli-application" >}} "test CLI
applications in Go").

The experience with Go first and then especially with Java has been very
different from Ruby. High-level "actual" tests were easy to write and relatively
fast. Because, again, these communities also developed around their languages
strengths. Which meant, for example, I could "actually" test a Kafka Streams
application with [Testcontainers](https://www.testcontainers.org/). Add one
dependency here, two lines of code there, and bam you've got a running Kafka
instance for each of your tests.

Statically compiled languages allowed me to focus much more on "actual" tests. I
enjoy that a lot. I like to think that the nature of these languages is the
reason why their communities produce testing tools more apt to my needs. Hard to
say but I _do not_ care enough for the pedant causation versus correlation
conversation.

## The editing experience

To me, the appeal of static typing is much more evident when it comes to
editing. Here's what I settled for:

- IntelliJ for Java and Kotlin
- VS code for TypeScript and Go

Both IntelliJ and VS code are _spectacular_ tools. They excel at every aspect of
every day programming. Be that editing, renaming, refactoring, debugging,
testing. Anything really.

They're both really fast. I ask the question "how on earth is IntelliJ so fast
at doing X?" weekly. Before going back to Java in 2017, I  had used IDEs in the
early 2000s and they were horrendous back then.

In this case, I think the nature of the language does contribute to a better
experience. For example, the Go community has produced a lot of static analysis
tools and VS code uses [many of
them](https://github.com/golang/vscode-go/blob/master/docs/tools.md). It's a bit
of a catch-22 situation. These languages have better tools because the tools are
(maybe?) easier to write. They are easier to write because these languages have
better statical analysis tool.

## The workflow


- types are docs
- the cognitive load is very different
- when it compiles, it probably works
- typescript specific examples


## Experience is everything

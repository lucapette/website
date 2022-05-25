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

- The point of this article isn't to say statically typed languages are better
  than dynamically typed ones.  That means nothing.
- I will not try to convince you that you should drop your favourite language
  and go for the ones I'll discuss in the article.
- I have problems expressing my opinion when I consider something "too obvious".
  I think that statically typed languages lead to a better programming
  experience. And I do think it's obvious. So here we are, I never say no to a
  writing challenge.

These premises should clarify I mean everything that follows as _my_ personal
opinion. I'm sharing my experience hoping it may be useful to other people.

## How I got here

I started my programming journey at the end of the year 2000. I was a computer
science student at the University of Naples, Italy. We did mostly C. I liked it:
it was difficult but satisfying.

After I graduated, I did Java for a better part of the first decade of my
professional career. The language and its ecosystem felt powerful but
unproductive.

In 2009, I started looking for something different. That's how I found Ruby and
Ruby on Rails.  As an exercise, I tried to build a prototype version of a
product my team was working on for a week or so. I got to feature parity in less
than day. My programming world was shattered in minutes.

The euphoria kept me going for a few years: meta-programming was cool, RSpec was
cool, Sequel was cool. The community was fantastic: it was inclusive and
engaging. 

While I loved the language and its ecosystem, I was always uncomfortable putting
Ruby applications to production. I often trivialised the experience saying "No
matter ho many tests we add, we still ship typos to production".

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
exploit Kafka's potential, I had to write applications on the JVM. The official
libraries offered so much more than the community ones and Kafka Streams, a
wonderful streaming library I was very keen on using, was only available for
JVM.

So I reluctantly went back to Java. I told myself "well it's going to be
unproductive but I won't have to write my own Kafka Streams library". I was _so
wrong_ about the productivity part, it's almost funny to write it down now.

The developer experience had improved a few orders of magnitude since 2008 so,
before I knew it, I did only Java for a few years.

This journey is a circle: Java was my first professional programming language,
and it's now my last. While the circle interesting in itself, the languages I
went through are not what fascinates me about the journey. Here's what stands
out to me:

- Every time I changed languages, the reasons had nothing to do with language
  features. Also, I was definitely not framing my choice as dynamically typed
  versus statically typed.
- Despite I never cared much for language features, nowadays I actively try to
  avoid working with dynamically typed languages. After 2 decades of
  programming, I developed opinions about programming languages 🙃

With this circular journey in mind, let's dive into what makes me say "yep, I'm
not going to do dynamically typed languages any more".

## Focus on "actual" tests

I follow a programming principle I call [write "actual" tests]({{< ref
"/writing/my-programming-principles#write-actual-tests" >}} "write actual
tests"). Quoting myself from the article:

> A test must change only if the behaviour it verifies changed.
>
> No other reason is good enough for a test to change. Which, in turn, means
> “actual tests” only test behaviour. I've seen tests testing language features,
> or missing language features (looking at you dynamically typed languages), or
> framework features.

That "missing language features" is what I'm talking about. 

Testing dynamically typed code was bittersweet. On one side, I loved how
expressive Ruby was. On the other hand, testing Ruby code made me a little
paranoid. I could pass _anything_ at runtime to every function so I felt a
constant cognitive overload. Also, a good chunk of the tests I wrote made me
feel silly. I couldn't help but think I was covering the gap for missing
language features. I mean static types of course.

The experience with Go first and then especially with Java has been very
different from Ruby. "Actual" tests were easy to write and relatively fast. Just
one trivial example: I can "actually" test a Kafka Streams application with
[Testcontainers](https://www.testcontainers.org/) with very little effort. Add
one dependency here, two lines of code there, and bam I've got a running Kafka
instance inside my test.

Statically compiled languages allow me to focus much more on "actual" tests. I
enjoy that a lot. I like to think that the nature of these languages is the
reason why their communities produce testing tools more apt to my needs. Hard to
say but I _do not_ care enough for the pedant causation versus correlation
conversation.

I enjoy writing tests in statically typed languages more for a variety of
reasons:

- the compiler cover a lot of ground. I wrote many more tests in dynamically
  typed languages.
- Integrations tests are easier to write because of the available tooling
- The statically typed languages I worked with are so much faster than the
  dynamically typed ones that the effect is visible in testing as well.

## The editing experience

To me, the appeal of static typing is evident when it comes to editing. Here's
what I use right now:

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
are easier to statically analyse.

The flagship example of how much better the editing experience is with
statically typed language is autocompletion. 

I remember spending countless hours getting Ruby autocompletion working. It was
never good enough. 

The experience with Java or Kotlin is flawless but the best example of this
difference is JavaScript vs TypeScript. TypeScript autocompletion is so much
better that it's enough of a good reason for adoption.


The editing experience is also better for learning purposes. IntelliJ suggests
more idiomatic Java or Kotlin code all the time. It provides warnings and
suggests improvements based on best practice. 

It is a little thing but it has a significant impact on the way you can learn a
new language. Let me provide an example:

```java
val favouriteThings = listOf("Raindrops on roses", "Whiskers on kittens")
// IntelliJ will show a warning on map
println(favouriteThings.map { it.lowercase() }.joinToString(" & "))
//and suggest this:
println(favouriteThings.joinToString(" & ") { it.lowercase() })
```

If you didn't know about this, it just taught you a new API. The key is that
this happens countless times a day, especially if you're new to the language.

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
languages, I'm somewhat sure this article won't convince a single soul. It was a
non-goal for me but it's still interesting to go over why I think my experience
won't convince anyone despite it did convince me.

Back when worked only with dynamic languages, I came across these talking points
many times. None convinced me. While the experience between these two worlds is
very different, what makes it so isn't one big thing. It's more a lot of small
every day things. 


There's something else: it's hard to look for alternatives when you're happy
with what you're doing. Also, there's definitely nothing wrong with being happy
with dynamically typed languages. In fact, they're pretty awesome!

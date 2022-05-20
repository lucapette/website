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
  than dynamically typed ones. That means nothing. The point is to share how my
  perspective changed over the years. I find myself wanting to refer to this
  article often in conversations with other programmers. Now it's there so I can
  refer back to it when I need to.
- I don't want to convince you that you should drop your favourite language and
  go for the ones I'll discuss in the article. This is really just me sharing my
  experience, hoping some people will find the perspective useful.
- Writing about static vs dynamic typing is quite a challenge for me. I have
  problems expressing my opinion when I find something "too obvious". In this
  case, it's: statically typed languages lead to a better programming
  experience. Because it's obvious to me, it's hard to articulate. So here we
  are, I never say no to a writing challenge.

While the premises should clarify I mean everything that follows as _my_
personal opinion, I want to share the journey that got me here. The way I got to
my informed opinion is the most interesting aspect of the conversation. I will
expand on this as I conclude the article.

## How I got here

I started my programming journey in the early 2000s, I was a computer science
student at the University of Naples, Italy. We did mostly C. I liked it,
actually. It was difficult but satisfying, I learned a lot. My optimistic
perspective about this phase of my programming career is that C set the pain bar
pretty high so every other language I worked it since then felt easier.

After I graduated, I did mostly Java for a better part of the first decade of my
professional career. I went through two jobs where I wrote lots of Java. The
language and its ecosystem felt powerful but unproductive (many reasons but out
of the scope of this article). So I started looking for something different.
That's how I found Ruby and Ruby on Rails.

My world view was shattered in minutes. As an exercise, I tried to build a
prototype version of a product my team was working on for a week or so. I got to
feature parity in less than day. I remember reading things like:

```ruby
class Post
  has_many :comments
end
```

my Java brain exploded. I couldn't understand how any of it worked.

I _had_ to understand how it worked. So I spent a few years in the the land of
dynamically typed languages. The euphoria kept me going for a while.
Meta-programming was cool, RSpec was cool, Sequel was cool. The community was
fantastic. I left the Java world and did not look back for years.

While I loved the language and its ecosystem, I was always uneasy putting Ruby
applications to production. Writing this feels funny to me now because I realise
it took a long time to start looking for something else; I really loved Ruby!

Around 2013, I started looking into Go. The language felt pretty ugly compared
to Ruby but I immediately loved its "production qualities". It was tedious to
write Go code but I loved how robust programs felt as soon as they compiled.
Even though 2013 was still early Go days, the tooling around the language was
already very advanced compared Ruby (with the obvious exception of dependency
management which, to this day, feels more like a practical joke in Go than a
real tool).

I went back and forth between Ruby and Go for a few years. I also shared a
longish article about [my experience with Go]({{< ref
"/writing/my-experience-with-go" >}} "my experience with go") in 2017. Around
that time, I started getting much deeper into Apache Kafka.

The "problem" with is that to exploit its potential, you have to use a JVM based
language. The official libraries are fully featured and Kafka Streams, a
wonderful streaming library, is only available for JVM.

I loved streaming so much I accepted that I had to go back to Java. I told
myself "well it's going to be unproductive but I won't have to write my own
Kafka Streams library". I was so wrong about this it's kind of funny.

The developer experience had improved a few order of magnitude since 2008. Apart
from a few little things here and there, I always felt very productive with
[Spring Boot](https://spring.io/projects/spring-boot) and
[Jdbi](https://jdbi.org/). So, before I knew it, did only Java and TypeScript
for a few years.

Recently, I started looking into Kotlin and maybe I found my favourite language.
Kotlin feels a little like "Ruby with static types". That's my sweet spot. I
will probably write about it toward the end of the year.

This journey is a circle: Java was my first professional programming language,
and it's now my last. While the circle interesting in itself, the languages I
went through are not what fascinates me about the journey. Here's what stands
out to me:

- Every time I changed languages, the reasons had nothing to do with language
  features. Also, it was definitely not a dynamically typed versus statically
  typed fight.
- Despite that, today I'm pretty much convinced I don't want to work with
  dynamically typed languages any more. Which means that, after 2 decades of
  programming, I finally developed some opinions about programming languages 🙃.

With this journey in mind, let me dive into the experiences that led me here.

## Focus on "actual" tests

I follow a principle I call [write "actual" tests]({{< ref
"/writing/my-programming-principles#write-actual-tests" >}} "write actual
tests"). Quoting myself here:

> A test must change only if the behaviour it verifies changed.
>
> No other reason is good enough for a test to change. Which, in turn, means
> “actual tests” only test behaviour. I’ve seen tests testing language features,
> or missing language features (looking at you dynamically typed languages), or
> framework features.

The name is a little cheeky, I can live with that. The last part of the quote is
obviously relevant to this conversation. When I worked with Ruby a lot, I
remember testing was a bittersweet experience.

On one side, I **loved** RSpec. For real. It felt so natural to me. Its API was
fantastic.

On the other hand, testing Ruby code often felt very defensive. Lots of cases to
consider for each parameter of each method. After all, one could pass _anything_
at runtime so tests were meant to help cover edge cases.

There was a constant cognitive overheard involved in writing tests. To add to
that, a good chunk of these tests felt a little silly. Some of them were there
because of missing language features. Of course, I mean static typing.

I felt Ruby tried to keep me away from "actual" tests. The community also
developed around Ruby strengths. That happens in all languages, after all we
don't experience programming languages; we experience programming communities.
Point being Ruby has great testing tools which focus on speeding things up, unit
testing, mocking.

"Actual" tests often means integration tests and it was challenging with Ruby.
Not that I really understood this until I found a simple and effective way to
 [test CLI in Go]({{< ref
"/writing/writing-integration-tests-for-a-go-cli-application" >}} "test CLI in
Go").

The experience with Go first and then especially with Java has been very
different from Ruby. High-level "actual" tests were easy to write and relatively
fast. Because, again, these communities also developed around their languages
strengths. Which meant, for example, I could "actually" test a Kafka Streams
application with [Testcontainers](https://www.testcontainers.org/). Add one
dependency here, two lines of code there, and bam you've got a running Kafka
instance for each of your tests.

I'm not sure how much of this has specifically to do with these languages being
statically typed but, and this may surprise you, I _do not_ care about the
pedant causation versus correlation conversation. I'm just sharing my opinion as
a programmer. What I'm saying is that statically compiled languages allow me to
focus much more on "actual" tests. I enjoy that a lot.

I like to believe that is precisely the reason why these communities produce
testing tools more apt to my needs. But, of course, nor I have any idea if
that's true neither I care to figure out.

## The tooling

    * IntelliJ
    * vs code

## The workflow


## Experience is everything

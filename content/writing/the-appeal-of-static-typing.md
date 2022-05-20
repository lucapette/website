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
  always had problems expressing my opinion when I find something "too obvious".
  In this case, it's: statically typed languages lead to a better programming
  experience. Because it's obvious to me, it's hard to articulate. So here we
  are, I never say no to a writing challenge.

While the premises should clarify I mean everything that follows as my personal
opinion, I want to share the journey that got me here. The way I got to my
informed opinion is the most interesting aspect of the conversation. I will
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
applications to production. It feels funny writing this to me now because it
took a long time to start looking for something else. I really loved the
language!

Around 2013, I started looking into Go. The language felt pretty ugly compared
to Ruby but I immediately loved its "production qualities". It was tedious to
write Go code but I loved how robust programs felt as soon as they compiled.
Even though 2013 was still early Go days, the tooling around the language was
already very advanced compared Ruby (with the obvious exception of dependency
management which, to this day, feels more like a practical joke in Go than a
real tool).

I went back and forth between Ruby and Go for a few years and also shared a
longish article about [my experience with Go]({{< ref
"/writing/my-experience-with-go" >}} "my experience with go") in 2017. Around
the same time, I started getting much deeper into Apache Kafka. The "problem"
here is that to exploit Kafka potential, you have to use a JVM based language.
The official libraries are full featured and Kafka Streams, a wonderful library,
is only available for JVM.

I loved streaming so much I accepted that I had to go back to Java. I told
myself "well it's going to be unproductive but I won't have to write my own
Kafka Streams library". I was so wrong about this, it's kind of funny to write
about it.

The developer experience has improved a few order of magnitude since 2008. Apart
from a few little things here and there, I always felt very productive with
[Spring Boot](https://spring.io/projects/spring-boot) and
[Jdbi](https://jdbi.org/). So, before I knew it, did only Java and TypeScript
for a few years.

Recently, I started looking into Kotlin and maybe I found my favourite language.
Kotlin feels a little like "Ruby with static types". That's my sweet spot.

This journey is a circle, Java was my first professional programming language,
and it's now my last.

While I find the circle interesting in itself, the actual languages I went
through is not what fascinates me about about the journey. It's more the opinion
I reached: I don't really want to work with dynamically typed languages any
more. I find the experience with statically compiled languages vastly superior.

Let me dive into the experiences that, over the years, contributed the most to
reaching this place.

## Focus on "actual" tests

I follow a principle I call [write "actual" tests]({{< ref
"/writing/my-programming-principles#write-actual-tests" >}} "write actual
tests"). The idea is that tests only change when the production behaviour
changes. The name is a little cheeky, I can live with that.

## The tooling

    * IntelliJ
    * vs code

## The workflow


## Experience is everything

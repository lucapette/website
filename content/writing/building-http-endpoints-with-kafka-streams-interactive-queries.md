---
title: "Building HTTP endpoints with Kafka Streams Interactive Queries"
date: 2022-03-31T16:47:45+02:00
draft: true
tags:
  - kafka
    - kafka streams
  - kotlin
keywords: kafka, kafka streams, kubernetes, kotlin
---

{{< message class="is-info">}}
Interactive queries are a somewhat advanced topic
in the context of Kafka Streams application, so this article assumes the reader
knows the basics of Kafka Streams.
{{</ message >}}

[Interactive
queries](https://kafka.apache.org/documentation/streams/developer-guide/interactive-queries.html)
enable Kafka Streams application to query their persistent local stores. The
Kafka Streams API also provides a mechanics to query the state of remote
application instances. The feature enables us to build HTTP endpoints with
interesting properties:

- As long as all the information we need is avalable in some Kafka topic, we can
  build HTTP endpoints that do not depend on other endpoints (by querying them
  via HTTP). We avoid the traditional pain of a micro-services architecture
  where a service calls a service that calls a service...
- The local stores update really fast, so the endpoints we'll build respond to
  change quickly.
- We avoid the usual headaches of a more traditional architecture where Kafka is
  used, at best, as a ingestion and trasport layer and we move the data into a
  separate store (for example, Cassandra). Keeping the two stores in sync is a
  full time job. While it may not be too hard thanks to the Kafka Connect
  framework, relying on interactive queries has a much ligther impact on our
  infrastructure. There's a lot less moving parts. Furthermore, Kafka Streams
  _already_ uses persistent local stores for caching (so that restarting a Kafka
  Stream app does not have to read the whole computed state from internal
  topics). If you're building Kafka Streams applications, you can immediately
  rely on interactive queries (as long as you're on JVM).

As always, every architecture has a different set of trade-offs, we'll discuss
them again (especially the negative sides of this approach) once we know what
we're building. It's easier to explain them with a concrete example in mind.

So what are we building here? For the sake of the discussion, we'll imagine a
_purposely_ trivial application. The application collect words from a Kafka
topic and exposes a `/search` endpoint that takes a word and returns its count.
This requirement gives us enough to discuss the underlining architecture of this
approach. We'll then add a few more features so that we can discuss how to
evolve HTTP endpoints that use interactive queries as their sole source of data.

Now that we have some basic requirements, here's the list the tools we'll use:

- `Kotlin` I'm evaluating the language so it felt natural to use it for
  something a little more complicated that a "hello world" program.
- Spring boot Mostly a matter of fluency for me. I have used it a lot in my past
  job. It complements Kotlin nicely.

I don't intent to make a point about any of these tools by using them in this
article. There's nothing special about them in the context of Kafka Streams
application. But it's worth mentioning that HTTP endpoints with interactive
queries only make sense on JVM. Furthermore, the code presented is purposely
simple so that it would not be a distraction from the main topic.

## Trade-offs of interactive queries

## Leveraging the Kubernetes downward API

## Deploy the demo app on Minikube to play with it

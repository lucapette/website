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
  where a service calls a service that calls a service and so on.
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
them again once we know what we're building. It's easier to explain them with a
concrete example in mind.

So what are we building here? For the sake of the discussion, we'll imagine a
_purposely_ trivial application. The application collect words from a Kafka
topic and exposes a `/search` endpoint that takes a word and returns its count.
This requirement gives us enough to discuss the underlining architecture of this
approach. We'll then add a few more features so that we can discuss how to
evolve HTTP endpoints that use interactive queries as their sole source of data.

Now that we have some basic requirements, here's the list the tools we'll use:

- `Kotlin` I'm evaluating the language so it felt natural to use it for
  something a little more complicated that a "hello world" program.
- `Spring Boot` Mostly a matter of fluency for me. I have used it a lot in my
  past job.

I'm not trying to make some point about these tools by using them here. There's
nothing special about them in the context of Kafka Streams applications. But
it's worth mentioning that HTTP endpoints with interactive queries only make
sense on JVM. Furthermore, the code presented is purposely simple so that Kotlin
or Spring Boot won't be much of a distraction.

Here's the code of our topology:

```kotlin {linenos=table, hl_lines=[6]}
private fun buildTopology(): Topology {
    val builder = StreamsBuilder()

    builder.stream<String, String>("words", Consumed.`as`("words_input_topic"))
        .groupByKey(Grouped.`as`("group_by_word"))
        .count(Materialized.`as`("words_count"))

    val topology = builder.build()

    log.info(topology.describe().toString())

    return topology
}
```

Even though this is a very simple topology, there's a few things to note:

- Each node of the topology is named. It's good practice for production-ready
  streaming applications and the [official
  documentation](https://kafka.apache.org/documentation/streams/developer-guide/dsl-topology-naming.html)
  does a great job explaning the reasons for this.
- The log statement on line 10 is helpful especially in the early phases of
  developing a new Kafka Streams application.
- The highlighted line is the most important one for our conversation. We're
  telling Kafka "hey I'm going to ask you about this store later by calling it
  `words_count`. We cannot write the search method without it.

Speaking of the search method, here's a first attempt:

```kotlin
@PostMapping("/search")
fun search(@RequestBody input: SearchRequest): ResponseEntity<SearchResponse> {
    val store = kafkaStreams.store(
        StoreQueryParameters.fromNameAndType(
            "words_count",
            QueryableStoreTypes.keyValueStore<String, Long>()
        )
    )

    val count = store.get(input.query) ?: return ResponseEntity.notFound().build()

    return ResponseEntity.ok(SearchResponse(input.query, count))
}

//I like to put these classes at the end of the controller file (after the class) when they're so small.
data class SearchRequest(val query: String)

data class SearchResponse(val word: String, val count: Long)
```

Easy enough: we're telling Kafka Streams "hey remember that store I asked you to
name for me? I'm going to need it now". And then `store.get(input.query)` gives
us the count of the word we're looking for. The operator `?:` is called the
[elvis operator](https://kotlinlang.org/docs/null-safety.html#elvis-operator)
and is Kotlin idomatic way of saying "if X is not null then B otherwise do
this". We're using it here to return `404` when queried for a word we do not
know anything about.

Let's test this, shall we?

The first thing we need it to create the topic:

```sh
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic words --replication-factor 1 --partitions 3
Created topic words.
```

Before we move forward, an important warning:

{{< message class="is-warning">}}
Neither the code nor the settings presented in this article are recommended for
production use as they'd complicate the content and reduce clarity. At the end
of the article, I will provide a list of recommendations for production-ready
Kafka Streams applications.
{{< /message >}}

With that out of the way, let's add some words to the topic and check out how
our endpoint works:

```sh
# using https://github.com/httpie/httpie to send a POST request
# to an endpoint that I added to our application (you'll find the code in IngestionController)
http localhost:8080/accept word=kafka

HTTP/1.1 200
Connection: keep-alive
Content-Length: 0
Date: Sat, 02 Apr 2022 07:16:16 GMT
Keep-Alive: timeout=60
```

So the endpoint workds and it looks like Kafka ingested our word. After playing around with the endpoint a little, consuming the topic from the beginning looked like this:

```sh
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic words --from-beginning
kafka
kafka
kafka
kafka
kafka
kafka
franz
franz
franz
caste
caste
kafka
kafka
castel
trail
castle
```

Now we can check the interactive query:

```sh
http localhost:8080/search query=trail
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Sat, 02 Apr 2022 07:19:26 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "count": 1,
    "word": "trail"
}

http localhost:8080/search query=kafka
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Sat, 02 Apr 2022 07:19:35 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "count": 8,
    "word": "kafka"
}
```

it works right? Well.. not quite. For these examples, I run one application
instance so the local store contains _all_ of the state. Let's see what happens
if we add one more instance. To keep it simple, we run both instances on the
same machine so the INSTANCE_ID determines the name of the state directory:

```sh
SERVER_PORT=8081 INSTANCE_ID=1 java -jar build/libs/interactive-queries-0.0.1-SNAPSHOT.jar
```

The app will go into rebalancing state while the coordinator assigns partitions
to the new instance. Once that's done, we hit the endpoint on both instances:

```sh
http localhost:8080/search query=kafka
HTTP/1.1 404
Connection: keep-alive
Content-Length: 0
Date: Sat, 02 Apr 2022 08:18:13 GMT
Keep-Alive: timeout=60

http localhost:8081/search query=kafka
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Sat, 02 Apr 2022 08:18:18 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "count": 8,
    "word": "kafka"
}
```

So the endpoints doesn't really work. If we'd have a load balancer in front of
our instances right now, we'd get a 404 for around half the requests. So what's
going on?

In order to achieve data parallelism, Kafka Streams relies on the same ideas
(and implementation) of consumer groups. If we run a Kafka Streams application
with multiple instances, the local state of each istance will contain only a
some slices of the state (based on which partitions are assigned to the given
instance). That is the core detail we need to care when building an HTTP
endpoint on interactive queries. We need to rewrite the search endpoint using
the [RPC
mechanism](https://kafka.apache.org/documentation/streams/developer-guide/interactive-queries.html#adding-an-rpc-layer-to-your-application)
Kafka Streams provide to solve this problem.

The idea is that

## Trade-offs of interactive queries

## Leveraging the Kubernetes downward API

---
title: "Building HTTP endpoints with Kafka Streams Interactive Queries"
date: 2022-03-31T16:47:45+02:00
draft: true
tags:
  - kafka
  - kafka streams
  - kotlin
keywords: kafka, kafka streams, kotlin
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
- The highlighted line is where we're telling Kafka "hey I'm going to ask you
  about this store later by calling it `words_count`. We cannot write the search
  method without it.

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

{{< message class="is-warning">}}
<b>Warning:</b> neither the code nor the settings presented in this article are recommended for
production use as they'd complicate the content and reduce clarity. At the end
of the article, I will provide a list of recommendations for production-ready
Kafka Streams applications.
{{< /message >}}

Now let's add some words to the topic:

```sh
# using https://github.com/httpie/httpie to send a POST request
# to an endpoint that I added to our application (you'll find the code in IngestionController)
http localhost:8080/accept word=kafka

HTTP/1.1 200
Connection: keep-alive
Content-Length: 0
Date: Sat, 02 Apr 2022 07:16:16 GMT
Keep-Alive: timeout=60
# more of this...
```

So the endpoint words and it looks like Kafka ingested our word. After playing
around with the endpoint a little, the topic looked like this:

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

It works, right? Well.. not quite. For these examples, I run one application
instance so the local store contains _all_ of the state. Let's see what happens
if we add one more instance. To keep it simple, I run both instances on the same
machine so the INSTANCE_ID determines the name of the state directory:

```sh
SERVER_PORT=8081 INSTANCE_ID=1 java -jar build/libs/interactive-queries-0.0.1-SNAPSHOT.jar
```

After a quick rebalancing process, both apps are in running state and we can
test the endopint again:

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

If we'd put a load balancer in front of our instances right now, we'd get a 404
for around half the requests. Obviously the endpoint isn't really working. Why?

In order to achieve data parallelism, Kafka Streams relies on the same ideas
(and implementation) of consumer groups. If we run a Kafka Streams application
with multiple instances, the local state of each istance will contain only a
some slices of the state (based on which partitions are assigned to the given
instance). This is the detail we need to care when building an HTTP endpoint on
interactive queries. We need to rewrite the search endpoint using the [RPC
mechanism](https://kafka.apache.org/documentation/streams/developer-guide/interactive-queries.html#adding-an-rpc-layer-to-your-application)
Kafka Streams provide to solve this problem.

It's a two-step process. First we configure the RPC endpoint via
`StreamsConfig.APPLICATION_SERVER_CONFIG` so that each instance tells the
coordinator how other instances can reach them. Then we change the search
endpoint to fetch the word count via the RPC endpoint when we detect the word
requested is not avaiable in the local store. Here's the code:

```kotlin
@PostMapping("/search")
    fun search(@RequestBody input: SearchRequest): ResponseEntity<SearchResponse> {
        val store = kafkaStreams.store(
            StoreQueryParameters.fromNameAndType(
                "words_count", QueryableStoreTypes.keyValueStore<String, Long>()
            )
        )

        val keyQueryMetadata =
            kafkaStreams.queryMetadataForKey("words_count", input.query, Serdes.String().serializer())

        return when (val activeHost = keyQueryMetadata.activeHost()) {
            HostInfo(config.rpcHost, config.rpcPort) -> {
                val count = store.get(input.query) ?: return ResponseEntity.notFound().build()
                ResponseEntity.ok(SearchResponse(input.query, count))
            }
            HostInfo.unavailable() -> ResponseEntity.notFound().build()
            else -> fetchViaRPC(activeHost, input)
        }
    }
```

Our search endpoint is a little more complicated now because it knows about
other instances but still a relatively small change for what it does. Let's see
if it works:

```sh
# First we start two instances (logs not shown)
SERVER_PORT=8080 INSTANCE_ID=0 java -jar build/libs/interactive-queries-0.0.1-SNAPSHOT.jar
SERVER_PORT=8081 INSTANCE_ID=1 java -jar build/libs/interactive-queries-0.0.1-SNAPSHOT.jar
# Once the apps are both in running state, we can test the endpoint
http localhost:8080/search query=kafka
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Sun, 03 Apr 2022 07:08:26 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "count": 8,
    "word": "kafka"
}
http localhost:8081/search query=kafka
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Sun, 03 Apr 2022 07:08:30 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "count": 8,
    "word": "kafka"
}
```

It works 🎉. Now we get data no matter which instance we hit so we could put a
load balancer in front of them.

Now that we know how an interactive query endpoint looks like, we can discuss
the trade-offs of this approach. While it may sound obvious to say that an
interactive query is first of all a Kafka Streams application, it's instead a
good starting point for the discussion.

Kafka Streams application have a pretty peculiar deployment strategy. First of
all, there's no way to do rolling restarts which you may be used to for HTTP
endpoints. This stragegy conflicts with how Kafka Streams applications does
parallelism. You can't really start a new version of the app with updated code
_and_ the same `StreamsConfig.APPLICATION_ID_CONFIG`, the app would immediately
go into rebalancing mode to reassign some of the partitions. If your endpoint
can afford a little downtime, you can just restart the application. Generally,
restoration is really fast (thanks to those persistent stores we use for
interactive queries). Of course, in some context, a little downtime might be
already problematic. In such cases, the easiest way out is to deploy a new
version of the application all together (meaning with a new app id). While I
don't really advocate for "immutable deployment" in general, that's definitely
how this would look like. Also it's worth noticing you will have to do that
_anyway_ in case of a topology change. Which leads to further considerations to
make sure you know what you're getting yourself into:

- How long a Kafka Streams application takes to recompute the state can be done
  rather precisely. You need to know your end to end throughput (how long it
  takes to process a record from input nodes till the terminal nodes of your
  topology) and how many records you have in your input topics.
- How often do you expect the computation (aka the topology you wrote) to
  change? This is relevant because changing the topology always comes with a
  "new version".

While such an "immutable" strategy may feel too complicated (well it indeed is),
it enables interesting testing strategies. For example, I found myself writing
scripts to compare the JSON response of two different versions of an endpoint.
That gave me very high confidence that the new version did was working as
expected. Another inteesting consequence of this strategy is that it often
enables "wild" experimentation with new versions. Because the increased
confidence often leads to leap changes in a topology, after all you can verify
the new versions with great accuracy before promoting them to production..

Operating interactive queries endpoint is almost the same operating a regular
Kafka Streams application:

- You definitely want to be looking at lag. After all, the main argument for
  having an interactive query endpont is its "freshness".
- You want to keep an eye on the disk space of each instance. You'd be getting
  all kind of "interesting" failures if one of your interactive queries runs out
  of space.
- Because of the peculiar deployment strategy, you want to continuously collect
  throughput metrics about your application. Sometimes, the nature of a topology
  (especially when there's a lot of grouping going on) is such that the app will
  "naturally" slow down its throughput over time.

If you made it this far in the article, I would not be surprised if you're not
sure there's any good use cases for interactive queries HTTP endpoints. So
here's a list of considerations that can help you orient yourself better:

- Despite the funny deployment strategy, a Kafka Streams application is still
  _just_ a bunch of Kafka consumers/producers. Meaning you can compute and serve
  pretty complex data, at "Kafka speed", while _not_ needing to operate any
  other store.
- Up to your number of partitions, you could just add more instances to improve
  parallel processing of your data.
- If the endpoint does key lookups, you can keep response time always pretty
  low. Especially if you employ a fast RPC layer.

The one use case where I think interactive queries endpoints are a strech is
those situations in which your endpoint _always_ needs all of the state to
compute the response. For example, imagine we'd like improve our `/search`
endpont so that it doesn't only do key lookups but also needs to return things
like "top X words by count" or "least recurrent words of the month". In such
cases, you'd always need to query all the instances which may turn a little slow
(or just make the response time of your endpoint unpredictable). Having said
that, I would consider an interactive query for such a use case if the system
can afford to run the query on a single instance. Sometimes, it's also worth
spending a little more effort into designing the topology so that we can rely on
[range
queries](<https://kafka.apache.org/31/javadoc/org/apache/kafka/streams/state/ReadOnlyKeyValueStore.html#range(K,K)>)
(check out [https://github.com/ulid/spec](https://github.com/ulid/spec) for keys
design. It may be helpful in this context).

As I mentioned, the code and the settings I wrote for this article are not
recommendated for production usage. So, as promised, here's a production
checklist for interactive queries endpoints:

- You want to setup your Kafka Streams application so that it's resilient to brokers outages:

```java
// from kafka streams in action. Great book!
Properties props = new Properties();
props.put(StreamsConfig.producerPrefix(
 ProducerConfig.RETRIES_CONFIG), Integer.MAX_VALUE);
props.put(StreamsConfig.producerPrefix(
 ProducerConfig.MAX_BLOCK_MS_CONFIG), Integer.MAX_VALUE);
props.put(StreamsConfig.REQUEST_TIMEOUT_MS_CONFIG, 305000);
props.put(StreamsConfig.consumerPrefix(
 ConsumerConfig.MAX_POLL_INTERVAL_MS_CONFIG), Integer.MAX_VALUE);
```

- You want to do some [RocksDB
  tuning](https://github.com/facebook/rocksdb/wiki/RocksDB-Tuning-Guide).
- Kafka Streams stateful topologies come with internal topics. Often, it's
  helpful to do some manual tunig on them. The pointer here is "know that's an
  option to consider".
- The RPC mechanism we implemented for this article may not be the best for
  production especially if your fetching a relatively large amount of data
  (there's a lot of serialization overhead with a JSON via HTTP endpoint as
  RPC).
- You want integration tests.
- The number of partitions of the input topics is always relevant, especially so
  in an interactive queries endpoint.

I am using the code used in this article as a Kotlin playground so I will
probably work on some of this points myself. If you're intested, go give it a
star.

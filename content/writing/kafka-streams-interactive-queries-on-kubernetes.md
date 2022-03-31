---
title: "Kafka Streams Interactive Queries on Kubernetes"
date: 2022-03-31T16:47:45+02:00
draft: true
tags:
  - kafka
  - kafka streams
  - kubernetes
  - kotlin
keywords: kafka, kafka streams, kubernetes, kotlin
---

{{< message class="is-info">}}
This article assumes familiarity with Kafka, Kafka Streams, and Kubernetes.
{{</ message >}}

## What are interactive queries

[Interactive
queries](https://kafka.apache.org/documentation/streams/developer-guide/interactive-queries.html)
enable Kafka Streams application to query their persistent local stores. The
Kafka Streams API also provides a mechanics to query the state of remote
application instances. The feature allows us to build, for example, HTTP
endpoints with interesting properties.

For the sake of the conversation, we'll imagine a purposely trivial application
called WordStats. This application accepts words via a `/accept` (great naming
uh?) endpoint and returns stats via the following endponts:

- '/top' returns the top N most frequent words with their counts
- '/stats' returns the longest and shortest words with their counts

## Trade-offs of interactive queries

## Leveraging the Kubernetes downward API

## Deploy the demo app on Minikube to play with it

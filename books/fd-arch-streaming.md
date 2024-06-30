---
layout: page
style: no-image
title: Fast Data Architectures for Streaming Applications, 2nd Edition
tagline:
---
{% include JB/setup %}
<div class="book-description">
  <a href="https://www.lightbend.com/ebooks/fast-data-architectures-for-streaming-applications-oreilly-2nd-edition" target="book" class="book-image"><img src="/assets/images/FastDataArch-StreamingApps-2ndEd-256x337.png" alt="Fast Data Architectures for Streaming Applications, Second Edition"/></a>
  <div class="book-description-text">
  <p>
    <a href="https://www.lightbend.com/ebooks/fast-data-architectures-for-streaming-applications-oreilly-2nd-edition" target="book">Fast Data Architectures for Streaming Applications, Second Edition</a> is a free report co-published by <a href="https://lightbend.com" target="lightbend">Lightbend</a> and <a href="https://oreilly.com" target="window">O'Reilly</a> on the architectural characteristics of highly available, resilient, scalable, and responsive systems for data stream processing at scale. Originally published in October, 2016, the second edition was published in October, 2018.
  </p>

  <p>
    The book provides an overview of the common requirements for reliable streaming systems, based on common use cases for streaming, such as serving machine learning in a streaming context. Other requirements include the need to handle potential data loss, duplication of data, late arrival, etc. Standard system concerns, the so called <a href="https://www.reactivemanifesto.org/" target="rm">reactive principles</a> are important for such systems, where services and applications must run reliably for weeks, months, even years. If you run anything long enough, it will see every rare anomaly: hardware failures, network partitions, traffic spikes, etc. Hence, streaming systems have harder operational requirements than shorter-lived batch processes
  </p>

  <p>
    <a href="https://kafka.apache.org" target="kafka">Apache Kafka</a> is the messaging backbone of these architectures, providing high scalability and reliability for ingesting data organized into topics and orders, similar to conventional message queues.
  </p>

  <p>
    The data can then be processed by one or more stream processors, including the following four, which decompose into two groups:
  </p>

  <b>Streaming <em>Services</em></b>:

  <ul>
    <li><a href="https://spark.apache.org" target="spark">Apache Spark</a>, for a rich variety of processing options, including SQL, using a <em>minibatch</em> processing model</li>
    <li><a href="https://flink.apache.org" target="flink">Apache Flink</a>, which offers low-latency (vs. minibatch) processing with rich semantics for reliably processing.</li>
  </ul>

  <b>Streaming <em>Libraries for Microservices</em></b>:

  <ul>
    <li><a href="https://akka.io" target="akka">Akka Streams</a>, which offers very low-latency event processing with rich integrations with other systems.</li>
    <li><a href="https://docs.confluent.io/3.0.0/streams/" target="ks">Kafka Streams</a>, for low-latency processing of Kafka topics.</li>
  </ul>

  <p>
    Services like Spark and Flink do a lot of heavy lifting for you, such as automatic data partitioning, tasks management across the cluster, etc., but impose some overhead and require that your application fit their programming and execution model. Libraries like Akka Streams and Kafka Streams, provide much more flexibility, including lower latency, but don't provide automatic partitioning and task management, like Spark and Flink.
  </p>

  <p>
    Rounding out the picture are tools for building other microservices, such as the <a href="https://www.lightbend.com/platform" target="lightbend">Lightbend Platform</a>, including management and monitoring tools.
  </p>
</div>

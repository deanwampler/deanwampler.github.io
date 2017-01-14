---
layout: page
style: no-image
title: Fast Data Architectures for Streaming Applications
tagline:
---
{% include JB/setup %}

<table>
<tr>
<td>
<p><a href="http://www.oreilly.com/data/free/fast-data-architectures-for-streaming-applications.csp">Fast Data Architectures for Streaming Applications</a> is a free report co-published by <a href="http://lightbend.com">Lightbend</a> and <a href="http://oreilly.com">O'Reilly</a> on the architectural characteristics of highly available, resilient, scalable, and responsive systems for data stream processing at scale.</p>

<p>The book provides an overview of the common requirements for reliable streaming systems, including the need to handle potential data loss, duplication, late arrival, etc. Standard system concerns, the so called <a href="http://www.reactivemanifesto.org/">reactive principles</a> are important for such systems, where services and applications need to run reliably for weeks, months, even years.</p>

<p><a href="http://kafka.apache.org">Apache Kafka</a> is the messaging backbone of these architectures, providing high scalability and reliability for ingesting data organized into topics and orders, similar to conventional message queues.</p>

<p>The data can then be processed by one or more stream processors, including the following:</p>

<ul>
  <li><a href="http://spark.apache.org">Apache Spark</a>, for a rich variety of processing options, including SQL, using a <em>minibatch</em> processing model</li>
  <li><a href="http://flink.apache.org">Apache Flink</a>, which offers low-latency (vs. minibatch) processing with rich semantics for reliably processing.</li>
  <li><a href="http://akka.io">Akka Streams</a>, which offers very low-latency event processing with rich integrations with other systems.</li>
  <li><a href="http://docs.confluent.io/3.0.0/streams/">Kafka Streams</a>, for low-latency processing of Kafka topics.</li>
</ul>

<p>Rounding out the picture are tools for building other microservices, such as the <a href="http://www.lightbend.com/platform">Lightbend Reactive Platform</a>, as well as management and monitoring tools.</p>

</td>
<td class="fd-arch-streaming-cover-cell"><a href="http://www.oreilly.com/data/free/fast-data-architectures-for-streaming-applications.csp"><img src="/assets/images/FastDataArch-StreamingApps-256x337.png" alt="Fast Data Architectures for Streaming Applications"/></a></td>
</tr>
</table>

---
layout: page
title: Dean Wampler, Ph.D.
tagline: Software developer, expert in Big Data, Scala, and Functional Programming,<br/>O'Reilly author, and frequent public speaker living in Chicago.
include_social: true
---
{% include JB/setup %}

<section id="research" class="centered">
  <p class="section-title"><span>Research</span></p>
  <article class="research-item">
    <h2>Polyglot Programming</h2>
    <p>Polyglot Programming is a website dedicated to exploring the benefits (and drawbacks) of combining multiple programming languages and multiple "modularity paradigms" in application development. The "paradigms" include Functional Programming, Object-Oriented Programming, and Aspect-Oriented Programming.</p>
    <div class="more">
      <a href="polyglotprogramming/" class="button">polyglotprogramming.com</a>
    </div>
  </article>
  <article class="research-item">
    <h2>Aspect Oriented Programming</h2>
    <p>AOP is an attempt to address <em>cross cutting concerns</em> in applications. The dominant decomposition into modules usually reflects the domain model or perhaps the implementation infrastructure. However, it becomes difficult to add in globally-consistent features, like logging, that cut across these module boundaries. AOP attempts to solve this problem.</p>
    <div class="more">
      <a href="aspectprogramming/" class="button">aspectprogramming.com</a>
    </div>
  </article>
</section>

<section id="books" class="centered">
  <p class="section-title"><span>Books</span></p>
  <div class="books-list">
    <a href="books/programmingscala2.html" class="books-book"><img src="/assets/images/prog_scala_2ed_comp-quarter_size.jpg" alt="Programming Scala, 2nd Edition"/></a>
    <a href="books/fpjava.html" class="books-book"><img src="/assets/images/FPforJavaDevsCover_256x337.png"/></a>
    <a href="books/programminghive.html" class="books-book"><img src="/assets/images/prog_hive_mech_cover_front_252x331.png"/></a>
  </div>
  <p class="talk"><a href="/books">More on my books</a>.</p>
</section>

<section id="talks" class="talks centered">
  <p class="section-title"><span>Talks &amp; Papers </span></p>

  <article class="talk">
    <h1>The SMACK Stack: Emerging Fast Data and Microservice Architectures</h1>
    <p class="talk-desc">CodeNode London, October 24, 2016</p>
    <p><a href="http://spark.apache.org">Spark</a>, <a href="http://mesos.apache.org">Mesos</a>, <a href="http://akka.io">Akka</a>, <a href="http://cassandra.apache.org">Cassandra</a>, and <a href="http://kafka.apache.org">Kafka</a> form the basis for new, flexible architectures for Fast Data and Microservice applications. This talk describes the characteristics these applications need and how the SMACK combination meets those needs.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/SMACKStack.pdf" class="button-pdf">Download PDF</a>
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>Why Spark Is The Next Top (Compute) Model</h1>
    <p class="talk-desc">Numerous Venues - 2014, 2015</p>
    <p><a href="http://spark.apache.org">Spark</a> has emerged as the replacement for <span class='keyword'>MapReduce</span> in <span class='keyword'>Hadoop</span> applications. This talk explains why.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/Spark-TheNextTopComputeModel.pdf" class="button-pdf">Download PDF</a>
      <a href="https://www.parleys.com/tutorial/why-spark-is-next-top-compute-model" class="button-video">Watch video</a>
      <!-- <span class="button-video-inactive">Watch video</span> (Coming Soon) -->
    </div>
  </article>

  <article class="talk">
    <h1>Scala and the JVM for Big Data: Lessons from Spark</h1>
    <p class="talk-desc">Strata + Hadoop World, San Jose, 2016</p>
    <p>The JVM is the standard platform for Big Data and Scala is emerging as the standard programming language for Big Data Developers, driven in part by <a href="http://spark.apache.org">Spark</a>. What lessons can we draw from this picture?</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ScalaJVMBigData-SparkLessons.pdf" class="button-pdf">Download PDF</a>
      <a href="/polyglotprogramming/papers/ScalaJVMBigData-SparkLessons-extended.pdf" class="button-pdf">Longer Version</a>
      <a href="https://www.youtube.com/watch?v=7mzsZq__Oh4" class="button-video">Watch video</a>
    </div>
  </article>

  <article class="talk">
    <h1>Spark on Mesos</h1>
    <p class="talk-desc">Strata + Hadoop World London and NYC 2015</p>
    <p>While <a href="http://spark.apache.org">Spark</a> is now popular on Hadoop, managed by YARN, it emerged as demonstration project for <a href="http://mesos.apache.org">Mesos</a>. This talk explores Mesos, compares it to YARN, and argues for why you should consider a Spark + Mesos cluster.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/SparkOnMesos.pdf" class="button-pdf">Download PDF</a>
      <!-- <a href="http://www.infoq.com/presentations/spark-scala-mapreduce-java" class="button-video">Watch video</a> -->
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>The Internet Was Made for Cats</h1>
    <p class="talk-desc"><a href="http://www.meetup.com/chicagoscala/events/226860360/">Chicago Scala Users Group, Jan 2016</a></p>
    <p>My informal introduction to the <a href="https://github.com/non/cats">Typelevel Cats</a> project, including why I think it's a model for open-source development.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/Cats.pdf" class="button-pdf">Download PDF</a>
    </div>
  </article>

  <p class="talk"><a href="/polyglotprogramming/papers">See all my talks and papers</a>.</p>
</section>

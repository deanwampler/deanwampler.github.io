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
    <a href="programmingscala2.html" class="books-book"><img src="/assets/images/prog_scala_2ed_comp-quarter_size.jpg" alt="Programming Scala, 2nd Edition"/></a>
    <a href="fpjava.html" class="books-book"><img src="/assets/images/FPforJavaDevsCover_256x337.png"/></a>
    <a href="programminghive.html" class="books-book"><img src="/assets/images/prog_hive_mech_cover_front_252x331.png"/></a>
  </div>
  <p class="talk"><a href="/books">More on my books</a>.</p>
</section>

<section id="talks" class="talks centered">
  <p class="section-title"><span>Talks &amp; Papers </span></p>

  <article class="talk">
    <h1>Spark on Mesos</h1>
    <p class="talk-desc">Strata + Hadoop World London 2015</p>
    <p>While <a href="http://spark.apache.org">Spark</a> is now popular on Hadoop, managed by YARN, it emerged as demonstration project for <a href="http://mesos.apache.org">Mesos</a>. This talk explores Mesos, compares it to YARN, and argues for why you should consider a Spark + Mesos cluster.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/SparkOnMesos.pdf" class="button-pdf">Download PDF</a>
      <!-- <a href="http://www.infoq.com/presentations/spark-scala-mapreduce-java" class="button-video">Watch video</a> -->
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>Why Spark Is The Next Top (Compute) Model</h1>
    <p class="talk-desc">Numerous Venues - 2014, 2015</p>
    <p><a href="http://spark.apache.org">Spark</a> has emerged as the replacement for <span class='keyword'>MapReduce</span> in <span class='keyword'>Hadoop</span> applications. This talk explains why.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/Spark-TheNextTopComputeModel.pdf" class="button-pdf">Download PDF</a>
      <a href="http://www.infoq.com/presentations/spark-scala-mapreduce-java" class="button-video">Watch video</a> (older version)
      <!-- <span class="button-video-inactive">Watch video</span> (Coming Soon) -->
    </div>
  </article>

  <article class="talk">
    <h1>Data Science at Scale with Spark</h1>
    <p class="talk-desc">GOTO Chicago 2015</p>
    <p>Using examples, I show how to use <a href="http://spark.apache.org">Spark</a> for Data Science at scale in ways that were previously not feasible with other tools.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/DataScienceAtScaleWithSpark.pdf" class="button-pdf">Download PDF</a>
      <!-- <a href="http://www.infoq.com/presentations/spark-scala-mapreduce-java" class="button-video">Watch video</a> -->
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>The Unreasonable Effectiveness of Scala for Big Data</h1>
    <p class="talk-desc"><a href="http://event.scaladays.org/scaladays-sanfran-2015#!#schedulePopupExtras-6536">Scala Days 2015</a>.</p>
    <p>Why <em>Scala</em> has proven so effective as the general-purpose programming language for Big Data development.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/UnreasonableEffectivenessOfScalaForBigData.pdf" class="button-pdf">Download PDF</a>
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>Reactive Systems - The Why and the How</h1>
    <p class="talk-desc"><a href="http://softwarearchitectureconf.com">Software Architecture 2015</a> and <a href="http://www.meetup.com/ChicagoJUG/events/222009916/">CJUG May 2015</a></p>
    <p>What exactly are <em>Reactive Systems</em>, as described by the <a href="http://reactive-manifesto.org">Reactive Manifesto</a>, and why are they important for modern architectures?</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ReactiveSystems-TheWhyAndTheHow.pdf" class="button-pdf">Download PDF</a>
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <article class="talk">
    <h1>Error Handling in Reactive Systems</h1>
    <p class="talk-desc"><a href="http://reactconf.com/">React Conf San Francisco 2014</a>, <a href="http://softwarearchitectureconf.com">Software Architecture 2015</a></p>
    <p>Failure handling must be "first-class" in reactive systems, to satisfy the <em>resilient</em> trait. This talk discusses how reactive models and libraries support failure handling (or don't).</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ErrorHandlingReactiveSystems.pdf" class="button-pdf">Download PDF</a>
      <!-- <a href="#" class="button-video">Watch video</a> -->
      <span class="button-video-inactive">Watch video</span> (Coming Soon)
    </div>
  </article>

  <p class="talk"><a href="/polyglotprogramming/papers">See all my talks and papers</a>.</p>
</section>

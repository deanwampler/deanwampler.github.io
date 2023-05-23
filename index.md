---
layout: page
title: Dean Wampler, Ph.D.
tagline: Industry expert in AI/ML engineering, streaming data, and Scala.<br/><a href="/books">Author</a> and <a href="/talks">industry public speaker</a>.<br/>Works for <a href="https://research.ibm.com/" target="ibm">IBM Research</a>. <a href="/photography/">Photographer</a>. Lives in Chicago.

include_social: true
include_photo_social: true
---
{% include JB/setup %}

<section id="books" class="centered">
  <p class="section-title"><span>Books &amp; Reports</span></p>
  <div class="books-list">
    <a href="books/programmingscala.html" class="books-book"><img src="/assets/images/prog_scala_3ed_comp-quarter_size.jpg" alt="Programming Scala, 3rd Edition"/></a>
    <a href="books/fd-arch-streaming.html" class="books-book"><img src="/assets/images/FastDataArch-StreamingApps-2ndEd-256x337.png" alt="Fast Data Architectures for Streaming Applications"/></a>
    <a href="books/what-is-ray.html" class="books-book"><img src="/assets/images/WhatIsRay.jpg" alt="What Is Ray?"/></a>
    <a href="books/hardware-software-process.html" class="books-book"><img src="/assets/images/HardwareSoftwareProcess-256x337.png" alt="Hardware > Software > Process"/></a>
    <a href="books/fpjava.html" class="books-book"><img src="/assets/images/FPforJavaDevsCover_256x337.png"/></a>
    <a href="books/programminghive.html" class="books-book"><img src="/assets/images/prog_hive_mech_cover_front_252x331.png"/></a>
  </div>
  <p class="talk"><a href="/scala3-highlights.html">Scala 3 highlights</a>.</p>
  <p class="talk"><a href="/books">More information</a> on my books and reports.</p>
</section>

<section id="talks" class="talks centered">
  <p class="section-title"><span>Some of My Talks</span></p>

  <article class="talk">
    <h1>Reinforcement Learning: ChatGPT, Games, and More</h1>
    <p class="talk-desc">GOTO Chicago, May 2023</p>
    <p>Things move fast; an update to January's RL talk that expands the coverage of <em>Reinforcement Learning from Human Feedback</em>, a key element in training <a href="https://openai.com/blog/chatgpt" target="_chatgpt">ChatGPT</a>.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ReinforcementLearningChatGPT.pdf" class="button button-pdf">Download PDF</a>
    </div>
  </article>

  <article class="talk">
    <h1>Lessons Learned from 15 Years of Scala in the Wild</h1>
    <p class="talk-desc">Several Conferences, 2021-2022</p>
    <p>Since I joined the Scala community roughly 15 years ago, the <a href="https://scala-lang.org" target="scala">Scala</a> community has learned a lot to make the language more robust and easier to use effectively. I've also learned lots of lessons about effective "enterprise" programming using Scala. Finally, I see warning signs for FP's future growth.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/15YearsOfScala.pdf" class="button button-pdf">Download PDF</a>
      <a href="https://www.youtube.com/watch?v=cpWc7j85inQ" class="button button-video">Watch video</a>
    </div>
  </article>

  <article class="talk">
    <h1>Next Generation AI: Transitioning to the Continuous, Self-Learning Enterprise</h1>
    <p class="talk-desc">Draft, February 2021</p>
    <p>A talk I'm developing that looks at the state of AI/ML and what enterprises need to do to fully leverage it.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/NextGenerationAI.pdf" class="button button-pdf">Download PDF</a>
    </div>
  </article>

  <article class="talk">
    <h1>Modularity: A Retrospective</h1>
    <p class="talk-desc">GOTO Chicago Nights, February 18, 2020 and Scala in the City, May 28, 2020</p>
    <p>A look at what we've accomplished in making software modular and
      where we need to go.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/Modularity-a-Retrospective.pdf" class="button button-pdf">Download PDF</a>
    </div>
  </article>

  <article class="talk">
    <h1>Cluster-wide Scaling of ML with Ray</h1>
    <p class="talk-desc">YOW! Data, July 1, 2020, and CodeMesh, Nov., 2020</p>
    <p><a href="https://ray.io" target="ray">Ray</a> is a distributed computing system that offers a concise, intuitive API, with excellent performance for distributed workloads. It emerged out of the AI community at U.C. Berkeley.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ClusterWideScalingOfMLWithRay.pdf" class="button button-pdf">Download PDF</a>
    </div>
  </article>

  <article class="talk">
    <h1>Executive Briefing: What It Takes to Use ML in Fast Data Pipelines</h1>
    <p class="talk-desc">Strata San Francisco, London, and NYC 2019</p>
    <p>A briefing for managers and executives about the challenges of serving ML models in a streaming data context.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ExecutiveBriefing-WhatItTakesToUseMLinFastDataPipelines.pdf" class="button button-pdf">Download PDF</a>
    </div>
  </article>

  <article class="talk">
    <h1>Executive Briefing: What You Need to Know about Fast Data</h1>
    <p class="talk-desc">Strata London and NYC 2018</p>
    <p>A briefing for managers and executives about the trends in Fast Data and how the impact on their organizations.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/ExecutiveBriefing-WhatYouNeedToKnowAboutFastData.pdf" class="button button-pdf">Download PDF</a>
      <a href="https://info.lightbend.com/webinar-fast-data-executive-briefing-recording.html" class="button button-video">Watch video</a> (A similar webinar)
    </div>
  </article>

  <article class="talk">
    <h1>Streaming Microservices with Akka Streams and Kafka Streams</h1>
    <p class="talk-desc">Strata San Jose and London 2018, Scala Days NYC 2018, Reactive Summit 2018, YOW! 2018</p>
    <p>I discuss processing data in microservices using Akka Streams and Kafka Streams, vs. using tools like Spark and Flink.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/KafkaMicroservices-AkkaStreams-KafkaStreams.pdf" class="button button-pdf">Download PDF</a>
      <a href="https://www.youtube.com/watch?time_continue=1&v=X3FjsYz2occ" class="button button-video">Watch video</a>
    </div>
  </article>

  <article class="talk">
    <h1>Stream All the Things!!</h1>
    <p class="talk-desc">Software Architecture Conference NYC 2017, Strata London and NYC 2017</p>
    <p>I discuss the emerging architecture for large-scale stream data processing, that also integrates the best of microservice architectures.</p>
    <div class="more">
      <a href="/polyglotprogramming/papers/StreamAllTheThings.pdf" class="button button-pdf">Download PDF</a>
      <a href="https://www.youtube.com/watch?v=xZZB2JFyurY" class="button button-video">Watch video</a>
    </div>
  </article>

  <p>
    <center><a href="/polyglotprogramming/papers">All Talks</a></center>
  </p>

</section>

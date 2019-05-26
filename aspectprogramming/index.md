---
layout: page
title: Aspect-Oriented Programming
tagline:
---
{% include JB/setup %}

<span class="keyword">Aspect-Oriented Programming</span> (AOP), a.k.a. <span class="keyword">Aspect-Oriented Software Development</span> (AOSD), is an attempt to address *cross cutting concerns* in applications. The dominant decomposition into modules usually reflects the domain model or perhaps the implementation infrastructure. However, it becomes difficult to add in globally-consistent features, like logging, that cut across these module boundaries. AOP attempts to solve this problem.

AOP requires two features to implement <span class="keyword">aspects</span>, which are analogous to objects in Object-Oriented Programming:

* <span class="keyword">Quantification</span>: The ability to specify in a concise way the points in the code, called <span class="keyword">joinpoints</span>, where behavior needs to be intercepted and possibly modified. Usually this set of joinpoints is called a <span class="keyword">point cut</span> (yes, there's a space here, but not in "joinpoints").
* <span class="keyword">Advice</span>: The code to invoke at each joinpoint, which could actually replacing existing code with new behavior.

However, in practice, AOP didn't become as useful as originally expected. It works well for injecting code modifications, like monitoring, debugging, and logging logic. However, other mechanisms were found to be "good enough" for addressing cross-cutting concerns.

I developed two AOP-based tools, [Aquarium](https://github.com/deanwampler/Aquarium), an AOP toolkit for the Ruby language, and [Contract4J](/contract4j), a [Design by Contract](https://en.wikipedia.org/wiki/Design_by_contract) tool for Java.

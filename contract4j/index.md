---
layout: page
title: Contract4J
tagline: A Design By Contract Tool for Java
---
{% include JB/setup %}

<p><span class="keyword">Contract4J</span> is a tool that supports
<a href="http://archive.eiffel.com/doc/manuals/technology/contract/" target='eiffel'>Design 
by Contract&reg;</a> (DbC) programming in 
<a href="http://java.sun.com/j2se/1.5.0/index.jsp" target='java5'>Java 5</a>. Contract tests are defined using Java 5
<a href="http://java.sun.com/j2se/1.5.0/docs/guide/language/annotations.html" target='java5'>annotations</a>
and aspects written in <a href="http://www.eclipse.org/aspectj/" target='aspectj'>AspectJ</a>
evaluate the test expressions at runtime and handle failures. 
</p>
<p>
In DbC, the designer considers the conditions required for a module or method to 
successfully perform its intended services, the <i>preconditions</i>,
and what results it can guarantee to deliver, the <i>postconditions</i>, 
if the preconditions are satisfied. The designer can also specify <i>invariants</i>
that must be satisfied. 
</p>
<p>
DbC is an underused technique for improving software quality, by thinking 
through design issues more carefully and testing compliance during development
and testing. It also complements other techniques very well, such as  
Test-Driven Development (<a href="http://c2.com/cgi/wiki?TestDrivenProgramming" target='tdd1'>here</a>
and <a href="http://www.objectmentor.com/writeUps/TestDrivenDevelopment" target='tdd2'>here</a>).
</p>
<p>
<b>Contract4J</b>, like the original implementation of
DbC in <a 
href="http://archive.eiffel.com/doc/manuals/technology/contract/" target='eiffel'>Eiffel</a>
and the <a href="http://xdoclet.sourceforge.net/" target='xdoclet'>XDoclet</a>-based 
<a href="http://barter.sourceforge.net/" target='barter'>Barter</a> for Java 1.4 and earlier,
provides a standard way to define DbC tests and to evaluate them
at runtime. Barter uses XDoclet tags and generates
AspectJ test code. <b>Contract4J</b> uses Java 5 annotations to define the
condition tests and it uses "prebuilt" AspectJ aspects to advise the execution points ("join points") where the tests should be evaluated. If the tests fail, both tools terminate program execution.</p> 
<p>Annotations have a few advantages over javadoc-style tags: they can be
  included in the generated Javadocs and the JVM can be aware of the annotations at runtime. The runtime awareness can support other aspect behaviors.</p>
<p>
For more information: 
<center>
<table class="basic-table">
  <tr valign="top">
    <td><a alt="Quick Example" href="example">Quick Example</a></td>
    <td>A quick example demonstrating usage of Contract4J5.</td>
  </tr>
  <tr valign="top">
    <td><a alt="Contract4J5 README" href="c4j5">Contract4J5 README</a></td>
    <td>Detailed information on installation, usage, theory, <i>etc.</i>
    for the annotation-based version of Contract4J.</td>
  </tr>
  <tr valign="top">
    <td><a alt="Contract4JBeans README" href="c4jbeans">Contract4JBeans README</a></a></td>
    <td>Detailed information for an earlier, "JavaBeans-like" version of Contract4J</td>
  </tr>
  <tr valign="top">
    <td>Maven.</td>
    <td><a href="http://mvnrepository.com/artifact/org.contract4j5/contract4j5/0.8.0">v0.8.0</a>.</td>
  </tr>
  <tr valign="top">
    <td>GitHub:</td>
    <td><a href="https://github.com/deanwampler/Contract4J5/">GitHub Repo</a>.</td>
  </tr>
  <tr valign="top">
    <td>SourceForge:</td>
    <td>(Old releases) <a href="https://sourceforge.net/projects/contract4j/">project</a> and <a href="https://sourceforge.net/project/showfiles.php?group_id=130191">downloads</a> pages.</td>
  </tr>
  <tr valign="top">
    <td>White&nbsp;Papers</td>
    <td>I presented two papers on Contract4J at AOSD 2006 in March. They discuss the Aspect Design issues encountered while developing Contract4J and the lessons learned for building reusable Aspect libraries. The papers can be found <a href="/papers">here</a>.</td>
  </tr>
</table>
</center>
<br/>
<b>Contract4J</b> is a project of <a 
href="/aspectresearchassociates" target='ara'>Aspect Research Associates</a>.</p>


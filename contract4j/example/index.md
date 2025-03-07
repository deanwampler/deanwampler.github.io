---
layout: page
title: Contract4J Example
---
{% include JB/setup %}

<p><span class="keyword">Contract4J</span>: <b>A Quick Example</b></p>
<p>
Suppose you have a virtual phone book module with a method 
<code>search</code> that guarantees to return the phone number of a 
person if you supply non-null
fields for the person's first name, last name, and street address. (I'll
ignore the fact that a user might specify "valid" names and addresses for
non-existent people...) 
This is the <i>contract</i> for the module, in simple terms, where the
<i>preconditions</i> are the requirements for the inputs to <code>search</code>,
and the one <i>postcondition</i> is the guaranteed result.
</p>
<p>
In Java, you might validate the contract at runtime using <code>asserts</code>
or other "in-line" tests: 
</p>
<pre>
import ...PhoneNumber;
import ...Address;

class SearchEngine {
  ...
  PhoneNumber search (String first, String last, 
                      Address streetAddress) {
    assert first   != null : "bad first name";
    assert last    != null : "bad last name";
    assert address != null : "bad address";
    PhoneNumber result = 
      doSearch (first, last, streetAddress);
    assert result != null && result.isValid() > 0 
           : "bad phone number";
    return result;
  }
  ...
}
</pre>
<p>
Here I have assumed the existence of <code>Address</code> and <code>PhoneNumber</code>
classes to hide some details and I've also assumed that the latter has an
<code>isValid()</code> method to confirm the returned phone number has a
"valid" value. Note also the "worker" method <code>doSearch</code>.
</p>
<p>
The application logic in this function is cluttered by the contract "concern",
to use <a href="/aspectprogramming">aspect-oriented programming</a> terminology. 
Also, for efficiency, once testing is done and we're ready for a production
deployment, we would like to remove these tests from the code. Fortunately, 
assertions can be turned off completely or selectively when the JVM is 
started. Other, <i>ad hoc</i> contract test mechanisms may not be so flexible.
</p>
<p>It would be nice to remove the clutter, yet still do the tests. 
<span class="keyword">Contract4J</span> helps you do that. Using the Java 5 annotation form of Contract4J, called <span class="keyword">Contract4J5</span> we can define
these tests less obtrusively. 
</p>
<p>
Here is <code>SearchEngine</code> rewritten to use Contract4J5 
annotations:
</p>
<pre>
import ...PhoneNumber;
import ...Address;
import com.contract4j5.contract.*;

<b>@Contract</b>
public class SearchEngine {
  ...
  <b>@Pre</b>
  <b>@Post("$return != null && $return.isValid()")</b>
  public PhoneNumber search (String first, String last, 
                             Address streetAddress) {
    PhoneNumber result = 
      doSearch (first, last, streetAddress);
    return result;
  }
  ...
}
</pre>
<p>
The <code><b>@Contract</b></code> annotation is required at the beginning of any
class (including nested classes and derived classes) that uses the other annotations to define tests. </p>
<p>
The <code><b>@Pre</b></code> annotation defines a precondition test on the method. In this case, none of the input parameters can be null, which is the default test when no expression is defined, as shown in this example.</p>
<p> 
The <code><b>@Post</b></code> annotation defines a postcondition test. It uses the
special keyword <code>$return</code>
that represents the object or primitive data value returned by the method.
Here, the expression string defines an executable test on the returned 
phone number. Compare this expression with the <code>assert</code> 
statement used previously.
</p>
<p>
So, <span class="keyword">Contract4J5</span> reduces the clutter (and the amount of typing), while
providing great flexibility for defining, building, and running <i>Design
by Contract</i> tests.
</p>
<p>
Now, we need to apply the aspects in <span class="keyword">Contract4J5</span>
to our code. There are three ways to do this:</p>
<ol>
  <li>Compile our code with <a href="https://eclipse.org/aspectj/">AspectJ's</a> <b>ajc</b> compiler instead of <b>javac</b>. You will need to add the <code>contract4j5.jar</code> to your <code>CLASSPATH</code>.</li>
  <li>Use a binary weaving step. After you compile your code with <code>javac</code>, use <code>ajc</code> to weave in the <span class="keyword">Contract4J5</span> aspects.</li>
  <li>Use load-time weaving to apply the <span class="keyword">Contract4J5</span> aspects at runtime.</li>
</ol>
<p>
All three approaches are demonstrated by <span class="keyword">Contract4J5's</span> <b>ant</b> build process, as described in the <a href="/contract4j/c4j5">README</a>.  However, because load-time 
weaving is the easiest and the least intrusive approach (but not the most runtime-efficient),
we recommend you start with it. Here is a brief description of how to use it.</p>
<ol>
  <li>Install <a href="https://eclipse.org/aspectj/">AspectJ</a> v1.5.3 or later.</li>
  <li>Make sure the <code>ASPECTJ_HOME</code> environment variable is defined.</li>
  <li>Add the <code>contract4j5.jar</code> to your <code>CLASSPATH</code>.
  <li>Copy <code>test/META-INF/aop.xml</code> to a <code>META-INF</code> directory 
    at the root of your application or somewhere in your CLASSPATH. 
    (Typically, <code>META-INF</code> would be a top-level directory in your application's jar file.)</li>
  <li>Edit the <code>aop.xml</code> and change the <code>&lt;include within="..."&gt;</code> tag to 
    the correct packages or classes for your application.</li> 
  <li>Invoke your application (<i>e.g.,</i> JUnit tests) using the <code>ASPECTJ_HOME/bin/aj5</code> (*nix) or <code>%ASPECTJ_HOME%\bin\aj5.bat</code> (Windows) script, instead of <code>java</code>.</li>
</ol>
<p>
That's it! The <span class="keyword">Contract4J5</span> aspects will be applied as your application's classes as they are loaded. For production deployments, simply use <code>java</code> to invoke your application, as before.</p>
<p>
<b>Note:</b> To see how to invoke JUnit tests from ant with load-time weaving, see the <code>_junitTemplate.ltw</code> build target in <code>ant/targets.xml</code>.</p>

<p>
See the <a href="/contrct4j/c4j5">README</a> for more detailed information on installation, usage, and theory.
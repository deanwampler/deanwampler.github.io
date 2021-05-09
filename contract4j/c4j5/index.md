---
layout: page
title: Contract4J5 V0.9.0.X README
---
{% include JB/setup %}

<p><strong>Design-by-Contract&#174; for Java Using Java 5 Annotations</strong></p>

<p>Dean Wampler<br/>
<a href="/contract4j">polyglotprogramming.com/contract4j</a><br/>
A project of <a href="/aspectresearchassociates">Aspect Research Associates</a></p>

<h3>Contents</h3>
<ul>
  <li><a href='#copyright'>Copyright</a></li>
  <li><a href='#wheretogetc4j'>Where to Get Contract4J5</a></li>
  <li><a href='#versioning'>Naming and Versioning</a></li>
  <li><a href='#manifest'>Distribution Manifest</a></li>
  <li><a href='#whatisc4j'>What Is "Contract4J5"?</a></li>
  <li><ul>
    <li><a href='#whatisdbc'>What is Design by Contract?</a></li>
    <li><a href='#dbcandaop'>Design by Contract and Aspect-Oriented Programming</a></li>
    <li><a href='#how'>How Does Contract4J5 Support Design by Contract?</a>
      <ul>
        <li><a href='#inherit'>Inheritance Behavior of Contracts</a></li>
      </ul></li>
    <li><a href='#usage'>How Do I Use Contract4J5?</a></li>
    <li><a href='#distribution'>Distribution</a></li>
    <li><a href='#building'>Building Contract4J5</a></li>
    <li><a href='#showme'>Show Me the Code!</a></li>
    <li><a href='#details'>Details of Contract Specifications</a></li>
    <li><a href='#limits'>Known Limitations</a></li>
    <li><a href='#tips'>Miscellaneous Notes and Debugging Tips</a></li>
    <li><a href='#config'>Configuration of Contract4J5</a></li>
  </ul></li>
  <li><a href='#todo'>TODO Items</a></li>
  <li><a href='#history'>History</a></li>
  <li><a href='#notes'>Notes for Each Release</a></li>
  <li><a href='#furtherinfo'>For Further Information...</a></li>
</ul>

<p><strong>Impatient?</strong> If you want to get started quickly, go to
  <a href='#showme'>Show Me the Code!</a>.</p>

<p><a name="copyright"></a></p>

<h2 id="copyright">Copyright</h2>

<p>Contract4J is open source software covered by the Eclipse Public License - v1.0. A complete copy of the license is shown below and it can be found in the LICENSE-ECLIPSE.txt file in the top-level directory of the distribution.</p>

<pre><code>===========================================================
Copyright 2005-2011 Contract4J Development Team

   Licensed under the Eclipse Public License - v 1.0; you
   may not use this software except in compliance with the
   License. You may obtain a copy of the License a

       https://www.eclipse.org/legal/epl-v10.html

   A copy is also included with this distribution. See the
   "LICENSE" file. Unless required by applicable law or
   agreed to in writing, software distributed under the
   License is distributed on an "AS IS" BASIS, WITHOUT
   WARRANTIES OR CONDITIONS OF ANY KIND, either express or
   implied. See the License for the specific language
   governing permissions and limitations under the License.
==========================================================
</code></pre>

<p>In addition, several third-party components are used by Contract4J5. Their licenses are described by the LICENSE.txt file.
<a name='wheretogetc4j'></a></p>

<h2 id="where_to_get_contract4j5">Where to Get Contract4J5</h2>

<p>Contract4J5 is hosted on <a href="https://github.com/deanwampler/Contract4J5">GitHub</a> (note that the project is called <em>Contract4J5</em>). Builds can be downloaded from <a href="https://sourceforge.net/project/showfiles.php?group_id=130191">SourceForge</a>. The home page is <a href="/contract4j">polyglotprogramming.com/contract4j</a>.</p>

<p><strong>NOTE:</strong> The previous web site for Contract4J5 was contract4j.org.
<a name='versioning'></a></p>

<h2 id="naming_and_versioning">Naming and Versioning</h2>

<p>The active development project is <strong>Contract4J5</strong>, which uses Java 5 annotations to define the contracts. There is a separate, dormant project called <strong>Contract4JBeans</strong>, which was an interesting, but not very successful experiment that required no Java 5 annotations, but relied on method naming conventions to define contracts for target methods and classes. The JavaBeans-like naming conventions were the origin of the Contract4JBeans name. Contract4JBeans can be found only as a jar distribution on the <a href="https://sourceforge.net/project/showfiles.php?group_id=130191">SourceForge</a> site.</p>

<p>The newest release of Contract4J5 described in these notes is version 0.9.0. As this is a &#8220;pre-1.0&#8221; release, it is possible that planned features and other changes may force API changes that break backwards-compatibility. However, at this point, we anticipate no such problems going forward (except where documented in the TODO Items).</p>

<p><a name='manifest'></a></p>

<h2 id="distribution_manifest">Distribution Manifest</h2>

<p>Contract4J5 comes in *.zip and *.tar.gz formats. For convenience, each archive includes the required Jexl, Groovy, JRuby and dependent libraries. If you are using only one of the languages, a table below lists which of the jars is required for each language choice. (The Spring framework is not included. It is required to build the optional Spring-configuration example.)</p>

<p>After expanding the archive you will have the following directory structure:</p>

<table>
  <tr><th>File or Directory</th><th>Description</th></tr>
  <tr><td>contract4j5_XYZ.jar</td><td>Pre-built Contract4J5 jar.</td></tr>
  <tr><td>lib</td><td>Location of Jexl, Groovy, JRuby and dependent libraries.</td></tr>
  <tr><td>contract45</td><td>Directory with the main distribution files</td></tr>
  <tr><td>contract4j5WithSpring</td><td>Directory with an example of configuring Contract4J5 using Spring 1.2.X or Spring 2.0. The example works with either release (it uses the 1.2.X API, which is supported by 2.0). We recommend using Spring for customizations. However, without any configuration, Contract4J5 works with suitable defaults. There is also a property configuration option, demonstrated in the JUnit tests and discussed in more detail below.</td></tr>
</table>

<p>Inside the contract4j5 directory, you will find:</p>

<table>
  <tr><td>ant</td><td>Directory of ant support files.</td></tr>
  <tr><td>build.bat</td><td>Window&#8217;s build driver script.</td></tr>
  <tr><td>build.sh</td><td>Linux/MacOSX/Cygwin build driver script.</td></tr>
  <tr><td>build.xml</td><td>Ant build script.</td></tr>
  <tr><td>Contract4J.properties.example</td><td>Example properties file for configuring Contract4J5 using properties. Note that we recommend using Spring for this purpose.</td></tr>
  <tr><td>doc</td><td>Created during a full build; where JavaDocs go.</td></tr>
  <tr><td>env.bat</td><td>Windows environment variable setup for running the build. Invoked by build.bat. Edit this file for your environment.</td></tr>
  <tr><td>env.sh</td><td>Linux, MacOSX, Cygwin environment variable setup for running the build. Invoked by build.sh. Edit this file for your environment.</td></tr>
  <tr><td>jdepend.sh</td><td>*nix driver script for jdepend. Uses XSLT and Graphviz to generate a PNG file with the results. See the script for more information.</td></tr>
  <tr><td>jdepend.bak</td><td>Created when you run the jdepend.sh script; results from previous runs are stored here.</td></tr>
  <tr><td>jdepend.properties</td><td>Configure jdepend.</td></tr>
  <tr><td>jdepend_report.png</td><td>Example output generated by jdepend.sh.</td></tr>
  <tr><td>LICENSE.txt</td><td>Contract4J license file (Eclipse Public License 1.0). Other license files for included components may also appear in this directory.</td></tr>
  <tr><td>README.html</td><td>This README.</td></tr>
  <tr><td>src</td><td>Directory with all the non-test source files.</td></tr>
  <tr><td>src-classes</td><td>Created during a build; where the src classes go.</td></tr>
  <tr><td>test</td><td>Directory with all of Contract4J5&#8217;s JUnit test sources.</td></tr>
  <tr><td>test-classes</td><td>Created during a build; where the test classes go.</td></tr>
  <tr><td>lib</td><td>3rd-party libraries required by Contract4J5.</td></tr>
</table>

<p><a name='whatisc4j'></a></p>

<h2 id="what_is_8220contract4j58221">What Is &#8220;Contract4J5&#8221;?</h2>

<p>Contract4J5 supports &#8220;Design by Contract&#8221;&#174; programming in Java. a programming practice introduced by Bertrand Meyer and incorporated in the <em>Eiffel</em> programming language in the 1980&#8217;s.</p>

<p>The Contract4J5 project is sponsored by <a href="/aspectresearchassociates">Aspect Research Associates</a>, a consulting firm specializing in Scala, Data Analytics, Aspect-Oriented Programming, Enterprise Java, and Ruby on Rails.</p>

<p><a name='whatisdbc'></a></p>

<h3 id="what_is_design_by_contract">What is Design by Contract?</h3>

<p><em>Design by Contract</em>&#174; (DbC) starts with the observation that, implicitly or explicitly, a component defines a &#8220;contract&#8221; with its clients. When a client invokes an operation on the component, it must agree to provide the component with appropriate inputs and context. Otherwise, the component can&#8217;t perform its services. In return, if the input constraints are satisfied the component guarantees delivery of prescribed results.</p>

<p>DbC encourages the component developer to state the contract explicitly, by specifying the input constraints, known as &#8220;preconditions&#8221;, and guaranteed results, known as &#8220;postconditions&#8221;, in a programmatic form that can be tested at runtime. In addition, state &#8220;invariants&#8221; may be defined.</p>

<p>DbC is a powerful and underused tool for detecting bugs during development and testing. A key principle is that if a test fails during execution, the program terminates abruptly. While this may seem draconian, it forces the developer to solve the problem immediately, rather than allow problems to &#8220;slide&#8221;, multiple, and thereby undermine the quality of the software.</p>

<p>Hence, during development, all tests are enabled and the code is thoroughly tested. During deployment, the tests are often disabled, both to prevent sudden shutdown and and to allow possible recovery should a contract-violating condition arise that was never detected during development. Turning off the tests also removes their overhead.</p>

<p>As such, DbC is a wonderful complement to Test-Driven Development, which exercises the code and hence the contract tests, thereby increasing the probability they will detect bugs. DbC tends to emphasize the fine-grained design a little more than test-driven development by itself. Designing the unit tests and specifying the contracts also force the developer to think through the details of the design before writing the code. A third technique that supports thinking through the design is to write the comment blocks for classes and methods before implementing them.</p>

<p>For more information on Design by Contract, see the references below.</p>

<p><a name='dbcandaop'></a></p>

<h3 id="design_by_contract_and_aspect_oriented_programming">Design by Contract and Aspect-Oriented Programming</h3>

<p>So what does DbC have to do with <a href="https://www.aspectprogramming.com/home/aosd">Aspect-Oriented Software Development</a> (Aspect-Oriented Programming - AOP - for short)? On the one hand, the component&#8217;s contract is an essential part of the complete, logical component specification that clients must support. For example, an interface for a bank account may have a contract requirement that all methods that return a balance must always return a non-negative number (ignoring overdraft features). However, in practical terms, contracts often include implementation concerns that may have little relationship to the domain logic of the application. For example, the code implementing the bank account may prohibit passing null values as method parameters.</p>

<p>For both types of contract details, AOP allows us to specify the details with sufficient &#8220;proximity&#8221; to the interface so that clients can see the constraints and AOP gives us an elegant way of testing the constraints at runtime without cluttering the code with logic to run the tests and handle failures.</p>

<p>More generally, AOP is a new approach to modularizing &#8220;concerns&#8221; that need to be handled by a component, but which tend to obscure the main logic of the component, often compromising clarity, maintainability, reusability, etc. For example, modern web and enterprise applications typically must support secure access, transactional behavior, persistence of data, and mundane support issues like logging. Without AOP, the code for these &#8220;concerns&#8221; gets mixed in with the domain logic, thereby cluttering the code and diminishing the &#8220;ilities&#8221; we all strive for. AOP keeps these concerns in separate modules and provides powerful facilities for &#8220;injecting&#8221; the concern behavior in the specific execution points where needed. Contract4J5 uses AOP techniques to find the contract specifications and test them at runtime at the appropriate execution points.</p>

<p>AOP is a good approach to supporting DbC because it permits DbC concerns to be managed in a modular and minimally-intrusive way, without cluttering application logic, while still allowing the contracts to be integrated into the runtime environment for development and testing. Contract4J5 uses the best-known AOP language, AspectJ, to support DbC for Java.</p>

<p>For more information on AOP, see the references below.</p>

<p><a name='how'></a></p>

<h3 id="how_does_contract4j5_support_design_by_contract">How Does Contract4J5 Support Design by Contract?</h3>

<p>I&#8217;m a long-time believer in DbC and wanted to use it in Java. A few years ago, I discovered the clever Barter project, which supports DbC in Java using XDoclet tags and AspectJ code generation to perform the tests as &#8220;advice&#8221;.</p>

<p>Two problems with doclet-based approaches are that they are buried in the comments, which means they are decoupled from the runtime environment, and a preprocessor step is required. Java 5 introduced annotations, which are like &#8220;JavaDoc tags for code&#8221;. In particular, annotations are used to ascribe meta-information to the code and to make that information available to the runtime environment, when desired. Annotations are a logical tool for associating contract tests with code and Contract4J5 uses them for this purpose.</p>

<p>The specifications are written as executable Java expressions, enclosed in the String &#8220;value()&#8221; attribute of the annotation. The details are discussed below. A contrived example suffices for now:</p>

<pre><code>@Contract
public class MyClass {
  @Invar ("name != null")
  private String name;

  @Pre ("n != null")
  public void setName (String n) { name = n; }
  @Post ("$return != null")
  public String getName () { return name; }

  @Pre ("n &gt; 10 &amp;&amp; s != null")
  @Post ("$return != null")
  public String  doIt (int n, String s) {...}
  ...
}
</code></pre>

<p>The <code>@Contract</code> annotation tells Contract4J5 that <code>MyClass</code> defines tests. The tests are defined using <code>@Pre</code>, <code>@Post</code>, and <code>@Invar</code> annotations, for <em>precondition</em>, <em>postcondition</em>, and <em>invariant</em> tests respectively. In this example, the tests are:</p>

<p>The field <code>name</code> has an invariant test that it can never be <code>null</code> (after the object has been constructed&#8230;).</p>

<p>The <code>setName</code> method has a precondition that the value of the parameter cannot be <code>null</code>.</p>

<p>The <code>getName</code> method has a postcondition that it can never return <code>null</code>.</p>

<p>The <code>doIt</code> method has both a pre- and a postcondition test.</p>

<p>If a test fails, Contract4J5 throws an unchecked exception, <code>ContractError</code>, after printing some diagnostic information (see the unit tests for examples). A special subclass of <code>ContractError</code> is thrown if the test itself can&#8217;t be evaluated for some reason (discussed in more detail below). That error is <code>TestSpecificationError</code>. So, clients who want to catch contract errors and also distinguish between these two types of failures should follow this idiom:</p>

<pre><code>try {
  ...
} catch (TestSpecificationError tse) {
  ...
} catch (ContractError ce) {
  ...
}
</code></pre>

<p>The versions of Contract4J5 before V0.7 embedded the Jakarta <strong>Jexl</strong> interpreter, an expression evaluator, to evaluate the test expressions in the annotations. However, there are quirks and limitations of Jexl (discussed throughout this README). In V0.7, Contract4J5 added support for <strong>Groovy</strong> and <strong>JRuby</strong>, as well as Jexl, as scripting engine options. The tests exercise all three options.</p>

<p>In V0.8, we have <strong>deprecated</strong> JRuby and we may deprecate <strong>Jexl</strong> before the final release. As we expanded the test suite to better exercise test expressions for generic classes and test expressions that use other objects and statics in other classes, it became clear that maintaining three engines is not cost-effective, as each has its quirks.</p>

<p>Therefore, we have standardized on <strong>Groovy</strong> as the best compromise between performance and full support for Java 5 features. However, Jexl offers the best performance (about 2 times faster than Groovy, when running the test suite), so we may continue supporting it, even though Jexl doesn&#8217;t appear to be as full-featured as Groovy (e.g., tests for generic classes don&#8217;t seem to work).</p>

<p>The V0.9 release does continue support for JRuby, but removal is planned before the 1.0 release. While there is great interest in JRuby and Java interoperability, JRuby is not an ideal choice for Contract4J, because it&#8217;s more useful for the syntax of the scripts to be as close to Java syntax as is &#8220;reasonable&#8221;. </p>

<p>Here are some performance numbers, from the V0.7 release, for the different scripting engines. (The numbers are a little bigger for V0.8+, due to the increase in the number of tests in subsequent release.) The numbers compare the performance of load-time weaving (LTW) vs. binary weaving and also the performance of the JVMs in JDK 5 vs. JDK 6.</p>

<table>
  <tr><td colspan="6">JDK 5 (sec.)</td><td colspan="6">JDK 6 (sec.)</td></tr>
  <tr><td colspan="2">Jexl</td><td colspan="2">Goovy</td><td colspan="2">JRuby</td><td colspan="2">Jexl</td><td colspan="2">Goovy</td><td colspan="2">JRuby</td></tr>
  <tr><td>Binary</td><td>LTW</td><td>Binary</td><td>LTW</td><td>Binary</td><td>LTW</td><td>Binary</td><td>LTW</td><td>Binary</td><td>LTW</td><td>Binary</td><td>LTW</td></tr>
  <tr><td>21.7</td><td>98.6</td><td>54.9</td><td>324.4</td><td>79.6</td><td>189.4</td><td>17.7</td><td>62.9</td><td>44.8</td><td>133.5</td><td>63.2</td><td>108.4</td></tr>
</table>

<p>These times are approximate &#8220;user times&#8221;, averaged over a few runs, measured using the &#8220;time&#8221; shell command on a ThinkPad T42 running Ubuntu Linux. There are slightly different build activities and I/O overhead involved in the LTW numbers, adding a few percentage points to the numbers. The tests do some different manipulations depending on which interpreter is used. Hence, the results are rough, at best. Most likely, the numbers reflect the relative amounts of overhead to load the interpreters and to set up the &#8220;environments&#8221; for evaluating the scripts. Your mileage may vary&#8230;</p>

<p>Note that the JDK 6 performance is significantly better. The code was compiled with the JDK 5 javac and executed using the JDK 6 JVM. Ironically, we saw slightly slower performance when we compiled with JDK 6 (not shown in the table).</p>

<p>The Jexl test runs are roughly twice as fast as the Groovy and JRuby runs, probably because Jexl is a more &#8220;minimalist&#8221; environment, incurring less startup and execution overhead.</p>

<p>While not shown, we observed that the memory requirements for Groovy and JRuby are also higher. In fact, we had to increase the maximum heap size for the LTW JUnit run. We also increased the heap size for the binary JUnit runs in Eclipse, when using JRuby.</p>

<p>The LTW test runs take significantly longer, but the results are somewhat hard to interpret. Under JDK 5, the Jexl and Groovy runs are five to six times longer, while the JRuby runs are only about 2.5 times longer. Under JDK 6, the Jexl and Groovy runs are a little more than three times longer, while the JRuby run is under twice as long. It is not clear while JRuby and LTW performs so much better under both JDKs.</p>

<p>In general, the longer LTW times occur because a weaving step happens at the beginning of each TestCase, as the class is loaded. The JDK 6 speed-up is a good argument for running this VM, at least for your tests!</p>

<p>LTW is the most convenient way to adopt Contract4J5 (as well as other aspects, in general), but if the startup time is too long for your needs, then binary weaving of compiled code provides better performance with only a modest change in the build process.</p>

<p>You can select which interpreter to use at runtime by passing one of the following flags to the JVM:</p>

<pre><code>-Dinterpreter=groovy (the default)
-Dinterpreter=groovybsf (Groovy using BSF - see below)
-Dinterpreter=jexl
-Dinterpreter=jexlbsf (Jexl using BSF - see below)
-Dinterpreter=jruby
</code></pre>

<p>If you use the Contract4J <code>PropertiesConfigurator</code>, you can use the following property:</p>

<pre><code>org.contract4j5.ExpressionInterpreter=org.contract4j5.interpreter.jexl.JexlExpressionInterpreter
</code></pre>

<p>(Jexl example). For Spring, the tests for the include Spring example <code>applicationContext-contract4j5.xml</code> demonstrates setting Groovy as the interpreter. There are more details on language configuration below.</p>

<p>For more information on the differences between the different scripting languages, see the V0.7.0 release notes.</p>

<p><a name='inherit'></a></p>

<h3 id="inheritance_behavior_of_contracts">Inheritance Behavior of Contracts</h3>

<p>In DbC, there are rules for proper behavior of inherited contracts, based on the <em>Lyskov Substitution Principle</em> (LSP), which is a minimal definition of inheritance. Class B is considered a child class of class A, if objects of type B caN be substituted for objects of type A without breaking the program. In DbC terms, this means that class B must obey A&#8217;s contract, including all the class, method, and field tests.</p>

<p>However, there is one nuance affecting derived preconditions and postconditions. A derived (overriding) precondition test can actually satisfy a &#8220;looser&#8221; restriction than the overridden test. Put another way, the set of valid inputs can be larger than the set for the parent (overridden) test. This is because precondition tests are tests the client must meet, so if a client already meets a strict test defined by the overridden test, then it will also satisfy a looser derived test transparently. This is &#8220;contravariant&#8221; behavior because even though subclassing is a form of increasing specialization and restriction, the restrictions imposed by the precondition test can actually grow looser.</p>

<p>In contrast, postcondition tests are &#8220;covariant&#8221;, meaning they must be as narrow or narrower than the tests they override. This is true because postcondition tests are tests on the results the component promises to deliver, as opposed to tests on clients. So, if the client is expecting a result in a set of possible results and a derived test narrows the set further, then the result will still satisfy the client&#8217;s expectations.</p>

<p>Contract4J5 provides only minimal support for contravariant precondition tests and covariant postcondition tests. First, because Java 5 annotations on methods are NOT inherited, it is a requirement for writers of subclass method overrides to also include the annotations on the parent method. However, the annotations do not have to reproduce the test expressions. C4J5 will locate the corresponding parent class test expressions automatically. In contrast, class-level invariants are inherited, since class annotations can be inherited. (However, it is harmless to repeat those in subclasses, too.)</p>

<p>C4J5 attempts to enforce the rule that invariant tests can&#8217;t change. However, it uses a simple string comparison, ignoring whitespace, so some logically equivalent expressions may get flagged incorrectly as different. For example:</p>

<pre><code>a == b   vs.   b == a
</code></pre>

<p>will appear to be different, when they are logically equivalent.</p>

<p>To properly write contravariant precondition tests and covariant postcondition tests, you will have to repeat the &#8220;inherited&#8221; test expression and &#8220;append&#8221; the appropriate refinements, e.g.,</p>

<pre><code>@Pre("new_test || parent_test")
@Post("new_test &amp;&amp; parent_test")
</code></pre>

<p>However, since the parent test is always valid for the derived method override, if you don&#8217;t need to modify the test, then you can simply use the <code>@Pre</code> or <code>@Post</code> annotation without a test expression and C4J5 will find the parent expression.</p>

<p>We are considering ways to support the correct inheritance behavior before the 1.0 release of Contract4J5.</p>

<p><a name='usage'></a></p>

<h3 id="how_do_i_use_contract4j5">How Do I Use Contract4J5?</h3>

<p>The distribution contains ant files and examples of how to use Contract4J5. The examples are actually part of the unit/acceptance test suite. If you are using Eclipse, the project configuration files are also in the source distribution. You may or may not want to remove them.</p>

<p><strong>NOTE:</strong> The tests/examples are the best way to see how to write test expressions correctly!</p>

<p>Installation and Configuration:</p>

<p>For Linux/Unix systems, use these commands:</p>

<pre><code>1) cd ~/work        # or wherever...
3) cp .../contract4j5_XYZ.tar.gz .
4) tar xvzf contract4j5_XYZ.tar.gz
</code></pre>

<p>On Windows systems, Unzip the zip file to an appropriate location.
You will need to have Java 5 or Java 6 and AspectJ 5 installed to use Contract4J5. You will need to select which scripting language you prefer for the annotation scripts used to define contracts. (See the notes above about plans to deprecate JRuby and possibly Jexl.) The distribution includes all the currently-supported options; Jakarta Commons Jexl 1.1, Groovy 1.0, and JRuby 1.0. (Several other jars, such as Commons Logging, are also included, which are required by the scripting engines).</p>

<p>If you build Contract4J5 yourself, you will also need JUnit 3.8.X.</p>

<p><strong>Note:</strong> You must use AspectJ v1.5.3 or later if you want to use load-time weaving (LTW) with Contract4J5.</p>

<p>Once you have started adding contract tests to your code, there are three ways to enable Contract4J5&#8217;s contract enforcement:</p>

<ol>
<li>Compile our code with AspectJ&#8217;s <code>ajc</code> compiler instead of <code>javac</code>. You will need to add the <code>contract4j5.jar</code> to your <code>CLASSPATH</code>.</li>
<li>Use a <em>binary weaving</em> step. After you compile your code with <code>javac</code>, use <code>ajc</code> to weave in the Contract4J5 aspects.</li>
<li>Use <em>load-time weaving</em> (LTW) to apply the Contract4J5 aspects at runtime.</li>
</ol>

<p>All three approaches are demonstrated by Contract4J5&#8217;s ant build process, as described below. We describe load-time weaving (LTW) here. It is the easiest and the least intrusive approach to adopt and we recommend you start with it. Recall, however, from the discussion above, that the performance of LTW is slower than &#8220;binary&#8221; weaving (discussed below), so you may wish to convert to that approach at a later time.</p>

<p>To use LTW, follow these steps.</p>

<ol>
<li>Install AspectJ v1.5.3 or later.</li>
<li>Make sure the <code>ASPECTJ_HOME</code> environment variable is defined.</li>
<li>Add the <code>contract4j5.jar</code> to your <code>CLASSPATH</code> and your <code>ASPECTPATH</code>, if using Eclipse or <code>ajc</code> (i.e., <code>-aspectpath</code> &#8230;).</li>
<li>Copy and adapt the <code>test/META-INF/aop.xml</code> file to your project.</li>
<li>Invoke your application (e.g., JUnit tests) using the <code>ASPECTJ_HOME/bin/aj5</code> (*nix) or <code>%ASPECTJ_HOME%\bin\aj5.bat</code> (Windows) script, instead of java.</li>
</ol>

<p>Now the Contract4J5 aspects will be applied as your application&#8217;s classes as they are loaded. For production deployments, simply use java to invoke your application, as before.</p>

<p><strong>Note:</strong> To see how to invoke JUnit tests from ant with load-time weaving (LTW), see the <code>_junitTemplate.ltw</code> build target in <code>ant/targets.xml</code>. This example also provides more details on how to use LTW with Contract4J5.</p>

<p>If you are already using the AspectJ <code>ajc</code> compiler to compile your Java and AspectJ sources, then simply include <code>contract4j5.jar</code> in the <code>-aspectpath</code> argument to <code>ajc</code>.</p>

<p>Another alternative, whether you use <code>ajc</code> or <code>javac</code> to build your code, is to add a final <em>binary weaving</em> step to your build.</p>

<p>To see how to do a binary weaving step, consult the <code>ant</code> build files in the distribution, in particular, the <code>test</code> target and dependencies. The <code>compile.test</code> target uses <code>javac</code> to compile the JUnit test code. The <code>project-test.jar</code> target uses <code>ajc</code> to do binary weaving, where the compiled class files are read by <code>ajc</code>, aspects are woven into them (note the <code>-aspectpath</code> option to <code>ajc</code>) and the <code>contract4j5-test.jar</code> file is output. The <code>ant/targets.xml</code> defines the <code>binaryWeaveTemplate</code> target used for this process.</p>

<p><strong>Note:</strong> To see what the <code>ajc</code> command does, invoke ant with the option <code>-Dbuild.compiler.verbose=true</code> which will cause <code>ajc</code> to print the command-line options used. Look for the output that is part of the <code>compile.test</code> target. This output may be easier to understand than trying to understand the ant files!</p>

<p>You can use the installed <code>contract4j5.jar</code> file as is. If you want to rebuild Contract4J5, use the ant driver script <code>build.sh</code> or <code>build.bat</code>. First, edit the corresponding <code>env.sh</code> or <code>env.bat</code> file and change the environment variable definitions as appropriate for your environment. Or, you can define the appropriate environment variables in your environment and use the <code>build.xml</code> ant script directly.</p>

<p><a name='distribution'></a></p>

<h2 id="distribution">Distribution</h2>

<p>The distribution has the following structure:</p>

<table>
  <tr><td>File/Directory</td><td>Description</td></tr>
  <tr><td>README.html</td><td>This file.</td></tr>
  <tr><td>LICENSE.txt</td><td>The Apache 2 license file for Contract4J5.</td></tr>
  <tr><td>build.sh</td><td>Unix/Linux build driver script. Sets &#8220;home&#8221; variables where the tools are found. Edit to taste.</td></tr>
  <tr><td>build.bat</td><td>Windows build driver script. Also sets &#8220;home&#8221; variables&#8230;</td></tr>
  <tr><td>build.xml</td><td>Ant build script.</td></tr>
  <tr><td>src</td><td>The source code tree.</td></tr>
  <tr><td>test</td><td>The JUnit unit/acceptance tests, which also function as usage examples. The files ending with &#8220;*Test.java&#8221; are JUnit tests. The other classes under &#8220;test&#8221; are example classes used by the tests, which also provide C4J5 usage examples. The JUnit test files often contain additional example classes that demonstrate usage and they contain comments about tests the demonstrate known idiosyncrasies or limitations of C4J5 and script evaluation using Jexl, Groovy, and JRuby.</td></tr>
  <tr><td>classes</td><td>Where build artifacts (except the jars) are stored.
  <tr><td>doc</td><td>Where Javadocs are written.</td></tr>
  <tr><td>contract4j5.jar</td><td>The runtime deployment jar. It contains the build products from &#8220;src&#8221;.</td></tr>
  <tr><td>contract4j5-test.jar</td><td>The jar containing the build products from &#8220;test&#8221;. Not part of the normal runtime deployment.</td></tr>
</table>

<p><a name='building'></a></p>

<h2 id="building_contract4j5">Building Contract4J5</h2>

<p>If you want to build Contract4J5:</p>

<pre><code>./build.sh all  # *nix
build.bat all   # windows
</code></pre>

<p>or</p>

<pre><code>ant all
</code></pre>

<p>The jar files <code>contract4j5.jar</code> and <code>contract4j5-test.jar</code> in the current directory will be built and the unit/acceptance tests will be executed for all three supported languages. The tests generate a LOT of output, but they should all pass. Also, there will be some warnings that fall into two categories:</p>

<p>Warnings in some unit tests when test annotations are used without the required <code>@Contract</code> annotation. This is deliberate for those tests.</p>

<p>The <code>javadocs</code> target also results in many warnings for references to aspects from Java files, which <code>javadoc</code> doesn&#8217;t know how to resolve. To be clear, the following missing &#8220;classes&#8221; are actually aspects:</p>

<ul>
<li><code>AbstractConditions</code></li>
<li><code>ConstructorBoundaryConditions</code></li>
<li><code>Invariant*Conditions</code> (several)</li>
<li><code>MethodBoundaryConditions</code></li>
</ul>

<p>For example, you&#8217;ll see lots of warnings about not being able to find members of these aspects.</p>

<p>If the unit tests fail, look for output in <code>contract4j5/TEST-*.txt</code> files. Usually, the problem will be a <code>CLASSPATH</code> issue.</p>

<p>To build the example of wiring the components and properties of Contract4J5 using <strong>Spring&#8217;s Dependency Injection</strong> run any of the following commands after building <code>all</code> as before:</p>

<pre><code>./build.sh all.spring   # *nix
build.bat all.spring    # windows
</code></pre>

<p>or</p>

<pre><code>ant all.spring
</code></pre>

<p>This will run a test target that confirms that Spring can &#8220;wire&#8221; Contract4J correctly.</p>

<p>This build target builds the example in the &#8220;sister&#8221; directory <code>../Contract4J5WithSpring</code>. It contains a separate example demonstrating how to use the Spring Framework&#8217;s Dependency Injection (DI) to configure the properties of Contract4J. This is done separately from the main build so that Spring is not required for those people not using it.</p>

<p>The key files in this directory tree are the following:</p>

<table>
    <tr><td>test/org/contract4j5/configurator/spring/test/ConstructWithSpringTest.java</td><td>Uses Spring&#8217;s &#8220;ApplicationContext&#8221; to construct C4J, then tests that the components and properties are wired as expected.</td></tr>
  <tr><td>test/conf/applicationContext-contract4j5.xml</td><td>The application context configuration file that defines the &#8220;wiring&#8221;.</td></tr>
  <tr><td>test/conf/contract4j.properties</td><td>A properties files used by the config. file.</td></tr>
</table>

<p>An example of running the test suite using Contract4J5 with Load-Time Weaving (LTW) is also included. (The details of using LTW were discussed above.)</p>

<p><strong>Note:</strong> LTW requires AspectJ 1.5.3 or newer, due to a bug in early versions.</p>

<p>To run the tests using load-time weaving, first build all as above, then build the following target:</p>

<pre><code>./build.sh test.ltw   # *nix
build.bat test.ltw    # windows
</code></pre>

<p>or</p>

<pre><code>ant test.ltw
</code></pre>

<p>This target will run the tests, using LTW of the aspects on the fly.</p>

<h4 id="more_details_on_building_contract4j5">More Details on Building Contract4J5</h4>

<p>The following third-party tools are required, along with corresponding <code>HOME</code> environment variable definitions needed by the ant build scripts:</p>

<ul>
<li>JUnit 3.8.1+ (<code>JUNIT_HOME</code>)</li>
<li>Java 5 (<code>JAVA_HOME</code>)</li>
<li>AspectJ 1.5.3+ (<code>ASPECTJ_HOME</code>)</li>
<li>Ant 1.6.5+ (<code>ANT_HOME</code>)</li>
<li>Jexl 1.1+ (included in the lib directory)</li>
<li>JRuby 1.0+ (included in the lib directory)</li>
<li>Groovy 1.0+ (included in the lib directory)</li>
<li>Other support libraries in the lib directory.</li>
<li>Also define <code>Contract4J5_HOME</code> to be the <code>.../contract4j5_XYZ/contract4j5</code> directory where you installed it.</li>
</ul>

<p>For your convenience, you can use the build driver script <code>build.sh</code> or <code>build.bat</code>. Edit the values of the environment variables in the corresponding scripts <code>env.sh</code> or <code>env.bat</code> for your environment.</p>

<p>Only Java, AspectJ, the libraries for one of the scripting engines, and the support jars it requires are needed if you simply use the binary contract4j5.jar in the distribution. (All the scripting engines jars are in the lib directory.) Make sure your build process compiles with AspectJ, weaves your precompiled jars or class files with contract4j5.jar, or uses load-time weaving, as demonstrated in the &#8220;LTW&#8221; tests.</p>

<p>Next, we&#8217;ll look at code examples, then return to a discussion of invoking and configuring Contract4J5.</p>

<p><a name='showme'></a></p>

<h2 id="show_me_the_code">Show Me the Code!</h2>

<p>Here is a large example showing how to define Contract4J5 tests in your code so that Contract4J5 can discover and execute them at runtime. It is the file <code>test/org/Contract4J5/test/BaseTestClass.java</code> (with some superfluous details omitted). See additional examples in the unit/acceptance suite under the &#8220;test&#8221; directory.</p>

<p>The comments in this class should be self explanatory. More specific details on writing contracts are provided below.
<code>BaseTestClass.java</code>:</p>

<pre><code>package org.contract4j5.test;

import org.contract4j5.contract.*;
/**
 * A (contrived) example Java class that demonstrates how
 * to define DbC tests. The "@Contract" annotation is
 * required. Then, we define a class-level invariant,
 * which happens to be for one of the fields. Note that
 * we have to prefix the field name with "$this", one of
 * several special keywords that begin with "$" and are
 * replaced with special values before passing the
 * expression to the script interpreter. In this case,
 * "$this" means "this object" (You can't just use "this"
 * without the "$" for backwards compatibility reasons;
 * this may be relaxed in a future release).
 * Prefixing field names with $this is necessary for the
 * scripting engine to be able to resolve the variable
 * name. While not required in all cases, as a rule it is
 * best to always refer to fields this way for consistent.
 * The one case where you don't need the "$this." is when
 * you define an invariant for a field itself (See the
 * test for "name" below). Note also that in order for
 * Jexl to resolve the field reference, a JavaBeans
 * "getter" method must exist for the field, even if the
 * field is public!
 */
@Contract
@Invar("$this.lazyPi==3.14159") // see comments for "lazyPi"
public class BaseTestClass {
  /**
   * A field that is initialized "lazily", but cannot
   * change after that. This invariance is enforced by the
   * @Invar annotation on the class. The constructor must
   * call {@link #getLazyPi()} BEFORE ANY OTHER PUBLIC
   * FUNCTION, or the invariant test will fail!
   * NOTE: the Jexl parser chokes if the invariant test
   * appends "f" to the constant!
   * NOTE: Jexl can't resolve "lazyPi" unless "getLazyPi()"
   * exists!
   */
  private float lazyPi = -1f;

  /**
   * "getLazyPi()" always simply sets the value to 3.14159,
   * so the class invariant "$this.lazyPi==3.14159" will
   * always pass. However, see {@link #setLazyPi(float)}.
   * @return pi
   */
  public float getLazyPi() {
    if (lazyPi == -1f) {
      lazyPi = 3.14159f;
    }
    return lazyPi;
  }

  /**
   * This function allows unit tests to force a failure!
   */
  public void setLazyPi (float f) {
    lazyPi = f;
  }

  /**
   * A field that should never be null or "". See also
   * comments in {@link #setName(String)}. Note that you
   * can safely use the "bare" field name "name" here.
   * You can also use "$this.name", which you have to use
   * in all other types of tests (i.e., tests other than
   * the invariant test on the field itself). You can also
   * use the keyword "$target", which currently is only
   * used to refer to a corresponding field when used in a
   * test expression. (In the future, "$target" may have
   * other uses in the more general AspectJ-sense of the
   * poincut "target()" expression.)
   * NOTE: You can specify an optional error message that
   * will be reported with any failure message. Also, as
   * stated before, "name" must have a "getName()"
   * accessor or Jexl can't resolve it!
   */
  @Invar(value="name != null &amp;&amp; name.length() &gt; 0",
   message="this.name must never be null!")
  private String name;

  /**
   * @return String name of the object
   */
  public String getName() { return this.name; }

  /**
   * Use a precondition to prevent setting name to null.
   * Note this test is less restrictive than the invariant
   * test on the field itself, a poor design. (Hopefully,
   * the developer will realize the mistake when one test
   * fails while the other passes.) In this case, this
   * "mistake" is useful for the dbc4j unit tests.
   * @param name String naming the object
   */
  @Pre("name != null")
  public void setName (String name) { this.name = name; }

  // A flag; used for other contract tests.
  private boolean flag;

  /**
   * Set the flag.
   */
  public void setFlag () { flag = true; }

  /**
   * Set the flag. This method is used in unit tests to
   * force a contract assertion failures.
   */
  public void setFlag (boolean f) { flag = f; }

  /**
   * Constructor. Note that the precondition on the "name"
   * parameter is redundant, since {@link #setName(String)}
   * is called, but it is still useful for documenting the
   * interface. Note that the @Pre test does not
   * define a test expression. In this case, C4J5 uses a
   * {@link org.contract4j5.testexpression.DefaultTestExpressionMaker}
   * to generate a default test expression. There are
   * separate "makers" for different types of tests and
   * contexts and they are user configurable. For
   * preconditions, the default is to require that all
   * arguments are non-null.
   * Note that tests can call methods, too, but watch for
   * side effects, especially since tests will normally be
   * disabled in production builds. Therefore, never call
   * a method with side effects!
   * @param name a non-null String
   */
  @Pre
  @Post ("$this.isValid() == true")
  public BaseTestClass (String name) {
    /* float ignore = */ getLazyPi();
    setName (name);
    setFlag ();
  }

  /**
   * Constructor. As discussed in {@link
   * #BaseTestClass(String)}, the default test expression
   * for the precondition test will be that all parameters,
   * in this case "name" and "flag", must be non-null. What
   * does that mean for "flag", which is boolean. Not much;
   * this argument will be converted to
   * {@link java.lang.Boolean} internally and it will never
   * be null! Also, in this example, the precondition test
   * is actually redundant, since {@link #setName(String)}
   * is called. However, the test is still useful for
   * documenting the interface.
   * @param name a non-null String
   * @param flag a boolean flag; if false, causes the
   *    postcondition to fail.
   */
  @Pre
  @Post ("$this.isValid() == true") // watch for side effects!
  public BaseTestClass (String name, boolean flag) {
    /* float ignore = */ getLazyPi();
    setName (name);
    setFlag (flag);
  }

  /**
   * Is the object valid?
   */
  public boolean isValid () {
    System.out.println ("ExampleClass.isValid(): flag: "+flag);
    return flag;        // reusing our flag...
  }

  /**
   * Method that requires flag to have been previously set.
   * E.g., {@link #setFlag(boolean)}, {@link #doIt()}, etc.
   * Note the postcondition to confirm that the method
   * succeeded, where "$return" is the keyword that matches
   * the value returned by the method (an int in this case).
   */
  @Pre(value="$this.flag == true",
        message="this.flag true before calling 'doIt()'?")
  @Post("$return == 0")
  public int doIt () {
    if (name != null &amp;&amp; name.equals("bad name")) {
      return 1;
    }
    return 0;
  }

  /**
   * Overloaded method. Useful to confirm that the
   * generated tests correctly discriminate between the
   * methods (note the conflicting @Post annotations on the
   * two versions.)
   */
  @Post("$return != 0")
  public int doIt (int toss) {
    if (name.equals("good name")) {
      return 1;
    }
    return 0;
  }

  /**
   * Method with tests on more than one parameter. The
   * keywords "$args[n]" refer to the parameter arguments,
   * counting from 0.
   */
  @Pre ("$args[0]&gt; 0 &amp;&amp; $args[1].equals(\"foo\")")
  public int doThat (int toss, String fooStr) {
    return toss;
  }

  /**
   * Method with tests on more than one parameter. Tests
   * whether we correctly generate matching aspects on the
   * second and last parameter. Note that a nested string
   * in a test must be escaped.
   */
  @Pre ("toss2 &gt; 0 &amp;&amp; toss4.equals(\"foo\")")
  public int doTheOther (int toss1, int toss2,
                         String toss3, String toss4) {
    return toss1;
  }

  /**
   * Test Contract4J5 with a nested class
   */
  @Contract()
  public static class NestedBaseTestClass {
    private String name;
    @Post
    public String getName() {
      return name;
    }

    @Pre
    public void setName(String name) {
      this.name = name;
    }

    @Invar ("$target &gt; 0")
    private int positive;

    /**
     * Method to force the invariant test to fail,
     * if a negative argument is used.
     */
    public void setPositive (int p) { this.positive = p; }

    public int getPositive () { return this.positive; }

    // The @Post on "name" should really be a @Pre on "nm",
    // as it is more restrictive, but it is useful for
    // example purposes.
    @Post ("$this.name != null &amp;&amp; $this.name.length() &gt; 0 &amp;&amp; nm != null")
    NestedBaseTestClass (String nm) {
      this.name = nm;
      this.positive  = nm != null ? nm.length() : -1;
    }
  }
}
</code></pre>

<p><a name='details'></a></p>

<h2 id="details_of_contract_specifications">Details of Contract Specifications</h2>

<p>Here are the rules for using Contract4J5, which clarify the examples just discussed.</p>

<h3 id="annotate_interfaces_and_classes_with_contract">Annotate Interfaces and Classes with <code>@Contract</code></h3>

<p>Examples:</p>

<pre><code>@Contract
public interface Foo { ... }

@Contract
public class Bar { ... }
</code></pre>

<p>Any interface or class that declares a contract must be annotated with <code>@Contract</code>. Otherwise, the tests will be ignored. C4J5 will issue a warning during compilation, but if you use <code>javac</code> to compile and then weave later with <code>ajc</code>, you may not get any warnings and the tests will be silently ignored.</p>

<p>The annotation must also appear on any derived interfaces or classes if they define new tests.</p>

<h3 id="define_class_invariants">Define Class Invariants</h3>

<p>Examples:</p>

<pre><code>@Contract
@Invar("boolean_test_expression")
public interface Foo { ... }

@Contract
@Invar(value="boolean_test_expression", message="The test failed")
public class Bar { ... }
</code></pre>

<p>The second example shows the optional &#8220;message&#8221; that will be reported on error, in addition to standard messages. The test expression must return boolean, or it will be treated as a contract failure!</p>

<p>For subclasses, it isn&#8217;t necessary to annotate them, too, because class-level annotations are inherited, but you can do so for consistency with method annotations, which aren&#8217;t inherited and must be added to subclass overrides.</p>

<p>Define Field Invariants</p>

<p>Examples:</p>

<pre><code>@Contract
public class Bar {
  @Invar("name != null &amp;&amp; name.length() &gt; 0")
  private String name;
  public  String getName() { return name; }
  ...
}
</code></pre>

<p>Note that field invariants can&#8217;t be defined on interfaces, since they don&#8217;t have mutable fields, but you can simulate the same thing by annotating corresponding accessor methods in the interfaces (see below).</p>

<p>Note that for the field <code>name</code>, we are able to use the &#8220;bare&#8221; field name when defining an invariant test for it. You can also use <code>$this.name</code> or the <code>$target</code> keyword.</p>

<p><strong>WARNING!</strong> As discussed previously, the JEXL interpreter can only resolve the field if a JavaBeans &#8220;getter&#8221; method is defined for it, as shown in the example.</p>

<p>In the future, <code>$target</code> may be used more generally for objects that correspond to AspectJ&#8217;s <code>target()</code> <em>pointcut</em> expression, but currently <code>$target</code> is only used in field invariant tests to refer to the field.</p>

<h3 id="define_method_and_constructor_preconditions_postconditions_and_invariants">Define Method and Constructor Preconditions, Postconditions, and Invariants</h3>

<p><strong>NOTE:</strong> we have to use <code>$this.name</code> in the following interface example, not just <code>name</code> by itself, because we are no longer defining a field invariant test! This is because <strong>Jexl requires a field getter method is required for Jexl to resolve the field reference.</strong> (We recommend the same convention for JRuby and Groovy, too.)</p>

<p>Examples:</p>

<pre><code>@Contract
public interface Foo {
  @Invar("$this.name != null &amp;&amp; $this.name.length() &gt; 0")
  String getName();

  @Invar("$this.name != null &amp;&amp; $this.name.length() &gt; 0")
  void setName (String s);

  @Pre("$args[0] &gt; 0")
  @Post("$this.i = $old($this.i) + $args[0]")
  void incrementI (int amount);
  ...
}

@Contract
public class FooImpl implements Foo {
  @Invar
  public String getName() {...}

  @Invar
  public void setName (String s) {...}

  @Pre @Post
  void incrementI (int amount) {...}

  int getI() { ... }     // getter method required by Jexl!
  ...
}

@Contract
public class Bar {
  private float factor;

  @Pre("fudge &gt; 1.0")
  @Post("$this.factor &gt; 1.0")
  void addFudgeFactor (float fudge) {...}

  @Pre("factor &gt; 0.0")
  @Post("$this.factor &gt; 0.0")
  public Bar (float factor) {
    this.factor = factor;
  }

  float getFactor() { ... }   // getter required by Jexl!
}
</code></pre>

<p>The <code>Foo</code> interface simulates a field invariant test on an implied name field by defining invariant tests on name&#8217;s accessor methods. The precondition test for <code>incrementI</code> requires the input amount to be positive. (You could also write this test <code>amount &gt; 0</code>, but this is actually less robust; it is more likely to trip over parsing idiosyncrasies in Contract4J5 and Jexl.) This test implies an <code>int i</code> field, as does the post condition test which requires that the new value of <code>i</code> be equal to the old value (grabbed by the <code>$old($this.i)</code> expression) plus the amount.</p>

<p><strong>NOTE:</strong> The <code>$this</code> cannot appear in <code>@Pre</code> tests on constructors, as the object doesn&#8217;t exist yet! However, <code>$this</code> is allowed in <code>@Invar</code> tests on classes and constructors, as they won&#8217;t be evaluated until after the constructor finishes executing.</p>

<p>The <code>FooImpl</code> implementing class <em>must</em> repeat the annotations, but it can omit the test expressions. Contract4J5 will determine the expressions from the parent class or interface.</p>

<p>What if you omit the expressions? Currently, there is no compile-time checking possible the tests will not be evaluated on these method implementations. For subclass overrides of parent class methods, the tests will not be evaluated on the overrides, but they will be evaluated on the parent methods if <code>super</code> is called.</p>

<p><strong>NOTE:</strong> A desired future enhancement is to either enforce proper usage of the annotations on implementing classes or subclasses or else evaluate the tests even when they are absent!</p>

<p>The <code>Bar</code> class shows that tests can also be defined for constructors. (Invariant tests are also allowed, but seldom useful, since contract tests mostly focus on object state, which won&#8217;t exist before construction!)</p>

<p>Note the idiom for the <code>@Pre</code> and <code>@Post</code> test expressions for the <code>Bar</code> constructor. The <code>@Pre</code> expression references the <code>factor</code> parameter, while the <code>@Post</code> expression references the factor field, using the <code>$this.</code> prefix. A common mistake is to omit the <code>$this.</code> causing the test to be executed with the parameter value instead.</p>

<p><strong>NOTE:</strong> Jexl appears to choke if floats have a trailing <code>f</code>. As shown in the example, they should be omitted.</p>

<h3 id="special_keywords">Special Keywords</h3>

<p>The previous examples use the special keywords. Contract4J5 substitutes the correct values before invoking the scripting engine to evaluate the expressions. Here is a description of the keywords and their proper use.</p>

<table>
  <tr><td>Keyword</td><td>Usage</td></tr>
  <tr><td>$this</td><td>The &#8220;this&#8221; object under test.</td></tr>
  <tr><td>$target</td><td>A field in an invariant test. There must be a corresponding JavaBeans &#8220;getter&#8221; method or Jexl won&#8217;t be able to resolve the field.</td></tr>
  <tr><td>$return</td><td>The return result of a method; only valid in postconditions.
  <tr><td>$args[n]</td><td>The &#8220;nth&#8221; argument in a parameter list.</td></tr>
  <tr><td>$old(..)</td><td>The &#8220;old&#8221; value (before a method is actually executed) of the contents of the expression, which can be one of the following:
    <table>
      <tr><td>$old($this)</td><td>Not recommended, because only the reference is saved and the object pointed to by &#8220;this&#8221; may change! Use fields or method calls instead.</td></tr>
      <tr><td>$old($target)</td><td>Equal to $old($this.field). Be careful if &#8220;field&#8221; is mutable; the value is not saved, just the reference to the object!</td></tr>
      <tr><td>$old($this.field)</td><td>Recommended usage, if &#8220;field&#8221; is primitive, in which case the value is captured, or it refers to an immutable object. Same for $old($target.otherField).</td></tr>
      <tr><td>$old($this.method(x,y))</td><td>The returned value is saved. Due to parser limitations, method calls may not contain nested method calls.</td></tr>
    </table>
  </td></tr>
</table>

<p>The most important thing to remember about <code>$old(..)</code> is that Contract4J5 only remembers the value, which may be a reference to a mutable object. (We can&#8217;t rely on <code>clone()</code> working.) Try to use it only with primitives or immutable objects like strings.</p>

<h3 id="test_expression_best_practices">Test Expression Best Practices</h3>

<p>This section outlines the right way to write test expressions, reflecting the limitations of Contract4J5 and the different supported scripting engines. For detailed examples, see the code in the test suite (e.g., <code>BSFExpressionInterpreterAdapterExpressionEvalTest.java</code>).</p>

<h4 id="default_test_expressions">Default Test Expressions</h4>

<p>If a contract annotation is used with no test expression, a default expression will be inferred with possible. For cases where no test can be inferred, it is considered an error, but this can be overridden with an API call:</p>

<pre><code>ExpressionInterpreter.setTreatEmptyTestExpressionAsValidTest(boolean);
</code></pre>

<p>If the test is on an element with no superclass equivalent, the following default rules apply:</p>

<table>
  <tr><td>@Pre</td><td>All arguments are expected to be non-null, which means there is no meaningful default test for primitive arguments.</td></tr>
  <tr><td>@Post</td><td>The return value is expected to be non-null, unless the method returns void.</td></tr>
  <tr><td>@Invar</td><td>There is no default expression except for field invariants, where the field is expected to be non-null.</td></tr>
</table>

<p>For elements with superclass equivalents, the following rules apply. First, recall that preconditions and postconditions can only be used on methods and constructors. Also, because Java5 method annotations are never inherited, you <em>must</em> annotate any method with the same annotations found on its parent. However, the test expressions can be empty and if so, the corresponding test defined in the parent element will be used. Note that if you don&#8217;t put the annotations on derived class overrides, if they call the parent methods, the parent methods will be tested, but not anything the override does (including constructors).</p>

<p>API calls exist in the <code>org.contract4j5.aspects..*.aj</code> aspects to specify customized objects for calculating a default expression at runtime. These objects must implement</p>

<pre><code>org.contract4j5.testexpression.DefaultTestExpressionMaker.
</code></pre>

<h4 id="inheritance_rules_for_annotation_test_expressions">Inheritance Rules for Annotation Test Expressions</h4>

<p>This was discussed in depth above, in the section titled How Does Contract4J5 Support Design by Contract?.</p>

<ul>
<li><code>$this</code> refers to the object being tested.</li>
</ul>

<p>You can call any public method on the object in the test expression; the scripting engine will resolve the type. Additionally, if you refer to a bare field that is not public, the scripting engine will convert the expression to the corresponding &#8220;getter&#8221; call. Unfortunately, it appears that the getter method is actually required in order for Jexl to resolve the field; it won&#8217;t just use the bare field directly.</p>

<ul>
<li><code>$target</code> currently is used only to refer to the field in a field invariant test.</li>
</ul>

<p>Future use may include any context associated with the <code>target()</code> <em>pointcut</em> expression. Just as for <code>$this</code>, you can reference any method or field defined for the object.</p>

<ul>
<li><code>$return</code> is the value returned by a method.</li>
</ul>

<p>It is only valid in postcondition tests. As for <code>$this</code>, you can reference any method or field defined for the object.</p>

<ul>
<li><code>$args[]</code> are the arguments passed to a method, indexed starting at zero.</li>
</ul>

<p>You can also use the declared argument name. However, if the name shadows an instance field, the parser may confuse the two; use the appropriate <code>$arg[n]</code> in this case. As for <code>$this</code>, you can reference any method or field defined for the objects in the array.</p>

<ul>
<li>Use of the <code>$old(..)</code> Keyword</li>
</ul>

<p>The &#8220;old&#8221; <code>$old(..)</code> keyword tells Contract4J5 to remember the value of the contained expression before evaluating evaluating the join point, so that value can be compared to the &#8220;new&#8221; value after evaluating the join point. It can only be used in <code>@Invar</code> and <code>@Post</code> condition tests and the saved value is forgotten once the test completes.</p>

<p>The most important thing to remember about <code>$old(..)</code> is that Contract4J5 only remembers the value, which may be a reference to a mutable object. Since <code>clone()</code> is not guaranteed by Java to be publicly available on an object, we can&#8217;t clone it and it was deemed too &#8220;obscure&#8221; to only permit, for example <code>$old($this)</code> on objects where clone is publicly available. Hence, you should try to use the <code>$old</code> keyword only with primitives or references to immutable like strings.</p>

<p>Here are the allowed expressions.</p>

<table>
  <tr><td>$old($this)</td><td>Not recommended, since only the reference is saved.</td></tr>
  <tr><td>$old($target)</td><td>Equal to $old($this.field). Be careful if &#8220;field&#8221; is a reference to a mutable object!</td></tr>
  <tr><td>$old($this.field)</td><td>Recommended usage, if &#8220;field&#8221; is primitive. A synonym for $old($target) when used in a field invariant test.</td></tr>
  <tr><td>$old($this.method(x,y))</td><td>Method call where the returned value is saved. Due to current parser limitations, nested method calls are not supported. So, the following is okay:
    $old($this.getFoo().doIt(1))
but this is not
    $old($this.getFoo().doIt(getIntI()))</td></tr>
</table>

<ul>
<li>References to Instance Fields.</li>
</ul>

<p>For fields, Jexl, Groovy, and JRuby will automatically convert a &#8220;bare&#8221; field reference to its accessor, even if the field is private. Hence, an expression like</p>

<pre><code>$this.foo.bar.baz.doIt(1)
</code></pre>

<p>is allowed and will be translated to</p>

<pre><code>$this.getFoo().getBar().getBaz().doIt(1)
</code></pre>

<p><strong>Note:</strong> In fact, for Jexl the corresponding getter methods are <em>required</em> or Jexl will not be able to resolve the field references.</p>

<p>Normally, you should prepend <code>$this.</code> before a &#8220;bare&#8221; field reference as the parser does not always correctly resolve the reference to an instance field. The one case where $this is unnecessary is inside a field invariant test. Using the field&#8217;s &#8220;bare&#8221; name will work. As an alternative in field invariant tests, $target can be used to refer to the field.</p>

<p>Note the example previously where a field invariant test was written with the bare field called <code>name</code>, but when a set of &#8220;conceptually similar&#8221; <code>@Pre</code> and <code>@Post</code> tests were written in an interface on <code>setName()</code> and <code>getName()</code> methods, it was necessary to use <code>$this.name</code>. Contract4J5 may not resolve the field correctly in those cases.</p>

<ul>
<li>Tests Defined on Interfaces</li>
</ul>

<p>You can define tests on interfaces and their methods. In fact, you are urged to do so. Unfortunately, for reasons discussed previously, you must include the same annotations, although not their test expressions, on the declarations of the method implementations in implementing classes. Contract4J5 will find the test expressions in the interfaces.</p>

<p>Unfortunately, you can&#8217;t define contract tests for constructors, since they don&#8217;t exist in an interface. You may be able to work around this using class invariants and tests on instance methods. Note that you can also implicitly define field invariant tests, either on declared accessor methods or as invariants on the class itself. You can refer to the (implied) bare field in the test, as long as you declare an appropriate accessor method for it.</p>

<p><a name='limits'></a></p>

<h3 id="known_limitations">Known Limitations</h3>

<p>(New for V0.8.0) See also Miscellaneous Notes and Debugging Tips and Inheritance Behavior of Contracts.</p>

<p>Referencing other objects, classes, and static methods/fields in test expressions. The scripting engines all run in a different &#8220;context&#8221; from your running classes. This means that simply referring to an object in an expression doesn&#8217;t guarantee that the script will see it, even if the same reference would compile if it were in regular Java code appearing in the class under test. Hence, there are limitations and they vary from one scripting engine to the other. Contract4J offers some help and where it won&#8217;t be able to resolve a reference, there are a few workarounds. Here is a summary of the behavior for Groovy:</p>

<ul>
<li>You can reference another instance field or method without the prefix $this..</li>
<li>You can reference a static field or method in the class under test, but you must prefix it with $this.. This appears to be a limitation of Groovy.</li>
<li>You can reference another class in the same package as the class under test, without qualifying it with its package.</li>
<li><p>Except for Groovy, you can reference another class in a different package with its fully qualified name. However, nested classes don&#8217;t work, because you have to use &#8216;$&#8217; instead of &#8216;.&#8217;, as Contract4J will attempt to load the class in the scripting context using <code>Class.forName(...)</code>, which requires the &#8216;$&#8217; delimiter (e.g., <code>com.foo.bar.MyClass$MyNestedClass</code>). However, when the expression is evaluated later, some scripting engines will choke on the &#8216;$&#8217; (catch-22&#8230;). <strong>Workaround:</strong> use a validation method in your class under test to invoke the static method or use one of the other workarounds described here. Groovy interprets the fully-qualified names as strings of property queries.
For Groovy and JRuby (not Jexl), you can &#8220;preregister&#8221; classes or objects in your static initializer block or constructor. Using the static initializer block will usually be the easiest approach, especially if you need the classes or objects in constructor precondition tests. You can register the classes or objects in the constructor if they are only needed by subsequent calls to instance methods. JRuby can only reference &#8220;global&#8221; variables. Here is an example for Groovy:</p>

<p>// In Validator.java
package com.foo.bar1;
public class Validator {
  public static boolean called = false;
  public static boolean valid(String s) { return s != null &amp;&amp; s.length() > 0; }
  public        boolean valid2(String s) { return valid(s); }
}</p>

<p>// In MyClass1.java
package com.foo.bar2; // different package
class MyClass1 {
  static {
    Contract4J.getInstance().registerGlobalContextObject(&#8220;Validator&#8221;, com.foo.bar1.Validator.class);
  }</p>

<p>@Pre(&#8220;Validator.valid(name)&#8221;)
  public MyClass1(String name) {&#8230;}</p>

<p>}</p>

<p>// In MyClass2.java
package com.foo.bar2; // different package
import com.foo.bar1.Validator;
class MyClass2 {
  static {
    Contract4J.getInstance().registerGlobalContextObject(&#8220;validator&#8221;, new Validator());
  }</p>

<p>@Pre(&#8220;validator.valid2(name)&#8221;)
  public MyClass2(String name) {&#8230;}
}</p></li>
<li><p>For JRuby, the previous test expressions must put a &#8216;$&#8217; before the Validator and the validator references. See <code>test.org.contract4j5.aspects.constructor.test.ConstructorBoundary*ExpressionsWithObjectReferencesTest</code> classes for more examples for the different languages.</p></li>
<li><p>JRuby support is deprecated and will be removed in a future release.</p></li>
<li><p>Jexl support may be deprecated, but keeping it provides a faster, if limited, scripting alternative.</p></li>
<li><p>It appears that Jexl and JRuby <strong>can&#8217;t handle Java 5 generic objects</strong>. The Contract4J unit tests with generics are effectively &#8220;no-ops&#8221; except when Groovy is the interpreter.</p></li>
</ul>

<p><a name='tips'></a></p>

<h3 id="miscellaneous_notes_and_debugging_tips">Miscellaneous Notes and Debugging Tips</h3>

<ul>
<li><p>All test expressions <em>must</em> evaluate to a <strong>boolean</strong> value.</p></li>
<li><p>Test expressions that fail to be evaluated by the scripting engine are treated as test failures, on the grounds that the expression is buggy in this case! Note that if an annotation is empty (i.e., it doesn&#8217;t define a test expression), then it is considered an error if a default expression can&#8217;t be inferred and no corresponding test exists on a parent class. However, there is an API call to allow empty tests, <code>ExpressionInterpreter.setTreatEmptyTestExpressionAsValidTest(boolean)</code> in the <code>org.contract4j5.interpreter</code> package).</p></li>
<li><p>When a test fails due to a buggy or empty test expression, as just described, a subclass of <code>ContractError</code> is thrown, <code>TestSpecificationError</code> (new as of v0.6.0).</p></li>
<li><p>Remember that when using Jexl, if any test accesses an instance field, the field must have a corresponding JavaBeans getter method. Otherwise, Jexl will fail to resolve the field and the test will fail.</p></li>
<li><p>Fields in expressions must be prefixed with ., except in field <code>@Invar</code> test expressions. Jexl is more forgiving if you omit the prefix.</p></li>
<li><p>Avoid expressions with side effects. Since tests will usually be turned off in production, test expressions with side effects, e.g., assignments, will not be evaluated, thereby changing the logical behavior of the application.
When using load-time weaving (LTW), if it appears that the tests aren&#8217;t being evaluated, make sure you have an <code>aop.xml</code> file in your class path. Adapt the example file used for the LTW tests.</p></li>
<li><p>Because runtime expression evaluation is very slow compared to compiled code, consider embedding non-trivial tests in &#8220;validation&#8221; methods and calling them from the test expression. (Prepend instance tests with <code>$this.</code>)</p></li>
<li><p>Keywords follow the same white space rules as for Java. Don&#8217;t allow whitespace between <code>$</code> and the keyword names.</p></li>
<li><p>Jexl can&#8217;t parse literal floats and doubles with the &#8216;f&#8217; and &#8216;d&#8217; appended, respectively. Leave them off in both cases.</p></li>
<li><p>Most other Java expressions, like comparisons and arithmetic expressions can be used. See the Jexl website for more information on allowed expressions.</p></li>
<li><p>Before passing the expressions to Jexl, substitutions are made. Normally, you shouldn&#8217;t case, but when debugging, you may see strings with these substitutions. All the &#8221; keywords are changed. For example,</p></li>
</ul>

<table>
  <tr><td>$this</td><td>becomes</td><td>c4jThis</td></tr>
  <tr><td>$target</td><td>becomes</td><td>c4jTarget</td></tr>
  <tr><td>$old($this)</td><td>becomes</td><td>c4jOldThis</td></tr>
  <tr><td>$old($target)</td><td>becomes</td><td>c4jOldTarget</td></tr>
  <tr><td colspan="3" style="text-align='left'">etc.</td></tr>
</table>

<ul>
<li><p>Turn on <code>DEBUG</code> logging to see what expressions are being evaluated and some of the substitutions that are made.</p></li>
<li><p>Some common test expression errors have &#8220;canned&#8221; strings defined for them in <code>org.contract4j5.interpreter.ExpressionInterpreter.java</code>. The heavy lifting of expression evaluation is done in <code>org.contract4j5.interpreter.ExpressionInterpreterHelper.java</code> and its subclasses for Groovy, Jexl, and JRuby, respectively; <code>org.contract4j5.interpreter.groovy.GroovyExpressionInterpreter</code> (default), <code>org.contract4j5.interpreter.bsf.groovy.GroovyBSFExpressionInterpreter</code> (through BSF), <code>org.contract4j5.interpreter.jexl.JexlExpressionInterpreter</code>, <code>org.contract4j5.interpreter.bsf.jexl.JexlBSFExpressionInterpreter</code> (through BSF), and <code>org.contract4j5.interpreter.bsf.jruby.JRubyBSFExpressionInterpreter</code>.</p></li>
<li><p>As of V0.8, the Bean Scripting Framework (BSF) is an option for Jexl and Groovy and remains the only option for JRuby. There is no noticeable performance penalty when BSF is used, compared to the rest of the Contract4J overhead, although there appears to be a noticeable overhead over invoking an &#8220;empty&#8221; script through BSF vs. invoking it directly with the scripting engine, when using test applications separate from Contract4J. By default, Jexl and Groovy don&#8217;t use BSF.</p></li>
<li><p>If you want to use a different language, the easiest approach is to create a subclass of <code>BSFExpressionInterpreterAdapter</code>. See e.g., <code>GroovyBSFExpressionInterpreter</code>.</p></li>
<li><p>To select a different language option, see the <strong>Configuration</strong> section (next).</p></li>
</ul>

<p><a name='config'></a></p>

<h3 id="configuration_of_contract4j5">Configuration of Contract4J5</h3>

<p>To configure the behavior of Contract4J5, when the default behavior doesn&#8217;t meet your needs, you have several options.</p>

<table>
  <tr><td>Spring dependency injection</td><td>The preferred method for nontrivial configuration changes. See the Spring V1.2 example in Contract4J5WithSpring. (There should be no issues using Spring V2.X)</td></tr>
  <tr><td>Property file configuration</td><td>The property file approach is fine for basic needs. See the unit test `org.contract4j5.configurator.test.PropertiesConfiguratorTest.java` for examples.</td></tr>
  <tr><td>API calls</td><td>There is an extensive internal API for configuration, which we discuss next.</td></tr>
</table>

<h4 id="configuration_through_the_api">Configuration Through the API</h4>

<p>Here are some API examples.</p>

<ul>
<li>Select a Different Scripting Language</li>
</ul>

<p>Set a property when invoking the JVM:</p>

<pre><code>java -Dinterpreter=lang ...
</code></pre>

<p>where <code>lang</code> is one of:</p>

<table>
  <tr></td>`lang`</td><td>Which language?</td></tr>
  <tr></td>groovy</td><td>Groovy without BSF (default)</td></tr>
  <tr></td>groovybsf</td><td>Groovy with BSF</td></tr>
  <tr></td>jexl</td><td>Jexl without BSF</td></tr>
  <tr></td>jexlbsf</td><td>Jexl with BSF</td></tr>
  <tr></td>jruby</td><td>JRuby with BSF</td></tr>
  <tr></td>other</td><td>Any other language for which you have implemented an integration with Contract4J, as discussed elsewhere in these notes.</td></tr>
</table>

<p>You can also specify the language using Spring configuration, as discussed above.</p>

<ul>
<li><p>Enable or Disable Test Types</p>

<p>import org.contract4j5.controller.Contract4J;
&#8230;
Contract4J.getInstance().setEnabled(Contract4J.TestType.Pre,   true); // or false
Contract4J.getInstance().setEnabled(Contract4J.TestType.Post,  true);
Contract4J.getInstance().setEnabled(Contract4J.TestType.Invar, true);</p></li>
</ul>

<p>To completely disable contract checking, e.g., in production builds, build the application without <code>contract4j5.jar</code> in your <code>ASPECTPATH</code>.</p>

<ul>
<li>Specifying Major Components</li>
</ul>

<p>There are several key components in Contract4J5 that are configurable:</p>

<table>
  <tr></td>ExpressionInterpreter</td><td>Wraps Groovy, Jexl, and JRuby (It could also wrap other languages)</td></tr>
  <tr></td>ContractEnforcer</td><td>Handles test invocation and failure handling.</td></tr>
  <tr></td>Reporter</td><td>Simple output/logging wrapper.</td></tr>
</table>

<p>Note that a runtime warning are issued if the <code>ExpressionInterpreter</code> or <code>ContractEnforcer</code> are not defined, as tests can&#8217;t be run otherwise! The Reporter objects will default to <code>stdout</code> and <code>stderr</code> if undefined.</p>

<p>Here are more details about these component interfaces and implementing classes:</p>

<table>
  <tr></td>org.contract4j5.enforcer.ContractEnforcer</td><td>The &#8220;enforcer&#8221; interface.</td></tr>
  <tr></td>org.contract4j5.enforcer.ContractEnforcerImpl</td><td>The one implementation used here. It runs the tests and on failure, logs a detailed error message and terminates program execution.</td></tr>
  <tr></td>org.contract4j5.interpreter.ExpressionInterpreter</td><td>The expression interpreter interface.</td></tr>
  <tr></td>org.contract4j5.interpreter.ExpressionInterpreterHelper</td><td>An abstract helper class that provides a partial implementation. Subclass this class to support the actual interpreters.</td></tr>
  <tr></td>org.contract4j5.interpreter.bsf.BSFExpressionInterpreterAdapter</td><td>Adapts the Bean Scripting Framework (BSF) for use by Contract4J5. (As of V0.8, Groovy and Jexl no longer use it, by default, although the BSF integration is still in the code base.)</td></tr>
  <tr></td>org.contract4j5.interpreter.groovy.GroovyExpressionInterpreter</td><td>The Groovy subclass of the &#8220;helper&#8221; class.</td></tr>
  <tr></td>org.contract4j5.interpreter.bsf.groovy.GroovyBSFExpressionInterpreter</td><td>The Groovy BSF support class (optional).</td></tr>
  <tr></td>org.contract4j5.interpreter.jexl.JexlExpressionInterpreter</td><td>The Jexl subclass of the &#8220;helper&#8221; class.</td></tr>
  <tr></td>org.contract4j5.interpreter.bsf.jexl.JexlBSFExpressionInterpreter</td><td>The Jexl BSF support class (optional).</td></tr>
  <tr></td>org.contract4j5.interpreter.bsf.jruby.JRubyBSFExpressionInterpreter</td><td>(Deprecated) The JRuby BSF support class.</td></tr>
  <tr></td>org.contract4j5.reporter.Reporter</td><td>The &#8220;reporter&#8221; interface that is a thin veneer for a logging abstraction.</td></tr>
  <tr></td>org.contract4j5.reporter.Severity</td><td>Defines logging levels of severity, like INFO, WARN, ERROR, etc.</td></tr>
  <tr></td>org.contract4j5.reporter.WriterReporter</td><td>&#8220;Logs&#8221; to stdout and stderr by default, but also supports file output. It would be very easy to implement a &#8220;log4j reporter&#8221;, for example.</td></tr>
</table>

<h3 id="notes">Notes:</h3>

<p>For most properties currently defined, if a value is empty, it is ignored! In some cases, warnings are issued.</p>

<p><a name='todo'></a></p>

<h3 id="todo_items">TODO Items</h3>

<p>Here is a brief list of the most important &#8220;TODO&#8221; items that we want to complete before the V1.0 release, roughly in order of importance.</p>

<ul>
<li>Improve performance. Unfortunately, the current runtime overhead of C4J is prohibitive, making it difficult to use continuously on large projects.</li>
<li>Drop support for JRuby and possibly Jexl. It&#8217;s getting increasingly difficult to support three languages, as each has its own tool chains, idiosyncrasies, etc. Would everyone be happy with Groovy as the officially-supported language with possibly limited support for Jexl, when optimal performance is most important?</li>
<li>Find a way to automatically enforce proper usage of method and constructor annotations in subclasses when they aren&#8217;t annotated.</li>
<li>Implement correct contravariant behavior of inherited precondition tests and covariant behavior of inherited postcondition tests.</li>
<li>Support a method for defining constructor conditions in interfaces, where constructors don&#8217;t exist.</li>
<li>Support end-user extensions with custom annotations and &#8220;behavior&#8221; handlers.</li>
<li>Support tests on static methods.</li>
<li>Mock out logging output so test runs don&#8217;t generate so much output. </li>
</ul>

<p>Here is a list of other possible enhancements.</p>

<ul>
<li>Extend @Pre and @Post to support class-level annotations that will apply to all protected and public instance and static methods. For example, @Contract @Pre @Post public class Foo {&#8230;} would require all such methods to take non-null arguments and return non-null values, consistent with the way @Pre and @Post work by default today on methods. Add optional configuration properties to each annotation to set the protection level for which the annotation applies (e.g., also do private methods.) and whether or not to exclude static methods.</li>
<li>Interface to scripting languages using the native scripting support in Java 6.</li>
<li>For the keywords:
<ul>
<li>Provide a &#8220;$field&#8221; alias for &#8220;$target&#8221;, since the latter isn&#8217;t used for anything other than fields. Keep &#8220;$target&#8221; for backwards compatibility.</li>
</ul></li>
</ul>

<p><a name='history'></a></p>

<h2 id="history">History</h2>

<h3 id="contract4j5_annotation_form">Contract4J5 (Annotation Form)</h3>

<ul>
<li>v0.9.0.0 November 13, 2009</li>
<li>v0.8.0.0 September 13, 2007</li>
<li>v0.7.1.0 January 21, 2007</li>
<li>v0.7.0.0 December 31, 2006</li>
<li>v0.6.0.0 September 21, 2006</li>
<li>v0.5.0.0 February 7, 2006</li>
<li>v0.1.1.0 October 4, 2005</li>
<li>v0.1.0.2 April 24, 2005</li>
<li>v0.1.0.1 February 6, 2005</li>
<li><p>v0.1.0.0 January 31, 2005</p>

<p>Contract4JBeans (Experimental)</p></li>
<li><p>v0.3.0.0 February 20, 2006</p></li>
<li>v0.2.0.0 October 5, 2005</li>
<li>v0.2.0.0M1   August 15, 2005</li>
</ul>

<p><a name='notes'></a></p>

<h2 id="notes_for_each_release">Notes for Each Release</h2>

<p>v0.9.0 November 13, 2009</p>

<p>Performance improvements and refactorings. </p>

<p>I&#8217;ve been away from Contract4J for two years! This release focuses on performance-related enhancements, the most important area of work required before a 1.0 release.</p>

<p>Added a new field for the contract annotations. Using run=NEVER disables the test, for @Pre, @Post, @Invariant annotations, and disables all the tests in the type for @Contract annotations. The other allowed value is run=ALWAYS, which is the default. I used ALWAYS and NEVER, rather than ON and OFF, for example, to allow possible additional values. ONCE, as in run each test only once, is a logical and potentially-useful choice, although the implementation is somewhat challenging.</p>

<p>v0.8.0 September 13, 2007</p>

<p>Deprecated JRuby and &#8220;partially&#8221; deprecated Jexl, made BSF optional, implemented bug fixes in test expression handling, etc.</p>

<p>Deprecated JRuby and raised the possibility of deprecating Jexl. Groovy is now the standard scripting language for Contract4J. (Load-time weaving with JRuby is no longer supported at all; the &#8220;test.ltw.jruby&#8221; target is still in the build, but isn&#8217;t executed as part of the standard &#8220;test.ltw&#8221; target anymore&#8230;)
Made BSF optional as the mediator between Contract4J and Jexl and Groovy. This was done originally because of a perceived performance penalty imposed by BSF and because different versions of BSF are needed for JRuby vs. Jexl and Groovy. The performance penalty turned out to be minimal, so the BSF integration was retained as an option. Since JRuby itself is also deprecated, it still uses BSF exclusively. (Feedback on the use of BSF is welcome.)
Fixed bugs that prevented tests from being applied for generic classes. (But it appears that only Groovy supports testing generics!)
Added support for referencing other objects and classes in test expressions, not just the class under test itself, which was not well supported previously. (See the Known Limitations for details.)
Increased the number of tests by ~15%.
Thanks to Chuck H. for additional feedback and Sebastiaan v. E. and Daniel S. for finding bugs!</p>

<p>v0.7.1 January 21, 2007</p>

<p>Minor Documentation Bug Fixes</p>

<p>Fixed examples that had obsolete package and class references. Added more details about how to use load-time weaving (LTW).
Thanks to Chuck H. for valuable feedback.</p>

<p>v0.7.0 December 31, 2006</p>

<p>Support for Groovy, JRuby, and Other Refinements</p>

<p>This release adds built-in support for using Groovy or JRuby as an alternative to Jexl as the scripting engine. In fact, because of Jexl limitations, Groovy is now the default scripting language at startup. (This is easily configurable, as discussed previously.) In our experiments, most Jexl-compatible expressions work just fine with Groovy.</p>

<p>Using JRuby requires porting some existing expressions. However, to facilitate using common Java idioms in Ruby without translation, Contract4J5 makes a few simple substitutions automatically in test expressions:</p>

<p>null is changed to nil
equals(&#8230;) is changed to eql?(&#8230;)
compareTo(&#8230;) is changed to &lt;=>(&#8230;)
New scripting engines can be integrated through the Jakarta Bean Scripting Framework (BSF). However, the integration with Groovy and Jexl no longer use BSF, be default, although the option is still there. Are the library dependencies:</p>

<p>Scripting Engine    Required Libraries
Several bsf-2.4.X.jar, commons-logging.jar-1.0.X.jar
Jexl    All plus commons-jexl-1.1.jar
Groovy  All plus groovy-1.0.jar (RC 1 or newer), asm-2.2.jar, and antlr-2.7.5.jar
JRuby   All plus jruby-1.0.jar or later
All these jars, which the exception of JRuby and its dependent jars, are included with the distribution (in the lib directory). Remove the ones you don&#8217;t need from your deployment. If you use JRuby, you&#8217;ll need to define the JRUBY_HOME environment variable appropriately in the env.sh</p>

<p>The following is the partial list of differences between scripts written with Jexl, Groovy, or JRuby. Some of these differences may reflect idiosyncrasies of how they are used in the implementation, rather than real language differences. Also, in general, Groovy and JRuby are more full-featured environments, so they provide facilities that Jexl doesn&#8217;t, such as closures.</p>

<p>Groovy and JRuby scripts can reference fields in a class without requiring the class to provide an accessor method. Jexl always requires accessors (as emphasized previously).
Groovy can reference accessor methods that aren&#8217;t public, while JRuby and Jexl only accesses public accessors.
JRuby will translate some idiomatic-Ruby expressions to corresponding idiomatic-Java expressions. For example, field to getField(), field= to setField(), and method<em>with</em>words to methodWithWords.
Jexl appears to swallow ArrayIndexOutOfBoundsExceptions while Groovy and JRuby do not.
When writing tests while using Groovy or JRuby, fields must be prefixed with $this., except in field @Invar expressions. (Jexl parsing is more forgiving.) If you get a test-failure message that reports a groovy.lang.MissingPropertyException, for Groovy, or &#8220;undefined local variable or method&#8221;, for JRuby, make sure the test expression doesn&#8217;t have a bare field reference!
Jexl can access package protected and protected fields while Groovy and JRuby cannot.
The Groovy interpreter provides more descriptive error messages when a test fails because the expression is bad, e.g., because it references a non-existent field on an object. As a result, while such tests fail whether using Jexl or Groovy, debugging the issue will often be easier with Groovy. JRuby messages are descriptive as well.
Search the class BSFExpressionInterpreterAdapterExpressionEvalTest for the variables isJexl, isJRuby, isGroovy and isBSF to see examples of tests where Jexl, JRuby, Groovy, and whether or not they are going through BSF, behave differently.</p>

<p>Configuration of Failure Handling</p>

<p>It is now easier to configure the behavior for what happens when a contract failure occurs. The interface ContractEnforcer exposes more configuration options, which are exercised by the unit tests, example property files, and the Spring example. The default enforcer is now called DefaultContractEnforcer (was ContractEnforcerImpl), which implements a new abstract class ContractEnforcerHelper. DefaultContractEnforcer implements an abstract method finishFailureHandling() that throws the usual ContractError. Create a new subclass of the helper that does your desired handling. One option is to override the makeContractError() method and have it return your own subclass of ContractError, then catch that error in your code.</p>

<p>This change is in anticipation of a planned generalization of Contract4J to support user-defined annotations and associated behaviors, so that the infrastructure can be used for a variety of applications beyond Design by Contract.</p>

<p>More Robust Handling of String Literals With Test Expressions</p>

<p>Previously, if a variable (field, parameter, etc.) name appeared within a string literal, it was sometimes replaced with an internal representation of the variable. This would break test expressions like param1.equals(&#8220;param1&#8221;), where param1 is a method parameter. The result was a test failure even if param1 really had the value &#8220;param1&#8221;! Now, string literals are unmodified.</p>

<p>Better Diagnostic Information</p>

<p>More descriptive information from nested exceptions is provided in the output. Furthermore, by default, BSF logs exceptions that are thrown. This should make debugging problems easier. (To disable BSF logging, configure Apache&#8217;s Commons Logging as desired.)</p>

<p>Java JDK 1.6 Compatability</p>

<p>This release of Contract4J was tested using the final release of Java SE 1.6.0. While it doesn&#8217;t use any Java 6 features (such as the JSR-223 support for scripting), it does offer performance improvements. The JUnit suite runs approximately 18% faster than under Java 5 and when the load-time weaving (LTW) test target is run, the improvement exceeds 30%!</p>

<p>Support for JSR-223 (Java 6) is under consideration.</p>

<p>JDepend and FindBugs Support</p>

<p>The distribution now includes convenience scripts for evaluating the structure of Contract4J5 using jdepend and findbugs. These tools were used to restructure Contract4J5 to eliminate a number of circular dependencies and other problems. See the jdepend.sh and findbugs.txt files in the contract4j5 directory for more information, such as how to install the required tools. A graph of jdepend output for Contract4J5 is jdepend_report.png.</p>

<p>General Clean-ups</p>

<p>More clean-ups of package dependencies and generics-related warnings were also done.</p>

<p>v0.6.0 September 21, 2006</p>

<p>Restructuring</p>

<p>This release is a major restructuring to reduce the component complexity, to reduce over-reliance on singletons, and to improve the &#8220;wiring&#8221; options. Defining contracts is unchanged (except for some bug fixes), so most users won&#8217;t be affected. However, the configuration API has changed (this is still pre-1.0 software!), as discussed below in these notes and elsewhere in this README.</p>

<p>Spring Dependency Injection Example</p>

<p>An example has been added that uses the Spring Framework (v1.2.5) to configure Contract4J5. See the separate folder called Contract4J5WithSpring. Spring v2.0 configuration should be backwards compatable, if not easier with the new 2.0 features. However, using Contract4J5 with Spring v2.0 has not been tested.</p>

<p>Improved Configuration Options</p>

<p>The restructuring greatly improved the options for using properties files to &#8220;wire&#8221; Contract4J5, as an alternative to using Spring. See the tests in org.contract4j5.configurator.test for examples.</p>

<p>New Error Thrown to Indicate Bad Test Specifications</p>

<p>If a test fails because the expression is empty or can&#8217;t be evaluated by Jexl, a new subclass of ContractError, TestSpecificationError, is now thrown. This makes it easier to determine when a test failed because the test itself was bad, as opposed to the class under test failing to meet the contract. Using a subclass of ContractError means that any existing catch (ContractError ce) code will continue to catch both kinds of failures. However, users who want to distinguish between the two types of errors should use this idiom:</p>

<p>try {
    &#8230;
  } catch (TestSpecificationError tse) {
    &#8230;
  } catch (ContractError ce) {
    &#8230;
  }
Binary Weaving and Load-Time Weaving Options</p>

<p>The build process now uses binary (jar) weaving when the &#8220;tests&#8221; are built, providing an example of using Contract4J5 with this approach. An example of using load-time weaving (LTW) is also provided, using a separate test target, as discussed above.</p>

<p>&#8220;Binary&#8221; weaving is weaving done after compiling all code, using a post-compilation weaving step to incorporate the Contract4J5 aspects. It is useful for organizations that prefer to use javac for all java files.</p>

<p>Load-time weaving is done as the application loads class files, using a special &#8220;java agent&#8221; for this purpose. Using load-time weaving is the least disruptive way to adopt Contract4J5 into &#8220;pure Java&#8221; environments. For this reason, we recommend starting with load-time weaving, since this approach requires minimal changes to the existing build environment. However, since LTW is slower than binary weaving, especially when running numerous tests, larger projects may prefer to switch to binary weaving at some point.</p>

<p>Previous releases of Contract4J5 used just compile-time weaving, where ajc was used to compile all sources and weave the class files. This approach is still used to build Contract4J5&#8217;s own source code (as opposed to its unit tests) and to create the contract4j5.jar file.</p>

<p>Miscellaneous</p>

<p>Replaced the entity definitions in the build-related XML files with the ant task. Apparently, NetBeans doesn&#8217;t like the entity definitions. (Thanks to Matthew Harrison for bringing this to my attention and for providing refactored build files.)
Added more tests that explicitly demonstrate that contract expressions that access instance properties only work if the properties have JavaBeans getter methods. This is an unfortunate Jexl limitation.
Fixed numerous small bugs, including a few warnings related to generics. (Thanks to Falk Bruegmann for a generics warning fix.)
Some of the API Changes</p>

<p>Handling of &#8220;Reporters&#8221;
Removed the API calls to set separate Reporter objects (for logging) in each major component. This greatly reduced some boilerplate code with a small reduction in flexibility that had dubious value. Now, a global Reporter object is used, the one set in the singleton instance of the Contract4J5 class.</p>

<p>All properties on aspects are no longer static methods. Instead, use the aspectOf() method to get the instance. So for example,</p>

<p>ConstructorBoundaryConditions.getDefaultPreTestExpressionMaker();
is now
  ConstructorBoundaryConditions.aspectOf().getDefaultPreTestExpressionMaker();
Contract4J5 is no longer an aspect, but a class. The aspect code was moved to a separate aspect called AbstractConditions, leaving only pure-Java code. The conversion to Java made it easier to instantiate these objects as needed, especially for testing, and also to exploit the more complete support for Java in Eclipse (e.g., for refactorings). Note that this change means that any properties that were previously set on Contract4J5 are not now accessed using aspectOf, as just described for the aspects. Instead, Contract4J5 properties are accessed, e.g., as follows:</p>

<p>Contract4J5.getInstance().setEnabled(Contract4J5.TestType.Pre, true);
Bugs</p>

<p>It seems that the ant build expects Spring to be present in order to define some properties or classpaths. This causes the build to fail if Spring is not present! This is a bug in the ant scripts. Workaround: Install Spring somewhere and point the &#8220;env.sh&#8221; or &#8220;env.bat&#8221; script to it. It won&#8217;t actually be used unless you explicitly invoke one of the demonstration *.spring ant targets.
v0.5.0 February 7, 2006</p>

<p>Major Rewrite</p>

<p>Eliminated the precompilation step, replacing it with runtime teste expression evaluation using the Jakarta Commons Jexl expression parser.</p>

<p>The package structure has been changed from com.aspectprogramming.* to org.contract4j5.*.</p>

<p>Deprecated several features; they aren&#8217;t supported in this milestone release.</p>

<p>Ad Hoc Configuration API</p>

<p>The ad hoc configuration API and full support for configuration through property files. Subsequent releases will &#8220;re-add&#8221; limited support for property file configuration, for convenience, but the preferred way to configure Contract4J5 is through a standard dependency injection (DI) solution like the Spring Framework. This release does all you to globally enable or disable all tests or just all precondition, postcondition, or invariant tests. To use, define one or more of the following System properties (true or false):
org.contract4j5.Contract    Enable/disable all tests
org.contract4j5.Pre Enable/disable all precondition tests
org.contract4j5.Post    Enable/disable all postcondition tests
org.contract4j5.Invar   Enable/disable all invariant tests
The &#8220;alwaysActive&#8221; property of the V0.1 annotations</p>

<p>This annotation property, when used, kept a test &#8220;active&#8221; even if all other annotations of the same kind were disabled globally. The complexity of implementing this feature in the new architecture outweighed the benefits. Use an alternative implementation like embedded assert statements.
The &#8220;messagePrefix&#8221; and &#8220;messageSuffix&#8221; annotation properties</p>

<p>Similarly, these properties of the @Contract annotation are no longer supported. However, you can still define individual messages in the test annotations themselves.
Annotations on method parameters</p>

<p>AspectJ5 has the limitation that it doesn&#8217;t support annotations on method parameters. They were supported in Contract4J5 v0.1, because it didn&#8217;t rely on AspectJ&#8217;s support. The workaround is to put all parameter tests in a method precondition test.
v0.1.1.0 October 4, 2005</p>

<p>Support for Ant builds and numerous small bug fixes in the V1 branch.
v0.1.0.2 April 24, 2005</p>

<p>Fixed a bug that prevented use of precondition annotations on individual method parameters if the method contains more than one parameter. (The generated aspectj code pointcut uses the &#8220;args()&#8221; specifier. For it to match correctly, args() must contain the correct parameter list for the method, with the parameter name used for the parameter of interest and the parameter types used for the other parameters.)
Numerous minor enhancements.</p>

<p>v1.0.1 February 6, 2005</p>

<p>Minor bug fixes.</p>

<p><a name='furtherinfo'></a></p>

<h2 id="for_further_information8230">For Further Information&#8230;</h2>

<p><a href="/contract4j">polyglotprogramming.com/contract4j</a> is the home page for Contract4J5 and Contract4JBeans. It is developed by <a href="/aspectresearchassociates">Aspect Research Associates</a> (ARA), a consulting company specializing in <em>Polyglot</em> Programming technologies, such as Aspect-Oriented, Functional, and Object-Oriented Programming, &#8220;enterprise&#8221; Scala and Java, and Ruby on Rails. ARA also manages the <a href="/aspectprogramming">Aspect Programming</a> web site, where you will find more information and whitepapers on Contract4J5 and Aspect-Oriented Software Development (AOSD), in general.</p>

<p>We recently released the first version of a new AOP framework for Ruby called <a href="https://github.com/Agilefreaks/Aquarium">Aquarium</a>. The examples included with Aquarium include a basic Design-by-Contract module.</p>

<p>The <a href="https://www.ibm.com/developerworks/views/java/libraryview.jsp?search_by=aop@work:">AOP@Work</a> series at <a href="https://developerWorks.com">developerWorks.com</a> contains an <a href="https://www.ibm.com/developerworks/java/library/j-aopwork17.html">article about Contract4J5</a>. It introduces Design by Contract and how Contract4J5 supports it in Java. The article concludes with a discussion of emerging trends in Aspect-Oriented Design.</p>

<p>The AOSD.06 Conference in Bonn, Germany (March 19-24) featured a talk in the Industry Track on Contract4J5, specifically on the lessoned learned about writing generic, reusable aspects in AspectJ while implementing Contract4J5. There was also a paper on aspect-oriented design patterns in Contract4J5 in the ACP4IS workshop. Both papers can be found at the conference <a href="https://aosd.net/2006">website</a>.</p>

<p>The AOSD.07 Conference in Vancouver, British Columbia (March 12-16) featured a talk in the Industry Track on emerging principles of Aspect-Oriented Design, based on adaptations of well-known Object-Oriented Design principles. The paper can be found at the conference <a href="https://aosd.net/2007/">website</a>.</p>

<p>The definitive site on AOSD is <a href="https://www.aosd.net">aosd.net</a>.</p>

<p>See <a href="https://www.eclipse.org/aspectj">aspectj.org</a> for information on AspectJ, which was used to implement Contract4J.</p>

<p>For more on Design by Contract, see <a href="https://www.eiffel.com/developers/design_by_contract.html">Building bug-free O-O software: An introduction to Design by Contract(TM)</a> and the discussion of DbC in the larger context of Agile Methods in Martin, et al., &#8220;Agile Software Development: Principles, Patterns, and Practices&#8221;, Prentice Hall, 2003 (ISBN 0-13-597444-5).</p>

<p>For alternative approaches to doing Design by Contract in Java, see the <a href="https://www.google.com/url?sa=t&amp;source=web&amp;cd=1&amp;ved=0CBsQFjAA&amp;url=http%3A%2F%2Fbarter.sourceforge.net%2F&amp;ei=cMhOTbPQGcatgQfXpMQL&amp;usg=AFQjCNHvW-VD4TQh964pjIZZN2zSIEi5fA">Barter</a> project, which uses <a href="https://www.google.com/url?sa=t&amp;source=web&amp;cd=1&amp;sqi=2&amp;ved=0CBkQFjAA&amp;url=http%3A%2F%2Fxdoclet.sourceforge.net%2F&amp;ei=ishOTbD_C87egQfB4pUg&amp;usg=AFQjCNFXfXQantxoEPUiSvf_hjnw0lnSPQ">XDoclet</a> and also generates AspectJ. Barter was an inspiration for Contract4J5.</p>

<p>JBoss AOP has basic support for contracts. Spring AOP may have similar support.</p>

<p>Copyright © 2003-2011 Aspect Research Associates. All Rights Reserved.</p>


---
layout: page
title: Contract4JBeans README
---
{% include JB/setup %}


<pre>
Contract4JBeans (Experimental "JavaBeans"-like form) README

v0.3.0.0   February 20, 2006
v0.2.0.0   October 5, 2005
v0.2.0M1   August 15, 2005

Dean Wampler 
http://www.polyglotprogramming.com/contract4j
http://www.aspectprogramming.com/contract4j

Contents:

** Introduction
** What Is "Contract4JBeans"?
  --- How Does Contract4JBeans Support Design by Contract?
  --- How Do I Use Contract4JBeans?
  --- Show Me the Code!
  --- Invocation and Configuration of Contract4JBeans
  --- Debugging Tips
** TODO Items
** Notes for Each Release
** For Further Information...

** Introduction

For the Copyright statement and a general overview of
Contract4J and Design by Contract, see the 
<%= link_to 'Contract4J5 README', :action => 'c4j5'%>.


** What Is "Contract4JBeans"?

After the initial version of Contract4J, a second, 
experimental version was developed that used a 
JavaBeans-like syntax for defining tests as regular
Java methods. Hence, it is called Contract4JBeans. 
This version was developed as a way of avoiding some 
issues with the original annotation-based version 
(which have since been resolved). While Contract4JBeans
demonstrates some interesting ideas, it is considered 
an inferior approach and it is not being developed 
further. The main flaws are:
  - Very poor runtime performance
  - The role of the tests as part of the component
    interface is more obscure than for the annotation
    approach used in Contract4J5.
However, it may be of interest to AOP tool writers, so 
it will remain publicly available. To distinguish the
two versions of Contract4J, the annotation version is
called Contract4J5 and the "beans" version is called
Contract4JBeans in the various READMEs and other
documentation.


--- How Does Contract4JBeans Support Design by Contract?

Contract4JBeans and Contract4J5 take very different 
approaches to supporting DbC, each with strengths and 
weaknesses.

I'm a long-time believer in DbC and wanted to use it in
Java. A few years ago, I discovered the clever "Barter"
project, which supports DbC in Java using XDoclet tags
and AspectJ code generation to perform the tests as AspectJ 
"advice" (http://barter.sourceforge.net/). 

A problem with doclet-based approaches is that they are 
buried in the comments and hence decoupled from the 
runtime environment. Java 5 introduced annotations, which
are like "javadoc tags for code". In particular,
annotations are used to ascribe meta-information to the
code and to support optional runtime exploitation of that
information. Annotations are a logical tool for associating
contract tests with code and Contract4J5 defines a set of 
annotations for specifying contracts in the form of 
executable statements. The details are discussed below. 
A contrived example suffices for now:

@Contract
public class MyClass {
  @Invar ("name != null")
  private String name;

  @Pre ("n != null")
  public void setName (String n) { name = n; }
  @Post ("$return != null")
  public String getName () { return name; }

  @Pre ("n > 10 && s != null")
  @Post ("$return != null")
  public String  doIt (int n, String s) {...}
  ...
}

The "@Contract" annotation tells Contract4J5 that 
"MyClass" defines tests. The tests are:
1) The attribute "name" has an invariant test that 
it can never be null (after the object has been 
constructed...).
2) The "setName" method has a precondition that the
value of the parameter cannot be null.
3) The "getName" method has a postcondition that it
can never return null.
4) The "doIt" method has both a pre- and a 
postcondition test.

Contract4J5 is described in more detail in the
<%= link_to 'Contract4J5 README', :action => 'c4j5'%>.

This approach, called "Contract4JBeans" or "ContractBeans"
for short, is an experimental version that uses a 
different approach. It started with the need to remove
some of the disadvantages of the first versions of
Contract4J. This approach is considered experimental
because it doesn't really offer a better approach and 
it will not be developed further. However, it has some
interesting characteristics that may be useful for other
tools that use AOP and AspectJ.

Contract4JBeans introduces a JavaBeans-like convention
for defining test as special methods that are discovered
through runtime reflection and invoked at appropriate
join points. Here is the previous Contract4J5 example
rewritten in the Contract4JBeans style:

public class MyClass {
  private String name;
  public boolean invarName() { return name != null; }

  public void    setName (String n) { name = n; }
  public boolean preSetName (String n) { return n != null; }
  public String  getName () { return name; }
  public boolean postGetName (String result) { return result != null; }

  public String  doIt (int n, String s) {...}
  public boolean preDoIt (int n, String s) {
    return n > 10 && s != null;
  }
  public boolean postDoIt (String result, int n, String s) {
    return result != null;
  }
  ...
}

All test methods return true if the test passes 
or false if it doesn't. The invariant test for the 
"name" attribute is called "invarName". The 
precondition test for "setName" is called "preSetName".
All precondition and invariant tests for methods take
the same argument list as the method itself. 
Postcondition tests, like the ones for "getName" and
"doIt" pass as the first argument the result of the
method call (compare with the "$return" keyword used
in Contract4J5 tests), followed by the same argument
list passed to the method. 

Note that, unlike the annotation form, the test methods
could actually appear anywhere in the class, not 
necessarily adjacent to the members they test, and 
the test methods could be called for other purposes.

So, the contract tests are defined without annotations.
This was not an original goal of the redesign, but a 
result of the naming convention. It has the benefit 
that Contract4JBeans can be used in pre-Java 5 environments.

Further comparisons of these two Contract4J approaches
can be found in the papers "The Challenges of Writing
Reusable Aspects in AspectJ" that was presented as part
of the Industry Track at at AOSD.06, March, 2006 and
"Pattern-Based Interfaces and Contract4J" that was 
presented as part of the ACP4IS Workshop at AOSD.06.
Contact me if you would like copies of these papers.

---- Inheritance Behavior of Contracts

As discussed in the <%= link_to 'Contract4J5 README', :action => 'c4j5'%>,
DbC has rules for proper behavior of inherited 
contracts. Contract4JBeans does not have any 
"native" support for the correct covariant or 
contravariant behavior, as appropriate, under
inheritance. C4J does invoke base-class tests on 
derived class overrides, like method calls, but it
relies on the test writer to implement tests
 appropriate. This is really no different than the
regular steps required to write good classes and
methods that obey the LSP and it reflects the fact
that C4J can't really know how to satisfy all state
requirements.

The patterns that test writers should use are these.
Only override a precondition test if you intend to
loosen it. Call the parent test and if it fails, then
test whether or not the "extra" allowed "states" are 
met (i.e., "or" the results). If so return true. 
Otherwise, return false. 

For postcondition tests, never relax the conditions, 
only strengthen them. Use similar coding logic; call
the parent test and if it passes, then test if "state"
is still within the tightened constraints (i.e., "and"
the results).

Here is a contrived example:

public class Base {
  public String generateString (int n) { 
    return Integer.toString(n);  
  }
  public boolean preGenerateString  (int n) {
    return n > 0;
  }
  public boolean postGenerateString (String result, int n) {
    return result != null && result.length() > 0; 
  }
  ...
}

public class Derived extends Base {
  public String generateString (int n) {
    String result = super.generateString(n);
    return "Result: "+result;
  }
  public boolean preGenerateString  (int n) {
    return super.preGenerateString(n) || n == 0;
  }
  public boolean postGenerateString (String result, int n) {
    return super.postGenerateString(result, n) && result.length() > 8; 
  }
  ...
}

The "generateString" method will return a string that is "based in some way"
on the input argument. The Base version simply converts to a string, while
the Derived version adds a prefix. (I said this was contrived!)

The Base method also has pre- and postconditions. The precondition says that
the integer argument must be positive and the postcondition says that the
returned string can't be null and it must not be empty.

The Derived precondition relaxes the Base precondition, it allows the argument
to be 0. Again, any clients using a Base will still assume a positive value
is required so the precondition will still always pass. In contrast, the
postcondition test is now tightened to require the returned value to be 9 or
more characters long (consistent with the behavior of Derived.generateString).
Since 9 > 0, clients using a Base reference to a Derived object will experience
no surprises, as all returned strings will still satisfy the Base
postcondition.

Note that the Derived precondition test calls the parent's test and "or's" the
result with a test of the extra allowed state, thereby expanding the allowed
values. Of course, I could have simply written "return n >=0;", but then I
would have to maintain the common subset of the test condition in two places.

Similarly, the Derived postcondition test "and's" the parent's result with
a test of the more constrained allowed states.

WARNING: Unfortunately, there is currently a bug that prevents you from writing
covariant postcondition tests. Using the example above, note that the method
"postGenerateString()" is public and defined in both classes, meaning that it
is virtual and overridden in the derived class. When the derived 
"generateString()" calls the parent method using "super", C4J will look for and
find the corresponding base-class "postGenerateString()". However, when it
invokes this method using Java reflection, if the object is actually of type
Derived, the derived-class version will be invoked instead. This is because
java.lang.reflect.Method.invoke() uses dynamic dispatch. However, if the
derived-class postcondition test "tightens" the constraints, then the test may
fail, even if the derived method *being tested* would make that test pass. 

The bug reflects a collision between normal Java method dispatch and the
semantics of contract inheritance. It's a flaw of the Contract4JBeans approach.
The ideal solution is to prevent the derived-class test from being used to test
the parent-class method. This may not be possible, since Java reflection is 
used to find and invoke the tests. A fall back solution is to only allow tests
to be run before and after the most derived version of the method under test. 
In other words, to ignore the tests when "super." methods are executed. This
can be implemented by maintaining context information within Contract4JBeans
code itself. 

Several unit tests in 
com.aspectprogramming.contract4jbeans.test.TestContractTestSubclass,
including testContractTestClassContravariantPreCovariantPost1() and
testContractTestClassContravariantPreCovariantPost1(), "pass" only because 
they explicitly anticipate this incorrect behavior and disallow postcondition
covariance.

--- How Do I Use Contract4JBeans?

This section describes how to use Contract4JBeans. For information on using
Contract4J5, the annotation form, see the README.txt in its distribution.

This distribution contains ant files and examples of how to use 
Contract4JBeans. The examples are also used by the unit/acceptance test suite.

---- Installation:

The following commands are Linux/Unix commands. They also work with Cygwin bash
on Windows. Or, use WinZip or an equivalent utility to expand the compressed 
tar file. 

You will need Java 5, AspectJ 5, and JUnit 3.8.1.

  tar xvzf contract4jbeans_030.tar.gz 

You can use the installed "contract4jbeans.jar" file as is. If you want to
rebuild it, use the ant driver script "build.sh" in this directory, edit 
the file and change the environment variable definitions to have the 
appropriate values for your environment or set up your environment so they
are defined apropriately before invocation of the script. If you have the
environment variables already defined, you can also invoke "ant" directly.

The distribution has the following structure:

build.sh - Unix/Linux build driver script
build.bat - Windows build driver script
build.xml - Ant build script

src - The source code tree.
src/com/aspectprogramming/contract4jbeans - The implementation code.

test - The JUnit unit/acceptance tests, which also function as usage examples.
src/com/aspectprogramming/contract4jbeans/test - The test classes. The files
beginning with "Test*" are JUnit tests. The other classes are example classes
used by the tests, which also provide usage examples.

classes - where build artifacts (except the jars) are stored
doc - Where Javadocs are written
contract4jbeans.jar - The runtime deployment jar. It contains the build 
products from "src".
contract4jbeans-test.jar - The jar containing the build products from "test".
Not required as part of the normal runtime deployment.

You can use the installed "contract4jbeans.jar" file as is (see below). 
To build Contract4J:

  6a) ./build.sh all
  6b) build.bat all
or
  6b) ant all

You will probably need to edit environment variables defined in the scripts.


Next, we'll look at code examples, then return to a discussion of invoking and
configuring Contract4J.

---- Show Me the Code!

Here is how to define Contract4JBeans tests in your code so that they can
be discovered and executed at runtime.

See the examples in
  contract4jbeans/test/com/aspectprogramming/contract4jbeans/test.
The files beginning with "Test*" are JUnit tests, some of which define nested
classes that illustrate usage. The other classes and aspects are supporting
tools and also demonstrate how to define tests and use Contract4JBeans.
Note that "Contract4JTestScope.aj" is a special example of an aspect you
should define in your environment that tells Contract4JBeans the "scope" of
where to look for tests. (For example, you don't want to include the JDK 
packages.) Let's look at some of the test examples.

----- Contract4JTestScope.aj:

A marker interface is defined called "Contracts" that is used
to indicate which classes define tests. It defines no members. Users can
explicitly declare their classes to implement this interface (see, for example, 
contract4jbeans/test/com/aspectprogramming/contract4jbeans/test/TestProperties.java).
However, the better way is to use AspectJ's intertype declaration (a.k.a.
introduction) feature to make Contracts a "parent" of all the classes
in your project's packages for which you intend to define tests. 

Contract4JTestScope.aj provides an example. The form of this file and 
similar files for your projects is as follows:

import  com.aspectprogramming.contract4jbeans.Contracts;
public aspect Contract4JTestScope { 
  declare parents: (<type specification>) implements Contracts;
}

Any unique name for the aspect will do. The "<type specification>" is, for
example, "com.aspectprogramming.contract4jbeans.test.ContractTest*" in 
Contract4JTestScope.aj. Any valid AspectJ type specification can be used.

----- ContractTestClass.java

Let's look at the most complete example, ContractTestClass.java in
the test directory. Many of the JUnit tests exercise the tests in
this file. Here is a partial listing of the file, annotated with comments.

...
// IContractTestClass defines some methods that this class and
// others define; used primarily for implementation convenience
// as well as for testing inheritance behavior.
public class ContractTestClass implements IContractTestClass {
  // Make "name" protected so we can test what happens when
  // subclasses manipulate it directly. (Normally would be private.)
  protected String name;
  public void    setName   (String s) { name = s; }
  public String  getName   () { return name; }
    // The invariant test for "name", which calls a service
    // method "checkStr" that is used by other tests, too.
    // Class-wide and method-specific invariant tests (examples below)
  // are called before and after all non-priviate instance method calls.
  // Contracts are never tested for static methods, because they don't
  // modify an object's state. The are never tested within contract test
  // methods themselves, either. Field invariant tests are executed
    // after every read and write of a field, except they are not
  // called within the context ("cflowbelow") of constructors, since the
  // fields are only assumed to be properly initialized after the
  // constructor completes, at which point the invariant tests are 
  // evaluated.
  public boolean invarName () { return checkStr (name); }

  // Contrived function for testing strings.
  protected static boolean checkStr (String s) {
    return (s != null && s.length() > 0);
  }

  /**
   * An instance method that has both pre- and postcondition tests.
   */
  public String methodWithPrePost (int flag, String s) {
    setName (s);
    if (s != null && s.equals("force null")) {
      return null;
    }
    return String.format ("flag = %d, s = %s", flag, s);
  }
  /**
   * The precondition test for the method. Note that the argument list is
   * the same as "methodWithPrePost".
   */
  public boolean preMethodWithPrePost (int flag, String s) {
    return (flag > 0 && s != null && s.length() > 0);
  }
  /**
   * The postcondition test for the method. Note that the argument list is
   * the same as "methodWithPrePost", except that the first argument is the
   * "result" returned by "methodWithPrePost".
   */
  public boolean postMethodWithPrePost (String result, int flag, String s) {
    return (result != null && result.length() > (s.length() + 13));
  }

  /**
   * Like methodWithPrePost, but with a void return value.
   */
  public void voidMethodWithPrePost (int flag, String s) {
    setName (s);
  }
  public boolean preVoidMethodWithPrePost (int flag, String s) {
    return (flag > 0 && s != null && s.length() > 0);
  }
  /**
   * If the method returns void, the argument list to the postcondition
   * test does not have a leading argument for the return value!
   * Contrast with the previous case of non-void returns.
     */
  public boolean postVoidMethodWithPrePost (int flag, String s) {
    return !name.equals("void");
  }

  /**
   * An instance method that has an invariant test.
   */
  public String methodWithInvar (int flag, String s) {
    return "methodWithInvar does nothing!";
  }
  /**
   * The invariant test. The argument list matches the argument list of
   * the method being tested.
   */
  public boolean invarMethodWithInvar (int flag, String s) {
    return flag > 0;  
  }
  /**
   * Note that this method attempts to define a postcondition test on another
   * test method. In fact, it is ignored!
   */
  public boolean postInvarMethodWithInvar (Boolean result, int flag, String s) {
    return !s.equals("force fail"); 
  }

  /**
   * Tests on static methods are ignored, since static methods don't affect
   * object state. However, it's conceivable that contracts on these methods
   * would be useful, so this is a planned enhancement.
   * You'll see warning messages during compilation for this method.
   */
  public static boolean staticMethodWithTests (int flag) {
    return true;     
  }
  // never called!    
  public static boolean preStaticMethodWithTests   (int flag)            {return false;}
  // never called!    
  public static boolean postStaticMethodWithTests  (boolean b, int flag) {return false;}
  // never called!    
  public static boolean invarStaticMethodWithTests (int flag)            {return false;}

  /**
   * Constructor with precondition, postcondition, and invariant tests.
   */
  public ContractTestClass (String n) {
    setName (n);
  }
  /**
   * Note that since a constructor is called before the object actually exists,
   * since obviously the constructor is part of the object creation and
   * initialization process, it is necessary to make the test methods static!
   */
  public static boolean preContractTestClass   (String n) { return checkStr (n); }
  public static boolean postContractTestClass  (String n) { return n != null && !n.equals("bad name"); }
  public static boolean invarContractTestClass (String n) { return n != null && !n.equals("bad invar name"); }

  /**
   * A class invariant test, which looks like an invariant test for a
   * default c'tor. You can tell the difference because this method
   * is not static, unlike c'tor methods.
   */
  public boolean invarContractTestClass () { 
    return name == null || !name.equals("bad class invar name"); 
  }
}


---- Details of Contract Specifications

Here are the rules for using Contract4J, which clarify the examples just
discussed.

1) Implement the "Contracts" Interface

Either explicitly declare your classes to implement this interface (in 
the com.aspectprogramming.contract4jbeans package) or use an aspect with
an intertype declaration:

import  com.aspectprogramming.contract4jbeans.Contracts;
...
public class <class name> ... implements Contracts { ... } 

or 

import  com.aspectprogramming.contract4jbeans.Contracts;
public aspect <aspect name> { 
  declare parents: (<type specification>) implements Contracts;
}

Where 
  <class name> is the name of a class with tests.
  <aspect name> is an arbitrary, but unique name for the aspect.
  <type specification> is an AspectJ type specification, e.g., one or
    more packages and/or classes (See the AspectJ documentation for details).

The advantage of the latter approach is that you can specify whole packages 
in the type specification, etc. For a complete example of the latter approach,
see  com/aspectprogramming/contract4j/test/Contract4JTestScope.aj in the 
distribution.

2) Define the Test Methods, Following the Method Signature Conventions

The conventions for the test methods are as follows:

a) Field (Attribute) Invariant Tests

Only invariant tests can be defined for fields. These tests will be evaluated
after every read or write of the field, except during constructor calls, where
the field may not yet be initialized, and during other test methods. The
evaluation is only done after access, not before, to allow "lazy
initialization" and because the test only matters after access, just before
the field is used by the "accessor". However, while test evaluation is not
done during a constructor call, it is performed after the constructor call
completes.

Test Format:
  [!private] boolean invar<Field name> () {...}

The method should not be private, since the contract is part of the interface
exposed to clients and/or derived class writers. It must return boolean. The 
name must be the field name, with the first character capitalized, prefixed 
with "invar". It must take no arguments.

Example: 
For "private String name" field,
    public boolean invarName() {...}

b) Instance Method Tests

Methods may have pre-, post-, and invariant-condition tests that will be
evaluated before, after, and "around" (both before and after) the method
execution, respectively. Generally speaking, method tests are usually only
"interesting" for non-private methods. Hence, tests defined for private
methods are ignored. Any test methods that are defined to test other test
methods (nesting) are ignored! (These restrictions are, in part,
simplifications to reduce the design complexity and the overhead of test 
method discovery.)

Precondition Test Format:
  [!private] boolean pre<Method name> (arglist) {...}

The method should not be private, since the contract is part of the interface
exposed to clients and/or derived class writers. It must return boolean. The 
name must be the method name, with the first character capitalized, prefixed 
with "pre". It must take the same argument list as the method being tested.

Postcondition Test Format:
  [!private] boolean post<Method name> (Type result, arglist) {...}

The method should not be private, since the contract is part of the interface
exposed to clients and/or derived class writers. It must return boolean. The
name must be the method name, with the first character capitalized, prefixed 
with "post". If the original method does not return void, then the test method 
must take an argument list where the first argument has the same return type as
the method being tested (the argument name itself is arbitrary; "result" is just
a convention). The rest of the argument list must be the same argument list as
the method being tested. However, if the original method returns void, then
the argument list for the test method must match the original method's argument
list exactly.

Invariant Condition Test Format:
  [!private] boolean invar<Method name> (arglist) {...}

The method should not be private, since the contract is part of the interface
exposed to clients and/or derived class writers. It must return boolean. The
name must be the method name, with the first character capitalized, prefixed 
with "invariant". It must take the same argument list as the method being 
tested.

Example: 
For "public String foo (String name, int flag)" method,
    public boolean preFoo   (String name, int flag) {...}
    public boolean postFoo  (String result, String name, int flag) {...}
    public boolean invarFoo (String name, int flag) {...}
For "public void bar (String name, int flag)" method,
    public boolean preBar   (String name, int flag) {...}
    public boolean postBar  (String name, int flag) {...}
    public boolean invarBar (String name, int flag) {...}

c) Constructor Tests

Constructors may have pre-, post-, and invariant-condition tests that will be
evaluated before, after, and "around" (both before and after) the method
execution, respectively. Unlike instance method tests, constructor tests can
be defined for private constructors and they will be evaluated, since in some 
cases instance creation might only be done with a private constructor call
(e.g., in factory patterns) and proper construction is important. 

Precondition Test Format:
  ... static boolean pre<Class name> (arglist) {...}

The method may be private, but it should only be private if the constructor is
private. The method must be static, because the instance doesn't exist yet upon
which to call an instance test method. It must return boolean. The name 
must be the class name, which is of course equivalent to the constructor name,
prefixed with "pre". It must take the same argument list as the constructor 
being tested.

Postcondition Test Format:
  ... static boolean post<Class name> (arglist) {...}

The method may be private, but it should only be private if the constructor is
private. The method must be static, primarily for "symmetry" with precondition
and invariant tests, even though after the constructor returns the instance
does exist with which a call to an instance test method could be made! This
"symmetry" simplifies the rules for constructor tests and thereby minimizes
the potential for confusion.
It must return boolean. The name must be the class name, which is of course
equivalent to the constructor name, prefixed with "post". It must take the
same argument list as the constructor being tested. (Note that because 
constructors never return a value, test methods always take the same argument 
list as the constructor being tested.)

Invariant Condition Test Format:
  ... static boolean invar<Class name> (arglist) {...}

The method may be private, but it should only be private if the constructor is
private. The method must be static, because before the constructor call the
instance doesn't exist yet upon which to call an instance test method. It must
return boolean. The name must be the class name, which is of course equivalent
to the constructor name, prefixed with "invar". It must take the same argument
list as the constructor being tested.

Example: 
For "public MyClass (String name, int flag)" constructor,
    public static boolean preMyClass   (String name, int flag) {...}
    public static boolean postMyClass  (String name, int flag) {...}
    public static boolean invarMyClass (String name, int flag) {...}

d) Class Invariant Tests

Class invariant tests evaluate the state of an instance before and after every
non-private instance method and constructor call, with the exception that they
aren't evaluated before the constructor calls, since the object isn't 
initialized to a valid state yet, nor are they called before calls to "get" or
"set" JavaBeans methods, since lazy evaluation may be used to initialize or
modify the state. (Recall, however, that any class-level, field-level, or
constructor-level invariant- or post-condition tests could test the state, so
careful construction of tests is required.)

Test Format:
  [!private] boolean invar<Class name> () {...}

The method should not be private, since the contract is part of the interface
exposed to clients and/or derived class writers. It must return boolean. The
name must be the class name, with the first character capitalized, prefixed
with "invar". It must take no arguments.

It would be easy to confuse class-level invariants with constructor invariants.
You can tell the difference because constructor invariants are always declared
static, since the instance doesn't exist before the constructor call, while
the class-level tests are actually instance state tests. Don't confuse the
word "class-level" with "static"; the tests are not static, "instance-
independent" tests.

Also, test methods for constructors that take arguments also take the same
argument list, while class-level invariant tests always take no arguments. 


----- Configuring the Behavior

The behavior of Contract4JBeans can be configured through property files or at 
runtime using API calls.  At startup, Contract4J will look for property
files named "contract4j.properties", "contract4jbeans.properties", 
"contract4j.xml", and "contract4jbeans.xml". The XML files must follow the
new Java 5 XML format for properties. Ifmore than one of these files is found,
settings in the later ones override settings in the earlier files. The default
file name can be overridden by defining the environment variable
"$CONTRACT4J_PROPS_FILE" or the Java System property 
"com.aspectprogramming.contract4jbeans.properties.filename". (The latter value
takes precedence.)

Which ever name(s) is used, Contract4JBeans searches for matching files in the
following directories, in order:
  $CONTRACT4J_HOME (environment variable - if defined)
  $HOME      (environment variable)
  user.home  (System property - usually equivalent to $HOME)
  user.dir   (System property - the current working directory)
  com.aspectprogramming.contract4jbeans.properties.dirname (Sys. prop. - if defined)

The last values read among all the files processed take precedence.

In the discussion below about particular properties, they can be defined in
any of these files. 

Note: For all properties currently defined, if a value is empty, it is ignored!


1) Turning Tests On and Off

a) Build Time

To completely disable contract checking, compile without "contract4jbeans.jar"
in your path. This is recommended for "production" builds when you don't want
any test overhead.

b) Property Files

To enable or disable test types in a property file, set the following 
properties:

  com.aspectprogramming.contract4jbeans.contracts = true|false
  com.aspectprogramming.contract4jbeans.pre = true|false
  com.aspectprogramming.contract4jbeans.post = true|false
  com.aspectprogramming.contract4jbeans.invar = true|false

Where the property value must be "true" or "false" (actually all non-true
values case ignored, are treated as false). The "contracts" property enables
or disables all others, if present, unless the one or more of the more
specific properties are specified, in which case it dominates.

c) API Calls

To disable one type of test, e.g., precondition tests, you can do this 
programmatically, by calling the following static method:

import com.aspectprogramming.contract4jbeans.Contract4J;
...
  // Turn off precondition tests
  Contract4J.setEnabled (Contract4J.TestType.Pre,  false);
  // Turn on precondition tests
  Contract4J.setEnabled (Contract4J.TestType.Pre,  true);
  // Turn off postcondition tests
  Contract4J.setEnabled (Contract4J.TestType.Post, false);
  // Turn on postcondition tests
  Contract4J.setEnabled (Contract4J.TestType.Post, true);
  // Turn off invariant tests
  Contract4J.setEnabled (Contract4J.TestType.Invar, false);
  // Turn on invariant tests
  Contract4J.setEnabled (Contract4J.TestType.Invar, true);

2) Configuring "Reporters"

By default, all output is written to using a simple logging-like interface
called "Reporter", defined in com.aspectprogramming.contract4jbeans.Reporter. 
It supports 5 levels of output and the ability to set the message threshold.
A default implementation writes to standard out or error with a threshold
of "Warn". Separate Reporters can also be defined and configured for some
of the other objects used by Contract4J (discussed below).

a) Property Files

To specify a different global Reporter class on the CLASSPATH, and/or to
change the threshold in a property file, set the following properties:

  com.aspectprogramming.contract4jbeans.reporter = <Reporter subclass name>
  com.aspectprogramming.contract4jbeans.reporter.threshold = <level>

Where "<Reporter subclass name>" is a fully-qualified name of subclass
of "Reporter" and where "<level>" is one of:

  d, D, debug, Debug  - debug level
  i, I, info, Info    - info level
  w, W, warn, Warn    - warning level
  e, E, error, Error  - error level
  f, F, fatal, Fatal  - fatal level

b) API Calls

To set the reporter or to change the threshold programmatically, use the
following API calls:

  import com.aspectprogramming.contract4jbeans.*;
  ...
  Reporter r = ...;    // Define instance appropriately...
  Contract4J.setReporter(r);
  ...
  Contract4J.getReporter().setThreshold(level);

where "level" is one of 

  Reporter.Level.Debug
  Reporter.Level.Info
  Reporter.Level.Warn
  Reporter.Level.Error
  Reporter.Level.Fatal

If you implement your own Reporter, e.g. to use a logging API like Log4J,
use WriterReporter.java as an example of how to proceed.

If you just want to write to a particular output "stream" and that stream is
a subclass of "java.io.Writer", you can do the following:
  import com.aspectprogramming.contract4jbeans.*;
  ...
  Writer myWriter = ...;
  WriterReporter wr = (WriterReporter) Contract4J.getContractEnforcer().getReporter();
  wr.setWriters (myWriter);   // use the same Writer for all levels
or
  wr.setWriter (level1, myWriter); // use only for "level1" and "level2"
  wr.setWriter (level2, myWriter);

Note that the default Reporter instance returned by this "getReporter()" call
actually returns an object of the derived type "WriterReporter"
(com.aspectprogramming.contract4jbeans.WriterReporter). The cast to WriterReporter
assumes that Contract4J.getContractEnforcer().setReporter(...) hasn't been
called with a different Reporter that is not of type WriterReporter.

Note: there are no corresponding properties for these configuration options.

3) Configuring Test Discovery and Handling

The interface TestMethodFinder specifies an abstraction for how test methods
are found. TestMethodFinderHelper implements some of the methods and the rest of
the methods are implemented with a default algorithm used by Contract4J in the
DefaultTestMethodFinder class, a subclass of TestMethodFinderHelper.
TestMethodFinder also uses the system default Reporter, but you can specify
and configure a unique one, if you want.
TestMethodFinder reports when a test method is found or not found. The 
threshold (a Reporter.Level value) is a configurable property.

Similarly, ContractEnforcer specifies an abstraction for how test methods are
invoked and what to do if the test fails. ContractEnforcerHelper implements
some of the methods and the rest have a default algorithm implemented by
DefaultContractEnforcer, a subclass of ContractEnforcerHelper.
When test failures are reported, the stack trace can be printed. This is a
configurable property.
ContractEnforcer also uses the system default Reporter, but you can specify
and configure a unique one, if you want. 

a) Property Files

To specify different objects, to configure their properties, to change the
Reporter objects used by them, and to change the properties of those reporters,
set the following properties:

  com.aspectprogramming.contract4jbeans.contract.enforcer = <ContractEnforcer subclass>
  com.aspectprogramming.contract4jbeans.contract.enforcer.show.stack.trace = true|false
  com.aspectprogramming.contract4jbeans.contract.enforcer.reporter = <Reporter subclass>
  com.aspectprogramming.contract4jbeans.contract.enforcer.reporter.threshold = <level>

  com.aspectprogramming.contract4jbeans.test.method.finder = <TestMethodFinder subclass>
  com.aspectprogramming.contract4jbeans.test.method.finder.found.level = <level>
  com.aspectprogramming.contract4jbeans.test.method.finder.not.found.level = <level>
  com.aspectprogramming.contract4jbeans.test.method.finder.reporter = <Reporter subclass>
  com.aspectprogramming.contract4jbeans.test.method.finder.reporter.threshold = <level>

The test.method.finder.found.level and test.method.finder.not.found.level are
threshold levels for reporting a test method was or was not found, respectively.
The default values are "Debug" and "Info", respectively, the rational being that 
you might be interested in knowing when a test method is missing and hence needs 
to be added, but you only want notification of a test found when debugging.

Warning: If you change the threshold of a reporter, but you didn't define a 
unique instance separate from the global reporter, the change will actually
be made to the global reporter!

b) API Calls

The instances of the "Default*" classes that are used by the Contract4J aspect
can be replaced with alternative implementations. To set a new ContractEnforcer,
use
  Contract4J.setContractEnforcer(...);

To set a new TestMethodFinder, use
  Contract4J.getContractEnforcer().setTestMethodFinder(...);

To set or configure the Reporter for either object, both support "getReporter()"
and "setReporter()" methods. (See the warning in the previous subsection.)


----- How Contract4JBeans Works at Compile and Runtime

All classes with tests must be compiled with AspectJ with "contract4jbeans.jar"
in the CLASSPATH. Or, the jars containing precompiled Java files must be woven by
AspectJ; put those jars in the "-inpath" option to ajc or the corresponding
"inpath" or "inpathRef" options for the iajc ant task.

During compilation, advice will be inserted before and after ALL non-private 
method calls, all constructor calls, and all field accesses in the classes that
implement the Contracts marker interface, as described above. 

At runtime, when a join point with advice is executed, the advice uses Java and
AspectJ reflection to find a test method that corresponds to the join point. 
The method must match the format specifications described above. If a matching
method is not found in the same class, parent classes and interfaces[1] are
searched. If a method is found, it is executed. If it returns false, an error
message is reported, a com.aspectprogramming.contract4jbeans.ContractError (an
unchecked exception) is thrown, and program execution is terminated.

[1] AspectJ's intertype declaration mechanism can be used to insert "default"
methods into interfaces. The test/example IContractTestClass2,
IContractTestClass2_Contract.aj, and ContractTestClass2 illustrate how this
can be done.
  IContractTestClass2.java         A normal Java interface
  IContractTestClass2_Contract.aj  Introduces test methods into classes
    implementing the interface. It cannot insert Constructor tests, however,
  since they must be static and AspectJ respects the Java requirement
  that interfaces can only declare instance methods
  ContractTestClass2.java         A normal Java class that implements
    IContractTestClass2 and "picks up" the tests in the corresponding
  aspect.

Summary and "Tips":

1) Every class defining tests must explicitly implement the Contracts interface
   or define an aspect that "introduces" this interface through the
   "declare parents:" construct. Otherwise, all tests in the class are ignored.
2) All tests must obey the specification conventions defined above. There are a
   few "gotchas" to watch.
  - Postcondition methods must declare as their first argument a parameter of
    the same type as the return type of the method they are testing. The rest
    of the argument list must match the list of the method under test. The
    value returned by the method will be passed as this first parameter. 
    However, if the method returns void (including constructors), then no
    "return" parameter is declared. In this case, the test method's argument 
    list must match the original method's argument list exactly.
  - Constructor test methods must be declared static, since the instance is
    being constructed during precondition and invariant test execution. While
      the instance does exist when postcondition tests are executed, for
    simplicity and "symmetry", these tests are also static.
  - Class invariant tests are instance state tests, not static (instance-
    independent) tests. So, they are not declared static. Hence, an invariant
    test for a default (no argument) constructor is static, while the class
    invariant test, which also takes no arguments, is an instance method.
      Note that these two methods will have the same name: "invar<Class_name>"!
    - You can only define one pre, post, and invariant test method for each
    method and constructor. You can define only one field invariant test per
    field and one class invariant test per class. This is a result of the
    specific method signature format rules; it is not possible to define
    multiple tests of a particular type unambiguously.
  - Test methods defined for other test methods, and for static and private
      instance methods are ignored. However, test methods for private
    constructors are supported.
3) There are several supported configuration options. The details are described 
   above.
    - Which types of tests, pre, post, and/or invar, are enabled or disabled.
    - Setting the reporting (output) threshold.
  - Specifying the output "sink" where output is written (e.g., standard
    out/err, Log4J, etc.)
  - The class used to locate test methods.
  - The class used to execute test methods and handle failures.
4) Avoid writing test methods with "side effects", as they may be turned off, 
   thereby changing the runtime behavior of your application!


--- Invocation and Configuration of Contract4J

Contract4JBeans has been tested on Linux, MacOS X, and Windows. To build it from 
scratch, the following third-party tools are required, along with corresponding
"HOME" environment variable definitions needed by the ant build scripts:

1) JUnit 3.8.1 (JUNIT_HOME)
2) Java 1.5 (JAVA_HOME). 
3) AspectJ 1.5 (ASPECTJ_HOME). 
3) Ant 1.6.3 (ANT_HOME)

Also define "CONTRACT4J_HOME" to be the directory ".../contract4jbeans_030/contract4j"
where you installed it.

For your convenience, you can use the build driver scripts "build.sh" or
"build.bat". Edit the values of the environment variables defined in the
scripts as appropriate or define them in your environment before invoking 
the scripts.

Only Java and AspectJ are required if you simply use the built jars in the
distribution. Make sure your build process either compiles with AspectJ
or weaves precompiled jar or class files as described in the section
"How Contract4JBeans Works at Compile and Runtime".


---- Contract4J5

The Contract4J5 and Contract4JBeans distributions are separate. both can
be found by following the links at http://www.polyglotprogramming.com/contract4j.


** TODO Items:

Here is a brief list of some of the more important "todo" items, roughly in 
order of importance. 

--- V.0.3.0
1) Fix bug that prohibits covariant postcondition tests from working (see 
above).
2) Consider support for Java 1.4 and perhaps 1.3, since the bean-style 
interface uses no Java 5 constructs (e.g., annotations). However, the current
implementation does use Java 5 constructs internally.
3) Exploit aspects to manage the "Reporter" objects, which are hard-coded in
classes that use them.


** Notes for Each Release

*** v0.3.0 February 20, 2006

Changing version labeling and package scheme as discussed previously. 
No other functional changes were made.

*** v0.2.0 October 5, 2005

Implemented caching in the algorithm that searches for test methods, so this
process isn't repeated over and over again. See
  src/com/aspectprogramming/contract4j/ContractMethodCache.aj 
for details and approximate timing numbers.

*** v0.2.0M1 August 15, 2005

Contract4JBeans2 is a complete rewrite that introduces a new model for defining 
contracts and a simpler adoption process. The previously-required preprocessor 
step, running Sun's Annotation Processor Tool (APT), has been eliminated. 
Therefore, adoption of Contract4JBeans is a simpler process of including the 
"contract4J.jar" file in the ASPECTPATH for weaving (and optionally for 
compiling) and runtime deployment. Then, the developer writes DbC tests in 
plain Java.

The new model for defining contracts does not use annotations, which allows 
its use in pre-Java 5 environments. Instead, a JavaBeans-like convention is 
used to define the tests, as discussed elsewhere in this README.

This is a milestone release with a number of planned enhancements before the
final release. See the TODO items above for details.



** For Further Information...

http://www.polyglotprogramming.com/contract4j/ is the home page for Contract4J. It is hosted by 
Aspect Programming, http://www.aspectprogramming.com/, where you'll find more
information and whitepapers on Contract4J and Aspect-Oriented Software 
Development (AOSD), in general. 

The definitive site on AOSD is http://www.aosd.net.

See http://www.aspectj.org for information on AspectJ. Note that there are 
plans to incorporate Contract4J into the new standard library for AspectJ that 
will be introduced with the forthcoming, final AspectJ5 release.

For alternative approaches to doing Design by Contract in Java, see the
"Barter" project, which uses XDoclet and also generates AspectJ. Barter 
partially inspired Contract4J. http://barter.sourceforge.net/. 

There is a discussion group doing DbC in Java and possibly getting a future
version of Java to support DbC natively. See http://dbc.dev.java.net/.

Some more sophisticated approaches to program correctness include the J-LO
tool for runtime checks of temporal assertions about the program.
http://www-i2.informatik.rwth-aachen.de/Research/RV/JLO/

Another project is the Java Modeling Language (JML), which supports DbC
for Java.
http://www.cs.iastate.edu/~leavens/JML/
</pre>


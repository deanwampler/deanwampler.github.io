---
layout: page
title: Scala 3 Metaprogramming - Inline
include_social: true
---
{% include JB/setup %}

# Exploring "inline"

This page is adapted from a talk I gave at Scala in the City, July 29, 2021.

The following examples are adapted from "Programming Scala, Third Edition":
* [programming-scala.com](http://programming-scala.com)
* [github.com/deanwampler/programming-scala-book-code-examples](https://github.com/deanwampler/programming-scala-book-code-examples)
* [medium.com/scala-3/](https://medium.com/scala-3/scala-3-a-look-at-inline-and-programming-scala-is-now-published-9690ca43c23a)
* [My synopsis of other Scala 3 changes](https://deanwampler.github.io/scala3-highlights.html)

## What's All This About Then?

`inline` is part of the new metaprogramming facilities in Scala 3. It _forces_ the compiler to "inline" the marked code, if feasible:

* Method bodies are expanded in place, removing the method invocation overhead.
* Conditionals are replaced with the branch that would be taken.
    - This means the predicate value must be determined at _compile time_.

Let's see some examples. Here is an inlined method:

```scala
// src/script/scala/progscala3/meta/inline/Recursive.scala

inline def repeat(s: String, count: Int): String =
  if count == 0 then ""
  else s + repeat(s, count-1)
```

Let's try it and see what happens:

```scala
repeat("hello", 3)    // Okay
```

```scala
val n=3
repeat("hello", n)    // ERROR!
```

The error is:

```
1 |time(repeat("hello",100_000))
  |     ^^^^^^^^^^^^^^^^^^^^^^^
  |     Maximal number of successive inlines (32) exceeded,
  |     Maybe this is caused by a recursive inline method?
  |     You can use -Xmax-inlines to change the limit.
  | This location contains code that was inlined from rs$line$8:1
  | This location contains code that was inlined from rs$line$1:3
  |...
  | This location contains code that was inlined from rs$line$1:3
  | This location contains code that was inlined from rs$line$1:3
```

Note that the maximum number of successive inlines allowed defaults to 32. Even though it is configurable, after a certain point the gains from elimination of method invocations will be replaced by byte code bloat...

However, we can fix this error as follows:

```scala
inline val n = 3
repeat("hello", n)
```

Even if we're limited to 32, does it noticeably affect performance? Here's the same method, non-inlined:

```scala
def repeatNI(s: String, count: Int): String =   // "NI" for "not inlined"
  if count == 0 then ""
  else s + repeatNI(s, count-1)
```

Here's an inlined timer method:

```scala
def time[R](f: => R): R =
  val startNanos = java.lang.System.nanoTime
  val r = f
  val endNanos = java.lang.System.nanoTime
  println(s"${endNanos - startNanos}ns")
  r
```

Running on my relatively recent MacBook Pro 16" laptop (Intel, not an M1 ;^):

```
scala> time(repeat("hello",31))
4535ns
val res4: String = hellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohello

scala> time(repeatNI("hello",31))
12042ns
val res5: String = hellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohellohello
```

About 2.6 times faster, but really, this can't be trusted too much. If you run these line repeatedly, you'll see a lot of variance. We also have the JVM "Hot Spotting" things for us...

But wait, there's more! We can inline the conditional:

```scala
// src/script/scala/progscala3/meta/inline/ConditionalMatch.scala

inline def repeat2(s: String, count: Int): String =
  inline if count == 0 then ""
  else s + repeat2(s, count-1)

repeat2("hello", 3)    // Okay
val n = 3
repeat2("hello", n)    // ERROR!
```

Now, an error is reported first while compiling the conditional:

```
1 |repeat2("hello", n)    // ERROR!
  |^^^^^^^^^^^^^^^^^^^
  |Cannot reduce `inline if` because its condition is not a constant value: n.==(0)
  | This location contains code that was inlined from rs$line$18:2
```

What does it mean to inline the conditional? It means that the byte code inserted will correspond to either the true or false branch, but the conditional test and both branches will not be in the byte code. 

To be clear, when we call `repeat2("hello", 3)`, it's basically as if we wrote the following code instead, where I added parentheses `()` to show each expansion:

```scala
"hello" + ("hello" + ("hello" + ""))
```

Whereas for `repeat`, the inserted byte code would have the conditional expression inserted three times.

Perhaps you've done a similar exercise yourself on paper while learning how a recursive function works??

Let's try our timing test again:

```
scala> time(repeat("hello",31))
4455ns    # roughly the same as last time...

scala> time(repeat2("hello",31))
4399ns
```

Not much difference in this case. Again, there is lots of variance in the actual run times.

Finally, we can inline conditionals:

```scala
inline def repeat3(s: String, count: Int): String =
  inline count match
    case 0 => ""
    case _ => s + repeat3(s, count-1)
```

I'll let you play with this yourself. Would you expect the performance to be better, worse, or about the same compared to `repeat2`?

Don't abuse this feature. Besides being limited to calling `repeat` with compile-time constants, you'll create code bloat and may not always get performance improvements.

## A Non-trivial Example

Let's write a function that _optionally_ checks that an _invariant_ condition is preserved by a method call. If you've heard of [_Design by Contract_](https://en.wikipedia.org/wiki/Design_by_contract), this might be something you've thought about.

Basically, I want an easy way to test a condition before and after some operation, confirming that it is satisfied both times, but I also want to "compile it away" before I go to production. Oh, and I also want a pony...

This example also uses _quoting_ and _splicing_, the building blocks for macros in the new metaprogramming system. I explain this example in this [Scala 3 blog post](https://medium.com/scala-3/scala-3-macros-d63dd6811f89):

```scala
// src/main/scala/progscala3/meta/Invariant.scala
package progscala3.meta
import scala.quoted.*    

object invariant:
  // Compile-time constant to enable or disable checking
  inline val ignore = false    

  // Test the predicate before and after evaluating the block.
  inline def apply[T](
      inline predicate: => Boolean, message: => String = "")( 
      inline block: => T): T =
    // From what you now know, what does the compiler output for byte code?
    inline if !ignore then
      if !predicate then fail(predicate, message, block, "before")
      val result = block
      if !predicate then fail(predicate, message, block, "after")
      result
    else
      block

  // Use inline to insert the definition at compile time.
  // The splice ${...} is inserted in the byte code.
  inline private def fail[T](
      inline predicate: => Boolean,
      inline message: => String,
      inline block: => T,
      inline beforeAfter: String): Unit =
    ${ failImpl('predicate, 'message, 'block, 'beforeAfter) }   

  case class InvariantFailure(msg: String) extends RuntimeException(msg)

  // Note the quote '{...} and the argument types
  private def failImpl[T](
      predicate: Expr[Boolean], message: Expr[String],
      block: Expr[T], beforeAfter: Expr[String])(
      using Quotes): Expr[String] =
    '{ throw InvariantFailure(  
      s"""FAILURE! predicate "${${showExpr(predicate)}}" """
      + s"""failed ${$beforeAfter} evaluation of block:"""
      + s""" "${${showExpr(block)}}". Message = "${$message}". """)
    }

  private def showExpr[T](expr: Expr[T])(using Quotes): Expr[String] =
    val code: String = expr.show 
    Expr(code)
```

When `ignore` is `true`, the entire implementation of `apply` reduces to `block`! If `ignore` is `false`, then the following code is compiled:

```scala
      if !predicate then fail(predicate, message, block, "before")
      val result = block
      if !predicate then fail(predicate, message, block, "after")
      result
```

The benefit of using a macro implementation is that the `InvariantFailure` message string will contain the code for `predicate` and `block`, so it's easier to see what failed. See the [blog post](https://medium.com/scala-3/scala-3-macros-d63dd6811f89) for more details.

## Overrides

If we have time, let's discuss a few subtleties.

There's some subtle behavior with method definitions and overrides, where declaring the abstract method `inline` behaves differently than overriding "normal" methods, but making the overrides inline. First, the latter case:

```scala
// src/script/scala/progscala3/meta/inline/Overrides.scala

trait T:
  def m1: String
  def m2: String = m1

object O extends T:
  inline def m1 = "O.m1"
  override inline def m2 = m1 + " called from O.m2"

val t: T = O
assert(O.m1 == t.m1)
assert(O.m2 == t.m2)
```

Now notice what happens if the parent's abstract method is inline:

```scala
trait T2:
  inline def m: String

object O2 extends T2:
  inline def m: String = "O2.m"

val t2: T2 = O2
O2.m
t2.m       // ERROR
```

The last line prints:

```
1 |t2.m
  |^^^^
  |Deferred inline method m in trait T2 cannot be invoked
```

## Transparent Inline

This is non-obvious, but notice what happens if you add `transparent` as well as `inline`:

```scala
// src/script/scala/progscala3/meta/inline/Transparent.scala

open class C1
class C2 extends C1:
  def hello = "hello from C2"

transparent inline def make(b: Boolean): C1 = if b then C1() else C2()

val c1: C1 = make(true)                 // <1>
// c1.hello                             // C1.hello doesn't exist!
val c2: C2 = make(false)                // <2>
c2.hello                                // Allowed!
```

Even though `make` is declared to return a `C1`, the compiler actually knows that a `C2` instance is returned in the second case, so it allows us to assign the returned value to `c2`, of type `C2`! While convenient, this technically breaks the usual "contract" for method return types and assignments. Normally, it would be a compilation error for `c2` to be of type `C2`, so I'm not sure it's really good idea to use this feature, except in rare cases.


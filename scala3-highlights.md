---
layout: page
title: Scala 3 Highlights
tagline: Notes on what's coming in Scala 3.
include_social: true
---
{% include JB/setup %}

[CASE meetup, Nov 19, 2020](https://www.meetup.com/chicagoscala/events/274110140/)

* [@deanwampler](https://twitter.com/deanwampler)
* [dean@deanwampler.com](mailto:dean@deanwampler.com)

These are the notes for my talk at [CASE](https://www.meetup.com/chicagoscala/events/274110140/). Some of these code examples are in my [running series of blog posts on Scala 3](https://medium.com/scala-3). Most are adapted from the book with a few "borrowed" from the [Dotty documentation](https://dotty.epfl.ch/docs/index.html).

## For More Information

![Programming Scala 3rd Edition Cover](/assets/images/prog_scala_3ed_comp-quarter_size.jpg)

* [Programming Scala 3rd Edition](http://programming-scala.org/)
* [Code examples](https://github.com/deanwampler/programming-scala-book-code-examples) (warning, very few comments!!)
* [My blog on Scala 3](https://medium.com/scala-3)
* [EPFL's very good documentation on Scala 3](https://dotty.epfl.ch/). See in particular the:
    *  [Blog](https://dotty.epfl.ch/blog/index.html)
    *  [Docs](https://dotty.epfl.ch/docs/index.html).
* [Gitter Channel on Dotty](https://gitter.im/lampepfl/dotty). Essential!

## General Comments

Some features are transitional; you can mix old with new in 3.0, but 3.1, etc. will start deprecating and warning about older features.

To get started, an EPFL SBT plugin brings Dotty/Scala 3 support to SBT:

* [Getting started](https://dotty.epfl.ch/docs/usage/getting-started.html)
* [Dotty example project](https://github.com/lampepfl/dotty-example-project)

## New Syntax

[Blog post](https://medium.com/scala-3/scala-3-new-but-optional-syntax-855b48a4ca76)

You can now use significant indentation ("braceless"), like Python or Haskell, rather than curly braces. You can also mix and match, or use a compiler flag to force one or the other (see below).

### Types

[gist](https://gist.github.com/deanwampler/b277550a3a7f9a1e52192a9120a38153#file-scala2-3-types-scala)

```scala
// With braces
trait Monoid2[A] {
  def add(a1: A, a2: A): A
  def zero: A
}

// Without braces
trait Monoid3[A]:     // Notice the colon
  def add(a1: A, a2: A): A
  def zero: A
```

### Methods

[gist](https://gist.github.com/deanwampler/6ca52d2cb096d64750d0d1cf2eea276c#file-scala2-3-methods-scala)

```scala
def m2(s: String): String = {
  val result = s.toUpperCase
  println(s"output: $result")
  result
}

def m3(s: String): String =   // =, while Python uses : (confusing! ;)
  val result = s.toUpperCase
  println(s"output: $result")
  result
```

### Partial Functions

[gist](https://gist.github.com/deanwampler/7d5579103bcf23032f3aeb10bf4c8332#file-scala2-3-partial-functions-scala)

```scala
val o2:Option[Int] => Int = {
  case Some(i) => i
  case None => 0
}

val o3:Option[Int] => Int =
  case Some(i) => i
  case None => 0
```

### Match Expressions

[gist](https://gist.github.com/deanwampler/852ab4ea8487b9579980bc1ab9dc9b18#file-scala2-3-match-expressions-scala)

```scala
0 match {
  case 0 => "zero"
  case _ => "other value"
}

0 match
  case 0 => "zero"
  case _ => "other value"
```

### But "Custom Controls" Don't Work

Create a custom `loop` "control":

[gist](https://gist.github.com/deanwampler/6c7124e65102e4c203a47127fa329716#file-scala2-3-custom-controls-scala)

```scala
import scala.annotation.tailrec

@tailrec def loop(whileTrue: => Boolean)(f: => Unit): Unit =
  f
  if (whileTrue) loop(whileTrue)(f)

var i=5
loop(i > 0) {
  println(i)
  i -= 1
}

var j=5
loop(j > 0):       // ERROR
  println(j)
  j -= 1
```

### New Control Syntax

There are also new options for control syntax, but whether or not you use them is controlled by compiler flags: 

[gist](https://gist.github.com/deanwampler/98647681a702c13da41342bf51684915#file-scala2-3-controls-scala)

```scala
for (i <- 0 until 5) println(i)   // Original syntax
for i <- 0 until 5 do println(i)  // New syntax: no () and `yield/do`
for i <- 0 until 5 yield 2*i
for i <- 0 until 10
  if i%2 == 0
  ii = 2*i
yield ii

val i = 10
if (i < 10) println("yes")        // Original syntax
else println("no")
if i < 10 then println("yes")     // New syntax: no () and `then`
else println("no")
```

## Contextual Abstractions

We begin the migration aware from the implicit _mechanism_ to constructs that more clearly indicate the _intent_.

### Extension Methods Instead of Implicit Conversions.

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-i-3471676f2d23)

Remember the `ArrowAssoc` implicit conversion?? 

[gist](https://gist.github.com/deanwampler/bf7bd15283c12a9e2bbb1db37920c0a0#file-scala2-3-arrowassoc-scala)

```scala
implicit final class ArrowAssoc[A](private val self: A) extends AnyVal {
  @inline def -> [B](y: B): (A, B) = (self, y)
  @deprecated("Use `->` instead...", "2.13.0")  
  def →[B](y: B): (A, B) = ->(y)  // you did know this is deprecated, right??
}
```

It's much simpler and more direct to use an _extension method_. The following example shows how to define a `~>` method on any type `A`. The `@targetName` annotation defines the name generated in byte code for the method (but this is only visible to other languages, like Java, not Scala code!).

> **NOTE:** Use `alpha` instead of `targetName` for Scala 3.0.0-M1.

[gist](https://gist.github.com/deanwampler/56bbdbd3129a76f594f9ff9a2e7cb48a#file-scala2-3-arrowassocextensionmethod-scala)

```scala
import scala.annotation.targetName

extension [A,B] (a: A):
  // @targetName called @alpha before 3.0.0-M2:
  @targetName("arrow2") def ~>(b: B): (A, B) = (a, b) 
```

Extension methods are part of a new syntax for _type classes_, which I'll cover in a moment.

> **NOTE:** If you also defined an `arrow2` method, it would collide with the target name given for `~>`.

There are still cases where implicit conversions are useful, e.g., allow users to specify `Double` values that are converted to domain types, like `Dollars` and `Percentage`:

[gist](https://gist.github.com/deanwampler/322591b0a814054458eb8a0199c1dbe5#file-scala2-3-conversion-scala).

```scala
import scala.language.implicitConversions

case class Dollars(amount: Double):
  override def toString = f"$$$amount%.2f"
case class Percentage(amount: Double):
  override def toString = f"${(amount*100.0)}%.2f%%" 
case class Salary(gross: Dollars, taxes: Percentage):
  def net: Dollars = Dollars(gross.amount * (1.0 - taxes.amount))
      
given Conversion[Double,Dollars] = d => Dollars(d)

given d2P as Conversion[Double,Percentage] = d => Percentage(d) 

val salary = Salary(100_000.0, 0.20)   // Add _ to number literals for clarity!
println(s"salary: $salary. Net pay: ${salary.net}") 
```

Note the new `given` syntax. This replaces `implicit val/def`, in general. 

Note the name that is synthesized for the first _given instance_ if you don't explicitly provide a name, `given_Conversion_Double_Dollars`. 

Note that `as` keyword when the second given instance is named.

By the way, the first definition is shorthand for this:

```scala
given Conversion[Double,Dollars]:
  def apply(d: Double): Dollars = Dollars(d)
```

Even when a given is anonymous, you can use `summon[Conversion[Double,Dollars]]` to bind to it. The new method `summon` is identical to `implicitly`; a new name for a newly-branded concept:

```scala
scala> summon[Conversion[Double,Dollars]]
val res68: Conversion[Double, Dollars] = <function1>
```

Maybe you already noticed that `Conversion` looks shockingly similar to `A => B`.

### New Type Classes

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-ii-35174c7f1baa)

The syntax combines traits (to define the abstraction), regular and _extension methods_, and _given instances_ (for type class instances): 

[gist](https://gist.github.com/deanwampler/6c1496da43bf338019beee2c5ea70ce4#file-scala2-3-monoidtypeclassdef-scala)

```scala
trait Semigroup[T]:
  extension (t: T):
    def combine(other: T): T
    def <+>(other: T): T = t.combine(other)

trait Monoid[T] extends Semigroup[T]:
  def unit: T
```

Notice which members are extensions and which ones aren't! The extension methods will be _instance_ members and the others will be the equivalent of _companion object_ members; we only need one `unit` per type `T`.

Create two _monoid instances_: 

[gist](https://gist.github.com/deanwampler/3060fb7cabb9f183795a2ce919f572cd#file-scala2-3-monoidtypeclassexamples-scala)

```scala
given StringMonoid as Monoid[String]:
  def unit: String = ""
  extension (s: String) def combine(other: String): String = s + other

given IntMonoid as Monoid[Int]:
  def unit: Int = 0
  extension (i: Int) def combine(other: Int): Int = i + other
```

Try them out: 

[gist](https://gist.github.com/deanwampler/42337a39370ac06117c11c9898607d0a#file-scala2-3-monoidtypeclassusage-scala)

```scala
"2" <+> ("3" <+> "4")             // "234"  Associativity works.
("2" <+> "3") <+> "4"             // "234"
StringMonoid.unit <+> "2"         // "2"
"2" <+> StringMonoid.unit         // "2"

2 <+> (3 <+> 4)                   // 9
(2 <+> 3) <+> 4                   // 9
IntMonoid.unit <+> 2              // 2
2 <+> IntMonoid.unit              // 2
```

We can actual define the monoid instance for all `T` for which `Numeric[T]`
exists: 

[gist](https://gist.github.com/deanwampler/bf09427538c38041c65ba028e935312f#file-scala2-3-monoidtypeclassnumeric-scala)

```scala
given NumericMonoid[T](using num: Numeric[T]) as Monoid[T]:
  def unit: T = num.zero
  extension (t: T) def combine(other: T): T = num.plus(t, other)

2.2 <+> (3.3 <+> 4.4)             // 9.9
(2.2 <+> 3.3) <+> 4.4             // 9.9

BigDecimal(3.14) <+> NumericMonoid.unit
NumericMonoid[BigDecimal].unit  <+> BigDecimal(3.14)
```

Now we see our first example of a _using clause_, the successor to an _implicit parameter list_.

### Using Clauses

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-iii-6b7be628a7de)

We just saw a _using clause_. They can be anonymous, too. Here's an (unnecessary ;) wrapper around `Seq` for sorting them:

[gist](https://gist.github.com/deanwampler/c8f95c00542910674b061684a353c81f#file-scala2-3-usingclauses-defs-scala)

```scala
case class SortableSeq[A](seq: Seq[A]):                              // <1>
  def sortByImplicits[B](transform: A => B)(implicit o: Ordering[B]): SortableSeq[A] =
    new SortableSeq(seq.sortBy(transform)(o))

  def sortBy1a[B](transform: A => B)(using o: Ordering[B]): SortableSeq[A] =
    new SortableSeq(seq.sortBy(transform)(o))

  def sortBy1b[B](transform: A => B)(using Ordering[B]): SortableSeq[A] =
    new SortableSeq(seq.sortBy(transform)(summon[Ordering[B]]))

  def sortBy2[B : Ordering](transform: A => B): SortableSeq[A] =
    new SortableSeq(seq.sortBy(transform)(summon[Ordering[B]]))
```

I passed the `implicit/given` values explicitly to `Seq.sortBy` for illustration purposes, but of course I could have passed them implicitly (usingly?).

### Given Imports

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-iii-6b7be628a7de)

To allow use of `_` for imports, but _not_ pull in all givens when you don't want them:

[gist](https://gist.github.com/deanwampler/0a3685df0763b9cd03f8f6fe4e5972e6#file-scala2-3-givenimports-scala)

```scala
object O1:
  val name = "O1"
  def m(s: String) = s"$s, hello from $name"
  class C1
  class C2
  given c1 as C1
  given c2 as C2

import O1._             // Import everything EXCEPT the givens, c1 and c2
import O1.given         // Import ONLY the givens (of type C1 and C2)
import O1.{given, _}    // Import everything, givens and non-givens in O1
import O1.{given C1}    // Import just the given of type C1
import O1.c1            // Import just the given c1 of type C1
```

In Scala 3.0, `_` will still import everything, for backwards compatibility, but Scala 3.1 will begin transitioning to this behavior.

> **NOTE:** "non-givens" should be called _takes_ IMHO... If you grew up with the King James Bible (1611) in your Baptist church like I did, they would be `giveth` and `taketh`...

## Infix Operator Notation

Because people abuse operator notation, Scala is migrating towards disallowing it, by default, unless:

1. The method name uses only "operator characters".
2. The method is annotated with `@scala.annotation.infix`.
3. Usage is followed by a curly brace.
4. Back ticks are used:

In our previous example:

```scala
scala> "2" combine ("3" combine "4")
     |
1 |"2" combine ("3" combine "4")
  |                 ^^^^^^^
  |Alphanumeric method combine is not declared @infix; it should not be used as infix operator.
  |The operation can be rewritten automatically to `combine` under -deprecation -rewrite.
  |Or rewrite to method syntax .combine(...) manually.
1 |"2" combine ("3" combine "4")
  |    ^^^^^^^
  |Alphanumeric method combine is not declared @infix; it should not be used as infix operator.
  |The operation can be rewritten automatically to `combine` under -deprecation -rewrite.
  |Or rewrite to method syntax .combine(...) manually.

scala> "2" combine { "3" combine { "4" } }
val res0: String = 234

scala> "2" `combine` ("3" `combine` "4")
val res1: String = 234
```

Or, annotate `combine` with `@infix`, then you can use `"2" combine ("3" combine "4")`: 

[gist](https://gist.github.com/deanwampler/5f7eac8bfe678816ff4b6863ee81522a)

```scala
import scala.annotation.infix

trait Semigroup[T]:
  extension (t: T):
    @infix def combine(other: T): T    // @infix allows "foo combine bar".
    def <+>(other: T): T = t.combine(other)

trait Monoid[T] extends Semigroup[T]:
  def unit: T
```

Now redefine the previous monoid instances with this new definition. 

> **NOTE:** the addition of `@infix` for each concrete `combine` definition is required!

```scala
given StringMonoid as Monoid[String]:
  def unit: String = ""
  extension (s: String) @infix def combine(other: String): String = s + other

given IntMonoid as Monoid[Int]:
  def unit: Int = 0
  extension (i: Int) @infix def combine(other: Int): Int = i + other
```

Now `combine` can be used with infix notation as an alternative to `<+>`:

```scala
scala> "2" combine ("3" combine "4")
val res2: String = 234
```

## Easier Enums

I can never remember the Scala 2 syntax for enums. Now I have an even easier syntax to forget!

Adapted from [Dotty docs](https://dotty.epfl.ch/docs/reference/enums/enums.html):

```scala
enum SimpleColor:
  case Red, Green, Blue  // These effectively `extends SimpleColor`

enum FancyColor(val rgb: Int):
  case Red   extends FancyColor(0xFF0000)
  case Green extends FancyColor(0x00FF00)
  case Blue  extends FancyColor(0x0000FF)
```

A new way to define ADTs!

```scala
enum Option[+T]:
  case Some(x: T)
  case None
```

> **NOTE:** As commented by Seth Tisue during the meeting, Scala 3 uses the Scala 2 library unchanged, so types like these won't be changed to enums until some future release.

## Type Madness!! (Or Sanity...)

### Opaque Type Aliases

Much better than _value classes_ (which were prone to unexpected boxing, etc.). Example adapted from the [Dotty docs](https://dotty.epfl.ch/docs/reference/other-new-features/opaques.html):

```scala
object Logarithms:

  opaque type Logarithm = Double

  object Logarithm:
    // These are the two ways to lift to the Logarithm type
    def apply(d: Double): Logarithm = math.log(d)
    def safe(d: Double): Option[Logarithm] =
      if (d > 0.0) Some(math.log(d)) else None

  // Extension methods define the instance methods for opaque types
  extension (x: Logarithm):
    def toDouble: Double = math.exp(x)
    def + (y: Logarithm): Logarithm = Logarithm(math.exp(x) + math.exp(y))
    def * (y: Logarithm): Logarithm = x + y
```

### Open Classes

No more ad-hoc extensions of concrete types (unless you want 'em):

```scala
// File Writer.scala
package p

open class Writer[T]:   // Fully concrete type, but extension is allowed.
  /** Sends to stdout, can be overridden */
  def send(x: T) = println(x)

  /** Sends all arguments using `send` */
  def sendAll(xs: T*) = xs.foreach(send)

// File EncryptedWriter.scala
package p

class EncryptedWriter[T: Encryptable] extends Writer[T]:
  override def send(x: T) = super.send(encrypt(x))
```

(Wouldn't make sense for abstract classes and traits, which must be extended to become concrete...)

What if a type isn't open, but you want to subclass it in a test to stub methods (i.e., make a _test double_)? Use `import scala.language.adhocExtensions` in the test file. This is the advantage over declaring the type `final`, which provides no mechanism for this sort of "exceptional" extension.

### Intersection and Union Types

#### Intersection Types

_Intersection types_ work much like `with` for trait mixins:

```scala
trait Resettable:
  override def toString:String = "Resettable:"+super.toString
  def reset(): Unit

trait Growable[T]:
  override def toString:String = "Growable:"+super.toString
  def add(t: T): Unit

def f(x: Resettable & Growable[String]): String = 
  x.reset()
  x.add("first")
  x.add("second")
  x.toString
```

Note how the argument `x` for `f` is declared. 

I find it a little confusing, but you have to actually instantiate these types using `with`:

```scala
case class RG(var str: String = "") extends Resettable with Growable[String]:
  override def toString:String = s"RG(str=$str):"+super.toString
  def reset(): Unit = str = ""
  def add(s: String): Unit = str = str + s

case class GR(var str: String = "") extends Growable[String] with Resettable:
  override def toString:String = s"GR(str=$str):"+super.toString
  def reset(): Unit = str = ""
  def add(s: String): Unit = str = str + s
```

I declared two types, one with `Resettable & Growable[String]` and the other with `Growable[String] & Resettable`. _They are considered the same type by `f`_. In other words, they _commute_, just like the intersection operation in set theory. In contrast, Scala 2 treated `Resettable with Growable[String]` and `Growable[String] with Resettable` as different types.

Let's try them!

```scala
scala> val rg = new RG
     | val gr = new GR
     |
val rg: RG = RG(str=):Growable:Resettable:rs$line$16$RG@173b3581
val gr: GR = GR(str=):Resettable:Growable:rs$line$16$GR@367164f5

scala> f(rg)     // Both can be passed to `f`, showing commutativity.
     | f(gr)     // But toString shows different ordering of the "supers"!
     |
val res0: String = RG(str=firstsecond):Growable:Resettable:rs$line$16$RG@697f3db1
val res1: String = GR(str=firstsecond):Resettable:Growable:rs$line$16$GR@b3cd3f1e

```

The `:rs$line$...` comes from calling `super.toString` on the object wrapping the code in the REPL! Just ignore it...

_Linearization_ is still used to decide which of an overridden method gets called when you use `super.method(...)`. In this example, `super.toString` returns different results for `Resettable & Growable[String]` vs. `Growable[String] & Resettable`, even though those two types are considered equivalent from the type-checking perspective! Note which trait's `toString` got called first for each case. (Hint: _linearization_ is basically _right to left_ ordering.)

> **WARNING:** While `A & B` and `B & A` are considered equivalent types, the behaviors of overridden methods may be different, due to _linearization_.

#### Union Types

_Union types_ could replace `Either[A,B]`, but they aren't limited to two nested types. Consider this pseudo DB query example:

```scala
case class User(name: String, password: String)

def getUser(id: String, dbc: DBConnection): String | User | Seq[User] = 
  try
    val results = dbc.query(s"SELECT * FROM users WHERE id = $id")
    results.size match 
      case 0 => s"No records found for id = $id"
      case 1 => results.head.as[User]
      case _ => results.map(_.as[User])
  catch 
    case dbe: DBException => dbe.getMessage

getUser(dbc) match
  case message: String => error(message)
  case User(name, password) => ...
  case seq: Seq[User] => ...
```

Note how pattern matching is necessary here. What you give up are the useful operations on `Either`, like `map`, `flatMap`, etc.

Speaking of match expressions...

### Match Types

This can be a bit "quirky", but it's a cool feature. (Example adapted from the [Dotty docs](https://dotty.epfl.ch/docs/reference/new-types/match-types.html)):

```scala
type Elem[X] = X match
  case String => Char
  case Array[t] => t
  case IterableOnce[t] => t
  case ? => X

val char: Elem[String] = 'c'
val doub: Elem[List[Double]] = 1.0
val tupl: Elem[Option[(Int,Double)]] = (1, 2.0)

val bad1: Elem[List[Double]] = "1.0"        // ERROR
val bad2: Elem[List[Double]] = (1.0, 2.0)   // ERROR

summon[Elem[String] =:= Char]  // ...: Char =:= Char = generalized constraint
summon[Elem[List[Int]] =:= Int]
summon[Elem[Nil.type] =:= Nothing]
summon[Elem[Array[Float]] =:= Float]
summon[Elem[Option[String]] =:= String]
summon[Elem[Some[String]] =:= String]
summon[Elem[None.type] =:= Nothing]
summon[Elem[Float] =:= Float]
summon[Elem[Option[List[Long]]] =:= Long]   // ERROR
```

## Migration

The book's code examples use the flag `-source 3.1` to force deprecation warnings for older constructs. The default, `-source 3.0`, is more forgiving.

Flags to control syntax preferences:

* `-indent`: Allow significant indentation.
* `-noindent`: Require classical {...} syntax, indentation is not significant.
* `-new-syntax`: Require `then` in conditional expressions.
* `-old-syntax`: Require `(...)` around conditions.

Flags to help migration:

* `-language:Scala2`: Compile Scala 2 code, highlight what needs updating.
* `-migration`: Emit warning and location for migration issues from Scala 2.
* `-rewrite`: Attempt to fix code automatically.

## Things I Didn't Cover

* Trait parameters
* Completely new macro system
* `@mixin` traits
* `export` clauses
* type lambdas
* kind polymorphism
* dependent function types (we've had dependent _method_ types)
* New types like `Tuple`
* Explicit `null`s
* Safe initialization
* `@main` methods
* No more 22-arity limits on tuples and functions
* ... and lots of smaller refinements

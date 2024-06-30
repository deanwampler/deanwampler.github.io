---
layout: page
title: Scala 3 Highlights
tagline: What's new in Scala 3?
include_social: true
---
{% include JB/setup %}

* Created, Nov 19, 2020. Latest update: July 2, 2021
* [@deanwampler](https://twitter.com/deanwampler)
* [dean@deanwampler.com](mailto:dean@deanwampler.com)

This is a concise summary of many of the changes in Scala 3. I used these notes for talks at [The Chicago-Area Scala Enthusiasts (CASE)](https://www.meetup.com/chicagoscala/events/274110140/), Nov. 19, 2020, [Scala Love in the City](https://inthecity.scala.love/), Feb. 13, 2021, and at the [Philly Area Scala Enthusiasts (PHASE)](https://www.meetup.com/scala-phase/events/277164777/), April 21, 2021. 

Some of the code examples are in my [Scala 3 blog](https://medium.com/scala-3). Most are adapted from the [Code examples](https://github.com/deanwampler/programming-scala-book-code-examples) for [_Programming Scala, Third Edition_](http://programming-scala.org/) with a few "borrowed" from the [_Dotty_ documentation](https://dotty.epfl.ch/docs/index.html) (_Dotty_ is the project that evolved into Scala 3).

## For More Information

* My book, _Programming Scala, Third Edition_:
  * [Book website](http://programming-scala.org/){:target="prog-scala"}
  * [Code examples](https://github.com/deanwampler/programming-scala-book-code-examples){:target="code"} (warning, not many comments!!)
* [My Scala 3 blog](https://medium.com/scala-3){:target="blog"}
* [Scala 3 home page](https://www.scala-lang.org/download/scala3.html){:target="scala3"} which links to the thorough [Scala documentation](https://dotty.epfl.ch/){:target="docs"}. See in particular the following:
  * [EPFL Blog](https://dotty.epfl.ch/blog/index.html){:target="epfl-blog"}
  * [EPFL docs](https://dotty.epfl.ch/docs/index.html){:target="epfl-docs"}

## General Comments

Some features are transitional to make upgrading easier. Scala 3.0 lets you mix older Scala 2 features with their Scala 3 replacements, but subsequent releases will start deprecating and eventually removing the older features. I'll discuss examples as we go.

To get started, SBT 1.5+ supports Scala 3:

* [Getting started](https://dotty.epfl.ch/docs/usage/getting-started.html)
* [Dotty example project](https://github.com/lampepfl/dotty-example-project)

## New Syntax

[Blog post](https://medium.com/scala-3/scala-3-new-but-optional-syntax-855b48a4ca76)

You can now use significant indentation ("braceless"), like Python or Haskell, rather than curly braces. You can also mix and match, or use a compiler flag to force one or the other (see below).

> **Note:** Additional changes to this syntax are coming in Scala 3.3. ([Blog Post](https://medium.com/scala-3/scala-3-even-fewer-braces-3890e4f9bc55))

### Import and Export Statements

[blog post](https://medium.com/scala-3/scala-3-the-they-are-achanging-32b1ebb90fa2)

Look at these examples of `import` statements:

<script src="https://gist.github.com/deanwampler/7205aa985d9b439530c76783c69bad9d.js"></script>

Scala 3 replaces `_` with `*` as the wild card for imports, which is what most other languages use. When you alias a type, you now use the new `as` keyword instead of `=>`. Hence, `=>` is now used solely for function literals.

What if you want to import a multiplication method named `*`? Use back ticks:

```scala
import Matrix.`*`
```

Scala 3 also introduces the concept of _exporting_ members of a type. See the [blog post](https://medium.com/scala-3/scala-3-the-they-are-achanging-32b1ebb90fa2) for details.

### Types

<script src="https://gist.github.com/deanwampler/b277550a3a7f9a1e52192a9120a38153.js"></script>

### Methods

<script src="https://gist.github.com/deanwampler/6ca52d2cb096d64750d0d1cf2eea276c.js"></script>

### Partial Functions

[gist](https://gist.github.com/deanwampler/7d5579103bcf23032f3aeb10bf4c8332#file-scala2-3-partial-functions-scala)
<script src="https://gist.github.com/deanwampler/7d5579103bcf23032f3aeb10bf4c8332.js"></script>

### Match Expressions

<script src="https://gist.github.com/deanwampler/852ab4ea8487b9579980bc1ab9dc9b18.js"></script>

### But "Custom Controls" Don't Work (yet...)

Create a custom `loop` "control":

<script src="https://gist.github.com/deanwampler/6c7124e65102e4c203a47127fa329716.js"></script>

There is an experimental compiler flag `-language:experimental.fewerBraces` that enables this to work, but there are details to work out before it's considered fully supported (Scala 3.1??).

### New Control Syntax

There are also new options for control syntax, but whether or not you use them is controlled by compiler flags: 

<script src="https://gist.github.com/deanwampler/98647681a702c13da41342bf51684915.js"></script>

## Even Less Use of `new`

[Blog post](https://medium.com/scala-3/scala-3-universal-apply-methods-2c583800b66a)

We don't need to use `new` when creating instances of case classes, because the compiler generates a companion object with an necessary `apply` method. For other types, like the abstract `Seq` type, one or more custom `apply` methods exist in a companion object, e.g., `Seq(1,2,3)`. In Scala 2, we had to use `new` for all other kinds of classes, especially those from Java libraries.

Scala 3 extends the case class mechanism to all concrete types, even those from Java and Scala 2 libraries. The compiler creates a _synthetic object_ with an `apply` method for each constructor. (Note: for companion objects, only the _primary_ constructor gets an `apply` method automatically).

## Contextual Abstractions

We begin the migration aware from the implicit _mechanism_ to constructs that more clearly indicate the _intent_.

### Extension Methods Instead of Implicit Conversions.

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-i-3471676f2d23)

Remember the `ArrowAssoc` implicit conversion?? 

<script src="https://gist.github.com/deanwampler/bf7bd15283c12a9e2bbb1db37920c0a0.js"></script>

It's much simpler and more direct to use an _extension method_. The following example shows how to define a `~>` method on any type `A`. The `@targetName` annotation defines the name generated in byte code for the method (but this is only visible to other languages, like Java, not Scala code!).

<script src="https://gist.github.com/deanwampler/56bbdbd3129a76f594f9ff9a2e7cb48a.js"></script>

Extension methods are part of a new syntax for _type classes_, which I'll cover in a moment.

> **NOTE:** If you also defined an `arrow2` method, it would collide with the target name given for `~>`.

There are still cases where implicit conversions are useful, e.g., allow users to specify `Double` values that are converted to domain types, like `Dollars` and `Percentage`:

<script src="https://gist.github.com/deanwampler/322591b0a814054458eb8a0199c1dbe5.js"></script>

Note the new `given` syntax. This replaces `implicit val/def`, in general. 

Note the name that is synthesized for the first _given instance_ if you don't explicitly provide a name, `given_Conversion_Double_Dollars`. 

The second given instance is named `d2P`. The declaration looks similar to a typical `val` declaration.

By the way, the first definition is shorthand for this:

```scala
given Conversion[Double,Dollars] with
  def apply(d: Double): Dollars = Dollars(d)
```

Even when a given is anonymous, you can use `summon[Conversion[Double,Dollars]]` to bind to it. The new method `summon` is identical to `implicitly`; a new name for a newly-branded concept:

```scala
scala> summon[Conversion[Double,Dollars]]
val 0: Conversion[Double, Dollars] = <function1>
```

Maybe you already noticed that `Conversion` looks shockingly similar to `A => B`.

### New Type Classes

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-ii-35174c7f1baa)

The syntax combines traits (to define the abstraction), regular and _extension methods_, and _given instances_ (for type class instances): 

<script src="https://gist.github.com/deanwampler/6c1496da43bf338019beee2c5ea70ce4.js"></script>

Notice which members are extensions and which ones aren't! The extension methods will be _instance_ members and the others will be the equivalent of _companion object_ members; we only need one `unit` per type `T`. (The fact they are split across two different types is incidental to how I defined them. All could be defined in the same type.)

Create two _monoid instances_: 

<script src="https://gist.github.com/deanwampler/3060fb7cabb9f183795a2ce919f572cd.js"></script>

Try them out: 

<script src="https://gist.github.com/deanwampler/42337a39370ac06117c11c9898607d0a.js"></script>

We can actual define the monoid instance for all `T` for which `Numeric[T]`
exists: 

<script src="https://gist.github.com/deanwampler/e806a3301380d5d70c3247264afb076a.js"></script>

Now we see our first example of a _using clause_, the successor to an _implicit parameter list_.

Finally, like the `Conversion[Double,Dollars]` example above, `given`s will often be declared anonymous:

<script src="https://gist.github.com/deanwampler/5d176eeb8f886f8c4fcf2c6dc8edab81.js"></script>

Note that we use `summon` to retrieve the `Numeric[T]` object. Here, the anonymous `given` is less convenient than a named `given` when we need the `unit` value.

The definition of the anonymous `given` may be a little hard to parse. Go back to the previous named definition and just remove the name!

### Using Clauses

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-iii-6b7be628a7de)

We just saw a _using clause_. They can be anonymous, too. Here's an (unnecessary ;) wrapper around `Seq` for sorting them:

<script src="https://gist.github.com/deanwampler/c8f95c00542910674b061684a353c81f.js"></script>

I passed the `implicit/given` values explicitly to `Seq.sortBy` for illustration purposes, but of course I could have passed them implicitly (usingly?). The term _context parameters_ (or arguments) is used for these parameters.

Note that the `using` keyword is now required when you pass an explicit argument (although optional in the 3.0 release, to ease the transition). Requiring the keyword when you pass an explicit argument disambiguates which clauses are using clauses and which aren't. This permits more flexible definitions like the following:

<script src="https://gist.github.com/deanwampler/ad16826c18aadd4d74bc7f9603f1e71d.js"></script>

#### By-Name Context Parameters

In general, by-name parameters are great for deferred evaluation, such as passing expressions that should be evaluated inside a method, not before calling the method. This works for context parameters, too, as in this sketch of a database access API:

<script src="https://gist.github.com/deanwampler/6042d6486474d27b43d5add8de10fa3b.js"></script>

#### Context Functions

_Context functions_ are functions with context parameters only. Scala 3 introduces a new context function type for them, indicated by `?=> T`. Distinguishing context functions from regular functions is useful because of how they are invoked. 

Consider this example that wraps `Future`s:

<script src="https://gist.github.com/deanwampler/846425a8d4324db47b4b52770c9a6af3.js"></script>

`Executable[T]` is a type alias for a context function that takes an `ExecutionContext` and returns a `T`. 

Note the syntax for `FutureCF.apply` compared to `Future.apply`, shown in the comment. It's a simpler syntax than having a using clause, but it can be a bit confusing that a `Future(...)` is returned, not an `Executable`.

Notice that we use `FutureCF.apply` just like `Future.apply`, either using the `given` `global` value or providing it explicitly. So, how do we get the `Exetuable[T]` returned when the body of `FutureCF.apply` is just `Future(...)`?

1. `Executable(Future(sleepN(1.second)))` is returned,
2. which is the same as `(given ExecutionContext) ?=> Future(sleepN(1.sec
ond))` (from the type alias for `Executable`),
3. which is invoked to produce `Future(sleepN(1.second))(using global)`,
4. which is invoked to return the `Future`.

You can also define methods that take context functions as arguments. Study this [Dotty documentation](https://dotty.epfl.ch/docs/reference/contextual/context-functions.html) for more examples and an explanation of how the compiler handles these definitions. 

### Given Imports

[Blog post](https://medium.com/scala-3/scala-3-contextual-abstractions-part-iii-6b7be628a7de)

To allow use of `*` for imports, but _not_ pull in all givens when you don't want them:

<script src="https://gist.github.com/deanwampler/0a3685df0763b9cd03f8f6fe4e5972e6.js"></script>

In Scala 3.0, `*` will still import everything, for backwards compatibility, but Scala 3.1 will begin transitioning to this behavior.

> **NOTE:** "non-givens" should be called _takes_ IMHO... If you grew up with the King James Bible (1611) in your Baptist church like I did, they would be `giveth` and `taketh`...

### Alias Givens

Consider the following definitions, which sort of look similar to our previous definitions of _Monoids_, but in fact they are different.

<script src="https://gist.github.com/deanwampler/2ed5c04f95b2356275d82f6312b574a0.js"></script>

Note the definitions the REPL prints for the two givens, a method for the given with a type parameter `T` and a `lazy val` for the `StringMonoid`.

That explains the "Initializing ..." messages. _Every time_ we use the `<+>` method for numeric values, the method `NumericMonoid2[T]` is called, instantiating a new monoid instance. For `StringMonoid`, this only happens once, like for other lazy values.

### Type Class Derivation

For some type classes, why do we have to write the boilerplate for givens. Can the compiler infer the definition for us? That's the goal of _type class derivation_, which Scala 3 supports.

There is a built-in, derivable type class called `CanEqual`, which is used with a language feature called _strict equality_ to make equality checking more restrictive, causing the compiler to reject obvious false comparisons between objects of different types that can never be equal. Recall that Scala 2 follows Java conventions that equals is declared `def equals(other: AnyRef): Boolean` (for reference types).

In the following example, we enable the language feature just for this file, then declare an `enum` for `Tree`s, which derives `CanEqual`. Finally, we check what's allowed. Note that attempting to compare instances of `Tree[Int]` with `Tree[String]` is rejected at compile time:

<script src="https://gist.github.com/deanwampler/9372fceaf9247f3064f1d691174c8b0b.js"></script>

See the [Dotty documentation](http://dotty.epfl.ch/docs/reference/contextual/derivation.html) for details about how `CanEqual` is implemented and how to implement your own derivable type classes. (The new `enum` syntax is discussed below.)

Finally, for this particular example, recall that normally we're allowed to compare any `AnyRef` to another `AnyRef`, even if they will never be equal. This is _universal equality_. The `CanEqual` type class supports _multiversal equality_, where types are grouped in such a way that comparison of instances is only allowed for types within the same group. That doesn't necessarily mean the same exact type. In the example, we can compare a `Branch[T]` and a `Leaf[T]` for the same `T`, but not compare instances for different `T`s.

## Infix Operator Notation

[blog post](https://medium.com/scala-3/scala-3-infix-operator-notation-26bd7c13d0a3)

Because people abuse operator notation, Scala is migrating towards disallowing it, by default, unless:

1. The method name uses only "operator characters".
2. The method is declared `infix`.
3. The argument is wrapped in curly braces.
4. Back ticks are used:

In our previous example:

```scala
scala> "2" combine ("3" combine "4")
     |
1 |"2" combine ("3" combine "4")
  |    ^^^^^^^
  |Alphanumeric method combine is not declared `infix`; it should not be used as infix operator.
  |The operation can be rewritten automatically to `combine` under -deprecation -rewrite.
  |Or rewrite to method syntax .combine(...) manually.
1 |"2" combine ("3" combine "4")
  |                 ^^^^^^^
  | (same error message)

scala> "2" combine { "3" combine { "4" } }
val res0: String = 234

scala> "2" `combine` ("3" `combine` "4")
val res1: String = 234
```

Or, declare `combine` with `infix`, then you can use `"2" combine ("3" combine "4")`: 

<script src="https://gist.github.com/deanwampler/5f7eac8bfe678816ff4b6863ee81522a.js"></script>

> **Note:** I had to redefine the previous monoid instances with this new definition to add `infix` to each concrete definition of `combine`. 

Now `combine` can be used with infix notation as an alternative to `<+>`:

```scala
scala> "2" combine ("3" combine "4")
val res2: String = 234
```

## Easier Enums

[blog post](https://medium.com/scala-3/scala-3-well-designed-object-oriented-type-hierarchies-a87d5f2baf26)

I can never remember the Scala 2 syntax for enums. Now I have an even easier syntax to forget!

Adapted from [Dotty docs](https://dotty.epfl.ch/docs/reference/enums/enums.html):

<script src="https://gist.github.com/deanwampler/74a774084b4da58a16882ebc2a803805.js"></script>

> **NOTE:** As commented by Seth Tisue during the CASE meeting, Scala 3 uses the Scala 2 library unchanged, so types like `Option` won't be changed to enums until some future release.

## Type Madness!! (Or Sanity...)

### Opaque Type Aliases

[Blog post](https://medium.com/scala-3/opaque-type-aliases-and-open-classes-13076a6c07e4)

_Opaque type aliases_ have advantages and disadvantages compared to _value classes_. Example adapted from the [Dotty docs](https://dotty.epfl.ch/docs/reference/other-new-features/opaques.html):

<script src="https://gist.github.com/deanwampler/67d3e15a39d6ffb8ca686f14a7bb581e.js"></script>

In action:

<script src="https://gist.github.com/deanwampler/dbe425da8dc056f8f7a99d3364e1a3fd.js"></script>

Value classes still have a few advantages. They are real classes, so you can customize the `equals` and `toString` methods for them and you can pattern match on them (although this causes boxing to be required). Opaque type aliases don’t provide these benefits, but they prevent occurrences of boxing. Finally, the JDK will eventually have its own form of value classes (the Valhalla project), in which case a Scala representation will be desirable.

### Open Classes

[blog post](https://medium.com/scala-3/opaque-type-aliases-and-open-classes-13076a6c07e4)

No more ad-hoc extensions of concrete types (unless you want 'em):

<script src="https://gist.github.com/deanwampler/5fa2f284baf2425a853bd10341e1fb51.js"></script>

(Wouldn't make sense for abstract classes and traits, which must be extended to become concrete...)

What if a type isn't open, but you want to subclass it in a test to stub methods (i.e., make a _test double_)? Use `import scala.language.adhocExtensions` in the test file. This is the advantage over declaring the type `final`, which provides no mechanism for this sort of "exceptional" extension.

### Intersection and Union Types

[blog post](https://medium.com/scala-3/intersection-and-union-types-860665b785c1) (**Note:** I used different examples in the post than the ones below. The post also goes into more details about algebraic properties of these types.)

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

Note that we got `Growable:Resettable` vs. `Resettable:Growable`.

(The `:rs$line$...` comes from calling `super.toString` on the object wrapping the code in the REPL! Just ignore it...)

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

getUser("1234", myDBConnection) match
  case message: String => println(s"ERROR: $message")
  case User(name, password) => println("Hello user: $name")
  case seq: Seq[User] => println ("Hello users: $seq")
```

Note how pattern matching is necessary to determine what was returned from `getUser`. Compared to `Either`, you give up the useful operations like `map`, `flatMap`, etc. as alternatives to pattern matching like this.

Speaking of match expressions...

### Match Types

[blog post](https://medium.com/scala-3/scala-3-dependent-types-part-ii-e7fc04dbfb08)

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

The last one fails because our match type doesn't handle nesting beyond one level. This is possible; the type can be recursive!

### Matchable

[blog post](https://medium.com/scala-3/scala-3-safer-pattern-matching-with-matchable-f0396430ded6)

Scala 3 introduces a new trait called `scala.Matchable`. It is used to fix some loopholes in pattern matching. It has a lot of implications for your code. See my [blog post](https://medium.com/scala-3/scala-3-safer-pattern-matching-with-matchable-f0396430ded6) for details.

### Type Lambdas

[blog post](https://medium.com/scala-3/scala-3-type-lambdas-polymorphic-function-types-and-dependent-function-types-2a6eabef896d)

_Type lambdas_ are the type-level analog of "value lambdas", i.e., _functions_. Scala 3 intoduces a syntax similar to function syntax, e.g., `type Work = [T] =>> Either[String,T]`. Note the similarity of `=>>` to function literal "arrows" `=>`.

This example suggests one important use for type lambdas; when you have a type with two type parameters that you want to use in a context where a single type parameter is expected, but one of the type parameters can be fixed. The [blog post](https://medium.com/scala-3/scala-3-type-lambdas-polymorphic-function-types-and-dependent-function-types-2a6eabef896d) provides a more extensive example.

## Metaprogramming

The metaprogramming system is completely new in Scala 3. See my blog posts on [`inline`](https://medium.com/scala-3/scala-3-a-look-at-inline-and-programming-scala-is-now-published-9690ca43c23a) and [macros](https://medium.com/scala-3/scala-3-macros-d63dd6811f89) for more details. See also <a href="./inline.html">this separate overview of `inline`</a>. (I have more posts planned about Scala 3 metaprogramming.)

## Migration

The book's code examples use the flag `-source future` to force deprecation warnings for older constructs. The default, `-source 3.0`, is more forgiving.

Flags to control syntax preferences:

* `-noindent`: Require classical {...} syntax, indentation is not significant.
* `-new-syntax`: Require `then` in conditional expressions.
* `-old-syntax`: Require `(...)` around conditions.

Flags to help migration:

* `-language:Scala2`: Compile Scala 2 code, highlight what needs updating.
* `-migration`: Emit warning and location for migration issues from Scala 2.
* `-rewrite`: Attempt to fix code automatically.
* `-indent`: Use with `-migration` and `-rewrite` to transform code into braceless notation.

## Other Things of Note...

* Traits can have constructor parameter lists, like classes.
* Kind polymorphism: generalize over `A`, `F[A]`, `G[A,B]`, ...
* Dependent function types - We've had dependent _method_ types. Now functions can be dependently typed. See [this blog post](https://medium.com/scala-3/scala-3-type-lambdas-polymorphic-function-types-and-dependent-function-types-2a6eabef896d).
* Polymorphic function types - We've had polymorphic _methods_, e.g., `def size[T](seq: Seq[T])Int = seq.size`. Now functions can be polymorphic. See [this blog post](https://medium.com/scala-3/scala-3-type-lambdas-polymorphic-function-types-and-dependent-function-types-2a6eabef896d).
* New types like `Tuple`, `EmptyTuple` (and tuple operations).
* Explicit `null`s: `def callJavaMethod(...): String | Null`.
* Safe initialization.
* `@main` methods that can replace `object Foo { def main(args: Array[String]):Unit = ??? }` boilerplate (although parsing of arguments is primitive in the first release).
* No more 22-arity limits on tuples and functions.
* _Transparent_ traits (i.e., they don't show up in type inference).
* ... and [lots of other refinements](https://dotty.epfl.ch/docs/Reference/index.html).

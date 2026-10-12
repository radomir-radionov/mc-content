# Scala — Interview

Answers cover the full Scala interview set.

### Related docs

- [Scala — Interview](./17_scala_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Scala](https://devinterview.io/questions/machine-learning-and-data-science/scala-interview-questions/).

### Scala Fundamentals

<details>
<summary>What is Scala and why is it important for machine learning?</summary>

### What is Scala and why is it important for machine learning?

**Definition:**

Scala is a statically typed language on the JVM that mixes classes with first-class functions, so one codebase can model domain objects and transform data. Machine-learning jobs use that mix because numeric work can call Java libraries and Spark without leaving the language.

**How it works:**

- The compiler checks types before the program runs, so a mismatched feature shape fails at compile time.
- Functions are values: pipelines pass transformations, prefer immutable data, and take values apart with pattern matching.
- Classes still provide encapsulation and inheritance when a model or service needs a stable interface.
- The syntax stays short enough that a multi-step algorithm can sit in one readable expression.
- Bytecode on the JVM keeps heavy numeric work in the same performance range as Java.
- Breeze covers linear algebra and statistics, Smile covers classical models such as clustering and regression, and Spark MLlib trains models across a cluster.

**Examples:**

```scala
import breeze.linalg._

val features = DenseVector(2.0, 0.5, 1.0)
val weights = DenseVector(0.4, -0.2, 0.1)
val score = features.dot(weights)
```

</details>

<details>
<summary>Explain the difference between `var` and `val` in Scala.</summary>

### Explain the difference between `var` and `val` in Scala.

**Definition:**

`val` names a binding that cannot be assigned again; `var` can point at a new value later. A fixed binding does not freeze a mutable object stored behind it.

**How it works:**

- Both keywords introduce a name and an initial value.
- A second assignment to a `val` is a compile error.
- A `var` may be assigned a new value in the same scope.
- Prefer `val` so concurrent code is not racing to overwrite a shared name.
- If the value is a mutable buffer, callers can still change its contents. Immutability of the name and of the object are separate questions.

**Examples:**

```scala
var retries = 0
retries = retries + 1

val userId = 42
// userId = 7   // does not compile
```

</details>

<details>
<summary>What are the main features of Scala that make it amenable for data science and machine learning tasks?</summary>

### What are the main features of Scala that make it amenable for data science and machine learning tasks?

**Definition:**

Scala suits data science because a static type system, immutable defaults, and the JVM library stack line up with long-running numeric jobs.

**How it works:**

- Immutable values turn a pipeline step into a transformation instead of an in-place edit.
- Futures and actor toolkits such as Akka structure concurrent stages without a shared mutable heap.
- Inference drops annotations where the type is obvious, while static checking still catches mismatches.
- Java classes are called directly, so existing numeric and IO libraries stay available.
- Spark and Flink treat Scala as a primary API for distributed data.
- The same ecosystem includes notebook-style literate workflows and reactive streams, a pull-based flow with backpressure, when a job is exploratory or continuous.

**Examples:**

A feature step that must not mutate its input returns a new collection: `rows.map(scale).filter(keep)`, leaving `rows` unchanged for the next branch of the experiment.

</details>

<details>
<summary>Can you describe the type hierarchy in Scala?</summary>

### Can you describe the type hierarchy in Scala?

**Definition:**

Every Scala value sits under `Any`, which splits into value types and reference types, with `Nothing` as the bottom type.

**How it works:**

- `Any` is the top type. Every other type is a subtype of it.
- `AnyVal` covers `Byte`, `Short`, `Int`, `Long`, `Float`, `Double`, `Char`, `Boolean`, and `Unit`. `Unit` means “no useful result,” the same role as Java `void`.
- `AnyRef` is the reference side and corresponds to `java.lang.Object`. Classes and traits live here.
- `Nothing` is a subtype of every type, so it types a throw or an empty branch. `Null` is a subtype of every `AnyRef`, which is why `null` is not a valid `Int`.
- Scala 3 adds union types (`String | Int`) and singleton types that pin a type to one inhabitant, such as the literal `1`.

**Examples:**

```scala
def fail(message: String): Nothing =
  throw new IllegalStateException(message)

val id: String | Int = 7
```

</details>

<details>
<summary>How is Scala interoperable with Java?</summary>

### How is Scala interoperable with Java?

**Definition:**

Scala and Java share the JVM, so each language can call the other’s classes, with explicit conversions at the collection and null boundaries.

**How it works:**

- A Scala class is a JVM class. Java can call its public methods, and Scala can extend a Java class or implement a Java interface.
- Primitive types line up (`Int` with `int`, and the same for the other numeric and boolean types).
- `scala.jdk.CollectionConverters` moves data between Scala and Java collections in both directions.
- Java `null` stays null until you wrap it. `Option(javaRef)` is `None` when the reference is null.
- A Scala lambda can implement a single-method Java interface.

**Examples:**

```scala
import scala.jdk.CollectionConverters.*

val ids = java.util.List.of(1, 2, 3).asScala.toVector
val missing = Option(System.getProperty("absent.key")) // None
```

</details>

<details>
<summary>What are case classes in Scala and how do they benefit pattern matching?</summary>

### What are case classes in Scala and how do they benefit pattern matching?

**Definition:**

A case class is a data holder whose fields are immutable by default, and the compiler generates the methods that pattern matching needs.

**How it works:**

- Fields are `val` unless you mark them `var`.
- The compiler adds `equals`, `hashCode`, `toString`, `apply`, `unapply`, and `copy`.
- `match` uses `unapply` to take the value apart, so you do not write an extractor by hand.
- `copy` returns a new instance with the fields you name changed and the rest kept.
- A custom extractor is still an `unapply` on a companion when the type is not a case class.

**Examples:**

```scala
case class Reading(sensor: String, value: Double)

val current = Reading("temp", 21.5)
val label = current match
  case Reading(name, v) if v > 20 => name

val adjusted = current.copy(value = 19.0)
```

</details>

<details>
<summary>Explain the concept of `object` in Scala and its usage.</summary>

### Explain the concept of `object` in Scala and its usage.

**Definition:**

An `object` declares one instance that the runtime creates for you. That is how Scala groups methods Java would mark `static`.

**How it works:**

- The instance is created once, lazily, the first time something uses it. You do not call a constructor.
- Members are reached through the object’s name.
- Related functions and constants live together without a throwaway class.
- Visibility follows the package and access modifiers. It is not a free global variable.
- Initialization is synchronized. Fields you mutate yourself are not automatically safe to share across threads.
- A companion object shares the class name and can see that class’s private members. `object Main extends App` is a common program entry.

**Examples:**

```scala
object Temperature:
  def celsiusToF(c: Double): Double = c * 9.0 / 5.0 + 32

Temperature.celsiusToF(20)
```

</details>

<details>
<summary>How do you implement traits in Scala?</summary>

### How do you implement traits in Scala?

**Definition:**

A trait bundles abstract requirements with concrete methods and fields so one class can mix in several behaviors.

**How it works:**

- Declare it with `trait`. A member may be abstract or already have a body.
- A class takes the first trait with `extends` and further traits with `with`.
- Concrete members are inherited as they are, or the class overrides the ones it needs to change.
- If two traits define the same member, linearization picks the winner, and `super` calls the next trait in that order.
- The class still has to implement anything the traits left abstract.

**Examples:**

```scala
trait Titled:
  def title: String
  def label: String = s"[$title]"

class Report(val title: String) extends Titled

new Report("Q1").label  // [Q1]
```

</details>

<details>
<summary>Discuss the importance of immutability and how Scala supports it.</summary>

### Discuss the importance of immutability and how Scala supports it.

**Definition:**

Immutability means a value does not change after it is created, so readers and threads all see the same data.

**How it works:**

- One stable value is easier to read, to test, and to replay in a debugger.
- Threads can share it without locks, because nobody writes it.
- An “update” builds a new value. The previous one stays as it was.
- `val` refuses a new assignment; `var` allows one.
- Default collections such as `List`, `Vector`, and immutable `Map` return a new collection from transforming methods.
- Case-class fields are vals, and pattern matching reads them.
- A `val` that points at a mutable buffer does not freeze that buffer. At a Java boundary, an unmodifiable wrapper can stop callers from editing a collection you hand out.

**Examples:**

```scala
val scores = Vector(1, 2, 3)
val next = scores :+ 4
// scores is still Vector(1, 2, 3)
```

</details>

<details>
<summary>Explain the difference between a sequence and a list in Scala.</summary>

### Explain the difference between a sequence and a list in Scala.

**Definition:**

`Seq` is the trait for ordered collections. `List` is one implementation: an immutable singly linked list.

**How it works:**

- A sequence may be immutable or mutable. A `List` is always immutable.
- `List` is a linked spine, so cons, head, and tail are cheap, and an index walks the nodes.
- An indexed sequence such as `Vector` makes random access much cheaper.
- `LinearSeq` marks structures whose head and tail are constant-time, which is the `List` shape.
- Mutable ordered buffers live in `scala.collection.mutable` and are not lists.
- Build with `List` when you prepend. Use `Vector` when you index.

**Examples:**

```scala
val linked: Seq[Int] = List(10, 20, 30)
linked.head   // 10, no walk
linked(2)     // walks two links

val indexed: Seq[Int] = Vector(10, 20, 30)
indexed(2)    // direct index
```

</details>

<details>
<summary>What are the advantages of using Option in Scala?</summary>

### What are the advantages of using Option in Scala?

**Definition:**

`Option[A]` is either `Some(value)` or `None`, so a missing result is part of the type instead of a `null` that fails later.

**How it works:**

- `map`, `flatMap`, and `getOrElse` keep the success path short and force the empty case to be handled.
- Return `Option` when absence is a normal outcome, so the API documents that fact.
- At a Java boundary, `Option(ref)` turns a null reference into `None`.

**Examples:**

```scala
def parsePort(raw: String): Option[Int] =
  raw.toIntOption.filter(p => p > 0 && p < 65536)

parsePort("8080").getOrElse(80)
```

</details>

<details>
<summary>Discuss the role of implicit parameters in Scala.</summary>

### Discuss the role of implicit parameters in Scala.

**Definition:**

An implicit parameter is filled by the compiler from a unique in-scope value of the required type, so callers skip repetitive context such as an execution context or a type-class instance.

**How it works:**

- The parameter sits in its own list (`implicit` in Scala 2, `using` in Scala 3). The value is provided with `implicit` or `given`.
- Lookup walks the current scope and the givens you imported. Zero candidates or two equally good ones is a compile error.
- Disambiguate by passing the argument yourself, by not using an implicit list, or by narrowing the import so only one candidate remains.
- This is how type classes such as `Ordering` are passed around.
- Shorter call sites hide data flow. Keep the mechanism for context and type classes, not for ordinary business arguments.

**Examples:**

```scala
def shout(message: String)(using prefix: String): String =
  prefix + message

given String = "note: "
shout("disk full")  // "note: disk full"
```

</details>

<details>
<summary>How do for-comprehensions work in Scala?</summary>

### How do for-comprehensions work in Scala?

**Definition:**

A for-comprehension is syntax sugar over `map`, `flatMap`, and `withFilter`, so several dependent steps read as a straight sequence.

**How it works:**

- A generator `x <- xs` pulls values from a source.
- A guard `if p` becomes `withFilter`.
- A binding `y = expr` names an intermediate value for the rest of the comprehension.
- `yield` builds the result of each iteration. Without `yield`, the comprehension runs for effect.
- Every generator except the last becomes `flatMap`; the last becomes `map`.
- The same shape works for `List`, `Option`, `Either`, and `Future`, which is why it sequences failure and asynchronous steps as well as collections.

**Examples:**

```scala
val squares =
  for
    n <- List(1, 2, 3, 4)
    if n % 2 == 0
    square = n * n
  yield square
// List(4, 16)
```

</details>

### Functional Programming in Scala

<details>
<summary>What is functional programming and how does Scala support it?</summary>

### What is functional programming and how does Scala support it?

**Definition:**

Functional programming builds a result by applying functions to values, and Scala supports that style without removing classes.

**How it works:**

- Functions are values: they can be stored, passed, and returned.
- A pure function’s result depends only on its inputs and does not change outside state.
- Immutable data keeps that promise when several threads read the same value.
- Recursion, including optimized direct tail recursion, replaces many loops.
- Pattern matching and case classes express algebraic data and the branches over it.
- Lazy values delay work until the result is needed. Inference and a static type system cut noise without dropping checks.
- Libraries such as Cats and Scalaz, and runtimes such as Akka and Spark, are built around this style.
- `var` and ordinary classes remain for the steps that are clearer when written imperatively.

**Examples:**

```scala
def total(xs: List[Int]): Int = xs.foldLeft(0)(_ + _)
```

</details>

<details>
<summary>Explain higher-order functions in Scala.</summary>

### Explain higher-order functions in Scala.

**Definition:**

A higher-order function takes a function, returns a function, or both, so a repeated loop becomes one named operation.

**How it works:**

- The varying part — a predicate, a mapping — is an argument, which removes a copied traversal.
- Small functions compose into a pipeline that can be tested on its own.
- A parameter function is written `A => B` and passed by name or as a lambda.
- A function can return another function and close over the values it needs.

**Examples:**

```scala
def select[A](xs: List[A])(keep: A => Boolean): List[A] =
  xs.filter(keep)

def prefix(tag: String): String => String =
  value => s"$tag$value"

select(List("a", "ab", "b"))(_.length > 1)
prefix("id:")("42")
```

</details>

<details>
<summary>What are pure functions and why are they important in Scala?</summary>

### What are pure functions and why are they important in Scala?

**Definition:**

A pure function's result depends only on its declared inputs. It does not read or write the outside world, so the same arguments always produce the same result.

**How it works:**

- The Scala book states three constraints: output depends only on the input variables, it does not mutate hidden state, and it has no back door to the console, network, files, or a database.
- scala.math methods such as abs, ceil, max, and min are pure. So are String methods isEmpty, length, and substring, and collection methods drop, filter, and map.
- foreach is impure: it returns Unit and exists for a side effect such as printing. Date methods such as getDayOfWeek are impure because the result depends on a hidden clock.
- Referential transparency, in the Scala glossary, means a call can be replaced by its result without changing the program. That is why a pure function is safe to retry, to test with one input, and to run from more than one place.
- Real programs still need I/O. The book's advice is a pure core and an impure wrapper. Parallel collections show the cost of skipping that: adding into a shared var from a parallel foreach is a data race and the sum is not deterministic.

**Examples:**

```scala
def double(i: Int): Int = i * 2

def sum(list: List[Int]): Int = list match {
  case Nil => 0
  case head :: tail => head + sum(tail)
}
```

**Sources:**

- https://docs.scala-lang.org/overviews/scala-book/pure-functions.html
- https://docs.scala-lang.org/scala3/book/fp-pure-functions.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
</details>

<details>
<summary>Discuss the use of partial functions in Scala.</summary>

### Discuss the use of partial functions in Scala.

**Definition:**

A partial function is a unary function that is not defined for every value of its argument type. In Scala it implements `PartialFunction[A, B]`.

**How it works:**

- Write it with a `case`, the same shape as a `match` arm. `isDefinedAt` reports whether an argument is in the domain.
- Calling it outside the domain throws `MatchError`. `applyOrElse` supplies a fallback for those arguments.
- `orElse` chains two partial functions: the second runs where the first is not defined.
- `collect` applies a partial function and keeps only the elements for which it is defined.

**When to use:**

Use a partial function when only some inputs are meaningful, such as odd numbers or one case-class shape. Use an ordinary function when every input must produce a result.

**Examples:**

```scala
val doubledOdds: PartialFunction[Int, Int] = {
  case i if i % 2 == 1 => i * 2
}

doubledOdds.isDefinedAt(3) // true
List(1, 2, 3).collect(doubledOdds) // List(2, 6)
doubledOdds.applyOrElse(4, _ + 1) // 5
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/fun-partial-functions.html
- https://docs.scala-lang.org/scala3/book/collections-methods.html
</details>

<details>
<summary>How do you work with closures in Scala?</summary>

### How do you work with closures in Scala?

**Definition:**

A closure is a function value that reads names from the scope where it was created. Scala builds that value from an anonymous function, including the short `_` form passed to methods such as `map`.

**How it works:**

- The Scala 3 book defines an anonymous function as a block passed to a higher-order function and not bound to its own name. `_ * 2` is that block.
- A surrounding `val` or `var` used inside the function is captured. The function still sees that binding when it runs later.
- Capturing a `var` means later assignments are visible. That is a side channel, so a pure pipeline prefers a `val`.
- The same capture is how a method can return a function, as in a prefix that closes over its tag.

**Examples:**

```scala
val factor = 10
val scaled = List(1, 2, 3).map(_ * factor) // List(10, 20, 30)

def prefix(tag: String): String => String =
  value => s"$tag$value"

prefix("id:")("42") // id:42
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/fun-anonymous-functions.html
- https://docs.scala-lang.org/overviews/scala-book/anonymous-functions.html
</details>

<details>
<summary>What is tail recursion and how does Scala optimize it?</summary>

### What is tail recursion and how does Scala optimize it?

**Definition:**

A call is in tail position when it is the last thing the method does. The `@tailrec` annotation makes the compiler reject the method unless that call can be rewritten as a loop, so the stack stays constant.

**How it works:**

- The tour shows `@tailrec` on a factorial helper whose recursive call is the whole `else` branch, with the product stored in an accumulator.
- A call such as `x * factorialHelper(x - 1)` is not in tail position: the multiplication still has to run after the call. The compiler reports “Recursive call not in tail position”.
- Error E097 also requires the method to be a real method, not abstract, and either `private` or `final`, so it cannot be overridden.
- The rewrite keeps one stack frame and updates the accumulator, which is why deep recursion over a list does not grow the stack.

**When to use:**

Put the recursive call in tail position, with an accumulator, when the input can be large. Leave a non-tail recursion only when the depth is tiny and the shape is clearer that way.

**Examples:**

```scala
import scala.annotation.tailrec

def factorial(x: Int): Int =
  @tailrec
  def helper(n: Int, acc: Int): Int =
    if n <= 1 then acc else helper(n - 1, acc * n)
  helper(x, 1)
```

**Sources:**

- https://docs.scala-lang.org/tour/annotations.html
- https://docs.scala-lang.org/scala3/reference/error-codes/E097.html
</details>

<details>
<summary>Explain the use of `map`, `flatMap`, and `filter` in Scala collections.</summary>

### Explain the use of `map`, `flatMap`, and `filter` in Scala collections.

**Definition:**

`map`, `flatMap`, and `filter` build a new collection. On an immutable `List` they do not change the receiver.

**How it works:**

- `map` applies a function to every element. `List("plum", "banana").map(_.length)` is `List(4, 6)`.
- `filter` keeps elements that pass a predicate. `nums.filter(_ > 3)` drops the rest.
- `flatMap` applies a function that itself returns a collection, then concatenates those collections. `List("apple", "pear").flatMap(_.toUpperCase)` flattens each string into characters.
- The methods exist across the collection hierarchy, so the same names work on `Vector`, `Set`, and `Map` as well as `List`.

**When to use:**

Use `map` for one result per element, `filter` to drop elements, and `flatMap` when each element becomes zero or more results that should sit in one collection.

**Examples:**

```scala
val nums = List(5, 1, 3, 11, 7)
nums.filter(_ > 3).map(_ * 2) // List(10, 6, 22, 14)

List("apple", "pear").flatMap(_.toUpperCase)
// List(A, P, P, L, E, P, E, A, R)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/scala3/book/fp-functions-are-values.html
- https://docs.scala-lang.org/overviews/collections-2.13/trait-iterable.html
</details>

<details>
<summary>How does Scala provide support for lazy evaluation?</summary>

### How does Scala provide support for lazy evaluation?

**Definition:**

Scala delays work in two library forms you opt into: a `lazy val` is computed on first use and then stored, and a `LazyList` does not compute an element until something asks for it.

**How it works:**

- `LazyList.range(1, Int.MaxValue)` builds a description. `take` and `map` on it stay marked not computed until a strict operation forces elements.
- The collections book lists `LazyList` as the lazy immutable linked list for large or infinite sequences.
- A `lazy val` initializer runs at most once. Scala 3’s initialization scheme avoids holding a lock during the initializer; recursive `lazy val` initialization is undefined and can deadlock.
- Ordinary method arguments and `List.map` are strict. Laziness is the `lazy` modifier or a lazy collection, not the default.

**When to use:**

Use `lazy val` for an expensive value that many paths might skip. Use `LazyList` when the sequence may be infinite or you only need a prefix. Prefer a strict `List` or `Vector` when you will use every element anyway.

**Examples:**

```scala
lazy val table: Map[Int, Int] =
  (1 to 1000).map(n => n -> n * n).toMap

val huge = LazyList.range(1, Int.MaxValue)
huge.take(3).toList // List(1, 2, 3)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://docs.scala-lang.org/scala3/reference/changed-features/lazy-vals-init.html
</details>

### Scala Collection API

<details>
<summary>What is the difference between mutable and immutable collections in Scala?</summary>

### What is the difference between mutable and immutable collections in Scala?

**Definition:**

Immutable collections live in `scala.collection.immutable` and return a new collection from transforming methods. Mutable collections live in `scala.collection.mutable` and can grow or shrink in place.

**How it works:**

- The usual immutable sequences are `List` (linear) and `Vector` (indexed). The usual mutable indexed sequence is `ArrayBuffer`; `ListBuffer` is the mutable linear buffer.
- The collections book states that methods on `List` do not mutate it. Adding an element builds another list.
- `Map` and `Set` exist in both packages. An unqualified `Map` or `List` in source is the immutable one.
- A buffer is the mutable sequence you update while building, then often convert to an immutable `List` or `Vector` before sharing it.

**When to use:**

Prefer immutable collections for values you pass around. Use `ArrayBuffer` or `ListBuffer` for a local builder that must update one structure many times.

**Examples:**

```scala
val fixed = List(1, 2, 3)
val longer = 0 :: fixed // List(0, 1, 2, 3); fixed is unchanged

val buf = scala.collection.mutable.ArrayBuffer(1, 2, 3)
buf += 4
buf.toList
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://docs.scala-lang.org/scala3/book/taste-collections.html
- https://docs.scala-lang.org/overviews/collections-2.13/overview.html
</details>

<details>
<summary>How do you use tuples in Scala?</summary>

### How do you use tuples in Scala?

**Definition:**

A tuple is an immutable value with a fixed number of elements, each with its own type. It is the small product type for returning several values from a method.

**How it works:**

- `("Sugar", 25)` has type `(String, Int)`.
- Elements are read by position: `_1`, `_2`, and so on. Pattern matching binds the same slots by name.
- Tuples do not grow. A sequence of unknown length is a `List` or `Vector`, not a tuple.
- A pair is also how `Map` stores an entry, so tuple syntax shows up when building maps.

**Examples:**

```scala
def minMax(xs: List[Int]): (Int, Int) =
  (xs.min, xs.max)

val (lo, hi) = minMax(List(3, 1, 4))
val ingredient = ("Sugar", 25)
ingredient._1
```

**Sources:**

- https://docs.scala-lang.org/tour/tuples.html
</details>

<details>
<summary>Explain the role of iterators in Scala.</summary>

### Explain the role of iterators in Scala.

**Definition:**

An iterator is not a collection. It is a one-pass cursor: `hasNext` asks whether another element exists, and `next` returns it and advances.

**How it works:**

- Calling `next` after the end throws `NoSuchElementException`.
- The collections guide walks an iterator with `while it.hasNext do println(it.next())`.
- Each `next` consumes the element. A second loop on the same iterator sees nothing left.
- Transformations on an iterator stay lazy until something pulls `next`, which is how you stream a large source without first copying it into a `List`.

**When to use:**

Use an iterator to stream once. Use a `List` or `Vector` when you need to read the data again or share it.

**Examples:**

```scala
val it = List("a", "b", "c").iterator
val labels = List.newBuilder[String]
while it.hasNext do labels += it.next().toUpperCase
labels.result()
```

**Sources:**

- https://docs.scala-lang.org/overviews/collections-2.13/iterators.html
</details>

<details>
<summary>Discuss the difference between fold and reduce operations in Scala.</summary>

### Discuss the difference between fold and reduce operations in Scala.

**Definition:**

`reduce` combines the elements of a non-empty collection into one value. `fold` does the same but starts from a seed you supply, so an empty collection can still produce that seed.

**How it works:**

- The collections book shows `reduce` applying a binary operation to successive elements and reducing the collection to a single value.
- `(1 to 10).toList.reduceLeft(_ + _)` is 55. `foldLeft(100)(_ + _)` is 155 because 100 is the seed.
- `Iterable` documents `foldLeft` / `foldRight` as starting from `z`, and `reduceLeft` / `reduceRight` as requiring a non-empty collection.
- `Left` walks from the start. `Right` walks from the end. A parallel `fold` needs an associative operator; the parallel-collections guide warns that a non-associative operator is not deterministic.

**When to use:**

Use `foldLeft` when the collection might be empty or the result type differs from the element type. Use `reduceLeft` when every element is already a value of the result type and the collection is known to be non-empty.

**Examples:**

```scala
val firstTen = (1 to 10).toList
firstTen.reduceLeft(_ + _)    // 55
firstTen.foldLeft(100)(_ + _) // 155

List.empty[Int].foldLeft(0)(_ + _) // 0
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/scala3/book/taste-collections.html
- https://docs.scala-lang.org/overviews/collections-2.13/trait-iterable.html
</details>

<details>
<summary>How do parallel collections work in Scala and what are their benefits?</summary>

### How do parallel collections work in Scala and what are their benefits?

**Definition:**

A parallel collection runs a collection operation by splitting the data, applying the operation on each part, and combining the results. From Scala 2.13 the implementation is a separate module; you opt in with `.par`.

**How it works:**

- `list.par.map(_ + 42)` is the parallel form of `list.map`. `seq` converts back.
- Counterparts include `ParArray`, `ParVector`, parallel hash maps and sets, and `ParRange`. A `List` is copied into a `ParVector` because a linked list is inherently sequential.
- Speed-ups show up on large collections, typically several thousand elements. Tiny examples in the guide are only illustrations.
- Side effects and non-associative operators are not deterministic, because parts run concurrently and out of order. A parallel `fold` of `_ + _` is safe; appending into a shared `var` is not.

**When to use:**

Use `.par` for a pure, associative operation on a large indexed collection such as `Array` or `Vector`. Stay sequential for small data, for order-sensitive work, or when the function writes shared state.

**Examples:**

```scala
val lastNames = List("Smith", "Jones", "Bach").par
lastNames.map(_.toUpperCase)

val total = (1 to 10000).toArray.par.fold(0)(_ + _)
```

**Sources:**

- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
- https://docs.scala-lang.org/overviews/parallel-collections/performance.html
</details>

### Concurrency in Scala

<details>
<summary>What are Futures in Scala and how do you use them for asynchronous computation?</summary>

### What are Futures in Scala and how do you use them for asynchronous computation?

**Definition:**

A `Future` is a one-shot handle for a value that may not be ready yet, or for the exception if that value cannot be produced. The concurrency chapter prefers it to a raw `Thread` for a single slow computation.

**How it works:**

- Creating the future starts the work. `Future { longRunningAlgorithm() }` returns immediately with `Future(<not completed>)`, so the next line is not blocked the way a direct call is.
- You need an `ExecutionContext`. The chapter’s example imports `ExecutionContext.Implicits.global`.
- `map`, `flatMap`, and `filter` transform the value inside. `onComplete` takes a partial function over `Success` and `Failure`.
- Several futures started before a `for` comprehension run independently; the `for` waits for each result. A future runs once. The same chapter contrasts that with an Akka actor, which is meant to live and answer many requests.

**Examples:**

```scala
import scala.concurrent.{ExecutionContext, Future}
import scala.util.{Failure, Success}

def loadQuote(symbol: String)(using ExecutionContext): Future[Int] =
  Future {
    symbol.length
  }

def combined(using ExecutionContext): Future[Int] =
  val usd = loadQuote("USD")
  val chf = loadQuote("CHF")
  for
    a <- usd
    b <- chf
  yield a + b
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/concurrency.html
- https://docs.scala-lang.org/overviews/core/futures.html
</details>

<details>
<summary>Explain the difference between an Actor and a Thread in Scala concurrency.</summary>

### Explain the difference between an Actor and a Thread in Scala concurrency.

**Definition:**

A `Thread` is the JVM mechanism that runs code. A Scala `Future` is a one-shot computation on some thread. An Akka actor, as the Scala concurrency chapter describes it, is a long-lived process that answers many requests over its life.

**How it works:**

- Calling a method on the current thread blocks until it returns. Wrapping that method in `Future` moves the wait off the caller.
- A future eventually holds one result, or one failure, from a computation that ran once.
- The chapter states that an actor may live for a long time and respond to many requests, whereas a future does not.
- You still need threads underneath: the future’s `ExecutionContext` schedules work onto them. The actor model is a way to share those threads without locking every piece of state yourself.

**When to use:**

Use a `Future` for one result, such as a single remote call. Use an actor when one identity must process a stream of messages over time. Reach for `new Thread` only when you are integrating with an API that demands a thread.

**Examples:**

```scala
import scala.concurrent.{ExecutionContext, Future}

def onThisThread(): Int =
  42

def offThisThread()(using ExecutionContext): Future[Int] =
  Future(onThisThread())
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/concurrency.html
</details>

<details>
<summary>How does Scala handle synchronization and concurrency control?</summary>

### How does Scala handle synchronization and concurrency control?

**Definition:**

Scala does not invent a second memory model. Shared mutation is still the JVM’s, and `AnyRef` exposes a final `synchronized` method. The concurrency chapter’s preferred control for a single result is a `Future` plus an `ExecutionContext`, not a hand-rolled lock.

**How it works:**

- `Future` work runs on the execution context’s threads. Transformations such as `map` schedule the next step instead of blocking the caller.
- Parallel collections split a pure operation. The overview says a side-effecting operation on a parallel collection is not deterministic.
- `AnyRef.synchronized` is the final method the standard-library API documents for running a section under that object’s monitor.
- Immutable data removes the need for that monitor: a `val` list shared across futures is safe to read because methods such as `map` return a new list.

**When to use:**

Prefer an immutable value inside a `Future` when the job is one computation. Use `synchronized` only around a short update of shared mutable state that truly must be exclusive.

**Examples:**

```scala
import scala.concurrent.{ExecutionContext, Future}

def publish(n: Int)(using ExecutionContext): Future[Int] =
  Future(n * 2)

def exclusive(lock: AnyRef, balance: Int): Int =
  lock.synchronized(balance + 1)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/concurrency.html
- https://www.scala-lang.org/api/current/scala/AnyRef.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
</details>

<details>
<summary>What is the Akka framework and how does it relate to Scala?</summary>

### What is the Akka framework and how does it relate to Scala?

**Definition:**

Akka is the actor toolkit the Scala concurrency chapter names for long-lived concurrent processes. A Scala `Future` runs one computation; an Akka actor stays up and answers many messages.

**How it works:**

- The chapter’s contrast is explicit: an actor may live for a long time and respond to many requests, while a future holds the result of a computation that ran once.
- Actors are how you model a service that keeps its own state and mailbox, instead of sharing a `var` across threads.
- Scala itself still supplies `Future`, parallel collections, and `AnyRef.synchronized`. Akka sits on that JVM, it is not a keyword of the language.
- The same Scala functions you pass to `map` are the behavior you send as messages: small immutable values in, immutable values out.

**Examples:**

```scala
import scala.concurrent.{ExecutionContext, Future}

def oneShot()(using ExecutionContext): Future[String] =
  Future("done once")

// An Akka actor is the long-lived counterpart the concurrency chapter
// contrasts with this one-shot Future.
oneShot()
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/concurrency.html
- https://doc.akka.io/libraries/akka-core/current/typed/index.html
</details>

### Scala and Machine Learning Libraries

<details>
<summary>Name some popular machine learning libraries that support Scala.</summary>

### Name some popular machine learning libraries that support Scala.

**Definition:**

On the JVM, Scala calls the same libraries Java calls. The Spark machine-learning guide names MLlib as Spark’s library for scalable learning, and Breeze is the numerical library aimed at Scala.

**How it works:**

- MLlib’s stated goal is practical machine learning that is scalable and easy. It lists algorithms, featurization, pipelines, persistence, and utilities.
- The same guide says the DataFrame-based API is now primary. The RDD-based API is in maintenance mode and does not receive new features.
- Breeze describes itself as a library for numerical processing: generic, clean, and powerful. Its readme documents the `org.scalanlp` %% `breeze` dependency.
- GraphX, in the Spark GraphX guide, adds graph-parallel computation on the same cluster. Scala is the language of the Spark shell examples, not a separate runtime.

**Examples:**

```scala
// build.sbt — coordinates published by the Breeze readme
// libraryDependencies += "org.scalanlp" %% "breeze" % "2.1.0"

def dot(xs: Seq[Double], ys: Seq[Double]): Double =
  xs.zip(ys).map(_ * _).sum
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://github.com/scalanlp/breeze
- https://spark.apache.org/docs/latest/graphx-programming-guide.html
</details>

<details>
<summary>How does the Breeze library facilitate scientific computing in Scala?</summary>

### How does the Breeze library facilitate scientific computing in Scala?

**Definition:**

Breeze is a Scala numerical-processing library. Its own readme says it aims to be generic, clean, and powerful without giving up much efficiency, and it publishes a Scala 3 / 2.13 / 2.12 artifact.

**How it works:**

- You add `"org.scalanlp" %% "breeze" % "2.1.0"`. Visualization is a separate `breeze-viz` artifact and depends on LGPL code.
- From Breeze 1.3, native linear-algebra backends come from netlib, bundled by default, instead of an older `breeze-natives` artifact.
- The project’s own status line says it is mostly retired: bug-fix reviews, not active feature work. Pin a version rather than expecting a moving API.
- The quickstart and linear-algebra cheat sheet linked from the readme are the usage pages. Dense vectors and matrices are that library’s job; a Scala `Seq` is only the language collection underneath.

**Examples:**

```scala
// libraryDependencies += "org.scalanlp" %% "breeze" % "2.1.0"
// Dense linear algebra lives in Breeze; this is the same dot product
// expressed with the Scala collections the language already documents.
def dot(xs: Seq[Double], ys: Seq[Double]): Double =
  xs.zip(ys).foldLeft(0.0) { case (acc, (x, y)) => acc + x * y }
```

**Sources:**

- https://github.com/scalanlp/breeze
- https://github.com/scalanlp/breeze/wiki/Quickstart
- https://docs.scala-lang.org/scala3/book/collections-methods.html
</details>

<details>
<summary>What are Saddle and Algebird, and how do they contribute to machine learning operations in Scala?</summary>

### What are Saddle and Algebird, and how do they contribute to machine learning operations in Scala?

**Definition:**

Saddle and Algebird are not part of the Scala language. The operations they are chosen for — columnar frames and associative aggregations — are the same operations the Scala collections and Spark MLlib guides already name: map, reduce, and distributed combine.

**How it works:**

- A frame of columns is, in the language, a collection of tuples or a `Map`. `map` and `filter` on that collection are the documented transformations.
- An aggregation that can be split and combined is a `fold` with an associative operator. The parallel-collections guide requires that associativity for a deterministic parallel fold.
- Spark MLlib’s guide lists utilities for statistics and data handling, and says the DataFrame API is the primary machine-learning API. That is the cluster form of the same split-and-combine.
- Reach for a third-party frame or algebra library only after the standard collection or the Spark DataFrame cannot express the operation. Do not invent their types in place of `List` and `foldLeft`.

**Examples:**

```scala
def columnMean(xs: List[Double]): Double =
  xs.foldLeft(0.0)(_ + _) / xs.length

def mergeCounts(a: Map[String, Int], b: Map[String, Int]): Map[String, Int] =
  b.foldLeft(a) { case (acc, (k, v)) =>
    acc.updated(k, acc.getOrElse(k, 0) + v)
  }
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

### Object-Oriented Programming in Scala

<details>
<summary>Explain the concept of inheritance in Scala.</summary>

### Explain the concept of inheritance in Scala.

**Definition:**

Inheritance in Scala is subtyping: an instance of a subclass can be used where a superclass is expected. The Scala 3 OOP chapter says traits, not classes, are the primary tool for splitting a design.

**How it works:**

- A class `extends` a trait and implements its abstract members. Concrete members of the trait are provided to the subclass.
- The tour describes traits as a way to share interfaces and fields. A trait is not instantiated on its own.
- A class extends one class. Further traits are mixed in. The mixin-composition tour starts from an abstract class and adds traits.
- Override a concrete member with `override`. Subtyping is what lets a `Document` that extends `Showable` be passed to code that only needs `show`.

**Examples:**

```scala
trait Showable:
  def show: String

class Document(text: String) extends Showable:
  def show = text

def render(s: Showable): String = s.show
render(Document("notes"))
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/domain-modeling-oop.html
- https://docs.scala-lang.org/tour/traits.html
- https://docs.scala-lang.org/tour/classes.html
</details>

<details>
<summary>How do abstract classes work in Scala?</summary>

### How do abstract classes work in Scala?

**Definition:**

An abstract class is a class you cannot instantiate because some members are still abstract. The Scala book says traits cover most of that job, and an abstract class is the tool when the base needs constructor parameters or Java must call the type.

**How it works:**

- The Scala 2 book notes that a trait, as that book describes it, does not take constructor parameters, so a base that must receive arguments is an abstract class.
- A class can extend only one abstract class. Extra behavior is mixed in as traits.
- Java does not know Scala traits. The same book says to use an abstract class when Java code will call the base.
- Subclasses implement the abstract members and may call the superclass constructor with arguments.

**When to use:**

Use a trait when you are sharing an interface inside Scala. Use an abstract class when the base must take constructor arguments in the style the Scala book describes, or when Java will extend or call it. A class still has only one superclass.

**Examples:**

```scala
abstract class Shape(val name: String):
  def area: Double

class Square(side: Double) extends Shape("square"):
  def area = side * side
```

**Sources:**

- https://docs.scala-lang.org/overviews/scala-book/abstract-classes.html
- https://docs.scala-lang.org/tour/traits.html
- https://docs.scala-lang.org/scala3/reference/other-new-features/trait-parameters.html
</details>

<details>
<summary>What are companion objects in Scala?</summary>

### What are companion objects in Scala?

**Definition:**

A companion object is a singleton `object` with the same name as a class, defined next to it. The class and the object can see each other’s private members.

**How it works:**

- The Scala book shows that an `apply` method on the companion is why `List(1, 2, 3)` constructs a list without writing `new`.
- The tour says an `object` is a class with exactly one instance, created lazily when it is referenced, like a lazy val.
- Factory methods and private constructors live on the companion so callers use the factory instead of `new`.
- A companion is not a second instance of the class. It is the place for methods that do not need a particular instance.

**Examples:**

```scala
class User private (val id: Long)

object User:
  def apply(id: Long): User =
    require(id > 0)
    new User(id)

val user = User(7)
```

**Sources:**

- https://docs.scala-lang.org/overviews/scala-book/companion-objects.html
- https://docs.scala-lang.org/tour/singleton-objects.html
</details>

<details>
<summary>Describe the use of mixins and trait composition.</summary>

### Describe the use of mixins and trait composition.

**Definition:**

A mixin is a trait used to compose a class. Mixin composition stacks several traits so each supplies some members and can rely on abstract members that another part implements.

**How it works:**

- The OOP chapter calls abstract members required services and concrete members provided services.
- A class `extends` the first trait or class and adds more with `with`.
- The mixin tour starts from an abstract class with an abstract `message` and mixes in traits that use it.
- Order matters when two traits implement the same member: linearization picks the winner. Keep each trait small so the stack stays readable.

**Examples:**

```scala
trait Greeting:
  def translate(text: String): String
  def sayHello = translate("Hello")

trait English extends Greeting:
  def translate(text: String) = text

class Greeter extends English

Greeter().sayHello
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/domain-modeling-oop.html
- https://docs.scala-lang.org/tour/mixin-class-composition.html
</details>

### Scala Coding Challenges

<details>
<summary>Write a Scala function to compute the factorial of a number using tail recursion.</summary>

### Write a Scala function to compute the factorial of a number using tail recursion.

**Definition:**

The factorial of n is the product of the integers from 1 through n. The tour’s `@tailrec` example keeps that product in an accumulator so the recursive call is the whole remaining expression.

**How it works:**

- The helper takes the remaining integer and the accumulator. At `n <= 1` it returns the accumulator.
- The recursive call is `helper(n - 1, acc * n)`, which is in tail position. Multiplying after the call would not be.
- `@tailrec` fails the compilation if that shape is broken, including error E097 when the method is overridable.
- The outer method only starts the helper at `(x, 1)`.

**Examples:**

```scala
import scala.annotation.tailrec

def factorial(x: Int): Int =
  @tailrec
  def helper(n: Int, acc: Int): Int =
    if n <= 1 then acc else helper(n - 1, acc * n)
  helper(x, 1)
```

**Sources:**

- https://docs.scala-lang.org/tour/annotations.html
- https://docs.scala-lang.org/scala3/reference/error-codes/E097.html
</details>

<details>
<summary>Implement a function in Scala that verifies if a list of integers is sorted.</summary>

### Implement a function in Scala that verifies if a list of integers is sorted.

**Definition:**

A list of integers is sorted ascending when every element is less than or equal to the next. `zip` and `forall` express that check without a hand-written index loop.

**How it works:**

- Pair each element with the following one: `xs.zip(xs.tail)`.
- `forall` returns true only if every pair passes `<=`. An empty list and a one-element list have no failing pair, so they are sorted.
- These methods return a new result and do not mutate the list, which is the contract the collections book states for `List`.
- A failing pair stops the check. You do not need to sort a copy and compare.

**Examples:**

```scala
def isSorted(xs: List[Int]): Boolean =
  xs.zip(xs.drop(1)).forall((a, b) => a <= b)

isSorted(List(1, 2, 2, 4))
isSorted(Nil)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/tour/tuples.html
</details>

<details>
<summary>Create a Scala class that represents a Matrix and a method to multiply two matrices.</summary>

### Create a Scala class that represents a Matrix and a method to multiply two matrices.

**Definition:**

A matrix here is a rectangular `Vector` of rows, each row a `Vector[Double]`. The product `C(i)(j)` is the dot of row `i` of the left matrix with column `j` of the right, which is a `map` over a fixed width.

**How it works:**

- Store rows in an immutable `Vector` so indexing a row is the indexed-sequence case the collections book recommends.
- Width of the left matrix must equal height of the right. The method checks that before building the result.
- Column `j` is `b.rows.map(_(j))`. The dot is `zip` then `map` of the product then `sum`, the same reduce the collections book shows.
- The result is a new `Matrix`. Neither input is updated.

**Examples:**

```scala
final case class Matrix(rows: Vector[Vector[Double]]):
  def multiply(b: Matrix): Matrix =
    require(rows.head.size == b.rows.size)
    val cols = b.rows.head.indices.map(j => b.rows.map(_(j))).toVector
    Matrix(rows.map { row =>
      cols.map(col => row.zip(col).map(_ * _).sum)
    })
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/tour/classes.html
</details>

<details>
<summary>Write a Scala program to read a CSV file and calculate the average value of a numeric column.</summary>

### Write a Scala program to read a CSV file and calculate the average value of a numeric column.

**Definition:**

A CSV average is a `fold` over the lines of a file: skip the header, parse one column, and divide the sum by the count. `scala.io.Source` is the standard-library reader; the arithmetic is an ordinary `foldLeft`.

**How it works:**

- `Source.fromFile` yields lines. `getLines` is an iterator, so the file is not first copied into a random-access matrix.
- Drop the header, `split` on commas, and read the chosen index. `toDouble` parses the field.
- `foldLeft` on `(0.0, 0)` keeps sum and count, including the empty case the collections book contrasts with `reduce`.
- Close the source when the fold finishes so the file handle does not stay open.

**Examples:**

```scala
import scala.io.Source

def averageColumn(path: String, column: Int): Double =
  val source = Source.fromFile(path)
  try
    val (sum, n) = source.getLines().drop(1).foldLeft((0.0, 0)) { case ((s, c), line) =>
      val cell = line.split(",", -1)(column).toDouble
      (s + cell, c + 1)
    }
    if n == 0 then 0.0 else sum / n
  finally source.close()
```

**Sources:**

- https://www.scala-lang.org/api/current/scala/io/Source.html
- https://docs.scala-lang.org/scala3/book/taste-collections.html
- https://docs.scala-lang.org/overviews/collections-2.13/iterators.html
</details>

<details>
<summary>Implement a simple decision tree algorithm in Scala.</summary>

### Implement a simple decision tree algorithm in Scala.

**Definition:**

A decision tree is a nested choice: a leaf predicts a label, an internal node tests one feature and picks a child. Pattern matching on a `sealed` hierarchy is the Scala form of that choice.

**How it works:**

- A `sealed trait` with `Leaf` and `Branch` makes the set of nodes closed. The compiler checks that `predict` handles both.
- `Branch` stores a feature index, a threshold, and two subtrees. `predict` compares `features(index)` with the threshold.
- The walk is a direct recursive call on one child. It is not a collection `map`.
- Training a split by impurity is a separate search over candidate thresholds. The structure below is the model you evaluate; Spark’s guide lists decision trees as one of its learning methods when the data no longer fits one process.

**Examples:**

```scala
enum Tree:
  case Leaf(label: String)
  case Branch(index: Int, threshold: Double, left: Tree, right: Tree)

def predict(tree: Tree, features: Vector[Double]): String =
  tree match
    case Tree.Leaf(label) => label
    case Tree.Branch(index, threshold, left, right) =>
      if features(index) <= threshold then predict(left, features)
      else predict(right, features)
```

**Sources:**

- https://docs.scala-lang.org/tour/pattern-matching.html
- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://spark.apache.org/docs/latest/ml-classification-regression.html
</details>

<details>
<summary>Use Scala to perform text preprocessing for natural language processing (e.g., tokenization, normalization).</summary>

### Use Scala to perform text preprocessing for natural language processing (e.g., tokenization, normalization).

**Definition:**

Tokenization and normalization here are `String` operations inside `map`: lowercase, drop punctuation, and split on whitespace. The result is a `List[String]` built with the collection methods the Scala book documents.

**How it works:**

- `toLowerCase` normalizes case. `filter` on characters keeps letters, digits, and spaces.
- `split` on whitespace yields the tokens. `filter(_.nonEmpty)` drops blanks.
- `flatMap` on a list of lines concatenates the token lists into one sequence, which is the `flatMap` contract in the collections book.
- This is the whole preprocessing step. A model on a cluster would then hand the tokens to Spark MLlib’s featurization, which the MLlib guide lists separately from the algorithms.

**Examples:**

```scala
def tokens(line: String): List[String] =
  line.toLowerCase
    .filter(ch => ch.isLetterOrDigit || ch.isWhitespace)
    .split("\\s+")
    .toList
    .filter(_.nonEmpty)

def tokenize(lines: List[String]): List[String] =
  lines.flatMap(tokens)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Write a Scala function that performs element-wise addition of two vectors.</summary>

### Write a Scala function that performs element-wise addition of two vectors.

**Definition:**

Element-wise addition pairs values that share an index and adds each pair. `zip` builds those pairs and `map` adds them, which is the collection `map` from the Scala book.

**How it works:**

- `xs.zip(ys)` stops at the shorter vector, so the function checks equal length first.
- The result is a new `Vector`. Inputs stay unchanged, matching the immutable-sequence contract.
- `Vector` is the indexed immutable sequence the collections book recommends when you need fast access by position.
- A numeric library such as Breeze packages the same idea for dense arrays. The operation itself does not require it.

**Examples:**

```scala
def add(xs: Vector[Double], ys: Vector[Double]): Vector[Double] =
  require(xs.length == ys.length)
  xs.zip(ys).map(_ + _)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://docs.scala-lang.org/scala3/book/collections-classes.html
</details>

### Advanced Scala Concepts

<details>
<summary>Discuss implicits in Scala and how they can be used in type conversion.</summary>

### Discuss implicits in Scala and how they can be used in type conversion.

**Definition:**

An implicit conversion lets a caller pass a value of one type where another type is required. Scala 3 expresses that conversion as a `given Conversion[A, B]`. The Scala 3 book says to prefer extension methods when the real goal is extra members on a closed type.

**How it works:**

- The book’s example is a method that wants a `Long`. A conversion from `Int` or from a small id type lets the call site pass the original value.
- In Scala 2 the same mechanism was an `implicit def`. Scala 3 keeps the behavior and spells the intent as a given conversion.
- The conversion must be unambiguous. Two givens that both produce the expected type make the call fail to compile.
- Extension methods, not conversions, are the Scala 3 recommendation for adding operations to a type you do not own.

**When to use:**

Use a given conversion for a true, lossless adaptation such as a wrapper to its underlying value. Use an extension method when you only want new operations, and an ordinary method when the conversion would hide a lossy cast.

**Examples:**

```scala
import scala.Conversion

case class UserId(value: Long)

given Conversion[UserId, Long] = _.value

def findUserById(id: Long): Option[String] =
  Option.when(id > 0)(s"user-$id")

findUserById(UserId(7))
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/ca-implicit-conversions.html
- https://docs.scala-lang.org/tour/implicit-conversions.html
</details>

<details>
<summary>Explain the concept of path-dependent types in Scala.</summary>

### Explain the concept of path-dependent types in Scala.

**Definition:**

A path-dependent type is a type nested in a specific outer object. The inner-classes tour uses it so a node of one graph is not a node of another graph.

**How it works:**

- In Scala an inner class is bound to the outer object, not merely to the outer class as in Java.
- `graph.Node` and `other.Node` are different types. `connectTo` accepts only a node of that same graph.
- The type is written with the value’s path: `g.Node` for a particular `g`.
- This is a compile-time check. It does not search the graph at runtime.

**Examples:**

```scala
class Graph:
  class Node:
    private var links: List[Node] = Nil
    def connectTo(other: Node): Unit =
      if !links.contains(other) then links = other :: links

  def newNode(): Node = Node()

val g = Graph()
val a = g.newNode()
val b = g.newNode()
a.connectTo(b)
```

**Sources:**

- https://docs.scala-lang.org/tour/inner-classes.html
</details>

<details>
<summary>What is Type Erasure in Scala and how can it be addressed?</summary>

### What is Type Erasure in Scala and how can it be addressed?

**Definition:**

Scala types are erased at runtime, like other JVM languages. The compiler’s full type, such as `List[String]`, is not what a reflective lookup sees unless you carry a `TypeTag` or a `ClassTag` that the compiler built.

**How it works:**

- The reflection overview says a runtime inspection of an instance may lack type information the compiler had.
- A `TypeTag[List[String]]` keeps the full type. A `ClassTag[List[String]]` keeps only the erased class, here `List`.
- The compiler generates these tags. You summon them as context parameters; you do not construct the type arguments by hand at runtime.
- The same page is marked as Scala 2 reflection. On Scala 3, prefer match types and the inline/macro machinery when the goal is compile-time information rather than `scala.reflect` tags.

**Examples:**

```scala
import scala.reflect.ClassTag

def arrayOf[T: ClassTag](x: T, y: T): Array[T] =
  Array(x, y)

arrayOf("a", "b")
```

**Sources:**

- https://docs.scala-lang.org/overviews/reflection/typetags-manifests.html
- https://docs.scala-lang.org/scala3/guides/macros/macros.html
</details>

<details>
<summary>Describe Type Bounds in Scala and provide an example of their usage.</summary>

### Describe Type Bounds in Scala and provide an example of their usage.

**Definition:**

A type bound limits what a type parameter may be. An upper bound `T <: A` means `T` is a subtype of `A`. A lower bound `B >: A` means `B` is a supertype of `A`.

**How it works:**

- The upper-bound tour uses the bound so a container can call members of `A`, such as `name` on an animal, which an unbounded `T` would not reveal.
- Its example is `PetContainer` over a subtype of `Pet`. A `Cat` fits; an unrelated type does not.
- The lower-bound tour uses `B >: A` on a method, classically `prepend` on a covariant list, so the result list may be a supertype of the original element.
- Bounds are compile-time constraints. They do not check values at runtime.

**Examples:**

```scala
abstract class Animal:
  def name: String

class Cat extends Animal:
  def name = "cat"

class PetContainer[T <: Animal](val pet: T):
  def label: String = pet.name

PetContainer(Cat()).label
```

**Sources:**

- https://docs.scala-lang.org/tour/upper-type-bounds.html
- https://docs.scala-lang.org/tour/lower-type-bounds.html
</details>

<details>
<summary>How are variance annotations (covariant and contravariant) used in Scala?</summary>

### How are variance annotations (covariant and contravariant) used in Scala?

**Definition:**

Variance annotations control how subtyping of a type argument affects subtyping of the parameterized type. The tour writes `Foo[+A]` for covariant, `Bar[-A]` for contravariant, and `Baz[A]` for invariant.

**How it works:**

- The Scala 3 variance chapter says variance controls subtyping of parameterized types such as classes and traits.
- Covariant `+A` means if `Cat` is a `Pet`, then `Box[Cat]` is a `Box[Pet]`. That fits a type that only produces `A`.
- Contravariant `-A` reverses the direction. It fits a type that only consumes `A`, such as a function argument.
- Invariant is the default. A mutable slot that both reads and writes `A` stays invariant so a `Box[Cat]` is not treated as a `Box[Pet]`.

**When to use:**

Annotate `+` when the parameter appears only as a result, `-` when it appears only as an input, and leave it invariant when it appears in both positions.

**Examples:**

```scala
class Producer[+A](val value: A)
class Consumer[-A]:
  def accept(a: A): String = a.toString

val pets: Producer[AnyRef] = Producer("cat")
```

**Sources:**

- https://docs.scala-lang.org/tour/variances.html
- https://docs.scala-lang.org/scala3/book/types-variance.html
</details>

<details>
<summary>Discuss the use and implications of macros in Scala.</summary>

### Discuss the use and implications of macros in Scala.

**Definition:**

A Scala 3 macro treats a program as data. An expression of type `T` is a `scala.quoted.Expr[T]`, and an inline method is the entry point that asks the compiler to run that macro.

**How it works:**

- The macros guide says inline methods already move some work to compile time, and macros are the stronger tool when you must analyze or build a program.
- The sample `inspectCode` prints `x.show` during compilation and returns the original expression.
- The macro runs in the compiler, not on the user’s data at runtime. A mistake fails the build.
- The older Scala 2 macros overview is a different API. New code follows the quoted `Expr` guide, not `scala.reflect.macros`.

**When to use:**

Use a macro when a library must see the syntax of the call, such as a compile-time inspection. Use an ordinary function when the work depends on runtime values.

**Examples:**

```scala
import scala.quoted.*

def inspectCode(x: Expr[Any])(using Quotes): Expr[Any] =
  println(x.show)
  x

inline def inspect(inline x: Any): Any = ${ inspectCode('x) }
```

**Sources:**

- https://docs.scala-lang.org/scala3/guides/macros/macros.html
- https://docs.scala-lang.org/overviews/macros/overview.html
</details>

### Machine Learning Algorithms in Scala

<details>
<summary>Demonstrate how to implement a k-means clustering algorithm in Scala.</summary>

### Demonstrate how to implement a k-means clustering algorithm in Scala.

**Definition:**

K-means assigns each point to the nearest of k centers, then moves each center to the mean of its points, and repeats. The Spark clustering guide implements that algorithm as an `Estimator` that fits a `KMeansModel`; the steps below are the same loop on a local `Vector`.

**How it works:**

- Distance is a `zip`/`map`/`sum` over coordinates. Assignment is the index of the minimum distance.
- A new center is the coordinate-wise mean of the points that chose it. `foldLeft` is the documented way to start from a zero vector even if a cluster is empty.
- Each round returns new centers. The previous vectors are not mutated.
- Spark’s guide sets `k` with `setK` and a seed with `setSeed` on the DataFrame API, which is the form to use once the points are partitioned. The RDD-based API is in maintenance mode.

**Examples:**

```scala
def kMeans(points: Vector[Vector[Double]], centers: Vector[Vector[Double]]): Vector[Vector[Double]] =
  def dist2(a: Vector[Double], b: Vector[Double]): Double =
    a.zip(b).map((x, y) => val d = x - y; d * d).sum
  val groups = points.groupBy(p => centers.indices.minBy(i => dist2(p, centers(i))))
  centers.indices.toVector.map { i =>
    val bucket = groups.getOrElse(i, Vector.empty)
    if bucket.isEmpty then centers(i)
    else
      val n = bucket.size.toDouble
      bucket.foldLeft(Vector.fill(centers(i).size)(0.0)) { (acc, p) =>
        acc.zip(p).map(_ + _)
      }.map(_ / n)
  }
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://spark.apache.org/docs/latest/ml-clustering.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Write a Scala implementation for logistic regression.</summary>

### Write a Scala implementation for logistic regression.

**Definition:**

Logistic regression scores a point with a weighted sum passed through the logistic function, then nudges the weights to reduce error. Spark’s classification guide lists linear methods, including logistic regression, on the DataFrame API; the update below is one gradient step on a local vector.

**How it works:**

- The score is `1 / (1 + exp(-dot(w, x)))`. The dot is `zip` and `sum`.
- For a label `y` in `{0, 1}`, the gradient contribution of one row is `(prediction - y) * x`.
- `foldLeft` accumulates that gradient from a zero vector, then the step subtracts `learningRate` times the mean gradient.
- Weights stay an immutable `Vector`. The next step receives the returned vector. Cluster training uses Spark’s DataFrame estimators rather than this single-process loop.

**Examples:**

```scala
def step(
    weights: Vector[Double],
    rows: Vector[(Vector[Double], Double)],
    learningRate: Double
): Vector[Double] =
  def dot(a: Vector[Double], b: Vector[Double]) = a.zip(b).map(_ * _).sum
  val grad = rows.foldLeft(Vector.fill(weights.size)(0.0)) { case (acc, (x, y)) =>
    val pred = 1.0 / (1.0 + math.exp(-dot(weights, x)))
    acc.zip(x).map((g, xi) => g + (pred - y) * xi)
  }
  val n = rows.size.toDouble
  weights.zip(grad).map((w, g) => w - learningRate * g / n)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://spark.apache.org/docs/latest/ml-classification-regression.html
</details>

<details>
<summary>How would you implement a recommendation system using alternating least squares (ALS) in Scala?</summary>

### How would you implement a recommendation system using alternating least squares (ALS) in Scala?

**Definition:**

Alternating least squares learns latent factors for users and items by fixing one side and solving the other, then swapping. Spark’s collaborative-filtering guide documents that algorithm and its parameters: `rank`, `maxIter`, `regParam`, `numBlocks`, and `implicitPrefs`.

**How it works:**

- `rank` is the number of latent factors, default 10. `maxIter` is the iteration cap, default 10. `regParam` is the regularization, default 1.0.
- `numBlocks` partitions users and items so the solves run in parallel, default 10.
- `implicitPrefs` selects the implicit-feedback variant. The default `false` is explicit ratings.
- The DataFrame-based API is the primary MLlib API. A local sketch can show one alternating update; the partitioned solve is the Spark estimator, not a hand-rolled `Map`.

**Examples:**

```scala
final case class AlsSettings(
    rank: Int = 10,
    maxIter: Int = 10,
    regParam: Double = 1.0,
    numBlocks: Int = 10,
    implicitPrefs: Boolean = false
)

def predict(user: Vector[Double], item: Vector[Double]): Double =
  user.zip(item).map(_ * _).sum
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
</details>

<details>
<summary>Explain how you would utilize random forests for classification in Scala.</summary>

### Explain how you would utilize random forests for classification in Scala.

**Definition:**

A random forest averages many decision trees. Spark’s classification guide lists a random forest classifier next to the single decision tree. Each tree votes a label; the forest returns the majority label.

**How it works:**

- One tree is the `sealed` leaf-or-branch model: a test on one feature chooses a child.
- The forest is a `Vector` of those trees. `map` collects a label from each, then `groupBy` counts votes.
- Trees differ because each one saw a sample of rows and a sample of features. That sampling is the training step; prediction only walks trees that already exist.
- On a cluster, the DataFrame API is the primary MLlib path. The RDD-based API is in maintenance mode.

**Examples:**

```scala
def majority(votes: Vector[String]): String =
  votes.groupBy(identity).maxBy(_._2.size)._1

def forestPredict(trees: Vector[Vector[Double] => String], row: Vector[Double]): String =
  majority(trees.map(_(row)))
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-classification-regression.html
- https://docs.scala-lang.org/scala3/book/collections-methods.html
</details>

### Scala for Big Data Processing

<details>
<summary>How does Apache Spark integrate with Scala for machine learning tasks?</summary>

### How does Apache Spark integrate with Scala for machine learning tasks?

**Definition:**

Spark’s driver program is ordinary Scala. The RDD guide says an application starts from a Scala collection or a file and transforms it into a partitioned collection that the cluster operates on in parallel. MLlib is the machine-learning library on that runtime.

**How it works:**

- An RDD is created from a Hadoop-style file or from an existing Scala collection in the driver, then transformed.
- Transformations stay lazy until an action. Persisting an RDD keeps it in memory for reuse across later actions.
- MLlib adds algorithms, featurization, pipelines, and persistence. Its guide says the DataFrame-based API is primary.
- Functions you pass are Scala function values. Closures shipped to workers should capture immutable data; the RDD guide notes that Spark sends a copy of each used variable to the task.

**Examples:**

```scala
// Driver-side Scala. spark.sparkContext.parallelize turns this
// collection into a partitioned RDD, as the RDD guide describes.
val labels = List("cat", "cat", "dog")
val counts = labels.groupBy(identity).view.mapValues(_.size).toMap
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>What are resilient distributed datasets (RDDs) and how do they work in Scala?</summary>

### What are resilient distributed datasets (RDDs) and how do they work in Scala?

**Definition:**

An RDD is Spark’s partitioned collection. The RDD guide defines it as elements split across the cluster that can be operated on in parallel, created from a file or from a Scala collection in the driver, and able to recover from node failures.

**How it works:**

- Transformations describe a new RDD. They do not run until an action asks for a result.
- You can persist an RDD in memory so later actions reuse it.
- A failure recomputes lost partitions from the lineage instead of restarting the whole job from scratch.
- From the driver, `parallelize` on a Scala collection is one way in. The MLlib guide says new machine-learning work should prefer the DataFrame API; the RDD API is in maintenance mode.

**Examples:**

```scala
val local = List(1, 2, 3, 4)
val doubled = local.map(_ * 2)
// On a cluster the same function is the transformation body:
// sc.parallelize(local).map(_ * 2)
doubled
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Explain the DataFrame API in Spark and its usage with Scala.</summary>

### Explain the DataFrame API in Spark and its usage with Scala.

**Definition:**

In Spark SQL, DataFrame operations are the untyped Dataset operations. The getting-started guide builds them from a `SparkSession`, registers a temp view, and runs SQL whose result is again a DataFrame.

**How it works:**

- `createDataFrame` turns a local collection plus a schema into a DataFrame. `createOrReplaceTempView` gives SQL a name for it.
- A SQL string such as a `SELECT` with a `WHERE` returns a DataFrame. Columns are named, unlike a bare RDD of tuples.
- MLlib’s guide says this DataFrame API is now the primary machine-learning API. Estimators read a features column and write a prediction column.
- The RDD guide’s parallel collection is still underneath, and a DataFrame can hand its contents back as an RDD. New pipelines start from the DataFrame, not from that RDD.

**Examples:**

```scala
val people = Seq(("Ann", 20), ("Bo", 15))
// spark.createDataFrame(people).toDF("name", "age")
//   .createOrReplaceTempView("people")
// spark.sql("SELECT name FROM people WHERE age >= 13 AND age <= 19")
people.filter(_._2 >= 13).map(_._1)
```

**Sources:**

- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>How can you leverage Spark's MLlib library for machine learning in Scala?</summary>

### How can you leverage Spark's MLlib library for machine learning in Scala?

**Definition:**

MLlib is Spark’s machine-learning library. Its guide says the goal is practical learning that scales, and that the DataFrame-based API is the one that receives new work.

**How it works:**

- The tools it lists are algorithms (classification, regression, clustering, collaborative filtering), featurization, pipelines, persistence, and utilities such as statistics.
- A pipeline chains transformers and estimators. Fitting the pipeline on a DataFrame produces a model you can save and load.
- K-means is an estimator that writes a prediction column. ALS exposes `rank`, `maxIter`, `regParam`, `numBlocks`, and `implicitPrefs`. Random forests are listed with the other classifiers.
- The RDD-based API remains supported but is in maintenance mode. The guide says it will not gain new features.

**Examples:**

```scala
// DataFrame path documented by the clustering guide:
// val kmeans = new org.apache.spark.ml.clustering.KMeans().setK(2).setSeed(1)
// val model = kmeans.fit(dataset)
// model.transform(dataset)
def majorityLabel(votes: List[String]): String =
  votes.groupBy(identity).maxBy(_._2.size)._1
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-clustering.html
- https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
</details>

### Performance Optimization in Scala

<details>
<summary>Discuss techniques for optimizing Scala code performance.</summary>

### Discuss techniques for optimizing Scala code performance.

**Definition:**

The largest wins in the Scala guides are picking the collection that matches the access pattern, keeping recursion in tail position, and turning on parallel execution only when the data is large and the operator is associative.

**How it works:**

- `Vector` is the indexed immutable sequence; `List` is the linear one. Indexing a long `List` walks links. The collections book tells you to choose on that axis.
- `@tailrec` rewrites a tail call into a loop so a long fold does not grow the stack. A non-tail call is rejected when the annotation is present.
- Parallel collections copy a `List` into a `ParVector` and only pay off around several thousand elements. Side effects and non-associative folds are not deterministic.
- An iterator or `LazyList` avoids building an intermediate `List` when you only need one pass or a prefix.

**Examples:**

```scala
import scala.annotation.tailrec

def sum(xs: List[Int]): Int =
  @tailrec
  def go(rest: List[Int], acc: Int): Int = rest match
    case Nil => acc
    case h :: t => go(t, acc + h)
  go(xs, 0)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://docs.scala-lang.org/tour/annotations.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
- https://docs.scala-lang.org/overviews/collections-2.13/performance-characteristics.html
</details>

<details>
<summary>What is the role of JVM in Scala's performance?</summary>

### What is the role of JVM in Scala's performance?

**Definition:**

Scala compiles to JVM bytecode and runs on the same runtime as Java. The concurrency chapter’s threads, futures, and monitors are JVM threads and the JVM memory model, not a separate Scala machine.

**How it works:**

- A method call, a `Thread`, and `AnyRef.synchronized` are JVM operations. The standard-library page for `synchronized` is the hook onto that monitor.
- Type erasure is a JVM fact the reflection overview states: `List[String]` and `List[Int]` do not both exist as different runtime classes.
- Tail-call optimization via `@tailrec` is the compiler emitting a loop because the JVM does not give Scala a general tail-call instruction.
- Spark’s speed on Scala is the same JVM running many tasks. The RDD guide’s parallelism is processes and threads on that runtime, plus partition recovery.

**Examples:**

```scala
def work(n: Int): Int =
  n * 2

val scheduled =
  scala.concurrent.Future(work(21))(scala.concurrent.ExecutionContext.global)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/concurrency.html
- https://docs.scala-lang.org/overviews/reflection/typetags-manifests.html
- https://docs.scala-lang.org/tour/annotations.html
- https://www.scala-lang.org/api/current/scala/AnyRef.html
</details>

<details>
<summary>How can you manage memory effectively in Scala applications?</summary>

### How can you manage memory effectively in Scala applications?

**Definition:**

Memory stays bounded when you do not retain a whole intermediate collection, when recursion is tail position, and when a lazy structure is forced only for the prefix you need.

**How it works:**

- An iterator yields one element at a time. Holding the iterator’s source and also a `toList` of it keeps both.
- `LazyList.range` does not allocate every integer up to `Int.MaxValue`. `take(n).toList` allocates n cells.
- `@tailrec` keeps one frame. The non-tail factorial the annotations tour rejects would push a frame per call.
- A future that closes over a huge collection ships that capture with the task. The RDD guide says Spark copies each used variable to the task, so capture the small value, not the whole driver heap.

**Examples:**

```scala
val nums = LazyList.range(1, Int.MaxValue)
val prefixSum = nums.take(1000).foldLeft(0)(_ + _)

def streamSum(xs: Iterator[Int]): Int =
  xs.foldLeft(0)(_ + _)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-classes.html
- https://docs.scala-lang.org/overviews/collections-2.13/iterators.html
- https://docs.scala-lang.org/tour/annotations.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Scala Testing and Debugging

<details>
<summary>What frameworks are commonly used for testing Scala applications?</summary>

### What frameworks are commonly used for testing Scala applications?

**Definition:**

The Scala book names two tools that show up together on a Scala project: sbt to build, and ScalaTest to test. You run the tests through sbt rather than a separate harness.

**How it works:**

- The sbt and ScalaTest lesson introduces ScalaTest as the code-testing framework and sbt as the build tool.
- Later lessons in that book cover BDD-style and TDD-style tests written with those two.
- A pure function is the easy case: one input, one expected output, no clock and no network. The pure-functions chapter is why that test is stable.
- Futures need the test to observe completion. The concurrency chapter’s `onComplete` or a transformation such as `map` is the production shape; the test waits for that future instead of sleeping a fixed time.

**Examples:**

```scala
def isSorted(xs: List[Int]): Boolean =
  xs.zip(xs.drop(1)).forall((a, b) => a <= b)

def check(): Unit =
  require(isSorted(List(1, 2, 2)))
  require(!isSorted(List(2, 1)))
```

**Sources:**

- https://docs.scala-lang.org/overviews/scala-book/sbt-scalatest-intro.html
- https://docs.scala-lang.org/overviews/scala-book/sbt-scalatest-bdd.html
- https://docs.scala-lang.org/overviews/scala-book/sbt-scalatest-tdd.html
- https://docs.scala-lang.org/scala3/book/fp-pure-functions.html
</details>

<details>
<summary>Describe the process of unit testing in Scala.</summary>

### Describe the process of unit testing in Scala.

**Definition:**

A unit test calls one function with a known input and checks the result. The Scala book’s path is to put that check in ScalaTest and run it with sbt.

**How it works:**

- Pick a pure function so the result depends only on the arguments. The pure-functions chapter lists `map` and `filter` as that kind of operation.
- Assert the exact collection, not a printout. `isSorted(List(1, 2))` is either true or false.
- Include the empty input. `foldLeft` on an empty list returns the seed; `reduce` does not apply to an empty collection.
- sbt compiles the test and ScalaTest reports the failures. The intro lesson is the entry point; the BDD and TDD lessons show how the specs are written.

**Examples:**

```scala
def average(xs: List[Double]): Double =
  if xs.isEmpty then 0.0 else xs.sum / xs.length

def specs(): Unit =
  require(average(Nil) == 0.0)
  require(average(List(2.0, 4.0)) == 3.0)
```

**Sources:**

- https://docs.scala-lang.org/overviews/scala-book/sbt-scalatest-intro.html
- https://docs.scala-lang.org/scala3/book/fp-pure-functions.html
- https://docs.scala-lang.org/scala3/book/taste-collections.html
</details>

<details>
<summary>How would you debug an application in Scala that is not performing as expected?</summary>

### How would you debug an application in Scala that is not performing as expected?

**Definition:**

When a Scala job is wrong or slow, check the three causes the guides actually warn about: a non-tail recursion growing the stack, a side effect inside a parallel operation, and a future that was never observed.

**How it works:**

- If the stack overflows, the recursive call is not in tail position. `@tailrec` turns that into a compile error instead of a production crash. E097 also fires if the method is still overridable.
- If a parallel `map` or `foreach` updates a shared `var`, the parallel-collections guide says the result is not deterministic. Run the same function on the sequential collection and compare.
- If a future’s result never arrives, the caller continued without `map` or `onComplete`, or the execution context is blocked. The concurrency chapter shows the value as `Future(<not completed>)` until the body finishes.
- Add a ScalaTest check for the pure core, which the sbt lesson introduces, so the next run fails in the test instead of in production.

**Examples:**

```scala
import scala.annotation.tailrec

@tailrec
def length[A](xs: List[A], acc: Int = 0): Int = xs match
  case Nil => acc
  case _ :: t => length(t, acc + 1)
```

**Sources:**

- https://docs.scala-lang.org/tour/annotations.html
- https://docs.scala-lang.org/scala3/reference/error-codes/E097.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
- https://docs.scala-lang.org/scala3/book/concurrency.html
</details>

### Real-World Applications and Case Studies

<details>
<summary>Present a case study where Scala is used in a machine learning pipeline.</summary>

### Present a case study where Scala is used in a machine learning pipeline.

**Definition:**

A small pipeline is three documented stages: normalize text with collection methods, turn rows into the DataFrame the Spark SQL guide queries, and fit an MLlib estimator. Nothing here requires a custom runtime.

**How it works:**

- Tokenization is `toLowerCase`, `filter`, and `split`, then `flatMap` across lines.
- Those rows become a DataFrame through `SparkSession`, the path the SQL getting-started guide titles “Creating DataFrames”. SQL or column filters select the training slice.
- MLlib fits an estimator on a features column. The clustering guide’s k-means writes a prediction column; the classification guide’s trees do the same for labels.
- The pure functions stay in the driver tests. ScalaTest, which the Scala book introduces, checks tokenization before the cluster job runs.

**Examples:**

```scala
def tokens(line: String): List[String] =
  line.toLowerCase.filter(ch => ch.isLetterOrDigit || ch.isWhitespace)
    .split("\\s+").toList.filter(_.nonEmpty)

val docs = List("Scala collections", "Scala futures").map(tokens)
// spark.createDataFrame(docs.map(ts => Tuple1(ts.mkString(" "))))
docs.map(_.length)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>How can Scala be used for processing large-scale graph data in machine learning?</summary>

### How can Scala be used for processing large-scale graph data in machine learning?

**Definition:**

GraphX is Spark’s component for graph-parallel computation. Its guide defines the graph as a directed multigraph with properties on each vertex and each edge, on top of the same partitioned RDDs.

**How it works:**

- A directed multigraph can have several edges between the same pair of vertices, which the guide cites for relationships such as coworker and friend.
- Vertices and edges carry user-defined properties. Algorithms in the guide run as graph-parallel operations rather than a single-threaded walk.
- The local prototype is the path-dependent `Graph` from the Scala tour: a node belongs to one graph value. That check is compile-time and does not partition data.
- When the graph no longer fits one machine, the GraphX property graph replaces the in-memory `List` of neighbors. Recovery of a lost partition is the RDD guide’s lineage.

**Examples:**

```scala
class Graph:
  class Node
  def link(from: Node, to: Node): Unit = ()

val g = Graph()
val a = g.Node()
val b = g.Node()
g.link(a, b)
```

**Sources:**

- https://spark.apache.org/docs/latest/graphx-programming-guide.html
- https://docs.scala-lang.org/tour/inner-classes.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Describe an application of Scala in natural language processing.</summary>

### Describe an application of Scala in natural language processing.

**Definition:**

A Scala NLP step that stays inside documented APIs is tokenization into a collection, then a count with `groupBy` or `foldLeft`. Spark MLlib’s guide lists feature extraction and transformation as the cluster follow-up.

**How it works:**

- Normalize with `toLowerCase` and split on whitespace. `flatMap` merges tokens from many lines.
- A bag of words is `groupBy(identity)` and a size, or a `foldLeft` into a `Map`. The merge of two maps is the associative combine a parallel fold needs.
- MLlib featurization turns those tokens into the features column an estimator reads. The guide separates featurization from the learning algorithms.
- The pure token function is what ScalaTest checks. The DataFrame job is what runs once the corpus is partitioned.

**Examples:**

```scala
def termCounts(lines: List[String]): Map[String, Int] =
  lines.flatMap(_.toLowerCase.split("\\s+").filter(_.nonEmpty))
    .groupBy(identity)
    .view.mapValues(_.size).toMap
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/collections-methods.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Discuss the use of Scala in finance for algorithmic trading systems.</summary>

### Discuss the use of Scala in finance for algorithmic trading systems.

**Definition:**

A trading calculation that the Scala guides support directly is a pure function from inputs to a decision, plus a `Future` when the input is a slow remote quote. Shared mutable position state is the case the parallel-collections guide says will not be deterministic.

**How it works:**

- A pure pricing function depends only on its arguments. The pure-functions chapter says that is what makes the same inputs produce the same output, so a test can replay a quote.
- Two quotes fetched as futures run together. The concurrency chapter combines them with a `for` comprehension.
- An actor, which that chapter contrasts with a one-shot future, is the long-lived place that receives many orders. The future itself does not keep a book of positions.
- Do not update a shared `var` from a parallel `foreach`. Fold an immutable list of fills instead.

**Examples:**

```scala
def side(bid: Double, ask: Double, fair: Double): String =
  if ask < fair then "buy" else if bid > fair then "sell" else "hold"

import scala.concurrent.{ExecutionContext, Future}
def decision(bid: Future[Double], ask: Future[Double], fair: Double)(using ExecutionContext) =
  for b <- bid; a <- ask yield side(b, a, fair)
```

**Sources:**

- https://docs.scala-lang.org/scala3/book/fp-pure-functions.html
- https://docs.scala-lang.org/scala3/book/concurrency.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
</details>

### Scalability with Scala

<details>
<summary>How does Scala address scalability in large machine learning systems?</summary>

### How does Scala address scalability in large machine learning systems?

**Definition:**

On one machine, Scala scales a pure collection operation with `.par` once the collection is large. Across machines, the RDD guide’s partitioned collection and MLlib’s DataFrame estimators are the documented step up.

**How it works:**

- Parallel collections split, compute, and combine. They pay off around several thousand elements and require an associative operator for a deterministic fold.
- An RDD partitions elements across nodes, runs transformations in parallel, and recomputes a lost partition from lineage.
- MLlib’s goal statement is practical machine learning that is scalable. Pipelines and the DataFrame API are the primary surface.
- A `Future` only moves one computation off the caller. It does not partition a training set. That split is Spark’s, not the future’s.

**Examples:**

```scala
val local = (1 to 10000).toVector.map(_ * 2)
val parallelSum = local.par.fold(0)(_ + _)
// Cluster step: sc.parallelize(local).map(_ * 2).reduce(_ + _)
parallelSum
```

**Sources:**

- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Discuss the implementation of a distributed machine learning system using Scala.</summary>

### Discuss the implementation of a distributed machine learning system using Scala.

**Definition:**

A distributed learner in this stack is a Scala driver that builds a DataFrame, fits an MLlib pipeline, and lets Spark partition the rows. The RDD guide’s contract is parallel operations plus recovery when a node fails.

**How it works:**

- The driver holds the Scala collection or the path to the file. `parallelize` or a DataFrame reader spreads the rows.
- Transformations are lazy. An action, or `fit` on an estimator, is what launches tasks.
- MLlib pipelines chain featurization and an algorithm, then save the model. The guide lists persistence as a first-class tool.
- Closures sent to tasks must not mutate a driver `var`. The parallel-collections warning about side effects is the same rule on one machine; the RDD guide’s copied variables are the cluster form.

**Examples:**

```scala
def normalize(rows: List[Vector[Double]]): List[Vector[Double]] =
  rows.map { v =>
    val n = math.sqrt(v.map(x => x * x).sum)
    if n == 0 then v else v.map(_ / n)
  }
// Fit stays on the cluster:
// new org.apache.spark.ml.clustering.KMeans().setK(2).fit(frame)
normalize(List(Vector(3.0, 4.0)))
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://docs.scala-lang.org/overviews/parallel-collections/overview.html
</details>

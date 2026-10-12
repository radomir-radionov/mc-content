# Scala — собеседование

Ответы покрывают полный набор вопросов по Scala.

### Related docs

- [Scala — собеседование](./17_scala_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Scala](https://devinterview.io/questions/machine-learning-and-data-science/scala-interview-questions/).

### Scala Fundamentals

<details>
<summary>Что такое Scala and why is it important for machine learning?</summary>

### Что такое Scala and why is it important for machine learning?

**Definition:**

Scala — статически типизированный язык на JVM, в котором классы сочетаются с функциями как значениями: одна кодовая база описывает и объекты предметной области, и преобразования данных. В машинном обучении это удобно тем, что тяжёлые вычисления вызывают Java-библиотеки и Spark, не покидая язык.

**How it works:**

- Компилятор проверяет типы до запуска, поэтому несовпадение формы признаков всплывает на этапе компиляции.
- Функции — значения: пайплайн передаёт преобразования, предпочитает неизменяемые данные и разбирает значения сопоставлением с образцом.
- Классы по-прежнему дают инкапсуляцию и наследование, когда модели или сервису нужен устойчивый интерфейс.
- Синтаксис достаточно короткий, чтобы многошаговый алгоритм уместился в одно читаемое выражение.
- Байткод JVM держит численную работу в том же диапазоне производительности, что и Java.
- Breeze закрывает линейную алгебру и статистику, Smile — классические модели вроде кластеризации и регрессии, Spark MLlib обучает модели на кластере.

**Examples:**

```scala
import breeze.linalg._

val features = DenseVector(2.0, 0.5, 1.0)
val weights = DenseVector(0.4, -0.2, 0.1)
val score = features.dot(weights)
```

</details>

<details>
<summary>Объясните the difference between `var` and `val` in Scala.</summary>

### Объясните the difference between `var` and `val` in Scala.

**Definition:**

`val` задаёт имя, которому нельзя присвоить другое значение; `var` позже может указывать на новое. Фиксированная связь не замораживает изменяемый объект за ней.

**How it works:**

- Оба ключевых слова вводят имя и начальное значение.
- Повторное присваивание `val` — ошибка компиляции.
- `var` можно присвоить заново в той же области видимости.
- Предпочтительнее `val`: параллельный код не соревнуется за перезапись общего имени.
- Если значение — изменяемый буфер, его содержимое всё ещё можно менять. Неизменяемость имени и неизменяемость объекта — разные вопросы.

**Examples:**

```scala
var retries = 0
retries = retries + 1

val userId = 42
// userId = 7   // не компилируется
```

</details>

<details>
<summary>Что такое the main features of Scala that make it amenable for data science and machine learning tasks?</summary>

### Что такое the main features of Scala that make it amenable for data science and machine learning tasks?

**Definition:**

Scala удобна для анализа данных: статическая система типов, неизменяемые значения по умолчанию и библиотеки JVM совпадают с долгими численными задачами.

**How it works:**

- Неизменяемые значения превращают шаг пайплайна в преобразование, а не в правку на месте.
- Future и акторные библиотеки вроде Akka задают конкурентные стадии без общей изменяемой кучи.
- Вывод типов убирает аннотации там, где тип очевиден, а статическая проверка всё равно ловит несовпадения.
- Java-классы вызываются напрямую, поэтому готовые численные библиотеки и библиотеки ввода-вывода остаются доступны.
- Spark и Flink считают Scala основным API для распределённых данных.
- В той же экосистеме есть блокнотный стиль и реактивные потоки — вытягиваемый поток с обратным давлением — когда задача исследовательская или непрерывная.

**Examples:**

Шаг признаков, который не должен менять вход, возвращает новую коллекцию: `rows.map(scale).filter(keep)`. Сам `rows` остаётся прежним для следующей ветки эксперимента.

</details>

<details>
<summary>Можете ли вы describe the type hierarchy in Scala?</summary>

### Можете ли вы describe the type hierarchy in Scala?

**Definition:**

Любое значение Scala находится под `Any`. Этот тип делится на типы-значения и ссылочные типы, а `Nothing` стоит внизу иерархии.

**How it works:**

- `Any` — верхний тип. Любой другой тип является его подтипом.
- `AnyVal` включает `Byte`, `Short`, `Int`, `Long`, `Float`, `Double`, `Char`, `Boolean` и `Unit`. `Unit` означает «полезного результата нет» — та же роль, что у `void` в Java.
- `AnyRef` — ссылочная сторона и соответствует `java.lang.Object`. Здесь живут классы и трейты.
- `Nothing` — подтип всех типов, поэтому им типизируют бросок исключения или пустую ветку. `Null` — подтип всех `AnyRef`, поэтому `null` нельзя присвоить `Int`.
- В Scala 3 есть union-типы (`String | Int`) и singleton-типы, которые фиксируют тип одним значением, например литералом `1`.

**Examples:**

```scala
def fail(message: String): Nothing =
  throw new IllegalStateException(message)

val id: String | Int = 7
```

</details>

<details>
<summary>Как Scala interoperable with Java?</summary>

### Как Scala interoperable with Java?

**Definition:**

Scala и Java делят JVM, поэтому каждый язык вызывает классы другого. На границе коллекций и `null` нужны явные преобразования.

**How it works:**

- Класс Scala — это класс JVM. Java вызывает его публичные методы, а Scala может расширить Java-класс или реализовать Java-интерфейс.
- Примитивы соответствуют друг другу (`Int` и `int`, и так же остальные числовые типы и логический тип).
- `scala.jdk.CollectionConverters` переводит данные между коллекциями Scala и Java в обе стороны.
- Java-`null` остаётся null, пока его не обернуть. `Option(javaRef)` даёт `None`, если ссылка равна null.
- Лямбда Scala может реализовать Java-интерфейс с одним методом.

**Examples:**

```scala
import scala.jdk.CollectionConverters.*

val ids = java.util.List.of(1, 2, 3).asScala.toVector
val missing = Option(System.getProperty("absent.key")) // None
```

</details>

<details>
<summary>Что такое case Classes in Scala and how do they benefit pattern matching?</summary>

### Что такое case Classes in Scala and how do they benefit pattern matching?

**Definition:**

Case class — держатель данных, поля которого по умолчанию неизменяемы. Компилятор сам создаёт методы, нужные для сопоставления с образцом.

**How it works:**

- Поля — `val`, если явно не пометить их `var`.
- Компилятор добавляет `equals`, `hashCode`, `toString`, `apply`, `unapply` и `copy`.
- `match` использует `unapply`, чтобы разобрать значение, поэтому экстрактор вручную не пишут.
- `copy` возвращает новый экземпляр: названные поля меняются, остальные сохраняются.
- Свой экстрактор — это всё тот же `unapply` у компаньона, когда тип не является case class.

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
<summary>Объясните the concept of `object` in Scala and its usage.</summary>

### Объясните the concept of `object` in Scala and its usage.

**Definition:**

`object` объявляет один экземпляр, который создаёт среда выполнения. Так в Scala группируют методы, которые в Java были бы `static`.

**How it works:**

- Экземпляр создаётся один раз, лениво, при первом обращении. Конструктор вызывать не нужно.
- К членам обращаются по имени объекта.
- Связанные функции и константы живут вместе без одноразового класса.
- Видимость задают пакет и модификаторы доступа. Это не свободная глобальная переменная.
- Инициализация синхронизирована. Поля, которые вы меняете сами, автоматически не становятся безопасными для нескольких потоков.
- Companion object носит имя класса и видит его приватные члены. `object Main extends App` — обычная точка входа.

**Examples:**

```scala
object Temperature:
  def celsiusToF(c: Double): Double = c * 9.0 / 5.0 + 32

Temperature.celsiusToF(20)
```

</details>

<details>
<summary>Как you implement traits in Scala?</summary>

### Как you implement traits in Scala?

**Definition:**

Трейт собирает абстрактные требования вместе с готовыми методами и полями, чтобы один класс подмешал несколько поведений.

**How it works:**

- Объявляют его через `trait`. Член может быть абстрактным или уже иметь тело.
- Класс берёт первый трейт через `extends`, остальные — через `with`.
- Конкретные члены наследуются как есть, либо класс переопределяет те, что нужно изменить.
- Если два трейта определяют один член, линеаризация выбирает победителя, а `super` вызывает следующий трейт в этом порядке.
- Класс всё равно обязан реализовать то, что трейты оставили абстрактным.

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
<summary>Обсудите the importance of immutability and how Scala supports it.</summary>

### Обсудите the importance of immutability and how Scala supports it.

**Definition:**

Неизменяемость значит, что значение после создания не меняется: и читатели, и потоки видят одни и те же данные.

**How it works:**

- Одно устойчивое значение проще читать, тестировать и воспроизводить в отладчике.
- Потоки могут делить его без блокировок, потому что никто его не пишет.
- «Обновление» строит новое значение. Прежнее остаётся как было.
- `val` запрещает новое присваивание; `var` его разрешает.
- Коллекции по умолчанию — `List`, `Vector`, неизменяемый `Map` — возвращают новую коллекцию из методов преобразования.
- Поля case class — это val, а сопоставление с образцом их читает.
- `val`, который указывает на изменяемый буфер, этот буфер не замораживает. На границе с Java неизменяемая обёртка не даёт вызывающему править отданную коллекцию.

**Examples:**

```scala
val scores = Vector(1, 2, 3)
val next = scores :+ 4
// scores по-прежнему Vector(1, 2, 3)
```

</details>

<details>
<summary>Объясните the difference between a sequence and a list in Scala.</summary>

### Объясните the difference between a sequence and a list in Scala.

**Definition:**

`Seq` — трейт упорядоченных коллекций. `List` — одна реализация: неизменяемый односвязный список.

**How it works:**

- Последовательность бывает неизменяемой и изменяемой. `List` всегда неизменяем.
- `List` — цепочка узлов: cons, голова и хвост дёшевы, а доступ по индексу идёт по узлам.
- Индексируемая последовательность вроде `Vector` делает случайный доступ намного дешевле.
- `LinearSeq` помечает структуры, у которых голова и хвост за постоянное время. Такова форма `List`.
- Изменяемые упорядоченные буферы лежат в `scala.collection.mutable` и списками не являются.
- Берите `List`, когда добавляете в начало. Берите `Vector`, когда обращаетесь по индексу.

**Examples:**

```scala
val linked: Seq[Int] = List(10, 20, 30)
linked.head   // 10, без обхода
linked(2)     // два шага по ссылкам

val indexed: Seq[Int] = Vector(10, 20, 30)
indexed(2)    // прямой индекс
```

</details>

<details>
<summary>Что такое the advantages of using Option in Scala?</summary>

### Что такое the advantages of using Option in Scala?

**Definition:**

`Option[A]` — это либо `Some(value)`, либо `None`: отсутствие результата входит в тип, а не всплывает позже как `null`.

**How it works:**

- `map`, `flatMap` и `getOrElse` оставляют успешный путь коротким и заставляют обработать пустой случай.
- Возвращайте `Option`, когда отсутствие — обычный исход: API это прямо показывает.
- На границе с Java `Option(ref)` превращает null-ссылку в `None`.

**Examples:**

```scala
def parsePort(raw: String): Option[Int] =
  raw.toIntOption.filter(p => p > 0 && p < 65536)

parsePort("8080").getOrElse(80)
```

</details>

<details>
<summary>Обсудите the role of implicit parameters in Scala.</summary>

### Обсудите the role of implicit parameters in Scala.

**Definition:**

Неявный параметр компилятор подставляет из единственного видимого значения нужного типа. Вызывающий не повторяет контекст вроде контекста выполнения или экземпляра тайпкласса.

**How it works:**

- Параметр стоит в отдельном списке (`implicit` в Scala 2, `using` в Scala 3). Значение задают через `implicit` или `given`.
- Поиск идёт по текущей области и импортированным given. Ноль кандидатов или два одинаково подходящих — ошибка компиляции.
- Уточняют выбор, передавая аргумент явно, отказываясь от неявного списка или сужая импорт, чтобы остался один кандидат.
- Так передают тайпклассы вроде `Ordering`.
- Короткие вызовы прячут поток данных. Механизм уместен для контекста и тайпклассов, а не для обычных бизнес-аргументов.

**Examples:**

```scala
def shout(message: String)(using prefix: String): String =
  prefix + message

given String = "note: "
shout("disk full")  // "note: disk full"
```

</details>

<details>
<summary>Как for-comprehensions work in Scala?</summary>

### Как for-comprehensions work in Scala?

**Definition:**

For-comprehension — синтаксический сахар над `map`, `flatMap` и `withFilter`: несколько зависимых шагов читаются как прямая последовательность.

**How it works:**

- Генератор `x <- xs` берёт значения из источника.
- Охрана `if p` превращается в `withFilter`.
- Связывание `y = expr` даёт имя промежуточному значению на остаток выражения.
- `yield` строит результат каждой итерации. Без `yield` выражение выполняется ради эффекта.
- Все генераторы, кроме последнего, становятся `flatMap`; последний становится `map`.
- Та же форма работает для `List`, `Option`, `Either` и `Future`, поэтому ею выстраивают и ошибки, и асинхронные шаги, и коллекции.

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
<summary>Что такое functional programming and how does Scala support it?</summary>

### Что такое functional programming and how does Scala support it?

**Definition:**

Функциональное программирование строит результат применением функций к значениям. Scala поддерживает этот стиль и при этом не отказывается от классов.

**How it works:**

- Функции — значения: их можно хранить, передавать и возвращать.
- Результат чистой функции зависит только от входов и не меняет внешнее состояние.
- Неизменяемые данные сохраняют это обещание, когда несколько потоков читают одно значение.
- Рекурсия, включая оптимизированную прямую хвостовую рекурсию, заменяет многие циклы.
- Сопоставление с образцом и case class выражают алгебраические данные и ветки по ним.
- Ленивые значения откладывают работу до момента, когда результат нужен. Вывод типов и статическая система убирают шум, не отменяя проверки.
- Библиотеки вроде Cats и Scalaz и среды вроде Akka и Spark построены вокруг этого стиля.
- `var` и обычные классы остаются для шагов, которые яснее записать императивно.

**Examples:**

```scala
def total(xs: List[Int]): Int = xs.foldLeft(0)(_ + _)
```

</details>

<details>
<summary>Объясните higher-order functions in Scala.</summary>

### Объясните higher-order functions in Scala.

**Definition:**

Функция высшего порядка принимает функцию, возвращает функцию или делает и то и другое. Повторяющийся цикл становится одной именованной операцией.

**How it works:**

- Меняющаяся часть — предикат, отображение — передаётся аргументом, и скопированный обход исчезает.
- Небольшие функции складываются в цепочку, которую можно проверить отдельно.
- Функция-параметр записывается как `A => B` и передаётся по имени или лямбдой.
- Функция может вернуть другую функцию и захватить нужные ей значения.

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
<summary>Что такое pure functions and why are they important in Scala?</summary>

### Что такое pure functions and why are they important in Scala?

**Definition:**

Результат чистой функции зависит только от объявленных входов. Она не читает и не пишет внешний мир, поэтому одни и те же аргументы всегда дают один и тот же результат.

**How it works:**

- Книга Scala задаёт три ограничения: выход зависит только от входных переменных, функция не меняет спрятанное состояние и у неё нет чёрного хода к консоли, сети, файлам или базе.
- Методы scala.math, такие как abs, ceil, max и min, чистые. Чистые и методы String isEmpty, length и substring, и методы коллекций drop, filter и map.
- foreach нечистый: он возвращает Unit и существует ради побочного эффекта, например печати. Методы даты вроде getDayOfWeek нечистые, потому что результат зависит от спрятанных часов.
- Ссылочная прозрачность в глоссарии Scala значит, что вызов можно заменить его результатом, не меняя программу. Поэтому чистую функцию безопасно повторить, проверить одним входом и вызвать из нескольких мест.
- Настоящим программам всё ещё нужен ввод-вывод. Совет книги — чистое ядро и нечистая оболочка. Параллельные коллекции показывают цену, если это пропустить: прибавление в общий var из параллельного foreach — гонка, и сумма не детерминирована.

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
<summary>Обсудите the use of partial functions in Scala.</summary>

### Обсудите the use of partial functions in Scala.

**Definition:**

Частичная функция — унарная функция, определённая не для каждого значения своего аргумента. В Scala она реализует `PartialFunction[A, B]`.

**How it works:**

- Её пишут через `case`, как ветку `match`. `isDefinedAt` говорит, входит ли аргумент в область определения.
- Вызов вне области бросает `MatchError`. `applyOrElse` подставляет запасное значение для таких аргументов.
- `orElse` сцепляет две частичные функции: вторая работает там, где первая не определена.
- `collect` применяет частичную функцию и оставляет только элементы, для которых она определена.

**When to use:**

Частичная функция уместна, когда осмысленны только некоторые входы, например нечётные числа или одна форма case-класса. Обычная функция нужна, когда результат обязателен для каждого входа.

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
<summary>Как you work with closures in Scala?</summary>

### Как you work with closures in Scala?

**Definition:**

Замыкание — значение-функция, которое читает имена из области, где его создали. Scala строит такое значение из анонимной функции, включая короткую форму `_`, которую передают в методы вроде `map`.

**How it works:**

- Книга Scala 3 определяет анонимную функцию как блок, переданный функции высшего порядка и не привязанный к собственному имени. `_ * 2` — такой блок.
- `val` или `var` снаружи, использованный внутри функции, захватывается. Функция видит эту привязку и при более позднем вызове.
- Захват `var` делает видимыми последующие присваивания. Это побочный канал, поэтому чистому конвейеру лучше `val`.
- Тот же захват позволяет методу вернуть функцию, например префикс, замкнутый на свой тег.

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
<summary>Что такое tail recursion and how does Scala optimize it?</summary>

### Что такое tail recursion and how does Scala optimize it?

**Definition:**

Вызов стоит в хвостовой позиции, если это последнее действие метода. Аннотация `@tailrec` заставляет компилятор отклонить метод, если этот вызов нельзя переписать в цикл, чтобы стек оставался постоянным.

**How it works:**

- Тур показывает `@tailrec` на помощнике факториала, где рекурсивный вызов — вся ветка `else`, а произведение лежит в аккумуляторе.
- Вызов вида `x * factorialHelper(x - 1)` не в хвостовой позиции: умножение ещё должно выполниться после вызова. Компилятор сообщает «Recursive call not in tail position».
- Ошибка E097 также требует, чтобы это был настоящий метод, не абстрактный, и либо `private`, либо `final`, чтобы его нельзя было переопределить.
- Переписывание держит один кадр стека и обновляет аккумулятор, поэтому глубокая рекурсия по списку не растит стек.

**When to use:**

Ставьте рекурсивный вызов в хвостовую позицию с аккумулятором, когда вход может быть большим. Нехвостовую рекурсию оставляйте только при крошечной глубине, если так яснее форма.

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
<summary>Объясните the use of `Map`, `flatMap`, and `filter` in Scala collections.</summary>

### Объясните the use of `Map`, `flatMap`, and `filter` in Scala collections.

**Definition:**

`map`, `flatMap` и `filter` строят новую коллекцию. На неизменяемом `List` они не меняют получателя.

**How it works:**

- `map` применяет функцию к каждому элементу. `List("plum", "banana").map(_.length)` даёт `List(4, 6)`.
- `filter` оставляет элементы, прошедшие предикат. `nums.filter(_ > 3)` отбрасывает остальные.
- `flatMap` применяет функцию, которая сама возвращает коллекцию, и склеивает эти коллекции. `List("apple", "pear").flatMap(_.toUpperCase)` разворачивает каждую строку в символы.
- Методы есть во всей иерархии коллекций, поэтому те же имена работают на `Vector`, `Set` и `Map`, не только на `List`.

**When to use:**

`map` — когда на элемент один результат, `filter` — чтобы выбросить элементы, `flatMap` — когда элемент становится нулём или несколькими результатами в одной коллекции.

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
<summary>Как Scala provide support for lazy evaluation?</summary>

### Как Scala provide support for lazy evaluation?

**Definition:**

Scala откладывает работу в двух библиотечных формах, которые включают явно: `lazy val` считается при первом использовании и затем хранится, а `LazyList` не считает элемент, пока его не запросят.

**How it works:**

- `LazyList.range(1, Int.MaxValue)` строит описание. `take` и `map` на нём остаются «not computed», пока строгая операция не заставит элементы.
- Книга коллекций называет `LazyList` ленивым неизменяемым связным списком для больших или бесконечных последовательностей.
- Инициализатор `lazy val` выполняется не больше одного раза. Схема инициализации Scala 3 не держит блокировку во время инициализатора; рекурсивная инициализация `lazy val` не определена и может зависнуть.
- Обычные аргументы методов и `List.map` строгие. Ленивость — это модификатор `lazy` или ленивая коллекция, а не поведение по умолчанию.

**When to use:**

`lazy val` — для дорогой величины, которую многие пути могут пропустить. `LazyList` — когда последовательность может быть бесконечной или нужен только префикс. Строгий `List` или `Vector` лучше, если элементы всё равно понадобятся все.

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
<summary>Что такое the difference between mutable and immutable collections in Scala?</summary>

### Что такое the difference between mutable and immutable collections in Scala?

**Definition:**

Неизменяемые коллекции живут в `scala.collection.immutable`, и преобразующие методы возвращают новую коллекцию. Изменяемые живут в `scala.collection.mutable` и могут расти или сжиматься на месте.

**How it works:**

- Обычные неизменяемые последовательности — `List` (линейный) и `Vector` (индексный). Обычная изменяемая индексная последовательность — `ArrayBuffer`; `ListBuffer` — изменяемый линейный буфер.
- Книга коллекций прямо говорит, что методы `List` его не мутируют. Добавление элемента строит другой список.
- `Map` и `Set` есть в обоих пакетах. Неквалифицированные `Map` и `List` в исходнике — неизменяемые.
- Буфер — изменяемая последовательность, которую обновляют при сборке и часто превращают в неизменяемый `List` или `Vector` перед тем, как отдать наружу.

**When to use:**

Неизменяемые коллекции предпочтительны для значений, которые передают дальше. `ArrayBuffer` или `ListBuffer` — для локального сборщика, который много раз обновляет одну структуру.

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
<summary>Как you use tuples in Scala?</summary>

### Как you use tuples in Scala?

**Definition:**

Кортеж — неизменяемое значение с фиксированным числом элементов, у каждого свой тип. Это маленький тип-произведение, чтобы вернуть из метода несколько значений.

**How it works:**

- `("Sugar", 25)` имеет тип `(String, Int)`.
- Элементы читают по позиции: `_1`, `_2` и далее. Сопоставление с образцом именует те же слоты.
- Кортеж не растёт. Последовательность неизвестной длины — это `List` или `Vector`, не кортеж.
- Пара — ещё и способ хранить запись `Map`, поэтому синтаксис кортежа появляется при сборке отображений.

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
<summary>Объясните the role of iterators in Scala.</summary>

### Объясните the role of iterators in Scala.

**Definition:**

Итератор — не коллекция. Это курсор на один проход: `hasNext` спрашивает, есть ли ещё элемент, а `next` возвращает его и сдвигается.

**How it works:**

- Вызов `next` после конца бросает `NoSuchElementException`.
- Руководство по коллекциям обходит итератор так: `while it.hasNext do println(it.next())`.
- Каждый `next` потребляет элемент. Второй цикл по тому же итератору уже ничего не видит.
- Преобразования итератора остаются ленивыми, пока кто-то не вызовет `next`. Так читают большой источник, не копируя его сначала в `List`.

**When to use:**

Итератор — чтобы пройти данные один раз. `List` или `Vector` — когда данные нужно прочитать снова или отдать нескольким местам.

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
<summary>Обсудите the difference between fold and reduce operations in Scala.</summary>

### Обсудите the difference between fold and reduce operations in Scala.

**Definition:**

`reduce` сворачивает элементы непустой коллекции в одно значение. `fold` делает то же, но начинает с заданного начального значения, поэтому пустая коллекция всё ещё может вернуть это значение.

**How it works:**

- Книга коллекций показывает, что `reduce` применяет бинарную операцию к соседним элементам и сводит коллекцию к одному значению.
- `(1 to 10).toList.reduceLeft(_ + _)` равно 55. `foldLeft(100)(_ + _)` равно 155, потому что 100 — начальное значение.
- `Iterable` описывает `foldLeft` / `foldRight` как старт с `z`, а `reduceLeft` / `reduceRight` — как требование непустой коллекции.
- `Left` идёт от начала, `Right` — от конца. Параллельный `fold` требует ассоциативный оператор; руководство по параллельным коллекциям предупреждает, что неассоциативный оператор недетерминирован.

**When to use:**

`foldLeft` — когда коллекция может быть пустой или тип результата отличается от типа элемента. `reduceLeft` — когда каждый элемент уже имеет тип результата и коллекция заведомо непуста.

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
<summary>Как parallel collections work in Scala and what are their benefits?</summary>

### Как parallel collections work in Scala and what are their benefits?

**Definition:**

Параллельная коллекция выполняет операцию, разбивая данные, применяя операцию к каждой части и соединяя результаты. С Scala 2.13 реализация — отдельный модуль; её включают через `.par`.

**How it works:**

- `list.par.map(_ + 42)` — параллельная форма `list.map`. `seq` возвращает последовательную коллекцию.
- Соответствия включают `ParArray`, `ParVector`, параллельные хеш-отображения и множества и `ParRange`. `List` копируется в `ParVector`, потому что связный список по природе последователен.
- Ускорение заметно на больших коллекциях, обычно в несколько тысяч элементов. Крошечные примеры в руководстве только иллюстрации.
- Побочные эффекты и неассоциативные операторы недетерминированы: части идут параллельно и не по порядку. Параллельный `fold` с `_ + _` безопасен; дописывание в общий `var` — нет.

**When to use:**

`.par` — для чистой ассоциативной операции на большой индексной коллекции вроде `Array` или `Vector`. Последовательный вариант — для малых данных, работы, чувствительной к порядку, и функций, которые пишут общее состояние.

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
<summary>Что такое Futures in Scala and how do you use them for asynchronous computation?</summary>

### Что такое Futures in Scala and how do you use them for asynchronous computation?

**Definition:**

`Future` — одноразовый дескриптор значения, которого может ещё не быть, или исключения, если значение получить нельзя. Глава о конкурентности предпочитает его голому `Thread` для одного медленного вычисления.

**How it works:**

- Создание future запускает работу. `Future { longRunningAlgorithm() }` сразу возвращает `Future(<not completed>)`, поэтому следующая строка не блокируется, как прямой вызов.
- Нужен `ExecutionContext`. Пример главы импортирует `ExecutionContext.Implicits.global`.
- `map`, `flatMap` и `filter` преобразуют значение внутри. `onComplete` принимает частичную функцию по `Success` и `Failure`.
- Несколько future, запущенных до `for`, идут независимо; `for` ждёт каждый результат. Future выполняется один раз. Та же глава противопоставляет это актору Akka, который живёт и отвечает на много запросов.

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
<summary>Объясните the difference between an Actor and a Thread in Scala concurrency.</summary>

### Объясните the difference between an Actor and a Thread in Scala concurrency.

**Definition:**

`Thread` — механизм JVM, который выполняет код. `Future` в Scala — одноразовое вычисление на каком-то потоке. Актор Akka, как его описывает глава Scala о конкурентности, — долгоживущий процесс, который за жизнь отвечает на много запросов.

**How it works:**

- Вызов метода в текущем потоке блокирует его до возврата. Обёртка в `Future` уносит ожидание с вызывающего.
- Future в итоге хранит один результат или одну ошибку вычисления, которое прошло один раз.
- Глава говорит, что актор может жить долго и отвечать на много запросов, а future — нет.
- Потоки всё равно нужны снизу: `ExecutionContext` future ставит работу на них. Модель акторов — способ делить эти потоки, не запирая каждое состояние самому.

**When to use:**

`Future` — для одного результата, например одного удалённого вызова. Актор — когда одна сущность должна обрабатывать поток сообщений во времени. `new Thread` — только если API прямо требует поток.

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
<summary>Как Scala handle synchronization and concurrency control?</summary>

### Как Scala handle synchronization and concurrency control?

**Definition:**

Scala не вводит вторую модель памяти. Общая мутация остаётся моделью JVM, а `AnyRef` даёт финальный метод `synchronized`. Предпочтительный контроль одного результата в главе о конкурентности — `Future` плюс `ExecutionContext`, а не замок, написанный вручную.

**How it works:**

- Работа `Future` идёт на потоках контекста выполнения. Преобразования вроде `map` планируют следующий шаг и не блокируют вызывающего.
- Параллельные коллекции дробят чистую операцию. Обзор говорит, что операция с побочным эффектом на параллельной коллекции недетерминирована.
- `AnyRef.synchronized` — финальный метод, который API стандартной библиотеки документирует для участка под монитором этого объекта.
- Неизменяемые данные снимают нужду в мониторе: `val`-список, общий для нескольких future, можно читать, потому что методы вроде `map` возвращают новый список.

**When to use:**

Неизменяемое значение внутри `Future` предпочтительно, когда задача — одно вычисление. `synchronized` — только вокруг короткого обновления общего изменяемого состояния, которое действительно должно быть исключительным.

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
<summary>Что такое the Akka framework and how does it relate to Scala?</summary>

### Что такое the Akka framework and how does it relate to Scala?

**Definition:**

Akka — инструментарий акторов, который глава Scala о конкурентности называет для долгоживущих конкурентных процессов. `Future` в Scala выполняет одно вычисление; актор Akka остаётся живым и отвечает на много сообщений.

**How it works:**

- Противопоставление в главе прямое: актор может жить долго и отвечать на много запросов, а future хранит результат вычисления, которое прошло один раз.
- Акторы моделируют сервис со своим состоянием и почтовым ящиком вместо общего `var` между потоками.
- Сам язык по-прежнему даёт `Future`, параллельные коллекции и `AnyRef.synchronized`. Akka стоит на этой JVM, это не ключевое слово языка.
- Те же функции Scala, которые передают в `map`, становятся поведением сообщений: на вход маленькие неизменяемые значения, на выход тоже.

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
<summary>Назовите some popular machine learning libraries that support Scala.</summary>

### Назовите some popular machine learning libraries that support Scala.

**Definition:**

На JVM Scala вызывает те же библиотеки, что и Java. Руководство Spark по машинному обучению называет MLlib библиотекой Spark для масштабируемого обучения, а Breeze — численная библиотека, нацеленная на Scala.

**How it works:**

- Заявленная цель MLlib — практичное машинное обучение, масштабируемое и простое. В списке алгоритмы, признаки, конвейеры, сохранение и утилиты.
- То же руководство говорит, что основной теперь API на DataFrame. API на RDD в режиме сопровождения и не получает новых возможностей.
- Breeze описывает себя как библиотеку численной обработки: обобщённую, ясную и мощную. Readme документирует зависимость `org.scalanlp` %% `breeze`.
- GraphX в руководстве Spark добавляет графо-параллельные вычисления на том же кластере. Scala — язык примеров оболочки Spark, а не отдельная среда выполнения.

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
<summary>Как the Breeze library facilitate scientific computing in Scala?</summary>

### Как the Breeze library facilitate scientific computing in Scala?

**Definition:**

Breeze — библиотека численной обработки для Scala. Её readme говорит, что цель — быть обобщённой, ясной и мощной, не теряя много в эффективности, и публикует артефакт для Scala 3 / 2.13 / 2.12.

**How it works:**

- Добавляют `"org.scalanlp" %% "breeze" % "2.1.0"`. Визуализация — отдельный артефакт `breeze-viz`, он зависит от кода под LGPL.
- С Breeze 1.3 нативные бэкенды линейной алгебры идут из netlib и входят по умолчанию, вместо старого артефакта `breeze-natives`.
- Собственная строка статуса проекта говорит, что он в основном на покое: ревью исправлений, не активная разработка возможностей. Фиксируйте версию.
- Quickstart и шпаргалка по линейной алгебре, на которые ссылается readme, — страницы использования. Плотные векторы и матрицы — задача этой библиотеки; `Seq` в Scala — только коллекция языка под ней.

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
<summary>Что такое Saddle and Algebird, and how do they contribute to machine learning operations in Scala?</summary>

### Что такое Saddle and Algebird, and how do they contribute to machine learning operations in Scala?

**Definition:**

Saddle и Algebird не входят в язык Scala. Операции, ради которых их выбирают — колоночные кадры и ассоциативные агрегации — те же, которые уже называют руководства по коллекциям Scala и Spark MLlib: map, reduce и распределённое объединение.

**How it works:**

- Кадр из колонок на языке — коллекция кортежей или `Map`. `map` и `filter` на этой коллекции — документированные преобразования.
- Агрегация, которую можно разбить и собрать, — это `fold` с ассоциативным оператором. Руководство по параллельным коллекциям требует эту ассоциативность для детерминированного параллельного fold.
- Руководство MLlib перечисляет утилиты статистики и работы с данными и говорит, что API DataFrame — основной API машинного обучения. Это кластерная форма того же «разбить и собрать».
- Стороннюю библиотеку кадров или алгебры берут, только если стандартная коллекция или DataFrame Spark не выражают операцию. Не подменяйте их типы вместо `List` и `foldLeft`.

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
<summary>Объясните the concept of inheritance in Scala.</summary>

### Объясните the concept of inheritance in Scala.

**Definition:**

Наследование в Scala — это подтипирование: экземпляр подкласса можно использовать там, где ждут суперкласс. Глава ООП Scala 3 говорит, что главный инструмент разбиения проекта — трейты, а не классы.

**How it works:**

- Класс делает `extends` трейта и реализует его абстрактные члены. Конкретные члены трейта достаются подклассу.
- Тур описывает трейты как способ делить интерфейсы и поля. Сам по себе трейт не создают.
- Класс наследует один класс. Дальнейшие трейты подмешивают. Тур по mixin-композиции начинает с абстрактного класса и добавляет трейты.
- Конкретный член переопределяют через `override`. Подтипирование позволяет передать `Document`, который расширяет `Showable`, туда, где нужен только `show`.

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
<summary>Как abstract classes work in Scala?</summary>

### Как abstract classes work in Scala?

**Definition:**

Абстрактный класс — класс, который нельзя создать, потому что часть членов ещё абстрактна. Книга Scala говорит, что трейты закрывают большую часть этой задачи, а абстрактный класс нужен, когда базе требуются параметры конструктора или тип должен вызываться из Java.

**How it works:**

- Книга Scala 2 отмечает, что трейт, как она его описывает, не принимает параметры конструктора, поэтому база, которой нужны аргументы, — абстрактный класс.
- Класс может расширить только один абстрактный класс. Дополнительное поведение подмешивают трейтами.
- Java не знает трейтов Scala. Та же книга советует абстрактный класс, когда базовый тип будет вызывать код на Java.
- Подклассы реализуют абстрактные члены и могут вызвать конструктор суперкласса с аргументами.

**When to use:**

Трейт — когда интерфейс делят внутри Scala. Абстрактный класс — когда базе нужны аргументы конструктора в том виде, как описывает книга Scala, или когда её будет расширять или вызывать Java. Суперкласс у класса всё равно один.

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
<summary>Что такое companion objects in Scala?</summary>

### Что такое companion objects in Scala?

**Definition:**

Объект-компаньон — синглтон `object` с тем же именем, что и класс, объявленный рядом. Класс и объект видят приватные члены друг друга.

**How it works:**

- Книга Scala показывает, что метод `apply` на компаньоне — причина, почему `List(1, 2, 3)` строит список без `new`.
- Тур говорит, что `object` — класс ровно с одним экземпляром, создаваемый лениво при первом обращении, как lazy val.
- Фабрики и приватные конструкторы живут на компаньоне, чтобы вызывающий шёл через фабрику, а не через `new`.
- Компаньон — не второй экземпляр класса. Это место для методов, которым не нужен конкретный экземпляр.

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
<summary>Опишите the use of mixins and trait composition.</summary>

### Опишите the use of mixins and trait composition.

**Definition:**

Mixin — это трейт, которым собирают класс. Композиция трейтов складывает несколько трейтов: каждый даёт часть членов и может опираться на абстрактные члены, которые реализует другая часть.

**How it works:**

- Глава ООП называет абстрактные члены требуемыми сервисами, а конкретные — предоставляемыми.
- Класс делает `extends` первого трейта или класса и добавляет остальные через `with`.
- Тур по mixin начинает с абстрактного класса с абстрактным `message` и подмешивает трейты, которые его используют.
- Порядок важен, когда два трейта реализуют один член: линеаризация выбирает победителя. Держите трейты маленькими, чтобы стек читался.

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
<summary>Напишите a Scala function to compute the Factorial of a number using tail recursion.</summary>

### Напишите a Scala function to compute the Factorial of a number using tail recursion.

**Definition:**

Факториал n — произведение целых от 1 до n. Пример `@tailrec` в туре хранит это произведение в аккумуляторе, чтобы рекурсивный вызов был всем оставшимся выражением.

**How it works:**

- Помощник принимает оставшееся число и аккумулятор. При `n <= 1` он возвращает аккумулятор.
- Рекурсивный вызов — `helper(n - 1, acc * n)`, он в хвостовой позиции. Умножение после вызова ею не было бы.
- `@tailrec` ломает компиляцию, если эта форма нарушена, включая ошибку E097, когда метод можно переопределить.
- Внешний метод только запускает помощника с `(x, 1)`.

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
<summary>Реализуйте a function in Scala that verifies if a list of integers is sorted.</summary>

### Реализуйте a function in Scala that verifies if a list of integers is sorted.

**Definition:**

Список целых отсортирован по возрастанию, если каждый элемент не больше следующего. `zip` и `forall` выражают эту проверку без ручного цикла по индексу.

**How it works:**

- Каждый элемент ставят в пару со следующим: `xs.zip(xs.tail)`.
- `forall` истинен, только если каждая пара проходит `<=`. У пустого списка и списка из одного элемента нет плохой пары, они отсортированы.
- Эти методы возвращают новый результат и не мутируют список — это контракт, который книга коллекций задаёт для `List`.
- Плохая пара останавливает проверку. Копию сортировать и сравнивать не нужно.

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
<summary>Создайте a Scala class that represents a matrix and a method to multiply two matrices.</summary>

### Создайте a Scala class that represents a matrix and a method to multiply two matrices.

**Definition:**

Матрица здесь — прямоугольный `Vector` строк, каждая строка — `Vector[Double]`. Элемент произведения `C(i)(j)` — скалярное произведение строки `i` левой матрицы и столбца `j` правой, то есть `map` по фиксированной ширине.

**How it works:**

- Строки хранят в неизменяемом `Vector`: индексация строки — тот индексный случай, который рекомендует книга коллекций.
- Ширина левой матрицы должна равняться высоте правой. Метод проверяет это до сборки результата.
- Столбец `j` — это `b.rows.map(_(j))`. Скалярное произведение — `zip`, затем `map` произведения и `sum`, тот же reduce, что в книге коллекций.
- Результат — новая `Matrix`. Ни один вход не обновляется.

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
<summary>Напишите a Scala program to read a CSV file and calculate the average value of a numeric column.</summary>

### Напишите a Scala program to read a CSV file and calculate the average value of a numeric column.

**Definition:**

Среднее по CSV — это `fold` по строкам файла: пропустить заголовок, разобрать один столбец и поделить сумму на число. `scala.io.Source` — читатель стандартной библиотеки; арифметика — обычный `foldLeft`.

**How it works:**

- `Source.fromFile` даёт строки. `getLines` — итератор, поэтому файл не копируют сначала в матрицу произвольного доступа.
- Заголовок отбрасывают, делят по запятым и читают нужный индекс. `toDouble` разбирает поле.
- `foldLeft` по `(0.0, 0)` хранит сумму и счётчик, включая пустой случай, который книга коллекций противопоставляет `reduce`.
- Источник закрывают, когда fold закончился, чтобы дескриптор файла не оставался открытым.

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
<summary>Реализуйте a simple decision tree alGorithm in Scala.</summary>

### Реализуйте a simple decision tree alGorithm in Scala.

**Definition:**

Дерево решений — вложенный выбор: лист предсказывает метку, внутренний узел проверяет один признак и выбирает ребёнка. Сопоставление с образцом по `sealed`-иерархии — форма этого выбора в Scala.

**How it works:**

- `sealed trait` с `Leaf` и `Branch` замыкает набор узлов. Компилятор проверяет, что `predict` обрабатывает оба.
- `Branch` хранит индекс признака, порог и два поддерева. `predict` сравнивает `features(index)` с порогом.
- Обход — прямой рекурсивный вызов одного ребёнка. Это не `map` по коллекции.
- Обучение разбиения по неоднородности — отдельный перебор порогов. Структура ниже — модель, которую вычисляют; руководство Spark перечисляет деревья решений среди методов обучения, когда данные уже не помещаются в один процесс.

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
<summary>Используйте Scala to perform text preprocessing for natural language processing (e.g., tokenization, normalization).</summary>

### Используйте Scala to perform text preprocessing for natural language processing (e.g., tokenization, normalization).

**Definition:**

Токенизация и нормализация здесь — операции `String` внутри `map`: нижний регистр, снятие пунктуации и разбиение по пробелам. Результат — `List[String]`, собранный методами коллекций, которые документирует книга Scala.

**How it works:**

- `toLowerCase` нормализует регистр. `filter` по символам оставляет буквы, цифры и пробелы.
- `split` по пробелам даёт токены. `filter(_.nonEmpty)` убирает пустые.
- `flatMap` по списку строк склеивает списки токенов в одну последовательность — это контракт `flatMap` в книге коллекций.
- Это весь шаг предобработки. Модель на кластере затем отдаёт токены в выделение признаков MLlib, которое руководство MLlib перечисляет отдельно от алгоритмов.

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
<summary>Напишите a Scala function that performs element-wise addition of two vectors.</summary>

### Напишите a Scala function that performs element-wise addition of two vectors.

**Definition:**

Поэлементное сложение ставит в пары значения с одним индексом и складывает каждую пару. `zip` строит пары, `map` их складывает — это коллекционный `map` из книги Scala.

**How it works:**

- `xs.zip(ys)` останавливается на более коротком векторе, поэтому функция сначала проверяет равную длину.
- Результат — новый `Vector`. Входы не меняются, как требует контракт неизменяемой последовательности.
- `Vector` — индексная неизменяемая последовательность, которую книга коллекций рекомендует, когда нужен быстрый доступ по позиции.
- Численная библиотека вроде Breeze упаковывает ту же идею для плотных массивов. Самой операции она не требуется.

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
<summary>Обсудите implicits in Scala and how they can be used in type conversion.</summary>

### Обсудите implicits in Scala and how they can be used in type conversion.

**Definition:**

Неявное преобразование позволяет передать значение одного типа туда, где требуется другой. Scala 3 выражает это преобразование как `given Conversion[A, B]`. Книга Scala 3 советует предпочесть методы расширения, если настоящая цель — дополнительные члены у закрытого типа.

**How it works:**

- Пример книги — метод, которому нужен `Long`. Преобразование из `Int` или из маленького типа идентификатора позволяет на месте вызова передать исходное значение.
- В Scala 2 тот же механизм был `implicit def`. Scala 3 сохраняет поведение и записывает намерение как given-преобразование.
- Преобразование должно быть однозначным. Два given, которые оба дают ожидаемый тип, ломают компиляцию вызова.
- Методы расширения, а не преобразования, — рекомендация Scala 3, когда к чужому типу добавляют операции.

**When to use:**

Given-преобразование — для настоящей адаптации без потерь, например обёртки к её значению. Метод расширения — когда нужны только новые операции. Обычный метод — когда преобразование спрятало бы потерю данных.

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
<summary>Объясните the concept of path-dependent types in Scala.</summary>

### Объясните the concept of path-dependent types in Scala.

**Definition:**

Путь-зависимый тип — тип, вложенный в конкретный внешний объект. Тур по внутренним классам использует его, чтобы узел одного графа не был узлом другого.

**How it works:**

- В Scala внутренний класс привязан к внешнему объекту, а не только к внешнему классу, как в Java.
- `graph.Node` и `other.Node` — разные типы. `connectTo` принимает только узел того же графа.
- Тип пишут путём значения: `g.Node` для конкретного `g`.
- Это проверка времени компиляции. Граф в рантайме она не обходит.

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
<summary>Что такое Type Erasure in Scala and how can it be addressed?</summary>

### Что такое Type Erasure in Scala and how can it be addressed?

**Definition:**

Типы Scala стираются во время выполнения, как у других языков JVM. Полный тип компилятора, например `List[String]`, рефлексивный поиск не видит, если не пронести `TypeTag` или `ClassTag`, который собрал компилятор.

**How it works:**

- Обзор рефлексии говорит, что осмотр экземпляра в рантайме может не иметь типовой информации, которая была у компилятора.
- `TypeTag[List[String]]` хранит полный тип. `ClassTag[List[String]]` хранит только стёртый класс, здесь `List`.
- Эти теги порождает компилятор. Их запрашивают как контекстные параметры; аргументы типа вручную в рантайме не собирают.
- Та же страница помечена как рефлексия Scala 2. В Scala 3 для информации времени компиляции предпочтительны match-типы и inline/макросы, а не теги `scala.reflect`.

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
<summary>Опишите Type Bounds in Scala and provide an example of their usage.</summary>

### Опишите Type Bounds in Scala and provide an example of their usage.

**Definition:**

Ограничение типа сужает, чем может быть параметр типа. Верхняя граница `T <: A` значит, что `T` — подтип `A`. Нижняя граница `B >: A` значит, что `B` — супертип `A`.

**How it works:**

- Тур по верхней границе использует её, чтобы контейнер мог вызвать члены `A`, например `name` у животного, чего неограниченный `T` не показал бы.
- Его пример — `PetContainer` над подтипом `Pet`. `Cat` подходит; посторонний тип — нет.
- Тур по нижней границе использует `B >: A` на методе, классически `prepend` у ковариантного списка, чтобы список-результат мог иметь супертип исходного элемента.
- Границы — ограничения времени компиляции. Значения в рантайме они не проверяют.

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
<summary>Как variance annotations (covariant and contravariant) used in Scala?</summary>

### Как variance annotations (covariant and contravariant) used in Scala?

**Definition:**

Аннотации вариантности управляют тем, как подтипирование аргумента типа влияет на подтипирование параметризованного типа. Тур пишет `Foo[+A]` для ковариантного, `Bar[-A]` для контравариантного и `Baz[A]` для инвариантного.

**How it works:**

- Глава Scala 3 о вариантности говорит, что она управляет подтипированием параметризованных типов, таких как классы и трейты.
- Ковариантный `+A` значит: если `Cat` — это `Pet`, то `Box[Cat]` — это `Box[Pet]`. Это подходит типу, который только производит `A`.
- Контравариантный `-A` переворачивает направление. Он подходит типу, который только потребляет `A`, например аргументу функции.
- По умолчанию тип инвариантен. Изменяемая ячейка, которая и читает, и пишет `A`, остаётся инвариантной, чтобы `Box[Cat]` не считали `Box[Pet]`.

**When to use:**

`+` — когда параметр встречается только как результат, `-` — только как вход, без аннотации — когда он встречается в обеих позициях.

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
<summary>Обсудите the use and implications of macros in Scala.</summary>

### Обсудите the use and implications of macros in Scala.

**Definition:**

Макрос Scala 3 обращается с программой как с данными. Выражение типа `T` — это `scala.quoted.Expr[T]`, а inline-метод — точка входа, которая просит компилятор выполнить этот макрос.

**How it works:**

- Руководство по макросам говорит, что inline-методы уже переносят часть работы на компиляцию, а макросы — более сильный инструмент, когда программу нужно разобрать или построить.
- Пример `inspectCode` печатает `x.show` во время компиляции и возвращает исходное выражение.
- Макрос выполняется в компиляторе, а не на данных пользователя в рантайме. Ошибка ломает сборку.
- Старый обзор макросов Scala 2 — другой API. Новый код следует руководству по quoted `Expr`, а не `scala.reflect.macros`.

**When to use:**

Макрос — когда библиотека должна видеть синтаксис вызова, например осмотр на этапе компиляции. Обычная функция — когда работа зависит от значений времени выполнения.

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
<summary>Продемонстрируйте how to implement a k-means clustering alGorithm in Scala.</summary>

### Продемонстрируйте how to implement a k-means clustering alGorithm in Scala.

**Definition:**

K-means относит каждую точку к ближайшему из k центров, затем сдвигает каждый центр в среднее своих точек и повторяет. Руководство Spark по кластеризации реализует этот алгоритм как `Estimator`, который обучает `KMeansModel`; шаги ниже — тот же цикл на локальном `Vector`.

**How it works:**

- Расстояние — `zip`/`map`/`sum` по координатам. Назначение — индекс минимального расстояния.
- Новый центр — среднее по координатам точек, которые его выбрали. `foldLeft` — документированный способ начать с нулевого вектора, даже если кластер пуст.
- Каждый раунд возвращает новые центры. Прежние векторы не мутируют.
- Руководство Spark задаёт `k` через `setK` и зерно через `setSeed` в API DataFrame — эта форма нужна, когда точки уже разбиты на разделы. API на RDD в режиме сопровождения.

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
<summary>Напишите a Scala implementation for logistic regression.</summary>

### Напишите a Scala implementation for logistic regression.

**Definition:**

Логистическая регрессия считает оценку точки как взвешенную сумму, пропущенную через логистическую функцию, и сдвигает веса, чтобы уменьшить ошибку. Руководство Spark по классификации перечисляет линейные методы, включая логистическую регрессию, в API DataFrame; обновление ниже — один шаг градиента на локальном векторе.

**How it works:**

- Оценка — `1 / (1 + exp(-dot(w, x)))`. Скалярное произведение — `zip` и `sum`.
- Для метки `y` из `{0, 1}` вклад строки в градиент — `(prediction - y) * x`.
- `foldLeft` копит этот градиент от нулевого вектора, затем шаг вычитает `learningRate`, умноженный на средний градиент.
- Веса остаются неизменяемым `Vector`. Следующий шаг получает возвращённый вектор. Обучение на кластере использует оценщики DataFrame в Spark, а не этот однопроцессный цикл.

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
<summary>Как бы вы implement a recommendation system using alternating least squares (ALS) in Scala?</summary>

### Как бы вы implement a recommendation system using alternating least squares (ALS) in Scala?

**Definition:**

Чередующиеся наименьшие квадраты учат скрытые факторы пользователей и предметов: фиксируют одну сторону, решают другую и меняют их местами. Руководство Spark по коллаборативной фильтрации документирует этот алгоритм и параметры: `rank`, `maxIter`, `regParam`, `numBlocks` и `implicitPrefs`.

**How it works:**

- `rank` — число скрытых факторов, по умолчанию 10. `maxIter` — предел итераций, по умолчанию 10. `regParam` — регуляризация, по умолчанию 1.0.
- `numBlocks` разбивает пользователей и предметы, чтобы решения шли параллельно, по умолчанию 10.
- `implicitPrefs` выбирает вариант с неявной обратной связью. Значение по умолчанию `false` — явные оценки.
- API на DataFrame — основной API MLlib. Локальный набросок может показать одно чередующееся обновление; разбитое на разделы решение — это оценщик Spark, а не ручной `Map`.

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
<summary>Объясните how you would utilize random foRESTs for classification in Scala.</summary>

### Объясните how you would utilize random foRESTs for classification in Scala.

**Definition:**

Случайный лес усредняет много деревьев решений. Руководство Spark по классификации ставит классификатор «случайный лес» рядом с одним деревом решений. Каждое дерево голосует за метку; лес возвращает метку большинства.

**How it works:**

- Одно дерево — `sealed`-модель листа или ветки: проверка одного признака выбирает ребёнка.
- Лес — `Vector` таких деревьев. `map` собирает метку с каждого, затем `groupBy` считает голоса.
- Деревья различаются, потому что каждое видело выборку строк и выборку признаков. Эта выборка — шаг обучения; предсказание только обходит уже существующие деревья.
- На кластере основной путь MLlib — API DataFrame. API на RDD в режиме сопровождения.

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
<summary>Как Apache Spark integrate with Scala for machine learning tasks?</summary>

### Как Apache Spark integrate with Scala for machine learning tasks?

**Definition:**

Программа-драйвер Spark — обычная Scala. Руководство по RDD говорит, что приложение стартует с коллекции Scala или файла и превращает её в разбитую на разделы коллекцию, с которой кластер работает параллельно. MLlib — библиотека машинного обучения на этой среде.

**How it works:**

- RDD создают из файла в стиле Hadoop или из уже существующей коллекции Scala на драйвере, затем преобразуют.
- Преобразования остаются ленивыми до действия. Сохранение RDD держит его в памяти для повторного использования в следующих действиях.
- MLlib добавляет алгоритмы, признаки, конвейеры и сохранение. Его руководство говорит, что основной API — на DataFrame.
- Функции, которые передают, — значения-функции Scala. Замыкания, уезжающие на рабочие узлы, должны захватывать неизменяемые данные; руководство по RDD отмечает, что Spark отправляет копию каждой использованной переменной в задачу.

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
<summary>Что такое resilient distributed datasets (RDDs) and how do they work in Scala?</summary>

### Что такое resilient distributed datasets (RDDs) and how do they work in Scala?

**Definition:**

RDD — разбитая на разделы коллекция Spark. Руководство по RDD определяет её как элементы, распределённые по кластеру, с которыми можно работать параллельно; её создают из файла или из коллекции Scala на драйвере, и она восстанавливается после отказа узла.

**How it works:**

- Преобразования описывают новый RDD. Они не выполняются, пока действие не запросит результат.
- RDD можно сохранить в памяти, чтобы следующие действия его переиспользовали.
- Отказ пересчитывает потерянные разделы по цепочке преобразований, а не запускает всю работу с нуля.
- С драйвера `parallelize` коллекции Scala — один вход. Руководство MLlib говорит, что новой работе по машинному обучению лучше предпочесть API DataFrame; API RDD в режиме сопровождения.

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
<summary>Объясните the DataFrame API in Spark and its usage with Scala.</summary>

### Объясните the DataFrame API in Spark and its usage with Scala.

**Definition:**

В Spark SQL операции DataFrame — это нетипизированные операции Dataset. Руководство по началу работы строит их из `SparkSession`, регистрирует временное представление и выполняет SQL, чей результат снова DataFrame.

**How it works:**

- `createDataFrame` превращает локальную коллекцию плюс схему в DataFrame. `createOrReplaceTempView` даёт SQL имя для него.
- Строка SQL, например `SELECT` с `WHERE`, возвращает DataFrame. У колонок есть имена, в отличие от голого RDD кортежей.
- Руководство MLlib говорит, что этот API DataFrame теперь основной API машинного обучения. Оценщики читают колонку признаков и пишут колонку предсказания.
- Параллельная коллекция из руководства по RDD всё ещё лежит снизу, и DataFrame может отдать содержимое обратно как RDD. Новые конвейеры начинают с DataFrame, не с этого RDD.

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
<summary>Как you leverage Spark's MLlib library for machine learning in Scala?</summary>

### Как you leverage Spark's MLlib library for machine learning in Scala?

**Definition:**

MLlib — библиотека машинного обучения Spark. Её руководство говорит, что цель — практичное обучение, которое масштабируется, и что новую работу принимает API на DataFrame.

**How it works:**

- Инструменты в списке: алгоритмы (классификация, регрессия, кластеризация, коллаборативная фильтрация), признаки, конвейеры, сохранение и утилиты вроде статистики.
- Конвейер сцепляет преобразователи и оценщики. Обучение конвейера на DataFrame даёт модель, которую можно сохранить и загрузить.
- K-means — оценщик, который пишет колонку предсказания. ALS открывает `rank`, `maxIter`, `regParam`, `numBlocks` и `implicitPrefs`. Случайные леса перечислены среди других классификаторов.
- API на RDD ещё поддерживается, но в режиме сопровождения. Руководство говорит, что новых возможностей он не получит.

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
<summary>Обсудите techniques for optimizing Scala code performance.</summary>

### Обсудите techniques for optimizing Scala code performance.

**Definition:**

Самые крупные выигрыши в руководствах Scala — выбрать коллекцию под шаблон доступа, держать рекурсию в хвостовой позиции и включать параллельное выполнение только когда данных много, а оператор ассоциативен.

**How it works:**

- `Vector` — индексная неизменяемая последовательность, `List` — линейная. Индексация длинного `List` идёт по ссылкам. Книга коллекций просит выбирать по этой оси.
- `@tailrec` переписывает хвостовой вызов в цикл, чтобы длинный fold не растил стек. Нехвостовой вызов при аннотации отклоняется.
- Параллельные коллекции копируют `List` в `ParVector` и окупаются примерно от нескольких тысяч элементов. Побочные эффекты и неассоциативные fold недетерминированы.
- Итератор или `LazyList` не строят промежуточный `List`, когда нужен один проход или только префикс.

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
<summary>Что такое the role of JVM in Scala's performance?</summary>

### Что такое the role of JVM in Scala's performance?

**Definition:**

Scala компилируется в байткод JVM и работает на той же среде, что и Java. Потоки, future и мониторы из главы о конкурентности — это потоки JVM и модель памяти JVM, а не отдельная машина Scala.

**How it works:**

- Вызов метода, `Thread` и `AnyRef.synchronized` — операции JVM. Страница стандартной библиотеки для `synchronized` — крючок к этому монитору.
- Стирание типов — факт JVM, который утверждает обзор рефлексии: `List[String]` и `List[Int]` не существуют как разные классы времени выполнения.
- Оптимизация хвостового вызова через `@tailrec` — это цикл, который выпускает компилятор, потому что JVM не даёт Scala общей инструкции хвостового вызова.
- Скорость Spark на Scala — та же JVM, на которой идёт много задач. Параллелизм из руководства по RDD — процессы и потоки этой среды плюс восстановление разделов.

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
<summary>Как you manage memory effectively in Scala applications?</summary>

### Как you manage memory effectively in Scala applications?

**Definition:**

Память остаётся ограниченной, если не удерживать всю промежуточную коллекцию, если рекурсия в хвостовой позиции и если ленивую структуру заставляют считаться только на нужном префиксе.

**How it works:**

- Итератор отдаёт по одному элементу. Если удержать и источник итератора, и его `toList`, в памяти оба.
- `LazyList.range` не выделяет каждое целое до `Int.MaxValue`. `take(n).toList` выделяет n ячеек.
- `@tailrec` держит один кадр. Нехвостовой факториал, который отклоняет тур по аннотациям, клал бы кадр на каждый вызов.
- Future, замкнутый на огромную коллекцию, увозит этот захват вместе с задачей. Руководство по RDD говорит, что Spark копирует каждую использованную переменную в задачу, поэтому захватывайте маленькое значение, а не всю кучу драйвера.

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
<summary>Какие frameworks обычно используют для testing Scala applications?</summary>

### Какие frameworks обычно используют для testing Scala applications?

**Definition:**

Книга Scala называет два инструмента, которые вместе встречаются в проекте на Scala: sbt для сборки и ScalaTest для тестов. Тесты запускают через sbt, а не отдельной обвязкой.

**How it works:**

- Урок про sbt и ScalaTest представляет ScalaTest как фреймворк тестирования кода, а sbt как инструмент сборки.
- Дальнейшие уроки той книги покрывают тесты в стиле BDD и TDD, написанные этими двумя.
- Чистая функция — лёгкий случай: один вход, один ожидаемый выход, без часов и сети. Глава о чистых функциях объясняет, почему такой тест стабилен.
- Для future тест должен дождаться завершения. `onComplete` или преобразование вроде `map` из главы о конкурентности — боевая форма; тест ждёт этот future, а не спит фиксированное время.

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
<summary>Опишите the process of unit testing in Scala.</summary>

### Опишите the process of unit testing in Scala.

**Definition:**

Модульный тест вызывает одну функцию с известным входом и проверяет результат. Путь книги Scala — положить эту проверку в ScalaTest и запустить её через sbt.

**How it works:**

- Берут чистую функцию, чтобы результат зависел только от аргументов. Глава о чистых функциях относит `map` и `filter` к таким операциям.
- Проверяют саму коллекцию, а не печать. `isSorted(List(1, 2))` либо истина, либо ложь.
- Включают пустой вход. `foldLeft` на пустом списке возвращает начальное значение; `reduce` к пустой коллекции не применяется.
- sbt компилирует тест, ScalaTest сообщает о сбоях. Вводный урок — точка входа; уроки BDD и TDD показывают, как пишут спецификации.

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
<summary>Как бы вы debug an application in Scala that is not performing as expected?</summary>

### Как бы вы debug an application in Scala that is not performing as expected?

**Definition:**

Когда работа на Scala ошибочна или медленна, проверяют три причины, о которых руководства предупреждают прямо: нехвостовая рекурсия растит стек, побочный эффект внутри параллельной операции и future, результат которой никто не наблюдает.

**How it works:**

- Если стек переполняется, рекурсивный вызов не в хвостовой позиции. `@tailrec` превращает это в ошибку компиляции, а не в падение в проде. E097 срабатывает и если метод всё ещё можно переопределить.
- Если параллельные `map` или `foreach` обновляют общий `var`, руководство по параллельным коллекциям говорит, что результат недетерминирован. Прогоните ту же функцию на последовательной коллекции и сравните.
- Если результат future не приходит, вызывающий пошёл дальше без `map` или `onComplete`, либо контекст выполнения заблокирован. Глава о конкурентности показывает значение как `Future(<not completed>)`, пока тело не закончится.
- Добавьте проверку ScalaTest для чистого ядра, которое вводит урок про sbt, чтобы следующий прогон падал в тесте, а не в проде.

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
<summary>Представьте a case study where Scala is used in a machine learning pipeline.</summary>

### Представьте a case study where Scala is used in a machine learning pipeline.

**Definition:**

Небольшой конвейер — три документированные стадии: нормализовать текст методами коллекций, превратить строки в DataFrame, который запрашивает руководство Spark SQL, и обучить оценщик MLlib. Отдельная среда выполнения для этого не нужна.

**How it works:**

- Токенизация — `toLowerCase`, `filter` и `split`, затем `flatMap` по строкам.
- Эти строки становятся DataFrame через `SparkSession` — путь, который руководство SQL по началу работы называет «Creating DataFrames». SQL или фильтры колонок выбирают обучающий срез.
- MLlib обучает оценщик на колонке признаков. K-means из руководства по кластеризации пишет колонку предсказания; деревья из руководства по классификации делают то же для меток.
- Чистые функции остаются в тестах драйвера. ScalaTest, который вводит книга Scala, проверяет токенизацию до запуска кластерной работы.

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
<summary>Как Scala be used for processing large-scale graph data in machine learning?</summary>

### Как Scala be used for processing large-scale graph data in machine learning?

**Definition:**

GraphX — компонент Spark для графо-параллельных вычислений. Его руководство определяет граф как ориентированный мультиграф со свойствами на каждой вершине и каждом ребре, поверх тех же разбитых на разделы RDD.

**How it works:**

- Ориентированный мультиграф может иметь несколько рёбер между одной парой вершин; руководство приводит отношения вроде коллеги и друга.
- Вершины и рёбра несут пользовательские свойства. Алгоритмы в руководстве идут как графо-параллельные операции, а не как однопоточный обход.
- Локальный прототип — путь-зависимый `Graph` из тура Scala: узел принадлежит одному значению графа. Эта проверка на этапе компиляции и данные не разбивает.
- Когда граф больше не помещается в одну машину, граф свойств GraphX заменяет `List` соседей в памяти. Восстановление потерянного раздела — цепочка преобразований из руководства по RDD.

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
<summary>Опишите an application of Scala in natural language processing.</summary>

### Опишите an application of Scala in natural language processing.

**Definition:**

Шаг NLP на Scala, который остаётся внутри документированных API, — токенизация в коллекцию и затем подсчёт через `groupBy` или `foldLeft`. Руководство MLlib перечисляет извлечение и преобразование признаков как продолжение на кластере.

**How it works:**

- Нормализация — `toLowerCase` и разбиение по пробелам. `flatMap` сливает токены многих строк.
- Мешок слов — `groupBy(identity)` и размер либо `foldLeft` в `Map`. Слияние двух отображений — ассоциативное объединение, которое нужен параллельному fold.
- Выделение признаков MLlib превращает эти токены в колонку признаков, которую читает оценщик. Руководство отделяет признаки от алгоритмов обучения.
- Чистую функцию токенов проверяет ScalaTest. Работа DataFrame запускается, когда корпус уже разбит на разделы.

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
<summary>Обсудите the use of Scala in finance for alGorithmic trading systems.</summary>

### Обсудите the use of Scala in finance for alGorithmic trading systems.

**Definition:**

Расчёт для торговли, который руководства Scala поддерживают прямо, — чистая функция от входов к решению плюс `Future`, когда вход — медленная удалённая котировка. Общее изменяемое состояние позиции — тот случай, про который руководство по параллельным коллекциям говорит, что он не будет детерминированным.

**How it works:**

- Чистая функция цены зависит только от аргументов. Глава о чистых функциях говорит, что именно поэтому одни и те же входы дают один и тот же выход, и тест может повторить котировку.
- Две котировки, полученные как future, идут вместе. Глава о конкурентности соединяет их через `for`.
- Актор, который эта глава противопоставляет одноразовому future, — долгоживущее место, куда приходит много заявок. Сам future книгу позиций не хранит.
- Не обновляйте общий `var` из параллельного `foreach`. Сворачивайте неизменяемый список исполнений.

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
<summary>Как Scala address Scalability in large machine learning systems?</summary>

### Как Scala address Scalability in large machine learning systems?

**Definition:**

На одной машине Scala масштабирует чистую операцию над коллекцией через `.par`, когда коллекция большая. Между машинами документированный следующий шаг — разбитая на разделы коллекция из руководства по RDD и оценщики DataFrame из MLlib.

**How it works:**

- Параллельные коллекции разбивают, считают и соединяют. Они окупаются примерно от нескольких тысяч элементов и требуют ассоциативный оператор для детерминированного fold.
- RDD разбивает элементы по узлам, выполняет преобразования параллельно и пересчитывает потерянный раздел по цепочке преобразований.
- Формулировка цели MLlib — практичное машинное обучение, которое масштабируется. Конвейеры и API DataFrame — основная поверхность.
- `Future` только уносит одно вычисление с вызывающего. Обучающую выборку он не разбивает. Это разбиение делает Spark, не future.

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
<summary>Обсудите the implementation of a distributed machine learning system using Scala.</summary>

### Обсудите the implementation of a distributed machine learning system using Scala.

**Definition:**

Распределённый обучатель в этом стеке — драйвер на Scala, который строит DataFrame, обучает конвейер MLlib и позволяет Spark разбить строки. Контракт руководства по RDD — параллельные операции плюс восстановление, когда узел отказал.

**How it works:**

- Драйвер держит коллекцию Scala или путь к файлу. `parallelize` или читатель DataFrame раздаёт строки.
- Преобразования ленивы. Действие или `fit` оценщика запускает задачи.
- Конвейеры MLlib сцепляют признаки и алгоритм, затем сохраняют модель. Руководство перечисляет сохранение как полноценный инструмент.
- Замыкания, отправленные в задачи, не должны мутировать `var` драйвера. Предупреждение параллельных коллекций о побочных эффектах — то же правило на одной машине; скопированные переменные из руководства по RDD — кластерная форма.

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

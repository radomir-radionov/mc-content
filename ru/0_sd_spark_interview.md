# Apache Spark — собеседование

Ответы заполнены.

[← К обзору](./0_system_design.md)

### Related docs

- [Apache Spark — собеседование](./0_sd_spark_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Apache Spark](https://devinterview.io/questions/machine-learning-and-data-science/apache-spark-interview-questions/).

### Spark Fundamentals

<details>
<summary>Что такое Apache Spark and what are its main components?</summary>

### Что такое Apache Spark and what are its main components?

**Definition:**

Apache Spark — распределённый движок для пакетов, SQL, машинного обучения и потоков. Промежуточные данные он держит в памяти, когда они туда помещаются.

**How it works:**

- Spark Core планирует задачи, режет данные на партиции и даёт модель выполнения RDD. Приложения пишут на Scala, Java, Python, R или SQL.
- Spark SQL добавляет DataFrame и Dataset, каталог и планировщик Catalyst. Structured Streaming использует те же таблицы для инкрементальных запросов.
- MLlib даёт распределённые конвейеры обучения. GraphX — операторы над графами. Оба сидят на том же runtime.
- Менеджер кластера — standalone, YARN или Kubernetes — размещает драйвер и исполнителей. Сам Spark менеджером ресурсов не является.

**Examples:**

```text
SparkSession
  -> задача Spark SQL / DataFrame
  -> план Catalyst
  -> задачи на executor
```
</details>

<details>
<summary>Объясните how Apache Spark differs from Hadoop MapReduce.</summary>

### Объясните how Apache Spark differs from Hadoop MapReduce.

**Definition:**

Оба умеют сканировать данные на кластере. Spark держит конвейер в памяти и не ограничен одной фазой map и одной фазой reduce.

**How it works:**

- MapReduce пишет каждую стадию на диск, прежде чем начнётся следующая задача. Spark передаёт партиции в памяти и сбрасывает на диск только то, что не влезло.
- Задача MapReduce — это фиксированные map и reduce. Задача Spark — граф преобразований, включая SQL, потоки и итерации в одном приложении.
- API Spark выше уровнем: DataFrame, SQL и типизированные Dataset. MapReduce просит автора написать классы mapper и reducer.
- Потерянная партиция Spark восстанавливается по lineage. MapReduce заново читает входной кусок упавшей задачи. Оба повторяют упавшие задачи.
- У Spark остаётся задержка shuffle. Один тяжёлый shuffle данных больше памяти ближе к задаче MapReduce, чем к чистому счёту в памяти.
</details>

<details>
<summary>Опишите the concept of RDDs (Resilient Distributed Datasets) in Spark.</summary>

### Опишите the concept of RDDs (Resilient Distributed Datasets) in Spark.

**Definition:**

RDD — неизменяемая коллекция, разрезанная на партиции. Преобразования только записывают новый RDD и не выполняются, пока действие не запросит результат.

**How it works:**

- Партиция — единица параллелизма. Узкое преобразование, такое как map или filter, остаётся внутри партиции. Широкое, такое как groupByKey или join, гоняет данные по сети.
- Lineage — цепочка родительских RDD и функция, которая построила каждую партицию. Если партиция потеряна, Spark пересчитывает её от этих родителей.
- Действия — count, collect, save — запускают задачу. collect огромного RDD стягивает результат на драйвер и может его исчерпать.
- cache или persist оставляют посчитанный RDD для следующего действия. Checkpoint обрезает длинный lineage, записывая материализованную копию.
</details>

<details>
<summary>Что такое DataFrames in Spark and how do they compare to RDDs?</summary>

### Что такое DataFrames in Spark and how do they compare to RDDs?

**Definition:**

DataFrame — распределённая таблица с именами колонок и схемой. Планировщик может её переписать; RDD из непрозрачных объектов обычно нет.

**How it works:**

- Строки лежат в компактном колоночном формате, поэтому фильтр и агрегация трогают только нужные столбцы.
- Catalyst видит реляционные операторы: filter, project, join, aggregate. Он может протолкнуть предикат в источник и выбрать стратегию соединения. map на RDD — чёрный ящик.
- Тот же join, записанный через keyBy и join на RDD, длиннее и пропускает эти переписывания. Dataset добавляет типы на этапе компиляции поверх того же плана в Scala и Java.
- RDD уместен, когда у записей нет полезных колонок. DataFrame — для структурированных файлов, JDBC и SQL.

**Examples:**

```text
events.filter(col("day") == "2026-10-04").groupBy("path").count()
```

Планировщик может отбросить неиспользуемые колонки и пропустить файлы чужого дня. Эквивалентные RDD `filter` и `reduceByKey` прячут эту структуру внутри лямбд.
</details>

<details>
<summary>Что такое lazy evaluation and how does it benefit Spark computations?</summary>

### Что такое lazy evaluation and how does it benefit Spark computations?

**Definition:**

Spark записывает преобразования и выполняет их, только когда действие запрашивает результат. Всю цепочку можно спланировать как одну задачу.

**How it works:**

- map, filter, select и join строят логический план. count, show, write и collect запускают выполнение.
- Планировщик сливает узкие шаги, выбрасывает лишнюю работу и ведёт партиции потоком, чтобы данные не материализовались между каждым вызовом.
- Преобразование, которое не нужно ни одному действию, не считается. Два действия на одном некэшированном lineage считают его дважды.
- Отладка менее локальна: сбой виден на действии, а стек указывает назад по lineage.

**Examples:**

```text
df = spark.read.parquet("/events")
df2 = df.filter(df.day == "2026-10-04")   # чтения ещё нет
df2.count()                                # одна задача: чтение, фильтр, count
```
</details>

<details>
<summary>Как Spark achieve fault tolerance?</summary>

### Как Spark achieve fault tolerance?

**Definition:**

Spark считает потерянную партицию тем, что нужно пересчитать от родителей, и повторяет упавшие задачи на других исполнителях.

**How it works:**

- У каждой партиции RDD есть lineage: родительские партиции и функция, которая её породила. Неизменяемые входы делают повтор безопасным.
- Файлы shuffle записываются, чтобы следующая стадия могла забрать их снова. Потерянный выход map-стадии shuffle пересчитывается.
- checkpoint пишет материализованный RDD и забывает lineage до этой точки. Это ограничивает повтор длинной итеративной задачи.
- Драйвер может заново запустить упавшую задачу. Спекулятивное выполнение стартует вторую копию отстающей задачи. Кэш — оптимизация, а не механизм надёжности.
- Результат действия согласован с пересчётом, пока функции детерминированы. Недетерминированный map делает повтор другим.
</details>

<details>
<summary>Что такое the role of Spark Driver and Executors?</summary>

### Что такое the role of Spark Driver and Executors?

**Definition:**

Драйвер строит план и назначает задачи. Исполнители выполняют эти задачи и держат закэшированные партиции.

**How it works:**

- На драйвере живёт SparkSession. Он превращает логический план в стадии и шлёт задачи исполнителям. Туда же приезжают небольшие результаты, такие как count и строки take.
- Исполнитель — рабочий процесс с пулом потоков. Он выполняет задачи, хранит блоки shuffle и держит кэш.
- Менеджер кластера запускает исполнителей. В режиме cluster драйвер сам работает внутри кластера; в режиме client он остаётся на машине, которая отправила заявку.
- Широкое преобразование становится границей стадии. Драйвер запускает следующую стадию, только когда выходы shuffle предыдущей стадии готовы.
</details>

<details>
<summary>Как Spark's DAG (Directed Acyclic graph) Scheduler work?</summary>

### Как Spark's DAG (Directed Acyclic graph) Scheduler work?

**Definition:**

Планировщик DAG превращает lineage задачи в стадии, разрезанные по shuffle, а планировщик задач выполняет задачи внутри стадии.

**How it works:**

- Узкие зависимости сливаются в одну стадию: несколько map и filter на той же партиции становятся одной задачей.
- Широкая зависимость, такая как shuffle join или groupBy, заканчивает стадию. Следующая стадия не начнётся, пока эти выходы не появятся.
- Граф ациклический, потому что каждый RDD указывает только на более старых родителей. Планировщик идёт от действия назад к источникам.
- Упавшие задачи отправляются снова. Если в стадии слишком много отказов, стадия падает, и повтор lineage может заново выполнить предыдущую стадию.

**Examples:**

```text
read -> filter -> map          стадия 1 (без shuffle)
groupBy("user")                граница стадии
count                          стадия 2
```
</details>

### Spark Architecture and Ecosystem

<details>
<summary>Объясните the concept of a Spark Session and its puRPOse.</summary>

### Объясните the concept of a Spark Session and its puRPOse.

**Definition:**

SparkSession — входной объект одного приложения: конфигурация, каталог, SQL и чтение-запись DataFrame.

**How it works:**

- Builder задаёт имя приложения, master и настройки, затем getOrCreate возвращает сессию. Повторный вызов в том же процессе её переиспользует.
- spark.read и DataFrameWriter идут через сессию. spark.sql выполняет запрос к временным представлениям и таблицам каталога.
- Каталог создаёт и удаляет представления, кэширует таблицы и перечисляет базы. UDF регистрируют на сессии, чтобы SQL мог их вызвать.
- spark.sparkContext по-прежнему открывает старую точку входа RDD. Остановка сессии освобождает контекст и принадлежащих ей исполнителей.

**Examples:**

```text
spark = SparkSession.builder.appName("daily").getOrCreate()
spark.read.parquet("/events").createOrReplaceTempView("events")
spark.sql("SELECT path, count(*) FROM events GROUP BY path")
```
</details>

<details>
<summary>Как Spark integrate with Hadoop components like HDFS and YARN?</summary>

### Как Spark integrate with Hadoop components like HDFS and YARN?

**Definition:**

Spark читает и пишет HDFS через файловые API Hadoop и может попросить YARN разместить драйвер и исполнителей.

**How it works:**

- Пути вроде `hdfs://namenode/events` используют конфигурацию Hadoop кластера: `core-site.xml` и `hdfs-site.xml`. Те же читатели открывают Parquet, ORC и текст.
- `--master yarn` отправляет приложение в ResourceManager. YARN выделяет контейнеры; Spark решает, какие задачи в них бегут.
- Режим `cluster` запускает драйвер в контейнере YARN, ноутбук может отключиться. Режим `client` оставляет драйвер на отправителе, и исполнители должны его видеть.
- Расположение блоков HDFS по-прежнему влияет на предпочтительное место задачи — та же идея локальности данных, что у MapReduce.
</details>

<details>
<summary>Опишите the various ways to run Spark applications (cluster, client, local modes).</summary>

### Опишите the various ways to run Spark applications (cluster, client, local modes).

**Definition:**

Локальный режим выполняет всё в одном процессе. Режимы client и cluster различаются тем, где живёт драйвер, а не тем, как написана задача.

**How it works:**

- `local` и `local[n]` запускают драйвер и задачи в JVM отправителя. `local[*]` берёт по потоку на ядро. Это для разработки, не для общего кластера.
- Standalone — собственные master и worker Spark. YARN и Kubernetes — внешние менеджеры. Код приложения тот же.
- Режим client оставляет драйвер на машине, которая отправила задачу. Исполнители должны достучаться до этого драйвера, и мёртвый ноутбук убивает задачу.
- Режим cluster кладёт драйвер внутрь кластера. `spark-submit` только доставляет приложение и может завершиться. Логи драйвера тогда на кластере, а не в локальном терминале.

**Examples:**

```text
spark-submit --master local[*] app.py
spark-submit --master yarn --deploy-mode client app.py
spark-submit --master yarn --deploy-mode cluster app.py
```
</details>

<details>
<summary>Что такое Spark's data source APIs and how do you use them?</summary>

### Что такое Spark's data source APIs and how do you use them?

**Definition:**

DataFrameReader и DataFrameWriter — общий API: выбрать формат, задать параметры и загрузить или сохранить таблицу.

**How it works:**

- `spark.read.format(name)` выбирает источник: parquet, orc, json, csv, text или jdbc. Параметры задают пути, схемы и учётные данные. `load` возвращает DataFrame.
- `df.write.format(name).mode(...)` сохраняет. Режимы: ошибка, если путь есть; overwrite; append; ignore.
- Обнаружение разделов считает имена каталогов вроде `day=2026-10-04` колонками. `partitionBy` при записи создаёт эти каталоги.
- Короткое имя вроде `spark.read.parquet(path)` — тот же читатель с уже выбранным форматом. API каталога регистрирует DataFrame как таблицу metastore.

**Examples:**

```text
spark.read.format("csv").option("header", "true").schema(schema).load("/in")
df.write.mode("overwrite").partitionBy("day").parquet("/out")
```
</details>

<details>
<summary>Обсудите the role of accumulators and broadcast variables in Spark.</summary>

### Обсудите the role of accumulators and broadcast variables in Spark.

**Definition:**

Аккумуляторы собирают побочный итог с задач. Broadcast-переменные доставляют одну копию небольшого значения только для чтения на каждый исполнитель.

**How it works:**

- Аккумулятор на исполнителях только для записи, а читает его драйвер. Задачи прибавляют; `value` должен читать драйвер. Надёжны обновления внутри действий: повтор стадии может прибавить дважды, если Spark не корректирует этот тип аккумулятора.
- Broadcast копируется один раз на исполнитель и переиспользуется всеми его задачами. Задачи должны считать его неизменяемым.
- Аккумулятор годится для диагностического счётчика, например битых строк. Broadcast — для справочника, который иначе уехал бы внутри замыкания каждой задачи.
- Ни то ни другое не заменяет shuffle. Большой справочник принадлежит join, а не broadcast, который не влезает в память исполнителя.

**Examples:**

```text
bad_rows = sc.accumulator(0)

def parse(line):
    if line.count(",") != 3:
        bad_rows.add(1)
        return None
    return line.split(",")

rates = sc.broadcast({"usd": 1.0, "eur": 0.92})
amount_usd = lambda row: row.amount / rates.value[row.currency]
```
</details>

<details>
<summary>Что такое the significance of the Catalyst optimizer in Spark SQL?</summary>

### Что такое the significance of the Catalyst optimizer in Spark SQL?

**Definition:**

Catalyst превращает DataFrame или SQL-запрос в исполнимый план: разбирает его, переписывает правилами и выбирает физические операторы.

**How it works:**

- Анализатор сопоставляет имена, типы и функции с каталогом и отвергает запрос, который не проходит проверку типов.
- Логические правила сворачивают константы, проталкивают фильтры к источнику, отбрасывают лишние колонки и упрощают предикаты. Правила можно добавлять: движок — это набор переписываний дерева.
- Физическое планирование выбирает операторы, например broadcast hash join против sort-merge join, и то, как резать shuffle, используя статистику, если она есть.
- Затем сгенерированный код выполняет выбранный план. Кэш таблицы — отдельное решение о хранении. Задача Catalyst — план, а не ручной ярус памяти.

**Examples:**

```text
SELECT path FROM events WHERE day = '2026-10-04'
```

Анализ связывает `events`. Логическое правило может протолкнуть `day` в чтение раздела и отбросить все колонки, кроме `path`, ещё до скана.
</details>

<details>
<summary>Как Tungsten contribute to Spark's performance?</summary>

### Как Tungsten contribute to Spark's performance?

**Definition:**

Tungsten — слой выполнения Spark. Он хранит строки в компактном двоичном виде и гоняет сгенерированный код, чтобы процессор меньше времени тратил на объекты и сборку мусора.

**How it works:**

- Двоичный формат держит значения колонок в раскладке, удобной для кэша, а не как граф Java-объектов на каждое поле.
- Управляемая память может лежать вне кучи Java, и сборщик мусора эти байты не сканирует. Память выполнения и память хранения делят один пул и могут занимать друг у друга.
- Генерация кода на всю стадию сливает несколько операторов в один метод Java. Это убирает виртуальные вызовы и промежуточные строки между ними.
- Кодировщики переводят объекты JVM в этот двоичный вид на границе. Операторы в середине работают с байтами и не сериализуют строку на каждом шаге.

**Examples:**

Закэшированный DataFrame `(name, age)` хранится строками Tungsten. Последующие `filter` и `count` могут идти сгенерированным кодом по этой раскладке, а не выделять Java-объект на каждое поле каждой строки.
</details>

### Spark Programming and APIs

<details>
<summary>Кратко опишите Spark Core API и его features.</summary>

### Кратко опишите Spark Core API и его features.

**Definition:**

Spark Core — это программа-драйвер плюс устойчивые распределённые наборы данных. RDD — разрезанная на партиции коллекция, которую преобразуют параллельно и могут пересчитать, если узел потерян.

**How it works:**

- У каждого приложения есть драйвер, который выполняет main и запускает параллельные операции. Собирают SparkConf с именем приложения и master, затем SparkContext. В одной JVM активен только один SparkContext. Вызовите stop, прежде чем создать другой.
- В кластере master не зашивают в код. Его даёт spark-submit. local — для тестов. В оболочке sc уже есть. Второй контекст создать не получится.
- RDD берётся из файла файловой системы, которую поддерживает Hadoop, включая HDFS, или из коллекции Scala на драйвере. Преобразования описывают новый RDD. Действия запускают вычисление.
- RDD можно сохранить в памяти, чтобы следующие действия его переиспользовали. Общие переменные — это broadcast, копия на каждый узел, и аккумуляторы, к которым задачи только прибавляют.
- Приложения на Scala зависят от spark-core_2.13 той версии Spark, которую вы запускаете. Spark 4.2.0 по умолчанию собран под Scala 2.13. Доступ к HDFS ещё требует hadoop-client вашей версии HDFS. Python использует pyspark и Python 3.10 или новее для этого выпуска.

**Examples:**

```scala
val conf = new SparkConf().setAppName(appName).setMaster(master)
val sc = new SparkContext(conf)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Что такое the transformations and actions in Spark RDDs?</summary>

### Что такое the transformations and actions in Spark RDDs?

**Definition:**

Преобразование строит новый RDD и остаётся ленивым. Действие запускает вычисление и возвращает значение драйверу или записывает данные наружу.

**How it works:**

- `map` — пример преобразования в руководстве: каждый элемент проходит через функцию, и результат — новый RDD. `reduce` — пример действия: оно агрегирует и возвращает итог драйверу.
- На `map` ничего не выполняется. Spark запоминает родословную. Именно `reduce` режет работу на задачи. Каждая машина может отобразить свою партицию и локально свернуть её, и только этот ответ уходит драйверу.
- Тот же ленивый RDD пересчитывается на каждое следующее действие, пока вы не вызовете `persist` или `cache`.
- Другие преобразования в руководстве: `filter`, `flatMap`, `reduceByKey`, `join`, `repartition` и `coalesce`. Другие действия: `collect`, `count`, `take`, `takeOrdered`, `saveAsTextFile` и `foreach`.
- `collect` тянет весь RDD на драйвер и может исчерпать его память. `take` — более безопасный способ посмотреть несколько строк. `foreach`, который меняет переменную драйвера, на кластере не определён. Используйте аккумулятор.

**Examples:**

```python
lines = sc.textFile("data.txt")
lineLengths = lines.map(lambda s: len(s))
totalLength = lineLengths.reduce(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Как you handle partitioning in Spark to optimize performance?</summary>

### Как you handle partitioning in Spark to optimize performance?

**Definition:**

Задача работает на одной партиции. Число партиций меняют, только когда shuffle или фильтр сделали текущее разбиение расточительным.

**How it works:**

- `repartition(numPartitions)` случайно перетасовывает каждую строку, чтобы партиций стало больше или меньше. Руководство говорит, что это всегда гоняет все данные по сети.
- `coalesce(numPartitions)` уменьшает число партиций. Руководство указывает его на случай, когда большой набор уже отфильтрован.
- `repartitionAndSortWithinPartitions(partitioner)` за один shuffle и переразбивает, и сортирует внутри каждой партиции. Это дешевле, чем сначала repartition, а потом сортировка.
- Агрегации по ключу принимают необязательный `numPartitions`: `reduceByKey`, `aggregateByKey`, `groupByKey`, `join`. Это число reduce-задач shuffle.
- Shuffle пишет промежуточные файлы и может сбросить таблицы сортировки на диск. Меньше, но полнее партиций после фильтра избегают пустых задач. Больше партиций помогает, только когда узкое место — одна партиция.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Проиллюстрируйте the differences between Map and flatMap functions in Spark.</summary>

### Проиллюстрируйте the differences between Map and flatMap functions in Spark.

**Definition:**

`map` превращает каждый входной элемент в один выходной. `flatMap` превращает каждый входной элемент в ноль или больше выходных, и результаты сплющиваются в один RDD.

**How it works:**

- Руководство RDD определяет `map(func)` как новый набор, где каждый элемент пропущен через `func`.
- `flatMap(func)` похож, но `func` возвращает последовательность, а не один элемент, поэтому один вход может стать многими выходами или ни одним.
- Строка текста, отображённая в её длину, остаётся одной строкой. Та же строка, раскрытая разбиением по пробелам, становится строкой на каждое слово.
- Оба — ленивые преобразования. Ни одно не возвращает значение драйверу. Их запускает действие вроде `count` или `reduce`.
- Structured Streaming использует ту же идею на строке из сокета: `flatMap` (Scala) или `split` плюс `explode` (Python) превращает одну строку в строки слов до `groupBy` и `count`.

**Examples:**

```python
lengths = lines.map(lambda s: len(s))          # one int per line
words = lines.flatMap(lambda s: s.split(" "))  # zero or more words per line
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Что такое Key-Value pair RDDs, and when would you use them?</summary>

### Что такое Key-Value pair RDDs, and when would you use them?

**Definition:**

RDD пар — это RDD пар. Операции над парами — те, что группируют или агрегируют по ключу, и это операции shuffle.

**How it works:**

- В Python пары — кортежи вроде `(1, 2)`. В Scala это `Tuple2`. В Java их строят через `mapToPair` в `JavaPairRDD`.
- Подсчёт в руководстве — `map` в `(line, 1)` и затем `reduceByKey`. `sortByKey` упорядочивает эти пары. `collect` приносит результат драйверу.
- Пары нужны, когда вопрос — на ключ: счётчик, join, cogroup. `groupByKey` возвращает `(K, Iterable)`. Для суммы или среднего руководство говорит, что `reduceByKey` или `aggregateByKey` работают лучше.
- Своему ключу нужны `equals` и совпадающий `hashCode`.
- Не используйте shuffle пар, чтобы менять счётчик в замыкании. Этот счётчик — копия на executor.

**When to use:**

Используйте RDD пар, когда агрегируете, сортируете или соединяете по ключу. Обычные RDD оставляйте, когда каждая строка независима.

**Examples:**

```python
pairs = lines.map(lambda s: (s, 1))
counts = pairs.reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Объясните how to perform a join operation in Spark.</summary>

### Объясните how to perform a join operation in Spark.

**Definition:**

На RDD пар `join` возвращает `(K, (V, W))` для каждой пары значений с общим ключом. Это shuffle.

**How it works:**

- Руководство определяет `join(otherDataset, [numPartitions])` на наборах типов `(K, V)` и `(K, W)`. Внешние формы — `leftOuterJoin`, `rightOuterJoin` и `fullOuterJoin`.
- `cogroup` (также `groupWith`) возвращает `(K, (Iterable, Iterable))` вместо одной пары на совпадение.
- Необязательный `numPartitions` задаёт число reduce-задач. Ключи должны встретиться, поэтому это копирует данные по сети. Раздел про shuffle перечисляет `join` и `cogroup` среди операций, которые делают shuffle.
- Своему ключу всё ещё нужны совпадающие `equals` и `hashCode`, иначе равные ключи разминутся.
- `cartesian` — другой конструктор пар, и он возвращает каждую пару элементов, а не пары с общим ключом. Это не join.

**Examples:**

```python
joined = left.join(right)  # (K, V) join (K, W) -> (K, (V, W))
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Подробно опишите, как работают window functions в Spark SQL.</summary>

### Подробно опишите, как работают window functions в Spark SQL.

**Definition:**

Страница знакомства со Spark SQL отделяет скалярную функцию, одно значение на строку, от агрегатной, одно значение на группу. Окно Structured Streaming — это такая группа, заданная по времени события.

**How it works:**

- Встроенные агрегаты включают `count()`, `count_distinct()`, `avg()`, `max()` и `min()`. Пользователь может задать и свой агрегат. Документированная форма DataFrame — `groupBy`, затем `count`, как в `df.groupBy("age").count()`.
- В Structured Streaming окно по времени события — группа по столбцу времени события. `window(timestamp, "10 minutes", "5 minutes")` — вызов в примере watermark, внутри `groupBy` вместе с другим столбцом, затем `count()`.
- Строка может принадлежать больше чем одному окну. Страница знакомства со стримингом говорит, что каждое временное окно — группа.
- Результат — всё ещё таблица, которую Spark обновляет. Режим вывода решает, пишете ли вы весь результат (`complete`), только новые строки (`append`) или только изменившиеся (`update`).
- Это не скаляр на строку. Скаляр возвращает одно значение для этой строки. Агрегат окна возвращает одно значение для группы.

**Sources:**

- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

### Spark Streaming and Real-Time Processing

<details>
<summary>Опишите the concept of discretized streams (DStreams) in Spark.</summary>

### Опишите the concept of discretized streams (DStreams) in Spark.

**Definition:**

Документация стриминга Spark 4.2 задаёт Structured Streaming: живой поток — неограниченная таблица, и движок выполняет запрос по нарастающей. Это модель, которую задают эти страницы.

**How it works:**

- Каждый пришедший элемент — новая строка входной таблицы. На каждом триггере движок обновляет таблицу результата. Весь вход он не хранит. Он хранит промежуточное состояние, которое нужно результату, например текущие счётчики.
- По умолчанию движок — микропакеты: маленькие пакетные задания, сквозная задержка в обзоре — до 100 миллисекунд, и exactly-once, когда источник может повторить смещения, а приёмник идемпотентен. Checkpoint и упреждающий журнал записывают диапазон смещений каждого триггера.
- С Spark 2.3 есть ещё непрерывная обработка. Документация говорит о задержках до 1 миллисекунды с гарантией at-least-once. Операции Dataset те же. Меняется режим.
- Страница знакомства противопоставляет это движкам, где текущую агрегацию держите вы и сами рассуждаете про at-least-once или at-most-once. Здесь таблицу результата обновляет Spark.
- Подсчёт слов из сокета — конкретная форма: `readStream.format("socket")`, разбить строку, `groupBy` и `count`, затем `writeStream.outputMode("complete")`.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Как Structured Streaming differ from DStream-BASEd streaming?</summary>

### Как Structured Streaming differ from DStream-BASEd streaming?

**Definition:**

Structured Streaming выполняет запрос пакетной формы над неограниченной таблицей на движке Spark SQL, и Spark поддерживает результат актуальным. Вы пишете `groupBy` и `count`, а не вручную хранимый текущий итог.

**How it works:**

- Обзор говорит, что потоковое вычисление выражают так же, как пакетное. Движок SQL выполняет его по нарастающей. Языки — API Dataset и DataFrame.
- Исполнение по умолчанию — микропакеты с exactly-once, когда у источников есть смещения, а приёмники идемпотентны. Непрерывная обработка с версии 2.3 — режим малой задержки, и она документирована как at-least-once. Запрос для смены режима не переписывают.
- Опоздавшие данные — это столбец времени события плюс watermark, а не отдельный тип потока. Движок может обновить старый агрегат и может выбросить состояние, когда watermark прошёл окно.
- Вывод — режим таблицы результата: complete, append или update. Append применим, только когда существующие строки результата не должны меняться.
- Отказоустойчивость — checkpoint и упреждающие журналы диапазона смещений плюс повторяемые источники и идемпотентные приёмники. Пользователь не хранит агрегат во внешней переменной.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
</details>

<details>
<summary>Что такое the fault-tolerance mechanisms in Spark Streaming?</summary>

### Что такое the fault-tolerance mechanisms in Spark Streaming?

**Definition:**

Structured Streaming записывает диапазон смещений каждого триггера в checkpoint и упреждающий журнал, затем продолжает с этого прогресса. Потерянные партиции RDD пересчитываются из родословной.

**How it works:**

- У источников предполагаются смещения, по аналогии со смещением Kafka. Движок записывает обрабатываемый диапазон. Приёмники сделаны идемпотентными, чтобы повтор не применился дважды. Вместе это документированный путь сквозного exactly-once.
- Непрерывная обработка — исключение, которое называет обзор: at-least-once, не exactly-once.
- Внутри задания RDD потерянная партиция сохранённого RDD пересчитывается из преобразований, которые её создали. Файлы shuffle сохраняют, чтобы родословная не пересобирала их, пока на эти RDD есть ссылки.
- Динамическое выделение не должно удалять файлы shuffle, когда executor уходит. Страница планирования требует внешний shuffle-сервис, отслеживание shuffle или вывод из эксплуатации, который сохраняет блоки shuffle.
- Переменная драйвера, увеличенная внутри `foreach`, — не отказоустойчивый счётчик. В замыкании лежит копия. Аккумулятор — механизм руководства для агрегата на весь кластер.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>Как you handle late data and stateful processing in Spark Streaming?</summary>

### Как you handle late data and stateful processing in Spark Streaming?

**Definition:**

Время события — столбец. Watermark — насколько позади этого столбца вы ещё принимаете опоздавшие строки. Состояние окна держат, пока watermark не пройдёт конец окна.

**How it works:**

- Страница знакомства определяет время события как время внутри данных, а не время, когда Spark получил строку. Агрегация по окну — группировка по этому столбцу. Строка может попасть больше чем в одно окно.
- `withWatermark(eventTime, delayThreshold)` отслеживает момент, до которого больше не ждут опоздавших данных. Python API считает watermark из максимального времени события по партициям минус задержка. Из-за цены согласования использованный watermark гарантированно отстаёт хотя бы на эту задержку.
- Для окна, которое кончается в `T`, страница API держит состояние и позволяет опоздавшие обновления, пока максимальное время события минус порог опоздания не прошло `T`. Строки позже порога начинают отбрасываться. Строки внутри порога всё ещё обновляют агрегат.
- Эта граница позволяет запросу работать днями, не храня каждый старый счётчик. Режим update обновляет окно, пока оно не старше watermark.
- Режим вывода всё ещё важен. Append может выдать окно, когда оно завершено. Complete переписывает таблицу результата.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

<details>
<summary>Объясните watermarks and windowing operations in Structured Streaming.</summary>

### Объясните watermarks and windowing operations in Structured Streaming.

**Definition:**

Окно группирует строки по интервалу времени события. Watermark — порог опоздания по этому столбцу, чтобы старое состояние окна можно было выбросить.

**How it works:**

- `withWatermark("timestamp", "10 minutes")` — документированный вызов. Затем `groupBy(window(timestamp, "10 minutes", "5 minutes"), value).count()`.
- Значение watermark — максимальное время события по партициям минус порог задержки. Оно может отставать сильнее порога. Некоторые записи позже порога всё ещё могут быть обработаны. API говорит, что гарантия — не меньше задержки, а не жёсткая отсечка в каждом случае.
- Правило страницы API для окна, которое кончается в `T`: держать состояние, пока `максимальное время события - порог опоздания` ещё не больше `T`. После этого более поздние строки этого окна начинают отбрасываться.
- Цели, которые называет API: знать, когда агрегат окна можно завершить для режимов вывода, которые не допускают обновлений, и ограничить состояние текущих агрегаций.
- Это есть только у Structured Streaming, как говорит примечание Python API.

**Examples:**

```python
from pyspark.sql.functions import window
query = (df
    .withWatermark("timestamp", "10 minutes")
    .groupBy(window(df.timestamp, "10 minutes", "5 minutes"), df.value)
    .count())
```

**Sources:**

- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

### Performance Tuning and Optimization

<details>
<summary>Что такое some common Spark performance issues and how do you resolve them?</summary>

### Что такое some common Spark performance issues and how do you resolve them?

**Definition:**

Дорогая часть — shuffle, горячая партиция и пересчёт RDD, за который вы уже заплатили. Руководство называет эти цены и вызовы, которые их избегают.

**How it works:**

- Shuffle делает дисковый ввод-вывод, сериализацию и сетевой ввод-вывод. `reduceByKey` и `aggregateByKey` строят таблицы на стороне map. Другие операции by-key строят их на стороне reduce. Если они не влезают, Spark сбрасывает их на диск и тратит больше на ввод-вывод и сборку мусора.
- `groupByKey` с последующей суммой перевозит каждое значение. `reduceByKey` сначала агрегирует. Так говорит таблица преобразований.
- `repartition` всегда перетасовывает всё. После фильтра `coalesce` уменьшает партиции без этой полной перетасовки.
- RDD, который используют дважды и не сохраняют, вычисляется дважды. `persist` держит партиции. Если они влезают, `MEMORY_ONLY` — самый эффективный по CPU уровень.
- `collect` большого RDD исчерпывает драйвер. `take` — нет. Замыкание, которое обновляет счётчик драйвера, не агрегирует по executor.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Как you minimize data shuffling in Spark?</summary>

### Как you minimize data shuffling in Spark?

**Definition:**

Делайте shuffle, только когда ключи должны встретиться. Агрегируйте до shuffle и не переразбивайте набор, который уже разрезан удачно.

**How it works:**

- `reduceByKey` и `aggregateByKey` соединяют значения на стороне map. `groupByKey` — нет. Руководство говорит предпочитать первые два для суммы или похожего агрегата.
- `join` и `cogroup` делают shuffle, потому что совпадения должны оказаться вместе. Передавайте `numPartitions`, когда нужно другое число reduce-задач, а не по привычке.
- `coalesce` убирает партиции после фильтра. `repartition` — вызов, который всегда двигает каждую строку.
- `repartitionAndSortWithinPartitions` вкладывает сортировку в shuffle, который вы и так собирались сделать.
- Узкие шаги вроде `map` и `filter` не нуждаются в shuffle. Они работают внутри партиции, которую действие уже запланировало.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Обсудите the importance and methods of caching/persistence in Spark.</summary>

### Обсудите the importance and methods of caching/persistence in Spark.

**Definition:**

Сохранение держит партиции в памяти между действиями, чтобы следующее действие их не пересчитывало. Руководство говорит, что поздние действия часто быстрее более чем в десять раз.

**How it works:**

- `cache()` — это `MEMORY_ONLY`: десериализованные объекты Java. Партиции, которые не влезли, пересчитываются. `persist(StorageLevel)` выбирает другой уровень.
- `MEMORY_AND_DISK` кладёт избыток на диск. `MEMORY_ONLY_SER` хранит один сериализованный массив байтов на партицию в Java и Scala: меньше места и больше CPU на чтение. Python всегда использует pickle, поэтому сериализованный уровень там не другой формат.
- Уровни с суффиксом `_2` реплицируют каждую партицию на два узла. Любой уровень может пересчитать потерянную партицию. Репликация позволяет задачам продолжаться без ожидания.
- Руководство говорит оставаться на `MEMORY_ONLY`, когда данные влезают. Не сбрасывайте на диск, если пересчёт не дорог или функция не отфильтровала много.
- Spark также сохраняет некоторые данные shuffle, например для `reduceByKey`, даже без `persist`. Вызывайте `persist` сами, если переиспользуете результат. LRU выбрасывает старые партиции. `unpersist()` выбрасывает один RDD. `blocking=true` ждёт, пока память освободится.

**Examples:**

```scala
lineLengths.persist()
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Объясните how you would monitor and log a Spark application.</summary>

### Объясните how you would monitor and log a Spark application.

**Definition:**

Драйвер видит результат действия. Печать executor не появляется на драйвере. Число на весь кластер должно быть аккумулятором, а не переменной из замыкания.

**How it works:**

- `rdd.foreach(println)` в режиме кластера пишет в stdout executor. Stdout драйвера остаётся пустым. `take(n)` или небольшой `collect` возвращает строки, а `collect` всего может исчерпать драйвер.
- Счётчик, увеличенный внутри `foreach`, остаётся нулём на драйвере, потому что каждый executor получил копию. Замена из руководства — аккумулятор.
- Внутри одного приложения задания — это действия. Планировщик выполняет их FIFO, если `spark.scheduler.mode` не `FAIR`. Пулы задают на поток через `spark.scheduler.pool`. Так одновременные запросы делят один SparkContext.
- PySpark по умолчанию не выравнивает потоки Python с потоками JVM, поэтому `setJobGroup` в другом потоке Python ненадёжен. Страница рекомендует `pyspark.InheritableThread`.
- Описатель Structured Streaming — это `StreamingQuery` из `writeStream.start()`. `awaitTermination()` удерживает процесс. Приёмник console печатает каждый пакет. Это журнал примера.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Что такое the role of partitioner objects in Spark and how do they affect performance?</summary>

### Что такое the role of partitioner objects in Spark and how do they affect performance?

**Definition:**

Partitioner решает, какой ключ попадает в какую партицию. `repartitionAndSortWithinPartitions` принимает его, чтобы shuffle и сортировка внутри партиции произошли вместе.

**How it works:**

- `repartitionAndSortWithinPartitions(partitioner)` в таблице преобразований переразбивает по этому partitioner и сортирует записи по ключу внутри каждой получившейся партиции. Руководство говорит, что это эффективнее, чем `repartition` плюс поздняя сортировка, потому что сортировку можно вложить в shuffle.
- `reduceByKey`, `groupByKey` и `join` принимают `numPartitions`, а не объект partitioner. Это целое — всё ещё число reduce-задач, а параллельность по умолчанию следует родительскому RDD.
- Ключи оказываются вместе только после shuffle. До этого значения одного ключа могут лежать на разных машинах. Плохое разбиение оставляет одной reduce-задаче большую часть строк. Цена сброса на диск и сборки мусора в разделе про shuffle — эта задача.
- Свои ключи должны вместе реализовать `equals` и `hashCode`. Иначе партиция, в которую хешируется ключ, не совпадёт с ключом, который вы считаете равным.
- `repartition` без ключа перетасовывает случайно. Это выравнивает размер. Строки одного ключа вместе он не держит.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Spark MLlib (Machine Learning Library)

<details>
<summary>Что такое the main features of Spark MLlib?</summary>

### Что такое the main features of Spark MLlib?

**Definition:**

MLlib — библиотека машинного обучения Spark. Основной API с Spark 2.0 — API DataFrame в `spark.ml`. API RDD в `spark.mllib` в режиме сопровождения.

**How it works:**

- Руководство перечисляет алгоритмы классификации, регрессии, кластеризации и коллаборативной фильтрации; подготовку признаков; конвейеры для сборки, оценки и подбора; сохранение алгоритмов, моделей и конвейеров; утилиты линейной алгебры и статистики.
- DataFrame — причина перехода: источники данных, SQL, Tungsten и Catalyst и один API на языках. Ни один API не объявлен устаревшим. Новые возможности идут в DataFrame. API RDD всё ещё получает исправления.
- Численные расчёты идут через Breeze и `dev.ludovic.netlib`. Они могут вызвать Intel MKL или OpenBLAS, если те установлены. Spark эти нативные библиотеки не поставляет. Без них вы получаете предупреждение и чистую реализацию на JVM. Python нужен NumPy 1.21 или новее.
- На той же странице среди заметного в Spark 3.0: многостолбцовые `StringIndexer` и `Binarizer`, веса примеров у деревьев и k-means, `RobustScaler`, factorization machines и два новых оценщика.
- «Spark ML» — не официальное имя. Так называют пакет `org.apache.spark.ml`.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Как Spark MLlib handle machine learning pipelines?</summary>

### Как Spark MLlib handle machine learning pipelines?

**Definition:**

Конвейер — упорядоченный список стадий. Transformer меняет DataFrame. Estimator обучается на DataFrame и порождает transformer. `Pipeline.fit` выполняет их по порядку и возвращает `PipelineModel`.

**How it works:**

- Пример потока работ: разрезать текст на слова, захэшировать слова в вектор признаков, затем выучить модель. `Tokenizer` и `HashingTF` — transformer. `LogisticRegression` — estimator.
- На fit каждый transformer вызывает `transform` у DataFrame. Каждый estimator вызывает `fit`, и получившаяся модель становится стадией `PipelineModel`. На тестовых данных `PipelineModel.transform` выполняет только transformer, поэтому обучение и тест делят одни и те же шаги признаков.
- Стадии должны быть уникальными экземплярами, потому что у каждой есть ID. DAG допустим, если стадии перечислены в топологическом порядке. Граф выводится из имён входных и выходных столбцов. Проверка типов происходит во время выполнения по схеме DataFrame.
- Параметры — это сеттеры (`lr.setMaxIter(10)`) или `ParamMap`, переданный в `fit` или `transform`. `ParamMap` перекрывает сеттеры.
- С Spark 2.3 API DataFrame умеет сохранять и загружать конвейеры между Scala, Java и Python. Младшие и патч-версии обратно совместимы по загрузке. Старшие версии — по возможности. У R другой формат.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Опишите a use case for MLlib's collaborative filtering alGorithms.</summary>

### Опишите a use case for MLlib's collaborative filtering alGorithms.

**Definition:**

MLlib перечисляет коллаборативную фильтрацию рядом с классификацией, регрессией и кластеризацией. API DataFrame — то, как любой из этих алгоритмов видит обучающие строки.

**How it works:**

- Набор данных — DataFrame. Руководство по конвейерам говорит, что DataFrame может держать метки, векторы признаков, текст и предсказания в именованных столбцах, плюс типы ML `Vector`.
- Модель фильтрации — всё ещё estimator, если она обучается: `fit` на DataFrame истории порождает transformer, а `transform` добавляет столбец предсказания. Это тот же контракт, что у `LogisticRegression`.
- Положите шаги признаков и estimator в один `Pipeline`, чтобы одни и те же столбцы строились при обучении и при обслуживании. Сохраните `PipelineModel`. Младшие версии Spark загружают его обратно.
- API RDD в `spark.mllib` не получит новых алгоритмов. Используйте `spark.ml`.
- Полученные здесь страницы называют коллаборативную фильтрацию семейством алгоритмов, которое предоставляется. Отдельный пример кода для неё они не показывают. Контракт конвейера выше — та часть, которую они задают.

**When to use:**

Используйте это, когда обучающие данные — история известных предпочтений в DataFrame, а на выходе нужен столбец предсказания из сохранённого конвейера.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

<details>
<summary>Объясните the difference between Spark MLlib and external machine learning libraries.</summary>

### Объясните the difference between Spark MLlib and external machine learning libraries.

**Definition:**

Основной API MLlib — конвейеры DataFrame в `spark.ml`, и выполняет их Spark. Внешние численные библиотеки — необязательные нативные слои под этим, а не второй ML API.

**How it works:**

- `spark.ml` даёт один набор estimator и transformer на языках, на DataFrame, с Catalyst и Tungsten под ними. `spark.mllib` на RDD остаётся в сопровождении и получает только исправления.
- Breeze и `dev.ludovic.netlib` делают линейную алгебру. Если на машине есть Intel MKL или OpenBLAS, эти пакеты могут их вызвать. Spark нативную библиотеку не распространяет. Иначе работает реализация JVM и пишет в журнал неудачную загрузку `JNIBLAS`.
- Python MLlib нужен NumPy 1.21 или новее. Это зависимость MLlib, а не замена estimator на DataFrame.
- Стадия конвейера должна быть transformer или estimator MLlib, если она внутри `Pipeline`. `fit` и `transform` в текущем API без состояния.
- Сохранение — собственный формат MLlib. Модель, сохранённая другой библиотекой, — не то, что документированно читает загрузка `PipelineModel`.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

### Coding Challenges

<details>
<summary>Напишите a Scala/Python Spark code snippet that reads a CSV file and calculates the average of a column.</summary>

### Напишите a Scala/Python Spark code snippet that reads a CSV file and calculates the average of a column.

**Definition:**

`spark.read.csv` загружает файл или каталог CSV. `avg()` — встроенный агрегат, который возвращает одно значение для группы строк.

**How it works:**

- Разделитель по умолчанию — запятая. `option("header", True)` берёт первую строку как имена столбцов. `option("delimiter", ";")` подменяет разделитель. `inferSchema` по умолчанию false и стоит лишнего прохода, если его включить.
- Чтение папки ожидает только CSV. Страница CSV показывает неверную схему, когда в том же каталоге лежат другие файлы.
- Когда DataFrame есть, зарегистрируйте временное представление и выполните SQL либо используйте агрегат DataFrame. Страница знакомства перечисляет `avg()` вместе с `count()`, `max()` и `min()`.
- При `header` false столбцы называются `_c0`, `_c1`. Усредняйте тот столбец, который вы реально назвали.
- Запись — `dataframe.write.csv`. Это папка part-файлов плюс `_SUCCESS`, а не один документ CSV.

**Examples:**

```python
df = spark.read.option("header", True).option("inferSchema", True).csv(path)
df.createOrReplaceTempView("people")
spark.sql("SELECT avg(age) AS avg_age FROM people")
```

**Sources:**

- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Реализуйте a Spark streaming application that counts words in text data received from a socket.</summary>

### Реализуйте a Spark streaming application that counts words in text data received from a socket.

**Definition:**

Быстрый пример Structured Streaming читает TCP-сокет, режет строки на слова и пишет текущий счётчик в режиме complete.

**How it works:**

- `spark.readStream.format("socket").option("host", "localhost").option("port", 9999)` — входная таблица. Столбец — `value`. Ничего не читается до `start()`.
- Python режет через `split` и `explode`. Scala использует `flatMap` на `Dataset[String]`. Затем `groupBy` и `count` — таблица результата.
- `writeStream.outputMode("complete").format("console").start()` печатает всю таблицу счётчиков на каждом триггере. `awaitTermination()` держит процесс.
- Сервер примера — `nc -lk 9999`. Путь запуска Python в руководстве — `./bin/spark-submit examples/src/main/python/sql/streaming/structured_network_wordcount.py localhost 9999`.
- Режим complete переписывает результат. Движок хранит счётчики, а не каждую исходную строку.

**Examples:**

```python
lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
words = lines.select(explode(split(lines.value, " ")).alias("word"))
query = words.groupBy("word").count().writeStream.outputMode("complete").format("console").start()
```

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Напишите код: a Spark SQL query that finds the top 3 most occurring words in a DataFrame.</summary>

### Напишите код: a Spark SQL query that finds the top 3 most occurring words in a DataFrame.

**Definition:**

Count — это агрегат группы. `takeOrdered` на RDD пар возвращает первые n элементов при заданном порядке. Это документированный способ оставить только вершину небольшого результата.

**How it works:**

- Соберите пары слов способом из руководства RDD или `groupBy("word").count()` способом DataFrame. Оба дают счётчик на слово.
- `collect` каждого счётчика может исчерпать драйвер. `takeOrdered(3, ordering)` возвращает три элемента. Таблица действий определяет это как первые n в естественном порядке или с компаратором.
- Временное SQL-представление — `createOrReplaceTempView`, затем `spark.sql`. Страница знакомства так выполняет `SELECT`. Страница агрегатов перечисляет `count()`.
- Тяжёлый подсчёт делайте в кластере. Возвращайте три строки, а не весь словарь.
- Счётчики стриминга в режиме complete — таблица результата, а не top-3 сами по себе. Эту таблицу всё ещё надо упорядочить и оставить три.

**Examples:**

```python
counts = words.map(lambda w: (w, 1)).reduceByKey(lambda a, b: a + b)
top3 = counts.takeOrdered(3, key=lambda kv: -kv[1])
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Создайте a Spark job that joins two datasets on a key and summarizes data.</summary>

### Создайте a Spark job that joins two datasets on a key and summarizes data.

**Definition:**

Сделайте обе стороны RDD пар, соедините их через `join`, затем `reduceByKey`, чтобы сводка соединялась до того, как поедут полные списки значений.

**How it works:**

- `join` на `(K, V)` и `(K, W)` даёт `(K, (V, W))` для каждого совпадения. `numPartitions` задаёт reduce-задачи этого shuffle.
- Сумма после join не должна делать `groupByKey` соединённых строк. Отобразите в `(key, number)` и `reduceByKey`.
- Внешние join есть, когда отсутствующая сторона всё равно должна появиться: `leftOuterJoin`, `rightOuterJoin`, `fullOuterJoin`.
- Сохраните сторону, которую соединяете больше одного раза. Иначе каждое действие пересчитает её.
- `groupBy` плюс `count` у DataFrame — SQL-форма сводки, когда результат join уже DataFrame. Сам join RDD задан на RDD пар.

**Examples:**

```python
joined = orders.join(customers)              # (id, (order, customer))
totals = joined.map(lambda kv: (kv[0], kv[1][0])).reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Напишите a Python function using PySpark to filter out records with null values in a specific column.</summary>

### Напишите a Python function using PySpark to filter out records with null values in a specific column.

**Definition:**

Null — значение, которое DataFrame сохраняет. Пример people показывает `age` null в `show()` и в `groupBy("age").count()`. Убрать этот столбец из результата — это `WHERE` на временном представлении.

**How it works:**

- И `spark.read.json`, и `spark.read.csv` дают DataFrame. При `header` false столбцы CSV — это `_c0` и далее. Назовите столбец, прежде чем его фильтровать.
- `createOrReplaceTempView("people")`, затем `spark.sql` — документированный способ выполнить запрос. `WHERE age IS NOT NULL` убирает null-группу, которую пример `groupBy` иначе посчитает.
- `df.filter(df["age"] > 21)` — документированный фильтр DataFrame. Сравнение само по себе не описывает удаление null. Это делает предикат SQL.
- `inferSchema` у CSV — лишний проход, и по умолчанию он выключен. Строковый столбец не сравнится как число, пока схема не скажет, что он числовой.
- Не делайте `collect` и не выбрасывайте null на драйвере. Фильтр принадлежит запросу.

**Examples:**

```python
def without_null_age(df):
    df.createOrReplaceTempView("people")
    return spark.sql("SELECT * FROM people WHERE age IS NOT NULL")
```

**Sources:**

- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
</details>

<details>
<summary>Реализуйте a logistic regression model in Spark MLlib.</summary>

### Реализуйте a logistic regression model in Spark MLlib.

**Definition:**

`LogisticRegression` — estimator. `fit` на DataFrame со столбцами `label` и `features` возвращает модель, и `transform` этой модели добавляет предсказание.

**How it works:**

- Руководство по конвейерам строит его как `LogisticRegression(maxIter=10, regParam=0.01)` и вызывает `lr.fit(training)`.
- Обучающие строки в примере — `(label, Vectors.dense(...))` с именами столбцов `label` и `features`. `transform` читает `features`.
- `ParamMap` может перекрыть сеттеры, включая `probabilityCol`. Пример переименовывает этот столбец в `myProbability` и затем выбирает `features`, `label`, `myProbability` и `prediction`.
- В конвейере логистическая регрессия — стадия estimator после `Tokenizer` и `HashingTF`. `Pipeline.fit` порождает `PipelineModel`, чьи стадии — все transformer.
- Этот класс живёт в `pyspark.ml.classification`. Пакет RDD `spark.mllib` в режиме сопровождения.

**Examples:**

```python
from pyspark.ml.classification import LogisticRegression
lr = LogisticRegression(maxIter=10, regParam=0.01)
model = lr.fit(training)  # columns: label, features
prediction = model.transform(test)
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Создайте a Spark Streaming application to aggregate event data by time windows.</summary>

### Создайте a Spark Streaming application to aggregate event data by time windows.

**Definition:**

Группируйте поток по окну времени события и задайте watermark, чтобы состояние окон не росло без границы.

**How it works:**

- Время события — столбец каждой строки. `window(df.timestamp, "10 minutes", "5 minutes")` внутри `groupBy`, плюс другой столбец, затем `count()` — это пример.
- `withWatermark("timestamp", "10 minutes")` нужно объявить, чтобы движок мог выбросить состояние окон старше порога опоздания.
- Выберите режим вывода, который агрегат допускает. Complete пишет всю таблицу результата. Update пишет строки, которые изменились. Append ждёт, пока существующие строки не должны меняться. Для этого и завершают окно.
- Запрос не выполняется до `writeStream.start()`. Checkpoint — то, что позволяет перезапуску продолжиться с записанных смещений.
- Время получения — не этот столбец. Страница знакомства говорит использовать время в данных, когда вопрос — события в минуту.

**Examples:**

```python
from pyspark.sql.functions import window
counts = (events
    .withWatermark("timestamp", "10 minutes")
    .groupBy(window("timestamp", "10 minutes", "5 minutes"))
    .count())
```

**Sources:**

- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
</details>

<details>
<summary>Напишите Spark code to calculate the PageRank of a simple website link graph.</summary>

### Напишите Spark code to calculate the PageRank of a simple website link graph.

**Definition:**

Граф ссылок в API RDD — это пары. `join` совмещает ранг страницы с её исходящими ссылками, а `reduceByKey` суммирует то, что возвращается к каждой цели. Это операции, которые руководство реально задаёт для такой формы.

**How it works:**

- Представьте ссылку как `(source, destination)`, а ранг как `(page, rank)`. Оба — RDD пар.
- `join` даёт `(page, (rank, destination))`, когда ключ — страница-источник. Отобразите это во вклад, направленный в destination, затем `reduceByKey`, чтобы сложить вклады по каждой цели.
- `groupByKey` перевёз бы каждый вклад. `reduceByKey` сначала их соединяет. Для суммы руководство предпочитает это.
- Каждая итерация — действие или позднее использование рангов. `persist` у RDD ссылок. Он не меняется. Без persist каждое действие пересчитает его.
- `collect` рангов — только когда множество страниц достаточно мало для драйвера. Иначе `saveAsTextFile`.

**Examples:**

```python
joined = ranks.join(links)  # (page, (rank, neighbors))
contribs = joined.flatMap(lambda kv: ((dest, kv[1][0] / len(kv[1][1])) for dest in kv[1][1]))
nextRanks = contribs.reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Case Studies and Scenario-Based Questions

<details>
<summary>Как бы вы build a recommender system in Spark?</summary>

### Как бы вы build a recommender system in Spark?

**Definition:**

MLlib называет коллаборативную фильтрацию одним из семейств алгоритмов. Способ обучить и отдавать любой из этих алгоритмов в основном API — конвейер DataFrame.

**How it works:**

- Обучающие строки живут в DataFrame с именованными столбцами. Estimator делает `fit` этого кадра и возвращает модель. `transform` добавляет предсказание.
- Держите построение признаков и estimator в одном `Pipeline`, чтобы обслуживание выполняло те же стадии. Сохраните `PipelineModel`. Загрузка обратно совместима между младшими и патч-версиями.
- API RDD не получит новых рекомендателей. `spark.ml` — основной пакет.
- Если история большая, те же правила сохранения относятся к любому RDD, который вы переиспользуете, собирая этот DataFrame: `persist` до повторных действий и не делайте `collect` всей истории на драйвер.
- Полученные страницы MLlib не содержат отдельного примера кода рекомендателя. Контракт, который они дают, — estimator, transformer и pipeline.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Обсудите a strategy to perform real-time sentiment analysis using Spark Streaming.</summary>

### Обсудите a strategy to perform real-time sentiment analysis using Spark Streaming.

**Definition:**

Оценивайте каждую пришедшую строку обученной моделью MLlib и агрегируйте эти оценки как запрос Structured Streaming. Модель — transformer. Поток — таблица.

**How it works:**

- Обучайте офлайн через `LogisticRegression` или `Pipeline` из `Tokenizer`, `HashingTF` и логистической регрессии. `PipelineModel.transform` добавляет столбец предсказания. `fit` и `transform` в текущем API конвейеров без состояния.
- На потоке вход — `readStream`. Примените `transform` того же обученного конвейера, затем `groupBy` по предсказанию и `count`, или окно этого счётчика с `withWatermark`, если нужно время события.
- `outputMode("complete")` печатает текущую таблицу, как в примере подсчёта слов из сокета. Делайте checkpoint запроса, чтобы перезапуск не обрабатывал заново с неизвестного смещения.
- Микропакеты — значение по умолчанию и путь exactly-once, когда у источника есть смещения, а приёмник идемпотентен. Непрерывная обработка — at-least-once.
- Не обучайте внутри `foreach`, меняя модель на драйвере. Это замыкание — копия на executor.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

<details>
<summary>Предложите an ETL pipeline design using Spark to process large datasets.</summary>

### Предложите an ETL pipeline design using Spark to process large datasets.

**Definition:**

Прочитайте источник данных в DataFrame, преобразуйте его ленивыми операциями DataFrame или RDD и запишите наружу. Действия — шаги, которые правда выполняются.

**How it works:**

- CSV — это `spark.read.option("header", True).csv(path)`. JSON — `spark.read.json`. Текстовый файл может остаться RDD через `textFile`, пока не нужны столбцы.
- Фильтры и map — преобразования. `filter`, `groupBy().count()` и SQL через временное представление — структурные формы. `avg()` и `count()` — встроенные агрегаты.
- Пишите через `dataframe.write.csv`. Это даёт каталог part-файлов. Руководство CSV предупреждает, что чтение каталога с не-CSV файлами выводит неверную схему.
- Сохраняйте только тот DataFrame или RDD, который нужен двум поздним действиям. Shuffle вроде `join` или `reduceByKey` — дорогой прыжок. Агрегируйте до него.
- Потоковый приём — тот же запрос с `readStream` и `writeStream` плюс checkpoint. Не используйте драйвер как место, где собирают строки.

**Sources:**

- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Как бы вы use Spark to detect fraudulent behavior in financial transactions?</summary>

### Как бы вы use Spark to detect fraudulent behavior in financial transactions?

**Definition:**

Разметка мошенничества в этом API — классификационный DataFrame: столбец метки, столбец признаков и `LogisticRegression` или конвейер, который порождает этот столбец. Потоковое обнаружение — `transform` на каждом микропакете.

**How it works:**

- Пример estimator обучается на `label` и `features`, а `transform` добавляет `prediction`. Метку вы выбираете из прошлых случаев. Библиотека не задаёт правило мошенничества.
- Конвейер держит одни и те же стадии признаков на истории и на живых строках. Сохраните `PipelineModel` и загрузите его в задании, которое оценивает.
- Живые строки — запрос Structured Streaming. Оконные счётчики событий с watermark — то, как движок ограничивает состояние, если сигнал — «слишком много событий за десять минут».
- Соедините транзакцию со справочником счёта через `join` RDD пар или держите обе стороны DataFrame до `transform`. Сохраните сторону справочника, если каждый пакет её использует.
- Оценка exactly-once зависит от повторяемого источника, checkpoint и идемпотентного приёмника. Напечатанная консоль — приёмник примера, не гроссбух.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Проиллюстрируйте how you would use Spark to optimize marketing strategies BASEd on customer behavior data.</summary>

### Проиллюстрируйте how you would use Spark to optimize marketing strategies BASEd on customer behavior data.

**Definition:**

Агрегируйте поведение по ключу клиента, затем обучите классификатор или алгоритм из семейства кластеризации на этом DataFrame. Перечисленные семейства MLlib — классификация, регрессия, кластеризация и коллаборативная фильтрация.

**How it works:**

- `reduceByKey` считает события на клиента, не перевозя каждое событие. `groupBy` и `count` — форма DataFrame. `avg()` суммирует числовой столбец.
- Вектор признаков — столбец `Vectors.dense` или выход transformer вроде `HashingTF`. `LogisticRegression.fit` учит метку вроде «конверсия».
- Коллаборативная фильтрация — перечисленное семейство, когда данные — история предпочтений, а не одна метка. Она всё равно обучается через API DataFrame.
- Сохраните агрегированный RDD клиентов, если его читают несколько моделей. Не собирайте каждого клиента на драйвер, чтобы их отсортировать. `takeOrdered` оставляет вершину рейтинга.
- Файл кампании — `write.csv` оценённого DataFrame, каталог part-файлов.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
</details>

<details>
<summary>Объясните a big data analytics project where Spark would be a better choice than other big data technologies and why.</summary>

### Объясните a big data analytics project where Spark would be a better choice than other big data technologies and why.

**Definition:**

Spark подходит, когда один и тот же набор в памяти переиспользуют несколько действий. Это случай, который раздел о сохранении называет итеративными алгоритмами и интерактивным использованием.

**How it works:**

- Преобразование бесплатно до действия. Движок может вложить `map` в `reduce` и вернуть только агрегат, а не записывать отображённый набор наружу.
- `persist` держит партиции для следующего действия. Цифра руководства — часто более чем десятикратное ускорение. Итеративные обновления, такие как цикл рангов из `join` и `reduceByKey`, читают ссылки на каждой итерации.
- SQL и DataFrame добавляют `groupBy`, `avg()` и `count()` на том же движке, и конвейеры MLlib потребляют эти DataFrame. Одно задание кластера может агрегировать и затем сделать `fit`.
- Structured Streaming выполняет тот же запрос непрерывно, с checkpoint, а не отдельным движком, чей пользователь хранит счётчик.
- Одна запись только с map огромного файла не нуждается в кэше. Выигрыш — переиспользование и API SQL и ML на этих переиспользуемых данных.

**When to use:**

Предпочитайте Spark, когда несколько проходов, SQL или MLlib читают один набор. Однопроходная запись кэш не использует.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/streaming/
</details>

### Advanced Topics and Research

<details>
<summary>Обсудите the advancements in Spark 3.x and their impact on big data processing.</summary>

### Обсудите the advancements in Spark 3.x and their impact on big data processing.

**Definition:**

Список Spark 3.0 в руководстве MLlib конкретен: больше estimator принимают веса примеров и несколько столбцов, и API DataFrame остаётся тем, который получает возможности.

**How it works:**

- Веса примеров появились у деревьев решений, случайных лесов, градиентного бустинга, k-means, bisecting k-means, смеси гауссиан и у оценщиков классификации и регрессии.
- `StringIndexer`, `Binarizer`, `StopWordsRemover` и PySpark `QuantileDiscretizer` получили поддержку нескольких столбцов. Добавлены `RobustScaler`, factorization machines и гауссов и комплементарный наивный Байес.
- `predictRaw` публичен у моделей классификации. `predictProbability` публичен везде, кроме `LinearSVCModel`. Функции ML Python и Scala свели к паритету.
- API RDD этого не получил. Он остаётся в режиме сопровождения. Это решение версии 2.0, которое та же страница всё ещё утверждает.
- Динамическое выделение старше 3.0 и всё ещё способ, которым общий кластер забирает простаивающие executor. `spark.shuffle.service.fetch.rdd.enabled` в Spark 3.0 позволяет забирать с диска сохранённые блоки RDD через внешний shuffle-сервис, чтобы такие executor можно было считать простаивающими.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>Объясните how Dynamic Resource Allocation works in Spark.</summary>

### Объясните how Dynamic Resource Allocation works in Spark.

**Definition:**

При динамическом выделении приложение отдаёт executor, когда они простаивают, и просит ещё, когда задачи ждут. По умолчанию это выключено.

**How it works:**

- Поставьте `spark.dynamicAllocation.enabled` в true. Также включите внешний shuffle-сервис (`spark.shuffle.service.enabled`), или отслеживание shuffle, или вывод из эксплуатации, который сохраняет блоки shuffle, или плагин ввода-вывода shuffle с надёжным хранением. Иначе удалённые executor уносят свои файлы shuffle.
- Это доступно на standalone, YARN и Kubernetes. Kubernetes не может использовать внешний shuffle-сервис. На standalone стоит задать `spark.executor.cores`, иначе каждый executor может забрать все ядра worker.
- Запросы начинаются после `spark.dynamicAllocation.schedulerBacklogTimeout` секунд ожидающих задач, затем повторяются каждые `spark.dynamicAllocation.sustainedSchedulerBacklogTimeout`. Каждый раунд просит экспоненциально больше executor: 1, затем 2, 4, 8.
- Executor, простаивающий дольше `spark.dynamicAllocation.executorIdleTimeout`, удаляют. Executor с кэшированными данными держат, если `spark.dynamicAllocation.cachedExecutorIdleTimeout` не говорит иначе.
- `killExecutors` у `SparkContext` не поддерживается, пока динамическое выделение включено.

**When to use:**

Включайте, когда несколько приложений делят кластер и спрос этого задания меняется. Оставляйте выключенным, когда вы уже зарезервировали фиксированный набор executor.

**Sources:**

- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>Как you implement custom aggreGAtions in Spark?</summary>

### Как you implement custom aggreGAtions in Spark?

**Definition:**

На RDD пар `aggregateByKey` принимает нулевое значение, функцию, которая вливает значение в агрегат, и функцию, которая сливает агрегаты. В SQL встроенные агрегаты включают `avg()` и `count()`, и можно задать свой.

**How it works:**

- Таблица RDD: `aggregateByKey(zeroValue)(seqOp, combOp, [numPartitions])` на `(K, V)` возвращает `(K, U)`. `U` может отличаться от `V`. Нулевое значение должно быть нейтральным для слияния.
- `reduceByKey` — более узкий случай, где тип агрегата остаётся `V`, а функция имеет вид `(V, V) => V`. Используйте его для суммы. `aggregateByKey` — когда агрегат другой формы, например сумма и счётчик.
- Оба делают shuffle. Они строят таблицу слияния на стороне map, поэтому для этой работы они лучше `groupByKey`. `numPartitions` задаёт reduce-задачи.
- Spark SQL перечисляет `count()`, `count_distinct()`, `avg()`, `max()` и `min()` и говорит, что пользователи могут создать свои агрегатные функции. Вызов DataFrame для встроенного count — `groupBy`, затем `count`.
- Нулевое значение рассылается и используется на партицию. Это не должен быть общий изменяемый объект, который вы меняете на месте.

**Examples:**

```python
# (sum, count) per key, then the caller divides
agg = pairs.aggregateByKey((0, 0))(
    lambda acc, v: (acc[0] + v, acc[1] + 1),
    lambda a, b: (a[0] + b[0], a[1] + b[1]))
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Что такое the current research areas or challenges in the Apache Spark ecosystem?</summary>

### Что такое the current research areas or challenges in the Apache Spark ecosystem?

**Definition:**

Текущие руководства всё ещё называют цену и гарантии, которые не даются даром: цена shuffle, exactly-once только на пути микропакетов и ML API на RDD, который больше не получает возможностей.

**How it works:**

- Shuffle — это диск, сериализация и сеть плюс сброс, когда таблицы в памяти не влезают. Долгие задания держат файлы shuffle, пока RDD не перестанут использовать. `spark.local.dir` — это временное место.
- Путь exactly-once в Structured Streaming требует смещения, checkpoint, упреждающие журналы и идемпотентные приёмники. Непрерывная обработка документирована как at-least-once. Watermark гарантированно отстаёт хотя бы на задержку, и опоздавшая строка всё ещё может прийти.
- Динамическое выделение не видит будущее. Оно использует таймауты очереди и таймаут простоя и должно сохранить файлы shuffle, иначе удалённый executor заставит пересчёт. Кэширующие executor по умолчанию сохраняют.
- API RDD в MLlib заморожен на исправлениях. Нативный BLAS необязателен и не поставляется. Сохранение ML между старшими версиями — по возможности.
- Драйвер — всё ещё одно место, где `collect` может исчерпать память, а замыкания не делят изменяемое состояние. Это границы масштаба, которые называет руководство RDD.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/job-scheduling.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

<details>
<summary>Как Spark support deep learning workloads and integration with popular deep learning frameworks?</summary>

### Как Spark support deep learning workloads and integration with popular deep learning frameworks?

**Definition:**

Руководство MLlib не задаёт встраивание фреймворка глубокого обучения. Что оно задаёт — масштабируемые estimator на DataFrame и необязательную нативную линейную алгебру под этими estimator.

**How it works:**

- Основной API — `spark.ml` на DataFrame: transformer, estimator и конвейеры. Классификация, регрессия, кластеризация и коллаборативная фильтрация — перечисленные семейства. `LogisticRegression` — estimator, который показывает страница конвейеров.
- Численные ядра идут через Breeze и `dev.ludovic.netlib`. Intel MKL или OpenBLAS используются, только если вы их установите. Иначе Spark пишет предупреждение `JNIBLAS` и использует код JVM. Python нужен NumPy 1.21 или новее.
- Обучающие данные должны оставаться DataFrame. `collect` полного набора в один процесс — противоположность предупреждению руководства RDD о памяти драйвера.
- Сохранённый `PipelineModel` — то, как перезагружают обученный поток работ MLlib. Этот формат — сохранение MLlib, с совместимостью по возможности между старшими версиями.
- Если нагрузке нужна библиотека, которую эта страница не называет, она вне этого контракта. Не предполагайте имя estimator, которого нет в руководстве.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

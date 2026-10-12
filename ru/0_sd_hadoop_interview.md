# Hadoop — собеседование

Ответы заполнены.

[← К обзору](./0_system_design.md)

### Related docs

- [Hadoop — собеседование](./0_sd_hadoop_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Hadoop](https://devinterview.io/questions/machine-learning-and-data-science/hadoop-interview-questions/).

### Hadoop Fundamentals

<details>
<summary>Что такое Hadoop and what are its core components?</summary>

### Что такое Hadoop and what are its core components?

**Definition:**

Hadoop — открытый набор для хранения очень больших файлов на кластере обычных серверов и пакетной обработки рядом с этими файлами.

**How it works:**

- HDFS режет файл на крупные блоки, хранит копии на DataNode, а дерево каталогов и карту блоков держит на NameNode. Байты файла через NameNode не проходят.
- YARN выдаёт контейнеры CPU и памяти. ResourceManager ведёт учёт кластера, NodeManager стоит на каждой машине, у каждого приложения свой ApplicationMaster.
- MapReduce — исходная пакетная модель: mapper выдаёт пары ключ-значение, shuffle собирает одинаковые ключи, reducer агрегирует один ключ за раз.
- Hive даёт SQL-подобное хранилище поверх файлов, Pig — язык потоков данных, HBase — произвольный доступ к строкам, ZooKeeper — координацию кластера, Oozie — многошаговые сценарии. Mahout — более старая библиотека пакетных задач машинного обучения.
</details>

<details>
<summary>Объясните the concept of a Hadoop Distributed File System (HDFS) and its architecture.</summary>

### Объясните the concept of a Hadoop Distributed File System (HDFS) and its architecture.

**Definition:**

HDFS — распределённая файловая система для обычных дисков. Она рассчитана на длинное последовательное чтение огромных файлов, а не на низкую задержку мелких обновлений.

**How it works:**

- Файл — последовательность блоков. Блок по умолчанию 128 МБ, коэффициент репликации обычно три; оба параметра задаются на файл. Коротким может быть только последний блок.
- NameNode владеет пространством имён: создание, переименование, удаление, права и то, какие DataNode держат каждый блок. DataNode хранят блоки и обслуживают чтение и запись клиентов.
- Запись прокачивает блок по цепочке DataNode, так что реплики заполняются по мере прихода байтов. Чтение спрашивает у NameNode адреса и читает поток с ближайшей реплики.
- Допущения: запись один раз (кроме append и truncate), один писатель в момент времени и перенос задачи к данным, а не файла к задаче.
- Мелкие файлы раздувают память NameNode, потому что каждый блок — это метаданные. HDFS плохо подходит для интерактивных случайных правок, которым нужна строгая транзакция на строку.

**Examples:**

Клиент открывает `/logs/2026-10-04/events.txt`. NameNode возвращает DataNode, где лежит блок `blk_42`. Клиент читает этот блок с ближайшего DataNode и не тянет полезную нагрузку через NameNode.
</details>

<details>
<summary>Как MapReduce programming model work in Hadoop?</summary>

### Как MapReduce programming model work in Hadoop?

**Definition:**

MapReduce сводит большой пакет к двум функциям: map помечает записи, reduce собирает все записи с одной пометкой.

**How it works:**

- Вход режется на куски, обычно по блокам HDFS. Каждый mapper читает свой кусок и выдаёт промежуточные пары ключ-значение.
- Partitioner решает, какому reducer принадлежит ключ. Shuffle сортирует и группирует пары, чтобы reducer увидел все значения своих ключей.
- Необязательный combiner заранее агрегирует пары на машине mapper, если операция это позволяет (сумма, счётчик), и тем самым уменьшает shuffle.
- Планировщик старается поставить задачу на узел, где блок уже лежит. Упавшая или слишком медленная задача запускается на другом узле.

**Examples:**

```text
map("red fish blue fish") -> (red, 1), (fish, 1), (blue, 1), (fish, 1)
shuffle собирает fish -> [1, 1]
reduce(fish, [1, 1]) -> (fish, 2)
```
</details>

<details>
<summary>Что такое YARN, and how does it improve Hadoop's resource management?</summary>

### Что такое YARN, and how does it improve Hadoop's resource management?

**Definition:**

YARN отделяет планирование кластера от модели MapReduce, поэтому несколько движков делят один пул CPU и памяти.

**How it works:**

- В старой схеме JobTracker один процесс и раздавал слоты, и вёл задачу MapReduce. Фиксированные map- и reduce-слоты делали Spark или Tez неудобными гостями.
- ResourceManager — планировщик кластера. Каждый NodeManager сообщает свободные CPU и память и запускает контейнеры. У приложения свой ApplicationMaster: он запрашивает контейнеры и следит за своими задачами.
- Контейнер — это порция CPU и памяти, а не заранее нарезанный map- или reduce-слот. Задача просит столько, сколько ей нужно.
- Очереди capacity или fair делят кластер между командами. MapReduce, Spark и Tez работают рядом, потому что они приложения, а не сам планировщик.
</details>

<details>
<summary>Объясните the role of the Namenode and Datanode in HDFS.</summary>

### Объясните the role of the Namenode and Datanode in HDFS.

**Definition:**

NameNode — хозяин метаданных. DataNode — рабочие узлы, которые держат байты блоков и отвечают клиентам.

**How it works:**

- NameNode хранит дерево каталогов, права, коэффициент репликации и список DataNode для каждого блока. Пользовательские данные через него не идут.
- DataNode хранит блоки на локальных дисках, обслуживает чтение и запись и создаёт, удаляет или копирует блоки по указанию NameNode.
- Каждый DataNode шлёт heartbeat и периодический отчёт о блоках. Пропавший heartbeat значит, что эти реплики потеряны, и NameNode заказывает новые копии, пока коэффициент не восстановится.
- Правки пространства имён пишутся в журнал и периодические снимки (fsimage). Резервный NameNode, который читает кворум journal node, может принять роль; старый secondary NameNode только собирал снимки и не был горячей заменой.
</details>

<details>
<summary>Что такое Rack Awareness alGorithm in HDFS, and why is it important?</summary>

### Что такое Rack Awareness alGorithm in HDFS, and why is it important?

**Definition:**

Rack awareness — это карта NameNode «DataNode → стойка». Она позволяет пережить отказ коммутатора, не гоняя каждую запись через магистраль.

**How it works:**

- Скрипт топологии или статическая таблица сообщает NameNode, в какой стойке сидит каждый DataNode. Трафик внутри стойки считается дешевле трафика через корневой коммутатор.
- При трёх репликах обычная политика кладёт первую на узел писателя (или в его стойку), вторую — на узел другой стойки, третью — на другой узел той же удалённой стойки.
- Стойка может целиком отказать, и одна реплика останется. Три реплики в трёх разных стойках тоже пережили бы отказ стойки, но каждая запись дважды пересекла бы магистраль.
- Читателя направляют к ближайшей реплике: тот же узел, затем та же стойка, затем местный дата-центр.
</details>

<details>
<summary>Что такое some of the characteristics that differentiate Hadoop from traditional RDBMS?</summary>

### Что такое some of the characteristics that differentiate Hadoop from traditional RDBMS?

**Definition:**

Hadoop — платформа файлов и пакетов. Реляционная база — транзакционное хранилище, где схема проверяется на каждой записи.

**How it works:**

- Hadoop накладывает структуру, когда задача читает файлы (schema on read). RDBMS отвергает строку, которая не совпала с таблицей (schema on write).
- HDFS растёт добавлением дешёвых машин и хорошо переносит огромные сканы. RDBMS сделана для поиска по индексу и коротких правок структурированных таблиц.
- Задачи MapReduce и SQL-on-Hadoop сканируют и агрегируют. RDBMS меняет отдельные строки под блокировками и ждёт много коротких транзакций.
- HDFS хранит несколько копий блока и допускает одного писателя, append и truncate. RDBMS стремится к зафиксированным транзакциям, которые другие сессии видят согласованно.
- Логи, клики и выгрузки сначала кладут в Hadoop. Заказы, балансы и остатки, которые нельзя порвать, живут в базе.
</details>

<details>
<summary>Как защитить Hadoop cluster? Назовите доступные security mechanisms.</summary>

### Как защитить Hadoop cluster? Назовите доступные security mechanisms.

**Definition:**

Боевой кластер проверяет личность, разрешает доступ к каждому сервису и шифрует данные на диске и в сети.

**How it works:**

- Обычная аутентификация — Kerberos. Учётные записи часто приходят из LDAP или Active Directory. После входа сервисы выдают delegation token, чтобы задачи не повторяли обмен Kerberos.
- HDFS применяет права владельца, группы и остальных, плюс ACL на файлы и каталоги. Apache Ranger (и более старый Sentry) добавляет центральные политики для Hive, HBase и похожих сервисов.
- Зоны шифрования HDFS хранят файлы зашифрованными на диске. Key management service раскрывает ключ зоны только разрешённым клиентам. RPC и передача данных могут идти по SASL и TLS.
- Настройки proxy user позволяют сервису вроде Hive действовать от имени конечного пользователя, чтобы проверка файловой системы видела его, а не учётную запись сервиса. Доступ к shell и хостам по-прежнему требует обычного укрепления ОС.
</details>

### Hadoop Ecosystem and Tools

<details>
<summary>Опишите the role of HBASE in Hadoop ecosystem.</summary>

### Опишите the role of HBASE in Hadoop ecosystem.

**Definition:**

HBase — колоночное хранилище поверх HDFS для больших таблиц, которым нужны случайные чтение и запись. Пакетный скан в Hive для этого слишком медленный.

**How it works:**

- Строки упорядочены по ключу и разрезаны на регионы. RegionServer обслуживает эти диапазоны. HMaster назначает регионы, ZooKeeper следит за активным мастером и расположением регионов.
- Семейство колонок группирует столбцы, которые лежат вместе. Версии ячейки хранятся по метке времени. Одна строка строго согласована; это не база многострочных транзакций.
- По оси CAP HBase для одной строки выбирает согласованность и поведение при разделении и теряет доступность региона, пока сервер лежит.
- Типичные таблицы — ряды времени, события кликов или устройств, поиск признаков и метаданные, которые нужно доставать по ключу, а не сканировать.
</details>

<details>
<summary>Что такое Apache Hive and what types of problems does it solve?</summary>

### Что такое Apache Hive and what types of problems does it solve?

**Definition:**

Hive — слой хранилища: аналитик пишет HiveQL, а Hive превращает запрос в задачи над файлами, которые уже лежат в HDFS или объектном хранилище.

**How it works:**

- Metastore записывает базы, схемы таблиц, разделы и расположение файлов. Байты остаются в файлах; тело таблицы в metastore не лежит.
- Драйвер разбирает HiveQL, оптимизатор переписывает план, движок (MapReduce, Tez или Spark) его выполняет. HiveServer2 принимает удалённые сессии JDBC и ODBC.
- Подходит для сканов, агрегаций, соединений и ETL больших пакетов. Базу для транзакций одной строки он не заменяет.
- Внешние таблицы с разделами позволяют запросу пропускать целые каталоги — так дневные выгрузки логов остаются дешёвыми для запросов.
</details>

<details>
<summary>Как Apache Pig fit into the Hadoop ecosystem?</summary>

### Как Apache Pig fit into the Hadoop ecosystem?

**Definition:**

Pig — язык сценариев Pig Latin для многошаговых файловых конвейеров, когда автор не хочет писать mapper и reducer вручную.

**How it works:**

- Сценарий загружает файлы, фильтрует, соединяет, группирует и сохраняет результат. Шаги описывают поток данных, а схема может оставаться свободной, пока поле не понадобится.
- Сценарий компилируется в логический план. Работа начинается, когда запрошены store или dump, поэтому неиспользуемые ветки не выполняются.
- План идёт как MapReduce или более новый движок. Пользовательские функции на Java или Python закрывают шаги, которых нет в языке.
- Аналитикам на SQL удобнее Hive. Процедурный конвейер со своими функциями удобнее писать на Pig.

**Examples:**

```text
clicks = LOAD '/logs/clicks' AS (user:chararray, path:chararray);
home = FILTER clicks BY path == '/';
per_user = GROUP home BY user;
STORE per_user INTO '/out/home-users';
```

Пока нет `STORE`, ничего не считается.
</details>

<details>
<summary>Объясните how Apache Flume helps with log and event data collection for Hadoop.</summary>

### Объясните how Apache Flume helps with log and event data collection for Hadoop.

**Definition:**

Flume переносит потоки событий, обычно логи, от многих источников в HDFS или другой приёмник, без своего сборщика на каждом хосте.

**How it works:**

- Агент состоит из трёх частей: source принимает события, channel их буферизует, sink записывает дальше.
- Надёжный file channel хранит принятые события, пока sink не подтвердит запись, поэтому перезапуск процесса не теряет эту порцию. Memory channel быстрее и может потерять буфер.
- Агенты стоят ярусами: много краевых агентов шлют в меньшее число агрегаторов, а те нарезают файлы в HDFS. Interceptor может пометить, отфильтровать или переписать событие по пути.
- Flume рассчитан на непрерывные потоки логов. Для массового копирования реляционных таблиц нужен Sqoop.

**Examples:**

```text
веб-серверы -> Flume source (avro) -> file channel -> HDFS sink
нарезает /logs/app/2026-10-04/events.0001
```
</details>

<details>
<summary>Что такое Apache SqOOP and how does it interact with Hadoop?</summary>

### Что такое Apache SqOOP and how does it interact with Hadoop?

**Definition:**

Sqoop массово копирует таблицы между реляционной базой и Hadoop параллельными map-задачами, а не одним клиентским курсором.

**How it works:**

- Import читает срезы таблицы через JDBC и пишет их в HDFS, Hive или HBase. Export читает файлы из HDFS и вставляет или обновляет строки обратно в базе.
- Столбец разбиения, часто числовой первичный ключ, делит таблицу на диапазоны. Каждый диапазон — один mapper.
- Коннекторы покрывают обычные базы через JDBC. Для некоторых движков есть прямой режим, который обходит JDBC ради скорости.
- Инкрементальный import может забрать только строки новее столбца last-value — так ночное хранилище не отстаёт от операционной таблицы.

**Examples:**

```text
sqoop import --table orders --split-by id --target-dir /warehouse/orders
```

Четыре mapper могут читать диапазоны `id` 1–250000, 250001–500000 и так далее и записать четыре part-файла.
</details>

<details>
<summary>Как Apache Oozie help in workflow scheduling in Hadoop?</summary>

### Как Apache Oozie help in workflow scheduling in Hadoop?

**Definition:**

Oozie выполняет ориентированный граф действий Hadoop и может запускать этот граф по часам или когда появляются входные данные.

**How it works:**

- Workflow — это граф: узлы MapReduce, Hive, Pig, Sqoop, shell и похожие действия, плюс узлы fork, join и decision.
- Coordinator запускает workflow по расписанию или когда готов экземпляр набора данных. Bundle объединяет coordinator, которые нужно стартовать и останавливать вместе.
- Каждое действие сообщает успех или неудачу. Граф может повторить шаг или уйти в переход ошибки. Параметры задаются языком выражений, так что одно описание обслуживает много дат.
- Сервер Oozie хранит состояние вне короткой заявки YARN, поэтому многочасовой конвейер может продолжиться после падения одного действия.

**Examples:**

Ночной граф параллельно делает import Sqoop и свёртку Flume, затем join и агрегацию Hive. Coordinator стартует его, когда есть `/raw/2026-10-04`, а упавший шаг Hive уходит в ветку ошибки, а не помечает весь день успешным.
</details>

<details>
<summary>Что такое Apache Zookeeper and why is it important for Hadoop?</summary>

### Что такое Apache Zookeeper and why is it important for Hadoop?

**Definition:**

ZooKeeper — небольшое реплицированное хранилище для координации: конфигурация, состав участников и то, кто сейчас лидер.

**How it works:**

- Клиенты видят дерево znode. Обновления полностью упорядочены. Сессия может создать эфемерный узел, который исчезнет вместе с сессией, и последовательный узел, к имени которого добавляется счётчик.
- Watch сообщает клиенту, что znode изменился, поэтому сервисы не опрашивают каждого соседа. Ансамбль фиксирует запись, только когда согласилось большинство.
- Высокая доступность HDFS использует ZooKeeper, чтобы активным был один NameNode. HBase выбирает через него мастера и публикует, какой сервер держит регион. YARN может хранить там состояние восстановления ResourceManager.
- Это сервис координации, а не база прикладных записей. Данные в znode должны оставаться маленькими.
</details>

<details>
<summary>Обсудите the role of Apache Spark in the Hadoop ecosystem.</summary>

### Обсудите the role of Apache Spark in the Hadoop ecosystem.

**Definition:**

Spark работает как приложение на YARN, менеджере ресурсов Hadoop, и читает данные из HDFS. Это не замена файловой системе.

**How it works:**

- HDFS хранит большие файлы блоками на DataNode. NameNode держит пространство имён и карту блоков. Данные пользователей через NameNode не идут. Модель — записал один раз, читаешь много раз, с упором на пропускную способность на обычном железе.
- Руководство по RDD говорит, что RDD можно создать из файла в файловой системе Hadoop. Чтобы трогать HDFS, добавляют зависимость hadoop-client и направляют HADOOP_CONF_DIR или YARN_CONF_DIR на клиентскую конфигурацию.
- spark-submit --master yarn берёт адрес ResourceManager из этой конфигурации Hadoop. В режиме cluster драйвер работает внутри application master YARN, и клиент может уйти. В режиме client драйвер остаётся в процессе клиента.
- Spark может сохранить RDD в памяти для повторного использования, ради этого его и ставят рядом с пакетным MapReduce на тех же файлах HDFS. Восстановление потерянной партиции — родословная RDD. Восстановление потерянного диска — по-прежнему повторная репликация HDFS.
- Java 17 требуется Spark 4.0.0 и новее. Hadoop поддерживает Java 17 с версии 3.5.0. Более старому кластеру YARN нужен отдельный JDK для приложения Spark.

**When to use:**

Spark на YARN — когда байты уже лежат в HDFS и нужны параллельные операции в памяти. Репликация HDFS и NameNode нужны независимо от того, Spark ли считает.

**Sources:**

- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Data Management and Processing

<details>
<summary>Как Hadoop handle the failure of a datanode?</summary>

### Как Hadoop handle the failure of a datanode?

**Definition:**

HDFS ожидает отказ DataNode. NameNode замечает пропавший heartbeat и реплицирует блоки, у которых число копий упало ниже заданного.

**How it works:**

- Каждый DataNode шлёт Heartbeat и Blockreport. Heartbeat значит, что узел жив. Blockreport перечисляет его блоки.
- Нет недавнего heartbeat: NameNode помечает DataNode мёртвым и не шлёт ему новый ввод-вывод. Таймаут по умолчанию намеренно длинный, больше 10 минут, чтобы мигание не вызвало шторм репликации. Более короткий интервал «устаревший» можно настроить.
- Смерть узла, порченая реплика, мёртвый диск или повышенный коэффициент репликации заставляют NameNode заново реплицировать недореплицированные блоки.
- Клиенты сверяют контрольную сумму. Если реплика не сошлась, клиент может прочитать другую.
- Данные пользователей через NameNode не идут. Потеря DataNode — потеря копий блоков, не пространства имён.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Объясните the process of data replication in HDFS.</summary>

### Объясните the process of data replication in HDFS.

**Definition:**

Каждый файл — последовательность блоков, и каждый блок лежит на нескольких DataNode. Размещение решает NameNode. Клиенты пишут через конвейер.

**How it works:**

- Размер блока и коэффициент репликации задаются на файл. Все блоки, кроме последнего, одного размера. Типичный блок — 128 MB. Файлы пишутся один раз, кроме дописывания и усечения, и у файла один писатель.
- NameNode выбирает DataNode для блока. При коэффициенте три клиент пишет в первый DataNode, тот пересылает порцию второму, второй — третьему. Узел может принимать и пересылать одновременно.
- Размещение реплик учитывает стойки, чтобы копии не лежали на одной стойке. Каждое решение о репликации принимает NameNode.
- Клиент сверяет контрольные суммы и может перейти на другую реплику.
- Больше реплик стоят диска и сети. Так HDFS остаётся доступным, когда железо отказывает. Дизайн считает это нормой.

**Examples:**

```text
client -> DN1 -> DN2 -> DN3
replication factor 3, block typically 128 MB
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Что такое speculative execution in Hadoop, and why is it used?</summary>

### Что такое speculative execution in Hadoop, and why is it used?

**Definition:**

Каркас MapReduce оставляет слоты задач под speculative-tasks и под упавшие задачи, чтобы одна медленная задача не была единственной копией этой работы.

**How it works:**

- Заметка учебника о размере держит число reduce-задач чуть ниже целого коэффициента, чтобы оставить несколько слотов reduce для speculative-tasks и упавших задач.
- Упавшим задачам нужно где выполниться снова. Ту же запасную ёмкость каркас использует для speculative-tasks.
- Больше reduce-задач добавляет накладные расходы каркаса и улучшает баланс, и снижает цену отказа. Ноль reduce-задач законен: выход map тогда идёт прямо в FileSystem и не сортируется.
- Это ёмкость внутри задания, не повторная репликация HDFS. Мёртвый DataNode лечит NameNode, копируя блоки. Медленная задача — забота планирования MapReduce.
- Не занимайте каждый слот первой попыткой. Причина запасных слотов в учебнике — эти дополнительные задачи.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Как large datasets processed in Hadoop?</summary>

### Как large datasets processed in Hadoop?

**Definition:**

HDFS хранит файл большими реплицированными блоками. MapReduce отдаёт каждому mapper один input split, а reducer собирает по ключу после shuffle.

**How it works:**

- HDFS сделан для потоковой пропускной способности на больших файлах, не для интерактивных правок с малой задержкой. Типичный файл — от гигабайт до терабайт. Вычисление переносят к данным.
- InputSplit — то, что обрабатывает один Mapper. FileSplit — значение по умолчанию. RecordReader превращает байтовый split в записи.
- Mapper порождает пары ключ-значение. Combiner может агрегировать локально. Partitioner назначает ключи reducer. Reducer получает iterable значений на ключ.
- Учебник WordCount собирает это через Job, TokenizerMapper и IntSumReducer, который стоит и combiner, и reducer.
- Spark на YARN может прочитать тот же файл HDFS в RDD и преобразовать его в памяти. Байты всё ещё лежат в HDFS.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Что такое the significance of the input split in MapReduce jobs?</summary>

### Что такое the significance of the input split in MapReduce jobs?

**Definition:**

InputSplit — данные, которые обрабатывает один Mapper. Обычно это диапазон байтов. RecordReader превращает этот диапазон в записи.

**How it works:**

- FileSplit — InputSplit по умолчанию. Он ставит mapreduce.map.input.file в путь входного файла этого логического split.
- Split не обязан быть одним блоком HDFS, но локальность — смысл переноса вычисления к данным. Mapper должен работать рядом с байтами, которые читает.
- Границы записей — дело RecordReader. Mapper видит ключи и значения, не сырые края split.
- Слишком много крошечных split — слишком много map-задач и больше накладных расходов каркаса. Учебник отмечает, что больше задач улучшает баланс и добавляет накладные расходы.
- Split — граница вычисления. Репликация лежащего под ним блока — забота HDFS.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Как partitioning work in Hadoop, and when is it used?</summary>

### Как partitioning work in Hadoop, and when is it used?

**Definition:**

Partitioner назначает промежуточные ключи reduce-задачам. Ключи, которые надо агрегировать вместе, должны попасть на один reducer.

**How it works:**

- Учебник говорит прямо: partitioner делит пространство ключей и управляет распределением ключей промежуточного выхода map.
- После map пары уходят shuffle на reducer, выбранный для этого ключа. Reducer затем видит каждое значение ключей, которыми владеет.
- Это нужно, когда reducer больше одного. При нуле reducer шага раздела нет: выход map пишется прямо в FileSystem и не сортируется.
- Плохой раздел отправляет все ключи одному reducer. Заметка учебника о большем числе reducer — про баланс. Запасные слоты всё ещё оставляют для speculative-tasks и отказов.
- Это не размещение блоков HDFS. Блоки размещает NameNode. Ключи размещает partitioner.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Объясните how reducers work in MapReduce and their interaction with shufflers.</summary>

### Объясните how reducers work in MapReduce and their interaction with shufflers.

**Definition:**

Reducer получает для каждого своего ключа значения, которые mapper породили для этого ключа. Shuffle — перевозка этих пар от mapper к нужному reducer.

**How it works:**

- В учебнике WordCount reducer реализует reduce(Text key, Iterable values, Context). Он суммирует значения этого ключа.
- Partitioner выбирает, какой reducer владеет ключом. Shuffle доставляет пары. Каркас сортирует выход map перед reducer, когда reducer есть.
- Combiner может сначала суммировать локально на стороне map, в примере тем же классом IntSumReducer, чтобы меньше пар шло по сети.
- Ноль reduce-задач пропускает это. Выход map идёт по пути из FileOutputFormat.setOutputPath и не сортируется.
- Больше reducer размазывает shuffle и снижает цену одного отказа ценой накладных расходов каркаса.

**Examples:**

```java
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Что такое SequenceFiles in Hadoop?</summary>

### Что такое SequenceFiles in Hadoop?

**Definition:**

Собственный выход MapReduce — файл записей, который пишет RecordWriter. HDFS хранит этот файл блоками; формат записи — выбор задания, не размер блока.

**How it works:**

- OutputFormat описывает выход задания. Каркас пишет записи через RecordWriter. FileOutputFormat.setOutputPath задаёт, куда идёт выход map, когда reducer нет, и куда идёт выход задания в остальных случаях.
- HDFS затем хранит этот файл реплицированными блоками, обычно 128 MB, с записью один раз, кроме append и truncate.
- NameNode хранит запись пространства имён. Байты лежат на DataNode. Плотный контейнер многих записей держит пространство имён меньше, чем файл на каждую запись. Это совпадает с тем, что HDFS настроен на большие файлы.
- Задание WordCount пишет ключи Text и значения IntWritable. Эта пара — запись. Границы блоков — забота HDFS.
- Spark потом может прочитать файл, который поддерживает Hadoop, в RDD. Формат файла не меняет решения NameNode о репликации.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Performance Tuning and Optimization

<details>
<summary>Опишите the ways to optimize a MapReduce job.</summary>

### Опишите the ways to optimize a MapReduce job.

**Definition:**

Урежьте байты на shuffle, балансируйте reducer и оставьте запасные слоты. Не ждите, что NameNode будет стримить данные файла.

**How it works:**

- Combiner агрегирует на стороне map. WordCount использует IntSumReducer и как combiner, и как reducer, чтобы по сети шли счётчики, а не каждый токен.
- Partitioner должен размазывать ключи. Один горячий ключ делает один reducer всем заданием.
- Больше reducer улучшает баланс и снижает цену отказа и добавляет накладные расходы. Учебник оставляет несколько слотов для speculative-tasks и упавших задач.
- Input split должен следовать локальности данных. Правило HDFS: перенести вычисление дешевле, чем перенести данные.
- Сжатие выхода map перечислено в учебнике среди полезных возможностей. Путь выхода всё ещё FileOutputFormat. Ноль reducer пропускает сортировку, когда она правда не нужна.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Как you diagnose and troubleshoot Hadoop performance issues?</summary>

### Как you diagnose and troubleshoot Hadoop performance issues?

**Definition:**

Отделите проблему метаданных на NameNode от проблемы данных на DataNode и от перекошенного shuffle MapReduce.

**How it works:**

- Если задания не могут открыть или перечислить файлы, смотрите NameNode. Он держит пространство имён. FsImage и EditLog — его центральные структуры. Данные пользователей через него не идут.
- Если чтения падают или медленные, проверьте мёртвые DataNode и недореплицированные блоки. Пропавшие heartbeat при длинном таймауте по умолчанию задерживают повторную репликацию. Контрольные суммы говорят, что реплика порчена.
- Если один reducer работает вечно, причина — partitioner или горячий ключ. Счётчики — возможность учебника видеть прогресс задач.
- Журналы задач — часть среды задач в учебнике. Map-задача далеко от своего split нарушает локальность.
- У Spark на том же кластере свой интерфейс драйвера. Не отлаживайте стадию RDD только heartbeat NameNode.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Что такое the significance of combiner in the Hadoop MapReduce framework?</summary>

### Что такое the significance of combiner in the Hadoop MapReduce framework?

**Definition:**

Combiner агрегирует выход map локально, до shuffle, чтобы сеть несла меньше данных. В WordCount это тот же класс, что и reducer.

**How it works:**

- Учебник ставит job.setCombinerClass(IntSumReducer.class) и тот же класс как reducer. Пары каждого map сортируются по ключу и затем агрегируются локально.
- Документированный локальный результат — счётчики вроде Hello 1 и World 2 на этом map, а не каждый сырой токен.
- Reducer всё ещё видит iterable на ключ и суммирует снова. Combiner должен быть безопасен при повторном применении для агрегации задания. Сумма — да. Среднее, записанное одним числом, — нет, если значение не несёт счётчик.
- Combiner не меняет разделение. Ключи всё ещё идут к reducer, которого выбрал partitioner.
- При нуле reducer нет shuffle, который можно ужать.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Объясните what you can do to optimize the performance of HDFS.</summary>

### Объясните what you can do to optimize the performance of HDFS.

**Definition:**

Держите файлы большими, реплики с учётом стоек и NameNode вне пути данных.

**How it works:**

- HDFS настроен на большие файлы и потоковую пропускную способность. Типичный блок — 128 MB. Пространство имён из крошечных файлов тратит NameNode на метаданные.
- Коэффициент репликации — на файл. Поднимайте его для данных, которые нельзя потерять. Не поднимайте для каждого временного файла. Конвейер записи копирует каждую порцию на каждую реплику.
- Размещение с учётом стоек улучшает надёжность и полосу. Цели выбирает NameNode.
- Таймаут мёртвого узла по умолчанию длинный, чтобы не было шторма репликации. Для чтений, чувствительных к задержке, дизайн позволяет более короткий интервал устаревания, чтобы клиенты обходили устаревшие узлы.
- Несколько синхронных копий FsImage и EditLog защищают метаданные и могут замедлить операции пространства имён. Этот обмен принят, потому что приложения интенсивны по данным, не по метаданным. HA с Journal — рекомендуемый вариант устойчивости.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Как job scheduling be optimized in Hadoop?</summary>

### Как job scheduling be optimized in Hadoop?

**Definition:**

Отправляйте задание MapReduce в очередь, подбирайте число map и reduce под split и пространство ключей и оставляйте место для повторов.

**How it works:**

- Учебник перечисляет отправку заданий в очереди как пользовательскую возможность, рядом со счётчиками и distributed cache.
- Параллельность map следует input split. Параллельность reduce следует тому, как вы делите ключи. Слишком мало reducer — перекос. Слишком много — накладные расходы.
- Оставляйте слоты для speculative-tasks и упавших задач, а не заполняйте каждый слот первой волной.
- YARN — менеджер ресурсов, которым пользуется и Spark. spark-submit --master yarn --queue thequeue ставит приложение в очередь. В режиме cluster драйвер работает в application master.
- Очередь не чинит недореплицированные блоки. Это остаётся за NameNode.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Что такое the best practices for managing memory and CPU resources in a Hadoop cluster?</summary>

### Что такое the best practices for managing memory and CPU resources in a Hadoop cluster?

**Definition:**

Учебник считает память задачи и параметры map/reduce конфигурацией задания. Размещение блоков HDFS — отдельный бюджет от кучи задач.

**How it works:**

- У выполнения задач MapReduce есть раздел управления памятью и отдельные параметры map и shuffle/reduce. Задайте их на задании, а не надейтесь, что куча по умолчанию вместит split.
- Combiner режет байты shuffle, а значит и память на стороне reduce.
- Ноль reducer избегает буфера сортировки, когда агрегация не нужна.
- На YARN пример Spark явно задаёт --driver-memory и --executor-memory. Та же идея: размер контейнера — это запрос, не всё свободное на машине.
- DataNode хранит блоки на диске. Не задавайте кучу задачи так, будто весь блок 128 MB должен стать одним объектом Java. RecordReader идёт по записям.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Coding Challenges

<details>
<summary>Напишите a MapReduce job in Java that counts the number of words in a text file.</summary>

### Напишите a MapReduce job in Java that counts the number of words in a text file.

**Definition:**

Задание WordCount из учебника MapReduce отображает токены в 1 и суммирует их в combiner и reducer.

**How it works:**

- Job создают с именем. Задают класс jar, mapper, combiner, reducer и классы выходного ключа и значения.
- FileInputFormat добавляет входной путь. FileOutputFormat задаёт выходной путь. waitForCompletion запускает задание.
- Mapper порождает Text и IntWritable. Reducer суммирует iterable для каждого слова. Combiner — тот же класс reducer, поэтому каждый map сначала агрегирует.
- Input split и record reader кормят mapper. Блок HDFS вы сами не открываете.
- Код выхода процесса — 0, только если waitForCompletion вернул true.

**Examples:**

```java
Job job = Job.getInstance(conf, "word count");
job.setJarByClass(WordCount.class);
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
job.setOutputKeyClass(Text.class);
job.setOutputValueClass(IntWritable.class);
FileInputFormat.addInputPath(job, new Path(args[0]));
FileOutputFormat.setOutputPath(job, new Path(args[1]));
System.exit(job.waitForCompletion(true) ? 0 : 1);
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Создайте an HDFS command script that copies files from a local file system to HDFS.</summary>

### Создайте an HDFS command script that copies files from a local file system to HDFS.

**Definition:**

Команда put оболочки файловой системы копирует один или несколько локальных путей в целевую файловую систему.

**How it works:**

- Использование: hadoop fs -put с необязательными флагами, затем источники и назначение.
- Копирование падает, если файл назначения уже есть, если не задан -f. -f перезаписывает.
- -p сохраняет времена, владельца и права, когда их можно перенести между файловыми системами.
- -l позволяет DataNode записать файл на диск лениво и форсирует коэффициент репликации 1. Документация оболочки говорит, что это снижает долговечность. Не используйте это для данных, которые надо сохранить.
- У той же оболочки есть mkdir, rm и cat. На странице дизайна HDFS они даны в формах bin/hadoop dfs и bin/hadoop fs. put — команда копирования внутрь на странице FileSystemShell.

**Examples:**

```text
hadoop fs -put ./events/ /data/events
# overwrite:
hadoop fs -put -f ./events/part-0 /data/events/part-0
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Реализуйте a simple Hive query to summarize data from a Hive table.</summary>

### Реализуйте a simple Hive query to summarize data from a Hive table.

**Definition:**

HiveQL SELECT читает таблицу, фильтрует через WHERE и агрегирует через GROUP BY. Имена таблиц и столбцов нечувствительны к регистру.

**How it works:**

- SELECT select_expr FROM table_reference WHERE condition GROUP BY col_list — форма из руководства языка.
- Сводка — это GROUP BY плюс агрегат в списке выбора. HAVING может фильтровать группы. Пример руководства: SELECT col1 FROM t1 GROUP BY col1 HAVING SUM(col2) > 10.
- Без предиката раздела SELECT сканирует всю таблицу. Если таблица создана с PARTITIONED BY, предикат по этому столбцу в WHERE отсекает разделы. Руководство показывает это на page_views.date.
- LIMIT n ограничивает строки. ORDER BY вместе с LIMIT возвращает упорядоченный префикс, как в примере customers.
- Hive не размещает блоки HDFS. Файлы таблицы — всё ещё файлы HDFS с обычной репликацией.

**Examples:**

```sql
SELECT region, SUM(amount)
FROM sales
WHERE amount > 10
GROUP BY region
```

**Sources:**

- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Напишите код: a Pig Latin script to process and transform a dataset into a desired format.</summary>

### Напишите код: a Pig Latin script to process and transform a dataset into a desired format.

**Definition:**

Тот же конвейер, который учебник MapReduce расписывает на Java: прочитать записи, оставить поля, отфильтровать и агрегировать по ключу. Pig Latin — отдельный язык для этого конвейера. Операции ниже — те, которые реально задают руководства MapReduce и Hive.

**How it works:**

- Mapper порождает поля, которые вы оставляете. Combiner и reducer агрегируют по ключу, как WordCount с TokenizerMapper и IntSumReducer.
- SELECT … WHERE … GROUP BY в Hive — декларативная форма этого фильтра и агрегата, когда данные — таблица Hive.
- put в HDFS кладёт исходный файл. Задание затем берёт FileInputFormat на этом пути.
- Не подставляйте выдуманное имя оператора Pig вместо этого. Если в среде стандарт — Pig, сверьтесь с его текущим руководством. Семантика, которую надо сохранить, — та, что выше.
- Результат держите в больших файлах. HDFS настроен на большие блоки, обычно 128 MB, а не на файл на каждую запись.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Автоматизируйте импорт данных из MySQL database в HDFS с помощью Sqoop.</summary>

### Автоматизируйте импорт данных из MySQL database в HDFS с помощью Sqoop.

**Definition:**

Осевшие байты должны стать файлом HDFS через оболочку файловой системы или входной путь задания. Инструмент импорта из базы полезен, только если его выход — этот файл.

**How it works:**

- hadoop fs -put копирует локальные файлы в HDFS. Если выгрузка уже породила файлы, put — документированное копирование. -f перезаписывает. Избегайте -l, когда нужна обычная репликация. Он форсирует коэффициент 1.
- Когда путь есть, задание MapReduce направляет на него FileInputFormat, или таблица Hive его читает.
- HDFS реплицирует блоки, обычно с коэффициентом файла, через конвейер записи.
- Не направляйте NameNode на базу. Данные пользователей через NameNode не идут. Импорт пишет блоки на DataNode.
- Отказавший DataNode во время записи обрабатывает конвейер, а позже — повторная репликация, если реплика потеряна. Импорт не закончен, пока файл не закрыт и реплики не существуют.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Реализуйте a Spark job in Scala or Python to perform a join operation on two datasets.</summary>

### Реализуйте a Spark job in Scala or Python to perform a join operation on two datasets.

**Definition:**

На RDD пар join возвращает (K, (V, W)) для каждой пары значений с общим ключом. Spark может прочитать оба входа из HDFS.

**How it works:**

- Руководство RDD определяет join(otherDataset, [numPartitions]) на наборах типов (K, V) и (K, W). leftOuterJoin, rightOuterJoin и fullOuterJoin — внешние формы.
- Соберите один SparkContext из SparkConf. Один контекст на JVM. В кластере master задаёт spark-submit. Для YARN этот master — yarn.
- textFile по пути HDFS требует конфигурацию Hadoop в classpath, HADOOP_CONF_DIR или YARN_CONF_DIR и зависимость hadoop-client.
- numPartitions у join управляет партициями shuffle. Это та же забота, что partitioner в MapReduce: один горячий ключ попадает в одну партицию.
- persist у RDD, который вы соединяете больше одного раза. Руководство говорит, что persistence переиспользует набор между действиями.

**Examples:**

```scala
val left = sc.textFile(pathA).map(parseLeft)   // (K, V)
val right = sc.textFile(pathB).map(parseRight) // (K, W)
val joined = left.join(right)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Разработайте an Oozie workflow that schedules and runs a set of MapReduce and Hive jobs.</summary>

### Разработайте an Oozie workflow that schedules and runs a set of MapReduce and Hive jobs.

**Definition:**

Поток работ — последовательность задания MapReduce и запроса Hive. Примитивы, которые реально можно запустить по полученным руководствам, — waitForCompletion и SELECT в Hive.

**How it works:**

- Задание MapReduce — Job в форме WordCount: задать mapper, combiner, reducer, входной путь, выходной путь, затем waitForCompletion.
- Шаг Hive — SELECT с WHERE и GROUP BY по таблице, чьи файлы уже в HDFS. Предикаты раздела отсекают скан.
- Упорядочьте их так, чтобы путь таблицы Hive существовал до запроса. put или выходной путь MapReduce создаёт этот файл.
- Очереди YARN — то, как кластер планирует контейнеры. Учебник MapReduce перечисляет отправку заданий в очереди. Spark использует --queue, когда считает Spark.
- Планировщик, который игнорирует неудачный waitForCompletion и всё равно запускает запрос, суммирует отсутствующий путь. Код выхода задания — это ворота.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
</details>

<details>
<summary>Напишите a Java program to implement custom input and output format Classes in Hadoop.</summary>

### Напишите a Java program to implement custom input and output format Classes in Hadoop.

**Definition:**

Свой входной формат всё равно должен порождать InputSplit и RecordReader. Свой выходной формат всё равно должен дать RecordWriter. Задание затем называет эти классы.

**How it works:**

- InputSplit — байтовый кусок, который обрабатывает один mapper. FileSplit — значение по умолчанию, и он ставит mapreduce.map.input.file. RecordReader показывает mapper записи.
- OutputFormat — спецификация выхода. Каркас пишет через RecordWriter. FileOutputFormat.setOutputPath — куда идёт файловый выход по умолчанию.
- Класс формата выбирают на Job так же, как учебник выбирает классы mapper и reducer. Mapper должен по-прежнему видеть ключи и значения, а не смещения блоков HDFS, если только запись — не это смещение.
- Байты, которые вы пишете, — файл HDFS. Репликация и размер блока остаются настройками HDFS, обычно блоки 128 MB.
- Границы записей держите внутри RecordReader. Split, который режет запись пополам, — проблема читателя. Поэтому учебник отдаёт границы RecordReader.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Advanced Hadoop Features and Architecture

<details>
<summary>Что такое the concept of erasure coding in HDFS, and how does it differ from replication?</summary>

### Что такое the concept of erasure coding in HDFS, and how does it differ from replication?

**Definition:**

Документированная отказоустойчивость данных файлов в архитектуре HDFS — репликация: полные копии каждого блока, а не схема чётности.

**How it works:**

- Файл — последовательность блоков. Блоки реплицируются. Коэффициент настраивается на файл. При коэффициенте три конвейер записи хранит три полные копии.
- Потерянный DataNode роняет коэффициент репликации своих блоков. NameNode реплицирует заново, пока коэффициент снова не выполнен. Клиент, у которого не сошлась контрольная сумма, читает другую полную реплику.
- Это стоит диска, равного коэффициенту, умноженному на данные. Дизайн принимает цену, потому что отказ железа — норма.
- Страница архитектуры не задаёт схему чётности или erasure. Не описывайте её так, будто она из этого документа. Механизм, который она задаёт, — лишние полные реплики и контрольные суммы.
- Метаданные — снова другое: несколько копий FsImage и EditLog или HA с Journal. Это тоже не erasure coding блоков.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Объясните how Hadoop uses data locality to improve performance.</summary>

### Объясните how Hadoop uses data locality to improve performance.

**Definition:**

HDFS кладёт блоки на DataNode и ожидает, что вычисление побежит рядом с этими блоками, потому что перенести вычисление дешевле, чем перенести данные.

**How it works:**

- Дизайн прямо называет это допущение и говорит, что HDFS даёт приложениям интерфейсы, чтобы перенести себя ближе к данным.
- InputSplit mapper должен быть байтами на близком узле. FileSplit называет файл. Выигрыш планировщика — задача на узле, который уже хранит блок.
- Репликация облегчает локальность: коэффициент три значит, что задачу могут принять три узла. Размещение с учётом стоек держит копии больше чем на одной стойке, чтобы локальность и устойчивость к отказам были возможны вместе.
- NameNode не стримит данные пользователей. Задание, которое читает всё через NameNode, отказалось от локальности.
- У Spark на YARN тот же интерес. RDD, созданный из файла HDFS, разрезан на партиции. Shuffle, например join, двигает данные и является дорогой частью. Чтение файла всё ещё должно быть локальным.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Как Hadoop support different file formats, and what are some of them?</summary>

### Как Hadoop support different file formats, and what are some of them?

**Definition:**

HDFS хранит байты в блоках. Раскладку записей выбирают входной и выходной форматы MapReduce или читатель вроде Spark.

**How it works:**

- FileSplit плюс RecordReader — способ по умолчанию, которым mapper видит записи. Другой RecordReader — другой формат на тех же блоках.
- OutputFormat и RecordWriter выбирают, как пишется результат. WordCount пишет Text и IntWritable через выходные классы задания.
- Hive читает таблицы. SELECT не интересуется размером блока 128 MB. Отсечение разделов интересуется столбцом раздела.
- textFile в Spark читает файловую систему, которую поддерживает Hadoop, в RDD строк. Это один формат: текстовые строки. join затем работает на парах, которые вы разобрали.
- NameNode всё ещё хранит одну запись пространства имён на файл. Формат, который создаёт миллионы маленьких файлов, давит на метаданные, даже если каждый файл — допустимый формат.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Что такое Hadoop federation, and how can it scale a Hadoop cluster?</summary>

### Что такое Hadoop federation, and how can it scale a Hadoop cluster?

**Definition:**

Документированный дизайн HDFS — один NameNode на пространство имён. Это просто, и это же потолок метаданных. Данные масштабируются добавлением DataNode.

**How it works:**

- Один NameNode распоряжается пространством имён. Данные пользователей через него не идут. Ёмкость для байтов — больше DataNode и больше блоков.
- NameNode может держать несколько синхронных копий FsImage и EditLog. Это защищает метаданные и может снизить число операций пространства имён в секунду. От приложений ждут интенсивности по данным, не по метаданным.
- Высокая доступность использует несколько NameNode с общим хранилищем или, что предпочтительно, распределённый журнал правок под названием Journal. Это переключение одного пространства имён, не второе независимое пространство.
- Добавление DataNode масштабирует хранение и места, где mapper может работать локально. Оно не снимает с NameNode роль пространства имён.
- Spark и MapReduce масштабируют вычисления на YARN большим числом задач и узлов. Блоки они всё равно ищут через это пространство имён.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Обсудите the concept and benefits of a journal node in HDFS HA configuration.</summary>

### Обсудите the concept and benefits of a journal node in HDFS HA configuration.

**Definition:**

Journal — рекомендуемый распределённый журнал правок для высокой доступности с несколькими NameNode.

**How it works:**

- FsImage и EditLog — центральные структуры метаданных. Их порча может сделать экземпляр нерабочим.
- NameNode может держать несколько копий и обновлять их синхронно. Это безопасно и может замедлить пропускную способность пространства имён.
- Другой вариант дизайна — высокая доступность с несколькими NameNode, либо с общим хранилищем NFS, либо с распределённым журналом правок под названием Journal. Journal — рекомендуемый подход.
- При перезапуске NameNode выбирает последний согласованный FsImage и EditLog.
- Journal не реплицирует блоки файлов. Реплики блоков остаются на DataNode согласно коэффициенту репликации. Journal — то, как запасные NameNode делят поток правок одного пространства имён.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Что такое the implications of small files on HDFS performance and how can this be mitiGAted?</summary>

### Что такое the implications of small files on HDFS performance and how can this be mitiGAted?

**Definition:**

HDFS настроен на большие файлы. Типичный блок — 128 MB. Много маленьких файлов превращается в много записей пространства имён на единственном NameNode и в много map-задач.

**How it works:**

- NameNode держит пространство имён и карту блоков. Данные пользователей через него не идут, но каждый файл — всё ещё объект метаданных. Дизайн говорит, что приложения не интенсивны по метаданным. Поток маленьких файлов ломает это допущение.
- Файл меньше блока всё равно занимает отображение блока. Вы платите памятью NameNode за мало данных.
- MapReduce сопоставляет split задаче. Куча крошечных файлов становится кучей задач и теми накладными расходами каркаса, о которых предупреждает учебник.
- Смягчение, которое остаётся внутри документации: пишите большие файлы, дайте RecordWriter упаковать много записей в один выходной путь и используйте разделы Hive только там, где предикат может отсечь скан. Не создавайте один файл HDFS на событие.
- Репликация умножает то, что вы сохранили. Коэффициент три на миллионах крошечных файлов умножает метаданные и блоки.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
</details>

### Troubleshooting and Maintenance

<details>
<summary>Как бы вы recover a Hadoop cluster from a Namenode failure?</summary>

### Как бы вы recover a Hadoop cluster from a Namenode failure?

**Definition:**

Восстановление метаданных использует последний согласованный FsImage и EditLog или запасной NameNode, который кормит Journal. Данные блоков всё ещё на DataNode.

**How it works:**

- Дизайн называет три обычных отказа: NameNode, DataNode и сетевые разделения.
- FsImage и EditLog центральны. NameNode может держать несколько копий и обновлять их синхронно. При перезапуске он выбирает последнюю согласованную пару.
- Рекомендуемый путь HA — несколько NameNode с распределённым журналом правок, Journal, а не только общий NFS.
- Пока NameNode лежит, лежит пространство имён. DataNode всё ещё держат блоки, но клиентам нужен NameNode, чтобы разрешать пути. Повторная репликация блоков это не чинит.
- Когда пространство имён вернулось, недореплицированные блоки — отдельный проход: heartbeat и повторная репликация. Снимки могут откатить порченый экземпляр к известному времени.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Какие соображения should be made for Hadoop cluster backup and disaster recovery?</summary>

### Какие соображения should be made for Hadoop cluster backup and disaster recovery?

**Definition:**

Сохраняйте пространство имён и держите реплики блоков. Снимок может вернуть экземпляр к известному времени. Репликация — не резервная копия плохой записи.

**How it works:**

- FsImage и EditLog — метаданные, которые нельзя собрать из одних DataNode. Несколько синхронных копий или Journal в HA — защита из дизайна. План резервного копирования должен включать эти файлы.
- Реплики блоков переживают диск DataNode. Они не переживают приложение, которое перезаписало или удалило файл на каждой реплике. Снимки хранят копию на момент времени и могут откатить порченый экземпляр.
- put -l форсирует коэффициент репликации 1, и документация говорит, что это снижает долговечность. Не используйте его для данных, которые надо восстановить.
- Восстановление старого FsImage отматывает пространство имён. Файлы, созданные после этого образа, исчезают из пространства имён, даже если блоки остались.
- Вычислительные задания заменяемы. WordCount можно отправить снова. Файл, который он читал, нельзя, если реплики или снимок его больше не держат.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Как бы вы monitor the health of a Hadoop cluster, and what tools would you use?</summary>

### Как бы вы monitor the health of a Hadoop cluster, and what tools would you use?

**Definition:**

Смотрите живость NameNode, heartbeat DataNode, недореплицированные блоки и прогресс задач MapReduce.

**How it works:**

- DataNode шлют heartbeat и отчёты о блоках. Отсутствие heartbeat — то, как NameNode объявляет узел мёртвым. Длинный таймаут по умолчанию значит, что дашборд должен показывать и устаревшие узлы, которые дизайн позволяет пометить раньше.
- Недореплицированные блоки — очередь, которую NameNode пытается залечить. Растущая очередь после отказа диска ожидаема. Очередь, которая никогда не рассасывается, — нет.
- Ошибки контрольных сумм значат, что клиенты отвергают реплики. Это порча, не медлительность.
- Учебник MapReduce показывает счётчики, журналы задач и статус отправки через waitForCompletion. Застрявший reducer — сигнал перекоса.
- Здоровье FsImage и EditLog — сигнал метаданных. HA с Journal должен показывать, какой NameNode активен. Собственный интерфейс Spark — другой процесс, когда задание — Spark на YARN.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Обсудите a strategy for Hadoop cluster CAPacity planning and scaling.</summary>

### Обсудите a strategy for Hadoop cluster CAPacity planning and scaling.

**Definition:**

Планируйте байты как блоки на DataNode, а метаданные как объекты пространства имён на NameNode. Они масштабируются не одинаково.

**How it works:**

- Блок 128 MB и коэффициент репликации три превращают терабайт пользовательских данных в больше чем три терабайта диска плюс трафик конвейера на записи.
- NameNode держит карту этих блоков. Большие файлы держат карту меньше. Маленькие файлы исчерпывают её первыми.
- Добавляйте DataNode, чтобы добавить диск и локальные слоты map. Ребаланс может перенести данные, когда свободное место неравномерно. Дизайн говорит, что архитектура совместима со схемами ребаланса.
- Слоты reduce и map добавляйте с учётом накладных расходов из учебника. Оставьте место для speculative-tasks и отказов.
- Для вычислений сверх MapReduce добавьте ёмкость YARN и запускайте Spark с явной памятью executor. Пространство имён HDFS — всё ещё один NameNode, если вы не в схеме HA с Journal, и это всё ещё одно пространство имён.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

### Scenario-Based Questions and Use Case Discussions

<details>
<summary>Компания хочет обрабатывать clickstream data в real-time. Как интегрировать Hadoop и Spark?</summary>

### Компания хочет обрабатывать clickstream data в real-time. Как интегрировать Hadoop и Spark?

**Definition:**

Кладите поток файлами в HDFS и дайте Spark на YARN преобразовать эти файлы. HDFS — долговечное хранилище. Spark — параллельное вычисление.

**How it works:**

- hadoop fs -put или конвейер записи кладёт байты на DataNode с репликацией. Не используйте put -l, если нужен обычный коэффициент.
- spark-submit --master yarn запускает приложение на том же кластере. Режим cluster держит драйвер в application master. HADOOP_CONF_DIR должен указывать на клиентскую конфигурацию Hadoop.
- RDD из textFile разрезан по кластеру. map и flatMap преобразуют строки. join — это join RDD пар. persist, если несколько действий перечитывают одни данные.
- HDFS сделан для высокой пропускной способности, не для интерактивных правок с малой задержкой. Настоящий путь «событие за событием» всё равно должен ограничить буфер. Пакетный файл в HDFS — долговечная копия, которую дизайн реально задаёт.
- Держите файлы большими. Файл на каждый клик исчерпает NameNode. Сворачивайте клики в файлы порядка блока 128 MB.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Предложите a data pipeline using Hadoop components to manage and analyze sensor data from IoT devices.</summary>

### Предложите a data pipeline using Hadoop components to manage and analyze sensor data from IoT devices.

**Definition:**

Копируйте файлы датчиков в HDFS, держите их большими и реплицированными, затем суммируйте через Hive или MapReduce и при необходимости соединяйте в Spark.

**How it works:**

- hadoop fs -put кладёт файлы. Долговечность держит репликация, не put -l. Блоки обычно 128 MB и идут конвейером на DataNode-реплики.
- Задание MapReduce или SELECT … GROUP BY в Hive суммирует по датчику или по часу. Столбец раздела в WHERE отсекает скан Hive.
- join в Spark соединяет сводку со справочной таблицей, если обе — RDD пар, и работает на YARN по тем же путям HDFS.
- NameNode должен оставаться небольшим. Сворачивайте показания в большие файлы. Наблюдайте heartbeat и недореплицированные блоки.
- Снимки или NameNode на Journal — путь восстановления. Реплики защищают диски. Они не защищают плохое удаление на каждой копии.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

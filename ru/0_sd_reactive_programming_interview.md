# Reactive Programming — собеседование

Ответы покрывают полный набор вопросов по реактивному программированию.

[← К обзору](./0_system_design.md)

### Related docs

- [Reactive Programming — собеседование](./0_sd_reactive_programming_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Reactive Programming](https://devinterview.io/questions/web-and-mobile-development/reactive-programming-interview-questions/).

### Reactive Programming Fundamentals

<details>
<summary>Что такое reactive programming, and how does it differ from Procedural Programming?</summary>

### Что такое reactive programming, and how does it differ from Procedural Programming?

**Definition:**

Реактивное программирование описывает вычисление как цепочку реакций на значения, которые приходят со временем. Процедурный код сам запрашивает следующее значение и идёт дальше.

**How it works:**

- Реактивная цепочка собирается из источника, операторов и подписчика. Работа начинается, когда кто-то подписался, и значения толкают вниз по цепочке.
- Процедурный код тянет: цикл читает список, вызывает функцию и сохраняет возврат до следующей строки.
- Реактивный код собирает асинхронные шаги (map, filter, flatMap) без вложенных колбэков. Ошибка и завершение — события той же последовательности, а не отдельный канал возврата.
- Стиль окупается на событиях интерфейса, ответах сети и тиках датчиков. Прямому сценарию по готовому массиву он не нужен.

**Examples:**

```text
процедурно: items = load(); for item in items: save(transform(item))
реактивно:  load().map(transform).subscribe(save)
```

Вторая форма может отдать `item`, как только пришёл ответ, и `save` выполнится тогда, а не после того, как весь список уже есть.
</details>

<details>
<summary>Объясните the concept of data streams in reactive programming.</summary>

### Объясните the concept of data streams in reactive programming.

**Definition:**

Поток данных — последовательность элементов во времени плюс завершающий сигнал, что последовательность сломалась или закончилась.

**How it works:**

- Каждый элемент — это `onNext`. Сбой — один `onError`. Нормальный конец — `onComplete`. После любого завершающего сигнала новых элементов нет.
- Источник может быть конечным (список) или бесконечным (клики, тики). Операторы стоят между источником и потребителем и возвращают новый поток.
- Холодный поток начинает работу на каждого подписчика. Горячий уже идёт, и подписчик видит то, что случается с момента подписки.
- Если источник быстрее потребителя, потоку нужна политика: копить, отбрасывать или просить источник замедлиться (backpressure).
</details>

<details>
<summary>Что такое the observer pattern, and how is it fundamental to reactive programming?</summary>

### Что такое the observer pattern, and how is it fundamental to reactive programming?

**Definition:**

Паттерн «наблюдатель» позволяет одному субъекту уведомлять многих зависимых, когда состояние меняется. Реактивные потоки — этот паттерн плюс контракт на последовательности, ошибки и отмену.

**How it works:**

- Субъект держит подписчиков. Когда что-то происходит, он вызывает каждого, а не заставляет их опрашивать себя.
- Реактивные библиотеки называют субъект Observable (или Publisher), а зависимого — Observer (или Subscriber).
- Дополнительный контракт — упорядоченные события: ноль или больше значений, затем либо ошибка, либо завершение, и способ отмены.
- Оператор — это наблюдатель, который сам является наблюдаемым, поэтому цепочка — конвейер одного и того же паттерна.

**Examples:**

Кнопка — субъект. Каждый слушатель — наблюдатель. Реактивная цепочка — та же идея, только между кликом и слушателем стоят `map` и `filter`, а `dispose` отсоединяет слушателя.
</details>

<details>
<summary>Опишите the role of Observables and observers in reactive programming.</summary>

### Опишите the role of Observables and observers in reactive programming.

**Definition:**

Observable — источник, который может выдавать элементы. Observer — потребитель, который их принимает. Пока наблюдатель не подписался, ничего не течёт.

**How it works:**

- Observable задаёт, как рождаются значения. Observer реализует `onNext`, `onError` и `onComplete`.
- Subscribe соединяет их и возвращает ручку для отмены. Каждая подписка на холодный источник может начать свою работу.
- Операторы возвращают новый observable. Нижний наблюдатель не видит тип верхнего источника; он видит преобразованную последовательность.
- В Reactive Streams имена — Publisher и Subscriber, и подписчик ещё запрашивает, сколько элементов готов принять.

**Examples:**

```text
source = Observable.just(1, 2, 3)
source.map(lambda n: n * 10).subscribe(
    on_next=print,
    on_error=log,
    on_complete=lambda: print("done"),
)
```
</details>

<details>
<summary>Как you create an Observable stream?</summary>

### Как you create an Observable stream?

**Definition:**

Либо оборачивают уже готовые значения, либо описывают источник, который вызовет наблюдателя, когда что-то произойдёт.

**How it works:**

- Фабрики закрывают обычные случаи: `just` и `fromIterable` для готовых значений, `interval` и `timer` для времени, `empty`, `never` и `error` для завершающих случаев.
- `defer` строит новый observable на каждого подписчика — так холодный вызов остаётся холодным.
- `create` — запасной выход: функция получает emitter и должна вызвать `onNext`, затем `onComplete` или `onError`, и остановиться, когда подписку отменили.
- Операторы вроде `map` не создают исходный источник, а оборачивают его. Поток ленив до `subscribe`.

**Examples:**

```text
ticks = Observable.interval(1, TimeUnit.SECONDS)
one_shot = Observable.defer(() -> Observable.just(fetchStatus()))
```

`interval` запускает часы в момент подписки. `defer` вызывает `fetchStatus` один раз на подписчика, а не когда цепочку собрали.
</details>

<details>
<summary>Что такое backpressure in the context of reactive programming?</summary>

### Что такое backpressure in the context of reactive programming?

**Definition:**

Backpressure — способ медленному подписчику сказать быстрому издателю, сколько элементов он готов принять, чтобы издатель не переполнил память.

**How it works:**

- В Reactive Streams подписчик вызывает `request(n)`. Издатель отдаёт не больше этого числа и ждёт следующий запрос.
- Без этого сигнала неограниченный источник, например сетевой поток, заполнит очередь, пока процесс не исчерпает память.
- Если источник нельзя замедлить, стратегия всё равно выбирает: буфер с пределом, отбросить новые или старые, оставить только последнее или завершить поток ошибкой.
- RxJava `Flowable` говорит на этом протоколе. `Observable` — нет; он предполагает, что потребитель успевает, или что вы поставили оператор вроде `onBackpressureBuffer`.

**Examples:**

Парсер запрашивает 32 строки. Источник файла читает 32 и останавливается. Когда парсер закончил порцию, он запрашивает ещё 32. Поток интерфейса, который нельзя поставить на паузу, может оставлять только последний клик.
</details>

<details>
<summary>Объясните the difference between cold and hot Observables.</summary>

### Объясните the difference between cold and hot Observables.

**Definition:**

Холодный observable начинает отдельную последовательность для каждого подписчика. Горячий делит одну последовательность, которая уже идёт.

**How it works:**

- Холодный: второй подписчик повторяет работу. Так ведут себя `just`, `fromIterable` и свежий HTTP-вызов через `defer`. Каждый подписчик может увидеть всю последовательность с начала.
- Горячий: подписчики делят один источник. Опоздавший пропускает ранние элементы. Клики, тики датчика и опубликованный subject — горячие.
- `publish` или `share` превращают холодный источник в горячий, чтобы сетевой вызов выполнился один раз. `replay` держит буфер, и опоздавший всё же видит недавние элементы.
- Холодный выбирают, когда каждому подписчику нужен свой результат. Горячий — когда событие существует независимо от того, слушает ли кто-то.
</details>

<details>
<summary>Какова роль Subscription в Reactive Programming.</summary>

### Какова роль Subscription в Reactive Programming.

**Definition:**

Подписка — связь, которую создаёт subscribe: она запускает поток и служит ручкой, чтобы отменить его или запросить ещё элементы.

**How it works:**

- Subscribe в RxJava возвращает `Disposable`, а в Reactive Streams — `Subscription`. Dispose отписывает и должен отпустить сокеты, таймеры и слушатели.
- У `Flowable` подписка — ещё и способ подписчика вызвать `request(n)` для backpressure.
- Один наблюдатель может держать много подписок. Составной disposable отменяет всю группу, когда экран закрывается.
- Потерянная ручка оставляет верхний поток работающим. Так обычно течёт слушатель после того, как экран уже уничтожен.

**Examples:**

```text
Disposable d = ticks.subscribe(this::onTick);
// позже, когда экран закрывается
d.dispose();
```
</details>

<details>
<summary>Как you unsubscribe from a stream to prevent memory leaks?</summary>

### Как you unsubscribe from a stream to prevent memory leaks?

**Definition:**

Вызовите dispose у подписки, когда время жизни потребителя кончилось, и очистите составной контейнер, куда эти подписки складывали.

**How it works:**

- `Disposable.dispose()` останавливает верхний поток и отпускает ссылку на наблюдателя, чтобы цепочку можно было собрать.
- Каждую подписку кладут в `CompositeDisposable`, которым владеет экран, презентер или область. Этот контейнер чистят в парном завершении (`onDestroy`, отмена scope, размонтирование компонента).
- Операторы вроде `takeUntil(destroySignal)` и `take(1)` заканчивают последовательность без ручного disposable. Это безопаснее, когда завершение само является потоком.
- Dispose идемпотентен. Дважды отменять безопасно. Не отменённый горячий источник или `interval` держит поток или слушателя живым.

**Examples:**

```text
CompositeDisposable bag = new CompositeDisposable();
bag.add(interval.subscribe(this::onTick));
bag.add(clicks.subscribe(this::onClick));
// onDestroy
bag.clear();
```
</details>

<details>
<summary>Что такое operators in reactive programming, and what are they used for?</summary>

### Что такое operators in reactive programming, and what are they used for?

**Definition:**

Операторы — функции из одного потока в другой. Они преобразуют, фильтруют, объединяют или меняют время элементов, и подписчику не нужно знать, как устроен источник.

**How it works:**

- Преобразующие операторы: `map` (один элемент в один) и `flatMap` (один элемент в поток, который сливается).
- Фильтрующие: `filter`, `distinct`, `take`, `skip` и `debounce`.
- Объединяющие: `zip`, `merge`, `concat` и `combineLatest`. Они различаются тем, ждут ли они, паруют ли по индексу или перемешивают.
- Операторы ошибок и побочных эффектов: `retry`, `onErrorReturn` и `doOnNext`. Оператор планировщика вроде `observeOn` переносит нижнюю работу на другой поток.
- Каждый оператор подписывается на верхний поток, только когда подписались на него самого, поэтому длинная цепочка всё ещё ленива.
</details>

### Reactive Programming with RxJava

<details>
<summary>Что такое RxJava, and how does it implement reactive programming?</summary>

### Что такое RxJava, and how does it implement reactive programming?

**Definition:**

RxJava — Java-библиотека наблюдаемых последовательностей и операторов. На JVM реактивные потоки обычно потребляют через неё.

**How it works:**

- Базовые типы соответствуют тому, сколько элементов ждут: `Observable` и `Flowable` — много, `Single` — один, `Maybe` — один или ни одного, `Completable` — ни одного.
- `Flowable` реализует backpressure Reactive Streams. `Observable` кредит не запрашивает; его берут, когда объём ограничен или уже идёт в ритме интерфейса.
- Цепочки ленивы до subscribe. Планировщики переносят работу момента подписки и работу момента наблюдения на пулы потоков.
- Последовательность для одной подписки — один источник элементов: значения, затем одно завершающее событие. Источники по времени вроде `interval` — обычные observable.

**Examples:**

```text
Single<User> user = api.user(id)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread());
```
</details>

<details>
<summary>Как RxJava handle multithreading?</summary>

### Как RxJava handle multithreading?

**Definition:**

RxJava сам поток не выбирает. `subscribeOn` задаёт, где подписывается источник, а `observeOn` — где выполняются дальнейшие операторы и наблюдатель.

**How it works:**

- `Schedulers.io()` — растущий пул для блокирующего ввода-вывода. `Schedulers.computation()` по размеру близок к числу ядер и предназначен для неблокирующей работы. `Schedulers.single()` — один общий поток. `trampoline` ставит работу в очередь текущего потока.
- На Android `AndroidSchedulers.mainThread()` возвращает работу на UI-поток. Трогать view из `io` — это падение.
- Ближайший к источнику `subscribeOn` побеждает, если их несколько в цепочке; лишние выше него подписку повторно не переносят. Каждый `observeOn` переключает поток с этого места и ниже.
- Операторы не становятся потокобезопасными относительно общей изменяемой переменной. Состояние держите внутри цепочки или синхронизируйте сами.
</details>

<details>
<summary>Объясните how the `flatMap` operator works in RxJava.</summary>

### Объясните how the `flatMap` operator works in RxJava.

**Definition:**

`flatMap` превращает каждый верхний элемент во внутренний observable и сливает эти внутренние потоки. Элементы разных внутренних потоков могут приходить вперемешку.

**How it works:**

- Внешняя функция возвращает `Observable<R>` или другой реактивный тип. `flatMap` подписывается на каждый внутренний поток и передаёт его элементы вниз как одну последовательность.
- Внутренние потоки идут параллельно до аргумента max-concurrency. Порядок не совпадает с порядком источника. `concatMap` ждёт завершения одного внутреннего, прежде чем подписаться на следующий. `switchMap` отменяет предыдущий внутренний, когда приходит новый внешний элемент.
- Ошибка любого внутреннего потока роняет нижний, если её не обработали внутри.
- Его берут, когда одно значение должно стать следующим асинхронным вызовом, например идентификатор пользователя — заказами этого пользователя.

**Examples:**

```text
userIds.flatMap(id -> ordersApi.list(id))
```

Если заказы пользователя 2 вернулись раньше заказов пользователя 1, эти строки всё равно сразу идут вниз. `concatMap` задержал бы пользователя 2, пока не закончится вызов пользователя 1.
</details>

<details>
<summary>Что такое the puRPOse of the `zip` operator in RxJava?</summary>

### Что такое the puRPOse of the `zip` operator in RxJava?

**Definition:**

`zip` парует элементы двух или более источников по индексу и выдаёт один составной элемент, когда у каждого источника есть следующий индекс.

**How it works:**

- Функция-застёжка получает по элементу от каждого источника. Выход ждёт самый медленный источник на этом индексе.
- Последовательность завершается, когда завершается самый короткий источник. Лишние элементы более длинного источника отбрасываются.
- Ошибка любого источника роняет zip. Источники обычно остаются холодными и подписка на них идёт вместе.
- Он подходит независимым вызовам, которые нужны вместе: профиль и настройки одного экрана или значение с меткой времени из второго потока. «Последнее от каждого» — это `combineLatest`, не zip.

**Examples:**

```text
Observable.zip(profileApi.get(id), settingsApi.get(id),
    (profile, settings) -> new Screen(profile, settings))
```

Объект экрана выдаётся один раз, когда оба вызова дали результат. Если настройки выдали два элемента, а профиль один, получится только первая пара.
</details>

<details>
<summary>Как you handle errors in an RxJava stream?</summary>

### Как you handle errors in an RxJava stream?

**Definition:**

Ошибка завершает последовательность: после `onError` она больше ничего не выдаёт. Восстановление — превратить сбой в значение, другую последовательность или повтор, прежде чем он дойдёт до подписчика.

**How it works:**

- Пусть ошибка дойдёт до `onError` в subscribe, если интерфейс должен показать сбой. Не глотайте её пустым колбэком.
- `onErrorReturn` заменяет ошибку запасным элементом и завершается. `onErrorResumeNext` переключается на другой observable, например кэш.
- `retry(n)` заново подписывается на верхний поток. Это повторяет холодный вызов. Повтор горячего источника не отматывает события, которые уже прошли.
- `doOnError` пишет лог или метрику и всё равно пропускает ошибку дальше. Операторы восстановления ставят выше `observeOn`, если запасной путь нужно выбрать на рабочем потоке.
</details>

<details>
<summary>Опишите the differences between `Map` and `flatMap` operators in RxJava.</summary>

### Опишите the differences between `Map` and `flatMap` operators in RxJava.

**Definition:**

`map` превращает каждый элемент в один элемент. `flatMap` превращает каждый элемент во внутренний observable и сливает эти внутренние последовательности в одну.

**How it works:**

- Оператор map в ReactiveX применяет функцию к каждому элементу источника и возвращает observable результатов. map в RxJava по умолчанию не работает на каком-то определённом Scheduler.
- Функция flatMap возвращает observable на элемент. FlatMap сливает эти observable, поэтому их элементы могут перемешиваться. Если любой внутренний сигналит onError, внешний сразу завершается ошибкой.
- concatMap — упорядоченный родственник: он подписывается на следующий внутренний поток только после завершения предыдущего. switchMap отписывается от предыдущего, когда приходит новый внешний элемент.
- Вариант flatMap принимает int, который ограничивает, сколько внутренних подписок активно сразу. На пределе он ждёт завершения одного внутреннего, прежде чем подписаться на другой.
- map — чтобы возвести число в квадрат или переименовать поле. flatMap — когда элемент должен стать другим асинхронным вызовом, например идентификатор становится списком заказов.

**Examples:**

```text
numbers.map(n -> n * n)                 // 1, 2, 3 -> 1, 4, 9
numbers.flatMap(n -> Observable.just(n * 2, n * 3))
// слияние; порядок между внутренними потоками не гарантирован
```

**Sources:**

- https://reactivex.io/documentation/operators/map.html
- https://reactivex.io/documentation/operators/flatmap.html
</details>

<details>
<summary>Что такое `Scheduler` in RxJava?</summary>

### Что такое `Scheduler` in RxJava?

**Definition:**

Scheduler — абстракция конкурентности, которой ReactiveX переносит работу операторов с потока, где подписались. `subscribeOn` выбирает, где стартует источник; `observeOn` — куда доставляются дальнейшие уведомления.

**How it works:**

- По умолчанию цепочка выполняется и уведомляет наблюдателей в потоке, где вызвали `subscribe`.
- `subscribeOn` задаёт scheduler, на котором работает observable, независимо от места оператора в цепочке.
- `observeOn` меняет поток, которым уведомления уходят вниз от этого оператора.
- Стандартный набор RxJava, описанный в readme библиотеки, включает computation, кэширующий scheduler для ввода-вывода, single для последовательной работы и trampoline.

**When to use:**

`subscribeOn` ставят один раз у источника, если создание медленное или блокирующее. `observeOn` — там, где следующий этап должен идти в другом потоке, например у единственного потребителя.

**Examples:**

```java
Flowable.fromCallable(() -> "Done")
    .subscribeOn(Schedulers.io())
    .observeOn(Schedulers.single())
    .subscribe(System.out::println, Throwable::printStackTrace);
```

**Sources:**

- https://reactivex.io/documentation/scheduler.html
- https://github.com/reactivex/rxjava/blob/4.x/README.md
- https://reactivex.io/RxJava/2.x/javadoc/io/reactivex/schedulers/Schedulers.html
</details>

<details>
<summary>Объясните the `Subject` class in RxJava.</summary>

### Объясните the `Subject` class in RxJava.

**Definition:**

Subject — одновременно Observer и Observable. Он может подписаться на источник и переизлучать увиденное, а также излучать собственные элементы своим наблюдателям.

**How it works:**

- Подписка Subject на холодный источник запускает этот источник. Дальше Subject ведёт себя как горячее представление источника.
- `AsyncSubject` излучает только последний элемент и только после завершения источника. Ошибка сверху пробрасывается, элемент не излучается.
- `BehaviorSubject` начинает нового наблюдателя с последнего элемента или с начального значения, если излучений ещё не было, и затем продолжает.
- `PublishSubject` излучает только элементы, пришедшие после подписки. `ReplaySubject` повторяет прежние элементы новому наблюдателю.

**When to use:**

Обычный observable — когда каждый подписчик должен получить свою холодную последовательность. Subject — когда многие подписчики делят одну живую последовательность; вариант выбирают по тому, сколько истории нужно опоздавшему.

**Examples:**

```java
PublishSubject<String> ticks = PublishSubject.create();
ticks.subscribe(System.out::println);
ticks.onNext("a");
ticks.onNext("b");
```

**Sources:**

- https://reactivex.io/documentation/subject.html
- https://github.com/ReactiveX/RxJava/wiki/Subject
</details>

<details>
<summary>Как you achieve thread-safety in RxJava?</summary>

### Как you achieve thread-safety in RxJava?

**Definition:**

Контракт Rx предполагает, что уведомления одному наблюдателю идут последовательно. Вики RxJava про Subject говорит, что вызовы в Subject из нескольких потоков небезопасны, и сериализующая обёртка восстанавливает этот контракт.

**How it works:**

- ReactiveX говорит, что на стороне наблюдателя у Subject параллельные `onNext` из нескольких потоков могут оставить неопределённое состояние.
- Вики RxJava показывает `new SerializedSubject(unsafe)`, чтобы эти вызовы выстроились в одну последовательность.
- `subscribeOn` и `observeOn` переносят работу между scheduler, но не делают произвольный Subject безопасным, если вы сами зовёте `onNext` из двух потоков.
- Лучше излучать из одного потока или из операторов, которые и так переключаются через `observeOn`, а не делить изменяемый накопитель.

**Examples:**

```java
Subject<Integer> raw = PublishSubject.create();
Subject<Integer> safe = new SerializedSubject<>(raw);
safe.subscribe(System.out::println);
safe.onNext(1);
```

**Sources:**

- https://reactivex.io/documentation/subject.html
- https://github.com/ReactiveX/RxJava/wiki/Subject
- https://reactivex.io/documentation/scheduler.html
</details>

<details>
<summary>Что такое the `debounce` operator in RxJava?</summary>

### Что такое the `debounce` operator in RxJava?

**Definition:**

`debounce` (также `throttleWithTimeout`) излучает элемент, только если за заданный промежуток источник не излучил следующий. Последний элемент всё равно излучается, если внутри окна за ним ничего не пришло.

**How it works:**

- Элементы, за которыми быстро приходит другой, отбрасываются.
- Промежуток задают длительностью и единицей времени. Аргумент Scheduler выбирает, где идёт таймер; без него оператор берёт свой scheduler по умолчанию.
- Вариант вызывает функцию на элемент и ждёт observable, который она вернула. Новый элемент источника до завершения этого внутреннего отбрасывает предыдущий.
- Вики о backpressure ставит `debounce` рядом с `sample` и `throttleFirst` как способ сжать всплеск до медленного потребителя.

**When to use:**

`debounce` — для всплеска, где важен элемент после паузы, например поле поиска. `sample` — когда нужен последний элемент по фиксированным часам, а не после тишины.

**Examples:**

```java
Observable<Integer> burstyDebounced =
    bursty.debounce(10, TimeUnit.MILLISECONDS);
```

**Sources:**

- https://reactivex.io/documentation/operators/debounce.html
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

### Reactive Programming with ReactiveX / RxJS

<details>
<summary>Что такое Reactive Extensions (ReactiveX or Rx) and RxJS?</summary>

### Что такое Reactive Extensions (ReactiveX or Rx) and RxJS?

**Definition:**

ReactiveX — семейство библиотек, которые собирают асинхронные события как наблюдаемые последовательности. RxJS — член этого семейства для JavaScript, RxJava — для JVM. Одни и те же имена операторов — map, zip, debounce, subjects, schedulers — описаны один раз и реализованы в каждом языке.

**How it works:**

- Observable толкает наблюдателю элементы, ошибку или завершение.
- Операторы вроде `zip` и `debounce` — функции из одной последовательности в другую. Они перечислены на страницах операторов ReactiveX, где RxJS и RxJava названы вместе.
- Subject — мост, который одновременно наблюдатель и observable; его делят реализации.
- Schedulers уводят цепочку с потока подписки. В браузере этот переход меньше; контракт оператора тот же.

**Examples:**

```java
// ReactiveX From, as implemented by RxJS `from` and RxJava `fromIterable`
Observable.fromIterable(java.util.List.of(1, 2, 3))
    .map(n -> n * 2);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://reactivex.io/documentation/operators.html
- https://reactivex.io/documentation/operators/from.html
</details>

<details>
<summary>Приведите an example of how to implement a simple RxJS Observable.</summary>

### Приведите an example of how to implement a simple RxJS Observable.

**Definition:**

Простой observable — функция, которая при подписке толкает элементы и затем завершение. `from` в ReactiveX превращает массив в такую последовательность; RxJS открывает её как `from`.

**How it works:**

- Ничего не излучается, пока кто-то не подпишется. Это холодное поведение, которое страница backpressure RxJava описывает для последовательности из статической коллекции.
- `from` обходит массив и излучает каждый элемент по порядку, затем завершается.
- Операторы вроде `map` возвращают новый observable. Сами они обход массива не запускают.
- Наблюдатель получает вызовы `next` и один завершающий сигнал. После завершения последовательность больше ничего не излучает.

**Examples:**

```javascript
import { from } from "rxjs";
import { map } from "rxjs/operators";

from([1, 2, 3]).pipe(map(n => n * 2)).subscribe(console.log);
```

**Sources:**

- https://reactivex.io/documentation/operators/from.html
- https://reactivex.io/documentation/observable.html
</details>

<details>
<summary>Как RxJS's `Observable` differ from a promise?</summary>

### Как RxJS's `Observable` differ from a promise?

**Definition:**

Promise завершается один раз, сразу, значением или отказом. Observable — последовательность: он может излучить много элементов, стартует при подписке и его можно остановить отпиской.

**How it works:**

- Создание Promise запускает исполнитель. Создание Observable из массива не обходит его до `subscribe` — это холодное поведение статического источника.
- Observable может много раз вызвать `next` и затем `complete` или `error`. У Promise одно разрешение.
- Операторы вроде `map` и `debounce` собирают последовательность. Цепочка Promise — это `then` над одним будущим значением.
- Отписка убирает наблюдателя. Promise в том же смысле не отменяется: он всё равно завершится.

**When to use:**

Promise — для одного результата, который должен начаться сразу. Observable — когда значений много, когда работа должна ждать подписки или когда потребитель должен иметь возможность отписаться.

**Examples:**

```javascript
// one settlement
Promise.resolve(1).then(console.log);

// a sequence, started by subscribe
from([1, 2, 3]).subscribe(console.log);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://reactivex.io/documentation/operators/from.html
</details>

<details>
<summary>Объясните the concept of marble diagrams in RxJS.</summary>

### Объясните the concept of marble diagrams in RxJS.

**Definition:**

Мраморная диаграмма рисует один observable как стрелку времени. Шарики — элементы, вертикальная черта — завершение, крест — ошибка. Страницы операторов ReactiveX этим рисунком показывают входные стрелки и выходную.

**How it works:**

- Время идёт вправо. Горизонтальный зазор между шариками — зазор между излучениями.
- Диаграмма на странице debounce отбрасывает шарики, за которыми слишком быстро пришёл следующий, и оставляет шарик после паузы, включая последний.
- Вторая стрелка под первой — результат оператора, а не вторая подписка.
- Завершение источника заканчивает выход. Ошибка источника заканчивает его ошибкой.

**Examples:**

```text
source:   --a--b-c------d--|
debounce: -----b------c------d--|
```

**Sources:**

- https://reactivex.io/documentation/operators/debounce.html
- https://reactivex.io/documentation/operators.html
</details>

<details>
<summary>В RxJS, how would you convert an array to an Observable sequence?</summary>

### В RxJS, how would you convert an array to an Observable sequence?

**Definition:**

Оператор ReactiveX `from` превращает массив, итерируемое или похожую структуру в observable, который излучает каждый элемент и затем завершается. RxJS называет эту функцию `from`; RxJava 2 называет форму для итерируемого `fromIterable`.

**How it works:**

- Подписка начинает обход. Второй подписчик обходит массив снова, потому что статический источник холодный.
- Порядок — порядок массива. Таймера нет, пока его не добавят.
- `map` после `from` преобразует каждый элемент и не меняет завершение.
- Пустой массив завершается без `next`.

**Examples:**

```javascript
from([1, 2, 3]).subscribe({
  next: console.log,
  complete: () => console.log("done")
});
```

**Sources:**

- https://reactivex.io/documentation/operators/from.html
</details>

<details>
<summary>Опишите the `switchMap` operator in RxJS.</summary>

### Опишите the `switchMap` operator in RxJS.

**Definition:**

ReactiveX называет этот оператор Switch. Он берёт observable, который излучает другие observable, и излучает элементы только самого недавнего внутреннего observable, отписываясь от предыдущего.

**How it works:**

- Switch подписывается на внутренний observable, когда внешний его излучил.
- Когда внешний излучает более новый внутренний, от предыдущего отписываются. Поздние элементы старого внутреннего игнорируются.
- RxJS записывает тот же оператор как `switchMap`: функция возвращает внутренний observable для каждого внешнего элемента.
- Это не слияние всех внутренних. Активен только один.

**When to use:**

`switchMap` — когда новый внешний элемент делает предыдущий внутренний ненужным, например последний поиск. `flatMap` — когда каждый внутренний нужно слить; этот контракт для flatMap уже зафиксирован.

**Examples:**

```javascript
// RxJS name for ReactiveX Switch
source.pipe(switchMap(id => from(load(id))));
```

**Sources:**

- https://reactivex.io/documentation/operators/switch.html
</details>

<details>
<summary>Как you handle backpressure in RxJS?</summary>

### Как you handle backpressure in RxJS?

**Definition:**

Backpressure — это что делать, когда источник излучает быстрее, чем потребитель успевает забирать. В RxJS нет pull-модели Reactive Streams с `request(n)`. Первая защита со страницы backpressure RxJava всё равно применима: сожмите последовательность через `debounce`, `sample` или `buffer`, прежде чем она дойдёт до подписчика.

**How it works:**

- Вики о backpressure показывает, как `debounce`, `sample` / `throttleLast` и `throttleFirst` срезают всплеск.
- `buffer` собирает элементы и излучает их списками, поэтому подписчик видит меньше, но более крупные уведомления.
- Холодный источник из `from` и так излучает лишь пока синхронный подписчик забирает. Горячий источник, например события UI, не ждёт.
- Библиотеки JVM добавляют `onBackpressureBuffer` и `onBackpressureDrop`, потому что реализуют `request`. Эти два оператора — не методы RxJS.

**Examples:**

```javascript
fromEvent(input, "input").pipe(
  debounceTime(10),
  map(event => event.target.value)
);
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://reactivex.io/documentation/operators/debounce.html
</details>

<details>
<summary>Что такое the `BehaviorSubject` in RxJS?</summary>

### Что такое the `BehaviorSubject` in RxJS?

**Definition:**

`BehaviorSubject` — вариант Subject, который при подписке излучает самый недавний элемент или начальное значение, если излучений ещё не было, а затем излучает дальнейшие.

**How it works:**

- Страница Subject в ReactiveX формулирует этот контракт прямо.
- Опоздавший подписчик не пропускает текущее значение. Всю историю он не видит; это уже `ReplaySubject`.
- Он горячий: значение есть независимо от того, подписан ли кто-то.
- Как у любого Subject, параллельные излучения из нескольких потоков не входят в контракт. RxJS обычно однопоточен; вики JVM всё равно говорит сериализовать Subject.

**Examples:**

```javascript
const selected = new BehaviorSubject("none");
selected.subscribe(console.log); // "none"
selected.next("row-1");
```

**Sources:**

- https://reactivex.io/documentation/subject.html
</details>

<details>
<summary>Что такое the uses of the `scan` operator in RxJS?</summary>

### Что такое the uses of the `scan` operator in RxJS?

**Definition:**

`scan` применяет функцию к каждому элементу вместе с аккумулятором и излучает каждый промежуточный аккумулятор. Это бегущая свёртка последовательности.

**How it works:**

- Страница scan в ReactiveX описывает излучение накопленного значения на каждый элемент источника, а не только итога.
- Передают начальное значение. Первое излучение — начальное значение, соединённое с первым элементом.
- Последовательность по-прежнему завершается или падает вместе с источником. `scan` ошибку не прячет.
- Счётчик, растущая сумма или объект состояния, свёрнутый из событий, — обычные формы.

**Examples:**

```javascript
from([1, 2, 3]).pipe(
  scan((acc, n) => acc + n, 0)
);
// 1, 3, 6
```

**Sources:**

- https://reactivex.io/documentation/operators/scan.html
</details>

<details>
<summary>Как you do error recovery in RxJS?</summary>

### Как you do error recovery in RxJS?

**Definition:**

Ошибка для этой последовательности терминальна. Восстановление значит заменить сбой значением, другим observable или повторной подпиской, прежде чем ошибка дойдёт до подписчика.

**How it works:**

- Допустить ошибку до `subscribe` правильно, когда интерфейс должен показать сбой.
- Восстановление в ReactiveX следует тому же разделению, что и путь ошибок RxJava: вернуть запасной элемент, переключиться на другой observable или подписаться снова.
- Повторная подписка повторяет холодный источник вроде `from` или новый HTTP-вызов. Горячий Subject она не отматывает.
- Журнал как побочный эффект должен пробросить ошибку или вернуть канал ошибки. Пустой callback прячет сбой.

**Examples:**

```javascript
source.pipe(
  catchError(() => of([]))
);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://reactivex.io/documentation/operators/catch.html
</details>

### Reactive Streams and Project Reactor

<details>
<summary>Что такое the Reactive Streams specification?</summary>

### Что такое the Reactive Streams specification?

**Definition:**

Reactive Streams — стандарт асинхронной обработки потоков с неблокирующим backpressure. Цель — передавать элементы через асинхронную границу, не заставляя приёмник буферизовать произвольный объём.

**How it works:**

- Readme JVM говорит, что быстрый источник не должен захлёстывать приёмник, а очереди между потоками остаются ограниченными, потому что backpressure входит в модель.
- Сами сигналы backpressure должны быть асинхронными. Синхронный сигнал отменил бы пользу асинхронной обработки.
- Спецификация — это API плюс TCK. Дополнительные возможности допустимы, если реализация всё ещё проходит TCK.
- JDK `java.util.concurrent.Flow` семантически совпадает с этими типами один к одному. Преобразование потоков в спецификацию явно не входит.

**Examples:**

```java
// Maven coordinates from the Reactive Streams JVM readme
// org.reactivestreams:reactive-streams:1.0.4
Publisher<Integer> source = subscriber -> { };
```

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Опишите the main interfaces of the Reactive Streams API.</summary>

### Опишите the main interfaces of the Reactive Streams API.

**Definition:**

Readme JVM перечисляет четыре компонента: Publisher, Subscriber, Subscription и Processor. Publisher даёт возможно неограниченную последовательность по спросу своего Subscriber.

**How it works:**

- `Publisher.subscribe(Subscriber)` начинает протокол. Readme задаёт порядок сигналов `onSubscribe onNext* (onError | onComplete)`.
- Subscription — то, что подписчик получает в `onSubscribe`. Спрос идёт по этой подписке.
- Processor — одновременно Subscriber и Publisher, поэтому он может стоять между двумя стадиями.
- Спецификация не определяет map или filter. Это операторы библиотек поверх этих четырёх типов.

**Examples:**

```java
void onSubscribe(Subscription s);
void onNext(T item);
void onError(Throwable t);
void onComplete();
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://www.reactive-streams.org/
</details>

<details>
<summary>Как backpressure work within the Reactive Streams API?</summary>

### Как backpressure work within the Reactive Streams API?

**Definition:**

Подписчик сообщает издателю, сколько элементов готов принять. Издатель не должен излучать больше этого непогашенного спроса, поэтому очередь между ними остаётся ограниченной.

**How it works:**

- Сайт Reactive Streams говорит, что backpressure не даёт быстрому источнику захлестнуть приёмник.
- Спрос запрашивают на Subscription. Сигналы, включая этот запрос, обязаны быть неблокирующими.
- Страница backpressure RxJava показывает тот же pull в `zip`: маленький буфер на источник и ещё один элемент запрашивается только после излучения.
- `onBackpressureBuffer` и `onBackpressureDrop` — адаптеры RxJava для источника, который игнорирует `request`. В четыре интерфейса они не входят.

**Examples:**

```java
public void onSubscribe(Subscription s) {
  s.request(1);
}
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Что такое Project Reactor, and how is it related to Reactive Streams?</summary>

### Что такое Project Reactor, and how is it related to Reactive Streams?

**Definition:**

Project Reactor — библиотека Reactive Streams для JVM. `Flux` и `Mono` — это Publisher, поэтому тип Reactor можно подписать тем же протоколом спроса, что и любой другой publisher Reactive Streams.

**How it works:**

- Reactor добавляет операторы поверх спецификации. Сама спецификация, как говорит readme JVM, преобразования не описывает.
- `subscribeOn` и `publishOn` переносят работу между scheduler. Страница scheduler говорит, что ничего не происходит до subscribe, `publishOn` влияет на цепочку после себя, а `subscribeOn` — на подписку.
- `StepVerifier` подписывается и сравнивает сигналы; это инструмент тестов Reactor, не часть спецификации.
- Publisher не из Reactor не несёт Reactor `Context`. Страница context говорит, что распространение context требует операторов Reactor.

**Examples:**

```java
Mono<String> one = Mono.just("ok");
Flux<Integer> many = Flux.just(1, 2, 3);
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Сравните and contrast Mono and Flux in Project Reactor.</summary>

### Сравните and contrast Mono and Flux in Project Reactor.

**Definition:**

`Mono` — publisher Reactor на ноль или один элемент. `Flux` — publisher на от нуля до многих элементов. Оба завершаются или падают с ошибкой, и оба соблюдают спрос Reactive Streams.

**How it works:**

- `Mono.just` излучает одно значение и завершается. `Flux.just` излучает каждый аргумент и затем завершается.
- Те же scheduler применимы. `subscribeOn` по-прежнему выбирает только поток подписки; `publishOn` по-прежнему переключает цепочку ниже.
- `transform` документирован на цепочке `Flux` и так же работает на `Mono`: функция применяется во время сборки.
- `Mono` — для одного тела HTTP-ответа. `Flux` — для потока строк. Разворот `Flux` из `Mono` — оператор поверх спецификации, а не третий интерфейс.

**When to use:**

`Mono` — когда контракт это один результат или ни одного. `Flux` — когда контракт это последовательность.

**Examples:**

```java
Mono<String> body = Mono.just("ok");
Flux<String> rows = Flux.just("a", "b");
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://projectreactor.io/docs/core/release/reference/advancedFeatures/advanced-mutualizing-operator-usage.html
</details>

<details>
<summary>Как Project Reactor enable asynchronous and non-blocking operations?</summary>

### Как Project Reactor enable asynchronous and non-blocking operations?

**Definition:**

Reactor держит цепочку ленивой до subscribe, затем выполняет стадии на scheduler, чтобы медленный источник не блокировал вызывающего. Спрос подписчика ограничивает, сколько может быть в пути.

**How it works:**

- Страница scheduler: ничего не происходит, пока вы не подпишетесь. `subscribeOn` меняет поток, который подписывается. Его позиция не выбирает, где выполнится поздний оператор; важен ближайший `subscribeOn` ниже по цепочке.
- `publishOn` переключает поток для операторов после себя, до следующего `publishOn`.
- FAQ показывает блокирующий вызов, обёрнутый и поставленный на `subscribeOn` сразу после источника, а дальнейшие операторы — после этой обёртки.
- Reactive Streams требует, чтобы обмен, включая backpressure, был неблокирующим. Безграничный буфер это ломает.

**Examples:**

```java
Mono<String> blockingWrapper = Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Что такое some strategies for testing Reactor flows?</summary>

### Что такое some strategies for testing Reactor flows?

**Definition:**

`StepVerifier` подписывается на `Flux` или `Mono` и сравнивает каждый сигнал со следующим ожиданием. Первое несовпадение бросает `AssertionError`.

**How it works:**

- Верификатор создают на publisher, который возвращает ваш метод. Ожидания вроде `expectNext` и затем `verifyComplete` и описывают последовательность, и запускают её.
- `expectAccessibleContext` проверяет context Reactor, затем `then()` возвращает к ожиданиям последовательности.
- `StepVerifierOptions` может начать тест с начальным context.
- `expectNoAccessibleContext` — для publisher не типа Reactor, который поэтому не может распространить context.

**Examples:**

```java
StepVerifier.create(Mono.just(1).map(i -> i + 10))
    .expectNext(11)
    .verifyComplete();
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/testing.html
</details>

<details>
<summary>Как you convert blocking code to non-blocking Reactive code in Reactor?</summary>

### Как you convert blocking code to non-blocking Reactive code in Reactor?

**Definition:**

Изолируйте блокирующий вызов в `Callable` и подпишите этот `Mono` на scheduler, предназначенный для блокирующей работы. FAQ говорит, что `subscribeOn` сам не подписывается; он только выбирает scheduler на момент подписки.

**How it works:**

- Поставьте `subscribeOn` сразу после блокирующего источника. Дальнейшие операторы определяют после этой обёртки.
- `Schedulers.boundedElastic()` — scheduler, который FAQ использует для такой обёртки.
- Не вызывайте блокирующий метод внутри `map` на неблокирующем потоке. Это занимает поток цикла событий, который страница scheduler старается держать свободным.
- Внешний тип остаётся `Mono` или `Flux`, поэтому вызывающие его собирают, а не ждут.

**When to use:**

Оборачивайте вызов, который блокирует. Уже асинхронный publisher не трогайте: `subscribeOn` вокруг неблокирующего источника только добавляет переход между потоками.

**Examples:**

```java
Mono<String> result = Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

<details>
<summary>В Reactor, how is `Context` used?</summary>

### В Reactor, how is `Context` used?

**Definition:**

Reactor `Context` — отображение, привязанное к подписке, а не к потоку. Оно распространяется от конечного подписчика вверх, чтобы операторы читали значения вроде correlation id без `ThreadLocal`.

**How it works:**

- Пишут его через `contextWrite` в момент подписки. `contextWrite` сливает переданное представление с context снизу через `putAll` и даёт новый context вверх.
- Из источника читают через `deferContextual`. В середине цепочки — `transformDeferredContextual`. Внутри `flatMap` представление материализует `Mono.deferContextual`.
- FAQ противопоставляет это MDC, который предполагает один поток. `publishOn` ломает это предположение; context — нет.
- `StepVerifier.expectAccessibleContext` проверяет, что распространилось. У publisher не из Reactor доступного context нет.

**Examples:**

```java
Mono<String> greeting = Mono.just("Hello")
    .flatMap(s -> Mono.deferContextual(ctx ->
        Mono.just(s + " " + ctx.get("name"))))
    .contextWrite(Context.of("name", "World"));
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/advancedFeatures/context.html
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/testing.html
</details>

<details>
<summary>Что такое the `transform` operator and how do you use it in Reactor?</summary>

### Что такое the `transform` operator and how do you use it in Reactor?

**Definition:**

`transform` упаковывает кусок цепочки операторов в функцию и применяет её один раз, во время сборки. Руководство Reactor говорит, что это равносильно записать операторы прямо на цепочке.

**How it works:**

- Каждый подписчик этой последовательности видит одни и те же собранные операторы.
- `transformDeferred` применяет функцию на каждого подписчика, поэтому у каждой подписки может быть другая цепочка.
- Пример руководства печатает, затем делает `transform(filterAndMap)`, затем подписывается.
- Время сборки значит, что функция выполняется, когда цепочку строят, а не один раз на элемент.

**When to use:**

`transform` — чтобы переиспользовать одну цепочку для каждого подписчика. `transformDeferred` — когда цепочка должна зависеть от конкретной подписки.

**Examples:**

```java
Function<Flux<String>, Flux<String>> filterAndMap =
    f -> f.filter(color -> !color.equals("orange"))
          .map(String::toUpperCase);

Flux.just("blue", "orange").transform(filterAndMap);
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/advancedFeatures/advanced-mutualizing-operator-usage.html
</details>

### Reactive Systems and Microservices

<details>
<summary>Определите Reactive Systems and how they relate to reactive programming.</summary>

### Определите Reactive Systems and how they relate to reactive programming.

**Definition:**

Reactive Manifesto называет систему реактивной, когда она отзывчива, устойчива, эластична и управляется сообщениями. Реактивное программирование — техника сборки асинхронных последовательностей; это один способ сделать часть, управляемую сообщениями, а не всё определение.

**How it works:**

- Отзывчивость — система отвечает вовремя. Устойчивость — она остаётся отзывчивой при сбое. Эластичность — она остаётся отзывчивой при меняющейся нагрузке. Управление сообщениями — части общаются асинхронными сообщениями.
- Манифест говорит, что такие системы гибче, слабее связаны и масштабируемее.
- Конвейер `Flux` — это реактивное программирование. Реактивной системой он становится, только когда спроектированы ещё сбой, нагрузка и границы сообщений.
- Reactive Streams — контракт JVM для одной из этих границ: спрос, чтобы быстрый отправитель не мог заставить очередь расти без предела.

**Examples:**

```java
Mono<String> call = Mono.fromCallable(this::remote)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Какую роль does reactive programming play in building microservices?</summary>

### Какую роль does reactive programming play in building microservices?

**Definition:**

Между сервисами реактивное программирование — это как один процесс потребляет поток, не блокируя свои потоки и не принимая безграничный хвост. Reactive Streams — контракт этого обмена.

**How it works:**

- Readme Reactive Streams целится в асинхронную границу, включая другой пул потоков или другой хост, с ограниченными очередями.
- `Mono` — форма одного ответа. `Flux` — форма многих. Оба ленивы до subscribe.
- Блокирующий ввод-вывод изолируют через `subscribeOn(Schedulers.boundedElastic())`, как показывает FAQ Reactor, чтобы вызывающий поток не удерживался.
- Черта манифеста «управление сообщениями» — это граница сервиса. Операторы вроде `map` живут внутри одного процесса.

**Examples:**

```java
Flux<String> pages = Flux.just("/a", "/b")
    .map(path -> path);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Объясните the difference between Reactive Systems and reactive programming.</summary>

### Объясните the difference between Reactive Systems and reactive programming.

**Definition:**

Реактивное программирование собирает последовательности операторами. Реактивная система, по манифесту, — архитектура, которая остаётся отзывчивой за счёт устойчивости, эластичности и сообщений.

**How it works:**

- Можно написать конвейер `Flux.map` и остаться одним блокирующим процессом. Это реактивное программирование без свойств системы.
- Можно передавать асинхронные сообщения между сервисами и вовсе не использовать `Flux`. Это всё ещё может быть реактивная система.
- Reactive Streams — контракт уровня библиотеки внутри стиля программирования. Манифест не задаёт Publisher или Subscriber.
- Полезное пересечение — граница: неблокирующий спрос, чтобы одна часть не могла заставить другую буферизовать без предела.

**When to use:**

Говорите об операторах, `Mono` и `Flux`, когда вопрос про программирование. Говорите о сбое, нагрузке и асинхронных сообщениях, когда вопрос про систему.

**Examples:**

```java
// programming: operators
Flux.just(1, 2, 3).map(n -> n * 2);
// system: bounded asynchronous exchange, per the Reactive Streams goal
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Как you ensure that Reactive Systems are resilient?</summary>

### Как you ensure that Reactive Systems are resilient?

**Definition:**

Устойчивость в манифесте значит, что система остаётся отзывчивой, когда часть отказала. В потоке это значит, что сбой — терминальный сигнал, который заменяют или изолируют, а не застрявший поток.

**How it works:**

- Reactive Streams заканчивает подписку через `onError` или `onComplete`. После `onError` этот подписчик больше ничего не получает.
- Восстановление — оператор библиотеки, который подставляет другой publisher, прежде чем ошибка дойдёт до края. Страница catch в ReactiveX — эта подстановка.
- Ограниченная очередь — часть того, чтобы оставаться на ногах: readme говорит, что безграничный буфер при быстром источнике отменяет асинхронный замысел.
- Блокирующую работу восстановления изолируйте на `boundedElastic`, как FAQ Reactor, чтобы путь сбоя не блокировал цикл событий.

**Examples:**

```java
source.onErrorResume(error -> Mono.just("fallback"));
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://reactivex.io/documentation/operators/catch.html
</details>

<details>
<summary>Опишите how you would implement backpressure across microservices.</summary>

### Опишите how you would implement backpressure across microservices.

**Definition:**

Через границу процесса backpressure — это сигнал спроса, чтобы отправитель не обгонял получателя. Readme Reactive Streams требует, чтобы этот сигнал был асинхронным, а очередь между ними — ограниченной.

**How it works:**

- Подписчик запрашивает `n`. Издатель излучает не больше непогашенного спроса.
- Если протокол — не Reactive Streams, примените адаптеры, которые называет вики RxJava: `onBackpressureBuffer` или `onBackpressureDrop`, либо сначала сожмите поток через `debounce` или `sample`.
- Синхронный вызов, который блокируется, пока удалённая сторона не примет ещё, — тот режим отказа, о котором предупреждает readme: он отменяет асинхронную обработку.
- Один ответ — это `Mono`. Потоковый ответ — `Flux`, чей спрос пересекает ту же границу.

**Examples:**

```java
public void onSubscribe(Subscription s) {
  s.request(8);
}
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>В a microservices architecture, how do Reactive Streams help manage resources?</summary>

### В a microservices architecture, how do Reactive Streams help manage resources?

**Definition:**

Reactive Streams держит очередь между быстрым производителем и медленным потребителем ограниченной. Эта граница — предел ресурса: память под элементы в пути остаётся конечной.

**How it works:**

- Сайт формулирует проблему как живые данные, чей объём заранее неизвестен.
- Спрос — ручка управления. Без неё `zip` двух бесконечных источников из вики RxJava буферизовал бы более быстрый без предела.
- Неблокирующий спрос значит, что приёмник может сказать «пока больше не надо», не занимая поток ради этого.
- JDK Flow — тот же контракт, поэтому сервис может говорить на нём, не завися от `Flux` одной библиотеки.

**Examples:**

```java
// zip requests only enough to refill a small buffer
// after each combined item, as the RxJava backpressure page describes
Observable.zip(left, right, (a, b) -> a + b);
```

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Обсудите the advantages of using reactive programming in microservices communication.</summary>

### Обсудите the advantages of using reactive programming in microservices communication.

**Definition:**

Выигрыш — сборка асинхронных вызовов без блокировки потока на каждый вызов плюс протокол спроса, чтобы один сервис не мог заставить другой держать безграничный буфер.

**How it works:**

- Операторы вроде `map` и `zip` описывают сочетание. Работа начинается в subscribe.
- `zip` ждёт элемент от каждого источника и в реализации RxJava запрашивает лишь столько, сколько нужно, чтобы пополнить маленький буфер.
- `Mono` моделирует один ответ. Сбои остаются в той же последовательности, что и значения, и заканчиваются `onError`.
- Слабая связанность из манифеста появляется, когда сервисы обмениваются сообщениями, а не делят поток.

**Examples:**

```java
Mono.zip(user, orders, (u, o) -> u + ":" + o);
```

**Sources:**

- https://reactivex.io/documentation/operators/zip.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Что такое some challenges you might face when designing Reactive Systems?</summary>

### Что такое some challenges you might face when designing Reactive Systems?

**Definition:**

Трудная часть — граница, которую называет readme Reactive Streams: оставаться асинхронным, держать очереди ограниченными и при этом оставаться отзывчивым, когда часть отказала.

**How it works:**

- Блокирующий вызов внутри `map` занимает поток, который выбрал `publishOn`. Исправление FAQ — изолировать его через `subscribeOn` на `boundedElastic`.
- Безграничный `onBackpressureBuffer` только переносит переполнение в память. Вики отмечает, что буфер с пределом ёмкости завершается ошибкой, когда его переполняют.
- Context — не `ThreadLocal`. После `publishOn` журнальное состояние потока пропадает, если его не положили в context Reactor.
- Спецификация не включает операторы. `flatMap` каждой библиотеки может чередовать внутренние последовательности; предполагать порядок — ошибка проекта.

**Examples:**

```java
Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Как a message-driven system relate to Reactive microservices?</summary>

### Как a message-driven system relate to Reactive microservices?

**Definition:**

Управление сообщениями — одна из четырёх черт манифеста: компоненты общаются асинхронными сообщениями. Реактивный микросервис использует это вместо общего потока или синхронного вызова, который держит вызывающего.

**How it works:**

- Сообщение может быть элементом потока. Reactive Streams тогда добавляет спрос, чтобы почтовый ящик не рос без границы.
- Протокол сигналов — `onSubscribe`, ноль или больше `onNext`, затем `onError` или `onComplete`.
- Эластичность и устойчивость в манифесте зависят от этой развязки: медленный потребитель замедляет производителя спросом, а упавший потребитель заканчивается `onError`, а не застрявшим вызовом.
- Операторы внутри процесса — это не сообщения между процессами.

**Examples:**

```java
subscriber.onSubscribe(subscription);
subscriber.onNext(message);
subscriber.onComplete();
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Что такое some of the most common Reactive libraries and frameworks for microservices?</summary>

### Что такое some of the most common Reactive libraries and frameworks for microservices?

**Definition:**

На JVM обычные части — сам API Reactive Streams, RxJava и Project Reactor. JDK `java.util.concurrent.Flow` — те же четыре роли. RxJS — член ReactiveX для JavaScript.

**How it works:**

- RxJava реализует наблюдаемые последовательности, scheduler и subject. Его история backpressure для источников, которые игнорируют спрос, — `onBackpressureBuffer` и `onBackpressureDrop`.
- Reactor реализует `Mono` и `Flux`, `publishOn` / `subscribeOn` и `Context`.
- TCK — способ библиотеке показать, что она и вправду следует спецификации, а не только именам типов.
- Манифест — не библиотека. Это описание системы, которому эти библиотеки часто служат.

**Examples:**

```java
Flux.just(1).map(n -> n + 1);
Observable.just(1).map(n -> n + 1);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://reactivex.io/documentation/observable.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

### Advanced Concepts in Reactive Programming

<details>
<summary>Объясните the concept of higher-order Observables.</summary>

### Объясните the concept of higher-order Observables.

**Definition:**

Observable высшего порядка излучает другие observable. И `flatMap`, и Switch имеют эту форму: каждый внешний элемент становится внутренней последовательностью, которую нужно развернуть.

**How it works:**

- `flatMap` сливает внутренние, поэтому их элементы могут чередоваться. Страница flatMap в ReactiveX говорит, что ошибка внутреннего сразу роняет внешний.
- Switch, который RxJS называет `switchMap`, отписывается от предыдущего внутреннего, когда приходит новый, и излучает только последний внутренний.
- Тип вложен, только пока нет оператора разворота. Ниже по цепочке видна одна последовательность элементов.
- Если не разворачивать, наружу уходят сами observable как элементы, а подписчику это редко нужно.

**When to use:**

`flatMap` — когда важен каждый внутренний результат. Switch — когда важен только последний внутренний.

**Examples:**

```java
orders.flatMap(id -> Observable.fromIterable(linesOf(id)));
queries.switchMap(text -> search(text));
```

**Sources:**

- https://reactivex.io/documentation/operators/flatmap.html
- https://reactivex.io/documentation/operators/switch.html
</details>

<details>
<summary>Как you implement custom operators in Rx libraries?</summary>

### Как you implement custom operators in Rx libraries?

**Definition:**

Свой оператор — функция из одного Publisher в другой, которая всё ещё соблюдает порядок сигналов Reactive Streams. `transform` в Reactor — поддерживаемый способ переиспользовать такую функцию, не писав новый Subscriber.

**How it works:**

- `transform` применяет функцию один раз при сборке. Все подписчики делят эту цепочку.
- `transformDeferred` применяет её на каждого подписчика.
- Протокол из readme JVM внутри функции тот же: `onSubscribe`, затем `onNext` ноль или больше раз, затем `onError` или `onComplete`.
- Не излучайте `onNext` после терминального сигнала. Это ломает протокол, который проверяет TCK.

**Examples:**

```java
Function<Flux<String>, Flux<String>> upper =
    flux -> flux.map(String::toUpperCase);

names.transform(upper);
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/advancedFeatures/advanced-mutualizing-operator-usage.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Что такое the implications of unicast vs multicast Observables?</summary>

### Что такое the implications of unicast vs multicast Observables?

**Definition:**

Одноадресный холодный observable выполняет свою последовательность отдельно для каждого подписчика. Многоадресный горячий, обычно Subject, делит одну последовательность между подписчиками.

**How it works:**

- Страница backpressure RxJava: холодный observable может начаться, когда наблюдатель хочет, и с его скоростью, не меняя последовательность. Пример — итерируемое, преобразованное через `from`.
- Subject, подписанный на холодный источник, запускает его и затем переизлучает многим наблюдателям — это горячее представление со страницы subject.
- `PublishSubject` даёт опоздавшему только будущие элементы. `BehaviorSubject` даёт ещё и последний. `ReplaySubject` даёт прежние.
- Многоадресность плюс источник, который игнорирует спрос, — путь к безграничному буферу. Спрос или `onBackpressureBuffer` / `onBackpressureDrop` должны стоять на этой общей последовательности.

**When to use:**

Оставайтесь холодными, когда каждому подписчику нужен свой запуск, например свой HTTP-вызов. Переходите к горячему, когда многие подписчики должны видеть одни и те же живые события.

**Examples:**

```java
Observable<Integer> cold = Observable.fromIterable(List.of(1, 2));
PublishSubject<Integer> hot = PublishSubject.create();
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://reactivex.io/documentation/subject.html
- https://reactivex.io/documentation/operators/from.html
</details>

<details>
<summary>Как you use `combineLatest`, `withLatestFrom`, and `zip` operators for combining multiple streams?</summary>

### Как you use `combineLatest`, `withLatestFrom`, and `zip` operators for combining multiple streams?

**Definition:**

`zip` спаривает элементы по индексу и излучает, только когда у каждого источника есть новый неспаренный элемент. `combineLatest` излучает, когда излучает любой источник, беря последний элемент каждого. `withLatestFrom` излучает, только когда излучает основной источник, присоединяя последнее значение другого.

**How it works:**

- Страница combineLatest говорит, что он похож на zip, кроме того что zip ждёт ранее не использованный элемент от каждого источника.
- На той же странице ReactiveX разбирает `withLatestFrom`: выход ведёт один источник, а не любой из них.
- `zip` в RxJava держит маленький буфер и после каждого сочетанного излучения запрашивает ещё один элемент с каждого источника — это пример вики о backpressure.
- Если один источник гораздо быстрее, `zip` удерживает его лишние элементы. Это тот хвост, о котором предупреждает вики.

**When to use:**

`zip` — чтобы спарить соответствующие элементы. `combineLatest` — когда любое обновление должно пересчитать результат из последних значений. `withLatestFrom` — когда триггер только один поток.

**Examples:**

```java
Observable.zip(ids, names, (id, name) -> id + ":" + name);
Observable.combineLatest(a, b, (x, y) -> x + y);
```

**Sources:**

- https://reactivex.io/documentation/operators/zip.html
- https://reactivex.io/documentation/operators/combinelatest.html
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Обсудите the concept of side-effects in Reactive Streams and how to avoid them.</summary>

### Обсудите the concept of side-effects in Reactive Streams and how to avoid them.

**Definition:**

Побочный эффект — работа, которая не является следующим элементом, например запись в общую переменную. Контракт последовательности проще держать, когда каждая стадия — чистая функция своего элемента, а ввод-вывод делает только подписчик.

**How it works:**

- `map` должен вернуть следующее значение. Изменение списка, захваченного снаружи, делает результат зависимым от того, кто подписался и какой поток выбрал `publishOn`.
- FAQ Reactor говорит, что `publishOn` переносит поздние стадии на другой поток, поэтому `ThreadLocal`, записанный выше, ниже не виден. Это значение кладут в `Context`.
- Обработчик ошибки, который глотает `onError`, ломает протокол: подписчик не видит терминальный сигнал.
- Журнал, который пробрасывает ошибку или только записывает и даёт ошибке идти дальше, — честный побочный эффект.

**Examples:**

```java
flux.map(String::toUpperCase)
    .contextWrite(Context.of("requestId", id));
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/advancedFeatures/context.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Объясните the difference between `takeUntil` and `takeWhile` operators.</summary>

### Объясните the difference between `takeUntil` and `takeWhile` operators.

**Definition:**

`takeWhile` излучает элементы, пока предикат на элементе истинен, и останавливается, когда он ложен. `takeUntil` излучает элементы источника, пока второй observable не излучит, и тогда останавливается.

**How it works:**

- Страница takeWhile фильтрует по самому элементу. Другой поток для решения не нужен.
- Страница takeUntil следит за уведомителем. Остановку вызывает не элемент источника, а другой observable.
- Оба завершают выход, когда останавливаются. Обычная остановка — не ошибка.
- Ни один не запрашивает безграничный остаток после остановки, если источник соблюдает спрос.

**When to use:**

`takeWhile` — когда остановку говорят сами элементы. `takeUntil` — когда остановку говорит другой поток, например щелчок или сигнал остановки.

**Examples:**

```java
source.takeWhile(n -> n < 10);
source.takeUntil(cancel);
```

**Sources:**

- https://reactivex.io/documentation/operators/takewhile.html
- https://reactivex.io/documentation/operators/takeuntil.html
</details>

<details>
<summary>Как concurrency control work in reactive programming?</summary>

### Как concurrency control work in reactive programming?

**Definition:**

Конкурентность явная. `subscribeOn` выбирает поток подписки, `observeOn` или `publishOn` в Reactor выбирает, где идут поздние уведомления, а спрос Reactive Streams ограничивает, сколько элементов может быть не погашено.

**How it works:**

- ReactiveX: по умолчанию цепочка остаётся на потоке подписки. `subscribeOn` переносит источник. `observeOn` переносит уведомления ниже этого вызова.
- Reactor: ближайший `subscribeOn` ниже по цепочке планирует подписку. `publishOn` влияет на операторы после себя до следующего `publishOn`.
- Subject всё ещё нужно сериализовать, если `onNext` приходит из нескольких потоков. `SerializedSubject` из вики RxJava — эта очередь.
- Ограниченный спрос — вторая половина. Scheduler без `request` всё ещё может растить очередь.

**Examples:**

```java
Flux.just(1, 2, 3)
    .publishOn(Schedulers.parallel())
    .map(n -> n * 2);
```

**Sources:**

- https://reactivex.io/documentation/scheduler.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://github.com/ReactiveX/RxJava/wiki/Subject
</details>

<details>
<summary>Что такое the role of virtual time in testing Reactive applications?</summary>

### Что такое the role of virtual time in testing Reactive applications?

**Definition:**

Виртуальное время двигает часы тестового scheduler, не дожидаясь настенных часов. `TestScheduler` RxJava — инструмент, который заметки 2.0 показывают для оператора с задержкой, например `delay` на одну секунду.

**How it works:**

- Создают `TestScheduler`, передают его оператору, зависящему от времени, и подписываются тестовым наблюдателем.
- `advanceTimeBy` двигает виртуальные часы. Пример 2.0 проверяет пустоту на 999 мс и значение на 1000 мс.
- `StepVerifier` Reactor сравнивает сигналы после подписки. Тесты Reactor, зависящие от времени, используют виртуальное расписание верификатора, а не `Thread.sleep`.
- Настоящий scheduler в модульном тесте делает тест зависимым от нагрузки. Виртуальные часы — нет.

**Examples:**

```java
TestScheduler scheduler = new TestScheduler();
TestObserver<Integer> ts = source.delay(1, TimeUnit.SECONDS, scheduler).test();
scheduler.advanceTimeBy(1, TimeUnit.SECONDS);
ts.assertValue(1);
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0
- https://projectreactor.io/docs/core/release/reference/testing.html
</details>

<details>
<summary>Как you manage state in reactive programming?</summary>

### Как you manage state in reactive programming?

**Definition:**

Состояние несут в последовательности. `scan` излучает каждый следующий аккумулятор. `BehaviorSubject` держит последнее значение для опоздавших подписчиков. Reactor `Context` держит данные области подписки, которые не являются элементом.

**How it works:**

- `scan` сворачивает элементы с начальным значением и излучает каждый промежуточный результат. Сумма тиков — форма со страницы scan.
- `BehaviorSubject` при подписке повторяет текущий элемент, затем поздние. Это общее состояние, поэтому сериализуйте его, если `onNext` зовут несколько потоков.
- `contextWrite` хранит correlation id подписки. Это не замена `scan` для деловых данных.
- Изменяемое поле, захваченное `map`, гоняется, как только `publishOn` переносит цепочку.

**When to use:**

`scan` — для состояния, выведенного из элементов. `BehaviorSubject` — для общего текущего значения. `Context` — для сопутствующих данных подписки, например идентификатора запроса.

**Examples:**

```java
flux.scan(0, Integer::sum);
BehaviorSubject<String> current = BehaviorSubject.createDefault("none");
```

**Sources:**

- https://reactivex.io/documentation/operators/scan.html
- https://reactivex.io/documentation/subject.html
- https://projectreactor.io/docs/core/release/reference/advancedFeatures/context.html
</details>

<details>
<summary>Опишите strategies to handle out-of-order messages in a Reactive system.</summary>

### Опишите strategies to handle out-of-order messages in a Reactive system.

**Definition:**

Потоки сохраняют порядок вызовов `onNext` одного publisher. Порядок между слитыми publisher не гарантирован. Выберите оператор сочетания, который соответствует порядку, который вам действительно нужен.

**How it works:**

- `zip` излучает шаг в шаг: n-й выход использует n-й элемент каждого источника. Более быстрый источник ждёт.
- `flatMap` сливает внутренние по мере прихода, поэтому медленный первый внутренний могут обогнать.
- Switch держит только последний внутренний и отбрасывает остаток предыдущего — это выбор порядка, когда старая работа уже не нужна.
- Номер последовательности внутри элемента — данные приложения. `scan` может помнить последний принятый номер, а `filter` — отбросить откат. Спецификация за вас не переупорядочивает.

**Examples:**

```java
Observable.zip(clock, values, (t, v) -> t + ":" + v);
latest.switchMap(id -> load(id));
```

**Sources:**

- https://reactivex.io/documentation/operators/zip.html
- https://reactivex.io/documentation/operators/flatmap.html
- https://reactivex.io/documentation/operators/switch.html
</details>

### Tools and Frameworks in Reactive Programming

<details>
<summary>Перечислите several popular reactive programming frameworks and their Primary use cases.</summary>

### Перечислите several popular reactive programming frameworks and their Primary use cases.

**Definition:**

RxJava и RxJS реализуют наблюдаемые последовательности ReactiveX. Project Reactor реализует `Mono` и `Flux` на контракте Reactive Streams. JDK Flow — тот же контракт в платформе.

**How it works:**

- RxJava — библиотека observable для JVM: scheduler, subject и операторы вроде `debounce` и `zip`.
- RxJS — член того же набора операторов для JavaScript, включая `from` и `scan`.
- Reactor — библиотека, чьи `publishOn`, `subscribeOn`, `Context` и `StepVerifier` описывает справочник.
- TCK Reactive Streams — то, что должна пройти следующая библиотека, если хочет взаимодействовать, а не только заимствовать имена.

**Examples:**

```java
Observable.just(1).map(n -> n + 1);
Mono.just(1).map(n -> n + 1);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Как Akka Streams fit into the reactive programming paradigm?</summary>

### Как Akka Streams fit into the reactive programming paradigm?

**Definition:**

Akka Streams — реализация потоков, которая может стоять на контракте Reactive Streams: стадии обмениваются элементами асинхронно, а приёмник ограничивает спрос, чтобы буферы оставались конечными. Акторы Akka, напротив, — долгоживущие обработчики сообщений, которые называет глава Scala о конкурентности.

**How it works:**

- Четыре типа из readme Reactive Streams — край взаимодействия. Стадия-Processor одновременно подписчик и издатель.
- Спрос через `request` не даёт быстрой стадии навязать медленной безграничную очередь.
- Операторы, которые преобразуют элементы, — возможности библиотеки. Спецификация говорит, что преобразование она не описывает.
- Почтовый ящик актора без спроса может расти. Граница Reactive Streams — та часть, которая добавляет предел.

**Examples:**

```java
// A Reactive Streams subscriber bounds an upstream stage.
subscription.request(16);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://doc.akka.io/libraries/akka-core/current/typed/index.html
</details>

<details>
<summary>Обсудите the integration of reactive programming libraries with traditional web frameworks.</summary>

### Обсудите the integration of reactive programming libraries with traditional web frameworks.

**Definition:**

Традиционный фреймворк ждёт, что поток заблокируется, пока результат не будет готов. Реактивная библиотека ждёт `Mono` или `Flux` и subscribe. Интеграция — адаптер на краю, а не переписывание каждого оператора.

**How it works:**

- Возвращайте `Mono` для одного тела ответа и `Flux` для потока. Вызывающий подписывается; до этого цепочка не идёт.
- Если существующий код блокирует, оберните его, как показывает FAQ Reactor: `Mono.fromCallable(...).subscribeOn(Schedulers.boundedElastic())`.
- Не вызывайте `block` на потоке цикла событий. Это возвращает цену «поток на запрос», ради избежания которой и разделено планирование.
- Ошибки остаются в `onError`. Адаптер на краю отображает этот терминальный сигнал в ошибочный ответ фреймворка.

**Examples:**

```java
Mono<String> body = Mono.fromCallable(this::legacyBlocking)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

<details>
<summary>Что такое some reactive programming utilities provided by the Spring WebFlux framework?</summary>

### Что такое some reactive programming utilities provided by the Spring WebFlux framework?

**Definition:**

Spring WebFlux говорит типами Reactor на краю HTTP: обработчик возвращает `Mono` или `Flux`, а фреймворк подписывается. Реактивные утилиты поэтому принадлежат Reactor, а не второму набору операторов.

**How it works:**

- Одно тело JSON — это `Mono`. Потоковое тело — `Flux`. Оба соблюдают спрос Reactive Streams.
- `publishOn` и `subscribeOn` — как обработчик уходит с цикла событий на CPU или блокирующую работу. Контракт — страница scheduler Reactor.
- Данные запроса, которые должны пережить смену потока, кладут в Reactor `Context`, а не в `ThreadLocal`, потому что `publishOn` меняет потоки.
- Тесты используют `StepVerifier` на возвращённом publisher, а не поток сервлета.

**Examples:**

```java
Mono<String> hello = Mono.just("ok");
Flux<String> ticks = Flux.just("a", "b");
```

**Sources:**

- https://docs.spring.io/spring-framework/reference/web/webflux.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://projectreactor.io/docs/core/release/reference/testing.html
</details>

<details>
<summary>Как you debug a complex Reactive Streams pipeline?</summary>

### Как you debug a complex Reactive Streams pipeline?

**Definition:**

Отлаживают последовательность сигналов, а не стек, который пересекает потоки. `StepVerifier` задаёт ожидаемые значения `onNext` и терминальный сигнал. Несовпадение бросает `AssertionError` на первом неверном сигнале.

**How it works:**

- Ужимайте цепочку, пока верификатор не проходит, затем верните один оператор. `transform` делает этот кусок именованной функцией.
- Если сбой в том, что «ничего не происходит», никто не подписался. Страница scheduler Reactor: ничего не происходит до subscribe.
- Если цикл событий заблокирован, блокирующий вызов сидит в `map`. Перенесите его за `subscribeOn(Schedulers.boundedElastic())`.
- Если журналы теряют идентификатор запроса после `publishOn`, читайте его из `Context`, который следует за подпиской, а не за потоком.

**Examples:**

```java
StepVerifier.create(flux.map(String::toUpperCase))
    .expectNext("A")
    .verifyComplete();
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/testing.html
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

<details>
<summary>Что такое the best tools for monitoring and troubleshooting Reactive applications?</summary>

### Что такое the best tools for monitoring and troubleshooting Reactive applications?

**Definition:**

Разбираются инструментами сигналов, которые документируют библиотеки: `StepVerifier` Reactor для ожидаемых элементов и context и `TestScheduler` RxJava для времени. Боевой мониторинг по-прежнему смотрит на ограниченные очереди, которые модель Reactive Streams и защищает.

**How it works:**

- `StepVerifier` падает на первом неожиданном сигнале, включая неожиданный `onError`.
- `expectAccessibleContext` проверяет, что correlation id пережил цепочку.
- `TestScheduler.advanceTimeBy` показывает, сработал ли `delay` или `debounce`, без сна.
- Очередь, которая растёт без соответствующего `request`, — то переполнение, которого readme Reactive Streams требует не допускать. Сигнальте о её глубине, а не только о числе потоков.

**Examples:**

```java
StepVerifier.create(mono)
    .expectNext("ok")
    .verifyComplete();
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/testing.html
- https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Объясните how reactive programming can be used with Kafka Streams.</summary>

### Объясните how reactive programming can be used with Kafka Streams.

**Definition:**

Kafka Streams обрабатывает журнал записей своим API топологии. Reactive Streams решает другую, соседнюю задачу: пересечь асинхронную границу с ограниченным спросом. Kafka Streams — для топологии журнала, а `Flux` — только на краю, который должен говорить на Publisher и Subscriber.

**How it works:**

- Не переименовывайте операцию Kafka Streams в `flatMap`. Оператор ReactiveX — не метод Kafka Streams.
- Потребитель, который является подписчиком Reactive Streams, запрашивает `n` и останавливается, когда спрос исчерпан, — так readme держит очередь ограниченной.
- Порядок внутри одного раздела — порядок журнала. `zip` спаривает два реактивных источника, а не переупорядочивает раздел.
- Backpressure через сеть всё равно должен быть асинхронным. Потребитель, который блокирует поток poll, чтобы применить спрос, отменяет это, о чём предупреждает readme.

**Examples:**

```java
// Reactive Streams demand at the edge of a consumer.
// Kafka Streams topologies stay in the Kafka Streams API.
subscription.request(32);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://kafka.apache.org/documentation/streams/
</details>

<details>
<summary>Как you handle load balancing in a Reactive microservices setup?</summary>

### Как you handle load balancing in a Reactive microservices setup?

**Definition:**

Эластичность в манифесте значит оставаться отзывчивым при изменении нагрузки. В потоке нагрузку уравновешивает спрос: медленный экземпляр запрашивает меньше, поэтому отправитель не продолжает наполнять к нему безграничную очередь.

**How it works:**

- `request(n)` Reactive Streams — тормоз на соединение. Это не замена маршрутизатору, и маршрутизатор не замена тормозу.
- `onBackpressureDrop` сбрасывает нагрузку, отбрасывая. `onBackpressureBuffer` держит ограниченный хвост и падает с ошибкой при достижении предела — так описывает вики RxJava.
- `debounce` и `sample` сжимают горячий поток, прежде чем его пересылать.
- Больше экземпляров помогает, только если каждый всё ещё ограничивает свою очередь. Лишние потоки без спроса лишь переносят переполнение.

**Examples:**

```java
source.onBackpressureDrop();
source.sample(500, TimeUnit.MILLISECONDS);
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Обсудите how reactive programming influences event-driven architecture.</summary>

### Обсудите how reactive programming influences event-driven architecture.

**Definition:**

Событийная архитектура уже передаёт асинхронные сообщения. Реактивное программирование добавляет подписчика, который может собирать эти события операторами и, на JVM, ограничивать спрос.

**How it works:**

- Черта манифеста «управление сообщениями» — это архитектура. `map`, `scan` и сужение в духе `filter` — это программирование.
- `scan` превращает поток событий в бегущее состояние. Это состояние излучается, а не лежит в изменяемом поле, захваченном цепочкой.
- Горячий Subject — шина внутри процесса. Опоздавшие видят только то, что обещает этот вариант Subject: будущие элементы, последний или повтор.
- Между процессами предел Reactive Streams всё ещё действует. Журнал событий сам по себе не реализует `request`.

**Examples:**

```java
events.scan(0, (count, event) -> count + 1);
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://reactivex.io/documentation/operators/scan.html
- https://reactivex.io/documentation/subject.html
</details>

<details>
<summary>Что такое RSocket, and how does it support reactive programming patterns?</summary>

### Что такое RSocket, and how does it support reactive programming patterns?

**Definition:**

RSocket — протокол реактивных потоков через сеть. Он переносит идею Reactive Streams об асинхронном спросе, чтобы одна сторона не могла заставить другую буферизовать без границы.

**How it works:**

- Readme Reactive Streams уже требует, чтобы сигналы backpressure были асинхронными между хостами, а не только между пулами потоков.
- Взаимодействие запрос-ответ — один элемент, форма `Mono`. Запрос-поток — много элементов, форма `Flux`.
- Операторы приложения по-прежнему живут в Reactor или RxJava. Протокол не заменяет `map` или `zip`.
- Блокирующий адаптер на любом конце снимает пользу, которую описывает readme.

**Examples:**

```java
// One response is a Mono; a stream of responses is a Flux.
// Demand still travels as Reactive Streams request(n).
Mono<String> one = Mono.just("ok");
```

**Sources:**

- https://rsocket.io/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

### Design Patterns and Best Practices

<details>
<summary>Что такое some common design patterns used in reactive programming?</summary>

### Что такое some common design patterns used in reactive programming?

**Definition:**

Шаблоны, которые видны в документации, — холодный источник на подписчика, Subject как горячий мост, операторы как конвейер и `request` как предел очереди.

**How it works:**

- `from` по коллекции — холодный шаблон: каждая подписка обходит данные снова.
- Subject — горячий шаблон. `BehaviorSubject` хранит последний элемент; `PublishSubject` — нет.
- `map` и `flatMap` — конвейер. `transform` именует переиспользуемый кусок.
- `request` Reactive Streams — шаблон, который держит асинхронную границу ограниченной.

**Examples:**

```java
Observable.fromIterable(List.of(1, 2, 3)).map(n -> n * 2);
```

**Sources:**

- https://reactivex.io/documentation/operators/from.html
- https://reactivex.io/documentation/subject.html
- https://projectreactor.io/docs/core/release/reference/advancedFeatures/advanced-mutualizing-operator-usage.html
</details>

<details>
<summary>Как you apply commonly known GAng of Four patterns in reactive programming?</summary>

### Как you apply commonly known GAng of Four patterns in reactive programming?

**Definition:**

Паттерн «наблюдатель» — это подписчик. Конвейер операторов — цепочка обёрток вокруг источника. Subject — заместитель, который одновременно наблюдатель и observable.

**How it works:**

- ReactiveX описывает Subject как мост или заместитель: он может подписаться и может переизлучать.
- `map` оборачивает источник и возвращает новый observable. Источник не меняется.
- Стратегия — функция, которую передают: та же форма `scan` с другим аккумулятором.
- Наблюдатель из «банды четырёх» не включает спрос. Reactive Streams добавляет `request`, чтобы наблюдатель мог замедлить источник.

**Examples:**

```java
PublishSubject<String> bus = PublishSubject.create();
bus.map(String::trim).subscribe(System.out::println);
```

**Sources:**

- https://reactivex.io/documentation/subject.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://reactivex.io/documentation/operators/scan.html
</details>

<details>
<summary>Что такое some best practices for error handling in Reactive Streams?</summary>

### Что такое some best practices for error handling in Reactive Streams?

**Definition:**

Считайте `onError` терминальным. Восстанавливайтесь, подставляя другой publisher, или дайте подписчику увидеть ошибку. Не излучайте `onNext` после ошибки.

**How it works:**

- Протокол readme JVM — `onSubscribe`, затем повторяющийся `onNext`, затем ровно один из `onError` или `onComplete`.
- Оператор catch в ReactiveX заменяет упавшую последовательность другой. Это восстановление.
- Пустой callback ошибки прячет терминальный сигнал. Подписчик тогда ждёт вечно.
- Повтор подписывается на холодный источник заново. Горячий Subject он не отматывает.

**Examples:**

```java
source.onErrorResume(error -> Mono.just("fallback"));
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://reactivex.io/documentation/operators/catch.html
</details>

<details>
<summary>Опишите a scenario where reactive programming is not the ideal approach. Why?</summary>

### Опишите a scenario where reactive programming is not the ideal approach. Why?

**Definition:**

Короткое конечное преобразование уже имеющихся данных, занятое процессором, не нуждается в подписчике, scheduler или спросе. Прямой вызов возвращает значение в том же потоке.

**How it works:**

- Постановка задачи Reactive Streams — живые данные неизвестного объёма через асинхронную границу. `List` в памяти — не эта задача.
- Обернуть этот список в `Flux.just` и заблокироваться на нём добавляет протокол без очереди, которую нужно ограничить.
- Один блокирующий вызов JDBC, спрятанный в `map` на цикле событий, хуже обычного вызова. FAQ помогает, только если вы и вправду перенесёте его на `boundedElastic`.
- Операторы окупаются, когда значений много, есть отмена или переход между потоками. Иначе они затемняют прямую функцию.

**When to use:**

Оставайтесь в обычном коде для одного конечного прохода по памяти. Publisher — когда данные приходят со временем, работа асинхронна или потребитель должен ограничить скорость.

**Examples:**

```java
List<Integer> doubled = List.of(1, 2, 3).stream().map(n -> n * 2).toList();
```

**Sources:**

- https://www.reactive-streams.org/
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

<details>
<summary>Объясните the use of backpressure strategies, like buffering and dropping.</summary>

### Объясните the use of backpressure strategies, like buffering and dropping.

**Definition:**

Когда источник игнорирует `request`, RxJava предлагает `onBackpressureBuffer` и `onBackpressureDrop`. Буферизация хранит элементы и излучает их по запросу подписчика. Отбрасывание выкидывает элементы, к которым подписчик не готов.

**How it works:**

- Вики о backpressure вводит оба оператора как способ навязать простую политику источнику, который не писали под реактивный pull.
- Экспериментальный ограниченный `onBackpressureBuffer` завершается ошибкой, если буфер переполнен. Безграничный буфер может исчерпать память; пример `zip` в вики — этот сбой.
- `debounce`, `sample` и `throttleFirst` сжимают поток, прежде чем понадобится буфер.
- `buffer` группирует элементы в списки. Это другой оператор: он меняет тип элемента и не реализует `request`.

**When to use:**

Буферизуйте, когда каждый элемент в итоге нужно доставить и всплеск ограничен. Отбрасывайте, когда устаревший элемент бесполезен, например замер датчика, который заменит более новый.

**Examples:**

```java
source.onBackpressureBuffer();
source.onBackpressureDrop();
bursty.debounce(10, TimeUnit.MILLISECONDS);
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://reactivex.io/documentation/operators/debounce.html
</details>

<details>
<summary>Как you make your Reactive Streams more composable and reusable?</summary>

### Как you make your Reactive Streams more composable and reusable?

**Definition:**

Возвращайте из метода `Flux` или `Mono` и переиспользуйте кусок через `transform`. Руководство Reactor говорит, что `transform` применяет одну функцию во время сборки, что равносильно записать операторы прямо.

**How it works:**

- Метод, который подписывается внутри себя, нельзя собрать с другими. Подписываться должен вызывающий.
- `transform` делит одну собранную цепочку. `transformDeferred` пересобирает её на подписчика, когда цепочка зависит от этой подписки.
- Держите функцию чистой: `map` возвращает значение. Побочные эффекты — у подписчика или в явном шаге журнала, который не глотает ошибки.
- Называйте функцию по политике, например `withoutOrange`, а не по месту вызова.

**Examples:**

```java
Function<Flux<String>, Flux<String>> withoutOrange =
    flux -> flux.filter(color -> !color.equals("orange"));

colors.transform(withoutOrange);
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/advancedFeatures/advanced-mutualizing-operator-usage.html
</details>

<details>
<summary>Что такое some best practices for scaling Reactive applications?</summary>

### Что такое some best practices for scaling Reactive applications?

**Definition:**

Масштабируют, ограничивая работу, а не добавляя безграничные буферы. Эластичность манифеста — оставаться отзывчивым под нагрузкой; Reactive Streams делает это спросом, а Reactor — тем, что блокирующая работа не сидит на цикле событий.

**How it works:**

- `request(n)` ограничивает элементы в пути на подписку.
- Блокирующие вызовы идут на `Schedulers.boundedElastic()` сразу после источника, как показывает FAQ.
- Работу CPU можно перенести через `publishOn`. Позиция важна: он влияет на операторы после себя.
- Горячий поток, за которым подписчики не успевают, отбрасывают или прореживают, а не буферизуют без предела.

**Examples:**

```java
Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic())
    .map(String::trim);
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Как you manage configuration changes in a Reactive system?</summary>

### Как you manage configuration changes in a Reactive system?

**Definition:**

Изменение конфигурации — элемент потока. `scan` сворачивает его в текущие настройки, либо `BehaviorSubject` держит последние настройки для того, кто подпишется следующим.

**How it works:**

- `BehaviorSubject` при подписке излучает текущее значение — это контракт на странице Subject в ReactiveX.
- Работа ниже, которую нужно перезапустить при изменении, использует Switch: новая конфигурация отписывает предыдущий внутренний поток.
- Не прячьте значение в изменяемое поле, которое читает `map` после `publishOn`. Поток будет не тем, который писал.
- Значения области подписки, которые не являются потоком конфигурации, принадлежат Reactor `Context`.

**Examples:**

```java
BehaviorSubject<Integer> limit = BehaviorSubject.createDefault(10);
configs.switchMap(cfg -> source.takeWhile(n -> n < cfg.limit()));
```

**Sources:**

- https://reactivex.io/documentation/subject.html
- https://reactivex.io/documentation/operators/switch.html
- https://projectreactor.io/docs/core/release/reference/advancedFeatures/context.html
</details>

<details>
<summary>Обсудите the CQRS pattern in the context of Reactive Systems.</summary>

### Обсудите the CQRS pattern in the context of Reactive Systems.

**Definition:**

CQRS отделяет команды, которые меняют состояние, от запросов, которые его читают. В реактивной системе обе стороны управляются сообщениями: команда — событие, которое сворачивают через `scan`, а запрос читает последнее значение из `BehaviorSubject` или из хранилища, которое кормит эта свёртка.

**How it works:**

- Черта манифеста «управление сообщениями» — причина, почему путь записи не делит поток с путём чтения.
- `scan` излучает каждое следующее состояние. Подписчики стороны запроса наблюдают эти излучения вместо того, чтобы запирать модель.
- Сбой потока команд — это `onError`, и для этой подписки он терминален. Внутри `map` он не применяется наполовину.
- Спрос по-прежнему ограничивает, как быстро можно применять команды. CQRS не отменяет backpressure.

**Examples:**

```java
commands.scan(State.empty(), State::apply);
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://reactivex.io/documentation/operators/scan.html
- https://reactivex.io/documentation/subject.html
</details>

<details>
<summary>Что такое the principles of the Reactive Manifesto?</summary>

### Что такое the principles of the Reactive Manifesto?

**Definition:**

Манифест называет четыре свойства: отзывчивость, устойчивость, эластичность и управление сообщениями. Системы с этими свойствами, по его словам, более гибки, слабее связаны и масштабируемы.

**How it works:**

- Отзывчивость: система отвечает своевременно.
- Устойчивость: она остаётся отзывчивой перед лицом сбоя.
- Эластичность: она остаётся отзывчивой при меняющейся нагрузке.
- Управление сообщениями: части опираются на асинхронную передачу сообщений. Readme Reactive Streams ссылается на манифест, объясняя, почему сам сигнал backpressure должен быть асинхронным.

**Examples:**

```text
// Four properties from the manifesto:
// responsive, resilient, elastic, message-driven
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

### Reactive Programming in Different Languages

<details>
<summary>Как reactive programming implemented in languages like JavaScript, Scala, or Java?</summary>

### Как reactive programming implemented in languages like JavaScript, Scala, or Java?

**Definition:**

Идея одна: наблюдаемая последовательность. JavaScript использует RxJS, Java — RxJava или Reactor, а страницы ReactiveX документируют одни и те же операторы для каждого. На JVM общий протокол спроса — Reactive Streams плюс JDK Flow.

**How it works:**

- RxJS `from` и RxJava `fromIterable` — оператор From: излучить каждый элемент коллекции и завершиться.
- `debounce`, `zip`, `scan` и варианты Subject сохраняют свои контракты в этих библиотеках.
- Scala может вызывать те же библиотеки JVM. `Future` в Scala — один результат, а не publisher Reactive Streams.
- JDK 9 `java.util.concurrent.Flow` совпадает с четырьмя типами Reactive Streams, поэтому код на Java может говорить на протоколе, не импортируя сторонний publisher.

**Examples:**

```java
Observable<Integer> javaSide = Observable.fromIterable(List.of(1, 2, 3));
// RxJS: from([1, 2, 3])
```

**Sources:**

- https://reactivex.io/documentation/operators/from.html
- https://www.reactive-streams.org/
- https://reactivex.io/documentation/observable.html
</details>

<details>
<summary>Сравните the Reactor pattern in Java with Reactive Extensions in .NET.</summary>

### Сравните the Reactor pattern in Java with Reactive Extensions in .NET.

**Definition:**

Классический паттерн Reactor демультиплексирует события готовности на небольшой набор потоков. Reactive Extensions и Project Reactor на JVM собирают значения, которые несут эти события, в последовательности с операторами и, на JVM, со спросом.

**How it works:**

- Цикл селектора — паттерн ввода-вывода. Он не определяет `map` или `zip`.
- ReactiveX документирует эти операторы один раз и реализует их и в Rx.NET, и в RxJava. Имена на страницах операторов — и есть сравнение.
- `publishOn` в Project Reactor переносит последовательность на scheduler после стадии. Это сборка, а не сам цикл селектора.
- Предел всё равно нужен. `request` Reactive Streams — форма JVM для «не доставляй больше, чем потребитель принял».

**Examples:**

```java
Flux.just(1, 2).publishOn(Schedulers.parallel()).map(n -> n + 1);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Как you use reactive programming in modern front-end frameworks like React or Angular?</summary>

### Как you use reactive programming in modern front-end frameworks like React or Angular?

**Definition:**

В браузере последовательность обычно RxJS. Angular потребляет observable напрямую. React хранит своё состояние: подписываются на краю и кладут последний элемент, часто то значение, которое `BehaviorSubject` уже держит.

**How it works:**

- `from` превращает массив в observable. `fromEvent` не обязателен, если callback и так зовёт `next` у Subject.
- `scan` сворачивает события в состояние, которое рисуют. `debounce` ждёт паузу — так его определяет страница debounce.
- Switch отбрасывает предыдущий внутренний запрос, когда пользователь печатает снова. Это оператор Switch в ReactiveX, в RxJS записанный как `switchMap`.
- Отписывайтесь, когда представление исчезает. Живая подписка — это утечка. Promise эту отмену не выражает.

**Examples:**

```javascript
const query = new BehaviorSubject("");
query.pipe(switchMap(text => from(search(text))));
```

**Sources:**

- https://reactivex.io/documentation/subject.html
- https://reactivex.io/documentation/operators/switch.html
- https://reactivex.io/documentation/operators/debounce.html
</details>

<details>
<summary>Что такое some considerations for using reactive programming in a functional programming language?</summary>

### Что такое some considerations for using reactive programming in a functional programming language?

**Definition:**

Операторы — функции из последовательности в последовательность. Они остаются ссылочно честными, когда `map` не пишет общее состояние, а единственные эффекты сидят в subscribe.

**How it works:**

- `map` и `scan` принимают функцию и возвращают новую последовательность. Источник не мутирует.
- `flatMap` — это разворот после функции, которая сама возвращает последовательность. Switch — вариант, который держит только последний внутренний поток.
- Ленивый холодный источник подходит функциональному стилю: описание `from(array)` его не запускает. Subscribe — край.
- `publishOn` меняет поток, поэтому захваченная изменяемая переменная — гонка. Передавайте значение в элементе или в Reactor `Context`.

**Examples:**

```java
flux.map(n -> n * 2).scan(0, Integer::sum);
```

**Sources:**

- https://reactivex.io/documentation/operators/scan.html
- https://reactivex.io/documentation/operators/flatmap.html
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

<details>
<summary>Как the async-await pattern in modern languages relate to reactive programming?</summary>

### Как the async-await pattern in modern languages relate to reactive programming?

**Definition:**

`async`/`await` ждёт одно будущее значение и затем продолжает. Publisher может излучить много значений, стартует в subscribe и может быть ограничен спросом. Await — случай одного элемента, ближе к `Mono`, чем к `Flux`.

**How it works:**

- Promise или `Future` в Scala завершается один раз. `Flux` — последовательность многих завершений.
- Await внутри цикла тянет. Подписчику толкают, и на JVM он тянет только через `request`.
- Один элемент можно адаптировать: первый элемент `Mono` — то, чего ждёт await. Многозначный `Flux` в один await не помещается.
- Отмена await и отписка от потока — разное. Отписка прекращает спрос.

**When to use:**

Async/await — для одного результата. Publisher — когда есть последовательность, отменяемая подписка или нужда ограничить скорость.

**Examples:**

```java
Mono<String> one = Mono.just("ok");
Flux<Integer> many = Flux.just(1, 2, 3);
```

**Sources:**

- https://reactivex.io/documentation/observable.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

### Industry Case Studies and Analysis

<details>
<summary>Приведите examples of real-world use cases where reactive programming is applied.</summary>

### Приведите examples of real-world use cases where reactive programming is applied.

**Definition:**

Документированные случаи — всплеск событий интерфейса, прореженный `debounce`, бегущий итог через `scan` и вызов сервиса, который не должен блокировать, обёрнутый в `Mono` на `boundedElastic`.

**How it works:**

- Страница debounce существует для элементов, которые приходят всплесками. Поле поиска — эта форма.
- `zip` сочетает два источника шаг в шаг, и RxJava запрашивает лишь столько, сколько нужно для маленького буфера.
- Обработчик в духе WebFlux возвращает `Mono` или `Flux` и даёт фреймворку подписаться.
- Конечный список в памяти ни в чём этом не нуждается.

**Examples:**

```java
bursty.debounce(10, TimeUnit.MILLISECONDS);
ticks.scan(0, Integer::sum);
```

**Sources:**

- https://reactivex.io/documentation/operators/debounce.html
- https://reactivex.io/documentation/operators/scan.html
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

<details>
<summary>Проанализируйте the impact of reactive programming on cloud-native application design.</summary>

### Проанализируйте the impact of reactive programming on cloud-native application design.

**Definition:**

Облачный дизайн и так масштабируется добавлением экземпляров. Реактивное программирование меняет сам экземпляр: меньше потоков, заблокированных на вызов, и сигнал спроса, чтобы сосед не мог навязать безграничный буфер.

**How it works:**

- Манифест называет это эластичностью и слабой связанностью через асинхронные сообщения.
- `boundedElastic` в Reactor — место, куда уходят оставшиеся блокирующие вызовы облачных SDK, с цикла событий.
- Reactive Streams между хостами — сетевая форма того же предела. RSocket — один протокол, который несёт эту идею.
- Автомасштабирование не чинит обработчик, который делает `block` на цикле событий. Обёртка из FAQ чинит.

**Examples:**

```java
Mono.fromCallable(this::cloudCall).subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://www.reactivemanifesto.org/
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Обсудите how large-scale streaming platforms benefit from reactive programming principles.</summary>

### Обсудите how large-scale streaming platforms benefit from reactive programming principles.

**Definition:**

Проблема потоковой платформы — та, которую формулирует Reactive Streams: живые данные неизвестного объёма должны пересечь границу без произвольного буфера. Польза — спрос или явное отбрасывание либо прореживание.

**How it works:**

- `request(n)` держит множество элементов в пути конечным.
- `onBackpressureDrop` и `sample` сбрасывают всплеск, когда потребитель отстаёт. Вики о backpressure показывает оба семейства.
- `zip` двух бесконечных источников без спроса — контрпример: буфер более быстрого растёт бесконечно.
- Собственный обработчик платформы, например Kafka Streams, сохраняет свой API. Реактивный принцип — это предел, а не переименованный оператор.

**Examples:**

```java
source.onBackpressureDrop();
source.sample(500, TimeUnit.MILLISECONDS);
```

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
</details>

<details>
<summary>Как reactive programming enhance the performance of IoT systems?</summary>

### Как reactive programming enhance the performance of IoT systems?

**Definition:**

Датчики излучают быстрее, чем хранилище успевает писать. `debounce`, `sample` или `onBackpressureDrop` оставляют последнее полезное чтение и отбрасывают остальное — это средство вики о backpressure против всплеска.

**How it works:**

- `sample` излучает самый недавний элемент по периоду. `debounce` излучает элемент только после паузы.
- Отбрасывание верно, когда старое чтение устарело. Буферизация верна, когда каждое чтение нужно сохранить и всплеск ограничен.
- `BehaviorSubject` держит последнее чтение для панели, которая подписалась поздно.
- Каналу устройства всё ещё нужен асинхронный сигнал спроса, если буферу нельзя расти. Это цель Reactive Streams между хостами.

**When to use:**

Прореживайте или отбрасывайте, когда важно только недавнее чтение. Буферизуйте, когда архив не должен терять точки и задана ёмкость, которая завершается ошибкой, а не растёт вечно.

**Examples:**

```java
readings.sample(1, TimeUnit.SECONDS);
readings.onBackpressureDrop();
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://reactivex.io/documentation/operators/debounce.html
- https://reactivex.io/documentation/subject.html
</details>

<details>
<summary>Приведите case study, где Reactive Programming решил проблему scalability.</summary>

### Приведите case study, где Reactive Programming решил проблему scalability.

**Definition:**

Режим отказа, который описывают документы, — поток, заблокированный на запрос, или буфер `zip`, который растёт без границы. Исправление, которое они описывают, — `Mono`, подписанный на `boundedElastic`, для блокирующего вызова и `request` или `onBackpressureDrop` для быстрого источника.

**How it works:**

- До: каждый вызов занимает поток, пока удалённая сторона не вернётся. Потоки кончаются, пока большинство из них ждёт.
- После: цикл событий принимает вызов, `subscribeOn(boundedElastic)` выполняет блокирующий клиент, и `Mono` завершается позже.
- Второй сбой, быстрый издатель и медленный потребитель, чинится спросом, а не большей кучей.
- Имена результата в манифесте — эластичность и отзывчивость. Код — обёртка из FAQ плюс `request` Reactive Streams.

**Examples:**

```java
Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://www.reactivemanifesto.org/
</details>

### Reactive Programming Challenges and Trade-offs

<details>
<summary>Что такое the trade-offs of using reactive programming over traditional imperative programming models?</summary>

### Что такое the trade-offs of using reactive programming over traditional imperative programming models?

**Definition:**

Вы получаете сборку асинхронных последовательностей и предел данных в пути. Платите моделью подписки, более трудными стеками и отсутствием пользы на конечном цикле по памяти.

**How it works:**

- Прямой вызов возвращает значение. Publisher возвращает описание, которое выполняется в subscribe.
- Спрос и `onBackpressureDrop` — инструменты, которых нет у обычного цикла. Они важны, только когда источник может обогнать потребителя.
- `publishOn` рвёт стек между потоками. FAQ Reactor говорит, что `ThreadLocal` за этим переходом не последует.
- Цель — отзывчивость из манифеста. Цепочка операторов оправдана, только когда асинхронность или предел настоящие.

**Examples:**

```java
// imperative
int doubled = n * 2;
// reactive, when the value arrives later
Mono.just(n).map(x -> x * 2);
```

**Sources:**

- https://www.reactive-streams.org/
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Обсудите some of the complexities and pitfalls that come with adopting reactive programming.</summary>

### Обсудите some of the complexities and pitfalls that come with adopting reactive programming.

**Definition:**

Обычные ловушки — подписаться слишком рано, заблокироваться внутри `map`, считать, что `flatMap` сохраняет порядок, и буферизовать быстрый источник без предела.

**How it works:**

- Ничего не выполняется до subscribe. Забытая подписка — тихий пустой ход, как говорит страница scheduler.
- `flatMap` сливает внутренние потоки и может их чередовать. Switch держит только последний.
- Безграничный буфер — сбой `zip` из вики RxJava о backpressure. Используйте буфер с пределом, который падает при переполнении, или отбрасывание.
- Context — не thread-local. После `publishOn` сопутствующие данные читают из Reactor `Context`.

**Examples:**

```java
source.flatMap(id -> load(id));          // merges
source.switchMap(id -> load(id));        // latest only
source.onBackpressureDrop();
```

**Sources:**

- https://reactivex.io/documentation/operators/flatmap.html
- https://reactivex.io/documentation/operators/switch.html
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

<details>
<summary>Как you manage thread exhaustion in a Reactive application?</summary>

### Как you manage thread exhaustion in a Reactive application?

**Definition:**

Исчерпание потоков здесь значит, что блокирующие вызовы сидят на немногих потоках цикла событий. Перенесите эти вызовы на `Schedulers.boundedElastic()` и оставьте цикл событий неблокирующим стадиям.

**How it works:**

- FAQ ставит `subscribeOn` сразу после блокирующего источника. Поздние операторы идут после этой обёртки.
- `subscribeOn` сам не подписывается. Он выбирает scheduler на момент подписки.
- `publishOn` не делает блокирующий `map` безопасным. Блокирующий вызов должен быть внутри callable, который выполняет `boundedElastic`.
- Больше потоков цикла событий не чинит вызов, который блокирует каждый из них. Предел — пул elastic, и он ограничен.

**Examples:**

```java
Mono.fromCallable(this::blockingCall)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
</details>

<details>
<summary>Как справиться с learning curve Reactive Programming? Как обучить команду?</summary>

### Как справиться с learning curve Reactive Programming? Как обучить команду?

**Definition:**

Сначала учат протокол сигналов, затем операторы одной библиотеки, и держат тесты на `StepVerifier`, чтобы неверная модель падала утверждением, а не зависанием в проде.

**How it works:**

- Протокол — `onSubscribe`, `onNext` ноль или больше раз, затем `onError` или `onComplete`. Readme JVM — эта страница.
- Дальше: `map` против `flatMap` против Switch и холодный `from` против Subject. Эти страницы закрывают большинство проектных споров.
- Упражнение — `StepVerifier.expectNext(...).verifyComplete()`. Виртуальное время заменяет `Thread.sleep` для `delay`.
- Scheduler добавляют, только когда последовательность верна на одном потоке. `subscribeOn` и `publishOn` — следующий урок со страницы scheduler Reactor.

**Examples:**

```java
StepVerifier.create(Flux.just(1, 2).map(n -> n * 2))
    .expectNext(2, 4)
    .verifyComplete();
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://projectreactor.io/docs/core/release/reference/testing.html
- https://reactivex.io/documentation/operators/flatmap.html
- https://reactivex.io/documentation/operators/switch.html
</details>

<details>
<summary>Что такое some known issues with debugging Reactive Streams and how do you tackle them?</summary>

### Что такое some known issues with debugging Reactive Streams and how do you tackle them?

**Definition:**

Стеки рвутся на `publishOn`, без subscribe ничего не идёт, а ошибку могут проглотить до подписчика. С этим справляются через `StepVerifier`, подписку в тесте и тем, что `onError` остаётся терминальным.

**How it works:**

- Первое несовпадение в `StepVerifier` бросает `AssertionError` и называет ожидаемый сигнал.
- Отсутствующая подписка — не ошибка логики внутри `map`. Страница scheduler: ничего не происходит до subscribe.
- После `publishOn` идентификатор запроса из `ThreadLocal` исчезает. `expectAccessibleContext` проверяет `Context`, который его заменил.
- Ошибки, зависящие от времени, используют `TestScheduler.advanceTimeBy`, чтобы тест не спал.

**Examples:**

```java
StepVerifier.create(mono)
    .expectNext("ok")
    .verifyComplete();
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/testing.html
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

### Future of Reactive Programming

<details>
<summary>Спрогнозируйте how reactive programming might evolve in the next few years.</summary>

### Спрогнозируйте how reactive programming might evolve in the next few years.

**Definition:**

Устойчивый центр уже специфицирован: Reactive Streams и JDK Flow — один протокол спроса, а библиотеки держат свои операторы сверху. Развитие — больше сред, говорящих на этом протоколе, а не новый набор имён сигналов.

**How it works:**

- Страница reactive-streams.org говорит, что JDK Flow семантически совпадает один к одному, и ждёт, что библиотеки перейдут на эти типы, с адаптером на время перехода.
- Операторы вроде `map`, `zip` и `debounce` остаются возможностями библиотек. Спецификация отказывается их определять.
- Виртуальные потоки уменьшают боль блокирующего вызова, но не ограничивают очередь. Пределом остаётся `request`.
- Четыре свойства манифеста не меняются вместе с библиотекой.

**Examples:**

```java
// java.util.concurrent.Flow matches Publisher, Subscriber,
// Subscription, and Processor.
subscription.request(1);
```

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Как reactive programming interact with AI and machine learning workloads?</summary>

### Как reactive programming interact with AI and machine learning workloads?

**Definition:**

Вызов модели часто блокирующий и одноразовый. Оберните его в `Mono` на `boundedElastic`. Поток признаков — это `Flux`, только когда вызывающий должен ограничить, сколько выводов одновременно в пути.

**How it works:**

- Callable плюс `subscribeOn` из FAQ — адаптер для блокирующего клиента вывода.
- `flatMap` может запустить один вывод на элемент. Предел конкурентности — параметр библиотеки только там, где библиотека его документирует; иначе ограничьте источник через `request`.
- Результаты, которые должны сохранить порядок входа, используют `zip` со входами, а не неупорядоченное слияние.
- Обучение пакета, который уже помещается в память, не нуждается в publisher. Задача Reactive Streams — живой неограниченный поток.

**Examples:**

```java
Mono.fromCallable(() -> model.predict(features))
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://reactivex.io/documentation/operators/zip.html
- https://www.reactive-streams.org/
</details>

<details>
<summary>Обсудите the relevance of reactive programming in the age of serverless architectures.</summary>

### Обсудите the relevance of reactive programming in the age of serverless architectures.

**Definition:**

Бессерверный вызов — обычно один запрос и один ответ, то есть `Mono` или просто async/await. Publisher оправдан, когда функция потребляет или производит последовательность, которую нужно ограничить.

**How it works:**

- Холодный старт и короткая жизнь спорят против долгоживущего Subject внутри функции.
- Блокирующий вызов SDK всё ещё принадлежит `boundedElastic`, если среда — цикл событий. На среде, которая даёт поток на вызов, достаточно обычного вызова.
- Потоковый ответ — это `Flux` со спросом, чтобы буфер платформы оставался конечным.
- Эластичность манифеста здесь — платформа добавляет вызовы. Задача кода — не блокировать те немногие потоки, которые ему дали.

**When to use:**

Возвращайте одно значение через async/await или `Mono`, когда функция делает одно дело. `Flux` — когда функция стримит и потребитель должен применить спрос.

**Examples:**

```java
Mono<String> body = Mono.fromCallable(this::handler)
    .subscribeOn(Schedulers.boundedElastic());
```

**Sources:**

- https://projectreactor.io/docs/core/release/reference/faq.html
- https://www.reactivemanifesto.org/
- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
</details>

<details>
<summary>Какую роль might reactive programming play in the future of distributed systems and edge computing?</summary>

### Какую роль might reactive programming play in the future of distributed systems and edge computing?

**Definition:**

Узлы на краю — медленные потребители быстрого ядра или наоборот. Роль реактивного программирования — асинхронный спрос, которого readme Reactive Streams требует между хостами, чтобы очередь на маленьком узле оставалась ограниченной.

**How it works:**

- Синхронный вызов backpressure заблокировал бы канал и, как говорит readme, отменил бы пользу асинхронности.
- `sample` или `onBackpressureDrop` подходит каналу, который может терять промежуточные чтения. Буфер с пределом — каналу, который не может.
- Управление сообщениями в манифесте — как край остаётся отвязанным от пула потоков ядра.
- Операторы остаются на концах. По проводу идут спрос и элементы — это протокол, а не `map`.

**Examples:**

```java
edge.onBackpressureDrop();
subscription.request(4);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://github.com/ReactiveX/RxJava/wiki/Backpressure
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Как current advancements in hardware affecting reactive programming paradigms?</summary>

### Как current advancements in hardware affecting reactive programming paradigms?

**Definition:**

Больше ядер делает асинхронную границу внутри одной машины обычной: readme Reactive Streams уже называет несколько ядер CPU причиной передавать элементы между пулами потоков с ограниченной очередью.

**How it works:**

- Более быстрые ядра не снимают нужду в `request`. Более быстрый производитель переполняет более медленного потребителя ещё скорее.
- `publishOn(Schedulers.parallel())` в Reactor — как стадии, занятые CPU, используют эти ядра. Блокирующий ввод-вывод этому scheduler всё ещё не принадлежит; FAQ использует `boundedElastic`.
- Виртуальные потоки удешевляют блокирующий вызов. Спрос они не реализуют. `zip` двух бесконечных источников всё ещё может буферизовать без границы.
- Четыре свойства манифеста по-прежнему про отзывчивость при сбое и нагрузке, а не про число ядер.

**Examples:**

```java
Flux.range(1, 100)
    .publishOn(Schedulers.parallel())
    .map(n -> n * n);
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://projectreactor.io/docs/core/release/reference/coreFeatures/schedulers.html
- https://projectreactor.io/docs/core/release/reference/faq.html
- https://www.reactivemanifesto.org/
</details>

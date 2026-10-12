# Reactive Programming — Interview

Answers cover the full reactive-programming interview set.

[← Back to hub](./0_system_design.md)

### Related docs

- [Reactive Programming — Interview](./0_sd_reactive_programming_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Reactive Programming](https://devinterview.io/questions/web-and-mobile-development/reactive-programming-interview-questions/).

### Reactive Programming Fundamentals

<details>
<summary>What is Reactive Programming, and how does it differ from Procedural Programming?</summary>

### What is Reactive Programming, and how does it differ from Procedural Programming?

**Definition:**

Reactive programming describes a computation as a chain of reactions to values that arrive over time. Procedural code asks for the next value and then continues.

**How it works:**

- A reactive chain is built from a source, operators, and a subscriber. Work starts when someone subscribes, and values are pushed downstream.
- Procedural code pulls: a loop reads a list, calls a function, and stores the return value before the next line.
- Reactive code composes async steps (map, filter, flatMap) without nesting callbacks. Errors and completion are events in the same sequence, not a separate return channel.
- The style pays off for UI events, network responses, and sensor ticks. A straight-line script over a fixed array does not need it.

**Examples:**

```text
procedural: items = load(); for item in items: save(transform(item))
reactive:   load().map(transform).subscribe(save)
```

The second form can emit `item` as each response arrives, and `save` runs then, not after the whole list exists.
</details>

<details>
<summary>Explain the concept of data streams in Reactive Programming.</summary>

### Explain the concept of data streams in Reactive Programming.

**Definition:**

A data stream is a sequence of items over time, plus a terminal signal that the sequence failed or finished.

**How it works:**

- Each item is an `onNext`. A failure is a single `onError`. A normal end is `onComplete`. After either terminal signal, no further items arrive.
- The producer can be finite (a list) or open-ended (clicks, ticks). Operators sit between producer and consumer and return a new stream.
- A cold stream starts work per subscriber. A hot stream is already running and subscribers see what happens from the moment they join.
- If the producer is faster than the consumer, the stream needs a policy: buffer, drop, or ask the producer to slow down (backpressure).
</details>

<details>
<summary>What is the Observer pattern, and how is it fundamental to Reactive Programming?</summary>

### What is the Observer pattern, and how is it fundamental to Reactive Programming?

**Definition:**

The observer pattern lets one subject notify many dependents when its state changes. Reactive streams are that pattern plus a contract for sequences, errors, and cancellation.

**How it works:**

- A subject holds subscribers. When something happens, it calls each subscriber instead of the subscribers polling.
- Reactive libraries name the subject an Observable (or Publisher) and the dependent an Observer (or Subscriber).
- The extra contract is ordered events: zero or more values, then either an error or a completion, and a way to cancel.
- Operators are observers that are also observables, so a chain is a pipeline of the same pattern.

**Examples:**

A button is the subject. Each listener is an observer. A reactive chain is the same idea with `map` and `filter` between the click and the listener, and with `dispose` to detach.
</details>

<details>
<summary>Describe the role of Observables and Observers in Reactive Programming.</summary>

### Describe the role of Observables and Observers in Reactive Programming.

**Definition:**

An Observable is the source that can emit items. An Observer is the consumer that receives them. Nothing flows until the observer subscribes.

**How it works:**

- The observable defines how values are produced. The observer implements `onNext`, `onError`, and `onComplete`.
- Subscribe connects them and returns a handle used to cancel. Each subscription can start its own work on a cold source.
- Operators return a new observable. The downstream observer does not see the upstream type; it sees the transformed sequence.
- In Reactive Streams the names are Publisher and Subscriber, and the subscriber also requests how many items it can accept.

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
<summary>How do you create an Observable stream?</summary>

### How do you create an Observable stream?

**Definition:**

You either wrap values you already have, or you describe a producer that will call the observer when something happens.

**How it works:**

- Factory methods cover the common cases: `just` and `fromIterable` for existing values, `interval` and `timer` for time, `empty`, `never`, and `error` for the terminal cases.
- `defer` builds a fresh observable per subscriber, which is how a cold call stays cold.
- `create` is the escape hatch: the function receives an emitter and must call `onNext`, then `onComplete` or `onError`, and it should stop when the subscription is disposed.
- Operators such as `map` do not create the original source; they wrap one. The stream stays lazy until `subscribe`.

**Examples:**

```text
ticks = Observable.interval(1, TimeUnit.SECONDS)
one_shot = Observable.defer(() -> Observable.just(fetchStatus()))
```

`interval` starts the clock at subscribe time. `defer` calls `fetchStatus` once per subscriber, not when the chain is assembled.
</details>

<details>
<summary>What is backpressure in the context of Reactive Programming?</summary>

### What is backpressure in the context of Reactive Programming?

**Definition:**

Backpressure is a way for a slow subscriber to tell a fast publisher how many items it can accept, so the publisher does not overflow memory.

**How it works:**

- In Reactive Streams the subscriber calls `request(n)`. The publisher emits at most that many items, then waits for the next request.
- Without that signal, an unbounded source such as a network feed can fill a queue until the process runs out of memory.
- When the source cannot slow down, a strategy still has to choose: buffer with a limit, drop newest or oldest, keep only the latest, or fail the stream.
- RxJava `Flowable` speaks this protocol. `Observable` does not; it assumes the consumer keeps up or that you applied an operator such as `onBackpressureBuffer`.

**Examples:**

A parser requests 32 lines. The file source reads 32, then pauses. After the parser finishes the batch it requests 32 more. A UI stream that cannot pause the user may keep only the latest click instead.
</details>

<details>
<summary>Explain the difference between cold and hot Observables.</summary>

### Explain the difference between cold and hot Observables.

**Definition:**

A cold observable starts a private sequence for each subscriber. A hot observable shares one sequence that is already in progress.

**How it works:**

- Cold: a second subscriber repeats the work. `just`, `fromIterable`, and a fresh HTTP call built with `defer` behave this way. Each subscriber can see the whole sequence from the start.
- Hot: subscribers share one producer. A late subscriber misses earlier items. Clicks, sensor ticks, and a published subject are hot.
- `publish` or `share` turns a cold source into a hot one so a network call runs once. `replay` keeps a buffer so a late subscriber still sees recent items.
- Pick cold when every subscriber needs its own result. Pick hot when the event exists whether or not anyone is listening.
</details>

<details>
<summary>What is the role of the Subscription in Reactive Programming.</summary>

### What is the role of the Subscription in Reactive Programming.

**Definition:**

A subscription is the link created by subscribe: it starts the flow, and it is the handle used to cancel or to request more items.

**How it works:**

- Subscribe returns a `Disposable` in RxJava, or a Reactive Streams `Subscription`. Disposing it unsubscribes and should release sockets, timers, and listeners.
- On a `Flowable`, the subscription is also how the subscriber calls `request(n)` for backpressure.
- One observer can hold many subscriptions. A composite disposable disposes the group when a screen closes.
- Forgetting the handle leaves the upstream running. That is the usual source of a leaked listener after a screen is gone.

**Examples:**

```text
Disposable d = ticks.subscribe(this::onTick);
// later, when the screen closes
d.dispose();
```
</details>

<details>
<summary>How do you unsubscribe from a stream to prevent memory leaks?</summary>

### How do you unsubscribe from a stream to prevent memory leaks?

**Definition:**

Call dispose on the subscription when the consumer's lifetime ends, and clear any composite that collected those subscriptions.

**How it works:**

- `Disposable.dispose()` stops the upstream and drops the observer reference so the chain can be collected.
- Add each subscription to a `CompositeDisposable` owned by the screen, presenter, or scope. Clear that composite in the matching teardown (`onDestroy`, scope cancel, component unmount).
- Operators such as `takeUntil(destroySignal)` and `take(1)` end the sequence without a hand-held disposable, which is safer when the teardown is itself a stream.
- Dispose is idempotent. Disposing twice is safe. Failing to dispose a hot source or an `interval` keeps a thread or a listener alive.

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
<summary>What are operators in Reactive Programming, and what are they used for?</summary>

### What are operators in Reactive Programming, and what are they used for?

**Definition:**

Operators are functions from one stream to another. They transform, filter, combine, or change the timing of items without the subscriber knowing how the source works.

**How it works:**

- Transforming operators include `map` (one item to one item) and `flatMap` (one item to a stream that is merged).
- Filtering operators include `filter`, `distinct`, `take`, `skip`, and `debounce`.
- Combining operators include `zip`, `merge`, `concat`, and `combineLatest`. They differ in whether they wait, pair by index, or interleave.
- Error and side-effect operators include `retry`, `onErrorReturn`, and `doOnNext`. A scheduler operator such as `observeOn` moves the downstream work to another thread.
- Each operator subscribes to its upstream only when someone subscribes to it, so a long chain is still lazy.
</details>

### Reactive Programming with RxJava

<details>
<summary>What is RxJava, and how does it implement Reactive Programming?</summary>

### What is RxJava, and how does it implement Reactive Programming?

**Definition:**

RxJava is a Java library of observable sequences and operators. It is the usual way JVM code consumes reactive streams.

**How it works:**

- Base types match how many items you expect: `Observable` and `Flowable` for many, `Single` for one, `Maybe` for one or none, `Completable` for none.
- `Flowable` implements Reactive Streams backpressure. `Observable` does not request a credit; use it when the volume is bounded or already on the UI cadence.
- Chains stay lazy until subscribe. Schedulers move subscribe-time work and observation-time work onto thread pools.
- A sequence is one source of items for that subscription: values, then a single terminal event. Time-based sources such as `interval` are ordinary observables.

**Examples:**

```text
Single<User> user = api.user(id)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread());
```
</details>

<details>
<summary>How does RxJava handle multithreading?</summary>

### How does RxJava handle multithreading?

**Definition:**

RxJava does not pick a thread for you. `subscribeOn` chooses where the source subscribes, and `observeOn` chooses where later operators and the observer run.

**How it works:**

- `Schedulers.io()` is a growing pool for blocking I/O. `Schedulers.computation()` is sized around the CPU count for non-blocking work. `Schedulers.single()` is one shared thread. `trampoline` queues work on the current thread.
- On Android, `AndroidSchedulers.mainThread()` hops back to the UI thread. Touching views from `io` is a crash.
- The closest `subscribeOn` to the source wins if several are chained; extra ones upstream of it do not move the subscription again. Every `observeOn` switches from that point downward.
- Operators are not magically thread-safe against a shared mutable variable. Keep state inside the chain or synchronise it yourself.
</details>

<details>
<summary>Explain how the `flatMap` operator works in RxJava.</summary>

### Explain how the `flatMap` operator works in RxJava.

**Definition:**

`flatMap` turns each upstream item into an inner observable and merges those inners. Items from different inners may arrive interleaved.

**How it works:**

- The outer function returns `Observable<R>` (or another reactive type). `flatMap` subscribes to each inner and forwards its items downstream as one sequence.
- Inners run concurrently up to a max-concurrency argument. Order is not the upstream order. `concatMap` waits for one inner to complete before subscribing to the next. `switchMap` disposes the previous inner when a new outer item arrives.
- An error from any inner fails the downstream unless you handle it inside the inner.
- Use it when one value must become a further async call, such as a user id becoming that user's orders.

**Examples:**

```text
userIds.flatMap(id -> ordersApi.list(id))
```

If user 2's orders return before user 1's, those rows still go downstream immediately. `concatMap` would hold user 2 until user 1's call finished.
</details>

<details>
<summary>What is the purpose of the `zip` operator in RxJava?</summary>

### What is the purpose of the `zip` operator in RxJava?

**Definition:**

`zip` pairs items from two or more sources by index and emits one combined item each time every source has produced the next index.

**How it works:**

- A zipper function receives one item from each source. The output waits for the slowest source at that index.
- The sequence completes when the shortest source completes. Extra items on a longer source are dropped.
- An error on any source fails the zip. Sources typically stay cold and are subscribed together.
- It fits independent calls that belong together: profile and settings for one screen, or a value with a timestamp from a second stream. It does not mean 'latest of each'; that is `combineLatest`.

**Examples:**

```text
Observable.zip(profileApi.get(id), settingsApi.get(id),
    (profile, settings) -> new Screen(profile, settings))
```

The screen object is emitted once, after both calls have a result. If settings emits twice and profile emits once, only the first pair is produced.
</details>

<details>
<summary>How do you handle errors in an RxJava stream?</summary>

### How do you handle errors in an RxJava stream?

**Definition:**

An error is terminal: after `onError`, that sequence emits nothing else. Recovery means turning the failure into a value, another sequence, or a retry before it reaches the subscriber.

**How it works:**

- Let it reach `onError` in subscribe when the UI should show a failure. Do not swallow it in an empty callback.
- `onErrorReturn` replaces the error with a fallback item and completes. `onErrorResumeNext` switches to another observable, such as a cache.
- `retry(n)` resubscribes to the upstream. That repeats a cold call. Retrying a hot source does not rewind events that already happened.
- `doOnError` logs or records a metric and still lets the error continue. Put recovery operators upstream of `observeOn` if the fallback must be chosen on the worker thread.
</details>

<details>
<summary>Describe the differences between `map` and `flatMap` operators in RxJava.</summary>

### Describe the differences between `map` and `flatMap` operators in RxJava.

**Definition:**

`map` turns each item into one item. `flatMap` turns each item into an inner observable and merges those inners into one sequence.

**How it works:**

- The ReactiveX map operator applies a function to each item the source emits and returns an observable of the results. RxJava's map does not by default run on a particular Scheduler.
- flatMap's function returns an observable per item. FlatMap merges those observables, so their items may interleave. If any inner signals onError, the outer fails immediately.
- concatMap is the ordered cousin: it subscribes to the next inner only after the previous one completes. switchMap unsubscribes from the previous inner when a new outer item arrives.
- A flatMap variant takes an int that caps how many inners are subscribed at once. When the cap is reached, it waits for one inner to terminate before subscribing to another.
- Use map to square a number or rename a field. Use flatMap when the item must become another asynchronous call, such as an id becoming a list of orders.

**Examples:**

```text
numbers.map(n -> n * n)                 // 1, 2, 3 -> 1, 4, 9
numbers.flatMap(n -> Observable.just(n * 2, n * 3))
// merges; order across inners is not guaranteed
```

**Sources:**

- https://reactivex.io/documentation/operators/map.html
- https://reactivex.io/documentation/operators/flatmap.html
</details>

<details>
<summary>What is a `Scheduler` in RxJava?</summary>

### What is a `Scheduler` in RxJava?

**Definition:**

A Scheduler is the concurrency abstraction ReactiveX uses so operators can run work off the thread that subscribed. `subscribeOn` chooses where the source starts; `observeOn` chooses where later notifications are delivered.

**How it works:**

- By default the chain runs and notifies observers on the thread where `subscribe` was called.
- `subscribeOn` sets the scheduler the observable operates on, wherever that operator sits in the chain.
- `observeOn` changes the thread used to send notifications downstream of that operator.
- RxJava’s standard set, described in the library readme, includes computation, a cached scheduler for I/O, single for sequential work, and trampoline.

**When to use:**

Use `subscribeOn` once, at the source, for blocking or slow creation. Use `observeOn` where the next stage must run on a different thread, such as a single consumer.

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
<summary>Explain the `Subject` class in RxJava.</summary>

### Explain the `Subject` class in RxJava.

**Definition:**

A Subject is both an Observer and an Observable. It can subscribe to a source and reemit what it sees, and it can also emit its own items to its own observers.

**How it works:**

- Subscribing a Subject to a cold source starts that source. The Subject then behaves as a hot view of it.
- `AsyncSubject` emits only the last item, and only after the source completes. An upstream error is forwarded and no item is emitted.
- `BehaviorSubject` starts a new observer with the most recent item, or a seed if nothing has been emitted yet, then continues.
- `PublishSubject` emits only items that arrive after subscription. `ReplaySubject` replays earlier items to a new observer.

**When to use:**

Use a plain observable when each subscriber should get its own cold sequence. Use a Subject when many subscribers must share one live sequence, and pick the variant by how much history a late subscriber needs.

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
<summary>How can you achieve thread-safety in RxJava?</summary>

### How can you achieve thread-safety in RxJava?

**Definition:**

An Rx contract assumes notifications for one observer are serial. The RxJava Subject wiki says calls into a Subject from several threads are not safe, and a serialized wrapper restores that contract.

**How it works:**

- ReactiveX says that on the observer side of a Subject, concurrent `onNext` from multiple threads can leave an undefined state.
- The RxJava wiki shows `new SerializedSubject(unsafe)` so those calls are queued onto one sequence.
- `subscribeOn` and `observeOn` move work between schedulers, but they do not make an arbitrary Subject safe if you also call `onNext` yourself from two threads.
- Prefer emitting from one thread, or from operators that already hop with `observeOn`, instead of sharing a mutable collector.

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
<summary>What is the `debounce` operator in RxJava?</summary>

### What is the `debounce` operator in RxJava?

**Definition:**

`debounce` (also called `throttleWithTimeout`) emits an item only when a timespan has passed without the source emitting another item. The last item is still emitted if nothing follows it inside the window.

**How it works:**

- Items that are quickly followed by another item are filtered out.
- The time span is a duration plus a time unit. A Scheduler argument chooses where the timer runs; without it the operator uses its default scheduler.
- A variant calls a function per item and waits on the observable that function returns. A new source item before that inner completes drops the previous one.
- The backpressure wiki lists `debounce` next to `sample` and `throttleFirst` as a way to shrink a burst before a slow consumer.

**When to use:**

Use `debounce` for a burst where only the quiet-period item matters, such as a search box. Use `sample` when you want the latest item on a fixed clock instead of a quiet gap.

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
<summary>What is Reactive Extensions (ReactiveX or Rx) and RxJS?</summary>

### What is Reactive Extensions (ReactiveX or Rx) and RxJS?

**Definition:**

ReactiveX is the family of libraries that compose asynchronous events as observable sequences. RxJS is the JavaScript member of that family; RxJava is the JVM member. The same operator names — map, zip, debounce, subjects, schedulers — are documented once and implemented per language.

**How it works:**

- An observable pushes items, an error, or completion to an observer.
- Operators such as `zip` and `debounce` are pure functions from one sequence to another. They are listed on the ReactiveX operator pages, which name RxJS and RxJava together.
- A Subject is the bridge that is both observer and observable, shared by the implementations.
- Schedulers are how a chain leaves the subscribing thread. In the browser that hop is smaller; the operator contract is the same.

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
<summary>Give an example of how to implement a simple RxJS Observable.</summary>

### Give an example of how to implement a simple RxJS Observable.

**Definition:**

A simple observable is a function that, on subscribe, pushes items and then a completion. ReactiveX’s `from` turns an array into that sequence; RxJS exposes it as `from`.

**How it works:**

- Nothing is emitted until something subscribes. That is the cold behavior the RxJava backpressure page describes for a sequence built from a static collection.
- `from` walks the array and emits each element in order, then completes.
- Operators such as `map` return a new observable. They do not run the array walk by themselves.
- The observer receives `next` calls and one terminal signal. After completion the sequence emits nothing else.

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
<summary>How does RxJS's `Observable` differ from a Promise?</summary>

### How does RxJS's `Observable` differ from a Promise?

**Definition:**

A Promise settles once, eagerly, with a value or a rejection. An Observable is a sequence: it can emit many items, it starts when subscribed, and it can be stopped by unsubscribing.

**How it works:**

- Constructing a Promise runs the executor. Constructing an Observable from an array does not walk it until `subscribe`, which is the cold behavior described for a static source.
- An observable may call `next` many times and then `complete` or `error`. A Promise has one resolution.
- Operators such as `map` and `debounce` compose the sequence. A Promise chain is `then` on a single future value.
- Unsubscribing drops the observer. A Promise has no cancel in the same sense; it still settles.

**When to use:**

Use a Promise for one result that should start immediately. Use an Observable when there are many values, when work should wait for subscribe, or when the consumer must be able to unsubscribe.

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
<summary>Explain the concept of marble diagrams in RxJS.</summary>

### Explain the concept of marble diagrams in RxJS.

**Definition:**

A marble diagram draws one observable as a time arrow. Marbles are items, a vertical bar is completion, and an X is an error. Operator pages on ReactiveX use that drawing to show input arrows and the output arrow.

**How it works:**

- Time moves to the right. The horizontal gap between marbles is the gap between emissions.
- The debounce page’s diagram drops marbles that are followed too quickly and keeps the one after a quiet gap, including the last marble.
- A second arrow under the first is the operator’s result, not a second subscription.
- Completion on the source ends the output. An error on the source ends it with an error instead.

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
<summary>In RxJS, how would you convert an array to an Observable sequence?</summary>

### In RxJS, how would you convert an array to an Observable sequence?

**Definition:**

The ReactiveX `from` operator converts an array, an iterable, or a similar structure into an observable that emits each element and then completes. RxJS names that function `from`; RxJava 2 names the iterable form `fromIterable`.

**How it works:**

- Subscription starts the walk. A second subscriber walks the array again, because a static source is cold.
- Order is the array order. There is no timer unless you add one.
- `map` after `from` transforms each element without changing the completion.
- An empty array completes without a `next`.

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
<summary>Describe the `switchMap` operator in RxJS.</summary>

### Describe the `switchMap` operator in RxJS.

**Definition:**

ReactiveX names this operator Switch. It takes an observable that emits observables and emits items only from the most recently emitted inner observable, unsubscribing from the previous inner.

**How it works:**

- Switch subscribes to an inner observable when the outer emits it.
- When the outer emits a newer inner, the previous inner is unsubscribed. Later items from the old inner are ignored.
- RxJS spells the same operator `switchMap`: the function returns the inner for each outer item.
- This is the opposite of merging every inner. Only one inner is active.

**When to use:**

Use `switchMap` when a new outer item makes the previous inner obsolete, such as the latest search. Use `flatMap` when every inner must be merged, which is the contract already stated for flatMap.

**Examples:**

```javascript
// RxJS name for ReactiveX Switch
source.pipe(switchMap(id => from(load(id))));
```

**Sources:**

- https://reactivex.io/documentation/operators/switch.html
</details>

<details>
<summary>How do you handle backpressure in RxJS?</summary>

### How do you handle backpressure in RxJS?

**Definition:**

Backpressure is what you do when a source emits faster than the consumer can take. RxJS has no Reactive Streams `request(n)` pull. The RxJava backpressure page’s first defense still applies: shrink the sequence with `debounce`, `sample`, or `buffer` before it reaches the subscriber.

**How it works:**

- The backpressure wiki shows `debounce`, `sample` / `throttleLast`, and `throttleFirst` cutting a burst down.
- `buffer` collects items and emits them as lists, so the subscriber sees fewer, larger notifications.
- A cold source built with `from` already emits only as the subscriber pulls by being synchronous. A hot source such as UI events does not wait.
- JVM libraries add `onBackpressureBuffer` and `onBackpressureDrop` because they implement `request`. Those two operators are not RxJS methods.

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
<summary>What is the `BehaviorSubject` in RxJS?</summary>

### What is the `BehaviorSubject` in RxJS?

**Definition:**

A `BehaviorSubject` is the Subject variant that, on subscription, emits the most recent item, or a seed if nothing has been emitted yet, and then emits later items.

**How it works:**

- The ReactiveX subject page states that contract directly.
- A late subscriber does not miss the current value. It also does not see the whole history; that is `ReplaySubject`.
- It is hot: the value exists whether or not someone is subscribed.
- As with any Subject, concurrent emissions from several threads are not part of the contract. RxJS is usually single-threaded; the JVM wiki still says to serialize a Subject.

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
<summary>What are the uses of the `scan` operator in RxJS?</summary>

### What are the uses of the `scan` operator in RxJS?

**Definition:**

`scan` applies a function to each item together with the accumulator so far and emits every intermediate accumulator. It is the running fold of a sequence.

**How it works:**

- The ReactiveX scan page describes emitting the accumulated value on each source item, not only the final one.
- You pass a seed. The first emission is the seed combined with the first item.
- The sequence still completes or errors with the source. `scan` does not hide an error.
- A counter, a growing sum, or a state object folded from events are the usual shapes.

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
<summary>How do you do error recovery in RxJS?</summary>

### How do you do error recovery in RxJS?

**Definition:**

An error is terminal for that sequence. Recovery means replacing the failure with a value, another observable, or a resubscription before the error reaches the subscriber.

**How it works:**

- Letting the error reach `subscribe` is correct when the UI should show the failure.
- ReactiveX recovery follows the same split the RxJava error path uses: return a fallback item, switch to another observable, or resubscribe.
- Resubscribing repeats a cold source such as `from` or a new HTTP call. It does not rewind a hot Subject.
- A side-effect logger must rethrow or return the error channel. Swallowing the error in an empty callback hides the failure.

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
<summary>What is the Reactive Streams specification?</summary>

### What is the Reactive Streams specification?

**Definition:**

Reactive Streams is a standard for asynchronous stream processing with non-blocking backpressure. Its goal is to pass elements across an asynchronous boundary without forcing the receiver to buffer an arbitrary amount.

**How it works:**

- The JVM readme says a fast source must not overwhelm the destination, and the queues between threads stay bounded because backpressure is part of the model.
- Backpressure signals themselves must be asynchronous. A synchronous signal would cancel the benefit of asynchronous processing.
- The specification is the API plus a TCK. Extra features are allowed if the implementation still passes the TCK.
- JDK `java.util.concurrent.Flow` is semantically 1:1 with these types. Transformation of streams is explicitly not part of the specification.

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
<summary>Describe the main interfaces of the Reactive Streams API.</summary>

### Describe the main interfaces of the Reactive Streams API.

**Definition:**

The JVM readme lists four components: Publisher, Subscriber, Subscription, and Processor. A Publisher provides a possibly unbounded sequence according to demand from its Subscriber.

**How it works:**

- `Publisher.subscribe(Subscriber)` starts the protocol. The readme gives the signal order `onSubscribe onNext* (onError | onComplete)`.
- A Subscription is what the subscriber receives in `onSubscribe`. Demand travels on that subscription.
- A Processor is both a Subscriber and a Publisher, so it can sit between two stages.
- The specification does not define map or filter. Those are library operators on top of these four types.

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
<summary>How does backpressure work within the Reactive Streams API?</summary>

### How does backpressure work within the Reactive Streams API?

**Definition:**

The subscriber tells the publisher how many elements it can accept. The publisher must not emit more than that outstanding demand, so the queue between them stays bounded.

**How it works:**

- The Reactive Streams site says backpressure is what keeps a fast source from overwhelming the destination.
- Demand is requested on the Subscription. Signals, including that request, are required to be non-blocking.
- The RxJava backpressure page shows the same pull in `zip`: a small buffer per source, and one more item requested only after an item is emitted.
- `onBackpressureBuffer` and `onBackpressureDrop` are RxJava adapters for a source that ignores `request`. They are not part of the four interfaces.

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
<summary>What is Project Reactor, and how is it related to Reactive Streams?</summary>

### What is Project Reactor, and how is it related to Reactive Streams?

**Definition:**

Project Reactor is a Reactive Streams library for the JVM. `Flux` and `Mono` are Publishers, so a Reactor type can be subscribed with the same demand protocol as any other Reactive Streams publisher.

**How it works:**

- Reactor adds operators on top of the specification. The specification itself, the JVM readme says, does not cover transformation.
- `subscribeOn` and `publishOn` move work across schedulers. The schedulers page says nothing happens until subscribe, and `publishOn` affects the chain after it while `subscribeOn` affects subscription.
- `StepVerifier` subscribes and compares signals, which is Reactor’s test tool, not part of the spec.
- A non-Reactor Publisher does not carry Reactor `Context`. The context page says context propagation needs Reactor operators.

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
<summary>Compare and contrast Mono and Flux in Project Reactor.</summary>

### Compare and contrast Mono and Flux in Project Reactor.

**Definition:**

`Mono` is the Reactor publisher of zero or one item. `Flux` is the publisher of zero to many items. Both complete or error, and both honor Reactive Streams demand.

**How it works:**

- `Mono.just` emits one value and completes. `Flux.just` emits each argument, then completes.
- The same schedulers apply. `subscribeOn` still only picks the thread for subscription; `publishOn` still switches the downstream chain.
- `transform` is documented on a `Flux` chain and works the same way on a `Mono`: the function is applied at assembly time.
- Use a `Mono` for one HTTP response body. Use a `Flux` for a stream of rows. Flattening a `Flux` of `Mono` is an operator on top of the spec, not a third interface.

**When to use:**

Pick `Mono` when the contract is one result or none. Pick `Flux` when the contract is a sequence.

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
<summary>How does Project Reactor enable asynchronous and non-blocking operations?</summary>

### How does Project Reactor enable asynchronous and non-blocking operations?

**Definition:**

Reactor keeps the chain lazy until subscribe, then runs stages on schedulers so a slow source does not block the caller. Demand from the subscriber bounds how much may be in flight.

**How it works:**

- The schedulers page: nothing happens until you subscribe. `subscribeOn` changes the thread that subscribes. Its position does not pick which later operator runs where; the closest downstream `subscribeOn` is the one that matters.
- `publishOn` switches the thread for operators after it, up to the next `publishOn`.
- The FAQ shows a blocking call wrapped and placed on `subscribeOn` immediately after the source, with later operators after that wrapper.
- Reactive Streams requires the exchange, including backpressure, to be non-blocking. Buffering without bound would break that.

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
<summary>What are some strategies for testing Reactor flows?</summary>

### What are some strategies for testing Reactor flows?

**Definition:**

`StepVerifier` subscribes to a `Flux` or `Mono` and compares each signal with the next expectation. The first mismatch throws `AssertionError`.

**How it works:**

- Create the verifier on the publisher your method returns. Expectations such as `expectNext` then `verifyComplete` both describe the sequence and start it.
- `expectAccessibleContext` checks Reactor context, then `then()` returns to sequence expectations.
- `StepVerifierOptions` can start the test with an initial context.
- `expectNoAccessibleContext` is for a publisher that is not a Reactor type and therefore cannot propagate context.

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
<summary>How do you convert blocking code to non-blocking reactive code in Reactor?</summary>

### How do you convert blocking code to non-blocking reactive code in Reactor?

**Definition:**

Isolate the blocking call in a `Callable` and subscribe that `Mono` on a scheduler meant for blocking work. The FAQ says `subscribeOn` does not itself subscribe; it only chooses the scheduler used when subscribe happens.

**How it works:**

- Put `subscribeOn` immediately after the blocking source. Further operators are defined after that wrapper.
- `Schedulers.boundedElastic()` is the scheduler the FAQ uses for that wrapper.
- Do not call the blocking method inside `map` on a non-blocking thread. That occupies the event-loop thread the schedulers page is trying to keep free.
- The outer type stays a `Mono` or `Flux`, so callers compose it instead of waiting.

**When to use:**

Wrap a call that blocks. Leave an already asynchronous publisher alone; adding `subscribeOn` around a non-blocking source only adds a thread hop.

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
<summary>In Reactor, how is `Context` used?</summary>

### In Reactor, how is `Context` used?

**Definition:**

Reactor `Context` is a map tied to a subscription, not to a thread. It propagates from the final subscriber upward so operators can read values such as a correlation id without a `ThreadLocal`.

**How it works:**

- Write it with `contextWrite` at subscription time. `contextWrite` merges the view you pass with the downstream context via `putAll`, producing a new context for upstream.
- Read it from a source with `deferContextual`. In the middle of a chain, use `transformDeferredContextual`. Inside `flatMap`, `Mono.deferContextual` materializes the view.
- The FAQ contrasts this with MDC, which assumes one thread. `publishOn` breaks that assumption; context does not.
- `StepVerifier.expectAccessibleContext` asserts what was propagated. A non-Reactor publisher has no accessible context.

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
<summary>What is the `transform` operator and how do you use it in Reactor?</summary>

### What is the `transform` operator and how do you use it in Reactor?

**Definition:**

`transform` encapsulates a piece of operator chain in a function and applies that function once, at assembly time. The Reactor guide says this is equivalent to writing the operators directly on the chain.

**How it works:**

- Every subscriber of that sequence sees the same assembled operators.
- `transformDeferred` applies the function per subscriber, so each subscription can get a different chain.
- The guide’s example prints, then `transform(filterAndMap)`, then subscribes.
- Assembly-time means the function runs when the chain is built, not once per element.

**When to use:**

Use `transform` to reuse one chain for every subscriber. Use `transformDeferred` when the chain must depend on the individual subscription.

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
<summary>Define Reactive Systems and how they relate to Reactive Programming.</summary>

### Define Reactive Systems and how they relate to Reactive Programming.

**Definition:**

The Reactive Manifesto calls a system Reactive when it is responsive, resilient, elastic, and message-driven. Reactive programming is the technique of composing asynchronous sequences; it is one way to build the message-driven part, not the whole definition.

**How it works:**

- Responsive means the system answers in a timely way. Resilient means it stays responsive under failure. Elastic means it stays responsive under changing load. Message-driven means the parts communicate by asynchronous messages.
- The manifesto says such systems are more flexible, loosely coupled, and scalable.
- A `Flux` pipeline is reactive programming. It becomes a reactive system only when failure, load, and message boundaries are designed as well.
- Reactive Streams is the JVM contract for one of those boundaries: demand so a fast sender cannot force an unbounded queue.

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
<summary>What role does Reactive Programming play in building microservices?</summary>

### What role does Reactive Programming play in building microservices?

**Definition:**

Between services, reactive programming is how one process consumes a stream without blocking its threads and without accepting an unbounded backlog. Reactive Streams is the contract for that exchange.

**How it works:**

- The Reactive Streams readme targets an asynchronous boundary, including another thread pool or another host, with bounded queues.
- A `Mono` is the shape of one response. A `Flux` is the shape of many. Both stay lazy until subscribe.
- Blocking I/O is isolated with `subscribeOn(Schedulers.boundedElastic())`, as the Reactor FAQ shows, so the calling thread is not held.
- The manifesto’s message-driven trait is the service boundary. Operators such as `map` are inside one process.

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
<summary>Explain the difference between Reactive Systems and Reactive Programming.</summary>

### Explain the difference between Reactive Systems and Reactive Programming.

**Definition:**

Reactive programming composes sequences with operators. A reactive system, in the manifesto, is an architecture that stays responsive by being resilient, elastic, and message-driven.

**How it works:**

- You can write a `Flux.map` pipeline and still be a single blocking process. That is reactive programming without the system properties.
- You can pass asynchronous messages between services and not use `Flux` at all. That can still be a reactive system.
- Reactive Streams is a library-level contract used inside the programming style. The manifesto does not specify Publisher or Subscriber.
- The useful overlap is the boundary: non-blocking demand so one part cannot force another to buffer without limit.

**When to use:**

Talk about operators, `Mono`, and `Flux` when the question is programming. Talk about failure, load, and asynchronous messages when the question is the system.

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
<summary>How can you ensure that Reactive Systems are resilient?</summary>

### How can you ensure that Reactive Systems are resilient?

**Definition:**

The manifesto’s resilience means the system stays responsive when a part fails. In a stream that means the failure is a terminal signal you replace or isolate, not a stuck thread.

**How it works:**

- Reactive Streams ends a subscription with `onError` or `onComplete`. After `onError` that subscriber receives nothing else.
- Recovery is a library operator that substitutes another publisher before the error reaches the edge. The ReactiveX catch page is that substitution.
- A bounded queue is part of staying up: the readme says an unbounded buffer under a fast source defeats the asynchronous design.
- Isolate blocking recovery work on `boundedElastic`, as the Reactor FAQ does, so the failure path does not block the event loop.

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
<summary>Describe how you would implement backpressure across microservices.</summary>

### Describe how you would implement backpressure across microservices.

**Definition:**

Across a process boundary, backpressure is a demand signal so the sender does not outrun the receiver. The Reactive Streams readme requires that signal to be asynchronous and the queue between them to be bounded.

**How it works:**

- The subscriber requests `n`. The publisher emits at most the outstanding demand.
- If the protocol is not Reactive Streams, apply the RxJava adapters the backpressure wiki names: `onBackpressureBuffer` or `onBackpressureDrop`, or shrink the stream with `debounce` or `sample` first.
- A synchronous call that blocks until the remote accepts more is the failure mode the readme warns about: it negates asynchronous processing.
- One response is a `Mono`. A streamed response is a `Flux` whose demand crosses the same boundary.

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
<summary>In a microservices architecture, how do Reactive Streams help manage resources?</summary>

### In a microservices architecture, how do Reactive Streams help manage resources?

**Definition:**

Reactive Streams keeps the queue between a fast producer and a slow consumer bounded. That bound is the resource limit: memory for in-flight elements stays finite.

**How it works:**

- The site states the problem as live data whose volume is not known in advance.
- Demand is the control knob. Without it, the RxJava wiki’s `zip` of two infinite sources would buffer the faster one without limit.
- Non-blocking demand means the receiver can say “no more for now” without occupying a thread to do it.
- JDK Flow is the same contract, so a service can speak it without taking a dependency on one library’s `Flux`.

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
<summary>Discuss the advantages of using Reactive Programming in microservices communication.</summary>

### Discuss the advantages of using Reactive Programming in microservices communication.

**Definition:**

The gain is composition of asynchronous calls without blocking a thread per call, plus a demand protocol so one service cannot force another to hold an unbounded buffer.

**How it works:**

- Operators such as `map` and `zip` describe the combination. Work starts at subscribe.
- `zip` waits for an item from each source and, in RxJava’s implementation, requests only enough to refill a small buffer.
- A `Mono` models one response. Failures stay on the same sequence as values, ending in `onError`.
- The manifesto’s loose coupling follows when services exchange messages instead of sharing a thread.

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
<summary>What are some challenges you might face when designing Reactive Systems?</summary>

### What are some challenges you might face when designing Reactive Systems?

**Definition:**

The hard part is the boundary the Reactive Streams readme names: stay asynchronous, keep queues bounded, and still remain responsive when a part fails.

**How it works:**

- A blocking call inside `map` occupies the thread `publishOn` selected. The FAQ’s fix is to isolate it with `subscribeOn` on `boundedElastic`.
- An unbounded `onBackpressureBuffer` only moves the overflow into memory. The wiki notes a capacity-limited buffer terminates with an error when it is overrun.
- Context is not a `ThreadLocal`. After `publishOn`, thread-local logging state is gone unless it was put in Reactor context.
- The specification does not include operators. Each library’s `flatMap` can interleave inners; assuming order is a design bug.

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
<summary>How does a message-driven system relate to Reactive Microservices?</summary>

### How does a message-driven system relate to Reactive Microservices?

**Definition:**

Message-driven is one of the four manifesto traits: components communicate with asynchronous messages. A reactive microservice uses that instead of a shared thread or a synchronous call that holds the caller.

**How it works:**

- A message can be a stream element. Reactive Streams then adds demand so the mailbox cannot grow without bound.
- The signal protocol is `onSubscribe`, zero or more `onNext`, then `onError` or `onComplete`.
- Elastic and resilient behavior in the manifesto depend on this decoupling: a slow consumer slows the producer by demand, and a failed consumer ends in `onError` rather than a stuck call.
- Operators inside a process are not the messages between processes.

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
<summary>What are some of the most common Reactive libraries and frameworks for microservices?</summary>

### What are some of the most common Reactive libraries and frameworks for microservices?

**Definition:**

On the JVM the common pieces are the Reactive Streams API itself, RxJava, and Project Reactor. JDK `java.util.concurrent.Flow` is the same four roles. RxJS is the JavaScript member of ReactiveX.

**How it works:**

- RxJava implements observable sequences, schedulers, and subjects. Its backpressure story for sources that ignore demand is `onBackpressureBuffer` and `onBackpressureDrop`.
- Reactor implements `Mono` and `Flux`, `publishOn` / `subscribeOn`, and `Context`.
- The TCK is how a library shows it actually follows the spec, not only the type names.
- The manifesto is not a library. It is the system-level description these libraries are often used to satisfy.

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
<summary>Explain the concept of higher-order Observables.</summary>

### Explain the concept of higher-order Observables.

**Definition:**

A higher-order observable emits other observables. `flatMap` and Switch both take that shape: each outer item becomes an inner sequence that must be flattened.

**How it works:**

- `flatMap` merges inners, so their items may interleave. The ReactiveX flatMap page states that an inner error fails the outer immediately.
- Switch, which RxJS calls `switchMap`, unsubscribes from the previous inner when a new one arrives and emits only the latest inner.
- The type is nested only until the flattening operator. Downstream sees a single sequence of items.
- Leaving the nested observable unflattened emits observables as items, which is rarely what a subscriber wants.

**When to use:**

Merge with `flatMap` when every inner result counts. Switch when only the latest inner counts.

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
<summary>How do you implement custom operators in Rx libraries?</summary>

### How do you implement custom operators in Rx libraries?

**Definition:**

A custom operator is a function from one Publisher to another that still obeys the Reactive Streams signal order. Reactor’s `transform` is the supported way to reuse such a function without writing a new Subscriber.

**How it works:**

- `transform` applies the function once at assembly. Every subscriber shares that chain.
- `transformDeferred` applies it per subscriber.
- The JVM readme’s protocol still holds inside the function: `onSubscribe`, then `onNext` zero or more times, then `onError` or `onComplete`.
- Do not emit `onNext` after a terminal signal. That breaks the protocol the TCK checks.

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
<summary>What are the implications of unicast vs multicast Observables?</summary>

### What are the implications of unicast vs multicast Observables?

**Definition:**

A unicast cold observable runs its sequence separately for each subscriber. A multicast hot observable, typically a Subject, shares one sequence among subscribers.

**How it works:**

- The RxJava backpressure page: a cold observable can start when the observer wants and at the observer’s rate, without changing the sequence. An iterable converted with `from` is the example.
- A Subject subscribed to a cold source starts it and then reemits to many observers, which is the hot view the subject page describes.
- `PublishSubject` gives a late subscriber only future items. `BehaviorSubject` also gives the latest item. `ReplaySubject` gives earlier items.
- Multicast plus a source that ignores demand is how an unbounded buffer appears. Demand or `onBackpressureBuffer` / `onBackpressureDrop` has to sit on that shared sequence.

**When to use:**

Stay cold when each subscriber needs a fresh run, such as one HTTP call each. Go hot when many subscribers must see the same live events.

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
<summary>How do you use `combineLatest`, `withLatestFrom`, and `zip` operators for combining multiple streams?</summary>

### How do you use `combineLatest`, `withLatestFrom`, and `zip` operators for combining multiple streams?

**Definition:**

`zip` pairs items by index and emits only when each source has a new unpaired item. `combineLatest` emits when any source emits, using the latest item from each. `withLatestFrom` emits only when the primary source emits, attaching the latest value from the other.

**How it works:**

- The combineLatest page says it is similar to zip, except zip waits for a previously unused item from every source.
- The same page is where ReactiveX discusses `withLatestFrom`: the output is driven by one source, not by either source.
- RxJava’s `zip` keeps a small buffer and requests one more item per source after each combined emission, which is the backpressure wiki’s example.
- If one source is much faster, `zip` holds its extra items. That is the backlog the wiki warns about.

**When to use:**

Use `zip` to pair corresponding items. Use `combineLatest` when any update should recompute from the latest of each. Use `withLatestFrom` when only one stream is the trigger.

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
<summary>Discuss the concept of side-effects in Reactive Streams and how to avoid them.</summary>

### Discuss the concept of side-effects in Reactive Streams and how to avoid them.

**Definition:**

A side effect is work that is not the next item, such as writing a shared variable. The sequence contract is easier to keep when each stage is a pure function of its input item and the subscriber is the only place that performs I/O.

**How it works:**

- `map` should return the next value. Mutating a list captured from outside makes the result depend on who subscribed and on which thread `publishOn` picked.
- The Reactor FAQ says `publishOn` moves later stages to another thread, so a `ThreadLocal` written upstream is not visible downstream. Put that value in `Context` instead.
- An error handler that swallows `onError` breaks the protocol: the subscriber never sees the terminal signal.
- Logging that rethrows, or that only records and lets the error continue, is the side effect that stays honest.

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
<summary>Explain the difference between `takeUntil` and `takeWhile` operators.</summary>

### Explain the difference between `takeUntil` and `takeWhile` operators.

**Definition:**

`takeWhile` emits items while a predicate on the item is true and stops when it is false. `takeUntil` emits items from the source until a second observable emits, then stops.

**How it works:**

- The takeWhile page filters by the item itself. The decision does not need another stream.
- The takeUntil page watches a notifier. The source items are not what trips the stop, the other observable is.
- Both complete the output when they stop. They do not emit an error for a normal stop.
- Neither requests an unbounded remainder after the stop if the source honors demand.

**When to use:**

Use `takeWhile` when the items themselves say when to stop. Use `takeUntil` when a different stream, such as a click or a shutdown signal, says when to stop.

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
<summary>How does concurrency control work in Reactive Programming?</summary>

### How does concurrency control work in Reactive Programming?

**Definition:**

Concurrency is explicit. `subscribeOn` chooses the subscription thread, `observeOn` or Reactor’s `publishOn` chooses where later notifications run, and Reactive Streams demand bounds how many items may be outstanding.

**How it works:**

- ReactiveX: by default the chain stays on the subscribing thread. `subscribeOn` moves the source. `observeOn` moves notifications downstream of that call.
- Reactor: the closest downstream `subscribeOn` is the one that schedules subscription. `publishOn` affects operators after it until the next `publishOn`.
- A Subject still needs serialization if `onNext` arrives from several threads. The RxJava wiki’s `SerializedSubject` is that queue.
- Bounded demand is the other half. A scheduler without `request` can still grow a queue.

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
<summary>What is the role of virtual time in testing Reactive applications?</summary>

### What is the role of virtual time in testing Reactive applications?

**Definition:**

Virtual time advances a test scheduler’s clock without waiting on the wall clock. RxJava’s `TestScheduler` is the tool the 2.0 notes show for an operator that delays, such as a one-second `delay`.

**How it works:**

- Create the `TestScheduler`, pass it to the time-based operator, and subscribe with a test observer.
- `advanceTimeBy` moves the virtual clock. The 2.0 example asserts emptiness at 999 ms and the value at 1000 ms.
- Reactor’s `StepVerifier` compares signals after subscribe. Time-based Reactor tests use the verifier’s virtual-time schedule rather than `Thread.sleep`.
- A real scheduler in a unit test makes the test depend on load. The virtual clock does not.

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
<summary>How do you manage state in Reactive Programming?</summary>

### How do you manage state in Reactive Programming?

**Definition:**

Carry state in the sequence. `scan` emits each next accumulator. A `BehaviorSubject` holds the latest value for late subscribers. Reactor `Context` holds subscription-scoped data that is not an item.

**How it works:**

- `scan` folds items with a seed and emits every intermediate result. A sum of ticks is the scan page’s shape.
- `BehaviorSubject` replays the current item on subscribe, then later items. It is shared state, so serialize it if several threads call `onNext`.
- `contextWrite` stores a correlation id for the subscription. It is not a replacement for `scan` of business data.
- A mutable field closed over by `map` races once `publishOn` moves the chain.

**When to use:**

Use `scan` for state derived from items. Use a `BehaviorSubject` for a shared current value. Use `Context` for ambient subscription data such as a request id.

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
<summary>Describe strategies to handle out-of-order messages in a Reactive system.</summary>

### Describe strategies to handle out-of-order messages in a Reactive system.

**Definition:**

Streams preserve the order of one publisher’s `onNext` calls. Order across merged publishers is not guaranteed. Pick the combining operator that matches the order you actually need.

**How it works:**

- `zip` emits in lockstep: the nth output uses the nth item of each source. A faster source waits.
- `flatMap` merges inners as they arrive, so a slow first inner can be overtaken.
- Switch keeps only the latest inner and drops the previous one’s remainder, which is the ordered choice when older work is stale.
- A sequence number inside the item is application data. `scan` can remember the last accepted number and `takeWhile` or `filter` can drop a regression. The specification does not reorder for you.

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
<summary>List several popular Reactive Programming frameworks and their primary use cases.</summary>

### List several popular Reactive Programming frameworks and their primary use cases.

**Definition:**

RxJava and RxJS implement ReactiveX observable sequences. Project Reactor implements `Mono` and `Flux` on the Reactive Streams contract. JDK Flow is that same contract in the platform.

**How it works:**

- RxJava is the JVM observable library: schedulers, subjects, and operators such as `debounce` and `zip`.
- RxJS is the JavaScript member of the same operator set, including `from` and `scan`.
- Reactor is the library whose `publishOn`, `subscribeOn`, `Context`, and `StepVerifier` the reference guide documents.
- The Reactive Streams TCK is what a further library must pass if it wants to interoperate, rather than only borrowing the names.

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
<summary>How does Akka Streams fit into the Reactive Programming paradigm?</summary>

### How does Akka Streams fit into the Reactive Programming paradigm?

**Definition:**

Akka Streams is a stream implementation that can sit on the Reactive Streams contract: stages exchange elements asynchronously and the receiver limits demand so buffers stay bounded. Akka actors, by contrast, are the long-lived message processors the Scala concurrency chapter names.

**How it works:**

- The Reactive Streams readme’s four types are the interoperability edge. A stage that is a Processor is both subscriber and publisher.
- Demand via `request` is what keeps a fast stage from forcing an unbounded queue on a slow one.
- Operators that transform elements are library features. The specification says it does not cover transformation.
- An actor mailbox without demand can grow. A Reactive Streams boundary is the piece that adds the bound.

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
<summary>Discuss the integration of Reactive Programming libraries with traditional web frameworks.</summary>

### Discuss the integration of Reactive Programming libraries with traditional web frameworks.

**Definition:**

A traditional framework expects a thread to block until the result is ready. A reactive library expects a `Mono` or `Flux` and a subscribe. The integration is an adapter at the edge, not a rewrite of every operator.

**How it works:**

- Return a `Mono` for one response body and a `Flux` for a stream. The caller subscribes; the chain does not run before that.
- If the existing code blocks, wrap it as the Reactor FAQ shows: `Mono.fromCallable(...).subscribeOn(Schedulers.boundedElastic())`.
- Do not call `block` on the event-loop thread. That recreates the thread-per-request cost the scheduler split exists to avoid.
- Errors stay on `onError`. The edge adapter maps that terminal signal to the framework’s error response.

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
<summary>What are some Reactive Programming utilities provided by the Spring WebFlux framework?</summary>

### What are some Reactive Programming utilities provided by the Spring WebFlux framework?

**Definition:**

Spring WebFlux speaks Reactor types at the HTTP edge: a handler returns `Mono` or `Flux`, and the framework subscribes. The reactive utilities are therefore Reactor’s, not a second operator set.

**How it works:**

- One JSON body is a `Mono`. A streamed body is a `Flux`. Both honor Reactive Streams demand.
- `publishOn` and `subscribeOn` are how a handler leaves the event loop for CPU or blocking work. The Reactor schedulers page is the contract.
- Request-scoped data that must survive a thread hop goes in Reactor `Context`, not in a `ThreadLocal`, because `publishOn` changes threads.
- Tests use `StepVerifier` on the returned publisher rather than a servlet thread.

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
<summary>How do you debug a complex Reactive streams pipeline?</summary>

### How do you debug a complex Reactive streams pipeline?

**Definition:**

Debug the signal sequence, not a stack that crosses threads. `StepVerifier` states the expected `onNext` values and the terminal signal. A mismatch throws `AssertionError` at the first wrong signal.

**How it works:**

- Shrink the chain until the verifier passes, then add one operator back. `transform` makes that slice a named function.
- If the failure is “nothing happens”, nothing subscribed. Reactor’s schedulers page: nothing happens until subscribe.
- If the failure is a blocked event loop, a blocking call sits in `map`. Move it behind `subscribeOn(Schedulers.boundedElastic())`.
- If logs lose the request id after `publishOn`, read it from `Context`, which follows the subscription rather than the thread.

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
<summary>What are the best tools for monitoring and troubleshooting Reactive applications?</summary>

### What are the best tools for monitoring and troubleshooting Reactive applications?

**Definition:**

Troubleshoot with the signal tools the libraries document: Reactor’s `StepVerifier` for expected items and context, and RxJava’s `TestScheduler` for time. Production monitoring still watches the bounded queues the Reactive Streams model exists to protect.

**How it works:**

- `StepVerifier` fails on the first unexpected signal, including an unexpected `onError`.
- `expectAccessibleContext` checks that a correlation id survived the chain.
- `TestScheduler.advanceTimeBy` shows whether a `delay` or `debounce` fired without sleeping.
- A queue that grows without a matching `request` is the overflow the Reactive Streams readme says must not happen. Alert on that depth, not only on thread count.

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
<summary>Explain how Reactive Programming can be used with Kafka Streams.</summary>

### Explain how Reactive Programming can be used with Kafka Streams.

**Definition:**

Kafka Streams processes a log of records with its own topology API. Reactive Streams solves a different, adjacent problem: crossing an asynchronous boundary with bounded demand. Use Kafka Streams for the log topology, and a `Flux` only at an edge that must speak Publisher and Subscriber.

**How it works:**

- Do not rename a Kafka Streams operation to `flatMap`. The ReactiveX operator is not the Kafka Streams method.
- A consumer that is a Reactive Streams subscriber requests `n` and stops when demand is exhausted, which is how the readme keeps the queue bounded.
- Order inside one partition is the log’s order. `zip` is for pairing two reactive sources, not for reordering a partition.
- Backpressure across the network still has to be asynchronous. A consumer that blocks the poll thread to apply demand negates that, which the readme warns about.

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
<summary>How do you handle load balancing in a Reactive microservices setup?</summary>

### How do you handle load balancing in a Reactive microservices setup?

**Definition:**

Elastic in the manifesto means staying responsive as load changes. In a stream, load is balanced by demand: a slow instance requests less, so the sender does not keep filling an unbounded queue toward it.

**How it works:**

- Reactive Streams `request(n)` is the per-connection brake. It is not a substitute for a router, and a router is not a substitute for the brake.
- `onBackpressureDrop` sheds load by discarding. `onBackpressureBuffer` keeps a bounded backlog and errors when the cap is hit, which the RxJava wiki describes.
- `debounce` and `sample` shrink a hot stream before it is forwarded.
- More instances help only if each one still bounds its own queue. Extra threads without demand just move the overflow.

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
<summary>Discuss how Reactive Programming influences event-driven architecture.</summary>

### Discuss how Reactive Programming influences event-driven architecture.

**Definition:**

An event-driven architecture already passes asynchronous messages. Reactive programming adds a subscriber that can compose those events with operators and, on the JVM, can limit demand.

**How it works:**

- The manifesto’s message-driven trait is the architecture. `map`, `scan`, and `filter`-style narrowing are the programming.
- `scan` turns a stream of events into a running state. That state is emitted, not stored in a mutable field closed over by the chain.
- A hot Subject is the in-process bus. Late subscribers see only what that Subject variant promises: future items, the latest item, or a replay.
- Between processes, the Reactive Streams bound still applies. An event log does not by itself implement `request`.

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
<summary>What is RSocket, and how does it support Reactive Programming patterns?</summary>

### What is RSocket, and how does it support Reactive Programming patterns?

**Definition:**

RSocket is a protocol for reactive streams across a network. It carries the Reactive Streams idea of asynchronous demand so one peer cannot force the other to buffer without bound.

**How it works:**

- The Reactive Streams readme already requires backpressure signals to be asynchronous across hosts, not only across thread pools.
- A request-response interaction is one item, the `Mono` shape. A request-stream interaction is many items, the `Flux` shape.
- Application operators still sit in Reactor or RxJava. The protocol does not replace `map` or `zip`.
- A blocking adapter at either end removes the benefit the readme describes.

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
<summary>What are some common design patterns used in Reactive Programming?</summary>

### What are some common design patterns used in Reactive Programming?

**Definition:**

The patterns that show up in the docs are a cold source per subscriber, a Subject as a hot bridge, operators as the pipeline, and `request` as the bound on the queue.

**How it works:**

- `from` on a collection is the cold pattern: each subscribe walks the data again.
- A Subject is the hot pattern. `BehaviorSubject` keeps the latest item; `PublishSubject` does not.
- `map` and `flatMap` are the pipeline. `transform` names a reusable slice of it.
- Reactive Streams `request` is the pattern that keeps the asynchronous boundary bounded.

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
<summary>How do you apply commonly known Gang of Four patterns in Reactive Programming?</summary>

### How do you apply commonly known Gang of Four patterns in Reactive Programming?

**Definition:**

The Observer pattern is the subscriber. A pipeline of operators is a chain of wrappers around a source. A Subject is a proxy that is both observer and observable.

**How it works:**

- ReactiveX describes a Subject as a bridge or proxy: it can subscribe, and it can reemit.
- `map` wraps a source and returns a new observable. The source is unchanged.
- Strategy is the function you pass: the same `scan` shape with a different accumulator.
- The Gang of Four observer does not include demand. Reactive Streams adds `request` so the observer can slow the subject down.

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
<summary>What are some best practices for error handling in Reactive streams?</summary>

### What are some best practices for error handling in Reactive streams?

**Definition:**

Treat `onError` as terminal. Recover by substituting another publisher, or let the subscriber see the error. Do not emit further `onNext` after the error.

**How it works:**

- The JVM readme’s protocol is `onSubscribe`, then `onNext` repeated, then exactly one of `onError` or `onComplete`.
- The ReactiveX catch operator replaces a failing sequence with another. That is recovery.
- An empty error callback hides the terminal signal. The subscriber then waits forever.
- Retry resubscribes a cold source. It does not rewind a hot Subject.

**Examples:**

```java
source.onErrorResume(error -> Mono.just("fallback"));
```

**Sources:**

- https://github.com/reactive-streams/reactive-streams-jvm/blob/master/README.md
- https://reactivex.io/documentation/operators/catch.html
</details>

<details>
<summary>Describe a scenario where Reactive Programming is not the ideal approach. Why?</summary>

### Describe a scenario where Reactive Programming is not the ideal approach. Why?

**Definition:**

A short, finite, CPU-bound transformation of data you already hold does not need a subscriber, a scheduler, or demand. A direct call returns the value on the same thread.

**How it works:**

- The Reactive Streams problem statement is live data whose volume is not known, across an asynchronous boundary. A `List` in memory is not that problem.
- Wrapping that list in `Flux.just` and blocking on it adds a protocol without a queue to bound.
- A single blocking JDBC call hidden inside `map` on the event loop is worse than an ordinary call. The FAQ only helps if you actually move it to `boundedElastic`.
- Operators pay off when there are many values, cancellation, or a thread hop. Otherwise they obscure a straight-line function.

**When to use:**

Stay imperative for one finite in-memory pass. Use a publisher when the data arrives over time, the work is asynchronous, or the consumer must bound the rate.

**Examples:**

```java
List<Integer> doubled = List.of(1, 2, 3).stream().map(n -> n * 2).toList();
```

**Sources:**

- https://www.reactive-streams.org/
- https://projectreactor.io/docs/core/release/reference/faq.html
</details>

<details>
<summary>Explain the use of backpressure strategies, like buffering and dropping.</summary>

### Explain the use of backpressure strategies, like buffering and dropping.

**Definition:**

When a source ignores `request`, RxJava offers `onBackpressureBuffer` and `onBackpressureDrop`. Buffering keeps items and emits them as the subscriber requests. Dropping discards items the subscriber is not ready for.

**How it works:**

- The backpressure wiki introduces both operators as the way to force a simple policy onto a source that was not written for reactive pull.
- An experimental bounded `onBackpressureBuffer` terminates with an error if the buffer is overrun. An unbounded buffer can exhaust memory; the wiki’s `zip` example is that failure.
- `debounce`, `sample`, and `throttleFirst` shrink the stream before a buffer is required.
- `buffer` groups items into lists. That is a different operator: it changes the item type, it does not implement `request`.

**When to use:**

Buffer when every item must eventually be delivered and the burst is bounded. Drop when a stale item is useless, such as a sensor sample replaced by a newer one.

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
<summary>How can you make your Reactive streams more composable and reusable?</summary>

### How can you make your Reactive streams more composable and reusable?

**Definition:**

Return a `Flux` or `Mono` from a method and reuse a slice with `transform`. The Reactor guide says `transform` applies one function at assembly time, equivalent to writing the operators inline.

**How it works:**

- A method that subscribes inside itself cannot be composed. The caller must be the one that subscribes.
- `transform` shares one assembled chain. `transformDeferred` rebuilds it per subscriber when the chain depends on that subscription.
- Keep the function pure: `map` returns a value. Side effects belong at the subscriber or in an explicit logging step that does not swallow errors.
- Name the function after the policy, such as `withoutOrange`, not after the call site.

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
<summary>What are some best practices for scaling Reactive applications?</summary>

### What are some best practices for scaling Reactive applications?

**Definition:**

Scale by bounding work, not by adding unbounded buffers. The manifesto’s elasticity is staying responsive under load; Reactive Streams does that with demand, and Reactor does it by keeping blocking work off the event loop.

**How it works:**

- `request(n)` caps in-flight elements per subscription.
- Blocking calls go to `Schedulers.boundedElastic()`, immediately after the source, as the FAQ shows.
- CPU work can move with `publishOn`. The placement matters: it affects operators after it.
- Drop or sample a hot stream that subscribers cannot keep up with, instead of buffering it without a cap.

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
<summary>How do you manage configuration changes in a Reactive system?</summary>

### How do you manage configuration changes in a Reactive system?

**Definition:**

Treat a configuration change as an item on a stream. `scan` folds it into the current settings, or a `BehaviorSubject` holds the latest settings for whoever subscribes next.

**How it works:**

- `BehaviorSubject` emits the current value on subscribe, which is the contract on the ReactiveX subject page.
- Downstream work that must restart on a change uses Switch: the new configuration unsubscribes the previous inner.
- Do not stash the value in a mutable field read from `map` after `publishOn`. The thread will not be the one that wrote it.
- Subscription-scoped values that are not the configuration stream belong in Reactor `Context`.

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
<summary>Discuss the CQRS pattern in the context of Reactive Systems.</summary>

### Discuss the CQRS pattern in the context of Reactive Systems.

**Definition:**

CQRS splits commands that change state from queries that read it. In a reactive system both sides are message-driven: a command is an event you fold with `scan`, and a query reads the latest value from a `BehaviorSubject` or a store fed by that fold.

**How it works:**

- The manifesto’s message-driven trait is why the write path does not share a thread with the read path.
- `scan` emits each next state. Subscribers of the query side observe those emissions instead of locking a model.
- A failure on the command stream is `onError` and is terminal for that subscription. It does not half-apply inside `map`.
- Demand still bounds how fast commands may be applied. CQRS does not remove backpressure.

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
<summary>What are the principles of the Reactive Manifesto?</summary>

### What are the principles of the Reactive Manifesto?

**Definition:**

The manifesto names four properties: responsive, resilient, elastic, and message-driven. Systems with those properties are, in its words, more flexible, loosely coupled, and scalable.

**How it works:**

- Responsive: the system responds in a timely fashion.
- Resilient: it stays responsive in the face of failure.
- Elastic: it stays responsive under a changing workload.
- Message-driven: the parts rely on asynchronous message passing. The Reactive Streams readme points at the manifesto to explain why that backpressure signal must itself be asynchronous.

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
<summary>How is Reactive Programming implemented in languages like JavaScript, Scala, or Java?</summary>

### How is Reactive Programming implemented in languages like JavaScript, Scala, or Java?

**Definition:**

The idea is one observable sequence. JavaScript uses RxJS, Java uses RxJava or Reactor, and the ReactiveX pages document the same operators for each. On the JVM, Reactive Streams plus JDK Flow is the shared demand protocol.

**How it works:**

- RxJS `from` and RxJava `fromIterable` are the From operator: emit each element of a collection, then complete.
- `debounce`, `zip`, `scan`, and Subject variants keep their contracts across those libraries.
- Scala can call the same JVM libraries. A Scala `Future` is one result, not a Reactive Streams publisher.
- JDK 9 `java.util.concurrent.Flow` matches the four Reactive Streams types, so Java code need not import a third-party publisher to speak the protocol.

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
<summary>Compare the Reactor pattern in Java with Reactive Extensions in .NET.</summary>

### Compare the Reactor pattern in Java with Reactive Extensions in .NET.

**Definition:**

The classic Reactor pattern demultiplexes readiness events onto a small set of threads. Reactive Extensions, and Project Reactor on the JVM, compose the values those events carry as sequences with operators and, on the JVM, with demand.

**How it works:**

- A selector loop is the I/O pattern. It does not define `map` or `zip`.
- ReactiveX documents those operators once and implements them in Rx.NET as well as RxJava. The names on the operator pages are the comparison.
- Project Reactor’s `publishOn` moves the sequence onto a scheduler after a stage. That is composition, not the selector loop itself.
- Both still need a bound. Reactive Streams `request` is the JVM form of “do not deliver more than the consumer accepted.”

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
<summary>How do you use Reactive Programming in modern front-end frameworks like React or Angular?</summary>

### How do you use Reactive Programming in modern front-end frameworks like React or Angular?

**Definition:**

In the browser the sequence is usually RxJS. Angular consumes observables directly. React keeps its own state: you subscribe at the edge and store the latest item, often the value a `BehaviorSubject` already holds.

**How it works:**

- `from` turns an array into an observable. `fromEvent` is not required to listen if you already have a callback that calls `next` on a Subject.
- `scan` folds events into the state you render. `debounce` waits for a quiet gap, which the debounce page defines.
- Switch drops the previous inner request when the user types again. That is the ReactiveX Switch operator, spelled `switchMap` in RxJS.
- Unsubscribe when the view goes away. A live subscription is the leak. A Promise cannot express that cancel.

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
<summary>What are some considerations for using Reactive Programming in a functional programming language?</summary>

### What are some considerations for using Reactive Programming in a functional programming language?

**Definition:**

Operators are functions from a sequence to a sequence. They stay referentially honest when `map` does not write shared state and the only effects sit at subscribe.

**How it works:**

- `map` and `scan` take a function and return a new sequence. The source is not mutated.
- `flatMap` is the flatten after a function that itself returns a sequence. Switch is the variant that keeps only the latest inner.
- A lazy cold source fits a functional style: describing `from(array)` does not run it. Subscribe is the edge.
- `publishOn` changes the thread, so a closed-over mutable variable is a race. Pass the value in the item or in Reactor `Context`.

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
<summary>How does the async-await pattern in modern languages relate to Reactive Programming?</summary>

### How does the async-await pattern in modern languages relate to Reactive Programming?

**Definition:**

`async`/`await` waits for one future value and then continues. A publisher can emit many values, starts at subscribe, and can be bounded by demand. Await is the one-item case, closest to a `Mono`, not to a `Flux`.

**How it works:**

- A Promise or a Scala `Future` settles once. `Flux` is the sequence of many settlements.
- Awaiting inside a loop pulls. A subscriber is pushed to, and on the JVM it pulls only via `request`.
- You can adapt one item: the first element of a `Mono` is what await wants. A multi-value `Flux` does not fit a single await.
- Cancellation of an await and unsubscribe of a stream are different. Unsubscribing stops demand.

**When to use:**

Use async/await for one result. Use a publisher when there is a sequence, a cancelable subscription, or a need to bound the rate.

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
<summary>Provide examples of real-world use cases where Reactive Programming is applied.</summary>

### Provide examples of real-world use cases where Reactive Programming is applied.

**Definition:**

The documented fits are a burst of UI events thinned by `debounce`, a running total via `scan`, and a service call that must not block, wrapped as a `Mono` on `boundedElastic`.

**How it works:**

- The debounce page exists for items that arrive in bursts. A search box is that shape.
- `zip` combines two sources in lockstep, and RxJava requests only enough to refill a small buffer.
- A WebFlux-style handler returns `Mono` or `Flux` and lets the framework subscribe.
- A finite in-memory list does not need any of this.

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
<summary>Analyze the impact of Reactive Programming on cloud-native application design.</summary>

### Analyze the impact of Reactive Programming on cloud-native application design.

**Definition:**

Cloud-native design already scales by adding instances. Reactive programming changes the instance: fewer threads blocked per call, and a demand signal so a peer cannot force an unbounded buffer.

**How it works:**

- The manifesto calls this elasticity and loose coupling through asynchronous messages.
- Reactor’s `boundedElastic` is where the remaining blocking cloud SDK calls go, off the event loop.
- Reactive Streams across hosts is the network form of the same bound. RSocket is one protocol that carries that idea.
- Autoscaling does not fix a handler that `block`s the event loop. The FAQ’s wrapper does.

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
<summary>Discuss how large-scale streaming platforms benefit from Reactive Programming principles.</summary>

### Discuss how large-scale streaming platforms benefit from Reactive Programming principles.

**Definition:**

A streaming platform’s problem is the one Reactive Streams states: live data of unknown volume must cross a boundary without an arbitrary buffer. Demand, or an explicit drop or sample, is the benefit.

**How it works:**

- `request(n)` keeps the in-flight set finite.
- `onBackpressureDrop` and `sample` shed a burst when the consumer is behind. The backpressure wiki shows both families.
- `zip` of two infinite sources without demand is the counterexample: the faster source’s buffer grows forever.
- The platform’s own processor, such as Kafka Streams, keeps its API. The reactive principle is the bound, not a renamed operator.

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
<summary>How can Reactive Programming enhance the performance of IoT systems?</summary>

### How can Reactive Programming enhance the performance of IoT systems?

**Definition:**

Sensors emit faster than a store can write. `debounce`, `sample`, or `onBackpressureDrop` keep the latest useful reading and discard the rest, which is the backpressure wiki’s remedy for a bursty source.

**How it works:**

- `sample` emits the most recent item on a period. `debounce` emits an item only after a quiet gap.
- Dropping is correct when an old reading is obsolete. Buffering is correct when every reading must be kept and the burst is capped.
- A `BehaviorSubject` holds the latest reading for a dashboard that subscribes late.
- The device link still needs an asynchronous demand signal if the buffer is not allowed to grow. That is the Reactive Streams goal across hosts.

**When to use:**

Sample or drop when only a recent reading matters. Buffer when the archive must not lose points and you have set a capacity that errors instead of growing forever.

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
<summary>Share a case study where the adoption of Reactive Programming solved a scalability issue.</summary>

### Share a case study where the adoption of Reactive Programming solved a scalability issue.

**Definition:**

The failure mode the docs describe is a thread blocked per request, or a `zip` buffer that grows without bound. The fix they describe is a `Mono` subscribed on `boundedElastic` for the blocking call, and `request` or `onBackpressureDrop` for the fast source.

**How it works:**

- Before: each call occupies a thread until the remote returns. Threads run out while most of them wait.
- After: the event loop accepts the call, `subscribeOn(boundedElastic)` runs the blocking client, and the `Mono` completes later.
- A second failure, a fast publisher and a slow consumer, is fixed by demand rather than by a larger heap.
- The manifesto’s names for the result are elastic and responsive. The code is the FAQ wrapper plus Reactive Streams `request`.

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
<summary>What are the trade-offs of using Reactive Programming over traditional imperative programming models?</summary>

### What are the trade-offs of using Reactive Programming over traditional imperative programming models?

**Definition:**

You gain composition of asynchronous sequences and a bound on in-flight data. You pay with a subscription model, harder stack traces, and no benefit on a finite in-memory loop.

**How it works:**

- A direct call returns a value. A publisher returns a description that runs at subscribe.
- Demand and `onBackpressureDrop` are tools an imperative loop does not have. They matter only when the source can outrun the consumer.
- `publishOn` splits the stack across threads. The Reactor FAQ says a `ThreadLocal` will not follow that hop.
- The manifesto’s responsiveness is the goal. The operator chain is justified only when asynchrony or a bound is real.

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
<summary>Discuss some of the complexities and pitfalls that come with adopting Reactive Programming.</summary>

### Discuss some of the complexities and pitfalls that come with adopting Reactive Programming.

**Definition:**

The usual pitfalls are subscribing too early, blocking inside `map`, assuming `flatMap` preserves order, and buffering a fast source without a cap.

**How it works:**

- Nothing runs until subscribe. A forgotten subscribe is a silent no-op, which the schedulers page states.
- `flatMap` merges inners and can interleave them. Switch keeps only the latest inner.
- An unbounded buffer is the `zip` failure in the RxJava backpressure wiki. Use a capped buffer, which errors when overrun, or drop.
- Context is not thread-local. After `publishOn`, read ambient data from Reactor `Context`.

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
<summary>How do you manage thread exhaustion in a Reactive application?</summary>

### How do you manage thread exhaustion in a Reactive application?

**Definition:**

Thread exhaustion here means blocking calls sitting on the few event-loop threads. Move those calls to `Schedulers.boundedElastic()` and keep the event loop for non-blocking stages.

**How it works:**

- The FAQ places `subscribeOn` immediately after the blocking source. Later operators come after that wrapper.
- `subscribeOn` does not subscribe by itself. It selects the scheduler used when subscribe happens.
- `publishOn` does not make a blocking `map` safe. The blocking call has to be inside the callable that `boundedElastic` runs.
- More event-loop threads do not fix a call that blocks each of them. The bound is the elastic pool, which is capped.

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
<summary>Address the learning curve associated with Reactive Programming. How do you get a team up to speed?</summary>

### Address the learning curve associated with Reactive Programming. How do you get a team up to speed?

**Definition:**

Teach the signal protocol first, then one library’s operators, and keep tests on `StepVerifier` so a wrong mental model fails as an assertion instead of a production hang.

**How it works:**

- The protocol is `onSubscribe`, `onNext` zero or more times, then `onError` or `onComplete`. The JVM readme is that page.
- Next: `map` versus `flatMap` versus Switch, and cold `from` versus a Subject. Those four pages cover most design arguments.
- `StepVerifier.expectNext(...).verifyComplete()` is the exercise. Virtual time replaces `Thread.sleep` for `delay`.
- Add schedulers only after the sequence is correct on one thread. `subscribeOn` and `publishOn` are the following lesson, from the Reactor schedulers page.

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
<summary>What are some known issues with debugging Reactive streams and how do you tackle them?</summary>

### What are some known issues with debugging Reactive streams and how do you tackle them?

**Definition:**

Stacks split at `publishOn`, nothing runs without subscribe, and an error can be swallowed before the subscriber. Tackle them with `StepVerifier`, by subscribing in the test, and by letting `onError` stay terminal.

**How it works:**

- The first mismatch in `StepVerifier` throws `AssertionError` and names the expected signal.
- A missing subscription is not a logic bug inside `map`. The schedulers page: nothing happens until subscribe.
- After `publishOn`, a `ThreadLocal` request id disappears. `expectAccessibleContext` checks the `Context` that replaced it.
- Time-based bugs use `TestScheduler.advanceTimeBy` so the test does not sleep.

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
<summary>Predict how Reactive Programming might evolve in the next few years.</summary>

### Predict how Reactive Programming might evolve in the next few years.

**Definition:**

The stable center is already specified: Reactive Streams and JDK Flow are the same demand protocol, and libraries keep their operators on top. Evolution is more runtimes speaking that protocol, not a new set of signal names.

**How it works:**

- The reactive-streams.org page says JDK Flow is semantically 1:1 and expects libraries to move to those types, with an adapter during the move.
- Operators such as `map`, `zip`, and `debounce` stay library features. The specification refuses to define them.
- Virtual threads reduce the pain of a blocking call, but they do not bound a queue. `request` remains the bound.
- The manifesto’s four properties do not change when the library does.

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
<summary>How does Reactive Programming interact with AI and machine learning workloads?</summary>

### How does Reactive Programming interact with AI and machine learning workloads?

**Definition:**

A model call is often blocking and one-shot. Wrap it as a `Mono` on `boundedElastic`. A stream of features is a `Flux` only when the caller must bound how many inferences are in flight.

**How it works:**

- The FAQ’s callable-plus-`subscribeOn` is the adapter for a blocking inference client.
- `flatMap` can run one inference per item. A cap on concurrency is a library parameter only where that library documents it; otherwise bound the source with `request`.
- Results that must stay in input order use `zip` with the inputs, not an unordered merge.
- Training a batch that already fits in memory does not need a publisher. The Reactive Streams problem is the live, unbounded feed.

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
<summary>Discuss the relevance of Reactive Programming in the age of serverless architectures.</summary>

### Discuss the relevance of Reactive Programming in the age of serverless architectures.

**Definition:**

A serverless invocation is usually one request and one response, which is a `Mono`, or simply async/await. A publisher earns its place when the function consumes or produces a sequence that must be bounded.

**How it works:**

- Cold start and a short lifetime argue against a long-lived Subject inside the function.
- A blocking SDK call still belongs on `boundedElastic` if the runtime is an event loop. On a runtime that gives you a thread per invocation, an ordinary call is enough.
- Streaming a response is a `Flux` with demand so the platform buffer stays finite.
- The manifesto’s elasticity here is the platform adding invocations. The code’s job is not to block the few threads it was given.

**When to use:**

Return one value with async/await or a `Mono` when the function does one thing. Use a `Flux` when the function streams and the consumer must apply demand.

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
<summary>What role might Reactive Programming play in the future of distributed systems and edge computing?</summary>

### What role might Reactive Programming play in the future of distributed systems and edge computing?

**Definition:**

Edge nodes are slow consumers of a fast core, or the reverse. The role of reactive programming is the asynchronous demand the Reactive Streams readme requires across hosts so the queue on the small node stays bounded.

**How it works:**

- A synchronous backpressure call would block the link and, the readme says, cancel the benefit of asynchrony.
- `sample` or `onBackpressureDrop` fits a link that can lose intermediate readings. A capped buffer fits a link that must not.
- Message-driven, in the manifesto, is how the edge stays decoupled from the core’s thread pool.
- Operators stay at the ends. The wire carries demand and items, which is the protocol, not `map`.

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
<summary>How are current advancements in hardware affecting Reactive Programming paradigms?</summary>

### How are current advancements in hardware affecting Reactive Programming paradigms?

**Definition:**

More cores make the asynchronous boundary inside one machine ordinary: the Reactive Streams readme already names multiple CPU cores as a reason to pass elements between thread pools with a bounded queue.

**How it works:**

- Faster cores do not remove the need for `request`. A faster producer overflows a slower consumer even more quickly.
- Reactor’s `publishOn(Schedulers.parallel())` is how CPU-bound stages use those cores. Blocking I/O still does not belong on that scheduler; the FAQ uses `boundedElastic`.
- Virtual threads make a blocking call cheaper. They do not implement demand. A `zip` of two infinite sources can still buffer without bound.
- The four manifesto properties stay about responsiveness under failure and load, not about a core count.

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

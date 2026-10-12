# Queues — Interview

Answers are filled.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Queues — Interview](./7_ds_queues_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Queues](https://devinterview.io/questions/data-structures-and-algorithms/queue-data-structure-interview-questions/).

### Queue Fundamentals

<details>
<summary>What is a Queue?</summary>

### What is a Queue?

**Definition:**

A queue adds items at the back and removes them from the front, so the oldest item leaves first.

**How it works:**

- Enqueue and dequeue are the two ends. Peek reads the front without removing it.
- A ring buffer or a linked list with a tail pointer makes both ends O(1).
- BFS, print spoolers, and task buffers are queues. A stack would serve the newest item instead.
- Empty dequeue is an error. A fixed buffer can also be full.

**Examples:**

Enqueue 1, then 2, then 3. Dequeue returns 1, then 2, then 3.
</details>

<details>
<summary>Explain the FIFO (First In, First Out) policy that characterizes a Queue.</summary>

### Explain the FIFO (First In, First Out) policy that characterizes a Queue.

**Definition:**

FIFO means the departure order matches the arrival order. Nothing jumps the line unless you switch to a different structure.

**How it works:**

- The front is the earliest remaining item. The back is where the next arrival lands.
- A stack is LIFO. A priority queue leaves by key, not by arrival.
- FIFO is the fairness rule for a single line: a request cannot starve behind later requests if the server keeps dequeuing.
- Two queues can still be unfair to each other if one of them is never served.
</details>

<details>
<summary>Name some Types of Queues.</summary>

### Name some Types of Queues.

**Definition:**

The family shares two ends and then relaxes which end you may use, or what 'next' means.

**How it works:**

- A simple queue is FIFO at two ends.
- A circular queue is that queue in a fixed array with wrap-around.
- A deque allows insert and delete at both ends.
- A priority queue ignores arrival order and extracts the best key. A double-ended priority queue exposes both extremes.
</details>

<details>
<summary>What is a Priority Queue and how does it differ from a standard Queue?</summary>

### What is a Priority Queue and how does it differ from a standard Queue?

**Definition:**

A priority queue extracts the item with the best key, even if it arrived last. A standard queue extracts the item that arrived first.

**How it works:**

- A binary heap implements insert and extract in O(log n). A FIFO queue does both in O(1) and has no key.
- Equal keys do not automatically keep FIFO order. Store a sequence number in the key if ties must be stable.
- Dijkstra and schedulers want the priority queue. A printer wants FIFO.
- You can build a FIFO queue from a priority queue by using arrival time as the key. You cannot build a priority queue from a FIFO queue without a scan.
</details>

<details>
<summary>When should I use a Stack or a Queue instead of Arrays/Lists?</summary>

### When should I use a Stack or a Queue instead of Arrays/Lists?

**Definition:**

Use a stack or a queue when the algorithm is defined by which end moves. Use an array when you need indexes.

**How it works:**

- DFS, undo, and bracket matching are stacks. BFS, buffers, and arrival order are queues.
- The restriction is the point. A list would let you remove from the middle and hide a bug.
- Underneath, both are often arrays or lists. The type documents the contract.
- If you need the i-th waiting item without removing the ones in front, you wanted a deque or an array, not a pure queue.
</details>

<details>
<summary>How do you reverse a Queue?</summary>

### How do you reverse a Queue?

**Definition:**

Drain the queue into a stack, then pop the stack back into the queue. The stack's reversal becomes the queue's new order.

**How it works:**

- Each dequeue pushes onto the stack. Each pop enqueues. Two passes, O(n) time, O(n) extra memory.
- A deque can reverse by taking from the front and pushing to the front of a second deque, or by swapping the meaning of the ends if you own both pointers.
- Recursion uses the call stack as that temporary stack and hits a depth of n.
- In-place reversal of a linked-list queue is the same pointer reverse as a list, then the head and tail swap roles.

**Examples:**

Queue front `1 2 3`. After the stack, pop order is `3 2 1`, and the queue's new front is 3.
</details>

<details>
<summary>Can a queue be implemented as a static data structure and if so, what are the limitations?</summary>

### Can a queue be implemented as a static data structure and if so, what are the limitations?

**Definition:**

Yes. A fixed array plus head and tail indexes is a static queue, and it cannot grow past that capacity.

**How it works:**

- Without wrap-around, each dequeue wastes a slot at the front and the queue 'fills' while the array still has holes.
- A circular layout reuses those slots. Full and empty must be distinguished by a size counter or by leaving one slot unused.
- Enqueue on a full queue fails or blocks. There is no amortized resize.
- The bound is an advantage when memory must be reserved up front, and a limitation when the burst can exceed the guess.
</details>

### Queue Operations

<details>
<summary>Write an algorithm to enqueue and dequeue an item from a Queue.</summary>

### Write an algorithm to enqueue and dequeue an item from a Queue.

**Definition:**

On a circular array, enqueue writes at tail and advances it. Dequeue reads at head and advances it. Both moves are modulo the capacity.

**How it works:**

- Reject enqueue when size equals capacity. Reject dequeue when size is 0.
- After a successful enqueue, size grows by one. After a successful dequeue, size shrinks by one.
- A linked list writes a node at the tail pointer and removes the head node. An empty list sets both pointers to null.
- Peek is the read without the advance.

**Examples:**

```text
enqueue(x):
  if size == capacity: fail
  buf[tail] = x
  tail = (tail + 1) % capacity
  size += 1
dequeue():
  if size == 0: fail
  x = buf[head]
  head = (head + 1) % capacity
  size -= 1
  return x
```
</details>

<details>
<summary>How to implement a Queue such that enqueue has O(1) and dequeue has O(n) complexity?</summary>

### How to implement a Queue such that enqueue has O(1) and dequeue has O(n) complexity?

**Definition:**

Append at the end of an array or a list for enqueue, and on dequeue shift every later item down by one, or walk a singly linked list that has no tail cache for the opposite cost.

**How it works:**

- Array: enqueue is an append, amortized O(1). Dequeue of index 0 memmoves the tail, O(n).
- That layout is simple and terrible for a hot queue. It is what you get from using a dynamic array as a queue without a head index.
- The circular head index removes the shift and makes dequeue O(1) too. The O(n) version is the one that refuses that index.
- A singly linked list with a head and no tail is the mirror: dequeue at the head is O(1) and enqueue walks to the end, O(n).
</details>

<details>
<summary>Discuss a scenario where dequeue must be prioritized over enqueue in terms of complexity.</summary>

### Discuss a scenario where dequeue must be prioritized over enqueue in terms of complexity.

**Definition:**

When consumers must pop the next job with a tight bound and producers can afford a slower push, make dequeue the O(1) end.

**How it works:**

- A linked list with the front at the head gives O(1) dequeue. Enqueue can walk to the tail, or you add a tail pointer and both become O(1).
- An interrupt handler or a UI frame that must take the next event should not shift an array.
- A batch producer that enqueues thousands and then sleeps can pay O(n) on the way in if the consumer's latency is the constraint. That is unusual once a tail pointer is free.
- If both ends are hot, stop choosing and use a ring or a list with both pointers.
</details>

<details>
<summary>Explain how you can efficiently track the Minimum or Maximum element in a Queue.</summary>

### Explain how you can efficiently track the Minimum or Maximum element in a Queue.

**Definition:**

Keep a deque of candidates in order, and drop a candidate when a newer item makes it useless or when it slides out of the window.

**How it works:**

- For a sliding-window maximum, the deque stores indexes in decreasing value. Before pushing the new index, pop the back while those values are smaller.
- Pop the front when its index has left the window. The front is then the maximum.
- Each index enters and leaves the deque once, so a window moving across n items is O(n).
- A plain queue plus a rescan on every dequeue is O(n) per step. The deque avoids that rescan. A heap also works and is O(log n) per step unless you lazily delete.

**Examples:**

Window of 3 over `[1, 3, 2, 4]`. After the third item the deque holds 3, then 2. The max is 3. When 4 arrives, 2 and 3 are smaller and leave, and the max is 4.
</details>

<details>
<summary>Discuss an algorithm to merge two or more Queues into one with efficient Dequeuing.</summary>

### Discuss an algorithm to merge two or more Queues into one with efficient Dequeuing.

**Definition:**

If the queues are FIFO and the merge should stay FIFO by original time, link them or drain them in arrival order. If each queue is sorted and the merge should be by key, use a heap of fronts.

**How it works:**

- Concatenating linked-list queues is O(1) per queue: point one tail at the next head. Dequeue of the result is still O(1).
- For k queues that must be interleaved by a priority, keep a heap of the current front of each. Each dequeue is O(log k).
- Copying every element into a new array queue is O(n) up front and then O(1) dequeue, and it loses the live sources.
- A round-robin merge dequeues one from each non-empty queue in turn. That is fair across sources and not a global sort.
</details>

### Queue Implementation Details

<details>
<summary>Name some Queue Implementations. Compare their efficiency.</summary>

### Name some Queue Implementations. Compare their efficiency.

**Definition:**

The usual implementations are a circular array, a linked list with a tail, and two stacks.

**How it works:**

- Circular array: enqueue and dequeue O(1), fixed or amortized growth, excellent locality.
- Linked list with head and tail: both ends O(1), a pointer and an allocation per item, poor locality.
- Two stacks: one for input, one for output. Each item is pushed and popped a constant number of times, so both ends are amortized O(1), and one dequeue can be O(n) when the output stack is refilled.
- `collections.deque` in Python is a block-linked deque. `ArrayDeque` in Java is a circular array.
</details>

<details>
<summary>Describe an array-based implementation of a Queue and its disadvantages.</summary>

### Describe an array-based implementation of a Queue and its disadvantages.

**Definition:**

An array queue keeps a head, a tail, and the items in one block. The naive version that does not wrap wastes the front.

**How it works:**

- A linear head that only moves forward cannot reuse dequeued slots. After n enqueues and n dequeues the array looks empty and is unusable.
- Shifting on every dequeue makes dequeue O(n).
- The circular form fixes both and still has a fixed capacity until you allocate a bigger block and copy the live range in order.
- A resize must unwind the wrap: copy from head to the end, then from 0 to tail, into a fresh array with head at 0.
</details>

<details>
<summary>What are the benefits of implementing a Queue with a Doubly Linked List versus a Singly Linked List?</summary>

### What are the benefits of implementing a Queue with a Doubly Linked List versus a Singly Linked List?

**Definition:**

A singly linked list with head and tail already gives O(1) enqueue and dequeue. A doubly linked list pays a second pointer and gains O(1) removal of a node you already hold.

**How it works:**

- Dequeue only needs the head's next pointer. The previous pointer is unused for pure FIFO.
- The double link matters when a waiting item is cancelled from the middle, as in a timer list or an LRU that is also a queue.
- Memory per node is higher, and the extra write on every enqueue is measurable in a tight buffer.
- Use the singly linked queue until you have a real mid-list delete. Then the second pointer pays for itself.
</details>

<details>
<summary>Explain the algorithm behind a Circular Queue and some potential issues that might arise.</summary>

### Explain the algorithm behind a Circular Queue and some potential issues that might arise.

**Definition:**

A circular queue stores a FIFO sequence in one fixed array and wraps the write and read indexes back to the start.

**How it works:**

- A write stores at the end index and sets it to `(end + 1) % capacity`. A read takes the start index and advances it the same way. Consumed slots are not shifted; the next write overwrites them.
- For a fixed maximum size every queue operation is constant time. Expanding the buffer requires shifting memory, which is comparatively costly. A linked list is the usual choice when the queue must grow without a fixed bound.
- Start and end alone cannot distinguish empty from full if every slot may be used. Leave one slot unused (empty when the indexes are equal, full when the in-use size is capacity − 1), or keep a count (empty at 0, full at capacity).
- On a full buffer the contract is either an error or an overwrite of the oldest element. Overwrite is the right semantics for a stream that should keep the newest samples.

**Examples:**

```text
capacity 4, with a count
enqueue 1, 2, 3 -> [1, 2, 3, _]
dequeue -> 1
enqueue 4, 5 -> [5, 2, 3, 4]
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>How to manage a Full Circular Queue Event?</summary>

### How to manage a Full Circular Queue Event?

**Definition:**

When the next write has nowhere to go, the queue is full. Handle that before moving the write index, or the new value overwrites live data by accident.

**How it works:**

- Count-based: full when `count == capacity`. Wasted-slot: full when `(end + 1) % capacity == start`, so the buffer holds at most capacity − 1 values.
- Refuse the enqueue and leave both indexes unchanged. The caller retries, drops the new item, or blocks until a read frees a slot.
- Overwrite: write the new value, then advance both end and start, so the oldest value is the one discarded. A keyboard buffer and a multimedia ring use this when the consumer is behind.
- Resize: allocate a larger array, copy from `start` through the live elements in order, set start to 0 and end to the old count. The copy is the expensive step Wikipedia contrasts with a linked list.

**When to use:**

Refuse when loss is not allowed. Overwrite when the newest sample wins. Resize when the capacity was only a starting guess.

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

### Queue Implementation Challenges

<details>
<summary>Implement a Queue using Two Stacks.</summary>

### Implement a Queue using Two Stacks.

**Definition:**

Two stacks can simulate a queue. GeeksforGeeks gives two arrangements: make enqueue walk every element, or make dequeue do that walk.

**How it works:**

- Costly enqueue: while `s1` is not empty, move it onto `s2`, push the new value onto `s1`, then move `s2` back. The front stays on top of `s1`, so dequeue and front are a single pop or peek. Stated bounds: enqueue O(n), dequeue O(1), front O(1), space O(n).
- Costly dequeue: push the new value onto `s1` in O(1). Dequeue pops `s2` if it is not empty. If `s2` is empty, move all of `s1` onto `s2` and then pop. That transfer is O(n). If both stacks are empty the queue is empty.
- Do not pop the output stack back after a transfer. The next dequeues are already in order on `s2`.
- Elements live in the two stacks only. The page's space bound is O(n).

**Examples:**

```text
enqueue 1, 2, 3 onto s1 -> s1 top is 3
dequeue: move to s2 -> s2 top is 1, pop 1
enqueue 4 onto s1
dequeue pops 2 from s2, no second move
```

**Sources:**

- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Implement a Queue using only One Stack.</summary>

### Implement a Queue using only One Stack.

**Definition:**

One explicit stack is not enough for FIFO unless another stack is borrowed. The call stack is that second stack: recursion walks to the bottom, and the bottom element is the front.

**How it works:**

- Enqueue pushes onto the single stack. That is the same O(1) push as the costly-dequeue arrangement on GeeksforGeeks, minus the second named stack.
- Dequeue: if the stack has one element, pop it. Otherwise pop the top into a local, recurse, then push the local back. The recursive call reaches the oldest element. The call depth is the number of elements.
- This is the costly dequeue from the two-stack article, with `s2` replaced by the call stack. The page states that transfer as O(n). The implicit stack also uses O(n) stack frames, so it is not a constant-space trick.
- An empty stack at the start of dequeue is underflow, the same check as an empty `s1` and empty `s2`.

**Examples:**

```js
function dequeue(stack) {
  if (stack.length === 0) throw new Error("empty");
  const top = stack.pop();
  if (stack.length === 0) return top;
  const front = dequeue(stack);
  stack.push(top);
  return front;
}
```

**Sources:**

- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Implement a Stack using Two Queues with efficient Push.</summary>

### Implement a Stack using Two Queues with efficient Push.

**Definition:**

A stack is LIFO. Efficient push on two FIFO queues means the new element is one enqueue; the linear walk happens only when something is popped.

**How it works:**

- Push enqueues into `q1` and stops. That matches the GeeksforGeeks arrangement that keeps one direction at a single push and pays O(n) on the other.
- Pop: while `q1` has more than one element, dequeue it into `q2`. The last element of `q1` is the most recently pushed, so dequeue it as the result. Swap the names of `q1` and `q2` so the survivors sit in `q1` in their old order.
- Top is the same walk without discarding the last element: move it to `q2` as well, remember it, then swap.
- Empty is an empty `q1` before pop. Both queues together store each element once.

**When to use:**

Choose this when pushes outnumber pops. If pops are the hot path, invert the walk: on push, enqueue into the empty queue and then move the old queue behind it, so pop is one dequeue.

**Examples:**

```text
push 1, push 2, push 3 -> q1 [1, 2, 3]
pop: move 1 and 2 to q2, dequeue 3, swap -> q1 [1, 2]
```

**Sources:**

- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

<details>
<summary>Write code for a Double-Ended Queue (Deque) and explain its usage.</summary>

### Write code for a Double-Ended Queue (Deque) and explain its usage.

**Definition:**

A deque is a container with insert and remove at both ends. It generalizes a stack and a queue.

**How it works:**

- As a queue it retains items and removes the oldest. As a stack it backtracks by removing the newest. Wikipedia describes both uses.
- A circular buffer already has a start and an end index, so both ends are the same index arithmetic: move start backward to push front, move end forward to push back, each modulo the capacity.
- The empty/full ambiguity is the same as a circular queue. Keep a count, or leave one slot unused.
- An input-restricted deque, which adds at one end only, is also how Wikipedia builds a monotonic queue.

**When to use:**

Use a deque when an algorithm must add or drop at either end. A plain queue is enough when every removal is the oldest element.

**Examples:**

```js
function pushFront(q, x) {
  if (q.count === q.cap) throw new Error("full");
  q.start = (q.start - 1 + q.cap) % q.cap;
  q.data[q.start] = x;
  q.count++;
}
```

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>Describe how to implement a Blocking Queue and a use case for it.</summary>

### Describe how to implement a Blocking Queue and a use case for it.

**Definition:**

A blocking queue is a bounded buffer: the producer waits while it is full, and the consumer waits while it is empty.

**How it works:**

- The storage is a circular buffer. Full and empty use a count, or the wasted-slot rule, so a waiter can tell the two states apart.
- Enqueue, if full, waits until a dequeue drops the count. Dequeue, if empty, waits until an enqueue raises it. Wikipedia's producer–consumer problem is this bounded buffer, including Dijkstra's solution and the monitor version.
- The same pattern is a message queue between threads: the sender and the receiver do not have to touch the queue at the same moment, and the message stays stored until it is retrieved.
- A limit on how many messages may be outstanding is the bound. Past that bound the sender blocks or the enqueue fails.

**When to use:**

Use it to hand work from a producer thread to a consumer thread without either spinning on an empty or full buffer.

**Sources:**

- [Wikipedia — Producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Solve for a queue that automatically resizes to hold new elements.</summary>

### Solve for a queue that automatically resizes to hold new elements.

**Definition:**

A growable queue keeps accepting enqueues after the current array is full by allocating a larger buffer and copying the live elements.

**How it works:**

- Detect full with the count, before the write. Allocate a new array, copy from `start` in order for `count` elements, then set start to 0 and end to count.
- Wikipedia notes that this shift of memory is comparatively costly. A linked list avoids that copy when the queue must grow without a chosen bound.
- After the copy, the enqueue that triggered growth writes at `end` and advances it. Readers still see the same FIFO order.
- Shrinking is optional. If you shrink, do it only when the count is a small fraction of capacity, and copy the same way, or the next growth pays the copy again.

**When to use:**

Grow an array queue when a fixed cap would reject work you must keep. Prefer a linked list when growth is the common case and you want to skip the copy.

**Examples:**

```text
[3, 4, 1, 2] start=2 count=4
copy in order -> [1, 2, 3, 4, _, _]
start=0 end=4
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>Create a Queue data structure with O(1) complexity for enqueue, dequeue, and finding the maximum element.</summary>

### Create a Queue data structure with O(1) complexity for enqueue, dequeue, and finding the maximum element.

**Definition:**

Pair a normal queue with a decreasing deque. The deque's front is the maximum among values still in the queue.

**How it works:**

- Enqueue writes the value on the normal queue and then, while the deque's back is smaller than the new value, pops that back. Push the new value on the deque. That is Wikipedia's decreasing monotonic queue.
- Dequeue removes the front of the normal queue. If that value is also the deque's front, pop the deque too. Older smaller values were already discarded, so they cannot be the max.
- Wikipedia's bound for the sliding-window form is linear in the length of the sequence. The naive scan is O(n·k). Each element is pushed once and popped at most once from the deque, so the linear bound is a constant amount of deque work per element across the run, not a claim that one dequeue is worst-case constant if you forget the amortization.
- The normal queue's own enqueue and dequeue stay constant time when it is a fixed circular buffer.

**Examples:**

```text
enqueue 1, 3, 2
deque backs: pop 1 (smaller than 3), keep 3, then 2
max is 3
dequeue 1 (not the max)
max is still 3
```

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

### Queue Types and Variants

<details>
<summary>Explain the difference between a Linear Queue and a Circular Queue.</summary>

### Explain the difference between a Linear Queue and a Circular Queue.

**Definition:**

A linear queue uses the array from left to right and does not wrap. A circular queue treats the same array as a ring.

**How it works:**

- After a linear dequeue, the free slot is at the front. The next enqueue still advances the end index, so the queue can look full while the front of the array is unused. Recovering that space means shifting every live element down.
- A circular queue sends the end index back to 0 with modulo. Wikipedia's point is that a consumed element does not force the others to move, and fixed-capacity operations stay constant time.
- The ring cannot tell empty from full with two indexes if every slot is usable. A linear queue can: empty when start equals end, and full when end equals capacity, but only until the wasted front appears.
- Expanding either array costs a copy. The circular copy must start at the read index so order survives the wrap.

**When to use:**

Use the ring for a fixed buffer that enqueues and dequeues continuously. A linear array is enough for a one-pass fill that is then drained once.

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

<details>
<summary>What is a Monotonic Queue, and how do you implement it?</summary>

### What is a Monotonic Queue, and how do you implement it?

**Definition:**

A monotonic queue keeps a subsequence in increasing or decreasing order and drops values that cannot matter later.

**How it works:**

- Wikipedia builds it from a deque. For an increasing sequence: while the back is greater than the new element, pop the back, then push the new element. For decreasing, pop while the back is smaller.
- The surviving elements stay in their original order. The other end of the deque is where you read the current extreme.
- A sliding-window maximum or minimum is linear in the length of the sequence. The naive window scan is O(n·k) time and O(1) extra space. A search per window is O(n log n) time and O(n) space.
- When the window slides, also drop the front if it has fallen out of the window. That front is the answer for the current window before the drop.

**Examples:**

```text
values 1, 3, 2 and a decreasing deque
1 -> [1]
3 -> pop 1, [3]
2 -> [3, 2]
window max is the front, 3
```

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
</details>

<details>
<summary>Describe a Bounded Queue. What considerations are necessary when implementing one?</summary>

### Describe a Bounded Queue. What considerations are necessary when implementing one?

**Definition:**

A bounded queue refuses to grow past a fixed number of outstanding elements.

**How it works:**

- Message queues have implicit or explicit limits on the size of one message and on how many messages may remain outstanding.
- A circular buffer is the fixed-capacity implementation. Constant-time enqueue and dequeue hold only while you do not expand. Decide full versus empty with a count or a wasted slot before you accept a write.
- On full, block the producer (the bounded-buffer problem), return an error, or overwrite the oldest. Those are different contracts.
- The limit should match the consumer. A cap that is too small stalls the producer. A cap that is too large holds memory the consumer will not drain.

**When to use:**

Bound the queue when a fast producer must not allocate without limit. Leave it unbounded only when loss of the cap is worse than memory growth, and then a linked list avoids the ring's copy.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
</details>

<details>
<summary>What is the difference between a Single-Ended Queue and a Double-Ended Queue?</summary>

### What is the difference between a Single-Ended Queue and a Double-Ended Queue?

**Definition:**

A single-ended queue adds at one end and removes at the other. A deque adds and removes at both ends.

**How it works:**

- The queue is the retainer: the oldest item leaves first. Wikipedia's deque can also backtrack, which is the stack direction, because the end you just wrote is the end you can remove.
- A circular queue implements the single-ended case with a start index and an end index moving only forward.
- A deque uses the same ring but allows start to move backward and end to move backward as well.
- Some algorithms need only an input-restricted deque: add at one end, remove at either. That is enough for the monotonic queue.

**When to use:**

Stay with a single-ended queue for FIFO. Move to a deque when the algorithm must push or pop both the oldest and the newest.

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

### Queue-Related Algorithms and Problem Solving

<details>
<summary>Implement a Queue Algorithm to sort a given list of integers.</summary>

### Implement a Queue Algorithm to sort a given list of integers.

**Definition:**

Sorting with a priority queue is heapsort: insert the values into a binary heap and repeatedly remove the minimum.

**How it works:**

- A binary heap's delete-min is O(log n) in the worst case, and find-min is O(1). Building the heap from the array is O(n).
- Heapsort's best, average, and worst time are O(n log n). It sorts in the array the heap already occupies.
- A plain FIFO queue does not compare keys, so it does not order them. The priority, not the arrival time, has to decide who leaves.
- If equal keys must keep their original order, the heap entry needs the original index as a tie break. The heap page's order is the key order, not arrival order.

**Examples:**

```text
[3, 1, 2] -> heap, then delete-min
1, then 2, then 3
```

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
- [Wikipedia — Priority queue](https://en.wikipedia.org/wiki/Priority_queue)
</details>

<details>
<summary>Implement an algorithm to reverse the first k elements of a Queue.</summary>

### Implement an algorithm to reverse the first k elements of a Queue.

**Definition:**

The first k elements reverse if you hold them in a stack and write them back, then rotate the untouched tail to the front.

**How it works:**

- Dequeue k values and push each onto a stack. Popping that stack yields them in reverse. Enqueue each popped value.
- The queue now ends with the reversed prefix, and the old tail is still at the front. Dequeue and enqueue the remaining `n - k` elements so the reversed prefix moves to the front.
- Each step is one circular-buffer enqueue or dequeue, which is constant time, done once per element of the prefix and once per element of the tail.
- If k is 0 or k equals n, the tail rotation is empty or the whole queue is the prefix. If k is larger than n, there is no such prefix.

**Examples:**

```text
queue [1, 2, 3, 4, 5], k = 3
stack after dequeue -> 3, 2, 1
enqueue pops -> [4, 5, 3, 2, 1]
rotate 2 -> [3, 2, 1, 4, 5]
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>How can Queues be used in the implementation of a Cache Eviction Policy, like FIFO?</summary>

### How can Queues be used in the implementation of a Cache Eviction Policy, like FIFO?

**Definition:**

A FIFO cache evicts the element that has been stored the longest. A queue is that arrival order.

**How it works:**

- Enqueue the key when it is stored. The front is the oldest. When the cache is at its bound, dequeue the front and drop that entry.
- The bound is the same idea as a message queue's limit on outstanding items, or a circular buffer's capacity.
- A hit does not move the key in a pure FIFO policy. Moving it would be least-recently-used, which needs removal from the middle, not only from the front.
- Lookup of a key is separate from the order. The queue only answers 'who arrived first'.

**When to use:**

Use FIFO when insertion order is the eviction rule. Use a structure that can delete from the middle when a hit must refresh the order.

**Sources:**

- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Provide an algorithm for rotating a Queue by a certain number of elements.</summary>

### Provide an algorithm for rotating a Queue by a certain number of elements.

**Definition:**

Rotating a queue by k moves the front element to the back, k times.

**How it works:**

- If the queue is empty, stop. Reduce k modulo the count so you never spin extra full turns.
- One step is dequeue then enqueue of that same value. On a circular buffer each of those is constant time, and the other elements stay put.
- Repeat that step k times. The new front is the old element that was k places back.
- Rotating by `count - k` the other way is the same permutation with fewer steps when k is close to the length. Take the smaller of k and count − k only if you also reverse direction; a one-direction rotate should use `k % count`.

**Examples:**

```text
[1, 2, 3, 4], k = 2
dequeue/enqueue 1 -> [2, 3, 4, 1]
dequeue/enqueue 2 -> [3, 4, 1, 2]
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

<details>
<summary>How would you use a Queue to generate binary numbers from 1 to N?</summary>

### How would you use a Queue to generate binary numbers from 1 to N?

**Definition:**

The binary representations from 1 through N are generated by a queue that appends a 0 and a 1 to each prefix.

**How it works:**

- Start by enqueueing `"1"`. That is the binary form of 1.
- N times: dequeue a prefix, record it, and if you still need more numbers enqueue `prefix + "0"` and `prefix + "1"`.
- Each dequeue produces the next number in numeric order because shorter prefixes were enqueued earlier, and within one length the 0-child was enqueued before the 1-child.
- Stop enqueueing children once N numbers have been recorded, or the queue holds strings you will never print.

**Examples:**

```text
N = 4
dequeue 1 -> record 1, enqueue 10, 11
dequeue 10 -> record 2, enqueue 100, 101
dequeue 11 -> record 3
dequeue 100 -> record 4
```

**Sources:**

- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

### Advanced Queue Concepts

<details>
<summary>Explain how a Queue can be utilized in Graph Traversal Algorithms.</summary>

### Explain how a Queue can be utilized in Graph Traversal Algorithms.

**Definition:**

Breadth-first search keeps the frontier in a FIFO queue. Depth-first search does not: it uses a stack, including the call stack.

**How it works:**

- BFS visits vertices in order of distance from the source. Wikipedia states it is optimal for shortest paths. Its time is O(|V| + |E|) when every vertex and edge is explored, and its space is O(|V|).
- The queue holds discovered vertices that are not yet expanded. A vertex is enqueued when it is first seen, and dequeued when its edges are followed.
- DFS deepens one path before a sibling. Its time on an explicit graph traversed without repetition is also O(|V| + |E|). It does not generally find shortest paths. The space follows the longest path when the graph is implicit.
- Choose the queue when the next vertex must be the one discovered earliest. Choose the stack when the search should finish a branch first.

**When to use:**

Use the queue for unweighted shortest paths and level order. Use DFS when you need a finishing time, a path that dives, or less memory on a deep narrow graph.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>Describe the application of Queues in Scheduling Algorithms, such as Round Robin Scheduling.</summary>

### Describe the application of Queues in Scheduling Algorithms, such as Round Robin Scheduling.

**Definition:**

Round-robin assigns equal time slices in circular order and does not give one process priority over another.

**How it works:**

- The ready processes sit in a FIFO queue. The scheduler dequeues the front, lets it run for one quantum, and if it is not finished enqueues it at the back.
- Wikipedia calls the slices time quanta, says the algorithm is simple and starvation-free, and notes the same idea schedules data packets on a network.
- A process that blocks leaves the ready queue until the event completes. It is not rotated while it cannot run.
- The quantum is the whole policy. A very small quantum spends the turn switching. A very large one makes the queue behave like a single long run of the front process.

**When to use:**

Use round-robin when each runnable process should get a turn. If short work should jump the line, the ready structure is a priority queue, not this FIFO ring.

**Sources:**

- [Wikipedia — Round-robin scheduling](https://en.wikipedia.org/wiki/Round-robin_scheduling)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

<details>
<summary>Explain a scenario where Queues are used in a multi-threading environment.</summary>

### Explain a scenario where Queues are used in a multi-threading environment.

**Definition:**

Threads that must not run the same critical section at the same moment pass work through a queue instead of sharing the data structure.

**How it works:**

- A message queue is the asynchronous pattern: the sender and the receiver do not have to use the queue at the same time. The message stays stored until the recipient retrieves it.
- The producer–consumer bounded buffer is the blocking form. The producer waits when the circular buffer is full. The consumer waits when it is empty.
- Erlang processes communicate asynchronously by message queuing. Competing consumers are several threads taking messages from the same queue.
- There is a limit on how many messages may stay outstanding. Past it, the sender blocks or the implementation rejects the send.

**When to use:**

Use the queue when one thread produces items and another consumes them. Do not use it as a substitute for a lock around a structure both threads mutate in place.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
- [Wikipedia — Producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>What is a Dequeuable Priority Queue, and when might one be used?</summary>

### What is a Dequeuable Priority Queue, and when might one be used?

**Definition:**

A priority queue removes the best key next. A dequeuable one can also remove some other waiting element, not only that best key.

**How it works:**

- On a binary heap, find-min is O(1) and delete-min is O(log n) worst case. Finding an arbitrary element is O(n).
- Decrease-key is O(log n) worst case when the heap already knows where the element sits. Deleting a known index is the same shape: swap with the last leaf and sift.
- Without a handle, cancellation scans for the element first, which is the O(n) find.
- The ordinary heap is enough when the only removal is the current minimum.

**When to use:**

Use the extra removal when a waiting job can be cancelled or its key can change before it reaches the front. A timer list and a graph decrease-key are that case.

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Priority queue](https://en.wikipedia.org/wiki/Priority_queue)
</details>

<details>
<summary>Discuss the role of Queues in the Breadth-First Search (BFS) and `A*` Search Algorithms.</summary>

### Discuss the role of Queues in the Breadth-First Search (BFS) and `A*` Search Algorithms.

**Definition:**

BFS orders expansion by a FIFO queue. A* orders expansion by a priority queue of f(n) = g(n) + h(n).

**How it works:**

- BFS dequeues the vertex discovered earliest. Wikipedia states that this finds shortest paths. Time is O(|V| + |E|) when the search explores the whole explicit graph, and space is O(|V|).
- A* removes the node with the lowest f(n) from the open set. g(n) is the cost from the start to n. h(n) estimates the cheapest cost from n to the goal and is problem-specific.
- The open set is the priority queue Wikipedia calls the fringe or frontier. Neighbors are added after their g and f values are updated.
- A FIFO queue is the wrong open set for A*, because arrival order is not lowest f. A binary heap's delete-min is the selection step, O(log n) worst case per removal.

**When to use:**

FIFO when every edge has the same cost. The f-ordered open set when edges have costs and you have a heuristic.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — A* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm)
- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
</details>

### Real-World Queue Applications

<details>
<summary>How are Queues used in Event-Driven Programming and Message Queuing Systems?</summary>

### How are Queues used in Event-Driven Programming and Message Queuing Systems?

**Definition:**

An event or a message is stored on a queue until the recipient handles it. The sender does not have to be running at that later moment.

**How it works:**

- Wikipedia defines the message queue as asynchronous communication: the parties do not interact with the queue at the same time, and the message stays until it is retrieved.
- Event notification and publish/subscribe are the same split. The sender reports that something happened and does not wait for a response. It also should not have to wait because one recipient crashed.
- A single queue can feed competing consumers, several concurrent handlers taking messages from that one queue.
- Outstanding messages and the size of one message have limits. The GUI case is the same buffer: input events wait until the event loop dequeues them.

**When to use:**

Queue the event when the producer must return before the work finishes. Handle it inline when the caller needs the result before it continues.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Discuss the importance of Queues in Operating Systems, specifically in Process Management.</summary>

### Discuss the importance of Queues in Operating Systems, specifically in Process Management.

**Definition:**

A process that is ready to run waits in a queue until the scheduler selects it. The queue is what keeps that wait ordered.

**How it works:**

- Round-robin keeps ready processes in a FIFO queue and runs the front for one equal quantum, then moves it to the back if it is still runnable. The policy has no priority and is starvation-free.
- The same circular order schedules network packets. A blocked process is not in the ready queue; it waits on the event instead.
- Inside one system, message queues pass control or content between processes or threads. Implementations exist for that system only, or between different computers.
- The ready queue's bound matters. An unbounded list of runnable processes still has to be stored, and a message queue puts an explicit limit on how many messages stay outstanding.

**When to use:**

A FIFO ready queue matches equal sharing. A priority queue matches a policy that must run the best key next, at the cost of the heap's delete-min.

**Sources:**

- [Wikipedia — Round-robin scheduling](https://en.wikipedia.org/wiki/Round-robin_scheduling)
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

<details>
<summary>Explain how a Consumer-Producer Model can be implemented using a Queue.</summary>

### Explain how a Consumer-Producer Model can be implemented using a Queue.

**Definition:**

The producer enqueues items. The consumer dequeues them. A bounded buffer makes the producer wait when the queue is full and the consumer wait when it is empty.

**How it works:**

- Store the items in a circular buffer. Full means the count equals capacity, or the wasted slot would be overwritten. Empty means the count is 0.
- Wikipedia's producer–consumer problem includes a bounded-buffer solution with semaphores and a version with monitors. Either way the queue is the shared buffer.
- The message-queue statement of the same idea: the parties need not touch the queue together, and the message remains stored until it is retrieved.
- Do not let the producer keep allocating when the consumer is behind. The outstanding-message limit is what stops that.

**Examples:**

```text
capacity 2
producer enqueue A, B
producer blocks
consumer dequeue A
producer enqueue C
```

**Sources:**

- [Wikipedia — Producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Describe an application of Queues in Network Packet Routing and Load Balancing.</summary>

### Describe an application of Queues in Network Packet Routing and Load Balancing.

**Definition:**

A router and a scheduler both hold packets or jobs until the next hop or the next worker can take them.

**How it works:**

- Round-robin assigns equal quanta in circular order and is used for data packet scheduling, not only for processes. There is no priority, and the policy is starvation-free.
- The packets wait in a FIFO queue. The front is transmitted, and a packet that cannot finish its turn goes to the back.
- Competing consumers are the load-balancing form: several workers dequeue from the same message queue, so a busy worker is not the only one draining the burst.
- The queue has a limit on outstanding messages. Past it, the sender blocks or the send fails instead of storing without bound.

**When to use:**

One FIFO queue shares a link fairly. Several consumers on one queue spread work when the arrival rate exceeds one worker.

**Sources:**

- [Wikipedia — Round-robin scheduling](https://en.wikipedia.org/wiki/Round-robin_scheduling)
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

### Queue Performance and Optimization

<details>
<summary>What strategies can be implemented to reduce Time Complexity in queue operations?</summary>

### What strategies can be implemented to reduce Time Complexity in queue operations?

**Definition:**

The expensive queue is the one that slides every element on each dequeue. Stop doing that.

**How it works:**

- A circular buffer leaves the other elements where they are. Wikipedia states that fixed-capacity queue operations are then constant time. The linear array that shifts on dequeue pays work proportional to the live elements.
- Do not grow on every enqueue. Expansion shifts memory and is comparatively costly. Grow by a larger chunk, or use a linked list when there is no fixed bound.
- Two stacks can implement a queue, but GeeksforGeeks states enqueue O(n) and dequeue O(1), or enqueue O(1) and dequeue O(n). That is worse than the ring when both directions are hot.
- A monotonic deque keeps a sliding maximum linear in the sequence length. Scanning the whole queue for the max on every step is the naive O(n·k) window.

**When to use:**

Use the ring for a hot FIFO with a known cap. Use the linked list when the cap is unknown and the copy on growth would dominate.

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
</details>

<details>
<summary>Name the Most Efficient way to implement Stack and Queue together.</summary>

### Name the Most Efficient way to implement Stack and Queue together.

**Definition:**

One deque implements both. A stack only uses one end. A queue uses opposite ends.

**How it works:**

- Wikipedia defines the deque as the generalization of the stack and the queue: a retainer in one direction and backtracking in the other.
- A circular buffer gives both ends with index arithmetic modulo the capacity. Fixed-capacity operations stay constant time, which is the bound Wikipedia gives for the circular queue.
- Empty versus full still needs a count or a wasted slot. That issue does not go away because you use both ends.
- Two stacks can fake a queue, but one of enqueue or dequeue is O(n) in the GeeksforGeeks arrangements. That is the wrong building block when both the stack and the queue operations are frequent.

**When to use:**

Use the deque when the same buffer must push and pop like a stack and also enqueue and dequeue like a queue. Keep a plain circular queue when you never touch the back except to enqueue.

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>How do modern CPUs and Memory Caching behavior affect the performance of queue operations?</summary>

### How do modern CPUs and Memory Caching behavior affect the performance of queue operations?

**Definition:**

A queue whose slots sit next to each other matches sequential locality. A queue of separately allocated nodes does not.

**How it works:**

- Spatial locality is the use of data in close storage locations. Sequential locality, a special case, is linear access such as walking a one-dimensional array. Systems with strong locality are the ones caching and prefetching help.
- A circular buffer is that array. The next enqueue or dequeue touches a neighboring slot, which is the access pattern a cache line and a prefetcher expect.
- Temporal locality is reuse of the same data within a short time. A hot head and tail are reused; the middle of a long queue is not, so a huge ring still misses once the working set exceeds the cache.
- Constant-time index arithmetic does not remove a cache miss. The circular-buffer bound counts operations, not memory stalls.

**When to use:**

Prefer the contiguous ring when the queue is on a hot path and fits in cache. A linked queue is justified when the copy on growth costs more than the pointer jumps.

**Sources:**

- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

### Queue Implementation with Constraints

<details>
<summary>Design a queue which supports a rolling average query with O(1) time complexity.</summary>

### Design a queue which supports a rolling average query with O(1) time complexity.

**Definition:**

Keep the FIFO values and a running sum. The average is the sum divided by the count, and both updates are one arithmetic step on top of a constant-time enqueue or dequeue.

**How it works:**

- Enqueue adds the value to the sum and stores it in a circular buffer. Dequeue subtracts the value just removed. Wikipedia's fixed-capacity queue operations are constant time, and the sum changes by one addition or subtraction.
- The query returns `sum / count` when count is not 0. An empty queue has no average.
- Integer sums can overflow the element type even when each element fits. Widen the accumulator, or the average is wrong while the indexes are still correct.
- This is a window over whatever is still in the queue, not a sliding window of a fixed k unless you also dequeue when the count would pass k.

**Examples:**

```text
enqueue 2, 4 -> sum 6, count 2, average 3
dequeue 2 -> sum 4, count 1, average 4
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>Implement a queue with constraints that minimizes total wait time for dequeuing.</summary>

### Implement a queue with constraints that minimizes total wait time for dequeuing.

**Definition:**

A FIFO queue serves in arrival order. It does not look at how long each job will take. Shortest-first service needs a priority queue.

**How it works:**

- Round-robin is the fair FIFO policy: equal quanta, no priority, starvation-free. It does not pick the shortest job.
- Order the ready structure by service time and remove the minimum. A binary heap's delete-min is O(log n) worst case, and find-min is O(1).
- The heap does not know a job's service time unless you store it as the key. Arrival time is a different key and reproduces FIFO.
- If a long job must not wait forever, FIFO or round-robin is the constraint that forbids pure shortest-first. You cannot have both starvation-freedom of round-robin and 'always run the shortest' with one ordering.

**When to use:**

Use shortest-service order when the goal is the sum of waits and every job's time is known up front. Use round-robin when a long job must still get a turn.

**Sources:**

- [Wikipedia — Round-robin scheduling](https://en.wikipedia.org/wiki/Round-robin_scheduling)
- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Priority queue](https://en.wikipedia.org/wiki/Priority_queue)
</details>

<details>
<summary>Implement an Immutable Queue in a functional programming style.</summary>

### Implement an Immutable Queue in a functional programming style.

**Definition:**

An immutable queue never mutates a previous version. Enqueue and dequeue return a new pair of lists, which is the two-stack queue with the stacks treated as persistent lists.

**How it works:**

- The front list is dequeued from its head. The rear list is enqueued by consing on its head, which is the new rear.
- When the front is empty, dequeue reverses the rear into the front and continues. That reverse is the transfer GeeksforGeeks describes when the output stack is empty, stated there as O(n) for the dequeue that performs it.
- Older versions keep their own front and rear lists. Nodes the new version does not replace can be shared. Sharing is what makes the structure persistent rather than a full copy of every element on each call.
- A caller who still holds the old queue still sees the old sequence. There is no in-place pop.

**Examples:**

```text
enqueue 1 on empty -> front [], rear [1]
enqueue 2 -> front [], rear [2, 1]
dequeue -> reverse rear, front [1, 2], pop 1
```

**Sources:**

- [GeeksforGeeks — Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

### Queue in Distributed Systems

<details>
<summary>Explain the concept of a Distributed Queue and its role in distributed systems.</summary>

### Explain the concept of a Distributed Queue and its role in distributed systems.

**Definition:**

A distributed queue passes messages between different computers, not only between threads in one process.

**How it works:**

- Wikipedia distinguishes queues that exist for one operating system or one application from implementations that connect multiple applications and multiple operating systems.
- The pattern stays asynchronous. The sender and the receiver do not have to use the queue at the same time, and the message is stored until a recipient retrieves it.
- Competing consumers on that shared queue are how several machines drain one stream of work.
- Limits still apply: the size of one message and the number of messages that may remain outstanding. The queue can run on private servers or as an external service.

**When to use:**

Use a cross-machine queue when the producer and the consumer are different programs or hosts. An in-process circular buffer is enough when they are threads in one process.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>How would you design a fault-tolerant Queuing System that ensures message delivery even after a crash?</summary>

### How would you design a fault-tolerant Queuing System that ensures message delivery even after a crash?

**Definition:**

The property Wikipedia states is that a message stays stored until a recipient retrieves it, and the sender does not have to wait because a recipient crashed.

**How it works:**

- Treat enqueue as finished only after the message is stored on the queue. Before that, the sender still has it and can retry. After that, the queue, not the sender, holds it.
- A crashed recipient is why the communication is asynchronous. Another consumer, or the same recipient after restart, retrieves what is still stored. Competing consumers are several retrievers on one queue.
- The page describes storage until retrieval. It does not say that the queue process's own memory survives a crash of the queue. A design that loses the process loses whatever was only in that memory.
- Messages that cross computers use an implementation that connects different systems, with a limit on outstanding messages. The sender still needs a way to tell a stored message from one that never made it in, or a retry will store a second copy.

**When to use:**

Acknowledge the sender only after the store. Keep the message until a consumer has retrieved it. If the queue process itself can die, the store has to be outside that process's memory; the article's guarantee is the stored-until-retrieved rule, not a disk format.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Describe a scenario where a queue is used for Load Leveling in a distributed application.</summary>

### Describe a scenario where a queue is used for Load Leveling in a distributed application.

**Definition:**

Load leveling lets a fast producer keep going while slower consumers drain a shared queue at their own rate.

**How it works:**

- The producer enqueues work and returns. Wikipedia's message queue stores the message until a recipient retrieves it, and the parties do not have to use the queue at the same time.
- Competing consumers are several workers dequeueing the same queue, so one slow worker is not the only drain.
- The queue's limit on outstanding messages is the shock absorber's size. Past that limit the producer blocks or the send fails, which is the backpressure.
- This is the bounded buffer across machines: implementations exist that connect different computer systems, not only threads in one process.

**When to use:**

Level the load when arrivals come in bursts and the workers are slower or fewer. Skip the queue when the caller must have the result before it returns.

**Sources:**

- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
- [Wikipedia — Producer–consumer problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem)
</details>

### Queue Interview Challenges

<details>
<summary>Create a simulation of a Queue at a grocery store, including multiple cashier lines.</summary>

### Create a simulation of a Queue at a grocery store, including multiple cashier lines.

**Definition:**

Each cashier is a FIFO queue of customers. A customer is held until that cashier can process them.

**How it works:**

- Wikipedia's queue is exactly this: persons stored to be processed later. Enqueue joins a line. Dequeue is the customer who reaches the cashier.
- With several lines, the arriving customer chooses a queue. The usual choice in the simulation is the line whose count is smallest.
- Service takes the front of a chosen line. The other lines do not move. A single shared queue with several cashiers is the competing-consumer variant: one line, several dequeuers.
- The simulation clock can advance one customer per cashier per tick, or a customer can carry a service count that decrements while they are at the front.

**Examples:**

```text
lines [A, B] and [C]
arrival joins the shorter line -> [A, B, D] and [C]
one service step dequeues A and C
```

**Sources:**

- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
- [Wikipedia — Message queue](https://en.wikipedia.org/wiki/Message_queue)
</details>

<details>
<summary>Implement a queue that supports finding the kth Last Element from the tail.</summary>

### Implement a queue that supports finding the kth Last Element from the tail.

**Definition:**

In a circular buffer the last written element and the one k places behind it are index arithmetic, not a walk.

**How it works:**

- If `end` is the next slot to write, the newest element is at `(end - 1 + capacity) % capacity`. The kth from that tail, counting the newest as 1, is at `(end - k + capacity) % capacity`.
- The query is valid only when `k` is at least 1 and `k` is at most the count. Otherwise that index is empty or wrapped into unused slots.
- Enqueue and dequeue stay the ordinary circular-buffer updates, which are constant time at fixed capacity. The query does not move start or end.
- A linked queue whose tail pointer has no backward link cannot do this without walking from the head. The ring can, because every live slot is addressable.

**Examples:**

```text
slots [5, 2, 3, 4], end = 1, count = 4, k = 2
newest is index 0 (value 5)
kth last is index (1 - 2 + 4) % 4 = 3, value 4
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
</details>

<details>
<summary>Design a queue that, after enqueuing an item, returns the Median of all elements in the queue.</summary>

### Design a queue that, after enqueuing an item, returns the Median of all elements in the queue.

**Definition:**

Keep the values in two binary heaps: a max-heap for the lower half and a min-heap for the upper half. The median is a root, which a binary heap can read in O(1).

**How it works:**

- Insert goes into one heap. A binary heap insert is O(log n) worst case and O(1) on average. After the insert, if the sizes differ by more than one, delete-min (or delete-max) from the larger heap and insert that value into the other. Delete-min is O(log n) worst case.
- The heaps stay balanced when their counts differ by at most 1. If the total count is odd, the bigger root is the median. If even, the two roots are the two central values.
- Find-min on a binary heap is O(1). The lower-half max is find-min on a heap that stores negated keys, or the same O(1) read of a max-heap root.
- Dequeue of an arbitrary FIFO element is not delete-min. Finding an arbitrary heap element is O(n). This structure answers the median of the multiset currently stored; removing the oldest FIFO element needs a handle or it pays that O(n) find.

**Examples:**

```text
enqueue 3 -> lower [3], median 3
enqueue 1 -> lower [1], upper [3], median 1 and 3
enqueue 2 -> lower [1, 2], upper [3], median 2
```

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Priority queue](https://en.wikipedia.org/wiki/Priority_queue)
</details>

<details>
<summary>Solve the problem of implementing a queue with automatic priority adjustment based on the frequency of element access.</summary>

### Solve the problem of implementing a queue with automatic priority adjustment based on the frequency of element access.

**Definition:**

Arrival order is a FIFO queue. Access frequency is a different key, so the structure that must surface the most accessed element is a priority queue, not the FIFO front.

**How it works:**

- Each stored element has a count. On access, increment that count. The next removal of the preferred element is delete-min on a heap ordered by the reversed count, or delete-max if the heap is a max-heap.
- A binary heap's delete-min is O(log n) worst case. Decrease-key is also O(log n) worst case when the heap already knows the element's index. A frequency change is that key update.
- Finding an element with no handle is O(n). If access is by value, keep the index or the frequency update scans.
- The FIFO queue still answers 'who arrived first'. It does not move an element forward because it was touched. Mixing the two without a handle means either evicting the oldest or paying the heap update, not both for free.

**When to use:**

Use the heap when the next element should be the most or least accessed. Use the FIFO queue when frequency must not jump the line.

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Priority queue](https://en.wikipedia.org/wiki/Priority_queue)
- [Wikipedia — Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
</details>

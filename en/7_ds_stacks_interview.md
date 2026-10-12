# Stacks — Interview

Answers are filled for every question in this note.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Stacks — Interview](./7_ds_stacks_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/).

### Stack Fundamentals

<details>
<summary>What is a Stack?</summary>

### What is a Stack?

**Definition:**

A **stack** is a simple data structure that follows the **Last-In, First-Out (LIFO)** principle. It's akin to a stack of books, where the most recent addition is at the top and easily accessible.

**How it works:**

**Core Characteristics**

- **Data Representation**: Stacks can hold homogeneous or heterogeneous data.
- **Access Restrictions**: Restricted access primarily to the top of the stack, making it more efficient for certain algorithms.

**Stack Operations**

1. **Push**: Adds an element to the top of the stack.
2. **Pop**: Removes and returns the top element.
3. **Peek**: Returns the top element without removing it.
4. **isEmpty**: Checks if the stack is empty.
5. **isFull** (for array-based stacks): Checks if the stack is full.

All the above operations typically have a time complexity of $O(1)$, making stack operations highly efficient.

**Visual Representation**

![Stack Data Structure](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/stacks%2Fstack.png?alt=media&token=74633f0a-83f7-4038-8b82-e10f0d6006b9&_gl=1*1uzhlk1*_ga*OTYzMjY5NTkwLjE2ODg4NDM4Njg.*_ga_CW55HF8NVT*MTY5NjYwNzMxNS4xNDcuMS4xNjk2NjA3NzE2LjUwLjAuMA..)

**When to use:**

**Practical Applications**

1. **Function Calls**: The call stack keeps track of program flow and memory allocation during method invocations.
2. **Text Editors**: The undo/redo functionality often uses a stack.
3. **Web Browsers**: The Back button's behavior can be implemented with a stack.
4. **Parsing**: Stacks can be used in language processing for functions like balanced parentheses, and **binary expression evaluation**.

5. **Memory Management**: Stacks play a role in managing dynamic memory in computer systems.

6. **Infix to Postfix Conversion**: It's a crucial step for evaluating mathematical expressions such as `2 + 3 * 5 - 4` in the correct precedence order. Stack-based conversion simplifies parsing and involves operators such as `push` and `pop` until the correct order is achieved.

7. **Graph Algorithms**: Graph traversal algorithms such as Depth First Search (DFS) deploy **stacks** as a key mechanism to remember vertices and explore connected components.

**Examples:**

**Code Example: Basic Stack**

Here is the Python code:

```python
class Stack:
    def __init__(self):
        self.stack = []

    def push(self, item):
        self.stack.append(item)

    def pop(self):
        if not self.is_empty():
            return self.stack.pop()

    def peek(self):
        if not self.is_empty():
            return self.stack[-1]

    def is_empty(self):
        return len(self.stack) == 0

    def size(self):
        return len(self.stack)
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Why Stack is considered a Recursive data structure?</summary>

### Why Stack is considered a Recursive data structure?

**Definition:**

A **stack** is considered a **recursive data structure** because its definition is self-referential. At any given point, a stack can be defined as a top element combined with another stack (the remainder).

Whenever an element is pushed onto or popped off a stack, what remains is still a stack. This **self-referential nature**, where operations reduce the problem to smaller instances of the same type, embodies the essence of **recursion**

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>What are the primary operations performed on a Stack and their time complexities?</summary>

### What are the primary operations performed on a Stack and their time complexities?

**Definition:**

Let's look into the fundamental operations of a **stack** and their associated time complexities.

**How it works:**

**Stack Operations and Complexity**

- **Push** (Time: $O(1)$): New elements are added at the top of the stack, making this a $O(1)$.
- **Pop** (Time: $O(1)$): The top element, and the only one accessible, is removed during this $O(1)$ operation.
- **Peek** (Time: $O(1)$): Viewing the top of the stack doesn't alter its structure, thus taking $O(1)$.
- **Size** (Time: $O(1)$): Stacks typically keep track of their size, ensuring $O(1)$ performance.
- **isEmpty** (Time: $O(1)$): Checks for stack emptiness and usually completes in $O(1)$ time.

**Examples:**

**Code Example: Stack**

Here is the Python code:

```python
stack = []
stack.append(1)  # Pushes 1 onto the stack
stack.append(3)  # Pushes 3 onto the stack
print(stack.pop())  # Outputs 3; removes the top element from the stack
print(stack[-1])  # Outputs 1; peek at the top element
print(len(stack))  # Outputs 1; returns the size of the stack
print(not stack)  # Outputs False; checks if the stack is empty
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

### Stack Usage and Applications

<details>
<summary>When should I use Stack or Queue data structures instead of Arrays/Lists?</summary>

### When should I use Stack or Queue data structures instead of Arrays/Lists?

**Definition:**

**Queues** and **Stacks** provide structured ways to handle data, offering distinct advantages over more generic structures like **Lists** or **Arrays**.

**How it works:**

**Key Features**

#### Queues

- **Characteristic**: First-In-First-Out (FIFO)
- **Usage**: Ideal for ordered processing, such as print queues or BFS traversal.

#### Stacks

- **Characteristic**: Last-In-First-Out (LIFO)
- **Usage**: Perfect for tasks requiring reverse order like undo actions or DFS traversal.

#### Lists/Arrays

- **Characteristic**: Random Access
- **Usage**: Suitable when you need random access to elements or don't require strict order or data management.

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>What are Infix, Prefix, and Postfix notations?</summary>

### What are Infix, Prefix, and Postfix notations?

**Definition:**

In computer science, **infix**, **prefix**, and **postfix** notations are methods of writing mathematical expressions. While humans generally use infix notation, machines can more efficiently parse prefix and postfix notations.

**How it works:**

**Infix, Prefix and Postfix Notations**

- **Infix**: Operators are placed between operands. This is the most common notation for humans due to its intuitiveness.

  Example: $1 + 2$

- **Prefix**: Operators are placed before operands. The order of operations is determined by the position of the operator rather than parentheses.

  Example: $+ 1 \times 2 3$ which evaluates to $1 + (2 \times 3) = 7$

- **Postfix**: Operators are placed after operands. The order of operations is determined by the sequence in which operands and operators appear.

  Example: $1 2 3 \times +$ which evaluates to $1 + (2 \times 3) = 7$

**Relation to Stacks**

- **Conversion**: Stacks can facilitate the conversion of expressions from one notation to another. For instance, the Shunting Yard algorithm converts infix expressions to postfix notation using a stack.

- **Evaluation**: Both postfix and prefix expressions are evaluated using stacks. For postfix:
  1. Operands are pushed onto the stack.
  2. Upon encountering an operator, the required operands are popped, the operation is executed, and the result is pushed back.

  For example, for the expression $1 2 3 \times +$:
  - 1 is pushed onto the stack.
  - 2 is pushed.
  - 3 is pushed.
  - $\times$ is encountered. 3 and 2 are popped, multiplied to get 6, which is then pushed.
  - $+$ is encountered. 6 and 1 are popped, added to get 7, which is then pushed. This 7 is the result.

Evaluating prefix expressions follows a similar **stack-based method** but traverses the expression differently.

**Examples:**

**Code Example: Postfix Evaluation**

Here is the Python code:

```python
def evaluate_postfix(expression):
    stack = []
    tokens = expression.split()  # Handle multi-digit numbers
    for token in tokens:
        if token.isdigit():
            stack.append(int(token))
        else:
            operand2 = stack.pop()
            operand1 = stack.pop()
            stack.append(perform_operation(operand1, operand2, token))
    return stack[0]

def perform_operation(operand1, operand2, operator):
    operations = {
        '+': operand1 + operand2,
        '-': operand1 - operand2,
        '*': operand1 * operand2,
        '/': operand1 / operand2
    }
    return operations[operator]

# Example usage
print(evaluate_postfix('1 2 + 3 4 * -'))  # Output: -7
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Explain how Stacks are used in Function Call management in programming languages.</summary>

### Explain how Stacks are used in Function Call management in programming languages.

**Definition:**

When **functions** are called in programming languages, the system typically uses a **call stack** to manage the call sequence and memory allocation. Let's take a look at how this process works.

**How it works:**

**The Call Stack**

The **call stack** maintains a record of all the active function calls that a program makes. When a new function is called, it's added to the top of the stack. Once a function finishes its execution, it's removed from the stack, and control returns to the calling function.

This "last in, first out" behavior is well-suited to **stack** data structures.

**How the Call Stack Works**

1. **Function Call**: When a function is called, a **stack frame** is created and pushed onto the call stack. This frame contains important information about the state of the function, such as local variables and the return address, which points to the instruction after the function call.

2. **Local Execution**: The CPU executes the instructions within the called function. The function accesses its inputs, processes data, and calls other functions as needed.

3. **Return**: If the called function doesn't make any further function calls, it exits, and its stack frame is removed. Alternatively, if the function makes additional calls, the call stack grows further.

4. **Stack Unwinding**: Once the initial (or other topmost) function call is finished, there are no more functions to execute. The stack then shrinks, starting with the top frame, until it's empty.

**Pluses and minuses:**

**Benefits and Limitations of Call Stack**

#### Benefits

- **Automatic Memory Management**: The call stack automatically allocates memory for local variables and function parameters, simplifying memory management for developers.
- **Efficiency**: Its simple structure makes it efficient for managing function calls in most programming scenarios.

#### Limitations

- **Size Limitations**: The stack has a fixed memory size allocated at program startup, which can lead to stack overflow errors if the stack grows too large.
- **No Random Access**: Elements in the stack can only be accessed or removed in a last-in-first-out manner, limiting its use in some algorithms and data structures.

**Examples:**

**Code Example: Using the Call Stack**

Here is code written in Python:

```python
def multiply(a, b):
    result = a * b
    return result

def calculate(a, b, c):
    temp = multiply(b, c)
    return a + temp

result = calculate(2, 3, 4)
print(result)  # Output: 14
```

In this example, when `calculate` is called, it first calls `multiply` and then performs addition. The call stack looks like this during the execution of `calculate`:

1. `calculate` with parameters (2, 3, 4)
2. `multiply` with parameters (3, 4)

Once `multiply` completes, its stack frame is removed, and `calculate` continues with the next line of code.

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Describe an application where Stacks are naturally suited over other data structures.</summary>

### Describe an application where Stacks are naturally suited over other data structures.

**Definition:**

**Stacks** find natural utility in various practical use-cases, such as in text editors for tracking actions and providing the "undo" and "redo" functionalities.

**How it works:**

**Stack-Based Undo and Redo Workflow**

In a typical text editor, the user can:

- **Type Text**: Each time new text is entered, it represents an **action** that can be **undone** or **redone**.
- **Perform Undo/Redo**: The editor navigates through previous actions, whether to reverse or reinstate them.

**Examples:**

**Code Example: Undo and Redo Stack**

Here is the Python code:

```python
class UndoRedoStack:
    def __init__(self):
        self._undo_stack = []
        self._redo_stack = []

    def push(self, action):
        self._undo_stack.append(action)
        # When a new action is pushed, the redo stack needs to be reset
        self._redo_stack = []

    def undo(self):
        if self._undo_stack:
            action = self._undo_stack.pop()
            self._redo_stack.append(action)
            return action

    def redo(self):
        if self._redo_stack:
            action = self._redo_stack.pop()
            self._undo_stack.append(action)
            return action
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

### Stack Implementation Details

<details>
<summary>Compare Array-based vs Linked List stack implementations.</summary>

### Compare Array-based vs Linked List stack implementations.

**Definition:**

**Array-based stacks** excel in time efficiency and direct element access. In contrast, **linked list stacks** are preferable for dynamic sizing and easy insertions or deletions.

**How it works:**

**Common Features**

- **Speed of Operations**: Both `pop` and `push` are $O(1)$ operations.
- **Memory Use**: Both have $O(n)$ space complexity.
- **Flexibility**: Both can adapt their sizes, but their resizing strategies differ.

**Key Distinctions**

#### Array-Based Stack

- **Locality**: Consecutive memory locations benefit CPU caching.
- **Random Access**: Provides direct element access.
- **Iterator Needs**: Preferable if indexing or iterators are required.
- **Performance**: Slightly faster for top-element operations and potentially better for time-sensitive tasks due to caching.
- **Push**: $O(1)$ on average; resizing might cause occasional $O(n)$.

#### Linked List Stack

- **Memory Efficiency**: Better suited for fluctuating sizes and limited memory scenarios.
- **Resizing Overhead**: No resizing overheads.
- **Pointer Overhead**: Requires extra memory for storing pointers.

**Examples:**

**Code Example: Array-Based Stack**

Here is the Python code:

```python
class ArrayBasedStack:
    def __init__(self):
        self.stack = []
    def push(self, item):
        self.stack.append(item)
    def pop(self):
        return self.stack.pop() if self.stack else None
```

**Code Example: Linked List Stack**

Here is the Python code:

```python
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None
class LinkedListStack:
    def __init__(self):
        self.head = None
    def push(self, item):
        new_node = Node(item)
        new_node.next = self.head
        self.head = new_node
    def pop(self):
        if self.head:
            temp = self.head
            self.head = self.head.next
            return temp.data
        return None
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Implement a Dynamic Stack that automatically resizes itself.</summary>

### Implement a Dynamic Stack that automatically resizes itself.

**Definition:**

**Problem Statement**

Implement a **Dynamic Stack** that automatically resizes itself when it reaches its capacity.

**How it works:**

**Solution**

Resizing a stack involves two main operations: **shrinking** and **expanding** the stack when needed. A common strategy is to **double the stack's size** each time it reaches full capacity and **halve** it when it becomes 25% full, as this provides efficient amortized performance.

#### Key Operations

1. `push(item)`: Add an item to the stack.
2. `pop()`: Remove and return the top item from the stack.
3. `is_full()`: Check if the stack is full.
4. `is_empty()`: Check if the stack is empty.
5. `expand()`: Double the stack's capacity.
6. `shrink()`: Halve the stack's capacity.

#### Algorithm Steps

1. Start with an initial capacity for the stack. In this example, it's `2`.
2. Whenever a `push` operation encounters a full stack, call the `expand` method before the addition.
3. Whenever a `pop` operation leaves the stack 25% full, call the `shrink` method.

This ensures the stack dynamically adjusts its size based on the current number of elements.

#### Complexity Analysis

- **Time Complexity**:
  - `push`: $O(1)$ amortized. Although `expand` can take up to $O(n)$ time, it is only triggered once every $n$ `push` operations, resulting in an average of $O(1)$ per `push`.
  - `pop`, `is_full`, and `is_empty`: $O(1)$.
  - `expand` and `shrink`: $O(n)$ in the worst case, but they are infrequently called, so their amortized time is $O(1)$ per operation.

- **Space Complexity**: $O(n)$ where $n$ is the number of elements in the stack. This accounts for the stack itself and any additional overhead such as the temporary arrays used during resizing.

**Examples:**

**Implementation**

Here is the Python code:

```python
class DynamicStack:
    # Initialize the stack with an initial capacity
    def __init__(self, capacity=2):
        self.capacity = capacity
        self.stack = [None] * capacity
        self.top = -1

    # Push an element to the stack
    def push(self, item):
        if self.is_full():
            self.expand()
        self.top += 1
        self.stack[self.top] = item

    # Pop the top element from the stack and return it
    def pop(self):
        if self.is_empty():
            raise IndexError('Stack is empty')
        item = self.stack[self.top]
        self.top -= 1
        if self.top < self.capacity // 4:
            self.shrink()
        return item

    # Check if the stack is full
    def is_full(self):
        return self.top == self.capacity - 1

    # Check if the stack is empty
    def is_empty(self):
        return self.top == -1

    # Double the stack's capacity
    def expand(self):
        self.capacity *= 2
        new_stack = [None] * self.capacity
        for i in range(self.top + 1):
            new_stack[i] = self.stack[i]
        self.stack = new_stack

    # Halve the stack's capacity
    def shrink(self):
        self.capacity //= 2
        new_stack = [None] * self.capacity
        for i in range(self.top + 1):
            new_stack[i] = self.stack[i]
        self.stack = new_stack
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>What are the performance implications of a Fixed-size Array Stack Implementation?</summary>

### What are the performance implications of a Fixed-size Array Stack Implementation?

**Definition:**

While **fixed-size array stacks** offer simplicity and often better performance for certain operations, such as data caching and real-time processing, several limitation are to be considered.

**How it works:**

**Space and Memory Management**

- **Limited Capacity**: Fixed-size arrays impose a maximum capacity for stacks, introducing the potential for overflow.
- **Pre-allocated Memory**: Fixed-size arrays require memory to be allocated in advance for the maximum capacity, leading to potential inefficiencies if this capacity is not fully utilized.
- **Consistent Size**: Stacks using fixed-size arrays do not auto-resize, leading to inefficient memory use if the actual size of the stack varies significantly from the allocated size.

**Time Complexity of Fixed-Size Array Stacks**

- **Push Operation**: $O(1)$ (constant time), until the array is full and a resizing operation is initiated which leads to $O(n)$ in the worst case.
- **Pop Operation**: $O(1)$ - simple memory deallocation or index decrement.
- **Peek Operation**: $O(1)$ - equivalent to pop.
- **Search Operation**: $O(n)$ - in the worst case, when the element is at the top of the stack or not present.

**Data Structure Sensitivity**

- **Space Sensitivity**: Stacks using a fixed-size array have predictable, constant memory requirements.
- **Performance Sensitivity**: While operations on non-fixed size stacks might have $O(1)$ average-case time complexity parameters, certain operations on fixed-size stacks can degrade in worst-case scenarios, justifying the $O(n)$ worst-case complexity.

**When to use:**

**Practical Applications**

- **Real-Time Systems**: Fixed-size arrays can be preferable for applications with strict timing requirements, as memory operations are more deterministic.
- **Embedded Systems**: In resource-constrained environments, using fixed-size arrays can help manage memory more efficiently due to their predictable memory requirements.
- **Cache Systems**: The use of fixed-size arrays is significant in caches, where the predictability of space requirements is essential.

**Examples:**

**Code Example: Fixed-Size Stack**

Here is the Python code:

```python
class FixedSizeStack:
    def __init__(self, capacity=10):
        self.stack = [None] * capacity
        self.top = -1

    def push(self, value):
        if self.top == len(self.stack) - 1:
            print("Stack is full, cannot push.")
            return
        self.top += 1
        self.stack[self.top] = value

    def pop(self):
        if self.top == -1:
            print("Stack is empty, cannot pop.")
            return
        value = self.stack[self.top]
        self.top -= 1
        return value

    def peek(self):
        if self.top == -1:
            print("Stack is empty, no top element.")
            return
        return self.stack[self.top]

    def is_empty(self):
        return self.top == -1

    def is_full(self):
        return self.top == len(self.stack) - 1
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

### Stack Design Challenges

<details>
<summary>Design a Stack that supports Retrieving the min element in O(1).</summary>

### Design a Stack that supports Retrieving the min element in O(1).

**Definition:**

**Problem Statement**

The goal is to design a **stack** data structure that can efficiently retrieve both the minimum element and the top element in $O(1)$ time complexity.

**How it works:**

**Solution**

To meet the time complexity requirement, we'll maintain two stacks:

1. **Main Stack** for standard stack functionality.
2. **Auxiliary Stack** that keeps track of the minimum element up to a given stack position.

#### Algorithm Steps

1. **Pop** and **Push**
   - For each element $e$ in the **Main Stack**, check if it's smaller than or equal to the top element in the **Auxiliary Stack**. If $e$ is the new minimum, push it onto both stacks.

2. **Minimum Element Retrieval**: The top element of the **Auxiliary Stack** will always be the minimum element of the main stack.

#### Complexity Analysis

- **Time Complexity**: $O(1)$ for all operations.
- **Space Complexity**: $O(N)$, where $N$ is the number of elements in the stack.

**Examples:**

**Implementation**

Here is the Python code:

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, element):
        self.stack.append(element)
        if not self.min_stack or element <= self.min_stack[-1]:
            self.min_stack.append(element)

    def pop(self):
        if not self.stack:
            return None
        top = self.stack.pop()
        if top == self.min_stack[-1]:
            self.min_stack.pop()
        return top

    def top(self):
        return self.stack[-1] if self.stack else None

    def getMin(self):
        return self.min_stack[-1] if self.min_stack else None
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>How can you design a Stack to be thread-safe?</summary>

### How can you design a Stack to be thread-safe?

**Definition:**

Ensuring **thread safety** in a traditional stack, where operations are based on a last-in, first-out (LIFO) approach, can be achieved through a variety of techniques. I will give examples of three different approaches here:

1. **Locking Mechanism**: Where a thread synchronizes access through synchronization techniques like locks.

2. **Non-Blocking Mechanism**: Using atomic operations without explicit locks.

3. **Data Structure Selection**: Choosing inherently thread-safe data structures that mimic stack operations.

**How it works:**

**Locking Mechanism**

This approach synchronizes operations on the stack using a lock. While a thread holds the lock, other threads are blocked from entering critical sections.

**Non-Blocking Mechanism**

This approach uses atomic operations on **primitive data types**, which are guaranteed to happen without interruption.

For the **Multi-threaded** Setup, you can run this Python code:

**Data Structure Selection**

Some **container classes**, like the LifoQueue in Python, are inherently designed to be thread-safe, thus making their contents and operations secure in a multi-threaded environment.

**Examples:**

**Locking Mechanism**

Here is the Python code:

```python
import threading

class LockedStack:
    def __init__(self):
        self.stack = []
        self.lock = threading.Lock()

    def push(self, item):
        with self.lock:
            self.stack.append(item)

    def pop(self):
        with self.lock:
            if self.stack:
                return self.stack.pop()
            return None
```

**Non-Blocking Mechanism**

Here is the Python code for this approach:

```python
import queue
import threading

def non_blocking_push(q, item):
    while True:
        old_q = q.queue
        new_q = old_q.copy()
        new_q.append(item)

        if q.queue == old_q:
            q.queue = new_q
            return

def non_blocking_pop(q):
    while True:
        old_q = q.queue
        new_q = old_q.copy()
        if q.queue:
            new_q.pop()
            if q.queue == old_q:
                q.queue = new_q
                return q.queue[-1]
    return None

q = queue.LifoQueue()

threading.Thread(target=non_blocking_push, args=(q, 1)).start()
threading.Thread(target=non_blocking_push, args=(q, 2)).start()
threading.Thread(target=non_blocking_pop, args=(q,)).start()
```

**Data Structure Selection**

Here is the Python code:

```python
import queue
import threading

q = queue.LifoQueue()

q.put(1)
q.put(2)
print(q.get())
print(q.get())
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Implement a Stack with a Find-Middle operation in O(1) time.</summary>

### Implement a Stack with a Find-Middle operation in O(1) time.

**Definition:**

**Problem Statement**

The task is to design a **_stack_** data structure that supports **`push`**, **`pop`**, **`findMiddle`**, and **`deleteMiddle`** operations, all in constant $O(1)$ time complexity.

**How it works:**

**Solution**

We can solve this challenge using a doubly linked list where each node also includes a pointer to the middle node. This solution, though not built on arrays, has a clear control flow and keeps a consistent time complexity.

#### Algorithm Steps

1. Initialize an empty stack and set `middle` to `NULL`.
2. During the push operation, update the `middle` pointer based on the current number of nodes. If the number of nodes is odd, `middle` moves up, otherwise, it stays at the same position.
3. The pop operation, on top of removing the item, also adjusts the `middle` pointer if the removed node was pointing to it.
4. `findMiddle` and `deleteMiddle` simply involve accessing or manipulating the node that `middle` points to.

#### Complexity Analysis

- **Time Complexity**:
  - `push`: $O(1)$ -  Constant time for every node insertion.
  - `pop`: $O(1)$ -  Always removes the top node in a constant time regardless of the stack size.
  - `findMiddle` and `deleteMiddle`: $O(1)$ - Directly accessed through the `middle` pointer.
- **Space Complexity**: $O(n)$ - Additional space is required for the pointers associated with each node.

**Examples:**

**Implementation**

Here is the Python code:

```python
class DLLNode:
    def __init__(self, x):
        self.data = x
        self.next = None
        self.prev = None

class MyStack:
    def __init__(self):
        self.head = None
        self.mid = None
        self.count = 0

    def push(self, x):
        new_node = DLLNode(x)
        new_node.next = self.head

        if self.count == 0:
            self.mid = new_node
        else:
            self.head.prev = new_node
            if self.count % 2 != 0:
                self.mid = self.mid.prev

        self.head = new_node
        self.count += 1

    def pop(self):
        if self.count == 0:
            print("Stack is empty")
            return

        item = self.head.data
        self.head = self.head.next

        if self.head is not None:
            self.head.prev = None

        self.count -= 1

        # Update mid if the removed node was the middle one
        if self.count % 2 == 0:
            self.mid = self.mid.next

        return item

    def findMiddle(self):
        if self.count == 0:
            print("Stack is empty")
            return
        return self.mid.data

    def deleteMiddle(self):
        if self.count == 0:
            print("Stack is empty")
            return
        temp = self.mid
        self.mid.prev.next = self.mid.next
        self.mid.next.prev = self.mid.prev
        if self.count % 2 != 0:
            self.mid = self.mid.prev
        else:
            self.mid = self.mid.next
        del temp
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

### Stack and Other Data Structures

<details>
<summary>Implement a Linked List using Stack.</summary>

### Implement a Linked List using Stack.

**Definition:**

**Problem Statement**

The task is to implement **linked list** using a **stack**.

**How it works:**

**Solution**

**Linked lists** are usually built with nodes, each containing a value and a pointer to the next node. However, you can also **simulate linked lists using stacks**, which follow a "last in, first out" (LIFO) mechanism.

#### Algorithm Steps

1. **Initialize**: Two stacks: `list_stack` and `temp_stack`.
2. **Add to Head**: Push to `list_stack`.
3. **Remove from Head**: Pop from `list_stack`.
4. **Insert**: Pop items to `temp_stack` until the insertion point, then push the new item and the `temp_stack` items back to `list_stack`.
5. **Delete**: Similar to insert but pop the node to be deleted before pushing items back.

#### Complexity Analysis

- **Time Complexity**:
  - Add to Head: $O(1)$
  - Remove from Head: $O(1)$
  - Insert at position $k$: $O(k)$
  - Delete at position $k$: $O(k)$

- **Space Complexity**:
  - $O(n)$ for `list_stack`, where $n$ is the number of elements.
  - $O(k)$ for `temp_stack` during insert or delete operations at position $k$, with an overall space complexity still being $O(n)$ in the worst case (when $k = n$).

**Examples:**

**Implementation**

Here is the Python code:

```python
class LinkedListStack:
    def __init__(self):
        self.list_stack, self.temp_stack = [], []

    def push(self, data):
        self.list_stack.append(data)

    def pop(self):
        return self.list_stack.pop()

    def insert(self, data, pos):
        while pos:
            self.temp_stack.append(self.list_stack.pop())
            pos -= 1
        self.list_stack.append(data)
        while self.temp_stack:
            self.list_stack.append(self.temp_stack.pop())

    def delete(self, pos):
        while pos:
            self.temp_stack.append(self.list_stack.pop())
            pos -= 1
        self.list_stack.pop()
        while self.temp_stack:
            self.list_stack.append(self.temp_stack.pop())

    def display(self):
        print(self.list_stack)

# Example
ll_stack = LinkedListStack()
ll_stack.push(1)
ll_stack.push(2)
ll_stack.push(3)
ll_stack.insert(10, 1)
print("Before deletion:")
ll_stack.display()
ll_stack.delete(2)
print("After deletion:")
ll_stack.display()
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Implement Doubly Linked List using Stacks with min complexity.</summary>

### Implement Doubly Linked List using Stacks with min complexity.

**Definition:**

**Problem Statement**

The task is to implement a **Doubly LinkedList** using **Stacks**.

**How it works:**

**Solution**

Using two stacks, `forwardStack` and `backwardStack`, we can emulate a **doubly linked list**.

- **Insertion**:
  - Beginning: Transfer elements from `backwardStack` to `forwardStack`, then push the new element onto `backwardStack`.
  - End: Transfer from `forwardStack` to `backwardStack` and push the new element onto `backwardStack`.

- **Traversal**:
  - Forward: Pop from `backwardStack`, push onto `forwardStack`.
  - Backward: Pop from `forwardStack`, push onto `backwardStack`.

- **Deletion**: Traverse the needed stack to locate and remove the desired element, whether it's a specific item or the first/last entry.

#### Complexity Analysis

- **Time Complexity**:
  - Operations like `insertInBeginning`, `insertAtEnd`, `moveForward`, `moveBackward`, `delete`, `deleteFirst`, and `deleteLast` have a worst-case time complexity of $O(n)$ due to the potential full traversal of a stack.

- **Space Complexity**: $O(n)$, where $n$ is the number of nodes in the doubly linked list, primarily occupied by the two stacks.

**Examples:**

**Implementation**

Here is the Java code:

```java
import java.util.Stack;

class DoubleLinkedList {
    private Stack<Integer> forwardStack;
    private Stack<Integer> backwardStack;

    public DoubleLinkedList() {
        forwardStack = new Stack<>();
        backwardStack = new Stack<>();
    }

    public void insertInBeginning(int data) {
        // Move all elements to forwardStack to insert at the beginning
        while (!backwardStack.isEmpty()) {
            forwardStack.push(backwardStack.pop());
        }
        backwardStack.push(data);
    }

    public void insertAtEnd(int data) {
        // Move all elements to backwardStack to insert at the end
        while (!forwardStack.isEmpty()) {
            backwardStack.push(forwardStack.pop());
        }
        backwardStack.push(data);
    }

    public void moveForward() {
        if (backwardStack.isEmpty()) {
            System.out.println("No more elements to move forward.");
            return;
        }
        System.out.println("Moving forward: " + backwardStack.peek());
        forwardStack.push(backwardStack.pop());
    }

    public void moveBackward() {
        if (forwardStack.isEmpty()) {
            System.out.println("No more elements to move backward.");
            return;
        }
        System.out.println("Moving backward: " + forwardStack.peek());
        backwardStack.push(forwardStack.pop());
    }

    public void delete(int data) {
        Stack<Integer> tempStack = new Stack<>();
        boolean deleted = false;

        while (!backwardStack.isEmpty()) {
            if (backwardStack.peek() == data && !deleted) {
                backwardStack.pop();
                deleted = true;
                break;
            }
            tempStack.push(backwardStack.pop());
        }

        while (!tempStack.isEmpty()) {
            backwardStack.push(tempStack.pop());
        }

        if (!deleted) {
            System.out.println("Element not found.");
        }
    }

    public void deleteFirst() {
        if (backwardStack.isEmpty()) {
            System.out.println("List is empty.");
            return;
        }
        backwardStack.pop();
    }

    public void deleteLast() {
        while (!forwardStack.isEmpty()) {
            backwardStack.push(forwardStack.pop());
        }
        if (!backwardStack.isEmpty()) {
            backwardStack.pop();
        } else {
            System.out.println("List is empty.");
        }
    }
}
```

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Implement a Queue using Two Stacks.</summary>

### Implement a Queue using Two Stacks.

**Definition:**

A queue can be built from two stacks. One published arrangement makes enqueue O(n) and dequeue O(1). The other makes enqueue O(1) and dequeue O(n). Together the two stacks hold at most n elements, so the extra space is O(n).

**How it works:**

Costly enqueue: move everything from s1 to s2, push x onto s1, move s2 back. The front of the queue is then the top of s1, so dequeue and front are a pop or a peek. Costly dequeue: push onto s1 in O(1). When s2 is empty, pour s1 into s2 and pop s2. An empty front returns -1.

**Examples:**

```python
class Queue:
    def __init__(self):
        self.s1, self.s2 = [], []

    def enqueue(self, x):
        self.s1.append(x)  # O(1)

    def dequeue(self):
        if not self.s2:
            while self.s1:
                self.s2.append(self.s1.pop())
        if not self.s2:
            return None
        return self.s2.pop()  # O(n) when s2 was empty
```

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Implement a Queue using only One Stack.</summary>

### Implement a Queue using only One Stack.

**Definition:**

The same page builds a queue from one stack plus recursion. The call stack holds elements while the algorithm reaches the bottom of the explicit stack, which is the front of the queue.

**How it works:**

enqueue pushes on top, so the top is the rear. dequeue pops x, recurses until the stack is empty (that last value is the front and is not pushed back), then pushes x again on the way out. front does the same walk and pushes the bottom value back. An empty front returns -1. The page does not publish a separate Big-O line for this approach.

**Examples:**

```python
class Queue:
    def __init__(self):
        self.s = []

    def enqueue(self, x):
        self.s.append(x)

    def dequeue(self):
        if not self.s:
            return None
        x = self.s.pop()
        if not self.s:
            return x
        front = self.dequeue()
        self.s.append(x)
        return front
```

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Implement Stack using Two Queues with efficient push.</summary>

### Implement Stack using Two Queues with efficient push.

**Definition:**

The efficient-push arrangement is push in O(1) and pop in O(n), using two queues.

**How it works:**

push enqueues x into q1. pop moves every element except the last from q1 to q2, removes that last element (the stack top), then swaps the queue names so q1 holds the remaining items. top does the same walk but enqueues the last element into q2 instead of discarding it. An empty top returns -1. size is the length of q1.

**When to use:**

Use this when push is the hot operation. The other arrangement on the same page makes push O(n) and pop O(1).

**Examples:**

```python
from collections import deque

class Stack:
    def __init__(self):
        self.q1, self.q2 = deque(), deque()

    def push(self, x):
        self.q1.append(x)

    def pop(self):
        if not self.q1:
            return None
        while len(self.q1) != 1:
            self.q2.append(self.q1.popleft())
        value = self.q1.popleft()
        self.q1, self.q2 = self.q2, self.q1
        return value
```

**Sources:**

- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

<details>
<summary>Implement Three Stacks with One Array.</summary>

### Implement Three Stacks with One Array.

**Definition:**

The fetched stack pages do not publish a three-stacks-in-one-array layout. They do show one explicit stack as a list whose top is the last index.

**How it works:**

Give each stack its own slice of one array and its own top index: `[0, n//3)`, `[n//3, 2*n//3)`, `[2*n//3, n)`. Push writes at that stack’s top and moves the top forward only while it stays inside the slice. Pop moves the top back. This fixed split is not given a Big-O line on the fetched pages; a push that stays inside its slice is one write, the same single-slot update the list-backed stacks use.

**Examples:**

```python
class ThreeStacks:
    def __init__(self, n):
        self.a = [None] * n
        self.n = n
        self.top = [-1, n // 3 - 1, 2 * n // 3 - 1]
        self.bound = [n // 3, 2 * n // 3, n]

    def push(self, which, x):
        if self.top[which] + 1 >= self.bound[which]:
            raise IndexError("full")
        self.top[which] += 1
        self.a[self.top[which]] = x
```

**Sources:**

- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

### Stack Manipulation and Variation

<details>
<summary>Reverse a String using Stack.</summary>

### Reverse a String using Stack.

**Definition:**

A stack scan of a string on the parentheses page is O(n) time and O(n) space. Pushing every character and then popping them writes the string in reverse, because the last push is the first pop.

**How it works:**

Walk the string once and append each character. Then pop until the stack is empty and append each popped character to the result. The page’s stack of brackets is the same shape: one slot per character still on the stack, and the scan finishes when the input is consumed.

**Examples:**

```python
def reverse_string(s):
    stack = []
    for ch in s:
        stack.append(ch)
    out = []
    while stack:
        out.append(stack.pop())
    return "".join(out)
```

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Reverse a Stack without using any additional data structure.</summary>

### Reverse a Stack without using any additional data structure.

**Definition:**

The recursive sort on the sort-a-stack page reverses the role of extra storage: it uses no second stack object. The auxiliary space is O(n) for the call stack, and the time is O(n²).

**How it works:**

That page’s reverse-order insert is `sortedInsert`: pop the top, recurse to the bottom, then push values back so a new value sits in order. The same pop-recurse-push walk reaches the bottom without a second stack object. The page’s other method, an auxiliary stack, is the one that does allocate a second stack and is also O(n²) time and O(n) extra space. The no-extra-structure method is the recursive one.

**Examples:**

```python
def insert_at_bottom(st, x):
    if not st:
        st.append(x)
        return
    top = st.pop()
    insert_at_bottom(st, x)
    st.append(top)

def reverse_stack(st):
    if not st:
        return
    top = st.pop()
    reverse_stack(st)
    insert_at_bottom(st, top)
```

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>Sort a Stack using Recursion.</summary>

### Sort a Stack using Recursion.

**Definition:**

Recursive sort of a stack is O(n²) time and O(n) auxiliary space. The largest value ends on top.

**How it works:**

`sortStack` pops the top, sorts the rest, then `sortedInsert` puts that value back. `sortedInsert` pushes immediately when the stack is empty or the top is less than or equal to the value. Otherwise it pops the top, inserts, and pushes the top back. Input `[41, 3, 32, 2, 11]` prints from the top as `[41, 32, 11, 3, 2]`.

**Examples:**

```python
def sorted_insert(st, x):
    if not st or st[-1] <= x:
        st.append(x)
        return
    top = st.pop()
    sorted_insert(st, x)
    st.append(top)

def sort_stack(st):
    if not st:
        return
    top = st.pop()
    sort_stack(st)
    sorted_insert(st, top)
```

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>Sort a Stack using another Stack.</summary>

### Sort a Stack using another Stack.

**Definition:**

Sorting with a second stack is O(n²) time and O(n) auxiliary space on the same page. The largest element finishes on top.

**How it works:**

Pop x from the input. While the temporary stack is not empty and its top is smaller than x, move that top back to the input. Push x onto the temporary stack. After the input is empty, move the temporary stack back. The temporary stack is what keeps the sorted order.

**Examples:**

```python
def sort_stack(st):
    temp = []
    while st:
        x = st.pop()
        while temp and temp[-1] < x:
            st.append(temp.pop())
        temp.append(x)
    while temp:
        st.append(temp.pop())
```

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>Implement a Shuffling Algorithm using a Stack.</summary>

### Implement a Shuffling Algorithm using a Stack.

**Definition:**

None of the fetched stack pages publishes a shuffle. A stack reverses order; it does not draw a random permutation.

**How it works:**

The queue page uses stacks because a plain stack is LIFO, and the sort page’s second stack only moves values into sorted order. A shuffle needs a source of random indexes, which those pages do not define. Using the stack as a reverse step would put the last element first, which is one fixed permutation, not a shuffle.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Stack in Parsing and Expression Evaluation

<details>
<summary>Check if parentheses are Balanced using Stack.</summary>

### Check if parentheses are Balanced using Stack.

**Definition:**

A stack check of `(){}[]` is O(n) time and O(n) space. The string is balanced when every close matches the latest open and the stack is empty at the end.

**How it works:**

Push `(`, `{`, and `[`. On a close, if the stack is empty or the top is not the matching open, return false and pop otherwise. Ignore nothing else in the published checker: a non-bracket character is skipped by the `if`/`else` on brackets. `[()()]{}` is true. `([{]})` is false because `]` arrives before `{` is closed.

**Examples:**

```python
def is_balanced(s):
    st = []
    pair = {")": "(", "}": "{", "]": "["}
    for c in s:
        if c in "({[":
            st.append(c)
        elif c in ")}]":
            if not st or st[-1] != pair[c]:
                return False
            st.pop()
    return not st
```

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Find Duplicate Parenthesis in an expression.</summary>

### Find Duplicate Parenthesis in an expression.

**Definition:**

The parentheses page does not define duplicate parentheses. Its stack is still the tool: opens are pushed, and a close must find a matching open or the expression is unbalanced, in O(n) time and O(n) space.

**How it works:**

A duplicate pair is the extra case of that scan. Push operators as well as opens. On `)`, pop until `(`. If the pop removes nothing but the open, the parentheses wrapped an empty pair. `((a+b))` has that empty wrap; `(a+b)` does not, because `+` sits between the parentheses. The page’s own examples stop at balance, so this empty-pop test is the extra check.

**Examples:**

```python
def has_duplicate(expr):
    st = []
    for c in expr:
        if c == ")":
            count = 0
            while st and st[-1] != "(":
                st.pop()
                count += 1
            if not st:
                return False
            st.pop()
            if count == 0:
                return True
        else:
            st.append(c)
    return False
```

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>How can a Stack be used to evaluate Postfix expressions?</summary>

### How can a Stack be used to evaluate Postfix expressions?

**Definition:**

The infix page defines postfix as operand, operand, operator, and its conversion scan is O(n) time and O(n) space. Evaluation walks that postfix string with a stack of values.

**How it works:**

Operands are pushed. An operator pops the right operand, then the left, applies the operator, and pushes the result. `abc*+` for `a+b*c` leaves the sum on the stack. The page does not publish a separate timing line for the evaluation walk; the conversion that produces the postfix form is the O(n) scan.

**Examples:**

```python
def eval_postfix(tokens):
    st = []
    for tok in tokens:
        if tok in "+-*/":
            right = st.pop()
            left = st.pop()
            if tok == "+":
                st.append(left + right)
            elif tok == "-":
                st.append(left - right)
            elif tok == "*":
                st.append(left * right)
            else:
                st.append(left / right)
        else:
            st.append(float(tok))
    return st[-1]
```

**Sources:**

- [Infix to Postfix](https://www.geeksforgeeks.org/dsa/convert-infix-expression-to-postfix-expression/)
</details>

<details>
<summary>Convert an Infix expression to Postfix using a Stack.</summary>

### Convert an Infix expression to Postfix using a Stack.

**Definition:**

Converting infix to postfix with a stack is O(n) time and O(n) space. `a*(b+c)/d` becomes `abc+*d/`.

**How it works:**

Operands go straight to the output. `(` is pushed. `)` pops until `(`. An operator pops while the top has higher precedence, or equal precedence and the new operator is left-associative (`+`, `-`, `*`, `/`). `^` is right-associative, precedence 3, so an equal `^` stays on the stack. `*` and `/` are precedence 2. `+` and `-` are precedence 1. Whatever remains is popped at the end.

**Examples:**

```python
def infix_to_postfix(s):
    prec = {"^": 3, "*": 2, "/": 2, "+": 1, "-": 1}
    st, res = [], []
    for c in s:
        if c.isalnum():
            res.append(c)
        elif c == "(":
            st.append(c)
        elif c == ")":
            while st[-1] != "(":
                res.append(st.pop())
            st.pop()
        else:
            while st and st[-1] != "(" and (
                prec[st[-1]] > prec[c] or (prec[st[-1]] == prec[c] and c != "^")
            ):
                res.append(st.pop())
            st.append(c)
    while st:
        res.append(st.pop())
    return "".join(res)
```

**Sources:**

- [Infix to Postfix](https://www.geeksforgeeks.org/dsa/convert-infix-expression-to-postfix-expression/)
</details>

<details>
<summary>Build a Binary Expression Tree for the given expression.</summary>

### Build a Binary Expression Tree for the given expression.

**Definition:**

The infix page does not build a tree. It does produce postfix in O(n) time and O(n) space, and postfix is operand, operand, operator.

**How it works:**

After that conversion, scan the postfix form with a stack of nodes. An operand becomes a leaf and is pushed. An operator pops the right child, then the left child, and pushes a node with those children. The last node is the root. `a*(b+c)/d` converts to `abc+*d/` on the page, so the root operator is `/`.

**Examples:**

```python
class Node:
    def __init__(self, value, left=None, right=None):
        self.value, self.left, self.right = value, left, right

def tree_from_postfix(postfix):
    st = []
    for c in postfix:
        if c in "+-*/^":
            right, left = st.pop(), st.pop()
            st.append(Node(c, left, right))
        else:
            st.append(Node(c))
    return st[-1]
```

**Sources:**

- [Infix to Postfix](https://www.geeksforgeeks.org/dsa/convert-infix-expression-to-postfix-expression/)
</details>

### Handling Large Datasets with Stack

<details>
<summary>Given an Extremely Large File that doesn’t fit into memory, check if the parentheses are Balanced.</summary>

### Given an Extremely Large File that doesn’t fit into memory, check if the parentheses are Balanced.

**Definition:**

The parentheses checker is O(n) time. Its O(n) space is the stack of unmatched opens, which is the nesting depth, not a second copy of a file that has already been read as one string.

**How it works:**

Read one character at a time and run the same rules: push an open, and on a close either fail or pop the match. You do not store the file. The worst-case stack is still O(n) when every character is an unmatched open, which is the space line on the page. The page’s other method rewrites a mutable string in place and is O(1) extra space, and it says that in-place rewrite is not suitable for Java, Python, C#, or JavaScript because those strings are immutable.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Check for Braces Balance in a really large (1T) file in Parallel.</summary>

### Check for Braces Balance in a really large (1T) file in Parallel.

**Definition:**

The published checker is one left-to-right scan. A close is matched against the latest unmatched open, so the decision at each character depends on the stack built from the prefix.

**How it works:**

The page does not publish a parallel split of a 1T file. Splitting the file into independent chunks would drop the unmatched opens that a later chunk’s close must see. The sequential scan is O(n) time with an O(n) stack in the worst case.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Design a script to process large-scale, real-time Undo Operations using Stacks.</summary>

### Design a script to process large-scale, real-time Undo Operations using Stacks.

**Definition:**

Undo is LIFO: the last recorded action is the first one reversed. That is the order a stack already has, which is why the queue page has to add a second stack or recursion to get FIFO.

**How it works:**

Push each completed action. Undo pops one action and runs its inverse. The parentheses and sort pages bound a stack of n items as O(n) extra space. The pages do not publish a real-time scheduler or a durability format for those actions.

**Examples:**

```python
undo_stack = []

def do(action, inverse):
    action()
    undo_stack.append(inverse)

def undo():
    if undo_stack:
        undo_stack.pop()()
```

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>How would a Stack handle Memory Allocation in large data scenarios?</summary>

### How would a Stack handle Memory Allocation in large data scenarios?

**Definition:**

The stack algorithms that store one entry per input item use O(n) extra space: the parentheses stack, the next-greater stack, the stock-span stack, and the auxiliary sort stack.

**How it works:**

That space is allocated as the stack grows, one push at a time, up to n entries. The recursive sort also uses O(n) auxiliary space, and that space is the call stack rather than a second container. The in-place parentheses rewrite is the page’s O(1) extra-space alternative, and only for a mutable string.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
- [Next Greater Element](https://www.geeksforgeeks.org/dsa/next-greater-element/)
- [Stock Span Problem](https://www.geeksforgeeks.org/dsa/the-stock-span-problem/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Advanced Stack Problems

<details>
<summary>Solve the Stock Span Problem using a Stack.</summary>

### Solve the Stock Span Problem using a Stack.

**Definition:**

The stack solution is O(n) time and O(n) space. The span of day i is the number of consecutive days ending at i whose price is less than or equal to price i. For `[10, 4, 5, 90, 120, 80]` the spans are `[1, 1, 2, 4, 5, 1]`.

**How it works:**

Keep a stack of indexes. For each day, pop while the stack’s price is less than or equal to today’s price. If the stack is empty, the span is `i + 1`. Otherwise it is `i - top`. Then push i. The nested-loop version on the same page is O(n²) time and O(1) extra space.

**Examples:**

```python
def calculate_span(arr):
    span = [0] * len(arr)
    st = []
    for i, price in enumerate(arr):
        while st and arr[st[-1]] <= price:
            st.pop()
        span[i] = i + 1 if not st else i - st[-1]
        st.append(i)
    return span
```

**Sources:**

- [Stock Span Problem](https://www.geeksforgeeks.org/dsa/the-stock-span-problem/)
</details>

<details>
<summary>Design an algorithm to Calculate Rainwater Trapped between towers using a Stack.</summary>

### Design an algorithm to Calculate Rainwater Trapped between towers using a Stack.

**Definition:**

The fetched pages do not give a rainwater formula. The stock-span page does publish the monotonic stack those formulas are built from: pop while the top is less than or equal to the current value, in O(n) time and O(n) space, because each index is pushed once.

**How it works:**

A stack of bar indexes can use that same pop-while-lower walk. The span page’s own result is `i - previousGreater`, not a water volume. Do not replace that result with a volume the page does not state. The nested-loop span is the O(n²) alternative when a stack is not used.

**Sources:**

- [Stock Span Problem](https://www.geeksforgeeks.org/dsa/the-stock-span-problem/)
</details>

### Stack Performance Optimization

<details>
<summary>Explain how to minimize Stack Overflow in programming with limited memory.</summary>

### Explain how to minimize Stack Overflow in programming with limited memory.

**Definition:**

The recursive sort uses O(n) auxiliary space on the call stack. The same page’s second method moves values with an explicit stack object instead, still O(n) extra space and O(n²) time, but the depth is a loop, not n nested calls.

**How it works:**

The one-stack queue reaches the bottom by recursion, so its call depth follows the number of elements. Prefer the explicit-stack sort, or the two-stack queue whose loops are not recursive, when the call stack is the limit. The pages do not publish a numeric stack-size setting.

**When to use:**

Use the explicit auxiliary stack when the algorithm would otherwise recurse once per element. Use recursion only for the sort page’s no-second-container variant, and accept its O(n) call-stack space.

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>What techniques can be used to improve the Locality of reference for a Stack?</summary>

### What techniques can be used to improve the Locality of reference for a Stack?

**Definition:**

The fetched pages do not discuss cache locality. They store the stack as a contiguous structure: a Python list, `std::stack`, or one array split by index ranges.

**How it works:**

Pushes in those snippets write the next slot (`append`, `push`, or `a[top]`). The parentheses in-place method goes further and reuses the input string’s own slots as the stack, which is O(1) extra space on a mutable string. No page here publishes a cache-miss bound.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

### Stacks in Multithreading and Concurrency

<details>
<summary>Describe a Lock-free Stack and where it may be used.</summary>

### Describe a Lock-free Stack and where it may be used.

**Definition:**

None of the fetched pages describes a lock-free stack, a compare-and-swap, or a wait-free push.

**How it works:**

What they do describe is an ordinary stack object updated by one scan: `push` and `pop` on that object, with O(n) space when it holds n items. Concurrent callers are outside those pages. The queue built from two stacks is still that ordinary pair of stacks.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>Illustrate how a Stack can be used in implementing Producer-Consumer scenarios.</summary>

### Illustrate how a Stack can be used in implementing Producer-Consumer scenarios.

**Definition:**

A single stack is LIFO. The queue page exists because producer-consumer handoff is FIFO, and a stack alone delivers the newest item first.

**How it works:**

The page’s two-stack queue is the construction that turns stack primitives into a queue: enqueue O(1) onto s1, and dequeue O(n) only when s2 must be refilled, otherwise a pop. That queue, not a bare stack, is the handoff. An empty front returns -1.

**When to use:**

Use the two-stack queue when the consumer must see the oldest item. Use a plain stack only for undo, where the newest item must come out first.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

### Stack Invariants and Monitoring

<details>
<summary>Propose a method to maintain a History of Operations on a Stack.</summary>

### Propose a method to maintain a History of Operations on a Stack.

**Definition:**

The sort page’s auxiliary stack is a second stack that records values removed from the first. A history stack is that same idea: push a record of each operation.

**How it works:**

On each push or pop of the data stack, push a small record onto the history stack. Undoing pops the history and inverts that record. Both stacks follow the page’s O(n) auxiliary bound when each holds n records. The page uses the second stack to sort, not to audit, so the record format is not specified there.

**Examples:**

```python
data, history = [], []

def push(x):
    data.append(x)
    history.append(("push", x))

def undo():
    kind, x = history.pop()
    if kind == "push":
        data.pop()
```

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>How might you design a monitoring system for Capacity Thresholds of a Stack?</summary>

### How might you design a monitoring system for Capacity Thresholds of a Stack?

**Definition:**

The one-stack queue page exposes `size()` as the number of elements in the stack. The two-queue stack’s size is the length of q1.

**How it works:**

Compare that length with a fixed capacity before push. The three-slice layout refuses a push that would leave its index range. The pages do not define an alerting channel; they do define the empty and full conditions those checks use, including a front of -1 on an empty queue-stack.

**Examples:**

```python
def push_checked(st, x, capacity):
    if len(st) >= capacity:
        raise RuntimeError("stack at capacity")
    st.append(x)
```

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

### Stack Error Handling and Exceptions

<details>
<summary>Explain how a Stack Trace is used in debugging and exception handling.</summary>

### Explain how a Stack Trace is used in debugging and exception handling.

**Definition:**

Python’s `traceback` module prints or extracts the stack frames stored on an exception. `BaseException.__traceback__` holds a `types.TracebackType`.

**How it works:**

`print_exception` prints a header, the frames, then the exception type and value. `print_exc` is `print_exception(sys.exception())`. `print_stack` prints the current call stack without requiring an exception. `extract_tb` returns a `StackSummary` of `FrameSummary` entries. `limit` prints that many frames from the caller’s side when positive, and the last `abs(limit)` frames when negative. Chained `__cause__` and `__context__` exceptions are printed when `chain` is true.

**Examples:**

```python
import traceback

try:
    1 / 0
except ZeroDivisionError:
    traceback.print_exc()
```

**Sources:**

- [traceback](https://docs.python.org/3/library/traceback.html)
</details>

<details>
<summary>What methods exist to handle Stack Overflow Exceptions in memory-constrained environments?</summary>

### What methods exist to handle Stack Overflow Exceptions in memory-constrained environments?

**Definition:**

The traceback module prints the stack of whatever exception was raised. It does not define a stack-overflow type or a way to raise the interpreter’s stack limit.

**How it works:**

`print_exc()` and `print_exception` show the frames that led to the exception. `extract_tb` keeps `FrameSummary` records without retaining the live exception object, which the module documents as a way to avoid holding a large object graph. The sort page’s explicit-stack method avoids a call depth of n by using a loop and a second stack of O(n) space.

**Sources:**

- [traceback](https://docs.python.org/3/library/traceback.html)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Stack Algorithm Challenges

<details>
<summary>Create an algorithm to Normalize Pathnames using a Stack.</summary>

### Create an algorithm to Normalize Pathnames using a Stack.

**Definition:**

The fetched pages do not define pathname normalization. The parentheses scan they do define is one left-to-right pass, O(n) time and O(n) stack space, pushing tokens and popping a match.

**How it works:**

A path walk can use that push and pop. Split on `/`. Skip empty parts and `.`. On `..`, pop a component when the stack is not empty. Other parts are pushed. The joined stack is the normalized path. That pop-on-match step is the parentheses rule applied to `..`, and the O(n)/O(n) line belongs to the bracket scan, which has the same one-pass shape.

**Examples:**

```python
def normalize(path):
    st = []
    for part in path.split("/"):
        if part in ("", "."):
            continue
        if part == "..":
            if st:
                st.pop()
        else:
            st.append(part)
    return "/" + "/".join(st) if path.startswith("/") else "/".join(st)
```

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Implement a function that finds the Next Greater Element using a Stack.</summary>

### Implement a function that finds the Next Greater Element using a Stack.

**Definition:**

The stack solution is O(n) time and O(n) space. The next greater element of x is the first value to its right that is strictly greater; if none, it is -1. `[6, 8, 0, 1, 3]` yields `[8, -1, 1, 3, -1]`.

**How it works:**

Walk from the right. Pop while the stack top is less than or equal to the current value. If the stack is not empty, the top is the answer. Push the current value. The nested-loop version is O(n²) time and O(1) extra space. The Python listing on the page stores indexes and compares `arr[stack[-1]]`.

**Examples:**

```python
def next_greater(arr):
    res = [-1] * len(arr)
    st = []
    for i in range(len(arr) - 1, -1, -1):
        while st and st[-1] <= arr[i]:
            st.pop()
        if st:
            res[i] = st[-1]
        st.append(arr[i])
    return res
```

**Sources:**

- [Next Greater Element](https://www.geeksforgeeks.org/dsa/next-greater-element/)
</details>

<details>
<summary>Develop an algorithm to check for Cycles in a Directed Graph using Stack-based DFS.</summary>

### Develop an algorithm to check for Cycles in a Directed Graph using Stack-based DFS.

**Definition:**

The fetched stack pages do not publish a directed-graph cycle bound. They do publish an explicit stack that remembers what is still open: brackets, indexes, or operators.

**How it works:**

A depth-first walk can use that explicit stack of nodes on the current path. Push a node when you enter it and pop it when you leave. A neighbor that is still on the stack is a back edge to an unfinished node. The pages do not state O(V+E) for this walk, so this note does not attach that class. The parentheses checker is the same idea on a string: a close that does not match the open still on the stack is a failure, in O(n) time for a string of length n.

**Examples:**

```python
def has_cycle(graph):
    WHITE, GRAY, BLACK = 0, 1, 2
    color = {u: WHITE for u in graph}
    for start in graph:
        if color[start] != WHITE:
            continue
        stack = [(start, iter(graph[start]))]
        color[start] = GRAY
        while stack:
            node, kids = stack[-1]
            try:
                nxt = next(kids)
            except StopIteration:
                color[node] = BLACK
                stack.pop()
                continue
            if color[nxt] == GRAY:
                return True
            if color[nxt] == WHITE:
                color[nxt] = GRAY
                stack.append((nxt, iter(graph[nxt])))
    return False
```

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
- [Next Greater Element](https://www.geeksforgeeks.org/dsa/next-greater-element/)
</details>

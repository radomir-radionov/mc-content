# Linked Lists — Interview

Answers are filled for every question in this note.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Linked Lists — Interview](./7_ds_linked_lists_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/).

### Basics of Linked Lists

<details>
<summary>What is a Linked List?</summary>

### What is a Linked List?

**Definition:**

A **Linked List** is a dynamic data structure ideal for fast insertions and deletions. Unlike arrays, its elements aren't stored contiguously but are linked via pointers.

**How it works:**

**Anatomy of a Linked List**

A Linked List is a collection of **nodes**, each having:
- **Data**: The stored value.
- **Next Pointer**: A reference to the next node.

The list starts with a **Head** node and ends with a node having a **null** `Next` pointer.

**Visual Representation**

![Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)

**Key Features**

- **Dynamic Size**: Adapts to data volume.
- **Non-Contiguous Memory**: Flexibility in storage.
- **Fast Insertions/Deletions**: Limited pointer adjustments needed.

**Types of Linked Lists**

1. **Singly Linked List**: Each node has a single pointer to the next node. Traversal is unidirectional: from head to tail.
![Singly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)
2. **Doubly Linked List**: Each node have two pointers: one pointing to the next node and another to the previous node. This allows for bidirectional traversal.
![Doubly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fdoubly-linked-list.svg?alt=media&token=5e14dad3-c42a-43aa-99ff-940ab1d9cc3d)
3. **Circular Linked List**: Like a singly linked list, but the tail node points back to the head, forming a loop.
![Circular Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fcircular-linked-list.svg?alt=media&token=b3b96bc7-3b16-4d07-978f-e4774a048ee1)
4. **Multi-level Linked List**: This specialized type has nodes with multiple pointers, each pointing to different nodes. It's often used in advanced data structures like multi-level caches.
![Multi-level Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fmulti-level-linked-list.svg?alt=media&token=967af5cf-8a95-4c05-a8fe-fb70f2b7ea57)

**Common Operations and Time Complexity**

- **Traversal**: Scan through nodes — $O(n)$.
- **Insertion at the Beginning**: Add a node at the start — $O(1)$.
- **Insertion (other cases)/Deletion**: Add or remove nodes elsewhere in the list — $O(n)$.
- **Search**: Locate specific nodes — $O(n)$.
- **Sorting**: Order or organize nodes in the list. Commonly-used algorithms for linked lists like merge sort have a time complexity of $O(n \log n)$.
- **Merging**: Combine two lists — $O(n)$ where $n$ is the total number of nodes in both lists.
- **Reversal**: Flip node order — $O(n)$.

**Code Example: Singly Linked List**

Here is the Python code:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
        else:
            last_node = self.head
            while last_node.next:
                last_node = last_node.next
            last_node.next = new_node

    def display(self):
        current = self.head
        while current:
            print(current.data)
            current = current.next

# Usage
my_list = LinkedList()
my_list.insert(1)
my_list.insert(2)
my_list.insert(3)
my_list.display()

# Output:
# 1
# 2
# 3
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>What are some pros and cons of Linked List compared to Arrays?</summary>

### What are some pros and cons of Linked List compared to Arrays?

**Definition:**

Let's look at the pros and cons of using **linked lists** compared to **arrays**.

**How it works:**

**Advantages of Linked Lists**

- **Dynamic Size**: Linked lists naturally adjust to changing sizes, while arrays are fixed-sized. Dynamic arrays auto-resize but can lag in efficiency during frequent mid-list insertions or deletions.

- **Efficient Insertions/Deletions**: Insertions and deletions in linked lists only require a few pointer adjustments, whereas arrays may need shifting of elements.

- **Flexibility in Size**: Memory for nodes in linked lists is allocated or released as needed, potentially reducing memory wastage.

- **Merging and Splitting**: It's simpler to merge or split linked lists.

**Disadvantages of Linked Lists**

- **Memory Overhead**: Each node has overhead due to data and a pointer, using more memory than arrays for the same number of elements.

- **Sequential Access**: Linked lists only allow sequential access, unlike arrays that support direct indexing.

- **Cache Inefficiency**: Nodes might be scattered in memory, leading to cache misses.

- **No Random Access**: Element retrieval might require full list traversal, whereas arrays offer constant-time access.

- **Data Integrity**: If a node's link breaks, subsequent nodes are lost.

- **Search Efficiency**: Requires linear scans, which can be slower than searches in sorted arrays or trees.

- **Sorting**: Certain sorting algorithms, like QuickSort, are less efficient with linked lists than with arrays.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Explain the difference between Singly Linked Lists and Doubly Linked Lists.</summary>

### Explain the difference between Singly Linked Lists and Doubly Linked Lists.

**Definition:**

**Linked List** variants, including  **Singly Linked Lists (SLL)** and **Doubly Linked Lists (DLL)**, each have unique characteristics when it comes to memory efficiency and traversal capabilities.

**How it works:**

**Key Distinctions**

**Memory Optimization**

- **Singly Linked List**: Uses less memory per node as it requires only one reference to the next node.
- **Doubly Linked List**: Consumes more memory per node due to its need for two references, one each for the previous and next nodes.

**Traversal Efficiency**

- **Singly Linked List**: Traverseable in one direction, which is from the head to the tail.
- **Doubly Linked List**: Offers bi-directional traversability. You can move in both directions, from head to tail and vice versa.

**Node Complexity**

- **Singly Linked List**: Each node stores data and a reference to the next node.
- **Doubly Linked List**: In addition to data and pointers, each node maintains a reference to its previous node.

**Visual Representation**

**Singly Linked List**: Nodes link unidirectionally.
![Singly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)
**Doubly Linked List**: Nodes connect both ways, with arrows pointing in two directions.
![Doubly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fdoubly-linked-list.svg?alt=media&token=5e14dad3-c42a-43aa-99ff-940ab1d9cc3d)

**Code Example: Singly Linked List**

Here is the Python code:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def add_node(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node

# Instantiate and populate the Singly Linked List
sll = SinglyLinkedList()
sll.add_node(1)
sll.add_node(2)
sll.add_node(3)
```

**Code Example: Doubly Linked List**

Here is the Python code:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None

    def add_node(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node
            new_node.prev = current

# Instantiate and populate the Doubly Linked List
dll = DoublyLinkedList()
dll.add_node(1)
dll.add_node(2)
dll.add_node(3)
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>How does a Linked List manage memory allocation differently from an Array?</summary>

### How does a Linked List manage memory allocation differently from an Array?

**Definition:**

Let's explore **how linked lists and arrays** differ in terms of memory management and their implications on data handling and computational complexity.

**How it works:**

**Memory Management**

- **Arrays**: Contiguously allocate memory for predefined sizes. This results in efficient element access but may lead to memory wastage or reallocation drawbacks if storage requirements change.

- **Linked Lists**: Use dynamic memory allocation, where each node, containing data and a pointer, is allocated as needed. This flexibility in memory management is a key distinction from arrays.

**Visual Representation**

![Array and Linked List Memory Layout](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Farray-linked-list-memory.webp?alt=media&token=9a02e0c9-9245-4de0-9ed1-2bc258d05fc6)

- **Array**: Elements are stored in contiguous memory cells or "slots," enabling direct $O(1)$ access based on the element index.

- **Linked List**: Nodes are disparate in memory, and connections between them unfold through pointers. Each node reserves memory addresses for the data it holds and for the subsequent node's memory address.

**Array vs. Linked List Performance**

**Element Access**

- **Array**: Elements are indexed, allowing direct access in $O(1)$ time, e.g., `arr[5]`.
- **Linked List**: Sequential traversal is typically necessary, making element access linear in time or $O(n)$.

**Memory Overhead**

- **Array**: Offers direct memory access and is efficient for homogeneous data types.
- **Linked List**: Introduces memory overhead due to node pointer storage, but it's more adaptable for dynamic operations.

**Insertion and Deletion**

- **Array**: Can be $O(n)$ in the worst case due to potential shift or resize requirements.
- **Linked List**: Unquestionably efficient, typically $O(1)$, especially for list head or tail insertions.

**Memory Allocation Efficacy**

- **Array**: Might face underutilization or require resizing, introducing computational and memory overheads.
- **Linked List**: More efficient, with memory being dispatched as and when nodes are created or deleted.

**Cache Efficiency and Data Locality**

- **Array**: Due to contiguous memory, excels in cache and CPU caching optimization.
- **Linked List**: Might incur cache misses due to non-contiguous node storage, potentially leading to less efficient data retrieval in comparison to arrays.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>What are the basic operations that can be performed on a Linked List?</summary>

### What are the basic operations that can be performed on a Linked List?

**Definition:**

**Linked Lists** are dynamic data structures optimized for insertion and deletion. Their key operations include:

**How it works:**

**Traversal**

- **Depiction**: Visualize each node consecutively. Common implementations are **iterative** and **recursive**.
- **Time Complexity**: $O(n)$
- **Code Example**:

```python
def traverse(self):
    current = self.head
    while current:
        print(current.data)
        current = current.next
```

2. **Search**:
- **Description**: Identify a target value within the list. Requires traversal.
- **Time Complexity**: Best: $O(1)$; Worst: $O(n)$
- **Code Example**:

```python
def search(self, target):
    current = self.head
    while current:
        if current.data == target:
            return True
        current = current.next
    return False
```

3. **Insertion**:
- **Description**: Add a new node at a specified position.
- **Time Complexity**: Best: $O(1)$; Worst: $O(n)$, if tail needs to be found
- **Code Example**:

```python
def insert_at_start(self, data):
    new_node = Node(data)
    new_node.next = self.head
    self.head = new_node
```

4. **Deletion**:
- **Description**: Remove a node that contains a particular value, or from a specific position.
- **Time Complexity**: Best: $O(1)$ (for head or single known middle node); Worst: $O(n)$ (for removing tail before finding the new tail)
- **Code Example**:

```python
def delete_at_start(self):
    if self.head:
        self.head = self.head.next
```

5. **Observations**:
   - Iterative traversal is faster than recursive due to stack overhead.
   - Arraylist provides better search performance.
   - Linked lists are a top pick for frequent insertions or deletions at random positions.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Advantages and Use Cases of Linked Lists

<details>
<summary>What are some real-life Use Cases of Linked Lists?</summary>

### What are some real-life Use Cases of Linked Lists?

**Definition:**

**Linked lists** are widely used in real-world applications for their advantages in dynamic memory management and data manipulation.

**How it works:**

**Practical Applications**

**Operating Systems**

- **Task Scheduling**: Linked lists efficiently manage queues of tasks awaiting execution.
- **Memory Management**: They facilitate dynamic memory allocation, especially useful in applications like memory pool management.

**Text Editors**

- **Undo/Redo Functionality**: Editors maintain a stack of changes using linked lists, enabling the undo and redo functionalities.

**Music Players**

- **Playlists**: Linked lists offer flexibility in managing playlists, allowing for the easy addition, deletion, and navigation of tracks.

**Web Browsers**

- **Browser History**: Linked lists, especially doubly linked ones, are instrumental in navigating web page histories, permitting both forward and backward traversal.

**Compilers**

- **Symbol Tables**: Compilers employ linked lists to manage tables containing variable and function identifiers. This provides scoped access to these identifiers during different stages of compilation.

**Database Management Systems**

- **Transient Storage Structures**: While core storage might use trees or hash indexes, linked lists can serve auxiliary roles, especially in in-memory databases.

**Artificial Intelligence and Machine Learning**

- **Graph Representation**: Algorithms requiring graph representations often utilize adjacency lists, essentially arrays of linked lists, to depict vertices and edges.

**Caching Algorithms**

- **LRU Cache**: Linked lists, particularly doubly linked ones, play a pivotal role in the Least Recently Used (LRU) caching algorithms to determine which items to replace.

**Networking**

- **Packet Management**: In networking scenarios, linked lists help manage queues of data packets awaiting transmission.

**Gaming**

- **Character Inventory**: In role-playing games, a character's inventory, where items are added and removed frequently, can be managed using linked lists.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>When is a Circular Linked List useful?</summary>

### When is a Circular Linked List useful?

**Definition:**

A **circular Linked List** is a specific type of linked list where the tail node is intentionally connected back to the head node to form a closed loop.

**How it works:**

![Circular Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fcircular-linked-list.svg?alt=media&token=b3b96bc7-3b16-4d07-978f-e4774a048ee1)

**Common Use Cases**

- **Emulating Circular Structures**: Useful for representing naturally circular data like polygon vertices, buffer pools, or round-robin scheduling in operating systems.

- **Queue Efficiency**: Accessing the front and rear elements in constant time, improving queue implementations.

- **Algorithmic Simplifications**: Enables easier data manipulations like list splitting and concatenation in constant time.

**Code Example: Queue Efficiency**

Here is the Python code:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class CircularQueue:
    def __init__(self):
        self.front = self.rear = None

    def enqueue(self, data):
        new_node = Node(data)
        if self.rear:
            self.rear.next, self.rear = new_node, new_node
        else:
            self.front = self.rear = new_node
        self.rear.next = self.front

    def dequeue(self):
        if not self.front: return None
        if self.front == self.rear: self.front = self.rear = None
        else: self.front = self.front.next; self.rear.next = self.front
        return self.front.data if self.front else None

# Example usage:
cq = CircularQueue()
cq.enqueue(1); cq.enqueue(2); cq.enqueue(3)
print(cq.dequeue(), cq.dequeue(), cq.dequeue(), cq.dequeue())
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>When is Doubly Linked List more efficient than Singly Linked List?</summary>

### When is Doubly Linked List more efficient than Singly Linked List?

**Definition:**

**Doubly linked lists** offer advantages in specific use-cases but use more memory and may require more complex thread-safety

**How it works:**

**Key Efficiency Differences**

- **Deletion**: If only the node to be deleted is known, doubly linked lists can delete it in $O(1)$ time, whereas singly linked lists may take up to $O(n)$ to find the prior node.

- **Tail Operations**: In doubly linked lists, tail-related tasks are $O(1)$. For singly linked lists without a tail pointer, these are $O(n)$.

**Practical Use-Cases**

- **Cache Implementations**: Doubly linked lists are ideal due to quick bidirectional insertion and deletion.

- **Text Editors and Undo/Redo**: The bidirectional capabilities make doubly linked lists more efficient for these functions.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Describe a scenario where the use of a Linked List is more suitable than a Dynamic Array.</summary>

### Describe a scenario where the use of a Linked List is more suitable than a Dynamic Array.

**Definition:**

**Linked Lists** and **Dynamic Arrays** are distinct data structures, each with its own advantages. Linked Lists, for instance, often outperform Dynamic Arrays in situations that involve **frequent insertions and deletions**.

**How it works:**

**Performance Considerations**

- **Insertion/Deletion**: Linked Lists have $O(1)$ time complexity, whereas Dynamic Arrays are generally slower with an average $O(n)$ time complexity due to potential element shifts.
- **Random Access**: While Dynamic Arrays excel in $O(1)$ random access, Linked Lists have an inferior $O(n)$ complexity because they're not index-based.

**Practical Scenario**

Consider an interactive crossword puzzle game. For convenience, let's assume each crossword puzzle consists of 10 words. In the scenario, players:

1. **Fill**: Begin with a set of words at known positions.
2. **Swap**: Request to relocate words (Words 3 and 5, for example).
3. **Expand/Contract**: Add or remove a word, potentially changing its position in the list.

**The Best Approach**: To support these dynamic operations and maintain list integrity, a **doubly-linked list** is the most suitable choice.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Linked Lists vs. Other Data Structures

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

**Array-Based Stack**

- **Locality**: Consecutive memory locations benefit CPU caching.
- **Random Access**: Provides direct element access.
- **Iterator Needs**: Preferable if indexing or iterators are required.
- **Performance**: Slightly faster for top-element operations and potentially better for time-sensitive tasks due to caching.
- **Push**: $O(1)$ on average; resizing might cause occasional $O(n)$.

**Linked List Stack**

- **Memory Efficiency**: Better suited for fluctuating sizes and limited memory scenarios.
- **Resizing Overhead**: No resizing overheads.
- **Pointer Overhead**: Requires extra memory for storing pointers.

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

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>How do Linked Lists perform in comparison with Trees for various operations?</summary>

### How do Linked Lists perform in comparison with Trees for various operations?

**Definition:**

Let's examine the **time and space complexity** of common operations in **linked lists** and **trees**, highlighting trade-offs of each data structure.

**How it works:**

**Common Data Structure Operations**

**Search**

- **Linked List**: $O(n)$
- **Tree**: $O(\log n) \text{ to } O(n)$ - in case of linked list-like skewed trees.
  - Balanced Trees (e.g., AVL, Red-Black): $O(\log n)$
  - Unbalanced Trees: $O(n)$

**Insert/Delete**

- **Linked List**: $O(1)$ to $O(n)$ if searching is required before the operation.
- **Tree**: $O(\log n)$ to $O(n)$ in the worst-case (e.g., for skewed trees).

**Operations at the Beginning/End**

- **Linked List**: $O(1)$
  - Singly Linked List: $O(1)$
  - Doubly Linked List: $O(1)$
- **Tree**: Not applicable.

**Operations in the Middle (based on the key or value)**

- **Linked List**: $O(n)$

To consider:
    - For the singly linked list, searching and finding the node before the target is involved leading to $O(n)$.
    - For the doubly linked list, direct access to the previous node reduces time to $O(1)$.

- **Tree**: $O(\log n)$ to $O(n)$

The operation involves a search ($O(\log n)$ in a balanced tree) and then, if found, a constant-time to $O(\log n)$ modification (in case of tree balancing requirements).

**Traversal**

- **Linked List**: $O(n)$
- **Tree**: $O(n)$

Both data structures require visiting every element once.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>When would you use a Linked List over a Hash Table?</summary>

### When would you use a Linked List over a Hash Table?

**Definition:**

**Performance considerations** and the nature of operations you plan to perform significantly influence whether a Linked List or a Hash Table best suits your needs.

**How it works:**

**Key Decision Factors**

- **Data Order and Relationship**: Linked Lists fundamentally maintain order, which is often crucial in many scenarios. In contrast, Hash Tables impose no specific order.

- **Memory Overhead**: Linked Lists offer a more streamlined approach to memory management without the potential for clustering. Hash Tables, on the other hand, can have memory overhead due to hash functions, collision handling, and the need for extra space to prevent performance degradation.

- **Access Time**: Both data structures require $O(1)$ time complexity for certain operations. Hash Tables are known for this in most cases, but Linked Lists can be equally efficient for operations that take place solely at either end of the list.

- **Duplication Handling**: Linked Lists can directly store duplicate values. In Hash Tables, they can be tricky to manage because they require unique keys.

- **Data Persistence and Disk Storage**: Linked Lists are more conducive to persistent data storage, such as disk storage, because of their sequential data storage and easy disk access via pointers.

**Use Case Considerations**

**Common Use Cases for Linked Lists**

- **Dynamic Allocation**: When you need a dynamic allocation of memory that's not limited by fixed table sizes, Linked Lists can expand and contract efficiently.
- **Efficient Insertions/Deletions**: For these operations in the middle of a list, implementing them on Linked Lists is particularly straightforward and efficient.
- **Sequential Data Processing**: Certain tasks like linear search or the traversal of ordered data are simpler to perform with Linked Lists.
- **Persistent Data Storage**: When data persistence is a concern, such as for persistent caches, Log-structured File Systems, or databases with transaction logs.
- **Memory Compactness**: In scenarios where memory segmentation or disk access indirectly impacts performance, the undivided blocks in Linked Lists can be an advantageous choice.

**Common Use Cases for Hash Tables**

- **Quick Lookups**: For rapid retrieval of data based on a unique key, Hash Tables shine.
- **Memory Mapped File Access**: Especially beneficial for very large data sets when practical, as it can reduce I/O cost.
- **Cache Performance**: Their fast access and mutation operations make them ideal for in-memory caching systems.
- **Distinct Value Storage**: Best suited when each key must be unique. If you try to insert a duplicate key, its existing value is updated, which can be useful in multiple contexts, like address tables in a network.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Traversal and Search in Linked Lists

<details>
<summary>Is it possible to Traverse a Linked List in O(n1/2)? (Jump Pointers).</summary>

### Is it possible to Traverse a Linked List in O(n1/2)? (Jump Pointers).

**Definition:**

While it may not be possible to **traverse a linked list** in better than $O(n)$ time complexity in the strictest sense, there are techniques that can make the traversal process more efficient in certain contexts.

**How it works:**

In particular, let's explore the idea of "**Jump Pointers**" or "**Square Root Jumps**" which allows you to traverse a linked list in $O(\sqrt{n})$ time complexity.

**What Are Jump Pointers?**

**Jump Pointers** allow for quicker traversal by "jumping" over a fixed number of nodes $k$ during each step. This reduces the total number of nodes visited, thereby improving the time complexity.

For instance, when $k = \sqrt{n}$, the traversal time complexity drops to $O(\sqrt{n})$.

**Code Example: Jump Pointers**

Here is the Python code:

```python
# Node definition
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None

# LinkedList definition
class LinkedList:
    def __init__(self):
        self.head = None

    # Add node to end
    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    # Traverse using Jump Pointers
    def jump_traverse(self, jump_size):
        current = self.head
        while current:
            print(current.data)
            for _ in range(jump_size):
                if not current:
                    return
                current = current.next

# Create linked list and populate it
llist = LinkedList()
for i in range(1, 11):
    llist.append(i)

# Traverse using Jump Pointers
print("Jump Pointer Traversal:")
llist.jump_traverse(int(10**0.5))
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>How to apply Binary Search in O(log n) on a Sorted Linked List? (Skip Lists).</summary>

### How to apply Binary Search in O(log n) on a Sorted Linked List? (Skip Lists).

**Definition:**

While **Binary Search** boasts a time complexity of $O(\log n)$, applying it to a singly linked list is less straightforward due to the list's linear nature and $O(n)$ access time. However, **Skip Lists** offer a clever workaround to achieve sub-linear search times in linked lists.

**How it works:**

**Why Traditional Binary Search Falls Short in Linked Lists**

In a **singly linked list**, random access to elements is not possible. To reach the $k$-th element, you have to traverse $k-1$ preceding nodes. Therefore, the act of **accessing a middle element** during binary search incurs a time complexity of $O(n)$.

**Skip Lists: A Solution for Sub-linear Search**

Skip Lists augment sorted linked lists with multiple layers of '**express lanes**', allowing you to leapfrog over sections of the list. Each layer contains a subset of the elements from the layer below it, enabling faster search.

![Skip List Example](https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Skip_list.svg/1920px-Skip_list.svg.png)

By starting at the topmost layer and working downwards, you can **reduce the search space** at each step. This results in an average time complexity of $O(\log n)$ for search operations.

**Code Example: Visualizing a Skip List**

Here is the Python code:

```python
# Define a node for the Skip List
class SkipNode:
    def __init__(self, value):
        self.value = value
        self.next = []

# Initialize a Skip List
class SkipList:
    def __init__(self):
        self.head = SkipNode(float('-inf'))  # Initialize with the smallest possible value
        self.levels = 1  # Start with a single level
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Is it possible to do Binary Search on a Doubly Linked List in O(n) time?</summary>

### Is it possible to do Binary Search on a Doubly Linked List in O(n) time?

**Definition:**

Applying **Binary search** to **doubly-linked lists** presents challenges because these lists lack the **random access** feature essential for binary search's efficiency.

**How it works:**

In **binary search**, each comparison cuts the search space in half. However, accessing the **middle** element of a doubly-linked list takes $O(n)$ time since you have to traverse from the head or tail to the middle.

Consequently, the running time becomes $O(n \log n)$, not the optimal $O(\log n)$ seen with arrays.

**Advanced Search Techniques for Doubly-Linked Lists**

- **Jump Pointers**: Utilizes multiple pointers to skip predetermined numbers of nodes, enhancing traversal speed. Although it approximates a time complexity of $O(n)$, with a jump interval of $k$, the complexity improves to $O(n/k)$. However, this might increase memory usage.

- **Interpolation Search**: A modified binary search that employs linear interpolation for superior jumping efficiency in certain data distributions. Its worst-case time complexity is $O(n)$, but for uniformly distributed data, it can be as efficient as $O(\log \log n)$.

**Code Example: Jump Pointers**

Here is the Python code:

```python
def jump_pointers_search(head, target):
    jump_factor = 2  
    current = head
    jump = head
    while current and current.value < target:
        jump = current
        for _ in range(jump_factor):
            if current.next:
                current = current.next
            else:
                break
    while jump and jump.value < target:
        jump = jump.next
    return jump
```

**Code Example: Interpolation Search**

Here is a Python code:

```python
def interpolation_search(head, target):
    low = head
    high = None
    while low and low.value <= target:
        high = low
        low = low.next
    while high and high.value < target:
        high = high.next
    return high
```

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Implement a method to find the middle element of a Linked List.</summary>

### Implement a method to find the middle element of a Linked List.

**Definition:**

Start both pointers at the head. Move the fast pointer two nodes at a time and the slow pointer one node at a time. When the fast pointer reaches the end, the slow pointer is at the middle.

**How it works:**

The two-pointer page states that rule and does not publish a Big-O line, and it does not say which node is the middle when the length is even. Stop the fast pointer when it cannot take another two-node step, and return the slow node.

**Examples:**

```python
def middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Implementations and Conversions

<details>
<summary>Implement a Linked List using Stack.</summary>

### Implement a Linked List using Stack.

**Definition:**

The two-pointer page does not build a linked list out of a stack. It assumes a list whose nodes already have a next field.

**How it works:**

A stack gives LIFO order. The page’s reverse walks next pointers and ends with prev as the new head, which is the list reversed, not a stack-backed list. No construction that stores nodes in a stack is shown.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Reverse a Singly Linked List using only Two Pointers.</summary>

### Reverse a Singly Linked List using only Two Pointers.

**Definition:**

The page’s reverse uses three pointers, not two: curr, prev, and next.

**How it works:**

Set curr to the head. While curr is not null, save curr’s next, point curr.next at prev, then move prev to curr and curr to the saved next. The reversed head is prev. The page does not publish a complexity for this walk.

**Examples:**

```python
def reverse(head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
    return prev
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Convert a Singly Linked List to a Circular Linked List.</summary>

### Convert a Singly Linked List to a Circular Linked List.

**Definition:**

The page does not describe a circular list. Its walks stop when a pointer reaches the end, which means a null next.

**How it works:**

A circular list would make that end test never succeed. The page’s loop check is a different idea: if the fast pointer catches the slow pointer, the list already has a loop. Closing the list on purpose is not shown.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Convert a Singly Linked List to a Doubly Linked List.</summary>

### Convert a Singly Linked List to a Doubly Linked List.

**Definition:**

The page’s nodes have a next field. A previous field is not part of that description, except the prev pointer used while reversing.

**How it works:**

During the reverse, prev is the node before curr and curr.next is turned to point at prev. That builds a backward next, not a second stored link on every node. The page does not show a doubly linked node.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement a Doubly Linked List using Stacks with minimal complexity.</summary>

### Implement a Doubly Linked List using Stacks with minimal complexity.

**Definition:**

The page does not implement a doubly linked list or assign a complexity to one.

**How it works:**

Its reverse already remembers the previous node in prev while curr walks forward. That is three local pointers for the duration of the walk, not a stack of nodes and not a published minimum complexity.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Convert a Binary Tree into a Doubly Linked List.</summary>

### Convert a Binary Tree into a Doubly Linked List.

**Definition:**

The page does not mention binary trees.

**How it works:**

The list operations it does describe follow next (and, while reversing, prev). A tree has child links, which this page never rewires.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Insertion and Deletion

<details>
<summary>Implement a method to Insert a node at a specific position in a Linked List.</summary>

### Implement a method to Insert a node at a specific position in a Linked List.

**Definition:**

The page does not show insertion. Its nodes are reached by following next.

**How it works:**

To place a node at a position you walk next that many steps, then set the new node’s next to the current node’s next and the current node’s next to the new node. The page publishes no cost for that walk.

**Examples:**

```python
def insert_after(node, value):
    created = Node(value)
    created.next = node.next
    node.next = created
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Describe how to Delete a node in a Linked List without having a head pointer.</summary>

### Describe how to Delete a node in a Linked List without having a head pointer.

**Definition:**

The page always starts its pointers at the head. It does not delete a node when the head is unknown.

**How it works:**

Every technique on the page, middle, loop, reverse, nth-from-end, and intersection, is defined from the head. Without that reference the walks are not the ones published there.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement an efficient function to Append a node to the end of a Linked List.</summary>

### Implement an efficient function to Append a node to the end of a Linked List.

**Definition:**

The page does not show append, and it does not say which walk is efficient.

**How it works:**

Its end test is a pointer that has reached the end. Following next until that end, then setting next to the new node, is how you attach a tail. No complexity is published.

**Examples:**

```python
def append(head, value):
    node = head
    while node.next:
        node = node.next
    node.next = Node(value)
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Insert an item in a Sorted Linked List, while maintaining order.</summary>

### Insert an item in a Sorted Linked List, while maintaining order.

**Definition:**

The page does not discuss sorted lists or ordered insertion.

**How it works:**

You still walk next. Stop at the first node whose value should come after the new value, and link the new node in front of it the same way a position insert rewires next. No order test or complexity is on the page.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Algorithms on Linked Lists

<details>
<summary>Explain Floyd’s Cycle Detection Algorithm.</summary>

### Explain Floyd’s Cycle Detection Algorithm.

**Definition:**

The page does not use the name Floyd. Its loop test is the fast and slow pointers.

**How it works:**

Both start at the head. The fast pointer moves two nodes at a time and the slow pointer one node at a time. If the fast pointer catches the slow pointer, the list has a loop. The page does not publish a time or space bound for this test.

**Examples:**

```python
def has_loop(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True
    return False
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Is there an algorithm Better Than Floyd’s for Cycle Detection in linked lists?</summary>

### Is there an algorithm Better Than Floyd’s for Cycle Detection in linked lists?

**Definition:**

The page gives one loop test and does not compare it with another algorithm or call it better.

**How it works:**

That test is fast moving two steps and slow moving one. A meeting means a loop. A hash of seen nodes is not on the page, so this note does not rank it against the two-pointer test.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Write an algorithm to Shuffle a Linked List.</summary>

### Write an algorithm to Shuffle a Linked List.

**Definition:**

The page does not shuffle a list.

**How it works:**

Its pointer moves are fixed: one step, two steps, or a switch to the other list’s head. None of them draws a random node. A shuffle is outside that page.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Detect if a List is Cyclic using a Hash Table.</summary>

### Detect if a List is Cyclic using a Hash Table.

**Definition:**

The page’s cycle test does not use a hash table. It uses two pointers.

**How it works:**

Both start at the head, fast moves two nodes and slow moves one. If fast catches slow, there is a loop. The page does not describe storing visited nodes in a table.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement an algorithm to Sort a Linked List.</summary>

### Implement an algorithm to Sort a Linked List.

**Definition:**

The page does not sort a list.

**How it works:**

The walks it defines preserve or reverse link order. Reverse makes prev the new head by turning each next backward. That is not a comparison sort, and no sort bound is published.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Merging and Reversing

<details>
<summary>Merge two sorted Singly Linked Lists without creating new nodes.</summary>

### Merge two sorted Singly Linked Lists without creating new nodes.

**Definition:**

The page does not merge two sorted lists. Its intersection walk does switch a pointer from one list to the other list’s head.

**How it works:**

That switch is for finding a shared node, not for splicing two ordered lists. Creating no new nodes would mean rewiring existing next fields. The page never does that across two sorted inputs.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Reverse a Linked List recursively.</summary>

### Reverse a Linked List recursively.

**Definition:**

The page’s reverse is a loop, not a recursive call. curr, prev, and next move until curr is null, and prev is the new head.

**How it works:**

There is no recursive reverse and no statement of call-stack cost. The iterative walk is the one published.

**Examples:**

```python
def reverse(head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
    return prev
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Write a function that Reverse K nodes of the Linked List at a time.</summary>

### Write a function that Reverse K nodes of the Linked List at a time.

**Definition:**

The page reverses the whole list. It does not reverse groups of k.

**How it works:**

The published reverse turns every next until curr is null. Stopping after k nodes and then repeating on the remainder is not described, and no complexity is given.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Advanced Manipulations

<details>
<summary>Find the N-th element from the end of a Singly Linked List.</summary>

### Find the N-th element from the end of a Singly Linked List.

**Definition:**

Two pointers p and q start at the head. Move q forward n nodes. Then move p and q one node at a time. When q reaches the end, p is at the nth node from the end.

**How it works:**

The page states those steps and does not publish a complexity. If q cannot move n nodes, the list is shorter than n and there is no such node.

**Examples:**

```python
def nth_from_end(head, n):
    p = q = head
    for _ in range(n):
        if q is None:
            return None
        q = q.next
    while q:
        p = p.next
        q = q.next
    return p
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Remove Duplicates from an Unsorted Linked List.</summary>

### Remove Duplicates from an Unsorted Linked List.

**Definition:**

The page does not remove duplicates.

**How it works:**

Its walks follow next at one or two steps, or switch lists. They do not compare node values or unlink a repeated value. No extra table is described.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Find Common Elements in two given Linked Lists and return them as a new Linked List.</summary>

### Find Common Elements in two given Linked Lists and return them as a new Linked List.

**Definition:**

The page does not build a list of common values. Its two-list walk looks for a shared node, the intersection.

**How it works:**

p starts on the first head and q on the second. Each runs to the end and then continues from the other head. If they meet, that node is the intersection. If both finish without meeting, there is no intersection. That is identity of a node, not equal values copied into a new list.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Split the Linked List into k consecutive parts.</summary>

### Split the Linked List into k consecutive parts.

**Definition:**

The page does not split a list into k parts.

**How it works:**

The middle technique is the special case of two pointers finding one cut, with fast moving twice as fast as slow. It does not produce k sublists, and it publishes no bound.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Sum two numbers represented as Linked Lists.</summary>

### Sum two numbers represented as Linked Lists.

**Definition:**

The page does not add numbers stored as lists.

**How it works:**

It moves pointers along next. It never reads a digit or carries a sum into a new node.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Special Cases and Challenges

<details>
<summary>Find a Merge Point (Intersection) of two Linked Lists.</summary>

### Find a Merge Point (Intersection) of two Linked Lists.

**Definition:**

p starts at the first head and q at the second. When a pointer reaches the end, it continues from the other list’s head. If p and q meet, that node is the intersection. If they both reach the end without meeting, there is none.

**How it works:**

The page gives those steps and no complexity. The same walk is the page’s answer for a Y-shaped pair of lists that share a tail.

**Examples:**

```python
def intersection(a, b):
    p, q = a, b
    while p is not q:
        p = p.next if p else b
        q = q.next if q else a
    return p
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Given a Singly Linked List, determine if it is a Palindrome.</summary>

### Given a Singly Linked List, determine if it is a Palindrome.

**Definition:**

The page does not test a palindrome.

**How it works:**

It can find the middle and it can reverse a list. It never compares values from both ends, so a palindrome check is not one of its published results.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Find the length of a Linked List that contains a Cycle.</summary>

### Find the length of a Linked List that contains a Cycle.

**Definition:**

The page tells you whether a loop exists. It does not measure the loop’s length.

**How it works:**

Fast moves two steps and slow moves one. If fast catches slow, there is a loop. The page stops at that fact. Counting nodes around the loop after the meeting is not described.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Duplicate a Linked List with Random Pointers using O(1) space.</summary>

### Duplicate a Linked List with Random Pointers using O(1) space.

**Definition:**

The page does not mention an extra arbitrary pointer, a copy of the list, or an O(1) space bound.

**How it works:**

Its extra state is the two or three local pointers of each technique. It never clones a node or rewires a second pointer besides next during reverse.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement a function to check if two Singly Linked Lists intersect without using extra space.</summary>

### Implement a function to check if two Singly Linked Lists intersect without using extra space.

**Definition:**

The intersection walk uses two pointers and no table. p and q start on the two heads, and each switches to the other head after it reaches the end.

**How it works:**

If the pointers meet, that node is the intersection. If both reach the end without meeting, the lists do not intersect. The page does not call this constant extra space in a Big-O line; it simply uses p and q.

**Examples:**

```python
def intersects(a, b):
    p, q = a, b
    while p is not q:
        p = p.next if p else b
        q = q.next if q else a
    return p is not None
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>How would you rotate a Linked List by k places?</summary>

### How would you rotate a Linked List by k places?

**Definition:**

The page does not rotate a list by k.

**How it works:**

The nth-from-end walk does move one pointer n nodes ahead of the other. That finds a node. It does not cut the list there and move that suffix to the front.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Complexity and Analysis

<details>
<summary>Discuss the Time Complexity of various operations in Linked Lists.</summary>

### Discuss the Time Complexity of various operations in Linked Lists.

**Definition:**

The two-pointer page does not publish a time complexity for any operation.

**How it works:**

What it does specify is the step size. Middle and loop detection move one pointer one node and the other two nodes. Reverse moves one node per loop. The nth-from-end walk advances one pointer n nodes, then both pointers one node at a time. Intersection switches each pointer to the other head once it reaches the end. Those are the published controls, not Big-O classes.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Assess the Space Complexity of a Linked List with auxiliary pointers, such as in a Skip List.</summary>

### Assess the Space Complexity of a Linked List with auxiliary pointers, such as in a Skip List.

**Definition:**

The page does not mention a skip list or publish a space bound.

**How it works:**

The auxiliary pointers it actually uses are local: slow and fast, or p and q, or curr, prev, and next. They are not extra links stored on every node. A skip list’s extra pointers are outside this page.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Explain how Recursion affects time and space complexity in Linked List operations.</summary>

### Explain how Recursion affects time and space complexity in Linked List operations.

**Definition:**

The page’s algorithms are loops. It does not analyze recursion or give a time or space class.

**How it works:**

Reverse, middle, loop detection, nth-from-end, and intersection are all described as pointer moves in a loop. There is no recursive formulation whose call stack could be counted.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Linked List Problems and Solutions

<details>
<summary>Solve the problem of finding the Intersection Point of a Y-shaped Linked List.</summary>

### Solve the problem of finding the Intersection Point of a Y-shaped Linked List.

**Definition:**

Use the page’s intersection walk. A Y shape is two lists that later share nodes.

**How it works:**

Start p and q on the two heads. When either reaches the end, send it to the other head. The node where they meet is the intersection. If they never meet, the page says there is no intersection point. No complexity is published.

**Examples:**

```python
def y_intersection(a, b):
    p, q = a, b
    while p is not q:
        p = p.next if p else b
        q = q.next if q else a
    return p
```

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Write an algorithm to segregate even and odd nodes in a Singly Linked List.</summary>

### Write an algorithm to segregate even and odd nodes in a Singly Linked List.

**Definition:**

The page does not separate even and odd nodes.

**How it works:**

It moves pointers by one or two steps without testing a value. Rewiring next so even values precede odd values is not described.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>How can you sort a Linked List that is sorted alternating between ascending and descending orders?</summary>

### How can you sort a Linked List that is sorted alternating between ascending and descending orders?

**Definition:**

The page does not sort, and it does not discuss alternating order.

**How it works:**

Reverse turns the whole list around by pointing each next at prev. That would reverse one of the directions, but the page does not split the list into ascending and descending runs first.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Develop a method to swap pairs of nodes in a Linked List.</summary>

### Develop a method to swap pairs of nodes in a Linked List.

**Definition:**

The page does not swap pairs.

**How it works:**

Reverse rewires every next until the end. Swapping only adjacent pairs and then skipping two nodes is a different rewiring, and it is not shown.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement an algorithm to remove all Odd-Indexed Nodes from a Linked List.</summary>

### Implement an algorithm to remove all Odd-Indexed Nodes from a Linked List.

**Definition:**

The page does not delete by index.

**How it works:**

The fast pointer already moves two nodes at a time for the middle and the loop test. The page uses that to find a node or a meeting, not to unlink every other node.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Describe a technique to randomly select a node from a Linked List with a uniform distribution.</summary>

### Describe a technique to randomly select a node from a Linked List with a uniform distribution.

**Definition:**

The page does not select a random node.

**How it works:**

Its pointer motion is determined: one step, two steps, or a jump to the other head. Nothing on the page draws a uniform index.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Implement a version of a Linked List where each node has an additional arbitrary pointer to within the list.</summary>

### Implement a version of a Linked List where each node has an additional arbitrary pointer to within the list.

**Definition:**

The page’s node link is next. Reverse also uses a temporary prev. An arbitrary extra pointer stored on every node is not part of the page.

**How it works:**

The intersection and nth-from-end algorithms only read and move along next. They do not allocate or follow a second per-node pointer.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

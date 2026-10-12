# Data Structures — Interview

Interview answers in this note are filled.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Data Structures — Interview](./7_ds_overview_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/).

### Arrays and Strings

<details>
<summary>Explain how you would reverse an array in place.</summary>

### Explain how you would reverse an array in place.

**Definition:**

In-place reversal modifies the original array without extra space.

**Examples:**

Here is a general-purpose implementation:

Code Example: Array Reversal

Here is the Python code:

```python
def reverse_array(arr):
    start, end = 0, len(arr) - 1
    while start < end:
        arr[start], arr[end] = arr[end], arr[start]
        start, end = start + 1, end - 1
        
my_array = [1, 2, 3, 4, 5]
print("Original Array:", my_array)
reverse_array(my_array)
print("Reversed Array:", my_array)
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>What is the difference between an array and a linked list?</summary>

### What is the difference between an array and a linked list?

**Definition:**

Let me put the two fundamental type of lists, Arrays and Linked Lists, into perspective.

**How it works:**

Key Distinctions

Data Organization

- **Array**: Employs sequential memory storage and each element has a unique index.
- **Linked List**: Elements are scattered in memory and accessed sequentially via references (pointers).

Memory Management

- **Array**: Typically requires a single, contiguous memory block.
- **Linked List**: Memory allocations are dynamic and non-contiguous.

Complexity Analysis

| Operation | Array | Linked List |
| --- | --- | --- |
| Access | `O(1)` (with index) | `O(n)` |
| Bulk Insertion | `O(n) or O(1)` | `O(1)` |
| Deletion | `O(n) to O(1)` | `O(1)` |

**When to use:**

When to Use Each

Arrays are preferable when:

There's a need for direct or random access such as in lookup tables.
The data will remain relatively unchanged, and performance in accessing elements takes precedence over frequent insertions or deletions.

Linked Lists are more suitable when:

Frequent insertions and deletions are expected, especially in the middle.
The exact size of the list isn't known in advance, and you want the memory to be used flexibly.
The primary operations are sequential, such as iteration from the beginning to the end.

**Examples:**

Code Example: Array vs. Linked List

Here is the Python code:

Array

```python
# Define array
my_array = [10, 20, 30, 40, 50]

# Access element by index
print(my_array[2])  # Output: 30

# Bulk insertion at the beginning
my_array = [5, 6, 7] + my_array
print(my_array)  # Output: [5, 6, 7, 10, 20, 30, 40, 50]

# Deletion from the middle
del my_array[4]
print(my_array)  # Output: [5, 6, 7, 10, 30, 40, 50]
```

Linked List

```python
# Define linked list nodes (in reality, you'd have a LinkedList class)
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Create linked list
head = Node(10)
node1 = Node(20)
node2 = Node(30)
head.next = node1
node1.next = node2

# Bulk insertion at the beginning
new_node1 = Node(5)
new_node2 = Node(6)
new_node3 = Node(7)
new_node3.next = head
head = new_node1
new_node1.next = new_node2
print_nodes(head)  # Output: 5, 6, 7, 10, 20, 30

# Deletion from the middle
new_node1.next = new_node3
# Now, just print_nodes(head) will output: 5, 6, 7, 20, 30
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>How would you check for duplicates in an array without using extra space?</summary>

### How would you check for duplicates in an array without using extra space?

**Definition:**

Checking for duplicates in an array without additional space is a common challenge with solutions using hash functions, sorting, and mathematical calculations.

**How it works:**

Brute Force Method

The code checks for duplicates based on numerical repetition.

Complexity Analysis

Time Complexity: `O(n^2)`

Space Complexity: `O(1)`

Sorting Approach

This method involves sorting the array using a comparison-based sorting algorithm like Quick Sort. If two adjacent elements are the same, then the array has duplicates.

Complexity Analysis

Time Complexity: Best/Worst: `O(n log n)`

- **Space Complexity**: `O(1) or O(n)` depending on sorting algorithm

Mathematical Approach

For this method, the sum of numbers in the array is calculated. Mathematically, if no duplicates are present, the sum of consecutive natural numbers can be calculated to compare against the actual sum.

If

`actual sum − sum of numbers in the array = 0`
, there are no duplicates.

**Examples:**

Code Implementation

Here is the Python code:

```python
def has_duplicates(arr):
    n = len(arr)
    for i in range(n):
        for j in range(i+1, n):
            if arr[i] == arr[j]:
                return True
    return False

arr = [1, 2, 3, 4, 3]
print(has_duplicates(arr))  # Output: True
```

Code Implementation

Here is the Python code:

```python
def has_duplicates_sorted(arr):
    arr.sort()
    n = len(arr)
    for i in range(n - 1):
        if arr[i] == arr[i+1]:
            return True
    return False

arr = [1, 2, 3, 4, 3]
print(has_duplicates_sorted(arr))  # Output: True
```

Code Implementation

Here is the Python code:

```python
def has_duplicates_math(arr):
    array_sum = sum(arr)
    n = len(arr)
    expected_sum = (n * (n-1)) // 2  # Sum of first (n-1) natural numbers
    return array_sum - expected_sum != 0

arr = [1, 2, 3, 4, 5, 5]
print(has_duplicates_math(arr))  # Output: True
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>Can you explain how to perform a binary search on a sorted array?</summary>

### Can you explain how to perform a binary search on a sorted array?

**Definition:**

Let's look at the high-level strategy behind binary search and then walk through a step-by-step example.

**How it works:**

Binary Search Strategy

- **Divide & Conquer**: Begin with the entire sorted array and refine the search range in each step.
- **Comparison**: Use the middle element to determine the next search range.
- **Repetition**: Continue dividing the array until the target is found or the search range is empty.

Step-by-Step Example

Let's consider the following array with the target value of 17:

```text
[1, 3, 6, 7, 9, 12, 15, 17, 20, 21]
```

- **Initial Pointers**: We start with the whole array.

```text
[1, 3, 6, 7, 9, 12, 15, 17, 20, 21]
^                               ^
Low                             High
```

- **Middle**: (Low + High) / 2 = 5
This identifies the Middle number as 12.

- **Comparison**: Since the Middle number is less than the target 17, we can discard the left portion of the array.

```text
[15, 17, 20, 21]
^           ^
Low        High
```

- **Updated Pointers**: We now have a reduced array to search.

Middle = 7
```text
^      ^
Low   High
```

Final Comparison:

Since the Middle number is now the target, 17, the search is successfully concluded.

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>How would you rotate a two-dimensional array by 90 degrees?</summary>

### How would you rotate a two-dimensional array by 90 degrees?

**Definition:**

Rotating a 2D array by

`90°` can be visually understood as a transpose followed by a reversal of rows or columns.

Algorithm: Transpose and Reverse

- **Transpose**: Swap each element `A[i][j]` with its counterpart `A[j][i]`

Reverse Rows (for `90°` CW) or Columns (for `90°` CCW)

Complexity Analysis

- **Time Complexity**: Both steps run in `O(n^2)` time.
- **Space Complexity**: Since we do an in-place rotation, it's `O(1)`.

**Examples:**

Code Example: Matrix Rotation

Here is the Python code:

```python
def rotate_2d_clockwise(matrix):
    n = len(matrix)
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse Rows
    for i in range(n):
        for j in range(n//2):
            matrix[i][j], matrix[i][n-j-1] = matrix[i][n-j-1], matrix[i][j]

    return matrix

def rotate_matrix_ccw(matrix):
    n = len(matrix)
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse Columns
    for i in range(n):
        for j in range(n//2):
            matrix[j][i], matrix[n-j-1][i] = matrix[n-j-1][i], matrix[j][i]

    return matrix

# Test 
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(rotate_2d_clockwise(matrix))
# Output: [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>Describe an algorithm to compress a string such as “aabbccc” to “a2b2c3”.</summary>

### Describe an algorithm to compress a string such as “aabbccc” to “a2b2c3”.

**Definition:**

You can compress a string following the count of each character. For example, "aabbccc" becomes "a2b2c3".

**How it works:**

Time Complexity

This algorithm has a time complexity of

`O(n)` since it processes each character of the input string exactly once.

Space Complexity

The space complexity is

`O(k)`

, where
k
is the length of the compressed string. This is because the output string is stored in memory.

**Examples:**

The python code for this algorithm is:

```python
def compress_string(input_string):
    # Initialize
    current_char = input_string[0]
    char_count = 1
    output = current_char

    # Iterate through the string
    for char in input_string[1:]:
        # If the character matches the current one, increment count
        if char == current_char:
            char_count += 1
        else:  # Append the count to the output and reset for the new character
            output += str(char_count) + char
            current_char = char
            char_count = 1

    # Append the last character's count
    output += str(char_count)

    # If the compressed string is shorter than the original string, return it
    return output if len(output) < len(input_string) else input_string
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>What is an array slice and how is it implemented in programming languages?</summary>

### What is an array slice and how is it implemented in programming languages?

**Definition:**

Let's look at what is an Array Slice and how it's implemented in some programming languages.

What is an Array Slice?
An array slice is a view on an existing array that acts as a smaller array. The slice references a continuous section of the original array which allows for efficient data access and manipulation.

Array slices are commonly used in languages like Python, Rust, and Go.

**How it works:**

Key Operations

- **Read**: Access elements in the slice.
- **Write**: Modify elements within the slice.
- **Grow/Shrink**: Resize the slice, often DWARF amortized.
- **Iteration**: Iterate over the elements in the slice.

Underlying Mechanism

A slice typically contains:

A pointer to the start of the slice.
The length of the slice (the number of elements in the slice).
The capacity of the slice (the maximum number of elements that the slice can hold).

Benefit of Use

- **No Copy Overhead**: Slices don't duplicate the underlying data; they're just references. This makes them efficient and memory-friendly.
- **Flexibility**: Slices can adapt as the array changes in size.
- **Safety**: Languages like Rust use slices for enforcing safety measures, preventing out-of-bounds access and memory issues.

Popular Implementations

- **Python**: Uses list slicing, with syntax like my_list[2:5]. This creates a new list.

- **Go Lang**: Employs slices extensively and is perhaps the most slice-oriented language out there.

- **Rust**: Similar to Go, it's a language heavily focused on memory safety, and slices are fundamental in that regard.

**Examples:**

Code Example: Array Slicing

Here is the Python code:

```python
original_list = [1, 2, 3, 4, 5]
my_slice = original_list[1:4]  # Creates a new list: [2, 3, 4]
```

Here is the Rust code:

```rust
let original_vec = vec![1, 2, 3, 4, 5];
let my_slice = &original_vec[1..4];  // References a slice: [2, 3, 4]
```

And here is the Go code:

```go
originalArray := [5]int{1, 2, 3, 4, 5}
mySlice := originalArray[1:4]  // References the originalArray from index 1 to 3
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>Can you discuss the time complexity of array insertion and deletion?</summary>

### Can you discuss the time complexity of array insertion and deletion?

**Definition:**

Both array insertions and deletions have a time complexity of

`O(n)` due to potential need for data re-arrangement.

Array Insertion

- **Beginning**: `O(n)` if array full; `1` for shifting.
- **Middle**: `O(n)` to make room and insert.
- **End**: `O(1)` on average for appending.

Array Deletion

- **Beginning**: `O(n)` due to re-arrangement often needed.
- **Middle**: `O(n)` as it involves shifting.
- **End**: `O(1)` for most cases, but `O(n)` when dynamic resizing is required.

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>What are some ways to merge two sorted arrays into one sorted array?</summary>

### What are some ways to merge two sorted arrays into one sorted array?

**Definition:**

Merging two sorted arrays into a new sorted array can be accomplished through a variety of well-established techniques.

**How it works:**

Methods of Merging Sorted Arrays

Using Additional Space:

Create a new array and add elements from both arrays using two pointers, then return the merged list.
- **Time Complexity**: `O(n + m)` - where `n and m` are the number of elements in each array. This approach is simple and intuitive.

Using a Min Heap:

Select the smallest element from both arrays using a min-heap and insert it into the new array.

Time Complexity: `O((n + m)log(n + m))`

- **Space Complexity**: `O(n + m)` - Heap might contain all the elements.
This approach is useful when the arrays are too large to fit in memory.

In-Place Merge:

Implement a merge similar to the one used in Merge Sort, directly within the input array.
- **Time Complexity**: `O(n ⋅ m)` - where `n and m` are the number of elements in each array.
In-Place Merging becomes inefficient as the number of insertions increases.

Using Binary Search:

Keep dividing the larger array into two parts and using binary search to find the correct position for elements in the smaller array.

Time Complexity: `O(m log n)`

Two-Pointer Technique:

Initialize two pointers, one for each array, and compare them to determine the next element in the merged array.

Time Complexity: `O(n + m)`

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>How do you find the kth largest element in an unsorted array?</summary>

### How do you find the kth largest element in an unsorted array?

**Definition:**

To find the

`kth` largest element in an unsorted array, you can leverage heaps or quicksort.

Quickselect Algorithm

- **Idea**: Partition the array using a pivot (similar to quicksort) and divide into subarrays until the partitioning index is the `kth` largest element.

Time Complexity:

- **Worst-case**: `O(n^2)` - This occurs when we're faced with the least optimized scenario, reducing
n
by only one element for each stitch step.
- **Average-case**: `O(n)` - Average performance is fast, making the expected time complexity linear.

Heap Method

Build a max-heap

`O(n)` - This takes linear time, making

`O(n) + O(k log n) = O(n + k log n)`.

Extract the max element

k
times (each time re-heapifying the remaining elements).

**Examples:**

Code Example: Python

```python
import random

def quickselect(arr, k):
    if arr:
        pivot = random.choice(arr)
        left = [x for x in arr if x < pivot]
        right = [x for x in arr if x > pivot]
        equal = [x for x in arr if x == pivot]
        if k < len(left):
            return quickselect(left, k)
        elif k < len(left) + len(equal):
            return pivot
        else:
            return quickselect(right, k - len(left) - len(equal))
```

Code Example: Python

```python
import heapq

def kth_largest_heap(arr, k):
    if k > len(arr): return None
    neg_nums = [-i for i in arr]
    heapq.heapify(neg_nums)
    k_largest = [heapq.heappop(neg_nums) for _ in range(k)]
    return -k_largest[-1]
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

### Linked Lists

<details>
<summary>Explain how a singly linked list differs from a doubly linked list.</summary>

### Explain how a singly linked list differs from a doubly linked list.

**Definition:**

Singly linked lists and doubly linked lists differ in how they manage node-to-node relationships.

**How it works:**

Structure

- **Singly Linked List**: Each node points to the next node.

- **Doubly Linked List**: Both previous and next nodes are pointed to.

Visual Representation

Singly Linked List

Singly Linked List

Doubly Linked List

Doubly Linked List

Key Distinctions

- **Access Direction**: Singly linked lists facilitate one-way traversal, while doubly linked lists support bi-directional traversal.

- **Head and Tail Movements**: Singly linked lists only operate on the head, while doubly linked lists can manipulate the head and tail.

- **Backward Traversal Efficiency**: Due to their structure, singly linked lists may be less efficient for backward traversal.

- **Memory Requirement**: Doubly linked lists use more memory as each node carries an extra pointer.

**Examples:**

Code Example: Singly Linked List

Here is the Java code:

```java
public class SinglyLinkedList {
    
    private static class Node {
        private int data;
        private Node next;

        public Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node head;

    public void insertFirst(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
    }

    public void display() {
        Node current = head;
        while (current != null) {
            System.out.println(current.data);
            current = current.next;
        }
    }
}
```

Code Example: Doubly Linked List

Here is the Java code:

```java
public class DoublyLinkedList {
    
    private static class Node {
        private int data;
        private Node previous;
        private Node next;

        public Node(int data) {
            this.data = data;
            this.previous = null;
            this.next = null;
        }
    }

    private Node head;
    private Node tail;

    public void insertFirst(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            head.previous = newNode;
            newNode.next = head;
            head = newNode;
        }
    }

    public void display() {
        Node current = head;
        while (current != null) {
            System.out.println(current.data);
            current = current.next;
        }
    }

    public void displayBackward() {
        Node current = tail;
        while (current != null) {
            System.out.println(current.data);
            current = current.previous;
        }
    }
}
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>How would you detect a cycle in a linked list?</summary>

### How would you detect a cycle in a linked list?

**Definition:**

Cycle detection in a linked list is a fundamental algorithm that uses pointers to identify if a linked list has a repeating sequence.

**How it works:**

Floyd's "Tortoise and Hare" Algorithm

Floyd's algorithm utilizes two pointers:

The "tortoise" moves one step each iteration.
The "hare" moves two steps.
If the linked list does not have a cycle, the hare either reaches the end (or null) before the tortoise, or vice versa. However, if there is a cycle, the two pointers are guaranteed to meet inside the cycle.

Algorithm Steps

Initialize both pointers to the start of the linked list.
Move the tortoise one step and the hare two steps.

Continuously advance the pointers in their respective steps:

If the tortoise reaches the hare (a collision point), return such a point.
If either pointer reaches the end (null), conclude there is no cycle.

Visual Representation

Floyd's Algorithm

Complexity Analysis

Time Complexity: `O(n)` where

n
is the number of nodes in the linked list, due to each pointer visiting each node only once.
- **Space Complexity**: `O(1)` as the algorithm uses only a constant amount of extra space.

**Examples:**

Code Example: Floyd's Cycle Detection

Here is the Python code:

```python
def has_cycle(head):
    tortoise = head
    hare = head

    while hare and hare.next:
        tortoise = tortoise.next
        hare = hare.next.next

        if tortoise == hare:
            return True

    return False
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>What are the major operations you can perform on a linked list, and their time complexities?</summary>

### What are the major operations you can perform on a linked list, and their time complexities?

**Definition:**

Let's look at the major operations you can perform on a singly linked list and their associated time complexities:

**How it works:**

Operations & Time Complexities

Access (Read/Write)

`O(n)`

- **Head**: Constant time: `O(1)`.
- **Tail**: `O(n)` without a tail pointer, but constant with a tail pointer.
- **Middle or k-th Element**: `n^2` is around the middle node; getting k-th element requires `O(k)`.

Search

`O(n)`

- **Unordered**: May require scanning the entire list. Worst case: `O(n)`.
- **Ordered**: You can stop as soon as the value exceeds what you're looking for.

Insertion

`O(1)` without tail pointer,

`O(n)` with tail pointer

Head: `O(1)`

- **Tail**: `O(1)` with a tail pointer, otherwise `O(n)`.
- **Middle**: `O(1)` with tail pointer and finding position in `O(1)` time; otherwise, it's `O(n)`.

Deletion

`O(1)` for Head and Tail,

`O(n)` otherwise

Head: `O(1)`

- **Tail**: `O(n)` because you must find the node before the tail for pointer reversal with a single pass.
- **Middle**: `O(n)` since you need to find the node before the one to be deleted.

Length

`O(n)`

- **Naive**: Requires a full traversal. Every addition or removal requires this traversal.
- **Keep Count**: Maintain a separate counter, updating it with each addition or removal.

**Examples:**

Code Example: Singly Linked List Basic Operations

Here is the Python code:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None
    
    def append(self, data):  # O(n) without tail pointer
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last_node = self.head
        while last_node.next:
            last_node = last_node.next
        last_node.next = new_node
    
    def delete(self, data):  # O(n) only if element is not at head
        current_node = self.head
        if current_node.data == data:
            self.head = current_node.next
            current_node = None
            return
        while current_node:
            if current_node.data == data:
                break
            prev = current_node
            current_node = current_node.next
        if current_node is None:
            return
        prev.next = current_node.next
        current_node = None

    def get_middle(self):  # O(n)
        slow, fast = self.head, self.head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def get_kth(self, k):  # O(k)
        current_node, count = self.head, 0
        while current_node:
            count += 1
            if count == k:
                return current_node
            current_node = current_node.next
        return None

    # Other methods: display, length, etc.
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>Can you describe an in-place algorithm to reverse a linked list?</summary>

### Can you describe an in-place algorithm to reverse a linked list?

**Definition:**

In-Place Algorithms modify data structures with a constant amount of extra working space `O(1)`.

A Singly Linked List presents a straightforward example of an in-place data structure, well-suited for in-place reversal algorithms.

**How it works:**

Reversing a Linked List: Core Concept

The reversal algorithm just needs to update each node's next reference so that they point to the previous node. A few key steps achieve this:

- **Initialize**: Keep track of the three key nodes: previous, current, and next.
- **Reverse Links**: Update each node to instead point to the previous one in line.
- **Move Pointers**: Shift previous, current, and next nodes by one position for the next iteration.
This process proceeds iteratively until current reaches the end, i.e., NULL.

Complexity Analysis

- **Time Complexity**: The algorithm exhibits a linear time complexity of `O(n)` as it visits each node once.
- **Space Complexity**: As the algorithm operates in-place, only a constant amount of extra space (for nodes pointers) is required: `O(1)`.

**Examples:**

Code Example: In-Place List Reversal

Here is the Python code:

```python
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last_node = self.head
        while last_node.next:
            last_node = last_node.next
        last_node.next = new_node

    def reverse_inplace(self):
        previous = None
        current = self.head
        while current:
            next_node = current.next
            current.next = previous
            previous = current
            current = next_node
        self.head = previous

    def display(self):
        elements = []
        current = self.head
        while current:
            elements.append(current.data)
            current = current.next
        print(" -> ".join(str(data) for data in elements))

# Populate the linked list
llist = LinkedList()
values = [4, 2, 8, 3, 1, 9]
for value in values:
    llist.append(value)

# Display original
print("Original Linked List:")
llist.display()

# Reverse in-place and display
llist.reverse_inplace()
print("\nAfter Reversal:")
llist.display()
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>Explain how you would find the middle element of a linked list in one pass.</summary>

### Explain how you would find the middle element of a linked list in one pass.

**Definition:**

Finding the middle element of a linked list is a common problem with several efficient approaches, such as the two-pointer (or "runner") technique.

**How it works:**

Two-Pointer Technique

Explanation

The two-pointer technique uses two pointers, often named slow and fast, to traverse the list. While fast moves two positions at a time, slow trails behind, covering a single position per move. When fast reaches the end, slow will be standing on the middle element.

Example

- **Given the linked list**: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7

The pointers will traverse as follows:

(1) slow: 1; fast: 2
(2) slow: 2; fast: 4
(3) slow: 3; fast: 6
(4) slow: 4; fast: end
At (4), the slow pointer has reached the middle point.

Complexity Analysis

- **Time Complexity**: `O(N)` -- For every N nodes, we check each node once.
- **Space Complexity**: `O(1)` -- We only use pointers; no extra data structures are involved.

**Examples:**

Code Example: Two-Pointer (Runner) technique

Here is the Python implementation:

```python
def find_middle_node(head):
    if not head:
        return None

    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

**Sources:**

- [Devinterview.io — Data Structures](https://devinterview.io/questions/data-structures-and-algorithms/data-structures-interview-questions/)
</details>

<details>
<summary>What are the advantages and disadvantages of using a linked list over an array?</summary>

### What are the advantages and disadvantages of using a linked list over an array?

**Definition:**

A linked list stores each element in its own node with a pointer to the next node. An array stores elements in one contiguous block and addresses them by index.

**How it works:**

GeeksforGeeks lists the tradeoff as follows. If you already hold the node, insertion or deletion updates a pointer and is O(1); an array shifts elements and is O(n) in the middle. A linked list grows one node at a time. The costs are O(n) access, extra memory per pointer, and poor cache locality because nodes are not contiguous. An array reads index i in O(1) and scans faster because neighbors share a cache line.

**When to use:**

Use a linked list when you insert and delete at a known node and rarely need an index. Use an array when you need random access or a tight scan.

**Examples:**

```python
# index 2 is one address calculation
value = items[2]
# the third node is two hops
node = head.next.next
```

**Sources:**

- [Linked List vs Array](https://www.geeksforgeeks.org/dsa/linked-list-vs-array/)
</details>

<details>
<summary>How can you implement a queue using linked lists?</summary>

### How can you implement a queue using linked lists?

**Definition:**

A queue is FIFO. A singly linked list with a head and a tail pointer enqueues at the tail and dequeues at the head, both in O(1).

**How it works:**

The front of the queue is the head node. `enqueue` creates a node, links it from the current tail, and moves the tail forward. On an empty queue the new node is both head and tail. `dequeue` returns the head value and advances head. When the last node leaves, tail becomes null too. No element shifts.

**Examples:**

```python
def enqueue(self, value):
    node = Node(value)
    if self.tail is None:
        self.head = self.tail = node
    else:
        self.tail.next = node
        self.tail = node

def dequeue(self):
    if self.head is None:
        raise IndexError("underflow")
    value = self.head.value
    self.head = self.head.next
    if self.head is None:
        self.tail = None
    return value
```

**Sources:**

- [Queue using Linked List](https://www.geeksforgeeks.org/dsa/queue-linked-list-implementation/)
</details>

<details>
<summary>Discuss how a linked list can be used to implement a stack.</summary>

### Discuss how a linked list can be used to implement a stack.

**Definition:**

A stack is LIFO. Push and pop at the head of a singly linked list are both O(1), and the stack grows until memory runs out.

**How it works:**

The top is the head. `push` inserts a new node before the current head. `pop` removes the head and returns its value. `peek` reads the head without unlinking it. An empty head means underflow. Unlike a fixed array, this stack does not overflow at a preset capacity.

**Examples:**

```python
def push(self, value):
    self.head = Node(value, self.head)

def pop(self):
    if self.head is None:
        raise IndexError("underflow")
    value = self.head.value
    self.head = self.head.next
    return value
```

**Sources:**

- [Stack using Linked List](https://www.geeksforgeeks.org/dsa/implement-a-stack-using-singly-linked-list/)
</details>

<details>
<summary>How would you remove a given node from a linked list without head pointer?</summary>

### How would you remove a given node from a linked list without head pointer?

**Definition:**

If you hold the node and it is not the last node, copy the next node's data into this node and unlink the next node. That is O(1) and does not need the head.

**How it works:**

You cannot walk backward in a singly linked list, so you cannot change the previous node's pointer. Overwriting this node with `node.next` makes the list skip the original successor. The last node has no successor to copy, so this trick does not delete the tail. A doubly linked list can unlink the given node directly when it has a previous pointer.

**Examples:**

```python
def delete_node(node):
    if node is None or node.next is None:
        raise ValueError("need a non-tail node")
    node.value = node.next.value
    node.next = node.next.next
```

**Sources:**

- [Delete a node without the head pointer](https://www.geeksforgeeks.org/dsa/delete-a-node-from-linked-list-without-head-pointer/)
</details>

<details>
<summary>Can you explain the concept of a sentinel node in a linked list?</summary>

### Can you explain the concept of a sentinel node in a linked list?

**Definition:**

A sentinel is a permanent dummy node that does not store a list element. It removes the empty-list and end-of-list special cases from insert and delete.

**How it works:**

GeeksforGeeks shows a doubly linked list with one sentinel at the start and one at the end. Every real insert happens between two existing nodes, so the same splice updates four pointers. The list is empty when the head sentinel's next node is the tail sentinel. A header node is the singly linked form of the same idea: `start.next is None` means empty.

**When to use:**

Use sentinels when the same insert and delete code should work at both ends. Skip them when the extra nodes are not worth it in a tiny list.

**Examples:**

```python
class List:
    def __init__(self):
        self.head = Node(None)
        self.tail = Node(None)
        self.head.next = self.tail
        self.tail.prev = self.head
```

**Sources:**

- [Doubly Linked List using Sentinel Nodes](https://www.geeksforgeeks.org/dsa/doubly-linked-list-using-sentinel-nodes/)
- [Header Linked List](https://www.geeksforgeeks.org/dsa/header-linked-list-in-c/)
</details>

### Stacks and Queues

<details>
<summary>What operations are performed by a stack data structure?</summary>

### What operations are performed by a stack data structure?

**Definition:**

A stack is a LIFO collection. The core operations are push, pop, and peek (top), plus an emptiness test.

**How it works:**

Push adds an element at the top. Pop removes and returns the top. Peek reads the top without removing it. On a linked-list stack each of these is O(1). On a fixed array, push fails when the array is full (overflow) and pop fails when it is empty (underflow). Size is the number of stored elements.

**Examples:**

```python
stack.append(1)   # push
top = stack[-1]   # peek
value = stack.pop()
```

**Sources:**

- [Stack Data Structure](https://www.geeksforgeeks.org/dsa/stack-data-structure/)
</details>

<details>
<summary>How would you evaluate an arithmetic expression using stacks?</summary>

### How would you evaluate an arithmetic expression using stacks?

**Definition:**

A stack evaluates a postfix expression in one left-to-right pass. Operands are pushed. An operator pops its inputs, applies, and pushes the result.

**How it works:**

For `2 3 1 * + 9 -`, push 2, 3, and 1. `*` pops 1 and 3 and pushes 3. `+` pops 3 and 2 and pushes 5. `-` pops 9 and 5 and pushes -4. The final stack holds one value. Infix expressions are converted to postfix with a second operator stack that respects precedence, then evaluated the same way. One pass over n tokens is O(n).

**Examples:**

```python
def eval_postfix(tokens):
    stack = []
    for token in tokens:
        if token in {"+", "-", "*"}:
            right, left = stack.pop(), stack.pop()
            stack.append(eval(f"{left}{token}{right}"))
        else:
            stack.append(int(token))
    return stack[-1]
```

**Sources:**

- [Evaluation of Postfix Expression](https://www.geeksforgeeks.org/dsa/evaluation-of-postfix-expression/)
</details>

<details>
<summary>Can you create a queue using two stacks? If so, describe how.</summary>

### Can you create a queue using two stacks? If so, describe how.

**Definition:**

Yes. Two stacks simulate a FIFO queue. One stack accepts new items. The other serves the front. Moving items between them reverses order, which turns LIFO into FIFO.

**How it works:**

GeeksforGeeks keeps an input stack and an output stack. `enqueue` pushes onto the input stack, O(1). `dequeue` pops the output stack when it is non-empty, O(1). When the output stack is empty, every input element is popped and pushed onto the output stack, O(n) for that call. Each element is moved at most once, so a sequence of operations is amortized O(1) per operation.

**Examples:**

```python
def dequeue(self):
    if not self.output:
        while self.input:
            self.output.append(self.input.pop())
    if not self.output:
        raise IndexError("underflow")
    return self.output.pop()
```

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Describe how you would implement a stack using queues.</summary>

### Describe how you would implement a stack using queues.

**Definition:**

A stack can be built from queues by keeping the newest element at the front of one queue, so pop and top read that front.

**How it works:**

GeeksforGeeks gives two shapes. With two queues, push enqueues into an empty helper, moves every old element behind it, and swaps the names, so push is O(n) and pop is O(1). The other shape pushes in O(1) and, on pop, moves elements until one remains. A single queue can also rotate: after enqueue, dequeue and enqueue the previous elements so the new item sits at the front. Push is then O(n), pop and top are O(1).

**When to use:**

Pay O(n) on push when pops are more common. Pay O(n) on pop when pushes are more common.

**Examples:**

```python
def push(self, value):
    self.q.append(value)
    for _ in range(len(self.q) - 1):
        self.q.append(self.q.popleft())
```

**Sources:**

- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

<details>
<summary>How would you distinguish a stack overflow from an underflow?</summary>

### How would you distinguish a stack overflow from an underflow?

**Definition:**

Overflow means push was asked when the stack cannot take another element. Underflow means pop or peek was asked when the stack has no element.

**How it works:**

A fixed array stack overflows when `top == capacity - 1`. It underflows when `top == -1` before a pop. A linked-list stack does not hit a fixed capacity, so overflow there is an out-of-memory failure, while underflow is still an empty head. Callers should test emptiness before pop. Growing the array is a separate dynamic-array policy, not a successful push into a full fixed buffer.

**Examples:**

```python
def push(self, value):
    if self.top + 1 == len(self.data):
        raise OverflowError("overflow")
    self.top += 1
    self.data[self.top] = value
```

**Sources:**

- [Stack using Array](https://www.geeksforgeeks.org/dsa/implement-stack-using-array/)
- [Stack Data Structure](https://www.geeksforgeeks.org/dsa/stack-data-structure/)
</details>

<details>
<summary>What is a priority queue and how can it be implemented?</summary>

### What is a priority queue and how can it be implemented?

**Definition:**

A priority queue removes the element with the best priority, not the oldest element. A binary heap is the usual implementation.

**How it works:**

A min-heap stores the smallest key at the root. Insert sifts the new key up. Delete-min replaces the root with the last leaf and sifts down. Both are O(log n) in a binary heap. An unsorted array makes insert O(1) and delete-min O(n). A sorted array reverses those costs. Dijkstra's algorithm uses a min-heap of (distance, vertex) pairs for this reason.

**When to use:**

Use a binary heap when both insert and delete-min must be logarithmic. Use a sorted list only when one of those operations is rare.

**Examples:**

```python
import heapq
heap = []
heapq.heappush(heap, (2, "b"))
heapq.heappush(heap, (1, "a"))
print(heapq.heappop(heap))  # (1, "a")
```

**Sources:**

- [Priority Queue](https://www.geeksforgeeks.org/dsa/priority-queue-set-1-introduction/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Explain the difference between a LIFO and a FIFO data structure.</summary>

### Explain the difference between a LIFO and a FIFO data structure.

**Definition:**

LIFO returns the newest element first. FIFO returns the oldest element first. A stack is LIFO. A queue is FIFO.

**How it works:**

Stack push and pop share one end, so the last push is the next pop. Queue enqueue and dequeue use opposite ends, so the first enqueue is the next dequeue. A priority queue is neither: it returns by key order. Choosing the wrong order breaks parsers (they need LIFO) and task buffers (they need FIFO).

**Examples:**

```python
stack = []
stack.append("a"); stack.append("b")
stack.pop()  # "b", LIFO

from collections import deque
q = deque(["a", "b"])
q.popleft()  # "a", FIFO
```

**Sources:**

- [Stack Data Structure](https://www.geeksforgeeks.org/dsa/stack-data-structure/)
- [Queue using Linked List](https://www.geeksforgeeks.org/dsa/queue-linked-list-implementation/)
</details>

<details>
<summary>Can you describe a scenario in which a circular queue would be useful?</summary>

### Can you describe a scenario in which a circular queue would be useful?

**Definition:**

A circular queue reuses empty slots at the front of a fixed array instead of shifting elements after each dequeue.

**How it works:**

Head and tail move forward with `index % capacity`. The queue is full when the next tail slot is the head, so one slot is often left unused or a size counter distinguishes full from empty. GeeksforGeeks presents this array form for a bounded buffer. A producer and consumer that share a fixed block of memory, such as a keyboard buffer or a streaming window, can run without allocating a node per item.

**When to use:**

Use it for a fixed-capacity buffer with frequent enqueue and dequeue. Use a linked queue when the bound is unknown.

**Examples:**

```python
def enqueue(self, value):
    nxt = (self.tail + 1) % self.capacity
    if nxt == self.head:
        raise OverflowError("full")
    self.data[self.tail] = value
    self.tail = nxt
```

**Sources:**

- [Circular Queue](https://www.geeksforgeeks.org/dsa/circular-queue-set-1-introduction-array-implementation/)
</details>

<details>
<summary>How would you design a stack that supports a special operation that retrieves the minimum element in constant time?</summary>

### How would you design a stack that supports a special operation that retrieves the minimum element in constant time?

**Definition:**

Pair the value stack with a second stack that stores the minimum at or below the current height. Push, pop, and getMin are then all O(1).

**How it works:**

On push, push the value and also push `min(value, current minimum)` onto the min stack. On pop, pop both stacks. getMin reads the top of the min stack. GeeksforGeeks describes this extra O(n) space as the way to keep getMin in O(1) time. A single stack of pairs `(value, min_so_far)` is the same idea.

**Examples:**

```python
def push(self, value):
    current = value if not self.mins else min(value, self.mins[-1])
    self.values.append(value)
    self.mins.append(current)

def get_min(self):
    return self.mins[-1]
```

**Sources:**

- [Design a stack with getMin in O(1)](https://www.geeksforgeeks.org/dsa/design-a-stack-that-supports-getmin-in-o1-time-and-o1-extra-space/)
</details>

<details>
<summary>Discuss the potential problems you might encounter when using dynamic arrays for stacks.</summary>

### Discuss the potential problems you might encounter when using dynamic arrays for stacks.

**Definition:**

A dynamic array stack appends in amortized O(1), but a growth copies every element into a larger buffer, and a shrink can do the same in reverse.

**How it works:**

When `size == capacity`, allocation of a new array is O(n) and invalidates old indexes. Doubling capacity makes a sequence of pushes amortized O(1), yet one push can still pause. Unused capacity wastes memory after a burst of pops unless you shrink, and shrinking too eagerly thrashes if the size hovers at the threshold. The array is one contiguous block, so a large resize can fail even when total free memory would fit a linked stack.

**Examples:**

```python
def push(self, value):
    if len(self.data) == self.capacity:
        self.data = self.data + [None] * self.capacity
        self.capacity *= 2
    self.data[self.size] = value
    self.size += 1
```

**Sources:**

- [Stack using Array](https://www.geeksforgeeks.org/dsa/implement-stack-using-array/)
- [Linked List vs Array](https://www.geeksforgeeks.org/dsa/linked-list-vs-array/)
</details>

### Trees

<details>
<summary>Describe a binary tree and its properties.</summary>

### Describe a binary tree and its properties.

**Definition:**

A binary tree is a rooted tree in which every node has at most two children, usually called left and right.

**How it works:**

A node stores a value and two child links. The depth of the root is 0. The height is the number of edges on the longest root-to-leaf path. A perfect binary tree of height h has 2^(h+1) - 1 nodes. A full binary tree has 0 or 2 children on every node. A complete binary tree fills every level except possibly the last, which is filled from the left. There is exactly one path between any two nodes because a tree has no cycles.

**Examples:**

```python
class Node:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right
```

**Sources:**

- [Binary Tree](https://www.geeksforgeeks.org/dsa/binary-tree-data-structure/)
</details>

<details>
<summary>How do you perform an in-order tree traversal recursively?</summary>

### How do you perform an in-order tree traversal recursively?

**Definition:**

In-order visits the left subtree, then the node, then the right subtree. On a binary search tree that order is sorted.

**How it works:**

The call returns immediately on a null node. Otherwise it recurses left, records the node, and recurses right. A tree of n nodes is visited once, so the time is O(n). The call stack is O(h), which is O(log n) when the tree is balanced and O(n) when it is a chain.

**Examples:**

```python
def inorder(node, out):
    if node is None:
        return
    inorder(node.left, out)
    out.append(node.value)
    inorder(node.right, out)
```

**Sources:**

- [Tree Traversals](https://www.geeksforgeeks.org/dsa/tree-traversals-inorder-preorder-and-postorder/)
</details>

<details>
<summary>Can you write an algorithm for a non-recursive post-order traversal?</summary>

### Can you write an algorithm for a non-recursive post-order traversal?

**Definition:**

Post-order visits left, right, then the node. An explicit stack records the path so the node is emitted only after both children.

**How it works:**

Walk left, pushing nodes. When a node has no unvisited right child, pop and emit it. If the right child exists and was not just emitted, push the node back and walk that right child. A previous-pointer tells you the right child is done. Time is O(n). Extra space is O(h).

**Examples:**

```python
def postorder(root):
    out, stack, prev = [], [], None
    node = root
    while node or stack:
        while node:
            stack.append(node)
            node = node.left
        peek = stack[-1]
        if peek.right and prev is not peek.right:
            node = peek.right
        else:
            out.append(stack.pop().value)
            prev = peek
    return out
```

**Sources:**

- [Tree Traversals](https://www.geeksforgeeks.org/dsa/tree-traversals-inorder-preorder-and-postorder/)
</details>

<details>
<summary>What are the advantages of a balanced binary tree over an unbalanced one?</summary>

### What are the advantages of a balanced binary tree over an unbalanced one?

**Definition:**

A balanced binary tree keeps its height O(log n). An unbalanced tree can become a chain of height n, and then search, insert, and delete become O(n).

**How it works:**

AVL trees and red-black trees restore balance after updates, so those operations stay O(log n). The extra work is a few rotations. An unbalanced binary search tree built from sorted input is a linked list: each comparison walks one more node, and the recursion depth is n. Balance does not change the O(n) cost of visiting every node.

**When to use:**

Balance the tree when lookups and updates are mixed and n is large. A plain binary tree is enough for a one-time hierarchy that you only traverse.

**Examples:**

```python
# sorted inserts without balance: height == n - 1
# balanced inserts: height is about log2(n)
```

**Sources:**

- [AVL Tree](https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/)
- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
</details>

<details>
<summary>Explain what a binary search tree (BST) is and how it’s different from other trees.</summary>

### Explain what a binary search tree (BST) is and how it’s different from other trees.

**Definition:**

A binary search tree is a binary tree with an order: every key in the left subtree is smaller than the node, and every key in the right subtree is larger.

**How it works:**

Search, insert, and delete follow one path, so they are O(h). That is O(log n) when the tree stays balanced and O(n) when it does not. An in-order walk emits keys in sorted order. A general binary tree has no such order, so search is O(n). A heap orders parents against children only, so it finds the minimum in O(1) but cannot find an arbitrary key in O(log n).

**Examples:**

```python
def search(node, key):
    if node is None or node.value == key:
        return node
    if key < node.value:
        return search(node.left, key)
    return search(node.right, key)
```

**Sources:**

- [Binary Search Tree](https://www.geeksforgeeks.org/dsa/binary-search-tree-data-structure/)
</details>

<details>
<summary>How would you check if a binary tree is a BST?</summary>

### How would you check if a binary tree is a BST?

**Definition:**

The tree is a BST when an in-order walk is strictly increasing, or when every node lies inside a range inherited from its ancestors.

**How it works:**

The range method passes a low and high bound. The root may be any value. The left child must be inside `(low, node.value)` and the right child inside `(node.value, high)`. A null node is valid. This is O(n) time and O(h) stack. Comparing a node only with its parent is not enough: a grandchild can still break the ancestor's bound.

**Examples:**

```python
def is_bst(node, low=float("-inf"), high=float("inf")):
    if node is None:
        return True
    if not (low < node.value < high):
        return False
    return is_bst(node.left, low, node.value) and is_bst(node.right, node.value, high)
```

**Sources:**

- [Check if a binary tree is BST](https://www.geeksforgeeks.org/dsa/a-program-to-check-if-a-binary-tree-is-bst-or-not/)
</details>

<details>
<summary>Discuss the method for finding the lowest common ancestor (LCA) in a BST.</summary>

### Discuss the method for finding the lowest common ancestor (LCA) in a BST.

**Definition:**

The lowest common ancestor of two nodes is the deepest node that has both as descendants. In a BST the search path decides it without a full traversal.

**How it works:**

Start at the root. If both keys are smaller, the ancestor is in the left subtree. If both are larger, it is in the right subtree. Otherwise this node is the split point and the ancestor. The walk is O(h). A general binary tree does not have this test and uses parent pointers or a post-order that returns when each side reports a match.

**Examples:**

```python
def lca(node, a, b):
    while node:
        if a < node.value and b < node.value:
            node = node.left
        elif a > node.value and b > node.value:
            node = node.right
        else:
            return node
```

**Sources:**

- [Lowest Common Ancestor in a BST](https://www.geeksforgeeks.org/dsa/lowest-common-ancestor-in-a-binary-search-tree/)
</details>

<details>
<summary>What are the use cases for a depth-first search versus a breadth-first search in a tree structure?</summary>

### What are the use cases for a depth-first search versus a breadth-first search in a tree structure?

**Definition:**

Depth-first search follows one branch to a leaf before backtracking. Breadth-first search finishes each level before the next. Both visit n nodes and n - 1 edges, so both are O(n) on a tree.

**How it works:**

DFS uses a stack or recursion of depth O(h). It fits path problems, detecting a root-to-leaf condition, and the preorder, inorder, and postorder walks. BFS uses a queue whose width is the widest level. It finds the shortest path in an unweighted tree, because the first time a node is reached is by the fewest edges. GeeksforGeeks notes BFS explores level by level and DFS goes deep first.

**When to use:**

Use BFS for the minimum number of edges. Use DFS when the answer is a root-to-leaf path or an inorder listing.

**Examples:**

```python
from collections import deque

def bfs(root):
    queue = deque([root])
    while queue:
        node = queue.popleft()
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```

**Sources:**

- [BFS vs DFS](https://www.geeksforgeeks.org/dsa/difference-between-bfs-and-dfs/)
</details>

<details>
<summary>Describe how you would convert a binary tree into a doubly linked list.</summary>

### Describe how you would convert a binary tree into a doubly linked list.

**Definition:**

An in-order walk can turn the same nodes into a doubly linked list. Left becomes previous, right becomes next, and the leftmost node becomes the head.

**How it works:**

GeeksforGeeks keeps `prev`, the last visited node. Visit the left subtree. If `prev` is null, the current node is the head. Otherwise set `prev.right` to the current node and `current.left` to `prev`. Then set `prev` to the current node and visit the right subtree. The conversion is in place, O(n) time, and O(h) recursion stack. No new node is allocated for the list.

**Examples:**

```python
def to_dll(node, state):
    if node is None:
        return
    to_dll(node.left, state)
    if state["prev"] is None:
        state["head"] = node
    else:
        state["prev"].right = node
        node.left = state["prev"]
    state["prev"] = node
    to_dll(node.right, state)
```

**Sources:**

- [Convert Binary Tree to Doubly Linked List](https://www.geeksforgeeks.org/dsa/convert-binary-tree-to-doubly-linked-list-using-inorder-traversal/)
</details>

<details>
<summary>How can you serialize and deserialize a binary tree?</summary>

### How can you serialize and deserialize a binary tree?

**Definition:**

Serialization writes the tree into a list that can be rebuilt into the same shape. A preorder walk with an explicit null marker is enough.

**How it works:**

Write the node value, then the left subtree, then the right subtree. Write a sentinel such as `None` for a missing child. Deserialization reads the next token: a null starts no node; a value creates a node and then reads its left and right children from the same stream. Both directions are O(n). The null markers are required, or the shape is ambiguous.

**Examples:**

```python
def serialize(node):
    if node is None:
        return ["#"]
    return [str(node.value)] + serialize(node.left) + serialize(node.right)
```

**Sources:**

- [Serialize and Deserialize a Binary Tree](https://www.geeksforgeeks.org/dsa/serialize-deserialize-binary-tree/)
</details>

### Hash Tables

<details>
<summary>What is a hash table and how does it work internally?</summary>

### What is a hash table and how does it work internally?

**Definition:**

A hash table stores keys in an array. A hash function turns a key into an index. Average lookup, insert, and delete are O(1). The worst case is O(n) when many keys land in one slot.

**How it works:**

The table length is m. The index is `hash(key) modulo m`. Two keys with the same index collide. Separate chaining stores a list in that slot. Open addressing probes another slot. The load factor is n / m. A good function spreads keys so chains stay short.

**Examples:**

```python
index = hash(key) % len(buckets)
buckets[index].append((key, value))
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Can you explain collision resolution in hash tables?</summary>

### Can you explain collision resolution in hash tables?

**Definition:**

A collision is two keys mapping to one index. Separate chaining and open addressing are the standard fixes.

**How it works:**

Chaining stores a linked list (or another structure) at the slot. Insert is at the head of that list. Search walks the chain. Open addressing keeps one item per slot and probes: linear probing steps by 1, quadratic probing steps by squares, double hashing uses a second hash. Chaining uses extra pointers. Open addressing needs a load factor below 1 and degrades as the table fills.

**When to use:**

Use chaining when the load factor may exceed 1. Use open addressing when you want one array and can keep the table partly empty.

**Examples:**

```python
slot = buckets[hash(key) % len(buckets)]
for stored, value in slot:
    if stored == key:
        return value
```

**Sources:**

- [Separate Chaining](https://www.geeksforgeeks.org/dsa/hashing-set-2-separate-chaining/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>What is a hash function and what makes a good hash function?</summary>

### What is a hash function and what makes a good hash function?

**Definition:**

A hash function maps a key to an integer index. A good one is deterministic, fast, and spreads keys uniformly across the table.

**How it works:**

The same key must always produce the same hash. Computing it should be cheap next to the rest of the operation. Uniform spread keeps chains near the load factor instead of dumping keys into one slot. Cryptographic hashes are not required for a table. For strings, a polynomial rolling hash is a common teaching choice. A function that uses only the first character clumps keys that share a prefix.

**Examples:**

```python
def hash_string(text, m):
    acc = 0
    for ch in text:
        acc = (acc * 31 + ord(ch)) % m
    return acc
```

**Sources:**

- [Choosing a hash function](https://www.geeksforgeeks.org/dsa/what-are-hash-functions-and-how-to-choose-a-good-hash-function/)
</details>

<details>
<summary>How would you design a data structure for an LRU cache?</summary>

### How would you design a data structure for an LRU cache?

**Definition:**

An LRU cache drops the least recently used entry when it is full. A hash map plus a doubly linked list does get and put in O(1).

**How it works:**

The map points from key to list node. The list orders nodes from most recent to least recent. On get, move that node to the front. On put of a new key at capacity, unlink the tail and delete it from the map, then insert the new node at the front and record it in the map. A hit updates the value and the position. GeeksforGeeks uses this pair of structures for O(1) operations.

**Examples:**

```python
def get(self, key):
    node = self.index.get(key)
    if node is None:
        return None
    self._move_front(node)
    return node.value
```

**Sources:**

- [LRU Cache](https://www.geeksforgeeks.org/dsa/lru-cache-implementation/)
</details>

<details>
<summary>Discuss the trade-offs between using an array versus a hash table.</summary>

### Discuss the trade-offs between using an array versus a hash table.

**Definition:**

An array gives O(1) access by a dense integer index and stores elements contiguously. A hash table gives average O(1) access by an arbitrary key and uses extra space for buckets and pointers.

**How it works:**

Use the array when keys are a compact range such as 0..n-1: there is no hash, no collision, and scans are cache-friendly. Use the hash table when keys are strings, ids, or a sparse set. The hash table's worst case is O(n) under a bad function or a high load factor. It does not keep key order. An array does not grow an entry for a missing key.

**When to use:**

Prefer the array for dense indexes. Prefer the hash table for sparse or non-integer keys.

**Examples:**

```python
present = [False] * (n + 1)   # dense ids
present = set()                # sparse ids
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Linked List vs Array](https://www.geeksforgeeks.org/dsa/linked-list-vs-array/)
</details>

<details>
<summary>What are some common uses for a hash set data structure?</summary>

### What are some common uses for a hash set data structure?

**Definition:**

A hash set stores keys with no associated value. Membership, insertion, and deletion are average O(1).

**How it works:**

Typical uses are deduplicating a stream, recording visited graph nodes, and testing whether a complement such as `target - x` was already seen. It does not provide order or an index. A boolean array replaces it when the key domain is a small integer range.

**Examples:**

```python
seen = set()
if x not in seen:
    seen.add(x)
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>How can you count the frequency of words in a long piece of text efficiently?</summary>

### How can you count the frequency of words in a long piece of text efficiently?

**Definition:**

One pass with a hash map counts each word in average O(1), so the whole text of n words is average O(n).

**How it works:**

Split the text into words. For each word, add 1 to `counts[word]`, inserting 0 on the first sight. The map uses space proportional to the number of distinct words, not the length of the text. Sorting the words first also counts runs, but that is O(n log n). A trie counts prefixes as well as whole words when the question needs both.

**Examples:**

```python
counts = {}
for word in text.split():
    counts[word] = counts.get(word, 0) + 1
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>What are the implications of a high load factor in a hash table?</summary>

### What are the implications of a high load factor in a hash table?

**Definition:**

The load factor is n / m, the number of keys divided by the number of slots. A high value means more collisions and slower operations.

**How it works:**

With chaining, the average chain length is the load factor, so search becomes slower as it grows, and the worst chain can be O(n). With open addressing, probe sequences get long as the table approaches full, and insert fails at load factor 1. Tables rehash into a larger array when the factor crosses a threshold, often around 0.75 for open addressing.

**Examples:**

```python
if len(keys) / len(buckets) > 0.75:
    rehash()
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Separate Chaining](https://www.geeksforgeeks.org/dsa/hashing-set-2-separate-chaining/)
</details>

<details>
<summary>Explain how rehashing works and when it should be performed.</summary>

### Explain how rehashing works and when it should be performed.

**Definition:**

Rehashing allocates a larger table and inserts every existing key again with the new modulus. It should run when the load factor crosses the table's threshold.

**How it works:**

Create a bucket array of about twice the size. For each stored key, compute `hash(key) modulo new_m` and insert it into the new chains. Old indexes are wrong after the size changes, so every key moves. One rehash is O(n). Across a series of doublings, the copy cost is amortized O(1) per insertion. Do it before open addressing is nearly full, not after every insert.

**Examples:**

```python
def rehash(self):
    old = self.buckets
    self.buckets = [[] for _ in range(len(old) * 2)]
    for chain in old:
        for key, value in chain:
            self._insert_new(key, value)
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Discuss the performance differences between hash tables and balanced trees.</summary>

### Discuss the performance differences between hash tables and balanced trees.

**Definition:**

A hash table is average O(1) for lookup, insert, and delete, and worst-case O(n). A balanced BST such as a red-black tree is O(log n) for those operations in every case, and it keeps keys ordered.

**How it works:**

Use the hash table when you only test membership or fetch by key and can accept a rare long chain. Use the balanced tree when you need successor, range queries, or a sorted iteration. The tree also avoids hashing untrusted keys into one bucket. Rehash pauses do not occur in the tree. The tree uses more pointer overhead per node and more comparisons.

**When to use:**

Hash table for unordered key access. Balanced tree for ordered keys and guaranteed O(log n).

**Examples:**

```python
by_id[user_id] = user          # hash, average O(1)
ordered_names.add(user.name)   # tree, O(log n), sorted walk
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
</details>

### Graphs

<details>
<summary>Define a graph and its representation in a computer’s memory.</summary>

### Define a graph and its representation in a computer’s memory.

**Definition:**

A graph is a set of vertices and a set of edges that connect pairs of vertices. Edges may be directed and may carry weights.

**How it works:**

An adjacency matrix is a V by V table. An edge u-v sets `matrix[u][v]`. Space is O(V^2). Testing an edge is O(1). An adjacency list stores, for each vertex, the list of neighbors. Space is O(V + E). Walking neighbors of u is proportional to that degree. GeeksforGeeks recommends the list for sparse graphs and the matrix for dense graphs or fast edge tests.

**When to use:**

Use an adjacency list when E is much smaller than V^2. Use a matrix when the graph is dense or you test arbitrary pairs often.

**Examples:**

```python
adj = [[] for _ in range(4)]
adj[0].append(1)
adj[1].append(2)
```

**Sources:**

- [Graph and its representations](https://www.geeksforgeeks.org/dsa/graph-and-its-representations/)
</details>

<details>
<summary>Explain how you would implement both depth-first search (DFS) and breadth-first search (BFS) on a graph.</summary>

### Explain how you would implement both depth-first search (DFS) and breadth-first search (BFS) on a graph.

**Definition:**

DFS follows one neighbor as far as it can, then backtracks. BFS visits vertices in order of increasing distance from the source. On an adjacency list both are O(V + E).

**How it works:**

BFS puts the source in a queue and a visited set. Each dequeued vertex enqueues unvisited neighbors. GeeksforGeeks counts each vertex once and each edge a constant number of times, which is O(V + E). DFS uses a stack or recursion and the same visited set. On a disconnected graph, start a search from every unvisited vertex. The queue or recursion stack uses O(V) extra space.

**Examples:**

```python
from collections import deque

def bfs(adj, src):
    seen = {src}
    queue = deque([src])
    while queue:
        u = queue.popleft()
        for v in adj[u]:
            if v not in seen:
                seen.add(v)
                queue.append(v)
```

**Sources:**

- [BFS](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [DFS](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>What is the shortest path problem in a graph, and how is it commonly solved?</summary>

### What is the shortest path problem in a graph, and how is it commonly solved?

**Definition:**

The single-source shortest path problem asks for the minimum total weight from one vertex to every other vertex.

**How it works:**

If every edge weight is 1, BFS gives the fewest edges, O(V + E). If weights are non-negative, Dijkstra with a binary heap is O((V + E) log V) and must not be used with negative edges. If weights may be negative and there is no negative cycle, Bellman-Ford relaxes every edge V - 1 times, O(V * E), and a further successful relaxation reports a negative cycle. A DAG can be solved in O(V + E) after a topological order.

**When to use:**

BFS for equal weights. Dijkstra for non-negative weights. Bellman-Ford when a negative edge is possible.

**Examples:**

```python
dist = bfs_unweighted(adj, src)       # weights are 1
dist = dijkstra(adj, src)             # weights >= 0
dist = bellman_ford(V, edges, src)    # negatives allowed
```

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Bellman-Ford](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
- [BFS](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Discuss Dijkstra’s algorithm for finding the shortest path.</summary>

### Discuss Dijkstra’s algorithm for finding the shortest path.

**Definition:**

Dijkstra grows a set of vertices whose shortest distance from the source is final. It always expands the unsettled vertex with the smallest tentative distance. It requires non-negative edge weights.

**How it works:**

Set `dist[src] = 0` and every other distance to infinity. Push `(0, src)` into a min-heap. Pop the smallest pair. If that distance is worse than `dist[u]`, skip it. Otherwise relax each neighbor: if `dist[u] + w < dist[v]`, update `dist[v]` and push it. GeeksforGeeks states the binary-heap complexity as O((V + E) log V) and auxiliary space O(V + E). A negative edge can make a finalized vertex wrong, because a later path could arrive cheaper.

**Examples:**

```python
import heapq
def dijkstra(adj, src):
    dist = [float("inf")] * len(adj)
    dist[src] = 0
    heap = [(0, src)]
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(heap, (dist[v], v))
    return dist
```

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Explain what a directed acyclic graph (DAG) is and give an example of where it might be used.</summary>

### Explain what a directed acyclic graph (DAG) is and give an example of where it might be used.

**Definition:**

A DAG is a directed graph with no directed cycle. Every edge has a direction, and you cannot return to a vertex by following edges.

**How it works:**

A course prerequisite graph is a DAG: an edge A to B means A must come before B, and a cycle would make the requirements impossible. Build systems and spreadsheet formulas use the same shape. A topological order lists every vertex after its prerequisites and exists only for a DAG. Shortest paths on a DAG are O(V + E) by relaxing edges in that order.

**Examples:**

```python
# 0 -> 1 -> 3
# 0 -> 2 -> 3
prereq = {0: [1, 2], 1: [3], 2: [3], 3: []}
```

**Sources:**

- [Directed Acyclic Graph](https://www.geeksforgeeks.org/dsa/introduction-to-directed-acyclic-graph/)
- [Topological Sorting](https://www.geeksforgeeks.org/dsa/topological-sorting/)
</details>

<details>
<summary>How would you detect a cycle in a directed graph?</summary>

### How would you detect a cycle in a directed graph?

**Definition:**

A directed cycle exists when a DFS path reaches a vertex that is still on the current recursion stack. The search is O(V + E).

**How it works:**

Color each vertex white, gray, or black. Gray means it is on the active path. A gray neighbor is a back edge and a cycle. Black means its subtree is finished. Start DFS from every white vertex so a disconnected graph is covered. GeeksforGeeks describes this DFS coloring. An undirected cycle uses a different test: a visited neighbor that is not the parent.

**Examples:**

```python
def dfs(u):
    color[u] = GRAY
    for v in adj[u]:
        if color[v] == GRAY or (color[v] == WHITE and dfs(v)):
            return True
    color[u] = BLACK
    return False
```

**Sources:**

- [Detect Cycle in a Directed Graph](https://www.geeksforgeeks.org/dsa/detect-cycle-in-a-graph/)
</details>

<details>
<summary>Discuss the differences between a graph and a tree.</summary>

### Discuss the differences between a graph and a tree.

**Definition:**

A tree is a connected undirected graph with no cycle. It has exactly V - 1 edges. A general graph may be disconnected, directed, weighted, and cyclic.

**How it works:**

Between any two tree vertices there is one path. Adding one edge creates a cycle. Removing one edge disconnects the tree. A graph can have many paths, or none. DFS and BFS on a tree do not need a visited set to avoid cycles if you do not walk back to the parent, but a graph does. GeeksforGeeks contrasts these structural rules.

**Examples:**

```python
is_tree = connected and len(edges) == len(vertices) - 1 and not directed
```

**Sources:**

- [Difference between Graph and Tree](https://www.geeksforgeeks.org/dsa/difference-between-graph-and-tree/)
</details>

<details>
<summary>What is topological sorting and where is it used?</summary>

### What is topological sorting and where is it used?

**Definition:**

A topological order is a linear order of a DAG in which every edge u to v puts u before v. It exists exactly when the directed graph has no cycle. Kahn's algorithm and DFS both run in O(V + E).

**How it works:**

Kahn's algorithm counts incoming edges. Queue every vertex with indegree 0. Pop u, append it, and decrement each neighbor. A neighbor that reaches 0 enters the queue. If the result has fewer than V vertices, there was a cycle. Build tools, course planners, and spreadsheet recalculation need this order.

**Examples:**

```python
from collections import deque
queue = deque(u for u in range(V) if indegree[u] == 0)
order = []
while queue:
    u = queue.popleft()
    order.append(u)
    for v in adj[u]:
        indegree[v] -= 1
        if indegree[v] == 0:
            queue.append(v)
```

**Sources:**

- [Topological Sorting](https://www.geeksforgeeks.org/dsa/topological-sorting/)
</details>

<details>
<summary>Can you explain Bellman-Ford’s algorithm and its time complexity?</summary>

### Can you explain Bellman-Ford’s algorithm and its time complexity?

**Definition:**

Bellman-Ford computes single-source shortest paths when edges may be negative, and it detects a negative cycle. GeeksforGeeks gives O(V * E) time and O(V) space.

**How it works:**

Set `dist[src] = 0` and the rest to a sentinel infinity. Relax every edge V - 1 times: if `dist[u] + w < dist[v]`, set `dist[v]`. A simple shortest path has at most V - 1 edges, so those passes are enough. One more pass that still relaxes an edge means a negative cycle; the GeeksforGeeks implementation returns -1 in that case. Dijkstra is faster but incorrect with a negative edge.

**Examples:**

```python
def bellman_ford(V, edges, src):
    dist = [10**8] * V
    dist[src] = 0
    for i in range(V):
        for u, v, w in edges:
            if dist[u] != 10**8 and dist[u] + w < dist[v]:
                if i == V - 1:
                    return [-1]
                dist[v] = dist[u] + w
    return dist
```

**Sources:**

- [Bellman-Ford](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
</details>

<details>
<summary>How do you determine if a graph is bipartite?</summary>

### How do you determine if a graph is bipartite?

**Definition:**

A graph is bipartite when its vertices split into two sets with every edge going between the sets. BFS or DFS two-coloring checks this in O(V + E).

**How it works:**

Color the start vertex 0 and each uncolored neighbor with the other color. If a neighbor is already the same color, there is an odd cycle and the graph is not bipartite. Repeat for every unvisited vertex so disconnected components are checked. GeeksforGeeks states BFS two-coloring as O(V + E) time. A tree is always bipartite.

**Examples:**

```python
from collections import deque

def is_bipartite(adj):
    color = [-1] * len(adj)
    for start in range(len(adj)):
        if color[start] != -1:
            continue
        queue = deque([start])
        color[start] = 0
        while queue:
            u = queue.popleft()
            for v in adj[u]:
                if color[v] == -1:
                    color[v] = 1 - color[u]
                    queue.append(v)
                elif color[v] == color[u]:
                    return False
    return True
```

**Sources:**

- [Check whether a graph is bipartite](https://www.geeksforgeeks.org/dsa/bipartite-graph/)
</details>

### Sorting and Searching

<details>
<summary>Describe the quicksort algorithm and its average case time complexity.</summary>

### Describe the quicksort algorithm and its average case time complexity.

**Definition:**

Quicksort picks a pivot, partitions the array so smaller elements are on the left and larger elements are on the right, then sorts the two sides. GeeksforGeeks gives the average case as θ(n log n).

**How it works:**

Lomuto partition, the version in that article, is O(n). The best case, Ω(n log n), is a pivot that splits the array in half. The worst case, O(n²), is a pivot that is always the smallest or largest element, which happens on a sorted array if the pivot is the first or last element. Extra stack space is O(log n) when partitions are balanced and O(n) when they are not. The sort is not stable. A random pivot avoids the sorted-array pattern.

**Examples:**

```python
def quick_sort(arr, low, high):
    if low < high:
        pivot = arr[high]
        i = low - 1
        for j in range(low, high):
            if arr[j] < pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        quick_sort(arr, low, i)
        quick_sort(arr, i + 2, high)
```

**Sources:**

- [QuickSort](https://www.geeksforgeeks.org/dsa/quick-sort/)
</details>

<details>
<summary>Explain the difference between a depth-first search and a breadth-first search in a binary tree.</summary>

### Explain the difference between a depth-first search and a breadth-first search in a binary tree.

**Definition:**

On a binary tree both searches visit each of the n nodes once, so both are O(n). They differ in order and in the auxiliary structure.

**How it works:**

DFS uses recursion or a stack and finishes a subtree before the sibling. Preorder, inorder, and postorder are DFS orders. The stack depth is the height h. BFS uses a queue and emits nodes level by level. The first time BFS reaches a node, that path has the fewest edges. The queue holds up to the widest level. A tree has n - 1 edges, so the graph bound O(V + E) is O(n) here.

**Examples:**

```python
def dfs(node):
    if node is None:
        return
    dfs(node.left)
    dfs(node.right)
```

**Sources:**

- [BFS vs DFS](https://www.geeksforgeeks.org/dsa/difference-between-bfs-and-dfs/)
- [Tree Traversals](https://www.geeksforgeeks.org/dsa/tree-traversals-inorder-preorder-and-postorder/)
</details>

<details>
<summary>How would you use a binary search on a rotated sorted array?</summary>

### How would you use a binary search on a rotated sorted array?

**Definition:**

A rotated sorted array is a sorted array whose ends were moved. One binary search still finds a key in O(log n) time and O(1) extra space.

**How it works:**

GeeksforGeeks compares `arr[low]` with `arr[mid]` to see which half is sorted. If the left half is sorted and the key lies in it, search left; otherwise search right. The symmetric test handles a sorted right half. Each step drops half of the remaining range. Finding the pivot first and then searching one sorted side is also O(log n).

**Examples:**

```python
def search(arr, key):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == key:
            return mid
        if arr[lo] <= arr[mid]:
            if arr[lo] <= key < arr[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        elif arr[mid] < key <= arr[hi]:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

**Sources:**

- [Search in a Rotated Sorted Array](https://www.geeksforgeeks.org/dsa/search-an-element-in-a-sorted-and-pivoted-array/)
</details>

<details>
<summary>What is the difference between merge sort and heap sort?</summary>

### What is the difference between merge sort and heap sort?

**Definition:**

Both are O(n log n) in the best, average, and worst cases. Merge sort is stable and needs O(n) extra space. Heap sort is in-place aside from O(1) extra memory and is not stable.

**How it works:**

Merge sort splits the array in half, sorts the halves, and merges them. The merge copies into an auxiliary array, which is why the space is O(n), and equal keys can keep their original order. Heap sort builds a max-heap in O(n), then repeatedly swaps the root with the last unsorted item and sifts down, O(n log n). Swaps do not preserve the order of equal keys. Quicksort is often faster in practice but its worst case is O(n²).

**When to use:**

Use merge sort when equal keys must stay in order or the input is a linked list. Use heap sort when extra memory is tight and stability does not matter.

**Examples:**

```python
# merge: stable, O(n) extra buffer
# heap: not stable, swaps inside the same array
```

**Sources:**

- [Merge Sort](https://www.geeksforgeeks.org/dsa/merge-sort/)
- [Heap Sort](https://www.geeksforgeeks.org/dsa/heap-sort/)
</details>

<details>
<summary>Can you describe bucket sort and give an example of its use case?</summary>

### Can you describe bucket sort and give an example of its use case?

**Definition:**

Bucket sort distributes values into ordered buckets, sorts each bucket, and concatenates them. When values are uniform over a range, the average time is O(n + k) for n items and k buckets.

**How it works:**

Map each value into a bucket index. Sort that small list with insertion sort. The worst case is O(n²) if every value falls in one bucket. A typical use is sorting numbers that are evenly spread over an interval, such as normalized scores in [0, 1). Counting sort is the related choice when the keys are small integers.

**When to use:**

Use it for uniformly distributed numbers in a known range. Do not use it for adversarial data that can fill one bucket.

**Examples:**

```python
def bucket_sort(values, k=10):
    buckets = [[] for _ in range(k)]
    for value in values:
        buckets[min(k - 1, int(value * k))].append(value)
    out = []
    for bucket in buckets:
        out.extend(sorted(bucket))
    return out
```

**Sources:**

- [Bucket Sort](https://www.geeksforgeeks.org/dsa/bucket-sort-2/)
</details>

<details>
<summary>Discuss the advantages of quicksort over other sorting algorithms.</summary>

### Discuss the advantages of quicksort over other sorting algorithms.

**Definition:**

GeeksforGeeks lists quicksort as cache-friendly and fast on large in-memory arrays when stability is not required. The average case is θ(n log n), and the sort works inside the original array.

**How it works:**

It does not copy the data into an auxiliary array the way merge sort does, so it uses cache lines well. A random or median pivot makes the O(n²) pattern unlikely. It is a poor choice for small arrays, it is not stable, and a fixed end pivot is O(n²) on sorted input. Heap sort has a stricter O(n log n) worst case. Merge sort is the stable alternative.

**When to use:**

Use quicksort for a large array of primitives when equal elements may be reordered. Use merge sort when the sort must be stable.

**Examples:**

```python
import random
pivot_index = random.randrange(low, high + 1)
```

**Sources:**

- [QuickSort](https://www.geeksforgeeks.org/dsa/quick-sort/)
</details>

<details>
<summary>How does the insertion sort work and what is its time complexity?</summary>

### How does the insertion sort work and what is its time complexity?

**Definition:**

Insertion sort grows a sorted prefix. Each next element is inserted into its place in that prefix. The best case is O(n), and the average and worst cases are O(n²).

**How it works:**

The best case is an already sorted array: each element is compared once and not moved. The worst case is reverse order: the i-th element shifts i items. Extra memory is O(1). The sort is stable. It is the usual sort inside a small bucket, and it is cheap for nearly sorted data. It is not the choice for a large random array.

**Examples:**

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

**Sources:**

- [Insertion Sort](https://www.geeksforgeeks.org/dsa/insertion-sort/)
</details>

<details>
<summary>What is a stable sort and why might it be important in certain scenarios?</summary>

### What is a stable sort and why might it be important in certain scenarios?

**Definition:**

A stable sort keeps the original relative order of elements that compare equal. Merge sort and insertion sort are stable. Quicksort and heap sort are not.

**How it works:**

If records are sorted by last name and then by first name, a stable second pass keeps the first-name order inside each last name. An unstable sort can swap two equal keys and destroy the earlier key. GeeksforGeeks notes that quicksort swaps by pivot position and therefore does not preserve that order. Stability matters for multi-key records. It does not matter for a single key with no payload order.

**Examples:**

```python
# stable: ("Ann", 1) stays before ("Ann", 2) when the name is the key
records.sort(key=lambda row: row[0])  # Python's sort is stable
```

**Sources:**

- [Stability in Sorting](https://www.geeksforgeeks.org/dsa/stability-in-sorting-algorithms/)
- [QuickSort](https://www.geeksforgeeks.org/dsa/quick-sort/)
</details>

<details>
<summary>Can you explain the counting sort algorithm?</summary>

### Can you explain the counting sort algorithm?

**Definition:**

Counting sort counts how many times each integer in a small range appears, then writes the values back in order. For n items and range k it is O(n + k).

**How it works:**

Allocate a count array of length k. Increment `count[value]` for each item. A prefix sum of the counts gives the last output index of each value. Walk the input from the right and place each value at that index, decrementing it, which keeps the sort stable. It is not a comparison sort. It is a poor fit when k is much larger than n.

**Examples:**

```python
def counting_sort(arr, k):
    count = [0] * k
    for value in arr:
        count[value] += 1
    out = []
    for value, times in enumerate(count):
        out.extend([value] * times)
    return out
```

**Sources:**

- [Counting Sort](https://www.geeksforgeeks.org/dsa/counting-sort/)
</details>

<details>
<summary>How would you sort a linked list?</summary>

### How would you sort a linked list?

**Definition:**

Merge sort fits a linked list because it does not need random access. Splitting and merging nodes is O(n log n) time, and the merge rewires pointers instead of allocating an n-element array.

**How it works:**

Find the middle with the slow and fast pointers, split the list, sort both halves, and merge them by always linking the smaller head. GeeksforGeeks presents merge sort for linked lists for this reason. Quicksort's partition is awkward without indexes. Insertion sort is O(n²) but fine for a short list. Bottom-up merge sort avoids the O(log n) recursion stack.

**Examples:**

```python
def merge(a, b):
    dummy = tail = Node(0)
    while a and b:
        if a.value <= b.value:
            tail.next, a = a, a.next
        else:
            tail.next, b = b, b.next
        tail = tail.next
    tail.next = a or b
    return dummy.next
```

**Sources:**

- [Merge Sort for Linked List](https://www.geeksforgeeks.org/dsa/merge-sort-for-linked-list/)
</details>

### Advanced Data Structures

<details>
<summary>Describe a red-black tree and its properties.</summary>

### Describe a red-black tree and its properties.

**Definition:**

A red-black tree is a binary search tree whose nodes are colored so the height stays O(log n). Search, insert, and delete are O(log n).

**How it works:**

Every node is red or black. The root is black. Every leaf sentinel is black. A red node has black children. Every path from a node to a descendant leaf has the same number of black nodes. Insert and delete restore these rules with recoloring and rotations. The tree is less strictly balanced than an AVL tree, so lookups can take a few more comparisons, and updates rotate less often.

**Examples:**

```python
# after insert, fix red-red parent/child by recolor or rotate
# black-height of every root-to-leaf path stays equal
```

**Sources:**

- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
</details>

<details>
<summary>Explain what a B-tree is and where it might be used.</summary>

### Explain what a B-tree is and where it might be used.

**Definition:**

A B-tree is a balanced search tree with many keys and children per node. All leaves sit on the same level. It is the usual index structure on disk.

**How it works:**

A node holds several sorted keys and one more child pointer than keys. Search follows one child, so the height is small when the branching factor is large. One node is sized to a disk page, so a lookup costs a few page reads, each O(1) in the tree height, overall O(log n). Databases and file systems use B-trees and B+ trees for this. A binary tree would need far more random reads for the same n.

**When to use:**

Use a B-tree when the data lives on disk or SSD pages. Use a red-black tree for an in-memory ordered map.

**Examples:**

```python
# one disk page: keys [10, 20, 30] and four child page ids
```

**Sources:**

- [B-Tree](https://www.geeksforgeeks.org/dsa/introduction-of-b-tree-2/)
</details>

<details>
<summary>Discuss the concept of a segment tree and one of its practical applications.</summary>

### Discuss the concept of a segment tree and one of its practical applications.

**Definition:**

A segment tree answers range queries and point updates on an array in O(log n). Building it is O(n).

**How it works:**

The root covers the whole array. Each internal node covers a half, and the leaves cover single indexes. A range-sum node stores the sum of its segment. A query splits the requested range into O(log n) disjoint node ranges and adds them. An update changes a leaf and recomputes ancestors up to the root, also O(log n). A practical use is the sum, minimum, or maximum of any subarray that changes over time.

**Examples:**

```python
def query(node, start, end, left, right):
    if right < start or end < left:
        return 0
    if left <= start and end <= right:
        return tree[node]
    mid = (start + end) // 2
    return query(2 * node, start, mid, left, right) + query(2 * node + 1, mid + 1, end, left, right)
```

**Sources:**

- [Segment Tree](https://www.geeksforgeeks.org/dsa/segment-tree-set-1-sum-of-given-range/)
</details>

<details>
<summary>What is a suffix tree and how is it used in substring search?</summary>

### What is a suffix tree and how is it used in substring search?

**Definition:**

A suffix tree is a compressed trie of every suffix of a text. After it is built, a pattern of length m is searched in O(m) time, independent of the text length.

**How it works:**

GeeksforGeeks describes building it by taking every suffix and inserting those suffixes into a compressed trie. A walk that consumes the pattern means the pattern occurs. The same tree answers longest repeated substring and longest common substring. It is a poor fit when the text changes often, such as a text editor, because the tree would be rebuilt. KMP searches one pattern in O(n + m) without the preprocess of all suffixes.

**Examples:**

```python
# text "banana$" suffixes share compressed edges
# search "nana" by walking those edges, O(len(pattern))
```

**Sources:**

- [Suffix tree overview](https://www.geeksforgeeks.org/dsa/overview-of-graph-trie-segment-tree-and-suffix-tree-data-structures/)
</details>

<details>
<summary>Can you explain what a trie is and when you might use one?</summary>

### Can you explain what a trie is and when you might use one?

**Definition:**

A trie is a tree of characters. Each edge is one character, and a word is a path from the root. Insert and search of a word of length m are O(m).

**How it works:**

Nodes do not store the whole key. The end of a word is marked on the node where the path stops. Shared prefixes share nodes, so a dictionary of words with common beginnings uses less space than a hash of full strings and can list every word with a given prefix. Autocomplete and spell-check prefixes are the usual cases. A hash table is better when you only test exact keys and prefixes do not matter.

**When to use:**

Use a trie for prefix queries. Use a hash table for exact-key lookup only.

**Examples:**

```python
def insert(root, word):
    node = root
    for ch in word:
        node = node.children.setdefault(ch, {})
    node["$"] = True
```

**Sources:**

- [Trie](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)
</details>

<details>
<summary>Discuss the use of a bloom filter and its advantages over a traditional hash table.</summary>

### Discuss the use of a bloom filter and its advantages over a traditional hash table.

**Definition:**

A Bloom filter is a bit array plus several hash functions. It answers whether a key is definitely absent or possibly present. It can have false positives and no false negatives.

**How it works:**

Insert sets k bits, one per hash. Lookup checks those k bits. If any bit is 0, the key was never inserted. If all are 1, the key may be present or the bits may belong to other keys. The structure is much smaller than storing the keys. It cannot return the key, and a standard Bloom filter cannot delete. Use it as a cheap pre-check in front of a real store.

**When to use:**

Use a Bloom filter when a compact maybe-present test is enough. Use a hash table when you must store the key and never report a false hit.

**Examples:**

```python
def add(bits, key, hashes):
    for fn in hashes:
        bits[fn(key) % len(bits)] = 1
```

**Sources:**

- [Bloom Filters](https://www.geeksforgeeks.org/dsa/bloom-filters-introduction-and-python-implementation/)
</details>

<details>
<summary>Explain the concept of a disjoint-set and its use in network connectivity.</summary>

### Explain the concept of a disjoint-set and its use in network connectivity.

**Definition:**

A disjoint-set (union-find) keeps elements in groups. `find` returns a group's representative. `union` merges two groups. With union by rank and path compression, both are amortized nearly O(1), the inverse Ackermann of n.

**How it works:**

Each element starts in its own set. `find` follows parent pointers to the root and then points those nodes directly at the root. `union` links the root of smaller rank under the other root. In a network, two hosts are connected when `find(a) == find(b)`. Kruskal's minimum spanning tree uses the same test to skip an edge that would close a cycle.

**Examples:**

```python
def find(parent, x):
    if parent[x] != x:
        parent[x] = find(parent, parent[x])
    return parent[x]
```

**Sources:**

- [Disjoint Set](https://www.geeksforgeeks.org/dsa/disjoint-set-data-structures/)
</details>

<details>
<summary>What is a Fenwick tree (or binary indexed tree) and when might it be used?</summary>

### What is a Fenwick tree (or binary indexed tree) and when might it be used?

**Definition:**

A Fenwick tree stores prefix aggregates of an array so a point update and a prefix query are both O(log n).

**How it works:**

Index i is responsible for a range ending at i whose length is the lowest set bit of i. Update adds a delta to i and then to i + lowbit(i), walking O(log n) indexes. A prefix sum subtracts that bit repeatedly. It uses less memory and simpler code than a segment tree, and it does not by itself answer an arbitrary range minimum. Use it for frequencies and prefix sums that change. A segment tree is the step up when the query is not a prefix or invertible aggregate.

**When to use:**

Use a Fenwick tree for point updates and prefix sums. Use a segment tree for arbitrary range combines such as range minimum.

**Examples:**

```python
def add(bit, i, delta):
    while i < len(bit):
        bit[i] += delta
        i += i & -i
```

**Sources:**

- [Fenwick Tree](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
</details>

<details>
<summary>Discuss the differences between AVL trees and red-black trees.</summary>

### Discuss the differences between AVL trees and red-black trees.

**Definition:**

Both are balanced binary search trees with O(log n) search, insert, and delete. An AVL tree keeps a stricter height balance. A red-black tree rotates less on update.

**How it works:**

An AVL node stores a balance factor of -1, 0, or 1, the height difference of its children. After an insert it may rotate to restore that. A red-black tree only enforces equal black-height and no two reds in a row, so it can be a little taller. Lookups are slightly faster on an AVL tree. Inserts and deletes that rebalance are cheaper on a red-black tree. Language ordered maps often use red-black trees for that update cost.

**When to use:**

Prefer AVL when lookups dominate. Prefer red-black when inserts and deletes are frequent.

**Examples:**

```python
balance = height(node.left) - height(node.right)  # AVL keeps this in {-1, 0, 1}
```

**Sources:**

- [AVL Tree](https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/)
- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
</details>

<details>
<summary>How would you implement a rope data structure and what is its use case?</summary>

### How would you implement a rope data structure and what is its use case?

**Definition:**

A rope is a binary tree whose leaves hold chunks of a string and whose internal nodes store how many characters sit to their left. It is used for large texts that are concatenated and edited.

**How it works:**

GeeksforGeeks shows concatenation as a new root whose children are the two ropes, which is O(1) before any rebalance. The left-count lets you find the i-th character by walking down, O(height). Splitting cuts one path. Unlike one contiguous array, a small edit does not copy the whole string. A gap buffer is the simpler editor structure: local insert at the cursor is amortized O(1), and GeeksforGeeks notes editors often prefer it because it is easier.

**When to use:**

Use a rope when many large strings are concatenated. Use a gap buffer when one editor cursor does local inserts and deletes.

**Examples:**

```python
def concat(left, right):
    return Rope(left_count=left.length, left=left, right=right, text=None)
```

**Sources:**

- [Ropes](https://www.geeksforgeeks.org/dsa/ropes-data-structure-fast-string-concatenation/)
- [Gap Buffer](https://www.geeksforgeeks.org/dsa/gap-buffer-data-structure/)
</details>

### Design Questions

<details>
<summary>How would you design a system that requires efficient frequent updates and retrieval of data?</summary>

### How would you design a system that requires efficient frequent updates and retrieval of data?

**Definition:**

Split the access pattern. Point reads and writes of arbitrary keys use a hash table. Ordered range reads that also update use a balanced tree or a segment tree.

**How it works:**

A hash table gives average O(1) get and put and does not keep order. A red-black tree gives O(log n) get, put, and successor. A segment tree or Fenwick tree gives O(log n) range sums with point updates. If the working set is larger than memory, a B-tree keeps the height small in pages. Cache the hottest keys in an LRU map in front of the durable structure.

**When to use:**

Hash table for key lookup. Balanced tree or segment tree when the query is a range. B-tree when the data is on disk.

**Examples:**

```python
by_id[key] = value          # point read/write
index_by_time.add(stamp)    # ordered scan
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
- [Segment Tree](https://www.geeksforgeeks.org/dsa/segment-tree-set-1-sum-of-given-range/)
</details>

<details>
<summary>Discuss how you might implement autocomplete functionality.</summary>

### Discuss how you might implement autocomplete functionality.

**Definition:**

A trie stores the dictionary so every word with a typed prefix shares one path. Suggesting completions is a walk of that subtree.

**How it works:**

Insert each word in O(m). On each keystroke, walk the prefix in O(m). If the node exists, DFS or BFS its subtree and collect nodes marked as word ends, usually stopping after k suggestions. A hash set only answers exact words. A sorted array of words can also lower-bound the prefix in O(log n) and scan neighbors, which is attractive when the dictionary is static.

**When to use:**

Use a trie when the dictionary changes and prefixes are the query. Use a sorted array when the word list is static.

**Examples:**

```python
node = walk(root, prefix)
if node:
    suggestions = collect_words(node, limit=5)
```

**Sources:**

- [Trie](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)
</details>

<details>
<summary>Can you design a data structure that supports insert, delete, get random element operations in constant time?</summary>

### Can you design a data structure that supports insert, delete, get random element operations in constant time?

**Definition:**

Combine a dynamic array with a hash map from value to index. Insert, delete, and random access are then average O(1).

**How it works:**

The array stores the values. The map stores each value's index. Insert appends and records the index. Delete swaps the victim with the last element, updates the moved element's index, and pops the last slot, so there is no O(n) shift. Random reads `array[randrange(len(array))]`. Duplicates need a map of value to a set of indexes.

**Examples:**

```python
def remove(self, value):
    i = self.index.pop(value)
    last = self.items[-1]
    self.items[i] = last
    self.index[last] = i
    self.items.pop()
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Linked List vs Array](https://www.geeksforgeeks.org/dsa/linked-list-vs-array/)
</details>

<details>
<summary>Explain how to design a parking lot using object-oriented principles.</summary>

### Explain how to design a parking lot using object-oriented principles.

**Definition:**

Model the lot as objects with one job each: spots, vehicles, a ticket, and a board that finds a free spot. The assignment of a vehicle to a spot is the state that must stay consistent.

**How it works:**

A `Spot` knows its size and whether it is free. A `Vehicle` knows the size it needs. `ParkingLot` holds spots grouped by size, for example a hash map from size to a set of free spot ids, so taking a spot is average O(1). A ticket stores spot id, vehicle id, and entry time. Do not scan every spot on each arrival when the free set can answer.

**Examples:**

```python
spot_id = free_by_size[vehicle.size].pop()
tickets[vehicle.id] = Ticket(spot_id, now)
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Describe a data structure that could efficiently store a sparse matrix.</summary>

### Describe a data structure that could efficiently store a sparse matrix.

**Definition:**

A sparse matrix is mostly zeros. Store only the non-zero entries in a hash map keyed by (row, column), or in compressed row lists.

**How it works:**

The map uses space proportional to the number of non-zeros, not rows times columns. Reading a cell is average O(1). Adding two sparse matrices walks the union of their keys. A coordinate list of triples is simple but slower to probe. Compressed sparse row stores column indexes and values per row and is the usual format when you multiply by a vector.

**When to use:**

Use a hash map for random cell edits. Use compressed rows when the matrix is static and you scan whole rows.

**Examples:**

```python
cells = {(0, 2): 5, (3, 1): 9}
value = cells.get((i, j), 0)
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>How would you implement a file system directory structure in terms of data structures?</summary>

### How would you implement a file system directory structure in terms of data structures?

**Definition:**

A directory is a tree of nodes. Each node is a file or a directory, and a directory's children are a hash map from name to node.

**How it works:**

Path lookup splits `/a/b` and walks from the root, average O(1) per component. A parent pointer supports `..`. The tree shape matches a graph that is a tree: one parent, no cycles, except for the special `.` and `..` links. If the file data is large, the inode stores a rope or a list of block ids rather than one contiguous array.

**Examples:**

```python
class Dir:
    def __init__(self):
        self.children = {}
root.children["docs"] = Dir()
```

**Sources:**

- [Difference between Graph and Tree](https://www.geeksforgeeks.org/dsa/difference-between-graph-and-tree/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Discuss the implementation of a phone book which needs to be efficient for look-up, insert, delete, and modification operations.</summary>

### Discuss the implementation of a phone book which needs to be efficient for look-up, insert, delete, and modification operations.

**Definition:**

Look up by exact name with a hash map. If the book must also list names in order or by prefix, keep a trie or a balanced tree beside it.

**How it works:**

Hash get, put, and delete are average O(1) and cover modification as a put of a new number. They do not produce a sorted phone book. A trie answers prefix lookup in O(m) and can list completions. A red-black tree answers exact lookup in O(log n) and an in-order listing. Pick the hash map when the only query is the full name.

**When to use:**

Hash map for exact names. Trie when the user types a prefix. Balanced tree when a sorted dump is required.

**Examples:**

```python
book["Ada Lovelace"] = "555-0100"
book["Ada Lovelace"] = "555-0199"  # modify
del book["Ada Lovelace"]
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Trie](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)
</details>

<details>
<summary>Explain the necessary data structures for a text editor which supports insertion, deletion, and character retrieval operations efficiently.</summary>

### Explain the necessary data structures for a text editor which supports insertion, deletion, and character retrieval operations efficiently.

**Definition:**

A gap buffer keeps a hole at the cursor inside one array. Local insert and delete are amortized O(1). A rope is the alternative when edits are scattered across a huge document.

**How it works:**

GeeksforGeeks describes moving the gap to the cursor and writing into it. Growing the buffer when the gap is empty is O(n), amortized across many local edits. Reading the i-th character skips the gap. A single contiguous string makes each middle insert O(n). A rope concatenates chunks in O(1) before rebalance and finds a character from left-counts, which fits very large documents better than one array.

**When to use:**

Gap buffer for one cursor and local edits. Rope for huge texts and frequent concatenation.

**Examples:**

```python
buffer[gap_left] = ch
gap_left += 1
```

**Sources:**

- [Gap Buffer](https://www.geeksforgeeks.org/dsa/gap-buffer-data-structure/)
- [Ropes](https://www.geeksforgeeks.org/dsa/ropes-data-structure-fast-string-concatenation/)
</details>

<details>
<summary>How would you store a large dataset that allows for efficient range queries and updates?</summary>

### How would you store a large dataset that allows for efficient range queries and updates?

**Definition:**

If the data fits in memory and queries are ranges with point updates, use a segment tree or a Fenwick tree. If it does not fit, use a B-tree.

**How it works:**

A Fenwick tree does prefix sums and point updates in O(log n) with a small constant. A segment tree does arbitrary ranges, including minimum, in O(log n), and build is O(n). A B-tree keeps ordered keys on disk with a high branching factor so a range scan reads few pages. A hash table cannot answer a range without scanning every key.

**When to use:**

Fenwick for prefix sums. Segment tree for general ranges. B-tree when the dataset is larger than memory.

**Examples:**

```python
total = prefix(right) - prefix(left - 1)  # Fenwick, O(log n)
```

**Sources:**

- [Segment Tree](https://www.geeksforgeeks.org/dsa/segment-tree-set-1-sum-of-given-range/)
- [Fenwick Tree](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
- [B-Tree](https://www.geeksforgeeks.org/dsa/introduction-of-b-tree-2/)
</details>

<details>
<summary>Suggest the appropriate data structure for developing a shared document editor that multiple users can edit simultaneously.</summary>

### Suggest the appropriate data structure for developing a shared document editor that multiple users can edit simultaneously.

**Definition:**

Store the document as a rope or a gap buffer for local edits, and store concurrent edits as an ordered log of operations that each client merges.

**How it works:**

The visible text still needs efficient insert and delete at a cursor, which a gap buffer or rope provides. Simultaneous users cannot share one mutable array index, because two inserts at the same offset diverge. Give each character or operation a stable id and keep a total order, for example a balanced tree of ids, so a remote insert can land without rewriting every later index. A hash map from id to node finds a character in average O(1) when applying a remote delete.

**Examples:**

```python
ops.append(("insert", after_id, "x"))
text.insert_after(after_id, "x")
```

**Sources:**

- [Ropes](https://www.geeksforgeeks.org/dsa/ropes-data-structure-fast-string-concatenation/)
- [Gap Buffer](https://www.geeksforgeeks.org/dsa/gap-buffer-data-structure/)
</details>

### Real-world Applications and Miscellaneous

<details>
<summary>What data structure would you use for a web browser’s back and forward button implementation?</summary>

### What data structure would you use for a web browser’s back and forward button implementation?

**Definition:**

Two stacks. One holds the pages behind the current page. The other holds the pages ahead. The current page sits between them.

**How it works:**

Back pops the back stack and pushes the current page onto the forward stack. Forward does the reverse. A new navigation clears the forward stack, because that future is no longer valid, then pushes the current page onto the back stack. Each button is O(1). A single list with an index also works, but the two stacks match the LIFO history directly.

**Examples:**

```python
def go_back(self):
    self.forward.append(self.current)
    self.current = self.back.pop()
```

**Sources:**

- [Stack Data Structure](https://www.geeksforgeeks.org/dsa/stack-data-structure/)
</details>

<details>
<summary>Can you create a data structure that dynamically adjusts to user load such that it expands and contracts based on activity?</summary>

### Can you create a data structure that dynamically adjusts to user load such that it expands and contracts based on activity?

**Definition:**

A dynamic array does this for a stack or a buffer. Double the capacity when it is full and shrink it when it is sparse, so a sequence of operations stays amortized O(1).

**How it works:**

Append when `size == capacity` allocates a new block, often twice as large, and copies n items, O(n) for that call. Halve the capacity when the size falls below one quarter, not one half, so grow and shrink do not alternate on every operation. A hash table uses the same idea: rehash when the load factor crosses a threshold. A linked list grows one node at a time and never copies, at the cost of O(n) access.

**Examples:**

```python
if self.size == self.capacity:
    self._resize(self.capacity * 2)
elif self.capacity > 8 and self.size * 4 < self.capacity:
    self._resize(self.capacity // 2)
```

**Sources:**

- [Linked List vs Array](https://www.geeksforgeeks.org/dsa/linked-list-vs-array/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Discuss how a data structure can influence database indexing.</summary>

### Discuss how a data structure can influence database indexing.

**Definition:**

An index is a search structure beside the table. A B-tree index answers ordered lookups and ranges in a few disk pages. A hash index answers exact keys and cannot walk a range.

**How it works:**

A B-tree node is about one page and has a large branching factor, so height stays small and a range scan is sequential inside leaves of a B+ tree. A hash index computes a bucket and reads that page, average O(1) for equality, with chains or overflow pages when the bucket is hot. A binary search tree in memory would cause one random read per level. The choice follows the query: equality versus range.

**When to use:**

B-tree for ranges and order. Hash index for equality on a key that is never scanned in order.

**Examples:**

```sql
-- equality: hash or B-tree
-- range: B-tree, not a hash index
```

**Sources:**

- [B-Tree](https://www.geeksforgeeks.org/dsa/introduction-of-b-tree-2/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Explain the choice of data structure for implementing a search engine index.</summary>

### Explain the choice of data structure for implementing a search engine index.

**Definition:**

An inverted index maps a term to the list of documents that contain it. The term dictionary is a hash table or a trie. The posting list is a sorted array of document ids.

**How it works:**

A query term is an average O(1) hash lookup or an O(m) trie walk. Intersecting two posting lists that are sorted is a linear merge, the same idea as merging sorted arrays. A Bloom filter in front of the dictionary can reject terms that are definitely absent. A suffix tree is for substring search inside one text, not for a corpus of documents keyed by words.

**Examples:**

```python
hits = posting[term]  # sorted document ids
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Trie](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)
- [Bloom Filters](https://www.geeksforgeeks.org/dsa/bloom-filters-introduction-and-python-implementation/)
</details>

<details>
<summary>How would you store and retrieve high-frequency data with minimal latency?</summary>

### How would you store and retrieve high-frequency data with minimal latency?

**Definition:**

Keep the hot records in a hash table in memory. Bound that table with an LRU list so the least recently used record leaves when the budget is full. Both get and put stay O(1) on average.

**How it works:**

A cache hit never goes to disk. A miss reads the durable B-tree or table and inserts the record at the front of the LRU list. If the cache is full, drop the tail. Do not use a structure with O(n) lookup on the hot path. A Bloom filter can skip a disk read when the key is definitely absent.

**Examples:**

```python
value = cache.get(key)
if value is None:
    value = store.read(key)
    cache.put(key, value)
```

**Sources:**

- [LRU Cache](https://www.geeksforgeeks.org/dsa/lru-cache-implementation/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>Discuss the data structures you would use to implement a social network feed.</summary>

### Discuss the data structures you would use to implement a social network feed.

**Definition:**

Store each user's posts in a structure that can prepend a new post quickly, and store the friend graph as adjacency lists.

**How it works:**

A linked list or a deque prepends a post in O(1). A dynamic array prepends in O(n) unless new posts are appended and the feed is read backward. The friend graph is sparse, so adjacency lists use O(V + E) rather than a matrix. Building a home feed walks the user's neighbors and merges their time-ordered posts, the same merge as two sorted lists. A hash map from user id to that user's post head makes the start of each list average O(1).

**Examples:**

```python
posts[user].appendleft(post)  # O(1) newest-first
for friend in adj[user]:
    merge(feed, posts[friend])
```

**Sources:**

- [Queue using Linked List](https://www.geeksforgeeks.org/dsa/queue-linked-list-implementation/)
- [Graph and its representations](https://www.geeksforgeeks.org/dsa/graph-and-its-representations/)
</details>

<details>
<summary>How can a leaderboard system in a gaming platform be implemented with regard to data structures?</summary>

### How can a leaderboard system in a gaming platform be implemented with regard to data structures?

**Definition:**

A balanced tree or a binary heap keeps players ordered by score. A hash map finds a player's current node so an update does not scan the board.

**How it works:**

A red-black tree ordered by score supports insert, delete, and the next rank in O(log n), and an in-order walk prints the board. Updating a score deletes the old key and inserts the new one. A hash map from player id to score makes that delete average O(1) to find plus O(log n) in the tree. A binary heap finds the top player in O(1) but cannot list ranks around an arbitrary player as cheaply. A Fenwick or segment tree of frequencies answers how many players have a score below x in O(log S).

**When to use:**

Use an ordered tree for a full ranked list. Use a heap when you only need the top player. Use a Fenwick tree of score frequencies for rank-by-count.

**Examples:**

```python
order.remove((old_score, player))
order.add((new_score, player))
scores[player] = new_score
```

**Sources:**

- [Red-Black Tree](https://www.geeksforgeeks.org/dsa/red-black-tree-set-1-introduction-2/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Fenwick Tree](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
</details>

<details>
<summary>Discuss how GPS systems use data structures to find the shortest path effectively.</summary>

### Discuss how GPS systems use data structures to find the shortest path effectively.

**Definition:**

The road network is a weighted graph: intersections are vertices and road segments are edges with a non-negative travel cost. Dijkstra, or A* with the same relaxation rule, computes the path.

**How it works:**

An adjacency list matches a sparse road graph, space O(V + E). Dijkstra with a binary heap is O((V + E) log V) and is valid because travel times are not negative. GeeksforGeeks shows the min-heap of (distance, vertex) and the skip of stale heap entries. A contraction or a hierarchy can shrink the graph that Dijkstra sees, but the core step is still non-negative shortest paths. Bellman-Ford is unnecessary without negative edges and is O(V * E).

**Examples:**

```python
dist, parent = dijkstra(roads, start)
path = restore(parent, goal)
```

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Graph and its representations](https://www.geeksforgeeks.org/dsa/graph-and-its-representations/)
</details>

<details>
<summary>What data structures are involved in the implementation of ride-sharing applications like Uber or Lyft?</summary>

### What data structures are involved in the implementation of ride-sharing applications like Uber or Lyft?

**Definition:**

The street map is a weighted graph. Nearby drivers are a spatial index plus a hash map. The match is a shortest-path query on that graph.

**How it works:**

Adjacency lists store the road graph. Dijkstra finds a route because edge weights are non-negative travel times. A hash map from driver id to location and status is average O(1) to update. A grid hash, or a tree over map cells, returns drivers near a rider without scanning every driver. An open trip can sit in a queue. Driver positions change often, so the spatial bucket is updated on each move rather than rebuilt.

**Examples:**

```python
cell = (lat // size, lng // size)
drivers_in[cell].add(driver_id)
route = dijkstra(roads, rider_cell, driver_cell)
```

**Sources:**

- [Graph and its representations](https://www.geeksforgeeks.org/dsa/graph-and-its-representations/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>How would a recommendation system benefit from specific data structures to personalize content for users?</summary>

### How would a recommendation system benefit from specific data structures to personalize content for users?

**Definition:**

A hash map stores a user's history and item features for average O(1) lookup. A graph stores who interacted with what. An inverted index stores which users touched an item.

**How it works:**

User to items is a hash map of sets, so recording a view is average O(1). Item to users is the same shape as a search posting list and supports 'people who viewed this'. Walking that bipartite graph a few steps finds neighbors. A trie still helps when the recommendation starts from a typed prefix. A heap of size k keeps the top suggestions without sorting every candidate. None of these structures replace the ranking function; they make the candidate lookup cheap.

**Examples:**

```python
seen[user].add(item)
for other in viewers[item]:
    if other != user:
        candidates[other] += 1
top = heapq.nlargest(k, candidates, key=candidates.get)
```

**Sources:**

- [Hashing](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Graph and its representations](https://www.geeksforgeeks.org/dsa/graph-and-its-representations/)
- [Priority Queue](https://www.geeksforgeeks.org/dsa/priority-queue-set-1-introduction/)
</details>

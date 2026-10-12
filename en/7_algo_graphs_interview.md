# Graph Theory — Interview

Answers are filled for every question in this note.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Graph Theory — Interview](./7_algo_graphs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Graph Theory](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/).

### Introduction to Graphs

<details>
<summary>What is a Graph?</summary>

### What is a Graph?

**Definition:**

A graph is a data structure that represents a collection of interconnected nodes through a set of edges.

This abstract structure is highly versatile and finds applications in various domains, from social network analysis to computer networking.

Core Components

A graph consists of two main components:

- **Nodes**: Also called vertices, these are the fundamental units that hold data.
- **Edges**: These are the connections between nodes, and they can be either directed or undirected.

Visual Representation

- **Graph**: Unidirected, Directed, Cyclic, Acyclic, Weighted, Unweighted, Sparse, Dense

**How it works:**

Graph Representations

There are several ways to represent graphs in computer memory, with the most common ones being adjacency matrix, adjacency list, and edge list.

Adjacency Matrix

In an adjacency matrix, a 2D Boolean array indicates the edges between nodes. A value of True at index [i][j] means that an edge exists between nodes i and j.

Adjacency List

An adjacency list represents each node as a list, and the indices of the list are the nodes. Each node's list contains the nodes that it is directly connected to.

Edge List

An edge list is a simple list of tuples, where each tuple represents an edge between two nodes.

**Examples:**

Here is the Python code:

```python
graph = [
    [False, True, True],
    [True, False, True],
    [True, True, False]
]
```

Here is the Python code:

```python
graph = {
    0: [1, 2],
    1: [0, 2],
    2: [0, 1]
}
```

Here is the Python code:

```python
graph = [(0, 1), (0, 2), (1, 2)]
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>What are some common Types and Categories of Graphs?</summary>

### What are some common Types and Categories of Graphs?

**Definition:**

Graphs serve as adaptable data structures for various computational tasks and real-world applications. Let's look at their diverse types.

**How it works:**

Types of Graphs

- **Undirected**: Edges lack direction, allowing free traversal between connected nodes. Mathematically, `(u, v)` as an edge implies `(v, u)` as well.
- **Directed (Digraph)**: Edges have a set direction, restricting traversal accordingly. An edge `(u, v)` doesn't guarantee `(v, u)`.
- **Graph Types**: Unidirected, Directed

Weight Considerations

- **Weighted**: Each edge has a numerical "weight" or "cost."
- **Unweighted**: All edges are equal in weight, typically considered as 1.
- **Graph Types**: Weighted, Unweighted

Presence of Cycles

- **Cyclic**: Contains at least one cycle or closed path.
- **Acyclic**: Lacks cycles entirely.
- **Graph Types**: Cyclic, Acyclic

Edge Density

- **Dense**: High edge-to-vertex ratio, nearing the maximum possible connections.
- **Sparse**: Low edge-to-vertex ratio, closer to the minimum.
- **Graph Types**: Sparse, Dense

Connectivity

- **Connected**: Every vertex is reachable from any other vertex.
- **Disconnected**: Some vertices are unreachable from others.
- **Graph Types**: Connected, Disconnected

Edge Uniqueness

- **Multigraph**: Allows duplicate edges between vertices.
- **Simple**: Limits vertices to a single connecting edge.
- **Graph Types**: Multigraph, Simple Graph

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>What is the difference between a Tree and a Graph?</summary>

### What is the difference between a Tree and a Graph?

**Definition:**

Graphs and trees are both nonlinear data structures, but there are fundamental distinctions between them.

**How it works:**

Key Distinctions

- **Uniqueness**: Trees have a single root, while graphs may not have such a concept.
- **Topology**: Trees are hierarchical, while graphs can exhibit various structures.
- **Focus**: Graphs center on relationships between individual nodes, whereas trees emphasize the relationship between nodes and a common root.

Graphs: Versatile and Unstructured

- **Elements**: Composed of vertices/nodes (denoted as V) and edges (E) representing relationships. Multiple edges and loops are possible.
- **Directionality**: Edges can be directed or undirected.
- **Connectivity**: May be disconnected, with sets of vertices that aren't reachable from others.
- **Loops**: Can contain cycles.

Trees: Hierarchical and Organized

- **Elements**: Consist of nodes with parent-child relationships.
- **Directionality**: Edges are strictly parent-to-child.
- **Connectivity**: Every node is accessible from the unique root node.
- **Loops**: Cycles are not allowed.

Visual Representation

Graph vs Tree

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>How can you determine the Minimum number of edges for a graph to remain connected?</summary>

### How can you determine the Minimum number of edges for a graph to remain connected?

**Definition:**

To ensure a graph remains connected, it must have a minimum number of edges determined by the number of vertices. This is known as the edge connectivity of the graph.

**How it works:**

Edge Connectivity Formula

The minimum number of edges required for a graph to remain connected is given by:

Edge Connectivity

= `max(δ(G), 1)`

Where:

`δ(G)` is the minimum degree of a vertex in G.

The maximum function ensures that the graph remains connected even if all vertices have a degree of 1 or 0.

For example, a graph with a minimum vertex degree of 3 or more requires at least 3 edges to stay connected.

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Define Euler Path and Euler Circuit in the context of graph theory.</summary>

### Define Euler Path and Euler Circuit in the context of graph theory.

**Definition:**

In graph theory, an Euler Path and an Euler Circuit serve as methods to visit all edges (links) exactly once, with the distinction that an Euler Circuit also visits all vertices once.

**How it works:**

Euler Path and Euler Circuit Definitions

A graph has an Euler Path if it contains exactly two vertices of odd degree.

A graph has an Euler Circuit if every vertex has even degree.

Degree specifies the number of edges adjacent to a vertex.

Key Concepts

- **Starting Vertex**: In an Euler Path, the unique starting and ending vertices are the two with odd degrees.
- **Reachability**: In both Euler Path and Circuit, every edge must be reachable from the starting vertex.
- **Direction-Consistency**: While an Euler Path is directionally open-ended, an Euler Circuit is directionally closed.

Visual Representation: Euler Path and Circuit

Euler Path and Euler Circuit

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

### Graph Representations

<details>
<summary>Compare Adjacency Lists and Adjacency Matrices for graph representation.</summary>

### Compare Adjacency Lists and Adjacency Matrices for graph representation.

**Definition:**

Graphs can be represented in various ways, but Adjacency Matrix and Adjacency List are the most commonly used data structures. Each method offers distinct advantages and trade-offs, which we'll explore below.

**How it works:**

Example Graph

Space Complexity

- **Adjacency Matrix**: Requires a `N × N` matrix, resulting in `O(N^2)` space complexity.
- **Adjacency List**: Utilizes a list or array for each node's neighbors, leading to `O(N + E)` space complexity, where E is the number of edges.

Time Complexity for Edge Look-Up

- **Adjacency Matrix**: Constant time, `O(1)`, as the presence of an edge is directly accessible.

- **Adjacency List**: Up to `O(k)`, where k is the degree of the vertex, as the list of neighbors may need to be traversed.

Time Complexity for Traversal

- **Adjacency Matrix**: Requires `O(N^2)` time to iterate through all potential edges.
- **Adjacency List**: Takes `O(N + E)` time, often faster for sparse graphs.

Time Complexity for Edge Manipulation

- **Adjacency Matrix**: `O(1)` for both addition and removal, as it involves updating a single cell.
- **Adjacency List**: `O(k)` for addition or removal, where k is the degree of the vertex involved.

Time Complexity for Vertex Manipulation

- **Adjacency Matrix**: `O(N^2)` as resizing the matrix is needed.
- **Adjacency List**: `O(1)` as it involves updating a list or array.

**Examples:**

Code Example: Adjacency Matrix & Adjacency List

Here is the Python code:

```python
adj_matrix = [
    [0, 1, 1, 0, 0, 0],
    [1, 0, 0, 1, 0, 0],
    [1, 0, 0, 0, 0, 1],
    [0, 1, 0, 0, 1, 1],
    [0, 0, 0, 1, 0, 0],
    [0, 0, 1, 1, 0, 0]
]

adj_list = [
    [1, 2],
    [0, 3],
    [0, 5],
    [1, 4, 5],
    [3],
    [2, 3]
]
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>What is an Incidence Matrix, and when would you use it?</summary>

### What is an Incidence Matrix, and when would you use it?

**Definition:**

An incidence matrix is a binary graph representation that maps vertices to edges. It's especially useful for directed and multigraphs. The matrix contains $0$s and $1$s, with positions corresponding to "vertex connected to edge" relationships.

**How it works:**

Matrix Structure

Columns: Represent edges

Rows: Represent vertices

- **Cells**: Indicate whether a vertex is connected to an edge
Each unique row-edge pair depicts an incidence of a vertex in an edge, relating to the graph's structure differently based on the graph type.

Example: Incidence Matrix for a Directed Graph

Directed Graph

Example: Incidence Matrix for an Undirected Multigraph

Uniderected Graph

Applications of Incidence Matrices

- **Algorithm Efficiency**: Certain matrix operations can be faster than graph traversals.
- **Graph Comparisons**: It enables direct graph-to-matrix or matrix-to-matrix comparisons.
- **Database Storage**: A way to represent graphs in databases amongst others.
- **Graph Transformations**: Useful in transformations like line graphs and dual graphs.

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Discuss Edge List as a graph representation and its use cases.</summary>

### Discuss Edge List as a graph representation and its use cases.

**Definition:**

Edge list is a straightforward way to represent graphs. It's apt for dense graphs and offers a quick way to query edge information.

**How it works:**

Key Concepts

- **Edge Storage**: The list contains tuples (a, b) to denote an edge between nodes a and b.
- **Edge Direction**: The edges can be directed or undirected.
- **Edge Duplicates**: Multiple occurrences signal multigraph. Absence ensures simple graph.

Visual Example

Edge List Example

**Examples:**

Code Example: Edge List

Here is the Python 3 code:

```python
# Example graph
edges = {('A', 'B'), ('A', 'C'), ('B', 'C'), ('C', 'D'), ('B', 'D'), ('D', 'E')}

# Check existence
print(('A', 'B') in edges)  # True
print(('B', 'A') in edges)  # False
print(('A', 'E') in edges)  # False

# Adding an edge
edges.add(('E', 'C'))

# Removing an edge
edges.remove(('D', 'E'))

print(edges)  # Updated set: {('A', 'C'), ('B', 'D'), ('C', 'D'), ('A', 'B'), ('E', 'C'), ('B', 'C')}
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Explain how to save space while storing a graph using Compressed Sparse Row (CSR).</summary>

### Explain how to save space while storing a graph using Compressed Sparse Row (CSR).

**Definition:**

In Compressed Sparse Row format, the graph is represented by three linked arrays. This streamlined approach can significantly reduce memory use and is especially beneficial for sparse graphs.

Let's go through the data structures and the detailed process.

**How it works:**

Data Structures

Indptr Array (IA): A list of indices where each row starts in the adj_indices array. It's of length n_vertices + 1.
Adjacency Index Array (AA): The column indices for each edge based on their position in the indptr array.
- **Edge Data**: The actual edge data. This array's length matches the number of non-zero elements.

Visual Representation

CSR Graph Representation

**Examples:**

Code Example: CSR Graph Representation

Here is the Python code:

```python
indptr = [0, 2, 3, 5, 6, 7, 8]
indices = [2, 4, 0, 1, 3, 4, 2, 3]
data = [1, 2, 3, 4, 5, 6, 7, 8]

# Reading from the CSR Format
for i in range(len(indptr) - 1):
    start = indptr[i]
    end = indptr[i + 1]
    print(f"Vertex {i} is connected to vertices {indices[start:end]} with data {data[start:end]}")

# Writing a CSR Represented Graph
# Vertices 0 to 5, Inclusive.
# 0 -> [2, 3, 4] - Data [3, 5, 7]
# 1 -> [0] - Data [1]
# 2 -> [] - No outgoing edges.
# 3 -> [1] - Data [2]
# 4 -> [3] - Data [4]
# 5 -> [2] - Data [6]

# Code to populate:
# indptr =  [0, 3, 4, 4, 5, 6, 7]
# indices = [2, 3, 4, 0, 1, 3, 2]
# data = [3, 5, 7, 1, 2, 4, 6]
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

### Graph Traversal Algorithms

<details>
<summary>Explain the Breadth-First Search (BFS) traversing method.</summary>

### Explain the Breadth-First Search (BFS) traversing method.

**Definition:**

Breadth-First Search (BFS) is a graph traversal technique that systematically explores a graph level by level. It uses a queue to keep track of nodes to visit next and a list to record visited nodes, avoiding redundancy.

**How it works:**

Key Components

- **Queue**: Maintains nodes in line for exploration.
- **Visited List**: Records nodes that have already been explored.

Algorithm Steps

- **Initialize**: Choose a starting node, mark it as visited, and enqueue it.
- **Explore**: Keep iterating as long as the queue is not empty. In each iteration, dequeue a node, visit it, and enqueue its unexplored neighbors.
- **Terminate**: Stop when the queue is empty.

Visual Representation

BFS Example

Complexity Analysis

Time Complexity: `O(V + E)` where V is the number of vertices in the graph and E is the number of edges. This is because each vertex and each edge will be explored only once.

- **Space Complexity**: `O(V)` since, in the worst case, all of the vertices can be inside the queue.

**Examples:**

Code Example: Breadth-First Search

Here is the Python code:

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    
    while queue:
        vertex = queue.popleft()
        if vertex not in visited:
            print(vertex, end=' ')
            visited.add(vertex)
            queue.extend(neighbor for neighbor in graph[vertex] if neighbor not in visited)

# Sample graph representation using adjacency sets
graph = {
    'A': {'B', 'D', 'G'},
    'B': {'A', 'E', 'F'},
    'C': {'F'},
    'D': {'A', 'F'},
    'E': {'B'},
    'F': {'B', 'C', 'D'},
    'G': {'A'}
}

# Execute BFS starting from 'A'
bfs(graph, 'A')
# Expected Output: 'A B D G E F C'
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Explain the Depth-First Search (DFS) algorithm.</summary>

### Explain the Depth-First Search (DFS) algorithm.

**Definition:**

Depth-First Search (DFS) is a graph traversal algorithm that's simpler and often faster than its breadth-first counterpart (BFS). While it might not explore all vertices, DFS is still fundamental to numerous graph algorithms.

**How it works:**

Algorithm Steps

- **Initialize**: Select a starting vertex, mark it as visited, and put it on a stack.

Loop: Until the stack is empty, do the following:

Remove the top vertex from the stack.
Explore its unvisited neighbors and add them to the stack.
- **Finish**: When the stack is empty, the algorithm ends, and all reachable vertices are visited.

Visual Representation

DFS Example

**Examples:**

Code Example: Depth-First Search

Here is the Python code:

```python
def dfs(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            stack.extend(neighbor for neighbor in graph[vertex] if neighbor not in visited)
    
    return visited

# Example graph
graph = {
    'A': {'B', 'G'},
    'B': {'A', 'E', 'F'},
    'G': {'A'},
    'E': {'B', 'G'},
    'F': {'B', 'C', 'D'},
    'C': {'F'},
    'D': {'F'}
}

print(dfs(graph, 'A'))  # Output: {'A', 'B', 'C', 'D', 'E', 'F', 'G'}
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>What are the key differences between BFS and DFS?</summary>

### What are the key differences between BFS and DFS?

**Definition:**

BFS and DFS are both essential graph traversal algorithms with distinct characteristics in strategy, memory requirements, and use-cases.

**How it works:**

Core Differences

- **Search Strategy**: BFS moves level-by-level, while DFS goes deep into each branch before backtracking.
- **Data Structures**: BFS uses a Queue, whereas DFS uses a Stack or recursion.
- **Space Complexity**: BFS requires more memory as it may need to store an entire level ($O(|V|)$), whereas DFS usually uses less ( `O(log n)` on average).
- **Optimality**: BFS guarantees the shortest path; DFS does not.

Visual Representation

BFS

BFS Traversal

DFS

DFS Traversal

**Examples:**

Code Example: BFS & DFS

Here is the Python code:

```python
# BFS
def bfs(graph, start):
    visited = set()
    queue = [start]
    while queue:
        node = queue.pop(0)
        if node not in visited:
            visited.add(node)
            queue.extend(graph[node] - visited)
            
# DFS
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    for next_node in graph[start] - visited:
        dfs(graph, next_node, visited)
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Implement a method to check if there is a Path between two vertices in a graph.</summary>

### Implement a method to check if there is a Path between two vertices in a graph.

**Definition:**

Problem Statement

Given an undirected graph, the task is to determine whether or not there is a path between two specified vertices.

Solution

The problem can be solved using Depth-First Search (DFS).

Algorithm Steps

Start from the source vertex.
For each adjacent vertex, if not visited, recursively perform DFS.
If the destination vertex is found, return True. Otherwise, backtrack and explore other paths.

Complexity Analysis

Time Complexity: `O(V + E)`

V is the number of vertices, and E is the number of edges.

Space Complexity: `O(V)`

For the stack used in recursive DFS calls.

Implementation

**Examples:**

Here is the Python code:

```python
from collections import defaultdict

class Graph:
    def __init__(self):
        self.graph = defaultdict(list)

    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)

    def is_reachable(self, src, dest, visited):
        visited[src] = True

        if src == dest:
            return True

        for neighbor in self.graph[src]:
            if not visited[neighbor]:
                if self.is_reachable(neighbor, dest, visited):
                    return True

        return False

    def has_path(self, src, dest):
        visited = defaultdict(bool)
        return self.is_reachable(src, dest, visited)

# Usage
g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 3)
g.add_edge(3, 3)

source, destination = 0, 3
print(f"There is a path between {source} and {destination}: {g.has_path(source, destination)}")
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Solve the problem of printing all Paths from a source to destination in a Directed Graph with BFS or DFS.</summary>

### Solve the problem of printing all Paths from a source to destination in a Directed Graph with BFS or DFS.

**Definition:**

Problem Statement

Given a directed graph and two vertices src and dest, the objective is to print all paths from src to dest.

Solution

Recursive Depth-First Search (DFS) Algorithm in Graphs: DFS is used because it can identify all the paths in a graph from source to destination. This is done by employing a backtracking mechanism to ensure that all unique paths are found.

To deal with cycles, a list of visited nodes is crucial. By utilizing this list, the algorithm can avoid revisiting and getting stuck in an infinite loop.

Complexity Analysis

Time Complexity: `O(V+E)`

V is the number of vertices and E is the number of edges.
We're essentially visiting every node and edge once.

Space Complexity: `O(V)`

In the worst-case scenario, the entire graph can be visited, which would require space proportional to the number of vertices.

Implementation

**Examples:**

Here is the Python code:

```python
# Python program to print all paths from a source to destination in a directed graph

from collections import defaultdict

# A class to represent a graph
class Graph:
    def __init__(self, vertices):
        # No. of vertices
        self.V = vertices

        # default dictionary to store graph
        self.graph = defaultdict(list)

    def addEdge(self, u, v):
        self.graph[u].append(v)

    def printAllPathsUtil(self, u, d, visited, path):
        # Mark the current node as visited and store in path
        visited[u] = True
        path.append(u)

        # If current vertex is same as destination, then print current path
        if u == d:
            print(path)
        else:
            # If current vertex is not destination
            # Recur for all the vertices adjacent to this vertex
            for i in self.graph[u]:
                if not visited[i]:
                    self.printAllPathsUtil(i, d, visited, path)

        # Remove current vertex from path and mark it as unvisited
        path.pop()
        visited[u] = False

    # Prints all paths from 's' to 'd'
    def printAllPaths(self, s, d):
        # Mark all the vertices as not visited
        visited = [False] * (self.V)

        # Create an array to store paths
        path = []
        path.append(s)

        # Call the recursive helper function to print all paths
        self.printAllPathsUtil(s, d, visited, path)

# Create a graph given in the above diagram
g = Graph(4)
g.addEdge(0, 1)
g.addEdge(0, 2)
g.addEdge(0, 3)
g.addEdge(2, 0)
g.addEdge(2, 1)
g.addEdge(1, 3)

s = 2 ; d = 3
print("Following are all different paths from %d to %d :" %(s, d))
g.printAllPaths(s, d)
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

### Graph Properties and Types

<details>
<summary>What is a Bipartite Graph? How to detect one?</summary>

### What is a Bipartite Graph? How to detect one?

**Definition:**

A bipartite graph is one where the vertices can be divided into two distinct sets, U and V, such that every edge connects a vertex from U to one in V. The graph is denoted as G = (U, V, E), where E represents the set of edges.

**How it works:**

Detecting a Bipartite Graph

You can check if a graph is bipartite using several methods:

Breadth-First Search (BFS)

BFS is often used for this purpose. The algorithm colors vertices alternately so that no adjacent vertices have the same color.

Cycle Detection

A graph is not bipartite if it contains an odd cycle. Algorithms like DFS or Floyd's cycle-detection algorithm can help identify such cycles.

**Examples:**

Code Example: Bipartite Graph using BFS

Here is the Python code:

```python
from collections import deque

def is_bipartite_bfs(graph, start_node):
    visited = {node: False for node in graph}
    color = {node: None for node in graph}
    color[start_node] = 1
    queue = deque([start_node])

    while queue:
        current_node = queue.popleft()
        visited[current_node] = True

        for neighbor in graph[current_node]:
            if not visited[neighbor]:
                queue.append(neighbor)
                color[neighbor] = 1 - color[current_node]
            elif color[neighbor] == color[current_node]:
                return False

    return True

# Example
graph = {'A': ['B', 'C'], 'B': ['A', 'C'], 'C': ['A', 'B', 'D'], 'D': ['C']}
print(is_bipartite_bfs(graph, 'A'))  # Output: True
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Define Cyclic and Acyclic graphs and explain how to identify them.</summary>

### Define Cyclic and Acyclic graphs and explain how to identify them.

**Definition:**

A cyclic graph contains a closed walk: a path that returns to a vertex already on the current route. An acyclic graph has no such closed walk. A directed acyclic graph is the directed case, and it is exactly the directed graph that has a topological order.

**How it works:**

On a directed graph, depth-first search reports a cycle when it reaches a vertex that is still on the current recursion path. GeeksforGeeks keeps `visited[]` for vertices seen at least once and `recStack[]` for the current path, because a vertex visited on an earlier branch is not by itself a cycle. After the neighbors of `u` are finished, `recStack[u]` is cleared. Kahn’s algorithm is the other check on the same page: compute indegrees, queue vertices of indegree 0, and decrement neighbors. If the number of processed vertices is not `V`, some vertices remain and the graph has a cycle. Both checks are O(V + E) time and O(V) extra space. An undirected cycle is a different test: BFS can detect one, and a traversal edge to an already visited vertex is a cycle only when that vertex is not the parent the search just came from.

**Examples:**

```python
def is_cyclic_directed(V, edges):
    adj = [[] for _ in range(V)]
    for u, v in edges:
        adj[u].append(v)
    indegree = [0] * V
    for u in range(V):
        for v in adj[u]:
            indegree[v] += 1
    q = [u for u in range(V) if indegree[u] == 0]
    seen = 0
    i = 0
    while i < len(q):
        u = q[i]
        i += 1
        seen += 1
        for v in adj[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                q.append(v)
    return seen != V
```

**Sources:**

- [Detect Cycle in a Directed Graph](https://www.geeksforgeeks.org/dsa/detect-cycle-in-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Explain Planar graphs and the conditions that make a graph planar.</summary>

### Explain Planar graphs and the conditions that make a graph planar.

**Definition:**

A planar graph can be drawn in the plane so that edges meet only at their endpoints. That drawing is a plane graph, or a planar embedding.

**How it works:**

Kuratowski’s theorem: a finite graph is planar if and only if it has no subgraph that is a subdivision of K5 or of K3,3. Wagner’s theorem says the same with minors instead of subdivisions. For a simple connected planar graph with v ≥ 3, e ≤ 3v − 6; if it also has no 3-cycles, e ≤ 2v − 4. Those inequalities are necessary, not sufficient: they can prove a graph is non-planar, not that it is planar. Euler’s formula for a finite connected plane graph is v − e + f = 2, counting the outer face. Planarity of an n-vertex graph can be tested in O(n) time. Because e is O(v), planar graphs are sparse next to the maximum O(v²) edges.

**Sources:**

- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

<details>
<summary>What are Dense and Sparse graphs? Discuss their density measures and typical use cases.</summary>

### What are Dense and Sparse graphs? Discuss their density measures and typical use cases.

**Definition:**

A dense graph has far more edges than vertices. A sparse graph has very few edges relative to the number of vertices.

**How it works:**

Floyd–Warshall always runs in O(V³), three nested loops, no matter how many edges exist, so GeeksforGeeks recommends it for dense graphs and Johnson’s algorithm for sparse ones. A simple connected planar graph is sparse in the same sense: it has O(v) edges, while the maximum simple graph has O(v²). An adjacency matrix stores a V×V table and suits the dense all-pairs update. An adjacency list, as in the DFS and BFS implementations, stores each edge once (twice if the graph is undirected) and suits a sparse traversal whose time is O(V + E).

**When to use:**

Use a matrix and Floyd–Warshall when most pairs have an edge. Use an adjacency list and a traversal or Dijkstra when E is much closer to V than to V².

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

### Graph Algorithms and Complexity

<details>
<summary>Why the complexity of DFS is O(V+E)?</summary>

### Why the complexity of DFS is O(V+E)?

**Definition:**

Depth-first search on an adjacency list is O(V + E) because each vertex is visited at most once and each edge is followed a constant number of times.

**How it works:**

GeeksforGeeks states the bound directly: every vertex is visited at most once, and every edge is traversed at most once in a directed graph and twice in an undirected graph (once from each end of the adjacency-list entry). A disconnected graph does not change the bound: the outer loop starts a search at every unvisited vertex, and the visited array still processes each vertex and edge once. The auxiliary space they report is O(V + E): a visited array of size V plus the recursion stack, on top of the adjacency list that already stores the edges.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Provide an implementation for Topological Sort in a Directed Acyclic Graph (DAG).</summary>

### Provide an implementation for Topological Sort in a Directed Acyclic Graph (DAG).

**Definition:**

A topological order is a sequence of all vertices such that every directed edge goes from an earlier vertex to a later one. It exists only when the directed graph is acyclic.

**How it works:**

Kahn’s algorithm on the cycle-detection page is a BFS topological sort. Compute each indegree. Queue every vertex with indegree 0. Pop a vertex, append it to the order, and decrement the indegree of each neighbor; enqueue a neighbor when its indegree hits 0. If the order contains V vertices, the graph is a DAG and the order is topological. If fewer than V vertices are processed, a cycle blocked the rest. Time is O(V + E) and extra space is O(V).

**Examples:**

```python
from collections import deque

def topo_sort(V, edges):
    adj = [[] for _ in range(V)]
    indegree = [0] * V
    for u, v in edges:
        adj[u].append(v)
        indegree[v] += 1
    q = deque(u for u in range(V) if indegree[u] == 0)
    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in adj[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                q.append(v)
    return order if len(order) == V else None
```

**Sources:**

- [Detect Cycle in a Directed Graph](https://www.geeksforgeeks.org/dsa/detect-cycle-in-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Write an algorithm to detect a Cycle in an undirected graph.</summary>

### Write an algorithm to detect a Cycle in an undirected graph.

**Definition:**

An undirected graph has a cycle when a traversal reaches a visited vertex that is not the parent it just came from.

**How it works:**

BFS and DFS both walk an adjacency list and mark vertices visited. In an undirected graph each edge appears at both ends, so the neighbor you just arrived from is always already visited and is not a cycle. Any other visited neighbor is a back edge and means a cycle. GeeksforGeeks lists cycle detection as a BFS application and gives BFS time O(V + E) with O(V) auxiliary space for the queue. DFS on the same representation is also O(V + E). A directed cycle uses a different test (the recursion stack), because a cross edge to a finished vertex is not a cycle.

**Examples:**

```python
def has_undirected_cycle(V, edges):
    adj = [[] for _ in range(V)]
    for u, v in edges:
        adj[u].append(v)
        adj[v].append(u)
    seen = [False] * V
    for start in range(V):
        if seen[start]:
            continue
        stack = [(start, -1)]
        seen[start] = True
        while stack:
            u, parent = stack.pop()
            for v in adj[u]:
                if not seen[v]:
                    seen[v] = True
                    stack.append((v, u))
                elif v != parent:
                    return True
    return False
```

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Discuss the complexity of finding All Pair Shortest Paths in a weighted graph.</summary>

### Discuss the complexity of finding All Pair Shortest Paths in a weighted graph.

**Definition:**

All-pairs shortest paths fill a V×V table of distances. Floyd–Warshall does that in O(V³) time.

**How it works:**

The algorithm starts from the direct-edge matrix (missing edges are a large INF, and the diagonal is 0) and tries every vertex k as an intermediate: dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]). Three nested loops of size V give O(V³). Extra space beyond the matrix they update in place is O(1). It allows negative edges and rejects negative cycles. The same page says this cubic bound does not depend on E, so it fits dense graphs; Johnson’s algorithm is the sparse alternative they name. Running the binary-heap Dijkstra from every source is a different all-pairs method for non-negative weights: each run is O((V + E) log V) on that page.

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

### Memory and Performance in Graph Operations

<details>
<summary>Why does Breadth-First Search use more memory than Depth-First Search?</summary>

### Why does Breadth-First Search use more memory than Depth-First Search?

**Definition:**

The published auxiliary bounds do not say BFS always uses more memory. BFS is charged O(V) for its queue. DFS is charged O(V + E) for the visited array plus the recursion stack.

**How it works:**

Both searches mark vertices so a cycle is not walked forever. BFS keeps a queue of the frontier and visits level by level; GeeksforGeeks lists auxiliary space O(V) for that queue. DFS recurses down one path and backtracks; the same site lists auxiliary space O(V + E) because of the visited array of size V and the recursive calls, on top of the adjacency list. The queue and the call stack are different shapes: the queue holds the current level, the call stack holds the current path. Which one is larger depends on the graph (a wide level versus a long path). Quote the O(V) and O(V + E) figures from those pages rather than a single winner.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Illustrate the difference in Peak Memory Consumption between DFS and BFS.</summary>

### Illustrate the difference in Peak Memory Consumption between DFS and BFS.

**Definition:**

Peak extra memory is the largest live frontier. For the GeeksforGeeks BFS that is the queue, bounded as auxiliary O(V). For their recursive DFS it is the call stack plus the visited array, reported as auxiliary O(V + E).

**How it works:**

At any moment BFS stores every vertex of the current level in the queue, then expands the next level. DFS stores one path: each recursive call is a vertex that has not backtracked yet. A star visited from the center makes the BFS queue hold every leaf at once. A single long path makes the DFS stack hold every vertex and leaves the BFS queue at size 1. Both still allocate a visited array of length V. The adjacency list is shared input, not part of the frontier. Do not add a tighter peak than O(V) for the queue or the O(V + E) auxiliary figure the DFS page states.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>How can Graph Data Structures be optimized for better Cache Locality?</summary>

### How can Graph Data Structures be optimized for better Cache Locality?

**Definition:**

Traversal cost follows how neighbors are stored. An adjacency matrix is one contiguous V×V table. An adjacency list is an array of neighbor lists, which is what the DFS, BFS, and Dijkstra pages use.

**How it works:**

A matrix scan of a row reads consecutive entries, which matches a dense graph where Floyd–Warshall already touches every pair in O(V³). A list stores only existing edges, so a sparse graph does not allocate V² cells; neighbor ids may sit in separate small arrays, and following them jumps. For a sparse graph the list still wins on memory and on the O(V + E) scans. For a dense graph the matrix avoids a pointer per edge and is the structure Floyd–Warshall updates in place. Packing each vertex’s neighbor array into one contiguous buffer keeps the list’s O(V + E) bound and makes one vertex’s neighbors adjacent in memory. That packing is a layout of the same adjacency list; it does not change the asymptotic bounds on those pages.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Discuss methods to Reduce Memory Usage in graph representations.</summary>

### Discuss methods to Reduce Memory Usage in graph representations.

**Definition:**

Store each edge as few times as the algorithm needs, and do not allocate a V×V table unless the algorithm fills one.

**How it works:**

An adjacency list stores the edges that exist. DFS and BFS use that form and charge the traversal O(V + E), not O(V²). An undirected list stores each edge twice, once at each endpoint, which is why DFS says an undirected edge is traversed twice. A directed list stores it once. Kruskal’s algorithm sorts an edge list and uses a disjoint-set of size V; its extra space is O(E + V), and it never builds a matrix. Floyd–Warshall needs the V×V distance matrix, so it is the representation to avoid when the graph is sparse; the page points sparse all-pairs work at Johnson’s algorithm instead. Planar graphs have only O(v) edges, so a list stays linear in v while a matrix is quadratic.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

### Shortest Path Algorithms

<details>
<summary>What is the difference between BFS and Dijkstra’s algorithms when looking for the shortest path?</summary>

### What is the difference between BFS and Dijkstra’s algorithms when looking for the shortest path?

**Definition:**

BFS finds a shortest path when every edge has the same cost, because it visits vertices in order of hop distance. Dijkstra finds a shortest path when edges have non-negative weights.

**How it works:**

BFS uses a FIFO queue and expands level by level. GeeksforGeeks lists shortest paths in an unweighted graph as a BFS application, with time O(V + E). Dijkstra keeps a distance array, initially infinite except 0 at the source, and a priority queue of (distance, vertex). It pops the smallest distance, skips a stale entry when the popped distance is greater than dist[u], and relaxes dist[v] = dist[u] + w. With a binary heap the page gives O((V + E) log V) time and O(V + E) extra space. The finalized-distance step is valid only with non-negative weights: a later negative edge could undercut a distance Dijkstra already treated as final.

**When to use:**

Use BFS for hop counts. Use Dijkstra when weights are non-negative and not all equal. Use Bellman–Ford if a weight can be negative.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Implement Dijkstra’s Algorithm for finding the shortest path in a graph with non-negative edge weights.</summary>

### Implement Dijkstra’s Algorithm for finding the shortest path in a graph with non-negative edge weights.

**Definition:**

Dijkstra computes shortest distances from one source in a weighted graph that has no negative edge.

**How it works:**

Initialize dist[] to infinity and dist[src] to 0. Push (0, src) on a min-heap. While the heap is not empty, pop (d, u). If d > dist[u], this entry is stale and is skipped. For each neighbor (v, w), if dist[u] + w < dist[v], set dist[v] and push (dist[v], v). The page’s reason it is safe to finalize u when it is popped: with non-negative weights, any other route would pass through a vertex whose distance is at least dist[u], so it cannot be shorter. Time is O((V + E) log V). Extra space is O(V + E).

**Examples:**

```python
import heapq

def dijkstra(adj, src):
    dist = [float("inf")] * len(adj)
    dist[src] = 0
    pq = [(0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(pq, (dist[v], v))
    return dist
```

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>How does Bellman-Ford Algorithm differ from Dijkstra’s and where would you use it?</summary>

### How does Bellman-Ford Algorithm differ from Dijkstra’s and where would you use it?

**Definition:**

Bellman–Ford is a single-source shortest-path algorithm that allows negative edges and reports a negative cycle. Dijkstra does not.

**How it works:**

Relax every edge V − 1 times: if dist[u] is finite and dist[u] + w < dist[v], set dist[v] = dist[u] + w. A simple path has at most V − 1 edges, so that many full passes cover every simple path from the source. One more successful relaxation means a negative cycle, and the page’s implementation returns −1. Unreachable vertices stay at the sentinel 10^8. The stated bound is O(V·E) time and O(V) space. Dijkstra’s priority-queue finalization fails on a negative edge because it does not revisit a vertex it already popped. Use Bellman–Ford when a weight may be negative or you must detect a negative cycle. Use Dijkstra when every weight is non-negative and you want the O((V + E) log V) heap bound.

**Sources:**

- [Bellman–Ford Algorithm](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Explain the concept of Floyd-Warshall Algorithm and its usage.</summary>

### Explain the concept of Floyd-Warshall Algorithm and its usage.

**Definition:**

Floyd–Warshall computes shortest distances between every pair by trying each vertex as an intermediate.

**How it works:**

dist[i][j] starts as the direct weight, INF if there is no edge, and 0 on the diagonal. For k from 0 to V − 1, and for every pair (i, j), replace dist[i][j] with dist[i][k] + dist[k][j] when that sum is smaller and neither piece is INF. The optimal-substructure claim on the page: if a shortest i–j path goes through k, the i–k and k–j pieces are themselves shortest. It works on directed and undirected weighted graphs, including negative edges, and not on a negative cycle. Time is O(V³). In-place extra space is O(1). The page uses it for network routing, airport connectivity, and GIS road networks, and prefers it on dense graphs.

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
</details>

### Heuristic-Based Graph Search Algorithms

<details>
<summary>What is `A*` Search?</summary>

### What is `A*` Search?

**Definition:**

A* picks the next node by the lowest f, where f = g + h: g is the cost from the start along the path already built, and h is an estimate of the remaining cost to the goal.

**How it works:**

GeeksforGeeks presents it on a grid with obstacles. At each step the open cell with the smallest f is expanded. g is known because it follows the path generated so far. h is a guess, because walls can block the straight route. The same write-up says games and web maps use it to find a short path. The admissibility page adds: if h(n) is never larger than the true remaining cost h*(n), and h(goal) = 0, A* returns an optimal path. A consistent heuristic also satisfies h(n) ≤ c(n, p) + h(p), so f does not decrease along a path and a closed node need not be reopened.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>What is the heuristic cost function in `A*` Search?</summary>

### What is the heuristic cost function in `A*` Search?

**Definition:**

The heuristic h(n) estimates the cheapest cost from n to the goal. It is not the cost already paid; that part is g(n).

**How it works:**

On a grid, GeeksforGeeks names three estimates: Manhattan distance (sum of absolute x and y differences) when moves are only horizontal and vertical; Euclidean distance (straight-line) when movement may go in any direction; Chebyshev distance when a diagonal step costs the same as an orthogonal step. Admissible means h(n) ≤ h*(n) for every n, with h(goal) = 0. Overestimating can make A* expand a node whose f looks small and miss the optimal path. Consistent (monotone) means h(n) ≤ c(n, p) + h(p) for every neighbor p.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>Explain the difference between Best-First Search and `A*` Search.</summary>

### Explain the difference between Best-First Search and `A*` Search.

**Definition:**

Greedy best-first search orders nodes by the heuristic h alone. A* orders them by f = g + h, so the cost already spent still counts.

**How it works:**

Best-first expands the node that looks closest to the goal. That can dive toward the goal and ignore a longer-looking start that is actually cheaper, because g is not in the key. A* will not prefer that node once g is added, and with an admissible h it is optimal. Both still need a way to avoid re-expanding a node; A* with a consistent heuristic processes each node at most once. Use greedy best-first when a fast plausible path is enough. Use A* when the path must be optimal under the edge costs.

**Examples:**

```text
best-first key: h(n)
A* key:         g(n) + h(n)
```

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>Provide an implementation of the Greedy Best-First Search and compare it to `A*`.</summary>

### Provide an implementation of the Greedy Best-First Search and compare it to `A*`.

**Definition:**

Greedy best-first search expands the open node with the smallest h(n). A* expands the open node with the smallest g(n) + h(n).

**How it works:**

Keep a min-heap of (h, node) and a visited set. Pop the smallest h, and if it is the goal, stop. Otherwise push each unvisited neighbor with its own h. The path can be reconstructed with parent pointers. Compared with A*, the heap key drops g, so a neighbor that is one step from the goal and reached by an expensive path is preferred over a neighbor that is two steps away and reached cheaply. A* adds g and, when h is admissible, does not make that trade. The grid heuristics on the A* page (Manhattan, Euclidean, Chebyshev) are the same functions you can pass as h.

**Examples:**

```python
import heapq

def greedy_best_first(start, goal, neighbors, h):
    heap = [(h(start), start)]
    parent = {start: None}
    seen = set()
    while heap:
        _, u = heapq.heappop(heap)
        if u in seen:
            continue
        seen.add(u)
        if u == goal:
            break
        for v in neighbors(u):
            if v not in parent:
                parent[v] = u
                heapq.heappush(heap, (h(v), v))
    return parent
```

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>How can you adapt `A*` Search to work on a puzzle game like Sudoku?</summary>

### How can you adapt `A*` Search to work on a puzzle game like Sudoku?

**Definition:**

Model puzzle states as vertices and legal moves as edges, then let A* expand the state with the smallest f = g + h.

**How it works:**

g(n) is the cost of the moves already made, for example the number of filled cells or the number of steps. h(n) must estimate remaining work and, for an optimal solution, must not exceed the true remaining cost. On the A* pages, Manhattan, Euclidean, and Chebyshev are grid estimates; a Sudoku board is not that grid, so those three distances are not the heuristic. A Sudoku heuristic has to be computed from the board, such as a count of empty cells (each empty cell needs at least one fill, so the count does not exceed the true number of fills). The search stores parent pointers and stops at a state that satisfies the rules. A* keeps every open state, and the heuristic page lists that memory use as the reason it becomes impractical on a very large graph.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

### Real-World Applications of Graphs

<details>
<summary>What are some real-world Applications of Graphs?</summary>

### What are some real-world Applications of Graphs?

**Definition:**

Graphs model things as vertices and relationships as edges, then a traversal or a shortest-path algorithm answers a question on that model.

**How it works:**

BFS is used for shortest paths in an unweighted network, cycle detection, connected components, and network routing. Dijkstra and A* are used when edges have non-negative costs; the A* page names games and web maps. Floyd–Warshall is used for all-pairs routing, flight connections, and GIS road networks. Kruskal’s algorithm connects every vertex at minimum total weight. Maximum flow sends as much as capacities allow from a source to a sink: the introduction lists communication routing, transportation, water pipes, bipartite matching, job assignment, and supply chains. Edge coloring schedules incidents that cannot share a vertex, including fiber frequency assignment.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
- [Edge coloring](https://en.wikipedia.org/wiki/Edge_coloring)
</details>

<details>
<summary>Provide some practical examples of using Depth-First Search vs Breadth-First Search.</summary>

### Provide some practical examples of using Depth-First Search vs Breadth-First Search.

**Definition:**

DFS finishes one path before it tries the next. BFS finishes a whole level before it goes deeper.

**How it works:**

Use DFS to walk every vertex of a component, including a disconnected graph by restarting at each unvisited vertex, or to detect a directed cycle with a recursion stack. Its time is O(V + E). Use BFS when the answer is the smallest number of edges: an unweighted shortest path, a connected component grown level by level, or a cycle check that the BFS page also lists. BFS time is O(V + E) and the queue is auxiliary O(V). A maze that must be solved in the fewest rooms is BFS. Exhausting one corridor before stepping back to a junction is DFS.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>How do social networks use graphs for recommendations and features like friend suggestions?</summary>

### How do social networks use graphs for recommendations and features like friend suggestions?

**Definition:**

People are vertices and friendships are edges. A suggestion is a nearby vertex that is not already a neighbor.

**How it works:**

BFS from a person visits friends, then friends of friends, in order of hop distance. That is the unweighted shortest-path use on the BFS page, so “people at distance 2” are exactly the second level. Vertices already adjacent to the start are current friends and are not suggestions. The same level-order walk is how a connected component finds everyone reachable from a person. This note does not add a ranking formula that was not on the fetched pages; the graph fact is the distance in the friendship graph.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Explain how a GPS navigation system might use graph data structures and algorithms.</summary>

### Explain how a GPS navigation system might use graph data structures and algorithms.

**Definition:**

Intersections are vertices and road segments are weighted edges. A route is a shortest path under those weights.

**How it works:**

If every segment has a non-negative cost (length or travel time), Dijkstra from the start computes dist[] with a priority queue in O((V + E) log V). A* does the same search but orders expansion by g + h. The A* page says web maps use it, and an admissible h is one that never exceeds the true remaining cost; straight-line (Euclidean) distance is the example of an admissible estimate on the heuristic page when movement is not restricted to a grid axis. BFS would be correct only if every segment had equal cost. Floyd–Warshall is the all-pairs alternative the GIS bullet on that page names, at O(V³), which fits a dense distance table rather than one query.

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

### Advanced Graph Topics

<details>
<summary>What is a Graph Embedding, and how is it applicable in machine learning?</summary>

### What is a Graph Embedding, and how is it applicable in machine learning?

**Definition:**

On the planar-graph page, a graph embedding is a drawing of the graph on a surface so edges meet only at vertices. The plane and the sphere have genus 0, which is the planar case.

**How it works:**

A planar embedding maps each vertex to a point and each edge to a curve whose endpoints are those points, with curves disjoint except at endpoints. Every plane drawing corresponds to a spherical drawing by stereographic projection. That is a topological embedding, not a table of feature vectors. The fetched page does not define a machine-learning map from vertices to numeric vectors, so this note does not name such an algorithm or a training loss. What the page does say for learning-adjacent use is indirect: planar graphs are sparse (O(v) edges), and the drawings are the object planarity testing classifies in O(n) time.

**Sources:**

- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

<details>
<summary>Describe the Min-Cut Max-Flow Theorem and its significance in network flow problems.</summary>

### Describe the Min-Cut Max-Flow Theorem and its significance in network flow problems.

**Definition:**

The max-flow min-cut theorem says the maximum flow from source to sink equals the capacity of a minimum cut: the smallest total capacity of edges whose removal disconnects the source from the sink.

**How it works:**

A flow network is a finite directed graph with a source, a sink, and a non-negative capacity on each edge. Flow on an edge cannot exceed capacity, and at every vertex other than source and sink, incoming flow equals outgoing flow. An augmenting path is a source-to-sink path whose residual capacity (capacity minus current flow) is positive on every edge; its bottleneck is the minimum residual on that path. Wikipedia’s pipe example is the significance: the smallest set of pipes that isolates the reservoir from the city sets the maximum water that can arrive. The introduction names Ford–Fulkerson, Dinic’s algorithm, and push-relabel as ways to compute the flow. The same quantity is a special case of linear-programming duality and is used to derive Menger’s theorem and the Kőnig–Egerváry theorem.

**Sources:**

- [Max-flow min-cut theorem](https://en.wikipedia.org/wiki/Max-flow_min-cut_theorem)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

<details>
<summary>Discuss the concept of Edge Coloring and its applications.</summary>

### Discuss the concept of Edge Coloring and its applications.

**Definition:**

A proper edge coloring assigns colors to edges so that no two edges that share a vertex receive the same color. The chromatic index is the smallest number of colors that suffice.

**How it works:**

Each color class is a matching: edges of one color share no vertex. By Vizing’s theorem a simple graph needs either Δ or Δ + 1 colors, where Δ is the maximum degree. Bipartite graphs and high-degree planar graphs need only Δ. Multigraphs may need as many as 3Δ/2. Optimal coloring of bipartite graphs, and a coloring of a non-bipartite simple graph with at most Δ + 1 colors, can be built in polynomial time. Finding an optimal coloring in general is NP-hard. An even cycle needs 2 colors; an odd cycle needs 3. Applications named on the page are scheduling and frequency assignment on fiber networks.

**Sources:**

- [Edge coloring](https://en.wikipedia.org/wiki/Edge_coloring)
</details>

<details>
<summary>Implement an algorithm that finds the Minimum Spanning Tree of a graph.</summary>

### Implement an algorithm that finds the Minimum Spanning Tree of a graph.

**Definition:**

A minimum spanning tree of a connected, undirected, weighted graph is a subset of edges with no cycle, connecting every vertex, of minimum total weight. It has V − 1 edges.

**How it works:**

Kruskal sorts edges by nondecreasing weight. A disjoint-set tracks components. For each edge in that order, if its endpoints lie in different sets, unite them and keep the edge; if they lie in the same set, the edge would close a cycle and is discarded. Stop at V − 1 kept edges. Sorting is O(E log E). Each find/union is at most O(log V), so the page’s overall bound is O(E log E) or O(E log V), because E is at most O(V²) and the logs match. Extra space is O(E + V).

**Examples:**

```python
def kruskal(V, edges):
    parent = list(range(V))
    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x
    cost = 0
    kept = 0
    for u, v, w in sorted(edges, key=lambda e: e[2]):
        a, b = find(u), find(v)
        if a == b:
            continue
        parent[a] = b
        cost += w
        kept += 1
        if kept == V - 1:
            break
    return cost
```

**Sources:**

- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
</details>

<details>
<summary>Explain the importance of Graph Databases and how they differ from traditional relational databases.</summary>

### Explain the importance of Graph Databases and how they differ from traditional relational databases.

**Definition:**

A graph database stores entities as vertices and relationships as edges, so a question about connections is a traversal. A relational database stores rows and rebuilds a connection by matching keys.

**How it works:**

Walking friends-of-friends is the BFS level order on an adjacency list: time follows O(V + E) for the vertices and edges you touch, not a V×V matrix. The same shape is a social graph, a road network for Dijkstra, or a supply network for max flow. In tables, that walk is a chain of joins, one join per hop. The graph store keeps the neighbor list with the vertex, which is the representation DFS and BFS already assume. This note does not name a query language or a product API that was not in the fetched algorithm pages.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

### Algorithms for Specific Graph Classes

<details>
<summary>Discuss how algorithms might vary when dealing with Weighted vs Unweighted Graphs.</summary>

### Discuss how algorithms might vary when dealing with Weighted vs Unweighted Graphs.

**Definition:**

An unweighted shortest path counts edges. A weighted shortest path sums edge weights, and the algorithm depends on the sign of those weights.

**How it works:**

BFS is correct for equal edge costs and runs in O(V + E). Dijkstra handles non-negative weights in O((V + E) log V) with a binary heap and is wrong if a weight is negative. Bellman–Ford handles negative weights in O(V·E) and returns −1 when a further relaxation after V − 1 passes finds a negative cycle. Floyd–Warshall handles all pairs, including negative edges and not negative cycles, in O(V³). Kruskal needs weights to sort edges for a minimum spanning tree; an unweighted connected graph’s spanning tree is just any V − 1 edges with no cycle.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Bellman–Ford Algorithm](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
</details>

<details>
<summary>What special considerations are needed when implementing algorithms for Directed vs Undirected Graphs?</summary>

### What special considerations are needed when implementing algorithms for Directed vs Undirected Graphs?

**Definition:**

An undirected edge is stored at both endpoints and is traversed from either side. A directed edge is stored once, from tail to head, and is not automatically reversible.

**How it works:**

DFS says an undirected edge is followed twice and a directed edge at most once, which is why both are still O(V + E). An undirected cycle is a visited neighbor that is not the parent. A directed cycle is a vertex still on the recursion stack; a vertex that was finished earlier is not, by itself, a cycle. Topological order and Kahn’s indegree queue apply to directed graphs and exist only when the processed count equals V. Dijkstra’s sample on GeeksforGeeks inserts each undirected edge in both directions. A one-way road must be inserted only in the legal direction or the distance is not the directed distance. Flow networks are directed: capacity does not imply a reverse edge unless the residual graph adds one to undo flow.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

<details>
<summary>Provide an efficient algorithm for finding Articulation Points in a graph.</summary>

### Provide an efficient algorithm for finding Articulation Points in a graph.

**Definition:**

An articulation point (cut vertex) is a vertex whose removal increases the number of connected components.

**How it works:**

Tarjan’s DFS records a discovery time disc[u] and a low value, the earliest discovery time reachable from the subtree of u, including back edges. For an unvisited child v, recurse and then set low[u] = min(low[u], low[v]). If low[v] ≥ disc[u], u is a cut vertex: v’s subtree has no edge to an ancestor of u. A back edge to a visited non-parent sets low[u] = min(low[u], disc[v]). The naive check runs a DFS after deleting each vertex and is O(V·(V + E)). Tarjan’s algorithm is O(V + E) time and O(V) space. An iterative version on the same page keeps the same disc, low, and parent data on an explicit stack and has the same bounds.

**Sources:**

- [Articulation Points (or Cut Vertices) in a Graph](https://www.geeksforgeeks.org/dsa/articulation-points-or-cut-vertices-in-a-graph/)
</details>

### Graph Challenges and Problem-Solving

<details>
<summary>Solve the Knight’s tour problem using graph traversal techniques.</summary>

### Solve the Knight’s tour problem using graph traversal techniques.

**Definition:**

The knight’s graph has a vertex per square and an edge per legal knight move. A tour is a path that visits every vertex once. The shortest knight route between two squares is a different problem: an unweighted shortest path.

**How it works:**

Build the adjacency list by trying the eight knight deltas from each square and keeping those that stay on the board. A tour search is DFS with backtracking: mark the square visited, recurse to an unused neighbor, and unmark it when the branch fails. Stop when the path length equals the number of squares. That search can explore many branches; the fetched DFS page gives O(V + E) only for a single traversal that does not retry failed branches, so this note does not assign that bound to the full tour search. The minimum number of moves from one square to another does use BFS on this unweighted graph, which the BFS page supports as a shortest-path method in O(V + E). The Floyd–Warshall problem list names that minimum-move knight question separately from a tour.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
</details>

<details>
<summary>How would you design an algorithm to evaluate the connected components of an undirected graph?</summary>

### How would you design an algorithm to evaluate the connected components of an undirected graph?

**Definition:**

A connected component is a maximal set of vertices joined by undirected paths. Disconnected graphs have more than one.

**How it works:**

The DFS and BFS pages handle a disconnected graph the same way: loop over every vertex and, if it is unvisited, start a traversal there. Each start is one component. Collect the vertices visited before the next start. The visited array makes the total time O(V + E) even though the outer loop looks at every vertex. BFS auxiliary space for the queue is O(V). DFS auxiliary space is O(V + E) including the recursion stack.

**Examples:**

```python
def components(adj):
    seen = [False] * len(adj)
    out = []
    for s in range(len(adj)):
        if seen[s]:
            continue
        stack = [s]
        seen[s] = True
        comp = []
        while stack:
            u = stack.pop()
            comp.append(u)
            for v in adj[u]:
                if not seen[v]:
                    seen[v] = True
                    stack.append(v)
        out.append(comp)
    return out
```

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Implement a graph solution for the River Crossing Problem.</summary>

### Implement a graph solution for the River Crossing Problem.

**Definition:**

Each legal arrangement of the actors is a vertex. Each legal one-step move is an unweighted edge. A solution is a path from the start vertex to the goal vertex.

**How it works:**

Encode a state so two states are equal only when every actor is on the same bank. From a state, generate moves that obey the puzzle rules and add an edge to each resulting state. Because every move has cost 1, BFS yields a shortest sequence: it visits states in order of the number of moves, in O(V + E) for the state graph it builds. DFS would also find some path if one exists, but the first path it finds is not the fewest moves. Store parent pointers and walk them back from the goal to list the crossings. The fetched pages do not fix a particular cast of characters or a move count, so this note does not state one.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

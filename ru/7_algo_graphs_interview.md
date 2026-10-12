# Graph Theory — собеседование

Ответы заполнены для каждого вопроса в этой заметке.

[← К обзору](./7_special_algorithms.md)

### Related docs

- [Graph Theory — собеседование](./7_algo_graphs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Graph Theory](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/).

### Introduction to Graphs

<details>
<summary>Что такое graph?</summary>

### Что такое graph?

**Definition:**

Граф — это структура данных, которая представляет набор связанных узлов через множество рёбер.

Эта абстрактная структура очень универсальна и применяется в разных областях, от анализа социальных сетей до компьютерных сетей.

Основные компоненты

Граф состоит из двух основных компонентов:

- **Узлы**: Их также называют вершинами. Это основные единицы, которые хранят данные.
- **Рёбра**: Это связи между узлами, и они могут быть ориентированными или неориентированными.

Визуальное представление

- **Граф**: Unidirected, Directed, Cyclic, Acyclic, Weighted, Unweighted, Sparse, Dense

**How it works:**

Представления графа

Есть несколько способов представить граф в памяти компьютера. Самые распространённые — матрица смежности, список смежности и список рёбер.

Матрица смежности

В матрице смежности двумерный булев массив показывает рёбра между узлами. Значение True в индексе [i][j] означает, что между узлами i и j есть ребро.

Список смежности

Список смежности представляет каждый узел списком, а индексы списка — это узлы. Список каждого узла содержит узлы, с которыми он напрямую связан.

Список рёбер

Список рёбер — это простой список кортежей, где каждый кортеж представляет ребро между двумя узлами.

**Examples:**

Вот код на Python:

```python
graph = [
    [False, True, True],
    [True, False, True],
    [True, True, False]
]
```

Вот код на Python:

```python
graph = {
    0: [1, 2],
    1: [0, 2],
    2: [0, 1]
}
```

Вот код на Python:

```python
graph = [(0, 1), (0, 2), (1, 2)]
```

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Что такое some common Types and CateGories of graphs?</summary>

### Что такое some common Types and CateGories of graphs?

**Definition:**

Графы — гибкие структуры данных для разных вычислительных задач и реальных приложений. Посмотрим на их типы.

**How it works:**

Типы графов

- **Неориентированный**: У рёбер нет направления, между связанными узлами можно идти в обе стороны. Математически ребро `(u, v)` означает также `(v, u)`.
- **Ориентированный (орграф)**: У рёбер есть заданное направление, и обход ему следует. Ребро `(u, v)` не гарантирует `(v, u)`.
- **Типы графов**: Unidirected, Directed

Учёт весов

- **Взвешенный**: У каждого ребра есть числовой «вес» или «стоимость».
- **Невзвешенный**: Все рёбра равны по весу, обычно его считают равным 1.
- **Типы графов**: Weighted, Unweighted

Наличие циклов

- **Циклический**: Содержит хотя бы один цикл или замкнутый путь.
- **Ациклический**: Циклов нет совсем.
- **Типы графов**: Cyclic, Acyclic

Плотность рёбер

- **Плотный**: Высокое отношение числа рёбер к числу вершин, близкое к максимуму возможных связей.
- **Разреженный**: Низкое отношение числа рёбер к числу вершин, ближе к минимуму.
- **Типы графов**: Sparse, Dense

Связность

- **Связный**: Каждая вершина достижима из любой другой.
- **Несвязный**: Некоторые вершины недостижимы из других.
- **Типы графов**: Connected, Disconnected

Уникальность рёбер

- **Мультиграф**: Допускает повторяющиеся рёбра между вершинами.
- **Простой**: Между вершинами допускается только одно связывающее ребро.
- **Типы графов**: Multigraph, Simple Graph

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Что такое the difference between a Tree and a graph?</summary>

### Что такое the difference between a Tree and a graph?

**Definition:**

И графы, и деревья — нелинейные структуры данных, но между ними есть принципиальные различия.

**How it works:**

Ключевые различия

- **Уникальность**: У дерева один корень, а у графа такого понятия может не быть.
- **Топология**: Деревья иерархичны, а графы могут иметь разную структуру.
- **Фокус**: Графы сосредоточены на связях между отдельными узлами, а деревья — на связи узлов с общим корнем.

Графы: универсальные и без жёсткой структуры

- **Элементы**: Состоят из вершин/узлов (обозначаются V) и рёбер (E), которые задают связи. Возможны кратные рёбра и петли.
- **Направленность**: Рёбра могут быть ориентированными или неориентированными.
- **Связность**: Граф может быть несвязным: есть множества вершин, недостижимых из других.
- **Петли**: Могут содержать циклы.

Деревья: иерархичные и упорядоченные

- **Элементы**: Состоят из узлов со связью родитель–потомок.
- **Направленность**: Рёбра строго от родителя к потомку.
- **Связность**: Каждый узел доступен из единственного корня.
- **Петли**: Циклы запрещены.

Визуальное представление

Граф и дерево

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Как вы можете определить минимальное количество ребер, чтобы graph оставался подключенным?</summary>

### Как вы можете определить минимальное количество ребер, чтобы graph оставался подключенным?

**Definition:**

Чтобы граф оставался связным, у него должно быть минимальное число рёбер, которое определяется числом вершин. Это называют рёберной связностью графа.

**How it works:**

Формула рёберной связности

Минимальное число рёбер, нужное, чтобы граф оставался связным, задаётся так:

Рёберная связность

= `max(δ(G), 1)`

Где:

`δ(G)` — минимальная степень вершины в G.

Функция max гарантирует, что граф остаётся связным, даже если у всех вершин степень 1 или 0.

Например, графу с минимальной степенью вершины 3 или больше нужно хотя бы 3 ребра, чтобы остаться связным.

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Определите траекторию Эйлера AND Цепь Эйлера в контексте graph theORy.</summary>

### Определите траекторию Эйлера AND Цепь Эйлера в контексте graph theORy.

**Definition:**

В теории графов путь Эйлера и эйлеров цикл — это способы обойти все рёбра (связи) ровно один раз, с тем отличием, что эйлеров цикл также посещает все вершины один раз.

**How it works:**

Определения пути Эйлера и эйлерова цикла

В графе есть путь Эйлера, если в нём ровно две вершины нечётной степени.

В графе есть эйлеров цикл, если у каждой вершины чётная степень.

Степень — это число рёбер, смежных с вершиной.

Ключевые понятия

- **Начальная вершина**: В пути Эйлера единственные начальная и конечная вершины — это две вершины нечётной степени.
- **Достижимость**: И в пути Эйлера, и в цикле каждое ребро должно быть достижимо из начальной вершины.
- **Согласованность направления**: Путь Эйлера по направлению открыт, а эйлеров цикл по направлению замкнут.

Визуальное представление: путь Эйлера и цикл

Путь Эйлера и эйлеров цикл

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

### Graph Representations

<details>
<summary>Сравните представление adjacency lists AND Маtrieцы смежности fORZ graph.</summary>

### Сравните представление adjacency lists AND Маtrieцы смежности fORZ graph.

**Definition:**

Графы можно представлять по-разному, но матрица смежности и список смежности — самые употребимые структуры. У каждого способа свои плюсы и компромиссы, их и разберём ниже.

**How it works:**

Пример графа

Пространственная сложность

- **Матрица смежности**: Нужна матрица `N × N`, поэтому пространственная сложность `O(N^2)`.
- **Список смежности**: Для соседей каждого узла используется список или массив, поэтому пространственная сложность `O(N + E)`, где E — число рёбер.

Временная сложность поиска ребра

- **Матрица смежности**: Константное время, `O(1)`, потому что наличие ребра читается напрямую.

- **Список смежности**: До `O(k)`, где k — степень вершины, потому что список соседей, возможно, придётся обойти.

Временная сложность обхода

- **Матрица смежности**: Нужно `O(N^2)` времени, чтобы пройти все потенциальные рёбра.
- **Список смежности**: Занимает `O(N + E)` времени и часто быстрее на разреженных графах.

Временная сложность изменения рёбер

- **Матрица смежности**: `O(1)` и на добавление, и на удаление, потому что меняется одна ячейка.
- **Список смежности**: `O(k)` на добавление или удаление, где k — степень затронутой вершины.

Временная сложность изменения вершин

- **Матрица смежности**: `O(N^2)`, потому что матрицу нужно переразмерить.
- **Список смежности**: `O(1)`, потому что обновляется список или массив.

**Examples:**

Пример кода: матрица смежности и список смежности

Вот код на Python:

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
<summary>Что такое Incidence matrix, and when would you use it?</summary>

### Что такое Incidence matrix, and when would you use it?

**Definition:**

Матрица инцидентности — двоичное представление графа, которое сопоставляет вершины рёбрам. Она особенно удобна для ориентированных графов и мультиграфов. В матрице стоят $0$ и $1$, а позиции соответствуют связям «вершина соединена с ребром».

**How it works:**

Структура матрицы

Столбцы: обозначают рёбра

Строки: обозначают вершины

- **Ячейки**: Показывают, соединена ли вершина с ребром
Каждая уникальная пара «строка–ребро» изображает инцидентность вершины ребру и по-разному отражает структуру графа в зависимости от его типа.

Пример: матрица инцидентности ориентированного графа

Ориентированный граф

Пример: матрица инцидентности неориентированного мультиграфа

Uniderected Graph

Применения матриц инцидентности

- **Эффективность алгоритмов**: Некоторые матричные операции быстрее обхода графа.
- **Сравнение графов**: Позволяет напрямую сравнивать граф с матрицей или матрицу с матрицей.
- **Хранение в базе**: Один из способов представить графы в базах данных.
- **Преобразования графов**: Полезно в преобразованиях вроде рёберных и двойственных графов.

**Sources:**

- [Devinterview.io — Graph Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/graph-data-structure-interview-questions/)
</details>

<details>
<summary>Обсудите Edge List как представление graph и варианты его использования.</summary>

### Обсудите Edge List как представление graph и варианты его использования.

**Definition:**

Список рёбер — прямой способ представить граф. Он подходит для плотных графов и даёт быстрый запрос сведений о ребре.

**How it works:**

Ключевые понятия

- **Хранение рёбер**: Список содержит кортежи (a, b), которые обозначают ребро между узлами a и b.
- **Направление ребра**: Рёбра могут быть ориентированными или неориентированными.
- **Повторы рёбер**: Несколько вхождений означают мультиграф. Их отсутствие даёт простой граф.

Наглядный пример

Пример списка рёбер

**Examples:**

Пример кода: список рёбер

Вот код на Python 3:

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
<summary>Объясните, как сэкономить место при хранении файла graph, используя сжатую разреженную строку (CSR).</summary>

### Объясните, как сэкономить место при хранении файла graph, используя сжатую разреженную строку (CSR).

**Definition:**

В формате сжатой разреженной строки граф представляется тремя связанными массивами. Такой сжатый подход заметно снижает расход памяти и особенно полезен для разреженных графов.

Разберём структуры данных и сам процесс.

**How it works:**

Структуры данных

Массив Indptr (IA): список индексов, с которых каждая строка начинается в массиве adj_indices. Его длина равна n_vertices + 1.
Массив индексов смежности (AA): индексы столбцов каждого ребра по их позиции в массиве indptr.
- **Данные рёбер**: Сами данные рёбер. Длина этого массива совпадает с числом ненулевых элементов.

Визуальное представление

Представление графа в CSR

**Examples:**

Пример кода: представление графа в CSR

Вот код на Python:

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
<summary>Объясните метод обхода breadth-first search (BFS).</summary>

### Объясните метод обхода breadth-first search (BFS).

**Definition:**

Поиск в ширину (BFS) — обход графа, который систематически исследует граф уровень за уровнем. Очередь хранит узлы, которые нужно посетить следующими, а список отмечает уже посещённые узлы, чтобы не повторяться.

**How it works:**

Ключевые компоненты

- **Очередь**: Держит узлы в очереди на исследование.
- **Список посещённых**: Записывает узлы, которые уже исследованы.

Шаги алгоритма

- **Инициализация**: Выберите начальный узел, отметьте его посещённым и поставьте в очередь.
- **Исследование**: Повторяйте, пока очередь не пуста. На каждом шаге извлеките узел, посетите его и поставьте в очередь его неисследованных соседей.
- **Завершение**: Остановитесь, когда очередь пуста.

Визуальное представление

Пример BFS

Анализ сложности

Временная сложность: `O(V + E)`, где V — число вершин графа, а E — число рёбер. Каждая вершина и каждое ребро исследуются только один раз.

- **Пространственная сложность**: `O(V)`, потому что в худшем случае в очереди могут оказаться все вершины.

**Examples:**

Пример кода: поиск в ширину

Вот код на Python:

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
<summary>Объясните алгоритм depth-first search (DFS).</summary>

### Объясните алгоритм depth-first search (DFS).

**Definition:**

Поиск в глубину (DFS) — алгоритм обхода графа, который проще и часто быстрее поиска в ширину (BFS). Даже если он не обходит все вершины, DFS остаётся основой множества алгоритмов на графах.

**How it works:**

Шаги алгоритма

- **Инициализация**: Выберите начальную вершину, отметьте её посещённой и положите в стек.

Цикл: пока стек не пуст, делайте следующее:

Снимите верхнюю вершину со стека.
Исследуйте её непосещённых соседей и добавьте их в стек.
- **Финиш**: Когда стек пуст, алгоритм заканчивается, и все достижимые вершины посещены.

Визуальное представление

Пример DFS

**Examples:**

Пример кода: поиск в глубину

Вот код на Python:

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
<summary>Что такое the key differences between BFS and DFS?</summary>

### Что такое the key differences between BFS and DFS?

**Definition:**

BFS и DFS — оба основные алгоритмы обхода графа, и у них разная стратегия, потребность в памяти и сценарии применения.

**How it works:**

Основные различия

- **Стратегия поиска**: BFS идёт уровень за уровнем, а DFS углубляется в каждую ветвь и только потом возвращается.
- **Структуры данных**: BFS использует очередь, а DFS — стек или рекурсию.
- **Пространственная сложность**: BFS требует больше памяти, потому что может хранить целый уровень ($O(|V|)$), тогда как DFS обычно использует меньше (`O(log n)` в среднем).
- **Оптимальность**: BFS гарантирует кратчайший путь; DFS — нет.

Визуальное представление

BFS

Обход BFS

DFS

Обход DFS

**Examples:**

Пример кода: BFS и DFS

Вот код на Python:

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
<summary>Реализуйте a method to check if there is a Path between two vertices in a graph.</summary>

### Реализуйте a method to check if there is a Path between two vertices in a graph.

**Definition:**

Постановка задачи

Дан неориентированный граф. Нужно определить, есть ли путь между двумя заданными вершинами.

Решение

Задачу можно решить поиском в глубину (DFS).

Шаги алгоритма

Начните с исходной вершины.
Для каждой смежной вершины, если она не посещена, рекурсивно выполните DFS.
Если найдена вершина назначения, верните True. Иначе вернитесь назад и исследуйте другие пути.

Анализ сложности

Временная сложность: `O(V + E)`

V — число вершин, а E — число рёбер.

Пространственная сложность: `O(V)`

Для стека рекурсивных вызовов DFS.

Реализация

**Examples:**

Вот код на Python:

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
<summary>Решите the problem of printing all Paths from a source to destination in a Directed graph with BFS or DFS.</summary>

### Решите the problem of printing all Paths from a source to destination in a Directed graph with BFS or DFS.

**Definition:**

Постановка задачи

Дан ориентированный граф и две вершины src и dest. Нужно напечатать все пути из src в dest.

Решение

Рекурсивный поиск в глубину (DFS) на графах: DFS используют, потому что он находит все пути в графе от источника к назначению. Для этого применяется откат, чтобы нашлись все уникальные пути.

Чтобы справиться с циклами, нужен список посещённых узлов. С ним алгоритм не возвращается в уже виденные узлы и не застревает в бесконечном цикле.

Анализ сложности

Временная сложность: `O(V+E)`

V — число вершин, а E — число рёбер.
По сути мы посещаем каждый узел и каждое ребро один раз.

Пространственная сложность: `O(V)`

В худшем случае можно посетить весь граф, и тогда память пропорциональна числу вершин.

Реализация

**Examples:**

Вот код на Python:

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
<summary>Что такое Bipartite graph? How to detect one?</summary>

### Что такое Bipartite graph? How to detect one?

**Definition:**

Двудольный граф — это граф, вершины которого можно разделить на два различных множества, U и V, так что каждое ребро соединяет вершину из U с вершиной из V. Граф обозначают как G = (U, V, E), где E — множество рёбер.

**How it works:**

Как распознать двудольный граф

Проверить, двудольный ли граф, можно несколькими способами:

Поиск в ширину (BFS)

Для этого часто используют BFS. Алгоритм раскрашивает вершины поочерёдно, чтобы у смежных вершин не было одного цвета.

Поиск цикла

Граф не двудольный, если в нём есть цикл нечётной длины. Такие циклы помогают найти алгоритмы вроде DFS или алгоритма Флойда для обнаружения цикла.

**Examples:**

Пример кода: двудольный граф через BFS

Вот код на Python:

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
<summary>Определите Cyclic AND Acyclic graphs AND объясните, как их идентифицировать.</summary>

### Определите Cyclic AND Acyclic graphs AND объясните, как их идентифицировать.

**Definition:**

Циклический граф содержит замкнутый маршрут: путь, который возвращается в вершину, уже лежащую на текущем маршруте. В ациклическом графе такого маршрута нет. Ориентированный ациклический граф — это ориентированный случай, и именно у него есть топологический порядок.

**How it works:**

В ориентированном графе поиск в глубину сообщает о цикле, когда попадает в вершину, которая всё ещё лежит на текущем пути рекурсии. GeeksforGeeks держит `visited[]` для вершин, увиденных хотя бы раз, и `recStack[]` для текущего пути: вершина, посещённая на другой ветке, сама по себе не цикл. Когда соседи `u` закончены, `recStack[u]` снимают. Вторая проверка на той же странице — алгоритм Кана: посчитать входящие степени, поставить в очередь вершины со степенью 0 и уменьшать степени соседей. Если обработанных вершин не `V`, часть вершин осталась и в графе есть цикл. Обе проверки — время O(V + E) и O(V) дополнительной памяти. Цикл в неориентированном графе проверяют иначе: BFS умеет его найти, а ребро обхода в уже посещённую вершину — цикл только если эта вершина не родитель, из которого поиск только что пришёл.

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
<summary>Объясните Planar graphs и условия, при которых graph становится плоским.</summary>

### Объясните Planar graphs и условия, при которых graph становится плоским.

**Definition:**

Планарный граф можно нарисовать на плоскости так, чтобы рёбра пересекались только в концах. Такой рисунок — плоский граф, или планарное вложение.

**How it works:**

Теорема Куратовского: конечный граф планарен тогда и только тогда, когда в нём нет подграфа, который является подразбиением K5 или K3,3. Теорема Вагнера говорит то же через миноры, а не подразбиения. У простого связного планарного графа с v ≥ 3 выполняется e ≤ 3v − 6; если нет циклов длины 3, то e ≤ 2v − 4. Эти неравенства необходимы, но не достаточны: ими можно доказать непланарность, но не планарность. Формула Эйлера для конечного связного плоского графа: v − e + f = 2, внешняя грань считается. Планарность графа на n вершинах проверяется за время O(n). Так как e есть O(v), планарные графы разрежены рядом с максимумом O(v²) рёбер.

**Sources:**

- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

<details>
<summary>Что такое dense и sparse graphs? Обсудите меры плотности и типичные use cases.</summary>

### Что такое dense и sparse graphs? Обсудите меры плотности и типичные use cases.

**Definition:**

В плотном графе рёбер намного больше, чем вершин. В разреженном рёбер очень мало относительно числа вершин.

**How it works:**

Флойд–Уоршелл всегда работает за O(V³), три вложенных цикла, сколько бы ни было рёбер, поэтому GeeksforGeeks рекомендует его для плотных графов, а алгоритм Джонсона — для разреженных. Простой связный планарный граф разрежен в том же смысле: у него O(v) рёбер, а у максимального простого графа O(v²). Матрица смежности хранит таблицу V×V и подходит плотному обновлению всех пар. Список смежности, как в реализациях DFS и BFS, хранит каждое ребро один раз (дважды, если граф неориентированный) и подходит разреженному обходу за O(V + E).

**When to use:**

Матрицу и Флойда–Уоршелла берут, когда ребро есть почти у каждой пары. Список смежности и обход или Дейкстру берут, когда E гораздо ближе к V, чем к V².

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

### Graph Algorithms and Complexity

<details>
<summary>Почему the complexity of DFS is O(V+E)?</summary>

### Почему the complexity of DFS is O(V+E)?

**Definition:**

Поиск в глубину по списку смежности — это O(V + E), потому что каждую вершину посещают не более одного раза, а каждое ребро проходят константное число раз.

**How it works:**

GeeksforGeeks прямо даёт оценку: каждую вершину посещают не более одного раза, а каждое ребро проходят не более одного раза в ориентированном графе и дважды в неориентированном (по записи с каждого конца списка смежности). Несвязный граф оценку не меняет: внешний цикл начинает поиск в каждой непосещённой вершине, а массив visited всё равно обрабатывает каждую вершину и каждое ребро один раз. Дополнительная память, которую они указывают, — O(V + E): массив visited размера V плюс стек рекурсии, поверх списка смежности, который уже хранит рёбра.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Приведите an implementation for topological sort in a Directed Acyclic graph (DAG).</summary>

### Приведите an implementation for topological sort in a Directed Acyclic graph (DAG).

**Definition:**

Топологический порядок — это последовательность всех вершин, в которой каждое ориентированное ребро идёт от более ранней вершины к более поздней. Он существует только у ациклического ориентированного графа.

**How it works:**

Алгоритм Кана на странице поиска цикла — это топологическая сортировка через BFS. Посчитайте входящие степени. Поставьте в очередь каждую вершину со степенью 0. Снимите вершину, допишите её в порядок и уменьшите входящую степень каждого соседа; соседа ставят в очередь, когда степень становится 0. Если в порядке V вершин, граф — DAG, и порядок топологический. Если обработано меньше V вершин, цикл остановил остальные. Время O(V + E), дополнительная память O(V).

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
<summary>Напишите an alGorithm to detect a Cycle in an Undirected Graph.</summary>

### Напишите an alGorithm to detect a Cycle in an Undirected Graph.

**Definition:**

В неориентированном графе цикл есть, когда обход попадает в посещённую вершину, которая не является родителем, из которого только что пришли.

**How it works:**

И BFS, и DFS идут по списку смежности и помечают вершины посещёнными. В неориентированном графе каждое ребро записано с обоих концов, поэтому сосед, из которого только что пришли, уже посещён и это не цикл. Любой другой посещённый сосед — обратное ребро и означает цикл. GeeksforGeeks относит поиск цикла к применениям BFS и даёт BFS время O(V + E) и O(V) дополнительной памяти на очередь. DFS на том же представлении тоже O(V + E). Цикл в ориентированном графе проверяют иначе (стек рекурсии): поперечное ребро в уже законченную вершину циклом не является.

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
<summary>Обсудите сложность поиска всех пар shortest path во взвешенном graph.</summary>

### Обсудите сложность поиска всех пар shortest path во взвешенном graph.

**Definition:**

Кратчайшие пути между всеми парами заполняют таблицу расстояний V×V. Флойд–Уоршелл делает это за время O(V³).

**How it works:**

Алгоритм начинает с матрицы прямых рёбер (отсутствующие рёбра — большое INF, диагональ — 0) и пробует каждую вершину k как промежуточную: dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]). Три вложенных цикла размера V дают O(V³). Дополнительная память сверх матрицы, которую обновляют на месте, — O(1). Отрицательные рёбра допустимы, отрицательные циклы — нет. На той же странице кубическая оценка не зависит от E, поэтому она подходит плотным графам; для разреженных они называют алгоритм Джонсона. Запуск Дейкстры с двоичной кучей из каждой вершины — другой способ для всех пар при неотрицательных весах: один запуск на той странице стоит O((V + E) log V).

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

### Memory and Performance in Graph Operations

<details>
<summary>Почему does breadth-first search use more memory than depth-first search?</summary>

### Почему does breadth-first search use more memory than depth-first search?

**Definition:**

Опубликованные оценки дополнительной памяти не говорят, что BFS всегда ест больше. BFS оценивают как O(V) из-за очереди. DFS оценивают как O(V + E) из-за массива visited и стека рекурсии.

**How it works:**

Оба поиска помечают вершины, чтобы цикл не обходился бесконечно. BFS держит очередь фронта и идёт по уровням; GeeksforGeeks указывает дополнительную память O(V) на эту очередь. DFS уходит вглубь одного пути и возвращается; тот же сайт указывает дополнительную память O(V + E) из-за массива visited размера V и рекурсивных вызовов, поверх списка смежности. Очередь и стек вызовов имеют разную форму: очередь держит текущий уровень, стек вызовов — текущий путь. Что больше, зависит от графа (широкий уровень или длинный путь). Цитируйте цифры O(V) и O(V + E) с этих страниц, а не одного победителя.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Проиллюстрируйте the difference in Peak Memory Consumption between DFS and BFS.</summary>

### Проиллюстрируйте the difference in Peak Memory Consumption between DFS and BFS.

**Definition:**

Пик дополнительной памяти — это самый большой живой фронт. У BFS с GeeksforGeeks это очередь, дополнительная память O(V). У их рекурсивного DFS это стек вызовов плюс массив visited, дополнительная память O(V + E).

**How it works:**

В каждый момент BFS хранит в очереди все вершины текущего уровня, затем раскрывает следующий. DFS хранит один путь: каждый рекурсивный вызов — вершина, из которой ещё не вернулись. Звезда, обойденная из центра, заставляет очередь BFS держать все листья сразу. Один длинный путь заставляет стек DFS держать все вершины, а очередь BFS остаётся размера 1. Оба всё равно выделяют массив visited длины V. Список смежности — общий вход, не часть фронта. Не добавляйте более тесный пик, чем O(V) для очереди или цифра дополнительной памяти O(V + E) со страницы DFS.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Как можно оптимизировать структуры данных graph для лучшей локализации кэша?</summary>

### Как можно оптимизировать структуры данных graph для лучшей локализации кэша?

**Definition:**

Цена обхода следует из того, как хранят соседей. Матрица смежности — одна непрерывная таблица V×V. Список смежности — массив списков соседей, и именно его используют страницы DFS, BFS и Дейкстры.

**How it works:**

Просмотр строки матрицы читает соседние ячейки и совпадает с плотным графом, где Флойд–Уоршелл и так касается каждой пары за O(V³). Список хранит только существующие рёбра, поэтому разреженный граф не выделяет V² ячеек; номера соседей могут лежать в отдельных маленьких массивах, и переход к ним прыгает. Для разреженного графа список всё равно выигрывает по памяти и по проходам O(V + E). Для плотного графа матрица не заводит указатель на каждое ребро, и именно её Флойд–Уоршелл обновляет на месте. Упаковка массива соседей каждой вершины в один непрерывный буфер сохраняет оценку списка O(V + E) и делает соседей одной вершины соседними в памяти. Эта упаковка — раскладка того же списка смежности; асимптотику со страниц она не меняет.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Обсудите методы сокращения использования памяти в представлениях graph.</summary>

### Обсудите методы сокращения использования памяти в представлениях graph.

**Definition:**

Храните каждое ребро столько раз, сколько нужно алгоритму, и не выделяйте таблицу V×V, если алгоритм её не заполняет.

**How it works:**

Список смежности хранит существующие рёбра. DFS и BFS используют эту форму и оценивают обход как O(V + E), а не O(V²). Неориентированный список хранит каждое ребро дважды, по разу у каждого конца, поэтому DFS говорит, что неориентированное ребро проходят дважды. Ориентированный список хранит его один раз. Алгоритм Краскала сортирует список рёбер и использует систему непересекающихся множеств размера V; его дополнительная память O(E + V), и матрицу он не строит. Флойду–Уоршеллу нужна матрица расстояний V×V, поэтому её избегают на разреженном графе; страница отправляет разреженные пути между всеми парами к алгоритму Джонсона. У планарных графов только O(v) рёбер, поэтому список линеен по v, а матрица квадратична.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

### Shortest Path Algorithms

<details>
<summary>Что такое the difference between BFS and Dijkstra’s alGorithms when looking for the shortest path?</summary>

### Что такое the difference between BFS and Dijkstra’s alGorithms when looking for the shortest path?

**Definition:**

BFS находит кратчайший путь, когда у всех рёбер одна цена, потому что посещает вершины в порядке числа рёбер. Дейкстра находит кратчайший путь, когда у рёбер неотрицательные веса.

**How it works:**

BFS использует очередь FIFO и раскрывает граф по уровням. GeeksforGeeks относит кратчайшие пути в невзвешенном графе к применениям BFS, время O(V + E). Дейкстра держит массив расстояний, вначале бесконечность кроме 0 у источника, и очередь с приоритетом пар (расстояние, вершина). Она снимает наименьшее расстояние, пропускает устаревшую запись, если снятое расстояние больше dist[u], и ослабляет dist[v] = dist[u] + w. С двоичной кучей страница даёт время O((V + E) log V) и дополнительную память O(V + E). Шаг «расстояние окончательно» верен только при неотрицательных весах: позднее отрицательное ребро может уменьшить расстояние, которое Дейкстра уже считала финальным.

**When to use:**

BFS берут для числа рёбер. Дейкстру — когда веса неотрицательны и не все равны. Беллмана–Форда — если вес может быть отрицательным.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Реализуйте Dijkstra’s AlGorithm for finding the shortest path in a graph with non-neGAtive edge weights.</summary>

### Реализуйте Dijkstra’s AlGorithm for finding the shortest path in a graph with non-neGAtive edge weights.

**Definition:**

Дейкстра считает кратчайшие расстояния от одного источника во взвешенном графе без отрицательных рёбер.

**How it works:**

Инициализируйте dist[] бесконечностью и dist[src] нулём. Положите (0, src) в мин-кучу. Пока куча не пуста, снимите (d, u). Если d > dist[u], запись устарела и её пропускают. Для каждого соседа (v, w), если dist[u] + w < dist[v], запишите dist[v] и положите (dist[v], v). Почему можно считать u окончательной в момент снятия: при неотрицательных весах любой другой маршрут пройдёт через вершину с расстоянием не меньше dist[u] и не будет короче. Время O((V + E) log V). Дополнительная память O(V + E).

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
<summary>Чем алгоритм Bellman-Ford отличается от алгоритма Dijkstra и где бы вы его использовали?</summary>

### Чем алгоритм Bellman-Ford отличается от алгоритма Dijkstra и где бы вы его использовали?

**Definition:**

Беллман–Форд — алгоритм кратчайших путей из одной вершины, который допускает отрицательные рёбра и сообщает об отрицательном цикле. Дейкстра — нет.

**How it works:**

Ослабьте каждое ребро V − 1 раз: если dist[u] конечно и dist[u] + w < dist[v], запишите dist[v] = dist[u] + w. У простого пути не больше V − 1 рёбер, поэтому столько полных проходов покрывают каждый простой путь от источника. Ещё одно успешное ослабление означает отрицательный цикл, и реализация на странице возвращает −1. Недостижимые вершины остаются на стороже 10^8. Указанная оценка — время O(V·E) и память O(V). Фиксация вершины в очереди Дейкстры ломается на отрицательном ребре, потому что она не возвращается к уже снятой вершине. Беллмана–Форда берут, когда вес может быть отрицательным или нужно найти отрицательный цикл. Дейкстру — когда все веса неотрицательны и нужна оценка кучи O((V + E) log V).

**Sources:**

- [Bellman–Ford Algorithm](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
</details>

<details>
<summary>Объясните концепцию Floyd-Warshall algorithm и ее использование.</summary>

### Объясните концепцию Floyd-Warshall algorithm и ее использование.

**Definition:**

Флойд–Уоршелл считает кратчайшие расстояния между каждой парой, пробуя каждую вершину как промежуточную.

**How it works:**

dist[i][j] вначале — прямой вес, INF если ребра нет, и 0 на диагонали. Для k от 0 до V − 1 и для каждой пары (i, j) замените dist[i][j] на dist[i][k] + dist[k][j], если сумма меньше и ни одна часть не INF. Утверждение об оптимальной подструктуре на странице: если кратчайший путь i–j идёт через k, куски i–k и k–j сами кратчайшие. Алгоритм работает на ориентированных и неориентированных взвешенных графах, включая отрицательные рёбра, и не работает на отрицательном цикле. Время O(V³). Дополнительная память при обновлении на месте — O(1). Страница применяет его к маршрутизации в сети, связности аэропортов и дорожным сетям GIS и предпочитает его на плотных графах.

**Sources:**

- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
</details>

### Heuristic-Based Graph Search Algorithms

<details>
<summary>Что такое `A*` Search?</summary>

### Что такое `A*` Search?

**Definition:**

A* выбирает следующую вершину с наименьшим f, где f = g + h: g — цена от старта по уже построенному пути, h — оценка оставшейся цены до цели.

**How it works:**

GeeksforGeeks показывает его на сетке с препятствиями. На каждом шаге раскрывают открытую клетку с наименьшим f. g известна, потому что идёт по уже построенному пути. h — догадка, потому что стены могут перекрыть прямой маршрут. Там же сказано, что игры и веб-карты используют его, чтобы найти короткий путь. Страница допустимости добавляет: если h(n) никогда не больше истинной оставшейся цены h*(n) и h(goal) = 0, A* возвращает оптимальный путь. Согласованная эвристика ещё удовлетворяет h(n) ≤ c(n, p) + h(p), поэтому f не убывает вдоль пути и закрытую вершину не нужно открывать снова.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>Что такое the heuristic cost function in `A*` Search?</summary>

### Что такое the heuristic cost function in `A*` Search?

**Definition:**

Эвристика h(n) оценивает самую дешёвую цену от n до цели. Это не уже заплаченная цена; та часть — g(n).

**How it works:**

На сетке GeeksforGeeks называет три оценки: манхэттенское расстояние (сумма модулей разностей x и y), когда ходы только по горизонтали и вертикали; евклидово (по прямой), когда двигаться можно в любую сторону; расстояние Чебышёва, когда диагональный шаг стоит столько же, сколько ортогональный. Допустимая значит h(n) ≤ h*(n) для каждого n, и h(goal) = 0. Завышение может заставить A* раскрыть вершину с обманчиво малым f и пропустить оптимальный путь. Согласованная (монотонная) значит h(n) ≤ c(n, p) + h(p) для каждого соседа p.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

<details>
<summary>Объясните the difference between Best-First Search and `A*` Search.</summary>

### Объясните the difference between Best-First Search and `A*` Search.

**Definition:**

Жадный поиск по лучшему сначала упорядочивает вершины только по эвристике h. A* упорядочивает их по f = g + h, поэтому уже потраченная цена тоже считается.

**How it works:**

Поиск по лучшему раскрывает вершину, которая выглядит ближе к цели. Он может уйти к цели и проигнорировать более длинный на вид старт, который на самом деле дешевле, потому что g не входит в ключ. A* не предпочтёт ту вершину, когда g добавлена, а с допустимой h он оптимален. Обоим всё равно нужен способ не раскрывать вершину повторно; A* с согласованной эвристикой обрабатывает каждую вершину не больше одного раза. Жадный поиск берут, когда достаточно быстрого правдоподобного пути. A* — когда путь обязан быть оптимальным по ценам рёбер.

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
<summary>Приведите an implementation of the greedy Best-First Search and compare it to `A*`.</summary>

### Приведите an implementation of the greedy Best-First Search and compare it to `A*`.

**Definition:**

Жадный поиск по лучшему раскрывает открытую вершину с наименьшим h(n). A* раскрывает открытую вершину с наименьшим g(n) + h(n).

**How it works:**

Держите мин-кучу пар (h, вершина) и множество посещённых. Снимите наименьшее h и, если это цель, остановитесь. Иначе положите каждого непосещённого соседа с его собственным h. Путь восстанавливают указателями на родителя. По сравнению с A* из ключа кучи убрана g, поэтому сосед в одном шаге от цели, до которого добрались дорогим путём, предпочтительнее соседа в двух шагах, до которого добрались дёшево. A* добавляет g и при допустимой h такой обмен не делает. Сеточные эвристики со страницы A* (манхэттенская, евклидова, Чебышёва) — те же функции, которые можно передать как h.

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
<summary>Как you adapt `A*` Search to work on a puzzle GAme like Sudoku?</summary>

### Как you adapt `A*` Search to work on a puzzle GAme like Sudoku?

**Definition:**

Смоделируйте состояния головоломки как вершины, а допустимые ходы как рёбра, и пусть A* раскрывает состояние с наименьшим f = g + h.

**How it works:**

g(n) — цена уже сделанных ходов, например число заполненных клеток или число шагов. h(n) должна оценивать оставшуюся работу и для оптимального решения не превышать истинную оставшуюся цену. На страницах A* манхэттенская, евклидова и Чебышёва — оценки для сетки; доска судоку — не та сетка, поэтому эти три расстояния не являются эвристикой. Эвристику судоку считают по доске, например как число пустых клеток (каждой пустой клетке нужна хотя бы одна запись, поэтому счётчик не превышает истинное число записей). Поиск хранит указатели на родителя и останавливается на состоянии, которое удовлетворяет правилам. A* хранит каждое открытое состояние, и страница эвристики называет этот расход памяти причиной, по которой алгоритм становится непрактичным на очень большом графе.

**Sources:**

- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [Admissibility of A* Algorithm](https://www.geeksforgeeks.org/dsa/a-is-admissible/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
</details>

### Real-World Applications of Graphs

<details>
<summary>Что такое some real-world Applications of graphs?</summary>

### Что такое some real-world Applications of graphs?

**Definition:**

Графы моделируют объекты вершинами, а связи рёбрами, после чего обход или алгоритм кратчайшего пути отвечает на вопрос по этой модели.

**How it works:**

BFS применяют для кратчайших путей в невзвешенной сети, поиска цикла, компонент связности и маршрутизации. Дейкстру и A* — когда у рёбер неотрицательная цена; страница A* называет игры и веб-карты. Флойда–Уоршелла — для маршрутов между всеми парами, авиасвязности и дорожных сетей GIS. Алгоритм Краскала соединяет все вершины с минимальным суммарным весом. Максимальный поток отправляет столько, сколько позволяют пропускные способности, от источника к стоку: введение перечисляет маршрутизацию связи, транспорт, водопровод, паросочетание в двудольном графе, назначение работ и цепочки поставок. Рёберная раскраска составляет расписание инцидентных событий, которые не могут делить вершину, включая назначение частот в оптоволокне.

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
<summary>Приведите some practical examples of using depth-first search vs breadth-first search.</summary>

### Приведите some practical examples of using depth-first search vs breadth-first search.

**Definition:**

DFS заканчивает один путь, прежде чем пробовать следующий. BFS заканчивает целый уровень, прежде чем идти глубже.

**How it works:**

DFS берут, чтобы обойти каждую вершину компоненты, в том числе несвязный граф, начиная заново в каждой непосещённой вершине, или чтобы найти ориентированный цикл со стеком рекурсии. Его время O(V + E). BFS берут, когда ответ — наименьшее число рёбер: невзвешенный кратчайший путь, компонента связности, выращенная по уровням, или проверка цикла, которую страница BFS тоже перечисляет. Время BFS — O(V + E), очередь — дополнительная память O(V). Лабиринт, который нужно пройти за наименьшее число комнат, — это BFS. Исчерпать один коридор, прежде чем вернуться к развилке, — это DFS.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Как социальные сети используют graphs для получения рекомендаций и функций, таких как предложения в друзья?</summary>

### Как социальные сети используют graphs для получения рекомендаций и функций, таких как предложения в друзья?

**Definition:**

Люди — вершины, дружба — рёбра. Подсказка — близкая вершина, которая ещё не сосед.

**How it works:**

BFS от человека посещает друзей, затем друзей друзей, в порядке числа рёбер. Это применение невзвешенного кратчайшего пути со страницы BFS, поэтому «люди на расстоянии 2» — ровно второй уровень. Вершины, уже смежные со стартом, — текущие друзья, и это не подсказки. Тот же обход по уровням находит компонентой связности всех, кто достижим от человека. Эта заметка не добавляет формулу ранжирования, которой не было на загруженных страницах; факт графа — расстояние в графе дружбы.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Объясните, как навигационная система GPS NAVIGA может использовать структуры данных и алгоритмы graph.</summary>

### Объясните, как навигационная система GPS NAVIGA может использовать структуры данных и алгоритмы graph.

**Definition:**

Перекрёстки — вершины, участки дорог — взвешенные рёбра. Маршрут — кратчайший путь по этим весам.

**How it works:**

Если у каждого участка неотрицательная цена (длина или время), Дейкстра от старта считает dist[] очередью с приоритетом за O((V + E) log V). A* делает тот же поиск, но упорядочивает раскрытие по g + h. Страница A* говорит, что веб-карты его используют, а допустимая h никогда не превышает истинную оставшуюся цену; расстояние по прямой (евклидово) — пример допустимой оценки на странице эвристики, когда движение не ограничено осями сетки. BFS был бы верен, только если у всех участков одна цена. Флойд–Уоршелл — альтернатива для всех пар, которую пункт GIS на той странице называет, за O(V³); она подходит плотной таблице расстояний, а не одному запросу.

**Sources:**

- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [A* Search Algorithm](https://www.geeksforgeeks.org/dsa/a-search-algorithm/)
- [A* algorithm and its Heuristic Search Strategy](https://www.geeksforgeeks.org/artificial-intelligence/a-algorithm-and-its-heuristic-search-strategy-in-artificial-intelligence/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

### Advanced Graph Topics

<details>
<summary>Что такое graph Embedding, and how is it applicable in machine learning?</summary>

### Что такое graph Embedding, and how is it applicable in machine learning?

**Definition:**

На странице планарных графов вложение графа — это рисунок графа на поверхности, где рёбра встречаются только в вершинах. Плоскость и сфера имеют род 0, это планарный случай.

**How it works:**

Планарное вложение сопоставляет каждой вершине точку, а каждому ребру кривую, концы которой — эти точки, и кривые не пересекаются кроме концов. Каждый плоский рисунок соответствует сферическому стереографической проекцией. Это топологическое вложение, а не таблица векторов признаков. Загруженная страница не определяет отображение машинного обучения из вершин в числовые векторы, поэтому эта заметка не называет такой алгоритм или функцию потерь. Что страница говорит для смежного с обучением применения, косвенно: планарные графы разрежены (O(v) рёбер), а рисунки — объект, который проверка планарности классифицирует за время O(n).

**Sources:**

- [Planar graph](https://en.wikipedia.org/wiki/Planar_graph)
</details>

<details>
<summary>Описать теорему о минимальном сокращении и максимальном потоке и ее значение в задачах сетевых потоков.</summary>

### Описать теорему о минимальном сокращении и максимальном потоке и ее значение в задачах сетевых потоков.

**Definition:**

Теорема о максимальном потоке и минимальном разрезе говорит, что максимальный поток от источника к стоку равен пропускной способности минимального разреза: наименьшей суммарной пропускной способности рёбер, удаление которых отсоединяет источник от стока.

**How it works:**

Сеть потока — конечный ориентированный граф с источником, стоком и неотрицательной пропускной способностью каждого ребра. Поток по ребру не может превысить пропускную способность, а в каждой вершине кроме источника и стока входящий поток равен исходящему. Увеличивающий путь — путь от источника к стоку, у которого остаточная пропускная способность (пропускная способность минус текущий поток) положительна на каждом ребре; его узкое место — минимум остатка на этом пути. Пример с трубами в Wikipedia и есть смысл: наименьший набор труб, который изолирует водохранилище от города, задаёт максимум воды, которая может прибыть. Введение называет Форда–Фалкерсона, алгоритм Диница и push-relabel как способы посчитать поток. Та же величина — частный случай двойственности линейного программирования, и из неё выводят теорему Менгера и теорему Кёнига–Эгервари.

**Sources:**

- [Max-flow min-cut theorem](https://en.wikipedia.org/wiki/Max-flow_min-cut_theorem)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

<details>
<summary>Обсудите концепцию раскраски краев и ее применение.</summary>

### Обсудите концепцию раскраски краев и ее применение.

**Definition:**

Правильная рёберная раскраска назначает рёбрам цвета так, чтобы никакие два ребра с общей вершиной не получили один цвет. Хроматический индекс — наименьшее число цветов, которого хватает.

**How it works:**

Каждый цветовой класс — паросочетание: рёбра одного цвета не делят вершину. По теореме Визинга простому графу нужно либо Δ, либо Δ + 1 цветов, где Δ — максимальная степень. Двудольным графам и планарным графам высокой степени хватает Δ. Мультиграфам может понадобиться до 3Δ/2. Оптимальную раскраску двудольного графа и раскраску недвудольного простого графа не более чем Δ + 1 цветами можно построить за полиномиальное время. Найти оптимальную раскраску в общем случае NP-трудно. Чётному циклу нужно 2 цвета, нечётному — 3. Применения на странице — расписания и назначение частот в оптоволоконных сетях.

**Sources:**

- [Edge coloring](https://en.wikipedia.org/wiki/Edge_coloring)
</details>

<details>
<summary>Реализуйте an alGorithm that finds the minimum spanning tree of a graph.</summary>

### Реализуйте an alGorithm that finds the minimum spanning tree of a graph.

**Definition:**

Минимальное остовное дерево связного неориентированного взвешенного графа — подмножество рёбер без цикла, соединяющее все вершины, с минимальным суммарным весом. В нём V − 1 рёбер.

**How it works:**

Краскал сортирует рёбра по неубыванию веса. Система непересекающихся множеств следит за компонентами. Для каждого ребра в этом порядке, если концы в разных множествах, их объединяют и ребро оставляют; если в одном множестве, ребро замкнуло бы цикл и его отбрасывают. Остановка на V − 1 оставленных рёбрах. Сортировка — O(E log E). Каждый find/union не больше O(log V), поэтому общая оценка страницы — O(E log E) или O(E log V), потому что E не больше O(V²) и логарифмы совпадают. Дополнительная память O(E + V).

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
<summary>Объясните важность баз данных graph и их отличие от традиционных реляционных баз данных.</summary>

### Объясните важность баз данных graph и их отличие от традиционных реляционных баз данных.

**Definition:**

Графовая база хранит сущности вершинами, а связи рёбрами, поэтому вопрос о соединениях — это обход. Реляционная база хранит строки и восстанавливает связь сопоставлением ключей.

**How it works:**

Обход друзей друзей — это порядок уровней BFS по списку смежности: время следует O(V + E) для задетых вершин и рёбер, а не матрице V×V. Та же форма — социальный граф, дорожная сеть для Дейкстры или сеть поставок для максимального потока. В таблицах этот обход — цепочка соединений, по соединению на шаг. Графовое хранилище держит список соседей вместе с вершиной, а это представление, которое DFS и BFS уже предполагают. Эта заметка не называет язык запросов или API продукта, которых не было на загруженных страницах алгоритмов.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

### Algorithms for Specific Graph Classes

<details>
<summary>Обсудите, как могут различаться алгоритмы при работе с взвешенным и невзвешенным graphs.</summary>

### Обсудите, как могут различаться алгоритмы при работе с взвешенным и невзвешенным graphs.

**Definition:**

Невзвешенный кратчайший путь считает рёбра. Взвешенный суммирует веса рёбер, и алгоритм зависит от знака этих весов.

**How it works:**

BFS верен при равной цене рёбер и работает за O(V + E). Дейкстра обрабатывает неотрицательные веса за O((V + E) log V) с двоичной кучей и неверна, если вес отрицателен. Беллман–Форд обрабатывает отрицательные веса за O(V·E) и возвращает −1, когда ещё одно ослабление после V − 1 проходов находит отрицательный цикл. Флойд–Уоршелл обрабатывает все пары, включая отрицательные рёбра и не отрицательные циклы, за O(V³). Краскалу нужны веса, чтобы сортировать рёбра для минимального остова; остов связного невзвешенного графа — это просто любые V − 1 рёбер без цикла.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Bellman–Ford Algorithm](https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [Kruskal’s Minimum Spanning Tree Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
</details>

<details>
<summary>Какие особые соображения нужны при реализации алгоритмов для directed vs undirected graphs?</summary>

### Какие особые соображения нужны при реализации алгоритмов для directed vs undirected graphs?

**Definition:**

Неориентированное ребро хранят у обоих концов и проходят с любой стороны. Ориентированное хранят один раз, от хвоста к голове, и оно не обратимо само.

**How it works:**

DFS говорит, что неориентированное ребро проходят дважды, а ориентированное не больше одного раза, поэтому оба случая всё ещё O(V + E). Неориентированный цикл — это посещённый сосед, который не родитель. Ориентированный цикл — вершина, всё ещё лежащая в стеке рекурсии; вершина, которую уже закончили раньше, сама по себе не цикл. Топологический порядок и очередь входящих степеней Кана относятся к ориентированным графам и существуют, только когда число обработанных вершин равно V. Пример Дейкстры на GeeksforGeeks вставляет каждое неориентированное ребро в обе стороны. Дорогу в одну сторону нужно вставить только в разрешённом направлении, иначе расстояние — не ориентированное. Сети потока ориентированы: пропускная способность не подразумевает обратное ребро, пока остаточный граф не добавит его, чтобы отменить поток.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Dijkstra's Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
- [Max Flow Problem Introduction](https://www.geeksforgeeks.org/dsa/max-flow-problem-introduction/)
</details>

<details>
<summary>Приведите an efficient alGorithm for finding Articulation Points in a graph.</summary>

### Приведите an efficient alGorithm for finding Articulation Points in a graph.

**Definition:**

Точка сочленения — вершина, удаление которой увеличивает число компонент связности.

**How it works:**

DFS Таржана записывает время открытия disc[u] и значение low — самое раннее время открытия, достижимое из поддерева u, включая обратные рёбра. Для непосещённого ребёнка v делают рекурсию и затем low[u] = min(low[u], low[v]). Если low[v] ≥ disc[u], u — точка сочленения: у поддерева v нет ребра к предку u. Обратное ребро к посещённому не-родителю ставит low[u] = min(low[u], disc[v]). Наивная проверка запускает DFS после удаления каждой вершины и стоит O(V·(V + E)). Алгоритм Таржана — время O(V + E) и память O(V). Итеративная версия на той же странице держит те же disc, low и родителя на явном стеке и имеет те же оценки.

**Sources:**

- [Articulation Points (or Cut Vertices) in a Graph](https://www.geeksforgeeks.org/dsa/articulation-points-or-cut-vertices-in-a-graph/)
</details>

### Graph Challenges and Problem-Solving

<details>
<summary>Решите the Knight’s tour problem using graph traveRSAl techniques.</summary>

### Решите the Knight’s tour problem using graph traveRSAl techniques.

**Definition:**

Граф коня имеет вершину на клетку и ребро на допустимый ход коня. Тур — путь, который посещает каждую вершину один раз. Кратчайший маршрут коня между двумя клетками — другая задача: невзвешенный кратчайший путь.

**How it works:**

Постройте список смежности, пробуя восемь смещений коня из каждой клетки и оставляя те, что остаются на доске. Поиск тура — DFS с возвратом: пометьте клетку посещённой, уйдите в неиспользованного соседа и снимите пометку, когда ветка не удалась. Остановка, когда длина пути равна числу клеток. Этот поиск может разобрать много веток; страница DFS даёт O(V + E) только для одного обхода, который не переигрывает неудачные ветки, поэтому эта заметка не назначает эту оценку полному поиску тура. Минимальное число ходов от одной клетки до другой использует BFS на этом невзвешенном графе, и страница BFS поддерживает его как метод кратчайшего пути за O(V + E). Список задач Флойда–Уоршелла называет вопрос о минимальном числе ходов коня отдельно от тура.

**Sources:**

- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
</details>

<details>
<summary>Как бы вы design an alGorithm to evaluate the connected components of an Undirected Graph?</summary>

### Как бы вы design an alGorithm to evaluate the connected components of an Undirected Graph?

**Definition:**

Компонента связности — максимальное множество вершин, соединённых неориентированными путями. У несвязных графов их больше одной.

**How it works:**

Страницы DFS и BFS обрабатывают несвязный граф одинаково: цикл по каждой вершине и, если она не посещена, обход от неё. Каждый старт — одна компонента. Соберите вершины, посещённые до следующего старта. Массив visited делает полное время O(V + E), хотя внешний цикл смотрит на каждую вершину. Дополнительная память BFS на очередь — O(V). Дополнительная память DFS — O(V + E), включая стек рекурсии.

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
<summary>Реализуйте a graph solution for the River Crossing Problem.</summary>

### Реализуйте a graph solution for the River Crossing Problem.

**Definition:**

Каждое допустимое расположение участников — вершина. Каждый допустимый ход на один шаг — невзвешенное ребро. Решение — путь от начальной вершины к целевой.

**How it works:**

Закодируйте состояние так, чтобы два состояния были равны, только когда каждый участник на том же берегу. Из состояния порождайте ходы, которые соблюдают правила головоломки, и добавляйте ребро в каждое полученное состояние. Поскольку каждый ход имеет цену 1, BFS даёт кратчайшую последовательность: он посещает состояния в порядке числа ходов, за O(V + E) для графа состояний, который он строит. DFS тоже найдёт какой-нибудь путь, если он есть, но первый найденный путь — не наименьшее число ходов. Храните указатели на родителя и пройдите их от цели назад, чтобы перечислить переправы. Загруженные страницы не фиксируют конкретный состав персонажей или число ходов, поэтому эта заметка его не утверждает.

**Sources:**

- [Breadth First Search or BFS for a Graph](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

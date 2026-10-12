# Linked Lists — собеседование

Ответы заполнены для каждого вопроса в этой заметке.
[← К обзору](./7_special_data_structures.md)

### Related docs

- [Linked Lists — собеседование](./7_ds_linked_lists_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/).

### Basics of Linked Lists

<details>
<summary>Что такое linked list?</summary>

### Что такое linked list?

**Definition:**

**Linked List** — динамическая структура данных, удобная для быстрых вставок и удалений. В отличие от массивов, её элементы лежат не подряд, а связаны указателями.

**How it works:**

**Анатомия связного списка**

Связный список — набор **узлов**, у каждого есть:
- **Data**: хранимое значение.
- **Next Pointer**: ссылка на следующий узел.

Список начинается с узла **Head** и заканчивается узлом, у которого указатель `Next` равен **null**.

**Наглядное представление**

![Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)

**Ключевые свойства**

- **Динамический размер**: подстраивается под объём данных.
- **Несмежная память**: гибкость хранения.
- **Быстрые вставки и удаления**: достаточно поправить несколько указателей.

**Виды связных списков**

1. **Singly Linked List**: у каждого узла один указатель на следующий. Обход односторонний: от головы к хвосту.
![Singly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)
2. **Doubly Linked List**: у каждого узла два указателя: один на следующий узел и один на предыдущий. Это даёт обход в обе стороны.
![Doubly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fdoubly-linked-list.svg?alt=media&token=5e14dad3-c42a-43aa-99ff-940ab1d9cc3d)
3. **Circular Linked List**: как односвязный список, но хвостовой узел указывает обратно на голову и замыкает петлю.
![Circular Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fcircular-linked-list.svg?alt=media&token=b3b96bc7-3b16-4d07-978f-e4774a048ee1)
4. **Multi-level Linked List**: у узлов этого специального вида несколько указателей, каждый ведёт на разные узлы. Его часто используют в продвинутых структурах вроде многоуровневых кэшей.
![Multi-level Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fmulti-level-linked-list.svg?alt=media&token=967af5cf-8a95-4c05-a8fe-fb70f2b7ea57)

**Обычные операции и временная сложность**

- **Обход**: просмотр узлов — $O(n)$.
- **Вставка в начало**: добавить узел в начало — $O(1)$.
- **Вставка (прочие случаи) / удаление**: добавить или убрать узлы в другом месте списка — $O(n)$.
- **Поиск**: найти конкретные узлы — $O(n)$.
- **Сортировка**: упорядочить узлы списка. Обычные алгоритмы для связных списков, например сортировка слиянием, имеют сложность $O(n \log n)$.
- **Слияние**: соединить два списка — $O(n)$, где $n$ — суммарное число узлов в обоих списках.
- **Разворот**: перевернуть порядок узлов — $O(n)$.

**Пример кода: односвязный список**

Вот код на Python:

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
<summary>Что такое some pros and cons of linked list compared to arrays?</summary>

### Что такое some pros and cons of linked list compared to arrays?

**Definition:**

Посмотрим на плюсы и минусы **связных списков** по сравнению с **массивами**.

**How it works:**

**Преимущества связных списков**

- **Динамический размер**: связные списки сами подстраиваются под меняющийся размер, а массивы фиксированы. Динамические массивы меняют размер сами, но могут проигрывать по эффективности при частых вставках и удалениях в середине.

- **Эффективные вставки и удаления**: в связном списке для вставки и удаления достаточно поправить несколько указателей, а массиву может понадобиться сдвиг элементов.

- **Гибкость размера**: память под узлы выделяют и освобождают по мере надобности, и это может уменьшить потери памяти.

- **Слияние и разделение**: слить или разрезать связные списки проще.

**Недостатки связных списков**

- **Накладные расходы памяти**: у каждого узла есть накладные расходы на данные и указатель, поэтому на то же число элементов памяти уходит больше, чем у массива.

- **Последовательный доступ**: связный список даёт только последовательный доступ, в отличие от массива с прямой индексацией.

- **Неэффективность кэша**: узлы могут быть разбросаны по памяти, отсюда промахи кэша.

- **Нет произвольного доступа**: чтобы достать элемент, может понадобиться обойти весь список, а массив даёт доступ за константное время.

- **Целостность данных**: если связь узла рвётся, последующие узлы теряются.

- **Эффективность поиска**: нужны линейные просмотры, и это может быть медленнее поиска в отсортированном массиве или дереве.

- **Сортировка**: некоторые алгоритмы сортировки, например QuickSort, на связных списках менее эффективны, чем на массивах.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Объясните разницу между одиночным linked lists и двойным linked lists.</summary>

### Объясните разницу между одиночным linked lists и двойным linked lists.

**Definition:**

Варианты **Linked List**, включая **Singly Linked Lists (SLL)** и **Doubly Linked Lists (DLL)**, по-разному ведут себя в эффективности памяти и возможностях обхода.

**How it works:**

**Ключевые различия**

**Оптимизация памяти**

- **Singly Linked List**: меньше памяти на узел, потому что нужна только одна ссылка на следующий узел.
- **Doubly Linked List**: больше памяти на узел, потому что нужны две ссылки: на предыдущий и на следующий узел.

**Эффективность обхода**

- **Singly Linked List**: обход в одну сторону, от головы к хвосту.
- **Doubly Linked List**: обход в обе стороны. Можно идти от головы к хвосту и обратно.

**Сложность узла**

- **Singly Linked List**: каждый узел хранит данные и ссылку на следующий узел.
- **Doubly Linked List**: помимо данных и указателей, каждый узел хранит ссылку на предыдущий узел.

**Наглядное представление**

**Singly Linked List**: узлы связаны в одну сторону.
![Singly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fsingly-linked-list.svg?alt=media&token=c6e2ad4f-e2d4-4977-a215-6253e71b6040)
**Doubly Linked List**: узлы связаны в обе стороны, стрелки идут в двух направлениях.
![Doubly Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fdoubly-linked-list.svg?alt=media&token=5e14dad3-c42a-43aa-99ff-940ab1d9cc3d)

**Пример кода: односвязный список**

Вот код на Python:

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

**Пример кода: двусвязный список**

Вот код на Python:

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
<summary>Как linked list управляет распределением памяти иначе, чем array?</summary>

### Как linked list управляет распределением памяти иначе, чем array?

**Definition:**

Разберём, **чем связные списки и массивы** отличаются в управлении памятью и что это значит для работы с данными и вычислительной сложности.

**How it works:**

**Управление памятью**

- **Массивы**: выделяют память подряд под заранее заданный размер. Доступ к элементам эффективен, но при смене потребности в памяти возможны потери или издержки перевыделения.

- **Связные списки**: динамическое выделение памяти: каждый узел с данными и указателем выделяют по мере надобности. Эта гибкость — главное отличие от массивов.

**Наглядное представление**

![Array and Linked List Memory Layout](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Farray-linked-list-memory.webp?alt=media&token=9a02e0c9-9245-4de0-9ed1-2bc258d05fc6)

- **Массив**: элементы лежат в соседних ячейках памяти, «слотах», и это даёт прямой доступ $O(1)$ по индексу элемента.

- **Связный список**: узлы разнесены в памяти, а связи между ними идут через указатели. Каждый узел резервирует адреса и под свои данные, и под адрес следующего узла.

**Производительность массива и связного списка**

**Доступ к элементу**

- **Массив**: элементы индексированы, прямой доступ за $O(1)$, например `arr[5]`.
- **Связный список**: обычно нужен последовательный обход, доступ к элементу линеен по времени, $O(n)$.

**Накладные расходы памяти**

- **Массив**: прямой доступ к памяти и эффективность для однородных типов данных.
- **Связный список**: накладные расходы из-за хранения указателя узла, зато он лучше приспособлен к динамическим операциям.

**Вставка и удаление**

- **Массив**: в худшем случае $O(n)$ из-за возможного сдвига или изменения размера.
- **Связный список**: безусловно эффективен, обычно $O(1)$, особенно для вставок в голову или хвост.

**Эффективность выделения памяти**

- **Массив**: может недоиспользовать память или требовать изменения размера, и это даёт вычислительные и памятные издержки.
- **Связный список**: эффективнее: память выдают тогда, когда узлы создают или удаляют.

**Эффективность кэша и локальность данных**

- **Массив**: из-за непрерывной памяти силён в оптимизации кэша и кэширования CPU.
- **Связный список**: возможны промахи кэша из-за несмежного хранения узлов, и выборка данных может быть менее эффективной, чем у массивов.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Что такое the basic operations that can be performed on a linked list?</summary>

### Что такое the basic operations that can be performed on a linked list?

**Definition:**

**Связные списки** — динамические структуры данных, оптимизированные под вставку и удаление. Их ключевые операции включают:

**How it works:**

**Обход**

- **Изображение**: смотреть узлы один за другим. Обычные реализации — **итеративная** и **рекурсивная**.
- **Временная сложность**: $O(n)$
- **Пример кода**:

```python
def traverse(self):
    current = self.head
    while current:
        print(current.data)
        current = current.next
```

2. **Поиск**:
- **Описание**: найти целевое значение в списке. Нужен обход.
- **Временная сложность**: лучшая: $O(1)$; худшая: $O(n)$
- **Пример кода**:

```python
def search(self, target):
    current = self.head
    while current:
        if current.data == target:
            return True
        current = current.next
    return False
```

3. **Вставка**:
- **Описание**: добавить новый узел в заданную позицию.
- **Временная сложность**: лучшая: $O(1)$; худшая: $O(n)$, если хвост ещё надо найти
- **Пример кода**:

```python
def insert_at_start(self, data):
    new_node = Node(data)
    new_node.next = self.head
    self.head = new_node
```

4. **Удаление**:
- **Описание**: убрать узел с конкретным значением или из конкретной позиции.
- **Временная сложность**: лучшая: $O(1)$ (для головы или одного известного среднего узла); худшая: $O(n)$ (чтобы убрать хвост, прежде чем найти новый хвост)
- **Пример кода**:

```python
def delete_at_start(self):
    if self.head:
        self.head = self.head.next
```

5. **Наблюдения**:
   - Итеративный обход быстрее рекурсивного из-за накладных расходов стека.
   - Arraylist даёт лучшую производительность поиска.
   - Связные списки — первый выбор при частых вставках или удалениях в случайных позициях.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Advantages and Use Cases of Linked Lists

<details>
<summary>Что такое some real-life Use Cases of linked lists?</summary>

### Что такое some real-life Use Cases of linked lists?

**Definition:**

**Связные списки** широко используют в реальных приложениях из-за удобства динамического управления памятью и манипуляций с данными.

**How it works:**

**Практические применения**

**Операционные системы**

- **Планирование задач**: связные списки эффективно ведут очереди задач, которые ждут выполнения.
- **Управление памятью**: они помогают динамически выделять память, особенно в задачах вроде управления пулом памяти.

**Текстовые редакторы**

- **Отмена и повтор**: редакторы держат стек изменений на связных списках и так дают undo и redo.

**Музыкальные проигрыватели**

- **Плейлисты**: связные списки гибко ведут плейлисты: легко добавлять, удалять и переходить по трекам.

**Веб-браузеры**

- **История браузера**: связные списки, особенно двусвязные, нужны, чтобы ходить по истории страниц вперёд и назад.

**Компиляторы**

- **Таблицы символов**: компиляторы применяют связные списки, чтобы вести таблицы идентификаторов переменных и функций. Это даёт доступ к ним в области видимости на разных стадиях компиляции.

**Системы управления базами данных**

- **Временные структуры хранения**: основное хранение может быть на деревьях или хеш-индексах, а связные списки играют вспомогательную роль, особенно во in-memory базах.

**Искусственный интеллект и машинное обучение**

- **Представление графа**: алгоритмам с графами часто нужны списки смежности — по сути массивы связных списков — чтобы изображать вершины и рёбра.

**Алгоритмы кэширования**

- **LRU Cache**: связные списки, особенно двусвязные, играют ключевую роль в алгоритмах Least Recently Used (LRU), когда решают, какие элементы заменить.

**Сети**

- **Управление пакетами**: в сетевых сценариях связные списки ведут очереди пакетов данных, которые ждут передачи.

**Игры**

- **Инвентарь персонажа**: в ролевых играх инвентарь, куда предметы часто добавляют и откуда часто убирают, можно вести связными списками.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Когда is a Circular linked list useful?</summary>

### Когда is a Circular linked list useful?

**Definition:**

**Кольцевой связный список** — вид связного списка, где хвостовой узел намеренно связан обратно с головным и образует замкнутую петлю.

**How it works:**

![Circular Linked List](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/linked-lists%2Fcircular-linked-list.svg?alt=media&token=b3b96bc7-3b16-4d07-978f-e4774a048ee1)

**Обычные случаи**

- **Имитация кольцевых структур**: удобно представлять естественно кольцевые данные: вершины многоугольника, пулы буферов или планирование round-robin в операционных системах.

- **Эффективность очереди**: доступ к переднему и заднему элементам за константное время улучшает реализации очереди.

- **Упрощение алгоритмов**: проще манипуляции вроде разрезания и конкатенации списка за константное время.

**Пример кода: эффективность очереди**

Вот код на Python:

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
<summary>Когда is doubly linked List more efficient than singly linked List?</summary>

### Когда is doubly linked List more efficient than singly linked List?

**Definition:**

**Двусвязные списки** выгодны в отдельных случаях, но занимают больше памяти и могут требовать более сложной потокобезопасности

**How it works:**

**Ключевые различия эффективности**

- **Удаление**: если известен только удаляемый узел, двусвязный список удаляет его за $O(1)$, а односвязному может понадобиться до $O(n)$, чтобы найти предыдущий узел.

- **Операции с хвостом**: в двусвязном списке задачи у хвоста — $O(1)$. У односвязного списка без указателя на хвост это $O(n)$.

**Практические случаи**

- **Реализации кэша**: двусвязные списки удобны из-за быстрой двусторонней вставки и удаления.

- **Текстовые редакторы и undo/redo**: двусторонние возможности делают двусвязные списки эффективнее для этих функций.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Опишите сценарий, в котором linked list подходит больше, чем dynamic array.</summary>

### Опишите сценарий, в котором linked list подходит больше, чем dynamic array.

**Definition:**

**Связные списки** и **динамические массивы** — разные структуры, у каждой свои плюсы. Связные списки, например, часто обгоняют динамические массивы там, где **вставки и удаления частые**.

**How it works:**

**Соображения производительности**

- **Вставка/удаление**: у связных списков временная сложность $O(1)$, а динамические массивы в среднем медленнее, $O(n)$, из-за возможного сдвига элементов.
- **Произвольный доступ**: динамические массивы сильны в произвольном доступе $O(1)$, у связных списков сложность хуже, $O(n)$, потому что они не индексированы.

**Практический сценарий**

Возьмите интерактивную игру-кроссворд. Для удобства пусть в каждом кроссворде 10 слов. В сценарии игроки:

1. **Заполняют**: начинают с набора слов в известных позициях.
2. **Меняют местами**: просят переставить слова (например, слова 3 и 5).
3. **Расширяют/сжимают**: добавляют или убирают слово, и его позиция в списке может измениться.

**Лучший подход**: чтобы поддержать эти динамические операции и сохранить целостность списка, самый подходящий выбор — **двусвязный список**.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Linked Lists vs. Other Data Structures

<details>
<summary>Сравните реализацию array с реализацией linked listZ stack.</summary>

### Сравните реализацию array с реализацией linked listZ stack.

**Definition:**

**Стеки на массиве** сильны во временной эффективности и прямом доступе к элементу. **Стеки на связном списке** предпочтительнее, когда размер динамический и нужны лёгкие вставки или удаления.

**How it works:**

**Общие черты**

- **Скорость операций**: и `pop`, и `push` — операции $O(1)$.
- **Память**: у обоих пространственная сложность $O(n)$.
- **Гибкость**: оба могут менять размер, но стратегии изменения размера разные.

**Ключевые различия**

**Стек на массиве**

- **Локальность**: соседние адреса памяти выгодны кэшу CPU.
- **Произвольный доступ**: прямой доступ к элементу.
- **Нужда в итераторе**: предпочтителен, если нужны индексация или итераторы.
- **Производительность**: чуть быстрее на операциях с верхним элементом и потенциально лучше для задач, чувствительных ко времени, из-за кэша.
- **Push**: в среднем $O(1)$; изменение размера иногда даёт $O(n)$.

**Стек на связном списке**

- **Эффективность памяти**: лучше подходит при колеблющемся размере и ограниченной памяти.
- **Накладные расходы изменения размера**: их нет.
- **Накладные расходы указателя**: нужна дополнительная память под указатели.

**Пример кода: стек на массиве**

Вот код на Python:

```python
class ArrayBasedStack:
    def __init__(self):
        self.stack = []
    def push(self, item):
        self.stack.append(item)
    def pop(self):
        return self.stack.pop() if self.stack else None
```

**Пример кода: стек на связном списке**

Вот код на Python:

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
<summary>Как работает linked lists по сравнению с деревьями для различных операций?</summary>

### Как работает linked lists по сравнению с деревьями для различных операций?

**Definition:**

Разберём **временную и пространственную сложность** обычных операций в **связных списках** и **деревьях** и компромиссы каждой структуры.

**How it works:**

**Обычные операции структур данных**

**Поиск**

- **Связный список**: $O(n)$
- **Дерево**: $O(\log n) \text{ to } O(n)$ — в случае перекошенных деревьев, похожих на связный список.
  - Сбалансированные деревья (например, AVL, красно-чёрные): $O(\log n)$
  - Несбалансированные деревья: $O(n)$

**Вставка/удаление**

- **Связный список**: от $O(1)$ до $O(n)$, если перед операцией нужен поиск.
- **Дерево**: от $O(\log n)$ до $O(n)$ в худшем случае (например, у перекошенных деревьев).

**Операции в начале/конце**

- **Связный список**: $O(1)$
  - Односвязный список: $O(1)$
  - Двусвязный список: $O(1)$
- **Дерево**: не применимо.

**Операции в середине (по ключу или значению)**

- **Связный список**: $O(n)$

Что учесть:
    - У односвязного списка поиск и поиск узла перед целью дают $O(n)$.
    - У двусвязного списка прямой доступ к предыдущему узлу снижает время до $O(1)$.

- **Дерево**: от $O(\log n)$ до $O(n)$

Операция включает поиск ($O(\log n)$ в сбалансированном дереве) и затем, если элемент найден, изменение от константного времени до $O(\log n)$ (если дерево надо балансировать).

**Обход**

- **Связный список**: $O(n)$
- **Дерево**: $O(n)$

Обе структуры требуют посетить каждый элемент один раз.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

<details>
<summary>Когда would you use a linked list over a hash table?</summary>

### Когда would you use a linked list over a hash table?

**Definition:**

**Соображения производительности** и характер операций, которые вы собираетесь делать, сильно влияют на то, что лучше подойдёт: связный список или хеш-таблица.

**How it works:**

**Ключевые факторы выбора**

- **Порядок данных и связь**: связные списки по сути хранят порядок, и во многих сценариях это важно. Хеш-таблицы никакого определённого порядка не навязывают.

- **Накладные расходы памяти**: у связных списков более прямой подход к памяти без риска кластеризации. У хеш-таблиц накладные расходы возможны из-за хеш-функций, обработки коллизий и лишнего места, чтобы производительность не падала.

- **Время доступа**: обеим структурам для некоторых операций нужна сложность $O(1)$. Хеш-таблицы этим известны в большинстве случаев, но связные списки столь же эффективны для операций только на одном из концов списка.

- **Дубликаты**: связные списки могут прямо хранить повторяющиеся значения. В хеш-таблицах с ними сложнее, потому что ключи должны быть уникальны.

- **Сохранность данных и дисковое хранение**: связные списки удобнее для постоянного хранения, например на диске, из-за последовательного размещения данных и простого доступа к диску через указатели.

**Соображения по случаям применения**

**Обычные случаи для связных списков**

- **Динамическое выделение**: когда нужно динамическое выделение памяти, не ограниченное фиксированным размером таблицы, связные списки эффективно растут и сжимаются.
- **Эффективные вставки/удаления**: для этих операций в середине списка реализация на связном списке особенно прямолинейна и эффективна.
- **Последовательная обработка данных**: задачи вроде линейного поиска или обхода упорядоченных данных проще делать связным списком.
- **Постоянное хранение данных**: когда важна сохранность, например постоянные кэши, лог-структурированные файловые системы или базы с журналами транзакций.
- **Компактность памяти**: там, где сегментация памяти или доступ к диску косвенно бьёт по производительности, неделимые блоки связного списка могут быть выгодны.

**Обычные случаи для хеш-таблиц**

- **Быстрый поиск**: для быстрого извлечения данных по уникальному ключу хеш-таблицы сильны.
- **Доступ к файлу через отображение в память**: особенно полезно для очень больших наборов данных, когда это практично: можно снизить стоимость ввода-вывода.
- **Производительность кэша**: быстрый доступ и изменение делают их удобными для кэшей в памяти.
- **Хранение различных значений**: лучше всего, когда каждый ключ должен быть уникален. Если вставить повторяющийся ключ, существующее значение обновляется, и это полезно во многих контекстах, например в адресных таблицах сети.

**Sources:**

- [Devinterview.io — Linked Lists](https://devinterview.io/questions/data-structures-and-algorithms/linked-list-data-structure-interview-questions/)
</details>

### Traversal and Search in Linked Lists

<details>
<summary>Возможно ли обойти linked list за O(n^1/2)? (Jump Pointers).</summary>

### Возможно ли обойти linked list за O(n^1/2)? (Jump Pointers).

**Definition:**

Обойти **связный список** быстрее, чем за $O(n)$, в самом строгом смысле может быть нельзя, но есть приёмы, которые в отдельных контекстах делают обход эффективнее.

**How it works:**

В частности, разберём идею «**Jump Pointers**» или «**Square Root Jumps**»: она позволяет обойти связный список за $O(\sqrt{n})$.

**Что такое Jump Pointers?**

**Jump Pointers** ускоряют обход: на каждом шаге «перепрыгивают» фиксированное число узлов $k$. Число посещённых узлов падает, и временная сложность улучшается.

Например, при $k = \sqrt{n}$ сложность обхода падает до $O(\sqrt{n})$.

**Пример кода: Jump Pointers**

Вот код на Python:

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
<summary>Как применить binary search за O(log n) к sorted linked list? (Skip Lists).</summary>

### Как применить binary search за O(log n) к sorted linked list? (Skip Lists).

**Definition:**

У **бинарного поиска** временная сложность $O(\log n)$, но на односвязном списке он менее прямолинеен: список линеен, а доступ стоит $O(n)$. **Skip Lists** дают остроумный обходной путь и сублинейное время поиска в связных списках.

**How it works:**

**Почему обычный бинарный поиск на связных списках не дотягивает**

В **односвязном списке** произвольного доступа к элементам нет. Чтобы дойти до $k$-го элемента, надо пройти $k-1$ предшествующих узлов. Поэтому **доступ к среднему элементу** во время бинарного поиска стоит $O(n)$.

**Skip Lists: решение для сублинейного поиска**

Skip Lists дополняют отсортированные связные списки несколькими слоями «**express lanes**» и позволяют перепрыгивать участки списка. Каждый слой содержит подмножество элементов слоя ниже, и поиск идёт быстрее.

![Skip List Example](https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Skip_list.svg/1920px-Skip_list.svg.png)

Если начать с самого верхнего слоя и спускаться вниз, **пространство поиска** уменьшается на каждом шаге. В среднем поиск получается за $O(\log n)$.

**Пример кода: как выглядит Skip List**

Вот код на Python:

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
<summary>Является ли possible to do binary search on a doubly linked List in O(n) time?</summary>

### Является ли possible to do binary search on a doubly linked List in O(n) time?

**Definition:**

Применять **бинарный поиск** к **двусвязным спискам** трудно: у этих списков нет **произвольного доступа**, без которого бинарный поиск эффективен.

**How it works:**

В **бинарном поиске** каждое сравнение режет пространство поиска пополам. Но доступ к **среднему** элементу двусвязного списка стоит $O(n)$: от головы или хвоста до середины надо дойти обходом.

В итоге время работы становится $O(n \log n)$, а не оптимальным $O(\log n)$, как у массивов.

**Продвинутые приёмы поиска для двусвязных списков**

- **Jump Pointers**: несколько указателей перескакивают заранее заданное число узлов и ускоряют обход. Это приближается к временной сложности $O(n)$, а при интервале прыжка $k$ сложность улучшается до $O(n/k)$. Памяти при этом может уйти больше.

- **Interpolation Search**: изменённый бинарный поиск, который линейной интерполяцией прыгает удачнее на некоторых распределениях данных. Худшая временная сложность — $O(n)$, но на равномерно распределённых данных он может быть столь же эффективен, как $O(\log \log n)$.

**Пример кода: Jump Pointers**

Вот код на Python:

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

**Пример кода: Interpolation Search**

Вот код на Python:

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
<summary>Реализуйте a method to find the middle element of a linked list.</summary>

### Реализуйте a method to find the middle element of a linked list.

**Definition:**

Оба указателя начинают с головы. Быстрый сдвигается на два узла, медленный на один. Когда быстрый доходит до конца, медленный стоит на середине.

**How it works:**

Страница двух указателей задаёт это правило и не публикует строку Big-O. Она также не говорит, какой узел считать серединой при чётной длине. Быстрый останавливают, когда он не может сделать ещё шаг на два узла, и возвращают медленный узел.

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
<summary>Реализуйте a linked list using stack.</summary>

### Реализуйте a linked list using stack.

**Definition:**

Страница двух указателей не строит список из стека. Она исходит из списка, у узлов которого уже есть поле next.

**How it works:**

Стек даёт порядок LIFO. Разворот на странице идёт по next и заканчивается тем, что prev — новая голова. Это перевёрнутый список, а не список на стеке. Сборки узлов в стеке там нет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Разверните a singly linked List using only two pointers.</summary>

### Разверните a singly linked List using only two pointers.

**Definition:**

Разворот на странице использует три указателя, не два: curr, prev и next.

**How it works:**

curr ставят на голову. Пока curr не null, сохраняют curr.next, направляют curr.next на prev, затем prev становится curr, а curr — сохранённым next. Голова развёрнутого списка — prev. Сложность этого прохода страница не публикует.

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
<summary>Преобразование списка singly linked в циркуляр linked list.</summary>

### Преобразование списка singly linked в циркуляр linked list.

**Definition:**

Страница не описывает кольцевой список. Её проходы кончаются, когда указатель доходит до конца, то есть до пустого next.

**How it works:**

Кольцевой список сделал бы эту проверку конца бесконечной. Проверка петли на странице другая: если быстрый догоняет медленного, петля уже есть. Намеренно замыкать список там не показано.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Преобразование списка singly linked в список doubly linked.</summary>

### Преобразование списка singly linked в список doubly linked.

**Definition:**

Узлы на странице имеют поле next. Поле previous в это описание не входит, кроме указателя prev на время разворота.

**How it works:**

Во время разворота prev — узел перед curr, и curr.next поворачивают на prev. Это обратный next, а не вторая сохранённая ссылка на каждом узле. Двусвязный узел страница не показывает.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте a doubly linked List using stacks with minimal complexity.</summary>

### Реализуйте a doubly linked List using stacks with minimal complexity.

**Definition:**

Страница не реализует двусвязный список и не назначает ему сложность.

**How it works:**

Её разворот и так помнит предыдущий узел в prev, пока curr идёт вперёд. Это три локальных указателя на время прохода, а не стек узлов и не опубликованный минимум сложности.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Преобразование binary tree в список doubly linked.</summary>

### Преобразование binary tree в список doubly linked.

**Definition:**

Страница не упоминает двоичные деревья.

**How it works:**

Операции списка, которые она описывает, идут по next и, при развороте, по prev. У дерева ссылки на детей, и эта страница их не переставляет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Insertion and Deletion

<details>
<summary>Реализуйте a method to Insert a node at a specific position in a linked list.</summary>

### Реализуйте a method to Insert a node at a specific position in a linked list.

**Definition:**

Страница не показывает вставку. До её узлов доходят по next.

**How it works:**

Чтобы поставить узел на позицию, проходят по next нужное число шагов, затем next нового узла ставят на next текущего, а next текущего — на новый узел. Стоимость этого прохода страница не публикует.

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
<summary>Опишите, как удалить узел в linked list без указателя заголовка.</summary>

### Опишите, как удалить узел в linked list без указателя заголовка.

**Definition:**

Страница всегда ставит указатели на голову. Она не удаляет узел, когда голова неизвестна.

**How it works:**

Каждый приём на странице — середина, петля, разворот, n-й с конца и пересечение — задан от головы. Без этой ссылки проходы не те, что там опубликованы.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте an efficient function to Append a node to the end of a linked list.</summary>

### Реализуйте an efficient function to Append a node to the end of a linked list.

**Definition:**

Страница не показывает добавление в хвост и не говорит, какой проход эффективен.

**How it works:**

Её проверка конца — указатель дошёл до конца. Идти по next до этого конца и поставить next на новый узел — так прикрепляют хвост. Сложность не опубликована.

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
<summary>Вставьте an item in a Sorted linked list, while maintaining order.</summary>

### Вставьте an item in a Sorted linked list, while maintaining order.

**Definition:**

Страница не обсуждает отсортированные списки и вставку по порядку.

**How it works:**

Всё равно идут по next. Останавливаются на первом узле, чьё значение должно стоять после нового, и вставляют новый узел перед ним той же перестановкой next, что и вставка на позицию. Проверки порядка и сложности на странице нет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Algorithms on Linked Lists

<details>
<summary>Объясните алгоритм cycle detection от Floyd.</summary>

### Объясните алгоритм cycle detection от Floyd.

**Definition:**

Страница не использует имя Флойда. Её проверка петли — быстрый и медленный указатели.

**How it works:**

Оба начинают с головы. Быстрый сдвигается на два узла, медленный на один. Если быстрый догоняет медленного, в списке есть петля. Время и память для этой проверки страница не публикует.

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
<summary>Существует ли an alGorithm Better Than Floyd’s for cycle detection in linked lists?</summary>

### Существует ли an alGorithm Better Than Floyd’s for cycle detection in linked lists?

**Definition:**

Страница даёт одну проверку петли и не сравнивает её с другим алгоритмом и не называет лучшей.

**How it works:**

Эта проверка — быстрый на два шага и медленный на один. Встреча означает петлю. Хеша уже виденных узлов на странице нет, поэтому здесь его не ставят выше или ниже проверки двумя указателями.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Напишите an alGorithm to Shuffle a linked list.</summary>

### Напишите an alGorithm to Shuffle a linked list.

**Definition:**

Страница не перетасовывает список.

**How it works:**

Её сдвиги указателей фиксированы: один шаг, два шага или переход на голову другого списка. Ни один не выбирает случайный узел. Перетасовка за пределами этой страницы.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Определите, является ли список циклическим, используя hash table.</summary>

### Определите, является ли список циклическим, используя hash table.

**Definition:**

Проверка цикла на странице не использует хеш-таблицу. Она использует два указателя.

**How it works:**

Оба начинают с головы, быстрый идёт на два узла, медленный на один. Если быстрый догоняет медленного, петля есть. Хранить посещённые узлы в таблице страница не описывает.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте an alGorithm to Sort a linked list.</summary>

### Реализуйте an alGorithm to Sort a linked list.

**Definition:**

Страница не сортирует список.

**How it works:**

Проходы, которые она задаёт, сохраняют или переворачивают порядок ссылок. Разворот делает prev новой головой, поворачивая каждый next назад. Это не сортировка сравнениями, и оценка сортировки не опубликована.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Merging and Reversing

<details>
<summary>Объедините two sorted singly linked Lists without creating new nodes.</summary>

### Объедините two sorted singly linked Lists without creating new nodes.

**Definition:**

Страница не сливает два отсортированных списка. Её проход пересечения переключает указатель с одного списка на голову другого.

**How it works:**

Это переключение нужно, чтобы найти общий узел, а не чтобы сшить два упорядоченных списка. Не создавать новые узлы значило бы переставить уже существующие next. Для двух отсортированных входов страница этого не делает.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Разверните a linked list Recursively.</summary>

### Разверните a linked list Recursively.

**Definition:**

Разворот на странице — цикл, не рекурсивный вызов. curr, prev и next двигаются, пока curr не станет null, и prev — новая голова.

**How it works:**

Рекурсивного разворота и цены стека вызовов там нет. Опубликован итеративный проход.

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
<summary>Напишите a function that Reverse K nodes of the linked list at a time.</summary>

### Напишите a function that Reverse K nodes of the linked list at a time.

**Definition:**

Страница разворачивает весь список. Группы по k она не разворачивает.

**How it works:**

Опубликованный разворот поворачивает каждый next, пока curr не null. Остановка после k узлов и повтор на остатке не описаны, сложность не дана.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Advanced Manipulations

<details>
<summary>Найдите N-й элемент с конца одиночного linked list.</summary>

### Найдите N-й элемент с конца одиночного linked list.

**Definition:**

Два указателя p и q начинают с головы. q сдвигают на n узлов вперёд. Затем p и q сдвигают по одному узлу. Когда q доходит до конца, p стоит на n-м узле с конца.

**How it works:**

Страница задаёт эти шаги и не публикует сложность. Если q не может сдвинуться на n узлов, список короче n и такого узла нет.

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
<summary>Удалите Duplicates from an Unsorted linked list.</summary>

### Удалите Duplicates from an Unsorted linked list.

**Definition:**

Страница не удаляет дубликаты.

**How it works:**

Её проходы идут по next на один или два шага либо переключают списки. Они не сравнивают значения узлов и не отцепляют повтор. Дополнительная таблица не описана.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Найдите общие элементы в двух заданных linked lists и верните их в виде нового linked list.</summary>

### Найдите общие элементы в двух заданных linked lists и верните их в виде нового linked list.

**Definition:**

Страница не строит список общих значений. Её проход по двум спискам ищет общий узел, пересечение.

**How it works:**

p начинает на первой голове, q на второй. Каждый доходит до конца и продолжается с чужой головы. Если они встречаются, этот узел — пересечение. Если оба закончили и не встретились, пересечения нет. Это тождество узла, а не равные значения, скопированные в новый список.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Разделите linked list на k последовательных частей.</summary>

### Разделите linked list на k последовательных частей.

**Definition:**

Страница не делит список на k частей.

**How it works:**

Середина — частный случай двух указателей, которые находят один разрез: быстрый идёт вдвое быстрее медленного. k подсписков это не даёт, и граница не опубликована.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Сложите два числа, представленных как linked lists.</summary>

### Сложите два числа, представленных как linked lists.

**Definition:**

Страница не складывает числа, записанные списками.

**How it works:**

Она двигает указатели по next. Цифру она не читает и сумму с переносом в новый узел не пишет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Special Cases and Challenges

<details>
<summary>Найдите точку слияния (пересечения) двух linked lists.</summary>

### Найдите точку слияния (пересечения) двух linked lists.

**Definition:**

p начинает с первой головы, q со второй. Когда указатель доходит до конца, он продолжается с головы другого списка. Если p и q встречаются, этот узел — пересечение. Если оба дошли до конца и не встретились, его нет.

**How it works:**

Страница даёт эти шаги и не даёт сложность. Тот же проход — её ответ для пары списков в форме Y с общим хвостом.

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
<summary>Учитывая только один linked list, определите, является ли он Palindrome.</summary>

### Учитывая только один linked list, определите, является ли он Palindrome.

**Definition:**

Страница не проверяет палиндром.

**How it works:**

Она умеет найти середину и развернуть список. Значения с двух концов она не сравнивает, поэтому проверка палиндрома не входит в опубликованные результаты.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Найдите длину linked list, содержащего цикл.</summary>

### Найдите длину linked list, содержащего цикл.

**Definition:**

Страница говорит, есть ли петля. Длину петли она не измеряет.

**How it works:**

Быстрый идёт на два шага, медленный на один. Если быстрый догоняет медленного, петля есть. Страница на этом останавливается. Считать узлы по петле после встречи не описано.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Дублируйте linked list на Random Pointers, используя пробел O(1).</summary>

### Дублируйте linked list на Random Pointers, используя пробел O(1).

**Definition:**

Страница не упоминает дополнительный произвольный указатель, копию списка и границу памяти O(1).

**How it works:**

Её дополнительное состояние — два или три локальных указателя каждого приёма. Узел она не клонирует и второй указатель, кроме next при развороте, не переставляет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте a function to check if two singly linked Lists intersect without using extra space.</summary>

### Реализуйте a function to check if two singly linked Lists intersect without using extra space.

**Definition:**

Проход пересечения использует два указателя и не использует таблицу. p и q начинают на двух головах, и каждый после конца переходит на другую голову.

**How it works:**

Если указатели встречаются, этот узел — пересечение. Если оба дошли до конца и не встретились, списки не пересекаются. Страница не называет это константной дополнительной памятью в строке Big-O; она просто использует p и q.

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
<summary>Как бы вы rotate a linked list by k places?</summary>

### Как бы вы rotate a linked list by k places?

**Definition:**

Страница не вращает список на k мест.

**How it works:**

Проход к n-му с конца сдвигает один указатель на n узлов впереди другого. Так находят узел. Список в этом месте не режут и суффикс в начало не переносят.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Complexity and Analysis

<details>
<summary>Обсудите time complexity различных операций в linked lists.</summary>

### Обсудите time complexity различных операций в linked lists.

**Definition:**

Страница двух указателей не публикует время ни для одной операции.

**How it works:**

Она задаёт размер шага. Середина и поиск петли двигают один указатель на узел, другой на два. Разворот двигает один узел за итерацию. Проход к n-му с конца сдвигает один указатель на n узлов, затем оба по одному. Пересечение переключает каждый указатель на другую голову, когда он дошёл до конца. Это опубликованные правила шага, а не классы Big-O.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Оцените space complexity linked list с помощью вспомогательных указателей, например, в skip list.</summary>

### Оцените space complexity linked list с помощью вспомогательных указателей, например, в skip list.

**Definition:**

Страница не упоминает список с пропусками и не публикует оценку памяти.

**How it works:**

Вспомогательные указатели, которые она реально использует, локальные: slow и fast, или p и q, или curr, prev и next. Это не дополнительные ссылки, хранимые на каждом узле. Лишние указатели списка с пропусками за пределами этой страницы.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Объясните, как recursion влияет на время и space complexity в операциях linked list.</summary>

### Объясните, как recursion влияет на время и space complexity в операциях linked list.

**Definition:**

Алгоритмы страницы — циклы. Рекурсию она не разбирает и класс времени или памяти не даёт.

**How it works:**

Разворот, середина, поиск петли, n-й с конца и пересечение описаны как сдвиги указателей в цикле. Рекурсивной формулировки, у которой можно было бы считать стек вызовов, нет.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

### Linked List Problems and Solutions

<details>
<summary>Решите the problem of finding the Intersection Point of a Y-SHAped linked list.</summary>

### Решите the problem of finding the Intersection Point of a Y-SHAped linked list.

**Definition:**

Используют проход пересечения со страницы. Форма Y — два списка, которые дальше делят узлы.

**How it works:**

p и q ставят на две головы. Когда один доходит до конца, его отправляют на другую голову. Узел, где они встречаются, — пересечение. Если они так и не встретились, страница говорит, что точки пересечения нет. Сложность не опубликована.

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
<summary>Напишите an alGorithm to segreGAte even and odd nodes in a singly linked List.</summary>

### Напишите an alGorithm to segreGAte even and odd nodes in a singly linked List.

**Definition:**

Страница не отделяет чётные узлы от нечётных.

**How it works:**

Она двигает указатели на один или два шага и не проверяет значение. Перестановка next так, чтобы чётные значения шли перед нечётными, не описана.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Как вы можете отсортировать linked list, который сортируется по возрастанию и убыванию?</summary>

### Как вы можете отсортировать linked list, который сортируется по возрастанию и убыванию?

**Definition:**

Страница не сортирует и не обсуждает чередующийся порядок.

**How it works:**

Разворот переворачивает весь список, направляя каждый next на prev. Это перевернуло бы одно направление, но страница сначала не делит список на возрастающие и убывающие отрезки.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Разработайте метод замены пар узлов в linked list.</summary>

### Разработайте метод замены пар узлов в linked list.

**Definition:**

Страница не меняет узлы парами.

**How it works:**

Разворот переставляет каждый next до конца. Поменять только соседние пары и перескочить два узла — другая перестановка, и она не показана.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте an alGorithm to remove all Odd-Indexed Nodes from a linked list.</summary>

### Реализуйте an alGorithm to remove all Odd-Indexed Nodes from a linked list.

**Definition:**

Страница не удаляет узлы по индексу.

**How it works:**

Быстрый указатель и так шагает на два узла в проверках середины и петли. Страница использует это, чтобы найти узел или встречу, а не чтобы отцепить каждый второй узел.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Опишите методику случайного выбора узла из linked list с равномерным распределением.</summary>

### Опишите методику случайного выбора узла из linked list с равномерным распределением.

**Definition:**

Страница не выбирает случайный узел.

**How it works:**

Движение её указателей определено: один шаг, два шага или прыжок на другую голову. Ничего на странице не берёт равномерный индекс.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

<details>
<summary>Реализуйте a version of a linked list where each node has an additional arbitrary pointer to within the list.</summary>

### Реализуйте a version of a linked list where each node has an additional arbitrary pointer to within the list.

**Definition:**

Ссылка узла на странице — next. Разворот ещё использует временный prev. Произвольный дополнительный указатель, хранимый на каждом узле, на страницу не входит.

**How it works:**

Алгоритмы пересечения и n-го с конца только читают next и идут по нему. Второй указатель на узел они не выделяют и не обходят.

**Sources:**

- [Two-Pointer Technique in a Linked List](https://www.geeksforgeeks.org/dsa/two-pointer-technique-in-a-linked-list/)
</details>

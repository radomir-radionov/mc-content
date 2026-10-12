# Stacks — собеседование

Ответы заполнены для каждого вопроса в этой заметке.
[← К обзору](./7_special_data_structures.md)

### Related docs

- [Stacks — собеседование](./7_ds_stacks_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/).

### Stack Fundamentals

<details>
<summary>Что такое stack?</summary>

### Что такое stack?

**Definition:**

**Стек** — простая структура данных, которая следует принципу **«последним вошёл — первым вышел» (LIFO)**. Она похожа на стопку книг: последнее добавление лежит сверху и к нему легко подобраться.

**How it works:**

**Основные свойства**

- **Представление данных**: стек может хранить однородные или разнородные данные.
- **Ограничение доступа**: доступ в первую очередь к вершине стека, из-за чего для некоторых алгоритмов он эффективнее.

**Операции стека**

1. **Push**: добавляет элемент на вершину стека.
2. **Pop**: снимает и возвращает верхний элемент.
3. **Peek**: возвращает верхний элемент, не снимая его.
4. **isEmpty**: проверяет, пуст ли стек.
5. **isFull** (для стека на массиве): проверяет, полон ли стек.

У всех перечисленных операций типичная временная сложность $O(1)$, поэтому операции стека очень эффективны.

**Наглядное представление**

![Stack Data Structure](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/stacks%2Fstack.png?alt=media&token=74633f0a-83f7-4038-8b82-e10f0d6006b9&_gl=1*1uzhlk1*_ga*OTYzMjY5NTkwLjE2ODg4NDM4Njg.*_ga_CW55HF8NVT*MTY5NjYwNzMxNS4xNDcuMS4xNjk2NjA3NzE2LjUwLjAuMA..)

**When to use:**

**Практическое применение**

1. **Вызовы функций**: стек вызовов следит за ходом программы и выделением памяти при вызове методов.
2. **Текстовые редакторы**: отмена и повтор часто устроены на стеке.
3. **Веб-браузеры**: поведение кнопки «Назад» можно реализовать стеком.
4. **Разбор**: стеки применяют в обработке языков, например для сбалансированных скобок и **вычисления двоичных выражений**.

5. **Управление памятью**: стеки участвуют в управлении динамической памятью в компьютерных системах.

6. **Преобразование инфиксной записи в постфиксную**: это важный шаг, чтобы вычислить математические выражения вроде `2 + 3 * 5 - 4` с правильным приоритетом. Преобразование на стеке упрощает разбор и использует операции вроде `push` и `pop`, пока не получится нужный порядок.

7. **Алгоритмы на графах**: обходы вроде поиска в глубину (DFS) используют **стеки** как основной способ запоминать вершины и исследовать связные компоненты.

**Examples:**

**Пример кода: базовый стек**

Вот код на Python:

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
<summary>Почему stack is considered a Recursive data structure?</summary>

### Почему stack is considered a Recursive data structure?

**Definition:**

**Стек** считают **рекурсивной структурой данных**, потому что его определение ссылается само на себя. В любой момент стек можно описать как верхний элемент вместе с другим стеком (остатком).

Когда элемент кладут в стек или снимают с него, то, что остаётся, по-прежнему стек. Эта **самоссылочная природа**, когда операции сводят задачу к меньшим экземплярам того же типа, и есть суть **рекурсии**

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Что такое the Primary operations performed on a stack and their time complexities?</summary>

### Что такое the Primary operations performed on a stack and their time complexities?

**Definition:**

Разберём основные операции **стека** и их временную сложность.

**How it works:**

**Операции стека и сложность**

- **Push** (время: $O(1)$): новые элементы добавляются на вершину стека, поэтому это $O(1)$.
- **Pop** (время: $O(1)$): верхний элемент, и единственный доступный, снимается за эту операцию $O(1)$.
- **Peek** (время: $O(1)$): просмотр вершины стека не меняет его структуру, поэтому занимает $O(1)$.
- **Size** (время: $O(1)$): стеки обычно сами помнят свой размер, что даёт производительность $O(1)$.
- **isEmpty** (время: $O(1)$): проверка пустоты стека обычно завершается за $O(1)$.

**Examples:**

**Пример кода: стек**

Вот код на Python:

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
<summary>Когда should I use stack or queue data structures instead of arrays/Lists?</summary>

### Когда should I use stack or queue data structures instead of arrays/Lists?

**Definition:**

**Очереди** и **стеки** дают структурированный способ работать с данными и заметные преимущества перед более общими структурами вроде **списков** или **массивов**.

**How it works:**

**Ключевые свойства**

#### Очереди

- **Характеристика**: первым вошёл — первым вышел (FIFO)
- **Применение**: удобны для обработки по порядку, например очереди печати или обход BFS.

#### Стеки

- **Характеристика**: последним вошёл — первым вышел (LIFO)
- **Применение**: подходят для задач с обратным порядком, например отмена действий или обход DFS.

#### Списки/массивы

- **Характеристика**: произвольный доступ
- **Применение**: уместны, когда нужен произвольный доступ к элементам или не требуется строгий порядок и управление данными.

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Что такое Infix, Prefix, and Postfix notations?</summary>

### Что такое Infix, Prefix, and Postfix notations?

**Definition:**

В информатике **инфиксная**, **префиксная** и **постфиксная** записи — это способы записывать математические выражения. Люди обычно пользуются инфиксной записью, а машины эффективнее разбирают префиксную и постфиксную.

**How it works:**

**Инфиксная, префиксная и постфиксная записи**

- **Инфиксная**: операторы стоят между операндами. Для людей это самая обычная запись, потому что она интуитивна.

  Пример: $1 + 2$

- **Префиксная**: операторы стоят перед операндами. Порядок действий задаёт положение оператора, а не скобки.

  Пример: $+ 1 \times 2 3$, что вычисляется как $1 + (2 \times 3) = 7$

- **Постфиксная**: операторы стоят после операндов. Порядок действий задаёт последовательность, в которой идут операнды и операторы.

  Пример: $1 2 3 \times +$, что вычисляется как $1 + (2 \times 3) = 7$

**Связь со стеками**

- **Преобразование**: стеки помогают переводить выражения из одной записи в другую. Например, алгоритм сортировочной станции (Shunting Yard) переводит инфиксные выражения в постфиксную запись с помощью стека.

- **Вычисление**: и постфиксные, и префиксные выражения вычисляют стеком. Для постфиксной записи:
  1. Операнды кладут в стек.
  2. Встретив оператор, нужные операнды снимают, выполняют операцию и кладут результат обратно.

  Например, для выражения $1 2 3 \times +$:
  - 1 кладут в стек.
  - 2 кладут.
  - 3 кладут.
  - Встречается $\times$. 3 и 2 снимают, умножают, получают 6 и кладут обратно.
  - Встречается $+$. 6 и 1 снимают, складывают, получают 7 и кладут обратно. Это 7 и есть результат.

Вычисление префиксных выражений идёт похожим **методом на стеке**, но выражение обходят иначе.

**Examples:**

**Пример кода: вычисление постфиксной записи**

Вот код на Python:

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
<summary>Объясните, как stacks используются в управлении вызовами функций на языках программирования.</summary>

### Объясните, как stacks используются в управлении вызовами функций на языках программирования.

**Definition:**

Когда в языках программирования вызывают **функции**, система обычно использует **стек вызовов**, чтобы вести порядок вызовов и выделение памяти. Посмотрим, как это устроено.

**How it works:**

**Стек вызовов**

**Стек вызовов** хранит запись обо всех активных вызовах функций, которые делает программа. Когда вызывается новая функция, её добавляют на вершину стека. Когда функция заканчивает работу, её снимают со стека, и управление возвращается к вызывающей функции.

Такое поведение «последним вошёл — первым вышел» хорошо ложится на структуру данных **стек**.

**Как работает стек вызовов**

1. **Вызов функции**: при вызове функции создаётся **кадр стека** и кладётся в стек вызовов. В кадре важные сведения о состоянии функции, например локальные переменные и адрес возврата, который указывает на инструкцию после вызова функции.

2. **Локальное выполнение**: процессор выполняет инструкции вызванной функции. Функция читает свои входные данные, обрабатывает их и при необходимости вызывает другие функции.

3. **Возврат**: если вызванная функция больше никого не вызывает, она завершается, и её кадр снимают. Если же она делает ещё вызовы, стек вызовов растёт дальше.

4. **Раскрутка стека**: когда исходный (или другой самый верхний) вызов закончен, выполнять больше нечего. Стек сжимается, начиная с верхнего кадра, пока не опустеет.

**Pluses and minuses:**

**Плюсы и ограничения стека вызовов**

#### Плюсы

- **Автоматическое управление памятью**: стек вызовов сам выделяет память под локальные переменные и параметры функций, и разработчику проще управлять памятью.
- **Эффективность**: простая структура делает его эффективным для управления вызовами функций в большинстве сценариев.

#### Ограничения

- **Ограничение размера**: у стека фиксированный объём памяти, выделенный при старте программы, и если стек вырастет слишком сильно, возможна ошибка переполнения стека.
- **Нет произвольного доступа**: элементы стека можно читать или снимать только в порядке «последним вошёл — первым вышел», и для части алгоритмов и структур данных это ограничение.

**Examples:**

**Пример кода: использование стека вызовов**

Вот код, написанный на Python:

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

В этом примере, когда вызывается `calculate`, она сначала вызывает `multiply`, а затем выполняет сложение. Во время выполнения `calculate` стек вызовов выглядит так:

1. `calculate` с параметрами (2, 3, 4)
2. `multiply` с параметрами (3, 4)

Когда `multiply` завершается, её кадр стека снимают, и `calculate` продолжает со следующей строки кода.

**Sources:**

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Опишите приложение, в котором stacks естественно подходит для других структур данных.</summary>

### Опишите приложение, в котором stacks естественно подходит для других структур данных.

**Definition:**

**Стеки** естественно полезны во многих практических случаях, например в текстовых редакторах, чтобы отслеживать действия и давать функции «отменить» и «повторить».

**How it works:**

**Отмена и повтор на стеке**

В обычном текстовом редакторе пользователь может:

- **Набирать текст**: каждый ввод нового текста — это **действие**, которое можно **отменить** или **повторить**.
- **Выполнять отмену/повтор**: редактор проходит по предыдущим действиям, чтобы отменить их или вернуть.

**Examples:**

**Пример кода: стек отмены и повтора**

Вот код на Python:

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
<summary>Сравните реализацию array с реализацией linked listZ stack.</summary>

### Сравните реализацию array с реализацией linked listZ stack.

**Definition:**

**Стеки на массиве** выигрывают по времени и прямому доступу к элементам. **Стеки на связном списке** предпочтительнее, когда размер меняется и нужны простые вставки или удаления.

**How it works:**

**Общие свойства**

- **Скорость операций**: и `pop`, и `push` — операции $O(1)$.
- **Память**: у обоих пространственная сложность $O(n)$.
- **Гибкость**: оба могут менять размер, но стратегии изменения разные.

**Ключевые различия**

#### Стек на массиве

- **Локальность**: соседние ячейки памяти помогают кешу процессора.
- **Произвольный доступ**: даёт прямой доступ к элементу.
- **Нужны итераторы**: предпочтителен, если нужны индексы или итераторы.
- **Производительность**: чуть быстрее для операций с верхним элементом и потенциально лучше для задач, чувствительных ко времени, благодаря кешу.
- **Push**: в среднем $O(1)$; изменение размера иногда даёт $O(n)$.

#### Стек на связном списке

- **Эффективность памяти**: лучше подходит при плавающем размере и ограниченной памяти.
- **Накладные расходы на изменение размера**: их нет.
- **Накладные расходы на указатели**: нужна дополнительная память под указатели.

**Examples:**

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

- [Devinterview.io — Stacks](https://devinterview.io/questions/data-structures-and-algorithms/stack-data-structure-interview-questions/)

</details>

<details>
<summary>Реализуйте a Dynamic stack that automatically resizes itself.</summary>

### Реализуйте a Dynamic stack that automatically resizes itself.

**Definition:**

**Постановка задачи**

Реализуйте **динамический стек**, который сам меняет размер, когда достигает своей ёмкости.

**How it works:**

**Решение**

Изменение размера стека — это две основные операции: **сжатие** и **расширение**, когда это нужно. Обычная стратегия — **удваивать размер стека** каждый раз, когда он заполнен, и **делить пополам**, когда он заполнен на 25%, потому что так получается эффективная амортизированная производительность.

#### Ключевые операции

1. `push(item)`: добавить элемент в стек.
2. `pop()`: снять и вернуть верхний элемент стека.
3. `is_full()`: проверить, полон ли стек.
4. `is_empty()`: проверить, пуст ли стек.
5. `expand()`: удвоить ёмкость стека.
6. `shrink()`: уменьшить ёмкость стека вдвое.

#### Шаги алгоритма

1. Начать с начальной ёмкости стека. В этом примере это `2`.
2. Когда `push` встречает полный стек, перед добавлением вызвать метод `expand`.
3. Когда `pop` оставляет стек заполненным на 25%, вызвать метод `shrink`.

Так стек сам подстраивает размер под текущее число элементов.

#### Анализ сложности

- **Временная сложность**:
  - `push`: $O(1)$ амортизированно. Хотя `expand` может занять до $O(n)$, его вызывают лишь раз на каждые $n$ операций `push`, и в среднем на один `push` приходится $O(1)$.
  - `pop`, `is_full` и `is_empty`: $O(1)$.
  - `expand` и `shrink`: $O(n)$ в худшем случае, но их вызывают редко, поэтому амортизированное время — $O(1)$ на операцию.

- **Пространственная сложность**: $O(n)$, где $n$ — число элементов в стеке. Сюда входит сам стек и дополнительные расходы, например временные массивы при изменении размера.

**Examples:**

**Реализация**

Вот код на Python:

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
<summary>Что такое the performance implications of a Fixed-size array stack Implementation?</summary>

### Что такое the performance implications of a Fixed-size array stack Implementation?

**Definition:**

**Стеки на массиве фиксированного размера** просты и часто быстрее в отдельных операциях, например при кешировании данных и обработке в реальном времени, но нужно учесть несколько ограничений.

**How it works:**

**Пространство и управление памятью**

- **Ограниченная ёмкость**: массивы фиксированного размера задают стеку максимальную ёмкость, и появляется возможность переполнения.
- **Память, выделенная заранее**: под максимальную ёмкость память нужно выделить заранее, и если эта ёмкость не заполнена целиком, память может расходоваться неэффективно.
- **Постоянный размер**: стеки на массиве фиксированного размера сами размер не меняют, и если фактический размер сильно отличается от выделенного, память используется неэффективно.

**Временная сложность стека на массиве фиксированного размера**

- **Операция Push**: $O(1)$ (константное время), пока массив не заполнен и не начато изменение размера, которое в худшем случае даёт $O(n)$.
- **Операция Pop**: $O(1)$ — простое освобождение памяти или уменьшение индекса.
- **Операция Peek**: $O(1)$ — эквивалентна pop.
- **Операция поиска**: $O(n)$ — в худшем случае, когда элемент на вершине стека или отсутствует.

**Чувствительность структуры данных**

- **Чувствительность к памяти**: у стека на массиве фиксированного размера предсказуемые, постоянные требования к памяти.
- **Чувствительность к производительности**: у операций стека нефиксированного размера средняя временная сложность может быть $O(1)$, а отдельные операции стека фиксированного размера в худшем случае ухудшаются, что и оправдывает худшую сложность $O(n)$.

**When to use:**

**Практическое применение**

- **Системы реального времени**: массивы фиксированного размера предпочтительны там, где жёсткие требования ко времени, потому что операции с памятью более детерминированы.
- **Встроенные системы**: при нехватке ресурсов массивы фиксированного размера помогают управлять памятью эффективнее за счёт предсказуемых требований.
- **Системы кеширования**: массивы фиксированного размера важны в кешах, где предсказуемость требований к памяти существенна.

**Examples:**

**Пример кода: стек фиксированного размера**

Вот код на Python:

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
<summary>Разработайте stack, который поддерживает ReTrie с использованием элемента min в O(1).</summary>

### Разработайте stack, который поддерживает ReTrie с использованием элемента min в O(1).

**Definition:**

**Постановка задачи**

Нужно спроектировать структуру данных **стек**, которая умеет эффективно получать и минимальный элемент, и верхний элемент за временную сложность $O(1)$.

**How it works:**

**Решение**

Чтобы уложиться в требование по времени, будем держать два стека:

1. **Основной стек** для обычных операций стека.
2. **Вспомогательный стек**, который помнит минимальный элемент вплоть до данной позиции в стеке.

#### Шаги алгоритма

1. **Pop** и **Push**
   - Для каждого элемента $e$ в **основном стеке** проверить, меньше ли он или равен верхнему элементу **вспомогательного стека**. Если $e$ — новый минимум, положить его в оба стека.

2. **Получение минимального элемента**: верхний элемент **вспомогательного стека** всегда будет минимумом основного стека.

#### Анализ сложности

- **Временная сложность**: $O(1)$ для всех операций.
- **Пространственная сложность**: $O(N)$, где $N$ — число элементов в стеке.

**Examples:**

**Реализация**

Вот код на Python:

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
<summary>Как вы можете спроектировать stack таким образом, чтобы он был потокобезопасным?</summary>

### Как вы можете спроектировать stack таким образом, чтобы он был потокобезопасным?

**Definition:**

**Потокобезопасность** обычного стека, где операции идут по принципу «последним вошёл — первым вышел» (LIFO), можно обеспечить разными приёмами. Здесь я приведу примеры трёх разных подходов:

1. **Механизм блокировки**: поток синхронизирует доступ приёмами синхронизации вроде замков.

2. **Неблокирующий механизм**: атомарные операции без явных замков.

3. **Выбор структуры данных**: взять изначально потокобезопасные структуры данных, которые повторяют операции стека.

**How it works:**

**Механизм блокировки**

Этот подход синхронизирует операции над стеком замком. Пока поток держит замок, другие потоки не входят в критические секции.

**Неблокирующий механизм**

Этот подход использует атомарные операции над **примитивными типами данных**, которые гарантированно проходят без прерывания.

Для **многопоточной** схемы можно запустить этот код на Python:

**Выбор структуры данных**

Некоторые **классы-контейнеры**, например LifoQueue в Python, изначально сделаны потокобезопасными, поэтому их содержимое и операции защищены в многопоточной среде.

**Examples:**

**Механизм блокировки**

Вот код на Python:

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

**Неблокирующий механизм**

Вот код на Python для этого подхода:

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

**Выбор структуры данных**

Вот код на Python:

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
<summary>Реализуйте a stack with a Find-Middle operation in O(1) time.</summary>

### Реализуйте a stack with a Find-Middle operation in O(1) time.

**Definition:**

**Постановка задачи**

Нужно спроектировать структуру данных **_stack_**, которая поддерживает операции **`push`**, **`pop`**, **`findMiddle`** и **`deleteMiddle`**, все за константное время $O(1)$.

**How it works:**

**Решение**

Эту задачу можно решить двусвязным списком, где у каждого узла есть ещё указатель на средний узел. Решение построено не на массивах, но поток управления ясный, и временная сложность остаётся постоянной.

#### Шаги алгоритма

1. Создать пустой стек и установить `middle` в `NULL`.
2. При push обновлять указатель `middle` по текущему числу узлов. Если число узлов нечётное, `middle` сдвигается вверх, иначе остаётся на месте.
3. Операция pop, помимо снятия элемента, также поправляет указатель `middle`, если снятый узел на него указывал.
4. `findMiddle` и `deleteMiddle` просто обращаются к узлу, на который указывает `middle`, или меняют его.

#### Анализ сложности

- **Временная сложность**:
  - `push`: $O(1)$ — константное время на вставку каждого узла.
  - `pop`: $O(1)$ — всегда снимает верхний узел за константное время независимо от размера стека.
  - `findMiddle` и `deleteMiddle`: $O(1)$ — прямой доступ через указатель `middle`.
- **Пространственная сложность**: $O(n)$ — дополнительное место нужно под указатели каждого узла.

**Examples:**

**Реализация**

Вот код на Python:

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
<summary>Реализуйте a linked list using stack.</summary>

### Реализуйте a linked list using stack.

**Definition:**

**Постановка задачи**

Нужно реализовать **связный список** с помощью **стека**.

**How it works:**

**Решение**

**Связные списки** обычно строят из узлов, в каждом значение и указатель на следующий узел. Но связный список можно и **имитировать стеками**, которые работают по принципу «последним вошёл — первым вышел» (LIFO).

#### Шаги алгоритма

1. **Инициализация**: два стека, `list_stack` и `temp_stack`.
2. **Добавление в голову**: push в `list_stack`.
3. **Удаление из головы**: pop из `list_stack`.
4. **Вставка**: снимать элементы в `temp_stack` до точки вставки, затем положить новый элемент и вернуть элементы из `temp_stack` в `list_stack`.
5. **Удаление**: как вставка, но перед возвратом элементов снять удаляемый узел.

#### Анализ сложности

- **Временная сложность**:
  - Добавление в голову: $O(1)$
  - Удаление из головы: $O(1)$
  - Вставка в позицию $k$: $O(k)$
  - Удаление в позиции $k$: $O(k)$

- **Пространственная сложность**:
  - $O(n)$ для `list_stack`, где $n$ — число элементов.
  - $O(k)$ для `temp_stack` при вставке или удалении в позиции $k$; общая пространственная сложность всё равно $O(n)$ в худшем случае (когда $k = n$).

**Examples:**

**Реализация**

Вот код на Python:

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
<summary>Реализуйте doubly linked List using stacks with min complexity.</summary>

### Реализуйте doubly linked List using stacks with min complexity.

**Definition:**

**Постановка задачи**

Нужно реализовать **двусвязный список** с помощью **стеков**.

**How it works:**

**Решение**

Двумя стеками, `forwardStack` и `backwardStack`, можно изобразить **двусвязный список**.

- **Вставка**:
  - В начало: перенести элементы из `backwardStack` в `forwardStack`, затем положить новый элемент в `backwardStack`.
  - В конец: перенести из `forwardStack` в `backwardStack` и положить новый элемент в `backwardStack`.

- **Обход**:
  - Вперёд: снять с `backwardStack`, положить в `forwardStack`.
  - Назад: снять с `forwardStack`, положить в `backwardStack`.

- **Удаление**: пройти нужный стек, найти и убрать нужный элемент — конкретный, первый или последний.

#### Анализ сложности

- **Временная сложность**:
  - У операций `insertInBeginning`, `insertAtEnd`, `moveForward`, `moveBackward`, `delete`, `deleteFirst` и `deleteLast` худшая временная сложность $O(n)$ из-за возможного полного прохода по стеку.

- **Пространственная сложность**: $O(n)$, где $n$ — число узлов двусвязного списка; место в основном занимают два стека.

**Examples:**

**Реализация**

Вот код на Java:

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
<summary>Реализуйте a queue using Two stacks.</summary>

### Реализуйте a queue using Two stacks.

**Definition:**

Очередь можно собрать из двух стеков. Одна опубликованная схема делает enqueue за O(n) и dequeue за O(1). Другая — enqueue за O(1) и dequeue за O(n). Вместе стеки держат не больше n элементов, дополнительная память O(n).

**How it works:**

Дорогой enqueue: переложить всё из s1 в s2, положить x в s1, вернуть s2 обратно. Голова очереди — вершина s1, поэтому dequeue и front — это pop или просмотр. Дорогой dequeue: push в s1 за O(1). Когда s2 пуст, перелить s1 в s2 и снять вершину s2. Пустой front возвращает -1.

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
<summary>Реализуйте a queue using only One stack.</summary>

### Реализуйте a queue using only One stack.

**Definition:**

Та же страница строит очередь из одного стека и рекурсии. Стек вызовов держит элементы, пока алгоритм доходит до дна явного стека — это голова очереди.

**How it works:**

enqueue кладёт на вершину, поэтому вершина — хвост. dequeue снимает x, рекурсивно доходит до пустого стека (последнее значение — голова, его не кладут обратно), затем на возврате снова кладёт x. front делает тот же проход и возвращает нижнее значение в стек. Пустой front возвращает -1. Отдельную строку Big-O для этого приёма страница не публикует.

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
<summary>Реализуйте stack using Two queues with efficient push.</summary>

### Реализуйте stack using Two queues with efficient push.

**Definition:**

Схема с дешёвым push — это push за O(1) и pop за O(n) на двух очередях.

**How it works:**

push ставит x в q1. pop перекладывает из q1 в q2 все элементы, кроме последнего, удаляет этот последний (вершину стека) и меняет имена очередей, чтобы остаток снова лежал в q1. top делает тот же проход, но последний элемент кладёт в q2, а не выбрасывает. Пустой top возвращает -1. size — длина q1.

**When to use:**

Этот вариант — когда горячая операция push. Другая схема на той же странице делает push за O(n) и pop за O(1).

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
<summary>Реализуйте Three stacks with One array.</summary>

### Реализуйте Three stacks with One array.

**Definition:**

Загруженные страницы про стек не публикуют схему «три стека в одном массиве». Они показывают один явный стек как список, у которого вершина — последний индекс.

**How it works:**

Каждому стеку — свой кусок одного массива и свой индекс вершины: `[0, n//3)`, `[n//3, 2*n//3)`, `[2*n//3, n)`. Push пишет в вершину этого стека и сдвигает её вперёд, только пока она внутри куска. Pop сдвигает вершину назад. Этому фиксированному делению на загруженных страницах нет строки Big-O; push внутри своего куска — одна запись, то же точечное обновление, что у стеков на списке.

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
<summary>Разверните a String using stack.</summary>

### Разверните a String using stack.

**Definition:**

Проход по строке со стеком на странице скобок — O(n) по времени и O(n) по памяти. Если положить каждый символ и затем снимать их, строка выходит в обратном порядке: последний push — первый pop.

**How it works:**

Один проход по строке, каждый символ в стек. Затем снимать, пока стек не пуст, и дописывать символ в результат. Стек скобок на странице той же формы: одна ячейка на символ, который ещё в стеке, и проход кончается, когда вход прочитан.

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
<summary>Разверните a stack without using any additional data structure.</summary>

### Разверните a stack without using any additional data structure.

**Definition:**

Рекурсивная сортировка на странице стека не заводит второй объект стека. Дополнительная память O(n) — это стек вызовов, время O(n²).

**How it works:**

Вставка на той странице — `sortedInsert`: снять вершину, рекурсивно дойти до дна и вернуть значения так, чтобы новое встало на место. Тот же проход «снять — рекурсия — положить» доходит до дна без второго стека. Второй метод страницы, вспомогательный стек, как раз выделяет второй стек и тоже стоит O(n²) времени и O(n) дополнительной памяти. Метод без дополнительной структуры — рекурсивный.

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
<summary>Отсортируйте a stack using recursion.</summary>

### Отсортируйте a stack using recursion.

**Definition:**

Рекурсивная сортировка стека — O(n²) времени и O(n) дополнительной памяти. Наибольшее значение оказывается на вершине.

**How it works:**

`sortStack` снимает вершину, сортирует остаток и `sortedInsert` кладёт значение обратно. `sortedInsert` кладёт сразу, если стек пуст или вершина меньше либо равна значению. Иначе снимает вершину, вставляет и возвращает её. Вход `[41, 3, 32, 2, 11]` при печати с вершины даёт `[41, 32, 11, 3, 2]`.

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
<summary>Отсортируйте a stack using another stack.</summary>

### Отсортируйте a stack using another stack.

**Definition:**

Сортировка вторым стеком на той же странице — O(n²) времени и O(n) дополнительной памяти. Наибольший элемент в конце на вершине.

**How it works:**

Снять x со входа. Пока временный стек не пуст и его вершина меньше x, вернуть эту вершину во вход. Положить x во временный стек. Когда вход пуст, переложить временный стек обратно. Порядок держит временный стек.

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
<summary>Реализуйте a Shuffling AlGorithm using a stack.</summary>

### Реализуйте a Shuffling AlGorithm using a stack.

**Definition:**

Ни одна загруженная страница про стек не публикует перетасовку. Стек переворачивает порядок и не выбирает случайную перестановку.

**How it works:**

Страница очереди берёт стеки именно потому, что обычный стек — LIFO, а второй стек на странице сортировки только раскладывает значения по порядку. Перетасовке нужен источник случайных индексов, и эти страницы его не задают. Стек как шаг разворота поставит последний элемент первым — это одна фиксированная перестановка, не перетасовка.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Stack in Parsing and Expression Evaluation

<details>
<summary>Проверьте, сбалансированы ли скобки с помощью stack.</summary>

### Проверьте, сбалансированы ли скобки с помощью stack.

**Definition:**

Проверка `(){}[]` стеком — O(n) времени и O(n) памяти. Строка сбалансирована, когда каждая закрывающая совпадает с последней открывающей и в конце стек пуст.

**How it works:**

Класть `(`, `{` и `[`. На закрывающей, если стек пуст или вершина — не пара, вернуть false, иначе снять вершину. В опубликованной проверке прочие символы не попадают в ветки скобок. `[()()]{}` — true. `([{]})` — false, потому что `]` приходит раньше, чем закроется `{`.

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
<summary>Найдите повторяющиеся круглые скобки в выражении.</summary>

### Найдите повторяющиеся круглые скобки в выражении.

**Definition:**

Страница скобок не определяет повторные скобки. Её стек всё же годится: открывающие кладут в стек, а закрывающая должна найти пару, иначе выражение не сбалансировано; время O(n), память O(n).

**How it works:**

Повторная пара — дополнительный случай того же прохода. Кроме открывающих кладут операторы. На `)` снимают до `(`. Если между ними ничего не было, скобки обернули пустую пару. У `((a+b))` такая пустая обёртка есть, у `(a+b)` нет, потому что `+` стоит между скобками. Примеры самой страницы кончаются на балансе, поэтому проверка пустого снятия — добавка.

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
<summary>Как можно использовать stack для вычисления постфиксных выражений?</summary>

### Как можно использовать stack для вычисления постфиксных выражений?

**Definition:**

Страница инфикса задаёт постфикс как операнд, операнд, оператор, а её проход преобразования — O(n) времени и O(n) памяти. Вычисление идёт по этой постфиксной строке стеком значений.

**How it works:**

Операнды кладут в стек. Оператор снимает правый операнд, затем левый, применяет операцию и кладёт результат. `abc*+` для `a+b*c` оставляет сумму в стеке. Отдельную оценку времени для прохода вычисления страница не публикует; проход, который строит постфикс, — это скан O(n).

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
<summary>Преобразование выражения Infix в Postfix с помощью stack.</summary>

### Преобразование выражения Infix в Postfix с помощью stack.

**Definition:**

Перевод инфикса в постфикс стеком — O(n) времени и O(n) памяти. `a*(b+c)/d` становится `abc+*d/`.

**How it works:**

Операнды сразу идут в выход. `(` кладут в стек. `)` снимает до `(`. Оператор снимает вершину, пока у неё старше приоритет или приоритет равен и новый оператор левоассоциативен (`+`, `-`, `*`, `/`). `^` правоассоциативен, приоритет 3, поэтому равный `^` остаётся в стеке. `*` и `/` — приоритет 2. `+` и `-` — приоритет 1. Остаток снимают в конце.

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
<summary>Постройте binary tree выражений fOR заданное выражение.</summary>

### Постройте binary tree выражений fOR заданное выражение.

**Definition:**

Страница инфикса дерево не строит. Она получает постфикс за O(n) времени и O(n) памяти, а постфикс — это операнд, операнд, оператор.

**How it works:**

После преобразования проходят постфикс стеком узлов. Операнд становится листом и кладётся в стек. Оператор снимает правого ребёнка, затем левого и кладёт узел с этими детьми. Последний узел — корень. `a*(b+c)/d` на странице переводится в `abc+*d/`, значит корень — `/`.

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
<summary>Учитывая чрезвычайно большой файл, который не помещается в память, проверьте, правильно ли расставлены круглые скобки.</summary>

### Учитывая чрезвычайно большой файл, который не помещается в память, проверьте, правильно ли расставлены круглые скобки.

**Definition:**

Проверка скобок — O(n) по времени. Её память O(n) — это стек незакрытых открывающих, то есть глубина вложенности, а не вторая копия файла, который уже прочитан одной строкой.

**How it works:**

Читают по одному символу и применяют те же правила: открывающую кладут, на закрывающей либо ошибка, либо снятие пары. Файл целиком не хранят. Худший стек всё ещё O(n), если каждый символ — незакрытая открывающая; это строка памяти на странице. Другой метод страницы переписывает изменяемую строку на месте и берёт O(1) дополнительной памяти, и он непригоден для Java, Python, C# и JavaScript, потому что там строки неизменяемы.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Проверьте баланс фиксаторов fOR в очень большом (1T) файле в parallel.</summary>

### Проверьте баланс фиксаторов fOR в очень большом (1T) файле в parallel.

**Definition:**

Опубликованная проверка — один проход слева направо. Закрывающая сопоставляется с последней незакрытой открывающей, поэтому решение на каждом символе зависит от стека, собранного по префиксу.

**How it works:**

Страница не публикует параллельное разбиение файла в 1 ТБ. Если разрезать файл на независимые куски, потеряются незакрытые открывающие, которые должна увидеть закрывающая в следующем куске. Последовательный проход — O(n) времени и в худшем случае стек O(n).

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
</details>

<details>
<summary>Разработайте сценарий для обработки крупномасштабных операций отмены в реальном времени с помощью stacks.</summary>

### Разработайте сценарий для обработки крупномасштабных операций отмены в реальном времени с помощью stacks.

**Definition:**

Отмена — это LIFO: последнее записанное действие отменяют первым. Такой порядок у стека уже есть, поэтому странице очереди нужен второй стек или рекурсия, чтобы получить FIFO.

**How it works:**

Каждое завершённое действие кладут в стек. Отмена снимает одно действие и выполняет обратное. Страницы скобок и сортировки оценивают стек из n элементов как O(n) дополнительной памяти. Планировщик реального времени и формат долговечного хранения этих действий страницы не публикуют.

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
<summary>Как stack обрабатывает memory allocation в сценариях с большими данными?</summary>

### Как stack обрабатывает memory allocation в сценариях с большими данными?

**Definition:**

Алгоритмы стека, которые хранят одну запись на элемент входа, берут O(n) дополнительной памяти: стек скобок, стек следующего большего, стек биржевого размаха и вспомогательный стек сортировки.

**How it works:**

Эта память выделяется по мере роста стека, по одному push, до n записей. Рекурсивная сортировка тоже берёт O(n) дополнительной памяти, и это стек вызовов, а не второй контейнер. Переписывание скобок на месте — альтернатива страницы с O(1) дополнительной памяти, и только для изменяемой строки.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
- [Next Greater Element](https://www.geeksforgeeks.org/dsa/next-greater-element/)
- [Stock Span Problem](https://www.geeksforgeeks.org/dsa/the-stock-span-problem/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Advanced Stack Problems

<details>
<summary>Решите the Stock Span Problem using a stack.</summary>

### Решите the Stock Span Problem using a stack.

**Definition:**

Решение стеком — O(n) времени и O(n) памяти. Размах дня i — число идущих подряд дней до i включительно, у которых цена меньше либо равна цене i. Для `[10, 4, 5, 90, 120, 80]` размахи `[1, 1, 2, 4, 5, 1]`.

**How it works:**

В стеке лежат индексы. Для каждого дня снимают вершину, пока её цена меньше либо равна сегодняшней. Если стек пуст, размах равен `i + 1`. Иначе `i - top`. Затем кладут i. Вложенные циклы на той же странице — O(n²) времени и O(1) дополнительной памяти.

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
<summary>Разработайте алгоритм для расчета дождевой воды, скапливающейся между башнями, с помощью stack.</summary>

### Разработайте алгоритм для расчета дождевой воды, скапливающейся между башнями, с помощью stack.

**Definition:**

Загруженные страницы не дают формулу дождевой воды. Страница биржевого размаха публикует монотонный стек, из которого такие формулы собирают: снимать вершину, пока она меньше либо равна текущему значению, за O(n) времени и O(n) памяти, потому что каждый индекс кладут один раз.

**How it works:**

Стек индексов столбцов может использовать тот же проход «снимать, пока меньше». Собственный результат страницы размаха — `i - previousGreater`, а не объём воды. Не подменяйте его объёмом, которого на странице нет. Вложенные циклы размаха — альтернатива O(n²), когда стек не используют.

**Sources:**

- [Stock Span Problem](https://www.geeksforgeeks.org/dsa/the-stock-span-problem/)
</details>

### Stack Performance Optimization

<details>
<summary>Объясните, как минимизировать stack overflow при программировании с ограниченной памятью.</summary>

### Объясните, как минимизировать stack overflow при программировании с ограниченной памятью.

**Definition:**

Рекурсивная сортировка берёт O(n) дополнительной памяти на стеке вызовов. Второй метод той же страницы перекладывает значения явным стеком, тоже O(n) дополнительной памяти и O(n²) времени, но глубина — это цикл, а не n вложенных вызовов.

**How it works:**

Очередь на одном стеке доходит до дна рекурсией, поэтому глубина вызовов следует за числом элементов. Когда ограничен стек вызовов, берут сортировку явным стеком или очередь на двух стеках, у которой циклы не рекурсивны. Числовой настройки размера стека страницы не публикуют.

**When to use:**

Явный вспомогательный стек — когда иначе алгоритм рекурсирует по разу на элемент. Рекурсию оставляют для варианта со страницы сортировки без второго контейнера и принимают его O(n) памяти стека вызовов.

**Sources:**

- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

<details>
<summary>Какие техники улучшают locality of reference для stack?</summary>

### Какие техники улучшают locality of reference для stack?

**Definition:**

Загруженные страницы не обсуждают локальность кэша. Стек в них — непрерывная структура: список Python, `std::stack` или один массив, разрезанный по индексам.

**How it works:**

Push в этих фрагментах пишет в следующую ячейку (`append`, `push` или `a[top]`). Метод скобок на месте идёт дальше и использует ячейки самой строки как стек — это O(1) дополнительной памяти для изменяемой строки. Оценки промахов кэша здесь ни одна страница не публикует.

**Sources:**

- [Valid Parentheses](https://www.geeksforgeeks.org/dsa/check-for-balanced-parentheses-in-an-expression/)
- [Implement Stack using Queues](https://www.geeksforgeeks.org/dsa/implement-stack-using-queue/)
</details>

### Stacks in Multithreading and Concurrency

<details>
<summary>Опишите stack AND, где его можно использовать.</summary>

### Опишите stack AND, где его можно использовать.

**Definition:**

Ни одна загруженная страница не описывает стек без блокировок, compare-and-swap или wait-free push.

**How it works:**

Они описывают обычный объект стека, который меняет один проход: `push` и `pop` этого объекта, с памятью O(n), когда в нём n элементов. Параллельные вызывающие за пределами этих страниц. Очередь из двух стеков — всё та же обычная пара стеков.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

<details>
<summary>Проиллюстрируйте how a stack can be used in implementing Producer-Consumer scenarios.</summary>

### Проиллюстрируйте how a stack can be used in implementing Producer-Consumer scenarios.

**Definition:**

Один стек — это LIFO. Страница очереди существует потому, что передача «производитель–потребитель» — FIFO, а стек сам по себе отдаёт сначала самый новый элемент.

**How it works:**

Очередь из двух стеков на странице как раз превращает операции стека в очередь: enqueue за O(1) в s1, а dequeue за O(n) только когда s2 надо заполнить заново, иначе это pop. Передача — эта очередь, не голый стек. Пустой front возвращает -1.

**When to use:**

Очередь из двух стеков — когда потребитель должен увидеть самый старый элемент. Обычный стек — только для отмены, где первым должен выйти самый новый элемент.

**Sources:**

- [Queue using Stacks](https://www.geeksforgeeks.org/dsa/queue-using-stacks/)
</details>

### Stack Invariants and Monitoring

<details>
<summary>Предложите a method to maintain a History of Operations on a stack.</summary>

### Предложите a method to maintain a History of Operations on a stack.

**Definition:**

Вспомогательный стек на странице сортировки — второй стек, который запоминает значения, снятые с первого. Стек истории — та же идея: класть запись каждой операции.

**How it works:**

На каждый push или pop стека данных кладут короткую запись в стек истории. Отмена снимает историю и делает обратное. Оба стека попадают в оценку O(n) дополнительной памяти со страницы, когда в каждом n записей. Страница использует второй стек для сортировки, не для аудита, поэтому формат записи там не задан.

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
<summary>Как спроектировать monitoring system для capacity thresholds stack?</summary>

### Как спроектировать monitoring system для capacity thresholds stack?

**Definition:**

Страница очереди на одном стеке даёт `size()` как число элементов в стеке. Размер стека на двух очередях — длина q1.

**How it works:**

Перед push сравнивают эту длину с фиксированной ёмкостью. Раскладка на три куска отвергает push, который вышел бы из своего диапазона индексов. Канал оповещения страницы не задают; они задают условия пусто и полно, которыми эти проверки пользуются, включая front, равный -1, у пустой очереди на стеке.

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
<summary>Объясните, как трассировка stack используется при отладке и обработке исключений.</summary>

### Объясните, как трассировка stack используется при отладке и обработке исключений.

**Definition:**

Модуль Python `traceback` печатает или извлекает кадры стека, лежащие на исключении. `BaseException.__traceback__` хранит `types.TracebackType`.

**How it works:**

`print_exception` печатает заголовок, кадры, затем тип и значение исключения. `print_exc` — это `print_exception(sys.exception())`. `print_stack` печатает текущий стек вызовов и не требует исключения. `extract_tb` возвращает `StackSummary` из записей `FrameSummary`. Положительный `limit` печатает столько кадров со стороны вызывающего, отрицательный — последние `abs(limit)` кадров. Сцепленные исключения `__cause__` и `__context__` печатаются, когда `chain` истинен.

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
<summary>Какие методы обрабатывают stack overflow exceptions в средах с ограниченной памятью?</summary>

### Какие методы обрабатывают stack overflow exceptions в средах с ограниченной памятью?

**Definition:**

Модуль traceback печатает стек того исключения, которое возникло. Он не задаёт тип переполнения стека и не поднимает лимит стека интерпретатора.

**How it works:**

`print_exc()` и `print_exception` показывают кадры, которые привели к исключению. `extract_tb` хранит записи `FrameSummary` и не удерживает живой объект исключения; модуль описывает это как способ не держать большой граф объектов. Метод явного стека на странице сортировки избегает глубины вызовов n за счёт цикла и второго стека памяти O(n).

**Sources:**

- [traceback](https://docs.python.org/3/library/traceback.html)
- [Sort a Stack](https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/)
</details>

### Stack Algorithm Challenges

<details>
<summary>Создайте algORithm в NORmalize Pathnames с помощью stack.</summary>

### Создайте algORithm в NORmalize Pathnames с помощью stack.

**Definition:**

Загруженные страницы не определяют нормализацию пути. Проход по скобкам, который они задают, — один проход слева направо, O(n) времени и O(n) памяти стека: токены кладут и снимают пару.

**How it works:**

Проход по пути может использовать эти push и pop. Делят по `/`. Пустые части и `.` пропускают. На `..` снимают компонент, если стек не пуст. Остальные части кладут. Склеенный стек — нормализованный путь. Шаг «снять пару» — правило скобок, применённое к `..`, а строка O(n)/O(n) принадлежит проходу по скобкам той же однопроходной формы.

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
<summary>Реализуйте a function that finds the Next Greater Element using a stack.</summary>

### Реализуйте a function that finds the Next Greater Element using a stack.

**Definition:**

Решение стеком — O(n) времени и O(n) памяти. Следующий больший элемент x — первое значение справа, которое строго больше; если его нет, это -1. `[6, 8, 0, 1, 3]` даёт `[8, -1, 1, 3, -1]`.

**How it works:**

Идут справа. Снимают вершину, пока она меньше либо равна текущему значению. Если стек не пуст, вершина — ответ. Кладут текущее значение. Вложенные циклы — O(n²) времени и O(1) дополнительной памяти. Фрагмент Python на странице хранит индексы и сравнивает `arr[stack[-1]]`.

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
<summary>Разработайте алгоритм проверки циклов в Directed graph с использованием DFS на основе stack.</summary>

### Разработайте алгоритм проверки циклов в Directed graph с использованием DFS на основе stack.

**Definition:**

Загруженные страницы стека не публикуют оценку цикла в ориентированном графе. Они публикуют явный стек того, что ещё открыто: скобки, индексы или операторы.

**How it works:**

Обход в глубину может использовать такой явный стек узлов текущего пути. Узел кладут при входе и снимают при выходе. Сосед, который всё ещё в стеке, — обратное ребро к незавершённому узлу. O(V+E) для этого обхода страницы не называют, поэтому здесь этот класс не приписывается. Проверка скобок — та же идея на строке: закрывающая, которая не совпала с открывающей на стеке, это неудача, за O(n) времени для строки длины n.

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

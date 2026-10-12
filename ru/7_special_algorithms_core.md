# Algorithms — Big O

Основы алгоритмов и анализ сложности.

[ ← Вернуться в хаб](./7_special_algorithms.md)

### Related docs

* [Алгоритмы — Big O](./7_special_algorithms_core.md) · [Алгоритмы — Общие](./7_special_algorithms_common.md) · [Алгоритмы — Шаблоны](./7_special_algorithms_patterns.md) · [Структуры данных](./7_special_data_structures.md) · [Компьютер наука](./7_special_cs.md) · [SOLID](./7_special_engineering.md)

### Sections

- [Algorithms & Big O](#algorithms--big-o)

---

## 5. Algorithms & Big O

<details>
  <summary>Algorithm</summary>

### Algorithm

**Definition:** Алгоритм — это конечная последовательность четко определенных шагов для решения проблемы или выполнения задачи. **Хороший алгоритм** сочетается с правильной **структурой данных** — например. двоичный поиск требует отсортированных данных; хеш-таблицы позволяют выполнять поиск в среднем за O(1).

**When to use:** Всякий раз, когда размер входных данных может вырасти, всегда учитывайте временную и пространственную сложность.

**Pluses and minuses:**

| + (формальные алгоритмы) | − |
|--------------------------|------|
| Предсказуемая производительность | Чрезмерная инженерия простых задач |
| Интервью и валюта проектирования системы | Неправильный алгоритм формирования данных приводит к потере усилий |

**Thoughts:** Инструкция = один шаг; процедура = составные инструкции. Знайте разницу между алгоритмом и реализацией.

**Sources:**

[javascript-алгоритмы](https://github.com/trekhleb/javascript-algorithms)

---

</details>

<details>
  <summary>Big O notation</summary>

### Big O notation

**Definition:**

Big O описывает, как **время** или **пространство** увеличивается относительно размера ввода. `n` — верхняя граница асимптотического сравнения в худшем случае. Не время настенных часов; игнорирует константы.

| Обозначение | Имя | Пример |
|----------|------|---------|
| О(1) | Константа | Доступ к индексу массива, Map.get |
| O(журнал n) | Логарифмический | Бинарный поиск |
| О(п) | Линейный | Однопроходный линейный поиск |
| O(n log n) | Линеарифмический | Сортировка слиянием, быстрая сортировка (среднее) |
| О(n²) | Квадратичный | Вложенные циклы, пузырьковая сортировка |
|О (2ⁿ) | Экспоненциальный | Наивная рекурсия Фибоначчи |
| O(n!) | Факториал | Перестановки грубой силы |

**When to use:** Выбор стратегии сортировки/поиска; просмотр PR с вложенными циклами на больших коллекциях; оценки проектной мощности системы.

**Pluses and minuses:**

| + | − |
|------|------|
| Сравнение алгоритмов независимо от аппаратного обеспечения | Скрывает константы — O(n) с огромной константой может превзойти O(log n) при малых n |
| Стандартный язык интервью | В худшем случае реальные данные могут не совпадать |

**Examples:**

```js
// O(1) — constant: index / hash lookup, independent of n
const first = arr[0];
map.get(key);

// O(log n) — logarithmic: halved search space each step (sorted input)
function binarySearch(arr, target) {
  let lo = 0;
  let hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}

// O(n) — linear: one pass
function sum(arr) {
  let total = 0;
  for (const x of arr) total += x;
  return total;
}

// O(n log n) — linearithmic: split in half, then linear merge (merge sort)
function merge(left, right) {
  const out = [];
  let i = 0;
  let j = 0;
  while (i < left.length && j < right.length) {
    out.push(left[i] <= right[j] ? left[i++] : right[j++]);
  }
  return out.concat(left.slice(i), right.slice(j));
}
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  const mid = Math.floor(arr.length / 2);
  return merge(mergeSort(arr.slice(0, mid)), mergeSort(arr.slice(mid)));
}

// O(n²) — quadratic: nested loops
function pairs(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++)
    for (let j = i + 1; j < arr.length; j++)
      result.push([arr[i], arr[j]]);
  return result;
}

// O(2ⁿ) — exponential: each call branches into two (naive Fibonacci)
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}

// O(n!) — factorial: all orderings of n items
function permutations(items) {
  if (items.length <= 1) return [items];
  return items.flatMap((x, i) =>
    permutations([...items.slice(0, i), ...items.slice(i + 1)]).map((rest) => [x, ...rest]),
  );
}
```

**Thoughts:** Также знайте **лучший** и **средний** случай. Быстрая сортировка: среднее O(n log n), худшее O(n²). Пространственная сложность имеет значение для глубины рекурсии и вспомогательных массивов.

**Sources:**

[ByteByteGo Big O](https://blog.bytebytego.com/p/ep132-big-o-notation-101-the-secret) · [Академия Хана — Big-O](https://www.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/big-o-notation) · [javascript-алгоритмы](https://github.com/trekhleb/javascript-algorithms)

---

</details>

<details>
  <summary>Time vs space complexity</summary>

### Time vs space complexity

**Definition:**

**Время** = операции vs `n`. **Пробел** = дополнительная память или `n` (без учета ввода).

| Компромисс | Пример |
|-----------|---------|
| Больше места за меньшее время | Хэш-таблица для поиска O(1) |
| Меньше места, больше времени | Сортировка на месте |

**Thoughts:**

Мемоизация меняет пространство на время. Хвостовая рекурсия может быть оптимизирована на некоторых языках, но не гарантируется в JS.

---

</details>

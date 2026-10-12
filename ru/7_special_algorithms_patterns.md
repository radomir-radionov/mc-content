# Algorithms — Patterns

Два указателя, скользящее окно и быстрое принятие жадных решений против DP.

[ ← Вернуться в хаб](./7_special_algorithms.md)

### Related docs

* [Алгоритмы — Big O](./7_special_algorithms_core.md) · [Алгоритмы — Общие](./7_special_algorithms_common.md) · [Алгоритмы — Шаблоны](./7_special_algorithms_patterns.md) · [Структуры данных](./7_special_data_structures.md)

### Sections

- [Interview patterns](#interview-patterns)

---

## 7. Interview patterns

<details>
  <summary>Two pointers</summary>

### Two pointers

**Definition:** Два индекса перемещаются по массиву/строке навстречу друг другу или в одном направлении.

**When to use:** Сумма отсортированной пары, проверка палиндрома, удаление дубликатов, объединение отсортированных массивов.

**Pluses and minuses:**

| + | − |
|------|------|
| O(n) вместо O(n²) | Требуется отсортированный или структурированный ввод |
| На месте | Требуется распознавание образов |

**Examples:**

```js
function twoSumSorted(nums, target) {
  let lo = 0, hi = nums.length - 1;
  while (lo < hi) {
    const sum = nums[lo] + nums[hi];
    if (sum === target) return [lo, hi];
    if (sum < target) lo++;
    else hi--;
  }
  return null;
}
```

**Thoughts:**

Для этого варианта требуется **отсортированный** массив. Несортированная сумма двух → Карта дополнений (следующий блок).

---

</details>

<details>
  <summary>Two Sum with Map</summary>

### Two Sum with Map

**Definition:** Найдите два индекса `i`, `j` такой, что `nums[i] + nums[j] === target` за **один проход** с использованием хеш-карты значений → индекс.

**How it works:**

- Для каждого `nums[i]`, искать `need = target - nums[i]` в `Map`. Если присутствует, верните `[map.get(need), i]`. В противном случае `map.set(nums[i], i)`.
- Время **O(n)**, дополнительное пространство **O(n)**. Вложенные циклы имеют размер O(n²).
- **`Map` vs object:** ключи объекта представляют собой строки (`{1: true}` и `{"1": true}` столкнуться). `Map` сохраняет цифровые клавиши как цифры (`SameValueZero`). Безопаснее для целочисленных массивов.

**When to use:**

Классическое живое программирование Two Sum. Вариант только с сортировкой → два указателя (предыдущий блок).

#### Common pitfalls

С использованием `{}` с цифровыми клавишами. Забываем, что карта хранит *предыдущие* индексы, поэтому один и тот же элемент не связан сам с собой.

**Pluses and minuses:**

| + | − |
|------|------|
| Один проход, амортизированный поиск O(1) | O(n) памяти |
| Работает с несортированным вводом | Не находит все пары, если не продолжать |

**Examples:**

```js
function twoSum(nums, target) {
  const seen = new Map(); // value → index
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);
  }
  return null;
}
```

**Thoughts:** * Храните то, что вы видели; найдите дополнение — `Map` сохраняет цифровые клавиши, объекты — нет.*

**Sources:**

[Карта MDN] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

---

</details>

<details>
  <summary>Sliding window</summary>

### Sliding window

**Definition:** Поддерживать окно `[left, right]` по массиву/строке; расширяться вправо, сжиматься влево при нарушении ограничения.

**When to use:** Подмассив максимальной суммы размером k, самая длинная подстрока без повторов, подстрока минимального окна.

**Pluses and minuses:**

| + | − |
|------|------|
| O(n) для проблем с подстроками/подмассивами | Инвариант окна должен быть прозрачным |
| Избегает повторного сканирования | |

**Examples:**

```js
function maxSumSubarray(nums, k) {
  let sum = 0, max = 0;
  for (let i = 0; i < nums.length; i++) {
    sum += nums[i];
    if (i >= k) sum -= nums[i - k];
    if (i >= k - 1) max = Math.max(max, sum);
  }
  return max;
}
```

**Thoughts:**

Окно фиксированного и переменного размера. Карта/набор для количества символов в строковых окнах.

---

</details>

<details>
  <summary>Greedy vs DP — quick decision</summary>

### Greedy vs DP — quick decision

**Definition:**

Эвристический контрольный список для выбора жадного и динамического программирования при собеседованиях и задачах проектирования.

| Вопрос | Наклонитесь к |
|----------|-------------|
| Оптимальная подструктура + перекрывающиеся подзадачи? | ДП |
| Местный выбор доказуемо безопасен? | Жадный |
| Посчитать все способы/мин/макс с ограничениями? | ДП |
| Планирование/интервалы с сортировкой? | Часто жадный |

---

</details>

# Divide & Conquer — собеседование

Ответы по этой теме заполнены.

[← К обзору](./7_special_algorithms.md)

### Related docs

- [Divide & Conquer — собеседование](./7_algo_divide_conquer_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Divide & Conquer](https://devinterview.io/questions/data-structures-and-algorithms/divide-and-conquer-interview-questions/).

### Basic Concepts of Divide & Conquer Algorithms

<details>
<summary>Определите алгоритмы Divide & Conquer и их основные характеристики.</summary>

### Определите алгоритмы Divide & Conquer и их основные характеристики.

**Definition:**

Разделяй и властвуй режет задачу на меньшие независимые подзадачи того же вида, решает их и соединяет результаты.

**How it works:**

- Разрез должен давать подзадачи, которым не нужно промежуточное состояние друг друга.
- База — вход постоянного размера, который решают напрямую.
- Шаг соединения часто и есть настоящая работа: слияние двух отсортированных половин или сложение двух частичных произведений.
- Обычная цена — рекуррентность вида `T(n) = a T(n/b) + f(n)`.

**Examples:**

Двоичный поиск смотрит середину и продолжает в одной половине. Вторая отбрасывается, поэтому `a = 1` и `b = 2`.
</details>

<details>
<summary>Объясните разницу между Divide & Conquer и Dynamic Programming.</summary>

### Объясните разницу между Divide & Conquer и Dynamic Programming.

**Definition:**

Разделяй и властвуй решает каждую подзадачу один раз по ходу рекурсии, и эти подзадачи не пересекаются. Динамическое программирование — для пересекающихся подзадач, и оно хранит ответы, чтобы не считать их снова.

**How it works:**

- Половины mergesort не пересекаются. Наивная рекурсия Фибоначчи считает одно и то же `F(k)` много раз; таблица ДП считает его один раз.
- Разделяй и властвуй обычно идёт сверху вниз. ДП — сверху вниз с мемоизацией или снизу вверх по таблице.
- Если к рекурсии разделяй и властвуй добавить кэш и он попадает, получилось динамическое программирование.
- ДП тоже требует оптимальную подструктуру: лучший ответ строится из лучших ответов меньших состояний.
</details>

<details>
<summary>Что такое the role of recursion in Divide & Conquer alGorithms?</summary>

### Что такое the role of recursion in Divide & Conquer alGorithms?

**Definition:**

Рекурсия — обычный способ применить тот же алгоритм к каждому меньшему куску, пока не дойдёте до базы.

**How it works:**

- Каждый вызов получает диапазон, часто `lo` и `hi`, и вызывает себя на одном или нескольких поддиапазонах.
- Стек вызовов держит незаконченные шаги соединения. Глубина O(log n), когда размер сжимается в постоянное число раз, и O(n) при сильно перекошенном разрезе.
- Явный стек или цикл снизу вверх могут заменить вызовы. Нижняя форма mergesort сливает отрезки длины 1, затем 2, затем 4.
- Сама рекурсия ускорение не создаёт. Ускорение — от меньшего числа соединений, чем наивный просмотр всех пар.
</details>

<details>
<summary>Что такое the three main steps in a typical Divide & Conquer alGorithm?</summary>

### Что такое the three main steps in a typical Divide & Conquer alGorithm?

**Definition:**

Три шага — разделить, решить и соединить.

**How it works:**

- Разделение режет вход на меньшие независимые куски. Равные размеры держат рекуррентность сбалансированной.
- Решение обрабатывает каждый кусок. Если он достаточно мал, его решают напрямую. Иначе рекурсия.
- Соединение собирает результаты кусков в ответ для исходного входа.
- У некоторых алгоритмов соединение тривиально (двоичный поиск возвращает ответ рекурсии), а у некоторых тривиально разделение (работа quicksort — это разбиение до рекурсивных вызовов).

**Examples:**

Mergesort: режем массив пополам, сортируем каждую половину, затем сливаем двумя указателями.
</details>

### Mathematical Foundations

<details>
<summary>Приведите пример рекуррентного соотношения, которое может описать time complexity алгоритма Divide & Conquer.</summary>

### Приведите пример рекуррентного соотношения, которое может описать time complexity алгоритма Divide & Conquer.

**Definition:**

Рекуррентность записывает цену размера n через цену меньших вызовов плюс работу вне них.

**How it works:**

- `T(n) = 2 T(n/2) + O(n)` — mergesort: две половины и линейное слияние.
- `T(n) = T(n/2) + O(1)` — двоичный поиск: одна половина и постоянная проверка.
- `T(n) = 2 T(n/2) + O(n²)` было бы квадратичным соединением двух половин, что хуже прямого двойного цикла.
- База — `T(1) = O(1)`. Округления вниз и вверх не меняют асимптотику этих гладких разрезов.

**Examples:**

Раскрытие mergesort: `n + 2·(n/2) + 4·(n/4) + …` — это `n` на уровень и `log₂ n` уровней, значит `O(n log n)`.
</details>

<details>
<summary>Объясните основную теорему и ее важность для анализа алгоритмов Divide & Conquer.</summary>

### Объясните основную теорему и ее важность для анализа алгоритмов Divide & Conquer.

**Definition:**

Основная теорема даёт асимптотику `T(n) = a T(n/b) + O(nᵏ)`, сравнивая работу в корне с работой в листьях.

**How it works:**

- Пусть `c_crit = log_b(a)`. Если `k < c_crit`, доминируют листья и `T(n) = Θ(n^{c_crit})`.
- Если `k = c_crit`, каждый уровень стоит одинаково и `T(n) = Θ(nᵏ log n)`.
- Если `k > c_crit`, доминирует соединение в корне и `T(n) = Θ(nᵏ)`.
- Она не покрывает неровные разрезы, неполиномиальную цену соединения и округления, которые меняют форму. Для них нужно дерево рекурсии или теорема Акры–Бацци.

**Examples:**

У mergesort `a = 2`, `b = 2`, `k = 1`. `log₂ 2 = 1 = k`, поэтому средний случай даёт `Θ(n log n)`.
</details>

<details>
<summary>Как можно применить основную теорему для нахождения time complexity алгоритма binary search?</summary>

### Как можно применить основную теорему для нахождения time complexity алгоритма binary search?

**Definition:**

Двоичный поиск делает один рекурсивный вызов на половине массива и постоянную работу на сравнение среднего элемента.

**How it works:**

- Значит `a = 1`, `b = 2` и `f(n) = O(1) = O(n⁰)`.
- `log₂ 1 = 0`, что равно показателю `f`. Применяется случай равных уровней.
- Теорема тогда говорит `T(n) = Θ(n⁰ log n) = Θ(log n)`.
- Та же граница видна по глубине: каждый вызов выбрасывает половину оставшихся элементов, поэтому вызовов около `log₂ n`.
</details>

### Algorithms Design and Implementation

<details>
<summary>Опишите, как вы будете использовать Divide & Conquer для нахождения максимума и минимума array.</summary>

### Опишите, как вы будете использовать Divide & Conquer для нахождения максимума и минимума array.

**Definition:**

Режем массив пополам, ищем минимум и максимум каждой половины и оставляем меньший минимум и больший максимум.

**How it works:**

- Диапазон из одного элемента возвращает его и как min, и как max. Диапазон из двух элементов берёт одно сравнение.
- Соединение — два сравнения. Рекуррентность `T(n) = 2 T(n/2) + O(1)`, то есть `Θ(n)`.
- Число сравнений около `3n/2 − 2`, когда n — степень двойки. Это лучше, чем `2n − 2` у одного прохода слева направо, который ведёт оба значения.
- Алгоритм всё равно линейный. Выигрыш в константе, не в классе относительно одного прохода.

**Examples:**

`[4, 1, 7, 3]` режется на `[4, 1]` и `[7, 3]`. Они возвращают `(1, 4)` и `(3, 7)`. Соединение возвращает минимум 1 и максимум 7.
</details>

<details>
<summary>Проиллюстрируйте how the merge sort alGorithm exemplifies the Divide & Conquer technique.</summary>

### Проиллюстрируйте how the merge sort alGorithm exemplifies the Divide & Conquer technique.

**Definition:**

Mergesort рекурсивно сортирует каждую половину и сливает две отсортированные половины линейным проходом двух указателей.

**How it works:**

- Разделение — середина. Решение — две рекурсивные сортировки. Соединение — слияние во вспомогательный буфер.
- Слияние устойчиво, если при равных ключах первым остаётся элемент левой половины.
- Время `Θ(n log n)` в любом случае. Дополнительная память буфера — `Θ(n)`.
- В дереве рекурсии `log n` уровней, и каждый уровень переносит `Θ(n)` элементов.

**Examples:**

```text
[3, 1, 4, 2]
[3, 1] [4, 2]
[3] [1] [4] [2]
слияние -> [1, 3] [2, 4] -> [1, 2, 3, 4]
```
</details>

<details>
<summary>Объясните, как работает quicksort и как он использует стратегию Divide & Conquer.</summary>

### Объясните, как работает quicksort и как он использует стратегию Divide & Conquer.

**Definition:**

Quicksort разбивает массив вокруг опоры, затем сортирует две стороны. Соединять нечего: разбиение уже положило каждый меньший ключ слева.

**How it works:**

- Работа — разбиение, линейное по текущему диапазону. Рекурсивные вызовы затем сортируют левую и правую части, пропуская опору.
- Среднее время при случайной или удачной опоре — `O(n log n)`. Опора, которая всегда крайний ключ, даёт `O(n²)`.
- Обычно сортировка на месте, стек `O(log n)` на сбалансированных опорах и `O(n)` на вырожденных. Сначала меньшая сторона держит стек логарифмическим.
- Обычное разбиение на месте не устойчиво.

**Examples:**

`[3, 1, 4, 2]` с опорой 3 после разбиения становится `[1, 2, 3, 4]`, и обе стороны уже тривиальны.
</details>

<details>
<summary>Как the Karatsuba alGorithm for multiplying large numbers employ Divide & Conquer?</summary>

### Как the Karatsuba alGorithm for multiplying large numbers employ Divide & Conquer?

**Definition:**

Карацуба умножает два n-значных числа тремя умножениями чисел из n/2 знаков вместо четырёх, которые дал бы школьный разрез.

**How it works:**

- Записывают `x = a · B + b` и `y = c · B + d`, где B — основание в середине.
- Школьное произведение требует `ac`, `ad`, `bc` и `bd`. Карацуба ещё считает `(a + b)(c + d)` и восстанавливает `ad + bc` как это произведение минус `ac` минус `bd`.
- Рекуррентность `T(n) = 3 T(n/2) + O(n)`. Так как `log₂ 3 ≈ 1.585`, время `O(n^{1.585})`, лучше `O(n²)`.
- Ниже небольшого порога по числу знаков реализация переходит на школьное умножение, потому что лишние сложения начинают доминировать.
</details>

<details>
<summary>Опишите алгоритм Штрассена для умножения маtrieц с использованием Divide & Conquer.</summary>

### Опишите алгоритм Штрассена для умножения маtrieц с использованием Divide & Conquer.

**Definition:**

Штрассен умножает две матрицы n×n семью рекурсивными умножениями блоков n/2 вместо восьми в блочном школьном методе.

**How it works:**

- Каждую матрицу режут на четыре блока n/2. Семи произведений сумм и разностей этих блоков хватает, чтобы собрать четыре блока произведения.
- Рекуррентность `T(n) = 7 T(n/2) + O(n²)`. `log₂ 7 ≈ 2.807`, поэтому время `O(n^{2.807})`, ниже классических `O(n³)`.
- Лишние сложения и неудобная блочная раскладка делают его медленнее настроенного классического ядра, пока n не станет большим.
- Более поздние алгоритмы имеют меньший показатель. Библиотеки всё равно предпочитают блочное классическое умножение на тех размерах, которые реально поставляют.
</details>

### Problem Solving with Divide & Conquer

<details>
<summary>Как бы вы use a Divide & Conquer approach to calculate the power of a number?</summary>

### Как бы вы use a Divide & Conquer approach to calculate the power of a number?

**Definition:**

Возведение в степень делением показателя пополам на каждом вызове делает степень за логарифмическое число умножений.

**How it works:**

- `xⁿ = (x²)^{n/2}`, когда n чётно. Когда n нечётно, `xⁿ = x · xⁿ⁻¹`, и n−1 чётно.
- Рекуррентность — `T(n) = T(n/2) + O(1)` умножений, значит `O(log n)` умножений вместо `O(n)`.
- Та же идея с умножениями по модулю считает `xⁿ mod m`, не строя огромное число.
- Итеративное двоичное возведение смотрит биты n и возводит накопитель в квадрат. Это тот же алгоритм без рекурсии.

**Examples:**

`3⁸`: квадрат `9`, квадрат `81`, квадрат `6561`. Три возведения в квадрат, а не восемь умножений на 3.
</details>

<details>
<summary>Решите the Tower of Hanoi problem using Divide & Conquer techniques.</summary>

### Решите the Tower of Hanoi problem using Divide & Conquer techniques.

**Definition:**

Чтобы перенести n дисков с колышка A на C, используя запасной B, переносят n−1 на запасной, переносят самый большой на цель, затем переносят n−1 с запасного на цель.

**How it works:**

- Самый большой диск можно двигать, только когда n−1 над ним сняты, и ставить только на пустой колышек или на больший диск. Рекурсивные переносы сохраняют этот инвариант.
- `T(n) = 2 T(n−1) + O(1)` при `T(1) = 1`, поэтому число ходов `2ⁿ − 1`.
- Разрез — на две задачи размера n−1, не n/2. Форма основной теоремы с `n/b` не применяется; замкнутый вид получается раскрытием.
- Для классических правил это оптимально: граф конфигураций требует не меньше стольких ходов.

**Examples:**

Три диска A→C: перенести 2 с A на B (через C), перенести диск 3 A→C, перенести 2 с B на C (через A). Шаги «перенести 2» сами режутся на ходы одного диска.
</details>

<details>
<summary>Решите the Closest Pair of Points problem using Divide & Conquer.</summary>

### Решите the Closest Pair of Points problem using Divide & Conquer.

**Definition:**

Сортируют точки по x, режут вертикальной прямой, решают каждую половину и проверяют только полосу у прямой, где ещё может прятаться более близкая пара.

**How it works:**

- Рекурсивные ответы дают расстояние δ — лучшую пару целиком слева или целиком справа.
- Более близкая пара обязана иметь по точке в каждой половине, и обе внутри вертикальной полосы ширины 2δ вокруг разреза.
- Точкам этой полосы, отсортированным по y, достаточно постоянного числа соседей вперёд по этому порядку. Каждая точка сравнивается с несколькими следующими, не со всей полосой.
- Рекуррентность `T(n) = 2 T(n/2) + O(n)` после начальной сортировки, поэтому поиск — `O(n log n)`.

**Examples:**

Ближайшее слева — 5, справа — 4, значит δ = 4. Через разрез сравнивают только точки в пределах 4 от средней линии, и истинная ближайшая пара может быть одной из этих пар на расстоянии 3.
</details>

<details>
<summary>Используйте Divide & Conquer для решения задачи Majority Element в array.</summary>

### Используйте Divide & Conquer для решения задачи Majority Element в array.

**Definition:**
Массив делится пополам, в каждой половине ищется кандидат в большинство, и он остаётся только если встречается больше n/2 раз на всём отрезке. Это рекурсивное разбиение. Голосование Бойера–Мура — другой алгоритм.

**How it works:**
- Большинство всего отрезка является большинством хотя бы одной половины.
- База: один элемент — своё большинство.
- На слиянии оба кандидата считаются на текущем отрезке. Этот проход — Θ(n).
- Рекуррентность T(n) = 2T(n/2) + Θ(n), та же, что у сортировки слиянием, даёт O(n log n).
- **Time:** O(n log n). **Space:** O(log n) на стек рекурсии и O(1) на счётчики.
- Голосование Мура находит кандидата за один проход и проверяет его: O(n) по времени и O(1) по памяти. Массив оно не делит.

**Examples:**
```text
majority(a, lo, hi):
    if lo == hi: return a[lo]
    mid = (lo + hi) // 2
    left = majority(a, lo, mid)
    right = majority(a, mid + 1, hi)
    if left == right: return left
    if count(a, lo, hi, left) > (hi - lo + 1) // 2: return left
    if count(a, lo, hi, right) > (hi - lo + 1) // 2: return right
    return null
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/majority-element/

</details>

<details>
<summary>Реализуйте an alGorithm to efficiently find the Median of Two Sorted arrays.</summary>

### Реализуйте an alGorithm to efficiently find the Median of Two Sorted arrays.

**Definition:**
Медиана — средний элемент объединённых отсортированных массивов (или среднее двух средних, если общая длина чётная). Двоичный поиск по меньшему массиву находит разрез, который делит оба массива на равные половины.

**How it works:**
- Выбирается разрез i в меньшем массиве. Парный разрез второго массива равен half − i.
- Разрез верен, когда наибольший слева в каждом массиве не больше наименьшего справа в другом.
- Если левая часть меньшего массива слишком велика, разрез сдвигается влево, иначе вправо.
- **Time:** O(log min(n, m)). **Space:** O(1).

**Examples:**
```text
median(a, b):  // a is the shorter array
    half = (len(a) + len(b) + 1) // 2
    lo, hi = 0, len(a)
    while lo <= hi:
        i = (lo + hi) // 2
        j = half - i
        leftA = a[i - 1] if i > 0 else -infinity
        rightA = a[i] if i < len(a) else +infinity
        leftB = b[j - 1] if j > 0 else -infinity
        rightB = b[j] if j < len(b) else +infinity
        if leftA <= rightB and leftB <= rightA:
            if (len(a) + len(b)) is odd: return max(leftA, leftB)
            return (max(leftA, leftB) + min(rightA, rightB)) / 2
        if leftA > rightB: hi = i - 1
        else: lo = i + 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/median-of-two-sorted-arrays-of-different-sizes/

</details>

### Advanced Divide & Conquer Applications

<details>
<summary>Объясните ключевые различия при применении Divide & Conquer к внешней сортировке по сравнению с сортировкой в памяти.</summary>

### Объясните ключевые различия при применении Divide & Conquer к внешней сортировке по сравнению с сортировкой в памяти.

**Definition:**
Оба подхода имеют форму сортировки слиянием: независимые серии сортируются, затем сливаются. Внешняя сортировка нужна, когда данные не помещаются в RAM. Внутренняя держит весь массив в RAM.

**How it works:**
- В памяти сортировке слиянием всё равно нужен буфер: O(n) дополнительной памяти, и любой случай — O(n log n) по времени.
- Быстрая сортировка — обычная альтернатива в памяти: в среднем θ(n log n), в худшем O(n²), когда опорный элемент всегда крайний, и она дружелюбна к кэшу, потому что делит массив на месте.
- Внешняя сортировка указана как применение сортировки слиянием: получить отсортированные серии и слить их. Последовательное слияние подходит хранилищу с дорогим случайным доступом.
- **Time:** слияние в памяти — O(n log n) в любом случае. **Space:** O(n) дополнительно для слияния массива.

**Examples:**
```text
external:
    read a RAM-sized chunk, sort it, write a run
    repeat until the file is consumed
    merge the runs with the same two-way merge used in memory
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/

</details>

<details>
<summary>Как Divide & Conquer utilized in the Cooley-Tukey FFT alGorithm?</summary>

### Как Divide & Conquer utilized in the Cooley-Tukey FFT alGorithm?

**Definition:**
FFT по основанию 2 с прореживанием по времени переписывает ДПФ длины N как два ДПФ длины N/2 (чётные и нечётные индексы) плюс этап из N/2 «бабочек» размера 2. Это лемма Дэниелсона–Ланцоша.

**How it works:**
- База: ДПФ длины 1 — сам отсчёт.
- Чётная и нечётная половины преобразуются рекурсивно, затем соединяются с поворачивающими множителями.
- Два меньших результата переиспользуются для каждого выхода, поэтому стоимость линейно-логарифмическая: порядок N log N.
- Многие реализации обходят это дерево в ширину и не делают явный рекурсивный вызов.
- **Time:** O(N log N). Прямое ДПФ — отдельный, более медленный алгоритм.

**Examples:**
```text
ditfft2(x, N, s):
    if N == 1: return [x[0]]
    even = ditfft2(x, N/2, 2s)
    odd = ditfft2(x[s:], N/2, 2s)
    for k in 0 .. N/2 - 1:
        t = exp(-2πi k / N) * odd[k]
        X[k] = even[k] + t
        X[k + N/2] = even[k] - t
    return X
```

**Sources:**
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm

</details>

<details>
<summary>Решите the Skyline Problem using Divide & Conquer.</summary>

### Решите the Skyline Problem using Divide & Conquer.

**Definition:**
Силуэт — последовательность точек (x, высота), где меняется видимый контур зданий. Ожидаемое опубликованное решение проходит эти углы слева направо.

**How it works:**
- Собираются все левые и правые углы, сортируются и обходятся слева направо.
- Очередь с приоритетом хранит здания, активные в текущем x. Точка пишется только когда максимальная активная высота меняется.
- Наивный обход, который заново смотрит здания на каждом углу, — O(n²) по времени и O(n) по памяти.
- **Time:** O(n log n) для обхода с очередью с приоритетом. **Space:** O(n).

**Examples:**
```text
buildings = [[1, 5, 11]]
corners at x = 1 (height 11) and x = 5 (height drops)
skyline = [[1, 11], [5, 0]]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/the-skyline-problem-using-divide-and-conquer-algorithm/

</details>

### Optimization and Improvement

<details>
<summary>Какие стратегии помогают снизить overhead рекурсивных вызовов в Divide & Conquer algorithms?</summary>

### Какие стратегии помогают снизить overhead рекурсивных вызовов в Divide & Conquer algorithms?

**Definition:**
Рекурсия — это дерево разбиения. Накладные расходы — стек вызовов и работа на крошечных отрезках. Их срезают более крупной базой, заменой одного рекурсивного вызова циклом или полным отказом от рекурсии, если шаг объединения это позволяет.

**How it works:**
- Выпуклая оболочка «разделяй и властвуй» останавливается примерно на 5 точках и достраивает их полным перебором O(n³). Эта локальная цена не меняет общую O(n log n); слияние двух оболочек — O(n).
- Быстрая сортировка хвостовая: вторую часть можно крутить циклом, и устранение хвостового вызова уменьшает стек. Лучший дополнительный стек — O(log n); худший, когда опорный элемент всегда крайний, — O(n).
- Рекурсивный heapify в пирамидальной сортировке занимает O(log n) стека. Тот же heapify циклом занимает O(1). Время остаётся O(n log n) в любом случае.
- **Space:** от стека O(n) до O(log n) или O(1), в зависимости от того, что алгоритм позволяет.

**Examples:**
```text
quicksort(a, lo, hi):
    while lo < hi:
        p = partition(a, lo, hi)
        if p - lo < hi - p:
            quicksort(a, lo, p - 1)  // smaller side only
            lo = p + 1                // loop the larger side
        else:
            quicksort(a, p + 1, hi)
            hi = p - 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>Обсудите, как оптимизация tail recursion может быть полезна для алгоритмов Divide & Conquer.</summary>

### Обсудите, как оптимизация tail recursion может быть полезна для алгоритмов Divide & Conquer.

**Definition:**
Вызов находится в хвостовой позиции, если после его возврата делать нечего. Устранение хвостового вызова переиспользует текущий кадр вместо нового.

**How it works:**
- Статья о быстрой сортировке описывает её как хвостовую рекурсию, поэтому компилятор с этой оптимизацией может заменить второй вызов прыжком.
- Сортировка слиянием не может так поступить с обеими половинами: вторая половина выполняется, затем всё ещё нужно слияние Θ(n). Стек уменьшает только форма, где одна сторона идёт циклом, как у быстрой сортировки.
- Оценки времени не меняются. Быстрая сортировка по-прежнему θ(n log n) в среднем и O(n²) в худшем случае. Стек может упасть с O(n) к O(log n), если большая сторона идёт циклом.
- **Space:** лучший дополнительный стек быстрой сортировки — O(log n), худший — O(n).

**Examples:**
```text
// after partition, recurse on one side and set lo/hi for the other
// the second call is gone; the frame is reused by the loop
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Объясните, как memoization или caching могут повлиять на производительность алгоритмов Divide & Conquer.</summary>

### Объясните, как memoization или caching могут повлиять на производительность алгоритмов Divide & Conquer.

**Definition:**
Мемоизация хранит результат подзадачи, чтобы повторный такой же вызов его взял. Стоимость меняется только если дерево разбиения повторяет работу.

**How it works:**
- Половины сортировки слиянием — непересекающиеся отрезки. На каждом уровне элемент лежит в одном подмассиве, а объединение — новое слияние Θ(n). Кэш этих отрезков слияние не убирает, поэтому оценка остаётся O(n log n) по времени и O(n) дополнительной памяти.
- FFT по основанию 2 переиспользует два ДПФ половинной длины: каждая «бабочка» читает уже посчитанные половины. Из-за этого ДПФ падает до порядка N log N. Кэш половины, посчитанной один раз, и есть алгоритм, а не дополнительная таблица поверх рекурсии, которая считает её заново.
- **Time:** без изменений для непересекающихся разбиений вроде слияния; порядок N log N для FFT, потому что половинные ДПФ переиспользуются.

**Examples:**
```text
merge sort level: [3, 1, 4, 2] → [3, 1] and [4, 2]
those two ranges never appear again, so a map keyed by range is never hit
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm

</details>

### Complexity Analysis

<details>
<summary>Сравните временные сложности worst-case merge sort, quicksort, AND heapsORT.</summary>

### Сравните временные сложности worst-case merge sort, quicksort, AND heapsORT.

**Definition:**
Все три сортируют, многократно уменьшая задачу, но худшие оценки не совпадают.

**How it works:**
- Сортировка слиянием: лучший, средний и худший случаи — O(n log n). Слияние — Θ(n), разбиение равное. Дополнительная память — O(n).
- Быстрая сортировка: худший случай O(n²), когда опорный элемент всегда крайний (отсортированный массив и опорный — первый или последний). В среднем θ(n log n). Лучший — Ω(n log n).
- Пирамидальная сортировка: O(n log n) в любом случае, на месте. Рекурсивный heapify занимает O(log n) стека, итеративный — O(1).
- **Time:** в худшем случае соответственно O(n log n), O(n²) и O(n log n).

**Examples:**
```text
sorted input, pivot = last element:
    partitions are 0 and n-1, then 0 and n-2, ...
    comparisons sum to about n²/2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>Как вы определяете значение space complexity для алгоритма Divide & Conquer?</summary>

### Как вы определяете значение space complexity для алгоритма Divide & Conquer?

**Definition:**
К дополнительной памяти одного шага объединения прибавляется самая глубокая цепочка кадров. Обе половины одновременно живы только вдоль этой цепочки.

**How it works:**
- Слияние сортировки слиянием выделяет временный массив текущего отрезка, поэтому дополнительная память — O(n). Рекурсия даёт ещё O(log n) кадров, и это не меняет оценку O(n).
- Быстрая сортировка делит массив на месте. Лишняя память — стек: O(log n) при равных частях и O(n), когда опорный элемент всегда крайний.
- Пирамидальная сортировка переставляет массив на месте. Рекурсивный heapify занимает O(log n) стека, итеративный — O(1).
- **Space:** указываются и буферы, и стек. Время остаётся O(n log n) для слияния и пирамиды и O(n²) в худшем случае быстрой сортировки.

**Examples:**
```text
merge of n items: two temp arrays whose lengths sum to n → O(n)
quicksort stack: one frame per pivot → O(log n) balanced, O(n) skewed
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>Почему do some Divide & Conquer alGorithms have a high space complexity, and how can this be mitiGAted?</summary>

### Почему do some Divide & Conquer alGorithms have a high space complexity, and how can this be mitiGAted?

**Definition:**
Дополнительная память велика, когда каждый уровень хранит копию своего отрезка или когда плохое разбиение превращает рекурсию в цепочку из n кадров.

**How it works:**
- Сортировка слиянием не на месте. Слияние использует вспомогательный массив, поэтому дополнительная память — O(n). Это цена устойчивого слияния Θ(n).
- Наивное разбиение быстрой сортировки тоже копирует массив и занимает O(n). Разбиения Ломуто и Хоара идут на месте; тогда остаётся только стек: O(log n) в лучшем случае и O(n) в худшем.
- Пирамидальная сортировка остаётся в том же массиве. Итеративный heapify убирает стек O(log n) и оставляет O(1). Время по-прежнему O(n log n).
- **Space:** O(n) для слияния с копированием; O(1) дополнительно, когда работа — обмены в исходном массиве и рекурсия убрана.

**Examples:**
```text
mitigations that the articles actually use:
    merge sort: accept the O(n) buffer (needed for the stable merge)
    quicksort: Lomuto/Hoare instead of a copied partition
    heapsort: iterative heapify → O(1) auxiliary
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

### Parallelization and Distributed Computing

<details>
<summary>Обсудите, как можно распараллелить алгоритмы Divide & Conquer.</summary>

### Обсудите, как можно распараллелить алгоритмы Divide & Conquer.

**Definition:**
Два рекурсивных вызова не читают результат друг друга, поэтому могут идти одновременно. Слияние ждёт, пока обе половины готовы.

**How it works:**
- Статья о сортировке слиянием называет это естественно параллельным: подмассивы сортируются независимо, затем сливаются.
- Само слияние — последовательный проход Θ(n) по двум половинам. Параллельность этот проход не убирает, работа остаётся O(n log n). Длина критической цепочки — глубина дерева: O(log n) разбиений плюс слияния вдоль одного пути.
- Две стороны быстрой сортировки тоже независимы после разбиения. Само разбиение — линейный просмотр текущего отрезка.
- **Time:** работа сортировки слиянием — O(n log n) в любом случае. **Space:** O(n) дополнительно на слияния плюс кадры на каждого исполнителя.

**Examples:**
```text
parallel_merge_sort(a, lo, hi):
    if hi - lo < cutoff: sort sequentially; return
    mid = (lo + hi) // 2
    in parallel:
        parallel_merge_sort(a, lo, mid)
        parallel_merge_sort(a, mid + 1, hi)
    merge(a, lo, mid, hi)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Приведите an example of a Divide & Conquer alGorithm that could benefit from a distributed computing environment.</summary>

### Приведите an example of a Divide & Conquer alGorithm that could benefit from a distributed computing environment.

**Definition:**
Внешняя сортировка слиянием — случай, который статья о сортировке слиянием называет для данных, не помещающихся в память. Каждая машина сортирует свою серию, затем слияние соединяет серии.

**How it works:**
- Набор делится на куски, которые помещаются в RAM. Каждый кусок сортируется независимо: это шаг разделения, и обмена данными нет.
- Отсортированные серии затем сливаются. Слияние в памяти — O(n log n) по времени и O(n) дополнительной памяти для серии из n элементов.
- Та же форма сортирует связный список: статья предпочитает для него сортировку слиянием, потому что узлы не дают случайного доступа, а половины всё равно сортируются отдельно.
- **Time:** O(n log n) на каждую серию в памяти. **Space:** O(n) дополнительно на машине, которая сливает серию из n элементов.

**Examples:**
```text
machines 1..k:
    sort the local chunk with merge sort
coordinator:
    merge the k sorted runs, same two-way merge
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

### Miscellaneous

<details>
<summary>Как the use of randomization in quicksort improve its performance on average?</summary>

### Как the use of randomization in quicksort improve its performance on average?

**Definition:**
Случайный опорный элемент не имеет фиксированного входа, который всегда оказывается крайним. Худший случай всё ещё существует, но это уже не отсортированный массив.

**How it works:**
- Опорный элемент «последний» даёт худший случай на уже отсортированном массиве: каждое разбиение отщепляет один элемент.
- Статья о быстрой сортировке предпочитает случайный опорный элемент, потому что для худшего случая нет закономерности.
- Среднее время остаётся θ(n log n). Худшее по-прежнему O(n²). Лучшее — Ω(n log n), когда опорный элемент делит массив пополам.
- **Space:** стек O(log n) при сбалансированном дереве и O(n), когда это цепочка.

**Examples:**
```text
partition(a, lo, hi):
    swap(a[lo + random(0, hi - lo)], a[hi])  // hide the random pivot at the end
    then Lomuto-partition around a[hi]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/quick-sort/

</details>

<details>
<summary>Опишите случай, когда алгоритм Divide & Conquer — не лучший выбор.</summary>

### Опишите случай, когда алгоритм Divide & Conquer — не лучший выбор.

**Definition:**
Разделяй-и-властвуй для максимального подмассива корректен и быстрее перебора всех подмассивов, но проход Кадане всё равно быстрее.

**How it works:**
- Перебор всех подмассивов — O(n²) по времени и O(1) по памяти.
- Версия «разделяй и властвуй» берёт максимум левой половины, правой половины и лучшей суммы через середину. Рекуррентность совпадает с сортировкой слиянием.
- **Time:** O(n log n) у разбиения, O(n) у Кадане. **Space:** O(n) у разбиения, O(1) у Кадане.
- Страница считает разбиение хорошим примером и прямо говорит, что алгоритм Кадане лучше.

**Examples:**
```text
a = [2, 3, -8, 7, -1, 2, 3]
Kadane and the split both return 11, from [7, -1, 2, 3]
Kadane does it in one forward pass
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Обсудите применение Divide & Conquer в областях, не связанных с информатикой.</summary>

### Обсудите применение Divide & Conquer в областях, не связанных с информатикой.

**Definition:**
Выпуклая оболочка — наименьший выпуклый многоугольник, содержащий набор точек. Построение «разделяй и властвуй» применяется и вне программирования, везде, где форма или зона столкновения — внешняя граница точек.

**How it works:**
- Точки делятся на левую и правую половины. Оболочка каждой половины строится рекурсивно, пока набор не станет маленьким (около 5 точек), где берётся полный перебор O(n³).
- Две оболочки сливаются верхней и нижней касательными. Это слияние — O(n).
- Статья называет обнаружение столкновений, выделение формы на изображении и контур облака точек.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
robot workspace: points the arm can reach
hull: the outer polygon used as the collision shape
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/

</details>

### Practical Applications and Coding Challenges

<details>
<summary>Разработайте алгоритм Divide & Conquer для подсчета инверсий в array.</summary>

### Разработайте алгоритм Divide & Conquer для подсчета инверсий в array.

**Definition:**
Инверсия — пара i < j с arr[i] > arr[j]. У отсортированного массива 0 инверсий. У обратного — максимум.

**How it works:**
- Массив делится, инверсии считаются внутри каждой половины, затем перекрёстные пары считаются при слиянии.
- Если правое значение меньше, оно меньше всех ещё не взятых левых, поэтому за один шаг прибавляется (n1 − i).
- Слияние сортирует отрезок. Если нужен исходный порядок, сначала копируют массив.
- Наивный перебор пар: O(n²) по времени и O(1) по памяти.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
count(a, l, r):
    if l >= r: return 0
    m = (l + r) // 2
    inv = count(a, l, m) + count(a, m + 1, r)
    return inv + merge_count(a, l, m, r)

merge_count: if left[i] <= right[j]: take left
             else: take right and add (n1 - i)
[4, 3, 2, 1] → 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/counting-inversions/

</details>

<details>
<summary>Напишите an efficient alGorithm for the Convex Hull problem using Divide & Conquer.</summary>

### Напишите an efficient alGorithm for the Convex Hull problem using Divide & Conquer.

**Definition:**
Нужен наименьший выпуклый многоугольник, содержащий все точки. Если известны оболочки левой и правой половин, полная оболочка — их верхняя и нижняя касательные плюс внешние цепи.

**How it works:**
- Точки делятся по координате x, пока в части не останется около 5 или меньше. Их решает полный перебор, локально O(n³).
- Слияние двух оболочек касательными — O(n). Деление пополам даёт всему алгоритму O(n log n).
- База из 3 точек — сами точки, но слияние оболочки из 2 точек с оболочкой из 3 может зациклиться; статья поэтому останавливается на 5 и берёт полный перебор.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
hull(points sorted by x):
    if size <= 5: return brute_hull(points)   // O(n³) locally
    left = hull(first half)
    right = hull(second half)
    return upper_tangent(left, right) + lower_tangent(left, right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Приведите an example of a real-world application where Divide & Conquer alGorithms significantly improve performance.</summary>

### Приведите an example of a real-world application where Divide & Conquer alGorithms significantly improve performance.

**Definition:**
Дискретное преобразование Фурье длины N, записанное как два преобразования длины N/2, — это шаг Кули–Тьюки по основанию 2. Переиспользование половин делает спектральную работу практической.

**How it works:**
- Прямая сумма по всем парам гораздо тяжелее. Разбиение Дэниелсона–Ланцоша переиспользует каждое ДПФ половинной длины в «бабочке», и стоимость становится порядка N log N.
- Та же идея есть во внешней сортировке: данные, не помещающиеся в память, сортируются независимыми сериями и затем сливаются. Сортировка слиянием указывает это как применение. Каждая серия — O(n log n).
- **Time:** O(N log N) для FFT по основанию 2.

**Examples:**
```text
audio frame of N samples:
    even = FFT(samples 0, 2, 4, ...)
    odd = FFT(samples 1, 3, 5, ...)
    combine with N/2 butterflies
```

**Sources:**
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Реализуйте a Divide & Conquer alGorithm to solve the Maximum Subarray Problem.</summary>

### Реализуйте a Divide & Conquer alGorithm to solve the Maximum Subarray Problem.

**Definition:**
Ответ — лучший из трёх кусков: лучший подмассив целиком слева от середины, целиком справа или лучшая сумма, которая пересекает середину.

**How it works:**
- Один элемент — своя лучшая сумма.
- Пересекающая сумма идёт влево от середины (включая середину) и вправо от середины + 1, затем складывает два лучших префикса.
- Рекуррентность той же формы, что у сортировки слиянием.
- Перебор всех подмассивов — O(n²) по времени и O(1) по памяти. Проход Кадане — O(n) и O(1), и статья называет его лучшим алгоритмом.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
max_sum(a, l, h):
    if l == h: return a[l]
    m = l + (h - l) // 2
    cross = best_sum(a, m, l, -1) + best_sum(a, m + 1, h, +1)
    return max(max_sum(a, l, m), max_sum(a, m + 1, h), cross)

[2, 3, -8, 7, -1, 2, 3] → 11
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

### Coding Challenges on Divide & Conquer Algorithms

<details>
<summary>Реализуйте an alGorithm to find the Median of Two Sorted arrays using Divide & Conquer.</summary>

### Реализуйте an alGorithm to find the Median of Two Sorted arrays using Divide & Conquer.

**Definition:**
Тот же поиск разреза, что и в эффективном алгоритме медианы: на каждом сравнении двух разрезов отбрасывается половина более короткого массива, пока левая и правая стороны объединённой последовательности не уравняются.

**How it works:**
- На шаге отбрасывается половина, в которой не может лежать разрез медианы.
- Нечётная длина: медиана — максимум двух левых границ. Чётная: среднее максимума левых границ и минимума правых.
- **Time:** O(log min(n, m)). **Space:** O(1).

**Examples:**
```text
// a = [1, 3], b = [2] → half = 2, cut i = 1 in a, j = 1 in b
// left borders 1 and 2, right border 3 → odd length → max(1, 2) = 2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/median-of-two-sorted-arrays-of-different-sizes/

</details>

<details>
<summary>Напишите решение Divide & Conquer для слияния k отсортированных списков в один отсортированный список.</summary>

### Напишите решение Divide & Conquer для слияния k отсортированных списков в один отсортированный список.

**Definition:**
k списков делятся на две группы, каждая группа сливается рекурсивно, затем два получившихся отсортированных списка сливаются обычным слиянием двух указателей.

**How it works:**
- Остановка, когда в группе один список: он уже отсортирован.
- Попарное слияние — тот же шаг, что у сортировки слиянием.
- Линейный поиск наименьшей головы среди k списков медленнее: O(n · k²), если в каждом списке O(n) узлов, и O(1) дополнительной памяти.
- Мин-куча текущих голов — O(n · k · log k), если в каждом списке O(n) узлов, это же O(N log k) для N узлов всего, и O(k) дополнительной памяти.
- **Time:** O(n · k · log k), где n — длина самого длинного списка. **Space:** O(log k) на рекурсию.

**Examples:**
```text
merge_k(lists, lo, hi):
    if lo == hi: return lists[lo]
    mid = (lo + hi) // 2
    left = merge_k(lists, lo, mid)
    right = merge_k(lists, mid + 1, hi)
    return merge_two(left, right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-k-sorted-linked-lists/

</details>

<details>
<summary>Разработайте алгоритм Divide & Conquer для решения задачи о максимальном Subarray и найдите смежный subarray с максимальной суммой.</summary>

### Разработайте алгоритм Divide & Conquer для решения задачи о максимальном Subarray и найдите смежный subarray с максимальной суммой.

**Definition:**
То же тройное разбиение, что и в задаче максимального подмассива. Возвращается сумма; проход через середину находит подмассив, лежащий по обе стороны разреза.

**How it works:**
- Левый проход включает индекс m и идёт к l. Правый начинается в m + 1 и идёт к h. Их сумма — лучший пересекающий подмассив.
- Берётся максимум из левой, правой и пересекающей сумм.
- **Time:** O(n log n). **Space:** O(n).
- Кадане находит ту же сумму за O(n) времени и O(1) памяти.

**Examples:**
```text
a = [-2, -4] → -2   // the single element -2 beats -4 and beats -6
a = [5, 4, 1, 7, 8] → 25
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Постройте binary tree из заданных preorder и inorder обходов с помощью Divide & Conquer.</summary>

### Постройте binary tree из заданных preorder и inorder обходов с помощью Divide & Conquer.

**Definition:**
Следующее значение предпорядка — корень. Его индекс в симметричном обходе делит остальные значения на левое и правое поддеревья.

**How it works:**
- Хеш-таблица «значение → индекс» делает поиск O(1). Без неё каждый корень ищется линейным просмотром.
- Рекурсия идёт по отрезку симметричного обхода слева от корня, затем справа. Индекс предпорядка сдвигается на один узел.
- **Time:** O(n) с таблицей, O(n²) если каждый корень ищется в симметричном массиве. **Space:** O(n) с таблицей; версия с просмотром занимает стек O(h).

**Examples:**
```text
build(inL, inR):
    if inL > inR: return null
    root = Node(pre[preIndex]); preIndex += 1
    i = indexOf[root.val]
    root.left = build(inL, i - 1)
    root.right = build(i + 1, inR)
    return root
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/construct-tree-from-given-inorder-and-preorder-traversal/

</details>

<details>
<summary>Постройте binary tree из данного InORder AND PostORder traveZRSAZl arraysZ, используя подход «divide and conquer».</summary>

### Постройте binary tree из данного InORder AND PostORder traveZRSAZl arraysZ, используя подход «divide and conquer».

**Definition:**
Следующее неиспользованное значение постпорядка, читая с конца, — корень. Постпорядок обходит левое, затем правое, затем корень, поэтому правое поддерево строится раньше левого.

**How it works:**
- Каждое значение симметричного обхода отображается в индекс. Поиск корня просмотром — медленная версия.
- Постпорядок читается с конца. Сначала строится правый отрезок симметричного обхода, затем левый.
- **Time:** O(n) с хешированием, O(n²) при линейном поиске каждого корня. **Space:** O(n) с таблицей; для версии с просмотром указана память O(n).

**Examples:**
```text
build(inL, inR):
    if inL > inR: return null
    root = Node(post[postIndex]); postIndex -= 1
    i = indexOf[root.val]
    root.right = build(i + 1, inR)
    root.left = build(inL, i - 1)
    return root
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/construct-a-binary-tree-from-postorder-and-inorder/

</details>

<details>
<summary>Преобразуйте отсортированный array в сбалансированный по высоте binary search tree с помощью Divide & Conquer.</summary>

### Преобразуйте отсортированный array в сбалансированный по высоте binary search tree с помощью Divide & Conquer.

**Definition:**
Средний элемент становится корнем. Левая половина — левое поддерево, правая — правое, поэтому стороны отличаются не больше чем на один узел.

**How it works:**
- Пустой отрезок — пустой ребёнок.
- Выбор середины оставляет примерно половину значений с каждой стороны, поэтому дерево остаётся сбалансированным по высоте.
- То же построение можно сделать очередью вместо рекурсии. Оба ожидаемых подхода на странице — O(n).
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
build(a, lo, hi):
    if lo > hi: return null
    mid = (lo + hi) // 2
    root = Node(a[mid])
    root.left = build(a, lo, mid - 1)
    root.right = build(a, mid + 1, hi)
    return root

[1, 2, 3, 4, 5, 6, 7] → root 4, left 2, right 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-array-to-balanced-bst/

</details>

<details>
<summary>Преобразуйте отсортированный linked list в сбалансированный по высоте binary search tree с помощью метода Divide & Conquer.</summary>

### Преобразуйте отсортированный linked list в сбалансированный по высоте binary search tree с помощью метода Divide & Conquer.

**Definition:**
Список уже отсортирован, поэтому симметричный обход дерева — это сам список. Сначала строится левая половина, следующий узел списка становится корнем, затем строится правая половина.

**How it works:**
- Узлы считаются один раз. Вызов, которому принадлежат n узлов, строит левое поддерево из n/2 узлов, делает текущую голову корнем, сдвигает голову и строит правое поддерево из n − n/2 − 1 узлов.
- Копия списка в массив и выбор середин тоже верны: O(n) по времени и O(n) по памяти. При чётной длине версия с массивом на странице берёт вторую середину, (start + end + 1) / 2.
- **Time:** O(n). **Space:** O(log n) для симметричной рекурсии. Версия с массивом занимает O(n).

**Examples:**
```text
1 → 2 → 3 → 4 → 5 → 6 → 7
root 4, left subtree from the first 3 nodes, right from the last 3
preorder: 4 2 1 3 6 5 7
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-linked-list-to-balanced-bst/

</details>

<details>
<summary>Создайте алгоритм Divide & Conquer для сортировки linked list.</summary>

### Создайте алгоритм Divide & Conquer для сортировки linked list.

**Definition:**
Сортировка слиянием — та, которую статья предпочитает для связного списка. Цепочка делится пополам, половины сортируются, затем узлы сшиваются заново.

**How it works:**
- У списка нет случайного доступа, поэтому статья говорит, что быстрая и пирамидальная сортировки на нём плохи.
- Разделение находит середину двумя указателями, сортирует обе стороны и сливает их, забирая меньшую голову. При равных ключах остаётся левый, как в слиянии массива (L[i] <= R[j]), которое устойчиво.
- **Time:** O(n log n) в лучшем, среднем и худшем случае. **Space:** алгоритм для массива занимает O(n) дополнительной памяти на временные массивы. Глубина рекурсии равного разбиения — O(log n).

**Examples:**
```text
sort(head):
    if head is null or head.next is null: return head
    left, right = split_in_half(head)
    return merge(sort(left), sort(right))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Реализуйте an efficient alGorithm to Search a 2D matrix II with a Divide & Conquer approach.</summary>

### Реализуйте an efficient alGorithm to Search a 2D matrix II with a Divide & Conquer approach.

**Definition:**
Каждая строка и каждый столбец отсортированы. Средняя ячейка сравнивается с ключом, затем ищутся только четверти, где он ещё может лежать.

**How it works:**
- Если середина меньше ключа, левый верхний угол его не содержит, поэтому поиск идёт в нижней полосе и в правой верхней четверти.
- Если середина больше, правый нижний угол его не содержит, поэтому поиск идёт в левой полосе и в правой верхней четверти.
- Если это три подзадачи размера n/2, рекуррентность T(n) = 3T(n/2) + O(1).
- **Time:** O(n^1.58) по основной теореме, для матрицы n×n. **Space:** O(log n).
- Проход «лестницей» от угла в родственном поиске по строкам и столбцам — O(n + m) по времени и O(1) по памяти. Это другой алгоритм.

**Examples:**
```text
search(mat, r1, r2, c1, c2, key):
    if r1 > r2 or c1 > c2: return false
    i = (r1 + r2) // 2
    j = (c1 + c2) // 2
    if mat[i][j] == key: return true
    if search upper-right quarter: return true
    if mat[i][j] < key: return search rows i+1..r2
    return search columns c1..j-1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/search-in-a-row-wise-and-column-wise-sorted-2d-array-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Код решения Divide & Conquer fOR Количество меньших чисел после себя в array.</summary>

### Код решения Divide & Conquer fOR Количество меньших чисел после себя в array.

**Definition:**
Для каждого индекса считается, сколько более поздних элементов меньше. Эти пары — инверсии. Слияние, которое считает инверсии, может записать их по индексу, а не только общей суммой.

**How it works:**
- Массив делится, затем сливаются две отсортированные половины. В правой половине более поздние индексы.
- Когда следующее правое значение меньше, оно меньше всех ещё не взятых левых, поэтому слияние инверсий сразу прибавляет (n1 − i). Эту прибавку кладут на каждый такой левый индекс.
- Слияние сортирует отрезок. Массив копируют, если исходный порядок нужно сохранить.
- Перебор пар: O(n²) по времени и O(1) дополнительной памяти.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
[5, 2, 6, 1]
5 is greater than 2 and 1 → 2
2 is greater than 1 → 1
6 is greater than 1 → 1
1 → 0
result [2, 1, 1, 0]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/counting-inversions/
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Используйте Divide & Conquer для вычисления Count of Range Sum в array для заданных нижней и верхней границ.</summary>

### Используйте Divide & Conquer для вычисления Count of Range Sum в array для заданных нижней и верхней границ.

**Definition:**
Считаются непрерывные подмассивы, сумма которых лежит в [l, r]. Префиксные суммы превращают это в пары: более поздний префикс минус более ранний попадает в [l, r].

**How it works:**
- Половины префиксов сортируются слиянием. Перед слиянием обе половины отсортированы, поэтому два указателя для каждого левого префикса считают, сколько правых лежит в [left + l, left + r].
- Складываются количества из двух половин и перекрёстные пары.
- Вложенные циклы — O(n²) по времени и O(1) по памяти.
- Другой ожидаемый метод на странице двигает окно и вычитает «сумма < l» из «сумма ≤ r»: O(n) по времени и O(1) по памяти.
- **Time:** O(n log n) для слияния префиксов. **Space:** O(n).

**Examples:**
```text
l = 4, r = 13, a = [2, 3, 5, 8]
subarrays in range: [2, 3], [2, 3, 5], [3, 5], [5], [5, 8], [8] → 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/number-of-subarrays-having-sum-in-a-given-range/

</details>

<details>
<summary>Найдите K наиболее часто встречающихся элементов в array, используя стратегию Divide & Conquer.</summary>

### Найдите K наиболее часто встречающихся элементов в array, используя стратегию Divide & Conquer.

**Definition:**
Считается, как часто встречается каждое значение, затем выбираются k наибольших частот. Выбор — Quickselect: разбиение вокруг опорного элемента и рекурсия только в ту сторону, где ещё лежит разрез.

**How it works:**
- Один проход строит частоту каждого различного значения.
- Quickselect ставит значения больше опорного слева. Если опорный элемент оказывается на индексе k − 1, первые k записей — топ. Иначе ищется только одна сторона.
- На этой странице худший случай — O(n²), средний — O(n log n). Дополнительная память — O(n) в худшем случае и O(log n) в среднем. Здесь n — число различных значений, которые делятся.
- Куча с той же страницы хранит k крайних за O(n log k) времени и O(k) памяти. Это другой алгоритм.
- **Time:** в среднем O(n log n) и в худшем O(n²) для шага Quickselect, как указано для этого разбиения. **Space:** в среднем O(log n), в худшем O(n), плюс таблица частот.

**Examples:**
```text
a = [1, 1, 1, 2, 2, 3], k = 2
counts: 1 → 3, 2 → 2, 3 → 1
partition by count; the cut at k - 1 keeps 1 and 2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/kth-smallest-largest-element-in-unsorted-array/

</details>

<details>
<summary>Определите K-й наибольший элемент в несортированном array с помощью метода Divide & Conquer.</summary>

### Определите K-й наибольший элемент в несортированном array с помощью метода Divide & Conquer.

**Definition:**
Quickselect делит массив так, что элементы больше опорного стоят слева. Индекс опорного сравнивается с k − 1, и ищется только сторона, где лежит k-й наибольший.

**How it works:**
- Если опорный элемент оказывается на индексе k − 1, это ответ.
- Полная сортировка сначала — O(n log n). Страница указывает для этой сортировки O(1) дополнительной памяти.
- Макс-куча из k кандидатов — ожидаемый подход на той же странице: O(n log k) по времени и O(k) по памяти.
- **Time:** O(n²) в худшем случае, в среднем O(n log n), и страница говорит, что это среднее обгоняет кучу. **Space:** O(n) на стек рекурсии в худшем случае, O(log n) в среднем.

**Examples:**
```text
a = [10, 5, 4, 3, 48, 6, 2, 33, 53, 10], k = 4
partition greater values to the left
stop when the pivot index is 3
→ 5
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/kth-smallest-largest-element-in-unsorted-array/

</details>

<details>
<summary>Реализуйте a Divide & Conquer alGorithm to compute the Number of Ships in a Rectangle on a 2D plane, given top-right and bottom-left coordinates of rectangles.</summary>

### Реализуйте a Divide & Conquer alGorithm to compute the Number of Ships in a Rectangle on a 2D plane, given top-right and bottom-left coordinates of rectangles.

**Definition:**
`hasShips(topRight, bottomLeft)` говорит, есть ли в прямоугольнике, включая границу, хотя бы один корабль. В целой точке не больше одного корабля. В прямоугольнике не больше 10 кораблей.

**How it works:**
- Пустой ответ `hasShips` — это 0. Такой квадрант не делится.
- Одна клетка с кораблём считается как 1.
- Иначе прямоугольник делится на четыре квадранта, и четыре количества складываются.
- **Time:** O(C × log max(m, n)). **Space:** O(log max(m, n)). C — число кораблей; m и n — длины сторон.

**Examples:**
```text
ships at (1,1), (2,2), (3,3), (5,5)
rectangle [0,0] .. [4,4]
(5,5) is outside → 3

count(topRight, bottomLeft):
    if the rectangle is inside-out: return 0
    if not hasShips: return 0
    if one cell: return 1
    split at the mid x and mid y
    return the sum of the four quadrants
```

**Sources:**
- https://github.com/doocs/leetcode/blob/main/solution/1200-1299/1274.Number%20of%20Ships%20in%20a%20Rectangle/README_EN.md

</details>

<details>
<summary>Сбалансируйте данный binary search tree в сбалансированный BST, используя Divide & Conquer.</summary>

### Сбалансируйте данный binary search tree в сбалансированный BST, используя Divide & Conquer.

**Definition:**
Симметричный обход BST — отсортированная последовательность. Среднее значение этой последовательности становится новым корнем, а две половины — двумя поддеревьями.

**How it works:**
- Узлы собираются симметричным обходом. Этот проход линеен по числу узлов.
- Построение из отсортированного массива берёт середину корнем, чтобы на каждой стороне было около половины значений. Стороны отличаются не больше чем на один узел, и дерево остаётся сбалансированным по высоте.
- Ту же форму можно собрать очередью. Рекурсивная и очередная версии на странице обе O(n).
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
inorder = [1, 2, 3, 4, 5, 6, 7]
root = 4
left subtree built from [1, 2, 3], right from [5, 6, 7]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-array-to-balanced-bst/

</details>

<details>
<summary>Создайте SORted array с помощью серии инструкций, используя двоичное индексированное дерево Divide & Conquer AND.</summary>

### Создайте SORted array с помощью серии инструкций, используя двоичное индексированное дерево Divide & Conquer AND.

**Definition:**
`instructions[i]` вставляются слева направо. Цена вставки — минимум из того, сколько уже вставленных значений строго меньше и сколько строго больше. Сумма цен возвращается по модулю 10^9 + 7.

**How it works:**
- Дерево Фенвика (двоичное индексированное дерево) хранит, сколько раз каждое значение уже вставлено.
- `query(num - 1)` — число строго меньших. `i - query(num)` — число строго больших, потому что уже вставлено i значений, а `query(num)` включает равные.
- Прибавляется `min(smaller, greater)`, затем `update(num, 1)`.
- Просмотр уже вставленных значений каждый раз — O(N²).
- **Time:** O(N log M): каждое обновление и запрос — O(log M), M — максимальное значение. **Space:** O(M).

**Examples:**
```text
instructions = [1, 5, 6, 2]
insert 1 → cost 0
insert 5 → 1 smaller, 0 greater → 0
insert 6 → 2 smaller, 0 greater → 0
insert 2 → 1 smaller, 2 greater → min = 1
total 1
```

**Sources:**
- https://algomap.io/question-bank/create-sorted-array-through-instructions

</details>

<details>
<summary>Используйте подход Divide & Conquer для сортировки array.</summary>

### Используйте подход Divide & Conquer для сортировки array.

**Definition:**
Сортировка слиянием делит массив, пока каждый кусок не станет одним элементом, затем сливает эти отсортированные куски.

**How it works:**
- T(n) = Θ(1) при n = 1 и T(n) = 2T(n/2) + Θ(n) иначе. Член Θ(n) — это слияние.
- **Time:** O(n log n) в лучшем, среднем и худшем случае. **Space:** O(n) на временные массивы.
- Слияние устойчиво: при равных головах берётся левая. Половины можно сортировать независимо, поэтому статья называет алгоритм естественно параллельным.

**Examples:**
```text
[38, 27, 43, 10]
→ [38, 27] and [43, 10]
→ [27, 38] and [10, 43]
→ [10, 27, 38, 43]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Разработайте и внедрите решение Divide & Conquer для создания красивого array.</summary>

### Разработайте и внедрите решение Divide & Conquer для создания красивого array.

**Definition:**
Красивый массив длины n — перестановка 1..n, в которой ни одно значение не стоит строго между двумя другими, для которых оно является средним: нет i < k < j с 2 * nums[k] = nums[i] + nums[j].

**How it works:**
- Если A красивый, красивы и нечётный образ 2·A − 1, и чётный образ 2·A.
- Нечётные строятся из красивого массива размера (n + 1) / 2, чётные — из массива размера n / 2, затем нечётные и чётные склеиваются.
- Один элемент, [1], красивый.
- Перебор всех перестановок — O(n! × n³) по времени и O(n) по памяти.
- **Time:** O(n log n). **Space:** O(n log n).

**Examples:**
```text
beautiful(1) = [1]
beautiful(4) = odds from 2, evens from 2 → [1, 3, 2, 4]
beautiful(5) = odds from 3, evens from 2 → [1, 5, 3, 2, 4]

beautiful(n):
    if n == 1: return [1]
    odds = [2*x - 1 for x in beautiful((n + 1) // 2)]
    evens = [2*x for x in beautiful(n // 2)]
    return odds + evens
```

**Sources:**
- https://www.tutorialspoint.com/practice/beautiful-array.htm

</details>

<details>
<summary>Решите the longest increasing subsequence II problem where updates and queries happen in real-time, using a Divide & Conquer strategy combined with advanced data structures.</summary>

### Решите the longest increasing subsequence II problem where updates and queries happen in real-time, using a Divide & Conquer strategy combined with advanced data structures.

**Definition:**
Длину наибольшей строго возрастающей подпоследовательности можно вести онлайн. Каждое новое значение спрашивает лучшую длину среди меньших значений и затем публикует свою.

**How it works:**
- Сжатие координат отображает значения на 1..N, чтобы дерево имело размер N.
- Двоичное индексированное дерево хранит лучшую длину в каждом сжатом значении. `query(index - 1)` — лучшая длина среди строго меньших. `update` пишет длину + 1 и идёт по дереву шагом `index += index & -index`.
- Массив обрабатывается слева направо. На элемент — один запрос и одно обновление.
- **Time:** O(N log N). **Space:** O(N).

**Examples:**
```text
[6, 5, 1, 3, 2, 4, 8, 7]
lengths become 1, 1, 1, 2, 2, 3, 4, 4
answer 4   // for example 1, 3, 4, 7
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/longest-increasing-subsequence-using-bit/

</details>

# CSS — Core

Как добавляется CSS, каскад, селекторы и блочная модель.

[ ← Вернуться в центр](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Sections

- [Core](#core)
- [Selectors & Cascade](#selectors-cascade)
- [Box Model & Display](#box-model-display)

---

<a name="fundamentals"></a>

## Core

<details>
  <summary>Ways to add CSS</summary>

### Ways to add CSS

**Definition:**

CSS может быть связан извне, встроен в `<style>`или применяется встроенно к элементам; внешние таблицы стилей используются по умолчанию.

| Метод | Синтаксис | Вариант использования |
|--------|--------|----------|
| **Внешний** | `<link rel="stylesheet" href="styles.css">` | Производственное значение по умолчанию — кэшируемое, повторное использование |
| **Внутренний** | `<style>` in `<head>` | Одностраничные прототипы, критический CSS |
| **Встроенный** | `style="..."` на элементе | Избегайте, за исключением динамических единичных случаев; специфичность 1000 |

**How it works:** предпочитать `<link rel="stylesheet">` через CSS `@import` — `@import` блокирует параллельные загрузки и добавляет цепочку последовательных запросов. Предварительная загрузка критического CSS: `<link rel="preload" href="critical.css" as="style">`.

**Sources:** [MDN — Использование CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/How_CSS_is_structured)

</details>

<details>
  <summary>@import anti-patterns</summary>

### @import anti-patterns

**Definition:** `@import` загружает другую таблицу стилей из CSS — удобно, но вредно для производительности и удобства обслуживания по сравнению с `<link>` теги или импорт пакетов.

**How it works:**

- Браузер должен проанализировать родительский CSS перед обнаружением `@import` URL-адреса — сериализует загрузки.
- `@import` внутри `<style>` или поздняя загрузка CSS задерживает все, что он получает.
- Вложенный `@import` цепи умножают задержку; порядок зависит от позиции источника.

**When to use:** Избегайте в производстве — используйте `<link rel="stylesheet">`, запись бандлера (`import './base.css'`), or `@layer` для заказа. Устаревшее исключение: только печать `@import url("print.css") print;` когда изолирован.

**Pluses and minuses:**

| + | − |
|------|------|
| Разделить файлы без изменений HTML | Блокирует параллельную выборку и `<link>` |
| Работает в простых проектах CSS | Трудно отследить в каскаде DevTools |

| Бандлеры могут дублироваться, если их смешивать с импортом JS |

**Examples:**

```css
/* Anti-pattern — blocks rendering */
@import url("reset.css");
@import url("components.css"); /* waits for reset to parse */

/* Prefer in HTML */
/* <link rel="stylesheet" href="reset.css"> */
/* <link rel="stylesheet" href="components.css"> */
```

**Sources:** [MDN — @import](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)

</details>

<details>
  <summary>CSS rule syntax</summary>

### CSS rule syntax

**Definition:** Правило CSS объединяет селектор с блоком объявлений `property: value` пары, стилизующие соответствующие элементы.

**Examples:**

```css
selector {
  property: value;
  another-property: value;
}
```

Множественные селекторы (группировка): `h1, .price { color: navy; }`  
Цепочка (И): `.card.featured { }` — элемент должен соответствовать всем частям.

**Sources:** [MDN — синтаксис CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax)

</details>

<details>
  <summary>Global keywords</summary>

### Global keywords

**Definition:**

Глобальные ключевые слова CSS (`inherit`, `initial`, `unset`, `revert`) контролировать, как значения свойств сбрасываются или каскадируются.

| Ключевое слово | Поведение |
|---------|----------|
| `inherit` | Взять вычисленное значение из родительского |
| `initial` | Сбросить настройки CSS по умолчанию |
| `unset` | `inherit` если наследуется, иначе `initial` |
| `revert` | Откат к таблице стилей пользовательского агента |
| `revert-layer` | Откат в пределах текущего `@layer` |

**How it works:**

- `inherit` — дочерний элемент принимает **вычисленное** значение родителя для этого свойства.
- `initial` — Начальное значение всего CSS (не всегда значение по умолчанию UA для этого элемента).
- `unset` — наследуемые свойства → `inherit`; другие → `initial`.
- `revert` — Значение таблицы стилей UA для этого элемента/свойства.

**When to use:** `all: unset` на кнопках перед кастомным стилем; `color: revert` восстановить цвета ссылок UA; `display: revert` чтобы отменить авторские изменения семантических элементов.

**Pluses and minuses:**

| + | − |
|------|------|
| Сброс без дублирования правил | `initial` ≠ элемент по умолчанию (например. `display` on `<div>`) |
| `all: unset` быстро раздевает авторские стили | `revert` поддержка все еще развивается в старых браузерах |

**Examples:**

```css
button {
  all: unset;
  cursor: pointer;
}

a { color: revert; }
```

**Sources:** [MDN — значения CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Values_and_Units)

</details>

<details>
  <summary>Code style — property order</summary>

### Code style — property order

**Definition:**

Группировка свойств CSS в последовательном порядке (позиция → поле → типографика → оформление → анимация) улучшает читаемость и скорость просмотра.

Групповые свойства для читабельности (позиционирование → блочная модель → типографика → оформление → анимация → разное):

**Examples:**

```css
.card {
  /* Positioning */
  position: relative;
  z-index: 1;

  /* Box model */
  display: flex;
  width: 100%;
  max-width: 320px;
  margin: 0 auto;
  padding: 1rem;

  /* Typography */
  font: 400 1rem/1.5 system-ui, sans-serif;
  color: #333;

  /* Decoration */
  background: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;

  /* Animation */
  transition: box-shadow 0.2s ease;

  /* Misc */
  will-change: auto;
}
```

**Sources:**

[Стиль CSS-кода Netology](https://netology-university.bitbucket.io/codestyle/css/) · [CSS для JS](https://learn.javascript.ru/css-for-js)

> `float` в старых руководствах по стилю является устаревшим — для макета отдавайте предпочтение Flexbox/Grid.

[Стиль кода Netology CSS](https://netology-university.bitbucket.io/codestyle/css/)

</details>

<details>
  <summary>Reset.css vs Normalize.css</summary>

### Reset.css vs Normalize.css

**Definition:**

Сброс настроек браузера по умолчанию до чистого листа; Normalize сохраняет полезные настройки по умолчанию, одновременно исправляя несогласованность между браузерами.

* **Сброс** — удаляет большинство стилей браузера по умолчанию с чистого листа.
* **Нормализация** — сохраняет полезные настройки по умолчанию, исправляет несогласованность между браузерами.

В современных проектах часто используется минимальный сброс или база фреймворка (Tailwind Preflight и т.п.).

---

**Sources:** [Normalize.css](https://necolas.github.io/normalize.css/)

</details>

<a name="selectors-cascade"></a>

### Cascade & specificity

**Definition:**

Каскад разрешает конфликтующие правила по происхождению, важности, специфичности и порядку источников; Наследование передает некоторые свойства от родителя к дочернему элементу.

| Концепция | Роль |
|---------|------|
| **Каскад** | Выбирает победившую декларацию в случае конфликта правил |
| **Специфика** | Оценка кортежа для каждого селектора — более конкретные победы |
| **Наследование** | `color`, `font-*`, `line-height` стекать вниз; макет реквизита нет |
| **`@layer`** | Явный порядок слоев превосходит специфичность авторских стилей |

**When to use:**

Каждая таблица стилей — понимание каскада предотвращает войны за специфичность и `!important` хаки.

В соответствии `style=""` и селекторы идентификаторов для стилизации — они повышают специфичность и ухудшают удобство обслуживания.

**Examples:**

```css
@layer reset, components, utilities;

@layer components {
  .button { background: #2563eb; }
}

.card .button { background: #1d4ed8; } /* wins: unlayered beats layered */
```

**Thoughts:** Конвейер рендеринга — CSS создает CSSOM перед макетом → [1_html.md](./1_html.md).

**Sources:** [MDN — Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade) · [Специфика](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

### Box model & layout

**Definition:**

Каждый элемент представляет собой блок (содержимое → отступ → граница → поле); системы компоновки позиционируют и распределяют эти блоки в 1D (гибкий) или 2D (сетка).

| Система | Лучшее для |
|--------|----------|
| **Флексбокс** | Панели инструментов, строки навигации, центрирование, 1D-выравнивание |
| **Сетка** | Оболочки страниц, информационные панели, адаптивные карточные матрицы |
| **`border-box`** | Предсказуемый размер — `width` включает отступы и границы |
| **БФК** | Содержит поплавки, предотвращает схлопывание полей |

**When to use:** Flex для компонентов; Сетка для структуры страницы; оба вместе в производственных макетах.

**Examples:**

```css
*, *::before, *::after { box-sizing: border-box; }

.app {
  display: grid;
  min-height: 100dvh;
  grid-template-columns: 240px 1fr;
  grid-template-areas: "sidebar main";
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
```

**Thoughts:** Изменения макета вызывают перекомпоновку в конвейере рендеринга → [1_html.md](./1_html.md).

**Sources:** [MDN — коробочная модель](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model) · [Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout) · [Сетка](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)

### Responsive & modern CSS

**Definition:**

Адаптивный CSS адаптирует макеты к области просмотра и размеру контейнера, используя гибкие единицы измерения, запросы мультимедиа/контейнера и современные функции, такие как настраиваемые свойства и вложение.

| Особенность | Цель |
|---------|---------|
| **Медиа-запросы** | Точки останова в окне просмотра, `prefers-reduced-motion`, темный режим |
| **Запросы контейнера** | Компонент адаптируется к ширине родительского элемента, а не к области просмотра |
| **`clamp()`** | Плавная типографика и интервалы без точек останова |
| **Пользовательские свойства** | Темы во время выполнения; JS-обновляемые токены |
| **Современный CSS** | `@scope`, позиционирование привязки, анимация с помощью прокрутки |

**When to use:** Мобильные устройства `min-width` запросы; `@container` для многократно используемых компонентов на боковых панелях и информационных панелях.

**Examples:**

```css
:root {
  --space-md: 1rem;
  --text: clamp(0.875rem, 0.8rem + 0.25vw, 1.125rem);
}

.card-wrap { container-type: inline-size; }

@container (min-width: 400px) {
  .card { display: grid; grid-template-columns: 120px 1fr; }
}

@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}
```

**Sources:** [web.dev — Изучите CSS](https://web.dev/learn/css/) · [MDN — Контейнерные запросы](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)

## Selectors & Cascade

<details>
  <summary>Selector types</summary>

### Selector types

**Definition:**

Селекторы CSS выбирают элементы по типу, классу, идентификатору, атрибуту или псевдоклассу/элементу; каждый тип вносит свою специфичность.

| Тип | Пример | Специфика |
|------|---------|-------------|
| Универсальный | `*` | 0 |
| Тип | `p`, `h1` | 0,0,1 |
| Класс | `.btn` | 0,1,0 |
| ID | `#header` | 1,0,0 |
| Атрибут | `[type="email"]`, `[href^="https"]` | 0,1,0 |
| Псевдо-класс | `:hover`, `:nth-child(2)` | 0,1,0 |
| Псевдоэлемент | `::before`, `::selection` | 0,0,1 (действует как тип) |

**Sources:** [MDN — Селекторы](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors)

</details>

<details>
  <summary>Combinators</summary>

### Combinators

**Definition:**

Комбинаторы определяют отношения между селекторами — потомок, дочерний элемент, соседний одноуровневый элемент и общий одноуровневый элемент.

| Комбинатор | Значение | Пример |
|------------|---------|---------|
| Потомок (пробел) | Любой вложенный потомок | `.nav li` |
| Ребенок `>` | Только прямой ребенок | `.nav > li` |
| Соседний `+` | Непосредственный следующий брат | `h2 + p` |
| Общий `~` | Любой следующий брат/сестра | `h2 ~ p` |

**Sources:** [MDN — Комбинаторы](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Combinators)

</details>

<details>
  <summary>Class vs ID</summary>

### Class vs ID

**Definition:**

Классы — это многоразовые крючки для стилей; Идентификаторы уникальны для каждой страницы и обладают более высокой специфичностью — для стилизации отдавайте предпочтение классам.

* **ID** — уникальный для каждой страницы; высокая специфичность; предпочитаю использовать JS-хуки или фрагменты, а не стили
* **Класс** — многоразовый; кебаб-кейс (`card-title`); несколько на элемент (через пробел)

**How it works:**

- Селектор идентификатора (`#header`) — специфика `(1,0,0)`; документ-уникальный.
- Селектор классов (`.nav`) — специфика `(0,1,0)`; многоразовый; несколько на каждый элемент.
- Идентификаторы действительны для целевых фрагментов (`#section`), `aria-labelledby`, `label for=`; классы для стилизации хуков.

**When to use:** Стиль с классами; зарезервируйте идентификаторы для якорей и ссылок ARIA. Избегать `#id .class` цепочки для достижения специфичности — сгладьте селекторы или используйте `@layer` вместо.

**Pluses and minuses:**

| + (класс) | − (ID для стиля) |
|--------------|----------------------|
| Низкая специфичность, простота переопределения | Идентификаторы создают войны специфичности |
| Составные модификаторы (`btn--primary`) | По одному на страницу — хрупкий для компонентов |
| Удобные для команды соглашения об именах | Трудно провести масштабный рефакторинг |

**Examples:**

```html
<header id="main-header" class="site-header">...</header>
<nav class="nav nav--compact">...</nav>
```

**Sources:** [MDN — селекторы идентификаторов](https://developer.mozilla.org/en-US/docs/Web/CSS/ID_selectors)

</details>

<details>
  <summary>Modern functional selectors</summary>

### Modern functional selectors

**Definition:** `:is()`, `:where()`, `:not()`, и `:has()` группировать или фильтровать селекторы с разными правилами специфичности.

**Examples:**

```css
/* :is() — groups selectors; specificity = highest argument */
:is(h1, h2, h3) { margin-block: 0; }

/* :where() — same grouping, ZERO specificity */
:where(h1, h2, h3) { margin-block: 0; }

/* :not() — exclusion */
li:not(:last-child) { border-bottom: 1px solid #eee; }

/* :has() — parent selector ("has selector") */
.card:has(img) { padding-top: 0; }
.form:has(:invalid) .submit { opacity: 0.5; }
```

**Sources:** [MDN — :has()](https://developer.mozilla.org/en-US/docs/Web/CSS/:has)

</details>

<details>
  <summary>Specificity calculation</summary>

### Specificity calculation

**Definition:**

Специфичность — это оценка кортежа (встроенная, идентификаторы, классы/атрибуты/псевдоклассы, типы/псевдоэлементы), которая определяет, какое правило победит в каскаде.

Количество (встроенное, идентификаторы, классы/атрибуты/псевдоклассы, типы/псевдоэлементы):

**How it works:**

более позднее правило в исходном порядке побеждает.

> Старая ментальная модель «1-10-100» является упрощением. Используйте кортежи реальной специфичности или панель DevTools «Вычисляемые».

| Селектор | Примечание по особенностям |
|----------|------------------|
| `:not(.a)` | `:not()` сам по себе равен 0; аргумент `.a` добавляет `(0,1,0)` |
| `:is(.a, #x)` | Принимает высший аргумент → `#x` = `(1,0,0)` |
| `:where(.a, #x)` | Всегда `(0,0,0)` |
| `::before` | Считается псевдоэлементом типа. `(0,0,1)` |

В пределах одного и того же происхождения многослойные стили превосходят многослойные независимо от специфики. Среди многослойных стилей сначала побеждает порядок слоев, а затем конкретность. `!important` инверсии применяются в зависимости от происхождения — автор `!important` у автора нормально, но у пользователя `!important` может победить автора `!important`.

```
Cascade resolution (simplified):
  1. Origin + importance (user !important > author !important > author normal > user normal > UA)
  2. @layer order (later layer wins; unlayered beats all layers)
  3. Specificity tuple
  4. Source order
```

**Examples:**

```
#nav .item a:hover     →  1,1,2
.btn.btn--primary     →  0,2,0
p                     →  0,0,1
style="" attribute    →  1000 (inline)
!important             →  beats normal declarations in same origin
```

**Sources:** [MDN — Специфика](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

</details>

<details>
  <summary>Cascade origins</summary>

### Cascade origins

**Definition:**

Каскад разрешает конфликты между таблицами стилей пользовательского агента, пользователя и автора до определения специфики и порядка источника.

| Происхождение | Источник | Типичные примеры |
|--------|--------|------------------|
| **Пользовательский агент (UA)** | Таблица стилей браузера по умолчанию | `display: block` on `div`, смелый `h1` |
| **Пользователь** | Пользовательские настройки/расширения | Режим высокой контрастности, пользовательские шрифты |
| **Автор** | Ваш CSS (связанный, встроенный, встроенный) | Таблицы стилей приложений |

**Наследование** отделено от каскада — унаследованные значения передаются вниз по дереву DOM, но по-прежнему участвуют в каскаде при переопределении на дочернем элементе.

**`revert`** выполняет откат к таблице стилей UA для этого свойства; `revert-layer` откатывается в пределах текущего `@layer`.

**Sources:** [MDN — Каскад](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

</details>

<details>
  <summary>Inheritance</summary>

### Inheritance

**Definition:** Некоторые свойства CSS передают свои вычисленные значения от родительского элемента к дочернему; другие этого не делают. Наследование отделено от каскада — дочерний элемент может наследовать значение, которое все равно проигрывает более конкретному правилу автора.

**How it works:**

- Наследственный: `color`, `font-*`, `line-height`, `text-align`, `visibility`, пользовательские свойства (`--*`) и т. д.
- Ненаследуемые: `margin`, `padding`, `border`, `width`, `display`, `background`, и т. д.
- `inherit` Ключевое слово обеспечивает наследование даже для ненаследуемых свойств.
- `initial` / `unset` / `revert` взаимодействовать с наследованием по правилам свойства.

**When to use:** Набор `font-family` / `color` on `body` or `:root`; использовать `color: inherit` на кнопках/ссылках; не ожидай `width` or `margin` для стекания — задайте явно или используйте контейнеры макета.

**Pluses and minuses:**

| + | − |
|------|------|
| Типографика/цветовая тема от `:root` | Легко забыть, что наследуется, а что нет |
| Пользовательские свойства естественным образом каскадируются | Унаследовано `visibility: hidden` скрывает всех потомков |
| Меньше дублирования стилей текста | Свойства макета должны быть установлены для каждого элемента |

**Examples:**

```css
:root {
  --brand: #2563eb;
  font-family: system-ui, sans-serif;
  color: #111;
}

.card {
  color: inherit;           /* explicit inherit on non-default */
  border: 1px solid #e5e7eb; /* does NOT inherit from parent */
}
```

**Sources:** [MDN — Наследование](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Inheritance)

</details>

<details>
  <summary>@layer (cascade layers)</summary>

### @layer (cascade layers)

**Definition:**

`@layer` объявляет явные каскадные уровни, поэтому сбросы, компоненты и утилиты складываются в предсказуемом порядке.

Явное управление порядком каскада — полезно для систем проектирования, сброса и утилит:

**Examples:**

```css
@layer reset, base, components, utilities;

@layer reset {
  * { margin: 0; }
}

@layer utilities {
  .hidden { display: none !important; }
}
```

Неслоистые стили превосходят многослойные стили. Более поздние слои превосходят более ранние слои в том же источнике.

**Sources:** [MDN — @layer](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer)

</details>

<details>
  <summary>Pseudo-classes</summary>

### Pseudo-classes

**Definition:**

Псевдоклассы выбирают элементы в определенном состоянии или структурной позиции (`:hover`, `:nth-child`, `:focus-visible`).

| Категория | Селекторы | Вариант использования |
|----------|-----------|----------|
| **Взаимодействие** | `:hover`, `:active` | Указатель обратной связи |
| **Фокус** | `:focus`, `:focus-visible`, `:focus-within` | Фокус клавиатуры и мыши; родитель, когда ребенок сосредоточен |
| **Структура** | `:first-child`, `:last-child`, `:nth-child(n)`, `:only-child` | Положение среди всех братьев и сестер |
| **Структура типа** | `:first-of-type`, `:nth-of-type(n)` | Позиция только среди братьев и сестер с одинаковым тегом |
| **Отрицание** | `:not(.disabled)` | Исключить совпадения |
| **Состояние формы** | `:checked`, `:disabled`, `:invalid`, `:valid`, `:placeholder-shown`, `:required` | Собственный стиль проверки |
| **Навигация** | `:target` | Выделение фрагмента URL |
| **Родитель** | `:has(.error)` | Родительский стиль на основе потомка |

**How it works:** `:focus` срабатывает при любом фокусе (включая щелчок мышью); `:focus-visible` только тогда, когда UA определяет, что фокус должен быть виден (клавиатура). Использовать `:focus-visible` для контурных колец; никогда не удаляй `:focus` без замены.

**Examples:**

```html
<div><p>A</p><span>B</span><p>C</p></div>
```

* `p:nth-child(2)` — нет совпадений (`span` это второй ребенок)
* `p:nth-of-type(1)` — первое совпадение `<p>` (A)
* `p:nth-of-type(2)` — соответствует второму `<p>` (C)

**Sources:** [MDN — Псевдоклассы](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)

</details>

<details>
  <summary>Pseudo-elements</summary>

### Pseudo-elements

**Definition:**

Стиль псевдоэлементов генерирует части элемента (`::before`, `::after`, `::selection`).

Целевые части элемента (используйте `::`):

* `::before`, `::after` — сгенерированный контент (`content` необходимый)
* `::first-letter`, `::first-line`
* `::selection` — выбранный пользователем текст
* `::placeholder` — введите текст-заполнитель

**Examples:**

```css
.tooltip::after {
  content: attr(data-tip);
  position: absolute;
}
```

**Sources:** [MDN — Псевдо-элементы](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)

</details>

<details>
  <summary>attr() function</summary>

### attr() function

**Definition:**

- `attr()` функция считывает значение атрибута HTML для использования в CSS, обычно в `content` на псевдоэлементах.

Считывает значение атрибута HTML для использования в CSS (обычно с `content`):

**How it works:**

- `attr(name)` считывает значение атрибута как строку для `content`и в современных браузерах для других свойств (`attr(data-label type(<string>))`).
- Напечатано `attr()` (Уровень значений CSS 5) позволяет `type(<color>)`, `type(<length>)`и т. д. — проверьте поддержку браузера.
- Пустые/отсутствующие атрибуты приводят к пустой строке в `content`.

**When to use:** Подсказки по внешним ссылкам в печатном виде; декоративные этикетки из `data-*`; счетчиковые значки. Всегда сочетайте декоративные `attr()` контент с доступными именами.

**Pluses and minuses:**

| + | − |
|------|------|
| DRY-подсказки/метки из данных HTML | Ограниченная поддержка свойств без типизированного `attr()` |
| Никакого JS для декоративного псевдоконтента | Программы чтения с экрана могут не читать `::after` контент — добавить `aria-label` |
| Хорошо подходит для расширения ссылок на печать | Безопасность: никогда не внедряйте неэкранированный пользовательский HTML-код через атрибуты |

**Examples:**

```css
a[href^="http"]::after {
  content: " (" attr(href) ")";
}
```

**Sources:** [MDN — attr()](https://developer.mozilla.org/en-US/docs/Web/CSS/attr)

</details>

<a name="box-model-display"></a>

## Box Model & Display

<details>
  <summary>CSS box model</summary>

### CSS box model

**Definition:**

Каждый элемент представляет собой прямоугольный блок: содержимое, отступы, границы и поля — основа определения размера макета.

Каждый элемент представляет собой прямоугольный блок: **контент → отступ → граница → поле**.

По умолчанию, `width` / `height` применяется только к **полю содержимого**; отступы и границы увеличивают общий размер.

**How it works:** [Введение в блочную модель CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model)

**Examples:**

```
┌──────────── margin ────────────┐
│  ┌──────── border ────────┐    │
│  │  ┌──── padding ────┐   │    │
│  │  │    content      │   │    │
│  │  └─────────────────┘   │    │
│  └────────────────────────┘    │
└────────────────────────────────┘
```

**Sources:** [MDN — блочная модель](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model)

</details>

<details>
  <summary>box-sizing</summary>

### box-sizing

**Definition:** `box-sizing` контролирует, `width`/`height` включить отступы и границу (`border-box`) или только контент (`content-box`).

**Examples:**

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

| Значение | `width` включает |
|-------|------------------|
| `content-box` | только контент (по умолчанию) |
| `border-box` | контент + отступы + граница |

Использовать `border-box` во всем мире, чтобы сделать калибровку предсказуемой. `padding-box` существует, но используется редко.

**Sources:** [MDN — размер коробки](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

</details>

<details>
  <summary>Margin & padding</summary>

### Margin & padding

**Definition:**

Отступы — это пространство внутри границы; маржа — это пространство снаружи. Вертикальные поля между одноуровневыми блоками могут схлопываться.

* **Padding** — пространство внутри рамки; влияет на фон; не рушится
* **Маржа** — пространство за пределами границы; может рухнуть

**How it works:**

вертикальные поля соседних одноуровневых блоков сжимаются до большего значения. Не применяется к гибким элементам/сеткам, плавающим элементам или горизонтальным полям.

* **Родитель + первый/последний дочерний элемент** — верхнее поле первого дочернего элемента может совпадать с верхним полем родительского элемента; то же самое для дна
* **Пустой блок** — верхнее и нижнее поля пустого блока сворачиваются в одно.
* **Предотвращение** — отступы/границы родительского элемента, `overflow: auto`, `display: flow-root`или гибкая/сетка на родительских остановках сворачивается

визуально вытягивать элементы; может нарушить макет — используйте экономно.

**Sources:** [MDN — маржа](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)

</details>

<details>
  <summary>Containing block</summary>

### Containing block

**Definition:**

Содержащий блок — это ссылочный блок, используемый для вычисления положения элемента и размеров в процентах.

**Содержащий блок** — это поле, используемое для расчета размера и положения элемента (`top`, `left`, `width`, `height`, проценты).

| `position` | Содержащий блок |
|------------|---------------------|
| `static` / `relative` | **Окно содержимого** ближайшего предка блока |
| `absolute` | Ближайший **позиционированный** предок (`relative`, `absolute`, `fixed`, `sticky`) край заполнения; если нет, то начальный содержащий блок (окно просмотра) |
| `fixed` | Область просмотра (или предок преобразования/фильтра в некоторых браузерах) |
| `sticky` | Окно содержимого ближайшего предка прокрутки |

**How it works:**

`position: relative` на родителе не меняет поток, но **становится** содержащим блоком для `position: absolute` дети.

[Содержащий блок](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

**Examples:**

```css
.parent { position: relative; }
.child  { position: absolute; inset: 0; } /* fills .parent, not viewport */
```

Процент `width`/`height` разрешается по ширине/высоте содержащего блока — если содержащий блок не имеет явной высоты, `height: 100%` терпит неудачу.

**Sources:** [MDN — Содержащий блок](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

</details>

<details>
  <summary>Intrinsic & extrinsic sizing</summary>

### Intrinsic & extrinsic sizing

**Definition:**

Внутренний размер определяется размером содержимого (`min-content`, `max-content`); внешний размер навязывается извне (`width`, `flex`).

| Ключевое слово | Значение |
|---------|---------|
| `min-content` | Наименьший размер без переполнения (самое длинное слово/самое широкое неразрывное содержимое) |
| `max-content` | Размер, необходимый для размещения всего содержимого в одной строке |
| `fit-content` | `min(max-content, max(min-content, stretch))` — сжимать, но не ниже минимального содержания |
| `auto` | Зависит от браузера; в flex/grid часто означает `minmax(auto, max-content)` |

**How it works:** по умолчанию `min-width: auto` на гибких элементах предотвращает сжатие ниже содержимого — используйте `min-width: 0` or `minmax(0, 1fr)` в сетке, чтобы разрешить усечение.

**Examples:**

```css
.sidebar { width: min(240px, 25vw); }
.card    { width: fit-content; }
.grid-col { grid-template-columns: minmax(0, 1fr); } /* allow shrink below content */
```

**Внешний размер** — размер, заданный извне (`width: 300px`, `flex: 1`). **Внутренний размер** – размер, определяемый на основе содержимого (`width: max-content`, `width: auto` на встроенном блоке).

**Sources:** [MDN — минимальный контент](https://developer.mozilla.org/en-US/docs/Web/CSS/min-content)

</details>

<details>
  <summary>Block formatting context (BFC)</summary>

### Block formatting context (BFC)

**Definition:** BFC — это изолированная область макета, в которой блоки блоков располагаются вертикально и содержат плавающие элементы.

**How it works:**

* `overflow` кроме `visible` (`auto`, `hidden`, `scroll`)
* `display: flow-root` (предпочтительно — отсутствие побочных эффектов)
* `display: flex` / `grid` на контейнере
* `position: absolute` / `fixed`
* `float` кроме `none`

* Содержит плавающие дочерние элементы (предотвращает схлопывание родительской высоты)
* Предотвращает схлопывание полей с внешними элементами.
* Изолирует макет для независимого укладки по z-индексу.

**Examples:**

```css
.clear-float-context {
  display: flow-root; /* modern clearfix replacement */
}
```

**Thoughts:** Изолированная область макета, в которой блоки блоков располагаются вертикально и содержат плавающие элементы.

**Sources:** [MDN — BFC](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)

</details>

<details>
  <summary>display</summary>

### display

**Definition:**

- `display` Свойство определяет, как элемент участвует в макете — блочный, встроенный, гибкий, сеточный или нет.

| Значение | Поведение |
|-------|----------|
| `block` | Новая линия, доступна полная ширина |
| `inline` | Потоки с текстом, без ширины/высоты |
| `inline-block` | Линейный поток, применяется блочная модель |
| `flex` | Гибкий контейнер |
| `grid` | Сетка-контейнер |
| `none` | Удален из дерева макета и доступности |
| `contents` | Коробка исчезает; дети остаются |
| `flow-root` | Создает BFC; содержит поплавки |

**Sources:** [MDN — отображение](https://developer.mozilla.org/en-US/docs/Web/CSS/display)

</details>

<details>
  <summary>display: contents</summary>

### display: contents

**Definition:** `display: contents` заставляет поле элемента исчезнуть — его дочерние элементы участвуют в макете родителя, как если бы оболочки там не было.

**How it works:**

- Элемент не отображается как прямоугольник; дочерние элементы поднимаются в родительский контекст форматирования.
- Полезно для семантических оберток (`<li>` вокруг гибких детей, исправлений таблиц) без дополнительных блоков макета.
- Псевдоэлементы (`::before`/`::after`) на `contents` элемент все еще может генерировать блоки.

**When to use:** Обертки элементов списка в гибких/сетчатых макетах; исправления доступности таблиц. **Избегайте** интерактивных элементов или элементов-памятников (`<button>`, `<nav>`, `<main>`) — используйте реальное значение макета или добавьте явные роли ARIA для дочерних элементов.

**Pluses and minuses:**

| + | − |
|------|------|
| Семантический HTML без штрафов за макет | **Доступность:** элемент удален из дерева во многих браузерах |
| Устраняет проблемы с участием таблиц и гибких программ | Нет коробки = нет `background`, `border`, `padding` на обертке |
| Очистка сеточных/гибких списков элементов | Непредсказуемо с позиционированными потомками |

**Examples:**

```css
/* Semantic <li> wrappers in a flex toolbar */
.toolbar > li {
  display: contents;
}

/* a11y warning — screen readers may skip the <li> box */
/* Prefer restructuring markup or use role="listitem" on children if needed */
```

**Sources:** [MDN — отображение: содержимое](https://developer.mozilla.org/en-US/docs/Web/CSS/display#contents)

</details>

<details>
  <summary>visibility vs display</summary>

### visibility vs display

**Definition:**

`display: none` удаляет элемент из макета; `visibility: hidden` сохраняет свое пространство, но скрывает живопись.

| Недвижимость | В макете? | Нарисовано? | События? |
|----------|------------|----------|---------|
| `display: none` | Нет | Нет | Нет |
| `visibility: hidden` | Да | Нет | Нет |
| `opacity: 0` | Да | Нет* | Да (если только `pointer-events: none`) |

**Sources:** [MDN — видимость](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility)

</details>

<details>
  <summary>overflow, aspect-ratio, object-fit</summary>

### overflow, aspect-ratio, object-fit

**Definition:** `overflow` клипы или прокрутки, переполняющие контент; `aspect-ratio` и `object-fit` управлять пропорциями медиа-бокса.

**Examples:**

```css
.scroll-panel {
  overflow: auto;           /* scroll when needed */
  aspect-ratio: 16 / 9;
}

.cover-image {
  width: 100%;
  height: 200px;
  object-fit: cover;      /* fill box, crop */
  object-position: center;
}
```

**Sources:** [MDN — соответствие объекта](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit)

</details>

<details>
  <summary>Logical properties</summary>

### Logical properties

**Definition:**

Логические свойства (`margin-inline`, `padding-block`) сопоставляется с физическими сторонами в зависимости от направления написания CSS, совместимого с i18n.

Стили письма с учетом направления (RTL, вертикальное письмо):

**Examples:**

```css
.card {
  margin-inline: auto;      /* left+right in LTR */
  padding-block: 1rem;      /* top+bottom */
  border-inline-start: 2px solid blue;
}
```

Предпочитайте логические свойства в новом коде физическим. `left`/`right` при поддержке i18n.

**Sources:** [MDN — Логические свойства](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_ological_properties_and_values)

</details>

<details>
  <summary>scrollbar-gutter</summary>

### scrollbar-gutter

**Definition:**

`scrollbar-gutter: stable` резервирует пространство полосы прокрутки, чтобы предотвратить сдвиг макета при появлении полос прокрутки.

Резервирует место для полосы прокрутки, чтобы предотвратить сдвиг макета:

**Examples:**

```css
html {
  scrollbar-gutter: stable;
}
```

Ценности: `auto` | `stable` | `stable both-edges`

---

**Sources:** [MDN — полоса прокрутки](https://developer.mozilla.org/en-US/docs/Web/CSS/scrollbar-gutter)

</details>

<a name="layout-systems"></a>

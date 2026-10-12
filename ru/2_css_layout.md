# CSS — Layout

Flexbox, сетка, позиционирование, адаптивный дизайн и мобильная область просмотра.

[ ← Вернуться в центр](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Sections

- [Layout Systems](#layout-systems)
- [Responsive Design & Container Queries](#responsive-design-container-queries)
- [Scroll, Touch & Mobile Viewport](#scroll-touch-mobile-viewport)

---

## Layout Systems

<details>
  <summary>Positioning</summary>

### Positioning

**Definition:**

`position` удаляет элементы из нормального потока (`absolute`, `fixed`) или компенсирует их (`relative`, `sticky`).

| Значение | Содержащий блок | Поведение прокрутки |
|-------|------------------|-----------------|
| `static` | Нормальный поток | Нормальный |
| `relative` | Self (смещение от нормального положения) | Нормальный |
| `absolute` | Ближайший предок | Нормальный |
| `fixed` | Область просмотра | Исправлено в окне просмотра |
| `sticky` | Ближайший предок свитка | Придерживается родительских границ |

**How it works:**

предок не должен иметь `overflow: hidden`; `top`/`bottom`/`left`/`right` должен быть установлен; родитель должен быть достаточно высоким, чтобы прокручивать.

`fixed` всегда относительно области просмотра; `sticky` переключается между относительными и фиксированными значениями в пределах диапазона прокрутки контейнера.

**Sources:** [MDN — позиция](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

</details>

<details>
  <summary>Stacking context & z-index</summary>

### Stacking context & z-index

**Definition:**

Контекст наложения слоев окрашенных элементов; `z-index` сравниваются только в одном и том же контексте.

`z-index` работает только с **позиционированными** элементами (`relative`, `absolute`, `fixed`, `sticky`) или гибкие/сетчатые дочерние элементы.

**How it works:**

`background-color` → `background-image` → `border` → потомки → `outline`. Полный список штабелирования ниже. Конвейер рендеринга → [1_html.md](./1_html.md).

```
1. Stacking contexts with negative z-index
2. In-flow, non-positioned blocks
3. Floated elements
4. In-flow inline elements
5. Positioned elements (z-index: auto / 0)
6. Positioned elements with positive z-index
7. Stacking contexts with z-index: auto that created a context (opacity, transform, etc.)
```

* `position` + `z-index` (нет `auto`) на позиционированном/гибком/сеточном элементе
* `opacity < 1`
* `transform`, `filter`, `perspective`, `clip-path`
* `will-change` для преобразования/непрозрачности/фильтра
* `isolation: isolate`
* `mix-blend-mode` кроме `normal`

переместите наложение/подсказку на портал верхнего уровня или убедитесь, что родительский объект не создает непреднамеренный контекст стекирования.

**`isolation: isolate`** — принудительно создать новый контекст стекирования без побочных эффектов (полезно для режимов наложения/охвата z-индекса).

```css
.overlay { mix-blend-mode: multiply; }
.hero    { background-blend-mode: overlay; }
```

**When to use:**

#### Pitfall

```
┌─ .modal-backdrop (z-index: 100, creates context) ─┐
│  ┌─ .tooltip (z-index: 9999) ─┐  ← still BELOW     │
│  │  still inside parent layer  │     sibling modal  │
│  └─────────────────────────────┘                     │
└──────────────────────────────────────────────────────┘
```

**Examples:**

```css
.card { isolation: isolate; }
.tooltip { z-index: 1; } /* competes only within .card */
```

**Sources:** [MDN — контекст стекирования] (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context)

</details>

<details>
  <summary>Flexbox</summary>

### Flexbox

**Definition:** Одномерная планировка: распределите пространство по **основной оси** (`row` or `column`).

**How it works:**

`display: flex` / `inline-flex`. Главная ось = `flex-direction`; поперечная ось перпендикулярна. Размер предметов с `flex` (`grow shrink basis`). По умолчанию `min-width: auto` (мин-контент) — установить `min-width: 0` (столбец: `min-height: 0`), поэтому предметы могут сжиматься. `justify-self` **не** применяется к гибким элементам — используйте `align-self` or `margin: auto`.

| Контейнер | Роль |
|-----------|------|
| `flex-direction` | `row` \| `row-reverse` \| `column` \| `column-reverse` |
| `flex-wrap` | `nowrap` \| `wrap` \| `wrap-reverse` |
| `justify-content` | Основная ось: `flex-start` \| `center` \| `flex-end` \| `space-between` \| `space-around` \| `space-evenly` |
| `align-items` | Поперечная ось, одна линия: `stretch` \| `flex-start` \| `center` \| `flex-end` \| `baseline` |
| `align-content` | Дополнительное поперечное пространство при упаковке; игнорируется в одной строке |
| `gap` | Пространство между элементами |

| Товар | Роль |
|------|------|
| `flex: 1` | `1 1 0%` — поровну распределите оставшееся пространство |
| `flex: auto` | `1 1 auto` — увеличивать/уменьшать размер контента |
| `flex: none` | `0 0 auto` - зафиксированный; нет роста/сжатия |
| `flex: 0 0 240px` | Фиксированная боковая панель |
| `align-self` | Переопределить `align-items` |
| `order` | Только визуальное изменение порядка табуляции — **не** порядок табуляции |

**When to use:**

Панели инструментов, навигация, ряды карточек, центрирование, боковая панель + основная.

#### Common pitfalls

Переполнение без `min-width: 0`. `order` против a11y. Колонка гибкая: `justify-content` является **вертикальным**.

**Pluses and minuses:**

| + | − |
| --- | --- |
| Выравнивание 1D на основе контента | Вложенные 2D-макеты противостоят Flex — используйте Grid |
| `gap` + `margin: auto` за оставшееся пространство | `min-width: auto` переполняет текст, если не выполнить сброс |

**Examples:**

```css
.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.sidebar { flex: 0 0 240px; }
.main { flex: 1 1 auto; min-width: 0; }

.nav { display: flex; }
.nav .logo { margin-inline-end: auto; } /* leftover space → links right */
```

**Thoughts:** По одной оси за раз — Flex для строк/столбцов; Сетка, когда вам нужно и то, и другое.

**Sources:** [MDN — Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout)

</details>

<details>
  <summary>CSS Grid</summary>

### CSS Grid

**Definition:** Двумерный макет: строки **и** столбцы в одном контейнере.

**How it works:**

`display: grid` / `inline-grid`. Явные следы = `grid-template-columns` / `rows` / `areas`. Переполняющиеся элементы создают **неявную** сетку (`grid-auto-rows` / `columns` / `flow`). `fr` = доля свободного места; `minmax(min, max)` ограничивает дорожку — используйте `minmax(0, 1fr)` поэтому треки могут сжиматься ниже содержимого. Предметы: `grid-column` / `grid-row` / `grid-area`, `justify-self` / `align-self`. Выровнять ячейки по `justify-items` / `align-items`; выровнять всю сетку по `justify-content` / `align-content`. `gap` для желобов.

`repeat(auto-fit|auto-fill, minmax(280px, 1fr))`: **автозаполнение** оставляет пустые дорожки; **автоподбор** сворачивает их, поэтому остальные столбцы растягиваются. Области: одинаковое количество ячеек в строке, только прямоугольники (без L-образных форм), `.` для пустых ячеек.

**When to use:**

Оболочки страниц, дашборды, карточные матрицы. Вложенный `subgrid` наследует родительские дорожки (выровняйте заголовок/тело/нижний колонтитул карточки по строке).

#### Common pitfalls

`1fr` который не будет сжиматься (используйте `minmax(0, 1fr)`). L-образный `grid-template-areas`. Использование вложений Flex 2D вместо Grid.

**Pluses and minuses:**

| + | − |
| --- | --- |
| 2D-шаблон + именованные области | Избыточное убийство для одной строки панели инструментов |
| `auto-fit` / `fr` без медиа-запросов | Неявные следы удивляют, если `auto-flow` не установлен |

**Examples:**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}

.dashboard {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 240px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100dvh;
}
.header { grid-area: header; }

.grid {
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(120px, auto);
  grid-auto-flow: dense;
}

.card-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; }
.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3;
}
```

**Thoughts:** Grid владеет 2D-оболочкой; Flex владеет 1D-выравниванием внутри ячеек.

**Sources:** [MDN — Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)

</details>

<details>
  <summary>Flex vs Grid — when to use which</summary>

### Flex vs Grid — when to use which

**Definition:**

Flexbox превосходно работает со строками одномерных компонентов; Grid превосходно справляется с двухмерными оболочками страниц и карточными матрицами.


| | Флексбокс | Сетка |
| | --- | --------- |
| | Размеры | 1Д | 2D |
| | На основе контента | Да — элементы определяют поток | Да — или на основе шаблонов |
| | Пробелы | `gap` | `gap` |
| | Лучшее для | Навигация, строки, выравнивание | Макет страницы, карточные матрицы |

Используйте оба вместе: Grid для оболочки страницы, Flexbox внутри ячеек.

**Sources:** [web.dev — Flexbox против Grid](https://web.dev/learn/css/grid/#grid-or-flexbox)

</details>

<details>
  <summary>Float (legacy)</summary>

### Float (legacy)

**Definition:**

Поплавки вытягивают элементы из потока для переноса текста; наследие для макета — предпочитайте гибкий или сеточный.

Плавает (`float: left/right`) вытягивать элементы из потока; текст обтекает их. Исторически использовался для макетов с несколькими столбцами.

**How it works:**

- Плавающий элемент выводится из нормального потока блоков; встроенный контент обтекает его.
- Родительский элемент часто сворачивается до нулевой высоты, если он не создает BFC (`flow-root`, `overflow`, гибкость/сетка).
- Очистка(`clear`, `display: flow-root` на родителе) прекращает перенос содержимого после переноса.

свернутая родительская высота, необходима очистка (`clearfix`, `overflow: hidden`, `display: flow-root`).

используйте Flexbox или Grid. Плавающие элементы остаются действительными для обтекания изображений текстом.

**When to use:** Изображение с переносом текста; `shape-outside` для непрямоугольной упаковки. **Нет** для столбцов страниц, панелей навигации или сеток карточек — используйте flex/grid.

**Pluses and minuses:**

| + | − |
|------|------|
| Естественный перенос текста вокруг изображений | Свернутая родительская высота без BFC |
| Все еще действует для упаковки в журнальном стиле | Удаление хаков(`clearfix`) добавить сложности |
| Работает без поддержки flex/grid | Хрупкие многоколоночные макеты по сравнению с Grid |

**Examples:**

```css
.figure {
  float: left;
  margin: 0 1rem 1rem 0;
  shape-outside: margin-box;
}

.article::after {
  content: "";
  display: table;
  clear: both; /* clearfix */
}
```

**Sources:** [MDN — float](https://developer.mozilla.org/en-US/docs/Web/CSS/float)

</details>

<details>
  <summary>Centering cookbook</summary>

### Centering cookbook

**Definition:** Общие шаблоны горизонтального, вертикального и полного центрирования с использованием поля, гибкости, сетки или абсолютного позиционирования.

**How it works:**

процентная высота требует явной высоты для каждого предка до содержащего блока. Предпочитать `min-height: 100dvh` on `html, body` для полноэкранных макетов.

```css
html, body {
  min-height: 100%;
}
/* or */
html {
  height: 100%;
}
body {
  min-height: 100%;
}
```

---

**Examples:**

```css
/* Horizontal — block element */
.center-h {
  width: fit-content;
  margin-inline: auto;
}

/* Vertical + horizontal — Flexbox */
.center-flex {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100dvh;
}

/* Vertical + horizontal — Grid */
.center-grid {
  display: grid;
  place-items: center;
  min-height: 100dvh;
}

/* Absolute center (known size) */
.center-abs {
  position: absolute;
  inset: 50%;
  transform: translate(-50%, -50%);
}
```

**Sources:** [CSS для JS](https://learn.javascript.ru/css-for-js)

</details>

<a name="responsive-design"></a>

## Responsive Design & Container Queries

<details>
  <summary>Responsive vs adaptive</summary>

### Responsive vs adaptive

**Definition:**

Адаптивные макеты легко масштабируются; адаптивные макеты переключаются в дискретных точках останова.

| Подход | Поведение |
|----------|----------|
| **Отзывчивый** | Гибкие макеты — один дизайн плавно масштабируется (%, `fr`, `clamp`, гибкий/сетка) |
| **Адаптивный** | Дискретные точки останова — разные макеты с фиксированной шириной |

Современная практика: **мобильное реагирование** с точками останова в качестве улучшения.

**Sources:** [web.dev — Адаптивный](https://web.dev/learn/design/Response)

</details>

<details>
  <summary>Media queries</summary>

### Media queries

**Definition:** Медиа-запросы применяют CSS на основе особенностей области просмотра — ширины, предпочтений движения, цветовой схемы, типа указателя.

**How it works:**

640 пикселей · 768 пикселей · 1024 пикселей · 1280 пикселей — настройтесь на свой контент.

```css
@supports (display: grid) {
  .layout { display: grid; }
}

@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}

@media (prefers-color-scheme: dark) {
  :root { color-scheme: dark; }
}
```

| Особенность | Ценности | Вариант использования |
|---------|--------|----------|
| `prefers-reduced-motion` | `reduce` | Отключить/ограничить анимацию |
| `prefers-color-scheme` | `light` \| `dark` | Переключение тем |
| `prefers-contrast` | `more` \| `less` | Высококонтрастный пользовательский интерфейс |
| `prefers-reduced-transparency` | `reduce` | Сплошные фоны над стеклом |

| Особенность | Ценности | Вариант использования |
|---------|--------|----------|
| `(hover: hover)` | устройство с возможностью наведения | Показывать пользовательский интерфейс, доступный только при наведении, на рабочем столе |
| `(hover: none)` | сенсорный первичный | Большие цели касания, отсутствие пользовательского интерфейса, зависящего от наведения |
| `(pointer: fine)` | мышь/стилус | Плотный интерфейс |
| `(pointer: coarse)` | палец | Кнопки большего размера, больше интервалов |

```css
@media (hover: hover) and (pointer: fine) {
  .card:hover { transform: translateY(-2px); }
}

@media (hover: none) {
  .card-actions { opacity: 1; } /* always visible on touch */
}
```

**`@supports`** — обнаружение функций для прогрессивного улучшения (не перехват браузера):

```css
@supports (backdrop-filter: blur(1px)) {
  .glass { backdrop-filter: blur(12px); }
}
```

**Examples:**

```css
/* Mobile-first: base styles, then enhance */
.card-grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
}

@media (min-width: 640px) {
  .card-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .card-grid { grid-template-columns: repeat(3, 1fr); }
}
```

**Sources:** [MDN — Медиа-запросы](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries)

</details>

<details>
  <summary>Container queries</summary>

### Container queries

**Definition:**

Контейнер запрашивает элементы стиля на основе размера родительского контейнера, а не области просмотра.

Стиль основан на **размере родительского контейнера**, а не на области просмотра — это важно для многократно используемых компонентов на боковых панелях, в разделенных представлениях и т. д.

**How it works:**

| Значение | Вопросы по |
|-------|------------|
| `inline-size` | Только ширина (наиболее распространенный вариант) |
| `size` | Ширина и высота |
| `normal` | Не контейнер запросов |

**Единицы запроса контейнера** (относительно контейнера запроса, а не области просмотра):

| Единица | Значение |
|------|---------|
| `cqw` | 1% ширины контейнера |
| `cqh` | 1% высоты контейнера |
| `cqi` / `cqb` | Линейная/блочная ось % |
| `cqmin` / `cqmax` | Меньше/больше cqi и cqb |

```css
.card-container { container-type: inline-size; }

.card-title {
  font-size: clamp(1rem, 4cqi + 0.5rem, 1.5rem);
}
```

использовать `@container` когда **компонент** должен адаптироваться к своему родительскому элементу (виджет боковой панели, панель мониторинга); использовать `@media` когда макет зависит от **окна просмотра** (глобальная навигация, сетка страниц).

**When to use:** компонент адаптируется к своему слоту; медиа-запросы, когда макет зависит от области просмотра (навигация, сетка страницы).

**Examples:**

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 120px 1fr;
  }
}

/* Shorthand */
@container (min-width: 30rem) {
  .widget { padding: 2rem; }
}
```

**Sources:** [MDN — запросы к контейнеру](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)

</details>

<details>
  <summary>Units</summary>

### Units

**Definition:**

Единицы длины CSS (`px`, `rem`, `%`, `vw`, `fr`, `dvh`) разрешается относительно разных ссылочных полей.

| Единица | Значение | Подводные камни |
|------|---------|----------|
| `px` | Абсолютные пиксели | Штраф за границы; избегать изменения размера корневого шрифта |
| `%` | Относительно родителя | Для высоты % требуется родительская высота |
| `em` | Относительно размера шрифта элемента | Соединения при вложенности |
| `rem` | Относительно корневого размера шрифта | Предпочтительно для интервалов/масштаба шрифта |
| `ch` | Ширина символа «0» | Максимальная длина строки (`max-width: 65ch`) |
| `vw` / `vh` | % области просмотра | мобильный `100vh` включает строку URL — используйте `dvh`/`svh`/`lvh` |
| `fr` | Сетка доля свободного пространства | Только сетка |
| `dvh` | Динамическая высота области просмотра | Адаптируется, когда Chrome мобильного браузера показывает/скрывает |

**How it works:**

предпочитать `rem` для консистенции; `em` при изменении размера относительно родительского компонента (кнопок внутри карточки).

```css
.sidebar {
  width: calc(100% - 240px);
  padding: max(1rem, 2vw);
  font-size: clamp(0.875rem, 0.8rem + 0.25vw, 1.125rem);
}
```

`calc()` — смешанные единицы (`%`, `px`, `rem`, `vw`). `min()` / `max()` — выберите самый маленький/самый большой. `clamp(min, preferred, max)` — изменчивые значения без медиа-запросов.

---

**Examples:**

```css
h1 {
  font-size: clamp(1.5rem, 2vw + 1rem, 3rem);
}

.section {
  padding: clamp(1rem, 5vw, 4rem);
}
```

**Sources:** [MDN — значения CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)

</details>

<details>
  <summary>CSS Custom Properties</summary>

### CSS Custom Properties

**Definition:** Пользовательские свойства (`--name`) — это живые переменные CSS, которые каскадируются и наследуются, обеспечивая тематизацию во время выполнения.

**How it works:**

Переменные, определенные с помощью `--name`, используется с `var()`. Каскадируйте и наследуйте как обычные свойства.

`var(--missing, fallback-value)`

установить на `:root` для глобального; в классе компонентов для локальной темы.

пользовательские свойства доступны в браузере — темы времени выполнения, медиа-запросы и обновления JS (`element.style.setProperty('--x', val)`).

**Examples:**

```css
:root {
  --color-primary: #2563eb;
  --space-md: 1rem;
  --radius: 8px;
}

.button {
  background: var(--color-primary);
  padding: var(--space-md);
  border-radius: var(--radius);
}

.button--danger {
  --color-primary: #dc2626; /* override in scope */
}

.theme-dark {
  --color-primary: #60a5fa;
}
```

**Sources:** [MDN — Пользовательские свойства](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)

</details>

<details>
  <summary>Native CSS nesting</summary>

### Native CSS nesting

**Definition:**

Встроенная вложенность позволяет писать дочерние и псевдоселекторы внутри блока правил без препроцессора.

Поддерживается в современных браузерах (2023+). Препроцессор не требуется:

**Examples:**

```css
.card {
  padding: 1rem;

  & .title {
    font-weight: 700;
  }

  &:hover {
    box-shadow: 0 4px 12px rgb(0 0 0 / 10%);
  }
}
```

---

**Sources:** [MDN — вложение](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)

</details>

<a name="scroll-touch-mobile"></a>

## Scroll, Touch & Mobile Viewport

<details>
  <summary>Scroll behavior</summary>

### Scroll behavior

**Definition:** Свойства, связанные с прокруткой, управляют плавной прокруткой, цепочкой прокрутки и смещением привязки для фиксированных заголовков.

**How it works:** `-webkit-overflow-scrolling: touch` (прокрутка по импульсу на старой iOS) — больше не требуется в современном Safari.

**Examples:**

```css
html {
  scroll-behavior: smooth; /* anchor links */
}

.modal-body {
  overflow-y: auto;
  overscroll-behavior: contain; /* no scroll chaining to body */
}
```

| Недвижимость | Ценности | Вариант использования |
|----------|--------|----------|
| `scroll-behavior` | `auto` \| `smooth` | Плавная навигация по якорям |
| `overscroll-behavior` | `auto` \| `contain` \| `none` | Предотвратить цепочку «тянуть-обновить/отскочить» |
| `scroll-padding-top` | длина | Смещение фиксированных заголовков при прокрутке привязки |

**Sources:** [MDN — поведение при прокрутке](https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior)

</details>

<details>
  <summary>Touch & pointer</summary>

### Touch & pointer

**Definition:** `touch-action` и мультимедийные запросы указателя адаптируют взаимодействие между устройствами с сенсорным экраном и мышью.

**Examples:**

```css
.carousel {
  touch-action: pan-x; /* horizontal swipe only */
}

.button {
  touch-action: manipulation; /* disable double-tap zoom delay */
}
```

| `touch-action` | Эффект |
|----------------|--------|
| `auto` | Браузер обрабатывает все жесты |
| `none` | Отключить жесты браузера (настраиваемое перетаскивание) |
| `pan-x` / `pan-y` | Разрешить прокрутку по одной оси |
| `manipulation` | Панорамирование + масштабирование; нет задержки двойного нажатия |

Сопряжение с медиа-запросами: `@media (pointer: coarse)` для больших зон воздействия на сенсорных устройствах.

**Sources:** [MDN — сенсорное действие](https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action)

</details>

<details>
  <summary>Safe areas & viewport units</summary>

### Safe areas & viewport units

**Definition:**

Вставки безопасной области и динамические единицы видового экрана (`dvh`) обрабатывать устройства с надрезом и мобильный браузер Chrome.

Устройства с надрезом и Chrome мобильного браузера нуждаются в дополнительном заполнении:

**Examples:**

```css
.header {
  padding-top: env(safe-area-inset-top);
  padding-inline: env(safe-area-inset-left) env(safe-area-inset-right);
}

.fullscreen {
  min-height: 100dvh; /* dynamic — adapts when URL bar hides */
  padding-bottom: env(safe-area-inset-bottom);
}
```

| Блок просмотра | Поведение |
|---------------|----------|
| `vh` / `vw` | Статическое окно просмотра — `100vh` может переполниться на мобильном телефоне |
| `dvh` / `dvw` | Динамический — меняется по мере того, как показывает/скрывает пользовательский интерфейс браузера |
| `svh` | Маленькое окно просмотра (виден пользовательский интерфейс) |
| `lvh` | Большое окно просмотра (пользовательский интерфейс скрыт) |

Включить безопасную зону: `<meta name="viewport" content="width=device-width, viewport-fit=cover">`

**Sources:** [MDN — единицы области просмотра](https://developer.mozilla.org/en-US/docs/Web/CSS/length#viewport-percentage_lengths)

</details>

---

<a name="typography-colors"></a>

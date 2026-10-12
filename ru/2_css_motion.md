# CSS — Motion

Трансформации, переходы, анимация и визуальные эффекты.

[ ← Вернуться в центр](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Sections

- [Transforms, Transitions & Animations](#transforms-transitions-animations)
- [Visual Effects](#visual-effects)

---

## Transforms, Transitions & Animations

<details>
  <summary>transform</summary>

### transform

**Definition:** `transform` применяет 2D/3D-преобразования без запуска макета — идеально подходит для анимации, созданной с использованием графического процессора.

**How it works:**

- `transform` применяет к элементу 2D/3D-матрицу без изменения схемы макета (элемент по-прежнему занимает исходное пространство, если не объединен с позиционированием).
- Функции составляются справа налево: `translateX(10px) rotate(45deg)` — вращать, а затем перемещать в трансформированном пространстве.
- `transform-origin` устанавливает точку поворота; `perspective` на родительском элементе включает глубину 3D.
- Перемещает элемент на слой композитора — перерисовывает макет пропуска.

`transform` и `opacity` перейти на слой композитора — плавная анимация со скоростью 60 кадров в секунду.

`translate()` не вызывает верстку; `top`/`left` изменения делают. Всегда анимировать `transform`/`opacity` когда это возможно.

**When to use:** Ховер-лифт (`translateY`), спиннеры (`rotate`), масштабирование при нажатии, параллакс (экономно). Оживить `transform` + `opacity` - никогда `top`/`left`/`width` для движения (они вызывают перекомпоновку и могут повысить [CLS](./1_html_ Performance.md#cls-cumulative-layout-shift)).

**Pluses and minuses:**

| + | − |
|------|------|
| Наложение графического процессора — плавность 60 кадров в секунду | Не влияет на документооборот (остается призрачное пространство) |
| Нет макета/перекомпоновки при изменении | 3D-преобразования могут привести к размытию текста на субпиксельных слоях |
| Составные функции | `transform` на родителе создается содержащий блок для `fixed` дети |

**Examples:**

```css
.card:hover {
  transform: translateY(-4px) scale(1.02);
}

.spinner {
  transform: rotate(0deg);
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

**Sources:** [MDN — преобразование](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

</details>

<details>
  <summary>transition</summary>

### transition

**Definition:** `transition` анимирует изменения свойств с течением времени с помощью продолжительности, плавности и задержки.

**How it works:**

- Браузер интерполирует вычисленное начальное значение до конечного значения при изменении свойства (переключение класса, `:hover`, JS `style` изменять).
- Сокращение: `transition: property duration timing-function delay` — через запятую для нескольких свойств.
- Переход только **анимируемых** свойств; `display` обычно нет (используйте `@starting-style` or `allow-discrete` где поддерживается).
- `transition-behavior: allow-discrete` обеспечивает дискретные переходы для `display`, `visibility`и т. д. (современные браузеры).

`transition: all` — перечисляет каждое имущество; может случайно анимировать дорогие свойства.

**When to use:** Изменение цвета/тени при наведении; ящик `transform`; непрозрачность исчезает. Список явных свойств — `transition: opacity 0.2s, transform 0.2s`.

**Pluses and minuses:**

| + | − |
|------|------|
| Простые микровзаимодействия без JS | `transition: all` случайно оживил макет макета |
| Компонуемое ослабление для каждого свойства | Нет контроля над средней анимацией без JS |
| С уважением `prefers-reduced-motion` если вы закроете это | Длительное использование реквизитов макета приводит к зависаниям |

**Examples:**

```css
.button {
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.button:hover {
  background-color: #1d4ed8;
  transform: translateY(-1px);
}
```

Сокращение: `transition: property duration timing-function delay`

**Sources:** [MDN — переход](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)

</details>

<details>
  <summary>@keyframes animation</summary>

### @keyframes animation

**Definition:** `@keyframes` определяет многошаговую анимацию, применяемую через `animation` стенография.

**How it works:**

- `@keyframes` определяет промежуточные шаги (`from`/`to` or `0%`/`100%`); `animation` их применяет стенография.
- `animation-fill-mode: forwards` сохраняет окончательное состояние ключевого кадра после завершения; `both` применяется до и после.
- `animation-iteration-count: infinite` для погрузчиков; `animation-play-state: paused` для управления наведением.
- В отличие от `transition`, запускается без изменения свойств триггера — запускается при загрузке или когда `animation-name` установлен.

`animation-name` · `duration` · `timing-function` · `delay` · `iteration-count` · `direction` · `fill-mode` · `play-state`

**When to use:** Погрузчики (`infinite rotate`), входные эффекты (`fade-in forwards`), импульсы внимания. Предпочитать `transform`/`opacity` ключевые кадры; ворота с `@media (prefers-reduced-motion: reduce)`.

**Pluses and minuses:**

| + | − |
|------|------|
| Многошаговый, циклический, двунаправленный | Сложнее повернуть вспять в середине полета, чем переходы |
| Независимость от триггеров свойств | Анимация реквизита макета (`width`, `top`) вызывает перекомпоновку |
| `animation-timeline` для прокрутки | Множество одновременных анимаций → Нехватка памяти графического процессора |

**Examples:**

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

.modal {
  animation: fade-in 0.3s ease forwards;
}
```

**Sources:** [MDN — @keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes)

</details>

<details>
  <summary>JS animation events</summary>

### JS animation events

**Definition:** `transitionend` и `animationend` срабатывает при завершении CSS-анимации — полезно для очистки DOM в JS.

**Examples:**

```js
el.addEventListener('transitionend', (e) => {
  if (e.propertyName === 'opacity') { /* done */ }
});

el.addEventListener('animationend', () => { /* keyframes done */ });
```

Проверять `e.target === el` во избежание пузырей от детей.

---

**Sources:** [MDN — переходный период](https://developer.mozilla.org/en-US/docs/Web/API/Element/transitionend_event)

</details>

<a name="visual-effects"></a>

## Visual Effects

<details>
  <summary>Filters</summary>

### Filters

**Definition:** CSS `filter` применяет графические эффекты (размытие, яркость, оттенки серого) к отображаемому результату элемента.

**How it works:**

- `filter` применяет графические эффекты ко **всему элементу** (включая дочерние элементы) как к одному растровому изображению.
- Цепочка функций слева направо: `filter: contrast(1.2) brightness(0.9)`.
- `filter: opacity()` отличается от `opacity` свойство — непрозрачность фильтра не создает контекст стекирования таким же образом.
- `drop-shadow()` следует за формой альфа — лучше для значков, чем `box-shadow` на прозрачных PNG.

**When to use:** Эффекты при наведении изображения (`brightness`, `saturate`); отключенное состояние (`grayscale`); тени значков (`drop-shadow`). Избегайте анимации `filter: blur()` на полноэкранных наложениях на мобильных устройствах.

**Pluses and minuses:**

| + | − |
|------|------|
| во многих случаях с использованием графического процессора | Дорого на крупных элементах (полный перекрас) |
| `drop-shadow` следует прозрачности | Цепные фильтры умножают стоимость |
| Никакого влияния на макет | `filter` на родителя влияет на всех потомков |

**Examples:**

```css
.blur-bg   { filter: blur(4px); }
.dim       { filter: brightness(0.8); }
.mono      { filter: grayscale(100%); }
.combined  { filter: contrast(1.2) saturate(1.1); }
```

Функции: `blur()` · `brightness()` · `contrast()` · `grayscale()` · `hue-rotate()` · `invert()` · `opacity()` · `saturate()` · `sepia()` · `drop-shadow()`

**Sources:** [MDN — фильтр](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)

</details>

<details>
  <summary>clip-path & shape</summary>

### clip-path & shape

**Definition:** `clip-path` маскирует форму элемента; `shape-outside` позволяет плавающему содержимому обтекать непрямоугольные блоки.

**How it works:**

- `clip-path` видимая область маски — содержимое за пределами пути обрезается, а не прокручивается.
- Основные формы: `circle()`, `ellipse()`, `inset()`, `polygon()`, `path()`; `clip-path: url(#svg)` для сложных путей SVG.
- `shape-outside` влияет только на геометрию обтекания — элемент должен быть плавающим.
- `clip-path` запускает краску; анимация сложных путей может оказаться дорогостоящей.

**When to use:** Круглые аватары (`circle(50%)`); диагональные разделители секций (`polygon`); плавающие изображения с переносом текста (`shape-outside`). Предпочитать `border-radius` когда круга достаточно.

**Pluses and minuses:**

| + | − |
|------|------|
| Непрямоугольные аватары и герои | Сложный `polygon()` сложно поддерживать — используйте SVG |
| `shape-outside` для макетов журналов | `clip-path` не влияет на проверку попадания одинаково во всех браузерах |
| Анимировать с помощью `@keyframes` на простых формах | Плавающий + `shape-outside` это устаревший шаблон |

**Examples:**

```css
.avatar {
  clip-path: circle(50%);
}

.blob {
  clip-path: polygon(0 0, 100% 0, 100% 85%, 50% 100%, 0 85%);
}

.float-text {
  float: left;
  shape-outside: circle(50%);
  width: 150px;
  height: 150px;
  border-radius: 50%;
}
```

**Sources:** [MDN — путь к клипу](https://developer.mozilla.org/en-US/docs/Web/CSS/clip-path)

</details>

<details>
  <summary>backdrop-filter</summary>

### backdrop-filter

**Definition:** `backdrop-filter` размывает или фильтрует контент за полупрозрачными элементами (морфизм стекла).

**Examples:**

```css
.glass {
  background: rgb(255 255 255 / 20%);
  backdrop-filter: blur(12px);
}
```

**Sources:** [MDN — фоновый фильтр](https://developer.mozilla.org/en-US/docs/Web/CSS/backdrop-filter)

</details>

<details>
  <summary>SVG styling</summary>

### SVG styling

**Definition:**

Использование элементов SVG `fill` и `stroke` вместо `background` и `border`.

SVG использует `fill`, `stroke`, `stroke-width` вместо `background`/`border`:

**Examples:**

```css
.icon {
  fill: currentColor;
  stroke: none;
}
```

**Sources:** [MDN — стиль SVG] (https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes)

</details>

<details>
  <summary>scroll-snap</summary>

### scroll-snap

**Definition:** Привязка прокрутки выравнивает позиции прокрутки по определенным точкам для каруселей и областей прокрутки с разбивкой на страницы.

**Examples:**

```css
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  gap: 1rem;
}

.slide {
  scroll-snap-align: start;
  flex: 0 0 100%;
}
```

**Sources:** [MDN — прокрутка](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll_snap)

</details>

<details>
  <summary>pointer-events & outline</summary>

### pointer-events & outline

**Definition:** `pointer-events` контролирует тестирование попадания; `:focus-visible` обеспечивает кольца фокусировки только для клавиатуры.

**Examples:**

```css
.overlay {
  pointer-events: none; /* clicks pass through */
}

.overlay .button {
  pointer-events: auto;
}

:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

Использовать `:focus-visible` вместо `:focus` для колец фокусировки только с клавиатурой.

---

**Sources:** [MDN — :focus-visible](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible)

</details>

<a name="modern-css"></a>

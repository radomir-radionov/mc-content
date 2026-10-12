# CSS — Architecture

Методологии, инструменты и производительность рендеринга.

[ ← Вернуться в центр](./2_css.md)

### Related docs

- [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md) · [SDLC — методологии](./7_special_sdlc.md#development-methodologies)

### Sections

- [Architecture & Tooling](#architecture-tooling)
- [Performance](#performance)

---

## Architecture & Tooling

<details>
  <summary>BEM (Block Element Modifier)</summary>

### BEM (Block Element Modifier)

**Definition:**

БЭМ — это соглашение об именах: `block__element--modifier` для предсказуемого CSS с низкой специфичностью.

Соглашение об именах для модульного CSS:

**Pluses and minuses:**

предсказуемая специфика, многоразовые блоки, слаженность команды

подробные имена классов; не требуется при использовании модулей CSS или фреймворков с ограниченной областью действия.

**Examples:**

```
block__element--modifier
```

```html
<button class="button button--primary button--large">
  <span class="button__icon">...</span>
  <span class="button__text">Submit</span>
</button>
```

| Часть       | Сепаратор | Пример            |
| ----------- | --------- | ----------------- |
| Блокировать | —         | `button`          |
| Элемент     | `__`      | `button__icon`    |
| Модификатор | `--`      | `button--primary` |

**Sources:** [getbem.com](http://getbem.com/)

</details>

<details>
  <summary>CSS preprocessors (Sass/SCSS, Less)</summary>

### CSS preprocessors (Sass/SCSS, Less)

**Definition:**

Препроцессоры компилируют Sass/Less в CSS, добавляя переменные, примеси и вложенность во время сборки.

Скомпилируйте в CSS. Исторически добавленные переменные, вложение, миксины, импорт.

**How it works:**

Собственные пользовательские свойства и вложенность уменьшают потребность в препроцессорах. Sass по-прежнему полезен для `@mixin`, `@function`и большие конвейеры токенов проектирования.

SCSS использует `{}` синтаксис (CSS-совместимый); синтаксис Sass с отступом встречается реже.

**Examples:**

```scss
$primary: #2563eb;

@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  color: $primary;

  &__title {
    font-weight: 700;
  }
}
```

**Sources:** [Sass](https://sass-lang.com/)

</details>

<details>
  <summary>PostCSS & Autoprefixer</summary>

### PostCSS & Autoprefixer

**Definition:**

PostCSS преобразует CSS с помощью плагинов; Autoprefixer добавляет префиксы поставщиков из конфигурации Browserslist.

PostCSS преобразует CSS с помощью плагинов. **Autoprefixer** добавляет префиксы поставщиков из конфигурации [Browserslist](https://browsersl.ist/) — не пишите вручную. `-webkit-` префиксы в 2025 году.

**How it works:**

- PostCSS анализирует CSS в AST; плагины преобразуют и преобразуют в строки — например, Babel для CSS.
- **Автопрефиксер** читает список браузеров (`> 1%, last 2 versions`) и добавляет только необходимые префиксы.
- Другие плагины: `postcss-preset-env` (этапные особенности), `cssnano` (уменьшить), `postcss-nesting`, пользовательские преобразования.
- Запускается во время сборки в Vite/Webpack — затраты времени выполнения нулевые.

**When to use:** Автопрефиксер для всех производственных сборок; `postcss-preset-env` для гнездования/`color-mix` полифилы; `cssnano` для минификации производства. Установить список браузеров в `package.json` — единственный источник истины.

**Pluses and minuses:**

| +                                             | −                                                                            |
| --------------------------------------------- | ---------------------------------------------------------------------------- |
| Напишите современный CSS; совместимость с CSS | Дополнительный шаг сборки и конфигурация                                     |
| Autoprefixer исключает ручные префиксы        | Порядок плагинов имеет значение — может сломаться при неправильной настройке |
| Экосистема трансформаций                      | Отладка сгенерированного CSS сложнее, чем исходного                          |

**Examples:**

```json
// package.json
"browserslist": ["> 1%", "last 2 versions", "not dead"]
```

```js
// postcss.config.js
export default {
  plugins: {
    "postcss-preset-env": { stage: 2 },
    autoprefixer: {},
    cssnano: process.env.NODE_ENV === "production" ? {} : false,
  },
};
```

**Sources:** [PostCSS](https://postcss.org/) · [Автопрефиксер](https://github.com/postcss/autoprefixer)

</details>

<details>
  <summary>CSS Modules</summary>

### CSS Modules

**Definition:**

Модули CSS определяют имена классов во время сборки, чтобы предотвратить глобальные конфликты стилей.

Имена классов области действия во время сборки — `import styles from './Button.module.css'` → уникальные хеши предотвращают глобальные коллизии.

**How it works:**

- Бандлер переименовывает `.button` → `.Button_button_x7f3a` во время компиляции.
- Импортировать как объект JS: `className={styles.button}` — защита от опечаток благодаря плагинам TypeScript.
- `:global(.foo)` избегает возможности сторонних переопределений; `:local` является значением по умолчанию.
- Состав: `composes: primary from './theme.module.css'`.

**When to use:** Стили компонентов React/Vue/Svelte; примитивы дизайн-системы; замена БЭМ в SPA. Используйте пользовательские свойства CSS для оформления тем между модулями.

**Pluses and minuses:**

| +                                              | −                                                                                        |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Никаких глобальных конфликтов пространств имен | Имена классов нечитабельны в DevTools                                                    |
| Совмещенные стили с компонентами               | Динамические имена классов нужны `styles[variant]` узор                                  |
| Работает с любым фреймворком                   | Никакой настоящей инкапсуляции DOM — глобальные переменные все равно могут просачиваться |

**Examples:**

```css
/* Button.module.css */
.button {
  padding: 0.5rem 1rem;
}
.primary {
  composes: button;
  background: var(--brand);
}
```

```jsx
import styles from "./Button.module.css";
<button className={styles.primary}>Submit</button>;
```

**Sources:** [Модули CSS](https://github.com/css-modules/css-modules)

</details>

<details>
  <summary>Tailwind CSS (utility-first)</summary>

### Tailwind CSS (utility-first)

**Definition:** Tailwind применяет атомарные служебные классы в HTML для быстрого и согласованного с токенами стиля.

**How it works:** Утилиты Tailwind обладают такой же специфичностью: важен порядок в сгенерированных CSS, а не порядок классов HTML. Использовать `tailwind-merge` или произвольные варианты конфликтов.

**Pluses and minuses:**

| +                                    | −                                                    |
| ------------------------------------ | ---------------------------------------------------- |
| Быстрое прототипирование             | Подробность HTML                                     |
| Последовательный дизайн жетонов      | Кривая обучения                                      |
| Удален неиспользуемый CSS в продукте | Проблемы приоритета классов с динамическими классами |

**Examples:**

```html
<button class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
  Submit
</button>
```

**Sources:** [Изучите Tailwind — адаптивную карточку](https://www.freecodecamp.org/news/learn-tailwind-css-by-building-a-Response-product-card/)

</details>

<details>
  <summary>Bootstrap</summary>

### Bootstrap

**Definition:**

Bootstrap — это компонентная среда с сеткой, утилитами и дополнительными виджетами JS.

Платформа компонентов с сеткой, утилитами и виджетами JS. Подходит для быстрых MVP; изучите базовый CSS, чтобы избежать борьбы с фреймворком.

**Sources:** [Bootstrap](https://getbootstrap.com/docs/)

</details>

<details>
  <summary>CSS methodologies comparison</summary>

### CSS methodologies comparison

**Definition:**

Методологии (BEM, OOCSS, SMACSS, ITCSS) организуют CSS для удобства сопровождения и масштабирования команды.

| Метод      | Идея                                            |
| ---------- | ----------------------------------------------- |
| **БЭМ**    | Именование блока/элемента/модификатора          |
| **ООКСС**  | Отдельная структура и скин                      |
| **СМАКСС** | База, макет, модуль, состояние, категории тем   |
| **ИТСС**   | Перевернутый треугольник — уровни специфичности |

---

**Sources:** [CSS-Tricks — методологии](https://css-tricks.com/poll-results-what-css-methodology-do-you-use/)

</details>

<a name="performance"></a>

## Performance

<details>
  <summary>Reflow, repaint, composite</summary>

### Reflow, repaint, composite

**Definition:**

Изменения стиля вызывают перерасчет стиля, макет (перекомпонование), рисование или компоновку — с разными затратами производительности.

Когда CSS меняется, браузер платит разные затраты:

| Шаг                        | Примеры триггеров                                     | Стоимость                      |
| -------------------------- | ----------------------------------------------------- | ------------------------------ |
| **Пересчет стиля**         | смена класса, `:hover`                                | Низкий–средний                 |
| **Макет (перекомпоновка)** | `width`, `height`, `margin`, `font-size`, вставка ДОМ | Высокий                        |
| **Краска**                 | `color`, `background`, `box-shadow`, `visibility`     | Средний                        |
| **Композитный**            | `transform`, `opacity`, `filter`                      | Низкий (графический процессор) |

**How it works:**

оживлять `transform` и `opacity`; избегайте свойств макета в циклах анимации. Пакетное чтение/запись DOM в JS, чтобы избежать сбоев в макете.

`width`, `height`, `min-*`, `max-*`, `margin`, `padding`, `border`, `font-size`, `line-height`, `display`, `position`, `top`/`left`, `flex-*`, `grid-*`, вставка/удаление DOM, чтение `offsetHeight`/`getBoundingClientRect()` после записи.

```
Need to animate?
├─ transform / opacity only → Composite (best)
├─ color / box-shadow / background → Paint (ok for short transitions)
└─ width / height / margin / top → Layout (expensive — avoid in loops)
```

**Thoughts:** Полный конвейер, грязный/глобальный макет, rAF против интервала → [1_html.md — Браузер и рендеринг](./1_html_rendering.md#browser--rendering). Свойство → макет/рисование/композит → [Триггеры CSS](https://csstriggers.com/). Метрики полей (LCP/INP/CLS) → [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals). Фазы/исправления INP → [6_web_api.md — INP](./1_html_performance.md#inp-interaction-to-next-paint).Метрика смещения макета → [CLS](./1*html* Performance.md#cls-cumulative-layout-shift).

**Sources:** [web.dev — Производительность рендеринга](https://web.dev/articles/critical-rendering-path)

</details>

<details>
  <summary>Critical CSS</summary>

### Critical CSS

**Definition:**

Критический CSS встраивает стили над сгибом, чтобы уменьшить блокировку рендеринга и улучшить первую отрисовку.

Для контента **над сгибом** требуется встроенный или предварительно загруженный CSS, чтобы уменьшить блокировку рендеринга.

Преимущества: более быстрая первая отрисовка, лучший LCP, улучшенная мобильная производительность.

**Sources:** [web.dev — Извлечение критического CSS](https://web.dev/articles/extract-critical-css)

</details>

<details>
  <summary>Compositor layers & GPU</summary>

### Compositor layers & GPU

**Definition:**

Слои композитора изолируют свойства, дружественные к графическому процессору (`transform`, `opacity`) для плавной анимации.

Свойства, которые обычно комбинируются с графическим процессором (без макета/отрисовки): `transform`, `opacity`, `filter`, `will-change`.

**How it works:**

`transform`, `opacity < 1`, `filter`, `will-change`, `position: fixed`, видео/холст и т. д.

| Делать                                                   | Не                                                        |
| -------------------------------------------------------- | --------------------------------------------------------- |
| Добавить кратко перед известной анимацией                | Оставить десятки элементов навсегда                       |
| Использовать `will-change: transform` для парящих лифтов | Использовать `will-change: auto` везде «на всякий случай» |
| Удалить после `transitionend` / `animationend`           | Ожидайте, что он исправит ошибки макета                   |

**When to use:**

#### Pitfall

слишком много слоев → большое использование памяти → вылетает на мобильном устройстве. Не посыпать `will-change` повсюду; применить перед анимацией, удалить после.

**Sources:** [web.dev — Слои](https://web.dev/articles/stick-to-compositor-only-properties-and-manage-layer-count)

</details>

<details>
  <summary>contain & content-visibility</summary>

### contain & content-visibility

**Definition:** `contain` изолирует макет/краску; `content-visibility: auto` пропускает рендеринг закадрового контента.

**How it works:**

```css
.section {
  content-visibility: auto;
  contain-intrinsic-size: auto 500px; /* placeholder height prevents scrollbar jump */
}
```

Браузер пропускает макет/отрисовку закадровых разделов; `contain-intrinsic-size` резервирует приблизительное пространство, поэтому положение прокрутки остается стабильным.

| `contain` ценность | Изоляты                                         |
| ------------------ | ----------------------------------------------- |
| `layout`           | Внутренняя планировка от влияния снаружи        |
| `style`            | Счетчики, котировки                             |
| `paint`            | Покраска в элемент коробки                      |
| `size`             | Размер как будто пустой (используйте осторожно) |
| `strict`           | `layout style paint size`                       |

**Examples:**

```css
.card-list-item {
  contain: layout style paint;
}

.below-fold {
  content-visibility: auto;
  contain-intrinsic-size: 0 500px;
}
```

`content-visibility: auto` пропускает рендеринг закадрового контента — значительный выигрыш в производительности для длинных списков.

**Sources:** [MDN — видимость контента](https://developer.mozilla.org/en-US/docs/Web/CSS/content-visibility) · INP/большой DOM: [6_web_api.md](./1_html_performance.md#inp-interaction-to-next-paint)

</details>

<details>
  <summary>CSS sprites (historical)</summary>

### CSS sprites (historical)

**Definition:**

Спрайты объединяют несколько значков в одном изображении, позиционированных с помощью `background-position` — в значительной степени заменен SVG.

Одно изображение, несколько значков через `background-position`. Менее актуально для HTTP/2, значков SVG и шрифтов значков, но все еще применимо для устаревших листов спрайтов.

**How it works:**

- Одно растровое изображение содержит все значки в фиксированных координатах.
- Каждый набор элементов значка `background-image: url(sprite.png)`, зафиксированный `width`/`height`и отрицательный `background-position` чтобы показать правильную плитку.
- Уменьшение количества запросов HTTP/1.1 — одно изображение вместо многих; HTTP/2 уменьшает это преимущество.

**When to use:** Устаревшие корпоративные сайты; шаблоны электронной почты (ограниченная поддержка SVG). **Сегодня:** встроенный SVG, спрайты SVG (`<symbol>` + `<use>`) или шрифты значков, а не растровые спрайты.

**Pluses and minuses:**

| +                                            | −                                                                        |
| -------------------------------------------- | ------------------------------------------------------------------------ |
| Меньше HTTP-запросов на HTTP/1               | Больно поддерживать — пиксельная математика при каждом добавлении значка |
| Согласованный размер значков в одном ресурсе | Сетчатка требует `@2x` листы спрайтов                                    |
| JS не требуется                              | Значки SVG + компоненты значков являются современными по умолчанию       |

**Examples:**

```css
.icon-home {
  width: 24px;
  height: 24px;
  background: url("icons.png") no-repeat 0 0;
}
.icon-search {
  background-position: -24px 0;
}
```

**Sources:** [MDN — фоновая позиция](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)

</details>

<details>
  <summary>Shorthand pitfalls</summary>

### Shorthand pitfalls

**Definition:**

Сокращения CSS сбрасывают пропущенные подсвойства к исходным значениям — используйте длинные обозначения при переопределении одного подсвойства.

Сокращение сбрасывает неуказанные подсвойства к исходным значениям:

**Examples:**

```css
/* Dangerous — resets background-image, position, etc. */
background: #fff;

/* Safer when overriding one sub-property */
background-color: #fff;
```

---

**Sources:** [MDN — сокращенные свойства](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties)

</details>

<details>
  <summary>Accessibility in CSS</summary>

### Accessibility in CSS

**Definition:** Доступный CSS учитывает предпочтения движения, сохраняет индикаторы фокуса и поддерживает достаточный контраст.

**Examples:**

```css
/* Respect motion preferences */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

/* Keyboard focus only */
:focus {
  outline: none;
}
:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

/* Screen-reader only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Sufficient contrast — aim WCAG AA: 4.5:1 body text, 3:1 large text */
```

Никогда не снимайте индикаторы фокусировки без `:focus-visible` замена.

---

**Sources:** [MDN — Доступность](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/CSS_and_JavaScript)

</details>

---

<a name="interview-practice"></a>

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Вопросы и практические задания от [0_interview.md](0_interview.md). Темы только для HTML → [1_html.md](1_html.md). Конвейер рендеринга → [1_html.md](./1_html.md).

Попробуйте ответить, прежде чем открывать решения.

### Original questions (from 0_interview.md)

**Младший — CSS/верстка (Q53–61)**

53. Что такое блочная модель CSS?
54. Какие способы центрирования вы знаете (горизонтальное и вертикальное)?
55. Какие подходы к верстке вы знаете (float, flex, сетка и т. д.)?
56. Как сделать приложение отзывчивым?
57. Почему некоторым свойствам CSS нужны вендорные префиксы (`-webkit-`, `-moz-`, и т. д.)?
58. Как упростить кроссбраузерную стилизацию?
59. Практическое: просмотрите и исправьте плохой пример CSS или HTML.
60. Что такое препроцессоры CSS? Какой вы использовали? Что они добавляют по сравнению со стандартным CSS?

> Q57 (семантическая раскладка) → [1_html.md](1_html.md)

**CSS/макет (вопросы 57–73, только CSS)**

57. Объясните разницу между `px`, `em`, и `rem`.
58. Для чего нужны пользовательские свойства CSS? Приведите примеры использования.
59. Что произойдет, если вы добавите `* { box-sizing: border-box; }`?
60. Как вы адаптируете страницу для печати?
61. Опишите стиль собственных элементов управления формы.
62. Что такое прогрессивный рендеринг? Какие подходы существуют?
63. Назовите способы ленивой загрузки мультимедиа на странице.
64. Назовите популярные методологии CSS и их различия.
65. Как работает CSS Grid?
66. Какие форматы изображений поддерживают анимацию?
67. Как вы отслеживаете CSS `@keyframes` or `transition` закончиться на JS?
68. Какие свойства CSS создаются с помощью графического процессора? Что такое слои композитора?
69. Как повторно использовать встроенные элементы SVG на странице?
70. Что такое рендеринг шрифта с искусственным жирным шрифтом или с искусственным курсивом?

> Q64 (шаблоны), Q71–72, Q74–77 (опция SVG, теневой DOM, холст) → [1_html.md](1_html.md) или общий интерфейс

### Question index

| #     | Тема                                                    | Ответ ниже                       |
|  -----  |  -------------------------------------------------------  |  --------------------------------  |
| 53    | Модель коробки                                          | Модель коробки                   |
| 54    | Центрирование                                           | Центрирование                    |
| 55    | плавающий / гибкий / сетка                              | Подходы к планировке             |
| 56    | Адаптивное приложение                                   | Адаптивный                       |
| 58–59 | Вендорные префиксы/кроссбраузерность                    | Префиксы поставщиков             |
| 60    | Обзор плохого CSS                                       | Плохой обзор CSS + Задача 6      |
| 61    | Препроцессоры                                           | Препроцессоры                    |
| 57 †  | пикс/эм/бэр                                             | Единицы |
| 58 † | CSS-переменные                           | Пользовательские свойства    |
| 59 †  | `* { box-sizing }`                                      | Универсальная калибровка коробок |
| 60 †  | Стили печати                                            | Стили печати                     |
| 61 †  | Стилизация формы                                        | Стилизация формы                 |
| 62 †  | Прогрессивный рендеринг                                 | Прогрессивный рендеринг          |
| 63 †  | Ленивая загрузка мультимедиа                            | Ленивая загрузка                 |
| 65 †  | CSS-методологии                                         | Методологии                      |
| 66 †  | CSS-сетка                                               | Сетка                            |
| 67 †  | Форматы анимированных изображений                       | Анимированные изображения        |
| 68 †  | Анимационные события в JS                               | Анимация JS + Задача 8 |
| 69 † | Слои графического процессора/композитора | Слои графического процессора |
| 70 †  | Повторное использование встроенного SVG                 | Повторное использование SVG      |
| 73 †  | Искусственный жирный/курсив                             | Искусственные шрифты             |
| —     | BFC, перекомпоновка, гибкое переполнение, неявная сетка | См. вопросы и ответы             |
| —     | Содержащий блок                                         | Содержащий блок                  |
| —     | `min-width: 0` в гибком                                 | Гибкая минимальная ширина        |
| —     | `auto-fit` vs `auto-fill`                               | Автоподбор против автозаполнения |
| —     | Внутренняя и внешняя калибровка                         | Внутренний размер                |
| —     | `100vh` vs `100dvh`                                     | Единицы просмотра                |
| —     | `@supports`                                             | Поддерживает запросы             |
| —     | Контекст укладки                                        | Контекст укладки                 |
| —     | `box-shadow` vs `drop-shadow`                           | Тень против тени                 |
| —     | Родной `<select>` пределы                               | Выберите стиль                   |
| —     | Переключатель на чистом CSS                             | Переключение CSS                 |
| 45    | Переключение iPhone без JS                              | Переключение CSS + Задача 15     |
| Q13   | Очистка кеша CSS/JS                                     | Перебор кеша                     |

† Расширенный блок · Семантическая компоновка (Q57) → [1_html.md](1_html.md)

---

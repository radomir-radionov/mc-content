# CSS — Visual

Типографика, цвет, оформление, стиль формы и печать.

[ ← Вернуться в центр](./2_css.md)

### Related docs

- [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md) · [Принципы UX и UI](./7_special_ux.md#ux-and-ui-principles)

### Sections

- [Typography & Colors](#typography-colors)
- [Decoration — Background, Border, Shadow](#decoration-background-border-shadow)
- [Form & Native Control Styling](#form-native-control-styling)
- [Print & Paged Media](#print-paged-media)

---

## Typography & Colors

<details>
  <summary>Font loading</summary>

### Font loading

**Definition:** `@font-face` и `font-display` контролировать загрузку и замену веб-шрифтов, чтобы избежать невидимого текста.

**How it works:**

один файл, `font-weight: 100 900` диапазон, `font-variation-settings`.

`font-family: "Inter", system-ui, sans-serif;` — всегда заканчиваются родовой семьей.

Сопоставьте резервные показатели с `size-adjust`, `ascent-override`, `descent-override`, `line-gap-override` поэтому своп не меняет макет. CLS (FOIT/FOUT, предварительная нагрузка, WOFF2, `unicode-range`) → [1_html.md — CLS](./1*html* Performance.md#cls-cumulative-layout-shift).

**Examples:**

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter.woff2") format("woff2");
  font-weight: 100 900;
  font-display: swap; /* show fallback immediately, swap when loaded */
}
```

| `font-display` | Поведение                                                                       |
| -------------- | ------------------------------------------------------------------------------- |
| `swap`         | Текст виден сразу; может ФУТ                                                    |
| `optional`     | Короткий период блокировки; может сохранить резервный вариант в медленных сетях |
| `block`        | Невидимый текст во время загрузки (FOIT) — избегать                             |

**Sources:** [MDN — @font-face](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face)

</details>

<details>
  <summary>Typography properties</summary>

### Typography properties

**Definition:** Свойства оформления управляют размером шрифта, высотой строки, усечением, переносом и качеством рендеринга текста.

**How it works:**

```css
/* Single-line ellipsis */
.ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Multi-line clamp — see .truncate above */
```

| Недвижимость                | Эффект                                                           |
| --------------------------- | ---------------------------------------------------------------- |
| `overflow-wrap: break-word` | При необходимости разбивайте длинные слова в произвольных точках |
| `word-break: break-all`     | Взломать куда угодно (URL-адреса, коды)                          |
| `hyphens: auto`             | Расстановка переносов с учетом языка (`lang` атрибут обязателен) |

**When to use:**

#### `white-space` pitfalls

| Значение   | Поведение                                                 |
| ---------- | --------------------------------------------------------- |
| `normal`   | Свернуть пробелы; обернуть                                |
| `nowrap`   | Без упаковки — необходимо `overflow: hidden` + многоточие |
| `pre-wrap` | Сохраняйте пробелы; обернуть                              |

**Examples:**

```css
body {
  font-size: 1rem;
  line-height: 1.5;
  font-weight: 400;
  text-wrap: pretty; /* reduce orphans */
}

.heading {
  font-size: clamp(1.25rem, 2vw + 1rem, 2rem);
  letter-spacing: -0.02em;
}

.truncate {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

- `line-height` — предпочтительно без единиц измерения (1,5 шкалы с размером шрифта)
- `line-clamp` — обрезать многострочный текст (используйте с `overflow: hidden`)
- `text-rendering: optimizeLegibility` — лучший кернинг/лигатуры; незначительные затраты на производительность
- `text-decoration-skip-ink: auto` — подчеркивание пропускает нижние элементы глифа

**Sources:** [MDN — Типографика](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text)

</details>

<details>
  <summary>text-wrap: balance / pretty</summary>

### text-wrap: balance / pretty

**Definition:** `text-wrap` контролирует, как браузер разбивает строки — `balance` выравнивает длину строк в заголовках; `pretty` уменьшает число сирот в основном тексте.

**How it works:**

- `text-wrap: balance` — браузер перераспределяет слова по строкам для более равномерной длины строк (лучше всего подходит для коротких заголовков).
- `text-wrap: pretty` — предпочитает разрывы, избегающие последних строк из одного слова (сирот) в абзацах.
- `text-wrap: wrap` — поведение упаковки по умолчанию.

**When to use:** `balance` on `h1`–`h3` и названия карт; `pretty` в тексте статьи; держать `nowrap` + многоточие для ячеек таблицы и значков.

**Pluses and minuses:**

| +                                                                              | −                                                                                                      |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| Лучшая типографика без JS                                                      | `balance` может быть дорогостоящим при использовании длинного текста — используйте только в заголовках |
| Прогрессивное улучшение                                                        | Проверьте caniuse — растущая поддержка в Chromium/Safari/Firefox                                       |
| Сочетается с `text-wrap: nowrap` + многоточие для пользовательского интерфейса | Не является заменой ручного. `<wbr>` в URL-адресах                                                     |

**Examples:**

```css
h1,
h2,
h3 {
  text-wrap: balance;
}

article p {
  text-wrap: pretty;
}
```

**Sources:** [MDN — перенос текста](https://developer.mozilla.org/en-US/docs/Web/CSS/text-wrap)

</details>

<details>
  <summary>Color formats</summary>

### Color formats

**Definition:** Современные цветовые форматы (`rgb()`, `hsl()`, `oklch()`, `color-mix()`) поддерживают единообразную тематизацию восприятия.

**Examples:**

```css
.keyword {
  color: navy;
}
.hex {
  color: #2563eb;
}
.rgb {
  color: rgb(37 99 235);
}
.rgba {
  color: rgb(37 99 235 / 80%);
}
.hsl {
  color: hsl(220 90% 56%);
}
.oklch {
  color: oklch(58% 0.22 264);
} /* perceptually uniform */
.mix {
  color: color-mix(in oklch, blue 70%, white);
}
.current {
  border-color: currentColor;
}
```

**Предпочитать `oklch` / `hsl` для оформления** — легче настроить яркость/насыщенность, чем в шестнадцатеричном формате.

**Sources:** [MDN — цвет](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)

</details>

<details>
  <summary>Gradients</summary>

### Gradients

**Definition:** CSS-градиенты (`linear-gradient`, `radial-gradient`) создавать плавные цветовые переходы без использования изображений.

**Examples:**

```css
.hero {
  background: linear-gradient(135deg, #667eea, #764ba2);
}

.badge {
  background: radial-gradient(circle at top left, #fff, #f0f0f0);
}

.shimmer {
  background: linear-gradient(90deg, #eee 25%, #f5f5f5 50%, #eee 75%);
  background-size: 200% 100%;
}
```

**Sources:** [MDN — градиент](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient)

</details>

<details>
  <summary>Dark mode</summary>

### Dark mode

**Definition:** Темный режим использует `prefers-color-scheme` и пользовательские свойства для замены токенов цвета во время выполнения.

**Examples:**

```css
:root {
  color-scheme: light dark;
  --bg: #fff;
  --text: #111;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #111;
    --text: #eee;
  }
}

body {
  background: var(--bg);
  color: var(--text);
}
```

**Sources:** [MDN — предпочитает-цветовую-схему](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)

</details>

<details>
  <summary>color-scheme & light-dark()</summary>

### color-scheme & light-dark()

**Definition:** `color-scheme` сообщает браузеру, какие цветовые схемы поддерживает страница (влияет на полосы прокрутки, элементы управления формами, `canvas`); `light-dark()` выбирает значение на основе используемой цветовой схемы.

**How it works:**

- `color-scheme: light dark` on `:root` — страница поддерживает оба варианта; браузер выбирает из `prefers-color-scheme` или переопределение автора.
- `color-scheme: dark` — подсказка о темном пользовательском интерфейсе Chrome, даже если токены являются пользовательскими.
- `light-dark(light-value, dark-value)` — одно объявление для обеих тем без дублирования правил.

**When to use:** Набор `color-scheme: light dark` on `:root`; использовать `light-dark()` для бордюров и поверхностей; объединить с `prefers-color-scheme` для сложных обменов токенов.

**Pluses and minuses:**

| +                                                                     | −                                                                                |
| --------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Встроенная тема управления формой                                     | `light-dark()` потребности `color-scheme` установить в корне                     |
| Меньше `@media` дублирование                                          | Не полная система дизайнерских токенов — в сочетании с настраиваемыми свойствами |
| Полоса прокрутки/пользовательский интерфейс Chrome соответствует теме | Контраст теста в обеих схемах                                                    |

**Examples:**

```css
:root {
  color-scheme: light dark;
  --surface: light-dark(#fff, #1a1a1a);
  --text: light-dark(#111, #eee);
  --border: light-dark(#e5e7eb, #333);
}

body {
  background: var(--surface);
  color: var(--text);
  border: 1px solid var(--border);
}
```

**Sources:** [MDN — цветовая схема](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme) · [light-dark()](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark)

</details>

<a name="decoration"></a>

## Decoration — Background, Border, Shadow

<details>
  <summary>Background</summary>

### Background

**Definition:**

Фоновые от руки независимо контролируют цвет, изображение, размер, положение, повтор, клип и начало координат.

Предпочитайте длинные записи при переопределении одного подсвойства — `background:` сокращение сбрасывает все подсвойства.

**Examples:**

```css
.hero {
  background-color: #1e293b;
  background-image: url("/bg.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed; /* parallax-like; avoid on mobile */
  background-clip: border-box;
  background-origin: padding-box;
}
```

| Недвижимость          | Цель                                                   |
| --------------------- | ------------------------------------------------------ |
| `background-size`     | `cover` \| `contain` \| явный размер                   |
| `background-position` | Точка привязки (`center top`, `50% 50%`)               |
| `background-repeat`   | `repeat` \| `no-repeat` \| `space` \| `round`          |
| `background-clip`     | Площадь покраски (`border-box`, `padding-box`, `text`) |
| `background-origin`   | Позиционирующий ящик для `background-position`         |

Несколько фонов сначала объявляются сверху:

```css
.badge {
  background:
    linear-gradient(rgb(0 0 0 / 40%), rgb(0 0 0 / 40%)),
    url("/photo.jpg") center / cover no-repeat;
}
```

**Sources:** [MDN — фон](https://developer.mozilla.org/en-US/docs/Web/CSS/background)

</details>

<details>
  <summary>Border, outline & box-shadow</summary>

### Border, outline & box-shadow

**Definition:** Границы влияют на макет; контуров нет. `box-shadow` рисует на поле многослойные тени.

**How it works:**


| | `box-shadow`                 | `filter: drop-shadow()`           |
| | ------------------  |  ----------------------------  |
| | Следует            | Box (включая радиус границы) | Альфа-форма контента              |
| | SVG/изображения    | Только прямоугольная коробка | Следит за непрозрачными пикселями |
| | Производительность | Краска                       | Композитный                       |

**Examples:**

```css
.card {
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  outline: 2px solid transparent; /* room for focus ring */
  outline-offset: 2px;
  box-shadow:
    0 1px 2px rgb(0 0 0 / 5%),
    0 8px 24px rgb(0 0 0 / 10%);
}
```

| Недвижимость    | Заметки                                                                 |
| --------------- | ----------------------------------------------------------------------- |
| `border`        | Сокращение — устанавливает ширину, стиль и цвет со всех сторон          |
| `border-radius` | За угол: `border-radius: 8px 8px 0 0`                                   |
| `outline`       | Не влияет на макет (в отличие от границы); использовать для фокусировки |
| `box-shadow`    | Несколько слоев, разделенных запятыми; `inset` для внутренней тени      |

**Sources:** [MDN — box-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)

</details>

---

<a name="form-styling"></a>

## Form & Native Control Styling

<details>
  <summary>Customizing form controls</summary>

### Customizing form controls

**Definition:** Собственные элементы управления формой можно изменить с помощью `appearance: none`, `accent-color`и селекторы псевдоэлементов.

**How it works:** полностью индивидуальный `<select>` выпадающие списки по-прежнему непоследовательны — используйте родной `<select>` с `appearance: none` + пользовательская стрелка или создайте собственный компонент с помощью ARIA для полного контроля.

**Examples:**

```css
.input {
  appearance: none;
  width: 100%;
  padding: 0.5rem 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  accent-color: #2563eb;
}

.input::placeholder {
  color: #9ca3af;
}

.input:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

input[type="file"]::file-selector-button {
  padding: 0.25rem 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  background: #f9fafb;
}
```

| Техника            | Применяется к                                 |
| ------------------ | --------------------------------------------- |
| `appearance: none` | Удалить Chrome ОС — перестроить с помощью CSS |
| `accent-color`     | Флажок, радио, диапазон, прогресс             |
| `::placeholder`    | Цвет текста заполнителя                       |
| `:focus-visible`   | Специальное кольцо фокусировки                |

**Sources:** [MDN — внешний вид](https://developer.mozilla.org/en-US/docs/Web/CSS/appearance)

</details>

<details>
  <summary>Pure CSS toggle (no JavaScript)</summary>

### Pure CSS toggle (no JavaScript)

**Definition:**

Скрытый флажок плюс одноуровневые селекторы (`:checked +`) может управлять переключаемым пользовательским интерфейсом без JavaScript.

Переключатель в стиле iOS с использованием скрытого флажка + селектора одного уровня:

```html
<label class="switch">
  <input type="checkbox" class="switch__input" />
  <span class="switch__track" aria-hidden="true"></span>
</label>
```

**Examples:**

```css
.switch__input {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
}

.switch__track {
  display: inline-block;
  width: 48px;
  height: 28px;
  background: #d1d5db;
  border-radius: 999px;
  position: relative;
  transition: background 0.2s ease;
  cursor: pointer;
}

.switch__track::after {
  content: "";
  position: absolute;
  top: 2px;
  left: 2px;
  width: 24px;
  height: 24px;
  background: #fff;
  border-radius: 50%;
  transition: transform 0.2s ease;
}

.switch__input:checked + .switch__track {
  background: #22c55e;
}

.switch__input:checked + .switch__track::after {
  transform: translateX(20px);
}

.switch__input:focus-visible + .switch__track {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}
```

Использование `:checked` + соседний брат `+`. Сохраняйте родной `<input type="checkbox">` для доступности и отправки формы.

**Sources:** [CSS для JS — формы](https://learn.javascript.ru/css-for-js)

</details>

---

<a name="print"></a>

## Print & Paged Media

<details>
  <summary>Print styles</summary>

### Print styles

**Definition:** `@media print` скрывает хром, настраивает типографику и контролирует разрывы страниц для вывода на печать.

**How it works:**

```css
@page {
  margin: 2cm;
  size: A4 portrait;
}
```

Избегайте темного фона при печати — трата чернил; расширяйте URL-адреса только тогда, когда это полезно.

**Examples:**

```css
@media print {
  nav,
  .sidebar,
  .ads,
  .no-print {
    display: none !important;
  }
  body {
    font-size: 12pt;
    color: #000;
    background: #fff;
  }
  a[href^="http"]::after {
    content: " (" attr(href) ")";
    font-size: 0.85em;
  }
  .page-break {
    break-before: page;
  }
  h1,
  h2,
  h3 {
    break-after: avoid;
  }
  img {
    max-width: 100% !important;
  }
}
```

| Недвижимость                   | Цель                                         |
| ------------------------------ | -------------------------------------------- |
| `break-before` / `break-after` | Принудительно или избегайте разрывов страниц |
| `break-inside: avoid`          | Сохранить блок на одной странице             |
| `print-color-adjust: exact`    | Печать цветов фона в соответствии с дизайном |

**Sources:** [MDN — Печать](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries#printing)

</details>

---

<a name="transforms-animations"></a>

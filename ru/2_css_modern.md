# CSS — Modern

Более 2024 функций CSS и расширенные темы фазы 2.

[ ← Вернуться в центр](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Sections

- [Modern CSS (2024+)](#modern-css-2024)
- [CSS Phase 2 (advanced)](#css-phase-2-advanced)

---

## Modern CSS (2024+)

Особенности, которые стоит знать для интервью и новых проектов.

<details>
  <summary>@starting-style</summary>

### @starting-style

**Definition:**

`@starting-style` определяет стили ввода для элементов при их первом появлении (диалоги, всплывающие окна).

Анимация входа для элементов при их первом появлении (поповер, диалог):

**How it works:**

- `@starting-style` определяет состояние «до» для элементов, входящих в документ или переключающихся `display` / `[open]` /поповер утверждает.
- В паре с `transition` — браузер интерполирует начальное правило к обычному правилу при входе.
- Заменяет JS `requestAnimationFrame` хаки для анимации первой отрисовки `<dialog>`, `[popover]`, аккордеоны.

**When to use:** Диалоговое/модальное появление; вход в масштабе поповера; элемент списка развернуть. Комбинировать с `@media (prefers-reduced-motion: reduce)`.

**Pluses and minuses:**

| + | − |
|------|------|
| Чистый CSS для ввода анимации | Требует `transition-behavior: allow-discrete` для `display` в некоторых случаях |
| Работает с API-интерфейсами popover/dialog | Поддержка браузеров все еще развивается — проверьте caniuse |
| Нет вспышки нестилизованного конечного состояния | Для анимации выхода по-прежнему требуются отдельные правила или View Transitions |

**Examples:**

```css
.dialog {
  transition: opacity 0.3s, transform 0.3s;
  opacity: 1;
  transform: scale(1);
}

@starting-style {
  .dialog[open] {
    opacity: 0;
    transform: scale(0.95);
  }
}
```

**Sources:** [MDN — @starting-style](https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style)

</details>

<details>
  <summary>CSS anchor positioning</summary>

### CSS anchor positioning

**Definition:**

Позиционирование привязки размещает всплывающие окна относительно элемента привязки без измерений JavaScript.

Расположите всплывающие окна/подсказки относительно элемента привязки без измерений JS:

**How it works:**

- Якорный элемент объявляет `anchor-name: --trigger`; позиционированный элемент ссылается на него через `position-anchor: --trigger`.
- `anchor()` функция размещает края относительно якоря: `top: anchor(bottom)`, `left: anchor(center)`.
- `position-area` / `inset-area` сокращенное отображение логических сторон (начало блока, конец строки и т. д.).
- `anchor-size()` можно изменить размер всплывающего окна по ширине привязки.

**When to use:** Подсказки, выпадающие меню, всплывающие окна, привязанные к кнопкам. Предоставьте запасной вариант JS или `@supports (anchor-name: --x)` прогрессивное улучшение.

**Pluses and minuses:**

| + | − |
|------|------|
| Нет JS `getBoundingClientRect` для подсказок | Chromium-first — проверьте поддержку Firefox/Safari |
| По умолчанию поддерживается прокрутка/изменение размера | Для краев области просмотра все еще необходима логика резервного позиционирования |
| Работает с `position: fixed` поповеры | Именование коллизий, если `--anchor` имена используются повторно |

**Examples:**

```css
.anchor {
  anchor-name: --trigger;
}

.tooltip {
  position: absolute;
  position-anchor: --trigger;
  inset-area: block-end;
}
```

**Sources:** [MDN — позиционирование привязки](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_anchor_positioning)

</details>

<details>
  <summary>Scroll-driven animations</summary>

### Scroll-driven animations

**Definition:**

Анимация, управляемая прокруткой, связывает прогресс ключевого кадра с позицией прокрутки, а не со временем.

Animate привязан к прогрессу прокрутки (без прослушивателей прокрутки JS):

**How it works:**

- `animation-timeline` привязывает прогресс по ключевым кадрам для прокрутки или просмотра прогресса вместо времени.
- `scroll()` — привязан к контейнеру прокрутки; `view()` — привязано к видимости элемента в области прокрутки.
- `animation-range` сопоставляет интервалы прокрутки/просмотра с ключевым кадром `0%`–`100%` (`entry`, `exit`, `cover`, `contain`).
- Работает на композиторе — нет `scroll` прослушиватели событий для эффектов параллакса/раскрытия.

**When to use:** Прокрутка-открытие разделов; индикаторы выполнения, привязанные к прокрутке; заголовок сжимается при прокрутке. Отступать: `IntersectionObserver` + переключение классов.

**Pluses and minuses:**

| + | − |
|------|------|
| Без прокрутки JS — лучшая производительность | Ограниченная поддержка браузеров — требуется постепенное улучшение |
| Сглаживание параллакса и раскрытие | Сложнее отлаживать, чем анимацию, основанную на времени |
| С уважением `prefers-reduced-motion` если закрытый | Сложные диапазоны требуют экспериментов |

**Examples:**

```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
```

**Sources:** [MDN — анимация, управляемая прокруткой](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll-driven_animations)

</details>

<details>
  <summary>@scope</summary>

### @scope

**Definition:**

`@scope` ограничивает доступ селектора поддеревом для инкапсуляции без Shadow DOM.

Ограничить доступ селектора к поддереву (собственная инкапсуляция без Shadow DOM):

**How it works:**

- `@scope (root)` ограничивает соответствие селектора потомкам `root` — как легкая граница Shadow DOM.
- `:scope` относится к самому корневому элементу области видимости.
- `@scope (.a) to (.b)` устанавливает необязательную верхнюю границу — селекторы не выходят за ее пределы `.b`.
- Специфика не изменилась: объем влияет только на **досягаемость**, а не на вес.

**When to use:** Стили компонентов карточек без префиксов БЭМ; Виджеты дизайн-системы в контенте CMS. Если требуется жесткая изоляция, отдавайте предпочтение модулям CSS/Shadow DOM.

**Pluses and minuses:**

| + | − |
|------|------|
| Изоляция компонентов без инструментов сборки | Никакой инкапсуляции стилей в отношении глобальных `!important` |
| Плоские селекторы внутри области видимости (`img` нет `.card img`) | Поддержка браузеров все еще развивается |
| Пары с каскадными слоями | Перекрывающиеся области требуют тщательного выбора корня |

**Examples:**

```css
@scope (.card) {
  :scope { padding: 1rem; }
  img { border-radius: 8px; } /* only images inside .card */
}
```

**Sources:** [MDN — @scope](https://developer.mozilla.org/en-US/docs/Web/CSS/@scope)

</details>

---

<a name="css-phase-2"></a>

## CSS Phase 2 (advanced)

Заглушки разделов — расширяйте по мере необходимости. Ссылки MDN для более глубокого изучения.

<details>
  <summary>Multi-column layout</summary>

### Multi-column layout

**Definition:**

Многоколоночный макет CSS распределяет текст по столбцам, как макеты журналов.

Столбцы CSS для оформления текста в журнальном стиле:

**How it works:** [Использование макетов с несколькими столбцами](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout)

**Examples:**

```css
.article {
  column-count: 3;
  column-gap: 2rem;
  column-rule: 1px solid #e5e7eb;
}
.break { break-inside: avoid; }
```

**Sources:** [MDN — Multicol](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout)

</details>

<details>
  <summary>View Transitions API</summary>

### View Transitions API

**Definition:**

Переходы просмотра анимируют общие элементы и изменения состояния страниц в SPA.

Анимация изменений состояния DOM (изменения маршрута SPA, переходы общих элементов):

**How it works:**

- JS-триггеры `document.startViewTransition(() => { /* DOM update */ })` — снимки браузера старых/новых состояний.
- `view-transition-name` на парах элементов старые/новые снимки для морфинга общих элементов.
- `::view-transition-old()` / `::view-transition-new()` псевдоэлементы стиля кроссфейд-слоев.
- Применяется кроссфейд по умолчанию; пользовательские ключевые кадры переопределяются для каждого именованного перехода.

[Просмотр переходов](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

**When to use:** Переходы страниц SPA; детальное преобразование героя; переключение темы, кроссфейд. Уважать `prefers-reduced-motion` — пропускать или сокращать переходы.

**Pluses and minuses:**

| + | − |
|------|------|
| Переходы маршрутов SPA без FLIP JS | Требуется вызов JS API, а не только чистый CSS |
| Анимации героев с общими элементами | Один переход за раз — параллельных ВТ нет |
| Работает с MPA через `@view-transition` правило (появляющееся) | Большие снимки DOM могут работать медленно на устройствах начального уровня |

**Examples:**

```css
.card { view-transition-name: hero; }
::view-transition-old(hero) { animation: fade-out 0.2s; }
::view-transition-new(hero) { animation: fade-in 0.2s; }
```

**Sources:** [MDN — Просмотр переходов](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

</details>

<details>
  <summary>@property registered custom properties</summary>

### @property registered custom properties

**Definition:**

`@property` регистрирует введенные пользовательские свойства, которые можно анимировать.

Сделайте переменные CSS анимируемыми и типизированными:

**How it works:**

- `@property --name` регистрирует пользовательское свойство с `syntax`, `inherits`, и `initial-value`.
- Браузер проверяет назначения — недопустимые значения возвращаются к `initial-value` вместо того, чтобы признать недействительным все имущество.
- Зарегистрированные свойства с типизированными `syntax` может быть **анимирован** в `@keyframes` (e.g. `--angle` от `0deg` to `360deg`).
- Незарегистрированный `--vars` обрабатываются как строки — без интерполяции.

[@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

**When to use:** Анимированные конические градиенты (`--angle`); токены темы с проверкой; цветовые переходы в пользовательских свойствах. Сопряжение с `@supports` или прогрессивное улучшение.

**Pluses and minuses:**

| + | − |
|------|------|
| Анимируемые градиенты, цвета, длины с помощью `--vars` | Подробная регистрация по каждому объекту недвижимости |
| Безопасность типов для токенов дизайна | `syntax` типы должны точно соответствовать назначениям |
| Обеспечивает расширяемость в стиле Houdini | Резервный вариант необходим для браузеров без `@property` |

**Examples:**

```css
@property --angle {
  syntax: '<angle>';
  inherits: false;
  initial-value: 0deg;
}
```

**Sources:** [MDN — @property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

</details>

<details>
  <summary>Relative color syntax</summary>

### Relative color syntax

**Definition:**

Синтаксис относительного цвета извлекает новые цвета из существующих значений (`oklch(from var(--brand) ...)`).

Получите цвета из существующих значений:

**How it works:** [Относительные цвета](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors/Relative_colors)

**Examples:**

```css
.theme {
  --brand: oklch(58% 0.22 264);
  --brand-muted: oklch(from var(--brand) l c h / 50%);
}
```

**Sources:** [MDN — Относительные цвета](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors/Relative_colors)

</details>

<details>
  <summary>Table and list styling</summary>

### Table and list styling

**Definition:**

Таблицы и списки имеют специальные свойства для границ, интервалов и маркеров.

`border-collapse`, `table-layout: fixed`, `list-style-type`, `list-style-position`. Используйте семантику `<table>` для табличных данных; стиль с `thead`/`tbody` для рядов зебры.

**Sources:** [MDN — макет таблицы](https://developer.mozilla.org/en-US/docs/Web/CSS/table-layout)

</details>

<details>
  <summary>Writing-mode and RTL</summary>

### Writing-mode and RTL

**Definition:**

`writing-mode` и `direction` управлять потоком текста; пара с логическими свойствами для поддержки RTL.

`writing-mode: vertical-rl`, `direction: rtl` — пара с логическими свойствами (`margin-inline`, `padding-block`). См. раздел **Логические свойства** в разделе «Блочная модель».

**Sources:** [MDN — режим письма](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode)

</details>

---

<a name="architecture-tooling"></a>

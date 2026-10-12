# CSS — Interview

Вопросы для собеседования по CSS, с ответом на каждый вопрос.

[← Вернуться в центр](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы из [Devinterview.io — CSS](https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/).

### CSS Basics

<details>
<summary>Что означает CSS stand for and what is its Primary use?</summary>

### Что означает CSS stand for and what is its Primary use?

**Definition:**

**Cascading Style Sheets** (**CSS**) в первую очередь созданы, чтобы отделить содержимое веб-страницы от её визуального оформления. Это позволяет сохранять единый и гибкий дизайн на многих веб-страницах.

**How it works:**

#### Ключевые понятия

- **Селекторы**: элементы, к которым применяются правила стилей.
- **Свойства**: визуальные признаки, такие как font-size, color и background.
- **Значения**: конкретные настройки свойств, например 'red' для свойства color.

**Examples:**

#### Наглядный пример

Вот HTML-код:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Example</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to Our Site</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </header>
    <section>
        <h2>Latest News</h2>
        <article>
            <h3>Breaking News: Big Event Tomorrow</h3>
            <p>Join us for our biggest event of the year!</p>
        </article>
    </section>
    <footer>
        &copy; 2022 YourSite
    </footer>
</body>
</html>
```

И вот сопутствующий CSS в `styles.css`:

```css
/* Targeting all text on the page */
body {
  font-family: Arial, sans-serif;
  color: #333;
  background-color: #f4f4f4;
}

/* Targeting the header elements */
header {
  background-color: #1e90ff;
  padding: 1em 0;
  text-align: center;
}

/* Targeting the nav elements */
nav ul {
  list-style-type: none;
  padding: 0;
}

/* Targeting the nav links */
nav a {
  text-decoration: none;
  color: #fff;
  margin: 0 10px;
}

/* Targeting the main section */
section {
  padding: 20px;
}

/* Targeting the footer element */
footer {
  text-align: center;
  margin-top: 50px;
  padding: 10px;
  background-color: #1e90ff;
  color: #fff;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Как you include CSS in your HTML document?</summary>

### Как you include CSS in your HTML document?

**Definition:**

Есть **четыре основных способа** включить CSS в HTML-документ, и у каждого свои преимущества и случаи применения.

**How it works:**

#### Способы подключения CSS

1. **Встроенный стиль (inline)**: правила CSS вставляются прямо в HTML-теги.
2. **Внедрённый стиль**: CSS помещается в секцию `<head>` HTML-документа.
3. **Внешняя таблица стилей**: создаётся отдельный файл `.css`, на который ссылается HTML.
4. **Импортированная таблица стилей**: `@import` внутри тега `<style>` или CSS-файла подключает другие CSS-файлы.

#### Ключевые соображения

- **Специфичность**: степень влияния селектора по сравнению с другими.
- **Повторное использование**: возможность применять одни и те же правила CSS к нескольким элементам.
- **Сопровождаемость**: насколько легко обновлять CSS и управлять им.

**When to use:**

#### Лучшие практики

- **В идеале выбирайте один способ**: смешивание приёмов усложняет сопровождение и понимание.
- **Inline-стили для быстрых правок**: удобны, когда стиль нужно изменить быстро.

**Examples:**

#### Пример HTML-кода: Methods Northwind.CSS

Вот HTML-код:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="mystyle.css">
    <title>Document</title>
</head>
    <style>
        h1, h2, p {
            color: green;
        }
        
        div {
            border: 1px solid black;
        }
    </style>
<body>
    <h1>My Header</h1>
    <p>Hello, World!</p>
</body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Можете ли вы explain the difference between class and ID selectors?</summary>

### Можете ли вы explain the difference between class and ID selectors?

**Definition:**

**Селекторы класса** и **селекторы ID** в CSS выполняют разные роли и имеют ограничения в применении.

**How it works:**

#### Избирательность и применимость

- **Селектор класса** (\`.classname{...}\`): совпадает с **несколькими элементами**, у которых один и тот же атрибут class. Эти элементы могут относиться к разным HTML-тегам (например, `<div>`, `<p>`).

- **Селектор ID** (`#idName{...}`): определяет **один уникальный элемент** по его уникальному атрибуту ID. Стилизовать несколько элементов с одним и тем же ID всё ещё возможно, но лучшие практики требуют уникальных ID для корректного использования CSS.

**When to use:**

#### Типичные случаи

- **Селектор класса**: подходит для оформления групп элементов с общими атрибутами или типом.

- **Селектор ID**: обычно оставляют для уникальных элементов, которым нужна очень точная стилизация или работа из JavaScript. Использовать ID для стилей допустимо, как сказано в спецификации HTML5, но обычно удобнее резервировать ID для однозначно идентифицируемых элементов, а для стилей использовать классы.

**Pluses and minuses:**

#### Эффективность и производительность

- **Селектор класса**: как правило, вычисляется быстрее селекторов ID в современных браузерах, особенно когда применяется к большому числу элементов.

- **Селектор ID**: раньше превосходил по скорости, современные браузеры сглаживают эту разницу.

**Examples:**

#### Пример кода: селекторы класса и ID

Вот HTML-код:

```html
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="styles.css" />
  </head>
  <body>
    <div class="content">Content 1</div>
    <div class="content">Content 2</div>
    <div class="content" id="uniqueContent">Special Content</div>
  </body>
</html>
```

Вот CSS-код:

```css
/* Styles applied using class selectors */
.content { color: blue; }

/* Styles applied using ID selector */
#uniqueContent { color: red; }
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Что такое pseudo-Classes in CSS?</summary>

### Что такое pseudo-Classes in CSS?

**Definition:**

**Псевдоклассы** — это специальные ключевые слова в CSS, которые позволяют применять стили к элементам не только по их состоянию или положению в дереве документа, но и по взаимодействию с пользователем.

**How it works:**

#### Категории псевдоклассов

- **Динамические псевдоклассы**: появляются, когда пользователь взаимодействует с элементом. Например, `:hover` срабатывает, когда курсор наводится на элемент.

- **Псевдоклассы действия пользователя**: фиксируют действия пользователя, например `:checked` для выбранных элементов ввода.

- **Псевдоклассы отношений**: относятся к иерархии дерева документа, например `:first-child` для элемента, который является первым дочерним у своего родителя.


- **Языковые псевдоклассы**: для элементов, показанных на определённых языках, например `:dir()`.

- **Псевдоклассы элементов ввода**: созданы специально для интерактивных элементов и стилизуют элементы форм вроде кнопок, полей ввода и текстовых областей. Примеры: `:default`, `:valid`, `:invalid` и `:optional`.

- **Псевдоклассы enabled и disabled**: говорят сами за себя; меняют стиль элементов в зависимости от того, включены они или выключены. Примеры: `:enabled` и `:disabled`.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Опишите how to implement a CSS reset and why it is useful.</summary>

### Опишите how to implement a CSS reset and why it is useful.

**Definition:**

**CSS reset** — это набор стилей, который уменьшает расхождения браузеров в таких вещах, как margins, paddings и разные типичные стили по умолчанию.

**Pluses and minuses:**

#### Польза CSS reset

- **Единая отправная точка**: убирает различия стилей по умолчанию между браузерами и делает процесс дизайна более предсказуемым.
- **Единая блочная модель**: обеспечивает одинаковый расчёт размеров элементов (например, ширины и высоты), чтобы не было неожиданных сдвигов вёрстки.
- **Только свои стили**: особенно полезно, если вы хотите начать с чистого листа и применить собственные стили.

**Examples:**

#### Код

Для HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="styles.css" rel="stylesheet">
  <title>Document</title>
</head>
<body>
  <h1>Hello, CSS Reset!</h1>
</body>
</html>
```

А для CSS вот простой reset на основе **normalize.css**:

```css
/* reset.css */
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */
h1 {
    font-size: 2em;
    margin: 0.67em 0;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Selectors and Combinators

<details>
<summary>Как you select elements by attribute in CSS?</summary>

### Как you select elements by attribute in CSS?

**Definition:**

Программируя на CSS, можно использовать **селекторы атрибутов**, чтобы задавать правила по наличию или значению конкретных HTML-атрибутов.

**How it works:**

#### Варианты

Селекторы атрибутов можно использовать тремя разными способами:

- **Точное совпадение**: `[]` выбирает точное значение атрибута.
- **Значение начинается с**: `[^]` нацелен на атрибуты с заданным началом значения.
- **Без учёта регистра**: селекторы обычно чувствительны к регистру, но с помощью `i` сравнение можно сделать нечувствительным к регистру.

**Pluses and minuses:**

#### Преимущества

У селекторов атрибутов несколько преимуществ, например:

- **Универсальность**: они покрывают широкий круг ситуаций.
- **Простота**: ими легко пользоваться.
- **Согласованность**: они входят в стандартный набор селекторов CSS.

**Examples:**

Вот фрагмент CSS-кода:

```css
/* Exact Match */
[class="important"] {
  color: red;
}

/* Value Starts With */
[href^="https"] {
  color: green;
}

/* Case Insensitive */
[alt="home" i] {
  background: url('home-icon.png');
}
```

В примере выше,

- `[class="important"]` выбирает все элементы, у которых атрибут `class` точно равен "important".
- `[href^="https"]` стилизует все якорные ссылки, у которых атрибут `href` начинается с "https".
- `[alt="home" i]` нацелен на атрибут `alt` со значением "home" без учёта регистра.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Что такое pseudo-element, and what are they used for?</summary>

### Что такое pseudo-element, and what are they used for?

**Definition:**

**Псевдоэлементы** — это виртуальные элементы, которые дают разработчикам возможность стилизовать части HTML-документа, не соответствующие настоящим HTML-элементам. По сути, они позволяют применять стили к конкретным частям элемента без дополнительной HTML-разметки.

Часто используемые `pseudo-elements` включают `::before` и `::after`, которые позволяют вставлять содержимое до или после элемента соответственно.

**How it works:**

#### Ключевые особенности

- **Автоматическая вставка**: эти псевдоэлементы могут добавлять содержимое постоянно, без ручных правок кода.
- **Динамическое содержимое**: с генерируемым содержимым и стилями псевдоэлементы могут подстраиваться под конкретные условия.
- **Своё оформление**: псевдоэлементы позволяют стилизовать части элемента иначе, чем остальное.

**When to use:**

#### Практическое применение

1. #### Обозначение внешних ссылок

   - **Связь**: указание на содержимое, которое открывает внешний сайт.
   - **Реализация**: визуальные или текстовые подсказки вроде стрелок или "External Link" рядом с якорными элементами.

2. #### Особые цифры и буквы

   - **Связь**: оформление одной буквы или цифры внутри текстового блока.
   - **Реализация**: особенно полезно в дизайне, например, чтобы выделить первую букву абзаца более крупным кеглем.

3. #### Адаптивные фоны

   - **Связь**: фоновые изображения или цвета для отдельных частей элемента при разных размерах экрана.
   - **Реализация**: медиазапросы внутри псевдоэлемента для конкретных размеров экрана.

4. #### Блоки кода и цитаты

   - **Связь**: декоративные элементы до и после блоков кода и элементов blockquote.
   - **Реализация**: помогают выделить примеры кода или визуально отделить длинные секции blockquote.

5. #### Свои радиокнопки и чекбоксы

   - **Связь**: переделать стандартный вид радиокнопок и чекбоксов под более индивидуальный.
   - **Реализация**: использовать ::before или ::after со свойством content, чтобы заменить стандартный вид.

6. #### Сброс float

   - **Связь**: решить проблему, когда родительские контейнеры не учитывают высоту плавающих дочерних элементов и схлопываются.
   - **Реализация**: создать элемент через псевдоэлемент ::after, содержимое которого сбрасывает float.

7. #### Хаки для старых браузеров

   - **Связь**: иногда, особенно в прежних версиях Internet Explorer, псевдоэлементы важны, чтобы получить нужное оформление.
   - **Реализация**: полезно применять специально подобранные стили, которые без этого приёма не работали бы как следует в старых браузерах.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Объясните the difference between the child combinator and the descendant combinator.</summary>

### Объясните the difference between the child combinator and the descendant combinator.

**Definition:**

**Дочерний комбинатор** (>) и **комбинатор потомка** (~) оба служат, чтобы выбирать HTML-элементы с помощью CSS. Однако они **работают по-разному**.

**How it works:**

#### Различия

- Дочерний комбинатор **>**: выбирает HTML-элемент, который является **прямым дочерним** другого элемента.
- Комбинатор потомка **~**: совпадает с HTML-элементом, который является **потомком** (прямым или косвенным дочерним) другого указанного элемента.

**When to use:**

#### Лучшие практики использования комбинаторов

1. **Точность выбора**: применяйте **дочерний комбинатор** $>$, когда нужно выбрать конкретного прямого дочернего элемента.
2. **Меньше глобального выбора**: **комбинатор потомка** используйте осторожно, потому что он может выбирать слишком широко. Часто полезнее брать более точные селекторы.
3. **Баланс оформления и производительности**: как правило, более точные селекторы могут ускорить отрисовку. Используйте комбинаторы сбалансированно, помня и о специфичности, и о производительности.

**Examples:**

#### Пример кода

Вот CSS:

```css
/* target direct children of the parent element */
nav > ul > li {
  color: red;
}

/* target any descendant list items under nav */
nav li {
  color: blue;
}
```

Вот HTML:

```html
<nav>
  <ul>
    <li>Direct Child</li> <!-- This is red -->
    <li>
      Nested Child <!-- This is blue -->
      <ul>
        <li>Nested List Item</li> <!-- This is blue -->
      </ul>
    </li>
  </ul>
</nav>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Как бы вы select all direct children elements of a particular type?</summary>

### Как бы вы select all direct children elements of a particular type?

**Definition:**

Чтобы **выбрать всех прямых потомков** определённого типа в CSS, можно использовать **дочерний селектор** `>` вместе с нужным элементом и собрать селектор.

**Examples:**

Например, чтобы выбрать всех прямых потомков, которые являются элементами `<li>` внутри элемента `<ul>`, используют такой CSS:

```css
ul > li {
  /* Styles here */
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Что такое the univeRSAl selector and the sibling combinator, and when would you use them?</summary>

### Что такое the univeRSAl selector and the sibling combinator, and when would you use them?

**Definition:**

**Универсальный селектор** (звёздочка, `*`) — мощный инструмент, который позволяет выбрать каждый элемент внутри заданного контейнера. Селектор простой, но последствия могут быть широкими.

**How it works:**

**Комбинатор соседа** (`+`) в CSS выбирает элементы, непосредственно перед которыми стоит указанный элемент. В отличие от дочернего (`>`) или потомка (пробел) **комбинатор соседа** позволяет выбирать прямого соседа.

**When to use:**

- **Когда использовать**: можно нормализовать или сбросить отдельные свойства CSS (padding, margin и т.д.) у всех элементов внутри контейнера или во всём документе. **Универсальный селектор** как раз это делает.

- **Лучшие практики**: чрезмерное использование **универсального селектора** может привести к проблемам производительности и неожиданным побочным эффектам стилей. Держите его применение коротким и чётко определённым.

#### Когда использовать комбинатор соседа

- **Когда использовать**: для структур DOM, где важны отношения прямых соседей, например вкладки или многошаговые формы.

- **Лучшие практики**: выбор прямого соседа полезен, но убедитесь, что это самый эффективный способ для вашей задачи. Чрезмерная опора на него делает CSS и HTML негибкими.

**Examples:**

#### Пример кода: универсальный селектор

Вот CSS:

```css
/* Remove margins, paddings on all elements within the container */
.containers > * {
    margin: 0;
    padding: 0;
}
```

HTML:

```html
<div class="container">
    <p>Paragraph 1</p>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
</div>
```

#### Пример кода: комбинатор соседа

Вот CSS:

```css
/* Style the direct sibling anchor tag when a list item is hovered */
ul li:hover + a {
    color: red;
}
```

HTML:

```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li><a href="#">Link</a></li>
</ul>
```

#### Разбор случая: практическое применение

Возьмём реальный пример.

#### Сброс margin и padding

В этом случае есть родительский контейнер, и нужно убрать стандартные margin и padding у всех его дочерних элементов.

Это может сделать **универсальный селектор**:

Вот CSS:

```css
.container > * {
    margin: 0;
    padding: 0;
}
```

HTML:

```html
<div class="container">
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</div>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Box Model and Layout

<details>
<summary>Что такое the CSS Box Model?</summary>

### Что такое the CSS Box Model?

**Definition:**

**Блочная модель** — базовое понятие CSS, которое описывает структуру HTML-элемента. Она включает четыре ключевых компонента: **content**, **padding**, **border** и **margin**.

**How it works:**

#### Компоненты блочной модели

- **Content**: собственно содержимое элемента, например текст, изображения или другие визуальные или интерактивные элементы.
- **Padding**: освобождает область вокруг содержимого элемента, внутри границы. Padding прозрачен и не имеет цвета фона или границы.
- **Border**: сплошная линия, которая задаёт границу области padding.
- **Margin**: освобождает область вокруг границы элемента, снаружи любого заданного фона или границы.

#### Наглядное представление

![CSS Box Model](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fcss-box-model.jpg?alt=media&token=f36af6aa-69e3-4cc5-bdce-8a0558d22861)

#### Ключевые атрибуты

- **Height** и **Width**: размеры элемента определяются суммой ширины/высоты содержимого и добавленных padding, border или margin.
- **Border**: задаёт размер, стиль и цвет границы вокруг содержимого и padding.
- **Margin**: определяет **зазор** между соседними элементами.

#### Отступы

#### Автоматические margin
Когда у окружающего контейнера задана ширина, горизонтальные margin со значением "auto" поровну распределяют оставшееся горизонтальное пространство по обе стороны элемента и центрируют его в контейнере.

#### Взаимодействие родителя и потомка

#### Box Sizing

По умолчанию ширина элемента не включает padding и border. CSS может изменить это поведение свойством `box-sizing`:

- **Content-Box** (по умолчанию): указанные width и height элемента считаются без padding и border. Когда вы меняете width или height элемента через CSS, используется именно эта модель.
- **Border-Box**: width и height включают content, padding и border, и блок не разрастается, когда к элементу добавляют padding или border.

Это различие помогает управлять раскладкой и обеспечивает единообразие.

**Examples:**

#### Пример кода: блочная модель

Вот HTML-код:
```html
<div id="boxModelExample">This is an example of text within the Box Model.</div>
```

Вот CSS-код:
``` CSS
#boxModelExample {
    border: 5px solid red;
    padding: 20px;
    margin: 20px;
}
```

``` CSS
#autoMarginExample {
    width: 50%;
    margin-left: auto;
    margin-right: auto;
}
```

``` CSS
#borderBoxExample {
    box-sizing: border-box;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Объясните margin collapsing.</summary>

### Объясните margin collapsing.

**Definition:**

**Схлопывание margin** может произойти, когда у элементов есть соседние вертикальные margin. В таких случаях больший из двух margin становится «схлопнутым margin».

**How it works:**

#### Наглядное представление схлопывания margin

- Схлопывание вертикального margin: 
![Vertical Margin Collapse](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fvartical-margin-collapse.jpg?alt=media&token=7a9f7961-302b-48c7-b74e-168f6780897f)
- Схлопывание горизонтального margin не происходит: ![Horizontal Margin Collapse](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fhorizontal-margin-collapse.jpg?alt=media&token=da23e77b-72aa-4c17-a287-3f274753be9a)

#### Как предотвратить схлопывание margin

1. **Padding или границы**: добавьте padding или границы, чтобы нарушить схлопывание margin, когда это нужно.
2. **Inline-block или float**: превратите соседние элементы в `inline-block` или используйте свойство `float`.
3. **Clearfix**: примените приём очистки для плавающих элементов, который отделяет следующие элементы в потоке и в итоге предотвращает схлопывание margin.

#### Правила CSS и схлопывание margin

1. **Заданный и вычисленный итог**:
   - Если margin заданы, каждый другой margin учитывается.
   - Если margin вычислены как ненулевое значение, margin могут схлопнуться.
   - Учитываются margin и с верхнего, и с нижнего края элементов.
2. **Некорневые элементы**: margin корневого элемента `<html>` не схлопываются.
3. **Несоседние элементы**: margin не схлопывается сквозь обёртки или строчные элементы.

Хотя схлопывание margin может упростить управление раскладкой в определённых условиях, важно понимать его поведение, чтобы дизайн был предсказуемым и согласованным.

**Examples:**

#### Виды схлопывания margin

1. **Схлопывание соседних братьев**: когда два блока или inline-block иерархически соседствуют, например как прямые потомки общего контейнера, их верхние и нижние margin могут схлопнуться. Например, в коде:

   ```html
   <div>
       <p>This is paragraph 1</p>
       <p>This is paragraph 2</p>
   </div>
   ```

2. **Схлопывание родителя и потомка**: если между родителем и первым/последним потомком внутри него нет содержимого, padding или border, margin родителя может схлопнуться с соответствующим margin потомка.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Что такое the different values for the box-sizing property and what do they do?</summary>

### Что такое the different values for the box-sizing property and what do they do?

**Definition:**

У свойства `box-sizing` три возможных значения:

1. **Content-Box**: размеры блока считаются сложением `width` и `height` только с содержимым. Это поведение по умолчанию. Математически это записывается так: $$ \text{Content Width/Height} = \text{Specified Width/Height} $$

2. **Border-Box**: `width` и `height` блока считаются так, что в указанные значения входят и содержимое, и padding. По сути браузер подгоняет **измерения ширины и высоты содержимого, чтобы включить толщину padding и border**. Математически это записывается так: $$ \text{Content Width/Height} = \text{Specified Width/Height} - \text{Padding} - \text{Border} $$

3. **Initial**: это значение ставит свойство `box-sizing` в его значение по умолчанию, которым обычно является `content-box`.

**How it works:**

#### Наглядное представление

![Box-Sizing Properties in CSS](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fcss-box-sizing.jpg?alt=media&token=6bba3876-148a-4b03-a5cc-c96bb8fddb5a)

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Как you center a block element with CSS?</summary>

### Как you center a block element with CSS?

**Definition:**

Чтобы центрировать элемент **блочного уровня** с помощью CSS, есть несколько способов, и они отвечают либо за горизонтальное, либо за вертикальное выравнивание.

**How it works:**

#### Горизонтальное центрирование

Есть три обычных варианта горизонтально центрировать блок:

1. **Автоматические margin**: оба margin, левый и правый, равны `auto`.
2. **Flexbox**: поместить блок во flex-контейнер и задать `justify-content: center`.
3. **Grid**: применить свойство `place-items` к контейнеру с раскладкой grid.

#### Вертикальное центрирование

Чтобы центрировать блок по вертикали, можно использовать современные приёмы вроде Flexbox и Grid.

**Examples:**

#### Пример кода: автоматические margin
```css
.container {
  width: 50%;
  margin: 0 auto; /* Center horizontally */
}
```

#### Пример кода: Flexbox
```css
.container {
  display: flex;
  justify-content: center; /* Horizontally center child elements */
}
```

#### Пример кода: Flexbox
```css
.container {
  display: flex;
  align-items: center; /* Vertically center child elements */
}
```

#### Пример кода: CSS Grid
```css
.container {
  display: grid;
  align-items: center; /* Vertically center child elements */
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Что такое the difference between block, inline, and inline-block elements?</summary>

### Что такое the difference between block, inline, and inline-block elements?

**Definition:**

**Блочные элементы** по умолчанию складываются вертикально, а **строчные элементы** стоят рядом друг с другом на одной строке.

**Inline-block элементы** сочетают черты обоих: складываются вертикально и позволяют менять высоту, margin и padding.

**Examples:**

#### Пример кода: ключевые различия

Вот HTML:

```html
<div class="block">Block</div>
<span class="inline">Inline</span>
<span class="inline-block">Inline-Block</span>
```

Вот CSS:

```css
div, span {
  display: block;
  margin-bottom: 10px;
}

.inline-block {
  display: inline-block;
  background: lightblue;
  padding: 10px;
}
```

У `div` и `span` из-за типа отображения по умолчанию (block) текст разделён
промежутком.

У `span.inline-block`: при `display: inline-block` текст делит строку, но визуально отделён своим фоном.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Positioning and Stacking Contexts

<details>
<summary>Объясните the different values for the position property.</summary>

### Объясните the different values for the position property.

**Definition:**

`position` задаёт способ размещения элемента. Начальное значение — `static`. Остальные значения: `relative`, `absolute`, `fixed` и `sticky`.

**How it works:**

#### Значения

- **static**: обычный поток. `top`, `right`, `bottom`, `left` и `z-index` не действуют.
- **relative**: остаётся в потоке и смещается относительно своей обычной позиции. Место в потоке сохраняется.
- **absolute**: вынимается из потока. Смещения считаются от ближайшего позиционированного предка, а если его нет — от начального содержащего блока.
- **fixed**: вынимается из потока. Смещения считаются от viewport, если у предка `transform`, `perspective`, `filter` или `will-change: transform` не равны `none`. Тогда содержащим блоком становится этот предок.
- **sticky**: остаётся в потоке, пока прокрутка не достигнет порога, затем прилипает. Хотя бы один inset (`top`, `right`, `bottom` или `left`) должен быть не `auto`.

**When to use:**

#### Как выбрать значение

- **relative**: небольшое смещение или содержащий блок для абсолютного потомка.
- **absolute**: бейджи и оверлеи, которым не нужно занимать место в потоке.
- **fixed**: интерфейс, который остаётся в viewport при прокрутке страницы.
- **sticky**: заголовок, который прилипает только внутри своей секции.

**Examples:**

```css
.badge {
  position: absolute;
  top: 0;
  right: 0;
}

.toolbar {
  position: sticky;
  top: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Опишите z-index and how stacking order is controlled in CSS.</summary>

### Опишите z-index and how stacking order is controlled in CSS.

**Definition:**

`z-index` задаёт уровень стека позиционированного элемента, а также flex- или grid-элемента. В одном контексте наложения большее значение рисуется поверх меньшего.

**How it works:**

#### Значения

- **auto**: уровень стека равен `0`. Блок не создаёт новый локальный контекст наложения.
- **integer**: это число и есть уровень стека, в том числе отрицательное. Блок также создаёт локальный контекст наложения, поэтому `z-index` потомков сравнивается только внутри него.

#### Контексты наложения

- Контекст наложения рисуется в родителе как единое целое. Потомок не может выйти из него, каким бы большим ни был его `z-index`.
- `position: fixed` и `position: sticky` всегда создают контекст наложения.
- `position: relative` или `absolute` создаёт его, когда `z-index` не `auto`. То же делает flex- или grid-элемент.
- Другие причины: `opacity` меньше `1`, `isolation: isolate`, а также `transform`, `filter`, `perspective` или `clip-path` со значением не `none`.

**When to use:**

- Держите небольшую шкалу целых чисел (`1`, `2`, `3`) на позиционированных, flex- или grid-блоках, которые реально перекрываются. Сначала поднимите родительский контекст, если потомок должен перекрыть соседа этого родителя.

**Examples:**

```css
.card {
  position: relative;
  z-index: 1;
}

.card .badge {
  position: absolute;
  z-index: 2;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Positioned_layout/Stacking_context
</details>

<details>
<summary>Как you create a sticky footer with CSS?</summary>

### Как you create a sticky footer with CSS?

**Definition:**

Сделайте страницу flex-колонкой не ниже viewport и дайте основной области расти. Подвал тогда стоит у низа короткой страницы и уходит вниз за содержимым на длинной.

**How it works:**

#### Flex-колонка

- **`min-height: 100vh`** у `body` делает колонку не ниже одного viewport.
- **`display: flex`** и **`flex-direction: column`** складывают шапку, основной блок и подвал.
- **`flex: 1`** у `main` — сокращение, которое позволяет элементу расти и сжиматься от нулевого basis, поэтому он забирает свободное место и отталкивает подвал вниз.

**When to use:**

- Подвал сайта и подвал карточки, когда при коротком содержимом подвал не должен висеть в середине viewport.

**Examples:**

```css
body {
  min-height: 100vh;
  margin: 0;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

<details>
<summary>Можете ли вы explain how to create a fixed header that remains at the top on scroll?</summary>

### Можете ли вы explain how to create a fixed header that remains at the top on scroll?

**Definition:**

Задайте шапке `position: fixed` и `top: 0`. Она остаётся у верхнего края viewport, пока документ прокручивается. Добавьте содержимому padding на высоту шапки, чтобы первые строки не оказались под ней.

**How it works:**

#### Фиксированный блок

- **`fixed`** вынимает шапку из потока, место под неё не резервируется.
- Смещения считаются от viewport, если предок не задал `transform`, `perspective`, `filter` или `will-change: transform` значением, отличным от `none`.
- **`left: 0`** и **`right: 0`** растягивают шапку на ширину viewport, когда `width` равен `auto`.
- `fixed` всегда создаёт контекст наложения. При печати элемент ставится в то же место на каждой странице.

**When to use:**

- Общая панель, которая должна быть видна на длинной странице. Если она должна отлипнуть в конце секции, используйте `sticky`.

**Examples:**

```css
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}

main {
  padding-top: 4rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Когда would you use position: sticky?</summary>

### Когда would you use position: sticky?

**Definition:**

`position: sticky` нужен, когда элемент прокручивается вместе со своей секцией, а затем остаётся видимым, пока секция не закончится. На оси прилипания нужен inset, отличный от `auto`.

**How it works:**

#### Порог

- Элемент ведёт себя как `relative`, пока содержащий блок не пересечёт порог inset, затем считается прилипшим.
- Он прилипает внутри ближайшего предка с прокруткой (`overflow` равен `hidden`, `scroll`, `auto` или `overlay`), пока не упрётся в противоположный край своего содержащего блока.
- Если оба inset на оси равны `auto`, на этой оси `sticky` ведёт себя как `relative`.
- `sticky` всегда создаёт контекст наложения.

**When to use:**

- Заголовки алфавитного списка, шапки таблиц и панели внутри секции, которые не должны закрывать следующую секцию. `fixed` оставался бы на всю страницу.

**Examples:**

```css
dt {
  position: sticky;
  top: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

### Styling Text and Fonts

<details>
<summary>Как you change the font of text in CSS?</summary>

### Как you change the font of text in CSS?

**Definition:**

Задайте `font-family` списком через запятую. Браузер берёт первое семейство, которое установлено или загружено через `@font-face`.

**How it works:**

#### Стек шрифтов

- Имена с пробелами берите в кавычки, например `"Trebuchet MS"`.
- Заканчивайте список общим семейством: `serif`, `sans-serif`, `monospace`, `cursive`, `fantasy` или `system-ui`.
- `font-size`, `font-weight`, `font-style` и `line-height` меняют размер и начертание. Сокращение `font` может задать несколько из них одной декларацией.

**When to use:**

- Задайте стек на `body`, чтобы текст его наследовал. Переопределяйте семейство у компонента только когда ему нужен другой шрифт.

**Examples:**

```css
body {
  font-family: "Trebuchet MS", Verdana, sans-serif;
  font-size: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-family
</details>

<details>
<summary>Что такое web-safe font?</summary>

### Что такое web-safe font?

**Definition:**

Веб-безопасный шрифт — семейство, которое уже установлено на многих операционных системах, поэтому страница показывает его без загрузки файла шрифта. Рабочий стек всё равно заканчивается общим семейством.

**How it works:**

#### Запасной вариант

- Браузер идёт по `font-family` слева направо и берёт первое начертание, которое может отрисовать.
- Общие имена вроде `sans-serif`, `serif`, `monospace` и `system-ui` указывают на шрифт, который уже есть в системе.
- В этой роли часто называют Arial, Verdana, Georgia, `"Times New Roman"`, `"Courier New"` и `"Trebuchet MS"`. На каждом устройстве они не гарантированы.

**When to use:**

- Берите такой стек, когда не хотите сетевой запрос ради текста, или как запасной вариант после своего шрифта в `@font-face`.

**Examples:**

```css
body {
  font-family: Verdana, "Trebuchet MS", sans-serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-family
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Text_styling/Fundamentals
</details>

<details>
<summary>Когда would you use a font shorthand property?</summary>

### Когда would you use a font shorthand property?

**Definition:**

Сокращение `font` нужно, когда одна декларация должна задать семейство вместе с размером и при необходимости style, variant, weight, width и line height.

**How it works:**

#### Обязательные части

- `font-size` и `font-family` обязательны, если вы не используете ключевое слово системного шрифта, например `caption`.
- Межстрочный интервал пишется после размера через слэш: `1.2em/1.5`.
- Необязательные значения перед размером: `font-style`, `font-variant`, `font-weight` и `font-width`.
- Любое составное свойство, которое вы не указали, сбрасывается в начальное значение, включая `line-height`. Сокращение также сбрасывает свойства вроде `font-kerning`.

**When to use:**

- Одно правило, которое полностью описывает шрифт, например у заголовка. Отдельные longhand-свойства лучше, когда нужно сменить только насыщенность или размер.

**Examples:**

```css
h1 {
  font: italic bold 1.2em/1.5 "Fira Sans", sans-serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font
</details>

<details>
<summary>Как you include and use custom fonts on a webpage?</summary>

### Как you include and use custom fonts on a webpage?

**Definition:**

Объявите начертание через `@font-face`, дайте ему имя в `font-family` и укажите файл в `src`. Затем используйте это имя в `font-family`.

**How it works:**

#### @font-face

- `src` перечисляет файлы, из которых браузер может выбрать. Предпочитайте WOFF2 и добавляйте `format("woff2")`, чтобы неподдерживаемые форматы пропускались.
- `font-weight` и `font-style` в at-правиле говорят, какой файл соответствует `bold` или `italic`.
- `font-display: swap` сразу показывает запасной текст, а затем подменяет его своим шрифтом, когда тот загрузится.
- Ставьте `@font-face` раньше правил, которые используют это семейство.

**When to use:**

- Фирменные шрифты и шрифты для чтения, которых нет в системе пользователя. В конце стека оставьте общее семейство.

**Examples:**

```css
@font-face {
  font-family: "Zantroke";
  src: url("fonts/zantroke.woff2") format("woff2");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

body {
  font-family: "Zantroke", serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@font-face
</details>

<details>
<summary>Что такое the difference between em and rem units?</summary>

### Что такое the difference between em and rem units?

**Definition:**

`em` считается от размера шрифта элемента. `rem` считается от размера шрифта корневого элемента.

**How it works:**

#### Длины от шрифта

- В самом `font-size` единица `em` берёт унаследованный размер, а не тот, который сейчас объявляется.
- В других свойствах, например `margin` или `padding`, `em` берёт вычисленный `font-size` элемента. Вложенные `em` перемножаются.
- `rem` берёт `font-size` корневого элемента, обычно `html`. В `font-size` самого корня `rem` означает начальный размер. Обычное значение браузера — `16px`, но настройки пользователя могут его изменить.
- Потомки наследуют вычисленную длину, а не токен `em` или `rem`.

**When to use:**

- `rem` для шкалы текста, отступов и раскладки, которые должны следовать корневому размеру шрифта пользователя.
- `em` когда padding компонента или размер иконки должны масштабироваться вместе с его собственным `font-size`.

**Examples:**

```css
html {
  font-size: 100%;
}

h1 {
  font-size: 2rem;
  margin-bottom: 0.5em;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/length
</details>

### Colors, Backgrounds, and Gradients

<details>
<summary>Как you add a background image to an element?</summary>

### Как you add a background image to an element?

**Definition:**

Задайте `background-image` через `url()` или градиент. Изображение рисуется за содержимым и padding и поверх `background-color`.

**How it works:**

#### Как рисуется изображение

- `background-repeat` управляет повтором. `no-repeat` показывает изображение один раз.
- `background-position` ставит его на место. `background-size` масштабирует. `cover` заполняет блок и может обрезать; `contain` целиком помещает изображение в блок.
- Сокращение `background` может объединить цвет, изображение, позицию, размер, повтор и origin в одной декларации.

**When to use:**

- Декоративные изображения, которые не являются содержимым. Смысловые картинки ставьте в `<img>`, чтобы у них был альтернативный текст.

**Examples:**

```css
.hero {
  background-image: url("hero.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background-image
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background
</details>

<details>
<summary>Что такое the difference between hex codes, RGB, and named color values?</summary>

### Что такое the difference between hex codes, RGB, and named color values?

**Definition:**

Все три способа записывают цвет в sRGB. Именованный цвет — ключевое слово, hex упаковывает каналы шестнадцатеричными цифрами, а `rgb()` записывает каналы числами или процентами.

**How it works:**

#### Записи

- **Именованные**: ключевые слова вроде `rebeccapurple` и `aliceblue`. Набор фиксирован CSS.
- **Hex**: `#rgb` — краткая форма `#rrggbb`. Каждая пара — красный, зелёный и синий от `00` до `ff`.
- **rgb()**: `rgb(255 0 153)` или `rgb(100% 0% 60%)`. Современная форма разделяет каналы пробелами. Необязательная альфа идёт после слэша: `rgb(255 0 153 / 80%)`.
- `hsl()`, `hwb()`, `lab()`, `lch()`, `oklab()` и `oklch()` — другие цветовые функции. Это не hex, не `rgb()` и не именованные цвета.

**When to use:**

- Именованные цвета для нескольких известных ключевых слов. Hex или `rgb()`, когда нужен точный канал. Внутри дизайн-системы лучше одна запись.

**Examples:**

```css
.swatch {
  color: rebeccapurple;
  background-color: #ff0099;
  border-color: rgb(255 0 153 / 80%);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value
</details>

<details>
<summary>Что такое alpha transparency and RGBA?</summary>

### Что такое alpha transparency and RGBA?

**Definition:**

Альфа — канал непрозрачности цвета. `0` полностью прозрачен, `1` или `100%` полностью непрозрачны. `rgba()` — старое имя функции для RGB-цвета с этим каналом. Современная форма — `rgb()` со слэшем.

**How it works:**

#### Альфа-канал

- `rgb(189 85 218 / 0.25)` и `rgb(189 85 218 / 25%)` рисуют те же красный, зелёный и синий с четвертью непрозрачности.
- Цвет накладывается на то, что находится за элементом. Он не делает прозрачными потомков, в отличие от `opacity`.
- `opacity` действует на весь элемент, включая потомков. Альфа цвета действует только на этот цвет.

**When to use:**

- Полупрозрачная заливка, граница или цвет текста. `opacity` — когда должен побледнеть весь элемент вместе с потомками.

**Examples:**

```css
.panel {
  background-color: rgb(189 85 218 / 0.25);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value/rgb
</details>

<details>
<summary>Как you create a gradient background with CSS?</summary>

### Как you create a gradient background with CSS?

**Definition:**

Градиент — изображение, которое генерирует CSS. Его задают через `background-image` или сокращение `background`. `linear-gradient()` смешивает цвета вдоль линии.

**How it works:**

#### Функции градиента

- Первый аргумент `linear-gradient()` — угол, например `45deg`, или сторона, например `to left top`. Дальше идут цветовые остановки.
- У остановки можно указать позицию: `green 40%`. Две остановки в одной позиции дают жёсткую границу.
- `radial-gradient()` смешивает от центра наружу. `conic-gradient()` смешивает вокруг центра.
- У градиента нет собственного размера. `background-size` и позиция по умолчанию решают, как он заполнит блок.

**When to use:**

- Переходы, полосы и декоративные заливки, которые должны масштабироваться вместе с элементом. Файл изображения для них не нужен.

**Examples:**

```css
.banner {
  background-image: linear-gradient(45deg, blue, red);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/gradient/linear-gradient
</details>

<details>
<summary>Объясните how to implement multiple backgrounds on a single element.</summary>

### Объясните how to implement multiple backgrounds on a single element.

**Definition:**

Передайте в `background-image` или `background` список через запятую. Первое изображение рисуется сверху. Следующие лежат под ним.

**How it works:**

#### Слои

- У каждого слоя через запятую могут быть свои позиция, размер и повтор. Перечисляйте эти значения в том же порядке, что и изображения.
- `background-color` рисуется под всеми изображениями. В сокращении `background` цвет может быть только у последнего слоя.
- Градиент считается изображением, поэтому он может стоять в одном списке с `url()`.

**When to use:**

- Фото плюс тонировка или узор поверх градиента на одном элементе.

**Examples:**

```css
.panel {
  background-image:
    linear-gradient(rgb(0 0 0 / 40%), rgb(0 0 0 / 40%)),
    url("photo.png");
  background-size: cover, cover;
  background-color: #222;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background-image
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background
</details>

### Flexbox

<details>
<summary>Опишите Flexbox and its advantages.</summary>

### Опишите Flexbox and its advantages.

**Definition:**

Flexbox — одномерная раскладка. `display: flex` делает элемент flex-контейнером, а его детей в потоке — flex-элементами на главной оси и поперечной оси.

**How it works:**

#### Оси

- `flex-direction` задаёт главную ось. Начальное значение — `row`, поэтому поперечная ось идёт вертикально. `column` меняет оси местами.
- `justify-content` распределяет свободное место по главной оси. `align-items` выравнивает элементы по поперечной оси. Начальное значение `align-items` — `stretch`.
- `flex-grow`, `flex-shrink` и `flex-basis` (или сокращение `flex`) решают, как элементы делят место. `gap` добавляет промежуток между элементами.

**When to use:**

- Ряд или колонка: навигация, панели, центрирование и карточки, которые делят остаток места. Grid нужен, когда дорожки нужны и по строкам, и по колонкам.

**Examples:**

```css
.nav {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
</details>

<details>
<summary>Как flex-grow work?</summary>

### Как flex-grow work?

**Definition:**

`flex-grow` — коэффициент роста flex-элемента. Он задаёт, какая доля положительного свободного места контейнера добавляется к основному размеру элемента. Начальное значение — `0`, поэтому элемент не растёт.

**How it works:**

#### Доля

- Положительное свободное место — основной размер контейнера минус основные размеры элементов.
- Если у всех элементов одинаковый коэффициент больше `0`, это место делится поровну. Число — отношение: `1` и `1` совпадают с `2` и `2`.
- Если коэффициенты разные, доля элемента равна его коэффициенту, делённому на сумму коэффициентов.
- Отрицательные значения недопустимы. Предпочтительнее сокращение `flex`, чтобы вместе задать `flex-shrink` и `flex-basis`. `flex: 1` означает рост `1`, сжатие `1` и basis `0`.

**When to use:**

- Элемент, который должен забрать остаток места, например растягиваемое поле рядом с кнопкой. Оставляйте `flex-grow: 0` у элементов, которые должны остаться размером содержимого.

**Examples:**

```css
.row {
  display: flex;
}

.grow {
  flex-grow: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

<details>
<summary>Объясните the difference between justify-content and align-items.</summary>

### Объясните the difference between justify-content and align-items.

**Definition:**

`justify-content` выравнивает flex-элементы по главной оси. `align-items` выравнивает их по поперечной оси.

**How it works:**

#### Какая ось

- При начальном `flex-direction: row` главная ось горизонтальна, поэтому `justify-content` двигает элементы влево и вправо. `align-items` двигает их вверх и вниз.
- При `flex-direction: column` оси меняются: `justify-content` вертикален, а `align-items` горизонтален.
- Частые значения `justify-content`: `flex-start`, `flex-end`, `center`, `space-between`, `space-around` и `space-evenly`.
- Начальное значение `align-items` — `stretch`. Другие значения: `flex-start`, `flex-end`, `center` и `baseline`.

**When to use:**

- `justify-content` для свободного места между элементами в ряду или колонке. `align-items` чтобы выстроить все элементы по поперечной оси, в том числе вертикально по центру ряда.

**Examples:**

```css
.box {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Aligning_items
</details>

<details>
<summary>Когда would you use align-self?</summary>

### Когда would you use align-self?

**Definition:**

`align-self` переопределяет `align-items` для одного flex-элемента на поперечной оси. Начальное значение — `auto`: берётся `align-items` контейнера.

**How it works:**

#### Один элемент

- Допустимые значения совпадают с `align-items`, включая `stretch`, `flex-start`, `flex-end`, `center` и `baseline`.
- Главную ось он не меняет. Положение одного элемента на главной оси задают `margin: auto` или коэффициенты `flex`.
- В колонке поперечная ось горизонтальна, поэтому `align-self` тогда управляет горизонтальным выравниванием.

**When to use:**

- Один элемент ряда должен быть по центру, растянут или прижат к началу, пока остальные следуют `align-items`.

**Examples:**

```css
.box {
  display: flex;
  align-items: flex-start;
}

.box .selected {
  align-self: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Aligning_items
</details>

<details>
<summary>Как you create equal-width columns using Flexbox?</summary>

### Как you create equal-width columns using Flexbox?

**Definition:**

Сделайте контейнер `display: flex` и задайте каждой колонке `flex: 1`. Это сокращение даёт всем элементам одинаковый basis `0`, а затем делит свободное место поровну.

**How it works:**

#### Равные колонки

- `flex: 1` раскрывается в рост `1`, сжатие `1` и basis `0`. Элементы стартуют от нуля и делят контейнер, поэтому более длинный текст не делает колонку шире.
- Один `flex-grow: 1` растит от размера содержимого (`flex-basis: auto`). Колонки с большим содержимым остаются шире.
- `flex: auto` — это `1 1 auto`. Элементы получают одинаковую добавку свободного места, но basis остаётся размером содержимого, поэтому при разном тексте они не равны.

**When to use:**

- Один ряд равных колонок. Для переносящихся рядов с минимальной шириной колонки лучше подходит Grid с `repeat(auto-fit, minmax(...))`.

**Examples:**

```css
.container {
  display: flex;
}

.container > * {
  flex: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

### CSS Grid

<details>
<summary>Что такое the CSS Grid layout, and why is it useful?</summary>

### Что такое the CSS Grid layout, and why is it useful?

**Definition:**

CSS Grid — двумерная раскладка. `display: grid` делит контейнер на строки и колонки и помещает детей в эти ячейки.

**How it works:**

#### Дорожки

- `grid-template-columns` и `grid-template-rows` задают дорожки. Единица `fr` делит свободное место. `repeat()` повторяет список дорожек.
- Элементы сами занимают ячейки. `grid-column`, `grid-row` и `grid-area` ставят элемент или растягивают его явно.
- `gap` задаёт промежуток между дорожками. Grid-элементы могут перекрываться, если их области делят ячейки.

**When to use:**

- Области страницы, сетки карточек и любая раскладка, где важно выравнивание и по горизонтали, и по вертикали. Для одного ряда или колонки хватает Flexbox.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout
</details>

<details>
<summary>Как you define grid columns and rows?</summary>

### Как you define grid columns and rows?

**Definition:**

Задайте `grid-template-columns` и `grid-template-rows` на grid-контейнере. Каждое значение в списке — одна дорожка.

**How it works:**

#### Размеры дорожек

- Длина вроде `100px`, процент или `1fr` задают размер дорожки. `fr` — доля свободного места после фиксированных дорожек.
- `repeat(3, 1fr)` — три равные колонки. `repeat(auto-fill, minmax(120px, 1fr))` добавляет столько колонок, сколько помещается, каждая не уже `120px`.
- `grid-auto-rows` задаёт размер строк, которые создаются неявно, когда элементов больше, чем явных строк.

**When to use:**

- Задавайте обе оси, когда структура известна. Оставляйте строки на `grid-auto-rows`, когда фиксировано только число колонок, а строки должны расти вместе с элементами.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 100px 100px;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-rows
</details>

<details>
<summary>Что такое the difference between grid-template-areas and grid-template-columns?</summary>

### Что такое the difference between grid-template-areas and grid-template-columns?

**Definition:**

`grid-template-columns` задаёт размеры колонок. `grid-template-areas` именует прямоугольные области из этих ячеек и само по себе размеры дорожек не задаёт.

**How it works:**

#### Имена и размеры

- Каждая строка в `grid-template-areas` — один ряд сетки. Повтор имени в прямоугольнике, например `"a a b"`, образует одну область. Точка `.` — пустая ячейка.
- Элемент занимает область через `grid-area: a`.
- Число колонок в каждой строке должно совпадать. Сочетайте области с `grid-template-columns`, когда дорожкам нужны явные размеры, например `1fr`.

**When to use:**

- `grid-template-areas`, когда раскладка — набор именованных областей, например header, sidebar и content. Одного `grid-template-columns` достаточно, когда нужны только равные или гибкие дорожки и автоматическая расстановка.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-areas:
    "header header"
    "nav content";
}

header {
  grid-area: header;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-areas
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
</details>

<details>
<summary>Как you place items in the CSS Grid layout?</summary>

### Как you place items in the CSS Grid layout?

**Definition:**

Элемент ставят, называя линии сетки, которые он занимает. `grid-column` и `grid-row` — сокращения. `grid-area` может назвать область или задать все четыре линии.

**How it works:**

#### Номера линий

- Линии нумеруются с `1`. `grid-column: 1 / 3` начинается на линии 1 и заканчивается на линии 3, поэтому элемент занимает две колонки.
- `span 2` покрывает две дорожки от начальной линии.
- `grid-area: header` использует имя из `grid-template-areas`.
- Элементы без явной позиции автоматически попадают в следующую свободную ячейку.

**When to use:**

- Явная расстановка для каркаса страницы. Остальные элементы оставляйте авторазмещению, если порядок в разметке и есть нужный порядок.

**Examples:**

```css
.box1 {
  grid-column: 1 / 2;
  grid-row: 1 / 4;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Line-based_placement
</details>

<details>
<summary>Как overlapping of grid items handled in CSS Grid?</summary>

### Как overlapping of grid items handled in CSS Grid?

**Definition:**

Grid-элементы перекрываются, когда их диапазоны колонок и строк покрывают одни и те же ячейки. В одном контексте наложения `z-index` решает, какой элемент рисуется сверху. Grid-элемент может использовать `z-index`, даже если `position` равен `static`.

**How it works:**

#### Порядок отрисовки

- Если у обоих элементов `z-index: auto`, тот, что позже в разметке, рисуется поверх более раннего.
- Целый `z-index` задаёт уровень стека и создаёт контекст наложения для этого grid-элемента.
- Отрицательный `z-index` рисует элемент под соседями с `auto` или с большим числом.

**When to use:**

- Оверлеи внутри ячейки, например подпись поверх плитки. Не полагайтесь на порядок в разметке, если перекрытие должно оставаться стабильным при изменении HTML.

**Examples:**

```css
.box1 {
  grid-column: 1 / 4;
  grid-row: 1 / 3;
  z-index: 2;
}

.box2 {
  grid-column: 1;
  grid-row: 2 / 4;
  z-index: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Basic_concepts
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index
</details>

### Responsive Design and Media Queries

<details>
<summary>Что такое media query?</summary>

### Что такое media query?

**Definition:**

Медиазапрос применяет CSS только когда браузер и устройство совпадают с условием, например viewport шире заданной длины.

**How it works:**

#### @media

- At-правило — `@media`, затем необязательный тип носителя (`screen`, `print`, `all`) и возможности в скобках.
- `(min-width: 35rem)` совпадает, когда ширина viewport не меньше этой длины. `(max-width: 50rem)` — когда она не больше.
- Форма диапазона `(width >= 480px)` — тот же вид проверки, что и `min-width`.
- Возможности проверяют не только ширину, например `prefers-reduced-motion` или `hover`.

**When to use:**

- Разная раскладка для разной ширины viewport и отдельный набор правил для печати.

**Examples:**

```css
@media (min-width: 35rem) {
  .layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>Как the viewport meta tag assist with mobile responsive designs?</summary>

### Как the viewport meta tag assist with mobile responsive designs?

**Definition:**

`<meta>` viewport говорит мобильному браузеру верстать страницу по ширине устройства в CSS-пикселях, а не по широкому виртуальному viewport, обычно около `980px`, который затем уменьшается.

**How it works:**

#### width=device-width

- Без тега узкий экран всё равно использует широкий layout viewport, и настольный CSS выглядит мелким.
- `content="width=device-width"` задаёт эту ширину равной ширине экрана. Медиазапросы тогда видят реальную ширину устройства.
- `initial-scale=1` задаёт начальный масштаб. Обычный тег включает оба значения.

**When to use:**

- Каждый документ с адаптивным CSS. Тег ставят в `<head>`.

**Examples:**

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>Опишите how to create a mobile-first responsive design.</summary>

### Опишите how to create a mobile-first responsive design.

**Definition:**

Сначала пишите базовый CSS для узкого viewport, затем добавляйте раскладку для широких экранов внутри медиазапросов `min-width`.

**How it works:**

#### Сначала узкий экран

- Правила по умолчанию — раскладка маленького экрана: одна колонка, читаемый текст и отступы, которые помещаются на телефоне.
- `@media (min-width: 480px)` и более крупные точки добавляют колонки, навигацию в ряд и более широкие отступы. Они не отменяют настольную раскладку.
- Синтаксис диапазона `(width >= 480px)` — та же идея: стили действуют от этой ширины и выше.
- В meta viewport должен быть `width=device-width`, иначе узкая база всё равно верстается в широком виртуальном viewport.

**When to use:**

- Новые раскладки. Усиление снизу вверх оставляет CSS маленького экрана короче и не заставляет бороться с переопределениями `max-width`.

**Examples:**

```css
.layout {
  display: grid;
  gap: 1rem;
}

@media (width >= 480px) {
  .layout {
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>Как you build a layout that changes from a two-column to a single-column with media queries?</summary>

### Как you build a layout that changes from a two-column to a single-column with media queries?

**Definition:**

Начните с двух колонок, а внутри запроса `max-width` верните сетку к одной колонке. Вариант mobile-first делает наоборот: одна колонка по умолчанию и две колонки от точки `min-width`.

**How it works:**

#### Точка перелома

- `(max-width: 40rem)` совпадает с viewport шириной `40rem` и уже, поэтому там побеждает одна колонка.
- `(min-width: 40rem)` совпадает с viewport не уже `40rem`, поэтому две колонки — это усиление.
- Чтобы сменить раскладку, достаточно одного свойства: `grid-template-columns`.

**When to use:**

- Для новой работы предпочтительна форма `min-width`. `max-width` уместен, когда уже существующие две колонки должны схлопнуться на маленьком экране.

**Examples:**

```css
.layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

@media (max-width: 40rem) {
  .layout {
    grid-template-columns: 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>Объясните the use of min-width and max-width in media queries.</summary>

### Объясните the use of min-width and max-width in media queries.

**Definition:**

`min-width` делает запрос истинным на этой ширине viewport и шире. `max-width` делает его истинным на этой ширине и уже.

**How it works:**

#### Направление

- `@media (min-width: 35rem)` — проверка mobile-first: базовые стили действуют для всех, а этот блок добавляет стили для более широких viewport.
- `@media (max-width: 50rem)` — проверка desktop-first: она переопределяет стили, только пока viewport достаточно узкий.
- Синтаксис диапазона записывает те же проверки как `(width >= 35rem)` и `(width <= 50rem)`.
- Полоса использует оба, например `(min-width: 35rem) and (max-width: 50rem)`.

**When to use:**

- `min-width`, когда каждая точка добавляет раскладку. `max-width`, когда широкую раскладку нужно упростить на меньших экранах.

**Examples:**

```css
@media (min-width: 35rem) {
  body {
    background: yellow;
  }
}

@media (max-width: 50rem) {
  body {
    border: 2px solid blue;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

### Transitions and Animations

<details>
<summary>Как you create a CSS transition?</summary>

### Как you create a CSS transition?

**Definition:**

Переход интерполирует свойство от старого вычисленного значения к новому, когда это значение меняется. Сокращение `transition` задаёт свойство, длительность, плавность и задержку.

**How it works:**

#### Порядок сокращения

- `transition: margin-right 4s ease-in-out 1s` — это свойство, длительность, плавность, затем задержка.
- Для видимого изменения нужна длительность. Начальная длительность — `0s`.
- Несколько свойств пишут через запятую. `transition-property: all` допустимо, но анимирует каждое анимируемое свойство, которое изменилось.
- Изменение обычно приходит из состояния: `:hover`, `:focus-visible` или класс.

**When to use:**

- Короткий отклик на смену состояния, например цвет или transform при наведении. Анимацию берите, когда у движения свои ключевые кадры или оно должно повторяться.

**Examples:**

```css
.button {
  transition: background-color 200ms ease-out;
}

.button:hover {
  background-color: blue;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
</details>

<details>
<summary>Что такое the difference between a CSS transition and animation?</summary>

### Что такое the difference between a CSS transition and animation?

**Definition:**

Переход идёт между двумя вычисленными значениями, когда свойство меняется. Анимация проигрывает последовательность `@keyframes` и может стартовать сама, повторяться и идти в обратную сторону.

**How it works:**

#### Что нужно каждому

- Переходу нужны длительность и смена анимируемого свойства. Промежуточные остановки он не задаёт.
- Анимации нужно имя `animation-name`, указывающее на `@keyframes`, и длительность. `animation-iteration-count`, `animation-direction` и `animation-fill-mode` управляют повтором, реверсом и стилями до и после проигрывания.
- Оба используют функцию плавности: `transition-timing-function` или `animation-timing-function`. Начальное значение ключевого слова — `ease`.

**When to use:**

- Переходы для наведения, фокуса и раскрывающихся панелей. Анимации для цикла, многошаговой последовательности или движения, которое должно начаться при появлении элемента.

**Examples:**

```css
.box {
  transition: transform 200ms ease-out;
}

.sun {
  animation: 4s linear infinite alternate sunrise;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation
</details>

<details>
<summary>Как you animate an item's entrance and exit on the page?</summary>

### Как you animate an item's entrance and exit on the page?

**Definition:**

Появление анимируют через `@keyframes` или переход из скрытого состояния, обычно `opacity` и `transform`. Уход можно перевести только пока элемент ещё в документе.

**How it works:**

#### Вход и выход

- `transform` и `opacity` можно анимировать без повторной раскладки страницы. Их лучше, чем анимация `top`, `width` или `height`.
- Для появления `@keyframes` может идти от `opacity: 0` и `translateY(1rem)` к видимым значениям, как только элемент показан.
- Для ухода меняйте эти свойства через `transition` до удаления элемента. Удаление узла из документа сразу обрывает переход.
- Дискретные свойства вроде `display` должны получить `allow-discrete` в переходе, если они в нём участвуют.

**When to use:**

- Тост, диалог или панель. При `prefers-reduced-motion: reduce` сократите или уберите движение.

**Examples:**

```css
.panel {
  opacity: 0;
  transform: translateY(1rem);
  transition: opacity 200ms ease-out, transform 200ms ease-out;
}

.panel.is-open {
  opacity: 1;
  transform: none;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes
</details>

<details>
<summary>Можете ли вы explain how to use @keyframes?</summary>

### Можете ли вы explain how to use @keyframes?

**Definition:**

`@keyframes` называет последовательность стилей. Сокращение `animation` или `animation-name` проигрывает её на элементе.

**How it works:**

#### Остановки

- `from` и `to` — это `0%` и `100%`. Промежуточные остановки пишут процентами, например `50%`.
- В каждой остановке перечисляют свойства для интерполяции. Свойство, которого нет в остановке, интерполируется от соседних остановок, если оно анимируемо.
- `animation: 4s linear infinite alternate sunrise` задаёт длительность, плавность, число повторов, направление и имя ключевых кадров.
- Если `animation-fill-mode` равен `forwards`, элемент сохраняет стили последнего кадра после конца анимации.

**When to use:**

- Движение больше чем из двух состояний или движение, которое должно идти без наведения и смены класса.

**Examples:**

```css
@keyframes sunrise {
  from {
    transform: translateY(110vh);
  }
  to {
    transform: translateY(0);
  }
}

.sun {
  animation: 4s linear infinite alternate sunrise;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation
</details>

<details>
<summary>Как you set the timing functions in CSS animations?</summary>

### Как you set the timing functions in CSS animations?

**Definition:**

`animation-timing-function` задаёт кривую плавности анимации. Те же кривые есть у `transition-timing-function`. Начальное значение — `ease`.

**How it works:**

#### Кривые

- `linear` движется с постоянной скоростью. Он равен `cubic-bezier(0, 0, 1, 1)`.
- `ease` ускоряется в середине и замедляется в конце. `ease-in` стартует медленно. `ease-out` стартует быстро и замедляется. `ease-in-out` делает и то и другое.
- `cubic-bezier()` принимает четыре числа. Первое и третье должны быть от `0` до `1`.
- `steps(n, jump-end)` держит значение равными интервалами. `jump-end` — позиция шага по умолчанию. `step-end` равен `steps(1, jump-end)`.
- Плавность может быть и третьим значением сокращения `animation`, как в `animation: 4s linear sunrise`.

**When to use:**

- `ease-out` для элемента, который появляется на экране. `linear` для непрерывного цикла. `steps()` для покадрового спрайта.

**Examples:**

```css
.sun {
  animation-name: sunrise;
  animation-duration: 4s;
  animation-timing-function: linear;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation-timing-function
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/easing-function
</details>

### Advanced CSS Techniques

<details>
<summary>Что такое CSS preprocessor and why would you use one?</summary>

### Что такое CSS preprocessor and why would you use one?

**Definition:**

Препроцессор CSS — язык, который компилируется в CSS. Обычный пример — Sass. Он добавляет переменные, вложенность, примеси и функции, которые браузер сам не разбирает.

**How it works:**

#### Шаг компиляции

- Вы пишете `.scss` (или другой синтаксис препроцессора). Сборка отдаёт браузеру обычный `.css`.
- Переменные Sass (`$color`) вычисляются при компиляции. Они не меняются в браузере и не наследуются так, как пользовательские свойства.
- Вложенность в исходнике становится отдельными селекторами в результате. Глубокая вложенность повышает специфичность и удлиняет CSS.
- Браузер видит только сгенерированный CSS, поэтому препроцессор не добавляет возможностей во время выполнения.

**When to use:**

- Кодовая база, которая уже собирает Sass, или общие примеси, которые нужно развернуть до отдачи. Для значений, которые должны следовать каскаду или меняться с темой в браузере, используйте пользовательские свойства.

**Examples:**

```scss
$brand: #1e90ff;

.button {
  background: $brand;

  &:hover {
    background: darken($brand, 8%);
  }
}
```

**Sources:**

- https://sass-lang.com/documentation/
</details>

<details>
<summary>Опишите the importance of using CSS variables (custom properties).</summary>

### Опишите the importance of using CSS variables (custom properties).

**Definition:**

Пользовательские свойства — значения, которые задаёт автор: их пишут как `--name` и читают через `var()`. Они участвуют в каскаде и наследуются, поэтому одно объявление может оформить целое поддерево.

**How it works:**

#### Каскад

- Задайте `--primary-color: blue` на `html` или на компоненте. Потомки читают его как `color: var(--primary-color)`.
- `var(--name, fallback)` берёт запасное значение, если свойство не задано.
- Смена свойства на родителе обновляет все использования. Это не переменная Sass, которая исчезает после компиляции.
- At-правило `@property` может задать синтаксис, начальное значение и наследование. Тогда браузер умеет интерполировать свойство в анимациях.

**When to use:**

- Токены дизайна: цвет, отступы и шрифт, общие для тем и компонентов. Медиазапрос или класс на предке может подменить токены без переписывания каждого правила.

**Examples:**

```css
html {
  --primary-color: red;
}

h1 {
  color: var(--primary-color);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
</details>

<details>
<summary>Как you use CSS modules?</summary>

### Как you use CSS modules?

**Definition:**

CSS Modules по умолчанию ограничивают имена классов и анимаций файлом, поэтому два компонента могут оба использовать `.button` без столкновения. Сборщик импортирует файл как объект сгенерированных имён классов.

**How it works:**

#### Локальные имена

- Класс в `Button.module.css` локальный. Сборка переписывает `.button` в уникальное имя и отдаёт его как `styles.button`.
- Эту строку ставят на элемент: `class={styles.button}`. Браузер по-прежнему получает обычный CSS.
- Класс с пометкой `:global(.theme)` сохраняет написанное имя, чтобы совпасть с разметкой вне модуля.
- Композиция — возможность сборки. Это не свойство CSS, которое браузер понимает сам.

**When to use:**

- CSS компонента в сборщике (webpack, Vite или компилятор фреймворка), когда нужна область видимости файла без библиотеки CSS-in-JS во время выполнения.

**Examples:**

```css
.button {
  color: white;
  background: #1e90ff;
}
```

```js
import styles from "./Button.module.css";

document.querySelector("button").className = styles.button;
```

**Sources:**

- https://github.com/css-modules/css-modules
</details>

<details>
<summary>Можете ли вы explain the concept of CSS-in-JS?</summary>

### Можете ли вы explain the concept of CSS-in-JS?

**Definition:**

CSS-in-JS пишет правила стилей на JavaScript, обычно рядом с компонентом. Библиотека генерирует имена классов и вставляет или извлекает CSS.

**How it works:**

#### Стили рядом с компонентом

- Стиль может читать пропсы компонента и значения темы, потому что он на том же языке, что и интерфейс.
- Многие библиотеки хешируют имя класса и дают такую же локальную область, как CSS Modules.
- Одни библиотеки вставляют тег `<style>` во время выполнения. Другие извлекают статический CSS при сборке, чтобы браузер не платил за это в рантайме.
- Результат всё равно CSS. Специфичность, каскад и раскладка работают как обычно, когда правила попали в документ.

**When to use:**

- Система компонентов, которая уже задаёт стили из JavaScript и должна менять их от пропсов. Обычный CSS или CSS Modules лучше, когда стили статичны и рантайм стилей не нужен.

**Examples:**

```js
const Button = styled.button`
  color: white;
  background: #1e90ff;
`;
```

**Sources:**

- https://styled-components.com/docs/basics
</details>

<details>
<summary>Какие инструменты would you use for CSS linting and formatting?</summary>

### Какие инструменты would you use for CSS linting and formatting?

**Definition:**

Stylelint ловит неверный или нежелательный CSS, а Prettier его форматирует. Это разные задачи: один сообщает о проблемах, другой переписывает пробелы.

**How it works:**

#### Линт и формат

- Stylelint разбирает CSS, SCSS и CSS-in-JS и сообщает о неизвестных свойствах, повторных селекторах и правилах, которые вы включили, например запрет `!important`.
- Prettier перепечатывает файл в едином виде. Он не решает, допустимо ли свойство.
- Запускайте оба в редакторе и в CI, чтобы запрос на слияние падал из-за ошибки линта. Отдайте форматирование Prettier и выключите в Stylelint правила, которые смотрят только на пробелы.
- Инструменты разработчика в браузере показывают каскад одного элемента. Они не заменяют линт исходников.

**When to use:**

- Любая общая таблица стилей. Stylelint нужен, когда CSS правит больше одного человека. Prettier нужен, когда спор только о том, как файл выглядит.

**Examples:**

```json
{
  "extends": ["stylelint-config-standard"]
}
```

**Sources:**

- https://stylelint.io/
- https://prettier.io/docs/
</details>

### Styling Forms and User Interfaces

<details>
<summary>Как you style a submit button to appear as a regular link?</summary>

### Как you style a submit button to appear as a regular link?

**Definition:**

Оставьте `<button type="submit">` и уберите нативную оболочку кнопки: без фона, без границы и без лишнего padding. Затем задайте цвет ссылки и подчёркивание.

**How it works:**

#### Вид ссылки, поведение кнопки

- `background: none`, `border: 0` и `padding: 0` убирают плашку кнопки.
- `font: inherit` и `color` отменяют шрифт и цвет кнопки по умолчанию.
- `text-decoration: underline` и `cursor: pointer` совпадают со ссылкой.
- Элемент остаётся кнопкой, поэтому Enter в форме по-прежнему отправляет её, а доступная роль остаётся кнопкой.

**When to use:**

- Слабое действие внутри предложения, например «Забыли пароль?». Основную кнопку отправки не оформляйте как ссылку.

**Examples:**

```css
button.link {
  background: none;
  border: 0;
  padding: 0;
  font: inherit;
  color: #1e90ff;
  text-decoration: underline;
  cursor: pointer;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>Как you create custom checkboxes and radio buttons with CSS?</summary>

### Как you create custom checkboxes and radio buttons with CSS?

**Definition:**

Задайте `appearance: none`, чтобы контрол потерял нативную отрисовку, и нарисуйте квадрат обычным CSS. `:checked` оформляет включённое состояние. Поле остаётся настоящим checkbox или radio.

**How it works:**

#### appearance: none

- `none` даёт виджету примитивный вид, который можно нарисовать CSS. Поведение контрола не меняется.
- Размер задают `width` и `height`, затем `border` и `background`. Для выбранного состояния используйте `:checked`.
- `none` может полностью спрятать виджет, если не задать размер и фон. Контрол по-прежнему переключается через подпись.
- Оставьте видимый контур `:focus-visible`. Атрибут `for` у label должен совпадать с `id` поля.

**When to use:**

- Когда нативный контрол не подходит к дизайну. Для простой смены цвета `accent-color` тонирует нативный контрол и не заставляет собирать его заново.

**Examples:**

```css
input[type="checkbox"] {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 2px solid currentColor;
  background: transparent;
}

input[type="checkbox"]:checked {
  background: currentColor;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>Опишите how to style a dropdown select box without JavaScript.</summary>

### Опишите how to style a dropdown select box without JavaScript.

**Definition:**

Закрытый `<select>` можно оформить через `font`, `color`, `padding`, `border` и `background`. `appearance: none` убирает нативный вид этого закрытого контрола. Выпадающий список остаётся нативным, пока вы не включите `appearance: base-select`.

**How it works:**

#### К чему достаёт CSS

- `appearance: none` на `<select>` позволяет нарисовать закрытый блок, в том числе свою стрелку через `background-image`.
- Список вариантов при `appearance: auto` всё ещё рисует платформа. CSS для `option` ограничен и неодинаков в браузерах.
- `appearance: base-select` на select и на `::picker(select)` позволяет оформить сам список. Поддержка ограничена. Список тогда становится блоком верхнего слоя, а не меню операционной системы.
- Не убирайте видимый контрол и его стиль фокуса. Select должен оставаться пригодным без скрипта.

**When to use:**

- Шрифт, цвет и граница закрытого контрола безопасны. `base-select` берите только после проверки поддержки и с пригодным закрытым контролом.

**Examples:**

```css
select.custom {
  appearance: none;
  font: inherit;
  padding: 0.5rem 2rem 0.5rem 0.75rem;
  border: 1px solid #333;
  background: url("chevron.svg") no-repeat right 0.6rem center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>Что такое the best practice for styling placeholder text of an input field?</summary>

### Что такое the best practice for styling placeholder text of an input field?

**Definition:**

Текст подсказки оформляют псевдоэлементом `::placeholder`. Это подсказка, а не подпись, и у неё должен быть достаточный контраст.

**How it works:**

#### ::placeholder

- `::placeholder` принимает ограниченный набор свойств — те, что применимы к `::first-line`, включая `color` и свойства шрифта.
- Подсказка исчезает, когда пользователь начинает ввод. Она не заменяет `<label>`.
- Слишком светлый серый не проходит по контрасту. Затемните его так, чтобы текст читался на фоне поля.
- Не прячьте в placeholder инструкцию, которая нужна и после ввода, например обязательный формат.

**When to use:**

- Короткий пример ожидаемого значения, например `name@example.com`, рядом с настоящей подписью или над полем.

**Examples:**

```css
input::placeholder {
  color: #595959;
  font: inherit;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::placeholder
</details>

<details>
<summary>Как you implement focus styles for accessibility?</summary>

### Как you implement focus styles for accessibility?

**Definition:**

Показывайте видимый индикатор фокуса, лучше через `:focus-visible`, чтобы пользователь клавиатуры видел активный контрол. Не убирайте `outline`, если не заменяете его.

**How it works:**

#### :focus-visible

- `:focus-visible` совпадает, когда браузер считает кольцо фокуса нужным, обычно при фокусе с клавиатуры, а не при щелчке мыши.
- `outline` рисуется снаружи границы и не меняет раскладку. `outline-offset` отодвигает кольцо от контрола.
- Запасной вариант для старых браузеров — `@supports not selector(:focus-visible)` вместе с `:focus`.
- Индикатору нужен контраст с фоном страницы. Один `:focus` уместен, когда кольцо должно быть при любом способе фокуса, включая мышь.

**When to use:**

- Каждая ссылка, кнопка и поле. Свой checkbox через `appearance: none` всё равно нуждается в этом кольце.

**Examples:**

```css
button:focus-visible {
  outline: 3px solid #0b5fff;
  outline-offset: 3px;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:focus-visible
</details>

### Layout Debugging and Troubleshooting

<details>
<summary>Как you diagnose and solve layout issues?</summary>

### Как you diagnose and solve layout issues?

**Definition:**

Посмотрите элемент в инструментах разработчика: блочную модель, вычисленный `display` и зачёркнутые правила. Затем меняйте одно свойство и снова проверяйте блок.

**How it works:**

#### Что смотреть

- Панель блочной модели показывает content, padding, border и margin, в том числе схлопнувшиеся margin.
- Оверлеи grid и flex показывают дорожки, промежутки и свободное место. Flex-элемент, который не сжимается, часто имеет начальный `min-width: auto` — это минимальный размер содержимого.
- Зачёркнутые декларации проиграли каскад. Панель Computed показывает победившее значение.
- `outline: 1px solid red` на подозрительном элементе показывает его border box и не меняет раскладку так, как может `border`.

**When to use:**

- До переписывания раскладки. Назовите сломанный блок, свойство, которого вы ждали, и вычисленное значение, которое получили.

**Examples:**

```css
.suspect {
  outline: 1px solid red;
  min-width: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
</details>

<details>
<summary>Можете ли вы explain how to fix a layout that has unexpectedly broken columns?</summary>

### Можете ли вы explain how to fix a layout that has unexpectedly broken columns?

**Definition:**

Колонка ломается, когда минимальный размер элемента больше его дорожки или когда контейнер переносится либо переполняется. Найдите этот минимум, затем разрешите сжатие или перенос намеренно.

**How it works:**

#### Обычные причины

- Минимальный размер flex-элемента по умолчанию — `min-width: auto` (минимум содержимого). Длинный текст или картинка тогда не сжимаются и раздвигают ряд. `min-width: 0` на элементе разрешает сжатие.
- Начальное значение `flex-wrap` — `nowrap`, поэтому лишние элементы вылезают, а не образуют следующий ряд.
- В Grid дорожки `1fr` всё равно учитывают минимум содержимого. `minmax(0, 1fr)` позволяет колонке стать уже этого минимума.
- Картинка без `max-width: 100%` может быть шире колонки. `box-sizing: content-box` плюс padding может вытолкнуть процентную ширину за родителя.

**When to use:**

- После того как оверлей инструментов показал, какой элемент шире своей дорожки. Меняйте минимум или перенос, а не все ширины на странице.

**Examples:**

```css
.col {
  min-width: 0;
}

.grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
</details>

<details>
<summary>Опишите approaches to resolving common browser compatibility issues in CSS.</summary>

### Опишите approaches to resolving common browser compatibility issues in CSS.

**Definition:**

Проверьте возможность по таблице совместимости MDN, затем ограничьте её через `@supports` или дайте более простую раскладку, которую старые браузеры уже понимают.

**How it works:**

#### Постепенное усиление

- `@supports (display: grid)` применяет Grid только там, где он есть. Правила до запроса — запасной вариант.
- Предпочитайте стандартное свойство префиксу. Префикс добавляйте только если таблица совместимости говорит, что текущий браузер всё ещё его требует.
- Логические свойства, `aspect-ratio` и `appearance: base-select` — примеры возможностей с неодинаковой поддержкой. Читайте таблицу, прежде чем на них опираться.
- Сброс или `box-sizing: border-box` убирает часть различий блочной модели. Отсутствующие возможности он не добавляет.

**When to use:**

- Когда раскладка должна работать в браузерах, которые вы поддерживаете. Запасной вариант убирайте только после того, как эти браузеры реализовали возможность.

**Examples:**

```css
.layout {
  display: block;
}

@supports (display: grid) {
  .layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@supports
</details>

<details>
<summary>Как бы вы determine which styles are applied to an element when multiple styles collide?</summary>

### Как бы вы determine which styles are applied to an element when multiple styles collide?

**Definition:**

Каскад выбирает одну декларацию на свойство. Для деклараций одного происхождения и одного слоя побеждает селектор с большей специфичностью. При равной специфичности побеждает тот, что стоит позже.

**How it works:**

#### Порядок каскада

- Сначала происхождение и важность. Авторский `!important` побеждает обычное авторское правило. Происхождения user-agent, user и author разделены.
- Специфичность селектора — три столбца: ID (`1-0-0`), затем класс, атрибут и псевдокласс (`0-1-0`), затем тип и псевдоэлемент (`0-0-1`). Атрибут `style` побеждает любой из этих селекторов.
- `*`, комбинаторы и `:where()` специфичность не добавляют. `:not()`, `:is()` и `:has()` берут специфичность своего самого специфичного аргумента.
- Слои каскада (`@layer`) решают, какая группа правил соревнуется, ещё до сравнения специфичности. Инструменты разработчика зачёркивают проигравшие декларации.

**When to use:**

- Когда два правила задают одно свойство. Прочитайте победившее правило в панели Styles, а не добавляйте ещё один `!important`.

**Examples:**

```css
#title {
  color: green; /* 1-0-0 */
}

.page .card .title {
  color: yellow; /* 0-3-0, loses to the ID */
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
</details>

<details>
<summary>Какие типичные CSS problems могут вызвать performance issues?</summary>

### Какие типичные CSS problems могут вызвать performance issues?

**Definition:**

Дорогая работа — раскладка и отрисовка большого дерева, а не длинный селектор. Обычные причины: анимация геометрии, большие тени и `will-change`, оставленный на многих элементах.

**How it works:**

#### Что браузер повторяет

- Смена `width`, `height`, `margin`, `top` или размера шрифта может вызвать раскладку (reflow) и затем отрисовку. Анимация `transform` и `opacity` может остаться на композиторе.
- Большой `box-shadow`, `filter` или размытый фон на прокручиваемой области перерисовывает большой участок.
- `will-change: transform, opacity` — подсказка подготовить слой. Если оставить его на каждом элементе, тратится память. Ставьте его перед изменением и снимайте после.
- `@import` в таблице стилей задерживает следующий CSS-файл. Таблицы в `<head>`, блокирующие отрисовку, задерживают первую отрисовку.

**When to use:**

- Сначала снимите профиль медленной прокрутки или анимации в инструментах производительности браузера. Исправляйте свойство, которое видно в layout или paint.

**Examples:**

```css
.card {
  transition: transform 200ms ease-out, opacity 200ms ease-out;
}

.card:hover {
  transform: translateY(-2px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

### CSS 3D Transforms and Visual Effects

<details>
<summary>Как you utilize 3D transforms in CSS?</summary>

### Как you utilize 3D transforms in CSS?

**Definition:**

Трёхмерные трансформации двигают элемент в трёх измерениях функциями `rotateX()`, `rotateY()`, `rotateZ()`, `translateZ()` и `perspective()`. Родителю нужны `perspective` и, чтобы дети делили одно 3D-пространство, `transform-style: preserve-3d`.

**How it works:**

#### Глубина

- `rotateY(180deg)` поворачивает элемент вокруг вертикальной оси. `translateZ()` двигает его к зрителю или от зрителя.
- `perspective` на родителе — длина: расстояние от пользователя до плоскости z=0. Меньшая длина усиливает перспективу. `none` перспективу не применяет.
- `transform-style: preserve-3d` оставляет детей в одном 3D-пространстве. Начальное `flat` сплющивает каждого ребёнка до трансформации родителя.
- `transform` со значением не `none` создаёт контекст наложения и может стать содержащим блоком для потомка с `position: fixed`.

**When to use:**

- Карточки, кубы и другие эффекты глубины. Для плоского сдвига или масштаба достаточно двумерных `translate()` или `scale()`.

**Examples:**

```css
.scene {
  perspective: 600px;
}

.card {
  transform-style: preserve-3d;
  transform: rotateY(30deg);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transform
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>Объясните how to use the perspective property in CSS.</summary>

### Объясните how to use the perspective property in CSS.

**Definition:**

`perspective` задаёт расстояние между пользователем и плоскостью z=0, чтобы трансформированные дети выглядели объёмными. Оно действует на детей, а не на сам элемент. Начальное значение — `none`.

**How it works:**

#### Расстояние

- Длина вроде `800px` и есть это расстояние. Отрицательные значения недопустимы. Значение меньше `1px` поднимается до `1px`.
- Большая длина даёт слабый эффект. Малая длина даёт сильный. Дети с положительным z становятся крупнее, с отрицательным — мельче.
- Точка схода сначала в центре. `perspective-origin` её сдвигает.
- Значение не `none` создаёт контекст наложения и содержащий блок для потомков с `position: fixed`.
- Функция `perspective()` в `transform` — другое: она входит в собственный список трансформаций элемента.

**When to use:**

- Ставьте `perspective` на родителя элемента, который вы поворачиваете или сдвигаете по z. Без этого `translateZ()` и `rotateY()` не дают видимой глубины.

**Examples:**

```css
.scene {
  perspective: 800px;
  perspective-origin: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>Что такое backface visibility?</summary>

### Что такое backface visibility?

**Definition:**

`backface-visibility` задаёт, видна ли обратная сторона элемента, когда 3D-трансформация поворачивает её к пользователю. Начальное значение — `visible`. `hidden` прячет обратную сторону.

**How it works:**

#### Лицевая и обратная сторона

- Обратная сторона — зеркало лицевой. У двумерной трансформации обратной стороны нет, поэтому свойство там ничего не делает.
- `hidden` делает элемент невидимым, пока он отвёрнут. Так прячется оборот карточки при `rotateY(180deg)`.
- Свойство применяется к трансформируемым элементам и не наследуется. Ребёнок может задать своё значение.
- Само 3D-сцену оно не создаёт. Родителю всё ещё нужны `perspective` и `transform-style: preserve-3d`.

**When to use:**

- Любая грань, которая должна исчезнуть после поворота за 90 градусов, например лицевая сторона переворачиваемой карточки.

**Examples:**

```css
.face {
  backface-visibility: hidden;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backface-visibility
</details>

<details>
<summary>Как you create a flip card effect using CSS?</summary>

### Как you create a flip card effect using CSS?

**Definition:**

Поставьте `perspective` на сцену, держите карточку в 3D через `transform-style: preserve-3d` и при наведении задайте `transform: rotateY(180deg)`. Спрячьте оборот каждой стороны через `backface-visibility: hidden`, а обратную сторону заранее поверните на 180 градусов.

**How it works:**

#### Две стороны

- Обе стороны складывают через `position: absolute`, чтобы они занимали один блок.
- Лицевая сторона без поворота. Обратная имеет `transform: rotateY(180deg)`, поэтому её лицо смотрит в другую сторону.
- `transition: transform` на карточке анимирует переворот.
- Без `preserve-3d` браузер сплющивает стороны, и оборот так и не оказывается на виду.

**When to use:**

- Карточка с коротким оборотом, например с деталью, которая всё ещё текст. Не прячьте обязательное действие только на обороте, если до него нельзя добраться без наведения.

**Examples:**

```css
.scene {
  perspective: 600px;
}

.card {
  position: relative;
  transform-style: preserve-3d;
  transition: transform 600ms;
}

.card:hover {
  transform: rotateY(180deg);
}

.face {
  position: absolute;
  inset: 0;
  backface-visibility: hidden;
}

.back {
  transform: rotateY(180deg);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backface-visibility
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>Что такое some examples of visual effects you can achieve with CSS filters?</summary>

### Что такое some examples of visual effects you can achieve with CSS filters?

**Definition:**

`filter` применяет к элементу одну или несколько функций фильтра, включая границу и потомков. `grayscale()` убирает цвет, `blur()` смягчает изображение.

**How it works:**

#### Функции

- Функции пишут через пробел и применяют по порядку: `filter: grayscale(0.4) blur(5px)`.
- `grayscale(0)` оставляет элемент как есть. `grayscale(1)` или `grayscale(100%)` полностью серый.
- `blur()` принимает длину. Больший радиус размазывает сильнее.
- `filter` со значением не `none` создаёт контекст наложения. `backdrop-filter` — другое свойство: оно фильтрует то, что находится за элементом.

**When to use:**

- Приглушённое или смягчённое изображение либо состояние наведения. Для постоянного крупного эффекта лучше готовый файл: фильтр перерисовывается вместе с элементом.

**Examples:**

```css
img.muted {
  filter: grayscale(0.4) blur(5px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/filter
</details>

### Layout and Design Principles

<details>
<summary>Что такое the principles of responsive web design?</summary>

### Что такое the principles of responsive web design?

**Definition:**

Адаптивный дизайн подстраивает один документ под viewport: гибкая раскладка, гибкие медиа и медиазапросы, плюс meta viewport на ширину устройства.

**How it works:**

#### Три части

- Гибкая раскладка использует `fr`, flex и проценты вместо одной фиксированной ширины страницы.
- Гибкие медиа используют `max-width: 100%`, чтобы изображение или видео не вышло из колонки. `height: auto` сохраняет пропорцию, когда ограничена только ширина.
- Медиазапросы меняют колонки и отступы, когда viewport переходит точку. Запросы mobile-first используют `min-width`.
- `<meta name="viewport" content="width=device-width">` заставляет эти запросы видеть ширину устройства, а не широкий виртуальный viewport.

**When to use:**

- Любая страница, которую читают на разных размерах экрана. Начинайте с узкой раскладки и добавляйте дорожки по мере роста viewport.

**Examples:**

```css
img {
  max-width: 100%;
  height: auto;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>Как бы вы approach designing a grid system with CSS?</summary>

### Как бы вы approach designing a grid system with CSS?

**Definition:**

Задайте дорожки колонок на grid-контейнере, промежутки между ними и позвольте компонентам занимать несколько дорожек. В CSS Grid это `grid-template-columns`, `gap` и `grid-column`.

**How it works:**

#### Дорожки

- `repeat(12, minmax(0, 1fr))` — система из 12 колонок. Элемент занимает несколько через `grid-column: span 4`.
- `repeat(auto-fit, minmax(16rem, 1fr))` строит столько колонок, сколько помещается, поэтому для их числа точка перелома может не понадобиться.
- `gap` — это желоб. Отступы на самих элементах для него не нужны.
- Именованные области (`header`, `main`, `aside`) яснее описывают каркас страницы, чем набор span.

**When to use:**

- Общая сетка страницы. Flexbox внутри области сетки — для одного ряда действий. Не вкладывайте вторую 12-колоночную сетку, если у области на самом деле нет колонок.

**Examples:**

```css
.page {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 1rem;
}

.main {
  grid-column: span 8;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout
</details>

<details>
<summary>Опишите the principle of "mobile-first" design.</summary>

### Опишите the principle of "mobile-first" design.

**Definition:**

Mobile-first значит, что CSS по умолчанию — узкая раскладка. Более широкие раскладки добавляют в медиазапросах `min-width`, и каждая точка усиливает предыдущую.

**How it works:**

#### Снизу вверх

- Базовые правила предполагают маленький viewport: одна колонка, навигация столбиком и текст в `rem`.
- `@media (min-width: …)` или `@media (width >= …)` добавляет колонки и горизонтальную навигацию только когда есть место.
- В meta viewport должен быть `width=device-width`, иначе телефон всё равно верстает страницу в широком виртуальном viewport.
- Обратный путь, desktop-first, начинает с широкой раскладки и переопределяет её через `max-width`. Список переопределений растёт, чем больше маленьких экранов вы поддерживаете.

**When to use:**

- Новые страницы. Сначала содержимое и самая узкая раскладка, затем распределяется лишнее место.

**Examples:**

```css
.nav ul {
  display: grid;
  gap: 0.5rem;
}

@media (min-width: 40rem) {
  .nav ul {
    display: flex;
    gap: 1rem;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>Что такое some techniques for maintaining aspect ratios in responsive designs?</summary>

### Что такое some techniques for maintaining aspect ratios in responsive designs?

**Definition:**

Задайте `aspect-ratio` и оставьте хотя бы один из `width` или `height` автоматическим. Браузер тогда вычислит другую сторону и сохранит пропорцию, когда контейнер меняется.

**How it works:**

#### Пропорция

- `aspect-ratio: 16 / 9` — ширина, делённая на высоту. `aspect-ratio: 1` то же, что `1 / 1`.
- Если и ширина, и высота — определённые длины, пропорция не меняет используемый размер.
- Для заменяемого элемента, например изображения, `aspect-ratio: auto 3 / 2` использует заданную пропорцию, пока ресурс не загрузился, затем собственную пропорцию.
- `max-width: 100%` вместе с `height: auto` позволяет картинке сжаться вместе с колонкой и сохранить свою пропорцию. Это отдельно от `aspect-ratio` на блоке, размер которого задаёте вы.

**When to use:**

- Кадры видео, постеры и карточки, которые должны оставаться 16:9 или квадратом, пока ширина колонки меняется.

**Examples:**

```css
.frame {
  width: 100%;
  height: auto;
  aspect-ratio: 16 / 9;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/aspect-ratio
</details>

<details>
<summary>Как you ensure your design is accessible and inclusive?</summary>

### Как you ensure your design is accessible and inclusive?

**Definition:**

Оставьте текст масштабируемым, контраст читаемым, а фокус видимым. Не используйте цвет как единственный способ различить состояния и дайте людям уменьшить движение.

**How it works:**

#### Что контролирует CSS

- Задавайте текст в `rem` или другой единице от шрифта, чтобы он следовал корневому размеру шрифта пользователя. Не фиксируйте текст в `px`.
- `:focus-visible` с контрастным `outline` показывает фокус клавиатуры. Не ставьте `outline: none` без замены.
- `@media (prefers-reduced-motion: reduce)` сокращает или убирает анимацию и плавную прокрутку.
- Состоянию вроде ошибки нужно больше, чем цвет: текст, значок или граница.
- `content` у `::before`, который повторяет настоящий текст, объявляется дважды. У декоративного сгенерированного содержимого строка должна быть пустой, чтобы его не читали как текст.

**When to use:**

- На каждом интерактивном контроле и на любом движении. Проверьте страницу только с клавиатуры и с увеличенным размером шрифта по умолчанию.

**Examples:**

```css
button:focus-visible {
  outline: 3px solid #0b5fff;
  outline-offset: 3px;
}

@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms;
    transition-duration: 0.01ms;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:focus-visible
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion
</details>

### Print Stylesheets and CSS for Print

<details>
<summary>Как you create a print stylesheet for your web page?</summary>

### Как you create a print stylesheet for your web page?

**Definition:**

Правила печати кладут в `@media print` или подключают отдельный файл с `media="print"`. Они действуют, когда документ печатают или показывают в предпросмотре печати.

**How it works:**

#### Где живут правила

- `@media print { ... }` в основной таблице переопределяет экранные правила только для печати. Правила `screen` всё равно применяются первыми, затем подходящие правила печати.
- `<link rel="stylesheet" href="print.css" media="print">` загружает файл, который браузер использует для печати.
- Внутри запроса прячут навигацию, раскрывают ссылки через `attr(href)` и задают удобную ширину текста. Длины `cm`, `mm` и `in` на принтере совпадают с физическими единицами.

**When to use:**

- Страницы, которые печатают: статьи, чеки, билеты. Экранную таблицу оставляйте источником стилей содержимого и переопределяйте только то, что нужно печати.

**Examples:**

```css
@media print {
  nav {
    display: none;
  }

  a[href]::after {
    content: " (" attr(href) ")";
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
</details>

<details>
<summary>Что такое the common best practices when creating CSS for print?</summary>

### Что такое the common best practices when creating CSS for print?

**Definition:**

CSS для печати должен показать содержимое, спрятать элементы, которые на бумаге ничего не делают, и не отрывать рисунок или заголовок от следующего текста.

**How it works:**

#### Бумага

- `display: none` для навигации, рекламы и кнопок, которые работают только на экране.
- Тёмный текст на белом фоне. Браузер может убрать фоновые изображения и цвета фона, чтобы экономить краску.
- `a[href]::after { content: " (" attr(href) ")"; }` печатает URL, потому что подчёркивание на бумаге не нажимается.
- `break-inside: avoid` у рисунка держит изображение вместе с подписью. `break-after: avoid` у заголовка держит его со следующим абзацем.
- Элемент с `position: fixed` ставится в то же место на каждой печатной странице, поэтому фиксированная шапка может закрывать содержимое на каждой странице.

**When to use:**

- Любой блок `@media print`. Проверяйте статью на несколько страниц, а не только страницу в один экран.

**Examples:**

```css
@media print {
  figure {
    break-inside: avoid;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-inside
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Объясните the use of page-break in CSS.</summary>

### Объясните the use of page-break in CSS.

**Definition:**

`page-break-before`, `page-break-after` и `page-break-inside` — устаревшие псевдонимы `break-before`, `break-after` и `break-inside`. Они управляют разрывом до блока, после него или внутри него, когда документ дробится на страницы или колонки.

**How it works:**

#### Псевдоним

- `page-break-before: always` обрабатывается как `break-before: page`. Старое `always` означало разрыв страницы, а не колонки.
- `break-before: page` принудительно разрывает страницу перед блоком. `avoid` и `avoid-page` стараются там не разрывать. `auto` разрешает разрыв, но не требует его.
- Возможный разрыв смотрит на три значения: `break-after` предыдущего блока, `break-before` следующего и `break-inside` контейнера. Принудительный разрыв побеждает. Значение avoid запрещает такой разрыв.
- `break-before: column` принудительно разрывает колонку внутри многоколоночного контейнера. Те же свойства действуют и для страниц, и для колонок.

**When to use:**

- В новом CSS предпочтительны `break-before` и `break-inside`. `page-break-*` оставляйте только чтобы совпасть со старой таблицей стилей.

**Examples:**

```css
h2 {
  break-before: page;
}

figure {
  break-inside: avoid;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-before
</details>

<details>
<summary>Как you test the print version of your web page?</summary>

### Как you test the print version of your web page?

**Definition:**

Откройте предпросмотр печати в браузере и включите эмуляцию типа носителя `print` в инструментах разработчика, чтобы `@media print` действовал, пока вы смотрите на страницу.

**How it works:**

#### Два вида

- Предпросмотр печати разбивает документ на страницы и показывает поля, колонтитулы и разрывы. `window.print()` открывает этот предпросмотр.
- Панель отрисовки или эмуляции может поставить тип носителя CSS в `print`. Раскладка обновляется на странице, и правило проще проверить, чем в диалоге предпросмотра.
- Смотрите больше одной страницы. Заголовок у нижнего края, разрезанное изображение и фиксированная панель, которая повторяется, легко пропустить на первой странице.
- Выключите фоновую графику в диалоге печати, если нужно увидеть, что обычная печать делает с `background`.

**When to use:**

- После любого изменения `@media print`, `break-before` или `break-inside`.

**Examples:**

```css
@media print {
  body {
    margin: 1.5cm;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-before
</details>

<details>
<summary>Что такое some challenges when designing CSS for print?</summary>

### Что такое some challenges when designing CSS for print?

**Definition:**

Печать дробит страницу, отбрасывает большую часть экранного оформления и не имеет наведения. Фиксированные панели, цвета фона и единицы viewport ведут себя иначе, чем на экране.

**How it works:**

#### Что меняется

- Браузер может опустить `background-color` и `background-image`, если пользователь не печатает фоны.
- `position: fixed` повторяется на каждой странице. Липкая шапка не прилипает так, как при прокрутке.
- Единицы viewport недопустимы внутри `@page`. Размер страницы и поля относятся к страничному блоку, а не к экранному viewport.
- Ссылки, состояния наведения и свёрнутая навигация на бумаге не работают. URL приходится печатать текстом.
- Разрыв может разрезать абзац, строку таблицы или изображение. `break-inside: avoid` это уменьшает, но блок выше одной страницы всё равно придётся разорвать.

**When to use:**

- Задумывайте печать как отдельную раскладку. Не рассчитывайте, что экранный дизайн сам аккуратно разобьётся на страницы.

**Examples:**

```css
@page {
  margin: 1.5cm;
}

@media print {
  header {
    position: static;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@page
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

### Pseudo-Elements and Pseudo-Classes

<details>
<summary>Опишите the puRPOse of the ::before and ::after pseudo-elements.</summary>

### Опишите the puRPOse of the ::before and ::after pseudo-elements.

**Definition:**

`::before` и `::after` порождают блок как первого или последнего ребёнка элемента. Они рисуются, только если `content` задан значением, отличным от `normal` и `none`.

**How it works:**

#### Порождённые блоки

- `content` может быть строкой, `attr()`, счётчиком или пустой строкой `""`, когда нужен только блок для оформления.
- Псевдоэлемент можно позиционировать, задавать ему размер и фон. В HTML его нет, поэтому в DOM это не отдельный элемент.
- Строку в `content` может прочитать вспомогательная технология. Для чисто визуальной метки используйте `content: ""`.
- Их специфичность — как у псевдоэлемента: один столбец типа (`0-0-1`) плюс остальной селектор.

**When to use:**

- Значки, подчёркивания, кавычки и URL при печати. Не помещайте текст, который пользователь обязан прочитать, или контрол только в `content`.

**Examples:**

```css
.required::after {
  content: " *";
}

a[href]::after {
  content: " (" attr(href) ")";
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::before
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::after
</details>

<details>
<summary>Что такое the :hover pseudo-class and how does it work?</summary>

### Что такое the :hover pseudo-class and how does it work?

**Definition:**

`:hover` совпадает с элементом, пока пользователь указывает на него указывающим устройством. Он также совпадает с предками этого элемента.

**How it works:**

#### Указатель

- Стили под `:hover` действуют, пока указатель над элементом. Увод указателя их снимает.
- Переход на том же свойстве анимирует вход в это состояние и выход из него.
- На сенсорном экране наведение может залипнуть после касания или не наступить. Не прячьте обязательное действие только за наведением.
- `:hover` — псевдокласс, поэтому он добавляет к специфичности `0-1-0`. Это не псевдоэлемент.

**When to use:**

- Подсказка: подчёркивание, цвет или лёгкий transform. Сочетайте с `:focus-visible`, чтобы пользователь клавиатуры получил такой же сигнал.

**Examples:**

```css
a:hover {
  text-decoration: underline;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:hover
</details>

<details>
<summary>Объясните the difference between :nth-child and :nth-of-type.</summary>

### Объясните the difference between :nth-child and :nth-of-type.

**Definition:**

`:nth-child()` считает каждого соседа-элемента. `:nth-of-type()` считает только соседей того же типа элемента. Совпадение всё равно должно подойти остальному селектору.

**How it works:**

#### Счёт

- Позиции начинаются с 1. `odd` и `even` — нечётные и чётные позиции. `An+B` — шаг и сдвиг, поэтому `2n+1` — тот же набор, что `odd`.
- `span:nth-child(2n+1)` считает каждого ребёнка. Если третий ребёнок — `em`, его считают, но не выбирают, и span сдвигаются.
- `span:nth-of-type(2n+1)` пропускает `em` и считает только элементы `span`.
- `:nth-child(1)` совпадает с тем же элементом, что `:first-child`. Синтаксис `of`, `:nth-child(even of .noted)`, считает только соседей, которые подходят этому селектору.

**When to use:**

- `:nth-child`, когда важна позиция среди всех детей. `:nth-of-type`, когда в разметке смешаны теги и нужен n-й элемент одного тега.

**Examples:**

```css
span:nth-child(2n + 1) {
  background: tomato;
}

span:nth-of-type(2n + 1) {
  background: tomato;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:nth-child
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:nth-of-type
</details>

<details>
<summary>Как :first-child and :last-child work?</summary>

### Как :first-child and :last-child work?

**Definition:**

`:first-child` совпадает с элементом, который является первым ребёнком своего родителя. `:last-child` совпадает с элементом, который является последним ребёнком. Оба считают каждого соседа-элемента, а не только тот же тег.

**How it works:**

#### Позиция у родителя

- `p:first-child` совпадает с `p`, только если этот `p` — первый дочерний элемент родителя. Если сначала идёт заголовок, абзац не совпадёт.
- `p:last-child` совпадает с `p`, только если внутри родителя после него ничего нет.
- `:first-child` — то же совпадение, что `:nth-child(1)`. `:last-child` — то же совпадение, что `:nth-last-child(1)`.
- `:first-of-type` и `:last-of-type` — версии с ограничением по типу. Они игнорируют соседей с другими тегами.

**When to use:**

- Отступ у каждого пункта, кроме первого, или граница у каждого пункта, кроме последнего. Если обёртка добавляет неожиданный первый узел, псевдокласс перестаёт совпадать.

**Examples:**

```css
li:first-child {
  margin-top: 0;
}

li:last-child {
  border-bottom: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:first-child
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:last-child
</details>

<details>
<summary>Что такое the :not pseudo-class and when would you use it?</summary>

### Что такое the :not pseudo-class and when would you use it?

**Definition:**

`:not()` совпадает с элементами, которые не подходят его списку селекторов. Сам псевдокласс специфичность не добавляет. Её даёт самый специфичный селектор в аргументе.

**How it works:**

#### Отрицание

- `p:not(.fancy)` совпадает с каждым `p` без этого класса.
- `:not(.foo, .bar)` совпадает с элементами, которые не `.foo` и не `.bar`. Это то же, что `:not(.foo):not(.bar)`.
- Неверный селектор внутри `:not()` делает недействительным всё правило. Список внутри `:is()` прощает ошибки: `:not(:is(.foo, :unknown))` всё ещё совпадает с элементами, которые не `.foo`.
- `:not(.foo)` совпадает с каждым элементом, который не `.foo`, включая `html` и `body`. С комбинатором потомка ссылка внутри таблицы всё ещё может подойти под `body :not(table) a` через другого предка. Пишите `a:not(table a)`, если имеете в виду ссылки не внутри таблицы.

**When to use:**

- Небольшое исключение, например каждая кнопка, кроме основной. Не используйте его, чтобы перечислить длинный список всего, чего вы не хотите.

**Examples:**

```css
p:not(.fancy) {
  color: green;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:not
</details>

### CSS Performance Optimization

<details>
<summary>Как the complexity of selectors impact performance?</summary>

### Как the complexity of selectors impact performance?

**Definition:**

Длинный селектор редко делает страницу медленной. Сопоставление стилей дешевле раскладки и отрисовки. Сложность важна, когда селектор совпадает с огромным числом элементов или когда он настолько широк, что обновление инвалидирует больше дерева, чем вы ожидали.

**How it works:**

#### Сопоставление

- Браузер строит CSSOM и сопоставляет правила с элементами. Универсальный селектор или селектор, который зависит от общего предка, может совпасть с куда большим числом узлов, чем один класс.
- Специфичность решает, какая декларация победит. Это не оценка времени. ID в текущем браузере не ощутимо быстрее класса.
- Дороже последующая инвалидация: смена класса на родителе может заново посчитать стили каждого потомка, который зависит от этого предка.
- Глубокая вложенность препроцессора компилируется в длинные селекторы и повышает специфичность. Цена — сопровождение и широкая инвалидация, а не несколько лишних символов в селекторе.

**When to use:**

- Предпочитайте один класс на компонент. Профилируйте пересчёт стилей только после того, как раскладка и отрисовка уже дёшевы.

**Examples:**

```css
.card-title {
  font-weight: 700;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

<details>
<summary>Какие стратегии would you use to optimize the rendering path of CSS?</summary>

### Какие стратегии would you use to optimize the rendering path of CSS?

**Definition:**

Критический путь отрисовки превращает HTML в DOM, CSS в CSSOM, затем строит дерево отрисовки, раскладывает его, рисует и композитит. CSS, который блокирует этот путь, должен быть небольшим и оказаться в документе рано.

**How it works:**

#### Шаги

- Таблица стилей в `<head>` блокирует отрисовку: браузер ждёт её до первой отрисовки, чтобы не нарисовать неготовый вид и тут же пересчитать стили.
- `@import` внутри таблицы обнаруживает следующий файл только после разбора первого, поэтому задержка идёт последовательно. Вместо этого используйте `<link>`.
- Неиспользуемые правила всё равно попадают в CSSOM. Отдавайте CSS, который нужен первому экрану, а остальное загружайте позже.
- Когда CSSOM уже есть, доминируют раскладка и отрисовка. Анимация `transform` и `opacity` не повторяет раскладку.

**When to use:**

- Когда первая отрисовка медленная или цепочка таблиц стилей видна в сетевом водопаде. Измеряйте, прежде чем дробить файлы ради дробления.

**Examples:**

```html
<link rel="stylesheet" href="styles.css" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

<details>
<summary>Как you minimize CSS repaints and reflows?</summary>

### Как you minimize CSS repaints and reflows?

**Definition:**

Reflow (раскладка) заново считает геометрию. Repaint заливает пиксели после того, как геометрия известна. Композитинг может сдвинуть уже готовый слой. Анимируйте `transform` и `opacity`, когда хотите обойтись без раскладки.

**How it works:**

#### Что меняется

- `width`, `height`, `margin`, `padding`, `top`, `left` и `font-size` могут вызвать раскладку, а затем отрисовку.
- `color` и `background-color` могут перерисовать элемент без полной раскладки.
- `transform` и `opacity` композитор может обработать, когда элемент на своём слое.
- `will-change: transform, opacity` просит браузер подготовить этот слой. Добавляйте его прямо перед анимацией и возвращайте `auto` после. Если оставить его на многих элементах, тратится память.
- Чтение раскладки в цикле, например `offsetWidth` между записями стилей, вызывает синхронную раскладку. Сначала прочитайте, потом запишите.

**When to use:**

- Движение при наведении, карусели и липкие панели. В панели производительности проверьте, что работа — composite, а не layout.

**Examples:**

```css
.card {
  transition: transform 200ms ease-out;
}

.card:hover {
  transform: translateY(-2px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change
- https://developer.mozilla.org/en-US/docs/Glossary/Reflow
</details>

<details>
<summary>Что такое some of the best practices for writing efficient CSS?</summary>

### Что такое some of the best practices for writing efficient CSS?

**Definition:**

Пишите наименьший набор правил, который совпадает с нужными элементами. Предпочитайте класс, пользовательские свойства для повторяющихся значений и свойства, которые при анимации не вызывают раскладку.

**How it works:**

#### Привычки

- Один класс компонента лучше длинной цепочки тегов и предков. Низкая специфичность проще переопределяется без `!important`.
- Пользовательские свойства хранят цвет или отступ один раз. Один и тот же hex в сотнях правил потом приходится искать.
- Не анимируйте свойства раскладки ради украшения. Используйте `transform` и `opacity`.
- Отдавайте только CSS, который страница использует. Блокирующий отрисовку файл с неиспользуемыми правилами всё равно нужно скачать и разобрать.
- Stylelint может отклонить неизвестные свойства и нежелательные приёмы до выкладки.

**When to use:**

- На ревью спросите, совпадает ли новый селектор с большим числом элементов, чем компонент, и касается ли новая анимация раскладки.

**Examples:**

```css
:root {
  --space: 1rem;
}

.card {
  margin-block: var(--space);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
- https://stylelint.io/
</details>

<details>
<summary>Как you optimize CSS for critical rendering path and fast load times?</summary>

### Как you optimize CSS for critical rendering path and fast load times?

**Definition:**

Сделайте CSS первого экрана небольшим, загрузите его из `<head>` без `@import` и отложите остальное, чтобы оно не блокировало первую отрисовку.

**How it works:**

#### Первая отрисовка

- Браузер не может надёжно рисовать, пока блокирующий CSS не скачан и не превращён в CSSOM.
- Встройте небольшой набор правил для верха страницы, если круговой сетевой запрос доминирует. Держите этот блок коротким.
- Полную таблицу загружайте через `<link>`. Значение `media`, которое не совпадает, например `media="print"`, позволяет браузеру не блокировать экран этим файлом.
- Уберите неиспользуемые правила и не прячьте второй запрос в `@import`.
- После загрузки держите взаимодействие на `transform` и `opacity`, чтобы первое действие не повторяло раскладку.

**When to use:**

- Когда сетевой водопад показывает, что CSS задерживает первую отрисовку. Измерьте размер передачи таблицы и время до первой отрисовки до и после.

**Examples:**

```html
<link rel="stylesheet" href="styles.css" />
<link rel="stylesheet" href="print.css" media="print" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
</details>

### Scalable and Maintainable CSS

<details>
<summary>Что такое Object-Oriented CSS (OOCSS)?</summary>

### Что такое Object-Oriented CSS (OOCSS)?

**Definition:**

OOCSS отделяет повторно используемую структуру от оформления и контейнер от содержимого внутри него, чтобы один и тот же объект можно было оформить в нескольких местах.

**How it works:**

#### Два разделения

- Структура — объект раскладки: медиаблок с изображением и текстом. Оформление — визуальная тема: цвет, граница и тень. Класс оформления меняют, не копируя правила раскладки.
- Контейнер не должен оформлять содержимое длинным селектором потомка. У объекта содержимого свой класс, поэтому он выглядит одинаково в боковой колонке и в карточке.
- В HTML становится больше классов и меньше правил, которые зависят от места элемента в дереве.

**When to use:**

- Дизайн-система из повторяющихся объектов, например медиаблоков и кнопок. BEM — одна схема имён, которая несёт ту же идею.

**Examples:**

```css
.media {
  display: flex;
  gap: 1rem;
}

.media-skin {
  background: #f4f4f4;
  border: 1px solid #ccc;
}
```

**Sources:**

- https://github.com/stubbornella/oocss/wiki
</details>

<details>
<summary>Можете ли вы explain the BEM (block Element Modifier) naming convention?</summary>

### Можете ли вы explain the BEM (block Element Modifier) naming convention?

**Definition:**

BEM называет компонент блоком, его части элементами, а варианты модификаторами: `block__element--modifier`.

**How it works:**

#### Имена

- Блок — самостоятельный компонент, например `menu`. Элемент принадлежит этому блоку и пишется `menu__item`. Модификатор — признак, его пишут `menu__item--active` или `menu--wide`.
- Класс и есть селектор. `.menu .item` не нужен, поэтому специфичность остаётся плоской, а часть не зависит от вложенности.
- Элемент не используют вне своего блока. Общую часть делают отдельным блоком.
- Двойное подчёркивание и двойной дефис — обычные разделители. Методу важно, чтобы блок, элемент и модификатор читались, а не конкретный препроцессор.

**When to use:**

- Рукописный CSS компонентов в большой кодовой базе. Утилитарные фреймворки вроде Tailwind используют другую модель имён.

**Examples:**

```css
.menu {
  display: flex;
  gap: 0.5rem;
}

.menu__item--active {
  font-weight: 700;
}
```

**Sources:**

- https://github.com/getbem/getbem.github.io/blob/master/src/pages/naming.mdx
</details>

<details>
<summary>Что такое the advantages of using a CSS framework like Bootstrap or TailwindCSS?</summary>

### Что такое the advantages of using a CSS framework like Bootstrap or TailwindCSS?

**Definition:**

Фреймворк даёт общий задокументированный набор стилей, чтобы команда не выдумывала отступы, цвет и раскладку для каждого экрана. Bootstrap поставляет компоненты. Tailwind поставляет утилиты, которые складывают в разметке.

**How it works:**

#### Два вида фреймворка

- Таблица Bootstrap включает сетку, формы и компоненты со стабильными именами классов. Согласованный интерфейс получается быстро, а когда дизайн расходится, его переопределяют.
- Tailwind сканирует шаблоны на имена классов и генерирует только использованные утилиты: отступы, цвет и flex. Отдельной таблицы компонентов нет, пока вы её не добавите.
- Оба уменьшают разовый CSS и упрощают ревью, потому что приём назван. Оба мешают своему дизайну, если вы постоянно переопределяете фреймворк вместо того, чтобы им пользоваться.

**When to use:**

- Bootstrap, когда нужны готовые компоненты. Tailwind, когда хотите проектировать в разметке и оставить в CSS только написанные классы. Не берите ни тот ни другой, если у продукта уже есть дизайн-система.

**Examples:**

```html
<button class="btn btn-primary">Save</button>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
- https://tailwindcss.com/docs/styling-with-utility-classes
</details>

<details>
<summary>Как you ensure CSS Scalability and maintainability in a large project?</summary>

### Как you ensure CSS Scalability and maintainability in a large project?

**Definition:**

Держите селекторы плоскими, общие значения кладите в пользовательские свойства и упорядочивайте правила так, чтобы поздние слои переопределяли ранние намеренно. Линтуйте результат.

**How it works:**

#### Рычаги

- Класс на компонент или BEM не ломается, когда HTML переезжает.
- Пользовательские свойства — это токены. Тема меняет токен, а не каждое правило.
- `@layer` задаёт порядок групп, например reset, components и utilities, ещё до сравнения специфичности.
- CSS Modules или соглашение об именах не дают двум файлам заявить один и тот же `.button`.
- Stylelint и форматирование ловят неизвестные свойства и разъезд стиля. Удаляйте неиспользуемый CSS, а не оставляйте его «на всякий случай».

**When to use:**

- Как только таблицы стилей правит больше одного человека. Небольшой сайт может оставаться одним файлом, пока селекторы не начнут конфликтовать.

**Examples:**

```css
@layer reset, components, utilities;

@layer components {
  .button {
    color: var(--text);
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@layer
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
</details>

<details>
<summary>Что такое some CSS architecture approaches for large-scale applications?</summary>

### Что такое some CSS architecture approaches for large-scale applications?

**Definition:**

Большие приложения выбирают один способ ограничивать область CSS и упорядочивать его. Обычные варианты: BEM, CSS Modules, утилитарные классы и слои каскада. Их можно сочетать. Не стоит выдумывать в одном файле вторую схему имён.

**How it works:**

#### Подходы

- BEM ограничивает область именем: `block__element--modifier`. Таблица остаётся глобальной, а границу несёт имя класса.
- CSS Modules ограничивают область файлом. Сборка переписывает класс, а компонент импортирует сгенерированное имя.
- Утилитарный CSS, как в Tailwind, ставит классы одного назначения в разметку и генерирует таблицу из этих имён.
- `@layer` упорядочивает reset, components и utilities, чтобы утилита не побеждала за счёт специфичности.
- Разделение структуры и оформления в OOCSS — идея повторно используемых объектов во всех этих подходах.

**When to use:**

- BEM или CSS Modules для библиотеки компонентов со своими таблицами. Утилиты, когда интерфейс собирают в шаблонах. `@layer` добавляйте, когда несколько источников CSS должны предсказуемо переопределять друг друга.

**Examples:**

```css
@layer components {
  .menu__item--active {
    font-weight: 700;
  }
}
```

**Sources:**

- https://github.com/getbem/getbem.github.io/blob/master/src/pages/naming.mdx
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@layer
</details>

### CSS Special Cases and Practical Scenarios

<details>
<summary>Как бы вы create a CSS-only hamburger menu toggle?</summary>

### Как бы вы create a CSS-only hamburger menu toggle?

**Definition:**

Переключателем служит checkbox, а кнопкой — `<label>`. Состояние `:checked` показывает навигацию через комбинатор соседнего элемента. Скрипт не нужен, и контрол остаётся доступен с клавиатуры.

**How it works:**

#### Checkbox и сосед

- Поле и nav — соседи. `input:checked ~ nav` совпадает с nav, только пока флажок включён.
- `for` у label указывает на `id` поля, поэтому щелчок или активация подписи переключает флажок.
- Не ставьте checkbox в `display: none`. Визуально скрытый контрол всё ещё можно сфокусировать. Дайте видимый стиль `:focus-visible` на подписи, например через `:focus-within` на обёртке.
- Кнопке меню нужно доступное имя. Текст label и есть это имя.

**When to use:**

- Простое раскрытие на маленьком экране. Для модального диалога или меню, которое должно удерживать фокус, используйте `<dialog>` или контрол со скриптом, а не checkbox.

**Examples:**

```css
.nav-toggle:checked ~ nav {
  display: block;
}

nav {
  display: none;
}
```

```html
<input class="nav-toggle" id="nav-toggle" type="checkbox" />
<label for="nav-toggle">Menu</label>
<nav>…</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:checked
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/Subsequent-sibling_combinator
</details>

<details>
<summary>Объясните how to make a pure CSS carousel or SLIder.</summary>

### Объясните how to make a pure CSS carousel or SLIder.

**Definition:**

Слайды кладут в ряд внутри горизонтального прокручиваемого контейнера и привязывают позицию прокрутки к каждому слайду через `scroll-snap-type` и `scroll-snap-align`.

**How it works:**

#### Привязка

- Дорожка — `display: flex` с `overflow-x: auto`. Каждый слайд — `flex: 0 0 100%`, поэтому он шириной в один видимый участок дорожки.
- `scroll-snap-type: x mandatory` на дорожке заставляет позицию прокрутки встать на точку привязки, когда прокрутка заканчивается. `proximity` привязывает только если позиция уже близко.
- `scroll-snap-align: start` на каждом слайде отмечает эту точку.
- Физику прокрутки выбирает браузер. Кнопок «назад» и «вперёд» это не добавляет. Ими становятся отдельные элементы, например ссылки на `id` каждого слайда.

**When to use:**

- Ряд панелей, которые пользователь листает или прокручивает. Добавьте кнопки и остановку, если карусель двигается сама. Автоматическая анимация — плохой вариант по умолчанию.

**Examples:**

```css
.track {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
}

.slide {
  flex: 0 0 100%;
  scroll-snap-align: start;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/scroll-snap-type
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/scroll-snap-align
</details>

<details>
<summary>Как you create a masonry layout with CSS?</summary>

### Как you create a masonry layout with CSS?

**Definition:**

Раскладка в духе каменной кладки кладёт каждый элемент в колонку, где больше свободного места. В CSS экспериментальный вариант называется grid lanes: `display: grid-lanes` вместе с `grid-template-columns`. Поддержка ограничена.

**How it works:**

#### Grid lanes

- Одна ось — обычная сетка. Другая складывает элементы в дорожку, где больше места, поэтому строгие строковые дорожки не используются.
- `grid-template-columns: repeat(auto-fill, minmax(120px, 1fr))` задаёт колонки. `gap` разделяет элементы.
- Элемент может занять несколько дорожек через `grid-column-end: span 2`. Элементы с явной колонкой ставятся до алгоритма упаковки.
- Многоколоночная раскладка (`columns`) широко доступна, но заполняет каждую колонку сверху вниз. Это другой порядок, не grid lanes.

**When to use:**

- Только там, где вы проверили поддержку `grid-lanes`, или за `@supports (display: grid-lanes)`. Иначе используйте обычную сетку с равными рядами или `columns`, если порядок по колонкам допустим.

**Examples:**

```css
.grid {
  display: grid-lanes;
  gap: 10px;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Grid_lanes
</details>

<details>
<summary>Как бы вы apply CSS to a web application that should print as well as display on screen?</summary>

### Как бы вы apply CSS to a web application that should print as well as display on screen?

**Definition:**

Общие правила пишут один раз, а то, чем нельзя пользоваться на бумаге, переопределяют внутри `@media print`. Раскладка только для экрана остаётся в `@media screen` или в базовых правилах.

**How it works:**

#### Один документ, два носителя

- Правила вне медиазапроса действуют и на экране, и при печати. Цвет, шрифт и колонка статьи относятся сюда.
- `@media screen` держит эффекты наведения, липкие панели и хромированное обрамление приложения.
- `@media print` прячет навигацию, задаёт физические поля и не рвёт рисунки. Отдельный `<link media="print">` — то же разделение в другом файле.
- `position: fixed` повторяется на каждой печатной странице, поэтому в запросе печати верните такие панели к `position: static`.

**When to use:**

- Любое приложение со страницей, которую пользователь должен напечатать или сохранить как PDF. После смены раскладки смотрите оба типа носителя.

**Examples:**

```css
article {
  max-width: 65ch;
}

@media print {
  nav {
    display: none;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Как you create a seamless waterfall flow of elements in a responsive design?</summary>

### Как you create a seamless waterfall flow of elements in a responsive design?

**Definition:**

Для широко поддерживаемого потока по колонкам задайте `columns` идеальной шириной колонки. Браузер добавляет колонки по мере роста контейнера. `break-inside: avoid` держит каждый элемент целым.

**How it works:**

#### Поток по колонкам

- `columns: 16rem` задаёт идеальную ширину колонки. Число колонок следует за контейнером: узкий viewport получает одну, широкий — несколько.
- Содержимое течёт вниз по колонке, затем в следующую. Этот порядок не совпадает с grid lanes, которые кладут каждый элемент в самую короткую колонку слева направо.
- `break-inside: avoid` на каждом ребёнке не даёт карточке разорваться между колонками. Margin или `column-gap` их разделяют.
- `display: grid-lanes` упаковывает по самой короткой колонке, но это экспериментально и поддержка ограничена.

**When to use:**

- Адаптивный поток заметок или карточек, где порядок сверху вниз по колонке допустим. Grid lanes используйте только за `@supports`, когда нужна упаковка в самую короткую колонку.

**Examples:**

```css
.flow {
  columns: 16rem;
  column-gap: 1rem;
}

.flow > * {
  break-inside: avoid;
  margin-bottom: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Grid_lanes
</details>

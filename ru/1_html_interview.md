# HTML — Interview

Вопросы для собеседования по HTML. Ответы заполнены для опубликованной части, остальные пустые.

[← Вернуться в центр](./1_html.md)

### Related docs

- [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1_html_performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы из [Devinterview.io — HTML5](https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/).

### HTML Basics

<details>
<summary>Что означает HTML и для чего он используется?</summary>

### Что означает HTML и для чего он используется?

**Definition:**

**HTML**, или **H**yper **T**ext **M**arkup **L**anguage, — стандартный язык для создания веб-страниц и приложений. HTML5, последняя версия на 2022 год, вводит несколько новых элементов и атрибутов, поднимая пользовательский опыт и стандарты программных приложений.

HTML отвечает за структуру веб-контента, за доступность и за то, как веб-страницы показаны визуально. Он остаётся базовой конструкцией, на которой работает почти весь веб-контент.

**How it works:**

#### Основные функции

- **Структурирование контента**: Теги вроде `<header>`, `<footer>` и `<section>` делят контент и упрощают его организацию.
- **Встраивание медиа**: HTML даёт теги, чтобы включать мультимедиа: изображения, аудио и видео.
- **Работа с формами**: Интерактивные секции, такие как формы пользовательского ввода, задаются тегами input и label.
- **Гиперссылки**: Нужны для навигации. Гипертекстовые ссылки вроде `<a>` привязывают контент внутри страницы или за её пределами.
- **Возможности доступности**: Семантические теги вроде `<nav>` и `<article>` не только структурируют данные, но и улучшают доступность для пользователей, которые опираются на скринридеры.
- **Интеграция других технологий**: Может соединяться со скриптовыми языками вроде JavaScript и с библиотеками и фреймворками вроде Bootstrap, чтобы усилить визуальную подачу.

#### Совместимость и разработка

Главная цель HTML5 — улучшить поддержку в языке новейшего мультимедиа и при этом оставить его легко читаемым для людей. Его спроектировали обратно и вперёд совместимым, чтобы контент, написанный в предыдущих версиях, бесшовно встраивался и взаимодействовал с контентом, созданным в последующих версиях.

#### Визуальная подача и адаптация интерфейса

HTML5 сделан так, чтобы давать больше гибкости, контроля и визуальной зрелости веб-страницам и веб-приложениям. Веб-разработчики могут собирать современные веб-интерфейсы с насыщенным визуалом и мультимедиа. Он также позволяет делать более отзывчивый и адаптивный дизайн, чтобы просмотр был удобным на разных устройствах и размерах экрана. Это отражает более широкий сдвиг технологий к пользовательскому опыту, который не привязан к одному устройству.

#### Роль CSS и JavaScript

Пока HTML даёт статичный контент, **CSS** и **JavaScript** добавляют слои оформления, интерактивности и динамических обновлений контента. Интеграция этих трёх технологий (её называют **HTML-CSS-JS**) — трио, которое составляет основу почти всего веб-контента. Их часто представляют как **HTML5-CSS3-JS**, чтобы обозначить единые современные практики. В сети есть даже инструменты, которые соединяют эти технологии в один фреймворк или язык, например WebAssembly или Dart. Трио даёт более модульный подход: отдельные команды могут сосредоточиться на отдельных слоях, и разработка в больших проектах идёт ровнее. Владение их пересечением помогает спроектировать цельный пользовательский опыт. Эту идею передаёт аббревиатура «PEA»: Platform (HTML), Engine (JavaScript) и Appearance (CSS). Каждая категория отвечает за свою сторону пользовательского опыта.

#### Практическое применение

- **Разработка сайтов**: Все привычные веб-ресурсы, от простых блогов до больших интернет-магазинов, по-прежнему опираются в первую очередь на HTML.
- **Веб-приложения**: Веб-технологии сильно выросли, и сложные приложения вроде Google Docs, Trello и Slack целиком работают в браузере. HTML5 сыграл в этом ключевую роль.
- **Реклама и медиа**: Продвинутые средства работы с медиа в HTML5 сделали его стандартом для онлайн-рекламы и мультимедийного контента.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML
- https://html.spec.whatwg.org/
</details>

<details>
<summary>Опишите the basic structure of an HTML document.</summary>

### Опишите the basic structure of an HTML document.

**Definition:**

**HyperText Markup Language** (HTML) служит каркасом веб-контента и задаёт его структуру и семантику. Дальше — основные элементы HTML-документа.

**How it works:**

#### Базовая структура HTML-документа

HTML-документ состоит из двух главных секций: `head` и `body`.

##### Объявление типа документа (DOCTYPE)

**Document Type Declaration** (DOCTYPE) — это не HTML-тег. Это инструкция браузеру о том, в какой версии HTML написана страница.

```html
<!DOCTYPE html>
```

Это объявление показывает, что документ является документом HTML5.

##### Элемент HTML

Элемент `html` — корневой элемент HTML-страницы. Он охватывает всё содержимое, и head, и body.

```html
<html>
  <!-- Head and Body Sections Are Nested Inside -->
</html>
```

##### Секция head

Секция `head` даёт метаинформацию о документе. Сама она в браузере не показывается, но служит разным целям: от заголовка документа до подключения внешних ресурсов.

```html
<head>
  <!-- Title and Meta-Tags, Styles, Scripts, etc. -->
</head>
```

###### Элемент title

Элемент `title` задаёт заголовок документа, который показывается в заголовке окна браузера или на вкладке.

```html
<title>Your Page Title</title>
```

##### Секция body

Секция `body` вмещает видимое содержимое документа — то, что пользователь видит и с чем взаимодействует.

```html
<body>
  <!-- Content Visible to Users: Headings, Paragraphs, Images, etc. -->
</body>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html
- https://developer.mozilla.org/en-US/docs/Glossary/Doctype
</details>

<details>
<summary>Что делают DOCTYPE и атрибут html lang?</summary>

### Что делают DOCTYPE и атрибут html lang?

**Definition:**

**Document Type** (DOCTYPE) и атрибут `lang` играют ключевую роль на веб-страницах.

**How it works:**

#### DOCTYPE: тип документа и режим проверки

##### Назначение

- Указывает версию HTML или XHTML, которая используется в документе.
- Определяет метод и алгоритм разбора для браузера и тем самым влияет на согласованность.

##### Пример кода

Объявление `<!DOCTYPE>` ставится в самом верху HTML-файла, ещё до того, как начинается тег `<html>`.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <!-- Content -->
  </body>
</html>
```

#### Атрибут lang: указание языка

Атрибут `lang` в теге HTML задаёт основной язык документа. Его значение — первичный языковой подтег по RFC 5646 (BCP 47). Это может быть допустимый код языка, допустимый код языка с допустимым кодом региона или просто «und» для неуказанного языка.

##### Пример кода

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>Welcome</h1>
    <p>This is a demo page.</p>
  </body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang
</details>

<details>
<summary>Что такое the difference between head and body tags?</summary>

### Что такое the difference between head and body tags?

**Definition:**

Теги **\<head>** и **\<body>** есть в каждом HTML-документе, но задачи у них разные, и лежат они в разных частях веб-страницы.

**How it works:**

#### Ключевые различия

##### 1. Роль и содержимое

- **Head**: Держит метаинформацию, такую как заголовок документа, кодировку символов и таблицы стилей. Всё это нужно для настройки страницы, но пользователю не видно.
- **Body**: Содержит основную массу видимого контента, включая текст, изображения, видео, ссылки и другое.

##### 2. Место в HTML-файле

- **Head**: Идёт перед body и задаёт настройку до того, как рисуется сам контент.
- **Body**: Следует за секцией head и охватывает весь видимый контент.

##### 3. Обычные элементы в каждой секции

- **Head**: Обычно ссылается на CSS-файлы или может содержать встроенный CSS, держит заголовок документа, любую ссылку на JavaScript, объявление набора символов и метатеги.
- **Body**: Держит структурные части вроде шапок, навбаров, статей, секций и подвала, а также визуальный контент вроде изображений и видимого текста.

#### Как это выглядит в HTML-файле

- **Секция \<head>**:

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <link rel="stylesheet" type="text/css" href="styles.css" />
    </head>
    <body>
      <!-- Content Here -->
    </body>
  </html>
  ```

- **Секция \<body>**:

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <link rel="stylesheet" type="text/css" href="styles.css" />
    </head>
    <body>
      <header>
        <h1>Welcome!</h1>
        <nav>
          <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#">About</a></li>
            <li><a href="#">Contact</a></li>
          </ul>
        </nav>
      </header>

      <section>
        <h2>Recent Posts</h2>
        <article>
          <h3>Post Title</h3>
          <p>Post content goes here.</p>
        </article>
      </section>

      <footer>
        <p>&copy; 2023 MySite</p>
      </footer>
    </body>
  </html>
  ```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body
</details>

<details>
<summary>Можете ли вы explain the puRPOse of meta tags in HTML?</summary>

### Можете ли вы explain the puRPOse of meta tags in HTML?

**Definition:**

**Метатеги** дают метаданные о веб-странице: сведения, невидимые посетителям, но нужные поисковым системам, социальным сетям и другим веб-технологиям. В эти метаданные входят такие детали, как заголовок страницы, ключевые слова и описание.

**How it works:**

#### Основные метатеги

- **Meta Description**: Краткое резюме содержимого страницы, часто используется в результатах поисковой выдачи.

- **Meta Keywords**: Исторически задавали релевантные ключевые слова страницы, но их в значительной степени вывели из употребления из-за злоупотреблений спамеров.

- **Meta Robots**: Указывает ботам поисковых систем, как взаимодействовать со страницей: индексировать её для выдачи, переходить по ссылкам или не делать ни того, ни другого.

- **Meta Viewport**: Важен для адаптивного дизайна. Подсказывает браузеру, как масштабировать и показывать страницу, особенно на мобильных устройствах.

- **Meta Charset**: Задаёт кодировку символов на странице, чтобы текст отображался правильно.

- **Meta Author**: Называет создателя или автора страницы.

- **Open Graph, Twitter Cards**: Специализированные метатеги, которыми социальные платформы вроде Facebook и Twitter пользуются, чтобы лучше показывать страницу при расшаривании.

- **Canonical URL**: Указывает предпочтительный URL, когда к странице можно прийти несколькими путями.

- **Refresh and Redirect**: Более старые и менее распространённые метатеги, которые диктуют поведение страницы.

#### Пример кода: обычные метатеги

Вот HTML-код:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="description"
      content="This is a sample web page with a concise description."
    />
    <meta name="keywords" content="HTML, meta tags, web design, SEO" />
    <meta name="author" content="John Doe" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sample Web Page</title>
  </head>
  <body>
    <!-- Page content goes here -->
  </body>
</html>
```

#### Ответственное использование метатегов

По мере развития поисковых систем значение многих тегов снизилось. Текущее положение такое:

- **Всё ещё уместны**: Meta Description, Viewport, Charset, Author и Canonical
- **Ограниченный эффект**: Keywords, Refresh и Robots
- **Специальные области**: Open Graph и Twitter Cards нужны для подобранного контента на социальных платформах

Чтобы держать устойчивое присутствие в сети, опирайтесь на качественный контент, пользовательский опыт и техническую прочность и не рассчитывайте только на метатеги.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name
</details>

<details>
<summary>Как you link a CSS file to an HTML document?</summary>

### Как you link a CSS file to an HTML document?

**Definition:**

**Подключение** CSS-файла к HTML-документу — базовый шаг для оформления. Обычно это делают так: путь к CSS-файлу указывают в секции `head` HTML-файла с помощью тегов `<link>`.

**How it works:**

#### HTML-тег link: `<link>`

HTML использует тег `<link>`, чтобы **подключать внешние ресурсы**, такие как CSS-файлы.

##### Синтаксис

```html
<link rel="stylesheet" href="path/to/style.css" />
```

- **rel**: Задаёт тип связи между текущим документом и подключаемым файлом. Для CSS его ставят в «stylesheet».

- **href**: Указывает расположение внешнего CSS-файла. Это может быть **абсолютный URL** (то есть `http://...`) или **относительный путь** к HTML-файлу.

- **type**: Даётся для совместимости со старым кодом, но не обязателен, раз файл является CSS-файлом.

##### Пример кода: тег link

Вот HTML-код:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="path/to/style.css" />
  </head>
  <body>
    <!-- Body content -->
  </body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link
</details>

<details>
<summary>Как you link a JavaScript file to an HTML document?</summary>

### Как you link a JavaScript file to an HTML document?

**Definition:**

Чтобы подключить файл JavaScript к HTML-документу, нужен HTML-тег `<script>`. Есть два основных способа:

1. **Внешний файл скрипта**: Подключить отдельный файл JavaScript к HTML-документу.
2. **Встроенный скрипт**: Вставить код JavaScript прямо в HTML-файл.

**How it works:**

#### Внешний файл скрипта

Чтобы использовать внешний файл JavaScript, сделайте так:

1. **Создайте файл JavaScript**: Сохраните код JavaScript в отдельном файле с расширением `.js`. Например, `script.js`.

2. **Подключите файл JavaScript к HTML-документу**: Добавьте следующий код внутри `<head>` или в конце секции `<body>` вашего HTML-файла.

   ```html
   <script src="path-to-your-js-file.js"></script>
   ```

   Замените `path-to-your-js-file.js` на фактический путь к вашему файлу JavaScript.

##### Практические рекомендации

- **Размещение**: Хорошая практика — ставить теги `<script>` в конце секции `<body>`, прямо перед закрывающим тегом `</body>`. Тогда содержимое HTML загружается первым, и первая отрисовка сайта может пройти быстрее.

- **Синтаксис**: Спецификация HTML5 не требует закрывающего тега для элемента `<script>`.

#### Встроенный скрипт

JavaScript можно включить и прямо в HTML-файл. Это называют «встроенным скриптом». Для этого заключите код JavaScript в теги `<script>`, вот так:

```html
<script>
  // Your JavaScript code goes here
</script>
```

##### Практические рекомендации

- **Разделение содержимого**: Для более ясной организации кода часто лучше держать JavaScript в отдельном файле, особенно в крупных приложениях.

- **Кэширование**: Когда используется внешний файл JavaScript, браузер кэширует скрипт, и при следующих визитах сайт может открываться быстрее. Если же скрипт меняется часто, такое кэширование может мешать.

- **Сопровождение и повторное использование**: Внешний файл JavaScript удобнее вести, повторно использовать и править сразу в нескольких HTML-файлах.

#### Пример HTML-файла

Вот код:

#### Реализация: HTML-файл

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="path-to-your-js-file.js"></script>
  </head>
  <body>
    <!-- Your content here -->
    <script>
      // Inline JavaScript code here.
    </script>
  </body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script
</details>

<details>
<summary>Как you add a comment in HTML and why would you use them?</summary>

### Как you add a comment in HTML and why would you use them?

**Definition:**

Чтобы добавить **комментарий** в HTML, оберните его между `<!--` и `-->`.

Например:

```html
<!-- This is a comment -->
<p>Hello, World!</p>
```

**How it works:**

#### Роль комментариев в разработке

Комментарии делают код понятнее, и их используют для такого:

- **Инструкции**: Подсказывают разработчикам следующие шаги.
- **Документация**: Поясняют сложные участки кода.
- **Отладка**: Временно убирают фрагменты, чтобы проверить ошибку.
- **Напоминания**: Отмечают секции для поздней правки.

#### Практические рекомендации по комментариям

- **Ясность по делу**: Комментарии должны объяснять, что делает код, а не как. Код и встроенные комментарии должны прояснять, как код работает.
- **Уместность**: Не говорите очевидное и сосредоточьтесь на необычных или сложных частях.
- **Краткость**: Держите комментарии короткими, чтобы не загромождать вид.
- **Регулярная поддержка**: Обновляйте или удаляйте устаревшие комментарии, чтобы они оставались точными.

#### Когда комментарии не нужны?

- **Тривиальные случаи**: Комментарии вроде «<div> wrapper» или «<p> tag» называют очевидное.
- **Самодокументируемый код**: Если код **сам себя описывает**, отдельные комментарии не нужны.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Comments
</details>

<details>
<summary>Как you serve your page in multiple languages?</summary>

### Как you serve your page in multiple languages?

**Definition:**

Разберём **практические рекомендации** по отдаче веб-страниц на нескольких языках и соответствующий тег HTML5, `<html lang="en">`.

**How it works:**

#### Языковой тег

Чтобы отдавать контент на нескольких языках и улучшать доступность и работу в поисковых системах, ставьте атрибут `lang` на теге `<html>`. Это считают **хорошей практикой**, даже если страница только на английском.

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Your Website</title>
  </head>
  <body>
    <!-- Page content here -->
  </body>
</html>
```

#### Обычные коды языков

Большинство языков следуют двухбуквенному коду ISO 639-1, например «en» для английского или «es» для испанского. Некоторые языки также используют расширенный код ISO 639-2 или 639-3, которому может понадобиться три или четыре буквы, как «por» для португальского.

Для диалектов или контента под регион можно поставить дефис и код страны ISO 3166-1 alpha-2. Например:

- «en-GB» для британского английского
- «es-ES» для испанского, как на нём говорят в Испании
- «pt-BR» для бразильского португальского
- «pt-PT» для европейского португальского

Имейте в виду: хотя атрибут `lang` помогает доступности, пользовательские агенты не всегда распознают эти подтеги или действуют по ним.

#### Соображения SEO

Отдача контента на нескольких языках накладывает обязанности по SEO. Одна обычная практика — назначить _языковой_ URL каждой версии контента. Кроме того, используйте человекочитаемые URL, чтобы понятно сообщать язык и тему или оформление контента.

Например:

- `example.com/en-US/about` для страниц на американском английском.
- `example.com/es-MX/sobre` для страниц на мексиканском испанском.

#### AI-Clearance Required

Этот приём при внедрении требует дальнейшей проверки и согласования, потому что многое зависит от ограничений SEO и локализованного контента.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang
</details>

<details>
<summary>Что такое data-* attributes and when should they be used?</summary>

### Что такое data-\* attributes and when should they be used?

**Definition:**

**Атрибуты данных** в HTML5, их часто называют **`data-*` attributes**, помогают вкладывать собственные данные в HTML-элементы. Для веб-разработчиков это сильный инструмент: он упрощает операции **JavaScript** и **CSS**.

**How it works:**

#### Основные плюсы

- **Доступность**: К атрибутам данных легко обратиться через API `dataset` в JavaScript.

- **Без опоры на формат**: Там, где контент приходит с бэкенда, нельзя считать, что он всегда закодирован как JSON, сокращён или имеет странное форматирование.

- **Изоляция данных**: Так веб-документы проще сопровождать. У атрибутов данных в HTML ясная, заданная роль.

#### Пример кода: атрибуты данных

Вот HTML и JavaScript:

```html
<div id="user" data-name="John Doe" data-age="25"></div>

<script>
  const userDiv = document.getElementById("user");
  console.log(userDiv.dataset.name); // Output: "John Doe"
  console.log(userDiv.dataset.age); // Output: "25"
</script>
```

#### Уместные сценарии

- **Свои данные для элементов DOM**: Чтобы прикрепить дополнительную информацию или настройки, которые относятся только к этому HTML-элементу.

  Пример: у `div` может быть атрибут `data-show-tooltip` со значением `true`, и это значит, что элемент должен показать подсказку.

- **Настройка интерактивности**: В самодельных виджетах атрибуты данных задают поведение более структурно и по назначению. Полезно там, где у отдельных блоков DIV или section есть переключатели интерактивности или категории.

- **Электронная коммерция и веб-товары**: Чтобы хранить идентификаторы товара или дополнительные детали в том виде, в каком товар представлен в DOM каталога.

- **Признаки для стилей**: Атрибуты данных можно использовать в CSS для разного оформления: цвета категорий, эффекты при наведении или даже объявления CSS из JavaScript.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Use_data_attributes
</details>

### Text Formatting in HTML

<details>
<summary>Что такое the difference between b and strong tags?</summary>

### Что такое the difference between b and strong tags?

**Definition:**

Теги `<b>` и `<strong>` **оба используются для выделения текста** в HTML, но семантический смысл у них разный.

**How it works:**

#### Bold и strong

- Назначение тега `<b>` — сделать текст **жирным**, в основном ради визуального оформления.
- Тег `<strong>`, напротив, **семантически выделяет** текст и указывает на его важность.

#### Семантическая важность

Семантические теги вроде `<strong>` полезны скринридерам, браузерам и поисковым системам: при правильном выделении они лучше передают опыт или понимание контента.

#### Пример кода: b и strong

В HTML содержимое «Caution» визуально жирное, а содержимое «Urgent Notice!» и визуально жирное, и семантически сильное.

```html
<p>
  <b>Caution</b>: This action cannot be undone.
  <br />
  <strong>Urgent Notice!</strong> Please save your work before proceeding.
</p>
```

#### Общая практика

- **Визуальное оформление** обычно оставляют CSS. `<b>` стоит применять осторожно, если вообще применять: во многих случаях он избыточен из-за широкого распространения CSS.
- **Семантические теги** вроде `<strong>` дают контенту контекст, ясность и доступность.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/strong
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/b
</details>

<details>
<summary>Когда would you use em over i, and vice veRSA?</summary>

### Когда would you use em over i, and vice veRSA?

**Definition:**

Посмотрим на разницу между HTML-тегами 'em' и 'i'.

**How it works:**

#### Когда использовать 'em'

Тег 'em' по умолчанию делает текст курсивом, и его стоит оставлять для случаев, когда нужно ударение.

Одно возможное применение — интерактивные инструкции:

```html
<p><strong>Press</strong> <em>Enter</em> to submit.</p>
```

#### Когда использовать 'i'

Тег 'i', или тег **курсива**, для оформления текста часто обходят стороной. Вместо этого, когда можно, берите семантический HTML, CSS или более явные HTML-варианты вроде `<em>` для ударения.

Вот пример `<em>` вместе с CSS, чтобы добавить ещё немного «свечения».

```html
<p>
  His <em style="background-color: yellow; color: red;">anger</em> was palpable.
</p>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/em
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/i
</details>

<details>
<summary>Что такое the puRPOse of small, s, and mark tags?</summary>

### Что такое the puRPOse of small, s, and mark tags?

**Definition:**

HTML5-теги `small`, `s` и `mark` используют, чтобы **менять структуру и подачу** текстового контента.

**How it works:**

#### `<small>`

Тег `<small>` показывает, что заключённый текст менее важен. Обычно это **мелкий шрифт**, юридические оговорки, уведомления об авторском праве и тому подобное.

Вот примеры:

##### Сценарий

```html
<footer>
  <small>&copy; 2022 Company Name</small>
</footer>
```

#### `<s>`

Тег `<s>`, от «strike», — несемантический, малозаметный тег, который часто заменяют более осмысленным тегом, например `<del>` для «удалённого» содержимого. При этом он по-прежнему визуально **зачёркивает** своё содержимое.

##### Сценарий

```html
<p>Your discount code is: <s>EXPIRED123</s></p>
```

##### Как это выглядит

Ваш код скидки: ~~EXPIRED123~~

#### `<mark>`

Тег `<mark>` используют, чтобы **подсветить** или **отделить** текст, не задавая дополнительной семантической информации.

##### Сценарий

```html
<p>
  Important: Please <mark>schedule your appointment</mark> at least 48 hours in
  advance.
</p>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/small
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/s
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/mark
</details>

<details>
<summary>Что такое semantic HTML tags and why are they important?</summary>

### Что такое semantic HTML tags and why are they important?

**Definition:**

**Семантические HTML-теги** дают веб-контенту и структуру, и смысл. Они позволяют краулерам, браузерам и даже вспомогательным технологиям лучше понимать контент и эффективнее его подавать. Такой подход улучшает доступность и поисковую оптимизацию, и страницы проще сопровождать и понимать.

**How it works:**

#### Польза семантических тегов

- **SEO и доступность**: Семантические теги улучшают позицию страницы в поиске и делают её доступной всем пользователям, включая людей с ограничениями.

- **Согласованная структура**: Семантические теги задают цельную раскладку, что важно для больших сайтов и платформ.

- **Понятность ботам и краулерам**: Алгоритмы поисковых систем точнее разбирают веб-страницы, когда контент правильно размечен.

- **Деление контента**: Разделение контента по смыслу делает документ понятнее и проще в сопровождении.

#### Обычные семантические теги

- `<p>`: Абзац.
- `<h1> - <h6>`: Заголовки, от 1 (самый высокий) до 6 (самый низкий) уровня.
- `<ul> / <ol>`: Маркированный или нумерованный список.
- `<li>`: Пункт списка внутри списка.
- `<a>`: Якорь, используется для ссылок.
- `<img>`: Изображение.
- `<figure> / <figcaption>`: Для иллюстрации, например изображения, с подписью.

#### Обязательные и необязательные теги

Важные теги вроде `<header>` и `<footer>` обозначают ключевые секции, но многие теги необязательны и зависят от характера сайта или от того, как страница делится на части. Например, блогу может понадобиться тег `<article>`, а розничному сайту — нет.

В современной веб-разработке ясное различие, которое дают семантические теги, незаменимо для быстрого понимания и сопровождения и даёт лучший результат и пользователям, и разработчикам.

#### Пример кода: до и после семантического HTML

Сравните примеры до и после, чтобы увидеть эффект семантических тегов.

##### До семантического HTML

```html
<div class="nav">
  <div class="logo">
    <a href="#">Logo</a>
  </div>
  <div class="nav-links">
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
  </div>
</div>
<div class="main-wrapper">
  <div class="image">
    <img src="image.jpg" alt="A beautiful landscape" />
  </div>
  <div class="content">
    <h3>Welcome</h3>
    <p>Some welcome text here.</p>
  </div>
</div>
<div class="footer">
  <p>© 2022 Company Name</p>
</div>
```

##### После внедрения семантических тегов

```html
<header>
  <div class="logo">
    <a href="#">Logo</a>
  </div>
  <nav>
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
  </nav>
</header>

<main>
  <figure>
    <img src="image.jpg" alt="A beautiful landscape" />
    <figcaption>A beautiful landscape</figcaption>
  </figure>
  <section>
    <h1>Welcome</h1>
    <p>Some welcome text here.</p>
  </section>
</main>

<footer>
  <p>© 2022 Company Name</p>
</footer>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
</details>

<details>
<summary>Как you create a paragraph or a line break in HTML?</summary>

### Как you create a paragraph or a line break in HTML?

**Definition:**

В HTML **абзац** создают тегами `<p>...</p>`, а **разрыв строки** — тегом `<br>`.

**How it works:**

#### Абзацы в HTML

Обычное оформление абзаца в HTML делают тегом `<p>`. Стили браузера по умолчанию обычно добавляют **отступы** сверху и снизу каждого элемента `<p>`, и так получаются отдельные абзацы.

##### Синтаксис

```html
<p>
  This is an example of a paragraph. The text enclosed within the &lt;p&gt; tags
  represents a single paragraph.
</p>
```

##### Как это выглядит

<p>
    This is an example of a paragraph. The text enclosed within the &lt;p&gt; tags represents a single paragraph.
</p>

#### Разрывы строки в HTML

Чтобы вставить простой разрыв строки в HTML-документ, используйте тег `<br>`. Закрывающего аналога у этого тега нет.

##### Синтаксис

```html
First Line<br />Second Line
```

##### Как это выглядит

First Line<br> Second Line (Этот текст не рисует разрыв строки; он только показывает сырой HTML.)

#### Многострочные текстовые элементы

В HTML тег `<textarea>` позволяет вводить несколько строк текста. При этом он сам не оформляет абзацы. Вместо этого он **переносит текст**, а вертикальные полосы прокрутки могут включиться в зависимости от шаблона и содержимого.

##### Синтаксис

```html
<textarea rows="4" cols="50">
This is a multi-line text area.
It doesn't automatically create separate paragraphs.
Text wraps based on dimensions supplied.
</textarea>
```

##### Как это выглядит

```html
<textarea rows="4" cols="50">
This is a multi-line text area.
It doesn't automatically create separate paragraphs.
Text wraps based on dimensions supplied.
</textarea>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/br
</details>

### Links and Navigation

<details>
<summary>Как you create a hyperlink in HTML?</summary>

### Как you create a hyperlink in HTML?

**Definition:**

Гиперссылка — это элемент `<a>`, у которого `href` задаёт URL назначения. Содержимое элемента — текст ссылки.

**How it works:**

#### Якорь

- `href` может вести на страницу, файл, `mailto:`, `tel:`, `sms:` или на фрагмент той же страницы.
- Если `href` есть, Enter активирует ссылку в фокусе. Без `href` элемент ссылкой не является.
- Текст должен называть назначение. «Нажмите здесь» — слабый текст ссылки.

**When to use:**

- Переход по настоящему URL. Действие на странице, которое никуда не ведёт, ставят на `<button>`.

**Examples:**

```html
<p>You can reach Michael at:</p>
<ul>
  <li><a href="https://example.com">Website</a></li>
  <li><a href="mailto:m.bluth@example.com">Email</a></li>
  <li><a href="tel:+123456789">Phone</a></li>
</ul>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>Что такое the difference between relative and absolute URLs?</summary>

### Что такое the difference between relative and absolute URLs?

**Definition:**

Абсолютный URL содержит части, достаточные, чтобы найти ресурс самостоятельно. Относительный URL опускает часть этих компонентов, и браузер дополняет их из URL текущего документа.

**How it works:**

#### Абсолютный URL

- Включает схему и остальной адрес, например `https://developer.mozilla.org/en-US/docs/Web/HTML`.
- В адресной строке нет документа, у которого можно заимствовать части, поэтому нужен абсолютный URL.

#### Относительный URL

- Схемно-относительный (`//example.com`) сохраняет схему текущей страницы.
- Путь, который начинается с `/`, разрешается от корня origin с текущими схемой и хостом.
- `p` и `./p` разрешаются относительно текущего каталога. `../p` поднимается на уровень вверх.
- `#section` оставляет текущий документ и меняет только фрагмент. Фрагмент на сервер не отправляется.

**When to use:**

- Абсолютный URL — для другого сайта.
- Относительный URL — для ресурса на том же сайте, чтобы ссылка работала при смене хоста.

**Examples:**

```html
<a href="https://example.com">Absolute URL</a>
<a href="//example.com">Scheme-relative URL</a>
<a href="/en-US/docs/Web/HTML">Origin-relative URL</a>
<a href="./p">Directory-relative URL</a>
<a href="../p">Parent-directory-relative URL</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>Как you open a link in a new tab?</summary>

### Как you open a link in a new tab?

**Definition:**

Поставьте на `<a>` атрибут `target="_blank"`. Браузер обычно открывает этот URL в новой вкладке.

**How it works:**

#### Ключевые слова `target`

- `_self` — значение по умолчанию: текущий контекст просмотра.
- `_blank` — обычно новая вкладка. Пользователь может настроить вместо этого новое окно.
- `_parent` — родительский контекст, или `_self`, если родителя нет.
- `_top` — самый верхний контекст-предок.

#### Что видит новая страница

- `target="_blank"` уже ведёт себя как `rel="noopener"`: у нового контекста `window.opener` равен `null`.
- В тексте ссылки скажите, что она открывается в новой вкладке, чтобы смена контекста не была неожиданностью.

**When to use:**

- Когда уход со страницы потеряет место пользователя, например для справки на другом сайте. Большинство ссылок должны оставаться в той же вкладке.

**Examples:**

```html
<a target="_blank" href="https://www.wikipedia.org">
  Wikipedia (opens in new tab)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/rel/noopener
</details>

<details>
<summary>Как you create an anchor to jump to a specific part of the page?</summary>

### Как you create an anchor to jump to a specific part of the page?

**Definition:**

Поставьте на целевой элемент `id`, а у ссылки задайте `href` как `#` плюс этот id.

**How it works:**

#### Идентификатор фрагмента

- Браузер прокручивает страницу к элементу, чей `id` совпадает с фрагментом.
- Фрагмент не отправляется на сервер вместе с запросом.
- `href="#top"` или пустой фрагмент `href="#"` ведёт в начало текущей страницы.
- Старый атрибут `name` у `<a>` устарел. Используйте глобальный атрибут `id`.

**When to use:**

- Оглавление, skip-ссылка или любой переход внутри того же документа.

**Examples:**

```html
<p><a href="#Section_further_down">Jump to the heading below</a></p>

<h2 id="Section_further_down">Section further down</h2>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL
</details>

<details>
<summary>Как you link to a downloadable file in HTML?</summary>

### Как you link to a downloadable file in HTML?

**Definition:**

Направьте `<a>` на файл через `href` и добавьте `download`, чтобы браузер считал этот URL загрузкой.

**How it works:**

#### Атрибут `download`

- Без значения браузер предлагает имя из заголовка `Content-Disposition`, последнего сегмента пути URL или медиатипа.
- Значение вроде `download="report.pdf"` предлагает это имя файла. `/` и `\` заменяются на подчёркивания.
- Атрибут действует для URL того же origin, а также для `blob:` и `data:`. Кросс-оригинный HTTP URL его не учитывает.
- Если в `Content-Disposition` есть имя файла, оно важнее значения атрибута.
- Текст ссылки должен сообщать, что назначение — файл.

**When to use:**

- Файл, который пользователь должен сохранить, отданный с того же origin, что и страница.

**Examples:**

```html
<a href="2017-annual-report.ppt" download="2017-annual-report.ppt">
  2017 annual report (PowerPoint file)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

### Images and Objects

<details>
<summary>Как you embed images in an HTML page?</summary>

### Как you embed images in an HTML page?

**Definition:**

Используйте элемент `<img>`. `src` — URL изображения, `alt` — замещающий текст.

**How it works:**

#### Элемент изображения

- У `<img>` нет закрывающего тега. Он встраивает ресурс, указанный в `src`.
- `width` и `height` — целые числа в CSS-пикселях, без единицы. Вместе они дают браузеру соотношение сторон до загрузки файла.
- `loading="lazy"` откладывает запрос, пока изображение не окажется рядом с областью просмотра. `eager` — значение по умолчанию, загрузка сразу.
- Чтобы отдать другой кадр или формат по условию, оберните варианты в `<picture>` и оставьте один `<img>` как запасной.

**When to use:**

- Фотография, схема или значок, которые входят в документ. Вектор, который должен быть в DOM, можно вставить как встроенный SVG.

**Examples:**

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
  width="400"
  height="341"
/>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>Что такое the importance of the alt attribute for images?</summary>

### Что такое the importance of the alt attribute for images?

**Definition:**

`alt` — текст, которым заменяют изображение, когда браузер его не показывает, и имя, которое вспомогательные технологии используют для этого изображения.

**How it works:**

#### Когда изображение не показано

- Невизуальные браузеры, пользователи, которые отключили картинки, и битый или неподдерживаемый файл могут опереться на `alt`.
- Тот же текст используется, когда изображение копируют как текст или сохраняют из ссылки в закладку.
- `alt=""` значит, что изображение декоративное или это пиксель отслеживания. Невизуальные браузеры могут его пропустить, а визуальный браузер прячет значок битой картинки, если файл не загрузился.
- Отсутствие `alt` — не то же самое, что пустое значение: тогда у браузера нет замещающего текста.

**When to use:**

- Содержательному изображению дают короткое описание того, что на нём.
- Декоративному изображению дают `alt=""`.
- У каждой `<area href>` в карте изображения свой `alt` с назначением этой области.

**Examples:**

```html
<img
  src="/shared-assets/images/examples/grapefruit-slice.jpg"
  alt="Grapefruit slice atop a pile of other slices"
/>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>Какие image formats поддерживают web browsers?</summary>

### Какие image formats поддерживают web browsers?

**Definition:**

Браузеры обычно декодируют GIF, JPEG, PNG, APNG, SVG, WebP и AVIF. Поддержка остальных типов уже.

**How it works:**

#### Широко используемые типы

- **GIF** (`image/gif`): индексированный цвет и простая анимация.
- **JPEG** (`image/jpeg`): фотографии с потерей качества. Альфа-канала нет.
- **PNG** (`image/png`): изображения без потерь, в том числе с прозрачностью.
- **APNG** (`image/apng`): анимация без потерь. Chrome, Edge, Firefox, Opera и Safari.
- **SVG** (`image/svg+xml`): вектор, который остаётся чётким при любом размере.
- **WebP** (`image/webp`): неподвижные и анимированные изображения, сжатие лучше, чем у JPEG или PNG. Chrome, Edge, Firefox, Opera и Safari.
- **AVIF** (`image/avif`): неподвижные и анимированные изображения, обычно меньше WebP. Те же текущие основные браузеры. Дайте запасной вариант в `<picture>`, потому что старые браузеры его не декодируют.

#### Более узкая поддержка

- **JPEG XL** (`image/jxl`) поддерживают не все: Safari — да, Chrome только за флагом, Firefox в Nightly. Ставьте запасной формат.
- **BMP** и **ICO** декодируются в основных браузерах, но MDN не советует их для содержимого страницы.
- **TIFF** поддерживает Safari.

**When to use:**

- JPEG или AVIF/WebP для фотографий. PNG или SVG, когда нужны резкие края или прозрачность. Для AVIF и JPEG XL нужен запасной вариант в `<picture>`.

**Examples:**

```html
<picture>
  <source srcset="photo.avif" type="image/avif" />
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="photo" />
</picture>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
</details>

<details>
<summary>Как you create image Maps in HTML?</summary>

### Как you create image Maps in HTML?

**Definition:**

Клиентская карта изображения — это `<map>` из ссылок `<area>`, на которую `<img>` ссылается через `usemap`.

**How it works:**

#### Клиентская карта

- У `<map name="primary">` обязателен непустой `name` без пробелов, уникальный в документе. Если задан и `id`, он должен совпадать с `name`.
- Каждая `<area>` — фигура `rect`, `circle` или `poly`, с `coords`, а также `href` и `alt`.
- Изображение указывает на карту через `usemap="#primary"`. Значение — решётка плюс `name` карты.
- `alt` у каждой области со ссылкой называет назначение этой области.

#### Серверная карта

- `ismap` на `<img>` отправляет координаты щелчка на сервер. Атрибут допустим только если это изображение внутри `<a>` с `href`, чтобы было запасное назначение.

**When to use:**

- Несколько ссылок на одной картинке, например на схеме. Отдельные элементы управления удобнее, когда области на самом деле не части одного изображения.

**Examples:**

```html
<map name="primary">
  <area
    shape="circle"
    coords="75,75,75"
    href="https://developer.mozilla.org/docs/Web/JavaScript"
    alt="JavaScript"
  />
  <area
    shape="circle"
    coords="275,75,75"
    href="https://developer.mozilla.org/docs/Web/CSS"
    alt="CSS"
  />
</map>
<img
  usemap="#primary"
  src="parrots.jpg"
  alt="350 x 150 picture of two parrots"
/>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/map
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>Что такое the difference between svg and canvas elements?</summary>

### Что такое the difference between svg and canvas elements?

**Definition:**

SVG — это XML-разметка векторных фигур, которая остаётся в DOM. `<canvas>` — растр, в который рисуют через Canvas API или WebGL.

**How it works:**

#### SVG

- Фигуры — элементы (`rect`, `circle`, `text` и другие). Более поздние элементы рисуются поверх более ранних.
- Графика масштабируется без «лесенки», а текст можно менять без редактора растра.
- Разметку можно встроить в HTML или сослаться на файл через `<img>`, `<object>` или `<iframe>`.
- У встроенного SVG пространство имён `xmlns="http://www.w3.org/2000/svg"`.

#### Canvas

- Элемент — пустая поверхность для рисования. `width` по умолчанию 300, `height` — 150, в CSS-пикселях.
- Задавайте эти атрибуты на элементе. Размер только через CSS масштабирует растр и может его исказить.
- Содержимое внутри `<canvas>` — запасной вариант для браузеров, которые не умеют его рисовать, и для случая без JavaScript.
- Нарисованные объекты не являются семантическим HTML. Вспомогательные технологии не видят их как отдельные элементы.

**When to use:**

- SVG для значков, схем и интерфейса, который должен оставаться чётким и выделяемым.
- Canvas для сцен на скрипте или WebGL, например холста для рисования или кадра игры, с запасным текстом внутри элемента.

**Examples:**

```html
<svg width="300" height="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="150" cy="100" r="80" fill="green" />
</svg>

<canvas width="120" height="120">
  An alternative text describing what your canvas displays.
</canvas>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/SVG
- https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/canvas
</details>

### Lists

<details>
<summary>Что такое the different types of lists available in HTML?</summary>

### Что такое the different types of lists available in HTML?

**Definition:**

В HTML три элемента списка: `<ul>` для маркированного, `<ol>` для нумерованного и `<dl>` для списка описаний.

**How it works:**

#### Какой список

- `<ul>` группирует пункты, чей порядок не меняет смысл. Пункты — `<li>`, обычно с маркером.
- `<ol>` группирует пункты, чей порядок важен: шаги рецепта, маршрут или рейтинг. Пункты — `<li>`, обычно с номерами.
- `<dl>` группирует термины (`<dt>`) и описания (`<dd>`): глоссарий или метаданные «ключ — значение». У одного термина может быть несколько описаний, и несколько терминов могут делить одно описание.
- Чтобы выбрать между `<ul>` и `<ol>`, поменяйте пункты местами. Если смысл меняется, нужен `<ol>`.

**When to use:**

- Маркеры для набора, номера для последовательности, список описаний для имён в паре с определениями. Не используйте `<dl>` только ради отступа.

**Examples:**

```html
<ul>
  <li>Milk</li>
  <li>Cheese</li>
</ul>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dl
</details>

<details>
<summary>Как you create ordered, unordered, and description lists in HTML?</summary>

### Как you create ordered, unordered, and description lists in HTML?

**Definition:**

Каждый пункт оберните в `<li>` внутри `<ul>` или `<ol>`. В `<dl>` поставьте один или несколько терминов `<dt>`, затем одно или несколько описаний `<dd>`.

**How it works:**

#### Разметка

- `<ul>`, `<ol>`, `<dl>`, `<dt>` и `<dd>` требуют оба тега. Закрывающий тег `<li>` можно опустить, если сразу следует другой `<li>` или в родителе больше ничего нет.
- Список описаний может обернуть каждую пару «имя — значение» в `<div>`.
- `<dd>` — это описание, а не второй пункт списка. Не ставьте `<li>` внутрь `<dl>`.

**When to use:**

- Элемент списка соответствует смыслу из предыдущего вопроса. CSS меняет маркер и не меняет, какой элемент выбрать.

**Examples:**

```html
<ol>
  <li>Mix flour, baking powder, sugar, and salt.</li>
  <li>In another bowl, mix eggs, milk, and oil.</li>
</ol>

<dl>
  <dt>Beast of Bodmin</dt>
  <dd>A large feline inhabiting Bodmin Moor.</dd>
</dl>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dl
</details>

<details>
<summary>Можно ли lists be nested in HTML? If so, how?</summary>

### Можно ли lists be nested in HTML? If so, how?

**Definition:**

Да. Внутренний `<ul>` или `<ol>` ставят внутрь `<li>` и закрывают этот `<li>` после внутреннего списка.

**How it works:**

#### Куда попадает внутренний список

- `<ul>` и `<ol>` можно вкладывать на любую глубину, и они могут чередоваться.
- Вложенный список — ребёнок `<li>`, а не сосед, который стоит между двумя пунктами.
- Вложенный список допустим, потому что он находится внутри `<li>` — элемента пункта, который эти списки содержат.

**When to use:**

- Подшаг или группа, которая принадлежит одному пункту, например ингредиенты одного шага рецепта.

**Examples:**

```html
<ol>
  <li>first item</li>
  <li>
    second item
    <ul>
      <li>second item first subitem</li>
      <li>second item second subitem</li>
    </ul>
  </li>
  <li>third item</li>
</ol>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
</details>

<details>
<summary>Какие attributes можно использовать с lists для изменения appearance или behavior?</summary>

### Какие attributes можно использовать с lists для изменения appearance или behavior?

**Definition:**

Поведение нумерации живёт на `<ol>` и `<li>`. Форма маркера — дело CSS: HTML-атрибут `type` у `<ul>` и `compact` устарели.

**How it works:**

#### Нумерованные списки

- `start` — арабское целое, с которого начинается счёт, даже если маркер — буква или римская цифра. `start="4"` начинает с «d» или «iv».
- `reversed` нумерует от большего к меньшему.
- `type` бывает `1` (по умолчанию), `a`, `A`, `i` или `I`. Используйте его, когда сам маркер входит в смысл, как в юридическом списке. Иначе задайте `list-style-type` в CSS.
- `value` на `<li>` задаёт номер этого пункта. Следующие пункты продолжают счёт от него.

#### Устаревшее

- `type` на `<ul>` (`disc`, `circle`, `square`) устарел. Используйте CSS `list-style-type`.
- `compact` на `<ul>`, `<ol>` и `<dl>` устарел. Используйте CSS.

**When to use:**

- `start`, `reversed` и `value`, когда последовательность — настоящие данные, например места 4, 5 и 6. Вид маркеров оставьте CSS.

**Examples:**

```html
<ol start="4" reversed>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
  <li>Wash and chop the salad.</li>
</ol>

<ol type="I">
  <li value="3">third item</li>
  <li>fourth item</li>
</ol>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/li
</details>

### Forms and Input Elements

<details>
<summary>Что такое HTML forms and how do you create one?</summary>

### Что такое HTML forms and how do you create one?

**Definition:**

`<form>` — это раздел элементов управления, которые отправляют информацию. `action` — URL, который обрабатывает отправку, `method` — метод HTTP.

**How it works:**

#### Отправка

- `method="get"` (значение по умолчанию) дописывает поля к URL `action`. Используйте его, когда у формы нет побочных эффектов.
- `method="post"` отправляет поля телом запроса.
- `method="dialog"` закрывает окружающий `<dialog>` и вызывает `submit`, не отправляя данные.
- `enctype` важен для `post`: `application/x-www-form-urlencoded` — значение по умолчанию, а `multipart/form-data` нужен, когда в форме есть поле файла.
- `novalidate` пропускает проверку ограничений при отправке. `target` выбирает контекст просмотра для ответа, с теми же ключевыми словами, что у ссылки (`_self`, `_blank`, `_parent`, `_top`).
- У каждого элемента, который должен уйти на сервер, нужен `name`.

**When to use:**

- Любой набор элементов, чьи значения идут на сервер, или форма внутри диалога, которая должна только закрыть диалог.

**Examples:**

```html
<form action="/my-handling-form-page" method="post">
  <label for="name">Name:</label>
  <input type="text" id="name" name="user_name" />
  <button>Save</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form
</details>

<details>
<summary>Опишите the different form input types in HTML5.</summary>

### Опишите the different form input types in HTML5.

**Definition:**

Атрибут `type` у `<input>` выбирает элемент управления. Если его опустить, тип — `text`.

**How it works:**

#### Текст

- `text` — одна строка. Переводы строк вырезаются.
- `search`, `tel`, `url` и `email` — однострочные поля с подходящей клавиатурой и, для `url` и `email`, проверкой.
- `password` скрывает значение.

#### Выбор и кнопки

- `checkbox` переключает одно значение. Кнопки `radio` с общим `name` позволяют одно значение из группы.
- У `button` нет поведения по умолчанию. `submit` отправляет форму. `reset` возвращает значения по умолчанию, и его не рекомендуют.
- `image` — графическая кнопка отправки. Ей нужны `src` и `alt`.

#### Значения с виджетом

- `number` и `range` вводят число. `range` — когда точное значение неважно; по умолчанию это середина между `min` и `max`.
- `date`, `month`, `week`, `time` и `datetime-local` вводят дату или время без часового пояса.
- `color` открывает выбор цвета. `file` выбирает файл; `accept` подсказывает типы.
- `hidden` отправляется и не показывается.

#### Устаревшее

- `datetime` устарел. Используйте `datetime-local`.

**When to use:**

- Тип, который соответствует данным, чтобы браузер дал нужную клавиатуру и проверки. Другие элементы, которые не являются `<input>`: `<textarea>`, `<select>` и `<button>`.

**Examples:**

```html
<label for="mail">Email:</label>
<input type="email" id="mail" name="user_email" />

<input type="date" name="trip" />
<input type="file" name="avatar" accept="image/png, image/jpeg" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input
</details>

<details>
<summary>Как you make form inputs required?</summary>

### Как you make form inputs required?

**Definition:**

Добавьте логический атрибут `required`. Форма не отправится, пока у этого элемента нет значения.

**How it works:**

#### Где он действует

- Он работает на `text`, `search`, `url`, `tel`, `email`, `password`, `date`, `month`, `week`, `time`, `datetime-local`, `number`, `checkbox`, `radio` и `file`, а также на `<select>` и `<textarea>`.
- Он не применяется к `range` и `color` (у них всегда есть значение по умолчанию), к `hidden` и к типам кнопок, включая `image`.
- Пустое обязательное значение не проходит проверку с `valueMissing` и соответствует `:invalid`.
- `required` не действует на элемент, у которого также стоит `readonly`.
- В группе radio `required` на любой одной кнопке значит, что одна кнопка с этим `name` должна быть отмечена. Ставьте его на каждую кнопку группы или ни на одну, чтобы разметка была очевидной.
- Для checkbox с общим именем обязательны только те, на которых есть `required`.
- `aria-required="true"` объявляет обязательность программе чтения с экрана и не блокирует отправку.

**When to use:**

- Поле, которое сервер не может принять пустым. Покажите это требование ещё и видимым текстом. Одного цвета недостаточно.

**Examples:**

```html
<form method="post">
  <label for="fname">First name (required):</label>
  <input id="fname" name="fname" type="text" required />
  <button>Save</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/required
</details>

<details>
<summary>Что такое the puRPOse of the label element in forms?</summary>

### Что такое the puRPOse of the label element in forms?

**Definition:**

`<label>` — подпись элемента формы. Активация подписи переводит фокус на этот элемент, а вспомогательные технологии читают подпись вместе с ним.

**How it works:**

#### Две связи

- Явная: `for` у подписи совпадает с `id` элемента. Первый подходящий элемент с этим `id` и есть подписываемый элемент.
- Неявная: вложите элемент внутрь подписи. Тогда `for` и `id` не нужны.
- Явная связь совместимее. Можно и вложить, и задать `for`/`id`, если `for` указывает на вложенный элемент.
- Щелчок или касание подписи переводит фокус на элемент и увеличивает область нажатия.
- Подписываемые элементы включают `<button>`, `<input>` (кроме `type="hidden"`), `<meter>`, `<output>`, `<progress>`, `<select>` и `<textarea>`.
- Не ставьте внутрь подписи другую ссылку, кнопку или заголовок. Полю `type="button"` со `value` и элементу `<button>` отдельная подпись не нужна.

**When to use:**

- Каждый элемент, который человек должен понять, кроме кнопок, чей собственный текст уже является именем.

**Examples:**

```html
<label for="username">Click me to focus on the input field</label>
<input type="text" id="username" name="username" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/label
</details>

<details>
<summary>Как you group form inputs and why would you do this?</summary>

### Как you group form inputs and why would you do this?

**Definition:**

Связанные элементы оберните в `<fieldset>` и назовите группу через `<legend>`. Группа объявляется как одно целое.

**How it works:**

#### Fieldset

- Первый `<legend>` внутри fieldset — его подпись.
- `disabled` на fieldset отключает вложенные элементы: их нельзя править, и они не отправляются. Элементы формы внутри вложенного `<legend>` этим атрибутом не отключаются.
- `name` называет группу. `form` может указывать на `id` формы, когда fieldset вне этой формы; самим элементам всё равно нужен собственный атрибут `form`, чтобы войти в эту форму.
- Неявная роль ARIA — `group`. Группе radio можно задать `role="radiogroup"`.

**When to use:**

- Набор radio или checkbox с одним вопросом или любые элементы с одной подписью. Заголовок целого раздела формы может быть и heading, но подпись fieldset — это `<legend>`.

**Examples:**

```html
<form method="post">
  <fieldset>
    <legend>Choose your favorite monster</legend>
    <input type="radio" id="kraken" name="monster" value="K" />
    <label for="kraken">Kraken</label>
    <input type="radio" id="sasquatch" name="monster" value="S" />
    <label for="sasquatch">Sasquatch</label>
  </fieldset>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/fieldset
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form
</details>

### HTML5 Features

<details>
<summary>Что такое new in HTML5 compared to previous versions?</summary>

### Что такое new in HTML5 compared to previous versions?

**Definition:**

HTML5 пришёл после HTML 4.01 и добавил элементы и возможности для структуры, медиа и форм. Сейчас HTML — это живой стандарт WHATWG, и номера версии у него нет.

**How it works:**

#### Что изменилось

- Страница использует `<!doctype html>`. Этот doctype выбирает текущий HTML, а не пронумерованный снимок HTML5.
- Элементы разделов называют области, которые раньше были безымянными `<div>`: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>` и `<footer>`.
- `<figure>` и `<figcaption>` собирают самостоятельную фигуру с подписью.
- `<video>` и `<audio>` встраивают плеер. `<canvas>` — растровая поверхность для рисования.
- У элементов форм появились типы вроде `email`, `url`, `date` и `number`, плюс атрибут `required`.
- До 2019 года W3C тоже публиковал нумерованный HTML5. 28 мая 2019 года W3C назвал живой стандарт WHATWG единственным HTML.

**When to use:**

- Элементы выше, когда они совпадают с содержимым. Блок, который нужен только как крючок для стилей, остаётся `<div>`.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>My page title</title>
  </head>
  <body>
    <header><h1>Header</h1></header>
    <main><p>Page content.</p></main>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/HTML5
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents
</details>

<details>
<summary>Как you create a section on a webpage using HTML5 semantic elements?</summary>

### Как you create a section on a webpage using HTML5 semantic elements?

**Definition:**

Используйте `<section>` для общей самостоятельной области, у которой нет более конкретного элемента. Дайте ей заголовок.

**How it works:**

#### Выбор области

- `<header>` — вводное содержимое. Внутри `<body>` это шапка страницы; внутри `<article>` или `<section>` он возглавляет эту область.
- `<nav>` — крупный блок навигационных ссылок.
- `<main>` — содержимое, уникальное для этой страницы. Используйте его один раз, прямо в `<body>`.
- `<article>` — самостоятельный фрагмент, например пост. `<section>` группирует одну тему или одну функцию внутри него или рядом.
- `<aside>` — косвенно связанное содержимое, например связанные ссылки. `<footer>` — завершающие сведения.
- `<section>` должен начинаться с заголовка (`<h1>`–`<h6>`), кроме части интерфейса приложения, например панели кнопок.
- Если нужна только обёртка для стилей, используйте `<div>`.

**When to use:**

- Блок с заголовком на одну тему: глава, набор заголовков или карта с её элементами управления. Меню навигации — это `<nav>`, а не `<section>`.

**Examples:**

```html
<section>
  <h2>Introduction</h2>
  <p>
    This document provides a guide to help with the important task of choosing
    the correct Apple.
  </p>
</section>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/section
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents
</details>

<details>
<summary>Что такое the role of the article element in HTML5?</summary>

### Что такое the role of the article element in HTML5?

**Definition:**

`<article>` — самостоятельная композиция, которую можно распространять или переиспользовать отдельно: пост, новость, комментарий или карточка товара.

**How it works:**

#### Самостоятельность

- Каждый article обозначайте заголовком (`<h1>`–`<h6>`).
- Вложенный `<article>` связан с внешним. Комментарии к посту — это article внутри article поста.
- Сведения об авторе можно поместить в `<footer>`, но этот footer не относится к вложенным article.
- Время публикации ставят на элемент `<time datetime>`.
- `<article>` не должен быть потомком `<address>`.

**When to use:**

- Единица, которая сохраняет смысл в ленте или на другой странице. Общая тема, которую нельзя переиспользовать отдельно, — это `<section>`.

**Examples:**

```html
<article>
  <h2>Jurassic Park</h2>
  <p>Dinos were great!</p>
  <footer>
    <p>Posted on <time datetime="2015-05-15 19:00">May 15</time> by Staff.</p>
  </footer>
</article>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/article
</details>

<details>
<summary>Можете ли вы explain the use of the nav and aside elements in HTML5?</summary>

### Можете ли вы explain the use of the nav and aside elements in HTML5?

**Definition:**

`<nav>` — крупный блок навигационных ссылок. `<aside>` — содержимое, лишь косвенно связанное с основным, часто боковая колонка или врезка.

**How it works:**

#### Навигация

- Обычное содержимое — меню, оглавление или указатель. Не каждая ссылка на странице принадлежит `<nav>`.
- В документе может быть несколько `<nav>`: навигация по сайту и навигация внутри страницы. `aria-labelledby` может назвать каждый из них.
- Неявная роль — `navigation`. Программы чтения с экрана могут пропускать чисто навигационное содержимое.

#### Aside

- Используйте его для глоссарной заметки, биографии автора или связанных ссылок.
- Не помечайте как `<aside>` текст в скобках, который входит в основное предложение.
- Неявная роль — `complementary`, или `generic`, если у aside нет доступного имени и он лежит внутри другого секционного содержимого.
- `<aside>` не должен быть потомком `<address>`.

**When to use:**

- `<nav>` для основного способа перемещаться по сайту или документу. `<aside>` для материала, который можно пропустить, не потеряв главную мысль.

**Examples:**

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<aside>
  <p>The movie earned $87 million during its initial release.</p>
</aside>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/aside
</details>

<details>
<summary>Как you use the figure and figCAPtion elements?</summary>

### Как you use the figure and figCAPtion elements?

**Definition:**

`<figure>` — самостоятельное содержимое, например изображение, схема или фрагмент кода, которое можно перенести, не ломая основной поток. `<figcaption>` — его подпись.

**How it works:**

#### Подпись

- Поставьте `<figcaption>` первым или последним ребёнком figure. Первая найденная подпись и есть подпись фигуры.
- Подпись — доступное имя фигуры.
- Figure может содержать `<img>`, `<pre>`, `<blockquote>` или другое потоковое содержимое.
- `alt` у изображения по-прежнему описывает изображение. Подпись называет фигуру и не заменяет `alt`.

**When to use:**

- То, на что текст ссылается как на единицу и что можно вынести в приложение. Декоративному изображению figure не нужен.

**Examples:**

```html
<figure>
  <img
    src="/shared-assets/images/examples/elephant.jpg"
    alt="Elephant at sunset"
  />
  <figcaption>An elephant at sunset</figcaption>
</figure>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/figure
</details>

### Tables

<details>
<summary>Как you create a table in HTML?</summary>

### Как you create a table in HTML?

**Definition:**

Таблица данных — это `<table>` из строк (`<tr>`). Ячейки заголовка — `<th>`, ячейки данных — `<td>`.

**How it works:**

#### Части

- `<caption>`, первый ребёнок `<table>`, описывает, что это за таблица.
- `<thead>`, `<tbody>` и `<tfoot>` группируют строки заголовка, данных и подвала. Если опустить `<tbody>`, браузер всё равно вставит его.
- `scope` на `<th>` говорит, покрывает ли заголовок `col`, `row`, `colgroup` или `rowgroup`.
- Таблица — для данных со связью по строкам и столбцам. Раскладка страницы в таблицу не кладётся.

**When to use:**

- Сетка связанных фактов, например расписание или список записей. Заголовка и абзацев достаточно, когда связи «строка — столбец» нет.

**Examples:**

```html
<table>
  <caption>
    Alien football stars
  </caption>
  <thead>
    <tr>
      <th scope="col">Player</th>
      <th scope="col">Gloobles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">TR-7</th>
      <td>7</td>
    </tr>
  </tbody>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/table
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>Что такое thead, tbody, and tfoot in a table?</summary>

### Что такое thead, tbody, and tfoot in a table?

**Definition:**

Они делят строки таблицы на заголовок, тело и подвал. Пишите их в таком порядке: `<thead>`, затем `<tbody>`, затем `<tfoot>`.

**How it works:**

#### Что в каждой группе

- `<thead>` оборачивает строки заголовка, обычно названия столбцов. Если есть `<colgroup>`, заголовок идёт после него.
- `<tbody>` оборачивает строки данных. Таблица всегда имеет его, даже если вы не пишете теги.
- `<tfoot>` оборачивает подвал, например строку итогов. Он идёт после `<tbody>`.
- Сами по себе эти группы не меняют то, что объявляет программа чтения с экрана. Они дают CSS и печати место для стилей: у длинной таблицы заголовок и подвал могут повторяться на каждой печатной странице, а тело — прокручиваться.

**When to use:**

- Любая таблица с настоящей строкой заголовка и с подвалом, когда последняя строка суммирует тело.

**Examples:**

```html
<table>
  <thead>
    <tr>
      <th>Student ID</th>
      <th>Name</th>
      <th>Credits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3741255</td>
      <td>Jones, Martha</td>
      <td>240</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th colspan="2" scope="row">Average Credits</th>
      <td>240</td>
    </tr>
  </tfoot>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/tfoot
</details>

<details>
<summary>Что такое colspan and rowspan?</summary>

### Что такое colspan and rowspan?

**Definition:**

`colspan` — сколько столбцов покрывает ячейка. `rowspan` — сколько строк. Оба по умолчанию равны `1`.

**How it works:**

#### Объединение

- Значение — неотрицательное целое. На `<th>` `colspan` выше 1000 обрезается до 1000, а `rowspan` выше 65534 — до 65534.
- `rowspan="0"` протягивает заголовок до конца его группы таблицы (`thead`, `tbody` или `tfoot`).
- Ячейки, которые вы не объединяете, по-прежнему занимают слот 1×1. Объединённая ячейка не должна перекрывать другую.
- У заголовка, который охватывает несколько ячеек, всё равно должно быть сказано, какие ячейки он называет: через `scope` или, в более запутанной шапке, через `headers` со ссылками на `id` других заголовков.

**When to use:**

- Подпись, которая правда покрывает несколько столбцов или строк, например «Membership Dates» над «Joined» и «Canceled», или подпись итога через столбцы, которые она суммирует.

**Examples:**

```html
<tr>
  <th scope="col" rowspan="2">Symbol</th>
  <th scope="col" rowspan="2">Code word</th>
  <th scope="col" colspan="2">Pronunciation</th>
</tr>
<tr>
  <th scope="col">IPA</th>
  <th scope="col">Respelling</th>
</tr>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>Как you make a table accessible?</summary>

### Как you make a table accessible?

**Definition:**

Дайте таблице `<caption>`, пометьте ячейки заголовка через `<th>` и задайте `scope`, чтобы программа чтения с экрана связала каждую ячейку данных с её заголовками.

**How it works:**

#### Программные заголовки

- Зрячий читатель сопоставляет ячейку с заголовками взглядом. Программе чтения нужны те же связи в разметке.
- `scope="col"` у заголовка столбца и `scope="row"` у заголовка строки достаточны для простой сетки. Часть вспомогательных средств выводит `scope` сама, но явное значение надёжнее.
- Если заголовку самому нужны другие заголовки, дайте этим `<th>` атрибуты `id` и перечислите их в `headers`.
- `<caption>` — описание, сразу после открывающего тега `<table>`. Старый атрибут `summary` устарел и невидим зрячим читателям.
- `<thead>`, `<tbody>` и `<tfoot>` в основном для стилей и для повтора шапки и подвала при печати. Они не заменяют `<th>` и `scope`.

**When to use:**

- Каждая таблица данных. Раскладка, которая не является табличными данными, вообще не должна быть таблицей.

**Examples:**

```html
<table>
  <caption>
    Dinosaurs in the Jurassic period
  </caption>
  <tr>
    <th scope="col">Symbol</th>
    <th scope="col">Code word</th>
  </tr>
  <tr>
    <th scope="row">A</th>
    <td>Alfa</td>
  </tr>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>Как tables be made responsive?</summary>

### Как tables be made responsive?

**Definition:**

Оставьте данные в настоящей `<table>` и задайте размер через CSS. В HTML нет атрибута, который делает таблицу адаптивной.

**How it works:**

#### Раскладка в CSS

- `table-layout: fixed` задаёт ширину столбцов по заголовкам, а не по самой длинной ячейке, и так предсказуемее.
- Ширина в процентах, например `90%`, позволяет таблице следовать за viewport и оставаться по центру с `margin: 10px auto`, как в руководстве MDN по стилям таблиц.
- У длинной таблицы шапка и подвал могут оставаться на месте, а `<tbody>` — прокручиваться.
- Презентационные атрибуты вроде `width`, `height`, `align` и `bgcolor` на `<th>` устарели. Используйте CSS.
- Не пересобирайте данные в стопку `<div>`. Связи строк и столбцов как раз делают таблицу понятной.

**When to use:**

- Всякий раз, когда одну и ту же таблицу читают в узком и в широком окне. Меняйте CSS, когда содержимое становится тесным, и не выбрасывайте ячейки заголовка.

**Examples:**

```html
<table>
  <caption>
    A summary of the UK's most famous punk bands
  </caption>
  <thead>
    <tr>
      <th scope="col">Band</th>
      <th scope="col">Year formed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Buzzcocks</th>
      <td>1976</td>
    </tr>
  </tbody>
</table>
```

```css
table {
  table-layout: fixed;
  width: 90%;
  margin: 10px auto;
  border-collapse: collapse;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Tables
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
</details>

### Media in HTML

<details>
<summary>Как you add audio and video to an HTML document?</summary>

### Как you add audio and video to an HTML document?

**Definition:**

`<video>` — плеер видео, `<audio>` — звук. Укажите файл в `src` или вложите элементы `<source>`, и браузер возьмёт первый тип, который умеет воспроизвести.

**How it works:**

#### Плеер

- `controls` показывает элементы управления браузера. Без него у `<audio>` нет видимого интерфейса.
- Содержимое между тегами — запасной вариант для браузеров, которые не поддерживают элемент. Туда ставят ссылку на скачивание.
- `type` у `<source>` — MIME-тип, чтобы браузер пропустил файл, который не может декодировать.
- Набор кодеков у браузеров не один. Предложите больше одного `<source>`.
- `<video>` может воспроизвести и звук. Для одного звука понятнее `<audio>`.

**When to use:**

- Медиа, которое должно играть на странице. Файл, который пользователь только сохраняет, — это `<a href>` с `download`, а не плеер.

**Examples:**

```html
<video controls width="250">
  <source src="/shared-assets/videos/flower.webm" type="video/webm" />
  <source src="/shared-assets/videos/flower.mp4" type="video/mp4" />
  <a href="/shared-assets/videos/flower.mp4">Download the MP4 video.</a>
</video>

<audio controls src="/shared-assets/audio/t-rex-roar.mp3"></audio>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>Что такое the attributes of the video and audio elements?</summary>

### Что такое the attributes of the video and audio elements?

**Definition:**

Оба элемента принимают `src`, `controls`, `autoplay`, `loop`, `muted`, `preload`, `crossorigin` и `loading`. У `<video>` ещё есть `width`, `height`, `poster` и `playsinline`.

**How it works:**

#### Общие

- Эти логические атрибуты включены, когда атрибут присутствует. `autoplay="false"` всё равно запускает воспроизведение, а `muted="false"` всё равно глушит звук. Чтобы выключить, атрибут удаляют.
- `preload` — подсказка: `none`, `metadata` или `auto` (пустое значение значит `auto`). Браузеры расходятся в значении по умолчанию; спецификация советует `metadata`. `autoplay` важнее `preload`.
- `crossorigin` бывает `anonymous` или `use-credentials`.
- `loading` бывает `eager` (по умолчанию) или `lazy`.
- `controlslist` может быть `nodownload`, `nofullscreen` или `noremoteplayback`.

#### Видео

- `width` и `height` — CSS-пиксели, не проценты.
- `poster` — изображение, пока нет первого кадра.
- `playsinline` просит воспроизведение внутри элемента, а не принудительный полноэкранный режим. Отсутствие атрибута не гарантирует полный экран.
- `disablepictureinpicture` блокирует картинку в картинке.

#### Аудио

- Нет `poster`, `width` и `height`. Ленивому аудио всё равно нужны `controls`, иначе оно невидимо и не загрузится.

**When to use:**

- `controls` почти у каждого плеера. `autoplay` только когда пользователь сам его включил: браузеры блокируют автозапуск со звуком.

**Examples:**

```html
<video controls width="250" poster="/images/sample.gif" preload="metadata">
  <source src="sample.mp4" type="video/mp4" />
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>Как you provide subtitles or CAPtions for video content in HTML?</summary>

### Как you provide subtitles or CAPtions for video content in HTML?

**Definition:**

Вложите `<track>` в `<video>`. `src` — файл WebVTT (`.vtt`), а `kind` говорит, что это за дорожка.

**How it works:**

#### Виды

- `subtitles` (значение по умолчанию) расшифровывает или переводит речь, когда звук есть, но непонятен. Для этого вида обязателен `srclang`.
- `captions` включают речь плюс звуковые эффекты и музыкальные подсказки, когда звука нет или его плохо слышно.
- `descriptions` кратко описывают картинку и предназначены для озвучивания. `chapters` — названия для навигации. `metadata` — для скриптов и не показывается.
- `default` включает эту дорожку, если настройки пользователя не выбрали другую. Только у одного `<track>` на медиаэлемент он может быть.
- `label` — имя в списке дорожек браузера. `src` должен быть того же origin, если у медиаэлемента нет `crossorigin`.
- Две дорожки не могут совпадать по `kind`, `srclang` и `label`.
- Страница MDN про audio говорит, что `<audio>` не поддерживает WebVTT так, как `<video>`. Если нужна `<track>`, воспроизведите запись в `<video>` или дайте транскрипт.

**When to use:**

- Captions, когда звук может быть выключен. Subtitles, когда аудитории нужен другой язык. Транскрипт тоже, для тех, кому нужно читать в своём темпе.

**Examples:**

```html
<video controls src="/shared-assets/videos/friday.mp4">
  <track
    default
    kind="captions"
    srclang="en"
    label="English"
    src="/shared-assets/misc/friday.vtt"
  />
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/track
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>В чём разница между embedding и linking media?</summary>

### В чём разница между embedding и linking media?

**Definition:**

Встраивание ставит плеер на страницу через `<video>` или `<audio>`. Ссылка направляет `<a href>` на файл, и браузер переходит к нему или предлагает скачать.

**How it works:**

#### На странице

- Встроенный плеер может показать `controls`, несколько типов `<source>` и `<track>`.
- Разметка внутри `<video>` или `<audio>` — запасной вариант, а не вторая копия ролика. Её показывает браузер, который не поддерживает элемент.

#### Ссылка

- `<a href="report.mp4">` ведёт к ресурсу. `download` просит браузер сохранить файл того же origin.
- В примерах video и audio эту ссылку кладут в запасное содержимое, чтобы файл оставался доступен.

**When to use:**

- Встраивайте, когда человек должен смотреть или слушать, не уходя со страницы. Ссылайтесь, когда файл скачивают или плеер неуместен.

**Examples:**

```html
<video controls>
  <source src="myVideo.webm" type="video/webm" />
  <source src="myVideo.mp4" type="video/mp4" />
  <a href="myVideo.mp4">Download the MP4</a>
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

### Responsive Design and Media Queries

<details>
<summary>Что такое viewport and how can you set it?</summary>

### Что такое viewport and how can you set it?

**Definition:**

Viewport — это область окна, где видна страница. Задайте его через `<meta name="viewport" content="width=device-width">`.

**How it works:**

#### Элемент meta

- `content` — список пар `key=value` через запятую. `width=device-width` ставит ширину раскладки равной экрану устройства в CSS-пикселях.
- Узкие устройства иначе часто раскладывают страницу в виртуальном viewport около 980px и сжимают её. Медиазапросы на 480px или 640px тогда никогда не срабатывают.
- `initial-scale` — коэффициент масштаба от `0.0` до `10.0`. Рекомендуемый фрагмент MDN опускает `initial-scale=1`. Добавьте его, когда вылезающее содержимое заставляет браузер сжать страницу, а вам нужен старт со 100% масштаба.
- `user-scalable=no` запрещает масштаб. Не используйте его: людям со слабым зрением нужно увеличивать страницу, а WCAG просит как минимум двукратный масштаб.

**When to use:**

- На каждом документе, который должен следовать ширине устройства, то есть на обычной странице.

**Examples:**

```html
<meta name="viewport" content="width=device-width" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>Можете ли вы describe the use of media queries in HTML?</summary>

### Можете ли вы describe the use of media queries in HTML?

**Definition:**

Медиазапрос проверяет условия вроде ширины viewport и применяет CSS только когда они совпали. В HTML атрибут `media` у `<source>` делает такую же проверку, чтобы выбрать изображение.

**How it works:**

#### В CSS

- `@media screen and (width >= 80rem)` применяет правила внутри только для экранного медиа при этой ширине или шире.
- Ширина, на которой меняется раскладка, — это breakpoint. Частый приём: сначала одна колонка, дополнительные колонки — когда окно уже достаточно широкое (mobile first).
- В breakpoint лучше относительные единицы, а не ширина в пикселях одного устройства.
- Гибкие сетки и значения `min`/`max` могут подстраиваться без медиазапроса. Для адаптивного дизайна медиазапросы не обязательны.

#### В HTML

- У `<source>` внутри `<picture>` `media` — условие. Если оно ложно, браузер пропускает этот source и берёт следующий.
- Тот же атрибут `media` может выбрать `<source>` видео или аудио для разных устройств.

**When to use:**

- Когда раскладка или файл должны смениться на какой-то ширине, а не чтобы собирать отдельный сайт под каждую модель телефона.

**Examples:**

```html
<picture>
  <source srcset="mdn-logo-wide.png" media="(width >= 600px)" />
  <img src="mdn-logo-narrow.png" alt="MDN" />
</picture>
```

```css
@media screen and (width >= 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
</details>

<details>
<summary>Как you create responsive images with different resolutions for different devices?</summary>

### Как you create responsive images with different resolutions for different devices?

**Definition:**

Предложите несколько файлов через `srcset` и скажите браузеру ширину слота через `sizes`, либо используйте `<picture>`, когда должны смениться кадрирование или формат.

**How it works:**

#### Разрешение и ширина

- У `<img>` `srcset` — список через запятую. Каждый кандидат — URL плюс дескриптор ширины (`480w`) или плотности (`2x`). Не смешивайте `w` и `x` в одном списке. Пропущенная плотность значит `1x`. Повторять дескрипторы нельзя.
- `sizes` используют с дескрипторами ширины. Это список условий и ширин слота. Браузер умножает выбранный слот на плотность пикселей устройства и берёт ближайший кандидат. Без `sizes` слот по умолчанию — `100vw`.
- Для экранов с высокой плотностью подходит `srcset` на `<img>`. Браузер всё ещё может выбрать файл меньше в режиме экономии трафика. Для одной только плотности `<picture>` и условие `media` не нужны.

#### Кадрирование и форматы

- `<picture>` содержит элементы `<source>` и один запасной `<img>`. `media` меняет кадр. `type` пропускает формат, который браузер не декодирует, например AVIF перед WebP перед JPEG.
- `max-width: 100%` у `img`, `picture` и `video` держит медиа внутри колонки. Один огромный файл, уменьшенный на экране, всё равно стоит трафика огромного файла.

**When to use:**

- `srcset`, когда картинка та же и меняется только плотность пикселей. `<picture>`, когда узкому экрану нужен другой кадр или новому формату нужен запасной вариант.

**Examples:**

```html
<img
  srcset="logo.png, logo-2x.png 2x"
  src="logo.png"
  width="320"
  height="320"
  alt="MDN Web Docs logo"
/>

<picture>
  <source srcset="photo.avif" type="image/avif" />
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="photo" />
</picture>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

<details>
<summary>Что такое responsive web design?</summary>

### Что такое responsive web design?

**Definition:**

Адаптивный веб-дизайн — это подход, а не одна технология: одна и та же страница подстраивает раскладку, медиа и производительность под экран, на котором её открыли.

**How it works:**

#### Идея

- HTML без CSS уже переносит текст, когда меняется окно. Фиксированная ширина страницы тогда даёт полосы прокрутки на узком экране и пустое место на широком.
- Термин ввёл Ethan Marcotte в 2010 году: текучие сетки, текучие изображения и медиазапросы.
- Текучее изображение использует `max-width: 100%`: оно сжимается вместе с колонкой и не растёт больше своего собственного размера.
- Flexbox и Grid адаптивны по умолчанию: элементы делят свободное место. Медиазапросы меняют раскладку на breakpoint, лучше в относительных единицах, и обычно сначала для узкого экрана.
- Отдавайте подходящее изображение, а не один файл для рабочего стола на все устройства. Трафик и батарея на телефонах всё ещё важны.

**When to use:**

- Любая страница, которую откроют больше чем на одном размере экрана. Не поставляйте отдельное дерево разметки на устройство, если тот же документ может подстроиться.

**Examples:**

```css
img,
picture,
video {
  max-width: 100%;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

<details>
<summary>Как flexbox and grids help in creating responsive layouts?</summary>

### Как flexbox and grids help in creating responsive layouts?

**Definition:**

Flexbox и Grid делят свободное место между элементами, поэтому раскладка гнётся вместе с viewport. Медиазапрос может включить это деление, когда окно уже достаточно широкое.

**How it works:**

#### Flexbox

- `display: flex` делает flex-контейнер. `flex: 1` у элементов даёт каждому равную долю свободного места.
- `flex-grow` и `flex-shrink` говорят, как элемент ведёт себя, когда места больше или меньше, чем нужно.
- Один приём: по умолчанию одна колонка, а `display: flex` включают внутри `@media screen and (width >= 600px)`.

#### Grid

- `display: grid` с `grid-template-columns: 1fr 1fr 1fr` делает три дорожки, каждая берёт одну часть свободного места. `fr` — доля этого места.
- Тот же breakpoint может включить `grid-template-columns: 1fr 2fr` и `column-gap`.
- Breakpoint не нужен на каждую ширину устройства. Добавляйте его, когда содержимое начинает выглядеть плохо.

**When to use:**

- Flexbox для ряда или колонки элементов. Grid, когда важны и строки, и столбцы. Оба заменяют жёсткую рамку в пикселях.

**Examples:**

```css
@media screen and (width >= 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

### Accessibility

<details>
<summary>Что такое accessibility and why is it important in web development?</summary>

### Что такое accessibility and why is it important in web development?

**Definition:**

Доступность — это делать сайт пригодным для как можно большего числа людей. Она рассчитана на людей с инвалидностью и также помогает тем, кто на телефоне и на медленной сети.

**How it works:**

#### Кого это касается

- Зрение: слепота, слабое зрение и неразличение цветов. Инструменты — масштаб и программы чтения с экрана, например NVDA, VoiceOver, Narrator и TalkBack.
- Слух: субтитры у видео и транскрипты у аудио. Текст должен оставаться достаточно простым.
- Движение: элементы управления должны работать с клавиатуры, а не только с указателя.
- Познание: простой язык, устойчивая раскладка, мало отвлечений и формы с ясными ошибками.

#### Почему это часть работы

- Семантический HTML помогает вспомогательным технологиям и также помогает поисковым системам.
- Во многих странах недоступный публичный сайт — юридическая проблема. WCAG группирует критерии как воспринимаемый, управляемый, понятный и надёжный.
- Браузеры отдают дерево доступности API доступности операционной системы. Стили и скрипт — не это дерево. Смысл несёт элемент.

**When to use:**

- С первой раскладки, а не заплаткой в конце. Поздняя переделка — дорогой случай.

**Examples:**

```html
<button type="submit">Save</button>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
</details>

<details>
<summary>Как you make a website accessible?</summary>

### Как you make a website accessible?

**Definition:**

Используйте HTML-элемент, у которого уже есть нужный смысл, назовите каждый элемент управления, сохраните путь с клавиатуры и проверяйте по WCAG, пока строите.

**How it works:**

#### В разметке

- Заголовки, ориентиры (`header`, `nav`, `main`, `footer`), списки и кнопки говорят дереву доступности, что это за область. `<div>` с обработчиком щелчка этого не говорит.
- У каждого изображения есть `alt`. У каждого элемента формы есть `<label>`. У видео есть `<track>`, у аудио — транскрипт.
- Не отключайте масштаб через `user-scalable=no`. Текст ссылки называет назначение.
- Опубликуйте заявление о доступности и исправляйте проблемы, о которых сообщают.

#### Проверка

- Автоматические проверки ловят пропущенный `alt` и слабый текст ссылки. Они не доказывают, что выбор даты работает с программой чтения с экрана.
- Пройдите страницу только с клавиатуры: Tab по элементам и активация через Enter или Space.
- «Стопроцентная доступность» — не финиш. У диаграммы всё равно нужна таблица данных, которую можно прочитать.

**When to use:**

- На каждой странице. Заложите субтитры и альтернативу для своих виджетов в начале.

**Examples:**

```html
<label for="email">Email</label>
<input id="email" name="email" type="email" autocomplete="email" required />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/label
</details>

<details>
<summary>Что такое ARIA roles and how do you use them?</summary>

### Что такое ARIA roles and how do you use them?

**Definition:**

Роль ARIA называет виджет или ориентир для вспомогательных технологий. Ставьте её на элемент только когда в HTML нет элемента, который уже это означает.

**How it works:**

#### Сначала элемент

- У многих элементов роль уже есть: `<nav>` — это `navigation`, `<main>` — это `main`, заголовок — это заголовок. `role="navigation"` на `<nav>` ничего не улучшает.
- Роли структуры документа, которые дублируют HTML, например `role="list"` на списке или `role="button"` там, где подошёл бы `<button>`, лучше не ставить.
- Роли без HTML-аналога включают `tab`, `tabpanel`, `toolbar` и `tooltip`. У `tablist` должны быть дети `tab`. `tabpanel` без вкладок — не панель вкладок, и роль тогда вредит.
- ARIA только добавляет сведения в дерево доступности. Она не меняет DOM, клавиатуру и мышь. Если вы строите набор вкладок, клавиши и состояния (`aria-selected`, `aria-controls`) всё равно пишете скриптом.
- Ролей-ориентиров (`banner`, `complementary`, `contentinfo`, `main`, `navigation`, `search`) должно быть мало. Страница, полная ими, — шум в программе чтения с экрана.

**When to use:**

- Свой виджет, который HTML не выражает. Для кнопки, ссылки, заголовка или блока навигации берите HTML-элемент и обходитесь без роли.

**Examples:**

```html
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    Details
  </button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">…</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles
- https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Guides/Techniques
</details>

<details>
<summary>Объясните how to use the tabindex attribute.</summary>

### Объясните how to use the tabindex attribute.

**Definition:**

`tabindex` решает, попадает ли элемент в порядок Tab. Используйте `0` или `-1`. Положительное число не используйте.

**How it works:**

#### Три случая

- `tabindex="-1"` убирает элемент из порядка Tab. На него всё ещё можно поставить фокус через `focus()` или щелчок. Это подходит диалогу, который должен получить фокус при открытии, или сообщению об ошибке, которое должно получить фокус при неудачной отправке.
- `tabindex="0"` ставит элемент в порядок Tab на его место в исходнике, после любых положительных значений.
- Положительное значение, до 32767, посещается раньше `0`, по числу. MDN говорит его не использовать: порядок перестаёт совпадать со страницей, и пользователи клавиатуры теряются. Упорядочивайте элементы в документе.
- Любое допустимое целое делает элемент фокусируемым скриптом или щелчком. Только знак решает, доходит ли до него Tab.
- Ссылки с `href` и элементы форм уже в порядке Tab. Добавляйте им `tabindex`, только чтобы это изменить, обычно через `-1`.
- Не ставьте `tabindex="0"` на `<div>` и не называйте его кнопкой. Такой div не кнопка в дереве доступности. Используйте `<button>`.

**When to use:**

- `-1`, чтобы перенести фокус на то, что обычно не является остановкой Tab. `0` только для своего виджета, который правда интерактивен и у которого нет нативного элемента.

**Examples:**

```html
<div tabindex="0">Tabbable due to tabindex.</div>
<div>Not tabbable: no tabindex.</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/tabindex
</details>

<details>
<summary>Как you ensure your images are accessible?</summary>

### Как you ensure your images are accessible?

**Definition:**

Дайте каждому `<img>` атрибут `alt`. Опишите содержательное изображение. Используйте `alt=""`, когда изображение декоративное.

**How it works:**

#### Текст

- `alt` — это то, что говорит программа чтения с экрана, и то, чем заменяют изображение, когда оно не загрузилось или пользователь отключил картинки.
- Пустой `alt` значит «пропусти это». Пропуск `alt` оставляет читателя без замены, часто с именем файла.
- Текст заменяет изображение в контексте. Полезное значение называет то, что показано, как в примере со слоном на странице figure.
- `<figcaption>` называет фигуру. Он не заменяет `alt` у `<img>`.
- Текст, который является единственной копией сведения, не должен жить только внутри растра. Положите этот текст в HTML.
- У каждой `<area href>` в карте изображения свой `alt`.

**When to use:**

- На каждом `<img>`, включая значки внутри ссылок. У ссылки-значка с `alt=""` пустое имя.

**Examples:**

```html
<img
  src="/shared-assets/images/examples/elephant.jpg"
  alt="Elephant at sunset"
/>

<img src="divider.png" alt="" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/figure
</details>

### Linking and Navigation Techniques

<details>
<summary>Как you make a naviGAtion bar in HTML?</summary>

### Как you make a naviGAtion bar in HTML?

**Definition:**

Главные ссылки кладут в `<nav>`, обычно как `<ul>` из элементов `<a>`. CSS превращает этот список в полосу.

**How it works:**

#### Что туда входит

- `<nav>` — крупный блок навигации: меню сайта, оглавление или указатель. Неявная роль — `navigation`.
- Не каждая ссылка страницы попадает в него. Абзац, в котором случайно есть ссылка, — не панель навигации.
- На странице может быть больше одного `<nav>`. Назовите каждый через `aria-labelledby` или `aria-label`, когда программа чтения иначе услышит несколько безымянных областей «navigation».
- Список — это структура. `list-style`, flex или grid — это CSS, а не дополнительные HTML-элементы.

**When to use:**

- Основной способ перемещаться по сайту или документу. След родительских страниц — это хлебные крошки, тоже `<nav>`, со своей подписью.

**Examples:**

```html
<nav class="menu" aria-label="Site">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
</details>

<details>
<summary>Каково значение breadcrumb navigation?</summary>

### Каково значение breadcrumb navigation?

**Definition:**

Хлебные крошки показывают, где текущая страница сидит в иерархии сайта. Текущая страница — последняя, и она не ссылка.

**How it works:**

#### След

- MDN размечает это как `<nav aria-label="Breadcrumb">` с `<ol>`. Каждый предок — `<a>`. Текущая страница — `<span aria-current="page">`.
- Порядок — путь от верха сайта вниз к этой странице, поэтому список нумерованный, а не мешок ссылок.
- `aria-label` говорит вспомогательным технологиям, какая это навигация. `aria-current="page"` помечает страницу, на которой вы находитесь.
- Разделители вроде «→» — это CSS. Если создать их через `content`, программы чтения могут их произнести. Предпочтительнее `::after`, чтобы разделитель объявили после ссылки, или `<img alt="">`.

**When to use:**

- Сайт с вложенными разделами, где читателю нужны родители текущей страницы. Это не замена главного меню.

**Examples:**

```html
<nav aria-label="Breadcrumb" class="breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/category">Category</a></li>
    <li><span aria-current="page">Product</span></li>
  </ol>
</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/How_to/Layout_cookbook/Breadcrumb_navigation
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
</details>

<details>
<summary>Как you create a dropdown menu in HTML?</summary>

### Как you create a dropdown menu in HTML?

**Definition:**

Используйте `<details>` и `<summary>`. Summary — подпись в закрытом виде. Остальное содержимое details скрыто, пока пользователь его не откроет.

**How it works:**

#### Раскрытие

- В закрытом виде видны только summary и маркер. Щелчок или Space при фокусе на виджете переключает его.
- `open` — логический атрибут. Наличие атрибута показывает содержимое. `open="false"` всё равно показывает его. Чтобы закрыть, атрибут удаляют.
- Одинаковый `name` у нескольких `<details>` держит открытым только один. Открытие одного закрывает остальные. Если у нескольких в группе есть `open`, открытым остаётся только первый в порядке исходника.
- Ссылки меню — обычные `<a>` внутри details. Неявная роль виджета — `group`.
- Встроенной анимации между закрытым и открытым нет.

**When to use:**

- Меню или блок дополнительных ссылок, который должен оставаться закрытым, пока его не попросили. Полоса на весь сайт, которая всегда видна, в `<details>` не нуждается.

**Examples:**

```html
<details>
  <summary>Products</summary>
  <ul>
    <li><a href="/bikes">Bikes</a></li>
    <li><a href="/parts">Parts</a></li>
  </ul>
</details>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details
</details>

<details>
<summary>Объясните использование атрибута target в ссылке.</summary>

### Объясните использование атрибута target в ссылке.

**Definition:**

`target` называет контекст просмотра, который загрузит ссылку: та же вкладка, новая, родитель или самый верх.

**How it works:**

#### Ключевые слова

- `_self` — значение по умолчанию: текущий контекст.
- `_blank` — обычно новая вкладка. Пользователь может настроить новое окно. Атрибут уже ведёт себя как `rel="noopener"`: у нового контекста `window.opener` равен `null`.
- `_parent` — родительский контекст, или `_self`, если родителя нет.
- `_top` — самый верхний предок, или `_self`, если предков нет.
- Любое другое значение — именованный контекст. Если контекста с таким именем нет, браузер открывает новый.
- В тексте ссылки скажите, когда назначение уходит из текущей вкладки.

**When to use:**

- `_blank` для справки, которую читатель должен держать рядом с текущей страницей. Для обычной навигации `target` не ставьте.

**Examples:**

```html
<a target="_blank" href="https://www.wikipedia.org">
  Wikipedia (opens in new tab)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>Объясните the use of the target attribute in a link.</summary>

### Объясните the use of the target attribute in a link.

**Definition:**

Меню, которое раскрывается вниз, — это элемент `<details>`. Закрытый, он высотой только с `<summary>`. Открытый, он растягивается и показывает меню.

**How it works:**

#### Открыто и закрыто

- Пользователь открывает его щелчком или клавишей Space. Событие `toggle` приходит на `<details>` после смены состояния.
- `open` отражает это состояние. Его можно поставить или снять из скрипта. `open="false"` его не закрывает.
- Несколько меню могут делить `name`, тогда открытие одного закрывает остальные: это аккордеон.
- MDN отмечает, что встроенной анимации смены открыто/закрыто нет. CSS может стилизовать `details[open]` и summary, но сам элемент перескакивает между двумя высотами.
- Ссылки меню кладут списком внутрь details, под summary.

**When to use:**

- Панель, которая должна занимать вертикальное место только пока открыта. Не выдумывайте второй виджет меню, когда `<details>` уже это делает.

**Examples:**

```html
<details>
  <summary>System Requirements</summary>
  <p>
    Requires a computer running an operating system. The computer must have some
    memory and ideally some kind of long-term storage.
  </p>
</details>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details
</details>

### Advanced HTML Technologies

<details>
<summary>Как you create a SLIdedown menu?</summary>

### Как you create a SLIdedown menu?

**Definition:**

Web Components — это три возможности платформы для переиспользуемого элемента, у которого разметка, стили и поведение остаются изолированными: custom elements, shadow DOM и HTML-шаблоны.

**How it works:**

#### Три части

- Custom element — это класс, зарегистрированный через `customElements.define(name, Class)`, а затем используемый как любой другой тег.
- Shadow DOM — дерево, прикреплённое к этому элементу. CSS страницы и `querySelectorAll` внутрь не видят, а его стили наружу не вытекают.
- `<template>` хранит разметку, которая не рисуется, пока скрипт её не клонирует, часто в shadow root. `<slot>` — отверстие, где появляются собственные дети страницы.

#### Жизненный цикл

- `connectedCallback` вызывается, когда элемент подключён к документу. `disconnectedCallback` — когда его удалили. `adoptedCallback` — когда он переехал в другой документ. `attributeChangedCallback` — когда изменился наблюдаемый атрибут.

**When to use:**

- Элемент управления, который вы вставите на много страниц и чьи классы или id не должны столкнуться со страницей. Разовый виджет может остаться обычным HTML.

**Examples:**

```html
<filled-circle color="blue"></filled-circle>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
</details>

<details>
<summary>Что такое Shadow DOM и как его использовать?</summary>

### Что такое Shadow DOM и как его использовать?

**Definition:**

Shadow DOM — это дерево DOM, прикреплённое к элементу-хозяину. Страница хозяина не может случайно стилизовать или найти это дерево, а стили дерева остаются внутри.

**How it works:**

#### Как прикрепить

- Хозяин — обычный элемент. Shadow root — корень скрытого дерева. Граница shadow — место, где скрытое дерево встречается со страницей.
- `element.attachShadow({ mode: "open" })` прикрепляет его. При `open` `element.shadowRoot` возвращает этот корень. При `closed` `shadowRoot` равен `null`. Закрытый режим — сигнал не лезть внутрь, а не граница безопасности.
- То же в HTML: `<template shadowrootmode="open">` внутри хозяина. Парсер заменяет этот template на shadow root.
- `document.querySelectorAll` не находит узлы в shadow-дереве. Правило страницы вроде `span { color: blue }` их не красит. `<style>` внутри shadow-дерева не красит страницу.
- `lang` и `dir` у shadow root наследуются от хозяина. Содержимое, назначенное в `<slot>`, по-прежнему наследует от своего родителя вне shadow-дерева.

**When to use:**

- Внутренности custom element или любой виджет, чьи кнопки и id не должны сталкиваться со страницей. Браузеры уже так прячут элементы управления внутри `<video>`.

**Examples:**

```html
<div id="host">
  <template shadowrootmode="open">
    <span>I'm in the shadow DOM</span>
  </template>
</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
</details>

<details>
<summary>Что такое Web Components and how are they used?</summary>

### Что такое Web Components and how are they used?

**Definition:**

Расширьте `HTMLElement`, вызовите `super()` первым в конструкторе и зарегистрируйте класс через `customElements.define`.

**How it works:**

#### Регистрация

- `customElements.define("filled-circle", FilledCircle)` заставляет `<filled-circle>` строить этот класс. Имена в примерах MDN содержат дефис.
- Внутренности собирают в `connectedCallback`, часто через `this.attachShadow({ mode: "open" })`.
- Атрибуты читают через `getAttribute`. Затем элемент используют в разметке как любой другой тег.
- Customized built-in расширяет конкретный элемент, например абзац, и применяется атрибутом `is` или через `{ is: "..." }` в `createElement`.
- `:defined` совпадает с элементом, чьё определение уже зарегистрировано.

**When to use:**

- Когда одно и то же поведение и разметка должны быть одним тегом. Не регистрируйте имя, которое столкнётся с другим компонентом; для этого есть scoped `CustomElementRegistry`.

**Examples:**

```html
<filled-circle color="blue"></filled-circle>
```

```js
class FilledCircle extends HTMLElement {
  constructor() {
    super();
  }
  connectedCallback() {
    const shadow = this.attachShadow({ mode: "open" });
    shadow.textContent = this.getAttribute("color");
  }
}
customElements.define("filled-circle", FilledCircle);
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
- https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM
</details>

<details>
<summary>Что такое SHAdow DOM and how do you use it?</summary>

### Что такое SHAdow DOM and how do you use it?

**Definition:**

`<template>` хранит фрагмент HTML, который не рисуется. Скрипт клонирует `template.content`, когда нужна копия, а `shadowrootmode` сразу превращает template в shadow root.

**How it works:**

#### Пока не на странице

- Вложенная разметка не является ребёнком template. `childNodes` пуст. Фрагмент живёт в свойстве `content` только для чтения, это `DocumentFragment`.
- Клонируйте через `document.importNode(template.content, true)`, чтобы custom elements внутри строились реестром текущего документа. `appendChild` на самом элементе template не обновляет `content`.
- `<slot name="person-name">` в shadow-дереве заполняется ребёнком светлого DOM с `slot="person-name"`. Запасной текст внутри slot показывается, когда ничего не назначено.
- `<template shadowrootmode="open">` (или `closed`) — это декларативный shadow DOM. Парсер прикрепляет shadow root к родителю и не оставляет там элемент template. Только первый такой template у родителя становится shadow root.
- `shadowrootclonable`, `shadowrootdelegatesfocus` и `shadowrootserializable` задают эти флаги shadow root. Старого атрибута `shadowroot` больше нет; используйте `shadowrootmode`.

**When to use:**

- Разметка, которую вы будете штамповать много раз, или внутренняя структура custom element. Не используйте его для содержимого, которое должно быть видно без скрипта, если это не декларативный shadow DOM.

**Examples:**

```html
<template id="custom-paragraph">
  <p><slot></slot></p>
</template>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
</details>

<details>
<summary>Как you create a custom HTML element?</summary>

### Как you create a custom HTML element?

**Definition:**

Браузер открывает односторонний поток через `new EventSource(url)`. Сервер отвечает `Content-Type: text/event-stream` и продолжает слать текстовые события в UTF-8.

**How it works:**

#### Клиент

- Сообщения без поля `event` приходят как `message`. Читайте `event.data` в `onmessage`.
- Именованное поле `event` — это своё событие. Слушайте через `addEventListener("ping", ...)`.
- Соединение перезапускается, если оборвалось. `retry` в потоке — ожидание в миллисекундах. `close()` его останавливает.
- Кросс-оригинному URL нужен `{ withCredentials: true }`, когда надо послать cookies. `onerror` вызывается при сбое сети или доступа.
- Без HTTP/2 браузеры ограничивают открытые соединения SSE примерно шестью на origin. HTTP/2 договаривается о пределе потоков (часто 100).

#### Поток

- События разделены пустой строкой. Поля — `event`, `data`, `id` и `retry`. Строка, которая начинается с `:`, — комментарий и может держать соединение живым.
- Несколько строк `data:` склеиваются переводами строк.

**When to use:**

- Обновления от сервера к браузеру, например живой список. Браузер не может слать события обратно по этому соединению. Для этого нужен другой запрос или WebSocket, если важны оба направления.

**Examples:**

```html
<ul id="list"></ul>
<script>
  const evtSource = new EventSource("sse-demo.php");
  evtSource.onmessage = (event) => {
    const item = document.createElement("li");
    item.textContent = event.data;
    document.getElementById("list").appendChild(item);
  };
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events
</details>

### SEO and HTML

<details>
<summary>Объясните HTML templates and their use cases.</summary>

### Объясните HTML templates and their use cases.

**Definition:**

Дайте краулерам документ, который можно прочитать: настоящий title, описание, семантические элементы, текст (не только картинки) и ссылки, по которым им можно идти.

**How it works:**

#### Чем пользуются краулеры

- Поисковые системы обходят сеть, переходя по ссылкам, и индексируют найденное содержимое. Подробности ранжирования остаются коммерческой тайной. Техническую сторону MDN называет так: семантический HTML, чтобы краулеры встретили содержимое, которое вы хотите проиндексировать.
- `<title>` — заголовок документа. Его предлагают для закладок и показывают в результатах поиска. Видимый заголовок страницы — `<h1>`, и это не тот же элемент.
- `<meta name="description" content="...">` — короткое резюме. Его могут показать на странице результатов, и оно помогает странице попасть в релевантный поиск.
- `<meta name="keywords">` игнорируют. Его забивали спамом.
- `lang` на `<html>` позволяет индексировать страницу для нужного языка.

**When to use:**

- На каждой публичной странице. Пишите слова, которыми пользуются посетители. Популярность по-прежнему зависит от ссылок с других сайтов, и один HTML этого не создаёт.

**Examples:**

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
  <meta
    name="description"
    content="A short summary of what this page is about."
  />
</head>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>Как you use server-sent events?</summary>

### Как you use server-sent events?

**Definition:**

Семантический HTML называет, чем является область (`article`, `nav`, `h1`, `p`), вместо того чтобы оборачивать всё в `div`. Краулеры используют эту структуру и текст, чтобы понять тему.

**How it works:**

#### Техническое SEO

- Технический класс SEO у MDN такой: разметьте содержимое семантическим HTML и дайте краулерам найти только то, что вы хотите проиндексировать.
- Текст всё ещё важен. Одни изображения не сообщают краулеру тему. Положите ту же информацию в текст.
- Те же элементы помогают вспомогательным технологиям. MDN отмечает, что эта работа по доступности также делает сайт легче находимым.
- Обёртка для стилей без смысла остаётся `<div>`. Краулеру она раздел не описывает.

**When to use:**

- Всякий раз, когда есть более конкретный элемент. Семантика — HTML-часть SEO. Она не заменяет полезный title или ссылки с других сайтов.

**Examples:**

```html
<main>
  <article>
    <h1>Beetles</h1>
    <p>A beetle is an insect with hardened forewings.</p>
  </article>
</main>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
</details>

<details>
<summary>Как you optimize HTML for search engines?</summary>

### Как you optimize HTML for search engines?

**Definition:**

`<h1>`–`<h6>` — шесть уровней заголовков. `<h1>` — самый высокий. Они дают оглавление страницы людям, программам чтения и краулерам.

**How it works:**

#### Оглавление

- Используйте один `<h1>`, который описывает содержимое страницы. Стандарт HTML позволяет больше одного, если они не вложены, но MDN говорит, что это не лучшая практика. Несколько вложенных `<h1>` внутри секционных элементов не соответствуют стандарту.
- Не пропускайте уровень вниз: `<h1>`, затем `<h2>`, затем `<h3>`. Пропуск путает того, кто прыгает с заголовка на заголовок. Закрытие подраздела может вернуться на более высокий уровень.
- Не используйте заголовок, чтобы увеличить текст. Для этого `font-size`.
- Агенты пользователя могут собрать оглавление из заголовков. Программы чтения перечисляют их, чтобы человек увидел иерархию.
- `<title>` — метаданные документа. `<h1>` — заголовок содержимого на странице.

**When to use:**

- Каждый отдельный раздел, который читатель или краулер должен уметь назвать. Страница без заголовков — стена текста.

**Examples:**

```html
<h1>Beetles</h1>
<h2>External morphology</h2>
<h3>Head</h3>
<h4>Mouthparts</h4>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>Что такое semantic HTML and how does it relate to SEO?</summary>

### Что такое semantic HTML and how does it relate to SEO?

**Definition:**

Структурированные данные — это стандартное описание страницы, например имя и автор рецепта, чтобы поисковая система могла классифицировать это содержимое и, возможно, показать расширенный результат.

**How it works:**

#### Форматы

- Google Search читает три формата. JSON-LD — тот, который он рекомендует: `script` с `type="application/ld+json"` в head или body. Microdata и RDFa кладут такие же факты в атрибуты HTML.
- Большая часть этой разметки использует словарь schema.org. Для поведения Google Search ориентир — собственные руководства Google по функциям. У schema.org есть дополнительные свойства, которые могут пригодиться другим потребителям.
- Данные должны описывать содержимое, которое пользователь видит на этой странице. Не создавайте пустую страницу только ради разметки и не размечайте факты, которых не видно.
- Тип имеет право на расширенный результат, только когда его обязательные свойства присутствуют и точны. Меньше полных свойств лучше длинного списка сломанных.
- Разметка `data-vocabulary.org` больше не годится для расширенных результатов Google. Проверяйте разметку в Rich Results Test.

**When to use:**

- Страница того типа, который документирует Google, например рецепт, когда обязательные поля можно заполнить текстом, уже лежащим на странице.

**Examples:**

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Recipe",
    "name": "Recipe title",
    "author": "Recipe author"
  }
</script>
```

**Sources:**

- https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
</details>

<details>
<summary>Объясните the significance of heading tags for SEO.</summary>

### Объясните the significance of heading tags for SEO.

**Definition:**

Задайте кодировку, язык, title и description; очертите страницу заголовками без пропуска уровней; пишите содержимое текстом HTML; структурированные данные добавляйте только для того, что страница правда показывает.

**How it works:**

#### Крепкий документ

- `<meta charset="utf-8">`, чтобы текст не прочитали неправильно.
- `<html lang="...">`, чтобы языковые результаты и программы чтения получили язык.
- Уникальный `<title>` и `<meta name="description">`, написанные для людей. `name="keywords"` не ставьте.
- Один `<h1>`, затем `<h2>` и ниже без пропуска уровней. Семантические области (`main`, `article`, `nav`) вокруг этого текста.
- `alt` у изображений, потому что краулер не берёт тему из одного растра.
- JSON-LD, который повторяет видимые факты, с каждым обязательным свойством. Проверьте его.

**When to use:**

- Как обычная форма публичной страницы. Ссылки с других сайтов — это по-прежнему та сторона SEO, которую MDN называет популярностью.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Beetles</title>
    <meta
      name="description"
      content="What a beetle is, and how its body is divided."
    />
  </head>
  <body>
    <h1>Beetles</h1>
    <p>A beetle is an insect with hardened forewings.</p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
- https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
</details>

### HTML APIs

<details>
<summary>Как structured data and schemas enhance SEO?</summary>

### Как structured data and schemas enhance SEO?

**Definition:**

`navigator.geolocation` читает местоположение устройства после разрешения пользователя. Это доступно только в безопасном контексте (HTTPS).

**How it works:**

#### Сначала разрешение

- Вызов `getCurrentPosition()` или `watchPosition()` показывает запрос, если разрешение ещё не дано и не отклонено. Пользователь может отказать.
- Колбэк успеха получает `GeolocationPosition`: метку времени и объект `GeolocationCoordinates` с широтой, долготой и связанными полями.
- Необязательный колбэк ошибки получает `GeolocationPositionError` с кодом и сообщением.
- `getCurrentPosition()` читает один раз. `watchPosition()` регистрирует обработчик, который вызывается снова, когда положение меняется. Тот же объект может снять ранее поставленное наблюдение.
- Стороннему iframe нужны `allow="geolocation"` и `Permissions-Policy`, где перечислен этот origin. Список разрешения по умолчанию — `self`.

**When to use:**

- Карта или результат, который зависит от того, где устройство, и только когда есть ясная причина спросить. Есть также элемент `<geolocation>` со встроенным интерфейсом и более простым запросом разрешения.

**Examples:**

```html
<script>
  navigator.geolocation.getCurrentPosition((position) => {
    const { latitude, longitude } = position.coords;
    document.body.append(`${latitude}, ${longitude}`);
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API
</details>

<details>
<summary>Что такое the best practices for using HTML with SEO?</summary>

### Что такое the best practices for using HTML with SEO?

**Definition:**

`localStorage` хранит строковые пары ключ-значение для origin между сессиями браузера. `sessionStorage` хранит их только до конца сессии этой страницы.

**How it works:**

#### Два хранилища

- Оба — объекты `Storage` на `window`. Значения — строки UTF-16. `setItem(key, value)` пишет, `getItem(key)` читает, `removeItem(key)` удаляет один ключ, `clear()` удаляет все.
- У `localStorage` нет срока. В приватном окне оно очищается, когда закрывается последняя приватная вкладка для него.
- `sessionStorage` очищается, когда сессия страницы заканчивается: MDN описывает это как закрытие страницы.
- Хранилище привязано к origin, включая схему. `http://example.com` и `https://example.com` не делят хранилище.
- `SecurityError` выбрасывается для origin, который не является кортежем схема/хост/порт, например `file:` или `data:`, или когда политика запрещает сохранение. Блокировка cookies обычно блокирует и это хранилище. Поведение `file:` URL не задано. Не опирайтесь на него.

**When to use:**

- `localStorage` для настройки, которая должна пережить перезапуск. `sessionStorage` для данных, которые должны умереть вместе со вкладкой. Ни то ни другое не место для секретов, и между origin они не общие.

**Examples:**

```html
<script>
  localStorage.setItem("myCat", "Tom");
  const cat = localStorage.getItem("myCat");
  localStorage.removeItem("myCat");
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
</details>

<details>
<summary>Что такое the Geolocation API and how is it used?</summary>

### Что такое the Geolocation API and how is it used?

**Definition:**

HTML Drag and Drop API позволяет перетащить элемент на цель сброса. Изображения, ссылки и выделенный текст перетаскиваются по умолчанию. Любому другому элементу нужен `draggable="true"`.

**How it works:**

#### Жест

- `dragstart`, `drag` и `dragend` приходят на перетаскиваемый элемент. `dragenter`, `dragleave`, `dragover` и `drop` — на возможную цель. Файл, перетащенный из операционной системы, не вызывает `dragstart`.
- Большинство элементов не являются целями сброса. Вызовите `preventDefault()` на `dragover`, иначе сброс отлетает назад и `drop` не приходит.
- Свои данные перетаскивания — строка (или `File`) в `event.dataTransfer`. `setData(type, value)` допустим только в `dragstart`. `getData(type)` допустим на `drop`.
- Типы — MIME, например `text/plain`, `text/html` и `text/uri-list`.

**When to use:**

- Перенос или копирование внутри страницы или приём файла, брошенного на страницу. Если элемент сделать перетаскиваемым, обычное выделение текста в нём блокируется, пока пользователь не зажмёт Alt.

**Examples:**

```html
<p id="p1" draggable="true">This element is draggable.</p>
<p id="target">Drop Zone</p>
<script>
  document.getElementById("p1").addEventListener("dragstart", (ev) => {
    ev.dataTransfer.setData("text/plain", ev.target.innerText);
  });
  const target = document.getElementById("target");
  target.addEventListener("dragover", (ev) => ev.preventDefault());
  target.addEventListener("drop", (ev) => {
    ev.preventDefault();
    ev.target.append(ev.dataTransfer.getData("text/plain"));
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API
</details>

<details>
<summary>Как you utilize local storage and session storage in HTML?</summary>

### Как you utilize local storage and session storage in HTML?

**Definition:**

`element.requestFullscreen()` просит браузер показать этот элемент на весь экран, скрыв интерфейс браузера. `document.exitFullscreen()` выходит из этого режима.

**How it works:**

#### Вход и выход

- Оба метода возвращают promise. `document.fullscreenElement` — элемент, который сейчас на весь экран, или `null`. `document.fullscreenEnabled` равен false, когда полный экран недоступен.
- `fullscreenchange` приходит при переходе. `fullscreenerror` — если переключение не удалось.
- Пользователь может выйти через Esc или F11, переходом на другую страницу или сменой приложения. Скажите ему, что такое управление есть.
- Старый булев `document.fullscreen` устарел. Используйте `fullscreenElement`.
- Permissions Policy с именем `fullscreen` по умолчанию равна `self`: верхний документ и фреймы того же origin.

**When to use:**

- Видео, игра или слайд, которые должны занять экран. Это не Baseline во всех основных браузерах. Проверьте поддержку, прежде чем на это опираться.

**Examples:**

```html
<video id="video" controls src="flower.mp4"></video>
<script>
  const video = document.getElementById("video");
  document.addEventListener("keydown", (e) => {
    if (e.key !== "Enter") return;
    if (!document.fullscreenElement) video.requestFullscreen();
    else document.exitFullscreen();
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API
</details>

### Internationalization

<details>
<summary>Можете ли вы describe the use of the Drag and Drop API?</summary>

### Можете ли вы describe the use of the Drag and Drop API?

**Definition:**

Поставьте `<meta charset="utf-8">` в head. UTF-8 может представить почти любой знак почти любого языка.

**How it works:**

#### Одна кодировка

- Элемент meta объявляет набор символов, которым документу разрешено пользоваться. Ставьте его на каждой странице.
- Устаревший набор вроде `ISO-8859-1` покрывает латиницу и исказит знаки вне неё, например японские.
- Часть браузеров угадывает и чинит неверную кодировку. Другие нет. Всё равно объявляйте UTF-8.
- Поток событий server-sent events тоже в UTF-8. Строки хранилища — UTF-16. Это отдельно от кодировки документа.

**When to use:**

- Одним из первых тегов в `<head>`, до текста, который парсер может прочитать неверно.

**Examples:**

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
</head>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>Что такое the Fullscreen API and why would you use it?</summary>

### Что такое the Fullscreen API and why would you use it?

**Definition:**

`lang` называет человеческий язык элемента. На `<html>` он называет язык страницы.

**How it works:**

#### Что он меняет

- Поисковые системы могут индексировать страницу для нужного языка.
- Программы чтения используют его для произношения. Одно написание может быть разными словами в двух языках.
- Часть страницы на другом языке получает свой `lang`, например `<span lang="ja">`.
- Значение — языковой тег вроде `en` или `en-US`. `lang` не задаёт направление текста. Это `dir`.
- Shadow-дерево наследует `lang` от хозяина, когда родитель — shadow root.

**When to use:**

- На корне каждого документа и ещё раз на любом отрывке, чей язык отличается от корня.

**Examples:**

```html
<html lang="en-US">
  <body>
    <p>Japanese example: <span lang="ja">ご飯が熱い。</span></p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/dir
</details>

<details>
<summary>Как you handle character encoding in HTML?</summary>

### Как you handle character encoding in HTML?

**Definition:**

Задайте атрибут `dir`. `ltr` — слева направо, `rtl` — справа налево, `auto` выбирает базовое направление по тексту.

**How it works:**

#### Базовое направление

- `lang` не задаёт направление. Арабской странице нужны и `dir="rtl"` на `<html>`, и `lang="ar"`. Когда `dir` нигде не задан, по умолчанию слева направо.
- Английский абзац внутри RTL-страницы получает `dir="ltr"` и свой `lang`. Короткую фразу противоположного направления, включая её пунктуацию, оберните в элемент с `dir`. `<bdi>` делает ту же изоляцию внутри строки.
- `dir="auto"` — для текста, чьё направление неизвестно, например комментария. Берётся первый знак с сильным направлением. Это не определение языка.
- Пропуск `dir` наследует направление родителя. Автоопределение он не включает. `<bdi>` ведёт себя как `dir="auto"`.
- У таблицы `dir="rtl"` выстраивает столбцы справа налево. Предпочитайте `dir` свойству CSS `direction`, чтобы текст оставался верным и без CSS.

**When to use:**

- На корне для письменности страницы и на любом блоке или фразе, которые идут в другую сторону.

**Examples:**

```html
<!doctype html>
<html dir="rtl" lang="ar">
  <head>
    <meta charset="utf-8" />
    <title>صفحة عربية</title>
  </head>
  <body>
    <p>محتوى الصفحة باللغة العربية.</p>
    <p dir="ltr" lang="en">This paragraph is in English.</p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/dir
</details>

### HTML and Web Development Tools

<details>
<summary>Что такое the lang attribute and its importance in HTML?</summary>

### Что такое the lang attribute and its importance in HTML?

**Definition:**

Отправьте документ в W3C Markup Validation Service. Сервис проверяет разметку и сообщает, что не так, с номером строки и столбца для каждого сообщения.

**How it works:**

#### Что ему передать

- Укажите адрес страницы, загрузите HTML-файл или вставьте весь документ на вкладке Validate by Direct Input и нажмите Check.
- Браузеры разбирают HTML снисходительно, поэтому сломанная страница всё равно может отобразиться. Валидатор находит ошибки, которые браузер замаскировал.
- Типичные отчёты: конечный тег подразумевается, потому что элемент остался открытым; элемент не закрыт; конечный тег нарушает вложенность; конец файла достигнут внутри значения атрибута.
- Одна ошибка даёт несколько сообщений. Исправьте несколько и проверьте снова. Чистый результат говорит, что ошибок и предупреждений нет.

**When to use:**

- Перед публикацией страницы и когда построенный DOM не совпадает с исходником, который вы написали.

**Examples:**

```html
<p>I didn't expect to find the <em>next-door neighbor's <strong>cat</em></strong> here!</p>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://validator.w3.org/
</details>

<details>
<summary>Как you accommodate left-to-right and right-to-left language support in HTML?</summary>

### Как you accommodate left-to-right and right-to-left language support in HTML?

**Definition:**

Pug компилирует более короткий шаблон в функцию JavaScript. Вызовите эту функцию с данными, и она вернёт строку HTML.

**How it works:**

#### Один раз скомпилировать

- `pug.compile()` или `pug.compileFile()` превращает шаблон в функцию. Передайте объект данных, его называют locals, и функция вернёт HTML.
- Скомпилированную функцию можно вызывать снова с другими данными. `#{name}` в шаблоне заменяется значением `name`.
- `pug.render()` и `pug.renderFile()` компилируют и отрисовывают за один шаг. Без параметра `cache` они компилируют заново при каждом вызове.
- Браузер получает обычный HTML. Pug выполняется раньше, обычно в Node.

**When to use:**

- Когда одна и та же разметка заполняется многими наборами данных и шаблон достаточно скомпилировать один раз.

**Examples:**

```text
p #{name}'s Pug source code!
```

```js
const compiledFunction = pug.compileFile("template.pug");
compiledFunction({ name: "Timothy" });
// "<p>Timothy's Pug source code!</p>"
```

**Sources:**

- https://pugjs.org/api/getting-started.html
</details>

<details>
<summary>Как you validate HTML?</summary>

### Как you validate HTML?

**Definition:**

Шаблонизатор превращает шаблон и данные в строку HTML. Браузер затем разбирает эту строку как обычный документ.

**How it works:**

#### Два шага

- Скомпилируйте шаблон в функцию. Отрисовка — вызов этой функции с данными текущей страницы.
- Подстановки в шаблоне становятся текстом в результате. `#{name}` в Pug — одна такая подстановка. Результат — строка элементов HTML, а не особый формат браузера.
- Используйте скомпилированную функцию для каждого набора данных. Повторная компиляция при каждой отрисовке стоит лишней работы.
- Это отдельно от элемента HTML `<template>`: он хранит инертную разметку в документе, пока скрипт её не клонирует.

**When to use:**

- На сервере или на шаге сборки, когда много страниц делят одну форму разметки.

**Examples:**

```js
pug.renderFile("template.pug", { name: "Timothy" });
// "<p>Timothy's Pug source code!</p>"
```

**Sources:**

- https://pugjs.org/api/getting-started.html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
</details>

<details>
<summary>Что такое the benefits of using an HTML preprocessor like Pug (Jade)?</summary>

### Что такое the benefits of using an HTML preprocessor like Pug (Jade)?

**Definition:**

Инструменты разработчика встроены в браузер. Они показывают HTML, который реально есть на странице, CSS каждого элемента и выполненный JavaScript.

**How it works:**

#### Инспектор

- Откройте их через Ctrl+Shift+I или F12 в Windows, Command+Option+I в macOS, или правым щелчком по странице и пунктом Inspect Element. Инструменты Safari включаются из меню Develop, которое сначала нужно включить в настройках.
- Вкладка Inspector (Elements в Chrome, Edge и Safari) показывает DOM во время выполнения. Наведите на узел, чтобы подсветить его на странице. Edit as HTML пробует правку. Правка не сохраняется в файл.
- Та же панель перечисляет правила CSS выбранного элемента, от более специфичных к менее, и блочную модель: содержимое, padding, border, margin.
- Console выполняет JavaScript на загруженной странице и печатает ошибки. Debugger (Sources в ряде браузеров) останавливается на точках останова.

**When to use:**

- Когда страница выглядит неправильно и нужен DOM, который построил браузер, а не только файл, который вы написали.

**Examples:**

```js
document.querySelector("html").style.backgroundColor = "purple";
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
</details>

### HTML Best Practices

<details>
<summary>Как a templating engine work with HTML?</summary>

### Как a templating engine work with HTML?

**Definition:**

Обычные ошибки — незакрытые элементы, элементы во вложенности не в том порядке и значение атрибута, у которого так и не закрылась кавычка. Страница всё равно может отрисоваться, потому что браузер переписывает разметку.

**How it works:**

#### Что ломается

- Незакрытый `<strong>` может сделать жирным весь остаток страницы. Пропущенная закрывающая кавычка у `href` может заставить браузер выбросить ссылку.
- Неверно вложенные теги переписываются. `<em>`, закрытый после `</strong>`, становится двумя отдельными элементами, и это может быть не то дерево, которое вы имели в виду.
- Таблицы для раскладки — устаревший приём. Для раскладки используйте flexbox или grid, а `<table>` оставьте для табличных данных.
- Классический скрипт без `async`, `defer` или `type="module"` загружается и выполняется до того, как парсер продолжит работу. `document.write()` во время разбора особенно дорог.
- У `<img>` без `width` и `height` браузеру нечего зарезервировать, поэтому раскладка прыгает, когда картинка приходит.

**When to use:**

- Как список проверки, когда инспектор DOM и валидатор расходятся с сохранённым файлом.

**Examples:**

```html
<a href="https://www.mozilla.org/>link to Mozilla homepage</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
</details>

<details>
<summary>Что такое browser developer tools, and how do you use them with HTML?</summary>

### Что такое browser developer tools, and how do you use them with HTML?

**Definition:**

Пишите теги в нижнем регистре, вкладывайте каждый элемент в тот, который его открыл, и прогоняйте документ через валидатор W3C, пока он не сообщит, что ошибок нет.

**How it works:**

#### Проверки, которые держатся

- Закрывайте элементы, которым нужен конечный тег. MDN считает пропущенный конечный тег частой ошибкой со странным результатом. Теги не чувствительны к регистру; нижний регистр удобнее читать.
- Начинайте файл с `<!doctype html>`, объявляйте `charset` и ставьте `lang` на `<html>`.
- Верная разметка позволяет работать другим инструментам. HTML Tidy может убрать пробелы и необязательные конечные теги и отказывается обрабатывать страницу с серьёзными ошибками разметки.
- Ставьте `width` и `height` на изображения, чтобы браузер зарезервировал место. Скрипты, которым нужен DOM, загружайте с `defer`.

**When to use:**

- На каждом документе и снова после того, как шаблон или шаг сборки выдал HTML.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>HTML debugging examples</title>
  </head>
  <body>
    <h1>HTML debugging examples</h1>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

<details>
<summary>Что такое some common bad practices in HTML?</summary>

### Что такое some common bad practices in HTML?

**Definition:**

Минификация убирает лишние пробелы и комментарии, чтобы файл стал меньше. Вес страницы — главный фактор того, как быстро она загружается.

**How it works:**

#### Меньше байт

- Меньше разметки — меньше скачивать. MDN называет это minimization: уберите ненужные пробелы и комментарии и вынесите встроенные скрипт и CSS во внешние файлы, когда это уменьшает страницу.
- HTML Tidy может срезать ведущие пробелы и лишние пустые строки у верного HTML. Страницу с серьёзными ошибками разметки он не обрабатывает.
- То же относится к SVG: программы рисования часто оставляют метаданные, которые можно удалить, а сервер может отдать результат в gzip.
- Минификация не меняет DOM, который браузер строит из оставшихся тегов. Меняются только байты в сети.

**When to use:**

- На копии, которую вы публикуете, после того как читаемый исходник прошёл валидатор.

**Examples:**

```html
<h1>HTML debugging examples</h1>
<p>What causes errors in HTML?</p>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

<details>
<summary>Как you ensure that your HTML code follows best practices?</summary>

### Как you ensure that your HTML code follows best practices?

**Definition:**

Уменьшите страницу, запрашивайте меньше файлов и не останавливайте парсер. Браузер должен нарисовать содержимое и нужный ему CSS до того, как запустит скрипты взаимодействия.

**How it works:**

#### Что менять в документе

- Сначала режьте вес страницы: уберите лишние пробелы и комментарии и сожмите изображения до того, как на них ссылаться.
- Меньше файлов — меньше запросов HTTP. Каждый лишний домен стоит ещё и поиска DNS. Кэшируйте повторно используемые файлы; статическим файлам сервер обычно ставит заголовок `Last-Modified`.
- Задайте `width` и `height` у изображений. Для картинок вне экрана ставьте `loading="lazy"`. Ленивые изображения могут быть ещё не готовы, когда сработает событие `load` окна.
- Классический скрипт блокирует разбор, если у него нет `async` или `defer`. `defer` выполняется в порядке документа после разбора, до `DOMContentLoaded`. `async` выполняется, как только этот скрипт загружен, без гарантии порядка. Если заданы оба, скрипт ведёт себя только как `async`.
- `<link rel="preload">` заранее забирает подресурс, который нужен текущей странице. В `as` укажите тип ресурса.

**When to use:**

- На любой странице, где первый экран ждёт изображения, лишние домены или скрипты в середине body.

**Examples:**

```html
<img src="./images/footerlogo.jpg" alt="MDN logo" loading="lazy" />
<script defer src="js/script2.js"></script>
<link rel="preload" href="main.js" as="script" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Speculative_loading
</details>

### HTML Frameworks and Libraries

<details>
<summary>Что такое the benefits of minifying HTML documents?</summary>

### Что такое the benefits of minifying HTML documents?

**Definition:**

CSS-фреймворк — это таблица стилей, которую вы подключаете из документа. Bootstrap — один такой набор: его CSS и JavaScript добавляются через `<link>` и `<script>`, а страница по-прежнему начинается с `<!doctype html>`.

**How it works:**

#### Как он подключается

- Поместите CSS `<link rel="stylesheet">` в `<head>`. Поместите пакет JavaScript перед `</body>`. В быстром старте Bootstrap также задан `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Для копии с CDN задайте `integrity` — хеш этого файла и версии — и `crossorigin="anonymous"`.
- JavaScript Bootstrap обслуживает компоненты вроде выпадающих списков, модальных окон и collapse. Выпадающим спискам, подсказкам и popover нужен ещё Popper; скрипт `bootstrap.bundle` его включает.
- Документ остаётся HTML. Фреймворк даёт правила CSS и, для части компонентов, скрипт. Элементы он не заменяет.

**When to use:**

- Когда нужен общий набор стилей и компонентов и вы готовы загрузить эту таблицу стилей и скрипт.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bootstrap demo</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css"
      integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB"
      crossorigin="anonymous"
    />
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
</details>

<details>
<summary>Как you optimize the loading time of an HTML page?</summary>

### Как you optimize the loading time of an HTML page?

**Definition:**

Bootstrap поставляет готовый файл CSS и пакет JavaScript, поэтому стили и компоненты появляются подключением этих файлов, а не написанием каждого правила вручную.

**How it works:**

#### Что вы получаете

- Нужен доктайп HTML5. Без него стили Bootstrap неполные. Мета-тег viewport нужен, чтобы страница оставалась удобной на маленьких экранах; проект собран mobile first и увеличивается через media queries.
- Reboot сбрасывает обычные элементы, чтобы они рисовались согласованнее в разных браузерах. Глобальный `box-sizing` равен `border-box`, поэтому padding входит в объявленную ширину.
- Компонентам со скриптом относятся alerts, carousels, collapse, dropdowns, modals, navbars и toasts. Одной ссылки на CSS для этих поведений мало.
- Popper можно не подключать и взять `bootstrap.min.js` вместо bundle, если вы не используете dropdowns, popovers и tooltips.

**When to use:**

- Для прототипа или страницы продукта, которой достаточно раскладки, сброса и компонентов Bootstrap из одной описанной установки.

**Examples:**

```html
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"
  integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI"
  crossorigin="anonymous"
></script>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
</details>

<details>
<summary>Что такое some popular CSS frameworks that can be integrated with HTML?</summary>

### Что такое some popular CSS frameworks that can be integrated with HTML?

**Definition:**

Библиотека — это готовый JavaScript, который упаковали другие люди. В обзоре MDN React назван библиотекой интерфейса, а Angular, Vue и Ember — фреймворками, которые тоже рисуют интерфейс из ваших данных.

**How it works:**

#### На чём они стоят

- Точка входа в HTML по-прежнему элемент `<script>`. `defer` запускает внешний классический скрипт после разбора документа и в порядке исходника. `type="module"` считает файл модулем и по умолчанию откладывает его.
- React — библиотека для отрисовки компонентов интерфейса. React DOM — часть, которая нацелена на веб-страницу. Angular и Vue расширяют HTML своим синтаксисом шаблонов; Angular компилирует эти шаблоны в JavaScript. Ember старше этих четырёх и всё ещё используется.
- Они появились, потому что ручное обновление DOM через `document.createElement` и новая отрисовка после каждого изменения состояния быстро разрастаются.
- Фреймворк — это библиотека с мнениями о том, как строить приложение. Те же взаимодействия можно сделать API DOM и без библиотеки.

**When to use:**

- Когда много частей страницы должны оставаться в согласии с меняющимся состоянием, и поддерживаемая библиотека уже решает этот цикл обновления.

**Examples:**

```html
<script defer src="app.js"></script>
<script type="module" src="main.js"></script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
</details>

### Advanced HTML Topics

<details>
<summary>Как frameworks like Bootstrap simplify HTML development?</summary>

### Как frameworks like Bootstrap simplify HTML development?

**Definition:**

Визуализация данных — это рисунок значений. В HTML Canvas API рисует её скриптом на элементе `<canvas>`. Сам элемент — это растр.

**How it works:**

#### Canvas

- `document.getElementById` находит элемент. `getContext("2d")` возвращает поверхность, на которой вы рисуете. `fillStyle` задаёт цвет, а `fillRect(x, y, width, height)` закрашивает прямоугольник.
- WebGL использует тот же элемент `<canvas>` для аппаратно ускоренного 2D и 3D.
- Содержимое canvas не открыто инструментам доступности так, как семантический HTML. Текст из пикселей расплывается при увеличении. Те же сведения положите в настоящую разметку, например в таблицу или заголовок, рядом с растром или вместо него, когда числа важны.
- MDN также перечисляет библиотеки canvas для графиков и тепловых карт, среди них heatmap.js и JavaScript InfoVis Toolkit. API рисования без них работает.

**When to use:**

- Для графики, которую считает скрипт. Для нескольких чисел, которые нужно прочитать вслух, начните с таблицы.

**Examples:**

```html
<canvas id="canvas"></canvas>
<script>
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  ctx.fillStyle = "green";
  ctx.fillRect(10, 10, 150, 100);
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
</details>

<details>
<summary>Можете ли вы name some JavaScript libraries that enhance HTML interactivity?</summary>

### Можете ли вы name some JavaScript libraries that enhance HTML interactivity?

**Definition:**

Отдайте основное содержимое и поведение в HTML, чтобы оно работало широко, а более богатый опыт добавляйте там, где браузер может его выполнить.

**How it works:**

#### Сначала основа

- Полезная страница — это разметка: ссылки, заголовки, формы с `action` и `method`. Браузер, который не запускает скрипт, всё равно может отправить форму и перейти по ссылке.
- Обнаружение возможностей спрашивает, есть ли в этом браузере более новая возможность. Полифилы добавляют недостающую возможность на JavaScript, если вы так решили.
- Дайте доступную замену там, где улучшенная версия иначе спрячет содержимое. График на `<canvas>` — один такой случай: числа всё равно принадлежат тексту или таблице.
- Graceful degradation начинает с полного опыта и урезает его. Progressive enhancement начинает с простой страницы и наращивает её. Оба подхода допустимы.

**When to use:**

- Когда скрипт или новый элемент не обязательны, чтобы прочитать страницу и закончить задачу.

**Examples:**

```html
<form action="/search" method="get">
  <label for="q">Search</label>
  <input id="q" name="q" />
  <button>Search</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement
- https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
</details>

<details>
<summary>Что такое data visualizations in HTML and how can they be implemented?</summary>

### Что такое data visualizations in HTML and how can they be implemented?

**Definition:**

HTML размечает структуру и смысл, CSS задаёт представление, а JavaScript меняет страницу после загрузки. Браузер строит DOM из HTML, затем применяет CSS и запускает скрипт против этого DOM.

**How it works:**

#### Три слоя

- HTML определяет части: абзац, заголовок, кнопку, таблицу, изображение. Таблица стилей браузера по умолчанию уже делает заголовки крупнее, а ссылки подчёркнутыми. Авторский CSS заменяет и дополняет эти умолчания.
- Таблица стилей — набор правил. Селектор вроде `button` выбирает элементы, а объявления вроде `font-size: 1.4em` задают значения. Правила в элементе `<style>` или в подключённом файле `.css` сопоставляются с DOM и рисуются.
- JavaScript нужен, когда страница делает больше, чем показывает статичный текст. `document.querySelector("button")` находит элемент, а `addEventListener` запускает вашу функцию по щелчку. `textContent` заменяет подпись кнопки.
- Держите три слоя в документе как HTML, таблицу стилей и скрипт. Скрипт читает и пишет DOM, который создал HTML.

**When to use:**

- На каждой странице: структура в HTML, вид в CSS, обновления в скрипте только там, где содержимое должно меняться.

**Examples:**

```html
<button>Player 1: Chris</button>
<script>
  const button = document.querySelector("button");
  button.addEventListener("click", () => {
    button.textContent = "Player 1: updated";
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/What_is_CSS
</details>

<details>
<summary>Можете ли вы explain how progressive enhancement is applied in HTML?</summary>

### Можете ли вы explain how progressive enhancement is applied in HTML?

**Definition:**

Выбранный элемент и есть документация содержимого. Заголовок, абзац и кнопка сообщают браузеру, вспомогательным технологиям и следующему автору, чем является этот кусок.

**How it works:**

#### В файле и снаружи

- HTML — язык разметки структуры. `<p>` помечает абзац, а `<h1>` — заголовок верхнего уровня. К этому выбору потом крепятся CSS и скрипт.
- Вкладывайте теги в том порядке, в котором открыли, и пишите их в нижнем регистре, чтобы файл оставался читаемым. Пропущенный конечный тег — частый источник DOM, который вы не задумывали.
- Комментарий HTML, `<!-- -->`, предназначен автору. В списке изменений HTML 5.2 комментарии — часть синтаксиса HTML, в том числе комментарии с двумя дефисами подряд.
- HTML Living Standard — точный справочник. Во введении сказано, что учебники и руководства для авторов — более мягкий вход, когда сама спецификация слишком плотная.

**When to use:**

- Пока вы пишете разметку, и снова, когда кто-то другой должен изменить страницу, не угадывая, какой элемент имелся в виду.

**Examples:**

```html
<!-- The label is the player’s current name. -->
<h1>Match</h1>
<p>First to five points.</p>
<button>Player 1: Chris</button>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
- https://html.spec.whatwg.org/multipage/introduction.html
- https://www.w3.org/TR/2017/REC-html52-20171214/changes.html
</details>

### HTML 5.1, 5.2, and Beyond

<details>
<summary>Как HTML, CSS, and JavaScript interconnected in web development?</summary>

### Как HTML, CSS, and JavaScript interconnected in web development?

**Definition:**

HTML 5.1 (рекомендация W3C от 1 ноября 2016) и HTML 5.2 (14 декабря 2017) — нумерованные снимки. Они фиксируют, что W3C добавил и убрал относительно предыдущего снимка. Текущий HTML — это Living Standard WHATWG.

**How it works:**

#### HTML 5.1 относительно HTML5

- Добавлены элемент `<picture>` и атрибут `srcset` для отзывчивых изображений, а также `<details>` с `<summary>` для необязательного дополнительного текста.
- Добавлены `rel="noopener"` и атрибут `nonce` на `<script>` и `<style>` для Content Security Policy.
- Убраны application cache, атрибут `usemap` на `<object>` и атрибут `form` на `<label>`. Вложенные элементы `<section>`, у каждого из которых свой `<h1>`, больше не строят оглавление.
- Тогда же добавили `<menuitem>` и `type="context"`. HTML 5.2 затем убрал `<menu>` и `<menuitem>`.

#### HTML 5.2 относительно HTML 5.1

- Добавлены `<dialog>`, интеграция с модулями JavaScript и `rel="canonical"` на ссылках. `nonce` определён и на `<link>`.
- Убраны `<keygen>`, `<menu>` и `<menuitem>`, метод `showModalDialog()` и, в этом снимке, `inputmode` и `dropzone`.
- Там стали допустимы: элемент `<style>` в body, больше одного `<main>`, если виден только один, `<div>` внутри `<dl>` и пустой `<option>` в `<datalist>`.
- Доктайп HTML 4 или XHTML 1 strict в этом снимке не является допустимым HTML.

**When to use:**

- Когда собеседование или старая статья ссылается на 5.1 или 5.2. Для элемента, который вы отдаёте сегодня, читайте Living Standard.

**Examples:**

```html
<details>
  <summary>More</summary>
  <p>Extended information.</p>
</details>
<dialog>
  <p>A dialog element added in the HTML 5.2 changes.</p>
</dialog>
```

**Sources:**

- https://www.w3.org/TR/2016/REC-html51-20161101/changes.html
- https://www.w3.org/TR/2017/REC-html52-20171214/changes.html
- https://html.spec.whatwg.org/multipage/introduction.html#history-2
</details>

<details>
<summary>Обсудите the importance of documentation in HTML.</summary>

### Обсудите the importance of documentation in HTML.

**Definition:**

У HTML нет назначенного номера версии, которого нужно ждать. Living Standard обновляется на месте, и возможность попадает в него, когда реализации и спецификацию приводят в соответствие.

**How it works:**

#### Как приходит изменение

- С соглашения 2019 года этот Living Standard — единственный документ HTML. Опубликованный текст озаглавлен Living Standard и показывает дату последнего обновления; издание, актуальное 4 октября 2026, несло эту дату.
- Рабочее правило WHATWG — обратная совместимость, и спецификация меняется так, чтобы совпасть с тем, что реализации реально делают, когда именно это даёт совместимость.
- Нумерованные снимки W3C вроде 5.1 и 5.2 перестали быть местом, где публикуют новый HTML. Удаление в одном из этих снимков — не последнее слово, если Living Standard с тех пор снова описал возможность.
- Точки расширения для авторов, которые уже есть и которые перечисляет спецификация: `class`, атрибуты `data-*`, `<meta name>`, `rel`, `<script type>`, который не является JavaScript, и пользовательские элементы.

**When to use:**

- Когда спрашивают дорожную карту. Укажите на текущий текст Living Standard для этой возможности, а не на имя будущего выпуска.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Current HTML</title>
  </head>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://html.spec.whatwg.org/multipage/introduction.html#extensibility
</details>

<details>
<summary>Какие updates были введены в HTML 5.1 и 5.2?</summary>

### Какие updates были введены в HTML 5.1 и 5.2?

**Definition:**

HTML ведётся как один Living Standard в WHATWG, и с 2019 года W3C сотрудничает над тем же документом. CSS, JavaScript, HTTP и DOM — отдельные спецификации, рядом с которыми стоит спецификация HTML.

**How it works:**

#### Один документ, много соседей

- С 1998 года W3C приостановил HTML в пользу XHTML. В 2004 году Apple, Mozilla и Opera продолжили HTML в WHATWG после того, как семинар W3C отверг это направление.
- В 2011 году группы разошлись по цели: W3C хотел законченный HTML5, а WHATWG хотел и дальше чинить известные проблемы и добавлять возможности.
- В 2019 году они подписали соглашение сотрудничать над одной версией HTML: Living Standard.
- Собственная карта платформы в спецификации HTML ставит рядом CSS, SVG, HTTP, DOM, JavaScript и другие. Представление остаётся в CSS. Поведение остаётся в скрипте. HTML определяет элементы и API скриптов, связанные с этими элементами.

**When to use:**

- Когда нужно объяснить, почему «HTML5» в записи блога и текущая спецификация могут расходиться.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="styles.css" />
    <title>Layers</title>
  </head>
  <body>
    <h1>Layers</h1>
    <script defer src="app.js"></script>
  </body>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
</details>

<details>
<summary>Что такое Living Standard и как HTML ему следует?</summary>

### Что такое Living Standard и как HTML ему следует?

**Definition:**

Living Standard — это спецификация HTML, которую непрерывно поддерживают, а не замораживают. Браузеры и авторы следуют этому документу. `<!doctype html>` выбирает этот HTML.

**How it works:**

#### Что значит «живой»

- WHATWG выбрал Living Standard, чтобы спецификация не выходила с оставленными неисправленными известными проблемами и чтобы новые возможности добавлялись, когда они нужны платформе.
- Спецификация отвечает на «Это HTML5?» коротко: да. «HTML5» — модное слово для современных веб-технологий, и этот документ — одна из них. Это не замороженный выпуск 5.x.
- Документы, отданные как `text/html`, разбираются как HTML. Тот же язык можно отдать как XML (`application/xhtml+xml`), и там небольшая синтаксическая ошибка останавливает полную отрисовку. Синтаксис HTML — тот, который спецификация предлагает большинству авторов.
- Соответствие для авторов — это то, что проверяет валидатор. W3C Markup Validation Service — инструмент, на который MDN указывает для этой проверки.

**When to use:**

- Как справочник для любого элемента или атрибута, на который вы собираетесь опереться, раньше нумерованной рекомендации 2016 или 2017 года.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Living Standard</title>
  </head>
  <body>
    <p>Parsed as HTML because of the doctype and the text/html type.</p>
  </body>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://html.spec.whatwg.org/multipage/introduction.html#is-this-html5?
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
</details>

### HTML Challenges and Real-world Scenarios

<details>
<summary>Какие future updates вы видите для HTML?</summary>

### Какие future updates вы видите для HTML?

**Definition:**

Пишите HTML, который делает дело без самой новой возможности, а эту возможность добавляйте там, где обнаружение возможностей показывает, что браузер может её выполнить.

**How it works:**

#### Что отдавать

- Progressive enhancement держит содержимое в элементах, которые уже понимают и старые, и новые браузеры: ссылки, формы, заголовки. Обнаружение возможностей проверяет дополнительную способность. Полифил может закрыть пробел на JavaScript.
- Для модулей отдайте `<script type="module">` и второй скрипт с `nomodule`. Браузеры, которые понимают модули, игнорируют скрипт с `nomodule`. Модульные скрипты по умолчанию откладываются.
- `async` и `defer` убирают блокировку парсера у классических скриптов. Руководство MDN по загрузке говорит использовать их и не считать, что каждый браузер их соблюдает.
- Не требуйте, чтобы страница выглядела пиксель в пиксель в старых браузерах. Руководство MDN по быстрой загрузке предлагает целиться в текущие браузеры, которые реализуют нужные стандарты.
- Верный HTML по-прежнему важен: браузеры по-разному чинят сломанную разметку, и это само по себе ошибка совместимости.

**When to use:**

- Когда возможность новее, чем самый старый браузер, от которого вы всё ещё ждёте выполнения задачи.

**Examples:**

```html
<script type="module" src="main.js"></script>
<script nomodule src="fallback.js"></script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

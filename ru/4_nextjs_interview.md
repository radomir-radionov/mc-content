# Next.js — Interview

Вопросы для собеседования по Next.js.

[← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Оптимизации](./4_nextjs_optimizations.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы из [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/).

### Next.js Fundamentals

<details>
<summary>Что такое Next.js and what are its main features?</summary>

### Что такое Next.js and what are its main features?

**Definition:**

**Next.js** выделяется как React-фреймворк промышленного уровня: он даёт и скорость, и специализацию. Универсальность, приёмы получения данных, поисковая оптимизация (SEO) и разные варианты развёртывания делают его частым выбором для современной веб-разработки.

**How it works:**

**Основные возможности**

1. **Server-Side Rendering (SSR)**: Next.js динамически генерирует HTML на сервере для каждого запроса страницы, отдаёт пользователям уже собранный контент и обеспечивает оптимальное SEO.

2. **Static Site Generation (SSG)**: Заранее вычисляя HTML отдельных страниц во время сборки, Next.js добивается более высокой производительности и безопасности. Он особенно хорошо подходит для сайтов с большим объёмом контента.

3. **Hybrid Rendering**: Сочетая SSR и SSG, Next.js позволяет одни страницы рендерить во время сборки (SSG), а другие по запросу (SSR). Это полезно, когда в приложении есть и динамический, и статический контент.

4. **Маршрутизация**: Можно выбрать маршрутизацию по файловой системе или динамическую. Первая убирает необходимость явно описывать маршруты, вторая даёт программное управление. Оба варианта работают вместе с SSG и SSR.

5. **Code Splitting**: Из коробки Next.js выделяет отдельные части приложения и оптимизирует сборку, отправляя клиенту только нужный код (и ресурсы). Это помогает и производительности, и времени загрузки.

6. **Мощная оптимизация изображений**: Компоненты изображений Next.js сами оптимизируют картинки, сохраняя качество на разных устройствах и быструю загрузку.

7. **Мгновенное развёртывание**: Сервисы вроде Vercel дают быстрое и простое развёртывание, A/B-тесты и другие продвинутые возможности.

8. **API Routes**: API-маршруты Next.js позволяют встроить серверные ресурсы прямо в приложение.

9. **Встроенная поддержка CSS**: В Next.js можно использовать обычные CSS-файлы, CSS-модули или препроцессоры вроде Sass без дополнительной настройки.

10. **Большая экосистема плагинов**: Функциональность проекта Next.js расширяется широким набором решений сообщества Next.js.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Как Next.js differ from Create React App?</summary>

### Как Next.js differ from Create React App?

**Definition:**

Рассмотрим ключевые отличия Next.js и Create React App.

**How it works:**

**Основная функциональность**

- **Next.js**: Встроенные server-side rendering (SSR) и client-side rendering (CSR). Также поддерживается static site generation (SSG). Есть возможности вроде предварительной загрузки данных, которые повышают производительность. Предусмотрена встроенная маршрутизация API.
- **Create React App**: В первую очередь ориентирован на client-side rendering (CSR). Не даёт встроенного server-side rendering и продвинутых режимов рендеринга вроде SSG или ISR. Для server-side rendering опирается на сторонние решения.

**Маршрутизация**

- **Next.js**: Маршрутизация по файловой системе упрощает описание маршрутов. Также поддерживаются пользовательские маршруты. Динамическая маршрутизация и **code splitting** работают автоматически и повышают производительность.
- **Create React App**: Клиентская маршрутизация через пакеты вроде React Router. Маршруты нужно явно описать в файле конфигурации или в виде компонентов.

**Структура файлов и конфигурация**

- **Next.js**: Модель без обязательной настройки для быстрого старта. Разумные значения по умолчанию подталкивают к хорошим практикам. Дополнительные параметры конфигурации позволяют уточнить поведение.
- **Create React App**: Как и Next.js, стартует без обязательной настройки. Изменения ведут через `react-scripts` и продвинутую, но объёмную процедуру `eject`. Этот шаг необратимо отрывает проект от конфигурации по умолчанию, и дальше сложнее подтягивать обновления.

**Работа с API**

- **Next.js**: Встроенные бессерверные API-маршруты. Эндпоинты API описывают обычными HTTP-запросами. Бэкенд получается проще: не нужно отдельно управлять сервером или писать дополнительную серверную логику.
- **Create React App**: Для общения с бэкендом используют библиотеки вроде Axios или Fetch. В отличие от Next.js, встроенного решения для серверных API-маршрутов нет.

**Code Splitting**

- **Next.js**: Автоматически разделяет импорты по страницам и компонентам. Загрузка по требованию сокращает время первой загрузки страницы. Кроме того, возможно управление разделением кода на уровне модуля и ещё более точечно.
- **Create React App**: Для разделения кода используют функцию `React.lazy` и компоненты `Suspense`. Точки разделения разработчик выбирает вручную.

**Получение данных**

- **Next.js**: Много способов получить данные: `getStaticProps`, `getStaticPaths`, `getServerSideProps` и клиентские методы. Эти функции входят в **Data Fetching API** и рассчитаны на конкретные стратегии рендеринга.
- **Create React App**: Нет специализированных приёмов получения данных, которые есть в Next.js. Вместо этого подключают библиотеки для загрузки данных или используют привычные подходы, например `useEffect` в функциональных компонентах.

**Развёртывание**

- **Next.js**: Гибкие варианты развёртывания. На бессерверном хостинге деплой эффективен и масштабируется, особенно для небольших и средних проектов. Обычный хостинг с сервером Node.js даёт больше возможностей для настройки.
- **Create React App**: Лучше всего подходит для статических приложений и одностраничных приложений, которые часто выкладывают через сети доставки контента (CDN). Для динамического контента его нужно связать с бэкендом вроде REST API и разворачивать как два отдельных приложения.

**SEO и метатеги**

- **Next.js**: Сильнее поддерживает поисковую оптимизацию (SEO). Закрыты базовые вещи вроде метатегов, а продвинутые режимы рендеринга, такие как SSG и ISR, заметно помогают SEO, что особенно важно для сайтов с большим объёмом контента.
- **Create React App**: Нет встроенных механизмов вроде SSR или специальных режимов рендеринга для SEO. Чтобы сделать сайт удобным для поисковиков, приходится брать сторонние инструменты или писать своё решение.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Какая команда используется для создания нового Next.js app?</summary>

### Какая команда используется для создания нового Next.js app?

**Definition:**

Чтобы создать новое приложение **Next.js**, можно использовать `npx` вместе с пакетом `create-next-app`.

**How it works:**

Подставьте вместо **"folder-name"** нужный каталог проекта. Здесь `npx` — запускатель пакетов, который берёт самую свежую версию `create-next-app`. Если у вас Node.js 10 или более ранний, `create-next-app` нужно установить как глобальный пакет.

Этот способ, через `npm` и глобальный пакет, не рекомендуется на более новых версиях Node.js.

**Examples:**

```bash
npx create-next-app folder-name
```

```bash
npm install -g create-next-app
create-next-app folder-name
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Как Next.js handle server-side rendering (SSR)?</summary>

### Как Next.js handle server-side rendering (SSR)?

**Definition:**

**Next.js** делает Server-Side Rendering (SSR) простым и понятным механизмом.

**How it works:**

**Ключевые части**

- **Page**: Конкретный маршрут, к которому обращаются.
- **Route**: Заданная пользователем конечная точка, которая соответствует конкретной странице или экрану.
- **App**: Корень приложения Next.js, который держит общую настройку и состояние.

**Процесс SSR**

1. **Начальный запрос**: Когда пользователь открывает конкретный маршрут или страницу, Next.js перехватывает этот запрос.
2. **Получение данных**: Нужные данные страницы загружаются заранее, до рендера страницы.
3. **SSR-рендер**: Next.js рендерит запрошенную страницу на сервере и отправляет готовый контент, включая полученные данные, ответом на первый запрос пользователя.

**Польза: данные видны сразу**

Контент страницы и данные рендерятся на сервере и приходят в первом ответе, поэтому пользователь быстрее видит осмысленное содержимое, в том числе на динамических страницах, которые зависят от данных.

**Этап гидратации данных**

После первого серверного рендера управление берёт клиентский JavaScript. В этот процесс входят:

- **Переход по страницам**: Каждый раз, когда пользователь переходит на новую страницу внутри приложения.
- **Клиентская маршрутизация**: Браузер берёт на себя смену маршрутов, а Next.js синхронизирует это.
- **Получение данных**: Если нужно, Next.js дополнительно запрашивает данные на клиенте для следующих обновлений состояния или смены динамического контента.

**Удобство для пользователя**

- Первый ответ сервера: в первом запросе приходят серверный контент и нужные данные, и страница ощущается быстрее.
- Более живой интерфейс: серверный контент сочетается с клиентскими обновлениями, и состояние страницы вместе с содержимым остаётся согласованным.

**Examples:**

Типичная структура страницы **Next.js**:

```jsx
import Head from 'next/head';

const SamplePage = ({ data }) => {
  return (
    <div>
      <Head>
        <title>Sample Page</title>
      </Head>
      <h1>Sample Content</h1>
      <p>{data}</p>
    </div>
  );
};

export async function getServerSideProps() {
  // Perform necessary data fetching
  let data = 'Some server-rendered data';
  return { props: { data } };
}

export default SamplePage;
```

В этом примере:

- Функция получения данных `getServerSideProps` гарантирует, что данные есть до рендера страницы. Этот метод специально предназначен для Server-Side Rendering в Next.js.
- Проп `data`, полученный из функции загрузки данных, передаётся в компонент `SamplePage` до первого рендера. Поэтому нужные данные можно показать сразу.
- Компонент `Head` из `next/head` собирает метаданные конкретной страницы, например заголовок.

Вот код:

```jsx
import Link from 'next/link';

const Navigation = () => {
  return (
    <nav>
      <Link href="/about">
        <a>About</a>
      </Link>
      <Link href="/contact">
        <a>Contact</a>
      </Link>
    </nav>
  );
};

export default Navigation;
```

В этом примере:

- Компонент `Link` оборачивает теги ссылок, чтобы переход внутри приложения был эффективным и учитывал и серверный контент, и последующие клиентские обновления.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Можете ли вы explain the puRPOse of the pages directory in a Next.js project?</summary>

### Можете ли вы explain the puRPOse of the pages directory in a Next.js project?

**Definition:**

Каталог **`pages`** — основной **инструмент описания маршрутов** в приложении **Next.js**. Каждый файл в нём представляет уникальный URL.

**How it works:**

**Ключевые возможности**

- **Автоматическая маршрутизация**: Свою маршрутизацию писать не нужно. Любой файл, добавленный в `pages`, становится маршрутом.
- **Соответствие файла и маршрута**: Структура файлов в `pages` отображается на конкретные маршруты. Например, `pages/post/index.js` связан с `/post`.
- **Динамические маршруты**: Файлы с квадратными скобками, такие как `[id].js`, задают динамические параметры. Например, `pages/post/[id].js` связан с `/post/:id`.

**Особые роли**

У некоторых имён файлов и подкаталогов своя роль в маршрутизации.

- **`404.js`**: Если файл есть, Next.js использует его для своей страницы 404.
- **`_app.js`**: Лежит на верхнем уровне. Это главная страница, которая оборачивает остальные. Удобно подключать глобальные стили или контексты.
- **`_document.js`**: Тоже на верхнем уровне, управляет серверным HTML-документом. Подходит для интерфейса, который должен быть общим для страниц.

**Examples:**

Рассмотрим такую структуру файлов:

```plaintext
pages/
  ├── index.js
  │
  ├── post/
  │    └── [id].js
  │
  └── about.js
```

Эта структура соответствует таким маршрутам:

- **/index.js**: Служит посадочной страницей.
- **/post**: Отдаёт маршрут `post`. Здесь должен быть код для запросов без `id`.
- **/about.js**: Прямой маршрут на страницу about, и он тоже считается маршрутом.
- **/post/abc**:
  - Соответствует `pages/post/[id].js`, где `id` равен "abc".

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>В Next.js, how do you create a page that is rendered on the server for every request?</summary>

### В Next.js, how do you create a page that is rendered on the server for every request?

**Definition:**

В Next.js серверный рендеринг настраивают для страниц, которым **всегда нужны свежие данные**. Хороший пример — дашборд в реальном времени или посадочная страница, которая зависит от данных.

**How it works:**

**Server-Side Rendering (SSR) и Static Generation (SG)**

- **SSR**: При каждом визите запрос уходит на сервер, чтобы получить свежие данные и собрать HTML-ответ. Так часто делают, если данные меняются часто.
- **SG**: Все запросы получают заранее собранный HTML. Это подходит контенту, который не меняется при каждом визите. Но SG может не подойти там, где нужны данные в реальном времени.

**Как включить SSR в Next.js**

Чтобы включить SSR в Next.js, сделайте так:

1. **Настройте компонент страницы**:
   - Вместе с обычным статическим методом `getStaticProps` добавьте `getServerSideProps` для серверного рендеринга.
   - Этот метод выполняется при **каждом запросе**, поэтому подходит для динамических данных.
   - Обязательно **экспортируйте** этот метод из компонента страницы.
2. **Использование данных**:
   - `getServerSideProps` должен вернуть нужные данные, как и другие методы получения данных в Next.js.
   - Данные придут в компонент страницы как пропсы.
3. **Разверните на системе, которая поддерживает SSR**:
   - Next.js отдаёт SSR из коробки, поэтому ради этого ничего отдельно настраивать не нужно.
   - Для эффективности можно использовать **механизм кэширования**, чтобы формировать данные, которые не меняются между соседними запросами.

**Examples:**

Вот код:

1. **Компонент страницы**: Pages/ServerSide.js

```javascript
export default function ServerSide({ time }) {
  return <p>Current time: {time}</p>;
}

export async function getServerSideProps() {
  return {
    props: {
      time: new Date().toISOString(),
    },
  };
}
```

Этот код передаёт проп `time`, который обновляется при каждом запросе, и тем самым показывает серверный рендеринг.

2. **Ссылка в приложении**: ParentComponent.js

```javascript
import Link from 'next/link';

export default function ParentComponent() {
  return (
    <div>
      <Link href="/ServerSide">Live Time Page</Link>
    </div>
  );
}
```

Пользователи видят обновлённое время каждый раз, когда открывают "Live Time Page."

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Какие file extensions Next.js поддерживает для pages?</summary>

### Какие file extensions Next.js поддерживает для pages?

**Definition:**

**Next.js** даёт большую свободу в описании компонентов страниц и поддерживает разные расширения файлов.

**How it works:**

**Поддерживаемые расширения**

- `.js`: Обычный JavaScript.
- `.jsx`: React с JavaScript.
- `.ts`: TypeScript.
- `.tsx`: React с TypeScript.

**Текстовые форматы**

- `.mdx`: MDX — markdown с расширениями JSX, чтобы добавить интерактивность.

**Стили компонентов**

- `.css`: Обычный CSS.
- `.module.css`: CSS-модули с локальной областью видимости.
- `.scss`: SCSS, с расширенными возможностями и вложенностью.

**Получение данных**

- `.fetch.js`: Получение данных на клиенте.
- `.fetch.ts`: Получение данных на клиенте в TypeScript.

**Обработка ошибок**

- `_error.*`: Особое имя файла для своих страниц ошибок.

**Examples:**

- Страница на `.jsx` и `.scss`

```jsx
// myPage.jsx
import React from 'react';
import styles from './myPage.module.scss';

const MyPage = () => {
  return <div className={styles.myPage}>Hello, SCSS!</div>;
};

export default MyPage;
```

- MDX-файл с компонентами React и стилями

```jsx
// myMDXPage.mdx
import MyComponent from '../components/MyComponent';
import './myMDXPageStyles.css';

# My MDX Page

<MyComponent />
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Как environment variables work in Next.js?</summary>

### Как environment variables work in Next.js?

**Definition:**

**Переменные окружения** в Next.js задают значения, которые зависят от конкретного развёртывания.

**How it works:**

**Серверные, сборочные и клиентские переменные окружения**

- **Сервер**: Нужны до старта приложения или веб-сервера, например ключи API.
- **Сборка**: Настройки на время процесса сборки.
- **Клиент**: Для общего использования на клиенте. Секреты сюда класть нельзя.

**Настройка переменных окружения**

1. Создайте файл `.env.local` в корне проекта и перечислите пары ключ-значение, каждую на отдельной строке.
2. Читайте заданные переменные в коде через `process.env.<VARIABLE_NAME>`.
3. Секретные значения на платформах вроде Vercel храните их средствами управления переменными окружения.
4. Значения по умолчанию: если задать запасное значение в коде, Next.js не сломается, когда переменной окружения нет.

**Examples:**

```plaintext
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

```javascript
console.log(process.env.DB_HOST);
```

```javascript
const dbHost = process.env.DB_HOST || 'localhost';
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Что такое Automatic Static Optimization in Next.js?</summary>

### Что такое Automatic Static Optimization in Next.js?

**Definition:**

**Automatic Static Optimization** в **Next.js** позволяет автоматически заранее отрендерить страницы в статический HTML, если они не запрашивают данные из внешнего источника.

Этот приём убирает необходимость серверного рендеринга во время запроса, когда страница опирается только на клиентские данные.

**How it works:**

**Как это работает**

1. **Анализ страницы**: Во время сборки Next.js смотрит каждую страницу и проверяет, запрашивает ли она данные. Если нет, страницу помечают для статической оптимизации. Сюда часто попадают страницы, которые используют только внутреннее состояние, клиентские библиотеки или контекст.
2. **Предварительный рендер**: Для страниц, помеченных как статические, Next.js создаёт HTML-файлы во время сборки. Эти оптимизированные страницы затем отдаются без server-side rendering.
3. **Повторная проверка данных**: У статических страниц данные могут устареть. Next.js использует стратегию revalidate, чтобы периодически обновлять данные. Для многих приложений это удачный компромисс между производительностью и точностью данных.

**Случаи использования**

- **Контентные страницы**: Подходят страницам с большим объёмом контента, например блогам или маркетинговым материалам, где данные меняются нечасто и небольшая задержка обновления допустима.
- **Маркетинговые кампании**: Для промо-кампаний или разовых событий, где данные в реальном времени не важны, а быстрая первая загрузка важна.

**Кэш и ограничения**

- **Клиентский кэш**: Отдельному пользователю могут отдать закэшированную статическую страницу, и клиентские данные всё равно могут устареть. Приёмы вроде incremental static regeneration (ISR) лучше гарантируют свежесть: страницы обновляются через заданные интервалы.
- **Зависимость от данных**: Страницам, которым нужны свежие внешние данные, нужен серверный или гибридный рендеринг. Иначе есть риск отдать устаревший контент.

**Examples:**

```javascript
// pages/product/[id].jsx

// Using getStaticProps for static optimization
export async function getStaticProps({ params }) {
  const product = await someFetchFunction(params.id);
  return {
    props: { product },
    revalidate: 10, // Regenerate every 10 seconds for a more updated page
  };
}

// If real-time data is an absolute must, use getServerSideProps instead
export async function getServerSideProps({ params }) {
  const product = await someFetchFunction(params.id);
  return { props: { product } };
}

function Product({ product }) {
  // Render product details
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

### Pages and Routing

<details>
<summary>Как file-BASEd routing work in Next.js?</summary>

### Как file-BASEd routing work in Next.js?

**Definition:**

Файловая маршрутизация в Next.js упрощает организацию страниц веб-приложения. Если соблюдать соглашения об именах файлов и выделенные папки, структуру проекта Next.js можно собрать без лишней возни.

**How it works:**

**Механизм**

Наличие **файлов JavaScript или Markdown** в определённых каталогах говорит Next.js, что в приложении нужно настроить страницы. Например, навигационные ссылки можно выстроить из структуры каталогов.

**Ключевые каталоги**

- Каталог `pages` — это **корень**, где лежат страницы приложения.
- Вложенные папки, например `pages/blog`, представляют **разделы** приложения.

**Форматы файлов**

- Для обычной страницы используют расширение `.js` или `.jsx`, а для TypeScript — `.ts` или `.tsx`.
- Для блога или разделов вроде документации можно взять `.mdx` (гибрид Markdown и React): контент рендерится рядом с компонентами.

**Pluses and minuses:**

**Плюсы**

- **Прямолинейная навигация**: Структура проекта в каталоге `pages` повторяет структуру сайта.
- **Модульность**: Разделы живут в отдельных каталогах, и кодовую базу проще держать упорядоченной.
- **Поддержка изоморфизма**: Наличие и клиентского, и серверного кода помогает приложениям работать и на клиенте, и на сервере, что улучшает SEO и время первой загрузки.

**Ограничения**

- **Ограничение основными каталогами**: Необычная конфигурация может выйти за возможности файловой маршрутизации, и тогда нужна дополнительная настройка.
- **Сложность глубокой вложенности**: Большое число глубоко вложенных каталогов трудно сопровождать, и это может мешать пониманию или производительности.

**Examples:**

Рассмотрим простой интернет-магазин.

Ниже структура каталогов:

```plaintext
pages/
|__ index.js
|__ cart.js
|__ products/
     |__ index.js
     |__ [product-id].js
```

В этой структуре:

- `pages/index.js`: Служит посадочной страницей.
- `pages/cart.js`: Корзина, доступная по `yoursite.com/cart`.
- `pages/products/index.js`: Посадочная страница раздела Products.
- `pages/products/[product-id].js`: Страница товара. К ней обращаются по уникальному идентификатору товара, например `yoursite.com/products/123`.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Как you create dynamic routes in Next.js?</summary>

### Как you create dynamic routes in Next.js?

**Definition:**

**Динамические маршруты** в **Next.js** позволяют рендерить страницы или компоненты по параметрам URL. Это полезно для личных кабинетов, постов блога или страниц товаров, где контент привязан к конкретным динамическим URL.

**How it works:**

**Настройка маршрута**

- **Структура имени файла**:
  - Для динамических маршрутов верхнего уровня, в зависимости от того, страница это или только клиентский маршрут, используют `[param].js` или `[param].client.js` соответственно. Для параметризованных маршрутов, вложенных в каталог, формат имени файла — `[[...slug]].js`.
- **Параметры маршрута и query**:
  - Чтобы поймать параметры маршрута, используют `[param]` для одного параметра или `[[...slug]]` для цепочки параметров.
  - Кроме того, хук `useRouter` или объект `router` достаёт query-параметры, как у обычного URL.

URL `/post/abc` отрисует "Post: abc".

**Настройка URL**

- **Сгруппированные пути страниц**: Чтобы собрать набор динамических маршрутов под общим сегментом пути, опишите параметризованную страницу `pages/[category]/[id].js`:
  - Внутренние страницы вроде `/food/sandwich`, `/drinks/cola` и другие попадут в этот маршрут.
- **Shallow routing**: Shallow routing, включённый опцией `shallow` у **`router.push`** или **`next/link`**, сохраняет query-данные при переходе:
  - Например, находясь на `/view?user=48&page=1` и нажав на `<Link href='/view?user=12'>`, URL остаётся `/view?user=12&page=1`.

**Рекомендации по инструментам**

- **Visual Studio Code** с расширениями **Path Autocomplete** и **vscode-react-docgen** упрощает работу с маршрутами и query-параметрами.

**Examples:**

Вот код Next.js и React:

```jsx
// pages/post/[id].js
import { useRouter } from 'next/router';

export default function Post() {
  const router = useRouter();
  const { id } = router.query;

  return <div>Post: {id}</div>;
}
```

Вот код Next.js и React:

```jsx
// pages/view.js
import Link from 'next/link';

export default function View() {
  return (
    <div>
      <Link href={{ pathname: '/view', query: { user: 10 } }} shallow>
        <a>User 10</a>
      </Link>
    </div>
  );
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Объясните how to access URL parameters in a dynamic route.</summary>

### Объясните how to access URL parameters in a dynamic route.

**Definition:**

**Next.js** упрощает динамические маршруты и параметры URL. Ниже шаги, как прочитать параметры URL в динамическом маршруте.

**How it works:**

**1. Опишите динамический маршрут**

Например, создайте динамический маршрут `pages/post/[id].js`, где `id` — уникальный идентификатор, который вы достанете как параметр.

**2. Прочитайте параметры**

Чтобы прочитать параметр, используйте `router.query` в файле компонента.

**Случаи для динамических параметров**

- **Загрузка контента**: Удобно доставать конкретный материал из базы данных.
- **Пагинация**: Полезно листать несколько страниц данных.
- **SEO**: Даёт более чистые и описательные URL.
- **Свои структуры URL**: Подходит, чтобы собрать URL под особые требования.

**Examples:**

Вот фрагмент кода:

```javascript
import { useRouter } from "next/router";

const DynamicPost = () => {
  const router = useRouter();
  const { id } = router.query;

  return <p>Post ID: {id}</p>;
};

export default DynamicPost;
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Опишите the functionality of the Link component in Next.js.</summary>

### Опишите the functionality of the Link component in Next.js.

**Definition:**

Компонент **`Link`** в **Next.js** упрощает клиентскую навигацию внутри приложения, повышает производительность и делает работу удобнее.

**How it works:**

**Ключевые возможности**

- **Prefetching**: `Link` заранее кэширует связанные страницы, на которые пользователь, скорее всего, перейдёт дальше. Это сокращает время загрузки при навигации.
- **Доступность**: Помогает с навигационными подсказками для скринридеров и пользователей клавиатуры.
- **Code Splitting**: Next.js применяет эту стратегию по умолчанию и загружает только тот JavaScript, который нужен конкретной открытой странице.
- **Продуманная маршрутизация**: Аккуратно обрабатывает клики по ссылкам и за счёт этого даёт более гладкие переходы.

В следующем `Example.tsx` видно, как Next.js под капотом разделяет код.

Компонент `AboutUs` загружается только когда пользователь переходит на этот маршрут, и это показывает эффективное разделение кода.

**When to use:**

Компонент **`Link`** особенно хорош для переходов внутри приложения, в частности когда:

- Пользователя ведут из одного раздела приложения в другой.
- Нужна понятная реакция на действие пользователя, например экран успеха после отправки формы.

При этом он не лучший выбор, когда:

- Ссылку нужно открыть в новой вкладке или окне браузера.
- Нужно управлять динамическим состоянием или побочными эффектами.

**Examples:**

```tsx
import Link from 'next/link';

const Home = () => {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">
        <a>About Us</a>
      </Link>
      {/* Other navigational links */}
    </div>
  );
};

export default Home;
```

```tsx
const AboutUs = () => {
  return (
    <div>
      <h1>About Us</h1>
      {/* About Us content */}
    </div>
  );
}

export default AboutUs;
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Как you handle catch-all routes in Next.js?</summary>

### Как you handle catch-all routes in Next.js?

**Definition:**

**Catch-all маршруты** в **Next.js** позволяют динамически сопоставлять URL с гибкими параметрами пути и расширяют систему маршрутизации приложения.

**How it works:**

**Ключевые возможности**

- **Динамические параметры**: Сегменты-маски маршрута читают через rest-параметр `...`.
- **Порядок сопоставления**: Порядок описания задаёт приоритет маршрута.
- **Поведение fallback**: Настройки fallback можно задать дополнительно.

**Конфигурация**

Файл маршрута Next.js (`pages/foo/[...bar].js`):

- **Root Catch-All**: Срабатывает для любого несопоставленного пути или маршрута `/foo` без дальнейших подпутей.
- **Nested Catch-Alls**: Срабатывают, когда корневой путь открывают вместе с подкаталогами.
- **Fallback Pages**: Подходят для обработки ошибок или ожидания динамических данных.

**Режимы fallback**

Поведение fallback задают в методе `getStaticPaths`:

- **True**: Генерирует пути во время сборки и обрабатывает отсутствующие маршруты как fallback (только SSG).
- **False**: Точно задаёт пути страниц во время сборки (без fallback).
- **'blocking'**: Обрабатывает fallback на сервере во время выполнения (SSR или SSG).

**Examples:**

```jsx
export default function CatchAll() {
  // Logic for Root Catch-All
}
```

```jsx
export default function NestedCatchAll() {
  // Logic for Nested Catch-All
}
```

```js
export async function getServerSideProps() {
  // Data-fetching logic
  return { props: { data } };
}

export default function Fallback({ data }) {
  return <div>{data}</div>;
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

### Data Fetching and Rendering

<details>
<summary>Что такое getStaticProps and when would you use it?</summary>

### Что такое getStaticProps and when would you use it?

**Definition:**

**`getStaticProps`** в Next.js позволяет **заранее отрендерить** страницу во **время сборки**, получив данные из API, базы данных или любого другого источника.

При этом статическая страница не пересобирается, пока вы не запустите **повторный деплой**. Поэтому `getStaticProps` полезен, когда контент не нужно обновлять часто и его можно кэшировать ради производительности.

Выбирая, **когда** контент обновляется — по запросу или при пересборке, — можно оптимизировать сборку и улучшить задачи вроде **SEO**.

**When to use:**

- **Постоянство контента**: Для контента, который меняется нечасто, например поста блога, когда страницу не нужно пересобирать при каждом изменении.
- **Получение данных**: Когда данные берут из headless CMS, внутреннего API, внешних сервисов или баз данных, которые не связаны в реальном времени.
- **Производительность**: Чтобы выиграть за счёт кэша и отдавать страницы быстрее, особенно если к контенту обращаются часто.

**Examples:**

Вот код на JavaScript:

```javascript
export async function getStaticProps() {
  // Fetch data from an API
  const res = await fetch('https://example.com/data');
  const data = await res.json();

  // Pass data to the page via props
  return {
    props: { data },
    // Re-generate the page every 60 seconds (optional)
    revalidate: 60,
  };
}

export default function MyStaticPage({ data }) {
  // Render data on the page
  // ...
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Что такое getServerSideProps and what kind of rendering does it enable?</summary>

### Что такое getServerSideProps and what kind of rendering does it enable?

**Definition:**

`getServerSideProps` — функция Pages Router. Её экспортируют из страницы. Next.js вызывает её на сервере при каждом запросе и из `props` собирает HTML. Это SSR: HTML появляется в момент запроса, а не на `next build`.

**How it works:**

- Экспорт только из страницы. Функция выполняется на сервере и возвращает JSON.
- Первый HTML строится из этих данных. Те же `props` вшиты в страницу для гидрации. Секреты в `props` не кладут.
- Переход через `next/link` или `next/router` снова запускает `getServerSideProps`.
- CMS, базу и сторонний API вызывают напрямую, без API-маршрута Next.js.
- Исключение в production показывает `pages/500.js`. В разработке — оверлей.
- На `res` можно поставить `Cache-Control` (`s-maxage`, `stale-while-revalidate`). Если HTML можно кэшировать, документация советует `getStaticProps` с ISR.

**When to use:**

- Данные известны только в момент запроса: `Authorization`, геолокация.
- Для пререндера и кэша берите `getStaticProps`.

**Examples:**

```jsx
export async function getServerSideProps() {
  const res = await fetch('https://api.github.com/repos/vercel/next.js')
  const repo = await res.json()
  return { props: { repo } }
}

export default function Page({ repo }) {
  return <p>{repo.stargazers_count}</p>
}
```

В App Router тот же запрос на каждый хит — async Server Component с `cache: 'no-store'`. Экспорта `getServerSideProps` нет.

**Sources:**

- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
- [Next.js — App Router migration](https://nextjs.org/docs/app/guides/migrating/app-router-migration)
</details>
<details>
<summary>Как бы вы implement Incremental Static Regeneration (ISR) in Next.js?</summary>

### Как бы вы implement Incremental Static Regeneration (ISR) in Next.js?

**Definition:**

ISR обновляет уже собранную страницу без пересборки всего сайта. Большинство запросов отдаёт статический кэш. После окна `revalidate` следующий запрос всё ещё получает старую страницу, а Next.js пересобирает её в фоне.

**How it works:**

- Pages Router: верните `revalidate` (секунды) из `getStaticProps`. По требованию API-маршрут вызывает `res.revalidate(urlPath)`. Если нужна только инвалидация по запросу, `revalidate` не указывают: по умолчанию `false`.
- Ошибка фоновой пересборки оставляет последнюю удачную страницу. Следующий запрос снова вызывает `getStaticProps`.
- App Router: экспортируйте `revalidate` из страницы и известные параметры через `generateStaticParams`. По требованию — `revalidatePath` или `revalidateTag` из `next/cache`. В документации вызов выглядит как `revalidateTag('posts', 'max')`.
- `fetch` с `revalidate: 0` или `cache: 'no-store'` делает маршрут динамическим.
- ISR работает на runtime Node.js и не работает с `output: 'export'`. Файловый кэш свой у каждого инстанса. Несколько серверов нуждаются в общем cache handler.

**When to use:**

- Контент, которому можно быть устаревшим секунды или минуты: блог, каталог.
- Уникальный ответ на пользователя — рендер в момент запроса.

**Examples:**

```jsx
export async function getStaticProps() {
  const post = await fetch('https://api.vercel.app/blog/1').then((res) => res.json())
  return { props: { post }, revalidate: 60 }
}
```

Эквивалент в App Router:

```jsx
export const revalidate = 60

export default async function Page() {
  const posts = await fetch('https://api.vercel.app/blog').then((res) => res.json())
  return <p>{posts.length}</p>
}
```

**Sources:**

- [Next.js — ISR (Pages)](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)
- [Next.js — ISR (App Router)](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
</details>
<details>
<summary>Можете ли вы use client-side data fetching in Next.js? If so, how?</summary>

### Можете ли вы use client-side data fetching in Next.js? If so, how?

**Definition:**

Да. Клиентская загрузка идёт в браузере после монтирования компонента. Документация советует её, когда данные не нужны в первом HTML: нет SEO, нет пререндера, или экран часто обновляется.

**How it works:**

- `useEffect` вызывает `fetch` при монтировании. Сначала состояние загрузки, затем `setState` с JSON.
- Эти данные не входят в пререндер и не кэшируются так, как `getStaticProps`. Оболочка страницы может отрисоваться раньше данных.
- Для клиентских запросов документация рекомендует [SWR](https://swr.vercel.app): кэш, ревалидация, повтор при фокусе.
- В App Router у компонента нужна директива `'use client'`. Server Component не вызывает `useEffect`.

**When to use:**

- Дашборды, виджеты пользователя, данные чаще окна ISR.
- Публичный индексируемый текст оставляйте на сервере.

**Examples:**

```jsx
import { useEffect, useState } from 'react'

export function Profile() {
  const [data, setData] = useState(null)

  useEffect(() => {
    fetch('/api/profile-data')
      .then((res) => res.json())
      .then(setData)
  }, [])

  if (!data) return <p>Loading...</p>
  return <h1>{data.name}</h1>
}
```

**Sources:**

- [Next.js — Client-side fetching](https://nextjs.org/docs/pages/building-your-application/data-fetching/client-side)
</details>
<details>
<summary>Объясните the 'fallback' key used in the getStaticPaths function.</summary>

### Объясните the 'fallback' key used in the getStaticPaths function.

**Definition:**

`fallback` в объекте из `getStaticPaths` решает, что делать с динамическим путём, который не собрали на `next build`.

**How it works:**

- `false`: собираются только возвращённые `paths`. Остальное — 404. Новый путь требует нового `next build`.
- `true`: неизвестный путь не 404. Первый запрос получает fallback (`router.isFallback`), пока идёт `getStaticProps`, затем путь кэшируется. Краулеры и переходы через `next/link` / `next/router` ведут себя как `blocking`. `fallback: true` не поддерживается с `output: 'export'`.
- `'blocking'`: ответ ждёт окончания `getStaticProps`. Отдельного fallback-экрана нет.
- В App Router это `dynamicParams`. `true` (по умолчанию) генерирует неизвестные параметры по запросу, `false` даёт 404. Отдельного `'blocking'` нет: разницу закрывает стриминг.

**When to use:**

- `false` — маленький закрытый набор путей.
- `true` — нужен экран загрузки на первом визите.
- `'blocking'` — лучше подождать, чем показать fallback.

**Examples:**

```jsx
export async function getStaticPaths() {
  return {
    paths: [{ params: { id: '1' } }, { params: { id: '2' } }],
    fallback: 'blocking',
  }
}
```

**Sources:**

- [Next.js — getStaticPaths](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths)
- [Next.js — dynamicParams](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config/dynamicParams)
</details>

### API Routes

<details>
<summary>Как you create an API route in Next.js?</summary>

### Как you create an API route in Next.js?

**Definition:**

В Pages Router файл в `pages/api` становится адресом `/api/*`. Файл экспортирует обработчик `(req, res)`. Это серверный бандл, в клиентский JavaScript он не попадает.

**How it works:**

- `req` — это `http.IncomingMessage`. Помощники: `req.cookies` (по умолчанию `{}`), `req.query` (`{}`), `req.body` (разбор по `Content-Type`, иначе `null`).
- `res.status(code)`, `res.json(body)`, `res.send(body)`, `res.redirect([status,] path)` (статус по умолчанию `307`), `res.revalidate(urlPath)`.
- Метод различают по `req.method`.
- Без CORS-заголовков маршрут same-origin. Со статическим экспортом API routes не работают. На них действует `pageExtensions`.
- `config.api.bodyParser` меняет лимит (по умолчанию `1mb`). `bodyParser: false` нужен для сырого тела, например подписи вебхука.
- В App Router вместо этого Route Handler: `app/api/.../route.js` экспортирует `GET`, `POST` и возвращает `Response`.

**Examples:**

```js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello from Next.js!' })
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
- [Next.js — Dynamic Route Handlers](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)
</details>
<details>
<summary>Можете ли вы use middleware within Next.js API routes?</summary>

### Можете ли вы use middleware within Next.js API routes?

**Definition:**

API-маршрут Pages — одна функция, не стек Express. Перед ответом можно вызвать обычные функции. Отдельно Next.js запускает Proxy всего проекта (раньше файл назывался `middleware`) до рендера маршрута.

**How it works:**

- Соглашение `middleware` устарело и переименовано в `proxy`. Файл `proxy.ts` лежит рядом с `app` или `pages` и экспортирует функцию `proxy` или default export.
- Proxy умеет редирект, rewrite, смену заголовков и прямой ответ. Данные дальше передают заголовками, cookie, rewrite, редиректом или URL. Общие глобалы не используют.
- `export const config = { matcher: '/api/:path*' }` ограничивает Proxy путями API. Без matcher Proxy идёт на каждый запрос, включая `/_next/static` и `public`.
- Внутри маршрута общая проверка — обычная функция. Аргумента `next()`, как в Express, у обработчика Pages нет.

**When to use:**

- Proxy — редиректы авторизации, лог и заголовки на многих путях.
- Функция внутри обработчика — логика одного эндпоинта.

**Examples:**

```js
import { NextResponse } from 'next/server'

export function proxy(request) {
  return NextResponse.redirect(new URL('/home', request.url))
}

export const config = { matcher: '/api/:path*' }
```

**Sources:**

- [Next.js — Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
</details>
<details>
<summary>Объясните how to handle query parameters in Next.js API routes.</summary>

### Объясните how to handle query parameters in Next.js API routes.

**Definition:**

В API-маршруте Pages `req.query` — разобранная строка запроса. По умолчанию `{}`. Динамические сегменты имени файла попадают в тот же объект.

**How it works:**

- `pages/api/post/[pid].js` получает `req.query.pid` для `/api/post/abc`.
- Catch-all `pages/api/post/[...slug].js` кладёт `slug` массивом: `/api/post/a/b` → `{ slug: ['a', 'b'] }`.
- Необязательный `[[...slug]].js` матчит и путь без сегментов, тогда `req.query` равен `{}`.
- Статический файл, например `pages/api/post/create.js`, побеждает `[pid]` и `[...slug]`.
- В App Router query читают через Web API: `new URL(request.url).searchParams`. Динамические сегменты приходят промисом `params`, не через `req.query`.

**Examples:**

```js
export default function handler(req, res) {
  const { pid } = req.query
  res.status(200).send(`Post: ${pid}`)
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
</details>

### Styling in Next.js

<details>
<summary>Как you include CSS modules in your Next.js application?</summary>

### Как you include CSS modules in your Next.js application?

**Definition:**

CSS Modules — файлы `*.module.css`. Next.js делает имя класса уникальным, поэтому `.title` в двух файлах не сталкиваются.

**How it works:**

- Импорт: `import styles from './blog.module.css'`.
- Сгенерированное имя ставят в `className={styles.blog}`.
- В production стили склеиваются в разрезанные CSS-файлы. Порядок совпадает с порядком импорта. Форматтер не должен переставлять CSS-импорты, если порядок важен.
- Глобальный CSS и Tailwind импортируют из корневого layout. Модуль — для стилей одного компонента.

**Examples:**

```css
.blog {
  padding: 24px;
}
```

```jsx
import styles from './blog.module.css'

export default function Page() {
  return <main className={styles.blog} />
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
</details>
<details>
<summary>Сравните and contrast CSS-in-JS and styled-components in the context of Next.js.</summary>

### Сравните and contrast CSS-in-JS and styled-components in the context of Next.js.

**Definition:**

CSS-in-JS пишет стили на JavaScript. styled-components — одна библиотека этого семейства. Next.js также умеет CSS Modules и Tailwind без runtime CSS-in-JS. Компилятор Next.js умеет собирать styled-components, включая серверный рендер.

**How it works:**

- CSS Modules именуют классы на сборке и отдают CSS-файлы. CSS-in-JS обычно создаёт классы при выполнении компонента, поэтому нужен серверный проход, чтобы первый HTML уже содержал CSS.
- Для styled-components в `next.config` ставят `compiler.styledComponents`. В документации есть `ssr` (включён по умолчанию), `displayName` (включён в разработке, в production выключен, пока не переопределите), `minify` и `cssProp`.
- Гид по CSS по умолчанию советует Tailwind для большей части UI и CSS Modules для написанного CSS. CSS-in-JS — если команда уже стилизует этой библиотекой.

**When to use:**

- styled-components — colocated шаблоны и SSR компилятора.
- CSS Modules — статический CSS без runtime-библиотеки.

**Examples:**

```js
module.exports = {
  compiler: {
    styledComponents: { ssr: true },
  },
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
- [Next.js — compiler](https://nextjs.org/docs/architecture/nextjs-compiler)
</details>
<details>
<summary>Можно ли global styles be used in Next.js? If so, how?</summary>

### Можно ли global styles be used in Next.js? If so, how?

**Definition:**

Да. Глобальный CSS действует на всё приложение. В App Router его импортируют из корневого layout, и он приходит на каждый маршрут.

**How it works:**

- Создайте `app/global.css` и сделайте `import './global.css'` в `app/layout.js`.
- Документация разрешает такой импорт из любого layout, page или компонента в `app`. Из-за стилей React вместе с Suspense эти файлы не снимаются при навигации и могут конфликтовать. Настоящие глобальные правила (сброс, Tailwind) держите в корневом импорте, стили компонента — в CSS Modules.
- Tailwind тоже глобальный импорт: `@import 'tailwindcss'` в CSS, затем импорт файла из корневого layout.
- Пакеты вроде `import 'bootstrap/dist/css/bootstrap.css'` можно импортировать в App Router, в том числе из корневого layout.

**Examples:**

```jsx
import './global.css'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
</details>

### Performance Optimization

<details>
<summary>Что такое the Image component and how does it help optimize performance in Next.js?</summary>

### Что такое the Image component and how does it help optimize performance in Next.js?

**Definition:**

`next/image` рисует `<img>` и включает оптимизацию Next.js: размер, формат и ленивую загрузку, вместо исходного файла как есть.

**How it works:**

- Обязательные пропсы в документации: `src` (путь, внешний URL или статический импорт) и `alt`.
- `width` и `height` задают размер. `fill` растягивает картинку на родителя.
- `sizes` говорит браузеру, какую ширину брать на брейкпоинте. `quality` — целое от 1 до 100. `loading` может быть `lazy`. `placeholder="blur"` использует `blurDataURL`.
- `unoptimized` выключает оптимизацию. `preload` предзагружает картинку.
- Внешний URL всё равно должен быть разрешён конфигом картинок. `alt` не опускают.

**Examples:**

```jsx
import Image from 'next/image'

export default function Page() {
  return (
    <Image src="/profile.png" width={500} height={500} alt="Picture of the author" />
  )
}
```

**Sources:**

- [Next.js — Image](https://nextjs.org/docs/app/api-reference/components/image)
</details>
<details>
<summary>Как code splitting work in Next.js?</summary>

### Как code splitting work in Next.js?

**Definition:**

Next.js режет клиентский JavaScript по маршрутам: визит качает код этого маршрута, а не всего приложения. `next/dynamic` выносит компонент в отдельный бандл и грузит его позже.

**How it works:**

- `dynamic(() => import('../components/A'))` грузит компонент отдельным клиентским бандлом.
- Рендер только после смены состояния грузит его по требованию.
- `{ ssr: false }` пропускает серверный рендер и грузит компонент только в браузере.
- Гид по lazy loading говорит, что автоматическое разрезание не работает, когда Server Component динамически импортирует Client Component. `dynamic` ставят в Client Component (`'use client'`).

**When to use:**

- Тяжёлые виджеты (графики, редакторы), не нужные для первого кадра.
- Мелкие общие компоненты оставляют на разрезе по маршруту.

**Examples:**

```jsx
'use client'

import dynamic from 'next/dynamic'

const Chart = dynamic(() => import('../components/Chart'), { ssr: false })

export function Dashboard() {
  return <Chart />
}
```

**Sources:**

- [Next.js — Lazy loading](https://nextjs.org/docs/app/guides/lazy-loading)
</details>
<details>
<summary>Что такое the methods provided by Next.js for prefetching pages?</summary>

### Что такое the methods provided by Next.js for prefetching pages?

**Definition:**

`next/link` заранее подгружает связанный маршрут, чтобы следующий клиентский переход был быстрее. Prefetch идёт только в production, когда ссылка попадает во вьюпорт.

**How it works:**

- `prefetch` принимает `"auto"`, `null`, `true` или `false`. `"auto"` и `null` — значение по умолчанию. Статический маршрут грузится целиком. Динамический — до ближайшего сегмента с `loading.js`.
- `prefetch={true}` грузит маршрут целиком и для статики, и для динамики.
- `prefetch={false}` отключает prefetch и во вьюпорте, и при наведении.
- Если данные протухли к моменту hover, Next.js пробует снова.
- У `useRouter` из `next/navigation` тоже есть `prefetch`. При `typedRoutes: true` вызов проверяется типами. `next/router` в Pages Router так не типизируется.
- Если Proxy переписывает пользователя на другую страницу, ссылке нужны и URL на экране, и URL для prefetch, иначе запрос уйдёт не туда.

**Examples:**

```jsx
import Link from 'next/link'

export function Nav() {
  return <Link href="/dashboard" prefetch={false}>Dashboard</Link>
}
```

**Sources:**

- [Next.js — Link](https://nextjs.org/docs/app/api-reference/components/link)
</details>

### Deployment and Hosting

<details>
<summary>Что такое the benefits of deploying a Next.js application to Vercel?</summary>

### Что такое the benefits of deploying a Next.js application to Vercel?

**Definition:**

Vercel — платформа хостинга той же команды, что и Next.js. Приложение там получает серверные возможности фреймворка, и свой процесс Node вам поднимать не нужно.

**How it works:**

- Документированный старт — `create-next-app` (`yarn create next-app@latest my-app --yes` включает TypeScript, Tailwind, ESLint, App Router и Turbopack).
- То, чему нужен сервер, работает на Node.js-хосте: серверный рендер, Route Handlers и ISR. Таблица платформ в гиде ISR: Node.js-сервер и Docker — да, статический экспорт для ISR — нет.
- На Vercel этот сервер, включая фоновую пересборку, запускает платформа. Свой хост — это `next build` и `next start` (или Docker). Несколько инстансов ISR нуждаются в общем cache handler, иначе инвалидация по запросу останется на одной машине.

**When to use:**

- Vercel — когда сервер Next.js должна вести платформа.
- Node или Docker — когда приложение должно жить на вашей инфраструктуре. ISR там тоже работает.

**Sources:**

- [Next.js — Installation](https://nextjs.org/docs/app/getting-started/installation)
- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
</details>
<details>
<summary>Как you configure custom domain names when deploying a Next.js application?</summary>

### Как you configure custom domain names when deploying a Next.js application?

**Definition:**

Свой домен — это DNS и настройка хоста. Next.js домены не выдаёт. Хост, который отдаёт деплой, связывает имя с приложением.

**How it works:**

- На Vercel домен добавляют в настройках проекта и направляют DNS на записи, которые показывает Vercel. Код приложения не меняется.
- На своём сервере обратный прокси или балансировщик завершает TLS для этого имени и проксирует на процесс `next start`.
- Абсолютные URL и callback внутри приложения должны использовать публичный origin. Берите хост запроса или переменную окружения этого деплоя. Если задаёте metadata, `metadataBase` держите на том же origin.
- Интернациональные хосты и заголовки безопасности отдельно: `headers()` в `next.config` зависит от пути, а Pages `i18n` может добавить префикс локали к этим source.

**Sources:**

- [Next.js — headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — ISR platform support](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
</details>
<details>
<summary>Объясните how to deploy a Next.js app using a different hosting service, such as AWS or Netlify.</summary>

### Объясните how to deploy a Next.js app using a different hosting service, such as AWS or Netlify.

**Definition:**

Любой хост с Node.js может отдать production-сборку Next.js. Статический экспорт уже: он отбрасывает то, чему нужен сервер.

**How it works:**

- Сборка — `next build`. Старт — `next start` (или `node server.js`, только если вы сами ведёте custom server). Для старта ставьте `NODE_ENV=production`, как в гиде custom server.
- Гид ISR: сервер Node.js и контейнер Docker поддерживают ISR, статический экспорт — нет. Адаптеры зависят от платформы.
- Файл custom server не трассируется режимом `output: 'standalone'`. Standalone пишет свой `server.js`. Эти два режима вместе не используют.
- Процесс или контейнер ставят за TLS. Несколько инстансов ISR нуждаются в общем cache handler. Пока кэш — локальная файловая система, инвалидация по запросу чистит только тот инстанс, который принял вызов.

**Examples:**

```json
{
  "scripts": {
    "build": "next build",
    "start": "next start"
  }
}
```

**Sources:**

- [Next.js — Custom server](https://nextjs.org/docs/pages/guides/custom-server)
- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
</details>

### Integrations and Plugins

<details>
<summary>Как you integrate a CSS framework like Tailwind CSS into a Next.js project?</summary>

### Как you integrate a CSS framework like Tailwind CSS into a Next.js project?

**Definition:**

Текущий гид по CSS ставит Tailwind CSS v4 плагином PostCSS `@tailwindcss/postcss` и импортирует его из глобальной таблицы стилей.

**How it works:**

- `npm install -D tailwindcss @tailwindcss/postcss`
- В `postcss.config.mjs`: `plugins: { '@tailwindcss/postcss': {} }`
- В глобальном CSS: `@import 'tailwindcss';`
- Этот CSS импортируют из корневого layout. Дальше работают классы вроде `className="text-4xl font-bold"`.
- `create-next-app --yes` уже включает Tailwind.
- Старый массив `content` (`./app/**/*.{js,ts,jsx,tsx}`) есть в гиде миграции на App Router для прежней настройки Tailwind. Гид v4 выше этот массив не использует.

**Examples:**

```js
export default {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
- [Next.js — App Router migration](https://nextjs.org/docs/app/guides/migrating/app-router-migration)
</details>
<details>
<summary>Что такое some popular plugins for Next.js and what functionalities do they add?</summary>

### Что такое some popular plugins for Next.js and what functionalities do they add?

**Definition:**

Внутри фреймворка нет реестра плагинов. Расширение — это npm-пакеты и поля `next.config`. Компилятор и TypeScript — встроенные точки, которые описывает документация.

**How it works:**

- `compiler.styledComponents` включает трансформацию styled-components (`ssr`, `displayName`, `minify` и остальные флаги из справки компилятора).
- TypeScript встроен. `next dev` / `next build` ставят типы и пишут `tsconfig.json`, когда появляется файл `.ts` или `.tsx`. Плагин редактора предупреждает о неверном segment config и об ошибках `'use client'`.
- `typedRoutes: true` генерирует типы маршрутов для `next/link` и методов `next/navigation`.
- Tailwind подключается как PostCSS, не как объект плагина Next.js. См. гид по CSS.
- MDX, авторизация и похожие инструменты — отдельные пакеты. Берите пакет, чья документация совпадает с вашей версией Next.js. Массива `plugins: []` в документации нет. Объект конфига — `NextConfig`.

**Sources:**

- [Next.js — compiler](https://nextjs.org/docs/architecture/nextjs-compiler)
- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Как you enable TypeScript support in a Next.js project?</summary>

### Как you enable TypeScript support in a Next.js project?

**Definition:**

У Next.js TypeScript встроен. `create-next-app` настраивает его сам. Существующее JavaScript-приложение включается переименованием файла в `.ts` или `.tsx`.

**How it works:**

- Запустите `next dev` или `next build`. Next.js поставит зависимости и добавит `tsconfig.json` с рекомендованными опциями.
- Если уже есть `jsconfig.json`, скопируйте `paths` в `tsconfig.json` и удалите `jsconfig.json`.
- `next-env.d.ts` генерируется и должен остаться в `include`. Его не редактируют. Документация говорит добавить его в gitignore.
- `next build` падает на ошибках типов, пока `typescript.ignoreBuildErrors` не равен `true`. Документация помечает этот флаг как опасный. В CI лучше `tsc --noEmit`.
- Для TypeScript 7 поставьте в проект `typescript@^7`. Next.js берёт локальный CLI `tsc`. `experimental.useTypeScriptCli: false` переключает на JavaScript compiler API.

**Examples:**

```bash
npm install -D typescript@^7
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>

### Advanced Features

<details>
<summary>Как you implement a custom server in Next.js and why would you do so?</summary>

### Как you implement a custom server in Next.js and why would you do so?

**Definition:**

Обычный production-сервер — `next start`. Custom server сам вызывает `next()` и отдаёт запросы в `app.getRequestHandler()`. Документация разрешает это только когда встроенный роутер не закрывает задачу.

**How it works:**

- `next({ dev, hostname, port, dir, conf, quiet, turbopack, webpack, httpServer })`. `dev` по умолчанию `false`, `dir` — `'.'`, Turbopack включён по умолчанию.
- Сначала `app.prepare()`, затем `createServer((req, res) => handle(req, res))`.
- Свои заголовки ставят до `handle`. После начала ответа `res.headersSent` уже true, и поздний `res.setHeader` отбрасывается.
- `server.js` компилятор Next.js не обрабатывает. Файл должен выполняться вашей версией Node.
- В `package.json` скрипты `dev` и `start` запускают `node server.js`. `build` остаётся `next build`.
- `useFileSystemPublicRoutes: false` отключает SSR путей из имён файлов `pages`, когда пути принадлежат вашему серверу. Клиентская навигация всё ещё может туда попасть, поэтому такие маршруты закрывают отдельно.
- `output: 'standalone'` не трассирует custom server и пишет другой `server.js`. Оба режима вместе не используют.

**When to use:**

- Next.js нужно посадить на уже существующий HTTP-сервер, и встроенного роутера мало.
- Соседний бэкенд может работать рядом с Next.js и без custom server. Гид говорит об этом прямо.

**Examples:**

```js
import { createServer } from 'http'
import next from 'next'

const port = parseInt(process.env.PORT || '3000', 10)
const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare().then(() => {
  createServer((req, res) => {
    handle(req, res)
  }).listen(port)
})
```

**Sources:**

- [Next.js — Custom server](https://nextjs.org/docs/pages/guides/custom-server)
</details>
<details>
<summary>Что такое the considerations when setting up internationalized routing in Next.js?</summary>

### Что такое the considerations when setting up internationalized routing in Next.js?

**Definition:**

В Pages Router `i18n` в `next.config` задаёт `locales` и `defaultLocale`. Next.js тогда добавляет локаль в путь. App Router этим объектом так не пользуется: локаль живёт в сегменте, обычно `[lang]`, и читается из `params`.

**How it works:**

- С Pages `i18n` source в `headers()` матчит все настроенные локали, пока не поставите `locale: false` и не напишете префикс сами.
- Локаль по умолчанию отдаётся и с `/`, и со своим префиксом — зависит от matcher. В справке headers путь `/en` совпадает с `/`, если `en` — `defaultLocale` и стоит `locale: false`.
- Source `/(.*)` превращается в `/(en|fr|de)/(.*)` и не матчит голый `/` так, как `/:path*`. Проверяйте оба.
- Одна локаль в URL, чтобы ссылки, `getStaticPaths` / `generateStaticParams` и кэш не смешивали языки.
- Proxy может редиректить `/` на локаль. Документация держит Proxy редиректом или rewrite, а не местом общей логики рендера.

**When to use:**

- Pages `i18n` — приложение ещё на Pages Router.
- Сегмент `[lang]` — новые приложения на App Router, `generateStaticParams` возвращает каждую локаль.

**Examples:**

```js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },
}
```

**Sources:**

- [Next.js — headers and i18n](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
</details>
<details>
<summary>Как you handle custom error pages in a Next.js application?</summary>

### Как you handle custom error pages in a Next.js application?

**Definition:**

В App Router отсутствующий ресурс рисует `not-found.js`. Его вызывают через `notFound()` из страницы. Исключение в Pages `getServerSideProps` в production показывает `pages/500.js`.

**How it works:**

- `app/not-found.js` экспортирует компонент. У стримингового ответа документация допускает HTTP 200, а нестриминговый 404 остаётся 404.
- `experimental.globalNotFound: true` в `next.config` включает общий 404, который обходит обычный рендер маршрута. Флаг экспериментальный.
- Ошибки Pages `getServerSideProps` идут в `pages/500.js`. Во время `next dev` этот файл не используется, виден оверлей.
- Ошибка пересборки ISR не подменяет последнюю удачную страницу. Следующий запрос пробует снова.

**Examples:**

```jsx
import Link from 'next/link'

export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <Link href="/">Return Home</Link>
    </div>
  )
}
```

**Sources:**

- [Next.js — not-found](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
</details>

### Best Practices

<details>
<summary>Какая folder structure рекомендуется для large-scale Next.js applications?</summary>

### Какая folder structure рекомендуется для large-scale Next.js applications?

**Definition:**

App Router — это файловая маршрутизация в `app/`. Маршрут — папка с `page.tsx`. Общий UI может лежать рядом в файлах, которые не являются маршрутами. Динамический сегмент пишут в скобках: `app/products/[id]/page.tsx`.

**How it works:**

- `app/layout.tsx` оборачивает дочерние маршруты. `app/page.tsx` — это `/`.
- Маршрутами становятся только файлы-соглашения: `page`, `layout`, `route`, `not-found`, `loading` и остальные из документации. Папка `components` рядом URL не добавляет.
- `generateStaticParams` живёт в динамическом `page` или `route`, которому принадлежит сегмент.
- `proxy.ts` остаётся в корне проекта (или в `src`), рядом с `app` или `pages`, не внутри папки маршрута.
- Аналог Pages Router — каталог `pages/`. Два дерева на один и тот же URL не смешивают.

**Examples:**

```txt
app/
├── layout.tsx
├── page.tsx
└── products/
    └── [id]/
        └── page.tsx
```

**Sources:**

- [Next.js — Building](https://nextjs.org/docs/app/guides/building)
</details>
<details>
<summary>Как следует you structure components in a Next.js application for optimal code reusability?</summary>

### Как следует you structure components in a Next.js application for optimal code reusability?

**Definition:**

В `app` по умолчанию Server Components. Они могут быть async и сами загружать данные. Компоненту со state или эффектами нужна директива `'use client'` в начале файла. Общие куски переиспользуют импортом, а не копией fetch в каждую страницу.

**How it works:**

- Общий Server Component кладут, например, в `components/` и импортируют из нескольких `page.tsx`. Файл не становится маршрутом, пока это не файл-соглашение.
- `'use client'` опускают к листу, которому нужна интерактивность, чтобы родитель остался Server Component.
- Тяжёлые клиентские листья грузят через `next/dynamic` из Client Component.
- Общие пропсы маршрута типизируют сгенерированными `PageProps` и `LayoutProps`, а не копируют типы параметров.

**Examples:**

```jsx
export function Price({ cents }) {
  return <p>{(cents / 100).toFixed(2)}</p>
}
```

**Sources:**

- [Next.js — Lazy loading](https://nextjs.org/docs/app/guides/lazy-loading)
- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Что такое some security best practices you should follow in a Next.js app?</summary>

### Что такое some security best practices you should follow in a Next.js app?

**Definition:**

Props, которые уходят в HTML, и публичные переменные окружения браузер видит. Секреты остаются на сервере. Нужные политики добавляют заголовками ответа.

**How it works:**

- `props` из `getServerSideProps` есть в HTML ради гидрации. Документация запрещает класть туда чувствительные данные.
- Server Component может читать секрет, пока выполняется на сервере. Этот модуль не импортируют в Client Component.
- `headers()` в `next.config` ставит заголовки ответа по `source`. Туда кладут Content-Security-Policy и соседние политики. С Pages `i18n` source матчит локали, пока не стоит `locale: false`.
- API-маршруты same-origin, пока вы не добавите CORS. Для вебхука парсер тела выключают и проверяют сырое тело.
- ISR по требованию сверяет секрет до `res.revalidate` или `revalidatePath`. Пример Pages сравнивает `req.query.secret` с `process.env.MY_SECRET_TOKEN` и иначе отвечает 401.
- Proxy — для редиректов и заголовков, не для хранения сессии в глобальной переменной.

**Examples:**

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [{ key: 'X-Content-Type-Options', value: 'nosniff' }],
      },
    ]
  },
}
```

**Sources:**

- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
- [Next.js — headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
</details>

### State Management in Next.js

<details>
<summary>Как you manage global state in a Next.js application without using external libraries?</summary>

### Как you manage global state in a Next.js application without using external libraries?

**Definition:**

Для клиентского состояния, которое читают много компонентов, хватает контекста React и `useState`. Server Components это состояние не держат: они рендерятся на сервере на запрос.

**How it works:**

- Client Component вызывает `useState` и отдаёт значение через `React.createContext`. Экспортируют провайдер и хук.
- Провайдер рендерят в корневом layout вокруг `{children}`, чтобы навигация под этим layout не сбрасывала клиентское состояние.
- Нельзя загрузить приватные данные в контекст при SSR и считать их секретом. Всё, что передано в Client Component, сериализуется в браузер.
- Данные, которые должны обновляться на каждый запрос, читают в Server Component и передают обычным пропом. Это не глобальное клиентское состояние.

**When to use:**

- Тема, выбор локали или небольшая корзина в рамках вкладки.
- То, что должно пережить перезагрузку, — база или кэш ISR.

**Examples:**

```jsx
'use client'

import { createContext, useContext, useState } from 'react'

const ThemeContext = createContext('light')

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light')
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export function useTheme() {
  return useContext(ThemeContext)
}
```

**Sources:**

- [Next.js — Lazy loading and Client Components](https://nextjs.org/docs/app/guides/lazy-loading)
- [React — createContext](https://react.dev/reference/react/createContext)
</details>
<details>
<summary>Когда should you consider using a state management library like Redux in a Next.js project?</summary>

### Когда should you consider using a state management library like Redux in a Next.js project?

**Definition:**

Клиентское хранилище добавляют, когда много далёких Client Components меняют одно состояние и прокидывание пропов или один контекст уже трудно сопровождать. Next.js Redux не требует.

**How it works:**

- Данные серверного рендера остаются в Server Components, `fetch` и кэше (`revalidate`, `revalidatePath`). Копия в Redux создаёт второй источник правды.
- Хранилище работает в браузере. Провайдер помечают `'use client'` и монтируют в корневом layout, если он нужен всему приложению.
- Контекст — встроенный вариант для одного значения, например темы. Библиотека окупается, когда много независимых срезов, middleware или отладка истории действий.
- Секреты и токен одного запроса в хранилище, которое уходит клиенту, не кладут.

**When to use:**

- Большой интерактивный клиент, чьи обновления не ложатся на URL или серверный fetch.
- Для маркетинговой страницы с ISR или Server Component хранилище не нужно.

**Sources:**

- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
- [React — createContext](https://react.dev/reference/react/createContext)
</details>
<details>
<summary>Объясните how to persist state between page transitions in a Next.js app.</summary>

### Объясните how to persist state between page transitions in a Next.js app.

**Definition:**

Клиентский переход через `next/link` не перезагружает документ. Состояние React в layout, который остаётся смонтированным, переживает переход. Полная загрузка стирает память. Через загрузки состояние держат в `localStorage` или на сервере.

**How it works:**

- Провайдер в корневом layout не размонтируется, когда меняется дочерний `page.tsx`, поэтому его `useState` сохраняется.
- Состояние внутри `page.tsx` пропадает, когда страница размонтируется. Если оно должно жить дальше, его поднимают в layout.
- `localStorage` доступен только после монтирования. Читают его в `useEffect`, чтобы серверный рендер и первый клиентский совпали.
- Данные нескольких устройств живут в базе и Server Component, не в состоянии компонента.
- Prefetch не выполняет вашу логику состояния. Он грузит маршрут. См. проп `prefetch` у `Link`.

**Examples:**

```jsx
'use client'

import { useEffect, useState } from 'react'

export function DraftName() {
  const [name, setName] = useState('')

  useEffect(() => {
    const saved = localStorage.getItem('draft-name')
    if (saved) setName(saved)
  }, [])

  return (
    <input
      value={name}
      onChange={(event) => {
        setName(event.target.value)
        localStorage.setItem('draft-name', event.target.value)
      }}
    />
  )
}
```

**Sources:**

- [Next.js — Link](https://nextjs.org/docs/app/api-reference/components/link)
- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
</details>

### Next.js and TypeScript

<details>
<summary>Что такое the advantages of using TypeScript in a Next.js project?</summary>

### Что такое the advantages of using TypeScript in a Next.js project?

**Definition:**

TypeScript проверяет пропсы, параметры маршрута и конфиг до того, как `next build` отдаст приложение. Next.js добавляет помощники маршрутов и плагин редактора поверх компилятора.

**How it works:**

- `next build` запускает проверку типов и падает на ошибках (`typescript.ignoreBuildErrors` отключает её и в документации помечен как опасный).
- Плагин редактора предупреждает о неверном segment config и о клиентских хуках без `'use client'`.
- Server Components в App Router могут передавать `Date`, `Map` и `Set` без шага JSON-сериализации Pages Router. Результат `fetch` типизируют обычным TypeScript.
- `PageProps`, `LayoutProps` и `RouteContext` генерируются и глобальны. Их пишут `next typegen`, `next dev` и `next build`.
- `typedRoutes: true` проверяет `href` у `next/link` и `push` / `replace` / `prefetch` из `next/navigation`.

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Как you add type definitions for a Next.js page's props?</summary>

### Как you add type definitions for a Next.js page's props?

**Definition:**

В App Router страницу типизируют сгенерированным `PageProps` или явным промисом `params`. В Pages Router используют `GetServerSideProps` / `GetStaticProps` и `InferGetServerSidePropsType`.

**How it works:**

- `PageProps`, `LayoutProps` и `RouteContext` генерируются, импорт не нужен.
- Динамические `params` в App Router — промис: `params: Promise<{ id: string }>`, затем `const { id } = await params`.
- Pages `getServerSideProps` типизируют как `GetServerSideProps<{ repo: Repo }>`. Пропсы страницы берут через `InferGetServerSidePropsType<typeof getServerSideProps>`.
- Тело `NextApiRequest` имеет тип `any`. Его проверяют в runtime, прежде чем использовать.

**Examples:**

```tsx
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  return <p>{id}</p>
}
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
</details>
<details>
<summary>Какие шаги нужны для миграции existing Next.js project на TypeScript?</summary>

### Какие шаги нужны для миграции existing Next.js project на TypeScript?

**Definition:**

Файлы JavaScript переименовывают в `.ts` и `.tsx`, затем Next.js создаёт `tsconfig.json`. Алиасы путей переносят, ошибки `next build` исправляют.

**How it works:**

- Переименуйте файл в `.ts` или `.tsx`. Запустите `next dev` или `next build`. Next.js поставит зависимости и запишет рекомендованный `tsconfig.json`.
- Скопируйте `compilerOptions.paths` из `jsconfig.json` и удалите `jsconfig.json`.
- В `include` держите `next-env.d.ts` и `.next/types/**/*.ts`. Свои объявления модулей кладите в отдельный `.d.ts`. `next-env.d.ts` не редактируют.
- Типизируйте страницы и обработчики по мере правок. `next build` падает, пока проект не пройдёт проверку, если не включён `typescript.ignoreBuildErrors`.
- Async Server Components требуют TypeScript 5.1.3+ и `@types/react` 18.2.8+. Иначе типы JSX отвергают возвращённый `Promise`.
- Инкрементальная проверка доступна, когда её включает `tsconfig` (поддержка с Next.js 10.2.1).

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Можете ли вы describe how to use TypeScript for defining API route handlers in Next.js?</summary>

### Можете ли вы describe how to use TypeScript for defining API route handlers in Next.js?

**Definition:**

API-маршруты Pages типизируют обработчик типами `NextApiRequest` и `NextApiResponse<Data>` из `next`. Route Handler в App Router использует Web `Request` и сгенерированный `RouteContext` для динамических параметров.

**How it works:**

- `NextApiResponse<ResponseData>` проверяет объект, который уходит в `res.json`.
- `req.body` остаётся `any`. Поля проверяют до использования.
- Динамический обработчик App Router: `GET(request: Request, context: RouteContext<'/api/posts/[id]'>)`. `params` нужно `await` (в документации его достают из аргумента).
- `generateStaticParams` может пререндерить такие обработчики. Возвращённые id статичны. Остальные выполняются в момент запроса.

**Examples:**

```ts
import type { NextApiRequest, NextApiResponse } from 'next'

type ResponseData = { message: string }

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>
) {
  res.status(200).json({ message: 'Hello from Next.js!' })
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
- [Next.js — Dynamic routes](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)
</details>
<details>
<summary>Как you enforce type checking for dynamic routes in Next.js using TypeScript?</summary>

### Как you enforce type checking for dynamic routes in Next.js using TypeScript?

**Definition:**

`typedRoutes: true` разрешает `href` у `next/link` и `router.push`, `replace`, `prefetch` в App Router только для существующих маршрутов. Динамические сегменты входят в этот сгенерированный тип.

**How it works:**

- В `next.config.ts` ставят `typedRoutes: true`. Проект должен быть на TypeScript.
- `next dev`, `next build` или `next typegen` пишут `.next/types`. Если проект создан не через `create-next-app`, добавьте `.next/types/**/*.ts` в `include`.
- Строковые литералы проверяются (`href="/aboot"` — ошибка). Нелитерал нужно привести `as Route` из `next`.
- `next/router` в Pages Router так не типизируется.
- Параметры обработчика динамического маршрута типизируют `RouteContext<'/api/posts/[id]'>`. У страницы `params` можно описать как `Promise<{ id: string }>`. `PageProps` — сгенерированный глобальный помощник страниц.

**Examples:**

```ts
import type { NextConfig } from 'next'

const nextConfig: NextConfig = { typedRoutes: true }

export default nextConfig
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Как Next.js handle automatic type generation for pages and API routes when using TypeScript?</summary>

### Как Next.js handle automatic type generation for pages and API routes when using TypeScript?

**Definition:**

Next.js генерирует типы маршрутов во время `next dev`, `next build` или `next typegen`. `PageProps`, `LayoutProps` и `RouteContext` импортировать не нужно: это глобальные помощники.

**How it works:**

- `next-env.d.ts` подключает типы Next.js, чтобы импорты CSS и картинок проходили проверку. Файл перезаписывается. Его не редактируют и держат в `include`.
- `.next/types` хранит карту маршрутов для `typedRoutes` и помощников сегментов.
- `typescript.tsconfigPath` выбирает, какой `tsconfig` уходит в проверку. В разработке следят только за `tsconfig.json`. После правки другого пути перезапустите `next dev`.
- `experimental.typedEnv: true` генерирует типы редактора для переменных, загруженных в разработке. Файлы `.env.production*` туда не входят, пока `next dev` не запущен с `NODE_ENV=production`.
- С проверкой через CLI TypeScript 7 диагностика — это вывод `tsc`. Next.js не переписывает её в кадры маршрутов, пока `experimental.useTypeScriptCli` не равен `false`.

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>
<details>
<summary>Что такое the role of the tsconfig.JSON file in a Next.js project with TypeScript, and how do you customize it for your specific needs?</summary>

### Что такое the role of the tsconfig.JSON file in a Next.js project with TypeScript, and how do you customize it for your specific needs?

**Definition:**

`tsconfig.json` — проект TypeScript, который проверяет Next.js. Его пишет `create-next-app`. `next dev` пишет его при первом файле `.ts`.

**How it works:**

- В `include` должны быть `next-env.d.ts`, `.next/types/**/*.ts` и ваш код. В `exclude` обычно `node_modules`.
- Свои ambient-типы кладут в отдельный `.d.ts` и добавляют этот файл в `include`.
- `compilerOptions.paths` — ваши алиасы. При миграции их копируют из `jsconfig.json`.
- Инкрементальная проверка соблюдается, если её включает `tsconfig`.
- Чтобы в CI проверять другим файлом, задайте `typescript.tsconfigPath` (например `tsconfig.build.json`). IDE по-прежнему читает `tsconfig.json`.
- `typescript.ignoreBuildErrors: true` пропускает проверку на `next build`. Если флаг включён, запускайте `tsc --noEmit` сами.

**Examples:**

```json
{
  "include": [
    "next-env.d.ts",
    ".next/types/**/*.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": ["node_modules"]
}
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
</details>

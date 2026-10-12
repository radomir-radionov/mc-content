# Express.js — Interview

Вопросы для собеседования по Express.js.

[ ← Вернуться в хаб](./8_nodejs.md)

### Related docs

- [Node.js — HTTP](./8_nodejs_http.md) · [Express.js — Интервью](./8_express_interview.md) · [Node.js — Интервью](./8_nodejs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — Express.js](https://devinterview.io/questions/web-and-mobile-development/express-interview-questions/).

### Express.js Fundamentals

<details>
<summary>Что такое Express.js, and how does it relate to Node.js?</summary>

### Что такое Express.js, and how does it relate to Node.js?

**Definition:**

Express.js — это веб-фреймворк, который работает на Node.js. Он упрощает процесс создания веб-приложений и API-интерфейсов, предоставляя ряд мощных функций, включая надежную маршрутизацию, поддержку middleware и служебные методы HTTP. Благодаря модульной конструкции вы можете расширить его функциональность за счет дополнительных библиотек и модулей Node.js.

**How it works:**

#### Ключевые особенности

Middleware: Express.js использует функции middleware, которые имеют доступ к циклу запрос-ответ. Это позволяет выполнять различные операции, такие как ведение журнала, аутентификация и анализ данных.

Маршрутизация. Платформа предлагает гибкую и интуитивно понятную систему маршрутизации, упрощающую обработку различных методов HTTP-запросов по различным URL-адресам.

Шаблоны: встроенная поддержка механизмов шаблонов обеспечивает динамическую визуализацию HTML-содержимого.

Методы HTTP: он предоставляет встроенные методы для всех HTTP-запросов, таких как get, post, put, delete, что упрощает обработку запросов.

Обработка ошибок: Express упрощает управление ошибками, а функции middleware могут специально обрабатывать ошибки.

API-интерфейсы RESTful: такие функции, как связывание объектов запросов и ответов, а также поддержка методов HTTP, делают его идеальным для создания API-интерфейсов RESTful.

#### Связь с Node.js

Express.js — это платформа веб-приложений, специально разработанная для расширения возможностей Node.js для веб-разработки. Node.js, с другой стороны, представляет собой кроссплатформенную среду выполнения JavaScript, которая позволяет разработчикам создавать серверные и сетевые приложения.

Express.js достигает этого посредством уровня абстракций и более структурированного подхода, который Node.js сам по себе не предоставляет «из коробки».

#### Пример кода: базовый экспресс-сервер

Вот код Node.js:

**Examples:**

```javascript
// Import required modules
const express = require('express');

// Create an Express application
const app = express();
const port = 3000;

// Define a route and its callback function
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start the server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Объясните the concept of middleware in Express.js.</summary>

### Объясните the concept of middleware in Express.js.

**Definition:**

Middleware действует как мост между входящими HTTP-запросами и вашим приложением Express.js, позволяя выполнять ряд операций, таких как анализ тела запроса, обработка аутентификации и даже обслуживание статических файлов.

**How it works:**

#### Функции middleware

Функция middleware в Express — это обработчик, вызываемый последовательно при получении HTTP-запроса. Он имеет доступ к объектам запроса и ответа, а также к следующей функции для запуска следующего middleware.

Каждая функция middleware обычно соответствует этой сигнатуре:

Обратите внимание, что вызов next() необходим для перехода к следующему middleware.

#### Типы middleware

#### Middleware уровня приложения

Зарегистрированный через app.use(middlewareFunction), он активен для каждого входящего запроса, что делает его пригодным для таких задач, как ведение журнала запросов или определение сквозных проблем.

#### Middleware уровня маршрутизатора

Работает на определенных путях маршрутизатора и определяется с помощью router.use(middlewareFunction). Это полезно для задач, связанных с определенными наборами маршрутов.

Middleware для обработки ошибок

#### Встроенное middleware

Express предлагает готовое к использованию middleware для таких задач, как обслуживание статических файлов или анализ тела запроса.

#### Цепочка middleware

Последовательно вызывая next() внутри каждого middleware, вы формируете цепочку, облегчающую каскад операций для входящего запроса.

Рассмотрите многоуровневую настройку безопасности, например, с аутентификацией, авторизацией и проверкой запроса. Только когда запрос пройдет через все три уровня, он будет обработан фактическим обработчиком маршрута.

#### Пример кода: цепочка middleware

Вот код:

**Examples:**

```javascript
function middlewareFunction(req, res, next) {
    // ...middleware logic
    next(); // or next(err); based on whether to proceed or handle an error
}
```

```javascript
Recognizable via its function signature (err, req, res, next), this type of middleware specifically handles errors. In the middleware chain, it should be placed after regular middlewares and can be added using app.use(function(err, req, res, next) { ... }).
```

```javascript
const express = require('express');
const app = express();

// Sample middleware functions
function authenticationMiddleware(req, res, next) {
    console.log('Authenticating...');
    next();
}

function authorizationMiddleware(req, res, next) {
    console.log('Authorizing...');
    next();
}

function requestValidationMiddleware(req, res, next) {
    console.log('Validating request...');
    next();
}

// The actual route handler
app.get('/my-secured-endpoint', authenticationMiddleware, authorizationMiddleware, requestValidationMiddleware, (req, res) => {
    res.send('Welcome! You are authorized.');
});

app.listen(3000);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как бы вы set up a basic Express.js application?</summary>

### Как бы вы set up a basic Express.js application?

**Definition:**

Чтобы настроить базовое приложение Express.js, выполните следующие действия:

**How it works:**

#### 1. Инициализируйте проект

Создайте новый каталог для своего проекта и запустите npm init, чтобы сгенерировать файл package.json.

#### 2. Установите зависимости

Установите Express как зависимость с помощью Node Package Manager (NPM):

#### 3. Создайте приложение

В каталоге вашего проекта создайте основной файл (обычно с именем app.js или index.js) для настройки приложения Express.

Вот код JavaScript:

#### 4. Запустите приложение

Вы можете запустить свой экспресс-сервер с помощью Node.js:

Для удобства вы можете рассмотреть возможность использования Nodemon в качестве зависимости разработки, которая автоматически перезапускает сервер при изменении файла.

**Examples:**

```bash
npm install express
```

```javascript
// Import the Express module
const express = require('express');

// Create an Express application
const app = express();

// Define a sample route
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

```bash
node app.js
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Что такое the puRPOse of the app.use() function?</summary>

### Что такое the puRPOse of the app.use() function?

**Definition:**

В Express.js функция app.use() — мощный инструмент управления middleware. Она может обрабатывать HTTP-запросы и ответы, а также подготавливать данные или выполнять промежуточные процессы.

**How it works:**

#### Ключевые функции

Глобальное middleware: без указания пути middleware будет обрабатывать каждый запрос.
Middleware, специфичное для маршрута: если указан путь, middleware будет применяться только к совпадающим маршрутам.

#### Цепочка middleware

Вы можете объединить несколько middleware с помощью app.use() в том порядке, в котором они должны выполняться. Для совпавшего маршрута управление может быть передано следующему совпадающему маршруту или прекращено досрочно с помощью next().

**When to use:**

#### Распространенные случаи использования

Анализ тела: для извлечения данных из входящих запросов, особенно полезно для запросов POST и PUT.

Обработка CORS: полезно в приложениях API для управления запросами между источниками.

Статическое обслуживание файлов: для обслуживания таких файлов, как изображения, CSS или клиентский JavaScript.

Ведение журнала: для записи деталей запроса для отладки или анализа.

Обработка ошибок: для управления ошибками во время обработки запросов и сообщения о них.

**Examples:**

```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```

```javascript
app.use(function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
});
```

```javascript
app.use(express.static('public'));
```

```javascript
app.use(function(req, res, next) {
    console.log(`${new Date().toUTCString()}: ${req.method} ${req.originalUrl}`);
    next();
});
```

```javascript
app.use(function(err, req, res, next) {
    console.error(err);
    res.status(500).send('Internal Server Error');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как you serve static files using Express.js?</summary>

### Как you serve static files using Express.js?

**Definition:**

В веб-приложении Express.js часто требуется обслуживать статические файлы, такие как таблицы стилей, клиентский JavaScript и изображения. Вы можете сделать это с помощью middleware express.static.

**How it works:**

#### Middleware для обслуживания статических файлов

Функция middleware express.static обслуживает статические файлы и обычно используется для обслуживания таких ресурсов, как изображения, CSS и клиентский JavaScript.

Вот пример кода:

В этом примере папка с именем public будет использоваться для обслуживания статических ресурсов.

#### Дополнительная конфигурация с цепочкой методов

Вы можете дополнительно настроить поведение middleware express.static, связав методы.

Например, чтобы установить заголовок управления кэшем, код выглядит следующим образом:

Здесь «1d» гарантирует, что кэширование включено на один день.

#### Использование подкаталога

Если вы хотите обслуживать файлы из подкаталога, вы можете указать его при использовании middleware express.static.

Вот пример кода:

При этом обслуживаются файлы из общей папки, но любые запросы к этим файлам должны начинаться с /static.

Что обслуживает express.static
Изображения: PNG, JPEG, GIF
Текстовый контент: HTML, CSS, JavaScript.
Шрифты

#### Данные JSON

#### Не для динамического контента

Хотя express.static отлично подходит для статических ресурсов, он не подходит для динамического контента или данных в запросах POST.

**Examples:**

```javascript
app.use(express.static('public'));
```

```javascript
app.use(express.static('public', {
    maxAge: '1d'
}));
```

```javascript
app.use('/static', express.static('public'));
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Обсудите the difference between app.get() and app.post() in Express.js.</summary>

### Обсудите the difference between app.get() and app.post() in Express.js.

**Definition:**

В Express.js app.get() и app.post() являются двумя наиболее часто используемыми middleware HTTP-метода. Выбор между ними (или использованием обоих) обычно зависит от того, извлекаете ли вы или отправляете/сохраняете данные.

**How it works:**

#### Ключевые отличия

#### HTTP-глаголы: внешняя видимость

app.get(): слушает запросы GET. Предназначен для поиска данных. Видимые URL-адреса обычно вызывают такие запросы (например, ссылки или прямой ввод URL-адреса в браузере).

app.post(): прослушивает POST-запросы. Предназначен для подачи данных. Обычно не отображается в строке URL-адреса и обычно используется для отправки форм.

#### Передача данных

app.get(): использует параметры запроса для передачи данных, видимые в URL-адресе. Полезно для простых, неконфиденциальных данных, доступных только для чтения (например, фильтрация или разбиение на страницы).

app.post(): использует тело запроса для передачи данных, которые могут быть в различных форматах (например, JSON, данные формы). Идеально подходит для более сложных данных, загрузки файлов или конфиденциальной информации.

Использование обоих app.get() и app.post() для одного и того же маршрута
Бывают случаи, особенно для дизайна RESTful, когда один URL-адрес должен обрабатывать как получение, так и отправку данных.

#### Извлечение и создание ресурсов:

Получить форму: используйте app.get(), чтобы вернуть форму для заполнения пользователями.
Отправка формы: используйте app.post() для обработки и сохранения отправленных данных формы.
Полная модификация объекта. Для полного обновления (или замены в REST) ​​использование app.post() гарантирует, что действие обновления запускается через POST-запрос, а не GET-запрос. Это различие важно для соблюдения принципов RESTful.
Пример кода: использование app.get() и app.post() для одного маршрута

Вот код JavaScript:

**Examples:**

```javascript
const userRecords = {}; // in-memory "database" for the sake of example

// Handle user registration form
app.get('/users/register', (req, res) => {
    res.send('Please register: <form method="POST"><input name="username"></form>');
});

// Process submitted registration form
app.post('/users/register', (req, res) => {
    userRecords[req.body.username] = req.body;
    res.send('Registration complete');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как you reTrieve the URL parameters from a GET request in Express.js?</summary>

### Как you reTrieve the URL parameters from a GET request in Express.js?

**Definition:**

В Express.js вы можете извлечь параметры URL-адреса из запроса GET, используя объект req.params. Вот краткий обзор шагов и пример кода:

**How it works:**

#### Пример кода: получение параметров URL-адреса

В этом примере идентификатор параметра URL извлекается и используется для получения соответствующих пользовательских данных.

#### Дополнительные шаги для сложных запросов GET

Для простых и понятных запросов GET прямая передача параметров URL работает хорошо. Однако для более сложных сценариев, таких как анализ параметров URL-адреса с помощью строк запроса или обработка дополнительных параметров, Express.js предлагает более продвинутые методы, которые описаны ниже:

#### Анализ параметров запроса

Что это такое: дополнительные данные, передаваемые в URL-адресе после ? характер. Пример: http://example.com/resource?type=user&page=1.

Как получить к нему доступ: используйте req.query, объект, который предоставляет пары ключ-значение анализируемых параметров запроса.

#### Пример кода: анализ параметров запроса

#### Необязательные и универсальные сегменты

Необязательные сегменты: сегменты URL-адресов, заключенные в круглые скобки, не являются обязательными, и к ним можно получить доступ с помощью req.params. Пример: /book(/:title)

Catch-All Segments: фиксирует оставшуюся часть URL-адреса и полезен в таких случаях, как перезапись URL-адреса. Обозначается звездочкой (*) или двойной звездочкой (**). Доступ также осуществляется с помощью req.params. Пример: /документы/*

**Examples:**

```javascript
// Sample URL: http://example.com/users/123
// Relevant Route: /users/:id

// Define the endpoint/route
app.get('/users/:id', (req, res) => {
    // Retrieve the URL parameter
    const userId = req.params.id;
    // ... (rest of the code)
});
```

```javascript
app.get('/search', (req, res) => {
    const { q, category } = req.query;
    // ... (rest of the code)
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Что такое route handlers, and how would you implement them?</summary>

### Что такое route handlers, and how would you implement them?

**Definition:**

Обработчики маршрутов в Express.js — это функции middleware, предназначенные для управления определенными путями в вашем приложении.

В зависимости от метода HTTP и конечной точки они могут выполнять различные задачи, такие как извлечение данных из базы данных, рендеринг представления или управление ответами HTTP.

Пример кода: настройка простого обработчика маршрута

Вот код:

Что такое цепочки обработчиков маршрутов?
Вы можете связать множество функций middleware для управления маршрутами с одним маршрутом. Каждая функция middleware в цепочке должна либо перейти к следующей функции с помощью next(), либо завершить цикл запрос-ответ.

Это позволяет выполнять такие проверки, как аутентификация пользователя, перед доступом к маршруту.

**How it works:**

#### HTTP-метод Удобные методы

Express.js предлагает специализированные, легко читаемые методы для наиболее распространенных HTTP-запросов:

приложение.get()
приложение.post()
приложение.пут()
приложение.delete()
приложение.use()
Эти методы упрощают настройку обработки маршрутов.

**Examples:**

```javascript
// Responds with "Hello, World!" for GET requests to the root URL (/)
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
In this example, the route handler is (req, res) => { res.send('Hello, World!'); }. It listens for GET requests on the root URL and responds with "Hello, World!".
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как you enable CORS in an Express.js application?</summary>

### Как you enable CORS in an Express.js application?

**Definition:**

Совместное использование ресурсов между источниками (CORS) — это механизм, который позволяет веб-страницам отправлять запросы в другой домен. В Express.js вы можете включить CORS с помощью пакета cors или вручную установив заголовки.

**How it works:**

#### Использование пакета cors

#### Установите корс:

Используйте npm или Yarn для установки пакета cors.

Используйте middleware app.use(cors()). Вы также можете настроить поведение CORS с помощью параметров.

Ручная настройка CORS
Используйте следующий пример кода, чтобы вручную настроить заголовки CORS в приложении Express:

Обязательно поместите это middleware перед определениями маршрутов.

**Examples:**

```bash
npm install cors
Integrate with Your Express App:
```

```javascript
const express = require('express');
const cors = require('cors');
const app = express();

// Enable CORS for all routes
app.use(cors());

// Example: Enable CORS only for a specific route
app.get('/public-data', cors(), (req, res) => {
    // ...
});

// Example: Customize CORS options
const customCorsOptions = {
    origin: 'https://example.com',
    optionsSuccessStatus: 200 // Some legacy browsers choke on 204
};

app.use(cors(customCorsOptions));
```

```javascript
app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
    if (req.method === 'OPTIONS') {
        res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, PATCH, DELETE, OPTIONS');
        return res.status(200).json({});
    }
    next();
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Объясните the use of next() in Express.js middleware.</summary>

### Объясните the use of next() in Express.js middleware.

**Definition:**

В Express.js функции middleware имеют решающее значение для обработки HTTP-запросов. Один запрос может пройти через несколько middleware, прежде чем достигнет конечной точки, предоставляя возможности для таких задач, как ведение журнала, анализ данных и обработка ошибок. Функция next() играет важную роль в этом процессе, обеспечивая как обычное связывание middleware, так и специальную обработку ошибок.

Что будет дальше()?
next(): функция обратного вызова, которая при вызове в middleware передает управление следующему middleware в стеке.
next() обычно вызывается, чтобы сигнализировать о том, что middleware выполнило свои задачи и что запрос должен перейти к следующему middleware.
Если middleware не вызывает next(), поток запросов может застрять, и последующие middleware не будут выполнены.

**How it works:**

#### Пример кода: middleware для ведения журналов

Вот код:

В этом примере оба middlewareного обеспечения вызывают next(), чтобы позволить запросу перейти к следующему middleware ведения журналов и, в конечном итоге, к конечной точке (не показано, но будет следующей в цепочке).

Без вызовов next() запрос зависнет после первого middleware.

**When to use:**

#### Варианты использования

Обычный поток: вызовите next(), чтобы переместить объекты запроса и ответа через стек middleware.

**Examples:**

```javascript
const app = require('express')();

// Sample middleware: logs the request method and URL
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Move to the next middleware
});

// Sample middleware: logs the current UTC time
app.use((req, res, next) => {
    console.log(new Date().toUTCString());
    next(); // Move to the next middleware
});

app.listen(3000);
```

```javascript
Error Handling: If a middleware detects an error, it can short-circuit the regular flow and jump directly to an error-handling middleware (defined with app.use(function(err, req, res, next) {})). This is achieved by calling next(err), where err is the detected error.
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>


### Routing and Requests

<details>
<summary>Что такое the role of the Express.Router class?</summary>

### Что такое the role of the Express.Router class?

**Definition:**

express.Router — мощный инструмент для управления несколькими контроллерами маршрутов. Это помогает организовать маршруты и функции их обработки в модульные, автономные группы.

**How it works:**

#### Ключевые особенности

Модульность: полагайтесь на отдельные модули маршрутов для улучшения организации кода, удобства обслуживания и совместной работы.

Middleware. Как и основное экспресс-приложение, маршрутизатор также может использовать middleware для обработки входящих запросов.

Цепочка методов HTTP: упрощает обработку маршрутов, позволяя определять маршруты для конкретных методов с использованием имен методов.

Пример: middleware и обработка маршрутов

Вот код Node.js:

Используйте встроенный экспресс.Роутер.
Прикрепите middleware общего назначения и два разных маршрута для конкретных методов HTTP.
Затем маршрутизатор интегрируется в основное экспресс-приложение с помощью:

Здесь app.use('/routerExample', router); назначает все маршруты, определенные в маршрутизаторе, /routerExample.

**Examples:**

```javascript
const express = require('express');
const router = express.Router();

// Logger Middleware
router.use((req, res, next) => {
  console.log('Router-specific Request Time:', Date.now());
  next();
});

// "GET" method route
router.get('/', (req, res) => {
  res.send('Router Home Page');
});

// "POST" method route
router.post('/', (req, res) => {
  res.send('Router Home Page - POST Request');
});

module.exports = router;
In this example, we:
```

```javascript
const app = express();
const router = require('./myRouterModule');

app.use('/routerExample', router);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как you handle 404 errors in Express.js?</summary>

### Как you handle 404 errors in Express.js?

**Definition:**

Обработка ошибок 404 в Express необходима для захвата и ответа на запросы несуществующих ресурсов. Для этой цели обычно вы используете как middleware, так и механизмы ответа HTTP.

**How it works:**

#### Middleware для ошибок 404

Используйте app.use в конце цепочки middleware для захвата неразрешенных маршрутов.
Вызовите middleware с помощью next() и объекта Error для пересылки в middleware для обработки ошибок.

Вот пример кода Node.js:

#### Middleware для обработки ошибок 404 и других ошибок

Определите middleware для обработки ошибок с четырьмя аргументами. Первым из них является объект ошибки.
Проверьте статус ошибки и отреагируйте соответствующим образом. Если это ошибка 404, обрабатывайте ее как необнаруженную ошибку; в противном случае воспринимайте это как ошибку сервера.

Вот код Node.js:

Полный пример:

Вот полное приложение Node.js:

**Examples:**

```javascript
app.use((req, res, next) => {
    const err = new Error(`Not Found: ${req.originalUrl}`);
    err.status = 404;
    next(err);
});
```

```javascript
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || "Internal Server Error";

    res.status(status).send(message);
});
```

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Sample router for demonstration
const usersRouter = express.Router();
usersRouter.get('/profile', (req, res) => {
    res.send('User Profile');
});
app.use('/users', usersRouter);

// Capture 404s
app.use((req, res, next) => {
    const err = new Error(`Not Found: ${req.originalUrl}`);
    err.status = 404;
    next(err);
});

// Error-handling middleware
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || "Internal Server Error";
    res.status(status).send(message);
});

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`);
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Что такое the differences between req.query and req.params?</summary>

### Что такое the differences between req.query and req.params?

**Definition:**

В Express.js req.query используется для доступа к параметрам запроса GET, а req.params используется для захвата параметров, определенных в пути URL.

**How it works:**

#### Понимание маршрутизации Express.js

Express.js использует app.get() и аналогичные функции для обработки различных типов HTTP-запросов.

app.get('/users/:id'): Сопоставляет запросы GET с /users/123, где 123 — это параметр :id в пути.

#### Доступ к данным запроса

req.query: используется для извлечения параметров строки запроса из URL-адреса запроса. Пример. Для URL-адреса /route?id=123 используйте req.query.id, чтобы получить 123.
req.params: используется для получения параметров из URL-адреса запроса. Для маршрута /users/:id используйте req.params.id для получения идентификатора, например /users/123.

#### Пример кода: запрос данных

Вот настройка сервера Express.js:

**Examples:**

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Endpoint to capture query string parameter
app.get('/query', (req, res) => {
  console.log(req.query);
  res.send('Received your query param!');
});

// Endpoint to capture URL parameter
app.get('/user/:id', (req, res) => {
  console.log(req.params);
  res.send('Received your URL param!');
});

app.listen(port, () => console.log(`Listening on port ${port}!`));
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Опишите the puRPOse of req.body and how you would access it.</summary>

### Опишите the puRPOse of req.body and how you would access it.

**Definition:**

В приложении Express.js req.body — это свойство объекта HTTP-запроса, которое содержит данные, отправленные через HTTP-запрос POST.

Запрос POST может исходить из формы HTML, кода JavaScript на стороне клиента или другого клиента API. Данные в req.body обычно структурируются как объект JSON или форма, закодированная в URL-адресе.

**How it works:**

#### Middleware и разбор тела запроса

Middleware express.json() и express.urlencoded() анализирует входящие объекты Request перед их передачей. Эти middleware заполняют req.body проанализированными данными в формате JSON и URL-адресом соответственно.

Вот пример того, как вы можете настроить анализ тела в приложении Express:

#### Доступ к данным req.body

После установки middleware для анализа тела вы можете получить доступ к проанализированным данным в функциях обработки маршрута:

Запросы POST или PUT. Когда клиент отправляет запрос POST или PUT с полезными данными JSON в теле запроса, вы можете получить доступ к этим данным через req.body.

Вот пример:

Клиентский JavaScript:

Обработчик экспресс-маршрута на стороне сервера:

HTML-формы: когда форма отправляется с использованием <form> с действием, указывающим на ваш экспресс-маршрут и метод как POST или PUT, а поля формы являются входными элементами внутри формы, req.body будет содержать эти значения полей формы.

Вот пример:

HTML-форма:

Пользовательские анализаторы. Хотя Express предоставляет встроенные анализаторы тела для данных в формате JSON и URL-адресов, вы можете получать данные в другом формате. В таких случаях вы можете создать собственное middleware для анализа и формирования данных по мере необходимости. Это middleware должно заполнить req.body.

**Examples:**

```javascript
const express = require('express');
const app = express();

// Parse JSON and URL-encoded data into req.body
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

```javascript
fetch('/example-route', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ key: 'value' })
});
```

```javascript
app.post('/example-route', (req, res) => {
  console.log(req.body); // Outputs: { key: 'value' }
});
```

```html
<form action="/form-endpoint" method="POST">
  <input type="text" name="username" />
  <input type="password" name="password" />
  <button type="submit">Submit</button>
</form>
Express route:

app.post('/form-endpoint', (req, res) => {
  console.log(req.body.username, req.body.password);
});
A modern technique for sending form data using fetch is by setting the Content-Type header to 'application/x-www-form-urlencoded' and using the URLSearchParams object:

fetch('/form-endpoint', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  },
  body: new URLSearchParams({ username: 'user', password: 'pass' })
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Как you create a middleware that logs the request method and URL for every request?</summary>

### Как you create a middleware that logs the request method and URL for every request?

**Definition:**

В Express.js middleware позволяет обрабатывать HTTP-запросы. Здесь вы узнаете, как создать простое middleware для ведения журналов, которое записывает метод запроса и URL-адрес.

**How it works:**

#### Настройка приложения Express

Сначала установите Express через npm и настройте файл app.js:

#### Создание middleware для ведения журналов

Определите функцию ведения журнала, которая извлекает метод запроса и URL-адрес, а затем используйте app.use(), чтобы смонтировать ее в качестве middleware.

#### Тестирование установки

Используйте app.get() для обработки запросов GET и app.listen() для запуска сервера.

Когда вы посетите http://localhost:3000/ в своем браузере и проверите консоль сервера, вы должны увидеть регистрируемый запрос.

**Examples:**

```javascript
const express = require('express');
const app = express();
```

```javascript
// Logging Middleware
const logRequest = (req, res, next) => {
  console.log(`Received ${req.method}  request for: ${req.url}`);
  next(); // Call next to proceed to the next middleware
};

// Mount the middleware for all routes
app.use(logRequest);
```

```javascript
// Sample route
app.get('/', (req, res) => {
  res.send('Hello World');
});

// Start the server
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Объясните how you would implement nested routes in Express.js.</summary>

### Объясните how you would implement nested routes in Express.js.

**Definition:**

Вложенные маршруты — это `express.Router`, смонтированный на другой роутер или на приложение с префиксом пути. Дочерний роутер видит только остаток пути после префикса.

**How it works:**

- `express.Router()` возвращает роутер с теми же `use`, `get`, `post`, что и приложение.
- `app.use('/users', usersRouter)` срезает `/users` до сопоставления. Дочерний `'/:id'` обрабатывает `/users/123`.
- Монтировать можно снова: `usersRouter.use('/:userId/orders', ordersRouter)`.
- Middleware роутера работает только для запросов, которые в него вошли. В гиде using middleware монтируют через `app.use('/', router)`.
- `req.originalUrl` остаётся полным путём. `req.params` собирает параметры со всех слоёв.
- `mergeParams: true` в конструкторе `express.Router` копирует параметры родителя. Это описано в гиде Routing: без опции параметры родителя в дочерних маршрутах недоступны.

**Examples:**

```js
const express = require('express')
const app = express()
const users = express.Router()
const orders = express.Router({ mergeParams: true })

orders.get('/', (req, res) => {
  res.json({ userId: req.params.userId })
})

users.use('/:userId/orders', orders)
app.use('/users', users)
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
</details>
<details>
<summary>Как you CAPture and respond to URL parameters in a route?</summary>

### Как you CAPture and respond to URL parameters in a route?

**Definition:**

Именованный сегмент `:name` попадает в `req.params`. Обработчик читает объект и отвечает через `res.json` или `res.send`.

**How it works:**

- `app.get('/user/:id', ...)` ставит `req.params.id` для `/user/42`.
- Пример using middleware сравнивает `req.params.id === '0'` и вызывает `next('route')`, чтобы перейти к следующему подходящему маршруту.
- Строка запроса — это `req.query`, не `req.params`.
- JSON отдают `res.json`. Текст или HTML — `res.send`. Статус ставят `res.status(code)` до тела.

**Examples:**

```js
app.get('/user/:id', (req, res) => {
  res.status(200).json({ id: req.params.id })
})
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)
</details>
<details>
<summary>Как you serve different content types (e.g., JSON, HTML) with Express.js responses?</summary>

### Как you serve different content types (e.g., JSON, HTML) with Express.js responses?

**Definition:**

`res.json` отправляет JSON и ставит `Content-Type`. `res.send` отправляет строку, объект или `Buffer`. `res.render` прогоняет шаблон и отдаёт HTML. `res.type` задаёт `Content-Type` до тела.

**How it works:**

- `res.json(obj)` сериализует объект. Гид по ошибкам для XHR использует `res.status(500).send({ error: 'Something failed!' })`, а для страницы — `res.render`.
- `res.send('<p>Hello</p>')` шлёт строку. Если строка должна быть HTML, а не plain text, перед этим вызывают `res.type('html')`.
- `res.format` выбирает обработчик по заголовку `Accept`, когда один маршрут отдаёт и JSON, и HTML.
- Исключение в `async`-обработчике доходит до Express. Статус берётся из `err.status` или `err.statusCode`, иначе 500. В production тело — HTML сообщения статуса.

**When to use:**

- `res.json` — клиент API.
- `res.render` — настроен шаблонизатор.
- `res.type` и `res.send` — тело уже собрано.

**Examples:**

```js
app.get('/item', (req, res) => {
  res.format({
    html() {
      res.type('html').send('<p>Item</p>')
    },
    json() {
      res.json({ name: 'Item' })
    },
  })
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — res.json](https://expressjs.com/en/api.html#res.json)
</details>
<details>
<summary>Что такое best practices for structuring a large Express.js application with multiple routes?</summary>

### Что такое best practices for structuring a large Express.js application with multiple routes?

**Definition:**

Маршруты делят на отдельный `express.Router` по области и монтируют из входного файла. Middleware ошибок с четырьмя аргументами ставят последним.

**How it works:**

- Пример миграции на Express 4 грузит модули маршрутов (`./routes`, `./routes/user`). Тот же разрез делается через `app.use('/users', userRouter)`.
- Порядок — это поведение: логгер и разбор тела, затем маршруты, затем 404, затем обработчики ошибок.
- Данные запроса не кладут в глобалы. Гид по производительности говорит, что процессы кластера не делят память. Состояние между запросами — во внешнем хранилище.
- `NODE_ENV=production` включает кэш шаблонов и короткие ошибки.

**Examples:**

```js
const users = require('./routes/users')
const orders = require('./routes/orders')

app.use('/users', users)
app.use('/orders', orders)

app.use((req, res) => {
  res.status(404).send("Sorry can't find that!")
})
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
</details>

### Middleware and Error Handling

<details>
<summary>Объясните the concept and use of built-in middleware in Express.js.</summary>

### Объясните the concept and use of built-in middleware in Express.js.

**Definition:**

Встроенный middleware поставляется с Express. Для разбора тела и файлов API описывает `express.json()`, `express.urlencoded()` и `express.static()`.

**How it works:**

- `express.json()` разбирает `Content-Type: application/json` и заполняет `req.body`. Лимит по умолчанию — `"100kb"`. `strict` по умолчанию `true` (только массивы и объекты).
- `express.urlencoded({ extended: true })` разбирает формы.
- `express.static(root, options)` отдаёт файл, склеивая `req.url` с `root`. Нет файла — вызывается `next()`. По умолчанию `etag: true`, `index: "index.html"`, `maxAge: 0`, `dotfiles: "ignore"`.
- Их монтируют через `app.use` до маршрутов, которым нужны `req.body` или статика.

**Examples:**

```js
app.use(express.json())
app.use(express.urlencoded({ extended: true }))
app.use(express.static('public'))
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)
- [Express — express.static](https://expressjs.com/en/api.html#express.static)
- [Express — req.body](https://expressjs.com/en/api.html#req.body)
</details>
<details>
<summary>Как you write custom middleware functions in Express.js?</summary>

### Как you write custom middleware functions in Express.js?

**Definition:**

Свой middleware — функция `(req, res, next)`. Она читает или меняет запрос, завершает ответ или вызывает `next()`.

**How it works:**

- Подключают через `app.use(fn)` или на роутере. Без пути функция идёт на каждый запрос, дошедший до этого роутера.
- `next()` вызывают один раз, когда слой закончил работу. Если нет ни `next()`, ни ответа, запрос зависает.
- `next(err)` пропускает обычный middleware. `next('route')` пропускает остаток этого маршрута и ищет следующий подходящий.
- `async`-middleware, который бросает ошибку, попадает в `next`, потому что возвращает промис. Callback API вроде `fs.readFile` должен сам вызвать `next(err)`.
- Обработчик ошибок `(err, req, res, next)` регистрируют после маршрутов.

**Examples:**

```js
app.use((req, res, next) => {
  console.log(req.method, req.originalUrl)
  next()
})
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
</details>
<details>
<summary>Как you handle file uploads in Express.js?</summary>

### Как you handle file uploads in Express.js?

**Definition:**

Express сам не разбирает multipart. Каталог middleware указывает `multer` для `multipart/form-data`. `express.json()` разбирает только JSON.

**How it works:**

- `multer` ставят на маршрут с файлом до того, как обработчик читает файл.
- Пример миграции на Express 4 вызывает `app.use(multer())` среди остальных разборщиков тела, до маршрутов.
- Файл не оказывается в `req.body`, как JSON. Middleware multer кладёт метаданные туда, где это описано у пакета. Нужен лимит размера, чтобы один запрос не забил диск.
- JSON API остаётся на `express.json()` с опцией `limit` (по умолчанию `"100kb"`).

**When to use:**

- `multer` — форма браузера с полем файла.
- `express.json()` — JSON без файла.

**Examples:**

```js
const multer = require('multer')
const upload = multer({ dest: 'uploads/' })

app.post('/profile', upload.single('avatar'), (req, res) => {
  res.json({ file: req.file })
})
```

**Sources:**

- [Express — Middleware](https://expressjs.com/en/resources/middleware.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)
</details>
<details>
<summary>Что такое Express-session, and how would you use it?</summary>

### Что такое Express-session, and how would you use it?

**Definition:**

`express-session` хранит данные сессии на сервере и кладёт в cookie только id сессии. Он заменяет middleware `express.session` из Express 3.

**How it works:**

- Хранилище по умолчанию — память, для production оно не годится. Гид по безопасности требует совместимое хранилище сессий.
- Задают `secret` и `name`, отличное от имени cookie по умолчанию, чтобы cookie не выдавал Express.
- Если TLS обрывается на прокси, ставят `app.set('trust proxy', 1)` и `cookie.secure: true`, чтобы cookie была помечена Secure.
- `cookie-session` хранит всю сессию в cookie. Его берут только для небольших примитивов. Клиент cookie видит. Лимит в гиде — 4093 байта на домен.
- В примере миграции на Express 4 есть `resave: true` и `saveUninitialized: true`. Эти флаги задают осознанно.

**When to use:**

- `express-session` — сессия должна остаться на сервере.
- `cookie-session` — полезная нагрузка маленькая и не секретная.

**Examples:**

```js
const session = require('express-session')

app.set('trust proxy', 1)
app.use(session({
  secret: process.env.SESSION_SECRET,
  name: 'sessionId',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true, httpOnly: true },
}))
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)
</details>
<details>
<summary>Обсудите error handling in an Express.js application. How do you define an error-handling middleware?</summary>

### Обсудите error handling in an Express.js application. How do you define an error-handling middleware?

**Definition:**

Middleware ошибок имеет четыре параметра: `(err, req, res, next)`. Express вызывает его, когда маршрут бросает исключение, `async`-обработчик отклоняется или вы вызываете `next(err)`.

**How it works:**

- Синхронный `throw` ловится сам. Отклонённый `async`-маршрут передаёт ошибку в `next`. Промис, который не вернули, нужно завершить `.catch(next)`. Callback вроде `fs.readFile` сам вызывает `next(err)`.
- `next('route')` — не ошибка. Любое другое значение в `next` пропускает обычный middleware.
- Обработчик ошибок определяют после маршрутов. Если `res.headersSent` уже true, вызывают `next(err)`, и обработчик по умолчанию закрывает соединение.
- Обработчик по умолчанию берёт `err.status` или `err.statusCode`, если это 4xx или 5xx, иначе 500. В production тело — сообщение статуса, не `err.stack`. Для этого нужен `NODE_ENV=production`.
- Если обработчик не вызывает `next` и не шлёт ответ, запрос зависает.

**Examples:**

```js
app.use((err, req, res, next) => {
  if (res.headersSent) return next(err)
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
</details>
<details>
<summary>Приведите an example of using third-party middleware, such as body-parser or morGAn.</summary>

### Приведите an example of using third-party middleware, such as body-parser or morGAn.

**Definition:**

Сторонний middleware — npm-пакет, который подключают через `app.use` или `router.use`. `morgan` пишет лог запросов. `body-parser` разбирает тело. В текущем Express те же JSON и формы закрывают `express.json()` и `express.urlencoded()`.

**How it works:**

- `app.use(morgan('combined'))` пишет логи Apache combined в stdout. Логгер ставят до маршрутов, чтобы попал каждый запрос.
- Пример API роутера делает `router.use(logger())`, затем `express.static`.
- Пример миграции на Express 4 ещё показывает `bodyParser.json()` и `bodyParser.urlencoded({ extended: true })`. Для обычных JSON и форм предпочтительны встроенные `express.json()` и `express.urlencoded({ extended: true })`.
- Гид по производительности говорит, что `console.log` в терминал синхронный. Для продакшена он указывает на библиотеку вроде Pino, а для лога запросов — на `morgan`.

**Examples:**

```js
const morgan = require('morgan')
const app = express()

app.use(morgan('combined'))
app.use(express.json())

app.get('/', (req, res) => {
  res.send('hello, world!')
})
```

**Sources:**

- [Express — morgan](https://expressjs.com/en/resources/middleware/morgan.html)
- [Express — req.body](https://expressjs.com/en/api.html#req.body)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Как you protect aGAinst SQL injection or other security threats in Express.js?</summary>

### Как you protect aGAinst SQL injection or other security threats in Express.js?

**Definition:**

Express SQL не чистит. Инъекцию останавливают параметрами запроса к драйверу базы. Остальные атаки снижают Helmet, TLS и строгие редиректы.

**How it works:**

- Гид по безопасности требует параметризованные запросы или prepared statements. `req.query` и `req.body` в строку SQL не склеивают.
- `helmet()` ставит заголовки, включая `Content-Security-Policy`, `Strict-Transport-Security` и `X-Content-Type-Options`. Он убирает `X-Powered-By`. То же делает `app.disable('x-powered-by')`.
- Любой URL проверяют до `res.redirect`. Гид отвергает хост, который вы не разрешили, и отвечает 400, если URL не разбирается.
- TLS гид советует завершать на Nginx. Пример сертификата — Let’s Encrypt.
- `npm audit` проверяет дерево зависимостей. Express 2 и 3 не сопровождаются.
- Маршруты входа ограничивают по неудачам. Гид указывает на `rate-limiter-flexible`.

**Examples:**

```js
app.get('/user', async (req, res) => {
  const { rows } = await pool.query('SELECT id, name FROM users WHERE id = $1', [req.query.id])
  res.json(rows[0] ?? null)
})
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>

### Response and Performance

<details>
<summary>Как бы вы implement caching in an Express.js application?</summary>

### Как бы вы implement caching in an Express.js application?

**Definition:**

Кэш не повторяет одну и ту же работу для одного и того же запроса. Express может поставить `Cache-Control`. Гид по производительности также советует кэш перед приложением, например Nginx или Varnish.

**How it works:**

- `express.static` ставит `Cache-Control`, когда `cacheControl` равен true (это умолчание). `maxAge` — max-age в миллисекундах (по умолчанию `0`). `immutable: true` добавляет директиву immutable.
- `NODE_ENV=production` заставляет Express кэшировать шаблоны и CSS из расширений CSS.
- Для вычисленного HTML или JSON `Cache-Control` ставят только если ответ можно переиспользовать. Персональный ответ общий кэш хранить не должен.
- Для нагруженного сайта гид советует сжимать и кэшировать на обратном прокси. `compression()` — gzip внутри приложения. Он не нужен, если gzip уже делает Nginx.

**When to use:**

- `maxAge` на статике с отпечатком в имени.
- Кэш прокси для публичных GET.
- Общий кэш не используют для ответа, который зависит от вошедшего пользователя.

**Examples:**

```js
app.use(express.static('public', { maxAge: '1d' }))

app.get('/catalog', (req, res) => {
  res.set('Cache-Control', 'public, max-age=60')
  res.json({ items: [] })
})
```

**Sources:**

- [Express — express.static](https://expressjs.com/en/api.html#express.static)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Как you set cookies and get cookies in an Express.js application?</summary>

### Как you set cookies and get cookies in an Express.js application?

**Definition:**

`res.cookie(name, value, options)` ставит заголовок `Set-Cookie`. `cookie-parser` читает заголовок `Cookie` в `req.cookies`. Подписанные cookie лежат в `req.signedCookies`.

**How it works:**

- Без `cookie-parser` сырой заголовок остаётся в `req.headers.cookie`. Middleware его разбирает.
- Опции cookie в гиде по безопасности: `secure` (только HTTPS), `httpOnly` (не видно скрипту страницы), `domain`, `path` и `expires`.
- Cookie сессии не должна носить имя по умолчанию. `name` задают у `express-session` или `cookie-session`.
- За прокси `app.set('trust proxy', 1)`, чтобы `secure` cookie работала, когда TLS заканчивается на прокси.

**Examples:**

```js
const cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', (req, res) => {
  res.cookie('sessionId', 'abc', { httpOnly: true, secure: true })
  res.json({ cookies: req.cookies })
})
```

**Sources:**

- [Express — cookie-parser](https://expressjs.com/en/resources/middleware/cookie-parser.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — res.cookie](https://expressjs.com/en/api.html#res.cookie)
</details>
<details>
<summary>Что такое ways to improve the performance of Express.js applications?</summary>

### Что такое ways to improve the performance of Express.js applications?

**Definition:**

Гид по производительности делит это на код и эксплуатацию. В коде остаются асинхронными и не блокируют логами. В production ставят `NODE_ENV`, берут актуальный Node.js LTS и запускают больше одного процесса за прокси.

**How it works:**

- На пути запроса не вызывают синхронные filesystem и crypto. `--trace-sync-io` печатает стек, когда сработал синхронный API. В production этот флаг не оставляют.
- `console.log` в терминал синхронный. Для запросов берут `morgan` или логгер вроде Pino, для отладки — пакет `debug`.
- `app.use(compression())` сжимает ответы. На загруженном сайте gzip делают на Nginx.
- `NODE_ENV=production` кэширует шаблоны и укорачивает ошибки. Гид сообщает большой прирост от одной этой настройки.
- Кластер — по процессу на ядро через модуль `cluster` или PM2 (`pm2 start npm --name my-app -i max -- start`). Воркеры не делят память, поэтому сессии лежат в хранилище вроде Redis.
- Nginx или HAProxy впереди забирают кэш, статику и балансировку.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Как you configure an Express.js app for a reverse proxy, like Nginx?</summary>

### Как you configure an Express.js app for a reverse proxy, like Nginx?

**Definition:**

Обратный прокси принимает публичный трафик и передаёт его в Express. Гид по производительности в production советует Nginx или HAProxy для TLS, gzip, кэша и балансировки.

**How it works:**

- Без доверия к прокси Express видит адресом клиента адрес прокси. `app.set('trust proxy', 1)` доверяет первому прокси. Примеры сессии так делают, чтобы соблюдался `cookie.secure`.
- Прокси должен ставить `X-Forwarded-For`, `X-Forwarded-Proto` и `Host`. После `trust proxy` `req.ip` и `req.protocol` берутся из этих заголовков.
- TLS завершают на прокси. Гид по безопасности указывает на Nginx и Let’s Encrypt.
- `compression()` не включают, если прокси уже жмёт gzip.
- Липкие сессии нужны, только если данные сессии живут в одном процессе. Общее хранилище это снимает.

**Examples:**

```js
app.set('trust proxy', 1)
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)
</details>
<details>
<summary>Объясните the puRPOse of template engines. How would you integrate one with Express.js?</summary>

### Объясните the puRPOse of template engines. How would you integrate one with Express.js?

**Definition:**

Шаблонизатор превращает шаблон и данные в HTML на сервере. Express вызывает его через `res.render(view, locals)`.

**How it works:**

- `app.set('views', directory)` — папка шаблонов. `app.set('view engine', 'pug')` выбирает движок, как в примере миграции на Express 4.
- `res.render('regular')` ищет этот вид. Гид using middleware рендерит `'regular'` и `'special'`.
- В production (`NODE_ENV=production`) Express кэширует скомпилированные шаблоны.
- Обработчик ошибок может сделать `res.status(500)` и `res.render('error', { error: err })`. `err.stack` пользователям в production не отдают. Обработчик по умолчанию уже прячет стек, когда `NODE_ENV` равен production.

**When to use:**

- HTML, собранный на сервере.
- JSON API обходится без движка и отвечает `res.json`.

**Examples:**

```js
app.set('views', path.join(__dirname, 'views'))
app.set('view engine', 'pug')

app.get('/', (req, res) => {
  res.render('index', { title: 'Home' })
})
```

**Sources:**

- [Express — Using template engines](https://expressjs.com/en/guide/using-template-engines.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>

### Testing and Debugging

<details>
<summary>Как you test an Express.js application?</summary>

### Как you test an Express.js application?

**Definition:**

Приложение Express — функция `(req, res, next)`. Тест может вызвать её через HTTP-сервер Node без браузера и проверить статус и тело.

**How it works:**

- Экспортируйте `app` и не вызывайте `listen` в импортируемом модуле. Тест слушает порт `0`, порт выбирает ОС.
- `NODE_ENV` меняет тела ошибок и кэш шаблонов. Ставьте его в процессе теста, если проверяете production.
- Покройте обработчик ошибок: брошенная ошибка даёт 500 и не должна утечь `err.stack`, когда `NODE_ENV` равен `production`.
- `npm audit` из гида по безопасности — проверка зависимостей, не замена тестов маршрутов.

**Examples:**

```js
const http = require('node:http')
const assert = require('node:assert/strict')
const app = require('./app')

const server = http.createServer(app)
server.listen(0, async () => {
  const { port } = server.address()
  const res = await fetch(`http://127.0.0.1:${port}/health`)
  assert.equal(res.status, 200)
  server.close()
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>
<details>
<summary>Обсудите common debugging techniques for Express.js applications.</summary>

### Обсудите common debugging techniques for Express.js applications.

**Definition:**

Отладка — это лог запроса, стек ошибки и инспектор Node. Гид по производительности отделяет отладочные логи от логов трафика в production.

**How it works:**

- В разработке обработчик ошибок по умолчанию шлёт `err.stack`. `NODE_ENV=production` ставят, когда стек нужно скрыть.
- `morgan('dev')` или `morgan('combined')` пишет метод, URL и статус.
- Пакет `debug` печатает, когда задана переменная `DEBUG`. Гид советует его вместо `console.log` для отладки.
- `--trace-sync-io` предупреждает о синхронном API. Им ищут блокирующие вызовы, это не production-флаг.
- `node --inspect server.js` открывает инспектор V8. К нему подключают Chrome DevTools или отладчик редактора и ставят точку в маршруте.

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
</details>
<details>
<summary>Что такое the role of environment variables in an Express.js application?</summary>

### Что такое the role of environment variables in an Express.js application?

**Definition:**

Переменные окружения настраивают процесс без секретов и хостов в коде. Express читает `NODE_ENV` и `app.get('env')`. Ваш код читает `process.env`.

**How it works:**

- `NODE_ENV=production` кэширует шаблоны, кэширует CSS из расширений и укорачивает ошибки. `app.get('env')` возвращает это значение (`development`, если переменная не задана).
- Пример миграции берёт `process.env.PORT || 3000`.
- `secret` сессии берут из окружения, не из исходников. Литерал в примерах безопасности — только фрагмент.
- Чтение `process.env` на каждый запрос стоит времени. Гид советует читать его редко.
- В production переменные задаёт init-система (systemd `Environment=`), а не только профиль оболочки разработчика.

**Examples:**

```js
const port = process.env.PORT || 3000
if (app.get('env') === 'production') {
  app.set('trust proxy', 1)
}
```

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)
</details>
<details>
<summary>Как you use a debugger with an Express.js app running in Node.js?</summary>

### Как you use a debugger with an Express.js app running in Node.js?

**Definition:**

Процесс запускают с `node --inspect`. Инспектор ждёт Chrome DevTools или IDE. Точка останова в маршруте срабатывает, когда запрос входит в функцию.

**How it works:**

- `node --inspect server.js` или `node --inspect-brk server.js` (второй ждёт, пока подключится отладчик).
- Маршрут — обычный JavaScript. Точка на `req.params` показывает запрос, который Node уже разобрал.
- `--trace-sync-io` из гида по производительности — отдельный флаг. Он пишет синхронный ввод-вывод. Это не интерактивный отладчик.
- Инспектор не оставляют на публичном интерфейсе в production. Отладку привязывают к локальному процессу разработки.

**Examples:**

```bash
node --inspect server.js
```

**Sources:**

- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>

### Express.js with Databases

<details>
<summary>Как бы вы connect a MongoDB dataBASE with an Express.js application?</summary>

### Как бы вы connect a MongoDB dataBASE with an Express.js application?

**Definition:**

В Express нет клиента базы. Подключение делает драйвер MongoDB для Node.js (или ODM) при старте процесса, а маршруты пользуются этим клиентом.

**How it works:**

- URI читают из окружения. Подключаются один раз, не внутри каждого запроса.
- `express.json()` должен отработать до того, как обработчик читает `req.body`.
- Ошибку передают в `next(err)` или пишут `async`-обработчик, чтобы сбой запроса дошёл до middleware ошибок.
- Запросы не собирают склейкой ввода пользователя. Фильтр передают объектом.

**Examples:**

```js
const { MongoClient } = require('mongodb')
const client = new MongoClient(process.env.MONGODB_URI)

app.get('/notes', async (req, res) => {
  const notes = await client.db().collection('notes').find({}).toArray()
  res.json(notes)
})
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [MongoDB Node.js driver — Connect](https://www.mongodb.com/docs/drivers/node/current/connect/connection/)
</details>
<details>
<summary>Объясните how to integrate an ORM like Sequelize with Express.js.</summary>

### Объясните how to integrate an ORM like Sequelize with Express.js.

**Definition:**

Sequelize — отдельный пакет. Express нужен только доступ к моделям в маршруте. Один экземпляр Sequelize открывают на процесс и закрывают при остановке.

**How it works:**

- Базу настраивают из переменных окружения. `sequelize.authenticate()` на старте роняет процесс с плохим URI до приёма трафика.
- Маршруты вызывают методы модели (`Note.findAll()`) и отвечают `res.json`. Отклонение `async`-обработчика уходит в middleware ошибок.
- Фильтры идут через API моделей или подстановки Sequelize. Сырой SQL из `req.query` не собирают.
- В кластере у каждого воркера свой экземпляр. Это совпадает с гидом: процессы не делят память.

**Examples:**

```js
app.get('/notes', async (req, res) => {
  const notes = await Note.findAll()
  res.json(notes)
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Sequelize — Getting started](https://sequelize.org/docs/v6/getting-started/)
</details>
<details>
<summary>Как you handle dataBASE errors in Express.js?</summary>

### Как you handle dataBASE errors in Express.js?

**Definition:**

Сбой запроса — такая же ошибка. `async`-маршрут, который отклонился, вызывает `next(err)`. Middleware ошибок выбирает HTTP-статус и тело.

**How it works:**

- Дубликаты ключей, таймауты и отказ соединения не должны стать необработанным исключением. Гид по производительности говорит, что необработанное исключение роняет процесс.
- Известные ошибки драйвера в обработчике отображают на 400 или 409. Неизвестные остаются 500.
- Если заголовки уже отправлены, делают `return next(err)`.
- В production обработчик по умолчанию не шлёт `err.stack`. Стек пишут в лог сервера, как `logErrors` в гиде.
- Клиенту не отдают текст SQL и строку подключения.

**Examples:**

```js
app.use((err, req, res, next) => {
  if (res.headersSent) return next(err)
  console.error(err.stack)
  res.status(500).json({ error: 'database request failed' })
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Что такое the advantages of using a dataBASE pooling mechanism in an Express.js app?</summary>

### Что такое the advantages of using a dataBASE pooling mechanism in an Express.js app?

**Definition:**

Пул держит набор открытых соединений и выдаёт одно на запрос. Следующий запрос берёт соединение снова, а не открывает новую TCP-сессию.

**How it works:**

- Подключение на каждый запрос тратит рукопожатие до любого SQL. Под нагрузкой это доминирует в задержке.
- `Pool` в `pg` и пул Sequelize живут внутри одного процесса. Пул создают при старте, рядом с `express()`.
- Модель кластера из гида остаётся: у каждого процесса свой пул. Сумма пулов не должна исчерпать лимит соединений базы.
- Клиента возвращают в пул, когда запрос закончен. Обработчик, который взял клиента и не вернул, остановит следующие запросы.

**When to use:**

- Любое приложение, где запросы идут не изредка.
- Короткий скрипт может открыть один клиент и закрыть его.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [node-postgres — Pooling](https://node-postgres.com/features/pooling)
</details>

### Authentication and Authorization

<details>
<summary>Опишите how you would implement user authentication in Express.js.</summary>

### Опишите how you would implement user authentication in Express.js.

**Definition:**

Аутентификация проверяет, кто вызывает. Обычная схема Express — `express-session` и маршрут, который сверяет пароль и кладёт id пользователя в `req.session`.

**How it works:**

- Пароли хранят только как хеш. На входе сравнивают хеш. Гид по безопасности не доверяет сырому вводу.
- Cookie сессии ставят `httpOnly` и `secure` и не используют имя по умолчанию.
- Неудачные входы ограничивают. Гид предлагает лимит подряд идущих неудач на пользователя и IP и указывает на `rate-limiter-flexible`.
- Следующий маршрут читает `req.session.userId`. Если его нет, ответ 401.
- Middleware сессии стоит до маршрутов, которым нужен `req.session`.

**Examples:**

```js
app.post('/login', async (req, res) => {
  const user = await findUser(req.body.email)
  if (!user || !verifyPassword(req.body.password, user.passwordHash)) {
    return res.status(401).json({ error: 'invalid credentials' })
  }
  req.session.userId = user.id
  res.json({ ok: true })
})
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)
</details>
<details>
<summary>Объясните how sessions are managed in Express.js.</summary>

### Объясните how sessions are managed in Express.js.

**Definition:**

`express-session` хранит сессию на сервере и кладёт id сессии в cookie. `cookie-session` хранит сессию в самой cookie.

**How it works:**

- MemoryStore — умолчание и не для production. Хранилище должно быть доступно каждому процессу. Гид по производительности называет Redis для данных сессии в кластере, потому что воркеры не делят память.
- `trust proxy` нужен, когда cookie `secure`, а TLS заканчивается на Nginx.
- `cookie-session` сериализует сессию в cookie. Клиент её читает. Гид держит лимит 4093 байта на домен. Секреты в такую cookie не кладут.
- Смена `name` уходит от имени по умолчанию, которое гид по безопасности считает отпечатком.

**When to use:**

- Серверное хранилище — для сессии входа.
- Сессия в cookie — только для маленьких несекретных значений.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Обсудите how you would handle user roles and permissions in an Express.js application.</summary>

### Обсудите how you would handle user roles and permissions in an Express.js application.

**Definition:**

Проверка роли — middleware после аутентификации. Он читает пользователя из сессии и вызывает `next()` или отвечает 403.

**How it works:**

- Пользователя загружают один раз. Поздний middleware не верит роли из тела или query.
- `next('route')` пропускает остаток стека текущего маршрута. Гид по ошибкам так делает, когда пользователь не оплатил доступ, чтобы сработал другой обработчик того же пути.
- Проверку вешают на роутер, который она защищает: `router.use(requireRole('admin'))`.
- Отказ закрытый. Нет пользователя — 401. Пользователь без роли — 403.

**Examples:**

```js
function requireUser(req, res, next) {
  if (!req.session.userId) return res.status(401).json({ error: 'login required' })
  next()
}

function requireRole(role) {
  return (req, res, next) => {
    if (req.session.role !== role) return res.status(403).json({ error: 'forbidden' })
    next()
  }
}
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
</details>
<details>
<summary>Как you secure passwords and sensitive information in your Express.js app?</summary>

### Как you secure passwords and sensitive information in your Express.js app?

**Definition:**

Пароли хранят только как хеши. Секреты держат в окружении. Их передают только по TLS и не кладут в cookie, которую читает скрипт страницы.

**How it works:**

- Гид по безопасности требует TLS для чувствительных данных и советует завершать его на Nginx.
- Cookie сессии: `httpOnly: true`, `secure: true` и нестандартный `name`.
- `cookie-session` кладёт данные туда, где их видит клиент. `express-session` берут, когда полезная нагрузка должна остаться на сервере.
- Тело маршрута входа не логируют. `morgan('combined')` пишет строку запроса, не JSON с паролем, пока пароль в теле.
- `npm audit` и поддерживаемый релиз Express входят в гид. Файлы `.env` в репозиторий не коммитят.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>

### Integration and Configuration

<details>
<summary>Как you integrate a third-party API in an Express.js application?</summary>

### Как you integrate a third-party API in an Express.js application?

**Definition:**

Чужой сервис вызывают из обработчика на сервере. Браузер говорит только с вашим Express, и секрет третьей стороны остаётся в окружении.

**How it works:**

- Берут глобальный `fetch` текущего Node или другой HTTP-клиент. Нужен таймаут, чтобы зависший upstream не держал запрос. У `fetch` в Node есть `AbortSignal`.
- Сбой upstream отображают на выбранный вами статус. Тело upstream не проксируют, если в нём могут быть секреты.
- Отклонение `async`-обработчика доходит до middleware ошибок.
- Публичный стабильный ответ можно кэшировать через `Cache-Control` или прокси. Ответ, который ходил с учётными данными вызывающего, не кэшируют.

**Examples:**

```js
app.get('/weather', async (req, res) => {
  const upstream = await fetch('https://api.example.com/weather', {
    headers: { Authorization: `Bearer ${process.env.WEATHER_TOKEN}` },
  })
  if (!upstream.ok) return res.status(502).json({ error: 'upstream failed' })
  res.json(await upstream.json())
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [MDN — fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
</details>
<details>
<summary>Объясните the steps to deploy an Express.js application to a cloud provider like AWS or Heroku.</summary>

### Объясните the steps to deploy an Express.js application to a cloud provider like AWS or Heroku.

**Definition:**

Деплой — это процесс Node, `NODE_ENV=production` и обратный прокси или маршрутизатор платформы перед ним. Чеклист Express — гид по производительности.

**How it works:**

- Слушают `process.env.PORT`. Пример миграции: `process.env.PORT || 3000`.
- `NODE_ENV=production` задают в окружении платформы, не только в профиле оболочки. Это кэширует шаблоны и прячет стеки.
- Менеджер процессов или политика перезапуска платформы поднимает процесс после падения. Гид показывает systemd и менеджер процессов.
- Берут текущий Node.js LTS. TLS завершают на прокси. `app.set('trust proxy', 1)` ставят, когда платформа проксирует.
- Сессиям нужно общее хранилище, если процессов больше одного. Перед выкладкой запускают `npm audit`.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>
<details>
<summary>Как you ensure that your Express.js application is Scalable?</summary>

### Как you ensure that your Express.js application is Scalable?

**Definition:**

Один процесс Node занимает одно ядро. Масштаб — это несколько процессов без состояния в памяти и балансировка запросов между ними.

**How it works:**

- Модуль `cluster` или PM2 (`-i max`) поднимает по воркеру на ядро. Воркеры не делят память.
- Сессию и прочее общее состояние держат в Redis или базе. Гид говорит, что состояние в процессе ломается на втором процессе.
- Балансировщик (Nginx или HAProxy) раздаёт трафик. Липкие сессии — обходной путь. Общее хранилище — предпочтение гида.
- Gzip, статику и кэш отдают прокси, чтобы Express занимался приложением.
- Синхронные вызовы на пути запроса не используют. Они останавливают все соединения этого процесса.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Что такое the Twelve-Factor App methodology, and how does it apply to Express.js?</summary>

### Что такое the Twelve-Factor App methodology, and how does it apply to Express.js?

**Definition:**

Методология двенадцати факторов — правила приложения на платформе: конфиг в окружении, процессы без состояния, логи как поток событий, чистая сборка. Express её не реализует. Её применяют тем, как запускают процесс.

**How it works:**

- Конфиг: `process.env` и `NODE_ENV`. Гид по производительности использует оба.
- Процессы: сессии в памяти не держат, если копий больше одной. Раздел про кластер говорит, что память локальна для процесса.
- Логи: лог запросов пишут в stdout (`morgan('combined')`), платформа его собирает.
- Привязка к порту: `app.listen(process.env.PORT)`.
- Админские задачи (миграции) — отдельная команда, не обработчик запроса.
- Близость dev и prod: гид по безопасности прячет стеки в production и требует TLS. Обработчик ошибок разработки в production не запускают.

**Sources:**

- [The Twelve-Factor App](https://12factor.net/)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — morgan](https://expressjs.com/en/resources/middleware/morgan.html)
</details>

### Coding Challenges

<details>
<summary>Напишите an Express.js middleware function that limits requests to 100 per hour per IP address.</summary>

### Напишите an Express.js middleware function that limits requests to 100 per hour per IP address.

**Definition:**

Встроенного ограничителя частоты в Express нет. Middleware может считать запросы по IP в памяти и отвечать 429 после 100 за час. Гид по безопасности для ограничения входа указывает на `rate-limiter-flexible`. Фрагмент ниже — та же идея на `Map`.

**How it works:**

- Ключ карты — `req.ip`. `trust proxy` нужен, когда впереди прокси, иначе у всех клиентов адрес прокси.
- Хранят начало окна и счётчик. Если окну больше часа, его сбрасывают.
- Ниже лимита вызывают `next()`. Иначе `res.status(429)` и `next()` не вызывают.
- `Map` одного процесса не ограничивает кластер. У каждого воркера своя карта. Общее хранилище нужно, если процессов больше одного. Гид говорит то же о сессиях.

**Examples:**

```js
const hits = new Map()
const HOUR = 60 * 60 * 1000

function limit100PerHour(req, res, next) {
  const now = Date.now()
  const entry = hits.get(req.ip)
  if (!entry || now - entry.start >= HOUR) {
    hits.set(req.ip, { start: now, count: 1 })
    return next()
  }
  entry.count += 1
  if (entry.count > 100) return res.status(429).send('Too many requests')
  next()
}

app.use(limit100PerHour)
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>
<details>
<summary>Создайте an Express.js route that accepts a JSON payload and responds with the same payload in reverse order.</summary>

### Создайте an Express.js route that accepts a JSON payload and responds with the same payload in reverse order.

**Definition:**

`express.json()` кладёт тело в `req.body`. Обработчик копирует массив, разворачивает копию и отвечает `res.json`.

**How it works:**

- Лимит JSON по умолчанию — `"100kb"`. `limit` поднимают, только если это осознанно.
- Тело, которое не массив, отвергают с 400.
- `Array.prototype.reverse` меняет массив на месте. Сначала копируют, чтобы не удивить другой middleware, который ещё держит `req.body`.
- Порядок ключей объекта в JSON не является контрактом. Этот маршрут разворачивает массив.

**Examples:**

```js
app.use(express.json())

app.post('/reverse', (req, res) => {
  if (!Array.isArray(req.body)) {
    return res.status(400).json({ error: 'expected a JSON array' })
  }
  res.json([...req.body].reverse())
})
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)
</details>
<details>
<summary>Разработайте a simple REST API with Express.js that includes CRUD operations for managing books. Include route definitions and handler functions that interact with a placeholder data store.</summary>

### Разработайте a simple REST API with Express.js that includes CRUD operations for managing books. Include route definitions and handler functions that interact with a placeholder data store.

**Definition:**

Ресурс книг — путь `/books`. `GET` читает, `POST` создаёт, `PUT` заменяет, `DELETE` удаляет. Вместо базы стоит `Map`.

**How it works:**

- Сначала `express.json()`, чтобы `POST` и `PUT` читали `req.body`.
- `POST` отвечает 201. Нет id — 404. `DELETE` существующей книги отвечает 204 через `res.status(204).end()`.
- Этот `Map` — память процесса. Гид по производительности говорит, что такое состояние не общее для воркеров кластера и пропадает при выходе процесса. Для всего, кроме примера, его заменяют базой.

**Examples:**

```js
const books = new Map()
let nextId = 1

app.get('/books', (req, res) => {
  res.json([...books.values()])
})

app.get('/books/:id', (req, res) => {
  const book = books.get(req.params.id)
  if (!book) return res.status(404).json({ error: 'not found' })
  res.json(book)
})

app.post('/books', (req, res) => {
  const id = String(nextId++)
  const book = { id, title: req.body.title }
  books.set(id, book)
  res.status(201).json(book)
})

app.put('/books/:id', (req, res) => {
  if (!books.has(req.params.id)) return res.status(404).json({ error: 'not found' })
  const book = { id: req.params.id, title: req.body.title }
  books.set(book.id, book)
  res.json(book)
})

app.delete('/books/:id', (req, res) => {
  if (!books.delete(req.params.id)) return res.status(404).json({ error: 'not found' })
  res.status(204).end()
})
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)
</details>

### Advanced Topics and Best Practices

<details>
<summary>Обсудите strategies for building a real-time application with Express.js.</summary>

### Обсудите strategies for building a real-time application with Express.js.

**Definition:**

Express обслуживает запрос и ответ HTTP. Обновления в реальном времени требуют долгого канала на том же сервере Node или отдельного сервиса. Гид миграции собирает сервер через `http.createServer(app)`.

**How it works:**

- HTTP-сервер создают из приложения Express и к нему присоединяют сервер WebSocket. Маршруты Express остаются запросом и ответом. Путь сокета — не `app.get`.
- Гид по производительности относит соединения WebSocket к состоянию процесса. Кластер их не делит. Нужна липкая маршрутизация или шина pub/sub, чтобы воркер достучался до клиента на другом воркере.
- Опрос обычным `GET` — запасной путь, если сокеты держать нельзя. Этот ответ кэшируют, только если он не на пользователя.
- Процесс Express остаётся асинхронным. Плотный цикл в обработчике сокета блокирует и HTTP.

**When to use:**

- WebSocket — сервер должен сам присылать данные.
- Короткий `GET` — инфраструктура не держит upgraded-соединения.

**Examples:**

```js
const http = require('node:http')
const app = require('./app')

const server = http.createServer(app)
server.listen(process.env.PORT || 3000)
```

**Sources:**

- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>
<details>
<summary>Что такое some common optimization techniques for Express.js applications?</summary>

### Что такое some common optimization techniques for Express.js applications?

**Definition:**

Действует тот же production-гид: меньше работы на запрос, нет блокирующих вызовов, прокси забирает статику, gzip и кэш.

**How it works:**

- `express.static` с `maxAge` для файлов с отпечатком в имени.
- `compression()` или gzip на Nginx, не оба сразу.
- `NODE_ENV=production`.
- Кластер или PM2 по ядрам и общее хранилище сессий.
- Лог через `morgan` или Pino, не `console.log` в терминал.
- Публичные GET кэшируют на Varnish или Nginx, как в разделе «Cache request results».

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express.static](https://expressjs.com/en/api.html#express.static)
</details>
<details>
<summary>Объясните the concept and benefits of server-side rendering with Express.js and a templating engine.</summary>

### Объясните the concept and benefits of server-side rendering with Express.js and a templating engine.

**Definition:**

Здесь серверный рендеринг значит, что Express собирает HTML через `res.render` до отправки ответа. Браузер получает документ, а не пустую оболочку, которая ждёт JavaScript.

**How it works:**

- Задают `view engine` и `views`. Данные передают вторым аргументом `res.render`.
- HTML можно индексировать, и он виден, когда JavaScript не выполняется. Первый ответ уже содержит текст.
- `NODE_ENV=production` кэширует скомпилированные шаблоны. Гид по производительности это отмечает.
- Секреты в вывод шаблона не кладут. Это тело ответа.
- JSON API это не нужно. Там `res.json`.

**Examples:**

```js
app.set('view engine', 'pug')
app.get('/books/:id', async (req, res) => {
  const book = await findBook(req.params.id)
  if (!book) return res.status(404).send('Not found')
  res.render('book', { book })
})
```

**Sources:**

- [Express — Using template engines](https://expressjs.com/en/guide/using-template-engines.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
</details>
<details>
<summary>Как you ensure that your Express.js code is maintainable and follows best practices?</summary>

### Как you ensure that your Express.js code is maintainable and follows best practices?

**Definition:**

Один роутер на область, middleware — именованные функции, ошибки — одна функция из четырёх аргументов в конце. Процесс настраивают из окружения.

**How it works:**

- Роутеры монтируют, а не растят один файл. Это образец гида Routing.
- Разбор тела и логгеры идут до маршрутов. 404 и обработчик ошибок — после. Гид по безопасности показывает этот порядок для своих 404 и 500.
- `async`-обработчики, чтобы отклонения доходили до Express. Код на callback вызывает `next(err)`.
- Берут сопровождаемый релиз Express. Гид по безопасности говорит, что 2.x и 3.x не сопровождаются.
- В production — `NODE_ENV=production`. Зависимости проверяют `npm audit`.

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>

### Case Studies and Scenario-Based Questions

<details>
<summary>Как бы вы structure an Express.js application for a large-scale e-commerce platform?</summary>

### Как бы вы structure an Express.js application for a large-scale e-commerce platform?

**Definition:**

Каталог, корзину и оплату разносят по роутерам. Процесс без состояния в памяти, чтобы поднять кластер. Запасы и заказы лежат в базе, не в `Map`.

**How it works:**

- `app.use('/catalog', catalogRouter)` и так же для корзины и оплаты.
- Сессии — в общем хранилище. Гид говорит, что воркеры кластера не делят память. Корзина в одном процессе невидима остальным.
- `express.json()` с `limit`. Картинки — через `express.static` или, лучше, через обратный прокси.
- Запись оплаты идёт параметризованными запросами. Гид по безопасности прямо говорит про SQL-инъекцию.
- `trust proxy` ставят, когда балансировщик завершает TLS.

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
</details>
<details>
<summary>Опишите the key considerations for building a secure API with Express.js.</summary>

### Опишите the key considerations for building a secure API with Express.js.

**Definition:**

Ввод проверяют, запросы к базе параметризуют, ставят заголовки безопасности и не отдают стек и заголовок `X-Powered-By`.

**How it works:**

- `helmet()` и `app.disable('x-powered-by')`.
- `express.json({ limit: '100kb' })` отвергает огромное тело. 100kb — документированный умолчательный лимит.
- Параметризованный SQL. Неожиданный тип отвергают 400 до базы.
- Сначала аутентификация, потом авторизация. 401 и 403 различают.
- TLS на прокси. `trust proxy` только для прокси, которыми вы управляете. Клиент подделает `X-Forwarded-For`, если доверять всем.
- `NODE_ENV=production`, чтобы обработчик по умолчанию не включал `err.stack`.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)
</details>
<details>
<summary>Обсудите how to handle session management in a distributed Express.js application.</summary>

### Обсудите how to handle session management in a distributed Express.js application.

**Definition:**

Больше одного процесса значит, что сессия не может жить в MemoryStore. У `express-session` должно быть хранилище, которое читает каждый процесс. Гид по производительности называет Redis.

**How it works:**

- Липкие сессии привязывают пользователя к одному процессу. Это ломается, когда процесс умирает. Общее хранилище это переживает.
- `cookie.secure` и `app.set('trust proxy', 1)`, потому что TLS обычно заканчивается на балансировщике.
- У cookie нестандартный `name`.
- `cookie-session` не нуждается в серверном хранилище, но вся сессия в cookie и видна клиенту. Гид по безопасности ограничивает это маленькими несекретными значениями.
- `trust proxy` не включают так широко, чтобы любой клиент подделал пересланные заголовки.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)
</details>
<details>
<summary>Что такое the implications of microservices architecture for an Express.js application?</summary>

### Что такое the implications of microservices architecture for an Express.js application?

**Definition:**

Каждый сервис — свой процесс Express со своими маршрутами и базой. Они говорят по сети. Обнаружения сервисов и шины сообщений в Express нет.

**How it works:**

- Процесс остаётся без состояния в памяти, как требует раздел про кластер, потому что копий каждого сервиса будет больше одной.
- Конфиг приходит из окружения. Логи идут в stdout (`morgan`). Это форма двенадцати факторов, которую ждёт платформа.
- Вызов другого сервиса идёт через `fetch` и должен пережить сбой таймаутом и статусом вроде 502. Медленный сервис не держат синхронными повторами на цикле событий.
- `trust proxy` применяют, когда впереди mesh или балансировщик.
- Режут только по настоящей границе (заказы и каталог). Один файл роутера проще, пока такой границы нет.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [The Twelve-Factor App](https://12factor.net/)
</details>

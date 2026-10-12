# TypeScript — Interview

Вопросы для собеседования по TypeScript. Ответы заполнены для опубликованной части, остальные пустые.

[← Назад к хабу](./5_ts.md)

### Related docs

[TypeScript — хаб](./5_ts.md) · [Типы](./5_ts_types.md) · [Сужение](./5_ts_narrowing.md) · [Функции](./5_ts_functions.md) · [Манипуляции типами](./5_ts_type_manipulation.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы из [Devinterview.io — TypeScript](https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/).

### TypeScript Fundamentals

<details>
<summary>Что такое TypeScript and how does it differ from JavaScript?</summary>

### Что такое TypeScript and how does it differ from JavaScript?

**Definition:**

TypeScript — статически типизированное надмножество JavaScript, которое разрабатывает и поддерживает Microsoft. Он делает код более поддерживаемым и предсказуемым. После компиляции TypeScript транспилируется в обычный JavaScript, совместимый с браузером.

**How it works:**

Ключевые отличия TypeScript от JavaScript — аннотации типов, возможность работать с уже существующим JavaScript и другое.

Возможности и преимущества TypeScript:

- Система типов: статическая типизация, разработчик задаёт тип переменных, параметров и возвращаемых значений. Ошибки ловятся во время разработки, а не в рантайме.
- Продвинутые возможности языка: современный синтаксис и возможности ECMAScript, часто раньше, чем они появляются в JavaScript. Кроме того, TypeScript добавляет приёмы функционального программирования, классы и модификаторы доступа (такие как `public` и `private`)
- Совместимость с JavaScript: TypeScript понимает существующий JavaScript с небольшими правками или вовсе без них.
- Инструменты и дополнительная надёжность: автодополнение, рефакторинг и документация в инструментах, которые понимают TypeScript. Типичные ошибки можно найти и исправить, не запуская код.
- Совместимость с ECMAScript: можно выбрать целевую версию ECMAScript, и сгенерированный JavaScript подойдёт для нужных браузеров.
- Структура и читаемость кода: заданная структура и ясность кода помогают поддерживать проект.

**When to use:**

Роль TypeScript в современной разработке:

- Применимость на работе: TypeScript используют в самом разном масштабе, от небольших утилит до крупных приложений.
- Поддержка сообщества: живое сообщество, частые обновления, исправления и полезные расширения.
- Непрерывная разработка: надёжный language server даёт обратную связь инструментов в реальном времени, например линтинг и подсказки об ошибках.
- Быстрое развитие: команда TypeScript постоянно добавляет новые возможности и укрепляет уже существующие.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Можете ли вы explain what is meant by "TypeScript is a superset of JavaScript"?</summary>

### Можете ли вы explain what is meant by "TypeScript is a superset of JavaScript"?

**Definition:**

TypeScript часто называют «надмножеством JavaScript», потому что любая корректная программа на JavaScript — корректная программа на TypeScript.

TypeScript устроен так, что полностью принимает существующий синтаксис и возможности JavaScript. За счёт этого переход или миграция проходят мягко.

**How it works:**

Ключевые возможности TypeScript поверх JavaScript:

- Определения типов: статическая типизация через аннотации. Они необязательны, поэтому существующую кодовую базу можно переводить постепенно, если типы ей пока не нужны.
- Более новый JavaScript: TypeScript расширяет синтаксис JavaScript и через компилятор даёт последние стандарты ECMAScript даже тогда, когда сам движок их ещё не поддерживает.
- Инструменты и поиск ошибок: проверка типов, более читаемый код и более жёсткие ошибки на этапе компиляции.

**Examples:**

Демонстрация кода. Вот код на TypeScript:

```ts
let num: number = 5;
num = "this will raise a type error";
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Что такое the basic types available in TypeScript?</summary>

### Что такое the basic types available in TypeScript?

**Definition:**

В TypeScript есть набор базовых типов для разных данных: числа, строки, логические значения, массивы, кортежи и другие.

**How it works:**

Основные базовые типы TypeScript:

- Boolean: значения true/false.
- Number: и целые, и числа с плавающей точкой.
- String: текстовые данные.
- Array: удобная работа со структурированными данными.
- Tuple: массив фиксированной длины, элементы которого могут иметь разные типы.
- Enum: набор именованных констант, например дни или цвета.
- Any: динамический тип, которым можно обойти проверку типов. Обычно его лучше избегать: он противоречит главной идее TypeScript, статической типизации. Но бывают случаи, когда он необходим.
- Void: обычно тип возврата функции, которая ничего не возвращает.
- Null и Undefined: позволяют присвоить `null` и `undefined`. По умолчанию это не включено, и такие значения лучше обрабатывать через настройки режима `strict`.
- Never: тип значений, которые никогда не возникают. Например, тип возврата функции, которая не доходит до конца или всегда бросает ошибку.
- Object: любой объект JavaScript.
- Function: функциональный тип.

**Examples:**

Пример кода: базовые типы TypeScript. Вот код на TypeScript:

```ts
// Boolean
let isActive: boolean = true;

// Number
let age: number = 30;

// String
let title: string = "Manager";

// Array
let scores: number[] = [85, 90, 78];
// or use a compact form: let scores: Array<number> = [85, 90, 78];

// Tuple
let employee: [string, number, boolean] = ['John', 35, true];

// Enum
enum WeekDays { Monday, Tuesday, Wednesday, Thursday, Friday }
let today: WeekDays = WeekDays.Wednesday;

// Any
let dynamicData: any = 20;

// Void
function greet(): void {
  console.log("Hello!");
}

// Null and Undefined
let data: null = null;
let user: undefined = undefined;

// Never
function errorMessage(message: string): never {
  throw new Error(message);
}

// Object
let person: object = {
  name: 'John',
  age: 30
};

// Function
let calculate: Function;
calculate = function (x: number, y: number): number {
  return x + y;
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Как you declare variables in TypeScript?</summary>

### Как you declare variables in TypeScript?

**Definition:**

В TypeScript объявление переменных поддерживает разные способы задать саму переменную и связанный с ней тип.

**How it works:**

Способы объявления переменной и типа:

1. var

```ts
var score: number = 100;
```

Такое объявление приводит к всплытию переменной и имеет глобальную или функциональную область видимости.

2. let

`let` используют, когда переменная нужна в пределах блока. Это рекомендуемый выбор по умолчанию:

```ts
let playerName: string = "John";
```

3. const

`const` объявляет константы и особенно полезен, когда нужно сохранить целостность данных:

```ts
const apiKey: string = "your-api-key-here";
```

4. Область видимости функции

Все три способа (`var`, `let` и `const`) ограничены ближайшей функцией:

```ts
function doSomething() {
  let tempValue: number = 42;
  var result: boolean = true;
}
```

Правила объявления и инициализации переменных:

- Порядок важен: в TypeScript переменную нужно объявить до использования. В JavaScript это не обязательно, но хорошая практика — объявлять переменную до того, как её используют.

Если код сложный и связанный, стоит брать `let` и `const`: блочная область видимости снижает проблемы со всплытием.

- Статические типы: TypeScript требует объявить тип переменной (или дать системе вывести его), а затем инициализировать значением ровно этого типа:

```ts
let count: number;  // Declaration
count = 42;  // Allowed
count = "42";  // Error! Type 'string' is not assignable to type 'number'.
```

- Вывод типа: TypeScript часто выводит тип по значению инициализации. Явно указывать тип тогда не нужно.

```ts
let word = "hello!";  // TypeScript infers the type as 'string' because of the initialization.
```

**When to use:**

Практические рекомендации по объявлению переменных:

- Берите `const`, где можете: это не всегда возможно, особенно со свойствами объектов. Но `const` лучше читается и защищает от случайной подмены данных.
- Предпочитайте `let`, а не `var`: у `let` блочная область видимости и более предсказуемое поведение.
- Инициализируйте при объявлении: TypeScript позволяет инициализировать позже, но яснее и безопаснее объявлять и задавать значение сразу.
- Предпочитайте аннотации типов: явный тип улучшает читаемость. Если тип и так очевиден из инициализации, хватает вывода типа.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Что такое interfaces in TypeScript and how do they work?</summary>

### Что такое interfaces in TypeScript and how do they work?

**Definition:**

В TypeScript интерфейс задаёт структуру и типы своих членов. Это контракт на обязательные свойства и методы: класс или объект, который его реализует, должен этой структуре соответствовать.

**How it works:**

Ключевые свойства интерфейсов:

- Согласованность типов: объект, который соблюдает структуру интерфейса, считается с ним совместимым.
- Необязательные и readonly-члены: необязательные атрибуты и члены только для чтения задаются ключевыми словами `?` и `readonly`.
- Сигнатуры вызова: интерфейс может описать тип метода, параметры функции и тип возврата.
- Индексные сигнатуры: удобны, когда у объекта может быть любое число свойств одного и того же типа.

**When to use:**

Основные сценарии:

- Единый вид объектов: разные объекты получают общую структуру, с ними проще работать согласованно.
- Контракт для классов: обязательные свойства и методы снижают число ошибок и упрощают поддержку.

**Examples:**

Пример кода: простой интерфейс. Вот код на TypeScript:

```ts
interface Point {
    x: number;
    y: number;
}
  
function printPoint(p: Point) {
    console.log(`Point coordinates: (${p.x}, ${p.y})`);
}

let pointA = { x: 3, y: 7 }; // This object matches Point's structure
let pointB = { x: 8 }; // This object is missing the 'y' property

printPoint(pointA); // Output: Point coordinates: (3, 7)
printPoint(pointB); // Compile-time error due to incorrect structure
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Опишите the Enum type and when you might use it.</summary>

### Опишите the Enum type and when you might use it.

**Definition:**

Enum — тип данных, который упрощает представление и работу с дискретными именованными значениями. Это базовый инструмент типобезопасности в TypeScript, и у него несколько важных применений:

- Меньше «магических значений»: читаемость и отказ от повторяющихся литералов вроде `1`, `2` или `'red'`.
- Настройка поведения: влияет на наборы связанных значений, например HTTP-методы, порядок или типы клиентов.
- Типобезопасность и однозначность: заранее заданный набор допустимых членов и ясный тип делают присваивания и операции однозначными и согласованными.

**How it works:**

Основные части:

- Ключ: уникальный идентификатор, обычно число или строка.
- Значение: данные, связанные с ключом. Если значение не задано, в качестве значения используется сам ключ.

Обычный, строковый и смешанный enum:

- Обычный enum: ключ и значение одного типа, как правило числового.
- Строковый enum: все ключи и значения — строки, представление данных остаётся единообразным.
- Смешанный enum: ключи и с числовыми, и со строковыми значениями. Из-за смешанных типов в большинстве случаев от таких enum лучше держаться подальше.

Прагматичное использование enum:

Хотя enum — сильный инструмент для поддержания типобезопасности, они упрощают связывание родственных наборов значений.

При этом значение enum можно вывести или принудительно привести к любому ключу и нижележащему типу значения.

**Pluses and minuses:**

Возможные минусы enum:

- Влияние на компиляцию: в более широком контексте или в структурах вроде массивов и map TypeScript порождает дополнительный код, чтобы превратить ключи enum в связанные значения.
- Память: каждое использование enum требует выделить память под его значение.

Если хватает простой константы или нужна более динамичная связь ключей и значений, подробный тип будет лучшей заменой.

**Examples:**

Пример кода: обычный enum. Вот код на TypeScript:

```ts
enum HttpMethods {
  GET,
  POST,
  PUT,
  DELETE
}

const requestType: HttpMethods = HttpMethods.GET;

// ❌ This is not allowed due to type safety
// const requestType2: HttpMethods = 'GET';
```

В примере ключу `GET` неявно присваивается значение `0`.

Пример кода: строковый enum. Вот код на TypeScript:

```ts
enum MediaTypes {
  Image = 'image',
  Video = 'video',
  Audio = 'audio'
}

const selectedType: MediaTypes = MediaTypes.Image;

// ✅ Accessing the value
const associatedText: string = MediaTypes.Image;

// ❌ This is not allowed due to type safety
// const invalidType: MediaTypes = 'image';
```

Enum помогает гарантировать нужный тип данных и его значения.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Как you define and use a function in TypeScript?</summary>

### Как you define and use a function in TypeScript?

**Definition:**

Когда в TypeScript определяют функцию, учитывают такие основные части:

- Сигнатура функции: назначение функции, параметры, тип и возвращаемое значение.
- Тело функции: сама операция или последовательность шагов, которые функция выполняет.

**How it works:**

1) Объявление функции

У функции указывают имя, список параметров и тип возврата. Если функция ничего не возвращает, тип возврата — `void`.

2) Функциональное выражение

Функцию можно объявить выражением: присвоить её переменной как значение. Так гибче, например когда нужны колбэки.

3) Необязательные параметры и параметры по умолчанию

TypeScript поддерживает и необязательные параметры, и параметры со значением по умолчанию, функция становится гибче.

Необязательный параметр обозначают символом `?` после имени.

Параметр по умолчанию — это параметр, которому сразу задают значение.

4) Rest-параметры

Параметр можно объявить как rest: функция примет любое число аргументов для него.

5) Перегрузки функции

Можно объявить несколько перегрузок: набор параметров и их типы возврата для одной функции. Это особенно полезно, когда поведение логически зависит от типов входа.

6) Сигнатура вызова

У объектов в TypeScript есть сигнатура вызова: ею задают ожидаемую структуру функции для конкретного метода объекта.

**Examples:**

Вот пример кода:

```ts
function greet(name: string): void {
    console.log(`Hello, ${name}!`);
}
```

Вот пример:

```ts
let greet: (name: string) => void;
greet = function(name: string): void {
    console.log(`Hello, ${name}!`);
};
```

Вот пример кода:

```ts
function greet(name: string, title?: string) {
    if (title) {
        console.log(`Hello, ${title} ${name}!`);
    } else {
        console.log(`Hello, ${name}!`);
    }
}
```

Вот пример кода:

```ts
function greet(name = "Stranger") {
    console.log(`Hello, ${name}!`);
}
```

Вот пример кода:

```ts
function introduce(greeting: string, ...names: string[]) {
    console.log(`${greeting}, ${names.join(", ")}!`);
}

introduce("Hello", "Alice", "Bob", "Carol");
```

Вот пример кода:

```ts
function specialGreet(name: string): void;
function specialGreet(title: string, name: string): void;

function specialGreet(a: any, b?: any): void {
    if (b) {
        console.log(`Hello, ${a}, ${b}`);
    } else {
        console.log(`Hello, ${a}`);
    }
}
```

Вот пример кода:

```ts
type Greeter = {
    (name: string): void
};

let welcome: Greeter;
welcome = function(name: string): void {
    console.log(`Welcome, ${name}!`);
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Что означает "type inference" mean in the context of TypeScript?</summary>

### Что означает "type inference" mean in the context of TypeScript?

**Definition:**

В TypeScript вывод типа — основная возможность: тип переменной определяется по её значению автоматически. Статическая типизация остаётся, а явные аннотации не обязательны.

**How it works:**

TypeScript выводит тип алгоритмом лучшего общего типа. Если при присваивании или в литерале массива у переменной встречается несколько типов, он строит их объединение и выбирает лучший общий тип.

TypeScript может вывести наиболее вероятный тип из контекста, например:

- Если значение присваивают сразу, переменная получает тип этого значения.
- Чтобы выбрать лучший общий тип, используется информация соседних типов. Если все значения совместимы с одним типом, берётся он.

**Pluses and minuses:**

Плюсы вывода типа:

- Краткость: явные объявления типов не нужны, код компактнее и читается легче.
- Гибкость: типы в кодовой базе естественно совпадают со значениями, и при смене значений код проще поддерживать.
- Меньше ошибок: ниже риск расхождения между объявленным типом и фактическим значением.

**Examples:**

Пример кода: вывод типа. Рассмотрим такой код:

```ts
let value = 10; // Type 'number' inferred
let message = "Hello, TypeScript!"; // Type 'string' inferred

function add(a: number, b: number) {
    return a + b;
}

let sum = add(5, 7); // Type 'number' inferred
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Объясните the use of 'let' and 'const' in TypeScript.</summary>

### Объясните the use of 'let' and 'const' in TypeScript.

**Definition:**

TypeScript использует `const` и `let` для объявления переменных. Эти два ключевых слова дают явность, область видимости и неизменяемость, и код проще поддерживать.

**How it works:**

Главные различия:

- const: константа, которая после объявления не меняется. Важно: неизменна ссылка, а внутреннее состояние сложных объектов, например массивов, само по себе не защищено от изменений.
- let: обычная изменяемая переменная.

**Examples:**

Пример кода: `const`. Вот код на TypeScript:

```ts
const productId: number = 5;
let productName: string = 'Tesla';

const getProductDetails = (id: number): string => {
  return `Product ID: ${id}`;
};

// Attempting to modify will result in a compilation error
// productId = 6;

// Reference is still immutable
const anotherProductId: number = 10;
// This will throw a compilation error since it's a constant
// anotherProductId = 12;

// Modifying internal state of an object is allowed for a const
const myArray: number[] = [1, 2, 3];
myArray.push(4);
```

Пример кода: `let`. Вот код на TypeScript:

```ts
let vehicleType: string = 'Car';

if (true) {
  let vehicleType: string = 'Motorcycle';
  console.log(vehicleType);  // Output: Motorcycle
}

console.log(vehicleType);  // Output: Car
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Как you compile TypeScript files into JavaScript?</summary>

### Как you compile TypeScript files into JavaScript?

**Definition:**

Компиляция TypeScript (.ts) в JavaScript (.js) идёт через компилятор TypeScript (`tsc`). Процесс настраивают в `tsconfig.json`, а для нужд проекта можно подключить и более сложные способы.

**How it works:**

Шаги:

1. Создание файлов: пишут файлы TypeScript (.ts).
2. Настройка компилятора: создают `tsconfig.json` с параметрами компиляции.
3. Компиляция: запускают команду `tsc`.
4. Проверка результата: смотрят сгенерированные файлы JavaScript.

**Examples:**

Конфигурация TypeScript (`tsconfig.json`). Вот файл `tsconfig.json`. Полное руководство по конфигурации доступно здесь.

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "strict": true,    
    "outDir": "dist",
    "rootDir": "src"  
  },
  "include": [
    "src/**/*.ts"    
  ],
  "exclude": [      
    "node_modules",   
    "**/*.spec.ts"    
  ]
}
```

Практический пример: приложение Vineyard Residential для управления задачами. Вот практичный и подробный файл `tsconfig.json`.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["dom", "es2015", "es5", "es6", "es7", "es2015.collection"],
    "allowJs": true,
    "checkJs": false,
    "jsx": "react",
    "declaration": false,
    "sourceMap": true,
    "outDir": "dist",  
    "rootDir": "src",
    "strict": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "removeComments": true,
    "suppressImplicitAnyIndexErrors": true,
    "typeRoots": ["node_modules/@types", "custom-typings"],
    "baseUrl": ".",
    "paths": {
      "components/*": ["src/components/*"],
      "utils/*": ["src/utils/*"],
    },
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "incremental": true,
    "diagnostics": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "newLine": "LF",
    "watchOptions": {
      "watchFile": "useFsEvents",  
      "fallbackPolling": "dynamicPriority",   
      "polling": true,
      "esModuleInterop": true,
            "pollingInterval": 2500,
      
      "followSymlinks": true
    }
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "@types"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

**Thoughts:**

Продвинутая конфигурация:

- Ссылки на проекты: полезны, когда большой проект делят на части.
- Пользовательские трансформеры: своя логика во время компиляции.
- Программный API: гибкое управление настройками компилятора и запуском.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

### TypeScript Classes and Objects

<details>
<summary>Объясните Classes in TypeScript. How are they different from ES6 Classes?</summary>

### Объясните Classes in TypeScript. How are they different from ES6 Classes?

**Definition:**

Классы TypeScript и ES6 во многом похожи, но у классов TypeScript есть дополнительные возможности и строгая типизация, и код от этого надёжнее.

**How it works:**

Общие возможности классов:

- Наследование: подклассы (потомки) расширяют родительские классы и получают их методы и свойства.
- Полиморфизм: производный класс может объявить метод с тем же именем, что у родителя.
- Инкапсуляция: данные скрывают модификаторами доступа, такими как `public`, `private` и `protected`.
- Конструктор: создание экземпляра начинается с метода-конструктора, если он определён.

Возможности, которые есть у классов TypeScript:

Объявление полей

В TypeScript поле можно объявить прямо в классе, не инициализируя его. При создании объекта оно автоматически становится `undefined`. В ES6 поле нужно инициализировать в конструкторе или в самом объявлении.

```ts
class Example {
    // Field is automatically initialized to undefined upon object creation
    someField: string;
}
```

Абстрактные классы

TypeScript поддерживает абстрактные классы как чертёж для других классов. Сами по себе их нельзя создать, но они могут дать реализацию, которую производные классы переопределяют.

```ts
abstract class AbstractExample {
    abstract someMethod(): void;  // Method has no implementation (abstract)
}
```

Свойства только для чтения

Свойство класса можно пометить `readonly`: его задают только при объявлении или в конструкторе класса.

```ts
class Example {
    readonly id: number;
    constructor(id: number) {
        this.id = id;  // Readonly can only be assigned in the constructor or declaration
    }
}
```

Статические члены

Классы TypeScript поддерживают статические члены: свойства и методы, которые принадлежат самому классу, а не экземплярам.

```ts
class Example {
    static count = 0;  // Static property
    static incrementCount() {
        Example.count++;
    }
}
```

Функции доступа

В TypeScript можно объявить функции `get` и `set`, аксессоры, и так управлять чтением и изменением свойств класса.

```ts
class Example {
    private _name: string;

    get name(): string {
        return this._name;
    }

    set name(newName: string) {
        this._name = newName.trim();
    }
}
```

Свойства-параметры

TypeScript даёт короткий способ объявить свойство и инициализировать его из параметра конструктора. Код короче, особенно когда параметр конструктора напрямую соответствует свойству класса.

```ts
class Example {
    constructor(private _name: string, public age: number) {
        // Private _name property created and initialized from constructor parameter
        // Public age property created and initialized from constructor parameter
    }
}
```

Типы пересечения для классов

В TypeScript, когда вы объявляете базовый класс, а потом расширяете его, вы создаёте тип пересечения. Это значит, что дочерний класс унаследует все свойства и методы и от родителя (родителей), и от себя.

Дополнительные возможности ES6, которых нет в TypeScript:

Выражения классов

И ES6, и TypeScript поддерживают выражения классов: класс можно определить без имени.

В ES6:

```javascript
const Animal = class {
    // Class methods and properties defined here
};
```

Протокол итератора

Классы ES6 поддерживают протокол итератора, и по объектам проще проходить циклом.

```ts
class IterableExample implements Iterable<string> {
    // Implement iterator function for strings
    Symbol.iterator {
        let index = 0;
        const data = ['one', 'two', 'three'];
        return {
            next: () => {
                if (index < data.length) {
                    return { value: data[index++], done: false };
                }
                return { value: undefined, done: true };
            }
        };
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Как you implement inheritance in TypeScript?</summary>

### Как you implement inheritance in TypeScript?

**Definition:**

Посмотрим, как использовать наследование в TypeScript и через классы ES6, и через прототипное наследование.

**How it works:**

1. Наследование синтаксисом классов ES6

С появлением ES6 появился более привычный способ наследования через классы. Его обычно легче читать и понимать.

Пример кода: наследование классами ES6. Вот код на TypeScript:

```ts
class Animal {
  private name: string;

  constructor(theName: string) {
    this.name = theName;
  }

  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends Animal {
  constructor(name: string) {
    super(name);
  }

  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

const mySnake = new Snake("Cobra");
mySnake.move();  // Output: Slithering... Cobra moved 5m.
```

2. Наследование прототипным способом

До ES6 TypeScript, как и JavaScript, использовал прототипное наследование.

Прототипный механизм полезен для сложных структур объектов, но нюансы нужно понимать, иначе поведение будет неожиданным.

Пример кода: прототипное наследование в TypeScript. Вот код на TypeScript:

```ts
// Define the Parent Class
function Animal(this: Animal, name: string) {
  this.name = name;
}

Animal.prototype.move = function(distanceInMeters: number = 0) {
  console.log(`${this.name} moved ${distanceInMeters}m.`);
};

// Define the Child Class
function Snake(name: string) {
  Animal.call(this, name);
}

// Set up the Inheritance
Snake.prototype = Object.create(Animal.prototype);
Snake.prototype.constructor = Snake;

// Override the Base Type's Method
Snake.prototype.move = function(distanceInMeters = 5) {
  console.log("Slithering...");
  Animal.prototype.move.call(this, distanceInMeters);
};

const mySnake = new Snake("Cobra");
mySnake.move();  // Output: Slithering... Cobra moved 5m.
```

Упрощённые способы

Прототипная конструкция поначалу сбивает с толку, но её сила в гибкости.

Сложностей прямого присваивания прототипов можно избежать конструкциями, выведенными из ES5, как дальше:

```ts
function Animal(name: string) {
  this.name = name;
}

Animal.prototype.move = function(distanceInMeters: number = 0) {
  console.log(`${this.name} moved ${distanceInMeters}m.`);
};

function Snake(name: string) {
  Animal.call(this, name);
}

// Utilize `Object.create` for simplified prototype delegation
Snake.prototype = Object.create(Animal.prototype);
Snake.prototype.constructor = Snake;

Snake.prototype.move = function(distanceInMeters = 5) {
  console.log("Slithering...");
  Animal.prototype.move.call(this, distanceInMeters);
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Что такое access modifiers and how do they work in TypeScript?</summary>

### Что такое access modifiers and how do they work in TypeScript?

**Definition:**

Модификаторы доступа — способ TypeScript управлять видимостью и изменяемостью членов класса. Они обеспечивают инкапсуляцию и особенно полезны в объектно-ориентированном проектировании.

**How it works:**

Основные модификаторы:

- Public: значение по умолчанию для членов класса. К ним можно обратиться и изнутри класса, и снаружи.
- Protected: члены доступны внутри класса и в его подклассах. Они помогают выразить отношение «является».
- Private: член доступен только внутри класса, где он объявлен. Снаружи его нельзя ни прочитать, ни изменить.

**Examples:**

Пример кода: модификаторы доступа в деле. Вот код на TypeScript:

```ts
class Person {
    public name: string;
    private age: number;
    protected contact: string;

    constructor(name: string, age: number, contact: string) {
        this.name = name;
        this.age = age;
        this.contact = contact;
    }
}

class Employee extends Person {
    private employeeId: string;

    constructor(name: string, age: number, contact: string, employeeId: string) {
        super(name, age, contact);
        this.employeeId = employeeId;
    }

    public displayDetails(): void {
        console.log(`${this.name} - ${this.age} - ${this.contact} - ${this.employeeId}`);
    }
}

// Somewhere in your code
const person = new Person("John Doe", 30, "1234567");
console.log(person.name);  // Accessible
console.log(person.age);   // ERROR: 'age' is private

const employee = new Employee("Jane Doe", 25, "2345678", "E123");
console.log(employee.contact);  // ERROR: 'contact' is protected
employee.displayDetails();     // Correctly displays details

employee.age = 35;    // ERROR: 'age' is private
employee.contact = "3456789";  // ERROR: 'contact' is protected
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Обсудите abstract classes and their puRPOses in TypeScript.</summary>

### Обсудите abstract classes and their puRPOses in TypeScript.

**Definition:**

В TypeScript абстрактные классы служат чертежами для производных классов: задают структуру и не обязаны давать полную реализацию методов.

**How it works:**

Основные свойства абстрактных классов:

Сигнатуры методов

Абстрактный класс задаёт сигнатуры методов, не описывая их работу. Производные классы получают готовую форму, от которой можно отталкиваться.

Конкретные определения методов

Кроме сигнатур, абстрактный класс может содержать полностью реализованные методы. Они либо поддерживают абстрактные методы, либо живут как самостоятельная функциональность.

Разделение абстрактных и неабстрактных членов

Абстрактный класс явно отделяет методы, которые производный класс обязан реализовать, от тех, что уже реализованы или необязательны.

Служебная возможность TypeScript: статические свойства

У абстрактного класса в TypeScript могут быть статические члены. Они принадлежат самому классу, а не конкретному экземпляру. Так удобно задавать свойства и методы, которые доступны без создания экземпляра.

**When to use:**

Обычные сценарии абстрактных классов:

- Повторное использование: общая или разделяемая функциональность нескольких производных классов собирается в одном месте.
- Контракт: производные классы следуют общей структуре, и набор методов, которые нужно реализовать, определён заранее.
- Частичная реализация: полностью готовые методы стоят рядом с теми, которым нужна конкретная реализация в производном классе.

**Examples:**

Пример кода: абстрактный класс. Вот код на TypeScript:

```ts
abstract class Shape {
    abstract getArea(): number;
    abstract getPerimeter(): number;
    color: string;

    constructor(color: string) {
        this.color = color;
    }

    static defaultColor: string = 'red';

    describe() {
        return `This shape is ${this.color}.`;
    }
}

// This will throw an error because the derived class does not provide concrete implementations for abstract methods.
class Circle extends Shape { 
    constructor(public radius: number, color: string) {
        super(color);
    }

    // The 'Circle' class inherited the following properties from 'Shape', but neither implements nor specifies them in the derived class: 'getArea' and 'getPerimeter'.
    getArea(): number {
        return Math.PI * this.radius ** 2;
    }

    getPerimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

const myCircle = new Circle(5, 'blue');
console.log(myCircle.getArea()); // Outputs: 78.54
console.log(myCircle.describe()); // Outputs: This shape is blue.
console.log(Shape.defaultColor); // Outputs: red
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Можете ли вы describe the use of constructors within TypeScript Classes?</summary>

### Можете ли вы describe the use of constructors within TypeScript Classes?

**Definition:**

TypeScript даёт удобный способ задать конструктор класса ключевым словом `constructor`. Метод-конструктор инициализирует члены класса и может иметь спецификаторы доступа. Им задают начальное состояние объекта.

**How it works:**

Ключевые свойства:

- Автоматический вызов: конструктор вызывается сам, когда создают объект класса.
- Один конструктор: у класса только один конструктор, инициализация собрана в одном месте.
- Перегрузки: конструктор можно перегрузить и задать несколько способов создать объект.

Пример: конструктор в TypeScript. Ключевое слово `this` указывает на текущий экземпляр, и данные попадают куда нужно.

**Examples:**

```ts
class Person {
  // Member variables
  name: string;
  age: number;
  
  // Constructor
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
```

**Thoughts:**

Модификаторы доступа конструктора

TypeScript поддерживает модификаторы доступа у параметров конструктора: инициализация класса получается короче и безопаснее.

- Public: параметр без модификатора по умолчанию публичный.
- Private: ключевое слово `private` делает параметр доступным только внутри класса.
- Только для чтения: `readonly` вместе с параметром и модификатором `private` или `public` гарантирует, что значение присваивается лишь один раз, в конструкторе.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Что такое Property decorators and how do you use them?</summary>

### Что такое Property decorators and how do you use them?

**Definition:**

Декоратор свойства — это функция с `@` над полем класса. В текущем TypeScript она вызывается при объявлении класса, получает поле и объект контекста и может записать метаданные об этом поле. Новому коду флаг `--experimentalDecorators` не нужен.

**How it works:**

- **Место:** `@mark` стоит сразу над полем, например `name = "Ada"`. Та же форма `@` работает для методов, геттеров, сеттеров, автоаксессоров и самого класса. Декораторы параметров в эту модель не входят.
- **Вызов:** Функция получает декорируемое значение и контекст. Для поля тип контекста — `ClassFieldDecoratorContext`. В документации через него читают `name`, `static`, `private` и `metadata`.
- **Метаданные:** Все декораторы класса делят один объект `context.metadata`. После декорирования его читают у конструктора через `Symbol.metadata`. В большинстве сред нужен полифил `Symbol.metadata`, `target` не выше `es2022` и `"esnext.decorators"` в `lib`.
- **Старый флаг:** `--experimentalDecorators` по-прежнему есть и проверяет другую, более старую форму (`target`, `propertyKey`). Декоратор, написанный под этот флаг, без него обычно не работает как есть. Новая модель несовместима с `--emitDecoratorMetadata`.

**When to use:**

- Пометить публичные поля экземпляра для сериализации, отладки или небольшого реестра фреймворка.
- Не ставить декоратор, если хватает обычного поля или метода. Старый флаг нужен только если проект уже зависит от экспериментальных декораторов, например от старых метаданных Angular или Nest.

**Examples:**

```ts
function mark(_target: any, context: ClassFieldDecoratorContext): void {
  context.metadata[context.name] = true;
}

class User {
  @mark
  name = "Ada";
}

console.log(User[Symbol.metadata]);
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)
- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)

</details>

<details>
<summary>Как getters and setters work in TypeScript?</summary>

### Как getters and setters work in TypeScript?

**Definition:**

Геттеры и сеттеры — это аксессоры `get` и `set` у класса или объекта. Снаружи их читают и пишут как свойство, а тело аксессора выполняется при каждом обращении.

**How it works:**

- **Пара:** `get size()` возвращает сохранённое значение. `set size(value)` принимает присваивание. Пара get/set, которая только проксирует поле и не добавляет логики, почти никогда не нужна. Достаточно публичного поля.
- **Вывод типов:** Если есть геттер и нет сеттера, свойство становится `readonly`. Если у параметра сеттера нет типа, TypeScript берёт его из возвращаемого типа геттера.
- **Разные типы чтения и записи:** С TypeScript 4.3 сеттер может принимать более широкий тип, чем возвращает геттер. Тип геттера при этом должен быть совместим с типом сеттера, чтобы свойство можно было присвоить самому себе.
- **Какой тип проверяется:** Связи между свойствами смотрят на тип чтения. Тип записи проверяется только при присваивании. То же разделение работает в объектных литералах и в интерфейсах (`get size(): number` и `set size(value: ...)`).

**When to use:**

- Спрятать приватное поле и проверить или преобразовать значение на входе, например строку в число.
- Отдать вычисляемое значение. Если логики нет, оставьте публичное поле.

**Examples:**

```ts
class Thing {
  #size = 0;

  get size(): number {
    return this.#size;
  }

  set size(value: string | number | boolean) {
    const num = Number(value);
    this.#size = Number.isFinite(num) ? num : 0;
  }
}

const thing = new Thing();
thing.size = "12";
const reading: number = thing.size;
```

**Sources:**

- [TypeScript handbook — Classes, Getters / Setters](https://www.typescriptlang.org/docs/handbook/2/classes.html#getters--setters)
- [TypeScript 4.3 — Separate Write Types on Properties](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-3.html#separate-write-types-on-properties)

</details>

<details>
<summary>Объясните Method Overloading in TypeScript.</summary>

### Объясните Method Overloading in TypeScript.

**Definition:**

Сигнатуры перегрузки описывают разные способы вызова функции или метода. Снаружи видны только они. Под ними одна сигнатура реализации с телом. Эта сигнатура реализации не входит в публичный список вызовов.

**How it works:**

- **Порядок:** Сначала две или больше сигнатур перегрузки, затем реализация. Реализация должна быть совместима с каждой перегрузкой. Её параметры и тип возврата — не то, чем может пользоваться вызывающий код.
- **Выбор:** TypeScript выбирает одну перегрузку на вызов. Значение с типом-объединением входов часто не подходит ни к одной, даже если каждый член по отдельности подошёл бы.
- **Лучше объединение:** Если перегрузки принимают одинаковое число аргументов и возвращают один и тот же тип, одна сигнатура с параметром-объединением проще и для вызова, и для реализации. В справочнике прямо сказано предпочитать union-параметры перегрузкам, когда это возможно.
- **Классы:** Тот же приём работает для методов и для перегрузок конструктора. В рантайме остаётся только реализация. Лишние сигнатуры стираются.

**When to use:**

- У функции правда разные числа аргументов или разные типы возврата, например один аргумент против трёх.
- Не перегружайте `len(s: string): number` и `len(arr: unknown[]): number`. Напишите `len(x: string | unknown[]): number`.

**Examples:**

```ts
function makeDate(timestamp: number): Date;
function makeDate(month: number, day: number, year: number): Date;
function makeDate(monthOrTimestamp: number, day?: number, year?: number): Date {
  if (day !== undefined && year !== undefined) {
    return new Date(year, monthOrTimestamp, day);
  }
  return new Date(monthOrTimestamp);
}

const fromStamp = makeDate(1_234_567_890);
const fromParts = makeDate(4, 10, 2026);
```

**Sources:**

- [TypeScript handbook — Function Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)

</details>

<details>
<summary>Что такое the puRPOse of the 'static' keyword in TypeScript Class?</summary>

### Что такое the puRPOse of the 'static' keyword in TypeScript Class?

**Definition:**

`static` помещает поле, метод, аксессор или блок инициализации на конструктор класса. Это общее состояние класса, а не данные каждого экземпляра.

**How it works:**

- **Доступ:** Статические члены читают и вызывают на конструкторе: `Counter.next()`. Это не свойства экземпляра. Подклассы их наследуют, поэтому `Derived.getGreeting()` видит `Base.getGreeting`.
- **Видимость:** `public`, `protected` и `private` применяются к статическим членам так же, как к членам экземпляра.
- **Статические блоки:** Блок `static { ... }` выполняется при инициализации класса, в порядке исходника. У него своя область видимости, и он может читать приватные статические поля. В классе может быть несколько таких блоков.
- **Занятые имена:** Не объявляйте статические `name`, `length` и `call`. Они сталкиваются со свойствами `Function`. Конструкции `static class` в стиле C# в TypeScript нет. Обычно вместо неё хватает обычного объекта или модуля.

**When to use:**

- Счётчики, кэши и фабричные помощники, которые относятся к типу, а не к одному объекту.
- Данные одного объекта держите в полях экземпляра. Если `new` не нужен, экспортируйте обычный объект, а не класс из одних статических членов.

**Examples:**

```ts
class Counter {
  static #count = 0;

  static {
    Counter.#count = 1;
  }

  static next(): number {
    return ++Counter.#count;
  }
}

console.log(Counter.next());
```

**Sources:**

- [TypeScript handbook — Static Members](https://www.typescriptlang.org/docs/handbook/2/classes.html#static-members)
- [TypeScript 4.4 — Static Blocks in Classes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-4.html#static-blocks-in-classes)
- [MDN — static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)

</details>

### TypeScript Types and Type Assertions

<details>
<summary>Как you define custom types with Type Aliases?</summary>

### Как you define custom types with Type Aliases?

**Definition:**

Псевдоним типа даёт имя любому типу: форме объекта, объединению, кортежу, примитиву или другому псевдониму. Имя значит то же самое, что и тип, на который оно указывает. Отдельного рантайм-бренда оно не создаёт.

**How it works:**

- **Синтаксис:** `type ID = number | string` или `type Point = { x: number; y: number }`.
- **Псевдоним прозрачен:** `type UserInputSanitizedString = string` — это по-прежнему `string`. Обычную `string` можно присвоить такому имени.
- **Интерфейсы:** `interface` тоже именует тип объекта, и TypeScript проверяет структуру, а не имя. Интерфейс можно открыть снова и слить. Псевдоним нельзя объявить дважды. Интерфейс описывает форму объекта. Псевдоним может ещё назвать примитив или объединение.
- **Расширение:** У интерфейса это `extends`. Псевдоним соединяет члены через `&`. В справочнике по умолчанию берут `interface`, пока не понадобится то, что умеет только `type`. `extends` часто дешевле для компилятора, чем большое пересечение.

**When to use:**

- Назвать объединение, кортеж, mapped type или условный тип.
- Назвать объект, если объявление одно и его потом не будут сливать. Для публичной формы объекта, которую другие файлы могут дополнить, лучше интерфейс.

**Examples:**

```ts
type ID = number | string;

type Point = {
  x: number;
  y: number;
};

function printCoord(pt: Point) {
  console.log(pt.x, pt.y);
}

printCoord({ x: 100, y: 100 });
```

**Sources:**

- [TypeScript handbook — Type Aliases](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)
- [TypeScript handbook — Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

</details>

<details>
<summary>Можете ли вы explain Union types and provide an example?</summary>

### Можете ли вы explain Union types and provide an example?

**Definition:**

Объединение `A | B` — это значение, которое может быть любым из типов-членов. Ведущий `|` допустим, поэтому члены можно писать с новой строки.

**How it works:**

- **Значение:** Подойдёт любой член. И `printId(101)`, и `printId("202")` входят в `number | string`. Объект — нет.
- **Использование:** TypeScript разрешает операцию, только если она верна для каждого члена. У `string | number` нет `toUpperCase`, потому что его нет у `number`. Общие члены, например `slice` у `string | string[]`, доступны без сужения.
- **Сужение:** Проверка `typeof`, `Array.isArray` или другой гард делит объединение. В ветке `else` остаются те члены, которые ещё возможны.
- **Свойства:** Объединение объектов показывает пересечение их свойств. Так устроена теория множеств: к целому объединению относятся только факты, верные для каждого члена.

**When to use:**

- Параметр или результат правда имеет больше одной формы, например идентификатор — число или строка.
- Один параметр-объединение лучше нескольких перегрузок, когда формы вызова совпадают по числу аргументов и типу возврата.

**Examples:**

```ts
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}

printId(101);
printId("202");
```

**Sources:**

- [TypeScript handbook — Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)

</details>

<details>
<summary>Как Intersection types work in TypeScript?</summary>

### Как Intersection types work in TypeScript?

**Definition:**

Пересечение `A & B` — это значение, которое является и `A`, и `B`. Для объектных типов это обычный способ собрать члены в один тип.

**How it works:**

- **Члены складываются:** У `Colorful & Circle` есть и `color`, и `radius`. Значение должно дать оба.
- **Конфликт:** Если у одного свойства несовместимые типы, тип свойства становится их пересечением. `string & number` у свойства — это `never`. Само объявление компилируется. Ошибка появляется, когда тип пытаются использовать.
- **Интерфейсы:** Два `interface` с одним именем сливаются, если свойства совместимы, и дают ошибку, если нет. Пересечение при конфликте сводит свойство к `never`, а не ругается на объявлении.
- **`extends`:** У интерфейса `extends` копирует члены и показывает, что типы связаны. Так расширяют объект. `&` нужен, когда соединяют типы, которые не оба интерфейсы, включая объединения и примитивы.

**When to use:**

- Добавить поля к существующему объектному типу без повторения: `type Bear = Animal & { honey: boolean }`.
- Смешать тип данных и тип поведения, например цветной круг. Не пересекайте свойства, которые не могут быть истинны одновременно.

**Examples:**

```ts
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

function draw(circle: ColorfulCircle) {
  console.log(circle.color, circle.radius);
}

draw({ color: "blue", radius: 42 });
```

**Sources:**

- [TypeScript handbook — Intersection Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)
- [TypeScript handbook — Interface Extension vs. Intersection](https://www.typescriptlang.org/docs/handbook/2/objects.html#interface-extension-vs-intersection)

</details>

<details>
<summary>Что такое Tuple types and when should they be used?</summary>

### Что такое Tuple types and when should they be used?

**Definition:**

Кортеж — это тип массива с фиксированной длиной и известным типом на каждом индексе. `[string, number]` значит, что индекс `0` — `string`, а индекс `1` — `number`. Отдельного представления в рантайме у него нет.

**How it works:**

- **Длина:** Чтение за последним индексом — ошибка. Деструктуризация работает как у обычного массива.
- **Необязательные элементы:** `?` допустим только в конце: `[number, number, number?]`. Тогда `length` имеет тип `2 | 3`.
- **Rest-элементы:** `...boolean[]` может стоять в начале, в середине или в конце. У такого кортежа нет одной `length`. Та же форма типизирует rest-параметр: `[string, number, ...boolean[]]` соответствует `(name: string, count: number, ...flags: boolean[])`.
- **`readonly`:** `readonly [string, number]` запрещает запись. `as const` у литерала массива выводит readonly-кортеж. Readonly-кортеж нельзя присвоить изменяемому кортежу.

**When to use:**

- Небольшая упорядоченная пара с фиксированными позициями, например координаты, или список rest-параметров.
- Если вызывающему коду приходится помнить, что значит индекс `0`, лучше объект с именами полей. Подписи вроде `[start: number, end: number]` нужны документации и инструментам. На совместимость типов они не влияют.

**Examples:**

```ts
type StringNumberPair = [string, number];

function logPair(pair: StringNumberPair) {
  const [text, count] = pair;
  console.log(text, count);
}

logPair(["hello", 42]);
```

**Sources:**

- [TypeScript handbook — Tuple Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types)
- [TypeScript 4.0 — Labeled Tuple Elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-0.html#labeled-tuple-elements)

</details>

<details>
<summary>Объясните Type Assertions and why they are useful.</summary>

### Объясните Type Assertions and why they are useful.

**Definition:**

Утверждение типа просит проверщик считать значение более узким или более широким типом. На этапе компиляции оно стирается и в рантайме значение не проверяет и не преобразует.

**How it works:**

- **Синтаксис:** `document.getElementById("main_canvas") as HTMLCanvasElement`. Угловые скобки `<HTMLCanvasElement>document.getElementById("main_canvas")` значат то же самое, кроме файлов `.tsx`, где они сталкиваются с JSX.
- **Пересечение:** Исходный и целевой типы должны пересекаться. `"hello" as number` — ошибка. Если это правда нужно, сначала утвердите через `unknown` (или `any`): `value as unknown as number`.
- **В рантайме ничего не происходит:** Неверное утверждение не бросает исключение и не даёт `null`. Следующее обращение к свойству может упасть позже.
- **Non-null assertion:** Постфикс `!` — отдельный оператор. Он убирает из типа `null` и `undefined` и тоже стирается.

**When to use:**

- Вы знаете, что узел DOM, разобранные данные или значение после миграции уже уже объявленного типа, а гард написать непрактично.
- Сужение (`typeof`, `in`, предикат) лучше, когда проверка может жить в коде. Утверждение — это заявление, которое компилятор не проверяет.

**Examples:**

```ts
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;

const context = myCanvas.getContext("2d");
```

**Sources:**

- [TypeScript handbook — Type Assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)
- [TypeScript handbook — Non-null Assertion Operator](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-)

</details>

<details>
<summary>Как you use the 'typeof' type guard in TypeScript?</summary>

### Как you use the 'typeof' type guard in TypeScript?

**Definition:**

`typeof` в выражении — это оператор JavaScript. Внутри `if (typeof value === "string")` и похожих проверок TypeScript сужает объединение до подходящего члена.

**How it works:**

- **Проверка строки:** Для `string | number` истинная ветка — `string`, ветка `else` — `number`. Методы вроде `toUpperCase` разрешены только в строковой ветке.
- **Что он различает:** Для сужения сравнивают с `"string"`, `"number"`, `"bigint"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"` и `"function"`.
- **Объекты и массивы:** `typeof null` равен `"object"`, и `typeof` массива тоже. Массивы отделяют через `Array.isArray`. После этой проверки в `else` остаётся другой член объединения.
- **Предикаты:** Функция с возвратом `x is number` — другой гард. TypeScript 5.5 умеет вывести такой предикат из простой проверки `typeof` или `!= null`. `typeof` в позиции типа (`typeof someValue`) — это оператор типа, а не этот гард.

**When to use:**

- Сузить объединение примитивов перед вызовом метода, который есть только у одного члена.
- `Array.isArray`, `in` или `instanceof` нужны, когда `typeof` для нескольких членов даст `"object"`.

**Examples:**

```ts
function formatCommandline(input: string | string[]) {
  if (typeof input === "string") {
    return input.trim();
  }
  return input.join(" ");
}
```

**Sources:**

- [TypeScript handbook — Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types)
- [MDN — typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<details>
<summary>Можете ли вы create types from existing data using Type Inference?</summary>

### Можете ли вы create types from existing data using Type Inference?

**Definition:**

Да. `typeof` в позиции типа копирует тип переменной или свойства. Форму не нужно переписывать вручную. Это оператор типа, отдельно от проверки `typeof` у значения.

**How it works:**

- **Только идентификаторы:** `typeof` в типе законен у имени или его свойств, например `typeof user` или `typeof user.name`. Произвольный вызов или выражение отклоняются, чтобы позиция типа не выглядела как выполняемый код.
- **Значение — не тип:** `ReturnType<getUser>` — ошибка, потому что `getUser` — значение. Подсказка в ошибке — `ReturnType<typeof getUser>`.
- **Вывод всё ещё расширяет:** `let name = "Ada"` имеет тип `string`. `const name = "Ada"` сохраняет литерал `"Ada"`. `as const` у массива или объекта сохраняет литералы, помечает свойства как readonly и превращает массив в readonly-кортеж. `typeof` затем копирует этот более узкий тип.
- **Тип возврата:** `ReturnType<typeof getUser>` берёт тип функции и даёт то, что функция возвращает.

**When to use:**

- Объект конфигурации, функция или экземпляр класса уже есть, и нужен тип, который за ними следит.
- Ручной псевдоним оставляют, когда публичный тип должен быть шире сегодняшнего значения или когда значение не в области видимости.

**Examples:**

```ts
function getUser() {
  return { id: 1, name: "Ada" };
}

type User = ReturnType<typeof getUser>;

const point = [3, 4] as const;
type Point = typeof point;
```

**Sources:**

- [TypeScript handbook — Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)
- [TypeScript 3.4 — const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

</details>

### TypeScript Generics

<details>
<summary>Что такое Generics in TypeScript and why are they used?</summary>

### Что такое Generics in TypeScript and why are they used?

**Definition:**

Дженерик — это параметр типа: заполнитель для типа, который подставляет вызывающий код. Компонент работает со многими типами и помнит, какой тип вошёл. `any` этого не помнит.

**How it works:**

- **Переменная типа:** `function identity<Type>(arg: Type): Type` захватывает тип аргумента и использует его как тип возврата. Информация входит с одной стороны и выходит с другой.
- **Вызов:** Аргумент можно передать явно, `identity<string>("myString")`, или дать выводу поставить `Type` по значению. Вывод — обычный случай. Явные аргументы нужны, когда тип вывести не из чего.
- **В теле:** `Type` обозначает любой возможный тип. `.length` читать нельзя, пока ограничение не скажет, что свойство есть.
- **Где бывают:** Параметры типа принимают функции, интерфейсы, классы и псевдонимы типов. Перечисления и пространства имён — нет. Дженерик-класс дженерик только на стороне экземпляра. Статические члены параметр типа класса не видят.

**When to use:**

- Одна функция или контейнер должны работать со строками, числами и будущими типами, без копии реализации и без возврата `any`.
- Параметр типа не нужен, если он встречается один раз и ничто не связывает две позиции. Там проще обычный `string | number`.

**Examples:**

```ts
function identity<Type>(arg: Type): Type {
  return arg;
}

const text = identity("myString");
const explicit = identity<string>("myString");
```

**Sources:**

- [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

<details>
<summary>Как you create a generic function?</summary>

### Как you create a generic function?

**Definition:**

Параметры типа пишут в угловых скобках после имени функции, а затем используют эти имена в параметрах, в типе возврата или в обоих местах.

**How it works:**

- **Объявление:** `function identity<Type>(arg: Type): Type`. Список параметров типа идёт первым, затем параметры значения.
- **Массив параметра:** `arg: Type[]` или `arg: Array<Type>` значит, что функция принимает и возвращает массив, поэтому `.length` законен, а `Type` — тип элемента.
- **Типы функций:** У типа дженерик-функции параметры стоят впереди: `type Identity = <Type>(arg: Type) => Type`. Имена могут отличаться, если совпадают число и позиции.
- **Вывод:** Вызов без `<...>` позволяет проверщику взять аргументы типа из значений. Их пишут вручную, когда аргумента, из которого можно вывести тип, нет.

**When to use:**

- Связать вход и выход, например «возвращает тот же тип, что получила» или «возвращает свойство с этим ключом».
- Объединение или перегрузка лучше, когда набор входов — фиксированный список, а не открытый параметр типа.

**Examples:**

```ts
function first<Type>(items: Type[]): Type | undefined {
  return items[0];
}

const name = first(["Ada", "Grace"]);
```

**Sources:**

- [TypeScript handbook — Hello World of Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html#hello-world-of-generics)
- [TypeScript handbook — Generic Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#generic-functions)

</details>

<details>
<summary>Обсудите how Generic interfaces can be defined.</summary>

### Обсудите how Generic interfaces can be defined.

**Definition:**

Дженерик-интерфейс объявляет один или несколько параметров типа на самом интерфейсе или на одной сигнатуре вызова внутри него. Вызывающий код подставляет эти параметры, когда использует интерфейс.

**How it works:**

- **На интерфейсе:** `interface Box<Type> { contents: Type }` заставляет все члены делить один `Type`. При использовании нужен аргумент: `Box<string>`. Для этого значения параметр зафиксирован.
- **На сигнатуре вызова:** `interface IdentityFn { <Type>(arg: Type): Type }` описывает дженерик-функцию. Каждый вызов может выбрать новый `Type`. Сам интерфейс при этом не дженерик.
- **Что выбрать:** Параметр на интерфейсе, когда весь объект должен сойтись на одном типе, как `Box<string>`. Параметр на сигнатуре, когда каждый вызов независим.
- **Псевдонимы объектов:** `type Box<Type> = { contents: Type }` — та же идея через псевдоним. Интерфейс может ещё `extend` другой дженерик-интерфейс.

**When to use:**

- Контейнер, ответ или словарь, у которых тип содержимого меняется: `Box<User>`, `Page<Item>`.
- Интерфейс в форме функции, когда типизируют саму функцию, а не коробку одного типа.

**Examples:**

```ts
interface Box<Type> {
  contents: Type;
}

const box: Box<string> = { contents: "hello" };

interface IdentityFn {
  <Type>(arg: Type): Type;
}
```

**Sources:**

- [TypeScript handbook — Generic Types](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-types)
- [TypeScript handbook — Generic Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#generic-object-types)

</details>

<details>
<summary>Объясните how Generics work with Classes.</summary>

### Объясните how Generics work with Classes.

**Definition:**

Дженерик-класс ставит список параметров типа после имени класса. Поля и методы экземпляра делят этот параметр. Статические члены его не видят.

**How it works:**

- **Форма:** `class Box<Type> { contents: Type; }` — классовый вариант дженерик-интерфейса. `new Box<string>()` или вывод из аргумента конструктора задаёт `Type`.
- **Один параметр на экземпляр:** Поля и методы `this` используют один и тот же `Type`. Поэтому `GenericNumber<number>` не может складывать строки тем же методом `add`.
- **Статическая сторона:** У типа класса есть статическая сторона и сторона экземпляра. Параметр типа принадлежит стороне экземпляра. Статическому методу нужен собственный параметр типа, если он ему нужен.
- **Конструкторы:** Фабрика, которая создаёт экземпляры, типизирует класс как конструктор, например `{ new (): Type }`. В справочнике эта форма нужна для примесей. Ограничение по-прежнему пишется через `extends`, как у функции.

**When to use:**

- Класс хранит или производит значение, тип которого должен следовать за экземпляром: коробка, репозиторий, типизированная цель события.
- Если у класса нет типа, который надо вести на экземпляр, хватает обычного класса.

**Examples:**

```ts
class Box<Type> {
  constructor(public contents: Type) {}

  get(): Type {
    return this.contents;
  }
}

const box = new Box("hello");
const value: string = box.get();
```

**Sources:**

- [TypeScript handbook — Generic Classes](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-classes)

</details>

<details>
<summary>Можете ли вы provide an example of a generic constraint?</summary>

### Можете ли вы provide an example of a generic constraint?

**Definition:**

Ограничение сужает параметр типа через `extends`. Функция может пользоваться только тем, что гарантирует ограничение, а вызывающий код может передать только типы с этими членами.

**How it works:**

- **Ограничение членом:** `Type extends Lengthwise` значит, что у `Type` есть `length: number`. Тогда `arg.length` законен. `loggingIdentity(3)` падает, потому что у `number` нет `length`. Строка или `{ length: 10, value: 3 }` принимаются, и тип возврата остаётся конкретным типом аргумента.
- **Другой параметр типа:** `Key extends keyof Type` значит, что `Key` — имя свойства `Type`. Тогда `obj[key]` имеет тип `Type[Key]`, а отсутствующее имя — ошибка.
- **Без ограничения:** Тело должно считать `Type` любым возможным типом, поэтому `.length` — ошибка, даже если сегодняшний вызов передаёт массив.

**When to use:**

- Телу нужно свойство, метод или ключ, и при этом конкретный тип аргумента надо сохранить.
- Если у всех вызовов один и тот же конкретный тип, параметр типа убирают и пишут этот тип прямо.

**Examples:**

```ts
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length);
  return arg;
}

loggingIdentity("hello");
loggingIdentity({ length: 10, value: 3 });
```

**Sources:**

- [TypeScript handbook — Generic Constraints](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-constraints)
- [TypeScript handbook — Using Type Parameters in Generic Constraints](https://www.typescriptlang.org/docs/handbook/2/generics.html#using-type-parameters-in-generic-constraints)

</details>

### TypeScript Advanced Types and Techniques

<details>
<summary>Что такое Discriminated Union in TypeScript?</summary>

### Что такое Discriminated Union in TypeScript?

**Definition:**

Размеченное объединение — это объединение объектных типов с общим свойством, дискриминантом. В каждом члене у него свой литерал. Проверка этого свойства сужает весь объект.

**How it works:**

- **Форма:** У каждого члена один и тот же ключ, обычно `kind` или `type`, с литеральным типом строки или числа. Остальные поля различаются.
- **Сужение:** `switch (shape.kind)` или `if (shape.kind === "square")` делает `shape` этим членом внутри ветки. Дополнительные поля тогда доступны без утверждения.
- **Остаток:** После того как один или несколько членов отброшены, ветка `else` — это оставшееся объединение. Если обработаны все члены, ничего не остаётся.
- **Зачем литерал:** Общий `kind: string` не размечает объединение. Проверщику нужен литерал, с которым можно сравнить.

**When to use:**

- Результат, действие или фигура с несколькими взаимоисключающими вариантами, например квадрат, прямоугольник и круг.
- Так лучше, чем набор необязательных полей, которые имеют смысл только вместе. Дискриминант не даёт незаконным сочетаниям попасть в тип.

**Examples:**

```ts
interface Square {
  kind: "square";
  size: number;
}

interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Circle;

function area(shape: Shape): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "circle":
      return Math.PI * shape.radius * shape.radius;
  }
}
```

**Sources:**

- [TypeScript 2.0 — Discriminated Unions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#discriminated-unions)

</details>

<details>
<summary>Как you declare a Readonly type?</summary>

### Как you declare a Readonly type?

**Definition:**

`readonly` у свойства говорит проверщику, что это свойство нельзя переприсвоить. Модификатор стирается и не замораживает значение в рантайме. `Readonly<Type>` ставит этот модификатор на каждое свойство.

**How it works:**

- **Одно свойство:** `readonly prop: string` разрешает чтение и отклоняет `obj.prop = "hello"`.
- **Вложенные значения:** `readonly` запрещает переписать само свойство. Оно не запрещает менять объект, на который свойство указывает. `home.resident.age = 43` всё ещё можно, если `readonly` стоит только у `resident`.
- **Псевдоним:** TypeScript не учитывает `readonly` при совместимости типов так, как часто ждут. Изменяемый псевдоним того же объекта может записать свойство.
- **Утилита:** `Readonly<Type>` — это mapped-форма `{ readonly [P in keyof Type]: Type[P] }`. `Object.freeze` типизирован как возврат `Readonly<Type>`. Для массивов есть `readonly Type[]` и `ReadonlyArray<Type>`.

**When to use:**

- Показать, что функция только читает конфиг или кортеж.
- `readonly` на массиве или кортеже, если вызывающий код не должен делать push или заменять элементы. Изменяемый псевдоним это всё равно не остановит, пока значение не заморожено в JavaScript.

**Examples:**

```ts
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = { title: "Delete inactive users" };

function visit(home: { readonly resident: { name: string; age: number } }) {
  home.resident.age += 1;
}
```

**Sources:**

- [TypeScript handbook — readonly Properties](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-properties)
- [TypeScript handbook — Readonly](https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlytype)

</details>

<details>
<summary>Можете ли вы describe the use of Mapped types?</summary>

### Можете ли вы describe the use of Mapped types?

**Definition:**

Mapped type строит новый объектный тип, обходя объединение ключей, обычно `keyof Type`, и записывая свойство для каждого ключа.

**How it works:**

- **Форма:** `{ [Property in keyof Type]: boolean }` копирует ключи `Type` и ставит каждому значение `boolean`.
- **Модификаторы:** `readonly` и `?` можно добавить или снять. Префикс `+` добавляет и стоит по умолчанию. Префикс `-` снимает: `-readonly` или `-?`. `Required<T>` — это mapped type, который снимает `?`.
- **Переименование ключей:** С TypeScript 4.1 `as` меняет ключи: `` [Property in keyof Type as `get${Capitalize<string & Property>}`] ``. Условный тип, который даёт `never`, выбрасывает этот ключ.
- **Другие объединения:** Слева от `in` может быть объединение не только из `string | number | symbol`, например объединение событий, ключом которых служит `kind`.

**When to use:**

- Вывести объект флагов, частичное обновление, readonly-вид или геттеры из типа, который уже есть.
- Не копируйте большой интерфейс руками, если связь — «те же ключи, другие значения».

**Examples:**

```ts
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

interface Features {
  darkMode: () => void;
  newUserProfile: () => void;
}

type FeatureOptions = OptionsFlags<Features>;
```

**Sources:**

- [TypeScript handbook — Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

</details>

<details>
<summary>Объясните Conditional types in TypeScript.</summary>

### Объясните Conditional types in TypeScript.

**Definition:**

Условный тип выбирает один из двух типов: `SomeType extends OtherType ? TrueType : FalseType`. Истинная ветка выбирается, когда левый тип можно присвоить правому.

**How it works:**

- **С дженериками:** Фиксированная проверка вроде `Dog extends Animal` — это просто `number` или `string`. Полезный случай — параметр типа, чтобы одна сигнатура функции заменила стопку перегрузок.
- **Сужение параметра:** В истинной ветке `T extends { message: unknown }` значит, что `T["message"]` разрешён. Ложная ветка может быть `never`.
- **`infer`:** `T extends (infer Item)[] ? Item : T` объявляет переменную типа для совпавшего куска. Так устроен `ReturnType`. У перегруженной функции вывод берёт последнюю сигнатуру.
- **Дистрибутивность:** Голый параметр типа распределяется по объединению. `ToArray<string | number>` становится `string[] | number[]`. Если обе стороны обернуть в кортеж из одного элемента, `[T] extends [any]`, объединение проверяется целиком и получается `(string | number)[]`.

**When to use:**

- Выходной тип зависит от входного: развернуть массив, выбрать тип подписи из `string | number`, извлечь тип возврата.
- Обычное объединение лучше, когда обе ветки одной формы и `infer` не нужен.

**Examples:**

```ts
type Flatten<T> = T extends any[] ? T[number] : T;

type Str = Flatten<string[]>;
type Num = Flatten<number>;
```

**Sources:**

- [TypeScript handbook — Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

</details>

<details>
<summary>Что такое Index types and how do you use the keyof keyword?</summary>

### Что такое Index types and how do you use the keyof keyword?

**Definition:**

`keyof Type` — это объединение имён свойств этого типа. Индексный доступ `Type[Key]` — тип свойства с именем `Key`. Вместе они типизируют «дай поле этого объекта по имени».

**How it works:**

- **`keyof`:** Для `{ foo: number; bar: string }` результат `keyof` — `"foo" | "bar"`. В справочнике это называется index type query.
- **Доступ:** `Type[Key]` читает одно свойство. Если `Key` — объединение, результат — объединение типов этих свойств.
- **Ограничение:** `function getProperty<T, K extends keyof T>(obj: T, key: K)` возвращает `T[K]`. Ключ, которого нет у `T`, — ошибка, а записываемое значение должно иметь тип `T[K]`.
- **Пересечения:** `keyof (A & B)` упрощается до `keyof A | keyof B`.

**When to use:**

- Помощник, который читает или пишет свойство по аргументу-ключу, или mapped type, который обходит каждый ключ.
- Сигнатура индекса (`[key: string]: Value`) — другой инструмент: она описывает неизвестные имена. `keyof` описывает имена, которые уже объявлены.

**Examples:**

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "Ada", age: 36 };
const name: string = getProperty(user, "name");
```

**Sources:**

- [TypeScript 2.1 — keyof and Lookup Types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#keyof-and-lookup-types)
- [TypeScript handbook — Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

</details>

<details>
<summary>Как Type Casting differ from Type Assertions?</summary>

### Как Type Casting differ from Type Assertions?

**Definition:**

`as` в TypeScript (и форма с угловыми скобками) — это утверждение типа: заявление на этапе компиляции, которое стирается. Значение оно не преобразует. Приведение в языках, где значение меняется в рантайме, — другая операция, и такого оператора в TypeScript нет.

**How it works:**

- **Утверждение:** `value as HTMLCanvasElement` меняет только тип, которым пользуется проверщик. Если заявление неверно, нет ни исключения, ни `null`. Типы должны пересекаться, иначе сначала утверждают через `unknown`.
- **Преобразование:** Строку в число превращает обычный JavaScript, например `Number(text)`. Тип следует из того, что возвращает эта функция. Утверждение этого шага не делает.
- **Проверенное сужение:** Предикат `value is string` или сигнатура `asserts value is string` выполняет настоящую проверку и затем сужает тип. Это ближайший инструмент к проверенному приведению, и это не оператор `as`.
- **`.tsx`:** Угловые скобки сталкиваются с JSX, поэтому в таких файлах пишут `as`.

**When to use:**

- `as`, когда проверщик не видит факт, который уже установлен вне системы типов.
- Предикат или функция `asserts`, когда программа должна реально проверить значение. `Number`, `String` или `Boolean` — когда нужно преобразованное значение.

**Examples:**

```ts
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new Error("Not a string");
  }
}

function upper(value: unknown): string {
  assertIsString(value);
  return value.toUpperCase();
}
```

**Sources:**

- [TypeScript handbook — Type Assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)
- [TypeScript 3.7 — Assertion Functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#assertion-functions)

</details>

<details>
<summary>Обсудите the Partial, Required, Readonly, and Pick utility types.</summary>

### Обсудите the Partial, Required, Readonly, and Pick utility types.

**Definition:**

Эти четыре глобальных типа — mapped types. `Partial` делает каждое свойство необязательным, `Required` делает каждое обязательным, `Readonly` помечает каждое как `readonly`, а `Pick` оставляет только названные ключи.

**How it works:**

- **`Partial<Type>`:** Каждое свойство получает `?`. Значение может содержать любое подмножество. Появился в 2.1. Определение: `{ [P in keyof Type]?: Type[P] }`.
- **`Required<Type>`:** Противоположность, с 2.8. Снимает `?` с каждого свойства, поэтому отсутствующее свойство — ошибка.
- **`Readonly<Type>`:** Каждое свойство `readonly`. Присваивание отклоняет проверщик. `Object.freeze` типизирован как возврат этого типа. Вложенные объекты он всё равно не замораживает глубоко.
- **`Pick<Type, Keys>`:** `Keys` — строковый литерал или их объединение. В результате только эти свойства. `Omit<Type, Keys>` — противоположность и появился в 3.5.

**When to use:**

- `Partial` для патча или формы обновления. `Required`, когда у черновика поля были необязательными, а готовому значению нужны все.
- `Readonly` для аргумента функции, который нельзя переприсвоить. `Pick` для превью или строки списка, где нужны только `title` и `completed`.

**Examples:**

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;
type TodoPatch = Partial<Todo>;

const preview: TodoPreview = { title: "Clean room", completed: false };
const patch: TodoPatch = { completed: true };
```

**Sources:**

- [TypeScript handbook — Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

</details>

<details>
<summary>Что такое the 'never' type and when would it be used?</summary>

### Что такое the 'never' type and when would it be used?

**Definition:**

`never` — тип значения, которое не возникает. Тип возврата `never` значит, что функция бросает исключение или прекращает выполнение. `never` — это ещё и то, что остаётся от объединения, когда каждый член отброшен.

**How it works:**

- **Нет возврата:** Функция с возвращаемым типом `never` не должна завершаться нормально. В справочнике это «бросить исключение или прекратить выполнение».
- **Полнота:** В `string | number` ветка после `typeof x === "string"` и `typeof x === "number"` типизирует `x` как `never`. Присваивание этого остатка параметру `never` помечает член объединения, который забыли.
- **Условные типы:** Ложная ветка условного типа часто `never`: «этот случай не производит тип». Дистрибутивный условный тип использует `never` как пустой член объединения.
- **Совместимость:** `never` можно присвоить любому типу. Почти ничто нельзя присвоить `never`, поэтому это удобный сток для невозможных состояний.

**When to use:**

- Пометить функцию, которая всегда бросает исключение.
- Проверить, что `switch` по дискриминанту обработал каждый член.
- Убрать ключи из mapped type, переназначив их в `never`.

**Examples:**

```ts
function assertNever(value: never): never {
  throw new Error("Unexpected value");
}

function area(shape: { kind: "square"; size: number } | { kind: "circle"; radius: number }): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "circle":
      return Math.PI * shape.radius * shape.radius;
    default:
      return assertNever(shape);
  }
}
```

**Sources:**

- [TypeScript handbook — never](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)

</details>

### TypeScript Modules and Namespaces

<details>
<summary>Как you orGAnize code with Modules in TypeScript?</summary>

### Как you orGAnize code with Modules in TypeScript?

**Definition:**

Модуль — это файл с `import`, `export` или top-level `await` на верхнем уровне. Его объявления остаются в области этого файла, пока их не экспортируют. Файл без этого — скрипт, и его имена попадают в глобальную область.

**How it works:**

- **ES-модули:** `import` / `export` — синтаксис, который справочник считает текущим форматом. CommonJS (`module.exports` и `require`) — второй формат, который справочник разбирает, потому что так до сих пор поставляется большинство пакетов npm.
- **Сделать файл модулем:** Если в файле нет import или export, но он не должен быть скриптом, добавьте `export {}`. Это работает при любой цели `module`.
- **Три выбора:** Какой синтаксис писать, как имя модуля сопоставляется с файлом (резолюция) и какой JavaScript выдаёт компилятор (`module`, отдельно от `target`).
- **Пространство имён — не это:** Namespace — более старая группировка TypeScript. Для нового кода справочник советует ES-модули, чтобы организация совпадала с JavaScript.

**When to use:**

- Любая программа, которая должна загружать файлы как зависимости, а не делить одну глобальную область.
- Приложения Node.js, библиотеки и браузерный код со сборщиком. Namespace — только для случаев из следующего вопроса.

**Examples:**

```ts
// hello.ts
export default function helloWorld() {
  console.log("Hello, world!");
}

// app.ts
import helloWorld from "./hello.js";

helloWorld();
```

**Sources:**

- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html)

</details>

<details>
<summary>Когда would you use Namespaces?</summary>

### Когда would you use Namespaces?

**Definition:**

Namespace — это имя TypeScript для объекта JavaScript в глобальной области. Им можно сгруппировать объявления, в том числе в нескольких файлах, которые склеивает `outFile`. Это не устаревшая возможность. Для нового кода справочник всё равно рекомендует ES-модули.

**How it works:**

- **Глобальный объект:** В отличие от модуля, namespace не нужен загрузчик. Несколько файлов могут дополнять один namespace, а `outFile` может их склеить. Страница тогда подключает эти скрипты тегами `<script>`.
- **Зависимости скрыты:** Всё делит глобальный объект, поэтому в большом приложении трудно увидеть, какой файл от какого зависит.
- **Не оборачивайте модуль:** `export namespace Shapes { export class Triangle {} }` заставляет писать `shapes.Shapes.Triangle`. Файл модуля уже и есть группа, а имя выбирает импортёр. Экспортируйте `Triangle` напрямую.
- **Где они остаются:** Сложные файлы объявлений, включая DefinitelyTyped, по-прежнему используют namespace. Это инструмент файлов типов, а не способ устроить приложение.

**When to use:**

- Глобальный набор скриптов, который грузят тегами script, или `.d.ts`, где паттерн namespace уже есть.
- Новый код приложения и библиотеки — это файлы с `import` и `export`.

**Examples:**

```ts
export class Triangle {
  /* ... */
}

export class Square {
  /* ... */
}
```

**Sources:**

- [TypeScript handbook — Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)
- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescript-namespaces)

</details>

<details>
<summary>Что такое the difference between Internal and External modules?</summary>

### Что такое the difference between Internal and External modules?

**Definition:**

«Internal module» — старое имя namespace. «External module» — старое имя файлового модуля, того, что пользуется `import` и `export`. В текущих документах эти новые имена.

**How it works:**

- **Internal / namespace:** Именованный объект в глобальной области. Он может занимать несколько файлов и склеиваться через `outFile`. Сам по себе он не объявляет зависимость, которую должен забрать загрузчик.
- **External / module:** Один файл, своя область. Другие файлы видят имя только после export и import. Рантайму нужен загрузчик: CommonJS, AMD или среда, которая выполняет ES-модули.
- **Ссылки:** `/// <reference path="..." />` указывает компилятору на файл объявлений. Это не замена `import`, когда вы потребляете модуль.
- **Выдача:** Модули соответствуют один исходный файл — один выходной. `outFile` не склеивает вывод `commonjs` или `umd`. С TypeScript 1.8 он может склеивать `amd` и `system`.

**When to use:**

- В старых текстах читайте «internal module» как «namespace», а «external module» как «module».
- Новый код пишите файловыми модулями. Namespace оставляйте, когда поддерживаете глобальный скрипт или файл объявлений, где он уже есть.

**Examples:**

```ts
// Файл с export на верхнем уровне — модуль (раньше external module).
export function area(radius: number): number {
  return Math.PI * radius * radius;
}
```

**Sources:**

- [TypeScript handbook — Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)

</details>

<details>
<summary>Как you export and import modules?</summary>

### Как you export and import modules?

**Definition:**

`export` публикует значение или тип из модуля. `import` связывает опубликованное имя в другом файле. Типы можно писать тем же синтаксисом или модификатором `type`, когда импорт должен стереться.

**How it works:**

- **Значения:** `export default function helloWorld() {}` — главный экспорт, его импортируют как `import helloWorld from "./hello.js"`. Остальные экспорты без `default` импортируют по имени: `import { pi } from "./maths.js"`.
- **Формы:** `import { pi as π }` переименовывает. `import * as maths` берёт объект модуля. `import "./maths.js"` выполняет файл ради побочных эффектов и ничего не связывает.
- **Типы:** `import type { Cat } from "./animal.js"` можно использовать только как тип. С 4.5 `import { createCat, type Cat }` помечает отдельные имена. Babel, swc и esbuild могут их выбросить.
- **`verbatimModuleSyntax`:** Импорты и экспорты без `type` сохраняются. Импорты с `type` выбрасываются. Под этим флагом `import` не переписывается в `require`. Для CommonJS пишут `import x = require("x")` и `export =`.

**When to use:**

- Именованные экспорты для большинства функций и типов. Default-экспорт — для одной главной функции файла.
- `import type` или встроенный `type`, когда имя существует только в системе типов, особенно если JavaScript выдаёт другой инструмент.

**Examples:**

```ts
export const pi = 3.14;

export function twice(n: number): number {
  return n * 2;
}

import { pi, twice } from "./maths.js";

console.log(twice(pi));
```

**Sources:**

- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html#es-module-syntax)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#verbatimmodulesyntax)

</details>

<details>
<summary>Можете ли вы describe module resolution and its strategies?</summary>

### Можете ли вы describe module resolution and its strategies?

**Definition:**

Резолюция модулей превращает строку в `import` или `require` в файл. Опция `moduleResolution` выбирает правила поиска.

**How it works:**

- **`classic`:** Старый режим по умолчанию, когда `module` не `commonjs`. Его оставили для совместимости.
- **`node`:** Повторяет поиск CommonJS в Node.js и дополнительно проверяет `.ts` и `.d.ts`. Относительный импорт без расширения работает. Современный ESM Node он не моделирует.
- **`node16` и `nodenext`:** Добавлены в TypeScript 4.7. Они следуют поиску ECMAScript-модулей в Node. Относительный импорт в ES-модуле должен включать расширение, например `./utils.mjs`.
- **`bundler`:** Добавлен в TypeScript 5.0 для Vite, esbuild, swc, Webpack, Parcel и похожих инструментов. Импорт без расширения работает, а `"exports"` в `package.json` предпочитает условие `import`. Публикация библиотеки с `bundler` может спрятать проблемы для пользователей без сборщика. Таким пакетам лучше `node16` или `nodenext`.
- **Связанные флаги:** `baseUrl`, `paths` и `rootDirs` меняют поиск. При `node16`, `nodenext` и `bundler` флаги `resolvePackageJsonExports` и `resolvePackageJsonImports` по умолчанию `true`. `customConditions` добавляет условия `exports` и допустим только в этих трёх режимах.

**When to use:**

- Приложение, которое собирает бандлер: `moduleResolution` `"bundler"`.
- Пакет для npm или код Node, который должен совпасть с загрузчиком Node: `"nodenext"` или `"node16"`.

**Examples:**

```json
{
  "compilerOptions": {
    "target": "esnext",
    "moduleResolution": "bundler"
  }
}
```

**Sources:**

- [TypeScript handbook — Module Resolution](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-resolution-options)
- [TypeScript 5.0 — moduleResolution bundler](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#moduleresolution-bundler)

</details>

<details>
<summary>Обсудите the compatibility of TypeScript modules with ES6 modules.</summary>

### Обсудите the compatibility of TypeScript modules with ES6 modules.

**Definition:**

Синтаксис модулей TypeScript — это синтаксис ES-модулей (`import` / `export`). Опция компилятора `module` решает, оставить этот синтаксис как ES-модули или переписать его под старый загрузчик.

**How it works:**

- **Выдача:** Если `module` настроен так, что выход — ES2020 или другая цель ES-модулей, `import` и `export` остаются. `commonjs` переписывает их в `require` и `exports`. `umd` оборачивает фабрику, которая выбирает CommonJS, AMD или глобальную переменную.
- **`target` — отдельно:** `target` понижает возможности языка. `module` выбирает, как файлы общаются. Загрузчик затем находит и выполняет зависимости до тела модуля.
- **Совместимость:** У CommonJS нет настоящего разделения default-экспорта и объекта пространства модуля. `esModuleInterop` уменьшает это несовпадение для default-импортов.
- **Типы:** Импорт только типа — не рантайм-импорт. `import type` и `--verbatimModuleSyntax` делают это стирание явным, чтобы оставшийся JavaScript был корректным синтаксисом ES-модулей.

**When to use:**

- Выдавайте ES-модули, когда их грузит рантайм или сборщик: современные браузеры, текущий Node с `"type": "module"` или бандлер.
- Выдавайте CommonJS, когда среда всё ещё грузит `require`. Под `verbatimModuleSyntax` для такого выхода пишите `import x = require("x")`, а не ждите, что `import` перепишут.

**Examples:**

```ts
import { valueOfPi } from "./constants.js";

export const twoPi = valueOfPi * 2;
```

**Sources:**

- [TypeScript handbook — Module Output](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-output-options)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#verbatimmodulesyntax)

</details>

### TypeScript Decorators and Metadata

<details>
<summary>Что такое decorators and how do you use them?</summary>

### Что такое decorators and how do you use them?

**Definition:**

Декоратор — это функция, которую вешают через `@`, чтобы настроить класс или его член. С TypeScript 5.0 этот синтаксис допустим без `--experimentalDecorators`. Флаг по-прежнему включает старую модель декораторов.

**How it works:**

- **Вызов:** `@loggedMethod` вызывает `loggedMethod` с декорируемым значением и объектом контекста. Если функция возвращает замену, ставится она. Если ничего не возвращает, остаётся оригинал.
- **Где:** Методы, поля, геттеры, сеттеры, автоаксессоры (`accessor`) и сам класс. Новая модель не декорирует параметры и несовместима с `--emitDecoratorMetadata`.
- **Контекст:** Контекст сообщает `name`, является ли член `static` или `private`, и с версии 5.2 общий объект `metadata`. `addInitializer` регистрирует колбэк, который выполняется при настройке класса или члена.
- **Место:** Декоратор может стоять до `export` или после `export` / `export default`. Оба сразу на одном классе — ошибка.
- **Старый флаг:** `--experimentalDecorators` проверяет и выдаёт старую форму (`target`, ключ свойства, дескриптор). Функция, написанная под этот флаг, на новой модели обычно не работает как есть.

**When to use:**

- Повторно использовать логирование, привязку, регистрацию или метаданные вокруг классов, не копируя этот код в каждый метод.
- Включайте флаг только в кодовой базе, которая уже зависит от экспериментальных декораторов, например от старых метаданных Angular или Nest.

**Examples:**

```ts
function loggedMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = String(context.name);
  function replacementMethod(this: any, ...args: any[]) {
    console.log(`Entering ${methodName}`);
    return originalMethod.call(this, ...args);
  }
  return replacementMethod;
}

class Person {
  constructor(public name: string) {}

  @loggedMethod
  greet() {
    return `Hello, ${this.name}`;
  }
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)
- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)

</details>

<details>
<summary>Можете ли вы define Class decorators and how they can alter Class behavior?</summary>

### Можете ли вы define Class decorators and how they can alter Class behavior?

**Definition:**

Декоратор класса — это функция прямо над классом. Она получает конструктор и `ClassDecoratorContext` и может зарегистрировать работу, которая выполнится после того, как класс окончательно определён.

**How it works:**

- **Контекст:** `context.kind` равен `"class"`. `context.name` — имя класса или `undefined` у безымянного класса. `context.metadata` — тот же объект, который видят декораторы членов этого класса.
- **`addInitializer`:** Колбэк выполняется после того, как определение класса завершено. Внутри него `this` — конструктор. В типах библиотеки этот хук регистрирует класс, например через `customElements.define`.
- **Что говорят заметки:** TypeScript 5.0 относит декораторы классов к наследованию и к регистрации. API контекста, который описан в типах, — это `addInitializer` и `metadata`.
- **Export:** `@register export default class Foo` и `export default @register class Bar` оба допустимы. `@before export @after class Bar` — нет.

**When to use:**

- Зарегистрировать класс в карте, пометить конструктор или повесить метаданные на все экземпляры этого класса.
- Не ставьте декоратор класса, если обычный подкласс или фабрика понятнее.

**Examples:**

```ts
function registered(_value: new (...args: any[]) => object, context: ClassDecoratorContext) {
  context.addInitializer(function () {
    console.log(context.kind, context.name);
  });
}

@registered
class User {
  constructor(public name: string) {}
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Как you use Method decorators in TypeScript?</summary>

### Как you use Method decorators in TypeScript?

**Definition:**

Декоратор метода получает исходный метод и `ClassMethodDecoratorContext`. Верните новую функцию, чтобы заменить метод. Ничего не возвращайте, чтобы метод остался на месте, а контекст использовался для другой работы.

**How it works:**

- **Замена:** Обычная обёртка вызывает `originalMethod.call(this, ...args)`, чтобы сохранить `this` и аргументы, и возвращает исходный результат.
- **Контекст:** `context.name` — имя метода. `context.private` и `context.static` говорят, как он объявлен. `ClassMethodDecoratorContext<This, Value>` может типизировать `this`, аргументы и тип возврата вместо `any`.
- **`addInitializer`:** Для метода экземпляра это выполняется в начале конструктора, до инициализации остальных полей. Для статического метода — когда инициализируется сам класс. Пример `bound` в заметках использует это, чтобы сделать `bind`.
- **Приватные имена:** `bound` в заметках TypeScript 5.0 бросает ошибку, если `context.private` установлен, потому что последующее присваивание по имени не подходит к методу `#private`.

**When to use:**

- Логировать, замерять или привязывать метод, не правя каждое место вызова.
- Обычная функция-обёртка лучше, когда метод не принадлежит классу.

**Examples:**

```ts
function loggedMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = String(context.name);
  function replacementMethod(this: any, ...args: any[]) {
    console.log(`Entering ${methodName}`);
    return originalMethod.call(this, ...args);
  }
  return replacementMethod;
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Что такое Accessor decorators?</summary>

### Что такое Accessor decorators?

**Definition:**

Декоратор аксессора стоит на геттере, сеттере или автоаксессоре (`accessor name = value`). У каждого вида свой контекст: `ClassGetterDecoratorContext`, `ClassSetterDecoratorContext` или `ClassAccessorDecoratorContext`.

**How it works:**

- **Геттер и сеттер:** В текущей модели декораторов это отдельные члены. `kind` контекста — `"getter"` или `"setter"`. `context.access` может читать или писать этот аксессор. `context.static` и `context.private` совпадают с объявлением.
- **Автоаксессор:** Ключевое слово `accessor` объявляет поле с геттером и сеттером. `kind` его контекста — `"accessor"`. Декоратор может вернуть результат с заменой `get`, `set` или `init`. `init` выполняется, когда вычисляется значение инициализатора.
- **Метаданные:** Тот же декоратор может записать `context.metadata[context.name]`. Так заметки TypeScript 5.2 помечают `accessor` для последующей сериализации.
- **Старое отличие:** Экспериментальные декораторы применяли один дескриптор свойства к паре get/set и требовали декораторы на первом аксессоре. Это правило относится к `--experimentalDecorators`, а не к этому объекту контекста.

**When to use:**

- Наблюдать или обернуть геттер или сеттер, или пометить автоаксессор как сериализуемый.
- Публичного поля достаточно, когда оборачивать тело аксессора нечего.

**Examples:**

```ts
function mark(_target: any, context: ClassAccessorDecoratorContext): void {
  context.metadata[context.name] = true;
}

class Person {
  @mark
  accessor age = 36;
}
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Объясните how to use Property decorators.</summary>

### Объясните how to use Property decorators.

**Definition:**

Декоратор свойства в текущем TypeScript — это декоратор поля. Это функция над полем класса. Тип контекста — `ClassFieldDecoratorContext`, а `context.kind` равен `"field"`.

**How it works:**

- **Аргументы:** Функция получает декорируемую цель и контекст. Полезные данные на контексте: `name`, `static`, `private`, `access` и `metadata`.
- **`access`:** `context.access.has`, `get` и `set` читают или пишут это поле у переданного объекта.
- **`addInitializer`:** Колбэк выполняется сразу после инициализации поля, статическое оно или нет. `this` внутри колбэка — экземпляр, или конструктор, если поле статическое.
- **Метаданные:** Присвойте `context.metadata[context.name]`, чтобы запомнить поле. После определения класса тот же объект читают как `User[Symbol.metadata]`. В большинстве сред нужен полифил `Symbol.metadata`, `target` не выше `es2022` и `"esnext.decorators"` в `lib`.

**When to use:**

- Пометить публичные поля экземпляра для сериализации или трассировки.
- Тот же `@` на методе или `accessor` — другой вид декоратора. Этот — для поля.

**Examples:**

```ts
function trace<This>(_value: undefined, context: ClassFieldDecoratorContext<This>) {
  context.addInitializer(function (this: This) {
    console.log(String(context.name), context.access.get(this));
  });
}

class User {
  @trace
  name = "Ada";
}
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Как decorators compose in TypeScript?</summary>

### Как decorators compose in TypeScript?

**Definition:**

Стопка декораторов применяется снизу вверх. Декоратор ближе к методу работает с исходным значением. Каждый декоратор выше работает с тем, что вернул нижний.

**How it works:**

- **Порядок:** Если `@bound` стоит над `@loggedMethod`, то `loggedMethod` декорирует `greet`, а `bound` декорирует результат `loggedMethod`. Побочные эффекты и обёртки видят этот порядок.
- **Одна строка:** `@bound @loggedMethod greet()` — та же стопка. Заметки допускают оба вида записи.
- **Фабрики:** Функция может вернуть декоратор, как это делает `loggedMethod("⚠️")`. Фабрику вызывают в выражении `@`. Вызов даёт функцию, которая затем участвует в применении снизу вверх.
- **Без возврата:** `bound` ничего не возвращает, поэтому метод не заменяет. Он только вызывает `context.addInitializer`. Это всё равно складывается с декоратором, который возвращает замену.

**When to use:**

- Совместить обёртку (логирование) и инициализатор (привязку) на одном методе.
- Держите стопку короткой. Порядок важен, как только один декоратор ждёт исходную функцию.

**Examples:**

```ts
function loggedMethod(originalMethod: any, _context: ClassMethodDecoratorContext) {
  return function replacement(this: any, ...args: any[]) {
    console.log("Entering");
    return originalMethod.call(this, ...args);
  };
}

function bound(_originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = context.name;
  if (context.private) {
    throw new Error(`Cannot bind private member ${String(methodName)}.`);
  }
  context.addInitializer(function (this: any) {
    this[methodName] = this[methodName].bind(this);
  });
}

class Person {
  constructor(public name: string) {}

  @bound
  @loggedMethod
  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Обсудите Reflection and how it works with decorators.</summary>

### Обсудите Reflection and how it works with decorators.

**Definition:**

Текущие декораторы отражают сведения, записывая обычный объект в `context.metadata`. Все декораторы одного класса делят этот объект. После декорирования конструктор отдаёт его как `Symbol.metadata`.

**How it works:**

- **Запись:** Декоратор кладёт имена или флаги в `context.metadata`, часто под ключом-`symbol`, чтобы не столкнуться с чужим кодом. `WeakMap` с ключом-объектом метаданных хранит данные приватно.
- **Чтение:** `instance.constructor[Symbol.metadata]` — этот объект. Помощник вроде примера `jsonify` в заметках TypeScript 5.2 обходит сохранённые имена свойств и читает их у экземпляра.
- **Пределы:** Новая схема не работает с `--emitDecoratorMetadata` и не умеет декорировать параметры. `context.static`, `context.private` и символьное `name` — проверки, которыми пример сериализации отклоняет члены, которые нельзя отразить.
- **Рантайм:** `Symbol.metadata` на многих движках всё ещё нужен полифил: `Symbol.metadata ??= Symbol("Symbol.metadata")`. Поставьте `target` не выше `es2022` и включите `"esnext.decorators"` в `lib`.
- **Старый флаг:** `--experimentalDecorators` — другой путь рефлексии. Не ждите, что декоратор, написанный под этот флаг, увидит `context.metadata`.

**When to use:**

- Небольшой сериализатор, реестр или фреймворк, которому нужно узнать, какие члены были декорированы.
- Метаданные не нужны, когда поведение может жить в самой функции-замене, как при логировании.

**Examples:**

```ts
function mark(_target: any, context: ClassFieldDecoratorContext): void {
  context.metadata[context.name] = true;
}

class User {
  @mark
  name = "Ada";
}

console.log(User[Symbol.metadata]);
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

### TypeScript Configuration and Compilation

<details>
<summary>Объясните the puRPOse of 'tsconfig.JSON'.</summary>

### Объясните the puRPOse of 'tsconfig.JSON'.

**Definition:**

`tsconfig.json` — файл проекта для компилятора TypeScript. Он перечисляет, какие файлы входят в программу и какие `compilerOptions` использовать. `tsc` читает его из каталога, который вы компилируете.

**How it works:**

- **Корень:** Папка этого файла — корень проекта. Опции и глобы считаются оттуда.
- **Две части:** `compilerOptions` управляет проверкой и выдачей. `files`, `include` и `exclude` задают набор файлов.
- **Как создать:** `tsc --init` пишет стартовый файл. С TypeScript 2.3 в нём включён `"strict": true`. Init в TypeScript 5.9 также ставит `"module": "nodenext"`, `"target": "esnext"`, `"sourceMap": true`, `"verbatimModuleSyntax": true`, `"isolatedModules": true` и `"skipLibCheck": true`.
- **Переопределение:** Поздний флаг в командной строке может выключить одну строгую проверку, как в `--strict --noImplicitThis false`. Флаг `strict` задаёт значение по умолчанию для своих проверок. Он их не запирает.

**When to use:**

- Любой проект, который компилируют через `tsc`, редактор или сборщик, уважающий проект TypeScript.
- Один конфиг на программу. Решение из нескольких проектов — это несколько конфигов, а не один глобальный набор флагов.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "strict": true,
    "sourceMap": true
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.0 — tsconfig include and exclude](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>Как you include and exclude files in compilation using 'tsconfig.JSON'?</summary>

### Как you include and exclude files in compilation using 'tsconfig.JSON'?

**Definition:**

`include` и `exclude` принимают глобы. `files` — явный список. Вместе они решают, какие файлы входят в компиляцию.

**How it works:**

- **Подстановки:** `*` совпадает внутри одного сегмента пути. `?` совпадает с одним символом. `**/` совпадает с любым вложенным каталогом. Сегмент, который состоит только из `*` или `.*`, оставляет поддерживаемые расширения: `.ts`, `.tsx` и `.d.ts`, плюс `.js` и `.jsx`, когда включён `allowJs`.
- **По умолчанию:** Если нет и `files`, и `include`, компилятор берёт файлы TypeScript в этом каталоге и ниже, минус `exclude`. Пустой `exclude` пропускает `node_modules`, `bower_components` и `jspm_packages`.
- **Объединение:** Если заданы `files` или `include`, программа — их объединение. `exclude` фильтрует файлы, пришедшие через `include`. Путь из `files` он не убирает.
- **Выход:** Файлы под `outDir` не входят, пока запись в `files` их не назовёт.

**When to use:**

- `include: ["src/**/*"]` для кода приложения и `exclude` для тестов или сгенерированных папок, которые не должны быть в этой программе.
- `files` для маленького явного набора или для выходного файла, который нужно компилировать снова.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

**Sources:**

- [TypeScript 2.0 — Glob support in tsconfig.json](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>Что такое some common compiler options in TypeScript?</summary>

### Что такое some common compiler options in TypeScript?

**Definition:**

`compilerOptions` выбирает JavaScript, который выдаётся, как резолвятся модули и насколько строг проверщик. Файл `tsc --init` в TypeScript 5.9 — текущая картина рекомендуемого набора.

**How it works:**

- **Язык и модули:** `target` — уровень JavaScript. `module` — формат модулей. Init 5.9 использует `"target": "esnext"` и `"module": "nodenext"`.
- **Проверка:** `"strict": true` включает строгое семейство. С 2.3 в него входят `strictNullChecks`, `noImplicitAny`, `noImplicitThis` и `alwaysStrict`, а поздние компиляторы могут добавить ещё. Init 5.9 также ставит `noUncheckedIndexedAccess` и `exactOptionalPropertyTypes` как дополнительные проверки. Эти два перечислены отдельно, рядом со `strict`.
- **Выдача:** `sourceMap`, `declaration` и `declarationMap` — «прочие выходы» в том init-файле. `rootDir` и `outDir` задают раскладку, там они показаны как комментарии.
- **Совместимость:** Тот же init включает `verbatimModuleSyntax`, `isolatedModules`, `moduleDetection: "force"`, `skipLibCheck` и `jsx: "react-jsx"`.

**When to use:**

- Начните с `tsc --init` и уберите опции, которые программе не нужны, например `jsx` в пакете без React.
- Ставьте `module` и `moduleResolution` под реальную среду: библиотека для Node или сборщик. Не копируйте конфиг браузерного приложения в публикуемый пакет, не проверив этот выбор.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "strict": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "sourceMap": true
  }
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>Как TypeScript support source Maps?</summary>

### Как TypeScript support source Maps?

**Definition:**

`"sourceMap": true` просит компилятор выдать source map вместе с JavaScript. Отладчик и стек ошибок могут тогда указывать на TypeScript, а не только на сгенерированный файл.

**How it works:**

- **Опция:** Её ставят в `compilerOptions`. Пример `tsconfig` в TypeScript 2.0 и вывод `tsc --init` в TypeScript 5.9 оба ставят `"sourceMap": true`.
- **Соседние выходы:** Init 5.9 группирует её с `"declaration": true` и `"declarationMap": true`. Карты объявлений — та же идея для файлов `.d.ts`, чтобы редактор переходил от типа к исходному TypeScript.
- **Раскладка:** Карты следуют за выдачей. Если задан `outDir`, JavaScript и его карта пишутся туда вместе с остальным выходом.

**When to use:**

- Любая сборка, которую отлаживают в браузере, Node или редакторе.
- Выключайте только для одноразовой компиляции, где лишние файлы мешают. Прод-бандлы часто всё ещё отдают карты трекеру ошибок и прячут их от публичного сайта.

**Examples:**

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true
  }
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.0 — tsconfig include and exclude](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>Обсудите incremental build support in TypeScript.</summary>

### Обсудите incremental build support in TypeScript.

**Definition:**

`"incremental": true` заставляет TypeScript сохранить сведения о прошлой компиляции и использовать их в следующей, чтобы не повторять неизменённую работу.

**How it works:**

- **Флаг:** TypeScript 3.4 добавил `incremental`. Компилятор ищет файл `.tsbuildinfo` в выходном каталоге и по нему ускоряет поздние сборки.
- **Где лежит:** При `"outDir": "./lib"` этот файл — часть выхода рядом с JavaScript. `include` по-прежнему решает, какие исходники входят в программу.
- **Чистая сборка:** Удалите `.tsbuildinfo`, когда нужна компиляция, которая не доверяет прошлому графу.

**When to use:**

- Локальные и CI-компиляции проекта, где полная проверка доминирует в сборке.
- Выключите для скрипта из одного файла, где файл сведений тяжелее самой компиляции.

**Examples:**

```json
{
  "compilerOptions": {
    "incremental": true,
    "outDir": "./lib"
  },
  "include": ["./src"]
}
```

**Sources:**

- [TypeScript 3.4 — Incremental compilation](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html)

</details>

<details>
<summary>Объясните how the "noImplicitAny" compiler option works.</summary>

### Объясните how the "noImplicitAny" compiler option works.

**Definition:**

`noImplicitAny` делает неявный `any` ошибкой. Так бывает, когда тип не написан и TypeScript не может вывести его из контекста.

**How it works:**

- **Без флага:** Справочник говорит, что компилятор в этом случае обычно берёт `any`. `any` не проверяется типами.
- **С флагом:** Нужно написать тип или дать проверщику значение, из которого он выведет тип, например аргумент типизированного вызова или инициализатор.
- **`strict`:** С TypeScript 2.3 `"strict": true` включает `noImplicitAny` вместе с остальными строгими проверками. Можно включить `strict` и затем явно выключить одну эту проверку.
- **Вывод остаётся:** `const myName = "Alice"` имеет тип `string` без аннотации. Флаг не требует аннотацию у каждого имени.

**When to use:**

- Всегда, кроме миграции файла JavaScript, где нужна временная дыра. `"strict": true` — переключатель, который её включает.
- Где значение правда нетипизировано, пишите `unknown` и сужайте его, а не оставляйте дыру как `any`.

**Examples:**

```ts
function greet(name: string) {
  return `Hello, ${name}`;
}

const myName = "Alice";
greet(myName);
```

**Sources:**

- [TypeScript handbook — noImplicitAny](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#noimplicitany)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>Как you enable strict null checks in TypeScript?</summary>

### Как you enable strict null checks in TypeScript?

**Definition:**

Поставьте `"strictNullChecks": true` или включите `"strict": true`, куда эта проверка входит. Когда она включена, `null` и `undefined` не присваиваются другим типам, и перед использованием значения их проверяют.

**How it works:**

- **Выключено:** `null` и `undefined` можно присвоить любому типу, а значение, которого может не быть, всё равно можно использовать. Справочник сравнивает это с языками без проверки null и говорит, что отсутствие проверок — крупный источник багов.
- **Включено:** Сначала сузьте. `if (name === null)` отделяет отсутствующий случай от строки, так же как необязательное свойство проверяют перед использованием.
- **`!`:** Постфикс `!` убирает из типа `null` и `undefined`. Это утверждение. Поведение в рантайме оно не меняет, поэтому его ставят только когда уже известно, что значение есть.
- **Как включить:** `"strict": true` задаёт значение по умолчанию для `strictNullChecks` (и для `noImplicitAny`, `noImplicitThis` и `alwaysStrict` по состоянию на 2.3). Один флаг `"strictNullChecks": true` включает эту проверку сам по себе.

**When to use:**

- Включайте в каждом проекте, который может оплатить миграцию. Совет справочника — включать, когда это практично.
- Во время миграции включайте по проекту и чините места вызова, а не глушите каждую ошибку через `!`.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

```ts
function hello(name: string | null) {
  if (name === null) {
    return;
  }
  console.log("Hello, " + name.toUpperCase());
}
```

**Sources:**

- [TypeScript handbook — null and undefined](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#null-and-undefined)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

### TypeScript Typings and @types

<details>
<summary>Как you manage type definitions for external libraries in TypeScript?</summary>

### Как you manage type definitions for external libraries in TypeScript?

**Definition:**

Берите файл объявлений, который уже лежит в пакете. Если типов нет, ставьте соответствующий пакет `@types`. TypeScript читает в `package.json` поле `"types"` (или старое `"typings"`), затем `"main"`.

**How it works:**

- **Свои типы:** Пакет, в котором уже есть `.d.ts`, не нуждается во втором `@types`. В документации по публикации поле `"types"` указывает на этот файл. Значок TypeScript на npm появляется только при наличии `"types"`.
- **`@types`:** `npm install --save-dev @types/lodash` — пример из справочника для библиотеки без своих типов. Импорт этого пакета подтягивает объявления.
- **Глобальные имена:** Опция `types` перечисляет пакеты `@types`, которые добавляют глобалы, например `node` или `jest`. В TypeScript 6.0 она по умолчанию равна `[]`, поэтому такие глобалы не подключаются, пока вы их не назовёте. `"types": ["*"]` возвращает старое поведение «загрузить каждый пакет `@types`».
- **Проверка:** `skipLibCheck` пропускает проверку файлов `.d.ts`. Ошибка может остаться скрытой, если ваш код дополняет объявление; в заметках 2.0 это названо редким случаем. `"declaration": true` — способ, которым ваш пакет сам выпускает `.d.ts`.

**When to use:**

- Ставьте `@types` только после проверки, что у библиотеки нет поля `"types"`.
- На TypeScript 6 и новее перечислите `node` или раннер тестов в `types`, если пропали глобалы вроде `process` или `describe`.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "types": ["node"]
  }
}
```

**Sources:**

- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)
- [TypeScript 6.0 — types defaults to []](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>Что такое DefinitelyTyped and how is it related to TypeScript?</summary>

### Что такое DefinitelyTyped and how is it related to TypeScript?

**Definition:**

DefinitelyTyped — репозиторий файлов объявлений для JavaScript-библиотек, которые не публикуют типы сами. Эти объявления выходят в организацию `@types` на npm.

**How it works:**

- **Два пути публикации:** Справочник говорит класть `.d.ts` рядом с пакетом, если типы генерируются из исходников (`"declaration": true` работает и для TypeScript, и для JavaScript). Иначе типы отправляют в DefinitelyTyped.
- **Что ставите вы:** Запись DefinitelyTyped становится пакетом вроде `@types/lodash`. Установка — обычная dev-зависимость npm. Отдельный инструмент TypeScript не нужен.
- **Не вторая копия:** Если библиотека позже публикует свой файл `"types"`, пакет `@types` больше не ставят. Читать нужно встроенный файл.

**When to use:**

- Зависимость — JavaScript-пакет без поля `"types"`.
- Вы ведёте типы чужой JavaScript-библиотеки. Их отправляют в DefinitelyTyped, а не публикуют под своим именем `@types`.

**Examples:**

```text
npm install --save-dev @types/lodash
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)

</details>

<details>
<summary>Объясните how to use typing files from npm.</summary>

### Объясните how to use typing files from npm.

**Definition:**

Установите пакет с файлами `.d.ts` и импортируйте библиотеку как обычно. Если TypeScript не идёт по карте `"exports"`, он смотрит в `package.json` сначала `"types"`, затем `"typings"`, затем `"main"`.

**How it works:**

- **Свои типы:** `"types": "./lib/main.d.ts"` рядом с `"main": "./lib/main.js"`. `"typings"` — синоним `"types"`.
- **Отдельные типы:** `npm install --save-dev @types/lodash`. Импорт `from "lodash"` находит эти объявления. Для обычной установки npm triple-slash reference не нужен.
- **Глобалы и импорты:** Импорты подтягивают типы модулей, которые вы импортируете. Массив `types` — только список глобальных пакетов. В TypeScript 6.0 он по умолчанию `[]`. Добавьте `"node"` или `"jest"`, когда ошибка говорит, что глобального имени нет.
- **Свой выпуск:** `"declaration": true` пишет `.d.ts` рядом с JavaScript, который вы публикуете.

**When to use:**

- Добавляйте `@types/...` в devDependencies, когда у runtime-пакета нет типов.
- Ставьте `"types"` на пакет, который публикуете вы: реестр использует это поле для значка TypeScript, даже если подстановка расширения нашла бы файл сама.

**Examples:**

```json
{
  "name": "awesome",
  "version": "1.0.0",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [TypeScript handbook — Module resolution reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html)

</details>

### Integrating TypeScript with Frameworks

<details>
<summary>Как you integrate TypeScript with Angular?</summary>

### Как you integrate TypeScript with Angular?

**Definition:**

Компонент Angular — это класс TypeScript. Декоратор `@Component` задаёт селектор, шаблон и стили. Сборкой TypeScript в проекте управляет Angular CLI.

**How it works:**

- **Класс и метаданные:** В классе живёт поведение. `@Component` указывает строку шаблона или `templateUrl`, стили или `styleUrl`. Селектор — имя элемента в HTML.
- **Шаблон:** Свойства, объявленные в классе, доступны в шаблоне. В документации Angular класс — место для ввода пользователя и запросов.
- **Сборка:** На странице Babel в справочнике TypeScript Angular назван фреймворком, который уже выбирает, как компилировать TypeScript. Для приложения, созданного CLI, выбор между `tsc` и Babel делать не нужно.
- **Библиотеки:** В `tsconfig` библиотеки можно поставить `angularCompilerOptions.compilationMode` в `"partial"`, чтобы пакет не был привязан к одной версии рантайма Angular.

**When to use:**

- Новый UI в Angular — класс компонента с `@Component`, а не обычный скрипт.
- `compilationMode` у приложения оставляют по умолчанию. `"partial"` ставят, когда публикуют библиотеку в формате пакета Angular.

**Examples:**

```ts
@Component({
  selector: "profile-photo",
  template: `<img src="profile-photo.jpg" alt="Your profile photo" />`,
})
export class ProfilePhoto {}
```

**Sources:**

- [Angular — Components](https://angular.dev/guide/components)
- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)

</details>

<details>
<summary>Объясните integrating TypeScript with React.</summary>

### Объясните integrating TypeScript with React.

**Definition:**

Компоненты пишут в `.tsx` и задают опцию компилятора `jsx`. Для React 17 и новее `react-jsx` и `react-jsxdev` выпускают автоматический JSX runtime.

**How it works:**

- **`jsx`:** `"jsx": "react-jsx"` — продакшен-значение из заметок TypeScript 4.1. `"jsx": "react-jsxdev"` — значение для разработки. Dev-конфиг может сделать `extends` продакшен-конфига и переопределить только `jsx`.
- **Старая фабрика:** `"jsx": "react"` выпускает `React.createElement`. `jsxFactory` и `jsxFragmentFactory` переименовывают эту фабрику — настройка для JSX-библиотеки не из React.
- **Проверка:** JSX проверяется типами. На выходе всё равно JavaScript. `"strict": true` стоит в том же блоке `compilerOptions`, что и `jsx`.
- **Типы:** Свои типы компонентов публикует сам пакет React. Их импортируют в файле `.tsx`. Второй пакет `@types` не нужен, если библиотека уже публикует `"types"`.

**When to use:**

- Новый код React: `jsx` равен `react-jsx`, файлы называются `.tsx`.
- Для локального конфига берите `react-jsxdev`. `react` плюс своя фабрика нужны только JSX-библиотеке, которая не использует автоматический runtime.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "target": "es2015",
    "jsx": "react-jsx",
    "strict": true
  }
}
```

```tsx
export const el = <div>foo</div>;
```

**Sources:**

- [TypeScript 4.1 — React 17 JSX factories](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html)

</details>

<details>
<summary>Как you use TypeScript with Vue.js?</summary>

### Как you use TypeScript with Vue.js?

**Definition:**

На скрипт компонента ставят `lang="ts"`. В `<script setup lang="ts">` в `defineProps` и `defineEmits` передают аргумент-тип, и пропсы с событиями проверяются.

**How it works:**

- **Включение:** `<script lang="ts">` выбирает препроцессор TypeScript для этого блока. `<script setup lang="ts">` — форма Composition API.
- **Пропсы:** `defineProps<Props>()` принимает интерфейс. Необязательные поля пишут через `?`. Документация Vue 3 разрешает и аргумент-тип прямо на месте.
- **События:** `defineEmits<{ (e: "change", id: number): void }>()` задаёт имя события и нагрузку. С Vue 3.3 короче: `change: [id: number]` — именованный кортеж.
- **Проверка в рантайме:** Объектная форма `defineEmits` может проверить нагрузку и вернуть true или false. Форма с аргументом-типом рантайм-валидатор не добавляет.

**When to use:**

- Типизируйте пропсы и события в однофайловом компоненте через `<script setup lang="ts">`.
- Объектную форму берите, когда нагрузку события нужно отклонить в рантайме, а не только в проверке типов.

**Examples:**

```vue
<script setup lang="ts">
interface Props {
  foo: string;
  bar?: number;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  change: [id: number];
}>();
</script>
```

**Sources:**

- [Vue — TypeScript with Composition API](https://vuejs.org/guide/typescript/composition-api)
- [Vue — script setup](https://vuejs.org/api/sfc-script-setup)

</details>

<details>
<summary>Обсудите using TypeScript in a Node.js application.</summary>

### Обсудите using TypeScript in a Node.js application.

**Definition:**

Направьте `module` на резолвер Node и поставьте `@types/node` для глобалов Node. `"module": "nodenext"` (или `"node16"`) заставляет TypeScript следовать правилам поиска ESM и CommonJS в Node.

**How it works:**

- **Формат:** В файле `.mts` `import` идёт по алгоритму ESM, и относительному пути нужно расширение (`./mod.js`). `import x = require("./mod")` идёт по алгоритму CommonJS и может обойтись без расширения. Файл `.cts` воспринимает `import` как `require`. Динамический `import()` остаётся ESM.
- **Глобалы:** `process`, `fs` и `path` приходят из `@types/node`. TypeScript 6.0 не загружает каждый пакет `@types` сам. Текст ошибки говорит сделать `npm i --save-dev @types/node` и добавить `"node"` в `types`.
- **Ассерты:** Под `--module nodenext` в TypeScript 5.8 импорт-ассершн с `assert` — ошибка. Текущий синтаксис Node использует `with` (import attributes).
- **Выпуск:** `nodenext` оставляет ESM-`import` как написано, если файл — ES-модуль, и выпускает `require` для файлов CommonJS.

**When to use:**

- Приложение Node или пакет для Node: `module` равен `nodenext`, в `types` есть `"node"`.
- `.mts` или `"type": "module"` — когда файл должен остаться `import`/`export`. `.cts` — когда файл должен выпустить `require`.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "types": ["node"],
    "strict": true
  }
}
```

**Sources:**

- [TypeScript 4.7 — module node16](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-7.html)
- [TypeScript 6.0 — types defaults to []](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)
- [TypeScript handbook — Modules reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html)

</details>

### TypeScript Tooling and Ecosystem

<details>
<summary>Что такое the benefits of using tools like TSLInt?</summary>

### Что такое the benefits of using tools like TSLInt?

**Definition:**

Линтер ловит то, чего не видит проверка типов: например, Promise, который никто не ждёт. Сам TSLint — цель миграции: typescript-eslint прямо говорит перенести эти правила в ESLint.

**How it works:**

- **Парсер:** `@typescript-eslint/parser` даёт ESLint читать TypeScript. `typescript-eslint` отдаёт рекомендованный flat config поверх рекомендованного конфига ESLint.
- **Правила с типами:** `parserOptions.projectService: true` (или `project` на `tsconfig.json`) даёт правилам доступ к проверке типов. Документированный пример — `@typescript-eslint/no-floating-promises`.
- **Мост с TSLint:** `@typescript-eslint/eslint-plugin-tslint` может гонять старые правила TSLint внутри ESLint на время переезда. В документации пакета сказано уйти с TSLint и следовать Getting Started.
- **Граница:** Линт не заменяет `tsc`. Он сообщает дополнительные стилевые и ошибочные шаблоны. Проходит ли программа проверку типов, решает проверка типов.

**When to use:**

- В репозитории TypeScript ставят typescript-eslint, а не новый TSLint.
- Правила с типами включают, когда проверка должна видеть типы, и выключают их для обычных `.js` через `tseslint.configs.disableTypeChecked`.

**Examples:**

```js
// @ts-check
import js from "@eslint/js";
import tseslint from "typescript-eslint";

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommended,
);
```

**Sources:**

- [typescript-eslint — packages](https://typescript-eslint.io/packages/typescript-eslint)
- [typescript-eslint — eslint-plugin-tslint](https://typescript-eslint.io/packages/eslint-plugin-tslint)

</details>

<details>
<summary>Можете ли вы describe how to set up a build process with TypeScript and Webpack?</summary>

### Можете ли вы describe how to set up a build process with TypeScript and Webpack?

**Definition:**

Ставят `typescript` и `ts-loader`, затем правило отправляет файлы `.ts` и `.tsx` в `ts-loader`. Webpack берёт параметры компиляции из `tsconfig.json`.

**How it works:**

- **Лоадер:** В руководстве webpack по TypeScript: `test: /\.tsx?$/`, `use: "ts-loader"`, `exclude: /node_modules/`. В `resolve.extensions` перечислены `.tsx`, `.ts` и `.js`.
- **Формат модулей:** Не ставьте `module` в `"CommonJS"`. Руководство говорит, что тогда webpack не сможет tree-shake выход. В примере `"module": "esnext"` и `"moduleResolution": "bundler"`.
- **Карты кода:** `"sourceMap": true` в `tsconfig.json` и `devtool: "inline-source-map"` в конфиге webpack, чтобы бандл хранил карту обратно к TypeScript.
- **Скорость:** `ts-loader` с `transpileOnly: true` пропускает проверку типов. Проверка уходит в `fork-ts-checker-webpack-plugin` в отдельный процесс. Если уже стоит `babel-loader`, вместо ещё одного лоадера можно взять `@babel/preset-typescript`. Этот плагин типы не проверяет.

**When to use:**

- Приложение собирается для браузера, и webpack должен импортировать не только JS. Для этого руководство выбирает `ts-loader`.
- `transpileOnly` — на медленной сборке, а проверку типов оставляют отдельно, чтобы ошибки не пропадали.

**Examples:**

```ts
import path from "node:path";
import { fileURLToPath } from "node:url";

const __dirname = path.dirname(fileURLToPath(import.meta.url));

export default {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  module: {
    rules: [
      { test: /\.tsx?$/, use: "ts-loader", exclude: /node_modules/ },
    ],
  },
  resolve: { extensions: [".tsx", ".ts", ".js"] },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

**Sources:**

- [webpack — TypeScript](https://webpack.js.org/guides/typescript)
- [webpack — Build performance](https://webpack.js.org/guides/build-performance)

</details>

<details>
<summary>Как TypeScript work with Babel?</summary>

### Как TypeScript work with Babel?

**Definition:**

`preset-typescript` в Babel выпускает JavaScript и не проверяет типы. `tsc` по-прежнему проверяет типы и при `declaration` вместе с `emitDeclarationOnly` пишет `.d.ts`, не писая `.js`.

**How it works:**

- **Кто выпускает:** Правило из справочника: если выход почти совпадает с входными файлами, берите `tsc`. Если нужен конвейер с несколькими выходами, Babel транспилирует, а `tsc` проверяет типы.
- **Фреймворки:** Angular, NestJS, tsdx и остальные фреймворки из Getting Started уже выбирают это за вас.
- **Чего Babel не делает:** Babel не сообщает об ошибках типов во время выпуска, поэтому ошибка, пропущенная в редакторе, может уехать в прод. Babel также не умеет писать `.d.ts`.
- **Флаги:** Справочник по Babel говорит включить `isolatedModules`, чтобы каждый файл было безопасно транспилировать отдельно, и `declaration` с `emitDeclarationOnly`, чтобы `tsc` писал только объявления. `emitDeclarationOnly` появился в TypeScript 2.8 как раз для случая, когда JavaScript пишет другой инструмент, например Babel.

**When to use:**

- Уже есть конвейер Babel, и к нему добавляют типы: ставят пресет и отдельную проверку `tsc`.
- Библиотека, которая публикует типы, не может обойтись одним Babel. `.d.ts` выпускает `tsc`.

**Examples:**

```json
{
  "compilerOptions": {
    "isolatedModules": true,
    "declaration": true,
    "emitDeclarationOnly": true
  }
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript 2.8 — emitDeclarationOnly](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html)

</details>

<details>
<summary>Объясните using TypeScript with Visual Studio Code or other IDEs.</summary>

### Объясните using TypeScript with Visual Studio Code or other IDEs.

**Definition:**

Языковой сервис TypeScript питает редактирование TypeScript и заодно JavaScript в семействе Visual Studio. Редактор можно направить на выбранную версию TypeScript.

**How it works:**

- **Версия:** В заметках TypeScript 3.9 сказано, что Visual Studio Code умеет выбирать версию TypeScript. Расширение JavaScript/TypeScript Nightly следит за ночной сборкой. Sublime Text 3 тоже умеет выбрать версию. Visual Studio 2017 и 2019 ставят SDK и MSBuild.
- **Переход к определению:** `"declarationMap": true` пишет карту от `.d.ts` к исходному файлу. В справочнике по файлам объявлений это нужно, чтобы Go to Definition в VS Code открывал исходник, а не только сгенерированное объявление.
- **Отладка:** `"sourceMap": true` выпускает карту от JavaScript обратно к TypeScript. Бандлер вроде webpack всё равно должен эту карту подхватить (`devtool`).
- **Файлы JavaScript:** `"allowJs": true` позволяет тому же проекту читать `.js`. Редактор тогда проверяет их опциями проекта.

**When to use:**

- Версия TypeScript в редакторе должна совпадать с версией в репозитории, чтобы редактор и `tsc` сообщали одни и те же ошибки.
- `declarationMap` публикуют, когда в пакете есть исходники `.ts` и потребителям нужно переходить к ним.

**Examples:**

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "emitDeclarationOnly": true,
    "outDir": "dist",
    "allowJs": true
  }
}
```

**Sources:**

- [TypeScript 3.9 — Editor improvements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html)
- [TypeScript handbook — Creating .d.ts from .js](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)

</details>

### TypeScript Best Practices and Patterns

<details>
<summary>Что такое the best practices for structuring a TypeScript application?</summary>

### Что такое the best practices for structuring a TypeScript application?

**Definition:**

Публичный API каждого файла вешают на `export`, включают `strict`, а файлы программы называет `tsconfig.json`. Для нового кода справочник по умолчанию выбирает ES-модули, не пространства имён.

**How it works:**

- **Модули:** Файл с `import` или `export` верхнего уровня — модуль. Его типы и значения локальны, пока вы их не экспортируете. `export {}` делает файл модулем, когда другого экспорта нет.
- **Конфиг:** `tsc --init` в TypeScript 5.9 включает `strict`, `verbatimModuleSyntax`, `isolatedModules` и `moduleDetection: "force"`. `verbatimModuleSyntax` оставляет в выпуске импорт значения и выбрасывает `import type`.
- **Раскладка:** Глобы `include`, например `src/**/*`, выбирают программу. `exclude` не отменяет файл, перечисленный в `files`. `outDir` — куда пишется выпуск.
- **Резолвинг:** Приложение или библиотека для Node ставят `module: "nodenext"`. Сборка бандлером — `moduleResolution: "bundler"`. Руководство webpack по TypeScript говорит не выпускать CommonJS, если бандл должен делать tree-shaking.

**When to use:**

- Начинайте с `tsc --init` и меняйте `module` под Node или бандлер, а не пишите слабый конфиг с нуля.
- Типы экспортируйте через `export type` или `import type`, чтобы ребро только из типов стёрлось и не стало импортом в рантайме.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "module": "nodenext",
    "moduleDetection": "force",
    "verbatimModuleSyntax": true,
    "isolatedModules": true
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/modules/introduction.html)

</details>

<details>
<summary>Как you handle async/await in TypeScript?</summary>

### Как you handle async/await in TypeScript?

**Definition:**

Функция `async` возвращает `Promise`. `await` ждёт, пока этот Promise завершится, и у выражения после `await` тип значения этого Promise.

**How it works:**

- **С 1.6:** `async` работает на объявлении функции, стрелке, функциональном выражении и методе класса. В теле можно сделать `await` у `Promise<T>`. После `await` значение имеет тип `T`.
- **Вывод:** При глобальном `Promise` и подходящем target TypeScript выводит тип возврата как `Promise<...>`. В заметках 1.6 этот вывод показан при `--target ES6`. Текущий `tsc --init` ставит `target` в `esnext`.
- **Ошибки:** Отклонённый Promise в рантайме всё равно отклонён. Тип успешного `await` не включает причину отклонения.
- **Итерация:** `async function*` — асинхронный генератор. `for await` законен только внутри async-функции или async-генератора. Нужны глобальный `Promise` и `Symbol.asyncIterator`.

**When to use:**

- Пишите `Promise<T>` явно, когда функция входит в публичный API и тип возврата не должен выводиться из тела.
- `for await` — для асинхронного итерируемого. Обычный `for...of` его не обходит.

**Examples:**

```ts
async function addOne(value: Promise<number>): Promise<number> {
  const current = await value;
  return current + 1;
}
```

**Sources:**

- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)
- [TypeScript 2.3 — Async generators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>Обсудите how you manage state in TypeScript-BASEd applications.</summary>

### Обсудите how you manage state in TypeScript-BASEd applications.

**Definition:**

Каждое состояние — отдельный член размеченного объединения. Общее литеральное свойство, например `status`, даёт проверке сузить остальной объект в каждой ветке.

**How it works:**

- **Объединение:** `RequestState` — это `"idle" | "loading" | "success" | "error"`, и только член success несёт `data`. Присвоить не ту нагрузку — ошибка типа.
- **Сужение:** `switch (state.status)` открывает `state.data` только в ветке `"success"`. Проверка на `never` в `default` ломает сборку, когда добавили новый status и не обработали его.
- **Обновления:** Возвращайте новый объект, а не меняйте поле `readonly`. `Readonly<T>` и свойства `readonly` отвергают присваивание.
- **Модули:** Тип состояния и функция следующего состояния живут в модуле. Тип экспортируют. Вызывающие его импортируют. До локальных имён другого файла они не дотягиваются.

**When to use:**

- У экрана или запроса несколько форм. Один тип объекта с необязательными полями прячет, какие поля есть при каком status.
- Библиотека состояния нужна, когда одно значение читают далёкие модули. Локальное состояние — это значение плюс функция, которая возвращает следующее.

**Examples:**

```ts
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: Error };

function label<T>(state: RequestState<T>): string {
  switch (state.status) {
    case "idle":
      return "idle";
    case "loading":
      return "loading";
    case "success":
      return "ready";
    case "error":
      return state.error.message;
    default: {
      const _exhaustive: never = state;
      return _exhaustive;
    }
  }
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>Что такое some common design patterns used in TypeScript?</summary>

### Что такое some common design patterns used in TypeScript?

**Definition:**

У TypeScript нет своей библиотеки паттернов. Обычные объектные и функциональные схемы выражают классами, типами функций и объединениями, а проверка следит за формой.

**How it works:**

- **Стратегия:** Тип функции вроде `(value: number) => number` и есть стратегия. Вызывающий передаёт функцию. Класс не обязателен.
- **Состояние:** Размеченное объединение — это автомат. У каждого `status` свои поля. `switch` и `default` с `never` закрывают каждый член.
- **Зависимость:** Конструктор класса принимает интерфейс. В тесте передают другой класс с тем же интерфейсом. Потребитель зависит от списка методов, а не от одного конкретного класса.
- **Граница модуля:** Экспортируют интерфейс и фабрику. Вспомогательные функции оставляют без export, чтобы другие файлы их не вызывали.

**When to use:**

- Тип функции — когда меняется только одно поведение.
- Класс — когда нужны конструктор, поля `private` или `implements`.

**Examples:**

```ts
type Format = (value: number) => string;

function render(value: number, format: Format): string {
  return format(value);
}
```

**Sources:**

- [TypeScript handbook — Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html)
- [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)

</details>

<details>
<summary>Как you debug a TypeScript application?</summary>

### Как you debug a TypeScript application?

**Definition:**

Выпускайте карту кода, чтобы отладчик показывал файл `.ts`. Редактор должен использовать ту же версию TypeScript, что и проект: тогда точки останова и ошибки совпадают с `tsc`.

**How it works:**

- **Компилятор:** `"sourceMap": true` пишет карту от выпущенного JavaScript обратно к TypeScript. Руководство webpack по TypeScript дополнительно ставит `devtool: "inline-source-map"`, чтобы бандл сохранил эту карту.
- **Объявления:** `"declarationMap": true` связывает `.d.ts` с исходником. Go to Definition в VS Code тогда открывает файл `.ts` или `.js`, на который указывает пример в справочнике.
- **Редактор:** Visual Studio Code умеет выбрать версию TypeScript из рабочей области. Несовпадение значит, что редактор сообщает другие ошибки, чем `tsc`.
- **Ошибки типов:** Карта кода ошибку типа не чинит. Её сообщает `tsc --noEmit` или языковой сервис редактора ещё до шага по JavaScript.

**When to use:**

- Карты кода включают в сборочной конфигурации для разработки. Публиковать их необязательно: руководство по выбору опций говорит, что они помогают отладке, только если вы также отдаёте исходники `.ts`, и они увеличивают размер пакета.
- Версию TypeScript рабочей области выбирают до того, как искать ошибку, которую видит только один из двух инструментов.

**Examples:**

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "declarationMap": true
  }
}
```

**Sources:**

- [webpack — TypeScript](https://webpack.js.org/guides/typescript)
- [TypeScript handbook — Choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

</details>

### TypeScript Testing and Quality Assurance

<details>
<summary>Как you write unit tests for TypeScript code?</summary>

### Как you write unit tests for TypeScript code?

**Definition:**

Тест пишут в файле `.ts` и импортируют проверяемую функцию. Jest запускает его после препроцессора, который превращает TypeScript в JavaScript. Файл теста всё равно проходит проверку типов.

**How it works:**

- **Глобалы:** `npm install --save-dev @jest/globals` и импорт `describe`, `test` и `expect` оттуда. Эти типы едут вместе с Jest. `@types/jest` — вариант DefinitelyTyped для глобалов, которые вы не импортируете. Мажорную версию подгоняют под ту версию Jest, которую запускаете.
- **Трансформ:** Getting started у Jest называет `ts-jest` или `@babel/preset-typescript` способом транспилировать тесты. Пресет Babel типы не проверяет. Если трансформ такой, `tsc` всё равно запускают отдельно.
- **Список глобалов:** В TypeScript 6 `types` по умолчанию `[]`. Если пользуетесь `@types/jest` без импорта, добавьте `"jest"` в `types`. Импорт из `@jest/globals` этой записи не требует.
- **Проверки:** `expect(sum(1, 2)).toBe(3)` — тест из документации. Типы аргументов `sum` проверяются, когда файл теста входит в программу TypeScript.

**When to use:**

- Импортируйте из `@jest/globals`, чтобы типы теста шли за той версией Jest, которую вы поставили.
- Держите тесты внутри `include` (или project reference), чтобы неверный аргумент был ошибкой `tsc`, а не только падением в рантайме.

**Examples:**

```ts
import { describe, expect, test } from "@jest/globals";
import { sum } from "./sum";

describe("sum module", () => {
  test("adds 1 + 2 to equal 3", () => {
    expect(sum(1, 2)).toBe(3);
  });
});
```

**Sources:**

- [Jest — Getting started](https://jestjs.io/docs/getting-started)

</details>

<details>
<summary>Какие testing frameworks обычно используют с TypeScript?</summary>

### Какие testing frameworks обычно используют с TypeScript?

**Definition:**

Jest — обычный раннер модульных тестов, с `ts-jest` или пресетом TypeScript у Babel для транспиляции. Playwright гоняет сквозные браузерные тесты из файлов TypeScript, которые импортируют `@playwright/test`.

**How it works:**

- **Jest:** Ставят `ts-jest` или `@babel/preset-typescript`, если Babel уже транспилирует приложение. Ставят `@jest/globals` и импортируют API тестов, либо `@types/jest`, когда файл теста пользуется необъявленными глобалами.
- **Playwright:** `import { test, expect } from "@playwright/test"`. Тесты лежат в файлах вроде `example.spec.ts`. `playwright.config.ts` вызывает `defineConfig` для `use`, таймаутов и `expect`.
- **Линт:** typescript-eslint — не раннер тестов. В его примере для Jest включают `eslint-plugin-jest` для файлов под `test/**`.
- **Типы:** Пакет Playwright уже содержит типы `test` и `expect`. Импортируемые API Jest приходят из `@jest/globals`. Второй файл объявлений для них не пишут.

**When to use:**

- Jest — для функции или модуля, без браузера.
- Playwright — когда проверка про страницу: заголовок, роль или клик.

**Examples:**

```ts
import { test, expect } from "@playwright/test";

test("has title", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await expect(page).toHaveTitle(/Playwright/);
});
```

**Sources:**

- [Jest — Getting started](https://jestjs.io/docs/getting-started)
- [Playwright — Writing tests](https://playwright.dev/docs/writing-tests)

</details>

<details>
<summary>Обсудите how to perform end-to-end testing in TypeScript applications.</summary>

### Обсудите how to perform end-to-end testing in TypeScript applications.

**Definition:**

Сквозной тест управляет браузером. TypeScript API у Playwright открывает страницу, действует через локаторы и проверяет через `expect`, который повторяет попытку, пока условие не выполнится или не кончится таймаут.

**How it works:**

- **Файл теста:** `import { test, expect } from "@playwright/test"`. `test` даёт `page`. `page.goto` грузит URL. `page.getByRole` ищет доступный элемент. `expect(page).toHaveTitle` и `expect(locator).toBeVisible` — проверки из документации.
- **Конфиг:** `playwright.config.ts` по умолчанию экспортирует `defineConfig({ use: { ... } })`. На странице опций показаны `headless`, `viewport`, `ignoreHTTPSErrors` и `video: "on-first-retry"`. `expect.timeout` задаёт, сколько ждут автоповторные проверки.
- **Объекты страницы:** Класс может обернуть `page`, когда одни и те же клики повторяются. Тест всё равно импортирует `test` и `expect`. Класс — обычный TypeScript.
- **Файлы JavaScript:** Страница writing-tests говорит поставить `// @ts-check` в начало JavaScript-теста, чтобы VS Code его проверил. Тест `.ts` проверяется как TypeScript.

**When to use:**

- Покрывайте путь пользователя: открыть приложение, кликнуть, увидеть следующий заголовок.
- Модульные проверки оставляйте в Jest. Браузерный тест медленнее и не подходит для чистой функции.

**Examples:**

```ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  use: {
    viewport: { width: 1280, height: 720 },
    video: "on-first-retry",
  },
});
```

**Sources:**

- [Playwright — Writing tests](https://playwright.dev/docs/writing-tests)
- [Playwright — Test configuration](https://playwright.dev/docs/test-configuration)

</details>

### TypeScript for Backend Development

<details>
<summary>Как you use TypeScript with Express.js?</summary>

### Как you use TypeScript with Express.js?

**Definition:**

Приложение пишут на TypeScript и импортируют `Express`, `Request` и `Response` из `express`. В стартовом примере Express файл можно запустить самим Node: он снимает типы без отдельного шага сборки.

**How it works:**

- **Приложение:** `const app: Express = express()`. `app.get` и `app.post` принимают путь и обработчик. У обработчика `req` имеет тип `Request`, а `res` — `Response`.
- **Те же объекты, что у Node:** На странице hello-world сказано, что `req` и `res` — это запрос и ответ Node, поэтому методы Node вроде `req.pipe()` на месте.
- **Настройки проекта:** Приложение Node ставит `"module": "nodenext"` и в TypeScript 6 перечисляет `"node"` в `types` после `npm i --save-dev @types/node`. Типы `Request` и `Response` в документированном импорте даёт сам пакет Express.
- **Маршруты:** `app.route("/book")` связывает `.get`, `.post` и `.put` на одном пути. Каждый обработчик по-прежнему `(req: Request, res: Response) => ...`.

**When to use:**

- Подписывайте `req` и `res` у публичного обработчика, чтобы неверный аргумент `res.send` или пропущенный импорт был виден в редакторе.
- Запуск Node со снятием типов подходит, когда в файле только стираемые типы. Проект с enum или другим нестираемым синтаксисом всё равно нуждается в `tsc` или бандлере.

**Examples:**

```ts
import express, { type Express, type Request, type Response } from "express";

const app: Express = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});

app.listen(3000);
```

**Sources:**

- [Express — Installing](https://expressjs.com/en/starter/installing)
- [Express — Hello world](https://expressjs.com/en/starter/hello-world)

</details>

<details>
<summary>Обсудите creating RESTful services with TypeScript.</summary>

### Обсудите creating RESTful services with TypeScript.

**Definition:**

REST-обработчик — это метод маршрута Express: `get`, `post` и остальные методы HTTP. Обработчик типизируют через `Request` и `Response`, а JSON, который он читает, — отдельным интерфейсом.

**How it works:**

- **Методы:** `app.get` читает, `app.post` создаёт. Руководство по маршрутам показывает оба на `"/"` и `res.send` для тела ответа. `app.route("/book")` собирает get, post и put на одном пути.
- **Тело:** Нагрузке дают имя, например `Book`. Поля читают из этого типа в обработчике. Пропущенное поле — ошибка типа на том вызове, который вы контролируете. Express этот интерфейс за вас не придумывает.
- **Статус:** `res.send` пишет тело. Значение, которое вы отправляете, должно быть присваиваемо типу, который вы описали для этого маршрута.
- **Модули:** Типы и маршруты одного ресурса кладут в модуль и экспортируют роутер или функцию регистрации. Другие файлы импортируют тип, а не копию формы объекта.

**When to use:**

- Один модуль маршрутов на ресурс, когда путь и тип нагрузки должны лежать рядом.
- Общий тип `Book`, когда несколько методов возвращают одну и ту же форму.

**Examples:**

```ts
import express, { type Request, type Response } from "express";

interface Book {
  title: string;
}

const app = express();

app.route("/book")
  .get((_req: Request, res: Response) => {
    res.send("Get a random book");
  })
  .post((_req: Request, res: Response) => {
    const created: Book = { title: "Guide" };
    res.send(created.title);
  });
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>Объясните how to handle data modeling in a TypeScript backend.</summary>

### Объясните how to handle data modeling in a TypeScript backend.

**Definition:**

Запись моделируют интерфейсом или псевдонимом типа. Объединение — когда у поля фиксированный набор вариантов, `readonly` — когда вызывающим нельзя в него писать.

**How it works:**

- **Тип объекта:** `interface Book { title: string; pages?: number }` делает `title` обязательным, а `pages` необязательным. Проверка лишних свойств срабатывает, когда объектный литерал присваивают этому типу.
- **Варианты:** Поле вроде `status: "draft" | "published"` — объединение литералов. `switch` по нему сужает остаток размеченного объединения.
- **Граница:** Нетипизированный вход проверяют там, где он входит в процесс. После этой проверки остальной модуль видит тип `Book`. Сам тип JSON в рантайме не разбирает.
- **Общее:** Тип экспортируют из модуля, которому он принадлежит. Файл объявлений нужен только внешним JavaScript-вызывающим (`"declaration": true`).

**When to use:**

- Интерфейс — для объекта данных, который другие модули реализуют или расширяют.
- Псевдоним типа — для объединения или отображённой формы этого объекта.

**Examples:**

```ts
interface Book {
  readonly id: string;
  title: string;
  status: "draft" | "published";
}

function titleOf(book: Book): string {
  return book.status === "draft" ? `(draft) ${book.title}` : book.title;
}
```

**Sources:**

- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [TypeScript handbook — Object types](https://www.typescriptlang.org/docs/handbook/2/objects.html)

</details>

<details>
<summary>Как you manage dataBASE interactions in TypeScript?</summary>

### Как you manage dataBASE interactions in TypeScript?

**Definition:**

Типизируйте строку, которую возвращает запрос. Если клиент базы уже везёт `.d.ts` или пакет `@types`, импортируйте этот клиент. Тип строки остаётся вашим, когда драйвер возвращает нетипизированный результат.

**How it works:**

- **Типы клиента:** Пакет с `"types"` в `package.json` используют как есть. Пакету без типов ставят `npm install --save-dev @types/...` из DefinitelyTyped. Для одного пакета не ставят оба варианта.
- **Строка:** `interface UserRow { id: string; email: string }` — значение, которое возвращает функция, выполнившая запрос. Вызывающие видят `UserRow`, а не открытый объект.
- **Вход:** Параметры функции запроса типизированы (`id: string`). Это не заменяет параметризованный запрос в драйвере. Это не даёт передать неверное значение TypeScript.
- **Глобалы:** В TypeScript 6 пакет драйвера или Node добавляют в `types`, только если он приносит глобалы. Обычный импорт и так подтягивает типы этого модуля.

**When to use:**

- Вызов драйвера прячут в функцию, которая возвращает ваш тип строки. Остальное приложение драйвер не импортирует.
- Типы обновляют вместе с клиентом, чтобы имена методов совпали с той версией, которую вы запускаете.

**Examples:**

```ts
interface UserRow {
  id: string;
  email: string;
}

function toUser(row: UserRow): UserRow {
  return { id: row.id, email: row.email };
}
```

**Sources:**

- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>Обсудите authentication and authorization strategies in TypeScript APIs.</summary>

### Обсудите authentication and authorization strategies in TypeScript APIs.

**Definition:**

Вызывающего представляют размеченным объединением и ветвятся по этому типу до того, как обработчик отправит данные. TypeScript проверяет ветку. Запрос он не аутентифицирует.

**How it works:**

- **Вызывающий:** `type Caller = { kind: "anonymous" } | { kind: "user"; id: string; role: "reader" | "editor" }`. `kind` — дискриминант. `role` есть только у `"user"`.
- **Обработчик:** Обработчик Express по-прежнему получает `Request` и `Response`. После того как вы собрали `Caller`, `switch (caller.kind)` его сужает. Ветка `"anonymous"` может отказать через `res.send`. Ветка `"user"` может прочитать `caller.role`.
- **Авторизация:** `role` сравнивают с литералом, который требует маршрут. Забытая роль — ошибка типа, если допустимые роли маршрута — объединение и в `default` стоит `never`.
- **Край:** Объединение верно ровно настолько, насколько верна проверка, которая его построила. Разбор сессии или токена остаётся на границе. Тип подпись не проверяет.

**When to use:**

- Один тип `Caller` на все маршруты, чтобы обработчик не принял анонимный запрос за пользователя без ветки.
- На админском маршруте объединение ролей уже, чем на публичном.

**Examples:**

```ts
type Caller =
  | { kind: "anonymous" }
  | { kind: "user"; id: string; role: "reader" | "editor" };

function canEdit(caller: Caller): boolean {
  return caller.kind === "user" && caller.role === "editor";
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [Express — Routing](https://expressjs.com/en/guide/routing)

</details>

### TypeScript Advanced Practices

<details>
<summary>Обсудите using TypeScript for domain-driven design (DDD).</summary>

### Обсудите using TypeScript for domain-driven design (DDD).

**Definition:**

Предметную область кодируют типами: сущность — класс или тип объекта, вариант — размеченное объединение, репозиторий — интерфейс. TypeScript проверяет формы. Правила области в рантайме он не исполняет.

**How it works:**

- **Сущность:** Класс может спрятать поля через `private` и открыть методы. Вызывающие пользуются методами, а не сырыми полями.
- **Значение:** Небольшой тип объекта с полями `readonly` — это значение. Два значения сравнивают по этим полям. Тип отвергает присваивание в свойство `readonly`.
- **Варианты:** `OrderStatus` как `"draft" | "placed" | "shipped"` плюс `switch` и `default` с `never` значит, что новый статус не скомпилируется, пока каждый переход его не обработает.
- **Репозиторий:** `interface OrderRepository { find(id: string): Promise<Order> }` — это порт. Модуль базы его реализует. Модуль области импортирует интерфейс, а не драйвер.

**When to use:**

- Объединение и класс кладут в модуль, которому принадлежит понятие, и экспортируют только этот API.
- Внешний ввод разбирают в тип области на границе. Дальше функции принимают `Order`, а не `unknown`.

**Examples:**

```ts
type Order =
  | { status: "draft"; id: string }
  | { status: "placed"; id: string };

function place(order: Order): Order {
  if (order.status !== "draft") return order;
  return { status: "placed", id: order.id };
}
```

**Sources:**

- [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)
- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>Как you implement event sourcing in TypeScript?</summary>

### Как you implement event sourcing in TypeScript?

**Definition:**

То, что произошло, хранят как размеченное объединение событий и сворачивают этот список в текущее состояние. Имя каждого события — литерал, поэтому новое событие становится ошибкой типа, пока свёртка его не обработает.

**How it works:**

- **Событие:** `{ type: "Added"; amount: number } | { type: "Removed"; amount: number }`. `type` — дискриминант. `amount` у члена `Added` читают только в его ветке. Оба члена могут делить `amount`, если поле есть у обоих.
- **Свёртка:** `reduce` стартует с нуля и делает `switch` по `event.type`. В `default` событие присваивают `never`, чтобы пропущенная ветка ломала сборку.
- **Async:** Загрузка журнала — это `Promise<Event[]>`. Функция `async` его ждёт и потом сворачивает. Сама свёртка остаётся синхронной.
- **Хранение:** Массив — это журнал, который возвращает хранилище. TypeScript типизирует элементы. Запись строк — отдельный клиент базы.

**When to use:**

- Состояние выводят из истории, и проверка должна заметить новое событие.
- Одной записи текущего состояния хватает, когда история не нужна. Объединение событий для этого не требуется.

**Examples:**

```ts
type AccountEvent =
  | { type: "Added"; amount: number }
  | { type: "Removed"; amount: number };

function balance(events: readonly AccountEvent[]): number {
  return events.reduce((total, event) => {
    switch (event.type) {
      case "Added":
        return total + event.amount;
      case "Removed":
        return total - event.amount;
      default: {
        const _exhaustive: never = event;
        return _exhaustive;
      }
    }
  }, 0);
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)

</details>

<details>
<summary>Объясните the concept of Isomorphic apps with TypeScript.</summary>

### Объясните the concept of Isomorphic apps with TypeScript.

**Definition:**

Изоморфный модуль — это TypeScript, который выпускает JavaScript, и его можно импортировать и на сервере, и в браузере. Типы стираются, поэтому общий файл не должен звать API, которое есть только на одной из этих платформ.

**How it works:**

- **Выпуск:** `tsc` или пресет TypeScript у Babel пишет JavaScript. Аннотации типа в этом выходе нет. Один и тот же модуль `.js` могут загрузить и Node, и бандлер.
- **Резолвинг:** Код Node использует `module: "nodenext"`. Браузерный бандл — `moduleResolution: "bundler"`. Общий исходник остаётся `import`/`export`. У каждой стороны свой `tsconfig`, если цели выпуска различаются.
- **Разделение:** Типы DOM и глобалы Node (`process`, `fs`) в общий модуль не входят. Их передают из файла конкретной платформы. Общая функция принимает обычные типы данных.
- **Импорты только типов:** `import type` стирается, поэтому общий модуль может упомянуть тип с любой стороны и не выпустить импорт в рантайме. `verbatimModuleSyntax` держит это стирание честным.

**When to use:**

- Делите чистую функцию, тип или `switch` по размеченному объединению между API и UI.
- `page` или `fs` оставляйте в файле, который работает на этой платформе.

**Examples:**

```ts
export type Role = "reader" | "editor";

export function canEdit(role: Role): boolean {
  return role === "editor";
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html)

</details>

### TypeScript Types and ECMAScript Standards

<details>
<summary>Как TypeScript align with the latest ECMAScript standards?</summary>

### Как TypeScript align with the latest ECMAScript standards?

**Definition:**

TypeScript проверяет синтаксис JavaScript и выпускает JavaScript для того `target`, который вы задали. TypeScript 7 следует поведению командной строки TypeScript 6: `target` по умолчанию — текущая стабильная версия ECMAScript перед `esnext`.

**How it works:**

- **Target:** `tsc --init` в TypeScript 5.9 ставит `"target": "esnext"`. В анонсе TypeScript 7 сказано, что умолчание 7.0 — стабильная редакция сразу перед `esnext`, а `module` по умолчанию `esnext`.
- **Старые цели убраны:** TypeScript 7 больше не принимает `target: es5`. `moduleResolution` `node` / `node10` и `classic` исчезли. Анонс рекомендует `nodenext` или `bundler`.
- **Атрибуты импорта:** Ключевое слово `asserts` на импорте отвергается. Синтаксис — `with`, как у атрибутов импорта ECMAScript. То же правило есть в заметках TypeScript 5.8 про `nodenext`.
- **Новое поведение типов:** Вывод шаблонных литералов в TypeScript 7 сохраняет целую кодовую точку Unicode. `HeadTail<"😀abc">` — это `["😀", "abc"]`, как у `for...of` и `[...str]`, а не как у кодовых единиц UTF-16.

**When to use:**

- В новом приложении оставляйте `target` по умолчанию, если не нужно выпускать более старый синтаксис.
- Уходите с устаревших флагов ещё на TypeScript 6. TypeScript 7 считает их жёсткими ошибками. `"ignoreDeprecations": "6.0"` глушит их только в 6.

**Examples:**

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "nodenext",
    "strict": true
  }
}
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>Какие enhancements TypeScript принёс для соответствия ES6 и далее?</summary>

### Какие enhancements TypeScript принёс для соответствия ES6 и далее?

**Definition:**

TypeScript принимает синтаксис JavaScript той редакции, на которую вы целитесь, и добавляет типы, которые стираются. `async`/`await`, классы и `import`/`export` — это тот самый JavaScript, который проверяет компилятор.

**How it works:**

- **Async:** В TypeScript 1.6 появились функции `async` при target ES6 и глобальном `Promise`. У `await` на `Promise<T>` тип `T`. Тип возврата в этой настройке выводится как `Promise<...>`.
- **Модули:** Файл с `import` или `export` верхнего уровня — ES-модуль. `module: "nodenext"` выпускает `import` для файлов ESM и `require` для файлов CommonJS. TypeScript 7 больше не поддерживает значения `module` `amd`, `umd`, `system` и `none`.
- **Классы:** Поля, методы и конструкторы проверяются так, как описано в справочнике по классам. Выпуск следует `target`.
- **Типы сверху:** Объединения, обобщения и шаблонные литеральные типы — не возможности рантайма ECMAScript. Из JavaScript они исчезают. И пресет Babel, и `tsc` их выбрасывают. `.d.ts` выпускает только `tsc`.

**When to use:**

- Пишите возможность JavaScript напрямую (`async`, `import`) и ставьте тип на границе.
- `target` и `lib` задают API, которые вы вызываете. Более низкий `target` просит компилятор выпустить более старый синтаксис. Сам по себе он не добавляет недостающие полифилы рантайма.

**Examples:**

```ts
export async function readCount(load: () => Promise<number>): Promise<number> {
  return await load();
}
```

**Sources:**

- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Обсудите how TypeScript handles async iterators.</summary>

### Обсудите how TypeScript handles async iterators.

**Definition:**

`async function*` возвращает асинхронный итерируемый объект. `for await` обходит его и законен только внутри async-функции или async-генератора. Типы опираются на `Promise` и `Symbol.asyncIterator`.

**How it works:**

- **Генератор:** Пример TypeScript 2.3 выдаёт числа, делает `await` и `yield*` и в обычный массив, и в другой async-генератор. В рантайме нужны глобальный `Promise` и `Symbol.asyncIterator`.
- **Цикл:** `for await (const x of g())` стоит внутри `async function f`. Каждый `x` — выданное значение. В не-async функции такой цикл недействителен.
- **Интерфейс:** В заметках 2.3 `AsyncIterator<T>` описан с `next`, необязательными `return` и `throw`. Каждый возвращает `Promise<IteratorResult<T>>`.
- **Выпуск:** Синтаксис — это асинхронная итерация JavaScript. Типы выданных значений стираются. `target` должен быть редакцией, которая может представить запрошенный выпуск, либо компилятор понижает то, что умеет.

**When to use:**

- `async function*` — когда последовательность приходит со временем, например страницы результатов.
- Одна функция `async` — когда есть один `Promise`, а не поток значений.

**Examples:**

```ts
async function* count(): AsyncGenerator<number> {
  yield 1;
  yield 2;
}

async function log(): Promise<void> {
  for await (const value of count()) {
    value.toFixed(0);
  }
}
```

**Sources:**

- [TypeScript 2.3 — Async generators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

### TypeScript Performance Optimization

<details>
<summary>Как you optimize the performance of a TypeScript application?</summary>

### Как you optimize the performance of a TypeScript application?

**Definition:**

Отделяйте «проверка типов медленная» от «выпущенный JavaScript медленный». TypeScript 7 — нативный компилятор, на полной сборке обычно примерно в 8–12 раз быстрее. JavaScript, который вы отдаёте, по-прежнему определяет `target` и бандлер.

**How it works:**

- **Компилятор:** TypeScript 7 разбирает, проверяет и выпускает в несколько потоков. `--checkers` по умолчанию — 4 рабочих проверки типов. `--builders` — сколько сборок project reference идёт сразу под `--build`. `--singleThreaded` выключает параллелизм. `--watch` использует слежение за файлами, перенесённое из Parcel.
- **Проверка и выпуск:** `ts-loader` с `transpileOnly: true` пропускает проверку в сборке webpack. `fork-ts-checker-webpack-plugin` проверяет в другом процессе. Пресет Babel тоже пропускает проверку, поэтому `tsc` остаётся в конвейере.
- **Объявления:** `isolatedDeclarations` (TypeScript 5.5) сообщает, когда `.d.ts` файла нельзя выпустить из него одного. Сам выпуск это не меняет. Это условие для инструментов, которые хотят выпускать объявления параллельно.
- **Рантайм:** Типы стираются, поэтому работающую программу они не ускоряют и не замедляют. Цена в рантайме — это написанный вами JavaScript и `target`, в который вы выпускаете.

**When to use:**

- Ставьте TypeScript 7 (`npm install -D typescript`, затем `npx tsc`), когда доминирует время сборки и редактора. Сначала уберите устаревшие флаги TypeScript 6. TypeScript 7 их отвергает.
- Поднимайте `--checkers` на машине со свободными ядрами. Анонс показывает дополнительную скорость и большую память. На маленьком раннере CI значение понижают.

**Examples:**

```text
npx tsc --checkers 4 --noEmit
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [webpack — Build performance](https://webpack.js.org/guides/build-performance)

</details>

<details>
<summary>Обсудите tree SHAking in TypeScript.</summary>

### Обсудите tree SHAking in TypeScript.

**Definition:**

Tree shaking выбрасывает неиспользуемые экспорты JavaScript. TypeScript помогает тем, что стирает импорты только типов и выпускает синтаксис ES-модулей, который видит бандлер. Выбрасывает сам бандлер.

**How it works:**

- **Импорты типов:** `import type` стирается полностью. Под `verbatimModuleSyntax` импорт значения сохраняется, а модификатор `type` у имени выбрасывается. Пустой `import { type xyz }` становится `import {}` и всё ещё может быть импортом в рантайме, поэтому так не помечают связь только из типов.
- **Вид модуля:** Руководство webpack по TypeScript говорит не ставить `module` в `"CommonJS"`, иначе webpack не сможет сделать tree-shaking. В примере `"module": "esnext"`.
- **Импорты с побочным эффектом:** `verbatimModuleSyntax` сохраняет импорт без именованных привязок, если вы написали его как импорт значения. Так импорт ради побочного эффекта остаётся в выпуске.
- **Скорость проверки — отдельно:** `isolatedModules` делает каждый файл безопасным для одиночной транспиляции. Экспорты из бандла он не удаляет.

**When to use:**

- Помечайте имена только типов через `type`, чтобы они не остались в JavaScript живым импортом.
- Для браузерного бандла выпускайте ESM. `nodenext` — когда программа является пакетом Node и файлы грузит потребитель, а не webpack.

**Examples:**

```ts
import type { User } from "./user.js";
import { format } from "./format.js";

export function label(user: User): string {
  return format(user.name);
}
```

**Sources:**

- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html)
- [webpack — TypeScript](https://webpack.js.org/guides/typescript)

</details>

<details>
<summary>Объясните how to optimize bundle size with TypeScript.</summary>

### Объясните how to optimize bundle size with TypeScript.

**Definition:**

Держите типы вне JavaScript и не отдавайте файлы, которые браузер не исполняет. `import type` и `emitDeclarationOnly` — рычаги компилятора. Минификация и shaking бандла принадлежат бандлеру.

**How it works:**

- **Стирание:** Аннотация типа и `import type` в выпуск не попадают. Импорт класса или функции — импорт в рантайме, даже если вы ещё используете его как тип. Разделяйте их встроенным модификатором `type`.
- **Объявления:** `declaration` вместе с `emitDeclarationOnly` пишет `.d.ts` и не пишет `.js`, когда JavaScript уже выпустил Babel или webpack. Две копии программы не публикуют.
- **Карты:** `sourceMap` и `declarationMap` помогают отладке и Go to Definition. Руководство по выбору опций говорит, что они окупаются, когда вы также отдаёте исходники `.ts`, и они увеличивают размер пакета.
- **Формат:** Выпускайте ES-модули (`module` не `"CommonJS"`), чтобы webpack мог сделать tree-shaking. TypeScript 7 больше не поддерживает `amd`, `umd` и `system` как `module`.

**When to use:**

- Библиотека публикует `.js` плюс `.d.ts`, а не лишний `.js` от второго инструмента.
- Карты включают в пакете только когда потребителям нужно шагать в ваш TypeScript.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "moduleResolution": "bundler",
    "verbatimModuleSyntax": true,
    "declaration": true,
    "emitDeclarationOnly": true
  }
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript handbook — Choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

</details>

### TypeScript 4.x Features and Updates

<details>
<summary>Что такое the new features included in the latest TypeScript release?</summary>

### Что такое the new features included in the latest TypeScript release?

**Definition:**

TypeScript 7.0 — нативный порт компилятора, написанный так, чтобы проверка и выпуск остались совместимы с TypeScript 6.0. На полной сборке он обычно примерно в 8–12 раз быстрее. Стабильного программного API он не везёт. Это запланировано на 7.1.

**How it works:**

- **Скорость:** Разбор, проверка и выпуск идут параллельно. `--checkers` по умолчанию 4. `--builders` параллелит project reference под `--build`. `--singleThreaded` это выключает. `--watch` использует порт слежения за файлами из Parcel на Go. Редакторы говорят с ним по language server protocol. У VS Code есть расширение TypeScript 7.
- **Совместимость:** Код, чистый на TypeScript 6 с `stableTypeOrdering` и без `ignoreDeprecations`, должен совпасть. В умолчания TypeScript 7 входят `strict`, `module: esnext`, `types: []` и `rootDir: "./"`. Флаги, устаревшие в 6, включая `target: es5` и `moduleResolution: node`, — жёсткие ошибки.
- **Рядом:** В 7.0 нет API компилятора. `@typescript/typescript6` даёт `tsc6` и API 6.0 для инструментов вроде typescript-eslint. Ночные сборки переезжают на `typescript@next`. Старое имя превью — `@typescript/native-preview`.
- **Типы:** Вывод шаблонного литерала сохраняет целый скаляр Unicode. `HeadTail<"😀abc">` — это `["😀", "abc"]`. Проверка JavaScript ближе к файлам TypeScript: значение в позиции типа пишут как `typeof someValue`, и несколько старых форм JSDoc больше не особенные.

**When to use:**

- `npm install -D typescript` и `npx tsc` для бинарника 7.0 после того, как проект собирается на 6 без `ignoreDeprecations`.
- Оставляйте `"typescript": "npm:@typescript/typescript6@^6.0.2"`, когда инструмент всё ещё импортирует API 6, и добавьте `"@typescript/native": "npm:typescript@^7.0.2"` для `tsc` версии 7.

**Examples:**

```json
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>Как you use Template Literal Types?</summary>

### Как you use Template Literal Types?

**Definition:**

Шаблонный литеральный тип собирает строковый тип из кусков: `` `${K}Changed` ``. Объединения в кусках перемножаются. `Uppercase`, `Lowercase`, `Capitalize` и `Uncapitalize` меняют буквы строкового литерального типа.

**How it works:**

- **Имена событий:** Метод `on` из TypeScript 4.1 обобщён по `K extends string & keyof T`. Аргумент события — `` `${K}Changed` ``. Колбэк получает `T[K]`. `firstNameChanged` передаёт `string`. `ageChanged` передаёт `number`.
- **Переименование:** Отображённый тип может переименовать ключи через `as`. `` `get${Capitalize<string & K>}` `` превращает `name` в `getName`.
- **Регистр:** `Uppercase<"hello">` — это `"HELLO"`. Эти встроенные типы не учитывают локаль. Строки в рантайме по-прежнему используют методы строк JavaScript.
- **Вывод:** TypeScript 4.2 сам не выводит шаблонный литеральный тип из значения. `` `hello ${name}` as const `` сохраняет `` `hello ${string}` ``. Без `as const` тип — `string`.

**When to use:**

- Именуйте конечный набор строковых API, например `${Key}Changed`, когда набор ключей небольшой. В заметках 4.1 сказано, что большое объединение лучше сгенерировать заранее.
- `as const` — когда вычисленная строка должна остаться шаблонным литеральным типом.

**Examples:**

```ts
type PropEventSource<T> = {
  on<K extends string & keyof T>(
    eventName: `${K}Changed`,
    callback: (newValue: T[K]) => void,
  ): void;
};
```

**Sources:**

- [TypeScript 4.1 — Template literal types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html)
- [TypeScript 4.2 — as const on template values](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-2.html)

</details>

<details>
<summary>Обсудите the improvements to Inference in Conditional Types.</summary>

### Обсудите the improvements to Inference in Conditional Types.

**Definition:**

`infer` может назвать кусок типа и ограничить его через `extends`. TypeScript 4.8 разбирает совпавшую строку или элемент кортежа как литерал, который подошёл под ограничение, а не как широкий примитив.

**How it works:**

- **Ограничение:** `T extends [infer U extends number, ...unknown[]] ? U : never` берёт первый элемент кортежа, только если это число. Иначе тип — `never`.
- **Шаблонные строки:** `` "100" extends `${infer U extends number}` ? U : never `` — это литерал `100`, а не `number`. Тот же приём отображает `"100"` в `` 100n `` для `bigint` и `"true"` в `true` для `boolean`.
- **Unicode:** TypeScript 7 выводит одну кодовую точку. `` S extends `${infer Head}${infer Tail}` `` на `"😀abc"` — это `["😀", "abc"]`. Старые проверки раскалывали эмодзи на две единицы UTF-16.
- **Распределение:** Голый параметр типа в условном типе по-прежнему распределяется по объединению. Ограничение `extends` фильтрует каждый выведенный кусок. Значение в рантайме оно в этот литерал не превращает.

**When to use:**

- Ограничивайте `infer`, когда нужно отвергнуть кусок не того примитива и сохранить литерал, который совпал.
- Не ставьте ограничение, когда годится любой кусок этой позиции.

**Examples:**

```ts
type SomeNum = "100" extends `${infer U extends number}` ? U : never;
```

**Sources:**

- [TypeScript 4.8 — infer constraints](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-8.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Что такое the use of labeled tuple elements?</summary>

### Что такое the use of labeled tuple elements?

**Definition:**

Метка называет позицию кортежа для читателя и для инструментов: `[start: number, end: number]`. На присваиваемость метка не влияет. `[number, number]` — по-прежнему тот же тип.

**How it works:**

- **С 4.0:** Метки нужны, чтобы кортеж читался как список параметров. От неподписанного кортежа с теми же типами элементов они при проверке не отличаются.
- **С 5.2:** В кортеже можно смешивать подписанные и неподписанные элементы. Раньше метка была у каждого элемента или ни у одного. Раскрытие подписанного кортежа в неподписанный раньше теряло метки. TypeScript 5.2 их сохраняет.
- **API:** `TupleType.labeledElementDeclarations` может быть `undefined` на неподписанном индексе, чтобы можно было представить смешанный кортеж.
- **Rest:** Больше не нужна фиктивная метка вроде `rest` на rest-элементе только ради старого правила «все или ничего».

**When to use:**

- Подписывайте кортеж, который входит в публичный тип функции, чтобы на месте вызова были видны `start` и `end`.
- Не подписывайте кортеж, который только короткая внутренняя пара и имена ничего не добавят.

**Examples:**

```ts
type Range = [start: number, end: number];

function span(range: Range): number {
  const [start, end] = range;
  return end - start;
}
```

**Sources:**

- [TypeScript 4.0 — Labeled tuple elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-0.html)
- [TypeScript 5.2 — Named and anonymous tuple elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html)

</details>

### TypeScript Community and Development

<details>
<summary>Как you contribute to the TypeScript community or TypeScript itself?</summary>

### Как you contribute to the TypeScript community or TypeScript itself?

**Definition:**

Типы публикуют вместе со своим пакетом или отправляют в DefinitelyTyped, если они не генерируются из ваших исходников. Сам компилятор разрабатывают открыто. Ночные сборки — это `typescript@next`.

**How it works:**

- **Своя библиотека:** `"declaration": true` и поле `"types"` отдают `.d.ts` рядом с JavaScript. Значок TypeScript на npm появляется только когда `"types"` задан.
- **Чужая библиотека:** Справочник по публикации говорит отправлять такие объявления в DefinitelyTyped. Их публикуют как `@types/...`. Второй пакет `@types` перестают отдавать, когда у библиотеки появляется свой `"types"`.
- **Компилятор:** TypeScript 7 — нативный компилятор и остаётся совместим с проверкой 6.0. Программного API нет до 7.1. Инструменты, которые импортируют `typescript`, продолжают использовать `@typescript/typescript6` (`tsc6`).
- **Проба изменений:** `npm install -D typescript@next` ставит ночную сборку. В анонсе 7.0 старое имя превью — `@typescript/native-preview`.

**When to use:**

- Добавьте `"types"` в следующий выпуск библиотеки, которую вы ведёте.
- Откройте изменение в DefinitelyTyped, когда поправили неверное объявление зависимости, которой вы не управляете.

**Examples:**

```text
npm install -D typescript@next
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Обсудите the role of TypeScript in open-source development.</summary>

### Обсудите the role of TypeScript in open-source development.

**Definition:**

Открытые библиотеки либо везут `.d.ts`, либо опираются на DefinitelyTyped. Компилятор — общая проверка, которую вызывают редакторы и инструменты. TypeScript 7.0 ускоряет эту проверку и до 7.1 оставляет API 6.0 в отдельном пакете.

**How it works:**

- **Пакеты:** `"types"` указывает на вход объявлений. Потребители импортируют пакет и получают проверку без дополнительной установки. `@types` закрывает пакеты, которые это поле так и не добавили.
- **Инструменты:** Правилам typescript-eslint с типами нужна программа TypeScript. До 7.1 они импортируют API 6.0 из `@typescript/typescript6`, а `tsc` может быть TypeScript 7.
- **Приложения:** Angular, Vue, Express, Jest и Playwright документируют вход на TypeScript: классы компонентов, `<script lang="ts">`, `Request`/`Response` и `import { test } from "@playwright/test"`.
- **Проверка JavaScript:** `allowJs` позволяет проекту читать `.js`. `// @ts-check` в начале JavaScript-файла просит редактор его проверить. TypeScript 7 сужает, какие формы JSDoc он ещё считает типами.

**When to use:**

- Объявления выпускают из исходников, которые их генерируют, чтобы DefinitelyTyped не был вторым источником правды.
- Пакет API 6 фиксируют только для инструментов, которые его ещё импортируют. Повседневный `tsc` может быть 7.

**Examples:**

```json
{
  "name": "awesome",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Объясните how to stay updated with TypeScript's changes and best practices.</summary>

### Объясните how to stay updated with TypeScript's changes and best practices.

**Definition:**

Перед обновлением читайте заметки к выпуску и снимите устаревшие флаги на TypeScript 6, прежде чем переходить на 7. Ночные сборки — `npm install -D typescript@next`.

**How it works:**

- **Стабильные заметки:** Страница справочника 6.0 — переходный выпуск. `"ignoreDeprecations": "6.0"` прячет там предупреждения. TypeScript 7 эти опции удаляет, включая `target: es5` и `moduleResolution: node`.
- **7.0:** Анонс описывает нативный компилятор, `--checkers`, отсутствующее программное API и новые умолчания (`strict`, `types: []`, `rootDir: "./"`). Поддержка редакторов — language server protocol. VS Code пользуется расширением TypeScript 7, пока редактор не включит его сам.
- **Ночная сборка:** `typescript@next` заменяет `@typescript/native-preview` для сборок после 7.0.
- **База проекта:** `tsc --init` в 5.9 уже включает `strict`, `verbatimModuleSyntax` и `isolatedModules`. В новом коде их оставляют. Не копируйте конфиг, который выключает `strict`.

**When to use:**

- Обновляйтесь по одному мажору: 6 без `ignoreDeprecations`, затем 7.
- Просматривайте заметки, когда сборка падает после установки. Часто это удалённый флаг, а не изменившийся тип в вашем коде.

**Examples:**

```text
npm install -D typescript@next
npx tsc --noEmit
```

**Sources:**

- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

### Transitioning from JavaScript to TypeScript

<details>
<summary>Какие шаги JavaScript developer должен предпринять для эффективного использования TypeScript?</summary>

### Какие шаги JavaScript developer должен предпринять для эффективного использования TypeScript?

**Definition:**

Поставьте компилятор, добавьте `tsconfig.json` со `strict` и оставьте существующий JavaScript в программе через `allowJs`, пока типизируете границы.

**How it works:**

- **Установка:** `npm install -D typescript` даёт `tsc` TypeScript 7. Запускают его через `npx tsc`. `tsc --init` в 5.9 пишет `strict`, `module: nodenext`, `target: esnext`, `verbatimModuleSyntax` и `isolatedModules`.
- **Старые файлы:** `"allowJs": true` заставляет TypeScript читать `.js`. `"declaration": true` вместе с `"emitDeclarationOnly": true` может написать из них `.d.ts`. `// @ts-check` в одном JavaScript-файле включает проверку в редакторе.
- **Глобалы:** В TypeScript 6 и 7 `types` по умолчанию `[]`. Ставьте `@types/node` и добавляйте `"node"`, когда пользуетесь глобалами Node. `@types` ставят только пакетам, которые не публикуют `"types"`.
- **Первые типы:** Подпишите параметры функций, которые были неявным `any`. `noImplicitAny` входит в `strict`. Вывод оставляют у локальных переменных, у которых инициализатор уже имеет тип.

**When to use:**

- Переименовывайте файл в `.ts`, когда готовы чинить его ошибки. Остальное держите `.js`, пока не готовы.
- Включите в редакторе версию TypeScript рабочей области, чтобы подчёркивания совпали с `npx tsc`.

**Examples:**

```json
{
  "compilerOptions": {
    "allowJs": true,
    "strict": true,
    "outDir": "dist",
    "types": ["node"]
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript handbook — Creating .d.ts from .js](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

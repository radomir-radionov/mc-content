# JS — Interview

Вопросы для собеседования по JavaScript. Ответы заполнены для всего списка.

[← Назад в JS-хаб](./3_js.md)

### Related docs

- [Язык](./3_js_core.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массивы](./3_js_array.md) · [Функции](./3_js_functions.md) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md)
- [Документ](./3_js_browser.md) · [События](./3_js_events.md) · [API браузера](./3_js_browser_extra.md) · [Async](./3_js_async.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы из [Devinterview.io — JavaScript](https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/).

### JavaScript Fundamentals

<details>
<summary>Что такое the data types present in JavaScript?</summary>

### Что такое the data types present in JavaScript?

**Definition:**

JavaScript имеет примитивные и составные типы данных.

**How it works:**

#### Примитивные типы данных

- **Boolean**: представляет логические значения true или false.

- **Null**: обозначает отсутствие значения.

- **Undefined**: указывает на переменную, которая была объявлена, но ей не было присвоено значение.

- **Number**: представляет числовые значения, включая целые числа и числа с плавающей запятой.

- **BigInt**: позволяет представлять целые числа с произвольной точностью.

- **String**: инкапсулирует последовательности символов.

- **Symbol (ES6)**: предоставляет уникальное неизменяемое значение.

#### Составные типы данных

- **Object**: представляет собой набор пар ключ-значение и используется для более сложных структур данных.

- **Function**: вызываемый объект, который можно определить с использованием обычного синтаксиса функции или с помощью нового конструктора Function() (используется редко).

#### Примечательные характеристики

JavaScript имеет динамическую типизацию, что означает, что тип данных переменной может меняться во время выполнения программы.
Может возникнуть принуждение типов данных, когда значения неявно преобразуются из одного типа в другой в определенных контекстах, например во время сравнения.
Арифметические операции, особенно когда один из операндов является строкой, могут привести к неявному преобразованию типов.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое the difference between null and undefined?</summary>

### Что такое the difference between null and undefined?

**Definition:**

Хотя null и undefined не представляют никакой ценности в JavaScript, они различаются по своей роли и происхождению.

Происхождение и контекст null обычно обозначает намеренно отсутствующее значение, и разработчики могут установить переменную null, чтобы обозначить отсутствие объекта или значения. Например, если вызов API не возвращает данные, вы можете установить для переменной значение null.

undefined обычно указывает на переменную, которая была объявлена, но которой еще не присвоено значение, или на свойство, которого не существует в объекте.

**How it works:**

#### Аргументы функции

Если вызывается функция, а параметр не указан или его значение не установлено, параметр не определен.
Вместо этого null будет явным значением, предоставленным в качестве аргумента.

#### Свойства объекта

Если вы попытаетесь получить доступ к свойству несуществующего объекта, результат будет неопределенным.

Null можно использовать для очистки значения свойства в объекте, который был установлен ранее.

obj.prop = null;

#### Операция равенства

В JavaScript, undefined и null рассматриваются как равные при использовании приблизительного равенства (==), но не строгого равенства (===).

**When to use:**

#### Варианты использования и лучшие практики

Когда вы инициализируете переменную и не готовы присвоить значимое значение, чаще всего используется undefined вместо null, чтобы указать, что значения еще нет.
Например, если вы объявляете объект пользователя, но еще не имеете его сведений, вы можете оставить его неопределенным.

**Examples:**

#### Инициализация и присвоение переменных

Переменные, которым не присвоено значение, по умолчанию имеют значение undefined, если только явно не установлено значение NULL.

```js
let foo; // undefined
let bar = null; // null
```

```js
let obj = {};
console.log(obj.nonExistentProperty); // undefined
```

```js
let obj = { prop: 'value' };
```

#### Пример кода

Вот код JavaScript:

```js
let var1;
let var2 = null;

let object = {
  a: 1,
  b: undefined
};

function test(arg1, arg2) {
  console.log(arg1);  // undefined: not provided
  console.log(arg2);  // null: provided as such
}

function clearProperty(prop) {
  delete object[prop];
}

console.log(var1);     // undefined
console.log(var2);     // null
console.log(object.a); // 1
console.log(object.b); // undefined
console.log(object.c); // undefined
```

тест();               // Оба аргумента undefined test(1, null);        // arg1 равен 1, arg2 равен null

ClearProperty('b');  // Удаляет свойство «b» из объекта

```js
console.log(object.b); // undefined: Property 'b' was removed, not set to null
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Как JavaScript handle type coercion?</summary>

### Как JavaScript handle type coercion?

**Definition:**

Приведение типов в JavaScript относится к автоматическому преобразованию значений из одного типа данных в другой.

**How it works:**

#### Явное и неявное приведение

- **Явный**: достигается с помощью таких методов, как parseInt(), Number() и toString().

- **Неявное**: автоматически возникает во время операций или сравнений. Например, объединение строки и числа в сложении приводит к автоматическому преобразованию числа в строку.

#### Распространенные сценарии принуждения

- **Арифметические операции**: строки преобразуются в числа.

- **Пример**: «5» — 3 оценивается как 2, поскольку строка преобразуется в число.

- **Свободное равенство (==)**: типы данных часто изменяются для сравнения.

- **Пример**: «4» == 4 истинно из-за приведения строк перед сравнением.

- **Условные операторы (если и тернарные операторы)**: определяется истинность или ложность.

- **Пример**: if(1) оценивается как true, потому что 1 приводит к true.

- **Логические операторы**: нелогические значения приводятся к логическим значениям.

- **Пример**: «привет» && 0 оценивается как 0, потому что истинное «привет» закорачивает операцию &&, а 0 приводит к ложному значению.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Объясните the concept of hoisting in JavaScript.</summary>

### Объясните the concept of hoisting in JavaScript.

**Definition:**

Подъем — это механизм JavaScript, который включает перемещение объявлений переменных и функций в начало области их содержания на этапе компиляции. Однако присвоения этим переменным или определения функций остаются неизменными.

Например, хотя вызов myFunction появляется перед ее определением, hoisting гарантирует, что он не вызовет ошибку.

**How it works:**

#### Подъем в действии

#### Почему подъем важен

Понимание hoisting может помочь вам предотвратить некоторые непредвиденные ситуации в вашем коде. Например, он может пролить свет на неожиданные значения «undefined», которые могут появиться даже после объявления и инициализации переменной.

#### Локальная область действия и подъем

Переменные и функции, объявленные в локальных областях внутри функций, также поднимаются в начало своей области.

#### Лучшие практики

Чтобы написать чистый, читаемый код, важно:

Объявляйте переменные в верхней части ваших скриптов или функций, чтобы избежать ошибок, связанных с hoisting.
Инициализируйте переменные перед использованием, независимо от hoisting, чтобы обеспечить предсказуемое поведение.

#### ES6 и подъем

С появлением let и const в ES6 поведение JavaScript адаптировалось. Переменные, объявленные с помощью let и const, по-прежнему поднимаются, но в отличие от var они не инициализируются.

**Examples:**

#### Вот пример кода

```js
console.log(myVar); // Undefined
var myVar = 5;

console.log(myVar); // 5

// Приведенный выше код на этапе компиляции эквивалентен следующему:
// var myVar;
// console.log(myVar);
// myVar = 5;

console.log(sayHello()); // "Hello, World!"
function sayHello() {
    return "Hello, World!";
}

// Приведенный выше код на этапе компиляции эквивалентен следующему:
// function sayHello() {
//     return "Hello, World!";
// }
// console.log(sayHello());
```

#### Глобальная область видимости и подъем

В глобальной области переменные, объявленные с помощью var, и функции всегда поднимаются наверх. Например:

```js
// На этапе компиляции поднимаются следующие глобальные объявления:
// var globalVar;
// function globalFunction() {}

console.log(globalVar); // Undefined
console.log(globalFunction()); // "Hello, Global!"
var globalVar = "I am global Var!";
function globalFunction() {
    return "Hello, Global!";
}
```

#### Вот пример кода

```js
function hoistingInLocalScope() {
    // Эти локальные объявления поднимаются на этапе компиляции:
    // var localVar;
    // function localFunction() {}

    console.log(localVar); // Undefined
    localVar = "I am a local var!";
    console.log(localFunction()); // "Hello, Local!"

    var localVar;
    function localFunction() {
        return "Hello, Local!";
    }
}
```

#### Вот пример

```js
console.log(myLetVar); // ReferenceError: Cannot access 'myLetVar' before initialization
let myLetVar = 5;
```

Константы и подъем const и let ведут себя одинаково при подъеме, но их разница заключается в том, что const должно быть присвоено значение во время объявления, тогда как let не требует начального значения.

#### Вот пример

```js
console.log(myConstVar); // ReferenceError: Cannot access 'myConstVar' before initialization
const myConstVar = 10;

console.log(myLetVar); // Undefined
let myLetVar = 5;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое the scope in JavaScript?</summary>

### Что такое the scope in JavaScript?

**Definition:**

Область определяет доступность и время жизни переменных в программе. В JavaScript существует два основных типа: глобальная область и локальная область.

**How it works:**

#### Глобальная область действия

Любая переменная, объявленная вне функции, находится в глобальной области видимости. Доступ к ним можно получить как из функций, так и из других тегов скрипта.

#### Локальная область действия

Переменные, объявленные внутри функции (с использованием let или const или до JavaScript ES6 с var), имеют локальную область действия, то есть они доступны только внутри этой функции.

#### Область действия блока

Начиная с ES6, JavaScript также поддерживает область действия блока, где переменные, определенные внутри блоков кода (обозначаемые {}, например циклы или условные операторы), с использованием let или const, доступны только внутри этого блока.

**Examples:**

#### Пример: глобальная область действия

Вот код JavaScript:

```js
let globalVar = 'I am global';

function testScope() {
    console.log(globalVar); // Output: 'I am global'
}

testScope();
console.log(globalVar); // Output: 'I am global'
```

#### Пример: локальная область действия

Вот код JavaScript:

```js
function testScope() {
    let localVar = 'I am local';
    console.log(localVar); // Output: 'I am local'
}

// Этот оператор выдаст ошибку, поскольку localVar не определен вне области действия функции.
// console.log(localVar);
```

#### Пример: область действия блока

Вот код JavaScript:

```js
function testScope() {
    let localVar = 'I am local';
    if (true) {
        let blockVar = 'I am local to this block';
        console.log(localVar, blockVar); // Both will be accessible
    }
    // Этот оператор выдаст ошибку, поскольку blockVar не определен вне области действия блока.
    // console.log(blockVar);
}

testScope();
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое the difference between == and ===?</summary>

### Что такое the difference between == and ===?

**Definition:**

Строгое равенство (===) в JavaScript требует совпадения значения и типа, проверки более конкретных условий и снижения вероятности неожиданных результатов.

Напротив, абстрактное сравнение равенства (==) может привести к принуждению типов, что потенциально может привести к нелогичным результатам.

Хотя оба режима сравнения проверяют равенство значений, === обеспечивает дополнительное совпадение типов данных.

**How it works:**

#### Абстрактное равенство

5 == '5' оценивается как true, поскольку JavaScript преобразует строку в число для сравнения.

#### Строгое равенство

5 === '5' оценивается как ложь, поскольку типы не совпадают.

#### Ключевые моменты

- **Типовая безопасность**: === безопаснее, поскольку позволяет избежать нежелательных преобразований типов.

- **Производительность**: === может быть быстрее, особенно для простых сравнений, поскольку не требует приведения типов или дополнительных проверок.

- **Ясность**. Использование === может сделать ваш код более понятным и предсказуемым.

#### Общие рекомендации

- **Использовать строгое равенство по умолчанию**: этот подход сводит к минимуму непредвиденные побочные эффекты.

- **Внимательно относитесь к приведению типов**: в особых случаях или при подтвержденном понимании == может подойти, но будьте осторожны, чтобы не запутаться.

**Examples:**

#### Наглядный пример: абстрактное и строгое равенство

#### Пример кода: операторы равенства

Вот код JavaScript:

```js
// Abstract equality
console.log('5' == 5);      // true
console.log(null == undefined);  // true
console.log(0 == false);    // true

// Strict equality
console.log('5' === 5);     // false
console.log(null === undefined); // false
console.log(0 === false);   // false
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Опишите closure in JavaScript. Can you give an example?</summary>

### Опишите closure in JavaScript. Can you give an example?

**Definition:**

В JavaScript замыкания позволяют функции получить доступ к своей внешней области, сохраняя доступ this даже после завершения выполнения родительской функции. Этот механизм предоставляет мощный инструмент для инкапсуляции данных и обеспечения конфиденциальности.

**How it works:**

#### Основная концепция

Когда функция определена внутри другой функции, она сохраняет ссылку на переменные из внешней функции даже после того, как внешняя функция завершила выполнение и ее локальные переменные обычно больше не доступны.

#### Ключевые компоненты

- **Внешняя функция (Родительская функция)**: содержит внутренние функции или замыкания.

- **Внутренняя функция (замыкание)**: определенная внутри родительской функции, она ссылается на переменные из внешней функции.

- **Лексическая среда**: контекст, в котором определена внутренняя функция, инкапсулирующий область, к которой она имеет доступ.

#### Ловушки, которых следует избегать

- **Утечка памяти**: при неосторожном использовании замыкания могут вызвать утечки памяти, поскольку переменные внешней функции продолжают жить в памяти из-за замыкающей ссылки.

- **Устаревшие данные**: помните об общих переменных, которые могут измениться после определения замыкания, что приведет к неожиданному поведению.

#### Совместимость браузера

Концепция замыканий является фундаментальным аспектом языка JavaScript и поддерживается всеми современными браузерами и средами.

**When to use:**

#### Приложение

- **Конфиденциальность данных**: шаблоны проектирования JavaScript, такие как шаблоны модулей и раскрывающихся модулей, используют замыкания для обеспечения конфиденциальности данных.

- **Тайм-ауты и обработчики событий**: замыкания помогают сохранить окружающий контекст в асинхронных операциях, таких как setTimeout и обработчики событий.

**Examples:**

#### Пример: генератор паролей

Рассмотрим простой сценарий функции, отвечающей за генерацию секретного пароля:

Внешняя функция генерировать пароль определяет локальную переменную пароль и возвращает внутреннюю функцию getPassword.
Внутренняя функция getPassword имеет монопольный доступ к переменной пароля даже после выполнения метода GeneratePassword.
Вот код JavaScript:

```js
function generatePassword() {
  let password = '';
  const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  const passwordLength = 8;
  for(let i = 0; i < passwordLength; i++) {
    password += characters.charAt(Math.floor(Math.random() * characters.length));
  }

  return function getPassword() {
      return password;
  };
}

const getPassword = generatePassword();

console.log(getPassword()); // Outputs the generated password.
```

В примере this getPassword по-прежнему имеет доступ к переменной пароля после завершения функцииgeneratePassword благодаря механизму закрытия.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое the 'this' keyword and how does its context change?</summary>

### Что такое the 'this' keyword and how does its context change?

**Definition:**

В JavaScript контекст this относится к контексту выполнения, обычно это объект, которому принадлежит функция, в которой используется this.

'this' в глобальной области видимости
В нестрогом режиме this в глобальной области ссылается на объект окна. В строгом режиме this не определен.

'this' в функциях
В функциях без стрелок значение this зависит от того, как функция вызывается. При вызове:

- **Как метод объекта**: this — это объект.

- **Alone**: в браузере this является оконным или глобальным в Node.js. В строгом режиме он не определен.

- **При вызове, применении или привязке**: this задан явно.

- **Как конструктор (с новым)**: this — это вновь созданный объект.

'this' в стрелочных функциях
Стрелочные функции имеют фиксированный контекст для this, определенный при создании функции, и не изменяется в зависимости от способа их вызова.

У них нет своего этого.
Они используют this из окружающего их лексического контекста (охватывающая функция или глобальный контекст).

**Examples:**

#### Code Example: Global Context

Вот код JavaScript:

```js
// Main
let globalVar = 10;

function globalFunction() {
    console.log('Global this: ', this.globalVar);
    console.log('Global this in strict mode: ', this);
}
```

- **глобальная функция();  // Вывод**: 10, окно или undefined (в строгом режиме)

// В Node.js все будет иначе, потому что "окно" не определено. Но "this" будет относиться к глобальному объекту.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое arrow functions and how do they differ from regular functions?</summary>

### Что такое arrow functions and how do they differ from regular functions?

**Definition:**

Давайте посмотрим на ключевые особенности стрелочных функций и чем они отличаются от традиционных функций в JavaScript.

**How it works:**

#### Функции стрелок: ключевые особенности

#### Краткий синтаксис

Особенно полезно для коротких однострочных функций.
Нет необходимости использовать ключевое слово функции или фигурные скобки, если есть одно выражение.

#### Неявный возврат

Если нет явного оператора { return ... ;}, стрелочные функции возвращают результат одного выражения внутри.

#### this Привязка

Не имеет своего этого. Оно «унаследовано» от окружающего (лексического) контекста. Эта функция известна как «лексическая область видимости».

**When to use:**

#### Когда использовать функции стрелок

- **Обработчики событий**: идеально подходят для краткой, встроенной обработки событий, где контекст this может быть унаследован из лексической области видимости.

- **Функции обратного вызова**: полезны для таких методов работы с массивами, как сопоставление, фильтрация и сокращение.

- **Избежание переопределения this**: если вы хотите сохранить окружающий контекст this и избежать непреднамеренного переопределения.

**Examples:**

#### Пример кода: стандартная функция и стрелочная функция

Вот код JavaScript:

```js
// Standard Function
function greet(name) {
  return "Hello, " + name + "!";
}

// Arrow Function
const greetArrow = name => "Hello, " + name + "!";
```

В приведенном выше коде Greeting — это стандартная функция, а GreetingArrow — это стрелочная функция, демонстрирующая разницу в синтаксисе и необходимых ключевых словах.

#### Пример кода: функция стрелки и контекст this

Вот код JavaScript:

```js
// Using traditional functions
document.getElementById('myButton').onclick = function() {
  console.log('Button clicked:', this);  // Refers to the button element
};

// Using arrow functions
document.getElementById('myButton').onclick = () => {
  console.log('Button clicked:', this);  // Refers to the global/window object
};
```

В примере с функцией стрелки контекст this относится не к элементу кнопки, а к объекту глобального окна, поскольку функции стрелки не имеют собственной привязки к этому элементу. Вместо этого они наследуют this от своей лексической области, которая в случае this является глобальным контекстом.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое template literals in JavaScript?</summary>

### Что такое template literals in JavaScript?

**Definition:**

Литералы шаблонов — это функция современных версий JavaScript, которая предлагает более гибкий и читаемый способ работы со строками. Их часто называют «строками шаблона».

**How it works:**

#### Ключевые особенности

- **Многострочный текст**: литералы шаблона поддерживают многострочные строки, не требуя escape-символов или объединения строк со знаком +.

- **Интерполяция строк**: они обеспечивают плавное встраивание выражений JavaScript в строки с помощью ${}.

#### Синтаксис

- **Одинарные и двойные кавычки**: для литералов шаблона используйте обратные кавычки (`) вместо одинарных ('') или двойных кавычек ("").

- **Заполнитель**: заполнитель ${expression} в обратных кавычках позволяет вводить переменные и выражения.

#### Преимущества

- **Читабельность**: они могут сделать код более понятным, особенно при работе с более длинными или сложными строками, сохраняя содержание ближе к его замыслу.

- **Интерполяция и выражение**: литералы шаблона уменьшают многословие и логику рендеринга при интеграции динамических данных.

#### Проблемы совместимости браузера

Литералы шаблонов повсеместно поддерживаются в современных браузерах и теперь считаются основной функцией JavaScript. Однако они могут не работать в старых браузерах, таких как Internet Explorer, без транспиляции или полизаполнения.

**Examples:**

#### Пример

```js
let name = "John";
let message = `Hi ${name}!`;

console.log(message);  // Output: "Hi John!"
```

#### Пример кода: интерполяция многострочного текста и строк

```js
// Regular String
let poem = "Roses are red,\nViolets are blue,\nSugar is sweet,\nAnd so are you.";

// Template Literal
let poemTemplate = `
  Roses are red,
  Violets are blue,
  Sugar is sweet,
  And so are you.
`;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

### JavaScript Functions and Higher-Order Functions

<details>
<summary>Что такое higher-order function in JavaScript?</summary>

### Что такое higher-order function in JavaScript?

**Definition:**

Функция высшего порядка в JavaScript — это функция, которая может принимать другие функции в качестве аргументов или возвращать функции. Эта функция позволяет использовать парадигмы функционального программирования, такие как отображение, сокращение и фильтрация. Функции высшего порядка обеспечивают универсальность и модульность, обеспечивая оптимизированный и эффективный код.

**How it works:**

#### Ключевые характеристики

- **Функции первого класса**: функции в JavaScript считаются первоклассными, что означает, что они являются допустимым типом данных и могут обрабатываться как любое другое значение, в том числе присваиваться переменным, храниться в структурах данных или возвращаться из других функций.

- **Поддержка замыканий**: благодаря замыканиям функция более высокого порядка может транспортировать не только данные, вложенные в определение функции, но и лексическую среду, в которой находятся эти данные.

- **Динамический код**: поскольку JavaScript позволяет динамически создавать и именовать функции, их можно динамически передавать функциям более высокого порядка.

**When to use:**

#### Практическое применение

- **Выполнение обратного вызова**: такие функции, как setTimeout и addEventListener, принимают функцию в качестве аргумента и, следовательно, имеют более высокий порядок.

- **Обработка событий**. Многие системы, управляемые событиями, используют функции более высокого порядка для таких задач, как подписка на события и их распространение.

- **Итеративные операции**: функции карты, фильтра и сокращения в JavaScript работают с массивами и требуют передачи функций, что делает их более высоким порядком.

- **Абстракция кода**: функции высшего порядка позволяют инкапсулировать повторяющиеся задачи, создавая более чистый и читаемый код.

**Examples:**

#### Пример кода: функции высшего порядка

Вот код JavaScript:

```js
// Simple higher-order function
function multiplier(factor) {
  return function(num) {
    return num * factor;
  };
}

// Invoke a higher-order function
const twice = multiplier(2);
console.log(twice(5));  // Output: 10

// Functional programming with higher-order functions
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(multiplier(2));  // [2, 4, 6, 8, 10]
const tripled = numbers.map(multiplier(3));  // [3, 6, 9, 12, 15]
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Можно ли functions be assigned as values to variables in JavaScript?</summary>

### Можно ли functions be assigned as values to variables in JavaScript?

**Definition:**

Да, JavaScript поддерживает функции первого класса. Это означает, что функции можно рассматривать как переменные, а затем присваивать их другим переменным или передавать в качестве аргументов другим функциям.

Функции, определенные как обычные функции или функции стрелок, являются первоклассными в JavaScript.

**When to use:**

#### Практические примеры использования

- **Обратные вызовы**: функции можно передавать в качестве параметров другим функциям.

- **Обработка событий**. В веб-разработке функции определяют, как реагировать на определенные события, и эти функции часто прикрепляются к прослушивателям событий.

- **Модульная разработка**. В шаблонах программирования, таких как шаблон «Модуль», функции определяются в пределах области видимости, а затем возвращаются, подобно переменным.

- **Функции высшего порядка**: эти функции работают с другими функциями, принимая их в качестве аргументов или возвращая, и являются неотъемлемой частью многих современных JavaScript библиотек и фреймворков.

**Examples:**

#### Практический пример кода

Вот код JavaScript:

```js
// Define a function
function greet() {
  console.log('Hello!');
}

// Assign the function to a variable
let sayHello = greet;

// Call the function through the variable
```

сказатьПривет();  // Вывод: «Привет!»

```js
// Reassign the variable to a new function
sayHello = function() {
  console.log('Bonjour!');
};

// Call it again to see the new behavior
```

сказатьПривет();  // Вывод: «Добрый день!»

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Как functional programming concepts apply in JavaScript?</summary>

### Как functional programming concepts apply in JavaScript?

**Definition:**

Концепции функционального программирования (FP) в JavaScript являются прямым результатом первоклассных функций языка. Ключевые принципы ФП, такие как неизменность, чистые функции и декларативный стиль, играют решающую роль.

**How it works:**

#### Основные понятия

#### Функции первого класса и функции высшего порядка

JavaScript рассматривает функции как первоклассных элементов, позволяя назначать их переменным, передавать в качестве параметров и возвращать из других функций. Эта функция является основополагающей для FP в языке.

**Examples:**

#### Пример кода

Вот код JavaScript:

```js
const sayHello = () => console.log('Hello!');
const runFunction = (func) => func();
```

runFunction (сказать Привет);  // Вывод: «Привет!»

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Что такое IIFEs (Immediately Invoked Function Expressions)?</summary>

### Что такое IIFEs (Immediately Invoked Function Expressions)?

**Definition:**

Шаблон проектирования «Выражение немедленно вызываемой функции» (IIFE) использует анонимную функцию, которая выполняется сразу после ее определения.

Ключевые характеристики IIFEs включают локализованные области видимости переменных и немедленную активацию после анализа интерпретатора.

**How it works:**

#### Основные функции IIFE

- **Инкапсуляция**: посредством лексической области IIFEs защищает переменные от утечки в глобальную область видимости. Это, в свою очередь, предотвращает непреднамеренное изменение переменных в глобальном контексте.

- **Скрытие данных**: внутренние функции или данные могут быть скрыты от внешнего доступа, обеспечивая механизм сокрытия информации и контроля доступа.

- **Инициализация**: структура IIFE идеально подходит для настройки начальных условий, таких как события привязки или данные предварительной обработки.

#### Современные альтернативы

С появлением ES6 и его объявлений let и const, а также блочных лексических сред необходимость IIFEs уменьшилась. Кроме того, стрелочные функции предоставляют более лаконичный метод определения немедленно вызываемых функций.

#### IIFE Варианты

- **Вызов круглых скобок**: пара круглых скобок немедленно вызывает включенную функцию. Хотя подход this более обширен, он лишен самоочевидных преимуществ.

- **Обтекание операторами**: подобно использованию круглых скобок для вызова, операторы !, + или - иногда используются для обеспечения ясности. Например:

- **Имя IIFE**: хотя имя IIFE встречается не так часто, оно может помочь в самореференции. Это наиболее эффективно, когда необходимо иметь более полную трассировку стека во время отладки.

#### Внимание при минификацииПри использовании IIFEs соблюдайте осторожность при использовании минификаторов для сжатия файлов JavaScript. Минимизация может привести к непредвиденным результатам, изменяя предыдущие ожидания объема.

**When to use:**

#### Варианты использования

- **Предотвращение загрязнения переменных**: при взаимодействии с библиотеками или вставке фрагментов кода IIFEs предотвращает загрязнение глобальной области видимости.

- **Шаблоны модулей**: IIFEs в сочетании с замыканиями закладывают основу для модульной организации кода, защищая частные переменные и функции.

**Examples:**

#### Пример кода: IIFE

Вот код JavaScript:

```js
(function(){
    var foo = 'bar';
    console.log(foo);
})();
```

В примере this функция заключена в круглые скобки, что гарантирует, что заключенная функция будет оценена как выражение. Впоследствии он вызывается с завершающей парой круглых скобок.

```js
(function(){
    console.log('Invoked!');
})();
```

```js
!function(){
    console.log('Invoked!');
}();
```

```js
(function factorial(n){
    if (n <= 1) return 1;
    return n * factorial(n-1);
})(5);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Как you create private variables in JavaScript?</summary>

### Как you create private variables in JavaScript?

**Definition:**

В JavaScript инкапсуляция частного состояния внутри объекта может быть достигнута с помощью замыкания. Это гарантирует, что состояние является локальным для объекта и недоступно напрямую извне.

**How it works:**

#### Как работают замыкания

Замыкание позволяет функции сохранять доступ к лексическому окружению (набору привязок переменных в момент объявления функции), в котором оно было определено, даже если функция выполняется вне этого лексического окружения.

Это означает, что любая внутренняя функция, определенная внутри другой функции, имеет доступ к переменным внешней функции, и этот доступ сохраняется даже после завершения выполнения внешней функции.

#### Практическая реализация с помощью функций и модулей конструктора

#### Функции конструктора

В примере this секрет является личным для каждого экземпляра гаджета благодаря закрытию.

#### Модули

В современных JavaScript шаблоны модулей в сочетании с немедленно вызываемыми функциональными выражениями (IIFE) часто используются для инкапсуляции и сокрытия данных.

Шаблон раскрывающего модуля позволяет выборочно раскрывать частные элементы.

Шаблон IIFE немедленно выполняется и возвращает назначаемый объект, фактически создавая модуль.

мойМодуль.частныйМетод();  // Выдает ошибку, поскольку частныйМетод не предоставлен
В примере this PrivateVariable и PrivateMethod доступны только в лексическом окружении IIFE, что делает их частными.

Инструменты JavaScript, такие как TypeScript и Babel, также предлагают такие модули, как Module.export, предоставляющие дополнительные возможности инкапсуляции.

**Examples:**

#### Например

```js
function outerFunction() {
    let outerVar = 'I am outer';  // This variable is in the lexical environment of outerFunction

    function innerFunction() {
        console.log(outerVar);  // Accesses outerVar from the lexical environment of outerFunction
    }

    return innerFunction;
}

let myInnerFunction = outerFunction();
```

- **myInnerFunction();  // Журналы**: «Я внешний»

Здесь InternalFunction сохраняет доступ к OuterVar.

При определении функции-конструктора JavaScript с function и new замыкание можно использовать для связывания частного состояния с каждым экземпляром:

```js
function Gadget() {
    let secret = 'top secret';
    this.setSecret = function (value) {
        secret = value;
    };
    this.getSecret = function () {
        return secret;
    };
}

let phone = new Gadget();
phone.setSecret('new secret');
console.log(phone.getSecret());  // 'new secret'
```

Вот код:

```js
let myModule = (function () {
    let privateVariable = 'I am private';

    function privateMethod() {
        console.log('I am a private method');
    }

    return {
        publicMethod: function () {
            console.log('I am a public method');
        },
        getPrivateVariable: function () {
            return privateVariable;
        }
    };
})();

console.log(myModule.getPrivateVariable());  // 'I am private'
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

### JavaScript Objects and Prototypes

<details>
<summary>Как you create an object in JavaScript?</summary>

### Как you create an object in JavaScript?

**Definition:**

Объект — это динамический набор собственных свойств. Его создают объектным литералом, `Object.create`, `new Object()` или `new` с функцией-конструктором или классом.

**How it works:**

- **Объектный литерал:** `{ name: "Ada" }` создаёт объект, у которого `[[Prototype]]` равен `Object.prototype`, если в литерале не задан `__proto__`.
- **`Object.create(proto)`:** создаёт объект, чей `[[Prototype]]` равен `proto` (`null` — без прототипа). Второй аргумент добавляет свойства через дескрипторы.
- **`new Object()`:** создаёт обычный объект того же рода, что и `{}`.
- **`new Constructor()` / `new Class()`:** создаёт объект, ставит `[[Prototype]]` равным `.prototype` конструктора и вызывает функцию с `this`, указывающим на этот объект.

**Examples:**

```js
const point = { x: 1, y: 2 };

const withProto = Object.create(point);
withProto.z = 3;

function Box(w) {
  this.w = w;
}
const box = new Box(4);
```

**Sources:**

- [MDN — Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [MDN — Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [javascript.info — Objects](https://javascript.info/object)
</details>

<details>
<summary>Что такое prototypes in JavaScript?</summary>

### Что такое prototypes in JavaScript?

**Definition:**

Прототип — это объект, на который другой объект ссылается через внутренний слот `[[Prototype]]`. Если имя не является собственным свойством, поиск продолжается на этом объекте.

**How it works:**

- **`[[Prototype]]`:** читается через `Object.getPrototypeOf(obj)` и задаётся через `Object.setPrototypeOf`. Стандартная форма в литерале — `{ __proto__: other }`. Аксессор `obj.__proto__` считается устаревшим.
- **Конец цепочки:** обход останавливается на `null`. У `null` нет прототипа.
- **`Function.prototype`:** другое понятие. Это объект, который становится `[[Prototype]]` экземпляров, созданных через `new` для этой функции. Это не собственный `[[Prototype]]` самой функции.
- **Обычные объекты:** прототип литерала — `Object.prototype`, а его цепочка заканчивается на `null`.

**Examples:**

```js
const parent = { kind: "parent" };
const child = Object.create(parent);

Object.getPrototypeOf(child) === parent; // true
child.kind; // "parent"
```

**Sources:**

- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [MDN — Object.getPrototypeOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)
- [javascript.info — Prototypal inheritance](https://javascript.info/prototype-inheritance)
</details>

<details>
<summary>Объясните prototypal inheritance.</summary>

### Объясните prototypal inheritance.

**Definition:**

Прототипное наследование переиспользует свойства через связь объектов. Отсутствующее свойство ищется на прототипе, затем на прототипе прототипа, пока не найдётся имя или не встретится `null`.

**How it works:**

- **Сначала собственные свойства:** если имя есть у объекта, берётся оно. Одноимённое свойство прототипа скрыто (затенение).
- **Унаследованные методы:** функция, лежащая на прототипе, выполняется с `this`, равным объекту, у которого её вызвали, а не прототипу.
- **Присваивание пишет собственное свойство:** `child.x = 1` не меняет прототип, если запись не перехватывает сеттер в цепочке.
- **Классы:** `class` и `extends` — синтаксис поверх этой цепочки. Второй модели наследования они не добавляют.

**Examples:**

```js
const parent = {
  value: 2,
  method() {
    return this.value + 1;
  },
};

const child = { __proto__: parent, value: 10 };
child.method(); // 11
```

**Sources:**

- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [javascript.info — Prototypal inheritance](https://javascript.info/prototype-inheritance)
- [javascript.info — F.prototype](https://javascript.info/function-prototype)
</details>

<details>
<summary>Что такое the difference between object literals and constructor functions?</summary>

### Что такое the difference between object literals and constructor functions?

**Definition:**

Объектный литерал собирает один объект на месте. Функция-конструктор (или класс) создаёт много объектов с общим `.prototype` и одним шагом инициализации через `new`.

**How it works:**

- **Литерал:** `{ a: 1 }` даёт один объект. Его `[[Prototype]]` — `Object.prototype`, если не задан `__proto__`.
- **Конструктор:** `new Foo()` создаёт объект, связывает `[[Prototype]]` с `Foo.prototype`, привязывает `this` к новому объекту и возвращает его, если функция сама не вернула другой объект.
- **Общие методы:** методы кладут на `Foo.prototype` (или в тело класса), чтобы каждый экземпляр находил их в цепочке, а не хранил свою копию функции.
- **Классы:** `class` — это функция-конструктор с более строгими правилами `new` и методами на `.prototype`.

**When to use:**

- Литерал — для одного объекта настроек, набора данных или разового набора полей.
- Конструктор или класс — когда нужны много экземпляров, общие методы и явная инициализация.

**Examples:**

```js
const once = { label: "solo" };

function User(name) {
  this.name = name;
}
User.prototype.greet = function () {
  return `Hi, ${this.name}`;
};

const ada = new User("Ada");
ada.greet(); // "Hi, Ada"
```

**Sources:**

- [MDN — Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [MDN — new operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)
- [MDN — Using classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_classes)
- [javascript.info — Constructor, operator new](https://javascript.info/constructor-new)
</details>

<details>
<summary>Как you add or remove properties from an object?</summary>

### Как you add or remove properties from an object?

**Definition:**

Присваивание свойству добавляет или заменяет собственное свойство. Оператор `delete` удаляет собственное настраиваемое свойство. Сам по себе он память не освобождает.

**How it works:**

- **Добавить или обновить:** `obj.key = value` или `obj[key] = value` создаёт или перезаписывает собственное свойство данных.
- **`delete obj.key`:** удаляет это собственное свойство и возвращает `true`. Для отсутствующего свойства тоже `true`, и объект не меняется.
- **Унаследованные имена:** `delete` действует только на собственные свойства. После удаления собственного свойства одноимённое свойство прототипа снова становится видимым.
- **Ненастраиваемые:** в нестрогом коде `delete` возвращает `false`, в строгом — `TypeError`. Глобальные `var` и встроенные свойства вроде `Math.PI` ненастраиваемые. Связки `let` и `const` не являются свойствами объекта, поэтому `delete` их не снимает.
- **Массивы:** `delete arr[i]` оставляет дыру и не меняет `length`. `splice` удаляет элемент и сдвигает остальные.

**When to use:**

- Присваивание — для обычного поля.
- `delete` — когда ключ должен исчезнуть (`in` становится ложным).
- `splice` — когда массив должен уменьшиться, а не стать разреженным.

**Examples:**

```js
const user = { name: "Ada", role: "dev" };
user.city = "London";
delete user.role;

"role" in user; // false
user.city; // "London"
```

**Sources:**

- [MDN — delete](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete)
- [MDN — Working with objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects)
- [javascript.info — Object properties configuration](https://javascript.info/property-descriptors)
</details>

### Asynchronous JavaScript

<details>
<summary>Что такое the event loop in JavaScript?</summary>

### Что такое the event loop in JavaScript?

**Definition:**

Цикл событий выполняет задачи JavaScript по одной на единственном стеке вызовов, затем опустошает очередь микрозадач до следующей задачи. Ожидание ввода-вывода само по себе этот ход не блокирует.

**How it works:**

- **Стек вызовов:** синхронный код выполняется до конца. Долгий цикл держит поток, пока не вернётся.
- **Задачи:** колбэки `setTimeout`, `setInterval` и ввода-вывода — это задачи. Цикл берёт самую старую готовую задачу на один ход.
- **Микрозадачи:** после этой задачи (и после выполнения скрипта) цикл запускает все стоящие в очереди микрозадачи, включая добавленные во время разбора очереди, и только потом следующую задачу. Реакции промисов и `queueMicrotask` — микрозадачи.
- **Отрисовка:** браузер может перерисовать страницу, когда микрозадачи пусты, и затем начать следующий ход.
- **Порядок:** сначала синхронные логи, затем колбэки промиса `.then`, затем колбэк `setTimeout(..., 0)`.

**Examples:**

```js
console.log("sync");
setTimeout(() => console.log("task"), 0);
Promise.resolve().then(() => console.log("microtask"));
// sync, microtask, task
```

**Sources:**

- [MDN — Using microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)
- [MDN — Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [javascript.info — Event loop](https://javascript.info/event-loop)
</details>

<details>
<summary>Объясните how callbacks work in JavaScript.</summary>

### Объясните how callbacks work in JavaScript.

**Definition:**

Колбэк — это функция, которую передают, чтобы другая функция вызвала её позже. Таймеры, события и многие асинхронные API сообщают о завершении вызовом этой функции.

**How it works:**

- **Синхронный колбэк:** `array.map(fn)` вызывает `fn` до возврата из `map`. Вызывающий остаётся на стеке.
- **Асинхронный колбэк:** `setTimeout(fn, 0)` возвращается сразу. `fn` выполнится позже как задача, когда текущий стек опустеет и микрозадачи будут разобраны.
- **Стиль error-first:** колбэки в стиле Node часто имеют вид `(error, result)`. Первый аргумент — ошибка или `null`, если вызов прошёл.
- **Вложенность:** несколько асинхронных шагов, каждый со своим колбэком, образуют пирамиду. Промисы и `async` / `await` выпрямляют эту последовательность.

**Examples:**

```js
function load(url, callback) {
  setTimeout(() => callback(null, `body of ${url}`), 0);
}

load("/notes", (error, body) => {
  if (error) return;
  console.log(body);
});
```

**Sources:**

- [MDN — Callback function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- [MDN — setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)
- [javascript.info — Introduction: callbacks](https://javascript.info/callbacks)
</details>

<details>
<summary>Что такое promises and how do they manage asynchronous code?</summary>

### Что такое promises and how do they manage asynchronous code?

**Definition:**

`Promise` — объект, который представляет значение, которого ещё может не быть. Он ожидает, затем выполняется со значением или отклоняется с причиной. Реакции в `.then`, `.catch` и `.finally` идут как микрозадачи.

**How it works:**

- **Исполнитель:** `new Promise(executor)` запускает `executor` сразу. `resolve(value)` выполняет промис, `reject(reason)` отклоняет. Исключение внутри исполнителя отклоняет промис.
- **Одно завершение:** промис завершается один раз. Поздние вызовы `resolve` или `reject` ничего не делают.
- **Цепочка:** `.then` возвращает новый промис. Верните значение, чтобы выполнить его; бросьте исключение или верните отклонённый промис, чтобы отклонить. Возврат промиса ждёт этот промис.
- **Ошибки:** отклонение переходит к следующему `.catch` (или к `.then` с обработчиком отклонения). Без обработчика это необработанное отклонение.
- **Комбинаторы:** `Promise.all` ждёт все выполнения и отклоняется при первом отклонении. `Promise.allSettled` ждёт каждый результат. `Promise.race` завершается первым завершением. `Promise.any` выполняется первым успешным результатом.

**Examples:**

```js
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

delay(10)
  .then(() => "done")
  .then((value) => console.log(value));
```

**Sources:**

- [MDN — Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN — Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [javascript.info — Promises](https://javascript.info/promise-basics)
</details>

<details>
<summary>Объясните async/await in JavaScript and how it differs from promises.</summary>

### Объясните async/await in JavaScript and how it differs from promises.

**Definition:**

`async` / `await` — синтаксис для потребления промисов. Функция `async` всегда возвращает промис. `await` приостанавливает эту функцию, пока ожидаемый промис не завершится, и продолжает её как микрозадачу.

**How it works:**

- **`async`:** возвращаемое значение оборачивается в выполненный промис. Брошенное исключение отклоняет этот промис.
- **`await`:** ждёт thenable. Выполненное значение становится результатом выражения. Отклонение бросает исключение внутри async-функции, поэтому его ловит `try` / `catch`.
- **Те же очереди:** `await` не добавляет новую модель планирования. Продолжение — это реакция промиса, поэтому оно всё ещё идёт раньше следующей задачи таймера.
- **Отличие от `.then`:** шаги читаются сверху вниз в одной функции. `.then` строит явную цепочку колбэков. Оба варианта дают промисы.

**When to use:**

- `async` / `await` — для последовательности шагов и `try` / `catch`.
- `.then` — когда промис уже есть и нужна короткая цепочка, или когда промис возвращают из не-async колбэка.

**Examples:**

```js
async function loadUser(id) {
  const response = await fetch(`/users/${id}`);
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN — await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [javascript.info — Async/await](https://javascript.info/async-await)
</details>

<details>
<summary>Что такое the Job queue (or Microtask queue)?</summary>

### Что такое the Job queue (or Microtask queue)?

**Definition:**

Очередь микрозадач (очередь заданий ECMAScript для работ промисов) хранит колбэки, которые выполняются после текущей задачи JavaScript и до следующей задачи, например таймера.

**How it works:**

- **Кто ставит в очередь:** `promise.then` / `catch` / `finally`, продолжения `await` и `queueMicrotask(fn)`.
- **Когда разбирается:** каждый раз, когда задача вот-вот вернёт управление циклу событий, выполняются все стоящие микрозадачи, включая добавленные во время разбора.
- **Против задач:** `setTimeout(fn, 0)` — задача. Она идёт только когда очередь микрозадач пуста.
- **Голодание:** микрозадача, которая снова ставит микрозадачу, откладывает следующую задачу и может отложить отрисовку. Цепочку обрывают, когда работа сделана.

**Examples:**

```js
setTimeout(() => console.log("task"), 0);
queueMicrotask(() => console.log("microtask"));
console.log("sync");
// sync, microtask, task
```

**Sources:**

- [MDN — Using microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)
- [MDN — queueMicrotask()](https://developer.mozilla.org/en-US/docs/Web/API/Window/queueMicrotask)
- [javascript.info — Microtasks](https://javascript.info/microtask-queue)
</details>

### DOM Manipulation and Browser APIs

<details>
<summary>Как you select DOM elements using JavaScript?</summary>

### Как you select DOM elements using JavaScript?

**Definition:**

Элементы ищут CSS-селекторами или старыми поисками по id, тегу и классу. `querySelector` возвращает первое совпадение или `null`. `querySelectorAll` возвращает все совпадения.

**How it works:**

- **`document.querySelector(selectors)`:** поиск в глубину до первого элемента. Неверная строка селектора бросает `SyntaxError`. Псевдоэлементы никогда не совпадают. При повторном id возвращается первый элемент с этим id.
- **`document.querySelectorAll(selectors)`:** статический `NodeList` всех совпадений. Он не обновляется, когда DOM меняется позже.
- **`getElementById`:** один элемент по id или `null`.
- **`getElementsByTagName` / `getElementsByClassName`:** живая `HTMLCollection`. Она отражает последующие изменения DOM.
- **Поиск внутри элемента:** `element.querySelector` ищет внутри этого элемента, а не по всему документу.

**Examples:**

```js
const form = document.querySelector("form.login");
const fields = form.querySelectorAll("input");
const title = document.getElementById("title");
```

**Sources:**

- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [MDN — Document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [javascript.info — Searching: getElement*, querySelector*](https://javascript.info/searching-elements-dom)
</details>

<details>
<summary>Объясните event propaGAtion in the DOM.</summary>

### Объясните event propaGAtion in the DOM.

**Definition:**

Событие DOM проходит три фазы: захват от окна вниз к цели, фазу цели, затем всплытие обратно вверх. `event.eventPhase` показывает, в какой фазе сейчас слушатель.

**How it works:**

- **Захват:** слушатели с `capture: true` (или третьим аргументом `true`) срабатывают по пути вниз.
- **Цель:** слушатели на элементе, который породил событие. `event.target` — этот элемент. `event.currentTarget` — элемент, на котором висит слушатель.
- **Всплытие:** слушатели с `capture: false` (значение по умолчанию) срабатывают по пути вверх. `click` всплывает. Не каждое событие всплывает.
- **`stopPropagation()`:** останавливает событие для слушателей на других узлах в фазах захвата и всплытия. Другие слушатели на том же элементе всё ещё выполняются. Действие браузера по умолчанию это не отменяет.
- **`stopImmediatePropagation()`:** дополнительно останавливает оставшиеся слушатели на текущем элементе.

**Examples:**

```js
parent.addEventListener("click", () => console.log("capture"), true);
child.addEventListener("click", () => console.log("target"));
parent.addEventListener("click", () => console.log("bubble"));
```

**Sources:**

- [MDN — Event.eventPhase](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase)
- [MDN — Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)
- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>Как you prevent a form from submitting using JavaScript?</summary>

### Как you prevent a form from submitting using JavaScript?

**Definition:**

Подпишитесь на событие `submit` формы и вызовите `event.preventDefault()`. Это отменяет действие браузера по умолчанию — переход по URL из атрибута action.

**How it works:**

- **`submit`:** срабатывает на `<form>`, когда активируют элемент отправки, до навигации. Событие всплывает.
- **`preventDefault()`:** сообщает браузеру, что событие обработано, и действие по умолчанию не выполняется. Распространение продолжается, если отдельно не вызвать `stopPropagation()`.
- **Отменяемость:** `preventDefault` ничего не делает, если `event.cancelable` ложно. Слушатель с `passive: true` не может отменить событие.
- **После отмены:** прочитайте `FormData` или значения полей и отправьте их сами, например через `fetch`.

**Examples:**

```js
const form = document.querySelector("form");
form.addEventListener("submit", (event) => {
  event.preventDefault();
  const data = new FormData(form);
  console.log(data.get("email"));
});
```

**Sources:**

- [MDN — Event.preventDefault()](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault)
- [MDN — HTMLFormElement: submit event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)
- [javascript.info — Form properties and methods](https://javascript.info/form-elements)
</details>

<details>
<summary>Что такое Web APIs in the context of JavaScript?</summary>

### Что такое Web APIs in the context of JavaScript?

**Definition:**

Web API — это интерфейсы, которые браузер (и некоторые другие хосты) открывают для JavaScript. Язык ECMAScript не определяет `document`, `fetch` и `localStorage`. Их даёт хост.

**How it works:**

- **Язык и хост:** синтаксис, объекты и промисы — это JavaScript. DOM, сеть, хранилище и история — Web API, которые вызывают из JavaScript.
- **DOM:** `document`, элементы и события. Страницу читают и меняют через эти объекты.
- **Сеть и время:** `fetch`, `WebSocket`, `setTimeout`. Завершение приходит промисом или колбэком в цикле событий.
- **Хранилище и история:** `localStorage`, `sessionStorage` и `history.pushState` — API окна, ограниченные источником страницы.

**Examples:**

```js
const title = document.querySelector("h1");
title.textContent = "Notes";

const response = await fetch("/api/notes");
const notes = await response.json();
```

**Sources:**

- [MDN — Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)
- [MDN — Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
</details>

<details>
<summary>Как you manipulate the browser history using JavaScript?</summary>

### Как you manipulate the browser history using JavaScript?

**Definition:**

`history.pushState` добавляет запись в историю сессии текущего документа и не загружает новую страницу. `popstate` срабатывает, когда пользователь позже переходит к такой записи.

**How it works:**

- **`pushState(state, unused, url)`:** `state` должен сериализоваться и хранится вместе с записью. Второй аргумент не используется; передайте `""`. `url` необязателен и должен быть того же источника. Браузер этот URL не запрашивает.
- **`replaceState`:** заменяет текущую запись, а не добавляет новую.
- **`popstate`:** срабатывает на `window`, когда активная запись меняется, потому что пользователь двигался по истории сессии. `event.state` — копия объекта состояния. Сам вызов `pushState` событие `popstate` не шлёт.
- **`back`, `forward`, `go`:** двигают историю сессии. `go(-1)` совпадает с `back()`, `go(1)` — с `forward()`. `go()` и `go(0)` делают то же, что `location.reload()`. Переход асинхронный; о завершении говорит `popstate`.

**Examples:**

```js
history.pushState({ page: 2 }, "", "/notes?page=2");

window.addEventListener("popstate", (event) => {
  console.log(event.state);
});
```

**Sources:**

- [MDN — History.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)
- [MDN — Window: popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)
- [MDN — History.go()](https://developer.mozilla.org/en-US/docs/Web/API/History/go)
- [MDN — History.replaceState()](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState)
</details>

### ES2015+ and Modern JavaScript Features

<details>
<summary>Что такое the new features introduced in ES6?</summary>

### Что такое the new features introduced in ES6?

**Definition:**

ES2015 (ES6) добавил блочную область видимости, более короткие функции и классы, деструктуризацию, модули и несколько новых встроенных объектов. Поздние редакции добавили ещё; «ES6» — это набор 2015 года.

**How it works:**

- **`let` и `const`:** блочная область. Чтение до объявления бросает `ReferenceError` (временная мёртвая зона). `var` имеет область функции и инициализируется как `undefined`.
- **Стрелочные функции:** короткая форма функции. У них нет своих `this`, `arguments` и `prototype`.
- **Классы:** синтаксис поверх функций-конструкторов и цепочки прототипов, включая `extends`.
- **Деструктуризация, значения по умолчанию, rest и spread:** распаковка массивов и объектов, подстановка пропущенных аргументов, сбор и разворот списков.
- **Модули:** `import` и `export` дают файлу свою область. В браузере точку входа грузят через `<script type="module">`.
- **В той же редакции:** шаблонные строки, `Promise`, `Map`, `Set`, `Symbol` и `for...of`.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
const { name } = user;
const greet = (who) => `Hello, ${who}`;
```

**Sources:**

- [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>Как you use destructuring assignments in ES6?</summary>

### Как you use destructuring assignments in ES6?

**Definition:**

Деструктуризация распаковывает элементы массива или свойства объекта в отдельные переменные. Она работает в объявлении, присваивании, списке параметров и в связывании `for...of`.

**How it works:**

- **Массивы:** `const [a, b] = pair` берёт значения по позиции. Дыра (`const [a, , b]`) пропускает одно. Так можно распаковать любой итерируемый объект.
- **Объекты:** `const { name } = user` копирует свойство `name`. Переименование: `{ name: label }`.
- **Значения по умолчанию:** срабатывают, когда значения нет или оно `undefined`. Для `null` не срабатывают.
- **Rest:** `...rest` должен быть последним. У массива он собирает остаток в новый массив. У объекта копирует оставшиеся перечисляемые собственные свойства в новый объект.
- **Форма присваивания:** `({ a } = obj)` нужны скобки. Без них `{ a }` разбирается как блок.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
const { name, role = "dev" } = user;
const [first, ...rest] = ["a", "b", "c"];
```

**Sources:**

- [MDN — Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring)
- [javascript.info — Destructuring assignment](https://javascript.info/destructuring-assignment)
</details>

<details>
<summary>Объясните the use of const and let keywords.</summary>

### Объясните the use of const and let keywords.

**Definition:**

`let` и `const` объявляют связывания с блочной областью. `const` нужно инициализировать, и его нельзя переприсвоить. `let` можно присвоить позже и переприсваивать.

**How it works:**

- **Временная мёртвая зона:** от начала блока до строки объявления чтение имени бросает `ReferenceError`. `var` в том же месте был бы `undefined`.
- **`const`:** связывание фиксировано. Свойства объекта в `const` менять можно. `const user = { name: "Ada" }; user.name = "Grace"` допустимо. `user = {}` — нет.
- **`let`:** одно связывание на блок. Одно и то же `let` нельзя объявить в блоке дважды. `let` в `for` — новое связывание на итерацию.
- **Не свойства:** `let` и `const` не становятся свойствами глобального объекта, и `delete` их не удаляет.

**When to use:**

- `const` — для связывания, которое не переприсваивают.
- `let` — когда переменная должна получить новое значение, например индекс цикла или накопитель.

**Examples:**

```js
const origin = { x: 0, y: 0 };
origin.x = 1;

let step = 0;
step += 1;
```

**Sources:**

- [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Grammar and types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types)
</details>

<details>
<summary>Что такое default parameters in JavaScript functions?</summary>

### Что такое default parameters in JavaScript functions?

**Definition:**

Параметр по умолчанию — значение, которое берётся, когда аргумент не передан или передан `undefined`. Выражение по умолчанию выполняется только в этом случае.

**How it works:**

- **Когда срабатывает:** пропущенный аргумент и явный `undefined` берут значение по умолчанию. Любое другое значение, включая `null`, `0` и `""`, сохраняется.
- **Поздние параметры:** значение по умолчанию может читать более ранние параметры, потому что они уже инициализированы.
- **Деструктуризация:** `function draw({ size = "big" } = {})` подставляет `{}` для всего аргумента, поэтому вызов без аргументов не бросает ошибку, а `size` всё ещё получает значение по умолчанию, если свойства нет.

**Examples:**

```js
function greet(name = "there") {
  return `Hello, ${name}`;
}

greet(); // "Hello, there"
greet(undefined); // "Hello, there"
greet(null); // "Hello, null"
```

**Sources:**

- [MDN — Default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [javascript.info — Destructuring assignment](https://javascript.info/destructuring-assignment)
</details>

<details>
<summary>Объясните the concept of modules in ES6.</summary>

### Объясните the concept of modules in ES6.

**Definition:**

ES-модуль — файл, который экспортирует значения и импортирует их в другие файлы. У каждого модуля своя область видимости. В браузере входной файл грузят через `<script type="module">`.

**How it works:**

- **`export`:** помечает связывание или значение по умолчанию, которое другие модули могут импортировать. `export const name = "square"` — именованный экспорт. `export default` — экспорт по умолчанию.
- **`import`:** `import { name } from "./square.js"` читает именованный экспорт. `import draw from "./square.js"` читает экспорт по умолчанию. В браузере голое имя вроде `"square"` бросает `TypeError`, если его не разрешает import map. Node может разрешать голые имена по файловой системе.
- **Загрузка:** современные браузеры выполняют модули без транспилятора. Сборщик всё ещё полезен для минификации, удаления мёртвого кода и нарезки чанков.
- **`import()`:** форма, похожая на функцию: грузит модуль по требованию и возвращает промис. Статический `import` поднимается и выполняется первым.

**When to use:**

- Статический `import` — для зависимостей, которые странице нужны всегда.
- Динамический `import()` — для маршрута, локали или тяжёлой возможности, которая может подождать.

**Examples:**

```js
// square.js
export function area(length) {
  return length * length;
}

// main.js
import { area } from "./square.js";
area(4); // 16
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [javascript.info — Modules, introduction](https://javascript.info/modules-intro)
</details>

### Event Handling

<details>
<summary>Как you handle events in JavaScript?</summary>

### Как you handle events in JavaScript?

**Definition:**

Слушатель регистрируют через `addEventListener` на элементе, который должен наблюдать событие. Браузер вызывает эту функцию, когда событие доходит до элемента.

**How it works:**

- **`addEventListener(type, listener, options)`:** `type` — строка вроде `"click"` или `"submit"`. Слушатель получает `Event`.
- **Параметры:** `capture: true` запускает слушатель в фазе захвата. `once: true` снимает слушатель после срабатывания. `passive: true` сообщает браузеру, что вы не вызовете `preventDefault` (вызов внутри такого слушателя действие не отменяет).
- **`this`:** у обычной функции-слушателя `this` равен `event.currentTarget`. Стрелочная функция сохраняет `this` внешней области.
- **Снятие:** `removeEventListener` нужна та же ссылка на функцию и тот же флаг захвата.

**Examples:**

```js
const button = document.querySelector("button");

function onClick(event) {
  console.log(event.currentTarget === this);
}

button.addEventListener("click", onClick);
```

**Sources:**

- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [javascript.info — Introduction to browser events](https://javascript.info/introduction-browser-events)
</details>

<details>
<summary>Что такое event deleGAtion and why is it useful?</summary>

### Что такое event deleGAtion and why is it useful?

**Definition:**

Делегирование событий — один слушатель на родителе, который обрабатывает события детей. Это работает, потому что событие всплывает, а `event.target` — элемент, с которого оно началось.

**How it works:**

- **`target` и `currentTarget`:** `target` — нажатый ребёнок. `currentTarget` — родитель, на котором висит слушатель.
- **Новые дети:** элементы, добавленные позже, всё равно всплывают к родителю, поэтому слушатель не вешают на каждый из них.
- **Фильтр:** игнорируйте клики не по нужному элементу, например когда `event.target.closest("li")` равен `null`.
- **Предел:** событие должно дойти до родителя. `stopPropagation` на ребёнке или событие, которое не всплывает, пропускает родительский слушатель.

**When to use:**

- Список, меню или таблица, строки которых добавляют и удаляют.
- Один слушатель, когда многим похожим детям нужен один и тот же обработчик.

**Examples:**

```js
const list = document.querySelector("ul");

list.addEventListener("click", (event) => {
  const item = event.target.closest("li");
  if (!item || !list.contains(item)) return;
  item.classList.toggle("done");
});
```

**Sources:**

- [MDN — Event.target](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)
- [javascript.info — Event delegation](https://javascript.info/event-delegation)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>Как you add and remove an event listener from an element?</summary>

### Как you add and remove an event listener from an element?

**Definition:**

`addEventListener` регистрирует слушатель. `removeEventListener` снимает его, только если передать тот же объект функции и ту же настройку захвата.

**How it works:**

- **Та же ссылка:** анонимную функцию, переданную в `addEventListener`, потом не снять: вызов удаления был бы другой функцией.
- **Флаг захвата:** слушатель, добавленный с `capture: true`, снимают через `removeEventListener(type, fn, true)`. Значение по умолчанию `false` не совпадает с `true`.
- **`once: true`:** браузер снимает слушатель после первого вызова. Сами `removeEventListener` не вызывают.
- **`AbortSignal`:** передайте `{ signal }` в параметрах. `controller.abort()` снимает все слушатели, зарегистрированные с этим сигналом.

**Examples:**

```js
const button = document.querySelector("button");

function onClick() {
  console.log("clicked");
  button.removeEventListener("click", onClick);
}

button.addEventListener("click", onClick);
```

**Sources:**

- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [MDN — EventTarget.removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)
- [javascript.info — Introduction to browser events](https://javascript.info/introduction-browser-events)
</details>

<details>
<summary>Можете ли вы explain how "this" works in event handlers?</summary>

### Можете ли вы explain how "this" works in event handlers?

**Definition:**

В обычной функции, переданной в `addEventListener`, `this` — элемент, на котором висит слушатель. Это `event.currentTarget`. Стрелочная функция такого связывания не получает.

**How it works:**

- **Обычная функция:** `this === event.currentTarget` истинно. `this === event.target` истинно только когда слушатель сидит на элементе, который начал событие.
- **Стрелочная функция:** `this` берётся из внешней области. Это не элемент. Элемент читают из `event.currentTarget`.
- **`bind`:** `fn.bind(somethingElse)` задаёт `this`. Связанная функция — новая ссылка, поэтому снимать нужно именно её.
- **Делегирование:** у слушателя на родителе `this` — родитель. Нажатый ребёнок — `event.target`.

**When to use:**

- Обычная функция, когда `this` должен быть элементом.
- Стрелочная функция, когда нужен внешний `this`, а элемент читают из события.

**Examples:**

```js
button.addEventListener("click", function (event) {
  console.log(this === event.currentTarget); // true
});

button.addEventListener("click", (event) => {
  event.currentTarget.classList.add("pressed");
});
```

**Sources:**

- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>Что такое the difference between event.preventDefault() and event.stopPropaGAtion()?</summary>

### Что такое the difference between event.preventDefault() and event.stopPropaGAtion()?

**Definition:**

`preventDefault()` отменяет действие браузера по умолчанию, и событие продолжает распространяться. `stopPropagation()` не даёт событию дойти до других узлов и действие по умолчанию не отменяет.

**How it works:**

- **`preventDefault()`:** без параметров и без возвращаемого значения. Работает, только если `event.cancelable` истинно. Слушатель с `passive: true` событие отменить не может: вызов игнорируется, и консоль может предупредить.
- **Действия по умолчанию:** переход по ссылке, отправка формы, переключение флажка, прокрутка. Отмена одного из них не останавливает родительские слушатели.
- **`stopPropagation()`:** останавливает слушателей захвата и всплытия на других узлах. Другие слушатели на текущем элементе всё ещё выполняются.
- **`stopImmediatePropagation()`:** дополнительно останавливает оставшиеся слушатели на текущем элементе. Само по себе действие по умолчанию не отменяет.

**When to use:**

- `preventDefault` — когда браузер не должен переходить, отправлять форму или делать своё действие.
- `stopPropagation` — когда родительский слушатель не должен тоже увидеть это событие.
- Оба — когда нужно отменить действие и не дать ему всплыть.

**Examples:**

```js
link.addEventListener("click", (event) => {
  event.preventDefault();
});

row.addEventListener("click", (event) => {
  event.stopPropagation();
});
```

**Sources:**

- [MDN — Event.preventDefault()](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault)
- [MDN — Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

### Web Storage and Security

<details>
<summary>Что такое the difference between localStorage, sessionStorage, and cookies?</summary>

### Что такое the difference between localStorage, sessionStorage, and cookies?

**Definition:**

`localStorage` и `sessionStorage` хранят строки в браузере и не отправляют их с HTTP-запросами. Cookie — небольшая пара имя/значение, которую браузер хранит и обычно отправляет обратно серверу.

**How it works:**

- **`localStorage`:** один объект `Storage` на источник (схема, хост и порт). Срока нет. Данные живут между сессиями. В приватном режиме они стираются, когда закрывается последняя приватная вкладка. Ключи и значения — строки UTF-16. Их читают и пишут `setItem`, `getItem`, `removeItem` и `clear`.
- **`sessionStorage`:** разделён по источнику и по вкладке верхнего уровня. Переживает перезагрузку и восстановление в этой вкладке и очищается, когда вкладку или окно закрывают. Новая вкладка — новая сессия. Если у страницы есть opener, хранилище начинается как копия; дальнейшие записи раздельные.
- **Cookie:** сервер ставит их заголовком `Set-Cookie`. Браузер шлёт их в следующих запросах в заголовке `Cookie`. MDN называет обычный размер около 4 КБ и ограниченное число на домен, поэтому они едут с каждым подходящим запросом. `document.cookie` читает и пишет cookie без `HttpOnly`.
- **Ограничения cookie:** `HttpOnly` скрывает cookie от JavaScript. `Secure` отправляет её только по HTTPS (localhost — исключение, в Safari его нет). `SameSite` бывает `Strict`, `Lax` или `None` и управляет межсайтовой отправкой. `Expires` или `Max-Age` задают срок; без них cookie сессионная.

**When to use:**

- `localStorage` — для настройки, которая должна пережить закрытую вкладку и не является секретом.
- `sessionStorage` — для данных, которые должны умереть вместе с вкладкой, например черновик этой вкладки.
- Cookie — для состояния, которое должен видеть сервер, например id сессии, с `HttpOnly`, `Secure` и `SameSite`.

**Examples:**

```js
localStorage.setItem("theme", "dark");
sessionStorage.setItem("draft", "hello");

document.cookie = "lang=en; Max-Age=86400; Secure; SameSite=Lax";
```

**Sources:**

- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN — Window.sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- [MDN — Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
- [javascript.info — LocalStorage, sessionStorage](https://javascript.info/localstorage)
</details>

<details>
<summary>Можете ли вы explain Cross-Site Scripting (XSS) and how to prevent it?</summary>

### Можете ли вы explain Cross-Site Scripting (XSS) and how to prevent it?

**Definition:**

XSS — атака, при которой сайт выполняет код атакующего так, будто это код самого сайта. Этот код может читать страницу, читать `localStorage` и слать запросы с учётными данными пользователя.

**How it works:**

- **Два шага:** сайт принимает ввод, который мог составить атакующий, и вставляет этот ввод в страницу, не убедившись, что он не выполнится как JavaScript.
- **Частый сток:** присваивание строки в `innerHTML` разбирает её как HTML. `<img>` с обработчиком `onerror` может запустить скрипт. Вставленные так теги `<script>` не выполняются, но другая разметка может.
- **Обычный текст:** `textContent` вставляет текст и не разбирает HTML.
- **Cookie:** помечайте сессионную cookie как `HttpOnly`, чтобы `document.cookie` её не прочитал.
- **CSP:** заголовок `Content-Security-Policy` может ограничить, какие скрипты грузятся, и потребовать Trusted Types перед вставкой HTML.

**Examples:**

```js
const name = userInput;
label.textContent = name;

// unsafe if userInput is attacker-controlled:
// box.innerHTML = userInput;
```

**Sources:**

- [MDN — Cross-site scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
- [MDN — Element.innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
- [MDN — Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
</details>

<details>
<summary>Что такое Cross-Origin Resource SHAring (CORS) and how does it work?</summary>

### Что такое Cross-Origin Resource SHAring (CORS) and how does it work?

**Definition:**

CORS — механизм HTTP-заголовков. Сервер называет, каким другим источникам (схема, хост и порт) браузер может дать скриптам прочитать ответ. `fetch` и `XMLHttpRequest` следуют политике одного источника, пока эти заголовки не разрешат ответ.

**How it works:**

- **Один источник:** страница на `https://domain-a.com`, которая через `fetch` берёт `https://domain-b.com/data.json`, делает межсайтовый запрос. Браузер отдаёт тело скрипту, только если ответ разрешает это, обычно через `Access-Control-Allow-Origin`.
- **Предварительный запрос:** для методов кроме `GET`, `HEAD` и `POST` или для заголовков вне safelist браузер сначала шлёт `OPTIONS` и описывает метод и заголовки, которые собирается использовать. Настоящий запрос идёт, только если сервер их разрешил.
- **Запросы как у формы:** «простой» запрос может уйти без предварительного, но скрипт всё равно не прочитает ответ, пока `Access-Control-Allow-Origin` его не разрешит.
- **Учётные данные:** cookie и HTTP-аутентификация включаются, только когда клиент просит и сервер явно разрешает credentials.
- **Ошибки:** сбой CORS отклоняет вызов в скрипте. Ошибка JavaScript не содержит причину сервера; подробности в консоли браузера.

**Examples:**

```js
const response = await fetch("https://domain-b.com/data.json");
if (!response.ok) throw new Error(response.statusText);
const data = await response.json();
```

**Sources:**

- [MDN — Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
</details>

<details>
<summary>Как content security policy (CSP) help in preventing security attacks?</summary>

### Как content security policy (CSP) help in preventing security attacks?

**Definition:**

CSP — набор инструкций, обычно в заголовке ответа `Content-Security-Policy`, который говорит браузеру, что странице можно загружать и делать. Главное применение — ограничить, какой JavaScript может выполняться, и этим отсечь многие XSS-нагрузки.

**How it works:**

- **Заголовок:** шлите его на ответах, не только на документе. `<meta http-equiv="Content-Security-Policy">` годится для части политик и поддерживает не каждую директиву.
- **Директивы:** через точку с запятой. `default-src 'self'` разрешает ресурсы с собственного источника страницы. `img-src` может добавить хосты. `script-src` управляет скриптами.
- **XSS:** внедрённый встроенный обработчик или скрипт с хоста не из списка отклоняется, если политика его не разрешает.
- **Другие задачи:** ограничить, кто может встроить страницу (кликджекинг), поднимать небезопасные запросы до HTTPS и требовать Trusted Types перед клиентской вставкой HTML.
- **Внедрение:** директивы независимы. Можно выпустить ограничение встраивания без ограничения скриптов.

**Examples:**

```http
Content-Security-Policy: default-src 'self'; img-src 'self' example.com
```

**Sources:**

- [MDN — Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
- [MDN — Cross-site scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
</details>

### JavaScript Debugging

<details>
<summary>Какие инструменты and techniques do you use for debugging JavaScript code?</summary>

### Какие инструменты and techniques do you use for debugging JavaScript code?

**Definition:**

Для отладки используют инструменты разработчика браузера: консоль для значений и ошибок и отладчик, чтобы остановиться на строке. `try` / `catch` обрабатывает ожидаемые исключения и не заменяет просмотр стека вызовов.

**How it works:**

- **Консоль:** `console.log`, `console.error` и `console.trace` печатают значения и стек, пока страница работает.
- **`debugger`:** оператор останавливает выполнение, когда открыты инструменты разработчика, как точка останова на этой строке.
- **Debugger:** отладчик показывает стек вызовов, позволяет шагать и следить за выражениями.
- **Исключения:** необработанный `throw` прерывает текущий ход и попадает в консоль. `catch` выполняется только для броска внутри своего `try`.

**Examples:**

```js
function parseCount(text) {
  const value = Number(text);
  if (Number.isNaN(value)) {
    console.error("not a number", text);
    return null;
  }
  return value;
}
```

**Sources:**

- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
</details>

<details>
<summary>Как you debug a JavaScript application in the browser?</summary>

### Как you debug a JavaScript application in the browser?

**Definition:**

Откройте инструменты разработчика, воспроизведите ошибку и сначала прочитайте консоль. Затем остановитесь в панели Sources на строке, где состояние уже плохое, и посмотрите переменные в этот момент.

**How it works:**

- **Консоль:** синтаксические ошибки, необработанные исключения и неудачные вызовы `fetch` видны здесь. Подробность сбоя CORS — в консоли, а не в объекте ошибки JavaScript.
- **Пауза:** поставьте точку останова или вставьте `debugger` при открытых инструментах. Выполнение останавливается перед следующей инструкцией.
- **Шаг:** step over остаётся в этой функции, step into заходит в вызов. Стек вызовов перечисляет функции, которые привели сюда.
- **Сеть:** панель Network показывает URL запроса, статус и тело, когда ошибка — неудачный HTTP-вызов.

**Examples:**

```js
button.addEventListener("click", () => {
  debugger;
  const name = input.value;
  console.log(name);
});
```

**Sources:**

- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
</details>

<details>
<summary>Объясните the concept and use of breakpoints.</summary>

### Объясните the concept and use of breakpoints.

**Definition:**

Точка останова — место, где отладчик приостанавливает JavaScript, чтобы можно было посмотреть стек вызовов и текущие связывания. Её ставят в панели Sources или пишут оператор `debugger`.

**How it works:**

- **Точка на строке:** выполнение останавливается до этой строки. Тогда можно читать локальные переменные.
- **`debugger`:** та же пауза, но только когда открыты инструменты разработчика. В поставляемом коде её не оставляют.
- **Шаги:** continue идёт до следующей точки. Step over выполняет вызов, не заходя внутрь. Step into заходит в вызов.
- **Когда останавливаться:** на итерации или ветке, которая ломается, а не логировать каждый проход.

**Examples:**

```js
for (const item of items) {
  if (item.id === brokenId) {
    debugger;
  }
}
```

**Sources:**

- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
</details>

<details>
<summary>Как you handle exceptions in JavaScript?</summary>

### Как you handle exceptions in JavaScript?

**Definition:**

`try` / `catch` / `finally` обрабатывает брошенное исключение. Сначала выполняется `try`. Если он бросает, выполняется `catch`. `finally` выполняется до выхода из всей конструкции, было исключение или нет.

**How it works:**

- **Форма:** нужны `try` и `catch`, `finally` или оба. Тела должны быть блоками.
- **Связывание:** `catch (error)` хранит брошенное значение и существует только в блоке `catch`. Связывание можно опустить, если значение не нужно.
- **Фильтр:** проверьте `error instanceof TypeError` (или другую встроенную ошибку) и перебросьте остальное через `throw error`.
- **`finally`:** для уборки. `return` или `throw` внутри `finally` перекрывает `return` или `throw` из `try` или `catch`, поэтому управляющие операторы в `finally` не ставят.
- **Async:** отклонённый промис не является `throw` у вызывающего, пока его не `await`-ят внутри `try` или не повесят `.catch`.

**Examples:**

```js
function readJson(text) {
  try {
    return JSON.parse(text);
  } catch (error) {
    if (error instanceof SyntaxError) return null;
    throw error;
  }
}
```

**Sources:**

- [MDN — try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
- [javascript.info — Error handling, try...catch](https://javascript.info/try-catch)
</details>

### Performance and Optimization

<details>
<summary>Какие техники can be used to improve JavaScript performance?</summary>

### Какие техники can be used to improve JavaScript performance?

**Definition:**

Главный поток оставляют свободным для ввода и отрисовки. Отдают меньше JavaScript, избегают долгой синхронной работы и грузят картинки и лишние модули только когда они нужны.

**How it works:**

- **Главный поток:** долгий цикл или тяжёлый синхронный разбор блокирует клики и отрисовку, пока не вернётся. Работу дробят или убирают с критического пути страницы.
- **Меньше кода:** сборщик может минифицировать, выбросить неиспользуемые экспорты (tree-shaking / удаление мёртвого кода) и нарезать чанки. Руководство MDN по модулям говорит, что это не отменяет сборщик.
- **По требованию:** `import()` грузит модуль позже и возвращает промис. `img.loading = "lazy"` просит браузер отложить картинку вне экрана.
- **DOM:** текст обновляют через `textContent`, когда HTML не нужен. Чтение `innerText` может вызвать reflow, потому что зависит от вычисленного стиля.

**Examples:**

```js
const panel = document.querySelector("#details");
panel.hidden = true;
button.addEventListener("click", async () => {
  const { open } = await import("./details.js");
  open(panel);
});
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
</details>

<details>
<summary>Как JavaScript minification and bundling contribute to performance?</summary>

### Как JavaScript minification and bundling contribute to performance?

**Definition:**

Минификация уменьшает файл: убирает пробелы и сокращает локальные имена. Сборка соединяет модули в меньшее число файлов и может выбросить код, который никто не импортирует. Браузер тогда скачивает и разбирает меньше, прежде чем страница сможет работать.

**How it works:**

- **Минификация:** передаваемых байт меньше. Поведение программы то же. Source map позволяет DevTools показать исходные файлы.
- **Сборка:** один граф `import` / `export` становится одним или несколькими чанками. Меньше запросов важно, когда каждый запрос платит круговой задержкой.
- **Мёртвый код:** MDN отмечает, что сборщики также удаляют мёртвый код и делают tree-shaking, поэтому неиспользуемый экспорт можно не отправлять.
- **Предел:** один огромный бандл задерживает первый запуск. Редкую часть отделяют динамическим `import()`, а не кладут в первый чанк.

**When to use:**

Минифицируйте и собирайте код, который нужен каждой странице. Тяжёлый необязательный экран отделите, чтобы его не было в первой загрузке.

**Examples:**

```js
const editor = await import("./editor.js");
editor.mount(document.querySelector("#editor"));
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
</details>

<details>
<summary>Обсудите the importance of web page loading time and performance.</summary>

### Обсудите the importance of web page loading time and performance.

**Definition:**

Время загрузки — сколько пользователь ждёт, пока страница станет полезной. HTML, CSS, JavaScript и картинки конкурируют за сеть и главный поток. Долгая задача JavaScript после прихода байтов всё равно задерживает взаимодействие.

**How it works:**

- **Байты:** большие скрипты и картинки дольше скачиваются. Минификация, меньше запросов и ленивые картинки сокращают это ожидание.
- **Разбор и запуск:** браузер должен разобрать JavaScript, прежде чем выполнить. Большой первый бандл задерживает первое взаимодействие даже в быстрой сети.
- **Главный поток:** пока скрипт выполняется, страница не обрабатывает ввод. Руководство MDN по микрозадачам также предупреждает, что цикл микрозадач может отложить следующую задачу и отрисовку.
- **Картинки:** `loading="lazy"` у картинки вне окна откладывает запрос, пока картинка не понадобится.

**Examples:**

```js
const img = document.createElement("img");
img.loading = "lazy";
img.width = 320;
img.height = 240;
img.src = "/photos/1.jpg";
img.alt = "Notes";
```

**Sources:**

- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>Объясните how lazy loading works in JavaScript.</summary>

### Объясните how lazy loading works in JavaScript.

**Definition:**

Ленивая загрузка откладывает работу, пока она не нужна. Для картинки `loading="lazy"` говорит браузеру отложить запрос, пока картинка вне окна. Для кода динамический `import()` запрашивает модуль, когда его вызывают.

**How it works:**

- **Картинки:** задайте `img.loading = "lazy"` (или HTML-атрибут). `eager` грузит при первой загрузке страницы. MDN описывает `lazy` как подсказку ждать, пока картинка понадобится.
- **Размер:** задайте `width` и `height`, чтобы браузер зарезервировал место до прихода файла.
- **Модули:** `import("./chart.js")` возвращает промис пространства имён модуля. Статический граф `import` не включает этот файл, пока вызов не выполнится.
- **Не замок:** подсказка всё равно может загрузить файл рано, если браузер решит, что картинка близко. На ней не прячут секретный ресурс.

**When to use:**

`loading="lazy"` — для картинок ниже первого экрана. `import()` — для возможности, которая не нужна первому виду.

**Examples:**

```js
const img = document.createElement("img");
img.loading = "lazy";
img.alt = "Diagram";
img.src = "/diagram.webp";

async function showChart() {
  const chart = await import("./chart.js");
  chart.draw();
}
```

**Sources:**

- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
</details>

### JavaScript Testing

<details>
<summary>Что такое some JavaScript testing frameworks you know?</summary>

### Что такое some JavaScript testing frameworks you know?

**Definition:**

В языке нет раннера тестов. В Node.js есть `node:test`. Проекты в браузере обычно добавляют библиотеку, которая находит файлы тестов, запускает их и сообщает успех или провал. Учебный модуль MDN по тестированию — точка входа для автоматических проверок во фронтенде.

**How it works:**

- **`node:test`:** импортируйте `describe` и `it` из `node:test` и проверяйте через `node:assert/strict`. Запуск: `node --test`.
- **Библиотеки:** Jest и Vitest — частые раннеры прикладного кода. Это пакеты, которые ставят; они не часть языка.
- **Сценарии в браузере:** раннер, который ведёт настоящую страницу, проверяет то, что видит пользователь. Это другой инструмент, не assert уровня функции.
- **Общее:** тест вызывает ваш код и проваливает прогон, когда утверждение бросает исключение.

**Examples:**

```js
import assert from "node:assert/strict";
import test from "node:test";

test("adds", () => {
  assert.equal(1 + 2, 3);
});
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
</details>

<details>
<summary>Как you write unit tests for JavaScript code?</summary>

### Как you write unit tests for JavaScript code?

**Definition:**

Модульный тест вызывает одну функцию с известными аргументами и проверяет возврат или брошенную ошибку. Он не запускает браузер или сервер.

**How it works:**

- **Подготовка:** соберите вход в тесте.
- **Действие:** вызовите функцию один раз.
- **Проверка:** `node:assert/strict` сравнивает в духе `Object.is` и бросает `AssertionError` при провале.
- **Изоляция:** передавайте зависимости аргументами. Не давайте модулю ходить в `fetch` или `document`, если проверяете чистый расчёт.

**Examples:**

```js
import assert from "node:assert/strict";

function fullName(user) {
  return `${user.first} ${user.last}`;
}

assert.equal(fullName({ first: "Ada", last: "Lovelace" }), "Ada Lovelace");
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
</details>

<details>
<summary>Что такое Test-Driven Development (TDD) in JavaScript?</summary>

### Что такое Test-Driven Development (TDD) in JavaScript?

**Definition:**

Разработка через тесты пишет провальную проверку до поведения. Добавляют тест, который падает, меняют функцию, пока тест не пройдёт, затем чистят код, не меняя результат.

**How it works:**

- **Красный:** новое утверждение падает, потому что поведения ещё нет.
- **Зелёный:** наименьшее изменение, после которого утверждение проходит.
- **Рефакторинг:** переименовывают и дробят, пока тот же тест проходит.
- **Раннер:** `node --test` (или другой раннер) — цикл, который повторяют. Язык порядок не заставляет; это делает человек.

**Examples:**

```js
import assert from "node:assert/strict";
import test from "node:test";
import { total } from "./total.js";

test("sums prices", () => {
  assert.equal(total([{ price: 2 }, { price: 3 }]), 5);
});
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Можете ли вы explain the difference between unit and integration testing?</summary>

### Можете ли вы explain the difference between unit and integration testing?

**Definition:**

Модульный тест проверяет одну функцию на входах, которые вы задаёте. Интеграционный тест проверяет несколько настоящих частей вместе, например функцию, которая вызывает `fetch` и читает JSON-тело.

**How it works:**

- **Модуль:** нет сети, нет DOM, нет часов, которые вы не подменили. Провал указывает на эту функцию.
- **Интеграция:** части те же, что в продуктиве. Провал может быть в функции, в проверке HTTP-статуса или на сервере.
- **`fetch`:** пример MDN проверяет `response.ok`, затем `response.json()`. Интеграционный тест может прогнать этот путь на тестовом сервере. Модульный тест разборщика берёт уже разобранное значение.
- **Оба:** утверждение всё равно бросает, когда результат неверен. Они отвечают на разные вопросы.

**When to use:**

Модульный тест — для чистого расчёта. Интеграционный — когда ошибка на стыке вашего кода и `fetch`, DOM или другого модуля.

**Examples:**

```js
export function cents(products) {
  return products.reduce((sum, item) => sum + item.price, 0);
}

export async function loadCents(url) {
  const response = await fetch(url);
  if (!response.ok) throw new Error(response.statusText);
  return cents(await response.json());
}
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
</details>

### Networking with JavaScript

<details>
<summary>Как you make HTTP requests in JavaScript?</summary>

### Как you make HTTP requests in JavaScript?

**Definition:**

Вызовите глобальный `fetch(url, options)`. Он возвращает промис `Response`. Промис отклоняется при сетевом сбое. Ошибочный HTTP-статус вроде 404 всё равно выполняет промис, поэтому проверяют `response.ok`.

**How it works:**

- **`fetch`:** доступен на `window` и в воркерах. Передайте строку URL или `Request` и необязательный объект настроек.
- **Тело:** `response.json()`, `response.text()` и другие методы тела возвращают промисы. Вызывают только один из них.
- **Статус:** бросайте, когда `response.ok` ложно. `ok` истинно для статусов 200–299.
- **CORS:** межсайтовый ответ читается, только если сервер разрешил. Ошибка JavaScript не объяснит несовпадение заголовков.

**Examples:**

```js
async function getNotes() {
  const response = await fetch("/api/notes");
  if (!response.ok) {
    throw new Error(`Response status: ${response.status}`);
  }
  return response.json();
}
```

**Sources:**

- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [javascript.info — Fetch](https://javascript.info/fetch)
- [MDN — Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое the difference between XMLHTTPRequest and Fetch API?</summary>

### Что такое the difference between XMLHTTPRequest and Fetch API?

**Definition:**

`fetch` — замена `XMLHttpRequest` на промисах. `XMLHttpRequest` сообщает о ходе через колбэки и обработчики событий. `fetch` возвращает промис и работает с service worker и CORS.

**How it works:**

- **`fetch`:** `await fetch(url)` даёт `Response`. `response.ok` проверяют сами; 404 промис не отклоняет.
- **`XMLHttpRequest`:** создайте объект, `open(method, url)`, `send()` и читайте `status` и `responseText` в обработчике `load`.
- **Оба:** межсайтовое чтение следует CORS. Браузер не отдаст тело без нужных заголовков ответа.
- **Прогресс:** у `XMLHttpRequest` есть события прогресса отправки и загрузки. У базового `fetch` такой же модели прогресса нет.

**When to use:**

`fetch` — для нового кода запросов. `XMLHttpRequest` — когда нужно слушать прогресс отправки или загрузки у API, который уже его использует.

**Examples:**

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", "/api/notes");
xhr.addEventListener("load", () => {
  console.log(xhr.status, xhr.responseText);
});
xhr.send();
```

**Sources:**

- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN — XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [javascript.info — Fetch](https://javascript.info/fetch)
</details>

<details>
<summary>Что такое AJAX, and how does it work?</summary>

### Что такое AJAX, and how does it work?

**Definition:**

AJAX — это приём: запросить данные у сервера из JavaScript и обновить страницу без полной навигации. Сегодня запрос обычно делает `fetch`. Более старый API с той же ролью — `XMLHttpRequest`.

**How it works:**

- **Запрос:** скрипт вызывает `fetch` или `XMLHttpRequest` после загрузки страницы.
- **Ответ:** читают тело (`response.json()` или `xhr.responseText`) и меняют DOM, например через `textContent`.
- **Без полной перезагрузки:** браузер не выбрасывает документ, как при обычной отправке формы.
- **Имя:** это термин приёма. Отдельного объекта в языке нет.

**Examples:**

```js
async function refreshCount() {
  const response = await fetch("/api/count");
  if (!response.ok) throw new Error(response.statusText);
  const data = await response.json();
  document.querySelector("#count").textContent = String(data.count);
}
```

**Sources:**

- [MDN — AJAX](https://developer.mozilla.org/en-US/docs/Glossary/AJAX)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [javascript.info — Fetch](https://javascript.info/fetch)
</details>

<details>
<summary>Как you use WebSockets in a web application?</summary>

### Как you use WebSockets in a web application?

**Definition:**

`WebSocket` — долгоживущее соединение. Страница шлёт и принимает сообщения без нового HTTP-запроса на каждое. Создают через `new WebSocket(url)`, затем `send` и слушают `message`.

**How it works:**

- **Открытие:** `new WebSocket("ws://localhost:8080")` или `wss:` для TLS. Событие `open` срабатывает, когда соединение готово.
- **Отправка:** `socket.send(data)` ставит данные в очередь. `bufferedAmount` — сколько байт ещё в очереди.
- **Приём:** `data` события `message` — то, что прислал сервер.
- **Закрытие:** `socket.close()` или событие `close`. `readyState` — текущее состояние.
- **Обратное давление:** API WebSocket его не применяет. Если сообщения приходят быстрее, чем вы их обрабатываете, растут память или CPU. MDN указывает на `WebSocketStream`, когда обратное давление нужно.

**When to use:**

Живая лента или чат, где обе стороны шлют много коротких сообщений. `fetch` — для одного запроса и ответа.

**Examples:**

```js
const socket = new WebSocket("wss://example.com/live");

socket.addEventListener("open", () => {
  socket.send("hello");
});

socket.addEventListener("message", (event) => {
  console.log(event.data);
});
```

**Sources:**

- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [javascript.info — WebSocket](https://javascript.info/websocket)
</details>

### JavaScript Patterns and Best Practices

<details>
<summary>Что такое design pattern in JavaScript?</summary>

### Что такое design pattern in JavaScript?

**Definition:**

Паттерн проектирования — повторяемый способ устроить код под задачу, которая встречается часто. Это не возможность языка и не библиотека. Замыкания, прототипы и модули — инструменты, из которых собраны обычные паттерны JavaScript.

**How it works:**

- **Назовите задачу:** один общий экземпляр, скрытое состояние или публичная поверхность над приватными функциями.
- **Средства языка:** замыкание держит переменные приватными. Литерал объекта или класс делит методы. `import` / `export` — модульная форма, которая теперь есть в языке.
- **Формулировка MDN:** фреймворк — это библиотека, которая добавляет мнения о том, как строить приложение. Паттерн меньше: форма, которую можно написать в одном файле.
- **Не копируйте вслепую:** паттерн должен совпасть с задачей. Класса хватает, когда нужно лишь несколько объектов.

**Examples:**

```js
function createCounter() {
  let value = 0;
  return {
    inc() {
      value += 1;
      return value;
    },
  };
}
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
</details>

<details>
<summary>Можете ли вы explain the module pattern?</summary>

### Можете ли вы explain the module pattern?

**Definition:**

Модульный паттерн прячет состояние в области функции и возвращает только функции, которые должны быть публичными. Замыкание держит приватные связывания живыми после возврата внешней функции. ES-модули (`export` / `import`) — языковая версия той же идеи, один файл на модуль.

**How it works:**

- **Замыкание:** переменные, объявленные во внешней функции, не являются свойствами возвращённого объекта.
- **Публичная поверхность:** методы возвращённого объекта могут читать и писать эти переменные.
- **IIFE:** старая форма сразу вызывает функцию и кладёт возвращённый объект в одну переменную.
- **ES-модули:** `export` помечает публичные имена. Другие файлы их `import`. Область модуля уже приватна.

**When to use:**

ES-модуль — для нового кода. Форма IIFE — когда нужна одна приватная область внутри файла, который сам не модуль.

**Examples:**

```js
const counter = (function () {
  let value = 0;
  return {
    inc() {
      value += 1;
      return value;
    },
  };
})();

counter.inc();
```

**Sources:**

- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>Что такое the singleton pattern in JavaScript?</summary>

### Что такое the singleton pattern in JavaScript?

**Definition:**

Одиночка — один общий экземпляр. В JavaScript его обычно создают один раз в области модуля и экспортируют этот объект, чтобы каждый import получил то же значение.

**How it works:**

- **Область модуля:** `const` верхнего уровня в ES-модуле создаётся один раз, когда модуль вычисляется.
- **То же связывание:** другие файлы, которые его `import`, делят этот один объект. Новый каждому не выдаётся.
- **Форма с замыканием:** IIFE может держать экземпляр в замкнутой переменной и возвращать его из `getInstance`.
- **Не глобаль:** не вешайте экземпляр на `globalThis`, если хватает экспорта модуля.

**When to use:**

Одна общая настройка или кэш страницы. `class` и `new` — когда каждый вызов должен дать отдельный объект.

**Examples:**

```js
const settings = { theme: "dark" };
export function getSettings() {
  return settings;
}
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [javascript.info — Modules, introduction](https://javascript.info/modules-intro)
- [javascript.info — Closure](https://javascript.info/closure)
</details>

<details>
<summary>Объясните the Revealing Module pattern.</summary>

### Объясните the Revealing Module pattern.

**Definition:**

Открывающий модульный паттерн определяет функции в приватной области, затем возвращает объект, чьи свойства указывают на те, что должны быть публичными. Вызывающий видит имена, которые вы выбрали. Остальное он не видит.

**How it works:**

- **Приватное:** функцию, которую не положили на возвращённый объект, снаружи не вызвать.
- **Открытие:** `return { inc: increment }` публикует `increment` под именем `inc`.
- **Замыкание:** публичные методы всё ещё читают приватные связывания `let`.
- **ES-модули:** `export { increment as inc }` — то же открытие, без IIFE.

**Examples:**

```js
const counter = (function () {
  let value = 0;
  function increment() {
    value += 1;
    return value;
  }
  return { inc: increment };
})();
```

**Sources:**

- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
</details>

<details>
<summary>Что такое some best practices for coding in JavaScript?</summary>

### Что такое some best practices for coding in JavaScript?

**Definition:**

Предпочитайте `const`, сравнивайте через `===`, держите модули небольшими и не разбирайте недоверенные строки как HTML. Сбои обрабатывайте на границе, где о них можно сообщить.

**How it works:**

- **Связывания:** `const`, если не переприсваиваете. `let`, если переприсваиваете. Оба с блочной областью и во временной мёртвой зоне до объявления.
- **Равенство:** `===` не приводит типы. `==` приводит, и это прячет ошибки типов.
- **Текст DOM:** пользовательский текст кладите в `textContent`. `innerHTML` разбирает HTML и является частым стоком XSS.
- **Async:** `await` внутри `try` и проверка `response.ok` после `fetch`. Статус 404 промис не отклоняет.
- **Модули:** один файл, явный `export`. Точку входа грузите через `<script type="module">`.

**Examples:**

```js
const title = document.querySelector("#title");
title.textContent = userName;

if (count === 0) {
  title.textContent = "Empty";
}
```

**Sources:**

- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
</details>

### JavaScript Algorithms and Data Structures

<details>
<summary>Как you implement a stack and a queue in JavaScript?</summary>

### Как you implement a stack and a queue in JavaScript?

**Definition:**

Массив может быть стеком или очередью. Стек добавляет и снимает с одного конца (`push` и `pop`). Очередь добавляет с одного конца и снимает с другого (`push` и `shift`).

**How it works:**

- **Стек:** `push` дописывает в конец. `pop` снимает последний элемент и возвращает его. Остальные индексы не двигаются.
- **Очередь:** `push` ставит в конец. `shift` удаляет индекс 0 и сдвигает каждый следующий элемент на одну позицию.
- **Цена:** `shift` переписывает хвост массива на каждом снятии. `push` / `pop` стека более ранние элементы не двигают.
- **Пусто:** `pop` и `shift` возвращают `undefined`, когда массив пуст. Проверяйте `length`, прежде чем считать это значением.

**When to use:**

`push` / `pop` — для стека. `push` / `shift` — для короткой очереди. Для длинной очереди лучше структура, которая не переиндексирует массив на каждом снятии.

**Examples:**

```js
const stack = [];
stack.push("a");
stack.pop();

const queue = [];
queue.push("a");
queue.shift();
```

**Sources:**

- [MDN — Array.prototype.push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
- [MDN — Array.prototype.pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
- [MDN — Array.prototype.shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
</details>

<details>
<summary>Объясните how to sort an array in JavaScript.</summary>

### Объясните how to sort an array in JavaScript.

**Definition:**

`array.sort()` сортирует массив на месте и возвращает тот же массив. Без функции сравнения элементы приводятся к строкам и сравниваются как единицы UTF-16, поэтому `10` оказывается перед `2`.

**How it works:**

- **Функция сравнения:** отрицательное число ставит `a` перед `b`, положительное — `a` после `b`, `0` — они равны. `(a, b) => a - b` сортирует числа по возрастанию, если среди них нет `NaN`.
- **Устойчивость:** с ECMAScript 2019 `sort` устойчив. Равные элементы сохраняют исходный порядок.
- **Сложность:** MDN говорит, что время и память гарантировать нельзя, потому что они зависят от реализации.
- **Копия:** `toSorted()` возвращает новый массив и не меняет исходный. Копия через spread и `sort` делает то же на старых движках.

**When to use:**

`sort` — когда массив нужно изменить. `toSorted` — когда вызывающим ещё нужен старый порядок.

**Examples:**

```js
const numbers = [10, 1, 5];
numbers.sort((a, b) => a - b);

const copy = [...numbers].sort((a, b) => b - a);
```

**Sources:**

- [MDN — Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [MDN — Array.prototype.toSorted()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
</details>

<details>
<summary>Как you check if a string is a palindrome?</summary>

### Как you check if a string is a palindrome?

**Definition:**

Палиндром читается одинаково вперёд и назад. Сравните строку со строкой, полученной разворотом её символов.

**How it works:**

- **Разворот:** разверните строку в массив единиц кода, вызовите `reverse` и `join`.
- **Сравнение:** `===` с исходной строкой. Оно чувствительно к регистру и сохраняет пробелы.
- **Сначала нормализуйте:** если на собеседовании регистр и пробелы не важны, приведите к нижнему регистру и уберите пробелы до сравнения. Делайте это на копии.
- **Два указателя:** идите с обоих концов и остановитесь на первом различии. Вторая строка тогда не нужна.

**Examples:**

```js
function isPalindrome(text) {
  const normalized = text.toLowerCase().replaceAll(" ", "");
  const reversed = [...normalized].reverse().join("");
  return normalized === reversed;
}
```

**Sources:**

- [MDN — String.prototype.replaceAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)
- [MDN — Array.prototype.reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
- [javascript.info — Strings](https://javascript.info/string)
</details>

<details>
<summary>Опишите a Recursive function and provide an example.</summary>

### Опишите a Recursive function and provide an example.

**Definition:**

Рекурсивная функция вызывает сама себя. Нужен базовый случай, который возвращается без следующего вызова, иначе вызовы не кончатся.

**How it works:**

- **База:** самый маленький вход, обработанный напрямую.
- **Рекурсивный случай:** в ту же функцию передают меньший вход и соединяют результат.
- **Стек:** каждый вызов ждёт на стеке, пока внутренний не вернётся. Глубокая цепочка может переполнить стек.
- **Тот же результат, что у цикла:** факториал и обход списка можно написать обоими способами. Цикл не добавляет кадр на шаг.

**Examples:**

```js
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

**Sources:**

- [javascript.info — Recursion and stack](https://javascript.info/recursion)
- [MDN — Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
</details>

<details>
<summary>Что такое the time complexity of JavaScript operations?</summary>

### Что такое the time complexity of JavaScript operations?

**Definition:**

Спецификация языка не назначает операциям стоимость в big-O. Алгоритм выбирает движок. MDN говорит это прямо про `Array.prototype.sort`: время и память гарантировать нельзя, потому что они зависят от реализации.

**How it works:**

- **Чтение и запись по индексу:** плотный массив читает `arr[i]`, не обходя более ранние элементы. Цикл `for` от `0` до `length - 1` посещает каждый индекс один раз.
- **`push` и `pop`:** меняют конец. Более ранние элементы не двигают.
- **`shift` и `unshift`:** удаляют или вставляют по индексу 0 и двигают каждый следующий элемент.
- **`sort`:** одну оценку big-O не цитируют. Передайте функцию сравнения, которая возвращает отрицательное число, положительное или ноль. С ES2019 сортировка устойчива.
- **Ключи объекта:** `Object.entries` обходит собственные перечисляемые строковые ключи. Цепочку прототипов, как `for...in`, он не обходит.

**Examples:**

```js
const values = [3, 1, 2];
values.push(4);
values.sort((a, b) => a - b);
```

**Sources:**

- [MDN — Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [MDN — Array.prototype.shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
- [MDN — Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
</details>

### JavaScript Libraries and Frameworks

<details>
<summary>Что такое the difference between a library and a framework in JavaScript?</summary>

### Что такое the difference between a library and a framework in JavaScript?

**Definition:**

MDN описывает фреймворк как библиотеку, которая предлагает мнения о том, как строить программу. Эти мнения делают большое приложение предсказуемее. Библиотека — код, который вы вызываете для одной задачи и оставляете.

**How it works:**

- **Библиотека:** вы решаете, когда её вызвать. Ранняя роль jQuery, в глоссарии полифилов MDN, — один API поверх различий браузеров.
- **Фреймворк:** в нём есть место для компонентов, состояния и событий, и он обновляет интерфейс, когда состояние меняется. Angular, Vue и связка React с ReactDOM — примеры, которые разбирает MDN.
- **Собственная метка React:** MDN говорит, что сам React — библиотека для отрисовки UI-компонентов. Вместе с ReactDOM его используют как фреймворк.
- **Цена:** фреймворк оправдан, когда трудно держать DOM в согласии с состоянием. Небольшая страница может остаться на API DOM.

**When to use:**

Библиотека — для одной задачи, например дат или HTTP-помощников. Фреймворк — когда работа приложения в том, чтобы меняющийся интерфейс совпадал с состоянием.

**Examples:**

```js
document.querySelector("#count").textContent = String(count);
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
</details>

<details>
<summary>Объясните the Virtual DOM in React.</summary>

### Объясните the Virtual DOM in React.

**Definition:**

Шаг render в React вызывает ваши компоненты, чтобы решить, что должно быть на экране. Шаг commit затем обновляет DOM браузера только там, где этот результат отличается от прошлого. Результат отрисовки в памяти и называют виртуальным DOM.

**How it works:**

- **Запуск:** первый `root.render` или обновление состояния начинает отрисовку.
- **Render:** React вызывает функции компонентов. Отрисовка должна быть чистой: те же входы — тот же выход, и никаких изменений объектов, которые существовали до отрисовки.
- **Commit:** при первой отрисовке React создаёт узлы DOM и вызывает `appendChild`. При следующей применяет минимум операций DOM, который посчитал. Если узел не изменился, React его не трогает.
- **Paint:** браузер рисует после commit. Документация React называет это painting, а не rendering.

**Examples:**

```js
import { createRoot } from "react-dom/client";

function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}

createRoot(document.getElementById("root")).render(<Greeting name="Ada" />);
```

**Sources:**

- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
</details>

<details>
<summary>Как data binding work in Angular?</summary>

### Как data binding work in Angular?

**Definition:**

Angular — компонентный фреймворк. Вы пишете декларативные HTML-шаблоны, а компилятор фреймворка превращает их в JavaScript. Шаблон показывает данные компонента, а события в шаблоне эти данные меняют.

**How it works:**

- **Компоненты:** интерфейс — дерево компонентов. У каждого есть шаблон.
- **Шаблоны:** MDN говорит, что Angular опирается на HTML-шаблоны и компилирует их в оптимизированный JavaScript. Вызовы `createElement` для этого вида руками не пишут.
- **TypeScript:** приложения Angular пишут на TypeScript, который добавляет типы поверх JavaScript.
- **Два направления:** шаблон читает состояние компонента, чтобы отрисовать его, а события DOM вызывают методы компонента, которые меняют состояние. Следующая отрисовка показывает новое состояние.

**Examples:**

```js
@Component({
  selector: "app-hello",
  template: `<button (click)="name = 'Ada'">{{ name }}</button>`,
})
export class Hello {
  name = "there";
}
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [Angular — Binding](https://angular.dev/guide/templates/binding)
</details>

<details>
<summary>Что такое Vue.js and what sets it apart from other frameworks?</summary>

### Что такое Vue.js and what sets it apart from other frameworks?

**Definition:**

Vue — фреймворк JavaScript для интерфейсов. MDN ставит его в ряд широко используемых фреймворков: он расширяет HTML своим синтаксисом шаблонов и в остальном использует обычный JavaScript.

**How it works:**

- **Происхождение:** Evan You выпустил Vue в 2014 году после работы над AngularJS.
- **Шаблоны:** как Angular, Vue добавляет в HTML свои пометки. Остальная часть компонента — обычный JavaScript.
- **Отличие от React:** MDN отмечает, что React расширяет JavaScript через JSX, а Vue сохраняет HTML-шаблон.
- **Масштаб:** Vue можно смонтировать на один элемент уже существующей страницы. Фреймворк всё равно остаётся библиотекой с мнением: он решает, как эта часть интерфейса держится в согласии с состоянием.

**Examples:**

```js
import { createApp } from "vue";

createApp({
  data() {
    return { name: "Ada" };
  },
  template: `<p>Hello, {{ name }}</p>`,
}).mount("#app");
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [Vue — Introduction](https://vuejs.org/guide/introduction.html)
</details>

<details>
<summary>Можете ли вы describe the jQuery library and its applications?</summary>

### Можете ли вы describe the jQuery library and its applications?

**Definition:**

jQuery — библиотека JavaScript, которую вызываете вы. Глоссарий полифилов MDN описывает первую версию как набор обходных путей под разные браузеры, давший разработчикам один API. Структурой приложения она не владеет, в отличие от фреймворка.

**How it works:**

- **Один API:** ранние браузеры по-разному реализовывали DOM. jQuery прятал различия за вызовами, которые делаете вы.
- **Вы её вызываете:** библиотека ждёт, пока ваш код её вызовет. Введение MDN во фреймворки отделяет это от фреймворка: библиотека плюс мнения о том, как строить приложение.
- **Сегодня:** `querySelector`, `addEventListener` и `fetch` закрывают задачи, для которых часто брали jQuery. Полифил медленнее нативной возможности, поэтому MDN предпочитает нативную реализацию, когда она есть.
- **Всё ещё библиотека:** существующая страница может оставить jQuery ради плагинов, которые его ждут, не принимая UI-фреймворк.

**Examples:**

```js
const title = document.querySelector("#title");
title.textContent = "Notes";
title.addEventListener("click", () => {
  title.hidden = true;
});
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
</details>

### JavaScript ES2017+ and Beyond

<details>
<summary>Что такое async iterators and generators?</summary>

### Что такое async iterators and generators?

**Definition:**

Асинхронный итератор выдаёт значения со временем. Каждый `next()` возвращает промис. `for await...of` ждёт этот промис на каждом шаге. Асинхронный генератор — это `async function*`, который выдаёт значения через `yield`.

**How it works:**

- **`for await`:** тело цикла выполняется после завершения каждого обещанного результата. Отклонение бросает исключение внутри цикла.
- **Асинхронный генератор:** `async function*` может `await` между yield. Вызов возвращает асинхронный итератор.
- **Обычный генератор:** `function*` и `yield` — та же идея без промисов. Его потребляет `for...of`.
- **Потоки:** читаемый поток — частый асинхронный итерируемый объект. `for await` читает куски по мере прихода.

**Examples:**

```js
async function* count() {
  yield 1;
  yield 2;
}

for await (const n of count()) {
  console.log(n);
}
```

**Sources:**

- [MDN — for await...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of)
- [javascript.info — Async iterators and generators](https://javascript.info/async-iterators-generators)
</details>

<details>
<summary>Что такое the puRPOse of the async keyword?</summary>

### Что такое the puRPOse of the async keyword?

**Definition:**

`async` помечает функцию, которая возвращает промис. Внутри неё `await` приостанавливает функцию, пока ожидаемый промис не завершится, и продолжает её как микрозадачу.

**How it works:**

- **Возврат:** возвращённое значение становится выполненным промисом. Брошенное исключение отклоняет этот промис.
- **`await`:** законен внутри `async`-функции (и на верхнем уровне модуля). Отклонение бросает на `await`, поэтому его ловит `try` / `catch`.
- **Те же очереди:** продолжение — реакция промиса. Оно идёт раньше следующей задачи таймера.
- **Вызывающий:** сразу получает промис. Итоговое значение он получит, только если сделает await или `.then`.

**Examples:**

```js
async function load() {
  const response = await fetch("/notes");
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN — await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [javascript.info — Async/await](https://javascript.info/async-await)
</details>

<details>
<summary>Можете ли вы explain the use of Object.entries() and Object.values()?</summary>

### Можете ли вы explain the use of Object.entries() and Object.values()?

**Definition:**

`Object.entries(obj)` возвращает массив собственных перечисляемых пар со строковыми ключами, каждая пара — `[key, value]`. `Object.values(obj)` возвращает только эти значения. Цепочку прототипов ни один не обходит.

**How it works:**

- **Собственные и перечисляемые:** неперечисляемое свойство пропускается. Унаследованные пропускаются. В этом отличие от `for...in`, который цепочку прототипов посещает.
- **Строковые ключи:** символьные ключи не входят. Ключи, похожие на целые, возвращаются строками, в том же порядке, что у `for...in`.
- **`Object.keys`:** только ключи. `entries` — когда нужны оба, `values` — когда нужны только значения.
- **Примитивы:** `undefined` и `null` бросают `TypeError`. Строка становится парами индексов. Другие примитивы дают пустой массив.

**When to use:**

`entries` — чтобы обойти ключи и значения или собрать `Map`. `values` — когда ключи не важны.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
Object.entries(user);
Object.values(user);
new Map(Object.entries(user));
```

**Sources:**

- [MDN — Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
- [MDN — Object.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)
</details>

<details>
<summary>Как JavaScript handle big integers (BigInt)?</summary>

### Как JavaScript handle big integers (BigInt)?

**Definition:**

`BigInt` — примитив для целых, которые не помещаются в `Number`. Пишут `9007199254740991n` или вызывают `BigInt(value)` без `new`. `typeof 1n` равен `"bigint"`.

**How it works:**

- **Нельзя смешивать в арифметике:** `+`, `-`, `*`, `/`, `%` и `**` требуют, чтобы оба операнда были BigInt или ни один. `1n + 1` бросает. Деление отсекает к нулю.
- **Сравнения:** `>`, `<`, `>=`, `<=` и `==` могут смешивать BigInt и Number. `1n === 1` ложно. `1n == 1` истинно.
- **Нет операторов:** унарный `+` и беззнаковый `>>>` BigInt не принимают.
- **JSON:** `JSON.stringify` бросает на BigInt, если replacer или метод `toJSON` не превратит его в то, что JSON умеет хранить.
- **Ложен только `0n`.** Методы `Math` BigInt не принимают.

**When to use:**

Целое, которое может превысить `Number.MAX_SAFE_INTEGER` (`2 ** 53 - 1`). `Number` — для обычных счётчиков и дробных значений.

**Examples:**

```js
const max = BigInt(Number.MAX_SAFE_INTEGER);
const next = max + 1n;
next === max + 1n;
```

**Sources:**

- [MDN — BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
</details>

<details>
<summary>Что такое dynamic imports in JavaScript?</summary>

### Что такое dynamic imports in JavaScript?

**Definition:**

Динамический импорт — это вызов `import()`. Он загружает модуль, когда выполняется эта строка, и возвращает промис экспортов модуля. Статический `import` поднимается и выполняется до тела модуля.

**How it works:**

- **Вызов:** `await import("./editor.js")` даёт пространство имён модуля. Именованные экспорты — свойства. Экспорт по умолчанию — `.default`.
- **Когда:** запрос начинается в момент вызова, не при разборе. Файл не попадает в первый бандл, если сборщик режет по `import()`.
- **Голые имена:** в браузере `import("lodash")` бросает `TypeError`, если имя не разрешает import map. Node может разрешить голый спецификатор по файловой системе.
- **Сбой:** сетевая ошибка или ошибка разбора отклоняет промис. Ловите её через `try` / `catch`.

**When to use:**

Статический `import` — для кода, который странице нужен всегда. `import()` — для маршрута, локали или тяжёлого редактора.

**Examples:**

```js
async function openEditor() {
  const editor = await import("./editor.js");
  editor.mount(document.querySelector("#editor"));
}
```

**Sources:**

- [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

### Browser Compatibility and Transpilation

<details>
<summary>Как you ensure your JavaScript code is cross-browser compatible?</summary>

### Как you ensure your JavaScript code is cross-browser compatible?

**Definition:**

Пишите на стандартных API, проверьте, что нужные браузеры их реализуют, и добавляйте полифил или шаг компиляции только для измеренного пробела. Таблицы совместимости MDN — справочник того, что уже вышло.

**How it works:**

- **Проверка возможности:** убедитесь, что функция есть (`typeof fetch === "function"`), прежде чем звать её. Не ветвитесь по имени браузера.
- **Полифил:** MDN определяет полифил как код, который даёт возможность, которой нет в старом браузере. Нативный код быстрее, поэтому полифил отдают только там, где возможности нет.
- **Синтаксис:** транспилятор вроде Babel переписывает новый синтаксис для старых движков. Сам по себе он не добавляет отсутствующие методы среды.
- **Проверьте движки:** откройте страницу больше чем в одном браузере. Сигнал — исключение в консоли, а не догадка о доле рынка.

**Examples:**

```js
async function load(url) {
  if (typeof fetch !== "function") {
    throw new Error("fetch is not available");
  }
  const response = await fetch(url);
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Babel — preset-env](https://babeljs.io/docs/babel-preset-env)
</details>

<details>
<summary>Что такое Babel and how is it used in JavaScript development?</summary>

### Что такое Babel and how is it used in JavaScript development?

**Definition:**

Babel — компилятор, который переписывает новый синтаксис JavaScript в код, который разбирают старые движки. `@babel/preset-env` выбирает преобразования по браузерам, которые вы перечислили.

**How it works:**

- **Синтаксис, не отсутствующие методы:** Babel может превратить стрелочные функции, классы и optional chaining в старый синтаксис. Отсутствующий `Array.prototype.toSorted` всё равно нуждается в полифиле или проверке.
- **preset-env:** вы называете цели. Пресет пропускает преобразования, которые эти цели уже понимают.
- **JSX:** проекты React также используют Babel (или другой компилятор), чтобы превратить JSX в `createElement` или вызовы автоматического runtime.
- **Source map:** оставляйте их, чтобы DevTools показывал исходные строки, а не только порождённый файл.

**When to use:**

Когда синтаксис, который вы пишете, новее движка, который ещё поддерживаете. Пропустите, если каждая цель уже разбирает этот синтаксис.

**Examples:**

```js
const greet = (name) => `Hello, ${name}`;
```

**Sources:**

- [Babel — preset-env](https://babeljs.io/docs/babel-preset-env)
- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
</details>

<details>
<summary>Что такое Polyfills, and when would you use them?</summary>

### Что такое Polyfills, and when would you use them?

**Definition:**

Полифил — код, обычно JavaScript, который даёт возможность, которую старый браузер не реализует. Пример MDN — подставить поведение, которого нет в нативном движке, например старую замену CSS- или языковой возможности.

**How it works:**

- **Проверьте, потом заполните:** если нет `Array.prototype.toSorted`, поставьте функцию, которая возвращает отсортированную копию. Если она есть, ничего не делайте.
- **Хуже нативного:** MDN говорит, что нативные реализации умеют больше и быстрее. Полифил `Object.create` покрывает только то, что может ненативная версия.
- **Не Babel:** Babel переписывает синтаксис. Полифил добавляет метод среды. Могут понадобиться оба.
- **История jQuery:** MDN называет первый jQuery ранним полифилом: один API поверх браузеров, которые расходились.

**When to use:**

Старый браузер, который вы ещё поддерживаете, не имеет одного метода. Не отдавайте полифил браузерам, у которых метод уже нативный.

**Examples:**

```js
if (!Array.prototype.toSorted) {
  Array.prototype.toSorted = function (compare) {
    return [...this].sort(compare);
  };
}
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Array.prototype.toSorted()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
</details>

### JavaScript and the DOM

<details>
<summary>Что такое the Document Object Model (DOM)?</summary>

### Что такое the Document Object Model (DOM)?

**Definition:**

DOM — объектное дерево браузера для загруженной страницы. JavaScript читает и меняет страницу вызовами методов этих объектов. Спецификация языка DOM не определяет; его определяет браузер.

**How it works:**

- **Дерево:** элементы, текст и комментарии — узлы. `document` — точка входа. `document.documentElement` — корневой элемент.
- **Живая страница:** изменение узла меняет то, что показывает браузер. HTML-файл заново не порождают.
- **События:** узлы реализуют цели событий. Слушатели видят фазы захвата, цели и всплытия.
- **Не исходник:** `innerHTML` — сериализация или разбор разметки. Сам DOM — это объекты.

**Examples:**

```js
const root = document.documentElement;
const title = document.querySelector("h1");
title.textContent = "Notes";
```

**Sources:**

- [MDN — Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [MDN — Document.documentElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement)
</details>

<details>
<summary>Как you create, append, or remove an element from the DOM?</summary>

### Как you create, append, or remove an element from the DOM?

**Definition:**

`document.createElement(tag)` создаёт элемент, которого ещё нет в документе. `appendChild` вставляет узел последним ребёнком. `element.remove()` вынимает этот элемент из родителя.

**How it works:**

- **Создание:** у нового элемента нет родителя, пока вы его не вставите. `textContent` или атрибуты задают до или после вставки.
- **`appendChild`:** переносит узел, если он уже в дереве. Возвращает вставленный узел.
- **`remove`:** элемент покидает дерево. Слушатели на нём уходят вместе с ним. Другие ссылки, которые вы сохранили, всё ещё указывают на объект.
- **Текст:** для обычного текста берите `textContent`. `innerHTML` разбирает разметку.

**Examples:**

```js
const item = document.createElement("li");
item.textContent = "Ada";
list.appendChild(item);
item.remove();
```

**Sources:**

- [MDN — Document.createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement)
- [MDN — Node.appendChild()](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)
- [MDN — Element.remove()](https://developer.mozilla.org/en-US/docs/Web/API/Element/remove)
</details>

<details>
<summary>Опишите different ways to find or access HTML elements in the DOM.</summary>

### Опишите different ways to find or access HTML elements in the DOM.

**Definition:**

Элементы ищут через `querySelector`, `querySelectorAll`, `getElementById` или живые коллекции по тегу и классу. Начинают с `document` или с элемента, который уже есть.

**How it works:**

- **`querySelector`:** первое совпадение CSS-селектора или `null`. Неверный селектор бросает `SyntaxError`.
- **`querySelectorAll`:** статический `NodeList` всех совпадений.
- **`getElementById`:** один элемент или `null`.
- **`getElementsByTagName` / `getElementsByClassName`:** живая `HTMLCollection`, которая обновляется при изменении DOM.
- **От узла:** `element.querySelector` ищет внутри этого элемента. `parentElement` и `children` идут от узла, который у вас есть.

**Examples:**

```js
const form = document.querySelector("form.login");
const email = form.querySelector("input[name='email']");
const title = document.getElementById("title");
```

**Sources:**

- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [javascript.info — Searching: getElement*, querySelector*](https://javascript.info/searching-elements-dom)
</details>

<details>
<summary>Объясните the difference between innerHTML and textContent.</summary>

### Объясните the difference between innerHTML and textContent.

**Definition:**

`textContent` читает и пишет обычный текст. `innerHTML` читает и пишет HTML-разметку. Присваивание строки в `innerHTML` разбирает её и является частым стоком XSS.

**How it works:**

- **Чтение `textContent`:** склейка текста потомков, включая `script` и `style`. Комментарии не входят. Узел документа возвращает `null`; для всей страницы берите `document.documentElement.textContent`.
- **Запись `textContent`:** снимает детей и вставляет один текстовый узел. Теги в строке остаются текстом.
- **`innerHTML`:** геттер сериализует потомков как HTML. Сеттер разбирает строку и заменяет детей. Вставленный так `<script>` не выполняется, но `<img onerror>` всё ещё может запустить код.
- **`innerText`:** пропускает скрытый текст и может вызвать reflow. `textContent` — нет.

**When to use:**

`textContent` — для текста, включая всё, что набрал пользователь. `innerHTML` — только для разметки, которой доверяете, или для значения `TrustedHTML`, когда включены Trusted Types.

**Examples:**

```js
title.textContent = userName;

// parses HTML; unsafe for untrusted userName:
// box.innerHTML = userName;
```

**Sources:**

- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
- [MDN — Element.innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
</details>

<details>
<summary>Как you handle DOM events in a memory-efficient way?</summary>

### Как you handle DOM events in a memory-efficient way?

**Definition:**

Вешайте один слушатель на устойчивого родителя и обрабатывайте всплывшие события детей. Снимайте слушатель, когда узел уходит, той же ссылкой на функцию, которую добавляли.

**How it works:**

- **Делегирование:** `event.target` — ребёнок. `event.currentTarget` — родитель, на котором висит слушатель. Новым детям свои слушатели не нужны.
- **Фильтр:** `event.target.closest("li")` игнорирует клики не по строке.
- **Снятие:** `removeEventListener` нужны та же функция и тот же флаг захвата. `once: true` снимает себя после первого вызова. `AbortSignal` снимает все связанные слушатели, когда вы вызываете abort.
- **Не держите утечку:** слушатель на `document`, который замыкает отсоединённый узел, держит этот узел живым. Снимите его, когда вид закрывается.

**Examples:**

```js
const list = document.querySelector("ul");

function onClick(event) {
  const item = event.target.closest("li");
  if (!item || !list.contains(item)) return;
  item.classList.toggle("done");
}

list.addEventListener("click", onClick);
```

**Sources:**

- [javascript.info — Event delegation](https://javascript.info/event-delegation)
- [MDN — Event.target](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)
- [MDN — EventTarget.removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)
</details>

### Tooling and Build Systems

<details>
<summary>Что такое npm, and how do you use it?</summary>

### Что такое npm, and how do you use it?

**Definition:**

npm — менеджер пакетов, который поставляется с Node.js. Он ставит пакеты, описанные в `package.json`, и может запускать скрипты из этого файла.

**How it works:**

- **`package.json`:** имя проекта, его зависимости и скрипты вроде `test` и `start`.
- **`npm install`:** читает этот файл и пишет `node_modules`. Lockfile фиксирует версии, которые были разрешены.
- **`npm install name`:** добавляет зависимость. `npm run test` запускает скрипт `test`.
- **Реестр:** публичный реестр — источник по умолчанию. Yarn и pnpm — другие клиенты реестров пакетов; npm — тот, который ставит Node.

**Examples:**

```js
{
  "scripts": {
    "test": "node --test"
  }
}
```

**Sources:**

- [npm — About npm](https://docs.npmjs.com/about-npm)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Обсудите the role of Webpack in modern JavaScript development.</summary>

### Обсудите the role of Webpack in modern JavaScript development.

**Definition:**

Webpack — сборщик. Он начинает со входного модуля, идёт по `import` и `require` и выдаёт один или несколько бандлов, которые может загрузить браузер. Руководство MDN по модулям говорит, что сборщики всё ещё нужны для минификации, удаления мёртвого кода и нарезки, хотя браузеры выполняют модули сами.

**How it works:**

- **Граф:** каждый `import` становится зависимостью. Бандл содержит этот граф, а не россыпь тегов script.
- **Оптимизация:** минификация, выброс неиспользуемых экспортов и отдельный файл для динамического `import()`.
- **Ресурсы:** лоадеры могут включить CSS и картинки в ту же сборку. Это работа Webpack, не языка.
- **Не обязателен:** маленькая страница может использовать `<script type="module">` без сборщика.

**When to use:**

Продакшен-приложение со многими модулями, CSS и разделом между первым экраном и поздними маршрутами. Пропустите его для одного файла-модуля.

**Examples:**

```js
import { area } from "./square.js";
area(4);
```

**Sources:**

- [webpack — Concepts](https://webpack.js.org/concepts/)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>Что такое source Map?</summary>

### Что такое source Map?

**Definition:**

Source map — файл, который сопоставляет порождённый JavaScript исходникам. DevTools использует его, чтобы точка останова и стек показывали ваши файлы, а не только минифицированный бандл.

**How it works:**

- **Зачем:** минификаторы и компиляторы переименовывают локальные имена и склеивают файлы. Строку стека в бандле трудно сопоставить со строкой исходника.
- **DevTools:** когда карта загружена, панель Sources показывает исходные модули.
- **Отдавайте осторожно:** публичная карта раскрывает исходный код. Отдавайте её только тем, кто отлаживает прод, или не кладите на публичный сайт.
- **Babel и сборщики:** они могут выдать карту рядом с порождённым файлом.

**Examples:**

```js
const greet = (name) => `Hello, ${name}`;
```

**Sources:**

- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
</details>

<details>
<summary>Как you use ESLInt for maintaining JavaScript code quality?</summary>

### Как you use ESLInt for maintaining JavaScript code quality?

**Definition:**

ESLint — линтер. Он читает JavaScript и сообщает об образцах, которые вы настроили как проблемы, например о неиспользуемом связывании, до запуска кода.

**How it works:**

- **Конфиг:** flat config или файл eslintrc перечисляет правила и файлы, к которым они относятся.
- **Запуск:** `npx eslint .` печатает нарушения. Код выхода проваливает скрипт, если проблемы остались.
- **Правка:** `eslint --fix` применяет правила, которые знают безопасную правку, например форматирование части замечаний. Логику он не чинит.
- **CI:** запускайте ту же команду на каждый push, чтобы нарушение не ждало ревью.

**Examples:**

```js
const count = 1;
console.log(count);
```

**Sources:**

- [ESLint — Getting started](https://eslint.org/docs/latest/use/getting-started)
- [npm — About npm](https://docs.npmjs.com/about-npm)
</details>

<details>
<summary>Что такое continuous integration/continuous deployment (CI/CD) in the context of JS development?</summary>

### Что такое continuous integration/continuous deployment (CI/CD) in the context of JS development?

**Definition:**

Непрерывная интеграция гоняет проверки проекта на каждый push: установка зависимостей, линт, тесты и сборка. Непрерывная доставка публикует сборку, которая эти проверки прошла.

**How it works:**

- **Установка:** `npm install` из lockfile, чтобы CI взял те же версии, что и автор.
- **Тест:** `node --test` или тестовый скрипт проекта. Учебные материалы MDN по тестированию — про проверки, которые выполняет этот шаг.
- **Линт и сборка:** ESLint, затем сборщик или сборка фреймворка. Ненулевой код выхода останавливает пайплайн.
- **Выкладка:** публикуют только артефакт зелёного прогона. Упавший тест до пользователей не доходит.

**Examples:**

```js
{
  "scripts": {
    "test": "node --test",
    "lint": "eslint ."
  }
}
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [ESLint — Getting started](https://eslint.org/docs/latest/use/getting-started)
</details>

### JavaScript and the Web Platform

<details>
<summary>Что такое the Window object and its significance?</summary>

### Что такое the Window object and its significance?

**Definition:**

`window` — глобальный объект страницы в браузере. DOM, location, history, хранилище и таймеры — свойства этого окна. У каждой вкладки и у каждого iframe своё.

**How it works:**

- **Глобалы:** к `document`, `location`, `history`, `localStorage`, `setTimeout` и `fetch` обращаются через окно. Голый `document` — то же, что `window.document`.
- **Один документ:** `window.document` — страница, загруженная в этом окне.
- **События:** `popstate` и многие события загрузки срабатывают на `window`.
- **Не Node:** у программы Node есть `globalThis`, а не браузерный `window`. `document` там нет.

**Examples:**

```js
window.addEventListener("popstate", (event) => {
  console.log(event.state);
});
```

**Sources:**

- [MDN — Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)
- [MDN — Window: popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)
</details>

<details>
<summary>Объясните the Document object.</summary>

### Объясните the Document object.

**Definition:**

`document` — это `Document` страницы, загруженной в окне. Он — вход в дерево DOM: с него ищут, создают и слушают.

**How it works:**

- **Корень:** `document.documentElement` — элемент `<html>`. `document.body` — body, когда он уже есть.
- **Поиск:** `querySelector`, `querySelectorAll` и `getElementById` начинают с документа.
- **Создание:** `createElement` делает узел, которого нет в дереве, пока его не добавят.
- **Весь текст:** `document.textContent` равен `null`. Для текста страницы берите `document.documentElement.textContent`.

**Examples:**

```js
const title = document.querySelector("h1");
const item = document.createElement("li");
item.textContent = "Ada";
document.querySelector("ul").appendChild(item);
```

**Sources:**

- [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [MDN — Document.documentElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement)
- [MDN — Document.createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement)
</details>

<details>
<summary>Какие new features HTML5 приносит в JavaScript development?</summary>

### Какие new features HTML5 приносит в JavaScript development?

**Definition:**

HTML5 — поколение HTML, которое добавило элементы и API браузера, к которым скрипт обращается напрямую: медиа, canvas, хранилище и история. JavaScript страницы вызывает эти API на `document` и `window`.

**How it works:**

- **Элементы:** `<video>`, `<audio>` и `<canvas>` — объекты, которыми управляют из скрипта после выбора.
- **Хранилище:** `localStorage` и `sessionStorage` держат строки источника и не шлют их как cookie.
- **История:** `history.pushState` обновляет историю сессии и не загружает новый документ.
- **Семантика:** элементы вроде `<article>` и `<section>` — обычные элементы DOM. Скрипт находит их теми же селекторами, что и любой другой тег.

**Examples:**

```js
const canvas = document.querySelector("canvas");
const context = canvas.getContext("2d");
context.fillRect(0, 0, 10, 10);

localStorage.setItem("theme", "dark");
```

**Sources:**

- [MDN — HTML5](https://developer.mozilla.org/en-US/docs/Glossary/HTML5)
- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN — History.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)
</details>

<details>
<summary>Обсудите the role of JavaScript in Progressive Web Apps (PWAs).</summary>

### Обсудите the role of JavaScript in Progressive Web Apps (PWAs).

**Definition:**

PWA — сайт, который можно установить и который может работать ближе к приложению платформы. Его JavaScript — всё ещё скрипт страницы на `window`, плюс необязательный service worker для офлайна и фоновой работы.

**How it works:**

- **Та же платформа:** MDN говорит, что PWA собран веб-технологиями. У страницы есть HTML, CSS и JavaScript, и скрипт видит обычные Web API.
- **Манифест:** манифест веб-приложения сообщает браузеру имя и значок, нужные для установки.
- **Service worker:** необязателен и часто используется для минимального офлайна. Браузер может запустить его в фоне, например чтобы обработать push.
- **Не обязан быть SPA:** PWA может состоять из нескольких страниц. Одностраничное приложение само по себе не PWA.

**Examples:**

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/sw.js");
}
```

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

### JavaScript and Mobile Development

<details>
<summary>Объясните how to use JavaScript for mobile development.</summary>

### Объясните how to use JavaScript for mobile development.

**Definition:**

На телефоне JavaScript по-прежнему работает как веб-страница, как устанавливаемое PWA или внутри React Native, который рисует нативные виды вместо DOM. Язык тот же. API хоста — нет.

**How it works:**

- **Адаптивный сайт:** страница — HTML, CSS и JavaScript в браузере телефона. События касания и meta viewport — часть этой страницы.
- **PWA:** тот же сайт можно установить, запускать отдельно и использовать service worker для офлайна.
- **React Native:** компоненты описывают интерфейс, а нативная сторона создаёт виды платформы. `document` нет.
- **Выбор по хосту:** API браузера — для сайта. Компоненты React Native — когда продукт является приложением магазина с нативными экранами.

**When to use:**

Сайт или PWA, когда хватает URL. React Native, когда интерфейс — нативные виды на iOS и Android.

**Examples:**

```js
const button = document.querySelector("button");
button.addEventListener("click", () => {
  button.textContent = "Saved";
});
```

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
</details>

<details>
<summary>Что такое React Native and how does it differ from traditional web apps?</summary>

### Что такое React Native and how does it differ from traditional web apps?

**Definition:**

React Native собирает мобильные приложения компонентами React. Обычное веб-приложение рисует HTML в DOM браузера. React Native рисует нативные виды на iOS и Android. В этом хосте DOM нет.

**How it works:**

- **Общая идея:** вы пишете компоненты и обновляете состояние. Модель React «реши интерфейс, затем примени» всё ещё описывает render и commit.
- **Веб:** `createRoot` из `react-dom` кладёт элементы в узел DOM. Результат — HTML-элементы.
- **Натив:** тот же стиль компонентов соответствует видам платформы. Раскладка — не CSS в окне браузера.
- **Замечание MDN:** React вместе с React Native нацелен на мобильные устройства; React вместе с ReactDOM — на веб.

**Examples:**

```js
import { createRoot } from "react-dom/client";

createRoot(document.getElementById("root")).render(<h1>Web</h1>);
```

**Sources:**

- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
</details>

<details>
<summary>Как JavaScript interact with native mobile components?</summary>

### Как JavaScript interact with native mobile components?

**Definition:**

В React Native JavaScript описывает экран компонентами вроде `View` и `Text`. Нативная сторона создаёт соответствующие виды платформы. Скрипт не вызывает `document.createElement`.

**How it works:**

- **Компоненты:** функциональный компонент возвращает другие компоненты. Обновление состояния планирует новую отрисовку — та же идея React, что и в вебе.
- **Хост:** iOS и Android дают настоящие элементы управления. React Native связывает описание отрисовки с этими элементами.
- **Не DOM:** нет `innerHTML` и нет движка раскладки CSS браузера. Путь веб-приложения через `document` не применяется.
- **PWA остаётся вебом:** если продукт — сайт, JavaScript говорит с DOM и Web API, включая возможности устройства, которые открывает браузер, и не создаёт нативные виды.

**Examples:**

```js
import { Text, View } from "react-native";

export function Hello() {
  return (
    <View>
      <Text>Hello</Text>
    </View>
  );
}
```

**Sources:**

- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

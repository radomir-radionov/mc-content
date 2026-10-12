# TypeScript — Основы

Статическая проверка типов, компилятор, tsconfig и краткое введение для JavaScript-разработчиков.

[← Назад к хабу](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Static typing & basics](#static-typing-basics)
- [Compiler & tsconfig](#compiler-tsconfig)
- [tsconfig.json](#tsconfig-json)
- [Declaration files (.d.ts)](#declaration-files-dts)
- [Design principles](#design-principles)
- [Modern TypeScript](#modern-typescript)

---

## Static typing & basics

<details>
  <summary>Compiler fundamentals</summary>

### Static Type-Checking

**Definition:**

Анализ кода во время компиляции без его запуска — TS проверяет типы и сообщает об ошибках перед развертыванием.

| Когда использовать                                                                                                                        | Заметки                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Бегать `tsc` или полагаться на диагностику IDE. Исправьте ошибки перед отправкой; использовать `--noEmit` в CI только для проверки типов. | TypeScript по умолчанию не останавливает работу вашего JS. Он сообщает об ошибках, но по-прежнему генерирует JavaScript, если вы не выберете более строгие правила генерации. Думайте об этом как о линтере со сверхспособностями, а не о среде выполнения e |

**Examples:**

```typescript
const message = "hello!";
message(); // Error: Type 'string' has no call signatures
// Caught before you ever run the code
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Non-Exception Failures

**Definition:**

Ошибки типа, которые не вызывают исключений — TS сообщает о них во время компиляции.

| Когда использовать                                                                                     | Заметки                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Прочтите закорючки IDE и `tsc` выход. Исправить перед слиянием; они не появятся в журналах выполнения. | TS иногда отклоняет _действительный_ JavaScript, который не приводит к немедленному сбою (например, доступ к `user.location` когда это не по типу). Это сделано намеренно — цель — выявить настоящие ошибки, а не зеркалировать e |

**Examples:**

```typescript
const user = { name: "Daniel", age: 26 };
user.location; // Error in TS — undefined in plain JS
```

```typescript
const announcement = "Hello World!";
announcement.toLocaleLowercase(); // Error — typo caught
announcement.toLocalLowerCase(); // Error — typo caught
announcement.toLocaleLowerCase(); // OK
```

```typescript
function flipCoin() {
  return Math.random < 0.5; // Error — forgot () on Math.random
  // return Math.random() < 0.5; // OK
}
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Types for Tooling

**Definition:**

Типы существуют в первую очередь для обеспечения функций редактора — автозаполнения, рефакторинга, перехода к определению.

| Когда использовать                                                                                             | Заметки                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Наведите указатель мыши на типы, используйте символ переименования и найдите ссылки; хорошие типы = лучший DX. | Вы получаете преимущества инструментов еще до того, как ваш код будет полностью напечатан. Частичный ввод по-прежнему помогает автозаполнению и навигации. Чем больше вы печатаете, тем умнее становится редактор. |

**Examples:**

```typescript
import express from "express";

const app = express();

app.get("/", function (req, res) {
  res.send; // editor suggests: send, sendDate, sendFile, sendStatus...
});

app.listen(3000);
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### `tsc` — The TypeScript Compiler

**Definition:**

CLI, который проверяет тип TypeScript и генерирует JavaScript (`typescript` упаковка).

| Когда использовать                                                                                                                                | Заметки                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Бегать `tsc` с `tsconfig.json`, or `tsc --noEmit` только проверить. Бандлеры (Vite, esbuild) часто транспилируются; `tsc` все еще проверяет типы. | `tsc` выполняет две работы одновременно: проверку типов И компиляцию TS → JS. Многие современные установки используют `tsc --noEmit` только для проверки и позвольте Vite/esbuild/SWC обрабатывать фактическое объединение. Узнайте, какой инструмент что делает |

**Examples:**

```typescript
// hello.ts
console.log("Hello world!");
```

```bash
tsc hello.ts
# No console output = no type errors
# Creates hello.js alongside hello.ts
```

```typescript
function greet(person, date) {
  console.log(`Hello ${person}, today is ${date}!`);
}

greet("Brendan"); // Error: Expected 2 arguments, but got 1
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Emitting with Errors

**Definition:**

По умолчанию `tsc` выдает JS даже при наличии ошибок типа.

| Когда использовать                                                                          | Заметки                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Набор `"noEmitOnError": true` в CI/производственных сборках сломанные типы блокируют вывод. | У тебя может быть сломанный `.ts` файл и недавно обновленный `.js` файл рядом. Использовать `--noEmitOnError` or `"noEmitOnError": true` in `tsconfig.json` когда вы хотите, чтобы компилятор отказывался от вывода до тех пор, пока er |

**Examples:**

```bash
# Default: emits JS despite errors
tsc hello.ts

# Strict emit: no JS file if errors exist
tsc --noEmitOnError hello.ts
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Explicit Types

**Definition:**

Введите аннотации, которые вы пишете сами, вместо того, чтобы полагаться на логические выводы.

| Когда использовать                                                                                         | Заметки                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Аннотировать параметры функции на границах общедоступного API; пусть вывод обрабатывает локальных жителей. | `Date()` (без `new`) возвращает **строку** в JavaScript, а не `Date` объект. TypeScript ловит `greet("Maddison", Date())` как ошибка. Вам нужно `new Date()`. |

**Examples:**

```typescript
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("Maddison", new Date()); // OK

// greet("Maddison", Date());     // Error: string not assignable to Date
// greet("Maddison");             // Error: expected 2 arguments
```

```typescript
let msg = "hello there!";
// TS infers: string — same as writing `let msg: string`
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Erased Types

**Definition:**

Вся информация о типах удаляется при компиляции TS в JS — нулевой след во время выполнения.

| Когда использовать                                                                                                         | Заметки                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Не ожидайте `typeof` или время выполнения `instanceof` по интерфейсам; используйте библиотеки проверки для внешних данных. | Аннотации типов никогда не меняют поведение во время выполнения. Если TS говорит, что все в порядке, но ваши типы неверны (например, неверное утверждение), вы все равно получите сбой во время выполнения. Типы используются только во время компиляции. |

**Examples:**

```typescript
// TypeScript source
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
greet("Maddison", new Date());
```

```javascript
// Emitted JavaScript — types gone
"use strict";
function greet(person, date) {
  console.log(
    "Hello ".concat(person, ", today is ").concat(date.toDateString(), "!"),
  );
}
greet("Maddison", new Date());
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

### Downleveling

**Definition:**

Переписывание нового синтаксиса JS для старых целей (например, ES2020 → ES5).

| Когда использовать                                                                                                   | Заметки                                                                                                                                                                                                                                    |
| -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Набор `"target"` in `tsconfig`. Пусть TS перепишет async/await, необязательную цепочку и т. д. для старых браузеров. | Цель по умолчанию — **ES5** (очень старая). Большинство браузеров сегодня поддерживают ES2015+. Набор `"target": "ES2015"` (или выше) в `tsconfig.json` если вам не нужна поддержка древних браузеров. Неправильная цель = раздутый или тр |

**Examples:**

```typescript
// Source (ES2015+ syntax)
`Hello ${person}, today is ${date.toDateString()}!`;
```

```javascript
// Emitted with target ES5
"Hello ".concat(person, ", today is ").concat(date.toDateString(), "!");
```

```javascript
// Emitted with target ES2015 — template literal preserved
`Hello ${person}, today is ${date.toDateString()}!`;
```

**Sources:** [Справочник по TypeScript — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

<a name="compiler-config"></a>

### Type system

**Definition:** Структурные типы, построенные на основе примитивов, объединений, пересечений, обобщений и сопоставленных/условных типов.

**When to use:** Границы API, общие модели, типизация библиотек.

**Pluses and minuses:**

| +                                                     | −                                           |
| ----------------------------------------------------- | ------------------------------------------- |
| Выразительные контракты без затрат времени выполнения | Продвинутым типам нужно учиться             |
| Типы утилит уменьшают дублирование                    | Слишком сложные типы ухудшают читабельность |

**Examples:**

```typescript
type Result<T> = { ok: true; data: T } | { ok: false; error: string };
type UserPreview = Pick<User, "id" | "name">;
```

**Sources:** [Повседневные типы](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### Compiler & config

**Definition:** `tsc` проверяет тип и транспилирует через `tsconfig.json` — `target`, `module`, `strict`, пути и ссылки на проекты.

**When to use:** Каждый проект ТС; затягивать `strict` флаги с течением времени.

**Pluses and minuses:**

| +                                      | −                                                      |
| -------------------------------------- | ------------------------------------------------------ |
| Одна конфигурация для всего репо       | `moduleResolution` может сбить с толку новичков        |
| Инкрементные сборки в монорепозиториях | Неправильно настроенные пути незаметно нарушают импорт |

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "noEmit": true
  }
}
```

**Sources:**

[tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

---

### TypeScript vs JavaScript

**Definition:** JavaScript является динамически типизированным; TypeScript — это **надмножество**, которое добавляет дополнительные статические типы, проверяемые во время компиляции, а затем удаляемые.

**When to use:** Долгоживущие кодовые базы, команды, библиотеки с общедоступными API.

**Pluses and minuses:**

| +                                    | −                                           |
| ------------------------------------ | ------------------------------------------- |
| Меньше производственных ошибок       | Дополнительный шаг компиляции               |
| Более безопасный рефакторинг модулей | Дисциплина необходима, чтобы избежать `any` |

**Examples:**

```typescript
const user = { name: "Ada" };
// user.age; // Error: Property 'age' does not exist
```

**Sources:** [TypeScript с нуля](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

<details>
  <summary>Compiler pipeline</summary>

## Compiler & tsconfig

<details>
  <summary>Strictness & tsconfig</summary>

### Strictness

**Definition:**

Семейство флагов компилятора, повышающих безопасность типов (`strict`, `noImplicitAny`, и т. д.).

| Когда использовать                                                                           | Заметки                                                                                                                                                                           |
| -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Давать возможность `"strict": true` о новых проектах; постепенно ужесточайте устаревший код. | `"strict": true` in `tsconfig.json` включает все строгие флаги одновременно. Вы также можете включить их по отдельности. Два самых больших: `noImplicitAny` и `strictNullChecks`. |

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `noImplicitAny`

**Definition:**

Флаг компилятора, который выдает ошибку, когда в противном случае TS сделал бы вывод `any` для переменной или параметра.

| Когда использовать                                                                                                       | Заметки                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Включить через `strict` or `"noImplicitAny": true`. Форсирует явные типы или лучший вывод — ловит молчание `any` утечки. | Без этого флага нетипизированные параметры функции автоматически становятся `any` и вы потеряете всю проверку внутри функции. Один только этот флаг значительно улучшает качество кода. |

**Examples:**

```typescript
// With noImplicitAny: OFF — no error, person is any
function greet(person) {
  console.log(person.toUpperCase());
}

// With noImplicitAny: ON — must annotate
function greet(person: string) {
  console.log(person.toUpperCase());
}
```

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `strictNullChecks`

**Definition:**

Изготовление флага `null` и `undefined` отдельные типы, которые должны обрабатываться явно.

| Когда использовать                                                                                                                          | Заметки                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Включить в `tsconfig`. Используйте необязательную цепочку `?.`, нулевое слияние `??`и защищает перед доступом к возможно нулевым значениям. | Включение этого в существующую кодовую базу может привести к сотням ошибок за одну ночь. Стоит того для нового кода; запланируйте постепенный переход на устаревшие проекты. Сопряжение с дополнительной цепочкой (`?.`) и нулевое сотрудничество |

**Examples:**

```typescript
// strictNullChecks: OFF — no error
function getLength(s: string) {
  return s.length;
}
getLength(null); // runs, crashes at runtime

// strictNullChecks: ON
function getLength(s: string | null) {
  if (s === null) return 0;
  return s.length; // s narrowed to string
}
```

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### Recommended tsconfig for new projects

**Definition:**

Рекомендуемый tsconfig для новых проектов — язык TypeScript или функция компилятора.

| Флаг               | Цель                                                                          |
| ------------------ | ----------------------------------------------------------------------------- |
| `strict`           | Включает `noImplicitAny`, `strictNullChecks`и т. д.                           |
| `moduleResolution` | Как разрешается импорт(`node`, `bundler`, `node16`)                           |
| `paths`            | Импортировать псевдонимы (`@/components` → `src/components`)                  |
| `isolatedModules`  | Каждый файл должен быть транспилируемым отдельно (требуется для Vite/esbuild) |
| `skipLibCheck`     | Пропустить проверку типа `.d.ts` файлы — более быстрая сборка                 |
| `noEmitOnError`    | Блокировать вывод JS при сбое типов                                           |

`strict: true` позволяет `noImplicitAny`, `strictNullChecks`и другие строгие флаги вместе.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2015",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "noEmitOnError": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### Advanced `tsconfig` flags — stricter and modern

**Definition:**

Включить более строгие флаги **поэтапно** — `noUncheckedIndexedAccess` сам по себе может добавить сотни ошибок в существующие базы кода.

| Флаг                                        | Цель                                                                                                                                                                                                              |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `noUncheckedIndexedAccess`                  | `arr[i]` и `obj[k]` становиться `T \| undefined` — безопаснее, шумнее                                                                                                                                             |
| `exactOptionalPropertyTypes`                | `prop?: T` ≠ `prop: T \| undefined` — более строгий дополнительный реквизит                                                                                                                                       |
| `strictFunctionTypes`                       | Параметры контравариантной функции (исправляет необоснованную бивариантность)                                                                                                                                     |
| `verbatimModuleSyntax`                      | Принудить `import type` / `export type`; более четкие границы ESM/CJS                                                                                                                                             |
| `moduleResolution: "node16"` / `"nodenext"` | Правильное разрешение для Node ESM+ `package.json` `"type"`                                                                                                                                                       |
| `declaration` + `declarationMap`            | излучать `.d.ts` (+исходные карты) для пользователей библиотек                                                                                                                                                    |
| `composite` + `incremental`                 | Ссылки на проекты и более быстрые инкрементальные сборки                                                                                                                                                          |
| `noImplicitOverride`                        | Требовать `override` при переопределении метода базового класса                                                                                                                                                   |
| `erasableSyntaxOnly`                        | Запретить синтаксис, который генерирует JS во время выполнения (свойства параметров, перечисления, пространства имен), поэтому `tsc` можно выполнять проверку типа без преобразования — стиль Node/ts-blank-space |

Включить более строгие флаги **поэтапно** — `noUncheckedIndexedAccess` сам по себе может добавить сотни ошибок в существующие базы кода.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "verbatimModuleSyntax": true,
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "declaration": true,
    "declarationMap": true,
    "composite": true
  }
}
```

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `@ts-ignore` vs `@ts-expect-error`

**Definition:**

Escape-штрихи, если компилятор неправильный или типы отсутствуют. Отдавайте предпочтение типам крепления; используйте экономно.

| Аспект | Деталь                                                                                                                          |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| Резюме | Escape-штрихи, если компилятор неправильный или типы отсутствуют. Отдавайте предпочтение типам крепления; используйте экономно. |

Спасательные люки, когда |

Escape-штрихи, если компилятор неправильный или типы отсутствуют. Отдавайте предпочтение типам крепления; используйте экономно.

**How it works:** `@ts-expect-error` предпочтителен — он терпит неудачу, когда основная проблема устранена, поэтому подавления не портятся.

**Examples:**

```typescript
// @ts-ignore — suppresses next line always (no error if line is fine)
// @ts-expect-error — errors if the NEXT line has NO type error (safer — catches stale suppressions)

// @ts-expect-error — legacy API not typed yet
legacyApi.doSomething();
```

**Sources:** [Справочник по TypeScript — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

</details>

<details>
  <summary>tsconfig.json</summary>

### tsconfig.json

**Definition:**

JSON-файл проекта, который сообщает компилятору TypeScript, **какие файлы входят в проект** и **как их проверять и компилировать**.

| When to use                                                                                                            | Notes                                                                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| В каждом TypeScript-проекте. Запустите `tsc --init`, чтобы получить стартовый файл, затем настройте `compilerOptions`. | `tsc` ищет `tsconfig.json` в текущей папке (или передайте `tsc -p path/to/tsconfig.json`). Без него проверяются только файлы из командной строки. |

**How it works:**

- **`compilerOptions`** — флаги: `strict`, `target`, `module`, `moduleResolution`, `paths`, `noEmit`.
- **`include` / `exclude`** — glob-паттерны исходников (например `"src/**/*"`; исключайте `node_modules` и тесты).
- **`files`** — явный список файлов, когда globs не нужны.
- **`extends`** — наследование общего базового конфига; `compilerOptions` сливаются, но `files` / `include` / `exclude` в дочернем файле **заменяют** списки базы.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "noEmit": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./dist"
  }
}
```

**Sources:** [TypeScript handbook — tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) · [TSConfig Reference](https://www.typescriptlang.org/tsconfig/)

</details>

<details>
  <summary>Declaration files (.d.ts)</summary>

### Declaration files (.d.ts)

**Definition:**

Файлы только с типами, которые описывают **форму** JavaScript-кода — при ручном написании **не генерируют runtime JS**, но дают TypeScript и редактору типы для библиотек и глобалов.

| When to use                                                                                                             | Notes                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Нетипизированные npm-пакеты (`@types/node`, `@types/react`), публикация типизированной библиотеки, расширение глобалов. | При module resolution TypeScript предпочитает `.ts` / `.d.ts` перед `.js`, чтобы проверять импорты без исходников реализации. |

**How it works:**

- **Ручные `.d.ts`** — описывают сторонний или legacy JS (`declare module "lodash"`, ambient globals).
- **Сгенерированные `.d.ts`** — `"declaration": true` в `tsconfig.json` при публикации TS-библиотеки.
- **Пакеты `@types/*`** — декларации от сообщества или DefinitelyTyped рядом с JS-пакетом.
- **`skipLibCheck`** — пропускает глубокую проверку `.d.ts` для скорости; сами типы остаются.

**Examples:**

```typescript
// types/legacy-lib.d.ts — форма нетипизированного пакета
declare module "legacy-lib" {
  export function parse(input: string): Record<string, unknown>;
}
```

```typescript
// global.d.ts — значение вне модулей (например, через <script>)
declare const APP_CONFIG: {
  apiUrl: string;
};
```

```typescript
// Использование установленных деклараций
import _ from "lodash";
_.padStart("Hello TypeScript!", 20, " ");
```

**Sources:** [Declaration files — introduction](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html) · [Consuming declarations](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html) · [The role of declaration files](https://www.typescriptlang.org/docs/handbook/modules/theory.html#the-role-of-declaration-files)

</details>

---

<a name="design-principles"></a>

## Design principles

<details>
  <summary>SOLID in TypeScript</summary>

### SOLID in TypeScript

**Definition:** Пять принципов **SOLID** для **TypeScript interfaces, classes и modules** — compile-time контракты через `interface`, `implements` и constructor injection.

| Principle                     | Meaning                         | TypeScript example                                                                    |
| ----------------------------- | ------------------------------- | ------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | One reason to change per module | `UserRepository` reads data · `UserFormatter` shapes labels — not one god class       |
| **O** — Open/Closed           | Extend via new code, not edits  | New `StripeProcessor implements PaymentProcessor` — don't edit `CheckoutService`      |
| **L** — Liskov Substitution   | Subtypes honor parent contract  | Every `UserStore` passed to `loadProfile()` must return the same `{ id, name }` shape |
| **I** — Interface Segregation | Small focused interfaces        | `ReadableLedger` for reports — not a fat `Ledger` with `write()` / `delete()`         |
| **D** — Dependency Inversion  | Depend on abstractions          | `placeOrder(id, logger: Logger)` — inject `Logger`, not a concrete console adapter    |

**How it works:**

Каждая буква соответствует **Examples** ниже — ищите `// good` и `// bad`:

1. **S** — разделите repository и formatter отдельными `interface` — не одним service class.
2. **O** — новый provider = новый class с interface — не растущий `if/else` в одном классе.
3. **L** — `implements UserStore` должен соблюдать return type — `tsc` ловит сломанные замены.
4. **I** — передавайте `ReadableLedger`, где хватает чтения — не требуйте лишние write API.
5. **D** — domain-функции принимают типы `Logger` / `DbClient` — wiring выбирает реализации.

**When to use:** Domain models, services, DI graphs, public module APIs. Skip heavy interfaces on a one-off script.

**Examples:**

```typescript
type DbClient = { query(id: string): Promise<{ id: string; name: string }> };

interface UserRepository {
  findById(id: string): Promise<{ id: string; name: string }>;
}

interface UserFormatter {
  label(user: { name: string; email: string }): string;
}

class DbUserRepository implements UserRepository {
  constructor(private db: DbClient) {}
  findById(id: string) {
    return this.db.query(id);
  }
}

interface PaymentProcessor {
  charge(amount: number): Promise<{ ok: true }>;
}

class StripeProcessor implements PaymentProcessor {
  charge(amount: number) {
    return Promise.resolve({ ok: true as const });
  }
}

class CheckoutService {
  constructor(private payments: PaymentProcessor) {}
  checkout(amount: number) {
    return this.payments.charge(amount);
  }
}

interface UserStore {
  get(id: string): Promise<{ id: string; name: string }>;
}

async function loadProfile(id: string, store: UserStore) {
  return store.get(id);
}

interface ReadableLedger {
  read(id: string): Promise<{ id: string; total: number }>;
}

function buildReport(ledger: ReadableLedger) {
  return ledger.read("summary");
}

interface Logger {
  info(message: string): void;
}

function placeOrder(orderId: string, logger: Logger) {
  logger.info(`Processing ${orderId}`);
}
```

**Thoughts:** TypeScript делает **I**, **L** и **D** видимыми на compile time — но types стираются в runtime; behavioral contracts всё равно важны. Классы: [Classes](./5_ts_classes.md). JS: [SOLID in Classes](./3_js_core.md#solid-in-classes) · [SOLID in Functions](./3_js_core.md#solid-in-functions). Фреймворки: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express). Полный справочник: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [TypeScript handbook — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html) · [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions)

</details>

---

<a name="modern-typescript"></a>

## Modern TypeScript

<details>
  <summary>Modern TypeScript</summary>

### Quick list — checklist

1. **`const` параметры типа** — буквальный вывод в дженериках (TS 5.0+)
2. **`using` / `Disposable`** — очистка ресурсов по заданной области (TS 5.2+)
3. **`override` ключевое слово** — явные, проверенные переопределения метода
4. **Более строгие флаги компилятора** — `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`
5. **ESM/CJS** — `moduleResolution: node16` / `nodenext`, `verbatimModuleSyntax`
6. **Ссылки на проекты** — разделение монорепозиториев, инкрементальное `tsc --build`
7. **Публикация типизированных пакетов** — `declaration`, `types` field, bundling `.d.ts`
8. **Рекурсивные типы** — `Json`, trees, deep `Partial`
9. **Дисперсия** — ковариантные возвраты, контравариантные параметры, подводные камни бивариантности
10. **Проверка времени выполнения** — типы TS ≠ runtime; используйте Zod/io-ts для внешних данных

### `const` Type Parameters (TS 5.0+)

**Definition:**

Модификатор generic-параметра, сохраняющий литеральные типы при выводе.

| Когда использовать                                                                               | Заметки                                                                                              |
| ------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| `function routes<const T extends readonly string[]>(paths: T)` — tuple литералов, не `string[]`. | recursion needs a **base case** (`T extends object` guard) или компилятор упирается в лимит глубины. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Variance (functions and assignability)

**Definition:**

Как подтипирование работает для параметров функции (контравариантно) и возвращаемых значений (ковариантно).

| Аспект | Деталь                                                                                                      |
| ------ | ----------------------------------------------------------------------------------------------------------- |
| Резюме | Как подтипирование работает для параметров функции (контравариантно) и возвращаемых значений (ковариантно). |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>

<a name="everyday-types"></a>

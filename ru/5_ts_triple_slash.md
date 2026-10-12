# TypeScript — Triple-Slash Directives

Compiler directives, ambient declarations, and `declare`.

[← Назад к хабу](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Triple-Slash Directives](#triple-slash-directives)

---

## Triple-Slash Directives

<details>
  <summary>Triple-Slash Directives</summary>

### Triple-Slash Directives

**Definition:**

Однострочный `///` комментарии в верхней части файла, которые дают указания компилятору.

| Когда использовать                                                                                           | Заметки                                                                                                            |
| ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| `/// <reference path="./types.d.ts" />` — legacy; prefer `импортировать` and `цконфигурация` `ссылки` today. | As of TS 5.5, the compiler does **not** emit handwritten triple-slash directives to output unless `save="истина"`. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Common directives

**Definition:**

/// <amd-module name="NamedModule"/> // Имя модуля AMD в выводе

|

- **`reference path`** — относительная зависимость файлов; разрешение в глубину
- **`reference types`** - нравиться `import` для пакетов деклараций (`@types/node`); предпочитать `"types"` in `tsconfig.json` для `.ts` файлы
- **`reference lib`** — извлечь фрагменты библиотеки DOM/ES.
- **`--noResolve`** — игнорирует все ссылочные директивы

**How it works:**

`/// <amd-dependency path="..." />` - использовать `import "moduleName"` вместо.

---

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | /// <amd-module name="NamedModule"/>    // Name AMD module in output |
```

```typescript
/// <reference path="./utils.ts" />     // Add file to compilation (ordering)
/// <reference types="node" />          // Include @types package
/// <reference lib="es2017.string" />   // Include built-in lib (like --lib)
/// <reference no-default-lib="true"/>  // Skip default lib.d.ts

/// <amd-module name="NamedModule"/>    // Name AMD module in output
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Ambient Declarations and `declare`

**Definition:**

Типовые описания кода, который существует где-либо еще (библиотеки JS, глобальные переменные, `.d.ts` прокладки).

| Когда использовать                                                              | Заметки                                                                                                                             |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `declare const $: JQueryStatic` or `объявить модуль «устаревшим»`. Emits no JS. | `объявить` does not create a value at runtime. Pair with `@типы/*` packages or hand-written `.d.ts` для нетипизированных библиотек. |

**Examples:**

```typescript
// Global variable loaded via <script>
declare const jQuery: (selector: string) => unknown;

// Ambient module — shape of an untyped npm package
declare module "legacy-lib" {
  export function doSomething(x: number): string;
}

// Ambient namespace — global library
declare namespace D3 {
  export function select(selector: string): unknown;
}

// declare class/function/var — types only, no JS emit
declare function greet(name: string): void;
declare class LegacyWidget {
  constructor(el: string);
  render(): void;
}

// Asset imports — tell TS non-TS files exist (Vite/Webpack)
declare module "*.svg" {
  const content: string;
  export default content;
}
declare module "*.png" {
  const src: string;
  export default src;
}
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### `noImplicitAny` and `strictNullChecks` — interview definitions

**Definition:**

Оба включены `"strict": true` in `tsconfig.json`.

---

| Аспект | Деталь                                            |
| ------ | ------------------------------------------------- |
| Резюме | Оба включены `"strict": true` in `tsconfig.json`. |
| ---    |

**`noImplicitAny`** — если TS не может определить тип, по умолчанию будет `any`. Этот флаг превращает это в **ошибку**, требующую явных аннотаций. Предотвращает тихую потерю безопасности типов в параметрах функции и нетипизированных переменных.

**`strictNullChecks`** - по умолчанию `null` и `undefined` присвоить любому типу. Этот флаг делает их разными — вы должны явно обрабатывать возможность обнуления. Отлавливает наиболее распространенный класс производственных ошибок.

## Оба включены `"strict": true` in `tsconfig.json`.

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>

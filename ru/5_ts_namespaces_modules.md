# TypeScript — Namespaces and Modules

When to use ES modules vs legacy namespaces.

[← Назад к хабу](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Namespaces and Modules](#namespaces-and-modules)

---

## Namespaces and Modules

<details>
  <summary>Namespaces and Modules</summary>

### Namespaces and Modules

**Definition:**

Как TS организует код — модули ES (файлы) и устаревшие пространства имен (глобальные).

| Когда использовать                                                                                                                     | Заметки                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Использовать `import`/`export` за файл. Пространства имен только для глобального расширения или взаимодействия с устаревшими версиями. | Не `/// <reference path="foo.ts">` когда ты имеешь в виду `import` — ссылки добавляют файлы в компиляцию; импорт — это граф модуля. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Common pitfalls

**Definition:**

// ХОРОШО

класс экспорта Треугольник {}

| Аспект                        | Деталь    |
| ----------------------------- | --------- |
| Резюме                        | // ХОРОШО |
| класс экспорта Треугольник {} |

**How it works:** ---

**Examples:**

```typescript
// BAD — needless namespace wrapper in a module
export namespace Shapes {
  export class Triangle {}
}
// Consumer: new shapes.Shapes.Triangle() 😬

// GOOD
export class Triangle {}

// Modules can't use outFile with commonjs/umd — only amd/system
```

```typescript
// myModules.d.ts (not a module)
declare module "SomeModule" {
  export function fn(): string;
}

// myOtherModule.ts
/// <reference path="myModules.d.ts" />
import * as m from "SomeModule";
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>

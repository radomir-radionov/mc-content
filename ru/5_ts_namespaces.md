# TypeScript — Namespaces

Legacy `namespace` organization and ambient declarations.

[← Назад к хабу](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Namespaces](#namespaces)

---

## Namespaces

<details>
  <summary>Namespaces</summary>

### Namespaces

**Definition:**

Устаревший способ группировки кода в глобальной области видимости: `namespace Utils { export function f() {} }`.

| Когда использовать                                                                                                           | Заметки                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Предпочитайте модули ES (`import`/`export`) в современном коде. Пространства имен все еще появляются в старых `.d.ts` файлы. | `module X` это устаревший псевдоним для `namespace X`. Предпочитайте **модули ES** (`import`/`export`) в современном коде — пространства имен загрязняют глобальную область видимости и усложняют отслеживание зависимостей. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Patterns

**Definition:**

Utils.getPass("Евгений", 31); // "Яухень31"

Utils.isEmpty(Utils.SECRET); // ложь

| Аспект                               | Деталь                                      |
| ------------------------------------ | ------------------------------------------- |
| Резюме                               | Utils.getPass("Евгений", 31); // "Яухень31" |
| Utils.isEmpty(Utils.SECRET); // ложь |

## **Окружающие пространства имен** (`declare namespace D3 { ... }`) описывают глобальные библиотеки, загруженные через `<script>`.

**Examples:**

```typescript
// Legacy namespace — encapsulates globals; must export to expose
namespace Utils {
  export const SECRET = "123321";
  const PI = 3.14; // private to namespace — not accessible outside
  export const getPass = (name: string, age: number) => `${name}${age}`;
  export const isEmpty = <T>(data: T) => !data;
}

Utils.getPass("Yauhen", 31); // "Yauhen31"
Utils.isEmpty(Utils.SECRET); // false

// Modern equivalent — ES modules (no namespace wrapper needed)
// export const SECRET = "123321";
// export const getPass = ...
// import { getPass, SECRET } from "./Utils";

namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
  export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
      return s.length === 5 && /^\d+$/.test(s);
    }
  }
}

// Alias — import q = x.y.z (not the same as import from "module")
import polygons = Shapes.Polygons;
let sq = new polygons.Square();

// Multi-file: same namespace name merges across files
// Use /// <reference path="..." /> to order compilation
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>

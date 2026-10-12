# TypeScript — Declaration Merging

Interface, namespace, class, function, and enum merging rules.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Declaration Merging](#declaration-merging)

---


## Declaration Merging

<details>
  <summary>Declaration Merging</summary>

### Declaration Merging

**Definition:** Multiple declarations with the same `interface` name merge into one type.

**When to use:** Augment globals (`interface Window { myApp: App }`) or library types. Only works with `interface`, not `type`.

**Thoughts:**

Declarations create entities in three groups — **namespace**, **type**, and **value**. What merges depends on declaration kind:

| Declaration | Namespace | Type | Value |
|-------------|-----------|------|-------|
| Namespace   | ✓         | ✓    | ✓     |
| Class       | ✓         | ✓    | ✓     |
| Enum        | ✓         | ✓    | ✓     |
| Interface   |           | ✓    |       |
| Type Alias  |           | ✓    |       |
| Function    |           |      | ✓     |
| Variable    |           |      | ✓     |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Interface merging

**Definition:**

// Function members → overloads (later interfaces win ordering for overload sets)

interface Cloner { clone(animal: Animal): Animal; }
interface Cloner { clone(animal: Sheep): Sheep; }
typescript
interface Box { height: number; width: number; }
interface Box { scale: number; }

let box: Box = { height: 5, width: 6, scale: 10 };

// Function members → overloads (later interfaces win ordering for overload sets)
interface Cloner { clone(animal: Animal): Animal; }
interface Cloner { clone(animal: Sheep): Sheep; }
```

Non-function members with the same name must have the **same type** or the compiler errors.

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // Function members → overloads (later interfaces win ordering for overload sets)
interface Cloner { clone(animal: Anima |

```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Namespace + class/function/enum merging

**Definition:**

// Function with static properties

function buildLabel(name: string): string {
  return buildLabel.prefix + name + buildLabel.suffix;
}
namespace buildLabel {
  export let suffix = "";
  export let prefix = "Hello, ";
}
typescript
// Inner class pattern
class Album {
  label: Album.AlbumLabel;
}
namespace Album {
  export class AlbumLabel {}
}

// Function with static properties
function buildLabel(name: string): string {
  return buildLabel.prefix + name + buildLabel.suffix;
}
namespace buildLabel {
  export let suffix = "";
  export let prefix = "Hello, ";
}
typescript
// map.ts
import { Observable } from "./observable";
declare module "./observable" {
  interface Observable<T> {
    map<U>(f: (x: T) => U): Observable<U>;
  }
}
Observable.prototype.map = function (f) { /* ... */ };
```

Limitations: can't add new top-level declarations; can't augment default exports.
---

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // Function with static properties
function buildLabel(name: string): string {
  return buildLabel.prefix + name + build |

```

```

**Module augmentation** — patch existing modules (not merge JS modules natively):

```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>

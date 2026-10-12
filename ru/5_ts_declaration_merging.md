# TypeScript — Declaration Merging

Слияние interface, namespace, class и enum.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Declaration Merging](#declaration-merging)

---


## Declaration Merging

<details>
  <summary>Declaration Merging</summary>

### Declaration Merging

**Definition:** Несколько объявлений с одним и тем же `interface` названия объединяются в один тип.

**When to use:** Дополнить глобальные переменные (`interface Window { myApp: App }`) или типы библиотек. Работает только с `interface`, нет `type`.

**Thoughts:**

Объявления создают сущности в трех группах — **пространство имен**, **тип** и **значение**. Какие слияния зависят от типа объявления:

| Декларация | Пространство имен | Тип | Значение |
|-------------|-----------|------|-------|
| Пространство имен | ✓ | ✓ | ✓ |
| Класс | ✓ | ✓ | ✓ |
| Перечисление | ✓ | ✓ | ✓ |
| Интерфейс |           | ✓ |
|Введите псевдоним |           | ✓ |       |
| Функция |           |      | ✓ |
| Переменная |           |      | ✓ |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Interface merging

**Definition:**

// Члены функций → перегрузки (более поздние интерфейсы выигрывают в порядке наборов перегрузок)

интерфейс Cloner { clone(animal: Animal): Animal; }
интерфейс Cloner { клон (животное: Овца): Овца; }
машинописный текст
интерфейсный блок { высота: число; ширина: число; }
интерфейсный блок { масштаб: число; }

let box: Box = { высота: 5, ширина: 6, масштаб: 10 };

// Члены функций → перегрузки (более поздние интерфейсы выигрывают в порядке наборов перегрузок)интерфейс Cloner { clone(animal: Animal): Animal; }
интерфейс Cloner { клон (животное: Овца): Овца; }
```

Non-function members with the same name must have the **same type** or the compiler errors.

**Examples:**

```

| Аспект | Деталь |
|--------|--------|
| Резюме | // Члены функций → перегрузки (более поздние интерфейсы выигрывают в порядке наборов перегрузок) |
интерфейс Cloner { клон(животное: Анима |

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

Ограничения: невозможно добавлять новые объявления верхнего уровня; не может увеличить экспорт по умолчанию.
---

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // Function with static properties |
function buildLabel(name: string): string {
  return buildLabel.prefix + name + build |

```

```

**Module augmentation** — patch existing modules (not merge JS modules natively):

```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>

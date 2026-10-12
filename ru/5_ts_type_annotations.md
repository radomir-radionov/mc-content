# TypeScript — Type Annotations

Явные аннотации типов на переменных и когда их не нужно писать.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Type annotations](#type-annotations)

---

## Type annotations

<details>
  <summary>Type annotations</summary>

### Type Annotations on Variables

**Definition:**

Явная метка типа переменной: `let name: string`.

| Когда использовать | Заметки |
|-------------|-------|
| Добавляйте, когда вывод неверен или слишком широк; опустить, если это очевидно (`const n = 5` → `number`). Тип идет **после** имени: `let x: number`. | В отличие от C/Java (`int x = 0`), TypeScript помещает тип **после** имени, а не перед ним. И в большинстве случаев вам это не нужно — TypeScript делает выводы на основе присвоенного вами значения. Эмпирическое правило: аннотируйте, когда |

**Examples:**

```typescript
// Explicit
let myName: string = "Alice";

// Inferred — usually prefer this
let myName = "Alice";  // TypeScript knows: string

// Annotation useful when you need a wider type than inference gives:
let id: string | number = Math.random() > 0.5 ? "abc" : 42;
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>



# TypeScript — JSX

JSX в `.tsx` и типизация компонентов.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [JSX](#jsx)

---


## JSX

<details>
  <summary>JSX</summary>

### JSX

**Definition:**

XML-подобный синтаксис в `.tsx` файлы для UI-фреймворков (React и т. д.).

| Когда использовать | Заметки |
|-------------|-------|
| Набор `"jsx": "react-jsx"` in `tsconfig`. TS типы реквизита через `JSX.IntrinsicElements` и типы компонентов. | Утверждения типа угловых скобок (`<Foo>bar`) **запрещены** в `.tsx` - использовать `bar as Foo` вместо этого (неоднозначность парсера с тегами JSX). |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Setup & jsx modes

**Definition:**

1. Используйте `.tsx` расширение

2. Установите `"jsx"` in `tsconfig.json`

1. Используйте `.tsx` расширение
2. Установите `"jsx"` in `tsconfig.json`

| Режим | Выход |
|------|--------|
| `preserve` | Сохранить JSX → `.jsx` |
| `react` | `React.createElement(...)` |
| `react-jsx` | Автоматическое время выполнения `_jsx(...)` |
| `react-native` | Сохранить JSX → `.js` |

**How it works:**

* Нижний регистр (`<div>`) → внутренний элемент DOM/хост → `JSX.IntrinsicElements`
* Прописные буквы (`<MyComponent>`) → ваш компонент → решено из области видимости

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Type checking highlights

**Definition:**

// FC должен вернуть JSX.Element (или настроить JSX.ElementType в TS 5.1+)

Реквизит интерфейса {имя: строка}
функция Greet(реквизит: Реквизит) {
  возвращение <span>{реквизит.имя}</span>;
}

| Аспект | Деталь |
|--------|--------|
| Резюме | // FC должен вернуть JSX.Element (или настроить JSX.ElementType в TS 5.1+) |
Реквизит интерфейса {имя: строка}
функция Приветствовать( |

Настроить через `jsxFactory`, `jsxFragmentFactory`, `jsxImportSource`.
---

**Examples:**

```typescript
// Intrinsic elements
declare namespace JSX {
  interface IntrinsicElements {
    foo: { bar?: boolean };
  }
}

// FC must return JSX.Element (or customize JSX.ElementType in TS 5.1+)
interface Props { name: string }
function Greet(props: Props) {
  return <span>{props.name}</span>;
}

// Children — JSX.ElementChildrenAttribute (default: children)
// Attributes — JSX.ElementAttributesProperty (default: props)
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>

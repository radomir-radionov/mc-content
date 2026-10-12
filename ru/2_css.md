# CSS — hub

Каскад, блочная модель, макет, адаптивный дизайн, движение, архитектура и производительность. Теория находится в файлах тем ниже.

### Related docs

[CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Архитектура](./2_css_architecture.md) · [CSS — Интервью](./2_css_interview.md)

### Navigation

| Тема                                         | Обложки                                                                            |
| -------------------------------------------- | ---------------------------------------------------------------------------------- |
| [CSS — ядро](./2_css_core.md)                | Как добавляется CSS, каскад, селекторы и блочная модель.                           |
| [CSS — Макет](./2_css_layout.md)             | Flexbox, сетка, позиционирование, адаптивный дизайн и мобильная область просмотра. |
| [CSS — Визуальный](./2_css_visual.md)        | Типографика, цвет, оформление, стиль формы и печать.                               |
| [CSS — Движение](./2_css_motion.md)          | Трансформации, переходы, анимация и визуальные эффекты.                            |
| [CSS — Современный](./2_css_modern.md)       | Более 2024 функций CSS и расширенные темы фазы 2.                                  |
| [CSS — Архитектура](./2_css_architecture.md) | Методологии, инструменты и производительность рендеринга.                          |
| [CSS — Интервью](./2_css_interview.md)       | Вопросы по CSS, ответы не заполнены.                                               |

### Interview practice

Вопросы и ответы находятся в теме интервью по этой теме.

---

## Resources & links

<details>
  <summary>External references</summary>

- [Справочник по CSS MDN](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [web.dev Изучение CSS](https://web.dev/learn/css/)
- [CSS-трюки](https://css-tricks.com/)
- [Flexbox Froggy](https://flexboxfroggy.com/)
- [Сетчатый сад](https://cssgridgarden.com/)
- [CSS для JS-разработчиков](https://learn.javascript.ru/css-for-js)\* [Современный CSS 2025 (Frontend Masters)](https://frontendmasters.com/blog/what-you-need-to-know-about-modern-css-2025-edition/)
- [Шаблоны CodemyUI](https://codemyui.com/)
- [Вопросы для собеседования по CSS](https://www.interviewbit.com/css-interview-questions/)
- [Шрифты Google](https://fonts.google.com/)
- [caniuse.com](https://caniuse.com/)

**Видео\*** [5 советов по CSS для адаптивного дизайна](https://www.youtube.com/watch?v=2IV08sP9m3U)

- [10 советов профессионалам CSS](https://www.youtube.com/watch?v=44FTAS-qT8Q)
- [CSS-анимация](https://www.youtube.com/watch?v=UxFnW2cPRi8)

</details>

### Sections

- [Ядро](./2_css_core.md#core)
- [Селекторы и каскад](./2_css_core.md#selectors-cascade)
- [Модель и отображение блока](./2_css_core.md#box-model-display)
- [Системы макетов](./2_css_layout.md#layout-systems)
- [Адаптивный дизайн](./2_css_layout.md#Response-design)
- [Прокрутка, касание и мобильное устройство](./2_css_layout.md#scroll-touch-mobile)
- [Типография и цвета](./2_css_visual.md#typography-colors)- [Украшение](./2_css_visual.md#decoration)
- [Стилизация формы](./2_css_visual.md#form-styling)
- [Печатные и постраничные материалы](./2_css_visual.md#print)
- [Преобразования и анимация](./2_css_visual.md#transforms-animations)
- [Визуальные эффекты](./2_css_motion.md#visual-effects)
- [Современный CSS](./2_css_motion.md#modern-css)
- [CSS Фаза 2](./2_css_modern.md#css-phase-2)- [Архитектура и инструменты](./2_css_architecture.md#architecture-tooling)
- [Производительность](./2_css_architecture.md# Performance)
- [Interview Practice](#interview-practice)
- [Вопросы для собеседования](./2_css_interview.md#interview-practice)

> Теория `<details>` блоки следуют: **Определение** · **Как это работает** · **Когда использовать** · **Плюсы и минусы** · **Примеры** · **Мысли** · **Источники**. **Определение** обязательно; другие разделы являются необязательными.

### Why CSS styles don't apply

1. Таблица стилей не связана или указан неправильный путь.
2. Селектор не соответствует элементу
3. Отменяется более высокой специфичностью или более поздним правилом.
4. Свойство недействительно или не поддерживается браузером.
5. `display: none` / `visibility: hidden` на родителе
6. Опечатка в свойстве или значении.

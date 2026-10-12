# HTML — Media

Изображения, изображения, аудио, видео, холст и SVG.

[ ← Вернуться в центр](./1_html.md)

### Related docs

- [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1*html* Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Media & graphics](#media-graphics)

---

## Media & graphics

<details>
  <summary>img element and responsive images</summary>

### img element and responsive images

**Definition:**

- `<img>` элемент встраивает изображения. Использовать `alt`, явные размеры, `srcset`/`sizes`, и `loading` по производительности и доступности.

| Атрибут            | Цель                                                         |
| ------------------ | ------------------------------------------------------------ |
| `alt`              | Альтернативный текст (требуется для осмысленных изображений) |
| `width` / `height` | Зарезервировать место → предотвратить CLS                    |
| `srcset` + `sizes` | Адаптивный выбор                                             |
| `loading="lazy"`   | Отложить изображения за кадром                               |

**How it works:**

- `src` отступать; `srcset` + `sizes` выберите разрешение по области просмотра.
- `alt` требуется для осмысленных изображений; декоративный → `alt=""`.
- `width`/`height` зарезервируйте место для предотвращения CLS.

| Атрибут          | Цель                 |
| ---------------- | -------------------- |
| `alt`            | Альтернативный текст |
| `srcset`/`sizes` | Адаптивный выбор     |
| `loading="lazy"` | Отложить за кадром   |

**When to use:**

Геройское изображение LCP · ленивый нижний сгиб · сетчатка `srcset`.

#### Common pitfalls

- Отсутствующий `alt` на изображениях контента.
- `loading="lazy"` на ЛКП-герое.
- Нет габаритов → смещение компоновки при нагрузке.

**Examples:**

```html
<img
  src="hero-800.jpg"
  srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w"
  sizes="(max-width: 600px) 100vw, 50vw"
  alt="Team collaborating in an office"
  width="800"
  height="450"
  loading="lazy"
  fetchpriority="high"
/>
```

**Sources:** [MDN — адаптивные изображения](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

</details>

<details>
  <summary>picture element</summary>

### picture element

**Definition:**

- `<picture>` Элемент обеспечивает художественное оформление и резервные варианты современных форматов (AVIF → WebP → JPEG).

| Вариант использования      | Подход                               |
| -------------------------- | ------------------------------------ |
| Резервный формат           | Несколько `<source type>`            |
| Художественное направление | Различные культуры на точку останова |

**How it works:**

- `<picture>` обертывает несколько `<source>` + запасной вариант `<img>`.
- `type` в источнике для согласования формата (AVIF/WebP).
- `media` в исходном коде для точек останова художественного направления.

| Использование              | Подход                                |
| -------------------------- | ------------------------------------- |
| Резервный формат           | `<source type="image/avif">`          |
| Художественное направление | `<source media="(max-width: 600px)">` |

**When to use:**

AVIF с резервным JPEG · кадрирование для мобильных устройств и кадрирование для настольных компьютеров.

#### Common pitfalls

- Требуется забыть `<img>` отступать.
- Такой же `srcset` на всех источниках (никакой выгоды).
- `picture` для простого изменения размера (используйте `img srcset` только).

**Examples:**

```html
<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img src="hero.jpg" alt="Hero banner" />
</picture>
```

**Sources:** [MDN — изображение](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture)

</details>

<details>
  <summary>video and audio</summary>

### video and audio

**Definition:**

- `<video>` и `<audio>` элементы встраивают медиафайлы с собственными элементами управления. Предоставьте несколько форматов и подписей для доступности.

| Атрибут    | Заметки                               |
| ---------- | ------------------------------------- |
| `controls` | Собственный интерфейс воспроизведения |
| `preload`  | `none`, `metadata`, `auto`            |
| `poster`   | Миниатюра перед воспроизведением      |
| `<track>`  | Титры/субтитры                        |

**How it works:**

- `<video>`/`<audio>` встраивайте медиафайлы с собственными элементами управления.
- Несколько `<source>` для резервного кодека.
- `<track kind="captions">` для доступности.

| Атрибут    | Заметки                                |
| ---------- | -------------------------------------- |
| `controls` | Собственный пользовательский интерфейс |
| `poster`   | Миниатюра видео                        |
| `<track>`  | Титры/субтитры                         |

**When to use:**

Демонстрационное видео продукта · проигрыватель подкастов · фоновое видео (предпочтительно использовать CSS, без звука, `playsinline`).

#### Common pitfalls

- Автовоспроизведение со звуком (заблокировано браузерами).
- Никаких подписей к речевому контенту.
- Единый формат без резервного источника.

**Examples:**

```html
<video controls preload="metadata" poster="thumb.jpg">
  <source src="clip.webm" type="video/webm" />
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="en.vtt" srclang="en" label="English" default />
</video>
```

**Sources:** [MDN — видео](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)

</details>

<details>
  <summary>canvas vs SVG</summary>

### canvas vs SVG

**Definition:**

`<canvas>` — это растровая поверхность, нарисованная с помощью JavaScript. `<svg>` — векторная разметка в DOM — масштабируемая и доступная.

|                 | `<canvas>`                        | `<svg>`                     |
| --------------- | --------------------------------- | --------------------------- |
| Модель          | Растровое изображение (пиксели)   | Вектор (узлы DOM)           |
| Масштабирование | Теряет качество                   | Независимость от разрешения |
| Доступность     | Нужен запасной вариант ARIA       | Родной ДОМ                  |
| Лучшее для      | Игры, тяжёлая пиксельная анимация | Иконки, диаграммы           |

**How it works:**

- `<canvas>` — растровая поверхность, нарисованная через JS API (`getContext('2d')`).
- `<svg>` — векторное DOM; весы чисто; стиль с помощью CSS.
- Холст для пикселей/игр; SVG для иконок/диаграмм.

|        | `<canvas>`                | `<svg>`      |
| ------ | ------------------------- | ------------ |
| Модель | Растровое изображение     | Вектор DOM   |
| А11y   | Требуется резервный текст | Доступен DOM |

**When to use:**

Chart.js на холсте · встроенный значок SVG · игровые спрайты на холсте.

#### Common pitfalls

- Иконочные шрифты вместо SVG (a11y, масштабирование).
- Холстовые диаграммы без текстовой альтернативы.
- Огромные SVG не оптимизированы (SVGO).

**Examples:**

```html
<canvas id="chart" width="400" height="200"></canvas>
<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M10 20v-6h4v6z" /></svg>
```

**Thoughts:** API рисования на холсте → [3_js_browser.md](./3_js_browser.md). Автономный `.svg` файлы представляют собой XML: установить `xmlns="http://www.w3.org/2000/svg"` на внешнем `<svg>` или браузер не будет воспринимать файл как SVG. Встроенный SVG в HTML не требуется. `xmlns`. Пространства имен: [6_web_api.md — XML](./6_web_api_rest.md#xml).

**Sources:** [MDN — холст](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas)

</details>

---

<a name="performance--resource-loading"></a>

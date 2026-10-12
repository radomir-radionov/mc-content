# HTML — Forms

Элементы управления формой, проверка, диалоговое окно и доступные шаблоны форм.

Ориентиры и значение заголовка находятся в [HTML — Semantics](./1_html_semantics.md).

[ ← Вернуться в центр](./1_html.md)

### Related docs

* [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1_html_ Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Forms & validation](#forms-validation)

---

## Forms & validation

<details>
  <summary>Form structure</summary>

### Form structure

**Definition:**

Форма представляет собой оболочку элементов управления, которые собирают вводимые пользователем данные и передают данные на сервер. Каждый отправляющий элемент управления нуждается в `name` атрибут.

| Элемент | Роль |
|---------|------|
| `<form>` | Контейнер с `action`, `method` |
| `<fieldset>` / `<legend>` | Элементы управления, связанные с группой |
| `<label for>` | Ассоциированная этикетка с контролем |
| `<button type="submit">` | Отправляет форму |

**How it works:**

- `<form action method enctype>` определяет цель представления.
- Необходимость контроля `name` появиться в представленных данных.
- `<fieldset>`/`<legend>` группировать связанные поля.
- `<label for>` связывает текст с идентификатором элемента управления.

| Элемент | Роль |
|---------|------|
| `<form>` | Контейнер подачи |
| `<label for>` | Доступное имя |
| `<button type="submit">` | Отправить |

**When to use:**

Вход · многошаговый мастер · поиск (`method="get"`).

#### Common pitfalls

- Входы без `name` (не представлено).
- Ярлыки только для заполнителей.
- Вложенные формы (недействительно).

**Examples:**

```html
<form action="/api/login" method="post" novalidate>
  <fieldset>
    <legend>Account</legend>
    <label for="email">Email</label>
    <input id="email" name="email" type="email" required autocomplete="email" />
    <label for="password">Password</label>
    <input id="password" name="password" type="password" required minlength="8" />
    <button type="submit">Sign in</button>
  </fieldset>
</form>
```

**Thoughts:** Отправка FormData и JS → [3_js_browser_extra.md](./3_js_browser_extra.md#formdata-file-api-filereader).

**Sources:** [MDN — Формы](https://developer.mozilla.org/en-US/docs/Learn/Forms)

</details>

<details>
  <summary>Input types</summary>

### Input types

**Definition:** - `type` атрибут включен `<input>` выбирает поведение элемента управления, правила проверки и мобильную клавиатуру.

**How it works:**

- `type` выбирает пользовательский интерфейс управления, правила проверки и мобильную клавиатуру.
- Браузер обеспечивает встроенную проверку `email`, `url`, `number`, и т. д.
- `required`, `pattern`, `min`/`max` добавить ограничения.

| `type` | Использование |
|--------|-----|
| `text`, `email`, `password` | Текстовые варианты |
| `number`, `range` | Числовой ввод |
| `checkbox`, `radio` | Логическое значение/одиночный выбор |
| `date`, `file` | Выбор даты/загрузка файла |

**When to use:**

Регистрация по электронной почте · шаговый регулятор количества · загрузка файлов с помощью `accept`.

#### Common pitfalls

- `type="number"` для кредитных карт (используйте `text` + `inputmode`).
- Отсутствующий `name` на входах.
- `placeholder` как только этикетка.

**Pluses and minuses:**

| + | − |
|------|------|
| Встроенная проверка и мобильные клавиатуры | Ограниченный стиль и пользовательские виджеты |
| Доступно по умолчанию | `type="number"` особенности некоторых форматов |

**Examples:**

```html
<input type="email" name="email" required />
<input type="number" name="qty" min="1" max="99" />
<input type="file" name="avatar" accept="image/png,image/jpeg" />
```

**Sources:** [MDN — типы ввода](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)

</details>

<details>
  <summary>select, textarea, and button types</summary>

### select, textarea, and button types

**Definition:** Вне `<input>`, формы использования `<select>` для списков опций, `<textarea>` для многострочного текста и `<button>` с явным `type` для действий.

**How it works:**

- `<select name>` отправляет выбранные `<option value>`; `multiple` позволяет множественный выбор (отправляется как массив в JS FormData).
- `<textarea name>` значением является **текстовое содержимое** между тегами, а не `value` атрибут.
- `<button type="submit">` (по умолчанию внутри формы), `type="reset"`, or `type="button"` за действия, не связанные с отправкой.

| Контроль | Ключевые атрибуты |
|---------|----------|
| `<select>` | `name`, `multiple`, `required` |
| `<textarea>` | `name`, `rows`, `maxlength` |
| `<button>` | `type`, `formaction` |

**When to use:**

Выбор страны · поле комментария · кнопка отмены (`type="button"`) рядом с отправкой.

#### Common pitfalls

- `<button>` без `type` случайно отправляет родительскую форму.
- Вставляем содержимое текстовой области. `value` атрибут (игнорируется).
- Опция пустого заполнителя без доступной метки для обязательного `<select>`.

**Pluses and minuses:**

| + | − |
|------|------|
| Встроенная клавиатура/специальные возможности | Ограниченный стиль и пользовательские раскрывающиеся списки |
| Для базовой отправки JS не требуется | `<button>` по умолчанию отправляется в формы |

**Examples:**

```html
<label for="country">Country</label>
<select id="country" name="country" required>
  <option value="">Choose…</option>
  <option value="de">Germany</option>
</select>

<label for="bio">Bio</label>
<textarea id="bio" name="bio" rows="4" maxlength="500"></textarea>

<button type="submit">Save</button>
<button type="button">Cancel</button>
```

**Sources:** [MDN — выберите](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select) · [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea) · [кнопка](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)

</details>

<details>
  <summary>Constraint Validation API</summary>

### Constraint Validation API

**Definition:**

HTML5 предоставляет встроенную проверку на стороне клиента с помощью атрибутов и API проверки ограничений в JavaScript.

| Атрибут/API | Поведение |
|-----------------|----------|
| `required` | Должно иметь значение |
| `pattern` | Соответствие регулярному выражению |
| `checkValidity()` | Возвращает логическое значение |
| `setCustomValidity()` | Пользовательское сообщение об ошибке |

**How it works:**

- HTML-атрибуты (`required`, `pattern`, `min`, `max`) установить ограничения.
- ЯС: `checkValidity()`, `reportValidity()`, `setCustomValidity(msg)`.
- `:valid`/`:invalid` псевдоклассы отражают состояние.

| API / атрибут | Поведение |
|------------|----------|
| `required` | Непустой |
| `setCustomValidity` | Пользовательское сообщение |
| `reportValidity` | Показать собственный интерфейс |

**When to use:**

Проверки формата на стороне клиента · пользовательская асинхронная проверка имени пользователя · `novalidate` + Проверка только на JS.

#### Common pitfalls

- Только проверка клиента (необходима повторная проверка на стороне сервера).
- `setCustomValidity('')` очистить — пустая строка означает действительную.
- `novalidate` забыли, полагаясь на пользовательский интерфейс.

**Examples:**

```html
<input type="text" name="code" pattern="[A-Z]{2}-\d{4}" title="Format: AB-1234" required />
```

**Thoughts:** Асинхронная проверка с помощью JS → [3_js_browser.md](./3_js_browser.md).

**Sources:** [MDN — проверка ограничений](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)

</details>

<details>
  <summary>Form submission attributes</summary>

### Form submission attributes

**Definition:**

Кнопки отправки могут переопределять настройки формы по умолчанию с помощью `formaction`, `formmethod`, `formenctype`, и `formnovalidate`.

| Атрибут | Переопределения |
|-----------|-----------|
| `formaction` | `action` URL-адрес |
| `formmethod` | `method` (получить/опубликовать) |
| `formenctype` | `enctype` |
| `formnovalidate` | Пропустить проверку этой отправки |

**How it works:**

- Кнопки отправки могут переопределять настройки по умолчанию для каждой кнопки.
- `formaction`/`formmethod`/`formenctype`/`formnovalidate` на кнопках отправки.
- `form` атрибут связывает контроль с удаленным `<form id>`.

| Атрибут | Переопределения |
|-----------|----------|
| `formaction` | `action` URL-адрес |
| `formnovalidate` | Пропустить проверку |

**When to use:**

Сохранить черновик или опубликовать · удалить с помощью `formmethod="post"` · кнопка вне формы через `form="id"`.

#### Common pitfalls

- Несколько кнопок отправки без отдельных `formaction`.
- `GET` формирует утечку конфиденциальных данных в URL-адресе.
- Отсутствующий `enctype="multipart/form-data"` для загрузки файлов.

**Examples:**

```html
<form action="/publish" method="post">
  <button type="submit">Publish</button>
  <button type="submit" formaction="/save-draft" formnovalidate>Save draft</button>
</form>
```

**Sources:** [MDN — кнопка](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)

</details>

<details>
  <summary>dialog element (native modal)</summary>

### dialog element (native modal)

**Definition:**

- `<dialog>` Элемент предоставляет встроенное модальное окно с фоном, ловушкой фокуса и рендерингом верхнего слоя — никаких проблем с z-index.

| Метод | Поведение |
|--------|----------|
| `showModal()` | Модал с фоном |
| `show()` | Немодальный |
| `close(returnValue)` | Закрывает диалог |

**How it works:**

- `<dialog>` рендерится в верхнем слое, когда `showModal()` называется.
- Щелчок по фону и закрытие Escape (настраивается).
- Ловушка фокуса и инертный фон обрабатываются браузером.

| Метод | Поведение |
|--------|----------|
| `showModal()` | Модальное + фон |
| `close()` | Возврат `returnValue` |

**When to use:**

Подтвердить удаление · модальный вход в систему · покадровый `show()` панели.

#### Common pitfalls

- Открытие без управления фокусом для пользовательского контента.
- Объединение нескольких модальных окон без плана UX.
- Замена на `<div role="dialog">` когда родной работает.

**Examples:**

```html
<dialog id="confirm">
  <p>Delete this item?</p>
  <button type="button" onclick="confirm.close()">Cancel</button>
  <button type="button" onclick="confirm.close('ok')">Delete</button>
</dialog>
<button type="button" onclick="confirm.showModal()">Open</button>
```

**Sources:** [MDN — диалог](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)

</details>

<details>
  <summary>Accessible form patterns</summary>

### Accessible form patterns

**Definition:**

Доступные формы связывают каждый элемент управления с меткой, группируют связанные поля и четко сообщают об ошибках.

| Узор | Реализация |
|---------|----------------|
| Ассоциация лейблов | `<label for="id">` |
| Группировка | `<fieldset>` + `<legend>` |
| Сообщение об ошибке | `aria-invalid` + `aria-describedby` |
| Мобильная клавиатура | `inputmode`, `enterkeyhint` |

**How it works:**

- Каждый элемент управления имеет видимый `<label for>` или упаковочная этикетка.
- Ошибки: `aria-invalid="true"` + `aria-describedby` указывая на текст ошибки.
- Группировать радио/флажки в `<fieldset>` с `<legend>`.

| Узор | Реализация |
|---------|----------------|
| Этикетка | `<label for="id">` |
| Ошибки | `aria-describedby` + `role="alert"` |

**When to use:**

Регистрационная форма · встроенная проверка · группа адресов из нескольких полей.

#### Common pitfalls

- `aria-label` неправильное скрытие видимой избыточности меток.
- Ошибка только по цвету (без текста).
- `autocomplete` отключен в полях входа (вредит менеджерам паролей).

**Examples:**

```html
<label for="phone">Phone</label>
<input id="phone" name="phone" type="tel" inputmode="tel" autocomplete="tel" aria-describedby="phone-hint" />
<span id="phone-hint">Include country code.</span>
```

**Sources:** [MDN — Доступные формы](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)

</details>

<details>
  <summary>autocomplete attribute reference</summary>

### autocomplete attribute reference

**Definition:** - `autocomplete` Атрибут предоставляет токены, которые сообщают браузеру, как автоматически заполнять поля формы, что улучшает совместимость UX и менеджера паролей.

**How it works:**

- Включить `<input>`, `<select>`, `<textarea>` (и `name` в форме для жетонов разделов).
- Значения представляют собой стандартизированные токены (`email`, `name`, `street-address`, `cc-number`, и т. д.).
- `autocomplete="off"` в одном поле могут быть проигнорированы браузерами для полей входа.

| Токен | Поле |
|-------|-------|
| `email` | Адрес электронной почты |
| `username` / `current-password` | Войти |
| `street-address` | Адресная строка |
| `cc-number` | Платежная карта |

**When to use:**

Формы входа · оформление заказа/доставка · одноразовые коды (`autocomplete="one-time-code"`).

#### Common pitfalls

- `autocomplete="email"` в поле имени.
- Отключение автозаполнения при входе в систему (расстраивает пользователей).
- Обычай `name` атрибуты без соответствующих токенов автозаполнения.

**Pluses and minuses:**

| + | − |
|------|------|
| Более быстрое заполнение формы | Неправильный токен неправильно заполняет поля |
| Интеграция менеджера паролей | `off` часто игнорируется при выборе паролей |

**Examples:**

```html
<input name="email" type="email" autocomplete="email" />
<input name="password" type="password" autocomplete="current-password" />
<input name="otp" autocomplete="one-time-code" />
```

**Sources:** [MDN — автозаполнение](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)

</details>

---

<a name="media--graphics"></a>

# VS Code & Browser — overview

Производительность редактора, расширения и инструменты разработчика браузера для разработки внешнего интерфейса.

### VS Code shortcuts (macOS)

| Действие                   | Ярлык            |
| -------------------------- | ---------------- |
| Палитра команд             | `Cmd+Shift+P`    |
| Быстрое открытие файла     | `Cmd+P`          |
| Переключить боковую панель | `Cmd+B`          |
| Закрыть вкладку            | `Cmd+W`          |
| Закрыть окно               | `Cmd+Shift+W`    |
| Окно перезагрузки          | `Cmd+R`          |
| Мультикурсор               | `Option+Click`   |
| Формат документа           | `Shift+Option+F` |

### Related

- [Build & ESLint/Prettier](./10_build.md) · [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](./1_html_performance.md#lcp-largest-contentful-paint) · [TTFB](./1_html_performance.md#ttfb-time-to-first-byte) · [CLS](./1*html* Performance.md#cls-cumulative-layout-shift) · [Интервью-хаб](./0_interview.md)

### Sections

- [VS Code](#vs-code)
- [Browser DevTools](#browser-devtools)
- [Ubuntu](#ubuntu)

---

## VS Code

**src** - https://www.freecodecamp.org/news/increase-your-vs-code-productivity/?ref=dailydev  
**vsc-ярлыки** – https://dev.to/usman_awan_003/20-vs-code-shortcuts-to-enhance-your-coding-productivity-2dp6?bb=177643

`auto-save` - настройки - часто используемые

`vsc` - https://code.visualstudio.com/  
`downlaod vsc` - https://www.youtube.com/watch?v=aogVqWGWaFk  
`usefull` - https://dev.to/idboussadel/how-to-become-a-10x-dev-ake?ref=dailydev

`create-files-and-folders` - https://dev.to/equiman/vscode-create-files-and-folders-on-the-go-2hd6  
`change-keyboard-shortcut-bindings` - https://stackoverflow.com/questions/33791097/how-can-i-change-keyboard-shortcut-bindings-in-visual-studio-code

<details>
  <summary>Extensions</summary>

### Extensions

#### ai

Tabnine: автозаполнение AI

#### common

ESLint  
Prettier — форматировщик кода  
Автозакрытие тега  
Автозаполнение тега  
Автоматический импорт  
Автоматическое переименование тега  
Живой сервер
Ошибка объектива
Проверка орфографии кода  
Предварительный просмотр Markdown Стилизация GitHub  
Код Бегун
Сохранение регистра нескольких курсоров

Консольный ниндзя  
Фрагменты регулярных выражений

Почтальон

#### github

Кодовые пространства GitHub  
Репозитории GitHub  
GitLens — усовершенствованный Git  
История Git  
Улучшен предварительный просмотр Markdown

#### html/css

HTML-шаблон  
Поддержка HTML CSS  
компоненты в стиле vscode  
CSS IntelliSense для попутного ветра

Обзор CSS

#### js/ts/react

Фрагменты кода JavaScript (ES6)  
JavaScript и TypeScript Nightly  
Фрагменты ES7+ React/Redux/React-Native  
Простые фрагменты React  
Симпатичные ошибки TypeScript
Квокка.js

Модули Node.js Intellisense

#### test

кодиат

#### db

MongoDB для VS Code

#### docker

Докер  
Контейнеры разработки  
ВСЛ

</details>

<details>
  <summary>Shortcuts</summary>

### Shortcuts

`ctrl + shift + e` | `ctrl + 0` сосредоточься на проводнике  
`ctrl + shift + n` - открыть новый терминал  
`ecs - вернуться назад, если вы находитесь в папке или файле

`ctrl + shift + k` - удалить строку кода  
`ctrl + shift + c` - добавить файл  
`ctrl + shift + v` - добавить папку
`alt + 1(любое число) - переключиться на открытый файл

`Ctrl + P` || `Ctrl + E`: быстрая навигация по файлам, позволяющая открывать файлы по имени.  
`Ctrl + /`: переключить комментарий к строке  
`Ctrl + A`: выбирает все строки в текущем файле.  
`Ctrl + L` || `Ctrl + Shift + Up/Down Arrow`: выбрать текущую строку  
`Alt + Up/Down Arrow`: Переместить текущую строку вверх или вниз.

`Ctrl + F`: находит определенный текст в коде.  
`Ctrl + Shift + L`: выбрать все вхождения текущего слова.

`Alt + Click`: удерживайте нажатой клавишу Alt и щелкайте в разных местах кода, чтобы создать несколько курсоров, что позволит редактировать или вводить эти места одновременно.  
`Ctrl + Shift + P`: открыть палитру команд для различных команд.

</details>

<details>
  <summary>Settings</summary>

### Settings

"prettier.printWidth": 120,

Чтобы добавить console.log, создайте файл и папку в качестве ярлыка, используя настройки файла — сочетания клавиш, затем значок вверху => справа.

```
 {
    "key": "ctrl+1",
    "command": "editor.action.insertSnippet",
    "args": {
      "snippet": "const ${1:name} = $0"
    }
  },
  {
    "key": "ctrl+2",
    "command": "editor.action.insertSnippet",
    "args": {
      "snippet": "let ${1:name} = $0"
    }
  },
  {
    "key": "ctrl+3",
    "command": "editor.action.insertSnippet",
    "args": {
      "snippet": "const ${1:name} = ($2) => {\n\t$0\n}"
    }
  },
  {
    "key": "shift+c",
    "command": "editor.action.insertSnippet",
    "args": {
      "snippet": "console.log($1);$0"
    }
  },
  {
    "key": "shift+c",
    "command": "editor.action.insertSnippet",
    "args": {
      "snippet": "console.log($1);$0"
    }
  },
  {
    "key": "ctrl+shift+c",
    "command": "explorer.newFile"
  },
  {
    "key": "ctrl+shift+v",
    "command": "explorer.newFolder"
  },
  {
    "key": "ctrl+5",
    "command": "editor.emmet.action.wrapWithAbbreviation"
  }
```

в keybindings.json

добавить форматтер

```
{
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.formatOnType": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "[html]": {
        "editor.defaultFormatter": "vscode.html-language-features"
    }
}
```

</details>

<details>
  <summary>Terminal</summary>

### Terminal

pwd — получить текущее направление
ls ( ls .env) + тег для автозаполнения  
бывает относительный и абсолюбтный путь(/)
mkdir «имя» — создать каталог
флаг - p использовать для создания вложенного каталога
«имя» дерева — для получения дерева
mv "name1" "name2" - для изменения имени или перемещения имени
mv "name1/_" "name2" - удалить содержимое имени в name2
rm «имя» — удалить файл (-r для удаления всего содержимого) (rf)
rm -r _ удалить все содержимое каталогаSet-Location -Path D: — сменить каталог
Get-ChildItem
используйте для просмотра истории команд во всех сеансах PS
(Get-PSReadlineOption).HistorySavePath
кот (Get-PSReadlineOption).HistorySavePath

</details>

<details>
  <summary>DevTools — Core Web Vitals</summary>

### DevTools — Core Web Vitals

**Definition:** Chrome DevTools — это лабораторный инструмент для _воспроизведения_ LCP, INP и CLS после того, как полевые данные (CrUX/Search Console) сообщают вам, какая метрика плохая. Полное поле и лабораторные примечания: [6_web_api.md — основные веб-показатели](./1_html_performance.md#core-web-vitals). Элемент LCP и четыре подраздела: [6_web_api.md — LCP](./1_html_performance.md#lcp-largest-contentful-paint).

**How it works:**

- **Маяк** (панель, CLI, CI) — лабораторный аудит + рекомендации. В запуске по умолчанию нет пользователя, поэтому **INP не является настоящим INP**; **TBT** — прокси-сервер лаборатории.
- **Сеть** — Ожидание запроса документа / TTFB. Полная метрика: [6_web_api.md — TTFB](./1_html_performance.md#ttfb-time-to-first-byte).
- **Производительность** — запись трассировки. Включить снимки экрана.Для INP: взаимодействуйте во время записи, затем проверьте дорожку **Взаимодействия** (задержка ввода/обработка/задержка представления). Красные треугольники в стеке вызовов обозначают длинные обработчики. **Сдвиги макета** отслеживают для CLS: какие узлы были перемещены и сколько точек они добавили. Боковая панель **Статистика** суммирует распространенные проблемы. Метрика + исправления: [1_html.md — CLS](./1*html* Performance.md#cls-cumulative-layout-shift).- Если локально быстро: дросселируйте процессор 4×/6× или выполните удаленную отладку настоящего телефона Android.
- **Приложение → Кэш назад/вперед → Тест** — может ли страница использовать **bfcache** (мгновенное восстановление при возврате/пересылке; LCP/FCP/CLS часто ~0). Chrome перечисляет блокировщики. `unload` / `beforeunload` часто ломаю — пользуйтесь `pagehide`. `Cache-Control: no-store` тоже может.

**When to use:** Локальное воспроизведение поля регрессии INP/CLS/LCP перед написанием заявки.

**Thoughts:** DevTools объясняет _почему_; CruUX p75 решает, будет ли он отправлен.

**Sources:** [web.dev — TBT](https://web.dev/articles/tbt) · [Производительность Chrome DevTools](https://developer.chrome.com/docs/devtools/ Performance)

</details>

<a name="browser-devtools"></a>

## Browser DevTools

<details>
  <summary>Debugging JS (Sources)</summary>

### Debugging JS (Sources)

**Definition:** Панель **Источники** приостанавливает выполнение скрипта, чтобы вы могли проверить переменные, стек вызовов и пошагово выполнить код. Три способа сделать паузу: **точка останова**, `debugger` оператор или пауза при исключении (когда DevTools открыт).

**How it works:**

- Щелкните номер строки, чтобы установить точку останова; щелкните правой кнопкой мыши → **условная** точка останова (останавливается только тогда, когда выражение истинно).
- `debugger;` in source приостанавливается только тогда, когда DevTools открыт — в противном случае браузеры игнорируют его.
- Во время паузы: **Область** (локальные/глобальные значения), **Наблюдение** (выражения), **Стек вызовов** (вложенные вызовы — щелкните кадр, чтобы перейти).
- Пошаговый режим: Возобновить (F8), Шаг вперед (F10 — переход к вложенным вызовам), Шаг вперед (F11), Выход (Shift+F11). Щелкните строку правой кнопкой мыши → **Продолжить здесь**.
- `console.log` /Esc консоль для отладки печати без паузы. Подробности API консоли → [Язык JS — Консоль](./3_js_core.md).

**When to use:** Воспроизвести неправильное значение или неожиданную ветвь; предпочитают точки останова вместо переполнения журналов. Пропускайте регистрацию секретов в производственных сборках.

**Examples:**

```js
function hello(name) {
  const phrase = `Hello, ${name}!`;
  debugger; // pauses here if DevTools is open
  say(phrase);
}
```

**Thoughts:** Пауза → прочитать область действия/стек вызовов → шаг. Этот цикл лучше, чем угадывание по разбросанным `console.log`s.

**Sources:** [javascript.info/debugging-chrome](https://javascript.info/debugging-chrome) · [Chrome DevTools — отладка JavaScript](https://developer.chrome.com/docs/devtools/javascript/)

</details>

# Extension for browser

### Chrome extensions

- Средство просмотра JSON – https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh/based.
- ColorZilla - https://chrome.google.com/webstore/detail/colorzilla/bhlhnicpbhignbdhedgjhgdocnmhomnp
- Wappalyzer - https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg- Поддельный наполнитель - https://chrome.google.com/webstore/detail/fake-filler/bnjjngeaknajbdcgpfkgnonkmififhfo
- PerfectPixel – https://chrome.google.com/webstore/detail/perfectpixel-by-welldonec/dkaagdgjmgdmbnecmcefdhjekcoceebi
- Octotree - дерево кода GitHub - https://chrome.google.com/webstore/detail/octotree-github-code-tree/bkhaagjahfmjljalopjnoealnfndnagc
- Гитхант - https://githunt.io/

### DevTools extensions

- Контрольный список для веб-разработчика – https://chrome.google.com/webstore/detail/web-developer-checklist/iahamcpedabephpcgkeikbclmaljebjp.
- Тестер API Talend - https://chrome.google.com/webstore/detail/talend-api-tester-free-ed/aejoelaoggembcahagimdiliamlcdmfm
- Очистить кэш – https://chrome.google.com/webstore/detail/clear-cache/cppjkneekbjaeellbfkmgnhonkkjfpdn.– Веб-разработчик – https://chrome.google.com/webstore/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm

### Accessibility extensions

- Шрифты Ninja - https://chrome.google.com/webstore/detail/fonts-ninja/eljapbgkmlngdpckoiiibecpemleclhh
- Проверьте мои ссылки – https://chrome.google.com/webstore/detail/check-my-links/ojkcdipcgfaekbeaelaapakgnjflfglf.
- Инструмент оценки WAVE – https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh.

## Ubuntu

explorer.exe. - открыть проводник Windows

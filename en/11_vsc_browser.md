# VS Code & Browser — overview

Editor productivity, extensions, and browser DevTools for frontend development.

### VS Code shortcuts (macOS)

| Action          | Shortcut         |
| --------------- | ---------------- |
| Command palette | `Cmd+Shift+P`    |
| Quick open file | `Cmd+P`          |
| Toggle sidebar  | `Cmd+B`          |
| Close tab       | `Cmd+W`          |
| Close window    | `Cmd+Shift+W`    |
| Reload window   | `Cmd+R`          |
| Multi-cursor    | `Option+Click`   |
| Format document | `Shift+Option+F` |

### Related

- [Build & ESLint/Prettier](./10_build.md) · [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](./1_html_performance.md#lcp-largest-contentful-paint) · [TTFB](./1_html_performance.md#ttfb-time-to-first-byte) · [CLS](./1_html_performance.md#cls-cumulative-layout-shift) · [Interview hub](./0_interview.md)

### Sections

- [VS Code](#vs-code)
- [Browser DevTools](#browser-devtools)
- [Ubuntu](#ubuntu)

---

## VS Code

**src** - https://www.freecodecamp.org/news/increase-your-vs-code-productivity/?ref=dailydev  
**vsc-shortcuts** - https://dev.to/usman_awan_003/20-vs-code-shortcuts-to-enhance-your-coding-productivity-2dp6?bb=177643

`auto-save` - settings - commonly used

`vsc` - https://code.visualstudio.com/  
`downlaod vsc` - https://www.youtube.com/watch?v=aogVqWGWaFk  
`usefull` - https://dev.to/idboussadel/how-to-become-a-10x-dev-ake?ref=dailydev

`create-files-and-folders` - https://dev.to/equiman/vscode-create-files-and-folders-on-the-go-2hd6  
`change-keyboard-shortcut-bindings` - https://stackoverflow.com/questions/33791097/how-can-i-change-keyboard-shortcut-bindings-in-visual-studio-code

<details>
  <summary>Extensions</summary>

### Extensions

#### ai

Tabnine: AI Autocomplete

#### common

ESLint  
Prettier - Code formatter  
Auto Close Tag  
Auto Complete Tag  
Auto Import  
Auto Rename Tag  
Live Server
Error Lens
Code Spell Checker  
Markdown Preview GitHub Styling  
Code Runner
Multiple cursor case preserve

Console Ninja  
Regex Snippets

Postman

#### github

GitHub Codespaces  
GitHub Repositories  
GitLens — Git supercharged  
Git History  
Markdown Preview Enhanced

#### html/css

HTML Boilerplate  
HTML CSS Support  
vscode-styled-components  
Tailwind CSS IntelliSense

CSS Peek

#### js/ts/react

JavaScript (ES6) code snippets  
JavaScript and TypeScript Nightly  
ES7+ React/Redux/React-Native snippets  
Simple React Snippets  
Pretty TypeScript Errors
Quokka.js

Node.js Modules Intellisense

#### test

Codiumate

#### db

MongoDB for VS Code

#### docker

Docker  
Dev Containers  
WSL

</details>

<details>
  <summary>Shortcuts</summary>

### Shortcuts

`ctrl + shift + e` | `ctrl + 0` focus on explorer  
`ctrl + shift + n` - open a new terminal  
`ecs - go back if you on folder or file

`ctrl + shift + k` - delete line of code  
`ctrl + shift + c` - add file  
`ctrl + shift + v` - add folder
`alt + 1(any num) - switch to am opened file

`Ctrl + P` || `Ctrl + E`: Quick file navigation, allowing you to open files by name.  
`Ctrl + /`: Toggle line comment  
`Ctrl + A`: Selects all the lines in the current file  
`Ctrl + L` || `Ctrl + Shift + Up/Down Arrow`: Select current line  
`Alt + Up/Down Arrow`: Move the current line up or down.

`Ctrl + F`: Finds a specific text in the code  
`Ctrl + Shift + L`: Select all occurrences of the current word.

`Alt + Click`: Hold down the Alt key and click at different locations in the code to create multiple cursors, allowing you to edit or type in those locations simultaneously.  
`Ctrl + Shift + P`: Open the command palette for various commands.

</details>

<details>
  <summary>Settings</summary>

### Settings

"prettier.printWidth": 120,

To add console.log, create file and folder as shortcut use file-preferences - keyboardshorcuts then icon top=>right

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

in keybindings.json

add formatter

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

pwd - get current direction
ls ( ls .env) + tag for autocomplete  
бывает относительный и абсолюбтный путь(/)
mkdir "name" - create directory
flag - p use for create nested directory
tree "name" - for get a tree
mv "name1" "name2" - for change name or move name
mv "name1/_" "name2" - remvoe content of name in name2
rm "name" - delete file (-r for delete all content)(rf)
rm -r _ delete all contetnt of directory

Set-Location -Path D: - change directory
Get-ChildItem
use for see the command history across all PS sessionns
(Get-PSReadlineOption).HistorySavePath
cat (Get-PSReadlineOption).HistorySavePath

</details>

<details>
  <summary>DevTools — Core Web Vitals</summary>

### DevTools — Core Web Vitals

**Definition:** Chrome DevTools is the lab tool for _reproducing_ LCP, INP, and CLS after field data (CrUX / Search Console) tells you which metric is bad. Full field vs lab notes: [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals). LCP element and four sub-parts: [6_web_api.md — LCP](./1_html_performance.md#lcp-largest-contentful-paint).

**How it works:**

- **Lighthouse** (panel, CLI, CI) — lab audit + recommendations. Default run has no user, so **INP is not real INP**; **TBT** is the lab proxy.
- **Network** — document request Waiting / TTFB. Full metric: [6_web_api.md — TTFB](./1_html_performance.md#ttfb-time-to-first-byte).
- **Performance** — record a trace. Enable Screenshots. For INP: interact during the recording, then inspect the **Interactions** track (input delay / processing / presentation delay). Red triangles in the call stack mark long handlers. **Layout Shifts** track for CLS: which nodes moved and how many points they added. Side panel **Insights** summarizes common issues. Metric + fixes: [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift).
- If it is fast locally: CPU throttle 4× / 6×, or remote-debug a real Android phone.
- **Application → Back/forward cache → Test** — whether the page can use **bfcache** (instant restore on Back/Forward; LCP/FCP/CLS often ~0). Chrome lists blockers. `unload` / `beforeunload` often break it — use `pagehide`. `Cache-Control: no-store` can too.

**When to use:** Local reproduction of a field INP/CLS/LCP regression before writing a ticket.

**Thoughts:** DevTools explains _why_; CrUX p75 decides _if it shipped_.

**Sources:** [web.dev — TBT](https://web.dev/articles/tbt) · [Chrome DevTools Performance](https://developer.chrome.com/docs/devtools/performance)

</details>

<a name="browser-devtools"></a>

## Browser DevTools

<details>
  <summary>Debugging JS (Sources)</summary>

### Debugging JS (Sources)

**Definition:** The **Sources** panel pauses script execution so you can inspect variables, the call stack, and step through code. Three ways to pause: a **breakpoint**, the `debugger` statement, or pause-on-exceptions (when DevTools is open).

**How it works:**

- Click a line number to set a breakpoint; right-click → **conditional** breakpoint (stops only when an expression is truthy).
- `debugger;` in source pauses only when DevTools is open — browsers ignore it otherwise.
- While paused: **Scope** (locals/globals), **Watch** (expressions), **Call Stack** (nested calls — click a frame to jump).
- Stepping: Resume (F8), Step over (F10 — skip into nested calls), Step into (F11), Step out (Shift+F11). Right-click a line → **Continue to here**.
- `console.log` / Esc console for print debugging without pausing. Console API detail → [JS Language — Console](./3_js_core.md).

**When to use:** Reproduce a wrong value or unexpected branch; prefer breakpoints over flooding logs. Skip logging secrets in production builds.

**Examples:**

```js
function hello(name) {
  const phrase = `Hello, ${name}!`;
  debugger; // pauses here if DevTools is open
  say(phrase);
}
```

**Thoughts:** Pause → read Scope/Call Stack → step. That loop beats guessing from scattered `console.log`s.

**Sources:** [javascript.info/debugging-chrome](https://javascript.info/debugging-chrome) · [Chrome DevTools — debug JavaScript](https://developer.chrome.com/docs/devtools/javascript/)

</details>

# Extension for browser

### Chrome extensions

- JSON Viewer - https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh/related
- ColorZilla - https://chrome.google.com/webstore/detail/colorzilla/bhlhnicpbhignbdhedgjhgdocnmhomnp
- Wappalyzer - https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg
- Fake Filler - https://chrome.google.com/webstore/detail/fake-filler/bnjjngeaknajbdcgpfkgnonkmififhfo
- PerfectPixel - https://chrome.google.com/webstore/detail/perfectpixel-by-welldonec/dkaagdgjmgdmbnecmcefdhjekcoceebi
- Octotree - GitHub code tree - https://chrome.google.com/webstore/detail/octotree-github-code-tree/bkhaagjahfmjljalopjnoealnfndnagc
- Githunt - https://githunt.io/

### DevTools extensions

- Web Developer Checklist - https://chrome.google.com/webstore/detail/web-developer-checklist/iahamcpedabephpcgkeikbclmaljebjp
- Talend API Tester - https://chrome.google.com/webstore/detail/talend-api-tester-free-ed/aejoelaoggembcahagimdiliamlcdmfm
- Clear Cache - https://chrome.google.com/webstore/detail/clear-cache/cppjkneekbjaeellbfkmgnhonkkjfpdn
- Web Developer - https://chrome.google.com/webstore/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm

### Accessibility extensions

- Fonts Ninja - https://chrome.google.com/webstore/detail/fonts-ninja/eljapbgkmlngdpckoiiibecpemleclhh
- Check My Links - https://chrome.google.com/webstore/detail/check-my-links/ojkcdipcgfaekbeaelaapakgnjflfglf
- WAVE Evaluation Tool - https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh

## Ubuntu

explorer.exe . - open windows explorer

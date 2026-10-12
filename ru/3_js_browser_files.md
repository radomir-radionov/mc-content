# JS Browser — Files & binary

`FormData`, `Blob`, `File`, `ArrayBuffer`, text encoders, and object URLs for uploads and previews.

[← Back to JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [Network from JS](./3_js_browser_network.md) · [Browser APIs](./3_js_browser_extra.md#url--urlsearchparams)
- [HTTP in JavaScript](./6_web_api_http.md#http-in-javascript) · [OS files](./8_nodejs_modules.md#files--file-systems)
- [JS Interview](./3_js_interview.md)

### Main concepts

| Area | Topics |
| ---- | ------ |
| **Binary** | `ArrayBuffer`, typed arrays, `DataView` |
| **Text** | `TextEncoder`, `TextDecoder` |
| **Blobs** | `Blob`, `File`, object URLs |
| **Upload** | `FormData`, `fetch` / XHR bodies |

### Sections

- [Binary data and files](#binary-data-and-files)
- [ArrayBuffer and binary arrays](#arraybuffer-and-binary-arrays)
- [TextDecoder and TextEncoder](#textdecoder-and-textencoder)
- [Blob](#blob)
- [File and FileReader](#file-and-filereader)
- [FormData](#formdata)
- [URL.createObjectURL](#url-createobjecturl)

---

<a name="binary-data-and-files"></a>

## Binary data and files

<details>
  <summary>Binary data and files</summary>

### Binary data and files

**Definition:** The browser does not expose arbitrary paths on disk. Scripts work with **`File`** objects the user picked, **`Blob`** in-memory bytes, or binary downloaded via `fetch`.

**How it works:** Pick files from `<input type="file">` or drag-and-drop (`dataTransfer.files`). Send bytes with `fetch` / XHR (`FormData`, raw `Blob`, or `ArrayBuffer`). Decode with typed arrays or `TextDecoder`.

**When to use:** Uploads, CSV import, canvas export, parsing binary API responses.

**Sources:** [javascript.info/binary](https://javascript.info/binary) · [MDN — File API](https://developer.mozilla.org/en-US/docs/Web/API/File_API)

</details>

<a name="arraybuffer-and-binary-arrays"></a>

## ArrayBuffer and binary arrays

<details>
  <summary>ArrayBuffer and binary arrays</summary>

### ArrayBuffer and binary arrays

**Definition:** An **`ArrayBuffer`** is a fixed-length raw byte buffer. It is **not** a JavaScript `Array` — use **views** to read and write.

**How it works:**

- **`ArrayBuffer(byteLength)`** — allocates bytes; length is fixed (no `push`).
- **Typed arrays** (`Uint8Array`, `Int16Array`, `Float32Array`, …) — interpret the same buffer with a type and endianness.
- **`DataView`** — read/write mixed widths at arbitrary offsets (useful for file formats).
- **`buffer.slice()`** copies bytes; sharing: multiple views on one buffer.
- From network: `await response.arrayBuffer()`; from file: `await file.arrayBuffer()`.

**When to use:** Protocol parsing, crypto, audio samples, compression before upload.

#### Common pitfalls

Treating `ArrayBuffer` like a normal array; forgetting endianness with `DataView`; huge buffers blocking the main thread.

**Examples:**

```js
const buf = new ArrayBuffer(8);
const view = new DataView(buf);
view.setUint32(0, 0xdeadbeef);
const bytes = new Uint8Array(buf);
```

**Sources:** [javascript.info/arraybuffer-binary-arrays](https://javascript.info/arraybuffer-binary-arrays) · [MDN — ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)

</details>

<a name="textdecoder-and-textencoder"></a>

## TextDecoder and TextEncoder

<details>
  <summary>TextDecoder and TextEncoder</summary>

### TextDecoder and TextEncoder

**Definition:** **`TextEncoder`** converts JavaScript strings to UTF-8 bytes (`Uint8Array`). **`TextDecoder`** converts bytes back to strings.

**How it works:**

- `new TextEncoder().encode("привет")` → `Uint8Array`.
- `new TextDecoder("utf-8").decode(uint8)` → string; optional `stream: true` for chunked input.
- Legacy encodings depend on browser support — UTF-8 is the default for web APIs.

**When to use:** After `arrayBuffer()` from `fetch` or `File`; before building a `Blob` from text.

**Examples:**

```js
const bytes = new TextEncoder().encode("hello");
const text = new TextDecoder().decode(bytes);
```

**Sources:** [javascript.info/text-decoder](https://javascript.info/text-decoder) · [MDN — TextEncoder](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder)

</details>

<a name="blob"></a>

## Blob

<details>
  <summary>Blob</summary>

### Blob

**Definition:** A **`Blob`** is immutable binary data plus an optional MIME **`type`**.

**How it works:**

- `new Blob([parts], { type: "text/plain" })` — `parts` can be strings, `ArrayBuffer`, or other blobs.
- `blob.slice(start, end, type)` — cheap sub-range without copying entire payload in many engines.
- `await blob.text()` / `blob.arrayBuffer()` — modern async readers (prefer over `FileReader` when available).
- `URL.createObjectURL(blob)` — temporary `blob:` URL for `<img src>`, `<a download>`, etc. — **revoke** when done.

**When to use:** Client-generated files, preview before upload, download links.

**Sources:** [javascript.info/blob](https://javascript.info/blob) · [MDN — Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

</details>

<a name="file-and-filereader"></a>

## File and FileReader

<details>
  <summary>File and FileReader</summary>

### File and FileReader

**Definition:** **`File`** extends **`Blob`** with `name` and `lastModified` (from a file picker or drag-drop). **`FileReader`** reads a `Blob`/`File` asynchronously via events.

**How it works:**

- `input.files[0]` — first selected `File`.
- `FileReader`: `readAsText`, `readAsDataURL`, `readAsArrayBuffer` — listen for `load` / `error`.
- Prefer `file.text()` / `file.arrayBuffer()` when supported — simpler than `FileReader`.

**When to use:** Local preview; parsing user CSV/JSON; legacy code paths still on `FileReader`.

**Examples:**

```js
const file = input.files[0];
const text = await file.text();
```

**Sources:** [javascript.info/file](https://javascript.info/file) · [MDN — FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)

</details>

<a name="formdata"></a>

## FormData

<details>
  <summary>FormData</summary>

### FormData

**Definition:** **`FormData`** builds a **`multipart/form-data`** body for `fetch` or `XMLHttpRequest`.

**How it works:**

- `new FormData(formElement)` — serializes fields; or `fd.append(name, value)` / `fd.set(name, value)` (set replaces same name).
- File field: `fd.append("avatar", file, file.name)`.
- Pass `body: fd` to `fetch` — **do not** set `Content-Type` manually (browser adds boundary).
- Upload progress is **not** on `fetch` — use XHR `upload.onprogress` ([Network](./3_js_browser_network.md#xmlhttprequest)).

**Examples:**

```js
const fd = new FormData(form);
fd.append("shot", canvasBlob, "shot.png");
await fetch("/upload", { method: "POST", body: fd });
```

**Sources:** [javascript.info/formdata](https://javascript.info/formdata) · [MDN — FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

</details>

<a name="url-createobjecturl"></a>

## URL.createObjectURL

<details>
  <summary>URL.createObjectURL</summary>

### URL.createObjectURL

**Definition:** Creates a temporary **`blob:`** URL referencing in-memory `Blob`/`File` data until revoked.

**How it works:** `const url = URL.createObjectURL(blob)` → use in `src` or `href` → `URL.revokeObjectURL(url)` when finished.

**When to use:** Image/video preview before upload; instant download link for generated PDF.

#### Common pitfalls

Never revoking → memory leak; storing blob URLs in `localStorage` (invalid after reload).

**Examples:**

```js
const url = URL.createObjectURL(file);
img.src = url;
img.onload = () => URL.revokeObjectURL(url);
```

**See also:** [URL / URLSearchParams](./3_js_browser_extra.md#url--urlsearchparams) for `https:` parsing.

**Sources:** [MDN — URL.createObjectURL](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL_static)

</details>

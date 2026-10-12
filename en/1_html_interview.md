# HTML — Interview

HTML interview questions. Answers are filled for the published subset; the rest are still empty.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — HTML5](https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/).

### HTML Basics

<details>
<summary>What does HTML stand for and what is its purpose?</summary>

### What does HTML stand for and what is its purpose?

**Definition:**

**HTML** or **H**yper **T**ext **M**arkup **L**anguage is the standard language for creating web pages and applications. HTML5, the latest version as of 2022, introduces several new elements and attributes, elevating user experience and software application standards.

HTML is responsible for structuring web content, ensuring accessibility, and guiding how web pages are visually presented. It remains the foundational structure for running nearly all web content.

**How it works:**

#### Core Functionalities

- **Structuring Content**: Tags like `<header>`, `<footer>`, and `<section>` divide content, streamlining its organization.
- **Embedding Media**: HTML provides tags to incorporate multimedia such as images, audio, and video.
- **Form Handling**: Interactive sections such as user input forms are defined with input and label tags.
- **Hyperlinks**: Essential for navigation, hypertext links like `<a>` anchor content within or outside the webpage.
- **Accessibility Features**: Semantic tags like `<nav>` and `<article>` not only structure data but also improve accessibility for users relying on screen readers.
- **Integration of Other Technologies**: Can integrate with scripting languages like JavaScript and libraries and frameworks like Bootstrap for enhanced visual appeal.

#### Compatibility and Development

The primary objective of HTML5 is to improve the language's support for the latest multimedia, while keeping it easily readable by humans. It was designed to be backward and forward compatible, so that content written in previous versions can be seamlessly integrated and interact with content authored in subsequent versions.

#### Visual Presentation and User Interface Adaptations

HTML5 is engineered to provide more flexibility, control, and aesthetic maturity for web pages and web-based software applications. Web developers can use it to craft modern web interfaces with rich visual and multimedia experiences. It also allows for more responsive and adaptive design, ensuring optimal viewing on a variety of devices and screen sizes. This reflects a broader shift in technology toward a more device-agnostic user experience. 

#### The Role of CSS and JavaScript

While HTML offers static content, **CSS** and **JavaScript** enable additional layers of styling, interactivity, and dynamic content updates. The integration of these three technologies (referred to as **HTML-CSS-JS**) stands as the trio that forms the backbone of almost all web-based content. They're often presented as **HTML5-CSS3-JS** to signify unified modern best practices. Online, there's even tools that combine these technologies into a  single framework or language such as WebAssembly or Dart. The trio represents a more modular approach, allowing distinct teams to focus on individual layers, streamlining development in larger projects. Mastering their intersection helps in designing a robust and cohesive user experience. This concept is captured by the acronym "PEA", which stands for the Platform (HTML), the Engine (JavaScript), and the Appearance (CSS). Each category focuses on a distinct aspect of user experience.

#### Practical Uses

- **Website Development**: All traditional web resources, from simple blogs to expansive e-commerce sites, remain based primarily on HTML.
- **Web Applications**: Web technologies have evolved significantly, allowing for sophisticated applications like Google Docs, Trello, and Slack to run entirely in a web browser. HTML5 has played a pivotal role in this development.
- **Advertising & Media**: HTML5's advanced media handling tools have made it the standard for online ads and multimedia content.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML
- https://html.spec.whatwg.org/
</details>

<details>
<summary>Describe the basic structure of an HTML document.</summary>

### Describe the basic structure of an HTML document.

**Definition:**

**HyperText Markup Language** (HTML) serves as the backbone of web content, defining its structure and semantics. We will now walk you through the fundamental elements of an HTML document.

**How it works:**

#### Basic Structure of an HTML Document

An HTML document consists of two primary sections: the `head` and the `body`.

##### Document Type Declaration (DOCTYPE)

The **Document Type Declaration** (DOCTYPE) is not an HTML tag; it's an instruction to the web browser about what version of HTML the page is written in.

```html
<!DOCTYPE html>
```

This declaration shows that the document is an HTML5 document.

##### HTML Element

The `html` element is the root element of an HTML page. It encompasses the entire content, both head and body.

```html
<html>
    <!-- Head and Body Sections Are Nested Inside -->
</html>
```

##### Head Section

The `head` section provides meta-information about the document. It isn't displayed in the web browser itself but serves various other purposes, from providing a title to linking external resources.

```html
<head>
    <!-- Title and Meta-Tags, Styles, Scripts, etc. -->
</head>
```

###### Title Element

The `title` element specifies the document's title, which is displayed in the browser's title bar or tab.

```html
<title>Your Page Title</title>
```

##### Body Section

The `body` section encapsulates the document's visible content—what users see and interact with.

```html
<body>
    <!-- Content Visible to Users: Headings, Paragraphs, Images, etc. -->
</body>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html
- https://developer.mozilla.org/en-US/docs/Glossary/Doctype
</details>

<details>
<summary>What do DOCTYPE and html lang attributes do?</summary>

### What do DOCTYPE and html lang attributes do?

**Definition:**

**Document Type** (DOCTYPE) and the `lang` attribute play crucial roles in our webpages.

**How it works:**

#### DOCTYPE: Defining Document Type and Validation Mode

##### Purpose
- Specifies the HTML or XHTML version used in the document.
- Identifies parsing method and algorithm for the web browser, affecting consistency.

##### Code Example

The `<!DOCTYPE>` declaration is placed at the very top of the HTML file, even before the `<html>` tag begins.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <!-- Content -->
  </body>
</html>
```

#### Lang Attribute: Language Specification

The `lang` attribute, present in the HTML tag, specifies the primary language used in the document. Its value is a primary language subtag as defined in RFC 5646 (BCP 47) and it can include a valid language code, a valid language code followed by a valid region code, or simply "und" for unspecified language.

##### Code Example

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>Welcome</h1>
    <p>This is a demo page.</p>
  </body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang
</details>

<details>
<summary>What is the difference between head and body tags?</summary>

### What is the difference between head and body tags?

**Definition:**

While the **\<head>** and **\<body>** tags are fundamental to every HTML document, they serve distinct purposes and are located in separate areas of the web page.

**How it works:**

#### Key Distinctions

##### 1. Role and Content

- **Head**: Houses meta-information, such as document title, character encoding, and stylesheets, all of which are essential for page setup but not visible to the user.
- **Body**: Contains the bulk of visible content, including text, images, videos, links, and more.

##### 2. Placement in the HTML File

- **Head**: Precedes the body and provides setup before actual content is rendered.
- **Body**: Follows the head section and encompasses all visible content.

##### 3. Common Elements in Each Section

- **Head**: Typically links to CSS files or may have inline CSS, contains the document title, any JavaScript reference, character set declaration, and meta tags.
- **Body**: Holds structural components like headers, navbars, articles, sections, and the footer, along with visual content like images and visible text.

#### Visual Representation in the HTML File

- **\<head> Section**:

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <link rel="stylesheet" type="text/css" href="styles.css">
    </head>
    <body>
      <!-- Content Here -->
    </body>
  </html>
  ```

- **\<body> Section**:

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <link rel="stylesheet" type="text/css" href="styles.css">
    </head>
    <body>
      <header>
        <h1>Welcome!</h1>
        <nav>
          <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#">About</a></li>
            <li><a href="#">Contact</a></li>
          </ul>
        </nav>
      </header>

      <section>
        <h2>Recent Posts</h2>
        <article>
          <h3>Post Title</h3>
          <p>Post content goes here.</p>
        </article>
      </section>

      <footer>
        <p>&copy; 2023 MySite</p>
      </footer>

    </body>
  </html>
  ```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/body
</details>

<details>
<summary>Can you explain the purpose of meta tags in HTML?</summary>

### Can you explain the purpose of meta tags in HTML?

**Definition:**

**Meta tags** provide metadata about a web page through information invisible to visitors but essential for search engines, social media, and other web technology. This metadata includes details such as the page's title, keywords, and description.

**How it works:**

#### Key Meta Tags

- **Meta Description**: A concise summary of the page's content, often used in search engine results.

- **Meta Keywords**: Historically used to specify relevant keywords for the page, but they have been largely deprecated due to abuse by spammers.

- **Meta Robots**: Directs search engine bots on how to interact with the page, such as index it for search results, follow its links, or refrain from both.

- **Meta Viewport**: Crucial for responsive design, it guides the browser on how to scale and display the page, especially useful for mobile devices.

- **Meta Charset**: Defines the character encoding used on the webpage, ensuring text is displayed correctly.

- **Meta Author**: Identifies the page's creator or author.

- **Open Graph, Twitter Cards**: Specialized meta tags used by social platforms like Facebook and Twitter to optimize page sharing.

- **Canonical URL**: Indicates the preferred URL when a page can be accessed through multiple paths.

- **Refresh and Redirect**: Older, less common meta tags that dictate page behavior.

#### Code Example: Common Meta Tags

Here is the HTML code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="description" content="This is a sample web page with a concise description.">
    <meta name="keywords" content="HTML, meta tags, web design, SEO">
    <meta name="author" content="John Doe">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sample Web Page</title>
</head>
<body>
    <!-- Page content goes here -->
</body>
</html>
```

#### Responsible Use of Meta Tags

With search engines evolving, many tags have diminished in significance. Here's the current state:

- **Still Relevant**: Meta Description, Viewport, Charset, Author, and Canonical
- **Limited Effect**: Keywords, Refresh, and Robots
- **Specialized Fields**: Open Graph, Twitter Cards are necessary for tailored content on social platforms

To maintain a robust online presence, focus on high-quality content, user experience, and technical soundness, and don't solely rely on meta tags.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name
</details>

<details>
<summary>How do you link a CSS file to an HTML document?</summary>

### How do you link a CSS file to an HTML document?

**Definition:**

**Linking** a CSS file to an HTML document is a fundamental step for styling. This is generally done by indicating the CSS file's path in the `head` section of the HTML file using `<link>` tags.

**How it works:**

#### HTML Link Tag: `<link>`

HTML uses the `<link>` tag to **integrate external resources** such as CSS files.

##### Syntax

```html
<link rel="stylesheet" href="path/to/style.css">
```

- **rel**: Specifies the type of relationship between the current document and the linked file. For CSS, it should be set to "stylesheet".

- **href**: Points to the location of the external CSS file. This can be via an **absolute URL** (i.e., `http://...`) or a **relative path** to the HTML file.

- **type**: Supplied for legacy purposes but is not required given the file is a CSS file.

##### Code Example: Using the Link Tag

Here is the HTML code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="path/to/style.css">
</head>
<body>
    <!-- Body content -->
</body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link
</details>

<details>
<summary>How do you link a JavaScript file to an HTML document?</summary>

### How do you link a JavaScript file to an HTML document?

**Definition:**

To link a JavaScript file to an HTML document, you need to use the `<script>` HTML tag. There are two primary ways to do this:

1. **External Script File**: Link a separate JavaScript file to your HTML document.
2. **Inline Script**: Embed JavaScript code directly within your HTML file.

**How it works:**

#### External Script File

To use an external JavaScript file, follow these steps:

1. **Create the JavaScript File**: Save your JavaScript code in a separate file with a `.js` extension. For example, `script.js`.

2. **Link the JavaScript File to your HTML Document**: Add the following code within the `<head>` or at the end of the `<body>` section of your HTML file.

    ```html
    <script src="path-to-your-js-file.js"></script>
    ```

    Replace `path-to-your-js-file.js` with the actual path to your JavaScript file.

##### Best Practices

- **Placement**: It's good practice to place your `<script>` tags at the end of the `<body>` section, just before the closing `</body>` tag. This ensures that the HTML content loads first, which can improve the website's initial rendering speed.

- **Syntax**: The HTML5 specification does not require a closing tag for the `<script>` element.

#### Inline Script

You can also include JavaScript directly within your HTML file. This is called an "inline script." To do this, encase your JavaScript code within `<script>` tags, like this:

  ```html
  <script>
      // Your JavaScript code goes here
  </script>
  ```

##### Best Practices

- **Content Separation**: For better code organization, it's often better to keep your JavaScript in a separate file, especially for larger applications.
  
- **Caching**: When using an external JavaScript file, the browser caches the script, which can speed up your site on subsequent visits. However, if the script changes often, this caching can be a problem.

- **Maintainability and Reusability**: Utilizing an external JavaScript file allows for better code management, reusability, and ease of making updates or fixes across multiple HTML files.

#### Example HTML File

Here is the code:

#### Implementation: HTML File

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="path-to-your-js-file.js"></script>
</head>
<body>
    <!-- Your content here -->
    <script>
        // Inline JavaScript code here.
    </script>
</body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script
</details>

<details>
<summary>How do you add a comment in HTML and why would you use them?</summary>

### How do you add a comment in HTML and why would you use them?

**Definition:**

To add a **comment** in HTML, wrap it between `<!--` and `-->`.

For example:

```html
<!-- This is a comment -->
<p>Hello, World!</p>
```

**How it works:**

#### Role of Comments in Development

Comments ensure clear code comprehension and can be used for:

- **Instructions**: Guiding developers on next steps.
- **Documentation**: Articulating intricate code segments.
- **Debugging**: Temporarily removing portions for bug testing.
- **Reminders**: Highlighting sections for later revision.

#### Best Practices for Using Comments

- **Purposeful Clarity**: Comments must explain what the code does, not how. Code and inline comments should clarify how the code works.
- **Relevance**: Avoid stating the obvious and focus on unique or complex components.
- **Conciseness**: Keep comments brief to reduce visual clutter.
- **Regular Maintenance**: Update or remove outdated comments to maintain accuracy.

#### When are Comments Unnecessary?

- **Trivial Cases**: Comments like "<div> wrapper" or "<p> tag" denote the obvious.
- **Self-Explanatory Code**: Writing **self-descriptive** code eliminates the need for specific comments.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Comments
</details>

<details>
<summary>How do you serve your page in multiple languages?</summary>

### How do you serve your page in multiple languages?

**Definition:**

Let's discuss the **best practices** for serving web pages in multiple languages and the corresponding HTML5 tag, `<html lang="en">`.

**How it works:**

#### Language Tag

For serving content in multiple languages and optimizing accessibility and search engine performance, you should use the `lang` attribute on the `<html>` tag. This is considered a **best practice**, even if the page is only in English.

```html
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Website</title>
  </head>
  <body>
    <!-- Page content here -->
  </body>
</html>
```

#### Common Language Codes

Most languages follow the two-letter ISO 639-1 code, such as "en" for English or "es" for Spanish. Some languages also use an extended ISO 639-2 or 639-3 code, which might require three to four letters, like "por" for Portuguese. 

For dialects or region-specific content, you can use a hyphen, followed by an ISO 3166-1 alpha-2 country code. For instance:

- "en-GB" for British English
- "es-ES" for Spanish as spoken in Spain
- "pt-BR" for Brazilian Portuguese
- "pt-PT" for European Portuguese

Understand that while the `lang` attribute assists in accessibility, user agents may not always recognize or act upon these subtags.

#### SEO Considerations

Serving content in multiple languages comes with SEO responsibilities. One common practice is to assign a *language-specific* URL for each version of your content. In addition to this, utilize human-readable URLs to effectively comminicate the language and the content topic/design.

For instance, use:
- `example.com/en-US/about` for pages in American English.
- `example.com/es-MX/sobre` for those in Mexican Spanish.

#### AI-Clearance Required

This technique requires further validation and clearance upon implementation as a lot is dependent on SEO constraints and localized content.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang
</details>

<details>
<summary>What are data-* attributes and when should they be used?</summary>

### What are data-* attributes and when should they be used?

**Definition:**

**Data attributes** in HTML5, often referred to as **`data-*` attributes**, help embed custom data within HTML elements. This presents a powerful tool for web developers, facilitating streamlined **JavaScript** and **CSS** operations.

**How it works:**

#### Core Benefits

- **Accessibility**: Data attributes are easily accessible through the `dataset` API in JavaScript.

- **Ignoring formatting tactics**: In places where content served by backend, cannot assume the content to always be JSON encoded, shortened, or have odd formatting.

- **Data Isolation**: For better maintenance of web documents. Data attributes have clear, defined roles within HTML.

#### Code Example: Using Data Attributes

Here is the HTML & JavaScript:

```html
<div id="user" data-name="John Doe" data-age="25"></div>

<script>
  const userDiv = document.getElementById('user');
  console.log(userDiv.dataset.name);  // Output: "John Doe"
  console.log(userDiv.dataset.age);   // Output: "25"
</script>
```

#### Appropriate Use-Cases

- **Custom Content for DOM Elements**: For attaching extra information or configuration settings exclusively relevant to an HTML element.
  
  Example: A `div` may have a `data-show-tooltip` attribute set to `true` to indicate it should display a tooltip.

- **Interactivity Configuration**: When working with user-made widgets, data attributes can specify how they behave in a more structured, intended manner. Useful in contexts where individual DIV or section blocks have interactivity toggles, or categories.

- **E-Commerce & Web Products**: To store product-specific IDs or additional details as they pertain to the DOM representation of a product in a catalog.

- **Styling Signifiers**: You can leverage data attributes in CSS for different types of styling like category colors, hover effects, or even in JavaScript-based CSS declarations.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Use_data_attributes
</details>

### Text Formatting in HTML

<details>
<summary>What is the difference between b and strong tags?</summary>

### What is the difference between b and strong tags?

**Definition:**

The `<b>` and `<strong>` tags are **both used for text emphasis** in HTML, but they have different semantic meanings.

**How it works:**

#### Bold vs. Strong

- The purpose of the `<b>` tag is to make the text **bold**, mainly for visual styling.
- The `<strong>` tag, on the other hand, **semantically emphasizes** the text, indicating its importance.

#### Semantic Importance

The use of semantic tags like `<strong>` is beneficial for components like screen readers, browsers, and search engines, which can provide better user experience or understanding of content with proper emphasis.

#### Code Example: B vs. Strong

In the HTML, the content "Caution" is visually bold and the content "Urgent Notice!" is both visually bold and semantically strong.

```html
<p>
  <b>Caution</b>: This action cannot be undone.
  <br>
  <strong>Urgent Notice!</strong> Please save your work before proceeding.
</p>
```

#### General Best Practice

- **Visual Styling** is usually left to CSS. `<b>` should be used with caution, if at all, as it becomes redundant in many scenarios due to CSS's wide adoption.
- **Semantic Tags** like `<strong>` provide context, clarity, and accessibility to the content.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/strong
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/b
</details>

<details>
<summary>When would you use em over i, and vice versa?</summary>

### When would you use em over i, and vice versa?

**Definition:**

Let's see the difference between 'em' and 'i' HTML tags.

**How it works:**

#### When to Use 'em'

The 'em' tag italicizes the text by default and should be reserved for occasions when emphasis is needed.

One potential usage could be for interactive instructions:

```html
<p><strong>Press</strong> <em>Enter</em> to submit.</p>
```

#### When to Use 'i'

The 'i' tag, or **italics** tag, is often avoided for text styling. Instead, consider semantic HTML, CSS, or more explicit HTML options like `<em>` for emphasis, when possible.

Here's an example of `<em>` combined with CSS for an additional bit of fluorescence. 

```html
<p>His <em style="background-color: yellow; color: red;">anger</em> was palpable.</p>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/em
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/i
</details>

<details>
<summary>What is the purpose of small, s, and mark tags?</summary>

### What is the purpose of small, s, and mark tags?

**Definition:**

The `small`, `s`, and `mark` HTML5 tags are used to **alter the structure and presentation** of text content.

**How it works:**

#### `<small>`

The `<small>` tag indicates that the enclosed text is of lesser importance, typically used for **fine print**, legal disclaimers, copyright notices, etc.

Here are examples:

##### Use Case

```html
<footer>
  <small>&copy; 2022 Company Name</small>
</footer>
```

#### `<s>`

The `<s>` tag, which stands for "strike," is a non-semantic, obscure tag that is often replaced with a more meaningful tag, such as `<del>` for "deleted" content. However, it still visually **strikes out** its content.

##### Use Case

```html
<p>Your discount code is: <s>EXPIRED123</s></p>
```

##### Visual Representation

Your discount code is: ~~EXPIRED123~~

#### `<mark>`

The `<mark>` tag is used to **highlight** or **set apart** text without specifying any additional semantic information.

##### Use Case

```html
<p>Important: Please <mark>schedule your appointment</mark> at least 48 hours in advance.</p>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/small
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/s
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/mark
</details>

<details>
<summary>What are semantic HTML tags and why are they important?</summary>

### What are semantic HTML tags and why are they important?

**Definition:**

**Semantic HTML tags** provide both structure and meaning to web content. They allow crawlers, browsers, and even assistive technologies to understand content better and present it more effectively. This approach improves accessibility and search engine optimization, making pages easier to maintain and understand.

**How it works:**

#### Benefits of Semantic Tags

- **SEO and Accessibility**: Employing semantic tags improves your page's search engine ranking and ensures it's accessible to all users, including those with disabilities.

- **Consistent Structure**: Semantic tags establish a cohesive layout, vital for large websites or platforms.

- **Relevance to Bots and Crawlers**: Search engine algorithms dissect web pages more accurately when content is correctly labeled.

- **Content Division**: Segregating content by their meaning makes the document more understandable and maintainable.

#### Common Semantic Tags

- `<p>`: A paragraph.
- `<h1> - <h6>`: Headings, with 1 (highest) to 6 (lowest) levels.
- `<ul> / <ol>`: Unordered or ordered list.
- `<li>`: List item inside a list.
- `<a>`: Anchor, used for links.
- `<img>`: An image.
- `<figure> / <figcaption>`: For a figure such as an image, with accompanying caption.

#### Necessary vs. Optional Tags

While essential tags like `<header>` and `<footer>` indicate crucial sections, many are optional based on the website's nature or the page's segregation needs. For example, a blog may require the `<article>` tag, while a retail site might not.

In modern web development, the clear distinction offered by semantic tags is invaluable for quick comprehension and maintenance, yielding superior results for both users and developers.

#### Code Example: Before vs. After Semantic HTML

Consider the before and after examples to see the impact of semantic tags.

##### Before Semantic HTML

```html
<div class="nav">
    <div class="logo">
        <a href="#">Logo</a>
    </div>
    <div class="nav-links">
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Contact</a>
    </div>
</div>
<div class="main-wrapper">
    <div class="image">
        <img src="image.jpg" alt="A beautiful landscape">
    </div>
    <div class="content">
        <h3>Welcome</h3>
        <p>Some welcome text here.</p>
    </div>
</div>
<div class="footer">
    <p>© 2022 Company Name</p>
</div>
```

##### After Implementing Semantic Tags

```html
<header>
    <div class="logo">
        <a href="#">Logo</a>
    </div>
    <nav>
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Contact</a>
    </nav>
</header>

<main>
    <figure>
        <img src="image.jpg" alt="A beautiful landscape">
        <figcaption>A beautiful landscape</figcaption>
    </figure>
    <section>
        <h1>Welcome</h1>
        <p>Some welcome text here.</p>
    </section>
</main>

<footer>
    <p>© 2022 Company Name</p>
</footer>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
</details>

<details>
<summary>How do you create a paragraph or a line break in HTML?</summary>

### How do you create a paragraph or a line break in HTML?

**Definition:**

In HTML, to create a **paragraph**, use `<p>...</p>` tags, and to insert a **line break**, use `<br>` tag.

**How it works:**

#### Paragraphs in HTML

Traditional paragraph formatting in HTML is achieved using the `<p>` tag. The browser's default styling generally adds **spacing** to the top and bottom of each `<p>` element, creating distinct paragraphs.

##### Syntax

```html
<p>
    This is an example of a paragraph. The text enclosed within the &lt;p&gt; tags represents a single paragraph.
</p>
```

##### Visual Representation

<p>
    This is an example of a paragraph. The text enclosed within the &lt;p&gt; tags represents a single paragraph.
</p>

#### Line Breaks in HTML

To insert a simple line break in an HTML document, use the `<br>` tag. This tag doesn't require a closing equivalent.

##### Syntax

```html
First Line<br>Second Line
```

##### Visual Representation

First Line<br> Second Line (This text doesn't render the line break; it's just to show the raw HTML.)


#### Multi-line Text Elements

In HTML, the `<textarea>` tag allows the input of several lines of text. Nonetheless, it does not auto-format for paragraphs. It **wraps text** instead, and vertical scroll bars might be enabled, based on the template and content.

##### Syntax

```html
<textarea rows="4" cols="50">
This is a multi-line text area.
It doesn't automatically create separate paragraphs.
Text wraps based on dimensions supplied.
</textarea>
```

##### Visual Representation

```html
<textarea rows="4" cols="50">
This is a multi-line text area.
It doesn't automatically create separate paragraphs.
Text wraps based on dimensions supplied.
</textarea>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/html5-interview-questions/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/br
</details>

### Links and Navigation

<details>
<summary>How do you create a hyperlink in HTML?</summary>

### How do you create a hyperlink in HTML?

**Definition:**

A hyperlink is an `<a>` element whose `href` is the destination URL. The content inside the element is the link text.

**How it works:**

#### The anchor

- `href` can address a page, a file, `mailto:`, `tel:`, `sms:`, or a fragment in the same page.
- With `href` present, Enter activates the focused link. Without `href`, the element is not a link.
- The text should name the destination. “Click here” is weak link text.

**When to use:**

- Navigation to a real URL. An on-page action that does not go anywhere belongs on a `<button>`.

**Examples:**

```html
<p>You can reach Michael at:</p>
<ul>
  <li><a href="https://example.com">Website</a></li>
  <li><a href="mailto:m.bluth@example.com">Email</a></li>
  <li><a href="tel:+123456789">Phone</a></li>
</ul>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>What is the difference between relative and absolute URLs?</summary>

### What is the difference between relative and absolute URLs?

**Definition:**

An absolute URL contains the parts needed to locate a resource on its own. A relative URL leaves some parts out, and the browser fills them from the current document’s URL.

**How it works:**

#### Absolute URL

- Includes the scheme and the rest of the address, for example `https://developer.mozilla.org/en-US/docs/Web/HTML`.
- The address bar has no document to borrow from, so it needs an absolute URL.

#### Relative URL

- Scheme-relative (`//example.com`) keeps the current page’s scheme.
- A path that starts with `/` is resolved from the origin root, with the current scheme and host.
- `p` and `./p` resolve against the current directory. `../p` goes up one level.
- `#section` keeps the current document and only changes the fragment. The fragment is not sent to the server.

**When to use:**

- An absolute URL for another site.
- A relative URL for a resource on the same site, so the link still works if the host changes.

**Examples:**

```html
<a href="https://example.com">Absolute URL</a>
<a href="//example.com">Scheme-relative URL</a>
<a href="/en-US/docs/Web/HTML">Origin-relative URL</a>
<a href="./p">Directory-relative URL</a>
<a href="../p">Parent-directory-relative URL</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>How can you open a link in a new tab?</summary>

### How can you open a link in a new tab?

**Definition:**

Set `target="_blank"` on the `<a>`. The browser usually opens that URL in a new tab.

**How it works:**

#### `target` keywords

- `_self` is the default: the current browsing context.
- `_blank` is usually a new tab. The user can configure a new window instead.
- `_parent` is the parent context, or `_self` when there is no parent.
- `_top` is the topmost ancestor context.

#### What the new page can see

- `target="_blank"` already acts like `rel="noopener"`: the new context’s `window.opener` is `null`.
- Say in the link text that it opens a new tab, so the change of context is not a surprise.

**When to use:**

- When leaving the page would lose the user’s place, such as a reference on another site. Most links should stay in the same tab.

**Examples:**

```html
<a target="_blank" href="https://www.wikipedia.org">
  Wikipedia (opens in new tab)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/rel/noopener
</details>

<details>
<summary>How do you create an anchor to jump to a specific part of the page?</summary>

### How do you create an anchor to jump to a specific part of the page?

**Definition:**

Put an `id` on the target element, then set the link’s `href` to `#` plus that id.

**How it works:**

#### Fragment identifier

- The browser scrolls to the element whose `id` matches the fragment.
- The fragment is not sent to the server with the request.
- `href="#top"` or an empty fragment `href="#"` jumps to the top of the current page.
- The old `name` attribute on `<a>` is deprecated. Use the global `id` attribute.

**When to use:**

- A table of contents, a skip link, or any jump inside the same document.

**Examples:**

```html
<p><a href="#Section_further_down">Jump to the heading below</a></p>

<h2 id="Section_further_down">Section further down</h2>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL
</details>

<details>
<summary>How do you link to a downloadable file in HTML?</summary>

### How do you link to a downloadable file in HTML?

**Definition:**

Point an `<a>` at the file with `href`, and add `download` so the browser treats that URL as a download.

**How it works:**

#### The `download` attribute

- With no value, the browser suggests a name from the `Content-Disposition` header, the last segment of the URL path, or the media type.
- A value such as `download="report.pdf"` suggests that filename. `/` and `\` are turned into underscores.
- It applies to same-origin URLs, and to `blob:` and `data:` URLs. A cross-origin HTTP URL does not honor it.
- If `Content-Disposition` includes a filename, that name takes priority over the attribute value.
- The link text should say that the destination is a file.

**When to use:**

- A file the user should save, served from the same origin as the page.

**Examples:**

```html
<a href="2017-annual-report.ppt" download="2017-annual-report.ppt">
  2017 annual report (PowerPoint file)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

### Images and Objects

<details>
<summary>How do you embed images in an HTML page?</summary>

### How do you embed images in an HTML page?

**Definition:**

Use the `<img>` element. `src` is the image URL, and `alt` is the replacement text.

**How it works:**

#### The image element

- `<img>` has no end tag. It embeds the resource named by `src`.
- `width` and `height` are integers in CSS pixels, with no unit. Together they let the browser reserve the aspect ratio before the file arrives.
- `loading="lazy"` defers the fetch until the image is near the viewport. `eager` is the default and loads immediately.
- For a different crop or format per condition, wrap sources in `<picture>` and keep one `<img>` as the fallback.

**When to use:**

- A photo, diagram, or icon that belongs in the document. A vector you want in the DOM can be inline SVG instead.

**Examples:**

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>What is the importance of the alt attribute for images?</summary>

### What is the importance of the alt attribute for images?

**Definition:**

`alt` is the text that replaces the image when the browser does not show it, and the name assistive technology uses for that image.

**How it works:**

#### When the image is not shown

- Non-visual browsers, users who turn images off, and a failed or unsupported file can all fall back to `alt`.
- The same text is used when the image is copied as text or saved from a link into a bookmark.
- `alt=""` means the image is decoration or a tracking pixel. Non-visual browsers may skip it, and a visual browser hides the broken-image icon if the file fails.
- Omitting `alt` is different from an empty value: the browser then has no replacement text.

**When to use:**

- A content image gets a short description of what it shows.
- A decorative image gets `alt=""`.
- Every `<area href>` in an image map needs its own `alt` naming that region’s destination.

**Examples:**

```html
<img
  src="/shared-assets/images/examples/grapefruit-slice.jpg"
  alt="Grapefruit slice atop a pile of other slices" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>What image formats are supported by web browsers?</summary>

### What image formats are supported by web browsers?

**Definition:**

Browsers commonly decode GIF, JPEG, PNG, APNG, SVG, WebP, and AVIF. Support for other types is narrower.

**How it works:**

#### Widely used types

- **GIF** (`image/gif`): indexed color and simple animation.
- **JPEG** (`image/jpeg`): lossy still photos. No alpha channel.
- **PNG** (`image/png`): lossless still images, including transparency.
- **APNG** (`image/apng`): lossless animation. Chrome, Edge, Firefox, Opera, and Safari.
- **SVG** (`image/svg+xml`): vectors that stay sharp at any size.
- **WebP** (`image/webp`): still and animated images, with better compression than JPEG or PNG. Chrome, Edge, Firefox, Opera, and Safari.
- **AVIF** (`image/avif`): still and animated images, usually smaller than WebP. Same current major browsers. Provide a fallback in `<picture>` because older browsers do not decode it.

#### Narrower support

- **JPEG XL** (`image/jxl`) is not universal: Safari supports it, Chrome only behind a flag, Firefox in Nightly. Pair it with a fallback.
- **BMP** and **ICO** decode in major browsers, but MDN says not to use them for page content.
- **TIFF** is supported in Safari.

**When to use:**

- JPEG or AVIF/WebP for photographs. PNG or SVG when you need sharp edges or transparency. AVIF and JPEG XL need a `<picture>` fallback.

**Examples:**

```html
<picture>
  <source srcset="photo.avif" type="image/avif" />
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="photo" />
</picture>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
</details>

<details>
<summary>How do you create image maps in HTML?</summary>

### How do you create image maps in HTML?

**Definition:**

A client-side image map is a `<map>` of `<area>` links, referenced from an `<img>` with `usemap`.

**How it works:**

#### Client-side map

- `<map name="primary">` must have a non-empty `name` with no spaces, unique in the document. If `id` is also set, it must match `name`.
- Each `<area>` is a shape: `rect`, `circle`, or `poly`, with `coords`, plus `href` and `alt`.
- The image points at the map with `usemap="#primary"`. The value is a hash plus the map’s `name`.
- `alt` on each linked area names that region’s destination.

#### Server-side map

- `ismap` on `<img>` sends the click coordinates to the server. It is valid only when that image is inside an `<a>` that has an `href`, so there is a fallback destination.

**When to use:**

- Several links on one picture, such as a diagram. Separate controls are easier to use when the regions are not really parts of one image.

**Examples:**

```html
<map name="primary">
  <area
    shape="circle"
    coords="75,75,75"
    href="https://developer.mozilla.org/docs/Web/JavaScript"
    alt="JavaScript" />
  <area
    shape="circle"
    coords="275,75,75"
    href="https://developer.mozilla.org/docs/Web/CSS"
    alt="CSS" />
</map>
<img usemap="#primary" src="parrots.jpg" alt="350 x 150 picture of two parrots" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/map
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
</details>

<details>
<summary>What is the difference between svg and canvas elements?</summary>

### What is the difference between svg and canvas elements?

**Definition:**

SVG is XML markup for vector shapes that stay in the DOM. `<canvas>` is a bitmap you draw into with the canvas scripting API or WebGL.

**How it works:**

#### SVG

- Shapes are elements (`rect`, `circle`, `text`, and others). Later elements paint on top of earlier ones.
- The graphic scales without becoming blocky, and its text can be updated without a bitmap editor.
- You can embed the markup in HTML, or reference a file with `<img>`, `<object>`, or `<iframe>`.
- Inline SVG binds the namespace `xmlns="http://www.w3.org/2000/svg"`.

#### Canvas

- The element is an empty drawing surface. `width` defaults to 300 and `height` to 150, in CSS pixels.
- Set those attributes on the element. Sizing only with CSS scales the bitmap and can distort it.
- Content inside `<canvas>` is the fallback for browsers that cannot render it and for when JavaScript is off.
- Drawn objects are not semantic HTML. Accessibility tools do not see them as separate elements.

**When to use:**

- SVG for icons, diagrams, and UI that must stay sharp and selectable.
- Canvas for scripted or WebGL scenes, such as a paint surface or a game frame, with fallback text inside the element.

**Examples:**

```html
<svg width="300" height="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="150" cy="100" r="80" fill="green" />
</svg>

<canvas width="120" height="120">
  An alternative text describing what your canvas displays.
</canvas>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/SVG
- https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/canvas
</details>

### Lists

<details>
<summary>What are the different types of lists available in HTML?</summary>

### What are the different types of lists available in HTML?

**Definition:**

HTML has three list elements: `<ul>` for an unordered list, `<ol>` for an ordered list, and `<dl>` for a description list.

**How it works:**

#### Which list

- `<ul>` groups items whose order does not change the meaning. Items are `<li>`, usually shown with a bullet.
- `<ol>` groups items whose order matters: recipe steps, directions, or a ranking. Items are `<li>`, usually numbered.
- `<dl>` groups terms (`<dt>`) and descriptions (`<dd>`): a glossary or key-value metadata. One term can have several descriptions, and several terms can share one description.
- To choose between `<ul>` and `<ol>`, swap the items. If the meaning changes, use `<ol>`.

**When to use:**

- Bullets for a set, numbers for a sequence, and a description list for names paired with definitions. Do not use `<dl>` only to indent text.

**Examples:**

```html
<ul>
  <li>Milk</li>
  <li>Cheese</li>
</ul>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dl
</details>

<details>
<summary>How do you create ordered, unordered, and description lists in HTML?</summary>

### How do you create ordered, unordered, and description lists in HTML?

**Definition:**

Wrap each item in `<li>` inside `<ul>` or `<ol>`. In a `<dl>`, put one or more `<dt>` terms, then one or more `<dd>` descriptions.

**How it works:**

#### Markup

- `<ul>`, `<ol>`, `<dl>`, `<dt>`, and `<dd>` need both tags. An `<li>` end tag may be omitted when another `<li>` follows immediately, or when nothing else remains in the parent.
- A description list may wrap each name-value group in a `<div>`.
- `<dd>` is the description, not a second list item. Do not put `<li>` inside `<dl>`.

**When to use:**

- The list element matches the meaning from the previous question. CSS changes the marker; it does not change which element you pick.

**Examples:**

```html
<ol>
  <li>Mix flour, baking powder, sugar, and salt.</li>
  <li>In another bowl, mix eggs, milk, and oil.</li>
</ol>

<dl>
  <dt>Beast of Bodmin</dt>
  <dd>A large feline inhabiting Bodmin Moor.</dd>
</dl>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dl
</details>

<details>
<summary>Can lists be nested in HTML? If so, how?</summary>

### Can lists be nested in HTML? If so, how?

**Definition:**

Yes. Put the inner `<ul>` or `<ol>` inside an `<li>`, and close that `<li>` after the inner list.

**How it works:**

#### Where the inner list goes

- `<ul>` and `<ol>` may nest as deeply as you need, and they may alternate.
- The nested list is a child of the `<li>`, not a sibling that sits between two items.
- A nested list is allowed because it sits inside the `<li>`, which is the item element those lists contain.

**When to use:**

- A sub-step or a group that belongs to one item, such as ingredients under one recipe step.

**Examples:**

```html
<ol>
  <li>first item</li>
  <li>
    second item
    <ul>
      <li>second item first subitem</li>
      <li>second item second subitem</li>
    </ul>
  </li>
  <li>third item</li>
</ol>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
</details>

<details>
<summary>What attributes can you use with lists to modify their appearance or behavior?</summary>

### What attributes can you use with lists to modify their appearance or behavior?

**Definition:**

Numbering behavior lives on `<ol>` and `<li>`. Bullet shape is a CSS concern: the HTML `type` on `<ul>` and `compact` are deprecated.

**How it works:**

#### Ordered lists

- `start` is an Arabic integer where counting begins, even when the marker is a letter or a Roman numeral. `start="4"` begins at “d” or “iv”.
- `reversed` numbers from high to low.
- `type` is `1` (default), `a`, `A`, `i`, or `I`. Use it when the marker itself is part of the meaning, as in a legal list. Otherwise set `list-style-type` in CSS.
- `value` on an `<li>` sets that item’s number. Following items continue from there.

#### Deprecated

- `type` on `<ul>` (`disc`, `circle`, `square`) is deprecated. Use CSS `list-style-type`.
- `compact` on `<ul>`, `<ol>`, and `<dl>` is deprecated. Use CSS.

**When to use:**

- `start`, `reversed`, and `value` when the sequence is real data, such as places 4, 5, and 6. Leave visual bullets to CSS.

**Examples:**

```html
<ol start="4" reversed>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
  <li>Wash and chop the salad.</li>
</ol>

<ol type="I">
  <li value="3">third item</li>
  <li>fourth item</li>
</ol>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/li
</details>

### Forms and Input Elements

<details>
<summary>What are HTML forms and how do you create one?</summary>

### What are HTML forms and how do you create one?

**Definition:**

A `<form>` is a section of controls that submit information. `action` is the URL that processes the submission, and `method` is the HTTP method.

**How it works:**

#### Submission

- `method="get"` (the default) appends the fields to the `action` URL. Use it when the form has no side effects.
- `method="post"` sends the fields as the request body.
- `method="dialog"` closes a surrounding `<dialog>` and fires `submit` without sending data.
- `enctype` matters for `post`: `application/x-www-form-urlencoded` is the default, and `multipart/form-data` is the one to use when the form has a file input.
- `novalidate` skips constraint validation on submit. `target` chooses the browsing context for the response, with the same keywords as a link (`_self`, `_blank`, `_parent`, `_top`).
- Each control that should be submitted needs a `name`.

**When to use:**

- Any set of controls whose values go to a server, or a form inside a dialog that should only close the dialog.

**Examples:**

```html
<form action="/my-handling-form-page" method="post">
  <label for="name">Name:</label>
  <input type="text" id="name" name="user_name" />
  <button>Save</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form
</details>

<details>
<summary>Describe the different form input types in HTML5.</summary>

### Describe the different form input types in HTML5.

**Definition:**

The `type` attribute on `<input>` chooses the control. If you omit it, the type is `text`.

**How it works:**

#### Text

- `text` is a single line. Line breaks are stripped.
- `search`, `tel`, `url`, and `email` are single-line fields with a matching keyboard and, for `url` and `email`, validation.
- `password` obscures the value.

#### Choices and buttons

- `checkbox` toggles one value. `radio` buttons that share a `name` allow one value from the group.
- `button` has no default behavior. `submit` sends the form. `reset` restores defaults and is not recommended.
- `image` is a graphical submit button. It needs `src` and `alt`.

#### Values with a widget

- `number` and `range` enter a number. `range` is for when the exact value does not matter; it defaults to the midpoint of `min` and `max`.
- `date`, `month`, `week`, `time`, and `datetime-local` enter a date or time with no time zone.
- `color` opens a color picker. `file` selects a file; `accept` hints at the types.
- `hidden` is submitted and not displayed.

#### Obsolete

- `datetime` is obsolete. Use `datetime-local`.

**When to use:**

- The type that matches the data, so the browser can offer the right keyboard and constraint checks. Other controls that are not `<input>` include `<textarea>`, `<select>`, and `<button>`.

**Examples:**

```html
<label for="mail">Email:</label>
<input type="email" id="mail" name="user_email" />

<input type="date" name="trip" />
<input type="file" name="avatar" accept="image/png, image/jpeg" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input
</details>

<details>
<summary>How do you make form inputs required?</summary>

### How do you make form inputs required?

**Definition:**

Add the Boolean `required` attribute. The form will not submit until that control has a value.

**How it works:**

#### Where it applies

- It works on `text`, `search`, `url`, `tel`, `email`, `password`, `date`, `month`, `week`, `time`, `datetime-local`, `number`, `checkbox`, `radio`, and `file`, and on `<select>` and `<textarea>`.
- It does not apply to `range` or `color` (they always have a default), to `hidden`, or to button types, including `image`.
- An empty required value fails constraint validation with `valueMissing` and matches `:invalid`.
- `required` has no effect on a control that is also `readonly`.
- In a radio group, `required` on any one button means one button in that `name` must be checked. Put it on every button in the group, or on none, so the markup stays obvious.
- For checkboxes that share a name, only the ones that carry `required` are required.
- `aria-required="true"` announces the requirement to a screen reader and does not block submission.

**When to use:**

- A field the server cannot accept empty. Also show that requirement in visible text. Color alone is not enough.

**Examples:**

```html
<form method="post">
  <label for="fname">First name (required):</label>
  <input id="fname" name="fname" type="text" required />
  <button>Save</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/required
</details>

<details>
<summary>What is the purpose of the label element in forms?</summary>

### What is the purpose of the label element in forms?

**Definition:**

`<label>` is the caption for a form control. Activating the label focuses that control, and assistive technology reads the caption with the control.

**How it works:**

#### Two associations

- Explicit: `for` on the label matches the control’s `id`. The first labelable element with that `id` is the labeled control.
- Implicit: nest the control inside the label. `for` and `id` are then unnecessary.
- Explicit association is the more compatible of the two. You can nest and set `for`/`id` together, as long as `for` points at the nested control.
- A click or tap on the label focuses the control and enlarges the hit target.
- Labelable elements include `<input>` (not `type="hidden"`), `<button>`, `<select>`, `<textarea>`, `<output>`, `<progress>`, and `<meter>`.
- Do not put another link, button, or heading inside the label. A `type="button"` input with a `value`, and a `<button>`, do not need a label.

**When to use:**

- Every control that a person must understand, except buttons whose own text is already the name.

**Examples:**

```html
<label for="username">Click me to focus on the input field</label>
<input type="text" id="username" name="username" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/label
</details>

<details>
<summary>How do you group form inputs and why would you do this?</summary>

### How do you group form inputs and why would you do this?

**Definition:**

Wrap related controls in `<fieldset>` and name the group with `<legend>`. The group is announced as one unit.

**How it works:**

#### Fieldset

- The first `<legend>` inside the fieldset is its caption.
- `disabled` on the fieldset disables the descendant controls: they are not editable and are not submitted. Form controls inside a nested `<legend>` are not disabled by that attribute.
- `name` names the group. `form` can point at a form’s `id` when the fieldset is outside that form; controls still need their own `form` attribute to join that form.
- The implicit ARIA role is `group`. A radio group may use `role="radiogroup"`.

**When to use:**

- A set of radios or checkboxes that share one question, or any controls that belong to one caption. A title for a whole form section can also be a heading, but the fieldset caption is `<legend>`.

**Examples:**

```html
<form method="post">
  <fieldset>
    <legend>Choose your favorite monster</legend>
    <input type="radio" id="kraken" name="monster" value="K" />
    <label for="kraken">Kraken</label>
    <input type="radio" id="sasquatch" name="monster" value="S" />
    <label for="sasquatch">Sasquatch</label>
  </fieldset>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/fieldset
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form
</details>

### HTML5 Features

<details>
<summary>What is new in HTML5 compared to previous versions?</summary>

### What is new in HTML5 compared to previous versions?

**Definition:**

HTML5 followed HTML 4.01 and added elements and capabilities for structure, media, and forms. HTML is now the WHATWG living standard and has no version number.

**How it works:**

#### What changed

- A page uses `<!doctype html>`. That doctype selects the current HTML, not a numbered HTML5 snapshot.
- Sectioning elements name regions that used to be anonymous `<div>`s: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, and `<footer>`.
- `<figure>` and `<figcaption>` group a self-contained figure with its caption.
- `<video>` and `<audio>` embed a player. `<canvas>` is a bitmap drawing surface.
- Form controls gained types such as `email`, `url`, `date`, and `number`, plus the `required` attribute.
- Until 2019 the W3C also published a numbered HTML5. On 28 May 2019 the W3C named the WHATWG living standard the sole HTML.

**When to use:**

- The elements above when they match the content. A box that is only a styling hook stays a `<div>`.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>My page title</title>
  </head>
  <body>
    <header><h1>Header</h1></header>
    <main><p>Page content.</p></main>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/HTML5
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents
</details>

<details>
<summary>How do you create a section on a webpage using HTML5 semantic elements?</summary>

### How do you create a section on a webpage using HTML5 semantic elements?

**Definition:**

Use `<section>` for a generic standalone region that has no more specific element. Give it a heading.

**How it works:**

#### Choosing the region

- `<header>` is introductory content. Inside `<body>` it is the page header; inside `<article>` or `<section>` it heads that region.
- `<nav>` is a major block of navigation links.
- `<main>` is the content unique to this page. Use it once, directly in `<body>`.
- `<article>` is a self-contained piece, such as a post. `<section>` groups one theme or piece of functionality inside or beside it.
- `<aside>` is indirectly related content, such as related links. `<footer>` is end matter.
- A `<section>` should start with a heading (`<h1>`–`<h6>`) except for some application UI, such as a button bar.
- If you only need a styling wrapper, use `<div>`.

**When to use:**

- A headed block of one theme: a chapter, a set of headlines, or a map and its controls. A navigation menu is `<nav>`, not `<section>`.

**Examples:**

```html
<section>
  <h2>Introduction</h2>
  <p>
    This document provides a guide to help with the important task of choosing
    the correct Apple.
  </p>
</section>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/section
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents
</details>

<details>
<summary>What is the role of the article element in HTML5?</summary>

### What is the role of the article element in HTML5?

**Definition:**

`<article>` is a self-contained composition that could be distributed or reused on its own, such as a post, a news story, a comment, or a product card.

**How it works:**

#### Independence

- Identify each article with a heading (`<h1>`–`<h6>`).
- A nested `<article>` is related to the outer one. Comments on a post are articles inside the post’s article.
- Author information can go in a `<footer>`, but that footer does not apply to nested articles.
- The publication time goes on a `<time datetime>` element.
- `<article>` must not be a descendant of `<address>`.

**When to use:**

- A unit that still makes sense in a feed or on another page. A generic theme that is not independently reusable is a `<section>`.

**Examples:**

```html
<article>
  <h2>Jurassic Park</h2>
  <p>Dinos were great!</p>
  <footer>
    <p>Posted on <time datetime="2015-05-15 19:00">May 15</time> by Staff.</p>
  </footer>
</article>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/article
</details>

<details>
<summary>Can you explain the use of the nav and aside elements in HTML5?</summary>

### Can you explain the use of the nav and aside elements in HTML5?

**Definition:**

`<nav>` is a major block of navigation links. `<aside>` is content only indirectly related to the main content, often a sidebar or a call-out.

**How it works:**

#### Navigation

- Typical contents are a menu, a table of contents, or an index. Not every link on the page belongs in `<nav>`.
- A document may have several `<nav>` elements, for site navigation and for navigation inside the page. `aria-labelledby` can name each one.
- The implicit role is `navigation`. Screen readers can skip navigation-only content.

#### Aside

- Use it for a glossary note, an author bio, or related links.
- Do not mark parenthetical text that is part of the main sentence as `<aside>`.
- The implicit role is `complementary`, or `generic` when the aside has no accessible name and sits inside other sectioning content.
- `<aside>` must not be a descendant of `<address>`.

**When to use:**

- `<nav>` for the primary way around the site or the document. `<aside>` for material a reader can skip without losing the main point.

**Examples:**

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<aside>
  <p>The movie earned $87 million during its initial release.</p>
</aside>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/aside
</details>

<details>
<summary>How do you use the figure and figcaption elements?</summary>

### How do you use the figure and figcaption elements?

**Definition:**

`<figure>` is self-contained content, such as an image, a diagram, or a code sample, that you could move without breaking the main flow. `<figcaption>` is its caption.

**How it works:**

#### Caption

- Put `<figcaption>` as the first or last child of the figure. The first one found is the caption.
- The caption is the accessible name of the figure.
- The figure can hold an `<img>`, a `<pre>`, a `<blockquote>`, or other flow content.
- `alt` on the image still describes the image. The caption names the figure; it does not replace `alt`.

**When to use:**

- Something the prose refers to as a unit and that could move to an appendix. An image that is just decoration does not need a figure.

**Examples:**

```html
<figure>
  <img
    src="/shared-assets/images/examples/elephant.jpg"
    alt="Elephant at sunset" />
  <figcaption>An elephant at sunset</figcaption>
</figure>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/figure
</details>

### Tables

<details>
<summary>How do you create a table in HTML?</summary>

### How do you create a table in HTML?

**Definition:**

A data table is a `<table>` of rows (`<tr>`). Header cells are `<th>`, and data cells are `<td>`.

**How it works:**

#### The pieces

- `<caption>`, as the first child of `<table>`, describes what the table is.
- `<thead>`, `<tbody>`, and `<tfoot>` group the header rows, the data rows, and the footer rows. If you omit `<tbody>`, the browser still inserts one.
- `scope` on a `<th>` says whether that header covers a `col`, a `row`, a `colgroup`, or a `rowgroup`.
- A table is for data with a relationship across rows and columns. Page layout does not belong in a table.

**When to use:**

- A grid of related facts, such as a schedule or a list of records. A heading plus paragraphs is enough when there is no row-and-column relationship.

**Examples:**

```html
<table>
  <caption>Alien football stars</caption>
  <thead>
    <tr>
      <th scope="col">Player</th>
      <th scope="col">Gloobles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">TR-7</th>
      <td>7</td>
    </tr>
  </tbody>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/table
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>What are thead, tbody, and tfoot in a table?</summary>

### What are thead, tbody, and tfoot in a table?

**Definition:**

They group a table’s rows into a header, a body, and a footer. Write them in that order: `<thead>`, then `<tbody>`, then `<tfoot>`.

**How it works:**

#### What each group is

- `<thead>` wraps the header rows, usually the column headings. If you use `<colgroup>`, the header comes after it.
- `<tbody>` wraps the data rows. A table always has one, even when you do not write the tags.
- `<tfoot>` wraps the footer, such as a totals row. It comes after `<tbody>`.
- These groups do not by themselves change what a screen reader announces. They give CSS and print layout a place to attach: a long table can repeat the header and footer on each printed page, and the body can scroll.

**When to use:**

- Any table with a real header row, and a footer when the last row summarizes the body.

**Examples:**

```html
<table>
  <thead>
    <tr>
      <th>Student ID</th>
      <th>Name</th>
      <th>Credits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3741255</td>
      <td>Jones, Martha</td>
      <td>240</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th colspan="2" scope="row">Average Credits</th>
      <td>240</td>
    </tr>
  </tfoot>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/tfoot
</details>

<details>
<summary>What is a colspan and rowspan?</summary>

### What is a colspan and rowspan?

**Definition:**

`colspan` is how many columns a cell covers. `rowspan` is how many rows it covers. Both default to `1`.

**How it works:**

#### Spanning

- The value is a non-negative integer. On `<th>`, a `colspan` above 1000 is clipped to 1000, and a `rowspan` above 65534 is clipped to 65534.
- `rowspan="0"` extends the header to the end of its table group (`thead`, `tbody`, or `tfoot`).
- Cells you do not span still occupy a 1×1 slot. The spanned cell must not overlap another cell.
- A header that spans should still say which cells it names, with `scope` or, in a more tangled header, `headers` pointing at other header `id`s.

**When to use:**

- A label that genuinely covers several columns or rows, such as “Membership Dates” over “Joined” and “Canceled”, or a totals label across the columns it summarizes.

**Examples:**

```html
<tr>
  <th scope="col" rowspan="2">Symbol</th>
  <th scope="col" rowspan="2">Code word</th>
  <th scope="col" colspan="2">Pronunciation</th>
</tr>
<tr>
  <th scope="col">IPA</th>
  <th scope="col">Respelling</th>
</tr>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>How do you make a table accessible?</summary>

### How do you make a table accessible?

**Definition:**

Give the table a `<caption>`, mark header cells with `<th>`, and set `scope` so a screen reader can tie each data cell to its headers.

**How it works:**

#### Programmatic headers

- Sighted readers match a cell to its headers by looking. A screen reader needs those same links in the markup.
- `scope="col"` on a column header and `scope="row"` on a row header are enough for a simple grid. Some assistive tools infer `scope`, but stating it is more reliable.
- For a header that itself needs other headers, give those `<th>` elements `id`s and list them in `headers`.
- `<caption>` is the description, placed immediately after the opening `<table>` tag. The old `summary` attribute is deprecated and is invisible to sighted readers.
- `<thead>`, `<tbody>`, and `<tfoot>` are mainly for styling and for repeating the header and footer in print. They are not a substitute for `<th>` and `scope`.

**When to use:**

- Every data table. A layout that is not tabular data should not be a table at all.

**Examples:**

```html
<table>
  <caption>Dinosaurs in the Jurassic period</caption>
  <tr>
    <th scope="col">Symbol</th>
    <th scope="col">Code word</th>
  </tr>
  <tr>
    <th scope="row">A</th>
    <td>Alfa</td>
  </tr>
</table>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/th
</details>

<details>
<summary>How can tables be made responsive?</summary>

### How can tables be made responsive?

**Definition:**

Keep the data in a real `<table>` and let CSS size it. HTML has no attribute that makes a table responsive.

**How it works:**

#### Layout in CSS

- `table-layout: fixed` sizes columns from the header widths instead of from the longest cell, which is more predictable.
- A percentage `width`, such as `90%`, lets the table follow the viewport and stay centered with `margin: 10px auto`, as in the MDN table-styling guide.
- A long table can keep its header and footer in place and scroll the `<tbody>`.
- Presentational attributes such as `width`, `height`, `align`, and `bgcolor` on `<th>` are deprecated. Use CSS.
- Do not rebuild the data as a stack of `<div>`s. The row and column relationships are what make the table understandable.

**When to use:**

- Whenever the same table is read on a narrow window and a wide one. Change the CSS when the content starts to feel cramped; do not drop the header cells.

**Examples:**

```html
<table>
  <caption>A summary of the UK's most famous punk bands</caption>
  <thead>
    <tr>
      <th scope="col">Band</th>
      <th scope="col">Year formed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Buzzcocks</th>
      <td>1976</td>
    </tr>
  </tbody>
</table>
```

```css
table {
  table-layout: fixed;
  width: 90%;
  margin: 10px auto;
  border-collapse: collapse;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Tables
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Table_accessibility
</details>

### Media in HTML

<details>
<summary>How do you add audio and video to an HTML document?</summary>

### How do you add audio and video to an HTML document?

**Definition:**

Use `<video>` for a video player and `<audio>` for sound. Point `src` at the file, or nest `<source>` elements and let the browser pick the first type it can play.

**How it works:**

#### The player

- `controls` shows the browser’s playback controls. Without it, `<audio>` has no visible UI.
- Content between the tags is the fallback for browsers that do not support the element. A download link belongs there.
- `type` on `<source>` is the MIME type, so the browser can skip a file it cannot decode.
- Browsers do not share one set of codecs. Offer more than one `<source>`.
- `<video>` can play audio too. `<audio>` is the clearer player for sound alone.

**When to use:**

- Media that should play in the page. A file the user only saves is an `<a href>` with `download`, not a player.

**Examples:**

```html
<video controls width="250">
  <source src="/shared-assets/videos/flower.webm" type="video/webm" />
  <source src="/shared-assets/videos/flower.mp4" type="video/mp4" />
  <a href="/shared-assets/videos/flower.mp4">Download the MP4 video.</a>
</video>

<audio controls src="/shared-assets/audio/t-rex-roar.mp3"></audio>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>What are the attributes of the video and audio elements?</summary>

### What are the attributes of the video and audio elements?

**Definition:**

Both elements take `src`, `controls`, `autoplay`, `loop`, `muted`, `preload`, `crossorigin`, and `loading`. `<video>` also takes `width`, `height`, `poster`, and `playsinline`.

**How it works:**

#### Shared

- These Boolean attributes are on when the attribute is present. `autoplay="false"` still autoplays, and `muted="false"` still mutes. Remove the attribute to turn it off.
- `preload` is a hint: `none`, `metadata`, or `auto` (an empty value means `auto`). Browsers differ on the default; the spec suggests `metadata`. `autoplay` wins over `preload`.
- `crossorigin` is `anonymous` or `use-credentials`.
- `loading` is `eager` (default) or `lazy`.
- `controlslist` may be `nodownload`, `nofullscreen`, or `noremoteplayback`.

#### Video

- `width` and `height` are CSS pixels, not percentages.
- `poster` is an image shown until the first frame is available.
- `playsinline` asks for playback inside the element rather than a forced fullscreen. Omitting it does not guarantee fullscreen.
- `disablepictureinpicture` blocks picture-in-picture.

#### Audio

- There is no `poster`, `width`, or `height`. Lazy audio still needs `controls`, or it is not visible and will not load.

**When to use:**

- `controls` on almost every player. `autoplay` only when the user opted in: browsers block unmuted autoplay.

**Examples:**

```html
<video controls width="250" poster="/images/sample.gif" preload="metadata">
  <source src="sample.mp4" type="video/mp4" />
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>How do you provide subtitles or captions for video content in HTML?</summary>

### How do you provide subtitles or captions for video content in HTML?

**Definition:**

Nest a `<track>` in the `<video>`. `src` is a WebVTT file (`.vtt`), and `kind` says what the track is.

**How it works:**

#### Kinds

- `subtitles` (the default) transcribe or translate dialogue when the sound is available but not understood. `srclang` is required for this kind.
- `captions` include dialogue plus sound effects and music cues, for when the sound is missing or hard to hear.
- `descriptions` summarize the picture and are meant to be spoken. `chapters` are titles for navigation. `metadata` is for scripts and is not shown.
- `default` enables that track unless the user’s preferences pick another. Only one `<track>` per media element may have it.
- `label` is the name in the browser’s track list. `src` must be same-origin unless the media element has `crossorigin`.
- Two tracks may not share the same `kind`, `srclang`, and `label`.
- MDN’s audio page says `<audio>` does not support WebVTT the way `<video>` does. Play that recording in `<video>` if you need a `<track>`, or provide a transcript.

**When to use:**

- Captions when sound may be off. Subtitles when the audience needs another language. A transcript as well, for people who need to read at their own pace.

**Examples:**

```html
<video controls src="/shared-assets/videos/friday.mp4">
  <track
    default
    kind="captions"
    srclang="en"
    label="English"
    src="/shared-assets/misc/friday.vtt" />
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/track
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio
</details>

<details>
<summary>What’s the difference between embedding and linking media?</summary>

### What’s the difference between embedding and linking media?

**Definition:**

Embedding puts a player in the page with `<video>` or `<audio>`. Linking points an `<a href>` at the file so the browser navigates to it or offers a download.

**How it works:**

#### In the page

- An embedded player can show `controls`, several `<source>` types, and a `<track>`.
- The markup inside `<video>` or `<audio>` is fallback, not a second copy of the movie. It is what a browser shows when it does not support the element.

#### A link

- `<a href="report.mp4">` goes to the resource. `download` asks the browser to save a same-origin file.
- The video and audio examples put that link in the fallback so the file is still reachable.

**When to use:**

- Embed when the person should watch or listen without leaving the page. Link when the file is a download or the player is not appropriate.

**Examples:**

```html
<video controls>
  <source src="myVideo.webm" type="video/webm" />
  <source src="myVideo.mp4" type="video/mp4" />
  <a href="myVideo.mp4">Download the MP4</a>
</video>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

### Responsive Design and Media Queries

<details>
<summary>What is a viewport and how can you set it?</summary>

### What is a viewport and how can you set it?

**Definition:**

The viewport is the area of the window where the page is visible. Set it with `<meta name="viewport" content="width=device-width">`.

**How it works:**

#### The meta element

- `content` is a comma-separated list of `key=value` pairs. `width=device-width` sets the layout width to the device screen in CSS pixels.
- Narrow devices otherwise often lay the page out in a virtual viewport around 980px and shrink it. Media queries written for 480px or 640px then never match.
- `initial-scale` is the zoom ratio, from `0.0` to `10.0`. MDN’s recommended snippet omits `initial-scale=1`. Add it when overflowing content makes the browser shrink the page and you want it to start at 100% zoom.
- `user-scalable=no` blocks zoom. Do not use it: low-vision readers need to scale the page, and WCAG asks for at least 2× zoom.

**When to use:**

- On every document that should follow the device width, which is the usual page.

**Examples:**

```html
<meta name="viewport" content="width=device-width" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>Can you describe the use of media queries in HTML?</summary>

### Can you describe the use of media queries in HTML?

**Definition:**

A media query tests conditions such as viewport width and applies CSS only when they match. In HTML, the `media` attribute on `<source>` does the same kind of test for which image to load.

**How it works:**

#### In CSS

- `@media screen and (width >= 80rem)` applies the rules inside only for screen media at that width or wider.
- The width where the layout changes is a breakpoint. A common pattern is one column first, then extra columns once the window is wide enough (mobile first).
- Prefer relative units in breakpoints over a single device’s pixel width.
- Flexible grids and `min`/`max` sizes can adapt without a media query. Media queries are not required for responsive design.

#### In HTML

- On `<source>` inside `<picture>`, `media` is a media condition. If it is false, the browser skips that source and tries the next one.
- The same `media` attribute can choose a video or audio `<source>` for different devices.

**When to use:**

- When the layout or the asset should change at a width, not to build a separate site per phone model.

**Examples:**

```html
<picture>
  <source srcset="mdn-logo-wide.png" media="(width >= 600px)" />
  <img src="mdn-logo-narrow.png" alt="MDN" />
</picture>
```

```css
@media screen and (width >= 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
</details>

<details>
<summary>How do you create responsive images with different resolutions for different devices?</summary>

### How do you create responsive images with different resolutions for different devices?

**Definition:**

Offer several files with `srcset` and tell the browser the display slot with `sizes`, or use `<picture>` when the crop or the format should change.

**How it works:**

#### Resolution and width

- On `<img>`, `srcset` is a comma-separated list. Each candidate is a URL plus a width descriptor (`480w`) or a density descriptor (`2x`). Do not mix `w` and `x` in one list. A missing density means `1x`. Duplicate descriptors are not allowed.
- `sizes` is used with width descriptors. It is a list of media conditions and slot widths. The browser multiplies the chosen slot by the device pixel ratio and picks the closest candidate. Without `sizes`, the default slot is `100vw`.
- For high-DPI screens, `srcset` on `<img>` is the right tool. The browser can still pick a smaller file in a data-saving mode. You do not need a `<picture>` and a `media` condition for density alone.

#### Art direction and formats

- `<picture>` holds `<source>` elements and one `<img>` fallback. `media` swaps the crop. `type` skips a format the browser cannot decode, such as AVIF before WebP before JPEG.
- `max-width: 100%` on `img`, `picture`, and `video` keeps the media inside its column. A single huge file scaled down still costs the bandwidth of the huge file.

**When to use:**

- `srcset` when the picture is the same and only the pixel density changes. `<picture>` when a narrow screen needs a different crop or a newer format needs a fallback.

**Examples:**

```html
<img
  srcset="logo.png, logo-2x.png 2x"
  src="logo.png"
  width="320"
  height="320"
  alt="MDN Web Docs logo" />

<picture>
  <source srcset="photo.avif" type="image/avif" />
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="photo" />
</picture>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

<details>
<summary>What is responsive web design?</summary>

### What is responsive web design?

**Definition:**

Responsive web design is an approach, not one technology: the same page adapts its layout, media, and performance to the screen it is on.

**How it works:**

#### The idea

- HTML with no CSS already reflows text when the window changes. A fixed page width then causes scrollbars on a narrow screen and empty space on a wide one.
- Ethan Marcotte coined the term in 2010 for fluid grids, fluid images, and media queries.
- A fluid image uses `max-width: 100%`, so it shrinks with its column and does not grow past its intrinsic size.
- Flexbox and Grid are responsive by default: items share the free space. Media queries change the layout at breakpoints, preferably in relative units, and usually mobile first.
- Serve an appropriate image instead of one desktop file for every device. Bandwidth and battery still matter on phones.

**When to use:**

- Any page that will be opened on more than one screen size. Do not ship a separate markup tree per device when the same document can adapt.

**Examples:**

```css
img,
picture,
video {
  max-width: 100%;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

<details>
<summary>How do flexbox and grids help in creating responsive layouts?</summary>

### How do flexbox and grids help in creating responsive layouts?

**Definition:**

Flexbox and Grid share free space among items, so the layout flexes with the viewport. A media query can turn that sharing on once the window is wide enough.

**How it works:**

#### Flexbox

- `display: flex` makes a flex container. `flex: 1` on the items gives each an equal share of the free space.
- `flex-grow` and `flex-shrink` say how an item behaves when there is extra space or not enough.
- One pattern keeps a single column by default and sets `display: flex` inside `@media screen and (width >= 600px)`.

#### Grid

- `display: grid` with `grid-template-columns: 1fr 1fr 1fr` makes three tracks that each take one part of the free space. `fr` is a fraction of that space.
- The same breakpoint can switch on `grid-template-columns: 1fr 2fr` and a `column-gap`.
- You do not need a breakpoint for every device width. Add one when the content starts to look bad.

**When to use:**

- Flexbox for a row or a column of items. Grid when rows and columns both matter. Either one replaces a fixed pixel frame.

**Examples:**

```css
@media screen and (width >= 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
</details>

### Accessibility

<details>
<summary>What is accessibility and why is it important in web development?</summary>

### What is accessibility and why is it important in web development?

**Definition:**

Accessibility is making a site usable by as many people as possible. It is aimed at people with disabilities and also helps people on phones and slow networks.

**How it works:**

#### Who it covers

- Visual: blindness, low vision, and color blindness. Tools include zoom and screen readers such as NVDA, VoiceOver, Narrator, and TalkBack.
- Hearing: captions on video and transcripts for audio. Text should stay plain enough to follow.
- Mobility: controls must work from the keyboard, not only from a pointer.
- Cognitive: plain language, a stable layout, few distractions, and forms with clear errors.

#### Why it is part of the job

- Semantic HTML helps assistive technology and also helps search engines.
- In many countries an inaccessible public site is a legal problem. WCAG groups the criteria as perceivable, operable, understandable, and robust.
- Browsers expose an accessibility tree to the operating system’s accessibility API. Styling and script are not that tree. The element’s meaning is.

**When to use:**

- From the first layout, not as a patch at the end. A late retrofit is the expensive case.

**Examples:**

```html
<button type="submit">Save</button>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
</details>

<details>
<summary>How do you make a website accessible?</summary>

### How do you make a website accessible?

**Definition:**

Use the HTML element that already has the right meaning, name every control, keep the keyboard path intact, and test against WCAG while you build.

**How it works:**

#### In the markup

- Headings, landmarks (`header`, `nav`, `main`, `footer`), lists, and buttons tell the accessibility tree what the region is. A `<div>` with a click handler does not.
- Every image has an `alt`. Every form control has a `<label>`. Video gets a `<track>`, and audio gets a transcript.
- Do not remove zoom with `user-scalable=no`. Link text names the destination.
- Publish an accessibility statement and fix problems people report.

#### Testing

- Automated checks catch missing `alt` and weak link text. They do not prove a date picker works with a screen reader.
- Try the page with only the keyboard: Tab through the controls and activate them with Enter or Space.
- “100% accessible” is not a finish line. A chart still needs a data table people can read.

**When to use:**

- On every page. Budget the captions and the alternative for custom widgets at the start.

**Examples:**

```html
<label for="email">Email</label>
<input id="email" name="email" type="email" autocomplete="email" required />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/label
</details>

<details>
<summary>What are ARIA roles and how do you use them?</summary>

### What are ARIA roles and how do you use them?

**Definition:**

An ARIA role names a widget or landmark for assistive technology. Put it on an element only when HTML has no element that already means that thing.

**How it works:**

#### Prefer the element

- Many elements already have a role: `<nav>` is `navigation`, `<main>` is `main`, a heading is a heading. Adding `role="navigation"` to a `<nav>` does not improve it.
- Document-structure roles that duplicate HTML, such as `role="list"` on a list or `role="button"` where a `<button>` would do, should be avoided.
- Roles with no HTML stand-in include `tab`, `tabpanel`, `toolbar`, and `tooltip`. A `tablist` needs `tab` children. A `tabpanel` with no tabs is not a tab panel, and the role then hurts.
- ARIA only adds information to the accessibility tree. It does not change the DOM, the keyboard, or the mouse. If you build a tab set, you still script the keys and the states (`aria-selected`, `aria-controls`).
- Landmark roles (`banner`, `complementary`, `contentinfo`, `main`, `navigation`, `search`) should be few. A page full of them is noise in a screen reader.

**When to use:**

- A custom widget HTML cannot express. For a button, a link, a heading, or a navigation block, use the HTML element and skip the role.

**Examples:**

```html
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    Details
  </button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">…</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles
- https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Guides/Techniques
</details>

<details>
<summary>Explain how to use the tabindex attribute.</summary>

### Explain how to use the tabindex attribute.

**Definition:**

`tabindex` decides whether an element is in the Tab order. Use `0` or `-1`. Do not use a positive number.

**How it works:**

#### The three cases

- `tabindex="-1"` removes the element from Tab order. It can still be focused with `focus()` or a click. That fits a dialog that should take focus when it opens, or an error message that should take focus on a failed submit.
- `tabindex="0"` puts the element in the Tab order at its place in the source, after any positive values.
- A positive value, up to 32767, is visited before `0`, in numeric order. MDN says not to use it: the order stops matching the page, and keyboard users get lost. Order the elements in the document instead.
- Any valid integer makes the element focusable by script or by a click. Only the sign decides whether Tab reaches it.
- Links with `href` and form controls are already in the tab order. Add `tabindex` to them only to change that, usually with `-1`.
- Do not put `tabindex="0"` on a `<div>` and call it a button. That div is not a button in the accessibility tree. Use `<button>`.

**When to use:**

- `-1` to move focus onto something that is not normally a tab stop. `0` only for a custom widget that is genuinely interactive and has no native element.

**Examples:**

```html
<div tabindex="0">Tabbable due to tabindex.</div>
<div>Not tabbable: no tabindex.</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/tabindex
</details>

<details>
<summary>How do you ensure your images are accessible?</summary>

### How do you ensure your images are accessible?

**Definition:**

Give every `<img>` an `alt`. Describe a content image. Use `alt=""` when the image is decoration.

**How it works:**

#### The text

- `alt` is what a screen reader says, and what replaces the image when it does not load or the user has images off.
- An empty `alt` means “skip this”. Omitting `alt` leaves the reader with no replacement, often the file name.
- The text replaces the image in context. A useful value names what is shown, as in the elephant example on the figure page.
- A `<figcaption>` names the figure. It does not replace `alt` on the `<img>`.
- Text that is the only copy of the information must not live only inside the bitmap. Put that text in HTML.
- Each `<area href>` in an image map needs its own `alt`.

**When to use:**

- On every `<img>`, including icons inside links. A linked icon with `alt=""` has an empty name.

**Examples:**

```html
<img
  src="/shared-assets/images/examples/elephant.jpg"
  alt="Elephant at sunset" />

<img src="divider.png" alt="" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/figure
</details>

### Linking and Navigation Techniques

<details>
<summary>How do you make a navigation bar in HTML?</summary>

### How do you make a navigation bar in HTML?

**Definition:**

Put the main links in a `<nav>`, usually as a `<ul>` of `<a>` elements. CSS turns that list into a bar.

**How it works:**

#### What belongs there

- `<nav>` is a major block of navigation: a site menu, a table of contents, or an index. The implicit role is `navigation`.
- Not every link on the page goes in it. A paragraph that happens to contain a link is not a navigation bar.
- A page may have more than one `<nav>`. Name each with `aria-labelledby` or `aria-label` when a screen reader would otherwise hear several unlabeled “navigation” regions.
- The list is the structure. `list-style` and flex or grid are CSS, not extra HTML elements.

**When to use:**

- The primary way around the site or the document. A trail of parent pages is a breadcrumb, still a `<nav>`, with its own label.

**Examples:**

```html
<nav class="menu" aria-label="Site">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
</details>

<details>
<summary>What’s the significance of breadcrumb navigation?</summary>

### What’s the significance of breadcrumb navigation?

**Definition:**

A breadcrumb shows where the current page sits in the site hierarchy. The current page is last and is not a link.

**How it works:**

#### The trail

- MDN marks it up as a `<nav aria-label="Breadcrumb">` containing an `<ol>`. Each ancestor is an `<a>`. The current page is a `<span aria-current="page">`.
- The order is the path from the top of the site down to this page, so the list is ordered, not a bag of links.
- `aria-label` tells assistive technology which navigation this is. `aria-current="page"` marks the page you are on.
- Separators such as “→” are CSS. If you generate them with `content`, screen readers may speak them. Prefer `::after` so the separator is announced after the link, or use an `<img alt="">`.

**When to use:**

- A site with nested sections, where the reader needs the parents of the current page. It does not replace the main menu.

**Examples:**

```html
<nav aria-label="Breadcrumb" class="breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/category">Category</a></li>
    <li><span aria-current="page">Product</span></li>
  </ol>
</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/How_to/Layout_cookbook/Breadcrumb_navigation
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav
</details>

<details>
<summary>How do you create a dropdown menu in HTML?</summary>

### How do you create a dropdown menu in HTML?

**Definition:**

Use `<details>` and `<summary>`. The summary is the closed label. The rest of the details is hidden until the user opens it.

**How it works:**

#### Disclosure

- Closed, only the summary (and the disclosure marker) shows. A click, or Space while the widget is focused, toggles it.
- `open` is Boolean. The presence of the attribute shows the contents. `open="false"` still shows them. Remove the attribute to close.
- The same `name` on several `<details>` keeps only one open. Opening one closes the others. If several in the group have `open`, only the first in source order stays open.
- The links of the menu are ordinary `<a>` elements inside the details. The widget’s implicit role is `group`.
- There is no built-in animation between closed and open.

**When to use:**

- A menu or a block of extra links that should stay closed until asked for. A site-wide bar that is always visible does not need `<details>`.

**Examples:**

```html
<details>
  <summary>Products</summary>
  <ul>
    <li><a href="/bikes">Bikes</a></li>
    <li><a href="/parts">Parts</a></li>
  </ul>
</details>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details
</details>

<details>
<summary>Explain the use of the target attribute in a link.</summary>

### Explain the use of the target attribute in a link.

**Definition:**

`target` names the browsing context that loads the link: the same tab, a new one, the parent, or the top.

**How it works:**

#### Keywords

- `_self` is the default: the current context.
- `_blank` is usually a new tab. Users can set a new window instead. It already behaves like `rel="noopener"`: the new context’s `window.opener` is `null`.
- `_parent` is the parent context, or `_self` if there is none.
- `_top` is the topmost ancestor, or `_self` if there is none.
- Any other value is a named context. If none exists with that name, the browser opens one.
- Say in the link text when the destination leaves the current tab.

**When to use:**

- `_blank` for a reference the reader should keep beside the current page. Leave `target` off for ordinary navigation.

**Examples:**

```html
<a target="_blank" href="https://www.wikipedia.org">
  Wikipedia (opens in new tab)
</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a
</details>

<details>
<summary>How do you create a slidedown menu?</summary>

### How do you create a slidedown menu?

**Definition:**

A disclosure menu that grows open is a `<details>` element. Closed, it is only as tall as the `<summary>`. Open, it expands to show the menu.

**How it works:**

#### Open and closed

- The user opens it with a click or with Space. The `toggle` event fires on `<details>` after the state changes.
- `open` reflects that state. You can set or remove it from script. `open="false"` does not close it.
- Several menus can share a `name` so opening one closes the rest, which is an accordion.
- MDN notes there is no built-in animation of the open/closed change. CSS can style `details[open]` and the summary, but the element itself jumps between the two heights.
- Put the menu links in a list inside the details, under the summary.

**When to use:**

- A panel that should take vertical space only while it is open. Do not invent a second menu widget when `<details>` already does this.

**Examples:**

```html
<details>
  <summary>System Requirements</summary>
  <p>
    Requires a computer running an operating system. The computer must have some
    memory and ideally some kind of long-term storage.
  </p>
</details>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details
</details>

### Advanced HTML Technologies

<details>
<summary>What are Web Components and how are they used?</summary>

### What are Web Components and how are they used?

**Definition:**

Web Components are three platform features for a reusable element whose markup, style, and behavior stay encapsulated: custom elements, shadow DOM, and HTML templates.

**How it works:**

#### The three pieces

- A custom element is a class, registered with `customElements.define(name, Class)`, then used like any other tag.
- Shadow DOM is a tree attached to that element. Page CSS and `querySelectorAll` do not see inside it, and its styles do not leak out.
- A `<template>` holds markup that is not rendered until script clones it, often into the shadow root. `<slot>` is the hole where the page’s own children appear.

#### Lifecycle

- `connectedCallback` runs when the element is connected to the document. `disconnectedCallback` runs when it is removed. `adoptedCallback` runs when it moves to another document. `attributeChangedCallback` runs when a watched attribute changes.

**When to use:**

- A control you will drop into many pages and must not have its classes or ids collide with the page. A one-off widget can stay ordinary HTML.

**Examples:**

```html
<filled-circle color="blue"></filled-circle>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
</details>

<details>
<summary>What is Shadow DOM and how do you use it?</summary>

### What is Shadow DOM and how do you use it?

**Definition:**

Shadow DOM is a DOM tree attached to a host element. The host’s page cannot style or query that tree by accident, and the tree’s styles stay inside it.

**How it works:**

#### Attaching it

- The host is the normal element. The shadow root is the root of the hidden tree. The shadow boundary is where the hidden tree meets the page.
- `element.attachShadow({ mode: "open" })` attaches it. With `open`, `element.shadowRoot` returns that root. With `closed`, `shadowRoot` is `null`. Closed mode is a signal not to poke inside, not a security boundary.
- The same attachment in HTML is `<template shadowrootmode="open">` inside the host. The parser replaces that template with a shadow root.
- `document.querySelectorAll` does not find nodes in the shadow tree. A page rule such as `span { color: blue }` does not paint them. A `<style>` inside the shadow tree does not paint the page.
- `lang` and `dir` on a shadow root are inherited from the host. Content assigned to a `<slot>` still inherits from its parent outside the shadow tree.

**When to use:**

- The internals of a custom element, or any widget whose buttons and ids must not clash with the page. Browsers already use this for the controls inside `<video>`.

**Examples:**

```html
<div id="host">
  <template shadowrootmode="open">
    <span>I'm in the shadow DOM</span>
  </template>
</div>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
</details>

<details>
<summary>How do you create a custom HTML element?</summary>

### How do you create a custom HTML element?

**Definition:**

Extend `HTMLElement`, call `super()` first in the constructor, and register the class with `customElements.define`.

**How it works:**

#### Registration

- `customElements.define("filled-circle", FilledCircle)` makes `<filled-circle>` construct that class. The names in the MDN examples contain a hyphen.
- Build the internals in `connectedCallback`, often with `this.attachShadow({ mode: "open" })`.
- Read attributes with `getAttribute`. The element is then used in markup like any other tag.
- A customized built-in extends a specific element such as a paragraph and is applied with the `is` attribute, or with `{ is: "..." }` in `createElement`.
- `:defined` matches an element whose definition has been registered.

**When to use:**

- When the same behavior and markup should be one tag. Do not register a name that collides with another component; a scoped `CustomElementRegistry` exists for that.

**Examples:**

```html
<filled-circle color="blue"></filled-circle>
```

```js
class FilledCircle extends HTMLElement {
  constructor() {
    super();
  }
  connectedCallback() {
    const shadow = this.attachShadow({ mode: "open" });
    shadow.textContent = this.getAttribute("color");
  }
}
customElements.define("filled-circle", FilledCircle);
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
- https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM
</details>

<details>
<summary>Explain HTML templates and their use cases.</summary>

### Explain HTML templates and their use cases.

**Definition:**

A `<template>` holds an HTML fragment that is not rendered. Script clones `template.content` when it needs a copy, or `shadowrootmode` turns the template into a shadow root immediately.

**How it works:**

#### Not in the page yet

- The nested markup is not a child of the template. `childNodes` is empty. The fragment lives on the read-only `content` property, a `DocumentFragment`.
- Clone it with `document.importNode(template.content, true)` so custom elements inside are constructed with the current document’s registry. `appendChild` on the template element itself does not update `content`.
- A `<slot name="person-name">` in the shadow tree is filled by a light-DOM child with `slot="person-name"`. Fallback text inside the slot shows when nothing is assigned.
- `<template shadowrootmode="open">` (or `closed`) is declarative shadow DOM. The parser attaches a shadow root to the parent and does not leave a template element there. Only the first such template in a parent becomes the shadow root.
- `shadowrootclonable`, `shadowrootdelegatesfocus`, and `shadowrootserializable` set those shadow-root flags. The old `shadowroot` attribute is gone; use `shadowrootmode`.

**When to use:**

- Markup you will stamp out many times, or the internal structure of a custom element. Do not use it for content that should be visible without script, unless it is declarative shadow DOM.

**Examples:**

```html
<template id="custom-paragraph">
  <p><slot></slot></p>
</template>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
- https://developer.mozilla.org/en-US/docs/Web/API/Web_components
</details>

<details>
<summary>How do you use server-sent events?</summary>

### How do you use server-sent events?

**Definition:**

The browser opens a one-way stream with `new EventSource(url)`. The server answers with `Content-Type: text/event-stream` and keeps sending UTF-8 text events.

**How it works:**

#### The client

- Messages with no `event` field arrive as `message`. Read `event.data` in `onmessage`.
- A named `event` field is a custom event. Listen with `addEventListener("ping", ...)`.
- The connection restarts if it drops. `retry` in the stream is the wait in milliseconds. `close()` stops it.
- A cross-origin URL needs `{ withCredentials: true }` when cookies must be sent. `onerror` runs on a network or access failure.
- Without HTTP/2, browsers cap open SSE connections at about 6 per origin. HTTP/2 negotiates the stream limit (often 100).

#### The stream

- Events are separated by a blank line. Fields are `event`, `data`, `id`, and `retry`. A line starting with `:` is a comment and can keep the connection alive.
- Several `data:` lines are joined with newlines.

**When to use:**

- Server-to-browser updates, such as a live list. The browser cannot send events back on this connection. That needs another request, or WebSocket if both directions matter.

**Examples:**

```html
<ul id="list"></ul>
<script>
  const evtSource = new EventSource("sse-demo.php");
  evtSource.onmessage = (event) => {
    const item = document.createElement("li");
    item.textContent = event.data;
    document.getElementById("list").appendChild(item);
  };
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events
</details>

### SEO and HTML

<details>
<summary>How do you optimize HTML for search engines?</summary>

### How do you optimize HTML for search engines?

**Definition:**

Give crawlers a document they can read: a real title, a description, semantic elements, text (not only images), and links they are allowed to follow.

**How it works:**

#### What crawlers use

- Search engines crawl by following links and index the content they find. Ranking details stay a trade secret. The technical side MDN names is semantic HTML, so crawlers meet the content you want indexed.
- `<title>` is the document title. It is suggested for bookmarks and shown in search results. The visible page heading is `<h1>`, and the two are not the same element.
- `<meta name="description" content="...">` is a short summary. It can be shown on the result page and helps the page match relevant searches.
- `<meta name="keywords">` is ignored. It was filled with spam.
- `lang` on `<html>` lets the page be indexed for the right language.

**When to use:**

- On every public page. Write the words your visitors use. Popularity still depends on other sites linking to you, which HTML alone cannot invent.

**Examples:**

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
  <meta
    name="description"
    content="A short summary of what this page is about." />
</head>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>What is semantic HTML and how does it relate to SEO?</summary>

### What is semantic HTML and how does it relate to SEO?

**Definition:**

Semantic HTML names what a region is (`article`, `nav`, `h1`, `p`) instead of wrapping everything in `div`. Crawlers use that structure, plus the text, to understand the subject.

**How it works:**

#### Technical SEO

- MDN’s technical class of SEO is: tag the content with semantic HTML, and let crawlers find only what you want indexed.
- Copy still matters. Images alone do not tell a crawler the subject. Put the same information in text.
- The same elements help assistive technology. MDN notes that this accessibility work also makes the site easier to find.
- A styling wrapper with no meaning stays a `<div>`. It does not describe a section to a crawler.

**When to use:**

- Whenever a more specific element exists. Semantics are the HTML part of SEO. They do not replace a useful title or links from other sites.

**Examples:**

```html
<main>
  <article>
    <h1>Beetles</h1>
    <p>A beetle is an insect with hardened forewings.</p>
  </article>
</main>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility
</details>

<details>
<summary>Explain the significance of heading tags for SEO.</summary>

### Explain the significance of heading tags for SEO.

**Definition:**

`<h1>` through `<h6>` are the six heading levels. `<h1>` is the highest. They outline the page for people, screen readers, and crawlers.

**How it works:**

#### The outline

- Use one `<h1>` that describes the page content. The HTML standard allows more than one if they are not nested, but MDN says that is not best practice. Nesting several `<h1>` elements inside sectioning elements is non-conforming.
- Do not skip a level on the way down: `<h1>`, then `<h2>`, then `<h3>`. Skipping confuses someone who jumps heading to heading. Closing a subsection may return to a higher level.
- Do not use a heading to make text bigger. That is `font-size`.
- User agents can build a table of contents from the headings. Screen readers list them so a person can see the hierarchy.
- `<title>` is metadata for the document. `<h1>` is the heading of the content on the page.

**When to use:**

- Every distinct section that a reader or a crawler should be able to name. A page with no headings is a wall of text.

**Examples:**

```html
<h1>Beetles</h1>
<h2>External morphology</h2>
<h3>Head</h3>
<h4>Mouthparts</h4>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>How do structured data and schemas enhance SEO?</summary>

### How do structured data and schemas enhance SEO?

**Definition:**

Structured data is a standard description of the page, such as a recipe’s name and author, so a search engine can classify that content and may show a rich result.

**How it works:**

#### Formats

- Google Search reads three formats. JSON-LD is the one it recommends: a `script` with `type="application/ld+json"` in the head or the body. Microdata and RDFa put the same kind of facts in HTML attributes.
- Most of this markup uses the schema.org vocabulary. For how Google Search behaves, Google’s own feature guides are the reference. schema.org has extra properties other consumers may use.
- The data must describe content the user can see on that page. Do not invent a blank page just to hold markup, and do not mark up facts that are not visible.
- A type is eligible for an enhanced result only when its required properties are present and accurate. Fewer complete properties beat a long list of broken ones.
- `data-vocabulary.org` markup is no longer eligible for Google rich results. Check markup with the Rich Results Test.

**When to use:**

- A page whose type Google documents, such as a recipe, when you can fill the required fields from text already on the page.

**Examples:**

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Recipe",
    "name": "Recipe title",
    "author": "Recipe author"
  }
</script>
```

**Sources:**

- https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
</details>

<details>
<summary>What are the best practices for using HTML with SEO?</summary>

### What are the best practices for using HTML with SEO?

**Definition:**

Set charset, language, title, and description; outline the page with headings that do not skip levels; write the content in HTML text; and add structured data only for what the page actually shows.

**How it works:**

#### A solid document

- `<meta charset="utf-8">` so the text is not misread.
- `<html lang="...">` so language-specific results and screen readers get the language.
- A unique `<title>` and a `<meta name="description">` written for people. Leave out `name="keywords"`.
- One `<h1>`, then `<h2>` and below without skipped levels. Semantic regions (`main`, `article`, `nav`) around that text.
- `alt` on images, because a crawler does not get the subject from a bitmap alone.
- JSON-LD that repeats visible facts, with every required property filled in. Validate it.

**When to use:**

- As the default shape of a public page. Links from other sites are still what MDN calls the popularity side of SEO.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Beetles</title>
    <meta name="description" content="What a beetle is, and how its body is divided." />
  </head>
  <body>
    <h1>Beetles</h1>
    <p>A beetle is an insect with hardened forewings.</p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/SEO
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements
- https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
</details>

### HTML APIs

<details>
<summary>What is the Geolocation API and how is it used?</summary>

### What is the Geolocation API and how is it used?

**Definition:**

`navigator.geolocation` reads the device location after the user allows it. It is available only in a secure context (HTTPS).

**How it works:**

#### Permission first

- Calling `getCurrentPosition()` or `watchPosition()` prompts unless the permission is already granted or denied. The user can refuse.
- The success callback receives a `GeolocationPosition`: a timestamp and a `GeolocationCoordinates` object with latitude, longitude, and related fields.
- The optional error callback receives a `GeolocationPositionError` with a code and a message.
- `getCurrentPosition()` reads once. `watchPosition()` registers a handler that runs again when the position changes. The same object can clear a watch that was set earlier.
- A third-party iframe needs `allow="geolocation"` and a `Permissions-Policy` that lists that origin. The default allowlist is `self`.

**When to use:**

- A map or a result that depends on where the device is, and only after a clear reason to ask. There is also a `<geolocation>` element for a built-in UI and a simpler permission flow.

**Examples:**

```html
<script>
  navigator.geolocation.getCurrentPosition((position) => {
    const { latitude, longitude } = position.coords;
    document.body.append(`${latitude}, ${longitude}`);
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API
</details>

<details>
<summary>How do you utilize local storage and session storage in HTML?</summary>

### How do you utilize local storage and session storage in HTML?

**Definition:**

`localStorage` keeps string key-value pairs for the origin across browser sessions. `sessionStorage` keeps them only until that page session ends.

**How it works:**

#### Two stores

- Both are `Storage` objects on `window`. Values are UTF-16 strings. `setItem(key, value)` writes, `getItem(key)` reads, `removeItem(key)` deletes one key, `clear()` deletes all.
- `localStorage` has no expiry. In a private window it is cleared when the last private tab for it closes.
- `sessionStorage` is cleared when the page session ends, which MDN describes as when the page is closed.
- Storage is per origin, including the scheme. `http://example.com` and `https://example.com` do not share a store.
- A `SecurityError` is thrown for an origin that is not a scheme/host/port tuple, such as `file:` or `data:`, or when a policy blocks persistence. Blocking cookies usually blocks this store too. `file:` URL behavior is not specified. Do not rely on it.

**When to use:**

- `localStorage` for a preference that should survive a restart. `sessionStorage` for data that should die with the tab. Neither is a place for secrets, and neither is shared across origins.

**Examples:**

```html
<script>
  localStorage.setItem("myCat", "Tom");
  const cat = localStorage.getItem("myCat");
  localStorage.removeItem("myCat");
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
</details>

<details>
<summary>Can you describe the use of the Drag and Drop API?</summary>

### Can you describe the use of the Drag and Drop API?

**Definition:**

The HTML Drag and Drop API lets a user drag an item onto a drop target. Images, links, and text selections are draggable by default. Any other element needs `draggable="true"`.

**How it works:**

#### The gesture

- `dragstart`, `drag`, and `dragend` fire on the dragged item. `dragenter`, `dragleave`, `dragover`, and `drop` fire on a possible target. A file dragged in from the operating system does not fire `dragstart`.
- Most elements are not drop targets. Call `preventDefault()` on `dragover`, or the drop flies back and `drop` never fires.
- Custom drag data is a string (or a `File`) on `event.dataTransfer`. `setData(type, value)` is allowed only in `dragstart`. `getData(type)` is allowed on `drop`.
- Types are MIME types such as `text/plain`, `text/html`, and `text/uri-list`.

**When to use:**

- Moving or copying something inside the page, or accepting a file dropped onto the page. Making an element draggable also blocks ordinary text selection in it unless the user holds Alt.

**Examples:**

```html
<p id="p1" draggable="true">This element is draggable.</p>
<p id="target">Drop Zone</p>
<script>
  document.getElementById("p1").addEventListener("dragstart", (ev) => {
    ev.dataTransfer.setData("text/plain", ev.target.innerText);
  });
  const target = document.getElementById("target");
  target.addEventListener("dragover", (ev) => ev.preventDefault());
  target.addEventListener("drop", (ev) => {
    ev.preventDefault();
    ev.target.append(ev.dataTransfer.getData("text/plain"));
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API
</details>

<details>
<summary>What is the Fullscreen API and why would you use it?</summary>

### What is the Fullscreen API and why would you use it?

**Definition:**

`element.requestFullscreen()` asks the browser to show that element on the whole screen, with the browser UI hidden. `document.exitFullscreen()` leaves that mode.

**How it works:**

#### Enter and leave

- Both methods return a promise. `document.fullscreenElement` is the element now fullscreen, or `null`. `document.fullscreenEnabled` is false when fullscreen is unavailable.
- `fullscreenchange` fires on the transition. `fullscreenerror` fires if the switch fails.
- The user can leave with Esc or F11, by navigating, or by switching apps. Tell them that control exists.
- The old `document.fullscreen` Boolean is obsolete. Use `fullscreenElement`.
- A Permissions Policy named `fullscreen` defaults to `self`: the top document and same-origin frames.

**When to use:**

- A video, a game, or a slide that should occupy the screen. It is not Baseline in every major browser. Check support before depending on it.

**Examples:**

```html
<video id="video" controls src="flower.mp4"></video>
<script>
  const video = document.getElementById("video");
  document.addEventListener("keydown", (e) => {
    if (e.key !== "Enter") return;
    if (!document.fullscreenElement) video.requestFullscreen();
    else document.exitFullscreen();
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API
</details>

### Internationalization

<details>
<summary>How do you handle character encoding in HTML?</summary>

### How do you handle character encoding in HTML?

**Definition:**

Put `<meta charset="utf-8">` in the head. UTF-8 can represent almost any character from any language.

**How it works:**

#### One encoding

- The meta element declares the character set the document is allowed to use. Set it on every page.
- A legacy set such as `ISO-8859-1` covers the Latin alphabet and will mangle characters outside it, such as Japanese.
- Some browsers guess and repair a wrong encoding. Others will not. Declare UTF-8 anyway.
- The event stream for server-sent events is also UTF-8. Storage strings are UTF-16. Those are separate from the document encoding.

**When to use:**

- As one of the first tags in `<head>`, before text the parser might misread.

**Examples:**

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
</head>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
</details>

<details>
<summary>What is the lang attribute and its importance in HTML?</summary>

### What is the lang attribute and its importance in HTML?

**Definition:**

`lang` names the human language of the element. On `<html>` it names the language of the page.

**How it works:**

#### What it changes

- Search engines can index the page for the right language.
- Screen readers use it for pronunciation. The same spelling can be different words in two languages.
- A part of the page in another language gets its own `lang`, for example `<span lang="ja">`.
- The value is a language tag such as `en` or `en-US`. `lang` does not set text direction. That is `dir`.
- A shadow tree inherits `lang` from the host when the parent is a shadow root.

**When to use:**

- On the root of every document, and again on any passage whose language differs from that root.

**Examples:**

```html
<html lang="en-US">
  <body>
    <p>Japanese example: <span lang="ja">ご飯が熱い。</span></p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/dir
</details>

<details>
<summary>How do you accommodate left-to-right and right-to-left language support in HTML?</summary>

### How do you accommodate left-to-right and right-to-left language support in HTML?

**Definition:**

Set the `dir` attribute. `ltr` is left to right, `rtl` is right to left, and `auto` picks a base direction from the text.

**How it works:**

#### Base direction

- `lang` does not imply direction. An Arabic page needs `dir="rtl"` on `<html>` as well as `lang="ar"`. The default, when nothing sets `dir`, is left to right.
- An English passage inside an RTL page gets `dir="ltr"` and its own `lang`. Wrap a short opposite-direction phrase, including its punctuation, in an element with `dir`. `<bdi>` does the same isolation inline.
- `dir="auto"` is for text whose direction you do not know, such as a comment. It uses the first strongly directional character. It is not language detection.
- Omitting `dir` inherits from the parent. It does not turn on automatic detection. A `<bdi>` acts as if `dir="auto"`.
- On a table, `dir="rtl"` orders columns from right to left. Prefer `dir` over the CSS `direction` property so the text is still right when CSS is off.

**When to use:**

- On the root for the page’s script, and on any block or phrase that runs the other way.

**Examples:**

```html
<!doctype html>
<html dir="rtl" lang="ar">
  <head>
    <meta charset="utf-8" />
    <title>صفحة عربية</title>
  </head>
  <body>
    <p>محتوى الصفحة باللغة العربية.</p>
    <p dir="ltr" lang="en">This paragraph is in English.</p>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/dir
</details>

### HTML and Web Development Tools

<details>
<summary>How do you validate HTML?</summary>

### How do you validate HTML?

**Definition:**

Send the document to the W3C Markup Validation Service. It checks the markup and reports what is wrong, with a line and column for each message.

**How it works:**

#### What you give it

- Provide a web address, upload an HTML file, or paste the whole document on the Validate by Direct Input tab, then press Check.
- Browsers parse HTML permissively, so a broken page can still display. The validator is how you find the mistakes the browser papered over.
- Typical reports: an end tag was implied because an element stayed open, an element is unclosed, an end tag breaks nesting, or the end of the file was reached inside an attribute value.
- One fault can cause several messages. Fix a few, then check again. A clean result says there are no errors or warnings to show.

**When to use:**

- Before you ship a page, and whenever the rendered DOM does not match the source you wrote.

**Examples:**

```html
<p>I didn't expect to find the <em>next-door neighbor's <strong>cat</em></strong> here!</p>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://validator.w3.org/
</details>

<details>
<summary>What are the benefits of using an HTML preprocessor like Pug (Jade)?</summary>

### What are the benefits of using an HTML preprocessor like Pug (Jade)?

**Definition:**

Pug compiles a shorter template into a JavaScript function. Call that function with data and it returns an HTML string.

**How it works:**

#### Compile once

- `pug.compile()` or `pug.compileFile()` turns the template into a function. Pass a data object, called locals, and the function returns HTML.
- The compiled function can be called again with different data. `#{name}` in the template is replaced by the value of `name`.
- `pug.render()` and `pug.renderFile()` compile and render in one step. They recompile on every call unless you set the `cache` option.
- The browser receives ordinary HTML. Pug runs before that, usually in Node.

**When to use:**

- When the same markup is filled with many sets of data and you want to compile the template once.

**Examples:**

```text
p #{name}'s Pug source code!
```

```js
const compiledFunction = pug.compileFile("template.pug");
compiledFunction({ name: "Timothy" });
// "<p>Timothy's Pug source code!</p>"
```

**Sources:**

- https://pugjs.org/api/getting-started.html
</details>

<details>
<summary>How does a templating engine work with HTML?</summary>

### How does a templating engine work with HTML?

**Definition:**

A templating engine turns a template plus data into an HTML string. The browser then parses that string as a normal document.

**How it works:**

#### Two steps

- Compile the template into a function. Render by calling the function with the data for this page.
- Placeholders in the template become text in the result. Pug’s `#{name}` is one such placeholder. The result is a string of HTML elements, not a special browser format.
- Reuse the compiled function for each set of data. Compiling again on every render costs extra work.
- This is separate from the HTML `<template>` element, which is inert markup in the document until script clones it.

**When to use:**

- On the server, or in a build step, when many pages share one markup shape.

**Examples:**

```js
pug.renderFile("template.pug", { name: "Timothy" });
// "<p>Timothy's Pug source code!</p>"
```

**Sources:**

- https://pugjs.org/api/getting-started.html
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template
</details>

<details>
<summary>What are browser developer tools, and how do you use them with HTML?</summary>

### What are browser developer tools, and how do you use them with HTML?

**Definition:**

Developer tools are built into the browser. They show the HTML that is actually in the page, the CSS on each element, and the JavaScript that ran.

**How it works:**

#### Inspector

- Open them with Ctrl+Shift+I or F12 on Windows, Command+Option+I on macOS, or by right-clicking the page and choosing Inspect Element. Safari’s tools start from the Develop menu, which you turn on in settings.
- The Inspector tab (Elements in Chrome, Edge, and Safari) shows the runtime DOM. Hover a node to highlight it in the page. Edit as HTML to try a change. The edit is not saved to your file.
- The same panel lists the CSS rules for the selected element, most specific first, and the box model (content, padding, border, margin).
- The Console runs JavaScript against the loaded page and prints errors. The Debugger (Sources in several browsers) pauses on breakpoints.

**When to use:**

- When the page looks wrong and you need the DOM the browser built, not only the file you wrote.

**Examples:**

```js
document.querySelector("html").style.backgroundColor = "purple";
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
</details>

### HTML Best Practices

<details>
<summary>What are some common bad practices in HTML?</summary>

### What are some common bad practices in HTML?

**Definition:**

The usual faults are unclosed elements, elements nested in the wrong order, and an attribute value that never closes its quote. The page can still render, because the browser rewrites the markup.

**How it works:**

#### What goes wrong

- An unclosed `<strong>` can paint the rest of the page bold. A missing closing quote on `href` can make the browser drop the link.
- Badly nested tags are rewritten. `<em>` closed after `</strong>` becomes two separate elements, which may not be the tree you meant.
- Layout tables are a legacy method. Use flexbox or grid for layout, and keep `<table>` for tabular data.
- A classic script with no `async`, `defer`, or `type="module"` is fetched and run before the parser continues. `document.write()` during parsing is especially costly.
- An `<img>` without `width` and `height` gives the browser no box to reserve, so the layout jumps when the image arrives.

**When to use:**

- As a checklist when the DOM inspector and the validator disagree with the file you saved.

**Examples:**

```html
<a href="https://www.mozilla.org/>link to Mozilla homepage</a>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
</details>

<details>
<summary>How can you ensure that your HTML code follows best practices?</summary>

### How can you ensure that your HTML code follows best practices?

**Definition:**

Write lowercase tags, nest each element inside the one that opened it, and run the document through the W3C validator until it reports no errors.

**How it works:**

#### Checks that hold up

- Close elements that need an end tag. MDN treats a missing end tag as a common error with odd results. Tags are case-insensitive; lowercase is the readable form.
- Start the file with `<!doctype html>`, declare `charset`, and set `lang` on `<html>`.
- Valid markup lets other tools run. HTML Tidy can strip whitespace and optional end tags, and it refuses a page with serious markup errors.
- Put `width` and `height` on images so the browser can reserve space. Load scripts that need the DOM with `defer`.

**When to use:**

- On every document, then again after a template or a build step emits HTML.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>HTML debugging examples</title>
  </head>
  <body>
    <h1>HTML debugging examples</h1>
  </body>
</html>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

<details>
<summary>What are the benefits of minifying HTML documents?</summary>

### What are the benefits of minifying HTML documents?

**Definition:**

Minifying removes unnecessary whitespace and comments so the file is smaller. Page weight is the main factor in how fast a page loads.

**How it works:**

#### Smaller bytes

- Less markup means less to download. MDN calls this minimization: drop whitespace and comments you do not need, and move inline script and CSS into external files when that shrinks the page.
- HTML Tidy can strip leading whitespace and extra blank lines from valid HTML. It will not process a page that still has serious markup errors.
- The same idea applies to SVG: drawing tools often leave metadata you can remove, and servers can gzip the result.
- Minifying does not change the DOM the browser builds from the remaining tags. It only changes the bytes on the wire.

**When to use:**

- On the copy you deploy, after the readable source has passed the validator.

**Examples:**

```html
<h1>HTML debugging examples</h1><p>What causes errors in HTML?</p>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

<details>
<summary>How do you optimize the loading time of an HTML page?</summary>

### How do you optimize the loading time of an HTML page?

**Definition:**

Shrink the page, request fewer files, and keep the parser moving. The browser should paint the content and the CSS it needs before it runs interaction scripts.

**How it works:**

#### What to change in the document

- Cut page weight first: drop unused whitespace and comments, and compress images before you reference them.
- Fewer files means fewer HTTP requests. Each extra domain also costs a DNS lookup. Cache reused files; static files usually get a `Last-Modified` header from the server.
- Set `width` and `height` on images. Use `loading="lazy"` for images that are off screen. Lazy images may still be incomplete when the window `load` event fires.
- A classic script blocks parsing unless it has `async` or `defer`. `defer` runs in document order after parsing, before `DOMContentLoaded`. `async` runs as soon as that script is fetched, with no order guarantee. If both are set, the script behaves as `async` only.
- `<link rel="preload">` fetches a subresource the current page needs now. Set `as` to the resource type.

**When to use:**

- On any page where the first view waits on images, extra domains, or scripts in the middle of the body.

**Examples:**

```html
<img src="./images/footerlogo.jpg" alt="MDN logo" loading="lazy" />
<script defer src="js/script2.js"></script>
<link rel="preload" href="main.js" as="script" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Speculative_loading
</details>

### HTML Frameworks and Libraries

<details>
<summary>What are some popular CSS frameworks that can be integrated with HTML?</summary>

### What are some popular CSS frameworks that can be integrated with HTML?

**Definition:**

A CSS framework is a stylesheet you link from the document. Bootstrap is one such toolkit: its CSS and JavaScript are added with a `<link>` and a `<script>`, and the page still starts with `<!doctype html>`.

**How it works:**

#### How it attaches

- Put the CSS `<link rel="stylesheet">` in `<head>`. Put the JavaScript bundle before `</body>`. Bootstrap’s quick start also sets `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- On a CDN copy, set `integrity` to the hash for that file and version, and set `crossorigin="anonymous"`.
- Bootstrap’s JavaScript covers components such as dropdowns, modals, and collapse. Dropdowns, tooltips, and popovers also need Popper; the `bootstrap.bundle` script includes it.
- The document stays HTML. The framework supplies CSS rules and, for some components, script. It does not replace elements.

**When to use:**

- When you want a shared set of styles and components and you are willing to load that stylesheet and script.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bootstrap demo</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css"
      integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB"
      crossorigin="anonymous" />
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
</details>

<details>
<summary>How do frameworks like Bootstrap simplify HTML development?</summary>

### How do frameworks like Bootstrap simplify HTML development?

**Definition:**

Bootstrap ships a ready CSS file and a JavaScript bundle, so you style and add components by linking those files instead of writing every rule yourself.

**How it works:**

#### What you get

- The HTML5 doctype is required. Without it, Bootstrap’s styling is incomplete. The viewport meta tag is how it stays usable on small screens; the project is built mobile first and scales up with media queries.
- Reboot resets common elements so they render more consistently across browsers. Global `box-sizing` is `border-box`, so padding is inside the declared width.
- Components that need script include alerts, carousels, collapse, dropdowns, modals, navbars, and toasts. The CSS link alone does not run those behaviors.
- You can skip Popper, and use `bootstrap.min.js` instead of the bundle, when you do not use dropdowns, popovers, or tooltips.

**When to use:**

- For a prototype or a product page that should pick up Bootstrap’s layout, reset, and components from one documented setup.

**Examples:**

```html
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"
  integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI"
  crossorigin="anonymous"></script>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
</details>

<details>
<summary>Can you name some JavaScript libraries that enhance HTML interactivity?</summary>

### Can you name some JavaScript libraries that enhance HTML interactivity?

**Definition:**

A library is reusable JavaScript that other people packaged. MDN’s overview names React as a UI library, and Angular, Vue, and Ember as frameworks that also render interface from your data.

**How it works:**

#### What they sit on

- The HTML hook is still a `<script>` element. `defer` runs an external classic script after the document is parsed and in source order. `type="module"` treats the file as a module and defers it by default.
- React is a library for rendering UI components. React DOM is the piece that targets the web page. Angular and Vue extend HTML with their own template syntax; Angular compiles those templates to JavaScript. Ember is the older of the four and still in use.
- They exist because updating the DOM by hand, with `document.createElement` and a fresh render after every state change, gets large quickly.
- A framework is a library with opinions about how the application is built. You can still do the same interactions with the DOM APIs and no library.

**When to use:**

- When many parts of the page must stay in sync with changing state, and a maintained library already solves that update loop.

**Examples:**

```html
<script defer src="app.js"></script>
<script type="module" src="main.js"></script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
</details>

### Advanced HTML Topics

<details>
<summary>What are data visualizations in HTML and how can they be implemented?</summary>

### What are data visualizations in HTML and how can they be implemented?

**Definition:**

A data visualization is a drawing of values. In HTML, the Canvas API draws it with JavaScript on a `<canvas>` element. The element itself is a bitmap.

**How it works:**

#### Canvas

- `document.getElementById` finds the element. `getContext("2d")` returns the surface you draw on. `fillStyle` sets the color, and `fillRect(x, y, width, height)` paints a rectangle.
- WebGL uses the same `<canvas>` element for hardware-accelerated 2D and 3D drawing.
- Canvas content is not exposed to accessibility tools the way semantic HTML is. Text drawn as pixels gets blurry when magnified. Put the same information in real markup, such as a table or a heading, beside or instead of the bitmap when the numbers matter.
- MDN also lists canvas libraries aimed at charts and heat maps, including heatmap.js and JavaScript InfoVis Toolkit. The drawing API does not require them.

**When to use:**

- For a graphic that is computed in script. For a few numbers that must be read aloud, start with a table.

**Examples:**

```html
<canvas id="canvas"></canvas>
<script>
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  ctx.fillStyle = "green";
  ctx.fillRect(10, 10, 150, 100);
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
</details>

<details>
<summary>Can you explain how progressive enhancement is applied in HTML?</summary>

### Can you explain how progressive enhancement is applied in HTML?

**Definition:**

Ship the essential content and behavior in HTML so it works widely, then add a richer experience where the browser can run it.

**How it works:**

#### Baseline, then more

- The useful page is markup: links, headings, forms with `action` and `method`. A browser that never runs the script can still submit the form and follow the link.
- Feature detection asks whether this browser has the newer capability. Polyfills supply a missing feature in JavaScript when you choose to.
- Give an accessible alternative where the enhanced version would otherwise hide the content. A `<canvas>` chart is one case: the numbers still belong in text or a table.
- Graceful degradation starts from the full experience and strips it back. Progressive enhancement starts from the simple page and builds up. Both are valid.

**When to use:**

- Whenever script or a new element is optional to reading and completing the task.

**Examples:**

```html
<form action="/search" method="get">
  <label for="q">Search</label>
  <input id="q" name="q" />
  <button>Search</button>
</form>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement
- https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
</details>

<details>
<summary>How are HTML, CSS, and JavaScript interconnected in web development?</summary>

### How are HTML, CSS, and JavaScript interconnected in web development?

**Definition:**

HTML marks up structure and meaning, CSS applies presentation, and JavaScript changes the page after it has loaded. The browser builds a DOM from the HTML, then applies CSS, then runs script against that DOM.

**How it works:**

#### Three layers

- HTML defines the parts: a paragraph, a heading, a button, a table, an image. The browser’s default stylesheet already makes headings larger and links underlined. Author CSS replaces and extends those defaults.
- A stylesheet is a set of rules. A selector such as `button` picks elements, and declarations such as `font-size: 1.4em` set values. Rules in a `<style>` element or in a linked `.css` file are matched to the DOM and painted.
- JavaScript is how the page does more than display static information. `document.querySelector("button")` finds the element, and `addEventListener` runs your function on a click. `textContent` replaces the button’s label.
- Keep the three in the document as HTML, a stylesheet, and a script. The script reads and writes the DOM the HTML created.

**When to use:**

- On every page: structure in HTML, look in CSS, updates in script only where the content must change.

**Examples:**

```html
<button>Player 1: Chris</button>
<script>
  const button = document.querySelector("button");
  button.addEventListener("click", () => {
    button.textContent = "Player 1: updated";
  });
</script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/What_is_CSS
</details>

<details>
<summary>Discuss the importance of documentation in HTML.</summary>

### Discuss the importance of documentation in HTML.

**Definition:**

The element you choose is the documentation of the content. A heading, a paragraph, and a button tell the browser, assistive technology, and the next author what that piece is.

**How it works:**

#### In the file and outside it

- HTML is a markup language for structure. `<p>` marks a paragraph and `<h1>` marks a top-level heading. That choice is what later CSS and script attach to.
- Nest tags in the order you opened them, and write them in lowercase so the file stays readable. A missing end tag is a common source of a DOM you did not intend.
- An HTML comment, `<!-- -->`, is for the author. The HTML 5.2 changes treat comments as part of the HTML syntax, including comments that contain two hyphens.
- The HTML Living Standard is the precise reference. Its introduction says tutorials and authoring guides are the gentler way in when the specification itself is too dense.

**When to use:**

- While you write the markup, and again when someone else has to change the page without guessing which element was meant.

**Examples:**

```html
<!-- The label is the player’s current name. -->
<h1>Match</h1>
<p>First to five points.</p>
<button>Player 1: Chris</button>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
- https://html.spec.whatwg.org/multipage/introduction.html
- https://www.w3.org/TR/2017/REC-html52-20171214/changes.html
</details>

### HTML 5.1, 5.2, and Beyond

<details>
<summary>What updates were introduced in HTML 5.1 and 5.2?</summary>

### What updates were introduced in HTML 5.1 and 5.2?

**Definition:**

HTML 5.1 (W3C Recommendation of 1 November 2016) and HTML 5.2 (14 December 2017) are numbered snapshots. They record what the W3C added and removed relative to the previous snapshot. Current HTML is the WHATWG Living Standard.

**How it works:**

#### HTML 5.1, since HTML5

- Added the `<picture>` element and the `srcset` attribute for responsive images, and `<details>` with `<summary>` for optional extra text.
- Added `rel="noopener"`, and the `nonce` attribute on `<script>` and `<style>` for Content Security Policy.
- Removed application cache, the `usemap` attribute on `<object>`, and the `form` attribute on `<label>`. Nested `<section>` elements that each use `<h1>` no longer build an outline.
- Also added `<menuitem>` and `type="context"`. HTML 5.2 then removed `<menu>` and `<menuitem>`.

#### HTML 5.2, since HTML 5.1

- Added `<dialog>`, integration with JavaScript modules, and `rel="canonical"` on links. `nonce` is also defined on `<link>`.
- Removed `<keygen>`, `<menu>`, and `<menuitem>`, the `showModalDialog()` method, and, in that snapshot, `inputmode` and `dropzone`.
- Newly valid there: a `<style>` element in the body, more than one `<main>` when only one is visible, a `<div>` inside a `<dl>`, and an empty `<option>` in a `<datalist>`.
- An HTML 4 or XHTML 1 strict doctype is not valid HTML in that snapshot.

**When to use:**

- When an interview or an old article cites 5.1 or 5.2. For an element you ship today, read the Living Standard.

**Examples:**

```html
<details>
  <summary>More</summary>
  <p>Extended information.</p>
</details>
<dialog>
  <p>A dialog element added in the HTML 5.2 changes.</p>
</dialog>
```

**Sources:**

- https://www.w3.org/TR/2016/REC-html51-20161101/changes.html
- https://www.w3.org/TR/2017/REC-html52-20171214/changes.html
- https://html.spec.whatwg.org/multipage/introduction.html#history-2
</details>

<details>
<summary>What future updates do you see coming for HTML?</summary>

### What future updates do you see coming for HTML?

**Definition:**

HTML has no scheduled version number to wait for. The Living Standard is updated in place, and a feature lands when implementations and the specification are brought in line.

**How it works:**

#### How a change arrives

- Since the 2019 agreement, this Living Standard is the single HTML document. The published text is titled Living Standard and shows a last-updated date; the edition current on 4 October 2026 carried that day.
- The WHATWG’s working rule is backwards compatibility, and that the specification changes to match what implementations actually do when that is what makes them interoperable.
- Numbered W3C snapshots such as 5.1 and 5.2 stopped being the place new HTML is published. A removal in one of those snapshots is not the last word if the Living Standard has since specified the feature again.
- Author-facing extension points that already exist, and that the specification lists, are `class`, `data-*` attributes, `<meta name>`, `rel`, a `<script type>` that is not JavaScript, and custom elements.

**When to use:**

- When someone asks for a roadmap. Point at the Living Standard’s current text for the feature, not at a future release name.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Current HTML</title>
  </head>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://html.spec.whatwg.org/multipage/introduction.html#extensibility
</details>

<details>
<summary>How does HTML continue to evolve with web standards?</summary>

### How does HTML continue to evolve with web standards?

**Definition:**

HTML is maintained as one Living Standard by the WHATWG, with the W3C collaborating on that same document since 2019. CSS, JavaScript, HTTP, and the DOM are separate specifications that the HTML spec sits among.

**How it works:**

#### One document, many neighbors

- From 1998 the W3C paused HTML in favor of XHTML. In 2004 Apple, Mozilla, and Opera continued HTML at the WHATWG after a W3C workshop rejected that direction.
- In 2011 the groups split on purpose: the W3C wanted a finished HTML5, and the WHATWG wanted to keep fixing known problems and adding features.
- In 2019 they signed an agreement to collaborate on a single version of HTML: the Living Standard.
- The HTML spec’s own map of the platform puts CSS, SVG, HTTP, the DOM, JavaScript, and others beside it. Presentation stays in CSS. Behavior stays in script. HTML defines the elements and the scripting APIs tied to those elements.

**When to use:**

- When you need to explain why a blog post’s “HTML5” and the current specification can disagree.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="styles.css" />
    <title>Layers</title>
  </head>
  <body>
    <h1>Layers</h1>
    <script defer src="app.js"></script>
  </body>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript
</details>

<details>
<summary>What is the Living Standard and how does HTML adhere to it?</summary>

### What is the Living Standard and how does HTML adhere to it?

**Definition:**

The Living Standard is the HTML specification that is continuously maintained instead of frozen. Browsers and authors follow that document. `<!doctype html>` selects this HTML.

**How it works:**

#### What “living” means

- The WHATWG chose a Living Standard so the spec would not ship with known problems left unfixed, and so new features could be added as the platform needed them.
- The specification answers “Is this HTML5?” with yes, in the short sense: “HTML5” is the buzzword for modern web technologies, and this document is one of them. It is not a frozen 5.x release.
- Documents sent as `text/html` are parsed as HTML. The same language can be sent as XML (`application/xhtml+xml`), and there a small syntax error stops a full render. The HTML syntax is the one suggested for most authors.
- Conformance for authors is what the validator checks. The W3C Markup Validation Service is the tool MDN points at for that check.

**When to use:**

- As the reference for any element or attribute you are about to rely on, ahead of a numbered Recommendation from 2016 or 2017.

**Examples:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Living Standard</title>
  </head>
  <body>
    <p>Parsed as HTML because of the doctype and the text/html type.</p>
  </body>
</html>
```

**Sources:**

- https://html.spec.whatwg.org/multipage/introduction.html#history-2
- https://html.spec.whatwg.org/multipage/introduction.html#is-this-html5?
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML
</details>

### HTML Challenges and Real-world Scenarios

<details>
<summary>How do you handle multiple browser support and compatibility issues with HTML?</summary>

### How do you handle multiple browser support and compatibility issues with HTML?

**Definition:**

Write HTML that does the job without the newest feature, then add that feature where feature detection shows the browser can run it.

**How it works:**

#### What to ship

- Progressive enhancement keeps the content in elements that old and new browsers already understand: links, forms, headings. Feature detection checks for the extra capability. A polyfill can fill a gap in JavaScript.
- For modules, serve `<script type="module">` and a second script with `nomodule`. Browsers that understand modules ignore the `nomodule` script. Module scripts defer by default.
- `async` and `defer` avoid parser-blocking classic scripts. MDN’s loading guide says to use them and not to assume every browser honors them.
- Do not require the page to look pixel-perfect in old browsers. MDN’s fast-loading guide says to aim at current browsers that implement the relevant standards.
- Valid HTML still matters: browsers repair broken markup differently, which is its own compatibility bug.

**When to use:**

- Whenever a feature is newer than the oldest browser you still expect to complete the task.

**Examples:**

```html
<script type="module" src="main.js"></script>
<script nomodule src="fallback.js"></script>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script
- https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages
</details>

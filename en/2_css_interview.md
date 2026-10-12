# CSS — Interview

CSS interview questions, with an answer for every question.

[← Back to hub](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — CSS](https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/).

### CSS Basics

<details>
<summary>What does CSS stand for and what is its primary use?</summary>

### What does CSS stand for and what is its primary use?

**Definition:**

**Cascading Style Sheets** (**CSS**) is primarily designed to separate web page content from its visual presentation. This allows for a consistent and adaptable design across multiple web pages.

**How it works:**

#### Key Concepts

- **Selectors**: Elements to which the style rules apply.
- **Properties**: Visual features, such as font-size, color, and background.
- **Values**: Specific settings for properties, like 'red' for the color property.

**Examples:**

#### Visual Example

Here is the HTML code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Example</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to Our Site</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </header>
    <section>
        <h2>Latest News</h2>
        <article>
            <h3>Breaking News: Big Event Tomorrow</h3>
            <p>Join us for our biggest event of the year!</p>
        </article>
    </section>
    <footer>
        &copy; 2022 YourSite
    </footer>
</body>
</html>
```

And, here is the accompanying CSS in `styles.css`:

```css
/* Targeting all text on the page */
body {
  font-family: Arial, sans-serif;
  color: #333;
  background-color: #f4f4f4;
}

/* Targeting the header elements */
header {
  background-color: #1e90ff;
  padding: 1em 0;
  text-align: center;
}

/* Targeting the nav elements */
nav ul {
  list-style-type: none;
  padding: 0;
}

/* Targeting the nav links */
nav a {
  text-decoration: none;
  color: #fff;
  margin: 0 10px;
}

/* Targeting the main section */
section {
  padding: 20px;
}

/* Targeting the footer element */
footer {
  text-align: center;
  margin-top: 50px;
  padding: 10px;
  background-color: #1e90ff;
  color: #fff;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>How do you include CSS in your HTML document?</summary>

### How do you include CSS in your HTML document?

**Definition:**

There are **four primary methods** to incorporate CSS in an HTML document, each presenting unique advantages and use cases.

**How it works:**

#### Methods of CSS Integration

1. **Inline Style**: Directly insert CSS rules within HTML tags.
2. **Embedded Style**: Encompass CSS within the HTML document's `<head>` section.
3. **External Style Sheet**: Create a standalone `.css` file to be referenced in the HTML.
4. **Imported Style Sheet**: Employ `@import` within a `<style>` tag or a CSS file to bring in other CSS files.

#### Key Considerations

- **Specificity**: The degree of influence a selector has over others.
- **Reuse**: The potential to apply the same CSS rules across multiple elements.
- **Maintainability**: The ease with which one can update and manage the CSS.

**When to use:**

#### Best Practices

- **Ideally, Choose a Single Method**: Mixing techniques can complicate maintenance and understanding.
- **Inline Styles for Quick Changes**: Useful when rapid style modifications are necessary.

**Examples:**

#### HTML Code Example: Methods Northwind.CSS

Here is the HTML code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="mystyle.css">
    <title>Document</title>
</head>
    <style>
        h1, h2, p {
            color: green;
        }
        
        div {
            border: 1px solid black;
        }
    </style>
<body>
    <h1>My Header</h1>
    <p>Hello, World!</p>
</body>
</html>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Can you explain the difference between class and ID selectors?</summary>

### Can you explain the difference between class and ID selectors?

**Definition:**

**Class** and **ID** selectors in CSS serve distinct roles and have limitations in their applicability.

**How it works:**

#### Selectivity and Applicability

- **Class Selector** (\`.classname{...}\`): Matches **multiple elements** that share the same class attribute. These elements can belong to various HTML tags (e.g., `<div>`, `<p>`).

- **ID Selector** (`#idName{...}`): Identifies a **single unique element** based on its unique ID attribute. While it's still possible to style multiple elements with the same ID, best practices mandate unique IDs for effective CSS usage.

**When to use:**

#### Common Use Cases

- **Class Selector**: Ideal for styling groups of elements based on shared attributes or type.

- **ID Selector**: Typically reserved for unique elements that require highly specific styling or JavaScript manipulation. While it's valid to use an ID for styling, as stated in the HTML5 specification, it's generally more maintainable to reserve the use of IDs for uniquely identifiable elements and use classes for styling.

**Pluses and minuses:**

#### Efficiency and Performance

- **Class Selector**: Generally faster to compute than ID selectors in modern browsers, particularly when applied to a large number of elements.

- **ID Selector**: Formerly superior in terms of speed, contemporary browsers mitigate this difference.

**Examples:**

#### Code Example: Class and ID Selectors

Here is the HTML code:

```html
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="styles.css" />
  </head>
  <body>
    <div class="content">Content 1</div>
    <div class="content">Content 2</div>
    <div class="content" id="uniqueContent">Special Content</div>
  </body>
</html>
```

Here is the CSS code:

```css
/* Styles applied using class selectors */
.content { color: blue; }

/* Styles applied using ID selector */
#uniqueContent { color: red; }
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>What are pseudo-classes in CSS?</summary>

### What are pseudo-classes in CSS?

**Definition:**

**Pseudo-classes** are special keywords in CSS that allow you to apply styles to elements based not only on their state or position in the document tree but also on user interaction.

**How it works:**

#### Categories of Pseudo-Classes

- **Dynamic Pseudo-classes**: These appear as the user interacts with an element. For instance, `:hover` is activated when the user hovers the cursor over an element.

- **User-action Pseudo-classes**: These capture actions taken by the user, such as `:checked` for input elements that are selected.

- **Relationship Pseudo-classes**: These pertain to the document tree's hierarchical structure, like `:first-child` for an element that's the first child within its parent.


- **Language Pseudo-Classes**: These cater to elements displayed in specific languages, for example `:dir()`.

- **Input Control Pseudo-Classes**: Designed specifically for interactive elements, these pseudo-classes style form controls like buttons, inputs, and text areas. Some examples are `:default`, `:valid`, `:invalid`, and `:optional`.

- **Enabled and Disabled Pseudo-classes**: These are self-explanatory; they alter the style of elements based on whether they're enabled or disabled. Examples include `:enabled` and `:disabled`.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Describe how to implement a CSS reset and why it is useful.</summary>

### Describe how to implement a CSS reset and why it is useful.

**Definition:**

A **CSS reset** is a set of styles intended to reduce browser inconsistencies in elements such as margins, paddings, and various typical style defaults.

**Pluses and minuses:**

#### Benefits of CSS Reset

- **Consistent Starting Point**: Eliminates default styling differences across browsers, making the design process more predictable.
- **Consistent Box Model**: Ensures uniform calculations of element sizing (e.g., widths and heights) to prevent unexpected layout shifts.
- **Want Only Custom Styles**: It's especially useful if you intend to start from a blank slate and apply your own bespoke styles.

**Examples:**

#### The Code

For HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="styles.css" rel="stylesheet">
  <title>Document</title>
</head>
<body>
  <h1>Hello, CSS Reset!</h1>
</body>
</html>
```

And for CSS, here is a simple **normalize.css**-based reset:

```css
/* reset.css */
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */
h1 {
    font-size: 2em;
    margin: 0.67em 0;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Selectors and Combinators

<details>
<summary>How do you select elements by attribute in CSS?</summary>

### How do you select elements by attribute in CSS?

**Definition:**

While programming in CSS, you can leverage **attribute selectors** to define rules based on the presence or value of specific HTML attributes.

**How it works:**

#### Variations

You can utilize attribute selectors in three distinct ways:

- **Exact Match**: `[]` selects an exact attribute value.
- **Value Starts With**: `[^]` targets attributes with specified starting values.
- **Case Insensitive**: Selectors are usually case-sensitive, but by using `i`, you can make them case-insensitive.

**Pluses and minuses:**

#### Benefits

Using attribute selectors has multiple advantages, such as:

- **Versatility**: They cater to a wide range of scenarios.
- **Simplicity**: They are easy to use.
- **Consistency**: They're a part of a standard set of CSS selectors.

**Examples:**

Here is the CSS code snippet:

```css
/* Exact Match */
[class="important"] {
  color: red;
}

/* Value Starts With */
[href^="https"] {
  color: green;
}

/* Case Insensitive */
[alt="home" i] {
  background: url('home-icon.png');
}
```

In the example above,

- `[class="important"]` selects all elements with the exact `class` attribute set to "important".
- `[href^="https"]` will style all anchor links with an `href` attribute that starts with "https".
- `[alt="home" i]` targets the `alt` attribute with a value of "home" in a case-insensitive manner.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>What is a pseudo-element, and what are they used for?</summary>

### What is a pseudo-element, and what are they used for?

**Definition:**

**Pseudo-elements** are virtual elements that give developers the power to style parts of an HTML document that don't correspond to actual HTML elements. Essentially, they let you apply styles to specific parts of an element without the need for extra HTML markup.

Commonly used `pseudo-elements` include `::before` and `::after` which let developers insert content before or after an element, respectively.

**How it works:**

#### Key Features

- **Automatic Insertion**: These pseudo-elements can add content continuously without requiring manual code changes.
- **Dynamic Content**: With generated content and styles, pseudo-elements can adapt based on the specific conditions.
- **Custom Styling**: Pseudo-elements enable developers to style parts of an element differently than the rest.

**When to use:**

#### Practical Applications

1. #### Indicating External Links

   - **Link**: Indicating content that opens an external website.
   - **Implementation**: Visual or textual cues like arrows or "External Link" next to anchor elements.

2. #### Specialized Numbers and Letters

   - **Link**: Styling a single letter or number within a text block.
   - **Implementation**: Especially useful in design, for instance, highlighting the first letter of a paragraph with a larger font size.

3. #### Responsive Backgrounds

   - **Link**: Apply background images or colors specific to certain parts of an element for various screen sizes.
   - **Implementation**: Use media queries within the pseudo-element for specific screen sizes.

4. #### Code Blocks and Blockquotes

   - **Link**: Add decorative elements preceding and following code blocks and blockquote elements.
   - **Implementation**: Help highlight code samples or visually delineate long blockquote sections.

5. #### Custom Radio Buttons and Checkboxes

   - **Link**: Rework default styling for radio buttons and checkboxes for a more customized look.
   - **Implementation**: Use ::before or ::after with content property to replace default appearance.

6. #### Clear Floats

   - **Link**: Overcome challenges in parent containers not respecting the height of floated child elements and collapsing.
   - **Implementation**: Create an element with ::after pseudo-element where the content clears the floats.

7. #### Hacks for Older Browsers

   - **Link**: Sometimes, especially with prior versions of Internet Explorer, using pseudo-elements proves crucial for achieving desired stylings.
   - **Implementation**: Useful for applying specifically crafted styles that wouldn't work properly on older browsers without this technique.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Explain the difference between the child combinator and the descendant combinator.</summary>

### Explain the difference between the child combinator and the descendant combinator.

**Definition:**

The **child combinator** (>) and the **descendant combinator** (~) both serve to target HTML elements with CSS. However, they **operate in different ways**.

**How it works:**

#### Distinct Characteristics

- Child Combinator **>**: Selects an HTML element that is a **direct child** of another element.
- Descendant Combinator **~**: Matches an HTML element that is a **descendant** (direct or indirect child) of another specified element.

**When to use:**

#### Best Practices for Combinator Use

1. **Specificity of Selection**: Implement the **child combinator** $>$ when you want to target a specific, direct child of an element.
2. **Minimize Global Targeting**: Utilize the **descendant combinator** cautiously as it has the potential for global targeting. It's often a good habit to opt for more specific selectors.
3. **Balance Styling and Performance**: As a rule of thumb, more specific selectors could improve rendering speed. Use combinators with a balanced approach keeping in mind both specificity and performance needs.

**Examples:**

#### Code Example

Here is the CSS:

```css
/* target direct children of the parent element */
nav > ul > li {
  color: red;
}

/* target any descendant list items under nav */
nav li {
  color: blue;
}
```

Here is the HTML:

```html
<nav>
  <ul>
    <li>Direct Child</li> <!-- This is red -->
    <li>
      Nested Child <!-- This is blue -->
      <ul>
        <li>Nested List Item</li> <!-- This is blue -->
      </ul>
    </li>
  </ul>
</nav>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>How would you select all direct children elements of a particular type?</summary>

### How would you select all direct children elements of a particular type?

**Definition:**

To **select all direct children** of a specific type in CSS, you can use the `>` **child selector** combined with the desired element to build the selector.

**Examples:**

For example, to select all the direct children that are `<li>` elements within an `<ul>` element, you would use the following CSS:

```css
ul > li {
  /* Styles here */
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>What are the universal selector and the sibling combinator, and when would you use them?</summary>

### What are the universal selector and the sibling combinator, and when would you use them?

**Definition:**

The **Universal Selector** (the asterisk, `*`) is a powerful tool that enables you to target every element within a specified container. While it's a straightforward selector, its implications can be broad.

**How it works:**

The **Sibling Combinator** (`+`) in CSS targets elements that are immediately preceded by a specified element. Unlike child (`>`) or descendant (whitespace) selectors, the **sibling combinator** allows direct sibling targeting.

**When to use:**

- **When to Use**: You might want to normalize or reset specific CSS properties (resetting padding, margin, etc.) across all elements within a container or the entire document. The **Universal Selector** effectively achieves this.

- **Best Practices**: Overuse of the **Universal Selector** can lead to performance issues and unexpected style side effects. Keep its use concise and well-defined.

#### When To Use Sibling Combinator

- **When to Use**: For DOM structures where direct sibling relationships are key, such as tabbed content or multi-step forms.

- **Best Practices**: While direct sibling targeting is useful, ensure it's the most efficient method for your objective. Overreliance can lead to inflexible CSS and HTML structures.

**Examples:**

#### Code Example: Universal Selector

Here is the CSS:

```css
/* Remove margins, paddings on all elements within the container */
.containers > * {
    margin: 0;
    padding: 0;
}
```

The HTML:

```html
<div class="container">
    <p>Paragraph 1</p>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
</div>
```

#### Code Example: Sibling Combinator

Here is the CSS:

```css
/* Style the direct sibling anchor tag when a list item is hovered */
ul li:hover + a {
    color: red;
}
```

The HTML:

```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li><a href="#">Link</a></li>
</ul>
```

#### Case Study: Practical Applications

Let us take a real-world example.

#### Resetting Margins and Paddings

In this scenario, you have a parent container and you want to remove the default margins and paddings from all its child elements.

The **Universal Selector** can accomplish this:

Here is the CSS:

```css
.container > * {
    margin: 0;
    padding: 0;
}
```

The HTML:

```html
<div class="container">
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</div>
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Box Model and Layout

<details>
<summary>What is the CSS Box Model?</summary>

### What is the CSS Box Model?

**Definition:**

The **Box Model** is the foundational concept in CSS that describes the structure of an HTML element. It encompasses four key components: **content**, **padding**, **border**, and **margin**.

**How it works:**

#### Box Model Components

- **Content**: The actual element content, such as text, images, or other visual or interactive elements.
- **Padding**: Clears an area around the element's content, inside the border. The padding is transparent and doesn't have a background color or border.
- **Border**: A solid line that defines the boundary of the padding area.
- **Margin**: Clears an area around the element's border, outside any defined background or border.

#### Visual Representation

![CSS Box Model](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fcss-box-model.jpg?alt=media&token=f36af6aa-69e3-4cc5-bdce-8a0558d22861)

#### Key Attributes

- **Height** & **Width**: Element dimensions are determined by the sum of content width/height, and any padding, border, or margin added to it.
- **Border**: Specifies the size, style, and color of the border surrounding the content and padding.
- **Margin**: Defines the **clearance** between adjacent elements.

#### Margins

#### Auto Margins
When the surrounding container has a defined width, horizontal margins set to "auto" equally distribute the remaining horizontal space on both sides of the element, centering it within the container.

#### Parent and Child Element Interplay

#### Box Sizing

By default, the width of an element does not include padding or border. CSS can alter this behavior using the `box-sizing` property:

- **Content-Box** (default): The element's specified width and height are calculated excluding padding and border. When you change the width or height of an element using CSS, this is the model being used.
- **Border-Box**: The width and height comprise the content, padding, and border, avoiding the expansion of the box when adding padding or border to an element.

This distinction aids in layout control and ensures uniformity.

**Examples:**

#### Code Example: Box Model

Here is the HTML code:
```html
<div id="boxModelExample">This is an example of text within the Box Model.</div>
```

Here is the CSS code:
``` CSS
#boxModelExample {
    border: 5px solid red;
    padding: 20px;
    margin: 20px;
}
```

``` CSS
#autoMarginExample {
    width: 50%;
    margin-left: auto;
    margin-right: auto;
}
```

``` CSS
#borderBoxExample {
    box-sizing: border-box;
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>Explain margin collapsing.</summary>

### Explain margin collapsing.

**Definition:**

**Margin collapsing** can occur when there are adjacent vertical margins between elements. In such cases, the larger of the two margins becomes the "collapsed margin."

**How it works:**

#### Visual Representation of Margin Collapsing

- Vertical margin collapse: 
![Vertical Margin Collapse](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fvartical-margin-collapse.jpg?alt=media&token=7a9f7961-302b-48c7-b74e-168f6780897f)
- Horizontal margin collapse does not occur: ![Horizontal Margin Collapse](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fhorizontal-margin-collapse.jpg?alt=media&token=da23e77b-72aa-4c17-a287-3f274753be9a)

#### Preventing Margin Collapsing

1. **Padding or Borders**: Introduce padding or borders to disrupt the margin collapsing behavior when required.  
2. **Inline-Block or Floats**: Transform adjacent elements into `inline-block` or use `float` property.
3. **Clearfix**: Employ a clearing technique for floated elements, which separates subsequent elements in the flow, ultimately preventing margin collapse.

#### CSS Rules and Margin Collapsing

1. **Marginal vs. Calculated Total**: 
   - If margins are set, every other margin is respected.
   - If margins are calculated to be a non-zero value, margins can be collapsed.
   - Margins from both the top and the bottom end of the elements are considered.
2. **Non-Root Elements**: The root `<html>` element's margins don't collapse.
3. **Non-Adjacent Items**: A margin doesn't collapse through wrapping or inline elements.

While margin collapsing can simplify layout management under specific conditions, it's essential to understand its behavior to ensure predictable and consistent designs.

**Examples:**

#### Types of Margin Collapsing

1. **Adjacent Sibling Collapsing**: When two blocks or inline-block elements are hierarchically adjacent, such as being direct children of a common container, their top and bottom margins might collapse. For instance, in the code:

   ```html
   <div>
       <p>This is paragraph 1</p>
       <p>This is paragraph 2</p>
   </div>
   ```

2. **Parent-Child Collapsing**: If there are no intervening content or padding or border between the parent and the first/last child inside it, the parent's margins might collapse with the child's corresponding margin.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>What are the different values for the box-sizing property and what do they do?</summary>

### What are the different values for the box-sizing property and what do they do?

**Definition:**

The `box-sizing` property has three possible values:

1. **Content-Box**: Box dimensions are calculated by adding the `width` and `height` to the content only. This is the default behavior. Mathematically, this is represented as: $$ \text{Content Width/Height} = \text{Specified Width/Height} $$

2. **Border-Box**: The `width` and `height` of the box are calculated by including both the content and padding within the specified values. Essentially, the browser will adjust the **content width and height measurements to include the padding and border** thickness. Mathematically, this is represented as: $$ \text{Content Width/Height} = \text{Specified Width/Height} - \text{Padding} - \text{Border} $$

3. **Initial**: This value sets the `box-sizing` property to its default value, which is typically `content-box`.

**How it works:**

#### Visual Representation

![Box-Sizing Properties in CSS](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/questions-images%2Fcss-box-sizing.jpg?alt=media&token=6bba3876-148a-4b03-a5cc-c96bb8fddb5a)

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>How do you center a block element with CSS?</summary>

### How do you center a block element with CSS?

**Definition:**

To center a **block-level** element using CSS, there are several methods, which handle either horizontal or vertical alignment.

**How it works:**

#### Horizontal Centering

There are three common options to horizontally center a block:

1. **Auto margins**: by setting both left and right margins to `auto`.
2. **Flexbox**: placing the block in a flex container and setting `justify-content: center`.
3. **Grid**: applying the `place-items` property on the container set to grid layout.

#### Vertical Centering

To vertically center a block, modern techniques such as Flexbox and Grid layout can be used.

**Examples:**

#### Code Example: Auto Margins
```css
.container {
  width: 50%;
  margin: 0 auto; /* Center horizontally */
}
```

#### Code Example: Flexbox
```css
.container {
  display: flex;
  justify-content: center; /* Horizontally center child elements */
}
```

#### Code Example: Flexbox
```css
.container {
  display: flex;
  align-items: center; /* Vertically center child elements */
}
```

#### Code Example: CSS Grid
```css
.container {
  display: grid;
  align-items: center; /* Vertically center child elements */
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

<details>
<summary>What is the difference between block, inline, and inline-block elements?</summary>

### What is the difference between block, inline, and inline-block elements?

**Definition:**

**Block elements**, by default, stack vertically, while **inline elements** sit next to each other on the same line.

**Inline-block elements** combine characteristics of both, stacking vertically and allowing for adjustments in height, margin, and padding.

**Examples:**

#### Code Example: Key Differences

Here is the HTML:

```html
<div class="block">Block</div>
<span class="inline">Inline</span>
<span class="inline-block">Inline-Block</span>
```

Here is the CSS:

```css
div, span {
  display: block;
  margin-bottom: 10px;
}

.inline-block {
  display: inline-block;
  background: lightblue;
  padding: 10px;
}
```

In `div` and `span` due to default display type (block): text is separated by
a gap.

In `span.inline-block`: with `display: inline-block`, text shares a line but is visually separated by its background.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/css-interview-questions/
</details>

### Positioning and Stacking Contexts

<details>
<summary>Explain the different values for the position property.</summary>

### Explain the different values for the position property.

**Definition:**

`position` sets how an element is placed. The initial value is `static`. The other values are `relative`, `absolute`, `fixed`, and `sticky`.

**How it works:**

#### Values

- **static**: Normal flow. `top`, `right`, `bottom`, `left`, and `z-index` have no effect.
- **relative**: Stays in flow, then offsets from its own position. The original space stays reserved.
- **absolute**: Removed from flow. Offsets are from the nearest positioned ancestor, or the initial containing block if none exists.
- **fixed**: Removed from flow. Offsets are from the viewport, unless an ancestor has `transform`, `perspective`, `filter`, or `will-change: transform` set to something other than `none`. That ancestor becomes the containing block.
- **sticky**: Stays in flow until a scroll threshold, then sticks. At least one inset (`top`, `right`, `bottom`, or `left`) must be a value other than `auto`.

**When to use:**

#### Choosing a value

- **relative**: A small offset, or a containing block for an absolute child.
- **absolute**: Badges and overlays that should not reserve space.
- **fixed**: UI that stays in the viewport while the page scrolls.
- **sticky**: A heading that sticks only inside its section.

**Examples:**

```css
.badge {
  position: absolute;
  top: 0;
  right: 0;
}

.toolbar {
  position: sticky;
  top: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Describe z-index and how stacking order is controlled in CSS.</summary>

### Describe z-index and how stacking order is controlled in CSS.

**Definition:**

`z-index` sets the stack level of a positioned element, or of a flex or grid item. In the same stacking context, a larger value paints above a smaller one.

**How it works:**

#### Values

- **auto**: Stack level is `0`. The box does not create a new local stacking context.
- **integer**: That integer is the stack level, including negative numbers. The box also creates a local stacking context, so descendant `z-index` values are compared only inside it.

#### Stacking contexts

- A stacking context is painted as one unit in its parent. A child cannot escape it, no matter how large its `z-index` is.
- `position: fixed` and `position: sticky` always create a stacking context.
- `position: relative` or `absolute` creates one when `z-index` is not `auto`. A flex or grid item does the same.
- Other triggers include `opacity` less than `1`, `isolation: isolate`, and `transform`, `filter`, `perspective`, or `clip-path` other than `none`.

**When to use:**

- Use a small integer scale (`1`, `2`, `3`) on positioned, flex, or grid boxes that actually overlap. Raise a parent context before raising a child that must cover a sibling of that parent.

**Examples:**

```css
.card {
  position: relative;
  z-index: 1;
}

.card .badge {
  position: absolute;
  z-index: 2;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Positioned_layout/Stacking_context
</details>

<details>
<summary>How do you create a sticky footer with CSS?</summary>

### How do you create a sticky footer with CSS?

**Definition:**

Make the page a column flex container at least as tall as the viewport, and let the main region grow. The footer then sits at the bottom of a short page and follows the content on a long page.

**How it works:**

#### Flex column

- **`min-height: 100vh`** on `body` makes the column at least one viewport tall.
- **`display: flex`** with **`flex-direction: column`** stacks header, main, and footer.
- **`flex: 1`** on `main` is the shorthand that lets that item grow and shrink from a zero basis, so it absorbs free space and pushes the footer down.

**When to use:**

- Site footers and card footers, when short content should not leave the footer floating in the middle of the viewport.

**Examples:**

```css
body {
  min-height: 100vh;
  margin: 0;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

<details>
<summary>Can you explain how to create a fixed header that remains at the top on scroll?</summary>

### Can you explain how to create a fixed header that remains at the top on scroll?

**Definition:**

Set the header to `position: fixed` and `top: 0`. It stays at the top of the viewport while the document scrolls. Pad the content by the header height so the first lines are not hidden under it.

**How it works:**

#### Fixed box

- **`fixed`** removes the header from flow, so no space is reserved for it.
- Offsets are from the viewport, unless an ancestor sets `transform`, `perspective`, `filter`, or `will-change: transform` to a value other than `none`.
- **`left: 0`** and **`right: 0`** stretch the header across the viewport when `width` is `auto`.
- `fixed` always creates a stacking context. On print, the element is placed in the same position on every page.

**When to use:**

- A global bar that must stay visible on a long page. If it should release at the end of a section, use `sticky` instead.

**Examples:**

```css
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}

main {
  padding-top: 4rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>When would you use position: sticky?</summary>

### When would you use position: sticky?

**Definition:**

Use `position: sticky` when an element should scroll with its section, then stay visible until that section ends. It needs an inset other than `auto` on the axis that should stick.

**How it works:**

#### Threshold

- The element is treated as `relative` until its containing block crosses the inset threshold, then it is treated as stuck.
- It sticks inside the nearest ancestor that scrolls (`overflow` of `hidden`, `scroll`, `auto`, or `overlay`), until the opposite edge of its containing block.
- If both insets on an axis are `auto`, `sticky` behaves as `relative` on that axis.
- `sticky` always creates a stacking context.

**When to use:**

- Alphabet headings, table headers, and in-section toolbars that should not cover the next section. `fixed` would stay for the whole page.

**Examples:**

```css
dt {
  position: sticky;
  top: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

### Styling Text and Fonts

<details>
<summary>How do you change the font of text in CSS?</summary>

### How do you change the font of text in CSS?

**Definition:**

Set `font-family` to a comma-separated list. The browser uses the first family that is installed or loaded with `@font-face`.

**How it works:**

#### Font stack

- Quote family names that contain spaces, such as `"Trebuchet MS"`.
- End the list with a generic family: `serif`, `sans-serif`, `monospace`, `cursive`, `fantasy`, or `system-ui`.
- `font-size`, `font-weight`, `font-style`, and `line-height` change size and shape. The `font` shorthand can set several of them in one declaration.

**When to use:**

- Set the stack on `body` so text inherits it. Override it on a component only when that component needs a different family.

**Examples:**

```css
body {
  font-family: "Trebuchet MS", Verdana, sans-serif;
  font-size: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-family
</details>

<details>
<summary>What is a web-safe font?</summary>

### What is a web-safe font?

**Definition:**

A web-safe font is a family that is already installed on many operating systems, so the page can show it without downloading a font file. A real stack still ends with a generic family.

**How it works:**

#### Fallback

- The browser walks `font-family` from left to right and uses the first face it can render.
- Generic names such as `sans-serif`, `serif`, `monospace`, and `system-ui` map to a face the system already has.
- Names often used in that role include Arial, Verdana, Georgia, `"Times New Roman"`, `"Courier New"`, and `"Trebuchet MS"`. They are not guaranteed on every device.

**When to use:**

- Use a web-safe stack when you do not want a network request for text, or as the fallback after a custom face in `@font-face`.

**Examples:**

```css
body {
  font-family: Verdana, "Trebuchet MS", sans-serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-family
- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Text_styling/Fundamentals
</details>

<details>
<summary>When would you use a font shorthand property?</summary>

### When would you use a font shorthand property?

**Definition:**

Use the `font` shorthand when one declaration should set the family together with size, and optionally style, variant, weight, width, and line height.

**How it works:**

#### Required pieces

- `font-size` and `font-family` are required, unless you use a system-font keyword such as `caption`.
- Line height follows size with a slash: `1.2em/1.5`.
- Optional values before the size are `font-style`, `font-variant`, `font-weight`, and `font-width`.
- Any constituent property you omit is reset to its initial value, including `line-height`. The shorthand also resets font-feature properties such as `font-kerning` to their initial values.

**When to use:**

- A single rule that should fully describe the font, such as a heading. Set longhands separately when you only want to change weight or size and keep the rest.

**Examples:**

```css
h1 {
  font: italic bold 1.2em/1.5 "Fira Sans", sans-serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font
</details>

<details>
<summary>How can you include and use custom fonts on a webpage?</summary>

### How can you include and use custom fonts on a webpage?

**Definition:**

Declare the face with `@font-face`, give it a `font-family` name, and point `src` at a font file. Then use that name in `font-family`.

**How it works:**

#### @font-face

- `src` lists files the browser can choose from. Prefer WOFF2, and add `format("woff2")` so unsupported formats are skipped.
- `font-weight` and `font-style` on the at-rule tell the browser which file matches `bold` or `italic`.
- `font-display: swap` shows fallback text immediately, then swaps in the custom face when it loads.
- Put `@font-face` before the rules that use the family.

**When to use:**

- Brand or reading faces that are not installed on the user's system. Keep a generic family at the end of the stack.

**Examples:**

```css
@font-face {
  font-family: "Zantroke";
  src: url("fonts/zantroke.woff2") format("woff2");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

body {
  font-family: "Zantroke", serif;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@font-face
</details>

<details>
<summary>What is the difference between em and rem units?</summary>

### What is the difference between em and rem units?

**Definition:**

`em` is relative to the element's font size. `rem` is relative to the root element's font size.

**How it works:**

#### Font-relative lengths

- On `font-size` itself, `em` uses the inherited font size, not the size being declared.
- On other properties, such as `margin` or `padding`, `em` uses the element's computed `font-size`. Nested `em` values compound.
- `rem` uses the `font-size` of the root element, usually `html`. On the root's own `font-size`, `rem` means the initial size. A common browser default is `16px`, which user settings can change.
- Children inherit the computed length, not the `em` or `rem` token.

**When to use:**

- `rem` for type scale, spacing, and layout that should follow the user's root font size.
- `em` when a component's padding or icon size should scale with that component's own font size.

**Examples:**

```css
html {
  font-size: 100%;
}

h1 {
  font-size: 2rem;
  margin-bottom: 0.5em;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/length
</details>

### Colors, Backgrounds, and Gradients

<details>
<summary>How do you add a background image to an element?</summary>

### How do you add a background image to an element?

**Definition:**

Set `background-image` to `url()` or to a gradient. The image paints behind the content and padding, and above `background-color`.

**How it works:**

#### Painting the image

- `background-repeat` controls tiling. `no-repeat` shows the image once.
- `background-position` places it. `background-size` scales it. `cover` fills the box and may crop; `contain` fits the whole image inside the box.
- The `background` shorthand can combine color, image, position, size, repeat, and origin in one declaration.

**When to use:**

- Decorative images that are not content. Put meaningful pictures in `<img>` so they have alternative text.

**Examples:**

```css
.hero {
  background-image: url("hero.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background-image
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background
</details>

<details>
<summary>What is the difference between hex codes, RGB, and named color values?</summary>

### What is the difference between hex codes, RGB, and named color values?

**Definition:**

All three write an sRGB color. A named color is a keyword, hex packs channels as hexadecimal digits, and `rgb()` writes the channels as numbers or percentages.

**How it works:**

#### Notations

- **Named**: keywords such as `rebeccapurple` and `aliceblue`. The set is fixed by CSS.
- **Hex**: `#rgb` is a short form of `#rrggbb`. Each pair is red, green, and blue from `00` to `ff`.
- **rgb()**: `rgb(255 0 153)` or `rgb(100% 0% 60%)`. The modern form separates channels with spaces. An optional alpha follows a slash: `rgb(255 0 153 / 80%)`.
- `hsl()`, `hwb()`, `lab()`, `lch()`, `oklab()`, and `oklch()` are other color functions. They are not hex, `rgb()`, or named colors.

**When to use:**

- Named colors for a few well-known keywords. Hex or `rgb()` when you need an exact channel value. Prefer one notation inside a design system.

**Examples:**

```css
.swatch {
  color: rebeccapurple;
  background-color: #ff0099;
  border-color: rgb(255 0 153 / 80%);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value
</details>

<details>
<summary>What are alpha transparency and RGBA?</summary>

### What are alpha transparency and RGBA?

**Definition:**

Alpha is the opacity channel of a color. `0` is fully transparent and `1` or `100%` is fully opaque. `rgba()` is the older function name for an RGB color plus that channel. The modern form is `rgb()` with a slash.

**How it works:**

#### Alpha channel

- `rgb(189 85 218 / 0.25)` and `rgb(189 85 218 / 25%)` paint the same red, green, and blue at one quarter opacity.
- The color is composited over whatever is behind the element. It does not fade the element's children the way `opacity` does.
- `opacity` applies to the whole element, including descendants. A color alpha applies only to that color.

**When to use:**

- A translucent fill, border, or text color. Use `opacity` when the whole element, including its children, should fade.

**Examples:**

```css
.panel {
  background-color: rgb(189 85 218 / 0.25);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value/rgb
</details>

<details>
<summary>How do you create a gradient background with CSS?</summary>

### How do you create a gradient background with CSS?

**Definition:**

A gradient is an image generated by CSS. Set it with `background-image` or the `background` shorthand. `linear-gradient()` blends colors along a line.

**How it works:**

#### Gradient functions

- The first argument of `linear-gradient()` is an angle, such as `45deg`, or a side such as `to left top`. Then come color stops.
- A stop can name a position: `green 40%`. Two stops at the same position make a hard edge.
- `radial-gradient()` blends from a center outward. `conic-gradient()` blends around a center.
- Gradients have no intrinsic size. `background-size` and the default positioning decide how they fill the box.

**When to use:**

- Fades, stripes, and decorative fills that should scale with the element. They do not need an image file.

**Examples:**

```css
.banner {
  background-image: linear-gradient(45deg, blue, red);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/gradient/linear-gradient
</details>

<details>
<summary>Explain how to implement multiple backgrounds on a single element.</summary>

### Explain how to implement multiple backgrounds on a single element.

**Definition:**

Give `background-image` or `background` a comma-separated list. The first image is painted on top. Later images sit underneath it.

**How it works:**

#### Layers

- Each comma-separated layer can have its own position, size, and repeat. List those values in the same order as the images.
- `background-color` is painted below every image. In the `background` shorthand, only the final layer may include a color.
- A gradient counts as an image, so it can sit in the same list as `url()`.

**When to use:**

- A photo plus a tint, or a pattern over a gradient, on one element.

**Examples:**

```css
.panel {
  background-image:
    linear-gradient(rgb(0 0 0 / 40%), rgb(0 0 0 / 40%)),
    url("photo.png");
  background-size: cover, cover;
  background-color: #222;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background-image
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/background
</details>

### Flexbox

<details>
<summary>Describe Flexbox and its advantages.</summary>

### Describe Flexbox and its advantages.

**Definition:**

Flexbox is a one-dimensional layout. `display: flex` makes the element a flex container and its in-flow children flex items, laid out on a main axis and a cross axis.

**How it works:**

#### Axes

- `flex-direction` sets the main axis. The initial value is `row`, so the cross axis runs vertically. `column` swaps them.
- `justify-content` distributes free space on the main axis. `align-items` aligns items on the cross axis. The initial `align-items` value is `stretch`.
- `flex-grow`, `flex-shrink`, and `flex-basis` (or the `flex` shorthand) decide how items share space. `gap` adds space between items.

**When to use:**

- A row or column of items: navigation, toolbars, centering, and cards that should share leftover space. Use Grid when rows and columns both need tracks.

**Examples:**

```css
.nav {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
</details>

<details>
<summary>How does flex-grow work?</summary>

### How does flex-grow work?

**Definition:**

`flex-grow` is the flex grow factor. It sets how much of the container's positive free space is added to the item's main size. The initial value is `0`, so the item does not grow.

**How it works:**

#### Ratio

- Positive free space is the container's main size minus the items' main sizes.
- If every item has the same factor greater than `0`, that free space is split evenly. The number is a ratio: `1` and `1` match `2` and `2`.
- If the factors differ, each item's share is its factor divided by the sum of the factors.
- Negative values are invalid. Prefer the `flex` shorthand so `flex-shrink` and `flex-basis` are set together. `flex: 1` means grow `1`, shrink `1`, and a basis of `0`.

**When to use:**

- An item that should absorb leftover space, such as a flexible input next to a button. Leave `flex-grow` at `0` for items that should stay at their content size.

**Examples:**

```css
.row {
  display: flex;
}

.grow {
  flex-grow: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

<details>
<summary>Explain the difference between justify-content and align-items.</summary>

### Explain the difference between justify-content and align-items.

**Definition:**

`justify-content` aligns flex items along the main axis. `align-items` aligns them along the cross axis.

**How it works:**

#### Which axis

- With the initial `flex-direction: row`, the main axis is horizontal, so `justify-content` moves items left and right. `align-items` moves them up and down.
- With `flex-direction: column`, the axes swap: `justify-content` is vertical and `align-items` is horizontal.
- Common `justify-content` values are `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, and `space-evenly`.
- `align-items` starts at `stretch`. Other values include `flex-start`, `flex-end`, `center`, and `baseline`.

**When to use:**

- `justify-content` for leftover space between items in a row or column. `align-items` to line every item up on the cross axis, including vertical centering in a row.

**Examples:**

```css
.box {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Aligning_items
</details>

<details>
<summary>When would you use align-self?</summary>

### When would you use align-self?

**Definition:**

`align-self` overrides `align-items` for one flex item on the cross axis. The initial value is `auto`, which uses the container's `align-items`.

**How it works:**

#### One item

- The accepted values match `align-items`, including `stretch`, `flex-start`, `flex-end`, `center`, and `baseline`.
- It does not change the main axis. Main-axis placement for one item is done with `margin: auto` or with the item's `flex` factors.
- In a column flex container the cross axis is horizontal, so `align-self` then controls horizontal alignment.

**When to use:**

- One item in a row should be centered, stretched, or pinned to the start while the others follow `align-items`.

**Examples:**

```css
.box {
  display: flex;
  align-items: flex-start;
}

.box .selected {
  align-self: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Aligning_items
</details>

<details>
<summary>How do you create equal-width columns using Flexbox?</summary>

### How do you create equal-width columns using Flexbox?

**Definition:**

Make the container `display: flex` and set each column to `flex: 1`. That shorthand gives every item the same basis of `0`, then shares the free space equally.

**How it works:**

#### Equal columns

- `flex: 1` expands to grow `1`, shrink `1`, and a basis of `0`. Items start from zero and split the container, so longer text does not make a wider column.
- `flex-grow: 1` alone grows from the content size (`flex-basis: auto`). Columns with more content stay wider.
- `flex: auto` is `1 1 auto`. Items grow by the same amount of free space but keep their content size as the basis, so they are not equal when the content differs.

**When to use:**

- A single row of equal columns. For wrapping rows with a minimum column width, Grid with `repeat(auto-fit, minmax(...))` fits better.

**Examples:**

```css
.container {
  display: flex;
}

.container > * {
  flex: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
</details>

### CSS Grid

<details>
<summary>What is the CSS Grid layout, and why is it useful?</summary>

### What is the CSS Grid layout, and why is it useful?

**Definition:**

CSS Grid is a two-dimensional layout. `display: grid` divides the container into rows and columns, and places children into those cells.

**How it works:**

#### Tracks

- `grid-template-columns` and `grid-template-rows` define the tracks. The `fr` unit shares free space. `repeat()` repeats a track list.
- Items flow into cells automatically. `grid-column`, `grid-row`, and `grid-area` place or span an item explicitly.
- `gap` sets the space between tracks. Grid items can overlap when their areas share cells.

**When to use:**

- Page regions, card grids, and any layout where horizontal and vertical alignment both matter. Flexbox is enough for a single row or column.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout
</details>

<details>
<summary>How do you define grid columns and rows?</summary>

### How do you define grid columns and rows?

**Definition:**

Set `grid-template-columns` and `grid-template-rows` on the grid container. Each value in the list is one track.

**How it works:**

#### Track sizes

- A length such as `100px`, a percentage, or `1fr` sizes a track. `fr` is a fraction of the free space after fixed tracks are placed.
- `repeat(3, 1fr)` is three equal columns. `repeat(auto-fill, minmax(120px, 1fr))` adds as many columns as fit, each at least `120px`.
- `grid-auto-rows` sizes rows that are created implicitly when there are more items than explicit rows.

**When to use:**

- Define both axes when the layout has a known structure. Leave rows to `grid-auto-rows` when only the column count is fixed and rows should grow with the items.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 100px 100px;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-rows
</details>

<details>
<summary>What is the difference between grid-template-areas and grid-template-columns?</summary>

### What is the difference between grid-template-areas and grid-template-columns?

**Definition:**

`grid-template-columns` sizes the column tracks. `grid-template-areas` names rectangular regions made of those cells and does not by itself set the track sizes.

**How it works:**

#### Names versus sizes

- Each string in `grid-template-areas` is one row. Repeated names in a rectangle, such as `"a a b"`, form one area. A period `.` is an empty cell.
- An item opts into an area with `grid-area: a`.
- The number of columns in every row string must match. Pair the areas with `grid-template-columns` when the tracks need explicit sizes such as `1fr`.

**When to use:**

- `grid-template-areas` when the layout is a set of named regions, such as header, sidebar, and content. `grid-template-columns` alone when you only need equal or flexible tracks and automatic placement.

**Examples:**

```css
.wrapper {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-areas:
    "header header"
    "nav content";
}

header {
  grid-area: header;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-areas
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
</details>

<details>
<summary>How do you place items in the CSS Grid layout?</summary>

### How do you place items in the CSS Grid layout?

**Definition:**

Place an item by naming the grid lines it spans. `grid-column` and `grid-row` are the shorthands. `grid-area` can name an area or set all four lines.

**How it works:**

#### Line numbers

- Lines are numbered from `1`. `grid-column: 1 / 3` starts at line 1 and ends at line 3, so the item spans two columns.
- `span 2` covers two tracks from the start line.
- `grid-area: header` uses a name from `grid-template-areas`.
- Items without a placement are auto-placed into the next free cell.

**When to use:**

- Explicit placement for a page shell. Leave the rest of the items to auto-placement when their order in the source is the order you want.

**Examples:**

```css
.box1 {
  grid-column: 1 / 2;
  grid-row: 1 / 4;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Line-based_placement
</details>

<details>
<summary>How is overlapping of grid items handled in CSS Grid?</summary>

### How is overlapping of grid items handled in CSS Grid?

**Definition:**

Grid items overlap when their column and row spans cover the same cells. Within one stacking context, `z-index` decides which item paints on top. Grid items can use `z-index` even when `position` is `static`.

**How it works:**

#### Paint order

- If `z-index` is `auto` on both items, the one that comes later in the source paints above the earlier one.
- An integer `z-index` sets the stack level and creates a stacking context for that grid item.
- A negative `z-index` paints the item below siblings with `auto` or a higher integer.

**When to use:**

- Overlays inside a cell, such as a caption over a tile. Do not rely on source order when the overlap must stay stable as the markup changes.

**Examples:**

```css
.box1 {
  grid-column: 1 / 4;
  grid-row: 1 / 3;
  z-index: 2;
}

.box2 {
  grid-column: 1;
  grid-row: 2 / 4;
  z-index: 1;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Basic_concepts
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index
</details>

### Responsive Design and Media Queries

<details>
<summary>What is a media query?</summary>

### What is a media query?

**Definition:**

A media query applies CSS only when the browser and device match a condition, such as a viewport wider than a given length.

**How it works:**

#### @media

- The at-rule is `@media`, then an optional media type (`screen`, `print`, `all`), then features in parentheses.
- `(min-width: 35rem)` matches when the viewport width is at least that length. `(max-width: 50rem)` matches when it is at most that length.
- The range form `(width >= 480px)` means the same kind of test as `min-width`.
- Features can also test things other than width, such as `prefers-reduced-motion` or `hover`.

**When to use:**

- Different layouts for different viewport sizes, and a separate set of rules for print.

**Examples:**

```css
@media (min-width: 35rem) {
  .layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>How does the viewport meta tag assist with mobile responsive designs?</summary>

### How does the viewport meta tag assist with mobile responsive designs?

**Definition:**

The viewport `<meta>` tells the mobile browser to lay the page out at the device width in CSS pixels instead of a wide virtual viewport, commonly about `980px`, that is then scaled down.

**How it works:**

#### width=device-width

- Without the tag, a narrow screen still uses a wide layout viewport, so desktop CSS looks tiny.
- `content="width=device-width"` sets that layout width to the screen width. Media queries then see the real device width.
- `initial-scale=1` sets the initial zoom. The usual tag includes both.

**When to use:**

- Every document that uses responsive CSS. Put the tag in `<head>`.

**Examples:**

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>Describe how to create a mobile-first responsive design.</summary>

### Describe how to create a mobile-first responsive design.

**Definition:**

Write the base CSS for a narrow viewport, then add layout for wider screens inside `min-width` media queries.

**How it works:**

#### Narrow first

- The default rules are the small-screen layout: a single column, readable type, and spacing that fits a phone.
- `@media (min-width: 480px)` and larger breakpoints add columns, side-by-side navigation, and wider spacing. They do not undo a desktop layout.
- The range syntax `(width >= 480px)` is the same idea: styles apply from that width upward.
- The viewport meta tag must set `width=device-width` or the narrow base is still laid out in a wide virtual viewport.

**When to use:**

- New layouts. Enhancing upward keeps the small-screen CSS smaller and avoids fighting `max-width` overrides.

**Examples:**

```css
.layout {
  display: grid;
  gap: 1rem;
}

@media (width >= 480px) {
  .layout {
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>How can you build a layout that changes from a two-column to a single-column with media queries?</summary>

### How can you build a layout that changes from a two-column to a single-column with media queries?

**Definition:**

Start from two columns, then inside a `max-width` query set the grid back to one column. A mobile-first version does the opposite: one column by default, two columns from a `min-width` breakpoint up.

**How it works:**

#### Breakpoint

- `(max-width: 40rem)` matches viewports that are `40rem` wide or narrower, so the single-column rule wins there.
- `(min-width: 40rem)` matches viewports that are at least `40rem`, so the two-column rule is the enhancement.
- One property is enough to change the layout: `grid-template-columns`.

**When to use:**

- Prefer the `min-width` form for new work. Use `max-width` when an existing two-column layout must collapse on small screens.

**Examples:**

```css
.layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

@media (max-width: 40rem) {
  .layout {
    grid-template-columns: 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>Explain the use of min-width and max-width in media queries.</summary>

### Explain the use of min-width and max-width in media queries.

**Definition:**

`min-width` makes the query true at that viewport width and above. `max-width` makes it true at that width and below.

**How it works:**

#### Direction

- `@media (min-width: 35rem)` is the mobile-first test: base styles apply to everyone, and this block adds styles for wider viewports.
- `@media (max-width: 50rem)` is the desktop-first test: it overrides styles only while the viewport is narrow enough.
- The range syntax writes the same tests as `(width >= 35rem)` and `(width <= 50rem)`.
- A band uses both, for example `(min-width: 35rem) and (max-width: 50rem)`.

**When to use:**

- `min-width` when each breakpoint adds layout. `max-width` when a wide layout must be reduced on smaller screens.

**Examples:**

```css
@media (min-width: 35rem) {
  body {
    background: yellow;
  }
}

@media (max-width: 50rem) {
  body {
    border: 2px solid blue;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

### Transitions and Animations

<details>
<summary>How do you create a CSS transition?</summary>

### How do you create a CSS transition?

**Definition:**

A transition interpolates a property from its old computed value to a new one when that value changes. The `transition` shorthand sets the property, duration, easing, and delay.

**How it works:**

#### Shorthand order

- `transition: margin-right 4s ease-in-out 1s` is property, duration, easing, then delay.
- Duration is required for a visible change. The initial duration is `0s`.
- Several properties are comma-separated. `transition-property: all` is valid but animates every animatable property that changes.
- The change usually comes from a state such as `:hover`, `:focus-visible`, or a class.

**When to use:**

- A short response to a state change, such as a color or transform on hover. Use an animation when the motion has its own keyframes or must repeat.

**Examples:**

```css
.button {
  transition: background-color 200ms ease-out;
}

.button:hover {
  background-color: blue;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
</details>

<details>
<summary>What is the difference between a CSS transition and animation?</summary>

### What is the difference between a CSS transition and animation?

**Definition:**

A transition runs between two computed values when a property changes. An animation plays a `@keyframes` sequence and can start on its own, repeat, and reverse.

**How it works:**

#### What each one needs

- A transition needs a duration and a change of an animatable property. It does not define intermediate stops.
- An animation needs `animation-name` pointing at `@keyframes`, plus a duration. `animation-iteration-count`, `animation-direction`, and `animation-fill-mode` control looping, reversing, and the styles before and after the run.
- Both use an easing function: `transition-timing-function` or `animation-timing-function`. The keyword `ease` is the initial value.

**When to use:**

- Transitions for hover, focus, and expanding panels. Animations for a loop, a multi-step sequence, or motion that should play when the element appears.

**Examples:**

```css
.box {
  transition: transform 200ms ease-out;
}

.sun {
  animation: 4s linear infinite alternate sunrise;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation
</details>

<details>
<summary>How do you animate an item's entrance and exit on the page?</summary>

### How do you animate an item's entrance and exit on the page?

**Definition:**

Animate the entrance with `@keyframes` or a transition from a hidden state, usually `opacity` and `transform`. An exit can transition only while the element is still in the document.

**How it works:**

#### Enter and leave

- `transform` and `opacity` can be animated without laying out the page again. Prefer them over animating `top`, `width`, or `height`.
- For an entrance, `@keyframes` can run from `opacity: 0` and `translateY(1rem)` to the visible values as soon as the element is shown.
- For an exit, change those properties with a `transition` before removing the element. Taking the node out of the document stops the transition immediately.
- Discrete properties such as `display` need `allow-discrete` in the transition if they should participate.

**When to use:**

- A toast, dialog, or panel. Respect `prefers-reduced-motion: reduce` by shortening or removing the motion.

**Examples:**

```css
.panel {
  opacity: 0;
  transform: translateY(1rem);
  transition: opacity 200ms ease-out, transform 200ms ease-out;
}

.panel.is-open {
  opacity: 1;
  transform: none;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes
</details>

<details>
<summary>Can you explain how to use @keyframes?</summary>

### Can you explain how to use @keyframes?

**Definition:**

`@keyframes` names a sequence of styles. The `animation` shorthand or `animation-name` plays that sequence on an element.

**How it works:**

#### Stops

- `from` and `to` are `0%` and `100%`. Intermediate stops use percentages, such as `50%`.
- Each stop lists the properties to interpolate. Properties missing from a stop are interpolated from surrounding stops when they are animatable.
- `animation: 4s linear infinite alternate sunrise` sets duration, easing, iteration count, direction, and the keyframe name.
- If `animation-fill-mode` is `forwards`, the element keeps the styles from the last keyframe after the animation ends.

**When to use:**

- Motion with more than two states, or motion that should run without a hover or class change.

**Examples:**

```css
@keyframes sunrise {
  from {
    transform: translateY(110vh);
  }
  to {
    transform: translateY(0);
  }
}

.sun {
  animation: 4s linear infinite alternate sunrise;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation
</details>

<details>
<summary>How do you set the timing functions in CSS animations?</summary>

### How do you set the timing functions in CSS animations?

**Definition:**

`animation-timing-function` sets the easing curve for an animation. The same curves are available on `transition-timing-function`. The initial value is `ease`.

**How it works:**

#### Curves

- `linear` moves at a constant rate. It equals `cubic-bezier(0, 0, 1, 1)`.
- `ease` speeds up in the middle and slows at the end. `ease-in` starts slow. `ease-out` starts fast and slows down. `ease-in-out` does both.
- `cubic-bezier()` takes four numbers. The first and third must be from `0` to `1`.
- `steps(n, jump-end)` holds the value at equal intervals. `jump-end` is the default step position. `step-end` equals `steps(1, jump-end)`.
- The easing can also be the third value of the `animation` shorthand, as in `animation: 4s linear sunrise`.

**When to use:**

- `ease-out` for an element arriving on screen. `linear` for a continuous loop. `steps()` for a frame-by-frame sprite.

**Examples:**

```css
.sun {
  animation-name: sunrise;
  animation-duration: 4s;
  animation-timing-function: linear;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation-timing-function
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/easing-function
</details>

### Advanced CSS Techniques

<details>
<summary>What is a CSS preprocessor and why would you use one?</summary>

### What is a CSS preprocessor and why would you use one?

**Definition:**

A CSS preprocessor is a language that compiles to CSS. Sass is the common one. It adds variables, nesting, mixins, and functions that the browser does not parse itself.

**How it works:**

#### Compile step

- You author `.scss` (or another preprocessor syntax). A build tool emits plain `.css` for the browser.
- Sass variables (`$color`) are resolved at compile time. They do not change in the browser and do not inherit the way custom properties do.
- Nesting in the source becomes separate selectors in the output. Deep nesting raises specificity and makes the CSS longer.
- The browser only sees the generated CSS, so a preprocessor does not add runtime features.

**When to use:**

- A codebase that already compiles Sass, or shared mixins that must be expanded before delivery. For values that should follow the cascade or change with a theme at runtime, use custom properties.

**Examples:**

```scss
$brand: #1e90ff;

.button {
  background: $brand;

  &:hover {
    background: darken($brand, 8%);
  }
}
```

**Sources:**

- https://sass-lang.com/documentation/
</details>

<details>
<summary>Describe the importance of using CSS variables (custom properties).</summary>

### Describe the importance of using CSS variables (custom properties).

**Definition:**

Custom properties are author-defined values, written `--name`, and read with `var()`. They cascade and inherit, so one declaration can theme a whole subtree.

**How it works:**

#### Cascade

- Define `--primary-color: blue` on `html` or on a component. Descendants read it with `color: var(--primary-color)`.
- `var(--name, fallback)` uses the fallback when the property is not set.
- Changing the custom property on a parent updates every use. That is different from a Sass variable, which is gone after compilation.
- The `@property` at-rule can set a syntax, an initial value, and whether the property inherits, which also lets the browser interpolate it in animations.

**When to use:**

- Design tokens: color, space, and type that themes and components share. A media query or a class on an ancestor can swap the tokens without rewriting every rule.

**Examples:**

```css
html {
  --primary-color: red;
}

h1 {
  color: var(--primary-color);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
</details>

<details>
<summary>How do you use CSS modules?</summary>

### How do you use CSS modules?

**Definition:**

CSS Modules scope class names and animation names to the file by default, so two components can both use `.button` without colliding. A bundler imports the file as an object of generated class names.

**How it works:**

#### Local names

- A class in `Button.module.css` is local. The build rewrites `.button` to a unique name and exposes it as `styles.button`.
- You put that string on the element: `class={styles.button}`. The browser still receives normal CSS.
- A class marked `:global(.theme)` keeps its written name so it can match markup outside the module.
- Composition is a build feature. It is not a CSS property the browser understands on its own.

**When to use:**

- Component CSS in a bundler (webpack, Vite, or a framework compiler) when you want file-level scope without a runtime CSS-in-JS library.

**Examples:**

```css
.button {
  color: white;
  background: #1e90ff;
}
```

```js
import styles from "./Button.module.css";

document.querySelector("button").className = styles.button;
```

**Sources:**

- https://github.com/css-modules/css-modules
</details>

<details>
<summary>Can you explain the concept of CSS-in-JS?</summary>

### Can you explain the concept of CSS-in-JS?

**Definition:**

CSS-in-JS writes style rules in JavaScript, usually next to a component. A library generates class names and injects or extracts the CSS.

**How it works:**

#### Colocated styles

- The style can read component props and theme values because it is in the same language as the UI.
- Many libraries hash the class name, which gives the same kind of local scope as CSS Modules.
- Some libraries insert a `<style>` tag at runtime. Others extract static CSS at build time so the browser does not pay a runtime cost.
- The result is still CSS. Specificity, the cascade, and layout behave as usual once the rules are in the document.

**When to use:**

- A component system that already styles from JavaScript and needs styles to follow props. Prefer plain CSS or CSS Modules when the styles are static and you want no styling runtime.

**Examples:**

```js
const Button = styled.button`
  color: white;
  background: #1e90ff;
`;
```

**Sources:**

- https://styled-components.com/docs/basics
</details>

<details>
<summary>What tools would you use for CSS linting and formatting?</summary>

### What tools would you use for CSS linting and formatting?

**Definition:**

Use Stylelint to catch invalid or discouraged CSS, and Prettier to format it. They solve different jobs: one reports problems, the other rewrites whitespace.

**How it works:**

#### Lint versus format

- Stylelint parses CSS, SCSS, and CSS-in-JS and reports unknown properties, duplicate selectors, and rules you configure, such as a ban on `!important`.
- Prettier reprints the file with a consistent layout. It does not decide whether a property is valid.
- Run both in the editor and in CI so a pull request fails on a lint error. Let Prettier own formatting and turn off Stylelint rules that only care about spacing.
- A browser's developer tools show the cascade for one element. They are not a substitute for linting the source.

**When to use:**

- Any shared stylesheet. Add Stylelint when more than one person edits CSS. Add Prettier when the argument is about how the file looks.

**Examples:**

```json
{
  "extends": ["stylelint-config-standard"]
}
```

**Sources:**

- https://stylelint.io/
- https://prettier.io/docs/
</details>

### Styling Forms and User Interfaces

<details>
<summary>How do you style a submit button to appear as a regular link?</summary>

### How do you style a submit button to appear as a regular link?

**Definition:**

Keep a `<button type="submit">` and remove the native button chrome: no background, no border, and no extra padding. Then give it link color and an underline.

**How it works:**

#### Link look, button behavior

- `background: none`, `border: 0`, and `padding: 0` drop the button face.
- `font: inherit` and `color` stop the browser's default button font and color.
- `text-decoration: underline` and `cursor: pointer` match a link.
- The control stays a button, so Enter in the form still submits and the accessible role stays a button.

**When to use:**

- A low-emphasis action in a sentence, such as "Forgot password?". Do not replace the primary submit with a link style.

**Examples:**

```css
button.link {
  background: none;
  border: 0;
  padding: 0;
  font: inherit;
  color: #1e90ff;
  text-decoration: underline;
  cursor: pointer;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>How can you create custom checkboxes and radio buttons with CSS?</summary>

### How can you create custom checkboxes and radio buttons with CSS?

**Definition:**

Set `appearance: none` so the control loses its native painting, then draw the box with ordinary CSS. `:checked` styles the on state. The input stays a real checkbox or radio.

**How it works:**

#### appearance: none

- `none` gives the widget a primitive appearance that CSS can paint. It does not change the control's behavior.
- Size the control with `width` and `height`, then set `border` and `background`. Use `:checked` for the selected state.
- `none` can hide the widget entirely if you do not give it a size and a background. The control is still clickable through its label.
- Keep a visible `:focus-visible` outline. A label's `for` attribute must match the input `id`.

**When to use:**

- When the native control clashes with the design. For a simple color change, `accent-color` tints the native control and avoids rebuilding it.

**Examples:**

```css
input[type="checkbox"] {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 2px solid currentColor;
  background: transparent;
}

input[type="checkbox"]:checked {
  background: currentColor;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>Describe how to style a dropdown select box without JavaScript.</summary>

### Describe how to style a dropdown select box without JavaScript.

**Definition:**

The closed `<select>` can be styled with `font`, `color`, `padding`, `border`, and `background`. `appearance: none` removes the native face of that closed control. The popup list stays a native picker unless you opt into `appearance: base-select`.

**How it works:**

#### What CSS can reach

- `appearance: none` on the `<select>` lets you paint the closed box, including a custom chevron with `background-image`.
- The option popup is still the platform UI for `appearance: auto`. CSS on `option` is limited and inconsistent.
- `appearance: base-select` on the select and on `::picker(select)` lets you style the picker. Support is limited. The picker is then a top-layer box, not the operating-system menu.
- Do not remove the visible control or its focus style. The select must stay usable without a script.

**When to use:**

- Font, color, and border on the closed control are safe. Reach for `base-select` only when you have checked support and still provide a usable closed control.

**Examples:**

```css
select.custom {
  appearance: none;
  font: inherit;
  padding: 0.5rem 2rem 0.5rem 0.75rem;
  border: 1px solid #333;
  background: url("chevron.svg") no-repeat right 0.6rem center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/appearance
</details>

<details>
<summary>What is the best practice for styling placeholder text of an input field?</summary>

### What is the best practice for styling placeholder text of an input field?

**Definition:**

Style placeholder text with the `::placeholder` pseudo-element. Keep it a hint, not a label, and give it enough contrast to read.

**How it works:**

#### ::placeholder

- `::placeholder` accepts a limited set of properties, the ones that apply to `::first-line`, including `color` and font properties.
- The placeholder disappears once the user types. It is not a substitute for `<label>`.
- A very light gray fails contrast. Darken it enough to read against the field background.
- Do not put instructions that the user still needs after typing, such as the required format, only in the placeholder.

**When to use:**

- A short example of the expected value, such as `name@example.com`, with a real label beside or above the field.

**Examples:**

```css
input::placeholder {
  color: #595959;
  font: inherit;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::placeholder
</details>

<details>
<summary>How do you implement focus styles for accessibility?</summary>

### How do you implement focus styles for accessibility?

**Definition:**

Show a visible focus indicator, preferably with `:focus-visible`, so keyboard users can see which control is active. Do not remove `outline` unless you replace it.

**How it works:**

#### :focus-visible

- `:focus-visible` matches when the browser decides a focus ring is needed, typically for keyboard focus rather than a mouse click.
- `outline` draws outside the border and does not change layout. `outline-offset` separates the ring from the control.
- A fallback for older browsers is `@supports not selector(:focus-visible)` together with `:focus`.
- The indicator needs contrast against the page. `:focus` alone is correct when every focus method, including mouse, should show the ring.

**When to use:**

- Every link, button, and field. A custom checkbox made with `appearance: none` still needs this ring.

**Examples:**

```css
button:focus-visible {
  outline: 3px solid #0b5fff;
  outline-offset: 3px;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:focus-visible
</details>

### Layout Debugging and Troubleshooting

<details>
<summary>How do you diagnose and solve layout issues?</summary>

### How do you diagnose and solve layout issues?

**Definition:**

Inspect the element in browser developer tools: the box model, the computed `display`, and which rules are crossed out. Then change one property and check the box again.

**How it works:**

#### What to read

- The box-model pane shows content, padding, border, and margin, including collapsed margins.
- The grid and flex overlays show tracks, gaps, and free space. A flex item that will not shrink often has the default `min-width: auto`, which is the content's minimum size.
- Crossed-out declarations lost the cascade. The Computed pane shows the value that won.
- `outline: 1px solid red` on a suspect element shows its border box without changing layout the way `border` can.

**When to use:**

- Before rewriting a layout. Name the broken box, the property you expected, and the computed value you actually got.

**Examples:**

```css
.suspect {
  outline: 1px solid red;
  min-width: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
</details>

<details>
<summary>Can you explain how to fix a layout that has unexpectedly broken columns?</summary>

### Can you explain how to fix a layout that has unexpectedly broken columns?

**Definition:**

A column breaks when an item's minimum size is larger than its track, or when the container wraps or overflows. Find that minimum, then let the item shrink or wrap on purpose.

**How it works:**

#### Usual causes

- A flex item's default minimum size is `min-width: auto` (the content minimum). Long text or an image then refuses to shrink and pushes the row. `min-width: 0` on the item allows it to shrink.
- `flex-wrap: nowrap` is the initial value, so extra items overflow instead of forming another row.
- In Grid, `1fr` tracks still respect a content minimum. `minmax(0, 1fr)` lets the column shrink below that minimum.
- An image without `max-width: 100%` can be wider than its column. `box-sizing: content-box` plus padding can make a percentage width overflow its parent.

**When to use:**

- After the devtools overlay shows which item is wider than its track. Change the minimum or the wrap, not every width on the page.

**Examples:**

```css
.col {
  min-width: 0;
}

.grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-grow
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
</details>

<details>
<summary>Describe approaches to resolving common browser compatibility issues in CSS.</summary>

### Describe approaches to resolving common browser compatibility issues in CSS.

**Definition:**

Check the feature on MDN's compatibility table, then gate it with `@supports` or provide a simpler layout that older browsers already understand.

**How it works:**

#### Progressive enhancement

- `@supports (display: grid)` applies Grid only where it exists. The rules before the query are the fallback.
- Prefer a standard property over a prefix. Add a prefix only when the compatibility table says a current browser still needs it.
- Logical properties, `aspect-ratio`, and `appearance: base-select` are examples of features whose support is not uniform. Read the table before you depend on them.
- A reset or `box-sizing: border-box` removes some cross-browser box differences. It does not add missing features.

**When to use:**

- When the layout must work in the browsers you support. Drop the fallback only after those browsers ship the feature.

**Examples:**

```css
.layout {
  display: block;
}

@supports (display: grid) {
  .layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@supports
</details>

<details>
<summary>How would you determine which styles are applied to an element when multiple styles collide?</summary>

### How would you determine which styles are applied to an element when multiple styles collide?

**Definition:**

The cascade picks one declaration per property. For declarations in the same origin and layer, the selector with the higher specificity wins. If specificity ties, the one that appears later wins.

**How it works:**

#### Order of the cascade

- Origin and importance come first. An author `!important` beats a normal author rule. User-agent, user, and author origins are separate.
- A selector's specificity is three columns: ID (`1-0-0`), then class, attribute, and pseudo-class (`0-1-0`), then type and pseudo-element (`0-0-1`). A `style` attribute beats any of those selectors.
- `*`, combinators, and `:where()` add no specificity. `:not()`, `:is()`, and `:has()` take the specificity of their most specific argument.
- Cascade layers (`@layer`) change which group of rules competes before specificity is compared. Developer tools strike through the declarations that lost.

**When to use:**

- When two rules set the same property. Read the winning rule in the Styles pane instead of adding another `!important`.

**Examples:**

```css
#title {
  color: green; /* 1-0-0 */
}

.page .card .title {
  color: yellow; /* 0-3-0, loses to the ID */
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
</details>

<details>
<summary>What common CSS problems might cause performance issues?</summary>

### What common CSS problems might cause performance issues?

**Definition:**

The expensive work is layout and paint on a large tree, not a long selector. Animating geometry, large shadows, and leaving `will-change` on many elements are the usual causes.

**How it works:**

#### What the browser repeats

- Changing `width`, `height`, `margin`, `top`, or font size can force layout (reflow) and then paint. Animating `transform` and `opacity` can stay on the compositor.
- A big `box-shadow`, `filter`, or blurred backdrop on a scrolling area repaints a large region.
- `will-change: transform, opacity` is a hint to prepare a layer. Leaving it on every element spends memory. Set it just before the change and remove it after.
- `@import` in a stylesheet delays the next CSS file. Render-blocking stylesheets in `<head>` delay first paint.

**When to use:**

- Profile a slow scroll or animation in the browser's performance tools before rewriting selectors. Fix the property that shows up in layout or paint.

**Examples:**

```css
.card {
  transition: transform 200ms ease-out, opacity 200ms ease-out;
}

.card:hover {
  transform: translateY(-2px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

### CSS 3D Transforms and Visual Effects

<details>
<summary>How do you utilize 3D transforms in CSS?</summary>

### How do you utilize 3D transforms in CSS?

**Definition:**

3D transforms move an element in three dimensions with functions such as `rotateX()`, `rotateY()`, `rotateZ()`, `translateZ()`, and `perspective()`. The parent needs `perspective` and, for children to share one 3D space, `transform-style: preserve-3d`.

**How it works:**

#### Depth

- `rotateY(180deg)` turns the element around its vertical axis. `translateZ()` moves it toward or away from the viewer.
- `perspective` on the parent is a length: the distance from the user to the z=0 plane. A smaller length makes the perspective stronger. `none` applies no perspective.
- `transform-style: preserve-3d` keeps children in the same 3D space. The initial `flat` flattens each child before the parent transforms.
- A `transform` other than `none` creates a stacking context and can become the containing block for a `position: fixed` descendant.

**When to use:**

- Cards, cubes, and other depth effects. For a flat move or scale, a 2D `translate()` or `scale()` is enough.

**Examples:**

```css
.scene {
  perspective: 600px;
}

.card {
  transform-style: preserve-3d;
  transform: rotateY(30deg);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transform
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>Explain how to use the perspective property in CSS.</summary>

### Explain how to use the perspective property in CSS.

**Definition:**

`perspective` sets the distance between the user and the z=0 plane so transformed children look three-dimensional. It applies to the children, not to the element itself. The initial value is `none`.

**How it works:**

#### Distance

- A length such as `800px` is that distance. Negative values are invalid. A value smaller than `1px` is clamped to `1px`.
- A large length is a mild effect. A small length is a strong effect. Children with a positive z get larger; children with a negative z get smaller.
- The vanishing point starts at the center. `perspective-origin` moves it.
- A value other than `none` creates a stacking context and a containing block for `position: fixed` descendants.
- The `perspective()` transform function is different: it is part of an element's own `transform` list.

**When to use:**

- Put `perspective` on the parent of the element you rotate or translate in z. Without it, `translateZ()` and `rotateY()` have no visible depth.

**Examples:**

```css
.scene {
  perspective: 800px;
  perspective-origin: center;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>What is backface visibility?</summary>

### What is backface visibility?

**Definition:**

`backface-visibility` sets whether the back of an element is shown when a 3D transform turns that face toward the user. The initial value is `visible`. `hidden` hides the back face.

**How it works:**

#### Front and back

- The back face is a mirror of the front. A 2D transform has no back face, so the property does nothing there.
- `hidden` makes the element invisible while it is turned away. That is what hides the reverse side of a card during `rotateY(180deg)`.
- The property applies to transformable elements and is not inherited. A child can set its own value.
- It does not create the 3D scene. The parent still needs `perspective` and `transform-style: preserve-3d`.

**When to use:**

- Any face that should disappear once it rotates past 90 degrees, such as the front of a flip card.

**Examples:**

```css
.face {
  backface-visibility: hidden;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backface-visibility
</details>

<details>
<summary>How do you create a flip card effect using CSS?</summary>

### How do you create a flip card effect using CSS?

**Definition:**

Put `perspective` on a scene, keep the card in 3D with `transform-style: preserve-3d`, and on hover set `transform: rotateY(180deg)`. Hide each face's back with `backface-visibility: hidden`, and pre-rotate the back face 180 degrees.

**How it works:**

#### Two faces

- Both faces are stacked with `position: absolute` so they occupy the same box.
- The front is unrotated. The back has `transform: rotateY(180deg)` so its front points the other way.
- `transition: transform` on the card animates the flip.
- Without `preserve-3d`, the browser flattens the faces and the back never turns into view.

**When to use:**

- A card with a short back, such as a detail that is still text. Do not put essential actions only on the back without a non-hover way to reach them.

**Examples:**

```css
.scene {
  perspective: 600px;
}

.card {
  position: relative;
  transform-style: preserve-3d;
  transition: transform 600ms;
}

.card:hover {
  transform: rotateY(180deg);
}

.face {
  position: absolute;
  inset: 0;
  backface-visibility: hidden;
}

.back {
  transform: rotateY(180deg);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/backface-visibility
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/perspective
</details>

<details>
<summary>What are some examples of visual effects you can achieve with CSS filters?</summary>

### What are some examples of visual effects you can achieve with CSS filters?

**Definition:**

`filter` applies one or more filter functions to an element, including its border and descendants. `grayscale()` removes color and `blur()` softens the image.

**How it works:**

#### Functions

- Functions are space-separated and applied in order: `filter: grayscale(0.4) blur(5px)`.
- `grayscale(0)` leaves the element unchanged. `grayscale(1)` or `grayscale(100%)` is fully gray.
- `blur()` takes a length. A larger radius spreads the blur further.
- `filter` other than `none` creates a stacking context. `backdrop-filter` is a different property: it filters what is behind the element.

**When to use:**

- A muted or softened image, or a hover state. Prefer an edited asset when the effect is permanent and large, because a filter is repainted with the element.

**Examples:**

```css
img.muted {
  filter: grayscale(0.4) blur(5px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/filter
</details>

### Layout and Design Principles

<details>
<summary>What are the principles of responsive web design?</summary>

### What are the principles of responsive web design?

**Definition:**

Responsive design adapts one document to the viewport: a fluid layout, flexible media, and media queries, with the viewport meta tag set to the device width.

**How it works:**

#### Three parts

- Fluid layout uses `fr`, flex, and percentages instead of a single fixed page width.
- Flexible media uses `max-width: 100%` so an image or video cannot exceed its column. `height: auto` keeps the ratio when only the width is capped.
- Media queries change columns and spacing when the viewport crosses a breakpoint. Mobile-first queries use `min-width`.
- `<meta name="viewport" content="width=device-width">` makes those queries see the device width rather than a wide virtual viewport.

**When to use:**

- Any page that is read on more than one screen size. Start from the narrow layout and add tracks as the viewport grows.

**Examples:**

```css
img {
  max-width: 100%;
  height: auto;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport
</details>

<details>
<summary>How would you approach designing a grid system with CSS?</summary>

### How would you approach designing a grid system with CSS?

**Definition:**

Define column tracks on a grid container, put gaps between them, and let components span tracks. CSS Grid does this with `grid-template-columns`, `gap`, and `grid-column`.

**How it works:**

#### Tracks

- `repeat(12, minmax(0, 1fr))` is a 12-column system. An item spans with `grid-column: span 4`.
- `repeat(auto-fit, minmax(16rem, 1fr))` builds as many columns as fit, so you may not need a breakpoint for the column count.
- `gap` is the gutter. It does not require margins on the items.
- Named areas (`header`, `main`, `aside`) document the page shell better than a pile of spans.

**When to use:**

- A shared page grid. Use Flexbox inside a grid area for a single row of actions. Do not nest a second 12-column grid unless that region really has columns.

**Examples:**

```css
.page {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 1rem;
}

.main {
  grid-column: span 8;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/grid-template-columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout
</details>

<details>
<summary>Describe the principle of "mobile-first" design.</summary>

### Describe the principle of "mobile-first" design.

**Definition:**

Mobile-first means the default CSS is the narrow layout. Wider layouts are added in `min-width` media queries, so each breakpoint enhances the previous one.

**How it works:**

#### Upward

- Base rules assume a small viewport: one column, stacked navigation, and type set in `rem`.
- `@media (min-width: …)` or `@media (width >= …)` adds columns and horizontal navigation only when there is room.
- The viewport meta tag must use `width=device-width`, or the phone still lays the page out on a wide virtual viewport.
- The opposite, desktop-first, starts wide and overrides with `max-width`. That override list grows as you support more small screens.

**When to use:**

- New pages. Content and the smallest layout are designed first, then the extra space is assigned.

**Examples:**

```css
.nav ul {
  display: grid;
  gap: 0.5rem;
}

@media (min-width: 40rem) {
  .nav ul {
    display: flex;
    gap: 1rem;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/width
</details>

<details>
<summary>What are some techniques for maintaining aspect ratios in responsive designs?</summary>

### What are some techniques for maintaining aspect ratios in responsive designs?

**Definition:**

Set `aspect-ratio` and leave at least one of `width` or `height` as an automatic size. The browser then computes the other side to keep the ratio as the container changes.

**How it works:**

#### Ratio

- `aspect-ratio: 16 / 9` is width divided by height. `aspect-ratio: 1` is the same as `1 / 1`.
- If both width and height are definite lengths, the ratio does not change the used size.
- For a replaced element such as an image, `aspect-ratio: auto 3 / 2` uses the given ratio until the resource loads, then the intrinsic ratio.
- `max-width: 100%` with `height: auto` lets an image shrink with its column and keep its own ratio. That is separate from `aspect-ratio` on a box you size yourself.

**When to use:**

- Video frames, posters, and cards that must stay 16:9 or square while the column width changes.

**Examples:**

```css
.frame {
  width: 100%;
  height: auto;
  aspect-ratio: 16 / 9;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/aspect-ratio
</details>

<details>
<summary>How do you ensure your design is accessible and inclusive?</summary>

### How do you ensure your design is accessible and inclusive?

**Definition:**

Keep text resizable, contrast readable, and focus visible. Do not use color as the only way to tell states apart, and let people reduce motion.

**How it works:**

#### Checks that CSS controls

- Set type in `rem` or another font-relative unit so it follows the user's root font size. Avoid locking text to `px`.
- `:focus-visible` with a contrasting `outline` shows keyboard focus. Do not set `outline: none` without a replacement.
- `@media (prefers-reduced-motion: reduce)` shortens or removes animation and smooth scrolling.
- A state such as an error needs more than color: text, an icon, or a border.
- `content` on `::before` that repeats real text is announced twice. Decorative generated content should be empty or marked so it is not read as content.

**When to use:**

- On every interactive control and on any motion. Check a page with keyboard only and with a larger default font size.

**Examples:**

```css
button:focus-visible {
  outline: 3px solid #0b5fff;
  outline-offset: 3px;
}

@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms;
    transition-duration: 0.01ms;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:focus-visible
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion
</details>

### Print Stylesheets and CSS for Print

<details>
<summary>How do you create a print stylesheet for your web page?</summary>

### How do you create a print stylesheet for your web page?

**Definition:**

Put print rules in `@media print`, or link a separate file with `media="print"`. Those rules apply when the document is printed or shown in print preview.

**How it works:**

#### Where the rules live

- `@media print { ... }` in the main stylesheet overrides screen rules only for print. `screen` rules still apply first, then print rules that match.
- `<link rel="stylesheet" href="print.css" media="print">` loads a file that the browser uses for print.
- Inside the query, hide navigation, expand links with `attr(href)`, and set a readable measure. Lengths in `cm`, `mm`, and `in` match physical units on a printer.

**When to use:**

- Pages people print: articles, receipts, tickets. Keep the screen stylesheet as the source of content styles and override only what print needs.

**Examples:**

```css
@media print {
  nav {
    display: none;
  }

  a[href]::after {
    content: " (" attr(href) ")";
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
</details>

<details>
<summary>What are the common best practices when creating CSS for print?</summary>

### What are the common best practices when creating CSS for print?

**Definition:**

Print CSS should show the content, hide controls that do nothing on paper, and avoid splitting a figure or heading from the text that follows it.

**How it works:**

#### Paper

- `display: none` on navigation, ads, and buttons that only work on screen.
- Dark text on a white background. Browsers may drop background images and background colors to save ink.
- `a[href]::after { content: " (" attr(href) ")"; }` prints the URL, because the underline is not clickable on paper.
- `break-inside: avoid` on a figure keeps the image with its caption. `break-after: avoid` on a heading keeps it with the next paragraph.
- A `position: fixed` element is placed in the same position on every printed page, so a fixed header can cover content on each page.

**When to use:**

- Any `@media print` block. Check a multi-page article, not only a one-screen page.

**Examples:**

```css
@media print {
  figure {
    break-inside: avoid;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-inside
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>Explain the use of page-break in CSS.</summary>

### Explain the use of page-break in CSS.

**Definition:**

`page-break-before`, `page-break-after`, and `page-break-inside` are legacy aliases of `break-before`, `break-after`, and `break-inside`. They control breaks before, after, or inside a box when the document is fragmented into pages or columns.

**How it works:**

#### Alias

- `page-break-before: always` is treated as `break-before: page`. The old `always` meant a page break, not a column break.
- `break-before: page` forces a page break before the box. `avoid` and `avoid-page` try not to break there. `auto` allows a break but does not force one.
- A possible break looks at three values: `break-after` of the previous box, `break-before` of the next, and `break-inside` of the container. A forced break wins. An avoid value blocks that kind of break.
- `break-before: column` forces a column break inside a multi-column container. The same properties apply to pages and columns.

**When to use:**

- Prefer `break-before` and `break-inside` in new CSS. Keep `page-break-*` only when you are matching an older stylesheet.

**Examples:**

```css
h2 {
  break-before: page;
}

figure {
  break-inside: avoid;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-before
</details>

<details>
<summary>How can you test the print version of your web page?</summary>

### How can you test the print version of your web page?

**Definition:**

Open the browser's print preview, and emulate the `print` media type in developer tools so `@media print` applies while you are still looking at the page.

**How it works:**

#### Two views

- Print preview paginates the document and shows margins, headers, and page breaks. `window.print()` opens that preview.
- The rendering or emulation panel can set the CSS media type to `print`. The layout updates in the page, which makes it easier to inspect a rule than in the preview dialog.
- Check more than one page. A heading at the bottom of a page, a split image, and a fixed bar that repeats are easy to miss on page one.
- Disable background graphics in the print dialog if you need to see what a default print does with `background`.

**When to use:**

- After any change to `@media print`, `break-before`, or `break-inside`.

**Examples:**

```css
@media print {
  body {
    margin: 1.5cm;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/break-before
</details>

<details>
<summary>What are some challenges when designing CSS for print?</summary>

### What are some challenges when designing CSS for print?

**Definition:**

Print fragments the page, drops a lot of screen styling, and has no hover. Fixed bars, background colors, and viewport units behave differently than they do on screen.

**How it works:**

#### What changes

- A browser may omit `background-color` and `background-image` unless the user prints backgrounds.
- `position: fixed` repeats on every page. A sticky header does not stick the way it does while scrolling.
- Viewport units are invalid inside `@page`. Page size and margin belong to the page box, not the screen viewport.
- Links, hover states, and collapsed navigation do not work on paper. URLs have to be printed as text.
- A break can split a paragraph, table row, or image. `break-inside: avoid` reduces that, but a box taller than one page still has to split.

**When to use:**

- Plan print as its own layout. Do not assume the screen design will paginate cleanly.

**Examples:**

```css
@page {
  margin: 1.5cm;
}

@media print {
  header {
    position: static;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@page
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

### Pseudo-Elements and Pseudo-Classes

<details>
<summary>Describe the purpose of the ::before and ::after pseudo-elements.</summary>

### Describe the purpose of the ::before and ::after pseudo-elements.

**Definition:**

`::before` and `::after` generate a box as the first or last child of an element. They render only when `content` is set to something other than `normal` or `none`.

**How it works:**

#### Generated boxes

- `content` can be a string, `attr()`, a counter, or an empty string `""` when you only need a box to style.
- The pseudo-element can be positioned, sized, and given a background. It is not in the HTML, so it is not a separate element in the DOM.
- A string in `content` can be read by assistive technology. Use `content: ""` for a purely visual marker.
- Their specificity is that of a pseudo-element: one type column (`0-0-1`), plus the rest of the selector.

**When to use:**

- Icons, underlines, quotation marks, and printed URLs. Do not put text the user must read, or a control, only in `content`.

**Examples:**

```css
.required::after {
  content: " *";
}

a[href]::after {
  content: " (" attr(href) ")";
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::before
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::after
</details>

<details>
<summary>What is the :hover pseudo-class and how does it work?</summary>

### What is the :hover pseudo-class and how does it work?

**Definition:**

`:hover` matches an element while the user designates it with a pointing device. It also matches ancestors of that element.

**How it works:**

#### Pointer

- Styles under `:hover` apply for as long as the pointer is over the element. Moving away removes them.
- A transition on the same property animates the change into and out of that state.
- On a touch screen, hover may stick after a tap or may not occur. Do not hide a required action behind hover only.
- `:hover` is a pseudo-class, so it adds `0-1-0` to specificity. It is not a pseudo-element.

**When to use:**

- A hint: underline, color, or a slight transform. Pair it with `:focus-visible` so keyboard users get an equivalent cue.

**Examples:**

```css
a:hover {
  text-decoration: underline;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:hover
</details>

<details>
<summary>Explain the difference between :nth-child and :nth-of-type.</summary>

### Explain the difference between :nth-child and :nth-of-type.

**Definition:**

`:nth-child()` counts every sibling element. `:nth-of-type()` counts only siblings of the same element type. A match still has to fit the rest of the selector.

**How it works:**

#### Counting

- Positions start at 1. `odd` and `even` are the odd and even positions. `An+B` is a step and an offset, so `2n+1` is the same set as `odd`.
- `span:nth-child(2n+1)` counts every child. If child 3 is an `em`, it is counted but not selected, and the spans shift.
- `span:nth-of-type(2n+1)` skips the `em` and counts only `span` elements.
- `:nth-child(1)` matches the same element as `:first-child`. The `of` syntax, `:nth-child(even of .noted)`, counts only siblings that match that selector.

**When to use:**

- `:nth-child` when the position among all children matters. `:nth-of-type` when the markup mixes tags and you want the nth element of one tag.

**Examples:**

```css
span:nth-child(2n + 1) {
  background: tomato;
}

span:nth-of-type(2n + 1) {
  background: tomato;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:nth-child
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:nth-of-type
</details>

<details>
<summary>How do :first-child and :last-child work?</summary>

### How do :first-child and :last-child work?

**Definition:**

`:first-child` matches an element that is the first child of its parent. `:last-child` matches an element that is the last child. Both count every element sibling, not only the same tag.

**How it works:**

#### Position in the parent

- `p:first-child` matches a `p` only when that `p` is the parent's first element child. If a heading comes first, the paragraph does not match.
- `p:last-child` matches a `p` only when nothing follows it inside the parent.
- `:first-child` is the same match as `:nth-child(1)`. `:last-child` is the same match as `:nth-last-child(1)`.
- `:first-of-type` and `:last-of-type` are the type-restricted versions. They ignore siblings of other tags.

**When to use:**

- A margin on every item except the first, or a border on every item except the last. If a wrapper adds an unexpected first node, the pseudo-class stops matching.

**Examples:**

```css
li:first-child {
  margin-top: 0;
}

li:last-child {
  border-bottom: 0;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:first-child
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:last-child
</details>

<details>
<summary>What is the :not pseudo-class and when would you use it?</summary>

### What is the :not pseudo-class and when would you use it?

**Definition:**

`:not()` matches elements that do not match its selector list. The pseudo-class itself adds no specificity. The most specific selector in the argument does.

**How it works:**

#### Negation

- `p:not(.fancy)` matches every `p` without that class.
- `:not(.foo, .bar)` matches elements that are neither `.foo` nor `.bar`. It is the same as `:not(.foo):not(.bar)`.
- An invalid selector inside `:not()` invalidates the whole rule. Wrapping the list in `:is()` makes the list forgiving: `:not(:is(.foo, :unknown))` still matches elements that are not `.foo`.
- `:not(.foo)` matches every element that is not `.foo`, including `html` and `body`. With a descendant combinator, a link inside a table can still match `body :not(table) a` through another ancestor. Write `a:not(table a)` when you mean links that are not inside a table.

**When to use:**

- A small exception, such as every button except the primary one. Do not use it to restate a long list of everything you do not want.

**Examples:**

```css
p:not(.fancy) {
  color: green;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:not
</details>

### CSS Performance Optimization

<details>
<summary>How does the complexity of selectors impact performance?</summary>

### How does the complexity of selectors impact performance?

**Definition:**

A long selector is rarely what makes a page slow. Style matching is cheaper than layout and paint. Complexity still matters when a selector matches a huge number of elements or when it is so specific that updates invalidate more of the tree than you expect.

**How it works:**

#### Matching

- The browser builds a CSSOM and matches rules to elements. A universal selector, or a selector that depends on a common ancestor, can match far more nodes than a single class.
- Specificity changes which declaration wins. It is not a timing score. An ID is not meaningfully faster than a class in a current browser.
- The expensive follow-up is invalidation: changing a class on a parent can restyle every descendant that depends on that ancestor.
- Deep nesting from a preprocessor compiles to long selectors and raises specificity. That cost shows up as maintenance and broad invalidation, not as a few extra characters in the selector.

**When to use:**

- Prefer one class per component. Profile style recalculation only after layout and paint are already cheap.

**Examples:**

```css
.card-title {
  font-weight: 700;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity
- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

<details>
<summary>What strategies would you use to optimize the rendering path of CSS?</summary>

### What strategies would you use to optimize the rendering path of CSS?

**Definition:**

The critical rendering path turns HTML into a DOM, CSS into a CSSOM, then builds a render tree, lays it out, paints, and composites. CSS that blocks that path should be small and in the document early.

**How it works:**

#### Steps

- A stylesheet in `<head>` is render-blocking: the browser waits for it before first paint so it does not draw unstyled content and then restyle it.
- `@import` inside a stylesheet discovers the next file only after the first file is parsed, so it adds a serial delay. Use `<link>` instead.
- Unused rules still have to be parsed into the CSSOM. Ship the CSS the first view needs, and load the rest later.
- After the CSSOM exists, layout and paint dominate. Animating `transform` and `opacity` avoids repeating layout.

**When to use:**

- When first paint is slow or a stylesheet chain shows up in the network waterfall. Measure before splitting files for their own sake.

**Examples:**

```html
<link rel="stylesheet" href="styles.css" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
</details>

<details>
<summary>How do you minimize CSS repaints and reflows?</summary>

### How do you minimize CSS repaints and reflows?

**Definition:**

A reflow (layout) recomputes geometry. A repaint fills in pixels after geometry is known. A composite can move an existing layer. Animate `transform` and `opacity` when you want to avoid layout.

**How it works:**

#### What changes

- `width`, `height`, `margin`, `padding`, `top`, `left`, and `font-size` can force layout, then paint.
- `color` and `background-color` can repaint without a full layout.
- `transform` and `opacity` can be handled on the compositor when the element is on its own layer.
- `will-change: transform, opacity` asks the browser to prepare that layer. Add it just before the animation and set it back to `auto` after. Leaving it on many elements costs memory.
- Reading layout in a loop, such as `offsetWidth` between style writes, forces synchronous layout. Batch the reads, then the writes.

**When to use:**

- Hover motion, carousels, and sticky bars. Confirm in the performance panel that the work is composite, not layout.

**Examples:**

```css
.card {
  transition: transform 200ms ease-out;
}

.card:hover {
  transform: translateY(-2px);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change
- https://developer.mozilla.org/en-US/docs/Glossary/Reflow
</details>

<details>
<summary>What are some of the best practices for writing efficient CSS?</summary>

### What are some of the best practices for writing efficient CSS?

**Definition:**

Write the smallest set of rules that match the elements you mean. Prefer a class, custom properties for repeated values, and properties that do not force layout when they animate.

**How it works:**

#### Habits

- One class for a component beats a long chain of tags and ancestors. Lower specificity is easier to override without `!important`.
- Custom properties hold a color or a space once. Repeating the same hex in hundreds of rules makes later changes a search.
- Do not animate layout properties for decoration. Use `transform` and `opacity`.
- Ship only the CSS the page uses. A render-blocking file full of unused rules still has to be downloaded and parsed.
- Stylelint can reject unknown properties and discouraged patterns before they ship.

**When to use:**

- In review, ask whether a new selector matches more elements than the component, and whether a new animation touches layout.

**Examples:**

```css
:root {
  --space: 1rem;
}

.card {
  margin-block: var(--space);
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
- https://stylelint.io/
</details>

<details>
<summary>How do you optimize CSS for critical rendering path and fast load times?</summary>

### How do you optimize CSS for critical rendering path and fast load times?

**Definition:**

Make the CSS required for the first view small, load it from `<head>` without `@import`, and defer the rest so it does not block first paint.

**How it works:**

#### First paint

- The browser cannot paint reliably until render-blocking CSS is downloaded and turned into a CSSOM.
- Inline the small set of rules for the top of the page if a network round trip dominates. Keep that block short.
- Load the full stylesheet with `<link>`. A `media` value that does not match, such as `media="print"`, lets the browser treat that file as non-blocking for the screen.
- Drop unused rules and avoid a second request hidden in `@import`.
- After load, keep interactions on `transform` and `opacity` so the first interaction does not repeat layout.

**When to use:**

- When the network waterfall shows CSS delaying first paint. Measure the stylesheet's transfer size and the time to first paint before and after.

**Examples:**

```html
<link rel="stylesheet" href="styles.css" />
<link rel="stylesheet" href="print.css" media="print" />
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
</details>

### Scalable and Maintainable CSS

<details>
<summary>What is Object-Oriented CSS (OOCSS)?</summary>

### What is Object-Oriented CSS (OOCSS)?

**Definition:**

OOCSS separates a reusable structure from its skin, and a container from the content inside it, so the same object can be styled in more than one place.

**How it works:**

#### Two separations

- Structure is the layout object: a media block with a figure and a body. Skin is the visual theme: color, border, and shadow. You change the skin class without copying the layout rules.
- A container should not style its content through a long descendant selector. The content object carries its own class, so it looks the same in a sidebar and in a card.
- The result is more classes in the HTML and fewer rules that depend on where an element sits in the tree.

**When to use:**

- A design system of repeated objects, such as media blocks and buttons. BEM is one naming scheme that carries the same idea.

**Examples:**

```css
.media {
  display: flex;
  gap: 1rem;
}

.media-skin {
  background: #f4f4f4;
  border: 1px solid #ccc;
}
```

**Sources:**

- https://github.com/stubbornella/oocss/wiki
</details>

<details>
<summary>Can you explain the BEM (Block Element Modifier) naming convention?</summary>

### Can you explain the BEM (Block Element Modifier) naming convention?

**Definition:**

BEM names a component as a block, its parts as elements, and its variants as modifiers: `block__element--modifier`.

**How it works:**

#### Names

- A block is a standalone component, such as `menu`. An element belongs to that block and is written `menu__item`. A modifier is a flag, written `menu__item--active` or `menu--wide`.
- The class is the selector. You do not need `.menu .item` , so specificity stays flat and the part does not depend on nesting.
- An element is not reused outside its block. A shared piece becomes its own block.
- The double underscore and double hyphen are the common separators. The methodology cares that block, element, and modifier stay readable, not about a particular preprocessor.

**When to use:**

- Hand-written component CSS in a large codebase. Utility frameworks such as Tailwind use a different naming model.

**Examples:**

```css
.menu {
  display: flex;
  gap: 0.5rem;
}

.menu__item--active {
  font-weight: 700;
}
```

**Sources:**

- https://github.com/getbem/getbem.github.io/blob/master/src/pages/naming.mdx
</details>

<details>
<summary>What are the advantages of using a CSS framework like Bootstrap or TailwindCSS?</summary>

### What are the advantages of using a CSS framework like Bootstrap or TailwindCSS?

**Definition:**

A framework gives a shared, documented set of styles so a team does not invent spacing, color, and layout for every screen. Bootstrap ships components. Tailwind ships utilities that you compose in the markup.

**How it works:**

#### Two styles of framework

- Bootstrap's stylesheet includes a grid, forms, and components with stable class names. You get a consistent UI quickly, and you override it when the design diverges.
- Tailwind scans your templates for class names and generates only the utilities you used, such as spacing, color, and flex. There is no separate component stylesheet unless you add one.
- Both reduce one-off CSS and make review easier, because the pattern is named. Both can fight a custom design if you keep overriding the framework instead of using it.

**When to use:**

- Bootstrap when you want ready components. Tailwind when you want to design in the markup and keep the generated CSS limited to the classes you wrote. Skip both when the product already has a design system.

**Examples:**

```html
<button class="btn btn-primary">Save</button>
```

**Sources:**

- https://getbootstrap.com/docs/5.3/getting-started/introduction/
- https://tailwindcss.com/docs/styling-with-utility-classes
</details>

<details>
<summary>How do you ensure CSS scalability and maintainability in a large project?</summary>

### How do you ensure CSS scalability and maintainability in a large project?

**Definition:**

Keep selectors flat, put shared values in custom properties, and order rules so later layers override earlier ones on purpose. Lint the result.

**How it works:**

#### Levers

- A class per component, or BEM, avoids selectors that break when the HTML moves.
- Custom properties are the tokens. A theme changes the token, not every rule.
- `@layer` sets the order of groups, such as reset, components, and utilities, before specificity is compared.
- CSS Modules or a naming convention stops two files from claiming the same `.button`.
- Stylelint and formatting catch unknown properties and drift. Delete unused CSS instead of leaving it "in case".

**When to use:**

- As soon as more than one person edits the stylesheets. A small site can stay as one file until the selectors start to fight.

**Examples:**

```css
@layer reset, components, utilities;

@layer components {
  .button {
    color: var(--text);
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@layer
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
</details>

<details>
<summary>What are some CSS architecture approaches for large-scale applications?</summary>

### What are some CSS architecture approaches for large-scale applications?

**Definition:**

Large apps pick one way to scope and order CSS. Common choices are BEM, CSS Modules, utility classes, and cascade layers. They can be combined. They should not all invent a second naming scheme in the same file.

**How it works:**

#### Approaches

- BEM scopes by name: `block__element--modifier`. The stylesheet stays global, and the class name carries the boundary.
- CSS Modules scope by file. The build rewrites the class, and the component imports the generated name.
- Utility CSS, as in Tailwind, puts single-purpose classes in the markup and generates the stylesheet from those names.
- `@layer` orders reset, components, and utilities so a utility does not have to win by specificity.
- OOCSS's split of structure and skin is the idea behind reusable objects in all of these.

**When to use:**

- BEM or CSS Modules for a component library with its own stylesheets. Utilities when product UI is composed in templates. Add `@layer` when several sources of CSS must override each other predictably.

**Examples:**

```css
@layer components {
  .menu__item--active {
    font-weight: 700;
  }
}
```

**Sources:**

- https://github.com/getbem/getbem.github.io/blob/master/src/pages/naming.mdx
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@layer
</details>

### CSS Special Cases and Practical Scenarios

<details>
<summary>How would you create a CSS-only hamburger menu toggle?</summary>

### How would you create a CSS-only hamburger menu toggle?

**Definition:**

Use a checkbox as the toggle and a `<label>` as the button. The `:checked` state shows the navigation with a sibling combinator. No script is required, and the control stays keyboard accessible.

**How it works:**

#### Checkbox and sibling

- The input and the nav are siblings. `input:checked ~ nav` matches the nav only while the box is checked.
- The label's `for` points at the input's `id`, so clicking or activating the label toggles the box.
- Do not `display: none` the checkbox. A visually hidden control can still be focused. Give it a visible `:focus-visible` style on the label, for example with `:focus-within` on a wrapper.
- The menu button needs an accessible name. The label text is that name.

**When to use:**

- A simple disclosure on a small screen. For a modal dialog or a menu that must trap focus, use a `<dialog>` or a scripted control instead of a checkbox.

**Examples:**

```css
.nav-toggle:checked ~ nav {
  display: block;
}

nav {
  display: none;
}
```

```html
<input class="nav-toggle" id="nav-toggle" type="checkbox" />
<label for="nav-toggle">Menu</label>
<nav>…</nav>
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/:checked
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/Subsequent-sibling_combinator
</details>

<details>
<summary>Explain how to make a pure CSS carousel or slider.</summary>

### Explain how to make a pure CSS carousel or slider.

**Definition:**

Lay slides in a row inside a horizontal scroll container and snap the scroll position to each slide with `scroll-snap-type` and `scroll-snap-align`.

**How it works:**

#### Snap

- The track is `display: flex` with `overflow-x: auto`. Each slide is `flex: 0 0 100%` so it is one viewport of the track wide.
- `scroll-snap-type: x mandatory` on the track forces the scroll position onto a snap point when scrolling ends. `proximity` snaps only when the position is already close.
- `scroll-snap-align: start` on each slide marks that snap point.
- The browser chooses the scroll physics. This does not add previous and next buttons. Those are extra elements, such as links to each slide's `id`.

**When to use:**

- A row of panels the user can swipe or scroll. Add buttons and a pause control if the carousel moves on its own. An automatic animation is a poor default.

**Examples:**

```css
.track {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
}

.slide {
  flex: 0 0 100%;
  scroll-snap-align: start;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/scroll-snap-type
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/scroll-snap-align
</details>

<details>
<summary>How do you create a masonry layout with CSS?</summary>

### How do you create a masonry layout with CSS?

**Definition:**

A masonry-style layout packs each item into the column with the most free space. CSS calls the experimental version grid lanes: `display: grid-lanes` with `grid-template-columns`. Support is limited.

**How it works:**

#### Grid lanes

- One axis is a normal grid. The other axis stacks items into the lane that has the most room, so row tracks are not strict.
- `grid-template-columns: repeat(auto-fill, minmax(120px, 1fr))` sets the column tracks. `gap` separates items.
- An item can span tracks with `grid-column-end: span 2`. Items with a definite column placement are placed before the packing algorithm runs.
- Multi-column layout (`columns`) is widely available but fills each column from top to bottom. That is a different order from grid lanes.

**When to use:**

- Only where you have checked that `grid-lanes` is supported, or behind `@supports (display: grid-lanes)`. Otherwise use a normal grid with equal rows, or columns if column-major order is acceptable.

**Examples:**

```css
.grid {
  display: grid-lanes;
  gap: 10px;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Grid_lanes
</details>

<details>
<summary>How would you apply CSS to a web application that should print as well as display on screen?</summary>

### How would you apply CSS to a web application that should print as well as display on screen?

**Definition:**

Write the shared rules once, then override what paper cannot use inside `@media print`. Screen-only layout stays in `@media screen` or in the base rules.

**How it works:**

#### One document, two media

- Rules outside a media query apply to both screen and print. Color, type, and the article column belong there.
- `@media screen` holds hover effects, sticky bars, and multi-column app chrome.
- `@media print` hides navigation, sets physical margins, and avoids breaks inside figures. A separate `<link media="print">` is the same split in another file.
- `position: fixed` repeats on every printed page, so set those bars back to `position: static` in the print query.

**When to use:**

- Any app with a page the user is expected to print or save as PDF. Preview both media types after a layout change.

**Examples:**

```css
article {
  max-width: 65ch;
}

@media print {
  nav {
    display: none;
  }
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media
- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position
</details>

<details>
<summary>How do you create a seamless waterfall flow of elements in a responsive design?</summary>

### How do you create a seamless waterfall flow of elements in a responsive design?

**Definition:**

For a widely supported column flow, set `columns` to an ideal column width. The browser adds columns as the container grows. `break-inside: avoid` keeps each item in one piece.

**How it works:**

#### Column flow

- `columns: 16rem` sets the ideal column width. The count follows the container, so a narrow viewport gets one column and a wide one gets several.
- Content flows down a column, then into the next column. That order is not the same as grid lanes, which place each item into the shortest column from left to right.
- `break-inside: avoid` on each child stops a card from splitting across columns. A margin or `column-gap` separates them.
- `display: grid-lanes` packs by the shortest column, but it is experimental and has limited support.

**When to use:**

- A responsive stream of notes or cards where top-to-bottom column order is acceptable. Use grid lanes only behind `@supports` when you need shortest-column packing.

**Examples:**

```css
.flow {
  columns: 16rem;
  column-gap: 1rem;
}

.flow > * {
  break-inside: avoid;
  margin-bottom: 1rem;
}
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/columns
- https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Grid_lanes
</details>

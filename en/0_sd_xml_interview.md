# XML — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [XML — Interview](./0_sd_xml_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — XML](https://devinterview.io/questions/software-architecture-and-system-design/xml-interview-questions/).

### XML Fundamentals

<details>
<summary>What is XML and what does it stand for?</summary>

### What is XML and what does it stand for?

**Definition:**

XML stands for Extensible Markup Language. It is a text format for trees of named elements, so two systems can exchange a document whose structure is explicit in the tags.

**How it works:**

- An element has a name, optional attributes, and content that is text, child elements, or both.
- A document has one root element. It is well-formed when the tags nest and close correctly, and valid when it also matches a DTD or a schema.
- Namespaces keep the same local name from colliding when vocabularies mix.
- People use it for data exchange, configuration, and document markup. XPath selects nodes, XSLT transforms them, XQuery queries them, and XML Schema describes them.
- It describes structure. It does not, by itself, present a page or run a script. HTML does that job with a fixed vocabulary.
- The format is verbose compared with JSON. It pays off when you need mixed content, namespaces, or a strict schema.

**Examples:**

```xml
<order id="17"><sku>A1</sku><qty>2</qty></order>
```

The tags say what each value is. A reader does not have to guess that the second field is a quantity.
</details>

<details>
<summary>What are the key features and benefits of XML?</summary>

### What are the key features and benefits of XML?

**Definition:**

XML is a self-describing tree in plain text. The tags travel with the data, so a receiver can see the structure without a separate out-of-band layout.

**How it works:**

- Elements nest, so a value can contain structure, not only a flat list of fields.
- It is text, usually Unicode, so it crosses platforms and languages without a binary layout.
- You invent element names (it is extensible) and can constrain them later with a schema.
- Comments and processing instructions can ride in the document without becoming data.
- A schema gives validation: types, which children are required, and how many.
- Namespaces let you mix vocabularies. Versioning is mostly “add an optional element” or a new namespace, so old readers can skip what they do not know if they were written to.
- The benefit is interoperability and a standard toolchain. The cost is size and the need to escape `<` and `&`.

**Examples:**

A supplier adds `<note>` inside `<item>` without breaking a reader that only looks at `<sku>` and `<qty>`, as long as the schema says the note is optional.
</details>

<details>
<summary>Explain the difference between XML and HTML.</summary>

### Explain the difference between XML and HTML.

**Definition:**

XML is a toolkit for defining markup languages. HTML is one markup language, aimed at documents in a browser, with a vocabulary the browser already understands.

**How it works:**

- HTML’s purpose is presentation and interaction: headings, forms, scripts. XML’s purpose is to carry a structure you named.
- HTML tags are fixed (`p`, `a`). XML tags are yours (`sku`, `qty`).
- XML must be well-formed: one root, closed tags, quoted attributes. HTML parsers repair a lot of broken markup. XHTML is HTML written under XML’s stricter rules.
- XML attributes and elements are data. HTML attributes often feed rendering and behavior (`href`, `onclick`).
- HTML documents are interactive because the browser runs script and applies CSS. An XML document does nothing until a program interprets it.
- Both can look similar as tags. Mixing them up means either a browser that will not render your `<order>`, or a strict parser that rejects an unclosed `<p>`.

**Examples:**

```html
<p>Qty: 2</p>
```

is HTML a browser shows. `<qty>2</qty>` is XML a program reads. The browser does not know that `qty` is a number. Your schema might.
</details>

<details>
<summary>What is an XML document and what are its main components?</summary>

### What is an XML document and what are its main components?

**Definition:**

An XML document is one tree: an optional declaration, optional comments and processing instructions, and exactly one root element that contains the rest.

**How it works:**

- The prolog may start with `<?xml version="1.0" encoding="UTF-8"?>`. It is not an element.
- The root element wraps every other element. Two top-level elements are not a well-formed document.
- Inside the root you get elements, text, attributes on those elements, comments, and processing instructions.
- Well-formed means the grammar of XML itself: matching tags, quoted attributes, legal characters.
- Valid means a DTD or schema additionally accepts this tree. A well-formed document can still be invalid.
- Entity references such as `&amp;` are resolved into characters before you see the text value.

**Examples:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
  <book id="b1">XML</book>
</catalog>
```

`catalog` is the root. `book` is a child. `id` is an attribute. The declaration is metadata for the parser, not a node in the element tree in the same way.
</details>

<details>
<summary>Describe the structure of an XML element.</summary>

### Describe the structure of an XML element.

**Definition:**

An element is a start tag, content, and an end tag with the same name. The start tag may carry attributes. Content is text, child elements, or empty.

**How it works:**

- `<book id="b1">Guide</book>`: name `book`, attribute `id`, text content `Guide`.
- Names are case-sensitive. `<Book>` and `<book>` are different.
- Tags must nest. You close the inner element before the outer one. Overlap is not allowed.
- Attribute values are quoted. An attribute name appears at most once on a tag.
- An empty element can be written `<flag></flag>` or `<flag/>`. Both have no children. Attributes are allowed on either form.
- Character data that must include raw `<` can sit in a CDATA section. Otherwise `<` and `&` are escaped.

**Examples:**

```xml
<book id="b1">
  <title>Guide</title>
</book>
```

`book` contains one child element, not the text `Guide` directly. A JSON object with `"title": "Guide"` is a different tree with the same facts.
</details>

<details>
<summary>What is the purpose of XML attributes?</summary>

### What is the purpose of XML attributes?

**Definition:**

Attributes hang small named values on an element, usually metadata about that element rather than a structure of their own.

**How it works:**

- They appear in the start tag: `id="b1"`, `lang="en"`. The value is always text, always quoted.
- A given name occurs once. If you need many values or nested values, use child elements.
- They suit identifiers, types, and flags: compact, unordered, and not repeated.
- Elements suit content that has its own children, repeats, or mixed text. An attribute cannot contain an element.
- Whitespace in an attribute is normalized more aggressively than in element text. Long prose belongs in an element.
- Whether something is an attribute or a child is a design choice in your vocabulary, not a rule XML enforces beyond the syntax.

**Examples:**

```xml
<book id="b1" lang="en">
  <title>Guide</title>
</book>
```

`id` and `lang` describe the book. The title is content, so it is an element and could later gain its own children.
</details>

<details>
<summary>Explain the concept of XML namespaces.</summary>

### Explain the concept of XML namespaces.

**Definition:**

A namespace keeps element and attribute names from colliding by tying a local name to a URI you choose. Two `id` names in different namespaces are different names.

**How it works:**

- You bind a prefix with `xmlns:ord="http://example.com/order"` and write `ord:id`.
- `xmlns="..."` sets the default namespace for that element and its children. Unprefixed elements use it. Unprefixed attributes do not. They stay in no namespace unless marked.
- The URI is an identifier, not a document the parser must download.
- Without namespaces, a combined document cannot tell your `title` from another vocabulary’s `title`.
- Schema validation and XPath need the same URIs. A prefix can change. The URI is what must match.
- Default namespaces are a common source of bugs: a child looks unprefixed and is still in the default namespace.

**Examples:**

```xml
<root xmlns:a="http://example.com/a" xmlns:b="http://example.com/b">
  <a:item id="1"/>
  <b:item id="2"/>
</root>
```

Both local names are `item`. They are not the same element. Drop the prefixes and a single schema cannot tell them apart.
</details>

<details>
<summary>What is the difference between well-formed and valid XML?</summary>

### What is the difference between well-formed and valid XML?

**Definition:**

Well-formed XML obeys the syntax rules of XML. Valid XML is well-formed and also satisfies a DTD or a schema you named.

**How it works:**

- Well-formed: one root, every start tag has an end tag, elements nest properly, attribute values are quoted, names are legal, and `&` and `<` in text are escaped.
- A parser rejects input that is not well-formed. There is no document to hand you.
- Validity adds a vocabulary: which elements may appear, in what order, how many times, which attributes are required, and what the text should look like (a date, an integer).
- A document can be well-formed and invalid: `<qty>lots</qty>` is fine syntax and fails a schema that says `qty` is an integer.
- Well-formedness is checked by every XML parser. Validity is checked only when you ask for DTD or schema validation.
- Do not skip well-formedness and “be liberal.” XML parsers are strict on purpose.

**Examples:**

`<a><b></a></b>` is not well-formed (the tags cross). `<qty>lots</qty>` inside a single root is well-formed. Against a schema where `qty` is `xs:integer`, it is invalid.
</details>

<details>
<summary>What is the role of XML declaration in an XML document?</summary>

### What is the role of XML declaration in an XML document?

**Definition:**

The XML declaration tells the parser which XML version and which character encoding the bytes use. It is optional, and it is not an element.

**How it works:**

- The form is `<?xml version="1.0" encoding="UTF-8" standalone="yes"?>`. It must be the very start of the file if you include it. No comment and no whitespace before it.
- `version` is `1.0` or `1.1`. 1.0 is what almost everything still uses.
- `encoding` names how to turn bytes into characters. If you omit the declaration, the parser falls back to UTF-8 or UTF-16 rules from the specification.
- `standalone="yes"` claims the document does not depend on external DTD declarations for its content. It is a hint to that processing, not a security switch.
- A wrong encoding declaration mojibakes the document or makes the parser fail. The bytes and the label have to match.
- It looks like a processing instruction. Treat it as the prolog, not as application data.

**Examples:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>café</note>
```

The file is UTF-8, so `é` is the UTF-8 bytes for that character. Labeling this file `encoding="UTF-16"` makes a correct parser reject it.
</details>

<details>
<summary>Describe the concept of XML entities and their usage.</summary>

### Describe the concept of XML entities and their usage.

**Definition:**

An entity is a named stand-in. When the parser sees a reference, it replaces it with the entity’s value. Built-in entities exist so markup characters can appear as text.

**How it works:**

- The predefined entities are `&lt;` `<`, `&gt;` `>`, `&amp;` `&`, `&apos;` `'`, and `&quot;` `"`. You need `&amp;` and `&lt;` in text or the parser thinks markup has started.
- A character reference is numeric: `&#233;` or `&#xE9;` is `é`. It does not need a declaration.
- A DTD can declare a general entity: `<!ENTITY city "Paris">`, used as `&city;` in content.
- Parameter entities (`%name;`) are for use inside the DTD itself, to reuse pieces of declarations.
- External entities point at another file or URL. That is the XXE problem: a parser that resolves them can be tricked into reading local files or calling internal hosts. Disable external entity resolution unless you fully control the DTD.
- Entities are expanded before your application sees the text. They are not tags.

**Examples:**

```xml
<note>Tom &amp; Jerry &#x2014; a title</note>
```

The text value is `Tom & Jerry — a title`. The raw `&` could not have been written unescaped.
</details>

### XML Syntax and Structure

<details>
<summary>What are the rules for naming XML elements and attributes?</summary>

### What are the rules for naming XML elements and attributes?

**Definition:**

Element and attribute names follow the same Name rule. They are case-sensitive, and they cannot contain spaces or start with a digit or punctuation that markup already uses.

**How it works:**

- A name may start with a letter, underscore, or colon. Later characters may also be digits, hyphens, and periods. Colons are allowed by the grammar but reserved for namespace prefixes. Do not put a colon in a name you invent unless it is a real prefix.
- `item`, `_id`, and `sku-2` are legal. `2sku`, `sku code`, and `sku<` are not.
- `<Item>` and `<item>` are two different elements. The same applies to attributes.
- There is no separate, stricter rule for attributes. `id` as an attribute is not special to the XML grammar (some applications treat `xml:id` specially).
- Names that start with `xml` in any case are reserved for the specification (`xml:lang`, `xml:space`).
- Prefer short, stable names without colons. The schema, not the name grammar, says which names are allowed in your vocabulary.

**Examples:**

```xml
<sku-code region="eu">A1</sku-code>   <!-- legal -->
<!-- <2sku region=eu> is illegal: name starts with a digit, attribute is unquoted -->
```
</details>

<details>
<summary>Explain the difference between XML elements and attributes.</summary>

### Explain the difference between XML elements and attributes.

**Definition:**

Elements build the tree and can contain text and other elements. Attributes are unordered name-value strings on a single element, and they cannot repeat or nest.

**How it works:**

- An element can appear many times as a sibling (`<sku>` twice). An attribute name can appear only once on that tag.
- An element can hold children and mixed content. An attribute value is flat text.
- Order of elements is significant in the document. Order of attributes is not. Parsers may hand them back in any order.
- Attributes fit ids, language, and units. Elements fit lists, long text, and anything that may grow structure later.
- Some APIs make attributes slightly more awkward to extend. Adding a child is usually a compatible change. Changing an attribute into an element is not.
- Neither is “more valid.” Validity is whatever your schema says. The split is about shape.

**Examples:**

```xml
<book id="b1">
  <author>Ada</author>
  <author>Edsger</author>
</book>
```

Two authors need elements. `author="Ada"` cannot be repeated on the same `book`, and it cannot later grow a `<born>` child.
</details>

<details>
<summary>What is the purpose of XML comments and how are they represented?</summary>

### What is the purpose of XML comments and how are they represented?

**Definition:**

A comment is a note for people reading the source. The parser does not treat it as data, and a normal application never sees it as an element or as text content.

**How it works:**

- The syntax is `<!-- words -->`. It may appear where miscellaneous markup is allowed: in the prolog, after the root, and between elements.
- It must not contain `--`. That sequence is how the comment grammar works, so a double hyphen ends it early or makes the document not well-formed.
- Comments are not a place for secrets. They are still in the file you ship, and some pipelines keep them.
- Do not wrap real data in a comment and expect a consumer to read it. Data belongs in elements.
- A comment cannot appear inside a tag, and it does not change validation of the elements around it except by being skipped.

**Examples:**

```xml
<qty>2</qty> <!-- warehouse count, not the order qty -->
```

The application’s text for `qty` is `2`. The note is for the person editing the file.
</details>

<details>
<summary>Describe the concept of XML processing instructions.</summary>

### Describe the concept of XML processing instructions.

**Definition:**

A processing instruction is a note addressed to a particular program, not to the XML data model of elements and text. The parser passes the target name and the rest of the text through.

**How it works:**

- The form is `<?target instructions?>`. The target is a name. What follows is up to that program and is not attributes in the XML sense, even when it looks like `key="value"`.
- `xml-stylesheet` is the common one: `<?xml-stylesheet type="text/xsl" href="style.xsl"?>` tells a browser or processor which transform to apply.
- The XML declaration `<?xml ...?>` looks similar but is the prolog, not an application instruction. Do not invent a target named `xml`.
- Targets are case-sensitive. Unknown targets are ignored by programs that are not the audience.
- They are not a substitute for elements. Data you need to query belongs in the tree. A processing instruction is a side channel for a tool.
- CDATA, character references, and the declaration solve different problems (raw text, a character, encoding). They are not processing instructions.

**Examples:**

```xml
<?xml-stylesheet type="text/xsl" href="order.xsl"?>
<order id="17"/>
```

A browser that honors the instruction applies `order.xsl`. A program that only reads `order` can ignore the instruction and still see the element.
</details>

<details>
<summary>What is the difference between an empty element and a self-closing element in XML?</summary>

### What is the difference between an empty element and a self-closing element in XML?

**Definition:**

In XML they are two ways to write the same thing: an element with no content. `<flag></flag>` and `<flag/>` produce the same element in the data model.

**How it works:**

- The empty-element tag `<flag/>` is syntax. The paired form `<flag></flag>` is also empty when nothing sits between the tags.
- Both may carry attributes: `<flag on="true"/>` and `<flag on="true"></flag>` match.
- Neither form can contain children. If there is text or a child, you must use a start tag and an end tag. A self-closing tag cannot “also” hold content.
- Parsers are allowed to report either spelling. Do not branch your code on which form was in the file. Branch on whether the element has children.
- HTML is different. Some HTML elements are void (`br`, `img`) and HTML5 does not use XML’s rules. XHTML, being XML, treats `<br/>` and `<br></br>` as the same empty element.
- Android layout XML follows the XML rule: the self-closing form and the paired empty form are equivalent.

**Examples:**

```xml
<img src="a.png"/>
<img src="a.png"></img>
```

An XML parser builds one element named `img` with one attribute either way. `<img src="a.png">caption</img>` is a third case: it is not empty, so the self-closing form cannot express it.
</details>

<details>
<summary>Explain the concept of XML CDATA sections and their usage.</summary>

### Explain the concept of XML CDATA sections and their usage.

**Definition:**

A CDATA section is a span of character data in which < and & are ordinary characters. Only the closing `]]>` is recognized as markup.

**How it works:**

- The XML specification says CDATA sections may occur anywhere character data may occur. They escape blocks of text that would otherwise be read as markup.
- They begin with `<![CDATA[` and end with `]]>`. They cannot nest. Inside, left angle brackets and ampersands need not and cannot be escaped with `&lt;` and `&amp;`.
- In the spec's example, `<greeting>Hello, world!</greeting>` inside a CDATA section is character data, not an element.
- MDN's XML introduction is the other tool for the same characters outside CDATA: the five predefined entities, including `&lt;` and `&amp;`. Use those when the text is short or when `]]>` itself appears in the data, because that sequence would end the section.
- A CDATA section is not a comment, not a processing instruction, and not a way to hide markup from a parser that expands the section into character data.

**Examples:**

```xml
<script><![CDATA[ if (a < b && c > d) { run(); } ]]></script>
```

**Sources:**

- https://www.w3.org/TR/xml/#sec-cdata-sect
- https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction
</details>

<details>
<summary>What is the purpose of the XML prolog and what does it contain?</summary>

### What is the purpose of the XML prolog and what does it contain?

**Definition:**

The prolog is the optional front of an XML document, before the root element. XML 1.0 says documents should begin with an XML declaration that specifies the version of XML being used.

**How it works:**

- The production is an optional XML declaration, then miscellaneous markup (comments and processing instructions), then an optional document type declaration and more miscellaneous markup.
- The XML declaration is `<?xml`, a required version, an optional encoding, and an optional standalone declaration, closed by `?>`. MDN’s form is `<?xml version="1.0" encoding="UTF-8"?>`.
- The document type declaration, when present, comes before the first element. It is what makes a document a candidate for validity: the document is valid only if it has one and complies with it.
- A document with only a root element and no declaration is still well-formed. `<?xml version="1.0"?><greeting>Hello, world!</greeting>` and `<greeting>Hello, world!</greeting>` are both complete well-formed documents in the spec’s examples. The second is not valid, because it has no document type declaration.

**Examples:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<greeting>Hello, world!</greeting>
```

**Sources:**

- https://www.w3.org/TR/xml/#sec-prolog-dtd
- https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction
</details>

<details>
<summary>Describe the concept of XML character references and their usage.</summary>

### Describe the concept of XML character references and their usage.

**Definition:**

A character reference names one character by its code point in ISO/IEC 10646. Decimal form is `&#` digits `;`. Hexadecimal form is `&#x` hex digits `;`.

**How it works:**

- The referenced character must match the Char production. That is a well-formedness constraint.
- `&#x` takes the hexadecimal code point. `&#` takes the decimal code point. Both end at `;`.
- Numeric character references are expanded when recognized and must be treated as character data. The spec says `&#60;` and `&#38;` may escape `<` and `&` in character data.
- They are not entity references. An entity reference is `&` name `;` and pulls in a named entity. The five predefined entities are `amp`, `lt`, `gt`, `apos`, and `quot`. Processors must recognize those whether or not they are declared.
- Use a character reference for a character you cannot type, or when you must write `<` or `&` as character data without a CDATA section.

**Examples:**

```xml
<price>&#8364;10</price>
<compare>&#60; and &#38;</compare>
```

**Sources:**

- https://www.w3.org/TR/xml/#sec-references
- https://www.w3.org/TR/xml/#sec-predefined-ent
</details>

<details>
<summary>What is the difference between an XML document and an XML fragment?</summary>

### What is the difference between an XML document and an XML fragment?

**Definition:**

An XML document matches the document production: an optional prolog, exactly one root element, and optional miscellaneous markup after it. A fragment is a piece of that tree, not a document by itself.

**How it works:**

- Well-formedness requires one or more elements and exactly one root, with every other element nested inside it. Two sibling elements with no parent are not a document.
- MDN’s `Document` is the object `DOMParser.parseFromString()` returns when it parses a full XML or HTML source string. `XMLSerializer` turns a DOM tree back into source.
- A `DocumentFragment` is a minimal document object with no parent. It stores a segment of nodes and is not part of the active document tree. Changes to the fragment do not change the document until you insert the fragment.
- Inserting the fragment with `append()` or `appendChild()` moves its children into the document and leaves the fragment empty.
- Do not send a fragment to a parser that requires a single root and expect a document. Wrap it in one root element first.

**When to use:**

Parse a document when you have one root and need a `Document`. Build a `DocumentFragment` when you are assembling several nodes before attaching them to a live tree.

**Sources:**

- https://www.w3.org/TR/xml/#sec-well-formed
- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
- https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment
</details>

<details>
<summary>Explain the concept of XML whitespace handling.</summary>

### Explain the concept of XML whitespace handling.

**Definition:**

White space in XML is one or more spaces (`#x20`), tabs (`#x9`), carriage returns (`#xD`), or line feeds (`#xA`). An XML processor must pass every character that is not markup through to the application.

**How it works:**

- Editors use white space to lay out tags. That white space is often not meant to be part of the delivered text. Poetry and source code are the spec’s examples of significant white space that should be kept.
- A validating processor must also tell the application which of those characters are white space in element content. SAX exposes that split: a validating parser reports ignorable white space through `ignorableWhitespace`, not only through `characters`.
- `xml:space` on an element signals intent. In a valid document it must be declared as an enumeration of `default`, `preserve`, or both. `default` means the application’s normal handling is acceptable. `preserve` means keep all the white space. The value applies to descendants until another `xml:space` overrides it. Any other value is an error.
- End-of-line handling happens first: literal `#xD` characters are removed or replaced by `#xA` before other processing. A character reference is how a `#xD` can still match the white-space production inside an entity value.

**Sources:**

- https://www.w3.org/TR/xml/#sec-white-space
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
</details>

### XML Validation and Schemas

<details>
<summary>What is XML Schema and its purpose?</summary>

### What is XML Schema and its purpose?

**Definition:**

XML Schema Definition Language describes the structure of XML documents and constrains their contents, including documents that use XML namespaces. The schema is itself an XML vocabulary and uses namespaces.

**How it works:**

- XSD 1.1 Part 1 substantially reconstructs and extends what a DTD can say. It depends on Part 2, Datatypes.
- A document can be well-formed and still fail a schema. Validity against a DTD is the XML 1.0 notion (a document type declaration plus compliance). Schema validation is the later, richer check.
- Components include element declarations, attribute declarations, simple type definitions, and complex type definitions. An element declaration associates a name with a type, an optional default, and identity constraints.
- `targetNamespace` on the `schema` element names the namespace the schema defines. The structures spec’s own example is `<schema targetNamespace="http://www.example.com/XMLSchema/1.0/mySchema">`.

**When to use:**

Use a schema when consumers must reject documents that are well-formed but shaped wrong. Use a DTD only when you are maintaining a document type that already has one.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Explain the difference between XML DTD and XML Schema.</summary>

### Explain the difference between XML DTD and XML Schema.

**Definition:**

A DTD is the document type declaration’s constraint language from XML 1.0. XML Schema is a separate XML vocabulary that describes structure and datatypes, and it extends what DTDs can express.

**How it works:**

- XML 1.0 says a document is valid if it has an associated document type declaration and complies with the constraints in it. The declaration must appear before the first element.
- The schema specification says the schema language substantially reconstructs and considerably extends DTD capabilities, and that it is itself represented in XML and uses namespaces. A DTD is not an XML document instance in that sense.
- Schema ties an element to a simple or complex type. A simple type constrains strings and the values they encode, for attributes and for text-only elements. A DTD’s attribute types are a smaller set (the XML spec’s CDATA, ID, IDREF, and the rest).
- Namespaces are a schema concern the DTD does not model as a namespace URI. The structures spec calls out documents which exploit the XML Namespace facility.

**When to use:**

Prefer XML Schema for new vocabularies that need types, namespaces, and constraints beyond a DTD. Keep a DTD when existing documents are validated that way and you are not ready to migrate.

**Sources:**

- https://www.w3.org/TR/xml/#sec-prolog-dtd
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What are the key components of an XML Schema document?</summary>

### What are the key components of an XML Schema document?

**Definition:**

An XML Schema is a set of schema components. The ones interviews name first are element declarations, attribute declarations, simple type definitions, and complex type definitions.

**How it works:**

- An element declaration associates a name with a simple or complex type, an optional default or fixed value, and a possibly empty set of identity-constraint definitions. It is either global or scoped to a complex type. The structures spec compares a top-level element declaration to a pair of DTD declarations.
- A complex type definition is a set of attribute declarations and a content type for the attributes and children of an element. The content may be empty, a string of a simple type, or a sequence of elements.
- A simple type definition is a set of constraints on strings and on the values they encode. It applies to attribute values and to elements with no element children. Each simple type, built-in or user-defined, is a restriction of its base type.
- Other components in the same specification include model groups, attribute groups, notations, and identity constraints. You do not need every component in a small schema. You do need a type for every element you want validated.
- The document element is `schema`, and `targetNamespace` names the namespace being defined.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Describe the concept of XML Schema data types.</summary>

### Describe the concept of XML Schema data types.

**Definition:**

XML Schema datatypes are the simple types Part 2 defines and the restrictions you build from them. A simple type constrains a string and the value that string encodes.

**How it works:**

- The structures spec says each simple type definition, built-in or user-defined, is a restriction of its base type. Built-in types are defined in XML Schema Datatypes. User-defined types restrict a base rather than starting from nothing.
- A simple type applies to an attribute value or to an element that has no element children. It does not describe nested elements. Nested structure is a complex type.
- The actual value is not always a string. The structures spec notes that it can be an integer, a boolean, a URI reference, and so on, after the lexical string is mapped.
- Facets, including the enumeration facet the structures spec names, are how a restriction narrows the base type.
- Part 1 depends on Part 2. You cannot talk about `xs:integer` or `xs:date` as structures-only ideas. They come from the datatypes specification.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What is the purpose of XML Schema elements and attributes?</summary>

### What is the purpose of XML Schema elements and attributes?

**Definition:**

In a schema, an element declaration and an attribute declaration say which names may appear and which type each name has. They are the bridge from a tag in the instance to a type in the schema.

**How it works:**

- An element declaration associates a name with a simple or complex type, an optional value constraint, and identity constraints. Global declarations can be referenced from many content models. A local declaration is scoped to the complex type that contains it.
- Attributes are declared as part of a complex type. The complex type definition is a set of attribute declarations plus a content type for the children.
- In the instance, XML 1.0 already treats attributes as name-value pairs on a start tag, with order not significant and with each name unique. The schema adds the type and whether the attribute is required.
- Use an element when the value has children or must repeat in order. Use an attribute for a single token that qualifies the element, such as `xml:lang`.

**When to use:**

Declare an element for structured or repeating data. Declare an attribute for a simple qualifier that does not itself contain elements.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
- https://www.w3.org/TR/xml/#sec-starttags
</details>

<details>
<summary>Explain the concept of XML Schema complex types and their usage.</summary>

### Explain the concept of XML Schema complex types and their usage.

**Definition:**

A complex type definition is a set of attribute declarations and a content type. It applies to the attributes and the children of an element.

**How it works:**

- The content type may require the children to be empty, or to be a string of a particular simple type, or to contain elements according to a content model.
- A complex type that allows element or attribute content in addition to what another type allows is an extension. A restriction narrows a base type. The structures spec notes that a vacuous restriction and a vacuous extension would accept the same inputs, but the syntax still requires each derived type to be written as one or the other.
- Use a complex type whenever an element has child elements or attributes that you want validated. A text-only element with no attributes can stay a simple type.
- Identity constraints and substitution-group affiliations hang off the element declaration, not off the idea of “a complex type” by itself. The type describes shape. The declaration attaches the shape to a name.

**When to use:**

Use a complex type for any element whose content is more than a single simple value. Extend a base when you are adding attributes or children. Restrict a base when every valid instance of the new type must already have been valid for the base.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What is the difference between XML Schema simple types and complex types?</summary>

### What is the difference between XML Schema simple types and complex types?

**Definition:**

A simple type constrains a string and the value it encodes. A complex type declares attributes and a content type for an element’s children.

**How it works:**

- The structures specification says a simple type applies to attribute values and to elements with no element children. Each simple type is a restriction of its base type. Built-in simple types come from the datatypes specification.
- A complex type is a set of attribute declarations and a content type. The content may be empty, a simple-typed string, or elements.
- If an element has child elements, or attributes you must validate, it needs a complex type. If it is only text, a simple type is enough.
- Derivation differs. Simple types restrict a base. Complex types are written as an extension or a restriction of another type.

**When to use:**

Use a simple type for a leaf value such as an integer or a date. Use a complex type as soon as the element has structure.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Describe the concept of XML Schema facets and their usage.</summary>

### Describe the concept of XML Schema facets and their usage.

**Definition:**

A facet is a constraint that restricts a simple type’s value space relative to its base type. The structures specification names the enumeration facet as one case, used where a value must be one of a listed set.

**How it works:**

- Every user-defined simple type is a restriction of a base. Facets are how that restriction is stated. Without them, a restriction would not say what changed.
- Enumeration is the facet the structures spec cites explicitly: the actual value is checked against the listed values, and the datatype may be more specific than a plain string.
- The same specification notes that fixed and default values on elements and attributes are also tied to the datatype mapping, not left as unchecked text.
- Facets apply to simple types. They do not replace a complex type’s content model. A content model says which children appear. A facet says which values a leaf may take.

**When to use:**

Put a facet on a simple type when the base type is too wide: a closed set of codes, a bounded number, or a pattern the lexical form must match. Use a complex type when the constraint is about child elements.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What is the purpose of XML Schema namespace declarations?</summary>

### What is the purpose of XML Schema namespace declarations?

**Definition:**

Namespace declarations in a schema keep the schema’s own vocabulary distinct from the vocabulary it defines, and they name the target namespace of the instance elements.

**How it works:**

- Namespaces in XML qualify element and attribute names by associating them with a namespace identified by a URI reference. A default namespace is declared with `xmlns`. A prefixed name uses a prefix bound to a URI.
- The schema language is itself an XML vocabulary and uses namespaces. The structures spec’s example puts `targetNamespace` on `schema` to name the namespace being defined: `http://www.example.com/XMLSchema/1.0/mySchema` in that example.
- Instance elements in the target namespace are the ones the schema validates. Elements in another namespace are a different vocabulary, which is how one document can mix several schemas.
- The prefix is local. Comparison is of the namespace URI, not of the prefix string. Two prefixes bound to the same URI are the same namespace.

**Sources:**

- https://www.w3.org/TR/xml-names/#ns-decl
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Explain the concept of XML Schema substitution groups.</summary>

### Explain the concept of XML Schema substitution groups.

**Definition:**

A substitution group lets one element stand where another element, the head, is expected. The structures specification records this on the element declaration as substitution group affiliations.

**How it works:**

- An element declaration has a set of substitution group affiliations: other element declarations it may substitute for. It also has substitution group exclusions and disallowed substitutions, which can block extension, restriction, or substitution.
- The head is an element declaration. Members are other element declarations that name that head. In the instance, a member can appear where the content model asked for the head, subject to those exclusions.
- This is a feature of element declarations, provided together with them. It is not a complex-type extension. Extension adds content to a type. Substitution swaps which element name is used.
- The `abstract` property on an element declaration, also in that component, can force the instance to use a member rather than the head itself.

**When to use:**

Use a substitution group when several element names share a role in one content model and you want the schema to allow any of those names. Use a choice inside a complex type when the alternatives are local and do not need to be reused as global elements.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

### XML Parsing and Processing

<details>
<summary>What is XML parsing and why is it important?</summary>

### What is XML parsing and why is it important?

**Definition:**

XML parsing reads a text document and reports its structure: elements, attributes, and character data, under the well-formedness rules. Without a parser, the application would be guessing at tags.

**How it works:**

- A well-formed document has exactly one root and properly nested elements. A parser rejects text that breaks those constraints instead of handing the application a half-read tree.
- Character references and the predefined entities are expanded by the processor. Markup such as comments and processing instructions is separated from character data.
- Two shapes of API are common. A tree API builds a document you can walk later. `DOMParser.parseFromString()` returns a `Document`. An event API reports start and end of elements as it reads. SAX `ContentHandler` is that shape: the parser calls `startElement` and `endElement`.
- Parsing is what makes the later steps possible: XPath, schema validation, and transformation all assume a processor has already recognized the markup.

**Sources:**

- https://www.w3.org/TR/xml/#sec-well-formed
- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
</details>

<details>
<summary>Explain the difference between DOM and SAX parsing.</summary>

### Explain the difference between DOM and SAX parsing.

**Definition:**

DOM parsing builds a tree and returns it. SAX parsing reports events while it reads and does not keep the tree.

**How it works:**

- In the browser, `DOMParser.parseFromString()` parses an XML string into a `Document`. You then walk `childNodes` or query the tree. The whole document is in memory after the call returns.
- SAX `ContentHandler` is the other contract. The application registers a handler with `setContentHandler`. The parser calls `startDocument`, `startElement`, `characters`, `endElement`, and `endDocument` in document order. Element content is reported between the matching start and end events.
- DOM can be revisited and modified. SAX is one pass. If you need a parent after you have seen a later sibling, SAX has already moved on unless you stored it yourself.
- SAX can reject or skip work before the document ends. A fatal error may stop the parse so that `endDocument` is never called.

**When to use:**

Use DOM or `DOMParser` when the document is small enough to hold and you will query it more than once. Use SAX when the document is large and you can decide as each element arrives.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
</details>

<details>
<summary>What are the advantages and disadvantages of using DOM parsing?</summary>

### What are the advantages and disadvantages of using DOM parsing?

**Definition:**

DOM parsing gives you a `Document` you can traverse and change. The cost is that the tree exists in memory for the whole document.

**How it works:**

- `DOMParser.parseFromString()` returns a `Document`. `XMLSerializer` can turn that tree back into XML text. Java’s `DocumentBuilder.parse` does the same job from a file, stream, URI, or SAX `InputSource`, and `newDocument()` builds an empty tree.
- Advantages: random access, a second query without rereading the file, and straightforward edits. XPath on MDN is defined as a way to address parts of that kind of tree.
- Disadvantages: memory grows with the document. You cannot emit a result until the parse finishes. A streaming handler such as SAX `ContentHandler` or StAX `XMLStreamReader` never holds the unused siblings.
- MDN notes that a `DocumentFragment` is a lighter place to assemble nodes before you attach them. It does not remove the cost of parsing a huge source into a `Document` first.

**When to use:**

Use DOM when you will query or edit the document and it fits in memory. Prefer a streaming parser when you only need one pass over a large file.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
</details>

<details>
<summary>Describe the concept of XML XPath and its usage.</summary>

### Describe the concept of XML XPath and its usage.

**Definition:**

XPath is the XML Path Language. MDN says it uses a non-XML syntax to address parts of an XML document and to test whether a node matches a pattern.

**How it works:**

- Paths look like URLs: steps through the tree. The non-XML syntax is deliberate so a path can sit in a URI or in an attribute value.
- XPath is mainly used in XSLT. MDN also documents it as a way to navigate the DOM of an XML-like document, including HTML and SVG, through `XPathExpression`, instead of only `getElementById`, `querySelectorAll`, and `childNodes`.
- Axes describe relationships between nodes. Functions compute on the nodes a path selects. MDN keeps those two lists as the reference.
- A path does not transform the document. It selects. Transformation is XSLT’s job, and querying a collection of documents is XQuery’s.

**Examples:**

A path such as `/catalog/book[@id='b1']/title` points at the `title` child of the `book` whose `id` attribute is `b1`, starting from the document root.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/XML/XPath
</details>

<details>
<summary>What is the purpose of XML XSLT and how does it work?</summary>

### What is the purpose of XML XSLT and how does it work?

**Definition:**

XSLT is a language for transforming XML documents into other XML documents. XSLT 3.0 is designed to be used with XPath 3.0 and shares XPath’s data model.

**How it works:**

- A stylesheet matches nodes and writes a result tree. XPath, not a second XML query language, is how a template addresses the source.
- XSLT 3.0’s main change from 2.0 is streaming: a transformation that does not have to hold the entire source or the entire result in memory. The other aim is modularity, so a large stylesheet can be built from independently developed components.
- The stylesheet is itself XML. The source is XML. The result is often XML, but the language’s stated primary purpose is transforming XML into other XML.
- SAX has a `TransformerHandler` among the known implementors of `ContentHandler`, which is the event-shaped way a processor can receive the source. You do not have to build a DOM first if the processor streams.

**When to use:**

Use XSLT when the job is a declarative mapping from one XML shape to another. Use application code when the mapping depends on state XSLT would only obscure.

**Sources:**

- https://www.w3.org/TR/xslt-30/#intro
- https://developer.mozilla.org/en-US/docs/Web/XML/XPath
</details>

<details>
<summary>Explain the concept of XML DOM traversal and manipulation.</summary>

### Explain the concept of XML DOM traversal and manipulation.

**Definition:**

DOM traversal walks the node tree a parser built. Manipulation inserts, moves, or replaces those nodes, then can serialize the tree back to XML.

**How it works:**

- `DOMParser.parseFromString()` produces a `Document`. From there, `childNodes`, `getElementById`, and `querySelectorAll` walk elements. MDN also points at XPath as a more powerful address into the same tree.
- `DocumentFragment` is the usual place to assemble a subtree off to the side. `append`, `prepend`, and `replaceChildren` change its children. Appending the fragment to a live parent moves the nodes and leaves the fragment empty.
- `XMLSerializer` performs the opposite of parsing: a DOM tree becomes XML or HTML source.
- Java’s `DocumentBuilder.newDocument()` creates an empty `Document` you can fill, and `parse` fills one from bytes. Edits are to the tree, not to the original file, until you write the serialization out.
- Traversal sees the logical tree. Attribute order on a start tag is not significant in XML, so do not depend on attribute order surviving a parse and a write.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
- https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment
- https://www.w3.org/TR/xml/#sec-starttags
</details>

<details>
<summary>What is the difference between XML pull parsing and push parsing?</summary>

### What is the difference between XML pull parsing and push parsing?

**Definition:**

Push parsing calls your code when the parser is ready. Pull parsing waits until your code asks for the next event.

**How it works:**

- SAX is push. You implement `ContentHandler` and the parser invokes `startElement`, `characters`, and `endElement` in document order. You cannot pause the parser between events except by throwing from the callback.
- StAX `XMLStreamReader` is pull. It starts on `START_DOCUMENT`. You call `hasNext()` and `next()`, then read the current event with `getEventType()`, `getLocalName()`, or `getText()`.
- Pull code can interleave other work between events and can stop without setting up a handler object. Push code keeps the parse loop inside the parser, which is simple when you only react.
- Both are one-pass. Neither builds the DOM tree that `DocumentBuilder.parse` returns.

**When to use:**

Use pull (`XMLStreamReader`) when the application should control the loop. Use push (SAX) when a handler already matches the events and you want the parser to drive.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/stream/XMLStreamReader.html
</details>

<details>
<summary>Describe the concept of XML streaming and its benefits.</summary>

### Describe the concept of XML streaming and its benefits.

**Definition:**

Streaming reads or writes XML without holding the whole document as a tree. The benefit is bounded memory and an earlier result.

**How it works:**

- `XMLStreamReader` is specified as forward, read-only access and as the lowest-level efficient way to read XML. You iterate with `next()` and never receive a `Document`.
- SAX streams in the other direction: events arrive in order, and character data may be split across several `characters` calls. You still do not hold the document.
- XSLT 3.0 added streaming so that neither the source nor the result has to be held in memory in its entirety. That is the same idea at stylesheet level.
- A DOM parse cannot stream. `DOMParser` and `DocumentBuilder.parse` return only after the tree exists.
- Streaming loses random access. If a later decision needs an earlier sibling you did not store, you cannot go back on an `XMLStreamReader`.

**When to use:**

Stream when documents are large or when you can emit output as soon as one element is complete. Build a DOM when you need to query arbitrary nodes afterward.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/stream/XMLStreamReader.html
- https://www.w3.org/TR/xslt-30/#intro
</details>

<details>
<summary>What is the purpose of XML SAX events and how are they handled?</summary>

### What is the purpose of XML SAX events and how are they handled?

**Definition:**

SAX events are the callbacks a parser makes as it recognizes markup. The application handles them by implementing `ContentHandler` and registering it with `setContentHandler`.

**How it works:**

- `startDocument` is called once, before the other events, except that `setDocumentLocator` may come first. `endDocument` is called after a successful parse. A fatal error may skip `endDocument`.
- `startElement` reports the namespace URI, the local name, the qualified name, and the attributes. `endElement` uses the same names. Content of an element, including children, is reported between those two calls, even when the element is empty.
- `characters` reports a chunk of character data. The parser may split one run of text into several calls. The application must read only `ch[start]` through `length` characters. Validating parsers report ignorable white space with `ignorableWhitespace` instead.
- `processingInstruction` reports a target and data. A SAX parser must not use that method for the XML declaration. A separate `declaration` callback exists for version, encoding, and standalone.
- `startPrefixMapping` and `endPrefixMapping` report namespace scope. `skippedEntity` reports an entity the parser did not expand.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
</details>

<details>
<summary>Explain the concept of XML StAX (Streaming API for XML) and its usage.</summary>

### Explain the concept of XML StAX (Streaming API for XML) and its usage.

**Definition:**

StAX is the pull-style streaming API. `XMLStreamReader` gives forward, read-only access: the application drives the cursor with `next()` and `hasNext()`.

**How it works:**

- An instance starts at `START_DOCUMENT`. `getEventType()` is the current event. `next()` loads the next one. At `END_DOCUMENT` the valid call is `close()`.
- Events include the XML declaration, a DTD, a start tag, character data, white space, an end tag, a comment, and a processing instruction. On `START_ELEMENT` you may call `getLocalName()`, `getAttribute` methods, and `getElementText()`. On character data you call `getText`.
- Calling a method in the wrong state throws `IllegalStateException`. The class documents which methods are valid for each event type.
- During a DTD event, `getProperty("javax.xml.stream.notations")` and `getProperty("javax.xml.stream.entities")` expose notations and entity declarations. Those properties are defined to return null if the information is not available, and they are only for the DTD event.
- This is not SAX. Nothing calls you. You poll.

**When to use:**

Use `XMLStreamReader` to read a large document in one forward pass when you want the loop in your own code.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/stream/XMLStreamReader.html
</details>

### XML Technologies and APIs

<details>
<summary>What is XML DOM (Document Object Model) and its purpose?</summary>

### What is XML DOM (Document Object Model) and its purpose?

**Definition:**

The DOM is a tree of nodes that represents a document after parsing. Its purpose is to let a program inspect and change the document without rereading the text.

**How it works:**

- In the browser, `DOMParser.parseFromString()` turns an XML or HTML string into a `Document`. `XMLSerializer` turns a tree back into source.
- The tree is made of nodes. Elements have children. A `DocumentFragment` holds a segment that is not yet in the live tree.
- Java obtains the same kind of object from `DocumentBuilder`, created by `DocumentBuilderFactory.newDocumentBuilder()`. `parse` reads a file, stream, URI, or `InputSource`. `newDocument()` starts an empty tree. `isNamespaceAware()` and `isValidating()` report how that builder was configured.
- The purpose stops at the tree. Selecting with a path is XPath. Rewriting to another shape is XSLT. Checking a schema is validation. The DOM is the structure those steps can sit on.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/DOMParser
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
</details>

<details>
<summary>Explain the concept of XML SAX (Simple API for XML) and its usage.</summary>

### Explain the concept of XML SAX (Simple API for XML) and its usage.

**Definition:**

SAX is an event API. The parser reads the document and calls a `ContentHandler` for each logical piece, instead of returning a tree.

**How it works:**

- Most SAX applications implement `ContentHandler` and register it with `setContentHandler`. The order of callbacks mirrors the document. An element’s character data, processing instructions, and children arrive between `startElement` and `endElement`.
- SAX 2.0, the interface in Java SE since 1.4, adds namespaces and `skippedEntity` relative to the older `DocumentHandler`.
- `DefaultHandler` is a provided class that implements the interface, so you override only the events you care about.
- `DocumentBuilder` reuses SAX types such as `InputSource`, `ErrorHandler`, and `EntityResolver`. Using those types does not mean the DOM builder is required to parse with a SAX parser. It means the application-facing API is shared.
- Use SAX to scan. Keep only the state you need. Do not expect to walk backward.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
</details>

<details>
<summary>What is XML JAXP (Java API for XML Processing) and its key components?</summary>

### What is XML JAXP (Java API for XML Processing) and its key components?

**Definition:**

JAXP is the Java SE API for processing XML. The pieces interviews meet are the DOM builder, the SAX handler, and the StAX cursor, all in the platform modules that `java.xml` exposes.

**How it works:**

- `DocumentBuilder`, from `DocumentBuilderFactory.newDocumentBuilder()`, parses to a W3C-style `Document` or creates an empty one with `newDocument()`. It can be namespace-aware and validating, and it accepts an `EntityResolver` and an `ErrorHandler`.
- SAX is the push parser behind `ContentHandler`: `startElement`, `endElement`, `characters`, and the rest.
- StAX is the pull parser: `XMLStreamReader.next()` walks `START_ELEMENT`, `CHARACTERS`, `END_ELEMENT`, and the other event types.
- `DocumentBuilder.getSchema()` returns the `Schema` in use, or null. That is the hook from parsing to XML Schema validation. Transformation sits next to these parsers; SAX even lists `TransformerHandler` as a `ContentHandler`.
- JAXP is not data binding. Mapping elements onto Java beans is Jakarta XML Binding, a separate specification.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/org/xml/sax/ContentHandler.html
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/stream/XMLStreamReader.html
</details>

<details>
<summary>Describe the concept of XML JAXB (Java Architecture for XML Binding) and its benefits.</summary>

### Describe the concept of XML JAXB (Java Architecture for XML Binding) and its benefits.

**Definition:**

Jakarta XML Binding, the specification that continues JAXB, maps between XML Schema and Java classes. Unmarshalling reads XML into objects. Marshalling writes objects back to XML.

**How it works:**

- The specification’s requirements cover XML Schema to Java, including default bindings and a customized binding schema, and the opposite direction, Java to XML Schema.
- The architecture uses mapping annotations and a binding framework. Unmarshalling and marshalling are the two runtime directions. Validation can run as part of those steps, and the specification also allows disabling schema validation.
- The benefit is that application code sees fields and objects instead of `startElement` events or a raw `Document`. The schema, not hand-written parsers, drives the mapping.
- It is not a parser you drop in where SAX belongs. Something still parses. Binding sits on top and allocates the object graph, so a huge document still costs memory proportional to the objects you materialize.
- The current specification is Jakarta XML Binding 4.0. The package is `jakarta.xml.bind`, not the older `javax.xml.bind` that Java SE no longer ships.

**When to use:**

Use binding when you have a stable schema and want Java objects. Use SAX or StAX when you must not build the object graph.

**Sources:**

- https://jakarta.ee/specifications/xml-binding/4.0/jakarta-xml-binding-spec-4.0.html
</details>

<details>
<summary>What is the purpose of XML XQuery and how does it differ from XPath?</summary>

### What is the purpose of XML XQuery and how does it differ from XPath?

**Definition:**

XQuery is a query language for XML data sources. XPath addresses parts of a document. XQuery uses XPath’s model and adds the language that queries across sources and builds a result.

**How it works:**

- The XQuery 3.1 recommendation says a query language that uses XML’s structure can query structured and semi-structured documents, relational data, and object repositories, whether the data is stored as XML or only viewed as XML.
- XPath, on MDN, is a non-XML path syntax for pointing at nodes and testing patterns. It does not by itself describe a FLWOR-style query or a constructed result document. XSLT is the transform language that embeds XPath.
- XQuery 3.1 also covers JSON: maps and arrays in the data model, for applications that mix JSON with XML and HTML.
- Use a path when you already hold one document and need a node. Use a query when the question spans many documents or must return a new structure.

**When to use:**

Choose XPath inside XSLT, a DOM, or a single-document lookup. Choose XQuery when the task is a query over a collection.

**Sources:**

- https://www.w3.org/TR/xquery-31/#id-introduction
- https://developer.mozilla.org/en-US/docs/Web/XML/XPath
</details>

<details>
<summary>Explain the concept of XML Namespaces and their usage in XML technologies.</summary>

### Explain the concept of XML Namespaces and their usage in XML technologies.

**Definition:**

An XML namespace qualifies an element or attribute name by associating it with a URI reference, so two vocabularies can share a document without colliding.

**How it works:**

- The Namespaces recommendation says this is a simple method for qualifying names. A default namespace uses `xmlns="uri"` and applies to unprefixed elements in scope. A prefix is bound with `xmlns:prefix="uri"` and used as `prefix:local`.
- Scope is the element where the declaration appears, and its descendants, until overridden. Attributes are not in the default namespace; an unprefixed attribute has no namespace.
- Names beginning with `xml` are reserved by XML itself. `xml:space` and `xml:lang` are in that reserved vocabulary. Do not invent your own `xml:` names.
- Schema uses namespaces twice: the schema document is in the schema vocabulary, and `targetNamespace` names the vocabulary being defined. XPath and DOM expose the namespace URI separately from the prefix. SAX `startElement` passes `uri`, `localName`, and `qName` for that reason. Canonical XML has to pick one physical form for those declarations, because prefixes are not part of the logical name.

**Sources:**

- https://www.w3.org/TR/xml-names/#ns-decl
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What is XML Signature and its role in XML security?</summary>

### What is XML Signature and its role in XML security?

**Definition:**

XML Signature is the W3C syntax and processing rules for a digital signature carried in XML. The recommendation says signatures provide integrity, message authentication, and/or signer authentication for data of any type, whether the data sits inside the signature’s XML or elsewhere.

**How it works:**

- The signature is an XML element. It can cover an element in the same document or data outside it. The processing rules, not a private layout, decide how the bytes are selected and hashed.
- Version 1.1 made Canonical XML 1.1 a required canonicalization algorithm and updated the mandatory algorithms, including Elliptic Curve DSA and additional hash algorithms. Canonicalization exists so two logically identical infosets produce the same bytes before the hash.
- This is the XML packaging of a digital signature. NIST’s definition still applies: the private key signs, the public key verifies, and confidentiality is a different service.
- XML Encryption is that other service. A signature does not hide the data it covers.

**Sources:**

- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
- https://csrc.nist.gov/glossary/term/digital_signature
</details>

<details>
<summary>Describe the concept of XML Encryption and its purpose.</summary>

### Describe the concept of XML Encryption and its purpose.

**Definition:**

XML Encryption specifies a process for encrypting data and representing the result in XML. The result is an XML Encryption element that contains or references the cipher data.

**How it works:**

- The data may be an octet stream, other unstructured data, or structured XML: a document, an element, or element content. You can encrypt one element and leave the rest of the document readable.
- The cipher data is either inside the encryption element or referenced from it. The recipient uses the key information carried with that element to decrypt.
- Purpose is confidentiality of the chosen data. It does not by itself prove who produced the document. That is XML Signature.
- The two recommendations are designed to be used together. XKMS, in turn, is the key-distribution protocol specified for use with both.

**Sources:**

- https://www.w3.org/TR/xmlenc-core1/#sec-Overview
</details>

<details>
<summary>What is the purpose of XML Canonicalization and its usage?</summary>

### What is the purpose of XML Canonicalization and its usage?

**Definition:**

Canonical XML produces one physical representation of an XML document so that logically equivalent documents, which XML allows to differ in syntax, compare as equal. Version 1.1 is a revision of 1.0 for namespace attributes inherited by document subsets.

**How it works:**

- XML 1.0 permits differences that do not change the logical document: attribute order, certain white space, and how namespaces are written. A signature over raw bytes would fail after a harmless rewrite.
- If two documents have the same canonical form, they are logically equivalent in the application context the specification describes, except for a few unusual cases it limits. The reverse is not always true: differing canonical forms can still be equivalent under application rules the spec does not try to capture.
- Canonical XML 1.1 is defined for XML 1.0 and the XPath 1.0 data model. It is not defined for XML 1.1. It changes inheritance of XML-namespace attributes on subsets, including not inheriting `xml:id`, and it treats `xml:base` URI paths in a defined way.
- XML Signature 1.1 requires Canonical XML 1.1 as a canonicalization algorithm. That is the usual place you meet it: before hashing signed XML.

**Sources:**

- https://www.w3.org/TR/xml-c14n11/#sec-Intro
- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
</details>

<details>
<summary>Explain the concept of XML Catalogs and their benefits.</summary>

### Explain the concept of XML Catalogs and their benefits.

**Definition:**

An XML catalog is a local map from the external identifiers in a document to resources the processor should actually open. The parser asks an entity resolver; the catalog is the table that resolver consults.

**How it works:**

- XML documents refer to external entities and to grammars through identifiers in the document type declaration. A processor that fetches each of those URIs over the network depends on the network and on whatever is currently published at that URI.
- Java’s `DocumentBuilder.setEntityResolver` is the hook. The application supplies an `EntityResolver` that turns those identifiers into a local `InputSource`. Setting the resolver to null restores the implementation’s default.
- The benefit is repeatability and speed: the same public or system identifier always resolves to the copy you shipped, and validation does not wait on a remote host.
- A catalog does not change well-formedness or the schema. It only changes where the bytes of an external entity are read from.

**When to use:**

Use a catalog or another `EntityResolver` when documents reference DTDs or schemas that you can ship beside the application. Leave the default resolver only when the remote copy is the one you intend to trust.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
- https://www.w3.org/TR/xml/#sec-prolog-dtd
</details>

### XML Best Practices and Design Patterns

<details>
<summary>What are some best practices for designing XML schemas?</summary>

### What are some best practices for designing XML schemas?

**Definition:**

Design a schema as a set of named types and global elements in a target namespace, with simple types restricted only as far as the values require and complex types reserved for structure.

**How it works:**

- Put the vocabulary in a `targetNamespace`. The schema document itself lives in the schema namespace, so the two do not collide. That is the split the structures specification is built on.
- Prefer a simple type, restricted from a built-in base, for a leaf. Add facets such as enumeration when the value set is closed. Use a complex type when there are children or attributes.
- Keep element declarations global when other schemas or substitution groups must refer to them. Use a local declaration when the name only makes sense inside one parent.
- Version the namespace or the schema location when the content model changes in a way old readers must not silently accept. A changed type in the same namespace without a version story makes old and new documents look alike and fail differently.
- Validate with the schema. Well-formedness alone does not enforce it. `DocumentBuilder` can be configured to validate and can expose the `Schema` it uses.

**Sources:**

- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Explain the concept of XML modularity and its benefits.</summary>

### Explain the concept of XML modularity and its benefits.

**Definition:**

XML modularity is splitting a vocabulary or a stylesheet into parts that compose, instead of one document that defines everything.

**How it works:**

- Namespaces are the composition rule for vocabularies. Each module has a namespace URI. A document mixes them with prefixes. The Namespaces recommendation exists so those names stay qualified.
- Schema components can be global, so another schema document can refer to an element or type instead of copying it. Substitution groups are one reuse mechanism on element declarations.
- XSLT 3.0 calls modularity out as a design aim: large stylesheets built from independently developed components. Streaming is the other aim; modularity is what keeps a large transform maintainable.
- External entities and catalogs are the physical side. A grammar can live in its own file, and an `EntityResolver` can load your copy. That is reuse of text, which is weaker than reuse of schema components, because the included text is not a namespace of its own.

**When to use:**

Split on namespace and on global schema components when more than one team owns the vocabulary. Split an XSLT stylesheet when independent transforms must be combined.

**Sources:**

- https://www.w3.org/TR/xml-names/#ns-decl
- https://www.w3.org/TR/xslt-30/#intro
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>What is the purpose of XML versioning and how is it achieved?</summary>

### What is the purpose of XML versioning and how is it achieved?

**Definition:**

Versioning tells a reader which grammar a document uses, so a change in that grammar is visible instead of being mistaken for a valid old document.

**How it works:**

- The XML declaration’s version is the version of XML, such as `1.0`, not the version of your vocabulary. Do not overload it as an application version.
- A schema’s `targetNamespace` is the stable name of a vocabulary. A breaking change belongs in a new namespace URI, so old and new elements do not validate against each other’s types.
- Within one namespace, a compatible extension can add optional content. The structures specification’s extension of a complex type is the mechanism that adds attributes or children. Old readers that ignore unknown optional pieces can still read the core. Readers that validate strictly need the new schema.
- The document can also carry an application version attribute in your own namespace. That is a convention on top of the namespace, useful when the namespace stays the same and only optional fields appear.

**When to use:**

Change the namespace URI when a required content model changes. Extend a complex type, and keep the namespace, when you are only adding optional information.

**Sources:**

- https://www.w3.org/TR/xml/#sec-prolog-dtd
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
</details>

<details>
<summary>Describe the concept of XML internationalization and localization.</summary>

### Describe the concept of XML internationalization and localization.

**Definition:**

Internationalization is marking which natural language a document’s content is in, and encoding that content in Unicode. Localization is supplying the content for a particular language.

**How it works:**

- XML 1.0 defines `xml:lang` for this. The attribute specifies the language of the element’s contents and attribute values. Values are IETF BCP 47 language tags, and the empty string is allowed. In a valid document the attribute must be declared if it is used.
- The declaration applies to the element and, by inheritance in the spec’s processing model, identifies the language of that content. A child may set a different `xml:lang`.
- The XML declaration’s encoding, such as `UTF-8` in MDN’s example, is how the bytes map to characters. Character references address a code point in ISO/IEC 10646 when the character is not typed directly.
- Names of elements should be words, the specification says, and almost all characters are allowed in names. That is a separate choice from `xml:lang`: the tag language and the content language need not match.
- Localization does not require a different schema for each language if the structure is the same. It requires different character data, identified by `xml:lang`.

**Examples:**

```xml
<p xml:lang="en">The quick brown fox</p>
<p xml:lang="ru">Быстрая бурая лиса</p>
```

**Sources:**

- https://www.w3.org/TR/xml/#sec-lang-tag
- https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction
</details>

<details>
<summary>What are some common XML design patterns and their usage?</summary>

### What are some common XML design patterns and their usage?

**Definition:**

The patterns that come straight from the specifications are: one vocabulary per namespace, elements for structure, attributes for qualifiers, simple types for leaves, and complex types for trees.

**How it works:**

- Envelope versus payload. SOAP’s message construct is the envelope; the application data sits inside it. The envelope is one namespace, the payload another, which is what namespaces are for.
- Document versus collection. A document has exactly one root. A list of siblings that is not yet a document is a fragment; wrap it before you parse it as a document, or hold it in a `DocumentFragment` until you insert it.
- Extension versus substitution. Extend a complex type to add children or attributes. Use a substitution group when a different element name should be accepted where a head element was declared.
- Streaming versus tree. SAX and `XMLStreamReader` handle one pass. DOM handles repeated queries. Pick the pattern from the access, not from habit.
- Sign and encrypt as separate layers. XML Signature covers integrity and signer authentication. XML Encryption covers confidentiality and can encrypt a single element.

**Sources:**

- https://www.w3.org/TR/xml-names/#ns-decl
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
- https://www.w3.org/TR/soap12-part1/#intro
</details>

<details>
<summary>Explain the concept of XML data binding and its advantages.</summary>

### Explain the concept of XML data binding and its advantages.

**Definition:**

Data binding maps between an XML document and programming-language objects. Jakarta XML Binding specifies that mapping in both directions: schema to Java, and Java to schema.

**How it works:**

- Unmarshalling reads XML and builds the object graph. Marshalling writes the graph back out. The specification’s binding framework includes both, plus validation during those steps.
- Default bindings cover ordinary schema components. A customized binding schema or mapping annotations override the defaults when a name or a shape should not follow the default.
- The advantage is that business code uses fields. It does not implement `ContentHandler` or walk a `Document` for every field. The schema remains the contract.
- The cost is the object graph. Streaming with SAX or StAX stays cheaper when you do not need the objects. Binding also lags the schema: a change in a complex type means regenerating or adjusting the classes.

**When to use:**

Bind when the schema is the API and the application wants objects. Parse events when the application is a filter or a transform that should not allocate the graph.

**Sources:**

- https://jakarta.ee/specifications/xml-binding/4.0/jakarta-xml-binding-spec-4.0.html
</details>

<details>
<summary>What is the purpose of XML data validation and how is it performed?</summary>

### What is the purpose of XML data validation and how is it performed?

**Definition:**

Validation checks a well-formed document against a grammar. In XML 1.0, a document is valid if it has a document type declaration and complies with the constraints in it. XML Schema is the later grammar that constrains structure and datatypes, including namespaced documents.

**How it works:**

- Well-formedness comes first: one root, nested tags, legal characters. A document can be well-formed and not valid.
- DTD validation uses the document type declaration, which must appear before the first element. A validating processor also reports which white space in element content is ignorable.
- Schema validation uses the components in the schema: element and attribute declarations and simple and complex types. Java’s `DocumentBuilder.isValidating()` reports whether that parser was configured to validate, and `getSchema()` returns the `Schema` in use, or null.
- Jakarta XML Binding can validate while unmarshalling or marshalling, and its specification also describes disabling schema validation when you intentionally skip that check.
- Failure means the document does not match the grammar. It does not mean the bytes were unreadable.

**Sources:**

- https://www.w3.org/TR/xml/#sec-prolog-dtd
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
</details>

<details>
<summary>Describe the concept of XML data transformation and its techniques.</summary>

### Describe the concept of XML data transformation and its techniques.

**Definition:**

Transformation takes an XML document and produces another document. XSLT is the language the W3C specifies for transforming XML into other XML, using XPath to address the source.

**How it works:**

- A stylesheet matches nodes and builds a result. XSLT 3.0 shares the XPath 3.0 data model. Paths select; templates write.
- Streaming in XSLT 3.0 transforms without holding the entire source or the entire result in memory. That is the technique for large inputs. A DOM-based transform holds the tree first: parse with `DOMParser` or `DocumentBuilder`, then walk it.
- SAX can feed a transform without a tree. `TransformerHandler` is a `ContentHandler`, so the parser’s events are the transform’s input.
- XQuery is the related technique when the job is a query across sources that returns a new structure, including JSON maps and arrays in XQuery 3.1, rather than a stylesheet applied to one document.
- Application code is the third technique: read events or a tree and write the output yourself. Use it when the mapping is not a good fit for a declarative stylesheet.

**Sources:**

- https://www.w3.org/TR/xslt-30/#intro
- https://www.w3.org/TR/xquery-31/#id-introduction
- https://developer.mozilla.org/en-US/docs/Web/XML/XPath
</details>

<details>
<summary>What are some best practices for XML performance optimization?</summary>

### What are some best practices for XML performance optimization?

**Definition:**

XML performance is dominated by whether you build a tree, how large the document is, and whether external grammars are fetched. Choose the API that matches the access pattern.

**How it works:**

- `XMLStreamReader` is specified as forward, read-only, and the lowest-level efficient read. Use it, or SAX `ContentHandler`, for one pass over a large file. `DocumentBuilder.parse` and `DOMParser.parseFromString()` hold the whole tree.
- XSLT 3.0 streaming exists so a transform need not hold the entire source or result. Use it when the stylesheet can be written that way. A DOM transform pays for the tree first.
- Do not fetch a DTD or schema on every parse when you already have the file. `DocumentBuilder.setEntityResolver` points external identifiers at a local `InputSource`.
- `DocumentBuilder.reset()` returns a builder to the state it had after `newDocumentBuilder()`, so a process can reuse the builder instead of constructing one per document. The reset builder is not guaranteed to be the same resolver object, only a functionally equal one.
- MDN’s note on `DocumentFragment` is the opposite caution: assembling a fragment is not automatically faster than appending nodes, and readability wins when the difference is marginal. Do not add a fragment only as a performance trick.
- Binding with Jakarta XML Binding allocates an object graph. That is the wrong tool when you only need to filter elements.

**Sources:**

- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/stream/XMLStreamReader.html
- https://www.w3.org/TR/xslt-30/#intro
- https://docs.oracle.com/en/java/javase/21/docs/api/java.xml/javax/xml/parsers/DocumentBuilder.html
</details>

<details>
<summary>Explain the concept of XML security and its key considerations.</summary>

### Explain the concept of XML security and its key considerations.

**Definition:**

XML security is the set of W3C mechanisms that sign XML, encrypt XML, and put signed bytes into a canonical form, plus the ordinary XML rules that keep a parser from treating data as markup.

**How it works:**

- Integrity and signer authentication are XML Signature: a signature over data inside the document or elsewhere. Confidentiality is XML Encryption: an encryption element that contains or references cipher data, which may be one element or element content.
- Canonical XML 1.1 gives the byte sequence those signatures hash, so attribute order and namespace syntax do not change the signed value. XML Signature 1.1 requires that canonicalization.
- The parser’s job is separate. Character data must escape `<` and `&`, or use a CDATA section whose only special sequence is `]]>`. External entities are resolved through an `EntityResolver`. Point that resolver at local copies you trust rather than at whatever a document asks the network to fetch.
- XKMS distributes and registers the public keys those signatures and encryption steps use. The key is not inside the algorithm; it has to be bound to the party.
- None of these replace TLS on the wire. They protect the XML document as a document, including after it is stored or forwarded.

**Sources:**

- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
- https://www.w3.org/TR/xmlenc-core1/#sec-Overview
- https://www.w3.org/TR/xml-c14n11/#sec-Intro
</details>

### Advanced XML Topics

<details>
<summary>What is XML Signature and how does it ensure data integrity?</summary>

### What is XML Signature and how does it ensure data integrity?

**Definition:**

An XML Signature is an XML structure whose verification succeeds only if the signed data still matches the digest that was signed. The recommendation defines that as an integrity service, together with message authentication and signer authentication.

**How it works:**

- The data may be inside the same XML or elsewhere. The signature processing rules select the bytes, canonicalize XML when the data is XML, hash them, and check that value with the signature.
- Canonical XML 1.1 is what makes the byte sequence stable. Without it, a change in attribute order would change the hash even though the logical document did not change. With it, a real change in the text changes the canonical form and the digest no longer matches.
- XML Signature 1.1 requires Canonical XML 1.1 and names the hash algorithms that are part of conformance. The hash is the integrity check. A weak hash undermines it, which is why the recommendation added further hash algorithms.
- Verification uses the public key. It does not decrypt the payload. Hiding the payload is XML Encryption.

**Sources:**

- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
- https://www.w3.org/TR/xml-c14n11/#sec-Intro
</details>

<details>
<summary>Explain the concept of XML Encryption and its role in data confidentiality.</summary>

### Explain the concept of XML Encryption and its role in data confidentiality.

**Definition:**

XML Encryption’s role is confidentiality: data is encrypted and the result is represented as XML. The recommendation says the result is an XML Encryption element that contains or references the cipher data.

**How it works:**

- The input may be an octet stream or XML: a whole document, one element, or the content of an element. Encrypting element content leaves the surrounding tags visible and hides what was inside.
- The recipient decrypts with the key information associated with that encryption element. The ciphertext is not useful without that key.
- Confidentiality here is about the document, not about the TLS session that carried it. A stored message stays confidential only if this encryption, or an equivalent, was applied.
- It does not provide the integrity service. A ciphertext can still be replaced. Pair it with XML Signature when you also need to detect a change and identify the signer.

**Sources:**

- https://www.w3.org/TR/xmlenc-core1/#sec-Overview
- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
</details>

<details>
<summary>What is the purpose of XML Digital Signatures and how are they created?</summary>

### What is the purpose of XML Digital Signatures and how are they created?

**Definition:**

The purpose is integrity, message authentication, and signer authentication for any data, inside the signature document or outside it. Creation follows the XML Signature processing rules: select the data, canonicalize XML, hash, and sign.

**How it works:**

- The signature element records what was signed, which canonicalization and hash algorithms were used, and the signature value. Version 1.1 requires Canonical XML 1.1 so the hashed bytes do not depend on insignificant syntax.
- Hash algorithms are part of the signature. The 1.1 recommendation extended the set that implementations must handle, including additional hashes and Elliptic Curve DSA, because the digest is what later verification recomputes.
- The private key produces the signature value. Verifiers use the public key. NIST’s description of a digital signature is the same operation: private key signs, public key verifies, and the result is not confidentiality.
- Creating a signature does not encrypt the referenced data. If the data must be hidden, apply XML Encryption as a separate step and sign the result you need protected.

**Sources:**

- https://www.w3.org/TR/xmldsig-core1/#sec-Overview
- https://www.w3.org/TR/xml-c14n11/#sec-Intro
</details>

<details>
<summary>Describe the concept of XML Key Management and its importance.</summary>

### Describe the concept of XML Key Management and its importance.

**Definition:**

XML Key Management (XKMS) specifies protocols for distributing and registering public keys, for use with XML Signature and XML Encryption. It has two parts: X-KISS, the key information service, and X-KRSS, the key registration service.

**How it works:**

- A signature is only as useful as the binding between the public key and the signer. XKMS is how a client asks a service to locate and validate that key information instead of doing every trust decision locally.
- X-KRSS is the registration side: a holder registers a public key with the service. X-KISS is the query side: a verifier asks what a key means.
- The recommendation is explicit that these protocols are suitable for use in conjunction with the XML Signature and XML Encryption recommendations. They do not replace those processing rules.
- Importance follows from that split. Encryption and signature algorithms do not name the person. Key management does. Without a registered, locatable public key, a verifier cannot decide whose signature it just checked.

**Sources:**

- https://www.w3.org/TR/xkms2/#sec-Introduction
</details>

<details>
<summary>What is the role of XML in web services and service-oriented architectures (SOA)?</summary>

### What is the role of XML in web services and service-oriented architectures (SOA)?

**Definition:**

In a service-oriented system, XML is the message format that carries a self-describing request or response between services. SOAP 1.2 defines that message as an XML construct that can travel over different underlying protocols.

**How it works:**

- SOAP’s messaging framework is extensible and decentralized. The envelope is XML, so intermediaries can see the structure, route on headers, and leave the payload in its own namespace.
- The payload can be described by a schema: element declarations and complex types say what a valid body contains. Validation at the edge rejects a well-formed message that does not match the contract.
- XML Signature and XML Encryption apply to the message as a document, which matters when the message is stored or passes through a hop that is not the original TLS session.
- XKMS is the companion for the public keys those services use. The architecture is: schema for the contract, SOAP for the envelope, signature and encryption for the document, namespaces so each service’s vocabulary stays distinct.
- A later HTTP API may send JSON instead. That does not change what XML’s role was in the SOAP-based service layer: the typed, namespaced message.

**Sources:**

- https://www.w3.org/TR/soap12-part1/#intro
- https://www.w3.org/TR/xmlschema11-1/#intro-purpose
- https://www.w3.org/TR/xml-names/#ns-decl
</details>

<details>
<summary>Explain the concept of XML-based protocols such as SOAP and REST.</summary>

### Explain the concept of XML-based protocols such as SOAP and REST.

**Definition:**

SOAP 1.2 is an XML protocol: a lightweight messaging framework whose message is an XML construct exchanged over an underlying protocol. REST is not an XML protocol. MDN defines REST as architecture constraints for distributed systems, and notes that HTTP APIs are often called REST even when they do not meet every constraint.

**How it works:**

- A SOAP message is XML. Namespaces separate the envelope from the payload. The framework is extensible, and the same message construct can be carried by more than one transport. Part 1 is the messaging framework, not a programming API.
- A REST interaction transfers a representation of a resource using standard client/server operations. The representation may be XML, or it may be something else. Nothing in the REST constraints requires an XML envelope, a WSDL description, or a SOAP header.
- Choosing SOAP means choosing XML as the message and, usually, a schema as the contract, plus XML Signature or XML Encryption when the document itself must be protected.
- Choosing a REST-style HTTP API means choosing the resource and the HTTP method. If the representation is XML, the XML rules still apply: one root, namespaces if you mix vocabularies, and a schema if you validate. The protocol is still HTTP.

**When to use:**

Use SOAP when the contract is an XML message that must travel unchanged across intermediaries. Use HTTP with an XML or non-XML representation when the client is calling resources with ordinary web tools, which is the sense in which MDN says beginners can treat a REST API as an HTTP service.

**Sources:**

- https://www.w3.org/TR/soap12-part1/#intro
- https://developer.mozilla.org/en-US/docs/Glossary/REST
</details>

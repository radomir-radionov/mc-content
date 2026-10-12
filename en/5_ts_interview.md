# TypeScript — Interview

TypeScript interview questions. Answers are filled for the published subset; the rest are still empty.

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Types](./5_ts_types.md) · [Narrowing](./5_ts_narrowing.md) · [Functions](./5_ts_functions.md) · [Type Manipulation](./5_ts_type_manipulation.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — TypeScript](https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/).

### TypeScript Fundamentals

<details>
<summary>What is TypeScript and how does it differ from JavaScript?</summary>

### What is TypeScript and how does it differ from JavaScript?

**Definition:**

TypeScript is a statically-typed superset of JavaScript, developed and maintained by Microsoft. It enables enhanced code maintainability and predictability. After compiling, TypeScript code is transpiled into standard, browser-compatible JavaScript.

**How it works:**

Key distinctions between TypeScript and JavaScript include the use of type annotations, the ability to work with existing JavaScript code, and more.

TypeScript Features & Benefits:

- Type System: Offers static typing, allowing developers to define the type of variables, parameters, and return values. This helps catch errors during development, reducing runtime issues.
- Advanced Language Features: Incorporates modern ECMAScript syntax and features, often before they are rolled out in JavaScript. Additionally, TypeScript brings functional programming patterns, classes, and access modifiers (such as `public` and `private`)
- Compatibility with JavaScript: TypeScript can interpret existing JavaScript code with minor or no modifications.
- Tooling and Extra Safety: Provides enhanced autocompletion, refactoring, and documentation via TypeScript-aware tools. TypeScript helps catch and rectify common programming errors without needing to run the code.
- ECMAScript Compatibility: TypeScript allows developers to target different ECMAScript versions, ensuring the generated JavaScript is compatible with the targeted browsers.
- Code Structure & Readability: Promotes maintainability by enforcing a defined coding structure and fostering code clarity.

**When to use:**

TypeScript's Role in Modern Development:

- Workplace Adaptability: TypeScript is used in an extensive range of projects, from small utilities to large-scale applications.
- Community Support: Supported by a vibrant developer community, TypeScript benefits from frequent updates, bug fixes, and useful extensions.
- On-Going Development: A robust language server furnishes accurate tooling feedback, such as linting and error suggestions in real time.
- Rapid Enhancement: The TypeScript team consistently introduces new features and reinforces existing ones.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Can you explain what is meant by "TypeScript is a superset of JavaScript"?</summary>

### Can you explain what is meant by "TypeScript is a superset of JavaScript"?

**Definition:**

TypeScript is often described as a "superset of JavaScript" because every valid JavaScript code is also a valid TypeScript code.

TypeScript is designed in a way that it fully embraces existing JavaScript syntax and functionality. This ensures a smooth transition for developers wishing to adopt or migrate to TypeScript.

**How it works:**

Key TypeScript Features On Top of JavaScript:

- Type Definitions: TypeScript introduces static typing through type annotations. These are optional, enabling gradual adoption for existing codebases that might not need them.
- Newer JavaScript Features: TypeScript extends JavaScript syntax, providing support for the latest ECMAScript standards more effectively through its compiler, even when the underlying JavaScript engine might not support them yet.
- Tooling and Error Detection: TypeScript offers robust type-checking, increased code readability, and stronger compile-time error detection.

**Examples:**

Code Demonstration. Here is the TypeScript code:

```ts
let num: number = 5;
num = "this will raise a type error";
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>What are the basic types available in TypeScript?</summary>

### What are the basic types available in TypeScript?

**Definition:**

TypeScript provides an assortment of basic types for different kinds of data, such as numbers, strings, boolean values, arrays, tuples and more.

**How it works:**

Common Basic Types in TypeScript:

- Boolean: Represents true/false values.
- Number: Applies to both integer and floating-point numbers.
- String: Refers to textual data.
- Array: Offers a flexible way to work with structured data.
- Tuple: Enables the definition of arrays with a fixed number of elements, each potentially of a different data type.
- Enum: Provides a set of named constants such as days or colors.
- Any: Offers a dynamic type, which can be used to bypass type-checking. It's typically best to be avoided, as it defeats the purpose of using TypeScript, which is primarily focused on static typing. However, there are certain use cases where it becomes necessary.
- Void: Typically used as the return type for functions that don't return a value.
- Null and Undefined: Allow for the assignment of null and undefined values, respectively. However, this isn't enabled by default, and these are probably better handled using the `strict` mode settings in TypeScript.
- Never: Represents the type of values that never occur. For instance, the return type of a function that doesn't reach its end or always throws an error.
- Object: Any JavaScript object.
- Function: Denotes a function type.

**Examples:**

Code Example: Basic TypeScript Types. Here is the TypeScript code:

```ts
// Boolean
let isActive: boolean = true;

// Number
let age: number = 30;

// String
let title: string = "Manager";

// Array
let scores: number[] = [85, 90, 78];
// or use a compact form: let scores: Array<number> = [85, 90, 78];

// Tuple
let employee: [string, number, boolean] = ['John', 35, true];

// Enum
enum WeekDays { Monday, Tuesday, Wednesday, Thursday, Friday }
let today: WeekDays = WeekDays.Wednesday;

// Any
let dynamicData: any = 20;

// Void
function greet(): void {
  console.log("Hello!");
}

// Null and Undefined
let data: null = null;
let user: undefined = undefined;

// Never
function errorMessage(message: string): never {
  throw new Error(message);
}

// Object
let person: object = {
  name: 'John',
  age: 30
};

// Function
let calculate: Function;
calculate = function (x: number, y: number): number {
  return x + y;
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>How do you declare variables in TypeScript?</summary>

### How do you declare variables in TypeScript?

**Definition:**

In TypeScript, variable declarations support different methodologies for declaring variables and their associated types.

**How it works:**

Variable and Type Declaration Methods:

1. var

```ts
var score: number = 100;
```

This declaration can lead to variable hoisting and has global scope or function-level scope.

2. let

Use `let` when you want to define variables within a block scope. This is the recommended default choice:

```ts
let playerName: string = "John";
```

3. const

`const` allows you to declare constants and is especially useful for maintaining data integrity:

```ts
const apiKey: string = "your-api-key-here";
```

4. Function Scope

All three methods (`var`, `let`, and `const`) are confined to their immediate function scope:

```ts
function doSomething() {
  let tempValue: number = 42;
  var result: boolean = true;
}
```

Rules for Variable Declaration and Initialization:

- Order Matters: In TypeScript, a variable must be declared before being used. This is not a requirement in JavaScript, but good JavaScript practice is to declare a variable before using it.

If you're dealing with complex or interconnected codes, it's a good practice to use the `let` and `const` declarations that ensure the block-level scoping, thus helping with potential hoisting issues.

- Static Types: TypeScript requires that you declare the data type of a variable (or let the system infer it) and then initialize it with a value of exactly the same type:

```ts
let count: number;  // Declaration
count = 42;  // Allowed
count = "42";  // Error! Type 'string' is not assignable to type 'number'.
```

- Type Inference: TypeScript can often infer the variable's type based on its initialization value. This reduces the need to specify a type explicitly.

```ts
let word = "hello!";  // TypeScript infers the type as 'string' because of the initialization.
```

**When to use:**

Best Practices for Variable Declarations:

- Use `const` Where You Can: This approach isn't always possible, especially when dealing with object properties. However, favor `const` for better code readability and to prevent accidental data mutations.
- Prefer `let` over `var`: `let` adheres better to block-level scoping and offers more predictability in the code.
- Initialize at Declaration: Although TypeScript allows initializations after declarations, it's best to declare and initialize variables simultaneously to improve code clarity and type safety.
- Prefer Type Annotations: Explicitly specifying variable types can improve code readability. However, when the variable type is obvious from the initialization, type inference should suffice.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>What are Interfaces in TypeScript and how do they work?</summary>

### What are Interfaces in TypeScript and how do they work?

**Definition:**

In TypeScript, an interface defines the structure and types of its members. It acts as a contract for the required properties and methods, ensuring that implementing classes or objects match this structure.

**How it works:**

Key Features of Interfaces:

- Type Consistency: Objects that adhere to an interface's structure are considered compatible with it.
- Optional and Readonly Members: Interfaces allow for optional attributes and readonly members with the `?` and `readonly` keywords respectively.
- Call Signatures: Interfaces can define method types, specifying function parameter and return types.
- Index Signatures: Useful for specifying that an object can have any number of properties, all of a certain type.

**When to use:**

Core Use-Cases:

- Standardizing Objects: Ensuring that disparate objects share a common structure for increased cohesiveness and ease of use.
- Contract Enforcement: Enforcing property and method requirements on classes to reduce errors and improve maintainability.

**Examples:**

Code Example: Basic Interface. Here is the TypeScript code:

```ts
interface Point {
    x: number;
    y: number;
}
  
function printPoint(p: Point) {
    console.log(`Point coordinates: (${p.x}, ${p.y})`);
}

let pointA = { x: 3, y: 7 }; // This object matches Point's structure
let pointB = { x: 8 }; // This object is missing the 'y' property

printPoint(pointA); // Output: Point coordinates: (3, 7)
printPoint(pointB); // Compile-time error due to incorrect structure
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Describe the Enum type and when you might use it.</summary>

### Describe the Enum type and when you might use it.

**Definition:**

The Enum is a data type that simplifies the representation and management of discrete, named values. It's a foundational tool to ensure type safety in TypeScript and a number of vital use-cases:

- Reducing 'Magic Values': When ensuring readability and preventing repetitive literal values, such as `1`, `2`, or `'red'`.
- Configuring Behaviour: influencing functionalities sets of associated values, such as HTTP methods, ordering or customer types.
- Ensuring Type Safety and Efficiency: The predefined set of valid members and a clear data type ensures that value assignments and operations are unequivocal and consistent.

**How it works:**

Core Components:

- Key: a unique identifier, typically a number or string.
- Value: Data associated with the key. If not provided, the key is used as the value.

Standard, String, and Heterogeneous Enums:

- Standard Enum: Every key and value are of the same data type, typically numbers.
- String Enum: All keys and values must be strings, ensuring consistent data representation.
- Heterogeneous Enum: Defines keys with both number or string values. However, due to the mixed-type nature of these enums, it's best to steer clear of them in most cases.

Pragmatic Use of Enums:

While Enums are a powerful tool for maintaining type safety, simplify associating related sets of values.

However, a consideration is that an Enum value can be inferred or forced to be of any key and underlying value type.

**Pluses and minuses:**

Potential Downsides of Enums:

- Compilation Impact: When used in a broader context, or in data structures like arrays or maps, TypeScript generates additional code to convert Enum keys to their associated values.
- Memory Usage: Every usage of an Enum requires memory allocation for its value.

When a simple constant would suffice or if there's a need for a more dynamic relationship between keys and values, detailed types would be a better alternative.

**Examples:**

Code Example: Standard Enum. Here is the TypeScript code:

```ts
enum HttpMethods {
  GET,
  POST,
  PUT,
  DELETE
}

const requestType: HttpMethods = HttpMethods.GET;

// ❌ This is not allowed due to type safety
// const requestType2: HttpMethods = 'GET';
```

In the example, the key `GET` is implicitly assigned the value `0`.

Code Example: String Enum. Here is the TypeScript code:

```ts
enum MediaTypes {
  Image = 'image',
  Video = 'video',
  Audio = 'audio'
}

const selectedType: MediaTypes = MediaTypes.Image;

// ✅ Accessing the value
const associatedText: string = MediaTypes.Image;

// ❌ This is not allowed due to type safety
// const invalidType: MediaTypes = 'image';
```

the Enum helps ensure the proper data type and its values.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>How do you define and use a function in TypeScript?</summary>

### How do you define and use a function in TypeScript?

**Definition:**

When defining a function in TypeScript, you have the following fundamental components to consider:

- Function Signature: Comprising the function's purpose, parameters, type, and return value.
- Function Body: Containing the actual operation or series of steps the function will execute.

**How it works:**

1) Function Declaration

To declare a function, you specify its name, its parameter list, and its return type. If the function doesn't return a value, you set the return type to `void`.

2) Function Expression

You can also declare functions using expressions, which involve assigning functions to variables as values. This approach allows you to be more flexible, such as when you're using callbacks.

3) Optional and Default Parameters

TypeScript supports both optional and default function parameters, enhancing the flexibility of your functions.

Optional Parameters are denoted by a `?` symbol after the parameter name.

Default Parameters are when you assign a default value to a parameter.

4) Use Rest Parameters

You can define a parameter as a "rest" parameter, which means the function can accept any number of arguments for that parameter.

5) Function Overloads

You can declare multiple function overloads to define a set of parameters and their return types for a single function. This feature is especially beneficial when the function's behavior logically varies based on different input types.

6) Call Signature

When using objects in TypeScript, you have the call signature to define the expected function structure for a specific method within the object.

**Examples:**

Here is a code example:

```ts
function greet(name: string): void {
    console.log(`Hello, ${name}!`);
}
```

Here is an example:

```ts
let greet: (name: string) => void;
greet = function(name: string): void {
    console.log(`Hello, ${name}!`);
};
```

Here is the code example:

```ts
function greet(name: string, title?: string) {
    if (title) {
        console.log(`Hello, ${title} ${name}!`);
    } else {
        console.log(`Hello, ${name}!`);
    }
}
```

Here is the code example:

```ts
function greet(name = "Stranger") {
    console.log(`Hello, ${name}!`);
}
```

Here is the code example:

```ts
function introduce(greeting: string, ...names: string[]) {
    console.log(`${greeting}, ${names.join(", ")}!`);
}

introduce("Hello", "Alice", "Bob", "Carol");
```

Here is the code example:

```ts
function specialGreet(name: string): void;
function specialGreet(title: string, name: string): void;

function specialGreet(a: any, b?: any): void {
    if (b) {
        console.log(`Hello, ${a}, ${b}`);
    } else {
        console.log(`Hello, ${a}`);
    }
}
```

Here is a code example:

```ts
type Greeter = {
    (name: string): void
};

let welcome: Greeter;
welcome = function(name: string): void {
    console.log(`Welcome, ${name}!`);
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>What does "type inference" mean in the context of TypeScript?</summary>

### What does "type inference" mean in the context of TypeScript?

**Definition:**

In TypeScript, type inference is a core feature that allows the type of a variable to be automatically determined from its value. This provides the benefits of static typing without the need for explicit type annotations.

**How it works:**

TypeScript employs a best common type algorithm to infer a variable's type. When TypeScript encounters multiple types for a variable during assignment or an array literal, it computes the union of these types and selects the best common type for the variable.

TypeScript can infer the most likely type from the context, such as:

- When a value is assigned immediately, TypeScript assigns the value's type to the variable.
- Type information from adjacent types is used to determine the best common type. If all values are of a compatible type, that type is used.

**Pluses and minuses:**

Benefits of Type Inference:

- Conciseness: Eliminates the need for explicit type declarations, leading to more compact and readable code.
- Adaptability: Codebase types align naturally with values, enhancing maintainability when values change.
- Error Reduction: Reduces the risk of inconsistencies between the declared type and the actual value.

**Examples:**

Code Example: Type Inference. Consider the following code:

```ts
let value = 10; // Type 'number' inferred
let message = "Hello, TypeScript!"; // Type 'string' inferred

function add(a: number, b: number) {
    return a + b;
}

let sum = add(5, 7); // Type 'number' inferred
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Explain the use of 'let' and 'const' in TypeScript.</summary>

### Explain the use of 'let' and 'const' in TypeScript.

**Definition:**

TypeScript makes use of `const` and `let` for variable declaration. These two keywords offer explicitness, scoping, and immutability for efficient code maintenance.

**How it works:**

Core Distinctions:

- const: Designates constants that remain unchanged once declared. It's important to note that this makes the reference immutable but doesn't actively prevent alteration of the internal state for complex objects like arrays.
- let: Initiates variables with standard mutable behavior.

**Examples:**

Code Example: `const`. Here is the TypeScript code:

```ts
const productId: number = 5;
let productName: string = 'Tesla';

const getProductDetails = (id: number): string => {
  return `Product ID: ${id}`;
};

// Attempting to modify will result in a compilation error
// productId = 6;

// Reference is still immutable
const anotherProductId: number = 10;
// This will throw a compilation error since it's a constant
// anotherProductId = 12;

// Modifying internal state of an object is allowed for a const
const myArray: number[] = [1, 2, 3];
myArray.push(4);
```

Code Example: `let`. Here is the TypeScript code:

```ts
let vehicleType: string = 'Car';

if (true) {
  let vehicleType: string = 'Motorcycle';
  console.log(vehicleType);  // Output: Motorcycle
}

console.log(vehicleType);  // Output: Car
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>How do you compile TypeScript files into JavaScript?</summary>

### How do you compile TypeScript files into JavaScript?

**Definition:**

Compiling TypeScript (.ts) into JavaScript (.js) involves integrating a TypeScript compiler (`tsc`). You can customize the compilation process using `tsconfig.json` and even adopt more advanced methods to suit project needs.

**How it works:**

Workflow Steps:

1. File Creation: Write TypeScript files (.ts).
2. Compiler Config: Set up a `tsconfig.json` file with compilation options.
3. Compile: Execute the `tsc` command to initiate the compilation process.
4. Output Verification: Review the generated JavaScript files.

**Examples:**

TypeScript Configuration (`tsconfig.json`). Here is the `tsconfig.json` file. The full configuration guide is available here.

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "strict": true,    
    "outDir": "dist",
    "rootDir": "src"  
  },
  "include": [
    "src/**/*.ts"    
  ],
  "exclude": [      
    "node_modules",   
    "**/*.spec.ts"    
  ]
}
```

Practical Example: Vineyard Residential Task Management App. Here is a practical and comprehensive `tsconfig.json` file.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["dom", "es2015", "es5", "es6", "es7", "es2015.collection"],
    "allowJs": true,
    "checkJs": false,
    "jsx": "react",
    "declaration": false,
    "sourceMap": true,
    "outDir": "dist",  
    "rootDir": "src",
    "strict": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "removeComments": true,
    "suppressImplicitAnyIndexErrors": true,
    "typeRoots": ["node_modules/@types", "custom-typings"],
    "baseUrl": ".",
    "paths": {
      "components/*": ["src/components/*"],
      "utils/*": ["src/utils/*"],
    },
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "incremental": true,
    "diagnostics": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "newLine": "LF",
    "watchOptions": {
      "watchFile": "useFsEvents",  
      "fallbackPolling": "dynamicPriority",   
      "polling": true,
      "esModuleInterop": true,
            "pollingInterval": 2500,
      
      "followSymlinks": true
    }
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "@types"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

**Thoughts:**

Advanced Configuration:

- Project Reference: Useful for code splitting in large projects.
- Custom Transformers: Employ custom logic during the compilation process.
- Programmatic API: Provides flexibility in managing compiler settings and execution.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

### TypeScript Classes and Objects

<details>
<summary>Explain classes in TypeScript. How are they different from ES6 classes?</summary>

### Explain classes in TypeScript. How are they different from ES6 classes?

**Definition:**

While TypeScript and ES6 classes share many similarities, TypeScript's classes offer additional features and strong typing to make your code more robust.

**How it works:**

Key Shared Class Features:

- Inheritance: Subclasses (children) can extend parent classes, inheriting their methods and properties.
- Polymorphism: Derived classes can define methods with the same name as their parent.
- Encapsulation: Data hiding is supported through access modifiers, such as `public`, `private`, and `protected`.
- Constructor: Instantiation starts with a constructor method, if defined.

Unique TypeScript Class Features:

Field Declaration

In TypeScript, you can specify fields directly in the class without initializing them. Automatic initialization to `undefined` occurs during object creation. ES6 requires initializing fields in the constructor or within their declaration.

```ts
class Example {
    // Field is automatically initialized to undefined upon object creation
    someField: string;
}
```

Abstract Classes

TypeScript supports abstract classes to serve as a blueprint for other classes. They cannot be instantiated on their own but can provide some implementation that derived classes can override.

```ts
abstract class AbstractExample {
    abstract someMethod(): void;  // Method has no implementation (abstract)
}
```

Readonly Properties

You can mark class properties as `readonly`, ensuring they are only set upon declaration or within the class constructor.

```ts
class Example {
    readonly id: number;
    constructor(id: number) {
        this.id = id;  // Readonly can only be assigned in the constructor or declaration
    }
}
```

Static Members

Classes in TypeScript support static members, such as properties and methods that belong to the class itself, rather than to instances of the class.

```ts
class Example {
    static count = 0;  // Static property
    static incrementCount() {
        Example.count++;
    }
}
```

Accessor Functions

You can define `get` and `set` functions in TypeScript, known as accessor functions, to control how class properties are accessed and modified.

```ts
class Example {
    private _name: string;

    get name(): string {
        return this._name;
    }

    set name(newName: string) {
        this._name = newName.trim();
    }
}
```

Parameter Properties

TypeScript provides a shortcut to declare a property and initialize it from the constructor parameter. This method can make code more concise, especially when a constructor parameter corresponds directly to a class property.

```ts
class Example {
    constructor(private _name: string, public age: number) {
        // Private _name property created and initialized from constructor parameter
        // Public age property created and initialized from constructor parameter
    }
}
```

Intersection Types for Classes

In TypeScript, when you define a base class and then later extend it, you are creating an intersection type. This means the child class will inherit all the properties and methods from both its parent(s) and itself.

ES6 Additional Features not Present in TypeScript:

Class Expressions

Both ES6 and TypeScript support class expressions, which allows you to define a class without a class name.

In ES6:

```javascript
const Animal = class {
    // Class methods and properties defined here
};
```

Iterator Protocol

ES6 classes support the Iterator protocol, making it easier to iterate over objects.

```ts
class IterableExample implements Iterable<string> {
    // Implement iterator function for strings
    Symbol.iterator {
        let index = 0;
        const data = ['one', 'two', 'three'];
        return {
            next: () => {
                if (index < data.length) {
                    return { value: data[index++], done: false };
                }
                return { value: undefined, done: true };
            }
        };
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>How do you implement Inheritance in TypeScript?</summary>

### How do you implement Inheritance in TypeScript?

**Definition:**

Let's look at how you can use inheritance in TypeScript using both ES6 classes and prototypal inheritance.

**How it works:**

1. Inheritance with ES6 Class Syntax

With the advent of ES6, a more familiar class-based inheritance method was introduced. This method is usually easier to read and understand.

Code Example: Inheritance using ES6 Classes. Here is the TypeScript code:

```ts
class Animal {
  private name: string;

  constructor(theName: string) {
    this.name = theName;
  }

  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends Animal {
  constructor(name: string) {
    super(name);
  }

  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

const mySnake = new Snake("Cobra");
mySnake.move();  // Output: Slithering... Cobra moved 5m.
```

2. Inheritance with Prototypal Methodology

Prior to ES6, TypeScript, like JavaScript, used a prototypal inheritance approach.

The prototypal mechanism can be useful when developing complex object structures, but it is important to be aware of the nuances in order to avoid unexpected behavior.

Code Example: Prototypal Inheritance in TypeScript. Here is the TypeScript code:

```ts
// Define the Parent Class
function Animal(this: Animal, name: string) {
  this.name = name;
}

Animal.prototype.move = function(distanceInMeters: number = 0) {
  console.log(`${this.name} moved ${distanceInMeters}m.`);
};

// Define the Child Class
function Snake(name: string) {
  Animal.call(this, name);
}

// Set up the Inheritance
Snake.prototype = Object.create(Animal.prototype);
Snake.prototype.constructor = Snake;

// Override the Base Type's Method
Snake.prototype.move = function(distanceInMeters = 5) {
  console.log("Slithering...");
  Animal.prototype.move.call(this, distanceInMeters);
};

const mySnake = new Snake("Cobra");
mySnake.move();  // Output: Slithering... Cobra moved 5m.
```

Simplified Methods

Using the prototypal construct can be perplexing at first, but its strength lies in its flexibility.

You can avoid the complexities of direct prototype assignments using ES5 derived constructions, as seen next:

```ts
function Animal(name: string) {
  this.name = name;
}

Animal.prototype.move = function(distanceInMeters: number = 0) {
  console.log(`${this.name} moved ${distanceInMeters}m.`);
};

function Snake(name: string) {
  Animal.call(this, name);
}

// Utilize `Object.create` for simplified prototype delegation
Snake.prototype = Object.create(Animal.prototype);
Snake.prototype.constructor = Snake;

Snake.prototype.move = function(distanceInMeters = 5) {
  console.log("Slithering...");
  Animal.prototype.move.call(this, distanceInMeters);
};
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>What are access modifiers and how do they work in TypeScript?</summary>

### What are access modifiers and how do they work in TypeScript?

**Definition:**

Access modifiers are TypeScript's way of controlling class member visibility and mutability. They enforce encapsulation and are especially useful for object-oriented design.

**How it works:**

Key Modifiers:

- Public: Default for class members. They are accessible from both inside and outside the class.
- Protected: Members can be accessed within the class and its subclasses. They help establish the "is-a" relationship.
- Private: Marks members as accessible only within the declaring class. This ensures they're not modified or accessed externally.

**Examples:**

Code Example: Access Modifiers in Action. Here is the TypeScript code:

```ts
class Person {
    public name: string;
    private age: number;
    protected contact: string;

    constructor(name: string, age: number, contact: string) {
        this.name = name;
        this.age = age;
        this.contact = contact;
    }
}

class Employee extends Person {
    private employeeId: string;

    constructor(name: string, age: number, contact: string, employeeId: string) {
        super(name, age, contact);
        this.employeeId = employeeId;
    }

    public displayDetails(): void {
        console.log(`${this.name} - ${this.age} - ${this.contact} - ${this.employeeId}`);
    }
}

// Somewhere in your code
const person = new Person("John Doe", 30, "1234567");
console.log(person.name);  // Accessible
console.log(person.age);   // ERROR: 'age' is private

const employee = new Employee("Jane Doe", 25, "2345678", "E123");
console.log(employee.contact);  // ERROR: 'contact' is protected
employee.displayDetails();     // Correctly displays details

employee.age = 35;    // ERROR: 'age' is private
employee.contact = "3456789";  // ERROR: 'contact' is protected
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Discuss Abstract classes and their purposes in TypeScript.</summary>

### Discuss Abstract classes and their purposes in TypeScript.

**Definition:**

In TypeScript, abstract classes serve as blueprints that guide derived classes, essentially laying out the structure without necessarily providing complete implementations of methods.

**How it works:**

Core Features of Abstract Classes:

Method Signatures

Abstract classes define method signatures without specifying their functionality. This feature provides a comprehensive form for derived classes to work from.

Specific Method Definitions

In addition to method signatures, abstract classes can contain completely implemented methods. These methods either support the abstract methods or serve as independent functionalities.

Abstract and Non-Abstract Members Separation

Abstract classes clearly demarcate between methods that require implementation by derived classes and those that are either fully implemented or optional.

TypeScript Utility: Static Properties

Abstract classes in TypeScript can have static members, which belong to the class itself and not to any specific instance. This feature provides a convenient way to define properties or methods that are accessible without the need for class instantiation.

**When to use:**

Common Use-Cases for Abstract Classes:

- Facilitate Reusability: Abstract classes help in consolidating common or shared functionalities among several derived classes.
- Contract Enforcement: They ensure that derived classes conform to a shared structure, guaranteeing a defined set of methods that must be implemented.
- Partial Implementations: Abstract classes allow for a mix of fully implemented methods alongside those requiring concrete implementations in derived classes.

**Examples:**

Code Example: Abstract Class. Here is the TypeScript code:

```ts
abstract class Shape {
    abstract getArea(): number;
    abstract getPerimeter(): number;
    color: string;

    constructor(color: string) {
        this.color = color;
    }

    static defaultColor: string = 'red';

    describe() {
        return `This shape is ${this.color}.`;
    }
}

// This will throw an error because the derived class does not provide concrete implementations for abstract methods.
class Circle extends Shape { 
    constructor(public radius: number, color: string) {
        super(color);
    }

    // The 'Circle' class inherited the following properties from 'Shape', but neither implements nor specifies them in the derived class: 'getArea' and 'getPerimeter'.
    getArea(): number {
        return Math.PI * this.radius ** 2;
    }

    getPerimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

const myCircle = new Circle(5, 'blue');
console.log(myCircle.getArea()); // Outputs: 78.54
console.log(myCircle.describe()); // Outputs: This shape is blue.
console.log(Shape.defaultColor); // Outputs: red
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>Can you describe the use of Constructors within TypeScript classes?</summary>

### Can you describe the use of Constructors within TypeScript classes?

**Definition:**

TypeScript provides a convenient way to define constructors for classes using the `constructor` keyword. A constructor method allows you to initialize class members and can have access specifiers. They are useful for setting up an object's initial state.

**How it works:**

Key Features:

- Automatic Invocation: The constructor is automatically called when an object of the class is instantiated.
- Single Unique Constructor: A class can only have one constructor, providing a centralized place for initialization.
- Overload Capabilities: You can overload a constructor to define multiple ways of object initialization.

Example: Constructor in TypeScript. We use the `this` keyword to refer to the current instance, ensuring proper data assignment.

**Examples:**

```ts
class Person {
  // Member variables
  name: string;
  age: number;
  
  // Constructor
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
```

**Thoughts:**

Constructor Access Modifiers

TypeScript supports access modifiers on constructor parameters, enabling concise and safe class initialization.

- Public: Parameters without a modifier are public by default.
- Private: Adding the `private` keyword makes them accessible within the class only.
- Read-Only: Combining `readonly` with parameter and the private or public access modifier ensures the parameter is assigned a value just once, in the constructor.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/typescript-interview-questions/

</details>

<details>
<summary>What are Property Decorators and how do you use them?</summary>

### What are Property Decorators and how do you use them?

**Definition:**

A property decorator is a function placed with `@` on a class field. In current TypeScript it runs when the class is defined, receives the field and a context object, and can record metadata about that field. New code does not need `--experimentalDecorators`.

**How it works:**

- **Placement:** `@mark` sits immediately above a field such as `name = "Ada"`. The same `@` form also works on methods, getters, setters, auto-accessors, and the class itself. Parameter decorators are not part of this model.
- **Call shape:** The function is called with the decorated value and a context. For a field, type the context as `ClassFieldDecoratorContext`. The docs use that context for `name`, `static`, `private`, and `metadata`.
- **Metadata:** Every decorator on a class shares one `context.metadata` object. After the class is decorated, read it from the constructor with `Symbol.metadata`. Most runtimes still need a `Symbol.metadata` polyfill, `target` of `es2022` or below, and `"esnext.decorators"` in `lib`.
- **Legacy flag:** `--experimentalDecorators` still exists and type-checks a different, older decorator shape (`target`, `propertyKey`). A decorator written for that flag usually does not work unchanged without it. The new model is also not compatible with `--emitDecoratorMetadata`.

**When to use:**

- Mark public instance fields for serialization, debugging, or a small framework registry.
- Skip them when a plain field or a method is enough. Use the legacy flag only if the project already depends on experimental decorators, such as older Angular or Nest metadata.

**Examples:**

```ts
function mark(_target: any, context: ClassFieldDecoratorContext): void {
  context.metadata[context.name] = true;
}

class User {
  @mark
  name = "Ada";
}

console.log(User[Symbol.metadata]);
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)
- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)

</details>

<details>
<summary>How do getters and setters work in TypeScript?</summary>

### How do getters and setters work in TypeScript?

**Definition:**

Getters and setters are `get` and `set` accessors on a class or object. Callers read and write them like a property, and the accessor body runs on each access.

**How it works:**

- **Pair:** `get size()` returns the stored value. `set size(value)` receives the assignment. A get/set pair that only forwards to a field, with no extra logic, is rarely worth writing. A public field is enough.
- **Inference:** If there is a getter and no setter, the property is `readonly`. If the setter parameter has no type, TypeScript takes it from the getter’s return type.
- **Different read and write types:** Since TypeScript 4.3 the setter may accept a wider type than the getter returns. The getter’s type must still be assignable to the setter, so the property can be assigned to itself.
- **Which type is used:** Relationships between properties use the reading type. The writing type is checked only on an assignment. The same split works on object literals and on interfaces (`get size(): number` and `set size(value: ...)`).

**When to use:**

- Hide a private field and validate or convert the value on the way in, such as turning a string into a number.
- Expose a computed value. Prefer a public field when there is no logic to add.

**Examples:**

```ts
class Thing {
  #size = 0;

  get size(): number {
    return this.#size;
  }

  set size(value: string | number | boolean) {
    const num = Number(value);
    this.#size = Number.isFinite(num) ? num : 0;
  }
}

const thing = new Thing();
thing.size = "12";
const reading: number = thing.size;
```

**Sources:**

- [TypeScript handbook — Classes, Getters / Setters](https://www.typescriptlang.org/docs/handbook/2/classes.html#getters--setters)
- [TypeScript 4.3 — Separate Write Types on Properties](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-3.html#separate-write-types-on-properties)

</details>

<details>
<summary>Explain Method Overloading in TypeScript.</summary>

### Explain Method Overloading in TypeScript.

**Definition:**

Overload signatures describe the distinct ways a function or method may be called. Callers see only those signatures. One implementation signature sits underneath and holds the body. That implementation signature is not part of the public call list.

**How it works:**

- **Order:** Write two or more overload signatures, then the implementation. The implementation must be compatible with every overload. Its parameter and return types are not what callers are allowed to use.
- **Resolution:** TypeScript picks a single overload for each call. A value typed as a union of the overload inputs often matches none of them, even when each member would be fine alone.
- **Prefer a union:** If the overloads take the same number of arguments and return the same type, one signature with a union parameter is easier to call and to implement. The handbook’s rule is to prefer union parameters over overloads when that works.
- **Classes:** The same pattern applies to methods and to constructor overload signatures. At runtime only the implementation exists. The extra signatures are erased.

**When to use:**

- A function really has different argument counts or different return types, such as one argument versus three.
- Do not overload `len(s: string): number` and `len(arr: unknown[]): number`. Write `len(x: string | unknown[]): number` instead.

**Examples:**

```ts
function makeDate(timestamp: number): Date;
function makeDate(month: number, day: number, year: number): Date;
function makeDate(monthOrTimestamp: number, day?: number, year?: number): Date {
  if (day !== undefined && year !== undefined) {
    return new Date(year, monthOrTimestamp, day);
  }
  return new Date(monthOrTimestamp);
}

const fromStamp = makeDate(1_234_567_890);
const fromParts = makeDate(4, 10, 2026);
```

**Sources:**

- [TypeScript handbook — Function Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)

</details>

<details>
<summary>What is the purpose of the 'static' keyword in TypeScript Class?</summary>

### What is the purpose of the 'static' keyword in TypeScript Class?

**Definition:**

`static` puts a field, method, accessor, or initialization block on the class constructor. It is shared by the class, not stored on each instance.

**How it works:**

- **Access:** Read and call static members on the constructor: `Counter.next()`. They are not instance properties. Subclasses inherit them, so `Derived.getGreeting()` sees `Base.getGreeting`.
- **Visibility:** `public`, `protected`, and `private` apply to static members the same way they apply to instance members.
- **Static blocks:** A `static { ... }` block runs while the class is initializing, in source order. It has its own scope and can read private static fields. A class may contain several blocks.
- **Reserved names:** Do not declare static `name`, `length`, or `call`. Those clash with properties on `Function`. TypeScript has no C# style `static class`. A single shared object or a module is the usual replacement.

**When to use:**

- Counters, caches, and factory helpers that belong to the type rather than to one object.
- Keep per-object data on instance fields. If nothing needs `new`, export a plain object instead of a class that only has static members.

**Examples:**

```ts
class Counter {
  static #count = 0;

  static {
    Counter.#count = 1;
  }

  static next(): number {
    return ++Counter.#count;
  }
}

console.log(Counter.next());
```

**Sources:**

- [TypeScript handbook — Static Members](https://www.typescriptlang.org/docs/handbook/2/classes.html#static-members)
- [TypeScript 4.4 — Static Blocks in Classes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-4.html#static-blocks-in-classes)
- [MDN — static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)

</details>

### TypeScript Types and Type Assertions

<details>
<summary>How do you define custom types with Type Aliases?</summary>

### How do you define custom types with Type Aliases?

**Definition:**

A type alias gives a name to any type: an object shape, a union, a tuple, a primitive, or another alias. Using the name is the same as writing the type it points at. It does not create a distinct runtime brand.

**How it works:**

- **Syntax:** `type ID = number | string` or `type Point = { x: number; y: number }`.
- **Aliases are transparent:** `type UserInputSanitizedString = string` is still `string`. You can assign a plain `string` to it.
- **Interfaces:** An `interface` also names an object type, and TypeScript checks structure, not the name. An interface can be reopened and merged. A type alias cannot be declared twice. Interfaces describe object shapes. A type alias can also name a primitive or a union.
- **Extending:** An interface uses `extends`. A type alias combines members with `&`. The handbook’s default is to use `interface` until you need something only `type` can express. `extends` is often cheaper for the compiler than a large intersection.

**When to use:**

- Name a union, a tuple, a mapped type, or a conditional type.
- Name an object when you want a single declaration that will not be merged later. Prefer an interface for a public object shape that other files might augment.

**Examples:**

```ts
type ID = number | string;

type Point = {
  x: number;
  y: number;
};

function printCoord(pt: Point) {
  console.log(pt.x, pt.y);
}

printCoord({ x: 100, y: 100 });
```

**Sources:**

- [TypeScript handbook — Type Aliases](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)
- [TypeScript handbook — Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

</details>

<details>
<summary>Can you explain Union types and provide an example?</summary>

### Can you explain Union types and provide an example?

**Definition:**

A union `A | B` is a value that may be any one of the member types. A leading `|` is allowed, so members can sit on their own lines.

**How it works:**

- **Providing a value:** Any member is a valid value. `printId(101)` and `printId("202")` both fit `number | string`. An object does not.
- **Using a value:** TypeScript allows an operation only when it is valid for every member. `string | number` has no `toUpperCase`, because `number` does not. Shared members, such as `slice` on `string | string[]`, are available without narrowing.
- **Narrowing:** A `typeof` check, `Array.isArray`, or another guard splits the union. In the `else` branch, the remaining members are what is left.
- **Properties:** A union of objects exposes the intersection of their properties. That follows from the set of values: only facts true of every member apply to the whole union.

**When to use:**

- A parameter or return value really has more than one shape, such as an id that is a number or a string.
- Prefer one union parameter over several overloads when the call shapes share a count and a return type.

**Examples:**

```ts
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}

printId(101);
printId("202");
```

**Sources:**

- [TypeScript handbook — Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)

</details>

<details>
<summary>How do Intersection types work in TypeScript?</summary>

### How do Intersection types work in TypeScript?

**Definition:**

An intersection `A & B` is a value that is both `A` and `B`. For object types it is the usual way to combine members into one type.

**How it works:**

- **Members add up:** `Colorful & Circle` has `color` and `radius`. A value must supply both.
- **Conflicts:** If the same property has incompatible types, the property’s type becomes an intersection of those types. `string & number` on one property is `never`. The declaration still compiles. The error shows up when you try to use it.
- **Interfaces:** Two `interface` declarations with the same name merge when the properties are compatible, and error when they are not. An intersection merges conflicting properties into `never` instead of erroring at the declaration.
- **`extends`:** An interface `extends` clause copies members and signals that the types are related. Use that when you are extending an object. Use `&` when you are combining types that are not both interfaces, including unions and primitives.

**When to use:**

- Add fields to an existing object type without repeating them: `type Bear = Animal & { honey: boolean }`.
- Mix a data type with a behavior type, such as a colorful circle. Avoid intersecting properties that cannot be true at the same time.

**Examples:**

```ts
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

function draw(circle: ColorfulCircle) {
  console.log(circle.color, circle.radius);
}

draw({ color: "blue", radius: 42 });
```

**Sources:**

- [TypeScript handbook — Intersection Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)
- [TypeScript handbook — Interface Extension vs. Intersection](https://www.typescriptlang.org/docs/handbook/2/objects.html#interface-extension-vs-intersection)

</details>

<details>
<summary>What are Tuple types and when should they be used?</summary>

### What are Tuple types and when should they be used?

**Definition:**

A tuple is an array type with a fixed length and a known type at each index. `[string, number]` means index `0` is a `string` and index `1` is a `number`. It has no special runtime representation.

**How it works:**

- **Length:** Reading past the last index is an error. Destructuring works like a normal array.
- **Optional elements:** A `?` is allowed only at the end: `[number, number, number?]`. `length` is then `2 | 3`.
- **Rest elements:** `...boolean[]` may sit at the start, middle, or end. That tuple has no single `length`. The same shape can type a rest parameter, so `[string, number, ...boolean[]]` matches `(name: string, count: number, ...flags: boolean[])`.
- **`readonly`:** `readonly [string, number]` rejects writes. `as const` on an array literal infers a readonly tuple. A readonly tuple is not assignable to a mutable tuple.

**When to use:**

- A small ordered pair whose positions are fixed, such as coordinates, or a rest-parameter list.
- Prefer an object with named fields when a caller has to remember what index `0` means. Labels such as `[start: number, end: number]` are for docs and tooling. They do not change assignability.

**Examples:**

```ts
type StringNumberPair = [string, number];

function logPair(pair: StringNumberPair) {
  const [text, count] = pair;
  console.log(text, count);
}

logPair(["hello", 42]);
```

**Sources:**

- [TypeScript handbook — Tuple Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types)
- [TypeScript 4.0 — Labeled Tuple Elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-0.html#labeled-tuple-elements)

</details>

<details>
<summary>Explain Type Assertions and why they are useful.</summary>

### Explain Type Assertions and why they are useful.

**Definition:**

A type assertion tells the checker to treat a value as a more specific or less specific type. It is erased at compile time and does not check or convert the value at runtime.

**How it works:**

- **Syntax:** `document.getElementById("main_canvas") as HTMLCanvasElement`. The angle-bracket form `<HTMLCanvasElement>document.getElementById("main_canvas")` is the same, except in `.tsx`, where it clashes with JSX.
- **Overlap:** The source and target must overlap. `"hello" as number` is an error. If you really mean it, assert through `unknown` (or `any`) first: `value as unknown as number`.
- **No runtime effect:** A wrong assertion does not throw and does not produce `null`. The next property access can fail later.
- **Non-null assertion:** The postfix `!` is a separate operator. It removes `null` and `undefined` from the type and is also erased.

**When to use:**

- You know a DOM node, a parsed payload, or a migrated value is narrower than the declared type, and a type guard is impractical.
- Prefer narrowing (`typeof`, `in`, a predicate) when the check can live in code. An assertion is a claim the compiler cannot verify.

**Examples:**

```ts
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;

const context = myCanvas.getContext("2d");
```

**Sources:**

- [TypeScript handbook — Type Assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)
- [TypeScript handbook — Non-null Assertion Operator](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-)

</details>

<details>
<summary>How do you use the 'typeof' type guard in TypeScript?</summary>

### How do you use the 'typeof' type guard in TypeScript?

**Definition:**

`typeof` in an expression is the JavaScript operator. Inside `if (typeof value === "string")` and similar checks, TypeScript narrows the union to the matching member.

**How it works:**

- **String check:** For `string | number`, the true branch is `string` and the else branch is `number`. Methods such as `toUpperCase` are then allowed only on the string branch.
- **What it distinguishes:** The comparisons used for narrowing are `"string"`, `"number"`, `"bigint"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, and `"function"`.
- **Objects and arrays:** `typeof null` is `"object"`, and so is `typeof` on an array. Split arrays with `Array.isArray`. After that check, the else branch is the other union member.
- **Predicates:** A function that returns `x is number` is a different guard. TypeScript 5.5 can infer that predicate from a simple `typeof` or `!= null` check. `typeof` in a type position (`typeof someValue`) is the type operator, not this guard.

**When to use:**

- Narrow a union of primitives before calling a method that exists on only one member.
- Use `Array.isArray`, `in`, or `instanceof` when `typeof` would report `"object"` for several members.

**Examples:**

```ts
function formatCommandline(input: string | string[]) {
  if (typeof input === "string") {
    return input.trim();
  }
  return input.join(" ");
}
```

**Sources:**

- [TypeScript handbook — Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types)
- [MDN — typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<details>
<summary>Can you create types from existing data using Type Inference?</summary>

### Can you create types from existing data using Type Inference?

**Definition:**

Yes. Write `typeof` in a type position to copy the type of a variable or a property. You do not restate the shape by hand. This is the type operator, separate from the `typeof` value check.

**How it works:**

- **Identifiers only:** `typeof` in a type is legal on a name or its properties, such as `typeof user` or `typeof user.name`. An arbitrary call or expression is rejected, so the type position cannot look like code that runs.
- **Values are not types:** `ReturnType<getUser>` is an error because `getUser` is a value. The fix the error suggests is `ReturnType<typeof getUser>`.
- **Inference still widens:** `let name = "Ada"` is typed as `string`. `const name = "Ada"` keeps the literal `"Ada"`. `as const` on an array or object keeps literals, marks properties readonly, and turns the array into a readonly tuple. `typeof` then copies that narrower type.
- **Return type:** `ReturnType<typeof getUser>` takes the function’s type and produces what the function returns.

**When to use:**

- A config object, a function, or a class instance already exists and you want a type that tracks it.
- Keep a hand-written alias when the public type should be wider than today’s value, or when the value is not in scope.

**Examples:**

```ts
function getUser() {
  return { id: 1, name: "Ada" };
}

type User = ReturnType<typeof getUser>;

const point = [3, 4] as const;
type Point = typeof point;
```

**Sources:**

- [TypeScript handbook — Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)
- [TypeScript 3.4 — const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

</details>

### TypeScript Generics

<details>
<summary>What are Generics in TypeScript and why are they used?</summary>

### What are Generics in TypeScript and why are they used?

**Definition:**

A generic is a type parameter: a placeholder for a type the caller supplies. The component works for many types and still remembers which type flowed in, which `any` does not.

**How it works:**

- **Type variable:** `function identity<Type>(arg: Type): Type` captures the argument type and uses it as the return type. The information goes in one side and comes out the other.
- **Call sites:** Pass the argument explicitly, `identity<string>("myString")`, or let inference set `Type` from the value. Inference is the common case. Explicit arguments are for when inference cannot see the type.
- **Inside the body:** `Type` stands for every possible type. You cannot read `.length` unless a constraint says the value has it.
- **Where they appear:** Functions, interfaces, classes, and type aliases can take type parameters. Enums and namespaces cannot. A generic class is generic on the instance side only. Static members cannot use the class type parameter.

**When to use:**

- The same function or container should work for strings, numbers, and future types without copying the implementation or returning `any`.
- Skip a type parameter when it appears only once and nothing relates two positions. A plain `string | number` is simpler there.

**Examples:**

```ts
function identity<Type>(arg: Type): Type {
  return arg;
}

const text = identity("myString");
const explicit = identity<string>("myString");
```

**Sources:**

- [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

<details>
<summary>How do you create a generic function?</summary>

### How do you create a generic function?

**Definition:**

Put the type parameters in angle brackets after the function name, then use those names in the parameters, the return type, or both.

**How it works:**

- **Declaration:** `function identity<Type>(arg: Type): Type`. The type parameter list comes first, then the value parameters.
- **Arrays of the parameter:** `arg: Type[]` or `arg: Array<Type>` means the function takes and returns an array, so `.length` is valid and `Type` is the element type.
- **Function types:** A generic function type lists the parameters first: `type Identity = <Type>(arg: Type) => Type`. The names may differ as long as the count and the positions match.
- **Inference:** A call without `<...> ` lets the checker set the type arguments from the values. Write them yourself when there is no argument to infer from.

**When to use:**

- Relate an input to an output, such as “returns the same type it receives” or “returns the property named by this key”.
- Use a union or an overload when the set of inputs is a fixed list, not an open type parameter.

**Examples:**

```ts
function first<Type>(items: Type[]): Type | undefined {
  return items[0];
}

const name = first(["Ada", "Grace"]);
```

**Sources:**

- [TypeScript handbook — Hello World of Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html#hello-world-of-generics)
- [TypeScript handbook — Generic Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#generic-functions)

</details>

<details>
<summary>Discuss how Generic Interfaces can be defined.</summary>

### Discuss how Generic Interfaces can be defined.

**Definition:**

A generic interface declares one or more type parameters on the interface, or on a single call signature inside it. Callers fill those parameters when they use the interface.

**How it works:**

- **On the interface:** `interface Box<Type> { contents: Type }` makes every member share `Type`. Using it requires an argument: `Box<string>`. The parameter is locked for that value.
- **On the call signature:** `interface IdentityFn { <Type>(arg: Type): Type }` describes a generic function. Each call can pick a new `Type`. The interface itself is not generic.
- **Which one:** Put the parameter on the interface when the whole object should agree on one type, as with `Box<string>`. Put it on the call signature when each call is independent.
- **Object aliases:** `type Box<Type> = { contents: Type }` is the type-alias form of the same idea. An interface can also `extend` another generic interface.

**When to use:**

- A container, a response, or a dictionary whose payload type varies: `Box<User>`, `Page<Item>`.
- A function-shaped interface when you are typing the function itself rather than a box of one type.

**Examples:**

```ts
interface Box<Type> {
  contents: Type;
}

const box: Box<string> = { contents: "hello" };

interface IdentityFn {
  <Type>(arg: Type): Type;
}
```

**Sources:**

- [TypeScript handbook — Generic Types](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-types)
- [TypeScript handbook — Generic Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#generic-object-types)

</details>

<details>
<summary>Explain how Generics work with Classes.</summary>

### Explain how Generics work with Classes.

**Definition:**

A generic class puts a type-parameter list after the class name. Instance fields and methods share that parameter. Static members do not see it.

**How it works:**

- **Shape:** `class Box<Type> { contents: Type; }` is the class form of a generic interface. `new Box<string>()` or inference from the constructor argument sets `Type`.
- **One parameter for the instance:** Fields and methods on `this` all use the same `Type`. That is why a `GenericNumber<number>` cannot add strings through the same `add` method.
- **Static side:** A class type has a static side and an instance side. The type parameter belongs to the instance side. A static method must declare its own type parameter if it needs one.
- **Constructors:** A factory that creates instances types the class as a constructor, such as `{ new (): Type }`. The handbook uses that form for mixins. A constraint still uses `extends`, the same way it does on a function.

**When to use:**

- A class holds or produces a value whose type should follow the instance: a box, a repository, a typed event target.
- If the class has no per-instance type to track, a normal class is enough.

**Examples:**

```ts
class Box<Type> {
  constructor(public contents: Type) {}

  get(): Type {
    return this.contents;
  }
}

const box = new Box("hello");
const value: string = box.get();
```

**Sources:**

- [TypeScript handbook — Generic Classes](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-classes)

</details>

<details>
<summary>Can you provide an example of a generic constraint?</summary>

### Can you provide an example of a generic constraint?

**Definition:**

A constraint limits a type parameter with `extends`. The function may use only what the constraint guarantees, and callers may pass only types that have those members.

**How it works:**

- **Member constraint:** `Type extends Lengthwise` means `Type` has a `length: number`. `arg.length` is then legal. `loggingIdentity(3)` fails because `number` has no `length`. A string or `{ length: 10, value: 3 }` is accepted, and the return type stays the specific argument type.
- **Another type parameter:** `Key extends keyof Type` means `Key` is a property name of `Type`. `obj[key]` is then `Type[Key]`, and a missing name is an error.
- **What is not allowed:** Without the constraint, the body must treat `Type` as every possible type, so `.length` is an error even if today’s caller passes an array.

**When to use:**

- The body needs a property, a method, or a key, and you still want the concrete argument type preserved.
- If every caller has the same concrete type, drop the type parameter and name that type.

**Examples:**

```ts
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length);
  return arg;
}

loggingIdentity("hello");
loggingIdentity({ length: 10, value: 3 });
```

**Sources:**

- [TypeScript handbook — Generic Constraints](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-constraints)
- [TypeScript handbook — Using Type Parameters in Generic Constraints](https://www.typescriptlang.org/docs/handbook/2/generics.html#using-type-parameters-in-generic-constraints)

</details>

### TypeScript Advanced Types and Techniques

<details>
<summary>What is a Discriminated Union in TypeScript?</summary>

### What is a Discriminated Union in TypeScript?

**Definition:**

A discriminated union is a union of object types that share one property, the discriminant, whose value is a different literal in each member. Checking that property narrows the whole object.

**How it works:**

- **Shape:** Each member has the same key, usually `kind` or `type`, with a string (or numeric) literal type. The other fields differ.
- **Narrowing:** `switch (shape.kind)` or `if (shape.kind === "square")` makes `shape` that member inside the branch. The extra fields are then available without an assertion.
- **Leftovers:** After one or more members are ruled out, the else branch is the remaining union. If every member is handled, nothing remains.
- **Why the literal matters:** A shared `kind: string` does not discriminate. The checker needs a literal it can compare.

**When to use:**

- Model a result, an action, or a shape that has several exclusive variants, such as square, rectangle, and circle.
- Prefer this over optional fields that only make sense together. The discriminant keeps the illegal combinations out of the type.

**Examples:**

```ts
interface Square {
  kind: "square";
  size: number;
}

interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Circle;

function area(shape: Shape): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "circle":
      return Math.PI * shape.radius * shape.radius;
  }
}
```

**Sources:**

- [TypeScript 2.0 — Discriminated Unions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#discriminated-unions)

</details>

<details>
<summary>How do you declare a Readonly type?</summary>

### How do you declare a Readonly type?

**Definition:**

`readonly` on a property tells the checker that property cannot be reassigned. It is erased and does not freeze the value at runtime. `Readonly<Type>` applies that modifier to every property.

**How it works:**

- **One property:** `readonly prop: string` allows a read and rejects `obj.prop = "hello"`.
- **Nested values:** `readonly` stops rewriting that property. It does not stop changes inside the object it points at. `home.resident.age = 43` is still allowed when only `resident` is `readonly`.
- **Alias:** TypeScript does not treat `readonly` as part of assignability in the way people expect. A mutable alias of the same object can still write the property.
- **The utility:** `Readonly<Type>` is the mapped form `{ readonly [P in keyof Type]: Type[P] }`. `Object.freeze` is typed as returning `Readonly<Type>`. Arrays use `readonly Type[]` or `ReadonlyArray<Type>`.

**When to use:**

- Signal that a function only reads a config object or a tuple.
- Use `readonly` on the array or tuple when a caller must not push or replace elements. It still will not stop a mutable alias unless the value is actually frozen in JavaScript.

**Examples:**

```ts
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = { title: "Delete inactive users" };

function visit(home: { readonly resident: { name: string; age: number } }) {
  home.resident.age += 1;
}
```

**Sources:**

- [TypeScript handbook — readonly Properties](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-properties)
- [TypeScript handbook — Readonly](https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlytype)

</details>

<details>
<summary>Can you describe the use of Mapped types?</summary>

### Can you describe the use of Mapped types?

**Definition:**

A mapped type builds a new object type by walking a union of keys, usually `keyof Type`, and writing a property for each key.

**How it works:**

- **Form:** `{ [Property in keyof Type]: boolean }` copies the keys of `Type` and sets every value to `boolean`.
- **Modifiers:** `readonly` and `?` can be added or removed. A prefix `+` adds (and is the default). A prefix `-` removes: `-readonly` or `-?`. `Required<T>` is the mapped type that removes `?`.
- **Key remapping:** Since TypeScript 4.1, `as` renames keys: `[Property in keyof Type as \`get${Capitalize<string & Property>}\`]`. A conditional that produces `never` drops that key.
- **Other unions:** The `in` clause can walk a union that is not only `string | number | symbol`, for example a union of event objects keyed by `kind`.

**When to use:**

- Derive a flags object, a partial update, a readonly view, or getters from a type you already have.
- Do not hand-copy a large interface when the relationship is “same keys, different values”.

**Examples:**

```ts
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

interface Features {
  darkMode: () => void;
  newUserProfile: () => void;
}

type FeatureOptions = OptionsFlags<Features>;
```

**Sources:**

- [TypeScript handbook — Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

</details>

<details>
<summary>Explain Conditional types in TypeScript.</summary>

### Explain Conditional types in TypeScript.

**Definition:**

A conditional type picks one of two types: `SomeType extends OtherType ? TrueType : FalseType`. The true branch is chosen when the left type is assignable to the right type.

**How it works:**

- **With generics:** A fixed check such as `Dog extends Animal` is just `number` or `string`. The useful case is a type parameter, so one function signature can replace a pile of overloads.
- **Narrowing the parameter:** In the true branch, `T extends { message: unknown }` means `T["message"]` is allowed. The false branch can be `never`.
- **`infer`:** `T extends (infer Item)[] ? Item : T` declares a type variable for the piece you matched. `ReturnType` is built this way. For an overloaded function, inference uses the last signature.
- **Distribution:** A naked type parameter distributes over a union. `ToArray<string | number>` becomes `string[] | number[]`. Wrapping both sides in a one-tuple, `[T] extends [any]`, checks the union as a whole and produces `(string | number)[]`.

**When to use:**

- The output type depends on the input type: unwrap an array, choose a label type from `string | number`, extract a return type.
- Prefer a plain union when both branches are the same shape and no `infer` is required.

**Examples:**

```ts
type Flatten<T> = T extends any[] ? T[number] : T;

type Str = Flatten<string[]>;
type Num = Flatten<number>;
```

**Sources:**

- [TypeScript handbook — Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

</details>

<details>
<summary>What are Index types and how do you use the keyof keyword?</summary>

### What are Index types and how do you use the keyof keyword?

**Definition:**

`keyof Type` is the union of that type’s property names. An indexed access `Type[Key]` is the type of the property named `Key`. Together they type “give me this object’s field by name”.

**How it works:**

- **`keyof`:** For `{ foo: number; bar: string }`, `keyof` that type is `"foo" | "bar"`. The handbook calls this an index type query.
- **Lookup:** `Type[Key]` reads one property. If `Key` is a union, the result is a union of those property types.
- **Constraint:** `function getProperty<T, K extends keyof T>(obj: T, key: K)` returns `T[K]`. A key that is not on `T` is an error, and the value you write back must be `T[K]`.
- **Intersections:** `keyof (A & B)` simplifies to `keyof A | keyof B`.

**When to use:**

- A helper that reads or writes a property by a key argument, or a mapped type that visits every key.
- An index signature (`[key: string]: Value`) is the other tool: it describes unknown names. `keyof` describes the names you already declared.

**Examples:**

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "Ada", age: 36 };
const name: string = getProperty(user, "name");
```

**Sources:**

- [TypeScript 2.1 — keyof and Lookup Types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#keyof-and-lookup-types)
- [TypeScript handbook — Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

</details>

<details>
<summary>How does Type Casting differ from Type Assertions?</summary>

### How does Type Casting differ from Type Assertions?

**Definition:**

TypeScript’s `as` (and the angle-bracket form) is a type assertion: a compile-time claim that is erased. It does not convert the value. A cast in languages that convert at runtime is a different operation, and TypeScript has no such operator.

**How it works:**

- **Assertion:** `value as HTMLCanvasElement` changes only the type the checker uses. There is no exception and no `null` if the claim is wrong. The two types must overlap, or you assert through `unknown` first.
- **Conversion:** Turning a string into a number is ordinary JavaScript, such as `Number(text)`. The type follows from what that function returns. The assertion does not do this step.
- **Checked narrowing:** A type predicate `value is string`, or an assertion signature `asserts value is string`, runs a real check and then narrows. That is the closest tool to a checked cast, and it is not the `as` operator.
- **`.tsx`:** Angle brackets clash with JSX, so `as` is the form to write in those files.

**When to use:**

- `as` when the checker cannot see a fact you already established outside the type system.
- A predicate or `asserts` function when the program should actually test the value. Use `Number`, `String`, or `Boolean` when you need a converted value.

**Examples:**

```ts
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new Error("Not a string");
  }
}

function upper(value: unknown): string {
  assertIsString(value);
  return value.toUpperCase();
}
```

**Sources:**

- [TypeScript handbook — Type Assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)
- [TypeScript 3.7 — Assertion Functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#assertion-functions)

</details>

<details>
<summary>Discuss the Partial, Required, Readonly, and Pick utility types.</summary>

### Discuss the Partial, Required, Readonly, and Pick utility types.

**Definition:**

These four globals are mapped types. `Partial` makes every property optional, `Required` makes every property required, `Readonly` marks every property `readonly`, and `Pick` keeps only the keys you name.

**How it works:**

- **`Partial<Type>`:** Each property gains `?`. A value may include any subset. Released in 2.1. The definition is `{ [P in keyof Type]?: Type[P] }`.
- **`Required<Type>`:** The opposite, released in 2.8. It removes `?` from every property, so a missing property is an error.
- **`Readonly<Type>`:** Every property is `readonly`. Assignment is rejected by the checker. `Object.freeze` is typed as returning this. It still does not deep-freeze nested objects.
- **`Pick<Type, Keys>`:** `Keys` is a string literal or a union of them. The result has only those properties. `Omit<Type, Keys>` is the opposite and arrived in 3.5.

**When to use:**

- `Partial` for a patch or an update form. `Required` when a draft type made fields optional and a finished value needs all of them.
- `Readonly` for a function argument that must not be reassigned. `Pick` for a preview or a list row that should carry only `title` and `completed`.

**Examples:**

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;
type TodoPatch = Partial<Todo>;

const preview: TodoPreview = { title: "Clean room", completed: false };
const patch: TodoPatch = { completed: true };
```

**Sources:**

- [TypeScript handbook — Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

</details>

<details>
<summary>What is the 'never' type and when would it be used?</summary>

### What is the 'never' type and when would it be used?

**Definition:**

`never` is the type of a value that never occurs. A function return type of `never` means the function throws or ends execution. `never` is also what is left of a union after every member has been ruled out.

**How it works:**

- **No return:** A function annotated to return `never` must not finish normally. The handbook’s picture is “throw or terminate”.
- **Exhaustiveness:** In `string | number`, the branch after `typeof x === "string"` and `typeof x === "number"` types `x` as `never`. Assigning that leftover to a `never` parameter flags a union member you forgot.
- **Conditional types:** The false branch of a conditional is often `never`, meaning “this case does not produce a type”. A distributive conditional uses `never` as the empty union member.
- **Assignability:** `never` is assignable to every type. Almost nothing is assignable to `never`, so it is a useful sink for impossible states.

**When to use:**

- Mark a function that always throws.
- Check that a `switch` on a discriminant handled every member.
- Drop keys from a mapped type by remapping them to `never`.

**Examples:**

```ts
function assertNever(value: never): never {
  throw new Error("Unexpected value");
}

function area(shape: { kind: "square"; size: number } | { kind: "circle"; radius: number }): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "circle":
      return Math.PI * shape.radius * shape.radius;
    default:
      return assertNever(shape);
  }
}
```

**Sources:**

- [TypeScript handbook — never](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)

</details>

### TypeScript Modules and Namespaces

<details>
<summary>How do you organize code with Modules in TypeScript?</summary>

### How do you organize code with Modules in TypeScript?

**Definition:**

A module is a file with a top-level `import`, `export`, or top-level `await`. Its declarations stay in that file’s scope until they are exported. A file with none of those is a script, and its names land in the global scope.

**How it works:**

- **ES modules:** `import` / `export` is the syntax the handbook treats as the current format. CommonJS (`module.exports` and `require`) is the other format the handbook covers, because that is how most npm packages are still shipped.
- **Make a file a module:** If a file has no import or export but should not be a script, add `export {}`. That works for any `module` emit target.
- **Three choices:** Which syntax you write, how a module name maps to a file (resolution), and what JavaScript the compiler emits (`module`, separate from `target`).
- **Namespaces are not this:** A namespace is an older TypeScript grouping. The handbook recommends ES modules for new code so the organization matches JavaScript.

**When to use:**

- Any program that should load files as dependencies instead of sharing one global scope.
- Node.js apps, libraries, and browser code built with a bundler. Use a namespace only for the cases in the next question.

**Examples:**

```ts
// hello.ts
export default function helloWorld() {
  console.log("Hello, world!");
}

// app.ts
import helloWorld from "./hello.js";

helloWorld();
```

**Sources:**

- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html)

</details>

<details>
<summary>When would you use Namespaces?</summary>

### When would you use Namespaces?

**Definition:**

A namespace is a TypeScript name for a JavaScript object in the global scope. It can group declarations, including across files that are concatenated with `outFile`. It is not deprecated. For new code the handbook still recommends ES modules.

**How it works:**

- **Global object:** Unlike a module, a namespace does not need a loader. Several files can add to the same namespace, and `outFile` can concatenate them. A page can then load those scripts with `<script>` tags.
- **Dependencies are hidden:** Everything shares the global object, so a large app makes it hard to see which file depends on which.
- **Do not wrap a module:** `export namespace Shapes { export class Triangle {} }` forces callers to write `shapes.Shapes.Triangle`. The module file is already the group, and the importer chooses its name. Export `Triangle` directly.
- **Where they remain:** Complex declaration files, including DefinitelyTyped, still use namespaces. That is a typing-file tool, not the way to structure an application.

**When to use:**

- A global script bundle loaded by script tags, or a `.d.ts` that already uses the namespace pattern.
- New application and library code should be files with `import` and `export`.

**Examples:**

```ts
export class Triangle {
  /* ... */
}

export class Square {
  /* ... */
}
```

**Sources:**

- [TypeScript handbook — Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)
- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescript-namespaces)

</details>

<details>
<summary>What is the difference between Internal and External modules?</summary>

### What is the difference between Internal and External modules?

**Definition:**

“Internal module” is the old name for a namespace. “External module” is the old name for a file module, the kind that uses `import` and `export`. Current docs use those newer names.

**How it works:**

- **Internal / namespace:** A named object in the global scope. It can span files and be concatenated with `outFile`. It does not by itself declare a dependency a loader must fetch.
- **External / module:** One file, its own scope. Other files see a name only after an export and an import. The runtime needs a loader (CommonJS, AMD, or a host that runs ES modules).
- **Reference tags:** `/// <reference path="..." />` points the compiler at a declaration file. It is not a substitute for `import` when you are consuming a module.
- **Emit:** Modules line up one source file to one output file. `outFile` cannot concatenate `commonjs` or `umd` output. From TypeScript 1.8 it can concatenate `amd` and `system`.

**When to use:**

- Read “internal module” in old posts as “namespace”, and “external module” as “module”.
- Write new code with file modules. Keep a namespace when you are maintaining a global script or a declaration file that already has one.

**Examples:**

```ts
// A file with a top-level export is a module (formerly an external module).
export function area(radius: number): number {
  return Math.PI * radius * radius;
}
```

**Sources:**

- [TypeScript handbook — Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)

</details>

<details>
<summary>How do you export and import modules?</summary>

### How do you export and import modules?

**Definition:**

`export` publishes a value or a type from a module. `import` binds a published name in another file. Types can use the same syntax, or the `type` modifier when the import must be erased.

**How it works:**

- **Values:** `export default function helloWorld() {}` is the main export, imported as `import helloWorld from "./hello.js"`. Other exports omit `default` and are imported by name: `import { pi } from "./maths.js"`.
- **Shapes:** `import { pi as π }` renames. `import * as maths` takes the module object. `import "./maths.js"` runs the file for side effects and binds nothing.
- **Types:** `import type { Cat } from "./animal.js"` can only be used as a type. Since 4.5, `import { createCat, type Cat }` marks individual names. Babel, swc, and esbuild can drop those.
- **`verbatimModuleSyntax`:** Imports and exports without `type` are kept. Imports with `type` are dropped. Under that flag, `import` is not rewritten to `require`. CommonJS emit uses `import x = require("x")` and `export =`.

**When to use:**

- Named exports for most functions and types. A default export for the one main function of a file.
- `import type` or an inline `type` when a name exists only in the type system, especially if another tool emits the JavaScript.

**Examples:**

```ts
export const pi = 3.14;

export function twice(n: number): number {
  return n * 2;
}

import { pi, twice } from "./maths.js";

console.log(twice(pi));
```

**Sources:**

- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html#es-module-syntax)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#verbatimmodulesyntax)

</details>

<details>
<summary>Can you describe module resolution and its strategies?</summary>

### Can you describe module resolution and its strategies?

**Definition:**

Module resolution turns the string in `import` or `require` into a file. The `moduleResolution` option selects the lookup rules.

**How it works:**

- **`classic`:** The old default when `module` is not `commonjs`. It is kept for compatibility.
- **`node`:** Follows Node.js CommonJS lookup and also checks `.ts` and `.d.ts`. Extensionless relative imports work. It does not model modern Node ESM.
- **`node16` and `nodenext`:** Added in TypeScript 4.7. They follow Node’s ECMAScript-module lookup. A relative import in an ES module must include the extension, such as `./utils.mjs`.
- **`bundler`:** Added in TypeScript 5.0 for Vite, esbuild, swc, Webpack, Parcel, and similar tools. Extensionless imports work, and `package.json` `"exports"` prefers the `import` condition. Publishing a library with `bundler` can hide problems for users who do not bundle. Those packages should use `node16` or `nodenext`.
- **Related flags:** `baseUrl`, `paths`, and `rootDirs` adjust lookups. Under `node16`, `nodenext`, and `bundler`, `resolvePackageJsonExports` and `resolvePackageJsonImports` default to `true`. `customConditions` adds extra `exports` conditions and is valid only in those three modes.

**When to use:**

- An application built by a bundler: `moduleResolution` `"bundler"`.
- A package published to npm, or Node code that should match Node’s own loader: `"nodenext"` or `"node16"`.

**Examples:**

```json
{
  "compilerOptions": {
    "target": "esnext",
    "moduleResolution": "bundler"
  }
}
```

**Sources:**

- [TypeScript handbook — Module Resolution](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-resolution-options)
- [TypeScript 5.0 — moduleResolution bundler](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#moduleresolution-bundler)

</details>

<details>
<summary>Discuss the compatibility of TypeScript modules with ES6 modules.</summary>

### Discuss the compatibility of TypeScript modules with ES6 modules.

**Definition:**

TypeScript’s module syntax is the ES module syntax (`import` / `export`). The `module` compiler option decides whether that syntax is left as ES modules or rewritten for an older loader.

**How it works:**

- **Emit:** With `module` set so the output is ES2020 (or another ES module target), `import` and `export` stay. `commonjs` rewrites them to `require` and `exports`. `umd` wraps a factory that picks CommonJS, AMD, or a global.
- **`target` is separate:** `target` downlevels language features. `module` chooses how files talk to each other. The loader then finds and runs dependencies before the module body.
- **Interop:** CommonJS has no real default-export versus module-namespace split. `esModuleInterop` reduces that mismatch for default imports.
- **Types:** A type-only import is not a runtime import. `import type` and `--verbatimModuleSyntax` make that erasure explicit so the remaining JavaScript is still valid ES module syntax.

**When to use:**

- Emit ES modules when the runtime or bundler loads them: modern browsers, current Node with `"type": "module"`, or a bundler.
- Emit CommonJS when the host still loads `require`. Under `verbatimModuleSyntax`, write `import x = require("x")` for that output instead of expecting `import` to be rewritten.

**Examples:**

```ts
import { valueOfPi } from "./constants.js";

export const twoPi = valueOfPi * 2;
```

**Sources:**

- [TypeScript handbook — Module Output](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-output-options)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#verbatimmodulesyntax)

</details>

### TypeScript Decorators and Metadata

<details>
<summary>What are Decorators and how do you use them?</summary>

### What are Decorators and how do you use them?

**Definition:**

A decorator is a function you attach with `@` so it can customize a class or a class member. Since TypeScript 5.0 this syntax is valid without `--experimentalDecorators`. The flag still selects the older decorator model.

**How it works:**

- **Call:** `@loggedMethod` calls `loggedMethod` with the decorated value and a context object. If the function returns a replacement, that replacement is installed. If it returns nothing, the original stays.
- **Where:** Methods, fields, getters, setters, auto-accessors (`accessor`), and the class itself. The new model does not decorate parameters, and it is not compatible with `--emitDecoratorMetadata`.
- **Context:** The context reports `name`, whether the member is `static` or `private`, and (since 5.2) a shared `metadata` object. `addInitializer` registers a callback that runs as the class or the member is set up.
- **Placement:** A decorator may sit before `export` or after `export` / `export default`. Both on the same class is an error.
- **Legacy:** `--experimentalDecorators` type-checks and emits the old shape (`target`, property key, property descriptor). A function written for that flag usually does not work unchanged on the new model.

**When to use:**

- Reuse logging, binding, registration, or metadata around classes without copying that code into every method.
- Turn the flag on only for a codebase that already depends on the experimental decorators, such as older Angular or Nest metadata.

**Examples:**

```ts
function loggedMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = String(context.name);
  function replacementMethod(this: any, ...args: any[]) {
    console.log(`Entering ${methodName}`);
    return originalMethod.call(this, ...args);
  }
  return replacementMethod;
}

class Person {
  constructor(public name: string) {}

  @loggedMethod
  greet() {
    return `Hello, ${this.name}`;
  }
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)
- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)

</details>

<details>
<summary>Can you define Class Decorators and how they can alter Class behavior?</summary>

### Can you define Class Decorators and how they can alter Class behavior?

**Definition:**

A class decorator is a function placed directly above a class. It receives the constructor and a `ClassDecoratorContext`, and it can register work that runs after the class is finalized.

**How it works:**

- **Context:** `context.kind` is `"class"`. `context.name` is the class name, or `undefined` for an anonymous class. `context.metadata` is the same object member decorators on that class see.
- **`addInitializer`:** The callback runs after the class definition is finalized. Inside it, `this` is the constructor. The library types use that hook to register the class, for example with `customElements.define`.
- **What the notes describe:** TypeScript 5.0 lists class decorators for subclassing and for registration. The context API the types document is `addInitializer` plus `metadata`.
- **Export:** `@register export default class Foo` and `export default @register class Bar` are both allowed. `@before export @after class Bar` is not.

**When to use:**

- Register a class in a map, seal or tag the constructor, or attach metadata for every instance of that class.
- Do not use a class decorator when a normal subclass or a factory function is clearer.

**Examples:**

```ts
function registered(_value: new (...args: any[]) => object, context: ClassDecoratorContext) {
  context.addInitializer(function () {
    console.log(context.kind, context.name);
  });
}

@registered
class User {
  constructor(public name: string) {}
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>How do you use Method Decorators in TypeScript?</summary>

### How do you use Method Decorators in TypeScript?

**Definition:**

A method decorator receives the original method and a `ClassMethodDecoratorContext`. Return a new function to replace the method. Return nothing to leave the method in place and use the context for other work.

**How it works:**

- **Replacement:** The usual wrapper calls `originalMethod.call(this, ...args)` so `this` and the arguments stay intact, then returns the original result.
- **Context:** `context.name` is the method name. `context.private` and `context.static` say how it was declared. `ClassMethodDecoratorContext<This, Value>` can type `this`, the arguments, and the return type instead of `any`.
- **`addInitializer`:** For an instance method this runs at the start of construction, before other fields initialize. For a static method it runs when the class itself is initializing. The `bound` example uses it to `bind` the method.
- **Private names:** `bound` in the TypeScript 5.0 notes throws if `context.private` is set, because the later assignment by name does not apply to a `#private` method.

**When to use:**

- Log, time, or bind a method without editing every call site.
- Prefer a plain function wrapper when the method is not part of a class.

**Examples:**

```ts
function loggedMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = String(context.name);
  function replacementMethod(this: any, ...args: any[]) {
    console.log(`Entering ${methodName}`);
    return originalMethod.call(this, ...args);
  }
  return replacementMethod;
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>What are Accessor Decorators?</summary>

### What are Accessor Decorators?

**Definition:**

An accessor decorator sits on a getter, a setter, or an auto-accessor (`accessor name = value`). Each kind has its own context: `ClassGetterDecoratorContext`, `ClassSetterDecoratorContext`, or `ClassAccessorDecoratorContext`.

**How it works:**

- **Getter and setter:** They are separate members in the current decorator model. The context `kind` is `"getter"` or `"setter"`. `context.access` can read or write that accessor. `context.static` and `context.private` match the declaration.
- **Auto-accessor:** The `accessor` keyword declares a field with a getter and setter. Its context `kind` is `"accessor"`. A decorator may return a result with a replacement `get`, `set`, or `init`. `init` runs when the initializer value is evaluated.
- **Metadata:** The same decorator can record `context.metadata[context.name]`, which is how the TypeScript 5.2 notes mark an `accessor` for later serialization.
- **Legacy difference:** The old experimental decorators applied one property descriptor to the get/set pair and required the decorators on the first accessor. That rule belongs to `--experimentalDecorators`, not to this context object.

**When to use:**

- Observe or wrap a getter or setter, or mark an auto-accessor as serializable.
- A public field is enough when there is no accessor body to wrap.

**Examples:**

```ts
function mark(_target: any, context: ClassAccessorDecoratorContext): void {
  context.metadata[context.name] = true;
}

class Person {
  @mark
  accessor age = 36;
}
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Explain how to use Property Decorators.</summary>

### Explain how to use Property Decorators.

**Definition:**

A property decorator in current TypeScript is a field decorator. It is a function above a class field. The context type is `ClassFieldDecoratorContext`, and `context.kind` is `"field"`.

**How it works:**

- **Arguments:** The function receives the decorated target and the context. The useful data is on the context: `name`, `static`, `private`, `access`, and `metadata`.
- **`access`:** `context.access.has`, `get`, and `set` read or write that field on an object you pass in.
- **`addInitializer`:** The callback runs immediately after the field is initialized, whether the field is static or not. `this` inside the callback is the instance, or the constructor when the field is static.
- **Metadata:** Assign `context.metadata[context.name]` to record the field. After the class is defined, read the same object from `User[Symbol.metadata]`. Most runtimes need a `Symbol.metadata` polyfill, `target` of `es2022` or lower, and `"esnext.decorators"` in `lib`.

**When to use:**

- Mark public instance fields for serialization or tracing.
- The same `@` on a method or an `accessor` is a different decorator kind. Use this one for a field.

**Examples:**

```ts
function trace<This>(_value: undefined, context: ClassFieldDecoratorContext<This>) {
  context.addInitializer(function (this: This) {
    console.log(String(context.name), context.access.get(this));
  });
}

class User {
  @trace
  name = "Ada";
}
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>How do Decorators compose in TypeScript?</summary>

### How do Decorators compose in TypeScript?

**Definition:**

Stacked decorators apply from the bottom up. The decorator closest to the method runs on the original value. Each decorator above it runs on what the one below returned.

**How it works:**

- **Order:** On `@bound` above `@loggedMethod`, `loggedMethod` decorates `greet`, and `bound` decorates the result of `loggedMethod`. Side effects and wrappers observe that order.
- **Same line:** `@bound @loggedMethod greet()` is the same stack. The notes allow either layout.
- **Factories:** A function can return the decorator, as `loggedMethod("⚠️")` does. You call the factory in the `@` expression. The call produces the function that then participates in the bottom-up application.
- **No return:** `bound` returns nothing, so it does not replace the method. It only calls `context.addInitializer`. That still composes with a decorator that does return a replacement.

**When to use:**

- Combine a wrapper (logging) with an initializer (binding) on one method.
- Keep the stack short. Order matters as soon as one decorator assumes it sees the original function.

**Examples:**

```ts
function loggedMethod(originalMethod: any, _context: ClassMethodDecoratorContext) {
  return function replacement(this: any, ...args: any[]) {
    console.log("Entering");
    return originalMethod.call(this, ...args);
  };
}

function bound(_originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = context.name;
  if (context.private) {
    throw new Error(`Cannot bind private member ${String(methodName)}.`);
  }
  context.addInitializer(function (this: any) {
    this[methodName] = this[methodName].bind(this);
  });
}

class Person {
  constructor(public name: string) {}

  @bound
  @loggedMethod
  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}
```

**Sources:**

- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

<details>
<summary>Discuss Reflection and how it works with Decorators.</summary>

### Discuss Reflection and how it works with Decorators.

**Definition:**

Current decorators reflect by writing a plain object on `context.metadata`. Every decorator on one class shares that object. After decoration, the constructor exposes it as `Symbol.metadata`.

**How it works:**

- **Write:** A decorator stores names or flags on `context.metadata`, often under a `symbol` key so other code does not collide. A `WeakMap` keyed by the metadata object keeps the data private.
- **Read:** `instance.constructor[Symbol.metadata]` is that object. A helper such as the `jsonify` example in the TypeScript 5.2 notes walks the stored property names and reads them off the instance.
- **Limits:** The new design does not work with `--emitDecoratorMetadata`, and it cannot decorate parameters. `context.static`, `context.private`, and a symbol `name` are the checks the serialization example uses to reject members it cannot reflect.
- **Runtime:** `Symbol.metadata` still needs a polyfill on many engines: `Symbol.metadata ??= Symbol("Symbol.metadata")`. Set `target` to `es2022` or below and include `"esnext.decorators"` in `lib`.
- **Old flag:** `--experimentalDecorators` is a different reflection path. Do not expect a decorator written for that flag to see `context.metadata`.

**When to use:**

- A small serializer, a registry, or a framework that must discover which members were decorated.
- Skip metadata when the behavior can live in the replacement function itself, as with logging.

**Examples:**

```ts
function mark(_target: any, context: ClassFieldDecoratorContext): void {
  context.metadata[context.name] = true;
}

class User {
  @mark
  name = "Ada";
}

console.log(User[Symbol.metadata]);
```

**Sources:**

- [TypeScript 5.2 — Decorator Metadata](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#decorator-metadata)
- [TypeScript 5.0 — Decorators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators)

</details>

### TypeScript Configuration and Compilation

<details>
<summary>Explain the purpose of 'tsconfig.json'.</summary>

### Explain the purpose of 'tsconfig.json'.

**Definition:**

`tsconfig.json` is the project file for the TypeScript compiler. It lists which files belong to the program and which `compilerOptions` to use. `tsc` reads it from the directory you compile.

**How it works:**

- **Root:** The file’s folder is the project root. Options and globs are interpreted from there.
- **Two parts:** `compilerOptions` controls checking and emit. `files`, `include`, and `exclude` control the file set.
- **Create one:** `tsc --init` writes a starting file. Since TypeScript 2.3 that file turns `"strict": true` on. The TypeScript 5.9 init also sets `"module": "nodenext"`, `"target": "esnext"`, `"sourceMap": true`, `"verbatimModuleSyntax": true`, `"isolatedModules": true`, and `"skipLibCheck": true`.
- **Overrides:** A later flag on the command line can turn one strict check back off, as in `--strict --noImplicitThis false`. The `strict` flag sets the default for its member checks. It does not lock them.

**When to use:**

- Any project compiled with `tsc`, an editor, or a bundler that honors the TypeScript project.
- One config per program. A solution of several projects uses several configs rather than one global set of flags.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "strict": true,
    "sourceMap": true
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.0 — tsconfig include and exclude](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>How do you include and exclude files in compilation using 'tsconfig.json'?</summary>

### How do you include and exclude files in compilation using 'tsconfig.json'?

**Definition:**

`include` and `exclude` take glob patterns. `files` is an explicit list. Together they decide which files are part of the compilation.

**How it works:**

- **Wildcards:** `*` matches inside one path segment. `?` matches one character. `**/` matches any nested directory. A segment that is only `*` or `.*` keeps supported extensions: `.ts`, `.tsx`, and `.d.ts`, plus `.js` and `.jsx` when `allowJs` is on.
- **Defaults:** If both `files` and `include` are omitted, the compiler takes TypeScript files in that directory and below, minus `exclude`. A missing `exclude` skips `node_modules`, `bower_components`, and `jspm_packages`.
- **Union:** If `files` or `include` is set, the program is the union of those two. `exclude` filters files that came in through `include`. It does not remove a path listed in `files`.
- **Output:** Files under `outDir` are left out unless a `files` entry names them.

**When to use:**

- `include: ["src/**/*"]` for application code, and `exclude` for tests or generated folders you do not want in that program.
- `files` for a small explicit set, or for an output file that must be compiled again.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

**Sources:**

- [TypeScript 2.0 — Glob support in tsconfig.json](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>What are some common compiler options in TypeScript?</summary>

### What are some common compiler options in TypeScript?

**Definition:**

`compilerOptions` selects the JavaScript you emit, how modules are resolved, and how strict the checker is. The TypeScript 5.9 `tsc --init` file is a current picture of the recommended set.

**How it works:**

- **Language and modules:** `target` is the JavaScript level. `module` is the module format. The 5.9 init uses `"target": "esnext"` and `"module": "nodenext"`.
- **Checking:** `"strict": true` turns on the strict family. Since 2.3 that family includes `strictNullChecks`, `noImplicitAny`, `noImplicitThis`, and `alwaysStrict`, and later compilers can add more. The 5.9 init also sets `noUncheckedIndexedAccess` and `exactOptionalPropertyTypes` as further checks. Those two are listed on their own, next to `strict`.
- **Emit:** `sourceMap`, `declaration`, and `declarationMap` are the “other outputs” in that init file. `rootDir` and `outDir` are the layout knobs, shown there as comments.
- **Interop:** The same init turns on `verbatimModuleSyntax`, `isolatedModules`, `moduleDetection: "force"`, `skipLibCheck`, and `jsx: "react-jsx"`.

**When to use:**

- Start from `tsc --init` and delete options the program does not need, such as `jsx` in a non-React package.
- Set `module` and `moduleResolution` for the real host: Node library versus a bundler. Do not copy a browser app’s config into a published package without checking that choice.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "strict": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "sourceMap": true
  }
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>How does TypeScript support source maps?</summary>

### How does TypeScript support source maps?

**Definition:**

`"sourceMap": true` tells the compiler to emit source maps along with the JavaScript. Debuggers and error stacks can then point at the TypeScript instead of only the generated file.

**How it works:**

- **Option:** Set it under `compilerOptions`. The TypeScript 2.0 `tsconfig` example and the TypeScript 5.9 `tsc --init` output both set `"sourceMap": true`.
- **Related outputs:** The 5.9 init groups it with `"declaration": true` and `"declarationMap": true`. Declaration maps are the same idea for `.d.ts` files, so an editor can jump from a type to the original TypeScript.
- **Layout:** Maps follow the emit. If `outDir` is set, the JavaScript and its map are written there with the rest of the output.

**When to use:**

- Any build you debug in a browser, Node, or an editor.
- Turn it off only for a throwaway compile where the extra files are in the way. Production bundles often still ship maps to the error tracker and hide them from the public site.

**Examples:**

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true
  }
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript 2.0 — tsconfig include and exclude](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html)

</details>

<details>
<summary>Discuss incremental build support in TypeScript.</summary>

### Discuss incremental build support in TypeScript.

**Definition:**

`"incremental": true` makes TypeScript save information about the previous compile and reuse it on the next one, so unchanged work is not repeated.

**How it works:**

- **Flag:** TypeScript 3.4 added `incremental`. The compiler looks for a `.tsbuildinfo` file in the output directory and uses it to speed up later builds.
- **Where it sits:** With `"outDir": "./lib"`, that info file is part of the output next to the JavaScript. `include` still decides which sources belong to the program.
- **Clean builds:** Delete the `.tsbuildinfo` file when you need a compile that does not trust the previous graph.

**When to use:**

- Local and CI compiles of a project large enough that a full check dominates the build.
- Leave it off for a one-file script where the info file is more overhead than the compile.

**Examples:**

```json
{
  "compilerOptions": {
    "incremental": true,
    "outDir": "./lib"
  },
  "include": ["./src"]
}
```

**Sources:**

- [TypeScript 3.4 — Incremental compilation](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html)

</details>

<details>
<summary>Explain how the "noImplicitAny" compiler option works.</summary>

### Explain how the "noImplicitAny" compiler option works.

**Definition:**

`noImplicitAny` makes an implicit `any` an error. That happens when you leave off a type and TypeScript cannot infer one from context.

**How it works:**

- **Default without the flag:** The handbook says the compiler will typically use `any` in that case. `any` is not type-checked.
- **With the flag:** You must write a type, or give the checker a value it can infer from, such as a parameter’s argument at a typed call or an initializer.
- **`strict`:** Since TypeScript 2.3, `"strict": true` turns `noImplicitAny` on, along with the other strict checks. You can set `strict` and then turn this one check off explicitly.
- **Inference still works:** `const myName = "Alice"` is `string` with no annotation. The flag does not demand an annotation on every name.

**When to use:**

- Always, unless you are migrating a JavaScript file and need a temporary hole. `"strict": true` is the switch that includes it.
- Where a value really is untyped, write `unknown` and narrow it, instead of letting the hole be `any`.

**Examples:**

```ts
function greet(name: string) {
  return `Hello, ${name}`;
}

const myName = "Alice";
greet(myName);
```

**Sources:**

- [TypeScript handbook — noImplicitAny](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#noimplicitany)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>How do you enable strict null checks in TypeScript?</summary>

### How do you enable strict null checks in TypeScript?

**Definition:**

Set `"strictNullChecks": true`, or turn on `"strict": true`, which includes it. With the check on, `null` and `undefined` are not assigned to other types, and you test for them before using the value.

**How it works:**

- **Off:** `null` and `undefined` may be assigned to any type, and a value that might be missing can still be used. The handbook compares that to languages that do not check null, and says the missing checks are a major source of bugs.
- **On:** Narrow first. `if (name === null)` separates the missing case from the string case, the same way an optional property is checked before use.
- **`!`:** A postfix `!` removes `null` and `undefined` from the type. It is an assertion. It does not change runtime behavior, so use it only when you already know the value is present.
- **How to enable:** `"strict": true` sets the default for `strictNullChecks` (and for `noImplicitAny`, `noImplicitThis`, and `alwaysStrict` as of 2.3). A single flag `"strictNullChecks": true` enables this check by itself.

**When to use:**

- On for every project that can pay the migration cost. The handbook’s advice is to turn it on when that is practical.
- During a migration, enable it per project and fix call sites, rather than silencing every error with `!`.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

```ts
function hello(name: string | null) {
  if (name === null) {
    return;
  }
  console.log("Hello, " + name.toUpperCase());
}
```

**Sources:**

- [TypeScript handbook — null and undefined](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#null-and-undefined)
- [TypeScript 2.3 — strict](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

### TypeScript Typings and @types

<details>
<summary>How do you manage type definitions for external libraries in TypeScript?</summary>

### How do you manage type definitions for external libraries in TypeScript?

**Definition:**

Use the declaration file that ships with the package. If the package has no types, install the matching `@types` package. TypeScript reads `package.json` `"types"` (or the legacy `"typings"` field) and then `"main"`.

**How it works:**

- **Bundled types:** A package that already includes its `.d.ts` does not need a second `@types` install. Publishing docs say to set `"types"` to that file. npm shows a TypeScript icon only when `"types"` is present.
- **`@types`:** `npm install --save-dev @types/lodash` is the handbook’s example for a library that does not ship types. An import of that package pulls the declarations in.
- **Globals:** The `types` compiler option lists `@types` packages that add globals, such as `node` or `jest`. In TypeScript 6.0 that option defaults to `[]`, so those globals are not loaded unless you name them. `"types": ["*"]` restores the old “load every `@types` package” behavior.
- **Checking:** `skipLibCheck` skips type-checking `.d.ts` files. It can hide an error when your code augments a declaration, which the 2.0 notes call rare. `"declaration": true` is how your own package emits `.d.ts`.

**When to use:**

- Install `@types` only after you confirm the library package has no `"types"` entry.
- List `node` or your test runner in `types` on TypeScript 6 or later when globals such as `process` or `describe` are missing.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "types": ["node"]
  }
}
```

**Sources:**

- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)
- [TypeScript 6.0 — types defaults to []](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>What is DefinitelyTyped and how is it related to TypeScript?</summary>

### What is DefinitelyTyped and how is it related to TypeScript?

**Definition:**

DefinitelyTyped is the repository of declaration files for JavaScript libraries that do not publish their own. Those declarations are published to the `@types` organization on npm.

**How it works:**

- **Two publish paths:** The handbook says to bundle `.d.ts` with your package when types are generated from your source (`"declaration": true` works for TypeScript and JavaScript projects). Otherwise, submit the types to DefinitelyTyped.
- **What you install:** A DefinitelyTyped entry becomes a package such as `@types/lodash`. Installing it is an ordinary npm dev dependency. No extra TypeScript tool is required.
- **Not a second copy:** If the library later ships its own `"types"` file, you stop installing the `@types` package. The bundled file is the one TypeScript should read.

**When to use:**

- You depend on a JavaScript package with no `"types"` field.
- You maintain types for someone else’s JavaScript library. Send them to DefinitelyTyped instead of publishing a private `@types` name.

**Examples:**

```text
npm install --save-dev @types/lodash
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)

</details>

<details>
<summary>Explain how to use typing files from npm.</summary>

### Explain how to use typing files from npm.

**Definition:**

Install the package that contains the `.d.ts` files, then import the library as usual. TypeScript follows `"types"`, then `"typings"`, then `"main"` in `package.json` when it is not using the package `"exports"` map.

**How it works:**

- **Own types:** `"types": "./lib/main.d.ts"` next to `"main": "./lib/main.js"`. `"typings"` is a synonym for `"types"`.
- **Separate types:** `npm install --save-dev @types/lodash`. The import `from "lodash"` resolves those declarations. You do not add a triple-slash reference for a normal npm install.
- **Globals versus imports:** Imports pull in the types of the modules you import. The `types` array is only the allow-list for global packages. TypeScript 6.0 defaults it to `[]`. Add `"node"` or `"jest"` when the error tells you a global name is missing.
- **Emit your own:** `"declaration": true` writes `.d.ts` next to the JavaScript you publish.

**When to use:**

- Add `@types/...` as a dev dependency when the runtime package has no types.
- Set `"types"` on a package you publish, even if extension substitution would find the file, because the registry uses that field for the TypeScript badge.

**Examples:**

```json
{
  "name": "awesome",
  "version": "1.0.0",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [TypeScript handbook — Module resolution reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html)

</details>

### Integrating TypeScript with Frameworks

<details>
<summary>How do you integrate TypeScript with Angular?</summary>

### How do you integrate TypeScript with Angular?

**Definition:**

An Angular component is a TypeScript class. The `@Component` decorator supplies the selector, template, and styles. The Angular CLI owns the project’s TypeScript build.

**How it works:**

- **Class plus metadata:** The class holds behavior. `@Component` points at a template string or `templateUrl`, and at styles or `styleUrl`. The selector is the element name used in HTML.
- **Template binding:** Properties declared on the class are available in the template. Angular’s docs describe the class as the place for user input and requests.
- **Build:** The TypeScript handbook’s Babel page names Angular as a framework that already chooses how TypeScript is compiled. You do not pick `tsc` versus Babel yourself for an app the CLI created.
- **Libraries:** A library `tsconfig` can set `angularCompilerOptions.compilationMode` to `"partial"` so the package is not tied to one Angular runtime version.

**When to use:**

- New Angular UI is a component class with `@Component`, not a plain script.
- Leave `compilationMode` at the application default. Set `"partial"` when you are publishing a library in the Angular package format.

**Examples:**

```ts
@Component({
  selector: "profile-photo",
  template: `<img src="profile-photo.jpg" alt="Your profile photo" />`,
})
export class ProfilePhoto {}
```

**Sources:**

- [Angular — Components](https://angular.dev/guide/components)
- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)

</details>

<details>
<summary>Explain integrating TypeScript with React.</summary>

### Explain integrating TypeScript with React.

**Definition:**

Write components in `.tsx` and set the `jsx` compiler option. For React 17 and later, TypeScript’s `react-jsx` and `react-jsxdev` options emit the automatic JSX runtime.

**How it works:**

- **`jsx`:** `"jsx": "react-jsx"` is the production setting from the TypeScript 4.1 notes. `"jsx": "react-jsxdev"` is the development setting. A dev config can `extends` the production config and override only `jsx`.
- **Older factory:** `"jsx": "react"` emits `React.createElement`. `jsxFactory` and `jsxFragmentFactory` rename that factory, which is the setting for a non-React JSX library.
- **Checking:** JSX is type-checked. The emit is still JavaScript. `"strict": true` belongs in the same `compilerOptions` block as `jsx`.
- **Types:** React’s own package ships the component types. You import them in the `.tsx` file. You do not need a second `@types` package when the library already publishes `"types"`.

**When to use:**

- New React code: `jsx` set to `react-jsx`, files named `.tsx`.
- Use `react-jsxdev` for the local config. Keep `react` plus a custom factory only for a JSX library that is not the automatic runtime.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "target": "es2015",
    "jsx": "react-jsx",
    "strict": true
  }
}
```

```tsx
export const el = <div>foo</div>;
```

**Sources:**

- [TypeScript 4.1 — React 17 JSX factories](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html)

</details>

<details>
<summary>How do you use TypeScript with Vue.js?</summary>

### How do you use TypeScript with Vue.js?

**Definition:**

Put `lang="ts"` on the component script. In `<script setup lang="ts">`, pass a type argument to `defineProps` and `defineEmits` so props and events are checked.

**How it works:**

- **Enable it:** `<script lang="ts">` selects the TypeScript preprocessor for that block. `<script setup lang="ts">` is the Composition API form.
- **Props:** `defineProps<Props>()` accepts an interface. Optional fields use `?`. Vue 3 docs also allow the type argument inline.
- **Events:** `defineEmits<{ (e: "change", id: number): void }>()` types the event name and payload. From Vue 3.3 the shorter form is `change: [id: number]`, a named tuple.
- **Runtime check:** The object form of `defineEmits` can validate a payload and return true or false. The type argument form does not add a runtime validator.

**When to use:**

- Type props and emits in a single-file component with `<script setup lang="ts">`.
- Use the object form when the event payload must be rejected at runtime, not only by the checker.

**Examples:**

```vue
<script setup lang="ts">
interface Props {
  foo: string;
  bar?: number;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  change: [id: number];
}>();
</script>
```

**Sources:**

- [Vue — TypeScript with Composition API](https://vuejs.org/guide/typescript/composition-api)
- [Vue — script setup](https://vuejs.org/api/sfc-script-setup)

</details>

<details>
<summary>Discuss using TypeScript in a Node.js application.</summary>

### Discuss using TypeScript in a Node.js application.

**Definition:**

Point `module` at Node’s resolver and install `@types/node` for Node globals. `"module": "nodenext"` (or `"node16"`) makes TypeScript follow Node’s ESM and CommonJS lookup rules.

**How it works:**

- **Format:** In a `.mts` file, `import` uses the ESM algorithm and a relative path needs the extension (`./mod.js`). `import x = require("./mod")` uses the CommonJS algorithm and may omit the extension. A `.cts` file treats `import` as `require`. Dynamic `import()` stays ESM.
- **Globals:** `process`, `fs`, and `path` come from `@types/node`. TypeScript 6.0 does not load every `@types` package automatically. The error text says to `npm i --save-dev @types/node` and add `"node"` to `types`.
- **Assertions:** Under `--module nodenext` in TypeScript 5.8, an import assertion written with `assert` is an error. Node’s current syntax uses `with` (import attributes).
- **Emit:** `nodenext` leaves ESM `import` as written when the file is an ES module, and emits `require` for CommonJS files.

**When to use:**

- A Node app or a package published for Node: `module` `nodenext` and `types` including `"node"`.
- Use `.mts` or `"type": "module"` when the file should stay `import`/`export`. Use `.cts` when the file should emit `require`.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "target": "esnext",
    "types": ["node"],
    "strict": true
  }
}
```

**Sources:**

- [TypeScript 4.7 — module node16](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-7.html)
- [TypeScript 6.0 — types defaults to []](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)
- [TypeScript handbook — Modules reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html)

</details>

### TypeScript Tooling and Ecosystem

<details>
<summary>What are the benefits of using tools like TSLint?</summary>

### What are the benefits of using tools like TSLint?

**Definition:**

A linter flags patterns the type checker does not, such as a Promise that is never awaited. TSLint itself is a migration target: typescript-eslint tells you to move those rules to ESLint.

**How it works:**

- **Parser:** `@typescript-eslint/parser` lets ESLint read TypeScript. `typescript-eslint` exports a recommended flat config on top of ESLint’s own recommended config.
- **Type-aware rules:** `parserOptions.projectService: true` (or `project` pointing at `tsconfig.json`) lets rules use the type checker. `@typescript-eslint/no-floating-promises` is the documented example.
- **TSLint bridge:** `@typescript-eslint/eslint-plugin-tslint` can run old TSLint rules inside ESLint during a move. The package docs say to migrate off TSLint and follow the Getting Started guide.
- **Scope:** Lint does not replace `tsc`. It reports extra style and bug patterns. The type checker still decides whether the program type-checks.

**When to use:**

- A TypeScript repo uses typescript-eslint, not a new TSLint setup.
- Turn on type-aware rules when you want checks that need the type checker, and disable them for plain `.js` files with `tseslint.configs.disableTypeChecked`.

**Examples:**

```js
// @ts-check
import js from "@eslint/js";
import tseslint from "typescript-eslint";

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommended,
);
```

**Sources:**

- [typescript-eslint — packages](https://typescript-eslint.io/packages/typescript-eslint)
- [typescript-eslint — eslint-plugin-tslint](https://typescript-eslint.io/packages/eslint-plugin-tslint)

</details>

<details>
<summary>Can you describe how to set up a build process with TypeScript and Webpack?</summary>

### Can you describe how to set up a build process with TypeScript and Webpack?

**Definition:**

Install `typescript` and `ts-loader`, then add a rule that sends `.ts` and `.tsx` files through `ts-loader`. Webpack reads `tsconfig.json` for the compile.

**How it works:**

- **Loader:** The webpack TypeScript guide uses `test: /\.tsx?$/`, `use: "ts-loader"`, and `exclude: /node_modules/`. `resolve.extensions` lists `.tsx`, `.ts`, and `.js`.
- **Module format:** Do not set `module` to `"CommonJS"`. The guide says webpack then cannot tree-shake the output. Its sample config uses `"module": "esnext"` and `"moduleResolution": "bundler"`.
- **Source maps:** Set `"sourceMap": true` in `tsconfig.json` and `devtool: "inline-source-map"` in the webpack config so the bundle keeps a map back to TypeScript.
- **Speed:** `ts-loader` with `transpileOnly: true` skips checking. Type-checking moves to `fork-ts-checker-webpack-plugin` in another process. If you already use `babel-loader`, `@babel/preset-typescript` can transpile instead. That plugin does not type-check.

**When to use:**

- The app is bundled for the browser and webpack should import non-JS assets. The guide picks `ts-loader` for that.
- Use `transpileOnly` on a slow build, and keep a separate type-check so errors are not dropped.

**Examples:**

```ts
import path from "node:path";
import { fileURLToPath } from "node:url";

const __dirname = path.dirname(fileURLToPath(import.meta.url));

export default {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  module: {
    rules: [
      { test: /\.tsx?$/, use: "ts-loader", exclude: /node_modules/ },
    ],
  },
  resolve: { extensions: [".tsx", ".ts", ".js"] },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

**Sources:**

- [webpack — TypeScript](https://webpack.js.org/guides/typescript)
- [webpack — Build performance](https://webpack.js.org/guides/build-performance)

</details>

<details>
<summary>How does TypeScript work with Babel?</summary>

### How does TypeScript work with Babel?

**Definition:**

Babel’s `preset-typescript` emits JavaScript and does not type-check. `tsc` still type-checks and, with `declaration` plus `emitDeclarationOnly`, writes `.d.ts` without writing `.js`.

**How it works:**

- **Who emits:** The handbook’s heuristic: if the output is mostly the same files as the input, use `tsc`. If you need a pipeline with several outputs, use Babel to transpile and `tsc` to type-check.
- **Frameworks:** Angular, NestJS, tsdx, and the other Getting Started frameworks already choose this for you.
- **What Babel skips:** Babel does not report type errors during emit, so an error missed in the editor can ship. Babel also cannot write `.d.ts` files.
- **Flags:** The Babel handbook says to turn on `isolatedModules` so each file is safe to transpile alone, and `declaration` with `emitDeclarationOnly` so `tsc` writes only declarations. TypeScript 2.8 added `emitDeclarationOnly` for the case where another tool, such as Babel, emits the JavaScript.

**When to use:**

- An existing Babel pipeline should gain types: add the preset and a separate `tsc` check.
- A library that publishes types should not rely on Babel alone. `tsc` has to emit the `.d.ts`.

**Examples:**

```json
{
  "compilerOptions": {
    "isolatedModules": true,
    "declaration": true,
    "emitDeclarationOnly": true
  }
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript 2.8 — emitDeclarationOnly](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html)

</details>

<details>
<summary>Explain using TypeScript with Visual Studio Code or other IDEs.</summary>

### Explain using TypeScript with Visual Studio Code or other IDEs.

**Definition:**

The TypeScript language service powers TypeScript editing, and it also powers JavaScript editing in the Visual Studio family. The editor can be pointed at a chosen TypeScript version.

**How it works:**

- **Version:** TypeScript 3.9 notes say Visual Studio Code can select a TypeScript version. The JavaScript/TypeScript Nightly extension tracks the nightly build. Sublime Text 3 can select a version too. Visual Studio 2017 and 2019 use the SDK and MSBuild installs.
- **Go to definition:** `"declarationMap": true` writes a map from `.d.ts` back to the original file. The declaration-file handbook shows this so VS Code’s Go to Definition opens the source, not only the generated declaration.
- **Debugging:** `"sourceMap": true` emits a map from the JavaScript back to TypeScript. A bundler such as webpack still has to pick that map up (`devtool`).
- **JavaScript files:** `"allowJs": true` lets the same project read `.js`. The editor then type-checks those files with the project’s options.

**When to use:**

- Match the editor’s TypeScript to the version in the repo so the editor and `tsc` report the same errors.
- Ship `declarationMap` when a published package includes its `.ts` sources and consumers should jump to them.

**Examples:**

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "emitDeclarationOnly": true,
    "outDir": "dist",
    "allowJs": true
  }
}
```

**Sources:**

- [TypeScript 3.9 — Editor improvements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html)
- [TypeScript handbook — Creating .d.ts from .js](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)

</details>

### TypeScript Best Practices and Patterns

<details>
<summary>What are the best practices for structuring a TypeScript application?</summary>

### What are the best practices for structuring a TypeScript application?

**Definition:**

Put each file’s public API on `export`, turn on `strict`, and let `tsconfig.json` name the files in the program. The handbook’s default for new code is ES modules, not namespaces.

**How it works:**

- **Modules:** A file with a top-level `import` or `export` is a module. Its types and values stay local until you export them. `export {}` forces a file to be a module when it has no other export.
- **Config:** TypeScript 5.9’s `tsc --init` turns on `strict`, `verbatimModuleSyntax`, `isolatedModules`, and `moduleDetection: "force"`. `verbatimModuleSyntax` keeps a value import in the emit and drops an `import type`.
- **Layout:** `include` globs such as `src/**/*` choose the program. `exclude` does not override a file listed in `files`. `outDir` is where emit goes.
- **Resolution:** An app or library published for Node uses `module: "nodenext"`. A bundler build uses `moduleResolution: "bundler"`. Webpack’s TypeScript guide says not to emit CommonJS if the bundle should tree-shake.

**When to use:**

- Start from `tsc --init` and change `module` for Node or the bundler, rather than a hand-written loose config.
- Export types with `export type` or `import type` so a type-only edge is erased and is not a runtime import.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "module": "nodenext",
    "moduleDetection": "force",
    "verbatimModuleSyntax": true,
    "isolatedModules": true
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/modules/introduction.html)

</details>

<details>
<summary>How do you handle async/await in TypeScript?</summary>

### How do you handle async/await in TypeScript?

**Definition:**

An `async` function returns a `Promise`. `await` pauses until that Promise settles, and the awaited expression has the Promise’s value type.

**How it works:**

- **Since 1.6:** `async` works on function declarations, arrow functions, function expressions, and class methods. The body may `await` a `Promise<T>`. After `await`, the value is `T`.
- **Inference:** With a global `Promise` and a target that supports it, TypeScript infers the return type as `Promise<...>`. The 1.6 notes show that inference when `--target ES6`. Current `tsc --init` sets `target` to `esnext`.
- **Errors:** A rejected Promise is still a rejection at runtime. The type of a successful `await` does not include the rejection reason.
- **Iteration:** An `async function*` is an async generator. `for await` is legal only inside an async function or async generator. That needs a global `Promise` and `Symbol.asyncIterator`.

**When to use:**

- Annotate `Promise<T>` when the function is part of a public API and you do not want the return type inferred from the body.
- Use `for await` for an async iterable. A normal `for...of` does not walk one.

**Examples:**

```ts
async function addOne(value: Promise<number>): Promise<number> {
  const current = await value;
  return current + 1;
}
```

**Sources:**

- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)
- [TypeScript 2.3 — Async generators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

<details>
<summary>Discuss how you manage state in TypeScript-based applications.</summary>

### Discuss how you manage state in TypeScript-based applications.

**Definition:**

Model each state as one member of a discriminated union. A shared literal property, such as `status`, lets the checker narrow the rest of the object in each branch.

**How it works:**

- **Union:** `RequestState` is `"idle" | "loading" | "success" | "error"`, and only the success member carries `data`. Assigning the wrong payload is a type error.
- **Narrowing:** `switch (state.status)` makes `state.data` available only in the `"success"` case. A `never` check in `default` fails the build when a new status is added and not handled.
- **Updates:** Return a new object instead of mutating a `readonly` field. `Readonly<T>` and `readonly` properties reject assignment.
- **Modules:** Keep the state type and the function that returns the next state in a module. Export the type. Callers import it. They do not reach into another file’s locals.

**When to use:**

- A screen or request has several shapes. One object type with optional fields hides which fields exist in which status.
- Reach for a state library only when many distant modules must read the same value. Local state stays a value plus a function that returns the next one.

**Examples:**

```ts
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: Error };

function label<T>(state: RequestState<T>): string {
  switch (state.status) {
    case "idle":
      return "idle";
    case "loading":
      return "loading";
    case "success":
      return "ready";
    case "error":
      return state.error.message;
    default: {
      const _exhaustive: never = state;
      return _exhaustive;
    }
  }
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>What are some common design patterns used in TypeScript?</summary>

### What are some common design patterns used in TypeScript?

**Definition:**

TypeScript does not add a pattern library. You express the usual object and function patterns with classes, function types, and unions, and the checker enforces the shape.

**How it works:**

- **Strategy:** A function type such as `(value: number) => number` is the strategy. Callers pass a function. No class is required.
- **State:** A discriminated union is the state machine. Each `status` has its own fields. `switch` plus a `never` default covers every member.
- **Dependency:** A class constructor takes an interface. Tests pass another class that implements the same interface. The consumer depends on the method list, not on one concrete class.
- **Module boundary:** Export the interface and the factory. Keep helpers unexported so other files cannot call them.

**When to use:**

- Prefer a function type when the only variation is one behavior.
- Prefer a class when you need constructors, `private` fields, or `implements`.

**Examples:**

```ts
type Format = (value: number) => string;

function render(value: number, format: Format): string {
  return format(value);
}
```

**Sources:**

- [TypeScript handbook — Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html)
- [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)

</details>

<details>
<summary>How do you debug a TypeScript application?</summary>

### How do you debug a TypeScript application?

**Definition:**

Emit a source map so the debugger shows the `.ts` file. The editor should use the same TypeScript version as the project, so breakpoints and errors match `tsc`.

**How it works:**

- **Compiler:** `"sourceMap": true` writes a map from the emitted JavaScript back to TypeScript. The webpack TypeScript guide also sets `devtool: "inline-source-map"` so the bundle keeps that map.
- **Declarations:** `"declarationMap": true` maps a `.d.ts` back to the original source. Go to Definition in VS Code then opens the `.ts` or `.js` file the handbook example points at.
- **Editor:** Visual Studio Code can select the workspace TypeScript version. A mismatch means the editor reports different errors than `tsc`.
- **Type errors:** Source maps do not fix a type error. `tsc --noEmit` or the editor’s language service reports it before you step through JavaScript.

**When to use:**

- Turn source maps on for a development build. Shipping them is optional: the modules guide says they help debugging only if you also ship the `.ts` sources, and they increase package size.
- Select the workspace TypeScript version before you chase an error that only one of the two tools reports.

**Examples:**

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "declarationMap": true
  }
}
```

**Sources:**

- [webpack — TypeScript](https://webpack.js.org/guides/typescript)
- [TypeScript handbook — Choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

</details>

### TypeScript Testing and Quality Assurance

<details>
<summary>How do you write unit tests for TypeScript code?</summary>

### How do you write unit tests for TypeScript code?

**Definition:**

Write the test in a `.ts` file and import the function under test. Jest runs it after a preprocessor turns TypeScript into JavaScript. The test file is still type-checked.

**How it works:**

- **Globals:** `npm install --save-dev @jest/globals` and import `describe`, `test`, and `expect` from it. Those types ship with Jest. `@types/jest` is the DefinitelyTyped alternative for globals you do not import. Match its major version to the Jest version you run.
- **Transform:** Jest’s getting started lists `ts-jest` or `@babel/preset-typescript` as the way to transpile tests. Babel’s preset does not type-check. Run `tsc` as well if that is the transform.
- **Globals list:** On TypeScript 6, `types` defaults to `[]`. If you use `@types/jest` without importing it, add `"jest"` to `types`. An import from `@jest/globals` does not need that entry.
- **Assertions:** `expect(sum(1, 2)).toBe(3)` is the documented test. The argument types of `sum` are checked when the test file is part of the TypeScript program.

**When to use:**

- Import from `@jest/globals` so the test types track the Jest version you installed.
- Keep tests inside `include` (or a project reference) so a wrong argument is a `tsc` error, not only a runtime failure.

**Examples:**

```ts
import { describe, expect, test } from "@jest/globals";
import { sum } from "./sum";

describe("sum module", () => {
  test("adds 1 + 2 to equal 3", () => {
    expect(sum(1, 2)).toBe(3);
  });
});
```

**Sources:**

- [Jest — Getting started](https://jestjs.io/docs/getting-started)

</details>

<details>
<summary>What testing frameworks are commonly used with TypeScript?</summary>

### What testing frameworks are commonly used with TypeScript?

**Definition:**

Jest is the usual unit-test runner, with `ts-jest` or Babel’s TypeScript preset to transpile. Playwright runs end-to-end browser tests from TypeScript files that import `@playwright/test`.

**How it works:**

- **Jest:** Install `ts-jest`, or `@babel/preset-typescript` if Babel already transpiles the app. Install `@jest/globals` and import the test APIs, or install `@types/jest` when the test file uses undeclared globals.
- **Playwright:** `import { test, expect } from "@playwright/test"`. Tests live in files such as `example.spec.ts`. `playwright.config.ts` uses `defineConfig` for `use`, timeouts, and `expect`.
- **Lint:** typescript-eslint is not a test runner. Its Jest example turns on `eslint-plugin-jest` for files under `test/**`.
- **Types:** Playwright’s package includes the `test` and `expect` types. Jest’s imported APIs come from `@jest/globals`. You do not invent a second declaration file for either.

**When to use:**

- Jest for a function or module, with no browser.
- Playwright when the assertion is about a page: title, role, or a click.

**Examples:**

```ts
import { test, expect } from "@playwright/test";

test("has title", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await expect(page).toHaveTitle(/Playwright/);
});
```

**Sources:**

- [Jest — Getting started](https://jestjs.io/docs/getting-started)
- [Playwright — Writing tests](https://playwright.dev/docs/writing-tests)

</details>

<details>
<summary>Discuss how to perform end-to-end testing in TypeScript applications.</summary>

### Discuss how to perform end-to-end testing in TypeScript applications.

**Definition:**

An end-to-end test drives a browser. Playwright’s TypeScript API opens a page, acts through locators, and asserts with `expect`, which retries until the condition holds or the timeout ends.

**How it works:**

- **Test file:** `import { test, expect } from "@playwright/test"`. `test` gives you `page`. `page.goto` loads a URL. `page.getByRole` finds an accessible element. `expect(page).toHaveTitle` and `expect(locator).toBeVisible` are the documented assertions.
- **Config:** `playwright.config.ts` default-exports `defineConfig({ use: { ... } })`. The options page shows `headless`, `viewport`, `ignoreHTTPSErrors`, and `video: "on-first-retry"`. `expect.timeout` sets how long auto-retrying assertions wait.
- **Page objects:** A class can wrap `page` when the same clicks repeat. The test still imports `test` and `expect`. The class is ordinary TypeScript.
- **JavaScript files:** Playwright’s writing-tests page says to put `// @ts-check` at the top of a JavaScript test so VS Code type-checks it. A `.ts` test is checked as TypeScript.

**When to use:**

- Cover a user path: open the app, click, see the next heading.
- Keep unit checks in Jest. A browser test is slower and is the wrong tool for a pure function.

**Examples:**

```ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  use: {
    viewport: { width: 1280, height: 720 },
    video: "on-first-retry",
  },
});
```

**Sources:**

- [Playwright — Writing tests](https://playwright.dev/docs/writing-tests)
- [Playwright — Test configuration](https://playwright.dev/docs/test-configuration)

</details>

### TypeScript for Backend Development

<details>
<summary>How do you use TypeScript with Express.js?</summary>

### How do you use TypeScript with Express.js?

**Definition:**

Write the app in TypeScript and import `Express`, `Request`, and `Response` from `express`. Express’s starter shows a file you can run with Node, which strips the types without a separate build step.

**How it works:**

- **App:** `const app: Express = express()`. `app.get` and `app.post` take a path and a handler. The handler’s `req` is `Request` and `res` is `Response`.
- **Same objects as Node:** The hello-world page says `req` and `res` are Node’s request and response, so Node methods such as `req.pipe()` are still there.
- **Project settings:** A Node app sets `"module": "nodenext"` and lists `"node"` in `types` on TypeScript 6, after `npm i --save-dev @types/node`. Express’s own package supplies the `Request` and `Response` types in the documented import.
- **Routes:** `app.route("/book")` chains `.get`, `.post`, and `.put` for one path. Each handler is still `(req: Request, res: Response) => ...`.

**When to use:**

- Annotate `req` and `res` on a public handler so a mistaken `res.send` argument or a missing import shows up in the editor.
- Use Node’s type-stripping run when the file has only erasable types. A project that emits enums or other non-erasable syntax still needs `tsc` or a bundler.

**Examples:**

```ts
import express, { type Express, type Request, type Response } from "express";

const app: Express = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});

app.listen(3000);
```

**Sources:**

- [Express — Installing](https://expressjs.com/en/starter/installing)
- [Express — Hello world](https://expressjs.com/en/starter/hello-world)

</details>

<details>
<summary>Discuss creating RESTful services with TypeScript.</summary>

### Discuss creating RESTful services with TypeScript.

**Definition:**

A REST handler is an Express route method: `get`, `post`, and the other HTTP methods. Type the handler with `Request` and `Response`, and type the JSON body your handler reads as its own interface.

**How it works:**

- **Methods:** `app.get` reads, `app.post` creates. The routing guide shows both on `"/"` and `res.send` for the response body. `app.route("/book")` groups get, post, and put on one path.
- **Body:** Give the payload a named type, for example `Book`. Read fields from that type in the handler. A missing field is a type error at the call site you control. Express does not invent that interface for you.
- **Status:** `res.send` writes the body. Keep the value you send assignable to the type you documented for that route.
- **Modules:** Put each resource’s types and routes in a module and export the router or the register function. Other files import the type, not a copy of the object shape.

**When to use:**

- One route module per resource when the path and the payload type should stay next to each other.
- A shared `Book` type when more than one method returns the same shape.

**Examples:**

```ts
import express, { type Request, type Response } from "express";

interface Book {
  title: string;
}

const app = express();

app.route("/book")
  .get((_req: Request, res: Response) => {
    res.send("Get a random book");
  })
  .post((_req: Request, res: Response) => {
    const created: Book = { title: "Guide" };
    res.send(created.title);
  });
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>Explain how to handle data modeling in a TypeScript backend.</summary>

### Explain how to handle data modeling in a TypeScript backend.

**Definition:**

Model a record as an interface or a type alias. Use a union when a field has a fixed set of variants, and `readonly` when callers must not assign to it.

**How it works:**

- **Object type:** `interface Book { title: string; pages?: number }` makes `title` required and `pages` optional. Excess-property checks apply when you assign an object literal to that type.
- **Variants:** A field like `status: "draft" | "published"` is a union of literals. `switch` on it narrows the rest of a discriminated union.
- **Boundaries:** Validate untyped input at the edge where it enters the process. After that check, the rest of the module sees the `Book` type. The type alone does not parse JSON at runtime.
- **Sharing:** Export the type from the module that owns it. A declaration file is only needed when JavaScript callers outside the project must see it (`"declaration": true`).

**When to use:**

- An interface for a data object other modules implement or extend.
- A type alias for a union or a mapped shape of that object.

**Examples:**

```ts
interface Book {
  readonly id: string;
  title: string;
  status: "draft" | "published";
}

function titleOf(book: Book): string {
  return book.status === "draft" ? `(draft) ${book.title}` : book.title;
}
```

**Sources:**

- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [TypeScript handbook — Object types](https://www.typescriptlang.org/docs/handbook/2/objects.html)

</details>

<details>
<summary>How do you manage database interactions in TypeScript?</summary>

### How do you manage database interactions in TypeScript?

**Definition:**

Type the row your query returns. If the database client ships `.d.ts` or an `@types` package, import that client. The row type is still yours when the driver returns an untyped result.

**How it works:**

- **Client types:** A package with `"types"` in `package.json` is used as-is. A package without types gets `npm install --save-dev @types/...` from DefinitelyTyped. Do not install both for the same package.
- **Row:** `interface UserRow { id: string; email: string }` is the value you return from a function that ran the query. Callers then see `UserRow`, not an open object.
- **Input:** Parameters of the query function are typed (`id: string`). That does not replace a parameterized query in the driver. It stops the wrong TypeScript value from being passed.
- **Globals:** On TypeScript 6, add the driver’s or Node’s package to `types` only when it contributes globals. An ordinary import already pulls in that module’s types.

**When to use:**

- Wrap the driver call in a function that returns your row type. The rest of the app does not import the driver.
- Regenerate or reinstall types when you upgrade the client, so the method names match the version you run.

**Examples:**

```ts
interface UserRow {
  id: string;
  email: string;
}

function toUser(row: UserRow): UserRow {
  return { id: row.id, email: row.email };
}
```

**Sources:**

- [TypeScript handbook — Consumption](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)
- [TypeScript handbook — Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<details>
<summary>Discuss authentication and authorization strategies in TypeScript APIs.</summary>

### Discuss authentication and authorization strategies in TypeScript APIs.

**Definition:**

Represent the caller as a discriminated union, and branch on that type before the handler sends data. TypeScript checks the branch. It does not authenticate the request.

**How it works:**

- **Caller:** `type Caller = { kind: "anonymous" } | { kind: "user"; id: string; role: "reader" | "editor" }`. `kind` is the discriminant. `role` exists only on `"user"`.
- **Handler:** An Express handler still receives `Request` and `Response`. After you have built a `Caller`, `switch (caller.kind)` narrows it. The `"anonymous"` branch can `res.send` a refusal. The `"user"` branch can read `caller.role`.
- **Authorization:** Compare `role` to the literal the route requires. A forgotten role is a type error if the route’s allowed roles are a union and you use a `never` default.
- **Edge:** The union is only as true as the check that built it. Session or token parsing stays at the boundary. The type does not verify a signature.

**When to use:**

- One `Caller` type for every route, so a handler cannot treat an anonymous request as a user without a branch.
- A narrower role union on an admin route than on a public route.

**Examples:**

```ts
type Caller =
  | { kind: "anonymous" }
  | { kind: "user"; id: string; role: "reader" | "editor" };

function canEdit(caller: Caller): boolean {
  return caller.kind === "user" && caller.role === "editor";
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [Express — Routing](https://expressjs.com/en/guide/routing)

</details>

### TypeScript Advanced Practices

<details>
<summary>Discuss using TypeScript for Domain-driven Design (DDD).</summary>

### Discuss using TypeScript for Domain-driven Design (DDD).

**Definition:**

Encode the domain in types: an entity is a class or an object type, a variant is a discriminated union, and a repository is an interface. TypeScript checks the shapes. It does not run the domain rules at runtime.

**How it works:**

- **Entity:** A class can hide fields with `private` and expose methods. Callers use the methods, not the raw fields.
- **Value:** A small object type with `readonly` fields is a value. Two values compare by those fields. The type rejects assignment to a `readonly` property.
- **Variants:** `OrderStatus` as `"draft" | "placed" | "shipped"` plus a `switch` and a `never` default means a new status does not compile until every transition handles it.
- **Repository:** `interface OrderRepository { find(id: string): Promise<Order> }` is the port. The database module implements it. The domain module imports the interface, not the driver.

**When to use:**

- Put the union and the class in the module that owns the concept, and export only that API.
- Parse outside input into the domain type at the boundary. After that, functions accept `Order`, not `unknown`.

**Examples:**

```ts
type Order =
  | { status: "draft"; id: string }
  | { status: "placed"; id: string };

function place(order: Order): Order {
  if (order.status !== "draft") return order;
  return { status: "placed", id: order.id };
}
```

**Sources:**

- [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)
- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>How do you implement Event Sourcing in TypeScript?</summary>

### How do you implement Event Sourcing in TypeScript?

**Definition:**

Store what happened as a discriminated union of events, and fold that list into the current state. Each event name is a literal, so a new event is a type error until the fold handles it.

**How it works:**

- **Event:** `{ type: "Added"; amount: number } | { type: "Removed"; amount: number }`. `type` is the discriminant. Only `Added` needs to be handled to read `amount` on that member. Both members can share `amount` if you put it on both.
- **Fold:** `reduce` starts from a zero state and `switch`es on `event.type`. The `default` assigns the event to `never` so a missing case fails the build.
- **Async:** Loading the log is `Promise<Event[]>`. An `async` function awaits it and then folds. The fold itself stays synchronous.
- **Persistence:** The array is the log your store returns. TypeScript types the elements. Writing the rows is the database client, typed separately.

**When to use:**

- The state is derived from a history and you need the checker to notice a new event.
- A single current-row update is enough when you do not need the history. You do not need an event union for that.

**Examples:**

```ts
type AccountEvent =
  | { type: "Added"; amount: number }
  | { type: "Removed"; amount: number };

function balance(events: readonly AccountEvent[]): number {
  return events.reduce((total, event) => {
    switch (event.type) {
      case "Added":
        return total + event.amount;
      case "Removed":
        return total - event.amount;
      default: {
        const _exhaustive: never = event;
        return _exhaustive;
      }
    }
  }, 0);
}
```

**Sources:**

- [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)

</details>

<details>
<summary>Explain the concept of Isomorphic apps with TypeScript.</summary>

### Explain the concept of Isomorphic apps with TypeScript.

**Definition:**

An isomorphic module is TypeScript that emits JavaScript you can import both on the server and in the browser. Types are erased, so the shared file must not call an API that exists on only one of those platforms.

**How it works:**

- **Emit:** `tsc` or Babel’s TypeScript preset writes JavaScript. A type annotation is not in that output. The same `.js` module can be loaded by Node and by a bundler.
- **Resolution:** Node code uses `module: "nodenext"`. A browser bundle uses `moduleResolution: "bundler"`. The shared source stays `import`/`export`. Each side has its own `tsconfig` if the emit targets differ.
- **Split:** DOM types and Node globals (`process`, `fs`) are not in the shared module. Pass those in from the platform-specific file. The shared function accepts plain data types.
- **Type-only imports:** `import type` is erased, so a shared module can mention a type from either side without emitting a runtime import. `verbatimModuleSyntax` keeps that erasure honest.

**When to use:**

- Share a pure function, a type, or a `switch` on a discriminated union between the API and the UI.
- Keep `page` or `fs` in the file that runs on that platform.

**Examples:**

```ts
export type Role = "reader" | "editor";

export function canEdit(role: Role): boolean {
  return role === "editor";
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html)

</details>

### TypeScript Types and ECMAScript Standards

<details>
<summary>How does TypeScript align with the latest ECMAScript standards?</summary>

### How does TypeScript align with the latest ECMAScript standards?

**Definition:**

TypeScript type-checks JavaScript syntax and emits JavaScript for the `target` you set. TypeScript 7 follows TypeScript 6’s command-line behavior: `target` defaults to the current stable ECMAScript version before `esnext`.

**How it works:**

- **Target:** TypeScript 5.9’s `tsc --init` sets `"target": "esnext"`. TypeScript 7’s announcement says the 7.0 default is the stable edition immediately preceding `esnext`, and `module` defaults to `esnext`.
- **Removed old targets:** TypeScript 7 drops `target: es5`. `moduleResolution` `node` / `node10` and `classic` are gone. The announcement recommends `nodenext` or `bundler`.
- **Import attributes:** The `asserts` keyword on imports is rejected. The syntax is `with`, matching ECMAScript import attributes. The same rule is in the TypeScript 5.8 `nodenext` notes.
- **New type behavior:** TypeScript 7 template-literal inference keeps a whole Unicode code point. `HeadTail<"😀abc">` is `["😀", "abc"]`, in line with `for...of` and `[...str]`, not with UTF-16 code units.

**When to use:**

- Leave `target` at the default for a new app unless you must emit older syntax.
- Move off deprecated flags while you are still on TypeScript 6. TypeScript 7 treats them as hard errors. `"ignoreDeprecations": "6.0"` only silences them in 6.

**Examples:**

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "nodenext",
    "strict": true
  }
}
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>What enhancements has TypeScript brought to match ES6 and beyond?</summary>

### What enhancements has TypeScript brought to match ES6 and beyond?

**Definition:**

TypeScript accepts the JavaScript syntax of the edition you target and adds types that are erased. `async`/`await`, classes, and `import`/`export` are that JavaScript, checked by the compiler.

**How it works:**

- **Async:** TypeScript 1.6 added `async` functions when targeting ES6, with a global `Promise`. `await` on `Promise<T>` has type `T`. The return type is inferred as `Promise<...>` in that setup.
- **Modules:** A file with top-level `import` or `export` is an ES module. `module: "nodenext"` emits `import` for ESM files and `require` for CommonJS files. TypeScript 7 no longer supports `module` values `amd`, `umd`, `system`, or `none`.
- **Classes:** Class fields, methods, and constructors type-check as the class handbook describes. The emit follows `target`.
- **Types on top:** Unions, generics, and template literal types are not ECMAScript runtime features. They disappear from the JavaScript. Babel’s preset and `tsc` both drop them. Only `tsc` emits `.d.ts`.

**When to use:**

- Write the JavaScript feature directly (`async`, `import`) and put the type on the boundary.
- Set `target` and `lib` for the APIs you call. A lower `target` asks the compiler to emit older syntax. It does not add missing runtime polyfills by itself.

**Examples:**

```ts
export async function readCount(load: () => Promise<number>): Promise<number> {
  return await load();
}
```

**Sources:**

- [TypeScript 1.6 — Async functions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Discuss how TypeScript handles async iterators.</summary>

### Discuss how TypeScript handles async iterators.

**Definition:**

An `async function*` returns an async iterable. `for await` walks it and is legal only inside an async function or an async generator. The types use `Promise` and `Symbol.asyncIterator`.

**How it works:**

- **Generator:** TypeScript 2.3’s example yields numbers, `await`s, and `yield*`s both a plain array and another async generator. The runtime needs a global `Promise` and `Symbol.asyncIterator`.
- **Loop:** `for await (const x of g())` is inside `async function f`. Each `x` is the yielded value. The loop itself is not valid in a non-async function.
- **Interface:** The 2.3 notes describe `AsyncIterator<T>` with `next`, optional `return`, and optional `throw`, each returning `Promise<IteratorResult<T>>`.
- **Emit:** The syntax is JavaScript async iteration. Types on the yielded values are erased. `target` must be an edition that can represent the emit you asked for, or the compiler downlevels what it can.

**When to use:**

- `async function*` when a sequence arrives over time, such as pages of results.
- A single `async` function when there is one `Promise`, not a stream of values.

**Examples:**

```ts
async function* count(): AsyncGenerator<number> {
  yield 1;
  yield 2;
}

async function log(): Promise<void> {
  for await (const value of count()) {
    value.toFixed(0);
  }
}
```

**Sources:**

- [TypeScript 2.3 — Async generators](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)

</details>

### TypeScript Performance Optimization

<details>
<summary>How do you optimize the performance of a TypeScript application?</summary>

### How do you optimize the performance of a TypeScript application?

**Definition:**

Separate “the checker is slow” from “the emitted JavaScript is slow.” TypeScript 7 is a native compiler, typically about 8× to 12× faster on a full build. The JavaScript you ship is still whatever `target` and your bundler emit.

**How it works:**

- **Compiler:** TypeScript 7 parses, checks, and emits on several threads. `--checkers` defaults to 4 type-checking workers. `--builders` is how many project-reference builds run at once under `--build`. `--singleThreaded` turns the parallelism off. `--watch` uses a file watcher ported from Parcel.
- **Checking versus emit:** `ts-loader` with `transpileOnly: true` skips checking in the webpack build. `fork-ts-checker-webpack-plugin` checks in another process. Babel’s preset also skips checking, so `tsc` stays in the pipeline.
- **Declarations:** `isolatedDeclarations` (TypeScript 5.5) reports when a file’s `.d.ts` cannot be emitted from that file alone. It does not change emit by itself. It is the condition for tools that want to emit declarations in parallel.
- **Runtime:** Types are erased, so they do not speed up or slow down the running program. Runtime cost is the JavaScript you wrote and the `target` you emit.

**When to use:**

- Install TypeScript 7 (`npm install -D typescript`, then `npx tsc`) when build and editor time dominate. Fix TypeScript 6 deprecations first. TypeScript 7 rejects them.
- Raise `--checkers` on a machine with spare cores. The announcement shows extra speed and higher memory. Lower it on a small CI runner.

**Examples:**

```text
npx tsc --checkers 4 --noEmit
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [webpack — Build performance](https://webpack.js.org/guides/build-performance)

</details>

<details>
<summary>Discuss tree shaking in TypeScript.</summary>

### Discuss tree shaking in TypeScript.

**Definition:**

Tree shaking drops unused JavaScript exports. TypeScript helps by erasing type-only imports and by emitting ES module syntax the bundler can see. The bundler does the shaking.

**How it works:**

- **Type imports:** `import type` is fully erased. Under `verbatimModuleSyntax`, a value import is kept and a `type` modifier on a name is dropped. An empty `import { type xyz }` becomes `import {}` and can still be a runtime import, so do not use it for a type-only binding.
- **Module kind:** The webpack TypeScript guide says not to set `module` to `"CommonJS"`, or webpack cannot tree-shake. Its sample uses `"module": "esnext"`.
- **Side-effect imports:** `verbatimModuleSyntax` keeps an import that has no named bindings if you wrote it as a value import. That is how a side-effect import stays in the emit.
- **Checker speed is separate:** `isolatedModules` makes each file safe for a single-file transpile. It does not remove exports from the bundle.

**When to use:**

- Mark type-only names with `type` so they are not left in the JavaScript as a live import.
- Emit ESM for a browser bundle. Use `nodenext` when the program is a Node package and the consumer, not webpack, loads the files.

**Examples:**

```ts
import type { User } from "./user.js";
import { format } from "./format.js";

export function label(user: User): string {
  return format(user.name);
}
```

**Sources:**

- [TypeScript 5.0 — verbatimModuleSyntax](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html)
- [webpack — TypeScript](https://webpack.js.org/guides/typescript)

</details>

<details>
<summary>Explain how to optimize bundle size with TypeScript.</summary>

### Explain how to optimize bundle size with TypeScript.

**Definition:**

Keep types out of the JavaScript, and do not ship files the browser does not run. `import type` and `emitDeclarationOnly` are the compiler levers. Minifying and shaking the bundle belong to the bundler.

**How it works:**

- **Erasure:** A type annotation and an `import type` are not in the emit. Importing a class or a function is a runtime import even if you also use it as a type. Split those with the inline `type` modifier.
- **Declarations:** `declaration` plus `emitDeclarationOnly` writes `.d.ts` and no `.js` when Babel or webpack already emitted the JavaScript. You do not publish two copies of the program.
- **Maps:** `sourceMap` and `declarationMap` help debugging and Go to Definition. The modules guide says they pay off when you also ship the `.ts` sources, and they increase package size.
- **Format:** Emit ES modules (`module` not `"CommonJS"`) so webpack can tree-shake. TypeScript 7 no longer supports `amd`, `umd`, or `system` as `module`.

**When to use:**

- A library publishes `.js` plus `.d.ts`, not the checker’s extra `.js` from a second tool.
- Turn maps on for the package only when consumers should step into your TypeScript.

**Examples:**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "moduleResolution": "bundler",
    "verbatimModuleSyntax": true,
    "declaration": true,
    "emitDeclarationOnly": true
  }
}
```

**Sources:**

- [TypeScript handbook — Babel with TypeScript](https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html)
- [TypeScript handbook — Choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

</details>

### TypeScript 4.x Features and Updates

<details>
<summary>What are the new features included in the latest TypeScript release?</summary>

### What are the new features included in the latest TypeScript release?

**Definition:**

TypeScript 7.0 is a native port of the compiler, written so that checking and emit stay compatible with TypeScript 6.0. It is typically about 8× to 12× faster on a full build. It does not ship a stable programmatic API. That is planned for 7.1.

**How it works:**

- **Speed:** Parsing, checking, and emit run in parallel. `--checkers` defaults to 4. `--builders` parallelizes project references under `--build`. `--singleThreaded` disables that. `--watch` uses a Go port of Parcel’s file watcher. Editors talk to it over the language server protocol. VS Code has a TypeScript 7 extension.
- **Compatibility:** Code that is clean on TypeScript 6 with `stableTypeOrdering` and without `ignoreDeprecations` should match. TypeScript 7’s defaults include `strict`, `module: esnext`, `types: []`, and `rootDir: "./"`. Flags deprecated in 6, including `target: es5` and `moduleResolution: node`, are hard errors.
- **Side by side:** There is no compiler API in 7.0. `@typescript/typescript6` provides `tsc6` and the 6.0 API for tools such as typescript-eslint. Nightlies move to `typescript@next`. The old preview package was `@typescript/native-preview`.
- **Types:** Template-literal inference keeps a whole Unicode scalar. `HeadTail<"😀abc">` is `["😀", "abc"]`. JavaScript checking is closer to TypeScript files: a value used as a type is written `typeof someValue`, and several old JSDoc forms are no longer special.

**When to use:**

- `npm install -D typescript` and `npx tsc` for the 7.0 binary after the project builds on 6 without `ignoreDeprecations`.
- Keep `"typescript": "npm:@typescript/typescript6@^6.0.2"` when a tool still imports the 6 API, and add `"@typescript/native": "npm:typescript@^7.0.2"` for the 7 `tsc`.

**Examples:**

```json
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```

**Sources:**

- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)
- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)

</details>

<details>
<summary>How do you use Template Literal Types?</summary>

### How do you use Template Literal Types?

**Definition:**

A template literal type builds a string type from pieces: `` `${K}Changed` ``. Unions in the pieces multiply. `Uppercase`, `Lowercase`, `Capitalize`, and `Uncapitalize` change the letters of a string literal type.

**How it works:**

- **Event names:** TypeScript 4.1’s `on` method is generic in `K extends string & keyof T`. The event argument is `` `${K}Changed` ``. The callback receives `T[K]`. `firstNameChanged` passes a `string`. `ageChanged` passes a `number`.
- **Remapping:** A mapped type can rename keys with `as`. `` `get${Capitalize<string & K>}` `` turns `name` into `getName`.
- **Casing:** `Uppercase<"hello">` is `"HELLO"`. These intrinsics are not locale-aware. Runtime strings still use JavaScript’s string methods.
- **Inference:** TypeScript 4.2 does not infer a template literal type from a value automatically. `` `hello ${name}` as const `` keeps `` `hello ${string}` ``. Without `as const`, the type is `string`.

**When to use:**

- Name a finite set of string APIs, such as `${Key}Changed`, when the key set is small. The 4.1 notes say a large union should be generated ahead of time.
- Use `as const` when a computed string must stay a template literal type.

**Examples:**

```ts
type PropEventSource<T> = {
  on<K extends string & keyof T>(
    eventName: `${K}Changed`,
    callback: (newValue: T[K]) => void,
  ): void;
};
```

**Sources:**

- [TypeScript 4.1 — Template literal types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html)
- [TypeScript 4.2 — as const on template values](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-2.html)

</details>

<details>
<summary>Discuss the improvements to Inference in Conditional Types.</summary>

### Discuss the improvements to Inference in Conditional Types.

**Definition:**

`infer` can name a piece of a type and constrain it with `extends`. TypeScript 4.8 parses a matching string or tuple element as the literal that fit the constraint, not as the wide primitive.

**How it works:**

- **Constraint:** `T extends [infer U extends number, ...unknown[]] ? U : never` takes the first tuple element only when it is a number. Otherwise the type is `never`.
- **Template strings:** `` "100" extends `${infer U extends number}` ? U : never `` is the literal `100`, not `number`. The same pattern maps `"100"` to `` 100n `` for `bigint` and `"true"` to `true` for `boolean`.
- **Unicode:** TypeScript 7 infers one code point. `` S extends `${infer Head}${infer Tail}` `` on `"😀abc"` is `["😀", "abc"]`. Older checkers split the emoji into two UTF-16 units.
- **Distribution:** A naked type parameter in a conditional still distributes over a union. The `extends` constraint filters each inferred piece. It does not turn the runtime value into that literal.

**When to use:**

- Constrain `infer` when you need to reject a piece that is the wrong primitive and keep the literal that matched.
- Leave the constraint off when any piece of that position is acceptable.

**Examples:**

```ts
type SomeNum = "100" extends `${infer U extends number}` ? U : never;
```

**Sources:**

- [TypeScript 4.8 — infer constraints](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-8.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>What is the use of labeled tuple elements?</summary>

### What is the use of labeled tuple elements?

**Definition:**

A label names a tuple position for readers and for tooling: `[start: number, end: number]`. The label does not change assignability. `[number, number]` is still the same type.

**How it works:**

- **Since 4.0:** Labels exist so a tuple reads like a parameter list. They are not checked as different from an unlabeled tuple of the same element types.
- **Since 5.2:** A tuple may mix labeled and unlabeled elements. Before that, every element had a label or none did. Spreading a labeled tuple into an unlabeled one used to drop the labels. TypeScript 5.2 keeps them.
- **API:** `TupleType.labeledElementDeclarations` may be `undefined` at an unlabeled index so mixed tuples can be represented.
- **Rest:** You no longer need a dummy label such as `rest` on a rest element just to satisfy the old all-or-nothing rule.

**When to use:**

- Label a tuple that is part of a public function type, so call sites show `start` and `end`.
- Skip labels on a tuple that is only a short internal pair and the names would not help.

**Examples:**

```ts
type Range = [start: number, end: number];

function span(range: Range): number {
  const [start, end] = range;
  return end - start;
}
```

**Sources:**

- [TypeScript 4.0 — Labeled tuple elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-0.html)
- [TypeScript 5.2 — Named and anonymous tuple elements](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html)

</details>

### TypeScript Community and Development

<details>
<summary>How can you contribute to the TypeScript community or TypeScript itself?</summary>

### How can you contribute to the TypeScript community or TypeScript itself?

**Definition:**

Publish types with your package, or send them to DefinitelyTyped when they are not generated from your source. The compiler itself is developed in the open. Nightly builds are `typescript@next`.

**How it works:**

- **Your library:** `"declaration": true` and a `"types"` field ship `.d.ts` next to the JavaScript. npm shows the TypeScript icon only when `"types"` is set.
- **Someone else’s library:** The publishing handbook says to submit those declarations to DefinitelyTyped. They are published as `@types/...`. Stop shipping a second `@types` package once the library has its own `"types"`.
- **The compiler:** TypeScript 7 is the native compiler and stays compatible with 6.0 checking. It has no programmatic API until 7.1. Tools that import `typescript` keep using `@typescript/typescript6` (`tsc6`).
- **Trying changes:** `npm install -D typescript@next` installs the nightly. The 7.0 announcement says the old preview name was `@typescript/native-preview`.

**When to use:**

- Add `"types"` on the next release of a library you maintain.
- Open a DefinitelyTyped change when you fixed a wrong declaration for a dependency you do not control.

**Examples:**

```text
npm install -D typescript@next
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Discuss the role of TypeScript in open-source development.</summary>

### Discuss the role of TypeScript in open-source development.

**Definition:**

Open-source libraries either ship `.d.ts` or rely on DefinitelyTyped. The compiler is the shared checker editors and tools call. TypeScript 7.0 speeds that checker up and leaves the 6.0 API in a separate package until 7.1.

**How it works:**

- **Packages:** `"types"` points at the declaration entry. Consumers import the package and get checking without an extra install. `@types` covers packages that never added that field.
- **Tools:** typescript-eslint type-aware rules need a TypeScript program. Until 7.1 they import the 6.0 API from `@typescript/typescript6` while `tsc` can be TypeScript 7.
- **Apps:** Angular, Vue, Express, Jest, and Playwright document TypeScript entry points: component classes, `<script lang="ts">`, `Request`/`Response`, and `import { test } from "@playwright/test"`.
- **Checking JavaScript:** `allowJs` lets the project read `.js`. `// @ts-check` at the top of a JavaScript file asks the editor to check it. TypeScript 7 narrows which JSDoc forms it still treats as types.

**When to use:**

- Ship declarations from the source that generates them so DefinitelyTyped is not a second source of truth.
- Pin the 6 API package only for tools that still import it. Day-to-day `tsc` can be 7.

**Examples:**

```json
{
  "name": "awesome",
  "main": "./lib/main.js",
  "types": "./lib/main.d.ts"
}
```

**Sources:**

- [TypeScript handbook — Publishing](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

<details>
<summary>Explain how to stay updated with TypeScript's changes and best practices.</summary>

### Explain how to stay updated with TypeScript's changes and best practices.

**Definition:**

Read the release notes before you upgrade, and clear deprecations on TypeScript 6 before you move to 7. Nightlies are `npm install -D typescript@next`.

**How it works:**

- **Stable notes:** The 6.0 handbook page is the transition release. `"ignoreDeprecations": "6.0"` hides warnings there. TypeScript 7 removes those options, including `target: es5` and `moduleResolution: node`.
- **7.0:** The announcement describes the native compiler, `--checkers`, the missing programmatic API, and the new defaults (`strict`, `types: []`, `rootDir: "./"`). Editor support is the language server protocol. VS Code uses its TypeScript 7 extension until the editor ships it.
- **Nightly:** `typescript@next` replaces `@typescript/native-preview` for builds after 7.0.
- **Project baseline:** `tsc --init` on 5.9 already turns on `strict`, `verbatimModuleSyntax`, and `isolatedModules`. New code should keep those. Do not copy a config that turns `strict` off.

**When to use:**

- Upgrade one major at a time: 6 with no `ignoreDeprecations`, then 7.
- Skim the release notes when a build starts failing after an install. The failure is often a removed flag, not a changed type in your code.

**Examples:**

```text
npm install -D typescript@next
npx tsc --noEmit
```

**Sources:**

- [TypeScript 6.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

### Transitioning from JavaScript to TypeScript

<details>
<summary>What steps should a JavaScript developer take to start using TypeScript effectively?</summary>

### What steps should a JavaScript developer take to start using TypeScript effectively?

**Definition:**

Install the compiler, add a `tsconfig.json` with `strict`, and let existing JavaScript stay in the program with `allowJs` while you type the boundaries.

**How it works:**

- **Install:** `npm install -D typescript` gives you TypeScript 7’s `tsc`. Run it with `npx tsc`. `tsc --init` on 5.9 writes `strict`, `module: nodenext`, `target: esnext`, `verbatimModuleSyntax`, and `isolatedModules`.
- **Existing files:** `"allowJs": true` makes TypeScript read `.js`. `"declaration": true` with `"emitDeclarationOnly": true` can write `.d.ts` from those files. `// @ts-check` on a single JavaScript file turns on checking in the editor.
- **Globals:** TypeScript 6 and 7 default `types` to `[]`. Install `@types/node` and add `"node"` when you use Node globals. Install `@types` only for packages that do not ship `"types"`.
- **First types:** Annotate function parameters that were implicit `any`. `noImplicitAny` is part of `strict`. Leave inference on local variables whose initializer already has a type.

**When to use:**

- Rename a file to `.ts` when you are ready to fix its errors. Keep the rest `.js` until then.
- Turn on the editor’s workspace TypeScript version so the squiggles match `npx tsc`.

**Examples:**

```json
{
  "compilerOptions": {
    "allowJs": true,
    "strict": true,
    "outDir": "dist",
    "types": ["node"]
  },
  "include": ["src/**/*"]
}
```

**Sources:**

- [TypeScript 5.9 — tsc --init](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-9.html)
- [TypeScript handbook — Creating .d.ts from .js](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)
- [Announcing TypeScript 7.0](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)

</details>

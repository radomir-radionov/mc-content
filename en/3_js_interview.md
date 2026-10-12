# JS — Interview

JavaScript interview questions. Answers are filled for the full list.

[← Back to JS hub](./3_js.md)

### Related docs

- [Language](./3_js_core.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Arrays](./3_js_array.md) · [Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md)
- [Document](./3_js_browser.md) · [Events](./3_js_events.md) · [Browser APIs](./3_js_browser_extra.md) · [Async](./3_js_async.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — JavaScript](https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/).

### JavaScript Fundamentals

<details>
<summary>What are the data types present in JavaScript?</summary>

### What are the data types present in JavaScript?

**Definition:**

JavaScript has primitive and composite data types.

**How it works:**

#### Primitive Data Types

- **Boolean**: Represents logical values of true or false.

- **Null**: Denotes the lack of a value.

- **Undefined**: Indicates a variable that has been declared but has not been assigned a value.

- **Number**: Represents numeric values, including integers and floats.

- **BigInt**: Allows for representation of integers with arbitrary precision.

- **String**: Encapsulates sequences of characters.

- **Symbol (ES6)**: Provides a unique, immutable value.

#### Composite Data Types

- **Object**: Represents a set of key-value pairs and is used for more complex data structures.

- **Function**: A callable object that can be defined using regular function syntax or using the new Function() constructor (rarely used).

#### Notable Characteristics

JavaScript is dynamically-typed, meaning the data type of a variable can change during the execution of a program.
Data type coercion can occur, where values are implicitly converted from one type to another in specific contexts, such as during comparisons.
Arithmetic operations, particularly when one of the operands is a string, can lead to implicit type conversions.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What is the difference between null and undefined?</summary>

### What is the difference between null and undefined?

**Definition:**

While both null and undefined represent "no value" in JavaScript, they are distinct in their roles and origins.

Origin and Context null usually denotes an intentionally absent value, and developers can set a variable to null to signify the absence of an object or a value. For example, if an API call doesn't return data, you might set a variable to null.

undefined typically indicates a variable that has been declared but not yet been assigned a value, or a property that doesn't exist on an object.

**How it works:**

#### Function Arguments

When a function is called, and the parameter isn't provided or its value is not set, the parameter is undefined.
null would instead be an explicit value provided as an argument.

#### Object Properties

If you try to access a property on an object that doesn't exist, the result is undefined.

Null can be used to clear a property value in an object that was previously set.

obj.prop = null;

#### The Equality Operation

In JavaScript, undefined and null are treated as equal when using loose equality (==) but not strict equality (===).

**When to use:**

#### Use-Cases and Best Practices

When you initialize a variable and are not ready to assign a meaningful value, it's more common to use undefined instead of null to indicate that the value isn't there yet.
For example, if you declare a user object but don't have their details yet, you might keep it as undefined.

**Examples:**

#### Variable Initialization and Assignment

Variables that haven't been assigned a value are undefined by default, unless explicitly set to null.

```js
let foo; // undefined
let bar = null; // null
```

```js
let obj = {};
console.log(obj.nonExistentProperty); // undefined
```

```js
let obj = { prop: 'value' };
```

#### Code Example

Here is the JavaScript code:

```js
let var1;
let var2 = null;

let object = {
  a: 1,
  b: undefined
};

function test(arg1, arg2) {
  console.log(arg1);  // undefined: not provided
  console.log(arg2);  // null: provided as such
}

function clearProperty(prop) {
  delete object[prop];
}

console.log(var1);     // undefined
console.log(var2);     // null
console.log(object.a); // 1
console.log(object.b); // undefined
console.log(object.c); // undefined
```

test();               // Both arguments are undefined test(1, null);        // arg1 is 1, arg2 is null

clearProperty('b');  // Removes property 'b' from object

```js
console.log(object.b); // undefined: Property 'b' was removed, not set to null
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>How does JavaScript handle type coercion?</summary>

### How does JavaScript handle type coercion?

**Definition:**

Type Coercion in JavaScript refers to the automatic conversion of values from one data type to another.

**How it works:**

#### Explicit and Implicit Coercion

- **Explicit**: Achieved through methods such as parseInt(), Number(), and toString().

- **Implicit**: Automatically occurs during operations or comparisons. For example, combining a string and a number in an addition results in the automatic conversion of the number to a string.

#### Common Coercion Scenarios

- **Arithmetic Operations**: Strings are coerced to numbers.

- **Example**: "5" - 3 evaluates to 2, as the string is coerced to a number.

- **Loose Equality (==)**: Data types are often modified for comparisons.

- **Example**: "4" == 4 is true due to string coercion before the comparison.

- **Conditionals (if and Ternary Operators)**: Truthiness or falsiness is determined.

- **Example**: if(1) evaluates to true because 1 coerces to true.

- **Logical Operators**: Non-boolean values are coerced to booleans.

- **Example**: "hello" && 0 evaluates to 0 because the truthy "hello" short-circuits the && operation, and 0 coerces to false.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Explain the concept of hoisting in JavaScript.</summary>

### Explain the concept of hoisting in JavaScript.

**Definition:**

Hoisting is a JavaScript mechanism that involves moving variable and function declarations to the top of their containing scope during the compile phase. However, the assignments to these variables or the definitions of functions remain in place.

For instance, even though the call to myFunction appears before its definition, hoisting ensures that it doesn't cause an error.

**How it works:**

#### Hoisting in Action

#### Why Hoisting Matters

Understanding hoisting can help you prevent certain unexpected behaviors in your code. For example, it can shed light on unexpected "undefined" values that might appear even after a variable is declared and initialized.

#### Local Scope and Hoisting

Variables and functions declared in local scopes within functions are also hoisted to the top of their scope.

#### Best Practices

To write clean, readable code, it's important to:

Declare variables at the top of your scripts or functions to avoid hoisting-related pitfalls.
Initialize variables before use, regardless of hoisting, to ensure predictable behavior.

#### ES6 and Hoisting

With the introduction of let and const in ES6, JavaScript's behavior has adapted. Variables declared using let and const are still hoisted, but unlike var, they are not initialized.

**Examples:**

#### Here's a Code Example

```js
console.log(myVar); // Undefined
var myVar = 5;

console.log(myVar); // 5

// The above code is equivalent to the following during the compile phase:
// var myVar;
// console.log(myVar);
// myVar = 5;

console.log(sayHello()); // "Hello, World!"
function sayHello() {
    return "Hello, World!";
}

// The above code is equivalent to the following during the compile phase:
// function sayHello() {
//     return "Hello, World!";
// }
// console.log(sayHello());
```

#### Global Scope and Hoisting

In the global scope, variables declared with var and functions are always hoisted to the top. For example:

```js
// During the compile phase, the following global declarations are hoisted:
// var globalVar;
// function globalFunction() {}

console.log(globalVar); // Undefined
console.log(globalFunction()); // "Hello, Global!"
var globalVar = "I am global Var!";
function globalFunction() {
    return "Hello, Global!";
}
```

#### Here's a Code Example

```js
function hoistingInLocalScope() {
    // These local declarations are hoisted during the compile phase:
    // var localVar;
    // function localFunction() {}

    console.log(localVar); // Undefined
    localVar = "I am a local var!";
    console.log(localFunction()); // "Hello, Local!"

    var localVar;
    function localFunction() {
        return "Hello, Local!";
    }
}
```

#### Here's an Example

```js
console.log(myLetVar); // ReferenceError: Cannot access 'myLetVar' before initialization
let myLetVar = 5;
```

Constants and Hoisting const and let behave similarly when hoisted, but their difference lies in the fact that const must be assigned a value at the time of declaration, whereas let does not require an initial value.

#### Here's an Example

```js
console.log(myConstVar); // ReferenceError: Cannot access 'myConstVar' before initialization
const myConstVar = 10;

console.log(myLetVar); // Undefined
let myLetVar = 5;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What is the scope in JavaScript?</summary>

### What is the scope in JavaScript?

**Definition:**

Scope defines the accessibility and lifetime of variables in a program. In JavaScript, there are two primary types: Global Scope and Local Scope.

**How it works:**

#### Global Scope

Any variable declared outside of a function is in the global scope. These can be accessed from both within functions and from other script tags.

#### Local Scope

Variables declared within a function (using let or const or prior to JavaScript ES6 with var) have local scope, meaning they are only accessible within that function.

#### Block Scope

Starting from ES6, JavaScript also supports block scope, where variables defined inside code blocks (denoted by {} such as loops or conditional statements) using let or const are accessible only within that block.

**Examples:**

#### Example: Global Scope

Here is the JavaScript code:

```js
let globalVar = 'I am global';

function testScope() {
    console.log(globalVar); // Output: 'I am global'
}

testScope();
console.log(globalVar); // Output: 'I am global'
```

#### Example: Local Scope

Here is the JavaScript code:

```js
function testScope() {
    let localVar = 'I am local';
    console.log(localVar); // Output: 'I am local'
}

// This statement will throw an error because localVar is not defined outside the function scope
// console.log(localVar);
```

#### Example: Block Scope

Here is the JavaScript code:

```js
function testScope() {
    let localVar = 'I am local';
    if (true) {
        let blockVar = 'I am local to this block';
        console.log(localVar, blockVar); // Both will be accessible
    }
    // This statement will throw an error because blockVar is not defined outside the block scope
    // console.log(blockVar);
}

testScope();
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What is the difference between == and ===?</summary>

### What is the difference between == and ===?

**Definition:**

Strict equality (===) in JavaScript requires both value and type to match, testing for more specific conditions and reducing the likelihood of unexpected results.

In contrast, the abstract equality comparison (==) can lead to type coercion, potentially causing counterintuitive outcomes.

While both comparison modes test value equality, === ensures an additional match of data type.

**How it works:**

#### Abstract Equality

5 == '5' evaluates to true because JavaScript converts the string to a number for comparison.

#### Strict Equality

5 === '5' evaluates to false because the types are not the same.

#### Key Considerations

- **Type Safety**: === is safer as it avoids unwanted type conversions.

- **Performance**: === can be faster, especially for simple comparisons, as it doesn't involve type coercion or additional checks.

- **Clarity**: Favoring === can make your code clearer and more predictable.

#### Common Best Practices

- **Use Strict Equality by Default**: This approach minimizes unintended side effects.

- **Consider Type Coercion Carefully**: In specific cases or with proven understanding, == can be suitable, but be cautious about potential confusion.

**Examples:**

#### Illustrative Example: Abstract vs. Strict Equality

#### Code Example: Equality Operators

Here is the JavaScript code:

```js
// Abstract equality
console.log('5' == 5);      // true
console.log(null == undefined);  // true
console.log(0 == false);    // true

// Strict equality
console.log('5' === 5);     // false
console.log(null === undefined); // false
console.log(0 === false);   // false
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Describe closure in JavaScript. Can you give an example?</summary>

### Describe closure in JavaScript. Can you give an example?

**Definition:**

In JavaScript, closures enable a function to access its outer scope, retaining this access even after the parent function has finished executing. This mechanism provides a powerful tool for data encapsulation and privacy.

**How it works:**

#### Core Concept

When a function is defined within another function, it maintains a reference to the variables from the outer function, even after the outer function has completed execution and its local variables are typically no longer accessible.

#### Key Components

- **Outer Function (Parent function)**: It contains the inner functions or closures.

- **Inner Function (Closure)**: Defined within the parent function, it references variables from the outer function.

- **Lexical Environment**: The context where the inner function is defined, encapsulating the scope it has access to.

#### Pitfalls to Avoid

- **Memory Leakage**: If not used carefully, closures can cause memory leaks, as the outer function's variables continue to live in memory because of the closure link.

- **Stale Data**: Be mindful of shared variables that might change after a closure has been defined, leading to unexpected behavior.

#### Browser Compatibility

The concept of closures is a fundamental aspect of the JavaScript language and is supported by all modern browsers and environments.

**When to use:**

#### Application

- **Data Privacy**: JavaScript design patterns like the Module and Revealing Module Patterns use closures to keep data private.

- **Timeouts and Event Handlers**: Closures help preserve the surrounding context in asynchronous operations such as setTimeout and event handlers.

**Examples:**

#### Example: Password Generator

Consider a simple scenario of a function in charge of generating a secret password:

The outer function, generatePassword, defines a local variable, password and returns an inner function getPassword.
The inner function, getPassword, has exclusive access to the password variable even after generatePassword has executed.
Here is the JavaScript code:

```js
function generatePassword() {
  let password = '';
  const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  const passwordLength = 8;
  for(let i = 0; i < passwordLength; i++) {
    password += characters.charAt(Math.floor(Math.random() * characters.length));
  }

  return function getPassword() {
      return password;
  };
}

const getPassword = generatePassword();

console.log(getPassword()); // Outputs the generated password.
```

In this example, getPassword still has access to the password variable after the generatePassword function has completed, thanks to the closure mechanism.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What is the 'this' keyword and how does its context change?</summary>

### What is the 'this' keyword and how does its context change?

**Definition:**

In JavaScript, the context of this refers to the execution context, typically an object that owns the function where this is used.

'this' in the Global Scope
In non-strict mode, this in the global scope refers to the window object. In strict mode, this is undefined.

'this' in Functions
In non-arrow functions, the value of this depends on how the function is invoked. When invoked:

- **As a method of an object**: this is the object.

- **Alone**: In a browser, this is window or global in Node.js. In strict mode, it's undefined.

- **With call, apply, or bind**: this is explicitly set.

- **As a constructor (with new)**: this is the newly created object.

'this' in Arrow Functions
Arrow functions have a fixed context for this defined at function creation and are not changed by how they are invoked.

They do not have their own this.
They use the this from their surrounding lexical context (the enclosing function or global context).

**Examples:**

#### Code Example: Global Context

Here is the JavaScript code:

```js
// Main
let globalVar = 10;

function globalFunction() {
    console.log('Global this: ', this.globalVar);
    console.log('Global this in strict mode: ', this);
}
```

- **globalFunction();  // Output**: 10, window or undefined (in strict mode)

// In Node.js, it will be different, because "window" is not defined. But "this" will refer to the global object.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What are arrow functions and how do they differ from regular functions?</summary>

### What are arrow functions and how do they differ from regular functions?

**Definition:**

Let's look at the key features of arrow functions and how they differ from traditional functions in JavaScript.

**How it works:**

#### Arrow Functions: Key Features

#### Concise Syntax

Especially useful for short, one-liner functions.
No need for function keyword or braces if there's a single expression.

#### Implicit Return

When there's no explicit { return ... ;} statement, arrow functions return the result of the single expression inside.

#### this Binding

Does not have its own this. It's "inherited" from the surrounding (lexical) context. This feature is known as 'lexical scoping'.

**When to use:**

#### When to Use Arrow Functions

- **Event Handlers**: Ideal for concise, inline event handling, where this context can be inherited from the lexical scope.

- **Callback Functions**: Useful for array methods like map, filter, and reduce.

- **Avoidance of this Redefinition**: When you want to maintain the surrounding context of this and avoid unintended redefinition.

**Examples:**

#### Code Example: Standard Function vs. Arrow Function

Here is the JavaScript code:

```js
// Standard Function
function greet(name) {
  return "Hello, " + name + "!";
}

// Arrow Function
const greetArrow = name => "Hello, " + name + "!";
```

In the code above, greet is a standard function, while greetArrow is an arrow function, showcasing the difference in syntax and required keywords.

#### Code Example: Arrow Function and this Context

Here is the JavaScript code:

```js
// Using traditional functions
document.getElementById('myButton').onclick = function() {
  console.log('Button clicked:', this);  // Refers to the button element
};

// Using arrow functions
document.getElementById('myButton').onclick = () => {
  console.log('Button clicked:', this);  // Refers to the global/window object
};
```

In the arrow function example, the context of this does not refer to the button element, but to the global window object, because arrow functions do not have their own binding of this. Instead, they inherit this from their lexical scope, which in this case is the global context.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What are template literals in JavaScript?</summary>

### What are template literals in JavaScript?

**Definition:**

Template literals are a feature in modern JavaScript versions that offer a more flexible and readable way to work with strings. They are often referred to as "template strings".

**How it works:**

#### Key Features

- **Multiline Text**: Template literals support multiline strings without requiring escape characters or string concatenation with a + sign.

- **String Interpolation**: They enable the seamless embedding of JavaScript expressions within strings, using ${}.

#### Syntax

- **Single Versus Double Quotes**: For template literals, use backticks (`) instead of single quotes ('') or double quotes ("").

- **Placeholder**: The ${expression} placeholder within the backticks allows for variable and expression injection.

#### Benefits

- **Readability**: They can make code more understandable, especially when dealing with longer or complex strings, by keeping content closer to its intention.

- **Interpolation & Expression**: Template literals reduce verbosity and rendering logic when integrating dynamic data.

#### Browser Compatibility Concerns

Template literals are universally supported in modern browsers and are now considered a core JavaScript feature. However, they may not work in older browsers such as Internet Explorer without transpilation or polyfilling.

**Examples:**

#### Example

```js
let name = "John";
let message = `Hi ${name}!`;

console.log(message);  // Output: "Hi John!"
```

#### Code Example: Multiline Text and String Interpolation

```js
// Regular String
let poem = "Roses are red,\nViolets are blue,\nSugar is sweet,\nAnd so are you.";

// Template Literal
let poemTemplate = `
  Roses are red,
  Violets are blue,
  Sugar is sweet,
  And so are you.
`;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

### JavaScript Functions and Higher-Order Functions

<details>
<summary>What is a higher-order function in JavaScript?</summary>

### What is a higher-order function in JavaScript?

**Definition:**

A higher-order function in JavaScript is a function that can take other functions as arguments or can return functions. This feature enables functional programming paradigms such as map, reduce, and filter. Higher-order functions offer versatility and modularity, fostering streamlined, efficient code.

**How it works:**

#### Key Characteristics

- **First-class functions**: Functions in JavaScript are considered first-class, meaning they are a legitimate data type and can be treated like any other value, including being assigned to variables, stored in data structures, or returned from other functions.

- **Closure support**: Due to closures, a higher-order function can transport not just the enclosed data within the function definition, but also the lexical environment in which that data resides.

- **Dynamic code**: Because JavaScript allows functions to be dynamically constructed and named, they can be dynamically passed to higher-order functions.

**When to use:**

#### Practical Applications

- **Callback Execution**: Functions like setTimeout and addEventListener take a function as an argument and are thus higher-order.

- **Event Handling**: Many event-driven systems leverage higher-order functions for tasks such as event subscription and emission.

- **Iterative Operations**: The map, filter, and reduce functions in JavaScript operate on arrays and require functions to be passed, making them higher-order.

- **Code Abstraction**: Higher-order functions enable the encapsulation of repetitive tasks, promoting cleaner, more readable code.

**Examples:**

#### Code Example: Higher-order Functions

Here is the JavaScript code:

```js
// Simple higher-order function
function multiplier(factor) {
  return function(num) {
    return num * factor;
  };
}

// Invoke a higher-order function
const twice = multiplier(2);
console.log(twice(5));  // Output: 10

// Functional programming with higher-order functions
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(multiplier(2));  // [2, 4, 6, 8, 10]
const tripled = numbers.map(multiplier(3));  // [3, 6, 9, 12, 15]
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>Can functions be assigned as values to variables in JavaScript?</summary>

### Can functions be assigned as values to variables in JavaScript?

**Definition:**

Yes, JavaScript supports first-class functions, meaning functions can be treated as variables and then assigned to other variables or passed as arguments to other functions.

Functions defined as regular functions or arrow functions are both first-class in JavaScript.

**When to use:**

#### Practical Use Cases

- **Callbacks**: Functions can be passed as parameters to other functions.

- **Event Handling**: In web development, functions define how to respond to specific events, and these functions are often attached to event listeners.

- **Modular Development**: In programming patterns like the Module pattern, functions are defined within a scope and then returned, similar to variables.

- **Higher-Order Functions**: These functions operate on other functions, taking them as arguments or returning them, and are an essential part of many modern JavaScript libraries and frameworks.

**Examples:**

#### Practical Code Example

Here is the JavaScript code:

```js
// Define a function
function greet() {
  console.log('Hello!');
}

// Assign the function to a variable
let sayHello = greet;

// Call the function through the variable
```

sayHello();  // Output: "Hello!"

```js
// Reassign the variable to a new function
sayHello = function() {
  console.log('Bonjour!');
};

// Call it again to see the new behavior
```

sayHello();  // Output: "Bonjour!"

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>How do functional programming concepts apply in JavaScript?</summary>

### How do functional programming concepts apply in JavaScript?

**Definition:**

Functional Programming (FP) concepts in JavaScript are a direct result of the language's first-class functions. Key FP principles, such as immutability, pure functions, and declarative style, play a crucial role.

**How it works:**

#### Core Concepts

#### First-Class Functions and Higher-Order Functions

JavaScript treats functions as first-class citizens, allowing them to be assigned to variables, passed as parameters, and returned from other functions. This feature is foundational to FP in the language.

**Examples:**

#### Code Example

Here is the JavaScript code:

```js
const sayHello = () => console.log('Hello!');
const runFunction = (func) => func();
```

runFunction(sayHello);  // Output: "Hello!"

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>What are IIFEs (Immediately Invoked Function Expressions)?</summary>

### What are IIFEs (Immediately Invoked Function Expressions)?

**Definition:**

The Immediately Invoked Function Expression (IIFE) design pattern employs an anonymous function that gets executed promptly after its definition.

Key characteristics of IIFEs include localized variable scopes and immediate activation upon interpreter parsing.

**How it works:**

#### Core Functions of IIFE

- **Encapsulation**: Through lexical scoping, IIFEs safeguard variables from leaking into the global scope. This, in turn, averts unintended variable tampering in the global context.

- **Data Hiding**: Internal functions or data can be hidden from external access, providing a mechanism for information concealment and access control.

- **Initialization**: The IIFE structure is ideal for setting up initial conditions, like binding events or pre-processing data.

#### Modern Alternatives

With the introduction of ES6 and its let and const declarations, as well as block-scoped lexical environments, the necessity of IIFEs has reduced. Additionally, arrow functions provide a more concise method for defining immediately invoked functions.

#### IIFE Variants

- **Parentheses Invocation**: A pair of parentheses immediately invoke the enclosed function. While this approach is more extensive, it's devoid of self-documenting advantages.

- **Wrapping in Operators**: Similar to using parentheses for invocation, the !, +, or - operators are sometimes used for invoking clarity. For instance:

- **Named IIFE**: Though not as common, naming an IIFE can assist with self-referencing. This is most effective when the intention is to have a more comprehensive stack trace during debugging.

#### Caution on Minification

When leveraging IIFEs, exercise caution while using minifiers to shrink JavaScript files. Minification might lead to unintended outcomes, altering the previous scope expectations.

**When to use:**

#### Use Cases

- **Avoiding Variable Pollution**: When interfacing with libraries or inserting code snippets, IIFEs prevent global scope pollution.

- **Module Patterns**: IIFEs, in combination with closures, lay the groundwork for modular code organization by shielding private variables and functions.

**Examples:**

#### Code Example: IIFE

Here is the JavaScript code:

```js
(function(){
    var foo = 'bar';
    console.log(foo);
})();
```

In this example, the function is enclosed within parentheses, ensuring the enclosed function is evaluated as an expression. Subsequently, it is invoked with a trailing pair of parentheses.

```js
(function(){
    console.log('Invoked!');
})();
```

```js
!function(){
    console.log('Invoked!');
}();
```

```js
(function factorial(n){
    if (n <= 1) return 1;
    return n * factorial(n-1);
})(5);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

<details>
<summary>How do you create private variables in JavaScript?</summary>

### How do you create private variables in JavaScript?

**Definition:**

In JavaScript, encapsulating private state within an object can be achieved using a closure. This ensures the state is local to the object and not directly accessible from outside.

**How it works:**

#### How Closures Work

A closure allows a function to retain access to the lexical environment (the set of variable bindings at the point of function declaration) in which it was defined, even when the function is executed outside that lexical environment.

This means that any inner function, defined inside another function, has access to the outer function's variables, and that access is maintained even after the outer function has finished executing.

#### Practical Implementation with Constructor Functions and Modules

#### Constructor Functions

In this example, secret is private to each Gadget instance, thanks to closure.

#### Modules

In modern JavaScript, module patterns combined with immediately-invoked function expressions (IIFE) are often used for encapsulation and data hiding.

The revealing module pattern enables selective exposure of private members.

The IIFE pattern immediately executes and returns the object to be assigned, effectively creating a module.

myModule.privateMethod();  // Throws an error because privateMethod is not exposed
In this example, privateVariable and privateMethod are accessible only within the IIFE's lexical environment, thus making them private.

JavaScript tools like TypeScript and Babel also offer modules such as module.export, providing additional options for encapsulation.

**Examples:**

#### For example

```js
function outerFunction() {
    let outerVar = 'I am outer';  // This variable is in the lexical environment of outerFunction

    function innerFunction() {
        console.log(outerVar);  // Accesses outerVar from the lexical environment of outerFunction
    }

    return innerFunction;
}

let myInnerFunction = outerFunction();
```

- **myInnerFunction();  // Logs**: "I am outer"

Here, innerFunction retains access to outerVar.

When defining a JavaScript constructor function with function and new, closure can be used to associate private state with each instance:

```js
function Gadget() {
    let secret = 'top secret';
    this.setSecret = function (value) {
        secret = value;
    };
    this.getSecret = function () {
        return secret;
    };
}

let phone = new Gadget();
phone.setSecret('new secret');
console.log(phone.getSecret());  // 'new secret'
```

Here is the code:

```js
let myModule = (function () {
    let privateVariable = 'I am private';

    function privateMethod() {
        console.log('I am a private method');
    }

    return {
        publicMethod: function () {
            console.log('I am a public method');
        },
        getPrivateVariable: function () {
            return privateVariable;
        }
    };
})();

console.log(myModule.getPrivateVariable());  // 'I am private'
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/javascript-interview-questions/
</details>

### JavaScript Objects and Prototypes

<details>
<summary>How do you create an object in JavaScript?</summary>

### How do you create an object in JavaScript?

**Definition:**

An object is a dynamic collection of own properties. You create one with an object literal, `Object.create`, `new Object()`, or `new` with a constructor or class.

**How it works:**

- **Object literal:** `{ name: "Ada" }` creates an object whose `[[Prototype]]` is `Object.prototype`, unless the literal sets `__proto__`.
- **`Object.create(proto)`:** creates an object whose `[[Prototype]]` is `proto` (`null` means no prototype). A second argument adds properties by descriptor.
- **`new Object()`:** creates an ordinary object, the same kind of result as `{}`.
- **`new Constructor()` / `new Class()`:** creates an object, sets its `[[Prototype]]` to the constructor’s `.prototype`, and runs the function with `this` bound to that object.

**Examples:**

```js
const point = { x: 1, y: 2 };

const withProto = Object.create(point);
withProto.z = 3;

function Box(w) {
  this.w = w;
}
const box = new Box(4);
```

**Sources:**

- [MDN — Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [MDN — Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [javascript.info — Objects](https://javascript.info/object)
</details>

<details>
<summary>What are prototypes in JavaScript?</summary>

### What are prototypes in JavaScript?

**Definition:**

A prototype is the object another object links to through the internal `[[Prototype]]` slot. Property lookup continues on that object when the name is not an own property.

**How it works:**

- **`[[Prototype]]`:** read with `Object.getPrototypeOf(obj)` and set with `Object.setPrototypeOf`. The standard literal form is `{ __proto__: other }`. The `obj.__proto__` accessor is legacy.
- **End of the chain:** the walk stops at `null`. `null` has no prototype.
- **`Function.prototype`:** a different thing. It is the object assigned as `[[Prototype]]` of instances created with `new` on that function. It is not the function’s own `[[Prototype]]`.
- **Ordinary objects:** a literal’s prototype is `Object.prototype`, which itself ends at `null`.

**Examples:**

```js
const parent = { kind: "parent" };
const child = Object.create(parent);

Object.getPrototypeOf(child) === parent; // true
child.kind; // "parent"
```

**Sources:**

- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [MDN — Object.getPrototypeOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)
- [javascript.info — Prototypal inheritance](https://javascript.info/prototype-inheritance)
</details>

<details>
<summary>Explain prototypal inheritance.</summary>

### Explain prototypal inheritance.

**Definition:**

Prototypal inheritance reuses properties by linking objects. A missing property is sought on the prototype, then on that prototype’s prototype, until a match or `null`.

**How it works:**

- **Own properties first:** if the object has the name, that value is used. A same-named property on the prototype is hidden (shadowing).
- **Inherited methods:** a function stored on the prototype still runs with `this` set to the object you called it on, not to the prototype.
- **Assignment writes an own property:** setting `child.x = 1` does not change the prototype, unless a setter on the chain intercepts the write.
- **Classes:** `class` and `extends` are syntax over this chain. They do not add a second inheritance model.

**Examples:**

```js
const parent = {
  value: 2,
  method() {
    return this.value + 1;
  },
};

const child = { __proto__: parent, value: 10 };
child.method(); // 11
```

**Sources:**

- [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [javascript.info — Prototypal inheritance](https://javascript.info/prototype-inheritance)
- [javascript.info — F.prototype](https://javascript.info/function-prototype)
</details>

<details>
<summary>What is the difference between object literals and constructor functions?</summary>

### What is the difference between object literals and constructor functions?

**Definition:**

An object literal builds one object inline. A constructor function (or class) builds many objects that share one `.prototype` and run the same initialization with `new`.

**How it works:**

- **Literal:** `{ a: 1 }` evaluates to a single object. Its `[[Prototype]]` is `Object.prototype` unless you set `__proto__`.
- **Constructor:** `new Foo()` creates an object, links `[[Prototype]]` to `Foo.prototype`, binds `this` to the new object, and returns that object unless the function returns another object.
- **Shared methods:** put methods on `Foo.prototype` (or in the class body) so every instance finds them on the chain instead of getting its own function copy.
- **Classes:** a `class` is a constructor function with stricter `new` rules and methods installed on `.prototype`.

**When to use:**

- A literal for one configuration object, a map of data, or a one-off bag of fields.
- A constructor or class when you need many instances, shared methods, and a clear initialization step.

**Examples:**

```js
const once = { label: "solo" };

function User(name) {
  this.name = name;
}
User.prototype.greet = function () {
  return `Hi, ${this.name}`;
};

const ada = new User("Ada");
ada.greet(); // "Hi, Ada"
```

**Sources:**

- [MDN — Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [MDN — new operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)
- [MDN — Using classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_classes)
- [javascript.info — Constructor, operator new](https://javascript.info/constructor-new)
</details>

<details>
<summary>How do you add or remove properties from an object?</summary>

### How do you add or remove properties from an object?

**Definition:**

Assign to a property to add or replace an own property. The `delete` operator removes an own configurable property. It does not free memory by itself.

**How it works:**

- **Add or update:** `obj.key = value` or `obj[key] = value` creates or overwrites an own data property.
- **`delete obj.key`:** removes that own property and returns `true`. A missing property also yields `true` and changes nothing.
- **Inherited names:** `delete` only affects own properties. After the own property is gone, a same-named property on the prototype shows through again.
- **Non-configurable:** `delete` returns `false` in non-strict code and throws `TypeError` in strict mode. `var` globals and built-ins such as `Math.PI` are non-configurable. `let` and `const` bindings are not object properties, so `delete` cannot remove them.
- **Arrays:** `delete arr[i]` leaves a hole and does not change `length`. `splice` removes the element and shifts the rest.

**When to use:**

- Assignment for a normal field.
- `delete` when the key should disappear (`in` becomes false).
- `splice` when an array should shrink, not become sparse.

**Examples:**

```js
const user = { name: "Ada", role: "dev" };
user.city = "London";
delete user.role;

"role" in user; // false
user.city; // "London"
```

**Sources:**

- [MDN — delete](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete)
- [MDN — Working with objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects)
- [javascript.info — Object properties configuration](https://javascript.info/property-descriptors)
</details>

### Asynchronous JavaScript

<details>
<summary>What is the event loop in JavaScript?</summary>

### What is the event loop in JavaScript?

**Definition:**

The event loop runs JavaScript tasks one after another on a single call stack, then drains the microtask queue before taking the next task. Waiting on I/O does not block that turn by itself.

**How it works:**

- **Call stack:** synchronous code runs to completion. A long loop holds the thread until it returns.
- **Tasks:** `setTimeout`, `setInterval`, and I/O callbacks are tasks. The loop takes the oldest runnable task for one turn.
- **Microtasks:** after that task (and after script evaluation), the loop runs every queued microtask, including ones enqueued by those microtasks, before the next task. Promise reactions and `queueMicrotask` are microtasks.
- **Rendering:** the browser may paint after microtasks are empty, then start the next turn.
- **Order:** synchronous logs run first, then promise `.then` callbacks, then a `setTimeout(..., 0)` callback.

**Examples:**

```js
console.log("sync");
setTimeout(() => console.log("task"), 0);
Promise.resolve().then(() => console.log("microtask"));
// sync, microtask, task
```

**Sources:**

- [MDN — Using microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)
- [MDN — Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [javascript.info — Event loop](https://javascript.info/event-loop)
</details>

<details>
<summary>Explain how callbacks work in JavaScript.</summary>

### Explain how callbacks work in JavaScript.

**Definition:**

A callback is a function you pass so another function can call it later. Timers, events, and many async APIs report completion by invoking that function.

**How it works:**

- **Sync callback:** `array.map(fn)` calls `fn` before `map` returns. The caller stays on the stack.
- **Async callback:** `setTimeout(fn, 0)` returns immediately. `fn` runs later as a task, after the current stack clears and microtasks drain.
- **Error-first style:** Node-style callbacks often use `(error, result)`. The first argument is the failure, or `null` when the call succeeded.
- **Nesting:** several async steps that each take a callback form a pyramid. Promises and `async`/`await` flatten that sequence.

**Examples:**

```js
function load(url, callback) {
  setTimeout(() => callback(null, `body of ${url}`), 0);
}

load("/notes", (error, body) => {
  if (error) return;
  console.log(body);
});
```

**Sources:**

- [MDN — Callback function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- [MDN — setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)
- [javascript.info — Introduction: callbacks](https://javascript.info/callbacks)
</details>

<details>
<summary>What are promises and how do they manage asynchronous code?</summary>

### What are promises and how do they manage asynchronous code?

**Definition:**

A `Promise` is an object that stands for a value that may not exist yet. It is pending, then fulfilled with a value or rejected with a reason. Reactions in `.then`, `.catch`, and `.finally` run as microtasks.

**How it works:**

- **Executor:** `new Promise(executor)` runs `executor` immediately. `resolve(value)` fulfills; `reject(reason)` rejects. A thrown error inside the executor rejects the promise.
- **One settlement:** a promise settles once. Later `resolve` or `reject` calls do nothing.
- **Chaining:** `.then` returns a new promise. Return a value to fulfill it, throw or return a rejected promise to reject it. Returning a promise waits for that promise.
- **Errors:** a rejection skips to the next `.catch` (or a `.then` with a rejection handler). Without a handler it becomes an unhandled rejection.
- **Combinators:** `Promise.all` waits for every fulfillment and rejects on the first rejection. `Promise.allSettled` waits for every result. `Promise.race` settles with the first settlement. `Promise.any` fulfills with the first fulfillment.

**Examples:**

```js
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

delay(10)
  .then(() => "done")
  .then((value) => console.log(value));
```

**Sources:**

- [MDN — Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN — Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [javascript.info — Promises](https://javascript.info/promise-basics)
</details>

<details>
<summary>Explain async/await in JavaScript and how it differs from Promises.</summary>

### Explain async/await in JavaScript and how it differs from Promises.

**Definition:**

`async` / `await` is syntax for consuming promises. An `async` function always returns a promise. `await` pauses that function until the awaited promise settles, then resumes as a microtask.

**How it works:**

- **`async`:** the return value is wrapped in a fulfilled promise. A thrown exception rejects that promise.
- **`await`:** waits for a thenable. A fulfilled value becomes the expression result. A rejection throws inside the async function, so `try` / `catch` handles it.
- **Same queues:** `await` does not add a new scheduling model. The continuation is a promise reaction, so it still runs before the next timer task.
- **Difference from `.then`:** the steps read top to bottom in one function. `.then` builds an explicit chain of callbacks. Both produce promises.

**When to use:**

- `async` / `await` for a sequence of steps and `try` / `catch`.
- `.then` when you already hold a promise and want a small chain, or when you return a promise from a non-async callback.

**Examples:**

```js
async function loadUser(id) {
  const response = await fetch(`/users/${id}`);
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN — await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [javascript.info — Async/await](https://javascript.info/async-await)
</details>

<details>
<summary>What is the Job Queue (or Microtask Queue)?</summary>

### What is the Job Queue (or Microtask Queue)?

**Definition:**

The microtask queue (the ECMAScript job queue for promise jobs) holds callbacks that run after the current JavaScript task finishes and before the next task, such as a timer.

**How it works:**

- **Who enqueues:** `promise.then` / `catch` / `finally`, `await` continuations, and `queueMicrotask(fn)`.
- **When it drains:** each time a task is about to return to the event loop, every queued microtask runs, including microtasks added while draining.
- **Versus tasks:** `setTimeout(fn, 0)` is a task. It runs only after the microtask queue is empty.
- **Starvation:** a microtask that keeps queueing another microtask delays the next task and can delay rendering. Stop the chain when the work is done.

**Examples:**

```js
setTimeout(() => console.log("task"), 0);
queueMicrotask(() => console.log("microtask"));
console.log("sync");
// sync, microtask, task
```

**Sources:**

- [MDN — Using microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide)
- [MDN — queueMicrotask()](https://developer.mozilla.org/en-US/docs/Web/API/Window/queueMicrotask)
- [javascript.info — Microtasks](https://javascript.info/microtask-queue)
</details>

### DOM Manipulation and Browser APIs

<details>
<summary>How do you select DOM elements using JavaScript?</summary>

### How do you select DOM elements using JavaScript?

**Definition:**

Query the document with CSS selectors or with the older id, tag, and class lookups. `querySelector` returns the first match or `null`. `querySelectorAll` returns every match.

**How it works:**

- **`document.querySelector(selectors)`:** depth-first search for the first element. An invalid selector string throws `SyntaxError`. Pseudo-elements never match. A duplicate id returns the first element with that id.
- **`document.querySelectorAll(selectors)`:** a static `NodeList` of all matches. It does not update when the DOM changes later.
- **`getElementById`:** one element by id, or `null`.
- **`getElementsByTagName` / `getElementsByClassName`:** a live `HTMLCollection`. It reflects later DOM changes.
- **Scoped search:** `element.querySelector` searches inside that element, not the whole document.

**Examples:**

```js
const form = document.querySelector("form.login");
const fields = form.querySelectorAll("input");
const title = document.getElementById("title");
```

**Sources:**

- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [MDN — Document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [javascript.info — Searching: getElement*, querySelector*](https://javascript.info/searching-elements-dom)
</details>

<details>
<summary>Explain event propagation in the DOM.</summary>

### Explain event propagation in the DOM.

**Definition:**

A DOM event goes through three phases: capturing from the window down to the target, the target phase, then bubbling back up. `event.eventPhase` reports which phase the current listener is in.

**How it works:**

- **Capturing:** listeners registered with `capture: true` (or the third argument `true`) run on the way down.
- **Target:** listeners on the element that dispatched the event run. `event.target` is that element. `event.currentTarget` is the element the listener is attached to.
- **Bubbling:** listeners with `capture: false` (the default) run on the way up. `click` bubbles. Not every event does.
- **`stopPropagation()`:** stops the event from reaching listeners on other nodes in the capture and bubble phases. Other listeners on the same element still run. It does not cancel the browser’s default action.
- **`stopImmediatePropagation()`:** also stops the remaining listeners on the current element.

**Examples:**

```js
parent.addEventListener("click", () => console.log("capture"), true);
child.addEventListener("click", () => console.log("target"));
parent.addEventListener("click", () => console.log("bubble"));
```

**Sources:**

- [MDN — Event.eventPhase](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase)
- [MDN — Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)
- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>How do you prevent a form from submitting using JavaScript?</summary>

### How do you prevent a form from submitting using JavaScript?

**Definition:**

Listen for the form’s `submit` event and call `event.preventDefault()`. That cancels the browser’s default action, which is to navigate to the form’s action URL.

**How it works:**

- **`submit`:** fires on the `<form>` when a submit control is activated, before the navigation. The event bubbles.
- **`preventDefault()`:** tells the browser the event is handled, so the default action does not run. Propagation continues unless you also call `stopPropagation()`.
- **Cancelable:** `preventDefault` has no effect when `event.cancelable` is false. A listener registered as `passive: true` cannot cancel the event.
- **After cancel:** read `FormData` or the field values and send them yourself, for example with `fetch`.

**Examples:**

```js
const form = document.querySelector("form");
form.addEventListener("submit", (event) => {
  event.preventDefault();
  const data = new FormData(form);
  console.log(data.get("email"));
});
```

**Sources:**

- [MDN — Event.preventDefault()](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault)
- [MDN — HTMLFormElement: submit event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)
- [javascript.info — Form properties and methods](https://javascript.info/form-elements)
</details>

<details>
<summary>What are Web APIs in the context of JavaScript?</summary>

### What are Web APIs in the context of JavaScript?

**Definition:**

Web APIs are interfaces the browser (and some other hosts) expose to JavaScript. The ECMAScript language does not define `document`, `fetch`, or `localStorage`. The host does.

**How it works:**

- **Language vs host:** syntax, objects, and promises are JavaScript. The DOM, network, storage, and history objects are Web APIs you call from JavaScript.
- **DOM:** `document`, elements, and events. You read and change the page through these objects.
- **Network and time:** `fetch`, `WebSocket`, `setTimeout`. Completion is reported through promises or callbacks on the event loop.
- **Storage and history:** `localStorage`, `sessionStorage`, and `history.pushState` are window APIs, scoped to the page’s origin.

**Examples:**

```js
const title = document.querySelector("h1");
title.textContent = "Notes";

const response = await fetch("/api/notes");
const notes = await response.json();
```

**Sources:**

- [MDN — Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)
- [MDN — Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
</details>

<details>
<summary>How can you manipulate the browser history using JavaScript?</summary>

### How can you manipulate the browser history using JavaScript?

**Definition:**

`history.pushState` adds a session-history entry for the current document without loading a new page. `popstate` fires when the user later navigates to an entry created that way.

**How it works:**

- **`pushState(state, unused, url)`:** `state` must be serializable and is stored with the entry. The second argument is unused; pass `""`. `url` is optional and must be same-origin. The browser does not fetch that URL.
- **`replaceState`:** replaces the current entry instead of pushing a new one.
- **`popstate`:** fires on `window` when the active entry changes because the user moved through session history. `event.state` is a copy of the state object. Calling `pushState` itself does not fire `popstate`.
- **`back`, `forward`, `go`:** move through the session history. `go(-1)` matches `back()`, `go(1)` matches `forward()`. `go()` and `go(0)` do the same as `location.reload()`. The move is asynchronous; listen for `popstate` to know when it finished.

**Examples:**

```js
history.pushState({ page: 2 }, "", "/notes?page=2");

window.addEventListener("popstate", (event) => {
  console.log(event.state);
});
```

**Sources:**

- [MDN — History.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)
- [MDN — Window: popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)
- [MDN — History.go()](https://developer.mozilla.org/en-US/docs/Web/API/History/go)
- [MDN — History.replaceState()](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState)
</details>

### ES2015+ and Modern JavaScript Features

<details>
<summary>What are the new features introduced in ES6?</summary>

### What are the new features introduced in ES6?

**Definition:**

ES2015 (ES6) added block-scoped bindings, shorter functions and classes, destructuring, modules, and several new built-in objects. Later editions added more; “ES6” means this 2015 set.

**How it works:**

- **`let` and `const`:** block scope. Reading them before the declaration throws `ReferenceError` (the temporal dead zone). `var` is function-scoped and initializes as `undefined`.
- **Arrow functions:** a shorter function form. They do not have their own `this`, `arguments`, or `prototype`.
- **Classes:** syntax over constructor functions and the prototype chain, including `extends`.
- **Destructuring, defaults, rest, and spread:** unpack arrays and objects, fill missing arguments, and gather or expand lists.
- **Modules:** `import` and `export` give a file its own scope. Load one in the browser with `<script type="module">`.
- **Also in that edition:** template literals, `Promise`, `Map`, `Set`, `Symbol`, and `for...of`.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
const { name } = user;
const greet = (who) => `Hello, ${who}`;
```

**Sources:**

- [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>How do you use destructuring assignments in ES6?</summary>

### How do you use destructuring assignments in ES6?

**Definition:**

Destructuring unpacks array elements or object properties into distinct variables. It works in a declaration, an assignment, a parameter list, or a `for...of` binding.

**How it works:**

- **Arrays:** `const [a, b] = pair` takes values by position. A hole (`const [a, , b]`) skips one. Any iterable can be unpacked this way.
- **Objects:** `const { name } = user` copies the property `name`. Rename with `{ name: label }`.
- **Defaults:** a default applies when the value is missing or `undefined`. It does not apply when the value is `null`.
- **Rest:** `...rest` must be last. On an array it builds a new array of the leftovers. On an object it copies the remaining enumerable own properties into a new object.
- **Assignment form:** `({ a } = obj)` needs parentheses. Without them `{ a }` is parsed as a block.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
const { name, role = "dev" } = user;
const [first, ...rest] = ["a", "b", "c"];
```

**Sources:**

- [MDN — Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring)
- [javascript.info — Destructuring assignment](https://javascript.info/destructuring-assignment)
</details>

<details>
<summary>Explain the use of const and let keywords.</summary>

### Explain the use of const and let keywords.

**Definition:**

`let` and `const` declare block-scoped bindings. `const` must be initialized and cannot be reassigned. `let` may be assigned later and can be reassigned.

**How it works:**

- **Temporal dead zone:** from the start of the block until the line that declares the name, reading it throws `ReferenceError`. `var` would have been `undefined` there.
- **`const`:** the binding is fixed. Properties of a `const` object can still change. `const user = { name: "Ada" }; user.name = "Grace"` is allowed. `user = {}` is not.
- **`let`:** one binding per block. You cannot declare the same `let` twice in that block. A `for` loop’s `let` is a new binding per iteration.
- **Not properties:** `let` and `const` do not become properties of the global object, and `delete` cannot remove them.

**When to use:**

- `const` for a binding you do not reassign.
- `let` when the variable must take a new value, such as a loop index or an accumulator.

**Examples:**

```js
const origin = { x: 0, y: 0 };
origin.x = 1;

let step = 0;
step += 1;
```

**Sources:**

- [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Grammar and types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types)
</details>

<details>
<summary>What are default parameters in JavaScript functions?</summary>

### What are default parameters in JavaScript functions?

**Definition:**

A default parameter is a value used when the caller omits that argument or passes `undefined`. The default expression runs only in that case.

**How it works:**

- **Trigger:** missing arguments and explicit `undefined` use the default. Any other value, including `null`, `0`, and `""`, is kept.
- **Later parameters:** a default may read earlier parameters, because they are already initialized.
- **Destructuring:** `function draw({ size = "big" } = {})` defaults the whole argument to `{}`, so a call with no arguments does not throw, and `size` still defaults when the property is missing.

**Examples:**

```js
function greet(name = "there") {
  return `Hello, ${name}`;
}

greet(); // "Hello, there"
greet(undefined); // "Hello, there"
greet(null); // "Hello, null"
```

**Sources:**

- [MDN — Default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [javascript.info — Destructuring assignment](https://javascript.info/destructuring-assignment)
</details>

<details>
<summary>Explain the concept of modules in ES6.</summary>

### Explain the concept of modules in ES6.

**Definition:**

An ES module is a file that exports values and imports them into other files. Each module has its own scope. In the browser you load the entry file with `<script type="module">`.

**How it works:**

- **`export`:** marks a binding or a default value that other modules may import. `export const name = "square"` is a named export. `export default` is the default export.
- **`import`:** `import { name } from "./square.js"` reads a named export. `import draw from "./square.js"` reads the default. In a browser a bare name such as `"square"` throws `TypeError` unless an import map resolves it. Node can resolve bare names from the file system.
- **Loading:** modern browsers run modules without a transpiler. A bundler is still useful for minification, dead-code elimination, and splitting chunks.
- **`import()`:** a function-like form that loads a module on demand and returns a promise. Static `import` is hoisted and runs first.

**When to use:**

- Static `import` for dependencies the page always needs.
- Dynamic `import()` for a route, a locale, or a heavy feature that can wait.

**Examples:**

```js
// square.js
export function area(length) {
  return length * length;
}

// main.js
import { area } from "./square.js";
area(4); // 16
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [javascript.info — Modules, introduction](https://javascript.info/modules-intro)
</details>

### Event Handling

<details>
<summary>How do you handle events in JavaScript?</summary>

### How do you handle events in JavaScript?

**Definition:**

Register a listener with `addEventListener` on the element that should observe the event. The browser calls that function when the event reaches the element.

**How it works:**

- **`addEventListener(type, listener, options)`:** `type` is a string such as `"click"` or `"submit"`. The listener receives an `Event`.
- **Options:** `capture: true` runs during the capture phase. `once: true` removes the listener after it runs. `passive: true` tells the browser you will not call `preventDefault` (and a call inside that listener does not cancel the action).
- **`this`:** a regular function listener has `this` equal to `event.currentTarget`. An arrow function keeps the `this` of the surrounding scope.
- **Remove:** `removeEventListener` needs the same function reference and the same capture flag.

**Examples:**

```js
const button = document.querySelector("button");

function onClick(event) {
  console.log(event.currentTarget === this);
}

button.addEventListener("click", onClick);
```

**Sources:**

- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [javascript.info — Introduction to browser events](https://javascript.info/introduction-browser-events)
</details>

<details>
<summary>What is event delegation and why is it useful?</summary>

### What is event delegation and why is it useful?

**Definition:**

Event delegation is one listener on a parent that handles events from its children. It works because the event bubbles, and `event.target` is the element that started it.

**How it works:**

- **`target` vs `currentTarget`:** `target` is the clicked child. `currentTarget` is the parent that owns the listener.
- **New children:** elements added later still bubble to the parent, so you do not attach a listener to each one.
- **Filter:** ignore clicks that are not on the control you care about, for example when `event.target.closest("li")` is `null`.
- **Limit:** the event must reach the parent. `stopPropagation` on a child, or an event that does not bubble, skips the parent listener.

**When to use:**

- A list, menu, or table whose rows are added and removed.
- A single listener when many similar children would each need the same handler.

**Examples:**

```js
const list = document.querySelector("ul");

list.addEventListener("click", (event) => {
  const item = event.target.closest("li");
  if (!item || !list.contains(item)) return;
  item.classList.toggle("done");
});
```

**Sources:**

- [MDN — Event.target](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)
- [javascript.info — Event delegation](https://javascript.info/event-delegation)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>How do you add and remove an event listener from an element?</summary>

### How do you add and remove an event listener from an element?

**Definition:**

`addEventListener` registers a listener. `removeEventListener` removes it only when you pass the same function object and the same capture setting.

**How it works:**

- **Same reference:** an anonymous function passed to `addEventListener` cannot be removed later, because the removal call would be a different function.
- **Capture flag:** a listener added with `capture: true` is removed with `removeEventListener(type, fn, true)`. The default `false` does not match `true`.
- **`once: true`:** the browser removes the listener after the first call. You do not call `removeEventListener` yourself.
- **`AbortSignal`:** pass `{ signal }` in the options. Calling `controller.abort()` removes every listener registered with that signal.

**Examples:**

```js
const button = document.querySelector("button");

function onClick() {
  console.log("clicked");
  button.removeEventListener("click", onClick);
}

button.addEventListener("click", onClick);
```

**Sources:**

- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [MDN — EventTarget.removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)
- [javascript.info — Introduction to browser events](https://javascript.info/introduction-browser-events)
</details>

<details>
<summary>Can you explain how "this" works in event handlers?</summary>

### Can you explain how "this" works in event handlers?

**Definition:**

In a regular function passed to `addEventListener`, `this` is the element the listener is attached to. That is `event.currentTarget`. An arrow function does not get that binding.

**How it works:**

- **Regular function:** `this === event.currentTarget` is true. `this === event.target` is true only when the listener sits on the element that started the event.
- **Arrow function:** `this` comes from the enclosing scope. It is not the element. Read the element from `event.currentTarget` instead.
- **`bind`:** `fn.bind(somethingElse)` sets `this` to that value. The bound function is a new reference, so removal must use that same bound function.
- **Delegation:** on a parent listener, `this` is the parent. The child that was clicked is `event.target`.

**When to use:**

- A regular function when you want `this` to be the element.
- An arrow function when you want the surrounding `this`, and you read the element from the event.

**Examples:**

```js
button.addEventListener("click", function (event) {
  console.log(this === event.currentTarget); // true
});

button.addEventListener("click", (event) => {
  event.currentTarget.classList.add("pressed");
});
```

**Sources:**

- [MDN — this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [MDN — EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

<details>
<summary>What is the difference between event.preventDefault() and event.stopPropagation()?</summary>

### What is the difference between event.preventDefault() and event.stopPropagation()?

**Definition:**

`preventDefault()` cancels the browser’s default action and lets the event keep propagating. `stopPropagation()` stops the event from reaching other nodes and does not cancel the default action.

**How it works:**

- **`preventDefault()`:** no parameters and no return value. It works only when `event.cancelable` is true. A `passive: true` listener cannot cancel the event; the call is ignored and the console may warn.
- **Default actions:** following a link, submitting a form, toggling a checkbox, scrolling. Canceling one of these does not stop parent listeners.
- **`stopPropagation()`:** stops capture and bubble listeners on other nodes. Other listeners already on the current element still run.
- **`stopImmediatePropagation()`:** also stops the remaining listeners on the current element. It still does not by itself cancel the default action.

**When to use:**

- `preventDefault` when the browser should not navigate, submit, or apply its own action.
- `stopPropagation` when a parent listener should not also see this event.
- Both when you need to cancel the action and keep it from bubbling.

**Examples:**

```js
link.addEventListener("click", (event) => {
  event.preventDefault();
});

row.addEventListener("click", (event) => {
  event.stopPropagation();
});
```

**Sources:**

- [MDN — Event.preventDefault()](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault)
- [MDN — Event.stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)
- [javascript.info — Bubbling and capturing](https://javascript.info/bubbling-and-capturing)
</details>

### Web Storage and Security

<details>
<summary>What is the difference between localStorage, sessionStorage, and cookies?</summary>

### What is the difference between localStorage, sessionStorage, and cookies?

**Definition:**

`localStorage` and `sessionStorage` keep string data in the browser and do not send it with HTTP requests. A cookie is a small name/value pair the browser stores and usually sends back to the server.

**How it works:**

- **`localStorage`:** one `Storage` object per origin (scheme, host, and port). It has no expiry. Data stays across sessions. In private browsing it is cleared when the last private tab closes. Keys and values are UTF-16 strings. `setItem`, `getItem`, `removeItem`, and `clear` read and write them.
- **`sessionStorage`:** partitioned by origin and by top-level tab. It survives reloads and restores in that tab and is cleared when the tab or window closes. A new tab gets a new session. If the page has an opener, the new storage starts as a copy; later writes stay separate.
- **Cookies:** the server sets them with `Set-Cookie`. The browser sends them on later requests in the `Cookie` header. MDN cites a usual size around 4KB and a limited count per domain, so they travel on every matching request. `document.cookie` can read and write cookies that are not `HttpOnly`.
- **Cookie limits:** `HttpOnly` hides the cookie from JavaScript. `Secure` sends it only on HTTPS (localhost is an exception, not in Safari). `SameSite` is `Strict`, `Lax`, or `None` and controls cross-site sending. `Expires` or `Max-Age` set a lifetime; without them the cookie is a session cookie.

**When to use:**

- `localStorage` for a preference that should survive a closed tab and is not a secret.
- `sessionStorage` for data that should die with the tab, such as a draft on that tab.
- A cookie for state the server must see, such as a session id, with `HttpOnly`, `Secure`, and `SameSite`.

**Examples:**

```js
localStorage.setItem("theme", "dark");
sessionStorage.setItem("draft", "hello");

document.cookie = "lang=en; Max-Age=86400; Secure; SameSite=Lax";
```

**Sources:**

- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN — Window.sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- [MDN — Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
- [javascript.info — LocalStorage, sessionStorage](https://javascript.info/localstorage)
</details>

<details>
<summary>Can you explain Cross-Site Scripting (XSS) and how to prevent it?</summary>

### Can you explain Cross-Site Scripting (XSS) and how to prevent it?

**Definition:**

XSS is an attack where a site runs an attacker’s code as if it were the site’s own code. That code can read the page, read `localStorage`, and send requests with the user’s credentials.

**How it works:**

- **Two steps:** the site accepts input an attacker can craft, then inserts that input into a page without making sure it cannot run as JavaScript.
- **A common sink:** assigning a string to `innerHTML` parses it as HTML. An `<img>` with an `onerror` handler can run script. `<script>` tags inserted this way do not run, but other markup still can.
- **Plain text:** `textContent` inserts text and does not parse HTML.
- **Cookies:** mark a session cookie `HttpOnly` so `document.cookie` cannot read it.
- **CSP:** a `Content-Security-Policy` header can restrict which scripts load and can require Trusted Types before HTML is injected.

**Examples:**

```js
const name = userInput;
label.textContent = name;

// unsafe if userInput is attacker-controlled:
// box.innerHTML = userInput;
```

**Sources:**

- [MDN — Cross-site scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
- [MDN — Element.innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
- [MDN — Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
</details>

<details>
<summary>What is Cross-Origin Resource Sharing (CORS) and how does it work?</summary>

### What is Cross-Origin Resource Sharing (CORS) and how does it work?

**Definition:**

CORS is an HTTP-header mechanism. The server names which other origins (scheme, host, and port) a browser may let scripts read. `fetch` and `XMLHttpRequest` follow the same-origin policy unless those headers allow the response.

**How it works:**

- **Same origin:** a page on `https://domain-a.com` that `fetch`es `https://domain-b.com/data.json` is cross-origin. The browser exposes the body to the script only if the response opts in, typically with `Access-Control-Allow-Origin`.
- **Preflight:** for methods other than `GET`, `HEAD`, and `POST`, or for headers outside the safelist, the browser first sends `OPTIONS` and describes the method and headers it intends to use. The actual request follows only if the server allows them.
- **Form-like requests:** a “simple” request can go out without a preflight, but the script still cannot read the response until `Access-Control-Allow-Origin` allows it.
- **Credentials:** cookies and HTTP authentication are included only when the client asks and the server explicitly allows credentials.
- **Errors:** a CORS failure rejects the script call. The JavaScript error does not include the server’s reason; the details are in the browser console.

**Examples:**

```js
const response = await fetch("https://domain-b.com/data.json");
if (!response.ok) throw new Error(response.statusText);
const data = await response.json();
```

**Sources:**

- [MDN — Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
</details>

<details>
<summary>How does content security policy (CSP) help in preventing security attacks?</summary>

### How does content security policy (CSP) help in preventing security attacks?

**Definition:**

CSP is a set of instructions, usually in the `Content-Security-Policy` response header, that tells the browser what the page is allowed to load and do. Its main use is to limit which JavaScript can run, which blocks many XSS payloads.

**How it works:**

- **Header:** send it on responses, not only the document. A `<meta http-equiv="Content-Security-Policy">` works for some policies, and it does not support every directive.
- **Directives:** semicolon-separated. `default-src 'self'` allows resources from the page’s own origin. `img-src` can add extra hosts. `script-src` controls scripts.
- **XSS:** an injected inline handler or a script from an unlisted host is refused when the policy does not allow it.
- **Other jobs:** restrict who may embed the page (clickjacking), upgrade insecure requests to HTTPS, and require Trusted Types before client-side HTML injection.
- **Deploy:** the directives are independent. You can ship a framing restriction without a script restriction.

**Examples:**

```http
Content-Security-Policy: default-src 'self'; img-src 'self' example.com
```

**Sources:**

- [MDN — Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
- [MDN — Cross-site scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
</details>

### JavaScript Debugging

<details>
<summary>What tools and techniques do you use for debugging JavaScript code?</summary>

### What tools and techniques do you use for debugging JavaScript code?

**Definition:**

Use the browser’s developer tools: the console for values and errors, and the debugger to pause on a line. `try` / `catch` handles exceptions you expect; it is not a substitute for seeing the call stack.

**How it works:**

- **Console:** `console.log`, `console.error`, and `console.trace` print values and the stack while the page runs.
- **`debugger`:** the statement pauses when developer tools are open, as if a breakpoint were on that line.
- **Debugger:** the debugger shows the call stack, lets you step, and can watch expressions.
- **Exceptions:** an uncaught throw stops the current turn and is reported in the console. `catch` runs only for a throw inside its `try`.

**Examples:**

```js
function parseCount(text) {
  const value = Number(text);
  if (Number.isNaN(value)) {
    console.error("not a number", text);
    return null;
  }
  return value;
}
```

**Sources:**

- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
</details>

<details>
<summary>How do you debug a JavaScript application in the browser?</summary>

### How do you debug a JavaScript application in the browser?

**Definition:**

Open developer tools, reproduce the bug, and read the console first. Then pause in the Sources panel on the line that runs the bad state and inspect variables at that moment.

**How it works:**

- **Console:** syntax errors, uncaught exceptions, and failed `fetch` calls show here. A CORS failure’s detail is in the console, not in the JavaScript error object.
- **Pause:** set a breakpoint, or insert `debugger`, with tools open. Execution stops before the next statement.
- **Step:** step over a call to stay in this function, or step into it. The call stack lists the functions that led here.
- **Network:** the Network panel shows the request URL, status, and body when the bug is a failed HTTP call.

**Examples:**

```js
button.addEventListener("click", () => {
  debugger;
  const name = input.value;
  console.log(name);
});
```

**Sources:**

- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
</details>

<details>
<summary>Explain the concept and use of breakpoints.</summary>

### Explain the concept and use of breakpoints.

**Definition:**

A breakpoint is a place where the debugger pauses JavaScript so you can inspect the call stack and current bindings. You set it in the Sources panel, or you write the `debugger` statement.

**How it works:**

- **Line breakpoint:** execution stops before that line runs. You can then read local variables.
- **`debugger`:** same pause, but only when developer tools are open. Leave it out of code you ship.
- **Step controls:** continue runs until the next breakpoint. Step over runs a call without entering it. Step into enters the call.
- **Conditional use:** pause on the iteration or the branch that fails instead of logging every pass.

**Examples:**

```js
for (const item of items) {
  if (item.id === brokenId) {
    debugger;
  }
}
```

**Sources:**

- [MDN — debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)
- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
- [javascript.info — Debugging in the browser](https://javascript.info/debugging-chrome)
</details>

<details>
<summary>How do you handle exceptions in JavaScript?</summary>

### How do you handle exceptions in JavaScript?

**Definition:**

`try` / `catch` / `finally` handles a thrown exception. `try` runs first. If it throws, `catch` runs. `finally` runs before control leaves the whole statement, whether or not something was thrown.

**How it works:**

- **Shape:** you need `try` plus `catch`, `finally`, or both. The bodies must be blocks.
- **Binding:** `catch (error)` holds the thrown value and exists only in the `catch` block. You can omit the binding when you do not need the value.
- **Filter:** test `error instanceof TypeError` (or another built-in error) and rethrow the rest with `throw error`.
- **`finally`:** use it for cleanup. A `return` or `throw` inside `finally` overrides the `return` or `throw` from `try` or `catch`, so keep control-flow statements out of `finally`.
- **Async:** a rejected promise is not a `throw` in the caller unless you `await` it inside `try`, or you attach `.catch`.

**Examples:**

```js
function readJson(text) {
  try {
    return JSON.parse(text);
  } catch (error) {
    if (error instanceof SyntaxError) return null;
    throw error;
  }
}
```

**Sources:**

- [MDN — try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
- [javascript.info — Error handling, try...catch](https://javascript.info/try-catch)
</details>

### Performance and Optimization

<details>
<summary>What techniques can be used to improve JavaScript performance?</summary>

### What techniques can be used to improve JavaScript performance?

**Definition:**

Keep the main thread free for input and painting. Ship less JavaScript, avoid long synchronous work, and load images and extra modules only when they are needed.

**How it works:**

- **Main thread:** a long loop or a heavy synchronous parse blocks clicks and paint until it returns. Split work or move it off the page’s critical path.
- **Less code:** a bundler can minify, drop unused exports (tree-shaking / dead-code elimination), and split chunks. MDN’s modules guide says this does not remove the need for a bundler.
- **On demand:** `import()` loads a module later and returns a promise. `img.loading = "lazy"` asks the browser to postpone an off-screen image.
- **DOM:** update text with `textContent` when you do not need HTML. Reading `innerText` can force a reflow because it depends on computed style.

**Examples:**

```js
const panel = document.querySelector("#details");
panel.hidden = true;
button.addEventListener("click", async () => {
  const { open } = await import("./details.js");
  open(panel);
});
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
</details>

<details>
<summary>How does JavaScript minification and bundling contribute to performance?</summary>

### How does JavaScript minification and bundling contribute to performance?

**Definition:**

Minification makes the file smaller by removing whitespace and shortening local names. Bundling combines modules into fewer files and can drop code that is never imported. The browser then downloads and parses less before the page can run.

**How it works:**

- **Minify:** the transferred bytes go down. The behavior of the program stays the same. A source map lets DevTools show the original files.
- **Bundle:** one graph of `import` / `export` becomes one or more chunks. Fewer requests matter when each request pays a round trip.
- **Dead code:** MDN notes that bundlers also do dead-code elimination and tree-shaking, so an unused export need not ship.
- **Limit:** one giant bundle delays the first run. Split the rarely used part with dynamic `import()` instead of putting it in the first chunk.

**When to use:**

Minify and bundle the code every page needs. Split a heavy optional screen so it is not in the first download.

**Examples:**

```js
const editor = await import("./editor.js");
editor.mount(document.querySelector("#editor"));
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
</details>

<details>
<summary>Discuss the importance of web page loading time and performance.</summary>

### Discuss the importance of web page loading time and performance.

**Definition:**

Loading time is how long the user waits before the page is useful. HTML, CSS, JavaScript, and images all compete for the network and the main thread. A long JavaScript task after the bytes arrive still delays interaction.

**How it works:**

- **Bytes:** large scripts and images take longer to download. Minification, fewer requests, and lazy images cut that wait.
- **Parse and run:** the browser must parse JavaScript before it can run it. A large first bundle delays the first interaction even on a fast network.
- **Main thread:** while a script runs, the page does not handle input. MDN’s microtask guide also warns that a microtask loop can delay the next task and rendering.
- **Images:** `loading="lazy"` on an image outside the viewport postpones that fetch until the image is expected to be needed.

**Examples:**

```js
const img = document.createElement("img");
img.loading = "lazy";
img.width = 320;
img.height = 240;
img.src = "/photos/1.jpg";
img.alt = "Notes";
```

**Sources:**

- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>Explain how lazy loading works in JavaScript.</summary>

### Explain how lazy loading works in JavaScript.

**Definition:**

Lazy loading defers work until it is needed. For an image, `loading="lazy"` tells the browser to postpone the fetch while the image is outside the viewport. For code, dynamic `import()` fetches a module when you call it.

**How it works:**

- **Images:** set `img.loading = "lazy"` (or the HTML attribute). `eager` loads during the initial page load. MDN describes `lazy` as a hint to wait until the image is expected to be needed.
- **Size:** set `width` and `height` so the browser can reserve space before the file arrives.
- **Modules:** `import("./chart.js")` returns a promise of the module namespace. The static `import` graph does not include that file until the call runs.
- **Not a spinner:** the hint can still load early if the browser decides the image is close. Do not rely on it to hide a secret resource.

**When to use:**

`loading="lazy"` for images below the first screen. `import()` for a feature the first view does not need.

**Examples:**

```js
const img = document.createElement("img");
img.loading = "lazy";
img.alt = "Diagram";
img.src = "/diagram.webp";

async function showChart() {
  const chart = await import("./chart.js");
  chart.draw();
}
```

**Sources:**

- [MDN — HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
- [MDN — Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
</details>

### JavaScript Testing

<details>
<summary>What are some JavaScript testing frameworks you know?</summary>

### What are some JavaScript testing frameworks you know?

**Definition:**

The language does not include a test runner. Node.js ships `node:test`. Browser and application projects usually add a library that finds test files, runs them, and reports pass or fail. MDN’s testing module is the starting point for automated checks in front-end work.

**How it works:**

- **`node:test`:** import `describe` and `it` from `node:test` and assert with `node:assert/strict`. Run `node --test`.
- **Libraries:** Jest and Vitest are common runners for application code. They are packages you install; they are not part of the language.
- **Browser flows:** a runner that drives a real page checks what the user sees. That is a different tool from a function-level assert.
- **What they share:** a test calls your code and fails the run when an assertion throws.

**Examples:**

```js
import assert from "node:assert/strict";
import test from "node:test";

test("adds", () => {
  assert.equal(1 + 2, 3);
});
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
</details>

<details>
<summary>How can you write unit tests for JavaScript code?</summary>

### How can you write unit tests for JavaScript code?

**Definition:**

A unit test calls one function with known inputs and checks the return value or the thrown error. It does not start a browser or a server.

**How it works:**

- **Arrange:** build the input in the test.
- **Act:** call the function once.
- **Assert:** `node:assert/strict` uses `Object.is`-style equality and throws `AssertionError` on failure.
- **Isolate:** pass collaborators in as arguments. Do not let the unit reach `fetch` or `document` if you are testing a pure calculation.

**Examples:**

```js
import assert from "node:assert/strict";

function fullName(user) {
  return `${user.first} ${user.last}`;
}

assert.equal(fullName({ first: "Ada", last: "Lovelace" }), "Ada Lovelace");
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
</details>

<details>
<summary>What is Test-Driven Development (TDD) in JavaScript?</summary>

### What is Test-Driven Development (TDD) in JavaScript?

**Definition:**

Test-driven development writes the failing check before the behavior. You add a test that fails, change the function until that test passes, then clean up the code without changing the result.

**How it works:**

- **Red:** the new assertion fails because the behavior is missing.
- **Green:** the smallest change that makes the assertion pass.
- **Refactor:** rename and split while the same test still passes.
- **Runner:** `node --test` (or another runner) is the loop you repeat. The language does not enforce the order; you do.

**Examples:**

```js
import assert from "node:assert/strict";
import test from "node:test";
import { total } from "./total.js";

test("sums prices", () => {
  assert.equal(total([{ price: 2 }, { price: 3 }]), 5);
});
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Can you explain the difference between unit and integration testing?</summary>

### Can you explain the difference between unit and integration testing?

**Definition:**

A unit test checks one function with inputs you control. An integration test checks several real parts together, such as a function that calls `fetch` and reads the JSON body.

**How it works:**

- **Unit:** no network, no DOM, no clock you do not fake. A failure points at that function.
- **Integration:** the pieces are the ones production uses. A failure can be the function, the HTTP status check, or the server.
- **`fetch`:** MDN’s example checks `response.ok` and then `response.json()`. An integration test can run that path against a test server. A unit test of the parser takes the already-parsed value.
- **Both:** an assertion still throws when the result is wrong. They answer different questions.

**When to use:**

A unit test for a pure calculation. An integration test when the bug lives in the join between your code and `fetch`, the DOM, or another module.

**Examples:**

```js
export function cents(products) {
  return products.reduce((sum, item) => sum + item.price, 0);
}

export async function loadCents(url) {
  const response = await fetch(url);
  if (!response.ok) throw new Error(response.statusText);
  return cents(await response.json());
}
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
</details>

### Networking with JavaScript

<details>
<summary>How do you make HTTP requests in JavaScript?</summary>

### How do you make HTTP requests in JavaScript?

**Definition:**

Call the global `fetch(url, options)`. It returns a promise of a `Response`. The promise rejects on a network failure. An HTTP error status such as 404 still fulfills the promise, so you check `response.ok`.

**How it works:**

- **`fetch`:** available on `window` and in workers. Pass a URL string or a `Request`, plus an optional options object.
- **Body:** `response.json()`, `response.text()`, and the other body methods return promises. Call only one of them.
- **Status:** throw when `response.ok` is false. `ok` is true for statuses in the 200–299 range.
- **CORS:** a cross-origin response is readable only when the server allows it. The JavaScript error will not explain the header mismatch.

**Examples:**

```js
async function getNotes() {
  const response = await fetch("/api/notes");
  if (!response.ok) {
    throw new Error(`Response status: ${response.status}`);
  }
  return response.json();
}
```

**Sources:**

- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [javascript.info — Fetch](https://javascript.info/fetch)
- [MDN — Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What is the difference between XMLHttpRequest and Fetch API?</summary>

### What is the difference between XMLHttpRequest and Fetch API?

**Definition:**

`fetch` is the promise-based replacement for `XMLHttpRequest`. `XMLHttpRequest` reports progress through callbacks and event handlers. `fetch` returns a promise and works with service workers and CORS.

**How it works:**

- **`fetch`:** `await fetch(url)` gives a `Response`. Check `response.ok` yourself; a 404 does not reject.
- **`XMLHttpRequest`:** create the object, `open(method, url)`, `send()`, and read `status` and `responseText` from the `load` handler.
- **Both:** cross-origin reads follow CORS. The browser will not expose the body without the right response headers.
- **Progress:** `XMLHttpRequest` has upload and download progress events. `fetch` does not offer that same progress model on the basic API.

**When to use:**

`fetch` for new request code. `XMLHttpRequest` when you must listen to upload or download progress on an API that already uses it.

**Examples:**

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", "/api/notes");
xhr.addEventListener("load", () => {
  console.log(xhr.status, xhr.responseText);
});
xhr.send();
```

**Sources:**

- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN — XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [javascript.info — Fetch](https://javascript.info/fetch)
</details>

<details>
<summary>What is AJAX, and how does it work?</summary>

### What is AJAX, and how does it work?

**Definition:**

AJAX is the pattern of asking the server for data from JavaScript and updating the page without a full navigation. Today that request is usually `fetch`. The older API with the same role is `XMLHttpRequest`.

**How it works:**

- **Request:** the script calls `fetch` or `XMLHttpRequest` after the page has loaded.
- **Response:** you read the body (`response.json()` or `xhr.responseText`) and change the DOM, for example with `textContent`.
- **No full reload:** the browser does not throw away the document the way a normal form navigation does.
- **Name:** the glossary term is the technique. It is not a separate object in the language.

**Examples:**

```js
async function refreshCount() {
  const response = await fetch("/api/count");
  if (!response.ok) throw new Error(response.statusText);
  const data = await response.json();
  document.querySelector("#count").textContent = String(data.count);
}
```

**Sources:**

- [MDN — AJAX](https://developer.mozilla.org/en-US/docs/Glossary/AJAX)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [javascript.info — Fetch](https://javascript.info/fetch)
</details>

<details>
<summary>How do you use WebSockets in a web application?</summary>

### How do you use WebSockets in a web application?

**Definition:**

A `WebSocket` is a long-lived connection. The page sends and receives messages on it without a new HTTP request per message. Construct it with `new WebSocket(url)`, then `send` data and listen for `message`.

**How it works:**

- **Open:** `new WebSocket("ws://localhost:8080")` or `wss:` for TLS. The `open` event fires when the connection is up.
- **Send:** `socket.send(data)` enqueues data. `bufferedAmount` is how many bytes are still queued.
- **Receive:** the `message` event’s `data` is what the server sent.
- **Close:** `socket.close()` or the `close` event. `readyState` is the current state.
- **Backpressure:** the WebSocket API does not apply backpressure. If messages arrive faster than you handle them, memory or CPU can grow. MDN points at `WebSocketStream` when you need backpressure.

**When to use:**

A live feed or chat, where both sides send many small messages. `fetch` for a single request and response.

**Examples:**

```js
const socket = new WebSocket("wss://example.com/live");

socket.addEventListener("open", () => {
  socket.send("hello");
});

socket.addEventListener("message", (event) => {
  console.log(event.data);
});
```

**Sources:**

- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [javascript.info — WebSocket](https://javascript.info/websocket)
</details>

### JavaScript Patterns and Best Practices

<details>
<summary>What is a design pattern in JavaScript?</summary>

### What is a design pattern in JavaScript?

**Definition:**

A design pattern is a repeated way to structure code for a problem you see often. It is not a language feature and not a library. Closures, prototypes, and modules are the tools the common JavaScript patterns are built from.

**How it works:**

- **Name the problem:** one shared instance, hidden state, or a public surface over private functions.
- **Use the language:** a closure keeps variables private. An object literal or a class shares methods. `import` / `export` is the module form the language now has.
- **MDN’s framing:** a framework is a library that adds opinions about how the application is built. A pattern is smaller: a shape you can write in one file.
- **Do not copy blindly:** the pattern should match the problem. A class is enough when you only need a few objects.

**Examples:**

```js
function createCounter() {
  let value = 0;
  return {
    inc() {
      value += 1;
      return value;
    },
  };
}
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
</details>

<details>
<summary>Can you explain the module pattern?</summary>

### Can you explain the module pattern?

**Definition:**

The module pattern hides state in a function scope and returns only the functions that should be public. A closure keeps the private bindings alive after the outer function returns. ES modules (`export` / `import`) are the language’s version of the same idea, one file per module.

**How it works:**

- **Closure:** variables declared in the outer function are not properties of the returned object.
- **Public surface:** the returned object’s methods can read and write those variables.
- **IIFE:** an older form calls the function immediately and assigns the returned object to one variable.
- **ES modules:** `export` marks the public names. Other files `import` them. The module scope is already private.

**When to use:**

An ES module for new code. The IIFE form when you need one private scope inside a file that is not itself a module.

**Examples:**

```js
const counter = (function () {
  let value = 0;
  return {
    inc() {
      value += 1;
      return value;
    },
  };
})();

counter.inc();
```

**Sources:**

- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>What is the Singleton pattern in JavaScript?</summary>

### What is the Singleton pattern in JavaScript?

**Definition:**

A singleton is one shared instance. In JavaScript you usually build it once in a module scope and export that object, so every import receives the same value.

**How it works:**

- **Module scope:** top-level `const` in an ES module is created once when the module is evaluated.
- **Same binding:** other files that `import` it share that one object. They do not each get a new one.
- **Closure form:** an IIFE can keep the instance in a closed-over variable and return it from `getInstance`.
- **Not global:** do not hang the instance on `globalThis` if a module export is enough.

**When to use:**

One shared configuration or cache for the page. A `class` and `new` when each call should be a separate object.

**Examples:**

```js
const settings = { theme: "dark" };
export function getSettings() {
  return settings;
}
```

**Sources:**

- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [javascript.info — Modules, introduction](https://javascript.info/modules-intro)
- [javascript.info — Closure](https://javascript.info/closure)
</details>

<details>
<summary>Explain the Revealing Module pattern.</summary>

### Explain the Revealing Module pattern.

**Definition:**

The revealing module pattern defines functions in a private scope, then returns an object whose properties point at the ones that should be public. Callers see the names you chose. They do not see the rest.

**How it works:**

- **Private:** a function that is not placed on the returned object cannot be called from outside.
- **Reveal:** `return { inc: increment }` publishes `increment` under the name `inc`.
- **Closure:** public methods still read the private `let` bindings.
- **ES modules:** `export { increment as inc }` is the same reveal, without the IIFE.

**Examples:**

```js
const counter = (function () {
  let value = 0;
  function increment() {
    value += 1;
    return value;
  }
  return { inc: increment };
})();
```

**Sources:**

- [javascript.info — Closure](https://javascript.info/closure)
- [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [MDN — Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)
</details>

<details>
<summary>What are some best practices for coding in JavaScript?</summary>

### What are some best practices for coding in JavaScript?

**Definition:**

Prefer `const`, compare with `===`, keep modules small, and do not parse untrusted strings as HTML. Handle failures at the boundary where they can be reported.

**How it works:**

- **Bindings:** `const` when you will not reassign. `let` when you will. Both are block-scoped and sit in the temporal dead zone until the declaration.
- **Equality:** `===` does not coerce. `==` does, which hides type bugs.
- **DOM text:** assign user text to `textContent`. `innerHTML` parses HTML and is a common XSS sink.
- **Async:** `await` inside `try`, and check `response.ok` after `fetch`. A 404 does not reject the promise.
- **Modules:** one file, explicit `export`. Load the entry with `<script type="module">`.

**Examples:**

```js
const title = document.querySelector("#title");
title.textContent = userName;

if (count === 0) {
  title.textContent = "Empty";
}
```

**Sources:**

- [MDN — const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
</details>

### JavaScript Algorithms and Data Structures

<details>
<summary>How do you implement a stack and a queue in JavaScript?</summary>

### How do you implement a stack and a queue in JavaScript?

**Definition:**

An array can be a stack or a queue. A stack adds and removes at the same end (`push` and `pop`). A queue adds at one end and removes at the other (`push` and `shift`).

**How it works:**

- **Stack:** `push` appends. `pop` removes the last item and returns it. The other items stay at their indexes.
- **Queue:** `push` enqueues at the end. `shift` removes index 0 and moves every later element down by one.
- **Cost:** `shift` rewrites the tail of the array on each dequeue. A stack’s `push` / `pop` does not move the earlier items.
- **Empty:** `pop` and `shift` return `undefined` when the array is empty. Check `length` before you treat that as a value.

**When to use:**

`push` / `pop` for a stack. `push` / `shift` for a short queue. For a long queue, a structure that does not reindex on every dequeue is the better fit.

**Examples:**

```js
const stack = [];
stack.push("a");
stack.pop();

const queue = [];
queue.push("a");
queue.shift();
```

**Sources:**

- [MDN — Array.prototype.push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
- [MDN — Array.prototype.pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
- [MDN — Array.prototype.shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
</details>

<details>
<summary>Explain how to sort an array in JavaScript.</summary>

### Explain how to sort an array in JavaScript.

**Definition:**

`array.sort()` sorts the array in place and returns the same array. With no compare function, elements are converted to strings and compared as UTF-16 code units, so `10` sorts before `2`.

**How it works:**

- **Compare function:** return a negative number to put `a` before `b`, a positive number to put `a` after `b`, and `0` when they are equal. `(a, b) => a - b` sorts numbers ascending, if the values are not `NaN`.
- **Stable:** since ECMAScript 2019, `sort` is stable. Equal items keep their original order.
- **Complexity:** MDN says the time and space complexity cannot be guaranteed because it depends on the implementation.
- **Copy:** `toSorted()` returns a new array and does not change the original. A spread copy plus `sort` does the same job on older engines.

**When to use:**

`sort` when you mean to change the array. `toSorted` when callers still need the old order.

**Examples:**

```js
const numbers = [10, 1, 5];
numbers.sort((a, b) => a - b);

const copy = [...numbers].sort((a, b) => b - a);
```

**Sources:**

- [MDN — Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [MDN — Array.prototype.toSorted()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
</details>

<details>
<summary>How do you check if a string is a palindrome?</summary>

### How do you check if a string is a palindrome?

**Definition:**

A palindrome reads the same forwards and backwards. Compare the string with the string you get by reversing its characters.

**How it works:**

- **Reverse:** spread the string into an array of code units, `reverse` that array, and `join` it.
- **Compare:** `===` against the original. This is case-sensitive and keeps spaces.
- **Normalize first:** if the interview ignores case and spaces, lowercase and remove spaces before the compare. Do that in a copy.
- **Two pointers:** walk from both ends and stop at the first mismatch. That avoids a second string.

**Examples:**

```js
function isPalindrome(text) {
  const normalized = text.toLowerCase().replaceAll(" ", "");
  const reversed = [...normalized].reverse().join("");
  return normalized === reversed;
}
```

**Sources:**

- [MDN — String.prototype.replaceAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)
- [MDN — Array.prototype.reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
- [javascript.info — Strings](https://javascript.info/string)
</details>

<details>
<summary>Describe a recursive function and provide an example.</summary>

### Describe a recursive function and provide an example.

**Definition:**

A recursive function calls itself. It needs a base case that returns without another call, or the calls never stop.

**How it works:**

- **Base case:** the smallest input, handled directly.
- **Recursive case:** a smaller input is passed to the same function, and the result is combined.
- **Stack:** each call waits on the stack until the inner call returns. A deep chain can overflow the stack.
- **Same result as a loop:** factorial and a list walk can be written either way. The loop does not add a frame per step.

**Examples:**

```js
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

**Sources:**

- [javascript.info — Recursion and stack](https://javascript.info/recursion)
- [MDN — Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
</details>

<details>
<summary>What is the time complexity of JavaScript operations?</summary>

### What is the time complexity of JavaScript operations?

**Definition:**

The language specification does not assign a big-O cost to operations. Engines choose the algorithm. MDN states this outright for `Array.prototype.sort`: time and space cannot be guaranteed because they depend on the implementation.

**How it works:**

- **Index read and write:** a dense array reads `arr[i]` without walking the earlier items. A `for` loop from `0` to `length - 1` does one visit per index.
- **`push` and `pop`:** they change the end. They do not move the earlier elements.
- **`shift` and `unshift`:** they remove or insert at index 0 and move every later element.
- **`sort`:** do not quote a single big-O. Pass a compare function that returns a negative number, a positive number, or zero. Since ES2019 the sort is stable.
- **Object keys:** `Object.entries` walks the object’s own enumerable string keys. It does not walk the prototype chain the way `for...in` does.

**Examples:**

```js
const values = [3, 1, 2];
values.push(4);
values.sort((a, b) => a - b);
```

**Sources:**

- [MDN — Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [MDN — Array.prototype.shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
- [MDN — Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
</details>

### JavaScript Libraries and Frameworks

<details>
<summary>What is the difference between a library and a framework in JavaScript?</summary>

### What is the difference between a library and a framework in JavaScript?

**Definition:**

MDN describes a framework as a library that offers opinions about how software gets built. Those opinions make a large application more predictable. A library is code you call for one job and then leave.

**How it works:**

- **Library:** you decide when to call it. jQuery’s early role, in MDN’s polyfill glossary, was one API over browser differences.
- **Framework:** it has a place for your components, state, and events, and it updates the UI when state changes. Angular, Vue, and the React-plus-ReactDOM stack are the examples MDN walks through.
- **React’s own label:** MDN says React itself is a library for rendering UI components. Together with ReactDOM it is used as a framework.
- **Cost:** a framework is worth it when keeping the DOM in sync with state is the hard part. A small page can stay on the DOM APIs.

**When to use:**

A library for one task, such as dates or HTTP helpers. A framework when the application’s job is to keep a changing UI matched to state.

**Examples:**

```js
document.querySelector("#count").textContent = String(count);
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
</details>

<details>
<summary>Explain the Virtual DOM in React.</summary>

### Explain the Virtual DOM in React.

**Definition:**

React’s render step calls your components to decide what should be on screen. The commit step then updates the browser DOM only where that result differs from the last one. People call the in-memory result of rendering the virtual DOM.

**How it works:**

- **Trigger:** the first `root.render`, or a state update, starts a render.
- **Render:** React calls the component functions. Rendering is meant to be pure: same inputs, same output, and no changes to objects that existed before the render.
- **Commit:** on the first render, React creates DOM nodes and `appendChild`s them. On a later render, it applies the minimum DOM operations it calculated. If a node is unchanged, React does not touch it.
- **Paint:** the browser paints after the commit. React’s docs call that painting, not rendering.

**Examples:**

```js
import { createRoot } from "react-dom/client";

function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}

createRoot(document.getElementById("root")).render(<Greeting name="Ada" />);
```

**Sources:**

- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
</details>

<details>
<summary>How does data binding work in Angular?</summary>

### How does data binding work in Angular?

**Definition:**

Angular is a component-based framework. You write declarative HTML templates, and the framework’s compiler turns those templates into JavaScript. The template shows component data, and events in the template update that data.

**How it works:**

- **Components:** the UI is a tree of components. Each one has a template.
- **Templates:** MDN says Angular extends the idea of HTML templates and compiles them to optimized JavaScript. You do not hand-write the `createElement` calls for that view.
- **TypeScript:** Angular apps are written in TypeScript, which adds types on top of JavaScript.
- **Two directions:** the template reads component state to render, and DOM events call component methods that change state. The next render shows the new state.

**Examples:**

```js
@Component({
  selector: "app-hello",
  template: `<button (click)="name = 'Ada'">{{ name }}</button>`,
})
export class Hello {
  name = "there";
}
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [Angular — Binding](https://angular.dev/guide/templates/binding)
</details>

<details>
<summary>What is Vue.js and what sets it apart from other frameworks?</summary>

### What is Vue.js and what sets it apart from other frameworks?

**Definition:**

Vue is a JavaScript framework for user interfaces. MDN places it with the widely used frameworks: it extends HTML with its own template syntax and otherwise uses standard JavaScript.

**How it works:**

- **Origin:** Evan You released Vue in 2014 after working on AngularJS.
- **Templates:** like Angular, Vue adds its own marks to HTML. The rest of the component is ordinary JavaScript.
- **React contrast:** MDN notes that React extends JavaScript with JSX, while Vue keeps an HTML template.
- **Scope:** you can mount Vue on one element of an existing page. A framework is still an opinionated library: it owns how that part of the UI stays in sync with state.

**Examples:**

```js
import { createApp } from "vue";

createApp({
  data() {
    return { name: "Ada" };
  },
  template: `<p>Hello, {{ name }}</p>`,
}).mount("#app");
```

**Sources:**

- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [Vue — Introduction](https://vuejs.org/guide/introduction.html)
</details>

<details>
<summary>Can you describe the jQuery library and its applications?</summary>

### Can you describe the jQuery library and its applications?

**Definition:**

jQuery is a JavaScript library you call. MDN’s polyfill glossary describes the first version as a set of browser-specific workarounds that gave developers one API. It does not own your application’s structure the way a framework does.

**How it works:**

- **One API:** early browsers implemented the DOM differently. jQuery hid those differences behind calls you make.
- **You call it:** a library waits until your code invokes it. MDN’s framework introduction separates that from a framework, which is a library plus opinions about how the app is built.
- **Today:** `querySelector`, `addEventListener`, and `fetch` cover the jobs jQuery was often used for. A polyfill is slower than the native feature, which is why MDN says native implementations are preferred when they exist.
- **Still a library:** an existing page can keep jQuery for plugins that expect it, without adopting a UI framework.

**Examples:**

```js
const title = document.querySelector("#title");
title.textContent = "Notes";
title.addEventListener("click", () => {
  title.hidden = true;
});
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
</details>

### JavaScript ES2017+ and Beyond

<details>
<summary>What are async iterators and generators?</summary>

### What are async iterators and generators?

**Definition:**

An async iterator produces values over time. Each `next()` returns a promise. `for await...of` waits for that promise on every step. An async generator is an `async function*` that uses `yield` to produce those values.

**How it works:**

- **`for await`:** the loop body runs after each promised result settles. A rejection throws inside the loop.
- **Async generator:** `async function*` may `await` between yields. Calling it returns an async iterator.
- **Sync generator:** `function*` and `yield` are the same idea without promises. `for...of` consumes that one.
- **Streams:** a readable stream is a common async iterable. `for await` reads chunks as they arrive.

**Examples:**

```js
async function* count() {
  yield 1;
  yield 2;
}

for await (const n of count()) {
  console.log(n);
}
```

**Sources:**

- [MDN — for await...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of)
- [javascript.info — Async iterators and generators](https://javascript.info/async-iterators-generators)
</details>

<details>
<summary>What is the purpose of the async keyword?</summary>

### What is the purpose of the async keyword?

**Definition:**

`async` marks a function that returns a promise. Inside it, `await` pauses that function until the awaited promise settles, then resumes it as a microtask.

**How it works:**

- **Return value:** a returned value becomes a fulfilled promise. A thrown exception rejects that promise.
- **`await`:** only legal inside an `async` function (and at module top level). A rejection throws at the `await`, so `try` / `catch` can handle it.
- **Same queues:** the continuation is a promise reaction. It runs before the next timer task.
- **Caller:** the caller gets a promise immediately. It does not get the final value until it awaits or uses `.then`.

**Examples:**

```js
async function load() {
  const response = await fetch("/notes");
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN — await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [javascript.info — Async/await](https://javascript.info/async-await)
</details>

<details>
<summary>Can you explain the use of Object.entries() and Object.values()?</summary>

### Can you explain the use of Object.entries() and Object.values()?

**Definition:**

`Object.entries(obj)` returns an array of the object’s own enumerable string-keyed pairs, each pair as `[key, value]`. `Object.values(obj)` returns only those values. Neither walks the prototype chain.

**How it works:**

- **Own and enumerable:** a non-enumerable property is skipped. Inherited properties are skipped. That is the difference from `for...in`, which does visit the prototype chain.
- **String keys:** symbol keys are not included. Integer-like keys are returned as strings, in the same order `for...in` would use.
- **`Object.keys`:** the keys alone. Use `entries` when you need both, `values` when you need only the values.
- **Primitives:** `undefined` and `null` throw `TypeError`. A string becomes index pairs. Other primitives become an empty array.

**When to use:**

`entries` to loop keys and values or to build a `Map`. `values` when the keys do not matter.

**Examples:**

```js
const user = { name: "Ada", city: "London" };
Object.entries(user);
Object.values(user);
new Map(Object.entries(user));
```

**Sources:**

- [MDN — Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
- [MDN — Object.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)
</details>

<details>
<summary>How does JavaScript handle big integers (BigInt)?</summary>

### How does JavaScript handle big integers (BigInt)?

**Definition:**

`BigInt` is a primitive for integers that do not fit in `Number`. Write `9007199254740991n` or call `BigInt(value)` without `new`. `typeof 1n` is `"bigint"`.

**How it works:**

- **No mix in arithmetic:** `+`, `-`, `*`, `/`, `%`, and `**` need both operands to be BigInt or neither. `1n + 1` throws. Division truncates toward zero.
- **Comparisons:** `>`, `<`, `>=`, `<=`, and `==` may mix BigInt and Number. `1n === 1` is false. `1n == 1` is true.
- **Missing operators:** unary `+` and unsigned `>>>` do not accept BigInt.
- **JSON:** `JSON.stringify` throws on a BigInt unless a replacer or a `toJSON` method turns it into something JSON can hold.
- **Only `0n` is falsy.** `Math` methods do not take BigInt.

**When to use:**

An integer that may exceed `Number.MAX_SAFE_INTEGER` (`2 ** 53 - 1`). Stay on `Number` for ordinary counts and for fractional values.

**Examples:**

```js
const max = BigInt(Number.MAX_SAFE_INTEGER);
const next = max + 1n;
next === max + 1n;
```

**Sources:**

- [MDN — BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
</details>

<details>
<summary>What are dynamic imports in JavaScript?</summary>

### What are dynamic imports in JavaScript?

**Definition:**

A dynamic import is the `import()` call. It loads a module when that line runs and returns a promise of the module’s exports. A static `import` is hoisted and runs before the module body.

**How it works:**

- **Call:** `await import("./editor.js")` resolves to the module namespace. Named exports are properties. The default export is `.default`.
- **When:** the fetch starts at the call, not at parse time. That keeps the file out of the first bundle when a bundler splits on `import()`.
- **Bare names:** in a browser, `import("lodash")` throws `TypeError` unless an import map resolves the name. Node can resolve bare specifiers from the file system.
- **Failure:** a network or parse error rejects the promise. Handle it with `try` / `catch`.

**When to use:**

Static `import` for code the page always needs. `import()` for a route, a locale, or a heavy editor.

**Examples:**

```js
async function openEditor() {
  const editor = await import("./editor.js");
  editor.mount(document.querySelector("#editor"));
}
```

**Sources:**

- [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)
- [javascript.info — Dynamic imports](https://javascript.info/modules-dynamic-imports)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

### Browser Compatibility and Transpilation

<details>
<summary>How do you ensure your JavaScript code is cross-browser compatible?</summary>

### How do you ensure your JavaScript code is cross-browser compatible?

**Definition:**

Write to the standard APIs, check that the browsers you support implement them, and add a polyfill or a compile step only for a gap you measured. MDN’s compatibility tables are the reference for what shipped.

**How it works:**

- **Feature detection:** test that the function exists (`typeof fetch === "function"`) before calling it. Do not branch on a browser name.
- **Polyfill:** MDN defines a polyfill as code that supplies a feature an older browser lacks. Native code is faster, so ship the polyfill only where the feature is missing.
- **Syntax:** a transpiler such as Babel rewrites newer syntax for older engines. It does not by itself add missing runtime methods.
- **Try the engines:** run the page in more than one browser. A console exception is the signal, not a guess about market share.

**Examples:**

```js
async function load(url) {
  if (typeof fetch !== "function") {
    throw new Error("fetch is not available");
  }
  const response = await fetch(url);
  if (!response.ok) throw new Error(response.statusText);
  return response.json();
}
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Babel — preset-env](https://babeljs.io/docs/babel-preset-env)
</details>

<details>
<summary>What is Babel and how is it used in JavaScript development?</summary>

### What is Babel and how is it used in JavaScript development?

**Definition:**

Babel is a compiler that rewrites newer JavaScript syntax into code older engines can parse. `@babel/preset-env` chooses the transforms from the browsers you list.

**How it works:**

- **Syntax, not missing methods:** Babel can turn arrow functions, classes, and optional chaining into older syntax. A missing `Array.prototype.toSorted` still needs a polyfill or a guard.
- **preset-env:** you name the targets. The preset skips transforms those targets already understand.
- **JSX:** React projects also use Babel (or another compiler) to turn JSX into `createElement` or the automatic runtime calls.
- **Source maps:** keep them so DevTools shows the original lines, not only the generated file.

**When to use:**

When the syntax you write is newer than an engine you still support. Skip it when every target already parses that syntax.

**Examples:**

```js
const greet = (name) => `Hello, ${name}`;
```

**Sources:**

- [Babel — preset-env](https://babeljs.io/docs/babel-preset-env)
- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
</details>

<details>
<summary>What are polyfills, and when would you use them?</summary>

### What are polyfills, and when would you use them?

**Definition:**

A polyfill is code, usually JavaScript, that provides a feature an older browser does not implement. MDN’s example is supplying behavior the native engine lacks, such as an older stand-in for a CSS or language feature.

**How it works:**

- **Detect, then fill:** if `Array.prototype.toSorted` is missing, install a function that returns a sorted copy. If it exists, do nothing.
- **Not as good as native:** MDN says native implementations can do more and are faster. The `Object.create` polyfill only covers what a non-native version can do.
- **Different from Babel:** Babel rewrites syntax. A polyfill adds a runtime method. You may need both.
- **jQuery’s history:** MDN calls the first jQuery an early polyfill: one API over browsers that disagreed.

**When to use:**

An older browser you still support is missing one method. Do not ship the polyfill to browsers that already have the native method.

**Examples:**

```js
if (!Array.prototype.toSorted) {
  Array.prototype.toSorted = function (compare) {
    return [...this].sort(compare);
  };
}
```

**Sources:**

- [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)
- [MDN — Array.prototype.toSorted()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
</details>

### JavaScript and the DOM

<details>
<summary>What is the Document Object Model (DOM)?</summary>

### What is the Document Object Model (DOM)?

**Definition:**

The DOM is the browser’s object tree for a loaded page. JavaScript reads and changes the page by calling methods on those objects. The language specification does not define the DOM; the browser does.

**How it works:**

- **Tree:** elements, text, and comments are nodes. `document` is the entry point. `document.documentElement` is the root element.
- **Live page:** changing a node changes what the browser shows. You do not regenerate the HTML file.
- **Events:** nodes implement event targets. Listeners see capture, target, and bubble phases.
- **Not the source:** `innerHTML` is a serialization or a parse of markup. The DOM itself is the objects.

**Examples:**

```js
const root = document.documentElement;
const title = document.querySelector("h1");
title.textContent = "Notes";
```

**Sources:**

- [MDN — Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [MDN — Document.documentElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement)
</details>

<details>
<summary>How do you create, append, or remove an element from the DOM?</summary>

### How do you create, append, or remove an element from the DOM?

**Definition:**

`document.createElement(tag)` creates an element that is not in the document yet. `appendChild` inserts a node as the last child. `element.remove()` takes that element out of its parent.

**How it works:**

- **Create:** the new element has no parent until you insert it. Set `textContent` or attributes before or after insertion.
- **`appendChild`:** moves the node if it is already in the tree. It returns the appended node.
- **`remove`:** the element leaves the tree. Listeners on it go with it. Other references you kept still point at the object.
- **Text:** prefer `textContent` for plain text. `innerHTML` parses markup.

**Examples:**

```js
const item = document.createElement("li");
item.textContent = "Ada";
list.appendChild(item);
item.remove();
```

**Sources:**

- [MDN — Document.createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement)
- [MDN — Node.appendChild()](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)
- [MDN — Element.remove()](https://developer.mozilla.org/en-US/docs/Web/API/Element/remove)
</details>

<details>
<summary>Describe different ways to find or access HTML elements in the DOM.</summary>

### Describe different ways to find or access HTML elements in the DOM.

**Definition:**

Find elements with `querySelector`, `querySelectorAll`, `getElementById`, or the live collections from tag and class name. Start from `document` or from an element you already hold.

**How it works:**

- **`querySelector`:** first match of a CSS selector, or `null`. An invalid selector throws `SyntaxError`.
- **`querySelectorAll`:** a static `NodeList` of every match.
- **`getElementById`:** one element or `null`.
- **`getElementsByTagName` / `getElementsByClassName`:** a live `HTMLCollection` that updates when the DOM changes.
- **From a node:** `element.querySelector` searches inside that element. `parentElement` and `children` walk from a node you have.

**Examples:**

```js
const form = document.querySelector("form.login");
const email = form.querySelector("input[name='email']");
const title = document.getElementById("title");
```

**Sources:**

- [MDN — Document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [MDN — Document.getElementById()](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [javascript.info — Searching: getElement*, querySelector*](https://javascript.info/searching-elements-dom)
</details>

<details>
<summary>Explain the difference between innerHTML and textContent.</summary>

### Explain the difference between innerHTML and textContent.

**Definition:**

`textContent` reads and writes plain text. `innerHTML` reads and writes HTML markup. Assigning a string to `innerHTML` parses it and is a common XSS sink.

**How it works:**

- **Read `textContent`:** the concatenation of descendant text, including `script` and `style`. Comments are excluded. A document node returns `null`; use `document.documentElement.textContent` for the whole page.
- **Write `textContent`:** removes the children and inserts one text node. Tags in the string stay as text.
- **`innerHTML`:** the getter serializes descendants as HTML. The setter parses the string and replaces the children. It does not run a `<script>` inserted this way, but an `<img onerror>` can still run code.
- **`innerText`:** skips hidden text and can force a reflow. `textContent` does not.

**When to use:**

`textContent` for text, including anything a user typed. `innerHTML` only for markup you trust, or a `TrustedHTML` value when Trusted Types are enforced.

**Examples:**

```js
title.textContent = userName;

// parses HTML; unsafe for untrusted userName:
// box.innerHTML = userName;
```

**Sources:**

- [MDN — Node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)
- [MDN — Element.innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)
</details>

<details>
<summary>How do you handle DOM events in a memory-efficient way?</summary>

### How do you handle DOM events in a memory-efficient way?

**Definition:**

Register one listener on a stable parent and handle bubbled events from its children. Remove a listener when the node goes away, using the same function reference you added.

**How it works:**

- **Delegation:** `event.target` is the child. `event.currentTarget` is the parent that owns the listener. New children do not need their own listeners.
- **Filter:** `event.target.closest("li")` ignores clicks that are not on a row.
- **Remove:** `removeEventListener` needs the same function and the same capture flag. `once: true` removes itself after the first call. An `AbortSignal` removes every listener tied to it when you abort.
- **Do not leak:** a listener on `document` that closes over a detached node keeps that node alive. Remove it when the view closes.

**Examples:**

```js
const list = document.querySelector("ul");

function onClick(event) {
  const item = event.target.closest("li");
  if (!item || !list.contains(item)) return;
  item.classList.toggle("done");
}

list.addEventListener("click", onClick);
```

**Sources:**

- [javascript.info — Event delegation](https://javascript.info/event-delegation)
- [MDN — Event.target](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)
- [MDN — EventTarget.removeEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)
</details>

### Tooling and Build Systems

<details>
<summary>What is npm, and how do you use it?</summary>

### What is npm, and how do you use it?

**Definition:**

npm is the package manager that ships with Node.js. It installs packages described by `package.json` and can run the scripts in that file.

**How it works:**

- **`package.json`:** names the project, its dependencies, and scripts such as `test` and `start`.
- **`npm install`:** reads that file and writes `node_modules`. A lockfile pins the versions that were resolved.
- **`npm install name`:** adds a dependency. `npm run test` runs the `test` script.
- **Registry:** the public registry is the default source. Yarn and pnpm are other clients of package registries; npm is the one Node installs.

**Examples:**

```js
{
  "scripts": {
    "test": "node --test"
  }
}
```

**Sources:**

- [npm — About npm](https://docs.npmjs.com/about-npm)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Discuss the role of Webpack in modern JavaScript development.</summary>

### Discuss the role of Webpack in modern JavaScript development.

**Definition:**

Webpack is a bundler. It starts from an entry module, follows `import` and `require`, and emits one or more bundles the browser can load. MDN’s modules guide says bundlers still matter for minification, dead-code elimination, and splitting, even though browsers run modules natively.

**How it works:**

- **Graph:** each `import` becomes a dependency. The bundle contains that graph, not a loose set of script tags.
- **Optimize:** minify, drop unused exports, and split a dynamic `import()` into another file.
- **Assets:** loaders can include CSS and images in the same build. That is Webpack’s job, not the language’s.
- **Not required:** a small page can use `<script type="module">` with no bundler.

**When to use:**

A production app with many modules, CSS, and a split between the first screen and later routes. Skip it for a single module file.

**Examples:**

```js
import { area } from "./square.js";
area(4);
```

**Sources:**

- [webpack — Concepts](https://webpack.js.org/concepts/)
- [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
</details>

<details>
<summary>What is a source map?</summary>

### What is a source map?

**Definition:**

A source map is a file that maps generated JavaScript back to the original sources. DevTools uses it so a breakpoint and a stack trace show your files, not only the minified bundle.

**How it works:**

- **Why:** minifiers and compilers rename locals and join files. A stack line in the bundle is hard to match to a source line.
- **DevTools:** when the map is loaded, the Sources panel shows the original modules.
- **Ship carefully:** a public map reveals the original source. Serve it only to people who debug production, or keep it off the public site.
- **Babel and bundlers:** they can emit the map next to the generated file.

**Examples:**

```js
const greet = (name) => `Hello, ${name}`;
```

**Sources:**

- [MDN — Source map](https://developer.mozilla.org/en-US/docs/Glossary/Source_map)
- [MDN — Debugging JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript)
</details>

<details>
<summary>How do you use ESLint for maintaining JavaScript code quality?</summary>

### How do you use ESLint for maintaining JavaScript code quality?

**Definition:**

ESLint is a linter. It reads your JavaScript and reports patterns you configured as problems, such as an unused binding or a missing `await` risk, before the code runs.

**How it works:**

- **Config:** a flat config or an eslintrc file lists the rules and the files they apply to.
- **Run:** `npx eslint .` prints violations. The exit code fails a script when problems remain.
- **Fix:** `eslint --fix` applies the rules that know a safe edit, such as formatting a subset of issues. It does not fix logic.
- **CI:** run the same command on each push so a violation does not wait for review.

**Examples:**

```js
const count = 1;
console.log(count);
```

**Sources:**

- [ESLint — Getting started](https://eslint.org/docs/latest/use/getting-started)
- [npm — About npm](https://docs.npmjs.com/about-npm)
</details>

<details>
<summary>What is continuous integration/continuous deployment (CI/CD) in the context of JS development?</summary>

### What is continuous integration/continuous deployment (CI/CD) in the context of JS development?

**Definition:**

Continuous integration runs the project’s checks on every push: install dependencies, lint, test, and build. Continuous deployment publishes a build that passed those checks.

**How it works:**

- **Install:** `npm install` from the lockfile, so CI uses the same versions as the author.
- **Test:** `node --test` or the project’s test script. MDN’s testing guides are about the checks this step runs.
- **Lint and build:** ESLint, then the bundler or the framework build. A non-zero exit stops the pipeline.
- **Deploy:** only the artifact from a green run is published. A failed test does not reach users.

**Examples:**

```js
{
  "scripts": {
    "test": "node --test",
    "lint": "eslint ."
  }
}
```

**Sources:**

- [MDN — Testing](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Testing)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [ESLint — Getting started](https://eslint.org/docs/latest/use/getting-started)
</details>

### JavaScript and the Web Platform

<details>
<summary>What is the Window object and its significance?</summary>

### What is the Window object and its significance?

**Definition:**

`window` is the global object of a browser page. The DOM, location, history, storage, and timers are properties of that window. Each tab, and each iframe, has its own.

**How it works:**

- **Globals:** `document`, `location`, `history`, `localStorage`, `setTimeout`, and `fetch` are reached through the window. A bare `document` is the same as `window.document`.
- **One document:** `window.document` is the page loaded in that window.
- **Events:** `popstate` and many load events fire on `window`.
- **Not Node:** a Node program has `globalThis`, not a browser `window`. `document` is absent there.

**Examples:**

```js
window.addEventListener("popstate", (event) => {
  console.log(event.state);
});
```

**Sources:**

- [MDN — Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)
- [MDN — Window: popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)
</details>

<details>
<summary>Explain the Document object.</summary>

### Explain the Document object.

**Definition:**

`document` is the `Document` for the page loaded in the window. It is the entry point to the DOM tree: you query, create, and listen from it.

**How it works:**

- **Root:** `document.documentElement` is the `<html>` element. `document.body` is the body once it exists.
- **Find:** `querySelector`, `querySelectorAll`, and `getElementById` start at the document.
- **Create:** `createElement` makes a node that is not in the tree until you append it.
- **Whole text:** `document.textContent` is `null`. Use `document.documentElement.textContent` for the page text.

**Examples:**

```js
const title = document.querySelector("h1");
const item = document.createElement("li");
item.textContent = "Ada";
document.querySelector("ul").appendChild(item);
```

**Sources:**

- [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [MDN — Document.documentElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement)
- [MDN — Document.createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement)
</details>

<details>
<summary>What new features does HTML5 bring to JavaScript development?</summary>

### What new features does HTML5 bring to JavaScript development?

**Definition:**

HTML5 is the generation of HTML that added elements and browser APIs scripts use directly: media, canvas, storage, and history, among others. The page’s JavaScript calls those APIs on `document` and `window`.

**How it works:**

- **Elements:** `<video>`, `<audio>`, and `<canvas>` are objects you can control from script after you select them.
- **Storage:** `localStorage` and `sessionStorage` keep strings on the origin without sending them as cookies.
- **History:** `history.pushState` updates the session history without loading a new document.
- **Semantics:** elements such as `<article>` and `<section>` are still DOM elements. Script finds them with the same selectors as any other tag.

**Examples:**

```js
const canvas = document.querySelector("canvas");
const context = canvas.getContext("2d");
context.fillRect(0, 0, 10, 10);

localStorage.setItem("theme", "dark");
```

**Sources:**

- [MDN — HTML5](https://developer.mozilla.org/en-US/docs/Glossary/HTML5)
- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN — History.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)
</details>

<details>
<summary>Discuss the role of JavaScript in Progressive Web Apps (PWAs).</summary>

### Discuss the role of JavaScript in Progressive Web Apps (PWAs).

**Definition:**

A PWA is a website that can be installed and can work more like a platform app. Its JavaScript is still page script on a `window`, plus an optional service worker for offline and background work.

**How it works:**

- **Same platform:** MDN says a PWA is built with web technologies. The page has HTML, CSS, and JavaScript, and the script sees the usual Web APIs.
- **Manifest:** a web app manifest tells the browser the name and icon it needs to install the app.
- **Service worker:** optional, and commonly used for a minimal offline experience. The browser can start it in the background, for example to handle a push message.
- **Not required to be a SPA:** a PWA can be multiple pages. A single-page app is not automatically a PWA.

**Examples:**

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/sw.js");
}
```

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

### JavaScript and Mobile Development

<details>
<summary>Explain how to use JavaScript for mobile development.</summary>

### Explain how to use JavaScript for mobile development.

**Definition:**

On a phone, JavaScript still runs as a web page, as an installable PWA, or inside React Native, which renders native views instead of the DOM. The language is the same. The host APIs are not.

**How it works:**

- **Responsive site:** the page is HTML, CSS, and JavaScript in the phone’s browser. Touch events and a viewport meta tag are part of that page.
- **PWA:** the same site can be installed, run standalone, and use a service worker for offline work.
- **React Native:** components describe UI, and the native side creates platform views. There is no `document`.
- **Choose by host:** browser APIs for a site. React Native’s components when the product is a store app with native screens.

**When to use:**

A website or PWA when a URL is enough. React Native when the UI is native views on iOS and Android.

**Examples:**

```js
const button = document.querySelector("button");
button.addEventListener("click", () => {
  button.textContent = "Saved";
});
```

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
</details>

<details>
<summary>What is React Native and how does it differ from traditional web apps?</summary>

### What is React Native and how does it differ from traditional web apps?

**Definition:**

React Native builds mobile apps with React components. A traditional web app renders HTML into the browser DOM. React Native renders native views on iOS and Android. There is no DOM in that host.

**How it works:**

- **Shared idea:** you write components and update state. React’s render-and-commit model still describes “decide the UI, then apply it.”
- **Web:** `react-dom`’s `createRoot` puts elements into a DOM node. The result is HTML elements.
- **Native:** the same component style maps to platform views. Layout is not CSS in a browser window.
- **MDN’s note:** React plus React Native is how that stack targets mobile; React plus ReactDOM targets the web.

**Examples:**

```js
import { createRoot } from "react-dom/client";

createRoot(document.getElementById("root")).render(<h1>Web</h1>);
```

**Sources:**

- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — Introduction to client-side frameworks](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
</details>

<details>
<summary>How does JavaScript interact with native mobile components?</summary>

### How does JavaScript interact with native mobile components?

**Definition:**

In React Native, JavaScript describes the screen as components such as `View` and `Text`. The native side creates the matching platform views. The script does not call `document.createElement`.

**How it works:**

- **Components:** a function component returns other components. State updates schedule another render, the same React idea as on the web.
- **Host:** iOS and Android provide the real controls. React Native connects the rendered description to those controls.
- **Not the DOM:** there is no `innerHTML` and no CSS layout engine of the browser. A web app’s `document` path does not apply.
- **A PWA stays on the web:** if the product is a website, JavaScript talks to the DOM and Web APIs, including device features the browser exposes, and does not create native views.

**Examples:**

```js
import { Text, View } from "react-native";

export function Hello() {
  return (
    <View>
      <Text>Hello</Text>
    </View>
  );
}
```

**Sources:**

- [React Native — Getting started](https://reactnative.dev/docs/getting-started)
- [React — Render and Commit](https://react.dev/learn/render-and-commit)
- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

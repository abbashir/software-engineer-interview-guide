# JavaScript Interview Notebook (Mid to Senior Level)

> **100 Essential Raw/Vanilla JavaScript Questions & Answers**

---

## 1. Core Execution, Event Loop & Scope

### Q1. Can you explain the JavaScript Event Loop?
**Answer:** The Event Loop is a mechanism that allows JavaScript to perform non-blocking operations despite being single-threaded. It constantly monitors the Call Stack and the Task/Callback Queue. If the Call Stack is empty, it pushes the first event from the Queue onto the Call Stack for execution.

### Q2. What is the difference between the Microtask Queue and the Macrotask Queue?
**Answer:** The Microtask queue (Promises, `MutationObserver`) has a higher priority than the Macrotask queue (`setTimeout`, `setInterval`, DOM events). After every function execution, the event loop empties the *entire* Microtask queue before processing the next single Macrotask.

### Q3. What is an Execution Context?
**Answer:** It is the environment in which JavaScript code is evaluated and executed. It contains the Variable Object (variables, functions, arguments), the Scope Chain, and the value of the `this` keyword. There is a Global Execution Context and Function Execution Contexts.

### Q4. What is Hoisting?
**Answer:** Hoisting is JavaScript's default behavior of moving variable and function *declarations* to the top of their containing scope during the compilation phase, before execution. Note: Only declarations are hoisted, not initializations.

### Q5. What is the Temporal Dead Zone (TDZ)?
**Answer:** The TDZ is the period between entering a block scope and the actual declaration of a `let` or `const` variable. Attempting to access the variable during this zone throws a `ReferenceError`.

### Q6. What is a Closure?
**Answer:** A closure is a function that remembers the variables and arguments of its outer (enclosing) lexical scope, even after the outer function has finished executing. 

### Q7. What are some practical use cases for Closures?
**Answer:** Data privacy/encapsulation (private variables), currying, memoization, event handlers, and the module pattern.

### Q8. What is Lexical Scope?
**Answer:** Lexical scope means that a variable's scope is defined by its position in the source code. Nested functions have access to variables declared in their outer scope based on where the functions are written, not where they are called.

### Q9. What does `"use strict"` do?
**Answer:** It enforces stricter parsing and error handling in JavaScript. It prevents accidental globals, throws errors for assigning values to non-writable properties, and disables features like the `with` statement.

### Q10. How does Garbage Collection work in JavaScript?
**Answer:** JavaScript uses an automatic garbage collector based primarily on the "Mark-and-Sweep" algorithm. It marks reachable objects starting from the roots (global object) and sweeps (deletes) objects that are no longer reachable or referenced.

### Q11. What causes Memory Leaks in JavaScript?
**Answer:** Unintended global variables, forgotten timers/callbacks (`setInterval`), closures holding onto large scopes unnecessarily, and detached DOM elements referenced in JavaScript but removed from the document.

### Q12. What is the difference between `var`, `let`, and `const`?
**Answer:** `var` is function-scoped and hoisted with an initial value of `undefined`. `let` and `const` are block-scoped, hoisted but kept in the Temporal Dead Zone. `const` requires initialization and its binding cannot be reassigned (though objects/arrays it points to can be mutated).

### Q13. What is the `globalThis` object?
**Answer:** `globalThis` provides a standard, unified way to access the global object across different JavaScript environments (like `window` in browsers or `global` in Node.js).

### Q14. What is a Memory Heap?
**Answer:** The Memory Heap is the largely unstructured region of memory where JavaScript allocates memory for objects, arrays, and functions (reference types).

### Q15. What is the Call Stack?
**Answer:** The Call Stack is a LIFO (Last In, First Out) data structure that records where we are in the program. If we step into a function, it pushes it to the stack. When we return from a function, it pops off the top of the stack.

### Q16. What is Stack Overflow in JavaScript?
**Answer:** It occurs when the Call Stack pointer exceeds the stack bound. This most commonly happens due to an infinite recursive function without a proper base case/exit condition.

### Q17. What is V8?
**Answer:** V8 is Google's open-source high-performance JavaScript and WebAssembly engine, written in C++. It is used in Chrome and Node.js. It compiles JS directly to native machine code using JIT (Just-In-Time) compilation.

### Q18. How does JIT (Just-In-Time) Compilation work in JS engines?
**Answer:** Instead of interpreting code line-by-line or compiling it entirely ahead of time, JIT compiles code into machine code immediately before it executes, optimizing "hot" (frequently executed) code paths on the fly.

### Q19. What is Shadowing in JavaScript?
**Answer:** Variable shadowing occurs when a variable declared within a certain scope (e.g., a local scope) has the same name as a variable declared in an outer scope. The inner variable "shadows" the outer one.

### Q20. What is an IIFE and why was it used?
**Answer:** An Immediately Invoked Function Expression `(function() { ... })();` executes immediately after being created. Before `let` and `const` block scoping, IIFEs were the primary way to create private scopes and avoid polluting the global namespace.

---

## 2. Types, Coercion & Data Structures

### Q21. What are the Primitive data types in JavaScript?
**Answer:** String, Number, BigInt, Boolean, Undefined, Null, and Symbol.

### Q22. Primitive vs. Reference Types?
**Answer:** Primitives are stored by value in the Stack; they are immutable. Reference types (Objects, Arrays, Functions) are stored by reference; the pointer is in the Stack, but the actual object is in the Memory Heap.

### Q23. What is Type Coercion?
**Answer:** The automatic or implicit conversion of values from one data type to another (e.g., `1 + '2'` becomes the string `'12'`).

### Q24. What is the difference between `==` and `===`?
**Answer:** `==` (loose equality) performs type coercion before comparing values. `===` (strict equality) checks both value and type without performing coercion. Always use `===`.

### Q25. What is `NaN`? What is its type?
**Answer:** `NaN` stands for "Not-a-Number", representing an illegal or undefined mathematical operation. Paradoxically, `typeof NaN` returns `"number"`. You should check for it using `Number.isNaN()`.

### Q26. What is the difference between `null` and `undefined`?
**Answer:** `undefined` means a variable has been declared but not assigned a value. `null` is an assignment value representing an intentional absence of an object value. Interestingly, `typeof null` returns `"object"` (a known historical bug).

### Q27. What is a `Symbol`?
**Answer:** A built-in primitive type whose value is guaranteed to be unique. Symbols are often used to add unique property keys to an object that won't collide with keys other code might add to the object.

### Q28. What is the difference between `Map` and an `Object`?
**Answer:** `Object` keys must be Strings or Symbols, whereas `Map` keys can be of *any* type (including functions or objects). `Map` maintains insertion order and has a built-in `size` property.

### Q29. What is a `Set`?
**Answer:** A `Set` is a collection of unique values of any type. It is commonly used to remove duplicates from an array (`[...new Set(array)]`).

### Q30. What are `WeakMap` and `WeakSet`?
**Answer:** Similar to `Map` and `Set`, but their keys/values must be objects. They hold "weak" references to these objects, meaning if there are no other references to the object, it will be garbage collected.

### Q31. What is destructuring assignment?
**Answer:** A syntax that allows unpacking values from arrays, or properties from objects, into distinct variables (e.g., `const { name, age } = user;`).

### Q32. What is the Spread operator (`...`)?
**Answer:** It expands an iterable (like an array or string) into individual elements, or expands an object into key-value pairs. Used for copying or merging arrays/objects.

### Q33. What is the Rest operator (`...`)?
**Answer:** It uses the same syntax as spread but does the exact opposite: it collects multiple elements and condenses them into a single array. Used in function parameters (`function sum(...args)`).

### Q34. How do you create a Shallow Copy vs Deep Copy of an object?
**Answer:** A shallow copy (`{...obj}` or `Object.assign({}, obj)`) only copies the first level; nested objects are still references. A deep copy (formerly `JSON.parse(JSON.stringify(obj))`, now `structuredClone(obj)`) creates entirely new instances of nested objects.

### Q35. What is Optional Chaining (`?.`)?
**Answer:** It permits reading the value of a property located deep within a chain of connected objects without having to check if each reference in the chain is valid (e.g., `user?.address?.zip`).

### Q36. What is the Nullish Coalescing Operator (`??`)?
**Answer:** It returns its right-hand operand when its left-hand operand is `null` or `undefined`. Unlike `||`, it does not return the right side for other falsy values like `0` or `""`.

### Q37. Why does `0.1 + 0.2 === 0.3` return `false`?
**Answer:** JavaScript numbers are represented internally as 64-bit floating-point numbers (IEEE 754 standard). This format cannot accurately represent some decimal fractions, leading to small rounding errors (`0.30000000000000004`).

### Q38. What is `BigInt`?
**Answer:** A built-in object that provides a way to represent whole numbers larger than `Number.MAX_SAFE_INTEGER` (2^53 - 1). Created by appending `n` to the end of an integer.

### Q39. What is `Array.from()`?
**Answer:** A static method that creates a new, shallow-copied Array instance from an iterable or array-like object (e.g., converting a `NodeList` or `arguments` object into a real array).

### Q40. What is the `arguments` object?
**Answer:** An array-like object accessible inside functions that contains the values of the arguments passed to that function. It does not have array methods like `map` or `filter`. (Arrow functions do not have an `arguments` object).

---

## 3. Functions, 'this' & Object-Oriented JS

### Q41. What are First-Class Functions?
**Answer:** In JS, functions are first-class citizens, meaning they can be assigned to variables, passed as arguments to other functions, and returned from other functions.

### Q42. What is a Higher-Order Function (HOF)?
**Answer:** A function that takes one or more functions as arguments, or returns a function as its result. Examples include `map()`, `filter()`, and `reduce()`.

### Q43. Explain how the `this` keyword works.
**Answer:** The value of `this` is determined by *how* a function is called, not where it is declared. In a method, `this` refers to the owner object. In a regular function, it refers to the global object (or `undefined` in strict mode).

### Q44. How do Arrow Functions handle `this`?
**Answer:** Arrow functions do not have their own `this` binding. They lexically inherit `this` from the parent scope at the time they are defined.

### Q45. Explain `call()`, `apply()`, and `bind()`.
**Answer:** All three manually set the `this` context for a function. `call()` takes comma-separated arguments. `apply()` takes an array of arguments. `bind()` does not execute the function immediately; it returns a new function with the bound `this` value.

### Q46. What is Currying?
**Answer:** A functional programming technique where a function with multiple arguments is transformed into a sequence of nested functions, each taking a single argument (e.g., `sum(1)(2)(3)`).

### Q47. What is Partial Application?
**Answer:** The process of fixing a number of arguments to a function, producing another function of smaller arity (fewer arguments). Often achieved using `bind()`.

### Q48. What is the Prototype Chain?
**Answer:** The mechanism by which objects inherit properties and methods from other objects. If a property isn't found on an object, JS looks up the chain to the object's `__proto__` until it hits `null`.

### Q49. What is the difference between `__proto__` and `prototype`?
**Answer:** `prototype` is a property on constructor functions used to build the prototype chain for objects created with `new`. `__proto__` is the actual reference on an object pointing to its constructor's prototype.

### Q50. What happens when you use the `new` keyword?
**Answer:** 1. Creates a new empty object. 2. Points the new object's `__proto__` to the constructor function's `prototype`. 3. Executes the constructor function with `this` bound to the new object. 4. Returns the new object.

### Q51. Explain ES6 Classes.
**Answer:** ES6 Classes are primarily syntactic sugar over JavaScript's existing prototype-based inheritance. They provide a much cleaner, more classical-looking syntax to create objects and deal with inheritance.

### Q52. What does `super()` do?
**Answer:** In a class constructor, `super()` calls the parent class's constructor. It must be called before accessing `this` inside a derived class constructor.

### Q53. What is `Object.create()`?
**Answer:** A method that creates a new object, using an existing object as the prototype of the newly created object. It's a direct way to implement Prototypal Inheritance without constructor functions.

### Q54. What is the difference between `Object.freeze()` and `Object.seal()`?
**Answer:** `Object.freeze()` makes an object completely immutable (cannot add, delete, or change properties). `Object.seal()` prevents adding or deleting properties, but allows modifying existing writable properties.

### Q55. What are Getters and Setters?
**Answer:** Methods inside an object or class (`get` and `set` keywords) that bind an object property to a function, allowing you to intercept and run logic when a property is read from or written to.

### Q56. What is a Generator Function (`function*`)?
**Answer:** A function that can pause its execution and yield multiple values sequentially using the `yield` keyword, returning an Iterator object.

### Q57. What is the difference between `map()` and `forEach()`?
**Answer:** `map()` returns a completely new array transformed by the callback. `forEach()` executes the callback for side effects and returns `undefined`.

### Q58. Explain how `reduce()` works.
**Answer:** `reduce()` executes a reducer callback on each element, passing in the return value from the calculation on the preceding element (the accumulator), resulting in a single output value.

### Q59. What is method chaining?
**Answer:** Calling multiple methods sequentially on the same object, made possible because each method returns an object (often `this` or a new array). Example: `arr.filter().map().reduce()`.

### Q60. What is OLOO (Objects Linking to Other Objects)?
**Answer:** A design pattern championed by Kyle Simpson that avoids classical "classes" and "constructors" entirely, instead using `Object.create()` to directly link objects together via the prototype chain.

---

## 4. Asynchronous JavaScript & Promises

### Q61. What is a Callback function?
**Answer:** A function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

### Q62. What is Callback Hell (Inversion of Control)?
**Answer:** A situation where multiple nested callbacks make code hard to read and maintain. It causes an "inversion of control" where you hand over control of your program's execution to a third-party API.

### Q63. What is a Promise?
**Answer:** An object representing the eventual completion (or failure) of an asynchronous operation and its resulting value. It solves the inversion of control problem of callbacks.

### Q64. What are the three states of a Promise?
**Answer:** Pending (initial state), Fulfilled (operation completed successfully), and Rejected (operation failed).

### Q65. How do you chain Promises?
**Answer:** By using `.then()` methods. Because `.then()` always returns a *new* promise, you can chain them. Errors anywhere in the chain are caught by a trailing `.catch()`.

### Q66. What is the difference between `Promise.all()` and `Promise.allSettled()`?
**Answer:** `Promise.all()` waits for all promises to fulfill, but rejects immediately if *any* promise rejects (fail-fast). `Promise.allSettled()` waits for all promises to complete regardless of whether they fulfilled or rejected.

### Q67. What is the difference between `Promise.race()` and `Promise.any()`?
**Answer:** `Promise.race()` returns the first promise to settle (fulfill OR reject). `Promise.any()` returns the first promise to *fulfill* (ignoring rejections unless all promises reject).

### Q68. Explain `async` and `await`.
**Answer:** Syntactic sugar over Promises introduced in ES8. `async` marks a function to always return a Promise. `await` pauses the function's execution until the awaited Promise settles, making async code look synchronous.

### Q69. How do you handle errors in `async/await`?
**Answer:** By wrapping the `await` calls inside a `try...catch` block.

### Q70. Can you use `await` outside of an `async` function?
**Answer:** Historically no, but modern JavaScript (ES2022) supports Top-Level Await inside ES Modules, allowing `await` at the root of a module without an async wrapper.

### Q71. What does `setTimeout(fn, 0)` actually do?
**Answer:** It doesn't execute the function immediately in 0 milliseconds. It pushes the callback to the Macrotask queue, delaying execution until the Call Stack and Microtask queue are completely empty.

### Q72. What is the fetch API?
**Answer:** A modern, Promise-based browser API for making HTTP requests, replacing the older `XMLHttpRequest` (XHR). Note that `fetch` only rejects on network failure, not on HTTP errors (like 404 or 500).

### Q73. How do you cancel a `fetch` request?
**Answer:** By using an `AbortController`. You pass the controller's `signal` to the fetch options, and call `controller.abort()` to cancel the request.

### Q74. What is a polyfill?
**Answer:** A piece of code (usually JavaScript) used to provide modern functionality on older browsers that do not natively support it.

### Q75. What is transpilation (e.g., Babel)?
**Answer:** The process of converting modern JavaScript (ES6+) into an older syntax (ES5) that backwards-compatible engines can parse.

### Q76. What are Web Workers?
**Answer:** Web Workers allow you to run heavy JavaScript computations in background threads. This prevents intensive tasks from blocking the main thread and freezing the UI.

### Q77. What are Service Workers?
**Answer:** A type of Web Worker acting as a network proxy. They intercept network requests, manage caching, and enable offline functionality and Push Notifications for Progressive Web Apps (PWAs).

### Q78. What is CORS?
**Answer:** Cross-Origin Resource Sharing. A browser security feature that restricts web pages from making requests to a different domain than the one that served the web page, unless the server explicitly allows it via HTTP headers.

### Q79. What is JSONP?
**Answer:** A legacy technique for bypassing CORS by injecting a `<script>` tag into the DOM, which executes a callback function containing the requested JSON data. Obsolete today.

### Q80. Explain WebSockets.
**Answer:** A protocol that provides full-duplex, bi-directional communication channels over a single TCP connection, ideal for real-time applications like chat apps or live tickers.

---

## 5. DOM, Browser APIs & Performance

### Q81. What is the DOM?
**Answer:** The Document Object Model is a programming interface for HTML. It represents the page so that programs can change the document structure, style, and content dynamically.

### Q82. What is Event Bubbling?
**Answer:** The process where an event triggered on a nested element propagates ("bubbles") up through its ancestor elements in the DOM tree until it reaches the `document` or `window`.

### Q83. What is Event Capturing (Trickling)?
**Answer:** The reverse of bubbling. The event starts from the top (`window`) and trickles down the DOM tree to the target element.

### Q84. What is Event Delegation?
**Answer:** A performance pattern where you attach a single event listener to a parent element to manage events for all of its children (taking advantage of Event Bubbling), instead of attaching individual listeners to each child.

### Q85. `e.preventDefault()` vs `e.stopPropagation()`?
**Answer:** `preventDefault()` stops the browser's default behavior for that event (e.g., stopping a form submission). `stopPropagation()` stops the event from bubbling up the DOM tree.

### Q86. Compare LocalStorage, SessionStorage, and Cookies.
**Answer:** LocalStorage stores data with no expiration. SessionStorage data is cleared when the page session (tab) ends. Cookies store small data sent back to the server with every HTTP request and can have expirations.

### Q87. DOMContentLoaded vs load event?
**Answer:** `DOMContentLoaded` fires when the initial HTML document has been completely loaded and parsed. `load` fires only after *all* resources (images, stylesheets, iframes) have fully loaded.

### Q88. What is the difference between Reflow and Repaint?
**Answer:** Reflow (or Layout) occurs when the structure or geometry of the DOM changes (e.g., width, position), triggering a layout recalculation. Repaint occurs when visual elements change but not the layout (e.g., background-color). Reflow is much more expensive.

### Q89. What is a `DocumentFragment`?
**Answer:** A lightweight, invisible Document object. It is used as a temporary container to build DOM nodes in memory and append them to the real DOM all at once, preventing multiple costly Reflows.

### Q90. What is Debouncing?
**Answer:** A technique to ensure a function is not called again until a certain amount of time has passed since the last call. Ideal for search input fields to prevent making an API call on every keystroke.

### Q91. What is Throttling?
**Answer:** A technique that ensures a function is called at most once in a specified time period, no matter how many times the event fires. Ideal for window scrolling or resizing events.

### Q92. What is `requestAnimationFrame`?
**Answer:** A browser API that tells the browser you wish to perform an animation and requests that the browser call a specified function to update an animation before the next repaint. It syncs with the monitor's refresh rate for smooth visuals.

### Q93. What is the Intersection Observer API?
**Answer:** A browser API used to asynchronously observe changes in the intersection of a target element with an ancestor element or the viewport. Commonly used for lazy-loading images or infinite scrolling.

### Q94. What is the Mutation Observer API?
**Answer:** An API that provides the ability to watch for changes being made to the DOM tree (e.g., nodes added/removed or attributes changed).

### Q95. What is the Shadow DOM?
**Answer:** A web standard that allows encapsulation of styling and markup within web components. It keeps the component's internal DOM separate from the main document DOM, preventing CSS clashes.

### Q96. What are ES Modules (ESM)?
**Answer:** The official standard format to package JavaScript code for reuse using `import` and `export` statements. Unlike CommonJS (`require`), ESM is statically analyzable, enabling Tree Shaking.

### Q97. What is Tree Shaking?
**Answer:** A term used in modern bundlers (like Webpack or Vite) for dead-code elimination. It relies on the static structure of ES modules to remove unused exports from the final production bundle.

### Q98. What is Strict Equality vs `Object.is()`?
**Answer:** `Object.is()` works exactly like `===` with two exceptions: it treats `NaN` as equal to `NaN`, and it treats `+0` and `-0` as not equal.

### Q99. What are Proxies in JavaScript?
**Answer:** The `Proxy` object allows you to create a wrapper for another object, enabling you to intercept and redefine fundamental operations for that object, such as property lookup (`get`), assignment (`set`), or enumeration.

### Q100. What is Tail Call Optimization (TCO)?
**Answer:** An engine optimization where if the last action of a function is to call another function (or itself recursively), the engine reuses the current stack frame instead of adding a new one, preventing Stack Overflow. (Note: Only fully supported in Safari currently).
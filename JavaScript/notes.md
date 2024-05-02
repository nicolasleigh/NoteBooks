### 8 different ways to create a new object

1.  Object literal

```javascript
let obj = {
  name: 'John',
  age: 25,
};
```

2.  Object constructor

```javascript
let obj = new Object();
obj.name = 'John';
obj.age = 25;
```

3.  Object.create

```javascript
let obj = Object.create(Object.prototype, {
  name: {
    value: 'John',
    enumerable: true,
    writable: true,
    configurable: true,
  },
  age: {
    value: 25,
    enumerable: true,
    writable: true,
    configurable: true,
  },
});
```

4.  Function constructor

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
let obj = new Person('John', 25);
```

5. Function constructor with prototype

```javascript
function Person() {}
Person.prototype.name = 'John';
Person.prototype.age = 25;
let obj = new Person();
```

6. Object's assign method

```javascript
let obj = Object.assign({}, { name: 'John', age: 25 });
```

7.  ES6 class

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
let obj = new Person('John', 25);
```

8.  Singleton pattern

```javascript
let obj = new (function () {
  this.name = 'John';
  this.age = 25;
})();
```

### prototype chain

Prototype chain is a mechanism in JavaScript that allows objects to inherit properties and methods from other objects.

In JavaScript, each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.

The prototype on object instance is available through **Object.getPrototypeOf(obj)** or **obj.\_\_proto\_\_**

> Professional JavaScript for Web Developers 5th - Prototype Inheritance - P347

### The difference between Call, Apply and Bind

- **Call**: The `call()` method calls a function with a given `this` value, and arguments provided one by one.

  ```javascript
  function greet(arg1, arg2) {
    return `${arg1}, ${this.name}. ${arg2}`;
  }
  const person = { name: 'John' };
  greet.call(person, 'Hi', 'Bye'); // Hi, John. Bye
  ```

- **Apply**: The `apply()` method calls a function with a given `this` value, and arguments provided as an array.

  ```javascript
  function greet(arg1, arg2) {
    return `${arg1}, ${this.name}. ${arg2}`;
  }
  const person = { name: 'John' };
  greet.apply(person, ['Hi', 'Bye']); // Hi, John. Bye
  ```

- **Bind**: The `bind()` method creates a new function that, when called, has its `this` keyword set to the provided value.

  ```javascript
  function greet(arg1, arg2) {
    return `${arg1}, ${this.name}. ${arg2}`;
  }
  const person = { name: 'John' };
  const greetPerson = greet.bind(person);
  greetPerson('Hi', 'Bye'); // Hi, John. Bye
  ```

`Call` and `Apply` are pretty much interchangeable. Both execute the current function immediately. You need to decide whether it’s easier to send in an array or a comma separated list of arguments. You can remember by treating `Call` is for comma (separated list) and `Apply` is for `Array`.

`Bind` creates a new function that will have `this` set to the first parameter passed to `bind()`.

### JSON

`JSON` is a text-based data format following JavaScript object syntax. It is useful when you want to transmit data across a network. It is basically just a text file with an extension of .json, and a MIME type of `application/json`

**Parsing**: Converting a string to a native object

```javascript
const obj = JSON.parse('{"name":"John", "age":30, "city":"New York"}');
console.log(obj.name); // John
```

**Stringification**: Converting a native object to a string so that it can be transmitted across the network

```javascript
const obj = { name: 'John', age: 30, city: 'New York' };
const myJSON = JSON.stringify(obj);
console.log(myJSON); // {"name":"John","age":30,"city":"New York"}
```

> Professional JavaScript for Web Developers 5th - JSON - P844
> [JavaScript Info - JSON](https://javascript.info/json)

### array.slice()

The `slice()` method returns a shallow copy of a portion of an array into a new array object selected from `begin` to `end` (`end` not included) where `begin` and `end` represent the index of items in that array. The original array will not be modified.

```javascript
const fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
const citrus = fruits.slice(1, 3);
console.log(citrus); // ['Orange', 'Lemon']
```

### array.splice()

The `splice()` method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place. This method modifies the original array and returns the deleted array.

```javascript
const fruits = ['Banana', 'Orange', 'Apple', 'Mango'];
fruits.splice(2, 0, 'Lemon', 'Kiwi');
console.log(fruits); // ['Banana', 'Orange', 'Lemon', 'Kiwi', 'Apple', 'Mango']
```

```javascript
const fruits = ['Banana', 'Orange', 'Apple', 'Mango'];
const deletedItems = fruits.splice(2);
console.log(fruits); // ['Banana', 'Orange']
console.log(deletedItems); // ['Apple', 'Mango']
```

> [JavaScript Info - Array Methods ](https://javascript.info/array-methods#splice)

### The difference between slice and splice

| Slice                                        | Splice                                            |
| -------------------------------------------- | ------------------------------------------------- |
| Doesn't modify the original array(immutable) | Modifies the original array(mutable)              |
| Returns the subset of original array         | Returns the deleted elements as array             |
| Used to pick the elements from array         | Used to remove/replace/add elements from/to array |

### Compare Object and Map

1. The Object type can only use integers, strings, or symbols as keys. A Map can use any type as the key, including functions, objects, and any primitive.

2. The keys in a Map are ordered while keys added to Object are not. Thus, when iterating over it, a Map object returns keys in the order of insertion.

3. You can get the size of a Map easily with the size property, while determining the number of items in an Object is more roundabout, a common way to do it is by using `Object.keys(obj).length`.

4. A Map is an iterable, so it can be directly iterated. The Object does not have a built-in iterator, and so objects are not directly iterable using the JavaScript `for...of` statement. You can get an iterable for an object using `Object.keys` or `Object.entries`. The `for...in` statement allows you to iterate over the `enumerable` properties of an object.

> Professional JavaScript for Web Developers 5th - Choosing Between Objects and Maps - P253

### Arrow function

- Arrow functions have no `this`.

  Arrow functions do not have `this`. If `this` is accessed, it is taken from the outside.

  ```javascript
  let group = {
    title: 'Our Group',
    students: ['John', 'Pete', 'Alice'],

    showList() {
      this.students.forEach((student) =>
        console.log(this.title + ': ' + student)
      );
    },
  };

  group.showList();
  ```

  Here in `forEach`, the arrow function is used, so `this.title` in it is exactly the same as in the outer method `showList`. That is: `group.title`.

  If we used a “regular” function, there would be an error:

  ```javascript
  let group = {
    title: 'Our Group',
    students: ['John', 'Pete', 'Alice'],

    showList() {
      this.students.forEach(function (student) {
        // Error: Cannot read property 'title' of undefined
        console.log(this.title + ': ' + student);
      });
    },
  };
  group.showList();
  ```

- Arrow functions can’t run with `new`

  Not having `this` naturally means another limitation: arrow functions can’t be used as constructors. They can’t be called with `new`.

- Arrow functions have no `arguments`

  For instance, `defer(f, ms)` gets a function and returns a wrapper around it that delays the call by `ms` milliseconds:

  ```javascript
  function defer(f, ms) {
    return function () {
      setTimeout(() => f.apply(this, arguments), ms);
    };
  }

  function sayHi(who) {
    alert('Hello, ' + who);
  }

  let sayHiDeferred = defer(sayHi, 2000);
  sayHiDeferred('John'); // Hello, John after 2 seconds
  ```

  The same without an arrow function would look like:

  ```javascript
  function defer(f, ms) {
    return function (...args) {
      let ctx = this;
      setTimeout(function () {
        return f.apply(ctx, args);
      }, ms);
    };
  }
  ```

- Arrow functions have no `super`

> Professional JavaScript for Web Developers 5th - Functions - P402 P407

> [JavaScript Info - Arrow functions revisited](https://javascript.info/arrow-functions)

### First class function

Functions in JavaScript are treated like any other variable. They can be assigned to variables, passed as arguments, and returned from other functions. First-class functions are a necessity for the `functional programming` style, in which the use of `higher-order functions` is a standard practice.

**Assigning a function to a variable**

```javascript
const foo = () => {
  console.log('foobar');
};
foo(); // Invoke it using the variable
// foobar
```

**Passing a function as an argument**

```javascript
function sayHello() {
  return 'Hello, ';
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
// Pass `sayHello` as an argument to `greeting` function
greeting(sayHello, 'JavaScript!');
// Hello, JavaScript!
```

**Returning a function**

```javascript
function sayHello() {
  return () => {
    console.log('Hello!');
  };
}
```

### First order function

A function that doesn't accept another function as an argument and doesn't return a function is called a `first-order function`.

### Higher order function

A function that accepts another function as an argument or returns a function is called a `higher-order function`.

### Unary function

A function that accepts exactly one argument is called a `unary function`.

### Currying function

`Currying` is the process of converting a function that takes multiple arguments into a sequence of functions that each take a single argument.

```javascript
function curry(f) {
  // curry(f) does the currying transform
  return function (a) {
    return function (b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

console.log(curriedSum(1)(2)); // 3
```

```javascript
function sum(a, b) {
  return a + b;
}

let curriedSum = _.curry(sum); // using _.curry from lodash library

console.log(curriedSum(1, 2)); // 3, still callable normally
console.log(curriedSum(1)(2)); // 3, called partially
```

### Pure function

A function is called `pure` if the return value is only determined by its input values, without any side effects.

```javascript
//Impure
let numberArray = [];
const impureAddNumber = (number) => numberArray.push(number);
//Pure
const pureAddNumber = (number) => (argNumberArray) =>
  argNumberArray.concat([number]);

//Display the results
console.log(impureAddNumber(6)); // returns 1
console.log(numberArray); // returns [6]
console.log(pureAddNumber(7)(numberArray)); // returns [6, 7]
console.log(numberArray); // returns [6]
```

the `push` function is impure by altering the array and returning the new `length` of the array, `concat` on the other hand takes the array and concatenates it with the other array producing a whole new array without side effects, and the return value is a new `Array` instance.

Remember that Pure functions are important as they simplify unit testing without any side effects and no need for dependency injection. They also avoid tight coupling and make it harder to break your application by not having any side effects.

### The difference between let and var

- **Scope**: `var` is function-scoped when declared inside a function, and globally scoped when declared outside a function. `let` is block-scoped.

  ```javascript
  function run() {
    if (true) {
      var foo = 'Foo';
      let bar = 'Bar';
    }
    console.log(foo); // Foo
    console.log(bar); // ReferenceError
  }
  run();
  ```

- **Hoisting**: `var` is hoisted to the top of the enclosing function. `let` is hoisted to the top of the block, but not initialized.

  ```javascript
  function userDetails(username) {
    if (username) {
      console.log(salary); // undefined due to hoisting
      console.log(age); // ReferenceError: Cannot access 'age' before initialization
      let age = 30;
      var salary = 10000;
    }
    console.log(salary); //10000 (accessible due to function scope)
    console.log(age); //error: age is not defined(due to block scope)
  }
  userDetails('John');
  ```

- **Redeclaration**: `var` can be re-declared and updated, `let` can be updated but not re-declared.

  ```javascript
  var foo = 'Foo 1';
  var foo = 'Foo 2'; // No problem, 'foo' is replaced.

  let bar = 'Bar 1';
  let bar = 'Bar 2'; // SyntaxError: Identifier 'bar' has already been declared
  ```

### Temporal Dead Zone

The Temporal Dead Zone(TDZ) is a specific period or area of a block where a variable is inaccessible until it has been initialized with a value. This behavior in JavaScript that occurs when declaring a variable with the `let` and `const` keywords, but not with `var`. In ES6, accessing a `let` or `const` variable before its declaration (within its scope) causes a `ReferenceError`.

```javascript
function someMethod() {
  console.log(counter1); // undefined
  console.log(counter2); // ReferenceError: Cannot access 'counter2' before initialization
  console.log(counter3); // ReferenceError: Cannot access 'counter3' before initialization
  var counter1 = 1;
  let counter2 = 2;
  const counter3 = 3;
}
```

> [Hoisting in JavaScript with let and const – and How it Differs from var](https://www.freecodecamp.org/news/javascript-let-and-const-hoisting/)

### IIFE (Immediately Invoked Function Expression)

An `IIFE` (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined. The primary reason to use an `IIFE` is to obtain data privacy because any variables declared within the `IIFE` cannot be accessed by the outside world.

```javascript
(function () {
  console.log('Hello!');
})();
```

### Encode or decode a URL in JavaScript

`encodeURI()` is used to encode an URL. This function requires a URL string as a parameter and return that encoded string.

`decodeURI()` is used to decode an URL. This function requires an encoded URL string as parameter and return that decoded string.

If you want to encode characters such as `/ ? : @ & = + $ #` then you need to use `encodeURIComponent()`.

```javascript
let uri = 'employee Details?name=john&occupation=manager';
let encoded_uri = encodeURI(uri);
let decoded_uri = decodeURI(encoded_uri);
let encoded_uri_component = encodeURIComponent(uri);
let decoded_uri_component = decodeURIComponent(encoded_uri_component);

console.log(encoded_uri); // employee%20Details?name=john&occupation=manager
console.log(decoded_uri); // employee Details?name=john&occupation=manager
console.log(encoded_uri_component); // employee%20Details%3Fname%3Djohn%26occupation%3Dmanager
console.log(decoded_uri_component); // employee Details?name=john&occupation=manager
```

> Professional JavaScript for Web Developers 5th - URI-Encoding Methods - P202

### Memoization

`Memoization` is an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

```javascript
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const stringifiedArgs = JSON.stringify(args);
    return cache[stringifiedArgs] || (cache[stringifiedArgs] = fn(...args));
  };
}
```

### Hoisting

`Hoisting` is a JavaScript mechanism where variables, function declarations and classes are moved to the top of their scope before code execution. Remember that JavaScript only hoists declarations, not hoists initialization.

> [Hoisting in JavaScript with let and const – and How it Differs from var](https://www.freecodecamp.org/news/javascript-let-and-const-hoisting/)

### Class

In ES6, Javascript `Class` are primarily syntactic sugar over JavaScript’s existing prototype-based inheritance. For example, the prototype based inheritance written in function expression as below:

```javascript
function Bike(model, color) {
  this.model = model;
  this.color = color;
}

Bike.prototype.getDetails = function () {
  return this.model + ' bike has ' + this.color + ' color';
};

let bike = new Bike('BMW', 'black');
console.log(bike.getDetails()); // BMW bike has black color
```

Whereas ES6 `Class` can be defined as an alternative

```javascript
class Bike {
  constructor(model, color) {
    this.model = model;
    this.color = color;
  }

  getDetails() {
    return this.model + ' bike has ' + this.color + ' color';
  }
}

let bike = new Bike('BMW', 'black');
console.log(bike.getDetails()); // BMW bike has black color
```

> [JavaScript Info - Classes](https://javascript.info/class)

### Closure

`Closures` are functions that have access to variables from another function’s scope. This is often accomplished by creating a function inside a function

```javascript
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log('Outer Variable: ' + outerVariable);
    console.log('Inner Variable: ' + innerVariable);
  };
}

const newFunction = outerFunction('outside');
newFunction('inside');
// Outer Variable: outside
// Inner Variable: inside
```

The inner function has access to the variables in the outer function scope even after the outer function has returned.

> Professional JavaScript for Web Developers 5th - Closures - P428

> [JavaScript Info - Closure](https://javascript.info/closure)

### Modules

`Modules` refer to small units of independent, reusable code and also act as the foundation of many JavaScript design patterns. Most of the JavaScript modules export an object literal, a function, or a constructor. `Modules` are particularly useful for a number of reasons:

- **Maintainability**: Modules allow you to keep related code together and organized.
- **Namespacing**: Modules allow you to avoid naming collisions.
- **Reusability**: Modules allow you to write code once and use it in multiple places.

> [JavaScript Info - Modules](https://javascript.info/modules-intro)

> Professional JavaScript for Web Developers 5th - Modules - P913

### Scope

`Scope` is the context in which a variable is declared. In JavaScript, variables can be declared either inside or outside of functions. The scope of a variable determines where the variable can be accessed.

- **Global Scope**: Variables declared outside of any function are in the global scope. They can be accessed from any function in the program.
- **Function Scope**: Variables declared inside a function are in the function scope. They can only be accessed from within that function.
- **Block Scope**: Variables declared with `let` and `const` are block-scoped. They can only be accessed within the block they are declared in.

> Professional JavaScript for Web Developers 5th - EXECUTION CONTEXT AND SCOPE - P154

### Service worker

`Service workers` are a separate type of `worker` that are designed to enable advanced web application features such as `offline functionality`, `push notifications`, and `background syncing`. They behave more like a network proxy than a separate browser thread, allowing them to intercept network requests and cache resources. This enables web applications to function even when there is no Internet connection, providing a more consistent and reliable user experience. `Service workers` can also enable `push notifications` for progressive web applications, allowing web applications to keep users engaged even when they are not actively using the application.

In addition to their caching and push notification capabilities, `service workers` can act as a highly customizable network cache. They can cache resources such as HTML, CSS, JavaScript, and images, allowing web applications to load quickly even when there is no network connection. This means that users can continue using the application even when they are offline or have a poor Internet connection, providing a smoother and more reliable user experience.

> Professional JavaScript for Web Developers 5th - SERVICE WORKERS - P973

### IndexedDB

`IndexedDB` is a structured data storage mechanism similar to an SQL database. Instead of storing data in `tables`, data is stored in `object stores`. `Object stores` are created by defining a key and then adding data. `Cursors` are used to query `object stores` for particular pieces of data, and `indexes` may be created for faster lookups on particular properties.

> Professional JavaScript for Web Developers 5th - INDEXEDDB - P899

> [JavaScript Info - IndexedDB](https://javascript.info/indexeddb)

### Web storage

`Web storage` is an API that provides a mechanism by which browsers can store key/value pairs locally within the user's browser, in a much more intuitive fashion than using `cookies`. The `web storage` provides two mechanisms for storing data on the client:

- **Local Storage**: Data stored in `local storage` persists even after the browser is closed and reopened. It is stored without an expiration date and must be manually cleared.
- **Session Storage**: Data stored in `session storage` is lost when the browser tab is closed.

### Cross-document messaging (XDM)

`Cross-document messaging` (XDM) is a method of communication between documents from different origins. It allows scripts to access objects in a different document, even if the document is in a different domain. This is useful for web applications that need to communicate with `iframes`, pop-up windows, or other documents that are not in the same domain.

At the heart of XDM is the `postMessage()` method. The `postMessage()` method accepts three arguments: a message, a string indicating the intended recipient origin, and an optional array of transferable objects (only relevant to `web workers`). It is possible to allow posting to any origin by passing in "`*`" as the second argument to `postMessage()`, but this is not recommended.

> Professional JavaScript for Web Developers 5th - CROSS-CONTEXT MESSAGING - P739

> [JavaScript Info - Cross-window communication](https://javascript.info/cross-window-communication)

### Cookie

`Cookies` are small pieces of data that are stored on the client-side by the browser. They are used to store information about the user, such as login credentials, shopping cart items, and user preferences. `Cookies` are sent to the server with each request, allowing the server to identify the user and provide a personalized experience.

> Professional JavaScript for Web Developers 5th - COOKIES - P891

> [JavaScript Info - Cookies, document.cookie](https://javascript.info/cookie)

### The differences between cookie, local storage and session storage

| Cookie                                                                  | Local Storage                                           | Session Storage                                                                                  |
| ----------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Cookies are primarily for reading server-side.                          | Local storage can only be read client-side.             | Session storage is only available per window (or tab in browsers like Chrome and Firefox).       |
| Cookies can be made secure by setting the HttpOnly flag for the cookie. | Local storage has no such feature.                      | Session storage has no such feature.                                                             |
| Cookies are sent with every HTTP request.                               | Local storage is not sent with every HTTP request.      | Session storage is not sent with every HTTP request.                                             |
| Cookies are limited to about 4 KB of data.                              | Local storage is limited to about 5 MB of data.         | Session storage is limited to about 5 MB of data.                                                |
| Cookies can be accessed on both the client and server side.             | Local storage can only be accessed on the client side.  | Session storage can only be accessed on the client side.                                         |
| Cookies can be set to expire after a certain amount of time.            | Local storage does not expire.                          | Session storage expires when the tab or browser is closed.                                       |
| Cookies can be used for tracking user behavior.                         | Local storage can be used for storing user preferences. | Session storage can be used for storing temporary data that is only needed for a single session. |

### Web worker

`Web workers` are a type of JavaScript worker that runs in the background, independently of other scripts, without affecting the performance of the page. They are used to perform tasks that are computationally expensive or time-consuming, such as processing large amounts of data, without blocking the main thread. `Web workers` run in a separate thread from the main thread, allowing them to run concurrently with other scripts.

There are three primary types of workers defined in the Web Worker specification: the `dedicated web worker`, the `shared web worker`, and the `service worker`.

The `dedicated web worker` runs in a separate thread from the main thread and is dedicated to a single script.

The `shared web worker` runs in a separate thread from the main thread and can be shared by multiple scripts.

The `service worker` is a type of web worker that runs in the background and is used to enable advanced web application features such as offline functionality, push notifications, and background syncing.

> Professional JavaScript for Web Developers 5th - WORKERS - P939

### Promise

A `promise` is an object that may produce a single value some time in the future with either a resolved value or a reason that it’s not resolved(for example, network error). It will be in one of the 3 possible states: `fulfilled`, `rejected`, or `pending`.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Success!');
  }, 1000);
});

promise.then((value) => {
  console.log(value); // Success!
});
```

> Professional JavaScript for Web Developers 5th - PROMISES - P439

> [JavaScript Info - Promise](https://javascript.info/promise-basics)

### Callback function

A `callback function` is a function that is passed as an argument to another function and is executed after some operation has been completed. `Callback functions` are often used to handle asynchronous operations, such as fetching data from a server or reading a file.

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = 'Data fetched!';
    callback(data);
  }, 1000);
}

fetchData((data) => {
  console.log(data); // Data fetched!
});
```

### Server-sent events (SSE)

`Server-sent events` (SSE) is a technology that allows a server to push data to a web page over a single, long-lived connection. With `SSE`, a server can send data to a client without the client having to request it. This is useful for applications that require real-time updates, such as news feeds, stock price updates, and chat applications.

To use `SSE`, the server sends a `Content-Type` header with the value `text/event-stream` and a `data` field with the data to be sent. The client can then listen for `message` events on the `EventSource` object to receive the data.

```javascript
const eventSource = new EventSource('/events');

eventSource.addEventListener('message', (event) => {
  console.log(event.data);
});
```

> Professional JavaScript for Web Developers 5th - THE EVENTSOURCE API - P888

> [JavaScript Info - Server-sent events](https://javascript.info/server-sent-events)

### Promise Chaining

`Promise chaining` is a technique used to chain multiple asynchronous operations together using `promises`. This allows you to perform a series of asynchronous operations in sequence, passing the result of one operation to the next operation in the chain. `Callback hell` is a common problem when working with asynchronous code, and `promise chaining` is a way to avoid this problem.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1);
  }, 1000);
});

promise
  .then((result) => {
    console.log(result); // 1
    return result * 2;
  })
  .then((result) => {
    console.log(result); // 2
    return result * 2;
  })
  .then((result) => {
    console.log(result); // 4
  });
```

> Professional JavaScript for Web Developers 5th - Promise Chaining - P455

### Strict mode

`Strict mode` can be enabled by adding the string `"use strict";` at the beginning of a script or function.

- All code defined inside classes and modules by default is in strict mode.

- Disallows accidental creation of global variables.

- To call `delete` on a variable will throw an error.

- Disallows variables named `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`, `eval`, `arguments`.

- In `non-strict mode`, changes to a named argument are also reflected in the arguments object, whereas `strict mode` ensures that each are completely separate.

- Disallows the use of `with` statement.

- Disallows function `declarations` unless they are at the top level of a script or function. That means functions declared, for instance, in an if statement are now a syntax error.

- `arguments.callee` and `arguments.caller` are not allowed.

- An `octal literal` is considered invalid syntax in strict mode.

> Professional JavaScript for Web Developers 5th - STRICT MODE - P1043

### The difference between null and undefined

| Null                                                                                                               | Undefined                                                                                      |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| `null` is an assignment value. It can be assigned to a variable which indicates that variable points to no object. | `undefined` means a variable has been declared but has not yet been assigned a value.          |
| Type of `null` is object                                                                                           | Type of `undefined` is undefined                                                               |
| The `null` value is a primitive value that represents the null, empty, or non-existent reference.                  | The `undefined` value is a primitive value used when a variable has not been assigned a value. |
| Converted to `0` while performing primitive operations                                                             | Converted to `NaN` while performing primitive operations                                       |

```javascript
let a = null;
let b = undefined;

console.log(a + 2); // 2
console.log(b + 2); // NaN
```

### The differences between undeclared and undefined variables

| undeclared                                                                                  | undefined                                                                              |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| These variables do not exist in a program and are not declared                              | These variables declared in the program but have not assigned any value                |
| If you try to read the value of an undeclared variable, then a runtime error is encountered | If you try to read the value of an undefined variable, an undefined value is returned. |

### Event flow

`Event flow` is the order in which events are received by the various elements in a web page. There are three phases of event flow:

- **Capturing phase**: The event is captured by the outermost element and propagated to the inner elements.

- **Target phase**: The event reaches the target element.

- **Bubbling phase**: The event bubbles up from the target element to the outermost element.

### The difference between document load and DOMContentLoaded events

- **DOMContentLoaded**: The `DOMContentLoaded` event is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

- **Load**: The `load` event is fired when the entire page has finished loading, including all stylesheets, images, and subframes.

```javascript
document.addEventListener('DOMContentLoaded', (event) => {
  console.log('DOM fully loaded and parsed');
});

window.addEventListener('load', (event) => {
  console.log('Page fully loaded');
});
```

> Professional JavaScript for Web Developers 5th - The load Event | The DOMContentLoaded Event - P622 | P638

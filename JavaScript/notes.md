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

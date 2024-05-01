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

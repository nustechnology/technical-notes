# Some useful features in new JS - Chapter 1

![](https://i.ibb.co/hcgsGYd/ruby.png)

### Arrow functions (=>) in ES6

Arrow functions make our code more concise. By using arrow functions, we avoid having to type the `function` keyword, `return` keyword, and curly brackets.

**No `function` keyword required**

```
// ES5
var multiplyES5 = function(x, y) {
  return x * y;
};

// ES6
const multiplyES6 = (x, y) => { return x * y };
```

**No `return` keyword required if there is no brackets**

```
const multiplyES6 = (x, y) => x * y;
```


The handling of this is also different in arrow functions compared to regular functions. In short, with arrow functions there are no binding of `this`. In regular functions the this keyword represented the object that called the function, which could be the window, the document, a button or whatever. With arrow functions the `this` keyword always represents the object that defined the arrow function. For example:

```
//Regular Function:
hello = function() {
  console.log(this);
}

//The window object calls the function:
window.addEventListener("load", hello);

=> Window is logged by function hello

//A button object calls the function:
document.addEventListener("mouseenter", hello);

=> document is logged by function hello
```
With arrow function:

```
//Arrow Function:
hello = () => {
  console.log(this);
}

//The window object calls the function:
window.addEventListener("load", hello);

=> Window is logged by function hello

//A button object calls the function:
document.addEventListener("mouseenter", hello);

=> Window is logged by function hello
```

#### When we should not use arrow function?

1. Object methods
  
  If you object method access and modify object's property, you should not use arrow function because `this` in arrow function is not bound to anything, it is inherited the value of `this` from its parent scope. For example

  ```
  var number = {
    value: 10,
    increase: () => {
      this.value++;
    },
    increase2: function() {
      this.value++;
    }
  }

  number.increase();
  console.log(number.value);
  => 10

  number.increase2();
  console.log(number.value);
  => 11
  ```

2. Binding DOM events

  If we need our context to be dynamic, arrow functions are not for us. Take a look at this event handler:

  ```
  var navItems = document.getElementByClassName('nav-item');
  navItems.addEventListener('click', () => {
    this.classList.toggle('active');
  });
  ```  

  There is an `TypeError` error if we click "nav-item" because this is not bound to the button, `this` is bound to parent scope of arrow function.

### Classes in ES6

### Let and Const in ES6

### Template strings in ES6

### Promise in ES6

### Array.prototype.includes in ES7

### Exponentiation Operator in ES7

### Object.values() in ES8

### String.prototype.padEnd() and String.prototype.padStart() in ES8

### Async function (async/await) in ES8

### Rest operator in ES9

### Promise.prototype.finally in ES9

### Optional Catch Binding in ES10

### Array.flat() in ES10

### Array.flatMap() in ES10

### String.trimStart() & String.trimEnd() in ES10

### globalThis Object in ES10

### Optional Chaining ?. in ES11

### undefined javascript - Nullish Coalescing ?? in ES11


## References

1. https://www.sitepoint.com/

2. https://developer.mozilla.org/

3. https://www.w3schools.com/

4. https://www.freecodecamp.org/

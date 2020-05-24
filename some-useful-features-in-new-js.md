# Some useful features in new JS - Chapter 1

What I write in this post is not something new. It's just what I collected from some popular websites for developers. For a developer who works with javascript a lot, he is certainly familiar with what I introduce in this post. But if you're not familiar with new JS, I think you will get lots of interesting information in this post. I won't introduce to you all features of the new JS6 to JS11, instead I just tell you about what I think they are exciting.

![](https://image.ibb.co/dWXB3T/javascript.jpg)

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


### Template strings in ES6

This feature that allows you to work with strings in a novel way compared to ES5 and below.

The syntax at a first glance is very simple, just use backticks instead of single or double quotes:

```
const a_string = `something`
```

Template literals can contain placeholders — which are indicated as `${expression}`. This syntax allows us to put not just variable but any code expression in it.

```
let welcome = "Hello " + (isWorld ? "World" : "me");
```

Now it is just simple like this:

```
let welcome = `Hello ${isWorld ? "World" : "me"}!`;
```

No more `()` to make sure the code result is correct.

And of course, we can say goodbye to `... + <variable> + " " + <variable> + ...`, we no longer need `+` or `" "` to compute a dynamic string.


### References

1. https://www.sitepoint.com/

2. https://developer.mozilla.org/

3. https://www.w3schools.com/

4. https://www.freecodecamp.org/

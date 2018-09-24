# Some common idioms which JavaScript developers should know

![](https://image.ibb.co/dWXB3T/javascript.jpg)

Javascript is moving fast these days. Lots of great features were added to ES6 (ECMAScript 6) which make the developer's life easier. More and more great JS frameworks are available for frontend web developers such as: Angular, ReactJS, VueJS, MeteorJS, etc. which change the way we program. But there is a fact: lots of JS developers, including people who have worked many years with JS, don't know common idiom and essential concept in JavaScript. Or they might know the names but they didn't really understand what it is.


Eric Elliott, a famous JS developer/writer, calls **Prototypal Inheritance** and **Functional Programming** as the two pillars of JavaScript. Have you ever heard about these paradigms? I'll give you an overview of them and some other exciting concepts in this post.


## Inheritance

There are 2 types of inheritance in JS: **Prototypal Inheritance** (which Eric calls the pillar of JavaScript) and **Class Inheritance**. What are differences between them? Let's clarify.

### Prototypal Inheritance

Instances inherit directly from other objects. JavaScript objects are dynamic *bags* of properties (referred to as own properties). JavaScript objects have a link to a prototype object. When trying to access a property of an object, the property will not only be found on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.

```
let f = function() {
   this.x = 'a';
   this.y = 'b';
}

let o = new f(); // {x: 'a', y: 'b'}

f.prototype.y = 'c';
f.prototype.z = 'd';

console.log(o.x); // "a"

console.log(o.y); // "b"

console.log(o.z); // "d"

console.log(o.w); // undefined
```

JavaScript does not have *methods* in the form that class-based languages define them. In JavaScript, any function can be added to an object in the form of a property. An inherited function acts just as any other property.

```
var o = {
  a: 1,
  f: function() {
    return this.a + 1;
  }
};

console.log(o.f()); // 2

var p = Object.create(o);

p.a = 4;
console.log(p.m()); // 5
```

Through the above 2 examples we see that JavaScript supports OOP with prototypal inheritance. There is more than one type of prototypal inheritance:

    - Delegation
    - Concatenative
    - Functional

If you are interested in learning about these types of prototypal inheritance, I have a good [article](https://medium.com/javascript-scene/3-different-kinds-of-prototypal-inheritance-es6-edition-32d777fa16c9) for you.

### Classical Inheritance

A class is a description of the object to be created. Classes inherit from classes and create subclass relationships: hierarchical class taxonomies. Instances inherit from classes. Instances are initialized via constructor functions with the `new` keyword. Class inheritance may or may not use the `class` keyword from ES6.

In JavaScript, class inheritance is implemented on top of prototypal inheritance, but that does not mean that it does the same thing: JavaScript’s class inheritance uses the prototype chain to wire the child `Constructor.prototype` to the parent `Constructor.prototype` for delegation while **Prototypal Inheritance** is *objects inherit directly from other objects*

```
function Shape(x, y) {
  this.x = x;
  this.y = y;
};

var shape = new Shape(1, 2);
console.log(shape.x) // 1;
console.log(shape.y) // 2;
```

## Functional programming

**Functional programming** is another pillar of JavaScript. According to Wikipedia, functional programming is a programming paradigm or pattern (a style of building the structure and elements of computer programs). Functional Programming treats computation as the evaluation of mathematical functions, avoids changing-state and mutable data.

Recently there has been a growing trend toward functional programming. In frameworks such as Angular and React, you'll actually get a performance boost by using immutable data structures. Immutability is a core of functional programming. It and pure functions make easier to debug your programs. Replacing procedural loops with functions can increase the readability of your program and make it more elegant. Overall, there are many advantages to functional programming.

If you want to know what functional programming means in practice, you have to start with an understanding some core concepts:

  - Pure functions
  - Function composition

Let's start figuring them

### Pure functions

A pure function is a function which:

  - Given the same inputs, always returns the same output. Its execution doesn’t depend on the state of the system.
  - The function does not produce any observable side effects such as network requests, input and output devices, or data mutation.


**Pure Function Example In JavaScript**

```
function priceWithVAT(productPrice) {
 return (productPrice * 0.1) + productPrice;
}
```

**Impure Function In JavaScript**

```
const VAT = 0.1;
function priceWithVAT(productPrice) {
 return (productPrice * VAT) + productPrice;
}
```

#### Why Are Pure Functions Important In JavaScript?

Pure functions are used heavily in Functional Programming. And, libraries such as ReactJS and Redux require the use of pure functions.

Not all functions need to be , or should be, pure. For example, an event handler for a button press that manipulates the DOM is not a good candidate for a pure function. But, the event handler can call other pure functions which will reduce the number of impure functions in your application.

Another reason to use pure functions where possible is testing and refactoring. One of the major benefits of using pure functions is they are immediately testable. They will always produce the same result if you pass in the same arguments. When used correctly the use of pure functions produces better quality code.

Pure functions have lots of properties that are important in functional programming. Here is a good [post](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976) where you can find more details of pure function


### Function composition

Function composition is a mechanism of combining multiple simple functions to build a more complicated one. The result of each function is passed to the next one. In mathematics, we often write something like: `f(g(x))`. So this is the result of `g(x)` that is passed to `f`. In programing we can achieved the composition by writing something similar. Understanding function composition is an important step towards understanding how software is constructed using the functional programming. Here is a simple example of function composition:

```
function addOne(x) {
  return x + 1;
}
function double(x) {
  return x * 2;
}

double(addOne(1)); //4
```

It's quite simple, right? But it could be better if we were able to automate the composition. At least it could be more readable.


```
function addOne(x) {
  return x + 1;
}
function double(x) {
  return x * 2;
}

var addOneAndDouble = x => double(addOne(x));

console.log(addOneAndDouble(1)); //4
console.log(addOneAndDouble(2)); //6
```

For more details of function composition, you can read this [post](https://www.sitepoint.com/function-composition-building-blocks-for-maintainable-code/)


## Conclusion

This post just gives you the overview of *the two pillars of JavaScript*. There are lots of things I cannot explain clearly in this post. That's why I also give you some references of some expert guys who can explain them better than me. I still have some interesting idioms/concepts of Javascript which I would like to share with you. But let's leave them for the next time. Some concepts I mention in this post are a huge amount of knowledge for you to learn.

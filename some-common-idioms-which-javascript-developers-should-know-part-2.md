# Some common idioms which JavaScript developers should know - Part 2

![](https://image.ibb.co/dWXB3T/javascript.jpg)

### POJO

A **POJO**, or Plain Old Javascript Object, is a set of "key / value pairs", a concept you will hear about everything around the world of programming. In Javascript, the "key" must be something that can be converted to a string while "value" can be anything (or nothing, in the case of null). You will be able to look up the value using the key.

```
var person = {
  name: 'Nhan',
  age: 23,
  languages: ['ruby', 'js', 'php'],
  sayHi: function(){
    return "Hi, my name is" + this.name;
  }
}

console.log("The name of 'person' is", person.name);
// output:
// "Nhan"
console.log(person.sayHi());
// output:
// "Hi, my name is Nhan"
```

A **POJO** is usually created using the object literal syntax `{}` but can also be created using the `new Object`.

```
var person = new Object();
person.name = 'Nhan';
person.languages = ['ruby', 'js', 'php'];

console.log("The name of 'person' is", person.name);
// output:
// "Nhan"
```

Lots of libraries use this pattern including

**jQuery**

```
$.ajax({
  url: 'http://example.com',
  type: 'GET',
  dataType: 'JSON'
});
```

**Facebook**

```
FB.init({
  appId            : 'your-app-id',
  autoLogAppEvents : true,
  xfbml            : true,
  version          : 'v3.1'
});
```

#### Why use POJOs

By using an object to pass the named argument to a function, an arbitrary required and option parameter can be added without changing the signature of the function, reducing the chance of code based Twill break because of the modification.

Take for example the process of finding developers who have their name include "Pe" and live in Vietnam:

```
var developers = findDevelopers('Pe', 'vi');
```

this works for a while but things get harder when you want to want to find developers with more conditions: who know ruby and has at least 5 years experience and is available now

```
var developers = findDevelopers('Pe', 'vi', 'ruby', 5, null);
```

and things get impossible to read when we want to leave out search options and just find developers who know ruby

```
var developers = findDevelopers(null, null, 'ruby', null, null);
```

findDevelopers could instead be written to take it's arguments using the options object and future you will know what everything means.

```
var developers = findDevelopers(
  name: 'Pe',
  country: 'vi',
  languages: ['ruby'],
  experience: 5,
  availableNow: true
);
```


```
var developers = findDevelopers(
  languages: ['ruby']
);
```


### Constructors

Constructer is one of the most common ways to create objects that you will see in nature. They are a more classical approach to OOP, using "classes" (constructors) to define a group of objects. The constructor will have the same structure as the following:

```
function Person(name, age) {
  this.name = name;
  this.age = age;

  this.sayHi = function() {
    console.log('Hello, my name is' + this.name + ". I'm " + his.age + 'years old.'');
  }
}
```

```
var me = new Person('Nhan', 23);
me.sayHi()       // "Hello, my name is Nhan. I'm 23 years old."
```

What exactly JS does here? How does `new Person()` create a new object and how does it take effect prototypal inheritance?

When `new Person` called, a new object created and value of `this` is set to object. Additionally, attribute `constructor` is changed to constructor function and `__proto__` is set to the the constructor’s prototype. If no return value at the end of the constructor’s function, function will return `this`, which is itself.

So you can rewrite:

```
var me = new Person('Nhan', 23);
```

to this:

```
var me = {
  name: 'Nhan',
  age: 23,
  sayHi: function() {
    console.log('Hello, my name is' + name + ". I'm " + age + 'years old.'');
  }
  constructor: Person,
  __proto__: Person.prototype
};
```

In prototypal inheritance, `me` is an instance of a `Person` object and Person is the prototype chain for `me`.

```
me instanceof Person       // true
me.__proto__               // Object {constructor: function...}
me.__proto__ === Person.prototype   // true
```

### OLOO

OLOO just stands for objects-linked-to-other-objects.

It’s a style pattern which embraces delegation oriented design and the nature of JS in general. No more need to use function as a constructor to create objects; `Object.create()` method will be hero here.

OLOO takes advantage of the `Object.create` method to take care of object creation and inheritance:

```
var Person = {
  init: function(name, age) {
    this.name = name;
    this.age = age;
    return this;
  },

  sayHi: function() {
    console.log('Hello, my name is' + name + ". I'm " + age + 'years old.'');
  }
}
var me = Object.create(Person).init('Nhan', 23);
me.sayHi()    // "Hello, my name is Nhan. I'm 23 years old."
```

`Object.create` is similar to a constructor: it creates a new object which inherits from the object passed in. In other words, it changes the value of `__proto__` on the new object to the passed in object. However, the advantage of `Object.create` comes from it’s emphasis on prototypes and delegation.

To better understand `Object.create`, you can rewrite it as:

```
function createObject(obj) {
  var newObj = {};

  newObj.__proto__ = obj;
  return newObj;
}
```

As you can see in the above function, the new created object does not have any of its own properties or behaviors. It inherits all of them from the transmitted object.

If a method is called on the new object, the program will find the method in the `__proto__` property instead of the object itself.

One downside of `Object.create` is that it does not allow the use of `instanceof` since it does not deal with the constructor property. Instead, to test the inheritance, you must use `.isPrototypeOf` method on the original object.

```
var me = Object.create(Person).init('Nhan', 23);
Person.isPrototypeOf(me);    // true
```

### Delegation

A delegation is a technique that encourages the reuse of code by allowing runtime function invocation in the context of a particular instance - regardless of the hierarchy of the instance and function. JavaScript has great support for Delegation in the form of calling and applying that allows us to inject an object into this value of any function. This allows unwanted code sharing, free from the constraints of unwieldy, unnatural and overly complex hierarchies

```
function Person(name, age) {
  this.name = 'Nhan';
  this.age = 23;
}
Person.prototype.sayHi = function() {
  console.log('Hello, my name is' + name + ". I'm " + age + 'years old.'');
}
```

In the code above, if a new Person object is created, it will not contain a `sayHi` method; the program will have to find it in `__proto__`.

You can even put common properties in the `prototype` as well:

```
function Person() {}
Person.prototype.name = 'Nhan';
Person.prototype.age = 23;
Person.prototype.sayHi = function() {
  console.log('Hello, my name is' + name + ". I'm " + age + 'years old.'');
};
```

You can then override those properties if need be.

```
var person = new Person();
person.name = 'Thuy';
person.age = 22;
person.sayHi()   // ""Hello, my name is Thuy. I'm 22 years old.""
```

### First Class Functions

In JavaScript, functions are first-class objects. Functions are of the type Object and they can be used like any other object (String, Array, Number, etc.). They can be "stored in variables, passed as arguments to functions, created within functions, and returned from functions". They enable 2 common patterns in Javascript.

  - A function can be passed into another function as a parameter
  - A function can be returned from another function

#### Callbacks

The first allows for *callbacks*, which is a way to allow other code to execute and call the *callback* function when it is done.

```
$.get('http://example.com', function(response){
  console.log(response);
});
```

#### Closures

In pre-ES6 Javascript, the only way to create a new scope is with a function. A new scope offers a way to protect both the global scope and a new set of variables. By using a new scope, the global scope is left uncluttered and no one can accidentally reassign the value of your variable when their code runs.

A neat behavior of functions in javascript is that they have access to the scope in which they were created, not the scope in which they are called.

A closure takes advantage of a both of these behaviors. First, a quick example.

```
function buildHello(from) {
  return function hello(to) {
     console.log('Hello' + to + ', ' + '. My name is' + from);
  }
}

var hello = buildHello('Nhan');
hello('Thuy');
// output:
// "Hello Thuy. My name is Nhan"
```

In the example above, the value of `from` that was passed as an argument to `buildHello` is available to the `hello` even though `from` may seem locked into the scope of `buildHello`.

Closures are useful because they let you associate some data (the lexical environment) with a function that operates on that data. This has a clear similarity with object-oriented programming where objects allow us to combine some data (attributes of an object) with one or more methods.

You can see more examples about when we should use closures [here.](https://medium.com/@dis_is_patrick/practical-uses-for-closures-c65640ae7304)

## Conclusion

There are lots of other idioms in Javascript but I cannot introduce all of them in this post for you. In the near future, I may continue giving you the remaining of JS idoms or introduce you some common patterns in JS. See you in next post ;)
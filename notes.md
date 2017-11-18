Learn Modern Javascript
======

**Topics**
- Advanced Functions
- Advanced Objects
- Object Oriented JavaScript
- Advanced Programming Patterns
- Modular Programming
- Function Programming Patterns in Javacsript

---

**Advanced Functions and Objects**
*(A lot of this is review)*

Functions are objects.
Functions can have properties attached, just like objects.
Some properties cannot be accessed. These are called internal properties. They are referenced with double squares, for example `[[Code]]`.

JS uses two main methods for defining a function: Function declarations (sometimes referred to as function statements) and function expressions. The main difference between these is that decs are hoisted, expressions are not. 

dec is `function sum(){}`.
exp is `var x = function(){}`.

A third way is using a function constructor. Not used very often. Looks like this
```
var report = new Function("val", "console.log(val);");
```

An interesting thing about object creation. When one is created, is is assigned a space in memory. When you use the variable to call the function, you're really just using a reference to that space in memory. Same thing applies when you make a new copy of an object. You're really just copying the reference to the original version.

This link is continous. If the original object is changed, all copies will be affected as well. Changing the original affects the space in memory, therefore any reference pointed at that space will inherit the changes.

---
**First Class Functions**

```
var sum = function( x, y){
    return x + y;
}
```

```
var a = sum
```

Both of these are pointing to the same space in memory.

```
var run = function(z){
    z()
};

run(a);
```

run is a function that takes another function as an argument, then executes it. Despite being inside another variable and function, `run(a)` still points to the original space of `sum`, retaining all properties of it.


**Invoking Functions**

Ways to Invoke
- As a function
- As a method
- As a constructor
- Indirectly using call() and apply()

Every function, in addition to the parameters that are passed into it, recieve two additional parameters. `this` and `arguments`. Each of the ways to invoke the method affect what `this` refers to.


*Example of this and arguments at work*

```
var test = function(val) {
    console.log(val)
}
```

Calling `test(3)` will log `3` of course. But what if you called `test(3, 4, 5)`? That would still only log `3`. Because the function was only set up to recieve a single parameter.

```
var test = function(val) {
    console.log(this) //logs Window
    console.log(val)
    console.log(arguments) //logs an array containing each argument.
}
```

Above is an updated version of the function, to demonstrate the values of `this` and `arguments`. So if we wanted to make the function return the sum of all arguments passed into it, we would write this:

```
var test = function(val) {
    let sum = 0;
    arguments.forEach( arg => sum += arg)
    return sum
}
```
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




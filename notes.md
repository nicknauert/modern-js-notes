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

## Advanced Functions and Objects
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
## First Class Functions

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

---
## Invoking Functions

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
    let args = [...arguments];
    args.forEach( arg => sum += arg)
    return sum
}
```

Because `arguments` is only an array*like* object, we need to dump the values into a proper array in order to have access to all the handy dandy array methods.


---
## Understanding this

`this` is determined at runtime, when a function is invoked. Determined by how a function is invoked, not where the function is defined. `this` is a reference to the object.
`this` is not *the function*. Though it is established when the function is invoked, it is not the function.

The binding of a value to this (this binding) can be either implicit (set by the js engine) or explicit (set by you);

### this In Normally Invoked Functions

```
let name = "global"

let fun1 = function(){
    let name = "fun1"
    console.log('From fun1 --------');
    console.log(this);
    console.log(this.name);
}

fun1()
```

In this case, running fun1 will log `Window` and `"global"`. This is because fun1 is invoked in the global space, assigning Window to `this` and therefore `this.name` will be equal to global. 

```
let name = "global"

let fun1 = function(){
    let name = "fun1"
    console.log('From fun1 --------');
    console.log(this);
    console.log(this.name);
    return function fun2(){
        let name = "fun2"
        console.log('From fun2 --------');
        console.log(this);
        console.log(this.name);
    }
}

fun1()();
```

In this case, the same thing is logged for both functions. They are both invoked in the global space, therefore they refer to the same function.

```
let name = "global"

let runIt = function(fn){
    let name = "runIt"
    console.log('From runIt --------');
    console.log(this);
    console.log(this.name);
    fn();
}

runIt(function fun2(){
    let name = "fun2"
    console.log('From fun2 --------');
    console.log(this);
    console.log(this.name);
});

```

Same result above.

## Understanding Prototypes

- Most objects are linked to other objects. That linked object is called the prototype.
- Objects inherit properties and methods from it's prototype ancestry.
- A prototype is automatically assigned to any object.
- You can define and objects prototype.
- You can change properties and methods of a prototype.

```
let obj = {};
"toString" in obj;
//returns true because toString is part of the prototype.

obj.hasOwnProperty("toString);
//returns false, because hasOwnProp checks whether the property is defined in the object.

dir(date);
// Allows you to inspect date object. Prototype is an Object.

obj.toString = function(){
    console.log("toString Function");
}

obj.toString();
// "toString Function"
// Defined properties will overwrite prototypal properties

```

### The Prototype of Function
```
let test = function(){
    console.log('test');
}

dir(test);
```

Inspecting `test` will show that functions have both a __proto__ as well as a `prototype` property. `prototype` is used for if the function is used as a constructor. __proto__ includes `apply` and `call`. These are used to change the value of this upon calling the function.

## Using call and apply function methods

Using either of these methods changes the binding of `this`. Using them allows you to invoke a function as if it were a method of some other object. 

- Using call
    - function.call(this, arg1 arg2)
    - First arg is an object that will become the value of `this`
    - One or more arguments to be sent to the function may follow
- Using apply
    - function.apply(this, [arg1, arg2])
    - First arg is an object that will become the value of this
    - More args to be sent in a single array


```
let greeting = function(){
    console.log('Good morning')
}

greeting();
greeting.call();
greeting.apply();
```
All of these console log 'Good morning.'

```
var user1 = {
    firstName: "Nick",
    lastName: "Nauert",
    fullName: function(){
        return this.firstName + " " + this.lastName;
    }
}

var user2 = {
    firstName: "Clint",
    lastName: "Eastwood",
    fullName: function(){
        return this.firstName + " " + this.lastName;
    }
}

let greeting = function(term, punct) {
    console.log(term + " " + this.firstName + punct);
}

greeting.call(user2, "Do you feel lucky", "?");
greeting.apply(user2,["Do you feel lucky", "?"]);
```
This will return "Do you feel lucky Clint Eastwood?" By using call, you have reassigned the value of this inside of the greeting function. The value of it will be assigned to the object passed in via the call method. The apply version works exactly the same, except your arguments are passed in as an array.

Here's a weird one.

```
user1.fullName.call(user2);
```

This will return user2's full name. Even though the method is invoked on user1, the call method overwrites that entirely.

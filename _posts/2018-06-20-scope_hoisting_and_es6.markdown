---
layout: post
title:      "Scope, Hoisting, and ES6"
date:       2018-06-20 03:06:16 -0400
permalink:  scope_hoisting_and_es6
---


## What is Scope?
Scope is the concept of where something is available. In the case of JavaScript, it has to do with where declared variables and methods are available within our code.

In JavaScript there are two types of scope:

* Global scope
* Local scope

Global Scope:
```
var temp = " Sunny";

// code here can use temp

function theWeather() {

    // code here can use temp

}
```

Local Scope:
```
// code here can not use temp

function theWeather() {
    var temp = "Sunny";

    // code here can use temp

}
```

Just like with variables, functions also can have global or local scopes:

```
// 'aNumber' is declared in the global scope and available everywhere in your code:
function aNumber () {
  return 42;
}
// => undefined
 
// 'myVar' is able to reference and invoke 'aNumber' because both are declared in the same scope (the global execution context):
var myVar = aNumber() * 2;
// => undefined
 
myVar;
// => 84
```

## Function Scope
When we declare a new function and write some code in the function body, we're no longer in the global execution context. Variables or functions declared within a function become **local** to that function, and can only be accessed within the function.

```
function aNumber () {
  var myVar = 42;
 
  return myVar * 2;
}
// => undefined
 
aNumber();
// => 84

myVar * 2;
// Uncaught ReferenceError: myVar is not defined
```

In the above example we see the `myVar` is only available within the `aNumber()` function scope, and when it is invoked globally, it throws and reference error.

## Block Scope
As of ES6, JavaScript has partial support for block scope:

```
if (true) {
  const num = 72;
 
  let otherNum = 613;
}
 
num;
// Uncaught ReferenceError: num is not defined
 
otherNum;
// Uncaught ReferenceError: otherNum is not defined
```

Variables declared with `const` and `let` are block-scoped.

However, variables declared with `var` are not block-scoped:


```
if (true) {
  var num = 42;
}
 
num;
// => 42
```

## Don't forget to declare new variables
Suppose we define a variable within the following function. 

```
function yum() {
 candy = "Is Good"
}
	 
candy;
```
Since `candy` is defined within the function it would seem that its scope, is localized to that function. 

However when we call `candy` in the global context:
```
function yum() {
 candy = "Is Good"
}
	 
candy;
	 
// => "Is Good"
```
It is available! What gives?

Variables not declared with `var`, `let`, or `const`, **are always globally scoped**, regardless of where they are found in your code!

Unless absolutely necessary, it is better not to use global variables, as they introduce a lot of potential bugs, and a more challenging debugging process.
# Hoisting

Suppose you were trying to teach your friend how to prepare cereal and milk. Since this can be a complicated process, and your friend has no idea how this delicacy is prepared, you need to explain it step by step. It would probably go something like this:

1. Open the cabinet and grab a bowl
2. Open the drawer and grab a spoon
2. Grab the box of cereal from the pantry
3. Open the fridge and get the milk
4. Fill the bowl with cereal
5. Add milk to bowl with cereal
6. Use spoon to eat 

Now that you have explained how to prepare it, you can simply say, "Make a bowl of cereal". 

If we would want to represent that using Javascript, it might look something like this:

```
function makeCereal () {
   console.log("Instructions how to make cereal...")
};

makeCereal();

// LOG: Instructions how to make cereal...
```

It certainly wouldn't make sense to first say "Make a bowl of cereal", and then explain how its made!


```
makeCereal();

function makeCereal () {
   console.log("Instructions how to make cereal...")
};

// LOG: Instructions how to make cereal...
```

**Huh? How do these have the same return value???** 

## The Javascript Engine
The Javascript engine is a two phase process:

1. Compilation Phase
2. Execution Phase

During the compilation phase, the engine skips over the `makeCereal()` invocation and only stores the declared function in memory:

```
// The engine ignores the makeCereal() invocation during the compilation phase. 

makeCereal();

function makeCereal () {
   console.log("Instructions how to make cereal...")
};
```

By the time the Javascript engine reaches the execution phase, `makeCereal()` has already been created in memory. The engine starts over at the top of the code and begins running it line-by-line:

```
// During the execution phase, the engine invokes makeCereal(), which was already initialized during the compilation phase.

makeCereal();
 
// During the execution phase, the engine will simply ignore this function declaration that was already carried out in the compilation phase.

function makeCereal () {
   console.log("Instructions how to make cereal...")
};
```

This process is called **hoisting**. The name makes it seem like that the actual declarations are being physically "hoisted" to the top of your code, but this is not in fact what happens. Rather, the declarations are put into memory during the compilation phase, but stay exactly where you typed them in your code.

## Variable Hoisting
Based on our understanding of **hoisting** thus far, what will this snippet evaluate to:

```
function sayHi () {
  console.log(hi);
 
  var hi = "Hi There!";
}
```

Seemingly during the compilation phase, the variable `hi` would be stored in memory, and then during the execution phase, the console would log out the value that is stored in the variable `hi`, `"Hi There!"`.

Running the above code returns....drumroll please.......
```
function sayHi () {
  console.log(hi);
 
  var hi = "Hi There!";
}

// LOG: undefined
```

**UNDEFINED!!! What happened to our hoisting?**

The answer is hoisting **only** applies to `variable declarations`, and **not** `variable assignments`.

Quick refresher:

```
// Declaration:
var hi
 
// Assignment:
hi = "Hi There!"
```

So when we ran our  above code this is what actually happened:

```
function sayHi () {
 
 var hi

// the variable declaration was "hoisted" (note that none of the code actually moved, this is just to explain what's going on "under the hood")
  
 console.log(hi);
 
 //So when the engine runs console.log(hi), hi is undefined
 
  var hi = "Hi There!";

//hi is only given the value of "Hi There!" at this point
}

// LOG: undefined
```

If we would run the same snippet with an additional line: 

```
function sayHi () {
  console.log(hi);
 
  var hi = "Hi There!";
	
	return hi;
}

// LOG: undefined
// => "Hi There!"
```

We see that the value of `hi` is returned.

## Avoid confusing var hoisting
One way is to always declare the variable at the top of its scope: 

```
// BAD
function badFunc () {
  console.log(myVar);
 
  var myVar = 10;
}
// LOG: undefined
 
// GOOD
function goodFunc () {
  var myVar = 10;
 
  console.log(myVar);
}
// LOG: 10
```

The other way is to never use `var` .

## ES6 let and const
ES6 introduced two new ways to create variables `let` and `const` . 

Without getting into all the differences between `var` and `let` / `const`, in terms of hoisting `let` and `const` do technically get "hoisted". However, the JavaScript engine doesn't allow them to be referenced before they've been initialized.

```
hi;
 
let hi = "Hello There!";
// ERROR: Uncaught ReferenceError: hi is not defined

yo;

var yo = "Yo"
// => undefined

```
By using `let` we avoid the whole issue of having an undefined variable, and instead are being told we can not  reference that variable altogether.

# Lets talk about Functions
Let us examine these two functions:

```
const square = function(x) {
  return x * x;
};

console.log(square(3))
// => 9
```

```
function square(x) {
  return x * x;
}

console.log(square(3))
// => 9
```

As you can see, these two functions return the exact same value. The latter way is known as declaration notation. The syntax is slightly shorter, and does not require a semicolon after the function. 

There is however one subtle difference, and it has to do with...you guessed right, **hoisting!**

```
console.log(greeting());

const greeting = function () {
  return "Why hello there";
}
// => Uncaught ReferenceError: greeting is not defined

//Declaration Notation
console.log(greeting());

function greeting() {
  return "Why hello there";
}
// => Why hello there
```

Function Declarations are "hoisted" to the top of the scope, and can be used in the entire scope. 

This is not the case by functions defined with a variable. 

As we discussed previously regarding  variables, only the declaration is hoisted, but the assignment is not.

## Arrow Functions
ES6, introduced the arrow function. 

```
const arrowFunction = () => {
  return 'I am an arrow function!'
};

arrowFunction()
//=>  'I am an arrow function!'
```

Arrow functions, mostly act the same as regular functions, but there are a few differences:

* Arrow functions allow for implicit returns when the body is in parenthesis, (i.e. without brackets). Within brackets you must explicitly return

```
const timesTwo = (n) => (n * 2)
 
timesTwo(3) 
// => 6

const timesTwoWithBrackets = (n) => {n * 2}

timesTwoWithBrackets(3)
// => undefined

const timesTwoWithBrackets = (n) => {return n * 2}

timesTwoWithBrackets(3)
// => 6
```

* All arrow functions are anonymous. This is unlike regular functions, which take their names from their identifiers.

```
function myName() {}
 
myName.name 
// => 'myName'

(() => {}).name 
// ''
```


### Arrow Functions and `this`

In general when a function is invoked within another function, the `this` value is global:

```
  const coffee = {
    type: 'latte',
    order: function() {
      return function actualOrder() {
        return `I am a ${this.type}`
      }
    }
  }
  coffee.order()()
  // "I am a undefined"
```

As you can see,`this` is undefined. That is because the context has reverted to global context, i.e. outside of the `coffee` object. So how would we fix this, if we wanted to have access to `this` in the inner function?

```
  const coffee = {
    type: 'latte',
    order: function() {
      return function actualOrder() {
        return `I am a ${this.type}`
		}.bind(this)
      }
    }
  }
  coffee.order()()
  // "I am a latte"
```

By using `bind` we bind the context of the `this` value to the `coffee` object.

Another way to achieve the same result, is by using an arrow function. By using an arrow function, the inner function retains the scope of the method it was declared in. 

```
  const coffee = {
    type: 'latte',
    order: function() {
      return () => {
        return `I am a ${this.type}`
      }
    }
  }
  coffee.order()()
  // "I am a latte"
```
As you can see, this inner arrow function retains the context of the outer `order` method. Just as the outer `order` method's context is `coffee`, the inner function's context is also `coffee`. The arrow function has performed .bind(this) for us behind the scenes.

## To recap
### Scope

In Javascript, there are two types of scopes, `global` and `local`. Understanding what scope you are currently working in, is a key tool to coding with Javascript.

Unless you are trying to define a global variable (not particularly recommended) always use `const`, `let`, or `var` to declare a variable.

While `const` and `let` maintain block scope, `var` does not. Another reason to not use `var`.

### Hoisting

Hoisting is one of Javascript's quirkier concepts, but it is very important to understand how it works. 

By following these two simple rules, you can avoid any potential hoisting confusion:

1. Declare first and use later. 
2. Don't use `var`, only `let` and `const`

### Arrow Functions

ES6 arrow functions allow us a shorthand syntax for writing functions (ideal for one liners). 

Arrow function also allow us to retain the same scope of where it was declared.




# Javascript Advanced Course

# Javascript Foundations

How does your code run on browser? How is it possible to tell the computer to run something or simply assign a variable using JavaScript?
The computer (and most specifically, the browser) can understand the code you wrote in JavaScript using what is called an **ECMAScript Engine**. The **ECMA**s, at the end of the day, are just some standards that an engine must follow to be considered as it.
There are a tone of Javascript Engine. The most popular (and actually fastest) one is the **V8 Engine**, built by Google in C++.

## How does an engine works?

But, how does an Egnine works?
To correctly work, an engine must first **Parse** the code you've written into tokens, that will then be passed over what is called an **AST** (_Abstract Syntax Tree_). Then, it passes the Tree over an **interpreter**, and the code gets "**Bytecoded**". The bytes gets into the CP in zeros and ones, the computer understands them, and execute the code.

Summarizing:
1. The JS file goes into a **Parser**
2. The parsed instruction goes into an **AST**
3. The Syntax Tree gets into an **Interpreter**
4. The interpreter **Bytecode** everything in 0 and 1
5. The bytes goes into the CPU
6. The cool code you've written gets executed :)
   
Actually this is the flow of Pure Javascript (because it's an interpreted language). But, if you work in (for example) TypeScript, the flow is a little bit different: after the interpreter, the code does't get immediately bitecode, but goes into a **Profiler**, then a **Compiler**, and finally you get the **Optimized Code**.

So, for compiled languages the flow is:
1. The JS file goes into a **Parser**
2. The parsed instruction goes into an **AST**
3. The Syntax Tree gets into an **Interpreter**
4. The interpreter "pass the ball" to a **Profiler** (notes about it in the next chapter)
5. The profiler goes into the **Compiler**
6. The compiler outputs **Optimized code**, and you're happy about it
7. The bytes goes into the CPU
8. The cool code you've written gets executed (usually faster) :) :)

## Interpreted VS Compiled languages

The main difference between an **Interpreted language** and a **Compiled language**, is that in the interpreted one, the code gets analyzed line by line, and it translate the code "on the fly". On the other side, a compiled language takes all the code we've written, and translate in in _another language_ (Just like TypeScript compiles the code into JavaScript), that does the same thing that our code does, but the machine is able to understand it. The compiled language can highlights immediately errors, because if there's an error, the code cannot be compiled in the other language, while in the interpreted one only rise an error when the interpreter goes in that line of code.

A compiler takes more to run the first time (because it has to _read_ all the file, and _translate_ it), but one it's done, it runs faster. Why? Because it **optimize** the code. How? For example, it replace calls to functions with the same parameters (that, based on the parameters, always returns the same thing) with just the result, so that the function doesn't have to be re-executed.

Well, but, what if we combine the best of the two types? If we do that, we get what it's called a **JIT Compiler** (_Just In Time Compiler_), and that's exactly what browsers do (expecially the V8 Engine).
This is done by the **Profiler**. I know you've been asking wtf it does all the time, and now you'll finally get an answer.
What the Profiler does, is to get the result of the interpeter (that immediately returns bytecode), and takes notes on how to optimize the code for the next time it'll be executed, and pass it to the compiler, that compiles the code to run in a better way (and rather then return bytecode, it'll return optimized code).
This means that the execution of our Javascript Code will constantly improve.

Hope i wrote it down in an understandable way... ._.

### Is Javascript an Interpreted Language?

Well, knowing what you've read until now, the answer is:
> "It depends..."

It surely was an interpreted language, but, right now, it's evolved to be a compiled one (based on the engines, of course).
So, next time you'll get that question, you can sound smart and say "Well, It depends". 

## Optimize code

To help the engine to compile the code, you want to write it in an optimized way.
Let's see the basics (the now developers commonly doesn't use, but it's useful to know).

You have to be really really aware with:
* Eval()
* Arguments
* For in loops
* With
* Delete

This makes the code less optimized, because can't permit to use `inline caching`.
Another thing to mantion, are **Hidden Classes**. You should never declare a new object property, or at least, for 2 objects, you should declare them in the same order. I know it sounds confusing, but here an example of what NOT TO DO:

```javascript
function Animal(x, y){
  this.x = x;
  this.y = y;
}

const obj1 = new Animal(1,2);
const obj2 = new Animla(3,4);

obj1.a = 30;
obj1.b = 100;
obj2.b = 30; //NOOOOO
obj.a = 100; //NOOOOO x2. You should swap them.
```

This "slow down" happens also with the `delete` key, becuase you change the Hidden class that the optimizer create.

### WebAssembly

WebAssembly is a standard binary executable format that the browsers must follow. It's a small note but it's important to know it :D.
Here a link for more about it: https://webassembly.org/.

## Call stack and Memory Heap

Call stack and memory Heap are two important part of the Javascript workflow.
The **call stack** is the history of the code to execute (in order). A new call function will be put on top of the call stack, and will be executed whenever it can. Is a region of memory that operate in a "FILO" way (First in, Last Out).
**Memory Heap**, on the other hand, is a place that the Javascript Engine provides to us to store our data. You don't have to worry about how it happens, because the engine will do it for you.

### Stack Overflow & Memory Leak

The **Stack Overflow** error can rise if a function gets called repeatedly (for example recursevely). It means that the function continue to add a function call to the call stack, until it's full and cant' store any more call.
Does a similar error exists for the Memory Heap.
Well, in Javscript, no. That's because it does **Garbage collection**, which means that if a variable isn't used anywhere, it deletes it form memory (technically, it does this when there aren't any pointers to a certain variable). This prevents **memory leaks**. Languages like JavaScript and Java handle this automatically, while in other languages like C, you have to control everything (this is more dangerous, but faster because the computer doesn't have to think about collecting garbage).

Note: in an infinite loop where you constantly add items in an array, you can stille cause memory leak in Javascript (try it in the browser XD). A memory leak can also be caused with too many event listeners on the page.

Fun Fact: to prevent the stack overflow in a recursive function, you can add the call of the function as the callback to a setTimeout, with the set to 0. This will cause the function to still be removed from the call stack (setTimeout runs in the Web API. To know more, see the "[Javascript Runtime](#js-runtime)" section).
Example:

```javascript
const list = new Array(60000).join('1.1').split('.');
function removeItemsFromList() {
  var item = list.pop();
  if (item) 
    setTimeout(removeItemsFromList, 0); // Without the "setTimeout", it cause a stack overflow.
};
removeItemsFromList();
```

## Single threaded

Javascript is a single threaded language. What does it means? It means that it can only do one thing at a time. You can find if a language is single threaded also based on the number of call stacks: Javascript has a single call stack, so is single threaded.
Because of this, Javscript is Synchronous. Synchronous code, actaully, can have problems with really long tasks, because can block everything until that long function ends its execution. BUT, you'll never see Javascript running "stand-alone", because there will be the **Web API** (or something else), where the Javascript Engine can do Asynchronous operations. That's the **Javascript Runtime**

## Javascript Runtime {#js-runtime}

The Javascript Runtime is the "space" where the operations happens on the browser. It's composed of:

* The Javascript Engine (what we already know)
* The Web API - adds functions to JS (just like `setTimeout()`, `setInterval()`, ecc.). It also permit to have asynchronous functions.
* The callback queue - The queue of the asynchronous functions to execute that will be added in the Javascript call stack
* The event loop - Constantly checks if the call stack is empty. If it is, it check the callback queue, and if there is a callback that must be executed, it adds it in the Javascript call stack.

To have a deep understanding on how this all fits together, see this link for a practical dimostration: [Latenflip](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gcHJpbnRIZWxsbygpIHsNCiAgICBjb25zb2xlLmxvZygnSGVsbG8gZnJvbSBiYXonKTsNCn0NCg0KZnVuY3Rpb24gYmF6KCkgew0KICAgIHNldFRpbWVvdXQocHJpbnRIZWxsbywgMzAwMCk7DQp9DQoNCmZ1bmN0aW9uIGJhcigpIHsNCiAgICBiYXooKTsNCn0NCg0KZnVuY3Rpb24gZm9vKCkgew0KICAgIGJhcigpOw0KfQ0KDQpmb28oKTs%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

### Node JS

Node JS is NOT a Javascript Engine. It's a whole Javascript Runtime. Why that? Because in NodeJS you don't use Javascript to run client code on the browser. You use javascript to run server code. That's why it MUST contain his personalized version of a Javascript Runtime (similar, but not identical, to what browsers have). It's NON-Blocking, so you can be sure about long time tasks even if it's single threaded :).

## Execution Context

Javascript has something called "**Execution Context**". An execution context is created every time a function gets called. Initially, Javascript creates a `global()` Execution context. The execution context has a structure of a call stack. Every context has a `Global Object` (which in the browsers is the `window` object) and the `this` Keyword. In the global context, the Global Object and the keyword this are equals. When the function runs, it will start adding values onto the execution context (but before, there is [Hoisting](#hoisting)).
Each function call will also have an `arguments` variable inside its context (will not be available in the global context).

Each declared variable in a function will be put inside its context (in another words, each execution context has its own variables).
When we pop a context, every variable declared inside will be removed from memory.

Another part of the execution context is the **Scope chain**. Every execution context has a link on the parent. This allows us to access the variable envirnoment of each parent function.

### The "arguments" keyword

If you remember, the keyword "arguments" is one of the things to NOT USE if you wanna optimize the code. You will rather do something like this:

```javascript
function ok(...args){
  console.log(args[0], args[1])
}
```

### Notes about eval() and with

Another things you've already seen to optimize the code are to not use `eval()` and `with`. That's because they "trick" the Javascript engine and can cause unoptimized code (because with "eval" and "with" you can't create a scope chain).

### A weird thing in JS

Javascript here can be a little bit weird:

```javascript
function weird(){
  height = 50
  return height
}
weird()
```
Where do you think the variable height will belong to?
The answer is: in the global context. That's because the variable doesn't hava a `var` or anything before, so it'll chech if the parent has this variable until it arrive to the global context. If neither the global contex has the variable `height`, it will be created automatically (usually you don't want this).
To prevent this, you can write `'use strict'` in the first line of code. After that, the function will now rise an error.

### Block Scope vs Function scope

`let` and `const` are **block scoped**. A block scope is defined by curly braces ("{}"). Outside the curly braces, the variable declared with one of those keys will not be available. The `var` key is not block scoped, so if for example you declare a "var variable" inside an if statement or a for loop, it'll be available also outside.

### Global Variables and IIFE

Global variables are really something to avoid in JS. Other than polluting the memory heap and also the global execution context, you also can have collision problems within 2 different files (where a file override the global variable of another one). To avoid this, Javascript developers used (before the creation of JS modules, which we'll see later) the **IIFE** (Immediately Invoced Function Expressions). See this example:

```javascript
(function(){

})()
```

With this code, you tell Javascript to not do [hoisting](#hoisting), and the variable you declare inside will not be global. This technique is particuarly used in frameworks and plugins (such jQuery).
Let's see a detailed example:

```javascript
// NOTE: "myPlugin" will still be global, but it's just one variable
// and you will hardly have collisions with that
var myPlugin = (function(){ 
  function a(){
    return 5
  }
  return {
    a: a
  }
})
myPlugin.a() // Returns 5 :)
```

### The "this" keyword 😱

==`this` is the object that the function is a property of.==
It simply means this... let's see:

```javascript
obj.someFunc(this)
``` 

`this`, refers to the `obj` object. So, as said, "is the object that the function is a property of". I think that with the example makes more sense, isn't it?

Why it has been created?
1. Gives methods access to theri object
2. Execute the same code for multiple objects

> **Note:** `this` is not inherited! If you define a function _inside_ an object property, it will not inherit the `this` keyword. To do that, you have to use the `bind()` method or use an arrow function.

#### call(), apply(), bind()

Every declared function have these 3 methods: `call()`, `apply()`, and `bind()`. Let's see what they does.

* `call()` - The "call" method simply calls a function, but, in the first parameter, we can say what the object should refers to. This allows us to use an object method in another object that doesn't have that method. If the method accepts parameters, you can also declare them in the next parameters.
* `apply()` - Same of the call method, but instead of using `call(newThis, param1, param2, ..., paramN)` it only accepts two parameters: the "newThis", and an array (the list of the parameters)
* `bind()` - Accepts the same parameters of call and apply, but instead of calling immediately the function, it simply will return a new function with the `this` keyword and the parameters "fixed" in it.

```javascript
// call() example
const wizard = {
  name: 'Merlin',
  health: 50,
  heal(){
    return this.health = 100
  }
}

const archer = {
  name: 'Robin Hood',
  health: 30
}
wizard.heal.call(archer)
console.log(archer) // HE RESTORED HIS LIFE THANKS TO THE WIZARD, WOAHH!
```

## Lexical Environment

Lexical environment and analysis, simly checks "where the code has been written" (in the global context? Inside another function?). This, way, in can know to which "universe" the function/variable belongs to.

```javascript
function a(){
  function b(){
    return
  }
}
```

In the above example, we know that the function `b` is lexically inside the `a` function.
In Javascript, our lexical "scope" (availabel data + variables where the function was defined) determines our available variables. Not where the function is called (dynamic scope).

> Lexical Environment === \[\[scope]] (\[\[scope]] it's actually a variable)

## Hoisting {#hoisting}

**Hoisting** is the behavior of moving the variables or function declaration to the top of the respective environment during compilation phase. What it means is that you, for example, can call a function before it has been declared, because the hoisting phase will place the declaration on top.
So:

```javascript
console.log(sing())
function sing(){
  console.log('ooooh la la la')
}
```

This piece of code will not raise an error, because of hoisting :)
Note that not all languages works like that.
Functions are **Fully hoisted**, while variables are just **Partially hoisted**, meaning that they'll be available, but the value of the variable will be undefined  (before the declaration).

> **Note:** Hoisting doesn't work with `let` and `const` variables (only with `var`) and also with functions wrapped between parentesis:
> ```javascript
> console.log(daddy)
> console.log(mommy)
> console.log(issues)
> let daddy = 'daddy issues'
> const mommy = 'mommy issues'
> (function issues(){
>   console.log('I have daddy and mommy issues')
> })
>```
>In the above code, all the console logs will raise an error.

> **Note 2:** If you declare a function as `var f = function(){}` it will be partially hoisted (the value will be undefined and it will not be callable).

Hoisting will not phisically move the lines of code, it just allocate some memory to the found declarations.

==🔑 Hoisting happens in every execution context.==

## End of Javascript foundations

Gosh. We've done so much, didn't we? My god... You're such a good boy/girl.
BUT, we're not done yet ;)




# Types in Javascript

In Javascript, fortunately (or not?), there are only 7 types:

* Numbers
* Booleans
* Strings
* undefined
* null
* Symbol
* Objects

> **Note**: Arrays, Maps, and so on, are objects. Also, even if `typeof function(){}` will return `'function'`, also functions are objects.

## Primitive and Non-Primitive types

A primitive type is a data that only represents a single value. So, in the above list of types, numbers, booleans, strings, symbol, null, and undefined, are **primitive values**, because there is only a single value. On the other side, Objects and derivates like arrays, functions, and so on, are **Non-Primitive** types.
But, Javascript is a little bit tricky with that. Here's Why:

```javascript
true.toString() // 'true'
```

Why is a boolean acting like an Object?
Actaully, Javascript create a "wrapper function" for each primitive value, so that you can work with it (just like this: `Boolean(true).toString()`). That's why you can access methods on primitive values.

> **Note**: Why isn't an array a primitive type? To answer this question, see the implementation of an array :)

### Passing by value VS Passing by reference

Primitive types are **immutable**, meaning you can't really change them. If you point a variable to another one with a primitive type, you still can't change it's reference. Here's what i mean:

```javascript
var a = 5
var b = a
b++
console.log(a) // 5
console.log(b) // 6
```

Incrementing `b` didn't change the value of a. This is because in memory there is a ==pass-by-value==.

On the other hand, in Non-primitive types, there is a ==pass-by-reference==, meaning that a new variable will _point_ to the reference of the previous one, so if you change the value of the `obj2`, you will also change the value of the `obj1`.

```javascript
var a = {value: 5}
var b = a
b.value++
console.log(a.value) // 6
console.log(b.value) // 6
```

There is a big problem of this if you want to copy an object, beacuse if you have nested object, you really can only do a **shallow** copy (copy only the superficial values, while the nested ones are pointers to a place in memory). To avoid this, there is a little method:

```javascript
const superClone = JSON.parse(JSON.stringify(obj))
```

Note: this can have performance implications!!

## Type Coercion

Type coercion is something like this:

```javascript
1 == '1' // true (WTF)
```

Basically, type coercion means the language tries converting a certain type to another type. 

> Do all languages have type coersion? This question might sound difficult and tricky, but the answer is: "yes, because we always need to convert types in memory (there are only 0 and 1!)".

In Javascript, type coercion can be "removed" by using three equals instead of two (===).

> You should always use "===" to make the code more predictable.

To know which types get converted into another (such as `0 == false` and `1 == true`), see this link: [Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness?retiredLocale=it).

## Dynamic type languages and Static type languages

Javascript is a **dynamically type language**, because you don't have to declare the type of a variable (also in Python). Type checking is done during runtime, so if you assign the value of 100 to a variable, Javascript will know that'll be a variable of the type "number".
In **Statically typed languages**, instead, you always have to declare the type of the variable.

In terms of "good code", statically typed languages are **_A LOT_** better. You have less bugs, easy documentation, autocompletation on editors, the code will run faster (the engine doesn't have to get by itself the type of a variable), and so on. BUT, the code is "slower" to write, and can have less readability (all code will be full of types. I mean is useful to know, but can be hard to understand).

Also, there can be **strong** and **weak** types. JS is weak, because, for example, you can add a number to a string (this will concatenate the string with the number). Python is strong (but also dynamic), because you can't do operations like that.




# Closures and Prototypal Inheritance

After all this big knowledge, we're arriving at the edges.
The two Big things that makes Javascript so grate, are **Closures** and **Prototypal Inheritance**.
We'll dig more and more deep into the details of this two big pillars of Javascript, but, first, let's talk more about functions in Javascript, and, more specifically, about **Higher Order Functions** (HOF).

## Functions

### Functions are objects

To prove what we previously said (that functions are objects), Javascript offers an Object to create a function:

```javascript
const f = new Function('console.log("woohoo")')
f() // woohoo
```

Another point is that, actually, you can add properties to a function:

```javascript
function f(){}
f.uck = 'WTF'
console.log(f.uck) // 'WTF'
```

In JS, Functions are "first class citizens". Because:
1. You can assign a function to a variable
2. You can create a function that accepts as a parameter another function
3. You can return functions as values into another function

**Functions are data**. This idea of "first class citizens" property introduces Javascript to a whole world called **Functional Programming**.

> **Note**: Be careful in initializing functions inside loops.

### Higher Order Functions (HOF) {#hof}

Higher Order Functions (HOF), are functions that can take a function as an argument, or a function that return a function.
That's it ._.

Why are they so useful?
They can make the code extremly more readable, optimized, and most importantly, felxible. (DRY = Don't Repeat Yourself), because you're able to "create" specialized functions dynamically.

## Closures {#closures}

**Closures** is the first of the two pillars of Javascript.
==Closures are _the sum of the "first class citizen" nature of function and the Lexical scope_==. Closures allows function to access variables from an enclosing scope or environment even after it leaves the scope in which it was declared.
Sounds pretty confusing, I know.
Let's see in concrete what it means:

```javascript
function a(){
  let grandpa = 'grandpa'
  return function b(){
    let father = 'father'
    return function c(){
      let son = 'son'
      return `${grandpa} > ${father} > ${son}`
    }
  }
}
const one = a()
// a()()() --> 'grandpa > father > son'
```

If you think about it, that's normal. Isn't it?
But, after we call `a()`, it'll be removed from the call stack with all its execution context (meaning all variables will be lost). How is it even possible that the nested functions will have still access to its variables?
Guess what. That's because of **closures**.
The execution context will be removed, BUT, because the inner function references the variables it had (the inner functions have _pointers_ in the parent function), these variables won't be lost.
How cool is that?
The `c` function, will look in its variable envirnonemnt and see if it has `grandpa` and `father`. It doesn't find them, but, instead of looking the parent function, it looks in the "closures box" (allocated memory).
That's why closures are the sum of functions and lexical scope: ==where we write the function matters, not where we called it==. That's a really important concept.

You can have a lot of hidden power with this, because you can create a parent generic function, and you can create other functions based on that one. See this example:

```javascript
const boo = (string) => (name) => (name2) => console.log(`${string} ${name} ${name2}`)

const booString = boo('hi)
const booStringName = booString()
...
```

The two main reasons why you want to use closures are:
1. **Memory efficient**
2. **Encapsulation**

### Memory efficient

The reason why closures are memory efficient, is because ==you don't create and destroy every time the data==, but you call the function once, so you allocate only once a "semi-persisent" part of memory, that will be then used by nested functions.
In concrete:
This Re-create the bigArray every time... VERY BAD!

```javascript
function heavyDuty(index){
  const bigArray = new Array(7000).fill('🤓')
  console.log('created')
  return bigArray[index]
}
heavyDuty(420)
```

This one create the array only one time. VERY GOOD AND EFFICIENT!!

```javascript
function heavyDuty2(){
  const bigArray = new Array(7000).fill('🤓')
  console.log('created again!')
  return function(index){
    return bigArray[index]
  }
}
const getHeavyDuty = heavyDuty() 
```

### Encapsulation

Encapsulation permit to have ==more security over data==, because data cannot be directly exposed.
Let's see this big example:

```javascript
const makeNuclearButton = () => {
  let timeWithoutDestruction = 0
  const passTime () => timeWithoutDestruction++
  const launch = () => {
    timeWithoutDestruction = -1
    return '💥'
  }
  setInterval(passTime, 1000)
  return {
    launch: launch,
    totalPeaceTime: totalPeaceTime
  }
}

const ohno = makeNuclearButton()
ohno.totalPeaceTime() // 0
// Wait 10 seconds
ohno.totalPeaceTime() // 10
ohno.launch()
ohno.totalPeaceTime() // -1
```

Here, you cannot access and modify by yourself the `timeWithoutDestruction` variable, so the function is more secure.

## Prototypal Inheritance

**Prototypal Inheritance** is the second of the two big pillar of Javascript.

==Inheritance is an object getting access to the properties and methods of another objects==. For example, as we said before, arrays and functions are just objects. This means that they inherit all methods/properties of Objects.
You can check this by doing:

```javascript
const arr = []
console.log(arr.__proto__) // All methods/properties of arrays
console.log(arr.__proto__.__proto__) // All methods/properties of objects
```

Javascript is different from other languages becasue of this. Other languages doesn't have the prototype chain.

Prototype chain allows us to make an object inherit all methods and properties of another one (without override them).
You can do that by assigning the `__proto__` object to another value:

```javascript
const person1 = {
  name: 'Tongi',
  lastName: 'Patongi',
  sing: () => {
    console.log('ohhh la la la)
  }
}

const person2 = {
  name: 'Cicci',
  lastNmae: 'Burricci
}
person2.__proto__ = person1
person2.sing() // ohhh la la la

person2.isPrototypeOf(person1) // true
person1.isPrototypeOf(person2) // false

person2.hasOwnProperty('sing') // false. But you can access it
person2.hasOwnProperty('name') // true
```

You can also create an object based on another one. This new created object will inherit all attributes of the parent, this way:

```javascript
let human = {
  mortal: true
}
let socrates = Object.create(human) // will inherit properties of human
console.log(socrates) // {} -> it's empty
console.log(socrates.human) // true. You should now know why :D
```

Although you can use and modify the `__proto__` value, you should never modify it by yourself, for performance reasons.
"But man, then Why TF are you teaching me this??"
First, because it's useful to know if you really wanna go deep down into the Javascript language, and, second, because it's the mechanism used in Object Oriented Programming (OOP) with Javascript.

Actually, the `__proto__` property points to the parent `prototype` property.

```javascript
child.__proto__ == parent.prototype // true
```

> **Note**: Only functions have the `prototype` property, that you can also modify (without performance implications)

> **Note**: the `Object` object (lol) it'a actually a function (you can check it by typing `typeof Object`). That's why yoy can access the `prototype`property.

The only time we use the `prototype` attribute is when we want to create **classes**. To do that, we want to create a function with a capital letter. I won't waste your time with complicated examples right now. It would look confusing, maybe. We'll see it more and more deeply in the [OOP section](#oop). Let's just see a quick example.
Adding a function to the `Date` object:

```javascript
Date.prototype.lastYear = function(){
  return this.getFullYear() - 1
}
new Date().lastYear() // 2021
```

Using the Javascript **closures** + **Prototypal Inheritance** you can now create really interesting and efficient programs.




# Object Oriented Programming (OOP) {#oop}

_Object Oriented Programming_ and _Functional Programming_ are what we call **Programming paradigms**. They allows us to organize code in a way more readable, clear, easy to extend, easy to maintain, memory efficient, and most importantly, DRY (don't repeat yourself).

In all programs there are 2 primary component:
1. Data
2. Behaviors

OOP is like building a robot, where with every single component you build the whole thing, while in FP you simply pass some data and you get a result.

In OOP you wanna simply look at the world, and organize the code in components that reflects what you see. Example: in a big fantasy game, you have maybe an elf, a wizard, a warrior, a dragon, and so on. You can so see a general `Character` component of the game. They all share the same properties (name, type, weapon, the "attack" method, ecc). But, maybe a wizard che also have an "heal" method, so the wizard will _extends_ the `Character` component.
Well, you can create the characters of the game with a **factory function**, where based on the parameters you return an object with the data of the character. Unfortunately, this is not really memory efficient, because for each elf or whatever character you create there will be an _attack_ function, and so on. Really bad.

Before you knew about prototypal inheritance, you would for example extract the attach function to a new object called `characterFunctions`, containing each function a character can do.
But, this is really something not clear and readable (also unhandy).

With **prototypal inheritance** you can clean up the code with `Object.create(characterFunctions)`. Very, very cool.
But, here's a thing. This is not really Object Oriented Programming, yet. How can we get closer?

## Constructor functions

Instead of using `Object.create()`, you can create a function where you just set the properties, with `this.proeprty1`, and so on. But, if you do that, you have to use the `new` keyword:

```javascript
function Character(name, weapon){
  this.name = name
  this.weapon = weapon
}
const elf = new Character('Sam', 'stones')
```

Every function called with new up front, it's called a **constructor function**.

> **Note**: it'll not raise an error, but you really should define the name of a constructor function with a capital letter (e.g. `Person`, not `person`), so that other developers know that that function it's a constructor function.

When you use the `new` keyword, you change the `this` keyword value to be the object you just created (insted of window). If you remove `new`, the `this` keyword will have the value of the window.
Here, with the `prototype` property, you can add methods to this newly createed object.

```javascript
// ... previusly created constructor function

Character.prototype.attack = function(){
  return 'attack with ' + this.weapon
}
const elf = new Character('Sam', 'stones')
elf.attack() // attack with stones
```

How awesome is that?
The attack function will also be present only one time in memory, and not one for each character we create. So, so good.

But... this is not pretty. Is it? ._.

## ES6 Classes

With ECMAScript 6 (ES6), Javscript finally comes out with classes.
The beauty of classes is that you contain every related functionality in the "box" of that class, keeping everything in order. Let's re-create the previous Character object:

```javascript
class Character {
  constructor(name, weapon){
    this.name = name
    this.weapon = weapon
  }
  attack(){
    return 'attack with ' + this.weapon
  }
}
const peter = new Character('Peter', 'nuclear bomb')
peter.attack()
```

How cool is that?
In JS Classes, you define the constructor function with the `constructor` keyword, and define functions without the `function` keyword.

But, underneath the hood, we're still creating a prototypal inheritence. Technically, _Javascript doesn't really have classes_. It's just "_syntactic sugar_".

Even if it doesn't looks like OOP, using `Object.create()` it's **pure prototypal inheritance** (but it's not better than using classes or the prototype property. Just "pure". The true nature).

## Inheritance

One big part of OOP is **inheritance**. usually you have a "generic" class, and you want a new object to "extends" the behavior of the generic one. For example, in our fantasy game, we want to implement the wizard, which is a normal character, but it also can heal other characters. It can also have a new `type` property.
Before we had the class syntax, it was really difficult implementing inheritance.
But, here's how you do it in ES6:

```javascript
class Wizard extends Character {
  constructor(name, weapon, type){
    super(this)
    this.type = type
  }
  heal(characterToHeal){
    return 'heal ' + characterToHeal.name
  }
}
const wizard = new Wizard('Merlin', 'stick', 'idk')
wizard.attach() // attack with stick
wizard.heal(peter) // heal Peter

Wizard.isPrototypeO(Character) // true
wizard isinstanceof Wizard // true
wizard isinstanceof Character // false
```

With the `super` method you access the constructor of the parent class (`Character`, in this case). That's why you have to have in the parameters (in this case) also the parameters that the parent class have.

> **Note**: In every extended class, you MUST call `super()` before accessing the `this` keyword in the constructor.

### The Gorilla-Banana Problem {#gorilla-banana}

There's a problem in inheritance, called the **gorilla-banana** problem, where maybe you ask for only a banana, but instead you get the gorilla holding the banana and maybe also the whole jungle behind.
You would look the gorilla and say... WTF.
This funny example show a big problem of OOP, that is the fact that if you change something in the parent element, all the inherited objects will also have that change. This is, actually, usually good, but not always.
In our fantasy game, if you want a `JuniorWizard` Character that can only heal, without attack, but inherits everything from the `Wizard` class, you would have a problem, because the JuniorWizard would still have access to the attack method. This logic also apply if you simply wanna add a method in the Character class, but you don't want ALL the children to have that method.
In this cases, OOP becomes really inefficient in term of coding simplicity, readability, and maintainability, because you would always have to refactor the code.

## Private and public fields

In JS, you really haven't private fields in classes. There's a standard that says you should use the underscore before the name of the field, but you can still access it, and that's a big problem.
But now, with ES2022 you can define a private method/property with the hash before the name:

```javascript
class Ehehe {
  #privateProperty = 200
  constructor(){
    console.log('ehehe')
  }
  #privateMethod(){
    console.log('private')
  }
}
const hehe = new Ehehe()
hehe.#privateMethod() // ERROR
hehe.#privateProperty // ERROR
```

## Pillars of OOP

When it comes to OOP you also just learn:
1. **Encapsulation** (wrap code into boxes related the one to each other). Re-usable code
2. **Abstraction**. Hiding the complexity for the user. Create simple interfaces.
3. **Inheritance**. Avoid having to re-write code.
4. **Polymorphism**. Ability to call the same method on different objects, getting different results.




# Functional Programming (FP)

In recent years the popularity of **functional programming** increased. Functional programming is all about separation of concerns and packaging the code in one box.
Generally, functional programming is a synonim of **simplicity**, because you separate data and functions. A functional programmer views the world as "well, there's data, and function that can interact with it".
The goals of FP are the same of OOP:
* Clear/Understandable
* Easy to maintain
* Easy to extend
* Memory efficient
* DRY

But, when it comes to FP you have one important pillar (rather than 4 like OOP): **Pure Functions**.

## Pure functions

A pure functions is a function that:
1. Always return the same output given the same input
2. Don't modify the data outside itself (side-effects)

For example:

```javascript
const array = [1,2,3]
function a(arr){
  arr.pop()
}
a(array)
console.log(array) // [1, 2]
```

The function in the above example modify the value of the original array. So, This is NOT a pure function. A behaviour like that can be really confusing and less maintainable. This is called **side effect**.

How can we write something without a side effect?
Well, we can, in the above example, copy the array and use the copy, rather than the original. This way, data is more secure, and the function doesn't affect the outside world.
Here's a tricky question: is the function below a pure function?

```javascript
function a(){
  console.log('hi)
}
```

The answer is: "no, it's not, because it affects the outside world (the `console` is a window variable)".

But, we've talk enough about the second point. What about the first one?
Look at this function:

```javascript
function a(num1, num2){
  return num1 + num2
}
a(5, 2) // 7
```

If I completely replace the call to the function with its result, will it affect the program? In this case, no. The function, with the parameters 5 and 2, will always return 7. This is called **Referencial Transparency**. That function also have no side-effects, so it's a _pure function_.

Can everything be pure? Ahahaha, no.
There will always be side effects, because you maybe make API calls, you interact with the browser, and so on, that means that you are doing something in the "outside world".

> The goal of Functional Programming is not to make everything "pure". ==The goal of Functional Programming is to minimize side effects==.

A perfect function should:
* Do 1 and only 1 task
* Always have a return statement
* Be pure
* Not have shared state
* Be immutable
* Be Composable
* Be predictable

At the end of the day, functional programming is just making the code... _predictable_.

## Indempotence

Indempotence means that, similar to pure functions, given the same inputs you get the same output over and over again, but, you also get the same result if you call the function with the result of the same function as a parameter. Here's what I mean:

```javascript
Math.abs(-50) // 50
Math.abs(Math.abs(-50)) // 50
```

The `Math.abs()` function is indempotent.

## Imperative vs Declarative

Imperative code is code that tell the machine what to to and how to do it. Declarative code, is a code that tell the machine what to do, and what should happen. It doesn't tell how to do it.

Technically, the computer likes Imperative code, but humans likes declarative.
Examples:

```javascript
// Imperative - You say "save i, console.log i, increment i, repeat"
for(let i=0; i<1000; i++){
  console.log(i)
}

// Declarative - You just say: "for each item, console.log the item"
[1,2,3].forEach(item => console.log(item))
```

Also, jQuery is very imperative, while React is very declarative.
That's why `React` is better for our mental health :D.
Just kidding (no i'm not).

## Immutability

Immutability simply means "not changing the data", "not changing the state".
You should alway cloning the object, modify it, and return the clone. Never the original data.
In functional programming, the idea of **Immutability** is very important.
But, should't it pollute the memory?
Well, you got a point, but, actually it does not. Ther's something called **Structural sharing** (that is a little beyond of this guide). When it comes to functional programming, a lot of languages have this. Instead of storing the entire copy, it just save the changes you do to the data.
Hope you got it, 'cause I'm not gonna say a single word more than this.

## Higher Order Functions and Closures

We've already spoke about [Higher Order Functions](#hof) (click the link :D).
We've also already spoke about [Closures](#closures) (click the link :D)
So, why TF there is this section? Literally for nothing, I'm sorry.
Is think is just to say that what you previously learned it's really imporant for functional programming. Don't forget about these concepts.

Also, even if closures have inner functions that can modify the state of the parent functon, they are to consider pure as long as they don't modify the outside state of the application.

## Currying

Currying is the technique of translating the evaluation of a function that takes multiple arguments into evaluating multiple functions that only takes one parameter.

```javascript
const multiply = (a, b) => a*b // Not curried
const curriedMultiply = (a) => (b) => a*b // curried

curriedMultiply(5)(3) //15
```

Why is this useful? We've seen also this before, but maybe not as technically. With currying, you can create multiple utility functions based on the "first" function.
For example:

```javascript
const multiplyBy5 = curriedMultiply(5)
const multiplyBy10 = curriedMultiply(10)

multiplyBy5(10) // 50
multiplyBy10(5) // 50
```

## Partial Application

**Partial application** is a process of producing a function with a smaller number of parameters. It's similar to currying.
Let's see how it look and compare it to currying.

```javascript
const multiply = (a, b, c) => a*b*c
const curriedMultiply = (a) => (b) => (c) => a*b*c // curried

curriedMultiply(5)(4)(10) // 200

const partialMultiplyBy5 = multiply.bind(null, 5)
partialMultiplyBy5(4, 10) // 200
```

Partial application says that, on the second call, it expects all the arguments, while currying is expecting one argument at a time.

## Memoization

Functional programming massivly use, with closures, **memoization** (see more in the other guide).

## Composition and Pipe

Is the idea that ==any sort of data transformation that we do, should be obvious==. It's a system design principle.
This way, you can create "factories":

```
Data -> Function -> Data -> Function -> ...
```

We wanna so build a `compose()` function that accepts multiple functions as parameters, and every function we pass will return, in order, the result for the next one.
In the next example, we create a function that multiply by 3, and then return the absolute result.

```javascript
const compose = (f,g) => (data) => f(g(data))
const multiplyBy3 = (num) => num*3
const makePositive = (num) => Math.abs(num)
const multiplyBy3AndAbsolute = compose(multiplyBy3, makePositive)

multiplyBy3AndAbsolute(-50) // 150
```

We can so compose functions to create new functionality.
This is so diffused that there are actually a lot of libraries that just do that.
The order of execution in the above example is:
1. `makePositive`
2. `multiplyBy3`

Because the result of `compose` is `f(g(data))` (`g()` gets executed first).
But it's not that clear, isn't it?
Wouldn't be great if the execution order of the functions is the same of the order you pass these functions in the parameters?
This is called **Pipe** (which I think is better, but it's just personal).
To implement the Pipe in the above example, you just have to swap the `g()` function with the `f()` function.

With composition, you can also avoid the [gorilla-banana problem](#gorilla-banana).

## Arity

Arity is simply the number of arguments the function takes.
For example, the function `f(param1, param2)` has an arity of 2.
Pretty simple, right?
In Functional Programming, a good practice is to have the fewer amount of parameters as possible, because the less parameters there are, the more the function is easier to use, because you can apply more interesting things (like composition/pipe) and keep the code readable and clean.

## The Amazon Example

To really have a basic foundation on how and when to use FP, i will make a big example that will integrate everything we've learned so far.
We have a user, and we want to:
1. Add items to cart
2. Add 3% Tax to item in cart
3. Buy item: cart --> puchases
4. Empty cart

```javascript
const user = {
  name: 'Kim',
  active: true,
  cart: [],
  purchases: []
}

const pipe = (f, g) => (...args) => g(f(...args))

function purchaseItem(...fns){
  return fns.reduce(pipe)
}

function addItemToCart(user, item){
  const updatedCart = user.cart.concat(item)
  return Object.assign({}, user, {cart: updateCart})
}

function applyTaxToItems(user){
  const {cart} = user
  const taxRate = 1.03
  const updatedCart = cart.map(item => {
    return {
      name: item.name,
      price: item.price * taxRate
    }
  })
  return Object.assign({}, user, {cart: updatedCart})
}

function buyItem(user){
  return Obejct.assign({}, user, {purchases: user.cart})
}

function emptyCart(user){
  return Object.assign({}, user, {cart: []})
}

purchaseItem(
  addItemToCart,
  applyTaxToItems,
  buyItem,
  emptyCart,
)(user, user, {name: 'laptop', price: 244}) // WOAHHH
```

Functional programming is so powerful because you can now, for example, add some "history" and go back and forth with the data.




# FP vs OOP

| FP | OOP |
| -- | --- |
| Many operations on fixed data | Few operations on common data |
| Stateless | Stateful |
| Pure functions (no side-effects) | Side effects |
| Declarative | Imperative |

Functional Programming Works really well for high performance in processors, for example.
If, on the other hand, you have many things with not so many operations, well then Object Oriented Programming might be the right solutions.
Of course, you can implement a mix of the two :D




# Asynchronous Javascript

In this part we're going to revist what we've already seen with the **Web API**, and so on.
Asynchronous code is used, for example, to make API Requests, and, basically, asycn functions are function that you can execute "later".

With ES6 Javascript comes out with **Promises**. A `Promise` is an object that may produce a single value some time in the future (either a result or a rejection). Promises were created to avoid the "piramid" problem (a lot of callbacks nested one to the other). It makes asynchronous code more readable, but you still have callbacks for `.then()` an `.catch()` ("then" for results, "catch" for errors).

With ES8, Javascript simplify things with the keywords `async` and `await`. This new feature make everything really more readable and understandable. You can mark a function as async (The result of the function will be a `Promise`), and you can put `await` in the asynchronous line of code.

```javascript
async function asynchronousFunction(){
  const results = await getResults()
  return results
}
asynchronousFunction() // Promise
```

This is asynchronous code, but it looks like if it's synchronous, right?

In ES9, again, Javascript added something really cool: the possibility to do an **"await for-of loop"**.
Ok, ok, let's see how you can do it:

```javascript
const urls = [
  'https://jsonplaceholder.typicode.com/users',
  'https://jsonplaceholder.typicode.com/posts',
  'https://jsonplaceholder.typicode.com/albums',
]
const getData = async function(){
  const arrayOfPromises = urls.map(url => fetch(url))
  for await (let request of arrayOfPromises){
    const data = await request.json()
    console.log(data)
  }
}
```

THIS IS SO COOL MAN, I SWEAR. 🔥

Ok, That's all for the cool, refreshing, staff that Javascript offers about asynchronous code.
Let's see now some theory (the hard and boring staff).

## Job Queue

Actually, we're missing a piece in the Javascript Runtime Enviroment. I'm sorry for that. I lied to you before, but i had to arrive here before mentioning it.
To solve the problem of Promises, the Javascript creators decided to add another queue: the **Job Queue**. It's similar to the **Callback Queue**, but it has _higher priority_ than it. It means that, before adding something to the callback queue, it checks that the job queue isn't empty.
Note that just like the Callback Queue, also the Job queue depends and is implemented by the browser.

## Parallel, Sequence and Race

When it comes to promises, there are 3 crucial things you have to decide, that is if the asyncronous code should run:
* **Parallel** - You want to execute everything in parallel.
* **Sequential** - You wanna run the first one, if succed, the second one, if succed, the tirth one.
* **Race** - You want to call, for example, 3 things, but when it comes the first one, ignore the rest.

Let's see an example for each one:

**PARALLEL:**

```javascript
async function parallel(){
  const promises = [a(), b(), c()]
  const [output1, output2, output3] = await Promise.all(promises)
  return `parallel is done: ${output1} ${output2} ${output3}`
}
parallel().then(console.log)
```

**SEQUENTIAL**:

```javascript
async function sequential(){
  const output1 = await a()
  const output2 = await b()
  const output3 = await c()
  return `sequential is done: ${output1} ${output2} ${output3}`
}
sequential().then(console.log)
```

**RACE**:

```javascript
async function race(){
  const promises = [a(), b(), c()]
  const output1 = await Promise.race(promises)
  return `race is done: ${output1}`
}
race().then(console.log)
```

## ES2020

ES2020 added a new feature to patch the problem that, with `Promise.all()`, if any of the promises throws an error, you get undefined as a result.
The new feature is `Promise.allSettled`, that returns an array of responses having the `status` and the `value` (or `reason`, if the promise throw an error)

## Threads, Concurrency and Paralellism

We said that Javascript is single-threaded, but, with the Web API, we can get multi-thread and paralellism to run asynchronous code.
Everytime you create a new tab on the browser, it'll be assigned to a new thread.
This is called a **Web Worker**. A Web Worker handle multiple threads on the background.
How can you create one?

```javascript
var worker = new Worker('worker.js)
worker.postMessage('Hellooo') // Post a message to another thread

addEventListener('message') // listen to this (note: it doesn't work)
```

To be honest, it's pretty useful. You'll never use a Worker. NEVER (trust me).
But, it's useful to know it's existance.

Javascript doesn't apply **Paralellism** (multiple CPU core running at the same time), but, node does.

In node:

```javascript
const {spawn} = require('child_process')
spawn('git', ['stuff']) // Genereate a new process that you can run.
```

This chapter is really confusing and "empty", but I'm not gonna deep down into this (i didn't get the point of it neither :D).
It's beyond the scope of this course.




# Modules

**Modules** are a really important topic in Javascript. They are different pieces of code groupped together with their own specific functionality.
We need a way in JS to import and export functionalities, and this comes with ES6. We'll see it later.

> The way you structure the data should be the most important part of the code

We've seen before the problem of polluting the global scope with generic functions and variables, even if you write multiple files and you include them in your page with different scripts, because then they will be merged together.
And what if one of your 100 files needs to have a certain variable, that maybe it's declared in a script positioned below than the one where it will be used? This will cause an undefined error.
Getting bigger and bigger, you can't really remember everything there is in all your 100 files.
That's why you _need_ to use modules.

But, JS modules are something really recent. How did this problem get resolved earlier?

## Module Pattern

The first of the many methods is the **Module pattern**.
The module pattern is structurized like this:

```
// Global scope
  // Module scope
    // Function scope
      // Block scope - let and const
```

This way we can be explicit on which functions, variables, and so on, we need to have in our module.
To achieve this, we need to use concepts like **encapsulation** and **closures**.
This is how it is done:

```javascript
var fightModule = (function(){
  var harry = 'potter'
  var voldemort = 'He who must not be named'
  function fight(){
    // ...
  }
  return {
    fight: fight
  }
})()
```

All the code we've written is private, so also `harry`, `voldemort`, and `fight` are private to that module and don't pollute the global scope.
Also, if we want an external module to access something, just like `fight()`, we can return it in an object, just like in the example.
So, if another module needs the fight method, you just have to access it like this:

```javascript
var anotherModule = (function(fightModule){
  var fight = fightModule.fight
  fight() // Potter wins
})(fightModule) // Needs the fightModule
```

Note that we pass in the arguments the fightModule. This is beacuse we don't want the `anotherMdoule` to modify the `fightModule`. It would be bad.

But... There are two main problems with this approach:
1. Even if you minimize the number of variables, you're still polluting the global scope
2. If you include a script with a module _after_ another script that uses it, you'll get an error. So, you have to be careful of the order.

## Common JS, AMD, UMD

Can we desgin a way to improve the module pattern (because of the problems we've seen)?
**CommonJS** and **AMD** (Asynchronous Module Definition) came out to resolve these problems.

CommonJS looks something like this

```javascript
var module = require('module1')
var module2 = require('module2)
```

NodeJS actually still uses CommonJS.
In CommonJS, modules are meant to be loaded synchronously, and that's not ideal for browsers (that's why it's mainly used in the servers).

This problem was solved by **Browserify**, that is a simple terminal application that reads the JS files and bunlde them togheter based on what each file requires (with the sintax of CommonJS), so that then you only have to include in your browser a sinlge `bundle.js` file.

On the other Hand, **AMD**, looks like this:

```javascript
define(['module1', 'module2'], function(module1Import, module2Import){
  var module1 = module1Import
  var module2 = module2Import
  function dance(){
    // ...
  }
  return {
    dance: dance
  }
})
```

I personally don't like it, but, it was designed specificately for browsers, so it loads modules asynchronously (that is way better than CommonJS for performance).

This two packages solve every problem we've mentioned before.
Except for one...

The community wanted to have a common place where they can store they're module (we're talking about `NPM` :D). Something **Universal**. But, some users use CommonJS... others AMD... How to solve this problem?
UMD (Universal Module Definition) came out, making a big scene.
...no, not really, because shortly came out the native module defintion of Javascript with ES6.
At the end of the day, UMD was just an "if-else" statement, so... not ideal, right?

## ES6 Modules

NOW, modules are built-in in Javascript, and they are very easy to use:

```javascript
import module1 from 'module1'

const harry = 'potter'
const voldemort = 'He who must not be named'

export function fight(){
  // ...
}
```

HOW. COOL. IS. THAT??
Simple and clear.
You can import some modules and export whatever you want to other modules. The `harry` and `voldemort` variables are not exported, so they are private.
Simply and clean.
This are ES6 Modules.

You can also `export default` something from a module.




# Errors

Errors or Exceptions are often inevitable. Perfect developers doesn't exists. For this reason, it's good to know how to handle an error.

## Try-Catch blocks

In JS there is a native `Error` object, that you can create. But, once you craete an error, nothing happens, because you have to `throw` it.

```javascript
throw new Error('oopsie')
```

Actually, you can throw everything, not just the Error object, but, the Error constructor is useful to get information about the error that happened, just like the `message`, and the stack (where we were in the call stack when the error happened).

> Throwing an error interrupt the execution of the program.

The `Error` contstructor is just a generic error. There are also specific ones:
* SyntaxError
* ReferenceError

If an error gets thrown, the runtime checks the call stack to know if there is a catch for that error, so that the error can be handled.
But, what is a catch?

A catch is simply a block of code that will be executed whenever an error occurs.

```javascript
try {
  consol.log('This works') // "consol" is a typo
} catch(error) {
  console.log('we have made an oopsie', error)
}
// we have made an oopsie
```

Or, you can manually throw an error. Also, you can add a `finally` block code that will be always executed:

```javascript
try {
  throw new Error('oopsie!!!')
} catch(error) {
  console.log(error.message) // oopsie!!!
} finally {
  console.log('This will ALWAYS be executed')
}
```

You can also have nested try and catch blocks.

## Asynchronous code errors

Promises and in general asynchronous code have a different type of error handling, which is the `.catch()`. The catch method is built-in in the Promises, so we can do this:

```javascript
Promise.resolve('asyncfail').then(response => {
  throw new Error('#1 fail')
  return response
}).catch(err => {
  console.log(err)
})
```

Without a catch block, you can't even see the error, and that's what is called **silence failing**. This is very dangerous, because you can't even know what is going wrong with your code.

But, with the new `async await` functions, you can still use the try-catch block.

```javascript
async function fail(){
  try {
    await Promise.reject('oopsie')
  } catch(err) {
    console.log(err.message)
  }
  console.log('still good?')
}

fail() // oopsie - still good?
```

## Extending Error

The `Error` is an object, so, technically, we can extend it.
For example:

```javascript
class AuthenticationError extend Error {
  constructor(message){
    super(message)
    this.name = 'AuthenticationError'
    this.favouriteSnack = 'grapes'
  }
}

const a = new AuthenticationError('oopsie')
a.favouriteSnack // grapes
```

You can so create your custom error. This method is usually adopted in servers, for example if you don't want the error to reveal the call stack or sensible data that a normal error can show.
You can also throw this custom errors, and handle them in the way you want.

# Conclusions

If you are here, i really have to give you a kiss on your forehead.
Nice Job. Really. I'm proud of you.
You can really say with honor that you are a **professional Javascript Developer**.
You can now tell with accuracy and determination what closures are, how the Javascript runtime works, how the Wep API Works, how to structure the code in a readable way, how to handle errors, what OOP and FP are, and so on...

Good boy.

Thank you for reading all of this <3.

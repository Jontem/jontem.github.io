---
id: 519
title: 'JavaScript series: Lexical scope, closures, function scope and block scope'
date: 2016-02-28T19:31:40+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=519
permalink: /javascript-series-lexical-scope-closures-function-scope-block-scope/
image: /wp-content/uploads/2016/02/javascript-825x200.png
categories:
  - JavaScript
  - JavaScript series
tags:
  - Block scope
  - Closure
  - Function scope
  - javascript
  - Lexical scope
---
## Lexical scope

Lexical scope which is also sometimes called static scope defines how variable names are resolved in nested functions.

<pre class="brush: jscript; title: ; notranslate" title="">function myOuterFunction() {
    var outerVariable = 10;

    console.log(globalVariable); // 5

    function myInnerFunction() {
        console.log(globalVariable); // 5
        console.log(outerVariable); // 10
    }

    myInnerFunction();
}

var globalVariable = 5;

myOuterFunction();
</pre>

With lexical scope we can access variables defined in parent scopes. Above `outerVariable` is declared in the scope of `myOuterFunction` but is still accessible inside the scope of `myInnerFunction`. The `globalVariable` is declared in the global scope and will be accessible everywhere.

<pre class="brush: jscript; title: ; notranslate" title="">function myFunction() {
    var myVariable = 10;

    console.log(myVariable); // 10
}

var myVariable = 5;

myFunction();

console.log(myVariable) // 5
</pre>

Variables declared in an outside scope can be overriden in a child scope. We&#8217;ve declared `myVariable` in the global scope and then declared it again in the scope of `myFunction`. We can see that `myVariable` has different values depending on which scope you&#8217;re in.

<pre class="brush: jscript; title: ; notranslate" title="">function myFunction() {
    var myVariable = 10;

    console.log(myVariable); // 10
}

myFunction();

console.log(myVariable) // ReferenceError
</pre>

Variables declared in an inner scope is not accessible by an outer scope.

## Closure

Closure is when a function remember its lexical scope even when it&#8217;s referenced and executed elsewhere and even after the outer function has returned.

<pre class="brush: jscript; title: ; notranslate" title="">function functionBuilder() {
    var count = 0;
    return function () {
        count++;
        console.log("Number of times this function has been called: " + count);
    }
}

var myFunction = functionBuilder();

myFunction(); // 1
myFunction(); // 2
myFunction(); // 3
</pre>

With the help of closures we can have private variables. Here we&#8217;ve created a function named `functionBuilder` which declares a variable named `count` and then returns a function which just increment `count` by one and logs to the console how many times it&#8217;s been called. The `count` variable is now inaccessible in the global scope. But our returned function still has access to it. This is what closure is all about.

<pre class="brush: jscript; title: ; notranslate" title="">var myModule = (function () {
    var count = 5;

    function printValue() {
        console.log("Value: " + count);
    }
    
    return {
        increment: function () {
            count++;
            printValue();
        },
        decrement: function () {
            count--;
            printValue();
        }
    };
} ());

myModule.increment(); // 6
myModule.decrement(); // 5
myModule.increment(); // 6

myModule.printValue(); // Won't work
myModule.count; // Won't work
</pre>

To use a modular design pattern we can make use of Immediately-invoked function expression(IIFE) which is a function that is executed immediately. Here we returned an object with public methods on it. The public methods then have access to our private state.

## Function scope

Before a function starts executing an `<code>executiion context`</code> is created and pushed to the stack. The `execution context` contains information about its lexical environment and which functions and variables that belongs to the scope. That means that every variable and function declaration is known before the function starts executing. Before ECMAScript 6 JavaScript only has function scope and not block scope like any C based language.

<pre class="brush: jscript; title: ; notranslate" title="">function functionScope() {
    if(true) {
        var a = 1;
    }
    
    console.log(a);
}

functionScope();
</pre>

This is valid code in JavaScript and it is called hoisting, meaning that every variable declared with the var statement will be lifted to the top of the function and available everywhere in the function even if it&#8217;s declared inside a block.

<pre class="brush: jscript; title: ; notranslate" title="">console.log(a); // undefined
var a = 1;
console.log(a); // 1

console.log(b); // ReferenceError
</pre>

It&#8217;s important to understand the difference between declaring and initializing a variable. Above we can see that `a` is accessible before the line declaring it but it&#8217;s not initialized which means it&#8217;s value is undefined. The last line shows us that forgetting to declare a variable gives us a `ReferenceError`. Because of hoisting it&#8217;s good practice to declare variables at the top of a function so that it&#8217;s clear that they are function scoped.

<pre class="brush: jscript; title: ; notranslate" title="">var functions = [];
for (var i=0;i&lt;3;i++) {
    functions[i] = function() {
        console.log(i);
    }
}

functions[0](); // 3
functions[1](); // 3
functions[2](); // 3
</pre>

Coming from a block scoped language can sometimes trick you to believe that the above code would print 1 2 3, but that is not correct because of function scope and closures. Closure is when a function remembers its lexical scope even after it&#8217;s referenced elsewhere and because of the variable called i is function scoped it&#8217;s value for each function in the array will be 3.

<pre class="brush: jscript; title: ; notranslate" title="">var functions = [];
for (var i = 0; i &lt; 3; i++) {
    functions[i] = (function (index) {
        return function () {
            console.log(index);
        }
    }(i));
}

functions[0](); // 0
functions[1](); // 1
functions[2](); // 2
</pre>

But we can solve this by creating a wrapping function which gives us a new function scope. Here we created an `IIFE` which takes `i` as a parameter and then returns our function. The reason that it works is beacuse it&#8217;s creating a new scope for our returned function in each iteration.

## Block scope

As of ECMAScript 6 you have the possibility to have block scope in JavaScript with the new `let` and `const` keyword. Because of how JavaScript declares variables `let` and `const` is also hoisted internally. But you will get a ReferenceError using it before the declare statement.

<pre class="brush: jscript; title: ; notranslate" title="">function test() {
    if (1 === 1) {
        let a = 1;
    }

    console.log(a); // ReferenceError
}

test();
</pre>

As we see above the variable called `a` declared with `let` is local to the if statement.

<pre class="brush: jscript; title: ; notranslate" title="">function test() {

    let a = 2;
    //let a = 3; // Throws an error. Already declared

    console.log(a); // 2
}

let a = 1;
test();
console.log(a); // 1
</pre>

The lexical aspect works the same as with `var` but you can&#8217;t redeclare a variable in the same block scope with `let`.

<pre class="brush: jscript; title: ; notranslate" title="">var functions = [];
for (let i=0;i&lt;3;i++) {
    functions[i] = function() {
        console.log(i);
    }
}

functions[0](); // 0
functions[1](); // 1
functions[2](); // 2
</pre>

With the `le`t keyword we don&#8217;t have to create a wrapping function to have a unique value of variable `i` in each iteration. This is because every iteration itself is a new block scope.

## Conclusion(tl;dr)

Lexical scope defines how variable names are resolved in nested functions.

Closure is the mechanism of a function remembering it&#8217;s lexical scope even when the functions is referenced elsewhere and even after the parent function has returned.

Before ECMAScript 6 JavaScript only has function scope meaning that every variable created with the `var` keyword is accessible in the entire function. It&#8217;s declared even before the row of the declaration but it&#8217;s value isn&#8217;t initialized. This is called hoisting.

In ECMAScript 6 we&#8217;ve got the `let` and `const` keywords that lets us declare variables block scoped.
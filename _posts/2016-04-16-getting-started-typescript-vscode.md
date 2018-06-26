---
id: 538
title: Getting started with TypeScript and Visual Studio Code
date: 2016-04-16T18:26:28+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=538
permalink: /getting-started-typescript-vscode/
image: /wp-content/uploads/2016/04/typescript_vscode.png
categories:
  - JavaScript
  - TypeScript
tags:
  - javascript
  - TypeScript
  - Visual Studio Code
  - vscode
---
## What&#8217;s TypeScript

Typescript is a typed superset of JavaScript that compiles down to plain JavaScript that works in any browser, host or operating system. TypeScript is open source and maintained by Microsoft.

TypeScript introduces features like:

  * Static typing
  * Interfaces
  * Generics

But also uses features from ES6:

  * Classes
  * Modules
  * The arrow syntax
  * Optional and default parameters

Larger JavaScript projects tends to get a bit messy without following design patterns. TypeScript tries to solve this problem by making it easier to write modular code and by introducing types in JavaScript.

## Setting up the environment

I assume that you have Visual Studio Code and `npm` already installed. We start by installing the TypeScript compiler(tsc).

<pre class="brush: bash; title: ; notranslate" title="">npm install -g typescript</pre>

Now lets create a file named `app.ts`. TypeScript files have the `.ts` extension and its&#8217;s in these files we will write our TypeScript code.

<pre class="brush: typescript; title: ; notranslate" title="">// app.ts
function sayHelloWorld(name) {
    console.log("Hello world " + name);
}

sayHelloWorld("Jonathan")
</pre>

As you can see this is just plain old JavaScript but it proves that you can mix TypeScript and JavaScript code. Now lets create `tsconfig.json` file. The `tsconfig` file is placed in the root directory of the TypeScript project. It specifies the files and the compiler options to compile the project. It is not mandatory to have this file but it&#8217;s simpler than specifying a whole bunch of command line arguments to `tsc`.

<pre class="brush: jscript; title: ; notranslate" title="">// tsconfig.js
{
    "compilerOptions": {
        "target": "es5",
        "removeComments": true,
        "sourceMap": true
    },
    "exclude": [
        "node_modules",
        "typings"
    ]
}
</pre>

To create a task in Visual Studio Code that will compile our TypeScript file press CTRL+SHIFT+P to access the command palette. Type &#8220;configure task runner&#8221; and press enter. You will now have a `task.json` file created under the folder `.vscode` in your root folder. It probably looks like this:

<pre class="brush: jscript; title: ; notranslate" title="">{
	// tasks.json
	"version": "0.1.0",
	"command": "tsc",
	"isShellCommand": true,
	"args": ["-w", "-p", "."],
	"showOutput": "silent",
	"isWatching": true,
	"problemMatcher": "$tsc-watch"
}
</pre>

If we look at the `args` array we have the `-w` parameter which tells the compiler to look for change files in our project so we don&#8217;t have to run our task manually. The `-p` parameter specifies which folder is the root of our project. The dot specifies the current directory.

Now press CTRL+SHIFT+B to start the task. It should have created a `app.js` file in the same directory. Create this simple html document and verify in the JavaScript console that it works.

<pre class="brush: xml; title: ; notranslate" title="">&lt;!doctype html&gt;
&lt;html&gt;

&lt;head&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;

&lt;body&gt;
&lt;/body&gt;

&lt;/html&gt;
</pre>

## Types

Lets look at types:

<pre class="brush: typescript; title: ; notranslate" title="">let any1: any = "asd";
let any2: any = 123;

let str1: string = "asd";
let str2 = "asd"; // Type inference

let number1: number = 123;

let bool1: Boolean = true;

let myFunction1: (message: string) =&gt; string = function (message: string) {
    return "hej";
}

let myFunction2 = function (message: string) { // Type inference
    return "hej";
}

let obj = { // Type inference
    property1: 123,
};
</pre>

I&#8217;m not going in to detail about every row but there&#8217;s two notable things. First we have the type any which can be any type of value. Second we have the object literal at the bottom. When an object is declared you cannot add properties later.

<pre class="brush: typescript; title: ; notranslate" title="">interface ISayHelloCallback {
    (message: string): void;
}

interface ISayHelloOptions {
    names: string[];
    alert?: boolean;
    callback: ISayHelloCallback;
}

function sayHello(options: ISayHelloOptions) {
    var msg = "Hello " + options.names.join(", ") + "!";
    
    if(options.alert) {
        alert(msg);
    } else {
        console.log(msg);
    }
    
    options.callback("Said hello!");
}

sayHello({
   names: [
       "Kalle",
       "Nisse"
   ],
   callback: (message: string) =&gt; {
       console.log(message);
   }
   
});
</pre>

You can set an interface as type parameter to a function so when you calling that function you need to specify an object with those properties. You can also set optional properties. Above i specified the `alert` property as an optional parameter. Interfaces can also be used for specifying function signatures.

## Type assertions

<pre class="brush: typescript; title: ; notranslate" title="">window.onload = () =&gt; {
    let input = &lt;HTMLInputElement&gt;document.getElementById("input1");
    input.value = "Tjena";
}
</pre>

Type assertions works like type casting in other languages. Sometimes you know more about the underlying type than TypeScript does. The return type of `document.getElementById` is `HTMLElement`. So we cast it to `HTMLInputElement` to get access to all the properties of an input element.

## Classes and inheritance

<pre class="brush: typescript; title: ; notranslate" title="">class BaseClass {
    public publicBaseProperty1: number;
    publicBaseProperty2: number; // default public
    protected protectedBaseProperty: number;
    private baseProperty3: number;

    constructor(someNumber: number) {
        this.baseProperty3 = someNumber
    }
    
    baseMethod() {
        console.log("hej");
    }
}

class ChildClass extends BaseClass {
    static childProperty1: number = 123;
    private privateChildProperty: number;
    
    constructor() {
        super(1);
        this.publicBaseProperty1 = 123;
        this.publicBaseProperty2 = 1234;
    }
    
    childMethod() {
        this.protectedBaseProperty = 555;
        super.baseMethod()
    }
    
    get childProperty1(): number {
        return this.privateChildProperty;
    }
    
    set childProperty1(value: number) {
        this.privateChildProperty = value;
    }
}
</pre>

Classes can inherit other classes and implement interfaces. Above we can se that TypeScript supports the common functionality of other object oriented languages.

## Loading external libraries

To get intellisense for external libraries like jQuery you need to get a TypeScript definition file. Go to <http://definitelytyped.org/>  and download the `.d.ts` file for jQuery. And then reference it in the top of your `.ts` file

<pre class="brush: jscript; title: ; notranslate" title="">/// &lt;reference path="typings/browser.d.ts" /&gt;

$(document).ready(function () {
    alert(1);
});
</pre>

Don&#8217;t forget to load the jQuery library either with a module loader or just include it on the web page.

## Moving on

I hope you&#8217;ve got something out of this and if you wan&#8217;t to learn more about TypeScript there&#8217;s a lot more documentation at the website <https://www.typescriptlang.org/>.
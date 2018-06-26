---
id: 674
title: Typescript ambient module declarations
date: 2017-09-24T21:48:21+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=674
permalink: /typescript-ambient-module-declarations/
categories:
  - JavaScript
  - TypeScript
tags:
  - TypeScript
---
With ES2015 JavaScript got the concept of modules using the export and import keywords. Typescript supports this and and it all works well as long as you are writing your modules in TypeScript. But to use external libraries or code that is not written in TypeScript you need to have a type declaration. For all major libraries these already exists in the definitelytyped repository which can be queried via <a href="https://microsoft.github.io/TypeSearch/" target="_blank" rel="noopener">TypeSearch</a>. But sometimes you&#8217;ll have to write one yourself.

I had some problems understanding how these declaration should be consumed and found the documentation a little bit tricky to understand which made me to write this.

## Ambient modules

From TypeScript documentation:

> We call declarations that don’t define an implementation “ambient”. Typically, these are defined in .d.ts files. If you’re familiar with C/C++, you can think of these as .h files

So to write type declarations for code that is already written in JavaScript we have to write an ambient module. As stated above these are almost always defined in a file ending with a `.d.ts` extension.

How can we define an ambient module ? There are two ways.

### Global declarations

When using `declare module` to create an ambient module the naming of the `.d.ts` file doesn&#8217;t matter what&#8217;s important is that the file is included in the compilation.

<pre class="brush: typescript; title: ; notranslate" title="">declare module "my-module" {
    export const a: number;
    export function b(paramA: number): void;
}
</pre>

When the file above is included in the compilation TypeScript will register that there&#8217;s a module named `my-module` which then can be imported

<pre class="brush: typescript; title: ; notranslate" title="">import { a, b } from "my-module"
</pre>

There are different ways to include declaration files in the compilation

  1. Specify path in the `typeRoots` compilerOptions in `tsconfig.`All global declarations files under typeRoots will be automatically included. This can be controlled with the `types` property compilerOption where you can explicitly control which definitions should be automatically included
  2. Specify the files property in `tsconfig` so that the declaration file is included
  3. Use the tripple slash directive `/// <reference path="..." />`
  4. With the help of paths compilerOptions in `tsconfig`

From TypeScript documentation

> Keep in mind that automatic inclusion is only important if you’re using files with global declarations (as opposed to files declared as modules). If you use an import &#8220;foo&#8221; statement, for instance, TypeScript may still look through node\_modules & node\_modules/@types folders to find the foo package.

### Files declared as modules

Using top-level export and import module declarations the `.d.ts` does not need to be included in the compilation. The important thing here is that the file is named `index.d.ts` and resides in a folder named after the module which in this case is `my-module`

<pre class="brush: typescript; title: ; notranslate" title="">// index.d.ts
export const a: number;
export function b(paramA: number): void;

// In a file importing the library
import { a, b } from "my-module"
</pre>

What TypeScript will do is by default to try and lookup `my-module`. It will try with a number of different steps looking for both code(ts files) and declarations(.d.ts files). One of the steps is to look for declaration files in `node_modules/@types`. It will look for a folder named like the imported module with an index.d.ts file looking like the one above.

Sometimes you don&#8217;t wan&#8217;t to publish declaration files to definitelytyped and have folder with custom type declarations and therefore inform TypeScript to look for declarations in other folder than node_modules/@types. This can be done with the help of `compilerOption` `paths`.

{
      
"baseUrl": ".",
      
"paths": {
          
"*": [
              
"custom-typings/*"
          
]
      
}
  
}

With this configuration in `tsconfig.json` TypeScript will look for code and declaration files in the `custom-typings` folder.

To verify where TypeScript is trying to resolve things you can run the compiler with the `traceresolution` flag

<pre class="brush: bash; title: ; notranslate" title="">tsc --traceresolution
</pre>
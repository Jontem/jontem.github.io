---
id: 470
title: 'JavaScript series: The ThisBinding'
date: 2016-02-21T20:04:18+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=470
permalink: /javascript-series-thisbinding/
image: /wp-content/uploads/2016/02/javascript-825x200.png
categories:
  - JavaScript
  - JavaScript series
tags:
  - javascript
  - this
  - this keyword
---
The `this` keyword in Javascript can be confusing even for experienced developers. What this is a reference to depends on how the function was called. It&#8217;s a runtime binding that has nothing to do with where the function was declared. The `ThisBinding` makes it possible to use a function in multiple contexts rather than creating multiple versions of it.

To find out what the `this` keyword will be a reference to you need to find the call-site of the function. In other words where the function was called either by looking at your code or through a debugger. Then you&#8217;ll have to inspect the call-site to determine which of the four rules explained below that applies.

## The new binding

The first rule with the highest precedence is the new binding.

<pre class="brush: jscript; title: ; notranslate" title="">function Person(name) {
	this.name = name;
}

var personObj = new Person("Jonathan");
console.log(personObj.name); // Jonathan
</pre>

When the `new` operator is put in front of a function call it creates a new object and assigns it&#8217;s reference to the `this` keyword.  The newly created object also gets it&#8217;s prototype to reference Person.prototype which in this case is an empty object. To learn more about the `prototype` read the post Prototype-based inheritance of my Javascript series.

If the function does not explicitly return an object the object `this` is referencing will be returned. Should the function return anything else that is not an object that return value will be ignored and the object referenced by `this` will be returned.

## Explicit binding

The explicit binding forces `this` to reference a specified object.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHi() {
	console.log("Hi " + this.name );
}

var person1 = {
	name: "Jonathan"
};

var person2 = {
	name: "Kalle"
};

sayHi.call(person1); // Hi Jonathan
sayHi.apply(person2); // Hi Kalle
</pre>

With `.call` and `.apply` we can set `this` to reference the object we want on any function call(except functions that is using hard binding).

> Function.prototype.apply(thisArg, [argsArray])
  
> Function.prototype.call(thisArg[, arg1[, arg2[, &#8230;]]])

The `call` and `apply` function is inherited from Function.prototype. They achieve the same goal but with different signatures. The `apply` function takes an array as it&#8217;s second parameter which will be the arguments for the function invoked. The `call` function requires the arguments to be listed explicitly as parameters.

### Hard binding

Sometimes we don&#8217;t want the caller to change the thisBinding. We can then use hard binding.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHello() {
	console.log(this.name);
}

var person1 = {
	name: "Jonathan"
};

var person2 = {
	name: "Kalle"
};

var origSayHello = sayHello;
sayHello = function() {
	origSayHello.call(person2);
};

sayHello.call(person1); // Kalle
</pre>

By saving a reference to `sayHello` and then creating a wrapper function which always runs the original function with `.call` and our specific object we&#8217;re throwing away whatever the `sayHello.call` specifies. This way we can always be sure that the `this` keyword is referencing what we want.

In ECMAScript 5 we have the built in function `bind` that is specified on `Function.prototype` to help us with hard binding.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHello() {
	console.log(this.name);
}

var person1 = {
	name: "Jonathan"
};

var name = "Kalle";

setTimeout(sayHello, 1000); // undefined
setTimeout(sayHello.bind(person1), 1000); // Jonathan
</pre>

The `setTimeout` function will have it&#8217;s `thisBinding` pointing at the global/window object which is the last of the four rules(Default binding, explained below). But with hard binding we force the `thisBinding` to point the object we want.

## Implicit binding

The third rule is the implicit binding. The rules states that the containing object is referenced by the `this` keyword when a function is called.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHello() {
	console.log(this.name);
}

var person1 = {
	name: "Jonathan",
	sayHi: sayHello
};

var person2 = {
	name: "Kalle",
	sayHi: sayHello
};

person1.sayHi(); // Jonathan
person2.sayHi(); // Kalle
</pre>

As we can see above it doesn&#8217;t matter were the function was declared only how it was called. So when `sayHi` was called with `person1.sayHi` the `this` keyword is referencing `person1` and on the line under it&#8217;s referencing `person2`.

<pre class="brush: jscript; title: ; notranslate" title="">var personUtilities = {
	sayHello: function() {
		console.log(this.name)
	}
};

var person1 = Object.create(personUtilities);
person1.name = "Jonathan";


var person2 = Object.create(personUtilities);
person2.name = "Kalle";

person1.sayHello(); // Jonathan
person2.sayHello(); // Kalle
</pre>

The same is true for functions in the prototype chain no matter how high up they&#8217;re defined.

The implicit binding is all about finding the nearest object and then you&#8217;ll know what `this` is a reference to.

## Default binding

The last catch-all rule is the default binding.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHello() {
	console.log(this.name);
	console.log(this === window)
}

var name = "Jonathan";

sayHello(); // Jonathan, true
</pre>

If none of the other three rules above matches the default binding kicks in. The example above shows us that `this` is referencing the global/window object in the `sayHello` function.

<pre class="brush: jscript; title: ; notranslate" title="">function sayHello() {
	"use strict";
	console.log(this === undefined)
}

var name = "Jonathan";

sayHello(); // undefined
</pre>

If `strict mode` is specified inside the function the global/window object is not eligible for the default binding. If the call-site is running in `strict mode` but not the executed function `this` will reference the global/window object.

<pre class="brush: jscript; title: ; notranslate" title="">var person = {
	name: "Jonathan",
	sayHello: function() {
		console.log(this.name);
	}
};

var sayHi = person.sayHello;


sayHi(); // undefined
</pre>

Remember that it&#8217;s all about the call-site. In the above code we created a variable `sayHi` and referenced `person.sayHello`. Then we called `sayHi` without an object in front of it so the default binding rule applies and in this case the global/window object doesn&#8217;t have variable called `name` and therefore we get an `undefined` error.

## Arrow functions

Until arrow functions was introduced in ECMAScript 6 every function defined it&#8217;s own `this` reference. Arrow functions lexically binds `this`. Meaning that `this` will be the same as in it&#8217;s enclosing context.

<pre class="brush: jscript; title: ; notranslate" title="">function Person(name) {
	this.name = name;

	setTimeout(function() {
		console.log(this.name); // undefined
		console.log(this === window); // true
	}, 1000);
}

var person = new Person("Jonathan");
</pre>

Without a normal callback function the default binding will apply and `this` will be the global/window object.

<pre class="brush: jscript; title: ; notranslate" title="">function Person(name) {
	this.name = name;

	setTimeout(() =&gt; {
		console.log(this.name); // Jonathan
	}, 1000);
}

var person = new Person("Jonathan");
</pre>

But with a arrow function `this` will be the same as in the enclosing context.

<pre class="brush: jscript; title: ; notranslate" title="">setTimeout(() =&gt; {
	"use strict";
	console.log(this === window); // true
}, 1000);	
</pre>

In the above code `this` is referencing the window object even though were using strict mode because `this` is lexically bound.

## Conclusion(tl;dr)

The `this` keyword in JavaScript can be confusing but when you learn the four binding rules it&#8217;s quite straightforward. It&#8217;s all about how and where the function was called.

We have the new binding which happens when the `new` keyword is put in front of any function. It creates an empty object and binds `this` to it inside that function.

The explicit binding with `.call` and `.apply` which makes it possible to call a function with a specified object as `this`.

The implicit binding that states that it doesn&#8217;t matter where a function was declared only where it was called. Find the call-site and on which object the function was called. If `obj.sayHello()` was called, then inside `sayHello` `this` would point to `obj`.

The default binding which is the catch-all rule binds `this` to the global/window object(in strict mode it will be undefined) when none of the other three rules apply.

Arrow functions introduced in ECMAScript 6 is the exception to these rules above. It uses a lexical `this` which means that `this` will be the same as `this` in the enclosing context.
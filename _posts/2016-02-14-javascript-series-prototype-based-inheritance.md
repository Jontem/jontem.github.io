---
id: 436
title: 'JavaScript series: Prototype-based inheritance'
date: 2016-02-14T11:06:22+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=436
permalink: /javascript-series-prototype-based-inheritance/
image: /wp-content/uploads/2016/02/javascript-825x200.png
categories:
  - JavaScript
  - JavaScript series
tags:
  - javascript
  - Object.create
  - Prototype
  - Protype-based inheritance
---
JavaScript does not have classes so when we&#8217;re talking about prototypes try not to think about classes. This can be very confusing for developers who come from a class oriented language like C# or java. JavaScript is truly an object oriented language. All objects in JavaScript are descended from the &#8220;root object&#8221; called `Object`. The prototype itself is an object that exists as a property on all objects and it&#8217;s what&#8217;s used to implement prototype-based inheritance and shared properties. By default a newly created object has a prototype that is a reference to `Object.prototype`.

## The prototype chain

Let&#8217;s create a new object.

<pre class="brush: jscript; title: ; notranslate" title="">var obj = {
	myFunction() {
		console.log(1);
	}
};


console.log(obj); // { myFunction: [Function: myFunction] }

console.log(obj.toString()) // [object Object]

console.log(obj.hasOwnProperty("myFunction")); // true

console.log(obj.hasOwnProperty("toString")); // false
</pre>

First we created an object called `obj` with the object literal syntax. When we log the object to the console we can see that it has a function named `myFunction`. But somehow `obj.toString()` references a function that prints `[object Object]`. I&#8217;ve then used a function called `hasOwnProperty` which is also not defined on `obj` which prints true for `myFunction `but prints false for `hasOwnProperty`. So where does these functions come from now that we can that they&#8217;re clearly not defined on `obj`.

This is where the JavaScript prototype object comes in. When calling `obj.toString()` and `obj` does not have that property it will try and go to it&#8217;s prototype and look for it. In our case obj&#8217;s prototype will be a reference to `Object.prototype` which is the default for created objects. When it came to `Object.prototype` it found a property named toString and used that. This is how the prototype works. JavaScript will traverse up the prototype chain until it finds the property it&#8217;s looking for or if the prototype is `null` which the prototype of `Object.prototype` is.

So what do the `hasOwnProperty()` function tell us. This description is from [developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)

> Returns a boolean indicating whether an object contains the specified property as a direct property of that object and not inherited through the prototype chain.

This means that obj doesn&#8217;t own the `toString` function and is only accessible because of how the prototype works.

## The new keyword and creating a prototype

<pre class="brush: jscript; title: ; notranslate" title="">var obj1 = {
	aFunction: function() {
		return 1;
	},
	aProperty: 123,
}

function Obj2() {

}

Obj2.prototype = obj1 // Assign obj1 as the prototype of Obj2

var obj2 = new Obj2(); // Create a new empty object and set it's prototype to reference Obj2.prototype

console.log(obj2); // {}

console.log(obj2.aFunction()); // 1
console.log(obj2.aProperty); // 123

console.log(Object.getPrototypeOf(obj2) === Obj2.prototype); // true
console.log(Object.getPrototypeOf(obj2) === obj1); // true
console.log(Object.getPrototypeOf(obj2).__proto__ === Object.prototype); // true
</pre>

In the above code we created an object called `obj1` with a function and a property with an integer value. Then we created a function object called `Obj2` and sat it&#8217;s prototype to reference object `obj1`. On line fourteen we used the `new` keyword in front of the `Obj2` function call. This is where it can get confusing coming from a class oriented language. When the `new` keyword is put in front of a function call it creates an empty object and assigns it to the `this` keyword it then magically returns the new object. In this case it sets it&#8217;s internal prototype reference to `Obj2.prototype` which in turn references the `obj1` object. The `obj1` objects internal prototype references `Object.prototype`. So now we have access to properties and functions in both object `obj1` and `Object.prototype` through the prototype chain.

The `Object.getPrototypeOf` was standardized in ECMAScript 5 and gets the prototype of an object. The `__proto__` property pronounced &#8220;dunder proto&#8221; is standardized in ECMAScript 6 but exists in most browsers today but it&#8217;s behavior is not standardized in earlier versions of ECMAScript. If you only want to read the prototype, `Object.getPrototypeOf` and `__proto__` achieves the same thing.

It&#8217;s important to understand that JavaScript does not make copies of prototypes when a new object is created. Prototypes are only references to other objects.

<pre class="brush: jscript; title: ; notranslate" title="">function Obj1() {
	this.sayHello = function() {
		console.log("Obj1 said hello");		
	}
}

Obj1.prototype.sayHello = function() {
	console.log("Obj1 prototype said hello");
}

var obj1 = new Obj1();

obj1.sayHello(); // Obj1 said hello
Object.getPrototypeOf(obj1).sayHello(); // Obj1 prototype said hello
console.log(Object.getPrototypeOf(obj1) === Obj1.prototype) // true
console.log(obj1.hasOwnProperty("sayHello")); // true
</pre>

First let&#8217;s repeat again what the `new` keyword does. It creates an empty object and assigns it to the `this` keyword. The prototype of the newly created object gets linked to the prototype of the function which was called with new keyword in front of it and then returns the object. Here we created a function called `sayHello` on the newly created object which is then assigned to the object called `a`. We also created a function called `sayHello` on the prototype object of `A`. What&#8217;s happening now when we call `a.sayHello()` is that it finds a function named `sayHello` directly on object `a` and will therefore not traverse the prototype chain. This is called shadowing. We can still access our prototypes version of sayHello by referencing the prototype object.

## Object.create vs new

The `object.create` function is available since ES5. This is what the ECMAScript specification says about the Object.create function:

**Object.create ( O [ , Properties ] )**

> The create function creates a new object with a specified prototype

<pre class="brush: jscript; title: ; notranslate" title="">function Obj1() {
    this.sayHello = function() {
        console.log("Obj1 said hello");
    }
}
 
Obj1.prototype.sayHello = function() {
    console.log("Obj1 prototype said hello");
}
 
var obj1WithNew = new Obj1();
var obj1WithCreate = Object.create(Obj1.prototype);
 
obj1WithNew.sayHello(); // Obj1 said hello
console.log(obj1WithNew.hasOwnProperty("sayHello")); // true
obj1WithCreate.sayHello(); // Obj1 prototype said hello
console.log(obj1WithCreate.hasOwnProperty("sayHello")); // false
</pre>

Here we can see that when using the `new` keyword we always have to run the constructor call to create a new object. But with `Object.create` we get an empty object without making a constructor call. The `object1WithCreate` object does not have a property named `sayHello` which the ECMAScript specification explained above.  To further explain `Object.create` let&#8217;s look at this simple polyfill without the second parameter.

<pre class="brush: jscript; title: ; notranslate" title="">Object.create = function(o) {
	function F() {}
	F.prototype = o;

	return new F(); 
};
</pre>

The polyfill creates an empty object with a constructor call that does nothing and assigns the `prototype` to the passed in object before returning it.

As we can see `Object.create` makes inheritance in JavaScript simpler.

## Conclusion(tl;dr)

JavaScript is an object-oriented language with prototype-based inheritance. When a property is not found on the current object it will traverse the prototype chain until it finds the property or reaches the top(when that prototype objects prototype is null).

The `new` keyword can be placed in front of any function call which makes JavaScript create an empty object and sets it&#8217;s prototype to reference the called functions prototype.

Shadowing is when a property is specified further down the prototype chain which hides a property with the same name higher up.

`Object.create` is used to create an empty object with a specified prototype without making a constructor call.
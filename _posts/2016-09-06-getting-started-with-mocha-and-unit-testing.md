---
id: 573
title: Getting started with Mocha and unit testing
date: 2016-09-06T18:44:10+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=573
permalink: /getting-started-with-mocha-and-unit-testing/
image: /wp-content/uploads/2016/09/xFVFxOioAU-e1473180066136.png
categories:
  - JavaScript
  - Testing
tags:
  - javascript
  - Mocha
---
## Mocha

Mocha is a JavaScript test framework running on NodeJS and in the browser. It&#8217;s feature rich and has great support for asynchronous testing. It also gives you the possibility to use the assertion library of choice.

A selection of features

  * Async support
  * Test coverage reporting
  * Javascript API for running tests
  * file watcher support
  * Highlights slow tests
  * Transpiler support

## Run our first test

Getting started with mocha is easy.

<pre class="brush: bash; title: ; notranslate" title="">npm install --global mocha
npm install --save-dev mocha
mkdir test
</pre>

Lets add our first test
  
`test/first_test.js`

<pre class="brush: jscript; title: ; notranslate" title="">var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
</pre>

Now we just run mocha

<pre class="brush: bash; title: ; notranslate" title="">mocha
</pre>

You should be getting an output like this.
  
<img class="size-full wp-image-579" src="https://www.mourtada.se/wp-content/uploads/2016/09/Screen-Shot-2016-09-04-at-16.00.27.png" alt="mocha test results" width="402" height="120" />

Well that wasn&#8217;t too complicated. So what happened.

First we used a built in node module called assert. Basically it just evaluates an expression and throws and exception if the result is false. Most times you would use a more advanced assert library like [ChaiJS](http://chaijs.com/).

Then we used the `describe` function which takes a string and a callback. It&#8217;s used for grouping tests and as seen above you can nest `describe` functions.

The `It` function takes a string and callback which will make one test case. We then assert the result which mocha displays in it&#8217;s test reporter.

## Specifying tests to run

When we run mocha it looks for test with this glob pattern test/*.js.

If we want to look for tests recursively

<pre class="brush: bash; title: ; notranslate" title="">mocha --recursive
</pre>

Or if you want to specify another directory for your tests

<pre class="brush: bash; title: ; notranslate" title="">mocha -- path/to/tests/*.js
</pre>

And if you don&#8217;t wan&#8217;t to re run your tests you can tell mocha to watch your test files for changes.

<pre class="brush: bash; title: ; notranslate" title="">mocha --watch
</pre>

To see all parameters just run `mocha --help`. In my osx terminal i had this strange issue that the help output was cut off so i only got a third of the output. Upgrading node from 6.2.0 to 6.5.0 solved that problem.

## Using the &#8220;fat arrow&#8221;(arrow functions)

You can use arrow functions with Mocha  to specify callbacks to `describe` and `It`. But because of the lexical binding of `this` it&#8217;s discourage because you wont be able to access Mocha context. With the mocha context you can specify how the tests should behave for example `this.skip()` and `this.slow(1000)`. The `skip` function explains itself and the `slow` function defines a threshold for mocha to mark this test as slow which the test reporter will notify you about.

## Testing async code

Lets add a new test.
  
`test/async_test.js`

<pre class="brush: jscript; title: ; notranslate" title="">var assert = require('assert');
describe('Async', function () {

  describe('callbacks', function () {
    it('should return 123 after 3 seconds', function (done) {
      this.timeout(4000);
      asyncWithCallback(function (data) {
        assert.equal(data, 123);
        done();
      })
    });
  });

  describe('promises', function () {
    it('should return 567 after 1 second', function () {
      return asyncWithPromise().then((data) =&gt; {
        assert.equal(data, 567);
      });
    });
  });

});

function asyncWithCallback(callback) {
  setTimeout(() =&gt; {
    callback(123);
  }, 3000);
}

function asyncWithPromise() {
  return new Promise((resolve) =&gt; {
    setTimeout(() =&gt; {
      resolve(567);
    }, 1000);
  });
}
</pre>

With mocha we can make use of the done callback to specify when asynchronous tests are done or we can return a promise which mocha will wait for being resolved. Mocha has a default timeout of 2000ms so if a test is taking longer you need to use the mocha context to set a higher timeout.

## Conclusion

It&#8217;s really simple and fast to get mocha up and running and write simple unit tests. Head over to the [Mocha website](http://mochajs.org/) fore more detailed information.
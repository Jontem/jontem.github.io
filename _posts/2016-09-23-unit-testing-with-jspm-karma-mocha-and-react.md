---
id: 607
title: Unit testing with jspm, karma, mocha and react
date: 2016-09-23T22:03:47+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=607
permalink: /unit-testing-with-jspm-karma-mocha-and-react/
image: /wp-content/uploads/2016/09/logo_og-825x510.png
categories:
  - JavaScript
  - Testing
tags:
  - jspm
  - karma
  - Mocha
  - react
---
Javascript biggest downside is all the dependencies you need to bring in and configuration needed to get something similar as unit testing going. Imagine your psyched for this idea of a code project you just had and just wan&#8217;t to get started coding. But after a few nights of your precious spare time you almost give up because you even haven&#8217;t started coding yet. All this packages and configuration is killing your motivation. Javascript fatigue&#8230; Enough ranting&#8230;

I&#8217;ve been struggling for some days now to get unit testing with react in a project using jspm. I&#8217;m documenting this to hopefully save future me and someone else the hours and frustration.

## Jspm

Jspm is a package manager for Javascript that aims to solve the problem of different module formats in JavaScript(ES6, AMD, CommonJS, and globals). Jspm uses the SystemJS module loader which handles all this cases and also can handle registries other than npm like github.

When developing your Javascript application jspm uses a transpiler of your choice to transpile your next generation Javascript code, TypeScript or whatever needs transpiling directly in the browser. When going to production you can produce a bundle.

## Karma

Karma is a test runner which runs your tests in a browser or even headless browsers like PhantomJS.  It spawns a web server that executes the test against each browser which then displays the results in the command line. It&#8217;s primary usage is for unit testing and not end to end testing.

Karma can watch specified files and run the tests whenever something changes. It also has the functionality to load preprocessors which means that your javascript files can be processed before getting served to the browser. One example could be running the files through babel or webpack.

It&#8217;s testing framework agnostic and there&#8217;s support for every popular testing framework out there like Mocha, QUnit and Jasmine and so on. If it doesn&#8217;t you can build your own adapter.

## Mocha

My previous post was about unit testing with mocha. [Read it here](https://www.mourtada.se/getting-started-with-mocha-and-unit-testing/).

## React

React is Javascript library for building user interfaces. Enough of that. Let&#8217;s get started.

## Installation

I&#8217;ve already created [jspm-react-boilerplace](https://github.com/Jontem/jspm-react-boilerplate). I will use that as a starting point. So start by cloning the github repo and install the packages.

<pre class="brush: bash; title: ; notranslate" title="">$ git clone https://github.com/Jontem/jspm-react-boilerplate.git
$ npm install
$ jspm install
</pre>

After the 1000 dependencies have finished installing we need to install karma and mocha along with some required karma plugins.

<pre class="brush: bash; title: ; notranslate" title="">$ npm install --save-dev karma karma-chrome-launcher karma-jspm karma-mocha mocha
</pre>

After another 1000 packages is downloaded we can run.

<pre class="brush: bash; title: ; notranslate" title="">$ ./node_modules/.bin/karma init
</pre>

Just choose mocha as the testing framework and Chrome for browser then choose the defaults. We will edit the config file directly. If you don&#8217;t want to specify the path to karma you can install it globally. But after we add an npm script we don&#8217;t need to worry about that.

Open `karma.conf.js` and find the frameworks property and add jspm.

<pre class="brush: jscript; title: ; notranslate" title="">frameworks: [
      'jspm',
      'mocha'
    ]
</pre>

After frameworks add a jspm configuration section

<pre class="brush: jscript; title: ; notranslate" title="">jspm: {
      loadFiles: [
        'test/**/*.js',
        'src/**/*.js'
      ]
    }
</pre>

This will make jspm run a transpiler on the files specified before handing them over to mocha.

The last thing we need to add to the karma cofig is the proxies property and this is because karma loads our specified files under the base url `/base/`. We can tell the karma webserver that som paths should automatically be proxied to `/base/`

<pre class="brush: jscript; title: ; notranslate" title="">proxies: {
      '/test/': '/base/test/',
      '/src/': '/base/src/',
      '/jspm_packages/': '/base/jspm_packages/'
    }
</pre>

Let&#8217;s install two more packages&#8230; `Chai` which is an assertion library and `react-addons-test-utils`

<pre class="brush: bash; title: ; notranslate" title="">$ jspm install --dev npm:chai npm:react-addons-test-utils
</pre>

Now edit package.json so we can run `npm test`. The script property should have these entries.

<pre class="brush: jscript; title: ; notranslate" title="">"scripts": {
    "start": "npm run webserver & npm run watch",
    "webserver": "node server.js",
    "watch": "chokidar-socket-emitter",
    "test": "karma start"
  }
</pre>

Running `npm start` launches a browser that will execute the tests. But it will fail because it won&#8217;t find any tests. So lets add one. We start by creating the `test/` and `src/` directories.

Create a file `hello_world_component.test.js` under the `test/` directory.

<pre class="brush: jscript; title: ; notranslate" title="">import {assert} from "chai";
import TestUtils from "react-addons-test-utils";
import * as React from "react";
import DOM from "react-dom";
import {HelloWorld} from "../src/hello_world_component";

describe("Hello world component", function () {

    it("should should display the text hello world", function () {
        var renderer = TestUtils.createRenderer();
        const result = renderer.render(&lt;HelloWorld /&gt;);

        assert.equal(result.type, "div");
        assert.deepEqual(result.props.children, [
            &lt;h1&gt;Hello world&lt;/h1&gt;,
            &lt;span&gt;test&lt;/span&gt;
        ]);
    });

});
</pre>

Next we create the file which defines the component `hello_world_component.js` under `src/`

<pre class="brush: jscript; title: ; notranslate" title="">import * as React from "react";

export const HelloWorld = React.createClass({
    render() {
        return (
            &lt;div&gt;
                &lt;h1&gt;Hello world&lt;/h1&gt;
                &lt;span&gt;test&lt;/span&gt;
            &lt;/div&gt;
        );
    }
});
</pre>

Now run `npm test` and you should have one green test!

The benefit of using karma is that we&#8217;re running our tests in a browser environment which means we have access to `DOM` which some of React test utils needs for simulating DOM events or rendering deeper hierarchies of react components. In our tests above we make use of shallow rendering which only renders react components one level deep and doesn&#8217;t need access to a `DOM`. Read more about React test utils on [reacts website](https://facebook.github.io/react/docs/test-utils.html).

If you think this post was long you don&#8217;t have any idea of the struggle to get this working :). Oh one last thing&#8230; If you want to use the `renderIntoDocument` method be aware that stateless components cannot be tested. It will only return null, I don&#8217;t know why&#8230; But it was hidden somewhere in the documentation.
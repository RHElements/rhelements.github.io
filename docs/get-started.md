# Get Started

## Prerequisites

The easiest way to get started is install `generator-rhelement` to scaffold out
a new RHElement. The generator requires that you have [Yeoman](http://yeoman.io/) installed. In addition to Yeoman, you'll want to install [Polyserve](https://github.com/Polymer/polyserve) for local development and [Web Component Tester](https://github.com/Polymer/web-component-tester) globally for running tests on your element.

```
npm install -g yo polyserve web-component-tester
```

Once you have Yeoman and Web Component Tester installed, install the RHElement generator.

```
npm install -g generator-rhelement
```

## Generate a RHElement

Using the generator-rhelement, the generator will ask you a few questions that will help with the scaffolding.

```
yo rhelement
```

## Scaffolding Structure

The generator will scaffold out a new RHElement that will include a ES6 module version of your element as well as a version compiled to ES5 code. These two files will live at the root of your new element. **DO NOT EDIT THESE TWO FILES**. These two files are the files that will be used when your element is distributed and they'll be overwritten during development and your build.

Instead, do your development in the `/src` directory of your element. In the `/src` directory you'll find a Javascript file that extends the Rhelement class that takes care of setting up a shadow root and ShadyCSS. The HTML file is where you'll add the HTML that will be cloned into the shadow root. And the CSS or SCSS file (depending on if you're using Sass) is where you'll add your styles. During the development and build tasks, a Gulp task will merge these three files together into the root of your element and will update the ES6 and ES5 versions.

## Develop

Run the start script in the package.json file at the root of your element and Gulp will start watching the files in your `/src` directory and will run a build each time you edit one of those files. The start script will also open a browser to the demo page of the element.

```
npm start
```

## Test

Run the test script in the package.json file and Web Component Tester will use Mocha and Chai to execute your tests in the browser using both Chrome and Firefox. Be sure to add your tests to the \*\_test.html file in the `/test` directory. We've also added a .travis.yml file with everything that's needed to run your tests on [Travis CI](https://travis-ci.org/).

```
npm test
```

## Build

Prepare your element for distribution by running the build script in the package.json file. If you've been running `npm start`, the start script runs the build script every time you save a file in the `/src` directory so running the build script might be redundant, but better safe than sorry.

```
npm run build
```

The build script will merge the files in the `/src` directory and update the ES6 and ES5 versions of your element in the root of the element. These two files are the files that your applications will either require or import for use.

## Publish

We've been publishing our RHElements to npm under the [RHElements organization](https://www.npmjs.com/org/rhelements), but you should feel free to update the package.json file and change the name of your element to point to an organization of your choice.

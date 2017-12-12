# Step 3: Test

To test the `rh-cool-element` that we've been working on, we'll rely on a few tools to help us ensure that the element we've built can be relied on in production. The first tool we'll use is [Web Component Tester](https://github.com/Polymer/web-component-tester) which was built and is maintained by the Polymer team. Web Component Tester makes testing pretty easy since we'll just need to add our HTML to a file and then set up our suite of tests. Under the hood, Web Component tester uses Mocha and Chai so we'll be using the [Chai Assertion Library](http://chaijs.com/api/assert/) to make sure our tests pass. Finally, we'll show how easy it is to integrate these tests with [Travis CI](https://travis-ci.org).

## Web Component Tester

If you've used followed the [Prerequisites]({{ "/docs/get-started.html#prerequisites" | relative_url }}) in [Getting Started]({{ "/docs/get-started.html" | relative_url }}), the setup should already be done. If you didn't, make sure you have wct installed globally `npm install -g wct`, add `wct-browser-legacy` as a dev dependency in your `package.json` file, and add  a `test` script in the scripts section of your `package.json` file - `"test": "wct --npm"`.

### Test Setup

In the root of our element, we have a `/test` directory that has an `index.html` file and a `rh-cool-element_test.html` file. The `index.html` file tells Web Component Tester which files should be tested, in this case `rh-cool-element_test.html`.

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
    <script src="../../../@webcomponents/webcomponentsjs/webcomponents-lite.js"></script>
    <script src="../../../wct-browser-legacy/browser.js"></script>
  </head>
  <body>
    <script>
      // Load and run all tests (.html, .js):
      WCT.loadSuites([
        'rh-cool-element_test.html'
      ]);
    </script>

</body></html>
```

In our `/test/index.html` file we've included the web component polyfill and `wct-browser-legacy/browser.js`. The reason for `wct-browser-legacy/browser.js` is that, by default, Web Component Tester is looking for our web components to be loaded from a `bower_components` directory and since our web components are installed via npm and served from a `node_modules` directory, we need to use `wct-browser-legacy` (Reference to the [Node support section of the Web Component Tester README](https://github.com/Polymer/web-component-tester#node-support)).

The setup for our `/test/rh-cool-element_test.html` file is pretty basic too. I've added four stubs for the functionality that we need to test.

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
    <script src="../../../@webcomponents/webcomponentsjs/webcomponents-lite.js"></script>
    <script src="../../../wct-browser-legacy/browser.js"></script>
    <script type="module" src="../rh-cool-element.js"></script>
  </head>
  <body>

    <rh-cool-element photo-url="https://avatars2.githubusercontent.com/u/330256?s=400&u=de56919e816dc9f821469c2f86174f29141a896e&v=4">
      Kyle Buchanan
    </rh-cool-element>

    <script>
      suite('<rh-cool-element>', () => {
        test('it should set a username from the light DOM', () => {

        });

        test('it should allow a user to follow a profile', () => {

        });

        test('it should set the state to follow if the following attribute is present', () => {

        });

        test('it should set a profile pic from the photo-url attribute', () => {

        });
      });
    </script>
  </body>
</html>
```

A couple of things to note here is that we've used `<script type="module"...` to load our element definition. We're doing this because we want to make sure that we're testing the true source of our element instead of the transpiled version. Second, notice that we just add the HTML we need to set up our tests. This is the same HTML from our `/demo/index.html` file.

Lastly, we have one additional file that helps with testing. In the root of our element, if you used our generator, there is a `wct.conf.json` file. We use this file to tell Web Component Tester that we want to use Chrome and Firefox for our testing.

```
{
  "verbose": false,
  "plugins": {
    "local": {
      "browsers": ["chrome", "firefox"]
    }
  }
}
```

Now that our setup is complete, we can start building out our tests.

### Test Cases

Testing our 'rh-cool-element' is pretty straight-forward. We use `document.querySelector` to grab our element and then we use the usual DOM API methods to get at the things we want to test. Here is how our final test code looks.

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
    <script src="../../../@webcomponents/webcomponentsjs/webcomponents-lite.js"></script>
    <script src="../../../wct-browser-legacy/browser.js"></script>
    <script type="module" src="../rh-cool-element.js"></script>
  </head>
  <body>

    <rh-cool-element photo-url="https://avatars2.githubusercontent.com/u/330256?s=400&u=de56919e816dc9f821469c2f86174f29141a896e&v=4">
      Kyle Buchanan
    </rh-cool-element>

    <script>
      suite('<rh-cool-element>', () => {
        test('it should set a username from the light DOM', () => {
          const element = document.querySelector('rh-cool-element');
          const elementLightDOMContent = element.textContent.trim();
          const shadowRoot = element.shadowRoot;
          const slotContent = shadowRoot.querySelector('slot').assignedNodes()[0].textContent.trim();

          assert.equal(slotContent, elementLightDOMContent);
        });

        test('it should allow a user to follow a profile', () => {
          const element = document.querySelector('rh-cool-element');
          element.button.click();

          assert.isTrue(element.hasAttribute('following'));
          assert.equal(element.button.textContent, 'Unfollow');

          element.button.click();

          assert.isNotTrue(element.hasAttribute('following'));
          assert.equal(element.button.textContent, 'Follow');
        });

        test('it should set the state to follow if the following attribute is present', () => {
          const element = document.querySelector('rh-cool-element');
          element.setAttribute('following', '');

          assert.isTrue(element.hasAttribute('following'));
          assert.equal(element.button.textContent, 'Unfollow');

          element.removeAttribute('following');

          assert.isNotTrue(element.hasAttribute('following'));
          assert.equal(element.button.textContent, 'Follow');
        });

        test('it should set a profile pic from the photo-url attribute', () => {
          const element = document.querySelector('rh-cool-element');
          const photoUrlAttribute = element.getAttribute('photo-url');
          const shadowRoot = element.shadowRoot;
          const profilePicContainer = shadowRoot.querySelector('#profile-pic');
          const backgroundImage = profilePicContainer.style.backgroundImage.slice(5, -2);

          assert.equal(photoUrlAttribute, backgroundImage);
        });
      });
    </script>
  </body>
</html>
```

A couple of things to note here is the access to `shadowRoot`. `shadowRoot` is available to our element since we have a shadow root set up for us by extending `Rhelement` in the definition of our element. Something else new you may not have seen before is

```
shadowRoot.querySelector('slot').assignedNodes()[0].textContent.trim();
```

If you want to access the content in the `<slot></slot>` of your element, you need to used the `assignedNodes()` method to get to it. In this case, our username is the only item being passed into the slot so it's available to us as the only item in the array returned by `assignedNodes()`.

### Run the Test

Now, we need to do is run our test command and see how we did.

```
npm test
```

And here is the command line output.

![test output]({{ "assets/images/test-output.png" | relative_url }})

Great! All four of our tests are working in Chrome and Firefox.

## Travis Integration

Now that we have our tests written and our element code passes the tests, we can set up continuous integration on [Travis CI](https://travis-ci.org) so anytime we push changes to our repository, Travis will run our tests and let us know if everything is still passing.

If you used the generator to create your element, there should already be a `.travis.yml` file at the root of your element.

```
language: node_js
dist: trusty
sudo: required
addons:
  firefox: "latest"
  apt:
    sources:
      - google-chrome
    packages:
      - google-chrome-stable
node_js: stable
before_install:
  - npm install -g web-component-tester
install:
  - npm install
before_script:
script:
  - xvfb-run npm run test
```

All we're telling Travis to do is to use the current, stable version of Node.js, install the Web Component Tester globally, install our dependencies, and then run our tests. Assuming that everything goes according to plan, we should see a nice "build passing" badge in Travis.

That's it for testing. Now that we have our `rh-cool-element` and our tests are passing, we can publish and share our component on npm.

[Move to Step 4: Publish](step-4.html)

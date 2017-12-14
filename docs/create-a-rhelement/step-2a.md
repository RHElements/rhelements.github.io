---
layout: doc
---

# Step 2: Develop (Structure)

Run the start command in the package.json file to open up the demo page and have the rh-cool-element.js and rh-cool-element.compiled.js files update when you make changes to the files in the `/src` directory.

```
npm start
```

After running the command, your default browser should open up the demo page and look something like this.

![npm start command]({{ "assets/images/npm-start.png" | relative_url }}){:width="500px"}

It doesn't look like much, but we're off to a good start. We have a new custom element that extends our base Rhelement class, uses shadow DOM, and has a fallback to ShadyCSS in case shadow DOM isn't supported.

Let's take a look at the rh-cool-element.js file in the `/src` directory to see what we have.

```
import Rhelement from '../rhelement/rhelement.js';

/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-cool-element.html and css from
 * rh-cool-element.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */

class RhCoolElement extends Rhelement {
  constructor() {
    super('rh-cool-element', template);
  }
}

window.customElements.define('rh-cool-element', RhCoolElement);
```

First, you'll notice that we are using ES6 module imports and we import the Rhelement base element.
```
import Rhelement from '../rhelement/rhelement.js';
```

Second, you'll see where we set up the template and just like the comments say, **DO NOT EDIT** this section.
```
/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-cool-element.html and css from
 * rh-cool-element.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */
```
The gulp merge task in gulpfile.js will fill this section of our compiled and transpiled files when you edit a file in the `/src` directory.

Third, you'll see where we define the `RhCoolElement` class that extends the Rhelement base element. In the constructor, we call `super` and pass a string that will be the tag we use on a page and the template.
```
class RhCoolElement extends Rhelement {
  constructor() {
    super('rh-cool-element', template);
  }
}
```
The Rhelement base element will create a shadow root for us and handle the ShadyCSS work we'll need for browsers that don't support shadow DOM.

Finally, we register the element.
```
window.customElements.define('rh-cool-element', RhCoolElement);
```

If you have any questions about how Custom Elements work and need to learn more about the basics of shadow DOM, checkout Eric Bidelman's post: [Custom Elements v1: Reusable Web Components](https://developers.google.com/web/fundamentals/web-components/customelements).

Now that we have our structure and our dev server running, let's make our element actually do something.

[Move to Step 2: Develop (HTML)](step-2b.html)

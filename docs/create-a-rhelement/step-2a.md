---
layout: doc
---

# Step 2: Develop (Structure)

Run the dev command in the package.json file located at the root of our element to open to start watching for changes to the files in the `/src` directory of the element. This command will build the rh-cool-element.js and rh-cool-element.umd.js files anytime you make changes to the files in the `/src` directory.

```
npm run dev
```

After running the command, we can start a server at the root of the RHElements repository to view our work in the browser.

```
# from the root of the RHElements repository
npm start
```

This will start a simple HTTP server for us and it will reload the browser anytime we make changes to our element

![npm start command]({{ "assets/images/rh-cool-element-start.png" | relative_url }}){:width="500px"}

Navigate to `http://localhost:1234/elements/rh-cool-element/demo/` to view the element in the browser.

It doesn't look like much, but we're off to a good start. We have a new custom element that extends our base RHElement class, it uses shadow DOM, and has a fallback to ShadyCSS in case shadow DOM isn't supported.

Let's take a look at the rh-cool-element.js file in the `/src` directory to see what we have.

```
import RHElement from "../rhelement/rhelement.js";

class RhCoolElement extends RHElement {
  static get tag() {
    return "rh-cool-element";
  }

  get templateUrl() {
    return "rh-cool-element.html";
  }

  get styleUrl() {
    return "rh-cool-element.scss";
  }

  constructor() {
    super(RhCoolElement.tag);
  }
}

RHElement.create(RhCoolElement);
```

First, you'll notice that we are using ES6 module imports and we import the RHElement base element.
```
import RHElement from '../rhelement/rhelement.js';
```

Second, you'll see where we define the string for the HTML tag that we'll use on the page when we include our element.

```
static get tag() {
  return "rh-cool-element";
}
```

Third, we have references to where the HTML for our template and the Sass for our styles are located.

```
get templateUrl() {
  return "rh-cool-element.html";
}

get styleUrl() {
  return "rh-cool-element.scss";
}
```

The gulp merge task in gulpfile.js will fill this section of our compiled and transpiled files when you edit a file in the `/src` directory.

Fourth, we have the constructor for our element.

```
constructor() {
  super(RhCoolElement.tag);
}
```

The RHElement base element will create a shadow root for us and handle the ShadyCSS work we'll need for browsers that don't support shadow DOM.

Finally, we register our element using the `create` method location in our RHElement class. This method will call `window.customElements.define` for us.

```
RHElement.create(RhCoolElement);
```

If you have any questions about how Custom Elements work and need to learn more about the basics of shadow DOM, checkout Eric Bidelman's post: [Custom Elements v1: Reusable Web Components](https://developers.google.com/web/fundamentals/web-components/customelements).

Now that we have our structure and our dev server running, let's make our element actually do something.

[Move to Step 2: Develop (HTML)](step-2b.html)

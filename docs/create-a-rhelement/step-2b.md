---
layout: doc
---

# Step 2: Develop (HTML)

Now that we have our element set up and our dev server is running. Let's take advantage of the shadow root and the slot to make our element a bit more interesting.

I'll edit the `/src/rh-cool-element.html` file to add some additional HTML. Let's turn `rh-cool-element` into a profile element that has a profile photo, a username, and a button that will allow us to follow the user.

Here's the updated HTML in `/src/rh-cool-element.html`.

```
<div id="profile-pic"></div>
<slot></slot>
<div>
  <button>Follow</button>
</div>
```

Also, I've updated `/demo/index.html` so my name is being passed into the slot that we have set up in our element.

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>RHElements: rh-cool-element Demo</title>

    <!-- uncomment the es5-adapter if you're using the umd version -->
    <script src="/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js"></script>
    <script src="/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.5/require.min.js"></script>
    <script>require(['../rh-cool-element.umd.js'])</script>
  </head>
  <body>
    <rh-cool-element>
      Kyle Buchanan
    </rh-cool-element>
  </body>
</html>
```

Slots are a great way to take HTML from the light DOM and move it into a specific place in the shadow DOM. To learn more about shadow DOM and how to use slots, checkout [Shadow DOM v1: Self-Contained Web Components](https://developers.google.com/web/fundamentals/web-components/shadowdom).

The end result should look like this in the browser.

![demo page html step]({{ "assets/images/demo-page-html-step.png" | relative_url }}){:width="500px"}

Remember, since we have the `npm run dev` command running, any changes we make in the `/src` directory are being watched. When changes happen, the `merge` and `compile` tasks run in our gulpfile and our ES6 and ES5 versions of the component are updated in the root of our element. Our ES6 version should now look like this.

```
import RHElement from "../rhelement/rhelement.js";

class RhCoolElement extends RHElement {
  get html() {
    return `
<style>
:host {
  display: block; }

:host([hidden]) {
  display: none; }
</style>
<div id="profile-pic"></div>
<slot></slot>
<div>
  <button>Follow</button>
</div>`;
  }

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

As you can see, the task took the HTML we wrote in `/src/rh-cool-element.html` and merged it into our ES6 version of our component. The HTML we wrote is now included in the `get html()` method of our element. The same thing has happened to our ES5 version, but that file is minified and ugly to look at so just take my word for it.

Now that we've updated the HTML, let's make our element look good. We'll update our Sass file and style the element the way we want.

[Move to Step 2: Develop (CSS or Sass)](step-2c.html)

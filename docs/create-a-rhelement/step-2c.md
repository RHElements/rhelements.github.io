---
layout: doc
---

# Step 2: Develop (CSS or Sass)

We're working on our `rh-cool-element` and we want it to look like a profile with a profile photo, username, and follow button. But right now, our element is pretty ugly so we need to update our CSS (or Sass) to make it look the way we want.

To update the styles for our element, we'll be working in the `/src/rh-cool-element.scss` file. It's a `.scss` file in this case since I chose to use Sass when I initially created the element using the RHElement generator.

The initial state of our Sass file imports some additional Sass from the cp-sass node module, but we can ignore that for now. The second part has a `:host` selector that is used for making our element display as a block element.

```
@import "node_modules/@rhelements/cp-sass/cp-sass";
:host {
  display: block;
}
```

I'm just going to update the styles a bit so it looks more like a profile.

```
@import "node_modules/@rhelements/cp-sass/cp-sass";
:host {
  display: flex;
  width: 128px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 32px;
  font-family: Arial;
  box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2),
              0px 2px 2px 0px rgba(0, 0, 0, 0.14),
              0px 1px 5px 0px rgba(0, 0, 0, 0.12);
}

#profile-pic {
  width: 50px;
  height: 50px;
  border: 2px solid #333;
  border-radius: 50%;
  background-color: #efefef;
  margin-bottom: 16px;
}

button {
  margin-top: 16px;
}
```

After those updates and refreshing our demo page, our profile looks much better.

![demo page css step]({{ "assets/images/demo-page-css-step.png" | relative_url }}){:width="500px"}

A couple of things to note in here is the use of the `:host` selector to set the styles of our container element `<rh-cool-element>`. Second, we added styles to our `button` element but we don't have to worry about this style affecting other buttons on our page. Since we have a shadow root, our styles for this element are encapsulated and won't bleed outside of our element. This is one of the main benefits of shadow DOM in that we can feel confident that our element will always look the same when we distribute our element. There are a lot of cool things that you can do with styling shadow DOM. If you'd like to learn more check out the Styling section of [Shadow DOM v1: Self-Contained Web Components](https://developers.google.com/web/fundamentals/web-components/shadowdom#styling).

Now that our demo page has been updated, let's take a quick look at what happened to our ES6 version of the element in the root of our directory.

```
import Rhelement from '../rhelement/rhelement.js';

/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-cool-element.html and css from
 * rh-cool-element.scss
 */
const template = document.createElement('template');
template.innerHTML = `
<style>:host {
  display: flex;
  width: 128px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 32px;
  font-family: Arial;
  box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2), 0px 2px 2px 0px rgba(0, 0, 0, 0.14), 0px 1px 5px 0px rgba(0, 0, 0, 0.12); }

#profile-pic {
  width: 50px;
  height: 50px;
  border: 2px solid #333;
  border-radius: 50%;
  background-color: #efefef;
  margin-bottom: 16px; }

button {
  margin-top: 16px; }</style>
<div id="profile-pic"></div>
<slot></slot>
<div>
  <button>Follow</button>
</div>
`;
/* end DO NOT EDIT */

class RhCoolElement extends Rhelement {
  constructor() {
    super('rh-cool-element', template);
  }
}

window.customElements.define('rh-cool-element', RhCoolElement);
```

You'll notice that our style tag has all of the styles we just wrote in our Sass file. If we had Sass variables in the Sass that we wrote, they would've been compiled and the values would be reflected in the changes above.

Now that we have our `rh-cool-element` styled, we need to add some interaction to the follow button. We also need to find a way to fill in the profile photo. We can accomplish this by updating our `/src/rh-cool-element.js` file.

[Move to Step 2d](step-2d.html)

# Create a RHElement

## Prerequisites

Before you begin, make sure you've followed the [Prerequisites]({{ "/docs/get-started.html#prerequisites" | relative_url }}) in [Getting Started]({{ "/docs/get-started.html" | relative_url }}).

## Step 1: Scaffold an Element

Use the RHElement generator to start the scaffolding process.

```
yo rhelement
```

The generator will ask a series of questions to help you get everything set up.

1. Your element name:
  - Your element name should be lowercase and contain at least one hyphen in it. For the exact rules to naming a custom element, refer to the W3C [Custom Elements Working Draft](https://www.w3.org/TR/custom-elements/#valid-custom-element-name).
  - We've been prefixing elements with `rh-` if they are meant to be used by Red Hat and `cp-` if the element will be specific to the Customer Portal. But you can prefix your element with whatever makes most sense for you.
2. Author name:
  - Add your name
  - NOTE: we're going to update the generator in the future so it will do a better job with the author object to include an email address and GitHub profile.
3. Do you want to use Sass with this element? (Yes or No)
4. If yes to question #3, Do you want to use existing Sass dependencies? (cp-sass or No thanks. I'll provide my own later)
  - cp-sass will include the cp-sass node module with all of the Customer Portal mixins and variables.

After you've answered the questions, you should see something like this.

![yo rhelement command]({{ "assets/images/yo-rhelement.png" | relative_url }}){:width="500px"}

Once that completes, change directories into the name of the element you just created. In this case will go into the rh-tabs directory.

```
cd rh-tabs
```

Open up your code editor and view the structure of the element. The important things to note are the `/src`, `/demo` and `/test` directories and the rh-tabs.js and rh-tabs.compiled.js files. You'll do all your development in the `/src` directory and write your tests in `/test` directory. We'll use the `/demo` directory to preview the element locally and the rh-tabs.js and rh-tabs.compiled.js files we'll be used for the demo.

## Step 2: Develop

Run the start command in the package.json file to open up the demo page and have the rh-tabs.js and rh-tabs.compiled.js files update when you make changes to the files in the `/src` directory.

```
npm start
```

Let's take a look at the rh-tabs.js file in the `/src` directory to see what we have.

```
import Rhelement from '../rhelement/rhelement.js';

/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-tabs.html and css from
 * rh-tabs.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */

class RhTabs extends Rhelement {
  constructor() {
    super('rh-tabs', template);
  }
}

window.customElements.define('rh-tabs', RhTabs);
```

First, you'll notice that we are using ES6 module imports and we import the Rhelement base element.
```
import Rhelement from '../rhelement/rhelement.js';
```

Second, you'll see where we set up the template and just like the comments say, **DO NOT EDIT** this section.
```
/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-tabs.html and css from
 * rh-tabs.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */
```
The gulp merge task in gulpfile.js will fill this section of our compiled and transpiled files when you edit a file in the `/src` directory.

Third, you'll see where we define the `RhTabs` class that extends the Rhelement base element. In the constructor, we call `super` and pass a string that will be the tag we use on a page and the template.
```
class RhTabs extends Rhelement {
  constructor() {
    super('rh-tabs', template);
  }
}
```
The Rhelement base element will create a shadow root for us and handle the ShadyCSS work we'll need for browsers that don't support shadow DOM.

Finally, we register the element.
```
window.customElements.define('rh-tabs', RhTabs);
```

If you have any questions about how Custom Elements work and need to learn more about the basics of shadow DOM, checkout Eric Bidelman's post: [Custom Elements v1: Reusable Web Components](https://developers.google.com/web/fundamentals/web-components/customelements).

---
layout: doc
---

# Create a RHElement

## Prerequisites

Before you begin, make sure you've followed the [Prerequisites]({{ "/docs/get-started.html#prerequisites" | relative_url }}) in [Getting Started]({{ "/docs/get-started.html" | relative_url }}).

## Step 1: Scaffold an Element

Use the RHElement generator to start the scaffolding process. From the root directory of the RHElements repository, run the following command.

```
npm run new
```

The generator will ask you a series of questions to set up your project.

1.  Your element name:
  - Your element's name should be lowercase and needs to contain at least one hyphen. For rules on naming custom elements, refer to the W3C [Custom Elements Working Draft](https://www.w3.org/TR/custom-elements/#valid-custom-element-name).
  - Red Hat prefixes elements with `rh-` for global components and then namespaces web property specific ones like `cp-` for the Customer Portal. However, prefix your elements with whatever fits your project.
  - As an example, we'll create `rh-your-element`.
  
2.  Author name:
  - Add your name
  - NOTE: we will update the generator in the future to include an email address and GitHub profile.

3.  Do you want to use Sass with this element? (Yes or No)

4.  If yes to question #3, Do you want to use existing Sass dependencies? (rh-sass or No thanks. I'll provide my own later)
  - rh-sass includes the rh-sass node module with all of the mixins and variables used in RHElements.

After answering the questions, you'll see something like this:

![npm run new command]({{ "assets/images/npm-run-new.png" | relative_url }}){:width="500px"}

Once that's done, switch directories to the element you just created. We'll cd into the rh-your-element directory.

```
cd elements/rh-your-element
```

Open your code editor to view the structure of the element. It's important to note the `/src`, `/demo` and `/test` directories and the rh-your-element.js and rh-your-element.compiled.js files. The `/src` directory is reserved for development and you can write tests in `/test` directory. Finally, the `/demo` directory lets you preview your element locally using the rh-your-element.js and rh-your-element.umd.js files.

[Move to Step 2: Develop (Structure)](step-2a.html)

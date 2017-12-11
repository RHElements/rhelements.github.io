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
  - Your element name should be lowercase and contain at least one hyphen in it. For the exact rules to naming a custom element, refer to the W3C [Custom Elements Working Draft](https://www.w3.org/TR/custom-elements/#valid-custom-element-name). In this case, we're going to create `rh-cool-element`.
  - We've been prefixing elements with `rh-` if they are meant to be used by Red Hat and `cp-` if the element will be specific to the Customer Portal. But you can prefix your element with whatever makes most sense for you.
2. Author name:
  - Add your name
  - NOTE: we're going to update the generator in the future so it will do a better job with the author object to include an email address and GitHub profile.
3. Do you want to use Sass with this element? (Yes or No)
4. If yes to question #3, Do you want to use existing Sass dependencies? (cp-sass or No thanks. I'll provide my own later)
  - cp-sass will include the cp-sass node module with all of the Customer Portal mixins and variables.

After you've answered the questions, you should see something like this.

![yo rhelement command]({{ "assets/images/yo-rhelement.png" | relative_url }}){:width="500px"}

Once that completes, change directories into the name of the element you just created. In this case will go into the rh-cool-element directory.

```
cd rh-cool-element
```

Open up your code editor and view the structure of the element. The important things to note are the `/src`, `/demo` and `/test` directories and the rh-cool-element.js and rh-cool-element.compiled.js files. You'll do all your development in the `/src` directory and write your tests in `/test` directory. We'll use the `/demo` directory to preview the element locally and the rh-cool-element.js and rh-cool-element.compiled.js files we'll be used for the demo.

[Move on to Step 2a](step-2a.html)

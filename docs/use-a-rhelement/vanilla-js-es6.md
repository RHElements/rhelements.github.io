---
layout: doc
---

# Use a RHElement: Vanilla JS ES6

Each RHElement ships with an ES6 version of the component. This method is your
best option if you don't have to support IE11. You can just use
`<script type="module">` to include a RHElement.

What's so nice about this method is that you only need to include the RHElements
that you want and the browser will take care of loading all of the dependencies.

[Script type module support](https://caniuse.com/#feat=es6-module)

## Syntax

### Using a Module with the Source Attribute

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">

    <!-- load the web component polyfills -->
    <script src="/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>

    <!-- load our components using type="module" -->
    <script type="module" src="@rhelements/rh-card/rh-card.js"></script>
  </head>
  <body>
    <rh-card>
      <h2 slot="header">My Card</h2>
      This is the rh-card body.
      <div slot="footer">Text in footer</div>
    </rh-card>
  </body>
</html>
```

### Using a Module with the Import Statement

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">

    <!-- load the web component polyfills -->
    <script src="/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>

    <!-- load our components using type="module" and an import statement -->
    <script type="module">
      import "@rhelements/rh-card/rh-card.js";
    </script>
  </head>
  <body>
    <rh-card>
      <h2 slot="header">My Card</h2>
      This is the rh-card body.
      <div slot="footer">Text in footer</div>
    </rh-card>
  </body>
</html>
```

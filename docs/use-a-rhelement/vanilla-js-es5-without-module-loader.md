---
layout: doc
---

# Use a RHElement: Vanilla JS ES5 without a Module Loader

If you need to support IE11 and you are not using a module loader, this method
works, but it's a bit cumbersome.

## Syntax

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">

    <!-- we need this adapter to "upgrade" our ES5 components to ES6 -->
    <script src="/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js"></script>

    <!-- load the web component polyfills -->
    <script src="/@webcomponents/webcomponentsjs/webcomponents-loader.js"></script>

    <!-- load our components. the order matters. dependencies need to be loaded first -->
    <script src="@rhelements/rhelement/rhelement.umd.js"></script>
    <script src="@rhelements/rh-card/rh-card.umd.js"></script>
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

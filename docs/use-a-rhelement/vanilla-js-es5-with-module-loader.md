---
layout: doc
---

# Use a RHElement: Vanilla JS ES5 with a Module Loader

Each RHElement ships with an ES5 UMD (Universal Module Definition) version of
the component. If you need to support IE11 and you are using a module loader
like require.js, loading a RHElement is pretty simple. Just use the module
loader to load the component and the module loader will take care of loading
the dependencies. Just make sure that you load the version of the component
that has `.umd.js` as the extension.

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

    <!-- add a module loader if we don't already have one -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.5/require.min.js"></script>

    <!-- load our components using require.js -->
    <script>require(["@rhelements/rh-card/rh-card.umd.js"])</script>
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

### Add as many RHElements as you need

```
<script>
  require([
    "@rhelements/rh-card/rh-card.umd.js",
    "@rhelements/rh-cta/rh-cta.umd.js"
  ])
</script>
```

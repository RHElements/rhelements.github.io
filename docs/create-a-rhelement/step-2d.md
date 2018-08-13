---
layout: doc
---

# Step 2: Develop (Javascript)

In this step, we will:
- Add a click listener to the follow button
- Set a follow state on element
- Add a the profile photo
- Properly disconnect our element

First, we'll go ahead and listen for click events on the button. The best place to add a click listener is in the constructor, according to the W3C Custom Elements draft section called ["2.2 Requirements for custom element constructors"](https://w3c.github.io/webcomponents/spec/custom/#custom-element-conformance):

> In general, the constructor is responsible for setting the initial state, default values, event listeners, and a shadow root.

Since our base RHElement that we extended already sets up a shadow root, all we'll need to do is set up an event listener.

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

    this.button = this.shadowRoot.querySelector("button");
    this.button.addEventListener("click", this._clickHandler);
  }

  _clickHandler(evt) {
    console.log("Button clicked!!!");
  }
}

RHElement.create(RhCoolElement);
```

In the constructor, we ran `querySelector` on our `shadowRoot` to locate the button and added a click listener `this._clickHandler` to capture the event.

Please note the underscore before the method name. This is a convention you'll notice in other custom elements where the author is trying to signal that it's meant as a private method.

After saving your files, the demo page will refresh and you'll notice the start of your button interactivity.

![demo page js click setup step]({{ "assets/images/demo-page-js-click-setup-step.png" | relative_url }}){:width="500px"}

Now that the click handler is set up, let's set a following state to keep track of whether or not you're following that user. 

We'll update the `constructor` like so:

```
constructor() {
  super('rh-cool-element', template);

  this.following = false;
  this._clickHandler = this._clickHandler.bind(this);

  this.button = this.shadowRoot.querySelector("button");
  this.button.addEventListener('click', this._clickHandler);
}
```

We did a couple things here. First, we set the state of `following` to false on our element and then we bound `this` to our click handler to continue using `this` to refer to our element in the `_clickHandler`.

## Getter, Setter, and Element State

Next, we'll need to create a getter and a setter for the `following` property of our element. The setter helps us reflect the state of following to an attribute on `rh-cool-element`, and the getter provides us with the value of that attribute. 

Let's use `following` as an attribute:

```
set following(value) {
  const isFollowing = Boolean(value);

  if (isFollowing) {
    this.setAttribute("following", "");
  } else {
    this.removeAttribute("following");
  }
}

get following() {
  return this.hasAttribute("following");
}
```

Now that our getter and setter is wired up, we can update the `_clickHandler` method to toggle the following state.

```
_clickHandler() {
  this.following = !this.following;
}
```

When we click the follow button now, you'll notice the `following` attribute set and unset as we toggle the button.

![demo page js attribute changed step]({{ "assets/images/demo-page-js-attribute-change-step.png" | relative_url }}){:width="500px"}

## Observed Attributes

Let's add one more thing with the follow state. We should update the button's text to "Unfollow" when `this.following` is true and to "Follow" when `this.following` is false. Observing attributes is an easy way to tell a custom element to perform an action when an attribute changes. In this case, we keep an eye on the `following` attribute for changes, and when it does, we'll update the button's text.

To observe an attribute, add this code to the top of your class:

```
static get observedAttributes() {
  return ["following"];
}
```

`observedAttributes` returns an array of attributes you want observed. To react to a change, we'll need to set up the `attributeChangedCallback` method.

```
attributeChangedCallback(name, oldValue, newValue) {
  switch (name) {
    case "following":
      this.button.textContent = this.following ? "Unfollow" : "Follow";
      break;
  }
}
```

If the changed attribute is `following`, the button text will update based on our conditional. Pretty straight-forward, right? Now our button completely reflects the state of `following` with everything wired up. 

The UI will look like this when we click the follow button:

![demo page js attribute changed step]({{ "assets/images/demo-page-js-follow-attribute-changed-step.png" | relative_url }}){:width="500px"}

Next, we add a `photo-url` attribute to pass in a profile image. Once again, we'll use the `observedAttributes` and the `attributeChangedCallback` to handle the work. We can add a profile image with only a few updates!

Initially, we'll need to include a reference to `#profile-pic` in the constructor by setting `this.profilePic`.

```
constructor() {
  super(RhCoolElement.tag);

  this.following = false;
  this._clickHandler = this._clickHandler.bind(this);

  this.button = this.shadowRoot.querySelector("button");
  this.button.addEventListener("click", this._clickHandler);

  this.profilePic = this.shadowRoot.querySelector("#profile-pic");
}
```

Now, we'll add the `photo-url` attribute to our `observedAttributes`:

```
static get observedAttributes() {
  return ["following", "photo-url"];
}
```

Now we can update our `attributeChangedCallback` to set the image:

```
attributeChangedCallback(name, oldValue, newValue) {
  switch (name) {
    case "following":
      this.button.textContent = this.following ? "Unfollow" : "Follow";
      break;

    case "photo-url":
      this.profilePic.style.backgroundImage = `url(${newValue})`;
      break;
  }
}
```

Finally, we'll need to update our demo page (`/demo/index.html`) to include the `photo-url` attribute. Pass in an image URL to see that it's working.

```
<rh-cool-element photo-url="https://avatars2.githubusercontent.com/u/330256?s=400&u=de56919e816dc9f821469c2f86174f29141a896e&v=4">
  Kyle Buchanan
</rh-cool-element>
```

We can also modify `/src/rh-cool-element.scss` to adjust the background-size property on `#profile-pic`:

```
#profile-pic {
  width: 50px;
  height: 50px;
  margin-bottom: 16px;
  border: 2px solid #333;
  border-radius: 50%;
  background-color: #efefef;
  background-size: contain;
}
```

The final result should look like this:

![demo page js profile pic step]({{ "assets/images/demo-page-js-profile-pic-step.png" | relative_url }}){:width="500px"}

Great! You're almost there.

## Disconnected Callback

It's a good idea to clean up your event listeners after a web component is disconnected. The lifecycle callback that runs on web components is the `disconnectedCallback`, ideal for cleaning up our code. For this example, all we'll need to do is remove the click listener we added to the follow button.

```
disconnectedCallback() {
  this.button.removeEventListener("click", this._clickHandler);
}
```

## Wrap-up

That's all it takes, folks! To summarize, we built a web component that extends our base element, and added some HTML, custom styles, and interactivity to our component. What's cool is that we've only scratched the surface of what's possible with custom elements.

For your reference, here's the final Javascript code for `rh-cool-element`:

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

  static get observedAttributes() {
    return ["following", "photo-url"];
  }

  set following(value) {
    const isFollowing = Boolean(value);

    if (isFollowing) {
      this.setAttribute("following", "");
    } else {
      this.removeAttribute("following");
    }
  }

  get following() {
    return this.hasAttribute("following");
  }

  constructor() {
    super(RhCoolElement.tag);

    this.following = false;
    this._clickHandler = this._clickHandler.bind(this);

    this.button = this.shadowRoot.querySelector("button");
    this.button.addEventListener("click", this._clickHandler);

    this.profilePic = this.shadowRoot.querySelector("#profile-pic");
  }

  attributeChangedCallback(name, oldValue, newValue) {
    switch (name) {
      case "following":
        this.button.textContent = this.following ? "Unfollow" : "Follow";
        break;

      case "photo-url":
        this.profilePic.style.backgroundImage = `url(${newValue})`;
        break;
    }
  }

  disconnectedCallback() {
    this.button.removeEventListener("click", this._clickHandler);
  }

  _clickHandler(evt) {
    this.following = !this.following;
  }
}

RHElement.create(RhCoolElement);
```

Now that our code works, we should create tests to ensure our element works as we iterate on it in the future.

[Move to Step 3: Test](step-3.html)

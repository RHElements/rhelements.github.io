# Step 2: Develop (Javascript)

In this step, we'll accomplish a few things.
- Add a click listener to our follow button
- Set a follow state on element
- Have an image show up in our profile photo
- Properly disconnect our element

First, let's listen for clicks on the button in our element. The best place to add the click listener to the button is in the constructor. According to the W3C Custom Elements draft section called ["2.2 Requirements for custom element constructors"](https://w3c.github.io/webcomponents/spec/custom/#custom-element-conformance):

> In general, the constructor should be used to set up initial state and default values, and to set up event listeners and possibly a shadow root.

Since the base Rhelement class that we're extending already sets up the shadow root, all we need to do is set up our click listener.

```
import Rhelement from '../rhelement/rhelement.js';

/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-cool-element.html and css from
 * rh-cool-element.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */

class RhCoolElement extends Rhelement {
  constructor() {
    super('rh-cool-element', template);

    this.button = this.shadowRoot.querySelector('button');
    this.button.addEventListener('click', this._clickHandler);
  }

  _clickHandler(evt) {
    console.log('Button clicked!!!');
  }
}

window.customElements.define('rh-cool-element', RhCoolElement);
```

In our constructor, we run `querySelector` on our `shadowRoot` to locate our button. Then we add the click listener, `this._clickHandler`. Note the underscore before the method name. This is a convention that you'll find in other custom elements where the author is trying to signal that this method is meant to be a private method.

Refresh your demo page, click the button and you should see the following result.

![demo page js click setup step]({{ "assets/images/demo-page-js-click-setup-step.png" | relative_url }}){:width="500px"}

Now that the click handler is set up, let's set a following state on our element to keep track of whether or not the user is following this profile. We'll update the `constructor` with a couple lines of code.

```
constructor() {
  super('rh-cool-element', template);

  this.following = false;
  this._clickHandler = this._clickHandler.bind(this);

  this.button = this.shadowRoot.querySelector('button');
  this.button.addEventListener('click', this._clickHandler);
}
```

We did two things here. First we set the state of `following` to false on our element. We also bound `this` to our click handler so we can continue to use `this` and refer to our element in the `_clickHandler`.

## Getter, Setter, and Element State

Next, let create a getter and a setter for the `following` property of our element. The setter will enable us to reflect the state of following to an attribute on `rh-cool-element`. The getter will provide us with the value of that attribute. Let's use `following` as the attribute to reflect the state.

```
set following(value) {
  const isFollowing = Boolean(value);

  if (isFollowing) {
    this.setAttribute('following', '');
  } else {
    this.removeAttribute('following');
  }
}

get following() {
  return this.hasAttribute('following');
}
```

Now that we have the following getter and setter wired up, let's update the `_clickHandler` method to toggle the following state.

```
_clickHandler() {
  this.following = !this.following;
}
```

So now when we click the follow button, we should see the `following` attribute set and unset as we toggle the button.

![demo page js attribute changed step]({{ "assets/images/demo-page-js-attribute-change-step.png" | relative_url }}){:width="500px"}

## Observed Attributes

Let's do one more thing with the follow state. Let's update the button text to "Unfollow" when `this.following` is true and be "Follow" when `this.following` is false. In this part, we'll introduce a new concept: observing attributes. Observing attributes is an easy way to tell your custom element to perform an action when an attribute your observing changes. In this case, we want to know when the `following` attribute has changed. When it does, we'll change the text of our follow button. To observe an attribute, add this code to the top of your class.

```
static get observedAttributes() {
  return ['following'];
}
```

`observedAttributes` should return an array of attributes that you want to observe. In this case, we'll observe the `following` attribute. To react to a change in the attributes were observing, we need to set up the `attributeChangedCallback` method.

```
attributeChangedCallback(name, oldValue, newValue) {
  switch (name) {
    case 'following':
      this.button.textContent = this.following ? 'Unfollow' : 'Follow';
      break;
  }
}
```

All this code does is check which attribute changed and if it was the `following` attribute, we update our button text. Pretty straight-forward. Now our button should reflect the state of `following` in our element. With everything wired up, our UI should now look like this when we click the follow button.

![demo page js attribute changed step]({{ "assets/images/demo-page-js-follow-attribute-changed-step.png" | relative_url }}){:width="500px"}

Two more things and then we're done. Next, let's use a `photo-url` attribute to pass in a photo url for our profile pic. Once again we can use `observedAttributes` and the `attributeChangedCallback` to handle this. So with just a few updates to those two methods, we can add our profile pic.

Here is the update to `observedAttributes`.

```
static get observedAttributes() {
  return ['following', 'photo-url'];
}
```

And the updates to the `attributeChangedCallback`.

```
attributeChangedCallback(name, oldValue, newValue) {
  switch (name) {
    case 'following':
      this.button.textContent = this.following ? 'Unfollow' : 'Follow';
      break;

    case 'photo-url':
      this.profilePic.style.backgroundImage = `url(${newValue})`;
      break;
  }
}
```

Now we need to update our demo page (`/demo/index.html`) and add the `photo-url` attribute with a URL to a profile pic so we can see everything working.

```
<rh-cool-element photo-url="https://avatars2.githubusercontent.com/u/330256?s=400&u=de56919e816dc9f821469c2f86174f29141a896e&v=4">
  Kyle Buchanan
</rh-cool-element>
```

And finally, we need to modify `/src/rh-cool-element.scss` and update the background-size property on `#profile-pic`.

```
#profile-pic {
  width: 50px;
  height: 50px;
  border: 2px solid #333;
  border-radius: 50%;
  background-color: #efefef;
  margin-bottom: 16px;
  background-size: contain;
}
```

The final result should look like this.

![demo page js profile pic step]({{ "assets/images/demo-page-js-profile-pic-step.png" | relative_url }}){:width="500px"}

Great! Almost done. We have one last step which is cleaning up our event listeners when our web component is disconnected.

## Disconnected Callback

It's a good idea to clean up any event listeners that you've added to your web component. One of the lifecycle callbacks that are run on web components is `disconnectedCallback` and this is where we run our cleanup code. In our case, all we need to do is remove the click listener that we added to our follow button.

```
disconnectedCallback() {
  this.button.removeEventListener('click', this._clickHandler);
}
```

## Wrap-up

And that's it! We're all done. To summarize, we've built a brand new web component that extends a base element. We built some HTML, added some custom style, and added some interactivity to our component. With this element we just scratched the surface of what's possible with custom elements.

For reference, here's the final Javascript code for our element.

```
import Rhelement from '../rhelement/rhelement.js';

/*
 * DO NOT EDIT. This will be autopopulated with the
 * html from rh-cool-element.html and css from
 * rh-cool-element.scss
 */
const template = document.createElement('template');
template.innerHTML = ``;
/* end DO NOT EDIT */

class RhCoolElement extends Rhelement {
  static get observedAttributes() {
    return ['following', 'photo-url'];
  }

  constructor() {
    super('rh-cool-element', template);

    this.following = this.hasAttribute('following') || false;
    this._clickHandler = this._clickHandler.bind(this);

    this.button = this.shadowRoot.querySelector('button');
    this.button.addEventListener('click', this._clickHandler);

    this.profilePic = this.shadowRoot.querySelector('#profile-pic');
  }

  disconnectedCallback() {
    this.button.removeEventListener('click', this._clickHandler);
  }

  set following(value) {
    const isFollowing = Boolean(value);

    if (isFollowing) {
      this.setAttribute('following', '');
    } else {
      this.removeAttribute('following');
    }
  }

  get following() {
    return this.hasAttribute('following');
  }

  _clickHandler() {
    this.following = !this.following;
  }

  attributeChangedCallback(name, oldValue, newValue) {
    switch (name) {
      case 'following':
        this.button.textContent = this.following ? 'Unfollow' : 'Follow';
        break;

      case 'photo-url':
        this.profilePic.style.backgroundImage = `url(${newValue})`;
        break;
    }
  }
}

window.customElements.define('rh-cool-element', RhCoolElement);
```

Now that we have all of the code working for our `rh-cool-element`, we need to make sure we have tests written to ensure that our element is going to work as it changes in the future. We'll take care of that in the next step.

[Move to Step 3: Test](step-3.html)

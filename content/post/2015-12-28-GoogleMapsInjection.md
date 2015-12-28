+++
categories = ["scripts", "tools", "javascript"]
date = "2015-12-28T12:46:08-03:00"
description = ""
keywords = ["google maps", "jquery", "javascript", "injection", "coordinates", "click"]
title = "Get GoogleMaps.com coordinates to clipboard on click"
+++

If you want to develop a Geolocalized app, **you probably will need some seeds**. Getting the coordinates requires a lot of clicks and mouse manipulation, making this task really slow 😥.

So, I wrote a Javascript code to inject some functions to [googlemaps.com](https://googlemaps.com), just to make it easier 👌

> You can view final result and code [here]({{< relref "#result" >}}).

## The Current and Slow Method

1.  Go to the map and click the location you want.
2.  Select the coordinates from the bottom modal view (Do not click it!).
3.  `CTRL+C` or `CMD+C`.
4.  Click somewhere else to hide the view.
5.  Go to step `1.` with another location.

{{% figure src="/images/googlemaps-default.png"%}}

## The Hacker (and fun) Way

Let's open the **Developer Tools**:

{{% figure src="/images/googlemaps-devtools.png"%}}

Now, we will inject [`jQuery`](https://jquery.com/) to the webpage 💉 adding a `<script/>` element and appending to the `<head/>`:

```javascript
var script = document.createElement('script');
script.src = "//ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(script);
```

Remember the floating view from the bottom? OK, let's get the element containing the coordinates:

```javascript
// Get coordinates <a> object
var coords = jQuery('#reveal-card > div > div.widget-reveal-card-container > a')
```

See the point `4.` from the old fashioned way, **we don't want to click somewhere else to hide the view after copying the contents**.

So we will programmatically press the *close* button, but first we need the `<button/>` element:

```javascript
// Get close button
var cross = jQuery('#reveal-card > div > button.widget-reveal-card-close')
```

### Events

Finally, we need to add an event: *every time the coordinates change should trigger an action that will capture the new coordinates.*

So, to the `coords` element we will response to `DOMSubtreeModified` event:

```javascript
coords.bind('DOMSubtreeModified', function() {

  /*
  TODO: Copy coordinates
  */

  // Close box
  cross.click();
}
```

My first approach to copy the contents was using `document.execCommand('copy')`:

```javascript
coords.bind('DOMSubtreeModified', function() {
  jQuery(this).select();

  try {
    // Always false :(
    var success = document.execCommand('copy');
    if (!success) {
      console.error('could not be copied');
    }
  } catch(err) {
    console.error('Oops, unable to copy');
    console.error(err);
  }

  // Close box
  cross.click();
});
```

But, **`document.execCommand` does not work on DeveloperTools**.

After spending a while researching, I found out that Chrome exposes the `copy('text')` function:

```javascript
coords.bind('DOMSubtreeModified', function() {
  // Get text
  var text = jQuery(this).text();

  // Copy coordinates to clipboard :)
  copy(text);

  // Display to console
  console.log(text);

  // Close box
  cross.click();
});
```

### Result

{{% figure src="/images/googlemaps-result.gif"%}}

#### Full Code

```javascript
// Inject jQuery
var script = document.createElement('script');
script.src = "//ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(script);
```

**Wait a second here**.

```javascript

// Get coordinates <a> object
var coords = jQuery('#reveal-card > div > div.widget-reveal-card-container > a')

// Get close button
var cross = jQuery('#reveal-card > div > button.widget-reveal-card-close')

// Add actions
coords.bind('DOMSubtreeModified', function() {
  // Get text
  var text = jQuery(this).text();

  // Copy coordinates to clipboard
  copy(text);

  // Display to console
  console.log(text);

  // Close box
  cross.click();
});
```

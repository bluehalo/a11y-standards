# Accessibility Standards for Developers
Accessibility, sometimes abbreviated as a11y, is the principle of designing a site or app to be usable for the widest range of possible users, to include those with disabilities either permanent or temporary. This guide includes best practices and standards for developing with accessibility in mind.

## Alt Text for Images
Every image in your app needs to have an alt text attribute describing what the image is showing. If the image is decorative only and doesn’t convey meaning or provide context, an empty alt attribute may be used to tell assistive technologies such as screen readers that the image may be ignored.

*Don't* 🚫
```HTML
<img src="/my/cool/image.png">
```

*Do* ✅
```HTML
<img src="./logo-horizontal-white.png" alt="Firehawk Logo"/>
```
```HTML
<img src="/my/cool/image.png" alt="">
```

## Focus styles
Focus styles are usually implemented by the browser as a blue outline around any focused element. These styles are important for keyboard navigation to be able to visualize which elements are focused. Do not add a global rule removing the focus outline on all elements without defining a style of your own.

*Don't* 🚫 *(unless you are redefining focus styles elsewhere)*
```CSS
*:focus {
  outline: none;
}
```

> ### Testing Tip
> Try navigating your site by keyboard only. Make a note of where you expect focus to move to when hitting the `tab` key and where the app doesn't meet your expectations.

## Make DOM order match visual order
Ensuring the DOM order matches the visual order of the page is important for assistive technologies to be able to read out the content on the page. It is also important to maintain tab order. Make sure the page’s structure is well defined in HTML and don’t get in the habit of using CSS to change the ordering of elements on the page.

A classic example of this is action buttons pulled to the right of a header row. The way this is often done is by using `float: right` on each button (or a bootstrap helper class like `.pull-right`), but this method forces the developer to list the buttons in reverse order.

*Don't* 🚫
```HTML
<button class="pull-right">Third</button>
<button class="pull-right">Second</button>
<button class="pull-right">First</button>
```

*Do* ✅
```HTML
<span class="pull-right">
  <button>First</button>
  <button>Second</button>
  <button>Third</button>
</span>
```

> ### Testing Tip
> Try disabling CSS on your app and see if it's still readable/usable and if the layout makes sense. This view is exactly how assistive technologies will interpret a web page and will also give insight into tab order.

## Write semantic markup
Assistive technologies rely on reading and parsing markup alone in order to convey meaning to the end user. Be sure to always use the correct HTML tag for each element, and don't use CSS to style one type of element to look like another. Also consider using HTML5 semantic elements to convey more meaning, e.g. `<nav>`, `<main>`, `<header>`, `<section>`, etc.

*Don't* 🚫
```HTML
<a class="btn btn-primary">Button</a>
```

*Do* ✅
```HTML
<button class="btn btn-primary">Button</button>
```

*Don't* 🚫
```HTML
<div class="link" onclick="redirectTo('otherPage')">Div link</div>
```

*Do* ✅
```HTML
<a href="path/to/otherPage">Actual link</a>
```

## ARIA Roles
Use ARIA roles to add semantic meaning to your markup, but **only when you cannot provide native HTML semantics**. The first rule of ARIA use is if can use a native HTML element with built in semantics, then you should do so. Additionally, it is bad practice to override or change native semantics.

*Don't* 🚫
```HTML
<h2 role="tab">heading tab</h2>
```

*Do* ✅
```HTML
<div role="tab">
  <h2>heading tab</h2>
</div>
```

Landmark roles can be used to identify large content areas to help screen readers navigate the webpage. Ideally all content of a document would be placed withing a landmark role.

*Do* ✅
```HTML
<footer role="contentinfo"></footer>
```

## Accessible Names
Make sure all interactive elements have an accessible name that a screen reader can read and attach to the element.

*Don't* 🚫
```HTML
User name <input type="text">
```

*Do* ✅
```HTML
<!-- use aria-label -->
<input type="text" aria-label="User name">
```
```HTML
<!-- use label with a for attribute -->
<label for="username">User name</label>
<input type="text" id="username">
```
```HTML
<!-- wrap the element with a label -->
<label>
  User name
  <input type="text">
</label>
```
```HTML
<!-- use aria-labelledby -->
<span id="username-label">User name</span>
<input type="text" aria-labelledby="username-label">
```

> ### Testing Tip
> Turn your screen off and try navigating your app with a screen reader. This will give you insight into your page structure and highlight which elements don't have accessible names, for example when you navigate to an element and hear the screen reader just say "button".

## Icons
Semantic icons are icons that convey meaning on their own, such as a button icon without a label. Semantic icons must have an `aria-label` for screen readers.

*Do* ✅
```HTML
<button class="btn-icon">
	<span class="fa fa-pencil" aria-label="Edit search criteria"></span>
</button>
```

Decorative icons are icons that are solely for visual delight and provide no additional meaning or context. An example would be a floppy disk icon in a button beside the word "Save". These icons should have `aria-hidden="true"` so screen readers know to ignore them.

*Do* ✅
```HTML
<span class="fa fa-save" aria-hidden="true"></span> Save
```

## Use of color
Color cannot be used as the only visual means of conveying information. For example, don't just use a red outline on invalid form fields as those fields will become indistinguishable to colorblind users.

## Contrast
The contrast between text and its background should be at least 4.5:1 for normal size text and 3:1 for large scale text.

## ARIA Live Regions
Use `aria-Live` to define a region of dynamic content and tell the screen reader to announce updates to the content within in the region.

The `aria-live` attribute accepts a politeness setting as a value:
* `aria-live="off"` is the default and means the screen reader will not announce dynamic changes
* `aria-live="polite"` will cause the screen reader to announce changes when the user is idle
* `aria-live="assertive"` will cause the screen reader to announce changes as soon as they happen, possibly interupting the user

In addition, there are specialized live region roles that can be used such as `alert` or `status`.

## Links
Anchor tags must have an `href` associated with them in order to be a valid link and therefore be fully accessible. Anchor tags without an `href` will not receive keyboard focus when using `tab` to navigate the page.

*Don't* 🚫
```HTML
<a>Some link</a>
```

*Do* ✅
```HTML
<a href="path/to/resource.html">Much better link</a>
```

Note: Angular will automatically apply an `href` when using the `routerLink` directive.

*Do* ✅
```HTML
<a [routerLink]="['/path/to/route']">Angular router link</a>
```

What if you have a link that doesn't redirect anywhere, but just exists to trigger a JavaScript function? You have a couple of options:

```HTML
<a href="#" onclick="action($event)">Action link</a>
<script>
	// prevent redirecting to the homepage
	function action(event) {
		event.preventDefault();
	}
</script>
```
```HTML
<!-- works, but not recommended -->
<a href="javascript:;">Link</a>
```

But the best method would be considering changing the anchor tag to a button; this would be more semantic. In general, links should be used for redirecting and routing, and buttons should be used for functionality.

*Do* ✅
```HTML
<button (click)="action()">Action button</button>
```

## A11Y Resources
* [I Used The Web For A Day Using A Screen Reader](https://www.smashingmagazine.com/2018/12/voiceover-screen-reader-web-apps/)
* [I Used The Web For A Day With Just A Keyboard](https://www.smashingmagazine.com/2018/07/web-with-just-a-keyboard/)
* [WCAG Standards](https://www.w3.org/WAI/standards-guidelines/wcag/)
* [WCAG Quickref](https://www.w3.org/WAI/WCAG21/quickref/)
* [The A11Y project](https://a11yproject.com/)
* [ARIA Live regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions#Simple_live_regions)
### Tools
* [WebAIM Color Contrast Checker](https://webaim.org/resources/contrastchecker/)
* [Web Developer extension](https://chrispederick.com/work/web-developer/)

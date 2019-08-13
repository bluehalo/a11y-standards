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

*Don't* 🚫
```CSS
*:focus {
  outline: none;
}
```

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

## Icons
Semantic icons are icons that convey meaning on their own, such as a button icon without a label. Semantic icons must have an `aria-label` for screen readers.

Decorative icons are icons that are solely for visual delight and provide no additional meaning or context. An example would be a floppy disk icon in a button beside the word "Save". These icons should have `aria-hidden="true"` so screen readers know to ignore them.

## Use of color
Color cannot be used as the only visual means of conveying information. For example, don't just use a red outline on invalid form fields as those fields will become indistinguishable to colorblind users.

## Contrast
The contrast between text and its background should be at least 4.5:1 for normal size text and 3:1 for large scale text.

## ARIA Live Regions
Use `aria-Live` to define a region of dynamic content to make the screen reader announce updates to the content within in the region.

## A11Y Resources
* [I Used The Web For A Day Using A Screen Reader](https://www.smashingmagazine.com/2018/12/voiceover-screen-reader-web-apps/)
* [I Used The Web For A Day With Just A Keyboard](https://www.smashingmagazine.com/2018/07/web-with-just-a-keyboard/)
* [WCAG Standards](https://www.w3.org/WAI/standards-guidelines/wcag/)
* [WCAG Quickref](https://www.w3.org/WAI/WCAG21/quickref/)
* [The A11Y project](https://a11yproject.com/)
* [ARIA Live regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions#Simple_live_regions)
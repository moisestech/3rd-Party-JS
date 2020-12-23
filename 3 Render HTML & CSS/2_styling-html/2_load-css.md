# Load CSS

## 3.2.2 Loading CSS Files

For a modestly complicated widget, it may be more appropriate to style your elements
using a separate CSS file. Just like a regular web application, you can define all your
CSS rules in a single file (styles.css, for example). Unlike a regular web application,
you’ll need to load these CSS files dynamically via JavaScript, similar to how you
loaded additional JavaScript files back in chapter 2.
Loading CSS files dynamically is fairly simple. You just need to create a <link>
DOM element, set that element’s href attribute to point to your target CSS file, and
append the element to the page. CSS files always load asynchronously when injected
dynamically via JavaScript, so you won’t run the risk of blocking other files:

```javascript
function loadStylesheet(url) {
  var link = document.createElement("link");
  link.rel = "stylesheet";
  link.type = "text/css";
  link.href = url;
  var entry = document.getElementsByTagName("script")[0];
  entry.parentNode.insertBefore(link, entry);
}
```

Again, not very difficult, but there’s a catch. This code doesn’t know when the
stylesheet file has finished loading. If you try to render elements on the page before
their corresponding CSS rules are loaded, there could be a short period of time when
those elements are unstyled. In the web development world, this is referred to as
FOUC, or flash of unstyled content. It’s safe to say that FOUC is undesirable.
REQUIRED ATTRIBUTES FOR <LINK> Earlier in this book, we mentioned that you
could omit type="text/javascript" from <script> tags, and they’ll still
behave as expected—even in old browsers. Alas, the same isn’t true of <link>
tags. Both the type and rel attributes must be present in order for them to
function correctly in all browsers.

DETERMINING WHEN A CSS FILE IS LOADED
No problem, you’ll just add an event handler that will fire when the stylesheet is
loaded, just as you did in chapter 2 when loading additional JavaScript files. Except
you can’t. This is because CSS files are loaded using the <link> HTML element, and
not every browser has a load event that fires when a <link> element becomes loaded.

So to keep things browser compatible, you’ll need to rely on a trick, which is to contin-
ually poll the page until one of the rules in the CSS file becomes active.

To figure that out, you’ll first add an ID rule to your CSS file whose sole purpose is

to determine when the file is loaded. The rule will apply a specific color to any ele-
ment that declares that ID. After you’ve added the stylesheet to the page, you’ll create

an element with the ID and append it to the page. If at any point the element has the
specific color from your CSS file, you’ll know the file was loaded.
Here’s what the test CSS rule will look like:
#stork-css-ready {
color: #bada55 !important;
}
The actual color used isn’t important, as long as it’s relatively uncommon—#bada55
should fit the bill nicely. The accompanying !important keyword is used to prioritize
this rule over possibly conflicting CSS rules specified by other stylesheets on the page.
If you’re a CSS neophyte, don’t worry—we’ll cover !important and CSS specificity
later in this chapter.
Next up is the code to poll the page to find out when the test rule has become
active (listing 3.4). You won’t be using this snippet for the Camera Stork widget
because, in the next section, we’ll show you a slightly more efficient way of including
CSS on the page. But read through this listing nevertheless because, more often than
not, you’ll want to use this function in your own projects.

**Listing 4.34 Testing for the presence of a CSS rule**

There are a few important things to notice in this code. First, the two methods for
determining the color of the test element return different strings: the deprecated
Microsoft accessor (for IE8 and previous) preserves the original hex value, whereas
the W3C accessor converts the hex value to an RGB expression. You’ll need to check
for both. Second, the test element is given an inline color before it’s appended to the
page. This is to help prevent the rare case in which the publisher is already using your

magic test color (#bada55), such that the test element inherits that color upon enter-
ing the DOM. This would cause the isCssReady function to fire the callback parame-
ter before your CSS file is actually loaded. Even with these precautions, it’s still

conceivable that isCssReady could fire early, but that would require near-malicious
targeting of your test element by the publisher. And, in practice, that doesn’t happen.

Now that you’re armed with these two functions (loadStylesheet and isCss-
Ready), you have everything you need to load CSS files and defer HTML rendering

until they’re processed by the browser. There’s just one downside we haven’t consid-
ered: loading CSS files separately like this will incur an additional HTTP request by

your third-party script.

ALTERNATE TECHNIQUE: USING DOCUMENT.STYLESHEETS We’ve outlined one
technique for detecting whether a stylesheet has loaded, but as it happens,
there are others. Yepnope.js, a library for loading JavaScript files and other

assets, has an alternate technique that uses the document.styleSheets prop-
erty to scan for newly inserted stylesheets. If you’re curious, take a look at the

Yepnope.js source code to find out more: https://github.com/SlexAxton/
yepnope.js.

Now, we haven’t spoken much about performance yet in this book, but a well-
accepted rule of web application performance is to limit the number of HTTP

requests your code makes. Even though we’ve advocated separating your JavaScript
into logically separate files and loading them individually to aid development, they
can always be concatenated into a single library file when deployed into production.
Not so with CSS. Because it’s a different file type, it can’t be bundled with your
JavaScript into a single file. Or can it?

---

#### From [[_2_styling-html]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_styling-html]: _2_styling-html "Styling HTML"
[//end]: # "Autogenerated link references"

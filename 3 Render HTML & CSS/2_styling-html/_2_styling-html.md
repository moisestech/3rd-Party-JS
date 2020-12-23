# Styling HTML

## 3.2 Styling your HTML

At this point, if you were following our examples from sections 3.1.3 and 3.1.4, you

have your application’s HTML rendering on the publisher’s page, but it’s looking a lit-
tle grim (see figure 3.2). That’s because it’s bare-bones HTML without any styles. In

today’s world of overdesigned web applications, that
won’t cut it. You’ll need to style your widget’s HTML in
order for it to be presentable.

Styling your HTML is difficult for third-party applica-
tions in the same way that loading additional script files

is difficult: you don’t control the page’s HTML source
code, and are required to load styles dynamically from
your third-party JavaScript. There are three fundamental
ways to do this: inline style rules with your HTML,
dynamically loading accompanying CSS in separate files,
and embedding stylesheet rules in your JavaScript
source code. Let’s start with inlining styles.

## 3.2.3 Embedding CSS in JacaScript

A clever technique for loading styles as a third-party script is to embed an entire CSS
ruleset as a string inside your JavaScript code. When you’re ready to add the styles to
the page, you create a new <style> element, append the CSS string as a text node

inside, and then append the style element to the DOM. Because you can embed this

CSS string alongside your preexisting JavaScript code, you can sidestep loading a sepa-
rate CSS file, sparing your application an additional HTTP request.

To get a better sense of how this works, consider this simple CSS ruleset:
.stork-container {
width: 300px;
height: 300px;
}
.stork-container a {
text-decoration: none;
}
The goal is to take such a ruleset and convert it to a string that’s stored in a variable
somewhere in your JavaScript code. For example, these rules could be converted to
the following:
Stork.css = '.stork-container { width:300px; height:300px; }' +
'.stork-container { text-decoration: none; }';

Afterward, it’s just a matter of taking this string and injecting it as a style rule into the
page. We’ll get to that in a minute, but first, how do you actually convert a CSS file to a
JavaScript string like this?
CONVERTING CSS TO JAVASCRIPT STRINGS
You’ll need some kind of build tool that reads your CSS file, converts it to a JavaScript
string, and then embeds that string as a variable into your final, built JavaScript file.
This is probably not something you want to do during development; otherwise you’ll
have to rebuild your JavaScript every time you change a style rule. Embedding CSS is
thus a technique that’s best reserved for production use only.
Listing 3.5 shows a sample Python function that converts a CSS file to a JavaScript
string. It uses a clever CSS-parsing library for Python called cssutils (see http://
pypi.python.org/pypi/cssutils/). But like other server-side examples in this book, this
simple function could be written in any language.

**Listing 3.5 Python script for converting a CSS ruleset to a JavaScript string**

INJECTING CSS INTO THE DOM

After you have your CSS string in your code, next up is inserting it into the page (list-
ing 3.6). As we mentioned earlier, you’ll create a new <style> DOM element, append

the CSS string as a text node to it, and then append the style element to the page. This
style element can go anywhere in the page, so as you’ve done earlier, you’ll insert it
before a found script element.

And you’re done! The nice thing about embedding CSS this way is that you don’t need
a convoluted helper function to detect when the CSS is ready (polling the DOM for
style changes). You can execute your “ready” callback immediately after the CSS has
been injected; there’s no waiting.
The downside, in addition to having to incorporate a build step to convert your CSS
file to a JavaScript string, is that debugging your CSS is tougher than if you’d loaded CSS
separately. This is because most browser developer tools (Firebug, Chrome Dev Tools,
and so on) will consider your style rules as part of the parent page, instead of grouping
them under a file attributed to your domain. We don’t think this is a deal-breaker, but
it can be annoying.

At this point, you should know how to render your widget’s HTML on the pub-
lisher’s DOM, and style that HTML using one of the three techniques we just covered.

The Camera Stork widget should now be looking good;
it’s not bare-bones HTML, but a fully styled work of art
(see figure 3.3).
This example looks good, but that’s because it’s being
rendered on a blank test page without any other HTML
or CSS. If you were to deploy this on a fully-functioning
publisher’s website, there’s a good chance that a CSS rule
defined by the publisher might somehow conflict, and
make the whole thing look out of whack.
When it comes to rendering on somebody else’s DOM,
you can’t naively write HTML and CSS like you might for

your own self-contained web application. You’ve got to think carefully about how pre-
existing CSS and JavaScript code might affect your application. Let’s find out more.

---

#### From [[_html-css]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_html-css]: ../_html-css "3️⃣ HTML & CSS"
[//end]: # "Autogenerated link references"

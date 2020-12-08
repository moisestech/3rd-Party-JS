# Challenges

## 1.4 Challenges Of Third-Party Development

You’ve learned how third-party JavaScript is a powerful way to write highly distribut-
able applications. But writing scripts that run on other people’s websites carries a

unique set of challenges distinct from regular JavaScript programming. Specifically,

your code is being executed in a DOM environment that you don’t control, on a differ-
ent domain. This means you have to contend with a number of unexpected complexi-
ties, like an unknown web page context, a JavaScript environment shared with other

first- and third-party scripts, and even restrictions put in place by the browser. We’ll
take a quick look at what each of these entails.

## 1.4.1 Unkown Context

When a publisher includes your third-party script on their web page, often you know
little about the context in which it’s being placed. Your script might get included on pages that sport a variety of different doctypes, DOM layouts, and CSS rules, and ought
to work correctly in all of them.
You have to consider that a publisher might include your script at the top of their

page, in the <head> tag, or they might include it at the bottom of the <body>. Publish-
ers might load your application inside an iframe, or on a page where the <head> tag is

entirely absent; in HTML5, head sections are optional, and not all browsers automati-
cally generate one internally. If your script makes assumptions about these core ele-
ments when querying or appending to the DOM, it could wind up in trouble.

If you’re developing an embedded widget, displaying proper styles also becomes a
concern. Is the widget being placed on a web page with a light background or a dark

background? Do you want your widget to inherit styles and “blend” into the pub-
lisher’s web design, or do you want your widget to look identical in every context?

What happens if the publisher’s HTML is malformed, causing the page to render in
quirks mode? Solving these problems requires more than well-written CSS. We’ll cover
solutions to these issues in later chapters.

## 1.4.2 Share environment

For a given web environment, there’s only one global variable namespace, shared by
every piece of code executing on the page. Not only must you take care not to pollute
that namespace with your own global variables, you have to recognize that other

scripts, possibly other third-party applications like yours, have the capability of modify-
ing standard objects and prototypes that you might depend on.

For example, consider the global JSON object. In modern browsers, this is a native

browser object that can parse and stringify JSON (JavaScript Object Notation) blaz-
ingly fast. Unfortunately, it can be trivially modified by anyone. If your application

depends on this object functioning correctly, and it’s altered in an incompatible way
by another piece of code, your application might produce incorrect results or just
plain crash.
The following sample code illustrates how easy it is to modify the global JSON
object by using simple variable assignment:
JSON.stringify = function() {
/_ custom stringify implementation _/
};
You might think to yourself, “Why would anyone do such a thing?” Web developers
often load their own JSON methods to support older browsers that don’t provide

native methods. The bad news is that some of these libraries are incompatible in sub-
tle ways. For example, older versions of the popular Prototype JavaScript library pro-
vide JSON methods that produce different output than native methods when handling

undefined values:
// Prototype.js
JSON.stringify([1, 2, undefined])
=> "[1, 2]"

// Native
JSON.stringify([1, 2, undefined])
=> "[1, 2, null]"
The JSON object is just one example of a native browser object that can be altered by
competing client code; there are hundreds of others. Over the course of this book
we’ll look at solutions for restoring or simply avoiding these objects.
Similarly, the DOM is another global application namespace you have to worry

about. There’s only one DOM tree for a given web page, and it’s shared by all applica-
tions running on the page. This means taking special care when you interact with it.

Any new elements you insert into the DOM have to coexist peacefully with existing ele-
ments, and not interfere with other scripts that are querying the DOM. Similarly, your

DOM queries can inadvertently select elements that don’t belong to you if they’re not
scoped properly. The opposite is also true; other applications might accidentally
query your elements if you haven’t carefully chosen unique IDs and class names.
Since your code exists in the same execution environment as other scripts, security
also becomes a taller order. Not only do you have to protect against improper use by
users of your application, you also have to consider other scripts operating on the
page, or even the publisher, to be a potential threat. For example, if you’re writing a
widget or script that ties to a larger, popular service, like a social networking website,
publishers might have a vested interest in attempting to fake user interactions with
their own pages.

## 1.4.3 Browser Restrictions

If an unknown document context, multiple global namespaces, and additional secu-
rity concerns aren’t bad enough, web browsers actively prohibit certain actions that

often directly affect third-party scripts. For example, AJAX has become a staple tool of
web developers for fetching and submitting data without refreshing the page. But the
web browser’s same-origin policy prevents XmlHttpRequest from reaching domains
other than the one you’re on (see figure 1.9). If you’re writing a third-party script that
needs to get or send data back to an application endpoint on your own domain, you’ll
have to find other ways to do it.
In the same vein, web browsers also commonly place restrictions on the ability of
applications to set or even read third-party cookies. Without them, your users won’t be
able to “log in” to your application or remember actions between subsequent
requests. Depending on the complexity of your application, not being able to set or
read third-party cookies can be a real hindrance.

**Figure 1.9** Failes cross-domain AJAX request from localhost to google.com in Google Chrome

Unfortunately, this list of challenges is just the tip of the iceberg. Third-party
JavaScript development is fraught with pitfalls, because at the end of the day, the web

browser wasn’t built with embedded applications and third-party code in mind. Brows-
ers are getting better, and new features are being introduced that alleviate some of the

burden of doing third-party development, but it’s still an uphill battle, and supporting
old browsers is typically a must for any kind of distributed application.
But don’t worry. You’ve already made the fine decision of purchasing this book,
which will cover the problems facing your third-party JavaScript code. And as an
added bonus, we’ll even tell you how to solve them.

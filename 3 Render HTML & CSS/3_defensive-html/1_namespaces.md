# Namespaces

## 3.3.1 Namespaces

If you look back at the source code examples from this chapter, you might have
noticed that all DOM IDs, classes, data-\* attributes, and matching CSS selectors have
been prefixed with stork-. The purpose? To reduce the likelihood of those attributes
conflicting with the parent page.
Consider the following situation. Your widget has a top-level <div> element that
acts as a container. It does this by setting an explicit width and height, effectively
bounding the area taken up by your widget. You’ve given this <div> a straightforward
class name, container, which matches a style rule in your accompanying CSS:

```html
<div class="container">...</div>

<style>
  .container {
    width: 200px;
    height: 200px;
  }
</style>
```

This might be perfectly appropriate for a regular stay-at-home application, but for a
third-party app, it’s a complete no-no. The reason? Such a generic class name has a
good chance of already being used by the parent page. If you introduce this style rule,
you might override an existing style rule put in place by the publisher and ruin their
site layout. Or, on the flip side, their rule might override yours and resize your widget
inadvertently.

The solution? Prefixing all of your class names (and other attributes) with an iden-
tifier unique to your application—a namespace. In the case of the Stork widget, the pre-
vious markup should be amended to look like this:

```html
<div class="stork-container">...</div>
<style>
  .stork-container {
    width: 200px;
    height: 200px;
  }
</style>
```

If this looks familiar, it should. In chapter 2 you namespaced your JavaScript code so
that you don’t declare global objects that conflict with code running on the parent
page. This is the same idea, and it extends to every piece of HTML you insert into the
page: IDs, classes, data-\* attributes, form names, and so on.
Namespacing HTML and CSS is a must for any third-party application that renders
directly to the publisher’s page. This isn’t just necessary for preventing conflicting CSS
rules; it’s also conceivable that the parent page has JavaScript that’s querying the DOM
for elements whose identifying properties might match your own. Be rigorous in
namespacing everything you put on the DOM!

---

#### From [[_3_defensive-html-css]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_defensive-html-css]: _3_defensive-html-css "Defensive HTML & CSS"
[//end]: # "Autogenerated link references"
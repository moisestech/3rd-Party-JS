# Inline Styles

## 3.2.1 Using Inline Styles

The most straightforward way of styling your HTML is to inline your style rules directly
on HTML elements. This means declaring CSS inside an element’s style attribute. For

example, for a simple version of the Camera Stork widget, you might insert the follow-
ing HTML:

<div style="width:200px">
<h3 style="font-size:18px">Mikon E90 Digital SLR</h3>
<img src="http://camerastork.com/img/1337-small.png"
style="border:1px solid #000"/>
<p style="font-weight:bold">$299.99</p>
<p>4.3/5.0 &bull; 176 Reviews</p>
</div>
Not the most glamorous way to style elements, but it works. There’s one major upside:
HTML styled this way is unlikely to conflict with styles from the publisher’s page. We’ll
talk more about this later in the chapter, but by foregoing a separate stylesheet (where
elements are targeted by tag names, class names, IDs, and so on), you’re almost
guaranteed not to accidentally style parts of the publisher’s page that are outside the
area occupied by your widget. Similarly, you’re unlikely to have your widget’s styles

modified by a conflicting rule defined by the publisher, because your style rules are
placed directly on the elements they affect.
The reasons for not inlining CSS? They’re no different than if you were writing a
regular stay-at-home web application. Depending on the complexity of your widget,
you might need to redundantly repeat a lot of style attributes throughout your HTML.
This repetition gets worse if you’re developing multiple widgets; without any reusable
CSS, everything will have to be repeated. Needless to say, inlining CSS can quickly
become unmaintainable, and possibly cause your designer to cry at night.
But that doesn’t mean inlining CSS is out of the question. If you have a particularly
simple application, the bonus of having conflict-free styles may outweigh the pain of
maintaining inline attributes.

---

#### From [[_2_styling-html]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_styling-html]: _2_styling-html "Styling HTML"
[//end]: # "Autogenerated link references"

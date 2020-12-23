# CSS Specificity

## 3.3.2 CSS Specificity

It’s important to note that, though helpful, namespacing your HTML and CSS only
prevents cases where the publisher is using styles or queries that reference attributes
with the same name as yours. Unfortunately, your widget can still conflict with styles
defined by the parent page, even if their CSS uses IDs, class names, and attributes that
don’t directly reference your elements. This is because some CSS rules are weighed
more heavily by the browser, and can take precedence over seemingly unrelated rules
you might define. This phenomenon is referred to as CSS specificity, and you’ll need to
understand it before you can safely render elements on the publisher’s page.
Let’s go back to the container example from the previous section on namespaces.
Suppose the publisher’s HTML has a top-level DIV that wraps all their content, with an
ID of page:

<div id="page">
... <!-- Publisher content ->

<div class="stork-container">
... <!-- Stork content ->
</div>
</div>
Additionally, let’s say the page has the following CSS, where the first rule is defined by
the publisher, and the second rule (targeting stork-container) is added by your
third-party script:
/* Publisher */
#page div {
background-color: green;
}
/* Camera Stork */
.stork-container {
background-color: blue;
}
Now, what color will .stork-container have? The answer might shock and appall
you: green. In this simple example, the publisher rule (#page div) takes priority over
your third-party application’s class rule (.stork-container). This happens because

the browser weighs rules containing IDs higher than those that target classes or attri-
butes.

CSS RULE PRIORITIES
The W3C CSS specification outlines how browsers are meant to prioritize different rule
types. Here’s a list of these rule types, ordered from highest priority to lowest:
1 Inline styles (style="...")
2 IDs
3 Classes, attributes, and pseudo-classes (:focus, :hover)
4 Elements (div, span, and so on) and pseudo-elements (:before, :after)
According to this chart, inline styles are weighed above all subsequent rule types: IDs,
classes, and elements. This continues logically down the list, with IDs prioritized

higher than classes and elements, and so on. There’s one exception to this list: prop-
erties tagged with the !important keyword take highest priority. But note that the

!important keyword affects a single property within a rule, not the entire rule.
What happens when you have multiple CSS rules of the same weight, each of which
could conceivably affect the same element? Let’s take a look at an example:

<div class="stork-container">
<span class="stork-msg">Eat your vegetables!</span>
</div>
<style>
.stork-container { background-color: blue; }
.stork-container span { background-color: red; }
.stork-container .stork-msg { background-color: yellow; }
</style>

What do you suppose the color of the span is? The answer again might be surprising:

yellow. Even though these rules are all primarily class-based, the second rule (.stork-
container span) is considered more specific than the first rule, and the third rule

(.stork-container .stork-msg) more specific than the second. How does this work?
INLINE STYLES ARE KING In terms of CSS specificity, that is. If you recall from
earlier in this chapter, we mentioned that inline styles have the benefit of
rarely conflicting with the parent page. Now it’s clear why: they’re prioritized
over every other type of regular CSS rule (excluding those with the
!important keyword). If you’re writing a particularly simple widget, it might
not be a bad idea to use inline styles; you’ll avoid most CSS specificity conflicts.
The browser uses a simple scoring system to determine which rule takes priority. For a
given rule, each selector composing that rule is worth a certain value. Those values are
summed to create a specificity score. When multiple rules affect the same element,
the browser compares each rule’s specificity score, and the rule with the highest score
takes priority. In the case of a tie, the rule that was defined last wins.

Table 3.1 revisits those CSS rule types, this time with their corresponding specific-
ity scores.

You’ll quickly notice these aren’t ordi-
nary numbers. A specificity score is actually

a tuple of the form (a, b, c, d), with a being
more valuable than b, b being more valuable
than c, and so on. That means that a style
caused by a single inline style attribute (1, 0,
0, 0) has higher specificity than a rule with
one hundred ID selectors (0, 100, 0, 0).

So, looking back at our previous exam-
ple, those CSS rules would have been assigned the following scores, with the highest-
scoring rule being prioritized by the browser:

 .stork-container (0,0,1,0—one class selector)
 .stork-container span (0,0,1,1—one class selector, one element selector)
 .stork-container .stork-msg (0,0,2,0—two class selectors)
At this point, you should have a good handle on how CSS specificity works, and why
the browser prioritizes some rules over others. You’ll next put this knowledge to use,

as we explore some approaches for writing CSS that stands tall in the face of conflict-
ing publisher styles.

---

#### From [[_3_defensive-html-css]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_defensive-html-css]: _3_defensive-html-css "Defensive HTML & CSS"
[//end]: # "Autogenerated link references"

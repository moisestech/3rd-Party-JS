# Overspecifying CSS

## 3.3.3 Overspecifying CSS

The first and simplest approach to writing CSS that doesn’t conflict with the pub-
lisher’s page is to overspecify your rules. This means declaring additional selectors to

boost the specificity of your rules, such that when the browser compares your rules
against those from the parent page, they’re likely to score higher and be prioritized.

Let’s look at this in practice. Consider this revised example of the Stork widget
container, now sporting two container <div> elements, each with a unique ID:

```html
<div id="stork-main">
  <div id="stork-container">
    <h3 class="stork-product">Mikon E90 Digital SLR</h3>
    <img src="http://camerastork.com/img/products/1337-small.png" />
    <p class="stork-price">$599</p>
    <p class="stork-rating">4.3/5.0 &bull; 176 Reviews</p>
  </div>
</div>
```

The accompanying CSS for this HTML could then look like this:
#stork-main #stork-container { ... }
#stork-main #stork-container .stork-product { ... }
#stork-main #stork-container .stork-price { ... }
By redundantly specifying both container IDs as parent selectors of all your CSS rules,
you’re effectively giving each of your CSS rules a minimum specificity score of (0,2,0,0).
Afterward, the publisher’s generic #page rule from earlier will no longer conflict with

your widget, because it only uses a single ID. Neither will any purely class- or element-
based rules conflict, because those are an entire CSS weight class below IDs. Even

though, for selection purposes, it’s completely unnecessary to specify a second ID for
your rules, here it works as an effective device for boosting specificity.
PRESERVE YOUR SANITY WITH CSS PREPROCESSORS Writing overspecified CSS
can be a real drag: you have to constantly rewrite the same IDs over and over

again for each one of your CSS rules. You can remedy this by using a CSS pre-
processor, which extends the CSS language with additional features like the

ability to declare nested hierarchies of rules. For example, using the LESS CSS
preprocessor, you could write the previous example like this:

```css
#stork-main {
  #stork-container {
    .stork-product {
      ...;
    }
    .stork-price {
      ...;
    }
  }
}
```

A number of popular CSS preprocessors are available today, all of which have
varying feature sets. Among the most popular are LESS (http://lesscss.org), Sass
(http://sass-lang.com), and Stylus (http://learnboost.github.com/stylus/).
On the flip side, this example requires that your widget use top-level containers with
IDs, which won’t be practical for widgets that can be rendered multiple times on the
same page. Additionally, it’s still not bulletproof: a publisher could follow your lead
and overspecify their own CSS rules, resulting in the same problem you had before.
But this is an unlikely scenario, especially since you’ve redundantly specified two IDs
in each of the rules (you could alternatively use one, but this will of course be more
vulnerable). The reality is that most publishers use sane CSS rules, and overspecifying
your rules like this will be compatible with most of them.

OVERSPECIFYING CSS DOESN’T MIX WITH CODE QUALITY TOOLS If you take to
overspecifying your CSS like this, you might find an unlikely enemy: tools that
evaluate the quality of your CSS code, such as CSS Lint, Google Page Speed,
and Yahoo’s YSlow. These tools will indicate that you’re making redundant
CSS selectors, and they’ll advise you to remove such selectors to reduce file
size and improve browsers’ CSS performance. Unfortunately, these tools
aren’t programmed with third-party scripts in mind, and don’t fairly evaluate
the usefulness of overspecifying CSS. The benefits of overspecification (for

third-party applications) will outweigh the extra file size and minuscule per-
formance hit.

ABUSING !IMPORTANT
If you feel that overspecifying your CSS with extra ID (or class) selectors doesn’t go far
enough, you can break out the nuclear option: the !important keyword. As we covered
earlier, properties within a CSS rule that sport the !important keyword are prioritized
highest of all, even above inline styles. This is because the !important keyword was
designed to give browser users a surefire way to override “author” (publisher) styles, in
the case of browser plugins or site-specific styles. You can abuse !important by using it
on all of your CSS properties, effectively prioritizing them over all other rules.
Here’s how you might use the !important keyword on a single CSS rule:

```css
.stork-price {
  font-size: 11px !important;
  color: #888 !important;
  text-decoration: none !important;
  display: block !important;
}
```

Since it’s per property, the !important keyword needs to be repeated like this, which
can become a drag over a long and complex stylesheet. But, in exchange, you get a
rock-solid set of stylesheets that are very unlikely to be reset by the publisher’s page.
It’s still conceivable that the publisher could in turn use !important to target your
elements and set their own styles, at which point they’re likely purposely targeting
your elements for customization. On one hand, that can be frustrating if you’re trying
to maintain a consistent look and feel. But, if you’ve decided to allow publishers to
customize your widget, this is probably desired behavior.

One thing should be clear: sharing the DOM with the publisher can make it partic-
ularly difficult to render a consistently styled widget. Although you can take steps to

overspecify your CSS rules to reduce the likelihood of conflicts, it’s always possible for
the publisher to target your elements with their rules, either accidentally or purposely.
But if sharing the DOM with the publisher is what’s causing so much grief, is it possible
to render your widget off of the DOM? Why, yes—yes you can.

---

#### From [[_3_defensive-html-css]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_defensive-html-css]: _3_defensive-html-css "Defensive HTML & CSS"
[//end]: # "Autogenerated link references"

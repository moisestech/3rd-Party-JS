# Custom Data Attributes

## 2.5.3 **Using custom data attributes**

Another technique for passing parameters is to store them as part of the script include
snippet as HTML5 custom data attributes (often known as data-\* attributes). In the case
of the Stork widget, this means storing the product ID as a custom attribute on the

<script> tag itself.
Let’s take a look at the amended Camera Stork code.

**Listing 2.8 Embedding the product ID in custom data attributes**

You can see that we’ve added an attribute to the script include snippet, named data-
stork-product-id. This attribute contains the product ID. The name of the data-*

attribute is prefixed with stork so that it’s less likely to conflict with other JavaScript
code.

WHAT ARE DATA-* ATTRIBUTES? HTML5 introduces a new feature, data-* attri-
butes, which are custom attributes for embedding data in HTML elements.

They’re simple to use; any attribute prefixed with the data- token is consid-
ered a custom attribute and ignored by the browser. Data attributes are sup-
ported by nearly every browser; even older browsers that don’t explicitly

recognize data-* attributes work correctly because they ignore unknown attri-
butes by default.

Next up, your third-party application code needs to query the DOM to locate the script
element containing this attribute. This should feel similar to the examples from the
query string and fragment identifier techniques.

**Listing 2.9 Locating and extracting the data-stork-product-id attribute value**

Fairly straightforward, right? One of the benefits of using data-* attributes like this is

that they can also help your script identify the DOM location of the script include snip-
pet on the publisher’s page. This is especially valuable if the script include snippet’s

location denotes where your third-party application ought to render itself. We’ll
explore this in greater detail in the next chapter.
THE DATA-* JAVASCRIPT API As part of HTML5, the W3C has defined useful
interfaces for accessing data-* attributes in JavaScript. For example, you could
instead use the dataset DOM property to access the data-stork-product-id
attribute from listing 2.9:
id = scripts[i].dataset.dataStorkProductId;
Unfortunately, browser support for the dataset property is still limited—no

version of Internet Explorer currently supports it. In the meantime, we rec-
ommend doing things the old-fashioned way, using the getAttribute DOM method.

---

#### From [[_5_passing-args]]

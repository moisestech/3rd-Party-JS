# Using Fragment ID

## 2.5.2 Using the Fragment Identifier

There’s a way to pass parameters to your script as part of the URL without sending the
product ID to the server. Instead of including the product ID in the URL’s query string,
you can pass it as part of the URL’s fragment identifier. The fragment identifier is the
last part of the URL, and includes everything that comes after the hash (#) character.
It’s traditionally used to identify a portion of a document, and unlike the query string,
isn’t sent to the server by the browser.
Here’s the script URL again, this time embedding the product ID in the fragment
identifier:
http://camerastork.com/widget.js#product_id=1234
Amending the earlier code to work with the fragment identifier is simple. You’ll still
obtain the full URL of the script element using the same getScriptUrl function from
earlier. Then you just need a different regular expression to separate the fragment
identifier from the full URL before calling getQueryParameters:
var params = getQueryParameters(url.replace(/^.\*\#/, ''));
Remember that parameters stored on the fragment identifier of the URL aren’t passed
to the server. For the purposes of the product widget, this is great; the base URL you
distribute with your script include snippet is the same for each product, which makes
caching your script file on the client easier. But for other third-party applications that

actually need these values passed to the server, you’ll want to stick with the conven-
tional query string.

We’ve looked at two techniques for passing parameters to your script. One passes
those parameters to the server; the other is strictly available on the client. We could
stop here, and continue with developing the rest of the widget. But we’ll cover some
additional techniques that may better suit your tastes.

---

#### From [[_5_passing-args]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_5_passing-args]: _5_passing-args "Passing Args"
[//end]: # "Autogenerated link references"

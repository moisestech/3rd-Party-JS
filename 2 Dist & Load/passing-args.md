# Passing Args

## 2.5 Passing script arguments

Let’s recap. You’ve given the publisher the script include snippet, which they’ve added
to their web page’s HTML source code, and which is causing your application’s initial

script file to load on their page. After the script is loaded, it loads any additional sup-
porting JavaScript files. There’s another step in the application-loading process that

you’ll need to tackle before continuing: how to pass configuration parameters from
the publisher page to your application code.
Remember, the primary goal of the widget is to display product information for an
item that already exists in your product catalog. You’ll need to somehow identify this

product in the publisher script include snippet before you can retrieve any informa-
tion about it. We’ll look at four ways to do that: by embedding the product ID in the

target script URL’s query string; in the script URL’s hash fragment component; using
HTML5 custom data attributes; and separately, using global variables.

## 2.5.1 Using the query string

The most straightforward way of passing parameters to your script is using the query

string component of the script’s URL. If you recall the script include snippet from sec-
tion 2.2, the script URL target included a query string component with a product ID

parameter:

<script>
(function() {
var script = document.createElement('script');
script.async = true;
script.src = 'http://camerastork.com/widget.js?product=1234';

var entry = document.getElementsByTagName('script')[0];
entry.parentNode.insertBefore(script, entry);
})();
</script>

You might remember that this is the same parameter-passing technique we covered in
chapter 1 with the weather widget example. In that example, a server-side Python
application read the ZIP code from the HTTP request’s query string. The application
then queried the database for the relevant weather data and generated JavaScript to
output the result on the publisher’s page.6
This time around, we’ll try a different approach. Instead of relying on a server-side
script to obtain the passed parameter, you’ll retrieve the parameter using strictly
client-side JavaScript.

## Working with HTTPS URLs

So far, all of the script-loading examples we’ve covered use strictly the http://
protocol for URLs. This will work fine for 99% of websites, but some publishers may
be serving their content using https:// (HTTP Secure). This is a secure protocol that
encrypts content between the server and the browser.

In order to load your application properly on these websites (and avoid “insecure con-
tent” warnings), your application will also need to be served using HTTPS. This

requires two things: configuring your servers to support HTTPS, and having your script
include snippet (and all other URLs) point to the correct protocol. One technique is to
check the protocol of the current page, and defer to the appropriate URL:
var secure = window.location.protocol === 'https:';
script.src = (secure ? 'https' : 'http') +
'://camerastork.com/widget.js';

Alternatively, you can use what are known as protocol-relative URLs. These automat-
ically resolve to the parent page’s protocol:

script.src = '//camerastork.com/widget.js';
On http:// websites, this URL will resolve to http://camerastork.com/widget.js. On
https:// websites, it’ll instead resolve to https://camerastork/widget.js.
Protocol-relative URLs are more elegant and work in all browsers, but be careful—
they can sometimes behave unexpectedly. For example, if you’re viewing a web page
on your local filesystem using the file:// protocol, this example would resolve to
file://camerastork.com/widget.js, a URL that (probably) doesn’t exist. There are
also issues where some resources (like <link> tags) will load twice when using
protocol-relative URLs on HTTPS pages.6

Remember that before you can use either technique, you have to configure your serv-
ers to handle both HTTP and HTTPS. We’ll talk more about HTTPS later in this book.

This is trickier than you might expect. This is because the JavaScript file that’s
being served has no innate knowledge of the URL it was served from. Your first instinct

might be to access the script’s URL using window.location.href, but you’ll be unsuc-
cessful. This is because window.location.href is the URL of the page that’s including

the JavaScript file, not the URL of the JavaScript file itself.
There’s a clever technique for obtaining the script URL. It relies on the fact that
the script DOM element that loads your third-party script can be queried on the DOM
like any regular HTML element. You can use document.getElementsByTagName to
return a list of all script elements on the page, and iterate through them until you find
the script element whose URL points to your script file. The following listing presents
a function that does exactly that.

**Listing 2.6 Getting the script URL**

**Listing 2.7 Extracting query parameters**

The final code for extracting the product ID from the script include URL looks like
this next snippet. Note that you can separate the query string from the full URL using
a simple regular expression:
var url = getScriptUrl();
var params = getQueryParameters(url.replace(/^.\*\?/, ''));
var productId = params.product;
At this point, your script has the ID of the product it needs to request information
from the server. All that remains is to make some kind of AJAX request for that data,
and render the corresponding HTML on the page.
But using the query string in this fashion has a significant drawback. You’ll need to
distribute a different script include snippet for each product, because the product ID
component of the URL will change for each one. This will make caching your

JavaScript file difficult, because the web browser will treat every new URL as a brand-
new resource, even if the query string doesn’t alter the code that’s being returned.

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

## 2.5.3 Using custom data attributes

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


## 2.5.4 Using global variables

In the past few sections, we’ve explored using the DOM as a means of passing parame-
ters to your third-party application. An alternate approach is to avoid the DOM

entirely, and instead use global variables. Any variables declared in the browser’s top-
most variable scope are attached to the window object, and are accessible by any script

running on the page.
The next listing changes the script include snippet from section 2.1 to declare a
global variable whose purpose is to pass the product ID to your third-party script.

**Listing 2.10 Passing script arguments using global variables**

In your third-party script file (widget.js), you can just read the contents of the
product_id variable, and you’re on your way to loading that product’s current vote
rating and rendering it into the page.
There’s a catch: global variables like this are shared by all scripts executing on the
page, and can be read or altered by any of them. Similarly, by setting this value, you
might be overwriting a variable that’s relied on by another script loaded elsewhere on
the publisher’s page.
A better idea is to put the global variable inside your application namespace. That
will reduce variable conflicts with other scripts, because the likelihood of another
script sharing your branded namespace is small. You’ll have to initialize the
namespace first, because the widget.js file where the namespace was previously
declared won’t have been loaded yet:

Namespacing the script parameter like this will reduce conflicts, but there’s still a sig-
nificant downside: you can only define one product_id parameter per page. If the

publisher tries to include the script include snippet a second time for a different prod-
uct, the second product_id assignment might overwrite the first value by the time the

first script file is loaded.

ASYNCHRONOUS SCRIPT INCLUDES DON’T PRESERVE EXECUTION ORDER Remem-
ber, we’re loading the third-party script asynchronously to improve page per-
formance. That means that there’s no predicting when the script will finish

downloading and run. It also means that the browser doesn’t maintain execu-
tion order of asynchronous scripts, which can lead to race conditions if you’re

not careful.
GLOBAL VARIABLE ARRAYS
To avoid global variable collisions, you can instead pass parameter variables in global
arrays. If a second script include detects earlier parameter declarations, it merely
appends to the array. When the script loads, it can iterate over the array of parameters
and execute the rendering method for each passed ID:

Now you have your bases covered. The downside is that the script include snippet has
grown by three lines of code—not a big deal. The upside is that the script include
snippet can be included many times on the page, and you’ll have an array of product
IDs to work with.
Is there a benefit to using global variables to pass parameters over embedding
them in the DOM, either via the query string or fragment identifier, or using data-*

attributes? Sure. For starters, you can embed any type of JavaScript object as a parame-
ter, like a dictionary object. And embedding parameters as global variables is, debat-
ably, more clear to publishers than appending them to the end of a long URL. Last,

unlike the other techniques we covered, you don’t need to query the DOM in order to
extract the parameters—they’re already initialized as JavaScript variables for you.
When performance matters above all else, global variables may be the best choice.
Table 2.1 summarizes the pros and cons of the various techniques.

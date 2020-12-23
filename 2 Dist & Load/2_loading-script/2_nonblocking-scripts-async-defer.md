# Nonblocking Scripts Async Defer

## 2.2.2 Nonblocking Scripts with Async and defer

Browser vendors have long recognized that synchronous script loading isn’t ideal. To
remedy this, the W3C has introduced two helpful attributes for the <script> tag—
defer and async —that indicate that the file can be downloaded without blocking the
browser.
THE DEFER SCRIPT ATTRIBUTE
The first of these, defer, was first introduced as part of HTML4. When specified on a

<script> tag, it tells the browser that the script file won’t generate any document con-
tent (using document.write), and can safely be downloaded without blocking the page. Then, when the browser is finished processing the page, it executes any
deferred scripts in the order they were encountered (see figure 2.3).
The following is an example of the same <script> tag we saw earlier, this time
sporting the defer attribute:
<script defer
src="http://camerastork.com/widget.js?product=1234"></script>

The defer attribute has been around for some time, and enjoys pretty broad support
among major browsers. The only notable exception is Opera, for which the attribute
is ignored, causing the <script> tag to be treated as a regular blocking one.
XHTML AND REQUIRED ATTRIBUTE VALUES If this value-less use of the defer
attribute looks funny, it might be because you’re used to working with
XHTML. In an XHTML world, the defer example would be written like this:

<script defer="defer"
src="http://camerastork.com/widget.js?product=1234"></script>

This is because XHTML requires attributes to have values, whereas HTML
doesn’t. The code examples in this book are all written in HTML and not
XHTML.

**Figure 2.3 Two Scripts (A and B) loaded using both the defer and async attributes. Files loaded with the async attribute execute as soon as they're downloaded, whereas files loaded with defer wait unitl the docuemnt is fully parsed.**

THE ASYNC SCRIPT ATTRIBUTE
The second attribute, async, is a more recent feature of HTML5, and behaves slightly
differently than defer. Again, it indicates that the downloaded file won’t call document
.write and can be downloaded as the page is being processed. But unlike defer,
which executes the file only after the page is completely parsed, scripts loaded with
the async attribute are executed as soon as they’re downloaded—whether the page is
finished processing or not. This means that async scripts can potentially execute
sooner than scripts loaded using defer (see figure 2.3).

Script A request
Web page Web server Web page

Script B request
Script B response

Document ready
Scripts A and B
executed

Script A response

Script A request
Web server

Script B request
Script B response Script B executed
Script A executed Script A response

Figure 2.3 Two scripts (A and B) loaded using both the defer and async attributes.
Files loaded with the async attribute execute as soon as they’re downloaded, whereas
files loaded with defer wait until the document is fully parsed.

Download from Wow! eBook <www.wowebook.com>

Loading the initial script 29
Here’s the widget script include one more time, using the async attribute:

<script async
src="http://camerastork.com/widget.js?product=1234"></script>

Though both async and defer are helpful attributes that prevent your script from
blocking the publisher’s page, we feel that async is the better choice for third-party
scripts. Because async scripts can execute before the page is finished processing, they
enable your script files to initialize and run your application as soon as possible.
Deferred scripts, on the other hand, could spend a long time waiting for the browser
to finish processing the page.
Alas, like many HTML5 features, the downside to the async attribute is that it isn’t
supported by every browser. You’ll find that only “modern” browsers like Firefox 3.6+,
Chrome, Safari, and Internet Explorer 10 recognize it. That leaves plenty of old, but
still actively used, browsers that don’t support the async attribute. But we’re not out of
luck: there’s another way to load scripts asynchronously that’s backward compatible
with such older browsers.
WHEN TO USE BLOCKING SCRIPTS Synchronous scripts aren’t all bad. There
are a few instances where you’ll want a blocking <script> tag. For instance, if
you need to render HTML to the page before anything else renders, you’ll
want to use a blocking script. Asynchronous scripts that render new elements

might not do so until after the page has mostly loaded, possibly causing ele-
ments to reflow and temporarily look poor.s

---

#### From [[_2_loading-script]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_loading-script]: _2_loading-script "Loading Script"
[//end]: # "Autogenerated link references"
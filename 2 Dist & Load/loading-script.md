# Loading Script

## 2.2 Loading the initial script

Now that you’ve sorted out your development environment, it’s time to tackle how
you’ll load your scripts on the publisher page. You’ll do this by using what we call the
script include snippet.
The script include snippet is one of the most important pieces of your application.
It’s the code that’s distributed to publishers and will actually load your code on their

web page. Not only does it have to work, but it has to load your files in the most effi-
cient way possible. And there’s a penalty to getting it wrong, because getting publish-
ers to update their web pages with a new version later is a painful undertaking.

We’ll look at two include snippets that you can use to load the initial script on your
publisher’s page: a standard, “blocking” <script> tag include, and an asynchronous
script include.

## 2.2.1 Blocking Script Includes

Let’s start with the basics. You should already be familiar with how to load scripts using

the standard HTML <script> tag. Using it to load your initial script file is straightfor-
ward: just give publishers a standard <script> tag to paste into their HTML source

code whose src attribute points to your JavaScript file. This is the simplest, smallest

amount of code you could give to your publishers. If you recall, we used this in chap-
ter 1 in the weather widget example.

Here’s what a standard script include would look like to load a single instance of
the product widget:

<script src="http://camerastork.com/widget.js?product=1234"></script>

The src attribute points to a JavaScript file, widget.js, that’s hosted on your servers at
http://camerastork.com. Embedded in the URL’s query string is a product ID. This
ID is how you’ll identify which product to render on the publisher’s page. Pretty
straightforward.
There’s no question this method will achieve the desired effect of loading your
script on the publisher’s page, but it comes with a major drawback. A regular

<script> tag include like this is considered blocking or synchronous. This means that

the browser will stop processing the page while it waits for the script file to be down-
loaded and executed. This is because the script might modify the page contents using

document.write. The browser can’t safely continue processing until any and all out-
put from document.write has been captured.

This is illustrated in the following example HTML. The browser waits for widget.js
to finish before rendering the header or executing application.js:
<body>
<script
src="http://camerastork.com/widget.js?product=1234"></script>
<p>Hello, world</p>
<script src="/application.js"></script>
</body>

Blocking script elements can slow down publishers’ pages because they stop the
browser from continuing if the target file is slow to download—or worse, unavailable.
The higher the publisher places the script include in their HTML source, the more
pronounced the effect. For example, if the publisher decides to place the widget at

the top of their website, the remainder of the page won’t get rendered until the wid-
get script file loads. The worst possible result is if the publisher adds your script

include to their <head> section. This could block the browser from reaching the

<body> tag, such that visitors will be stuck with a blank page until the script resolves.
LIMITING THE IMPACT OF BLOCKING SCRIPTS
One way to limit the effect of blocking scripts is to advise publishers to place the script
include at the end of their HTML source. That way, if your script is slow, it won’t matter
(as much) because the browser has already rendered the page. The downside is that

you can’t rely on the script element’s position in the DOM to determine where to ren-
der your widget. But you can always ask publishers to identify the target render loca-
tion separately using a uniquely identified element:

<body>
<div id="camerastork-widget"/>
<p>Hello, world</p>
<script
src="http://camerastork.com/widget.js?product=1234"></script>
</body>
In this example, when widget.js loads, it’ll render the product widget and append it to
the <div> element with id="camerastork-widget". From a performance perspective,
this is better than the earlier script include, because it doesn’t block the publisher’s
page (although the total load time will be about the same). But this is contingent on
the publisher correctly placing the script include at the end of their HTML source.
And experience tells us this isn’t always a given.

The bigger takeaway is that if widget.js isn’t outputting HTML in-place, it’s not mak-
ing use of document.write. And if widget.js isn’t using document.write, the browser

should theoretically not have to pause rendering while it’s executing. But unfortu-
nately the browser doesn’t know this; only we do. What if there were a way to tell the

browser that our script can be loaded without blocking the page?

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
ments to reflow and temporarily look poor.

## 2.2.3 Dynamic Script Insertion

It turns out you can re-create the behavior achieved by the async attribute by dynami-
cally creating a script DOM element in JavaScript and appending it to the publisher’s

page. Because you can append this script element to an arbitrary DOM location, even

one that has already been processed by the browser, browsers don’t preserve execu-
tion order for JavaScript loaded in this fashion. And because execution order isn’t

preserved, the browser downloads these files in parallel. This is your path to asynchro-
nous script loading in browsers old and new.

Here’s how the script include snippet looks using dynamic <script> tag insertion.

**Listing 2.1 Asynchronous Script Include**

Let's discuss some interesting points about this example. You’ll notice that the script
element’s async attribute is set to true. This isn’t just for posterity—Opera and some

older versions of Firefox require this attribute to be set in order for the script to be exe-
cuted as soon as it is downloaded. Otherwise, Opera and Firefox will attempt to pre-
serve execution order (similar to the defer attribute). Second, you’ll notice that this

entire snippet is wrapped in an immediately-invoked function expression, or IIFE. This

prevents the script and entry variables from leaking into the global scope. Remem-
ber: this code snippet is executing on the publisher’s page, which could be home to

any number of additional, unknown scripts. It’s best to avoid declaring global vari-
ables, which could interfere with—or be interfered with by—other JavaScript code.

ERROR-FREE <SCRIPT> TAG INSERTION In the previous example, the <script>
tag element is inserted before another found script element on the page.
Alternatively, you could append the <script> tag to the head or body, but
that isn’t always safe. In some browsers it’s possible to load a web page without
a head element (admittedly, rare). And appending to the body element

before it’s finished parsing can sometimes cause browser exceptions. Insert-
ing before a found script element is a surefire way to load a script file without

running into these gotchas. And you can always guarantee there’s at least one
script element: the one that loaded your application.
You might be wondering to yourself, “Why are we paying so much attention to this
include snippet?” When it comes to third-party development, first impressions count.
After you distribute this include snippet to publishers, it’ll be incredibly difficult to get
them to change this code later. And unfortunately, since you likely don’t have access
to the servers hosting their website, you can’t change it for them. It’s a good idea to
come out with your best solution first.
FASTER SCRIPT LOADING If you’re interested in learning more techniques for
loading your scripts faster, you’ll want to read the High Performance Web Sites
series of books by Steve Souders (O’Reilly). Steve covers every conceivable
way of loading scripts, some of which can translate to big performance savings
in your applications. We’ll cover some of these techniques as they pertain to
third-party scripts in chapter 9 on performance.

We’re moving on for now, but we’ll revisit the script include snippet later in this chap-
ter. In the meantime, pretend you’ve already given the asynchronous include snippet

to one of your store’s loyal fans, which they’ve added to their web page’s HTML source
and published. This will cause your application’s initial script file, widget.js, to load on
their web page. Let’s take a look inside this script file and see what it’s doing.

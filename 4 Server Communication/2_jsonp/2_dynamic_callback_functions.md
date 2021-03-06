# Dynamic Callback Functions

## **4.2.2 Dynamic Callback Function**

The biggest downside of these approaches for loading JSON via <script> elements is
that the responses assume that the application loading these files already knows the
name of the global variable store or callback function. This is where JSONP comes in.
JSONP is a pattern of loading JSON responses via <script> elements where the server
wraps its response in a callback function whose name is provided by the requesting
party, via the script URL’s query string. This callback is referred to as padding—the P in

JSONP. Padding doesn’t necessarily have to be a callback function; it can also be a vari-
able assignment (as exhibited earlier) or any valid JavaScript statement. That said,

we’ll stick to callback functions here, because they’re used as the padding 99.99% of
the time.
Let’s look at an example of initiating a simple JSONP request.

**Listing 4.1 Initiating a simple JSONP request**

The first thing you need is a callback function that will be executed from the JSONP

response after it’s loaded. The browser will be evaluating the response in the same doc-
ument, so your callback function must be defined within the execution context of the

browser (global). Initiating the request is done by appending an HTML <script> ele-
ment to the DOM. The name of the callback function is passed as part of the target URL.

When the server receives this request, it’ll generate the following response, using
the callback name taken from the URL’s query string:
jsonpCallback({
'title': 'Third-party JavaScript',
'authors': ['Anton', 'Ben'],
'publisher': 'Manning'
});

This will execute the previously defined callback function (jsonpCallback), provid-
ing your code with the JSON data to do as you please. There’s a snag, though. Because

the callback function name has to be inserted into the response body, this document
has to be generated dynamically using a server-side script. Following is an example
implementation of info.js using PHP:

**Listing 4.2 Generating a JSONP response on the server**

If you’re still unclear about how JSONP works, look at figure 4.3, which takes you
through the steps involved in a typical JSONP request, from generating a callback and
sending a request to executing that callback and parsing the resulting response.

One last thing: the earlier example code from listing 4.1 always uses jsonpCallback as
the callback function’s name:
script.src =
'http://thirdpartyjs.com/info.js?callback=jsonpCallback';
In real-world applications, you should generate a new, unique callback function name
for each JSONP request, either using a random value, timestamp, or incrementing ID.

This will prevent potential conflicts with other, concurrent JSONP requests. Most pop-
ular AJAX libraries (including jQuery) already include functions for making JSONP

requests, and they’ll generate unique callback functions for you.

JSONP and other response formats
It’s technically possible to support other response formats besides JSON with

JSONP. For example, your JSONP endpoint could alternatively return a response con-
taining an XML string:

jsonpCallback(
'<?xml version="1.0" encoding="UTF-8" ?>' +
'<response>' +
' <title>Third-party JavaScript</title>' +
' <author>Anton</author>' +
' <author>Ben</author>' +
' <publisher>Manning</publisher>' +
'</response>'
);
Your application code would then need to take this XML string and deserialize it in
order for it to be useful. But this is kind of silly—JSONP in its normal form already
returns a JavaScript object, so by using an intermediate format you’re merely creating
more work for yourself.

---

### From [[_2_jsonp]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_jsonp]: _2_jsonp "JSONP"
[//end]: # "Autogenerated link references"

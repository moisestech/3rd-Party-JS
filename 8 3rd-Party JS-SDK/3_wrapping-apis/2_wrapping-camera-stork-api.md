# Wrapping Camera Stork API

## **8.3.2 Wrapping the Camera Stork API**

In this section, we’ll implement a public JavaScript function in the SDK that lets the
user communicate with a web service API. To do that, we’ll design a function through
which API requests are made, and then implement a cross-domain tunnel for issuing
HTTP requests. But before we start, we need a web service API to work with.
So without further ado, let us introduce the Camera Stork Web Service API. This

API allows publishers a]ccess to public information about Camera Stork’s product cata-
log and user reviews. It exposes the HTTP endpoints shown in table 8.1, which accept

a number of parameters and return responses as structured JSON.
The root URL for accessing these endpoints is http://camerastork.com/api/. That
means that if a publisher wants to request data from reviews/list, they’d issue an
HTTP request to http://camerastork.com/api/reviews/list. Parameters are submitted
as query string arguments.

**Table 8.1 The Camera Stork Web Service API**

Here’s an example of a curl command that requests a list of reviews from the API
ordered by date:
curl http://camerastork.com/api/reviews/list?ordered_by=date
Please note that these endpoints are read-only: they allow a publisher to query the
Camera Stork database, but not modify it. As such, they all respond to GET requests,
not POSTs. We’ll return to this fact later.
THE JAVASCRIPT INTERFACE
Now that you know what the Camera Stork web service API looks like, it’s time to

design a function interface for accessing those endpoints from your SDK. You’ll intro-
duce a new public function, Stork.api, through which publishers will access the API.

This function will accept three parameters: the API endpoint the publisher is trying to
access (reviews/list), a set of API parameters to send with the request
(ordered_by), and finally a callback function that’s executed when the API response is
ready:
Stork.api = function(endpoint, params, callback) {
// Function body
};
For example, for a publisher to request a list of user reviews ordered by date (like the
example curl request from earlier), they’d use the following code:
Stork.api('reviews/list', { ordered_by: 'date' }, function(response) {
if (response.error) {
console.log("An error occurred: " + response.error);
} else {
console.log(response.data);
}
});

When the response is ready, the SDK passes a response object to the publisher’s call-
back function. If the API request succeeded, the API’s JSON payload is accessible via

the response object’s data attribute. If an error occurred, then the error response
string is accessible via the response object’s error attribute.

FACEBOOK’S JAVASCRIPT SDK AND FB.API Does Stork.api look familiar? If
you’ve done any work with Facebook’s JavaScript SDK, it might. Stork.api
is inspired by FB.api, a public function exposed by Facebook’s JavaScript
SDK that wraps the Facebook Graph API. Learn more at https://developers
.facebook.com/docs/reference/javascript/FB.api/.
All right, this interface looks pretty good to us. It’ll give publishers a straightforward
means of communicating with the Camera Stork API from the browser, and they don’t
need to concern themselves with the details of cross-domain communication. Now,
let’s get down to implementing it.
COMMUNICATING WITH THE API
The next order of business is having your newly minted Stork.api function issue a

request to the web service API. But as you recall, the SDK is being loaded onto the pub-
lisher’s document, and the API is located on a different domain (camerastork.com),

so you can’t use XmlHttpRequest as-is. Although CORS and JSONP are possible solu-
tions here, you want to support the widest range of browsers possible, so you’ll imple-
ment a cross-domain tunnel using easyXDM, through which XmlHttpRequest can be

safely used.
This is how it works: your code will create a new iframe element on the publisher’s
page that points to a tunnel file hosted on camerastork.com. Because this tunnel file is

hosted on the same domain as your API, it can issue XmlHttpRequests without repri-
mand. To communicate with the code running on the tunnel file (inside the iframe),

you’ll use the easyXDM messaging library (see figure 8.2). This means that easyXDM
will need to be loaded alongside the SDK’s main library files during initialization.
Speaking of which, let’s return to the initialization method. As you did before, you’ll
load the main SDK library file. But when that file is ready, you’ll create a new easyXDM
RPC (remote procedure call) object that loads a tunnel file hosted on camerastork.com

**Figure 8.2 Initiating a cross-domain XmlHttpRequest through a tunnel file**

(listing 8.9). You’ll also now execute the publisher’s callback function after the tunnel
file is loaded, instead of immediately after lib.js is ready.

**Listing 8.9 Initializing the host RPC object**

Stork.init = function(callback) {
loadScript("http://camerastork.com/sdk/lib.js", function() {
Stork.rpc = new Stork.easyXDM.Rpc(
{
remote: "http://camerastork.com/sdk/tunnel.html",
onReady: callback
},
{
remote: {
apiTunnel: {}
}
}
);
});
};
The RPC object has a single remote function: apiTunnel. This is a function that you’ll

implement inside the tunnel file that’s responsible for issuing the actual XmlHttp-
Request to the web service API. The actual Stork.api implementation does nothing

more than wrap the RPC accessor to this function:
Stork.api = function(endpoint, params, callback) {
Stork.rpc.apiTunnel(endpoint, params, callback);
}
So when a publisher calls Stork.api, they’re actually invoking apiTunnel, which is
implemented inside the tunnel file. Let’s go to where the magic happens—here’s the
apiTunnel implementation, inside tunnel.html.

**Listing 8.10 The tunnel file, tunnel.html**

<!DOCTYPE html>
<html>
<script src="http://camerastork.com/lib/easyXDM.js"></script>
<script src="http://camerastork.com/lib/jquery.js"></script>
<script>
function apiTunnel(endpoint, params, callback) {
var options = {
url: 'http://camerastork.com/api/' + endpoint,
data: params,
type: 'GET'
};
options.complete = function(xhr) {
var response = {};
if (xhr.status !== 200) {
response.error = xhr.responseText;
} else {
response.data = JSON.parse(xhr.responseText);
}
callback(response);
};
jQuery.ajax(options);
}
var rpc = new easyXDM.Rpc({}, {
local: {
apiTunnel: apiTunnel
}
});
</script>
</html>

The tunnel file loads a few dependencies—easyXDM, of course, and jQuery, for its
AJAX utility function—and implements the apiTunnel RPC function. This function

takes a few parameters: an API endpoint string, a hash of API parameters, and a call-
back function that’s invoked when the AJAX call completes. The function assembles

these values into a hash of options, which is passed to jQuery’s ajax function. The
complete function handles the server’s HTTP response: it converts the result from
JSON to a JavaScript object, and then invokes the publisher’s callback function. Note
that the callback function isn’t actually executed in this scope—it’s actually an RPC
function generated by easyXDM that triggers the callback back on the publisher’s page.
Remember: this code is being executed inside an iframe, so it’s safe if easyXDM and
jQuery exist as global variables. Also, it’s not critical to use jQuery here—you can feel
free to substitute jQuery for any AJAX helper library.
There’s a reason why Stork.api is implemented inside the tunnel file, and not in
sdk.js: it’s safer. A poor approach would be to export jQuery’s ajax helper function as
an RPC object, and call that instead from your SDK. But remember: just as you’ve
accessed this iframe via easyXDM, so can anyone else. A malicious publisher could
wrap your ajax RPC method, and then have the power to issue XmlHttpRequests that
appear to originate from camerastork.com. That’s fine if they’re accessing your API,
but bad news if they decide to request arbitrary pages from camerastork.com, pages
which might have side effects. It’s better to be safe than sorry, and implement your API
accessor inside the tunnel file.

At this point, you’ve implemented the core components of a web service API wrap-
per. Your publishers can now communicate with the Camera Stork API with impunity,

which is the topic of our next section: now that requests to your API are coming
directly from visitors’ browsers (instead of publisher web servers), how do you identify
the originating application?

---

#### From [[_3_wrapping-apis]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_wrapping-apis]: _3_wrapping-apis "Wraping APIs"
[//end]: # "Autogenerated link references"

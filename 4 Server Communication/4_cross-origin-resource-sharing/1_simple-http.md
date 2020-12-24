## **4.4.1 Sending Simple HTTP requests**

When initiating cross-origin HTTP requests, browsers that support CORS indicate the
origin of the request by including an extra header called Origin. The value of this
header is the same triple as the one used by the same-origin policy—protocol, host
and port:
Origin: http://www.example.com/
The server’s job is to check that header and decide whether the request should be
allowed. If it decides in favor of the request, it must send back a response with an
Access-Control-Allow-Origin header echoing back the same origin that was sent:
Access-Control-Allow-Origin: http://www.example.com/
The server can also send a wildcard ("_") if the resource is public and pages from all
origins are allowed to make requests to that server:
Access-Control-Allow-Origin: _
If the request doesn’t have an Origin header (perhaps it’s from a browser that doesn’t
support CORS), the server shouldn’t send any CORS headers back.
Now, when the browser receives the corresponding HTTP response from the server,
it checks the value of Access-Control-Allow-Origin. Its value must exactly match the

**Figure 4.8 Cross-domain HTTP request with CORS. A request from the camerastork.com to
api.camerastork.com is permitted, so the server sends an appropriate response. A request from
Other website isn’t allowed, so the server doesn’t return any CORS headers.**

value of the Origin header that was sent to the server (or "\*"). If the header is missing
or the origins don’t match, the browser disallows the request. If the value is present
and matches Origin, the browser can continue processing the request.
Figure 4.8 illustrates a simple cross-domain HTTP request with CORS.
XMLHTTPREQUEST AND XDOMAINREQUEST
What’s great about CORS is that it’s implemented today by nearly all modern browsers.
Google Chrome, Mozilla Firefox, and Apple Safari all support CORS through the
XMLHttpRequest object. Microsoft added support for CORS in Internet Explorer 8, but
through the XDomainRequest object instead of XMLHttpRequest. And you don’t need
to explicitly enable CORS; it’ll be automatically triggered as soon as you try to make a
cross-origin request. If the browser doesn’t support CORS, XMLHttpRequest will raise a
permission exception when trying to open a resource from a different origin.

DETERMINING VALID ORIGINS CORS enables your servers to accept or block

HTTP requests that originate from a specific origin. It’s up to you to deter-
mine whether an origin header value is valid.

If you want to make a server endpoint accessible to third-party code execut-
ing on any arbitrary domain (like the Camera Stork widget), your endpoint

should return Access-Control-Allow-Origin: \*. This is perhaps the most

common use case for CORS, but remember—it means a request to your end-
point can be made from any origin.

If you’re developing an application that should only be accessible for a
small list of vetted publishers, your server endpoint should verify the request’s
Origin value against a list of allowed publisher domains. If your application

recognizes the domain, the server should return Access-Control-Allow-
Origin: vettedpublisher.com.

After you’ve figured out which object is supported, you’ll use that object to issue an
HTTP request. Fortunately, both the XMLHttpRequest and XDomainRequest objects
expose pretty much the same API, so you don’t need any additional browser checks to
initiate the request:
var req = makeCORSRequest('http://example.com/', 'GET');
if (req) {
req.onload = function () { /_ ... _/ };
req.send();
}
Geared with this knowledge, you can now start sending requests from your third-party
application to your servers using the makeCORSRequest function. This function will
work for plain, ordinary HTTP requests that use standard HTTP methods (GET and

POST). But there is a class of nonsimple requests for which CORS requires some addi-
tional work.

## **Transferring cookies wih CORS**

By default, browsers don’t send any identifying information—like cookies or HTTP
auth headers—with CORS requests. In order to indicate that a request should send
identifying information, you must set the withCredentials property of the
XmlHttpRequest object to true:
var xhr = new XmlHttpRequest();
xhr.withCredentials = true;
If the server expects and supports identifying information, it should respond with a
corresponding special HTTP header called Access-Control-Allow-Credentials (in

addition to Access-Control-Allow-Origin). If this header isn’t returned when with-
Credentials is true, the browser will reject the response:

Access-Control-Allow-Credentials: true

Alas, the withCredentials property is only available as a property of XmlHttp-
Request—not XDomainRequest. This means Internet Explorer 8 and 9 don’t support

credentialed requests and are incapable of transferring cookies using CORS. This
should be addressed in Internet Explorer 10, which is expected to implement
XmlHttpRequest Level 2, the latest W3C standard.

---

#### From [[_4_cross-origin-resource-sharing]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_4_cross-origin-resource-sharing]: _4_cross-origin-resource-sharing "Cross-Origin Resouce Sharing"
[//end]: # "Autogenerated link references"

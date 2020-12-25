# JSON Hi-Jacking

## **7.3.2 JSON hijacking**

You just learned a classical example of XSRF vulnerability but, unfortunately, XSRF
attacks come in different forms and shapes. One of these forms is called JSON hijacking,
and is often used to steal protected information from the user. If you recall the JSONP
technique from chapter 4, you’ll remember that <script> tags aren’t susceptible to
the restrictions of the same-origin policy, and you used that fact to make cross-domain
HTTP requests from your widget. JSON hijacking uses this SOP exception to steal data
from JSON endpoints by loading such endpoints using standard <script> tags.
Let’s say that your Camera Stork widget allows users to review their past purchases.
This information is considered private—your users wouldn’t be happy if you leaked
their purchase history to other web users. In order for your widget to access this data,

you’ve implemented an HTTP endpoint on your servers that returns the current ses-
sion user’s purchase history as JSON. You’ll use XmlHttpRequest to fetch the endpoint

from inside an iframe that contains a web page hosted on the Camera Stork domain.
Here’s an example response from that endpoint:
[
{ "model": "Canon Rebel XT", "price": 499, "order": 12030123 },
{ "model": "Olympus E-P2", "price": 350, "order": 12330393 }
]
Let’s suppose an attacker wants to gain access to this data using an XSRF exploit. As

you already know, if an attacker can initiate a request to this endpoint under the iden-
tity of a user (using a malicious web page), the server will read the user’s session and

respond with their purchase history. The attacker just needs to access the response’s
contents, and they’re off to the races. But how? The same-origin policy prevents them
from using XmlHttpRequest, because the endpoint has a different origin than the
attacker’s page. That leaves using the <script> tag to load the endpoint, but the
attacker has hit another wall—they can’t capture the contents of the response data,
because the JSON output isn’t assigned to a variable or otherwise persisted. As such, it
seems like the purchase history endpoint is sufficiently protected.
Not necessarily. In older browsers, it was once possible to define a custom setter on
the Object prototype in such a way that you could capture any values passed to it. This

made it possible to capture output from a JSON endpoint, like the example output
returned from the purchase history endpoint.

**Listing 7.5 JSON hijacking a seemingly secure HTTP endpoint**

<script>
var captured = [];
Object.prototype.__defineSetter__('model',
function (str) {
captured.push(str);
}
);
</script>
<script src="http://camerastork.com/api/orders.json"></script>

With this code on the page, the attacker only needs to lure an unsuspecting visitor to
their website and load the vulnerable JSON endpoint using a <script> tag. After the
endpoint has loaded, they can send the contents of the captured variable to their

servers. This isn’t just a hypothetical scenario; it was once possible to use this tech-
nique to extract a visitor's Twitter followers.7

The good news is that this particular vulnerability is no longer present in today’s

set of modern browsers. However, it hasn’t been the only JSON hijacking vector intro-
duced by browsers, and it probably won’t be the last. Next up, we’ll take a look at a few

ways you can protect yourself from XSRF attacks, including potential JSON attacks like
this one.

# Publisher Impersonation

## **7.4.1 Publisher impersonation**

Publisher impersonation is where a malicious publisher presents themselves as another
publisher that has registered with your application. In doing so, the attacker both
gains access to data that should only be presented to the victimized publisher, and any
actions made by the attacker are incorrectly attributed to the victim.
Let’s look at an example. If you recall chapter 1, you’ll remember that you need to
have publishers place a script include snippet in their web page source code that loads

your application. Suppose that you’ve also had publishers register with your applica-
tion beforehand, and the script include snippet you’ve given them (shown in the next

listing) specifies a configuration variable with the publisher’s account username (or
some other identifying token).

**Listing 7.6 A script include snippet that identifies the publisher**

<script>
(function() {
var script = document.createElement('script');
script.async = true;
script.src = 'http://camerastork.com/widget.js?' +
'publisher=somepublisher&product_id=1337';
var entry = document.getElementsByTagName('script')[0];
entry.parentNode.insertBefore(script, entry);
})();
</script>

Now, what will happen if a different publisher—by mistake or for malicious reasons—

installs this snippet on their website? A clueless publisher, instead of reading your doc-
umentation and going through the installation process properly, might decide to copy

and paste the snippet code—with configuration parameters not specific to them—
from another website. In that case, the Camera Stork service will mistakenly attribute
any actions performed on the imposter’s page to the original publisher.
Now, for the Camera Stork widget, this isn’t the end of the world. The worst that
might happen is that the imposter might earn referrals for the original publisher. But
for other third-party applications, the results can have far more impact. For example,
if an imposter loaded an analytics-gathering script (similar to Google Analytics) that
was attributed to another publisher, they could ruin that publisher’s data, causing
untold damages to their business. Another possible scenario: if you have a service that
charges publishers differently according to the load they bring on your servers, an
imposter could bring unexpected charges against another publisher. Based on our
personal experience, publishers seem to dislike that.
VERIFYING PUBLISHERS WITH THE REFERER HEADER
The strongest defense against publisher impersonation is to verify the requesting
party using the Referer HTTP header. In case you’re not familiar with it, the Referer
header is sent by the browser with every HTTP request, and contains the URL of the
document from which the request originated. That means that you can verify that the
originating document is a valid URL belonging to the publisher. If the Referer header
contains a URL with an unknown domain, you can reject the request and return an
error response.
To do this, you’ll need to have publishers submit to you a set of trusted domains
from which you can expect requests for your script to originate. This can be done

through a form hosted on your website (see figure 7.3). You’ll need to save these val-
ues in a database, and attribute them to the publisher’s account.

Afterward, you can perform the trusted domain verification in your web server

code. Let’s suppose that the initial script file (widget.js) is actually served via a server-
side application. When this resource is requested, you’ll look up the publisher’s

trusted domains and verify that the Referer URL falls under one of these values. The

**Figure 7.3 The Disqus commenting widget asks publishers to submit a set of trusted domains during configuration.**

following listing, a simple Flask server endpoint written in Python, demonstrates per-
forming this check.

**Listing 7.7 Verifying the requesting publisher’s domain using the Referer HTTP header**

from urlparse import urlparse
from stork_app import get_trusted_domains
@app.route('/widget.js')
def script_loader():
publisher = request.args.get('publisher')
trusted_domains = get_trusted_domains(publisher)
domain = urlparse(request.headers['Referer']).hostname
if domain not in trusted_domains(api_key):
return response(\
'Unauthorized domain for publisher: %s' % publisher
, 403)
return send_static_file('widget.js')
This example only checks the initial script file, but ideally you should perform this
check for all dynamic HTTP resources requested from your third-party application—
like any subsequent AJAX calls. Also, note that this example doesn’t cover resources

requested from inside an iframe hosted on your domain—since those requests origi-
nate from your domain, and not the publisher’s. We’ll address that scenario when we

revisit referrer checking in chapter 9.
Referrer checking isn’t perfect. The Referer header is submitted by the browser,

so it can be spoofed with different tools like web browser extensions or internet prox-
ies. Nonetheless, by checking the referrer you’ll eliminate the most blatant publisher

spoofing, not to mention honest mistakes.

---

#### From [[1_publisher-impersonation]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1_publisher-impersonation]: 1_publisher-impersonation "Publisher Impersonation"
[//end]: # "Autogenerated link references"

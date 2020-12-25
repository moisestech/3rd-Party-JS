# Clickjacking

## **7.4.2 Clickjacking**

Clickjacking, also known as a UI redress attack, is where a malicious web publisher tricks
unsuspecting visitors into clicking seemingly innocent parts of their website, but
which are in actuality hidden user-interface elements that belong to another web
application. This can cause visitors to unknowingly invoke actions on other websites,
including those for which they have authenticated sessions.
Let’s look at an example. Suppose that there’s a photography equipment company,
CheapCo, that has products available for sale through the Camera Stork online store.

This equipment company would greatly desire to have better reviews for their prod-
ucts on your website, but instead of doing it the old fashioned way—by making better

products—they’ve decided to cheat.

To do this, CheapCo has loaded an invisible iframe on their homepage whose con-
tents are the Camera Stork website. When anyone visits their homepage, they use

JavaScript to position the iframe beneath the visitor’s mouse cursor. When the user

clicks what they believe to be a regular UI element on CheapCo’s website, they’re actu-
ally clicking the web page that is being hosted inside the iframe. It’s possible that

CheapCo can position the iframe such that the user will always click a particular UI ele-
ment—like a button that submits a 5-star rating in favor of one of their products. Ouch!

X-FRAME-OPTIONS AND FRAMEKILLER SCRIPTS
The most straightforward way to prevent clickjacking is to make it so that your web
pages can’t be loaded via iframes. There are a few techniques to achieve this. The first
is to return a special HTTP header, X-Frame-Options, in responses for requests to your
content. X-Frame-Options can specify one of two values: sameorigin, which only
allows iframes from the same origin to display this content, and deny, which prevents
any iframe from doing so.
Here’s an example of an HTTP response which makes use of X-Frame-Options:
HTTP/1.1 200 OK
Date: Wed, 28 Sep 2011 14:33:37 GMT
Content-type: text/html
X-Frame-Options: sameorigin
If you’re wondering why X-Frame-Options is prefixed with an X, it’s because the
header isn’t a part of the HTTP specification. It was introduced by Microsoft for IE8 in
2009 as a response to public disclosures of redress attacks (see http://mng.bz/7Xl0),
and has since been adopted by the other major browser vendors. Because this HTTP
header has only recently been adopted, a slew of older browsers don’t support it.
Luckily, there’s another technique for preventing a web page from being loaded

inside an iframe: using what’s referred to as a framekiller JavaScript snippet. These snip-
pets use JavaScript to detect whether the current document is being displayed in a

frame and, if so, prevent the page from loading. This can be done by executing an
infinite loop, hiding content, or redirecting the document to a blank page—anything
that prevents the targeted page from rendering.
FRAMEKILLER KILLERS Not all framekiller scripts are made equal. Some are

susceptible to framekiller killers: scripts loaded on the parent page that pre-
vent the effectiveness of framekillers (the example we provide happens to be

unaffected). To find out how framekiller killers work, we recommend reading
Wikipedia’s article on the subject: http://en.wikipedia.org/wiki/Framekiller.
Here’s an example framekiller script that hides the document body if the window isn’t
the topmost window:

<script>
if (top != self) {
document.body.style.display = 'none';
}
</script>

CLICKJACKING AND THIRD-PARTY SCRIPTS
These solutions are great for stay-at-home web applications, but what about iframes

loaded from third-party applications? In this context, using either the X-Frame-
Options header or a framekiller script is not an option; you need that content to load

on the publisher’s page. In that case, you can take a few alternate steps to mitigate
clickjacking vulnerabilities:
 Have all data-modifying click actions require confirmation. If you have an action in
your iframe that will modify data, require the user to confirm the action in a
way that can’t be impersonated via clickjacking. The most popular approach is a
confirmation dialog that opens in a new window. Alternatively, it could require
typing a confirmation phrase into an input box.
 Require publishers to register with your service. If you require publishers to register
with your service in order to install your application, you at least have a paper

trail if a publisher abuses it. Registration makes it possible to shut down offend-
ing publishers.

Clickjacking is scary because it’s a vulnerability of the web and HTML, and not a bug in

your application or software. The worst part is that all websites are vulnerable to click-
jacking by default unless they take action to prevent it. For regular content which isn’t

served inside an iframe, that’s easily accomplished using the X-Frame-Options header
or framekiller scripts we covered earlier. For third-party applications, solutions are less
straightforward, but clickjacking is still manageable.

---

#### From [[_4_publisher-vulnerabilities]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_4_publisher-vulnerabilities]: _4_publisher-vulnerabilities "Publisher Vulnerabilities"
[//end]: # "Autogenerated link references"

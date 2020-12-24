# url-fragmentation

## **5.2.2 Sending messages using the URL fragment identifier**

In browser JavaScript environments, all window objects have a property called
location. This property can be used to get information about the current document
and its URL. It can also be used to redirect the current document to another URL:

```javascript
console.log(window.location.href);
```

> http://thirdpartyjs.com/
> window.location = 'http://example.com/';
> console.log(window.location.href);
> http://example.com/
> When working with iframes and other child windows, this property is accessible, but
> exclusively in a write-only mode. This means that you can redirect a child window to
> another URL, but you can’t read back the current URL of that window. This policy was

implemented as a security measure so that a malicious website can’t open your favor-
ite web mail client in a hidden iframe and gather information by reading the URL’s

query string or logging redirects.
Of course, not all URL changes result in network requests. As you’ve already
learned, HTML defines a special portion of a URL—called the fragment identifier or URL

hash—that’s designed to point to a location inside the current document. This por-
tion always comes last in a URL string and is preceded by the hash (#) symbol:

http://camerastork.com/#products
The fallback technique you’re about to learn transfers small chunks of data across
windows by changing each other’s fragment identifiers. Since changing fragment
identifiers doesn’t reload the page, you can maintain state in each frame. But keep in
mind that you can’t read an iframe’s URL; you can only write to it. So when loading a
document into an iframe, you have to pass your current URL in to it so that it doesn’t
accidentally redirect your publisher’s page to some other address. You can then start
sending information by changing the iframe’s location property to its original URL
plus your data in the anchor portion. And when the child window wants to send some
data back to you, it can change its parent window’s location to the URL you sent with
your initial request, plus the data (see figure 5.3).

**Figure 5.3 Using the URL fragment identifier to pass messages between windows**

The next listing shows an example of the client creating a new iframe and listening to
new messages using the fragment identifier technique.

**Listing 5.5 Client listening to new messages using the window.hash workaround**

```javascript
var url = window.location.href;
var iframe = document.createElement("iframe");
iframe.style.display = "none";
iframe.src =
  "http://camerastork.com/hashtransport/server.html?url=" +
  encodeURIComponent(url);
var body = document.getElementsByTagName("body")[0];
body.appendChild(iframe);
var listener = function () {
  var hash = location.hash;
  if (hash && hash !== "#") {
    console.log("Incoming: " + hash.replace("#", ""));
    window.location.href = url + "#";
  }
  setTimeout(listener, 100);
};
listener();
```

Note that, in order to receive data, you have to monitor the current URL in order to
know when it’s been changed. You can do that by polling the URL every N milliseconds.

The next listing shows an example of a page hosted on the server that sends a mes-
sage to the parent page by modifying the parent’s fragment identifier.

**Listing 5.6 Sending messages by modifying parent window’s fragment identifier**

```html
<!DOCTYPE html>
<html>
  <head>
    <script>
      function init() {
        var url = window.location.href;
        url = url.split("?")[1].replace("?", "");
        window.parent.location = decodeURIComponent(url) + "#helloworld";
      }
    </script>
  </head>
  <body onload="init()"></body>
</html>
```

These listings demonstrate how to send a single message from the iframed document
to the parent document. But the iframed document itself can also receive messages
from the parent—the roles are just reversed. In this scenario, the parent modifies the

iframe’s fragment identifier, and the iframed document polls for changes. If there is

bidirectional communication, the documents are both changing each other’s frag-
ment identifier and polling for changes to their own identifier. Dizzying, we know.

THE HASHCHANGE EVENT Normally, you can listen to changes to the fragment

identifier by binding to the window’s hashchange event. This is an event intro-
duced in HTML5 that fires whenever the fragment identifier (or hash)

changes. But because it’s only supported in browsers that also implement
window.postMessage, it isn’t practical for use with this fallback technique.
URL SIZE LIMITATION WHEN USING FRAGMENT TRANSPORT
Since this transport mechanism depends on changing the fragment identifier of the
URL, it is constrained by limitations placed on the URL itself. As you’ve already
learned, Internet Explorer has a maximum URL length of 2,083 characters. This
means that the maximum amount of data you can send in a single message is 2,083
characters minus the length of the original URL. It’s possible to get around this by

breaking up messages into smaller, consecutive chunks, and then sending them piece-
by-piece over the document fragment. Though not terribly complex, splitting mes-
sages into packets adds significant complexity to this transport protocol.

The fragment transport technique is a good one because it works reliably in older
browsers. Now, let’s look at one final workaround for emulating the window
.postMessage API: using Adobe’s Flash browser plugin.

---

#### From [[_2_fallback-techniques]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_fallback-techniques]: _2_fallback-techniques "Fallback Techniques"
[//end]: # "Autogenerated link references"

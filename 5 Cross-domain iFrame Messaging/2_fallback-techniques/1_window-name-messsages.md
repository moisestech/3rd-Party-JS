# window.name

## **5.2.1 Sending messages using window.name**

First off, what’s the window.name property? This property stores the name of the cur-
rent window so that other windows—like the window’s parent window—can get a ref-
erence to it according to its name. This property has a valuable peculiarity: after it’s

assigned a value, that value doesn’t change when the window is redirected to a new

URL. This behavior makes it possible to read the window.name value specified by a doc-
ument from a different origin, thereby bypassing the same-origin policy and making

cross-domain messaging possible.
This is a somewhat-complicated protocol, so to explain it best we’ll use a simple
Hello World example. On the publisher’s side, you’ll create a new iframe and assign
its URL to a target page on your domain. When loaded, the iframe will then initiate a
new message back to the parent window by assigning a message string to the
window.name property. But the parent page can’t read this value as-is, because the
iframe is hosted on a different domain (remember: SOP). To correct that, the iframe
will redirect itself to a URL on the publisher’s domain. After the iframe is redirected,

**Figure 5.2 Sending a message from an iframe to its parent window using window.name**

the parent page can now read the value of window.name, because the iframe now hosts
a document on the same domain. And because the window.name property doesn’t
change between redirects, it still contains the value of the message set by the original,
external iframe. This program flow is visualized in figure 5.2.
The next listing shows an example of a client creating a new iframe and polling for
new messages.

**Listing 5.3 Client listening to new messages using the window.name workaround**

```javascript
var iframe = document.createElement("iframe");
var body = document.getElementsByTagName("body")[0];
iframe.style.display = "none";
iframe.src = "http://camerastork.com/nametransport/server.html";
var done = false;
iframe.onreadystatechange = function () {
  if (iframe.readyState !== "complete" || done) {
    return;
  }
  console.log("Listening");
  var name = iframe.contentWindow.name;
  if (name) {
    console.log("Data: " + iframe.contentWindow.name);
    done = true;
  }
};
body.appendChild(iframe);
```

The code on the server is also not very complicated. It changes the window.name property and then redirects its host window object to an empty HTML page hosted on the publisher’s domain:

```html
<!DOCTYPE html>
<html>
  <head>
    <script>
      function init() {
        window.name = "Hello, World!";
        window.location = "http://publisher.com/empty.html";
      }
    </script>
  </head>
  <body onload="init();"></body>
</html>
```

This empty.html file is exactly as it sounds: a minimally valid HTML page. You need

this because the browser needs to load something, so it may as well be as small a docu-
ment as possible:

<!DOCTYPE html>
<html></html>
One disadvantage of this approach is that you have to make a network request every
time you want to retrieve a new message.3

In addition, most of the time, widget devel-
opers don’t have access to their customers’ websites so you don’t have an empty page

you can redirect to in the last step. That means that you either have to ask your users to
host such a page on their website or you’ll have to redirect to some other random URL
on the client’s website—like a 404 page. Using a random URL like this can significantly
alter that site’s traffic statistics by increasing the number of requests to their pages.
SECURITY IMPLICATIONS WITH WINDOW.NAME In theory, other frames loaded
on the page might attempt to access the loading frame and navigate it to their
own URLs in order to get hold of the data you placed in the window.name
property. In practice, navigating to a frame from another frame that’s neither

a child nor a parent of that frame is prohibited in browsers, with one excep-
tion: Firefox 2. But that version of Firefox has almost no market share, so you

shouldn’t worry about this scenario too much.

AVOIDING SOUND EFFECTS CAUSED BY MODIFYING WINDOW.LOCATION

The window.name trick works pretty well in legacy browsers with one exception: Inter-
net Explorer versions 7 and below accompany every location change with an annoying

clicking sound. As you might imagine, if your application sends a lot of messages using
this technique, your computer will end up sounding like a toy machine gun.

To solve this problem in Internet Explorer 6, you need to add a <bgsound> ele-
ment to the page. This will cause the browser to think that there’s background music

playing and not play the clicking sound while navigating. You don’t need to link to an
actual sound file—just an empty <bgsound> tag will do (see the Mozilla Developer
Network discussion at https://developer.mozilla.org/en/HTML/Element/bgsound).
Unfortunately the <bgsound> trick doesn’t work in Internet Explorer 7, for which
you’ll use an alternate workaround shown next. This workaround relies on ActiveX,
an API that’s only available in Internet Explorer (all versions).

**Listing 5.4 Using a detached document to silence the clicking sound in IE7**

```html
var doc, iframe, html; if ("ActiveXObject" in window) { html = "
<html>
  <body>
    " + " <iframe id="iframe"></iframe>" + "
  </body>
</html>
"; doc = new ActiveXObject("htmlfile"); doc.open(); doc.write(html);
doc.close(); iframe = doc.getElementById('iframe'); } else { iframe =
document.createElement('iframe'); document.body.appendChild(iframe); }
```

As you can see, we create a new ActiveX object, htmlfile, that acts as an HTML docu-
ment, and append the iframe inside. This document happens to be disconnected from

the browser’s UI, so when changes occur to the iframe element’s window.location
property, the browser decides to save resources—and our nerve cells—by not playing
the accompanying clicking sound.

Phew! We just looked at one fallback technique for sending messages between doc-
uments. But there are other techniques, all with their own advantages and disadvan-
tages. Let’s look at a different approach that relies on a similar browser quirk to

circumvent the same-origin policy.

---

### From [[_2_fallback-techniques]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_fallback-techniques]: _2_fallback-techniques "Fallback Techniques"
[//end]: # "Autogenerated link references"

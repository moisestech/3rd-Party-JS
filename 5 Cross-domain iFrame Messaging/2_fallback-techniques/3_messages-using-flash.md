# messages-using-flash

## **5.2.3 Sending messages using Flash**

In chapter 4, we stated that the browser is a complicated program with many different
components, and not all of them are subject to the same-origin policy. Adobe’s Flash
plugin is one such component; it doesn’t adhere to the SOP like browsers do. Instead,
it assumes that if you let a user upload a Flash object on your website, you implicitly

trust everything it does. Third-party JavaScript widgets can load anything on the web-
site, including a special Flash object that acts as a tunnel between the host site and an

iframe. It doesn’t matter what host you load this Flash object from; you can load it
from any of the two participating domains, or you can use a common host—such as a
CDN—to load the file faster.
The actual Flash object should be written in ActionScript—a dialect of ECMAScript
developed in 1999 by Macromedia, Inc., specifically for Flash scripting. Because it’s a
dialect of ECMAScript, it has the same syntax and semantics as JavaScript, so you
should have some basic understanding of the examples we’re about to show you.

APACHE SHINDIG PROJECT If you’re interested in third-party widgets develop-
ment—and why else would you be reading this book?—check out the Shindig

project from the Apache Foundation. Apache Shindig is an OpenSocial con-
tainer that provides utilities to render gadgets, proxy requests, and handle

REST/RPC requests. Even if you don’t plan to use it, the project page contains

tons of interesting information on cross-domain communication and third-
party JavaScript development.

To establish a connection between the Flash object and its container, you can define
public interfaces using the flash.external.ExternalInterface class. The class has a
public method, addCallback, that can be used to register an ActionScript method as
callable from the container. For example, to define a public postMessage method on

the Flash object, you just need to call the ExternalInterface.addCallback and pro-
vide your method’s implementation:

```javascript
import flash.external.ExternalInterface;
class Main {
private static function main(swfRoot:MovieClip):Void {
ExternalInterface.addCallback("postMessage", {},
function(channel:String, message:String) {
/_ ... _/
}
);
}
}
```

The ExternalInterface class also exposes a public call method that can be used to
access JavaScript properties and methods from the host page. You can use that
method to notify the host page about new messages coming their way. The example in

listing 5.7 shows an implementation of the onMessage function that notifies an exter-
nal JavaScript method on the host page. We took this example from the source code

of a popular cross-domain messaging library called easyXDM. We’ll cover easyXDM
shortly in section 5.3.

**Listing 5.7 onMessage implementation from easyXDM JavaScript library**

```javascript
listeningConnection.onMessage = function (message, fromOrigin, remaining) {
  if (fromOrigin !== remoteOrigin) {
    return;
  }
  incomingFragments.push(message);
  if (remaining <= 0) {
    // escape \\ and pass on
    ExternalInterface.call(
      prefix + 'easyXDM.Fn.get("flash_' + channel + '_onMessage")',
      incomingFragments.join("").split("\\").join("\\\\"),
      remoteOrigin
    );
    incomingFragments = [];
  } else {
    log(
      "received fragment, length is " +
        message.length +
        " remaining is " +
        remaining
    );
  }
};
```

This ActionScript implementation of a cross-domain channel isn’t the most fun pro-
gram to write, so usually developers—including the authors of this book—reuse an
existing open source implementation that was written and tested by somebody else. If
you’re interested in the specifics of the ActionScript implementation, we encourage
you to check out the source code of easyXDM. It’s available on GitHub: https://
github.com/oyvindkinsey/easyXDM/. But before we dive into easyXDM, let’s talk
about a couple of gotchas that often bite developers who decide to rely on Flash for
their cross-domain communication needs.
ISSUES WITH HIDDEN FLASH OBJECTS

Often, the target iframe you’re communicating with is hidden from the page. Typi-
cally this is done by setting the element’s display CSS property to none or by changing

its visibility property to hidden. As it turns out, loading a Flash object inside a hid-
den iframe causes a handful of known issues.

One such issue affects only Internet Explorer 7. This browser doesn’t initialize any
Flash objects that are loaded inside a hidden iframe. That means that the browser will

request the Flash object and load it from the server, but afterward nothing will hap-
pen. To solve this problem, you need to hide your iframe in a different way, by setting

its position property to absolute and its top property to a value that places the
iframe well outside the viewport (say, -10000px). That will trick Internet Explorer into
thinking that your iframe is still visible (despite being outside the viewport), and the
browser will initialize the contained Flash object as expected.
Another problem occurs with newer versions of Flash that throttle access to hidden
Flash objects. This can limit the rate at which you can send messages between iframes.
The good news is that this is only likely to affect a small number of applications that

are sending a lot of messages. The bad news is that we don’t know any good work-
arounds other than to not hide the container iframe. Usually, giving the iframe a min-
imally recognized width and height (20px has been reported to be effective) will

cause Flash to treat it normally.
All things considered, Flash is an excellent fallback technique when postMessage
isn’t available. Yes, it has issues with hidden iframe containers, requires browser users

to have Flash installed, and requires users to make a network request to load the inter-
mediate Flash object. But what makes Flash valuable is that it doesn’t require publish-
ers to host any intermediate files on their servers. For this reason, many third-party

services use Flash before falling back to window.name or window.hash.

---

#### From [[_2_fallback-techniques]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_fallback-techniques]: _2_fallback-techniques "Fallback Techniques"
[//end]: # "Autogenerated link references"

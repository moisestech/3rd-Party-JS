# Event Listeners

## **8.1.4 Event listeners**

The next set of public functions to implement are Stork.listen and Stork.unlisten.

We described Stork.listen in the introduction: it’s a way for publishers to listen to public named events occurring in your application and respond to them accordingly.

It should logically follow that Stork.unlisten is the opposite: it’s for the publisher to stop listening to a named event, given that they’re already listening to one.

Let’s look at a simple implementation of Stork.listen that takes two parameters—a string representing an event name, and a function handler to invoke if/when that event occurs:

```javascript
var listeners = {};
Stork.listen = function (eventName, handler) {
  if (typeof listeners[eventName] === "undefined") {
    listeners[eventName] = [];
  }
  listeners[eventName].push(handler);
};
```

Here, the application maintains an internal list of event handlers in a private variable named listeners. When Stork.listen is called, the publisher’s event handler is appended to the list for the given event. But when does the event fire?

Your application now needs an internal function that triggers these event handlers when an event occurs. Let’s call this function broadcast. When an important event occurs inside your application, you’ll call broadcast with the name of the occurring event. For example, when a product widget finishes rendering, you could use broadcast to indicate that an event named productWidget.rendered has occurred: broadcast `('productWidget.rendered');`

Here’s an example implementation of broadcast as a private function inside sdk/lib.js.

The reason it’s private? You don’t want other parties—like publishers—triggering events in your system:

```javascript
function broadcast(eventName) {
  if (!listeners[eventName]) {
    return;
  }
  for (var i = 0; i < listeners[eventName].length; i++) {
    listeners[eventName][i]();
  }
}
```

You should invoke broadcast wherever you want to publicize that an event has taken place. We just looked at one example event—productWidget.rendered—but there are plenty of use cases, for example, to report when an affiliate link has been clicked `(productWidget.referralClicked)`, or if a user has signed in to your service `(productWidget.authenticated)`. Remember: these are your own internal events, and you’re free to name them and initiate them as you see fit.

At some point, publishers might want to stop listening to an event. For example, if a publisher is only interested in a single occurrence of an event, they can unsubscribe after it has occurred once. This is where Stork.unlisten comes in—it unsubscribes a previously registered event handler from the system:

```javascript
Stork.unlisten = function (eventName, handler) {
  if (!listeners[eventName]) {
    return;
  }
  for (var i = 0; i < listeners[eventName].length; i++) {
    if (listeners[eventName][i] === handler) {
      listeners[eventName].splice(i, 1);
      break;
    }
  }
};
```

Between these three functions—Stork.listen, Stork.unlisten, and the broadcast function—you now have all the pieces you need for publishers to listen to events occurring in your application.

**USING CUSTOM EVENT LIBRARIES**

As is the case with many things we’ve covered in this book, there are open source alter-natives to the example custom event code we just covered. For example, jQuery has support for custom events using its regular DOM event helper functions:1

- jQuery.bind—Attach an event handler to a DOM element.
- jQuery.unbind—Remove a previously-attached event handler from a DOM element.
- jQuery.trigger—Trigger an event on a DOM element.

These functions look suspiciously close to the Stork event functions you just implemented. Wouldn’t it be nice if you could use them instead? That way, you could leverage jQuery’s well-tested custom event code, instead of maintaining your own.

There’s one small hurdle: the jQuery functions bind and trigger events on JavaScript objects, whereas the Stork event functions don’t. This is quickly solved by using a stub object upon which you’ll bind and trigger events. Here are modified versions of Stork.listen, Stork.unlisten, and broadcast, which wrap jQuery’s event functions.

**Listing 8.4 Wrapping jQuery’s DOM event functions**

```javascript
var stub = {};
Stork.listen = function (eventName, handler) {
  jQuery(stub).bind(eventName, handler);
};
Stork.unlisten = function (eventName, handler) {
  jQuery(stub).unbind(eventName, handler);
};
function broadcast(eventName) {
  jQuery(stub).trigger(eventName);
}
```

Of course, if you’re not interested in the full jQuery feature set, there are also focused libraries that offer the same custom event functionality. Bean is a DOM event micro-library whose API closely resembles jQuery’s (http://github.com/fat/ bean). JS-Signals is a feature-rich custom event library that will also fit the bill (http://millermedeiros.github.com/js-signals/). Take a look and consider whether any of these are worthwhile additions to your application.

---

#### From [[_1_sdk-implementation]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_sdk-implementation]: _1_sdk-implementation "SDK Implementation"
[//end]: # "Autogenerated link references"

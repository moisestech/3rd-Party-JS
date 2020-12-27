# Throttle Debounce

## **9.2.2 Controlling Expensive Calls: Throttle and Debounce**

The solution to this problem lies in two techniques designed to control how often
your functions are called. These techniques—throttle and debounce—work by creating a

wrapper function that rate-limits calls to the original function. These functions oper-
ate in slightly different ways, which we’ll get to in a moment. But what’s important is

they’re both effective in rate-limiting calls to functions bound to frequently firing
browser events—like the window’s scroll event.

Throttle works by limiting function calls to no more than once every N millisec-
onds (see figure 9.2). An example implementation of a throttle function, shown in

listing 9.5, remembers the most recent time when the original function was called and
executes it again only when the specified time has passed. The time value is passed as
an argument to throttle and can vary according to your preferences. Throttling a
function can be useful when you want to limit your function’s execution frequency,
regardless of how often it’s being called.

**Listing 9.5 Implementation of function throttling**

```javascript
function throttle(el, name, handler, delay) {
  var last = new Date().getTime();
  function wrapper(ev) {
    var now = new Date().getTime();
    if (now - last >= delay) {
      last = now;
      handler(ev);
    }
  }
  el.addEventListener(name, wrapper, false);
}
```

If you look closely at the code from listing 9.5 you’ll notice that throttling doesn’t
guarantee that your code will be called every N milliseconds. If the code that uses a
throttled function suddenly stops calling it, the original function won’t be executed a
final time unless it occurs exactly on the throttle interval.

Debounce makes sure that the wrapped function is called only once after the begin-
ning or end of a continuous sequence of calls. The implementation shown in listing 9.6

monitors the frequency of calls, and only executes the wrapped function after a pre-
defined interval of time (the delay) has passed without a subsequent call. For exam-
ple, suppose that a function has been wrapped with debounce using a delay of 1

second (1,000 milliseconds). If the debounce function is called every 100 ms for a
short period, and then a 1-second pause occurs, the wrapped function will only fire
once—after the pause.

**Figure Throttling a function at 250 milliseconds. Regardless of how often the outer function is called, it only executes the original (wrapped) function every 250 milliseconds.**

**Listing 9.6 Implementation of function debouncing**

function debounce(el, name, handler, delay) {
var exec;
el.addEventListener(name, function (ev) {
if (exec) {
clearTimeout(exec);
}
exec = setTimeout(function () {
handler(ev);
}, delay);
}, false);
}

Both throttle and debounce are possible solutions to the performance problem affect-
ing the deferred avatar code from section 9.1.3. You can use either function to limit

the number of times displayDeferredAvatars is invoked from the window’s scroll

event, reduce the number of DOM reflows and repaints, and help to make the pub-
lisher’s page more responsive. But each of these functions behaves slightly differently.

Which is better suited to the task?
If you go with throttling, it means that as the user scrolls down the page, the
displayDeferredAvatars function will fire repeatedly over the throttle interval. This
means that you may end up loading more images than you intend to, and even end up
loading images that have since moved out of the user’s viewport (see figure 9.3).
Debounce will only call the displayDeferredAvatars function after the user has
stopped scrolling (or dramatically slowed down their scrolling rate). This results in
less HTTP requests than throttling (since images will be skipped), but perhaps a

**Figure 9.3 Throttling the deferred avatar function. Depending on how fast the user scrolls down the page, you can end up processing avatars that are already off the screen.**

reduced user experience; the user won’t see any images appear while the viewport is
in the process of scrolling. If you’re curious, the Disqus commenting widget uses
throttling, because we don’t mind loading the extra images, but you could make an
argument for either approach.

## **Deferring computation with setTImeout**

You’ve just learned how debounce and throttle are great techniques for avoiding
browser lockup by reducing the number of times your code is executed when it’s
bound to an event that fires multiple times. But if the code that fires is particularly
expensive, it can still tie up the browser’s UI thread, preventing both browser updates
and other JavaScript operations.
Let’s look at an example. If you recall from chapter 8, you implemented a function
for the Camera Stork JavaScript SDK, Stork.productWidget, which lets publishers
insert widget instances on their web pages. A later version of this function allowed
publishers to specify a target location using a CSS selector string. This snippet of code
uses that function to insert a product widget wherever the widget class is found:
Stork.productWidget({ id: 1337, dom: '.widget' });
Now, suspend disbelief for a moment, and imagine that the publisher’s page has 100

DOM elements with the class widget. This snippet would then have the effect of insert-
ing a widget into all 100 locations where the class appears. That’s an expensive opera-
tion, involving many DOM operations, that’s likely to block the browser’s UI thread.

Not only might this show a loading spinner in the user’s browser—it might even fire
the dreaded “script has become unresponsive” browser alert, which occurs when a
script has been executing for too long.
The solution involves a function by the name of setTimeout. This function takes

two arguments: a callback function and an integer that represents time in millisec-
onds. When that time elapses, setTimeout executes the provided callback function:

setTimeout(function() { /_ ... _/ }, 1000);
What’s not commonly known is that you can pass setTimeout a time value of 0.
Although you might expect that this executes the callback function immediately, it
doesn’t. What the browser does instead is place the callback function at the end of the
render queue processed by the UI thread. It effectively lets you defer code until the

browser has finished its current queue of jobs and is free to run your code. Using set-
Timeout in this manor is sometimes referred to as script yielding.

This is powerful stuff, and helps solve your initial problem—long-running
JavaScript operations. What you can do is split up that code into smaller, bite-size
operations, and queue each one using setTimeout with a time parameter of zero.
Then, every time one of those operations finishes, it’ll release the UI thread to run any
other queued jobs before returning to your code.

NOT EXACTLY ZERO As it turns out, when you call setTimeout with a delay
of 0, this value is actually substituted with a browser-defined minimum. The
HTML5 specification defines this value is 4ms; previously it was 10ms. But note
that your code isn’t guaranteed to execute in exactly this time—the browser
will delay timeouts if it’s busy with other tasks. See http://mng.bz/R44K.

To see this in action, here’s a sample implementation of the Camera Stork SDK’s inter-
nal renderWidgets function. This function is passed an array of target DOM elements

where the widget is to be inserted, and an object containing the product’s attributes:
function renderWidgets(targets, productData) {
function processNext() {
var target = target.shift();
renderWidget(target, productData);
if (targets.length) {
setTimeout(processNext, 0);
}
}
processNext();
}
In this example, the private processNext function gets the next target to process by

shifting an element off the front of the targets array. If there are any remaining tar-
gets, the processNext function is executed again using setTimeout. This continues

until there are no more targets to process.
Now, optimizing for 100 inserts of a widget might seem like an extreme scenario—
but you may have other long-running scripts in your code that have the same effect.

By splitting such code into smaller chunks and queueing their execution with set-
Timeout, you release the UI thread to handle other browser operations—like re-
rendering the viewport when the user scrolls the page, rendering CSS animations, or

running the publisher’s own JavaScript code. In other words, the publisher’s page is
still fully functional. If your application has any such long computations, it absolutely
must use this technique, or publishers will be quick to leave your platform.
JAVASCRIPT MICRO OPTIMIZATIONS Micro optimizations are small adjustments to
your code that either help you to speed up your program a tiny bit, or save bytes

when transferring your code over the network. For example, consider this sim-
ple loop that iterates over an array of integers and prints out each value:

var arr = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ];
for (var i = 0; i < arr.length; i++) {
console.log(arr[i]);
}
This is a simple piece of code, but nevertheless a lot of people would love to
get their hands on it and micro-optimize it to death. You could start by saving
the value of arr.length into a local variable so that it doesn’t get accessed 10
times. Then there’s the comparison between the variable i and the array’s

length—for which it’s technically faster to loop backward and compare the

iterator variable with 0. By the time you’re done with these types of optimiza-
tions, your loop will end up looking something like this:

var arr = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ];
var i = arr.length;
while (i -= 1) console.log(arr[i]);
Micro optimization can be a nice hobby, but it should be the last thing you
worry about when writing your JavaScript code. Unless you’re developing
JavaScript games or other graphics-intensive applications, converting loops
into their uglier counterparts won’t improve your application’s performance

in any significant manner. Besides, modern JavaScript minifiers already opti-
mize your code for you, so don’t waste your time obsessing about whether you

can save a few bytes by using a quadruple-nested ternary operator.
If we haven’t dissuaded you and micro optimization is still your thing,
make sure to check out jsPerf (http://jsperf.com), a website that provides an
easy way to create and share small JavaScript benchmarks.

---

#### From [[_2_optimizing-js]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_optimizing-js]: _2_optimizing-js "Optimizing JS"
[//end]: # "Autogenerated link references"

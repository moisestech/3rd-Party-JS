# Deffering HTTP Req

## 9.1.4 **Defferring HTTP Request**

As you’re aware, third-party applications often must download many different files in
order to function (JavaScript, CSS, images, and so on). But sometimes you’ll observe

that some of these resources aren’t immediately essential to your application’s opera-
tion. And yet by downloading these additional resources up front, you’re increasing

the crucial interval between the moment your application starts to load and when it
takes effect on the publisher’s page.

One technique you can employ to decrease this interval is to defer non-essential
HTTP requests. What this means is that your application avoids loading such resources
until they’re absolutely necessary. This reduces the size of your initial payload, helping
you to initialize your application faster. As an added bonus, you may find that many

browser users will never end up loading these deferred resources, which can signifi-
cantly reduce the amount of HTTP traffic sent to your servers.

There are a number of ways you can defer HTTP requests, but we’ll look at two that
are particularly beneficial to third-party widgets: deferring the entire widget body, and
deferring image resources.

DEFERRING THE WIDGET BODY
This technique—referred to by some as lazy loading—is an extreme way of reducing
the initial payload by deferring most of your application code until the widget body is

actually viewed by the user. The implementation is simple: the initial loader script ini-
tializes itself on the page, and then does nothing except listen to the window’s scroll

event. When the loader detects that the widget is about to enter the user’s viewport, it
begins downloading the required components and renders the widget as usual. If
many of your publishers’ visitors will never actually view your widget content, your
application will issue less HTTP requests, reducing both the load on your servers and
the performance hit to those users’ browsers.
The following shows an example of a loader script with a lazy load implementation.

**Listing 9.2 Lazy load implementation for the Camera Stork Widget**

function getWindowDimensions() {
var documentElement = document.documentElement;
return ('pageYOffset' in window) ? {
// W3C
scrollTop: window.pageYOffset,
scrollLeft: window.pageXOffset,
height: window.innerHeight,
width: window.innerWidth
} : {
// IE 8 and below
scrollTop: documentElement.scrollTop,
scrollLeft: documentElement.scrollLeft,
height: documentElement.clientHeight,
width: documentElement.clientWidth
};
}
function getPosition(el) {
var left = 0;
var top = 0;
while (el && el.offsetParent) {
left += el.offsetLeft;
top += el.offsetTop;
el = el.offsetParent;
}
return { top: top, left: left };
}
function insideViewport(el) {
var win = getWindowDimensions();
var pos = getPosition(el);
return pos.top >= win.scrollTop &&
pos.top <= win.scrollTop + win.height;

}
function whenVisible(callback) {
var el = document.getElementById('stork-container');
Listing 9.2 Lazy load implementation for the Camera Stork widget

Browser-compatible
helper function for
determining window
dimensions

Returns element’s
current position

Determines if
element is in
viewport

Fires callback function
when widget container
is in viewport

if (!el) { return; }
function listener() {
if (insideViewport(el)) {
callback();
}
}
Stork.debounce(window, 'scroll', listener, 250);
listener();
}
...
whenVisible(function () {
initializeWidget();
});
Let’s take a look closer at this code. When the whenVisible function is invoked, it gets

a reference to the target container element, and every time the user scrolls the win-
dow, it checks whether the container is now in the viewport. The window dimensions,

the window scroll position, and the container position are determined using a series
of simple helper functions. Besides initializeWidget, which we presume you know
how to implement on your own, there’s an additional undefined function: debounce.
This is a special function that makes sure your event handlers don’t accidentally make
the browser unresponsive. We’ll cover it in depth later in this chapter.

Deferring the widget body like this isn’t for everyone. At Disqus, we use this tech-
nique only as a nuclear option when our commenting application is experiencing an

unusual amount of traffic. This is because deferring the widget can result in a reduced
user experience. When it’s enabled, visitors must scroll down the publisher’s page
before the widget commences initialization, and users will likely notice a short period
in which the widget is unavailable. But having the widget load slowly is always better
than not loading at all because our servers couldn’t keep up with the traffic load. So,
by default we keep lazy loading turned off, but using feature switches (a tool for

dynamically enabling or disabling features that we’ll introduce in chapter 10 on test-
ing and debugging), we can turn it on for a percentage of our users—sometimes all of

them—in order to better deal with traffic spikes.
DEFERRING IMAGES
Speaking of the Disqus commenting widget, let’s look at another situation that can
benefit from deferring HTTP requests. Let’s imagine for a moment that the Disqus
widget is loaded for a large commenting thread with more than 50 participants. Each
person commenting on the thread has a unique avatar image that they’re proud of,
and the job of the Disqus widget is to display this avatar next to their comment. If we
were to just insert those images into the page, the browser would immediately queue
up 50 images to download, which would certainly slow things down. And we don’t
even know whether a visitor will even view all—or any—of these images.
Just as we’ve done with the widget body, you can defer loading non-essential images
until they’re actually viewed—or close to being viewed—by the browser user. In this

example, user avatars are a terrific candidate for being deferred, because they’re not
critical to the widget’s operation. To do that, we use a placeholder avatar image for
each comment until the user is about to read a comment, after which we substitute the
placeholder with the real avatar image. Instead of requesting 50 images at once, this
causes image downloads to be spread out as the user scrolls down the page.
Here’s an example avatar img tag generated by Disqus:
<img src="http://cdn.disqus.com/img/default_avatar.png"
data-dsq-src="http://cdn.disqus.com/img/avatars/25551.png"
class="dsq-deferred-avatar" alt="User avatar"/>
You’ll notice that this image element displays a generic default image avatar, while the

real avatar URL is “hidden” inside of the data-dsq-src attribute (it’s intentionally pre-
fixed with dsq to avoid conflicts with other scripts). All that the application needs to

do is substitute the regular src attribute for the contents of the data-dsq-src attri-
bute, and the real image will be downloaded by the browser and finally displayed.

The next question: when should the real avatar be loaded? The obvious answer

would be when a user looks at the comment—when an element containing the com-
ment and corresponding avatar is inside the browser’s viewport. Listing 9.3 is example

code that iterates through the all of the avatars on the page (identified by the simi-
larly-prefixed dsq-deferred-avatar class), determines whether they’re inside the

viewport, and if so replaces their src attribute with the value from data-src.

**Listing 9.3 A function to find and display deferred avatars**

function displayDeferredAvatars() {
var avatars =
document.querySelectorAll('img.dsq-deferred-avatar');
for (var i = 0, len = avatars.length; i < len; i++) {
(function (el) {
var src = el.getAttribute('data-dsq-src');
if (!src) {
return;
}
if (inViewport(el)) {
el.setAttribute('src', src);
el.className = '';
el.removeAttribute('data-dsq-src');
}
})(avatars[i]);
}
}
This is a simple, brute-force implementation that relies mostly on the helper code we
wrote in listing 9.2. Like the example from that listing, the displayDeferredAvatars

function should be called both when your widget initializes (to display any images cur-
rently in the viewport), and again when the user scrolls the browser by binding to the

window’s scroll event.

This function is a good start, but it has a couple of downsides. First and foremost,
every time this function is called (every scroll event), it iterates over all the images on

the page and uses expensive DOM operations—like querying for the element’s coordi-
nate offsets—on each of them. For a page with a large number of avatars, performing

those operations could be costly, and even momentarily lock up the browser. Second,
since the avatar only gets substituted when it enters the viewport, there’s a short

period when the user will see the default avatar while the real avatar is being down-
loaded. Ideally, the application should fetch the real image a little sooner, to try to

avoid that swapping effect.

Listing 9.4 shows an optimized and improved version of the displayDeferred-
Avatars function. This version uses a binary search to more efficiently locate avatars

in the viewport, avoiding the need to touch the DOM for every avatar. This requires
that the avatars be displayed in the same order as they’re queried from the DOM—if
they aren’t, this won’t work. Additionally, it substitutes avatars early by expanding the
size of the viewport according to a predefined padding value (in pixels).

**Listing 9.4 Optimized version of displayDeferredAvatars function**

function displayDeferredAvatars() {
var PADDING = 200;
var all = document.querySelectorAll('img.dsq-deferred-avatar');
var avatars = [];
for (var i = 0; i < all.length; i++) {
if (all[i].offsetParent) {
avatars.push(all[i]);
}
}
if (!avatars.length) {
return;
}
var win = getWindowDimensions();
function clipsViewport(el) {
var pos = getPosition(el);
var top = pos.top;
var bot = pos.top + el.offsetHeight;
if (bot <= win.scrollTop - PADDING) {
return -1;
} else if (top >= win.scrollTop + win.height + PADDING) {
return 1;
} else {
return 0;
}
}
var pivot = (function() {
var high = avatars.length;
var low = 0;
var clip;
var i;

while (low < high) {
i = parseInt((low + high) / 2, 10);
clip = clipsViewport(avatars[i]);
if (clip === -1) { // above
low = i + 1;
} else if (clip === 1) { // below
high = i;
} else {
return i;
}
}
return -1;
})();
if (pivot === -1) {
return;
}
function displayIfVisible(i) {
var el = avatars[i];
if (clipsViewport(el) !== 0) {
return false;
}
el.setAttribute('src', el.getAttribute('data-dsq-src'));
el.className = '';
el.removeAttribute('data-dsq-src');
return true;
}
for (i = pivot; i >= 0 && displayIfVisible(i); i--) {}
for (i = pivot + 1;
i < avatars.length && displayIfVisible(i);
i++) {}
}

Now, your widget or application may not use avatar images like Disqus, but that’s
okay—you can easily convert this example code to work with other types of images.
This implementation of displayDeferredAvatars is certainly improved, but it’s

still not perfect. The binary search is a clever algorithmic optimization, but the func-
tion still performs some operations that might make serious JavaScript optimizers

wince. We’ll highlight these trouble areas and others in the next section, which
focuses on optimizing JavaScript code.

---

#### From [[_1_optimizing-payload]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_optimizing-payload]: _1_optimizing-payload "Optimizing Payload"
[//end]: # "Autogenerated link references"

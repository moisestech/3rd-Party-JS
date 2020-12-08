# Loading Additional Files

## 2.4 Loading Additional Files

It’s possible that your third-party script might only rely on a single JavaScript file. But
it’s more likely that, like most web applications, your script will depend on a number
of supporting files. These could be files of your own (such as a script that contains
helper functions), or files that contain helpful JavaScript libraries, like jQuery or

Prototype.js. Often you’ll need to have these files in place before your script can per-
form any further actions.

You could always expand the script include snippet to include any additional files
you intend to load. For example, you could ask publishers to include the following
code on their pages:

<script src="http://camerastork.com/widget/jquery.js"></script>
<script src="http://camerastork.com/widget/helpers.js"></script>
<script src="http://camerastork.com/widget.js"></script>

This will work; it loads the supporting files (jquery.js, helpers.js) before the main wid-
get file (widget.js). But altering the script include snippet like this is a really bad idea.

Colossally bad. It’s bad because it’s incredibly inflexible; you’re committed to always
loading these files. If you ever need to change what files you’re depending on, you

won’t be able to, because this code is stuck on the publishers’ pages. And getting pub-
lishers to update their HTML source with new code is notoriously difficult.

What you want to do is stick with the original script include snippet, which loads a
single script file that serves as the entry point for the application. After that initial
script is loaded, you’ll then load any additional supporting files dynamically using
JavaScript. In this section, you’ll learn how to do that, beginning with plain JavaScript
files that you’ve written, and then moving on to popular JavaScript libraries.

## 2.4.1 JavaScript Files

To load additional JavaScript files from your application, you’ll use the same tech-
nique from the asynchronous script include snippet. If you recall, that snippet created

a new <script> tag element using document.createElement, and then appended it to
the DOM.
This time around, there are a few catches. For starters, you’ll need to know when
the file has been loaded by the browser before you can execute any functions it

declares. Second, seeing as you might have to load several JavaScript files in this fash-
ion, you’ll want to encapsulate this code in a reusable function.

Figuring out when a file has been loaded isn’t too tricky. The browser has events
that can be listened to that report when a file has been loaded (see figure 2.5). The
only catch is that different browsers support different onload events, and you’ll need

to support all of them. Also, since you don’t have any helpful JavaScript libraries avail-
able (yet), you’ll need to write the raw event-handling code yourself.

The following listing shows an implementation of such a script-loading function,
which we’ve dubbed loadScript. The behavior of this function is visualized in figure 2.5.

**Figure 2.5 The loadScript function loads a JavaScript file from the provided URL and invokes a callback function when that files is ready.**

**Listing 2.3 Asynchronous JavaScriopt loader function**

Listing 2.4 shows loadScript in action. Imagine that you depend on a file named
dom.js that contains a number of cross-browser DOM helper functions critical to your
application. The contents of dom.js might look something like this listing.

Organizing your code into modules like this isn’t required; it’s a convention we’ve
used in developing our own JavaScript apps. We find grouping functions into logically

separated modules makes good sense. You could alternatively place new utility meth-
ods directly on the top-level namespace object. As long as you’re not declaring new

global functions, we won’t object.
To load dom.js and gain access to its helper methods, loadScript is used as follows:

Don’t forget that loadScript loads files asynchronously. In this example, the DOM
helper functions aren’t ready until loadScript’s callback parameter has executed.
Invoking the helper functions immediately after calling loadScript will throw an
exception; the code hasn’t been loaded yet.

## 2.4.2 Libraries

Unless you’re a JavaScript guru, it’s probably unlikely that you’ll be developing this
application using your own DOM helper functions. And why should you? There are

dozens of popular, tested JavaScript libraries out there that have already solved cross-
browser DOM manipulation. As you’ve probably heard hundreds of times, “Why re-
invent the wheel?”

Let’s say that you’ve decided to use the jQuery library in your application. If you’re
not familiar with it, jQuery is currently the most popular JavaScript library on the web.
It provides a concise API for querying and manipulating the DOM, and a number of
helper methods for handling events, performing animations, and making AJAX calls.
jQuery can go a long way toward reducing the amount of code you’ll need to write in
developing third-party JavaScript applications—not to mention the Camera Stork
product widget. Let’s make it a dependency, and load jquery.js using the loadScript
helper function.
Unfortunately, loading a commonly used library like jQuery poses a hurdle for
third-party applications. jQuery itself is nearly ubiquitous,4

and there’s a high chance
that a given publisher may have already loaded it on their own pages. If you try to load
jQuery when it already exists on the page, the two will likely conflict.
NAMESPACES AND NOCONFLICT
The trick to using common libraries is namespacing. You want to load your copy of the
jQuery object inside your application namespace, and only use that copy. Often this
will mean having to modify the library source code itself.

In the case of jQuery, this is easy. The jQuery authors have been kind enough to
provide a helper function called noConflict that allows you to reassign the global
jQuery object ($) to another variable. You can use this function to assign the jQuery
object to your application namespace. To do so, you’ll need to amend the jQuery
source code (jquery.js), as shown in the next listing.

**Listing 2.5 Namespacing the jQuery object**

Not all libraries will have a helper function like jQuery’s noConflict. Depending on
the library, you may have to go to greater lengths to ensure that your file includes
don’t conflict with versions that already exist on the global namespace.

WHAT ABOUT LIBRARIES LOADED FROM A CDN? Google, Microsoft, and other

companies offer free hosting of popular JavaScript libraries on high perfor-
mance CDNs (content delivery networks). These services are great because

CDNs are distributed around the world, serve files ridiculously fast, and free
you from having to serve files yourself. The downside is that you can’t modify
the original library source code, which makes it difficult for use in embedded
scripts.

WHAT ABOUT LIBRARIES LOADED FROM A CDN? Google, Microsoft, and other

companies offer free hosting of popular JavaScript libraries on high perfor-
mance CDNs (content delivery networks). These services are great because

CDNs are distributed around the world, serve files ridiculously fast, and free
you from having to serve files yourself. The downside is that you can’t modify
the original library source code, which makes it difficult for use in embedded
scripts.

SCRIPT AND ASSET LOADERS

Now that we’ve traipsed through the wonderful world of asset loading, you’re proba-
bly wondering to yourself, “Isn’t this a solved problem?” You’re on to something. A

number of commonly used libraries known as script loaders will handle this work for
you. LABjs, a popular script-loading library developed by Kyle Simpson with guidance
from web performance guru Steve Souders (see http://labjs.com/), is optimized to
use different loading techniques depending on the browser or environment. LABjs is
also capable of queueing multiple file loads at once and maintaining execution order
of scripts. LABjs isn’t alone in this endeavor—other libraries, like RequireJS and
yepnope.js, provide varying feature sets centered around loading both JavaScript and
other asset files.
Even if LABjs and other script loaders have you covered, that doesn’t mean you
should forget everything you just learned. There’s value in learning techniques from

first principles. In this book, we try hard to cover basic solutions for third-party pro-
gramming problems, like the JavaScript loading examples we covered in this section.

You’re encouraged to use your own choice of libraries and helper code when it makes
sense. Just listen to your heart; we know you’ll do the right thing.

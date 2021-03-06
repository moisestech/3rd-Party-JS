# Libraries

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

---

#### From [[_4_loading-additional]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_4_loading-additional]: _4_loading-additional "Loading Additional Files"
[//end]: # "Autogenerated link references"

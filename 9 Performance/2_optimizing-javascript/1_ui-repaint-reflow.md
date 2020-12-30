# ui, repaing, reflow

## 9.2.1 Inside the browser: UI Thread, Repaint and Reflow

Every browser has what’s known as a UI thread that’s responsible for both JavaScript
execution and UI updates, like drawing elements on the page. Because UI updates can
affect the flow of JavaScript code, and vice versa, only one thing can happen at a
time—if the thread is busy, all other jobs for UI updates and JavaScript execution will
become queued. Only when the UI thread has finished performing its current task
will it pick a new job from the queue—either another UI update or JavaScript code
execution.
Because of this nonparallel nature of UI threads, JavaScript executing on the page
can prevent UI updates, making the page unresponsive. If your code is running slow,
the browser will simply queue up all UI updates no matter where they came from—

your third-party application or the host page. But what are these UI updates we’re talk-
ing about? UI updates can be split into two main categories: reflow and repaint (also

sometimes referred to as redraw).
MORE ABOUT HIGH-PERFORMANCE JAVASCRIPT If you want to learn more about
UI threads and other JavaScript performance tips, we recommend you pick
up High Performance JavaScript by Nicholas Zakas (O’Reilly, 2010). Nicholas
has also given a number of informative talks on this subject that are available
on YouTube.
Reflow occurs when the DOM is modified such that the geometry of an element is

changed. When this happens, the browser must traverse through the DOM and recal-
culate the dimensions of any possibly affected elements. Many DOM operations or

style changes can cause reflows:

Adding, removing, or changing the location of an element
 Changing the size of an element—either by changing its contents (innerText)
or through a style change (width, height, padding, and so on)
 Resizing the window

Repaint occurs after the browser has completed a reflow. It literally paints the ele-
ments on the screen after their dimensions have been calculated. Sometimes a repaint

can occur without a reflow, whenever a change occurs that doesn’t require recalculat-
ing geometry. An example of this is changing an element’s foreground color—the

browser doesn’t need to reflow the DOM in order to change a paragraph’s text color.
Both reflow and repaint are expensive in terms of performance and are often the

source of your JavaScript performance problems. Browsers try to improve perfor-
mance by bundling various reflow and repaint events together before executing

them, but some operations cause the browser to stop everything and reflow the cur-
rent DOM tree.

Remember earlier in this chapter, when talking about deferring images, we said
that accessing an element’s offset coordinates (offsetTop and offsetLeft) is an

expensive operation? That’s because your browser has to make sure that it has up-to-
date information about the element’s position before it can return the correct result.

And the only way browsers can do this is by forcing a DOM tree reflow. This means that
the browser’s UI thread must execute all queued jobs, because they might modify the
DOM in a way that changes the value of the requested property. And only after that
forced reflow will the browser calculate the current coordinate offset and return it to
the callee.
With this knowledge handy, you might recognize why the previous version of the
deferred avatar solution wasn’t sufficient. First, it queries the window’s viewport
dimensions, which can potentially force a DOM reflow. Second, every time it loads a

new avatar, it causes a repaint operation (or possibly even a reflow, if the image dimen-
sions change). And because of the synchronous nature of the UI thread, both of these

operations can tie up other scripts and UI updates. This wouldn’t be so bad were this
function called infrequently, but because it’s bound to the window’s scroll event,
which can fire multiple times per second, this function could conceivably make the
whole page unresponsive while the user is scrolling.

---

#### From [[_2_optimizing-js]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_optimizing-js]: _2_optimizing-js "Optimizing JS"
[//end]: # "Autogenerated link references"
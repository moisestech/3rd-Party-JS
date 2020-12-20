# 8️⃣ 3rd Party JS SDK

## This Chapter Covers

1. Initializing an SDK synchronously & asynchronously
2. Exposing public functions
3. Versioning Techniques
4. Wrapping and communicating with a Web Services API

## **Intro**

Let’s flash-forward to the not-so-distant future. The Camera Stork product widget
has been an unparalleled success, and is helping drive hundreds of thousands of
visitors from publisher websites to camerastork.com each month. Publishers, too,
are sharing in the success; you’ve even introduced a revenue-sharing program that
pays publishers a share of each sale you make from visitors that are referred
through the widget. And all of this thanks to a little book on third-party
JavaScript—who knew?
It gets better. During this period, you’ve introduced new widgets to help
increase traffic (and dollars), like the Top Sellers widget, which lists the current

best-selling products on camerastork.com. These widgets are all served using sepa-
rate, individual JavaScript files, loaded in the same fashion as your original product

widget. You’ve also developed the Camera Stork Web Service API, a set of HTTP end-
points that provides programmatic access to the Camera Stork’s product and reviews

database. Developers have been using this API to build revenue-generating applica-
tions that help refer you sales.

Publishers have become so enamored with the Camera Stork suite of publisher
tools that they’re looking for deeper integration with your platform. Some publishers
want more control over how and when they load your various widgets. Others want to
know when events are occurring within your software, like when a referral link is
clicked, so that they can collect their own statistics about how their visitors are using
your widgets. And some publishers want a simple set of client-side JavaScript functions

for accessing your valuable web service API. And they don’t want a set of single-
purpose, automatically executing scripts to provide these features; they want a single,

powerful JavaScript library that does it all.
The solution is a third-party JavaScript SDK (software development kit). This is a
JavaScript library that bundles an entire suite of JavaScript functionalities and exposes
them via documented public functions. So, rather than have single-purpose scripts
that fulfill small pieces of functionality (like individual widgets), you have publishers
load a single JavaScript file that provides access to all of your tools: widgets, internal
event bindings, your web service API, and more (see figure 8.1).
To give you an idea, let’s look at a public function that could be provided through
an SDK. Suppose that the Camera Stork JavaScript SDK already exists and provides a
function named Stork.productWidget, which renders a product widget instance on
the publisher’s page. This function accepts two arguments: the ID of the product to be
rendered, and the ID of a DOM element on the publisher’s page into which the widget
will be inserted. A publisher might use the following code to invoke the function:

This function effectively replaces the single-purpose script you wrote for initializing and
rendering a product widget (widget.js). On its own, it may not seem exciting. But even
this small function provides powerful advantages that a self-executing script doesn’t.
For instance, the publisher can choose when to initialize the widget, instead of having
it occur automatically—a must for AJAX-heavy web pages where content might be
deferred. In addition, it’s now easy for the publisher to instantiate multiple instances of
the product widget on their page without having to include multiple copies of the script

include snippet. Forgoing those extra script includes can lead to significant perfor-
mance gains, and ultimately requires less effort on the part of the publisher.

We just looked at one public function provided via an SDK—let’s look at another.
Suppose that after calling Stork.productWidget, the publisher wants to be notified
when visitors interact with the rendered widget. Perhaps a publisher wants to know

when a referral link inside the widget has been clicked, or when a review has been sub-
mitted by the visitor.

To solve this, your SDK will expose a function, Stork.listen, that lets publishers
listen to key application events and invoke a callback function when they occur. In the

following example, the publisher is using Stork.listen to listen to the review-
Submitted event, which occurs when a visitor submits a review through the widget:

Stork.listen('reviewSubmitted', function() {
alert("The user submitted a review!");
});
Again, this is a deeper integration with your software than just rendering a widget. By
providing a JavaScript SDK, you’re empowering publishers to programmatically define
how your software interacts with their content. For these reasons, in many circles it’s
considered the preferred way of distributing third-party applications—even though it
may require more programming know-how on the part of your publishers.
SOFTWARE DEVELOPMENT KITS SDK is a relatively new term in the world of
client-side JavaScript, but it’s common in software development. Nominally, it

refers to a set of tools that enable the development of applications for a spe-
cific hardware or software platform. For example, the iOS SDK is a set of

Objective C libraries for developing native applications for Apple’s iOS
mobile operating system (https://developer.apple.com/devcenter/ios/).
These libraries expose public functions for interacting with the iOS device’s
hardware, like rendering to the screen or taking a picture with an onboard

camera. JavaScript SDKs function under a similar premise, but instead of pro-
viding functions that interact with hardware, they expose JavaScript functions

for developing or integrating with a particular web software platform.
In this chapter, through the lens of the Camera Stork example, we’ll teach you how to
implement a third-party JavaScript SDK, which will become the primary means
through which publishers use your client-side code. We’ll start by implementing SDK
basics: initialization, exposing some simple public functions, and providing bindable
event hooks. Afterward, we’ll move into advanced topics, like versioning your SDK and
using your SDK to wrap web service APIs.

## **8.1 Implementing a bare-bones SDK**

[[_1_sdk-implementation]]

## **8.2 Versioning**

[[_2_versioning]]

## **8.3 Wrapping web service APIs**

[[_3_wrapping-apis]]

## **8.4 Summary**

- **Distributing a third-party JavaScript SDK** is the natural evolution of any third-party JavaScript application.
- Providing **easy-to-use functions that initialize and execute your application instances gives publishers more control over how they integrate your software with their web properties**.
- In this chapter, we’ve covered the **basics of converting your third-party application into a JavaScript SDK**.
- You learned:
  - **how to expose and implement an initialization method**,
  - as well as several **possible techniques for guiding publishers to load your SDK asynchronously**.

#### Project Widget

- You also **implemented basic public functions for instantiating a Camera Stork product widget**
  - and for **binding publisher callback functions to internal SDK events**.
- Last, you **learned how an SDK is an excellent tool for providing client-side access to a web services API**
  - and **implemented a public function (Stork.api)** that does so.
- Give yourself a pat on the back, because at this point, we’ve covered most of the functional basics of implementing third-party JavaScript applications.

#### Next Chapter

[[_performance]]

- In the next chapter, we’ll look at optimizing the performance of these applications.
- After all, what good is a third-party application if it’s slow?

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_sdk-implementation]: 1_sdk-implementation/_1_sdk-implementation "SDK Implementation"
[_2_versioning]: 2_versioning/_2_versioning "Versioning"
[_performance]: ../9 Performance/_performance "9️⃣ Performance"
[//end]: # "Autogenerated link references"

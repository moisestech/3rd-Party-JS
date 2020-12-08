# Uses

## 1.2 The many uses of Third-Party Javascript

We’ve established that third-party JavaScript is code that’s being executed on someone
else’s website. This gives third-party code access to that website’s HTML elements and
JavaScript context. You can then manipulate that page in a number of ways, which

might include creating new elements on the DOM (Document Object Model), insert-
ing custom stylesheets, and registering browser events for capturing user actions. For

the most part, third-party scripts can perform any operation you might use JavaScript
for on your own website or application, but instead, on someone else’s.

**Figure 1.3 A Script-Loading snippet placed on the publisher's web page loads third-party JavaScript code.**

Armed with the power of remote web page manipulation, the question remains: what
is it good for? In this section, we’ll look at some real-world use cases of third-party
scripts:

- Embedded widgets—Small interactive applications embedded on the publisher’s
  web page
- Analytics and metrics—For gathering intelligence about visitors and how they
  interact with the publisher’s website
- Web service API wrappers—For developing client-side applications that communi-
  cate with external web services

This isn’t a complete list, but should give you a solid idea of what third-party JavaScript
is capable of. We’ll start with an in-depth look at the first item: embedded widgets.

## 1.2.1 Embedded Widgets

Embedded widgets (often third-party widgets) are perhaps the most common use case

of third-party scripts. These are typically small, interactive applications that are ren-
dered and made accessible on a publisher’s website, but load and submit resources to

and from a separate set of servers. Widgets can vary widely in complexity; they can be
as simple as a graphic that displays the weather in your geographic location, or as
complex as a full-featured instant messaging client.
Widgets enable website publishers to embed applications into their web pages with
little effort. They’re typically easy to install; more often than not publishers need only
insert a small HTML snippet into their web page source code to get started. Since
they’re entirely JavaScript-based, widgets don’t require the publisher to install and
maintain any software that executes on their servers, which means less maintenance
and upkeep.

Some businesses are built entirely on the development and distribution of embed-
ded widgets. Earlier we mentioned Disqus, a web startup based in San Francisco. Dis-
qus develops a commenting widget (see figure 1.3) that serves as a drop-in

commenting section for blogs, online publications, and other websites. Their product

is driven almost entirely by third-party JavaScript. It uses JavaScript to fetch comment-
ing data from the server, render the comments as HTML on the page, and capture

form data from other commenters—in other words, everything. It’s installed on web-
sites using a simple HTML snippet that totals five lines of code.

Disqus is an example of a product that’s only usable in its distributed form; you’ll
need to visit a publisher’s page to use it. But widgets aren’t always standalone products
like this. Often they’re “portable” extensions of larger, more traditional stay-at-home
web applications.
For example, consider Google Maps, arguably the web’s most popular mapping
application. Users browse to https://maps.google.com to view interactive maps of
locations all over the world. Google Maps also provides directions by car and public
transit, satellite imagery, and even street views using on-location photography.

**Figure 1.3 An example commenting section on a pubglisher's website, powere by the Disques commenting widget**

Incredibly, all of this magic also comes in a widget flavor.
Publishers can embed the maps application on their own web pages using some simple JavaScript code snippets obtained from the Google Maps website.
On top of this, Google provides a set of public functions for publishers to modify the map contents.

Let’s see how simple it is to embed an interactive map on your web page using Google Maps (listing 1.2).
This code example begins by first pointing to the Maps JavaScript library using a simple script include.
Then, when the body’s onload handler fires, you check whether the current browser is compatible, and if so, initialize a new
map and center it at the given coordinates.1

We’re done, and all it took was roughly 10 lines of code—powerful stuff!

**Listing 1.2 Initializing the Google Maps Widget**

We just looked at two examples of embedded widgets. But really, any application idea
is fair game for embedding on a publisher’s page. In our own travels, we’ve come

across a wide variety of widgets: content management widgets, widgets that play real-
time video, widgets that let you chat in real time with a customer support person, and

so on. If you can dream it, you can embed it.

## Analytics and Metrics

Third-party JavaScript isn’t used exclusively in the creation of embedded widgets.

There are other uses that don’t necessarily involve graphical, interactive web page ele-
ments. Often they’re silent scripts that process information on the publisher’s page

without the user ever knowing they’re there. The most common such use case is in
analytics and metrics gathering.
One of JavaScript’s most powerful features is that it enables developers to capture
and respond to user events as they occur on a web page. For example, you can write
JavaScript to respond to a website visitor’s mouse movements and/or mouse clicks.
Third-party scripts are no exception: they too can observe browser events and capture
data about how the visitor interacts with the publisher’s page. This might include
tracking how long a visitor stays on a page before moving on, what content they saw
while they were reading the page, and where they went afterward. There are dozens of

browser events your JavaScript code can hook into from which you could derive hun-
dreds of different insights.

PASSIVE SCRIPTS

Crazy Egg, another web startup, is one example of an organization that uses third-
party scripts in this way. Their analytics product generates visualizations of user activity

on your web page (see figure 1.4). To obtain this data, Crazy Egg distributes a script to
publishers that captures the mouse and scroll events of web page visitors. This data is
submitted back to Crazy Egg’s servers, all in the same script. The visualizations Crazy
Egg generates help publishers identify which parts of their website are being accessed
frequently, and which are being ignored. Publishers use this information to improve
their web design and optimize their content.

**Figure 1.4 Crazy Egg's hear map visualization highlights trafficked areas of publishers websites**

Crazy Egg’s third-party script is considered a passive script; it records statistical data
without any interaction from the publisher. The publisher is solely responsible for
including the script on the page. The rest happens automatically.
ACTIVE SCRIPTS
Not all analytics scripts behave passively. Mixpanel is an analytics company whose

product tracks publisher-defined user actions to generate statistics about website visi-
tors or application users. Instead of generic web statistics, like page views or visitors,

Mixpanel has publishers define key application events they want to track. Some exam-
ple events might be “user clicked the signup button,” or “user played a video.” Pub-
lishers write simple JavaScript code (see listing 1.3) to identify when the action takes

place and then call a tracking method provided by Mixpanel’s third-party scripts to

register the event with their service. Mixpanel then assembles this data into interest-
ing funnel statistics to help answer questions like, “What series of steps do users take

before upgrading the product?”

**Listing 1.4 Tracking user sighups with the Mixpanel JS API**

There’s something else interesting about Mixpanel’s use of third-party scripting. In
actuality, Mixpanel provides a set of client-side functions that communicate with their
web service API—a set of server HTTP endpoints that both track and report on events.
This is a practical use case that can be extended to any number of different services.
Let’s learn more.

## 1.2.3 Web Serive API Wrappers

In case you’re not familiar with them, web service APIs are HTTP server endpoints that
enable programmatic access to a web service. Unlike server applications that return
HTML to be consumed by a web browser, these endpoints accept and respond with
structured data—usually in JSON or XML formats—to be consumed by a computer
program. This program could be a desktop application or an application running on

a web server, or it could even be client JavaScript code hosted on a web page but exe-
cuting in a user’s browser.

This last use case—JavaScript code running in the browser—is what we’re most

interested in. Web service API providers can give developers building on their plat-
form—often called integrators—third-party scripts that simplify client-side access to

their API. We like to call these scripts web service API wrappers, since they’re effectively
JavaScript libraries that “wrap” the functionality of a web service API.
EXAMPLE: THE FACEBOOK GRAPH API
How is this useful? Let’s look at an example. Suppose there’s an independent web
developer named Jill who’s tired of freelance work and looking to score a full-time
job. Jill’s decided that in order to better appeal to potential employers, she needs a
terrific-looking online resume hosted on her personal website. This resume is for the
most part static—it lists her skills and her prior work experience, and even mentions
her fondness for moonlight kayaking.
Jill’s decided that, in order to demonstrate her web development prowess, there
ought to be a dynamic element to her resume as well. And she’s got the perfect idea.
What if visitors to Jill’s online resume—potential employers—could see if they had any
friends or acquaintances in common with Jill (see figure 1.5)? Not only would this be
a clever demonstration of
Jill’s skills, but having a
common friend could be a
great way of getting her
foot in the door.
To implement her
dynamic resume, Jill uses
Facebook’s Graph API. This
is a web service API from

Facebook that enables soft-
ware applications to access

or modify live Facebook
user data (with permission,

**Figure 1.5 At the bottom of Jill's resume, the visitor can see friends they share with Jill**

of course). Facebook also has a JavaScript library that provides functions for communi-
cating with the API. Using this library, it’s possible for Jill to write client-side code that

can find and display friends common to herself and a visitor to her resume. Figure 1.6
illustrates the sequence of events that occur between the browser and the two servers.

**Firegure 1.6 Embedding Facebook content in a website using client-side JavaScript**

Listing 1.4 shows the code to implement this feature on her resume. To keep things
simple, this example uses jQuery, a JavaScript library, to simplify DOM operations.
Learn more at http://jquery.com.

**Listing 1.4 Using Facebook's Graph API to fetch and display a list of mutual friends**

Jill managed to embed some powerful functionality in her resume, all using a small
amount of client-side JavaScript. With this impressive piece of work, she should have
no problem landing a top-flight software job.

BENEFITS OF CLIENT-SIDE API ACCESS
It’s worth pointing out that this entire example could’ve been done without client-side
JavaScript. Instead, Jill could’ve written a server application to query the Facebook
Graph API for the necessary data and then render the result as HTML in the response
Listing 1.4 Using Facebook’s Graph API to fetch and display a list of mutual friends

Load both jQuery
and Facebook
JavaScript SDK.

Initialize Facebook JavaScript
SDK. You need to register
your application at http://
developers.facebook.com and
obtain an application ID first.

FB.login opens up
new Facebook
window asking
website visitor to
log in and grant
the application
permission to
access the
visitor’s data.

If login was
successful,
request visitor’s
mutual Facebook
friends using the
Graph API’s /
mutualfriends/
endpoint. When
response is ready,

execute showMutualFriends callback function.

Iterate through list of friends and render them to page.

Developing a bare-bones widget 13
to the browser. In that case, the browser downloads the HTML from Jill’s server and
displays the result to the user—no JavaScript is executed.
But it’s arguably better to have the website visitor perform this work in the browser,
for a few reasons:

- Code executing in the browser is code that’s not executing on the integrator’s
  servers, which can lead to bandwidth and CPU savings.

- It’s faster—the server implementation has to wait for the response from Face-
  book’s API before showing any content.

- Some websites are completely static, such that client-side JavaScript is their only
  means of accessing a web service API.
  AN API FOR EVERY SEASON This example we just covered might be regarded
  as a niche use case, but this is just one possible application. Facebook is just a
  single web service API provider, but the reality is that there are thousands of
  popular APIs, all of which provide access to varying data and/or functionality.
  Besides social networking applications like Facebook, Twitter, and LinkedIn,

there are publishing platforms like Blogger and WordPress, or search applica-
tions like Google and Bing, all of which provide varying degrees of access to

their data via APIs.
Many web services—large and small—offer APIs. But not all of them have gone the
extra mile of providing a JavaScript library for client-side access. This matters because
JavaScript in the browser is the single largest development platform: it’s supported on
every website, in every browser. If you or your organization develops or maintains a

web service API, and you want to reach the largest number of possible integrators pos-
sible, you owe it to yourself to provide developers with a client-side API wrapper—a

topic we’ll discuss in detail later in this book.

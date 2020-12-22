# Dev Widget

## **1.3 Developing a bare-bones Widget**

We’ve explored some popular uses for third-party JavaScript. You’ve seen how it can
be used in the development of widgets, in analytics gathering, and as a client-side
wrapper for web service APIs. Hopefully this has given you an idea of what’s possible
when you’re designing your own third-party application.

Now that you’ve walked through some real-world exam-
ples, it’s time to develop something yourself. Let’s start with

something fairly simple: a bare-bones embedded widget.
Pretend for a moment that you run a website that provides
up-to-the-minute local weather information. Traditionally,
users visit your website directly in order to get the latest
weather news. But in order to reach a wider audience, you’ve
decided to go a step further and give users access to your data

outside of your website. You’ll do this by providing an embed-
dable widget version of your service, depicted in figure 1.7.

**Figure 1.7 The rather widget as it will appear on the publisher's page**

You’ll market this widget to publishers who are interested in providing their readers
with local weather information, with the easy installation of a third-party script.
Luckily, you’ve already found a publisher who’s interested, and they’ve signed on
to test-drive your widget. To get them started, you’ll need to provide them with an
HTML snippet that will load the weather widget on their web page. The publisher will
copy and paste this snippet into their HTML source code at the location where they
want the widget to appear. The snippet itself is simple: it’s a <script> tag pointed to a
third-party JavaScript file hosted on your servers at weathernearby.com:

```html
<script src="http://weathernearby.com/widget.js?zip=94105"></script>
```

You’ll notice the URL for this script element contains a single parameter, zip. This is
how you’ll identify what location to render weather information for.
Now when the browser loads the publisher’s web page, it’ll encounter this

<script> tag and request widget.js from your servers at weathernearby.com. When
widget.js is downloaded and executed, it’ll render the weather widget directly into the
publisher’s page. That’s the goal, at least.
To do this, widget.js will need to have access to the company’s weather data. This

data could be published directly into the script file, but given that there are approxi-
mately 43,000 US ZIP codes, that’s too much data to serve in a single request. Unless

the user is connecting from Sweden or South Korea, where 100 Mbps connections are
the norm, it’s clear that the widget will need to make separate requests for the weather
data. This is typically done using AJAX, but for simplicity we’ll use a different
approach: server-side script generation.

## **Sub-chapters**

- [[1_server-js-generation]]
- [[2_dist-iframe-widgets]]

---

#### From [[_3rd-party-js]]

# 4️⃣ Server Com

## **This Chapter Covers**

1. Same-origin policy (SOP)
2. Techniques to enable cross-domain messaging around the SOP
3. Security implications associated with SOP workarounds
4. Cross-origin resource sharing (CORS)

## **Intro**

In previous chapters you learned how to distribute, load, and render a third-party
JavaScript application on the publisher’s web page. You’re off to a great start, but so
far your application only has access to the predefined data embedded in your
JavaScript files. Unless you’re dealing with small, unchanging datasets, at some
point you’ll need to make dynamic requests for data from your servers. And if your
application is collecting data, either passively or directly via user input, you’ll likely
want to push that data to your servers too.

Let’s go back to the Camera Stork widget example from the previous two chap-
ters. As the premier destination for cameras and camera accessories online, the

Camera Stork inventory is huge and constantly changing. There’s no way it could be

embedded in a single JavaScript file. Instead, when you know which product the pub-
lisher is trying to render, you’ll need to make an individual request for the latest data

for that product from your servers. Furthermore, when you begin collecting ratings
and reviews from users via the widget, you’ll want to be able to submit those ratings to
your servers as well. To do all this, your application has to have a communication

channel with your servers. Building such channels is the topic of this and the follow-
ing chapter, our two-part discussion on communicating with the server.

If you’re an experienced web developer, you might think that sending data from
JavaScript running in the browser to your web servers is a simple task. In an ordinary
web application, you’d be right. But third-party applications almost always necessitate
making what are known as cross-domain requests: HTTP requests originating from a
web page on one domain to a second, separate domain. An example would be an
HTTP request originating from a publisher’s web page located at publisher.com to
your servers (camerastork.com) for product data.
These cross-domain requests cause all sorts of trouble for third-party scripters—for
instance, you can’t use XmlHttpRequest, the go-to tool for making dynamic HTTP
requests from JavaScript. Instead, you’ll have to use hacks and workarounds that differ
in speed, complexity, and browser support.
In this chapter, we’ll go over an initial set of techniques you can use to send data
back to your servers: subdomain proxies, JSONP, and the cross-origin resource sharing

specification (CORS). They all differ in their abilities—some work only between sub-
domains; others don’t support the full range of HTTP methods—but all of them can

prove themselves useful under certain circumstances. Real-world, third-party applica-
tions usually use a healthy mix of all these techniques.

But before we get started, let’s see why cross-domain requests are hard and why you
can’t use good ol’ XMLHttpRequest.

## **4.1 AJAX and the Browser Same-Origin Policy**

[[_1_ajax-same-origin-policy]]

## **4.2 JSON with padding (JSONP)**

[[_2_jsonp]]

## **4.3 Subdomain Proxies**

[[_3_subdomain-proxies]]

## **4.4 Cross-origin Resource Sharing**

[[_4_cross-origin-resource-sharing]]

## **4.5 Summary**

- This chapter was dedicated to the same-origin policy and basic techniques you can use to bypass it.
- You should now have a clear understanding of what SOP is, how it works, what types of communication it affects, and what options you have when trying to send messages across different domains.
- The natural question now is how to decide what technique to use for your application.
- We recommend always starting with CORS simply because it’s the most stable, documented, and future-proof way of making cross-domain requests.
- For browsers that don’t support CORS, the easiest alternative is to use JSONP, but it’ll mean giving up making POST requests from your third-party application.
- You won’t be able to upload files or send long user reviews, for instance. Subdomain proxies aren’t restricted in the types of HTTP requests they use, but they only permit messaging between subdomains, and only make sense if you have a small number of publishers.
- As far as the Camera Stork product widget is concerned, none of these solutions are really up to the task.
- We want to be able to make POST requests, support as many browsers as possible, and have too many publishers for subdomain proxies.
- But what options do we have left?
- The answer is, plenty.
- As you may have noticed in this chapter, some of the techniques we covered rely on the iframe HTML element.
- This element can be powerful and can provide a solid communication channel between documents with different origins.

### Next Chapter

[[_cross-domain-messaging]]

- In the next chapter, we’ll go over additional techniques that rely on the iframe element to pass messages back and forth between domains.
- Get ready to see the iframe element as you’ve never seen it before.

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_ajax-same-origin-policy]: 1_ajax-same-origin-policy/_1_ajax-same-origin-policy "AJAX Same Origin Policy"
[_2_jsonp]: 2_jsonp/_2_jsonp "JSONP"
[_3_subdomain-proxies]: 3_subdomain-proxies/_3_subdomain-proxies "Subdomain Proxies"
[_4_cross-origin-resource-sharing]: 4_cross-origin-resource-sharing/_4_cross-origin-resource-sharing "Cross-Origin Resouce Sharing"
[_cross-domain-messaging]: ../5 Cross-domain iFrame Messaging/_cross-domain-messaging "5️⃣ Cross-Domain Messaging"
[//end]: # "Autogenerated link references"

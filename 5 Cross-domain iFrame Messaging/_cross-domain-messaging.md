# 5️⃣ Cross-Domain Messaging

## This Chapter Covers

1. The role of **iFrames in cross-domain messaging**
2. **HTML5** `window.postMessage API`
3. `window.postMessage` alternatives for legacy browsers
4. **easyXDM** — the cross-domain messaging library

## **Intro**

In chapter 4 you learned about the same-origin policy—a browser security concept
that prohibits pages from different origins from accessing each other’s methods
and properties. You also learned a few tricks—subdomain proxies, JSONP, and
CORS—that allow you to circumvent the SOP in order to send HTTP requests to
your servers.
One of those solutions, subdomain proxies, used iframe elements as a means of
communicating with your servers. It relied upon the fact that documents hosted
inside iframes can freely communicate with URLs on the same domain. But in
order for your third-party JavaScript code to access the iframe and initiate network
requests, the target document needed to reside in the same domain space as the
publisher’s website—using a subdomain proxy. As you learned, asking publishers to

configure dedicated subdomains for your application is a significant burden. But what
if there were another way of accessing iframes?
As you’ve already seen, HTML5 has been a boon for JavaScripters, adding many
useful tools that make our lives easier. Since it has become a living standard,1
browser
vendors have become more involved, and don’t wait long before implementing new
features. One such feature is the window.postMessage API—a messaging system that
allows documents to communicate with each other in a safe and controlled manner,

regardless of origin. This powerful API allows your third-party JavaScript code to com-
municate with external documents hosted inside iframes. These documents could be

cross-domain channels for initiating XmlHttpRequest objects to your servers. Or they
could be UI elements of your application, contained in an iframe in order to prevent
it from being modified or accessed by the publisher’s page.
In this chapter, we’ll cover all things iframe messaging. We’ll start with the HTML5
window.postMessage API and then move on to fallback techniques you can employ to

imitate the postMessage behavior in legacy browsers. Finally, we’ll conclude this chap-
ter by teaching you how to use easyXDM—a popular JavaScript library that encapsu-
lates postMessage and backward-compatible messaging protocols via a single API.

## **5.1 HTML5 window.postMessage API**

[[_1_windows-message-api]]

## **5.2 Fallback Techniques**

[[_2_fallback-techniques]]

## **5.3 Simple Cross-Domain Messaging with easy XDM**

[[_3_easy-xdm]]

## **5.4 Summary**

- This chapter covered one of the most important topics when it comes to third-party JavaScript development:
  - **communication between your application code executing in the publisher’s context**
  - and **iframes hosted on your domain**.
- You should now have a **clear understanding of how cross-domain communication works in modern browsers**
  - and what **workarounds are available for legacy browsers**.
- You should also be familiar with **easyXDM—a library that takes care of all the dirty work for you**.
- If something is still not clear, don’t worry.
- A bit of practice usually helps to **understand how the browser security model works**
  - and **why API designers came up with such things as** the `window.postMessage API`.
- You might be wondering, where do we go from here?

### Project Widget

- Cross-domain messaging is a complicated topic, so we suggest that you take a break and try to implement a feature for your Stork widget that requires it to communicate with the server.
- You can add the ability to:
  - post reviews
  - rate products
  - or even challenge yourself in the analytics field.
- For example, try logging what browsers your users use or what JavaScript libraries are installed on their pages.

#### Examples

- When in doubt, refer to examples from this and previous 4 In chapter 1, we mentioned how Prototype JavaScript library versions 1.7 and below introduce subtle, breaking changes to the built-in JSON object. chapters.
- Sooner or later, you’ll find yourself comfortable dealing with browsers’ idiosyncrasies and using easyXDM to satisfy your cross-domain messaging needs.

- At this point, we assume that:
  - you can load your third-party application on the publisher’s page
  - render elements on the DOM
  - communicate with iframes on your domain
  - and transmit data to and from your servers.

#### Next Chapter

[[_auth-sessions]]

- But we’re not finished. We still have some notable topics to cover, including authentication, security, and troubleshooting, which we’ll cover in the following chapters.
- You’ll learn how to authenticate your users on different sites, how to make your widget faster and more secure, and debugging techniques for after your third-party application is out in the wild.
- But pat yourself on the back, because the hardest part is over.
- It’s all downhill from here!

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_windows-message-api]: 1_windows-message-api/_1_windows-message-api "Windows Message API"
[_2_fallback-techniques]: 2_fallback-techniques/_2_fallback-techniques "Fallback Techniques"
[_3_easy-xdm]: 3_easy-xdm/_3_easy-xdm "Easy XDM"
[_auth-sessions]: ../6 Auth & Sessions/_auth-sessions "6️⃣ Auth & Sessions"
[//end]: # "Autogenerated link references"

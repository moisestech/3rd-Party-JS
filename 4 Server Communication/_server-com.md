# Server Com

1. Same-origin policy (SOP)
2. Techniques to enable cross-domain messaging around the SOP
3. Security implications associated with SOP workarounds
4. Cross-origin resource sharing (CORS)

## **4.1 AJAX and the Browser Same-Origin Policy**

## **4.2 JSON with padding (JSONP)**

## **4.3 Subdomain Proxies**

## **4.4 Cross-origin Resource Sharing**

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
- In the next chapter, we’ll go over additional techniques that rely on the iframe element to pass messages back and forth between domains.
- Get ready to see the iframe element as you’ve never seen it before.

---

From [[3rd-party-js]]

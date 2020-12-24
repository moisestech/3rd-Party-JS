# document.domain

## **4.3.1 Changing a document's origin using document.domain**

First, how do you change a document’s origin? Every DOM core document object has a property called domain. By default, it contains the hostname part of the current ori- gin. You can change the current document’s origin by assigning a new value to this property. For example, to have a page hosted on sub.example.com change its origin to example.com, you’d have that page execute the following JavaScript: document.domain = 'example.com';

You can change the value of document.domain only once per page. For this reason, it’s better to change it early in the document (say, in the <head>) before any other code runs. If you set this value later in the page, it can cause unexpected behavior where some code believes it has one document.domain value, and other code believes it has another.

Presumably, after you’ve made this change, the SOP will no longer prevent docu- ments hosted on example.com from communicating with this document located at sub.example.com. But it’ll still be blocked. There’s one step remaining: the other communicating document on example.com needs to also opt in to the same domain suffix. The following listing shows the two pages opting in to the same domain suffix.

**Listing 4.3 Example of two websites opting in to the same origin space**

You might be wondering why the page on example.com needs to set its document
.domain property to the same value as it was before. This is due to a browser restriction
that prevents a particular security exploit. Consider the following situation: Google
now allows you to publish your profile at profiles.google.com/<your name>. Suppose
one day they allow their users to install custom third-party JavaScript widgets, and one
malicious widget decides to change the origin of the page to google.com. Now, without
the restriction requiring both parties to explicitly opt in to the same domain suffix, the
malicious widget could access any Google site hosted on google.com and access its
properties on the user’s behalf. Browser vendors (correctly) see this as a security flaw
and thus require both parties to explicitly state their intentions.
CHANGING DOCUMENT.DOMAIN WILL RESET THE ORIGIN’S PORT TO 80 If you host
your website on any nonstandard port (say, 8080), you should note that
changing the document.domain property on your pages will reset the origin’s
port value to 80. This can cause AJAX requests to fail because browsers will

think that you’re trying to make a request from port 80 to the original, non-
standard port, thus failing an SOP check.

Now that you know how to make two web pages opt in to the same origin, let’s look at
how you can use this to send data to a different subdomain.

---

#### From [[1_document-domain]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1_document-domain]: 1_document-domain "document.domain"
[//end]: # "Autogenerated link references"
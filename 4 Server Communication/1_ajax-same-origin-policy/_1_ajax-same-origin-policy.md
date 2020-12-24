# AJAX Same Origin Policy

## **4.1 AJAX and the Browser Same-Origin Policy**

Suppose you’re making an ordinary web application that’s accessible at a single
domain. This application is deployed in a controlled environment: you know exactly
where your code will be executed, and all resources are served from the same domain.
You use the XMLHttpRequest object to asynchronously send requests to your servers.
This object enables you to send data back and forth without reloading the page, which
you use to update components of your web application seamlessly and provide a
smooth user experience.
This experience is exactly what you want to have in a third-party widget. You could
even argue that this experience is more important in a third-party world because
nobody wants to install a widget that steals users by redirecting them away from the
original host. So why can’t you use XMLHttpRequest again?
AJAX (Asynchronous JavaScript and XML) is a powerful set of methods. And
because it’s so powerful, it can also be dangerous when used by a malicious party in an

**Figure 4.1 A malicious website is prevented from accessing the contents of the GMail iframe thanks to the same-origin policy.**

uncontrolled manner. Without the necessary security measures, any random website
will have the power to perform potentially destructive actions on your behalf. For
example, if run without restrictions, a malicious website could load your favorite web
mail application as a hidden document, grab your unique session identifier, and then
use it to do horrible things with your data. What’s worse is that the application—the
one under attack—will have no idea that those actions weren’t explicitly triggered by
you. Your email account will never be the same (see figure 4.1).
Fortunately for us, browser vendors developed and implemented perhaps the most
important security concept in web clients: the same-origin policy (SOP). The SOP was first
introduced in Netscape Navigator 2.0 (the same version that first introduced
JavaScript), and at its core it ensures that documents are isolated from each other if

they’re served from different origins. In other words, the policy permits scripts on dif-
ferent documents to access each other’s DOM methods and properties with no specific

restrictions only if they were served from the same domain, port, and HTTP protocol. Scripts

that are trying to access methods and properties from a document on a different ori-
gin must, according to the policy, receive a slap on the wrist by way of a lovely Permis-
sion Denied exception.

Today, all browsers implement the same-origin policy in regards to XMLHttp-
Request, iframes, and other ways of exchanging messages between documents. It’s

thanks to the SOP that the following example will fail with a security exception in
every major browser:

var mail = document.createElement('iframe');
mail.src = 'http://gmail.com/';
mail.style.display = 'none';
document.body.appendChild(mail);
mail.contentWindow.GMAIL.deleteAllMyMailPlease();
In this section, we’ll look at rules for determining same origin, as well as the SOP and
script loading.

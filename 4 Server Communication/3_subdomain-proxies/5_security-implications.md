# Security Implications

## **4.3.5 Security Implications**

Internet Explorer bugs aside, subdomain proxies can be useful. If they don’t find a
home in your third-party scripts, you’ll probably use them in one form or another
elsewhere. But when employing subdomain proxies, you have to always remember the
security issues associated with them.
First, the document.domain property can be the source of security vulnerabilities.
When any two legitimate subdomains (say, status.example.com and auth.example
.com) opt in to the same domain namespace, any other resource served from that
domain may set their document.domain property to example.com and gain access to
properties and methods from legitimate subdomains. So if you have user pages hosted
on pages.example.com where it’s possible to run arbitrary JavaScript code, it’s not a
good idea to opt in any other subdomains into the top-level namespace because that
user code could access it.
Also, document.domain behavior is not very well specified. For example, there’s no
single rule on how document.domain should deal with top-level domains like .com or
.org, or what should be the behavior for sites that are accessed by their IP addresses. At
one point, browsers exposed a large security hole where they allowed locally saved
files (file://) to access all other files on the disk or on the web. Fortunately, that
behavior has been fixed, but it’s a good example of a security vulnerability that was
caused by a bad specification.
GOOGLE’S BROWSER SECURITY HANDBOOK If you’re interested in learning
more about the same-origin policy and security risks associated with it, you’ll
want to read part 2 of Google’s Browser Security Handbook (http://
code.google.com/p/browsersec/wiki/Part2). This document is a great
resource that, unfortunately, not a lot of people know about.
We just looked at one way to enable cross-domain requests and, although it works only
in a narrow use case—when all participating parties share the same higher-level
domain—from time to time you might find it useful. While using it, you should always
be aware of its security risks and be careful with your actual implementation.

---

#### From [[_3_subdomain-proxies]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_subdomain-proxies]: _3_subdomain-proxies "Subdomain Proxies"
[//end]: # "Autogenerated link references"

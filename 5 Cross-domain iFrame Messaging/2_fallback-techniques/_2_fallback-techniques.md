# Fallback Techniques

## **5.2 Fallback Techniques**

In the last section, you learned how to use the newly available window.postMessage
API to exchange messages between iframes. You also learned that although today’s
modern browsers support postMessage in their stable versions, older browsers—such
as Internet Explorer 6 and 7—don’t have that support. For these browsers, you’ll have
to employ a few hacks that allow you to use undocumented browser features to pass

small strings back and forth between an iframe and its parent window. All of the tech-
niques described in this section transmit messages much more slowly than window

.postMessage, and you should only use them as your backup plan.
In this section, we’ll go over three fallback techniques: using the window.name
property, using the URL fragment identifier (or hash), and using an Adobe Flash

object. All of these techniques base themselves on the fact that some browsers—inten-
tionally or unintentionally—skip same-origin policy checks when dealing with their

respective components.
As an aside, we’ll mostly be looking at simple proof-of-concept examples of these
transport mechanisms. We want you to understand how they work, not necessarily how
to implement them as 100% postMessage replacements. That’s because they can

become really complicated, and also later in this chapter we’ll look at easyXDM, a mes-
saging library that actually implements all of these protocols for you.

Let’s take a look at the first technique: using window.name to exchange messages
between windows.

## **Sub-chapters**

- [[1_window-name-messsages]]
- [[2_url-fragmentation]]
- [[3_messages-using-flash]]

---

#### From [[_cross-domain messaging]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1_window-name-messsages]: 1_window-name-messsages "window.name"
[2_url-fragmentation]: 2_url-fragmentation "url-fragmentation"
[3_messages-using-flash]: 3_messages-using-flash "messages-using-flash"
[_cross-domain messaging]: ../_cross-domain messaging "5️⃣ Cross-Domain Messaging"
[//end]: # "Autogenerated link references"

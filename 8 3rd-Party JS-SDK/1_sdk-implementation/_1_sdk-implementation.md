# SDK Implementation

## **8.1 Implementing a bare-bones SDK**

- In this section, we’ll implement a bare-bones JavaScript SDK for the Camera Stork web platform. This will be a minimal library which we’ll use to demonstrate core SDK concepts. It will provide three distinct pieces of functionality:

- Initialization—Load required dependencies and ready the SDK for use by publishers.
- Widget rendering—Public functions that render widget instances on the publisher’s page.
- Event listeners—Functions that enable publishers to listen and respond to internal events occurring in your various application components.
- Now, every SDK is different, and what we cover here might not necessarily find a home in your real-world SDK implementation. For example, if you’re developing a third-party SDK that provides tools for advanced web analytics, you probably don’t care about rendering widgets.
- But this minimal implementation should give you a great foundation from which to start.

- Let’s roll up our sleeves and get started. We’ll begin with loading the SDK’s initial script file and introducing a function for initializing the SDK: Stork.init. Next, we’ll implement Stork.productWidget for rendering widget instances on the publisher’s page. Last, we’ll provide functions for enabling publishers to bind and respond to events that take place in your application: Stork.listen and Stork.unlisten.

## **Sub-chapters**

- [[1_init-sdk]]
- [[2_async-load]]
- [[3_expose-public-functions]]
- [[4_event-listeners]]

---

#### From [[_3rd-party-js-SDK]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1_init-sdk]: 1_init-sdk "Init SDK"
[2_async-load]: 2_async-load "Asynchrous Loading"
[3_expose-public-functions]: 3_expose-public-functions "Expose Public Functions"
[4_event-listeners]: 4_event-listeners "Event Listeners"
[_3rd-party-js-SDK]: ../_3rd-party-js-SDK "8️⃣ 3rd Party JS SDK"
[//end]: # "Autogenerated link references"
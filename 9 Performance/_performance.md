# 9️⃣ Performance

## This Chapter Covers

1. **Minimizing initial payload**
2. **Reducing network requests**
3. Minimizing **impact on the publisher’s page**
4. Controlling **expensive JavaScript calls**
5. Improving **perceived performance**

## **Intro**

At this point, you’ve learned almost everything you need to know about building a
third-party JavaScript application. You know how to distribute your code, render

safely on the publisher’s page, establish communication channels, and even pro-
vide a suite of client-side functionalities through a JavaScript SDK. You’re also famil-
iar with the common security threats you might encounter when your application is

exposed to the public. Now that we’ve covered the fundamentals, we’ll discuss how
to make your third-party application fast.
Performance is an extremely important topic. Companies with high traffic
applications save millions of dollars by tweaking and optimizing their performance.
Google, for example, found out that a one-half-second delay in returning a search
results page damaged user satisfaction, resulting in a 20% drop in traffic. And for a
company that generates 95% of its profits from advertising, a 20% drop in traffic
meant millions of dollars in lost revenue. Amazon did a similar experiment as well,
and found out that even very small delays—increments of 100 milliseconds—resulted
in a significant drop in revenue (see http://mng.bz/73U0).
These examples show that users want their websites to load faster and be more
responsive, and they’re ready to vote with their wallets. And although it’s true that
high-traffic giants—such as Google, Yahoo!, or Facebook—benefit from small tweaks
to reduce their bandwidth, the biggest reason why these experiments showed drops in
traffic was due to a reduced user experience. This is a crucial argument, especially for
third-party JavaScript applications. People don’t visit your application directly; they go
to a website that happens to have your application installed. And there they expect to
see all the parts of the website working neatly together. Any significant delay between
the moment that website loads and the moment your application takes effect on the
page will likely result in a reduced user experience.
How upset can users get? At Disqus, when our widget fails to load quickly enough

on publisher websites, visitors commonly voice their concerns to publishers, with com-
plaints like “Disqus seriously slows down the [whole] page.” If your application is slow,

users will be quick to blame all issues affecting the publisher’s website on you, whether
you’re genuinely at fault or not. But, luckily, it cuts both ways: if your application is
fast, people will interact more with it, generate more content, and add more value to
your service in publishers’ eyes.

Optimizing your application is more than just tuning your web and database serv-
ers to respond faster. In this chapter, we’ll cover techniques you—as front-end engi-
neers—can use to significantly improve the delivery, execution, and responsiveness of

your applications. We’ll start with optimizing your application’s payload, then cover

JavaScript runtime performance, and finally look at improving perceived perfor-
mance. By the time you’re done reading this chapter, you’ll know how to make your

application lightning fast and, perhaps more important, how not to make it slow. Full
speed ahead!

## **9.1 Optimizing Payload**

## **9.2 Optimizing Javascript**

## **9.3 Perceived Performance**

## **9.4 Summary**

- This chapter was all about making your application **load**, **run**, and **feel faster**.
- You learned that the best way to make your third-party JavaScript application faster is by:

  - **reducing the initial payload**
  - **deferring the expensive parts of your application** until they’re absolutely necessary
  - and **making sure you’re not bombarding your browser with DOM reflows and repaints**.

- And for parts that can’t be optimized any further,
  - you can **increase user satisfaction by improving the perceived performance of your widget**.
- No matter what kind of a third-party JavaScript application you’re working on,
  - you should always consider applying knowledge you acquired in this chapter.
- But don’t stop there—pick up books we recommended, go play with **jsPerf**,
  - and watch videos of talks about front-end performance.
- The more you learn about performance,
  - the less likely you are to waste your time on a **premature optimization** which, as we know, is the root of all evil.

#### Next Chapter

[[_debugging-testing]]

- Our last chapter, “Debugging and testing,” will show you what to do when things go wrong and how to make sure that you don’t repeat old bugs over and over again.
- We’ll talk about writing automated tests for your widget and debugging it both on your development environment and—more exciting—in production.
- Onward!

#### Related

- [[perceived-performance]]
- [[optimizing-payload]]
- [[optimizing-javascript]]

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[_debugging-testing]: ../10 Debugging and Testing/_debugging-testing "🔟 Debugging & Testing"
[//end]: # "Autogenerated link references"

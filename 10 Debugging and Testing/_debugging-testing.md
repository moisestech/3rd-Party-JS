# 🔟 Debugging & Testing

## This Chapter Covers

1. Debugging production code using **proxies and switches**
2. Stepping through code with the **JavaScript debugger**
3. **Testing with [QUnit](https://qunitjs.com/) and [Hiro](https://github.com/valueof/hiro)**

## **Intro**

After all the work of previous chapters, your application is out there in the wild.
And yet, despite our commitment to walking you though the pitfalls and perils you

might encounter during development, despite our belief that you’re now well pre-
pared to tackle these problems before you encounter them, you’re probably still

going to run into issues that we haven’t covered. And when you encounter one of
these issues, all you can do is to put on your Sherlock Holmes hat, detect the source
of the issue, and solve the problem. Afterward, when the problem is solved, you’ll
probably want to make sure that the bug won’t reappear again. These two steps—
how to quickly debug issues in your application and how to prevent them from
reoccurring—are what our final chapter is about.
Alas, like many things we’ve covered in this book, debugging third-party

JavaScript applications is hard. First, you have absolutely no control over the envi-
ronment in which your application runs. It could be placed inside a carefully

crafted website or—more likely—an ad hoc page full of poorly written code. Your
application could be placed inside of an iframe element without your knowledge,
or even inside of an HTML form. Your application could be destroyed at any time and
then loaded again. This makes debugging tough, because a bug in your application
could be caused by any number of extraneous factors that you don’t control.
Additionally, your application will likely need to rely on ugly browser hacks in
order to be compatible with older browsers. Remember the iframe trickery we
employed in chapter 5 to emulate window.postMessage in legacy browsers? These
hacks sometimes rely on byzantine solutions that can be extremely hard to debug. By
our own estimation, we’ve spent days debugging some of the cross-domain messaging
techniques covered in earlier chapters. Worse yet, any functionality that’s not covered
by a specification—and hacks fall under this category—relies on unspecified browser
behavior, which could be removed in any future browser or OS update.

Finally, you must remember that—at their core—third-party JavaScript applica-
tions are still ordinary web applications. So in addition to problems specific to third-
party scripts, you still have to deal with all the issues affecting normal web application

development: old browsers with poor debugging tools, plugins that change the
browser’s default behavior, and of course Heisenbugs that are only reproducible in
production and not when you’re pointing a debugger at them.
When you’ve identified and fixed a bug, you’ll probably want to write a test for it.

Testing third-party widgets isn’t a walk in the park, either. Besides having to test multi-
ple browsers and browser versions, you may have to test against a wide variety of differ-
ent web environments and contexts. For instance, often you’ll have to simulate a

rogue publisher’s environment in order to re-create the situation where a bug occurs.
You’ll then need to keep that environment in your test suite forever to make sure
you’re not introducing regressions going forward.

We’ll start this chapter off by preparing your production environment for the occa-
sional debugging session. Then we’ll use the JavaScript debugger to step through

code in order to isolate and squash a bug. After it’s squashed, you’ll learn how to pre-
vent bugs by writing good unit and integration tests. Now let’s get ourselves a bug and

see if we can find its source!

## **10.1 Debugging**

[[1_debugging]]

## **10.2 Testing**

[[2_testing]]

## **10.3 Summary**

- Testing and debugging are things that most developers do reluctantly.
- It makes sense to debug—after all, if you’re debugging, it means that there’s a bug, someone found
  it, and this person is probably not happy about the whole situation.
- But now that you know how to approach bugs in third-party widgets, you’re able to put all the pieces of
  information together and fix your next bug much quicker.
- In addition to that, you know how to **set up your testing and debugging environment in an unobtrusive but
  extremely helpful way**.

- Figure 10.7 Example **Hiro** output:
- green borders around each suite mean that both of them successfully passed.
- Once again, everything works as expected.

- And though debugging isn’t pleasant, writing tests is fun!
- In this chapter, we covered two testing frameworks: **[QUnit](https://qunitjs.com/) and [Hiro](https://github.com/valueof/hiro)**.
- Knowing how to use just one of them should be sufficient for covering your widget’s functionality with tests.
- So go write some tests!
- The earlier you start, the more confident you’ll be when implementing new features and changing code.
- And **the more confident you are, the faster you can iterate with your third-party JavaScript application**.
- What’s next? Profit!

#### Related

- [[3_console]]

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[2_testing]: 2_testing/2_testing "Testing"
[3_console]: 3_console/3_console "JS Console"
[//end]: # "Autogenerated link references"

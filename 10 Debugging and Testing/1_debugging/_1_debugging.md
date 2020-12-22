# Debugging

## Tools

- [Web.Dev](https://web.dev/)

## **10.1 Debugging**

Consider the following situation. You’ve released the Camera Stork third-party appli-
cation into the wild and one particular publisher has reported a problem: visitors

can’t submit reviews on the publisher’s website. The widget is configured and ren-
dered on the page normally, but when a user clicks the Submit button, nothing hap-
pens: no obvious errors, no visual feedback, and there’s no HTTP request to the server.

Fortunately, this bug is occurring in all browsers, so you can pick and choose the best
development tools to guide you through this case.

In general, every debugging session consists of reproducing the bug in a con-
trolled environment and then stepping through the code, trying to locate the source

of the problem. So the first thing you should always do is try to reproduce the
reported bug in your controlled development environment. One way to do this is to
save a local copy of the publisher’s page, including their assets and JavaScript files, to
your local filesystem. This way, you can replicate the publisher’s environment but
instead load the development version of your third-party application. Sometimes this

technique works great, but sometimes websites have so many different scripts, add-
ons, and widgets that downloading and configuring them all can be a waste of time.

Let’s say that the publisher that reported this bug happens to love third-party
applications, such that their website is littered with dozens of them: sharing buttons for
every major social network, analytics scripts, third-party ads, and so on. Unfortunately,
trying to replicate this Wild West environment on your computer could easily take

hours of your life. You’ve got a business to run! So for this case it’d be far more bene-
ficial to debug in production. You’ll have to step through the code, look for data struc-
tures that have incorrect values, maintain a list of possible scenarios that could’ve

caused the bug and—after eliminating all other factors—find the source that actually
causes this bug to happen.
JAVASCRIPT ERROR REPORTING Not all your bugs and application errors will

be reported by users. Often you won’t be so lucky! That’s why it’s worth invest-
ing in tools that detect and report JavaScript errors occurring in your applica-
tion while it’s in production. Errorception (http://errorception.com) and

ExceptionHub (http://exceptionhub.com) are some commercial offerings,
or you can look at open source projects like Sentry (https://github.com/
getsentry/sentry).

But before you get started, you should recognize that there are a number of difficul-
ties with debugging production code. For starters, if you followed our advice from chapter 9, all your scripts and stylesheets are likely combined into a single minified and obfuscated JavaScript file. Such minified files can be a nightmare to debug (see figure 10.1). Additionally, since you don’t control the publisher’s servers, you can’t set
any debugging breakpoints on their page to help get you started. You can’t even update your own code without affecting all other users of Camera Stork.

This means that you need to somehow load the development version of Camera Stork directly on a publisher’s website.

This development version can be hosted anywhere your browser has access to—even on your own computer. This way, you’ll have freedom to examine and modify your code while running it in the exact same environment that triggered the bug.

This might seem like a complicated setup, but fear not—there are tools that make it easier to inject your development code into a production environment.

You can use web proxies to reroute your browser to the development computer whenever it tries to contact your production servers. Or you can implement a system of feature switches and let your production servers do the routing work. We’ll show you how to implement both.

## Sub-Chapters

- [[1-1_serv-dev-to-prod]]
- [[1-2_step-through-code]]

## **Courses**

- [Mastering Chrome Developer Tools v2](https://frontendmasters.com/courses/chrome-dev-tools-v2/) by Jon Jupermanof Adobe, @Frontend Masters
  - Github Repo [jkup/mastering-chrome-devtools](https://github.com/jkup/mastering-chrome-devtools)

## **Chrome Developer Tools**

- [Mastering Chrome Developer Tools: Next Level Front-End Development Techniques](https://www.freecodecamp.org/news/mastering-chrome-developer-tools-next-level-front-end-development-techniques-3ac0b6fe8a3/)
- [Mastering The Developer Tools Console](https://blog.teamtreehouse.com/mastering-developer-tools-console) by Matt West, Treehouse
- [Mastering browser DevTools](https://leftlogic.com/training/debug/) by LeftLogic
- [Improve Your Debugging Skills with Chrome DevTools (Part 2)](<https://www.telerik.com/blogs/improve-your-debugging-skills-with-chrome-devtools-(part-2)>)
- [Debug JavaScript in Google Chrome’s Dev Tools in 7 easy steps](https://raygun.com/blog/debug-javascript-google-chrome/)

## **Sub-chapters**

---

#### From [[_debugging-testing]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1-1_serv-dev-to-prod]: 1-1_serv-dev-to-prod "settings.py"
[1-2_step-through-code]: 1-2_step-through-code "1-2_step-through-code"
[_debugging-testing]: ../_debugging-testing "🔟 Debugging & Testing"
[//end]: # "Autogenerated link references"

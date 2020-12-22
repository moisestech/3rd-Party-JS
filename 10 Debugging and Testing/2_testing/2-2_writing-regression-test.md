## **10.2.2 Writting regression tests using QUnit**

QUnit is a JavaScript testing framework that was created for the jQuery project and is
used to test the jQuery codebase and associated plugins. But don’t be mistaken:
QUnit is simple, flexible, and capable of testing any generic JavaScript code, including
third-party widgets like the Camera Stork application. To use QUnit, you need to

download a copy of the library from https://github.com/jquery/qunit and create an
HTML file that loads QUnit as well as your tests files.
WRITING A SIMPLE QUNIT TEST CASE
Your job is to write a regression test that makes sure Stork.JSON works both on clean
pages as well as on rogue pages with incompatible Object.toJSON implementations.
You can start by writing a simple QUnit test case for Stork.JSON, as shown here.

**Listing 10.2 Example of a test case using QUnit testing framework**

```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="qunit.css" />
    <script src="jquery.js"></script>
    <script src="qunit.js"></script>
    <script src="stork/lib.js"></script>
    <script>
      $(document).ready(function () {
        QUnit.module("JSON");
        QUnit.test("test JSON.parse and JSON.stringify", function () {
          expect(2);
          var obj = { chapter: "Testing", pages: [1, 2, 3] };
          equal(
            Stork.JSON.stringify(obj),
            '{"chapter":"Testing","pages":[1,2,3]}'
          );
          equal(Stork.JSON.parse(Stork.JSON.stringify(obj)), obj);
        });
      });
    </script>
  </head>
  <body>
    <h1 id="qunit-header">CameraStork Tests</h1>
    <h2 id="qunit-banner"></h2>
    <div id="qunit-testrunner-toolbar"></div>
    <h2 id="qunit-userAgent"></h2>
    <ol id="qunit-tests"></ol>
  </body>
</html>
```

Now, if you open this HTML file in your browser, you should see something similar to
figure 10.6. This means that QUnit ran the test and it passed!
This successful test means that you can be confident that Stork.JSON will work on
clean pages. But your goal is to run the test shown in listing 10.1 on broken pages as

well as on clean ones. This means that you need to load the same test, but in a differ-
ent environment—the one that introduces the incompatible implementation of the

Object.toJSON. But QUnit runs all its tests in the same window, so simply including a
script that implements the incompatible Object.toJSON method isn’t an option

**Figure 10.6 Example QUnit output. The green bar means that all tests were successful— meaning that everything works as you expect it to work. Isn’t that nice?**

because it’ll affect all other tests—not just the intended regression test. To solve this
problem, we recommend having an isolated environment just for this regression test.
CREATING AN ISOLATED ENVIRONMENT FOR QUNIT TESTS

The best way to isolate a test is to place objects that are being tested inside a newly cre-
ated src-less iframe. This iframe will have a completely clean environment that’s not

affected by the outside window. Inside this iframe, you can load any files and modify
any built-in objects you want without introducing any conflicts with existing tests. The
next listing shows how to run the test from listing 10.2 inside an isolated iframe.

**Listing 10.3 Example of an isolated test case using QUnit and iframes**

```html
<script>
$(document).ready(function () {
QUnit.module("JSON/broken toJSON");
QUnit.test("test JSON funcs w/ modified toJSON", function () {
expect(2);
var iframe = document.createElement('iframe');
iframe.style.position = 'absolute';
iframe.style.top = '-2000px';
document.body.appendChild(iframe);
var isolatedWin = iframe.contentWindow;
var isolatedDoc = isolatedWin.document;
isolatedDoc.write(
"<html>" +
"<body>" +
"<script>Object.prototype.toJSON = " +
"function () { return ''; }" +
"</script>" +

"</body>" +
"</html>"
);
isolatedDoc.close();
function assertJSON() {
var obj = {'chapter':'Testing',pages: [ 1, 2, 3 ] };
start();
equal(isolatedWin.JSON.stringify(obj),
'{"chapter":"Testing","pages":[1,2,3]}');
equal(isolatedWin.JSON.parse(
isolatedWin.JSON.stringify(obj)), obj);
}
var interval = setInterval(function () {
if (isolatedWin.Prototype !== undefined) {
assertJSON();
clearInterval(interval);
document.body.removeChild(iframe);
iframe = null;
}
}, 10);
});
});
</script>
```

This code works pretty well if you have a single regression test that necessitates an iso-
lated iframe environment. But if you need to re-create this environment for multiple

regression tests, you’ll find yourself redundantly writing a lot of code to create and open
iframes. To remedy this, you can move the iframe creation and destruction code into
two special QUnit methods called setup and teardown. These methods will be called
before and after each test, respectively, and are declared on a QUnit test module:

```javascript
module("JSON/broken toJSON", {
  setup: function () {
    /_ ... _/;
  },
  teardown: function () {
    /_ ... _/;
  },
});
```

QUnit is a terrific general-purpose JavaScript testing framework, but it was never writ-
ten with third-party applications in mind. This means that you have to manually cre-
ate, manage, and clean up iframe environments yourself. Fortunately there’s another

testing framework that provides better tooling around managing iframes.

## **Writing regression tests using Hiro**

Hiro (http://hirojs.com/)—pronounced Hee-ro—is a testing framework that runs
each test suite—a small collection of tests—in a separate iframe sandbox, preventing
global state leaks and conflicts. Hiro’s usage pattern is similar to testing frameworks
written in other languages like Python (PyUnit) and Java (JUnit), so it should feel

familiar. Hiro was developed by engineers at Disqus in order to write good regression
tests for our widgets.
Like QUnit, Hiro is a JavaScript library that’s easy to install. Just download the
library, create an HTML file, and you’re ready to go. In this section, we’ll rewrite the
JSON regression test from earlier to use Hiro.
WRITING A SIMPLE HIRO TEST CASE
Test suites in Hiro are created using the hiro.module method. The method accepts
two parameters—a string containing the name of the test suite, and a JavaScript object
containing its implementation:

```javascript
hiro.module("JSON Tests", {
setUp: function () { ... },
onTest: function () { ... },
testStringifyParse: function () {
/_ test implementation _/
}
});
```

Hiro recognizes test functions by their name—they must be prefixed with test. All
other methods are ignored unless they’re special hooks such as setUp or waitFor. The
setUp method is run once before every test suite, whereas the onTest method is run

before every test execution. These are just two example hooks; others are docu-
mented on the Hiro website.

What makes Hiro special is that you don’t need to manually manage iframes—
Hiro will make an iframe sandbox available for every test suite. All you need to do is
define the iframe’s contents using a test fixture. Test fixtures refer to any kind of initial
state or data that’s loaded before running a test. In Hiro, fixtures are HTML markup

that’s injected into the iframe used by your test suite. The next listing shows an exam-
ple of a test case that tests JSON functionality in an isolated environment using Hiro.

**Listing 10.4 Example of a test case using Hiro testing framework**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Camera Stork Tests</title>
    <link rel="stylesheet" href="bootstrap.css" />
    <link rel="stylesheet" href="webui.css" />
    <script src="underscore.js"></script>
    <script src="jquery.js"></script>
    <script src="hiro.js"></script>
    <script src="webui.js"></script>
  </head>
  <body onload="main()">
    <div class="container">...</div>
  </body>
</html>
```

**Listing 10.4 Example of a test case using Hiro testing framework**

Load Hiro and application files
Boilerplate Hiro code

```html
<script data-name="clean" type="hiro/fixture">
<html>
<head>
<script src="stork/lib.js"></script>
</head>
</html>
</script>
<script>
hiro.module("JSONTests", {
setUp: function () {
this.loadFixture({ name: 'clean' });
},
testStringifyParse: function (test) {
test.expect(2);
var obj = {'chapter':'Testing',pages: [ 1, 2, 3 ] };
test.assertEqual(
this.window.Stork.JSON.stringify(obj),
'{"chapter":"Testing","pages":[1,2,3]}');
test.assertEqual(this.window.Stork.JSON.parse(
this.window.JSON.stringify(obj)), obj);
}
});
</script>
</body>
</html>
```

TESTING UNDER MULTIPLE IFRAME ENVIRONMENTS

You just used Hiro to test JSON functionality in a perfectly functioning iframe sand-
box. But remember, you want to cover two contexts: when the host page overwrites

toJSON and when it doesn’t.
Hiro makes this easy using what are called mixins. Instead of copying and pasting
your test function into a different test suite, you can use mixins to run your original
test suite against a wholly different iframe environment—one that defines a broken
Object.prototype.toJSON implementation. To do this, you’ll first need a second
HTML fixture that defines this environment:

```html
<script data-name="overwritten" type="hiro/fixture">
<html>
<head>
<script src="stork/lib.js"></script>
<script>
Object.prototype.toJSON = function () {
return '';
};
var isReady = true;
</script>
</head>
</html>
</script>
```

**Figure 10.7 Example Hiro output: green borders around each suite mean that both of them successfully passed. Once again, everything works as expected.**

Now you can create a new test suite that loads this fixture, but also indicates that the
original JSONTests module should be mixed in using the mixin property:

```javascript
hiro.module("JSONTestsPrototype", {
  mixin: ["JSONTests"],
  setUp: function () {
    this.loadFixture({ name: "overwritten" });
  },
});
```

Bam! Now when you run the resulting HTML in a browser, you should see something
similar to figure 10.7.

Earlier, we mentioned that Hiro generates iframe environments per test suite, not for each individual test function. Although it might be ideal to have a separate iframe per test, we found that continually creating and destroying iframes can be expensive on the browser. We chose this compromise so that our tests finish in a reasonable amount of time. Nothing’s worse than a slow-running test suite!

```

```

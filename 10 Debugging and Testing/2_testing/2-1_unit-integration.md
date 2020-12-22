## **10.2.1 Unit, Integration, and regression tests**

In software programming, tests can be categorized under several different types. We’ll

introduce three such types: unit tests, which test isolated software components; inte-
gration tests, which test multiple software components as they fit together in the final

product; and regression tests, which test that bugs or issues don’t reoccur.
UNIT TESTS

A unit test tests an individual “unit” of source code, ignoring other software compo-
nents. A good unit test knows only about one function, its signature, and its expected

return values. It neither knows nor cares about the bigger picture of your application:
how those functions work together, how they pass messages between each other, and
so on.
As an example, let’s write a quick unit test for JSON.stringify. We’ll write this test

without the help of any unit testing frameworks—which we’ll cover shortly. This func-
tion checks that both JSON and JSON.stringify are defined and that JSON.stringify

can handle serialization of JavaScript objects, arrays, and string literals:
function testStringify() {
var expectedValue =
'{"title":"3rd-party JS","authors":["Anton","Ben"]}';
var actualValue = JSON.stringify({
title: "3rd-party JS",
authors: [ "Anton", "Ben" ]
});
if (actualValue !== expectedValue) {
throw new Error("Test Failed: " + expectedValue +
" != " + actualValue);
}
}
Not bad for such a simple function, aye? And if you want to go further, you can write
another test case for JSON.parse and even another that checks whether JSON.parse
can parse what JSON.stringify stringified! You should write unit tests whenever you
want to make sure that your function always adheres to some specified behavior—like
converting an object into a JSON string. Unit tests give you more confidence that your
code behaves as intended.
STATIC CODE ANALYSIS Another way to test your code is to run it through a
static code analysis tool. These tools scan your program’s source code and
report about commonly made mistakes and potential bugs. The potential

problem could be a syntax error, a bug due to implicit type conversion, a leak-
ing variable, or something else.

Static code analysis tools are extremely useful for day-to-day development.

We particularly recommend JSHint (http://jshint.com/), a community-
driven tool maintained by one of the authors of this book. It’s flexible so you

can adjust it to your particular coding style. This flexibility doesn’t prevent
JSHint from spotting many errors and potential problems in your JavaScript
code—before you deploy them live.

INTEGRATION TESTS
Unit tests are written from a programmer’s perspective: that individual functions and
objects behave correctly in isolation. But this is just one side of the picture. On the
other side, there are integration tests. These tests make sure that your program works as
expected from the user’s perspective. For example, for the Camera Stork application,
this would mean testing that users can sign in, log out, submit reviews, and so on. In

this section, we’ll write a simple integration test that tests the product review submis-
sion process. In this test, we’ll make sure that the program correctly retrieves input

values from the widget’s form elements and then sends those values through a cross-
domain channel to the server.

Because integration tests cover more territory than unit tests, they can often be very
slow. This is especially true when they require a running server application that
responds to API requests and/or form submissions. Unless you’re a patient person, you
don’t want to spend minutes of your time running the test before finding out that
you’ve made a minor mistake in your JavaScript code. To counter this, developers
often replace slow components of their application with empty stub functions and then
validate that those functions were called. Listing 10.1 demonstrates an integration test
that ensures that Stork.submitReview correctly gets its data from the form text area,
and then sends that data to the server through Stork.getChannel().submit. This

integration test stubs the getChannel function to return an object that updates a spe-
cial flag to indicate that the function was called:

**Listing 10.1 An integration test that stubs out a server request**

function testSubmitReview() {
Stork.jQuery('textarea#stork-review').
val("Love it. 5 stars!");
var submitCalled = false;
Stork.getChannel = function () {
return {
submit: function (data) {
submitCalled = true;
var msg = JSON.parse(data).message;
if (msg !== "Love it. 5 stars!") {
throw new Error("Message is incorrect.");
}
}
};
};
Stork.submitReview();
if (!submitCalled) {
throw new Error("Channel.submit was\
never called");
}
}

MOCKING JAVASCRIPT OBJECTS WITH SINON.JS In these code examples, we
manually created our own stub functions, but you don’t have to do the same
in your projects. Sinon.JS is a helpful JavaScript library that provides stubs,
mocks, spies, and other test objects that will greatly simplify your testing code.
Check it out at http://sinonjs.org/.
You might’ve noticed that in listing 10.1 and in other prior testing examples in this

chapter, we’ve been doing a lot of work manually. Our test functions don’t have asser-
tions—predicates indicating that something must be true—or even a test runner that

combines these tests together, runs them, and presents the results in a nice UI. We did
this to show you that automatic tests aren’t magic; they’re just programs that make
sure that other programs work as expected. We’ll look at some JavaScript testing
frameworks shortly, but first let’s talk about one last testing category: regression tests.
TAKING YOUR INTEGRATION TESTS TO A HIGHER LEVEL WITH SELENIUM So far,
we’ve explored writing integration tests that use strictly JavaScript code. But

what if you could write tests that open a web browser window, load your appli-
cation, and interact with it just as a real user would? This is possible using

what are known as browser automation frameworks. We recommend you investi-
gate using Selenium—perhaps the most popular and feature-complete

browser automation tool. Selenium supports most major browsers and allows
you to write tests in a number of popular programming languages including
Python, Ruby, PHP, Java, Perl, and Groovy. Learn more about Selenium at
http://seleniumhq.org/.
REGRESSION TESTS
In the previous section, we were dealing with a nasty bug when our application didn’t
work correctly on pages that overwrite Object.toJSON. Sure, you’ve fixed the bug, but
there’s still a remaining task: to make sure that this bug won’t get re-introduced in the

future with a new feature or other code modification. To prevent this from happen-
ing, you need to write a regression test. Regression tests are unit or integration tests that

create an environment that led to the bug and make sure that the bug doesn’t appear
again. A good regression test for our bug would be to load the Camera Stork widget
on a “broken” page and verify that Stork.JSON still correctly serializes and parses
JSON data.

Instead of writing an ad hoc regression test like we’ve done in the previous sec-
tions, we’ll use two helpful JavaScript testing frameworks: QUnit and Hiro. We’ll start

with perhaps the most popular testing framework today: QUnit.

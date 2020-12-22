# **10.1.2 Stepping through the code**

In the dark days of JavaScript programming, developers had only one way of debugging JavaScript code—through good ol’ alert messages.

Compared to debugging tools available for other major programming languages, JavaScript used to have the most terrible development platform—bar none. Fortunately, browser vendors realized that JavaScript developers needed better tools, and today all modern browsers come with amazingly powerful built-in inspectors, profilers, and debuggers.

In this section, we’ll concentrate on debuggers—programs that let you examine JavaScript code while it’s executing and use them to step through the live code.

**SETTING BREAKPOINTS**
Let’s go back to your Camera Stork bug: users can’t publish reviews on a particular publisher’s website and you don’t have any clues as to what could be causing this. Let’s presume that you’ve managed to replicate this behavior on the publisher’s website. You can now run unminified and unobfuscated code, so you’ll probably want to set a
breakpoint—a special statement telling the debugger where to pause its execution—
near code that you suspect is causing the bug. There’s no single rule that tells where
to place your breakpoints; you’ll have to make an educated guess. With the Camera
Stork bug we’re investigating, it makes sense to place a breakpoint at some point
between where the user review is submitted and where it’s sent to the server.
Let’s pretend that the following code receives user review data from the widget’s
HTML form and sends it through an easyXDM cross-domain channel that has already
been initialized:

```javascript
Stork.prototype.submitReview = function (data, onSuccess, onFailure) {
  if (!data.reviewText) {
    return;
  }
  var channel = this.getChannel();
  var message = JSON.stringify(data);
  channel.submit(message, onSuccess, onFailure);
};
```

If review was empty, return
without sending any data. A wrapper
around

easyXDM-
powered

cross-domain
channel.

Turn JavaScript
object into string.
Usually, easyXDM
handles these things
but we made it
explicit for the
example’s sake.

Because you have absolutely no idea what’s causing the failure, it’s a good strategy to
pause at the beginning of the function and see what happens. There are two ways to
place a breakpoint: using the debugger’s UI or placing the special debugger keyword
inside your JavaScript code. When using the latter, you need to insert it just before the
line on which you’d like to pause the execution:

```javascript
Stork.prototype.submitReview = function (data, onSuccess, onFailure) {
  debugger;
  if (!data.reviewText) {
    return;
  }
  var channel = this.getChannel();
  var message = JSON.stringify(data);
  channel.submit(message, onSuccess, onFailure);
};
```

Now you have to inspect whether your input data is correct—because if it isn’t, you’ll

have to go back and put a breakpoint earlier in the code. In this case, the data hap-
pens to be valid, so you’ll have to dig deeper into this function in order to find the

source of the bug. You’ll use the debugger to step through the code until the line
where the message variable is created.
HOW TO STEP THROUGH THE CODE

Every browser’s debugger has a slightly different—but similar—user interface for step-
ping through code. Figure 10.5 shows a screenshot of Chrome’s JavaScript debugger.

Safari’s JavaScript debugger looks and behaves nearly the same.

When you’re on the statement that stringifies the data object, you can step for-
ward once more to execute JSON.stringify and then use the console or debugger’s

Scope Variables pane to check the value of the message variable:

```javascript
> console.log(message);
> ""{\"reviewText\":\"This camera is great!\",\"user\":1,\"camera\":2}""
> There’s something odd about the output. JSON.stringify is supposed to turn a
> JavaScript object into a string, but the current value of the message contains escaped
> double quotes. This means that JSON.stringify must have done its job twice—it first
> turned your data object into a string, and then subsequently turned that string into
> another, escaped, string. Clearly not what you’d expect from the browser’s built-in
> JSON.stringify function.
```

THE CRAFT OF DEBUGGING When it comes to debugging JavaScript applica-
tions, there are no big theories or complicated concepts. All you can do is

make an educated guess, place a breakpoint, pause execution, and start step-
ping through the code—line by line. You continue this process until you

notice an oddity—such as a variable that holds an unexpected value or a func-
tion that doesn’t return what it’s supposed to return. Such oddities are your

clues. Use them to make theories about your bug and then test these theories

by moving a breakpoint to another location or modifying your code to high-
light the oddity. If your theory is correct, congratulations, you’ve solved yet

another mystery! If it’s not, repeat the process until you’re successful.
At this point, you’ve found the line in your code that produces an unexpected value so

there’s (probably) no need to continue stepping further. But before closing the debug-
ger, you might as well use this moment to test a few theories on why JSON.stringify

doesn’t work as expected. First, let’s check whether JSON.stringify and JSON.parse
have been modified:

```javascript
> JSON.stringify
> function stringify() { [native code] }
> JSON.parse
> function parse() { [native code] }
> JSON.stringify({})
> ""{}""
```

Figure 10.5 Breakpoint and paused execution in Google Chrome’s built-in debugger
When printing out—but not
calling—native methods, browsers
always report that they’re native in
one way or another.
Two pairs of quotes when stringifying an empty object
mean that it was converted into a JSON string twice:
first from {} to "{}" and then from "{}" to ""{}"".

ALWAYS CHECK FOR OVERWRITTEN NATIVES When debugging third-party

JavaScript applications, your first assumption should always be that the pub-
lisher overwrote a native JavaScript object. This theory is correct more often

than not; nothing protects from overwriting—or shadowing—native
JavaScript objects, and publishers often don’t realize what damage they can
cause when modifying these objects.
WHEN FACED WITH NATIVE FUNCTIONS IMPERVIOUS TO DEBUGGERS
In this case, the native JSON functions don’t appear to have been modified. Is it a
browser bug? Unfortunately, you’re now at the point where you can’t inspect these
functions any further because JavaScript debuggers can’t step into native code. When

faced with misbehaving built-in functions, a good rule of thumb is to find the func-
tion’s specification and learn how it’s supposed to work. Go over the specification step

by step while trying to find the spot where the implementation fails.
In this case, we’ll spare you two good hours of studying the JSON spec and reading
through the browser’s source code. JSON.stringify, before turning an object into a
JSON string, checks whether that object defines a special toJSON method. This

method, per specification, should return a simplified object that can be properly seri-
alized if the original object can’t be serialized. With this knowledge handy, let’s try to

check whether there’s a toJSON method on your data object;

> data.toJSON()
> "{"reviewText":"This camera is great!","user":1,"camera":2}"
> Whoa! Not only does the data.toJSON method exist, it actually does what JSON.stringify
> was supposed to do—it turns the data object into a JSON string! This explains the quotes
> duplication and why JSON.stringify was broken without being overwritten. And since

your code is working on every other publisher’s site just fine, it’s a reasonable assump-
tion that the publisher has a rogue script that’s causing this behavior.

PROTOTYPE.JS AND TOJSON ISSUES Does this example problem seem unlikely?
Well, it actually happened. At Disqus, we discovered that an older version of a
popular JavaScript library—Prototype.js—was overwriting Object.prototype
.toJSON with a custom JSON serializer. This serializer didn’t match the toJSON
specification and had edge cases that produced different results. This bug has
been subsequently fixed in later versions of Prototype.js, but there are still
websites out there that are serving the old version. Watch out!
Is there a way to fix this problem without telling your publisher to fix their code?

One solution is to write a JSON polyfill that omits the toJSON part of the specifica-
tion. Use this polyfill when it appears that the publisher’s page has made the JSON

```javascript
object unusable:
var corrupted = false;
var arr = [1, 2, 3];
Stork.JSON = {};

if (typeof arr.toJSON === "function") {
arr = arr.toJSON();
corrupted = !(arr && arr.length === 3 && arr[2] === 3);
}

if (!JSON || corrupted) {
Stork.JSON.stringify = function (obj) { /_ ... _/ };
Stork.JSON.parse = function (str) { /_ ... _/ };
} else {
Stork.JSON.stringify = JSON.stringify;
Stork.JSON.parse = JSON.parse;
}
```

Now that you’ve found and fixed the bug, you should write a regression test to make
sure that you won’t get bitten by this bug again. Any experienced software developer
will tell you that automatic testing is important in any software project, but it can get
tricky with third-party JavaScript applications. This next section shows you how to
write good unit tests for third-party applications.

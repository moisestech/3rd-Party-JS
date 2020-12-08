# Init Script

## 2.3 The initial script file

At this point, the publisher is using your script include snippet, which is requesting a
single script file from your servers and loading it onto their page. This file, widget.js, is
the main entry point for your third-party application. It’s responsible for the whole
show: loading any additional code or resources, fetching data about the product, and
ultimately rendering the widget on the page.
Here’s a high-level tour of widget.js.

NAMESPACES IN JAVASCRIPT You’ve probably seen namespace objects in

JavaScript before. For example, the jQuery library puts all of its public meth-
ods behind the jQuery object (jQuery also aliases this object to $ for conve-
nience). Namespaces are common for nearly all JavaScript libraries, and are

just good programming practice.
For the most part, widget.js is empty; it’s just some boilerplate code that we’ll fill in

shortly. But before we look at implementations to some of these stubbed-out func-
tions, we want to highlight a few things.

## 2.3.1 Aliasing Window and Undefined

The function expression from listing 2.2 has two parameters: window and undefined.
You might recognize these as global objects that should be accessible at every scope.
So why redeclare them as function arguments?
For starters, window and undefined are two objects that you’ll likely use repeatedly
in your code. When they’re declared as local variables like this, a JavaScript minifier

can shorten their variable names to help reduce the script’s file size. But if they’re ref-
erenced as global variables, those references can’t be renamed and shortened.

Listing 2.2 widget.js: main script body

The Stork global variable is a
namespace object that
encapsulates your application. The
purpose of the namespace object is
to house any public functions your
application will expose. Instead of
declaring those functions globally,
they’re assigned as properties of
the Stork namespace, to prevent
them from leaking into the global
variable scope and possibly
conflicting with other code.

These empty
function
stubs
encapsulate
parts of the
application
we’ll cover
later.

Main application flow—initialize,
load, and render widget.
IIFE prevents local variables and functions from
leaking into the global scope. If you couldn’t tell
by now, we take great precautions to avoid global
variables!

Download from Wow! eBook <www.wowebook.com>

32 CHAPTER 2 Distributing and loading your application
JAVASCRIPT MINIFIERS JavaScript minifiers are utilities that take JavaScript
source code and rewrite it to be as short as possible, in order to reduce file
size. This is usually accomplished by removing whitespace, renaming variables

to be as small as possible, or using alternate, terse forms of common opera-
tions. The output of code minifiers is usually difficult, if not impossible, to

read; they’re usually reserved for code that’s deployed in a production envi-
ronment. You’ll learn more about minifiers in chapter 9 on performance.

The second reason for redeclaring these variables centers mostly around the
undefined object. You’ll notice that undefined isn’t explicitly passed to the function.
Because no value is passed, the undefined function argument is given the default
value of undefined by the browser. This would be true of any function argument that
isn’t passed an explicit value:
function foo(a, b) {
console.log(a); // => 1
console.log(b); // => undefined
}
foo(1);

There’s a benefit to aliasing undefined like this. If the original object has been modi-
fied by code elsewhere in the current execution environment, it won’t affect your

code, because you’re using a local alias that has the original, untouched value. You’ll
find this is a common technique used by JavaScript library authors.

## 2.3.2 Basic Application Flow

Moving on, let’s look at the function calls that are taking place in the main application

body from listing 2.2. These are high-level functions that implement the basic applica-
tion flow described in figure 2.4. Not all third-party applications will need to follow

these exact steps, but it’s a good starting place.

**Figure 2.4 High-level application fow for widget.js**

The first function, loadSupportingFiles, is intended to load any supporting files

needed by the application before continuing. It’s not always necessary to load addi-
tional files, but many applications can benefit by splitting up their JavaScript distribut-
ables into separate downloads, so we’ve included it as a step. This function accepts a

single parameter: a callback function that’s intended to execute after loadSupport-
Files is finished:

loadSupportingFiles(function() {
var params = getWidgetParams();
Load supporting
files

Fetch product

data Identify product Render product

**Figure 2.4 High-level application flow for widget.js**

getRatingData(params, function() {
drawWidget();
});
});
In the callback function, any parameters passed to the script are extracted using
getWidgetParams. For now, the Camera Stork example will make use of a single
parameter: the ID of the product the publisher is trying to display on their website.
But there are plenty of other parameters you could pass. For example, you could
embed identifying information about the publisher in the script include snippet, and
pass that information to your servers. This could be useful if you want to track which

publishers are sending you traffic—the Camera Stork business could even pay publish-
ers affiliate revenue for each sale they help generate.

After the script has extracted those parameters, it’s time to get data about the
product from the server using the getRatingData function. This data will be the
name of the product, perhaps a URL where the product can be purchased, and the
current rating or score. As a final step, after the data has been received from the
server, the application uses this data to render the actual widget on the page. This
takes place inside the drawWidget function.

It’s worth noting that none of these functions exist yet. We’ve strictly taken a high-
level overview of the widget.js code. You’ll be implementing these functions over the

next few sections. Let’s start with the first: loadSupportingFiles.

# Global Variables

## 2.5.4 Using global variables

In the past few sections, we’ve explored using the DOM as a means of passing parame-
ters to your third-party application. An alternate approach is to avoid the DOM

entirely, and instead use global variables. Any variables declared in the browser’s top-
most variable scope are attached to the window object, and are accessible by any script

running on the page.
The next listing changes the script include snippet from section 2.1 to declare a
global variable whose purpose is to pass the product ID to your third-party script.

**Listing 2.10 Passing script arguments using global variables**

In your third-party script file (widget.js), you can just read the contents of the
product_id variable, and you’re on your way to loading that product’s current vote
rating and rendering it into the page.
There’s a catch: global variables like this are shared by all scripts executing on the
page, and can be read or altered by any of them. Similarly, by setting this value, you
might be overwriting a variable that’s relied on by another script loaded elsewhere on
the publisher’s page.
A better idea is to put the global variable inside your application namespace. That
will reduce variable conflicts with other scripts, because the likelihood of another
script sharing your branded namespace is small. You’ll have to initialize the
namespace first, because the widget.js file where the namespace was previously
declared won’t have been loaded yet:

Namespacing the script parameter like this will reduce conflicts, but there’s still a sig-
nificant downside: you can only define one product_id parameter per page. If the

publisher tries to include the script include snippet a second time for a different prod-
uct, the second product_id assignment might overwrite the first value by the time the

first script file is loaded.

ASYNCHRONOUS SCRIPT INCLUDES DON’T PRESERVE EXECUTION ORDER Remem-
ber, we’re loading the third-party script asynchronously to improve page per-
formance. That means that there’s no predicting when the script will finish

downloading and run. It also means that the browser doesn’t maintain execu-
tion order of asynchronous scripts, which can lead to race conditions if you’re

not careful.
GLOBAL VARIABLE ARRAYS
To avoid global variable collisions, you can instead pass parameter variables in global
arrays. If a second script include detects earlier parameter declarations, it merely
appends to the array. When the script loads, it can iterate over the array of parameters
and execute the rendering method for each passed ID:

Now you have your bases covered. The downside is that the script include snippet has
grown by three lines of code—not a big deal. The upside is that the script include
snippet can be included many times on the page, and you’ll have an array of product
IDs to work with.
Is there a benefit to using global variables to pass parameters over embedding
them in the DOM, either via the query string or fragment identifier, or using data-\*

attributes? Sure. For starters, you can embed any type of JavaScript object as a parame-
ter, like a dictionary object. And embedding parameters as global variables is, debat-
ably, more clear to publishers than appending them to the end of a long URL. Last,

unlike the other techniques we covered, you don’t need to query the DOM in order to
extract the parameters—they’re already initialized as JavaScript variables for you.
When performance matters above all else, global variables may be the best choice.
Table 2.1 summarizes the pros and cons of the various techniques.

---

#### From [[_5_passing-args]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_5_passing-args]: _5_passing-args "Passing Args"
[//end]: # "Autogenerated link references"

# Version Init

## **8.2.2 Versioned initialization**

Using separate files to serve different versions of your SDK is a tried-and-true tech-
nique, but we’ll explore an alternate approach: versioning via your SDK’s initialization

function. Versioning at the initialization level offers some subtle advantages over URL
versioning—but before we get there, let’s first see how it works.
Remember: before a publisher can use your SDK, they have to call Stork.init,
which loads most of the application code, and then executes a provided callback when
that code is ready. This initialization function is an excellent place for publishers to
declare a version string:

<script src="http://camerastork.com/sdk.js"></script>
<script>
Stork.init('1.1', function(S) {
S.productWidget({
id: '1337',
dom: '.stork-widget-location-class'
});
});
</script>

In this example, the first argument to Stork.init is the desired version string, and
the callback function becomes the second argument. You’ll notice that the callback
function now accepts a single argument, S, which is actually a reference to the Stork
SDK oriented at the requested version. Inside the callback function, the publisher calls
the newer version of Stork.productWidget, which accepts a CSS selector for the DOM

target location. You’ll also notice that the script URL doesn’t have a version embed-
ded; the versioning is handled only via Stork.init.

Let’s implement this new versioning behavior for Stork.init. Here’s a modified ver-
sion of sdk.js, whose public init function now accepts an optional version parameter.

**Listing 8.6 Modifying Stork.init to accept a version parameter**

Stork.instances = {};
Stork.callbackQueue = {};
Stork.init = function(version, callback) {
if (typeof version === 'function') {
callback = version;
version = '1.1';
}
if (Stork.instances[version] !== undefined) {
callback(Stork.instances[version]);
return;
} else if (Stork.callbackQueue[version] !== undefined) {
Stork.callbackQueue.push(callback);
return;
}
var file;
switch (version) {
case '1.0': file = 'lib.1_0.js'; break;
case '1.1': file = 'lib.1_1.js'; break;
default:
throw "Unknown SDK version: " + version;
}
Stork.callbackQueue[version] = [callback];
file = 'http://camerastork.com/sdk/' + file;
loadScript(file, function() {
for (var i = 0; i < callbackQueue; i++) {
Stork.callbackQueue[i](Stork.instances[version]);
}
});
};
The first thing this function does is check to see whether the first argument is a
function. If it’s a function, then the latest version of the SDK is used by default (1.1).

This means that if publishers decline to specify a version string, they’ll always get the
latest version.
Next up, init checks whether the requested version has already been loaded, or is

in the process of being loaded. If it’s already been loaded, then the callback is imme-
diately called and the library instance is passed as an argument. If it’s in the process of

being loaded, but not finished, then the callback function is appended to a queue.
VERSIONING ISN’T FREE Versioning is great for publishers, because it provides

assurance that their code will continue working even if you introduce break-
ing API changes. But providing this feature requires that you actively maintain

and serve previous versions of your code, which can be a maintenance bur-
den. For this reason, some popular SDKs eschew versioning altogether. For

example, the Facebook JS SDK—a large, constantly changing library—inten-
tionally doesn’t provide versioning. Be aware of the trade-offs of versioning,

and decide whether it’s a good choice for your library.
If the requested version hasn’t been loaded and isn’t in the process of being loaded,
then init loads the corresponding library file using the standard loadScript helper
function. Just as before, this library file is considered the “meat” of your JavaScript
SDK and contains all your public functions. After the file has been downloaded, init
iterates through the queued callback functions, invoking each of them with the library
instance as the first (and only) argument.
You’re probably wondering, how does the versioned library get assigned to
Stork.instances? To explain that, let’s look at lib.1_1.js, the library file associated
with version 1.1 of the SDK:

(function(window, undefined) {
var exports = {};
exports.productWidget = function (options) { ... };
exports.listen = function (eventName, handler) { ... };
exports.unlisten = function (eventName, handler) { ... };
window.Stork.instances['1.1'] = exports;
})(this);

The first thing you’ll notice that the public functions—productWidget, listen, and
unlisten—are no longer directly assigned to the Stork object. Instead, they’re
assigned to a local variable, which is then assigned to Stork.instances. When
Stork.init detects that the library file is ready, it retrieves this instance and passes it
to the publisher’s callback function.
Congratulations—you’ve successfully implemented versioning at the initialization
level. But why do this over standard URL versioning?

The main reason is because this technique is (arguably) less error-prone for pub-
lishers. Forcing publishers to use the Stork object through the callback function

means they’re less likely to use a Stork object that belongs to a different version (or
another code snippet). This is different from URL versioning, where the publisher has
to intentionally use Stork.noConflict to avoid such errors.

Versioning during initialization isn’t without flaws. One such flaw is that it doesn’t
let you version your initialization function (Stork.init)—you’ll have to be careful if/
when you modify this function, because you can accidentally introduce breaking
changes. Additionally, this technique may be perceived as being more complicated
than URL versioning, which most publishers are already familiar with. As such, we
don’t necessarily endorse one technique over the other—it’s a matter of preference.

---

#### From [[_2_versioning]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_versioning]: _2_versioning "Versioning"
[//end]: # "Autogenerated link references"

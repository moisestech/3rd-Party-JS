# Init SDK

## ** 8.1.1 Initialization**

As with all third-party scripts, in order to gain access to your SDK’s public functions, publishers will have to load an initial script file. You’ll give this a new filename, sdk.js, to differentiate it from the single-purpose self-executing script (widget.js) you implemented in some of the early chapters of this book. Let’s take a look at sdk.js.

**Listing 8.1 The initial JavaScript file: sdk.js**

```javascript
(function (window, undefined) {
  var Stork = {};
  if (window.Stork) {
    return;
  }
  function loadScript(callback) {}
  Stork.init = function (callback) {
    loadScript("http://camerastork.com/sdk/lib.js", callback);
  };
  window.Stork = Stork;
})(this);
```

Like your initial widget architecture from chapter 2, this creates a global Stork object on the publisher’s window. This time, there’s a single public function: Stork.init. This function is responsible for initializing the SDK and must be called by the publisher before any other Stork functions. It takes a single parameter—a callback function—that’s invoked when initialization is complete and the SDK is ready.

Right now, Stork.init has a light implementation. It loads a single file—sdk/lib.js—which will house the SDK’s remaining public functions. You’ll implement these functions as we progress through this chapter. You might be wondering, why have a public initialization method? Why not load all of your code in a single file? We think separating your initial script file and your main application code is good practice for the following reasons:

- Configuration—The initialization function can optionally take global configuration arguments, like an SDK version string or a publisher’s API key.

- Performance—You can defer loading JavaScript files and other assets until the
  publisher actually invokes your SDK.

- File size—By keeping the initial script file small, you minimize the impact of
  blocking script includes.

- This last part is important. Let’s say you were to distribute your SDK using a standard, blocking script include. That means publishers might write the following example code to use your SDK:

```html
<script src="http://camerastork.com/sdk.js"></script>
<script>
  Stork.init(function () {
    Stork.productWidget({ id: "1337", dom: "widget-location-id" });
  });
</script>
```

By wrapping the guts of your application in a separate file (lib.js), you’re keeping the initial script file (sdk.js) extremely small. If this file remains small, it’ll load faster, which minimizes the amount of time it’ll spend blocking the page.

Now, you’re probably thinking, aren’t blocking script includes El Diablo? Shouldn’t we insist that publishers load sdk.js asynchronously? The answers are yes and yes. But there’s a good reason for allowing publishers to load your script in the blocking fashion: it’s the simplest, most recognizable way of loading JavaScript, and it’s easier to understand by junior and novice JavaScript programmers. And inevitably, somebody will load your application using a blocking script include, in which case, you may as well minimize the impact it’ll have on their site.

**Listing 8.3 Defining a global callback function that executes when the SDK is ready**

```html
<script>
  (function () {
    var script = document.createElement("script");
    script.async = true;
    script.src = "http://camerastork.com/sdk.js";
    var entry = document.getElementsByTagName("script")[0];
    entry.parentNode.insertBefore(script, entry);
  })();
  window.Stork_ready = function () {
    Stork.init(function () {
      Stork.productWidget({
        id: "1337",
        dom: "stork-widget-location",
      });
    });
  };
</script>
```

The publisher has played their part, but in order for their code to work, you’ll need to modify your SDK to check whether the Stork_ready function exists, and if it does, execute it. That’s handled by the following code, which should go at the end of sdk.js:

```javascript
if (typeof window.Stork_ready === "function") {
  window.Stork_ready();
}
```

Using a global variable to store the publisher’s callback function is a huge step up from the first solution we covered. This snippet is close to half as many lines of code.

But it does carry a small—but addressable—flaw: there can only be one Stork_ready function at any given time. If a separate block of code overwrites a previously defined Stork_ready function, the older callback function will never be executed. That might seem an unlikely occurrence, but if your SDK becomes popular, you might find that it’s used by other third-party scripts operating on the publisher’s page. Ideally, you should support multiple publisher-defined ready callbacks.

Not to worry—this is quickly solved by having publishers treat Stork_ready, not as a single global function, but as a global array of functions. To register a new callback, they append to this array, which can maintain callback order in case there are subsequent declarations:

```javascript
Stork_ready = Stork_ready || [];
Stork_ready.push(function () {
  Stork.init(function () {
    Stork.productWidget({
      id: "1337",
      dom: "stork-widget-location",
    });
  });
});
```

Is this code starting to look familiar? It’s similar to some sample code we showed you at the end of chapter 2, which involved passing publisher configuration arguments to your third-party script. If you think about it, the callback function could be considered a configuration argument for your SDK. That being the case, any number of techniques from that section could work here.

For example, you could have publishers pass the name of the callback function to your SDK as part of the query string—similar to a JSONP request. As long as publishers declare uniquely named callback functions, they won’t conflict. This happens to be the approach that Google’s Maps JavaScript API uses: `script.src = "http://maps.googleapis.com/maps/api/js?callback=my_func";`

As with many solutions we’ve covered during the course of this book, there’s no single correct choice for how publishers define an asynchronous callback function. Choose the method that appeals most to you. For now, we’ll move on from loading and initial- ization, and jump into the heart of this chapter: implementing public functions on your SDK.

---

#### From [[_1_sdk-implementation]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_sdk-implementation]: _1_sdk-implementation "SDK Implementation"
[//end]: # "Autogenerated link references"

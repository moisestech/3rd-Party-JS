# Basic App Flow

## 2.3.2 Basic Application Flow

- Moving on, let’s look at the function calls that are taking place in the main application body from `listing 2.2`.
- These are high-level functions that implement the basic application flow described in `figure 2.4`.
- Not all third-party applications will need to follow these exact steps, but it’s a good starting place.

**Figure 2.4 High-level application fow for widget.js**

- The first function, loadSupportingFiles, is intended to load any supporting files needed by the application before continuing.
- It’s not always necessary to load additional files, but many applications can benefit by splitting up their JavaScript distributables into separate downloads, so we’ve included it as a step.
- This function accepts a single parameter: a callback function that’s intended to execute after loadSupport-
  Files is finished:

```javascript
loadSupportingFiles(function() {
var params = getWidgetParams();
```

1. Load supporting files
2. Fetch product data
3. Identify product Render product

**Figure 2.4 High-level application flow for widget.js**

```javascript
getRatingData(params, function() {
drawWidget();
});
});
```

- In the callback function, any parameters passed to the script are extracted using
  getWidgetParams.
- For now, the Camera Stork example will make use of a single parameter:
  - the ID of the product the publisher is trying to display on their website.
- But there are plenty of other parameters you could pass.
- For example, you could embed identifying information about the publisher in the script include snippet, and
  pass that information to your servers.
- This could be useful if you want to track which publishers are sending you traffic—the Camera Stork business could even pay publishers affiliate revenue for each sale they help generate.

- After the script has extracted those parameters, it’s time to get data about the product from the server using the getRatingData function.
- This data will be the name of the product, perhaps a URL where the product can be purchased, and the current rating or score.
- As a final step, after the data has been received from the server, the application uses this data to render the actual widget on the page.
- This takes place inside the drawWidget function.

- It’s worth noting that none of these functions exist yet.
- We’ve strictly taken a high-level overview of the widget.js code.
- You’ll be implementing these functions over the next few sections.
- Let’s start with the first: `loadSupportingFiles`.

---

#### From [[_3_init-script]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_init-script]: _3_init-script "Init Script"
[//end]: # "Autogenerated link references"

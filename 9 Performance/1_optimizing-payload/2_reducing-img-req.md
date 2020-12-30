# Reduce Img Req

## **9.1.2 Reducing Image Request**

Images—like scripts, stylesheets, and other web page components—require the

browser to make a separate HTTP request for every single file. Luckily, there are tech-
niques you can use to reduce the number of image requests made by your application.

These techniques aren’t as simple as concatenating source files, but they’re not rocket
science either.
IMAGE SPRITES
The most common way of combining multiple images is using image sprites. With
image sprites, you combine multiple images into a single, larger image file, and then

display only the part you need using the background-image and background-posi-
tion CSS properties.

Figure 9.1 shows an image sprite we used for the Disqus commenting widget,
which contains a number of different icons and the Disqus logo.

To display a specific part of the sprite, we specify a CSS class that sets the back-
ground-image property to point to the

sprite file, and the background-position

to the pixel location of the desired subim-
age. For example, to display the grey

thumbs-up button, the background image
needs to be positioned 15 pixels from the
sprite’s top and left corners:
.thumbs-up {
background-image: url(http://cdn.disqus.com/img/sprite.png);
background-position: 15px 15px;
}

Originally, these icons were all individual image files, each requiring a separate HTTP request. By bundling them into a sprite, only a single HTTP request is made. Even if a sprite file is referenced multiple times in your CSS, it’s only downloaded once by the browser. For applications with a significant number of image assets, this can significantly reduce the number of HTTP requests issued. Sprites are a no-brainer—use them wherever you can.

CONTENT DELIVERY NETWORKS Ideally, you should be serving your static files
from a content delivery network (CDN). CDNs provide high-performance
asset delivery, usually from multiple data centers around the world, and are
the fastest way to deliver files to your users. Akamai, Amazon CloudFront, and
CloudFlare are just a handful of notable CDN providers.

**INLINE IMAGES**
Another neat way to reduce the number of HTTP requests is using inline images. With
this technique, you actually embed an entire image file as a binary string into your
application’s source code, forgoing the need for a separate network request.

To see how this works, let’s look at a real-world example involving the Disqus com-
menting widget. This widget begins with a loader script: a piece of JavaScript that

always loads first, configures itself, and then loads the remaining application files. At
one point, we decided that it’d be nice for the loader script to display our company’s
logo while it’s chugging along, but we didn’t want to introduce an additional network
request just to display the tiny Disqus logo (< 1 KB).

The solution to this problem came in the shape of data URIs and Base64-encoded
images. The data URI scheme allows you to include otherwise external data inline,
without making an additional network request. This is great, except you can’t just
copy and paste an image into your source code—you have to first convert the binary
image file into an ASCII-compatible format. And this is where Base64—an encoding
scheme that allows you to represent binary data as an ASCII string—comes in handy.
When used together, data URIs and Base64-encoded images provide a great tool to
inline images instead of making additional network requests. And conveniently for all
web developers, there are plenty of web-based utilities that make it easy to convert
your images into their Base64 counterparts.
Listing 9.1 shows a snippet of JavaScript code that displays a (small) Disqus logo.
To do this, it creates an img DOM element, and then assigns a Base64-encoded string
containing the image data to the element’s src attribute using the data URI scheme.

**Listing 9.1 Embedding an image into JavaScript source code**

var logo = "iVBORw0KGgoAAAANSUhEUgAAAEcAA...";
var container = document.getElementById('dsq-logo');
container.innerHTML = '<img width="71" height="17" ' +
'src="data:image/png;base64,' + logo + '"/>';

Note that, unfortunately, this technique doesn’t work in Internet Explorer 7 and ear-
lier. For these browsers, you’ll have to degrade to loading images the old-fashioned

way—by requesting them over the network.

---

#### From [[_1_optimizing-payload]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_optimizing-payload]: _1_optimizing-payload "Optimizing Payload"
[//end]: # "Autogenerated link references"
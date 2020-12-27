# Access Server APIs

## **8.3.1 Accessing web service APIs on the client**

In case you’re not familiar with them, a web service API is a set of HTTP endpoints that
enable programmatic access to a web application. In order to query the API for data, a
publisher issues an HTTP request to one of your endpoints, and the server responds
with formatted data in the HTTP response body, usually in JSON or XML formats.
Here’s a quick example of how you can use curl, a Unix utility for issuing HTTP
requests,2
to query Twitter for a list of the 20 newest public status updates (tweets)
made by ManningBooks, Manning Publication’s official Twitter account:
curl http://api.twitter.com/1/statuses/user_timeline.json?\
screen_name=ManningBooks
Twitter3

responds to this request with structured JSON data containing Manning-
Books’ user timeline. To keep things legible, we’ve shortened this to a status update.

**Listing 8.7 JSON containing ManningBooks’ user status updates from Twitter**

[
{
coordinates: null,
contributors: null,
retweet_count: 0,
in_reply_to_user_it: null,
favorited: false,
geo: null,
possibly_sensitive: false,
in_reply_to_screen_name: null,
user: { ... },
id_str: "163357792512643073",
place: null,
retweeted: false,
in_reply_to_status_id: null,
created_at: "Sat Jan 28 20:28:27 +0000 2012",
in_reply_to_status_id_str: null,
truncated: false,
source: "<a href=\"http://www.socialoomph.com\">SocialOomph</a>",
in_reply_to_user_id_str: null,
id: 163357792512643070,
text: "Deal of the Day! Get half off the Sass and Compass in \
Action (http://t.co/gdVIl8gd ) eBook! Use code dotd0128tw \
at checkout."
},
...
]

Like this example using curl, most web service APIs are designed to be accessed server-
side, from a programming language running on your private servers. Typically, this is

so that the API can identify the requesting party, either using the IP address from
which the request originates, or using a private and uniquely identifying API token
that’s submitted with each request. This helps web service APIs rate-limit or even shut
down publishers who are abusing the API by making too many requests.

**JSONP AND CORS**
Increasingly, web service APIs are seeing the value in making requests directly from the browser. Many APIs already have support for JSONP, which (as we learned in chapter 4) enables a web page to request JSON data from a different domain.

Twitter is an example of a web service API that supports JSONP requests. In the last
section, we demonstrated a server-side request to the Twitter API using curl, but the
same request can be made on the browser using JSONP. The following code snippet
issues the same request for ManningBooks’ user timeline, using jQuery’s AJAX helper:

jQuery.ajax({
url: 'http://api.twitter.com/1/statuses/user_timeline?\
screen_name=ManningBooks',
type: 'jsonp',
callback: function(response) {
// success
}
});

**Listing 8.7 JSON containing ManningBooks’ user status updates from Twitter**

JSONP is great because it works in all browsers. Its downsides, if you recall, are that it

only works with GET requests, there’s a limit on the amount of data that can be sub-
mitted, it can’t detect 400 or 500 HTTP response errors, and caching browser-side is

difficult. That’s a laundry-list of failings, but luckily we’re beginning to see some web
service APIs supporting CORS, which permits the use of XmlHttpRequest across
domains. This is preferable; XmlHttpRequest doesn’t have the same failings as JSONP.
GitHub, the popular source-code hosting service, is a terrific example of an API that
supports CORS. Provided you’ve registered your website’s domain with their API, you

can use XmlHttpRequest (XDomainRequest for Internet Explorer 8) to issue a cross-
domain request from that domain. For example, we’ve registered thirdpartyjs.com

(this book’s website) with GitHub’s API, which permits us to issue a CORS request from
our website to their API. The following code snippet uses jQuery’s ajax helper to query
information about the thirdpartyjs-code repository, where this book’s companion
source code examples are hosted:
$.ajax({
url: 'https://api.github.com/repos/thirdpartyjs/thirdpartyjs-code',
success: function(response) {
console.log(response);
}
});
If this code sample looks surprisingly simple—it is. jQuery makes using CORS a breeze.
If you’re curious, this is the JSON response returned by the server, abbreviated slightly.

**Listing 8.8 JSON representing the thirdpartyjs-code repo, from GitHub’s API**

{
updated_at: "2012-01-24T08:35:04Z",
has_downloads: true,
ssh_url: "git@github.com:thirdpartyjs/thirdpartyjs-code.git",
language: "JavaScript",
created_at: "2011-09-05T03:19:37Z",
fork: false,
description: "Companion source code to \
Third-party JavaScript (the book)",
watchers: 39,
pushed_at: "2012-01-24T08:35:04Z",
private: false,
html_url: "https://github.com/thirdpartyjs/thirdpartyjs-code",
git_url: "git://github.com/thirdpartyjs/thirdpartyjs-code.git",
owner: { ... }
name: "thirdpartyjs-code",
clone_url: "https://github.com/thirdpartyjs/thirdpartyjs-code.git",
id: 2326050,
url: "https://api.github.com/repos/thirdpartyjs/thirdpartyjs-code",
forks: 4,
homepage: "http://thirdpartyjs.com"
}

That’s a lot of rich, valuable data about the thirdpartyjs-code repository. Developers
are accessing this and other endpoints via CORS to build client-side applications that

interface directly with GitHub. For example, Dabblet (http://dabblet.com) is a web-
based code editor that saves files as new repositories in your GitHub account. It does

this all using strictly client-side JavaScript and GitHub’s API—check it out.
WHY A CLIENT LIBRARY?
You might be asking yourself, if CORS and JSONP already make API access from the
browser possible, what’s the benefit of wrapping the API in a JavaScript SDK? There
are a number of good reasons:
 Additional browser support—CORS is a working draft spec and doesn’t have

full browser support. With your JavaScript SDK, you can instead use cross-
domain tunnels to initiate HTTP requests to your API. Paired with the browser-
compatibility techniques covered in chapter 4 (or a library like easyXDM),

you’ll be able to support a larger number of users than CORS alone.
 Easier abstractions—By providing a set of clearly named and documented
JavaScript functions that access your API, you can eliminate a lot of boilerplate
code for developers, which helps publishers develop for your platform faster.
This also lets you side-step complicated concepts like JSONP and CORS, thus
lowering the entry barrier for developers.
 Decoupling—Web service APIs change all the time, sometimes with breaking
changes for publishers. If you wrap calls to your API from your JavaScript SDK,
it’s possible to anticipate and fix breaking changes at the JavaScript level.
Those may not be critical issues for you—and that’s okay. Not everyone needs to go
the extra mile of building an SDK interface that wraps calls to their web service API.
But you’d be how surprised how easy it is to implement one. Over the remainder of
this chapter, we’ll show you how.

---

#### From [[_3_wrapping-apis]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_wrapping-apis]: _3_wrapping-apis "Wraping APIs"
[//end]: # "Autogenerated link references"

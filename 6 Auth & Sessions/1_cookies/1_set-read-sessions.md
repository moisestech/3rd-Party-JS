# Set, Read Sessions

## **6.1.1 Setting and reading sessions**

Let’s look at a scenario where the Camera Stork widget uses third-party cookies to
store a user’s session. Suppose that you’ve added a link to the Camera Stork widget,
labeled Login, that when clicked reveals a form containing username and password

inputs (see figure 6.1). For security purposes, this form and the entire widget are con-
tained within an external iframe hosted on camerastork.com. This is to prevent code

on the publisher’s page from accessing the username and password inputs.

**Figure 6.1 Clicking the Login link reveals a form for authenticating with the Camera Stork service.**

When the form is submitted, it triggers an AJAX call to an authentication endpoint on

your servers. If the submitted username and password are valid, the application gener-
ates a new session in its database.1

The server returns a JSON response with the user’s
information, but, more importantly, sets a cookie that contains the newly generated
session ID.
Here’s the HTTP server response from the AJAX endpoint:
HTTP/1.1 200 OK
Date: Wed, 28 Sep 2011 14:33:37 GMT
Content-type: application/jsonSet-cookie
Set cookie: sessionid=66d2520a77a3c6dac4db658c6dd13061
{ "username":'johndoe', "first":'John', "last":'Doe' }

Now that you have the user’s information returned as JSON, you can update the wid-
get to reflect that the user is logged in. You could do this by replacing the Login link

with the user’s full name, for example.
What’s more important in this example HTTP response is the Set-cookie header.
This will have the effect of persisting the user’s session ID inside a cookie stored on the
user’s browser. When the user subsequently visits any publisher’s page containing your
widget, your application will initiate a browser request to fetch the product’s data, just
as it did before. This time, the browser will pass the stored cookie as part of the HTTP
request:
GET /products/1337.json HTTP/1.1
Host: camerastork.com
Accept: application/json
Cookie: sessionid=66d2520a77a3c6dac4db658c6dd13061

Again, you’ll test the validity of the passed session ID on your servers, and if every-
thing’s kosher, return some metadata about the user in your response. And like

before, you’ll render the user’s identifying information into the widget, to help indi-
cate visually that they’re logged in.

What we’ve just described is the standard, conventional way of using cookies to
persist sessions. If you’ve done any kind of web application development before, this
should look familiar to you. What’s different is that this cookie is being set on a

domain (camerastork.com) that’s not the same as the domain of the top-level docu-
ment (your publisher’s website). It’s thus considered a third-party cookie and subject

to all the browser restrictions that can be placed on third-party cookies—including
being outright disabled.

---

#### From [[_1_cookies]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_cookies]: _1_cookies "Cookies"
[//end]: # "Autogenerated link references"

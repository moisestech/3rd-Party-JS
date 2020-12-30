# Service API Wrappers

## **1.2.3 Web Service API Wrappers**

In case you’re not familiar with them, **web service APIs are HTTP server endpoints that enable programmatic access to a web service**.

Unlike server applications that return HTML to be consumed by a web browser, **these endpoints accept and respond with structured data—usually in JSON or XML formats—to be consumed by a computer program**.

This program could be a **desktop application** or an **application running on a web server**,  
or it could even be **client JavaScript code** hosted on a web page but **executing in a user’s browser**.

This last use case - **JavaScript code running in the browser—is what we’re most interested in**.

**Web service API providers can give developers building on their platform—often called integrators**.
— third-party scripts that simplify client-side access to their API\*\*.

We like to call these scripts **web service API wrappers**, since they’re effectively **JavaScript libraries that “wrap” the functionality of a web service API**.

**EXAMPLE: THE FACEBOOK GRAPH API**

### **Example: Developer Jill**

Jill’s decided that in order to better appeal to potential employers, she needs a terrific-looking online resume hosted on her personal website.

**This resume is for the most part static.**  
It lists her skills and her prior work experience, and even mentions her fondness for moonlight kayaking.

Jill’s decided that, in order to demonstrate her web development prowess, there ought to be a dynamic element to her resume as well. And she’s got the perfect idea.

What if visitors to **Jill’s online resume—potential employers—could see if they had any friends or acquaintances in common with Jill** (see figure 1.5)?

Not only would this be a clever demonstration of Jill’s skills, but h**aving a common friend could be a great way of getting her foot in the door**.

To implement her dynamic resume, **Jill uses Facebook’s Graph API**.

This is a **web service API from Facebook that enables software applications to access or modify live Facebook user data** (with permission of course).

`Figure 1.5` **At the bottom of Jill's resume, the visitor can see friends they share with Jill**

**Facebook also has a JavaScript library that provides functions for communicating with the API.**

Using this library, **it’s possible for Jill to write client-side code that can find and display friends common to herself and a visitor to her resume**.

`Figure 1.6` illustrates the **sequence of events that occur between the browser and the two servers**.  
[**IMAGE NEEDED**]

**`Firegure 1.6` Embedding Facebook content in a website using client-side JavaScript**

`Listing 1.4` Shows the code to implement this feature on her resume. To keep things simple, this example uses jQuery, a JavaScript library, to simplify DOM operations. Learn more at http://jquery.com.

`Listing 1.4` **Using Facebook's Graph API to fetch and display a list of mutual friends**

Jill managed to embed some powerful functionality in her resume, all using a small amount of client-side JavaScript. With this impressive piece of work, she should have no problem landing a top-flight software job.

### **BENEFITS OF CLIENT-SIDE API ACCESS**

#### Dev Jill Example Continued:

It’s worth pointing out that **this entire example could’ve been done without client-side JavaScript**.  
Instead,
**Jill could’ve written a server application to query the Facebook Graph API for the necessary data**
and then render the result as HTML in the response _Listing 1.4_ Using Facebook’s Graph API to fetch  
and display a list of mutual friends Load both jQuery and Facebook JavaScript SDK.

Initialize Facebook JavaScript SDK. You need to register your application at http:// developers.facebook.com and obtain an application ID first. `FB.login` opens up new Facebook window asking website visitor to log in and grant the application permission to access the visitor’s data.

If login was successful, request visitor’s mutual Facebook friends using the Graph API’s / mutualfriends/ endpoint. When response is ready, execute `showMutualFriends` **callback function**.
Iterate through list of friends and render them to page.

Developing a bare-bones widget to the browser.  
In that case, the browser downloads the HTML from Jill’s server and displays the result to the user—no JavaScript is executed.

### **Benefits of 3rd Party**

But **it’s arguably better to have the website visitor perform this work in the browser, for a few reasons**:

1. Code executing in the **browser is code that’s not executing on the integrator’s servers**, which can **lead to bandwidth and CPU savings**.

2. It’s **faster—the server implementation has to wait for the response from Face- book’s API before showing any content**.

3. **Some websites are completely static, such that client-side JavaScript is their only means of accessing a web service API**.

### **AN API FOR EVERY SEASON**

This example we just covered might be regarded as a niche use case, but this is just one possible application.  
**Facebook is just a single web service API provider**, but the reality is that there are thousands of popular APIs, all of which provide access to varying data and/or functionality.  
Besides social networking applications like **Facebook, Twitter, and LinkedIn**, there are publishing platforms like **Blogger** and **WordPress**, or search applications like **Google** and **Bing**, all of which provide varying degrees of access to their data via APIs.

Many web services—large and small—offer APIs. But not all of them have gone the extra mile of providing a JavaScript library for client-side access. This matters because **JavaScript in the browser is the single largest development platform**: it’s supported on every website, in every browser.

**If you or your organization develops or maintains a web service API**, and **you want to reach the largest number of possible integrators possible**, you owe it to yourself to **provide developers with a client-side API wrapper—a topic** we’ll discuss in detail later in this book.

---

#### From [[_2_uses-3rd-party-js]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_uses-3rd-party-js]: _2_uses-3rd-party-js "Uses of 3rd Party"
[//end]: # "Autogenerated link references"

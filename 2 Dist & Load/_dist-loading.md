# 2️⃣ Dist & Loading

## **This Chapter Covers**

1. Configuring your development environment
2. Including Scripts on a Publisher’s Web Page
3. Loading supporting JS files and Libs
4. Passing parameters to your 3rd Party Script

### **Project Example**

Let’s pretend you’re the owner of a small but growing e-commerce website. Your
online store specializes in the sale of cameras and camera accessories, and carries
the playful name of Camera Stork (http://camerastork.com).1

You’re doing well at

attracting business, and you’ve developed a loyal customer base of both profes-
sional and amateur photographers. Your website has a bustling review section for

each product, where many of your customers have submitted ratings and reviews of
their purchases.
Some of these customers are so fanatical about your store that they’d love to
refer directly to your products from their own blogs and websites. In the past,
you’ve handed out referral URLs for publishers to link directly to your products,
but the experience is subpar, and you’d like to go further.

The solution: an embeddable widget (see figure 2.1) that
shows up-to-date information about a specific product, and a
link to purchase it at your store. It’ll include all the standard
product information: name, photo, price, and average rating.
But it won’t be strictly read-only. Given that your store has a
strong emphasis on user reviews, you also want users to be

able to submit new reviews from the widget. Since it’s impor-
tant to know who’s reviewing what, you also want users to be

able to log in through the widget, and even persist sessions
between different widget instances.
As you might expect, you’ll implement this widget as a
third-party JavaScript application. Just like the weather widget
example from chapter 1, loyal fans (or publishers) will obtain
an HTML snippet from your e-commerce store and insert it
into their own web pages’ HTML source code. This snippet will load your application
code and render the widget on their web page. The location in their HTML source
code where the snippet is placed denotes where the widget will be rendered.
We’ll develop this product widget over the course of the book. Later chapters will

deal with more advanced topics: communicating with the server, rendering, maintain-
ing sessions, and performance. But before we get there, we have to address some

important first steps: getting your application code loaded and executed on the pub-
lisher’s website.

There’s a lot more to this than you might expect. For starters, you’ll need to craft
the aforementioned HTML snippet that will load your initial script file on publishers’
web pages. Then you’ll need to load any supporting JavaScript files, libraries, and
stylesheets. You’ll also need to figure out how the publisher will pass parameters to
your script files, in order to identify what product they’re trying to embed on their
page. And unlike the weather widget example from chapter 1, you’ll do all of this
without using any server-side script generation.
But since this is your first foray into the art of third-party development, you’ll also
want to spend a few minutes to configure your local development machine to represent
the cross-domain environment you’ll be writing your code for. Let’s tackle that next.

**Figure 2.1 The feature complete Camera Stork product widget**

## **2.1 Config Env for 3rd-Party Dev**

[[_1_config-env]]

## **2.2 Loading the Initial Script**

[[_2_loading-script]]

## **2.3 The initial Script File**

[[_3_init-script]]

## **2.4 Loading Additional Files**

[[_4_loading-additional]]

## **2.5 Passing Script Arguments**

[[_5_passing-args]]

## **2.6 Fetching App Data**

[[_6_fetching-app-data]]

## **2.7 Summary**

- There’s a lot of **up-front work to be done before you can render a widget on a publisher’s page**.

1. First, you’ve got to **distribute your script include snippet**,
   - **the entry point for loading the application**.
2. You then **need to load any dependencies**,
   - like **additional JavaScript files**, or **libraries**.
3. Last, you **need to pass identifying information from the host page to the application**,
   - so that you can **properly retrieve and submit data for the correct object**.

- You’ve seen how **there are a variety of techniques for implementing each of these first steps**.
- **Techniques that vary in complexity and scope**.
  - It’s up to you to **choose the techniques that best suit your application**.
- At this point, **product widget development** is well under way.

### **Project Widget**

- You’ve **given your loyal publishers a script include snippet**
  - that they’ve since **added to their HTML source code**.
- That **snippet is loading your initial script file**,
  - `widget.js`, onto their web page.
- That initial script has **loaded**
  - **all its dependent JavaScript files and libraries**.
- The script has **also extracted the product ID from the script include snippet**,
  - and **used that ID to extract the corresponding product information from the embedded catalog object**.

### **Next Chapter**

[[_html-css]]

- All this **code is executing on the publisher’s page**,
  - but **there’s nothing to see yet**.
- **The widget still hasn’t been rendered**.
- Luckily, this is the focus of the next chapter,
- **“Rendering HTML and CSS.”**

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_config-env]: 1_config-env/_1_config-env "Config Env"
[_2_loading-script]: 2_loading-script/_2_loading-script "Loading Script"
[_3_init-script]: 3_init-script/_3_init-script "Init Script"
[_4_loading-additional]: 4_loading-additional/_4_loading-additional "Loading Additional Files"
[_5_passing-args]: 5_passing-args/_5_passing-args "Passing Args"
[_6_fetching-app-data]: 6_fetching-app-data/_6_fetching-app-data "Fetching App Data"
[_html-css]: ../3 Render HTML & CSS/_html-css "3️⃣ HTML & CSS"
[//end]: # "Autogenerated link references"

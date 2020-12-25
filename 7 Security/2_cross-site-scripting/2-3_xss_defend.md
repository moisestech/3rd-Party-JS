# XSS-Defend

## **7.2.3 Defending your application against XSS attacks**

All web applications—and especially third-party widgets—have many sources of untrusted data, and these sources can become entry points for XSS attacks. Unfortunately, the only viable solution for defending yourself against such attacks is to carefully filter all data coming from the outside—no matter whether it comes from the end user or even from your publisher.

In general, before inserting any external data into the DOM (both the publisher’s and your own), you should convert any characters contained therein that could be used to transform an innocent string into an offending piece of HTML/JavaScript code:

- Convert < into &lt;
- Convert > into &gt;
- Convert & into &amp;
- Convert " into &quot;
- Convert ' into &#39;

Converting these characters into the corresponding HTML entities will make sure that an attacker won’t be able to insert a <script> tag into their message and run arbitrary code within your widget. This approach is called sanitization, and it’s often used when there’s a need to accept data that can’t be guaranteed to be safe. In this case, you process the data and delete or convert all potentially offensive parts—like HTML tags.

Effective sanitization is a difficult problem to solve, especially if you want to give your users the ability to use some HTML elements (the ones that don’t expose vulnerabilities). There are different approaches to data sanitization but the most effective ones work by filtering the data to maintain control over the input.

**REJECT KNOWN BAD**
Let’s consider a simple WYSIWYG editor that you’re building for your Reviews widget.  
You want users to be able to use simple tags like <strong> and <em>, but don’t want
to be sad when somebody decides to hijack other users’ sessions by inserting a `<script>` tag.

The first thing that comes to mind is to strip all instances of <script>. This is a simple solution, or—to paraphrase famous American journalist Henry Louis Mencken—this solution is neat, plausible, and wrong. The only thing the attacker needs to do in order to bypass such a filter is to replace their initial <script> tag with `<scr<script>ipt>`.

Your filter will replace <script> with an empty string, accidentally generating another <script> element as a result. And in the case of multistep validation—when you validate user input using multiple filters—the attacker might be able to exploit the ordering of these steps to bypass them all.

The approach we just described is commonly referred to as reject known bad. You have some kind of a blacklist containing a set of patterns that are potentially dangerous to your widget and for every input you check it against that blacklist. Although this approach is sometimes necessary—for example, when the input is a free-form text field, like a product review or email body—it’s probably the least safe technique to employ when defending your application against XSS attacks.

The data transmitted through the web can be encoded in so many ways that there’s a big chance that you’ll
miss a few patterns in your blacklist.

**ACCEPT KNOWN GOOD**
A better approach that you should use whenever possible is known as accept known
good. This is the exact opposite of the previous approach. Instead of maintaining a
blacklist of bad values, you keep a whitelist of good (safe) values. For example, if you’re
passing a product identifier into your widget’s iframe from the publisher page, instead
of sanitizing the input, you can verify that the value is numeric:

```javascript
function isValidIdentifier(uid) {
  return !isNaN(uid);
}

function setConfig(config) {
  var args = {};
  if (config.productId && isValidIdentifier(config.productId)) {
    /* ... */
  }
}
```

Another good approach, when you’re trying to limit the input data only to good values, is to narrow the scope around the input. Consider the CSS cross-site scripting example we showed back in listing 7.2. The code expected a valid hexadecimal color representation, but instead the attacker provided a CSS expression and stole sessions from users (who happened to be using early versions of Internet Explorer).

The next listing shows how to prevent this attack by narrowing the scope of how you apply the data provided to you from the publisher’s page.

**Listing 7.3 Preventing XSS attacks by narrowing the scope around the input data**

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="/js/helpers.js"></script>
    <script>
      var args = parseQueryArguments(window.location.search);
      var body = document.getElementsByTagName("body")[0];
      if (args.bg) {
        args.bg = args.bg.split(";")[0];
        body.style.backgroundColor = args.bg;
      }
      if (args.fc) {
        args.fc = args.fc.split(";")[0];
        body.style.color = args.fc;
      }
    </script>
  </head>
</html>
```

As you can see from listing 7.3, you don’t even need to sanitize the code because—in case of invalid, unexpected data—the worst thing that can possibly happen is that the attacker won’t see the correct background color. And since we don’t like the attacker anyway, it’s no biggie.

Checking your code for potential XSS holes can be frustrating, but you should never, ever underestimate the impact of XSS attacks. A few hours of additional work is better than an embarrassing security breach and the loss of trust from your publishers and users.

In the next section, we’ll talk about another big source of attacks—a vulnerability called cross-site request forgery that allows the attacker to quietly make malicious requests using the identity of your user.

---

#### From [[_2_cross-site-scripting]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_cross-site-scripting]: _2_cross-site-scripting "Cross-Site Scripting"
[//end]: # "Autogenerated link references"

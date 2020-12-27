# Optim User Actions

## **9.3.1 Optimistic user actions**

Let’s look closer at the use case of submitting a review to the Camera Stork widget. To
perform this action, the user types their message into the widget’s input box, and then
hits the Submit button. Afterward, the usual flow undertaken by the program looks like
this: the widget displays a loading indicator, transfers the review data to the Camera
Stork servers, and then waits for the response. When the widget receives a response

from the server, it hides the loading indicator,
and either shows an error or inserts the newly
created review somewhere into the DOM. This
flow is visualized in figure 9.4.
You can’t optimize this flow unless your
back end is slow for some reason, so your
users will probably see a loading indicator
from time to time and will have to wait until
the widget is done and the review appears on
the page. This will be more noticeable if you
have to initialize a cross-domain channel (or
any other resources) before you can send
data to the server.
But ask yourself a question: how often do
users get errors from the server? Considering

that you can duplicate most of your valida-
tion logic on the client—to catch user mis-
takes early—the answer is rarely. So instead of

showing a loading indicator and waiting for
the response from the server, why not insert
the user’s review into the DOM as if it has
been successfully saved? And, in the unlikely
event of an error returned from the server,
you can revert your DOM modifications and
display an error apologizing to the user. This flow is shown in figure 9.5.

**Figure 9.4 Conservative approach to displaying the results of a user action. Nothing is rendered until the server responds.**

**Figure 9.5 Optimistic approach to displaying the results of a user action. A success message is rendered before the server responds.**

Technically speaking, this version of the program isn’t running any faster. But
since users will see an immediate response from submitting their review, they’ll get the
warm fuzzy feeling of your widget being amazingly fast.

---

#### From [[_3_perceived-performancew]]



# Multi Auth

## **6.3.2 Multilevel authentication**

Session hijacking is a reality faced by every web application, but the impact is greater
for some applications than others. For example, if an attacker were to gain access to a

user’s session for a small online forum, they might be able to make disparaging com-
ments under that user’s identity. Certainly an awful situation, but perhaps an accept-
able risk. Contrast this with a banking application—an attacker gaining a user’s

banking session could conceivably empty their bank accounts, commit identity fraud

with the information contained therein, or perform any number of financially damag-
ing actions. This kind of security breach could sink a company.

Let’s go back to the Camera Stork example for a moment. If you recall, you were

building a widget for camerastork.com, an existing e-commerce store that sells photog-
raphy equipment online. Let’s imagine that Camera Stork users store their credit card

information on camerastork.com, so they don’t have to punch in their details every
time they buy a shiny new lens filter. The downside is that if an attacker gained access

to a user’s session, they might be able to order products for themselves with the stored
credit card information. You definitely don’t want attackers to gain hold of that session
information. Compare this action of purchasing products on the camerastork.com
website to the types of authenticated actions that can be performed via the Camera
Stork product widget—which, right now, is just submitting reviews for products.
There are clearly two classes of potential actions that can be taken by the user—
those that are financially damaging (on the main camerastork.com website), and
those that are somewhat frivolous (reviewing products on the widget). Why should the
same session be used for performing both sets of actions?
This is the idea behind multilevel authentication. Instead of using one session to
authorize all possible actions with your application, you generate multiple session
tokens that correspond to different levels of authentication. For example, one token
could be used for performing relatively low-risk actions like reviewing products via the
Camera Stork product widget. The other token could correspond to more sensitive
actions, like purchasing products using a stored credit card number.

On their own, two session tokens aren’t any more secure than using a single ses-
sion token; a high-level token could be stolen just as easily a low-level token. Multilevel

authentication becomes valuable when you want to serve sensitive parts of your appli-
cation (and their cookies) using HTTPS, and low-risk parts of your application using

plain-text HTTP. But when would you want to do that?
As is illustrated in figure 6.9, ideally you should use HTTPS for all requests that
handle private session data. The web development community would tar and feather
us for suggesting any less. But the hard reality is that HTTPS requires additional server
overhead, and, at massive levels of scale, serving HTTPS for all your application

requests can be a difficult engineering challenge. For example, Facebook, despite hav-
ing over 1 billion users, didn’t provide HTTPS to all its users until November 2012.

Before that, Facebook only offered HTTPS as an optional setting that users had to
enable themselves.6

Not every company is Facebook, but in developing a third-party

application, you might find yourself dealing with considerable levels of scale, and mul-
tilevel authentication can help your web application stay afloat by sanely partitioning

low-risk requests to use unencrypted HTTP.
Session hijacking isn’t some made-up bogeyman meant to scare you—it’s real, and
it poses a greater threat to third-party web applications. Ultimately, there’s no better
precaution than using HTTPS everywhere you transmit session information. But if you

can’t do that, using some level of HTTPS with multilevel authentication will signifi-
cantly mitigate the risk of session hijacking on your service without surrendering too

much performance.

---

#### From [[_3_securing-sessions]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_securing-sessions]: _3_securing-sessions "Securing Sessions"
[//end]: # "Autogenerated link references"

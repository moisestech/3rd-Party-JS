# Securing Sessions

## **6.3 Securing Sessions**

Session hijacking is a situation where a malicious party gains access to a user’s session

with a web service in order to make unauthorized requests. We outlined one such vul-
nerability of sessions in the last section: a user’s session ID can be obtained by a mali-
cious publisher if you store that ID in a variable in the publisher’s page context. That’s

just one situation, and possibly a rare one.
What’s far more likely is a malicious party obtaining a user’s session through HTTP

sniffing. So far, we’ve been describing an application that uses HTTP to transfer ses-
sions, using either cookies or as a GET or POST parameter. HTTP is an unencrypted

transfer protocol, and as such, there are situations in which full request bodies can be
read in plain text by unauthorized parties.
This happens most frequently when people use an unencrypted Wi-Fi network,

many of which are found at cafes, airports, or even your local library. Over such net-
works, all network requests between your computer and the network are transferred

in plain text. This means that other users on the network can record and inspect your
HTTP traffic, and extract any private session information for their own use. There are

even automated tools for identifying and hijacking cookies for popular web destina-
tions. The most popular of these, a Firefox plugin named Firesheep,4

will show you a
list of users on an unencrypted network currently signed into Facebook, Flickr, and
other popular web services, and let you sign in as those users with just the click of a
button (see figure 6.8)!
This vulnerability is worse for third-party applications. In order for Firesheep to
identify a session to hijack on the network, the user has to visit a popular web service

**Figure 6.8 Firesheep, a Firefox browser plugin, makes it easy to hijack users’ sessions on unencrypted networks.**

first (Firesheep sniffs the HTTP request). If the user doesn’t visit the site, there’s no
harm. But third-party applications are distributed, and can be installed on a great

number of web properties (for example, Facebook’s Like widget is seemingly every-
where). In those cases, just requesting a single instance of these types of applications,

no matter where they’re situated, could potentially transfer cookies in plain text over
the network. The odds of having your session hijacked just became much higher.
We’re starting to paint a grim picture, but don’t worry, all hope isn’t lost. There are

a couple of measures you can take to eliminate session hijacking, or at the least mini-
mize its impact significantly. We’ll first look at encrypting traffic to your application

using HTTPS. Then we’ll explore using multilevel authentication, where you generate
multiple session tokens that correspond to different levels of actions. First up: HTTPS.

## **Sub-chapters**

- [[1_https-secure-cookies]]

---

#### From [[_auth-sessions]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[1_https-secure-cookies]: 1_https-secure-cookies "Https Secure Cookies"
[_auth-sessions]: ../_auth-sessions "6️⃣ Auth & Sessions"
[//end]: # "Autogenerated link references"

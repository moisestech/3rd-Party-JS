## **6.2.1 Using dedicated windows**

As you may recall from earlier, a third-party cookie is only considered such when it’s
being set from or read from a domain that differs from the document requested by

the user. If the browser user happens to be visiting your domain directly (camera-
stork.com), then you can set cookies freely—you’re no longer a third party. So all you

need to do is get the user to visit your page directly, and you’re home free.
OPENING A NEW WINDOW
The best way to do this is to serve your authentication/login page (in our example,
camerastork.com/login) from a new window, opened by your application using
JavaScript. Because this is a new window, the browser will consider this a first-party
request, and you’re free to persist any cookies. When the user is finished, you can
communicate the user’s details from the new window back to the application using a
window messaging API (like postMessage, or for wider compatibility, easyXDM).
For this example code, let’s assume that the majority of the Camera Stork widget is
being rendered inside of an iframe. Inside the iframe is a Login link that appears next
to the other widget content. You’ll assign a click event handler to the Login link, and
when it’s clicked, open a new window to your dedicated login page (listing 6.2). Yes—
you can open a new window from inside an iframe, just as you would from the parent
document. You’re also not restricted as to the URL in which that new window opens; it
can be a different origin than the iframe.

**Listing 6.2 Opening a new window from code executing inside an iframe**

var popup;
function openLoginWindow(e) {
e.preventDefault();
popup = window.open('http://camerastork.com/widget/login.html',
'\_blank', 'width=460,height=225');
}
var login = document.getElementById('login');
if (login.addEventListener) {
login.addEventListener('click', openLoginWindow, false);
} else if (login.attachEvent) {
login.attachEvent('onclick', openLoginWindow);
}

**Figure 6.5 The Camera Stork login window, opened dynamically from your third-party script**

Now, when the user clicks the Login link, the new window will open. And this proba-
bly shouldn’t be just any old login page. It’s best if this is a login page that’s designed

to be confined in a new window—ideally, a small one. That doesn’t mean it has to look
ugly, either. Figure 6.5 shows what the Login page for the Camera Stork widget will
look like.
Not bad looking, right? You can create a pretty decent user experience within the
constraints of a dedicated new window.3

AVOIDING POP-UP BLOCKERS Today, every web browser has a pop-up blocker
built in, which means that if you’re not careful, the browser can prevent you
from opening new windows from JavaScript. To avoid this, always make sure
that you open a new window as a direct result of a user action—like a mouse click
or form submission. If there’s a delay between when the user invoked an

action and you open the window, most browsers will see this behavior as mali-
cious and block the operation.

Now, let’s say the user enters their username and password, and hits the Sign-in but-
ton, which POSTs the form to your servers. Your server-side code will verify the user-
name and password, generate a new session, and assign the newly generated session ID

to a cookie. It’ll also render a new HTML page as part of the response, whose purpose

is to communicate the successful authentication back to your application code run-
ning inside the parent iframe (see figure 6.6).

COMMUNICATING THE SUCCESSFUL AUTHENTICATION
To send messages between an iframe and a window object, you’ll need some kind of
window-messaging API. For simplicity, we’ll show you how to do this using the
window.postMessage API (in production, you’d probably want to use easyXDM or a
comparable library for greater browser compatibility).

Figure 6.6 Using a dedicated window to authenticate a user on camerastork.com

To send a message back to the iframe using postMessage, you can use the following
code:
window.opener.postMessage(
JSON.stringify({success: true, name: 'John Doe'}),
'http://camerastork.com'
);
Unless the iframe is actually listening for this message, nothing will happen. You’ll
need to define a postMessage listener function back in the parent iframe before the
window is opened. When the listener receives the successful authentication message,

it’ll close the pop-up window, because it has fulfilled its role and is no longer neces-
sary. These steps are implemented next.

**Listing 6.3 postMessage listener waits for success message from the pop-up window**

function onMessageReceived(e) {
var response = JSON.parse(e.data);
if (response.success && popup) {
popup.close();
renderUserName(response.name);
}
}
if (window.addEventListener) {
window.addEventListener('message', onMessageReceived, false);
} else if (window.attachEvent) {
window.attachEvent('onmessage', onMessageReceived);
}

That’s it—you’re done. You’ve successfully managed to set a cookie on the camera-
stork.com domain even with third-party cookies disabled. Please note that, although

this example assumed your application code was running inside an iframe hosted on
your domain, the same general solution will work for code that’s executing on the
publisher’s website. The only difference is that the origin parameter to postMessage
will need to be the publisher’s domain (or '\*') instead of camerastork.com.
It’s worth noting that setting cookies in a new window like this will work in every
browser. It’s just of limited value for Firefox and Chrome because, even though the
cookies are set successfully, those browsers won’t send them for subsequent requests.
As we’ve suggested earlier, those browsers will need a separate workaround, which
we’ll cover later in this chapter.

---

#### From [[_2_setting-cookies]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_2_setting-cookies]: _2_setting-cookies "Setting Cookies"
[//end]: # "Autogenerated link references"

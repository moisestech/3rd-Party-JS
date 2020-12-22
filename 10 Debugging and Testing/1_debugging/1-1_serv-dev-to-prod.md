## **10.1.1 Serving development code in production**

When faced with a bug in production, it’s clear that you’ll want your unminified devel-
opment code before debugging a publisher’s live website. Well look at two solutions

for making that happen: rewriting proxies and feature switches. Both techniques have
their pros and cons, and which technique you’ll use depends on the situation at hand.
Let’s start with rewriting proxies.
REWRITING PROXIES
Proxies are server applications that stand in between the client and the server, acting
as intermediaries for requests. Such proxies can be used for a variety of different tasks.
A common proxy server is a caching web proxy—an application that caches web pages
and static files requested from remote web servers to make their delivery faster for
local network clients.

**Figure 10.2 Two browsers requesting the same resource but receiving different results. Browser A goes through a proxy and receives the unminified copy of Camera Stork, whereas Browser B goes directly and receives a minified version of the widget.**

For our purposes, we’ll use a similar concept: an intermediary server application that
listens for requests to your application’s initial JavaScript file (the loader) and
reroutes the client to use a development version. And since you don’t want every user

to hit the proxy server—you don’t want regular users to have access to your develop-
ment code—you’ll have to manually configure your browser to use the proxy server.

Figure 10.2 shows an example of two browsers accessing the same page but getting
two different results. A browser behind the proxy receives a development version of
Camera Stork, while everyone else gets to use a version stored on your production
servers.
Proxy servers that reroute certain types of requests from their original destination
are also known as rewriting proxies. Their job is to intercept, analyze, and rewrite
requests passing through them.
To debug the Camera Stork widget, your proxy will intercept requests to
media.camerastork.com and rewrite them as requests to media.camerastork.dev. And
although you could set up your proxy to rewrite requests to the higher-level
camerastork.com domain, you probably don’t want to do this. This is because your
local computer may not have access to the production database servers or web service
APIs provided by camerastork.com. By rewriting only the subset of requests that load
static resources, you’ll make your browser use your development code while running
“live” on a publisher’s website.
CONFIGURING APACHE AS A REWRITING PROXY
Let’s configure Apache as a rewriting proxy. We picked Apache because it’s the de
facto standard web server on the web, and most UNIX-based operating systems
(including OS X and many Linux distributions) have a version of Apache installed by
default. To turn Apache into a rewriting proxy, you’ll need to enable two modules:
proxy and rewrite. To do so, you’ll use a special program called a2enmod. Just run the
following command from your terminal:
$ a2enmod proxy rewrite

MODULES IN APACHE Apache supports a lot of different features, and many
of them are implemented as compiled modules. These modules extend the
core functionality of the server and can be enabled or disabled with special
programs named a2enmod and a2dismod. Note that in environments where

a2enmod and a2dismod aren't available, you can still enable modules by edit-
ing Apache config files. Please consult the Apache documentation online for

more information.
After you’ve enabled these modules, you need to configure your Apache settings file
to intercept all URLs that have media.camerastork.com as their hostname and swap
that hostname with media.camerastork.dev. The settings file that you need to modify
depends on your setup and your operating system. For example, since you probably
don’t want to dedicate a whole new machine with a separate IP address to be your

proxy server, you’ll need to set up a name-based virtual host on your Apache installa-
tion. Name-based virtual hosting is a way for you to host multiple host names from the

same IP address and—most often—from the same physical machine.
Apache makes it easy to configure multiple name-based virtual hosts. For example,
on OS X all you need is to modify /etc/apache2/extra/httpd-vhosts.conf. To tell Apache
to rewrite all requests going to media.camerastork.com with media.camerastork.dev,
you’ll need to open that file and add a new entry for your proxy server:
<VirtualHost _:8080>
ServerName proxy.dev
RewriteEngine on
RewriteCond %{HTTP*HOST} ^media.camerastork\.com
RewriteRule ^proxy:http://.*?/(.\*)$ \
http://media.camerastork.dev/$1 [P,L]
</VirtualHost>
After you’re finished modifying the settings file, just restart your Apache instance and
you’re golden:
$ apachectl restart
Now that you have a rewriting proxy server up and running, all you need to do is to
configure your web browser to use it. You can configure your entire operating system
to connect to the proxy, which is typically found in your operating system’s network
preferences. Better yet, some web browsers have built-in support for HTTP proxies,
like Mozilla Firefox. To configure Firefox to use your proxy, go to Preferences >
Advanced > Network and provide proxy.dev:8080 under HTTP Proxy, as shown in
figure 10.3.

When you’re done, you can revisit the offending publisher’s page and try repro-
ducing the bug. This time, your browser will use the Camera Stork’s development

JavaScript files, so you can fire up your browser’s debugger and start stepping through
code. And if you’ve never touched a JavaScript debugger before, don’t worry—we’ll
show you how shortly. But for now let’s look at an alternative way of configuring your
debugging environment to access development code—feature switches.

**Figure 10.3 A network connections dialog in Mozilla Firefox where you can provide your custom HTTP proxy information**

FEATURE SWITCHES
If you’ve ever used Twitter or Facebook, you might’ve noticed that these companies
don’t release new features to all their users at once. Most often, they start by giving a
small percentage of their users access to a new feature (say, 10%), and then, after
additional testing, they gradually increase that percentage until it reaches all users.
You can use this same technique with a system of feature switches (sometimes also called

feature flippers). Switches are special conditions that allow you to restrict certain fea-
tures of your application to a subset of users, publishers, and environments. This

means that you can use switches to decide—at request time—which code path to
choose, which interface to load, and so on.
At this point, you might be wondering how feature switches relate to debugging
and testing your application. With feature switches, you can have both versions of
your code—production and development—on your servers and pick which one to

load based on the current request and your current switch configuration. For exam-
ple, you can make a switch that loads debug code only when a user with the user-
name admin makes a request to your servers. Then, in order to be served unminified

and debuggable JavaScript code, you merely need to log in as the admin user and visit
the offending page.
Let’s configure a switch that implements this behavior. We’ll do this by declaring

the switch definition in your application’s configuration file. For an application writ-
ten in Python, you can create a file called settings.py that holds your switch configura-
tion, such that it can be imported by other application files. Because this is a regular

Python file, you can use regular Python objects like arrays and dictionaries to declare
the current switch state:

# settings.py

SWITCHES = {
'debug': [ 'admin' ],
'new_user_interface': []
}

# views.py

from settings import SWITCHES
def loader(self, request):
if request.user.username in SWITCHES['debug']:
return js('loader.debug.js')
else:
return js('loader.js')
You’ll notice that in this example we hardcoded a list of usernames in the settings file.
This means that whenever you want to add or remove a user from a switch, you’ll need
to push your updated settings file to your production servers—redeploy your code.
But having to redeploy code every time you want to update a switch is exactly what we
were trying to avoid!
So the next obvious step is to move your switch data into the database. By storing
your switches in the database, you can access their data like any other data stored on
your servers—like the Camera Stork product catalog, user reviews, and so on. With
this setup, you don’t have to redeploy your code every time you make a change to your
switch configuration, which makes toggling switches a simpler and faster process.
With your switch configuration in place, let’s see what happens the next time you
load the Camera Stork widget. It begins with the browser sending a request to your
servers, which request is then analyzed by your application. The application will then
query the database asking whether the debug switch should be turned on for that
request. If the answer is yes, the application loads the development version of your
JavaScript files suitable for debugging. And if the answer is no, then it proceeds to load
your application normally. Figure 10.4 demonstrates this behavior.
The actual code that decides which version to load isn’t particularly complicated.
You need to make a database query and check whether the current user has the debug
switch enabled for them. For example, feature switches in a typical web application
written in Python might look like this:

from models import Switches
def loader(self, request):
sw = Switches.objects.get(name='debug')
if request.user in sw.users:
return js('camerastork.debug.js')
else:
return js('camerastork.js')
The actual implementation of database-backed switches is out of the scope of this
book. But you should be able to find information on how to implement feature
switches for your web platform of choice. There are web framework plugins and
libraries out there that will do this for you.
DJANGO GARGOYLE If you happen to be using Django as part of your web
application stack, we recommend you take a look at Gargoyle. Gargoyle is a
switches platform implementation built by engineers at Disqus on top of
Django. It’s both powerful and easy to use:
from gargoyle import gargoyle
def loader(self, request):
if gargoyle.is_active('debug', request):
return js('loader.debug.js')
else:
return js('loader.js')
Additionally, Gargoyle has a convenient web interface for toggling switches
from the browser. Gargoyle is an open source project and is available on
GitHub: https://github.com/disqus/gargoyle.
Now that you know how both switches and proxy servers work, the questions remains,
which is better for serving debuggable development files? The answer is blurry.
Switches are great because they’re easy to implement, can be enabled with the press of
a button, and can be enabled under a variety of complex parameters (username, IP
address, browser version—the sky’s the limit). Proxies can offer you more fine-grained
control over what files you’re serving. For example, with proxies, you can even test
websites using an experimental version of your local JavaScript code. At Disqus, we use
both solutions day to day.

**Figure 10.4 Loader checks status of a switch by checking its value in the database and decides whether to load development files or production files**

COMING SOON: JAVASCRIPT SOURCE MAPS Source Maps is a bleeding-edge
browser feature that allows the debugger to map combined and minified files

to their original unminified counterparts. In browsers that support it (cur-
rently only Chrome), it makes debugging production code possible without

resorting to proxies or feature switches. To learn more, we recommend this
article from HTML5 Rocks: http://mng.bz/JENs.
At this point you’re now able to access development files when browsing the offending
publisher’s page. Now it’s time to roll up your sleeves and actually debug the issue. In
this next section, we’ll show you how to step through the code while monitoring your
program’s state to find and defeat nontrivial bugs.

# Config Env

## 2.1 COnfiguring your environment for Third-Party Development

As we discussed in chapter 1, the web browser treats scripts that communicate with
external servers differently than regular script files. To tackle these issues head-on, it’s
highly recommended that you simulate a cross-domain environment on your local
development machine. If you don’t, it’s extremely likely that you’ll run into unforeseen
problems when you deploy your code onto live servers. We’re guessing you don’t want
that to happen, so in this next section, we’ll guide you through such a configuration.

Remember: your goal is to serve a script from your servers and have it loaded on a
publisher’s website. So to re-create that scenario, you’ll need to have a test page to
simulate a publisher’s website and a web server to serve your script from. You’ll also
need both the test page and script files served from different domains.

## 2.1.1 Publisher Test Page

Let’s start with the publisher test page. This is the page where you’ll load your third-
party script and the environment in which that script will be executed. Any page will

do, but we’ll start with a bare-bones HTML file:

<!DOCTYPE html>
<html>
<head>
<title>Publisher Test Page</title>
</head>
<body>
<h1>Publisher Test Page</h1>
<!-- script include snippet here -->
</body>
</html>
Ironically, the part of this example you’ll want to focus on is the part that’s missing:
the script include snippet. This is the HTML snippet that you’ll give to publishers
that will load your third-party script on their page. We don’t have one yet, so keep it
blank for now; we’ll come back to this later.
Now, this example makes for a boring test page; it’s a blank page with a header. But
this is just to get you started. In an ideal world, your test page will be representative of

a typical page from your target audience. For example, if your product widget is tar-
geted primarily at bloggers, the test page should illustrate how the widget might

appear on a typical blog. You could even use a static copy of a known publisher’s web
page. The closer your test page reflects the environment in which your script will be
deployed, the fewer surprises you’ll face later.

## 2.1.2 The Web Server

In order to serve your test page and script files to the browser, you’ll need to have web
server software running on your local development machine. Even though you could
use your web browser to open these files directly from your filesystem, they’ll be
served using the file:// protocol, which doesn’t have a domain component. This will
make simulating a cross-domain environment nigh impossible. Save yourself a world
of pain and use a local web server.
If you’re not already using a local web server, don’t worry. Mac users will be pleased
to know that the Apache web server is installed by default on OS X. If you’re doing
your development on Windows, you’ll need to download and install Apache yourself.2

You can start Apache by running the following command in your terminal:
$ sudo apachectl start
By default, Apache on OS X (Mountain Lion) makes /Library/WebServer/
Documents/ available at http://localhost/. If you add your publisher test page named
test.html to this Documents folder, it should be available at http://localhost/test.html.
We’ll tweak these locations later in this tutorial.
ANOTHER SOLUTION: BUILT-IN SERVERS If dedicated web server software like
Apache feels a little heavy for you, most popular programming languages
have built-in web server support. For example, you can easily start a web
server in Python using the SimpleHTTPServer module, which comes installed
with every Python installation:
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
Ad hoc web servers like this one have fewer features than full server offerings
like Apache, but they’re great at serving code quickly.

## 2.1.3 Simulating Multiple Domains

At this point, you have a local web server running, and your files available via the local-
host hostname. Next, we’ll tackle the problem of needing different domains from

which to serve your test page and third-party script.
The good news is that you don’t need to spend money registering domain names.
You can just edit your operating system’s hosts file and create two entries that alias
your localhost. On OS X and Unix-based operating systems, you should find your host
settings in /etc/hosts. On Windows, try C:/windows/system32/drivers/etc/hosts.
Please note that you’ll probably need administrator access to edit your hosts file:
$ sudo vi /etc/hosts

Add the following two entries to the hosts file. The format is the same in both Win-
dows and Unix-based operating systems:

127.0.0.1 publisher.dev
127.0.0.1 widget.dev
After this change, you should be able to access your local files served through Apache
using http://publisher.dev and http://widget.dev. You’re probably aware that .dev
isn’t an actual top-level domain (TLD). We recommend using it over .com or .net so
that you don’t conflict with any actual live websites with the same address.3
There’s just one last step: configuring Apache to point the root of each domain to
a different directory. That way you can host your third-party scripts in one folder and
your test page files in another.

**Figure 2.2 Each Vistual Host points to a separate filder in your filesystem**

Open up Apache’s configuration file, httpd.conf. On OS X you’ll find this in /etc/
apache2. Add the following rules:
NameVirtualHost _:80
<VirtualHost _:80>
ServerName publisher.dev
DocumentRoot "/Users/username/project/publisher"
</VirtualHost>
<VirtualHost \*:80>
ServerName widget.dev
DocumentRoot "/Users/username/project/widget"
</VirtualHost>
When you’re done, restart Apache:
$ sudo apachectl restart
Now your files in the /publisher directory will be accessible at http://publisher.dev,
and your files in /widget at http://widget.dev (see figure 2.2). Organized!

This is just one way to set up your project. You’re free to choose your own host-
names, web server software, or folder location for your code. We’ll refer mostly to pro-
duction domain names throughout these chapters (such as camerastork.com), but

you can always substitute them for your local development domains for testing.

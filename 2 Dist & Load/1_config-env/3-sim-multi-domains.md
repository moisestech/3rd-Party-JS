# Sim Multi Domain

## **2.1.3 Simulating Multiple Domains**

- At this point, you have a local web server running, and your files available via the local-host hostname.
- Next, we’ll tackle the problem of needing different domains from which to serve your test page and third-party script.

- The good news is that you don’t need to spend money registering domain names.
- You can just edit your operating system’s hosts file and create two entries that alias your localhost.
- On OS X and Unix-based operating systems, you should find your host settings in /etc/hosts.

- On Windows, try C:/windows/system32/drivers/etc/hosts.
- Please note that you’ll probably need administrator access to edit your hosts file:

```bash
$ sudo vi /etc/hosts
```

- Add the following two entries to the hosts file.
- The format is the same in both Windows and Unix-based operating systems:

```bash
127.0.0.1 publisher.dev
127.0.0.1 widget.dev
```

- After this change, you should be able to access your local files served through Apache using http://publisher.dev and http://widget.dev.
- You’re probably aware that .dev isn’t an actual top-level domain (TLD).
- We recommend using it over .com or .net so that you don’t conflict with any actual live websites with the same address.3

- There’s just one last step: configuring Apache to point the root of each domain to a different directory.
- That way you can host your third-party scripts in one folder and your test page files in another.

**`Figure 2.2` Each Vistual Host points to a separate filder in your filesystem**

Open up Apache’s configuration file, httpd.conf. On OS X you’ll find this in /etc/
apache2. Add the following rules:

```bash
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
```

- Now your files in the /publisher directory will be accessible at http://publisher.dev, and your files in /widget at http://widget.dev (see figure 2.2). Organized!

- This is just one way to set up your project.
- You’re free to choose your own hostnames, web server software, or folder location for your code.
- We’ll refer mostly to production domain names throughout these chapters (such as camerastork.com), but you can always substitute them for your local development domains for testing.

---

#### From [[_1_config-env]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_1_config-env]: _1_config-env "Config Env"
[//end]: # "Autogenerated link references"

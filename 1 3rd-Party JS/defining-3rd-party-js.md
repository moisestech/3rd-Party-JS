# Defining

## **1.1 Defining Third-Party JavaScript**

In a typical software exchange, there are two parties. There’s the consumer, or first
party, who is operating the software. The second party is the provider or author of that
software.
On the web, you might think of the first party as a user who’s operating a web

browser. When they visit a web page, the browser makes a request from a content pro-
vider. That provider, the second party, transmits the web page’s HTML, images,

stylesheets, and scripts from their servers back to the user’s web browser.

For a particularly simple web exchange like this one, there might only be two par-
ties. But most website providers today also include content from other sources, or

third parties. As illustrated in figure 1.1, third parties might provide anything from
article content (Associated Press), to avatar hosting (Gravatar), to embedded videos
(YouTube). In the strictest sense, anything served to the client that’s provided by an
organization that’s not the website provider is considered to be third-party.

**Figure 1.1 Website today make use of a large number of third-party services.**

When you try to apply this definition to JavaScript, things become muddy. Many devel-
opers have differing opinions on what exactly constitutes third-party JavaScript. Some

classify it as any JavaScript code that providers don’t author themselves. This would
include popular libraries like jQuery and Backbone.js. It would also include any code
you copied and pasted from a programming solutions website like Stack Overflow.
Any and all code you didn’t write would come under this definition.
Others refer to third-party JavaScript as code that’s being served from third-party
servers, not under the control of the content provider. The argument is that code
hosted by content providers is under their control: content providers choose when
and where the code is served, they have the power to modify it, and they’re ultimately
responsible for its behavior. This differs from code served from separate third-party
servers, the contents of which can’t be modified by the provider, and can even change
without notice. The following listing shows an example content provider HTML page
that loads both local and externally hosted JavaScript files.

**Listing 1.1 Sample content provider web page loads both local and extenrnal scripts**

There’s no right or wrong answer; you can make an argument for both interpreta-
tions. But for the purposes of this book, we’re particularly interested in the latter defi-
nition. When we refer to third-party JavaScript, we mean code that is:

- Not authored by the content provider
- Served from external servers that aren’t controlled by the content provider
- Written with the intention that it’s to be executed as part of a content provider’s
  website

WHERE’S TYPE="TEXT/JAVASCRIPT"? You might have noticed that the

<script> tag declarations in this example don’t specify the type attribute.
For an “untyped” <script> tag, the default browser behavior is to treat the
contents as JavaScript, even in older browsers. In order to keep the examples
in this book as concise as possible, we’ve dropped the type attribute from
most of them.  

So far we’ve been looking at third-party scripts from the context of a content provider.
Let’s change perspectives. As developers of third-party JavaScript, we author scripts that
we intend to execute on a content provider’s website. In order to get our code onto
the provider’s website, we give them HTML code snippets to insert into their pages
that load JavaScript files from our servers (see figure 1.2). We aren’t affiliated with the
website provider; we’re merely loading scripts on their pages to provide them with
helpful libraries or useful self-contained applications.  

If you’re scratching your head, don’t worry. The easiest way to understand what
third-party scripts are is to see how they’re used in practice. In the next section, we’ll
go over some real-world examples of third-party scripts in the wild. If you don’t know
what they are by the time we’re finished, then our status as third-rate technical
authors will be cemented. Onward!

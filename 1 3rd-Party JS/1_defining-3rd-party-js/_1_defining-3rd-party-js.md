# Defining

### **1.1 Defining Third-Party JavaScript**

In a typical software exchange, there are two parties.

1. There’s **the consumer**, or first party, who is operating the software.
2. The second party is **the provider** or author of that software.

#### 1st Party Consumer

- On the web, you might think of the **first party as a user who’s operating a web browser**.
- When they visit a web page, **the browser makes a request from a content provider**.

#### 2nd Party Provider

- That provider, **the second party, transmits the web page’s HTML, images, stylesheets,
  and scripts from their servers back to the user’s web browser**.
- For a particularly simple web exchange like this one, there might only be two parties.

#### 3rd Party Provider

- But **most website providers today also include content from other sources, or third parties**.

- As illustrated in `figure 1.1`, third parties might provide anything

1. **from article content** (Associated Press)
2. to **avatar hosting** (Gravatar)
3. to **embedded videos** (YouTube).

### 3rd Party Definition

- In the strictest sense, **anything served to the client that’s provided by an organization that’s not the website provider is considered to be third-party**.

**`Figure 1.1` Website today make use of a large number of third-party services.**

**[IMAGE-MISSING]**

##### Developer Opinions

- When you try to apply this definition to JavaScript, things become muddy.
- Many developers have **differing opinions on what exactly constitutes third-party JavaScript**.
- Some classify it as any JavaScript code that providers don’t author themselves.
- This would include popular libraries like **jQuery and Backbone.js**.
- It would also include any code you copied and **pasted from a programming solutions website like Stack Overflow**.
- Any and all code you didn’t write would come under this definition.

#### 3rd Party Servers

- **Others refer to third-party JavaScript as code that’s being served from third-party servers**,
  - not under the control of the content provider.

##### Content Provider Argument

- The argument is that code hosted by content providers is under their control:
- **content providers choose when and where the code is served**
- they have the power to modify it,
- and they’re ultimately responsible for its behavior.

#### Differs from 3rd Party Servers

- This differs from code served from separate third-party servers,
  - the contents of which can’t be modified by the provider,
  - and can even change without notice.

**`Listing 1.1` Sample content provider web page loads both local and extenrnal scripts**

- The following listing shows an example content provider HTML page
  - that loads both local
  - and externally hosted JavaScript files.

### 3rd Party Book Definition

- There’s no right or wrong answer; you can make an argument for both interpretations.
- But for the purposes of this book, we’re particularly interested in the latter definition.
- When we refer to third-party JavaScript, we mean code that is:

1. Not authored by the content provider
2. Served from external servers that aren’t controlled by the content provider
3. Written with the intention that it’s to be executed as part of a content provider’s website.

**WHERE’S TYPE="TEXT/JAVASCRIPT"?**

- You might have noticed that the `<script>` **tag declarations in this example don’t specify the type attribute**.
- For an “untyped” `<script>` tag, **the default browser behavior is to treat the contents as JavaScript, even in older browsers**.

**In order to keep the examples in this book as concise as possible, we’ve dropped the type attribute from most of them.**

- So far we’ve been **looking at third-party scripts from the context of a content provider**.
- Let’s change perspectives. As developers of third-party JavaScript, we author scripts that we intend to execute on a content provider’s website.

**In order to get our code onto the provider’s website, we give them HTML code snippets to insert into their pages that load JavaScript files from our servers** (see `figure 1.2`).

**We aren’t affiliated with the website provider; we’re merely loading scripts on their pages to provide them with helpful libraries or useful self-contained applications.**

If you’re scratching your head, don’t worry.
**The easiest way to understand what third-party scripts are is to see how they’re used in practice.**

In the next section, we’ll go over some real-world examples of third-party scripts in the wild.
If you don’t know what they are by the time we’re finished, then our status as third-rate technical authors will be cemented.

Onward!

---

#### From [[_3rd-party-js]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3rd-party-js]: ../_3rd-party-js "💻 "
[//end]: # "Autogenerated link references"

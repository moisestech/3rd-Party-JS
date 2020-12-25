# easyXDM.Rpc

**5.3.3 Defining JSON-RPC interfaces using easyXDM.Rpc**

RPC (which stands for Remote Procedure Call) is a way for a program to cause a procedure or a function to execute in another address space without the programmer explicitly coding the communication details. Address space can be a memory cell, disk sector, network host, or any other entity. Throughout history, programmers—usually while working for big corporations—have tried to make it easier to call procedures on remote entities by designing complicated RPC protocols. They came up with things like the Common Object Request Broker Architecture (CORBA) and Simple Object Access Protocol (SOAP), protocols that at least one of these authors still sees in his nightmares. These ghosts of programming past aside, the concept of RPC is useful, especially when you’re developing a third-party application that communicates with remote documents and servers.

Let’s go back to the Camera Stork widget. If you recall, the widget needs to accept product reviews from users and transmit them to the Camera Stork servers. With a sim- ple message-passing system—using either window.postMessage or easyXDM.Sockeyou’ll have to come up with a protocol that both consumer and provider can use to sort messages from each other. You don’t want a message containing a password to be mistaken for a message that contains a user’s camera review. Since we’re all JavaScript developers here, you’ll most probably pick JSON as the default format for your protocol. So a message to post a review will look something like this:

```javascript
{
"commandId": 235711,
"command": "postReview",
"productId": 31415,
"userId": 31337,
"text": "This camera is great!"
}
```

The provider will receive this message, process it, and send it to the Camera Stork servers. Then, depending on the response from the servers, the provider will send a message to the consumer describing whether the operation was successful. Both consumer and provider need to maintain a shared state of commands—hence, the commandId parameter—or the consumer won’t know, upon receiving the response, which operation it belongs to. As you can see, this implementation might get hairy quickly. Luckily for us, easyXDM.Rpc takes care of all of this for us.

With easyXDM.Rpc you specify local and remote functions. Local functions are just as they sound—local functions implemented in the current window context. Remote functions, on the other hand, are actually implemented inside the iframed document.

The easyXDM.Rpc constructor takes two formal parameters: communication options (the same as for easyXDM.Socket from section 5.3.2) and lists of local and remote functions. Because you don’t actually implement remote functions in this context, the remote option takes a list of stubs that represent the remote functions.

The following listing shows how to create an instance of easyXDM.Rpc on the consumer side.

**Listing 5.8 Creating an instance of easyXDM.Rpc on the consumer**

```javascript
var rpc = new Stork.easyXDM.Rpc(
  {
    remote: "http://camerastork.com/xdm/provider/",
  },
  {
    local: {
      redrawReviews: function (reviews, onSuccess, onFailure) {
        try {
          Stork.redrawReviews(reviews);
          onSuccess();
        } catch (exc) {
          onFailure(exc.message);
        }
      },
      remote: {
        postReview: {},
        editReview: {},
      },
    },
  }
);
```

Functions can take as many parameters as they want, but the last two parameters
passed to a function will always be success and failure callback handlers. As you can
see from listing 5.8, every RPC function is expected to call those callbacks to indicate
its success or failure. As with easyXDM.Socket, you have to initialize easyXDM.Rpc for
the provider as well. Also note that you have to explicitly define all functions you plan
to call from the consumer. The next listing shows an example of creating an instance
of easyXDM.Rpc on the provider.

**Listing 5.9 Creating an instance of easyXDM.Rpc on the provider**

```javascript
var rpc = new Stork.easyXDM.Rpc(
  {},
  {
    local: {
      postReview: function (prodId, userId, text, onSuccess, onFailure) {
        try {
          var response = sendReview(prodId, userId, text);
          onSuccess(response);
        } catch (exc) {
          onFailure(exc.message);
        }
      },
      editReview: function (prodId, text, onSuccess, onFailure) {
        /_ ... _/;
      },
    },
    remote: {
      redrawReviews: {},
    },
  }
);
```

**Figure 5.5 illustrates creating both provider and consumer easyXDM.Rpc instances.**

Now that you’ve configured both consumer and provider RPC functions, it’s time to
call them. To do so, you simply call the desired remote function on the rpc instance
itself. The following listing demonstrates how to call the postReview remote function
declared in listing 5.9.

**Listing 5.10 Calling a remote function created with easyXDM.Rpc**

```javascript
function onSuccess() {
  console.log("Review posted successfully!");
}
function onFailure(message) {
  console.log("Review posting failed with message", message);
}
rpc.postReview(31415, 31337, "This camera is awesome!", onSuccess, onFailure);
```

Another neat thing about easyXDM is that it intelligently intercepts exceptions origi-
nated in RPC functions and passes their exception objects to the onFailure callback

function. That means that in the code from listing 5.9, you can omit the try/catch
block—easyXDM will handle the exception case automatically, as shown here.

**Listing 5.11 Creating an instance of easyXDM.Rpc on the provider**

```javascript
var rpc = new Stork.easyXDM.Rpc(
  {},
  {
    local: {
      postReview: function (prodId, userId, text, onSuccess, onFailure) {
        var response = sendReview(prodId, userId, text);
        onSuccess(response);
      },
      editReview: function (
        prodId,
        text,
        onSuccess,

        onFailure
      ) {
        /_ ... _/;
      },
    },
    remote: {
      updateReviews: {},
    },
  }
);
```

Behind the scenes, easyXDM uses the native JSON serializer that’s available in all
modern browsers. For older browsers, it falls back to the popular JSON2 library. It’s
also possible to specify your own serializer by setting a special serializer property when
creating an easyXDM.Rpc instance:

```javascript
var rpc = new Stork.easyXDM.Rpc(
  {},
  {
    local: {
      postReview: function () {
        /_ ... _/;
      },
    },
    remote: {},
    serializer: Stork.JSON,
  }
);
```

Note that your serializer object must have the same methods as the standardized window.JSON object. Specifically, it has to provide two methods: stringify to serialize JavaScript objects into portable strings, and parse to turn those strings back into JavaScript objects.

If you’re wondering why you’d ever want to change the default serializer, there’s a really good reason: sometimes you can’t trust the built-in JSON object on the publisher’s page. If the consumer serializes (or deserializes) strings differently than the provider (your iframed document), your application will probably break.4

You can spare yourself the heartbreak by loading your own JSON serializer on the consumer side (i.e., Stork.JSON), and specify it when creating easyXDM.Rpc instances. DESTROYING EASYXDM INSTANCES Most of the time, you’ll leave your iframe channels running until the user navigates off the page or closes their browser.

But sometimes you’ll need to destroy your easyXDM instances manually. Both easyXDM.Socket and easyXDM.Rpc provide a destroy method that cleans up everything for you, including removing your iframes from the DOM.

---

#### From [[_3_easy-xdm]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[_3_easy-xdm]: _3_easy-xdm "Easy XDM"
[//end]: # "Autogenerated link references"

---
layout: post
title: Consuming ASP.NET Web API with RestSharp
tags: aspnet mvc4 webapi
excerpt: So you've built your restful web services using the new ASP.NET Web API stack. But how do you consume those web services from other .NET applications? See how we built a RESTful API.
---
So you've built your restful web services using the new ASP.NET Web API stack.

But how do you consume those web services from other .NET applications? You could use the regular [HttpClient](http://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) framework class or leverage my favorite rest client library: [RestSharp](http://restsharp.org) from [Jon Sheehan](http://twitter.com/johnsheehan) (of Twilio fame).

### The Objective

We need a simple .NET library that can be used by Web Form or WinForm applications that wish to invoke our new ASP.NET Web API services in a RESTful manner. Most applications will require some sort of authentication and so we will need to pass user credentials when calling a web service method.

This is not as simple as it seems because as an ISV we have to support Windows authentication and handle those situations where the "corporate" IIS box does not allow PUT/DELETE requests to hit the server (kinda kills the notion of REST!).

### Connecting to a ASP.NET Web API Endpoint

RestSharp makes it dead simple to point towards your Web API (e.g. http://server/app/api/method).

So here is our endpoint in our ApiController:

![Rest method](/images/rest-method.png "Rest method")

And this is what is looks like from the client side that is consuming the web service endpoint:

![Rest call](/images/rest-call.png "Rest call")

It's simple. Allow any .NET application to invoke our web services with a couple of lines of code.

### Enter RestSharp

So what's the "glue" that allows any .NET application to talk to web services built using the ASP.NET Web API?

![Rest service](/images/rest-service.png "Rest service")

The above represents our "proxy" class that leverages RestSharp to call web service endpoints. Note how we specify the endpoint URL passing in our REST resource parameters (e.g. /api/types/123).

The RestRequest object is all we need to invoke any web service method.

### Accept & Content Type Headers

Once we have the RestRequest object we should allow the data format for inbound (Accept) and outbound (Content types to be specified as desired. Usually XML is what most people send down the wire but JSON works as well.

![Rest headers](/images/rest-headers.png "Rest headers")

### Http Verb Overrides (for not so RESTful situations)

RESTful means leveraging GET/POST/PUT/DELETE verbs to convey the type of action we want to perform. But what happens when you come up against "corporate IT" when they disallow PUT and DELETE verbs? Isn't your nice REST API dead in the water?

On the client side we can simply re-route PUT and DELETE verbs as POSTs:

![Rest re-route](/images/rest-reroute.png "Rest re-route")

But how would you know on the client side if the server disallows PUT or DELETE verbs? Just add some test methods (TestApiController?) as part of your implementation so clients can check to see if the server accepts PUTs and DELETEs.

The great thing with ASP.NET Web API is that we can simply plug our own logic into the processing pipeline to handle this situation. On the server we need to intercept requests using a simple HTTP Message Handler to detect and re-route certain POST requests (marked as X-HTTP-Method-Override) to what they should be (PUTs or DELETEs).

![Rest handler](/images/rest-handler.png "Rest handler")

We can then simply wire up the above handler into the ASP.NET Web API pipeline:

![Rest handler](/images/rest-handlerwire.png "Rest handler")

I couldn't stress how simple the ASP.NET guys have made things for ISV's like us who have to cater for all sorts of curve balls. Kudos to them!

### Authentication

Securing calls to web services is best practice if not mandatory in most enterprise scenarios. Thankfully RestSharp provides authenticator classes out of the box that can help you send user credentials with every REST call.

![Rest auth](/images/rest-auth.png "Rest auth")

On the server side you can again leverage HTTP Message Handlers to wire up your custom authentication checks into the ASP.NET Web API pipeline:

![Rest auth](/images/rest-authhandler.png "Rest auth")

Using the above approach we managed to move our proprietary REST API codebase onto ASP.NET Web API stack in a couple of days. Big shout to Jon Sheehan for his RestSharp library.

### Further Reading & Resources

Some useful resources:

* [RestSharp](http://restsharp.org) – simple REST and HTTP API Client for .NET
* Henrik F Nielsen is the ASP.NET Web API architect and has a [treasure trove](http://blogs.msdn.com/b/henrikn) of Web API stuff on his blog
* Fantastic summary of HTTP Message Handlers from [Mike Wasson](http://www.asp.net/web-api/overview/working-with-http/http-message-handlers)
* [Fiddler tool](http://fiddler2.com/fiddler2) – super helpful for testing out our your Web API stuff

Happy resting.


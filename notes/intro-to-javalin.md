# Javalin
Javalin is a lightweight web framework for Java and Kotlin. Javalin is primarily used to develop RESTful APIs, but is also useful for WebSockets and websites. Javalin is built to be fast, simple, and approachable. Javalin is built on top of Jetty, a webserver and servlet container. Javalin is designed to be used with an embedded Jetty webserver, but we can also create a Javalin instance that exposes the servlets to be used with other servers.


### Context
An HTTP request has two parts: request and response. An HTTP request is always sent from client to server, and the server should then send back a response. When building web services with servlets or some other solution we probably worked with request and response objects. In Javalin these two have been combined into a single object, the context object. Rather than having a request/response pair for every exchange, you have a single context object and Javalin handles the rest. 

The context object contains the underlying servlet request, servlet response, and a bunch of getters and setters. Mostly the getters operate on the request, while the setters operate exclusively on the response. This makes sense, we don't generally need to alter the request and the response generally doesn't contain anything we dont already know.


### Handlers
Handlers are blocks of code executed to process requests. There are several types of handler: before-, after-, and endpoint-handlers. Endpoint-handlers are similar to servlets, an http request comes in and is matched to an endpoint based on path and verb. Each request is matched to one handler which produces a response. The before- and after-handlers are also invoked for each request, before and after the endpoint handler. Note: after-handlers will still be invoked even when an exception occurs. Before- and after-handlers can be provided with a path, and they will only be invoked for requests to that path, or they can be global.
  
So, the workflow is: 
1. http request received
2. optional before-handler invoked
3. endpoint matched and endpoint-handler is invoked
4. optional after-handler is invoked
5. http response dispatched
  
  
There is a handler method for each http verb, and the method should be supplied with a path and a function. The handlers functions are commonly written as lambda expressions which take in a context object and manipulate that object. These can also be written as class methods which take in a context object and have a void return type. We use the context object to set up responses, rather than returning anything.



<BR><BR>See also:
 - [Javalin Documentation](https://javalin.io/documentation#getting-started)
 - [Web Applications with Javalin](https://leanpub.com/javalin/read)

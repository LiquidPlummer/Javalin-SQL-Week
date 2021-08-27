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
  
### Exception/Error Handlers
There are also handlers for exceptions and errors. Before-, after-, and endpoint-handlers can all throw exceptions. We can handle these exceptions with exception mapping. Use the Javalin instance `exception` method, which should be provided with an exception class it handles, and a function to handle it. Like the other handlers the function can be a lambda exression or a class method. The funciton should take in two parameters, the exception and the context objects.  
  
Error handling works the same way, but with http status codes rather than exceptions. An error handler takes in the error code and a function as parameters. The function should take in a context object as a parameter as well. An handler can throw an exception, which can be handled by an exception-handler that sets an appropriate status code, and an error-handler is invoked to handle that status code.  
  
Error handlers can also be provided with an optional parameter, a content type. This is the second parameter, and is a string that allows us to have different error-handlers handle different content types.


<BR><BR>See also:
 - [Javalin Documentation](https://javalin.io/documentation#getting-started)
 - [Web Applications with Javalin](https://leanpub.com/javalin/read)

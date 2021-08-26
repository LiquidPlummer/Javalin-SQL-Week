# HTTP - HyperText Transfer Protocol

HTTP is the protocol we use to browse the web. HTTP is a client-server protocol where the client must initiate communication. Every HTTP communication is made up of a client request, and a server response. HTTP is stateless, which means the server should be able to handle each request separately and only with the information provided in the request. The server should not have to maintain any state in order for any individual request to be processed.  
  
### HTTP Request
![http request](https://raw.githubusercontent.com/LiquidPlummer/Javalin-SQL-Week/main/images/http-request-image.png)
Each HTTP request is composed of:
 - Verb - the HTTP method to execute
 - URI - indicates the endpoint used to access the resource
 - HTTP Version - in order to be certain of communication protocol
 - Request Header - Metadata (info) about the request represented as key-value pairs
 - Request Body - message content (resource representation in REST)

### HTTP Response
![http response](https://raw.githubusercontent.com/LiquidPlummer/Javalin-SQL-Week/main/images/http-response-image.png)
Each HTTP response is composed of:
 - Response Code - code indicating the status of the request
 - Version - in order to be certain of communication protocol
 - Response Header - Metadata (info) about the response represented as key-value pairs
 - Response Body - message content (resource representation in REST)

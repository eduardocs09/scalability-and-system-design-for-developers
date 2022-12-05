# Client Server Architecture
Works on a _**request-response**_ model.

# Types of Clients
- Thin Client: just presentation, no business logic. For every action, a request is sent to the backend => 3 tiered of N tiered apps;
- Thick Client: holds part or all the business logic => two tiered apps, ex: single page applications;

# Communication Between the Client & the Server

- Communication happens over HTTP protocol;
- HTTP is a _**request-response**_ protocol that defines how informations is transmitted accross the web;
- HTTP is a stateless protocol, every HTTP request has everything needed for getting a response, every request is executed independently and has no knowledge of previous requests;
- Also in traditional HTTP, always a client has to send a request for getting a response from server.

# REST API
- API that adheres to the REST architectural constraints;
- Takes advantage of the HTTP methodologies;
- Enables servers to cache the response;
- Decouples client code from backend;
- Before REST-based APIs, backend was often tightly coupled with the client (JSP, ASP .NET etc);

### API Gateway
- A REST API can act as a gateway, a single entry point to the system;

# HTTP Push & Pull
### HTTP Pull
- Clients pulls data from the servers (_request_ to get a _response_);
- Clients don't know when there are updates, should they keep pulling?
- Excessive pulls are bad, they cost money and can bring down the server;

### HTTP Push
- Clients can send the first request and after that the server keeps pushing data to the client whenever they are avaiable;
- This saves money and resources;
- E.g: notifications, web sockets, AJAX long polling etc;

# HTTP Pull - Polling with AJAX
- Instead of sending a HTTP GET request, AJAX enables the client to fetch the updated data by automatically sending requests over and over at stipulated intervals;
- AJAX uses the _XMLHttpRequest_ object;
- This technique of requesting data at regular intervals is called polling;

# HTTP Push
- Every HTTP Pull request has a Time To Live (TTL - time out);
- Open connections consume resources and servers usually have limits of open connections they can handle;
- In case we know the response will take more than the TTL, we need a _**Persistent Connection**_, which facilitates HTTP Push and by enabling updates to be sent from the server to the client once the connection is first set (like Web sockets);
- To maintain Persistent Connections (browsers kill them after time), _**Heartbeat Interceptors**_ are used, basically empty request-response between client and server;
- Heartbeat Interceptors are resource-intensive;

# Web Sockets
- Preferred when we need a persistent **_bi-directional_** low latency data flow from client to server and back;
- E.g: messaging, chat applications, real-time applications, browser based multiplayer games;
- They run over TCP, not HTTP;

# AJAX - Long Polling
- Long polling is somewhere between Ajax and Websockets;
- Instead of immediately returning the response, the server holds it until it finds an update;
- Connections stay open a bit longer than compared to regular polling;
- Server doesn't return empty responses, if the connection breaks, the client needs to re-establish it;
- Makes less requests than regular polling;

# HTML5 Event Source API & Server Sent Events
- Instead of the client pulling data, the server automatically pushes data as events;
- Once the client has established the connection with an initial request, servers can initiate data transmission;
- Avoids unnecessary blank request-responses;
- Client needs to use the HTML% Event source API;
- Once the connection is set, the data flow is one-direction only, from server to client;
- SSE is ideal for real-time feed, like Twitter, displaying stock quotes;
- Good resource [here](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events);

# Streaming Over HTTP
- Ideal for streaming large data over HTTP by breaking into smaller chunks;
- A Persistent Connections is first established by the client, which gets events/responses from the server later with each chunk;
- Client and server need to agree on some stream settings, so they know when the stream begins and ends over HTTP;
- Resource [here](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API/Concepts);

# Client-Side vs. Server-Side Rendering
### Client-Side Rendering
- Browser has to do a lot of things to convert the response to a HTML page;
- Building the DOM tree, rendering, painting etc all takes time;

### Server-Side Rendering
- HTML is generated on the server to save rendering time on the client;
- Faster rendering;

### Use Cases
- Server-Side is perfect for delivering static content (such as Wordpress Blogs) and for SEO;
- Client-Side is better for modern dynamic websites and apps in which the content needs to be generated on the fly;
- Client-Side is also better for when the website takes a big load of users, as it doesn't put too much stress on the servers;
- A Hybrid approach can be used to leverage the best of both worlds, like having Server-Side for the home page and other static content, while using Client-Side for the dynamic pieces;
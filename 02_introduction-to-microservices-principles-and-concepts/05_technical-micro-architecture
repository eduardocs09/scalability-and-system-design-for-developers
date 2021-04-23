# Technical micro architecture
- Usually microservices are implemented with reactive technologies;

# Requirements
- Communication:
  - Microservices have to communicate with each other (UI Integration, REST, messaging);
  - Protocol to be used is a macro architecture decision;
- Operation:
  - Deployment
  - Configuration
  - Logs
  - Metrics
- New microservices:
  - Should be easy to create new microservices;
- Resilience:
  - Each microservice needs to be able to deal with the failure of other services;

# Reactive Programming
- When new data comes into the system, it is processed;
- A server application without Reactive programming processes a request in a thread;
  - If DB call is required, the thread blocks until the query result arrives;
  - A thread has tobe provided for each request;
- In reactive servers, applications only reacts to events:
  - It must not block while it's waiting for I/O, like an HTTP request;
  - Application don't wait for the result of a DB query, for example;
  - It suspends the processing and awaits for the next event, which can be the result from the DB or not;
- Event loop is a thread and processes one request at a time;
  - Instead of waiting for I/O, event processing is suspended;
- A single event loop can process many network connections;
- Processing of an event must not block the event loop for longer than necessary, otherwise other events will be stopped;

### Reactive Manifesto
- Responsive: systems responds as fast as possible;
- Resilient: systems remain available even if parts fail;
- Elastic: systems can deal with different levels of load;
- Asynchronous communication: systems communicate through async messages or events;

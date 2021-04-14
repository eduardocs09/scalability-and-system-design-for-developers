# Event Driven Architecture
- AKA as _Reactive_ or _Non-blocking_ architecure, very popular in the modern web apps development;
- _NodeJS_ and other similar technologies/frameworks are very popular for this kind of architecture because:
  - They're _non-blocking_ in nature and are built for modern high _IO_ scalable applications;
  - Capable of handling large number of concurrent connections with minimal resources;
  - Enable us to write code without worrying much about handling _multi-threads_, _thread lock_, _out of memory_ issues and so on;
- Modern applications need a fully async model to scale;
- _Event driven_ means just reacting to events occuring regularly;
- Code is written to react to events as opposed to sequentially moving through the lines regularly;
- The sequence of events occurring over a period of time is called _stream of events_;
- _Event driven architecture_ is all about processing _async data streams_;

# Web Hooks
- Like _call-backs_, it "calls" the client when new information is available;
- _Event-based mechanism_, it enables communication between two services without a middleware;
- Consumers register an HTTP endpoint with the service with an unique API key;
- Whenever new information is available, the server fires an HTTP event to the registered endpoints (from consumers), notifying them of new updates;

# Shared Nothing Architecture
- Architecture in which the modules share nothing, not even RAM or disk, to eliminate all single points of failure;

# Hexagonal Architecture
- AKA _Ports & Adapters Architecture_;
- Cosists of three main components:
  - Ports;
  - Adapters;
  - Domain (**Hello DDD**);
- Goal is to make the components independent, loosely coupled and easy to test;
- The _Domain_ is the core, the businees logic;
- _Ports_ are the interface with the outside world, like APIs, DB connectores, message queue clients and so on;
- _Adapters_ convert the data received/sent from _Ports_ into/from the _Domain_;  

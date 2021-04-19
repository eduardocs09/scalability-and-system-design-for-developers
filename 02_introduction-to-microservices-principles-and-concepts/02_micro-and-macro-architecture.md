# Introduction
- Microservices provide better decoupling;
- Architecture has to ensure that the services can work together;
- Micro architecture relates to decision made individually for each service;
- Macro architecture relates to decisions made at global level and that affect all services;

# DDD and Bounded Contexts
- DDD offers a collection of patterns for the domain model of a system;
- Simplest design consists of multiple specialized domain models tha are valid only in a certain context;
- Communication between bounded contexts can be achieved through domain events;

# Strategic Design and Common Patterns
- Dividing a system into different bounded contexts is part of the strategic design;

# Architecture Decisions
- Since microservices provide technological isolation, we can extend the concept of micro and macro architecture to technical decisions;
- Different databases can be chosen for different microservices, but that requires more effort to operate them;
- Even if the DB technology is the same for all services, the instances and schema must not be shared;

### Typical macro architecture decisions
- Communication protocols (REST, messaging, etc);
- Authentication (unacceptable for a user to re-authenticate with every microservice);
- Integration testing (microservices should be tested together, so they should run together in an integration test);

### Typical micro architecture decisions
- Authorization (what a user is allowed to do);
- Testing;
- CD pipelines;

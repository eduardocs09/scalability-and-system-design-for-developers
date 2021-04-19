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

# Operations
- When decisions are made at the level of macro architecture (e.g how to log, monitor, provide configuration parameters), they standardize only the technologies;
  - Each service has different contexts, so might require monitoring on different metrics, loading different configurations etc;
- Macro architecture rules can be checked with tests;

# Macro architecture decisions
- Specifying only a few points in the macro architecture helps focusing;
- Rules should be minimal (e.g OK to define monitoring technology, but not how to monitor);
- Macro rules need to be enforced, but should also be in the self-interest of the micro services teams (not applying them means a service can't be deployed or supported in production);
- Macro architecture can evolve from restrictive rules at the beginning of a project (e.g single tech stack, reduces learning effort and operating costs) to something more flexible;

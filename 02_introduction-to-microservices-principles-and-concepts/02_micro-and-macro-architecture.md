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

# Organizational Aspects
### Who makes macro architecture decisions?
- Commitee of representative team members:
  - Can become too large;
  - Team members are often focused on their own services and work and don't care about macro architecture;
- Independent commitee:
  - Should serve and support the teams, not restrict them;
  - Should moderate decisions, not enforce them;
### Testing Conformance
- Deploying a microservice and checking its log output and metrics;
- Checks if the service conforms with the defined macro architecture;
- Called _**black box test**_, checks the behaviour of the micro service from the outside;
- Benefits are:
  - does not limit the free choice of technologies;
  - does not enforce unnecessary rules and standards;

# Independent Systems Architecture (ISA) Principles
- Term for a collection of fundamental principles for microservices;
- Macro and micro architecture are very important for the goal of building software out of independent systems;
- Minimal macro architecture leaves lots of freedom to the level of the micro architecture, making the systems independent;

### 1 - System MUST be divided into modules
- Division into modules that offer _interfaces_;
- Modules may not depend directly on the implementation details of another module;

### 2 - System MUST have 2 levels of architectural decisions
- Macro and micro architecture;

### 3 - Modules MUST be separate processes/containers/VMs
- Modules must be separate to maximize independence;
- In a monolith, most decisions are on the macro level (one language for all, frameworks, etc);
- To make more decisions on the micro level, modules must be in separate containers;

### 4 - Integration and communication options MUST be standardized
- Limited and standardized options for integrations and communication between services;
  - Synchronous, asynchronous, integration on the UI, protocols, etc;

### 5 - Metadata MUST be standardized
- Metadata that is transferred between services;
- Examples:
  - Authentication tokens;
  - Trace IDs;

### 6 - Modules MUST have their own CD pipelines
- Tests are part of the pipeline, so each module's tests have to be independent as well;

### 7 - Operations SHOULD be standardized
- Standardization is the only way to handle a large number of microservices;
- Involves:
  - Configuration;
  - Deployment;
  - Log analysis;
  - Tracing;
  - Monitoring;
  - Alarms;

### 8 - Interface SHOULD enforce standards for operations, communication and integration
- Standards should be defined only on the interface level;

### 9 - Modules MUST be resilient
- A module may not fail when other modules are unavailable or when facing communication problems;
- They must be able to shut down without losing data or state;
- They must be movable to other environments;

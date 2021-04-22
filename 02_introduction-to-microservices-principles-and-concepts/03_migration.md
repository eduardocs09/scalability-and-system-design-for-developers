# Migration from Monolith to Microservices
- Most common case for introducing microservices;

# Reasons for migrating
- Microservices offer a fresh start;
- Code not maintenable;
- Outdated technologies;
- Robutsness;
- Independend scaling;
- Speed of development and deployment;

# Typical Migration Strategies
- Integration with the legacy system should be asynchronous to decouple domain logic;
- Further integration can be done at the UI level;
- Avoid synchronous communication to avoid coupling the new service with the legacy in terms of availability;
- Synchronous communications could be necessary to guarantee that last changes are visible in other systems/modules;
  - If not, asynchronous and _**replication**_ are used;
- Log in should be integrated and happen only once for the new microservices and the legacy code;
- A separate database means _**data replication**_, it's the only way to each service implement its own data model;
  - Changes to the data can be communicated via events;
- Replication should be done in one direction, there should be only one source of each type of event;

### Black box migration
- Usually the legacy code is hard to understand;
- No reverse engineering of what it does;
- Separate the legacy code into bounded contexts (not implement, just analyzis);
- Extrat one bounded context into new service;
- To minimize risk, start with an unimportant and low load bounded context;
- Migrate bounded context that will need to be changed a lot;

### Extreme migration
- Changes are not allowed in the legacy codebase;
- For each required change, a new microservice needs to be extracted first;
- Could result in services that implement only part of a bounded context;

### Strangler pattern
- Gradually migrate parts of the monolith that are going through major changes, so that migrations are worhtwhile;
- Micro services increasingly strangle the legacy system until there's nothing left;

# Alternative Strategies
- Reliability goal:
  - First improve reliability on interfaces to external systems or databases;
  - The split the sytem into services;
- Layers:
  - Doing one layer at a time, like starting with the UI and then moving the other layers to the new service;
- Copy/change:
  - Copies the legacy code;
  - In one copy, develop one part of the sytem further while removing the other;
  - Does the inverse in second copy;
  - Still uses old code :(
  - Database remains shared :(
  - Tech debt is carried over :(
  - Does not represent a fresh start;
  - Only for exceptional cases;

# Build, Operation and Organization
- Dealing with the first microservice will require extra effort since the infrastructure behind it doesn't exist;
- Recommended to build the infrastructure as early as possible to reduce risks of migration;
- Changes affecting both the legacy system and microservices are hard to implement and deploy;
- Integration tests of the microservice against production and development versions of the legacy codebase;
  - Ensure what's being developed will work when legacy is deployed;
- Deployment of legacy and microservices can be coordinated:
  - Both are rolled out together;
  - More changes at the same time, so more risk;
  - Harder to roll out;
  - Hard to implement without downtime;
- Organizational aspects:
  - To enable independent teams, reorganization is required;  

# Monoliths
- Self-contained software, in which all layers of the app are in the same codebase;
- Simpler to build, test and deploy;
- When starting with a monolith to later break it into microservices, there are some trade-offs:
  - Re-writing stuff has its costs;
  - Striping down things in a tightly coupled architecture & re-writing demands a lot of resources and time;
### Advantages
- Simplicity;
### Disavantages
- CD can be a pain (minor change in a specific layer requires the re-deployment of the whole thing);
- Regression Testing needs to be done after each change/deploy, which is complex;
- Single point of failure;
- Tricky and dangerous to leverage multiple technologies;
### When to Pick
- When the use cases and requirements are simple;
- Working on business proof of concept;
- App is expected to handle a limited amount of traffic;

# Microservices
- Every service has its own database, there are no single points of failure;
- Facilitates the management, development, testing and deployment for large teams;
- Easier to scale;

### Advantages
- No single point of failure;
- Leverage multiple technologies;
- CI/CD;

### Disavantages
- Complex to manage, we need distributed tracing, more nodes to monitor etc;
- May need dedicated team to manage these services;
- No strong consistency, instead we get **_Eventual consistency_**;

### When to Pick
- Complex use cases and expecting big traffic;

# Trade-Offs - Monolith vs. Microservices

### Fault isolation
- In Microservices, it's easier to isolate faults and debug them;
- Also, if the service goes down, other services are up and running;

### Dev Team Autonomy
- In Monoliths, too many developers working in the same codebase can affect productivity;
- Harder to push isolated changes to production in a Monolith;
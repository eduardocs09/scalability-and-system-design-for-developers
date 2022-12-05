# Microservices
- Independently deployable modules;

# Advantages
- Scale development:
  - Easier for large teams to work together;
  - Each service just needs to offer an interface for others;
  - Each service's internal structure does not matter;
  - Independent deployments from the rest of the application;
- Replacing legacy code:
  - Each new service can replace parts of the old codebase;
- Sustainable development:
  - Microservice-based architecture promises the sytem remains maintenable in the long run;
  - When a microservice can no longer be maintained, it can be rewritten;
- CD:
  - Each service has its own CD pipeline, which facilitates CD;
  - CD pipelines are a lot faster, since each unit being deployed is smaller;
  - Deployment of a microservice is less riskier than the deployment of a monolith;
- Independent scaling;
- Isolated in respect to failures, which improves robutsness;

# Tradeoffs, Prioritizing Advantages & Levels
- The reasons for adopting a microservices architecture should focus on increasing the business value;
- Microservices can be divided by:
  - Domain isolation;
  - Technical reasons;

# Challenges
- Requires more operations effort than running a monolith:
  - More units to deploy and monitor;
- Requires effort to make deployments independent:
  - Changes to interfaces need to maintain backwards compatibility;
- Changes that affect multiple services are harder to implement than in a monolith;
- Increases latency, since services communicate over the network;

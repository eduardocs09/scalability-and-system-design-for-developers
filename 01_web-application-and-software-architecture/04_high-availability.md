# Scalability
- Application's ability to handle & withstand increased workload without sacrificing the latency;
- Latency is the amount of time a system takes to respond a user's request;
- Latency is divided in two parts: **_Network Latency_** and **_Application Latency_**;
- High latency is a big factor in customers bouncing off a website;

# Types Of Scalability
### Vertical Scaling
- AKA scale up;
- Adding more power/resources to your server, like memory and CPUs;
- Ideally it's the first step when traffic starts to build upon your app;
- It's simple since it doesn't require code refactoring or complex configurations;
- But it's limited, we can only get to a certain point doing it;

### Horizontal Scaling
- AKA scale out;
- Adding more hardware to the existing hardware resource pool, like more machines;
- There's no limit to how much we can scale horizontally;
- Has the ability to dynamically scale in real-time as traffic increases or decreases;

### Cloud Elasticity
- Cloud computing is very popular because we can do both dynamically;
- Adding or removing nodes as needed saves money and it's called **_Cloud Elasticity_**;
- Having multiple node also helps maintaining the website up even if a few nodes crash, this is called **_High Availability_**;

### Vertical vs. Horizontal
- Vertical scaling is simpler and doesn't require code changes or complex infrastructure configuration, requires less monitoring and management;
- Vertical scaling is risky, servers are powerful but few;
- Horizontal scaling needs the code to be stateless, so no static instances stored in memory;
- Horizontal scaling has no limits;

### Use cases
- Uutility or internal tool with minimum traffic doesn't need to be distributed, single server with vertical scaling should be enough;
- Public facing apps with high traffic require high availability, so horizontal scaling and deployed in the cloud would be ideal;

# Bottlenecks that Hurt Scalability
- A single database in a single server or not shardeed/partioned is a bottleneck, doesn't matter if the application server nodes scale if the DB doesn't;
- Poor architecture that doesn't allow asynchronous processes or background tasks;
- Lack of proper caching;
- Inneficient load balancer configuration;
- Business logic in the DB, no need for the unnecessary load there;
- Not picking the right DB: relational is the way if we need transactions and/or strong consistency, but NoSQL handles better scalability on the fly;
- Inneficient code;

# Improving and Testing the Scalability
- A performance optimized application can take more load with less resources;

### Tuning the Performance
- **Profile** the application through application and code profilers;
- Cache whenever we can and wisely, trying to avoid the DB, using a **write-through** cache;
- CDN's;
- Compress data;
- Avoid unnecessary client-server requests;

### Testing the Scalability
- Simulated traffic is routed to the system;
- Run several load and stress tests on the application;
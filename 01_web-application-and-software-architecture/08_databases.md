# Relational Databases
- Saves structured data containing relationships in a normalized manner;
  - Normalized data means one entity is only in one place/table and not spread;
- Easy to maintain consistency;
- Supports _ACID_ (Atomicity, Consistency, Isolation and Durability) transactions;
  - The operation can either fully succeed or completely fail, it goes from state A to B and no in the middle, maintaining the consistency for both states;

### When to Pick
- If data consistency and transactions are super important;
- Larger community to support and knowledgeable;
- Data is strongly relational;

# NoSQL Databases
- Built for high frequency read & writes (like Twitter, real-time news or sports apps, online games etc);
- Easier to scale than Relational DBs:
  - Relational DBs must be sharded or replicated, which is hard;
  - NoSQL can just add new server nodes;
- Designed to run intelligently (with minimal human intervention) on clusters;
- Sacrifice consistency and ACID Transactions to scale horizontally over a cluster and across data centers;
- Data is more _Eventually Consistent_;

### Advantages
- Easy to learn;
- No strict schemas, so more flexible and easier to change and spread things around;
- The ability to add nodes on the fly enable it to handle more concurrent traffic & big amount of data with minimal latency;

### Disavantages
- Risk of inconsistency;
- No support for ACID Transactions;

### When to Pick
- Need to scale fast;
- Large number of read-writes and large amount of data;
- When not sure about data more and things are expected to change at rapid pace;
- Don't require transactions and _Eventual Consistency_ is OK to have, instead of _Strong Consistency_;
- Good for _data analytics_ use cases, when we have a massive influx of data;

# SQL vs. NoSQL Performance
- NoSQL is **NOT** more performant than SQL;
- Performance is more related to how we design the system with the technology;
- A well designed SQL database will always be more performant than a poorly designed NoSQL;

# Polyglot Persistence
- Using different technologies to fulfil different persistence requirements;
- Complex to make all the different technologies to work together;

# Multi-Model Database
- Ability to use different data models in a single database system;
- Reduce the operational complexity of managing multiple persistence technologies in a single service;

# Eventual Consistency
- Consistency models which enables the data store to be highly available;
- AKA _**optimistic replication**_, and is key to distributed systems;
- Data takes time to get replicated across different data centers, so the data may be "incorrect" at some time;

# Strong Consistency
- All the server nodes across the world should contain the same value of an entity at any point in time;
- Nodes need to be locked down when being updated;
- Queueing all the requests is a good way to keep a system _**Strongly Consistent**_;

# CAP Theorem
- Consistency, Availability, Partition Tolerance, AKA **_CAP_**;
- Partition Tolerance means Fault Tolerance, the system is tolerant of failures or partitions;
- In case of a network failure, we need to choose between **Availability** or **Consistency**;
- Picking between them depends on use case and business requirements;

# Document Oriented Database
- Main types of NoSQL;
- Data is stored in a semi-structured way, in a JSON-like format;
- Typical use cases:
  - Real-time feeds;
  - Live sports apps;
  - Product catalogues;
  - Inventory Management;
  - User comments;
  - Multiplayer games;

# Graph Database
- Store nodes/vertices and edges in the form of relationships;
- Compared to regular SQL, they're good for visualization and low latency;
- Relationships are not calculated at the query time (as opposed to joins);
- Typical use cases:
  - Social, knowledge, network graphs;
  - Recommendation engines;
  - Fraud analysis app;

# Key Value Database
- Simple key-value storage to quickly store and fetch data with minimum latency;
- Fetches data based on the key, O(1) time complexity;
- The key feature is the super fast data fetch;
- No query language required;
- Typical use cases:
  - Caching;
  - Persisting user state;
  - Persisting user session;
  - Manage real-time data;
  - Queues;

# Time Series Database
- Optimized for tracking & persisting time series data;
  - Data containing data points associated with the occurence of an event with respect to time
  - Generaly ingested from IoT devices, self-driving vehicles, industry sensors, social networks, stock market;
- Good for studying data, tracking the behaviour of the system, learning about user patterns & how things change over time;
- Primarily used for running analytics, deducing conclusions and making future decisions based on analytics;

# Wide Column Database
- Primarily used for _Big Data_;
- Good for analytical use cases, they have high performance and scalable architecture;
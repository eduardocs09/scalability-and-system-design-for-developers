# Step by step guide
- Unclear open ended questions;

### Requirements clarifications
- Not ONE correct answer;
- Ask questions to clarify what features we need to design;
- E.g questions for designing Twitter:
  - Users can post tweets and follow people?
  - Design the user's timeline?
  - Tweet can contain photos and videos?
  - BE only or FE also?
  - Search tweets?
  - Display hot trending topics?
  - Push notification for new/important tweets?

### Back-of-the-envelope estimation
- Estimate the scale of the system we're designing;
- Helps later on when we focus on scaling, partitioning, load balancing, caching;
- E.g:
  - Scale expected from the system (number of tweets, number of tweet views, number of timeline generations per sec)?
  - How much storage will we need (different storage requirements if users can have photos and videos)?
  - What network bandwith usage are we expecting (crucial in deciding how we will manage traffic and balance load)?

### System interface definition
- Define what APIs/endpoints are expected from the system;

### Defining the data model
- Define data model for main entities to clarify how data will flow between different systems components;
- It will help guide data partitioning and management later;
- E.g:
  - User: userId, name, email, DoB, creationData, lastLogin, etc;
  - Twwet: tweetId, content, tweetLocation, numberOfLikes, timeStamp, etc;
  - UserFollow: userId1, userId2;
  - FavouriteTweets: userId, tweetId, timeStamp;
- With the data, we can think of what kind of database we want, what kind of storage we want for photos and videos;

### High-level design
- Draw diagram with 5-6 boxes representing the core components of the system;
- Identify enough components that are needed to solve the actual problem from e2e;
- E.g:
  - Clients;
  - Load balancer;
  - Application Servers;
  - Databases;
  - File storage;

### Detailed design
- Based on interviewers feedback, dig deeper into two or three major components;
- Present different approaches, present pros and cons and explain why we prefer one over the other;
- E.g:
  - Since we'll be storing massive data, should we partition to distribute it to multiple databases?
  - Should all of o single user's data be stored in on the same database?
  - How to handle hot users that tweet a lot or follow lots of people?
  - Since users'timeline will contain the most recent tweets, should we try to store our data so it is optimized for scanning the latest tweets?
  - How much and at which layer should we introduce caching to speed things up?
  - What components need better load balancing?

### Identifying and resolving bottlenecks
- Try to find possible bottlenecks and discuss approaches to mitigate them;
- E.g:
  - Single points of failure? How to solve them?
  - Enough replicas of the data so that we can still serve our users if we lose a few servers?
  - Enough copies of different services running such that a few failures will not cause a systems' shut down;
  - How are we monitoring the system?

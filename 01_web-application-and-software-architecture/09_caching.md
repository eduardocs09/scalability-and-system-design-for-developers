# Caching
- Ensures low latency and high throughput;
- It's basically copying frequently accessed data from a database which is disk-based and store it in RAM;
- We can cache dynamic data:
  - Data that changes often, has an TTL; After that, the data is invalidated and the newly updated data is stored in it;
- We can cache static data:
  - Things like images, font files, CSS and so on.
  - Can be cached on the client-side or on CDNs;
- Most common usage is database caching, which helps aliviate the stress on the database by intercepting the requests being routed to the database for data;
- Can be used at multiple places;
- Aside the static content in the client and database chace, we can look for patterns and find frequently accessed content accros the application that can be cached;
- Cache is part of HTTP;
- Can be used for cross-module communication in a microservices architecture;

# Caching Strategies
### Cache Aside
- Most common strategy;
- Works along the database, trying to reduce hits on it;
- Data is lazy-loaded in the cache:
  - When we look for some data we try to find it in the cache first;
  - If present, just return it to the user;
  - If not, fetch from the DB, saves it in the cache and then return it;
- Works best with read-heavy loadsm like data which is not freqeuently updated, like a user profile;
- Data is written directly to the database, which means data in the cache can get inconsistent;
- Data has a TTL;
### Read-Through
- Similar to _Cache Aside_, but in this case the data is always consistent with the DB;
- Caching library or framework takes responsability to maintain the consistency;
### Write-Through
- Before the data is written to the DB, the cache is updated with it;
- High consistency between cache and DB;
- Adds a little latency to write operations;
- Works well for write-heavy workloads, like online games;
- Generally used with other caching strategies to achieve optimized performance;
### Write-Back
- Data is directly written to the cache instead of the database;
- After some delay based on business logic, the cache persists the data to the DB;
- Used to reduce the frequency of database writes when dealing with a heavy number of writes in the application, to cut down the load & associated costs;
- Risky in case the cache fails, so data can be lost;
- Generally used together with other caching strategies;
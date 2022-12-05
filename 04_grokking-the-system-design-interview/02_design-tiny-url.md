# URL Shortening service like TinyUrl
- Difficulty: easy;

# Why we need them
- Saves ton of space to type, display, tweet, store in dbs;
- Used to track individual links to analyse audience, measure ad campaigns' performance, hide affiliate links;

# Requirements and goals of the system
- Functional:
  - Given a url, generate a shorter and unique alias of it;
  - When users access the short link, we redirect them to the original link;
  - Users should optionally be able to pick custom short link;
  - Links expire after a timespan, which should be customizable by the user;
- Non functional:
  - System should be highly available (if the service is down, all urls redirections will fail);
  - Url redirection should happen in real-time with minimal latency;
  - Shortened links should not be guessable/predictable;
- Extended:
  - Analytics (how many times used);
  - Accessible through RESTful APIs by other services;

# Capacity Estimation
- Read-heavy, way more Urls redirection than new Url shortenings;
  - 100:1 ratio is assumed between read and write;
- Traffic estimates:
  - 500M new Url shortenings per month, that gives us 50B redirections per month;
- Queries per second:
  - New urls shortenings per second => 500M / (30d * 24h * 3600s) =~ 200 urls/s;
  - 100:1 => 20K url redirections per second; 
- Storage estimates:
  - Assume we keep old links for 5 years;
  - For 500M new Urls every month => 500M * 5y * 12m = 30B links;
  - If each stored object is 500 bytes => 30B * 500 bytes = 15 TB;
- Bandwidth estimates:
  - 200 new Urls per second: 200 * 500 bytes = 100 KB/s;
  - Read requests, 20K url redirections per second: 20K * 500 bytes = 10 MB/s;
- Memory estimates:
  - How much memory will we need if we decide to cache the frequently accessed?
  - If we follow the 80-20 rule, we'd have: 20K requests/s * 3600s * 24h = 1.7B requests/day => 20% * 1.7B * 500 bytes = 170GB
 
# APIs
- createURL(api_dev_key, original_url, custom_alias=None, user_name=None, expire_date=None)
  - api_dev_key: dev api key for the registered account, can be used to throttle users based on their allocated quota;
  - original_url: url to be shortened;
  - custom_alias: optional custom key;
  - user_name: optional to be used in the encoding ??
  - expire_date: optional custom expiration date;
  - returns: string with the shortened url;
- deleteURL(api_dev_key, url_key);
- We can prevent malicious users from taking our system down by limiting them via their api_dev_key;

# Database design
- We need to store billions of records;
- Each object is small;
- No relationship between records;
- Service is read-heavy;
- Since data is not heavily relational, we can use a NoSQL store, which would be easier to scale and help us save the billions of records;

### Schema
- Url table:
  - Hash (PK);
  - OriginalUrl;
  - CreationDate;
  - ExpirationDate;
  - UserId;
- User: ??
  - Name;
  - Email;
  - CreationDate;
  - LastLogin;

# Basic System Design and Algorithm
- We need to generate the short and unique key;
- What should be its length?

### Option 1 - encode the actual URL
- We need to answer:
  - What kind of encryption to use?
  - How to shorten the result of the encryption?
  - Strategies to make sure 2 people with the same link don't get the same key?
  
### Option 2 - offline key generation
- Pre-generate unique keys in a Key Generating System and store them in a key-Db;
- New requests to shorten urls will take one key and use it;
- Faster and simpler aproach than encoding, and we won't need to worry about duplications or collisions;
- Concurrency can cause issues (concurrent users getting same key):
  - We need to mark used keys;
  - We can have separate tables for used and non-used keys;
  - We can keep some keys in memory to quickly provide them;
  - It must lock on the data structure giving keys;
- Key-DB size in KGS:
  - 6 chars per key * 68.7B (unique base64 encoding 6 letters possibilities) = 412 GB 
- KGS and Key-DB are single points of failure:
  - We need standby replica of server and database
- Each application server can cache some keys from KGS;
- Key lookup:
  - if found, return HTTP 302 (redirect) with the original url in the response;
  - if not, return 404;

### Final High Level system
- Clients;
- Application server;
- Database;
- KGS;
- Key-DB;

# Data partitioning and replication
- We need partition to scale a db to store billions of records;
- We need a partition shceme to divide the records into different db servers;

### Range based partition
- We can store urls in separate partitions based on the first hash key's;
  - We can have separate tables for used and non-used keys; letter;
- It can lead to unbalanced DB servers (if we have too many url keys with a same letter);

### Hash-based partition
- Hash part of the data and the hashing function distributes urls to different partitions;
- It can still generate overloaded partitions, but can be solved with Consistent Hashing;

# Cache
- Cache urls frequently used;
- We can use it before hitting the Application servers' DB;
- If we start with 20% of faily traffic, we'd need 170GB of memory;
- Modern day servers can have 256GB of memory, so we can fit all the cache in one machine;
- Alternatively, we can store all of them in many smaller servers;

### Cache eviction policy
- When new frequently used keys need to be added and the cache is full, we need to remove some;
- Least Recently Used (LRU) would be a good policy;
- Every time we have a cache miss, we can pass the new entry to all cache replicas;

# Load Balancers
- We can LB in 3 places:
  - Between clients and application servers;
  - Between apllication servers and cache servers;
  - Between application servers and database servers;
- We can start with simple Round robin approach to distribute requests between application servers;
  - Will distribute requests equally;
  - Will take offline servers out of rotation;
  - Does not take load into consideration => might need to create more elaborate solution later based on traffic;

# DB Clean up
- We should not check the DB frequently for expired links to avoid unnecessary load on the DB;
- We can mix lazy clean up with periodic cleanup:
  - If a user request an expired one, we delete it before returnng error for the user;
  - Dedicated service checks for expired links in less frequent intervals and clean the found ones;

# Telemetry
- How many times a short URL has been visited?
- What are the user locations?
- If it's part of the operational DB, how would that be handled if it gets slammed with multiple concurrent requests?

# Security and Permissions
- Can users create private Urls?
- Can they restrict Urls to group of users?
- We can have separate table/collection to store those, in which the link hash would be the key;

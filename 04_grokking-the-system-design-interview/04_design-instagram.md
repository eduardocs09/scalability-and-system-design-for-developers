# Photo-sharing service like Instagram
- Users can upload photos to share them with other users;
- Difficulty: medium;

# What is the service
- User can share photos and videos publicly or privately;
- Share with other social netwroking platforms, like Facebook, Twitter and so on;
- Our goal is to design a simpler version:
  - Users can share photos and follow other users;
  - News feed for each user will consist of top photos of all the people the user follows;

# Requirements and goals of the system
- Functional:
  - Users can upload, download and view photos;
  - User can search photos and videos by title;
  - Users can follo other users;
  - System should generate and display a user's News Feed consisting of top photos from all the people the user follows;
- Non-functional:
  - Highly available;
  - Accepted latency of the system is 200ms for News Feed generation;
  - Consistency can take a hit (prioritizing availability), users don't need to photos as soon as they're uploaded;
  - System should be highly reliable, any uploaded photo or video should never be lost;
- Not in scope:
  - Tags, searching by tags, commenting on photos, tagging users in photos, who to follow reccomendations;

# Design considerations
- System would be read-heavy, so the focus should be on building a system that can retrieve photos quickly;
- Users can upload as many photos as they like, so we need efficient management of storage;
- Low latency is expected while viewing photos;
- Data should be 100% reliable, photos and videos are never lost;

# Capacity Estimation and Constraints
- Total 500M users with 1M daily active users;
- 2M new photos every day, 23 new photos per second;
- Average photo file size: 200 KB;
- Space required for 1 day of photos: 2M * 200 KB = 400 GB;
- Total space required for 10 years: 400 GB * 365d * 10y = 1425 TB;

# High Level System Design
- We nee to support 2 main scenarios:
  - Upload photos;
  - View/search photos;
- We need some object storage server to store photos and some database server to store metadata information for the photos;
- Main components so far:
  - Clients uploading images;
  - Clients viewing and searching images;
  - Image Hosting System;
  - Image Storage;
  - Image Metadata;

# Database schema
- We need to store data about the users, their uploaded photos and people they follow;
- User:
  - userId int;
  - name varchar(20);
  - email varchar(32);
  - dateOfBirth datetime;
  - creationDate datetime;
  - lastLogin datetime;
- UserFollow:
  - userId1 int;
  - userId2 int;
- Photo:
  - photoId int;
  - userId int;
  - photoPath varchar(256);
  - photoLatitude int;
  - photoLongitude int;
  - userLatitude int;
  - userLongitude int;
  - creationDate datetime;
- In the Photo table, we'll need an index on (PhotoId ?? and CreationDate) to fetch recent photos first => I'd do userId instead of photoId;
- Because of the few relational contraints, a straightforward approach would be using a relational db, but those com with challenges to scale them;
- For the distributed file storage, we can use something like S3 or Hadoop Distributed File System;
- An alternative to the relational db would be to store the photo metadata to a NoSQL db;

# Data size estimation
- Estimate how much storage we need for each table and then total for 10 years;
- Assume int and datetime are 4 bytes;
- Users:
  - userId (4 bytes) + name (20 bytes) + email (32 bytes) + dateOfBirth (4 bytes) + creationDate (4 bytes) + lastLogin (4 bytes) = 68 bytes;
  - 500M users: 500M * 68 bytes = 32 GB;
- Photos:
  - photoiD (4 bytes) + userID (4 bytes) + photoPath (256 bytes) + photoLatitude (4 bytes) + photoLongitude(4 bytes) + userLatitude (4 bytes) + userLongitude (4 bytes) + creationDate (4 bytes) = 284 bytes;
  - 2M photos per day => 2M * 284 bytes = 0.5 GB per day;
  - 10 years => 365 * 0.5GB * 10 = 1.88TB;
- UserFollow:
  - Each row takes 8 bytes;
  - 500M users and each user follow in average 500 other users => 500M * 500 * 8 bytes = 1.82TB;
- 10 years for all tables => 32GB + 1.88TB + 1.82TB = 3.7TB;

# Component Design
- Photo uploads/wirtes can be slow;
- Reads will be faster, especially if they're served from cache;
- If we have too many uploads, they can get all the available connections and users won't be able to read;
- To avoid this bottleneck, we can separate writes/reads into separate services and have dedicated servers for reads and for writes;
- Updated main components:
  - Clients uploading;
  - Upload service;
  - Clients reading;
  - Reading service;
  - Image storage;
  - Image metadata db;

# Reliability and redundancy
- Losing files is not acceptable;
- We need copies of each file so that if the storage server dies, we can retrieve a copy from other server;
- Same principle applies to other components:
  - Backup of the image metadata db;  
  - Multiple replicas of upload/reading services running;
  - If only one instance of the service is required to run at a point (not enough load to scale into more copies), we can keep another copy not serving traffic as standby in case of problems;
- Redundancy will remove single points of failure and provide backups in case of crisis;

# Data sharding

### Partitioning on userId
- We can all photos from an user in the same shard;
- Issues:
  - How do we handle hot users with millions of followers?
  - Some users have way more photos than others, distribution won't be even;
  - Storing all photos from user into single shard may cause all of those to become unavailable if we have issues with that shard, or higher latency if its serving higher load;

### Partitioning on photoId
- Generate unique ids for photos first and then assign a shard. to it;
- We need a dedicated service/db to generate those;
- The dedicated service/db will be a single point of failure, so we'd need to address that like in tinyUrl;

# Ranking and news feed generation
- We need to fetch the latest, most popular and relevant photos of the people th user follows;
- We'd need to fetch all users a user follows, fetch each one's 100 (if we wanna generate 100 photos for the feed) last photos and givem them to the ranking algorithm;
- Might run into latency issues, since we need to query multiple databases, perform sorting, merging, ranking;
- An alternative is to pre generate the feed:
  - Dedicated servers are continuously generating users news feeds and storing them in a user news feed table

### Sending news feed to users
- Pull:
  - clients pull the feed content from servers at regular interval, or manually;
  - new data might not be shown;
  - most pull requests will result in no updates;
- Push:
  - servers push new data to users as soon as it's available;
  - clients could maintain a long poll request for this;
- Hybrid:
  - mixes approach depending on type of users, how many followers they have and so on; 

# News feed creation with sharded data
- To quickly retrieve the latest photos, we can have creation date as part of the photo id;
- Since we'll have a primary index on it, it'll be fast to retrieve;
- The id can have two parts:
  - time (in epoch seconds - first entry is 0 s)
  - increment for every second;

# Cache and load balancing
- We'd need a massive-scale photo delivery system to serve globally distributed users;
- Service needs to push its content closer to the users using geographically distributed photo cache servers and CDNs;
- We can have cache for the metadata databases to cache hot database rows;
- We can use the 80-20 rule to cache the 20% most read of the daily read volume for photos;

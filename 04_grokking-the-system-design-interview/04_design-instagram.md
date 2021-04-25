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

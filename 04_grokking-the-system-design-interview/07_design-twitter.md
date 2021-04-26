# Twitter
- Social networking service in each user post tweets, follow other people and favourite tweets;
- 140 chars posts;
- Registered users can read and post, unregistered can only read;
- Difficulty: medium;

# Requirements and goals
- Functional:
  - Post tweets;
  - User sshould be able to follow other users;
  - User can favourite tweets;
  - Create and display a user's timeline consisting of top tweets from all the people the user follows;
  - Tweets can contain photos and videos;
- Non-functional:
  - Highly available;
  - Latency of max 200ms for timeline generation;
  - Consistency can take a hit to favour high availability;
- Extended:
  - Searching for tweets;
  - Replying to tweets;
  - Trending topics;
  - Tagging users;
  - Notification;
  - Who to follow?
  - Moments;

# Capacity Estimation and Contraints
- 1B users with 200M daily active users;
- 100M new Tweets per day;
- Each user follows on average 200 people;
- Favourites per day?
  - Each user favourites 5 tweets per day;
  - 200 M * 5 favs = 1 B favs per day;
- Tweet views per day?
  - Each user visits their timeline 2 twice a day and 5 other people's pages;
  - In each page, they see 20 tweets;
  - 200 M * ((2 + 5) * 20) = 28 B views per day;
- Storage estimate
  - 140 chars at 2 bytes for char (no compression);
  - 30 bytes for metadata;
  - 100 M * (280 + 30) bytes = 30 GB per day; 
  - Every 5th tweet has a photo, every 10th has a video;
  - Photo is 200 KB, video is 2 MB;
  - 100 M/5 * 200 KB + 100 M/10 * 2 MB = 24 TB per day;
- Bandwidth estimates:
  - Users see 28 B tweets per day;
  - They see every photo and every third video;
  - 28 B * 280 byte / 86400s = 93 MB/s of text;
  - 28 B/5 * 200 KB / 86400s = 13 GB/s of photos;
  - 28 B/10/3 * 2 MB / 86400s = 22GB/s of videos;
  - Total: 35 GB/s; 

# API
- tweet(api_dev_key, tweet_data, tweet_location, user_location, media_ids)

# High level system design
- Must efficiently store the 100 M new tweets per day, 1150 tweets per second;
- Must be able to read 28 B tweets per day, so 325 K tweets per second;
- Traffic will be distributed unevenly, might have to deal with 1 M requests per second;
- Main components so far:
  - Clients;
  - Load balancer;
  - Cluster of Application servers;
  - Databases;
  - File storage;

# Database schema:
- Tweet;
- User;
- UserFollow;
- Favorite;

# Data Sharding
- Huge number of new tweets and our read load is very high;
- Need to distribute data into multiple machines;
- Sharding based on userId:
  - All of the user's data is in one server;
  - If user becomes hot, server could get a very high load;
  - Difficult to maintain even distribution of growing user data;
  - Would need to repartition/redistribute the data or use consistent hashing;
- Sharding based on tweetId:
  - Hash function maps each tweet to a server;
  - Searching for a users'tweets will need to query all servers;
  - Can result in high latency;
- Sharding on tweet creation time:
  - Makes easier to retrieve top tweets;
  - Traffic will not be distributed;
- Sharding in a tweetId and tweet creation date time combination;

# Caching
- Cache servers to cache hot tweets and users;
- Cache replacement policy?
  - LRU seems a good fit;
- Smart cache?
  - 80-20 rule;

# Detailed Components
![Screen Shot 2021-04-26 at 7 07 39 PM](https://user-images.githubusercontent.com/17072046/116161945-bf670c80-a6c2-11eb-9c7d-47b08c4b0a90.png)

# Timeline Generation
- Check Facebook's approach;

# Replication and Fault Tolerance
- System is read-heavy and we need H.A., so we can have secondary db servers for each partition;
- Secondary servers can be used for read traffic only, all writes go to the primary server and then it'll be replicated;

# Load balancing
- Between clients and application servers;
- Between application servers and database replication servers;
- Between aggregation servers and cache servers;

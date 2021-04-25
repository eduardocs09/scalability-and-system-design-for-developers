# Facebook feed
- Constantly updating list of stories;
- Contains status updates,photos, videos, links, app activity, likes from people and so on;
- Difficulty: hard;

# Requirements and goals
- Functional:
  - Newsfeed generated based on the posts from people, pages and groups a user follows;
  - User has many friends and follow large number of pages/groups;
  - Feeds may contain images, videos, or just text;
  - Support appending new posts as they arrive to the newsfeed for all active users;
- Non-functional:
  - Should generate any user's newsfeed in real-time - maximum latency should not be over 2s;
  - Post should not take more than 5s to make it to a user's feed;

# Capacity estimation
- Average has 300 friends and follows 200 pages;
- Traffic estimates:
  - 300M daily active users with each user fetching their timeline 5x/day;
  - 1.5B newsfedd requests per day, 17500 requests per second;
- Storage estimates:
  - 500 posts in every users feed kep in memory for a quick fetch;
  - average 1 KB per post;
  - 500 KB per user in memory;
  - 500 KB * 300M = 150 TB of data in memory;
  - If a server can hold 100GB in memory, we'd need 1500 machines to keep 500 posts in memory for every user;

# APIs
- getUserFeed(api_dev_key, user_id, since_id, count, max_id, exclude_replies)
  - api_dev_key: api developer key, which can be used to throttle users based on their allocated quota;
  - user_id
  - since_id: optional, return results with id higher (more recent) than this one;
  - count: optional, specifies the number of feed items to retrieve up to maximum of 200;
  - max_id: optional, return results with id less (older) than this one
  - exclude_replies: optional, prevent replies from appearing

# Database design
- Main objects are User, Entity (page or group) andFeedItem;
- User can follow entities and become friends with users;
- Both users and entities can post feed items, which can contain text, images or videos;
- Each feeditem will have a userId and optionally an entityId;
- User:
  - userId int;
  - name varchar(20);
  - email varchar(32);
  - dateOfBirth datetime;
  - creationDate datetime;
  - lastLogin datetime;
- Entity:
  - entityId int;
  - name varchar(20);
  - type tinyint;
  - description varchar(512);
  - creationDate datetime;
  - category smallint;
  - phone varchar(12);
  - email varchar(20);
- UserFollow:
  - userId int;
  - entityOrFriendId int;
  - type tinyint;
- FeedItem:
  - feedItemId int;
  - userId int;
  - contents varchar(256);
  - entityId int;
  - locationLatitude int;
  - locationLongitude int;
  - creationDate datetime;
  - numLikes int;
- FeedMedia:
  - feedItemId int;
  - mediaId int;
- Media:
  - mediaId int;
  - type smallint;
  - description varchar(256);
  - path varchar(256);
  - locationLatitude int;
  - locationLongitude int;
  - creationDate datetime;

# High Level System Design
- Problem can be divided into 2 main parts:
  - Feed generation;
  - Feed publishing;

### Feed generation
- Whenever our system receives a request to generate the feed, we do the following:
  - Retrieve ids of users and entities that the user follows;
  - Retrieve latest, most popular and relevant posts for those ids;
  - Rank these posts based on their relevance to the requesting user;
  - Store feed in cache and return top (say 20) posts to be rendered on user's feed;
  - On the FE, when the user reaches the end of current feed, next 20 are fetched from the server/cache (no need to recalculate);
- What about new posts?
  - If user is online, we can have a periodic routine that fetch, rank and add the new posts to the feed;

### Feed publishing
- When the user loads the newsfeed page, a request to pull the feed items will be made;
- Reachin the end of current feed can pull more;
- For newer posts, server can notify the client and then it can pull the updates, or the server can push;

### Main components
- Web server: deliver the static content and maintain connection with the user;
- Application server: execute logics of storing posts and fetching feeds;
- Metadata db and cache: store metadata about users, pages, groups;
- Posts db and cache: store metadata about posts and their contents;
- Videos and photo storage and cache: blob storage to store media;
- Newsfeed generation service: gather and rank all the relevant posts for a user and store in cache. Also deal with updates;
- Feed notification service: notify user about new items;
![Screen Shot 2021-04-25 at 4 57 02 PM](https://user-images.githubusercontent.com/17072046/116009372-43e45d00-a5e7-11eb-8a48-a1eaf885777e.png)

# Detailed Component Design

### Feed generation

### Feed publishing

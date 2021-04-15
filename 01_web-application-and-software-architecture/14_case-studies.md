# Web-based Mapping Service like Google Maps
- Started as a desktop-based software written in C++ and evolved into what it is today;
- Read-heavy, not write-heavy, meaning that the data can be largely cached;

### Database
- Uses _spatial data_, which is data represented by geometric information like points, lines, polygons;
- Data also has alphanumeric stuff, like longitudes and latitudes;
- There are dedicated databases for _spatial data_, but popular ones like MySQL, MongoDB and other also support it;
- Coordinates of places are persisted in the DB;
- When they're fetched, numbers are converted into a map image;
- Traffic spikes usually during peak hours, festivals or major events in a city, so needs horizontal scalability;
- Would choose a NoSQL DB because map data doesn't contain many relationships and NoSQL is natively horizontally scalable;
- Relational DB could be used and the system would scale well in the read-heavy scenario because of caching, but real-time features (e.g traffic patterns and updates) with lots of updates, would make it harder;

### Architecture
- Client-server model;

### Backend Technology
- Needs to be performant to convert _spatial data_ into map images;
- No persistent connection needed for the map part, so good options are Java, Scala, Go, Python etc;

### Monolith vs Microservice
- Many different features:
  - Gaming API;
  - Direction API;
  - Geocoding API;
  - so on...
- To isolate the load in each service and keep the core map search working, microservices are the best approach;

### Server-Side Rendering
- After matching the user search to a specific location, we need to convert the coordinates to a map image;
- Doing that on the server, enables us to cache the rendered image and reuse it for future requests, since the data is kinda of static;
- Entire map is broken into tiles to enable the system to generate only the part the user wants to engage with;
- Map can even be created in advance and cached before being used;

### Real-time Features
- Need persistent connection;
- Real-time features are very demanding in terms of resource, should be implemented only when required;

# Baseball Game Ticket Booking Web App

### Database
- Selling tickets is critical and needs to be ACID transactional, stronglyconsistent;
- Must choose a relational DB;

### Concurrency
- Needs to handle high number of concurrent connections (large number of people trying to get tickets once they're available);
- Might have more users than tickets, so possible solutions to handle this are:
  - Queue: queue the requests;
  - DB Locks:  ensures consistency and that at one point, only one transaction has access to a resource;
 
### Caching
- Many users just browsing for information, prices etc and not actually buying;
- Caching reduces load on the DB;

### UI
- It's a CRUD-based app, so no need for persistent connection;

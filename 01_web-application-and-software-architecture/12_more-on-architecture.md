# Event Driven Architecture
- AKA as _Reactive_ or _Non-blocking_ architecure, very popular in the modern web apps development;
- _NodeJS_ and other similar technologies/frameworks are very popular for this kind of architecture because:
  - They're _non-blocking_ in nature and are built for modern high _IO_ scalable applications;
  - Capable of handling large number of concurrent connections with minimal resources;
  - Enable us to write code without worrying much about handling _multi-threads_, _thread lock_, _out of memory_ issues and so on;
- Modern applications need a fully async model to scale;
- _Event driven_ means just reacting to events occuring regularly;
- Code is written to react to events as opposed to sequentially moving through the lines regularly;
- The sequence of events occurring over a period of time is called _stream of events_;
- _Event driven architecture_ is all about processing _async data streams_;

# Web Hooks
- Like _call-backs_, it "calls" the client when new information is available;
- _Event-based mechanism_, it enables communication between two services without a middleware;
- Consumers register an HTTP endpoint with the service with an unique API key;
- Whenever new information is available, the server fires an HTTP event to the registered endpoints (from consumers), notifying them of new updates;

# Shared Nothing Architecture
- Architecture in which the modules share nothing, not even RAM or disk, to eliminate all single points of failure;

# Hexagonal Architecture
- AKA _Ports & Adapters Architecture_;
- Cosists of three main components:
  - Ports;
  - Adapters;
  - Domain (**Hello DDD**);
- Goal is to make the components independent, loosely coupled and easy to test;
- The _Domain_ is the core, the businees logic;
- _Ports_ are the interface with the outside world, like APIs, DB connectores, message queue clients and so on;
- _Adapters_ convert the data received/sent from _Ports_ into/from the _Domain_;  

# Peer to Peer Architecture
- Base of blockchain and torrents;
- Network in which computers known as _nodes_ communicate with each other without a _central server_;
- No single point of failure;
- All computer have equal rights;
- A computer can _seed_ and _leech_ at the same time;
- Each computer acts as client & server;
- Data is eschanged over TCP IP just like in HTTP in a client-server model;
- P2P design has an overlay network which enables users to connect directly;
- Nodes/Peers are indexed & discoverable in this overlay network;
- Big files are divided into chunks, which are transfered in a non-sequential order;
- Chunks can be downloaded by multiple _leechers_, which start _seeding_ it and making it more available, AKA _Segmented P2P file transfer_;

### Downsides of a Centralized Architecture
- Communication is not really secure, sercer has access to all the data;
- Central server going down takes service offline;
- Organization which owns the central server can maintain or not your data according to their interests;

### Advantages of a P2P Network
- Nobody has control over your data or the power to delete it;
- Unlimited size file sharing;
- Can scale infinitely without putting the load on a single entity or node;
- Offers more availability;

### Types of P2P Networks
- Unstructured:
  - Nodes keep connecting with each other randomly;
  - Simply connect and grow the network;
  - No indexing of nodes;
  - To search the data, every node is scanned;
- Structured:
  - Network holds proper indexing of the nodes or the topology;
  - Easier to find the data;
  - Implementes a _distributed hash table_ to index nodes;
  - e.g Bittorrent;
- Hybrid:
  - Used by most blockchain startups;
  - Cherry-picks the good parts of P2P and client-server model;
  - Usually have a central indexing server that helps peers find each other;

# Decentralized Social Networks
- Servers spread across the globe, hosted by common individuals;
- No control over the network, everyone has equal say;
- User data layer is separate, so you don't lose it if you quit the network, AKA _Bring Your Own Data_;
- Data is not shared with organizations, just who you decide to share it with;
- Economic compensation for sharing your computer to host the network;

# Federated Architecture
- Extension of the _descentralized architecture_;
- Has entities called _pods_, to which a larger of _nodes_ subscribe to;
- _Pods_ facilitate node discovery;
- Several _pods_ in the network that are linked to each other and share information with each other;
- _Pods_ can be hosted by individuals;

# Load Balancing
- It's the distribution of heavy traffic across the servers running in a cluster based on several different algorithms;
- It's facilitaded by **_load balancers_**, making them a vital component in an application's architecture;
- Load balancers act as a single point of contact for all client requests;
- Can be set to distribute traffic towards any component, e.g backend server, databases, message queues and so on;
- This distributes the load uniformly across all tiers of the application;
- To route requests to the running servers, load balancers must know of their health and status, so they perform health checks on the servers;
- Machines up and running are known as **_in-service_**, otherwise are **_out of service_**;

# DNS
- Every machine online and part of the world wide web has an IP adress;
- _Domain name system_, AKA _**DNS**_, maps IP adresses to a domain name;
- Typing a url in the browser and hitting enter is an event know as **_DNS querying_**;

### How it works
- DNS Infrasctructure is composed of 4 main components:
  - _DNS Recursive Nameserver_, aka _**DNS Resolver**_;
  - _Root Nameserver_;
  - _Top-Level Domain Nameserver_, aka _TLD nameserver_;
  - _Authoritative Nameserver_;
- Browser sends a reques to the _DNS Resolver_; 
  - _DNS Resolvers_ are managed by ISPs (data centers conatining clusters of servers optimized to process _DNS queries_);
- _DNS Resolver_ forward the request to the _Root nameserver_ to get the address of the _TLD nameserver_;
- _Root nameserver_ returns the address of the _TLD nameserver_ (e.g the TLD for _a**mazon.com**_ is _**.com)**_;
- _DNS Resolver_ sends a request to the _TLD nameserver_ to fetch details on the domain name, including the IP address of the domain name server, aka _Authoritative nameserver_;
  - _Authoritative nameserver_ is the final end server in the **lookup chain**, owned by the owner of the domain name;
- _DNS Resolver_ send a request to the _Authoritative nameserver_, which responds with the website's IP address;
- _DNS Resolver_ caches the information and forwards it to the client;
- Browser sends a request to the IP address of the website to fetch the data;
- Browser also caches the information with a TTL (time to live);

![DNS.jpeg](https://boostnote.io/api/teams/mPlvzW3BY/files/62db065d5998e16557c734d5de3c33d7ad1323642d1d25f464a5265ade7f04b0-DNS.jpeg)

# DNS Load Balancing
- Big systems are deployed across multiple datacenters in different geographical locations across the globe;
- Load balancing is used to distribute traffic to those data centers;
- We can set up DNS Load balancing on the _*Authoritative nameserver*_;
- Largely used by companies to distribute traffic across multiple data centers, in which the application run;
- The _Authoritative nameserver_ returns a list of IP adresses, changing the order for every request;
- The list (not just the first) is sent to the client, so that it can use a different one other than the first, if that one doesn't respond;

### Limitations of DNS Load Balancing
- Doesn't take into account the existing load on the servers, content they hold, request processing time, their status and so on;
- Since these IP addresses are cached, they can be cached and fetch in a moment in which that machine is out of service;
- Despite the limitations, is preferred by companies because it's an easy and not expensive way to start load balancing the traffic;

# Load Balancing Methods
- DNS Load Balancing;
- Hardware-based Load Balancing;
- Software-based Load Balancing;

### Hardware Load Balancers
- Highly performant hardware that sits in front of the application servers and distribute the load based on the number of existing open connections, CPU usage and so on;
- Expensive to set up, require maintenance (and people with skills to do it) & regular updates;
- When using these, we may need to overprovision them to deal with peak traffic;
- Primarily picked because of the top-notch performance;

### Software Load Balancers
- Can be installed in commodity hardware and VMs;
- More cost-effective and more flexible;
- Easier to be upgraded and provisioned in comparison to the hardware ones;
- There are **_Load Balancers as a Service_**, aka LBaaS;
- Way more advanced than DNS Load Balancers and can consider many parameters, such as CPU usage, HTTP Headers, load on the network and servers etc;
- They perform health-checks on the servers to keep a list of inservice machines;

#### Load Balacing Algorithms
- Round Robin: simple list, send them sequentially;
- Weighted Round Robin: takes server's capacity into consideration and assign weights, redirecting more traffic to biggest servers;
- Least Connections: traffic is directed to the machine with least open connections (can take CPU usage in consideration or not), which comes in handy when the server has long opened connections (like persistent connections in a game);
- Random;
- Hash: source IP and the request URL are hashed, ensuring that that client will always get the same server (if the server holds the client's/user's data in a cache, this would improve the UX);



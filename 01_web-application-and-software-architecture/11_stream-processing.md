# Use Cases for Data Stream Processing
- Large-scale use of IoT devices;
- Backend systems needed to manage the massive amount of data, gather the meaningful and purge the garbage;
- Track service efficiency (e.g health check from millions of IoT devices worldwide);

# Data Ingestion
- Process of colelcting data streamin-ing from several sources and making it ready to be processed by the system;
- Data is routed to different components/layers through the _data pipeline_, algorithms are run on it and it's archived;

# Layers of Data Processing Setup
- Data Collection & Preparation;
  - Data streams in from different sources in different formats, it needs to _standardized_;
- Data Processing;
  - e.g: classified into different flows;
- Data Analytics;
  - execution of different models, like predictive modelling, statistical analytics, text analytics;
- Data Visualization;
  - data is presented to stake-holders, usually in a web-based dashboard like _Kibana_
- Data Storage;
- Data Security;
  - ensures secure movement of data all along;

![layers](https://user-images.githubusercontent.com/17072046/114637155-adcf3f00-9c96-11eb-9afb-05fcc816d284.jpeg)

# Ways to Ingest Data
- Depends on the use cases and requirements, but the two primary ways are:
  - Real-time;
  - Batches;
- Real-time: medical IoT, financial data etc;
- Batches: read trends over time;

# Challenges in Data Ingestion
- Slow Process;
  - Data has to be transformed to common format;
  - Data has to be authenticated;
  - ETLs are not that effective anymore;
  - What about reak-time? It's actually not that accurate, since accuracy comes from time spent studying the data;
- Complex & Expensive;
  - Resourse-intensive;
  - Usually takes dedicated teams to pull it off;
  - Data may come from external sources, so it can change which requires updated to the data collection;
- Moving Data is Risky;
  - Possibility of breach;

# Data Ingestion Use Cases
- Moving Big Data into Hadoop (or similar) for analysis and stuff;
- Streaming Data from DBs to Elasticsearch;
- Log processing to study the behaviour of the system;
- Handling live information;

# Data Pipelines
- Ensure smooth flow of data;
- Can apply filters and logic to data;
- Parallel process via multiple streams;
- Avoid corruption;

# Distributed Data Processing
- Diverging large amounts of data to several nodes for parallel processing;
- Because of distributed nodes and parallel execution, it's _**sclable & highly available**_;
- Data is made redundant and replicated to avoid data loss;

### Distributed Data PRocessing Technologies
- MapReduce - Apache Hadoop;
  - Manages the distributed data processing in several machines in a cluster, running work in parallel, the communication and data transfer;
  - Used by Facebook (BigData) and Twitter (Analytics);
  - Preferred for batch processing;
- Apache Spark;
  - High performance for both batch and real-time in-stream processing;
  - Has a cluster manager and distributed data storage, manager handles communication between nodes and storage is for big data; 
- Apache Storm;
  -  Used for massive amounts of streaming data;
  -  Real-time analytics, machine learning etc;
- Apache Kafka;
  - Distributed stream processing and messaging platform;
  - Storage layer involves distributed pub/sub message queue, which helps read & write streams of data like a messaging system;
  - Used to develop real-time features, like notifications, streams of massive amounts of data, monitoring metrics, messaging and log aggregation;

# Lambda Architecture
- Leverages both batch & real-time streaming processing to tackle latency issues from regular batching;
- Joins results from batching and real-time before presenting to the user;
- Has different streaming layers that converges into one at the end;

### Layers
- Batch Layer;
  - take time to process massive amounts of data and generates high accuracy results;
- Speed Layers;
  - analytics is run over a small piece of data to provide quick acess to insights, but results are not that accurate; 
- Serving Layer;
  - combines the results from both layers;

# Kappa Architecture
- Single streaming pipeline, flowing data from real-time and batch processing through it;
- Reduced complexity, since doesn't need to handle separate layers;
- Not alternative to _Lambda_, used for different use cases;
- It's preferred if the batch and streaming analytics are faily identical. Lambda is preferred if they're not;

### Layers
- Speed;
  - Processing layer; 
- Serving;
  - Final layer;

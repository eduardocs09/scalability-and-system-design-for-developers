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

![layers](https://www.educative.io/api/collection/6064040858091520/6411938009448448/page/4569452697878528/image/5250130053693440)

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

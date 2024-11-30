# Kinesis
- Makes it easy to collect, process and analyze streaming data in real time
- Ingest real-time data such as application logs, metrics, website clickstreams, IoT telemetry data...

## Kinesis Data Stream (KDS)
- Real-time data streaming service
- Used to ingest data in real time directly from source
- Throughput
  - Publishing: 1MB/sec per shard or 1000 msg/sec per shard
  - Consuming:
    - 2MB/sec per shard (throughput shared between all consumers)
    - Enhanced Fanout: 2MB/sec per shard per consumer (dedicated throughput for each consumer)
- Throughput scales with shards (manual scaling)
- Not Serverless
- Billing per shard (provisioned)
- Data Retention: 1 day (default) to 365 days
- A record consists of a partition key (used to partition data coming from multiple publishers) and data blob (max 1MB)
- Records will be ordered in each shard
- Producers use SDK, Kinesis Producer Library (KPL) or Kinesis Agent to publish records
- Consumers use SDK or Kinesis Client Library (KCL) to consume the records
- Once data is inserted in Kinesis, it can’t be modified or deleted (immutability)
- Ability to reprocess (replay) data

<img src=./images/kds.jpg width="500"/>

## Kinesis Data Firehose (KDF)
- Used to load streaming data into a target location
- Writes data in batches efficiently (near real time)
- Can ingest data in real time directly from source
- Greater the batch size, higher the write efficiency
- Auto-scaling
- Serverless
- Destinations:
  - AWS: Redshift, S3, ElasticSearch
  - 3rd party: Splunk, MongoDB, DataDog, NewRelic, etc.
  - Custom: send to any HTTP endpoint
- Pay for data going through Firehose (no provisioning)
- Supports custom data transformation using Lambda functions
- Max record size: 1MB
- No replay capability

<img src=./images/kdf.jpg width="500"/>

## Kinesis Data Analytics (KDA)
### For SQL Application
- Real time analytics on **Kinesis Data Streams & Firehouse** using SQL
- Can add referance data from S3 to enrich streaming data
- Fully managed, serverless
- Pay for the data processed (no provisioning)
- Use cases:
  - Time-series analytics
  - Real-time dashboards
  - Real-time metrics

<img src=./images/kda.png width="500"/>

### For Apache Flink
- Use Flink (Java, Scala, or SQL) to process and analyze streaming data
- Read from Kinesis Data Streams and Amazon MSK

<img src=./images/apacheflink.png width="500"/>

- Run any Apache Flink application on a managed cluster on AWS

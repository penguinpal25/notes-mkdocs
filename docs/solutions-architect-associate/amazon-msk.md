# Amazon MSK
- Amazon Managed Streaming for Apache Kafka (Amazon MSK)
- Fully managed Apache Kafka on AWS
- Allows to create, update, and delete clusters
- Creates and manages Kafka broker nodes and Zookeeper nodes for you
- Deploy the MSK cluster in your VPC, multi-AZ (upto 3 for HA)
- Automatic recovery from common apache kafka failures
- Data is stored on **EBS volumes for as long as you want**
- **MSK Serverless**
  - Run Apache Kafka on MSK without managing the capacity
  - MSK automatically provsions resources and scales compute & storage

<img src=./images/kafka.png width="500"/>

## Kinesis Data Streams vs Amazon MSK

| **Kinesis Data Streams** | **Amazon MSK** |
|--------------------------|----------------|
|1MB message size limit    | 1MB default, configure for higher (ex:10MB)
|Data Streams with Shards  | Kafka Topics with partitions
|Shard splitting & Merging | Can only add Partitions to a Topic
|TLS in-flight encryption  | PLAINTEXT or TLS in-flight encryption
|KMS at-rest encryption    | KMS at-rest encryption
# DynamoDB
- Fully managed, HA with multi-az
- Distributed Database
- NoSQL DB with transaction support
- Not an in-memory database (uses storage devices)
- Storage auto-scaling
- Single digit millisecond response time at any scale
- Maximum size of an item: 400 KB
- Millions of requests per seconds, trillions of row, 100s of TB storage
- Integrated with IAM for security, authorization and administration
- Standard and Infrequest Access (IA) Table class
- Primary key (must be decided at creation) can be a single field or a pair of fields (partition key and sort key)
- Each table can have an infinite number of items (=rows)
- Each item has attributes (can be added over time-can be null)
- You can rapidly evolve schemas
- Data types supported are:
  - Scalar types:  String, Boolean, Number, Binary, Null
  - Document types: List, Map
  - Set types: String set, Number set, Binary set
- Supports TTL (automatically delete an item after an expiry timestamp)

## Capacity modes
- **Provisioned Mode (default)**
  - Provision read & write capacity
  - Pay for the provisioned capacity (Read capacity unit (RCU) and Write capacity unit (WCU))
  - Auto-scaling option (eg. set RCU and WCU to 80% and the capacities will be scaled automatically based on the workload)
  - RCU and WCU are decoupled, so you can increase/decrease each value separately.
- **On-demand Mode**
  - Capacity auto-scaling based on the workload
  - Pay for what you use (more expensive)
  - Great for unpredictable workloads, steep sudden spikes

## DynamoDB Accelerator (DAX)
- Fully managed, highly available, in-memory cache
- Caches the queries and scans of DynamoDB items
- Solves read congestion (*ProvisionedThroughputExceededException*)
- Microseconds latency for cached data
- Doesn’t require application code changes
- 5 minutes TTL for cache (default)

## DynamoDB Streams
- Ordered stream of notifications of item-level modifications (create/update/delete) in a table
- Destination can be
  - Kinesis Data Streams
  - AWS Lambda
  - Kinesis Client Library applications
- Data Retention for up to 24 hours
- Use cases: sending welcome emails to new users after they sign up

<img src=./images/dynamodb.png width="500"/>

## Global Tables
- For low latency access in multiple-regions
- Applications can READ and WRITE to the table in any region and the change will automatically be replicated to other tables (active-active cross-region replication)
- Must enable DynamoDB Streams as a pre-requisite (DynamoDB Streams enable DynamoDB to get a changelog and use that changelog to replicate data across replica tables in other AWS Regions.)

## Backups for DR
- Continuous backups using point-time-recovery (PITR)
  - Optionally enabled for the last 35 days
  - The recovery process creates a new table
- On-demand backups
  - Full backups for long-term retention, until explicitely deleted
  - Doesn't affect performance or latency
  - Can be configured and managed in AWS Backup (enables cross-region copy)
  - The recovery process creates a new table

## Integration with S3
- Export to S3 (must enable PITR)
  - Works for any point of time in the last 35 days
  - Retain snapshots for auditing
  - Export in DynamoDB JSON or ION format
- Import from S3
  - Import CSV, DynamoDB JSON or ION format
  - Creates new table
  - Import errors are logged in cloudwatch logs

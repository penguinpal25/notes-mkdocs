# Aurora
- Regional Service (supports global databases)
- Supports Multi AZ
- AWS managed Relational DB cluster
- Preferred over Relational Database Service (RDS)
- Storage auto-scaling in increments of 10GB (max 128TB)
- Up to 15 read replicas
- Asynchronous Replication (milliseconds)
- Supports only MySQL & PostgreSQL
- Cloud-optimized (5x performance improvement over MySQL on RDS, over 3x the performance of PostgreSQL on RDS)
- Costs more than RDS (20% more)
- Failover is instantaneous (HA by native)
- Aurora Global Databases allows you to have an Aurora Replica in another AWS Region, with up to 5 secondary regions.

## High Availability & Read Scaling
- Self healing (if some data is corrupted, it will be automatically healed)
- Storage is striped across 100s of volumes (more resilient)
- One Aurora instance takes writes (master)
- Automated failover
  - A read replica is promoted as the new master in less than 30 seconds
  - Aurora flips the CNAME record for your DB Instance to point at the healthy replica
  - In case no replica is available, Aurora will attempt to create a new DB Instance in the same AZ as the original instance. This replacement of the original instance is done on a best-effort basis and may not succeed.
- Support for Cross Region Replication
- Aurora maintains 6 copies of your data across 3 AZ:
  - 4 copies out of 6 needed for writes (can still write if 1 AZ completely fails)
  - 3 copies out of 6 need for reads

## Endpoints
- Writer Endpoint (Cluster Endpoint)
  - Always points to the master (can be used for read/write)
  - Each Aurora DB cluster has one cluster endpoint
- Reader Endpoint
  - Provides load-balancing for read replicas only (used to read only)
  - If the cluster has no read replica, it points to master (can be used to read/write)
  - Each Aurora DB cluster has one reader endpoint
- Custom endpoint
  - Custom endpoint for a DB cluster represents a set of DB instances that you choose.
  - Aurora performs load balancing and chooses one of the instances in the group to handle the connection.
  - An Aurora DB cluster has no custom endpoints until one is created and up to five custom endpoints can be created for each provisioned cluster.
  - The Reader endpoint is generally not used after defining Custom Endpoints
- Instance endpoint
  - An instance endpoint connects to a specific DB instance within a cluster and provides direct control over connections to the DB cluster.
  - Each DB instance in a DB cluster has its own unique instance endpoint. So there is one instance endpoint for the current primary DB instance of the DB cluster, and there is one instance endpoint for each of the Replicas in the DB cluster.

## Auto scaling
- Read replicas are auto scaled
- Endpoints are extended
- Suitable if existing Read replicas having high resource usage or getting high requests (to handle sudden increases in connectivity or workload)
- When the connectivity or workload decreases, Aurora Auto Scaling removes unnecessary Aurora Replicas so that you don't pay for unused provisioned DB instances.
- scaling policy defines the minimum and maximum number of Aurora Replicas that Aurora Auto Scaling can manage.

## Aurora Serverless
- Optional
- Automated database instantiation and auto scaling based on usage
- Good for unpredictable workloads
- No capacity planning needed
- Pay per second

## Aurora Multi-Master
- Optional
- Every node (replica) in the cluster can read and write
- Used for immediate failover for write node (high availability in terms of write). If disabled and the master node fails, need to promote a Read Replica as the new master (will take some time).
- Client needs to have multiple DB connections for failover

## Aurora Global Database
- Entire database is replicated across regions to recover from region failure
- Designed for globally distributed applications with low latency local reads in each region
- 1 Primary Region (read / write)
- Up to 5 secondary (read-only) regions (replication lag < 1 second)
- Up to 16 Read Replicas per secondary region
- Helps for decreasing latency for clients in other geographical locations
- RTO of less than 1 minute (to promote another region as primary)

## Machine Learning
- To add ML based predictions to applications via SQL
- Secure integration b/w Aurora and ML services
- Supported services:
  - Amazon SageMaker (use with any ML model)
  - Amazon Comprehend (for sentiment analysis)

<img src=./images/auroraml.png width="300"/>

- Use cases:
  - Fraud detection
  - Sentimemt analysis
  - Ads targeting
  - Product recommendations

## Backups
- Automated Backups
  - Backup retention: 1 to 35 days (cannot be disabled)
  - Point in time recovery in that timeframe
- DB Snapshots:
  - Manually triggered
  - Backup retention: unlimited

## Restore options
- Creates a new database
- Restoring MySQL Aurora cluster from S3
  - Creates backup of your on-premises database using Percona XtraBackup
  - Store it in Amazon S3
  - Restore the backup file onto a new Aurora cluster running MySQL

## Database Cloning
- Creates a new Aurora DB cluster from an existsing one
- Faster than snapshot & restore
- New DB cluster uses the same volume and data of original but will change when data updates are made
- Very fast & cost effective
- Useful to create a "staging" db from "production" db without impacting production database

## Encryption & Network Security
- Encryption at rest using KMS (same as RDS)
- Encryption in flight using SSL (same as RDS)
- You can’t SSH into Aurora instances (same as RDS)
- Network Security is managed using Security Groups (same as RDS)
- EC2 instances should access the DB using [IAM DB Authentication] but they can also do it using credentials fetched from the [SSM Parameter Store] (same as RDS)

## Aurora Events
- Invoke a Lambda function from an Aurora MySQL-compatible DB cluster with a native function or a stored procedure (same as RDS)
- Used to capture data changes whenever a row is modified
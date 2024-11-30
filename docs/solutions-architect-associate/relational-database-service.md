# Relational Database Service (RDS)
- AWS Managed SQL Database
- Supported Engines
  - Postgres
  - MySQL
  - MariaDB
  - Oracle
  - Microsoft SQL Server
  - Aurora (AWS Proprietary database)
- Regional Service
- Supports Multi AZ
- Storage backed by EBS (gp2 or io1)
- We don't have access or can't SSH in to the underlying instance
- DB connection is made on port 3306
- Security Groups are used for network security (must allow incoming TCP traffic on port 3306 from specific IPs)
- Point in Time restore

## Auto Scaling
- Automatically scales the RDS storage within the max limit (Maximum Storage Threshold)
- Condition for automatic storage scaling:
  - Free storage is less than 10% of allocated storage
  - Low-storage lasts at least 5 minutes
  - 6 hours have passed since last modification

## Read Replicas
- Up to 5 read replicas (within AZ, cross AZ or cross region)

<img src=./images/readreplica.jpg alt="readreplica" width="300"/>

- Asynchronous Replication (seconds)
- Replicas can be promoted to their own DB
- Applications must update the connection string to leverage read replicas
- Used for SELECT(=read) only kind of statements (not INSERT,DELETE,UPDATE)
- Network fee for replication
  - Same region: free
  - Cross region: paid
> The Read Replicas can be setup as Multi AZ for Disaster Recovery (DR)

## Multi AZ
- Increase availability of the RDS database by replicating it to another AZ

<img src=./images/rdsmultiaz.jpg alt="rdsmultiaz" width="300"/>

- Synchronous Replication
- Connection string does not require to be updated (both the databases can be accessed by one DNS name, which allows for automatic DNS failover to standby database)
- When failing over, RDS flips the CNAME record for the DB instance to point at the standby, which is in turn promoted to become the new primary.
- Cannot be used for scaling as the standby database cannot take read/write operation

### RDS - From Single-AZ to Multi-AZ
- Zero downtime operation (No need to stop the db)
- Just click on "modify" for the database and enable Multi-AZ

<img src=./images/rdsmultiaz.png width="300"/>

- The following happens internally:
  - A snapshot is taken
  - A new DB is restored from the snapshot in a new AZ
  - Synchronization is established b/w these two databases

## Backups
- Automated Backups (enabled by default)
  - Daily full backup of the database (during the defined maintenance window)
  - Backup retention: 1 to 35 days (set 0 to disable automated backups)
  - Transaction logs are backed-up every 5 minutes (point in time recovery)
- DB Snapshots:
  - Manually triggered
  - Backup retention: unlimited
> In a stopped RDS database, you will still pay for the storage. If you plan on stopping it for a long time, you should snapshot & restore instead

## Restore options
- Creates a new database
- Restoring MySQL RDS database from S3
  - Creates backup of your on-premises database
  - Store it in Amazon S3
  - Restore the backup file onto a new RDS instance running MySQL

## RDS Custom
- Managed database services for applications that require operating system and database customization (For Oracle and Microsoft SQL Server)
- Access to underlying OS and database so we can:
  - Configure settings
  - Install patches
  - Enable native features
  - Access the underlying EC2 instance using SSH or SSM Session Manager
- De-activate automation mode to perform customization (better to take db snapshot before)

## Encryption
- At rest encryption
  - KMS AES-256 encryption
  - Encrypted DB => Encrypted Snapshots, Encrypted Replicas and vice versa
- In flight encryption
  - SSL certificates
  - Force all connections to your DB instance to use SSL by setting the **rds.force_ssl** parameter to true
  - To enable encryption in transit, download the AWS-provided root certificates & used them when connecting to DB
- To encrypt an un-encrypted RDS database:
  - Create a snapshot of the un-encrypted database
  - Copy the snapshot and enable encryption for the snapshot
  - Restore the database from the encrypted snapshot
  - Migrate applications to the new database, and delete the old database
- To create an encrypted cross-region read replica from a non-encrypted master:
  - Encrypt a snapshot from the unencrypted master DB instance
  - Create a new encrypted master DB instance
  - Create an encrypted cross-region Read Replica from the new encrypted master

## Access Management
- Username and Password can be used to login into the database
- EC2 instances & Lambda functions should access the DB using IAM DB Authentication (AWSAuthenticationPlugin with IAM) - token based access
- EC2 instance or Lambda function has an IAM role which allows is to make an API call to the RDS service to get the auth token which it uses to access the MySQL database.

<img src=./images/rdsaccess.jpg width="300"/>

- Only works with MySQL and PostgreSQL
- Auth token is valid for 15 mins
- Network traffic is encrypted in-flight using SSL
- Central access management using IAM (instead of doing it for each DB individually)
- EC2 & Lambda can also get DB credentials from SSM Parameter Store to authenticate to the DB - credentials based access

## RDS Proxy
- Fully managed database proxy for RDS
- Allows apps to pool and share db connections established with the database
- Improving the database efficiency by reducing the stress on database resources (eg; cpu,ram) and minimize open connections (and timeouts)
- Serverless, auto-scaling, HA (multi-az)
- If Lambda functions directly access your database, they may open too many connections under high load
- The Lambda function must be deployed in your VPC, bz RDS proxy is never publicly accessible

<img src=./images/rdsproxy.png width="300"/>

- Supports RDS (MySQL, PostgreSQL, MariaDB) and Aurora (MySQL, PostgreSQL)
- No code changes required for most apps
- Enfore IAM authentication for DB, and securely store credentials in AWS Secret Manager
- Cannot publicly accessible (must be accessed from vpc)

## RDS Events
- RDS events only provide operational events on the DB instance (not the data)
- To capture data modification events, use native functions or stored procedures to invoke a Lambda function.

## Monitoring
- CloudWatch Metrics for RDS
  - Gathers metrics from the hypervisor of the DB instance
    - CPU Utilization
    - Database Connections
    - Freeable Memory
- Enhanced Monitoring
  - Gathers metrics from an agent running on the RDS instance
    - OS processes
    - RDS child processes
  - Used to monitor different processes or threads on a DB instance (ex. percentage of the CPU bandwidth and total memory consumed by each database process in your RDS instance)

## Maintenance & Upgrade
  - Any database engine level upgrade for an RDS DB instance with Multi-AZ deployment triggers both the primary and standby DB instances to be upgraded at the same time. This causes downtime until the upgrade is complete. This is why it should be done during the maintenance window.

## RDS Databases ports:
- PostgreSQL: 5432
- MySQL: 3306
- Oracle RDS: 1521
- MSSQL Server: 1433
- MariaDB: 3306 (same as MySQL)
- Aurora: 5432 (if PostgreSQL compatible) or 3306 (if MySQL compatible)

- RDS MySQL to Aurora MySQL
  - Option 1: DB snapshots from RDS mysql restored as Mysql aurora db
  - Option 2: Create an Aurora read replica from your rds mysql, and when the replication lag is 0, promote it as its own db cluster.
  
- External MySQL to Aurora MySQL
  - Option 1: Use Percona XtraBackup to create a file backup in Amazon S3, Create an Aurora mysql db from Amazon S3
  - Option 2: Create an Aurora Mysql db, Use the mysqldump utility to migrate mysql into Aurora (slower than s3 method)
  
- Use DMS if both databases are running

- For Aurora PostgreSQL migrations
  - If it is RDS then the process is same as of RDS Mysql to Aurora mysql.
  - If it is External then, Create a backup and put it in Amazon S3, Import it using aws_s3 Aurora extension
  - Use DMS if both databases are up and running.

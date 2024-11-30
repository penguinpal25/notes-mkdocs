# Redshift
- fully managed
- Based on PostgreSQL
- Used for **analytics and data warehousing**
- Not used for OLTP
- Used for **Online Analytical Processing (OLAP)** and high performance querying
- **Columnar** storage of data (instead of row based) and parallel query execution in SQL
- Need to provision instances as a part of the Redshift cluster (pay for the instances provisioned)
- Can be integrate with Business Intelligence (BI) tools such as QuickSight or Tableau
- Faster querying than Athena due to indexes
- Data must be loaded into Redshift before queries can be run

## Redshift Cluster
- Provision the node size in advance
- Redshift Cluster can have 1 to 128 compute nodes (128TB per node)
  - Leader Node: query planning & result aggregation
  - Compute Nodes: perform queries & send the result to leader node
- Can use Reserved instances for cost savings

<img src=./images/redshiftcluster.png width="300"/>

## Snapshots & DR
- **Redshift has Multi-AZ mode for some clusters**
- Snapshots are point-in-time backups of a cluster and Stored internally in S3
- Incremental (only changes are saved)
- Can be restored into a **new Redshift cluster**
- Automated
  - based on a schedule (every 8hrs) or storage size (every 5 GB)
  - set retention
- Manual
  - retains until you delete them
- **Can configure Redshift to copy snapshots (automated or manual) of a cluster to another AWS region**

## Loading data into Redshift
- **Kinesis Data Firehose**
  - Sends data to S3 and issues a COPY command to load it into Redshift
- **S3**
  - Use COPY command to load data from an S3 bucket into Redshift
  - Without Enhanced VPC Routing
    - data goes through the public internet
  - With Enhanced VPC Routing
    - data goes through the VPC
- **EC2 Instance**
  - Using JDBC driver
  - Used when an application needs to write data to Redshift
  - Better to write data in batches

## Redshift Spectrum
- Query data present in S3 without loading it into Redshift
- **Must have a Redshift cluster available to start the query**
- Query is executed by 1000s of Redshift Spectrum nodes

<img src=./images/redshiftspectrum.jpg width="300"/>


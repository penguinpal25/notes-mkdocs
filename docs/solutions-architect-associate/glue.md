# Glue
- Managed **extract, transform, and load (ETL)** service
- Fully **serverless** service
- Used to prepare & transform data for analytics

<img src=./images/glue.png width="500"/>

- Used to get data from a store, process and put it in another store (could be the same store)
>Glue ETL job can write the transformed data using a compressed file format to save storage cost

## Convert data into Parquet format
- Parquet format is of Columnar storage data

<img src=./images/glueconvert.png width="500"/>

- The process of conversion can be automated using Lambda function or EventBridge, so whenever a data is inserted to S3 then Glue ETL job will be triggered

## Glue Data Catalog
- Glue Data Crawlers crawl databases and collect metadata which is populated in Glue Data Catalog
- Metadata can be used for analytics

<img src=./images/gluedatacatalog.png width="500"/>

## Things to know at high-level
- **Glue Job Bookmarks**: Prevent re-processing of old data
- **Glue Elastic Views**: 
  - Combine and replicate data across multiple data stores using SQL
  - No custom code, Glue monitors for changes in source data, serverless
  - Leverages a "virtual table" 
- **Glue DataBrew**: Clean and normalize data using pre-built tranformation
- **Glue Studio**: new GUI to create, run and monitor ETL jobs in Glue
- **Glue Streaming ETL** (built on Apache Spark structured streaming): compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka)
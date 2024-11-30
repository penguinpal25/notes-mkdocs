# Athena
- Serverless query service to perform analytics on data stored in S3 
- Uses SQL language to query the files (built on Presto engine)
- Supports CSV, JSON, ORC, Avro and Parquet file formats
- Runs directly on S3 (no copying needed)
- Pricing: $5 per TB of scanned
- Commonly used with Amazon Quicksight for reporting/dashboards
- Uses cases: analyze and query VPC Flow logs, ELB logs, CloudTrails logs etc

## Performance improvement
- Use **columnar data** for cost-savings (due to less scan)
  - Apache Parquet or ORC is recommended
  - Use Glue to convert your data to Parquet or ORC
- **Compress data** for smaller retrievals (bzip2, gzip, lz4, snappy...)
- **Partition** datasets in S3 for easy querying on virtual columns
  - Ex: s3://athena-examples/flight/parquet/year=2009/month=2/day=5/
- **Use larger files** (> 128MB) 

## Federated query
- Allows to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises)
- Uses Data Source Connector that run on AWS Lambda
- Store the results back in S3

[alt](images/athena.png)


> Can be used along with AWS Transcribe (automatic speech recognition service that converts audio to text) to analyze audio for sentiment analysis.
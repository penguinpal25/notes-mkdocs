# Neptune
- AWS managed graph database
- Used for high relationship data (eg. social networking)
- Highly available across 3 AZ with up to 15 read replicas
- Point-in-time recovery due to continuous backup to S3
- Support for KMS encryption at rest + HTTPS for in-flight encryption
- Need to provision nodes in advance (pay for the provisioned nodes)
- Optimized for complex and hard queries
- Can store upto billions of relations and query the graph with milliseconds latency
# ElastiCache
- Regional Service
- AWS managed caching service
- In-memory key-value store with sub-millisecond latency
- Need to provision an underlying EC2 instance
- Makes the application stateless because it doesn’t have to cache locally
- Using ElastiCache requires heavy application code changes (setup the application to query the cache before and after querying the database)
- DB Cache (lazy loading)

  <img src=./images/dbcache.png width="400"/>

  - Application queries ElastiCache, if not available, get from RDS and store it in ElastiCache
  - Help relieve load in RDS
  - Cache must have an invalidation strategy to make sure only the most current data is used in there
- User session store
  
  <img src=./images/sessionstore.png width="400"/>

  - User logs into any of the application
  - The application writes the session data into ElastiCache
  - The user hits another instance of our application
  - The instance retrieves the data and the user is already logged in
  - store user's session data like cart info

## Redis vs Memcached
|           **Redis**           |           **Memcached**           |
|:------------------------------|:----------------------------------|
|In-memory data store	        |Distributed memory object cache    |
|Read Replicas (for scaling reads & HA)	| No replication            |
|Backup & restore	| No backup & restore                           |
|Single-threaded	| Multi-threaded |
|HIPAA compliant	| Not HIPAA compliant |
|Data is stored in an in-memory DB which is replicated	| Data is partitioned across multiple nodes (sharding) |
|Redis Sorted Sets are used in realtime Gaming Leaderboards	| 
|Good for auto-completion	|
|Multi-AZ support with automatic failover (disaster recovery)	|

## Security & Access Management
- Network security is managed using Security Groups (only allow EC2 security group for incoming requests)
- At rest encryption using KMS
- In-flight encryption using SSL
- Use Redis Auth to authenticate to ElastiCache for Redis
- Memcached supports SASL-based authentication

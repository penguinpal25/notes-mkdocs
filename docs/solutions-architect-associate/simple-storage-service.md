# Simple Storage Service (S3)
- Global Service
- Object store (key-value pairs)
- Buckets must have a globally unique name
- Buckets are defined at the regional level
- Objects have a key (full path to the object)
  > s3://my_bucket/my_folder/another_folder/my_file.txt
- The key is composed of bucket + prefix + object name
- There’s no concept of directories within buckets (just keys with very long names that contain slashes)
- Max Object Size: 5TB
- Durability: 99.999999999% (total 11 9's)
- SYNC command can be used to copy data between buckets, possibly in different regions
- S3 delivers strong read-after-write consistency (if an object is overwritten and immediately read, S3 always returns the latest version of the object)
- S3 is strongly consistent for all GET, PUT and LIST operations
- Multi-Part Upload is recommended as soon as the file is over 100 MB.

## Pre-signed URL
- Pre-signed URLs for S3 have temporary access token as query string parameters which allow anyone with the URL to temporarily access the object before the URL expires (default 1h)
- Pre-signed URLs inherit the permission of the user who generated it
- Allow only logged-in users to access the object
- URL expiration:
  - S3 console: 12hrs
  - AWS CLI: default 3600 secs, can configure expiration with --expires-in parameter in seconds

## Access Management
- User based security
  - IAM policies define which API calls should be allowed for a specific user
  - Preferred over bucket policy for fine-grained access control
  - Explicit DENY in an IAM Policy will take precedence over an S3 bucket policy.
- Resource based security (Bucket Policy)
  - Grant public access to the bucket
  - Force objects to be encrypted at upload
  - Cross-account access
  - Object Access Control List (ACL) - applies to the objects while uploading
  - Bucket Access Control List (ACL) - access policy that applies to the bucket
> An IAM principal can access an S3 object if "the user IAM permissions ALLOW it OR the resource policy ALLOWS it AND there is no explicit DENY"
  - JSON based policies

    <img src=./images/bucketpolicy.png width="300"/>

    - Resources: buckets and objects
    - Effect: Allow or Deny
    - Action: Set of API to allow or deny
    - Principal: The account or user to apply the policy to

## S3 Static Websites
- Host static websites (may contain client-side scripts) and have them accessible on the public internet over HTTP only (for HTTPS, use CloudFront with S3 bucket as the origin)
- The website URL will be either of the following:
  > bucket-name.s3-website-region.amazonaws.com  
  > bucket-name.s3-website.region.amazonaws.com
- If you get a 403 (Forbidden) error, make sure the bucket policy allows public reads
- To host an S3 static website on a custom domain using Route 53, the bucket name should be the same as your domain or subdomain Ex. for subdomain portal.example.com, the name of the bucket must be portal.example.com

## Bucket Versioning
- Enabled at the bucket level
- Protects against unintended deletes
- Ability to restore to a previous version
- Any file that is not versioned prior to enabling versioning will have version “null”
- Suspending versioning does not delete the previous versions, just disables it for the future
- To restore a deleted object, delete it's "delete marker"
- Versioning can only be suspended once it has been enabled.
- Once you version-enable a bucket, it can never return to an unversioned state.

## Replication
- Supports cross-region, same-region and cross-account replication
- Versioning must be enabled for source and destination buckets
- Asynchronous replication
- Objects are replicated with the same version ID
- Must give proper IAM permissions to S3
- For DELETE operations:
  - Replicate delete markers from source to target (optional)
  - Permanent deletes are not replicated
- There is no chaining of replication. So, if bucket 1 has replication into bucket 2, which has replication into bucket 3. Then objects created in bucket 1 are not replicated to bucket 3.
- Uses cases:
  - CRR: compliance, lower-latency access, cross-account replication
  - SRR: log aggregation, live replication b/w production and test accounts
- After you enable replication, only new objects are replicated
- You can replicate existing objects using S3 Batch Replication
  - Replicates existing objects and objects that failed replication

## Storage Classes
- Data can be transitioned between storage classes manually or automatically using lifecycle rules
- Data can be put directly into any storage class
- **Standard**
  - 99.99% availability
  - Most expensive
  - Instant retrieval
  - No cost on retrieval (only storage cost)
  - For frequently accessed data
- **Infrequent Access**
  - For data that is infrequently accessed, but requires rapid access when needed
  - Lower storage cost than Standard but cost on retrieval
  - Can move data into IA from Standard only after 30 days
  - Two types:
    - **Standard IA**
      - 99.9% Availability
    - **One-Zone IA**
      - 99.5% Availability
      - Data is lost if AZ fails
      - Storage for infrequently accessed data that can be easily recreated
- **Glacier**
  - For data archival
  - Cost for storage and retrieval
  - Can move data into Glacier from Standard anytime
  - Objects cannot be directly accessed, they first need to be restored which could take some time (depending on the tier) to fetch the object.
  - Default encryption for data at rest and in-transit
  - Three types:
    - **Glacier Instant Retrieval**
      - 99.9% availability
      - Millisecond retrieval
      - Minimum storage duration of 90 days
      - Great for data accessed once a quarter
    - **Glacier Flexible Retrieval**
      - 99.99% availability
      - 3 retrieval flexibility (decreasing order of cost):
        - Expedited (1 to 5 minutes)
        - Standard (3 to 5 hours)
        - Bulk (5 to 12 hours)
      - Minimum storage duration of 90 days
    - **Glacier Deep Archive**
      - 99.99% availability
      - 2 flexible retrieval:
        - Standard (12 hours)
        - Bulk (48 hours)
      - Minimum storage duration of 180 days
      - Lowest cost
- **Intelligent Tiering**
  - 99.9% availability
  - Moves objects automatically between Access Tiers based on usage
  - Small monthly monitoring and auto-tiering fee
  - No retrieval charges
- **Moving between Storage Classes**
  - In the diagram below, transition can only happen in the downward direction

  <img src=./images/storageclass.jpg width="500"/>

##  Lifecycle Rules
- Used to automate transition or expiration actions on S3 objects
- Transition Action (transitioned to another storage class)
- Expiration Action (delete objects after some time)
  - delete a version of an object
  - delete incomplete multi-part uploads
- Lifecycle Rules can be created for a prefix (ex s3://mybucket/mp3/*) or objects tags (ex Department: Finance)

## S3 Analytics
- Provides analytics to determine when to transition data into different storage classes
- Does not work for ONEZONE-IA & GLACIER

## Requester Pays Buckets
- In general, the bucket owner pays for all Amazon S3 storage and data transfer costs associated with the objects
- With Requester Pays buckets, Requester pays the cost of the request and the data downloaded from the bucket. The bucket owner only pays for the storage.
- Used to share large datasets with other AWS accounts
- The requester must be authenticated in AWS (cannot be anonymous)

  <img src=./images/reqpays.png width="500"/>

## S3 Notification Events
- Optional
- Generates events for operations performed on the bucket or objects
- Object name filtering using prefix and suffix matching
- Deliver events in seconds but can sometimes take a minute or longer
- Targets:
  - SNS topics
  - SQS Standard queues (not FIFO queues)
  - Lambda functions
  - Amazon EventBridge
    - All events from S3 bucket goes to Amazon EventBridge and which have rules to route the events to go to over 18 AWS services as destinations
    - Advanced filtering options with JSON rules (metadata, object size, name...)
    - Multiple destinations
    - EventBridge capabilities - Archive, Reply events, Reliable delivery

## Performance
- 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix
- Recommended to spread data across prefixes for maximum performance
- SSE-KMS may create bottleneck in S3 performance
- Performance Optimizations
  - **Multi-part Upload**
    - parallelizes upload
    - recommended for files > 100MB
    - must use for files > 5GB
  - **Byte-range fetches**
    - Parallelize download requests by fetching specific byte ranges in each request
  - **S3 Transfer Acceleration**
    - Speed up upload and download for large objects (>1GB) for global users
    - Data is ingested at the nearest edge location and is transferred over AWS private network (uses CloudFront internally)

## S3 Select
- Select a subset of data from S3 using SQL queries (server-side filtering)
- Less network cost
- Less CPU cost on the client-side

## S3 Batch Operations
 - Perform bulk operations on existing objects with a single request
 - Ex:
   - Modify object meta-data and properties
   - Copy objects b/w S3 buckets
   - Encrypt unencrypted objects
   - Modify ACLs, Tags
   - Restore objects from S3 Glacier
   - Invoke Lambda function to perform custom action on each object
- Manager retries, tracks progress, sends completion notifications, generate reports

<img src=./images/batchops.png width="300"/>

- Use S3 inventory to get object list and use S3 Select to filter your objects

## Encryption
- Can be enabled at the bucket level or at the object level
- **Server Side Encryption (SSE)**
  - **SSE-S3**
    - Keys managed by S3
    - AES-256 encryption
    - HTTP or HTTPS can be used
    - Must set header: *"x-amz-server-side-encryption": "AES256"*
    - Enabled by default for new buckets & objects
  - **SSE-KMS**
    - Keys managed by KMS
    - HTTP or HTTPS can be used
    - Must set header: *"x-amz-server-side-encryption": "aws:kms"*
    - KMS advantages: user control + audit key usage using CloudTrail
    - With SSE-KMS, the encryption happens in AWS, and the encryption keys are managed by AWS but you have full control over the rotation policy of the encryption key. Encryption keys stored in AWS.
    - Limitations:
      - You may be impacted by KMS limits
      - upload: calls GenerateDataKey KMS API, download: Decrypt KMS API
  - **SSE-C**
    - Keys managed by the client
    - Client sends the key in HTTPS headers for encryption/decryption (S3 discards the key after the operation)
    - Does NOT store the key
    - HTTPS must be used
- **Client Side Encryption**
  - Customer fully manages the key and encryption cycle
  - Customer encrypts the object before sending it to S3 and decrypts it after retrieving it from S3

### Default Encryption
- SSE-S3 encryption is automatically applied to new objects and we can change that to SSE-KMS if we want
- Bucket policy can be used to force a specific type of encryption (SSE-KMS or SSE-C) on the objects uploaded to S3
> Bucket policies are evaluated before Default encryption

## S3 CORS
- Cross-Origin Resource Sharing (CORS)
- An origin is a combination of scheme (protocol), host (domain) and port
- Eg: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
- Same origin: http://example.com/api1 & http://example.com/api2
- Different origins: http://api1.example.com & http://api2.example.com
- CORS is a web browser based security to allow requests to other origins while visiting the main origin only if the other origin allows for the requests from the main origin, using CORS Headers (*Access-Control-Allow-Origin* & *Access-Control-Allow-Methods*)

<img src=./images/cors.png width="700"/>

- Assume a web browser accesses www.example.com and the request is routed to the origin web server. The origin server responds with index.html, which includes the address of the cross-origin server where the images are stored. Because web browsers support CORS, they will first perform a preflight request using the OPTIONS method to ask the cross-origin server to check and allow requests from the origin server. If the cross-origin server has CORS configured to allow requests from the origin server, it will send a preflight response with CORS headers. When the origin server receives the CORS headers, the web browser can make the original requests to cross-origin to serve images.
- For cross-origin access to the S3 bucket, we need to enable CORS on the bucket

<img src=./images/corss3.png width="700"/>

## MFA Delete
- MFA required to
  - permanently delete an object version
  - suspend versioning on the bucket
- Bucket Versioning must be enabled
- Can only be enabled or disabled by the root user

## S3 Access Logging
- Most detailed way of logging access to S3 buckets (better than CloudTrail)
- Store S3 access logs into another bucket
- Logging bucket should not be the same as monitored bucket (logging loop)
- Does not support Data Events & Log File Validation (use CloudTrail for that)
- Target logging bucket must be in same aws region

## Glacier Vault Lock
- WORM (Write Once Read Many) model for Glacier
- Create a Lock policy (can no longer be changed or deleted)
- For compliance and data retention

## Object Lock (Versioning must be enabled)
- WORM (Write Once Read Many) model
- Block an object version modification or deletion for a specified amount of time
- Modes:
  - **Compliance mode**
    - A protected object version cannot be overwritten or deleted by any user, including the root user
    - The object's retention mode can’t be changed, and the retention period can’t be shortened
  - **Governance mode**
    - Only users with special permissions can overwrite or delete the object version or alter its lock settings
- Retention period: protect the object for a fixed period, it can be extended
- LegalHold:
  - Protect the object indefenitely, independent from retention period
  - can be freely placed and removed using the s3:PutObjectLegalHold IAM permission

## S3 Access Points
- Each access point gets its own DNS and policy to limit who can access it
  - A specific IAM user/group
  - One policy per access point=> Its easier to manage than complex bucket policies

  <img src=./images/s3ap.png width="700"/>

## S3 Object Lambda
- Use AWS Lambda functions to change the object before its retrieved by the caller application
- Only one S3 bucket is needed, on top of which we creates S3 Access Point and S3 Object Lambda Access Points

<img src=./images/s3objectlambda.png width="400"/>
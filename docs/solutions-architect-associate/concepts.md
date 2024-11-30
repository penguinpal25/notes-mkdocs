# Scalability
- Ability to handle greater loads by adapting
- **Vertical Scalability (scaling up / down)**
  - increasing the size (performance) of the instance
  - used in non-distributed systems, such as a database.
  - limited by hardware
  - From: t2.nano - 0.5G of RAM, 1vCPU 
  - To: u-12tb1.metal - 12 TiB of RAM, 448vCPUs
- **Horizontal Scalability (elasticity) (scaling out / in)**
  - increasing the number of instances of the application
  - used in distributed systems
  - easier to achieve than vertical scalability
  - Auto scaling group and Load balancer

# High Availability
 - Ability to survive a hardware or AZ failure
 - Can be achived by running your application / system in at least 2 data centers (== AZ)
 - Auto scaling group multi AZ, Load balancer multi AZ

 # DNS
- Translates the human friendly hostnames into the machine IP addresses
- Terminologies:
  - Domain Registrar: registers domain names (Amazon Route 53, GoDaddy, etc.)
  - DNS Records: A, AAAA, CNAME, NS, etc.
  - Hosted Zone: contains DNS records (used to match hostnames to IP addresses)
  - Name Server (NS): resolves DNS queries (Authoritative or Non-Authoritative)
  - Top Level Domain (TLD)
  - Second Level Domain (SLD)
- Domain Name Structure

<img src=./images/dns.png width="500"/>

- How DNS works

<img src=./images/dnsworking.jpg width="500"/>

  - Your web browser wants to access the domain example.com which is being served by a server at IP 9.10.11.12. Your browser will first query the local DNS server which if it has that domain cached, it will return it right away. Otherwise, it will ask the same question to the Root DNS server. The root DNS server will extract the TLD (.com) from the domain and direct the local DNS to the TLD DNS Server that can serve .com TLD. The query to the TLD DNS server will be the same. The TLD DNS server returns the IP of the SLD DNS server which can store the IP of web server hosting example.com. Once again the same query is made to the SLD DNS Server which returns the IP 9.10.11.12 instead of NS (named server).

# Micro-Services Architecture
- Many services interact with each other directly using a REST API
- Allows us to have a leaner development lifecycle for each service
- Services can scale independently of each other
- Each service has a separate code repository (easy for development)
- Communication between services:
  - Synchronous patterns: API Gateway, Load Balancers
  - Asynchronous patterns: SQS, Kinesis, SNS

<img src=./images/micro.png width="500"/>

# Software updates distribution
- We have an application running on EC2, that distributes software updates once in a while
- When a new software update is out, we get a lot of request and the content is distributed in mass over the network, It's very costly
- We don't want to change our application, but want to optimize our cost and cpu, how can we do it?

<img src=./images/ar.png width="500"/>

- Why **CloudFront**?
  - No changes to architecture
  - Will cache software update files at the edge
  - Software update files are not dynamic, they are static (never changing)
  - Our EC2 instances are not serverless
  - But CloudFront is, and will scale for us
  - Our ASG will not scale as much, and we will save more in EC2
  - We will also save in availability, network bandwidth cost etc
  - Easy way to make an existing application more scalable and cheaper

# Big Data ingestion pipeline

<img src=./images/pipeline.png width="700"/>

- IoT Core allows to harvest data from IoT devices
- Kinesis is great for real-time data collection
- Firehouse helps with data delivery to S3 in near real-time (1min)
- Lambda can help Firehouse with data transformations
- Amazon S3 can trigger notifications to SQS
- Lambda can subscribe to SQS
- Athena is a serverless SQL service and results are stored in S3
- The reporting bucket contains analyzed data and can be used by reporting tool such as AWS QuickSight, Redshift etc

# Encryption
## Encryption in flight (SSL)
- Data is encrypted before sending and decrypted after receiving
- SSL certificates helps with encryption (HTTPS)
- Ensures no MITM (man in the middle attack) can happen

## Server side encryption at rest
- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form (data key)
- The encryption/decryption keys must be managed somewhere and the server must have access to it

## Client side encryption
- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by the receiving client
- Server should not be able to decrypt the data
- Could leverage Envelope Encryption
# Route53
- Global Service
- AWS managed Authoritative DNS (customer can update the DNS records and have full control over the DNS)
- Also a Domain Registrar (for registering domain names)
- Only AWS service which provides 100% availability SLA
> It is recommended to use DNS names or URLs instead of IPs wherever possible

## Hosted Zone
- A container for DNS records that define how to route traffic to a domain and its subdomains.
- Hosted zone is queried to get the IP address from the hostname
- Two types:
  - Public Hosted Zone
    - resolves public domain names
    - can be queried by anyone on the internet
  - Private Hosted Zone
    - resolves private domain names
    - can only be queried from within the VPC

## TTL (Time-To-Live)
- The time for which a DNS resolver caches a response
- Amazon Route 53 does not have a default TTL for any record type
- You must always specify a TTL for each record so that caching DNS resolvers can cache your DNS records to the length of time specified through the TTL.
- Each DNS record has a TTL (Time To Live) which orders clients for how long to cache these values and not overload the DNS Resolver with DNS requests. The TTL value should be set to strike a balance between how long the value should be cached vs. how many requests should go to the DNS Resolver.


## Record Types
- A - maps a hostname to IPv4
- AAAA - maps a hostname to IPv6
- CNAME - maps a hostname to another hostname
  - The target is a domain name which must have an A or AAAA record
  - Cannot point to root domains (Zone Apex) Ex: you can’t create a CNAME record for *example.com*, but you can create for *something.example.com*
- NS (Name Servers) - controls how traffic is routed for a domain
- Alias - maps a hostname to an AWS resource
  - AWS proprietary
  - Can point to root (zone apex) and non-root domains
  - Alias Record is of type A or AAAA (IPv4 / IPv6)
  - Targets can be
    - Elastic Load Balancers
    - CloudFront Distributions
    - API Gateway
    - Elastic Beanstalk environments
    - S3 Websites
    - VPC Interface Endpoints
    - Global Accelerator accelerator
    - Route 53 record in the same hosted zone
  - Target cannot be EC2 DNS hostname

## Routing Policies
- **Simple**
  - Route to one or more resources
  - If multiple values are returned, client chooses one at random (client-side load balancing)
  - No health check (if returning multiple resources, some of them might be unhealthy)

  <img src=./images/simplerp.jpg width="300"/>

- **Weighted**
  - Control the % of requests that go to each specific resource
  - Assign each record a relative weight
    - traffic (%) = $\frac{Weight for a specific record}{Sum of all the weights for all records}$
  - DNS records must have the same name and record and can be associated with health checks
  - Uses cases:
    - Load balancing between regions
    - testing a new application version by sending a small amount of traffic
  - Assign a weight of 0 to a record to stop sending traffic to a resource
  - If you set Weight to 0 for all of the records in the group, traffic is routed to all resources with equal probability. 

  <img src=./images/weightedrp.png width="300"/>

- **Latency-based**
  - Redirect to the resource that has the lowest latency
  - Latency is based on traffic b/w users and AWS Regions
  - Health checks
  - Can be used for Active-Active failover strategy

  <img src=./images/latencyrp.png width="500"/>

- **Failover**
  - Primary & Secondary Records (if the primary application is down, route to secondary application)
  - Health check must be associated with the primary record
  - Used for Active-Passive failover strategy

  <img src=./images/rpf.png width="500"/>

- **Geolocation**
  - Routing based on the client's location
  - Specify location by continent, country or by state (If there's overlapping, most precise location is selected)
  - Should create a “Default” record (in case there’s no match on location)
  - Use cases: restrict content distribution & language preference
  - Can be associated with health checks
- **Geoproximity**
  - Route traffic to your resources based on the geographic location of clients and the resources
  - Ability to shift more traffic to resources based on the defined bias.
    - To expand (bias: 1 to 99) → more traffic to the resource
    - To shrink (bias: -1 to -99) → less traffic to the resource
  - Resources can be:
    - AWS resources (specify AWS region)
    - Non-AWS resources (specify Latitude and Longitude)
  - Uses Route 53 Traffic Flow
- **Multi-value**
  - Route traffic to multiple resources (max 8)
  - Health Checks (only healthy resources will be returned)

## Health Checks
- HTTP Health Checks are only for public resources
- Allows for Automated DNS Failover
- Three types:
  - **Monitor an endpoint (application or other AWS resource)**
    - Multiple global health checkers check the endpoint health
    - Must configure the application firewall to allow incoming requests from the IPs of Route 53 Health Checkers
    - Supported protocols: HTTP, HTTPS and TCP
    
    <img src=./images/hce.png width="300"/>
  
    - Healthy/Unhealthy threshold -3 (default)
    - Interval - 30 sec (can set to 10 sec - higher cost)
    - Health checks only pass when the endpoint responds with 2xx or 3xx status codes
  - **Monitor other health checks (Calculated Health Checks)**
    - Combine the results of multiple Health Checks into one (AND, OR, NOT)

    <img src=./images/hcc.png width="300"/>

    - Specify how many of the health checks need to pass to make the parent pass
    - Can monitor upto 256 child health checks
    - Usage: perform maintenance to your website without causing all health checks to fail
  - **Monitor CloudWatch Alarms (to perform health check on private resources)**
    - Route 53 health checkers are outside the VPC. They can’t access private endpoints (private VPC or on-premises resources).
    
    <img src=./images/hcp.png width="300"/>

    - Create a CloudWatch Metric and associate a CloudWatch Alarm to it, then create a Health Check that checks the CW alarm.

## GoDaddy with Route 53
- Use GoDaddy as registrar and Route 53 as DNS
  - Once we register a domain at GoDaddy, we need to update the name servers (NS) of domain to match the name servers of a public hosted zone created in Route 53. This way, GoDaddy will use Route 53’s DNS.

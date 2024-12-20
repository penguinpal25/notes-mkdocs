# Elastic Load Balancer
- Regional Service
- Managed load balancer
- Supports Multi AZ
- Spread load across multiple EC2 instances
- Separate public traffic from private traffic
- Expose a single point of access (DNS) to your application
- Does not support weighted routing

## Health Checks
- Health checks allow ELB to know which instances are working properly (to know if instances it forwards traffic to are available to reply requests)
- Done on a port and a route, /health is common (Example, Protocol: HTTP    Port:4567   Endpoint: /health )
- If the response is not 200 (OK), then the instance is unhealthy

## Types of ELB
- Classic Load balancer (v1 - old generation) - 2009 - CLB
  - HTTP, HTTPS, TCP, SSL (Secure TCP)
- Application Load balancer (v2 - new generation) - 2016 - ALB
  - HTTP, HTTPS, WebSocket
- Network Load balancer (v2 - new generation) - 2017 - NLB
  - TCP, TLS (Secure TCP), UDP
- Gateway Load balancer - 2020 - GWLB
  - Operates at Layer 3 (Network Layer) - IP Protocol

## Classic Load Balancer (CLB) - deprecated
- The Classic Load Balancer is deprecated at AWS and will soon not be available in the AWS Console.
- Load Balancing to a single application
- Supports HTTP, HTTPS (layer 7) & TCP, SSL (layer 3)
- Health checks are HTTP or TCP based
- Provides a fixed hostname (xxx.region.elb.amazonaws.com)

## Application Load Balancer (ALB)
- Load balancing to multiple applications (target groups)
- Operates at Layer 7 (HTTP, HTTPS and WebSocket)
- Provides a fixed hostname (xxx.region.elb.amazonaws.com)
- ALB terminates the upstream connection and creates a new downstream connection to the targets (connection termination)
- Great for micro services & container-based applications (Docker & ECS)
- Support redirects (From HTTP to HTTPS for example)
- Has a port mapping feature to redirect a dynamic port in ECS
- Client info is passed in the request headers
  - Client IP => X-Forwarded-For
  - Client Port => X-Forwarded-Port
  - Protocol => X-Forwarded-Proto
- Target Groups
  - Health checks are done at the target group level
  - Target Groups could be
    - EC2 instances - HTTP
    - ECS tasks - HTTP
    - Lambda functions - HTTP request is translated into a JSON event
    - Private IP Addresses
- Listener Rules can be configured to route traffic to different target groups based on:
  - Path (example.com/users & example.com/posts)
  - Hostname (one.example.com & other.example.com)
  - Query String (example.com/users?id=123&order=false)
  - Request Headers
  - Source IP address

## Network Load Balancer (NLB)
- Operates at Layer 4 (TCP, TLS, UDP)
- Can handle millions of request per seconds (extreme performance)
- Lower latency ~ 100 ms (vs 400 ms for ALB)
- 1 static public IP per AZ (vs a static hostname for CLB & ALB)
- Elastic IP can be assigned to NLB (helpful for whitelisting specific IP)
- Maintains the same connection from client all the way to the target
- **No security groups can be attached to NLBs**. They just forward the incoming traffic to the right target group as if those requests were directly coming from client. So, the attached instances must allow TCP traffic on port 80 from anywhere.
- Within a target group, NLB can send traffic to
  - EC2 instances
    - If you specify targets using an instance ID, traffic is routed to instances using the primary private IP address
  - IP addresses
    - Used when you want to balance load for a physical server having a static IP.
  - Application Load Balancer (ALB)
    - Used when you want a static IP provided by an NLB but also want to use the features provided by ALB at the application layer.

## Gateway Load Balancer (GWLB)
- Operates at layer 3 (Network layer) - IP Protocol
- Used to route requests to a fleet of 3rd party virtual appliances like Firewalls, Intrusion Detection and Prevention Systems (IDPS), etc.
- Performs two functions:
  - Transparent Network Gateway (single entry/exit for all traffic)
  - Load Balancer (distributes traffic to virtual appliances)
- Uses GENEVE protocol (on port 6081)
- Target groups for GWLB could be
  - EC2 instances
  - IP addresses

<img src=./images/gwlb.png width="200"/>

## Sticky Sessions (Session Affinity)
- Requests coming from a client is always redirected to the same instance based on a cookie. After the cookie expires, the requests coming from the same user might be redirected to another instance.
- Only supported by CLB & ALB
- Used to ensure the user doesn’t loss his session data, like login or cart info, while navigating between web pages.
- Stickiness may cause load imbalance
- Cookies could be
  - Application-based (TTL defined by the application)
    - Custom cookie
      - Generated by the target
      - Can include custom attributes required by the application
      - Cookie name must be specified individually for each target group
      - Don't use AWSALB, AWSALBAPP, or AWSALBTG (reserved for use by the ELB)
    - Application cookie
      - Generated by the load balancer
      - Cookie name is AWSALBAPP
  - Duration based cookie
    - Cookie generated by the load balancer
    - Cookie name is AWSALB for ALB, AWSELB for CLB

## Cross-zone Load Balancing
- Allows ELBs in different AZ containing unbalanced number of instances to distribute the traffic evenly across all instances in all the AZ registered under a load balancer.
![](/crosszoelb.jpg)
- Supported Load Balancers
  - Classic Load Balancer
    - Disabled by default
    - No charges for inter AZ data
  - Application Load Balancer
    - Always on (can’t be disabled)
    - Can be disabled at target level
    - No charges for inter AZ data
  - Network Load Balancer & Gateway Load Balancer
    - Disabled by default
    - Charges for inter AZ data

## SSL & TLS Certificates
- A certificate is used to guarantee trust between two parties during a transaction
- TLS (Transport Layer Security) certificate ensure that the communication between the user and the server is encrypted and the server is who it says it is.
- TLS or HTTPS relies on symmetric encryption which is computationally light. But, if the key is sniffed while being sent from the server to the user, it could be used to decrypt the communication between the user and the server. So, we use asymmetric encryption to encrypt & transfer the symmetric key from the user to the server and then use symmetric encryption for future communications.

### Load Balancer - SSL Certificates
- The load balancer uses an X.509 certificate (SSL/TLS server certificate)
- You can manage certificate using ACM (AWS Certificate Manager)
- You can create and upload your own certificate 
- HTTPS listner:
  - You must specify a default certificate
  - You can add an optional list of certs to support multiple domains
  - Clients can use SNI (Server Name Indication) to specify the hostname they reach
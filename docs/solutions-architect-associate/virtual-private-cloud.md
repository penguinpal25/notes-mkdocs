# Virtual Private Cloud (VPC)
- Regional resource
- Soft limit of 5 VPCs per region
- Only the Private IPv4 ranges are allowed

> New EC2 instances are launched into the default VPC if no subnet is specified

## Classless Inter-Domain Routing (CIDR)
- Way to define a range of IP addresses

<img src=./images/cidr.jpg width="500"/>

- Two parts
  - Base IP - 192.168.0.0
  - Subnet Mask (defines how many bits are frozen from the left side) - /16

- Private IP ranges:
  - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) ⇒ used in big networks (24 bits can change)
  - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) ⇒ AWS default VPC
  - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) ⇒ home networks
- Rest of the IP ranges are Public
- Max 5 CIDR per VPC
  - Min. size is /28 (16 IP addresses)
  - Max. size is /16 (65536 IP addresses)
- Your VPC CIDR should not overlap with your other networks

## Subnets
- Sub-ranges of IP addresses within the VPC
- Each subnet is bound to an AZ
- Default VPC only has public subnets (1 public subnet per AZ, no private subnet)
- AWS reserves **5 IP addresses (first 4 & last 1)** in each subnet. These 5 IP addresses are not available for use. Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are 
  - 10.0.0.0 - Network address
  - 10.0.0.1 - reserved for VPC router
  - 10.0.0.2 - reserved for mapping to Amazon provided DNS
  - 10.0.0.3 - reserved for future use
  - 10.0.0.255 - Network broadcast address

> To make the EC2 instances running in private subnets accessible on the internet, place them behind an internet-facing (running in public subnets) Elastic Load Balancer.
> There is no concept of Public and Private subnets. Public subnets are subnets that have: - “Auto-assign public IPv4 address” set to “Yes” - The subnet route table has an attached Internet Gateway. This allows the resources within the subnet to make requests that go to the public internet. A subnet is private by default. 
> Since the resources in a private subnet don't have public IPs, they need a NAT gateway for address translation to be able to make requests that go to the public internet. NAT gateway also prevents these private resources from being accessed from the internet.

## Internet Gateway (IGW)
- Allows resources in a VPC to connect to the Internet

<img src=./images/igw.jpg width="500"/>

- Attached to the VPC
- Should be used to connect public resources to the internet (use NAT gateway for private resources since they need network address translation)
- Route table of the public subnets must be edited to allow requests destined outside the VPC to be routed to the IGW

<img src=./images/igwrt.jpg width="500"/>

## Bastion Hosts
- A EC2 instance running in the public subnet (accessible from public internet), to allow users to SSH into the instances in the private subnet.

<img src=./images/bastion.jpg width="300"/>

- Security groups of the private instances should only allow traffic from the bastion host.
- Bastion host should only allow port 22 traffic from the IP address you need

## Network Address Translation (NAT) Instance
- An EC2 instance launched in the public subnet which performs network address translation to enable private instances to use the public IP of the NAT instance to access the internet. This is exactly the same as how routers perform NAT. This also prevents the private instances from being accessed from the public internet.

<img src=./images/nat.jpg width="300"/>

- Must disable EC2 setting: **source / destination check** on the NAT instance as the IPs can change.
- Must have an Elastic IP attached to it
- Route Tables for private subnets must be configured to route internet-destined traffic to the NAT instance (its elastic IP)
- Can be used as a Bastion Host
- Disadvantages
  - Not highly available or resilient out of the box. Need to create an ASG in multi-AZ + resilient user-data script
  - Internet traffic bandwidth depends on EC2 instance type
  - You must manage Security Groups & rules:
    - Inbound:
      - Allow HTTP / HTTPS traffic coming from Private Subnets
      - Allow SSH from your home network (access is provided through Internet Gateway)
    - Outbound:
      - Allow HTTP / HTTPS traffic to the Internet

<img src=./images/natintance.jpg width="500"/>

## NAT Gateway
- AWS managed NAT with bandwidth autoscaling (up to 45Gbps)
- Uses an Elastic IP and Internet Gateway behind the scenes 
- Preferred over NAT instances
- Created in a public subnet
- Bound to an AZ 
- Cannot be used by EC2 instances in the same subnet (only from other subnets)
- Cannot be used as a Bastion Host
- Route Tables for private subnets must be configured to route internet-destined traffic to the NAT gateway

<img src=./images/natgwrt.jpg width="500"/>

- No Security Groups to manage
- Pay per hour

<img src=./images/natgwarch.jpg width="500"/>

- High availability
  - Create NAT gateways in public subnets bound to different AZ all routing outbound connections to the IGW (attached to the VPC)
  - No cross-AZ failover needed because if an AZ goes down, all of the instances in that AZ also go down.

<img src=./images/natgwha.jpg width="400"/>

## Network Access Control List (NACL)
- NACL is a firewall at the subnet level
- One NACL per subnet but a NACL can be attached to multiple subnets
- New subnets are assigned the Default NACL
- Default NACL allows all inbound & outbound requests

<img src=./images/nacldefault.jpg width="500"/>

- NACL Rules
  - Based only on IP addresses
  - Rules number: 1-32766 (lower number has higher precedence)
  - First rule match will drive the decision
  - The last rule denies the request (only when no previous rule matches)
- NACL vs Security Group 
  - NACL
    - Firewall for subnets
    - Supports both Allow and Deny rules
    - Stateless (both request and response will be evaluated against the NACL rules)
    - Only the first matched rule is considered
  - Security Group
    - Firewall for EC2 instances
    - Supports only Allow rules
    - Stateful (only request will be evaluated against the SG rules)
    - All rules are evaluated
  
  <img src=./images/nacl.jpg width="500"/>

- NACL with Ephemeral Ports
  - In the example below, the client EC2 instance needs to connect to DB instance. Since the ephemeral port can be randomly assigned from a range of ports, the Web Subnets’s NACL must allow inbound traffic from that range of ports and similarly DB Subnet’s NACL must allow outbound traffic on the same range of ports.

    <img src=./images/ephe.jpg width="500"/>

## VPC Peering
- Connect two VPCs (could be in **different region or account**) using the AWS private network
- Participating VPCs must have non-overlapping CIDR
- VPC Peering connection is **non-transitive** (A - B, B - C != A - C) because it works based on route-table rules. 
  
  <img src=./images/peering.png width="300"/>

- **Must update route tables in each VPC’s subnets to ensure requests destined to the peered VPC can be routed through the peering connection**

  <img src=./images/peer1.jpg width="500"/>
  <img src=./images/peer2.jpg width="500"/>

- You can reference a security group in a peered VPC across account or region. This allows us to use SG instead of CIDR when configuring rules.

> VPC Peering does not facilitate centrally-managed VPC like VPC Sharing

## VPC Endpoints
- Allows you to connect to AWS service using a **private network**  instead of using public internet (cheaper)
- Powered by AWS PrivateLink
- Route table is updated automatically
- Bound to a region (do not support inter-region communication)
- They remove the need of IGW, NATGW,... to access AWS services
- In case of issues:
  - Check DNS setting Resolution in VPC
  - Check Route tables
- Two types:
  - **Interface Endpoint**
    - Provisions an ENI (private IP) as an entry point per subnet
    - Need to attach a security group to the interface endpoint to control access
    - Supports most AWS services
    - Paid
  - **Gateway Endpoint** 
    - Provisions a gateway
    - Must be used as a target in a route table
    - Supports only **S3 and DynamoDB**
    - Free
  - Gateway is preferred all the time at exam. Interface Endpoint is preferred when access is required from on-premises (Site to Site VPN or Direct Connect), a different VPC or a different region

## VPC Flow Logs
- Captures information about IP traffic going into your interfaces
  - VPC Flow Logs
  - Subnet Flow Logs
  - ENI Flow Logs
- Helps to monitor and troubleshoot connectivity issues
- Flow logs data can be sent to S3 (bulk analytics) or CloudWatch Logs (near real-time decision making)
- Query VPC flow logs using Athena in S3 or CloudWatch Logs Insights
- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache...
- Syntax
  
  <img src=./images/flowlogs.png width="700"/>

## IPv6 Support
- IPv4 cannot be disabled for your VPC
- Every IPv6 address is public and Internet-routable (no private range)
- Enable IPv6 to operate in dual-stack mode in which your EC2 instances will get at least a private IPv4 and a public IPv6. They can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway.

  <img src=./images/ipv6.jpg width="300"/>

- If you cannot launch an EC2 instance in your subnet, It’s not because it cannot acquire an IPv6 (the space is very large). It’s because there are no available IPv4 in your subnet. **Solution: Create a larger IPv4 CIDR for the subnet**

## Egress-only Internet Gateway
- Allows instances in your VPC to initiate outbound connections over IPv6 while preventing inbound IPv6 connections to your private instances.
- Similar to NAT Gateway but for IPv6
- Must update Route Tables

<img src=./images/ipv6route.jpg width="500"/>

## Networking Costs
- **Inter-AZ and Inter-Region Networking**
  - Traffic entering the AWS is free
  - Traffic leaving an AWS region is paid
  - Use Private IP instead of Public IP for good savings and better network performance
  - Use same AZ for maximum savings (at the cost of availability) 
    
    <img src=./images/costaz.jpg width="500"/>

- **Minimizing Egress Traffic**
  - Try to keep as much internet traffic within AWS to minimize costs
  - **Direct Connect location that are co-located in the same AWS Region result in lower cost for egress network**
    
    <img src=./images/egresscost.jpg width="500"/>

- **S3 Data Transfer**
  - S3 ingress (uploading to S3): free
  - S3 to Internet: $0.09 per GB
  - S3 Transfer Acceleration: 
    - Additional cost on top of Data Transfer (+$0.04 to $0.08 per GB)
  - S3 to CloudFront: free (internal network)
  - CloudFront to Internet: $0.085 per GB (slightly cheaper than S3)
    - Caching capability (lower latency)
    - Reduced costs of S3 Requests (cheaper than just S3)
  - S3 Cross Region Replication: $0.02 per GB

    <img src=./images/s3cost.jpg width="500"/>
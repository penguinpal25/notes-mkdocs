# Region
- **Cluster of Availability Zones** (usually 3, min 2, max 6) within a geographic region
- Ex: **us-east-1, ap-south-1**
- The regions are interconnected each other using AWS private network
- Some AWS Service like Route53 is a Global service and some like EC2 are region specific
- AWS [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
- Consideration when selecting a region
  - Compliance (some countries require the data to be present in a data centre present in that country by law)
  - Proximity to customers: reduced latency
  - Available services within a Region: new services and new features aren’t available in every Region
  - Pricing: pricing varies region to region and is transparent in the service pricing page

# Availability Zone (AZ)
- Each AZ is one or more discrete data centers with redundant power, networking, and connectivity
- Ex: **us-east-1a, us-east-1b & us-east-1c**
- AZs are separated from each other (isolated from disasters)
- They’re connected with high bandwidth, ultra-low latency networking
> AZ name (eg. us-east-1a) is linked to an AWS account. Same AZ name for two AWS accounts might not refer to the same physical AZ. Use AZ ID (unique ID for each AZ) to coordinate AZ across accounts.

# AWS Points of Presence (Edge Locations)
- Content is delivered to end users with low latency



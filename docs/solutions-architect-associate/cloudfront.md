# CloudFront
- Global service
- Global Content Delivery Network (CDN)
- Edge Locations are present outside the VPC
- Supports HTTP/RTMP protocol (does not support UDP protocol)
- Caches content at edge locations, reducing load at the origin
- Geo Restriction feature
- Improves performance for both cacheable content (such as images and videos) and dynamic content (such as API acceleration and dynamic site delivery)
- To block a specific IP at the CloudFront level, deploy a WAF on CloudFront
- Supports Server Name Indication (SNI) to allow SSL traffic to multiple domains
- DDOS protection, Integration with Shield, AWS WAF

## Origin
- **S3 Bucket**
  - For distributing static files
  - Origin Access Identity (OAl) allows the S3 bucket to only be accessed by CloudFront
  - Origin Access Control (OAC) is replacing OAI
  - Can be used as ingress to upload files to S3

  <img src=./images/s3origin.png width="500"/>

- **Custom Origin (for HTTP)**
  - need to be publicly accessible on HTTP by public IPs of edge locations
  - EC2 Instance
  - ELB
  - S3 Website (may contain client-side script)
  - On-premise backend
> To restrict access to ELB directly when it is being used as the origin in a CloudFront distribution, create a VPC Security Group for the ELB and use AWS Lambda to automatically update the CloudFront internal service IP addresses when they change.

### ALB or EC2 as origin

<img src=./images/albec2origin.png width="500"/>

## Geo Restriction
- You can restrict who can access your distribution
  - Allowlist: Allow your users to access the content only if they are on the list of approved countries
  - Blocklist: Prevent your user from accessing the content if they are on the list of banned countries
- The "country" is determined using 3rd-party Geo-IP database
- Use case: Copyright laws to control access to content
- If you are on the list of countries that are banned from accessing the distribution, you should see the error shown below.
> 403 ERROR The request could not be satisfied.

## Price classes
- The cost of data out per edge location varies
- You can reduce the number of edge locations for cost reduction
- Three price class:
  - Price Class All: all regions (best performance)
  - Price Class 200: most regions (excludes the most expensive regions)
  - Price Class 100: only the least expensive regions

## Cache invalidation
- CloudFront doesn't know about backend origin update and will only get the refreshed content after the TTL has expired
- You can force an entire or partial cache refresh (thus bypassing the TTL) by creating a cache invalidation
- You can invalidate all files (*) or a special path (/images/)

<img src=./images/cachein.png width="400"/>

## Edge Function
- A code that you write and attach to CloudFront distributions
- Runs close to your users to minimize latency
- Two types:
  - CloudFront Functions
  - Lambda@Edge
- No need to manage any servers, deployed globally
- Pay only for what you use
- Fully serverless
- Use case: Customize CDN content

### CloudFront Functions
- Lightweight functions written JS
- For high-scale, latency-sensitive CDN customizations
- Sub ms startup times, millions of requests/second
- Used to change viewer requests and responses:
  - Viewer Request: after CloudFront receives a request from viewer
  - Viewer Response: before CloudFront forwards the response to the viewer
- Uses cases:
  - Cache key normalization (Transform request attributes (headers, cookies, query strings, URL) to create an optimal cache key)
  - Header manipulation (Insert/Modify/Delete HTTP headers in the request or response)
  - URL rewrites or redirects
  - Request authentication & authorization (Create and validate user-generated tokens (eg; JWT) to allow/deny requests)

## CloudFront Functions vs Lambda@Edge

<img src=./images/cflm.png width="500"/>


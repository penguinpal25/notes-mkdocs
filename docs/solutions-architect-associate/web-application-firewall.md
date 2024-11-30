# Web Application Firewall (WAF)
- Protects your application from common **layer 7**(HTTP) web exploits such as SQL Injection and Cross-Site Scripting (XSS)
- Can only be deployed on
  - Application Load Balancer
  - API Gateway
  - CloudFront
  - AppSync GraphQL API
  - Cognito user pool
- WAF contains Web ACL (Access Control List) containing rules to filter requests based on:  
  - IP addresses
  - HTTP headers
  - HTTP body
  - URI strings
  - Size constraints (ex. max 5kb)
  - Geo-match (block countries)
  - Rate-based rules (to count occurrences of events per IP) for DDoS protection
- A rule group is a **reusable set of rules that you can add to a web ACL**
- Web ACL are regional except CloudFront
- Layer 7 has more data about the structure of the incoming request than layer 4 (used by AWS Shield)

## Fixed IP while using WAF with a LB
- WAF doesn't support NLB (layer 4)
- We can use Global Accelerator for fixed IP and WAF on ALB

<img src=./images/waf.png width="500"/>
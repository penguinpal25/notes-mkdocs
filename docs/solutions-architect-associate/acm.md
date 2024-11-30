# AWS Certificate Manager (ACM)
- Provision, manage, and deploy **TLS certificates**
- Provide in-flight encryption (HTTPS)
- Supports both public (free of charge) and private TLS certificates
- Automatic TLS certificate renewal
- Load TLS certificates on
  - ELB (CLB, ALB, NLB)
  - CloudFront distributions
  - APIs on API gateway
- Can't use ACM with EC2

## Requesting Public certificates
1. List domain names to be included in certificate
    - FQDN: test.example.com
    - Wildcard domain: *.example.com
2. Select validation method: DNS validation or Email validation
    - DNS validation is preferred for automation purpose
    - Email validation will sent email to contact address in the WHOIS database
    - DNS validation will leverage a CNAME record to DNS config 
3. It will take few hours to get verified
4. The public certificates will be enrolled for automatic renewal
    - ACM automatically renews ACM generated certificates 60days before expiry

## Importing Public certificates
- Option to generate certificate outside of ACM and then import it
- **No automatic renewal**, must import a new certificate before expiry
- **ACM sends daily expiration events** starting 45 days prior to expiration
  - The number of days can be configured
  - Events are appearing in EventBridge
  - AWS config has a managed rule (*acm-certificate-expiration-check*)to check for expiring certificates

## Integration with API Gateway
- Create a **Custom domain name** in API Gateway
- Edge-optimized (default): for global clients
  - The TLS certificate must be in the same region as CloudFront, us-east-1
  - Setup a CNAME or (better) A-Alias record in Route53
- Regional
  - The TLS certificate must be imported on API Gateway, in the same region as the API stage
  - Setup a CNAME or (better) A-Alias record in Route53

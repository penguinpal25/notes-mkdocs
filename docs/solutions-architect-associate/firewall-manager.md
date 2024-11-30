# Firewall Manager
- Manage firewall rules across all the accounts of an AWS Organization 
- Common set of security rules:
  - WAF rules (ALB, API Gateways, CloudFront)
  - AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
  - Security Groups for EC2, ALB, and ENI resources in VPC
  - AWS Network Firewall (VPC Level)
  - Amazon Route 53 Resolver DNS Firewall
  - Policies are created at Region level
> Does not support NACL as of now

## WAF vs Firewall Manager vs Shield
- **WAF, Shield, and Firewall Manager are used together for comprehensive protection**
- For granular protection of your choice, WAF alone is the correct choice
- If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF
- Shield advanced adds additional features on top of AWS WAF, such as dedicated support from Shield Response Team (SRT) and advanced reporting

# Amazon Inspector
- Detect vulnerabilities in
  - EC2 instances using System Manager (SSM) Agent running on EC2 instances
  - Amazon ECR - Assessment of containers as they are pushed to ECR
  - Lambda functions - Identifies software vulnerabilities in function code and package dependencies
- Integration with AWS Security Hub
- Send findings to EventBridge 
- Gives a risk score associated with all vulnerabilities for prioritization
- Detects vulnerabilities which could cause threats (detected by GuardDuty)
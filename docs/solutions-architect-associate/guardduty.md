# Amazon GuardDuty
- Intelligent threat discovery using ML to protect AWS account
- No management required (just enable)
- Input data includes:
  - CloudTrail Logs (unusual API calls, unauthorized deployments)
  - VPC Flow Logs (unusual internal traffic, unusual IP address)
  - DNS Logs (compromised EC2 instances sending encoded data within DNS queries)
  - EKS Audit Logs (suspicious activities and potential EKS cluster compromises)
- Setup **EventBridge rules** to target AWS Lambda or SNS for automation
- **Can protect against CryptoCurrency attacks (has a dedicated "finding" for it)**
- Disabling GuardDuty will delete all remaining data, including your findings and configurations

<img src=./images/guardduty.png width="500"/>
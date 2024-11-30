# SSM Parameter Store
- Serverless
- Used to store configuration and secrets
- Parameter versioning
- Seamless Encryption with KMS for encryption and decryption of stored secrets
- Parameters are stored in hierarchical fashion
- Security through IAM
- Notifications with Amazon Event Bridge
- Integration with CloudFormation

## Tiers

|   | Standard Tier  | Advanced Tier  |
|---|---|---|
| Number of parameters  |  10,000 | 100,000  |
| Max parameter size  |  4KB | 8KB  |
|  Parameter Policy | Not supported  | Supported  |
|  Cost | Free  |  Paid |

## Parameter Policies
- Only supported in advanced tier
- Assign policies to a parameter for additional features
  - Expire the parameter after some time (TTL)
  - Parameter expiration notification
  - Parameter change notification
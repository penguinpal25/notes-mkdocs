- Centrally manage and automate backups of AWS services across regions and accounts
- Fully managed service
- Supported services:
  - EC2 / EBS
  - S3
  - RDS / Aurora / DynamoDB
  - DocumentDB / Neptune
  - EFS / FSx (Lustre & Windows)
  - Storage Gateway (Volume Gateway)
- PITR for supported service
- On-demand and scheduled backups
- Tag based backup policies
- Create backup policies known as backup plans
- Backup is storing in an internal bucket of S3

## AWS Backup Vault Lock
- WORM (Write Once Read Many) model for backups
- Even the root user cannot delete backups
- Additional layer of defense to protect your backups against:
  - Inadvertent or malicious delete operations
  - Updates that shorten or alter retention periods
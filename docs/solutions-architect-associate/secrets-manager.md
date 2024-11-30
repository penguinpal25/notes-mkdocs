# Secrets Manager
- For storing secrets only
- **Ability to force rotation of secrets every n days (not available in Parameter Store)**
- A secret consists of multiple key-value pairs
- Secrets are encrypted using KMS 
- Mostly used for **RDS authentication**
  - need to specify the username and password to access the database
  - link the secret to the database to allow for automatic rotation of database login info

## Multi-region secrets
- Replicate secrets across multiple AWS regions
- Keeps read replicas in sync with primary secret
- Ability to promote a read replica secret to a standalone secret
- Uses cases: multi-region apps, DR strategies, multi-region DB
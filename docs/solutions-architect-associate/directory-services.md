# AWS Directory Services
## What is Microsoft Active Directory (AD)?
- Found on any Windows server with AD domain services
- Database of **objects**: User, Accounts, Computers, Printers, File Shares, Security Groups
- Centralized security management, create account, assign permissions
- Objects are organized in **trees**
- A group of trees is a **forest**

## AWS Managed Microsoft AD
- Create your own AD in AWS, manage users locally, supports MFA
- Establish "trust" connections with your on-premise AD
- Integration is out of the box to connect IAM Identity center with AWS Managed Microsoft AD
- To connect to a Self-managed directory with IAM Identity center, create a two way Trust relatioship using AWS Managed Microsoft AD

## AD Connector
- Directory Gateway (proxy) to redirect to on-premise AD, supports MFA
- Users are managed on on-premise AD
- To connect to a Self-managed directory with IAM Identity center, create an AD 

## Simple AD
- AD-compatible managed directory on AWS
- Cannot be joined with on-premise AD

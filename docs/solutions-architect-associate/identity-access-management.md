# Identity & Access Management (IAM)
- Global Service (IAM entities like roles can be used in any region without recreation)

# Users & Groups
- Users are people in your organization and can be grouped
- Groups are collections of users and have policies attached to them
- Groups cannot be nested
- User can belong to multiple groups
- User doesn't have to belong to a group
- **Root User** has full access to the account
- **IAM User** has limited permission to the account
- You should log in as an IAM user with admin access even if you have root access. This is just to be sure that nothing goes wrong by accident.
> - An IAM Group is not an identity and cannot be identified as a principal in an IAM policy
> - Only users and services can assume a role (not groups)
> - A new IAM user created using the AWS CLI or AWS API has no AWS credentials

# Policies
- Policies are JSON documents that outline permissions for users, groups or roles
- Policies assigned to a user are called inline policies
- Follow least privilege principle for IAM Policies
- Two types
  - **User based policies**
    - IAM policies define which API calls should be allowed for a specific user
  - **Resource based policies**
    - Control access to an AWS resource
    - Grant the specified principal permission to perform actions on the resource and define under what conditions this applies
- An IAM principal can access a resource if the user policy ALLOWS it OR the resource policy ALLOWS it AND there’s no explicit DENY.
- [Policy Simulator](https://policysim.aws.amazon.com/)
  - Online tool that allows us to check what API calls an IAM User, Group or Role is allowed to perform based on the permissions they have.
- Policy Structure

<img src=./images/iampolicystructure.jpg width="700"/>

## Trust Policies
- Defines which principal entities (accounts, users, roles, federated users) can assume the role
- An IAM role is both an identity and a resource that supports resource-based policies.
- You must attach both a trust policy and an identity-based policy to an IAM role.
- The IAM service supports only one type of resource-based policy called a role trust policy, which is attached to an IAM role.

# Multi Factor Authentication (MFA)
- We can set a password policy in AWS account and it can be used to enforce standards for password and to prevent brute force attack
  - password rotation
  - password reuse

- MFA = password you know + security device you own
- Main benefit of MFA is, even if password is stolen or hacked then the account will not be compromised
- MFA device options in AWS
  - Virtual MFA device
    - Google Authenticator (phone only)                                             
    - Authy (multi-device) 
    - Both support multiple tokens on a single device 
  - Universal 2nd Factor (U2F) Security Key
    - YubiKey by Yubico (3rd party)
    - Support multiple root and IAM users using a single security key
  - Hardware Key Fob MFA device
    - Gemalto (3rd party)
  - Hardware Key Fob MFA device for AWS GovCloud (US)
    - SurePassID (3rd party)

# Access Keys, CLI & SDK
## AWS Access Key
- Need to use access keys for AWS CLI and SDK
- Don't share access keys with anyone (every user can generate their own access keys)
- Access keys are only shown once and if you lose them you need to generate a new access key
- Access Key ID ~ username
- Secret Access Key ~ password
## AWS CLI
- Open-source tool to interact with AWS services using commands
- Direct access to public APIs of AWS services
- The permissions will be same as of permissions set in AWS Console for the user
- Ex: *$ aws iam list-users* 
## AWS SDK 
- Language specific APIs to access and manage AWS services programmatically
- Supports
  - SDKs (JavaScript, Python, PHP, .NET, Java, Ruby, Go, Node.js, C++)
  - Mobile SDKs (Android, iOS)
  - IoT Device SDKs (Embedded C, Arduino)
- AWS CLI is built on AWS SDK for Python called **Boto**
## AWS CloudShell
- Browser based shell that gives command line access to AWS services.
- AWS CLI, Python, Node.js and more are pre-installed
- 1 GB of storage free per AWS region
- Files saved in your home directory are available in future sessions for the same AWS region
- The default region will be the one from which you've accessed CloudShell
- Can download and upload files

# Roles
- Collection of policies for AWS services
> If you are going to use an IAM Service Role with Amazon EC2 or another AWS service that uses Amazon EC2, you must store the role in an instance profile (An instance profile is a container for an IAM role that you can use to pass role information to an EC2 instance when the instance starts). When you create an IAM service role for EC2, the role automatically has EC2 identified as a trusted entity.

# Reporting Tools
- **Credentials Report**
  - lists all the users and the status of their credentials (MFA, password rotation, etc.)
  - account level 
  - used to audit security for all the users
- **Access Advisor**
  - shows the service permissions granted to a user and when those services were last accessed
  - user-level
  - used to revise policies for a specific user

# Guidelines
- Use root account only for account setup
- 1 physical user = 1 AWS user
- Enforce MFA for both root and IAM users
- Never share IAM credentials & Access Keys
- Create a strong password policy
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for programmatic access 
- Assign users to groups and assign permission to groups

# Permission Boundaries
- Set the maximum permissions an IAM entity can get
- Can be applied to users and roles (not groups)
- Used to ensure some users can’t escalate their privileges (make themselves admin)
- Ex: suppose a user has "AdministratorAccess" policy attached and Permissions boundary is set to "AmazonS3FullAccess", then the user can only access S3.

# Assume Role vs Resource-based Policy
- When you assume an IAM Role, you give up your original permissions and take the permissions assigned to the role
- When using a resource based policy, the principal doesn’t have to give up their permissions

# AWS Organizations
- Global service
- Allows to manage multiple accounts
- The main account is the management account and other accounts are member accounts
- Member accounts can only be part of one organization
- Consolidated billing across all accounts - single payment method
- Pricing benefit from aggregated usage (volume discount on EC2, S3...)
- **Shared reserved instances and Savings plans discounts across accounts**
- API is available to automate AWS account creation

<img src=./images/org.png width="500"/>

- **Advantages**
  - Use tagging standards for billing purposes
  - Enable CloudTrail on all accounts, send logs to central S3 account
  - Send CloudWatch logs to central logging account
  - Establish Cross account roles for admin purposes

- **Organizational Unit (OU)**

  <img src=./images/ou.png width="500"/>

- **Security: Service Control Policies (SCP)**
  - IAM policies applied to OU or account to restrict Users and Roles
  - They do not apply to management account (full admin power)
  - Must have an explicit allow

- **SCP Hierarchy**

  <img src=./images/scp.png width="500"/>

  - Management account
    - Can do anything (no SCP apply)
  - Account A
    - Can do anything
    - EXCEPT access Redshift (Explicit deny from OU)
  - Account B
    - Can do anything
    - EXCEPT access Redshift (Explicit deny from Prod OU)
    - EXCEPT access Lambda (Explicit deny from HR OU)
  - Account C
    - Can do anything
    - EXCEPT access Redshift (Explicit deny from Prod OU)

# IAM Conditions
- **aws:SourceIp**: restrict the client IP **from** which the API calls are being made
- **aws:RequestedRegion**: restrict the region the API calls are made **to**

  <img src=./images/condition1.png width="700"/>

- **ec2:ResourceTag**: restrict based on **tags**
- **aws:MultiFactorAuthPresent**: to force MFA

  <img src=./images/condition2.png width="700"/>

- **IAM for S3**
  - s3:ListBucket permission applies to arn:aws:s3:::test => Bucket level permission
  - s3:GetObject, s3:PutObject, s3:DeleteObject applies to arn:aws:s3:::test/* => Object level permission

  <img src=./images/s3iam.png width="300"/>

- **aws:PrincipalOrgID**: Can be used in any resource policies to restrict access to accounts that are member of an AWS Organization

  <img src=./images/principalid.png width="700"/>

# IAM Roles vs Resource based policy
- Cross account:
  - attaching a resource based policy to a resource (ex: S3 bucket policy)
  - OR using a Role as a proxy
- **When you assume a role (user, application, or service), you give up your original permissions and take the permissions assigned to the role**
- When using resource-based policy, the principal doesn't have to give up his permissions
- Amazon EventBridge - Security
  - When a rule runs, it needs permissions on target
  - Resource based policy: Lambda, SNS, SQS, CloudWatch, Logs, API Gateway...
  - IAM Role: Kinesis Stream, Systems Manager run command, ECS task...

  <img src=./images/eventbridgesec.png width="300"/>

# AWS IAM Identity Center
- Successor to AWS Single Sign-On
- One login for all your 
  - **AWS accounts in AWS Organization**
  - Business cloud applications (ex; salesforce, Box, Microsoft 365,...)
  - SAML2.0-enabled applications
  - EC2 Windows instances

<img src=./images/identitycenter.png width="500"/>

- **Multi-Account permissions**
  - Manage access across AWS accounts in your AWS Organization
  - Permission sets: a collection of one or more IAM policies assigned to users and groups to define AWS access
- **Application assignments**
  - SSO access to many SAML 2.0 business applications (ex; salesforce, Box, Microsoft 365,...)
  - Provide required URLs, certificates, and metadata
- **Attribute-based Access Control (ABAC)**
  - Fine-grained permissions based on user's attributes stored in IAM Identity Center Identity Store
  - Ex: cost center, title, locale,...
  - Use case: Define permission once, then modify AWS access by changing the attributes

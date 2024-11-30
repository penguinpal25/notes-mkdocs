# Systems Manager
- AWS Systems Manager is the operations hub for your AWS applications and resources and a secure end-to-end management solution for hybrid cloud environments that enables secure operations at scale.

## Run Command
- Execute a script or just run a command across multiple instances (using resource groups) without the need of SSH
- Command output can be shown in AWS Console, sent to S3 bucket or CloudWatch Logs
- Send notifications to SNS about command status (In progress, Success, Failed...)
- Integrated with IAM & CloudTrail
- Can be invoked using EventBridge

<img src=./images/run.png width="300"/>

## Patch Manager
- Automates the process of patching like OS Updates, applications updates, security updates,... on EC2 instances and on-premises servers
- Support Linux, MacOS, and Windows
- Patch on-demand or on a schedule using **Maintenance Windows**
- Scan instances and generate patch compliance report (missing patches)

## Maintenance Windows
- Defines a schedule for when to perform actions on your instances
- Ex: OS patching, updating drivers, installing software,...
- Contains:
  - Schedule
  - Duration
  - Set of registered instances
  - Set of registered tasks

## Automation
- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- Ex: Restart instances, create AMI, EBS snapshot
- **Automation Runbook**: SSM Documents to define actions performed on your EC2 instances or AWS resources (pre-defined or custom)
- Can be triggered using
  - Manually using AWS Console, AWS CLI, or SDK
  - Amazon EventBridge
  - On a schedule using Maintenance Windows
  - By AWS Config for rules remediations

<img src=./images/automation.png width="300"/>
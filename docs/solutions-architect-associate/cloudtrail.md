# CloudTrail
- **Provides governance, compliance and audit for the AWS Account**
- Enabled by default
- Get **a history of events/API calls made within your AWS account** by:
  - Console
  - SDK
  - CLI
  - AWS Services
- Can put logs from CloudTrail into S3 (encrypted by default using SSE-S3) or CloudWatch Logs
> Modifications to log files can be detected by enabling Log File Validation on the logging bucket
- **A trail can be applied to all regions (default) or a single region**
- If a resource is deleted in AWS, investigate CloudTrail first
- Event retention: 90 days (To keep events beyond this period, log them to S3 and use Athena)

## Event Types
- **Management Events**
  - Events of operations that modify AWS resources. Ex:
    - Creating a new IAM user
    - Deleting a subnet
  - **Enabled by default**
  - Can separate Read Events (that don’t modify resources) from Write Events (that may modify resources)
- **Data Events**
  - Events of operations that modify data
    - S3 object-level activity
    - Lambda function execution
  - **Disabled by default (due to high volume of data events)**
  - Can seperate Read and Write events
- **Insight Events**
  - **Enable CloudTrail Insights to detect unusual activity in your account**
    - inaccurate resource provisioning
    - hitting service limits
    - bursts of AWS IAM actions
    - gaps in periodic maintenance activity
  - CloudTrail Insights analyzes normal management events to create a baseline and then continuously analyzes write events to detect unusual patterns. If that happens, CloudTrail generates insight events that
    - show anomalies in the Cloud Trail console
    - can can be logged to S3
    - can trigger an EventBridge event for automation
# SSM Session Manager
- Allows you to start a secure shell on your EC2 and on-premise servers **without SSH keys or a bastion host**
- **No port 22 needed (better security)**
- Support Linux, MacOS, and Windows
- Send session log data to S3 or CloudWatch logs
- Sessions are secured using an AWS Key Management Service key
- IAM instance profile needs to attach to the instance with the policy *AmazonSSMMangedInstanceCore*

<img src=./images/sessionmanager.png width="300"/>
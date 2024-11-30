# Elastic Container Service (ECS)
- Used to launch Docker containers on AWS = Launch ECS tasks on ECS clusters
- Integrates with ALB for load balancing to ECS tasks
- EFS is used as persistent multi-AZ shared storage for ECS tasks

## Launch Types
- **EC2 Launch Type**
  - You must provision & maintain EC2 instances (use ASG)
  - Each EC2 instance must contain ECS Agent to register in the ECS cluster
  - AWS take care of starting and stopping containers
  - Containers run on underlying EC2 instances
  - Not Serverless

  <img src=./images/ecs1.png width="300"/>

- **Fargate Launch Type**
  - Serverless
  - No need to provision infrastructure
  - No need to worry about infrastructure scaling
  - You just create task defenitions
  - ECS launches the required containers based on the CPU / RAM needed (we won’t know where these containers are running)

  <img src=./images/ecs2.png width="300"/>

  ## IAM Roles for ECS Tasks
  - **EC2 Instance Profile** (EC2 Launch type only)
    - Used by the ECS agent to:
      - Make API calls to ECS service
      - Send container logs to Cloud Watch
      - Pull Docker image from ECR
      - Reference sensitive data in Secrets Manager or SSM Parameter Store

  <img src=./images/ecs3.png width="300"/>

  - **ECS Task Role**
    - Each task can have a specific role
    - Allows ECS tasks to access AWS resources
    - Task Role is defined in the task definition
    - Use different roles for the different ECS Services
    - Use *taskRoleArn* parameter to assign IAM policies to ECS Task Role

## Load Balancing
- Application Load Balancer: Supported and works for most uses cases
- Network Load Balancer: Recommended only for high throughput/high performance use cases, or to pair it with AWS Private Link
- Elastic Load Balancer: Supported but not recommended (no advanced features, no Fargate support)

## Data Volumes
- Mount EFS file systems onto ECS tasks
- Works for both EC2 and Fargate launch type
- Fargate + EFS = Serverless
- Persistent multi-AZ shared storage for containers
> Amazon S3 cannot be mounted as a file system

<img src=./images/ecs4.png width="300">

## Auto-scaling
### Fargate Launch type
- Automatically increase/decrease the desired number of ECS tasks
- Uses AWS Application Auto Scaling
  - ECS Service Average CPU utilization
  - ECS Service Average Memory utilization - scale on RAM
  - ALB Request count per target - metric coming from the ALB
- Types:
  - Target tracking: Scale based on target value for a specific CloudWatch metric
  - Step scaling: Scale based on a specific CloudWatch alarm
  - Scheduled scaling: Scale based on a specific date/time (predictable changes)

- ECS Service auto scaling (Task level) != EC2 auto scaling (EC2 instance level)

### EC2 Launch type
- ECS Service scaling by adding underlying EC2 instances
- **Auto scaling Group scaling**
  - Scale ASG based on CPU utilization
  - Add EC2 instances over time
- **ECS Cluster Capacity provider**
  - Automatically provision and scale the infrastructure for your ECS tasks
  - Paired with ASG
  - Add EC2 instances when missing capacity (CPU, RAM...)

<img src=./images/ecs5.png width="500"/>

## ECS with EventBridge, EventBridge Schedule and SQS
- **with EventBridge**

  <img src=./images/ecs6.png width="500"/>

- **with EventBridge Schedule**

  <img src=./images/ecs7.png width="500"/>

- **with SQS**

  <img src=./images/ecs8.png width="500"/>
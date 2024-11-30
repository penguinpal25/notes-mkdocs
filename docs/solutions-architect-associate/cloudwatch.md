# CloudWatch
- Serverless performance monitoring service

## Metrics
- Variable to monitor in CloudWatch (CPUUtilization)
- **Dimension** is an attribute of a metric (instance id, environment, etc.)
- Up to 10 dimensions per metric
- Metric belong to **namespaces**
- Metrics have timestamps
- Can create Cloudwatch dashboards of metrics

## CloudWatch Metric Streams
- Continually stream CloudWatch metrics to a destination of your choice, with **near-real-time-delivery** and low latency
  - Amazon Kinesis Data Firehouse (and then to its destinations)
  - 3rd party service provider (Datadog, New Relic, Splunk...)
- Option to **filter metrics** to only stream a subset of them

## CloudWatch Logs
- Used to store application logs
- Logs Expiration: never expire, 30 days, etc.
- **Log groups**: arbitrary name, usually represening an application
- **Log stream**: instances within application/logs/containers
- Logs can be sent to:
  - S3 buckets (exports)
  - Kinesis Data Streams
  - Kinesis Data Firehose
  - Lambda functions
  - OpenSearch
- Logs sources:
  - SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
  - Elastic Beanstalk: collection of logs from application
  - ECS: collection from containers
  - AWS Lambda: collection from function logs
  - VPC Flow logs: VPC specific logs
  - API Gateway
  - CloudTrail based on filter
  - Route53: log DNS queries
- **Metric Filters** can be used to filter expressions and use the count to trigger CloudWatch alarms. Example filters:
  - find a specific IP in the logs
  - count occurrences of “ERROR” in the logs
- **Cloud Watch Logs Insights** can be used to query logs and add queries to CloudWatch Dashboards
- Logs can take **up to 12 hours** to become available for exporting to S3 (not real-time)
  - The API call is **CreateExportTask**
- To stream logs in real-time, apply a **Subscription Filter** on logs
- Logs from multiple accounts and regions can be aggregated using subscription filters

<img src=./images/cloudwatchlogs.jpg width="500"/>

> Metric Filters are a part of CloudWatch Logs (not CloudWatch Metrics)

## CloudWatch Logs for EC2
- By default, no logs from EC2 machine will go to CloudWatch and you need to run a CloudWatch agent on EC2 to push the log files you want
- An IAM role should be applied to EC2 to send logs to cloudwatch
- The CloudWatch agent can be setup on-premises too
- EC2 instances have metrics **every 5 minutes**
- With detailed monitoring (for a cost), you get metrics **every 1 minute**
- **CloudWatch Logs Agent**
  - Old version of the agent
- **CloudWatch Unified Agent**
  - Collect additional system level metrics such as RAM, processes etc
  - Centralized configuration using SSM parameter store
  - CPU (active, guest, idle, system, user, steal)
  - Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
  - RAM (free, inactive, used, total, cached)
  - Netstat (number of TCP and UDP connections, net packets, bytes)
  - Processes (total, dead, bloqued, idle, running, sleep)
  - Swap Space (free, used, used %)

## CloudWatch Alarm
- Alarms are used to trigger notifications for any metric (can be based on Metric Filters too)
- Various options to trigger alarm (sampling, %, max, min, etc.)
- On a single metric
- Alarm States:
  - OK
  - INSUFFICIENT_DATA
  - ALARM
- Period
  - Length of time in seconds to evaluate the metric before triggering the alarm
  - High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec
- Targets
  - Stop, Terminate, Reboot, or Recover an EC2 Instance
  - Trigger Auto Scaling Action (ASG)
  - Send notification to SNS
- To test alarms and notifications, set the alarm state to ALARM using CLI
- **Composite Alarms**
  - **Monitoring the states of multiple other alarms**
  - Can use AND and OR conditions

  <img src=./images/compositealarm.png width="500"/>

## EC2 Instance Recovery
- Status check
  - Instance status: check the VM
  - System status: check the underlying hardware
- CloudWatch alarm to automatically recover an EC2 instance if it becomes impaired
- Terminated instances cannot be recovered
- After the recovery, the following are retained
  - Placement Group
  - Public IP
  - Private IP
  - Elastic IP
  - Instance ID
  - Instance metadata
- After the recovery, RAM contents are lost

## Container Insights
- Collect, aggregate, summarize **metrics and logs** from containers
- Available for containers on:
  - Amazon ECS
  - Amazon EKS
  - Kubernetes platforms on EC2
  - Fargate (both for ECS and EKS)
- **Using a containerized version of CloudWatch agent to discover containers on Amazon EKS and Kubernetes**

## Lambda Insights
- Monitoring and troubleshooting solution for serverless applications running on AWS Lambda
- Collects, aggregates, and summarizes system-level metrics including CPU time, memory, disk, and network
- Collects, aggregates, and summarizes diagnostic information such as cold starts and Lambda worker shutdowns
- Provided as a Lambda layer

## Contributor Insights
- Analyze log data and create time series that display contributor data
  - **Metrics about top-N contributors**
- Helps you find top talkers and understand who or what is impacting system performance
- For ex: You can find bad hosts, **identify the heaviest network users**, or find the URLs that generate the most errors.
- Can build rules from scratch or use sample rules that AWS generated

## Application Insights
- **Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues**
- Your applications run on Amazon EC2 Instances with select technologies only (Java, .NET, Microsoft IIS, databases...)
- Also can use other AWS resources such as Amazon EBS, RDS, ASG etc
- Powered by SageMaker
- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter
# EventBridge
- Formerly CloudWatch Events
- Schedule: Cron jobs
  - Ex: Schedule to trigger a script on Lambda function every hour
- Event pattern: Event rules to react to a service doing something
  - Ex: Sent a SNS topic with Email notification whenever there occur a root user login
- Event Rules: how to process the events

<img src=./images/eventbridgerules.png width="500"/>

- Event buses types:
  - **Default event bus**: events from AWS services are sent to this
  - **Partner event bus**: receive events from external SaaS applications
  - **Custom Event bus**: for your own applications
- Event buses support cross-account access using Resource-based policies
- You can **archive events** sent to an event bus (indefinitely or set period)
- Can **replay archived events**

## Schema Registry
- Defines how the data is structured in the event bus
- Schema can be versioned

## Resource-based policy
- Manage permissions for a specific Event bus
- Ex: allow/deny events from another AWS account or AWS region
- Use case: aggregate all events from your AWS Organization in a single AWS account or AWS region
# Simple Queue Service (SQS)
- Fully managed service
- Used to asynchronously decouple applications
- Supports multiple producers & consumers
- When the consumer has successfully processed a message, it is removed from the queue
- Unlimited number of messages in queue
- Max 10 messages per batch
- Consumers could be EC2 instances, Lambda functions or On-premises servers
- Producing messages using SendMessage API
- Delete messages using DeleteMessage API

## Queue Types
1. **Standard Queue**
- Unlimited throughput, unlimited number of messages in queue
- Low latency (<10 ms on publish and receive)
- Can have duplicate messages (at least once delivery, occasionally) and out of order messages (best effort ordering)
- Max message size: 256KB
- Default message retention: 4 days (max: 14 days)
2. **FIFO Queue**
- FIFO = First-In-First-Out (ordering of messages in the queue)
- Limited throughput
  - 300 msg/s without batching (batch size = 1)
  - 3000 msg/s with batching (batch size = 10)
- No duplicate messages
- The queue name must end with .fifo to be considered a FIFO queue
- Sending messages to a FIFO queue requires:
  - Group ID: messages will be ordered and grouped for each group ID
  - Message deduplication ID: for deduplication of messages


## Consumer Auto Scaling
- We can attach an ASG to the consumer instances which will scale based on the CW metric = Queue length / Number of EC2 instances. CW alarms can be triggered to step scale the consumer application.

<img src=./images/sqsasg.png width="500"/>

## SQS to decouple b/w application tiers

<img src=./images/sqsde.png width="500"/>

## SQS as a buffer to database writes
- If the load is too big, some transaction may be lost 

<img src=./images/sqsbuffer.png width="500"/>

## Encryption
- In-flight encryption using HTTPS API
- At-rest encryption using KMS keys
- Client-side encryption also supported

## Access Management
- IAM Policies to regulate access to the SQS API
- SQS Access Policies (resource based policy like bucket policy)
  - Used for cross-account access to SQS queues
  - Used for allowing other AWS services to send messages to an SQS queue

## Configurations
### Message Visibility Timeout
- Once a message is polled by a consumer, it becomes invisible to other consumers for the duration of message visibility timeout. After the message visibility timeout is over, the message is visible in the queue.
- If a consumer crashes while processing the message, it will be visible in the queue after the visibility timeout
- If a message is not processed within the visibility timeout, it will be processed again (by another consumer). However, a consumer could call the *ChangeMessageVisibility* API to change the visibility timeout for that specific message. This will give the consumer more time to process the message.
- Default: 30s
- Can be configured for the entire queue
  - High: if the consumer crashes, re-processing will take long
  - Low: may get duplicate processing of messages

<img src=./images/mvt.jpg width="500"/>

### Long Polling
- When a consumer requests messages from the queue, it can optionally wait for messages to arrive if there are none in the queue
- Decreases the number of API calls made to SQS (cheaper)
- Reduces latency (incoming messages during the polling will be read instantaneously) which will make the application efficient
- Wait time can be between 1 sec to 20 sec (20sec preferrable)
- Long Polling is preferred over Short Polling
- Can be enabled at the queue level or at the API level by using *WaitTimeSeconds*


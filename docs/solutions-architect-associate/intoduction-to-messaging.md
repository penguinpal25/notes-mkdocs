# Introduction to Messaging
- Deploying multiple applications need to communicate with each other
- Two patterns of application communication:
  - Synchronous communication (application to application)
  - Asynchronous/Event based communication (application to que to application)
- Synchronous between applications can be problematic if there are sudden spikes of traffic, in that case it's better to decouple your applications using:
  - SQS: queue model
  - SNS: pub/sub model
  - Kinesis: real-time streaming model
- These services can scale independently from our application

<img src=./images/message.png width="500"/>
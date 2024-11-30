# Lambda
- Function as a Service (FaaS)
- Serverless
- Auto-scaling (Lambda scales up and down automatically to handle your workloads, and you don't pay anything when your code isn't running.)
- Run on-demand
- Limited by time - short executions
- Pay per request (number of invocations) and compute time
- Integrated with CloudWatch for monitoring
- Easy to get more resources per function (upto 10GB of RAM)
- Not good for running containerized applications
- Can package and deploy Lambda functions as container images

## Performance
- Increasing RAM will improve CPU and network
- RAM: 128 MB - 10GB

## Supported Languages
- Node.js (JavaScript)
- Python
- Java
- C#
- Golang
- Ruby
- Any other language using Custom Runtime API (community supported, example Rust)
- Lambda Container image
  - Must implement the Lambda Runtime API
  - ECS/Fargate is preferred for running arbitrary Docker images
     
## Use cases
- Serverless thumbnail creation using S3 & Lambda

  <img src=./images/lambda.png width="500"/>

- Serverless CRON job using EventBridge & Lambda

  <img src=./images/lambda1.png width="500"/>

## Limits - per region
- Execution
  - Memory allocation: 128MB-10GB (1MB increments)
  - Maximum execution time: 900 seconds (15 minutes)
  - Environment variables: 4KB
  - Disk capacity in the "function container" (in /tmp): 512MB to 10GB
  - Concurrency executions: 1000 (can be increased)


- Deployment
  - Lambda function deployment size (compressed.zip): 50MB
  - Size of uncompressed deployment (code + dependencies): 250MB
  - Can use /tmp directory to load other files at startup
  - Size of environment variables: 4KB

## Lambda@Edge
- Feature of CloudFront that lets you run code closer to your users, which improves performance and reduces latency.
- Lambda functions written in Node.Js or Python
- Scales to 1000s of requests/second
- Used to change CloudFront requests and responses
  - Viewer request - after CF receives a request from viewer
  - Viewer response - before CF forwards the response to viewer
  - Origin request - before CF forwards the request to origin
  - Origin response - after CF receives the response from the origin
- Author your functions in one AWS region (us-east-1), then CF replicates to its locations
- Longer execution time (several ms)
- Adjustable CPU or memory
- Code depends on a 3rd party libraries (eg; AWS SDk to access other AWS services)
- Network access to use external services for processing
- File system access or access to the body of HTTP requests

<img src=./images/lambda4.jpg width="500"/>

## Networking
- By default, Lambda function is launched outside your own VPC (in an AWS owned VPC)
- So it can not access resources in your VPC (RDS, ElastiCache, internal ELB...)

<img src=./images/lambda2.png width="300"/>

- **Lambda in VPC**
  - You must define the VPC ID, the subnets, and the security groups
  - Lambda will create an ENI (Elastic Network Interface) in your subnets

<img src=./images/lambda3.png width="300"/>
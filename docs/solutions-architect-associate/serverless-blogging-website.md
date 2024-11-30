# Serverless Blogging Website

## Requirements
- This website should scale globally
- Blogs are rarely written, but often read
- Some of the website is purely static files, the rest is a dynamic REST API (public)
- Caching must be implement where possible
- Any new users that subscribes should receive a welcome email
- Any photo uploaded to the blog should have a thumbnail generated

## Architecture

<img src=./images/blogg.png width="500"/>

- Static content being distributed using CloudFront with S3
- The REST API was serverless, didn't need Cognito beacuse public
- Leveraged a Global DynamoDB table to serve the data globally (we could have used Aurora Global Database)
- Enabled DynamoDB Stream to trigger a Lambda function, The Lambda function had an IAM role which could use SES (Simple Email Service) used to send emails in serverless way
- S3 can trigger SQS/SNS/Lambda to notify of events

<img src=./images/blog.jpg width="500"/>
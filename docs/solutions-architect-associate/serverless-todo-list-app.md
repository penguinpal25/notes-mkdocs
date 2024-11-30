# Serverless ToDo List App

## Requirements
- Expose as REST API with HTTPS
- Serverless architecture
- Users should be able to directly interact with their own folder in S3
- Users should authenticate through a managed serverless service
- Users can write and read to-dos, but they mostly read them
- The database should scale, and have some high read throughput

## Architecture
- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
- Giving users access to a folder in S3
  - Using Cognito to generate temporary credentials with STS to access S3 bucket with restricted policy. 
  - App users can directly access AWS resources this way
  - Pattern can be applied to DynamoDB, Lambda...
- Improving read throughputs
  - Implement a DAX layer to cache DynamoDB read queries.
  - Caching can also be implemented as the API gateway level if the read responses don’t change much.

  <img src=./images/todoapp.jpg width="500"/>
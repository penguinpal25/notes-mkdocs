# API Gateway
- Serverless REST APIs
- Invoke Lambda functions using REST APIs (API gateway will proxy the request to lambda)
- Supports WebSocket (stateful)
- Cache API responses
- Transform and validate requests and responses
- Can be integrated with any HTTP endpoint in the backend or any AWS API
- Support API Versioning
- Swagger/Open API import to quickly define APIs
- Generate SDK and API specifications
> We can use an API Gateway REST API to directly access a DynamoDB table by creating a proxy for the DynamoDB query API.

> API cache is not enabled for a method, it is enabled for a stage

## Endpoint Types
- **Edge-Optimized (default)**
  - For global clients
  - Requests are routed through the CloudFront edge locations (improves latency)
  - The API Gateway lives in only one region but it is accessible efficiently through edge locations
- **Regional**
  - For clients within the same region
  - Could manually combine with your own CloudFront distribution for global deployment (this way you will have more control over the caching strategies and the distribution)
- **Private**
  - Can only be accessed within your VPC using an Interface VPC endpoint (ENI)
  - Use resource policy to define access

## Security
- **User authentication through**
  - IAM roles (useful for internal applications)
  - Cognito (identity for external users - mobile users)
  - Custom authorizer (your own logic)
- **Cusom Domain Name HTTPS** security through integration with AWS Certificate Manager (ACM)
  - If using Edge optimized endpoint, then the certificate must be in us-east-1
  - If using Regional endpoint, the certificate must be in API gateway region
  - Must setup CNAME or A-alias record in Route53

## Serverless CRUD Application

<img src=./images/crud.jpg width="500"/>
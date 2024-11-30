# Elastic Container Registry (ECR)
- AWS managed private Docker repository
- Public repository is also available (Amazon ECR public gallery)
- Pay for the storage you use to store docker images (no provisioning)
- Integrated with ECS & IAM for security (permission erros => policy)
- Storage backed by S3
- Can upload Docker images on ECR manually or we can use a CICD service like CodeBuild

<img src=./images/ecr.png width="200"/>
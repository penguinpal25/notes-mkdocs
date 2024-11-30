# Docker
- Software development platform to deploy apps
- Apps are packaged in containers that can be run on any OS
  - Any machine
  - No compatability issues
  - Predictable behaviour
  - Less work
  - Easier to maintain and deploy
  - Works with any language, any OS, any technology
- Use cases: Microservices architecture, lift and shift apps from on-premises to aws cloud...
- Docker images are stored in Docker repositories
  - **Docker Hub** (https://hub.docker.com/)
    - Public repository
    - Find base images for many technologies or OS (Ubuntu, MySQL...)
  - **Amazon ECR** (Amazon Elastic Container Registry)
    - Private repository
    - Public repository (Amazon ECR public gallery https://gallery.ecr.aws/)

## Docker vs VM
- Resources are shared with host => can run many containers in one server

<img src=./images/docker.png width="500"/>

<img src=./images/docker1.png width="500"/>

## Docker container management on AWS
- Amazon Elastic Container Service (Amazon ECS)
  - Amazon's own container platform
- Amazon Elastic Kubernetes Service (Amazon EKS)
  - Amazon's managed Kubernetes (opensource)
- AWS Fargate
  - Amazon's own Serverless platform
  - Works with both ECS and EKS
- Amazon ECR
  - Store container images
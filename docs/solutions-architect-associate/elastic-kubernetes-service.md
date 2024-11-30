# Elastic Kubernetes Service (EKS)
- Used to launch managed Kubernetes (open-source) clusters on AWS
- Supports both EC2 (to deploy worker nodes) and Fargate (to deploy serverless containers) launch types
- Kubernetes is cloud-agnostic (can be used in any cloud - Azure, GCP...)

<img src=./images/eks.jpg width="700"/>

- Inside the EKS cluster, we have EKS nodes (EC2 instances) and EKS pods (tasks) within them. We can use a private or public load balancer to access these EKS pods.
> CloudWatch Container Insights can be configured to view metrics and logs for an EKS cluster in the CloudWatch console
- Use case:
  - If your company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes

## Node types
- **Managed Node Groups**
  - Creates and manages Nodes (EC2 instances) for you
  - Nodes are part of ASG managed by EKS
  - Support On-demand and Spot instances
- **Self-Managed Nodes**
  - Nodes are created by you and registered to the EKS cluster and managed by an ASG
  - You can use prebuilt AMI - Amazon EKS optimized AMI
  - Support On-demand and Spot instances
- **AWS Fargate**
  - No maintenance required; no nodes managed

## Data Volumes
- Need to specify *StorageClass* manifest on your EKS cluster
- Leverages a Container Storage Interface (CSI) compliant driver
- Support for:
  - Amazon EBS
  - Amazon EFS (works with Fargate)
  - Amazon FSx for Lustre
  - Amazon FSx for NetApp ONTAP
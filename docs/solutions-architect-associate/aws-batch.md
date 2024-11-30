# AWS Batch
- **Fully managed** batch processing at **any scale**
- A "batch job" is a job with a start and an end
- Batch will dynamically launch **EC2 instances or Spot instances**
- Provisions the right amount of compute/memory
- Submit or schedule batch jobs
- Defined as **Docker Images and run on ECS**

## Batch vs Lambda
- **Lambda**:
  - Time limit
  - Limited runtimes
  - Limited temporary disk space
  - Serverless
- **Batch**:
  - No time limit
  - Any runtime as long it's packaged as a Docker image
  - Rely on EBS/instance store for disk space
  - Relies on EC2

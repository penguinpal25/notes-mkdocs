# Elastic File System (EFS)
- AWS managed Network File System (NFS)
- Can be mounted to multiple EC2 instances across AZs
- Pay per use (no capacity provisioning)
- Auto scaling (up to PBs)
- Compatible with Linux based AMIs (POSIX file system)
- Uses security group to control access to EFS
- Uses NFSv4.1 protocol
- Encryption at rest using KMS
- Lifecycle management feature to move files to EFS-IA after N days
- POSIX Permissions to control access from hosts by IAM User or Group
- EFS port number: 2049 (NFS also)

## Performance Mode
- **General Purpose (default)**
  - latency-sensitive use cases (web server, CMS, etc.)
  - for small size files
- **Max I/O**
  - higher latency & throughput (big data, media processing)
  - for large size files

## Throughput Mode
- **Bursting (default)**
  - Throughput: 50MB/s per TB
  - Burst of up to 100MB/s
  - Increasing the speed w.r.t the size of data
- **Provisioned**
  - Fixed throughput (provisioned)

## Storage Tiers
- **Standard** or **Regional** (New name) - for frequently accessed files, Keeping the copy of the data in multiple AZ (Highly available, redundant, reliable)
- **One Zone** - Keeping the data only on one AZ
- **Infrequent access (EFS-IA)** - cost to retrieve files, lower price to store
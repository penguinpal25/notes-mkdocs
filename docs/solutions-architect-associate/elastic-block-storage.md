# Elastic Block Storage (EBS)
- Volume Network Drive (provides low latency access to data)
- Can only be mounted to 1 instance at a time (except EBS multi-attach)
- Bound to an AZ
- Must provision capacity in advance (size in GB & throughput in IOPS)
- By default, upon instance termination, the root EBS volume is deleted and any other attached EBS volume is not deleted (can be over-ridden using **DeleteOnTermination** attribute)
- To replicate an EBS volume across AZ or region, need to copy its snapshot
- EBS Multi-attach allows the same EBS volume to attach to multiple EC2 instances in the same AZ
- **DeleteOnTermination** attribute can be controlled by AWS Console/AWS CLI

## Volume Types
### **General Purpose SSD**
- Good for system boot volumes, virtual desktops
- Storage: 1 GB - 16 TB
- **gp3**
  - 3,000 lOPS baseline (max 16,000 - independent of size)
  - 125 MiB/s throughput (max 1000MiB/s - independent of size)
- **gp2**
  - Max IOPS: 16,000 (at 5,334 GB)
  - 3 IOPS per GB
  - Burst IOPS up to 3,000
### **Provisioned IOPS SSD**
- Optimized for Transaction-intensive Applications with high frequency of small & random IO operations. They are sensitive to increased I/O latency.
- Maintain high IOPS while keeping I/O latency down by maintaining a low queue length and a high number of IOPS available to the volume.
- Supports EBS Multi-attach (not supported by other types)
- **io1 or io2**
  - Storage: 4 GB - 16 TB
  - Max IOPS: 64,000 for Nitro EC2 instances & 32,000 for non-Nitro
  - 50 IOPS per GB (64,000 IOPS at 1,280 GB)
  - io2 have more durability and more IOPS per GB (at the same price as io1)
- **io2 Block Express**
  - Storage: 4 GiB - 64 TB
  - Sub-millisecond latency
  - Max IOPS: 256,000
  - 1000 IOPS per GB
### **Hard Disk Drives (HDD)**
- Optimized for Throughput-intensive Applications that require large & sequential IO operations and are less sensitive to increased I/O latency (big data, data warehousing, log processing)
- Maintain high throughput to HDD-backed volumes by maintaining a high queue length when performing large, sequential I/O
- Cannot be used as boot volume for an EC2 instance
- Storage: 125 GB - 16 TB
- **Throughput Optimized HDD (st1)**
  - Optimized for large sequential reads and writes (Big Data, Data Warehouses, Log Processing)
  - Max throughput: 500 MB/s
  - Max IOPS: 500
- **Cold HDD (sc1)**
  - For infrequently accessed data
  - Cheapest
  - Max throughput: 250 MB/s
  - Max IOPS: 250

## EBS Multi-Attach - io1/io2 family
- Attach the same EBS volume to multiple EC2 instances in the same AZ
- Each instance has full read & write permissions to the high performance volume
- Use case:
  - Achieve higher application availability in clustered Linux applications (ex: Teradata)
  - Application must manage concurrent write operations
- Upto 16 instances at a time
- Must use file system that's cluster aware (not XFS, EX4 etc)

## Encryption
- Optional
- For Encrypted EBS volumes
  - Data at rest is encrypted
  - Data in-flight between the instance and the volume is encrypted
  - All snapshots are encrypted
  - All volumes created from the snapshot are encrypted
- Encrypt an un-encrypted EBS volume
  - Create an EBS snapshot of the volume
  - Copy the EBS snapshot and encrypt the new copy
  - Create a new EBS volume from the encrypted snapshot (the volume will be automatically encrypted)

> All EBS types and all instance families support encryption but not all instance types support encryption.

# EBS Snapshots
- Backup of your EBS volume
- Not necessary to detach volume to do snapshot, but recommended
- Can copy across AZ or region
- **Data Lifecycle Manager (DLM)** can be used to automate the creation, retention, and deletion of snapshots of EBS volumes
- Snapshots are incremental (which means that only the blocks on the device that have changed after your most recent snapshot are saved)
- Only the most recent snapshot is required to restore the volume

## EBS Snapshots features
- **EBS Snapshot Archive**
  - Move a Snapshot to an "archive tier" that is 75% cheaper but takes 24 to 72 hours for restoring
- **Recyle Bin for Snapshots**
  - Setup rules to retain deleted snapshots for accidental deletion and can specify retention (from 1 day to 1 year)
- **Fast Snapshot Restore**
  - Force full initialization of snapshot (No latency on first use)

# Amazon Machine image (AMI)
- AMI are a customization of EC2 instance
  - You add your on software, configuration, operating system, monitoring...
  - Faster boot/ configuration becuase all your software is pre-packaged
- AMI are built for a specific region and the AMI ID will be differ across regions (can be copied across regions)
- AMI are stored in S3 but can't be find in S3 console
- You can launch EC2 instances from
  - A Public AMI: AWS provided
  - Your own AMI: you make and maintain them yourself
  - AWS Marketplace AMI: an AMI someone else made and potentially sells
- Once we deregister an AMI, the metadata for creating an instance will be deleted (removes the block device details) and we need to remove the snapshots seperately

## AMI Process (from an EC2 instance)
- Start an EC2 instance and customize it
- Stop the instance (for data integrity)
- Build AMI - this will also create EBS snapshots
- Launch instances from AMI

## AMI Permission
- Public: The AMI can access for all users in that region
- Private: The AMI can access only from the account which created 
  - We can share the AMI across accounts so that they can also acces the AMI but the AMI reside only on the AMI owner account

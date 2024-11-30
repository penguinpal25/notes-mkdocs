# FSx
- Allows us to launch 3rd party high-performance file systems on AWS
- Useful when we don’t want to use an AWS managed file system like S3
- Can be accessed from your on-premise infrastructure (VPN or Direct Connect)

## FSx for Windows
- Shared File System for Windows (like EFS but for Windows)
- Supports SMB protocol, Windows NTFS, Microsoft Active Directory integration, ACLs, user quotas, Microsoft Distributed Filesystem (DFS), Namespaces
- Built on SSD, scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- Supports Multi-AZ (high availability)
- Data is backed-up daily to S3
- Does not integrate with S3 (cannot store cold data)
- Can be mounted on Linux EC2 instances

## FSx for Lustre
- Parallel distributed file system for HPC (like EFS but for HPC)
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Only works with Linux
- Seamless integration with S3
  - Can read S3 buckets as a file system (through FSx)
  - Can write the output back to S3 (through FSx)

## FSx Deployment Options
- **Scratch File System**
  - Temporary storage (cheaper)
  - Data is not replicated (data lost if the file server fails)
  - High burst (6x faster than persistent file system)
  - Usage: short-term processing
- **Persistent File System**
  - Long-term storage (expensive)
  - Data is replicated within same AZ
  - Failed files are replaced within minutes
  - Usage: long-term processing, sensitive data

## FSx for NetApp ONTAP
- Compatible with NFS, SMB, iSCSI protocol
- Works with
  - Linux
  - Windows
  - MacOS
  - VMWare cloud on aws
  - Amazon Workspaces and App Stream 2.0
  - Amazon EC2, ECS, and EKS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression, and data de-duplication
- Point-in-time instantaneous cloning

## FSx for OpenZFS
- Compatible with NFS(v3, v4, v4.1, v4.2)
- Works with
  - Linux
  - Windows
  - MacOS
  - VMWare cloud on aws
  - Amazon Workspaces and App Stream 2.0
  - Amazon EC2, ECS, and EKS
- Upto 1,000,000 IOPS with < 0.5ms latency
- Snapshots, compression and low-cost
- Point-in-time instantaneous cloning


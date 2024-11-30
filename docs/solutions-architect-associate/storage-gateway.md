# Storage Gateway
- Bridge between on-premises data and S3 for Hybrid Cloud
- Not suitable for one-time sync of large amounts of data (use DataSync instead)
- Optimizes data transfer by sending only changed data

## Types of Storage Gateway
- **S3 File Gateway**
  - Used to expand on-premise NFS by leveraging S3
  - Configured S3 buckets are accessible on premises using the NFS and SMB protocol
  - Most recent data is cached at file gateway for low latency access
  - Integrated with Active Directory (AD) for user authentication
  - Bucket access using IAM roles for each File gateway

  <img src=./images/s3fg.png width="500"/>

- **FSx File Gateway**
  - Used to expand on-premise Windows-based storage by leveraging FSx for Windows
  - Windows native compatibility (SMB, NTFS, Active Directory)
  - Most recent data is cached at file gateway for low latency access
  - Useful for group file shares and home directories

  <img src=./images/fsxfg.png width="500"/>

- **Volume Gateway**
  - Used for on-premise storage volumes
  - Uses iSCSI protocol
  - Two kinds of volumes:
    - Cached volumes: storage extension using S3 with caching (most recent data) at the volume gateway
    - Stored volumes: entire dataset is on premise, scheduled backups to S3 as EBS snapshots
  
  <img src=./images/volg.png width="500"/>

- **Tape Gateway**
  - Used to backup on-premises data using tape-based process to S3 as Virtual Tapes
  - Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
  - Uses iSCSI protocol

  <img src=./images/tapg.png width="500"/>

## Storage Gateway - Hardware Appliance
- Storage Gateway requires on-premises virtualization. If you don’t have virtualization available, you can use a Storage Gateway - Hardware Appliance. It is a mini server that you need to install on-premises.
- Does not work with FSx File Gatway


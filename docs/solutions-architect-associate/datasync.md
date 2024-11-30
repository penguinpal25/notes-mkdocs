# DataSync
- Move large amounts of data from your on-premises NAS or file system via NFS or SMB protocol to AWS over the public internet using TLS
- Can synchronize to:
  - S3 (all storage classes)
  - EFS
  - FSx (Windows, Lustre, NetApp, OpenZFS...)
- Replication tasks can be scheduled hourly, daily, and weekly 
- File permission and metadata are preserved (NFS POSIX, SMB...)

<img src=./images/datasync.jpg width="500"/>

- Can also be used to transfer between two storage services in AWS

<img src=./images/datasyncaws.png width="500"/>

- Perfect to move large amounts of historical data from on-premises to S3 Glacier Deep Archive (directly).


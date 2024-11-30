# Instance Store
- Hardware storage directly attached to EC2 instance (cannot be detached and attached to another instance)
- Highest IOPS of any available storage (millions of IOPS)
- Ephemeral storage (loses data when the instance is stopped, hibernated or terminated)
- Good for buffer / cache / scratch data / temporary content
- AMI created from an instance does not have its instance store volume preserved
- Risk of data loss if hardware fails
- Backup and Replication are your responsibility

> You can specify the instance store volumes only when you launch an instance. You can’t attach instance store volumes to an instance after you’ve launched it.


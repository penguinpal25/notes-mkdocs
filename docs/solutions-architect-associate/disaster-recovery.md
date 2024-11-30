# Disaster Recovery

- Any event that has a negative impact on a company’s business continuity or finances is a disaster
- https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html

## Types of Disaster Recovery (DR)
- On-premise => On-premise: traditional and very expensive
- On-premise => AWS Cloud: hybrid recovery
- AWS Cloud Region A => AWS Cloud Region B

Recovery Point Objective (RPO): how often you backup your data (determines how much data are you willing to lose in case of a disaster)
Recovery Time Objective (RTO): how long it takes to recover from the disaster (down time)

![image](https://user-images.githubusercontent.com/65948438/205014811-3b4fbfb2-00ff-4ee5-8ddd-531fd6f7162b.png)

## Strategies

Backup & Restore
- High RPO (hours)
- Need to spin up instances and restore volumes from snapshots in case of a disaster => High RTO
- Cheapest and easiest to manage

Pilot Light
- Critical parts of the app are always running in the cloud (eg. continuous replication of data to another region)
- Low RPO (minutes)
- Critical systems are already up => Low RTO
- Ideal when RPO should be in minutes and the solution should be inexpensive
- DB is critical so it is replicated continuously but EC2 instance is spin up only when a disaster strikes

Warm Standby
- A complete backup system is up and running at the minimum capacity. This system is quickly scaled to production capacity in case of a disaster.
- Very low RPO & RTO (minutes)
- Expensive

Multi-Site or Hot Site Approach
- A backup system is running at full production capacity and the request can be routed to either the main or the backup system.
- Multi-data center approach
- Lowest RPO & RTO (minutes or seconds)
- Very Expensive

## Disaster Recovery Tips
Backup
- EBS snapshots, RDS automated backups/snapshots etc
- Regular pushes to S3 / S3 IA / Glacier, Life cycle policy, Cross region replication
- From On-premise: Snowball or Storage gateway

High Availability
- Use Route53 to migrate DNS over from region to region
- RDS Multi AZ, ElastiCache Multi-AZ, EFS, S3
- Site to Site VPN as a recovery from Direct Connect

Replication
- RDS replication (cross region), AWS Aurora + global databases
- Database replication from on-premise to RDS
- Storage gateway

Automation
- CloudFormation /  Elastic BeanStalk to recreate a whole new environment
- Recover / Reboot EC2 instances with cloudwatch if alarms fails
- AWS Lambda functions for customized automations

Chaos
- Netflix has a "simian-army" randomly terminating EC2

# Elastic Map Reduce (EMR)
- Used to create **Hadoop Clusters (Big Data)** to analyze and process vast amounts of data
- The clusters can be made of **100s of EC2 instances**
- Bundled with Apache Spark, HBase, Presto, Flink, etc
- EMR takes care of all the provisioning and configuration
- Auto-scaling and Integrated with Spot Instances
- Can be used to process large amounts of log files

## Node types & Purchasing
- **Master Node**: Manages the cluster, coordinate, manage health - long running
- **Core Node**: Run task and store data - long running
- **Task Node** (optional): Just to run task- usually Spot
- **Purchasing options**:
  - On-demand: reliable, predictable, won't be terminated
  - Reserved (min 1 year): cost savings (automatically use if available)
  - Spot: cheaper, can be terminated, less reliable
- Can have long running or transient (temporary) cluster
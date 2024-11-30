Before cloud, if we want high availability and low latency, we need to provision two or more data centres in different regions, and if we want low latency, we should provision one data centre close to us. These procedures will be time-consuming and expensive.

## Region

- Specific geographical location to host your resources
- Google provides 20+ regions around the world and it's expanding every year
- For example, the us-central1 region denotes a region in the Central United States
- Advantages:
  - High availability
  - Low latency
  - Global footprint (A company can provision resources gloabally around the world in minutes)
  - Adhere to goverment regulations
 
  
 ## Zones
 
 - By using zones, we can achieve high-availability in the same region.
 - Each region has three or more zones
 - Increased availability and fault-tolerance in the same region
 - Each zone has one or more discrete data centers
 - Zones in a region are connected through low-latency links
 - For example,  us-central1-a, us-central1-b, and us-central1-f.

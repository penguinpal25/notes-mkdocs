# Traffic Mirroring
- Capture and inspect network traffic in your VPC without disturbing the normal flow of traffic
- Inbound and outbound traffic through ENIs (eg. attached to EC2 instances) will be mirrored to the destination (NLB) for inspection without affecting the original traffic.
- Capture the traffic
  - From (Source) ENIs
  - To (Targets) an ENI or NLB 
- Source and Target can be in the same or different VPCs (VPC Peering)
- Use cases: content inspection, threat monitoring, troubleshooting, etc.

<img src=./images/trafficm.jpg width="300"/>
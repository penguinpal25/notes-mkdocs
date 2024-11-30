# Site-to-Site VPN
- Easiest and most cost-effective way to connect a VPC to an on-premise data center
- IPSec Encrypted connection through the public internet
- **Virtual Private Gateway (VGW)**: VPN concentrator on the VPC side of the VPN connection
- **Customer Gateway (CGW)**: Software application or physical device on customer side of the VPN connection
- If you need to ping EC2 instances from on-premises, make sure you add the **ICMP** protocol on the inbound rules of your security groups
- **What IP address to use in Customer Gateway Device (On-premises)?**
  - Public Internet-routable IP address for your Customer Gateway device
  - If it's behind a NAT device that's enabled for NAT traversal (NAT-T), use the public IP address of NAT device

    <img src=./images/s2n.png width="300"/>

- **Important step**: enable **Route Propagation** for the virtual private gateway in the route table that is associated with your subnets

## VPN CloudHub
- Low-cost hub-and-spoke model for network connectivity between a VPC and multiple on-premise data centers
- Every participating network can communicate with one another through the VPN connection 
- It's a VPN connection so goes over the public internet
- To set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables

<img src=./images/cloudhub.jpg width="500"/>
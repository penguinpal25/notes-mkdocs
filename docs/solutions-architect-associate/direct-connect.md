# Direct Connect (DX)
- Dedicated **private** connection from an on-premise data center to a VPC
- More stable and secure than Site-to-Site VPN
- Supports both IPv4 and IPv6
- Access public (S3) & private (EC2) resources on the same connection using Public & Private Virtual Interface (VIF) respectively
- Connection to a data center is made from a Direct Connect Location
- Connects to a Virtual Private Gateway (VGW) on the VPC end
- Supports Hybrid Environments (on premises + cloud)
- Lead time > 1 month

<img src=./images/dx.jpg width="500"/>

- **If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway**
  
  <img src=./images/dxgw.png width="500"/>

  - Using DX, we will create a Private VIF to the Direct Connect Gateway which will extend the VIF to Virtual Private Gateways in multiple VPCs (possibly across regions).
## Connection Types
- **Dedicated Connection**
  - 1 Gbps, 10Gbps and 100 Gbps (fixed capacity)
  - Physical ethernet port dedicated to a customer
- **Hosted Connection**
  - 50Mbps, 500 Mbps, up to 10 Gbps
  - On-demand capacity scaling (more flexible than dedicated connection)
  - Capacity can be **added or removed on-demand**

## Encryption
- Data in transit is not-encrypted but the connection is private (secure)
- For encryption in flight, use AWS Direct Connect + VPN which provides an IPsec-encrypted private connection
- Good for an extra level of security

<img src=./images/dxenc.jpg width="500"/>

## Resiliency
- Best way (redundant direct connect connections)

<img src=./images/dxres.jpg width="500"/>

- Cost-effective way (VPN connection as a backup)
  - Implement an IPSec VPN connection and use the same BGP prefix. Both the Direct Connect connection and IPSec VPN are active and being advertised using the Border Gateway Protocol (BGP). The Direct Connect link will always be preferred unless it is unavailable.

## Site-to-Site VPN connection as a backup
- In case Direct Connect (DX) fails, you can setup a backup Direct Connect connection (expensive), or a site-to-site VPN connection
- So if the private network to DX fails, we can still access services using Site-to-Site VPN through public internet 

<img src=./images/s2sdx.png width="500"/>
# Transit Gateway
- **Transitive peering between thousands of VPCs and on-premise data centers using hub-and-spoke (star) topology**
- Works with Direct Connect Gateway, VPN Connection and VPC
- Bound to a region, can work cross-region (using Resource Access Manager (RAM))
- Route Tables to control communication within the transitive network
- Supports **IP Multicast** (not supported by any other AWS service)

<img src=./images/transitgw.jpg width="300"/>

## Increasing BW of Site-to-Site VPN connection
- **ECMP (Equal Cost Multi-path)** routing is a routing strategy to allow to forward a packet over multiple best path
- To increase the bandwidth of the connection between Transit Gateway and corporate data center, create multiple site-to-site VPN connections, each with 2 tunnels (2 x 1.25 = 2.5 Gbps per VPN connection).
 
  <img src=./images/tgwinc.jpg width="500"/>

- Only one VPN connection to a VPC having 2 tunnels out of which only 1 is used (1.25 Gbps)

## Share DX between multiple Accounts
- Share Transit Gateway across accounts using Resource Access Manager (RAM) connection between VPCs in the same region but different accounts

<img src=./images/tgwshare.jpg width="500"/>
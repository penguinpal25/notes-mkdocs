Example: transfer 200TB of data with 100Mbps internet connection.

- Over the internet / Site-to-Site VPN:
  - Immediate to setup
  - Will take 200(TB)*1000(GB)*1000(MB)*8(Mb)/100Mbps = 16000000s = 185d
- Over Direct Connect 1Gbps:
  - Long for the one-time setup (over a month)
  - will take 200(TB)*1000(GB)*8(Gb)/1Gbps = 1600000s = 18.5d
- Over Snowball:
  - Will take 2-3 snowballs in parallel
  - Takes about 1 week for the end to end transfer
  - Can be combined with DMS
- For on-going replication / transfers: Site-to-Site VPN or DX with DMS or DataSync

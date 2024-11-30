## Google Cloud Compute Engine (GCE)

- In data centers, applications are deployed to physical servers
- In Cloud, the applications are deploying to virtual machines
- GCP provides such virtual machines, called Compute Engine

## Compute Engine - Features

- Create and manage lifecycle of VM instances (Start, Stop, Suspend VM instances etc)
- Load balancing and Auto scaling multiple VM instances
- Attach storage (&network storage) to VM instances
- Manage network connectivity (IP address allocation etc) and configuration for VM instances

## Compute Engine - Machine Family

- General purpose (E2, N2, N2D, N1): Best price-performance ratio
  - Web and application servers, Small-medium databases, Dev environments
- Memory optimized (M2, M1): Ultra high memory workloads
  - Large in-memory databases and In-memory analytics
- Compute optimized (C2): Compute intensive workloads
  - Gaming applications

## Compute Engine - Machine types

- Different machine types are available for each machine family
- For example: e2-standard-2
  - e2: Machine family
  - standard: Type of workload
  - 2: Number of CPUs
- Memory, disk and networking capabilities increase along with vCPUs

## Compute Engine - Image

- The OS that you want to use in your VM
- Types of Images:
  - Public image: Provided and maintained by Google or Open source communities or third party vendors
  - Custom image: The image created by you for your projects

## Internal and External IP addresses

- External (Public) IP addresses can be accessed within Internet
- Internal (Private) IP addresses are internal to a network
- External IP address is unique publically however two different networks can have resources with same internal IP address
- All VM instances are assigned at least one internal IP address
- Creation of external IP address can be enabled for VM instances
- When you stop a VM instance, external IP address is lost
- Sometimes, GCP reuses the same external IP on restart

## Static IP address

- IP addresses that are constant (Will not change during stop/start)
- The static IP address should be created in the same region as of VM instance
- Static IP can be switched to another VM instance in the same project
- Static IP remains attahced even if you stop the instance. You have to manually detach it.
- You're billed for an static IP when you're not using it
- Static IP addresses not attached to an instance or load balancer are billed at a higher hourly rate

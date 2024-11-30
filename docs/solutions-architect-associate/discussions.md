# Classic Solutions Architecture Discussions

# Stateless webapp - whatisthetime.com
- whatisthetime.com allows people to know what time is it
- we don't need a database

## Starting simple
- Create a public EC2 with t2 class and assign elastic IP address so that user can access the webapp using the IP

## Scaling vertically
- More users are accessing the webapp and the t2 instance can't handle that much requests so we are scaling vertically by upgrading the instance class from t2 to m5
- There will be downtime when upgrading to m5

## Scaling horizontally
- More and more users are accessing the webapp and the m5 instance can't handle the requests. 
- This time we are scaling it horizontally meaning adding more number of instances
- Add couple of m5 instances and attach elastic ip to all instances
- This approach is not good because users need to remember the IP addresses also we can only use 5 Elastic IP addresses per Region.
- Remove the elastic IP addresses and create Route 53 hosted zone and add a domain (api.whatisthetime.com) with the value of IP address of public EC2 instances with a TTL value of 1hr
- What if a single instance fails, causing users to experience up to an hour of downtime before being transferred to a functioning instance?

## Scaling horizontally with a load balancer
- Create private EC2 instances manually in an availability zone
- Create a public facing ELB, includes health checks as well

<img src=./images/webapp1.png width="500"/>

- The traffic b/w ELB and EC2 instances are restrcited using security group rules
- Update the record api.whatisthetime.com with an alias record to point to the ELB endpoint

## Scaling horizontally with auto-scaling group
- Create an auto-scaling group in the AZ and rest of the architecture is same
- Now we have no downtime

<img src=./images/webapp2.png width="500"/>

## Making our app multi-AZ
- What if an earthquke strikes and our only AZ is gone, need to make our app multi-AZ
- Making the ELB to use multiple AZs

<img src=./images/webapp3.png width="500"/>

- Auto-scaling group place the instances in multiple AZs
- Even if one AZ is gone, we should have another

### Cost saving approach

<img src=./images/webapp4.png width="500"/>

- 5 pillars for a well architected application
  - costs
  - performance
  - reliability
  - security
  - operational excellence

# Stateful web app: Myclothes.com
- myclothes.com allows users to buy clothes online
- There's a shopping cart
- Our website is having hundreads of users at the same time
- We need to scale, maintain horizontal scalability and keep our web app as stateless as possible
- Users should not lose their shopping cart
- Users should have their details (address,etc) in a database

<img src=./images/sapp1.png width="500"/>

 - User accessing the webapp and ELB route the request to the instance in AZ1 and the shopping cart data is stored in AZ1 instance
 - Suppose ELB re-route the second request to instance in AZ2 thus user lossing the shopping cart data

## Introduce Stickiness (Session Affinity)
  - By enabling stickiness in ELB, every request from a user will go to the same instance and user will be able to access the shopping cart data everytime

  <img src=./images/sapp2.png width="500"/>

  - What if the instace failed? User will loss the shopping cart data

## Introduce User Cookies
  - Instead of storing the shopping cart data in EC2 servers, store the data in web cookies by user (in web browsers)
  
  <img src=./images/sapp3.png width="500"/>

  - Everytime the user connect to ELB, the shopping cart data will be fetch from web cookies

## Introduce Server Session
  - Instead of storing the whole shopping cart data in web cookies, store only the session_id in web cookies
  - Ec2 instance store the session data in ElastiCache 

  <img src=./images/sapp4.png width="500"/>

  - On the next requests, the EC2 instances can retrieve the session data from ElastiCache
  - It's a secure method and no attackers can intrude into ElastiCache
  - As an alternative, we can use amazon DynamoDB to store the session data

## Storing user data in database
  - EC2 instance can store the user data (address, name, etc) in a Amazon RDS database

  <img src=./images/sapp5.png width="500"/>

## Scaling reads
  - Suppose the webapp getting large number of requests so we need to scale the reads

  <img src=./images/sapp6.png width="500"/>

  - We will be having a RDS master for writes
  - We can enable read replicas so that every read requests will go to read replica instances

## Scaling reads alternative - Lazy loading
  - User talk to EC2 instance and it check if the data is cached in ElastiCache
  - If the cache is not present in ElastiCache, Ec2 send requests to RDS and store the data in ElastiCache
  - For next time, the EC2 retrive the data from ElastiCache (hit)

  <img src=./images/sapp7.png width="500"/>

  - By using this strategy, the RDS will receive less requests, which will make it less stressed.

## Multi-AZ: Survive disasters
  - Enable multi-az for ELB, Auto-scaling group, RDS, and ElastiCache (Redis)

  <img src=./images/sapp8.png width="500"/>

## Security groups
  <img src=./images/sapp9.png width="500"/>

# Stateful web app - Mywordpress.com
  - We are trying to create a fully scalable wordpress website
  - We want the website to access and correctly display picture uploads
  - Our user data, blog content should be stored in MySQL database

  - RDS can be used to store the user data however Aurora is recommended with multi-az and read replica
  - Storing images with EBS is useful for single application
  - If it's distributed application, make use EFS
  
  <img src=./images/wp1.png width="500"/>

# Instantiating applications quickly
- EC2 Instances
  - Use a Golden AMI: Install your applications, OS dependencies etc beforehand and launch your instances from golden AMI. Golden AMI is an image that contains all your software installed and configured so that future EC2 instances can boot up quickly from that AMI.
  - Bootstrap using User Data: For dynamic configuration, use user data scripts
  - Hybrid: mix Golden AMI and User data (ElasticBeanstalk)
- RDS databases
  - Restore from snapshot: the database will have schemas and data ready
- EBS volume
  - Restore from snapshot: the disk will already be formatted and have data
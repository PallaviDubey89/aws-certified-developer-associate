# aws-certified-developer-associate
AWS Certified Developer Associate - Tutorial by Stephane Maarek (Udemy)

##### 5. AWS Budget Setup

-  Billing dashboard ->
-  Budgets ->
-  Create budget ->
-  Cost budget ->
    -  Name: Learning AWS
    -  Budgeted amount: $20
    -  Configure thresholds
    -  Set email
-  Confirm -> Create
 
#####  11. AWS Regions and AZs

-  [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
-  Overview
-  Regions and AZs
-  AWS Regional Services ->
    -  Region Table
    
#####  12. IAM Introduction

#####  13. IAM Hands-On

-  IAM - it's a Global Service
-  Multi-factor authentication (MFA)
    -  Activate MFA
    -  Virtual -> Google Auth
-  Users -> Add User
    -  Programmatic access - `true`
    -  AWS Management Console access - `true`
    -  Next: Set permissions ->
    -  Attach existing policies directly
    -  AdministratorAccess
    -  Next: Review ->
    -  Create User
    -  **Download CSV file with credentials**
    -  Close
-  Groups -> New Group
    -  Name: admin
    -  Next ->
    -  Attach policy: AdministratorAccess 
    -  Create group
-  IAM->Groups->admin
    -  Add Users to Group
    -  add `art-admin`
-  Go to users
    -  remove `AdministratorAccess` from `Attached directly` as it is already in groups `Attached permissions`
-  Apply an IAM Password Policy
    -  Allow users to change their own password
    -  Enable password expiration (90 days)
-  Dashboard
    -  Sign-in URL for IAM users in this account->
    -  Customize : `artarkatesoft`
    -  copy link
    -  login
    -  change password

#####  15. EC2 Introduction

-  EC2
    -  Launch Instance
    -  AMI - Amazon Machine Image
        -  `Amazon Linux 2`
    -  Step 5: Add Tags
        -  Name: My Instance From Stephane Tutorial
    -  Step 6: Configure Security Group
        -  Security group name: aws-tutorial-first-ec2
        -  Description: Created from my first EC2
    -  Launch
    -  Create a new key pair

#####  17. How to SSH using Linux or Mac

-  `ssh -i "certified-dev-assoc-course.pem" ec2-user@ec2-13-48-23-85.eu-north-1.compute.amazonaws.com`
-  `chmod 0400`  

#####  18. How to SSH using Windows

-  `Putty`
    -  use `PuttyGen` to convert .rem private key to .ppk
    -  use .ppk

#####  19. How to SSH using Windows 10

-  sometimes it is need to configure access (like chmod on Linux)        

#####  21. EC2 Instance Connect

#####  22. Introduction to Security Groups

#####  23. Security Groups Deep Dive

-  Security group can be attached to multiple instances (many-to-many)
-  Locked down to a region/VPC combination
-  **EC2 with same security group can access this EC2 no matter what IP it has** (??? Does not work with RDS ???)
-  Referencing other security group diagram 

#####  24. Private vs Public vs Elastic IP

#####  25. Private vs Public vs Elastic IP Hands On

-  Elastic IP
    -  Allocate
    -  Associate
    -  Test it
    -  Disassociate
    -  Release

#####  26. Install Apache on EC2

-  `sudo su`
-  `yum update -y` - **-y** - do not prompt
-  `yum install -y httpd.x86_64`
-  `systemctl start httpd.service`
-  `systemctl enable httpd.service` - starts after reboot
-  `curl localhost:80`
-  `publicIP:80` - wait,wait,wait,wait,wait,wait, - timeout **!CONFIGURE SECURITY GROUP!**
-  `echo "Hello World" > /var/www/html/index.html`
-  `hostname -f` - name of machine
-  `echo "Hello World $(hostname -f)" > /var/www/html/index.html`

#####  27. EC2 User Data

-  script **only runs ONCE** at the instance **FIRST start**
-  the EC2 User Data script runs with the **root** user
-  hands on:
    -  terminate old instance
    -  launch new instance ->
    -  security group: use existing
    -  Configure Instance Details ->
    -  Advanced Details -> 
    -  User Data
    -  [Specify instance user data at launch](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)

```shell script
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hello World $(hostname -f)" > /var/www/html/index.html
``` 

#####  28. EC2 Instances Launch Types

-  On Demand Instances
-  Reserved: (minimum 1 year)
    -  Reserved instances
    -  Convertible Reserved instances
    -  Scheduled Reserved instances
-  Spot Instances
-  Dedicated Instances
-  Dedicated Hosts

#####  29. EC2 Instances Launch Types Hands On 

#####  30. EC2 Elastic Network Interfaces

-  create EC2
    -  Number of instances: 2
    -  Subnet: choose ONE (1b for example)
    -  Review and Launch
    -  Look at network interface
- create new Elastic Network Interface
    -  Description: My secondary interface
    -  Subnet: same (1b)
    -  IPv4 Private IP: Auto-assign
    -  Security group: the same
    -  Create
    -  Attach to the first ec2
    -  Detach
    -  Attach to the second ec2
    -  You can:
        -  Manage IP addresses (assign new address)
        -  Change security groups
        -  etc
-  clean up:
    -  detach secondary eni
    -  delete eni
    -  terminate 2 ec2 

####  Section 4: AWS Fundamentals: ELB + ASG

#####  33. High Availability and Scalability

-  **Scalability**:
    -  **Vertical**:
        -  junior -> senior
        -  **_t2.micro -> t2.large_**
        -  RDS: micro -> large
        -  ElastiCache: can scale vertically too
        -  non-distributed system
        -  has limit (hardware limit)
    -  **Horizontal**:
        -  1 operator -> N operators
        -  **_increase No of instances_**
        -  distributed systems
        -  EC2 - increase count
-  **High Availability**:
    -  hand-in-hand with horizontal scaling (usually)
    -  means running app in >=2 AZs (data centers)
    -  goal is to survive a data center loss
    -  can be **passive** (for RDS **_Multi AZ_** for example)        
    -  can be **active** (horizontal scaling)        

#####  34. Elastic Load Balancing (ELB) Overview

-  **Load balancers** are servers that forward internet traffic to multiple EC2s
-  ELB (EC2 Load Balancer) - is a managed load balancer
-  Health Check
    -  `/health`
    -  not `200 OK` - unhealthy
    -  port (4567 for example)
-  AWS Load Balancers
    1.  Classic Load Balancer (v1 - old generation) - 2009
        -  HTTP,HTTPS, TCP
    2.  Application Load Balancer (v2 - new generation) - 2016
        -  HTTP, HTTPS, WebSocket
    3.  Network Load Balancer (v2 - new generation) - 2017
        -  TCP, TLS (Secure TCP) & UDP
-  You can setup ELBs
    -  internal (private)
    -  external (public)
-  Security    
    -  Load Balancers Security Groups
        -  HTTP - 80
        -  HTTPS - 443
        -  Source: 0.0.0.0/0 - from everywhere
    -  Application Security Group: allow traffic only from LB
        -  HTTP - 80
        -  Source: sg-... (load balancer)
        
#####  35. Classic Load Balancer (CLB) with Hands On

-  Load Balancers
    -  Create -> `Classic Load Balancer`
        -  Name: `MyFirstCLB`
        -  LB protocol: HTTP
        -  LB port: 80
        -  Instance protocol: HTTP
        -  Instance port: 80
        -  Next: Assign Security Group
        -  SG name: `my-first-load-balancer`
        -  Description: `My first load balancer security group`
        -  Next: Configure Security Settings -> Warning for HTTP (that's OK for now)
        -  Next: Configure Health Check
        -  Ping Path: `/index.html` (OK for now) or just `/`
        -  interval: set 10s
        -  Healthy threshold: 5
        -  Next: Add EC2 Instances
            -  Choose `My second EC2` (or whatever EC2 you want)
        -  Create
-  now we can access **BOTH** CLB and EC2 on port 80
    -  need to modify this:
    -  go to security group `aws-tutorial-first-ec2`
    -  modify source `0.0.0.0/0` to `my-first-load-balancer`
-  Instances:
    -  right mouse click -> Image and Templates
    -  **Launch more like this**
    -  add another from **different AZ**
-  Load balancers
    -  Edit Instances
    -  Add newly created instances
    -  Save
-  Play with load balancer
    -  visit url `http://myfirstclb-29850709.eu-north-1.elb.amazonaws.com/`
    -  update page -> server name changes
    -  stop some instances
    -  play again
-  Delete Load Balancer

##### 36. Application Load Balancer (ALB) with Hands On

-  Characteristics:
    -  ALB - is Layer 7 (HTTP)
    -  LB to multiple HTTP applications across machines (target groups)
    -  LB to multiple applications on the same machine (ex: containers)
    -  Support for HTTP/2 and WebSocket
    -  Support redirects (ex: HTTP -> HTTPS)
    -  Routing tables to different target groups
        -  path (`example.com/users` & `example.com/posts`)  
        -  hostname in URL (`users.example.com` & `posts.example.com`)
        -  query string, headers (`example.com/users?id=123&order=false`)
    -  ALBs are a great fit for microservices & container-based apps (ex: Docker, Amazon ECS)
    -  Has a port mapping feature to redirect dynamic port in ECS
    -  Target Groups
        -  EC2 instances (can be managed by Auto Scaling Group) - HTTP
        -  ECS tasks (managed by ECS itself) - HTTP
        -  Lambda functions - HTTP request is translated into a JSON event
        -  IP Addresses - must be private IPs
    -  ALB can route to multiple target groups
    -  Health Checks are at the target group level
    -  Good to know
        -  Fixed hostname (XXX.region.elb.amazonaws.com)
        -  The application servers EC2 don't see client's IP directly
            -  true IP of the client is inserted in the header `X-Forwarded-For`
            -  we can also get port (`X-Forwarded-Port`) and proto (`X-Forwarded-Proto`)
-  Create Load Balancer
    -  Name: MyFirstALB
    -  Availability Zones: all 3 zones
    -  Security groups: we gonna reuse `my-first-load-balancer`
    -  Target group
        -  Name: `my-first-target-group`
        -  Target type: instance
        -  Advanced Health Check
            - Interval: 10s
    -  Register targets -> register only 2 targets (for now - 1a, 1b)
    -  Create
    -  Wait while `provisioning`
    -  Copy DNS name and go to the that page -> OK
-  Create another target group 
    -  Name: `my-second-target-group`
    -  add 1 instance `1c`
-  Go to `MyFirstALB`
    -  Listeners
        -  1 Listener -> View/edit rules
        -  Add rule
            -  IF `Path` is `/test`
            -  THEN `Forward to` -> `my-second-target-group`
        -  Add rule
            -  IF `Path` is `/constant`
            -  THEN `return fixed response`
            -  404, `OOOOPPPPPSSSS!!!`
-  Clean up
    -  delete 2 unnecessary rules
    -  delete `my-second-target-group`
    -  to the `my-first-target-group` add missing target

#####  37. Network Load Balancer (NLB) with Hands On

-  Characteristics:
    -  Forward TCP & UDP traffic to your instances
    -  Handle millions of request per second
    -  Less latency ~ 100 ms (vs 400 ms for ALB)  
    -  NLB has **one static IP per AZ**
    -  supports assigning Elastic IP (helpful for whitelisting specific IP)
    -  NLB are used for extreme performance, TCP or UDP traffic
    -  Not included in the AWS free tier
-  Create NLB
    -  Name: `MyFirstNLB`
    -  AZ: all 3 AZs
    -  Routing
    -  Target Group
        -  New
        -  Name: my-target-group-nlb
        -  interval: 10s
    -  Register Targets: all 3
    -  Create
    -  then go to DNS name 
        -> Nothing loads (need to configure security)
-  Configure EC2 security
    -  go to security group `aws-tutorial-first-ec2`
    -  add rule:
        -  from anywhere
        -  port 80
        -  TCP
-  Test everything is OK now
-  Clean Up
    -  delete NLB
    -  delete target group
    -  delete rule from security group `aws-tutorial-first-ec2`
    
    
    
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
    
#####  38. Elastic Load Balancer - Stickiness

-  Target Groups
    -  Edit Attributes
        -  Enable
        -  Stickiness duration: 2 minutes
-  test it
-  disable it

#####  39. Elastic Load Balancer - Cross Zone Load Balancing

1.  Characteristics
    -  Classic Load Balancer
        -  disabled by default
        -  no charges for inter AZ if enabled
    -  Application Load Balancer
        -  always **ON** (can't be disabled)
        -  no charges for inter AZ
    -  Network Load Balancer
        -  disabled by default
        -  you pay charges ($) if enabled
2.  Hands on          
                    
#####  40. Elastic Load Balancer - SSL Certificates

-  Characteristics
    -  Classic Load Balancer
        -  supports only one SSL certificate
        -  must use multiple CLB for multiple hostname with multiple SSL certificates
    -  Application Load Balancer
        -  supports multiple listeners with multiple SSL certificates
        -  uses Server Name Indication (SNI) to make it work
    -  Network Load Balancer
        -  supports multiple listeners with multiple SSL certificates
        -  uses Server Name Indication (SNI) to make it work

#####  41. Elastic Load Balancer - Connection Draining

Names:
-  CLB:
    -  Connection Draining
-  ALB, NLB:
    -  Target group: Deregistration Delay    

#####  43. Auto Scaling Groups Hands On

-  Creating Auto Scaling
    -  Auto Scaling Groups ->
    -  Create
    -  Name: MyFirstASG
    -  Create Launch Template
        -  Name: MyFirstTemplate
        -  Template version description: My first template
        -  Autoscaling guidance: false
        -  Create new launch template
            -  same as EC2 from previous sections (with User Data!!!)
    -  in ASG console choose newly created ASG
    -  Next
    -  Subnets: choose all 3 AZs
    -  Next
    -  Load Balancing: ALB
        -  Group:  `my-first-target-group`
        -  Health checks:
            -  EC2: true
            -  ELB: true
        -  Enable group metrics collection within CloudWatch: true
    -  Next
    -  Configure group size and scaling policies
        -  Group size 
            -  Desired capacity: 1
            -  Minimum capacity: 1
            -  Maximum capacity: 3
        -  Scaling policies: None (for now, see next lesson)
        -  Next->Next->Next->Create Auto Scaling group
-  Playing  with ASG
    -  ASG: MyFirstASG
    -  Details
    -  Activity:
        -  Activity history
    -  Instance management
    -  Go to Target Groups -> see Targets -> healthy
    -  Go to ASG -> MyFirstASG
        -  Details -> Group Details -> Edit
            - Desired Capacity: 2
        -  Activity -> see Diff (starting new EC2) 
        -  Instance Management
            -  see both EC2 healthy
    -  go to ALB DNS -> refresh -> monitor different IPs (make sure TargGr->GroupDetails->Stickiness is DISABLED)
    -  Target Groups -> Targets (healthy, healthy)
-  Scale in
    -  change Desired Capacity back to 1
    -  Activity History: WaitingForELBConnectionDraining
    -  after 300 sec instance was terminated

#####  44. Auto Scaling Groups - Scaling Policies

-  Auto Scaling Groups - Scaling Cooldowns
-  Automatic Scaling -> Scaling Policies
    -  Add policy
        -  Target Tracking policy
        -  Instances need **10**  seconds warm up before including in metric (for testing purpose)
        -  Create
-  Monitoring
    -  EC2 ->
    -  CPU Utilization -> see low
-  Increase desired instances to 2
    -  Details -> Edit -> Desired Capacity
-  CloudWatch
-  After certain period of Time 
    -  ```Terminating EC2 instance: i-082c8ce5284deddd2```
    -  Desired capacity made 1 by scaling policy
-  Delete Target Tracking Policy
-  Look at Creation Step Scaling    
-  Look at Creation Simple Scaling
-  Create Scheduled Action
    -  ScaleAtXPM
    -  delete then after tests
        
####  Section 5: EC2 Storage - EBS & EFS

#####  46. EBS Intro Hands On

-  Elastic Block Storage (EBS)
-  Create new EC2
    -  As usual
    -  Step 4: Storage
        -  Add New Volume
            -  Size: 2 GiB
            -  Delete on Termination: false
    -  Tags:
        -  Name: EBSDemo
-  Volumes
    -  2 volumes
-  SSH to EC2
    -  `lsblk`
    -  [Making an Amazon EBS volume available for use on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
    -  `sudo file -s /dev/xvdf` - is there file system on drive?
    -  `sudo file -s /dev/nvme1n1` - `data` - my drive does not have fs, need to format
    -  `sudo mkfs -t ext4 /dev/nvme1n1` - create fs
    -  `sudo mkdir /data`
    -  `sudo mount /dev/nvme1n1 /data`
    -  `cd /data`
    -  `sudo touch Hello.txt`
    -  `nano Hello.txt`
    -  `hello world`
    -  `sudo cp /etc/fstab /etc/fstab.orig`
    -  `sudo nano /etc/fstab`
    -  add `/dev/nvme1n1 /data ext4 defaults,nofail  0  2`
    -  **or** with using UUID
    -  add `UUID= ??? /data ext4 defaults,nofail  0  2`
    -  `sudo umount /data` or `sudo umount -l /data` (if busy) 
    -  `lsblk`
    -  `sudo mount -a`
    -  `lsblk` -> if `/data` is mounted then OK
    
#####  48. EBS vs Instance Store

-  `c5d.large` - look but do not create
-  `ephemeral0	/dev/nvme0n1` - this is instance store - physical drive not network like EBS 

#####  49. EFS Overview

-  `EBS` - **single** AZ
-  `EFS` - **multi** AZs - Elastic File System
-  expensive - ~3*gp2 drive, but __pay per use__

#####  50. EFS Hands On

1.  Services -> Storage -> EFS
    -  Create File System -> Customize
    -  Name: <leave empty>
    -  All default
    -  Network access:
        -  all 3 AZs
        -  create new security group
            -  name: `my-efs-demo`
            -  description: `SG for EFS`
            -  no Inbound rule (for now)
        -  delete default SGs for AZs
        -  use SG `my-efs-demo` for all 3 AZs
    -  Create
2.  EC2 -> Launch instance
    -  instance 1
        -  Subnet: 1a
        -  File Systems: do **NOT** add for now
        -  Security Group -> create new
            -  Name: `ec2-to-efs`
            -  only SSH for now
        -  Launch
    -  instance 2
        -  launch more like this (like previous)
        -  Subnet: 1b
        -  Launch
3.  SSH to both instances
    -  EFS console
        -  my efs -> Attach
        -  Using DNS
        -  `sudo mount -t efs -o tls fs-06cb7797:/ efs` -> `mount: efs: mount point does not exist.`
        -  need to install the **amazon-efs-utils package**
            -  [Mounting EFS file systems](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html)
            -  [Installing the amazon-efs-utils Package on Amazon Linux](https://docs.aws.amazon.com/efs/latest/ug/installing-amazon-efs-utils.html)
            -  `sudo yum install -y amazon-efs-utils`
        -  `mkdir efs`
        -  `sudo mount -t efs -o tls fs-06cb7797:/ efs` -> timeout -> `Connection reset by peer`
        -  need to modify security group
        -  modify `my-efs-demo` inbound rule
            -  Type: NFS
            -  Source: sg `ec2-to-efs`
        -  `sudo mount -t efs -o tls fs-06cb7797:/ efs` -> OK
        -  `cd efs`
        -  `ll` -> no files
        -  `sudo touch ReadArt.txt`
        -  `ll` in both EC2s -> file present
4.  Clean Up (**51. EBS & EFS - Section Cleanup**)
    -  terminate both EC2 instances
    -  delete file system
    -  volumes - delete available
    -  snapshots - delete snapshots (I have none)
    -  delete security groups (**do not delete** default)
    
####  Section 6: AWS Fundamentals: RDS + Aurora + ElastiCache

#####  55. AWS RDS Hands On

1.  RDS Console
    -  Paris -> Free tier
    -  MySQL
    -  DB instance identifier (name) must be unique across region: `my-first-mysql`
    -  Master username: `art`
    -  Master password: `password`
    -  DB instance size: `db.t2.micro`
    -  Enable storage autoscaling: `false`
    -  Public access: `Yes` (for study purpose)
    -  VPC security group: `Create new`
        -  name: `my-first-rds-sg`
    -  Additional configuration
        -  Initial database name: `mydb`
    -  Create database
2.  Use SQLectron
    -  Add
    -  Name: `My RDS database for AWS Udemy course (Stephane)`
    -  Save
    -  Connect
3.  RDS Console -> `my-first-mysql`
    -  Create read replica
        -  `my-first-mysql-replica`
    -  Create table
        `CREATE TABLE Persons (
             PersonID int,
             LastName varchar(255),
             FirstName varchar(255),
             Address varchar(255),
             City varchar(255)
         );`
    -  Insert row
        `INSERT INTO `Persons` VALUES (1,'Shyshkin','Art','my address','my city');`
4.  Connect to REPLICA
    -  test table exists too
    -  trying to insert another row
        -  `INSERT INTO `Persons` VALUES (2,'Shyshkina','Kate','my address','my city');`
    -  got an error
        -  `The MySQL server is running with the --read-only option so it cannot execute this statement`

#####  58. Aurora Hands On

1.  RDS Console
    -  create new database
    -  Aurora -> MySQL compatibility
    -  version 5.6.10a (Stephane version)
    -  DB Cluster Identifier: `my-aurora-db`
    -  Templates: `Production` (then I choosed `Dev/Test`)
    -  DB instance size: use cheapest
    -  Multi-AZ deployment:  yes
    -  Initial database name: `aurora`
    -  Enable deletion protection: true
    -  KMS key ID (generated): `6c8e68f7-fb9b-4fa9-a680-bae90321affc`
    -  Create database
2.  Test it
    -  `my-aurora-db.cluster-cha39fdqzzb3.eu-west-3.rds.amazonaws.com`
    -  Endpoints
        -  `my-aurora-db.cluster-cha39fdqzzb3.eu-west-3.rds.amazonaws.com` - writer
        -  `my-aurora-db.cluster-ro-cha39fdqzzb3.eu-west-3.rds.amazonaws.com` - **ro** means read-only
    -  Click `my-aurora-db-instance-1`
        -  Endpoint is `my-aurora-db-instance-1.cha39fdqzzb3.eu-west-3.rds.amazonaws.com`
        -  You can connect but **IT IS NOT RECOMMENDED WAY**
        -  choose Endpoint for Writer (shown above)
    -  Click `my-aurora-db-instance-1-eu-west-3b`
        -  Endpoint is `my-aurora-db-instance-1-eu-west-3b.cha39fdqzzb3.eu-west-3.rds.amazonaws.com`
        -  You can connect but **IT IS NOT RECOMMENDED WAY**
        -  choose Endpoint for Reader (shown above)
    -  Actions possible:
        -  Add reader
        -  Create cross-Region read replica
        -  Create clone
        -  Restore to point in time
        -  Add replica auto-scaling
    -  Actions -> Add replica auto-scaling
        -  Policy name: `MyScalingAurora`
        -  Avg CPU Utilization: 60
        -  Rest leave default
        -  Add policy
3.  Clean Up
    -  delete Writer Endpoint
    -  `delete me`       
    -  Modify Cluster ->
        -  `Enable deletion protection` : false
        -  Scheduling of modifications: Immediately
    -  delete Reader Endpoint
        
#####  60. ElastiCache Hands On

1.  ElastiCache console
    -  Redis
    -  Name: `MyFirstRedis`
    -  Description: `My first Redis instsance`
    -  Node type: `cache.t2.micro` - free tier
    -  Number of replicas: 0 (for study, otherwise pay money)
        -  so we loose MultiAZ option
    -  Subnet group:
        -  Name: `my-first-subnet-group`
    -  Encryption
        -  `at-rest` blank
        -  `in-transit` leave blank
    -  Create
2.  Clean Up
    -  Delete Redis Cluster    

####  Section 7: Route 53

#####  63. Route 53 Hands On

1.  Route 53 Console
    -  Create record
    -  `nslookup dockerapp.shyshkin.net`
    -  **or**
    -  `nslookup dockerapp.shyshkin.net 8.8.8.8`                 
    -  `nslookup dockerapp.shyshkin.net dns.google`                 
     

#####  64. Route 53 - EC2 Setup

1.  [Instance metadata and user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
    -  `http://169.254.169.254/latest/meta-data/`
    -  `http://169.254.169.254` - aws metadata server
2.  Create EC2 instance in one Region (for example Stockholm)
    -  User Data
        `#!/bin/bash
         yum update -y
         yum install -y httpd
         systemctl start httpd
         systemctl enable httpd
         EC2_AVAIL_ZONE=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)
         echo "<h1>Hello World $(hostname -f) in AZ $EC2_AVAIL_ZONE </h1>" > /var/www/html/index.html`
    -  new security group -> HTTP enable all       
3.  Create EC2 in another region (Paris)          
4.  Create EC2 in Asia (Tokyo)
5.  Table
    -  http://52.47.145.218/ - eu-west-3b
    -  http://13.48.49.160/ - eu-north-1c
    -  http://3.112.13.107/ - ap-northeast-1a
6.  LoadBalancer
    -  New ALB
    -  new SG
    -  new Target Group `DemoRoute53TG`
    -  add Target to TG
    - Review -> Create             

#####  65. Route 53 - TTL

1.  Route53 Console
    -  `ttldemo.shyshkin.net` -> to EC2 in Paris (15.236.141.98)
    -  TTL -> 120 sec
2.  `dig ttldemo.shyshkin.net`
    -  got an answer with such a row `ttldemo.shyshkin.net.   46      IN      A       15.236.141.98`
    -  46 is seconds left to request DNS of 120 sec (TTL 120) - IP Cached            
3.  ttldemo changed to Tokyo but still got Paris while TTL expires       

#####  66. Route 53 CNAME vs Alias  

1.  Theory
    -  CNAME - Points a hostname to any other hostname
        -  mydemobalancer.shyshkin.net -> DemoALBRoute53-187376732.eu-north-1.elb.amazonaws.com
        -  **ONLY FOR NON ROOT DOMAIN** (something.mydomain.com)
    -  Alias - Points a hostname to an AWS Resource 
        -  app.mydomain.com -> blabla.amazonaws.com
        -  works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
        -  _Free of charge_
        -  Native health check
2.  Hands on
    -  CNAME
        - `myapp.shyshkin.net`
        -  Record type: CNAME
        -  Value/Route traffic to -> IP Addr or another... -> LoadBalancer DNS
    -  Alias
        -  `myalias.shyshkin.net`
        -  Alias for ALB
    -  Alias ROOT
        -  Record name: empty (will be just `shyshkin.net`)
        -  Value/Route traffic to:
            -  Alias to another record in this hosted zone
            -  `us-east-1` (only one that available)
            -  `myalias.shyshkin.net` (or directly to LoadBalancer)
        -  Record type: A
    -  CNAME ROOT
        -  Record name: empty (will be just `shyshkin.net`)
        -  Value/Route traffic to:
            -  IP Address or another value depending on the record type
            -  `myalias.shyshkin.net`
        -  Record type: CNAME
        -  got an **Error**
        -  `Bad request.
            (InvalidChangeBatch 400: RRSet of type CNAME with DNS name shyshkin.net. is not permitted at apex in zone shyshkin.net.)`
        -  so we can not use CNAME for apex domain (root domain)

#####  67. Routing Policy - Simple

-  `simple.shyshkin.net`
-  IPs:
    -  52.47.145.218
    -  13.48.49.160
    -  3.112.13.107
-  TTL: 60
-  client randomly choose one of IP

#####  68. Routing Policy - Weighted

1.  Create Record:
    -  Weighted
    -  `weighted.shyshkin.net`
    -  TTL 60
    -  Define weighted record
        -  Paris IP
        -  Tokyo IP
        -  Stockholm LB Alias
        -  All with different weights (10, 20, 70 % _or_ 2,3,10 weight coefs, no matter)
        -  without health check for now
2.  Testing
    -  go to  `weighted.shyshkin.net`
    -  `dig weighted.shyshkin.net`                

#####  69. Routing Policy - Latency

1.  Create Record
    -  Latency
    -  `latency.shyshkin.net`
    -  TTL 10
    -  Define latency record
        -  Paris IP
        -  Tokyo IP
        -  Stockholm LB Alias        
2.  Testing
    -  using VPN (NordVPN, TouchVPN)
    -  monitor different EC2 instances

#####  70. Route 53 Health Checks

1.  Route 53 Console
    -  Health checks -> Create new
    -  Name: `Paris Health Check`
    -  Endpoint
        -  IP: Paris IP (`52.47.145.218`)
        -  Hostname: empty
        -  Port: 80
    -  create health check
2.  Create Tokyo health check
3.  Create Stockholm ALB health check
    -  Domain name
    -  Domain name: ALB DNS

#####  71. Routing Policy - Failover

1.  Create Record
    -  Failover
        -  `failover.shyshkin.net`
        -  TTL 30
    -  Define failover record
        -  to ALB in Stockholm `myalias.shyshkin.net` - Primary
            -  Health check: Stockholm ALB Health Check                   
        -  to Paris EC2 - Secondary
        -  ~~to Tokyo EC2~~ - Secondary (not enabled)
2.  Testing
    -  shut down EC2 in Stockholm so ALB health status will be unhealthy
    -  make sure Route 53 switches to Paris         

#####  72. Routing Policy - Geolocation

1.  Create record
    -  Geolocation
        -  `geolocation.shyshkin.net`
    -  Define geolocation record Paris
        -  IP: `Paris IP`
        -  Location: `Europe` (can choose country)
    -  Define geolocation record Tokyo
        -  IP: `Tokyo IP`
        -  Location: `Asia` (for test choose Ukraine)
    -  Define geolocation record Stockholm
        -  Alias to another record: `myalias.shyshkin.net` (ALB)
        -  Location: `Default`
2.  Testing
    -  use VPN
    -  from Europe -> should redirect to Paris
    -  from Ukraine -> should be Tokyo
    -  from anywhere else -> Stockholm
3.  Note
    -  **DEFAULT** should be present          
        
#####  73. Routing Policy - Multi Value

1.  Create record
    -  Multivalue answer
    -  `multi.shyshkin.net`
    -  Define multivalue answer record for Paris
        -  IP: `Paris IP`
        -  health check: Paris
    -  Define multivalue answer record for Tokyo
        -  IP: `Tokyo IP`
        -  health check: Tokyo
    -  ~~Define multivalue answer record for Stockholm~~
        -  ~~IP: `DemoALBRoute53-187376732.eu-north-1.elb.amazonaws.com`~~
        -  ~~health check: Stockholm~~
        -  `Bad request.
        (InvalidChangeBatch 400: ARRDATAIllegalIPv4Address (Value is not a valid IPv4 address) encountered with 'DemoALBRoute53-187376732.eu-north-1.elb.amazonaws.com')`
    -  Define multivalue answer record for Stockholm
           -  IP: `Stockholm EC2 IP`
           -  health check: Stockholm
2.  Testing
    -  `dig multi.shyshkin.net`
    -  it will show available IPs
```
;; ANSWER SECTION:
multi.shyshkin.net.     33      IN      A       13.48.134.232
multi.shyshkin.net.     33      IN      A       3.112.13.107
multi.shyshkin.net.     33      IN      A       52.47.145.218
```               
It works like **FAULT TOLERANCE ON THE CLIENT SIDE**                
    
#####  74. Section Cleanup

1.  Route 53 Console
    -  peek all unused records one-by-one
    -  delete
2.  Terminate all unused EC2s
3.  Delete load balancer
4.  Delete health checks 
      

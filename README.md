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
---
title: '"AWS - EC2 AND EBS"'
draft: 
tags:
  - AWS
  - ec2
  - ebs
  - networking
---
  
EC2 is a primary web service component of AWS that provide resizable compute power. 

  

## Instance type families: 

1. C4 compute 
2. R3 RAM
3. I2 I/O
4. G2 GPU instances

  

## AMI (Amazon Machines Image)

  

- **It’s an operating system image , that might or might not be customised this is installed on EC2 Instance** 

- Linux AMI
- Windows AMI
- AMI’s for Deep Learning 
- Based on X86 arch OS

- **Four Sources for finding AMI on AWS** 

- Published by AWS 
- From Marketplace on AWS 
- Generated from Existing Instances 
- Upload virtual servers (virtual machine, etc)

- **Securely using an Instance (Once launched it can be accessed over the internet)**

- DNS

- Aws provides a DNS name to the EC2 instance
- User can’t change this on its own 
- Can’t be transfer from one instance to another

- Public IP

- AWS asign a public IP address to the Instance 
- This Ip is assigned from the reserved addresses from AWS 

- Elastic IP

- Its an address that we reserve independently and associate it with our EC2 Instance 
- It persists until user releases it
- It can be transferred on the time of failure of the instance 

- **Initial Access of Launched EC2** 

- AWS provides a key-pair, public key is provided to the user and private key is held by AWS 

- **Virtual Firewall Protection** 

- Using Security groups 

- Security Groups for EC2 will give the control on outgoing traffic 
- For VPC is controls outgoing/Incoming traffic  
- Instance can have more than one security groups attached 
- Instance must attach to one security group 
- Default is Deny 

- Port : allowing connection on specific port, for HTTP it’s 80. 
- Protocol: HTTP
- Source/Destination : Identifies other end of the communication

- CIDR block: x.x.x.x/x, It will allow specific range of IP
- Security Group: Includes any instance the is associates with given group. Helps Preventing coupling od security coups associated with IP addresses. (I did not understand the meaning of this.)

- Applied at Instance level (better for security purposes)

- **Life Cycle of Instance** 

- **Bootstrapping**: Pre-configuration of machine and install software on the fly 

- The bootstrap script is stored with the string value “User Data”. 
- Can also import VM machine into EC2 
- Tags are available for tagging EC2, easy to grouping when you have large number of EC2 
- Keys, values pair to store Tags 

- Project : RecSys
- Development : Production 
- Billing Code : 110011 

- **Monitoring EC2** 

- CloudWatch service 

- **Pricing Options** 

- On-Demand Instances 

- Per hour Pricing

  

## EBS (Elastic Block Storage)

  

- **Basics** 

- AWS provides a persistent block-level storage volumes  
- Protects from component failure, high availability and durability 
- Can be attached to one Instance at a time 
- Data replicated within same region for fault tolerance 

- **Types** 

- Magnetic 

- Lowest performance, lower price, 
- Range form 1 GB to 1 TB 
- 100 average IOPS, can Burst upto 100s 
- Bill based on data space provisioned 

- General purpose SSD

- Used where high performance is not required. 

- If IOPS are not used then they are stored as a IO credit and can be used when you need heavy lifting which can 

- Provisioned IOPS SSD 

- For I/O intensive workloads 
- 20,000 IOPS

- EBS-Optimised Instances 

- Better optimised 
- You pay additional per hour price 

  

## **Data Protection**

  

Backups/Recovery (Snapshot)

  

- On EBS backups can be done by taking snapshots we can put a scheduler for regular backups and save them to S3 
- Only changes in the snapshot what we pay for 
- We can’t use them as like our S3 buckets but we can use them to restore data 
- We can create volume from snapshots. Data is loaded lazily. 
- EBS persists after the instance termination so the data is stored in root volume and can be used with other instance 
- If instance set on termination on deletion then data on EBS will be lost 
- EBS can be encrypted using AWS key Management and it’s transparent
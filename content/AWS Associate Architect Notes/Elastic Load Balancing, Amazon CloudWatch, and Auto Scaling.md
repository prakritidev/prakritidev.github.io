---
title: '"Elastic Load Balancing, Amazon CloudWatch, and Auto Scaling"'
draft: 
tags:
  - AWS
  - elb
  - autoscaling
---




  

  

These three services can be used very effectively together to create a highly available application with a resilient architecture on AWS. 

  

  

## **Elastic Load Balancing**

  

ELB distributes the incoming traffic to multiple EC2 instances in one or more availability zones. ELB supports routing of HTTP, HTTPS, TCP, SSL, DNS, CNAME. They do the health checks of instances so that traffic can’t be transfer to unhealthy EC2, and also launch new instances based on he matrices. 

  

  

**Internet-Facing Load Balancers** 

  

They take requests coming from internet and distributes to the EC2 container registered with load balancers. 

- AWS recommends to use DNS rather than IP address of the ELB in order to provide a single, stable entry point. 

  
**There are two types of ELBs**


**Internal load balancers**

  

it is often useful to load balance between the tiers of the application.

  

**HTTPS Load Balancers** 

  

We can create these load balancers that uses SSL/Transport Layer Security for encrypted connections, for this you have to install certification on your load balancer to decrypt. 

  

**Listeners** 

  

Listeners are nothing but a process that checks for connection requests. You can allow which kinda connection are allowed and which are not. 

  

## **Configuring Elastic Load Balancing** 

  

- **Idle Connection Timeout** 

- Load balancer created two connection when a request comes to the ELB, one is with client and second is with backend services. The idle connection timeout is 60 seconds if there is no data data flow. And if request doesn’t complete within the idle timeout period, the load balancer closes the connection, even if data is still being transferred. 
- Keep-alive, when enabled, allows the load balancer to reuse connections to your back-end instance, which reduces CPU utilisation. 

- **Cross-Zone Load Balancing** 

- traffic is routed evenly across all back-end instances for your load balancer, regardless of the Availability Zone in which they are located 
- Cross-zone load balancing reduces the need to maintain equivalent numbers of back-end instances in each Availability Zone and improves your application’s ability to handle the loss of one or more back-end instances 

- **Connection Draining** 

- LB stops sending the new connection to the instances that are deregistering or unhealthy, while keeping the existing connections open 
- The maximum timeout value can be set between 1 and 3,600 seconds (the default is 300 seconds). When the maximum time limit is reached, the load balancer forcibly closes connections to the deregistering instance. 

- **Proxy Protocol**

- When you use TCP or SSL for both front-end and back-end connections, your load balancer forwards requests to the back-end instances without modifying the request headers 
- If you enable proxy protocol a human readable header is attached with the connection information 
- The header is then sent to the back-end instance as part of the request 
- If Proxy Protocol is enabled on both the proxy server and the load balancer, the load balancer adds another header to the request, which already has a header from the proxy server. Depending on how your back-end instance is configured, this duplication might result in errors. 

- **Sticky Session** 

- **L**ad balancer routes each request independently to the registered instance with the smallest load. However, sticky session feature (also known as session affinity),  enables load balancer to bind a user’s session to a specific instance. 

- **Health Checks**

- ELB checks all the EC2 which are registered with it, by sending a ping, a connection attempt. If it not responds it labels as OutOfSerivce and ELB does not send the new request to that EC2.  

## **Amazon CloudWatch**

  

Amazon CloudWatch supports monitoring and specific metrics for most AWS Cloud services, including: 

  

Auto Scaling, Amazon CloudFront, Amazon CloudSearch, Amazon DynamoDB, Amazon EC2, Amazon EC2 Container Service (Amazon ECS), Amazon ElastiCache, Amazon Elastic Block Store (Amazon EBS), Elastic Load Balancing, Amazon Elastic MapReduce (Amazon EMR), Amazon Elasticsearch Service, Amazon Kinesis Streams, Amazon Kinesis Firehose, AWS Lambda, Amazon Machine Learning, AWS OpsWorks, Amazon Redshift, Amazon Relational Database Service (Amazon RDS), Amazon Route 53, Amazon SNS, Amazon Simple Queue Service (Amazon SQS), Amazon S3, AWS Simple Workflow Service (Amazon SWF), AWS Storage Gateway, AWS WAF, and Amazon WorkSpaces. 

  

- CloudWatch does not aggregate data across regions but can aggregate across Availability Zones within a region. 
- CloudWatch stores for 2 week, to retain more than that save the logs to S3.

  

**Auto Scaling**

  

Auto Scaling is a service that allows you to scale your Amazon EC2 capacity automatically by scaling out and scaling in according to criteria that you define. 

  

- **Manual Scaling** 

  

When you need a very basic, pre-calculated change in your need.

  

- **Scheduling Scaling** 

  

You can Schedule when the demand will be at peak and add some compute instances and after that remove those instances can save a lot of bill 

  

- **Dynamic Scaling** 

  

Dynamic scaling lets you define parameters that control the Auto Scaling process in a scaling policy

  

## **Auto Scaling Components** 

  

  

## **Launch Configuration:** 

- A launch configuration is the template that Auto Scaling uses to create new instances, and it is composed of the configuration name, Amazon Machine Image (AMI), Amazon EC2 instance type, security group, and instance key pair. Each Auto Scaling group can have only one launch configuration at a time 
- default number of Amazon EC2 instances you can currently launch within a region, which is 20 

  

## **AutoScale Group** 

  

EC2 instances are attached to Auto Scale group which are used for scaling purpose. Auto Scaling Group has a Launch configuration file that is used as a base image for deploying new EC2 instances. They have minimum and maximum limit , they also can use spot on! Instances that are way more cheaper than on demand. 

  

  

## **Scaling policy** 

  

- You can associate Amazon CloudWatch alarms and scaling policies with an Auto Scaling group to adjust Auto Scaling dynamically. 

  

- When a threshold is crossed, Amazon CloudWatch sends alarms to trigger changes (scaling in or out) to the number of Amazon EC2 instances currently receiving traffic behind a load balancer. 

  

- After the Amazon CloudWatch alarm sends a message to the Auto Scaling group, Auto Scaling executes the associated policy to scale your group. The policy is a set of instructions that tells Auto Scaling whether to scale out, launching new Amazon EC2 instances referenced in the associated launch configuration, or to scale in and terminate instances. 
- Best practice: 

- Scale out fast, but scale in slowly. Because that will avoid the relaunching of new instance as EC2 charges on per hour basis. 
- Bootstrap your EC2, that will make ec2 to join faster to the pool. 

  

When we are doing the AutoScale, we copy the code of frontend and backend to all the machines.
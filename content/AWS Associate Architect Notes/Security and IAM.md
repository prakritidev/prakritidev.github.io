---
title: '"Security and IAM"'
draft: 
tags:
  - AWS
  - security
  - iamRoles
---



## **AWS Identity and Access Management (IAM)** 

- users (people or services that are using AWS)
- groups (containers for sets of users and their permissions)
- roles (containers for permissions assigned to AWS service instances)



IAM uses traditional identity concepts such as users, groups, and access control policies to control who can use your AWS account, what services and resources they can use, and how they can use them. 
  

- IAM is not an identity store/authorisation system for your applications 

  

## **Principals**

  

Principal is an IAM entity that is allowed to interact with AWS resources. 

  

There are three types of principals :

  

- **root users**

- When you first create an AWS account, you begin with only a single sign-in principal that has complete access to all AWS Cloud services and resources in the account 

  

- **IAM users**

- Users are persistent identities set up through the IAM service to represent individual people or applications. 
- Users are persistent in that there is no expiration period; they are permanent entities that exist until an IAM administrator takes an action to delete them. 

- **roles/temporary security tokens** 

- These tokens are issued for a small period of time. When one of the actors assumes a role, AWS provides the actor with a temporary security token from the AWS Security Token Service (STS) 
- The range of a temporary security token lifetime is 15 minutes to 36 hours. 

- **EC2 roles** 

- Assign IAM role to access Ec2 and S3 storage. 

- **Cross-region Access**

- This is useful when you have 

  

## **Authorization** 

  

The process of specifying exactly what actions a principal can and cannot perform is called authorization 

  

## **Policies**

  

A policy is a JSON document that fully defines a set of permissions to access and manipulate AWS resources. Policy documents contain one or more permissions, with each permission defining: 

  

- Effect—A single word: Allow or Deny.  
    Service—For what service does this permission apply? Most AWS Cloud services 
- Support granting access through IAM, including IAM itself. 
- Resource—The resource value specifies the specific AWS infrastructure for which this permission applies. This is specified as an Amazon Resource Name (ARN). The format for an ARN varies slightly between services, but the basic format is: "arn:aws:service:region:account-id:[resourcetype:]resource”. 

  

  

## **Associating Policies with Principals** 

  

A policy can be associated directly with an IAM user in one of two ways: 

  

**User Policy:** These policies exist only in the context of the user to which they are attached 

**Managed Policies:** These policies are created in the policies tab on IAM page, these are independent and can be associated with any group or any single user. You can also write your own policy specific to your case. 

**Group policy:**  These policies exist only in the context of the group to which they are attached. 

  

  

**Best practice is to create a new IAM group called “IAM Administrators” and assign the managed policy, “IAMFullAccess.” Then create a new IAM user called “Administrator,” assign a password, and add it to the IAM Administrators group.**

  

**MFA(Multi-Factor Authentication)**

  

With MFA, authentication also requires entering a One-Time Password (OTP) from a small device. The MFA device can be either a small hardware device you carry with you or a virtual device via an app on your smart phone (for example, the AWS Virtual MFA app). 

  

- In multiple permission if there is any not any explicit allow, default is deny.
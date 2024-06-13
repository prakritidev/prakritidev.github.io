---
title: '"Database (RDS) and Redshift"'
draft: 
tags:
  - AWS
  - database
  - RDS
  - Redshift
  - dynamoDb
  - Aurora
---




  

**AMAZON RDS**

  

Amazon provides RDS service where AWS do everything on their own. Scaling, replicating, etc. We don’t have to do anything or we can put everything on EC2 and do everything on by own. For licensed databases like Oracle and Microsoft, you have to Bring Your Own License (BYOL) 

  

**Backup and recovery**

  

Amazon RDS provides two mechanisms for backing up the database: automated backups and manual snapshots. By using a combination of both techniques, you can design a backup recovery model to protect your application data.

  

**RPO**

  

RPO is defined as the maximum period of data loss that is acceptable in the event of a failure or incident. For example, many systems back up transaction logs every 15 minutes to allow them to minimize data loss in the event of an accidental deletion or hardware failure. 

  

**RTO**

  

RTO is defined as the maximum amount of downtime that is permitted to recover from backup and to resume processing. For large databases in particular, it can take hours to restore from a full backup. In the event of a hardware failure, you can reduce your RTO to minutes by failing over to a secondary node. You should create a recovery plan that, at a minimum, lets you recover from a recent backup. 

  

**Automated Backups**

  

One day of backups will be retained by default, but you can modify the retention period up to a maximum of 35 days 

  

**Manual DB Snapshots** 

  

Unlike automated snapshots that are deleted after the retention period, manual DB snapshots are kept until you explicitly delete them with the Amazon RDS console or the DeleteDBSnapshot action. 

  

**Security**

  

Deploy RDS in VPC with private subnet that limits network access to the DB Instance**.** 

  

**Amazon Redshift**

  

Amazon Redshift is a fast, powerful, fully managed, petabyte-scale data warehouse service in the cloud. 

  

**Clusters and Nodes**

  

The key component of an Amazon Redshift data warehouse is a cluster. A cluster is composed of a leader node and one or more compute nodes. The client application interacts directly only with the leader node, and the compute nodes are transparent to external applications. 

  

**Table Design** 

  

- **Data Types** 

- All data types are available 
- Additional columns can be added to a table using the ALTER TABLE command; however, existing columns cannot be modified

- **Compression Encoding** 

- One of the key performance optimisations used by Amazon Redshift is data compression. When loading data for the first time into an empty table, Amazon Redshift will automatically sample your data and select the best compression scheme for each column

- **Distributed Strategy** 

- The goal in selecting a table distribution style is to minimize the impact of the redistribution step by putting the data where it needs to be before the query is performed. 

- **Querying Data** 

- Allows you to write standard SQL query on your tables . 

- **Security** 

- First layer is IAM Policies 
- Private subnet 

  

**Amazon DynamoDB**

  

Amazon DynamoDB is designed to simplify database and cluster management, provide consistently high levels of performance, simplify scalability tasks, and improve reliability with automatic replication. Developers can create a table in Amazon DynamoDB and write an unlimited number of items with consistent latency. 

  

To maximize Amazon DynamoDB throughput, create tables with a partition key that has a large number of distinct values and ensure that the values are requested fairly uniformly. Adding a random element that can be calculated or hashed is one common technique to improve partition distribution
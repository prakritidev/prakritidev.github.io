---
title: '"S3 and Glacier"'
draft: 
tags:
  - AWS
  - security
  - s3
  - Glacier
  - storage
  - cost
---



  
## **Buckets** : Global namespace [best practice to save name as organisation name]. 

  

- Name with 63 lowercase number hyphens and periods. 
- 100 buckets per account.
- Created at a specific reason.
- Bucket can store unlimited number of objects 
- Private by default

  

**Objects:** Entities or files n Amazon in buckets. 

  

- Virtually can store any data in any format. 
- Storage from 0 kb to 5 TB. 

  

**Keys:** Each s3 Bucket has a unique Identifier called as a key. 

  

**S3 Operations :** 

  

- Create/Delete buckets. 
- Write/Read/Delete an object. 
- Key list in buckets 

  

**Durability and Availability :**

  

- 99.999999999% durability [Will my data still be there?]
- 99.99% availability  [can I access my data right now]?
- To protect from accident deletions use versioning, cross-region and MFA Deletion. 

  

**Data Consistency :**

- Provides read-after-write consistency.
- Updates to a single key are atomic — you’ll never get a mix of the data (old and new)

  

**Access Control :**

  

- Private by default
- ACLs (Access control List)

- Grants coarse-grained permissions : READ, WRITE, and Full-Control at bucket level or object level. 
- S3 Bucket policies are recommended access control for S3.

- Bucket policies 

- Bucket policies are similar to IAM role but on bucket level. 
- They include an explicit reference with IAM principle in policies. 
- S3 bucket policies allows cross-accounts access to S3. 
- You can allow and block anyone using CIDR and during what time of the day. 

  

**S3 Advance Features** 

  

**Prefixes and Delimiters :**

  

- S3 uses a flat structure in bucket. 
- Use prefix and delimiter for logical organise new data and maintain the hierarchy of the data.
- Can apply access control on prefixes and delimiters 

  

**Storage Class.** 

  

- S3 Standard 

- Delivers first-low byte latency and high throughput. 
- Best choice for frequently accessed data. 

- S3 Standard IA(Infrequent Access)

- Same durability as Standard , low latency, high throughputs. 
- Designed for long-lived data, less frequent access
- Pricing is less than standard, minimum duration limit of data is 30 days 
- Minimum object with 128kb. 
- per-Gb retrieval cost. 

- RSS (Reduced Redundancy Storage)

- Lower durability 99.99%
- Better for thumbnails like easy reproducible data. 
- Cheaper than over storages. 

  

**Amazon Glacier [small intro]**

  

- Best for storing achieves
- Free data access to 5% data in glacier. 
- Takes hours to restore 
- Makes a copy to RSS but data retains in the glacier until you delete it. 
- Its’s a standalone service. 

  

**Object lifecycle Management**

  

Hot(Frequently access data)-> Warm (Less frequent data)-> Cold (Long-term archival data)

  

Reduce storage cost and we can automate storage class based on the time. 

  

1. Initially on Amazon S3 Standard 
2. After 30 days, transition to IA 
3. After 90 days, Glacier 
4. After 3 years, delete. 

  

**Encryption** 

  

- Best practice is to Encrypt data at flight and at rest. 
- Amazon SSL Api endpoints 

- Assures that all data will be encrypted while in transit using HTTPS protocol. 

- Data encrypt at rest 

- Use SSE (Server Side Encryption)
- AWS encrypt data at object level before writing to disk and decrypt when you retrieve it. 
- SSL and Amazon KMS (Key management service) uses 256-bit AES (Advance Encryption Standards)
- Client side Encryption. 

- SSE-S3 (AWS manages key)

- Fully integrated “check-box-style”
- AWS handles this 

- SSE - KMS (key management Service)

- Keys are manages by us 
- Increase access control at object level 
- Track user activity at object level and user level 
- Attempt to login, failed login. Etc

- SSE-C (Customer - provided key)

- You control every thing 

- Best practice 

- Use server-side encryption with SSE-S3 or SSE-KMS

  

**Versioning** 

  

- It’s a protection against accidental deletion. 
- Create multiple versions of each object in your bucket. 
- Apply at bucket level 
- Once started it can’t be stopped, only suspended
- Data can be retrieved using previous version of your bucket

  

**MFA Delete** 

  

- Adds another layer of data protection at buckets 
- Maybe OTP or hardware for deletion something. 

  

**Multipart Upload** 

  

- Use for uploading larger files in multi parts
- Better network utilisation 
- Ability to pause and resume
- Can upload with unknown file size
- It's a three way process 

- Initialisation 
- Uploading into parts
- Completions 
- Upload in any manner 
- Aws re arrange the parts at their end 

- Its better to use multi part if file is more than 100 MB 

  

**Cross Region Replication** 

  

- Asynchronously copy S3 bucket from one region to another 
- Reduce latency  
- Versioning must be turned on to both destination and source buckets 
- Must use IAM policy to replicate object on your behalf 
- Any changes on source bucket trigger replication on destination bucket. 

  

**Logging** 

  

- For sureliance 
- Best practice is to save log in same bucket using a prefix 

  

**Event Notification** 

  

- Setup at bucket Level 
- Uses aws SNS or SQS
- Trigger messages on events on your bucket. 

  

**Best Practices, patters and performance**

  

- Make s3 as blob storage and index data to database this enables quick searches and complex queries to run. 
- If S3 uses extensive GET- requests use Amazon Cloud front distribution as caching layer 

  

**Amazon Glacier** 

  

- Used for long term Backup 
- Stores in Zip files

**Archives** 

- immutable after created
- Data stores in archives which can store upto 40 TB
- Can have unlimited number of archives 
- Automated encrypted 

**Vaults** 

- Container for archives 
- AWS account have limits upto 1000 vaults 
- Easily deploy and enforce compliance controls for Glacier vault 
- Write once read many (because they are immutable)
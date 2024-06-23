---
title: '"Data Collection And Database Choice"'
draft: 
tags:
  - Algorithm
  - AlgoTrading
  - architecture
  - Async
  - mongodb
  - queue
---
# Scrapping Process 

- [x]  Write a process in golang for downloading data from bse to download history  [[Efficient Data Processing on a Budget - My Journey with File-Based Queues]]
- [x] Write a process on golang for downloading intraday data [[A Personal Tale of Data Transformation - The Simple Genius of jq, GNU Parallel, and a Task Queue]]

# Database Selection 

First choice for MYSQL second was influxDB and third was mongodb. I went with mongodb as I did not knew what I will store and second I was eaiser to setup on ec2 with docker. 

Maybe later I can migrate to somthing better maybe a files based storage. (parquet, avro stuff) and put them in s3, and use minIO for fetching the data. 




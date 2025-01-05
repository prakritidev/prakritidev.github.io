---
title: '"Trading System - Low Level System Design"'
draft: 
tags: []
---

### **1. Create a Well-Defined Folder Structure for S3**
The idea is to organize the data into a hierarchy that is easy to query and retrieve later. The folder structure should follow a pattern that makes it clear where the data for each stock symbol is and which date it corresponds to.

For example, you can structure it like this:
```
s3://your-bucket/intraday/{stock_symbol}/{date}/data.csv
s3://your-bucket/daily_ohlcv/{date}/{stock_symbol}/ohlcv.csv
```

**Explanation:**
- `intraday/{stock_symbol}/{date}/data.csv`: This stores intraday data for each stock symbol, organized by date.
- `daily_ohlcv/{date}/{stock_symbol}/ohlcv.csv`: This stores daily OHLCV data, organized by date first (to optimize querying for a specific date range), and then by stock symbol.

### **2. Create a Script to Organize Data**
You need a script that:
- Scrapes the intraday data and stores it in a structured format on S3.
- Organizes data in folders as per the above structure.
- The script should handle:
  - **Folder creation**: It checks if a folder for that stock symbol and date exists. If not, it creates the folder.
  - **Data upload**: After scraping, the script uploads the data (in CSV or JSON format) to the appropriate S3 folder.

### **3. Automating the Process**
- **Scheduling**: Use a scheduling mechanism (like **cron** or **Airflow**) to run the script at defined intervals.
    - For example, you can run the intraday data scraper every 15 minutes, hourly, or daily based on your needs.
  
### **Script Example** (in Go or Python):
Here’s a simplified example using **Python** and the `boto3` library to upload data to S3:

```python
import boto3
import os
from datetime import datetime

# Initialize S3 client
s3_client = boto3.client('s3')

def upload_data_to_s3(stock_symbol, date, data, bucket_name):
    # Define the folder structure
    folder = f'intraday/{stock_symbol}/{date}/'
    s3_key = f'{folder}data.csv'
    
    # Upload data to S3
    s3_client.put_object(Body=data, Bucket=bucket_name, Key=s3_key)
    print(f"Data uploaded to {s3_key}")

# Example data
stock_symbol = 'AAPL'
date = datetime.now().strftime('%Y-%m-%d')
data = 'timestamp,price\n2024-11-17T12:00:00,150.00'  # This would be your scraped data
bucket_name = 'your-bucket-name'

upload_data_to_s3(stock_symbol, date, data, bucket_name)
```

This script uploads the scraped data for each stock symbol into the appropriate folder (`intraday/{stock_symbol}/{date}/data.csv`).

### **4. Benefits of This Structure**
- **Efficiency**: S3 folders are hierarchical and efficient when using AWS tools for querying (e.g., AWS Athena or Redshift Spectrum).
- **Scalability**: Storing data by stock symbol and date helps scale the data as you grow your dataset with more equities and over time.
- **Easy Data Retrieval**: You can easily query or load data by specifying the stock symbol and date range.

---
- [x] This pipeline is Build and ready on production.
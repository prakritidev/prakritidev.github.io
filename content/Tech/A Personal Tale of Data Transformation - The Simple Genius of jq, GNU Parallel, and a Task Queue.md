---
title: A Personal Tale of Data Transformation - The Simple Genius of jq, GNU Parallel, and a Task Queue
draft: 
tags:
  - GNU
  - parallel
  - jq
  - queue
---

As a lead backend engineer and an avid finance enthusiast, my journey has blended advanced programming with machine learning insights in the finance sector. Starting with Python, then moving to JavaScript and Go for their respective strengths, I eventually found that their capabilities were stretched by the demanding pace of my tasks. This led me to discover the efficiency of jq, GNU Parallel, and a file-based task queue. This powerful trio not only streamlined my stock market projects but also proved invaluable for parsing application logs and transforming data across various domains. Their impact was so profound that I’ve since incorporated them into complex tasks like transferring millions of events swiftly using SQS. I’m excited to share how these tools have simplified my approach beyond traditional programming.

## **What is jq?**

jq is like a Swiss Army knife for JSON data. It’s a lightweight and flexible command-line JSON processor. You can slice, dice, transform, and reshape JSON data in whichever way you fancy. The best part? Its syntax is intuitive and powerful, perfect for quick one-liners or complex transformations.

Let’s take an example json that I build myself (chatGPT) for better understanding.

```
{  
  "products": [  
    {"id": 1, "name": "Laptop", "price": 800, "inStock": true},  
    {"id": 2, "name": "Smartphone", "price": 500, "inStock": false},  
    {"id": 3, "name": "Tablet", "price": 300, "inStock": true},  
    {"id": 4, "name": "Smartwatch", "price": 200, "inStock": true},  
    {"id": 5, "name": "Camera", "price": 150, "inStock": false}  
  ]  
}
```
**Filtering by Price and Stock Availability**

- Task: Select products priced over $300 and currently in stock.
- Command: `jq '.products[] | select(.price > 300 and .inStock == true)' products.json`
- Output: `{ "id": 1, "name": "Laptop", "price": 800, "inStock": true }`

**Calculating Average Price**

- Task: Compute the average price of all products.
- Command: `jq '[.products[].price] | add / length' products.json`
- Output: `390`

**Combining and Deduplicating Data from Multiple Files**

- Additional File (`additional_products.json`): `[{"id": 6, "name": "Headphones", "price": 100, "inStock": true}, {"id": 1, "name": "Laptop", "price": 800, "inStock": true}]`
- Task: Merge product data from two files, removing duplicates.
- Command: `jq -s 'add | unique_by(.id)' products.json additional_products.json`

[  
  {"id": 1, "name": "Laptop", "price": 800, "inStock": true},  
  {"id": 6, "name": "Headphones", "price": 100, "inStock": true}  
]

Converting JSON to CSV Format

- Command: `jq -r '.products[] | [.id, .name, .price] | @csv' products.json`
```
1,"Laptop",800  
2,"Smartphone",500  
3,"Camera",150
```

As you can see we can do the things that we used to to with pandas or javascript etc. Imagine doing this calculation on mongodb exported documents data to maybe on an elastisearch query response (Elasticsearch has REST endpoints unlike others).

Now comes the GNU Parallel.

## **What is GNU Parallel?**

GNU Parallel is a tool for executing jobs in parallel, using one or more computers. It’s like a magic wand that takes a bunch of tasks and distributes them across available CPU cores, speeding up processing significantly. It’s particularly useful for repetitive tasks that can be run concurrently.

**Task**: Run a complex command involving pipes and redirections.

Command: `parallel "grep -E '^[^2][0-9]{2} ' {} | wc -l > {}.count" ::: access_log*.txt`

UseCase: I heavily use this when searching things in logs, specifically when shit hits the floor # funtimes

**Task**: You want to run a set number of jobs in parallel from a larger list of tasks.

command: `cat urls.txt | parallel -j 10 curl -O`

UseCase**:** This will download files from a list of URLs in urls.txt, running 10 downloads in parallel at any time. (Torrents I guess #collegedays), I use it for data scrapping POC though # fuckMyLife.

**Task**: Execute tasks in parallel but keep the output in the same order as input.

command: `parallel — keep-order echo ::: $(seq 1 5)`

UseCase: because sometime you need order in your life.

**Task**: Suppose you have a script process.sh that takes two arguments, and you want to run it for different combinations of arguments in parallel.

command: parallel ./process.sh ::: arg1 arg2 arg3 ::: argA argB

This command will execute ./process.sh with all combinations of the arguments, resulting in six executions:

```
- ./process.sh arg1 argA
- ./process.sh arg1 argB
- ./process.sh arg2 argA
- ./process.sh arg2 argB
- ./process.sh arg3 argA
- ./process.sh arg3 argB
```

UseCase: I used this to test our system with different settings, revealing varying outcomes. We suspected the database might cache data, so we gave it a shot. Surprisingly, we ran ElasticSearch and Spring Boot on a modest 2 GB server, hitting an impressive average of 300+ requests per second on an EC2 instance with 30 concurrent requests. This convinced us to embrace Docker for ElasticsSearch and our apps, giving us more control over hardware and the power to maximize CPU usage. #goodtimes.

**How to Emulate File-Based Queue Logic for Resuming Processing?**

The brilliance of a file-based task queue lies in its simplicity and dependability. Here’s a quick guide on how to set it up:

1. Create a Task File: This file lists all the tasks to be processed, one per line, like a to-do list.
2. Process with GNU Parallel: Run GNU Parallel, directing it to your task file. It picks up each task and executes it.
3. Logging Progress: GNU Parallel keeps a progress log. If the process halts, you can resume it, and Parallel will continue from where it stopped, skipping completed tasks.
4. Easy Resumption: This setup ensures that interruptions don’t require starting from scratch. Simply run the command, and it continues processing the remaining tasks.

# Create a tasks file  
```
echo "task1\ntask2\ntask3" > tasks.txt  
```
  
# Process with GNU Parallel  
```
parallel --joblog progress.log < tasks.txt
```

An example of web scraping using a combination of tools like jq, GNU Parallel, and a file-based queue and utilising the DummyJSON website (chatGPT). Keep in mind that you can also use these tools individually for different purposes.

```
#!/bin/bash  
```
  
# Create a task file with URLs  
```
echo "https://dummyjson.com/products/1" > tasks.txt  
echo "https://dummyjson.com/products/2" >> tasks.txt  
echo "https://dummyjson.com/products/3" >> tasks.txt  
echo "https://dummyjson.com/products/4" >> tasks.txt  
echo "https://dummyjson.com/products/5" >> tasks.txt  
```
  
# Create a queue file to store failed tasks  
```
touch queue.txt  
```
  
# Create a CSV file with headers  
```
echo "id,title,description,price,discountPercentage,rating,stock,brand,category" > output.csv  
```
  
# Function to process a task and append to CSV  
```
function process_task() {  
    local url="$1"  
      
    # Download JSON response and convert to CSV  
    local csv_data  
    csv_data=$(curl -s "$url" | jq -r '.id, .title, .description, .price, .discountPercentage, .rating, .stock, .brand, .category' | tr '\n' ',')  
  
    # Check HTTP status code  
    local http_status  
    http_status=$(curl -s -o /dev/null -w "%{http_code}" "$url")  
  
    if [ "$http_status" -eq 200 ]; then  
        # Append CSV data to output.csv  
        echo "$csv_data" >> output.csv  
        # Remove the processed task from tasks.txt  
        grep -v "$url" tasks.txt > tasks_tmp.txt  
        mv tasks_tmp.txt tasks.txt  
    else  
        echo "Failed: $url (HTTP Status: $http_status)"  
        # Append the failed task to the queue file for retry  
        echo "$url" >> queue.txt  
    fi  
}  
  
```
# Export the function so it can be used with parallel  
```
export -f process_task  
```
  
# Process tasks with GNU Parallel  
```
cat tasks.txt | parallel -j10 process_task  
```
  
# Clean up JSON files  
```
rm -f *.json  
```
  
# Verify that tasks.txt and queue.txt are empty  
```
if [ ! -s tasks.txt ]; then  
    rm tasks.txt  
fi  
if [ ! -s queue.txt ]; then  
    rm queue.txt  
fi
echo "Processing completed."
```
  

I’m pondering whether to write about how I turned a stock prediction challenge into a machine learning adventure or perhaps dive into the showdown between Java virtual threads and Spring WebFlux. Well, that’s the plan for now. Until then, jai ram ji ki.
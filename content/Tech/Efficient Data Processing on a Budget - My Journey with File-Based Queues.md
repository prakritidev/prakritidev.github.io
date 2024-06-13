---
title: Efficient Data Processing on a Budget - My Journey with File-Based Queues
draft: 
tags:
  - python
  - architecture
  - parallelProcessing
  - dataProcessing
  - queue
  - multithreading
  - multiprocessing
  - AsyncCodes
---

I want to share my experience with managing background tasks in a Python application, especially in the context of financial data processing. This isn’t just about choosing between file-based queues and Celery, but also about making decisions under hardware constraints and specific industry requirements.

## **The Challenge**

My task was to process intraday data for over 4,000 equities — a hefty amount of data! All this needed to be efficiently inserted into MongoDB, which was hosted on a modest server with just 1 core and 2 GB RAM, also running Docker. The catch? I had to do this within a tight budget and resource constraints.

## Why I Chose File-Based Queues?

Given the server limitations, I decided against using Celery, despite its robust features. Here’s why:

- Hardware Limitations: My server setup was quite basic — definitely not cut out for the additional load that comes with Celery and a message broker like Redis or RabbitMQ.
- Budget Constraints: As a small-scale operation in the finance sector, keeping costs low was crucial. More complex setups would mean more resources and potentially higher costs.
- Simplicity and Reliability: I needed something straightforward yet reliable. File-based queues allowed me to manage tasks effectively without overcomplicating the system.

The Setup and Results: I set up a file-based queue system with two worker threads using Futures. This approach was surprisingly efficient. The entire process of handling the intraday data for over 4,000 equities (Roughly 7 Lakh document processing) and inserting them into MongoDB took about 2.5 hours. Not bad, right? Especially considering the server constraints!

Here’s a Peek at the Code: To give you a better idea, here’s a simplified example of the file-based queue system I used:

import json  
import concurrent.futures  
  
## Sample task file (tasks.json)  
```
[  
{"task_id": 1, "data": "Task 1 data"},  
{"task_id": 2, "data": "Task 2 data"},  
 ...  
]
```  
  
```
def process_task(task):  
    print(f"Processing task {task['task_id']}: {task['data']}")  
    # Do Some processing or storing it to DB etc etc.   
  
def main():  
    try:  
        with open('tasks.json', 'r') as file:  
            tasks = json.load(file)  
        # Sadly I can't increase the workers due to hardware limition.  
        with concurrent.futures.ThreadPoolExecutor(max_workers=2) as executor:  
            future_to_task = {executor.submit(process_task, task): task for task in tasks}  
  
            for future in concurrent.futures.as_completed(future_to_task):  
                task = future_to_task[future]  
                try:  
                    future.result()  
                except Exception as e:  
                    print(f"Task {task['task_id']} generated an exception: {e}")  
                else:  
                    print(f"Task {task['task_id']} completed successfully.")  
  
        print("All tasks completed.")  
  
    except Exception as e:  
        print(f"An error occurred when reading the task file: {e}")  
  
if __name__ == "__main__":  
    main()
```
## When Would I Consider Celery?

Now, Celery is an excellent tool, no doubt. It shines in scenarios where you have:

- More Complex Task Management: If you’re juggling a multitude of tasks that need advanced routing or scheduling.
- Higher Scalability Needs: For applications that need to scale and distribute tasks across multiple servers.
- Resource Availability: If you have a server setup that can handle the extra load without breaking a sweat (or the bank!).

My journey with file-based queues has been a testament to the fact that sometimes, simplicity and resourcefulness trump complexity, especially in the world of finance where every penny and every millisecond counts. Whether you’re a fellow data enthusiast in finance or just someone exploring background task management in Python, I hope my experience sheds some light on the practical aspects of choosing the right tool for the right job!
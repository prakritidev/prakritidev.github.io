---
title: '"SQS, SNS, and SWF"'
draft: 
tags:
  - AWS
  - SQS
  - transactions
  - SWF
  - workflows
  - dataProcessing
  - queue
  - fanout
---



  **Amazon Simple Queue Service (Amazon SQS)** 

  

Amazon SQS is a fast, reliable, scalable, and fully managed message queuing service. Amazon SQS makes it simple and cost effective to decouple the components of a cloud application. You can use Amazon SQS to transmit any volume of data, at any level of throughput, without losing messages or requiring other services to be continuously available. 

  

If your processing servers cannot process the work fast enough (perhaps due to a spike in traffic), the work is queued so that the processing servers can get to it when they are ready 

  

**Delayed Queues and Timeouts**

  

Delay queues allow you to postpone the delivery of new messages in a queue for a specific number of seconds 

  

When a message is in the queue but is neither delayed nor in a visibility timeout, it is considered to be “in flight.” You can have up to 120,000 messages in flight at any given time. Amazon SQS supports up to 12 hours’ maximum visibility timeout. 

  

**Queue and Message Identifiers** 

  

Amazon SQS uses three identifiers that you need to be familiar with: queue URLs, message IDs, and receipt handles. 

When creating a new queue, you must provide a queue name that is unique within the scope of all of your queues. Amazon SQS assigns each queue an identifier called a queue URL, which includes the queue name and other components that Amazon SQS determines. Whenever you want to perform an action on a queue, you must provide its queue URL. 

  

**Dead Letter Queues** 

  

Amazon SQS provides support for dead letter queues. A dead letter queue is a queue that other (source) queues can target to send messages that for some reason could not be successfully processed. A primary benefit of using a dead letter queue is the ability to sideline and isolate the unsuccessfully processed messages. You can then analyze any messages sent to the dead letter queue to try to determine the cause of failure. 

  

  

  

**Amazon Simple Workflow Service (Amazon SWF)** 

  

In Amazon SWF, a task represents a logical unit of work that is performed by a component of your application. Coordinating tasks across the application involves managing inter-task dependencies, scheduling, and concurrency in accordance with the logical flow of the application. 

  

With Amazon SWF, you can implement, deploy, scale, and modify these application components independently. 

  

**Workflows**

  

**Same concept as airflow** 

  

Using Amazon SWF, you can implement distributed, asynchronous applications as workflows. Workflows coordinate and manage the execution of activities that can be run asynchronously across multiple computing devices and that can feature both sequential and parallel processing. 

  

**Workflow Domains**

  

Domains provide a way of scoping Amazon SWF resources within your AWS account. You must specify a domain for all the components of a workflow, such as the workflow type and activity types. It is possible to have more than one workflow in a domain; however, workflows in different domains cannot interact with one another. 

  

  

  

**Exams Essentials** 

  

**Know how to use Amazon SQS**. Amazon SQS is a unique service designed by Amazon to help you to decouple your infrastructure. Using Amazon SQS, you can store messages on reliable and scalable infrastructure as they travel between your servers. This allows you to move data between distributed components of your applications that perform different tasks without losing messages or requiring each component always to be available.

  

**Understand Amazon SQS visibility timeouts**. Visibility timeout is a period of time during which Amazon SQS prevents other components from receiving and processing a message because another component is already processing it. By default, the message visibility timeout is set to 30 seconds, and the maximum that it can be is 12 hours. 

  

Know how to use **Amazon SQS long polling**. Long polling allows your Amazon SQS client to poll an Amazon SQS queue. If nothing is there, ReceiveMessage waits between 1 and 20 seconds. If a message arrives in that time, it is returned to the caller as soon as possible. If a message does not arrive in that time, you need to execute the ReceiveMessage function again. This helps you avoid polling in tight loops and prevents you from burning through CPU cycles, keeping costs low. 

  

**Amazon SWF** gives you full control over implementing tasks and coordinating them without worrying about underlying complexities such as tracking their progress and maintaining their state.

  

**Amazon SNS** is a push notification service that lets you send individual or multiple messages to large numbers of recipients. Amazon SNS consists of two types of clients: publishers and subscribers (sometimes known as producers and consumers). Publishers communicate to subscribers asynchronously by sending a message to a topic.
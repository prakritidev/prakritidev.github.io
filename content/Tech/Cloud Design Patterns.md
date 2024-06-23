---
title: '"Cloud Design Patterns"'
draft: 
tags:
  - architecture
  - autoscaling
  - DesignPrinciples
  - distributed
---

## **Cloud Design Patterns**

  

**Scalability :** 

Architecture that scale in a linear manner where adding extra resources results in at least a proportional increase in ability to serve additional load**.** There are generally two ways to scale an IT architecture: vertically and horizontally

  

**Scaling Vertically :**  

Increasing the hardware capacity by stopping the instance and increase the capacity, Good for short term scaling. Not a good way to scaling for long shot because It has its limits and not always cost effective and reliable. 

  

**Horizontal Scaling :**

Increase the number of resources, but not all architecture are capable of horizontal scaling or to distribute their workload to multiple resources. Bellow are some scenarios fro horizontal scaling. 

  

**Stateless Applications**

- A stateless application is an application that needs no knowledge of previous interactions and stores no session information. 

How to distribute load to multiple nodes

- Push model: Use load balancers or Route53(does not help every time).
- Pull model: Asynchronous event-driven workloads.

**Statefull Applications:** 

- Databased are sate-full

**Distributed Processing :** 

- **Hadoop, AWS EMR.**
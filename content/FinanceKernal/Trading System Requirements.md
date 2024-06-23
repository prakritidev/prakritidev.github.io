---
title: '"Trading System Requirements"'
draft: 
tags: []
---

# **FinanceKernel: Trading system requirements.** 

  
1. Build a system based on swing trading using software principles and ML software engineering. 

  

**What is swing trading and what is necessary for build the software ?**

  

It’s a small to mid term investment strategy majorly depends upon mean either trend or mean reversal. 

  

  

**Prerequisite for the software. .?**

  

  

Initial amount: 10000. 

  

How much money can I loose in a day or in a specific time period: at any given day my any of my selected stock should not fall less than 3% and whole portfolio shouldn’t fall less than 2%. 

  

  

What is success to you: if all the stocks are increased by 1% then and portfolio by 2 that makes sense. On a daily basis. 

  

**How many portfolios do we need ?** 

  

1. For faster trading 1 day or 2 day. 
2. For long term trading like 7 days for 15 days. 

Each portfolios will have 10k in them. 

  

**Expected output from the software ?** 

  

The expected output is the list of stock for each portfolio in a sorted order of profitability. That is refreshed every day. 

  

**How do we measure the performance of the system and how will feedback work ?**

  

Short term portfolio will be measured based on the end of the day performance. 2 ways to look at the matrices. 

  

1. My overall investment should not go down more than 1 % on that day. Is good 
2. If increase of portfolio is 1% or greater than that is consider good. 
3. If 80% of the company have move 1% is good. 

  

Collect these matrices for 10 days and check the performance. If system is able to give these result 80% is considered good. That mean 8 days were profitable but 2 days were not profitable.

  

**How will feedback will work ?** 

  

We need this is as a reinforcement learning like system. This is not well thought yet how it will work. TBD.
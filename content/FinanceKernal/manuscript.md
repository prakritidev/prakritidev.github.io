# Introduction



### Project Overview: Building a System for Consistent Daily Profits

The goal of this project is to explore the feasibility of building a system that can consistently achieve an average daily profit of 1%. Although it is widely recognized that this task is practically impossible due to the generally efficient nature of financial markets, the project serves as a valuable learning experience. Key areas of focus include:

1. **Understanding Time Series Data:**
   - Learning how to handle and analyze time series data, which is critical for financial modeling and forecasting.

2. **Data Handling and Processing:**
   - Exploring techniques for managing large datasets, including data cleaning, transformation, and normalization.

3. **Database Selection:**
   - Investigating different databases to determine which are best suited for storing and querying financial data efficiently.

4. **Portfolio Selection:**
   - Learning how to select a diversified portfolio of assets to optimize returns and manage risk.

5. **Risk Management:**
   - Implementing strategies to minimize losses and protect capital, including stop-loss orders, position sizing, and diversification.

While the primary objective may be ambitious, the knowledge and skills gained through this project will be invaluable for understanding the complexities of financial markets and quantitative trading.







---

# Low Level System Design


# **FinanceKernel: Low Level Design**


Q: How to setup the flow ? which thing should we build first ?

  
A: We need to learn about fundamental stuff and which technical indicators works for eveyone in swing trading. 

Also how charting works and which charts to include. 



Build an understanding of it and the create a backtesting unit test for it. Create the backtesting module using OOPS logics this will help a lot and make it configurable for strategies. 
  

Try to make profit by the understanding of techincals and charting. code it and test it. 
  

Q: Stock Selector Module. 


Build this next time, this will filter down to 200 - 500 potential stocks. The use ML to filter stock that will give profit in next day using ML pipeline. 
  

Q: Signal Module. 


This model should tell, should we but that filter stock or not, what should be the size of our trade and the risk analysis as well. Even if stock looks promising and its ambguos to buy then we dont do trade it. 

  

Q: Trader Module.  
  
This will be done manually.

---

# Trading System Requirements



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

---

# Swing Trading - 01




---

# Swing Trading - 02




---

# Stock Selection Algorithm


# How to select the stocks ?

There are multuple ways of doing it. Below are the steps we'll follow. 



## Naive Selection Algo. 

1. For all stocks, calculate daily closing price difference.
2. combine them all and then sort then by the diference. 
3. Select first 20% as positive and last 20% as -ve. 
4. 

### Naive Selection Algo Results (Day 1).

### Naive Selection Algo (Day 2).
### Naive Selection Algo (Day 3).

### Naive Selection Algo (Day 5).

### Naive Selection Algo (Day 8).
### Naive Selection Algo (Day 13).
### Naive Selection Algo (Day 21).


---

# Data Collection And Database Choice


# Scrapping Process 

- [x]  Write a process in golang for downloading data from bse to download history  Efficient Data Processing on a Budget - My Journey with File-Based Queues
- [x] Write a process on golang for downloading intraday data A Personal Tale of Data Transformation - The Simple Genius of jq, GNU Parallel, and a Task Queue

# Database Selection 

First choice for MYSQL second was influxDB and third was mongodb. I went with mongodb as I did not knew what I will store and second I was eaiser to setup on ec2 with docker. 

Maybe later I can migrate to somthing better maybe a files based storage. (parquet, avro stuff) and put them in s3, and use minIO for fetching the data. 





---

# High Level Design




# **FinanceKernel: High level Design** 

  

Q: We Have NSE and BSE, over 4000+ listed stocks, how to choose which stocks to select for the portfolio. 

A: We have 3 Types, Large, mid, small. 


We might need to create 3 portfolio and divide investment amongst them.

## Fundamental Analysis (Manual process or maybe a script for this)

  

These are stable companies as they are big and dont take much risk. We can filter them based on Fundamental ratios like P/E, P/B, Intrinic value and other ratios. Make find some rules for swing trading for fundamential ratios. 

## Techincal Indicators and charts. 


We need to understand the rules of tedhcnical indicators how they work on time lags. This will tell the history and tendency. 

We also need to understand about charts gives us entry and exist points hence give us the rules of taking the bet if it will go down or up on that day or week etc when combined with Technials.  
  
This will give us the list of stock to trade or invest into.  

  

Q: We might have 1000 Stocks after this. How to pick now ? 


A: We need to can pic top 50 stock for each portfolio.



Q: How do you validate this process that selection is good?

  

A: We can implement a backtest on all those companies and check if we are profitable based on our timelines and target. We maybe just can use some basic rules for sell and buy and check the profitability. And compare this with Nifty 50 performance.  


This will give a +ve signal if the filtering process was right or not.


Q: As we don’t know what impacts the price to move up or down. How to learn this without learning as might have very naive idea like moving average charting etc. 

  

A: Based on our knowledge of how technical analysis works we might be able to model that ? Plus we can add some time series feature extraction that can identify  the pattern. 


Q: how to Model this problem into ML ? Or DL or reinforcement Learning ? 


A: We can see this as multi process step or more of a pipeline. 

  

First target is to model the market or learn what parameters have to be set in order to make the profit. 
  

As a feature we can add the technical indicators as they tells a lot, we can add charting data as well that

The Target need to labeled, we can label based on multiple factors like, mark the data true if the profit of the stock next day is > 1% and last 3 days were in profit as well. Or maybe we just focus on a model that can tell me when will stock go down. If it went up yesterday. We can learn these patterns. 

  

We can just use the idea of triple barrier method as well, set out profit and loss criteria, like 1 % is good but loss of 1 % is not good. We have to fix the timeline first. Let’s just focus on 1 day, 

  

We aim to learn the patterns using technical data and charting that when was I able to make profit > 1 % and when did I make a loss. 

  

Model will tell us to buy a stock or not. It will not tell how much this will increase but it will increase by 1% with some confidence levels. Same model should tell me how much a stock will go down as well. 

  

With these signals, we will trust our system and run it through out backtesting pipeline, if it performs better than our stock selection process that means model labeling works and feature selection also works. 

  

Q: How to manage for loss and diversification. ? 

  

We will not go into that as of now. Let's work on baby steps.

---

# Derivative 


Value is derived from other thing. 

Types: 
1. Forward 
2. Futures 
3. Swaps 
4. Options

# Forward and Swaps 

Forward contract is you think price will go up in the future. Today the price is 10 but you believe it will go to 20 in next 3 months. 
You put the contract for 15 and wait for it. If it goes up you'll get a profit of 5. otherwise loss.

#### Types of Forward

1. This month 
2. Next Month 
3. Far Month

# Futures 

We have to but it at the end of the month. But these contract in itself are tradable. 

Options 

options are buying and selling 


Call(BUY) and pull(SELL)


---

# undefined



---

# undefined


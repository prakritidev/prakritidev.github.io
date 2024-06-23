---
title: '"High Level Design"'
draft: 
tags:
  - financekernel
  - basicIdea
  - AlgoTrading
---


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
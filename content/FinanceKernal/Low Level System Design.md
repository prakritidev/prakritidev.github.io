---
title: '"Low Level System Design"'
draft: 
tags:
  - AlgoTrading
  - financekernel
  - basicIdea
---
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
---
title: '"Stationary Time Series"'
draft: 
tags:
  - Machine-Learning
  - Time-Series
  - AlgoTrading
---
Parsimony => The idea of good amount of params so that model don't overfit 

# What is the real meaning of it ? 

If we have a timeseries of say Y0, Y1....YN with a certian joint distribution. and shifting the data by m units hence Ym+1, Ym+2, .... YM+N

If both had same joint probability distribution, means All the aspects remains the same even if went ahead of the time. The its stationaty. 

Always pick adjusted close price.


## See how data looks like before predicting. 


1. Trend/Cycle 
2. Season
3. Error


You can adjust the data based on season, delete season from timeline 

## How this is done, Do time series decomposition? 

People use moving averages etc for removing error but morders is 

1. LOESS Regression
Do expenential smoothing models 

---
title: '"Trading System - Feature Engineering - Part 1"'
draft: 
tags:
  - AlgoTrading
  - feature-engineering
---
Here’s the full table with all the features we discussed, including custom features and TA-Lib indicators:

  

**Feature** **Custom Feature/Calculation** **TA-Lib Indicator/Function** **Description**

Log Returns log_return = np.log(close / close.shift(1)) NA Measures daily return as the logarithmic difference between consecutive closing prices.

Cumulative Returns cumulative_return = (1 + log_return).cumprod() - 1 NA Cumulative return over time based on daily log returns.

Rolling Volatility (Std Dev) rolling_volatility = df.groupby('isin')['log_return'].transform(lambda x: x.rolling(window=14).std()) NA Rolling standard deviation of returns, capturing volatility over a specific window.

Rolling Momentum (Mean Return) rolling_momentum = df.groupby('isin')['log_return'].transform(lambda x: x.rolling(window=14).mean()) NA Measures the average momentum over a rolling window of time.

14-day Rolling Max Price rolling_max = df.groupby('isin')['close'].transform(lambda x: x.rolling(window=14).max()) NA Maximum closing price over the past 14 days.

14-day Rolling Min Price rolling_min = df.groupby('isin')['close'].transform(lambda x: x.rolling(window=14).min()) NA Minimum closing price over the past 14 days.

Price Range (Close - Low) price_range = close - low NA Difference between the closing price and the lowest price of the day.

Volume-based Percentage volume_pct = df.groupby('isin')['volume'].transform(lambda x: x / x.rolling(window=14).sum()) NA Measures the stock’s volume as a percentage of its total trading volume over a window.

Rolling Trading Volume rolling_total_trading_volume = df.groupby('isin')['totaltradingvolume'].transform(lambda x: x.rolling(window=14).sum()) NA Rolling sum of the total trading volume over the past 14 days.

Rolling Trade Value rolling_total_trade_value = df.groupby('isin')['totaltradevalue'].transform(lambda x: x.rolling(window=14).sum()) NA Rolling sum of the total trade value over the past 14 days.

Rolling Number of Trades rolling_total_number_of_trades = df.groupby('isin')['totalnumberoftradesexecuted'].transform(lambda x: x.rolling(window=14).sum()) NA Rolling sum of the number of trades executed over the past 14 days.

Simple Moving Average (SMA) NA SMA(close, timeperiod=7) A moving average of the closing price over a fixed period (e.g., 7 days).

Exponential Moving Average (EMA) NA EMA(close, timeperiod=14) A weighted moving average giving more weight to recent prices.

Relative Strength Index (RSI) NA RSI(close, timeperiod=14) Measures the magnitude of recent price changes to evaluate overbought/oversold conditions.

Average True Range (ATR) NA ATR(high, low, close, timeperiod=14) Measures volatility by decomposing the entire range of price movement.

Bollinger Bands NA BBANDS(close, timeperiod=20, nbdevup=2, nbdevdn=2, matype=0) A band plotted above and below a moving average, capturing volatility.

Moving Average Convergence Divergence (MACD) NA MACD(close, fastperiod=12, slowperiod=26, signalperiod=9) Shows the relationship between two moving averages of a stock’s price.

Stochastic Oscillator (Stoch) NA STOCH(high, low, close, fastk_period=14, slowk_period=3, slowd_period=3) Indicates overbought/oversold conditions by comparing a stock’s closing price to its price range over a period.

On-Balance Volume (OBV) obv = (df['volume'] * np.sign(df['close'].diff())).cumsum() OBV(close, volume) A cumulative volume-based indicator that shows whether volume is flowing into or out of a stock.

Moving Average of Volume (MAV) mav = df['volume'].rolling(window=14).mean() NA Moving average of volume over a rolling window.

Relative Volume (RVOL) rvol = df['volume'] / df['volume'].rolling(window=14).mean() NA Measures volume relative to the average volume over a set period.

Price-to-Earnings Ratio (P/E) pe_ratio = df['close'] / df['earnings'] (if earnings data is available) NA Ratio of the stock’s price to its earnings per share.

Price-to-Book Ratio (P/B) pb_ratio = df['close'] / df['book_value'] (if book value data is available) NA Ratio of the stock’s price to its book value.

Moving Average of Volume-to-Price Ratio mav_vpr = df['volume'] / df['close'].rolling(window=14).mean() NA Measures the relationship between volume and price, averaged over a rolling window.

14-day Average of Price Range price_range_avg = df.groupby('isin')['price_range'].transform(lambda x: x.rolling(window=14).mean()) NA The average range between closing price and low price over a rolling window.

  

This table includes both custom features we discussed and the corresponding TA-Lib indicators, along with their descriptions and Python code where applicable. Let me know if you need further adjustments!
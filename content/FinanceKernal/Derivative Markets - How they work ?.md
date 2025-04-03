---
title: '"Derivative Markets - How they work ?"'
draft: 
tags:
  - AlgoTrading
  - forward
  - Derivative
---
# Understanding Derivatives

## What are Derivatives?

Derivatives are financial contracts whose value depends on an underlying asset like stocks, commodities, currencies, or indices. They are used for hedging risk, speculation, and arbitrage. In India, derivatives trade on NSE and BSE.

## Types of Derivatives

### 1. Forward Contracts

A forward contract is an agreement to buy or sell an asset at a fixed price on a future date. These contracts are private and not traded on exchanges, meaning they carry counterparty risk.

**Example (Gold Trader in India):** A jeweler in Mumbai expects gold prices to rise. Gold is currently at Rs. 6,500 per gram. They enter a forward contract to buy gold at Rs. 6,800 per gram in three months. If prices rise to Rs. 7,000, they save Rs. 200 per gram. If prices fall to Rs. 6,400, they incur a Rs. 400 loss per gram.

### 2. Futures Contracts

Futures are similar to forwards but standardized and traded on exchanges, reducing counterparty risk. They require margin deposits and are marked to market daily.

**Example (NIFTY 50 Futures):** NIFTY 50 is at 22,000. A trader expects it to rise and buys a futures contract at 22,100. If NIFTY rises to 23,000, they make Rs. 900 per unit. If it falls to 21,500, they lose Rs. 600 per unit.

### 3. Options

Options give the right (but not obligation) to buy or sell an asset at a fixed price before expiry.

- **Call Option**: Right to buy, used when expecting a price rise.
    
- **Put Option**: Right to sell, used when expecting a price drop.
    

**Example (Reliance Stock Options):** Reliance trades at Rs. 2,500. A trader buys a call option with a Rs. 2,600 strike price, paying a Rs. 50 premium. If Reliance rises to Rs. 2,800, they profit Rs. 150 (Rs. 200 gain - Rs. 50 premium). If the stock stays below Rs. 2,600, they lose the Rs. 50 premium.

### 4. Swaps

Swaps involve exchanging cash flows. They are used for managing interest rate or currency risks.

**Example (Interest Rate Swap for Indian Companies):** Tata Motors has a floating rate loan but wants fixed payments. HDFC Bank has fixed-rate obligations but wants floating payments. They enter a swap agreement to exchange their cash flows.

## How to Make Money Using Derivatives?

1. **Hedging** - Reduce risk (e.g., a farmer locks wheat prices to avoid loss from falling prices).
    
2. **Speculation** - Predict price movements and profit (e.g., buying NIFTY futures expecting a rise).
    
3. **Income Generation** - Selling options for premiums (e.g., selling covered calls on stocks).
    
4. **Arbitrage** - Exploit price differences across markets (e.g., buying gold on MCX at Rs. 6,450 and selling internationally at Rs. 6,500).
    

## Data Needed for Forecasting & Modeling

- **Historical Price Data** - Trends of NIFTY, gold, USD/INR, etc.
    
- **Market Indicators** - RBI policy, interest rates, GDP growth.
    
- **Volatility Metrics** - India VIX (volatility index).
    
- **Forward Curves** - Futures pricing trends.
    
- **Sentiment Analysis** - News, social media, FII/DII data.
    

Understanding derivatives can help manage risk and improve investment decisions, but they also carry significant risk. Proper research and risk management are key to successful trading.


# TODO: How to Collect data for this.



## Methods to Model Derivatives & Market Movements

1. **Time Series Analysis** - ARIMA, GARCH models for volatility forecasting and TCN, AutoEncoder for feature extractions
    
2. **Machine Learning Models** - Random Forest, XGBoost, and LSTMs for predictive modeling.
    
3. **Reinforcement Learning** - Multi-armed bandits for strategy selection.
    
4. **Monte Carlo Simulations** - Used to model and predict price movements under different scenarios.
    
5. **Options Pricing Models** - Black-Scholes, Binomial Trees for pricing options accurately.
    

## Algorithms with High Success Rate (>50%)

1. **Mean Reversion Strategies** - Works well in sideways markets (e.g., Bollinger Bands, Statistical Arbitrage).
	1. Using TCN and other information bars looked promising on local developement.
2. **Momentum Strategies** - Buying assets that show strength and selling weak ones (e.g., Moving Averages, RSI-based trading).
	1. ARIMA, GRACH, Stuff.
3. **Volatility-Based Strategies** - Using VIX to determine market uncertainty and adjust positions accordingly.
	1. Not explored yet.
4. **Pair Trading** - Finding correlated assets and trading deviations from their normal price relationships.
	1. Can be modeled using Graph Theory, #todo: Need to write notes on it again.
---
title: '"TimeSeries - Part 1"'
draft: 
tags:
  - Time-Series
  - seasonality
---
## Time Series Notes (for Financial Forecasting - BSE Index examples)

These are my running notes as I dig deeper into time series analysis with a focus on financial markets. I’m looking at the BSE index and building intuition for what matters when forecasting returns or volatility.

---

###  Statistical Moments  
Quick sanity check:
- **Mean** – avg return. Helps in understanding drift.
- **Variance** – how noisy the series is.
- **Skewness** – asymmetry. Important for modeling tail risk.
- **Kurtosis** – fat tails? Happens a lot in finance. Crashes, rallies.

In BSE daily returns: high kurtosis, slight negative skew → I can't just assume Gaussian behavior. Need fat-tailed models (t-distributions or nonparametrics).

Ignoring these = model might look fine on paper, but will collapse in rare events.

---

###  Stationarity  
**Core concept.** A stationary series has constant mean, variance, and autocovariance.
- Stock **prices** = non-stationary (they trend)
- **Returns** = often close to stationary

Check with:
- **ADF Test** (Augmented Dickey-Fuller)
- **KPSS Test** (Kwiatkowski–Phillips–Schmidt–Shin)

Also use **rolling stats** to visualize mean/var shifts.

If I ignore this:
- I might fit models on non-stationary data → pseudo relationships
- Predictions drift

---

###  Differencing  
Most common trick:
- First difference: `X_t - X_(t-1)`
- Log diff = log returns → already pretty usable

Apply until stationarity is achieved. Be careful not to over-difference (can remove signal).

---

###  Log Transform  
Useful when price grows exponentially (BSE has done this long term).
- Log(price) stabilizes variance
- Log returns = additive → modeling becomes cleaner

Skip this and recent data dominates scale.

---

###  Autocorrelation  
If past values influence current → there's memory.
- Check ACF/PACF plots
- Prices: strong autocorr
- Returns: weak/short autocorr

Useful for building lag features.

If I don’t check this:
- I may use unnecessary lags → overfitting
- Or miss exploitable structure (esp. in volatility)

---

### Volatility Clustering  
Returns = mostly white noise. But squared returns = show memory.
- High vol periods → stick together
- GARCH captures this

For BSE, spikes cluster. Need this to forecast **risk** correctly.

---

###  Long Memory / Persistence  
Some financial time series decay **slowly** → long-term autocorrelation.
- Volatility is a classic example
- Model with **ARFIMA** or fractional differencing

Ignoring this = underestimate market stress duration

---

### Nonlinearity  
Finance is rarely linear. Past values may not explain future directly.
- BDS test, Hinich test to detect
- Consider nonlinear AR (TAR, SETAR), or trees/NLP models

Linear-only models = underfit nonlinear alpha

---

###  Seasonality  
Not obvious in stocks, but exists (budget days, earnings cycles, Diwali rally, etc.)
- Monthly/quarterly returns sometimes show patterns
- Use STL decomposition

If I ignore this:
- Model sees regular events as anomalies

---

###  Structural Breaks (within series)
The statistical properties can shift. Not always macro.
- Variance, mean, or correlation regime changes
- Detect with **CUSUM**, **Chow Test**

Ignore this = past patterns no longer apply → backtest invalid

---

###  Cointegration  
Two non-stationary series might have a stationary linear combo.
- NIFTY & BSE500 could be cointegrated
- Useful for pairs/spread trading

Use Johansen test or Engle-Granger.

Skip this and I miss relative value setups.

---

###  Mean Reversion  
Series wants to revert to a long-term mean.
- Use ADF to test
- Ornstein-Uhlenbeck process is a model for this

Apply to spreads, z-scores, etc.

Assuming mean reversion when not → buy dips into downtrends

---

###  Multiscale Behavior  
Behavior changes at different timeframes.
- Intraday: noisy/mean-reverting
- Daily: maybe trending
- Monthly: clear macro trends

Use **Wavelet Transforms** or **EMD** to decompose.

If I ignore this:
- Might model noise as trend

---

###  Spectral Analysis  
Not core, but can help spot cycles.
- Use FFT or periodogram
- Find dominant periodic components (e.g. 20-day rhythm?)

Can help decide moving avg or filter window size.

---

###  Filtering & Smoothing  
Extract signals from noisy price series:
- Moving Avg (SMA, EMA)
- Kalman filter
- Low-pass filters

Good pre-processing before modeling or signal generation.

---

## TL;DR (for my workflow)

- Start with log transform → log returns
- Check stationarity (ADF/KPSS)
- Use differencing if needed
- Always look at autocorrelation & volatility structure
- Watch for structural breaks and seasonal patterns
- Use cointegration for relative value ideas
- Don’t assume linearity
- Always model volatility separately
- Use filtering and multi-scale ideas to clean signal before modeling

---

These notes evolve as I test things on actual BSE Index data — I’ll branch these into separate .md files when the experiments get deeper (especially around GARCH, TCN, and cointegration trading setups).


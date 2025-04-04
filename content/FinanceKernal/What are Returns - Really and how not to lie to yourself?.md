---
title: '"What are Returns ?"'
draft: 
tags:
  - AlgoTrading
  - returns
  - log-returns
  - return-metrices
  - basics
---


# Part 1: Returns from annual timeframes.


#### Compounding Returns (Don’t Just Add Returns!)

Returns stack **multiplicatively**, not additively.  
For example:

- Day 1: +10%
    
- Day 2: -3%
    

You might think:  
`10% - 3% = 7%` →  Not true.

Real return is:  
`(1 + 0.10) × (1 - 0.03) - 1 = 6.7%`

That’s because once you gain 10%, your base changes. So any future returns apply to the new amount.

**Another Example:**

- Day 1: +20% → value becomes 1.2x
    
- Day 2: -20% → value becomes 1.2 × 0.8 = 0.96  
    Total return = -4%, **not 0%**!
    

---

#### Annualized Returns – Why We Use Them

If an investment grows 1% a month, it’s tempting to just say:  
`1% × 12 = 12%` – but that’s **too simplistic**.

Each month builds on the last. So:

`(1 + 0.01)^12 - 1 = 12.68%`

That little difference can really add up over time.

**Example:**

- Monthly return = 2%
    
- Annualized = `(1.02)^12 - 1 ≈ 26.8%`  
    Much higher than just `2% × 12 = 24%`
    

---

#### Converting to Annual Terms

Just plug into this formula:  
`(1 + periodic_return)^periods_per_year - 1`

Quick guide:

- Daily → Annual: `^252` (trading days)
    
- Weekly → Annual: `^52`
    
- Monthly → Annual: `^12`
    

---

#### Comparing Investments Fairly

Let’s say:

- Strategy A: 1% return every month
    
- Strategy B: 5% return weekly
    

They don’t _look_ that different until we anualize:

- A: `(1.01)^12 - 1 = 12.68%`
    
- B: `(1.05)^52 - 1 ≈ 1039%`
    

Same initial impression, completely different long-term outcome.  
**Always annualize if you’re comparing.**

**Example:**

- Investment 1: +6% over 6 months → Anualized = `(1.06)^2 - 1 ≈ 12.36%`
    
- Investment 2: +4% over 3 months → Anualized = `(1.04)^4 - 1 ≈ 16.99%`
    

Investment 2 has a better anualized return despite lower short-term gain.

---

#### Log Returns – Why Nerds Use Them

Log returns (aka continuously compounded returns) make life easier when modeling stuff.

Instead of multiplying returns across periods, you **add them**.

Formula:

`log_return = ln(P_t / P_t-1)`

Example:  
From 100 → 110

- Simple return = 10%
    
- Log return = ln(1.1) ≈ 9.53%
    

Not a big deal over 1 day, but **over 1000s of days**, the math makes a difference.

**Multi-period Example:**

- Day 1: 5% up → log = ln(1.05) ≈ 4.88%
    
- Day 2: 4% down → log = ln(0.96) ≈ -4.08%
    
- Total log = 0.8% → Actual = `exp(0.008) - 1 ≈ 0.8%`
    

Bonus:  
Annualized log return = daily_log × 252

---

#### Risk-Adjusted Returns (Because Raw Return ≠ Good Return)

Returns alone don’t tell the full story—you’ve gotta ask: **How much risk did I take to get that?**

---

**Sharpe Ratio**

Formula:  
`(Portfolio return - Risk-free rate) / Std dev of returns`

Higher = better. Tells you how much bang you're getting per unit of risk.

**Example:**

- A: 15% return, 10% volatility → Sharpe = 1.5
    
- B: 20% return, 25% volatility → Sharpe = 0.8
    

A is better on a risk-adjusted basis.

---

**Sortino Ratio**

Like Sharpe, but only punishes **downside volatility**.  
Formula:  
`(Return - Risk-free rate) / Downside deviation`

More aligned with how humans actually think about risk (we don't mind upside swings, right?).

---

**Max Drawdown**

The scariest stat:  
How far your portfolio drops from a peak before recovering.

Formula:  
`(Bottom - Peak) / Peak`

**Example:**  
Peak = 100, Drops to 70 → Max Drawdown = `(70 - 100)/100 = -30%`

Useful for understanding worst-case scenarios. Some high-return strategies also crash hard.

---

### Deeper Dive: More Ways to Understand Returns (Important Stuff...)

---

#### CAGR – Compound Annual Growth Rate

This tells you the **smoothed annual return** as if your investment grew at a steady pace.

Formula:  
`CAGR = (Ending Value / Beginning Value)^(1 / n) - 1`

**Example:**

- 100 → 150 in 3 years
    
- CAGR = `(150/100)^(1/3) - 1 ≈ 14.47%`
    

Great for understanding long-term performance, especially when yearly returns are uneven.

---

#### Total Return vs. Price Return

- **Price Return** is just the stock price movement.
    
- **Total Return** includes dividends (assuming reinvestment).
    

**Example:**  
Stock rises 8%, dividend yield 2% → Total return = 10%

Always prefer Total Return when analyzing true performance.

---

#### Real Return vs. Nominal Return

- **Nominal** = what you see on paper.
    
- **Real** = adjusted for inflation.
    

Formula:  
`Real ≈ (1 + Nominal) / (1 + Inflation) - 1`

**Example:**

- Nominal return = 8%
    
- Inflation = 5%
    
- Real = `(1.08 / 1.05) - 1 ≈ 2.86%`
    

Real return tells you if your wealth is actually growing.

---

#### Volatility Drag

Volatility hurts compounding. Even with the same average return, higher volatility leads to lower final value.

Approx formula:  
`Geometric Return ≈ Arithmetic Return - 0.5 × Variance`

**Example:**

- +20% one year, -20% next → Final value = `1.2 × 0.8 = 0.96` (i.e., -4%)
    
- Average return = 0%, but actual wealth declined.
    

More swings = less money over time.

---

#### Time-Weighted vs. Money-Weighted Returns

- **Time-Weighted (TWR)**: Neutral to deposits/withdrawals. Good for comparing managers.
    
- **Money-Weighted (MWR)** = IRR: Reflects actual investor experience (includes cash flows).
    

**Example:** If you invest a lot right before a downturn, MWR will be worse than TWR.

---

#### Internal Rate of Return (IRR)

Used in private equity, VC, real estate. Tells you the effective annual return, considering all cash inflows and outflows.

**Example:**  
You invest $1000, get $200 back yearly for 6 years → IRR = return rate that makes NPV = 0

---

#### Rolling Returns

Instead of looking at fixed periods, rolling returns measure performance over moving windows (e.g., 3-year return calculated every month).

Helpful to understand consistency and timing sensitivity.

**Example:**

- Fund A: Consistently 7–9% every 3-year window
    
- Fund B: Swings from -5% to +20%
    

Fund A shows stability, B shows timing risk.

---

#### Alpha & Beta

- **Alpha**: How much you beat the market.
    
- **Beta**: How much your portfolio reacts to market moves.
    

**Example:**

- Beta = 1 → matches market
    
- Beta > 1 → more volatile
    
- Alpha = +3% → you outperformed after adjusting for market exposure
    

Helps you understand active skill (alpha) vs market exposure (beta).

---

#### Downside Capture Ratio

Measures how much you lose when the market falls.

- < 100% → You're losing less than market.
    
- > 100% → You're losing more than market.
    

**Example:**

- Market down 10%, Fund down 7% → Downside capture = 70%
    

Useful to see how protective your strategy is during bear markets.

---

#### Return Path Matters

Two portfolios can start and end at the same point, but the journey might be very different.

One might be smooth and steady, another could be a rollercoaster.

**Example:**

- Portfolio A: +1% per month
    
- Portfolio B: +20%, -15%, +10%, -12%, etc.
    

Both may end up at similar values, but B causes way more anxiety.

Visualizing the return path (e.g., via equity curves) tells you more than just start-to-end return.

---

### TL;DR

-  **Returns compound** — don’t just add them up. Multiplication matters.
    
-  **Always annualize** returns for apples-to-apples comparison.
    
-  **Use log returns** for better math modeling and easier aggregation.
    
-  **Risk-adjusted metrics** (Sharpe, Sortino, Max Drawdown) > raw returns.
    
-  **CAGR** smooths uneven performance; tells true long-term growth.
    
-  **Total > Price return** — dividends matter!
    
-  **Volatility hurts compounding** — higher swings = less money.
    
-  **Real return** = nominal minus inflation. Only real growth counts.
    
-  **TWR vs. MWR**: Know whether you’re judging the investment or the investor.
    
-  **IRR** helps evaluate performance when there are cash flows.
    
-  **Rolling returns** show consistency, not just endpoint luck.
    
-  **Downside capture** and **drawdowns** show how painful losses can be.
    
-  **Alpha & Beta**: Skill vs. exposure. Know what you’re really earning.
    
-  **Return path matters** — not just the destination, but the journey.


# Part 2: Since I am getting old, I wanna get rich quickly. How to Look at return daily, weekly or monthly timeframe (7 din mei paisa double...).


You don’t always need to annualize—**sometimes it's more useful to analyze returns in their natural timeframe**, especially for short- or medium-term trends.

####  What’s the difference?

- **Daily Return**: Measures change from one day to the next  
    `Return = (P_today / P_yesterday) - 1`
    
- **Weekly Return**: From previous week's close to current week’s close  
    `Return = (P_week_end / P_week_start) - 1`
    
- **Monthly Return**: Same idea, just measured monthly  
    `Return = (P_month_end / P_month_start) - 1`
    

These returns can be:

- **Simple (arithmetic)**: Just % change 
    
- **Log (continuous)**: `ln(P_t / P_t-1)` – better for adding across time
    

---

###  Metrics by Timeframe

You can extract insights from each period **without** converting to annual terms. Some useful metrics:

####  Rolling Returns

- Rolling 7-day, 30-day, or 90-day returns show **short-term performance momentum**
    
- Example: Fund X had positive returns in 18 of the last 20 rolling 7-day periods → Strong short-term trend
    

####  Average Periodic Return

- Mean return over each week or month  
    Helpful to understand “typical” performance
    

####  Hit Ratio

- % of days/weeks/months with positive return  
    e.g., Stock has 65% positive days → tends to move upward often
    

####  Max/Min Period Return

- Shows **volatility** in that timeframe  
    e.g., biggest weekly drawdown or best monthly rally
    

---

###  Want to Compare Across Timeframes? (Read Robert Carver ch 1 again if this looks confusing in future)

You can still convert between them:

|From → To|Formula|
|---|---|
|Daily → Weekly|`(1 + daily_return)^5 - 1`|
|Daily → Monthly|`(1 + daily_return)^21 - 1`|
|Weekly → Monthly|`(1 + weekly_return)^4 - 1`|
|Monthly → Weekly|`(1 + monthly_return)^(1/4) - 1`|

> These let you **translate** returns into different periods without annualizing.

- **Skewness**: Measures asymmetry.
    
    - Positive skew → more frequent small losses, rare big gains.
        
    - Negative skew → more frequent small gains, rare big losses.
        
    
    **Example:**
    
    - Strategy A: Mostly gains, occasional big loss → Negative skew.
        
    - Strategy B: Mostly flat/slightly down, rare big spike → Positive skew.
        
- **Kurtosis**: Measures "fat tails" or extreme events.
    
    - High kurtosis → More outliers (more big shocks).
        
    - Low kurtosis → More “normal” distribution.
        
    
    **Example:**
    
    - Market crash strategy: Rare events dominate → High kurtosis.
        

Together, skewness and kurtosis help evaluate **tail risk** and **return asymmetry**, which average returns and volatility might hide.


# Important Note to Self:

Don’t get too smart and use all return variants as features — more noise than signal; daily modeling’s a grind solo, better for risk monitoring.

so again: 

- Returns **compound**, not add.
    
- Always **annualize** to compare. (This is important)
    
- Use **log returns** in models.
    
- Look at **risk-adjusted** stats like Sharpe, Sortino.
    
- Analyze **daily, weekly, monthly returns** properly using compounding and proper annualization formulas.
    
- **Rolling and path-dependent metrics** give better insight than point-to-point snapshots.
    
- Understand **return shape** via skewness and kurtosis for better risk awareness.

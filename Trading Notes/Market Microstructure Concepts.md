# Market Microstructure: Order Books, Liquidity, Execution, and Quant Trading

## Quick Reference Table

| Concept | Definition | Why It Matters |
|----------|------------|----------------|
| Market Microstructure | Study of how trades actually occur | Determines execution quality |
| Exchange | Venue where orders interact | NASDAQ, NYSE, CME |
| Order Book | Queue of buy/sell orders | Source of liquidity |
| Bid | Highest buy price | Demand |
| Ask (Offer) | Lowest sell price | Supply |
| Spread | Ask − Bid | Transaction cost |
| Mid Price | Average of bid and ask | Fair-value approximation |
| Market Order | Execute immediately | Prioritizes speed |
| Limit Order | Specify execution price | Prioritizes price |
| Liquidity | Ability to trade without moving price | Key execution metric |
| Market Depth | Available size in order book | Liquidity measure |
| Slippage | Difference between expected and actual execution | Trading cost |
| Impact | Price movement caused by your trade | Hidden cost |
| Maker | Adds liquidity | Earns rebates on some venues |
| Taker | Removes liquidity | Pays fees |
| Tick Size | Minimum price increment | Market structure parameter |
| Queue Position | Rank in order book | Determines fill probability |
| Adverse Selection | Getting filled before unfavorable move | Major HFT concern |
| Latency | Time delay in trading system | Critical in HFT |
| VWAP | Volume Weighted Average Price | Execution benchmark |
| TWAP | Time Weighted Average Price | Execution algorithm |
| Participation Rate | Fraction of market volume traded | Execution control |
| Implementation Shortfall | Realized execution cost | Institutional benchmark |

---

# Intuition

Traditional finance often treats markets as if you can instantly buy or sell at a known price.

Real markets do not work this way.

Instead:

Orders interact through an exchange mechanism.

Questions become:

- Who is willing to buy?
- Who is willing to sell?
- At what price?
- How much size is available?
- How quickly can you execute?

Market microstructure studies these questions.

---

# Market Structure Overview

Simplified flow:

$$
\text{Trader}
\rightarrow
\text{Broker}
\rightarrow
\text{Exchange}
\rightarrow
\text{Order Book}
\rightarrow
\text{Execution}
$$

---

# Order Book

The order book contains:

- Buy orders
- Sell orders

waiting for execution.

---

# Example Order Book

| Bid Size | Bid | Ask | Ask Size |
|-----------|------|------|----------|
| 100 | 99.99 | 100.01 | 200 |
| 500 | 99.98 | 100.02 | 100 |
| 300 | 99.97 | 100.03 | 250 |

---

# Best Bid

Highest buy price.

Example:

$$
99.99
$$

---

# Best Ask

Lowest sell price.

Example:

$$
100.01
$$

---

# Spread

Formula:

$$
\text{Spread}
=
\text{Ask}
-
\text{Bid}
$$

Example:

$$
100.01
-
99.99
$$

$$
=
0.02
$$

---

# Mid Price

Approximate fair value.

Formula:

$$
\text{Mid}
=
\frac{\text{Bid}+\text{Ask}}{2}
$$

Example:

$$
\frac{99.99+100.01}{2}
$$

$$
=
100.00
$$

---

# Limit Orders

Specify a maximum buy price or minimum sell price.

Example:

Buy:

$$
100
$$

shares at:

$$
99.95
$$

Execution only occurs if a seller is willing to trade at:

$$
99.95
$$

or lower.

---

# Market Orders

Execute immediately.

Example:

Buy:

$$
1000
$$

shares now.

Order consumes available asks.

---

# Market Order Example

Order Book:

| Ask | Size |
|-------|------|
| 100.01 | 200 |
| 100.02 | 500 |
| 100.03 | 1000 |

Buy:

$$
1000
$$

shares.

Execution:

$$
200
$$

shares at:

$$
100.01
$$

$$
500
$$

shares at:

$$
100.02
$$

$$
300
$$

shares at:

$$
100.03
$$

---

# Slippage

Expected:

$$
100.01
$$

Actual average:

$$
100.022
$$

Slippage:

$$
0.012
$$

per share.

---

# Liquidity

Liquidity means:

Ability to trade without significantly moving price.

---

# Characteristics of Liquid Markets

- Tight spreads
- Large depth
- Frequent trades
- Low impact

Examples:

- SPY
- ES Futures
- EUR/USD

---

# Characteristics of Illiquid Markets

- Wide spreads
- Low depth
- Large impact

Examples:

- Small-cap equities
- Frontier markets

---

# Market Depth

Amount available at each level.

Example:

| Price | Size |
|---------|------|
| 100.01 | 200 |
| 100.02 | 500 |
| 100.03 | 1000 |

Total visible depth:

$$
1700
$$

shares.

---

# Tick Size

Smallest price increment.

Example:

US equities:

$$
\$0.01
$$

Many futures:

$$
0.25
$$

index points.

---

# Price-Time Priority

Most exchanges use:

1. Best price
2. Earliest order

---

# Example

Two traders bid:

$$
100.00
$$

First trader enters:

9:00:00

Second trader enters:

9:00:01

First trader receives fills first.

---

# Queue Position

Where your order sits in line.

Very important in:

- HFT
- Market making

---

# Example

Queue:

$$
5000
$$

shares ahead.

You submit:

$$
100
$$

shares.

Need:

$$
5000
$$

shares to trade before you get filled.

---

# Maker-Taker Model

Many exchanges incentivize liquidity provision.

---

## Maker

Posts resting order.

Adds liquidity.

May receive rebate.

---

## Taker

Consumes resting order.

Removes liquidity.

Pays fee.

---

# Example

Maker rebate:

$$
0.0020
$$

per share.

Taker fee:

$$
0.0030
$$

per share.

These amounts matter enormously in HFT.

---

# Adverse Selection

One of the most important microstructure concepts.

---

# Intuition

You post a buy order.

Someone immediately sells to you.

Question:

Why were they so eager?

Often because they know bad news.

Price may continue lower.

---

# Example

Buy order:

$$
100.00
$$

filled.

Immediately afterward:

$$
99.90
$$

market.

Loss:

$$
0.10
$$

Adverse selection occurred.

---

# Order Flow

Sequence of trades entering market.

Order flow often contains information.

---

# Order Flow Imbalance

Simple metric:

$$
\text{OFI}
=
\text{Buy Volume}
-
\text{Sell Volume}
$$

Large positive values often indicate buying pressure.

---

# Signed Volume

Trade classification:

Buyer initiated:

$$
+1
$$

Seller initiated:

$$
-1
$$

Often used in microstructure research.

---

# Trade Classification

Common methods:

- Lee-Ready
- Tick Rule
- Quote Rule

Used to infer:

Buyer or seller aggressiveness.

---

# Bid-Ask Bounce

Trades alternate between:

Bid

and

Ask.

Creates noise in returns.

Very important at high frequencies.

---

# Market Impact

Large trades move prices.

---

# Temporary Impact

Price moves during execution.

Later reverts.

---

# Permanent Impact

Price moves and remains there.

Often interpreted as information entering market.

---

# Square-Root Impact Model

Widely used approximation:

$$
\text{Impact}
\propto
\sigma
\sqrt{
\frac{Q}{V}
}
$$

where:

- $Q$ = order size
- $V$ = daily volume

---

# Latency

Time delay in system.

Examples:

- Network delay
- Exchange delay
- Software delay

Measured in:

- milliseconds
- microseconds
- nanoseconds

---

# Latency Arbitrage

Faster participants react first.

Common in:

- HFT
- Market making

---

# Co-location

Servers placed physically near exchange.

Purpose:

Reduce latency.

---

# Hidden Liquidity

Orders not fully visible.

Examples:

- Iceberg Orders
- Dark Pools

---

# Iceberg Orders

Only portion displayed.

Example:

Real size:

$$
10,000
$$

Visible:

$$
500
$$

---

# Dark Pools

Private trading venues.

Designed to reduce:

- Market impact
- Information leakage

Common for institutional orders.

---

# Execution Algorithms

Large institutions rarely send one huge order.

Instead:

Break order into pieces.

---

# TWAP

Time Weighted Average Price.

Trade evenly through time.

---

Example:

Buy:

$$
100,000
$$

shares over:

$$
100
$$

minutes.

Execute:

$$
1000
$$

shares every minute.

---

# VWAP

Volume Weighted Average Price.

Match market volume profile.

---

Formula:

$$
VWAP
=
\frac{
\sum P_i V_i
}{
\sum V_i
}
$$

---

# Participation Algorithm

Trade fixed fraction of market volume.

Example:

$$
10\%
$$

participation rate.

If market trades:

$$
50,000
$$

shares,

algorithm trades:

$$
5000
$$

shares.

---

# Arrival Price Algorithm

Attempts to minimize:

Implementation shortfall.

---

# Implementation Shortfall

Execution cost relative to decision price.

Formula:

$$
IS
=
P_{exec}
-
P_{decision}
$$

for a buy order.

---

# Example

Decision:

$$
100.00
$$

Average execution:

$$
100.15
$$

Shortfall:

$$
0.15
$$

per share.

---

# Market Making

Market makers continuously quote:

Bid

and

Ask.

Profit from:

Spread capture.

---

# Example

Quote:

$$
99.99
/
100.01
$$

Buy:

$$
99.99
$$

Sell:

$$
100.01
$$

Spread capture:

$$
0.02
$$

---

# Inventory Risk

Market makers accumulate positions.

Example:

Long:

$$
100,000
$$

shares.

Price falls.

Losses occur.

Inventory management is critical.

---

# Avellaneda-Stoikov Model

Classic market-making model.

Balances:

- Spread capture
- Inventory risk

Widely studied in quantitative trading.

---

# Microprice

Weighted estimate of fair value.

Formula:

$$
\text{Microprice}
=
\frac{
A \cdot B_s
+
B \cdot A_s
}{
A_s+B_s
}
$$

where:

- $A$ = Ask price
- $B$ = Bid price
- $A_s$ = Ask size
- $B_s$ = Bid size

---

# Microprice Intuition

Large bid size:

Microprice shifts upward.

Large ask size:

Microprice shifts downward.

---

# Common Features Used in HFT

## Spread

$$
Ask - Bid
$$

---

## Mid Return

$$
r_t
=
\log(M_t)
-
\log(M_{t-1})
$$

---

## OFI

Order Flow Imbalance.

---

## Queue Imbalance

$$
QI
=
\frac{
BidSize
-
AskSize
}{
BidSize
+
AskSize
}
$$

---

## Trade Intensity

Trades per second.

---

# Common Data Sources

## Equities

- NASDAQ TotalView
- NYSE OpenBook
- SIP Feed

---

## Futures

- CME MDP
- Databento

---

## Crypto

- Binance
- Coinbase
- Deribit

---

# Common Python Libraries

## Data

```python
pandas
numpy
polars
pyarrow
```

---

## Statistics

```python
scipy
statsmodels
```

---

## Machine Learning

```python
lightgbm
xgboost
catboost
```

---

## Optimization

```python
cvxpy
```

---

# Example: Spread

Bid:

$$
99.99
$$

Ask:

$$
100.02
$$

Spread:

$$
100.02 - 99.99
$$

$$
=
0.03
$$

---

# Example: VWAP

Trades:

| Price | Volume |
|---------|---------|
| 100 | 200 |
| 101 | 100 |
| 102 | 300 |

VWAP:

$$
\frac{
100(200)
+
101(100)
+
102(300)
}{
200+100+300
}
$$

$$
=
101.17
$$

---

# Example: Queue Imbalance

Bid Size:

$$
600
$$

Ask Size:

$$
400
$$

$$
QI
=
\frac{600-400}{600+400}
$$

$$
=
0.20
$$

Positive imbalance.

Potentially bullish.

---

# Common Quant Applications

## Market Making

Estimate:

- Fill probability
- Adverse selection
- Inventory risk

---

## Optimal Execution

Minimize:

- Impact
- Slippage
- Information leakage

---

## Short-Term Alpha

Predict:

Next few seconds or minutes.

---

## Transaction Cost Analysis (TCA)

Measure:

- VWAP slippage
- Implementation shortfall
- Impact costs

---

# Common Beginner Mistakes

## Assuming Mid Price Is Tradable

Usually cannot trade exactly at midpoint.

---

## Ignoring Market Impact

Large trades move markets.

---

## Ignoring Queue Position

A limit order does not guarantee execution.

---

## Confusing Volume with Liquidity

High volume does not always imply deep order books.

---

## Ignoring Adverse Selection

Getting filled is not always good.

Sometimes fills indicate informed traders are trading against you.

---

## Optimizing Signals Without Costs

Many high-frequency signals disappear after:

- Spread costs
- Fees
- Impact

---

# Key Takeaways

- Market microstructure studies how orders become trades.
- Order books, liquidity, spreads, and queue dynamics determine execution quality.
- Market orders prioritize speed; limit orders prioritize price.
- Adverse selection and market impact are among the most important hidden trading costs.
- Market makers earn spreads but manage inventory and information risk.
- Institutional execution relies heavily on VWAP, TWAP, participation-rate, and arrival-price algorithms.
- Modern quantitative trading increasingly relies on order-book features such as queue imbalance, OFI, microprice, and trade intensity.
- Many alpha signals that appear profitable at mid prices disappear once realistic microstructure costs are included.
- Understanding market microstructure is essential for execution, HFT, market making, and production quantitative trading systems.
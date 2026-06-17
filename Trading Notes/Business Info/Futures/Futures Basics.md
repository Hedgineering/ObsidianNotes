# Futures Contracts: Market Structure, Pricing, Trading, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Key Formula |
|----------|------------|-------------|
| Futures Contract | Agreement to buy/sell an asset at a future date | — |
| Underlying Asset | Asset referenced by contract | Oil, Gold, ES, Treasury Bonds, etc. |
| Notional Value | Dollar exposure controlled by contract | Price × Multiplier |
| Contract Multiplier | Dollar value per point move | Exchange-defined |
| Initial Margin | Collateral required to open position | Exchange-defined |
| Maintenance Margin | Minimum required collateral | Exchange-defined |
| Mark-to-Market | Daily settlement of PnL | Daily cash adjustment |
| Long Futures | Profit if price rises | Positive delta |
| Short Futures | Profit if price falls | Negative delta |
| Spot Price | Current market price | $S$ |
| Futures Price | Price for future delivery | $F$ |
| Cost of Carry | Financing/storage costs | Included in futures pricing |
| Contango | Futures > Spot | Upward curve |
| Backwardation | Futures < Spot | Downward curve |
| Basis | Difference between spot and futures | $S-F$ |
| Roll | Move position to later contract | Avoid delivery |
| Open Interest | Outstanding contracts | Liquidity indicator |
| Tick Size | Smallest price increment | Exchange-defined |
| Tick Value | Dollar value of one tick | Tick Size × Multiplier |

---

# Intuition

A futures contract is a standardized agreement traded on an exchange.

You agree today on a price for a transaction that will occur later.

Example:

An airline wants fuel in six months.

They worry oil prices may rise.

They buy oil futures today to lock in a price.

---

# Why Futures Exist

Futures serve two primary purposes:

## Hedging

Reduce risk.

Examples:

- Airline hedges jet fuel
- Farmer hedges crop prices
- Pension fund hedges interest rates

---

## Speculation

Take directional views.

Examples:

- Oil prices will rise
- S&P 500 will fall
- Interest rates will decline

---

# Futures vs Stocks

| Stocks | Futures |
|----------|----------|
| Ownership | Contract |
| Requires full capital | Uses margin |
| No expiration | Expiration date |
| Dividends possible | No dividends |
| Direct asset ownership | Exposure only |

---

# Futures vs Forwards

Both lock in future prices.

---

## Forward Contract

Private agreement.

Characteristics:

- OTC
- Customized
- Counterparty risk

---

## Futures Contract

Exchange traded.

Characteristics:

- Standardized
- Highly liquid
- Clearinghouse guarantees performance

---

# Standard Futures Contract Components

Example:

E-mini S&P 500 (ES)

| Field | Value |
|---------|---------|
| Underlying | S&P 500 Index |
| Multiplier | \$50 per point |
| Exchange | CME |
| Expiration | Quarterly |
| Tick Size | 0.25 |
| Tick Value | \$12.50 |

---

# Contract Multiplier

The multiplier determines exposure.

Example:

ES futures trading at:

$$
6000
$$

Multiplier:

$$
50
$$

Notional value:

$$
6000 \times 50
$$

$$
=
300,000
$$

One contract controls:

$$
\$300,000
$$

of equity exposure.

---

# Long Futures Position

You profit if prices rise.

Example:

Buy ES at:

$$
6000
$$

Sell later at:

$$
6050
$$

Price move:

$$
50
$$

PnL:

$$
50 \times 50
$$

$$
=
\$2500
$$

---

# Short Futures Position

You profit if prices fall.

Example:

Sell at:

$$
6000
$$

Buy back at:

$$
5950
$$

Price move:

$$
50
$$

Profit:

$$
50 \times 50
$$

$$
=
\$2500
$$

---

# Margin

Unlike stocks, futures do not require full payment.

Only collateral is posted.

---

## Initial Margin

Required to open position.

Example:

Notional:

$$
300,000
$$

Margin:

$$
15,000
$$

Leverage:

$$
\frac{300,000}{15,000}
=
20
\times
$$

---

## Maintenance Margin

Minimum balance required.

If account falls below:

Margin call occurs.

Additional funds must be deposited.

---

# Mark-to-Market

One of the most important futures concepts.

Every day:

- Positions are repriced
- Gains credited
- Losses debited

Cash moves daily.

---

# Example

Day 1:

Long ES at:

$$
6000
$$

Day 2 close:

$$
6010
$$

Gain:

$$
10 \times 50
$$

$$
=
500
$$

Broker deposits:

$$
\$500
$$

into account.

---

# Physical vs Cash Settlement

## Physical Delivery

Actual asset delivered.

Examples:

- Corn
- Wheat
- Crude Oil

---

## Cash Settlement

No delivery.

Only cash exchanged.

Examples:

- S&P 500 Futures
- VIX Futures

---

# Futures Pricing

A futures contract should reflect:

- Spot price
- Financing costs
- Storage costs
- Income received

---

# Cost-of-Carry Model

Basic formula:

$$
F
=
S(1+r)^T
$$

where:

- $S$ = Spot price
- $r$ = interest rate
- $T$ = time to maturity

---

# Intuition

Suppose:

Spot Gold:

$$
2000
$$

Interest rate:

$$
5\%
$$

One year maturity.

Fair futures value:

$$
F
=
2000(1.05)
$$

$$
=
2100
$$

---

# More Complete Formula

For commodities:

$$
F
=
S e^{(r+c-y)T}
$$

where:

- $r$ = financing cost
- $c$ = storage cost
- $y$ = convenience yield

---

# Convenience Yield

Benefit of physically holding commodity.

Examples:

- Immediate oil inventory
- Grain inventory during shortages

Higher convenience yield lowers futures prices.

---

# Basis

Difference between spot and futures.

Formula:

$$
\text{Basis}
=
S-F
$$

---

# Example

Spot:

$$
100
$$

Futures:

$$
105
$$

Basis:

$$
100-105
$$

$$
=
-5
$$

---

# Convergence at Expiration

As expiration approaches:

$$
F
\rightarrow
S
$$

Otherwise arbitrage would exist.

This convergence is one of the most important principles in futures markets.

---

# Contango

Futures prices increase with maturity.

Example:

| Contract | Price |
|------------|---------|
| Front Month | 100 |
| 3 Month | 104 |
| 6 Month | 108 |

Typically occurs when:

- Interest rates high
- Storage costs high

---

# Backwardation

Futures prices decrease with maturity.

Example:

| Contract | Price |
|------------|---------|
| Front Month | 100 |
| 3 Month | 97 |
| 6 Month | 95 |

Often occurs when:

- Commodity shortages exist
- Convenience yield is high

---

# Open Interest

Number of active contracts.

High open interest often implies:

- Better liquidity
- Lower transaction costs

---

# Volume vs Open Interest

## Volume

Contracts traded today.

---

## Open Interest

Contracts still outstanding.

These are not the same quantity.

---

# Rolling Futures

Contracts expire.

Most investors do not want delivery.

They "roll" positions.

Example:

$$
ESU5
\rightarrow
ESZ5
$$

Sell old contract.

Buy next contract.

---

# Roll Yield

Gain/loss caused by rolling.

Very important in:

- Commodity funds
- CTA strategies
- Managed futures

---

# Major Futures Markets

## Equity Index Futures

Examples:

- ES (S&P 500)
- NQ (Nasdaq)
- RTY (Russell 2000)

---

## Interest Rate Futures

Examples:

- Treasury Futures
- SOFR Futures

---

## Commodity Futures

Examples:

- CL (Crude Oil)
- GC (Gold)
- SI (Silver)
- NG (Natural Gas)

---

## Currency Futures

Examples:

- EUR/USD
- JPY/USD

---

# Treasury Futures

One of the largest futures markets.

Examples:

- 2-Year Treasury Futures
- 5-Year Treasury Futures
- 10-Year Treasury Futures
- Ultra Bond Futures

Used heavily by:

- Macro funds
- Banks
- Hedge funds

---

# Futures in Quant Trading

Many systematic funds primarily trade futures.

Reasons:

- Highly liquid
- Low transaction costs
- Global exposure
- Leverage available
- Easier short exposure

---

# Typical Futures Data Fields

| Field | Description |
|---------|-------------|
| Symbol | ES |
| Contract | ESU5 |
| Open | Session Open |
| High | Session High |
| Low | Session Low |
| Close | Settlement |
| Volume | Contracts traded |
| Open Interest | Active contracts |
| Bid | Best bid |
| Ask | Best ask |

---

# Continuous Futures

Backtests require long histories.

Problem:

Contracts expire.

Solution:

Create continuous series.

Methods:

- Back-adjusted
- Ratio-adjusted
- Panama adjustment

---

# Back-Adjusted Futures

Most common method.

Adjust historical prices to remove artificial roll gaps.

Preserves returns.

Not necessarily absolute prices.

---

# Common Futures Factors

## Time-Series Momentum

Most important CTA factor.

Signal:

$$
\operatorname{sign}
(
r_{12m}
)
$$

Go long winners.

Short losers.

---

## Carry

Uses futures curve shape.

Related to:

- Contango
- Backwardation

---

## Trend Following

Moving average systems.

Example:

$$
MA_{50}
>
MA_{200}
$$

Long position.

---

## Volatility Scaling

Position size:

$$
w
\propto
\frac{1}{\sigma}
$$

Higher volatility:

Smaller position.

---

# Futures Portfolio Construction

Very common approach:

Signal:

$$
s_i
$$

Risk:

$$
\sigma_i
$$

Position:

$$
w_i
=
\frac{s_i}{\sigma_i}
$$

Often followed by:

- Vol targeting
- Correlation adjustment
- Risk parity

---

# Common Optimizers

## Mean-Variance

$$
\max
\left(
\mu^T w
-
\lambda w^T \Sigma w
\right)
$$

---

## Risk Parity

Equalize risk contribution.

Very popular among futures funds.

---

## Volatility Targeting

Scale portfolio:

$$
w
\leftarrow
w
\times
\frac{\sigma_{target}}
{\sigma_{portfolio}}
$$

---

# FIX Protocol in Futures

Workflow:

$$
\text{Signal}
\rightarrow
\text{OMS}
\rightarrow
\text{FIX}
\rightarrow
\text{Broker}
\rightarrow
\text{Exchange}
$$

Common exchanges:

- CME
- ICE
- Eurex
- SGX

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

## Market Data

```python
ib_insync
databento
polygon
```

---

## Optimization

```python
cvxpy
osqp
gurobipy
mosek
```

---

## Statistics

```python
statsmodels
scipy
arch
```

---

## Backtesting

```python
vectorbt
backtrader
zipline
```

---

# Example: Futures PnL

Suppose:

Long:

$$
2
$$

ES contracts.

Entry:

$$
6000
$$

Exit:

$$
6025
$$

Multiplier:

$$
50
$$

PnL:

$$
2
\times
25
\times
50
$$

$$
=
2500
$$

Answer:

$$
\$2500
$$

---

# Example: Futures Fair Value

Spot:

$$
100
$$

Rate:

$$
5\%
$$

One year maturity.

Fair futures:

$$
100(1.05)
$$

$$
=
105
$$

---

# Python Example: Futures PnL

```python
contracts = 2
entry = 6000
exit = 6025
multiplier = 50

pnl = contracts * (exit - entry) * multiplier

print(pnl)
```

---

# Python Example: Continuous Return Series

```python
import pandas as pd

returns = prices.pct_change()

cum_returns = (1 + returns).cumprod()
```

---

# Common Beginner Mistakes

## Confusing Notional with Margin

A contract controlling:

$$
\$300,000
$$

may require only:

$$
\$15,000
$$

in margin.

---

## Ignoring Rolls

Expired contracts disappear.

Backtests must use rolling logic.

---

## Using Raw Continuous Prices

Back-adjustments affect absolute price levels.

Returns are usually more meaningful.

---

## Ignoring Contract Multipliers

A one-point move is not always one dollar.

Multiplier determines actual PnL.

---

## Forgetting Daily Mark-to-Market

Futures gains and losses settle every day.

This differs from forwards.

---

# Key Takeaways

- Futures are standardized exchange-traded contracts.
- They provide leveraged exposure using margin.
- Daily mark-to-market is fundamental to futures markets.
- Contract multipliers determine actual dollar exposure.
- Futures pricing is driven by spot price and cost of carry.
- Contango and backwardation shape futures curves.
- Contracts must be rolled before expiration.
- Most quantitative macro and CTA funds trade futures rather than individual securities.
- Continuous futures construction is a major data engineering challenge.
- Trend following, carry, volatility targeting, and risk parity are among the most common systematic futures strategies.
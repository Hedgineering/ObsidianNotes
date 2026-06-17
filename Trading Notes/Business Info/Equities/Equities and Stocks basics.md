# Equities: Market Structure, Data, Corporate Actions, Risk Models, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Common Identifiers / Tools |
|----------|------------|----------------------------|
| Equity | Ownership stake in a company | Ticker, CUSIP, ISIN |
| Common Stock | Standard voting ownership shares | AAPL, MSFT, etc. |
| Preferred Stock | Equity with dividend priority | Often no voting rights |
| Market Capitalization | Total company value | Price × Shares Outstanding |
| Float | Shares available for public trading | Used in index construction |
| Corporate Action | Event affecting securities | Splits, dividends, mergers |
| CUSIP | North American security identifier | 9-character code |
| ISIN | Global security identifier | 12-character code |
| SEDOL | UK security identifier | 7-character code |
| FIX Protocol | Standard electronic trading protocol | Orders, executions, cancels |
| Short Locate | Confirmation shares can be borrowed | Required before shorting |
| Barra Factors | Risk model factors | Market, Value, Momentum |
| Alpha | Expected excess return signal | Quant prediction |
| Beta | Market sensitivity | CAPM exposure |
| Factor Exposure | Security sensitivity to factor | Risk model input |
| Portfolio Optimizer | Chooses portfolio weights | cvxpy, Gurobi |
| OMS | Order Management System | Portfolio → Orders |
| EMS | Execution Management System | Orders → Market |
| TCA | Transaction Cost Analysis | Measure execution quality |

---

# Intuition

An equity represents ownership in a company.

If a company has:

$$
1,000,000
$$

shares outstanding and you own

$$
10,000
$$

shares, you own

$$
\frac{10,000}{1,000,000}
=
1\%
$$

of the company.

Equities differ from bonds because:

| Bonds | Equities |
|---------|---------|
| Debt | Ownership |
| Fixed payments | Variable returns |
| Higher priority in bankruptcy | Lowest priority |
| Usually lower risk | Usually higher risk |

---

# Core Equity Terminology

## Shares Outstanding

Total number of issued shares.

Example:

$$
100,000,000
$$

shares.

---

## Market Capitalization

Total market value of a company.

Formula:

$$
\text{Market Cap}
=
\text{Price}
\times
\text{Shares Outstanding}
$$

Example:

$$
\$200
\times
100,000,000
=
\$20B
$$

---

## Float

Shares actually available for trading.

Excludes:

- Founder holdings
- Locked-up shares
- Government ownership

Many indexes use float-adjusted weights.

---

## Liquidity

Ease of buying or selling shares.

Measured using:

- Average Daily Volume (ADV)
- Bid-Ask Spread
- Market Depth

---

# Security Identifiers

One of the most important operational topics in quant firms.

---

## Ticker Symbol

Human-readable identifier.

Examples:

- AAPL
- MSFT
- NVDA

Problem:

Tickers can change.

Example:

Facebook:

$$
FB \rightarrow META
$$

---

## CUSIP

Committee on Uniform Securities Identification Procedures.

Used primarily in North America.

Example:

```
037833100
```

(AAPL common stock)

Characteristics:

- 9 characters
- Security-specific
- Frequently used by brokers and custodians

---

## ISIN

International Securities Identification Number.

Example:

```
US0378331005
```

Characteristics:

- Global identifier
- Contains country code

Very common in institutional systems.

---

## SEDOL

UK-focused security identifier.

Used frequently in global equity datasets.

---

# Security Master

A critical database at every quant firm.

Maps:

| Field | Example |
|---------|---------|
| Ticker | AAPL |
| CUSIP | 037833100 |
| ISIN | US0378331005 |
| Exchange | NASDAQ |
| Currency | USD |

Security master systems prevent identifier mismatches.

---

# Corporate Actions

## Intuition

Corporate actions change the economic characteristics of a security.

Historical data must usually be adjusted.

Failing to process corporate actions correctly can completely invalidate backtests.

---

# Stock Split

Example:

2-for-1 split.

Before:

$$
100
$$

shares at

$$
\$200
$$

each.

After:

$$
200
$$

shares at

$$
\$100
$$

each.

Total value unchanged.

---

## Split Adjustment

Historical prices are adjusted.

Example:

| Date | Raw Price |
|--------|---------|
| Yesterday | 200 |
| Today | 100 |

Naively:

$$
-50\%
$$

return.

Actual return:

$$
0\%
$$

after split adjustment.

---

# Cash Dividend

Company distributes cash.

Example:

$$
\$1
$$

per share dividend.

If stock closes at:

$$
\$100
$$

it may open near:

$$
\$99
$$

on ex-dividend date.

---

# Special Dividend

Large one-time dividend.

Can create substantial price adjustments.

---

# Merger

Example:

Company A acquires Company B.

Security master updates:

- Ticker mapping
- Identifier mapping
- Historical lineage

---

# Spin-Off

Part of company becomes new company.

Example:

```
Parent Company
      ↓
Parent + New Company
```

Backtesting systems must allocate value correctly.

---

# Delisting

Company removed from exchange.

One of the largest sources of survivorship bias.

Bad datasets often exclude delisted companies.

This artificially inflates backtest performance.

---

# Adjusted Prices

Most quant research uses:

$$
\text{Adjusted Close}
$$

rather than raw close.

Adjustments account for:

- Splits
- Dividends
- Corporate actions

Common data vendors:

- CRSP
- Refinitiv
- Bloomberg
- FactSet

---

# Short Selling

## Intuition

Normally:

$$
\text{Buy}
\rightarrow
\text{Sell Later}
$$

Short selling:

$$
\text{Borrow}
\rightarrow
\text{Sell}
\rightarrow
\text{Buy Back}
$$

---

# Example

Borrow:

$$
100
$$

shares at

$$
\$100
$$

Sell:

$$
\$10,000
$$

Later buy back at:

$$
\$90
$$

Cost:

$$
\$9,000
$$

Profit:

$$
\$1,000
$$

---

# Short Locate

Regulators require confirmation that shares can be borrowed.

This confirmation is called a locate.

Workflow:

$$
\text{Trader}
\rightarrow
\text{Prime Broker}
\rightarrow
\text{Locate Approval}
$$

Without a valid locate, many short sales cannot proceed.

---

# Borrow Fees

Hard-to-borrow securities incur borrow costs.

Annualized borrow rate:

$$
20\%
$$

or more is possible.

Important in short alpha strategies.

---

# FIX Protocol

Financial Information eXchange Protocol.

Industry standard messaging protocol.

Used for:

- Order submission
- Cancel requests
- Execution reports
- Market data

---

## Simplified FIX Flow

Portfolio generates order:

$$
\text{Buy 10,000 AAPL}
$$

OMS sends FIX message.

Broker acknowledges.

Exchange executes.

Execution reports returned via FIX.

---

## Example FIX Message

```text
35=D
55=AAPL
54=1
38=10000
40=2
44=200.50
```

Meaning:

| Tag | Meaning |
|-------|---------|
| 35 | New Order |
| 55 | Symbol |
| 54 | Buy/Sell |
| 38 | Quantity |
| 40 | Order Type |
| 44 | Limit Price |

Actual messages are much longer.

---

# Quant Research Pipeline

Typical workflow:

$$
\text{Raw Data}
\rightarrow
\text{Clean Data}
\rightarrow
\text{Features}
\rightarrow
\text{Alpha Signals}
\rightarrow
\text{Risk Model}
\rightarrow
\text{Optimization}
\rightarrow
\text{Orders}
$$

---

# Alpha Factors

Alpha factors attempt to predict returns.

Examples:

---

## Momentum

Recent winners continue outperforming.

Example:

$$
r_{12m}
-
r_{1m}
$$

---

## Value

Cheap companies outperform expensive companies.

Metrics:

$$
\frac{\text{Book Value}}{\text{Price}}
$$

$$
\frac{\text{Earnings}}{\text{Price}}
$$

---

## Quality

Examples:

- ROE
- Profitability
- Earnings stability

---

## Size

Typically:

$$
-\log(\text{Market Cap})
$$

Small-cap effect.

---

## Volatility

Historical realized volatility.

Sometimes low-volatility stocks outperform.

---

# Barra Risk Models

Widely used institutional factor models.

Goal:

Separate:

$$
\text{Alpha}
$$

from

$$
\text{Risk}
$$

---

# Common Barra-Style Factors

## Market

Overall equity market exposure.

---

## Size

Company size.

Typically:

$$
\log(\text{Market Cap})
$$

---

## Value

Book-to-price.

---

## Momentum

Past returns.

---

## Volatility

Historical variability.

---

## Liquidity

Trading volume and turnover.

---

## Growth

Revenue and earnings growth.

---

# Factor Model

Return decomposition:

$$
R_i
=
\alpha_i
+
\beta_i^T f
+
\epsilon_i
$$

where

- $\alpha_i$ = stock-specific alpha
- $f$ = factor returns
- $\beta_i$ = factor exposures
- $\epsilon_i$ = idiosyncratic return

---

# Factor Neutralization

Many firms remove unwanted exposures.

Example:

Neutralize market cap effect.

Regression:

$$
\text{Signal}
=
\beta
\cdot
\log(\text{Market Cap})
+
\epsilon
$$

Residual:

$$
\epsilon
$$

becomes new signal.

---

# Common Signal Transformations

## Winsorization

Clip outliers.

Example:

1st and 99th percentile.

---

## Z-Score Normalization

$$
z
=
\frac{x-\mu}{\sigma}
$$

Very common before optimization.

---

## Cross-Sectional Ranking

Convert values to ranks.

Example:

Top stock:

$$
1.0
$$

Bottom stock:

$$
0.0
$$

---

## Sector Neutralization

Remove industry effects.

Example:

Compare banks to banks.

Not banks to software companies.

---

# Portfolio Optimization

After alpha generation:

Need portfolio weights.

---

## Mean-Variance Optimization

Harry Markowitz framework.

Objective:

$$
\max
\left(
\mu^T w
-
\lambda
w^T \Sigma w
\right)
$$

where

- $\mu$ = expected returns
- $\Sigma$ = covariance matrix
- $w$ = portfolio weights

---

## Risk-Parity

Allocate risk equally.

Instead of allocating capital equally.

---

## Factor-Constrained Optimization

Typical constraints:

$$
\text{Market Beta}
=
0
$$

$$
\text{Sector Exposure}
=
0
$$

$$
\sum w_i = 1
$$

---

# Common Optimizers

## cvxpy

Most common Python convex optimization framework.

```python
import cvxpy as cp
```

Used for:

- Mean-variance
- Turnover constraints
- Factor constraints

---

## OSQP

Very common quadratic-program solver.

Fast for portfolio optimization.

---

## ECOS

Convex optimization solver.

---

## SCS

Large-scale convex optimization.

---

## Gurobi

Industry gold standard.

Commercial.

Very fast.

Common at hedge funds.

---

## MOSEK

Popular institutional optimizer.

Excellent numerical stability.

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
scikit-learn
xgboost
lightgbm
catboost
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

## Backtesting

```python
vectorbt
zipline
backtrader
```

---

## Market Data

```python
yfinance
polygon
alpaca
ib_insync
```

---

# Common Quant Production Architecture

Research:

$$
\text{Data Lake}
\rightarrow
\text{Feature Store}
\rightarrow
\text{Alpha Research}
$$

Production:

$$
\text{Predictions}
\rightarrow
\text{Portfolio Construction}
\rightarrow
\text{OMS}
\rightarrow
\text{FIX}
\rightarrow
\text{Broker}
\rightarrow
\text{Exchange}
$$

Monitoring:

$$
\text{Executions}
\rightarrow
\text{TCA}
\rightarrow
\text{Performance Attribution}
$$

---

# Common Beginner Mistakes

## Ignoring Corporate Actions

Can create fake returns.

Always use adjusted data when appropriate.

---

## Ignoring Delisted Stocks

Creates survivorship bias.

---

## Using Tickers as Permanent IDs

Tickers change.

Use:

- CUSIP
- ISIN
- Internal security ID

---

## Optimizing Raw Signals

Most firms first:

- Winsorize
- Normalize
- Neutralize

before optimization.

---

## Ignoring Transaction Costs

Backtests often fail in production because:

$$
\text{Alpha}
<
\text{Trading Costs}
$$

---

# Key Takeaways

- Equities represent ownership in companies.
- Security master systems map identifiers such as ticker, CUSIP, and ISIN.
- Corporate actions are critical for maintaining correct historical datasets.
- Short selling requires borrow availability and often short locates.
- FIX is the dominant protocol for electronic order routing.
- Modern quant firms build alpha signals, neutralize exposures, estimate risk, and optimize portfolios before execution.
- Barra-style factor models are widely used for risk decomposition and exposure control.
- Most production equity systems consist of research pipelines, portfolio construction engines, OMS/EMS infrastructure, FIX connectivity, and execution monitoring.
- Proper handling of identifiers, corporate actions, risk models, and transaction costs is just as important as generating alpha.
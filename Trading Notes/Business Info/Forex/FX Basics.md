# Foreign Exchange (Forex / FX): Market Structure, Pricing, Trading, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Example |
|----------|------------|----------|
| Forex (FX) | Market for exchanging currencies | EUR/USD |
| Currency Pair | Exchange rate between two currencies | USD/JPY |
| Base Currency | First currency in pair | EUR in EUR/USD |
| Quote Currency | Second currency in pair | USD in EUR/USD |
| Spot FX | Immediate currency exchange | T+2 settlement |
| Forward FX | Future currency exchange agreement | EUR/USD 3M Forward |
| FX Swap | Simultaneous spot and forward transaction | Common funding instrument |
| Pip | Smallest standard FX movement | 0.0001 |
| Notional | Contract size | \$10M EUR/USD |
| Carry Trade | Borrow low-rate currency, buy high-rate currency | JPY → AUD |
| Cross Currency Pair | Pair without USD | EUR/GBP |
| Bid | Dealer buys base currency | Lower price |
| Ask | Dealer sells base currency | Higher price |
| Spread | Ask − Bid | Trading cost |
| Fixing | Official benchmark rate | WM/Reuters Fix |
| NDF | Non-Deliverable Forward | Emerging markets |
| CIP | Covered Interest Parity | Forward pricing relationship |
| UIP | Uncovered Interest Parity | Expected return relationship |

---

# Intuition

Forex is the largest financial market in the world.

Unlike stocks:

- There is no single exchange.
- Trading occurs globally.
- Markets operate nearly 24 hours per day.

Every FX trade involves:

Buying one currency and selling another.

You can never buy "EUR."

You buy:

$$
EUR/USD
$$

meaning:

- Long EUR
- Short USD

simultaneously.

---

# Why FX Exists

FX transactions occur because:

- International trade
- Tourism
- Investing abroad
- Central bank operations
- Speculation
- Hedging

Example:

A U.S. company purchasing German equipment needs Euros.

They exchange:

$$
USD \rightarrow EUR
$$

through FX markets.

---

# Currency Pair Basics

## Example

EUR/USD:

$$
1.1500
$$

means:

$$
1 \text{ EUR}
=
1.15 \text{ USD}
$$

---

# Base Currency

First currency.

Example:

EUR/USD

Base:

$$
EUR
$$

---

# Quote Currency

Second currency.

Example:

EUR/USD

Quote:

$$
USD
$$

---

# Interpretation

If EUR/USD rises:

$$
1.15
\rightarrow
1.20
$$

then:

Euro strengthened.

Dollar weakened.

---

# Major Currency Pairs

Most liquid currencies.

| Pair | Nickname |
|--------|-----------|
| EUR/USD | Euro |
| USD/JPY | Dollar-Yen |
| GBP/USD | Cable |
| USD/CHF | Swissy |
| AUD/USD | Aussie |
| NZD/USD | Kiwi |
| USD/CAD | Loonie |

---

# Cross Currency Pairs

Pairs without USD.

Examples:

- EUR/GBP
- EUR/JPY
- GBP/JPY
- AUD/NZD

Crosses are often derived from major pairs.

---

# Spot FX

Standard FX transaction.

Settlement usually occurs:

$$
T+2
$$

(two business days later).

Exception:

USD/CAD often settles:

$$
T+1
$$

---

# Bid and Ask

Example:

$$
EUR/USD
=
1.1500 / 1.1502
$$

Meaning:

| Price | Meaning |
|---------|---------|
| 1.1500 | Dealer buys EUR |
| 1.1502 | Dealer sells EUR |

---

# Spread

Trading cost.

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
1.1502 - 1.1500
$$

$$
=
0.0002
$$

---

# Pips

Standard FX movement unit.

Most pairs:

$$
1 \text{ pip}
=
0.0001
$$

---

Example:

$$
1.1500
\rightarrow
1.1505
$$

Move:

$$
5
\text{ pips}
$$

---

# Pip Value

Suppose:

$$
1,000,000
$$

EUR position.

Move:

$$
1
\text{ pip}
=
0.0001
$$

Profit/Loss:

$$
1,000,000
\times
0.0001
$$

$$
=
100
\text{ USD}
$$

per pip.

---

# FX Market Structure

Unlike equities:

There is no central exchange.

Participants include:

- Banks
- Hedge Funds
- Corporations
- Central Banks
- Brokers

---

# Tier 1 Banks

Examples:

- JPMorgan
- Citi
- Goldman Sachs
- Deutsche Bank
- UBS

These institutions provide liquidity.

---

# Electronic Trading Venues

Examples:

- EBS
- Reuters Matching
- Currenex
- FXall
- Hotspot

Used heavily by institutions.

---

# Spot FX Trading

Suppose:

Buy:

$$
10M
\text{ EUR/USD}
$$

at:

$$
1.1500
$$

Notional:

$$
10,000,000
\text{ EUR}
$$

Equivalent USD:

$$
11,500,000
$$

---

# FX Forwards

Agreement today to exchange currencies later.

Used for:

- Hedging
- Funding
- Carry strategies

---

# Example

A company needs:

$$
10M
\text{ EUR}
$$

in 6 months.

Instead of accepting exchange-rate uncertainty:

They lock in a forward rate today.

---

# FX Forward Pricing

Unlike stocks, currencies have interest rates on both sides.

---

# Covered Interest Parity (CIP)

One of the most important relationships in FX.

$$
F
=
S
\frac{(1+r_d)^T}
{(1+r_f)^T}
$$

where:

- $F$ = Forward rate
- $S$ = Spot rate
- $r_d$ = Domestic rate
- $r_f$ = Foreign rate

---

# Intuition

Suppose:

USD interest:

$$
5\%
$$

EUR interest:

$$
2\%
$$

USD earns more interest.

Forward rates must adjust to eliminate arbitrage.

---

# Example

Spot:

$$
EUR/USD = 1.10
$$

USD rate:

$$
5\%
$$

EUR rate:

$$
2\%
$$

One-year forward:

$$
F
=
1.10
\frac{1.05}{1.02}
$$

$$
=
1.1324
$$

---

# FX Swaps

Most traded FX product.

Structure:

1. Spot transaction
2. Forward transaction

simultaneously.

---

Example:

Today:

$$
USD \rightarrow EUR
$$

Three months later:

$$
EUR \rightarrow USD
$$

---

# Carry Trade

One of the most famous FX strategies.

---

# Intuition

Borrow:

Low-rate currency.

Invest in:

High-rate currency.

---

Example:

Borrow:

$$
JPY
$$

at:

$$
0.5\%
$$

Invest:

$$
AUD
$$

at:

$$
4.5\%
$$

Spread:

$$
4\%
$$

Potential profit if exchange rates remain stable.

---

# Risks of Carry Trades

Carry trades can suffer huge losses during:

- Crises
- Risk-off events
- Central bank shocks

These trades often appear profitable until large market dislocations occur.

---

# Central Banks

FX markets are heavily influenced by:

- Federal Reserve
- ECB
- BOJ
- BOE
- SNB

Interest-rate expectations are major FX drivers.

---

# Common FX Drivers

## Interest Rates

Higher rates often attract capital.

---

## Inflation

Affects monetary policy.

---

## Economic Growth

Strong economies often support currencies.

---

## Trade Balance

Exports vs imports.

---

## Risk Sentiment

Global investors moving into:

- Risk assets
- Safe havens

---

# Safe Haven Currencies

Common examples:

- USD
- CHF
- JPY

Often appreciate during crises.

---

# Emerging Market FX

Examples:

- BRL
- MXN
- ZAR
- INR

Higher volatility.

Higher carry.

Greater political risk.

---

# Non-Deliverable Forwards (NDFs)

Some currencies have capital controls.

Examples:

- INR
- KRW
- TWD

Instead of delivering currency:

Only cash settlement occurs.

---

# Quantitative FX Trading

Many FX strategies are systematic.

---

# Momentum

Currencies that have appreciated continue appreciating.

Example:

$$
r_{12m}
-
r_{1m}
$$

---

# Carry

Use interest-rate differentials.

Higher yielding currencies:

Long.

Lower yielding currencies:

Short.

---

# Value

Estimate fair value.

Compare:

Actual exchange rate

vs

Purchasing Power Parity.

---

# Purchasing Power Parity (PPP)

Long-term valuation framework.

Example:

If:

- Big Mac costs \$5 in U.S.
- Equivalent costs \$10 abroad

Exchange rate may be misaligned.

PPP is not useful for short-term trading but important for long-horizon valuation.

---

# Volatility

FX volatility is often modeled using:

$$
\sigma
=
\sqrt{\text{Var}(r)}
$$

Common models:

- GARCH
- EWMA
- Realized Volatility

---

# FX Risk Factors

Typical institutional factors:

- Carry
- Momentum
- Value
- Volatility
- Dollar Factor

---

# Currency Factor Model

Example:

$$
R_i
=
\beta_1
\text{Carry}
+
\beta_2
\text{Momentum}
+
\beta_3
\text{Value}
+
\epsilon
$$

---

# Portfolio Construction

Common framework:

Signal:

$$
s_i
$$

Volatility:

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

- Risk parity
- Volatility targeting
- Correlation adjustment

---

# FIX Protocol in FX

Most institutional FX orders use FIX.

Workflow:

$$
\text{Alpha}
\rightarrow
\text{OMS}
\rightarrow
\text{FIX}
\rightarrow
\text{Liquidity Provider}
\rightarrow
\text{Execution}
$$

---

# Common FIX Messages

Examples:

- New Order
- Cancel Request
- Execution Report
- Market Data Request

Very similar to equities and futures.

---

# FX Data Fields

| Field | Description |
|---------|------------|
| Bid | Best bid |
| Ask | Best ask |
| Mid | Average of bid/ask |
| Spread | Ask − Bid |
| Volume | Trading activity |
| Forward Points | Spot-forward adjustment |
| Swap Points | Forward premium/discount |

---

# Common Quant Technologies

## Data

```python
pandas
numpy
polars
pyarrow
```

---

## Time Series

```python
statsmodels
arch
scipy
```

---

## Machine Learning

```python
scikit-learn
xgboost
lightgbm
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

# Example: Spot FX PnL

Long:

$$
1M
\text{ EUR/USD}
$$

Entry:

$$
1.1000
$$

Exit:

$$
1.1050
$$

Move:

$$
0.0050
=
50
\text{ pips}
$$

Profit:

$$
1,000,000
\times
0.0050
$$

$$
=
5,000
\text{ USD}
$$

---

# Example: Forward Pricing

Spot:

$$
1.10
$$

USD rate:

$$
5\%
$$

EUR rate:

$$
2\%
$$

One-year maturity.

Forward:

$$
F
=
1.10
\frac{1.05}
{1.02}
$$

$$
=
1.1324
$$

---

# Python Example: FX Return

```python
import pandas as pd

returns = fx_prices.pct_change()

annualized_vol = returns.std() * (252 ** 0.5)
```

---

# Python Example: Carry Signal

```python
signal = interest_rate_country_a - interest_rate_country_b
```

---

# Common Beginner Mistakes

## Misreading Currency Pairs

EUR/USD rising means:

- EUR stronger
- USD weaker

not necessarily both becoming stronger.

---

## Ignoring Interest Rates

FX is heavily driven by rate differentials.

Often more important than economic growth.

---

## Assuming Spot and Forward Are Equal

Interest-rate differences create forward premiums and discounts.

---

## Ignoring Transaction Costs

Many FX alpha signals disappear after spreads and execution costs.

---

## Using Raw Prices Across Regimes

Currencies experience:

- Central bank interventions
- Peg changes
- Structural shifts

These must be accounted for.

---

# Key Takeaways

- FX is the largest and most liquid financial market in the world.
- Every FX trade simultaneously buys one currency and sells another.
- Spot, forwards, swaps, and NDFs are the primary FX instruments.
- Covered Interest Parity is one of the most important pricing relationships in finance.
- Carry, momentum, and value are the dominant systematic FX factors.
- Interest-rate differentials are often the primary driver of exchange rates.
- FX trading infrastructure is heavily FIX-based and dealer-driven.
- Most institutional FX strategies focus on risk-adjusted portfolio construction rather than directional bets on individual currencies.
- Quant FX research heavily overlaps with macro, futures, and fixed-income trading because all are fundamentally driven by global interest rates and economic expectations.
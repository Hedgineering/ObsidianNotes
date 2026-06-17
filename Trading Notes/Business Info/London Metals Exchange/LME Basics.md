# London Metal Exchange (LME) Contracts: Market Structure, Pricing, Warehousing, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Example |
|----------|------------|----------|
| LME | London Metal Exchange | World's largest industrial metals exchange |
| Base Metals | Core industrial metals traded on LME | Copper, Aluminum, Zinc |
| Prompt Date | Specific settlement date of an LME contract | 3-Month Copper |
| Cash Contract | Nearest settlement date contract | Spot-equivalent |
| Tom-Next | Tomorrow-to-next-day spread | Funding spread |
| Carry Trade | Long one prompt date, short another | Calendar spread |
| Warrant | Title to metal stored in LME warehouse | Warehouse receipt |
| Cancelled Warrant | Metal requested for withdrawal | Inventory signal |
| Cash-to-3M Spread | Most important spread in LME markets | Basis indicator |
| Contango | Forward prices above nearby prices | Positive carry |
| Backwardation | Nearby prices above forwards | Tight physical supply |
| Ring Trading | Historic open-outcry session | Unique LME feature |
| Warehouse Stock | Metal stored in approved facilities | Supply indicator |
| LME Select | Electronic trading platform | Primary execution venue |
| Prompt Date Curve | LME version of futures curve | Key risk structure |
| Physical Premium | Local premium over LME price | Regional supply-demand |
| Delivery Point | Approved warehouse location | Rotterdam, Singapore |

---

# Intuition

Most futures exchanges trade:

- Monthly contracts
- Quarterly contracts
- Standard expiries

Examples:

- March
- June
- September
- December

LME is different.

LME contracts are built around:

> Specific delivery dates called prompt dates.

This structure evolved because industrial metal consumers and producers often need metal on very specific dates.

---

# Why LME Exists

Industrial participants need metals.

Examples:

- Automakers need aluminum.
- Construction firms need copper.
- Battery manufacturers need nickel.
- Steel producers need zinc.

LME provides:

- Price discovery
- Hedging
- Physical delivery infrastructure

---

# Major LME Metals

## Copper

Ticker:

```
CA
```

Often considered:

> The most important industrial metal.

Nicknamed:

> Dr. Copper

because it is viewed as an indicator of global economic activity.

---

## Aluminum

Ticker:

```
AH
```

Used in:

- Transportation
- Packaging
- Aerospace

---

## Zinc

Ticker:

```
ZN
```

Used heavily for galvanizing steel.

---

## Nickel

Ticker:

```
NI
```

Important for:

- Stainless steel
- EV batteries

---

## Lead

Ticker:

```
PB
```

Used primarily in batteries.

---

## Tin

Ticker:

```
SN
```

Used in electronics and soldering.

---

# Unique LME Contract Structure

Unlike CME futures:

LME contracts settle on:

specific calendar dates.

---

# Prompt Dates

A prompt date is the date on which delivery obligations occur.

Examples:

- Tomorrow
- Next week
- Three months
- Six months

---

# Daily Contracts

Near-term contracts may exist for every business day.

Example:

| Date | Contract |
|--------|----------|
| Monday | Contract |
| Tuesday | Contract |
| Wednesday | Contract |

This is very different from traditional futures exchanges.

---

# Weekly Contracts

After near-term dates:

Contracts become weekly.

---

# Monthly Contracts

Further out:

Contracts become monthly.

---

# The Famous 3-Month Contract

The benchmark LME contract.

When traders say:

> Copper is trading at \$9,000

they usually mean:

3-Month Copper.

---

# Cash Contract

The nearest settlement contract.

Often viewed as:

spot-equivalent.

Notation:

$$
\text{Cash Copper}
$$

---

# 3-Month Contract

Most liquid contract.

Notation:

$$
\text{Copper 3M}
$$

Used heavily by:

- Producers
- Consumers
- Hedge funds
- CTAs

---

# Cash-to-3M Spread

One of the most important relationships in LME.

Formula:

$$
\text{Cash-to-3M}
=
P_{Cash}
-
P_{3M}
$$

---

# Example

Cash:

$$
9000
$$

3M:

$$
9050
$$

Spread:

$$
9000 - 9050
$$

$$
=
-50
$$

Contango.

---

# Contango

Forward prices exceed nearby prices.

Example:

| Contract | Price |
|------------|---------|
| Cash | 9000 |
| 3M | 9050 |
| 6M | 9100 |

---

## Intuition

Metal storage costs money.

Holding inventory requires:

- Financing
- Insurance
- Warehousing

Forward prices reflect these costs.

---

# Backwardation

Nearby prices exceed forward prices.

Example:

| Contract | Price |
|------------|---------|
| Cash | 9200 |
| 3M | 9100 |

---

Spread:

$$
9200 - 9100
=
100
$$

Backwardation.

---

## Interpretation

Physical metal is scarce.

Immediate delivery is valuable.

---

# Basis Risk

Difference between:

- Physical metal
- Futures/forward prices

Often measured using:

$$
\text{Cash}
-
\text{Forward}
$$

relationships.

---

# LME Warehouse System

One of the most unique aspects of LME.

---

# Approved Warehouses

Metal must be stored in approved facilities.

Examples:

- Rotterdam
- Singapore
- New Orleans
- Busan

---

# Warehouse Stocks

Published daily.

Important fundamental signal.

Example:

Copper inventory:

$$
150,000
\text{ tons}
$$

---

# Warrant

Represents ownership of metal in warehouse.

Think of a warrant as:

> Title to specific inventory.

---

# Example

Warehouse contains:

$$
25
$$

tons copper.

A warrant is issued.

Owner of warrant controls:

that metal.

---

# Cancelled Warrants

Metal scheduled for withdrawal.

Important signal.

High cancelled warrants may imply:

Future inventory declines.

---

# Example

Inventory:

$$
100,000
\text{ tons}
$$

Cancelled:

$$
40,000
\text{ tons}
$$

Cancelled percentage:

$$
40\%
$$

Traders may view this as bullish.

---

# Physical Premiums

Physical buyers often pay:

$$
\text{LME Price}
+
\text{Regional Premium}
$$

---

# Example

LME Copper:

$$
9000
$$

US Midwest Premium:

$$
200
$$

Physical cost:

$$
9200
$$

---

# Tom-Next

Important short-term funding spread.

Definition:

Tomorrow settlement

vs

Next-day settlement.

---

Used heavily in:

- Financing
- Inventory management
- Carry trading

---

# Carry Trade

Core strategy in metals.

---

## Example

Buy:

Cash Copper

Sell:

3-Month Copper

Profit comes from:

Spread convergence.

---

# Cost-of-Carry Model

Theoretical relationship:

$$
F
=
S
e^{(r+s-c)T}
$$

where:

- $r$ = financing cost
- $s$ = storage cost
- $c$ = convenience yield

---

# Convenience Yield

Benefit of physically holding inventory.

Examples:

- Production continuity
- Avoiding shortages
- Meeting customer demand

---

High convenience yield often creates:

Backwardation.

---

# LME Trading Venues

## Ring

Historic open-outcry floor.

Unique to LME.

Traders physically stand in a circle.

---

## LME Select

Electronic trading platform.

Most volume occurs here today.

---

## Inter-Office Market

Dealer-to-dealer OTC market.

Important source of liquidity.

---

# Hedging Example

Copper producer expects:

$$
1000
\text{ tons}
$$

production in 3 months.

Risk:

Copper prices fall.

---

Solution:

Short:

3-Month Copper.

If prices decline:

Physical loss offset by futures gain.

---

# Consumer Hedge

Manufacturer needs:

$$
500
\text{ tons}
$$

in 3 months.

Risk:

Prices rise.

---

Solution:

Long:

3-Month Copper.

---

# Quantitative LME Trading

Many quantitative commodity funds trade:

- Copper
- Aluminum
- Nickel
- Zinc

---

# Common Signals

## Momentum

Trend-following.

Example:

$$
r_{12m}
-
r_{1m}
$$

---

## Carry

Curve shape.

Example:

Cash-to-3M spread.

---

## Inventory Signals

Warehouse stocks.

Examples:

- Inventory changes
- Cancelled warrant percentage

---

## Spread Signals

Examples:

$$
Cash - 3M
$$

$$
3M - 6M
$$

---

# Inventory Analytics

Example:

Inventory yesterday:

$$
150,000
$$

Inventory today:

$$
140,000
$$

Change:

$$
-10,000
$$

tons.

Potentially bullish signal.

---

# Curve Trading

Trade relationships between maturities.

Example:

Long:

$$
Cash
$$

Short:

$$
3M
$$

Bet on tightening supply.

---

# Common Data Fields

| Field | Description |
|---------|-------------|
| Metal | Copper |
| Prompt Date | Settlement date |
| Cash Price | Nearby contract |
| 3M Price | Benchmark contract |
| Inventory | Warehouse stock |
| Cancelled Warrants | Withdrawal requests |
| Volume | Trading activity |
| Open Interest | Outstanding contracts |

---

# Risk Factors

## Price Risk

Outright metal exposure.

---

## Curve Risk

Changes in term structure.

---

## Inventory Risk

Supply changes.

---

## Liquidity Risk

Thin contracts.

---

## Physical Risk

Regional shortages.

---

# Common Quant Models

## Time-Series Momentum

```python
signal = returns.rolling(252).sum()
```

---

## Inventory Signal

```python
signal = inventory.pct_change()
```

---

## Carry Signal

```python
carry = cash_price - three_month_price
```

---

# Optimization

Common objective:

$$
\max
(
\mu^T w
-
\lambda
w^T \Sigma w
)
$$

subject to:

- Position limits
- Sector limits
- Liquidity limits

---

# Common Technologies

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
statsmodels
scipy
arch
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

## Commodity Research

```python
xgboost
lightgbm
scikit-learn
```

---

# Example: Cash-to-3M Spread

Cash:

$$
9100
$$

3M:

$$
9000
$$

Spread:

$$
9100 - 9000
$$

$$
=
100
$$

Interpretation:

Backwardation.

---

# Example: Inventory Signal

Inventory:

$$
200,000
\rightarrow
180,000
$$

Change:

$$
-10\%
$$

Potentially bullish.

---

# Common Beginner Mistakes

## Treating LME Like CME Futures

LME uses:

Prompt dates

rather than traditional monthly expiries.

---

## Ignoring Inventory Data

Warehouse stocks are often as important as price.

---

## Ignoring Cancelled Warrants

These frequently provide information about future inventory changes.

---

## Looking Only at Outright Prices

Many LME traders focus primarily on:

- Cash-to-3M spreads
- Curve shape
- Carry

rather than outright direction.

---

## Ignoring Physical Premiums

Real-world metal users often care about:

$$
\text{LME Price}
+
\text{Regional Premium}
$$

rather than exchange price alone.

---

# Key Takeaways

- The LME is the dominant global exchange for industrial metals.
- Unlike traditional futures exchanges, LME contracts are organized around prompt dates.
- The Cash-to-3M spread is one of the most important indicators in metals markets.
- Contango and backwardation reflect inventory conditions, financing costs, and physical supply-demand.
- The LME warehouse system, warrants, and cancelled warrants provide unique information about physical markets.
- Quantitative metals strategies commonly use momentum, carry, inventory changes, and curve spreads.
- Many professional metals traders focus more on spreads and inventory flows than on outright price forecasts.
- Understanding the interaction between futures curves, warehousing, and physical supply chains is critical for trading LME products.
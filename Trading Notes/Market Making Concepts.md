# Market Making Concepts

## Quick Reference Table

| Concept | Definition | Key Idea |
|---|---|---|
| Market Maker | Trader or firm continuously quoting buy and sell prices | Provides liquidity |
| Bid | Price market maker is willing to buy | Lower side of quote |
| Ask / Offer | Price market maker is willing to sell | Higher side of quote |
| Spread | Difference between ask and bid | Main revenue source |
| Mid Price | Average of bid and ask | Approximate fair value |
| Inventory | Current position held by market maker | Source of risk |
| Adverse Selection | Trading against someone better informed | Major hidden cost |
| Order Flow | Sequence of market orders and limit orders | Contains information |
| Fill Probability | Chance resting quote gets executed | Depends on queue and price |
| Queue Position | Place in the limit order book | Determines priority |
| Tick Size | Minimum price increment | Controls quote granularity |
| Skew | Shift quotes to manage inventory | Encourages buys or sells |
| Edge | Expected profit per trade | Must exceed costs |
| Latency | Time delay in reacting to market changes | Critical in fast markets |
| Delta Hedging | Hedge directional exposure | Common in options market making |
| Gamma Risk | Risk from changing delta | Important in options |
| Volatility | Uncertainty of price movement | Affects spread width |

---

# Big Picture

Market making means continuously offering to both buy and sell.

A market maker quotes:

$$
\text{Bid}
<
\text{Ask}
$$

Example:

$$
99.99 / 100.01
$$

Meaning:

- Buy at $99.99$
- Sell at $100.01$

If the market maker buys at $99.99$ and sells at $100.01$, they earn:

$$
100.01 - 99.99
=
0.02
$$

per share before costs and risks.

---

# Core Intuition

A market maker is like a shopkeeper.

The shopkeeper buys inventory at a lower price and sells it at a higher price.

But unlike a normal store, financial prices move constantly.

So the market maker faces three major risks:

1. Inventory risk
2. Adverse selection risk
3. Execution and latency risk

---

# Bid, Ask, and Spread

## Bid

The bid is the price at which someone is willing to buy.

Example:

$$
\text{Bid}
=
99.99
$$

---

## Ask

The ask is the price at which someone is willing to sell.

Example:

$$
\text{Ask}
=
100.01
$$

---

## Spread

The spread is:

$$
\text{Spread}
=
\text{Ask}
-
\text{Bid}
$$

Example:

$$
100.01 - 99.99
=
0.02
$$

---

# Mid Price

The mid price is the midpoint between bid and ask.

$$
\text{Mid}
=
\frac{\text{Bid}+\text{Ask}}{2}
$$

Example:

$$
\frac{99.99+100.01}{2}
=
100.00
$$

The mid price is often used as a quick estimate of fair value.

---

# Spread Capture

Suppose a market maker quotes:

$$
99.99 / 100.01
$$

They buy one share at:

$$
99.99
$$

Then sell one share at:

$$
100.01
$$

Gross profit:

$$
100.01-99.99
=
0.02
$$

This is spread capture.

---

# Why Spread Capture Is Not Free Money

The market maker earns spread only if price does not move against them.

Example:

Market maker buys at:

$$
99.99
$$

But the fair value immediately falls to:

$$
99.90
$$

Then their mark-to-market loss is approximately:

$$
99.90 - 99.99
=
-0.09
$$

This loss is much larger than the spread.

---

# Inventory Risk

Inventory is the position the market maker holds.

Example:

If the market maker buys:

$$
10,000
$$

shares and sells:

$$
7,000
$$

shares, inventory is:

$$
10,000 - 7,000
=
3,000
$$

shares long.

---

# Inventory PnL

If inventory is $q$ shares and price changes by $\Delta S$, approximate PnL is:

$$
\Delta PnL
=
q \Delta S
$$

Example:

Inventory:

$$
q=3,000
$$

Price drops:

$$
\Delta S=-0.10
$$

PnL:

$$
3,000(-0.10)
=
-300
$$

---

# Inventory Skew

Market makers adjust quotes to reduce unwanted inventory.

Suppose a market maker is too long.

They want to sell.

So they may lower both bid and ask:

$$
99.98 / 100.00
$$

instead of:

$$
99.99 / 100.01
$$

This makes selling easier and buying less attractive.

---

# Quote Skew Formula

A simple conceptual quote model:

$$
\text{Bid}
=
\text{Fair Value}
-
\frac{\text{Spread}}{2}
-
\text{Inventory Skew}
$$

$$
\text{Ask}
=
\text{Fair Value}
+
\frac{\text{Spread}}{2}
-
\text{Inventory Skew}
$$

If inventory is long, skew may be positive, shifting quotes downward.

---

# Adverse Selection

Adverse selection occurs when a market maker trades with someone who has better information.

Example:

Market maker quotes:

$$
99.99 / 100.01
$$

A trader aggressively sells to the market maker at:

$$
99.99
$$

Soon after, price drops to:

$$
99.80
$$

The market maker got filled because someone knew or inferred price was going down.

---

# Adverse Selection Intuition

Getting filled is not always good.

A resting quote may be filled because:

- The quote is stale
- News arrived
- A faster trader detected a price move
- Order flow is toxic
- The market maker is quoting too aggressively

---

# Toxic Order Flow

Order flow is toxic when fills tend to predict losses.

Example:

If every time your bid gets filled, price keeps falling, then sell orders hitting you contain information.

Toxic flow creates adverse selection losses.

---

# Market Maker Expected Value

A simple expected value decomposition:

$$
E[Profit]
=
\text{Spread Capture}
-
\text{Adverse Selection}
-
\text{Inventory Cost}
-
\text{Fees}
$$

A trade is profitable only if:

$$
E[Profit] > 0
$$

---

# Quote Width

Market makers widen spreads when risk is high.

Spreads widen when:

- Volatility increases
- Liquidity decreases
- Inventory risk increases
- Adverse selection risk increases
- Latency risk increases
- News risk increases

---

# Simple Spread Logic

A simple conceptual spread:

$$
\text{Spread}
=
\text{Order Processing Cost}
+
\text{Inventory Risk Cost}
+
\text{Adverse Selection Cost}
+
\text{Profit Margin}
$$

---

# Volatility and Spread

Higher volatility means price can move more before inventory is unwound.

Therefore:

$$
\uparrow \sigma
\Rightarrow
\uparrow \text{Spread}
$$

Low volatility usually supports tighter spreads.

---

# Tick Size

Tick size is the minimum price increment.

Example:

If tick size is:

$$
0.01
$$

then valid prices are:

$$
100.00,\ 100.01,\ 100.02,\ldots
$$

Tick size affects:

- Spread
- Queueing
- Fill probability
- Market maker profitability

---

# Queue Position

Most limit order books use price-time priority.

Priority is based on:

1. Better price
2. Earlier time

If you join the bid after:

$$
50,000
$$

shares are already ahead of you, those shares must fill before your order fills.

---

# Fill Probability

Fill probability depends on:

- Distance from mid price
- Queue position
- Market order arrival rate
- Cancellation rate
- Volatility
- Toxicity of order flow

A quote closer to mid has higher fill probability but worse adverse selection risk.

---

# Maker-Taker Fees

Some exchanges pay rebates to liquidity providers and charge liquidity takers.

Example:

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

For high-frequency market making, fees and rebates can materially affect profitability.

---

# Limit Order vs Market Order

## Limit Order

Adds liquidity.

May earn spread.

Risk:

May not fill or may be adversely selected.

---

## Market Order

Removes liquidity.

Guarantees faster execution.

Risk:

Pays spread and may cause impact.

---

# Market Making PnL Components

A market maker's PnL can be decomposed into:

$$
PnL
=
\text{Spread PnL}
+
\text{Inventory PnL}
-
\text{Fees}
-
\text{Adverse Selection Loss}
$$

---

# Spread PnL

Profit from buying at bid and selling at ask.

Example:

Buy:

$$
1000
$$

shares at:

$$
99.99
$$

Sell:

$$
1000
$$

shares at:

$$
100.01
$$

Spread PnL:

$$
1000(100.01-99.99)
=
20
$$

---

# Inventory PnL

Profit or loss from price movement while holding position.

Example:

Long:

$$
1000
$$

shares.

Price falls from:

$$
100.00
\rightarrow
99.95
$$

Inventory PnL:

$$
1000(99.95-100.00)
=
-50
$$

---

# Net PnL Example

Spread PnL:

$$
20
$$

Inventory loss:

$$
-50
$$

Fees:

$$
5
$$

Net PnL:

$$
20-50-5
=
-35
$$

Even though spread was captured, total trade lost money.

---

# Fair Value

Market makers need a fair value estimate.

Fair value may use:

- Mid price
- Microprice
- Related instruments
- Futures
- ETFs
- Options
- Order flow
- Statistical models

---

# Microprice

Microprice adjusts mid price using book imbalance.

Let:

- $B$ = bid price
- $A$ = ask price
- $B_s$ = bid size
- $A_s$ = ask size

A common microprice formula:

$$
\text{Microprice}
=
\frac{
A B_s + B A_s
}{
A_s+B_s
}
$$

If bid size is large relative to ask size, microprice moves closer to the ask.

---

# Order Book Imbalance

A common feature:

$$
I
=
\frac{
B_s-A_s
}{
B_s+A_s
}
$$

where:

- $B_s$ = bid size
- $A_s$ = ask size

If:

$$
I>0
$$

there is more size on the bid.

This may indicate upward pressure.

---

# Example: Order Book Imbalance

Bid size:

$$
800
$$

Ask size:

$$
200
$$

Then:

$$
I
=
\frac{800-200}{800+200}
=
\frac{600}{1000}
=
0.6
$$

This indicates strong bid-side depth.

---

# Market Making in Equities

Equity market makers quote stocks and ETFs.

Important issues:

- Tick size
- Exchange rebates
- Fragmented venues
- Short locates
- Reg NMS
- Queue priority
- Opening and closing auctions

Equity market makers often hedge ETF and single-stock exposures against correlated instruments.

---

# Market Making in Futures

Futures market makers quote contracts like:

- ES
- NQ
- CL
- GC
- ZN

Important issues:

- Contract multiplier
- Tick value
- Expiration and roll
- Exchange margin
- Calendar spreads

Example ES tick value:

$$
0.25 \times 50
=
12.50
$$

One ES tick is worth:

$$
\$12.50
$$

per contract.

---

# Market Making in Options

Options market making is more complex because the market maker manages Greeks.

Important risks:

- Delta
- Gamma
- Vega
- Theta
- Rho
- Skew
- Vol surface changes

---

# Delta

Delta measures sensitivity to the underlying.

$$
\Delta
=
\frac{\partial V}{\partial S}
$$

If an option has:

$$
\Delta = 0.50
$$

then a $1 increase in stock price changes option value by about:

$$
0.50
$$

---

# Gamma

Gamma measures how delta changes.

$$
\Gamma
=
\frac{\partial^2 V}{\partial S^2}
$$

High gamma means delta changes rapidly.

---

# Vega

Vega measures sensitivity to volatility.

$$
\text{Vega}
=
\frac{\partial V}{\partial \sigma}
$$

Options market makers often quote implied volatility rather than raw option price.

---

# Theta

Theta measures time decay.

$$
\Theta
=
\frac{\partial V}{\partial t}
$$

Short options often earn theta but take gamma and tail risk.

---

# Delta Hedging

Options market makers hedge directional exposure using the underlying.

Example:

Short one call contract.

Call delta:

$$
0.60
$$

Contract controls:

$$
100
$$

shares.

Position delta:

$$
-0.60 \times 100
=
-60
$$

To delta hedge, buy:

$$
60
$$

shares.

---

# Gamma Scalping

Gamma scalping means repeatedly delta hedging a long-gamma position.

Long gamma benefits from large realized moves.

Basic intuition:

- Price rises: delta increases, sell stock to hedge
- Price falls: delta decreases, buy stock to hedge

This can create buy-low, sell-high behavior.

---

# Market Making in ETFs

ETF market makers use arbitrage relationships.

ETF price should be close to net asset value:

$$
NAV
$$

If ETF trades above NAV:

- Sell ETF
- Buy basket

If ETF trades below NAV:

- Buy ETF
- Sell basket

Large authorized participants can create or redeem ETF shares.

---

# Market Making in FX

FX market making is often dealer-based.

Important issues:

- Bilateral credit
- Last look
- Internalization
- Skew by client flow
- Cross-currency relationships

Example:

If EUR/USD and USD/JPY move, EUR/JPY fair value changes.

Approximate triangular relation:

$$
EUR/JPY
=
EUR/USD
\times
USD/JPY
$$

---

# Market Making in Crypto

Crypto market making has unique risks:

- Fragmented exchanges
- Withdrawal risk
- Exchange credit risk
- Funding rates
- 24/7 trading
- Higher volatility
- API reliability issues

---

# Avellaneda-Stoikov Model

A classic academic model of market making.

Goal:

Choose optimal bid and ask quotes while considering:

- Fair value
- Inventory
- Volatility
- Risk aversion
- Time horizon

The reservation price is often written conceptually as:

$$
r
=
S
-
q\gamma\sigma^2(T-t)
$$

where:

- $r$ = reservation price
- $S$ = mid price
- $q$ = inventory
- $\gamma$ = risk aversion
- $\sigma$ = volatility
- $T-t$ = remaining horizon

If inventory $q$ is positive, reservation price shifts downward to encourage selling.

---

# Quote Skew from Inventory

If long inventory:

$$
q>0
$$

then:

$$
r<S
$$

The market maker quotes lower to reduce inventory.

If short inventory:

$$
q<0
$$

then:

$$
r>S
$$

The market maker quotes higher to encourage buying back.

---

# Market Maker Risk Controls

Common limits:

- Max position
- Max notional
- Max loss
- Max order size
- Max order rate
- Max stale quote time
- Max volatility condition
- Kill switch

---

# Kill Switch

A kill switch cancels orders and stops quoting when something goes wrong.

Triggered by:

- Data feed outage
- Excessive losses
- Latency spike
- Bad model output
- Exchange disconnect
- Position limit breach

---

# Latency Risk

Latency risk occurs when quotes become stale.

Example:

Fair value moves from:

$$
100.00
\rightarrow
99.80
$$

but market maker still quotes bid:

$$
99.99
$$

Fast traders sell into stale bid.

Loss:

$$
99.80 - 99.99
=
-0.19
$$

---

# Market Data Feeds

Market makers use:

- Top-of-book data
- Full depth order book
- Trades
- Auction imbalance feeds
- Related instrument feeds
- News and event feeds

---

# Production Market Making System

Typical architecture:

$$
\text{Market Data}
\rightarrow
\text{Fair Value Model}
\rightarrow
\text{Quote Engine}
\rightarrow
\text{Risk Checks}
\rightarrow
\text{Order Gateway}
\rightarrow
\text{Exchange}
$$

Executions flow back:

$$
\text{Exchange}
\rightarrow
\text{Fills}
\rightarrow
\text{Position Manager}
\rightarrow
\text{Risk Engine}
\rightarrow
\text{Quote Updates}
$$

---

# Common Technologies

## Low-Latency Systems

```text
C++
Rust
Java
C#
kernel bypass
Solarflare / Onload
FPGA
shared memory
lock-free queues
```

---

## Research and Analytics

```python
pandas
numpy
polars
pyarrow
scipy
statsmodels
scikit-learn
lightgbm
xgboost
```

---

## Messaging and Infrastructure

```text
FIX
ITCH
OUCH
SBE
Kafka
Redis
Aerospike
ClickHouse
kdb+/q
Snowflake
Datadog
Grafana
```

---

# Python Example: Spread, Mid, and Imbalance

```python
bid = 99.99
ask = 100.01
bid_size = 800
ask_size = 200

spread = ask - bid
mid = (bid + ask) / 2
imbalance = (bid_size - ask_size) / (bid_size + ask_size)

print(spread)
print(mid)
print(imbalance)
```

---

# Python Example: Simple Quote Skew

```python
fair_value = 100.00
base_spread = 0.02
inventory = 3000
max_inventory = 10000
skew_strength = 0.01

inventory_ratio = inventory / max_inventory
skew = skew_strength * inventory_ratio

bid = fair_value - base_spread / 2 - skew
ask = fair_value + base_spread / 2 - skew

print(bid, ask)
```

---

# Python Example: Market Maker PnL

```python
buy_price = 99.99
sell_price = 100.01
shares = 1000
fees = 5

spread_pnl = shares * (sell_price - buy_price)
net_pnl = spread_pnl - fees

print(spread_pnl)
print(net_pnl)
```

---

# Worked Example 1: Basic Spread Capture

A market maker quotes:

$$
50.00 / 50.04
$$

They buy:

$$
500
$$

shares at the bid and sell:

$$
500
$$

shares at the ask.

Find gross PnL.

## Solution

Spread:

$$
50.04 - 50.00
=
0.04
$$

Gross PnL:

$$
500 \times 0.04
=
20
$$

Answer:

$$
\$20
$$

---

# Worked Example 2: Inventory Loss

A market maker is long:

$$
20,000
$$

shares.

Price falls by:

$$
0.03
$$

Find inventory PnL.

## Solution

$$
PnL
=
q\Delta S
$$

$$
=
20,000(-0.03)
$$

$$
=
-600
$$

Answer:

$$
-\$600
$$

---

# Worked Example 3: Net Market Making PnL

Spread PnL:

$$
\$300
$$

Inventory PnL:

$$
-\$450
$$

Fees:

$$
\$50
$$

Find net PnL.

## Solution

$$
Net\ PnL
=
300 - 450 - 50
$$

$$
=
-200
$$

Answer:

$$
-\$200
$$

---

# Worked Example 4: Microprice

Suppose:

$$
B=99.99
$$

$$
A=100.01
$$

$$
B_s=900
$$

$$
A_s=100
$$

Microprice:

$$
\frac{
A B_s + B A_s
}{
A_s+B_s
}
$$

## Solution

$$
\text{Microprice}
=
\frac{
100.01(900)+99.99(100)
}{
900+100
}
$$

$$
=
\frac{
90009+9999
}{
1000
}
$$

$$
=
100.008
$$

The microprice is closer to the ask because bid size is much larger than ask size.

---

# Worked Example 5: Options Delta Hedge

A market maker is short:

$$
10
$$

call option contracts.

Each contract controls:

$$
100
$$

shares.

Each call has delta:

$$
0.40
$$

Find hedge.

## Solution

Short option delta:

$$
-10 \times 100 \times 0.40
=
-400
$$

To become delta-neutral, buy:

$$
400
$$

shares.

---

# Common Interview Questions

## Question 1

Why do market makers widen spreads when volatility increases?

## Answer

Because price can move farther before the market maker can unwind inventory. Higher volatility increases inventory risk and adverse selection risk, so the market maker requires more compensation.

---

## Question 2

Why can getting filled be bad?

## Answer

A fill may mean the quote was too aggressive or stale. If informed traders trade against the market maker, the price may continue moving against the filled position.

---

## Question 3

How does a market maker reduce long inventory?

## Answer

They skew quotes downward:

- Lower bid to discourage more buying
- Lower ask to encourage others to buy from them

This helps sell inventory.

---

## Question 4

What is the difference between spread capture and total PnL?

## Answer

Spread capture only measures buying low and selling high. Total PnL also includes inventory price movement, fees, rebates, hedging costs, and adverse selection.

---

## Question 5

How does options market making differ from stock market making?

## Answer

Options market makers must manage Greeks. They do not only quote price; they quote implied volatility and hedge delta, gamma, vega, theta, and skew risk.

---

# Common Beginner Mistakes

## Mistake 1: Thinking Spread Equals Profit

The spread is potential compensation, not guaranteed profit.

Adverse selection and inventory losses can exceed spread capture.

---

## Mistake 2: Ignoring Inventory

Market makers are not always flat.

Inventory creates directional exposure.

---

## Mistake 3: Assuming Every Fill Is Good

A fill can indicate toxic flow.

---

## Mistake 4: Ignoring Latency

A quote that was fair milliseconds ago may now be stale.

---

## Mistake 5: Forgetting Fees and Rebates

At high volume, fee economics can determine whether a strategy works.

---

## Mistake 6: Treating Market Making as Pure Prediction

Market making is not just predicting price direction.

It is balancing:

- Spread
- Fill probability
- Inventory
- Adverse selection
- Fees
- Latency
- Risk limits

---

# Key Takeaways

- Market makers provide liquidity by quoting both bid and ask prices.
- The spread compensates for costs, inventory risk, and adverse selection risk.
- Spread capture is not guaranteed profit.
- Inventory management is central to market making.
- Adverse selection occurs when informed traders trade against stale or mispriced quotes.
- Quote skew helps control inventory.
- Fill probability and queue position determine whether resting orders execute.
- Options market making adds Greek risk management, especially delta, gamma, vega, and theta.
- Production market making systems require fast market data, fair value models, quote engines, risk checks, order gateways, and kill switches.
- Successful market making is about balancing expected edge against risk and execution uncertainty.
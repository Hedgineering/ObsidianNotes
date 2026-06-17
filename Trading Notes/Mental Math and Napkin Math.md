# Mental Math and Napkin Math for Quant Finance, Quant Interviews, and System Design

## Quick Reference Table

| Category | Key Idea | Useful Approximation |
|---|---|---|
| Percentages | Convert percent changes into decimals | $5\% = 0.05$ |
| Basis Points | Used for rates, spreads, fees | $1\text{ bp}=0.01\%=0.0001$ |
| Compounding | Small rates approximately add | $(1+r)^n \approx 1+nr$ for small $r$ |
| Rule of 72 | Time to double money | $\text{Years} \approx \frac{72}{100r}$ |
| Annualization | Scale daily volatility by $\sqrt{252}$ | $\sigma_{ann} \approx \sigma_{daily}\sqrt{252}$ |
| Sharpe Ratio | Risk-adjusted return | $SR=\frac{\mu-r_f}{\sigma}$ |
| Expected Value | Probability-weighted outcome | $E[X]=\sum_i p_i x_i$ |
| Kelly Bet | Growth-optimal betting fraction | $f^\star=\frac{bp-q}{b}$ |
| Latency | Delay in systems/trading | $1\text{ ms}=1000\mu s$ |
| Throughput | Requests/events per second | $\text{QPS}=\frac{\text{Requests}}{\text{Seconds}}$ |
| Storage | Data size estimate | $\text{Rows}\times\text{Bytes per row}$ |
| Network | Transfer time | $\frac{\text{Data Size}}{\text{Bandwidth}}$ |

---

# Big Picture

Mental math and napkin math are about quickly estimating quantities without exact computation.

In quant interviews and system design interviews, the goal is usually not perfect arithmetic.

The goal is to show:

- Clear assumptions
- Correct order of magnitude
- Fast estimation
- Sanity checking
- Good risk intuition

A good napkin estimate is usually better than a precise but unjustified number.

---

# Core Mental Math Principles

## Principle 1: Round First

Instead of computing:

$$
397 \times 51
$$

round to:

$$
400 \times 50
=
20,000
$$

Exact value:

$$
397 \times 51
=
20,247
$$

Close enough for many interview estimates.

---

## Principle 2: Break Problems Into Pieces

Example:

$$
17 \times 24
$$

Break:

$$
17 \times 24
=
17 \times (20+4)
$$

$$
=
340+68
=
408
$$

---

## Principle 3: Use Powers of Ten

Approximate:

$$
3.2M \approx 3 \times 10^6
$$

$$
45B \approx 4.5 \times 10^{10}
$$

Useful in:

- Market sizing
- System design
- Data storage
- Trading notional estimates

---

## Principle 4: Track Units

Always ask:

- Dollars?
- Shares?
- Contracts?
- Seconds?
- Requests per second?
- Bytes?
- Basis points?

Most mistakes come from unit confusion.

---

# Percentages

## Basic Conversions

$$
1\%
=
0.01
$$

$$
5\%
=
0.05
$$

$$
25\%
=
0.25
$$

$$
100\%
=
1
$$

---

## Percent Change

Formula:

$$
\text{Percent Change}
=
\frac{\text{New}-\text{Old}}{\text{Old}}
$$

Example:

Price moves from:

$$
100
\rightarrow
105
$$

Percent change:

$$
\frac{105-100}{100}
=
0.05
=
5\%
$$

---

# Basis Points

Basis points are used constantly in finance.

$$
1\text{ bp}
=
0.01\%
=
0.0001
$$

$$
100\text{ bps}
=
1\%
$$

$$
250\text{ bps}
=
2.5\%
$$

---

# Example: Basis Point PnL

Portfolio value:

$$
100M
$$

Return:

$$
5\text{ bps}
$$

PnL:

$$
100,000,000 \times 0.0005
=
50,000
$$

Answer:

$$
\$50,000
$$

---

# Rule of 72

Approximate time to double money:

$$
\text{Years to Double}
\approx
\frac{72}{\text{Annual Return in Percent}}
$$

Examples:

| Return | Years to Double |
|---|---|
| 6% | 12 years |
| 8% | 9 years |
| 10% | 7.2 years |
| 12% | 6 years |

---

# Simple vs Compound Returns

## Simple Return

$$
R
=
\frac{P_1-P_0}{P_0}
$$

---

## Compound Return

$$
P_T
=
P_0(1+r)^T
$$

---

# Useful Compound Approximations

For small $r$:

$$
(1+r)^n
\approx
1+nr
$$

Better approximation:

$$
(1+r)^n
\approx
e^{nr}
$$

---

# Common Squares to Memorize

| Number | Square |
|---|---|
| $11$ | $121$ |
| $12$ | $144$ |
| $13$ | $169$ |
| $14$ | $196$ |
| $15$ | $225$ |
| $16$ | $256$ |
| $17$ | $289$ |
| $18$ | $324$ |
| $19$ | $361$ |
| $20$ | $400$ |
| $25$ | $625$ |
| $30$ | $900$ |

---

# Common Square Roots to Memorize

| Value | Approximate Square Root |
|---|---|
| $\sqrt{2}$ | $1.414$ |
| $\sqrt{3}$ | $1.732$ |
| $\sqrt{5}$ | $2.236$ |
| $\sqrt{10}$ | $3.162$ |
| $\sqrt{252}$ | $\approx 15.9 \approx 16$ |
| $\sqrt{365}$ | $\approx 19.1$ |

---

# Probability Facts to Memorize

## Complements

$$
P(A^c)
=
1-P(A)
$$

Example:

If probability of rain is:

$$
30\%
$$

then probability of no rain is:

$$
70\%
$$

---

## Independent Events

$$
P(A\cap B)
=
P(A)P(B)
$$

Example:

Two fair coin flips.

Probability of two heads:

$$
\frac{1}{2}
\times
\frac{1}{2}
=
\frac{1}{4}
$$

---

## At Least One Event

For independent repeated trials:

$$
P(\text{at least one success})
=
1-(1-p)^n
$$

---

# Useful Approximation

For small $p$:

$$
1-(1-p)^n
\approx
np
$$

when $np$ is small.

Example:

A rare event has probability:

$$
p=0.001
$$

over:

$$
n=100
$$

trials.

Approximate probability:

$$
np
=
100(0.001)
=
0.10
=
10\%
$$

---

# Expected Value

Expected value is the backbone of quant interview thinking.

$$
E[X]
=
\sum_i p_i x_i
$$

---

# Example: Simple Bet

Win:

$$
\$100
$$

with probability:

$$
60\%
$$

Lose:

$$
\$50
$$

with probability:

$$
40\%
$$

Expected value:

$$
0.6(100)+0.4(-50)
=
60-20
=
40
$$

Positive EV:

$$
\$40
$$

---

# Variance and Standard Deviation

Variance measures spread.

$$
Var(X)
=
E[X^2]
-
(E[X])^2
$$

Standard deviation:

$$
\sigma
=
\sqrt{Var(X)}
$$

---

# Quant Finance Numbers to Memorize

## Trading Days

| Quantity | Approximation |
|---|---|
| Trading days per year | $252$ |
| Trading days per month | $21$ |
| Trading weeks per year | $52$ |
| Calendar days per year | $365$ |
| Months per year | $12$ |

---

## Volatility Scaling

Daily to annual volatility:

$$
\sigma_{ann}
=
\sigma_{daily}\sqrt{252}
$$

Since:

$$
\sqrt{252}
\approx
16
$$

A daily volatility of:

$$
1\%
$$

annualizes to:

$$
16\%
$$

---

## Annual to Daily Volatility

$$
\sigma_{daily}
=
\frac{\sigma_{ann}}{\sqrt{252}}
$$

Example:

Annual volatility:

$$
32\%
$$

Daily volatility:

$$
\frac{32\%}{16}
=
2\%
$$

---

# Common Annualized Volatility Ranges

| Asset | Rough Annual Volatility |
|---|---|
| Short-term Treasuries | $1\%-5\%$ |
| Bonds | $5\%-10\%$ |
| Large-cap equities | $15\%-25\%$ |
| Equity indexes | $15\%-20\%$ |
| Single stocks | $25\%-80\%$ |
| FX majors | $5\%-12\%$ |
| Commodities | $20\%-50\%$ |
| Crypto | $50\%-100\%+$ |

---

# Sharpe Ratio

Sharpe ratio:

$$
SR
=
\frac{\mu-r_f}{\sigma}
$$

where:

- $\mu$ = expected return
- $r_f$ = risk-free rate
- $\sigma$ = volatility

---

# Example

Annual return:

$$
12\%
$$

Risk-free rate:

$$
4\%
$$

Volatility:

$$
16\%
$$

Sharpe:

$$
SR
=
\frac{12\%-4\%}{16\%}
=
\frac{8\%}{16\%}
=
0.5
$$

---

# Sharpe Ratio Intuition

| Sharpe | Interpretation |
|---|---|
| $0$ | No excess return |
| $0.5$ | Decent |
| $1.0$ | Good |
| $2.0$ | Excellent |
| $3.0+$ | Extremely strong, often capacity-limited or suspicious |

---

# Information Ratio

Used for alpha strategies.

$$
IR
=
\frac{\text{Active Return}}
{\text{Tracking Error}}
$$

Similar intuition to Sharpe ratio.

---

# Drawdown

Drawdown measures peak-to-trough loss.

$$
\text{Drawdown}
=
\frac{\text{Current Value}-\text{Peak Value}}
{\text{Peak Value}}
$$

Example:

Portfolio falls from:

$$
100
\rightarrow
80
$$

Drawdown:

$$
\frac{80-100}{100}
=
-20\%
$$

---

# Notional Exposure

Notional is total market exposure.

For futures:

$$
\text{Notional}
=
\text{Price}
\times
\text{Multiplier}
\times
\text{Contracts}
$$

---

# Example: ES Futures

Suppose:

- ES price = $6000$
- Multiplier = $50$
- Contracts = $2$

Notional:

$$
6000 \times 50 \times 2
=
600,000
$$

---

# DV01

DV01 means dollar value of a 1 basis point move.

If:

$$
DV01 = 5000
$$

then a:

$$
10\text{ bp}
$$

move produces approximately:

$$
5000 \times 10
=
50,000
$$

dollars of PnL.

---

# Options Quick Facts

## Call Payoff

$$
\max(S_T-K,0)
$$

---

## Put Payoff

$$
\max(K-S_T,0)
$$

---

## Approximate Delta PnL

$$
\Delta V
\approx
\Delta
\cdot
\Delta S
$$

---

## Approximate Delta-Gamma PnL

$$
\Delta V
\approx
\Delta\Delta S
+
\frac{1}{2}\Gamma(\Delta S)^2
$$

---

# Black-Scholes Intuition to Memorize

Option value increases when:

- Underlying price moves favorably
- Volatility increases
- Time to expiration increases
- Interest rates increase for calls
- Interest rates decrease for puts

---

# Quant Interview Mental Math Patterns

## Pattern 1: Expected Value

Question:

A die pays the face value in dollars. Cost to play is $3.50$. Should you play?

Expected payout:

$$
E[X]
=
\frac{1+2+3+4+5+6}{6}
=
\frac{21}{6}
=
3.50
$$

Fair game.

---

## Pattern 2: Conditional Probability

Question:

Two children. At least one is a boy. Probability both are boys?

Sample space:

$$
BB, BG, GB, GG
$$

Given at least one boy:

$$
BB, BG, GB
$$

Probability both boys:

$$
\frac{1}{3}
$$

---

## Pattern 3: Break-Even Probability

If you risk:

$$
L
$$

to win:

$$
W
$$

break-even probability:

$$
p^\star
=
\frac{L}{W+L}
$$

Example:

Win $3$, lose $1$.

$$
p^\star
=
\frac{1}{3+1}
=
25\%
$$

---

## Pattern 4: Bayes Rule

Core formula:

$$
P(A\mid B)
=
\frac{P(B\mid A)P(A)}
{P(B)}
$$

For interviews, often expand denominator:

$$
P(B)
=
P(B\mid A)P(A)
+
P(B\mid A^c)P(A^c)
$$

---

# Bayes Example

Disease prevalence:

$$
1\%
$$

Sensitivity:

$$
99\%
$$

False positive rate:

$$
5\%
$$

Find probability of disease given positive test.

Numerator:

$$
0.99 \times 0.01
=
0.0099
$$

Denominator:

$$
0.99(0.01)+0.05(0.99)
=
0.0099+0.0495
=
0.0594
$$

Posterior:

$$
\frac{0.0099}{0.0594}
\approx
0.167
$$

Answer:

$$
16.7\%
$$

Key intuition:

Low base rates matter.

---

# Kelly Criterion

Used in betting, trading, and risk sizing.

For a bet that wins $b$ dollars per $1$ risked:

$$
f^\star
=
\frac{bp-q}{b}
$$

where:

$$
q=1-p
$$

---

# Even-Money Kelly

If win/loss payoff is symmetric:

$$
f^\star
=
2p-1
$$

Example:

Win probability:

$$
55\%
$$

Kelly fraction:

$$
2(0.55)-1
=
0.10
$$

Bet:

$$
10\%
$$

of bankroll.

In practice, traders often use fractional Kelly.

---

# System Design Numbers to Memorize

## Time Units

| Unit | Approximation |
|---|---|
| $1$ second | $1000$ ms |
| $1$ ms | $1000$ microseconds |
| $1$ microsecond | $1000$ ns |
| $1$ day | $86,400$ seconds |
| $1$ year | $\approx 31.5M$ seconds |

---

## Storage Units

| Unit | Approximation |
|---|---|
| $1$ KB | $10^3$ bytes |
| $1$ MB | $10^6$ bytes |
| $1$ GB | $10^9$ bytes |
| $1$ TB | $10^{12}$ bytes |
| $1$ PB | $10^{15}$ bytes |

---

## Common Data Sizes

| Data Type | Approximate Size |
|---|---|
| Boolean | $1$ byte |
| int32 | $4$ bytes |
| int64 | $8$ bytes |
| float64 | $8$ bytes |
| UUID string | $36$ characters |
| Timestamp | $8$ bytes |
| Small JSON event | $0.5$ KB to $2$ KB |
| Typical log line | $0.2$ KB to $2$ KB |

---

# System Design Throughput Formula

$$
\text{QPS}
=
\frac{\text{Total Requests}}
{\text{Seconds}}
$$

---

# Example: Daily Active Users

Suppose:

$$
10M
$$

daily active users.

Each sends:

$$
20
$$

requests per day.

Total requests:

$$
10M \times 20
=
200M
$$

Seconds per day:

$$
86,400
\approx
100,000
$$

Average QPS:

$$
\frac{200M}{100,000}
=
2000
$$

Peak QPS may be:

$$
5\times
$$

average:

$$
10,000
$$

---

# Storage Estimate Example

Suppose:

- $100M$ events per day
- $1$ KB per event

Daily storage:

$$
100M \times 1KB
=
100GB
$$

Monthly storage:

$$
100GB \times 30
=
3TB
$$

Yearly storage:

$$
100GB \times 365
=
36.5TB
$$

---

# Market Data Storage Example

Suppose:

- $1B$ ticks per day
- $100$ bytes per tick

Daily raw size:

$$
1B \times 100
=
100B
\text{ bytes}
$$

Since:

$$
1GB
=
10^9
\text{ bytes}
$$

Daily storage:

$$
100GB
$$

With replication factor:

$$
3
$$

Total:

$$
300GB
$$

per day.

---

# Latency Reference

| Operation | Rough Latency |
|---|---|
| CPU L1 cache | $\sim 1$ ns |
| Main memory access | $\sim 100$ ns |
| SSD read | $\sim 100\mu s$ |
| Network same data center | $\sim 0.1$ ms to $1$ ms |
| Cross-region network | $\sim 50$ ms to $150$ ms |
| Disk seek HDD | $\sim 5$ ms to $10$ ms |

---

# Quant Systems Architecture Napkin Math

## Example: Real-Time Tick Pipeline

Assume:

- $500,000$ ticks per second
- $200$ bytes per tick

Throughput:

$$
500,000 \times 200
=
100,000,000
\text{ bytes/s}
$$

$$
=
100
\text{ MB/s}
$$

Per day:

$$
100
\text{ MB/s}
\times
86,400
\approx
8,640,000
\text{ MB}
$$

$$
\approx
8.6
\text{ TB/day}
$$

---

# Common Quant Finance Constants

| Quantity | Useful Approximation |
|---|---|
| Trading days per year | $252$ |
| Minutes in trading day for US equities | $390$ |
| Seconds in US equity trading day | $23,400$ |
| $\sqrt{252}$ | $16$ |
| $1\text{ bp}$ | $0.0001$ |
| $100\text{ bps}$ | $1\%$ |
| Futures multiplier for ES | $\$50$ per index point |
| ES tick size | $0.25$ index points |
| ES tick value | $\$12.50$ |
| Standard equity option contract | $100$ shares |
| US equities regular hours | $9:30$ AM to $4:00$ PM ET |

---

# Trading Cost Napkin Math

Suppose:

- Portfolio = $100M$
- Turnover = $20\%$ per day
- Cost = $5$ bps per dollar traded

Daily traded notional:

$$
100M \times 20\%
=
20M
$$

Daily cost:

$$
20M \times 0.0005
=
10,000
$$

Annual cost:

$$
10,000 \times 252
=
2,520,000
$$

Trading costs matter.

---

# Capacity Napkin Math

Suppose strategy alpha is:

$$
10
\text{ bps/day}
$$

Trading cost is:

$$
2
\text{ bps}
+
0.01
\sqrt{\frac{Q}{V}}
$$

As trade size $Q$ grows relative to volume $V$, impact rises.

Basic idea:

$$
\text{Net Alpha}
=
\text{Gross Alpha}
-
\text{Costs}
-
\text{Impact}
$$

A strategy dies when:

$$
\text{Net Alpha}
\leq
0
$$

---

# Interview Method for Napkin Estimates

Use this structure:

1. Restate the quantity.
2. Define assumptions.
3. Round numbers.
4. Calculate order of magnitude.
5. Sanity check result.
6. Mention bottlenecks or uncertainty.

---

# Example: Estimate Storage for Order Book Data

Assume:

- $1000$ symbols
- $10$ levels per side
- $2$ sides
- Each level stores price and size
- Price = $8$ bytes
- Size = $8$ bytes
- Updates every second

Bytes per level:

$$
8+8
=
16
$$

Bytes per snapshot:

$$
1000 \times 10 \times 2 \times 16
=
320,000
$$

$$
=
320KB
$$

Per day:

$$
320KB \times 86,400
\approx
27.6GB
$$

With compression, maybe much lower.

---

# Example: Estimate PnL from Alpha

Portfolio:

$$
50M
$$

Expected daily alpha:

$$
3
\text{ bps}
$$

Daily expected PnL:

$$
50M \times 0.0003
=
15,000
$$

Annual expected PnL:

$$
15,000 \times 252
=
3.78M
$$

---

# Example: Estimate Daily Risk

Portfolio:

$$
100M
$$

Annual volatility:

$$
16\%
$$

Daily volatility:

$$
\frac{16\%}{16}
=
1\%
$$

Daily dollar risk:

$$
100M \times 1\%
=
1M
$$

So a typical one-standard-deviation daily move is about:

$$
\$1M
$$

---

# Example: Estimate Sharpe from Daily Returns

Average daily return:

$$
5
\text{ bps}
=
0.0005
$$

Daily volatility:

$$
1\%
=
0.01
$$

Daily Sharpe:

$$
\frac{0.0005}{0.01}
=
0.05
$$

Annual Sharpe:

$$
0.05 \times \sqrt{252}
\approx
0.05 \times 16
=
0.8
$$

---

# Example: Estimate Hit Rate Needed

A strategy wins:

$$
\$1
$$

when right.

Loses:

$$
\$1
$$

when wrong.

Break-even:

$$
p=50\%
$$

If transaction cost is:

$$
0.02
$$

per trade, payoff becomes:

Win:

$$
0.98
$$

Lose:

$$
-1.02
$$

Break-even:

$$
p(0.98)+(1-p)(-1.02)=0
$$

$$
0.98p-1.02+1.02p=0
$$

$$
2p=1.02
$$

$$
p=0.51
$$

Need:

$$
51\%
$$

hit rate.

---

# Common System Design Bottlenecks

| Bottleneck | Symptom | Fix |
|---|---|---|
| CPU | High compute time | Parallelize, optimize algorithms |
| Memory | Slow due to paging | Reduce working set |
| Disk I/O | Slow reads/writes | Batch, cache, compress |
| Network | High transfer time | Co-locate, compress, reduce payload |
| Database | Slow queries | Index, partition, cache |
| Queue | Consumer lag | Add consumers, shard topics |

---

# Common Quant System Components

| Component | Purpose |
|---|---|
| Market Data Feed | Ingest ticks, bars, quotes |
| Feature Pipeline | Compute signals |
| Research Store | Historical data |
| Backtester | Simulate strategy |
| Risk Engine | Compute exposures |
| Optimizer | Choose positions |
| OMS | Manage orders |
| EMS | Execute orders |
| TCA | Analyze execution quality |
| Monitoring | Detect failures |

---

# Python Snippets

## Percent and Basis Points

```python
bps = 5
decimal_return = bps / 10_000

portfolio = 100_000_000
pnl = portfolio * decimal_return

print(pnl)
```

---

## Annualize Volatility

```python
import numpy as np

daily_vol = 0.01
annual_vol = daily_vol * np.sqrt(252)

print(annual_vol)
```

---

## Estimate Storage

```python
events_per_day = 100_000_000
bytes_per_event = 1_000
replication_factor = 3

daily_storage_gb = events_per_day * bytes_per_event / 1e9
replicated_storage_gb = daily_storage_gb * replication_factor

print(daily_storage_gb)
print(replicated_storage_gb)
```

---

## Estimate QPS

```python
daily_users = 10_000_000
requests_per_user = 20
seconds_per_day = 86_400

avg_qps = daily_users * requests_per_user / seconds_per_day
peak_qps = 5 * avg_qps

print(avg_qps)
print(peak_qps)
```

---

# Practice Problems

## Problem 1: Basis Point PnL

A portfolio has value:

$$
250M
$$

It earns:

$$
4
\text{ bps}
$$

in one day.

Find PnL.

### Solution

Convert bps:

$$
4\text{ bps}
=
0.0004
$$

PnL:

$$
250M \times 0.0004
=
100,000
$$

Answer:

$$
\$100,000
$$

---

## Problem 2: Daily Volatility

A stock has annual volatility:

$$
32\%
$$

Estimate daily volatility.

### Solution

Use:

$$
\sqrt{252}
\approx
16
$$

$$
\sigma_{daily}
=
\frac{32\%}{16}
=
2\%
$$

---

## Problem 3: Expected Value

A trade wins:

$$
\$200
$$

with probability:

$$
40\%
$$

and loses:

$$
\$100
$$

with probability:

$$
60\%
$$

Find expected value.

### Solution

$$
E[X]
=
0.4(200)+0.6(-100)
$$

$$
=
80-60
=
20
$$

Answer:

$$
\$20
$$

---

## Problem 4: System Storage

A system receives:

$$
50M
$$

events per day.

Each event is:

$$
2KB
$$

Estimate daily storage.

### Solution

$$
50M \times 2KB
=
100MKB
$$

Since:

$$
1MKB
\approx
1GB
$$

Daily storage:

$$
100GB
$$

---

## Problem 5: QPS Estimate

An app has:

$$
5M
$$

daily users.

Each makes:

$$
40
$$

requests per day.

Estimate average QPS.

### Solution

Total requests:

$$
5M \times 40
=
200M
$$

Seconds per day:

$$
86,400
\approx
100,000
$$

Average QPS:

$$
\frac{200M}{100,000}
=
2000
$$

---

# Common Mistakes

## Mistake 1: Using Too Much Precision

Bad:

$$
86,400
$$

seconds every time.

Usually better:

$$
100,000
$$

seconds per day for rough estimates.

---

## Mistake 2: Forgetting Basis Point Conversion

Wrong:

$$
5\text{ bps}=5\%
$$

Correct:

$$
5\text{ bps}=0.05\%=0.0005
$$

---

## Mistake 3: Confusing Daily and Annual Volatility

Annual volatility does not divide by:

$$
252
$$

It divides by:

$$
\sqrt{252}
$$

---

## Mistake 4: Ignoring Peak Load

Average QPS is not enough.

Always estimate:

$$
\text{Peak QPS}
\approx
3\times
\text{ to }
10\times
\text{ average QPS}
$$

---

## Mistake 5: Ignoring Trading Costs

A strategy with small alpha can be destroyed by:

- Spread
- Fees
- Slippage
- Market impact

---

# Key Takeaways

- Mental math is about fast, reasonable estimates, not exact arithmetic.
- Quant interviews heavily test expected value, probability, volatility scaling, and basis point arithmetic.
- Quant finance napkin math often revolves around PnL, risk, Sharpe, costs, leverage, and capacity.
- System design napkin math usually revolves around QPS, storage, bandwidth, latency, and bottlenecks.
- Always track units carefully.
- Always state assumptions before calculating.
- Always sanity check the final number.
- Good estimates are simple, transparent, and directionally correct.
# Expected Value Thinking in Quant Finance

## Quick Reference Table

| Concept | Definition | Key Formula / Idea |
|---|---|---|
| Expected Value | Probability-weighted average outcome | $E[X]=\sum_i p_i x_i$ |
| Positive EV | Long-run average outcome is profitable | $E[X]>0$ |
| Negative EV | Long-run average outcome loses money | $E[X]<0$ |
| Edge | Expected advantage over fair value | $\text{Edge}=E[\text{PnL}]$ |
| Fair Game | Expected value equals zero | $E[X]=0$ |
| Risk-Adjusted EV | EV adjusted for variance, drawdowns, or capital usage | EV alone is not enough |
| Break-Even Probability | Win probability needed for zero EV | $p^\star=\frac{L}{W+L}$ |
| Sharpe Ratio | Return per unit volatility | $SR=\frac{\mu-r_f}{\sigma}$ |
| Kelly Criterion | Growth-optimal bet sizing | $f^\star=\frac{bp-q}{b}$ |
| Transaction Costs | Fees, spread, slippage, impact | Net EV = Gross EV − Costs |
| Base Rate | Prior probability before new evidence | Critical for Bayesian thinking |
| Optionality | Asymmetric payoff exposure | Limited downside, large upside |
| Capacity | Max size before costs destroy edge | Bigger is not always better |
| Model EV | Expected value implied by a model | Must be validated out-of-sample |

---

# Big Picture

Expected value thinking means judging decisions by their average long-run outcome, not by whether a single trade wins or loses.

In quant finance, this is central because every decision is uncertain.

A good strategy can lose money today.

A bad strategy can make money today.

The key question is:

$$
E[\text{PnL}] > 0?
$$

after accounting for:

- Risk
- Costs
- Slippage
- Market impact
- Capacity
- Model error
- Tail risk

---

# Core Intuition

A single outcome does not prove whether a decision was good.

Example:

A trade has:

$$
60\%
$$

chance to win:

$$
100
$$

and:

$$
40\%
$$

chance to lose:

$$
100
$$

Expected value:

$$
E[X]
=
0.6(100)+0.4(-100)
$$

$$
=
60-40
=
20
$$

This is a positive EV trade.

But it still loses:

$$
40\%
$$

of the time.

A loss does not mean the trade was bad.

---

# Expected Value Formula

For discrete outcomes:

$$
E[X]
=
\sum_i p_i x_i
$$

where:

- $p_i$ = probability of outcome $i$
- $x_i$ = payoff of outcome $i$

For continuous outcomes:

$$
E[X]
=
\int_{-\infty}^{\infty}
x f(x)\,dx
$$

---

# EV in Trading

For a trade:

$$
E[\text{PnL}]
=
P(\text{Win})(\text{Avg Win})
-
P(\text{Loss})(\text{Avg Loss})
$$

If there are costs:

$$
E[\text{Net PnL}]
=
E[\text{Gross PnL}]
-
\text{Costs}
$$

---

# Example: Simple Trade EV

A strategy wins:

$$
55\%
$$

of the time.

Average win:

$$
\$100
$$

Average loss:

$$
\$100
$$

Expected PnL:

$$
E[\text{PnL}]
=
0.55(100)
-
0.45(100)
$$

$$
=
55-45
=
10
$$

Answer:

$$
\$10
$$

per trade.

---

# Why EV Alone Is Not Enough

Two strategies can have the same EV but very different risk.

| Strategy | Expected PnL | Risk |
|---|---:|---:|
| A | $10$ | Low |
| B | $10$ | Very high |

In quant finance, we care about:

$$
\text{Expected Return}
$$

and

$$
\text{Uncertainty Around Expected Return}
$$

---

# Risk-Adjusted Expected Value

A simple objective:

$$
\max
\left(
E[R]
-
\lambda Var(R)
\right)
$$

where:

- $E[R]$ = expected return
- $Var(R)$ = risk
- $\lambda$ = risk aversion

This is the intuition behind mean-variance optimization.

---

# Expected Value vs Realized Outcome

Expected value is about the distribution before the outcome.

Realized outcome is one draw from that distribution.

Example:

You flip a fair coin.

Win:

$$
\$2
$$

Lose:

$$
\$1
$$

EV:

$$
0.5(2)+0.5(-1)
=
0.5
$$

Even if you lose the first flip, the bet was still positive EV.

---

# The Law of Large Numbers

Over many independent repetitions:

$$
\bar X_n
\rightarrow
E[X]
$$

as:

$$
n \rightarrow \infty
$$

This is why positive EV matters.

But in finance, repetitions are often not perfectly independent.

Markets change.

Edges decay.

Tail events cluster.

---

# Key Quant Finance EV Mindset

A model should not ask:

> Will this trade win?

It should ask:

> Is the expected payoff positive after costs and risk?

Better question:

$$
E[\text{PnL} \mid \text{Signal}]
-
\text{Cost}
>
0
$$

---

# Signal-Based Expected Value

Suppose a model outputs signal:

$$
s_t
$$

A simple interpretation:

$$
E[r_{t+1}\mid s_t]
=
\alpha s_t
$$

If:

$$
s_t>0
$$

expected return is positive.

If:

$$
s_t<0
$$

expected return is negative.

But the realized return may still go either way.

---

# Model Design Principle

A model should estimate conditional expectation:

$$
E[Y\mid X]
$$

not just classify outcomes.

Examples:

| Problem | Better Target |
|---|---|
| Will stock go up? | Expected return |
| Will trade fill? | Expected fill value |
| Will borrower default? | Expected loss |
| Will option be profitable? | Expected payoff |
| Is signal positive? | Conditional alpha after costs |

---

# Expected Value After Costs

Many strategies look good before costs.

Net EV:

$$
E[\text{Net PnL}]
=
E[\text{Gross PnL}]
-
\text{Spread}
-
\text{Fees}
-
\text{Slippage}
-
\text{Impact}
$$

A small edge can disappear quickly.

---

# Example: Costs Destroying Edge

Gross expected alpha:

$$
4
\text{ bps}
$$

Costs:

$$
1
\text{ bp spread}
+
1
\text{ bp fees}
+
3
\text{ bps impact}
$$

Net expected alpha:

$$
4-1-1-3
=
-1
\text{ bp}
$$

The strategy is negative EV after costs.

---

# EV and Market Making

Market maker expected profit:

$$
E[\text{Profit}]
=
\text{Spread Capture}
-
\text{Adverse Selection}
-
\text{Inventory Cost}
-
\text{Fees}
$$

Market making is profitable only if:

$$
E[\text{Profit}]>0
$$

---

# EV and Statistical Arbitrage

A stat arb strategy may estimate:

$$
E[r_{i,t+1}\mid X_t]
$$

Then build positions:

$$
w_i
\propto
\frac{E[r_{i,t+1}\mid X_t]}{\sigma_i^2}
$$

This means larger positions go to assets with:

- Higher expected return
- Lower risk
- Lower cost

---

# EV and Options

Options have nonlinear payoffs.

Expected option value:

$$
V
=
e^{-rT}
E^Q[\text{Payoff}]
$$

For a call:

$$
C
=
e^{-rT}
E^Q[\max(S_T-K,0)]
$$

Important distinction:

- Real-world expectation $E^P$ is used for trading edge.
- Risk-neutral expectation $E^Q$ is used for pricing.

---

# EV and Risk-Neutral Pricing

In derivatives pricing, fair price is discounted expected payoff under the risk-neutral measure:

$$
\text{Price}
=
e^{-rT}
E^Q[\text{Payoff}]
$$

This does not mean the real-world expected return is zero.

It means arbitrage-free pricing uses risk-neutral probabilities.

---

# EV and Credit Risk

Expected credit loss:

$$
EL
=
PD \times LGD \times EAD
$$

where:

- $PD$ = probability of default
- $LGD$ = loss given default
- $EAD$ = exposure at default

This is expected value thinking applied to credit.

---

# EV and Portfolio Construction

Portfolio expected return:

$$
E[R_p]
=
w^T \mu
$$

Portfolio variance:

$$
Var(R_p)
=
w^T \Sigma w
$$

A common objective:

$$
\max_w
\left(
w^T\mu
-
\lambda w^T\Sigma w
\right)
$$

---

# EV and System Design

Expected value thinking also applies to systems.

Example:

Should we optimize a pipeline?

Decision should compare:

$$
\text{Expected Benefit}
-
\text{Engineering Cost}
$$

If a data pipeline failure costs:

$$
\$50,000
$$

and happens with probability:

$$
5\%
$$

per month, expected monthly loss is:

$$
0.05 \times 50,000
=
2,500
$$

If monitoring costs:

$$
\$500
$$

per month, it is positive EV.

---

# Expected Value in Model Infrastructure

When designing quant systems, ask:

- What failure modes exist?
- How often do they happen?
- What is the cost if they happen?
- What does prevention cost?
- What is the expected loss reduction?

Expected benefit:

$$
\Delta E[\text{Loss}]
=
P_0L_0
-
P_1L_1
$$

---

# Base Rates

A base rate is the prior probability of an event.

Ignoring base rates is one of the most common mistakes.

Example:

If only:

$$
1\%
$$

of signals are truly useful,

then even a model with a good hit rate may produce many false positives.

---

# Bayesian EV Thinking

A useful decision framework:

$$
E[\text{Payoff}\mid \text{Evidence}]
$$

Use evidence to update probabilities.

Then compute expected value.

---

# Example: Conditional Trade EV

Suppose historical data shows:

When signal fires:

- Probability of up move = $55\%$
- Average up move = $20$ bps
- Average down move = $15$ bps
- Trading cost = $3$ bps

Gross EV:

$$
0.55(20)
-
0.45(15)
$$

$$
=
11-6.75
=
4.25
\text{ bps}
$$

Net EV:

$$
4.25-3
=
1.25
\text{ bps}
$$

This is positive EV, but small.

---

# Break-Even Probability

If win amount is $W$ and loss amount is $L$, break-even probability is:

$$
p^\star
=
\frac{L}{W+L}
$$

---

# Example

Win:

$$
3
$$

Lose:

$$
1
$$

Break-even probability:

$$
p^\star
=
\frac{1}{3+1}
=
25\%
$$

You only need to win more than:

$$
25\%
$$

of the time.

---

# Key Interview Pattern

If payoff is asymmetric, win rate alone is meaningless.

A strategy with:

$$
30\%
$$

win rate can be good if wins are large.

A strategy with:

$$
90\%
$$

win rate can be bad if losses are huge.

---

# Kelly Criterion

Kelly sizing maximizes long-run geometric growth.

For a bet that wins $b$ per $1$ lost:

$$
f^\star
=
\frac{bp-q}{b}
$$

where:

$$
q=1-p
$$

For even-money bets:

$$
f^\star
=
2p-1
$$

---

# Kelly Example

Win probability:

$$
55\%
$$

Even-money payoff.

Kelly fraction:

$$
f^\star
=
2(0.55)-1
=
0.10
$$

Bet:

$$
10\%
$$

of capital.

In practice, many traders use fractional Kelly because estimates are noisy.

---

# Why Fractional Kelly Is Common

Kelly is sensitive to estimation error.

If estimated edge is too high, Kelly overbets.

Safer sizing:

$$
f
=
c f^\star
$$

where:

$$
0<c<1
$$

Common choices:

- Half Kelly
- Quarter Kelly

---

# EV and Tail Risk

Positive average EV may hide rare catastrophic losses.

Example:

| Outcome | Probability | Payoff |
|---|---:|---:|
| Normal profit | $99\%$ | $+1$ |
| Crash loss | $1\%$ | $-200$ |

EV:

$$
0.99(1)+0.01(-200)
=
0.99-2
=
-1.01
$$

Looks profitable most days but is negative EV.

---

# Tail Risk Lesson

Strategies that win frequently can still be bad.

Examples:

- Short volatility
- Selling deep OTM options
- Leveraged carry trades
- Illiquid credit strategies

Always ask:

$$
E[\text{Loss from Tail Events}]
$$

---

# EV and Capacity

Expected value often declines with size.

A simple model:

$$
\text{Net Alpha}(Q)
=
\alpha
-
c
\sqrt{\frac{Q}{V}}
$$

where:

- $Q$ = trade size
- $V$ = market volume
- $c$ = impact coefficient

Capacity is reached when:

$$
\text{Net Alpha}(Q)
\leq 0
$$

---

# EV and Exploration

Sometimes negative short-term EV actions are positive long-term EV because they produce information.

Example:

Testing a small trade may lose money now but reveal whether a larger strategy works.

Total EV:

$$
E[\text{Total Value}]
=
E[\text{Trading PnL}]
+
E[\text{Information Value}]
$$

---

# Expected Value Checklist for Quant Strategy Design

Before trading a strategy, ask:

1. What is the expected gross return?
2. What are the expected costs?
3. What is the uncertainty around the estimate?
4. Is the edge statistically significant?
5. Does the edge survive out-of-sample?
6. Does the edge survive transaction costs?
7. Does the edge decay with capital?
8. Is there hidden tail risk?
9. Is the strategy crowded?
10. Can the system execute it reliably?

---

# Expected Value Checklist for Quant Systems

Before building infrastructure, ask:

1. What risk does this reduce?
2. How often does that risk happen?
3. What is the loss if it happens?
4. What is the engineering cost?
5. What is the operational burden?
6. Does this improve latency, reliability, correctness, or research speed?
7. Is the improvement measurable?

---

# Practice Problems

## Problem 1: Basic Expected Value

A game pays:

- $+\$100$ with probability $40\%$
- $-\$50$ with probability $60\%$

Find expected value.

### Solution

$$
E[X]
=
0.40(100)+0.60(-50)
$$

$$
=
40-30
=
10
$$

Answer:

$$
\$10
$$

The game is positive EV.

---

## Problem 2: Break-Even Win Rate

A trade makes:

$$
\$300
$$

when correct and loses:

$$
\$100
$$

when wrong.

Find break-even win probability.

### Solution

Use:

$$
p^\star
=
\frac{L}{W+L}
$$

$$
p^\star
=
\frac{100}{300+100}
=
\frac{100}{400}
=
0.25
$$

Answer:

$$
25\%
$$

The trade only needs to be right more than $25\%$ of the time.

---

## Problem 3: EV After Costs

A strategy has:

- Win probability = $52\%$
- Average win = $10$ bps
- Average loss = $10$ bps
- Cost per trade = $1$ bp

Find net EV.

### Solution

Gross EV:

$$
0.52(10)-0.48(10)
$$

$$
=
5.2-4.8
=
0.4
\text{ bps}
$$

Net EV:

$$
0.4-1
=
-0.6
\text{ bps}
$$

Answer:

Negative EV after costs.

---

## Problem 4: Market Making EV

A market maker expects:

- Spread capture = $2$ ticks
- Adverse selection loss = $1.2$ ticks
- Inventory cost = $0.4$ ticks
- Fees = $0.3$ ticks

Find expected profit.

### Solution

$$
E[\text{Profit}]
=
2-1.2-0.4-0.3
$$

$$
=
0.1
\text{ ticks}
$$

Answer:

Positive but very small EV.

---

## Problem 5: Credit Expected Loss

A loan has:

$$
PD=3\%
$$

$$
LGD=60\%
$$

$$
EAD=\$10,000,000
$$

Find expected loss.

### Solution

$$
EL
=
PD \times LGD \times EAD
$$

$$
=
0.03 \times 0.60 \times 10,000,000
$$

$$
=
180,000
$$

Answer:

$$
\$180,000
$$

---

## Problem 6: Options Expected Payoff

A call option expires with these possible stock prices:

| $S_T$ | Probability |
|---:|---:|
| $80$ | $30\%$ |
| $100$ | $40\%$ |
| $130$ | $30\%$ |

Strike:

$$
K=100
$$

Risk-free discounting ignored.

Find expected payoff.

### Solution

Call payoff:

$$
\max(S_T-K,0)
$$

For each state:

$$
S_T=80
\Rightarrow
0
$$

$$
S_T=100
\Rightarrow
0
$$

$$
S_T=130
\Rightarrow
30
$$

Expected payoff:

$$
0.30(0)+0.40(0)+0.30(30)
$$

$$
=
9
$$

Answer:

$$
9
$$

---

## Problem 7: Tail Risk EV

A strategy earns:

$$
+\$1,000
$$

with probability:

$$
99.5\%
$$

and loses:

$$
-\$500,000
$$

with probability:

$$
0.5\%
$$

Find EV.

### Solution

$$
E[X]
=
0.995(1000)+0.005(-500000)
$$

$$
=
995-2500
=
-1505
$$

Answer:

$$
-\$1,505
$$

The strategy usually wins but is negative EV.

---

## Problem 8: Model-Based Trade Decision

A model predicts expected return:

$$
6
\text{ bps}
$$

Expected cost:

$$
2
\text{ bps}
$$

Expected impact:

$$
3
\text{ bps}
$$

Should you trade?

### Solution

Net EV:

$$
6-2-3
=
1
\text{ bp}
$$

Answer:

Positive EV, but small.

In practice, you would also check:

- Forecast uncertainty
- Liquidity
- Risk
- Capacity
- Correlation with existing portfolio

---

## Problem 9: Kelly Sizing

A strategy wins with probability:

$$
60\%
$$

It wins:

$$
\$1
$$

for every:

$$
\$1
$$

risked.

Find full Kelly fraction.

### Solution

Even-money Kelly:

$$
f^\star
=
2p-1
$$

$$
=
2(0.60)-1
=
0.20
$$

Answer:

$$
20\%
$$

In real trading, this is usually too aggressive due to estimation error.

---

## Problem 10: Quant System Reliability EV

A data bug has:

$$
2\%
$$

monthly probability.

If it happens, expected loss is:

$$
\$200,000
$$

A validation system costs:

$$
\$1,500
$$

per month and reduces bug probability to:

$$
0.5\%
$$

Is it positive EV?

### Solution

Current expected monthly loss:

$$
0.02(200000)
=
4000
$$

New expected monthly loss:

$$
0.005(200000)
=
1000
$$

Loss reduction:

$$
4000-1000
=
3000
$$

Net benefit after system cost:

$$
3000-1500
=
1500
$$

Answer:

Positive EV by:

$$
\$1,500
$$

per month.

---

# Common Quant Interview Patterns

## Pattern 1: Fair Price

If a game has random payoff $X$, fair price is:

$$
E[X]
$$

If price is below expected payoff, buyer has positive EV.

---

## Pattern 2: Optionality

Options and choices have value because you can choose favorable outcomes.

Payoff often looks like:

$$
\max(X,0)
$$

---

## Pattern 3: Base Rate Adjustment

Do not ignore priors.

A very accurate signal can still be misleading when the base rate is low.

---

## Pattern 4: Asymmetric Payoffs

Always compute:

$$
pW-(1-p)L
$$

not just win probability.

---

## Pattern 5: Repeated Bets

Positive EV matters most when you can repeat bets many times with controlled risk.

But repeated overbetting can still lead to ruin.

---

# Common Mistakes

## Mistake 1: Confusing Good Outcome with Good Decision

A lucky win does not prove the decision was good.

An unlucky loss does not prove the decision was bad.

---

## Mistake 2: Ignoring Costs

In quant finance:

$$
\text{Small Gross Edge}
-
\text{Costs}
=
\text{No Edge}
$$

---

## Mistake 3: Ignoring Tail Risk

High win-rate strategies can have negative EV.

---

## Mistake 4: Ignoring Estimation Error

Estimated EV is not true EV.

You observe:

$$
\widehat{EV}
=
EV
+
\text{Noise}
$$

---

## Mistake 5: Maximizing EV Without Risk Limits

A positive EV bet can still be too large.

Risk controls matter.

---

## Mistake 6: Assuming Independence

Repeated trades may be correlated.

Correlation increases drawdown risk.

---

# Practical Rules of Thumb

- Always compute EV after costs.
- Always ask how uncertain the EV estimate is.
- Prefer many small independent positive EV bets over one large concentrated bet.
- A strategy with small EV and huge tail risk is dangerous.
- A model should estimate magnitude, not just direction.
- A backtest should be treated as an estimate, not proof.
- Capacity matters because impact grows with size.
- Positive EV must survive production constraints.

---

# Python Snippets

## Basic Expected Value

```python
import numpy as np

payoffs = np.array([100, -50])
probs = np.array([0.4, 0.6])

ev = np.sum(probs * payoffs)
print(ev)
```

---

## EV After Costs

```python
win_prob = 0.52
avg_win_bps = 10
avg_loss_bps = 10
cost_bps = 1

gross_ev = win_prob * avg_win_bps - (1 - win_prob) * avg_loss_bps
net_ev = gross_ev - cost_bps

print(gross_ev)
print(net_ev)
```

---

## Break-Even Probability

```python
win = 300
loss = 100

p_break_even = loss / (win + loss)
print(p_break_even)
```

---

## Credit Expected Loss

```python
PD = 0.03
LGD = 0.60
EAD = 10_000_000

expected_loss = PD * LGD * EAD
print(expected_loss)
```

---

## Kelly Fraction

```python
p = 0.55
b = 1
q = 1 - p

kelly = (b * p - q) / b
half_kelly = 0.5 * kelly

print(kelly)
print(half_kelly)
```

---

# Key Takeaways

- Expected value is the foundation of rational decision-making under uncertainty.
- In quant finance, every trade, model, system, and strategy should be evaluated by expected net benefit.
- Positive EV does not guarantee short-term profit.
- Negative EV can still produce lucky wins.
- Costs, slippage, impact, and tail risk are essential parts of true EV.
- Good models estimate conditional expected value, not just direction.
- Risk-adjusted EV matters more than raw EV.
- Sizing matters because overbetting a positive EV strategy can still cause ruin.
- Quant interview problems often test whether you can identify payoff, probability, costs, and break-even conditions quickly.
- The best quant mindset is probabilistic: judge decisions by process and expected value, not by individual outcomes.
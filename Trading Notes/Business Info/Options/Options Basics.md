# Options Contracts: Theory, Pricing, Greeks, Volatility, Strategies, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Formula / Intuition |
|----------|------------|---------------------|
| Option | Right, but not obligation, to buy or sell an asset | Asymmetric payoff |
| Call Option | Right to buy | Bullish |
| Put Option | Right to sell | Bearish |
| Strike Price | Agreed transaction price | $K$ |
| Expiration | Last exercise date | $T$ |
| Premium | Price paid for option | Option value |
| Intrinsic Value | Immediate exercise value | Depends on spot vs strike |
| Time Value | Extra value beyond intrinsic | Probability of future payoff |
| ITM | In-the-money | Immediate value exists |
| ATM | At-the-money | Spot ≈ Strike |
| OTM | Out-of-the-money | No intrinsic value |
| Delta | Sensitivity to underlying | $\frac{\partial V}{\partial S}$ |
| Gamma | Sensitivity of Delta | $\frac{\partial^2 V}{\partial S^2}$ |
| Vega | Sensitivity to volatility | $\frac{\partial V}{\partial \sigma}$ |
| Theta | Sensitivity to time | $\frac{\partial V}{\partial t}$ |
| Rho | Sensitivity to interest rates | $\frac{\partial V}{\partial r}$ |
| Implied Volatility | Market-implied volatility | Derived from option price |
| Realized Volatility | Actual observed volatility | Historical estimate |
| Black-Scholes | Classic option pricing model | European options |
| Vol Surface | IV by strike and expiry | Market structure |
| Vol Smile | Non-flat IV across strikes | Common in equity options |

---

# Intuition

Options are fundamentally different from stocks.

A stock gives ownership.

An option gives a choice.

The holder has a right:

but not an obligation.

This asymmetry is why options have nonlinear payoffs.

---

# Call Option

A call gives the right to buy.

Example:

Current stock price:

$$
S = 100
$$

Call strike:

$$
K = 110
$$

If stock rises to:

$$
130
$$

holder can buy at:

$$
110
$$

and immediately own something worth:

$$
130
$$

---

# Put Option

A put gives the right to sell.

Example:

Stock:

$$
100
$$

Put strike:

$$
90
$$

Stock falls to:

$$
70
$$

Holder can sell for:

$$
90
$$

something worth:

$$
70
$$

---

# Long Call Payoff

At expiration:

$$
\max(S_T-K,0)
$$

---

# Long Put Payoff

At expiration:

$$
\max(K-S_T,0)
$$

---

# Short Call Payoff

Opposite of long call:

$$
-\max(S_T-K,0)
$$

---

# Short Put Payoff

Opposite of long put:

$$
-\max(K-S_T,0)
$$

---

# Moneyness

## In-The-Money (ITM)

Call:

$$
S>K
$$

Put:

$$
S<K
$$

---

## At-The-Money (ATM)

$$
S \approx K
$$

---

## Out-Of-The-Money (OTM)

Call:

$$
S<K
$$

Put:

$$
S>K
$$

---

# Intrinsic Value

Call:

$$
\max(S-K,0)
$$

Put:

$$
\max(K-S,0)
$$

---

# Time Value

Option Price:

$$
=
\text{Intrinsic Value}
+
\text{Time Value}
$$

---

# Why Time Value Exists

Even if currently worthless:

future movements may create value.

Example:

Stock:

$$
100
$$

Call strike:

$$
120
$$

Expires:

6 months.

Even though OTM today:

there is still a chance stock exceeds:

$$
120
$$

before expiration.

---

# Option Pricing Variables

Option value depends on:

| Variable | Symbol |
|-----------|---------|
| Stock Price | $S$ |
| Strike | $K$ |
| Time | $T$ |
| Volatility | $\sigma$ |
| Interest Rate | $r$ |
| Dividends | $q$ |

---

# Volatility

The most important option concept.

Options trade volatility more than direction.

Higher volatility:

$$
\Rightarrow
$$

higher option value.

Reason:

More opportunity for large favorable moves.

---

# Realized Volatility

Historical volatility estimate.

Example:

Daily returns:

$$
r_t
$$

Annualized volatility:

$$
\sigma
=
\sqrt{252}
\cdot
\text{std}(r_t)
$$

---

# Implied Volatility (IV)

Volatility implied by market price.

Not directly observed.

Must be solved numerically.

---

# Black-Scholes Model

Foundational option pricing model.

Assumptions:

- Lognormal prices
- Constant volatility
- Continuous trading
- No arbitrage

---

# Black-Scholes Inputs

$$
S
$$

Current price

$$
K
$$

Strike

$$
T
$$

Time to expiration

$$
r
$$

Risk-free rate

$$
\sigma
$$

Volatility

---

# Black-Scholes Outputs

Call value

Put value

Greeks

---

# Put-Call Parity

Very important relationship:

$$
C-P
=
S
-
Ke^{-rT}
$$

Used to identify arbitrage opportunities.

---

# The Greeks

Greeks measure sensitivity.

Think:

> "How does option value change when inputs move?"

---

# Delta

Sensitivity to stock price.

$$
\Delta
=
\frac{\partial V}{\partial S}
$$

Approximation:

$$
\Delta
=
0.60
$$

means:

Stock rises:

$$
\$1
$$

Option rises roughly:

$$
\$0.60
$$

---

# Delta Intuition

| Position | Delta |
|-----------|---------|
| Long Stock | +1 |
| Long Call | Positive |
| Long Put | Negative |
| Short Call | Negative |
| Short Put | Positive |

---

# Gamma

Rate of change of Delta.

$$
\Gamma
=
\frac{\partial^2 V}{\partial S^2}
$$

Measures curvature.

Highest near:

ATM options.

---

# Gamma Intuition

Delta changes.

Gamma measures:

how fast Delta changes.

---

# Vega

Sensitivity to volatility.

$$
\text{Vega}
=
\frac{\partial V}{\partial \sigma}
$$

Long options:

Positive Vega.

---

# Vega Intuition

Volatility rises:

Options generally become more valuable.

---

# Theta

Time decay.

$$
\Theta
=
\frac{\partial V}{\partial t}
$$

Usually negative for option buyers.

---

# Theta Intuition

Time passes.

Opportunities disappear.

Option value decays.

---

# Rho

Interest rate sensitivity.

$$
\rho
=
\frac{\partial V}{\partial r}
$$

Important for long-dated options.

---

# Volatility Surface

Implied volatility varies by:

- Strike
- Expiration

Visualization:

$$
IV(K,T)
$$

---

# Volatility Smile

Observed market phenomenon.

Deep OTM puts often have higher IV.

Especially in equities.

---

# Volatility Skew

Equity markets typically exhibit:

Downside puts:

Higher IV

Upside calls:

Lower IV

---

# American vs European Options

## European

Exercise only at expiration.

Used in Black-Scholes.

---

## American

Exercise anytime before expiration.

Common for equities.

---

# Option Strategies

---

# Covered Call

Long stock.

Short call.

Structure:

$$
+100
\text{ shares}
$$

$$
-1
\text{ call}
$$

---

## When Used

Moderately bullish.

Generate income.

---

## Payoff

Upside capped.

Downside remains.

---

# Protective Put

Long stock.

Long put.

---

## When Used

Bullish but want downside protection.

Equivalent to insurance.

---

# Bull Call Spread

Long lower strike call.

Short higher strike call.

Example:

$$
+100C
$$

$$
-120C
$$

---

## When Used

Moderately bullish.

Reduce premium cost.

---

## Payoff

Limited gain.

Limited loss.

---

# Bear Put Spread

Long higher strike put.

Short lower strike put.

Example:

$$
+100P
$$

$$
-80P
$$

---

## When Used

Moderately bearish.

---

# Long Straddle

Buy:

$$
1
\text{ ATM Call}
$$

and

$$
1
\text{ ATM Put}
$$

---

## When Used

Expect large move.

Direction unknown.

Common before:

- Earnings
- Economic releases

---

## Payoff Shape

V-shaped.

Profits from:

Large up move.

Large down move.

---

# Long Strangle

Buy:

OTM call

and

OTM put.

---

## When Used

Expect very large move.

Cheaper than straddle.

---

# Iron Condor

Combination:

- Bull Put Spread
- Bear Call Spread

---

## When Used

Expect low volatility.

Range-bound market.

---

## Payoff

Flat profit zone.

Losses outside range.

---

# Butterfly Spread

Three strikes.

Example:

$$
+95C
$$

$$
-2(100C)
$$

$$
+105C
$$

---

## When Used

Expect stock near strike at expiration.

---

# Calendar Spread

Same strike.

Different expirations.

---

Example:

Long:

6-month call.

Short:

1-month call.

---

## When Used

Volatility and time-decay trades.

---

# Box Spread

Synthetic bond.

Constructed from:

- Bull Call Spread
- Bear Put Spread

Used to imply funding rates.

---

# Market Makers

Most option volume comes from:

- Market makers
- Dealers

Goal:

Remain delta-neutral.

Capture spreads.

Manage risk.

---

# Delta Hedging

Adjust stock position to neutralize delta.

Example:

Long option.

Delta:

$$
0.50
$$

Need:

$$
-50
$$

shares per contract.

---

# Gamma Trading

Profit from realized volatility.

Repeatedly:

- Buy low
- Sell high

through delta hedging.

---

# Volatility Trading

Professional desks often trade:

Volatility

not

direction.

---

# Variance Swaps

Trade realized variance directly.

Payoff approximately:

$$
\sigma_{realized}^2
-
\sigma_{strike}^2
$$

---

# Volatility Risk Premium

Historically:

$$
IV
>
RV
$$

Implied volatility tends to exceed realized volatility.

Reason:

Investors pay for protection.

---

# Common Option Data Fields

| Field | Description |
|---------|------------|
| Bid | Best bid |
| Ask | Best ask |
| IV | Implied Volatility |
| Delta | Greek |
| Gamma | Greek |
| Theta | Greek |
| Vega | Greek |
| Open Interest | Outstanding contracts |
| Volume | Contracts traded |

---

# Common Sources of Options Data

## Retail

- Yahoo Finance
- Polygon
- Alpaca

---

## Institutional

- OPRA
- OptionMetrics
- CBOE
- Refinitiv
- Bloomberg

---

## Quant Research

- IvyDB
- ORATS
- Hanweck

---

# Quant Option Factors

Common signals:

- Volatility Risk Premium
- Skew
- Term Structure
- Dealer Positioning
- Gamma Exposure (GEX)
- Open Interest Dynamics

---

# Common Quant Technologies

## Pricing

```python
QuantLib
py_vollib
```

---

## Data

```python
pandas
numpy
polars
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
xgboost
lightgbm
scikit-learn
```

---

# Python Example: Option Payoff

Long Call:

```python
import numpy as np

S = np.linspace(50, 150, 100)

K = 100

payoff = np.maximum(S - K, 0)
```

---

# Python Example: Bull Call Spread

```python
payoff = (
    np.maximum(S - 100, 0)
    -
    np.maximum(S - 120, 0)
)
```

---

# Example Problem 1

Stock:

$$
100
$$

Call strike:

$$
110
$$

Expiration stock price:

$$
130
$$

Find payoff.

### Solution

$$
\max(130-110,0)
$$

$$
=
20
$$

Answer:

$$
\$20
$$

per share.

---

# Example Problem 2

Stock:

$$
100
$$

Put strike:

$$
90
$$

Expiration stock price:

$$
70
$$

Find payoff.

### Solution

$$
\max(90-70,0)
$$

$$
=
20
$$

Answer:

$$
\$20
$$

per share.

---

# Example Problem 3

Long Straddle:

Call premium:

$$
5
$$

Put premium:

$$
4
$$

Total cost:

$$
9
$$

ATM strike:

$$
100
$$

Find break-even points.

### Solution

Upper:

$$
100+9
=
109
$$

Lower:

$$
100-9
=
91
$$

Answer:

$$
91
\quad\text{and}\quad
109
$$

---

# Example Problem 4

Bull Call Spread

Long:

$$
100C
$$

Short:

$$
120C
$$

Net premium:

$$
6
$$

Maximum profit:

$$
20-6
$$

$$
=
14
$$

Maximum loss:

$$
6
$$

---

# Common Beginner Mistakes

## Confusing Delta with Probability

Delta is not exactly probability.

Though ATM deltas are often used as rough approximations.

---

## Ignoring Implied Volatility

Options can lose money even when direction is correct.

Example:

IV collapse after earnings.

---

## Buying Far OTM Options

Cheap options often expire worthless.

---

## Ignoring Theta

Long options lose value every day.

---

## Assuming Black-Scholes Is Perfect

Real markets have:

- Jumps
- Stochastic volatility
- Transaction costs

---

## Trading Direction Instead of Volatility

Professionals often care more about:

$$
IV
-
RV
$$

than:

$$
S_T
$$

---

# Key Takeaways

- Options provide nonlinear exposure and asymmetric risk/reward.
- Calls benefit from rising prices; puts benefit from falling prices.
- Option value depends heavily on volatility and time.
- The Greeks measure sensitivity to key risk factors.
- Implied volatility is one of the most important variables in options markets.
- Most professional options trading is volatility trading rather than directional trading.
- Common strategies include spreads, straddles, strangles, iron condors, butterflies, and calendars.
- Delta hedging and volatility management are central to market-making and institutional options trading.
- Quantitative options research focuses heavily on volatility surfaces, skew, term structure, dealer positioning, and volatility risk premia.
- Understanding payoff diagrams, Greeks, and volatility is far more important than memorizing individual option strategies.
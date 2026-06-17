# Fixed Income: Bonds, Rates Products, Credit Instruments, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Key Formula |
|----------|------------|-------------|
| Fixed Income | Securities with contractual cash flows | — |
| Bond | Debt instrument paying coupons and principal | PV of future cash flows |
| Yield | Annualized return implied by price | Solved from bond price |
| Coupon | Periodic interest payment | Coupon Rate × Face Value |
| Face Value (Par) | Amount repaid at maturity | Usually \$100 or \$1000 |
| Duration | Sensitivity to interest rates | Approximate price change |
| Convexity | Curvature of price-yield relationship | Second-order sensitivity |
| Yield Curve | Interest rates across maturities | Treasury curve |
| Credit Spread | Yield above risk-free rate | Corporate − Treasury Yield |
| Treasury | Government-issued debt | Lowest credit risk |
| Corporate Bond | Company-issued debt | Credit risk present |
| Agency Bond | Government-sponsored debt | Fannie Mae, Freddie Mac |
| MBS | Mortgage-Backed Security | Pool of mortgages |
| ABS | Asset-Backed Security | Pool of loans |
| CDS | Credit Default Swap | Default protection |
| IRS | Interest Rate Swap | Exchange fixed/floating payments |
| DV01 | Dollar value of 1 bp move | Rate sensitivity |
| SOFR | Secured Overnight Financing Rate | Modern USD benchmark |
| OAS | Option-Adjusted Spread | Spread after embedded options |

---

# Intuition

Fixed income is fundamentally about lending money.

You provide capital today.

The borrower promises future payments.

Examples:

- Government borrowing money
- Company issuing debt
- Homeowner taking a mortgage

All fixed-income instruments are variations of this basic idea.

---

# Why Fixed Income Exists

Governments need funding.

Companies need funding.

Consumers need funding.

Instead of issuing stock:

They borrow money.

Investors receive:

- Interest payments
- Principal repayment

---

# Fixed Income vs Equities

| Fixed Income | Equities |
|--------------|----------|
| Debt | Ownership |
| Defined cash flows | Variable cash flows |
| Higher bankruptcy priority | Lower priority |
| Lower volatility | Higher volatility |
| Yield-focused | Growth-focused |

---

# Basic Bond Structure

Suppose:

Face Value:

$$
1000
$$

Coupon Rate:

$$
5\%
$$

Maturity:

$$
10
$$

years.

Annual coupon:

$$
50
=
0.05 \times 1000
$$

Cash flows:

| Year | Payment |
|--------|----------|
| 1 | \$50 |
| 2 | \$50 |
| ... | ... |
| 10 | \$1050 |

---

# Bond Pricing

A bond equals the present value of future cash flows.

$$
P
=
\sum_{t=1}^{n}
\frac{C}{(1+y)^t}
+
\frac{F}{(1+y)^n}
$$

where:

- $P$ = Price
- $C$ = Coupon
- $F$ = Face value
- $y$ = Yield
- $n$ = Maturity

---

# Yield to Maturity (YTM)

The discount rate making:

$$
\text{Present Value}
=
\text{Price}
$$

Think of YTM as:

> "What annual return am I earning if I hold the bond until maturity?"

---

# Bond Price-Yield Relationship

One of the most important concepts in finance.

If yields rise:

$$
\uparrow y
\Rightarrow
\downarrow P
$$

If yields fall:

$$
\downarrow y
\Rightarrow
\uparrow P
$$

Bond prices and yields move inversely.

---

# Treasury Securities

Issued by the U.S. government.

---

## Treasury Bills

Maturity:

$$
<1
$$

year.

No coupons.

Sold at discount.

---

## Treasury Notes

Maturity:

$$
2-10
$$

years.

Coupon-paying.

---

## Treasury Bonds

Maturity:

$$
20-30
$$

years.

Coupon-paying.

---

# Yield Curve

Plots yields across maturities.

Example:

| Maturity | Yield |
|------------|---------|
| 3M | 4.0% |
| 2Y | 4.2% |
| 5Y | 4.5% |
| 10Y | 4.7% |
| 30Y | 5.0% |

---

# Normal Yield Curve

Longer maturities have higher yields.

Usually implies:

- Economic growth
- Inflation expectations

---

# Inverted Yield Curve

Short-term yields exceed long-term yields.

Historically associated with:

- Recessions
- Monetary tightening

---

# Yield Curve Trades

Common macro strategy.

Example:

Expect curve steepening:

Long:

$$
10Y
$$

Short:

$$
2Y
$$

---

# Duration

Measures interest-rate sensitivity.

Approximation:

$$
\frac{\Delta P}{P}
\approx
-D
\Delta y
$$

where:

- $D$ = Duration
- $\Delta y$ = Yield change

---

# Duration Example

Duration:

$$
7
$$

Yield rises:

$$
1\%
$$

Approximate price change:

$$
-7\%
$$

---

# DV01

Dollar Value of One Basis Point.

Measures:

> "How many dollars does the bond gain or lose if yields move by 1 bp?"

---

# Example

DV01:

$$
150
$$

A:

$$
10
\text{ bp}
$$

move produces:

$$
150 \times 10
=
1500
$$

dollars.

---

# Convexity

Duration is only a linear approximation.

Convexity improves accuracy.

Approximation:

$$
\frac{\Delta P}{P}
\approx
-D\Delta y
+
\frac{1}{2}
C(\Delta y)^2
$$

where:

- $C$ = Convexity

---

# Why Convexity Matters

Large rate moves cause:

- Duration approximation errors
- Nonlinear price changes

Long-dated bonds often have high convexity.

---

# Credit Bonds

Corporate bonds introduce:

- Interest rate risk
- Credit risk

Unlike Treasuries:

Borrowers may default.

---

# Credit Spread

Additional yield required for default risk.

Example:

Treasury:

$$
4\%
$$

Corporate:

$$
6\%
$$

Spread:

$$
2\%
$$

or

$$
200
\text{ bps}
$$

---

# Investment Grade vs High Yield

## Investment Grade

Ratings:

- AAA
- AA
- A
- BBB

---

## High Yield

Ratings below:

$$
BBB-
$$

Higher return.

Higher default risk.

---

# Floating Rate Notes (FRNs)

Coupon changes over time.

Example:

$$
SOFR + 150bp
$$

If:

$$
SOFR = 4\%
$$

Coupon:

$$
5.5\%
$$

---

# Mortgage-Backed Securities (MBS)

Pool of mortgages.

Investors receive:

- Principal payments
- Interest payments

---

# Prepayment Risk

Homeowners can refinance.

This creates uncertainty.

Mortgage duration changes over time.

One of the most important MBS concepts.

---

# Asset-Backed Securities (ABS)

Backed by:

- Auto loans
- Credit cards
- Student loans

Cash flows come from borrowers.

---

# Interest Rate Swaps (IRS)

Extremely important fixed-income derivative.

---

# Intuition

Two parties exchange:

Fixed payments

for

Floating payments.

---

# Example

Party A pays:

$$
5\%
$$

fixed.

Party B pays:

$$
SOFR
$$

floating.

No principal exchanged.

Only net cash flows.

---

# Swap Cash Flow

Suppose:

Fixed payment:

$$
500,000
$$

Floating payment:

$$
450,000
$$

Net payment:

$$
50,000
$$

from fixed payer.

---

# Swap Curve

Interest-rate swaps create their own yield curve.

Often used more than Treasuries for pricing.

Modern benchmark:

SOFR swap curve.

---

# Credit Default Swaps (CDS)

Insurance-like credit contract.

Buyer pays premium.

Seller compensates losses after default.

---

# Example

Own:

$$
\$10M
$$

corporate bond.

Purchase CDS protection.

If default occurs:

CDS offsets losses.

---

# CDS Spread

Quoted in basis points.

Example:

$$
150
\text{ bps}
$$

Higher CDS spread:

Higher default risk.

---

# Fixed Income Factors

Many systematic strategies use:

---

## Level

Overall rate movement.

---

## Slope

Difference between long and short yields.

Example:

$$
10Y - 2Y
$$

---

## Curvature

Shape of middle maturities.

Example:

$$
2\times5Y
-
2Y
-
10Y
$$

---

# Principal Component Analysis (PCA)

Very important in rates trading.

Empirically:

Most yield-curve movement explained by:

1. Level
2. Slope
3. Curvature

---

# Rates Trading Instruments

## Cash Bonds

Physical bonds.

---

## Treasury Futures

Most liquid rates product.

Examples:

- ZN (10Y)
- ZB (30Y)
- UB (Ultra Bond)

---

## Interest Rate Swaps

Most institutional rate exposure.

---

## Swaptions

Options on swaps.

---

## Eurodollar Futures (Historical)

Previously dominant short-rate product.

Mostly replaced by:

SOFR Futures.

---

# SOFR

Secured Overnight Financing Rate.

Modern USD benchmark.

Replaced LIBOR.

Used in:

- Swaps
- Loans
- Futures

---

# LIBOR Transition

Historically:

Most fixed-income products referenced LIBOR.

Now replaced by:

SOFR.

Very important operational transition.

---

# Quant Fixed-Income Research Pipeline

$$
\text{Market Data}
\rightarrow
\text{Curve Construction}
\rightarrow
\text{Risk Factors}
\rightarrow
\text{Signals}
\rightarrow
\text{Optimization}
\rightarrow
\text{Execution}
$$

---

# Yield Curve Construction

Bootstrapping builds zero-rate curves.

Inputs:

- Deposits
- Futures
- Swaps

Outputs:

$$
r(t)
$$

for every maturity.

---

# Common Quant Signals

## Carry

Expected return from holding bond.

---

## Roll Down

Gain from moving down yield curve.

---

## Relative Value

Bond appears rich or cheap.

---

## Momentum

Rates continue trending.

---

## Mean Reversion

Rates revert toward equilibrium.

---

# Portfolio Optimization

Objective:

$$
\max
(
\mu^T w
-
\lambda w^T \Sigma w
)
$$

subject to:

- Duration limits
- DV01 limits
- Sector limits
- Credit limits

---

# Common Fixed Income Risk Measures

| Metric | Meaning |
|----------|---------|
| Duration | Rate sensitivity |
| DV01 | Dollar risk per bp |
| Convexity | Curvature sensitivity |
| Spread Duration | Credit sensitivity |
| VaR | Portfolio loss estimate |
| Expected Shortfall | Tail risk measure |

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

## Fixed Income Analytics

```python
QuantLib
rateslib
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

## Machine Learning

```python
xgboost
lightgbm
scikit-learn
```

---

# QuantLib Example

Price a bond:

```python
import QuantLib as ql

# industry-standard fixed income library
```

Common uses:

- Bond pricing
- Yield curves
- Swaps
- Swaptions
- Risk calculations

---

# Example: Duration Approximation

Duration:

$$
5
$$

Yield rise:

$$
0.50\%
=
0.005
$$

Price change:

$$
-5 \times 0.005
$$

$$
=
-0.025
$$

Answer:

$$
-2.5\%
$$

approximately.

---

# Example: Credit Spread

Treasury:

$$
4.25\%
$$

Corporate:

$$
6.00\%
$$

Spread:

$$
1.75\%
$$

or

$$
175
\text{ bps}
$$

---

# Example: DV01

Portfolio DV01:

$$
2500
$$

Yield move:

$$
8
\text{ bps}
$$

PnL:

$$
2500
\times
8
$$

$$
=
20,000
$$

---

# Common Quant Infrastructure

Security Master:

- CUSIP
- ISIN
- Ticker

Market Data:

- Treasury yields
- Swap rates
- Credit spreads

Risk Engine:

- Duration
- DV01
- Convexity

Execution:

$$
\text{Signal}
\rightarrow
\text{Portfolio}
\rightarrow
\text{OMS}
\rightarrow
\text{FIX}
\rightarrow
\text{Dealer}
$$

---

# Common Beginner Mistakes

## Confusing Yield and Coupon

Coupon:

Fixed cash payment.

Yield:

Market-implied return.

---

## Ignoring Duration

Duration often dominates bond risk.

---

## Ignoring Convexity

Duration alone becomes inaccurate for large moves.

---

## Treating All Bonds Equally

Treasuries and corporate bonds have very different risk profiles.

---

## Ignoring Credit Risk

Higher yield usually implies higher risk.

---

## Forgetting Curve Risk

A portfolio can have:

$$
DV01 = 0
$$

while still having significant:

- Slope risk
- Curvature risk

---

# Key Takeaways

- Fixed income encompasses bonds, credit products, mortgages, swaps, and rate derivatives.
- Bond prices are the present value of future cash flows.
- Duration, DV01, and convexity are the core rate-risk measures.
- Yield curves drive much of fixed-income trading and portfolio construction.
- Credit products introduce default risk and spread risk.
- Interest-rate swaps are among the largest financial markets in the world.
- SOFR is now the dominant USD benchmark replacing LIBOR.
- Quant fixed-income research revolves around curve construction, risk decomposition, relative value analysis, carry, roll-down, and optimization.
- Institutional fixed-income systems are heavily centered around yield curves, factor models, DV01 aggregation, and large-scale optimization engines.
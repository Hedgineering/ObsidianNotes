# Swaps: Interest Rate Swaps, Cross-Currency Swaps, CDS, and Quant Implementation

## Quick Reference Table

| Concept | Definition | Key Formula / Idea |
|----------|------------|--------------------|
| Swap | Contract exchanging cash flows between two parties | Fixed ↔ Floating |
| Notional | Reference amount used to calculate payments | Usually never exchanged |
| Fixed Leg | Fixed-rate payments | Known in advance |
| Floating Leg | Variable-rate payments | SOFR, EURIBOR, etc. |
| Interest Rate Swap (IRS) | Exchange fixed and floating interest payments | Most common swap |
| Swap Rate | Fixed rate that makes swap value zero initially | Par swap rate |
| SOFR | Secured Overnight Financing Rate | Main USD benchmark |
| Cross-Currency Swap (CCS) | Exchange payments in different currencies | USD ↔ EUR |
| Basis Swap | Exchange floating rates | SOFR ↔ EURIBOR |
| CDS | Credit Default Swap | Default insurance |
| Receiver Swap | Receive fixed, pay floating | Bullish on bonds |
| Payer Swap | Pay fixed, receive floating | Bearish on bonds |
| DV01 | Dollar value of 1 bp move | Rate risk |
| OIS | Overnight Indexed Swap | SOFR-linked swap |
| Swap Curve | Yield curve built from swaps | Institutional benchmark |
| Clearing House | Central counterparty | Reduces credit risk |
| CSA | Credit Support Annex | Collateral agreement |

---

# Intuition

A swap is one of the most important financial contracts ever created.

Unlike a bond:

You do not typically exchange principal.

Instead:

You exchange cash flows.

Think:

> "I will pay you one stream of payments, and you pay me another."

---

# Why Swaps Exist

Organizations often want different risk exposures.

Examples:

- Company wants fixed borrowing cost
- Bank wants floating exposure
- Pension fund wants long-duration assets
- Hedge fund wants rate exposure

Swaps allow these exposures to be exchanged without buying or selling bonds.

---

# Basic Swap Structure

Suppose:

Notional:

$$
100,000,000
$$

One side pays:

$$
5\%
$$

fixed.

Other side pays:

$$
SOFR
$$

floating.

Only cash flow differences are exchanged.

---

# Notional

The notional is used only for calculation.

Usually:

$$
\text{Notional}
\neq
\text{Money Exchanged}
$$

Example:

$$
100,000,000
$$

notional.

No one transfers:

$$
100,000,000
$$

at inception.

It simply determines payment sizes.

---

# Interest Rate Swap (IRS)

The most common swap.

One side:

Pays fixed.

Receives floating.

The other side:

Receives fixed.

Pays floating.

---

# Example

Notional:

$$
100M
$$

Fixed Rate:

$$
5\%
$$

Floating Rate:

$$
SOFR
$$

If:

$$
SOFR = 4\%
$$

then:

Fixed payment:

$$
5M
$$

Floating payment:

$$
4M
$$

Net:

$$
1M
$$

paid by fixed payer.

---

# Swap Legs

## Fixed Leg

Known cash flows.

Example:

$$
5\%
$$

paid every six months.

---

## Floating Leg

Variable payments.

Example:

$$
SOFR + 25bp
$$

Changes each reset period.

---

# Payment Frequency

Common frequencies:

| Leg | Frequency |
|--------|----------|
| Fixed | Semiannual |
| Floating | Quarterly |
| Floating | Monthly |
| OIS | Daily compounded |

---

# Swap Rate

The fixed rate that makes:

$$
PV_{\text{Fixed}}
=
PV_{\text{Floating}}
$$

at inception.

This is called:

Par Swap Rate.

---

# Why Swap Rates Matter

Many institutional markets reference swap rates more than Treasury yields.

Used in:

- Derivatives pricing
- Corporate finance
- Mortgage hedging
- Risk management

---

# Interest Rate Risk Interpretation

---

## Receiver Swap

Receive fixed.

Pay floating.

Benefits when rates fall.

Equivalent intuition:

Long bond exposure.

---

## Payer Swap

Pay fixed.

Receive floating.

Benefits when rates rise.

Equivalent intuition:

Short bond exposure.

---

# Swap Valuation

General principle:

$$
V
=
PV_{\text{Receive}}
-
PV_{\text{Pay}}
$$

---

At inception:

$$
V
=
0
$$

because swap rate is chosen to balance both legs.

---

# Example

Receive leg:

$$
10.5M
$$

Pay leg:

$$
10.0M
$$

Swap value:

$$
500,000
$$

---

# SOFR

Modern USD benchmark.

Replaced LIBOR.

Used in:

- Swaps
- Futures
- Loans

---

# Why LIBOR Was Replaced

Problems:

- Manipulation scandals
- Thin underlying market

Transition:

$$
LIBOR
\rightarrow
SOFR
$$

---

# OIS (Overnight Indexed Swap)

Most important modern swap.

Fixed leg exchanged against:

Compounded overnight SOFR.

---

# Why OIS Matters

Used for:

- Discounting derivatives
- Curve construction
- Institutional valuation

---

# Swap Curve

A yield curve built from swap rates.

Often viewed as:

More relevant than Treasuries for derivative pricing.

---

# Yield Curve Construction

Inputs:

- Deposits
- SOFR
- Futures
- Swaps

Outputs:

$$
r(t)
$$

for all maturities.

---

# DV01

Most important swap risk metric.

Definition:

Dollar value change for:

$$
1
\text{ bp}
$$

rate movement.

---

# Example

DV01:

$$
5000
$$

Rates move:

$$
3
\text{ bps}
$$

PnL:

$$
5000
\times
3
$$

$$
=
15000
$$

---

# Swap Curve Factors

Three dominant factors:

---

## Level

Entire curve shifts.

---

## Slope

Short rates move relative to long rates.

Example:

$$
10Y - 2Y
$$

---

## Curvature

Middle maturities move differently.

Example:

$$
2 \times 5Y
-
2Y
-
10Y
$$

---

# Principal Component Analysis (PCA)

Most swap curve movement explained by:

1. Level
2. Slope
3. Curvature

Common in rates desks.

---

# Basis Swaps

Exchange one floating rate for another.

Example:

Pay:

$$
SOFR
$$

Receive:

$$
3M SOFR + 10bp
$$

or

$$
EURIBOR
$$

vs

$$
SOFR
$$

---

# Why Basis Exists

Different funding markets.

Different liquidity conditions.

Different credit risks.

---

# Cross-Currency Swaps (CCS)

Exchange cash flows in different currencies.

---

# Example

Party A:

Pays USD.

Receives EUR.

Party B:

Pays EUR.

Receives USD.

---

# Typical Structure

Initial principal exchange:

$$
USD
\leftrightarrow
EUR
$$

Periodic coupon exchange.

Final principal re-exchange.

---

# Why Cross-Currency Swaps Exist

Used by:

- Multinational corporations
- Sovereigns
- Banks

to obtain foreign funding.

---

# Example

European company wants:

USD borrowing.

Can:

1. Borrow EUR cheaply
2. Use CCS
3. Convert exposure into USD

---

# Cross-Currency Basis

Additional spread needed to balance market supply and demand.

Important macro indicator.

---

# Credit Default Swaps (CDS)

One of the most important credit derivatives.

---

# Intuition

Insurance on a bond.

Protection Buyer:

Pays premium.

Protection Seller:

Pays if default occurs.

---

# Example

Own:

$$
10M
$$

corporate bond.

Purchase CDS protection.

Premium:

$$
150
\text{ bp}
$$

per year.

---

# CDS Premium

Annual payment:

$$
10M
\times
0.015
$$

$$
=
150,000
$$

per year.

---

# CDS Payout

If default occurs:

Protection seller compensates losses.

---

# Recovery Rate

Suppose:

Recovery:

$$
40\%
$$

Loss:

$$
60\%
$$

CDS typically covers:

$$
60\%
$$

loss amount.

---

# CDS Spread Interpretation

Higher spread:

Higher default probability.

Example:

| CDS Spread | Interpretation |
|------------|----------------|
| 25 bp | Very safe |
| 100 bp | Moderate risk |
| 500 bp | Distressed |
| 2000 bp | Near default |

---

# Total Return Swaps (TRS)

Very important at hedge funds.

---

# Structure

One side receives:

Total return of asset.

Other side receives:

Funding rate.

---

# Example

Asset:

$$
S\&P 500
$$

Fund receives:

- Price appreciation
- Dividends

Bank receives:

$$
SOFR + Spread
$$

---

# Why TRS Exists

Allows:

- Leverage
- Synthetic exposure
- Balance-sheet efficiency

Widely used by hedge funds.

---

# Equity Swaps

Similar to TRS.

Underlying:

- Stock
- Index
- Basket

Instead of owning stock directly.

---

# Commodity Swaps

Exchange:

Fixed commodity price

for

Floating commodity price.

---

# Example

Airline:

Pays fixed oil price.

Receives floating oil price.

Locks in fuel cost.

---

# Inflation Swaps

Exchange:

Fixed inflation rate

for

Actual inflation.

Common benchmark:

CPI.

---

# Example

Pay:

$$
2.5\%
$$

fixed inflation.

Receive:

Actual CPI inflation.

---

# Clearing and Counterparty Risk

Historically:

Swaps were OTC.

Counterparty risk significant.

---

# Post-2008 Reforms

Many swaps now cleared through:

- CME
- LCH
- ICE

---

# Central Clearing

Clearinghouse becomes:

Buyer to every seller.

Seller to every buyer.

Reduces counterparty risk.

---

# Collateral and CSA

CSA:

Credit Support Annex.

Defines:

- Collateral requirements
- Margin rules
- Eligible collateral

---

# Initial Margin

Collateral posted at inception.

---

# Variation Margin

Daily mark-to-market transfers.

Similar to futures.

---

# Quantitative Swap Trading

Common strategies:

---

## Yield Curve Trades

Example:

Long:

$$
10Y
$$

swap.

Short:

$$
2Y
$$

swap.

---

## Spread Trades

Example:

Treasury yield

vs

Swap rate.

---

## Relative Value

Identify rich/cheap points on curve.

---

## Carry and Roll

Expected return from:

- Time passage
- Curve movement

---

# Common Fixed Income Factors

| Factor | Description |
|----------|------------|
| Level | Parallel shift |
| Slope | Curve steepness |
| Curvature | Curve shape |
| Carry | Income effect |
| Roll Down | Maturity migration effect |

---

# Common Quant Technologies

## Fixed Income Analytics

```python
QuantLib
rateslib
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

# QuantLib Example

```python
import QuantLib as ql

# industry-standard swap analytics
```

Used for:

- Yield curves
- Swaps
- CDS
- Swaptions

---

# Common Production Architecture

$$
\text{Market Data}
\rightarrow
\text{Curve Construction}
\rightarrow
\text{Valuation}
\rightarrow
\text{Risk Engine}
\rightarrow
\text{Portfolio}
\rightarrow
\text{Execution}
$$

---

# Example: Swap Valuation

Receive leg:

$$
12M
$$

Pay leg:

$$
11.5M
$$

Value:

$$
500,000
$$

---

# Example: DV01

DV01:

$$
2500
$$

Rate move:

$$
8
\text{ bp}
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

# Example: CDS Premium

Notional:

$$
5M
$$

Spread:

$$
200
\text{ bp}
$$

Annual payment:

$$
5,000,000
\times
0.02
$$

$$
=
100,000
$$

---

# Common Beginner Mistakes

## Thinking Notional Is Exchanged

Usually only cash flows are exchanged.

---

## Confusing Swap Rate and Treasury Yield

Swap curves and Treasury curves are different.

---

## Ignoring DV01

DV01 is often more important than notional.

---

## Treating CDS Like Bonds

CDS trades default risk directly.

No bond ownership required.

---

## Ignoring Collateral

Modern swap pricing depends heavily on collateralization assumptions.

---

## Ignoring Curve Risk

A portfolio may have:

$$
DV01 = 0
$$

while still having significant:

- Slope risk
- Curvature risk

---

# Key Takeaways

- Swaps are contracts exchanging streams of cash flows rather than principal.
- Interest Rate Swaps are among the largest financial markets in the world.
- The swap rate is chosen so that fixed and floating legs have equal present value at inception.
- SOFR has replaced LIBOR as the dominant USD floating benchmark.
- Cross-currency swaps allow institutions to obtain foreign-currency funding efficiently.
- CDS contracts isolate and trade credit risk separately from bonds.
- TRS and equity swaps provide synthetic asset exposure without owning the underlying asset.
- Modern swap markets are highly collateralized and increasingly centrally cleared.
- Quant swap trading revolves around curve construction, DV01 management, carry, roll-down, relative value, and factor exposures.
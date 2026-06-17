# Credit and Risk Securities

## Quick Reference Table

| Concept | Definition | Key Formula / Metric | Intuition |
|----------|------------|----------------------|-----------|
| Credit Risk | Risk that a borrower fails to make promised payments | Expected Loss = PD × LGD × EAD | Borrower might not pay back the loan |
| Default | Failure to meet debt obligations | — | Borrower misses payments or goes bankrupt |
| Probability of Default (PD) | Likelihood borrower defaults | $PD$ | Chance of failure |
| Loss Given Default (LGD) | Fraction lost if default occurs | $LGD = 1 - \text{Recovery Rate}$ | How much money is lost |
| Exposure at Default (EAD) | Amount owed at default | $EAD$ | Size of the loan when default occurs |
| Expected Loss (EL) | Average expected credit loss | $EL = PD \times LGD \times EAD$ | Long-run average loss |
| Credit Spread | Extra yield above a risk-free bond | Spread = Corporate Yield − Treasury Yield | Compensation for risk |
| Investment Grade Bond | Relatively safe corporate debt | BBB-/Baa3 or higher | Low default probability |
| High Yield Bond (Junk Bond) | Riskier corporate debt | Below BBB-/Baa3 | Higher return, higher risk |
| Recovery Rate | Fraction recovered after default | $1-LGD$ | What lenders get back |
| CDS | Credit Default Swap | Insurance-like contract | Protection against default |
| Structured Product | Security built from pools of assets | — | Risk packaged and redistributed |
| MBS | Mortgage-Backed Security | Pool of mortgages | Investors receive mortgage payments |
| ABS | Asset-Backed Security | Pool of loans/assets | Cash flows from many borrowers |
| CDO | Collateralized Debt Obligation | Structured debt product | Slices risk into tranches |

---

# Intuition

When you buy a Treasury Bill, the U.S. government promises to pay you.

When you buy a corporate bond, a company promises to pay you.

The key question becomes:

> "What if they don't?"

That possibility is called **credit risk**.

The entire field of credit investing attempts to answer:

- How likely is default?
- How much money will be lost?
- What return compensates for taking that risk?

---

# Risk-Free vs Credit Securities

## Risk-Free Security

Examples:

- Treasury Bills
- Treasury Notes
- Treasury Bonds

Investors generally assume:

$$
PD \approx 0
$$

because the U.S. government is extremely unlikely to default.

---

## Credit Security

Examples:

- Corporate bonds
- Bank loans
- Mortgages
- Municipal bonds
- Asset-backed securities

These depend on the financial health of the borrower.

---

# What Is Credit Risk?

Credit risk is the possibility that a borrower cannot fulfill promised payments.

Suppose:

You lend a company

$$
\$1000
$$

The company promises:

- 5% annual interest
- Repayment in 5 years

If the company goes bankrupt after 2 years:

- Interest payments stop
- Principal may not be fully repaid

You experience credit loss.

---

# Default

A default occurs when a borrower violates debt obligations.

Examples:

- Missed interest payment
- Missed principal payment
- Bankruptcy filing
- Debt restructuring

Default does not necessarily mean total loss.

Often lenders recover some portion of the debt.

---

# Recovery Rate

Recovery rate is the percentage recovered after default.

Suppose:

$$
\$1000
$$

was loaned.

After bankruptcy proceedings investors recover

$$
\$400
$$

Recovery rate:

$$
\frac{400}{1000}
=
40\%
$$

---

# Loss Given Default (LGD)

LGD measures the percentage lost.

Formula:

$$
LGD
=
1
-
\text{Recovery Rate}
$$

For the previous example:

$$
LGD
=
1 - 0.40
=
0.60
$$

or

$$
60\%
$$

loss.

---

# Probability of Default (PD)

PD measures the chance a borrower defaults.

Examples:

| Borrower | Approximate Risk |
|-----------|------------------|
| U.S. Treasury | Extremely low |
| Apple | Very low |
| Major Bank | Low |
| Small Startup | High |
| Distressed Company | Very high |

A bond with:

$$
PD = 0.02
$$

means roughly a

$$
2\%
$$

chance of default over the measurement horizon.

---

# Exposure at Default (EAD)

EAD is the amount owed when default occurs.

Suppose:

Loan balance:

$$
\$500,000
$$

at default.

Then:

$$
EAD
=
500,000
$$

---

# Expected Loss

Expected loss combines all three components.

Formula:

$$
EL
=
PD
\times
LGD
\times
EAD
$$

---

# Example: Expected Loss

Suppose:

$$
PD
=
5\%
$$

$$
LGD
=
60\%
$$

$$
EAD
=
100,000
$$

Then:

$$
EL
=
0.05
\times
0.60
\times
100000
$$

$$
=
3000
$$

Expected loss:

$$
\$3000
$$

---

# Credit Spreads

Investors require compensation for taking risk.

Corporate bonds therefore usually yield more than Treasuries.

Example:

| Security | Yield |
|-----------|---------|
| Treasury Bond | 4% |
| Corporate Bond | 6% |

Credit spread:

$$
6\%
-
4\%
=
2\%
$$

or

$$
200
\text{ basis points}
$$

---

# Basis Points (bps)

One basis point:

$$
1\text{ bp}
=
0.01\%
$$

Examples:

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

Credit traders frequently quote spreads in basis points.

---

# Credit Ratings

Rating agencies estimate creditworthiness.

Major agencies:

- Moody's
- S&P
- Fitch

---

## Investment Grade

Higher quality borrowers.

Ratings:

| S&P | Moody's |
|------|---------|
| AAA | Aaa |
| AA | Aa |
| A | A |
| BBB | Baa |

---

## High Yield (Junk)

Riskier debt.

Ratings:

| S&P | Moody's |
|------|---------|
| BB | Ba |
| B | B |
| CCC | Caa |
| CC | Ca |

Higher risk generally requires higher yields.

---

# Why Credit Spreads Change

Credit spreads widen when:

- Economy weakens
- Recession risk increases
- Company performance deteriorates

Investors demand more compensation.

---

Example:

Before recession:

$$
\text{Spread}
=
150\text{ bps}
$$

During recession:

$$
\text{Spread}
=
400\text{ bps}
$$

Bond prices fall because investors fear default.

---

# Credit Default Swaps (CDS)

A CDS is similar to insurance against default.

Participants:

- Protection Buyer
- Protection Seller

Buyer pays periodic premiums.

Seller compensates buyer if default occurs.

---

# CDS Example

Suppose:

You own:

$$
\$1,000,000
$$

of corporate bonds.

You fear bankruptcy.

You purchase CDS protection.

If default occurs:

- CDS seller compensates losses
- Bond holder receives protection

---

# Structured Credit Securities

Instead of lending to one borrower, financial institutions pool many loans together.

Examples:

- Mortgages
- Auto loans
- Credit card debt
- Student loans

The pool is then sold to investors.

---

# Mortgage-Backed Securities (MBS)

An MBS is a pool of mortgages.

Homeowners make mortgage payments.

These payments flow through to investors.

Visual:

$$
\text{Homeowners}
\rightarrow
\text{Mortgage Pool}
\rightarrow
\text{Investors}
$$

---

# Asset-Backed Securities (ABS)

Similar concept.

Underlying assets may include:

- Car loans
- Credit card receivables
- Student loans

Investors receive cash flows generated by borrowers.

---

# Collateralized Debt Obligations (CDOs)

CDOs package debt into layers called tranches.

Typical structure:

| Tranche | Risk |
|-----------|------|
| Senior | Lowest |
| Mezzanine | Medium |
| Equity | Highest |

Losses occur from bottom upward.

Visual:

$$
\text{Equity}
\rightarrow
\text{Mezzanine}
\rightarrow
\text{Senior}
$$

Equity absorbs losses first.

Senior investors are protected unless losses become very large.

---

# The 2008 Financial Crisis Connection

Many structured products contained mortgages.

Investors assumed housing defaults would remain low.

When defaults increased dramatically:

- MBS values collapsed
- CDO values collapsed
- Financial institutions suffered enormous losses

This highlighted the importance of credit risk modeling.

---

# Practice Problems

## Problem 1

A company bond has:

$$
PD = 4\%
$$

$$
LGD = 50\%
$$

$$
EAD = 200000
$$

Find expected loss.

### Solution

$$
EL
=
0.04
\times
0.50
\times
200000
$$

$$
=
4000
$$

Answer:

$$
\$4000
$$

---

## Problem 2

A bond has:

Recovery Rate:

$$
30\%
$$

Find LGD.

### Solution

$$
LGD
=
1 - 0.30
$$

$$
=
0.70
$$

Answer:

$$
70\%
$$

---

## Problem 3

Treasury Yield:

$$
4.2\%
$$

Corporate Yield:

$$
6.7\%
$$

Find spread.

### Solution

$$
6.7\%
-
4.2\%
=
2.5\%
$$

In basis points:

$$
2.5\%
=
250
\text{ bps}
$$

Answer:

$$
250\text{ bps}
$$

---

## Problem 4

A bond has:

Price:

$$
950
$$

Face Value:

$$
1000
$$

If issuer defaults and investors recover:

$$
400
$$

How much is lost?

### Solution

Loss:

$$
1000 - 400
$$

$$
=
600
$$

Recovery rate:

$$
\frac{400}{1000}
=
40\%
$$

LGD:

$$
60\%
$$

---

# Python Example: Expected Loss

```python
PD = 0.03
LGD = 0.55
EAD = 500000

expected_loss = PD * LGD * EAD

print(expected_loss)
```

---

# Python Example: Credit Spread

```python
treasury_yield = 0.04
corporate_yield = 0.062

spread = corporate_yield - treasury_yield

print(f"{spread:.2%}")
print(f"{spread * 10000:.0f} bps")
```

---

# Common Beginner Mistakes

## Confusing Yield with Credit Risk

High yield often indicates:

- Higher risk
- Higher default probability

not a "better" investment.

---

## Ignoring Recovery Rates

Default does not imply:

$$
100\%
$$

loss.

Recovery can be substantial.

---

## Assuming Ratings Never Change

Credit ratings can be upgraded or downgraded.

Downgrades often cause bond prices to fall.

---

## Treating Treasury Securities Like Corporate Bonds

Treasuries primarily carry:

- Interest rate risk

Corporate bonds carry:

- Interest rate risk
- Credit risk

---

# Key Takeaways

- Credit risk is the risk that promised payments are not made.
- Expected credit loss is

$$
EL
=
PD
\times
LGD
\times
EAD
$$

- Credit spreads compensate investors for default risk.
- Investment-grade bonds have lower default risk than high-yield bonds.
- CDS contracts provide protection against default.
- Structured products pool many loans together and redistribute risk.
- MBS, ABS, and CDOs are major examples of credit securities.
- Understanding credit risk is essential for bond investing, fixed income trading, and quantitative finance.
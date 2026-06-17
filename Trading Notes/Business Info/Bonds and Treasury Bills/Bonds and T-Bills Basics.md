# Bonds and Treasury Bills (T-Bills)

## Quick Reference Table

| Concept | Definition | Key Formula | Main Use |
|----------|------------|-------------|-----------|
| Bond | Loan made by investors to a government or company | Price = PV of coupons + PV of face value | Long-term borrowing |
| Face Value (Par Value) | Amount repaid at maturity | Usually $1000$ for many bonds | Principal repayment |
| Coupon Payment | Periodic interest payment | Coupon = Coupon Rate $\times$ Face Value | Income to bondholder |
| Coupon Rate | Fixed interest rate on face value | $\text{Coupon Rate} = \frac{\text{Annual Coupon}}{\text{Face Value}}$ | Determines cash flows |
| Maturity | Date principal is repaid | — | End of bond life |
| Yield to Maturity (YTM) | Market-implied return if held to maturity | Solved numerically | Compare investments |
| Treasury Bill (T-Bill) | Short-term U.S. government debt with no coupons | Bought at discount, redeemed at par | Cash management |
| Discount Rate | Difference between purchase price and face value | $\text{Profit} = F - P$ | T-Bill return |
| Treasury Note | Medium-term Treasury security | Pays coupons | 2–10 year maturity |
| Treasury Bond | Long-term Treasury security | Pays coupons | 20–30 year maturity |

---

# Intuition

Before learning formulas, think of bonds as IOUs.

Suppose your friend wants to borrow $100.

You lend him $100 today.

In exchange he promises:

1. To pay you interest every year.
2. To return the $100 later.

That promise is essentially a bond.

Governments and companies do exactly the same thing, except they borrow from thousands or millions of investors.

---

# Why Bonds Exist

Borrowers need money.

Examples:

- Governments fund roads, bridges, military spending, etc.
- Companies fund expansion, factories, acquisitions, etc.

Instead of taking a bank loan, they can sell bonds to investors.

Investors buy bonds because:

- Generally safer than stocks
- Predictable income
- Diversification
- Often backed by governments

---

# Basic Bond Terminology

Suppose a bond has:

- Face value = \$1000
- Coupon rate = 5%
- Maturity = 10 years

The annual coupon payment is

$$
50
=
0.05 \times 1000
$$

Every year the investor receives:

$$
\$50
$$

At maturity the investor receives:

$$
\$1000
$$

back in addition to the final coupon payment.

---

# Bond Cash Flow Timeline

Suppose:

- Face value = \$1000
- Coupon rate = 5%
- Maturity = 3 years

Cash flows:

| Time | Cash Flow |
|--------|------------|
| Today | Pay \$1000 |
| Year 1 | Receive \$50 |
| Year 2 | Receive \$50 |
| Year 3 | Receive \$50 + \$1000 |

---

# Present Value of a Bond

A bond is worth the present value of all future cash flows.

General formula:

$$
P
=
\sum_{t=1}^{n}
\frac{C}{(1+r)^t}
+
\frac{F}{(1+r)^n}
$$

where

- $P$ = bond price
- $C$ = coupon payment
- $F$ = face value
- $r$ = market interest rate
- $n$ = years to maturity

---

# Why Bond Prices Move

Bond prices move because interest rates move.

Suppose:

- Existing bond pays 5%
- New bonds suddenly pay 8%

Nobody wants your 5% bond unless it becomes cheaper.

Therefore:

$$
\text{Interest Rates Up}
\Rightarrow
\text{Bond Prices Down}
$$

and

$$
\text{Interest Rates Down}
\Rightarrow
\text{Bond Prices Up}
$$

This inverse relationship is one of the most important ideas in fixed income.

---

# Premium, Discount, and Par Bonds

## Par Bond

Price equals face value.

$$
P = F
$$

Example:

$$
P = 1000
$$

---

## Premium Bond

Price exceeds face value.

$$
P > F
$$

Example:

$$
P = 1100
$$

Reason:

- Coupon rate higher than market rates

---

## Discount Bond

Price below face value.

$$
P < F
$$

Example:

$$
P = 900
$$

Reason:

- Coupon rate lower than market rates

---

# Yield to Maturity (YTM)

YTM is the annualized return earned if:

- Bond is held until maturity
- Coupons are reinvested at the same rate

Conceptually:

$$
\text{Price}
=
\text{Present Value of Future Cash Flows}
$$

using YTM as the discount rate.

YTM is usually solved numerically.

---

# Treasury Securities Overview

The U.S. government issues several kinds of debt.

| Security | Maturity | Coupons? |
|-----------|-----------|-----------|
| Treasury Bill | Less than 1 year | No |
| Treasury Note | 2–10 years | Yes |
| Treasury Bond | 20–30 years | Yes |

---

# Treasury Bills (T-Bills)

Treasury Bills are the simplest fixed-income security.

They:

- Mature in one year or less
- Pay no coupons
- Are sold at a discount
- Pay full face value at maturity

---

# T-Bill Example

Suppose:

- Face value = \$1000
- Purchase price = \$950
- Maturity = 1 year

You pay:

$$
\$950
$$

One year later you receive:

$$
\$1000
$$

Profit:

$$
1000 - 950
=
50
$$

---

# T-Bill Return

General formula:

$$
\text{Return}
=
\frac{F-P}{P}
$$

where

- $F$ = face value
- $P$ = purchase price

For the example:

$$
\frac{1000-950}{950}
=
0.05263
$$

Therefore

$$
5.26\%
$$

annual return.

---

# Why T-Bills Are Considered Safe

T-Bills are backed by the U.S. government.

Historically they are considered one of the lowest-risk investments in the world.

Because of this:

- Returns are usually lower
- Institutions use them as a "risk-free" benchmark

Many finance models approximate the risk-free rate using Treasury yields.

---

# Relationship to Interest Rates

Suppose a 1-year T-Bill has:

$$
F = 1000
$$

and yields 5%.

Price must satisfy:

$$
1000
=
P(1.05)
$$

Therefore:

$$
P
=
\frac{1000}{1.05}
=
952.38
$$

Higher yields imply lower prices.

---

# Zero-Coupon Bonds

A T-Bill is a special case of a zero-coupon bond.

A zero-coupon bond:

- Pays no coupons
- Pays only face value at maturity

Pricing formula:

$$
P
=
\frac{F}{(1+r)^n}
$$

---

# Practice Problems

## Problem 1

A bond has:

- Face value = \$1000
- Coupon rate = 4%

Find annual coupon payment.

### Solution

$$
0.04 \times 1000
=
40
$$

Answer:

$$
\$40
$$

per year.

---

## Problem 2

A bond pays:

- Face value = \$1000
- Coupon rate = 6%
- Maturity = 5 years

How much coupon income is received over the life of the bond?

### Solution

Annual coupon:

$$
0.06 \times 1000
=
60
$$

Total coupons:

$$
60 \times 5
=
300
$$

Answer:

$$
\$300
$$

---

## Problem 3

A T-Bill has:

- Purchase price = \$980
- Face value = \$1000

Find profit.

### Solution

$$
1000 - 980
=
20
$$

Answer:

$$
\$20
$$

---

## Problem 4

A T-Bill has:

- Face value = \$1000
- Purchase price = \$970

Find return.

### Solution

$$
\frac{1000-970}{970}
=
0.03093
$$

Therefore:

$$
3.09\%
$$

---

## Problem 5

A zero-coupon bond has:

- Face value = \$1000
- Yield = 5%
- Maturity = 2 years

Find price.

### Solution

$$
P
=
\frac{1000}{(1.05)^2}
$$

$$
=
907.03
$$

Answer:

$$
\$907.03
$$

---

## Problem 6

A bond has:

- Face value = \$1000
- Coupon = \$50
- Market interest rate = 5%
- Maturity = 1 year

Find fair price.

### Solution

Cash flow at maturity:

$$
50 + 1000
=
1050
$$

Discount back one year:

$$
P
=
\frac{1050}{1.05}
=
1000
$$

Answer:

$$
\$1000
$$

---

# Python Examples

## Price a Zero-Coupon Bond

```python
face_value = 1000
yield_rate = 0.05
years = 3

price = face_value / (1 + yield_rate) ** years

print(price)
```

---

## Price a Coupon Bond

```python
import numpy as np

face_value = 1000
coupon = 50
yield_rate = 0.04
years = 5

cashflows = [coupon] * (years - 1)
cashflows.append(coupon + face_value)

price = sum(
    cf / (1 + yield_rate) ** t
    for t, cf in enumerate(cashflows, start=1)
)

print(price)
```

---

# Common Beginner Mistakes

1. Confusing coupon rate with yield.

Coupon rate:

$$
\frac{\text{Coupon}}{\text{Face Value}}
$$

Yield depends on current market price.

---

2. Thinking bonds always trade at face value.

They frequently trade:

- Above par
- Below par

depending on interest rates.

---

3. Forgetting that bond prices and yields move in opposite directions.

$$
\uparrow \text{Yield}
\Rightarrow
\downarrow \text{Price}
$$

$$
\downarrow \text{Yield}
\Rightarrow
\uparrow \text{Price}
$$

---

4. Assuming Treasury Bills pay coupons.

They do not.

T-Bills are discount securities.

---

# Key Takeaways

- A bond is a loan from investors to a government or company.
- Bondholders typically receive coupon payments and face value at maturity.
- Bond price equals the present value of future cash flows.
- Bond prices and interest rates move in opposite directions.
- Treasury Bills are short-term government debt that pay no coupons.
- T-Bills are purchased below face value and redeemed at par.
- Treasury yields are often used as the closest practical approximation to a risk-free rate in finance.
- Understanding discounting and present value is the foundation for all fixed-income valuation.
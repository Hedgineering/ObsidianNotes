# Poisson Distribution

## Quick Summary

The Poisson distribution models the **number of events occurring in a fixed interval of time, space, area, volume, or other continuous measure**, assuming:

- Events occur independently.
- Events occur at a constant average rate.
- Two events cannot occur at exactly the same instant (or the probability is negligible).
- The probability of an event in a very small interval is proportional to the interval length.

Examples:

- Number of customer arrivals per hour.
- Number of trades executed per second.
- Number of emails received today.
- Number of manufacturing defects per meter.
- Number of meteor strikes per year.
- Number of photons hitting a sensor.
- Number of insurance claims per month.

---

# Parameters

The Poisson distribution has one parameter:

$$
\lambda > 0
$$

where

$$
\lambda
=
\text{expected number of events during the interval}
$$

Examples

- 5 customers/hour

$$
\lambda=5
$$

- 20 website requests/minute

$$
\lambda=20
$$

---

# Random Variable

Define

$$
X
=
\text{number of events occurring during one interval}
$$

Possible values

$$
0,1,2,3,\ldots
$$

---

# Probability Mass Function (PMF)

The probability of observing exactly k events is

$$
P(X=k)
=
\frac{e^{-\lambda}\lambda^k}{k!}
$$

for

$$
k=0,1,2,\ldots
$$

---

# Intuition Behind the Formula

Think of the interval as being split into millions of tiny pieces.

Each tiny interval has

- tiny probability of an event
- almost impossible for two events simultaneously

This resembles many independent Bernoulli trials.

As the interval becomes infinitely small, the Binomial distribution converges to the Poisson distribution.

This is why Poisson is often called

> The limiting distribution of the Binomial.

---

# Mean and Variance

One unique property:

$$
\boxed{
E[X]
=
Var(X)
=
\lambda
}
$$

Therefore

Standard deviation

$$
\sigma
=
\sqrt{\lambda}
$$

Example

If

$$
\lambda=16
$$

then

Mean

$$
16
$$

Variance

$$
16
$$

Standard deviation

$$
4
$$

---

# Shape

Small λ

```text
■■■■■■■■
■■■
■
```

Mostly zeros and ones.

---

Moderate λ

```text
      ■
    ■■■■
  ■■■■■■■
■■■■■■■■■■
```

Right-skewed.

---

Large λ

Looks increasingly like a Normal distribution.

Rule of thumb

$$
\lambda \ge 10
$$

often gives a reasonable Normal approximation.

---

# Relationship to the Binomial Distribution

Suppose

$$
X
\sim
Binomial(n,p)
$$

When

- n is very large
- p is very small

and

$$
np=\lambda
$$

remains constant,

then

$$
Binomial(n,p)
\approx
Poisson(\lambda)
$$

Rule of thumb

Use the Poisson approximation when

$$
n>20
$$

and

$$
p<0.05
$$

---

# Memory Trick

Binomial

> Fixed number of trials.

Poisson

> Fixed amount of time or space.

Geometric

> Waiting until first success.

Negative Binomial

> Waiting until the r-th success.

---

# Common Quant Finance Applications

Market Microstructure

- Orders arriving each millisecond
- Market orders
- Limit order arrivals
- Cancel events

Risk

- Operational failures
- Default events
- Insurance claims

Trading

- Quote requests
- RFQs
- News events
- Message traffic

Networking

- Packet arrivals
- Server requests

Manufacturing

- Defect counts
- Equipment failures

---

# Example 1

A call center receives

$$
8
$$

calls per hour.

Find the probability of receiving exactly

$$
5
$$

calls this hour.

Solution

Given

$$
\lambda=8
$$

Need

$$
P(X=5)
$$

Use

$$
P(X=k)
=
\frac{e^{-8}8^5}{5!}
$$

Compute

$$
5!=120
$$

Therefore

$$
P(X=5)
=
\frac{e^{-8}(32768)}{120}
\approx
0.0916
$$

Answer

$$
\boxed{0.0916}
$$

---

# Example 2

Average defects:

$$
3
$$

per sheet.

Find probability of zero defects.

Solution

$$
\lambda=3
$$

Need

$$
P(X=0)
$$

Using

$$
P(X=0)
=
e^{-3}
\frac{3^0}{0!}
$$

Since

$$
0!=1
$$

Answer

$$
\boxed{
e^{-3}
\approx
0.0498
}
$$

---

# Example 3

Customers arrive

at

$$
12
$$

per hour.

Probability of at least one customer arriving?

Solution

Use complement.

$$
P(X\ge1)
=
1-P(X=0)
$$

Compute

$$
P(X=0)
=
e^{-12}
$$

Answer

$$
P(X\ge1)
=
1-e^{-12}
\approx
0.999994
$$

---

# Example 4

A stock exchange receives

$$
20
$$

market orders per second.

What is the probability exactly

$$
18
$$

arrive next second?

Solution

$$
\lambda=20
$$

Need

$$
P(X=18)
$$

$$
=
\frac{e^{-20}20^{18}}{18!}
$$

Calculator

$$
\boxed{0.084}
$$

---

# Example 5

Server requests average

$$
100
$$

per minute.

Find

$$
P(X>120)
$$

Solution

Direct calculation is tedious.

Since

$$
\lambda=100
$$

is large,

Approximate

$$
X
\approx
N(100,100)
$$

Standard deviation

$$
10
$$

Compute

$$
z
=
\frac{120.5-100}{10}
=
2.05
$$

(Use continuity correction.)

Look up

$$
P(Z>2.05)
\approx
0.020
$$

Answer

Approximately

$$
2\%
$$

---

# Example 6 (Poisson Approximation)

A factory produces

$$
5000
$$

light bulbs.

Each has

$$
0.001
$$

probability of being defective.

Find probability exactly

$$
8
$$

defects.

Exact

$$
Binomial(5000,0.001)
$$

Instead

$$
\lambda=np
=
5
$$

Approximate

$$
P(X=8)
=
\frac{e^{-5}5^8}{8!}
\approx
0.065
$$

---

# Expected Value

One elegant property

$$
E[X]
=
\lambda
$$

Interpretation

If the average arrival rate is

$$
30
$$

orders/minute,

then on average

$$
30
$$

orders arrive every minute.

---

# Variance

Also

$$
Var(X)
=
\lambda
$$

This equality is special.

Many real datasets have

Variance

>

Mean.

This is called

**Overdispersion.**

In that case,

the Poisson model is often inappropriate.

Common alternatives

- Negative Binomial
- Zero-inflated Poisson
- Hawkes Process (finance)
- Cox Process

---

# Properties

Independent intervals

If arrivals are Poisson,

then disjoint time intervals are independent.

---

Additivity

If

$$
X_1
\sim
Poisson(\lambda_1)
$$

and

$$
X_2
\sim
Poisson(\lambda_2)
$$

are independent,

then

$$
X_1+X_2
\sim
Poisson(\lambda_1+\lambda_2)
$$

Very useful in queueing theory.

---

Scaling Time

Suppose

Average

$$
12
$$

arrivals/hour.

Then

30 minutes

has

$$
\lambda=6
$$

15 minutes

has

$$
\lambda=3
$$

Generally

$$
\lambda_{\text{new}}
=
\lambda_{\text{old}}
\times
\frac{\text{new interval}}{\text{old interval}}
$$

---

# Common Interview Questions

Q:

Orders arrive at

$$
50
$$

per second.

Expected orders in

0.2 seconds?

Answer

$$
50(0.2)=10
$$

---

Q:

Customers average

6/hour.

Probability no customers in

30 minutes?

Solution

Scale λ.

$$
\lambda=3
$$

Answer

$$
P(X=0)
=
e^{-3}
\approx0.0498
$$

---

Q:

If two independent Poisson processes have rates

$$
5
$$

and

$$
8
$$

per minute,

what is the combined process?

Answer

$$
Poisson(13)
$$

---

Q:

What is unusual about the Poisson distribution?

Answer

$$
E[X]=Var(X)=\lambda
$$

---

# Common Mistakes

❌ Using Poisson when the number of trials is fixed.

Use Binomial instead.

---

❌ Forgetting to rescale λ when changing the time interval.

---

❌ Forgetting

$$
0!=1
$$

---

❌ Using Poisson when arrivals are clustered or dependent.

Poisson assumes independent arrivals.

---

# Cheat Sheet

| Quantity | Formula |
|-----------|----------|
| PMF | $\frac{e^{-\lambda}\lambda^k}{k!}$ |
| Mean | $\lambda$ |
| Variance | $\lambda$ |
| Std Dev | $\sqrt{\lambda}$ |
| Approximation | $Bin(n,p)\rightarrow Poisson(np)$ |
| Additivity | $Pois(\lambda_1)+Pois(\lambda_2)=Pois(\lambda_1+\lambda_2)$ |
| Time Scaling | $\lambda_{new}=\lambda\cdot\frac{t_{new}}{t_{old}}$ |

---

# Key Intuition

The Poisson distribution answers one question:

> **"If events occur randomly at an average rate of λ, how many events should I expect to see in a fixed interval?"**

Unlike the Binomial distribution, there is **no fixed number of trials**. Instead, events occur continuously over time or space.

The Poisson distribution naturally arises whenever many independent opportunities for an event exist, each with a tiny probability of occurring, making it one of the most important models for arrival processes in statistics, operations research, networking, and quantitative finance.
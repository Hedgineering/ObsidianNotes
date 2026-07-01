# Exponential Distribution

## Quick Reference

| Property           | Formula                                     |
| ------------------ | ------------------------------------------- |
| Notation           | $X \sim \text{Exponential}(\lambda)$        |
| Parameter          | $\lambda > 0$ (rate parameter)              |
| Support            | $x \ge 0$                                   |
| PDF                | $f_X(x)=\lambda e^{-\lambda x}$             |
| CDF                | $F_X(x)=1-e^{-\lambda x}$                   |
| Mean               | $E[X]=\frac{1}{\lambda}$                    |
| Variance           | $\operatorname{Var}(X)=\frac{1}{\lambda^2}$ |
| Standard Deviation | $\sigma=\frac{1}{\lambda}$                  |
| Median             | $\frac{\ln 2}{\lambda}$                     |
| Mode               | $0$                                         |
| Memoryless?        | Yes                                         |

---

# What Does

$$
X\sim\text{Exponential}(\lambda)
$$

Actually Mean?

This notation means:

> The random variable $X$ follows an **Exponential distribution** with **rate parameter** $\lambda$.

This completely specifies the distribution.

In particular,

- the PDF is

  $$
  f_X(x)=
  \begin{cases}
  \lambda e^{-\lambda x}, & x\ge0\\
  0, & x<0
  \end{cases}
  $$

- the CDF is

  $$
  F_X(x)
  =
  1-e^{-\lambda x},
  \qquad x\ge0
  $$

- the expected value is

  $$
  E[X]
  =
  \frac1\lambda.
  $$

---

# Intuition

The exponential distribution models

> **The waiting time until the first event occurs.**

Examples include:

- Time until the next customer enters a store.
- Time until the next radioactive decay.
- Time until the next phone call.
- Time until the next trade arrives.
- Time until the next machine failure.

The only assumption is that events occur

- randomly,
- independently,
- at a constant average rate.

---

# Understanding the Rate Parameter

The parameter

$$
\lambda
$$

is called the **rate**.

Think of it as

> Expected events per unit time.

Example:

If

$$
\lambda=4,
$$

then on average,

there are

$$
4
$$

events every unit of time.

Since events happen quickly,

the waiting time is short.

Indeed,

$$
E[X]=\frac14.
$$

---

If instead

$$
\lambda=0.2,
$$

events are rare.

Average waiting time becomes

$$
E[X]=5.
$$

---

## Summary

Large

$$
\lambda
$$

↓

Events happen frequently.

↓

Waiting time is short.

---

Small

$$
\lambda
$$

↓

Events happen rarely.

↓

Waiting time is long.

---

# Shape of the PDF

The PDF is

$$
f(x)=\lambda e^{-\lambda x}.
$$

Properties:

- Starts at

  $$
  f(0)=\lambda.
  $$

- Decreases exponentially.

- Never becomes negative.

- Area under the curve equals 1.

Typical shape:

```text
 ^
 |\
 | \
 |  \
 |   \
 |    \
 |      \
 |        \ ________________
 +---------------------------->
 0
```

---

# Support

The exponential distribution only models **nonnegative waiting times**.

Therefore,

$$
X\ge0.
$$

Negative waiting times are impossible.

---

# PDF

The probability density function is

$$
f_X(x)=
\begin{cases}
\lambda e^{-\lambda x}, & x\ge0\\
0,&x<0.
\end{cases}
$$

Remember:

A PDF is **not** a probability.

Probabilities are computed by integration.

Example:

$$
P(a\le X\le b)
=
\int_a^b
\lambda e^{-\lambda x}\,dx.
$$

---

# CDF

The cumulative distribution function is

$$
F_X(x)
=
P(X\le x).
$$

Integrating the PDF gives

$$
F_X(x)
=
1-e^{-\lambda x},
\qquad x\ge0.
$$

Therefore,

$$
P(X>x)
=
1-F(x)
=
e^{-\lambda x}.
$$

This is called the **survival function**.

---

# Expected Value

The mean waiting time is

$$
E[X]
=
\frac1\lambda.
$$

Interpretation:

If events occur

at rate

$$
\lambda,
$$

then the average waiting time is

its reciprocal.

---

# Variance

The variance is

$$
\operatorname{Var}(X)
=
\frac1{\lambda^2}.
$$

The standard deviation is therefore

$$
\sigma
=
\frac1\lambda.
$$

Notice that

for an exponential distribution,

Mean = Standard Deviation.

---

# Memoryless Property

The exponential distribution is famous because it is

> **Memoryless.**

This means

$$
P(X>s+t\mid X>s)
=
P(X>t).
$$

Interpretation:

If you've already waited

10 minutes,

your expected future waiting time is

the same as if you had just started waiting.

The process has **no memory**.

---

## Example

Suppose buses arrive according to

$$
X\sim\text{Exponential}(0.2).
$$

You've already waited

15 minutes.

What is the probability you wait another

5 minutes?

The answer is

$$
P(X>20\mid X>15)
=
P(X>5).
$$

You completely ignore the first

15 minutes.

---

# Why Is It Memoryless?

Imagine customers arrive randomly.

Suppose nobody has arrived for

30 minutes.

Does this make an arrival more likely now?

No.

The process has no "built-up pressure."

Every future second behaves exactly like the first.

---

# Relationship to the Poisson Distribution

The exponential distribution and Poisson distribution describe the **same process** from different viewpoints.

## Poisson

Counts

> **How many events occur in a fixed time interval.**

Example:

How many emails arrive in one hour?

---

## Exponential

Measures

> **How long until the next event occurs.**

Example:

How long until the next email arrives?

---

If events follow a Poisson process with rate

$$
\lambda,
$$

then

- Number of events

  follows a Poisson distribution.

- Waiting times

  follow an Exponential distribution.

---

# Computing Expectations Using LOTUS

Suppose

$$
X\sim\text{Exponential}(\lambda).
$$

To compute

$$
E[g(X)],
$$

use

$$
E[g(X)]
=
\int_0^\infty
g(x)\lambda e^{-\lambda x}\,dx.
$$

---

## Example 1

Find

$$
E[X].
$$

Using LOTUS,

$$
E[X]
=
\int_0^\infty
x\lambda e^{-\lambda x}\,dx.
$$

Evaluating the integral gives

$$
\boxed{E[X]=\frac1\lambda.}
$$

---

## Example 2

Suppose

$$
X\sim\text{Exponential}(2).
$$

Find

$$
P(X>3).
$$

Use the survival function:

$$
P(X>3)
=
e^{-2(3)}
=
e^{-6}.
$$

---

## Example 3

Suppose

$$
X\sim\text{Exponential}(5).
$$

Find the average waiting time.

Since

$$
E[X]=\frac1\lambda,
$$

we have

$$
E[X]=\frac15.
$$

---

# When Is the Exponential Distribution Used?

The exponential distribution is commonly used to model:

- Customer arrivals.
- Network packet arrivals.
- Machine failures.
- Radioactive decay.
- Insurance claim arrivals.
- Incoming phone calls.
- Trades in financial markets.
- Time between earthquakes.
- Time until component failure.

Whenever events occur randomly and independently at a constant average rate, the exponential distribution is often an appropriate model.

---

# Common Mistakes

### Confusing the Rate and Mean

Rate is actually:

$$
\lambda
$$

Mean or expectation is actually:

$$
\frac1\lambda.
$$

---

### Using the PDF as a Probability

Remember:

$$
f(x)
$$

is a density,

not a probability.

Probabilities require integration.

---

### Forgetting the Support

The exponential distribution only exists on

$$
[0,\infty).
$$

Negative values are impossible.

---

# Summary

- \(X\sim\text{Exponential}(\lambda)\) means \(X\) is exponentially distributed with rate \(\lambda\).
- Models **waiting times** between random events.
- Support is

  $$
  x\ge0.
  $$

- PDF:

  $$
  f(x)=\lambda e^{-\lambda x}.
  $$

- CDF:

  $$
  F(x)=1-e^{-\lambda x}.
  $$

- Mean:

  $$
  E[X]=\frac1\lambda.
  $$

- Variance:

  $$
  \operatorname{Var}(X)=\frac1{\lambda^2}.
  $$

- Memoryless property:

  $$
  P(X>s+t\mid X>s)=P(X>t).
  $$

- Closely related to the [[Poisson Distribution]]:

  - Poisson counts events.
  - Exponential measures waiting times between events.
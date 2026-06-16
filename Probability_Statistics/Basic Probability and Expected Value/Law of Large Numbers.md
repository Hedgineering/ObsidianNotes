# Law of Large Numbers (LLN)

## Motivation

The Law of Large Numbers explains why averages become stable as we collect more data.

Even though individual observations may vary significantly, the average of many observations tends to settle near the true expected value.

This theorem provides the theoretical foundation for:

- Polling and surveys
- Monte Carlo simulation
- Casino profitability
- Insurance pricing
- Statistical estimation
- Machine learning model evaluation

---

## Informal Statement

As the number of observations increases, the sample average approaches the true mean.

In other words:

> Randomness averages out over time.

For a small sample, the average can fluctuate substantially.

For a large sample, the average becomes increasingly stable.

---

## Setup

Suppose

$$
X_1, X_2, \ldots, X_n
$$

are independent and identically distributed (i.i.d.) random variables with

$$
E[X_i] = \mu
$$

and

$$
\operatorname{Var}(X_i) = \sigma^2
$$

Define the sample mean:

$$
\bar{X}_n
=
\frac{1}{n}
\sum_{i=1}^{n}
X_i
$$

The Law of Large Numbers describes what happens to

$$
\bar{X}_n
$$

as

$$
n \to \infty.
$$

---

## Weak Law of Large Numbers (WLLN)

The Weak Law states that the sample mean converges in probability to the true mean.

Mathematically,

$$
\bar{X}_n
\xrightarrow{P}
\mu
$$

This means that for every

$$
\varepsilon > 0,
$$

$$
P
\left(
\left|
\bar{X}_n - \mu
\right|
>
\varepsilon
\right)
\to 0
\quad \text{as } n \to \infty.
$$

Interpretation:

As more observations are collected, the probability that the sample mean is far from the true mean approaches zero.

---

## Strong Law of Large Numbers (SLLN)

The Strong Law states that the sample mean converges almost surely to the true mean.

Mathematically,

$$
\bar{X}_n
\xrightarrow{\text{a.s.}}
\mu
$$

Interpretation:

With probability 1, the sequence of sample means eventually approaches and remains arbitrarily close to the true mean.

The Strong Law is a stronger statement than the Weak Law.

---

## Intuition

Suppose you repeatedly flip a fair coin.

Let

$$
X_i
=
\begin{cases}
1, & \text{Heads} \\
0, & \text{Tails}
\end{cases}
$$

Then

$$
E[X_i]
=
0.5
$$

For a small number of flips, the proportion of heads may be far from 0.5.

Examples:

$$
\frac{4}{5}
=
0.8
$$

or

$$
\frac{2}{5}
=
0.4
$$

However, as the number of flips grows,

$$
\frac{\text{Number of Heads}}
{\text{Number of Flips}}
$$

tends to move closer and closer to

$$
0.5.
$$

The Law of Large Numbers guarantees this behavior.

---

## Why the Sample Mean Stabilizes

Using variance properties,

$$
\operatorname{Var}
\left(
\bar{X}_n
\right)
=
\operatorname{Var}
\left(
\frac{1}{n}
\sum_{i=1}^{n}
X_i
\right)
$$

For independent observations,

$$
=
\frac{1}{n^2}
\sum_{i=1}^{n}
\operatorname{Var}(X_i)
$$

$$
=
\frac{1}{n^2}
(n\sigma^2)
$$

$$
=
\frac{\sigma^2}{n}
$$

As

$$
n \to \infty,
$$

$$
\frac{\sigma^2}{n}
\to 0.
$$

Thus the variability of the sample mean shrinks as more observations are collected.

This is the key reason the sample mean becomes increasingly reliable.

---

## Relationship to Expected Value

The expected value

$$
E[X]
=
\mu
$$

is a theoretical quantity describing the long-run average outcome.

The sample mean

$$
\bar{X}_n
$$

is the quantity actually observed from data.

The Law of Large Numbers connects these concepts:

$$
\bar{X}_n
\to
E[X]
$$

as the sample size grows.

---

## Relationship to the Central Limit Theorem

The Law of Large Numbers and the Central Limit Theorem answer different questions.

### Law of Large Numbers

Answers:

> Where does the sample mean go?

Result:

$$
\bar{X}_n
\to
\mu
$$

### Central Limit Theorem

Answers:

> How does the sample mean fluctuate around the mean?

Result:

$$
\frac{
\bar{X}_n - \mu
}{
\sigma / \sqrt{n}
}
\xrightarrow{d}
N(0,1)
$$

The LLN describes convergence.

The CLT describes the distribution of the error around that limit.

---

## Common Misconception: The Gambler's Fallacy

The Law of Large Numbers does NOT say that outcomes "must balance out" in the short run.

After observing

$$
10
$$

heads in a row,

the probability of heads on the next fair coin flip is still

$$
0.5.
$$

The LLN concerns long-run averages, not short-run corrections.

Future outcomes do not compensate for past outcomes.

---

## Conditions

The standard LLN generally requires:

- Independent observations
- Identically distributed observations
- Finite expected value

Typically we assume

$$
E[|X|] < \infty.
$$

Many common probability distributions satisfy these assumptions.

---

## Key Takeaways

The sample mean is

$$
\bar{X}_n
=
\frac{1}{n}
\sum_{i=1}^{n}
X_i
$$

Weak Law:

$$
\bar{X}_n
\xrightarrow{P}
\mu
$$

Strong Law:

$$
\bar{X}_n
\xrightarrow{\text{a.s.}}
\mu
$$

Variance of the sample mean:

$$
\operatorname{Var}
\left(
\bar{X}_n
\right)
=
\frac{\sigma^2}{n}
$$

As more observations are collected:

- The sample mean becomes more stable.
- The sample mean approaches the true mean.
- Random fluctuations become less important.
- Long-run averages become predictable.
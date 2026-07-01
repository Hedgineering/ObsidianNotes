# Chi-Squared Goodness-of-Fit Test

## Quick Reference

| Concept | Formula | Purpose |
|---|---|---|
| Test Statistic | $$\chi^2=\sum_{i=1}^{k}\frac{(O_i-E_i)^2}{E_i}$$ | Measures discrepancy between observed and expected frequencies |
| Null Hypothesis | $H_0:$ Data follow the specified distribution | Assumes observed differences are due to random chance |
| Alternative Hypothesis | $H_a:$ Data do **not** follow the specified distribution | Indicates the model is a poor fit |
| Degrees of Freedom | $$df=k-1-p$$ | $k$ = number of categories, $p$ = estimated parameters |
| Decision Rule | Large $\chi^2$ $\Rightarrow$ reject $H_0$ | Large deviations from expected counts are unlikely by chance |

---

# Intuition

The Chi-Squared Goodness-of-Fit test asks:

> **"Do the observed frequencies differ from what we would expect simply because of random chance?"**

If the observed frequencies are close to the expected frequencies, then the data are consistent with the hypothesized distribution.

If the observed frequencies differ substantially, then the proposed distribution is probably incorrect.

---

# When Is It Used?

The test is commonly used to determine whether observed data follow a specified distribution.

Examples include:

- Uniform distribution
- Binomial distribution
- Poisson distribution
- Normal distribution (after binning)
- Dice fairness
- Coin fairness
- Random number generator testing

For pseudo-random number generators, it is often used to determine whether generated values appear uniformly distributed.

---

# Hypotheses

The hypotheses are

$$
H_0:
\text{The data follow the specified distribution.}
$$

$$
H_a:
\text{The data do not follow the specified distribution.}
$$

---

# Test Statistic

Suppose there are

$$
k
$$

categories.

For each category,

- $O_i$ = observed frequency
- $E_i$ = expected frequency

The test statistic is

$$
\chi^2
=
\sum_{i=1}^{k}
\frac{(O_i-E_i)^2}{E_i}.
$$

Each term measures how far the observed count is from the expected count.

Large discrepancies contribute heavily because the difference is squared.

---

# Why Divide by $E_i$?

Suppose category A expects

$$
5
$$

observations,

while category B expects

$$
500.
$$

An error of

$$
5
$$

is enormous for category A,

but negligible for category B.

Dividing by

$$
E_i
$$

normalizes the deviations so that categories with different expected counts are comparable.

---

# Degrees of Freedom

If no parameters are estimated,

$$
df
=
k-1.
$$

More generally,

$$
df
=
k-1-p,
$$

where

- $k$ = number of categories,
- $p$ = number of estimated parameters.

Examples:

| Distribution | Estimated Parameters | Degrees of Freedom |
|---|---:|---:|
| Uniform | 0 | $k-1$ |
| Binomial ($p$ estimated) | 1 | $k-2$ |
| Normal ($\mu,\sigma$ estimated) | 2 | $k-3$ |

---

# Why Do We Lose One Degree of Freedom?

The observed counts must satisfy

$$
\sum_{i=1}^{k}
O_i
=
n.
$$

Once

$$
k-1
$$

counts are known,

the final count is determined automatically.

Therefore only

$$
k-1
$$

counts are independent.

Estimating parameters reduces the degrees of freedom even further.

---

# Worked Example

Suppose we roll a die

$$
60
$$

times.

The observed frequencies are

| Face | Observed |
|---:|---:|
|1|8|
|2|12|
|3|9|
|4|11|
|5|10|
|6|10|

---

## Step 1

Assume the die is fair.

Therefore

$$
P(i)=\frac16.
$$

Expected frequency:

$$
E_i
=
60
\times
\frac16
=
10.
$$

---

## Step 2

Compute each contribution.

| Face | $O_i$ | $E_i$ | Contribution |
|---:|---:|---:|---:|
|1|8|10|$$\dfrac{(8-10)^2}{10}=0.4$$|
|2|12|10|$$0.4$$|
|3|9|10|$$0.1$$|
|4|11|10|$$0.1$$|
|5|10|10|$$0$$|
|6|10|10|$$0$$|

---

## Step 3

Add them together.

$$
\chi^2
=
0.4
+
0.4
+
0.1
+
0.1
=
1.0.
$$

---

## Step 4

Degrees of freedom

There are

$$
k=6
$$

categories,

so

$$
df
=
6-1
=
5.
$$

---

## Step 5

Compare with the critical value.

For

$$
df=5,
\qquad
\alpha=0.05,
$$

the critical value is approximately

$$
11.07.
$$

Since

$$
1.0
<
11.07,
$$

we **fail to reject** the null hypothesis.

Conclusion:

The observed frequencies are consistent with a fair die.

---

# Example: Testing a Random Number Generator

Suppose an RNG generates

$$
1000
$$

numbers.

Divide

$$
[0,1)
$$

into

$$
10
$$

equal bins.

Expected count per bin:

$$
E_i
=
100.
$$

Suppose the observed counts are

| Bin | Count |
|---:|---:|
|1|105|
|2|93|
|3|102|
|4|95|
|5|100|
|6|110|
|7|97|
|8|98|
|9|101|
|10|99|

Compute

$$
\chi^2
=
\sum
\frac{(O_i-100)^2}{100}.
$$

If the statistic exceeds the critical value,

the generator fails the goodness-of-fit test.

---

# Interpretation

Small values of

$$
\chi^2
$$

mean

- observed counts are close to expected,
- the model fits well.

Large values mean

- observed counts differ substantially,
- the proposed distribution is questionable.

---

# Assumptions

The Chi-Squared Goodness-of-Fit test assumes

- observations are independent,
- categories are mutually exclusive,
- expected counts are sufficiently large.

A common rule is

$$
E_i
\ge
5
$$

for every category.

---

# Python Example

```python
from scipy.stats import chisquare

observed = [8, 12, 9, 11, 10, 10]

expected = [10] * 6

statistic, p_value = chisquare(
    observed,
    f_exp=expected
)

print("Chi-square:", statistic)
print("p-value:", p_value)
```

---

# Manual NumPy Implementation

```python
import numpy as np

observed = np.array([8, 12, 9, 11, 10, 10])

expected = np.full(6, 10)

chi2 = np.sum((observed - expected) ** 2 / expected)

print(chi2)
```

---

# Common Mistakes

### Mistake 1

Using probabilities instead of counts.

The formula uses

$$
O_i
$$

and

$$
E_i,
$$

which are frequencies, **not probabilities**.

---

### Mistake 2

Expected counts too small.

If several categories have

$$
E_i<5,
$$

the Chi-Squared approximation becomes inaccurate.

Combine categories if necessary.

---

### Mistake 3

Interpreting "fail to reject" as "prove true."

Failing to reject

$$
H_0
$$

does **not** prove the distribution is correct.

It only means there is insufficient evidence against it.

---

### Mistake 4

Using continuous data without binning.

Continuous distributions must first be divided into intervals.

The test compares frequencies, not raw observations.

---

# Advantages

- Easy to compute.
- Works for many distributions.
- Widely used in statistics.
- Useful for testing pseudo-random number generators.
- Applicable whenever expected frequencies are known.

---

# Limitations

- Sensitive to sample size.
- Requires sufficiently large expected counts.
- Detects frequency differences only.
- Cannot detect serial correlation or dependence.

For example, the [[RANDU]] generator can pass a simple goodness-of-fit test while still failing geometric tests like the spectral test because its outputs lie on only 15 hyperplanes.

---

# Relationship to Other Tests

| Test | Purpose |
|---|---|
| [[Chi-Squared Goodness-of-Fit Test]] | Compare observed frequencies to a theoretical distribution |
| [[Chi-Squared Test of Independence]] | Determine whether two categorical variables are independent |
| [[Kolmogorov-Smirnov Test]] | Compare continuous distributions without binning |
| [[Anderson-Darling Test]] | More sensitive in the tails than the KS test |
| [[Runs Test]] | Detect dependence or non-random ordering |
| [[Serial Correlation Test]] | Detect dependence between successive observations |

---

# Key Takeaways

- The Chi-Squared Goodness-of-Fit test measures how closely observed frequencies match expected frequencies.

- The test statistic is

$$
\chi^2
=
\sum
\frac{(O_i-E_i)^2}{E_i}.
$$

- Small values indicate good agreement.

- Large values provide evidence against the hypothesized distribution.

- The test is commonly used for testing fairness of dice, coins, categorical models, and the uniformity of pseudo-random number generators.

- Passing the goodness-of-fit test **does not** guarantee high-quality randomness—additional tests such as serial correlation, runs tests, and spectral tests are also necessary.
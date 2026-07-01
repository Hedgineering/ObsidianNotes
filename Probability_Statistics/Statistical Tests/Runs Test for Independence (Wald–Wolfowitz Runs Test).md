# Runs Test for Independence (Wald–Wolfowitz Runs Test)

## Quick Reference

| Concept | Formula | Purpose |
|---|---|---|
| Run | Consecutive sequence of identical symbols | Measures clustering vs alternation |
| Null Hypothesis | $H_0:$ Observations are independent and randomly ordered | No serial dependence |
| Alternative Hypothesis | $H_a:$ Observations are not independent | Indicates non-random ordering |
| Expected Runs | $$E(R)=\frac{2n_1n_2}{n}+1$$ | Average number of runs under randomness |
| Variance | See below | Used to standardize the test statistic |
| Test Statistic | $$Z=\frac{R-E(R)}{\sqrt{\operatorname{Var}(R)}}$$ | Approximately standard normal for large samples |

---

# Intuition

The Runs Test asks:

> **"Does the order of the observations look random?"**

Unlike the [[Chi (Kai) Squared Goodness of Fit Test]], which only checks whether the **distribution** looks correct, the Runs Test checks whether the **ordering** looks random.

For example, both of these sequences contain five heads and five tails:

```
HHHHHTTTTT
```

```
HTHTHTHTHT
```

The frequencies are identical, but the ordering is clearly different.

The Runs Test detects this difference.

---

# What Is a Run?

A **run** is a maximal consecutive sequence of identical values.

Example:

```
HHHTTHHHHT
```

Group identical adjacent values:

```
HHH | TT | HHHH | T
```

There are

$$
R=4
$$

runs.

---

Another example:

```
HTHTHTHT
```

Every symbol starts a new run:

```
H | T | H | T | H | T | H | T
```

Thus

$$
R=8.
$$

---

Another example:

```
HHHHHTTTTT
```

There are only

```
HHHHH | TTTTT
```

so

$$
R=2.
$$

---

# Why Runs Matter

Too **few** runs means observations tend to cluster together.

Example:

```
111111111000000000
```

This suggests **positive serial correlation**.

---

Too **many** runs means observations alternate excessively.

Example:

```
1010101010101010
```

This suggests **negative serial correlation**.

---

A random sequence should have approximately the expected number of runs.

---

# Hypotheses

The null hypothesis is

$$
H_0:
\text{The observations are independent and randomly ordered.}
$$

The alternative hypothesis is

$$
H_a:
\text{The observations are not independent.}
$$

---

# Expected Number of Runs

Suppose

- $n_1$ observations belong to category 1,
- $n_2$ observations belong to category 2,

where

$$
n=n_1+n_2.
$$

The expected number of runs is

$$
E(R)
=
\frac{2n_1n_2}{n}
+
1.
$$

---

# Variance

The variance of the number of runs is

$$
\operatorname{Var}(R)
=
\frac{
2n_1n_2
\left(
2n_1n_2-n
\right)
}{
n^2(n-1)
}.
$$

---

# Test Statistic

For sufficiently large samples,

the standardized statistic is

$$
Z
=
\frac{
R-E(R)
}{
\sqrt{\operatorname{Var}(R)}
}.
$$

Under

$$
H_0,
$$

this approximately follows

$$
N(0,1).
$$

---

# Decision Rule

For a significance level

$$
\alpha,
$$

reject

$$
H_0
$$

if

$$
\vert Z\vert
>
z_{\alpha/2}.
$$

At

$$
\alpha=0.05,
$$

reject if

$$
\vert Z\vert
>
1.96.
$$

---

# Worked Example

Suppose we observe

```
H H T T H T T T H H
```

---

## Step 1

Identify runs.

```
HH | TT | H | TTT | HH
```

Thus

$$
R=5.
$$

---

## Step 2

Count each category.

Heads:

$$
n_1=5.
$$

Tails:

$$
n_2=5.
$$

Total:

$$
n=10.
$$

---

## Step 3

Expected runs.

$$
E(R)
=
\frac{2(5)(5)}{10}
+
1
=
6.
$$

---

## Step 4

Variance.

$$
\operatorname{Var}(R)
=
\frac{
2(5)(5)
(50-10)
}{
10^2(9)
}
=
2.22.
$$

Standard deviation:

$$
\sqrt{2.22}
=
1.49.
$$

---

## Step 5

Compute

$$
Z.
$$

$$
Z
=
\frac{5-6}{1.49}
=
-0.67.
$$

Since

$$
\vert-0.67\vert<1.96,
$$

we fail to reject

$$
H_0.
$$

The ordering appears consistent with randomness.

---

# Example of Too Few Runs

Sequence:

```
1111111100000000
```

Runs:

```
11111111 | 00000000
```

Only

$$
R=2.
$$

This indicates strong clustering.

Possible cause:

- positive serial correlation,
- non-random ordering.

---

# Example of Too Many Runs

Sequence:

```
1010101010101010
```

Runs:

```
1|0|1|0|1|0|...
```

Nearly every observation starts a new run.

This suggests excessive alternation,

which is also inconsistent with independence.

---

# Runs Test for Random Number Generators

The Runs Test is frequently applied to pseudo-random number generators.

Example:

Generate

$$
U_1,U_2,\ldots,U_n.
$$

Convert each value into

```
Above median
```

or

```
Below median.
```

For example,

| Value | Symbol |
|---:|---|
|0.71|H|
|0.23|L|
|0.82|H|
|0.44|L|
|0.61|H|

Then perform the Runs Test on the resulting binary sequence.

If there are significantly too many or too few runs,

the generator likely has serial dependence.

---

# Advantages

- Very easy to compute.
- Nonparametric (does not assume a specific distribution).
- Detects dependence in ordering.
- Useful for validating pseudo-random number generators.

---

# Limitations

- Detects only certain types of dependence.
- Does not measure the strength of correlation.
- Does not test whether the distribution itself is correct.
- Less powerful than specialized serial correlation tests for some alternatives.

---

# Python Example

```python
from statsmodels.sandbox.stats.runs import runstest_1samp

data = [
    0.71, 0.23, 0.82, 0.44,
    0.61, 0.33, 0.18, 0.77,
    0.59, 0.41
]

z_stat, p_value = runstest_1samp(data)

print(z_stat)
print(p_value)
```

---

# Manual Python Implementation

```python
import math

sequence = "HHTTHTTTHH"

# Count runs
runs = 1
for i in range(1, len(sequence)):
    if sequence[i] != sequence[i-1]:
        runs += 1

n1 = sequence.count("H")
n2 = sequence.count("T")
n = n1 + n2

expected = (2 * n1 * n2) / n + 1

variance = (
    2 * n1 * n2 * (2 * n1 * n2 - n)
) / (n**2 * (n - 1))

z = (runs - expected) / math.sqrt(variance)

print("Runs:", runs)
print("Expected:", expected)
print("Z:", z)
```

---

# Relationship to Other Randomness Tests

| Test                                       | Detects                                |
| ------------------------------------------ | -------------------------------------- |
| [[Chi (Kai) Squared Goodness of Fit Test]] | Incorrect distribution                 |
| Runs Test                                  | Non-random ordering                    |
| [[Serial Correlation Test]]                | Correlation between consecutive values |
| [[Spectral Test]]                          | Geometric structure of LCGs            |
| [[Kolmogorov-Smirnov Test]]                | Continuous distribution mismatch       |

---

# Common Mistakes

### Mistake 1

Thinking the Runs Test checks the distribution.

It only checks the **ordering**.

A sequence can have perfectly uniform frequencies while still failing the Runs Test.

---

### Mistake 2

Confusing too many runs with good randomness.

Both extremes are suspicious.

- Too few runs → clustering.
- Too many runs → excessive alternation.

Random sequences lie somewhere in between.

---

### Mistake 3

Using more than two categories.

The standard Runs Test assumes exactly **two** categories.

Continuous data are usually converted into two groups (e.g., above/below the median) before applying the test.

---

# Key Takeaways

- A **run** is a consecutive block of identical observations.
- The Runs Test determines whether observations are ordered randomly.
- Too few runs indicate clustering (positive dependence).
- Too many runs indicate excessive alternation (negative dependence).
- The test statistic compares the observed number of runs to the expected number under randomness:

$$
Z
=
\frac{
R-E(R)
}{
\sqrt{\operatorname{Var}(R)}
}.
$$

- The Runs Test complements the [[Chi (Kai) Squared Goodness of Fit Test]]: one checks **ordering**, while the other checks **distribution**.
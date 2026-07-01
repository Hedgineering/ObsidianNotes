# Runs Test Above and Below the Mean

## Quick Reference

| Concept | Formula | Purpose |
|---|---|---|
| Above Mean | $X_i>\bar X$ | Label as "+" |
| Below Mean | $X_i<\bar X$ | Label as "−" |
| Run | Consecutive sequence of identical "+" or "−" symbols | Measures clustering around the mean |
| Null Hypothesis | $H_0:$ Observations are independent | No serial dependence |
| Alternative Hypothesis | $H_a:$ Observations are not independent | Indicates clustering or oscillation |
| Expected Runs | $$E(R)=\frac{2n_1n_2}{n}+1$$ | Average number of runs under randomness |
| Variance | $$\operatorname{Var}(R)=\frac{2n_1n_2(2n_1n_2-n)}{n^2(n-1)}$$ | Used to standardize the statistic |
| Test Statistic | $$Z=\frac{R-E(R)}{\sqrt{\operatorname{Var}(R)}}$$ | Approximately standard normal |

---

# Intuition

The **Runs Test Above and Below the Mean** asks:

> **"Do observations occur randomly above and below the mean, or do they tend to cluster?"**

Instead of looking at whether values are increasing or decreasing, this test looks at **where each observation lies relative to the sample mean**.

If the observations are independent, points above and below the mean should be well mixed.

Long streaks above or below the mean suggest dependence.

---

# Difference from Other Runs Tests

| Test | Converts Data Into |
|---|---|
| [[Runs Test for Independence]] | Two categories (H/T, Success/Failure, etc.) |
| [[Runs Test Above and Below the Mean]] | Above or below the sample mean |
| [[Runs Test Up and Down]] | Increasing or decreasing |

---

# Step 1: Compute the Mean

Suppose the observations are

$$
4,\;
8,\;
7,\;
3,\;
6,\;
2,\;
9,\;
5.
$$

The sample mean is

$$
\bar X
=
\frac{4+8+7+3+6+2+9+5}{8}
=
5.5.
$$

---

# Step 2: Convert to "+" and "−"

Compare every observation to

$$
\bar X=5.5.
$$

| Observation | Symbol |
|---:|---|
|4|−|
|8|+|
|7|+|
|3|−|
|6|+|
|2|−|
|9|+|
|5|−|

The resulting sequence is

```
- + + - + - + -
```

---

# Step 3: Count Runs

Group consecutive identical symbols.

```
- | ++ | - | + | - | + | -
```

Therefore,

$$
R=7.
$$

---

# Hypotheses

The null hypothesis is

$$
H_0:
\text{The observations are independent.}
$$

The alternative hypothesis is

$$
H_a:
\text{The observations are not independent.}
$$

---

# Expected Number of Runs

Let

- $n_1$ = number of observations above the mean,
- $n_2$ = number below the mean,

where

$$
n=n_1+n_2.
$$

Then

$$
E(R)
=
\frac{2n_1n_2}{n}
+
1.
$$

---

# Variance

The variance is

$$
\operatorname{Var}(R)
=
\frac{
2n_1n_2
(2n_1n_2-n)
}{
n^2(n-1)
}.
$$

---

# Test Statistic

Compute

$$
Z
=
\frac{
R-E(R)
}{
\sqrt{\operatorname{Var}(R)}
}.
$$

For reasonably large samples,

$$
Z
\approx
N(0,1).
$$

---

# Decision Rule

Reject

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
\vert Z\vert>1.96.
$$

---

# Worked Example

Suppose

```
- + + - + - + -
```

contains

$$
n_1=4
$$

observations above the mean (+),

and

$$
n_2=4
$$

below the mean (-).

The observed number of runs is

$$
R=7.
$$

---

## Step 1

Expected runs:

$$
E(R)
=
\frac{2(4)(4)}{8}
+
1
=
5.
$$

---

## Step 2

Variance:

$$
\operatorname{Var}(R)
=
\frac{
2(4)(4)(32-8)
}{
8^2(7)
}
=
2.14.
$$

Standard deviation:

$$
\sqrt{2.14}
=
1.46.
$$

---

## Step 3

Compute

$$
Z.
$$

$$
Z
=
\frac{7-5}{1.46}
=
1.37.
$$

Since

$$
\vert1.37\vert<1.96,
$$

we fail to reject

$$
H_0.
$$

The sequence appears consistent with independence.

---

# Interpretation

## Too Few Runs

Example

```
+++++++--------
```

Only two runs.

This indicates observations tend to stay above or below the mean for long periods.

Possible causes:

- positive serial correlation,
- persistence,
- trend.

---

## Too Many Runs

Example

```
+-+-+-+-
```

Nearly every observation crosses the mean.

This indicates excessive alternation.

Possible causes:

- negative serial correlation,
- oscillatory behavior.

---

## Random Sequence

Example

```
++--+-+--
```

Produces approximately the expected number of runs.

Consistent with independence.

---

# Handling Values Equal to the Mean

If

$$
X_i=\bar X,
$$

most implementations either

- discard the observation,
- or use a predefined tie-breaking rule.

Observations exactly equal to the mean are generally **not** labeled "+" or "−".

---

# Why This Test Is Useful

Suppose an RNG produces

```
0.81
0.83
0.85
0.79
0.82
0.84
```

The histogram may look perfectly uniform over many samples,

yet long sequences above the mean indicate dependence.

The Runs Test Above and Below the Mean detects this type of behavior.

---

# Python Implementation

```python
import math
import numpy as np

data = np.array([
    4, 8, 7, 3,
    6, 2, 9, 5
])

mean = np.mean(data)

# Remove ties
filtered = data[data != mean]

symbols = [
    "+" if x > mean else "-"
    for x in filtered
]

# Count runs
runs = 1
for i in range(1, len(symbols)):
    if symbols[i] != symbols[i-1]:
        runs += 1

n1 = symbols.count("+")
n2 = symbols.count("-")
n = n1 + n2

expected = (2 * n1 * n2) / n + 1

variance = (
    2 * n1 * n2 * (2 * n1 * n2 - n)
) / (n**2 * (n - 1))

z = (runs - expected) / math.sqrt(variance)

print(symbols)
print(runs)
print(z)
```

---

# Advantages

- Simple to compute.
- Nonparametric.
- Detects clustering around the mean.
- Useful for checking independence of pseudo-random numbers.

---

# Limitations

- Detects only one form of dependence.
- Does not verify the distribution itself.
- Sensitive to how ties are handled.
- Less informative than full autocorrelation analysis.

---

# Relationship to Other Tests

| Test                                                      | Detects                                      |
| --------------------------------------------------------- | -------------------------------------------- |
| [[Chi (Kai) Squared Goodness of Fit Test]]                | Incorrect distribution                       |
| [[Runs Test Above and Below the Mean]]                    | Clustering around the mean                   |
| [[Runs Test Up and Down for Independence]]                | Trends and oscillation                       |
| [[Runs Test for Independence (Wald–Wolfowitz Runs Test)]] | Random ordering of binary categories         |
| [[Serial Correlation Test]]                               | Correlation between neighboring observations |

---

# Common Mistakes

### Mistake 1

Using the median instead of the mean.

This version of the Runs Test specifically uses the **sample mean** (some alternative versions use the median, but the formulas may differ).

---

### Mistake 2

Counting observations instead of runs.

Runs are **consecutive blocks** of "+" or "−", not the number of observations above or below the mean.

---

### Mistake 3

Ignoring ties.

Observations exactly equal to the mean should be handled consistently (usually removed before counting runs).

---

### Mistake 4

Interpreting only too few runs as evidence against randomness.

Both extremes indicate non-random behavior.

- Too few runs → clustering.
- Too many runs → excessive alternation.

---

# Key Takeaways

- Convert each observation into

```
+
```

or

```
−
```

depending on whether it is above or below the sample mean.

- Count the number of runs,

$$
R.
$$

- Under independence,

$$
E(R)
=
\frac{2n_1n_2}{n}
+
1,
$$

$$
\operatorname{Var}(R)
=
\frac{
2n_1n_2
(2n_1n_2-n)
}{
n^2(n-1)
}.
$$

- Compute

$$
Z
=
\frac{
R-E(R)
}{
\sqrt{\operatorname{Var}(R)}
}.
$$

- Reject

$$
H_0
$$

if

$$
\vert Z\vert
>
z_{\alpha/2}.
$$

- The Runs Test Above and Below the Mean complements the [[Chi (Kai) Squared Goodness of Fit Test]] by checking **independence of ordering**, not just the distribution of values.

> **Note:** Some references use
>
> $$
 E(R)=\frac{2n_1n_2}{n}+1,
 $$
>
> while others use
>
> $$
 E(R)=\frac{2n_1n_2}{n}+\frac12.
 $$
>
 The $+1$ formula is the **classical Wald–Wolfowitz Runs Test** and is the exact expected number of runs under the null hypothesis. The $+\frac12$ version appears in some simulation, engineering, and older statistics texts as part of a different normal approximation or continuity correction. **Neither is universally "correct"—they correspond to slightly different formulations of the test. For coursework and exams, always use the convention given by the instructor.**
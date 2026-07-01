
# Runs Test Up and Down

## Quick Reference

| Concept | Formula | Purpose |
|---|---|---|
| Up | $X_{i+1}>X_i$ | Record a "+" |
| Down | $X_{i+1}<X_i$ | Record a "−" |
| Run | Consecutive sequence of identical "+" or "−" symbols | Measures randomness in the direction of successive changes |
| Null Hypothesis | $H_0:$ Successive observations are independent | No trend or serial dependence |
| Alternative Hypothesis | $H_a:$ Successive observations are not independent | Indicates non-random ordering |
| Expected Runs | $$E(A)=\frac{2n-1}{3}$$ | Average number of runs under independence |
| Variance | $$\operatorname{Var}(A)=\frac{16n-29}{90}$$ | Used to standardize the test statistic |
| Test Statistic | $$Z_0=\frac{A-E(A)}{\sqrt{\operatorname{Var}(A)}}$$ | Approximately standard normal for large $n$ |

---

# Intuition

The **Runs Test Up and Down** checks whether a sequence randomly alternates between **increasing** and **decreasing**.

Instead of classifying observations into categories (like Heads/Tails), we classify **changes**.

For each consecutive pair,

- increase → "+"
- decrease → "−"

Then we count runs of "+" and "−".

If there are too many or too few runs, the sequence likely exhibits serial dependence.

---

# Difference from the Ordinary Runs Test

| Ordinary Runs Test | Runs Test Up and Down |
|---|---|
| Operates on categories (H/T, Above/Below Median, etc.) | Operates on increases and decreases |
| Tests whether categories occur randomly | Tests whether changes in direction occur randomly |
| Requires two categories | Works directly on ordered numerical data |

---

# Step 1: Compute the Signs

Suppose the observations are

$$
0.41,\;
0.68,\;
0.89,\;
0.84,\;
0.74,\;
0.91,\;
0.55,\;
0.71,\;
0.36,\;
0.30,\;
0.09.
$$

Compare each observation to the previous one.

| Comparison | Sign |
|---|---|
|$0.68>0.41$|+|
|$0.89>0.68$|+|
|$0.84<0.89$|−|
|$0.74<0.84$|−|
|$0.91>0.74$|+|
|$0.55<0.91$|−|
|$0.71>0.55$|+|
|$0.36<0.71$|−|
|$0.30<0.36$|−|
|$0.09<0.30$|−|

The resulting sign sequence is

```
+ + − − + − + − − −
```

---

# Step 2: Count the Runs

Group consecutive identical symbols.

```
++ | -- | + | - | + | ---
```

Therefore,

$$
A=6
$$

runs.

This is exactly the example from the lecture.

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

# Distribution of the Number of Runs

For sufficiently large samples,

$$
n\ge20,
$$

the number of runs

$$
A
$$

is approximately normally distributed.

Specifically,

$$
A
\sim
N\left(
\frac{2n-1}{3},
\;
\frac{16n-29}{90}
\right).
$$

This means

$$
E(A)
=
\frac{2n-1}{3},
$$

and

$$
\operatorname{Var}(A)
=
\frac{16n-29}{90}.
$$

---

# Test Statistic

Standardize the observed number of runs:

$$
Z_0
=
\frac{
A-E(A)
}{
\sqrt{\operatorname{Var}(A)}
}.
$$

For large samples,

$$
Z_0
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
\vert Z_0\vert
>
z_{\alpha/2}.
$$

For

$$
\alpha=0.05,
$$

reject if

$$
\vert Z_0\vert>1.96.
$$

Both unusually **large** and unusually **small** numbers of runs indicate non-randomness.

---

# Worked Example

Suppose

$$
n=20
$$

observations produce

$$
A=10
$$

runs.

---

## Step 1

Expected runs:

$$
E(A)
=
\frac{2(20)-1}{3}
=
13.
$$

---

## Step 2

Variance:

$$
\operatorname{Var}(A)
=
\frac{16(20)-29}{90}
=
\frac{291}{90}
=
3.233.
$$

Standard deviation:

$$
\sqrt{3.233}
=
1.798.
$$

---

## Step 3

Compute

$$
Z_0.
$$

$$
Z_0
=
\frac{10-13}{1.798}
=
-1.67.
$$

Since

$$
\vert-1.67\vert<1.96,
$$

we fail to reject

$$
H_0.
$$

The ordering is consistent with independence.

---

# Interpretation

### Too Few Runs

Example

```
+++++++-------
```

Only two runs.

This suggests long increasing or decreasing trends.

Possible causes:

- positive serial correlation,
- persistence,
- trend.

---

### Too Many Runs

Example

```
+-+-+-+-
```

Every comparison changes direction.

This suggests excessive oscillation.

Possible causes:

- negative serial correlation,
- alternating process,
- overcorrection.

---

### Random Sequence

Example

```
++--+-+---
```

Moderate number of runs.

Looks consistent with independence.

---

# Why This Test Is Useful

The Chi-Squared Goodness-of-Fit test checks

> "Are the values uniformly distributed?"

The Runs Test Up and Down asks

> "Are the values changing direction randomly?"

An RNG may pass the goodness-of-fit test while failing the Runs Test if it exhibits trends or oscillations.

---

# Python Implementation

```python
import math

data = [
    0.41, 0.68, 0.89, 0.84, 0.74,
    0.91, 0.55, 0.71, 0.36, 0.30,
    0.09
]

# Convert to + / -
signs = []

for i in range(1, len(data)):
    if data[i] > data[i-1]:
        signs.append("+")
    else:
        signs.append("-")

# Count runs
runs = 1

for i in range(1, len(signs)):
    if signs[i] != signs[i-1]:
        runs += 1

n = len(data)

expected = (2 * n - 1) / 3

variance = (16 * n - 29) / 90

z = (runs - expected) / math.sqrt(variance)

print("Signs:", "".join(signs))
print("Runs:", runs)
print("Expected:", expected)
print("Z:", z)
```

---

# Advantages

- Very easy to compute.
- Detects trends and oscillations.
- Nonparametric.
- Useful for testing pseudo-random number generators.
- Complements the [[Chi (Kai) Squared Goodness of Fit Test]].

---

# Limitations

- Only detects one type of dependence.
- Does not verify the underlying distribution.
- Requires a reasonably large sample for the normal approximation.

---

# Relationship to Other Randomness Tests

| Test                                                      | Detects                                       |
| --------------------------------------------------------- | --------------------------------------------- |
| [[Chi (Kai) Squared Goodness of Fit Test]]                | Incorrect distribution                        |
| [[Runs Test for Independence (Wald–Wolfowitz Runs Test)]] | Clustering of categories                      |
| [[Runs Test Up and Down for Independence]]                | Trends and oscillation in numerical sequences |
| [[Serial Correlation Test]]                               | Correlation between successive observations   |
| [[Spectral Test]]                                         | Geometric structure in LCGs                   |

---

# Common Mistakes

### Mistake 1

Counting observations instead of changes.

Runs are computed from the sequence of

```
+
```

and

```
−
```

symbols, **not** from the original values.

---

### Mistake 2

Ignoring ties.

If

$$
X_i=X_{i-1},
$$

most implementations either

- remove the tied observation,
- or follow a predefined tie-handling rule.

Ties should **not** simply be labeled "+" or "−".

---

### Mistake 3

Thinking only too few runs are problematic.

Both extremes indicate non-randomness.

- Too few runs → long trends.
- Too many runs → excessive alternation.

---

# Key Takeaways

- Convert consecutive differences into

```
+
```

or

```
−
```

depending on whether the sequence increases or decreases.

- Count the number of runs,

$$
A.
$$

- Under independence,

$$
E(A)
=
\frac{2n-1}{3},
$$

$$
\operatorname{Var}(A)
=
\frac{16n-29}{90}.
$$

- Compute

$$
Z_0
=
\frac{
A-E(A)
}{
\sqrt{\operatorname{Var}(A)}
}.
$$

- Reject

$$
H_0
$$

if

$$
\vert Z_0\vert
>
z_{\alpha/2}.
$$

- The Runs Test Up and Down is a simple yet powerful method for detecting trends, persistence, and oscillatory behavior in pseudo-random number sequences.
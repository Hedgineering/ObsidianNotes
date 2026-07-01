# Lag-1 Correlation Test (Serial Correlation Test)

## Quick Reference

| Concept | Formula | Purpose |
|---|---|---|
| Null Hypothesis | $H_0:\rho=0$ | Successive observations are independent |
| Alternative Hypothesis | $H_a:\rho\neq0$ | Successive observations are correlated |
| Lag-1 Correlation | $$\rho=\operatorname{Corr}(R_i,R_{i+1})$$ | Measures correlation between consecutive values |
| Estimator | $$\hat{\rho}=\frac{12}{n-1}\sum_{k=1}^{n-1}R_kR_{k+1}-3$$ | Estimates lag-1 correlation |
| Variance | $$\operatorname{Var}(\hat{\rho})=\frac{13n-19}{(n-1)^2}$$ | Under $H_0$ |
| Test Statistic | $$Z_0=\frac{\hat{\rho}}{\sqrt{\operatorname{Var}(\hat{\rho})}}$$ | Approximately standard normal |

---

# Intuition

The **Lag-1 Correlation Test** checks whether each random number is **independent of the next one**.

A high-quality pseudo-random number generator should produce numbers satisfying

$$
\operatorname{Corr}(R_i,R_{i+1})=0.
$$

That does **not** mean every adjacent pair is different.

Rather, knowing

$$
R_i
$$

should provide **no information** about

$$
R_{i+1}.
$$

---

# Why Is This Important?

Suppose a generator produces

```
0.10
0.20
0.30
0.40
0.50
```

Every number predicts the next one.

The lag-1 correlation is very close to

$$
1.
$$

This sequence is clearly **not random**.

A good generator should produce something like

```
0.81
0.17
0.42
0.95
0.08
0.61
```

where adjacent values show no systematic relationship.

---

# Hypotheses

The null hypothesis is

$$
H_0:
\rho=0,
$$

meaning consecutive observations are independent.

The alternative hypothesis is

$$
H_a:
\rho\neq0.
$$

---

# Lag-1 Correlation

The population lag-1 correlation is

$$
\rho
=
\operatorname{Corr}(R_i,R_{i+1}).
$$

Ideally,

$$
\rho=0.
$$

Positive values indicate persistence.

Negative values indicate alternating behavior.

---

# Estimator

A convenient estimator for the lag-1 correlation is

$$
\hat{\rho}
=
\left(
\frac{12}{n-1}
\sum_{k=1}^{n-1}
R_kR_{k+1}
\right)
-
3.
$$

where

- $n$ = sample size,
- $R_k$ = $k$th pseudo-random number.

For independent

$$
U(0,1)
$$

random variables,

$$
E(R_iR_{i+1})
=
\frac14,
$$

which makes the estimator centered around zero.

---

# Distribution Under the Null

If

$$
H_0
$$

is true,

then

$$
\hat{\rho}
\approx
N
\left(
0,
\frac{13n-19}{(n-1)^2}
\right).
$$

Therefore,

$$
E(\hat{\rho})=0,
$$

and

$$
\operatorname{Var}(\hat{\rho})
=
\frac{13n-19}{(n-1)^2}.
$$

---

# Test Statistic

Standardize the estimator:

$$
Z_0
=
\frac{
\hat{\rho}
}{
\sqrt{\operatorname{Var}(\hat{\rho})}
}.
$$

Under

$$
H_0,
$$

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

At

$$
\alpha=0.05,
$$

reject if

$$
\vert Z_0\vert
>
1.96.
$$

---

# Worked Example

Suppose

$$
n=30.
$$

After computing the estimator,

suppose we obtain

$$
\hat{\rho}
=
0.950.
$$

---

## Step 1

Compute the variance.

$$
\operatorname{Var}(\hat{\rho})
=
\frac{13(30)-19}{29^2}
=
0.441.
$$

---

## Step 2

Compute the standard deviation.

$$
\sqrt{0.441}
=
0.664.
$$

---

## Step 3

Compute the test statistic.

$$
Z_0
=
\frac{0.950}{0.664}
=
1.43.
$$

---

## Step 4

Decision.

Since

$$
\vert1.43\vert
<
1.96,
$$

we **fail to reject**

$$
H_0.
$$

The data are consistent with independence.

---

# Interpretation

## Positive Correlation

Example:

```
0.11
0.18
0.22
0.29
0.34
```

Large values tend to follow large values.

Small values tend to follow small values.

This produces

$$
\rho>0.
$$

---

## Negative Correlation

Example:

```
0.90
0.10
0.85
0.15
0.80
0.20
```

Large values tend to follow small values.

This produces

$$
\rho<0.
$$

---

## Independent Sequence

Example:

```
0.83
0.14
0.56
0.92
0.37
0.61
```

No obvious relationship exists between neighboring observations.

Thus,

$$
\rho\approx0.
$$

---

# Why Does the Formula Work?

For independent

$$
U(0,1)
$$

variables,

$$
E(R_i)
=
\frac12,
$$

and

$$
E(R_iR_{i+1})
=
E(R_i)E(R_{i+1})
=
\frac14.
$$

Substituting this into

$$
\hat{\rho}
=
\frac{12}{n-1}
\sum
R_kR_{k+1}
-
3
$$

gives

$$
12
\left(
\frac14
\right)
-
3
=
0.
$$

Thus the estimator is unbiased under the null hypothesis.

---

# Relationship to Other Randomness Tests

| Test                                                      | Detects                                            |
| --------------------------------------------------------- | -------------------------------------------------- |
| [[Chi (Kai) Squared Goodness of Fit Test]]                | Incorrect distribution                             |
| [[Runs Test Above and Below the Mean]]                    | Clustering around the mean                         |
| [[Runs Test Up and Down for Independence]]                | Trends and oscillation                             |
| [[Runs Test for Independence (Wald–Wolfowitz Runs Test)]] | Clustering of categories                           |
| **Lag-1 Correlation Test**                                | Linear dependence between consecutive observations |
| [[Spectral Test]]                                         | Geometric structure of LCGs                        |

---

# Python Implementation

```python
import numpy as np
from scipy.stats import norm

rng = np.random.default_rng(42)

R = rng.random(30)

n = len(R)

rho_hat = (
    12 / (n - 1)
) * np.sum(R[:-1] * R[1:]) - 3

variance = (13 * n - 19) / ((n - 1) ** 2)

z = rho_hat / np.sqrt(variance)

p_value = 2 * (1 - norm.cdf(abs(z)))

print("rho_hat =", rho_hat)
print("Variance =", variance)
print("Z =", z)
print("p-value =", p_value)
```

---

# Advantages

- Very easy to compute.
- Specifically detects first-order serial dependence.
- Useful for validating pseudo-random number generators.
- Simple normal approximation.

---

# Limitations

- Detects only **linear** dependence.
- Tests only **lag 1**.
- Higher-order dependence may go undetected.
- A generator can pass this test while failing the [[Spectral Test]] or [[Runs Test for Independence (Wald–Wolfowitz Runs Test)]].

---

# Common Mistakes

### Mistake 1

Thinking

$$
\rho=0
$$

implies true randomness.

Zero correlation only rules out **linear dependence**.

Nonlinear dependence may still exist.

---

### Mistake 2

Assuming one failed test proves a generator is unusable.

Randomness testing typically uses many tests together.

Passing one test is not sufficient.

---

### Mistake 3

Confusing independence with correlation.

Independent variables always have zero correlation.

The converse is **not** always true.

Zero correlation does **not** guarantee independence.

---

# Key Takeaways

- The Lag-1 Correlation Test checks whether consecutive observations are linearly related.

- A good pseudo-random number generator should satisfy

$$
\rho
=
\operatorname{Corr}(R_i,R_{i+1})
=
0.
$$

- Estimate the correlation using

$$
\hat{\rho}
=
\left(
\frac{12}{n-1}
\sum_{k=1}^{n-1}
R_kR_{k+1}
\right)
-
3.
$$

- Under

$$
H_0,
$$

$$
\hat{\rho}
\approx
N
\left(
0,
\frac{13n-19}{(n-1)^2}
\right).
$$

- Compute

$$
Z_0
=
\frac{
\hat{\rho}
}{
\sqrt{\operatorname{Var}(\hat{\rho})}
},
$$

and reject

$$
H_0
$$

if

$$
\vert Z_0\vert
>
z_{\alpha/2}.
$$

- The Lag-1 Correlation Test complements the [[Runs Test for Independence (Wald–Wolfowitz Runs Test)]] and [[Chi (Kai) Squared Goodness of Fit Test]] by directly measuring **linear dependence between adjacent pseudo-random numbers**.
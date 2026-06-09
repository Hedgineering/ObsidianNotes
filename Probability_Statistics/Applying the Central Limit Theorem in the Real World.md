Prereq:
- [[Central Limit Theorem (CLT)]]
## Applying the CLT in Practice

The CLT is useful because it lets us approximate the sampling distribution of an estimator by a Normal distribution, even when the underlying population distribution is unknown.

### Checklist Before Applying the CLT

Before using a Normal approximation, verify:

#### 1. Independence

Observations should be independent.

Examples where this is reasonable:

- Repeated measurements from separate experiments.
- Survey responses from randomly selected individuals.
- Independent Monte Carlo simulations.

Examples where it fails:

- Time series data.
- Repeated measurements on the same subject.
- Spatially correlated observations.

If independence fails, the CLT may not apply directly.

---

#### 2. Finite Variance

The CLT requires

$$
\operatorname{Var}(X_i)
<
\infty.
$$

Most common distributions satisfy this:

- Bernoulli
- Binomial
- Poisson
- Uniform
- Exponential
- Normal

Some heavy-tailed distributions do not:

- Certain Pareto distributions
- Cauchy distribution

For such distributions, the sample mean may not become Normal.

---

#### 3. Sufficiently Large Sample Size

A common rule is

$$
n \ge 30.
$$

However, the required sample size depends on the shape of the distribution.

| Distribution Shape | Typical Sample Size |
|----------|----------|
| Nearly symmetric | $20-30$ |
| Moderate skew | $50-100$ |
| Strong skew | $100+$ |
| Heavy tails | Hundreds or more |

There is no universal threshold.

---

### Using the CLT to Approximate Sample Means

If

$$
X_1,\dots,X_n
\overset{\text{i.i.d.}}{\sim}
(\mu,\sigma^2),
$$

then

$$
\bar X_n
\approx
N\!\left(
\mu,
\frac{\sigma^2}{n}
\right).
$$

This allows probability calculations such as

$$
P(a \le \bar X_n \le b).
$$

Compute

$$
Z
=
\frac{\bar X_n-\mu}
{\sigma/\sqrt n}
$$

and use the standard Normal distribution.

---

### Confidence Intervals

Suppose we observe a sample mean

$$
\bar x.
$$

For large samples,

$$
\bar X_n
\approx
N\!\left(
\mu,
\frac{\sigma^2}{n}
\right).
$$

A $95\%$ confidence interval is

$$
\bar x
\pm
1.96
\frac{\sigma}{\sqrt n}.
$$

If $\sigma$ is unknown, replace it with the sample standard deviation

$$
s.
$$

Then use

$$
\bar x
\pm
1.96
\frac{s}{\sqrt n}.
$$

for large $n$.

---

### Hypothesis Testing

To test

$$
H_0:\mu=\mu_0,
$$

compute

$$
Z
=
\frac{\bar x-\mu_0}
{\sigma/\sqrt n}.
$$

Under the null hypothesis,

$$
Z
\approx
N(0,1).
$$

Large positive or negative values provide evidence against $H_0$.

Most classical statistical tests rely on this idea.

---

### When the Population Variance is Unknown

In practice,

$$
\sigma
$$

is almost never known.

Estimate it using

$$
s^2
=
\frac{1}{n-1}
\sum_{i=1}^{n}
(X_i-\bar X_n)^2.
$$

The standardized statistic becomes

$$
T
=
\frac{\bar X_n-\mu}
{s/\sqrt n}.
$$

For small samples,

$$
T
\sim
t_{n-1}.
$$

This leads to Student's $t$ procedures.

---

### Small Sample Sizes

When $n$ is small:

- The CLT approximation may be poor.
- Confidence intervals may be inaccurate.
- Hypothesis tests may have incorrect error rates.

Possible solutions:

#### Use Exact Distributions

Examples:

- Binomial tests
- Fisher's exact test
- Exact Poisson methods

These do not rely on asymptotic approximations.

---

#### Use Student's $t$ Distribution

If the data are approximately Normal,

$$
T
=
\frac{\bar X_n-\mu}
{s/\sqrt n}
$$

follows a $t$ distribution.

This corrects for additional uncertainty from estimating $\sigma$.

---

#### Collect More Data

The most reliable solution is often increasing

$$
n.
$$

The CLT becomes more accurate as sample size grows.

---

### Bootstrap Methods

A powerful alternative when:

- Sample size is small.
- Distribution shape is unknown.
- Analytical formulas are difficult.

Procedure:

1. Sample with replacement from the observed data.
2. Compute the statistic.
3. Repeat thousands of times.
4. Use the empirical distribution of the statistic.

This approximates the sampling distribution directly.

Bootstrap methods often outperform crude Normal approximations.

---

### Skewed Data

Strong skew slows convergence to Normality.

Examples:

- Income distributions.
- Insurance claims.
- Waiting times.
- Stock trade sizes.

Possible remedies:

#### Increase Sample Size

The simplest solution.

---

#### Transform the Data

Common transformations:

$$
Y=\log(X)
$$

$$
Y=\sqrt X
$$

$$
Y=X^{1/3}
$$

These often reduce skewness and improve Normal approximations.

---

### Time Series Data

The classical CLT assumes independence.

Time series violate this assumption because nearby observations are correlated.

Examples:

- Stock returns.
- Weather measurements.
- Economic indicators.

In such settings:

- Use time-series-specific CLTs.
- Use HAC/Newey-West standard errors.
- Use block bootstrap methods.

Ignoring dependence typically underestimates uncertainty.

---

### Monte Carlo Applications

Suppose

$$
I=E[g(X)].
$$

Estimate it using

$$
\hat I_n
=
\frac1n
\sum_{i=1}^{n}
g(X_i).
$$

By the CLT,

$$
\hat I_n
\approx
N\!\left(
I,
\frac{\operatorname{Var}(g(X))}{n}
\right).
$$

This gives an approximate confidence interval:

$$
\hat I_n
\pm
1.96
\sqrt{
\frac{\widehat{\operatorname{Var}}(g(X))}
{n}
}.
$$

This is one of the most important practical uses of the CLT.

---

### Summary: Practical Decision Tree

If:

$$
n \text{ is large}
$$

and observations are independent with finite variance,

use the CLT.

If:

$$
n \text{ is small}
$$

and the population is approximately Normal,

use Student's $t$ methods.

If:

- the distribution is unknown,
- the sample is small,
- analytical formulas are unavailable,

use the bootstrap.

If:

- observations are dependent,
- data form a time series,

use methods designed for dependence rather than the classical CLT.

The CLT is extraordinarily robust, but always check independence, finite variance, and sample size before relying on a Normal approximation.
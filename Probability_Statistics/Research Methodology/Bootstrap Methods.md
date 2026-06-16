## Key Takeaways

1. Bootstrap methods estimate the sampling distribution of a statistic using repeated resampling from observed data.

2. The bootstrap is one of the most important tools in modern statistics because it often avoids difficult mathematical derivations.

3. The central idea:

   > Treat the observed sample as a proxy for the population.

4. Bootstrap methods can estimate:
   - Standard errors
   - Confidence intervals
   - Bias
   - Prediction uncertainty

5. Bootstrap is especially useful when:
   - The sampling distribution is unknown.
   - The statistic is complicated.
   - Analytical formulas are unavailable.

6. Common applications in quant finance:
   - Sharpe ratio confidence intervals
   - Strategy performance uncertainty
   - Portfolio risk estimation
   - Model stability analysis
   - Factor return uncertainty

7. Bootstrap does **not** create new information. It uses the information already present in the sample more efficiently.

---

# Quick Reference Table

| Concept | Formula / Idea | Purpose |
|----------|----------|----------|
| Original Sample | $X_1,\dots,X_n$ | Observed data |
| Bootstrap Sample | Sample with replacement from original data | Mimic repeated sampling |
| Bootstrap Replicate | $\theta^{*(b)}$ | Statistic from bootstrap sample |
| Bootstrap Distribution | Distribution of $\theta^*$ | Approximate sampling distribution |
| Bootstrap Standard Error | $\operatorname{sd}(\theta^*)$ | Estimate uncertainty |
| Percentile CI | Quantiles of bootstrap distribution | Confidence interval |
| Bias Estimate | $\bar\theta^*-\hat\theta$ | Estimate bias |

---

# Intuition

Suppose we observe:

$$
5,\;8,\;9,\;10,\;12
$$

and wish to estimate:

$$
\mu
$$

the population mean.

We compute:

$$
\bar X
=
8.8
$$

How uncertain is this estimate?

Normally we would derive:

$$
SE(\bar X)
=
\frac{\sigma}{\sqrt n}
$$

but what if:

- $\sigma$ is unknown?
- The estimator is complicated?
- The distribution is unusual?

Bootstrap provides a general solution.

---

# Core Idea

Suppose our sample is:

$$
X_1,\dots,X_n
$$

The empirical distribution is:

$$
\hat F_n
$$

which places probability

$$
\frac1n
$$

on each observation.

Bootstrap assumes:

$$
\hat F_n
\approx
F
$$

where $F$ is the true population distribution.

We repeatedly sample from:

$$
\hat F_n
$$

instead of the unknown population.

---

# Sampling With Replacement

Bootstrap samples are drawn **with replacement**.

This is critical.

---

## Example

Original sample:

$$
[5,8,9,10,12]
$$

Possible bootstrap sample:

$$
[5,5,8,10,12]
$$

Another:

$$
[12,12,12,8,5]
$$

Another:

$$
[9,9,9,9,10]
$$

Notice:

- Some observations repeat.
- Some observations disappear.

This mimics random sampling variation.

---

# Bootstrap Algorithm

Suppose we want uncertainty for statistic:

$$
\theta
$$

Examples:

- Mean
- Median
- Variance
- Sharpe ratio
- Regression coefficient

---

## Step 1

Observe data:

$$
X_1,\dots,X_n
$$

---

## Step 2

Compute statistic:

$$
\hat\theta
$$

---

## Step 3

Draw bootstrap sample:

$$
X_1^*,\dots,X_n^*
$$

with replacement.

---

## Step 4

Compute bootstrap statistic:

$$
\hat\theta^*
$$

---

## Step 5

Repeat

$$
B
$$

times.

Typical values:

$$
B=1000
$$

or

$$
B=10000
$$

---

## Step 6

Use resulting distribution:

$$
\hat\theta_1^*,
\hat\theta_2^*,
\dots,
\hat\theta_B^*
$$

to estimate uncertainty.

---

# Bootstrap Distribution

After many resamples:

$$
\hat\theta_1^*,
\hat\theta_2^*,
\dots,
\hat\theta_B^*
$$

form the bootstrap distribution.

This approximates:

$$
\text{Sampling Distribution of }
\hat\theta
$$

which is usually what we actually want.

---

# Example 1: Bootstrap Mean

Suppose:

$$
X=
[2,4,7,9]
$$

---

## Original Estimate

$$
\bar X
=
\frac{2+4+7+9}{4}
=
5.5
$$

---

## Bootstrap Samples

Sample 1:

$$
[2,2,7,9]
$$

Mean:

$$
5
$$

---

Sample 2:

$$
[4,4,7,9]
$$

Mean:

$$
6
$$

---

Sample 3:

$$
[2,9,9,9]
$$

Mean:

$$
7.25
$$

---

Continue thousands of times.

The resulting histogram approximates the sampling distribution of:

$$
\bar X
$$

---

# Bootstrap Standard Error

Suppose bootstrap estimates are:

$$
\theta_1^*,\dots,\theta_B^*
$$

Bootstrap mean:

$$
\bar\theta^*
=
\frac1B
\sum_{b=1}^{B}
\theta_b^*
$$

Bootstrap standard error:

$$
SE_{\text{boot}}
=
\sqrt{
\frac{
\sum_{b=1}^{B}
(\theta_b^*-\bar\theta^*)^2
}
{B-1}
}
$$

---

# Example 2: Standard Error

Suppose bootstrap means are:

$$
5.0,\;5.4,\;5.8,\;6.0,\;5.3
$$

Mean:

$$
\bar\theta^*
=
5.5
$$

Standard error:

$$
SE_{\text{boot}}
=
\sqrt{
\frac{
(5.0-5.5)^2
+
(5.4-5.5)^2
+
(5.8-5.5)^2
+
(6.0-5.5)^2
+
(5.3-5.5)^2
}
{4}
}
$$

$$
=
0.41
$$

Approximately.

---

# Bootstrap Confidence Intervals

One of the most common uses.

---

# Percentile Bootstrap Interval

The simplest approach.

---

## Procedure

Sort:

$$
\theta_1^*,
\dots,
\theta_B^*
$$

Take:

$$
\alpha/2
$$

and

$$
1-\alpha/2
$$

quantiles.

---

## 95% Interval

Take:

$$
2.5\%
$$

and

$$
97.5\%
$$

quantiles.

---

## Example

Suppose:

$$
10000
$$

bootstrap estimates are generated.

Sorted values yield:

$$
2.5\%
=
4.2
$$

$$
97.5\%
=
6.7
$$

Then

$$
95\%
\text{ CI}
=
(4.2,6.7)
$$

---

# Normal Approximation Bootstrap Interval

Use:

$$
\hat\theta
\pm
z_{\alpha/2}
SE_{\text{boot}}
$$

---

## Example

Estimate:

$$
\hat\theta=10
$$

Bootstrap standard error:

$$
SE_{\text{boot}}=1.5
$$

Then:

$$
10
\pm
1.96(1.5)
$$

$$
=
10
\pm
2.94
$$

giving

$$
(7.06,12.94)
$$

---

# Bootstrap Bias Estimation

Bias:

$$
\operatorname{Bias}
=
E[\hat\theta]
-
\theta
$$

Unknown in practice.

---

Bootstrap estimate:

$$
\widehat{\operatorname{Bias}}
=
\bar\theta^*
-
\hat\theta
$$

---

## Example

Original estimate:

$$
\hat\theta=20
$$

Bootstrap average:

$$
\bar\theta^*=21.2
$$

Estimated bias:

$$
1.2
$$

---

# Why Bootstrap Works

The sampling distribution depends on:

$$
F
$$

the true population distribution.

But:

$$
F
$$

is unknown.

Bootstrap replaces:

$$
F
$$

with

$$
\hat F_n
$$

the empirical distribution.

As

$$
n \to \infty
$$

we have:

$$
\hat F_n
\to
F
$$

which makes the bootstrap increasingly accurate.

---

# Nonparametric Bootstrap

The standard bootstrap.

No assumptions about distribution.

Resample directly from data.

---

# Parametric Bootstrap

Assume a model.

Example:

$$
X_i
\sim
N(\mu,\sigma^2)
$$

Estimate:

$$
\hat\mu,\hat\sigma
$$

Generate simulated observations from:

$$
N(\hat\mu,\hat\sigma^2)
$$

instead of resampling original data.

---

# Example: Regression Coefficients

Suppose:

$$
Y
=
\beta_0+\beta_1X+\varepsilon
$$

Interested in:

$$
\beta_1
$$

Procedure:

1. Resample observations.
2. Refit regression.
3. Store coefficient.
4. Repeat thousands of times.

Result:

Bootstrap distribution of:

$$
\hat\beta_1
$$

gives:

- Standard error
- Confidence interval
- Stability assessment

---

# Bootstrap for the Median

One major advantage:

The median has a complicated sampling distribution.

Analytical variance formulas are difficult.

Bootstrap handles it naturally:

1. Resample.
2. Compute median.
3. Repeat.
4. Use resulting distribution.

No special derivation required.

---

# Bootstrap in Quant Finance

## Example 1: Sharpe Ratio

Suppose daily returns:

$$
r_1,\dots,r_n
$$

Observed Sharpe:

$$
\hat S
=
\frac{\bar r}{s_r}
\sqrt{252}
$$

Bootstrap:

1. Resample returns.
2. Recompute Sharpe.
3. Repeat.

Obtain:

- Confidence interval
- Standard error
- Probability Sharpe exceeds threshold

---

## Example 2: Portfolio Returns

Resample historical returns.

Estimate uncertainty of:

$$
E[R]
$$

and

$$
\operatorname{Var}(R)
$$

---

## Example 3: Alpha Stability

Resample periods.

Estimate distribution of:

$$
\hat\alpha
$$

from a factor model.

---

# Bootstrap vs Cross Validation

| Bootstrap | Cross Validation |
|------------|------------|
| Estimates uncertainty | Estimates prediction performance |
| Resamples with replacement | Splits data into folds |
| Focuses on sampling distributions | Focuses on generalization |
| Used for CI and SE | Used for model evaluation |

---

# Limitations

## Small Samples

Very small samples may not adequately represent the population.

Bootstrap can then be misleading.

---

## Dependence

Standard bootstrap assumes observations are independent.

Time series data often violates this.

---

## Extreme Tails

Bootstrap may struggle with rare events because the sample contains few examples.

---

## Computational Cost

Requires:

$$
B
$$

model fits or statistic calculations.

Can become expensive for large models.

---

# Block Bootstrap for Time Series

Ordinary bootstrap destroys temporal structure.

Bad:

$$
[1,2,3,4,5]
\rightarrow
[4,1,5,2,2]
$$

This removes autocorrelation.

---

## Solution

Resample blocks:

$$
[1,2]
$$

$$
[3,4]
$$

$$
[5,6]
$$

instead of individual observations.

This preserves some dependence structure.

---

# Common Mistakes

## Mistake 1

Sampling without replacement.

That is not bootstrap.

---

## Mistake 2

Using too few bootstrap replications.

Common choices:

$$
B \ge 1000
$$

Preferred:

$$
B \ge 5000
$$

for stable confidence intervals.

---

## Mistake 3

Using ordinary bootstrap on time series data.

Use block bootstrap instead.

---

## Mistake 4

Assuming bootstrap fixes biased data.

If the original sample is biased:

$$
\text{Bootstrap}
\rightarrow
\text{Biased}
$$

---

# Relationship to Other Topics

Bootstrap methods are closely related to:

- [[Confidence Intervals]]
- [[Hypothesis Testing]]
- [[Cross Validation]]
- [[Experimental Design]]
- [[Central Limit Theorem (CLT)]]
- [[Law of Large Numbers (LLN)]]
- [[Monte Carlo Methods]]
- [[Regression Analysis]]
- [[Time Series Analysis]]

---

# Interview-Level Summary

Bootstrap methods estimate the sampling distribution of a statistic by repeatedly resampling the observed data with replacement. The resulting bootstrap distribution can be used to estimate standard errors, confidence intervals, and bias without requiring analytical formulas. Bootstrap methods are especially valuable when the statistic is complicated or the underlying distribution is unknown. In quant finance they are commonly used for Sharpe ratio confidence intervals, alpha stability analysis, portfolio risk estimation, and performance uncertainty assessment.
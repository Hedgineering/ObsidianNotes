## Key Takeaways

1. A confidence interval (CI) provides a range of plausible values for an unknown population parameter.

2. Confidence intervals quantify estimation uncertainty.

3. A narrower interval means a more precise estimate.

4. Increasing sample size $n$ decreases interval width.

5. A $95\%$ confidence interval does NOT mean:

   > "There is a 95% probability the parameter is inside the interval."

   The parameter is fixed; the interval is random.

6. Correct interpretation:

   > If we repeatedly collected samples and constructed intervals using the same procedure, approximately $95\%$ of those intervals would contain the true parameter.

7. Confidence intervals and hypothesis testing are closely related:

   - If a hypothesized value falls outside the CI, it would be rejected by the corresponding significance test.

---

# Quick Reference Table

| Parameter | Conditions | Confidence Interval |
|------------|------------|------------|
| Mean ($\mu$), $\sigma$ known | Normal or large $n$ | $\bar X \pm z_{\alpha/2}\frac{\sigma}{\sqrt n}$ |
| Mean ($\mu$), $\sigma$ unknown | Normal or large $n$ | $\bar X \pm t_{\alpha/2,n-1}\frac{S}{\sqrt n}$ |
| Proportion ($p$) | Large sample | $\hat p \pm z_{\alpha/2}\sqrt{\frac{\hat p(1-\hat p)}{n}}$ |
| Difference of Means | Independent samples | $(\bar X_1-\bar X_2)\pm \text{MOE}$ |
| Monte Carlo Estimate | Large $n$ | $\bar I \pm z_{\alpha/2}\frac{S_I}{\sqrt n}$ |

---

# Intuition

Suppose we want to estimate:

$$
\mu = \text{average height of all students}
$$

We cannot measure everyone.

Instead:

1. Take a sample.
2. Compute sample mean:

$$
\bar X
$$

3. Build an interval around $\bar X$.

Example:

$$
170 \pm 2
$$

giving

$$
(168,172)
$$

This interval reflects uncertainty caused by sampling variation.

---

# General Confidence Interval Form

Most confidence intervals look like:

$$
\text{Estimate}
\pm
\text{Margin of Error}
$$

or

$$
\text{CI}
=
\text{Estimate}
\pm
(\text{Critical Value})
\times
(\text{Standard Error})
$$

---

# Components

## Point Estimate

The statistic used to estimate the parameter.

Examples:

Population mean:

$$
\mu
\quad \rightarrow \quad
\bar X
$$

Population proportion:

$$
p
\quad \rightarrow \quad
\hat p
$$

---

## Standard Error

Measures variability of the estimator.

For a sample mean:

$$
SE(\bar X)
=
\frac{\sigma}{\sqrt n}
$$

or

$$
SE(\bar X)
=
\frac{S}{\sqrt n}
$$

when $\sigma$ is unknown.

---

## Critical Value

Determined by confidence level.

### Common $z$ Values

| Confidence Level | Critical Value |
|------------|------------|
| 90% | 1.645 |
| 95% | 1.96 |
| 99% | 2.576 |

---

# Why the Formula Works

By the [[Central Limit Theorem (CLT)]],

$$
\bar X
\approx
N
\left(
\mu,
\frac{\sigma^2}{n}
\right)
$$

Therefore

$$
Z
=
\frac{\bar X-\mu}
{\sigma/\sqrt n}
\sim N(0,1)
$$

and

$$
P
\left(
-z_{\alpha/2}
<
Z
<
z_{\alpha/2}
\right)
=
1-\alpha
$$

Rearranging yields

$$
P
\left(
\bar X
-
z_{\alpha/2}
\frac{\sigma}{\sqrt n}
<
\mu
<
\bar X
+
z_{\alpha/2}
\frac{\sigma}{\sqrt n}
\right)
=
1-\alpha
$$

which is the confidence interval.

---

# Confidence Interval for a Mean (Known Variance)

## Formula

When population variance is known:

$$
\mu
\in
\bar X
\pm
z_{\alpha/2}
\frac{\sigma}{\sqrt n}
$$

---

# Example 1: Known Variance

Suppose:

$$
\bar X = 50
$$

$$
\sigma = 10
$$

$$
n = 100
$$

Construct a $95\%$ confidence interval.

---

## Step 1: Compute Standard Error

$$
SE
=
\frac{10}{\sqrt{100}}
=
1
$$

---

## Step 2: Find Critical Value

For $95\%$ confidence:

$$
z_{0.025}
=
1.96
$$

---

## Step 3: Compute Margin of Error

$$
ME
=
1.96(1)
=
1.96
$$

---

## Step 4: Construct Interval

$$
50
\pm
1.96
$$

$$
(48.04,51.96)
$$

---

# Confidence Interval for a Mean (Unknown Variance)

In practice:

$$
\sigma
\text{ is usually unknown}
$$

Use the sample standard deviation:

$$
S
$$

and the Student's $t$ distribution.

---

## Formula

$$
\mu
\in
\bar X
\pm
t_{\alpha/2,n-1}
\frac{S}{\sqrt n}
$$

---

# Why Use the t Distribution?

Replacing $\sigma$ with $S$ introduces additional uncertainty.

The $t$ distribution accounts for this.

Properties:

- Similar to normal distribution
- Heavier tails
- Approaches normal as $n \to \infty$

---

# Example 2: Unknown Variance

Suppose:

$$
\bar X = 80
$$

$$
S = 12
$$

$$
n = 25
$$

Construct a $95\%$ confidence interval.

---

## Step 1: Standard Error

$$
SE
=
\frac{12}{\sqrt{25}}
=
2.4
$$

---

## Step 2: Critical Value

Degrees of freedom:

$$
df
=
24
$$

Table value:

$$
t_{0.025,24}
\approx
2.064
$$

---

## Step 3: Margin of Error

$$
ME
=
2.064(2.4)
$$

$$
=
4.954
$$

---

## Step 4: Confidence Interval

$$
80
\pm
4.954
$$

$$
(75.05,84.95)
$$

---

# Confidence Interval for a Proportion

Suppose:

$$
X
\sim
\text{Binomial}(n,p)
$$

Estimate:

$$
\hat p
=
\frac{X}{n}
$$

---

## Standard Error

$$
SE(\hat p)
=
\sqrt{
\frac{\hat p(1-\hat p)}
{n}
}
$$

---

## Confidence Interval

$$
\hat p
\pm
z_{\alpha/2}
\sqrt{
\frac{\hat p(1-\hat p)}
{n}
}
$$

---

# Example 3: Proportion

Survey:

$$
n=500
$$

People favoring candidate:

$$
X=290
$$

Estimate:

$$
\hat p
=
\frac{290}{500}
=
0.58
$$

---

## Step 1: Standard Error

$$
SE
=
\sqrt{
\frac{0.58(0.42)}
{500}
}
$$

$$
=
0.0221
$$

---

## Step 2: Margin of Error

$$
ME
=
1.96(0.0221)
$$

$$
=
0.0433
$$

---

## Step 3: Interval

$$
0.58
\pm
0.0433
$$

$$
(0.537,0.623)
$$

---

# Confidence Interval for Difference of Means

Suppose:

$$
\mu_1
=
\text{Treatment Mean}
$$

$$
\mu_2
=
\text{Control Mean}
$$

Estimate:

$$
\bar X_1-\bar X_2
$$

---

## Formula

Large-sample version:

$$
(\bar X_1-\bar X_2)
\pm
z_{\alpha/2}
\sqrt{
\frac{S_1^2}{n_1}
+
\frac{S_2^2}{n_2}
}
$$

---

# Example 4: Comparing Strategies

Strategy A:

$$
\bar X_A = 12
$$

$$
S_A = 8
$$

$$
n_A = 100
$$

Strategy B:

$$
\bar X_B = 9
$$

$$
S_B = 7
$$

$$
n_B = 100
$$

---

## Step 1: Difference

$$
12-9
=
3
$$

---

## Step 2: Standard Error

$$
SE
=
\sqrt{
\frac{8^2}{100}
+
\frac{7^2}{100}
}
$$

$$
=
\sqrt{1.13}
$$

$$
=
1.063
$$

---

## Step 3: Margin of Error

$$
ME
=
1.96(1.063)
$$

$$
=
2.083
$$

---

## Step 4: Interval

$$
3
\pm
2.083
$$

$$
(0.917,5.083)
$$

Since zero is NOT inside the interval:

$$
0 \notin (0.917,5.083)
$$

there is evidence of a difference.

---

# Monte Carlo Confidence Intervals

Common in simulation and quant finance.

Suppose:

$$
I_1,\ldots,I_n
$$

are simulation outputs.

Estimate:

$$
\bar I_n
=
\frac1n
\sum_{i=1}^n I_i
$$

Sample variance:

$$
S_I^2
=
\frac{1}{n-1}
\sum_{i=1}^n
(I_i-\bar I_n)^2
$$

---

## Confidence Interval

Using the CLT:

$$
\bar I_n
\pm
z_{\alpha/2}
\frac{S_I}{\sqrt n}
$$

---

# Example 5: Monte Carlo Integration

Suppose:

$$
\bar I_n = 0.742
$$

$$
S_I = 0.31
$$

$$
n=1000
$$

---

## Step 1: Standard Error

$$
SE
=
\frac{0.31}{\sqrt{1000}}
$$

$$
=
0.00980
$$

---

## Step 2: Margin of Error

$$
ME
=
1.96(0.00980)
$$

$$
=
0.0192
$$

---

## Step 3: Interval

$$
0.742
\pm
0.0192
$$

$$
(0.723,0.761)
$$

---

# Confidence Intervals in Quant Finance

Common uses:

- Average strategy return
- Sharpe ratio estimation
- Information ratio estimation
- Factor premia
- Alpha estimation
- Forecast accuracy

Example:

Estimate annualized alpha:

$$
\hat\alpha
=
3.2\%
$$

with CI:

$$
(1.0\%,5.4\%)
$$

Interpretation:

A positive alpha remains plausible across the entire interval.

---

# Relationship to Hypothesis Testing

A two-sided hypothesis test

$$
H_0:\mu=\mu_0
$$

at significance level $\alpha$

is equivalent to checking whether:

$$
\mu_0
$$

lies inside the

$$
100(1-\alpha)\%
$$

confidence interval.

---

## Example

Suppose:

$$
95\%
\text{ CI}
=
(2.1,5.4)
$$

Testing:

$$
H_0:\mu=0
$$

Since

$$
0 \notin (2.1,5.4)
$$

reject $H_0$ at the $5\%$ significance level.

---

# Common Mistakes

## Mistake 1

Interpreting:

> "There is a 95% probability the parameter is inside."

Incorrect.

---

## Mistake 2

Using a $z$ interval when variance is unknown and sample size is small.

Use $t$ instead.

---

## Mistake 3

Confusing standard deviation and standard error.

Standard deviation:

$$
S
$$

Standard error:

$$
\frac{S}{\sqrt n}
$$

---

## Mistake 4

Ignoring assumptions.

Most confidence intervals rely on:

- Random sampling
- Independence
- CLT or normality assumptions

---

# Interview-Level Summary

A confidence interval is a range of plausible values for an unknown population parameter. Most confidence intervals take the form

$$
\text{Estimate}
\pm
(\text{Critical Value})
\times
(\text{Standard Error})
$$

The width reflects uncertainty in the estimate. Larger samples produce narrower intervals because standard error decreases like

$$
\frac{1}{\sqrt n}.
$$

The most common intervals are for means, proportions, differences in means, and Monte Carlo estimates. Confidence intervals provide both statistical significance information and a practical measure of estimation precision.
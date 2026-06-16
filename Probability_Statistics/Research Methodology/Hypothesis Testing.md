## Quick Reference Table

| Test | Used For | Null Hypothesis | Common Interpretation |
|---|---|---|---|
| One-sample z-test | Test a population mean when population variance is known or sample size is large | $H_0:\mu=\mu_0$ | Tests whether a sample mean differs from a claimed population mean |
| One-sample t-test | Test a population mean when population variance is unknown | $H_0:\mu=\mu_0$ | Like a z-test, but uses sample standard deviation |
| Two-sample t-test | Compare means of two independent groups | $H_0:\mu_1=\mu_2$ | Tests whether two groups have different average values |
| Paired t-test | Compare before/after measurements on the same subjects | $H_0:\mu_D=0$ | Tests whether average paired difference is zero |
| One-proportion z-test | Test a population proportion | $H_0:p=p_0$ | Tests whether a sample proportion differs from a claimed proportion |
| Two-proportion z-test | Compare two population proportions | $H_0:p_1=p_2$ | Tests whether two groups have different success rates |
| Chi-square goodness-of-fit test | Compare observed counts to expected counts | $H_0:\text{data follows claimed distribution}$ | Tests whether categorical frequencies match a proposed distribution |
| Chi-square test of independence | Test relationship between two categorical variables | $H_0:\text{variables are independent}$ | Tests whether two categorical variables are associated |
| ANOVA | Compare means across more than two groups | $H_0:\mu_1=\mu_2=\cdots=\mu_k$ | Tests whether at least one group mean differs |
| F-test | Compare two variances | $H_0:\sigma_1^2=\sigma_2^2$ | Tests whether two populations have equal variance |
| Ljung-Box test | Test autocorrelation in time series residuals | $H_0:\text{no autocorrelation}$ | Tests whether residuals behave like white noise |
| Durbin-Watson test | Test first-order autocorrelation in regression residuals | $H_0:\rho=0$ | Tests whether residuals are serially correlated |
| Augmented Dickey-Fuller test | Test for unit root / nonstationarity | $H_0:\text{series has a unit root}$ | Low p-value suggests stationarity |
| KPSS test | Test for stationarity | $H_0:\text{series is stationary}$ | Low p-value suggests nonstationarity |
| Granger causality test | Test whether one time series helps predict another | $H_0:X\text{ does not Granger-cause }Y$ | Low p-value suggests past $X$ helps predict $Y$ |
| Jarque-Bera test | Test normality using skewness and kurtosis | $H_0:\text{data is normally distributed}$ | Low p-value suggests non-normality |

---

# Big Picture

Hypothesis testing is a formal way to decide whether observed data provides enough evidence against a default assumption.

The default assumption is called the null hypothesis.

The competing claim is called the alternative hypothesis.

We usually write:

$$
H_0 = \text{null hypothesis}
$$

$$
H_A = \text{alternative hypothesis}
$$

or

$$
H_1 = \text{alternative hypothesis}
$$

---

# Core Idea

Hypothesis testing asks:

> If the null hypothesis were true, how surprising would our observed data be?

If the data would be very unlikely under the null hypothesis, we reject the null.

If the data is not very unlikely under the null hypothesis, we fail to reject the null.

Important:

Failing to reject $H_0$ does not prove $H_0$ is true.

It only means we do not have enough evidence against it.

---

# Main Ingredients of a Hypothesis Test

A hypothesis test usually involves:

1. Define the null hypothesis $H_0$.
2. Define the alternative hypothesis $H_A$.
3. Choose a significance level $\alpha$.
4. Compute a test statistic.
5. Compute a p-value.
6. Compare the p-value to $\alpha$.
7. Make a conclusion in context.

---

# Significance Level

The significance level is the threshold for rejecting the null hypothesis.

Common choices:

$$
\alpha = 0.10
$$

$$
\alpha = 0.05
$$

$$
\alpha = 0.01
$$

The most common choice is

$$
\alpha = 0.05
$$

A smaller $\alpha$ means stronger evidence is required to reject $H_0$.

---

# P-Value

The p-value is the probability, assuming $H_0$ is true, of observing a result at least as extreme as the one we observed.

Small p-value:

- Data is surprising under $H_0$
- Evidence against $H_0$
- Reject $H_0$ if $p \le \alpha$

Large p-value:

- Data is not very surprising under $H_0$
- Not enough evidence against $H_0$
- Fail to reject $H_0$ if $p > \alpha$

Decision rule:

$$
p \le \alpha
\implies
\text{Reject } H_0
$$

$$
p > \alpha
\implies
\text{Fail to reject } H_0
$$

---

# Type I and Type II Errors

## Type I Error

A Type I error occurs when we reject a true null hypothesis.

$$
\text{Type I Error}
=
\text{False Positive}
$$

Probability of Type I error:

$$
P(\text{Type I Error}) = \alpha
$$

---

## Type II Error

A Type II error occurs when we fail to reject a false null hypothesis.

$$
\text{Type II Error}
=
\text{False Negative}
$$

Probability of Type II error:

$$
P(\text{Type II Error}) = \beta
$$

---

## Power

Power is the probability of correctly rejecting a false null hypothesis.

$$
\text{Power}
=
1-\beta
$$

Higher power means the test is better at detecting real effects.

Power increases when:

- Sample size increases
- Effect size increases
- Noise decreases
- Significance level $\alpha$ increases

---

# One-Sided vs Two-Sided Tests

## Two-Sided Test

Use when we care whether a parameter is different in either direction.

Example:

$$
H_0:\mu=\mu_0
$$

$$
H_A:\mu \ne \mu_0
$$

---

## Right-Tailed Test

Use when we care whether a parameter is greater than some value.

$$
H_0:\mu=\mu_0
$$

$$
H_A:\mu>\mu_0
$$

---

## Left-Tailed Test

Use when we care whether a parameter is less than some value.

$$
H_0:\mu=\mu_0
$$

$$
H_A:\mu<\mu_0
$$

---

# One-Sample Z-Test

## Description

A one-sample z-test is used to test whether a sample mean differs from a claimed population mean when the population standard deviation is known or the sample size is large.

## Hypotheses

$$
H_0:\mu=\mu_0
$$

$$
H_A:\mu \ne \mu_0
$$

## Test Statistic

$$
z
=
\frac{\bar{x}-\mu_0}
{\sigma/\sqrt{n}}
$$

where:

- $\bar{x}$ is the sample mean
- $\mu_0$ is the hypothesized mean
- $\sigma$ is the population standard deviation
- $n$ is the sample size

## Example

A company claims its light bulbs last

$$
1000
$$

hours on average.

A sample of

$$
n=64
$$

bulbs has mean lifetime

$$
\bar{x}=980
$$

Assume population standard deviation

$$
\sigma=80
$$

Test at

$$
\alpha=0.05
$$

whether the true mean differs from $1000$.

### Solution

Hypotheses:

$$
H_0:\mu=1000
$$

$$
H_A:\mu \ne 1000
$$

Test statistic:

$$
z
=
\frac{980-1000}
{80/\sqrt{64}}
$$

$$
z
=
\frac{-20}
{10}
$$

$$
z
=
-2
$$

For a two-sided test,

$$
p
=
2P(Z<-2)
$$

Using a standard normal table,

$$
P(Z<-2)
\approx
0.0228
$$

Therefore,

$$
p
=
2(0.0228)
=
0.0456
$$

Since

$$
0.0456 < 0.05
$$

we reject $H_0$.

Conclusion:

There is statistically significant evidence that the true mean lifetime differs from $1000$ hours.

---

# One-Sample T-Test

## Description

A one-sample t-test is used to test a population mean when the population standard deviation is unknown.

It is very common because in practice $\sigma$ is usually unknown.

## Hypotheses

$$
H_0:\mu=\mu_0
$$

$$
H_A:\mu \ne \mu_0
$$

## Test Statistic

$$
t
=
\frac{\bar{x}-\mu_0}
{s/\sqrt{n}}
$$

where:

- $s$ is the sample standard deviation
- degrees of freedom are $n-1$

## Example

A sample of

$$
n=16
$$

students has average test score

$$
\bar{x}=82
$$

with sample standard deviation

$$
s=8
$$

Test whether the population mean differs from

$$
78
$$

at

$$
\alpha=0.05
$$

### Solution

Hypotheses:

$$
H_0:\mu=78
$$

$$
H_A:\mu \ne 78
$$

Test statistic:

$$
t
=
\frac{82-78}
{8/\sqrt{16}}
$$

$$
t
=
\frac{4}
{2}
$$

$$
t
=
2
$$

Degrees of freedom:

$$
df
=
16-1
=
15
$$

A two-sided t-test with $t=2$ and $df=15$ has p-value slightly above $0.05$.

Approximate p-value:

$$
p
\approx
0.064
$$

Since

$$
0.064 > 0.05
$$

we fail to reject $H_0$.

Conclusion:

There is not enough evidence at the $5\%$ level to say the true mean differs from $78$.

---

# Two-Sample T-Test

## Description

A two-sample t-test compares the means of two independent groups.

Use this when the groups are separate.

Examples:

- Treatment group vs control group
- Men vs women
- Two different machines
- Two different trading strategies

## Hypotheses

$$
H_0:\mu_1=\mu_2
$$

$$
H_A:\mu_1 \ne \mu_2
$$

Equivalently,

$$
H_0:\mu_1-\mu_2=0
$$

$$
H_A:\mu_1-\mu_2 \ne 0
$$

## Welch's T-Test Statistic

Welch's t-test is commonly used when variances may be unequal.

$$
t
=
\frac{\bar{x}_1-\bar{x}_2}
{
\sqrt{
\frac{s_1^2}{n_1}
+
\frac{s_2^2}{n_2}
}
}
$$

## Example

Group 1:

$$
n_1=25,\quad \bar{x}_1=10,\quad s_1=4
$$

Group 2:

$$
n_2=25,\quad \bar{x}_2=13,\quad s_2=5
$$

Test whether the means differ.

### Solution

Hypotheses:

$$
H_0:\mu_1=\mu_2
$$

$$
H_A:\mu_1 \ne \mu_2
$$

Test statistic:

$$
t
=
\frac{10-13}
{
\sqrt{
\frac{4^2}{25}
+
\frac{5^2}{25}
}
}
$$

$$
t
=
\frac{-3}
{
\sqrt{
\frac{16}{25}
+
\frac{25}{25}
}
}
$$

$$
t
=
\frac{-3}
{\sqrt{0.64+1}}
$$

$$
t
=
\frac{-3}
{\sqrt{1.64}}
$$

$$
t
\approx
\frac{-3}{1.28}
$$

$$
t
\approx
-2.34
$$

This gives evidence that the means may differ.

For a two-sided test at $\alpha=0.05$, this would usually be significant depending on degrees of freedom.

Conclusion:

The sample provides evidence that the two group means are different.

---

# Paired T-Test

## Description

A paired t-test is used when observations naturally come in pairs.

Examples:

- Before and after treatment
- Same person measured twice
- Matched pairs
- Same asset before and after an event

Instead of comparing two independent means, we compute differences.

## Setup

Let

$$
D_i
=
X_{i,\text{after}}-X_{i,\text{before}}
$$

Then test:

$$
H_0:\mu_D=0
$$

$$
H_A:\mu_D \ne 0
$$

## Test Statistic

$$
t
=
\frac{\bar{d}-0}
{s_D/\sqrt{n}}
$$

## Example

Five people have before/after scores.

| Person | Before | After |
|---|---:|---:|
| 1 | 70 | 75 |
| 2 | 80 | 82 |
| 3 | 65 | 70 |
| 4 | 90 | 92 |
| 5 | 75 | 80 |

Differences:

$$
D
=
5,2,5,2,5
$$

Mean difference:

$$
\bar{d}
=
\frac{5+2+5+2+5}{5}
=
3.8
$$

Suppose the sample standard deviation of differences is

$$
s_D
\approx
1.64
$$

Test whether the average change differs from zero.

### Solution

Hypotheses:

$$
H_0:\mu_D=0
$$

$$
H_A:\mu_D \ne 0
$$

Test statistic:

$$
t
=
\frac{3.8}
{1.64/\sqrt{5}}
$$

$$
t
\approx
\frac{3.8}
{0.733}
$$

$$
t
\approx
5.19
$$

This is large in magnitude.

Conclusion:

There is strong evidence that the average after-score differs from the average before-score.

---

# One-Proportion Z-Test

## Description

A one-proportion z-test is used to test a population proportion.

Examples:

- Conversion rate
- Election support
- Defect rate
- Click-through rate

## Hypotheses

$$
H_0:p=p_0
$$

$$
H_A:p \ne p_0
$$

## Test Statistic

$$
z
=
\frac{\hat{p}-p_0}
{\sqrt{\frac{p_0(1-p_0)}{n}}}
$$

## Example

A website claims its conversion rate is

$$
p_0=0.10
$$

In a sample of

$$
n=500
$$

visitors,

$$
60
$$

convert.

Test whether the conversion rate differs from $10\%$.

### Solution

Sample proportion:

$$
\hat{p}
=
\frac{60}{500}
=
0.12
$$

Hypotheses:

$$
H_0:p=0.10
$$

$$
H_A:p \ne 0.10
$$

Test statistic:

$$
z
=
\frac{0.12-0.10}
{\sqrt{\frac{0.10(0.90)}{500}}}
$$

$$
z
=
\frac{0.02}
{\sqrt{0.00018}}
$$

$$
z
=
\frac{0.02}
{0.0134}
$$

$$
z
\approx
1.49
$$

For a two-sided test,

$$
p
\approx
0.136
$$

Since

$$
0.136 > 0.05
$$

we fail to reject $H_0$.

Conclusion:

There is not enough evidence to say the conversion rate differs from $10\%$.

---

# Chi-Square Goodness-of-Fit Test

## Description

A chi-square goodness-of-fit test compares observed counts to expected counts under a claimed distribution.

Examples:

- Is a die fair?
- Are website visits evenly distributed across weekdays?
- Do categories follow expected market shares?

## Hypotheses

$$
H_0:\text{observed distribution matches expected distribution}
$$

$$
H_A:\text{observed distribution does not match expected distribution}
$$

## Test Statistic

$$
\chi^2
=
\sum_i
\frac{(O_i-E_i)^2}{E_i}
$$

where:

- $O_i$ is observed count
- $E_i$ is expected count

## Example

A die is rolled

$$
60
$$

times.

Observed counts:

| Face | Observed |
|---|---:|
| 1 | 8 |
| 2 | 9 |
| 3 | 10 |
| 4 | 11 |
| 5 | 12 |
| 6 | 10 |

If the die is fair, expected count for each face is

$$
E_i
=
\frac{60}{6}
=
10
$$

### Solution

Compute:

$$
\chi^2
=
\frac{(8-10)^2}{10}
+
\frac{(9-10)^2}{10}
+
\frac{(10-10)^2}{10}
+
\frac{(11-10)^2}{10}
+
\frac{(12-10)^2}{10}
+
\frac{(10-10)^2}{10}
$$

$$
\chi^2
=
\frac{4}{10}
+
\frac{1}{10}
+
0
+
\frac{1}{10}
+
\frac{4}{10}
+
0
$$

$$
\chi^2
=
1.0
$$

Degrees of freedom:

$$
df
=
6-1
=
5
$$

A chi-square statistic of $1.0$ with $5$ degrees of freedom is not large.

Conclusion:

We fail to reject $H_0$.

There is not enough evidence to say the die is unfair.

---

# Chi-Square Test of Independence

## Description

A chi-square test of independence tests whether two categorical variables are associated.

Examples:

- Gender and product preference
- Region and political preference
- Device type and conversion
- Industry and default status

## Hypotheses

$$
H_0:\text{the variables are independent}
$$

$$
H_A:\text{the variables are associated}
$$

## Expected Counts

For each cell,

$$
E_{ij}
=
\frac{
(\text{row total})(\text{column total})
}{
\text{grand total}
}
$$

## Test Statistic

$$
\chi^2
=
\sum_i
\sum_j
\frac{(O_{ij}-E_{ij})^2}{E_{ij}}
$$

## Interpretation

Small p-value:

- Reject independence
- Evidence of association

Large p-value:

- Fail to reject independence
- Not enough evidence of association

---

# ANOVA

## Description

ANOVA stands for analysis of variance.

It is used to compare means across more than two groups.

Examples:

- Compare average sales across three marketing campaigns
- Compare average returns across multiple strategies
- Compare average yields across different fertilizer types

## Hypotheses

$$
H_0:
\mu_1
=
\mu_2
=
\cdots
=
\mu_k
$$

$$
H_A:
\text{at least one group mean is different}
$$

## Test Statistic

ANOVA uses an F-statistic:

$$
F
=
\frac{
\text{between-group variability}
}{
\text{within-group variability}
}
$$

## Interpretation

Large F-statistic:

- Group means are far apart relative to within-group noise
- Evidence against $H_0$

Small F-statistic:

- Group means are not far apart relative to within-group noise
- Weak evidence against $H_0$

Important:

ANOVA tells us that at least one mean differs.

It does not tell us which means differ.

For that, use post-hoc comparisons such as Tukey's HSD.

---

# F-Test for Equal Variances

## Description

An F-test compares the variances of two populations.

## Hypotheses

$$
H_0:\sigma_1^2=\sigma_2^2
$$

$$
H_A:\sigma_1^2 \ne \sigma_2^2
$$

## Test Statistic

$$
F
=
\frac{s_1^2}{s_2^2}
$$

Usually, the larger sample variance is placed in the numerator so that

$$
F \ge 1
$$

## Interpretation

Large F-statistic:

- Sample variances are very different
- Evidence against equal population variances

Small F-statistic near $1$:

- Sample variances are similar
- Weak evidence against $H_0$

---

# Hypothesis Tests in Regression

## T-Test for Regression Coefficients

In regression, we often test whether a coefficient is zero.

For coefficient $\beta_j$:

$$
H_0:\beta_j=0
$$

$$
H_A:\beta_j \ne 0
$$

Test statistic:

$$
t
=
\frac{\hat{\beta}_j - 0}
{SE(\hat{\beta}_j)}
$$

Interpretation:

If the p-value is small, the predictor has statistically significant linear association with the response, controlling for other variables in the model.

---

## F-Test for Overall Regression

The overall F-test checks whether the regression model explains significant variation.

For a regression with predictors $X_1,\ldots,X_k$:

$$
H_0:
\beta_1=\beta_2=\cdots=\beta_k=0
$$

$$
H_A:
\text{at least one } \beta_j \ne 0
$$

Interpretation:

Small p-value means at least one predictor is useful for explaining the response.

---

# Time Series Hypothesis Tests

Time series data often violates the independence assumptions of standard hypothesis tests.

Common time series concerns:

- Autocorrelation
- Nonstationarity
- Unit roots
- Seasonality
- Structural breaks
- Heteroskedasticity
- Residual whiteness

---

# Ljung-Box Test

## Description

The Ljung-Box test checks whether a time series has autocorrelation up to a chosen number of lags.

It is commonly used on model residuals.

After fitting an ARIMA or other time series model, residuals should behave like white noise.

## Hypotheses

$$
H_0:
\text{no autocorrelation up to lag } h
$$

$$
H_A:
\text{autocorrelation exists at one or more lags}
$$

## Interpretation

Small p-value:

- Reject $H_0$
- Residuals are autocorrelated
- Model may be missing time-series structure

Large p-value:

- Fail to reject $H_0$
- No strong evidence of autocorrelation
- Residuals look more like white noise

## Example

Suppose we fit an ARIMA model and run a Ljung-Box test on residuals up to lag $10$.

The result is:

$$
p=0.03
$$

At

$$
\alpha=0.05
$$

since

$$
0.03 < 0.05
$$

we reject $H_0$.

Conclusion:

There is evidence that the residuals are autocorrelated. The model may not have captured all time dependence.

---

# Durbin-Watson Test

## Description

The Durbin-Watson test checks for first-order autocorrelation in regression residuals.

It is especially common in regression with time-ordered data.

## Hypotheses

$$
H_0:\rho=0
$$

$$
H_A:\rho \ne 0
$$

where $\rho$ is the lag-1 residual autocorrelation.

## Test Statistic

$$
DW
=
\frac{
\sum_{t=2}^{T}(e_t-e_{t-1})^2
}{
\sum_{t=1}^{T}e_t^2
}
$$

## Interpretation

The Durbin-Watson statistic is approximately between $0$ and $4$.

Rules of thumb:

| DW Value | Interpretation |
|---|---|
| Near $2$ | No first-order autocorrelation |
| Less than $2$ | Positive autocorrelation |
| Greater than $2$ | Negative autocorrelation |
| Near $0$ | Strong positive autocorrelation |
| Near $4$ | Strong negative autocorrelation |

---

# Augmented Dickey-Fuller Test

## Description

The Augmented Dickey-Fuller test, or ADF test, checks whether a time series has a unit root.

A unit root usually means the series is nonstationary.

Many financial prices and economic series are nonstationary in levels.

## Hypotheses

$$
H_0:
\text{series has a unit root}
$$

$$
H_A:
\text{series is stationary}
$$

## Interpretation

Small p-value:

- Reject $H_0$
- Evidence against a unit root
- Series is likely stationary

Large p-value:

- Fail to reject $H_0$
- Not enough evidence against a unit root
- Series may be nonstationary

## Example

Suppose an ADF test gives:

$$
p=0.01
$$

At

$$
\alpha=0.05
$$

since

$$
0.01 < 0.05
$$

we reject $H_0$.

Conclusion:

There is evidence that the series is stationary.

---

# KPSS Test

## Description

The KPSS test is another stationarity test, but its hypotheses are reversed compared to the ADF test.

This makes it useful as a complement to ADF.

## Hypotheses

$$
H_0:
\text{series is stationary}
$$

$$
H_A:
\text{series is nonstationary}
$$

## Interpretation

Small p-value:

- Reject $H_0$
- Evidence that the series is nonstationary

Large p-value:

- Fail to reject $H_0$
- Not enough evidence against stationarity

## ADF vs KPSS

| ADF Result | KPSS Result | Interpretation |
|---|---|---|
| Reject unit root | Fail to reject stationarity | Strong evidence of stationarity |
| Fail to reject unit root | Reject stationarity | Strong evidence of nonstationarity |
| Reject unit root | Reject stationarity | Mixed result; possible trend stationarity or structural break |
| Fail to reject unit root | Fail to reject stationarity | Inconclusive |

---

# Granger Causality Test

## Description

A Granger causality test checks whether past values of one time series improve prediction of another time series.

Despite the name, Granger causality is not true causal proof.

It is predictive causality.

## Hypotheses

To test whether $X$ Granger-causes $Y$:

$$
H_0:
X \text{ does not Granger-cause } Y
$$

$$
H_A:
X \text{ Granger-causes } Y
$$

## Interpretation

Small p-value:

- Reject $H_0$
- Past values of $X$ help predict $Y$

Large p-value:

- Fail to reject $H_0$
- Not enough evidence that $X$ helps predict $Y$

## Important Warning

Granger causality does not prove real-world causation.

It only says that lagged values of one variable improve forecasts of another variable.

There may be:

- Confounding variables
- Shared trends
- Spurious correlation
- Nonstationarity issues

---

# Jarque-Bera Test

## Description

The Jarque-Bera test checks whether data is normally distributed based on skewness and kurtosis.

It is often used on regression or time series residuals.

## Hypotheses

$$
H_0:
\text{data is normally distributed}
$$

$$
H_A:
\text{data is not normally distributed}
$$

## Interpretation

Small p-value:

- Reject normality
- Evidence of skewness, excess kurtosis, or both

Large p-value:

- Fail to reject normality
- Not enough evidence against normality

---

# ARCH Test

## Description

The ARCH test checks for autoregressive conditional heteroskedasticity.

This means it tests whether volatility changes over time in a predictable way.

It is common in financial time series.

## Hypotheses

$$
H_0:
\text{no ARCH effects}
$$

$$
H_A:
\text{ARCH effects are present}
$$

## Interpretation

Small p-value:

- Reject $H_0$
- Evidence of time-varying volatility
- A GARCH-type model may be appropriate

Large p-value:

- Fail to reject $H_0$
- No strong evidence of time-varying volatility

---

# Phillips-Perron Test

## Description

The Phillips-Perron test is another unit root test.

Like the ADF test, it tests for nonstationarity due to a unit root.

## Hypotheses

$$
H_0:
\text{series has a unit root}
$$

$$
H_A:
\text{series is stationary}
$$

## Interpretation

Small p-value:

- Reject unit root
- Evidence of stationarity

Large p-value:

- Fail to reject unit root
- Series may be nonstationary

---

# Cointegration Tests

## Description

Cointegration tests are used when individual time series are nonstationary, but some linear combination of them is stationary.

This matters in time series because two nonstationary series can appear highly correlated even when the relationship is spurious.

Cointegration suggests a long-run equilibrium relationship.

## Engle-Granger Test

Common for two time series.

Hypotheses:

$$
H_0:
\text{no cointegration}
$$

$$
H_A:
\text{cointegration exists}
$$

Interpretation:

Small p-value:

- Reject no cointegration
- Evidence of a long-run equilibrium relationship

Large p-value:

- Fail to reject no cointegration
- No strong evidence of cointegration

---

# Structural Break Tests

## Description

Structural break tests check whether the relationship in a model changes at some point in time.

Examples:

- Policy change
- Market regime change
- Financial crisis
- Product launch
- COVID-era behavior shift

## Chow Test

The Chow test is used when the suspected break date is known.

Hypotheses:

$$
H_0:
\text{no structural break}
$$

$$
H_A:
\text{structural break exists}
$$

Interpretation:

Small p-value:

- Reject $H_0$
- Evidence that parameters changed around the break date

Large p-value:

- Fail to reject $H_0$
- No strong evidence of a structural break

---

# Multiple Testing Problem

If many hypothesis tests are performed, some will be significant just by chance.

For example, with

$$
\alpha=0.05
$$

about

$$
5\%
$$

of true null hypotheses are expected to be rejected by random chance.

This is called the multiple testing problem.

## Bonferroni Correction

If performing $m$ tests, use adjusted significance level:

$$
\alpha_{\text{adjusted}}
=
\frac{\alpha}{m}
$$

This is conservative but simple.

## False Discovery Rate

False discovery rate methods control the expected proportion of false positives among rejected hypotheses.

A common method is the Benjamini-Hochberg procedure.

---

# Confidence Intervals and Hypothesis Tests

Two-sided hypothesis tests are closely related to confidence intervals.

For example, at significance level

$$
\alpha=0.05
$$

a two-sided hypothesis test corresponds to a

$$
95\%
$$

confidence interval.

If the null value is outside the confidence interval, reject $H_0$.

If the null value is inside the confidence interval, fail to reject $H_0$.

---

# Statistical Significance vs Practical Significance

A result can be statistically significant but practically unimportant.

With a large enough sample size, very small effects can have tiny p-values.

Always ask:

- Is the effect statistically significant?
- Is the effect large enough to matter?
- Is the result practically useful?
- Is the model assumption reasonable?

---

# Common Assumptions

Many hypothesis tests rely on assumptions.

Common assumptions include:

- Random sampling
- Independence
- Approximate normality
- Equal variances
- Stationarity for time series tests
- No major outliers
- Correct model specification

If assumptions fail, the test result may be misleading.

---

# Common Mistakes

## Mistake 1: Saying the p-value is the probability that $H_0$ is true

Wrong:

$$
p = P(H_0 \text{ is true})
$$

Correct:

The p-value is the probability of observing data this extreme or more extreme, assuming $H_0$ is true.

---

## Mistake 2: Saying fail to reject means accept $H_0$

Wrong:

$$
\text{Fail to reject } H_0
=
\text{prove } H_0
$$

Correct:

Failing to reject means there is not enough evidence against $H_0$.

---

## Mistake 3: Ignoring Effect Size

A tiny effect can be statistically significant if the sample size is huge.

A large effect can fail to be significant if the sample size is tiny.

---

## Mistake 4: Testing After Looking at the Data

If hypotheses are created after inspecting the data, p-values can be misleading.

This is related to:

- Data snooping
- P-hacking
- Multiple comparisons
- Overfitting

---

## Mistake 5: Ignoring Time Series Dependence

Standard tests often assume independence.

Time series observations are frequently autocorrelated.

Using ordinary tests on autocorrelated data can produce misleading p-values.

---

# General Hypothesis Testing Template

## Step 1: State Hypotheses

$$
H_0:
\text{default assumption}
$$

$$
H_A:
\text{claim we are testing for}
$$

## Step 2: Choose Significance Level

Commonly,

$$
\alpha=0.05
$$

## Step 3: Compute Test Statistic

Generic form:

$$
\text{test statistic}
=
\frac{
\text{estimate} - \text{null value}
}{
\text{standard error}
}
$$

## Step 4: Compute P-Value

The p-value measures how extreme the test statistic is under $H_0$.

## Step 5: Make Decision

$$
p \le \alpha
\implies
\text{Reject } H_0
$$

$$
p > \alpha
\implies
\text{Fail to reject } H_0
$$

## Step 6: Interpret in Context

Always state the result using the language of the original problem.

---

# Key Takeaways

Hypothesis testing evaluates whether observed data is surprising under a null hypothesis.

The null hypothesis is the default assumption:

$$
H_0
$$

The alternative hypothesis is the competing claim:

$$
H_A
$$

The p-value is computed assuming $H_0$ is true.

Reject $H_0$ when:

$$
p \le \alpha
$$

Fail to reject $H_0$ when:

$$
p > \alpha
$$

A small p-value means evidence against $H_0$.

A large p-value means insufficient evidence against $H_0$.

For time series, always pay attention to:

- Autocorrelation
- Stationarity
- Unit roots
- Structural breaks
- Time-varying volatility
- Residual diagnostics

The goal is not just to get a significant p-value.

The goal is to make a statistically valid and practically meaningful conclusion.
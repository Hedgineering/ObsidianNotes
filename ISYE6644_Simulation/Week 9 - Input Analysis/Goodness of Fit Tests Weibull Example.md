#statistics #hypothesis-testing #goodness-of-fit #chi-squared #weibull #newtons-method

# Goodness-of-Fit Test: Weibull Example (Two Unknown Parameters)

Companion note to [[Goodness-of-Fit Tests]] and [[Goodness-of-Fit Tests - Exponential Example]] — this one steps things up to a **two-parameter** distribution, which forces us to use **Newton's method** to get the MLEs.

---

## Setup

$$ H_0 : X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Weibull}(r, \lambda) $$

The Weibull has c.d.f. $$ F(x) = 1 - \exp!\left[-(\lambda x)^r\right], \qquad x \ge 0 $$

> Note that $r = 1$ yields the $\mathrm{Exp}(\lambda)$ as a special case — see [[Goodness-of-Fit Tests - Exponential Example]].

Again we'll do a $\chi^2$ g-o-f test with **equal-probability intervals**. We need $a_i$ such that

$$ F(a_i) = 1 - e^{-(\lambda a_i)^r} = i/k \quad\Longrightarrow\quad a_i = \frac{1}{\lambda}\left[-\ln!\left(1-\frac{i}{k}\right)\right]^{1/r}, \qquad i=1,\dots,k $$

---

## Step 1 — Set Up the Likelihood

Now $\lambda$ **and** $r$ are both unknown, so we'll need $s = 2$ MLEs.

The p.d.f. is $$ f(x) = \lambda r (\lambda x)^{r-1} e^{-(\lambda x)^r}, \qquad x \ge 0 $$

Likelihood for an i.i.d. sample of size $n$: $$ L(r,\lambda) = \prod_{i=1}^n f(x_i) = \lambda^{nr} r^n \prod_{i=1}^n x_i^{r-1}\exp!\left[-\lambda^r\sum_{i=1}^n x_i^r\right] $$

Log-likelihood: $$ \ln(L) = n\ln(r) + (r-1)\ln!\left(\prod_{i=1}^n x_i\right) + nr\ln(\lambda) - \lambda^r \sum_{i=1}^n x_i^r $$

---

## Step 2 — Maximize with Respect to $r$ and $\lambda$

Set $$ \frac{\partial}{\partial r}\ln(L) = 0 \qquad\text{and}\qquad \frac{\partial}{\partial \lambda}\ln(L) = 0 $$

After tons of algebra, this gives:

$$ \lambda = \left(\sum_{i=1}^n x_i^r\right)^{-1/r} $$

and $r$ solves

$$ g(r) = \frac{n}{r} + \sum_{i=1}^n \ln(x_i) - \frac{n\sum_i x_i^r \ln(x_i)}{\sum_i x_i^r} = 0 $$

> The equation for $\lambda$ looks easy enough — **if only we could solve for $r$ first!** The equation for $r$ has no closed form, so we need a numerical root-finder.

---

## Step 3 — Solve for $r$ via Newton's Method

**Recall: ways to solve for a zero:**

- trial-and-error — blech.
- bisection — OK.
- **Newton's method** — let's use this one.

To use Newton's method we also need the derivative:

$$ g'(r) = -\frac{n}{r^2} - \frac{n\sum_i x_i^r[\ln(x_i)]^2}{\sum_i x_i^r} + \frac{n\left[\sum_i x_i^r \ln(x_i)\right]^2}{\left[\sum_i x_i^r\right]^2} $$

### Newton's Method Algorithm

1. **Initialize** $\hat r_0 = \bar X / S$, where $\bar X$ is the sample mean and $S^2$ is the sample variance.
2. **Update:** $$ \hat r_j \leftarrow \hat r_{j-1} - \frac{g(\hat r_{j-1})}{g'(\hat r_{j-1})} $$
3. **Stop condition:** if $|g(\hat r_{j-1})| < 0.001$, stop and set the MLE $\hat r = \hat r_j$. Otherwise, let $j \leftarrow j+1$ and go back to Step 2.

> Newton's method usually converges pretty quickly — maybe after 3 or 4 iterations.

---

## Step 4 — Recover $\hat\lambda$ and the Equal-Probability Points

Given the MLE $\hat r$, we immediately get the MLE for $\lambda$:

$$ \hat\lambda = \left(\sum_{i=1}^n x_i^{\hat r}\right)^{-1/\hat r} $$

Then, by the **Invariance Property**, we finally get the MLEs for the equal-probability interval endpoints:

$$ \hat a_i = \frac{1}{\hat\lambda}\left[-\ln!\left(1-\frac{i}{k}\right)\right]^{1/\hat r}, \qquad i = 1, 2, \dots, k $$

---

## Worked Example (from BCNN)

Suppose we take $n = 50$ observations and divide them into $k = 8$ equal-probability intervals. Suppose the Newton's-method fit gives

$$ \hat r = 0.525, \qquad \hat\lambda = 0.161 $$

Then the interval endpoints are:

$$ \hat a_i = 6.23\left[-\ln!\left(1-\frac{i}{8}\right)\right]^{1.905}, \qquad i = 1, 2, \dots, 8 $$

(Here $1/\hat\lambda = 1/0.161 \approx 6.23$ and $1/\hat r = 1/0.525 \approx 1.905$.)

### Tally the Observations

|$(\hat a_{i-1}, \hat a_i]$|$O_i$|$E_i = n/k$|
|---|---|---|
|$(0, 0.134]$|6|6.25|
|$(0.134, 0.578]$|5|6.25|
|$\vdots$|$\vdots$|$\vdots$|
|$(11.54, 24.97]$|5|6.25|
|$(24.97, \infty)$|6|6.25|
|**Total**|**50**|**50**|

### Test Statistic and Decision

$$ \chi_0^2 = \sum_{i=1}^k \frac{(O_i-E_i)^2}{E_i} = 1.20 $$

- $k = 8$ intervals, $s = 2$ parameters estimated ($r$ and $\lambda$)
- degrees of freedom: $k - 1 - s = 8 - 1 - 2 = 5$
- at $\alpha = 0.05$: $\chi^2_{\alpha,k-1-s} = \chi^2_{0.05,5} = 11.1$

Since $\chi_0^2 = 1.20 \le 11.1 = \chi^2_{0.05,5}$, we **"accept"** $H_0$ (fail to reject).

**Conclusion:** these observations are sort of Weibull. $\square$

---

## Summary / Key Takeaways

- The Weibull generalizes the Exponential (which is the special case $r=1$), adding a **shape parameter** $r$ on top of the scale parameter $\lambda$.
- With **two unknown parameters**, we get $s=2$ MLEs, which reduces the degrees of freedom for the chi-squared test to $k-1-2$.
- The MLE equation for $\lambda$ has a closed form **given** $r$, but the equation for $r$ itself does **not** — it must be solved numerically.
- **Newton's method** is a practical way to solve $g(r) = 0$:
    - initialize with a sensible starting guess (e.g. $\hat r_0 = \bar X/S$),
    - iterate $\hat r_j \leftarrow \hat r_{j-1} - g(\hat r_{j-1})/g'(\hat r_{j-1})$,
    - stop once $|g(\hat r_{j-1})|$ is small (e.g. $< 0.001$).
- Once $\hat r$ and $\hat\lambda$ are found, use the **Invariance Property of MLEs** to directly get the equal-probability cutpoints $\hat a_i$, then run the usual chi-squared g-o-f test.

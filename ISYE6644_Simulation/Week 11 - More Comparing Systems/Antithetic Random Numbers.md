#statistics #simulation #output-analysis #variance-reduction #antithetic-random-numbers

# Antithetic Random Numbers (ARN)

> **Last time:** introduced the common random numbers (CRN) variance reduction technique. **This time:** the **antithetic random numbers** method, which intentionally introduces **negative** correlation between estimators. Very useful for estimating certain means.

---

## 1. The Idea

**Opposite of CRN.** Suppose $\hat\theta_1$ and $\hat\theta_2$ are i.i.d. **unbiased** estimators for some parameter $\theta$.

- **CRN** tries to induce **positive** correlation between two _different systems'_ estimators, to shrink the variance of their _difference_.
- **ARN** instead tries to induce **negative** correlation between two runs of the **same** estimation problem, to shrink the variance of their _average_.

If we can induce **negative correlation** between $\hat\theta_1$ and $\hat\theta_2$, then the average of the two, $(\hat\theta_1+\hat\theta_2)/2$, is **also unbiased** (average of two unbiased estimators is unbiased) and may have **very low variance**.

---

## 2. Why It Works: The Variance Calculation

$$\mathrm{Var}\left(\frac{\hat\theta_1+\hat\theta_2}{2}\right) = \frac{1}{4}\Big[\mathrm{Var}(\hat\theta_1) + \mathrm{Var}(\hat\theta_2) + 2,\mathrm{Cov}(\hat\theta_1,\hat\theta_2)\Big]$$

Since $\hat\theta_1,\hat\theta_2$ are identically distributed, $\mathrm{Var}(\hat\theta_1)=\mathrm{Var}(\hat\theta_2)$, so this simplifies to:

$$= \frac{1}{2}\Big[\mathrm{Var}(\hat\theta_1) + \mathrm{Cov}(\hat\theta_1,\hat\theta_2)\Big]$$

**If we successfully induce negative correlation** ($\mathrm{Cov}(\hat\theta_1,\hat\theta_2) < 0$), then:

$$< \frac{\mathrm{Var}(\hat\theta_1)}{2} \quad (\leftarrow \text{"usual" avg of two independent reps!})$$

➡️ The antithetic average has **lower variance than the usual average of two independent replications** — again, a "something for nothing" result, but achieved via **negative** correlation this time (contrast with CRN, which exploited **positive** correlation to shrink the variance of a _difference_).

---

## 3. Worked Example: Monte Carlo Integration

**Goal:** estimate $I = \displaystyle\int_1^2 \frac{1}{x},dx$, using ARN to obtain a nice variance reduction.

(True answer: $I = \ln(2) \approx 0.693$.)

### Step 1 — Ordinary Monte Carlo estimate

Using the general Monte Carlo integration formula, with $g(x) = 1/x$, $a=1$, $b=2$: $$\hat\theta_1 = \bar I_n = \frac{b-a}{n}\sum_{i=1}^n g\big(a+(b-a)U_i\big) = \frac{1}{5}\sum_{i=1}^5 g(1+U_i) = \frac{1}{5}\sum_{i=1}^5 \frac{1}{1+U_i}$$

Using $n=5$ Unif(0,1) draws: $$0.85,\quad 0.53,\quad 0.98,\quad 0.12,\quad 0.45$$

Result: $$\hat\theta_1 = 0.6563 \quad \text{(not bad)}$$

### Step 2 — Antithetic estimate

Use the **antithetic random numbers** — literally the "opposite" of the original PRNs, i.e., $1-U_i$ for each $U_i$ above:

$$0.15,\quad 0.47,\quad 0.02,\quad 0.88,\quad 0.55$$

(Notice: each pair $(U_i, 1-U_i)$ sums to exactly 1 — that's the defining feature of antithetic pairs for Uniform(0,1) variates.)

**Antithetic version of the estimator:** $$\hat\theta_2 = \frac{1}{5}\sum_{i=1}^5 \frac{1}{1+(1-U_i)} = 0.7475 \quad \text{(also not bad)}$$

### Step 3 — Average the two

$$\frac{\hat\theta_1 + \hat\theta_2}{2} = \frac{0.6563+0.7475}{2} = 0.6989$$

**Result:** much closer to the true answer of $\approx 0.693$ than either $\hat\theta_1$ or $\hat\theta_2$ alone!

- $\hat\theta_1 = 0.6563$ → error $\approx 0.037$
- $\hat\theta_2 = 0.7475$ → error $\approx 0.054$
- Average $= 0.6989$ → error $\approx 0.006$

The averaging didn't just "split the difference" — it **canceled out** much of the error, because $g(x)=1/x$ is monotonic, so overshooting with $U_i$ tends to correspond to undershooting with $1-U_i$ (and vice versa), which is exactly the negative-correlation mechanism at work.

---

## Summary

|Concept|Key point|
|---|---|
|ARN idea|Use $1-U_i$ as the "antithetic" partner for each Unif(0,1) draw $U_i$, to induce negative correlation between two estimates|
|Why it works|$\mathrm{Var}\big(\frac{\hat\theta_1+\hat\theta_2}{2}\big) = \frac{1}{2}[\mathrm{Var}(\hat\theta_1)+\mathrm{Cov}(\hat\theta_1,\hat\theta_2)]$ — negative covariance shrinks this below the usual $\mathrm{Var}(\hat\theta_1)/2$|
|Contrast with CRN|CRN: positive correlation shrinks variance of a **difference** between two different systems. ARN: negative correlation shrinks variance of an **average** of two runs of the same estimation problem|
|Best use case|Monotonic functions $g(\cdot)$ — errors from $U_i$ and $1-U_i$ tend to offset each other|
|Demo result|Individual estimates off by ~0.04–0.05; antithetic average off by only ~0.006|

## Open Threads / To Follow Up

- [ ] When ARN works best/worst (relationship to monotonicity of $g$)
- [ ] Extending ARN to multivariate or more complex simulation settings, not just simple Monte Carlo integration
- [ ] Control variates — the next variance reduction technique in this module
- [ ] Combining CRN and ARN in the same study
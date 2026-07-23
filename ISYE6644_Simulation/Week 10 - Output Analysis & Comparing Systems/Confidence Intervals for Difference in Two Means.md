#statistics #simulation #output-analysis #confidence-intervals #two-sample

# Confidence Intervals for the Difference in Two Means

> Builds on the one-sample classical CI derivation. Here we compare **two** independent normal populations.

---

## 1. Setup: Two-Sample Case

Suppose: $$X_1, X_2, \dots, X_n \overset{iid}{\sim} \mathrm{Nor}(\mu_X, \sigma_X^2)$$ $$Y_1, Y_2, \dots, Y_m \overset{iid}{\sim} \mathrm{Nor}(\mu_Y, \sigma_Y^2)$$

**Goal:** a CI for the difference $\mu_X - \mu_Y$.

There are three standard "baby stats" methods, chosen based on what's known/assumed about $\sigma_X^2$ and $\sigma_Y^2$:

|Method|Use when|
|---|---|
|**Pooled CI**|$\sigma_X^2$ and $\sigma_Y^2$ are **equal but unknown**|
|**Approximate CI**|$\sigma_X^2$ and $\sigma_Y^2$ are **unequal and unknown**|
|**Paired CI**|$\mathrm{Cov}(X_i, Y_i) > 0$ (i.e., the $X$'s and $Y$'s are naturally paired/correlated, not independent)|

Throughout, $\bar{X}_n, \bar{Y}_m, S_X^2, S_Y^2$ are the obvious sample means and variances of the $X$'s and $Y$'s.

---

## 2. Pooled CI

**Applies when:** the $X$'s and $Y$'s are independent, with a **common, unknown variance** ($\sigma_X^2 = \sigma_Y^2 = \sigma^2$).

**Pooled variance estimator:** $$S_P^2 \equiv \frac{(n-1)S_X^2 + (m-1)S_Y^2}{n+m-2}$$

(a weighted average of the two sample variances, weighted by degrees of freedom)

**Resulting CI:** $$\mu_X - \mu_Y \ \in\ \bar{X}_n - \bar{Y}_m \ \pm\ t_{\alpha/2,,n+m-2}, S_P \sqrt{\frac{1}{n}+\frac{1}{m}}$$

- Degrees of freedom: $n+m-2$ (exact, since variances are assumed equal).

---

## 3. Approximate CI (Welch's t-test)

**Applies when:** the $X$'s and $Y$'s are independent, but with **arbitrary, unequal, unknown variances**.

**CI:** $$\mu_X - \mu_Y \ \in\ \bar{X} - \bar{Y} \ \pm\ t_{\alpha/2,,\nu} \sqrt{\frac{S_X^2}{n} + \frac{S_Y^2}{m}}$$

**Not exact** — it uses an _approximate_ degrees of freedom (Welch–Satterthwaite approximation): $$\nu \equiv \frac{\left(\dfrac{S_X^2}{n} + \dfrac{S_Y^2}{m}\right)^2}{\dfrac{(S_X^2/n)^2}{n+1} + \dfrac{(S_Y^2/m)^2}{m+1}} - 2$$

---

## 4. Paired CI

**Applies when:** $\mathrm{Cov}(X_i, Y_i) > 0$ — i.e., $X_i$ and $Y_i$ are **not independent** (e.g., before/after measurements on the same subject).

**Idea (not fully derived in these notes, but the standard approach):** work with the differences $D_i \equiv X_i - Y_i$ and apply the **one-sample** CI (from earlier notes) to the $D_i$'s directly, since pairing/positive covariance reduces variance and the pooled/approximate methods (which assume independence) would be invalid here.

---

## 5. Worked Example: Parallel Parking Times

**Setup:** Times (seconds, assume normal) for people to parallel park two different cars — a Honda ($X_i$, parked by "a guy") and a Cadillac ($Y_i$, parked by a **different, independent** guy).

|Honda $X_i$|Caddy $Y_i$|
|---|---|
|10|30|
|25|15|
|5|40|
|20|10|
|15|25|

Since these are **different, independent** people (not the same person parking both cars), this is an **independent two-sample** setup — not paired.

**Summary statistics** (after algebra): $$\bar{X} = 15, \quad \bar{Y} = 24, \quad S_X^2 = 62.5, \quad S_Y^2 = 142.5$$

Since $S_X^2 \ne S_Y^2$ noticeably, use the **Approximate CI** (unequal, unknown variances). $n=m=5$.

**Degrees of freedom:** $$\nu = \frac{6(62.5+142.5)^2}{(62.5)^2 + (142.5)^2} - 2 = 8.4 \approx 8 \quad \text{(round down)}$$

**90% CI** ($\alpha = 0.10 \Rightarrow t_{0.05,8}$): $$\mu_X - \mu_Y \ \in\ \bar{X} - \bar{Y} \pm t_{0.05,8}\sqrt{\frac{S_X^2}{n}+\frac{S_Y^2}{m}} = -9 \pm 11.91$$

**Interpretation:** the CI is $[-20.91,\ 2.91]$, which **contains 0** — so the result is **inconclusive** about which of $\mu_X$ (mean Honda-parking time) and $\mu_Y$ (mean Caddy-parking time) is actually bigger. We cannot conclude a statistically significant difference at the 90% confidence level.

---

## Summary

|Method|Assumption|Degrees of freedom|Notes|
|---|---|---|---|
|Pooled|$\sigma_X^2=\sigma_Y^2$, unknown|$n+m-2$ (exact)|Combines both samples' variance info into one pooled estimate|
|Approximate|$\sigma_X^2\ne\sigma_Y^2$, both unknown|$\nu$ (Welch–Satterthwaite approx.)|Safer default when variances look different|
|Paired|$\mathrm{Cov}(X_i,Y_i)>0$|reduces to one-sample case on differences $D_i$|Use when observations are naturally linked, not independent|

**Practical takeaway from the worked example:** always check whether $S_X^2$ and $S_Y^2$ look meaningfully different before picking pooled vs. approximate — using pooled when variances are actually unequal can distort the CI.

## Open Threads / To Follow Up

- [ ] Full derivation of the paired CI from the one-sample result
- [ ] Formal tests for equality of variances (to decide pooled vs. approximate) — and their own pitfalls
- [ ] How these two-sample CIs extend to comparing simulated systems (this was the original motivating question for this lesson sequence)
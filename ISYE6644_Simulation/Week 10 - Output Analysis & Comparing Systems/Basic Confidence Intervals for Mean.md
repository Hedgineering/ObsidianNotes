#statistics #simulation #output-analysis #confidence-intervals

# Confidence Interval for the Mean (One-Sample, i.i.d. Normal)

> **Last lesson:** intro to the problem of comparing simulated systems — which is best? **This lesson:** step back and review/derive the **classical confidence interval** for the mean of i.i.d. normal data. **After this:** graduate to the comparison of **two** systems.

---

## 1. Setup: One-Sample Case

**Goal:** obtain a two-sided $100(1-\alpha)%$ CI for the unknown mean $\mu$ of a normal distribution.

**Assumptions:**

- Data $X_1, X_2, \dots, X_n$ are **independent and identically distributed (i.i.d.) normal**.
- The variance $\sigma^2$ is **unknown**.

**Tool:** the well-known **$t$-distribution based CI** — derived below.

---

## 2. Key Distributional Facts

Before deriving the CI, recall three standard facts:

1. **Sample mean:** $$\bar{X}_n \equiv \frac{1}{n}\sum_{i=1}^n X_i \ \sim\ \mathrm{Nor}(\mu,\ \sigma^2/n)$$
    
2. **Sample variance:** $$S_X^2 \equiv \frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X}_n)^2 \ \sim\ \sigma^2 \cdot \frac{\chi^2(n-1)}{n-1}$$
    
3. **Independence:** $\bar{X}_n$ and $S_X^2$ **are independent** of each other. (This is a special property of normal samples — not true in general for other distributions.)
    

---

## 3. Constructing the Pivotal Quantity $T$

Combine the three facts above to build a quantity whose distribution **doesn't depend on the unknown $\sigma^2$**:

$$T = \frac{\bar{X}_n - \mu}{\sqrt{S_X^2/n}} = \frac{\dfrac{\bar{X}_n-\mu}{\sqrt{\sigma^2/n}}}{\sqrt{S_X^2/\sigma^2}} \ \sim\ \frac{\mathrm{Nor}(0,1)}{\sqrt{\dfrac{\chi^2(n-1)}{n-1}}} \ \sim\ t(n-1)$$

**Why this works:**

- The numerator, $\dfrac{\bar X_n - \mu}{\sqrt{\sigma^2/n}}$, is a standardized normal, i.e. $\mathrm{Nor}(0,1)$.
- The denominator, $\sqrt{S_X^2/\sigma^2}$, is $\sqrt{\chi^2(n-1)/(n-1)}$.
- A standard normal divided by $\sqrt{\chi^2(\nu)/\nu}$ (with the two independent) is, by **definition**, a $t(\nu)$ random variable.
- The $\sigma$'s cancel out top and bottom, leaving a quantity, $T$, that depends only on **known/observable** quantities ($\bar X_n$, $S_X^2$, $n$) plus the unknown $\mu$ — and its distribution ($t(n-1)$) is completely known.

This $T$ is called a **pivotal quantity**: it's a function of the data and the parameter of interest, but its distribution doesn't depend on any unknown parameters.

---

## 4. Deriving the Confidence Interval

Let $t_{\gamma,\nu}$ denote the $1-\gamma$ quantile of a $t$-distribution with $\nu$ degrees of freedom.

By definition of the $t$-distribution's quantiles: $$1-\alpha = P\big(-t_{\alpha/2,n-1} \le T \le t_{\alpha/2,n-1}\big)$$

Substitute $T$: $$= P\left(-t_{\alpha/2,n-1} \ \le\ \frac{\bar{X}_n - \mu}{\sqrt{S_X^2/n}} \ \le\ t_{\alpha/2,n-1}\right)$$

**Algebraically isolate $\mu$** in the middle (multiply through by $\sqrt{S_X^2/n}$, then rearrange): $$= P\Big(\bar{X}_n - t_{\alpha/2,n-1}, S_X/\sqrt{n} \ \le\ \mu \ \le\ \bar{X}_n + t_{\alpha/2,n-1}, S_X/\sqrt{n}\Big)$$

---

## 5. Result: The Classical CI

$$\boxed{\mu \in \bar{X}_n \pm t_{\alpha/2,,n-1}, \dfrac{S_X}{\sqrt{n}}}$$

This is the standard $100(1-\alpha)%$ confidence interval for the mean of i.i.d. normal data with unknown variance.

---

## Summary / Mental Model

|Step|What happens|
|---|---|
|Start with $\bar X_n \sim \mathrm{Nor}(\mu,\sigma^2/n)$ and $S_X^2 \sim \sigma^2\chi^2(n-1)/(n-1)$, independent|Standard iid-normal sampling theory|
|Form $T = (\bar X_n-\mu)/\sqrt{S_X^2/n}$|A normal divided by $\sqrt{\chi^2(\nu)/\nu}$ → exactly $t(n-1)$, with $\sigma$ canceling out|
|Sandwich $T$ between $\pm t_{\alpha/2,n-1}$|Uses the known $t$-distribution to make a probability statement|
|Solve for $\mu$ in the middle|Algebraic rearrangement — same probability statement, different "subject"|
|Result|$\mu \in \bar X_n \pm t_{\alpha/2,n-1},S_X/\sqrt n$|

**Why this matters going forward:** this is exactly the _form_ every steady-state CI we've built (IR's $\theta \in \bar Z_r \pm t_{\alpha/2,r-1}\sqrt{S_Z^2/r}$, BM's $\mu \in \bar Y_n \pm t_{\alpha/2,b-1}\sqrt{\hat V_B/n}$, OBM's version, etc.) is mimicking — they all substitute in an appropriate "sample mean" and "variance estimator" pair for a set of quantities that are only **approximately** i.i.d. normal, and lean on this exact classical derivation as their template.

## Open Threads / To Follow Up

- [ ] Extending this framework to the comparison of **two** systems (paired vs. unpaired analysis)
- [ ] How the CI's validity degrades when the i.i.d.-normal assumption is only approximate (ties back to earlier notes on dependent-data pitfalls)
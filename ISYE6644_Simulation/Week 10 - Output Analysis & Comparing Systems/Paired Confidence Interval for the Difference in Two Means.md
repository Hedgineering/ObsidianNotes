#statistics #simulation #output-analysis #confidence-intervals #paired-t

# Paired Confidence Interval for the Difference in Two Means

> **Last time:** covered CIs for the difference in two means assuming completely independent samples. **This time:** what if the samples **aren't** independent of each other? Not necessarily bad! → **Paired CI** for the difference in means.

---

## 1. Setup: Paired Data

Again consider two competing normal populations with unknown means $\mu_X$ and $\mu_Y$. Suppose we collect observations from the two populations in **pairs**.

**Key structural assumption:**

- **Different pairs are independent** of one another.
- But the **two observations within the same pair may not be independent**.

$$\underbrace{\begin{cases}\text{Pair 1: } (X_1,Y_1) \ \text{Pair 2: } (X_2,Y_2) \ \quad\vdots \ \text{Pair } n\text{: } (X_n,Y_n)\end{cases}}_{\text{pairs indep. of each other}}, \qquad \underbrace{(X_i,Y_i)}_{\text{not indep. within a pair}}$$

### Motivating example: twin studies

Think sets of twins — one twin takes a new drug, the other takes a placebo.

**Idea:** by pairing up similar subjects (twins share genetics, environment, etc.), we hope to capture the difference between the two populations **more precisely**, since the pairing **eliminates extraneous noise**.

> This is the same trick used later in simulation via **common random numbers** — deliberately inducing correlation between two systems' outputs to sharpen a comparison, rather than avoiding it.

---

## 2. Formal Set-up

Take $n$ pairs of observations: $$X_1,\dots,X_n \overset{iid}{\sim} \mathrm{Nor}(\mu_X,\sigma_X^2), \qquad Y_1,\dots,Y_n \overset{iid}{\sim}\mathrm{Nor}(\mu_Y,\sigma_Y^2)$$

**Technical assumption:** all $X_i$'s and $Y_j$'s are **jointly normal**.

- Variances $\sigma_X^2, \sigma_Y^2$ are **unknown and possibly unequal**.
- Pair $i$ is independent of pair $j$ (**between** pairs) — but $X_i$ may **not** be independent of $Y_i$ (**within** a pair).

---

## 3. The Trick: Work with Pairwise Differences

Define the **pairwise differences**: $$D_i \equiv X_i - Y_i, \qquad i=1,\dots,n$$

Then: $$D_1, D_2, \dots, D_n \overset{iid}{\sim} \mathrm{Nor}(\mu_D, \sigma_D^2)$$

where:

- $\mu_D \equiv \mu_X - \mu_Y$ — exactly the quantity we want a CI for!
- $$\sigma_D^2 \equiv \sigma_X^2 + \sigma_Y^2 - 2,\mathrm{Cov}(X_i,Y_i)$$

**The key idea:** we _hope_ that $\mathrm{Cov}(X_i,Y_i)$ is **strongly positive** — this directly **lowers $\sigma_D^2$**, and lower variance means a **tighter, more informative CI**. This is exactly the mechanism by which "pairing eliminates extraneous noise": positively-correlated pairs (like twins, or the same person doing two tasks) share common variation that cancels out when you take the difference.

---

## 4. Reduction to the One-Sample Case

Once we work with $D_i$'s, the problem **reduces exactly to the earlier one-sample $\mathrm{Nor}(\mu,\sigma^2)$ case** with unknown $\mu$ and $\sigma^2$ (see prior notes on the classical one-sample CI).

Compute the sample mean and variance of the differences, just as before: $$\bar{D} \equiv \frac{1}{n}\sum_{i=1}^n D_i \ \sim\ \mathrm{Nor}(\mu_D, \sigma_D^2/n)$$ $$S_D^2 \equiv \frac{1}{n-1}\sum_{i=1}^n (D_i - \bar D)^2 \ \sim\ \frac{\sigma_D^2,\chi^2(n-1)}{n-1}$$

**Resulting CI (identical form to the one-sample case):** $$\boxed{\mu_D \in \bar{D} \pm t_{\alpha/2,,n-1}\sqrt{S_D^2/n}}$$

---

## 5. Worked Example: Parallel Parking (Paired Version)

**Setup:** this time, the **same person** parks _both_ a Honda and a Cadillac (contrast with the earlier independent-samples version, where different people parked each car).

|Person|Park Honda|Park Cadillac|Difference|
|---|---|---|---|
|1|10|20|−10|
|2|25|40|−15|
|3|5|5|0|
|4|20|35|−15|
|5|15|20|−5|

**Key structural note:** the individual people are independent of each other, but **the times for the same individual to park the two cars are NOT independent** — this is a genuinely paired design.

### Computing the CI

From the differences: $\bar{D} = -9$, $S_D^2 = 42.5$, $n=5$.

**90% two-sided CI** ($\alpha=0.10 \Rightarrow t_{0.025,4} = 2.13$): $$\mu_D \in \bar{D} \pm t_{0.025,4}\sqrt{S_D^2/n} = -9 \pm 2.13\sqrt{42.5/5} = -9 \pm 6.21$$

**Interpretation:** the interval $[-15.21,\ -2.79]$ is **entirely to the left of 0**, indicating $\mu_D < 0$ — i.e., **Caddys take longer to park, on average**. Unlike the earlier independent-samples version of this example (which was inconclusive), the paired design gives a **statistically significant, conclusive** result.

---

## 6. Paired vs. Approximate (Independent) CI — Direct Comparison

|Approach|Result|Width|
|---|---|---|
|Approximate (independent-samples) CI, earlier example|$-9 \pm 11.91$|wider — inconclusive (contains 0)|
|Paired-$t$ CI, this example|$-9 \pm 6.21$|**much narrower** — conclusive|

The paired CI is **quite a bit shorter (more informative)** than the independent-samples "approximate" CI, because the paired-$t$ approach **takes advantage of the correlation within observation pairs** — the same underlying point estimate ($-9$) becomes far more precise once the shared, person-specific noise is subtracted out.

> **Moral: use paired-$t$ when you can.** If you have the ability to design a paired experiment (or, in simulation, to induce correlation via common random numbers), it will generally give tighter, more useful confidence intervals than treating the two samples as independent.

---

## Summary

|Concept|Key point|
|---|---|
|Setup|Pairs independent of each other, but within-pair observations may be correlated|
|Trick|Reduce to one-sample problem via differences $D_i = X_i - Y_i$|
|Why it helps|$\sigma_D^2 = \sigma_X^2+\sigma_Y^2-2\mathrm{Cov}(X_i,Y_i)$ — positive covariance shrinks variance|
|CI|Exactly the one-sample formula, applied to the $D_i$'s|
|Practical payoff|Much narrower CIs than independent-sample methods, when pairing induces real positive correlation|
|Forward link|This is the same principle behind the simulation technique of **common random numbers**|

## Open Threads / To Follow Up

- [ ] Common random numbers (CRN) — how simulation deliberately engineers this positive correlation between two system configurations
- [ ] What happens if $\mathrm{Cov}(X_i,Y_i) < 0$ — when pairing could backfire and _increase_ variance instead
- [ ] Formal criteria for deciding whether a paired or independent design is more appropriate for a given comparison
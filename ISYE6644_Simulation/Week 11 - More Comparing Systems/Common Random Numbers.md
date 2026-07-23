#statistics #simulation #output-analysis #variance-reduction #common-random-numbers

# Common Random Numbers (CRN)

> **Last time:** implemented baby stats CIs for the differences in means for use in simulations. **This time:** start discussion of **variance reduction techniques**. Three to come this module: **Common random numbers** (this doc, similar to paired-$t$ CIs), Antithetic random numbers, Control variates.

---

## 1. The Idea

**Idea behind paired-$t$ CI, applied to simulation:** use **common random numbers (CRN)** — i.e., use the **same pseudo-random numbers**, in exactly the same ways, for corresponding runs of each of the competing systems.

**Example:** use the **same customer arrival and service times** when simulating different proposed configurations of a job shop.

**Why this helps:** by subjecting the alternative systems to **identical experimental conditions**, we hope to make it **easier to distinguish which system is best**, even though the respective estimators still have sampling error. This is the exact same logic as the earlier paired-$t$ / twins example: shared "noise" (here, the random number stream) gets subtracted out when you look at the _difference_ between systems.

---

## 2. Why We Need This: The Variance Problem

**Setup:** compare two queueing systems, $A$ and $B$, based on their expected customer transit times, $\theta_A$ and $\theta_B$ — **smaller** is better.

Suppose we have estimators $\hat\theta_A$ and $\hat\theta_B$. We'll declare $A$ the better system if $\hat\theta_A < \hat\theta_B$.

**If $\hat\theta_A$ and $\hat\theta_B$ are simulated independently:** $$\mathrm{Var}(\hat\theta_A - \hat\theta_B) = \mathrm{Var}(\hat\theta_A) + \mathrm{Var}(\hat\theta_B)$$

This could be **very large** — and if it is, our declaration of "A is better" might **lack conviction** (i.e., the CI for $\theta_A - \theta_B$ could be so wide it's inconclusive, even if the point estimate favors A).

---

## 3. The Fix: Induce Positive Correlation

If we could **reduce** $\mathrm{Var}(\hat\theta_A - \hat\theta_B)$, we'd be much more confident about our declaration.

**CRN sometimes induces a high positive correlation** between the point estimators $\hat\theta_A$ and $\hat\theta_B$. Then:

$$\mathrm{Var}(\hat\theta_A - \hat\theta_B) = \mathrm{Var}(\hat\theta_A) + \mathrm{Var}(\hat\theta_B) - 2,\mathrm{Cov}(\hat\theta_A, \hat\theta_B) ;<; \mathrm{Var}(\hat\theta_A) + \mathrm{Var}(\hat\theta_B)$$

➡️ **We obtain a savings in variance** — a tighter CI for $\theta_A - \theta_B$, for essentially the same simulation effort. (This is the same "something for nothing" flavor seen earlier with overlapping batch means, and the same mechanism as the twins/parking paired-$t$ example — positive covariance between the two quantities being differenced directly lowers the variance of the difference.)

---

## 4. Demo: Queueing Configuration Comparison

**Question:** which strategy yields shorter cycle times, with exponential interarrival and service times?

- **A.** One line feeding into two parallel servers, or
- **B.** Customers making a 50-50 choice between two lines, each feeding into a single server

**Setup:** simulate each alternative for **20 replications of 1000 minutes**.

### Without CRN (independent simulations)

$$\mu_A - \mu_B \in -16.19 \pm 9.26$$

### With CRN (same arrival and service times across strategies)

$$\mu_A - \mu_B \in -15.05 \pm 3.37$$

**Result: much tighter CI!** The half-width shrank from $9.26$ down to $3.37$ — roughly a **2.7x reduction** — just by reusing the same underlying random numbers across the two strategies' corresponding replications, with essentially the same simulation budget (still 20 reps of 1000 minutes each).

Both CIs point the same direction (A appears better, since both intervals are entirely negative — smaller cycle time), but the CRN version delivers a far more precise, decisive answer for the same computational cost.

---

## Summary

|Concept|Key point|
|---|---|
|CRN idea|Reuse the same pseudo-random numbers, applied identically, for corresponding replications of competing systems|
|Why it works|Induces positive correlation between $\hat\theta_A$ and $\hat\theta_B$, which subtracts out via $-2\mathrm{Cov}$ in $\mathrm{Var}(\hat\theta_A-\hat\theta_B)$|
|Analogy|Same mechanism as the paired-$t$ CI (twins, parking-time examples) — shared "noise" cancels in the difference|
|Practical payoff|Much narrower CI for the difference in means, same simulation budget (demo: 9.26 → 3.37 half-width)|
|Caveat|CRN "sometimes" induces positive correlation — it's not automatic; depends on how the random numbers are synchronized across systems|

## Open Threads / To Follow Up

- [ ] Practical implementation: how to actually synchronize random number streams across different system configurations (especially when systems have structurally different logic, like strategy A vs. B above)
- [ ] Cases where CRN can backfire (induce negative correlation, increasing variance instead)
- [ ] Antithetic random numbers — the next variance reduction technique in this module
- [ ] Control variates — the technique after that
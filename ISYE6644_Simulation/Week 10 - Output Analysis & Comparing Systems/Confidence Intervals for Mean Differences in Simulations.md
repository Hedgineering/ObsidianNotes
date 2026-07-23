#statistics #simulation #output-analysis #confidence-intervals #comparison-of-systems

# Confidence Intervals for Mean Differences in Simulations

> **Last lesson:** discussed the paired-$t$ CI for the difference in two normal means. Use when the two samples are correlated. **This time:** how to apply this in simulations, where observations **aren't** i.i.d. normal. **Idea:** use **replicate sample means** as (approximately) i.i.d. normals.

---

## 1. Comparison of Simulated Systems — Motivation

One of the most important uses of simulation output analysis is **comparing competing systems or alternative configurations**.

**Example:** evaluate two different "re-start" strategies an airline can invoke following a disrupting snowstorm — which policy minimizes a certain cost function associated with the re-start?

Simulation is uniquely equipped to help with these comparisons. Several techniques exist:

- **Classical CI's** adapted to simulations (this document's focus)
- **Variance reduction methods**
- **Ranking and selection procedures**

---

## 2. Setting Up the Problem: Replication Sample Means

With the airline example in mind, let $Z_{i,j}$ be the **cost from the $j$th simulation replication of strategy $i$**, for $i=1,2$ and $j=1,2,\dots,b_i$.

**Assume:** $Z_{i,1}, Z_{i,2}, \dots, Z_{i,b_i}$ are (approximately) **i.i.d. normal** with unknown mean $\mu_i$ and unknown variance, for $i=1,2$.

### Justification for this assumption

This is exactly the trick from earlier IR (independent replications) notes, applied here to justify treating raw simulation output as iid normal:

**(a) Get independence** by controlling the random numbers between replications (i.e., use different seeds per rep — same idea as IR).

**(b) Get identically distributed costs** between reps by performing the reps under **identical conditions** (same initial state, same warm-up handling, etc.).

**(c) Get approximate normality** by adding up (or averaging) **many sub-costs** to get an overall cost for both strategies — leaning on the CLT, since a sum/average of many components tends toward normal even if the components themselves aren't.

> This three-part justification is the general recipe for legitimately applying classical iid-normal machinery to simulation output: control the RNG streams, standardize the operating conditions, and aggregate enough sub-quantities to invoke a CLT.

---

## 3. Approach 1: Independent (Unpaired) CI for $\mu_1 - \mu_2$

**Goal:** obtain a $100(1-\alpha)%$ CI for the difference in means, $\mu_1 - \mu_2$.

Suppose the $Z_{1,j}$'s are **independent of** the $Z_{2,j}$'s (i.e., strategy 1's replications don't share random numbers with strategy 2's). Define:

$$\bar{Z}_{i,b_i} \equiv \frac{1}{b_i}\sum_{j=1}^{b_i} Z_{i,j}, \qquad i=1,2$$ $$S_i^2 \equiv \frac{1}{b_i-1}\sum_{j=1}^{b_i}(Z_{i,j}-\bar Z_{i,b_i})^2, \qquad i=1,2$$

**Approximate $100(1-\alpha)%$ CI:** $$\mu_1 - \mu_2 \ \in\ \bar{Z}_{1,b_1} - \bar{Z}_{2,b_2} \ \pm\ t_{\alpha/2,,\nu}\sqrt{\frac{S_1^2}{b_1}+\frac{S_2^2}{b_2}}$$

where the (approximate) degrees of freedom $\nu$ is the same Welch–Satterthwaite expression from the earlier two-sample notes.

**This is exactly the "Approximate CI"** from the two-sample notes, just with $Z_{i,j}$'s (simulation replication costs) standing in for the generic $X_i$'s and $Y_i$'s.

### Interpreting the CI (decision rule)

Suppose (as in the airline example) **smaller cost is better**:

- If the interval lies **entirely to the left of 0**: system 1 is better.
- If the interval lies **entirely to the right of 0**: system 2 is better.
- If the interval **contains 0**: the two systems are, statistically, **about the same** — no significant difference detected.

---

## 4. Approach 2: Paired-$t$ CI (Common Random Numbers)

**Alternative strategy:** use a CI analogous to a paired-$t$ test — the simulation analog of the paired design from the earlier notes.

**Setup:** take $b$ replications from **both** strategies (same number $b$ for each), and set the difference: $$D_j \equiv Z_{1,j} - Z_{2,j}, \qquad j=1,2,\dots,b$$

Compute: $$\bar{D}_b \equiv \frac{1}{b}\sum_{j=1}^b D_j \qquad\text{and}\qquad S_D^2 \equiv \frac{1}{b-1}\sum_{j=1}^b (D_j - \bar D_b)^2$$

**Resulting $100(1-\alpha)$% paired-$t$ CI:**
$$\boxed{\mu_1 - \mu_2 \ \in\ \bar{D}_b \pm t_{\alpha/2,,b-1}\sqrt{S_D^2/b}}$$

### When this pays off

The paired-$t$ CI is **very efficient** if $\mathrm{Corr}(Z_{1,j}, Z_{2,j}) > 0$ — i.e., if replication $j$ of strategy 1 and replication $j$ of strategy 2 are **positively correlated**.

**How to engineer that positive correlation in simulation:** use the **same random numbers** to drive replication $j$ of _both_ strategies (e.g., the same sequence of customer arrival times, service times, weather disruptions, etc., feeds into both re-start policies' $j$th replication). This is the simulation technique of **common random numbers (CRN)**, foreshadowed in the earlier notes — it deliberately induces the positive correlation that the paired-$t$ approach thrives on.

---

## 5. Comparing the Two Approaches

|Approach|Requires|Efficiency|
|---|---|---|
|Independent (unpaired) CI|$Z_{1,j}$'s independent of $Z_{2,j}$'s; possibly $b_1 \ne b_2$|Standard — no extra correlation to exploit|
|Paired-$t$ CI (via CRN)|Same $b$ reps for both strategies, driven by the **same random numbers** per replication index $j$|**Much more efficient** (narrower CI) when $\mathrm{Corr}(Z_{1,j},Z_{2,j})>0$|

This mirrors the earlier parking-example lesson exactly: pairing (here, via common random numbers) eliminates shared noise between the two systems' replications, shrinking the variance of the difference and giving a tighter, more decisive CI — just as it did for the twin/parking-time examples.

> **Moral (carried over from before):** use the paired-$t$ (CRN) approach when you can engineer positive correlation between the two systems' replications — it's essentially "something for nothing," much like overlapping batch means was in the earlier steady-state notes.

---

## Summary

|Concept|Key point|
|---|---|
|Goal|CI for $\mu_1-\mu_2$, the difference in expected cost/performance between two simulated systems|
|Justifying iid-normality|Control RNGs (independence), identical operating conditions (identical distribution), aggregate sub-costs (approximate normality via CLT)|
|Independent CI|Standard Welch-type approximate CI, same form as earlier two-sample notes|
|Paired-t (CRN) CI|Apply same random numbers to both strategies per replication → induces positive correlation → tighter CI|
|Decision rule|CI entirely left/right of 0 → one system is better; CI contains 0 → no significant difference|

## Open Threads / To Follow Up

- [ ] Practical mechanics of implementing common random numbers (synchronizing random number streams across strategies)
- [ ] Variance reduction methods beyond CRN (antithetic variates, control variates, etc.)
- [ ] Ranking and selection procedures for comparing more than two systems at once
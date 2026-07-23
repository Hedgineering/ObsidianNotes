#simulation #statistics #ranking-and-selection

# Multinomial Selection: Single-Stage Procedure & Extensions

Continues from [[Multinomial Selection]]. Here we formalize the indifference-zone framework for multinomial selection, present a single-stage procedure, and cover extensions to save observations — paralleling the structure used for [[Bernoulli Selection]] and [[Bernoulli Selection Extensions]].

---

## Assumptions and Notation

- $\boldsymbol{X}_j = (X_{1j}, \dots, X_{kj})$ ($j \geq 1$) are independent observations taken from a multinomial distribution having $k \geq 2$ categories with associated **unknown** probabilities $\boldsymbol{p} = (p_1, \dots, p_k)$.
- $X_{ij} = 1$ [0] if category $i$ does [does not] occur on the $j$-th observation.
- The (unknown) ordered $p_i$'s are $p_{[1]} \le \cdots \le p_{[k]}$.
- The category with $p_{[k]}$ is the **most probable** or **best**.
- The cumulative sum for category $i$ after $m$ multinomial observations have been taken is $$ Y_{im} = \sum_{j=1}^m X_{ij}. $$
- The ordered $Y_{im}$'s are $Y_{[1]m} \le \cdots \le Y_{[k]m}$.

---

## Goal and Probability Requirement

**Goal:** Select the category associated with $p_{[k]}$.

A **correct selection (CS)** is made if the Goal is achieved.

**Probability Requirement:** For specified $(P^\star, \theta^\star)$ with $1/k < P^\star < 1$ and $\theta^\star > 1$, we require

$$ P(\mathrm{CS}) \geq P^\star \quad \text{whenever} \quad p_{[k]}/p_{[k-1]} \geq \theta^\star. \tag{3} $$

$\theta^\star$ is the **"smallest $p_{[k]}/p_{[k-1]}$ ratio worth detecting."**

> [!note] Compare to Bernoulli This is the same **odds-ratio-style** flavor as $\mathcal{B}_{BKS}$ in [[Bernoulli Selection Extensions]], but here it's a straight **probability ratio** $p_{[k]}/p_{[k-1]}$ rather than an odds ratio — since there's no natural "failure" category to normalize against in the general multinomial setting.

There are various procedures to guarantee requirement (3).

---

## Single-Stage Procedure $\mathcal{M}_{BEM}$

For the given $k$, $P^\star$, and $\theta^\star$, find $n$ from a table (originally from Bechhofer, Elmaghraby, and Morse 1959).

**Steps:**

1. Take $n$ multinomial observations $\boldsymbol{X}_j = (X_{1j}, \dots, X_{kj})$ ($1 \le j \le n$) in a **single** stage.
2. Calculate the ordered sample sums $Y_{[1]n} \le \cdots \le Y_{[k]n}$.
3. Select the category with the **largest sum**, $Y_{[k]n}$, as the one associated with $p_{[k]}$, **randomizing to break ties**.

This directly parallels $\mathcal{B}_{SH}$ from [[Bernoulli Selection]]: fixed sample size from a table, compare summary counts, pick the max.

---

## Remark: Least-Favorable Configuration

The $n$-values in the table are computed so that $\mathcal{M}_{BEM}$ achieves $P(\mathrm{CS}) \geq P^\star$ when the cell probabilities $\boldsymbol{p}$ are in the **least-favorable (LF) configuration** (Kesten and Morse 1959):

$$ p_{[1]} = \cdots = p_{[k-1]} = \frac{1}{\theta^\star + k - 1} \quad\text{and}\quad p_{[k]} = \frac{\theta^\star}{\theta^\star + k - 1}. $$

Here all the "losing" categories are tied at the smallest value consistent with the required ratio, and the winning category sits right at the boundary ratio $\theta^\star$ above them — the hardest case to correctly detect. This mirrors the **slippage/LF configuration** idea from [[Single-Stage Normal Means Procedure]]: guarantee the worst case, and the guarantee holds everywhere else too.

---

## Table: Sample Size $n$ for $\mathcal{M}_{BEM}$ to Guarantee (3)

|$P^\star$|$\theta^\star$|$k=2$|$k=3$|$k=4$|$k=5$|
|---|---|---|---|---|---|
|0.75|2.0|5|12|20|29|
|0.75|1.8|5|17|29|41|
|0.75|1.6|9|26|46|68|
|0.75|1.4|17|52|92|137|
|0.75|1.2|55|181|326|486|
|0.90|2.0|15|29|43|58|
|0.90|1.8|19|40|61|83|
|0.90|1.6|31|64|98|134|
|0.90|1.4|59|126|196|271|
|0.90|1.2|199|437|692|964|
|0.95|2.0|23|42|61|81|
|0.95|1.8|33|59|87|115|
|0.95|1.6|49|94|139|185|
|0.95|1.4|97|186|278|374|
|0.95|1.2|327|645|979|1331|

> [!note] Same pattern as before Larger $P^\star$ or $\theta^\star$ closer to 1 (need to detect finer differences) → much larger $n$ required.

---

## Example: Cola Taste Test

A soft drink producer wants to find the most popular of $k = 3$ proposed cola formulations. The company will give a taste test to $n$ people.

The sample size $n$ is chosen so that $P(\mathrm{CS}) \geq 0.95$ whenever the ratio of the largest to second-largest true (but unknown) proportions is at least $1.4$.

Entering the table with $k=3$, $P^\star = 0.95$, $\theta^\star = 1.4$: $n = 186$ individuals must be interviewed.

If we find that $Y_{1,186} = 53$, $Y_{2,186} = 110$, $Y_{3,186} = 26$, then **we select formulation 2 as the best**.

---

## Extensions

Just as with Bernoulli selection, several tricks improve on the fixed-$n$ single-stage procedure:

- **Curtailed procedure:** Stop sampling once the category in second place **can't win** — mirrors the Bechhofer/Kulkarni curtailment trick from [[Bernoulli Selection Extensions]].
- **Sequential procedures:** Sample one observation at a time (across all categories) and stop as soon as the winner is statistically clear — mirrors $\mathcal{B}_{BKS}$ from [[Bernoulli Selection Extensions]].
- **Real-world examples:** This machinery is easy to apply in simulation settings — each "observation" is simply a simulation run, and the category counts accumulate naturally as runs complete.

---

## Summary

- Multinomial selection formalizes "most probable category" via the indifference-zone requirement $P(\mathrm{CS}) \geq P^\star$ whenever $p_{[k]}/p_{[k-1]} \geq \theta^\star$.
- $\mathcal{M}_{BEM}$: single-stage procedure — fixed $n$ from a table, pick the category with the largest count, randomize ties.
- Required $n$ is derived from the least-favorable configuration, where the top category exceeds all (tied) others by exactly the ratio $\theta^\star$.
- Curtailment and sequential sampling extend this procedure to save observations, just as in the Bernoulli case — and both are naturally suited to simulation experiments where each replication is a full simulation run.
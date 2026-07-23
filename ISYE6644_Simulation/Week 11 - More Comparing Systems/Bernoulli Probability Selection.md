#simulation #statistics #ranking-and-selection

# Bernoulli Selection

Continues from [[Normal Means Extensions]]. Analogous to the normal means problem, but now the competing populations are **Bernoulli** rather than normal. Lots of applications in simulation, medical trials, etc.

---

## Introduction

**Goal:** Select the Bernoulli population with the largest success parameter.

### Examples

- Which anti-cancer drug is most effective?
- Which simulated system is most likely to meet design specs?
- Which $(s, S)$ inventory policy has the highest profit probability?

There are hundreds of such procedures. Highlights:

|Type|Reference|
|---|---|
|Single-Stage Procedure|Sobel and Huyett (1957)|
|Sequential Procedure|Bechhofer, Kiefer, and Sobel (1968)|
|"Optimal" Procedures|Bechhofer et al. (1980's)|

---

## Setup

We have $k$ competing Bernoulli populations with success parameters $p_1, p_2, \dots, p_k$. Denote the ordered $p$'s by $$ p_{[1]} \le p_{[2]} \le \cdots \le p_{[k]}. $$

**Goal:** Select the population having the largest probability $p_{[k]}$.

**Probability Requirement:** For specified constants $(P^\star, \Delta^\star)$ with $1/k < P^\star < 1$ and $0 < \Delta^\star < 1$, we require $$ P(\mathrm{CS}) \geq P^\star \quad \text{whenever} \quad p_{[k]} - p_{[k-1]} \geq \Delta^\star. $$

This is exactly analogous to the normal-means indifference-zone requirement (see [[Normal Means Selection]]), but expressed in terms of the difference $p_{[k]} - p_{[k-1]}$, with $\Delta^\star$ interpreted as the **"smallest difference worth detecting."**

---

## A Single-Stage Procedure $\mathcal{B}_{SH}$ (Sobel and Huyett 1957)

**Steps:**

1. For the specified $(P^\star, \Delta^\star)$, find $n$ from a table.
2. Take a sample of $n$ observations $X_{ij}$ ($1 \le j \le n$) in a **single stage** from each population ($1 \le i \le k$). ($X_{ij}$ is the 0/1 outcome of the $j$-th Bernoulli trial for population $i$.)
3. Calculate the $k$ sample sums: $$ Y_{in} = \sum_{j=1}^n X_{ij}. $$
4. Select the treatment that yielded the **largest** $Y_{in}$ as the one associated with $p_{[k]}$.

> [!note] Ties In the case of ties, **randomize** among the tied populations to choose the winner.

This mirrors $\mathcal{N}_B$ from the normal case (see [[Single-Stage Normal Means Procedure]]): fixed single-stage sample, compare a summary statistic, pick the max — except here the summary statistic is a sum of successes rather than a sample mean, since with Bernoulli data comparing sums is equivalent to comparing sample proportions.

---

## Table: Smallest $n$ for $\mathcal{B}_{SH}$ to Guarantee Probability Requirement

**$k = 3$**

|$\Delta^\star$|0.60|0.75|0.80|0.85|0.90|0.95|0.99|
|---|---|---|---|---|---|---|---|
|0.10|20|52|69|91|125|184|327|
|0.20|5|13|17|23|31|46|81|
|0.30|3|6|8|10|14|20|35|
|0.40|2|4|5|6|8|11|20|
|0.50|2|3|3|4|5|7|12|

**$k = 4$**

|$\Delta^\star$|0.60|0.75|0.80|0.85|0.90|0.95|0.99|
|---|---|---|---|---|---|---|---|
|0.10|34|71|90|114|150|212|360|
|0.20|9|18|23|29|38|53|89|
|0.30|4|8|10|13|17|23|39|
|0.40|3|5|6|7|9|13|21|
|0.50|2|3|4|5|6|8|13|

(Columns are $P^\star$.)

> [!note] Same pattern as normal means Larger $P^\star$ (want more confidence) → larger $n$. Larger $\Delta^\star$ (only need to detect big differences in $p$) → smaller $n$.

---

## Example

Suppose we want to select the best of $k = 4$ treatments with probability at least $P^\star = 0.95$ whenever $p_{[4]} - p_{[3]} \geq 0.10$.

- From the table: $k=4$, $\Delta^\star = 0.10$, $P^\star = 0.95$ → $n = 212$ observations.

Suppose that, at the end of sampling, we have: $$ Y_{1,212} = 70,\quad Y_{2,212} = 145,\quad Y_{3,212} = 95,\quad Y_{4,212} = 102. $$

Since $Y_{2,212}$ is largest, **we select population 2 as the best.**

---

## Summary

- Bernoulli selection: pick the population with largest success probability $p_{[k]}$, out of $k$ candidates.
- Indifference-zone framework carries over directly from the normal case: guarantee $P(\mathrm{CS}) \geq P^\star$ whenever the true gap $p_{[k]} - p_{[k-1]} \geq \Delta^\star$.
- $\mathcal{B}_{SH}$ (Sobel and Huyett 1957): single-stage analog of $\mathcal{N}_B$ — fixed sample size $n$ from a table, compare sums of successes $Y_{in}$, pick the largest (randomize on ties).
- As with the normal case, this single-stage procedure is likely to have practical limitations (conservative, requires independence, fixed $n$) — sequential (Bechhofer, Kiefer, Sobel 1968) and "optimal" procedures (Bechhofer et al., 1980s) exist to address these, paralleling the Rinott / Kim & Nelson extensions for normal means (see [[Normal Means Extensions]]).
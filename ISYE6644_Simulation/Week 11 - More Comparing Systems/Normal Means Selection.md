#simulation #statistics #ranking-and-selection

# Selecting the Normal Distribution with the Largest Mean

Continues from [[Ranking and Selection]]. Here we look at the classic R&S problem: choosing the normal distribution (among several) that has the largest mean, while guaranteeing a certain probability of correct selection.

---

## Setup

We give procedures for selecting the one of $k$ **normal** distributions having the largest mean, using the **indifference-zone** approach.

**Assumptions:**

- Independent observations $Y_{i1}, Y_{i2}, \dots$ ($1 \le i \le k$) are taken from $k \geq 2$ normal populations $\Pi_1, \dots, \Pi_k$.
- $\Pi_i$ has **unknown** mean $\mu_i$.
- $\Pi_i$ has variance $\sigma_i^2$, which may be **known or unknown**.

**Notation:**

- Vector of means: $\boldsymbol{\mu} = (\mu_1, \dots, \mu_k)$
- Vector of variances: $\boldsymbol{\sigma}^2 = (\sigma_1^2, \dots, \sigma_k^2)$
- The ordered (but unknown) means: $\mu_{[1]} \le \mu_{[2]} \le \cdots \le \mu_{[k]}$

The system with the largest mean $\mu_{[k]}$ is defined as the **"best."**

> [!info] Goal Select the population associated with mean $\mu_{[k]}$.

A **correct selection (CS)** occurs if this goal is achieved.

---

## Indifference-Zone Probability Requirement

For specified constants $(P^\star, \delta^\star)$ with $\delta^\star > 0$ and $1/k < P^\star < 1$, we require:

$$ P(\mathrm{CS}) \geq P^\star \quad \text{whenever} \quad \mu_{[k]} - \mu_{[k-1]} \geq \delta^\star. \tag{1} $$

- $\delta^\star$ = the **"smallest difference worth detecting."** If the best and second-best means are separated by at least $\delta^\star$, we want a high guaranteed probability of picking the true best.
- $P^\star$ = the desired probability of correct selection when that separation holds.
- The actual probability of correct selection depends on the differences $\mu_i - \mu_j$, the sample size $n$, and $\sigma^2$.

---

## Preference Zone vs. Indifference Zone

Picture the ordered means on a number line, with $\delta^\star$ marking the "gap of interest" near the top two means.

**Preference zone:** parameter configurations $\boldsymbol{\mu}$ satisfying $$ \mu_{[k]} - \mu_{[k-1]} \geq \delta^\star $$ are said to be in the _preference zone_ for a correct selection — here the top mean is well-separated from the rest, so we should be able to detect it reliably.

**Indifference zone:** if instead $$ \mu_{[k]} - \mu_{[k-1]} < \delta^\star, $$ then $\boldsymbol{\mu}$ is in the _indifference zone_ — the best and second-best are so close together that we don't really care if we "misselect," since the two systems are practically equivalent in quality.

> [!note] Key idea Any procedure that guarantees the probability statement (1) is said to be **employing the indifference-zone approach**. We only promise a high $P(\mathrm{CS})$ when the top two means are far enough apart ($\geq \delta^\star$) to matter; when they're closer than that, we're "indifferent" to which one gets picked.

---

## So Many Procedures!

There are **hundreds** of R&S procedures in the literature. Highlights:

|Type|Reference|
|---|---|
|Single-Stage|Bechhofer (1954)|
|Two-Stage|Rinott (1979)|
|Sequential|Kim and Nelson (2001)|

This module focuses on the **single-stage procedure**, covering just the basics, and assumes **common, known variance** across all $k$ populations (i.e., $\sigma_1^2 = \cdots = \sigma_k^2 = \sigma^2$, known).

---

## Summary

- Problem: pick the normal population with the largest mean $\mu_{[k]}$ out of $k$ candidates.
- Use indifference-zone approach: guarantee $P(\mathrm{CS}) \geq P^\star$ whenever the true gap $\mu_{[k]} - \mu_{[k-1]} \geq \delta^\star$.
- $\delta^\star$: smallest meaningful difference; $P^\star$: desired confidence level ($1/k < P^\star < 1$).
- Configurations split into a **preference zone** (gap $\geq \delta^\star$, guarantee applies) and an **indifference zone** (gap $< \delta^\star$, don't care which is selected).
- Next: details of the **single-stage procedure** (Bechhofer 1954) under common, known variance.
#simulation #statistics #ranking-and-selection

# Single-Stage Procedure $\mathcal{N}_B$ (Bechhofer 1954)

Continues from [[Normal Means Selection]]. This is a specific, "baby" single-stage procedure — just to give a taste of how indifference-zone procedures work in practice.

> [!warning] Scope
> 
> - Assumes **known, common variance** $\sigma^2$ across all $k$ populations.
> - Better (more general) procedures exist — this is a simple starting point.

---

## The Procedure

$\mathcal{N}_B$ takes **all** necessary observations and makes the selection decision **at once**, in a single stage (unlike two-stage or sequential procedures).

**Steps:**

1. For the given $k$ and specified $(P^\star, \delta^\star/\sigma)$, determine the sample size $n$ (usually from a table).
2. Take a random sample of $n$ observations $Y_{ij}$ ($1 \le j \le n$) in a single stage from each population $\Pi_i$ ($1 \le i \le k$).
3. Calculate the $k$ sample means: $$ \bar Y_i = \sum_{j=1}^n Y_{ij}/n, \qquad 1 \le i \le k. $$
4. Select the population that yielded the **largest sample mean**, $$ \bar Y_{[k]} = \max{\bar Y_1, \dots, \bar Y_k}, $$ as the one associated with $\mu_{[k]}$ (i.e., declare it "best").

> [!tip] Very intuitive All you have to do is figure out $n$ — then just compare sample means. $n$ can be obtained:
> 
> - from a table (easy), or
> - from a multivariate normal quantile (not too bad), or
> - via a separate simulation (if all else fails)

---

## Table: Common Sample Size $n$ per Population Required by $\mathcal{N}_B$

Rows indexed by $k$ and $P^\star$; columns indexed by $\delta^\star/\sigma$.

**$k = 2$**

|$P^\star$|0.1|0.2|0.3|0.4|0.5|0.6|0.7|0.8|0.9|1.0|
|---|---|---|---|---|---|---|---|---|---|---|
|0.75|91|23|11|6|4|3|2|2|2|1|
|0.90|329|83|37|21|14|10|7|6|5|4|
|0.95|542|136|61|34|22|16|12|9|7|6|
|0.99|1083|271|121|68|44|31|23|17|14|11|

**$k = 3$**

|$P^\star$|0.1|0.2|0.3|0.4|0.5|0.6|0.7|0.8|0.9|1.0|
|---|---|---|---|---|---|---|---|---|---|---|
|0.75|206|52|23|13|9|6|5|4|3|3|
|0.90|498|125|56|32|20|14|11|8|7|5|
|0.95|735|184|82|46|30|21|15|12|10|8|
|0.99|1309|328|146|82|53|37|27|21|17|14|

**$k = 4$**

|$P^\star$|0.1|0.2|0.3|0.4|0.5|0.6|0.7|0.8|0.9|1.0|
|---|---|---|---|---|---|---|---|---|---|---|
|0.75|283|71|32|18|12|8|6|5|4|3|
|0.90|602|151|67|38|25|17|13|10|8|7|
|0.95|851|213|95|54|35|24|18|14|11|9|
|0.99|1442|361|161|91|58|41|30|23|18|15|

> [!note] Pattern Larger $P^\star$ (want more confidence) → larger $n$. Larger $\delta^\star/\sigma$ (only need to detect big differences) → smaller $n$.

---

## Remark: Direct Calculation (No Table Needed)

You can compute $n$ directly instead of using the table:

$$ n = \left\lceil 2\left(\sigma Z^{(1-P^\star)}_{k-1,1/2}/\delta^\star\right)^2 \right\rceil, $$

where $\lceil \cdot \rceil$ rounds up, and the constant $Z^{(1-P^\star)}_{k-1,1/2}$ is an upper equicoordinate point of a certain multivariate normal distribution.

---

## Why This Works: The Least-Favorable Configuration

The value of $n$ satisfies the probability requirement $P(\mathrm{CS}) \geq P^\star$ (whenever $\mu_{[k]} - \mu_{[k-1]} \geq \delta^\star$) **for any** $\boldsymbol{\mu}$, as long as it holds for the "worst case" $\boldsymbol{\mu}$.

**Slippage configuration:** $$ \mu_{[1]} = \mu_{[k-1]} = \mu_{[k]} - \delta^\star. \tag{2} $$

Here all the "losing" means are tied and pushed right up against the boundary $\delta^\star$ below the best mean — $\mu_{[k]}$ has "slipped" ahead by exactly $\delta^\star$, and everyone else is bunched together at the bottom.

This is also the **least-favorable (LF)** configuration for procedure $\mathcal{N}_B$: for fixed $n$, it **minimizes** $P(\mathrm{CS})$ among all $\boldsymbol{\mu}$ in the preference zone. So if we choose $n$ large enough to guarantee $P^\star$ at the LF configuration, we guarantee it everywhere else in the preference zone too.

### Derivation sketch

$$ P^\star = P(\mathrm{CS} \mid \mathrm{LF}) = P{\bar Y_i < \bar Y_k,\ i = 1,\dots,k-1 \mid \mathrm{LF}} $$

Standardizing and using independence of the $\bar Y_i$:

$$ = P\left{\frac{\bar Y_i - \mu_k}{\sqrt{\sigma^2/n}} < \frac{\bar Y_k - \mu_k}{\sqrt{\sigma^2/n}},\ i=1,\dots,k-1 ;\middle|; \mathrm{LF}\right} $$

Conditioning on $x = (\bar Y_k - \mu_k)/\sqrt{\sigma^2/n}$ and integrating it out against its standard normal density $\phi(x)$:

$$ = \int_{\mathbb{R}} P\left{\frac{\bar Y_i - \mu_k}{\sqrt{\sigma^2/n}} < x,\ i=1,\dots,k-1 ;\middle|; \mathrm{LF}\right}\phi(x),dx $$

Using $\mu_k - \mu_i = \delta^\star$ under LF to re-center on $\mu_i$:

$$ = \int_{\mathbb{R}} P\left{\frac{\bar Y_i - \mu_i}{\sqrt{\sigma^2/n}} < x + \frac{\sqrt{n},\delta^\star}{\sigma},\ i=1,\dots,k-1\right}\phi(x),dx $$

Since the $k-1$ standardized $\bar Y_i$'s are iid standard normal:

$$ = \int_{\mathbb{R}} \Phi^{k-1}!\left(x + \frac{\sqrt{n},\delta^\star}{\sigma}\right)\phi(x),dx = \int_{\mathbb{R}} \Phi^{k-1}(x+h),\phi(x),dx, $$

where $h = \sqrt{n},\delta^\star/\sigma$.

**Procedure:** solve this equation numerically for $h$, then set

$$ n = \left\lceil (h\sigma/\delta^\star)^2 \right\rceil. $$

---

## Example

Suppose $k = 4$ and we want to detect a difference in means as small as $0.2$ standard deviations, with $P(\mathrm{CS}) \geq 0.99$.

- From the table: $\delta^\star/\sigma = 0.2$, $P^\star = 0.99$, $k=4$ → $n = 361$ observations per population.

Suppose after taking $n = 361$ observations we find: $$ \bar Y_1 = 13.2,\quad \bar Y_2 = 9.8,\quad \bar Y_3 = 16.1,\quad \bar Y_4 = 12.1. $$

Since $\bar Y_3$ is largest, **we select population 3 as the best.**

> [!tip] Sensitivity of $n$ Increasing $\delta^\star$ and/or decreasing $P^\star$ requires a **smaller** $n$. E.g., with $\delta^\star/\sigma = 0.6$ and $P^\star = 0.95$, $\mathcal{N}_B$ requires only $n = 24$ observations per population — a huge reduction from $n=361$.

---

## Summary

- $\mathcal{N}_B$: single-stage, known common variance, compare sample means, pick the largest.
- Required sample size $n$ depends on $k$, $P^\star$, and $\delta^\star/\sigma$ — obtainable from a table or direct formula.
- Guarantee is derived using the **slippage / least-favorable configuration**, where the top mean beats all others by exactly $\delta^\star$ and the rest are tied.
- Larger required confidence ($P^\star \uparrow$) or finer resolution ($\delta^\star \downarrow$) → larger $n$ needed.
- Next: presumably two-stage (Rinott) or sequential (Kim & Nelson) procedures, which relax the known-variance assumption.
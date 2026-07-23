#statistics #simulation #output-analysis #finite-horizon #independent-replications

# Finite-Horizon Extensions: Smaller CIs & Quantile CIs

> **Last time:** introduced the method of independent replications (IR) for finite-horizon simulations. **This time:** expand what IR can do — get **smaller CIs for the mean**, and get **CIs for quantiles**.

---

## 1. Shrinking the CI for the Mean

**Problem:** the current CI half-length may be too wide. Fix: run **more replications**.

### Setup

- $H \equiv t_{\alpha/2,,r-1}\sqrt{S_Z^2/r}$ — half-length of the _current_ CI (based on $r$ reps)
- $\epsilon \equiv t_{\alpha/2,,r^\star-1}\sqrt{S_Z^2/r^\star}$ — _desired_ half-length, using the **same** variance estimator $S_Z^2$ but more reps $r^\star > r$

### Solving for the number of reps needed

$$r^\star = \frac{t_{\alpha/2,,r^\star-1}^2 S_Z^2}{\epsilon^2} < \frac{t_{\alpha/2,,r-1}^2 S_Z^2}{\epsilon^2} = (H/\epsilon)^2, r$$

(The inequality holds because $t$ quantiles increase as degrees of freedom decrease, i.e. $t_{\alpha/2,r^\star-1} < t_{\alpha/2,r-1}$ when $r^\star > r$.)

### Practical recipe

1. Set $r^\star \leftarrow (H/\epsilon)^2, r$
2. Run $r^\star - r$ **additional** replications
3. Recompute the CI using **all** $r^\star$ replications
4. You'll _probably_ get a CI with half-length close to $\epsilon$ (not guaranteed exactly, since $S_Z^2$ will change with more data)

### Rule of thumb

> To reduce CI length by a factor of **10**, you need to increase replications by a factor of **100** (since $r^\star \propto 1/\epsilon^2$ — the usual $\sqrt{n}$ convergence rate of CI width).

---

## 2. CIs for Quantiles

Sometimes the mean isn't the performance measure of interest — we want a **quantile** (e.g., "the 90th percentile of waiting time").

### Definition

The **$p$-quantile** of a random variable $W$ with cdf $F(w)$: $$\xi_p \equiv \min{w \mid F(w) \ge p}$$

If $W$ is continuous: $F(\xi_p) = p ;\Rightarrow; \xi_p = F^{-1}(p)$.

**Example (Exponential):** $W \sim \mathrm{Exp}(\lambda)$, $F(w) = 1-e^{-\lambda w}$ $$\xi_p = -\frac{1}{\lambda}\ln(1-p)$$

We can use **IR** to build CIs for quantiles too.

---

### 2.1 Point Estimator for $\xi_p$

**Setup:** Let $W_i$ be some scalar performance measure (e.g., max waiting time between 8am–5pm) from replication $i=1,\dots,r$. Since replications are independent, $W_1,\dots,W_r$ are **i.i.d.**

Order statistics: $W_{(1)} \le W_{(2)} \le \dots \le W_{(r)}$

**Point estimator:** $$\hat{\xi}_p \equiv W_{(\lfloor rp+0.5\rfloor)}$$ where $\lfloor\cdot\rfloor$ is the floor function.

---

### 2.2 CI for $\xi_p$ — Derivation

**Goal (Hahn & Meeker 1991):** a conservative nonparametric CI of the form $$\xi_p \in [W_{(j)},, W_{(k)}]$$

**Step 1 — Binomial connection.** Since $P(W_i \le \xi_p) = p$, define $A$ = number of $W_i$'s that are $\le \xi_p$. Then: $$A \sim \mathrm{Bin}(r,p)$$

**Step 2 — Translate the order-statistic event.** The event ${j \le A \le k-1}$ (between $j$ and $k-1$ of the $W_i$'s are $\le \xi_p$) is equivalent to: $${\xi_p \ge W_{(j)}} \text{ and } {\xi_p < W_{(k)}}$$

**Step 3 — Put it together.** $$P\big(W_{(j)} \le \xi_p < W_{(k)}\big) = P(j \le A \le k-1) = \sum_{\ell=j}^{k-1}\binom{r}{\ell}p^\ell (1-p)^{r-\ell}$$

**Step 4 — Normal approximation to the binomial** (with continuity correction): $$\doteq \Phi\left(\frac{k-0.5-rp}{\sqrt{rp(1-p)}}\right) - \Phi\left(\frac{j-0.5-rp}{\sqrt{rp(1-p)}}\right)$$

where $\Phi(\cdot)$ = standard normal cdf. Approximation requires $rp \ge 5$ and $r(1-p) \ge 5$.

---

### 2.3 Choosing $j$ and $k$

We need $j,k$ such that: $$\Phi\left(\frac{k-0.5-rp}{\sqrt{rp(1-p)}}\right) - \Phi\left(\frac{j-0.5-rp}{\sqrt{rp(1-p)}}\right) \ge 1-\alpha$$

> **Note:** the CI is a little conservative due to the binomial's discreteness — many $(j,k)$ pairs can satisfy the inequality, so there's freedom in how to pick them.

**Suggested (symmetric) choice** — set each tail to $z_{\alpha/2}$: $$\frac{j-0.5-rp}{\sqrt{rp(1-p)}} = -z_{\alpha/2}, \qquad \frac{k-0.5-rp}{\sqrt{rp(1-p)}} = z_{\alpha/2}$$

Solving: $$j = \left\lfloor rp + 0.5 - z_{\alpha/2}\sqrt{rp(1-p)} \right\rfloor$$ $$k = \left\lceil rp + 0.5 + z_{\alpha/2}\sqrt{rp(1-p)} \right\rceil$$

> ⚠️ For **extreme quantiles** (e.g., $p$ close to 0 or 1), you may need a large number of replications to get reasonably tight ($j,k$) bounds — since $rp$ or $r(1-p)$ must both stay $\ge 5$ for the normal approximation to hold.

---

### 2.4 Worked Example

**Goal:** 95% CI for $\xi_{0.9}$, with $r=1000$ replications.

**Point estimator:** $$\hat{\xi}_{0.9} = W_{(\lfloor 1000(0.9)+0.5\rfloor)} = W_{(900)}$$

**CI bounds** ($\alpha=0.05 \Rightarrow z_{0.025}=1.96$; $rp(1-p) = 1000(0.9)(0.1)=90$): $$j = \lfloor 900.5 - 1.96\sqrt{90}\rfloor = 881$$ $$k = \lceil 900.5 + 1.96\sqrt{90}\rceil = 920$$

**Result:** $$\xi_{0.9} \in [W_{(881)},, W_{(920)}]$$

---

## Summary

|Task|Tool|Key formula|
|---|---|---|
|Shrink CI for mean $\theta$|Run more reps $r^\star = (H/\epsilon)^2 r$|width shrinks like $1/\sqrt{r}$|
|CI for quantile $\xi_p$|Order statistics of i.i.d. per-replication summary $W_i$|$[W_{(j)}, W_{(k)}]$ via binomial/normal approx|

**Big idea common to both:** everything still rests on the IR trick — independent replications give you i.i.d. quantities ($Z_i$ for means, $W_i$ for other per-replication measures), which is what lets you legitimately apply classical (CLT- or binomial-based) inference.

## Open Threads / To Follow Up

- [ ] Other choices of $(j,k)$ besides the symmetric one (e.g., minimizing interval width)
- [ ] Behavior of quantile CIs for very extreme $p$ (need for many reps)
- [ ] Sequential procedures that adaptively decide how many more reps to run
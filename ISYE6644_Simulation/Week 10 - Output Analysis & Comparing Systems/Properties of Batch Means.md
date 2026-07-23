#statistics #simulation #output-analysis #steady-state #batch-means

# Properties of Batch Means (BM)

> **Last time:** introduced the basics of steady-state analysis, including the batch means variance estimator. **This time:** Properties of the BM estimator, plus the BM confidence interval for the steady-state mean. **Note:** the CI is perhaps _the_ key result in output analysis.

---

## 1. Recap: The Batch Means Estimator

$$\hat{V}_B \equiv \frac{m}{b-1}\sum_{i=1}^b (\bar{Y}_{i,m} - \bar{Y}_n)^2 \ \approx\ \frac{\sigma^2\chi^2(b-1)}{b-1}$$

where $m$ = batch size, $b$ = number of batches, $n = bm$.

**Question:** how good is $\hat V_B$ as an estimator of $\sigma^2$? Look at its mean and variance.

**Approximate mean:** $$\mathrm{E}[\hat{V}_B] \doteq \frac{\sigma^2}{b-1}\mathrm{E}[\chi^2(b-1)] = \sigma^2$$ so $\hat V_B$ is **asymptotically unbiased** for $\sigma^2$ as batch size $m \to \infty$.

---

## 2. More-Precise Result: Bias and Variance

A sharper expansion shows: $$\mathrm{E}[\hat{V}_B] = \sigma^2 + \underbrace{\frac{\gamma(b+1)}{mb}}_{\text{Bias term}} + o(1/m)$$

where: $$\gamma \equiv -2\sum_{k=1}^\infty k R_k$$

and $o(1/m)$ denotes a function that vanishes **faster than rate $1/m$** as $m$ grows.

**Variance:** $$\mathrm{Var}(\hat{V}_B) \doteq \frac{\sigma^4}{(b-1)^2}\mathrm{Var}\big(\chi^2(b-1)\big) = \frac{2\sigma^4}{b-1}$$

**Takeaways:**

- Bias shrinks like $1/m$ (depends on batch size, not directly on $b$ except through the $(b+1)/b$ factor which $\to 1$).
- Variance shrinks like $1/b$ (depends on number of batches).
- These two error sources are controlled by **different knobs** ($m$ vs. $b$) — this sets up a genuine trade-off given a fixed total sample size $n = bm$.

---

## 3. Mean Squared Error (MSE) and the Bias-Variance Trade-off

For large $m$ and $b$: $$\mathrm{MSE}(\hat{V}_B) = \mathrm{Bias}^2 + \mathrm{Var} \ \doteq\ \frac{\gamma^2}{m^2} + \frac{2\sigma^4}{b}$$

**Goal:** choose $b$ and $m$ (subject to the budget constraint $n = bm$) to minimize MSE.

### Step 1 — parametrize $m$

Let $m = cn^\delta$ for $c>0$, $0<\delta<1$. Substituting (and using $b = n/m$): $$\mathrm{MSE}(\hat{V}_B) \doteq \frac{\gamma^2}{c^2 n^{2\delta}} + \frac{2c\sigma^4}{n^{1-\delta}}$$

### Step 2 — optimize $\delta$

The choice $\boxed{\delta = 1/3}$ balances the two terms' rates and yields the **fastest convergence to 0** for the MSE. Substituting $\delta=1/3$: $$\mathrm{MSE}(\hat{V}_B) \doteq \frac{1}{n^{2/3}}\left[\frac{\gamma^2}{c^2} + 2c\sigma^4\right]$$

### Step 3 — optimize $c$

Minimizing over $c$ gives the **"optimal" batch size**: $$m^\star \equiv \left(\frac{\gamma^2 n}{\sigma^4}\right)^{1/3}$$

and the resulting **"optimal" MSE**: $$\mathrm{MSE}^\star(\hat{V}_B) \equiv 3\left(\frac{\gamma\sigma^4}{n}\right)^{2/3}$$

**Practical caveat:** $\sigma^2$ and $\gamma$ are unknown in practice and must be estimated (a topic for another day).

> **Bottom line:** the batch size $m$ needed to minimize MSE is $O(n^{1/3})$ — batch size should grow, but much more slowly than the total sample size.

---

## 4. The Batch Means Confidence Interval for $\mu$

Since the batch means are approximately i.i.d. $\mathrm{Nor}(\mu, \sigma^2/m)$ for large $m$, we get the approximate $100(1-\alpha)%$ CI for $\mu$:

$$\mu \in \bar{Y}_n \pm t_{\alpha/2,,b-1}\sqrt{\hat{V}_B/n}$$

**Comparison to IR:** this looks just like the IR-based CI, $\mu \in \bar Z_r \pm t_{\alpha/2,r-1}\sqrt{S_Z^2/r}$, from earlier notes.

- **IR** divides work into $r$ independent (shorter) simulation runs.
- **BM** divides _one long run_ into $b$ batches.
- They're mathematically interchangeable: if you relabel the old IR example's $Z_i$'s as batch means instead of replicate means, the same numbers carry through, since $S_Z^2/r = \hat{V}_B/n$.

---

## 5. Properties of the Batch Means CI

Define the CI **half-length**: $$H \equiv t_{\alpha/2,,b-1}\sqrt{\hat{V}_B/n}$$

As $m\to\infty$, it can be shown that: $$\sqrt{mb},H \approx \sigma, t_{\alpha/2,,b-1}, \frac{\chi(b-1)}{\sqrt{b-1}} \quad \text{(the chi distribution)}$$

Taking expectation and variance: $$\sqrt{mb},\mathrm{E}[H] \to \sigma, t_{\alpha/2,,b-1}\sqrt{\frac{2}{b-1}},\frac{\Gamma(b/2)}{\Gamma((b-1)/2)}$$

$$mb,\mathrm{Var}(H) \to \sigma^2 t_{\alpha/2,,b-1}^2 \left{1 - \frac{2}{b-1}\left[\frac{\Gamma(b/2)}{\Gamma((b-1)/2)}\right]^2\right}$$

---

## 6. Practical Remarks

- $\mathrm{E}[H]$ **decreases in $b$** (more batches → tighter CI, all else equal), though the improvement **smooths out around $b=30$** — diminishing returns kick in.
- **Common recommendation:** fix $b \doteq 30$, and put remaining sample budget toward **increasing the batch size $m$** as much as possible.
- BM is popular because it's **intuitively appealing and easy to understand**.

### When BM can go wrong

Problems can arise if:

1. The $Y_j$'s are **not stationary** — e.g., significant initialization bias still present.
2. The batch means are **not (approximately) normal**.
3. The batch means are **not (approximately) independent**.

> If any of these assumption violations exist, **poor CI coverage may result — unbeknownst to the analyst.** This mirrors the earlier warning about $S_Y^2/n$ under raw dependent data giving misleadingly narrow CIs.

### Remedies

|Problem|Fix|
|---|---|
|Initialization bias / non-stationarity|Truncate early data, or use a very long run (as discussed in initialization notes)|
|Batch means not independent or not normal|**Increase the batch size $m$** — larger batches → weaker correlation between batches & better CLT approximation|

---

## Summary

|Quantity|Behavior|
|---|---|
|$\mathrm{Bias}(\hat V_B)$|$O(1/m)$ — controlled by batch size|
|$\mathrm{Var}(\hat V_B)$|$O(1/b)$ — controlled by number of batches|
|Optimal batch size $m^\star$|$O(n^{1/3})$|
|Rule of thumb|$b \approx 30$ batches, maximize $m$ given budget $n=bm$|
|CI validity depends on|stationarity, normality, and independence of batch means|

## Open Threads / To Follow Up

- [ ] How to actually estimate $\sigma^2$ and $\gamma$ in practice (needed for $m^\star$)
- [ ] Sequential / adaptive batching procedures that check independence and normality as they go
- [ ] Comparison of BM vs. IR in terms of computational efficiency and CI quality
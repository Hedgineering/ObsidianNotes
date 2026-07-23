#statistics #simulation #output-analysis

# Output Analysis: Issues with Non-IID Data

> Core theme: in **steady-state simulation**, observations $Y_1, Y_2, \dots, Y_n$ are typically _identically distributed_ with mean $\mu$, but **not independent**. This breaks several classical results and creates traps for the unwary.

## Working Assumptions

- $Y_1, \dots, Y_n$ identically distributed, mean $\mu$
- **Not independent**
- Common in steady-state simulation output (e.g., successive waiting times in a queue)

---

## 1. Sample Mean $\bar{Y}_n$

### Unbiasedness still holds

$$\mathrm{E}[\bar{Y}_n] = \frac{1}{n}\sum_{i=1}^n \mathrm{E}[Y_i] = \mu$$

- ✅ Sample mean is **still unbiased** for $\mu$, even under dependence.

### Variance does NOT simplify the usual way

Define the **covariance function**: $R_k \equiv \mathrm{Cov}(Y_1, Y_{1+k})$ for all $k$ (depends only on lag $k$, not on $i$ — stationarity assumption).

$$\mathrm{Var}(\bar{Y}_n) = \frac{1}{n^2}\sum_{i=1}^n\sum_{j=1}^n \mathrm{Cov}(Y_i,Y_j) = \frac{1}{n^2}\sum_{i=1}^n\sum_{j=1}^n R_{|i-j|}$$

Collapsing the double sum (counting how many times each lag appears in the $n\times n$ covariance matrix: $n$ copies of $R_0$, $2(n-1)$ copies of $R_1$, $2(n-2)$ copies of $R_2$, ..., $2$ copies of $R_{n-1}$):

$$\mathrm{Var}(\bar{Y}_n) = \frac{1}{n}\left[R_0 + 2\sum_{k=1}^{n-1}\left(1-\frac{k}{n}\right)R_k\right] \tag{2}$$

**Key takeaway:** the variance of the sample mean depends on _all the covariances_, not just $R_0 = \mathrm{Var}(Y_1)$.

---

## 2. Variance Parameters $\sigma_n^2$ and $\sigma^2$

Define: $$\sigma_n^2 \equiv n\mathrm{Var}(\bar{Y}_n) = R_0 + 2\sum_{k=1}^{n-1}\left(1-\frac{k}{n}\right)R_k$$

And the limiting **variance parameter**: $$\sigma^2 \equiv \lim_{n\to\infty}\sigma_n^2 \overset{\star}{=} R_0 + 2\sum_{k=1}^\infty R_k = \sum_{k=-\infty}^{\infty} R_k$$

- $\overset{\star}{=}$ holds only if $R_k \to 0$ fast enough as $k\to\infty$ (summability condition).
- $\sigma^2$ shows up constantly in output analysis (batch means, CLTs for dependent sequences, etc.)

### IID special case

If $Y_i$'s are iid: $R_k = 0$ for all $k\neq 0$, so $$\sigma^2 = \sigma_n^2 = R_0 = \mathrm{Var}(Y_1)$$ — the familiar result.

### Dependent (queueing) case ⚠️

- Covariances are typically **positive** in queueing systems (e.g., consecutive waiting times are positively correlated).
- So $\sigma^2 \doteq \sigma_n^2 \gg \mathrm{Var}(Y_1)$ — **often much bigger than you'd expect**.
- Interpretation: $\sigma_n^2/\mathrm{Var}(Y_1)$ ≈ number of (dependent) $Y_i$'s needed to match the information content of **one independent observation**.

> **Warning:** $\sigma_n^2 \gg \mathrm{Var}(Y_1)$ is exactly what causes classical confidence intervals to misbehave (see §4).

### Example: AR(1) process

$$Y_i = \phi Y_{i-1} + \varepsilon_i, \quad i=1,2,\dots$$ where $-1<\phi<1$, $Y_0 \sim \mathrm{Nor}(0,1)$, $\varepsilon_i \overset{iid}{\sim}\mathrm{Nor}(0,1-\phi^2)$ independent of $Y_0$.

- All $Y_i \sim \mathrm{Nor}(0,1)$, and $R_k = \phi^{|k|}$.
- $$\sigma^2 = \sum_{k=-\infty}^\infty \phi^{|k|} = \frac{1+\phi}{1-\phi}$$
- **Numeric example:** $\phi = 0.9 \Rightarrow \sigma^2 = 19$ (vs. $\mathrm{Var}(Y_1)=1$!) — huge inflation from positive correlation.

---

## 3. Sample Variance $S_Y^2$

$$S_Y^2 \equiv \frac{1}{n-1}\sum_{i=1}^n (Y_i - \bar{Y}_n)^2$$

### IID case — everything's fine 🙂

- $S_Y^2$ is unbiased for $R_0 = \mathrm{Var}(Y_1)$
- Also unbiased for $\sigma_n^2$ and $\sigma^2$ (since all three equal $R_0$ under iid)

### Dependent case — biased low 🙁

Derivation sketch: $$\mathrm{E}[S_Y^2] = \frac{n}{n-1}\left(\mathrm{Var}(Y_1) - \mathrm{Var}(\bar{Y}_n)\right)$$

Substituting equation (2) and assuming $R_k > 0$ (positive correlation, typical of queueing):

$$\mathrm{E}[S_Y^2] = R_0 - \frac{2}{n-1}\sum_{k=1}^{n-1}\left(1-\frac{k}{n}\right)R_k ;<; R_0$$

**Bottom line result:** $$\mathrm{E}[S_Y^2] < \mathrm{Var}(Y_1) \ll n\mathrm{Var}(\bar{Y}_n)$$

- $S_Y^2$ **underestimates** $\mathrm{Var}(Y_1)$ when correlations are positive.
- It underestimates $\sigma_n^2 = n\mathrm{Var}(\bar Y_n)$ even more severely.

---

## 4. ⚠️ Practical Consequence: Broken Confidence Intervals

**Do NOT use $S_Y^2/n$ to estimate $\mathrm{Var}(\bar{Y}_n)$** when data is dependent.

The classical $100(1-\alpha)% CI (valid for **iid normal** data with unknown variance) is: $$\mu \in \bar{Y}_n \pm t_{\alpha/2,,n-1}\sqrt{S_Y^2/n}$$

Since (for positively correlated data): $$\mathrm{E}[S_Y^2/n] \ll \mathrm{Var}(\bar{Y}_n)$$

➡️ **The interval is too narrow.** True coverage probability is **much less than** the nominal $1-\alpha$.

> **"Oops!"** — this is precisely why naive application of classical iid-based confidence intervals to correlated simulation output (e.g., raw sequential output from a single simulation run) gives **misleadingly tight, overconfident intervals**.

### Why this matters for simulation practice

- Steady-state simulation output is inherently autocorrelated (positively, usually).
- Blindly computing $\bar Y_n \pm t\sqrt{S_Y^2/n}$ on raw output **understates uncertainty**.
- Motivates specialized techniques: **batch means, replication/deletion, regenerative methods, spectral/time-series methods** — designed to properly account for $\sigma^2$ instead of $R_0$.

---

## Quick Reference Table

|Quantity|Definition|IID case|Dependent (positively correlated) case|
|---|---|---|---|
|$\mathrm{E}[\bar Y_n]$|—|$\mu$|$\mu$ (still unbiased)|
|$\mathrm{Var}(\bar Y_n)$|eq. (2)|$R_0/n$|inflated by covariance terms|
|$\sigma_n^2$|$n\mathrm{Var}(\bar Y_n)$|$=R_0$|$\gg R_0$|
|$\sigma^2$|$\lim \sigma_n^2 = \sum_{k=-\infty}^\infty R_k$|$=R_0$|$\gg \mathrm{Var}(Y_1)$|
|$\mathrm{E}[S_Y^2]$|—|$=R_0$ (unbiased)|$< R_0$ (biased low)|
|CI using $S_Y^2/n$|—|valid|**invalid — too narrow, true coverage $\ll 1-\alpha$**|


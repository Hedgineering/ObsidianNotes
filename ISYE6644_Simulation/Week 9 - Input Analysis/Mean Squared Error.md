#simulation #statistics #point-estimation

# Mean Squared Error and the Bias-Variance Tradeoff

## 1. Motivation: Why Unbiasedness Alone Isn't Enough

From [[Unbiased Point Estimation]], we know a good estimator $T(\mathbf{X})$ should ideally have:

- **Low bias** — the difference between the estimator's expected value and the true parameter value.
- **Low variance** — estimates shouldn't bounce around too much from sample to sample.

> [!warning] You actually need BOTH
> 
> - **Low bias + high variance = bad.** A meaningless, noisy estimator — right on average, but any single estimate could be far off.
> - **Low variance + high bias = bad.** A confidently wrong answer — consistent, but consistently off-target.
> 
> This tension is the classic **bias-variance tradeoff**, and **Mean Squared Error (MSE)** is the standard tool for combining both into a single number to compare estimators.

---

## 2. Definitions

> [!info] Definition — Bias The **bias** of an estimator $T(\mathbf{X})$ for parameter $\theta$ is $$\mathrm{Bias}(T) \equiv \mathrm{E}[T] - \theta.$$ ($T$ is unbiased exactly when $\mathrm{Bias}(T) = 0$ — see [[Unbiased Point Estimation]].)

> [!info] Definition — Mean Squared Error (MSE) $$\mathrm{MSE}(T) \equiv \mathrm{E}\left[(T-\theta)^2\right].$$ This is the expected squared distance between the estimator and the true parameter — capturing _both_ how off-center ($T$'s bias) and how spread out ($T$'s variance) the estimator is.


### Derivation: MSE = Variance + Bias²

**Start with the definition of MSE, and expand the square:**
$$
\mathrm{E}\left[(T-\theta)^2\right] = \mathrm{E}\left[T^2 - 2T\theta + \theta^2\right] = \mathrm{E}[T^2] - 2\theta\,\mathrm{E}[T] + \theta^2
$$

**Rewrite $\mathrm{E}[T^2]$ using the standard variance identity** $\mathrm{Var}(Y) = \mathrm{E}[Y^2] - (\mathrm{E}[Y])^2$, applied to $Y = T$:
$$
\mathrm{E}[T^2] = \mathrm{Var}(T) + (\mathrm{E}[T])^2
$$

**Substitute this back in:**
$$
\mathrm{E}[(T-\theta)^2] = \underbrace{\mathrm{Var}(T) + (\mathrm{E}[T])^2}_{\mathrm{E}[T^2]} - 2\theta\,\mathrm{E}[T] + \theta^2
$$

$$
= \mathrm{Var}(T) + \Big[(\mathrm{E}[T])^2 - 2\theta\,\mathrm{E}[T] + \theta^2\Big]
$$

**Recognize the bracketed term as a perfect square** (with $a = \mathrm{E}[T]$, $b = \theta$, so $a^2 - 2ab + b^2 = (a-b)^2$):
$$
(\mathrm{E}[T])^2 - 2\theta\,\mathrm{E}[T] + \theta^2 = \big(\mathrm{E}[T] - \theta\big)^2
$$

**Result:**
$$
\mathrm{MSE}(T) = \mathrm{E}\left[(T-\theta)^2\right] = \mathrm{Var}(T) + \big(\underbrace{\mathrm{E}[T]-\theta}_{Bias}\big)^2 = \mathrm{Var}(T) + \mathrm{Bias}(T)^2
$$

### 2.1 The Bias–Variance Decomposition

> [!theorem] MSE = Variance + Bias² $$\mathrm{MSE}(T) = \mathrm{Var}(T) + \big(\mathrm{E}[T] - \theta\big)^2 = \mathrm{Var}(T) + \mathrm{Bias}(T)^2.$$


**Key implication:** _Lower MSE is better — even if that means accepting a little bias._ A slightly biased estimator with much lower variance can easily beat an unbiased-but-noisy one. This is why biased estimators (e.g., ridge regression, shrinkage estimators) are often preferred in practice.

### 2.2 Relative Efficiency

> [!info] Definition — Relative Efficiency The **relative efficiency** of $T_2$ to $T_1$ is $$\frac{\mathrm{MSE}(T_1)}{\mathrm{MSE}(T_2)}.$$ If this ratio is **< 1**, then $T_1$ has smaller MSE and is the preferred estimator (and vice versa if > 1).

This gives a clean way to rank _any_ two estimators of the same parameter — even ones with different bias — on equal footing.

---

## 3. Worked Example: Estimating $\theta$ for $\mathrm{Unif}(0,\theta)$

**Setup:** Suppose $X_1, \dots, X_n \stackrel{iid}{\sim} \mathrm{Unif}(0,\theta)$, i.e. pdf $f(x) = 1/\theta$ for $0 < x < \theta$. Consider two candidate estimators for $\theta$:

$$Y_1 \equiv 2\bar{X} \qquad \text{and} \qquad Y_2 \equiv \frac{n+1}{n}\max_{1 \le i \le n} X_i.$$

### 3.1 Both estimators are unbiased

**$Y_1$:** Since $\mathrm{E}[X_i] = \theta/2$ for a $\mathrm{Unif}(0,\theta)$ r.v., $$\mathrm{E}[Y_1] = 2,\mathrm{E}[\bar X] = 2,\mathrm{E}[X_i] = \theta.$$ So $Y_1$ is unbiased for $\theta$. ✅

**$Y_2$:** This takes more work. Let $M \equiv \max_i X_i$. First derive the CDF of $M$:

$$P(M \le y) = P(X_1 \le y \text{ and } \cdots \text{ and } X_n \le y) = \prod_{i=1}^n P(X_i \le y) = \big[P(X_1 \le y)\big]^n \quad (X_i \text{'s iid})$$

$$= \left[\int_0^y \frac{1}{\theta},dx\right]^n = (y/\theta)^n.$$

Differentiate to get the pdf of $M$: $$f_M(y) = \frac{d}{dy}(y/\theta)^n = \frac{n y^{n-1}}{\theta^n}.$$

Then: $$\mathrm{E}[M] = \int_0^\theta y, f_M(y),dy = \int_0^\theta \frac{n y^n}{\theta^n},dy = \frac{n\theta}{n+1}.$$

So: $$\mathrm{E}[Y_2] = \frac{n+1}{n},\mathrm{E}[M] = \frac{n+1}{n}\cdot\frac{n\theta}{n+1} = \theta.$$ So $Y_2$ is **also** unbiased for $\theta$. ✅

(Intuition for the $\frac{n+1}{n}$ correction factor: the raw sample max $M$ _always_ underestimates $\theta$ on average — since $M < \theta$ with probability 1 — so it needs to be scaled up to become unbiased.)

### 3.2 But they have very different variances

After similar algebra (using $\mathrm{E}[M^2]$ etc.): $$\mathrm{Var}(Y_1) = \frac{\theta^2}{3n} \qquad \text{and} \qquad \mathrm{Var}(Y_2) = \frac{\theta^2}{n(n+2)}.$$

Comparing: $\mathrm{Var}(Y_2)$ has $n(n+2) \approx n^2$ in the denominator vs. $\mathrm{Var}(Y_1)$'s $3n$ — so **$Y_2$'s variance shrinks much faster as $n$ grows**. For any $n \ge 1$, $Y_2$ has dramatically lower variance than $Y_1$.

### 3.3 Comparing via MSE

Since both estimators are unbiased, $\mathrm{Bias} = 0$ for each, so MSE reduces to just the variance:

$$\mathrm{MSE}(Y_1) = \mathrm{Var}(Y_1) = \frac{\theta^2}{3n}, \qquad \mathrm{MSE}(Y_2) = \mathrm{Var}(Y_2) = \frac{\theta^2}{n(n+2)}.$$

**Conclusion:** Since $\mathrm{MSE}(Y_2) \ll \mathrm{MSE}(Y_1)$ for any $n > 1$ (equivalently, the relative efficiency $\mathrm{MSE}(Y_1)/\mathrm{MSE}(Y_2) = \frac{n+2}{3} > 1$ for $n>1$), $Y_2$ is the far better estimator, despite both being unbiased.

> [!tip] Takeaway Unbiasedness alone doesn't tell you which estimator to prefer. Here, the "obvious" moment-based estimator $2\bar X$ is unbiased but wasteful — it ignores the fact that in a $\mathrm{Unif}(0,\theta)$ sample, the **maximum observed value** is a much more informative statistic about the upper bound $\theta$ than the sample mean is. MSE (via variance, since both are unbiased) makes this precise and lets you rank the two rigorously.

---

## 4. Summary

- **Bias** measures how far off an estimator is _on average_; **variance** measures how much it _fluctuates_ from sample to sample.
- **MSE** unifies both: $\mathrm{MSE}(T) = \mathrm{Var}(T) + \mathrm{Bias}(T)^2$.
- Comparing estimators by MSE (or **relative efficiency** $\mathrm{MSE}(T_1)/\mathrm{MSE}(T_2)$) is more useful than comparing by unbiasedness alone, since it's possible — and common — for a slightly biased estimator to have much lower MSE than an unbiased one.
- Worked example: for $\mathrm{Unif}(0,\theta)$, both $2\bar X$ and $\frac{n+1}{n}\max_i X_i$ are unbiased for $\theta$, but the max-based estimator has far lower variance (and thus MSE), especially as $n$ grows.

## Related

- [[Unbiased Point Estimation]]
- [[Identifying Distributions for Simulation Inputs]]
- [[Maximum Likelihood Estimation]]
- [[Minimum Variance Unbiased Estimators]]
#simulation #statistics #point-estimation

# Unbiased Point Estimation

## 1. Statistics vs. Parameters

> [!info] Definition — Statistic A **statistic** is a function of the observations $X_1, \dots, X_n$ that does **not** explicitly depend on any unknown parameters.

- Examples: $\bar{X} = \dfrac{1}{n}\sum_{i=1}^n X_i$ (sample mean), $S^2 = \dfrac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2$ (sample variance).
- Statistics are themselves **random variables** — if you draw two different samples, you'd generally get two different values of the statistic.
- A statistic is usually computed in order to **estimate** some unknown _parameter_ of the underlying distribution generating the $X_i$'s (e.g., $\mu$, $\sigma^2$).

Contrast:

- **Parameter** ($\mu, \sigma^2, \lambda, \dots$): a fixed but unknown number describing the true distribution.
- **Statistic** ($\bar{X}, S^2, \dots$): a random, computable quantity based on data, used to estimate a parameter.

## 2. Point Estimators

> [!info] Definition — Point Estimator Let $X_1, \dots, X_n$ be i.i.d. random variables and let $T(\mathbf{X}) \equiv T(X_1, \dots, X_n)$ be a statistic based on the $X_i$'s. If $T(\mathbf{X})$ is used to estimate some unknown parameter $\theta$, then $T(\mathbf{X})$ is called a **point estimator** for $\theta$.

- $\bar{X}$ is usually a point estimator for the mean $\mu = \mathrm{E}[X_i]$.
- $S^2$ is often a point estimator for the variance $\sigma^2 = \mathrm{Var}(X_i)$.

### Desirable properties of a point estimator $T(\mathbf{X})$

- Its **expected value should equal the parameter** it's trying to estimate (_unbiasedness_).
- It should have **low variance** (so estimates from different samples don't bounce around too much).

These two properties trade off against each other in general.

## 3. Unbiasedness

> [!info] Definition — Unbiased $T(\mathbf{X})$ is **unbiased** for $\theta$ if $$\mathrm{E}[T(\mathbf{X})] = \theta.$$

If $\mathrm{E}[T(\mathbf{X})] \ne \theta$, the estimator is **biased**, with bias defined as $\mathrm{E}[T(\mathbf{X})] - \theta$.

---

### 3.1 $\bar{X}$ is unbiased for $\mu$

> [!theorem] Theorem Suppose $X_1, \dots, X_n$ are i.i.d. **anything** with mean $\mu$. Then $$\mathrm{E}[\bar{X}] = \mathrm{E}\left[\frac{1}{n}\sum_{i=1}^n X_i\right] = \frac{1}{n}\sum_{i=1}^n \mathrm{E}[X_i] = \mathrm{E}[X_i] = \mu.$$

- This holds for **any** underlying distribution (as long as the mean exists) — that's why $\bar{X}$ is called _the_ sample mean, and why it's the go-to unbiased estimator of $\mu$ regardless of what distribution you're sampling from.

**Baby example:** If $X_1, \dots, X_n \stackrel{iid}{\sim} \mathrm{Exp}(\lambda)$, then $\bar{X}$ is unbiased for $\mu = \mathrm{E}[X_i] = 1/\lambda$.

> [!warning] Careful — unbiasedness doesn't transfer through nonlinear functions Even though $\bar{X}$ is unbiased for $1/\lambda$, the "obvious" estimator $1/\bar{X}$ is **biased** for $\lambda$ itself: $$\mathrm{E}\left[\frac{1}{\bar{X}}\right] \ne \frac{1}{\mathrm{E}[\bar{X}]} = \lambda.$$ This follows from **Jensen's inequality**: since $g(x) = 1/x$ is convex (for $x>0$), $\mathrm{E}[g(\bar X)] \ge g(\mathrm{E}[\bar X])$, with strict inequality unless $\bar X$ is constant. So $\mathrm{E}[1/\bar X] > \lambda$ in general — plugging an unbiased estimator into a nonlinear function does **not** generally preserve unbiasedness.

---

### 3.2 $S^2$ is unbiased for $\sigma^2$

> [!theorem] Theorem Suppose $X_1, \dots, X_n$ are i.i.d. **anything** with mean $\mu$ and variance $\sigma^2$. Then $$\mathrm{E}[S^2] = \mathrm{E}\left[\frac{\sum_{i=1}^n (X_i - \bar{X})^2}{n-1}\right] = \mathrm{Var}(X_i) = \sigma^2.$$

This is exactly why $S^2$ is called the **sample variance**, and why it divides by $n-1$ (not $n$) — the $n-1$ divisor is what makes it unbiased ("Bessel's correction").

**Baby example:** If $X_1, \dots, X_n \stackrel{iid}{\sim} \mathrm{Exp}(\lambda)$, then $S^2$ is unbiased for $\mathrm{Var}(X_i) = 1/\lambda^2$.

#### Proof of the general result

Start with some algebra to rewrite $S^2$: $$S^2 = \frac{\sum_{i=1}^n (X_i - \bar{X})^2}{n-1} = \frac{\sum_{i=1}^n X_i^2 - n\bar{X}^2}{n-1}.$$

Since $\mathrm{E}[X_1] = \mathrm{E}[\bar X]$ and $\mathrm{Var}(\bar X) = \mathrm{Var}(X_1)/n = \sigma^2/n$ (i.i.d. sample mean), we have:

$$\mathrm{E}[S^2] = \frac{\sum_{i=1}^n \mathrm{E}[X_i^2] - n\mathrm{E}[\bar{X}^2]}{n-1} = \frac{n}{n-1}\Big(\mathrm{E}[X_1^2] - \mathrm{E}[\bar{X}^2]\Big)$$

$$= \frac{n}{n-1}\Big(\mathrm{Var}(X_1) + (\mathrm{E}[X_1])^2 - \mathrm{Var}(\bar{X}) - (\mathrm{E}[\bar{X}])^2\Big)$$

$$= \frac{n}{n-1}\left(\sigma^2 - \frac{\sigma^2}{n}\right) = \sigma^2. \qquad \blacksquare$$

The key algebra trick: $\mathrm{E}[Y^2] = \mathrm{Var}(Y) + (\mathrm{E}[Y])^2$ applied to both $X_1$ and $\bar X$, and then the $(\mathrm{E}[X_1])^2 - (\mathrm{E}[\bar X])^2$ terms cancel since $\mathrm{E}[X_1] = \mathrm{E}[\bar X] = \mu$.

> [!warning] Remark — $S$ is biased for $\sigma$ Even though $S^2$ is unbiased for $\sigma^2$, the sample standard deviation $S = \sqrt{S^2}$ is **biased** for $\sigma$. Again this is a Jensen's inequality issue: $g(x) = \sqrt{x}$ is **concave**, so $\mathrm{E}[\sqrt{S^2}] \le \sqrt{\mathrm{E}[S^2]} = \sigma$. In practice $S$ slightly _underestimates_ $\sigma$, though the bias shrinks as $n$ grows.

---

## 4. Takeaways

- **Unbiasedness is about the estimator's expectation matching the true parameter, exactly, for finite $n$** — not just asymptotically (that's [[Consistency of Estimators|consistency]], a different property).
- $\bar X$ is unbiased for $\mu$ **no matter the underlying distribution**, as long as the mean exists.
- $S^2$ (with the $n-1$ divisor) is unbiased for $\sigma^2$, again for **any** distribution with a finite variance.
- Unbiasedness is **not preserved under nonlinear transformations**: $1/\bar X$ is biased for $\lambda$ (even though $\bar X$ is unbiased for $1/\lambda$), and $S$ is biased for $\sigma$ (even though $S^2$ is unbiased for $\sigma^2$). Watch for this whenever you plug an unbiased estimator into a nonlinear function of interest.
- A good point estimator ideally balances **unbiasedness** with **low variance** — see [[Mean Squared Error and the Bias-Variance Tradeoff]] and [[Minimum Variance Unbiased Estimators]] for how these interact.

## Related

- [[Identifying Distributions for Simulation Inputs]]
- [[Maximum Likelihood Estimation]]
- [[Confidence Intervals]]
- [[Mean Squared Error and the Bias-Variance Tradeoff]]
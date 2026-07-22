## The Invariance Property

> [!theorem] Invariance Property
> If $\hat\theta$ is the MLE of some parameter $\theta$, and $h(\cdot)$ is a **one-to-one function**, then $h(\hat\theta)$ is the MLE of $h(\theta)$.

**In words:** once you have the MLE for a parameter, you get the MLE for *any* one-to-one function of that parameter "for free" — no new derivation needed, just plug $\hat\theta$ into $h(\cdot)$.

> [!warning] This is special to MLEs — it does NOT hold for unbiasedness
> Recall from [[Unbiased Point Estimation]]: $\mathrm{E}[S^2] = \sigma^2$ (unbiased), but $\mathrm{E}[\sqrt{S^2}] \ne \sigma$ (biased). Unbiasedness does **not** transfer through nonlinear transformations — but the MLE invariance property says the *maximizing* property **does** transfer through any 1:1 function. This is one of the main practical advantages of MLE over other estimation approaches.

---

### 9.1 Example: MLE of $p^2$ for $X_i \stackrel{iid}{\sim} \mathrm{Bern}(p)$

Recall (from [[Maximum Likelihood Estimators]] §4) the MLE of $p$ is $\hat p = \bar X$, which also happens to be unbiased.

Take the 1:1 function $h(\theta) = \theta^2$ (for $\theta > 0$). By the Invariance Property, the MLE of $p^2$ is:

$$\widehat{p^2} = \hat p^2 = \bar X^2. \qquad \blacksquare$$

> [!note] Careful — this MLE is biased!
> Even though $\hat p = \bar X$ is unbiased for $p$, $\bar X^2$ is **not** unbiased for $p^2$ — again because squaring is a nonlinear transformation (same Jensen's-inequality issue as $1/\bar X$ and $\sqrt{S^2}$ from earlier notes). The Invariance Property guarantees $\bar X^2$ is still the *maximum likelihood* estimate of $p^2$; it says nothing about bias.

---

### 9.2 Example: MLE of $\sigma$ for $X_i \stackrel{iid}{\sim} \mathrm{Nor}(\mu, \sigma^2)$

Recall (from [[Maximum Likelihood Estimators]] §6) the MLE of $\sigma^2$ is:
$$\widehat{\sigma^2} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar X)^2.$$

We'd like the MLE of $\sigma$ itself (not just $\sigma^2$). Take the 1:1 function $h(\theta) = +\sqrt{\theta}$ (valid since $\sigma > 0$, so we only need the positive root). By the Invariance Property:

$$\hat\sigma = \sqrt{\widehat{\sigma^2}} = \sqrt{\frac{\sum_{i=1}^n (X_i - \bar X)^2}{n}}. \qquad \blacksquare$$

> [!tip] Why this matters
> Without the Invariance Property, you'd have to set up an entirely separate likelihood maximization for $\sigma$ directly. Instead, since $\widehat{\sigma^2}$ was already the "hard part," getting $\hat\sigma$ is literally just a square root away.

---

### 9.3 Example: MLE of the Survival Function for $X_i \stackrel{iid}{\sim} \mathrm{Exp}(\lambda)$

Recall the MLE of $\lambda$ is $\hat\lambda = 1/\bar X$ (from [[Maximum Likelihood Estimators]] §3).

Define the **survival function** (also called the *reliability function*):
$$\bar F(x) \equiv P(X > x) = 1 - F(x) = e^{-\lambda x}.$$

This is a function of $\lambda$ (for fixed $x$), and $h(\lambda) = e^{-\lambda x}$ is one-to-one in $\lambda$ (for fixed $x > 0$). By the Invariance Property, the MLE of $\bar F(x)$ is:

$$\widehat{\bar F(x)} = e^{-\hat\lambda x} = e^{-x/\bar X}. \qquad \blacksquare$$

> [!info] Real-world relevance
> This exact kind of calculation — plugging an MLE'd parameter into a survival/reliability function to estimate "probability of surviving past time $x$" — is used constantly in **actuarial science** (e.g., mortality/reliability modeling), reliability engineering, and survival analysis.

---

### 9.4 Summary

- The Invariance Property lets you "chain" MLEs through any 1:1 transformation without redoing the optimization.
- It applies broadly: powers ($p \to p^2$), roots ($\sigma^2 \to \sigma$), and more complex functions (rate $\lambda \to$ survival probability $e^{-\lambda x}$).
- **Don't confuse this with unbiasedness** — the transformed MLE is still the maximum-likelihood estimate of the transformed parameter, but it can easily be biased even when the original MLE wasn't.
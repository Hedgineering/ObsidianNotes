#simulation #statistics #point-estimation

# Maximum Likelihood Estimation (MLE)

Maximum likelihood estimation is perhaps the most popular point estimation method — a very flexible technique that many software packages use under the hood to fit distributions to data. See [[Unbiased Point Estimation]] and [[Mean Squared Error and the Bias-Variance Tradeoff]] for the broader context of what makes a "good" estimator.

## 1. Definitions

> [!info] Definition — Likelihood Function Consider an i.i.d. random sample $X_1, \dots, X_n$, where each $X_i$ has pdf/pmf $f(x)$, and suppose $\theta$ is some unknown parameter of $X_i$. The **likelihood function** is $$L(\theta) \equiv \prod_{i=1}^n f(x_i).$$

- Intuition: $L(\theta)$ measures how "plausible" the observed data $x_1, \dots, x_n$ is, as a function of the candidate parameter value $\theta$. Different $\theta$'s make the observed data more or less likely.

> [!info] Definition — Maximum Likelihood Estimator (MLE) The **maximum likelihood estimator (MLE)** of $\theta$ is the value of $\theta$ that maximizes $L(\theta)$. $$\hat\theta = \arg\max_\theta L(\theta)$$ The MLE is a function of the $X_i$'s and is therefore itself a **random variable**.

**In words:** out of all possible values of $\theta$, pick the one that makes the data you actually observed _most probable_.

## 2. The Log-Likelihood Trick

Maximizing $L(\theta) = \prod_i f(x_i)$ directly usually means differentiating a big product — painful. The standard workaround:

> [!tip] Useful Trick The natural log function $\ln(\cdot)$ is **one-to-one and monotonically increasing**, so whatever value of $\theta$ maximizes $L(\theta)$ _also_ maximizes $\ln(L(\theta))$ (the **log-likelihood**). Since $\ln$ turns products into sums, this is almost always much easier to differentiate.

General recipe:

1. Write down $L(\theta) = \prod_{i=1}^n f(x_i)$.
2. Take $\ell(\theta) \equiv \ln(L(\theta))$ and simplify (products → sums, exponents → multiplication).
3. Differentiate: $\dfrac{d}{d\theta}\ell(\theta) = 0$, and solve for $\theta$.
4. (Optional but proper) Check the second derivative is negative at that point to confirm it's a **maximum**, not a minimum or saddle point.
5. Replace the solved value's lowercase $x_i$'s with uppercase $X_i$'s and put a hat on $\theta$: the result $\hat\theta$ is the MLE, a random variable.

---

## 3. Worked Example 1: MLE for $\lambda$, $X_i \stackrel{iid}{\sim} \mathrm{Exp}(\lambda)$

**Likelihood function:** $$L(\lambda) = \prod_{i=1}^n f(x_i) = \prod_{i=1}^n \lambda e^{-\lambda x_i} = \lambda^n \exp\left(-\lambda \sum_{i=1}^n x_i\right).$$

**Log-likelihood:** $$\ln(L(\lambda)) = \ln\left(\lambda^n \exp\left(-\lambda\sum_{i=1}^n x_i\right)\right) = n\ln(\lambda) - \lambda\sum_{i=1}^n x_i.$$

**Differentiate and set to zero:** $$\frac{d}{d\lambda}\ln(L(\lambda)) = \frac{d}{d\lambda}\left(n\ln(\lambda) - \lambda\sum_{i=1}^n x_i\right) = \frac{n}{\lambda} - \sum_{i=1}^n x_i \equiv 0.$$

**Solve for $\lambda$:** $$\frac{n}{\lambda} = \sum_{i=1}^n x_i \quad\Longrightarrow\quad \lambda = \frac{n}{\sum_i x_i} = \frac{1}{\bar x}.$$

$$\boxed{\hat\lambda = 1/\bar X} \qquad \blacksquare$$

> [!note] Remarks
> 
> 1. $\hat\lambda = 1/\bar X$ makes intuitive sense: since $\mathrm{E}[X] = 1/\lambda$ for $\mathrm{Exp}(\lambda)$, "inverting" the sample mean is a natural way to estimate the rate.
> 2. The hat ($\hat{\ }$) over $\lambda$ signals "this is the MLE" (an estimator), as opposed to the true unknown parameter $\lambda$ itself.
> 3. At the end, lowercase $x_i$'s (specific observed data values) get promoted to uppercase $X_i$'s, signaling that $\hat\lambda$ is a random variable (a function of the random sample), not just a single fitted number.
> 4. Technically you should verify this is a maximum via a second-derivative test, but it's a common (harmless) shortcut to skip it if the shape of the likelihood is obviously concave in the relevant range.

> [!warning] Recall from [[Unbiased Point Estimation]] Even though $\bar X$ is unbiased for $1/\lambda$, the MLE $\hat\lambda = 1/\bar X$ is **biased** for $\lambda$ (a nonlinear transform of an unbiased estimator). MLEs are not guaranteed to be unbiased in general — they have other desirable large-sample properties instead (see §5).

---

## 4. Worked Example 2: MLE for $p$, $X_i \stackrel{iid}{\sim} \mathrm{Bern}(p)$

**Useful trick:** Since $X_i \in {0, 1}$, the pmf can be written compactly as a single formula (instead of a piecewise one): $$f(x) = p^x (1-p)^{1-x}, \qquad x = 0, 1.$$

(Check: at $x=1$, $f(1) = p$; at $x=0$, $f(0) = 1-p$ — matches the Bernoulli pmf exactly.)

**Likelihood function:** $$L(p) = \prod_{i=1}^n f(x_i) = \prod_{i=1}^n p^{x_i}(1-p)^{1-x_i} = p^{\sum_{i=1}^n x_i}(1-p)^{n - \sum_{i=1}^n x_i}.$$

**Log-likelihood:** $$\ln(L(p)) = \sum_{i=1}^n x_i ,\ln(p) + \left(n - \sum_{i=1}^n x_i\right)\ln(1-p).$$

**Differentiate and set to zero:** $$\frac{d}{dp}\ln(L(p)) = \frac{\sum_i x_i}{p} - \frac{n - \sum_i x_i}{1-p} \equiv 0.$$

**Solve for $p$:**

$$\boxed{\hat p = \bar X} \qquad \blacksquare$$

> [!note] Remark $\hat p = \bar X$ makes sense since $\mathrm{E}[X] = p$ for a Bernoulli — the sample proportion of successes is exactly the natural estimate of the success probability. Unlike the exponential example above, this MLE is actually the same as $\bar X$ itself, and is therefore **unbiased** for $p$ (since $\bar X$ is always unbiased for the true mean — see [[Unbiased Point Estimation]]).

---

## 5. General Notes on MLE

- **Not always unbiased** — as seen above, an MLE can be biased in finite samples (e.g., $\hat\lambda = 1/\bar X$), even though it's derived from an unbiased quantity like $\bar X$.
- **Large-sample properties** MLEs are prized for: as $n \to \infty$, under mild regularity conditions, MLEs are:
    - **Consistent** — converge in probability to the true parameter.
    - **Asymptotically unbiased** — bias vanishes as $n$ grows.
    - **Asymptotically efficient** — achieve the lowest possible variance among (asymptotically) unbiased estimators (i.e., they asymptotically attain the Cramér–Rao lower bound).
    - **Asymptotically normal** — the sampling distribution of $\hat\theta$ approaches a normal distribution, which is what underlies large-sample confidence intervals and hypothesis tests for MLEs.
- **Invariance property:** if $\hat\theta$ is the MLE of $\theta$, then for any function $g$, $g(\hat\theta)$ is the MLE of $g(\theta)$. (This is a genuinely useful shortcut — e.g., the MLE of $\theta^2$ is just $\hat\theta^2$, no new derivation needed.)
- **General-purpose:** MLE can be applied to essentially any parametric family with a well-defined pdf/pmf, which is why it's the default distribution-fitting method in most statistical software (as flagged in the lesson intro).
- Always double check whether the log-likelihood is actually maximized (not minimized) at the critical point — via a second-derivative test, or by reasoning about the shape of the likelihood, or by checking boundary behavior for parameters with restricted domains (e.g., $\mathrm{Unif}(0,\theta)$, where the MLE isn't found by differentiation at all, but by inspection of where $L(\theta)$ is largest).

---

## 6. Worked Example 3: Simultaneous MLEs for $\mu, \sigma^2$, $X_i \stackrel{iid}{\sim} \mathrm{Nor}(\mu,\sigma^2)$

This is a **two-parameter** MLE problem — both $\mu$ and $\sigma^2$ are unknown, so you take partial derivatives with respect to each and solve simultaneously.

**Likelihood function:**
$$L(\mu,\sigma^2) = \prod_{i=1}^n f(x_i) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left\{-\frac{1}{2}\frac{(x_i-\mu)^2}{\sigma^2}\right\} = \frac{1}{(2\pi\sigma^2)^{n/2}}\exp\left\{-\frac{1}{2}\sum_{i=1}^n \frac{(x_i-\mu)^2}{\sigma^2}\right\}.$$

**Log-likelihood:**
$$\ln(L(\mu,\sigma^2)) = -\frac{n}{2}\ln(2\pi) - \frac{n}{2}\ln(\sigma^2) - \frac{1}{2\sigma^2}\sum_{i=1}^n(x_i-\mu)^2.$$

**Partial w.r.t. $\mu$:**
$$\frac{\partial}{\partial\mu}\ln(L(\mu,\sigma^2)) = \frac{1}{\sigma^2}\sum_{i=1}^n(x_i-\mu) \equiv 0 \quad\Longrightarrow\quad \hat\mu = \bar X.$$

> [!note] This makes sense — the MLE for the population mean is just the sample mean, same story as in the Bernoulli example.

**Partial w.r.t. $\sigma^2$** (note: differentiate with respect to $\sigma^2$ as a single variable, **not** $\sigma$):
$$\frac{\partial}{\partial\sigma^2}\ln(L(\mu,\sigma^2)) = -\frac{n}{2\sigma^2} + \frac{1}{2\sigma^4}\sum_{i=1}^n (x_i-\hat\mu)^2 \equiv 0.$$

Solving (plugging in $\hat\mu$ from above):
$$\boxed{\widehat{\sigma^2} = \frac{\sum_{i=1}^n (X_i-\bar X)^2}{n}} \qquad \blacksquare$$

> [!warning] Compare to $S^2$ from [[Unbiased Point Estimation]]
> This MLE divides by $n$, **not** $n-1$. Recall $S^2 = \frac{\sum_i (X_i-\bar X)^2}{n-1}$ is the *unbiased* estimator of $\sigma^2$. So the MLE $\widehat{\sigma^2}$ is a **biased** (specifically, downward-biased) estimator of $\sigma^2$ — another example (like $\hat\lambda = 1/\bar X$) of an MLE that isn't unbiased. This is exactly why the sample variance formula uses $n-1$: it's a bias-correction of the "natural"/MLE version that divides by $n$.

---

## 7. Worked Example 4: Simultaneous MLEs for $r, \lambda$, $X_i \stackrel{iid}{\sim} \mathrm{Gam}(r,\lambda)$

**pdf:** $f(x) = \dfrac{\lambda^r}{\Gamma(r)} x^{r-1} e^{-\lambda x}$, $x>0$, where $\Gamma(\cdot)$ is the Gamma function.

**Likelihood function:**
$$L(r,\lambda) = \prod_{i=1}^n f(x_i) = \frac{\lambda^{nr}}{[\Gamma(r)]^n}\left(\prod_{i=1}^n x_i\right)^{r-1} e^{-\lambda\sum_i x_i}.$$

**Log-likelihood:**
$$\ln(L) = rn\ln(\lambda) - n\ln(\Gamma(r)) + (r-1)\ln\left(\prod_i x_i\right) - \lambda\sum_i x_i.$$

**Partial w.r.t. $\lambda$:**
$$\frac{\partial}{\partial\lambda}\ln(L) = \frac{rn}{\lambda} - \sum_{i=1}^n x_i \equiv 0 \quad\Longrightarrow\quad \hat\lambda = \hat r/\bar X.$$

**Partial w.r.t. $r$** — this is where it gets messy:
$$\frac{\partial}{\partial r}\ln(L) = n\ln(\lambda) - \frac{n}{\Gamma(r)}\frac{d}{dr}\Gamma(r) + \ln\left(\prod_i x_i\right) \equiv 0.$$

> [!info] The digamma function
> Define $\Psi(r) \equiv \Gamma'(r)/\Gamma(r)$ — this is called the **digamma function**. It shows up naturally whenever you differentiate $\ln(\Gamma(r))$ (since $\frac{d}{dr}\ln\Gamma(r) = \Gamma'(r)/\Gamma(r) = \Psi(r)$), which is unavoidable for Gamma-family MLEs.

Substituting $\hat\lambda = \hat r/\bar X$ into the $r$-equation gives a single equation in $\hat r$ alone:
$$n\ln(\hat r/\bar X) - n\Psi(\hat r) + \ln\left(\prod_i x_i\right) \equiv 0.$$

**There's no closed-form solution for $\hat r$** — this must be solved **numerically** (bisection, Newton's method, etc.), since $\Psi(r)$ has no elementary closed form.

> [!tip] Numerical fallback for $\Gamma'(r)$
> If you can't find/use a closed-form expression for $\Gamma'(r)$ directly, approximate it with a finite difference for small $h$:
> $$\Gamma'(r) \approx \frac{\Gamma(r+h) - \Gamma(r)}{h}.$$

**Takeaway:** not every MLE has a clean closed-form solution — sometimes the best you can do is set up the estimating equation(s) and hand them off to a numerical solver.

---

## 8. Worked Example 5: MLE for $\theta$, $X_i \stackrel{iid}{\sim} \mathrm{Unif}(0,\theta)$

This example is **fundamentally different** from the previous ones — the domain of $X_i$ depends on $\theta$, so ordinary calculus (setting a derivative to 0) doesn't apply. You have to reason about the likelihood directly.

**pdf:** $f(x) = 1/\theta$ for $0 < x < \theta$ (beware of the "funny," $\theta$-dependent limits).

**Likelihood function:**
$$L(\theta) = \prod_{i=1}^n f(x_i) = \begin{cases} 1/\theta^n & \text{if } 0 \le x_i \le \theta,\ \forall i \\ 0 & \text{otherwise} \end{cases}$$

**Reasoning (no differentiation needed):**
- For $L(\theta) > 0$ at all, we need **every** $x_i \le \theta$ — i.e., $\theta \ge \max_i x_i$.
- Subject to that constraint, $L(\theta) = 1/\theta^n$ is a **decreasing** function of $\theta$ (bigger $\theta^n$ in the denominator = smaller likelihood).
- So to maximize $L(\theta)$, pick $\theta$ **as small as the constraint allows** — i.e., the smallest permissible value is $\theta = \max_i x_i$.

$$\boxed{\hat\theta = \max_{1\le i \le n} X_i} \qquad \blacksquare$$

> [!note] Connects back to [[Mean Squared Error and the Bias-Variance Tradeoff]]
> This result makes sense in light of the unbiased estimator $Y_2 = \frac{n+1}{n}\max_i X_i$ examined earlier for the same Uniform(0,θ) setup: the MLE $\hat\theta = \max_i X_i$ is essentially $Y_2$ **without** the bias-correcting $\frac{n+1}{n}$ factor. Since $\max_i X_i < \theta$ always (probability 1), the raw MLE **systematically underestimates** $\theta$ — i.e., it's biased downward, and $Y_2$ is the "fixed up," unbiased version of this same basic idea.

> [!tip] General lesson
> When a distribution's support depends on the parameter itself (like $\mathrm{Unif}(0,\theta)$), you can't just take $\frac{d}{d\theta}L(\theta)=0$ — the likelihood is often not even differentiable in the usual sense across its full domain. Instead, reason about where the likelihood is largest **subject to the support constraint**.


Read more into [[Invariance Property of Maximum Likelihood Expectations]] for cases where you need to chain multiple Maximum Likelihood Expectations.
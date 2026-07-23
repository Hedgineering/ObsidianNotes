#statistics #hypothesis-testing #goodness-of-fit #chi-squared #exponential

# Goodness-of-Fit Test: Continuous Case (Exponential Example)

Companion note to [[Goodness of Fit Tests]] — this one covers the **continuous-distribution** version of the chi-squared g-o-f test using **equal-probability intervals**.

---

## Continuous Distributions & Equal-Probability Intervals

For the continuous case, denote the intervals

$$ A_i \equiv (a_{i-1}, a_i], \qquad i = 1, 2, \dots, k $$

For convenience, we choose the $a_i$'s so that we get **equal-probability intervals**, i.e.,

$$ p_i = P(X \in A_i) = P(a_{i-1} < X \le a_i) = \frac{1}{k} \quad \text{for all } i $$

> **Why equal-probability?** It guarantees $E_i = np_i = n/k$ is the same for every cell, which makes it easy to satisfy the "$E_i \ge 5$" rule of thumb and simplifies the arithmetic.

---

## Example: Are Interarrival Times Exponential?

**Question:** Suppose we're interested in fitting a distribution to a series of interarrival times. Could they be **Exponential**?

$$ H_0 : X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Exp}(\lambda) $$

We'll run a $\chi^2$ g-o-f test with equal-probability intervals.

### Step 1 — Find the interval endpoints $a_i$ (assuming $\lambda$ known)

We need $a_i$ such that $F(a_i) = i/k$:

$$ F(a_i) = P(X \le a_i) = 1 - e^{-\lambda a_i} = \frac{i}{k}, \qquad i = 1, 2, \dots, k $$

Solving for $a_i$:

$$ a_i = -\frac{1}{\lambda}\ln!\left(1 - \frac{i}{k}\right), \qquad i = 1, 2, \dots, k $$

### Step 2 — But $\lambda$ is unknown

Since $\lambda$ is unknown, we must **estimate** it — this uses up $s = 1$ parameter.

**Good news:** we already know the MLE is $$ \hat\lambda = \frac{1}{\bar X} $$

By the **Invariance Property of MLEs**, the MLEs of the interval endpoints $a_i$ are

$$ \hat a_i = -\frac{1}{\hat\lambda}\ln!\left(1 - \frac{i}{k}\right) = -\bar X \ln!\left(1 - \frac{i}{k}\right), \qquad i = 1, \dots, k $$

### Step 3 — Plug in the data

Suppose we take $n = 100$ observations, divided into $k = 5$ equal-probability intervals, and the sample mean is $\bar X = 9.0$.

$$ \hat a_i = -9.0,\ln(1 - 0.2i), \qquad i = 1, \dots, 5 $$

### Step 4 — Tally observations and compute the test statistic

Determine which interval each of the 100 observations falls into to get the $O_i$'s. Since intervals are equal-probability, $E_i = n/k = 100/5 = 20$ for every cell.

|interval $(a_{i-1}, a_i]$|$O_i$|$E_i = n/k$|
|---|---|---|
|$(0, 2.01]$|25|20|
|$(2.01, 4.60]$|27|20|
|$(4.60, 8.25]$|23|20|
|$(8.25, 14.48]$|13|20|
|$(14.48, \infty)$|12|20|
|**Total**|**100**|**100**|

$$ \chi_0^2 = \sum_{i=1}^k \frac{(O_i - E_i)^2}{E_i} = 9.80 $$

### Step 5 — Compare to the critical value

- $k = 5$ intervals, $s = 1$ parameter estimated ($\lambda$)
- degrees of freedom: $k - 1 - s = 5 - 1 - 1 = 3$
- at $\alpha = 0.05$: $\chi^2_{\alpha, k-1-s} = \chi^2_{0.05, 3} = 7.81$

Since $\chi_0^2 = 9.80 > 7.81 = \chi^2_{0.05,3}$, we **reject** $H_0$.

**Conclusion:** these observations ain't Expo. $\square$

---

## Summary / Key Takeaways

- For **continuous** distributions, use **equal-probability intervals** $(a_{i-1}, a_i]$ chosen so $P(X \in A_i) = 1/k$ — this keeps every $E_i = n/k$, satisfying the $E_i \ge 5$ rule automatically (for reasonable $n, k$).
- Interval endpoints $a_i$ come from inverting the hypothesized CDF: $F(a_i) = i/k$.
- If distribution parameters (like $\lambda$) are unknown, estimate them via MLE, then use the **Invariance Property** to get $\hat a_i$ directly — no need to redo the derivation from scratch.
- Degrees of freedom for the chi-squared reference distribution: $\nu = k - 1 - s$, where $s$ = number of estimated parameters.
- Same decision rule as the discrete case: reject $H_0$ if $\chi_0^2 > \chi^2_{\alpha,\nu}$.

#statistics #hypothesis-testing #goodness-of-fit #chi-squared

# Goodness-of-Fit (G-o-F) Tests

## Motivating Idea

> Does the choice of a particular simulation input distribution actually reflect reality?

After we've **guessed a reasonable distribution** (e.g. via a histogram / Q-Q plot) and **estimated its parameters** (e.g. via [[Method of Moments]] or MLE), we need a **formal test** to check how good that fit really is.

---

## The Hypothesis Test

We test

$$ H_0 : X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \text{p.m.f./p.d.f. } f(x) $$

at level of significance

$$ \alpha \equiv P(\text{Reject } H_0 \mid H_0 \text{ true}) = P(\text{Type I error}) $$

(usually $\alpha = 0.05$ or $0.01$).

---

## High-Level Procedure

1. **Divide** the domain of $f(x)$ into $k$ sets $A_1, A_2, \dots, A_k$ (distinct points if $X$ is discrete, or intervals if $X$ is continuous).
    
2. **Tally** the actual (observed) number of observations falling in each set: $$O_i, \quad i = 1, \dots, k$$ If $p_i \equiv P(X \in A_i)$, then $O_i \sim \mathrm{Bin}(n, p_i)$.
    
3. **Determine the expected** number of observations in each set under $H_0$: $$ E_i = \mathrm{E}[O_i] = np_i, \qquad i=1,\dots,k $$
    
4. **Calculate the test statistic** based on the differences between $E_i$'s and $O_i$'s — the **chi-squared g-o-f statistic**: $$ \chi_0^2 \equiv \sum_{i=1}^k \frac{(O_i - E_i)^2}{E_i} $$
    
5. **Decision rule**: a large $\chi_0^2$ indicates a bad fit.
    
    - **Reject** $H_0$ if $\chi_0^2 > \chi^2_{\alpha, k-1-s}$
    - **Fail to reject** $H_0$ if $\chi_0^2 \le \chi^2_{\alpha, k-1-s}$
    
    where:
    
    - $s$ = number of unknown parameters of $f(x)$ that had to be estimated from the data (e.g., $s=2$ for $\mathrm{Nor}(\mu,\sigma^2)$).
    - $\chi^2_{\alpha,\nu}$ is the $(1-\alpha)$ quantile of the $\chi^2_\nu$ distribution: $$P(\chi^2_\nu < \chi^2_{\alpha,\nu}) = 1-\alpha$$

> **Intuition:** each parameter you had to estimate "uses up" one degree of freedom, since fitting the parameters to the data artificially makes $O_i$ closer to $E_i$.

---

## Practical Remarks

- **Rule of thumb** for the test to work well: choose $k, n$ such that $E_i \ge 5$ for all $i$, and $n \ge 30$. If some $E_i < 5$, **combine adjacent cells** until this holds.
- If the degrees of freedom $\nu = k-1-s$ is very large, use the approximation: $$ \chi^2_{\alpha,\nu} \approx \nu\left[1 - \frac{2}{9\nu} + z_\alpha\sqrt{\frac{2}{9\nu}}\right]^3 $$ where $z_\alpha$ is the corresponding standard normal quantile.
- **Other g-o-f tests** exist besides chi-squared: Kolmogorov–Smirnov, Anderson–Darling, Shapiro–Wilk, etc.

---

## Baby Example: Testing Uniform(0,1)

Test $H_0$: $X_i \sim \mathrm{Unif}(0,1)$, with $n = 1000$ observations divided into $k=5$ equal-width intervals.

|interval|$[0,0.2]$|$(0.2,0.4]$|$(0.4,0.6]$|$(0.6,0.8]$|$(0.8,1.0]$|
|---|---|---|---|---|---|
|$E_i$|200|200|200|200|200|
|$O_i$|179|208|222|199|192|

$$ \chi_0^2 = \sum_{i=1}^k \frac{(O_i-E_i)^2}{E_i} = 5.27 $$

- $\alpha = 0.05$, no unknown parameters $\Rightarrow s=0$
- $\chi^2_{\alpha,k-1-s} = \chi^2_{0.05,4} = 9.49$

Since $\chi_0^2 \le \chi^2_{0.05,4}$ ⟹ **fail to reject** $H_0$. $\square$

---

## Discrete Example: Geometric Distribution (Circuit Board Defects)

**Setup:** the number of defects in printed circuit boards is hypothesized to follow $\mathrm{Geometric}(p)$. A sample of $n=70$ boards gives:

|# defects|frequency|
|---|---|
|1|34|
|2|18|
|3|2|
|4|9|
|5|7|
|**Total**|**70**|

$H_0$: the observations are $\mathrm{Geom}(p)$.

### Step 1 — Estimate $p$ via MLE

Likelihood: $$ L(p) = \prod_{i=1}^n f(x_i) = \prod_{i=1}^n (1-p)^{x_i-1}p = (1-p)^{\sum x_i - n}p^n $$

Log-likelihood and derivative: $$ \ln(L(p)) = \left(\sum_{i=1}^n x_i - n\right)\ln(1-p) + n\ln(p) $$ $$ \frac{d,\ln(L(p))}{dp} = \frac{-\sum_{i=1}^n x_i + n}{1-p} + \frac{n}{p} = 0 $$

Solving gives the familiar MLE: $$ \hat p = \frac{1}{\bar X} = \frac{70}{1(34)+2(18)+3(2)+4(9)+5(7)} = 0.476 $$

### Step 2 — Build the expected-counts table

By the **Invariance Property of MLEs**, the expected number of boards with value $x$ is $$ E_x = nP(X=x) = n(1-\hat p)^{x-1}\hat p $$

Combine the tail so probabilities sum to 1:

|$x$|$P(X=x)$|$E_x$|$O_x$|
|---|---|---|---|
|1|0.4762|33.33|34|
|2|0.2494|17.46|18|
|3|0.1307|9.15|2|
|4|0.0684|**4.79**|9|
|$\ge 5$|0.0752|5.27|7|
|**Total**|1.0000|70|70|

Since $E_4 = 4.79 < 5$, **combine the last two cells** ($x=4$ and $x \ge 5$) to satisfy the $E_i \ge 5$ rule:

|$x$|$P(X=x)$|$E_x$|$O_x$|
|---|---|---|---|
|1|0.4762|33.33|34|
|2|0.2494|17.46|18|
|3|0.1307|9.15|2|
|$\ge 4$|0.1436|10.06|16|
|**Total**|1.0000|70|70|

### Step 3 — Compute the test statistic

$$ \chi_0^2 = \sum_{x=1}^4 \frac{(E_x - O_x)^2}{E_x} = \frac{(33.33-34)^2}{33.33} + \cdots = 9.12 $$

### Step 4 — Compare to critical value

- $k = 4$ cells (after combining), $s = 1$ parameter estimated ($p$)
- $\alpha = 0.05 \Rightarrow \chi^2_{\alpha,k-1-s} = \chi^2_{0.05,2} = 5.99$

Since $\chi_0^2 = 9.12 > 5.99 = \chi^2_{0.05,2}$, we **reject** $H_0$.

**Conclusion:** the number of defects probably **isn't** Geometric. $\square$

---

## Summary / Key Takeaways

- G-o-F testing formalizes "does this data plausibly come from this distribution?"
- Compute $\chi_0^2 = \sum \frac{(O_i-E_i)^2}{E_i}$ and compare to $\chi^2_{\alpha, k-1-s}$.
- **Degrees of freedom** = (# cells $-1$) $-$ (# estimated parameters).
- Cells with small expected counts ($E_i < 5$) should be **merged** before running the test.
- A **large** $\chi_0^2$ ⟹ **reject** $H_0$ (bad fit); a **small** $\chi_0^2$ ⟹ **fail to reject** (fit looks fine).
- Chi-squared is only one option — K-S, Anderson–Darling, and Shapiro–Wilk are alternatives depending on context (e.g., continuous data, normality testing).

## Related
- [[Goodness of Fit Tests Exponential Example]]
- [[Goodness of Fit Tests Weibull Example]]
- [[Other Goodness of Fit Tests]]
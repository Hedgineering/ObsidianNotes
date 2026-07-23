#statistics #hypothesis-testing #goodness-of-fit #kolmogorov-smirnov

# Other Goodness-of-Fit Tests: Kolmogorov–Smirnov (and Friends)

The chi-squared test isn't the only option — this note covers the **Kolmogorov–Smirnov (K-S) test**, plus a quick tour of other g-o-f tests.

---

## Why Look Beyond Chi-Squared?

There are plenty of g-o-f tests you can use instead of a $\chi^2$ test. The **K-S test** is one that works well in **low-data situations** — it doesn't require binning data into cells or the "$E_i \ge 5$" rule that the chi-squared test needs.

---

## Setup: The Empirical CDF

We test

$$ H_0 : X_1, X_2, \dots, X_n \overset{\text{iid}}{\sim} \text{some distribution with c.d.f. } F(x) $$

Recall the **empirical c.d.f.** (a.k.a. **sample c.d.f.**) of the data $X_1, \dots, X_n$:

$$ \hat F_n(x) \equiv \frac{\text{number of } X_i\text{'s} \le x}{n} $$

$\hat F_n(x)$ is a **step function** with jumps of height $1/n$ every time an observation occurs.

> **Example:** the empirical CDF of 10 $\mathrm{Exp}(1)$ observations closely tracks the true $\mathrm{Exp}(1)$ CDF, stair-stepping up around the smooth curve.

---

## The Glivenko–Cantelli Lemma

The **Glivenko–Cantelli Lemma** says that $\hat F_n(x) \to F(x)$ for all $x$ as $n \to \infty$. So if $H_0$ is true, $\hat F_n(x)$ should be a good approximation to the true c.d.f. $F(x)$ for large $n$.

**Main question:** Does the empirical distribution actually support the assumption that $H_0$ is true?

---

## The K-S Test Statistic

The K-S test rejects $H_0$ if

$$ D \equiv \max_{x \in \mathbb{R}} |F(x) - \hat F_n(x)| > D_{\alpha,n} $$

where:

- $\alpha$ is the level of significance
- $D_{\alpha,n}$ is a K-S quantile that depends on the hypothesized c.d.f. $F(x)$ (looked up from a table)

---

## Baby K-S Example: Testing Uniform(0,1)

Test $H_0$: $X_1, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Unif}(0,1)$.

Under the $\mathrm{Unif}(0,1)$ assumption, $F(x) = x$, so the statistic simplifies:

$$ D = \max_{x\in\mathbb{R}} |F(x)-\hat F_n(x)| = \max_{0\le x\le 1}|x - \hat F_n(x)| $$

The max can only occur when $x$ equals one of the observations $X_1, \dots, X_n$ — i.e., at one of the **jump points** of $\hat F_n(x)$. At the $i$th jump point, $\hat F_n(x)$ increases from $\frac{i-1}{n}$ to $\frac{i}{n}$.

### Computing $D$: An Easy Algorithm

First define the **ordered points** $X_{(1)} \le X_{(2)} \le \cdots \le X_{(n)}$.

> **Example:** if $X_1=4$, $X_2=1$, $X_3=6$, then $X_{(1)}=1$, $X_{(2)}=4$, $X_{(3)}=6$.

Then compute:

$$
D^+ \equiv \max_{1\le i\le n}\left\{\frac{i}{n} - X_{(i)}\right\}, \qquad
D^- \equiv \max_{1\le i\le n}\left\{X_{(i)} - \frac{i-1}{n}\right\}
$$
and finally, $$ D = \max(D^+, D^-) $$

> **Intuition:** $D^+$ captures the largest gap where the empirical CDF lags _below_ the hypothesized CDF right before a jump; $D^-$ captures the largest gap where it sits _above_ right after a jump.

---

## Numerical Example (from BCNN)

Data: $X_i = 0.039,\ 0.706,\ 0.016,\ 0.198,\ 0.793$ (so $n=5$).

| | | | | | |
|---|---|---|---|---|---|
| $X_i$ | 0.039 | 0.706 | 0.016 | 0.198 | 0.793 |
| $X_{(i)}$ | 0.016 | 0.039 | 0.198 | 0.706 | 0.793 |
| $i/n$ | 0.2 | 0.4 | 0.6 | 0.8 | 1.0 |
| $(i-1)/n$ | 0 | 0.2 | 0.4 | 0.6 | 0.8 |
| $i/n - X_{(i)}$ | 0.184 | 0.361 | **0.402** | 0.094 | 0.207 |
| $X_{(i)} - (i-1)/n$ | 0.016 | — | — | **0.106** | — |

So: $$ D^+ = 0.402, \qquad D^- = 0.106, \qquad D = \max(D^+,D^-) = 0.402 $$

Looking up a K-S table for the uniform distribution: $D_{\alpha,n} = D_{0.05,5} = 0.565$.

Since $D = 0.402 \le 0.565 = D_{0.05,5}$, we **fail to reject** uniformity. $\square$

---

## Remarks

- **K-S is conservative** — it takes a lot of "bad news" (a large discrepancy) to reject $H_0$.
- K-S can easily be applied to **other distributions** beyond Uniform, using the appropriate $F(x)$ and corresponding $D_{\alpha,n}$ tables.
- **Other g-o-f tests** exist beyond chi-squared and K-S:
    - **Anderson–Darling**
    - **Cramér–von Mises**
    - **Shapiro–Wilk**
- **Shapiro–Wilk (S-W)** is especially appropriate for **testing normality**.
- **Graphical techniques** like **Q–Q plots** can also be used to evaluate normality (or fit to other distributions) informally.

---

## Comparison at a Glance

|Test|Good for|Notes|
|---|---|---|
|Chi-squared ($\chi^2$)|Discrete or binned continuous data, large $n$|Requires $E_i \ge 5$ rule; loses info from binning|
|Kolmogorov–Smirnov (K-S)|Continuous data, small samples|Conservative; uses raw empirical CDF, no binning|
|Anderson–Darling|Continuous data|More sensitive to tail differences than K-S|
|Cramér–von Mises|Continuous data|Similar family to A-D/K-S, based on integrated squared difference|
|Shapiro–Wilk|Normality specifically|Standard choice for normality testing|
|Q–Q plot|Any distribution|Graphical/informal, good first check|

---

## Summary / Key Takeaways

- The K-S test compares the **empirical CDF** $\hat F_n(x)$ directly to the hypothesized CDF $F(x)$, using the max absolute deviation $D$.
- Justified by the **Glivenko–Cantelli Lemma**: $\hat F_n(x) \to F(x)$ as $n \to \infty$ under $H_0$.
- $D$ can be computed exactly at the **ordered data points** via the $D^+$/$D^-$ formulas — no need to search over all $x$.
- Reject $H_0$ if $D > D_{\alpha,n}$ (critical value depends on the hypothesized distribution and $n$).
- K-S tends to be **conservative** (harder to reject $H_0$ than $\chi^2$ in some cases).
- Beyond $\chi^2$ and K-S, other options (Anderson–Darling, Cramér–von Mises, Shapiro–Wilk) exist, each suited to different situations — S-W in particular is the go-to for normality testing.

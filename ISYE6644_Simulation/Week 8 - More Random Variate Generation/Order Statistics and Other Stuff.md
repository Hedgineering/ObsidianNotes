# Order Statistics

## What Is a "First Order Statistic"?

There are two unrelated uses of this phrase from different fields — not a real conflict within one definition.

### 1. Order Statistics (Probability / Mathematical Statistics)

Given a sample $X_1, \ldots, X_n$, sort them from smallest to largest:

$$ X_{(1)} \le X_{(2)} \le \cdots \le X_{(n)} $$

The subscripts in parentheses denote _rank_, not original index. So:

- $X_{(1)} = \min{X_1,\ldots,X_n}$ is the **first order statistic**
- $X_{(n)} = \max{X_1,\ldots,X_n}$ is the **$n$-th (last) order statistic**
- $X_{(2)}$ is the second-smallest, and so on

This is the standard, unambiguous definition used in probability theory, mathematical statistics, and simulation. "Order" here literally means _rank order_ in the sorted sample.

### 2. "First-Order Statistics" (Signal / Image Processing)

In a totally different field — image processing, texture analysis, time series — "first-order statistics" means something else entirely: statistics computed from individual values in isolation, ignoring any spatial or sequential relationship between them. This includes the mean, variance, skewness, kurtosis, and histogram-based measures.

"Second-order statistics" then refers to measures that describe the relationship _between pairs_ of values — covariance, correlation, co-occurrence matrices (e.g., GLCM in texture analysis), autocorrelation in time series, etc. Here "order" refers to how many points you're relating at once (one at a time vs. pairs), not rank in a sorted list.

### Bottom Line

The two definitions aren't competing answers to the same question — they're different vocabularies from different subfields that happen to reuse the word "order." In a simulation/probability context, "first order statistic" unambiguously means the **minimum**.

## Setup

Suppose $X_1, X_2, \ldots, X_n$ are i.i.d. from some distribution with c.d.f. $F(x)$, and let

$$ Y = \min{X_1, \ldots, X_n} $$

with c.d.f. $G(y)$. ($Y$ is called the **first order statistic**.)

> [!question] Can we generate $Y$ using just _one_ $\mathcal{U}(0,1)$? Yes! Even though $Y$ is defined in terms of $n$ different $X_i$'s, we don't need $n$ separate uniforms — a single uniform, run through the right inverse transform, suffices.

## Deriving G(y)

Since the $X_i$'s are i.i.d., we can write:

$$ G(y) = 1 - P(Y > y) = 1 - P\left(\min_i X_i > y\right) $$

$$ = 1 - P(\text{all } X_i\text{'s} > y) = 1 - [P(X_1 > y)]^n $$

$$ = 1 - [1 - F(y)]^n $$

**Key idea:** the minimum exceeds $y$ if and only if _every single_ $X_i$ exceeds $y$ — and since the $X_i$'s are independent, that probability is just $[P(X_1 > y)]^n$.

## Inverse Transform for Y

Set $G(Y) = U$ and solve for $Y$. After a little algebra (don't be afraid), we get:

$$ Y = F^{-1}\left(1 - (1-U)^{1/n}\right) $$

This lets us generate the minimum of $n$ i.i.d. draws using only **one** uniform random number and the inverse c.d.f. of the parent distribution.

## Example: Exponential

Suppose $X_1, \ldots, X_n \sim \text{Exp}(\lambda)$. Then:

$$ G(y) = 1 - (e^{-\lambda y})^n = 1 - e^{-n\lambda y} $$

This is recognizable as an exponential c.d.f. with rate $n\lambda$. Thus:

$$ Y = \min_i {X_i} \sim \text{Exp}(n\lambda) $$

So we can simply take:

$$ Y = -\frac{1}{n\lambda}\ln(U) \qquad \blacksquare $$

> [!tip] Nice closed form The minimum of $n$ i.i.d. exponentials is itself exponential (with the rate scaled up by $n$) — a classic "memoryless" property result, and it means generating the min costs no more work than generating a single exponential.

## What About the Maximum?

The same general approach (derive $G(y)$ for $Z = \max_i X_i$ using $P(Z \le y) = [F(y)]^n$, then invert) works for the **largest order statistic**:

$$ Z = \max_i X_i $$

Analogous derivation: since $Z \le y$ iff _all_ $X_i \le y$,

$$ G(y) = P(Z \le y) = [F(y)]^n $$

and inverse transform gives $Z = F^{-1}(U^{1/n})$.

---

# Other Quickies (Distributional Relationships)

A few useful facts that let you generate related distributions once you already have standard normals in hand.

### Chi-Square Distribution

If $Z_1, Z_2, \ldots, Z_n$ are i.i.d. $\text{Nor}(0,1)$, then

$$ \sum_{i=1}^{n} Z_i^2 \sim \chi^2(n) $$

### t Distribution

If $Z \sim \text{Nor}(0,1)$ and $Y \sim \chi^2(n)$, and $Z$ and $Y$ are independent, then

$$ \frac{Z}{\sqrt{Y/n}} \sim t(n) $$

> [!note] $t(1)$ is the **Cauchy distribution** — consistent with the earlier Box-Muller corollary that $Z_2/Z_1 \sim \text{Cauchy}$.

### F Distribution

If $X \sim \chi^2(n)$ and $Y \sim \chi^2(m)$, and $X$ and $Y$ are independent, then

$$ \frac{X/n}{Y/m} \sim F(n,m) $$

### Continuous Empirical Distributions

Generating RVs directly from continuous empirical (data-driven) distributions is its own topic — one practical tool is the **CONT** function in Arena, which builds a generator directly from empirical data without requiring a named parametric family.

## Related

- [[Composition of Random Variables]]
- [[Box-Muller Standard Normal Random Variable Generation]]
- [[Inverse Transform Theorem]]
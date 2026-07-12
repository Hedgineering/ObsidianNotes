
> [!abstract] Core Idea **Convolution** refers to _adding things up_. If a target random variable $Y$ can be written as a **sum of simpler, independent random variables**, $$Y = X_1 + X_2 + \dots + X_n$$ then we can generate $Y$ by generating the individual $X_i$'s (using whatever method works for them, e.g. Inverse Transform) and just **adding them together**.

This is useful whenever a distribution's definition (or a known theorem) expresses it as a sum of i.i.d. or independent simpler random variables.

---

## General Recipe

1. Identify that $Y \sim \text{target distribution}$ can be represented as $Y = \sum_{i=1}^n X_i$ for some simpler $X_i$'s.
2. Generate each $X_i$ (often via Inverse Transform, using i.i.d. Uniform$(0,1)$ draws $U_i$).
3. Sum the $X_i$'s to obtain one realization of $Y$.

> [!note] Trade-off Convolution is often **conceptually simple** and easy to code, but it may require generating **multiple** uniforms (or other RVs) per single realization of $Y$ — so it isn't always the _fastest_ method, even when it's the _easiest_.

---

## Example 1 — Binomial$(n,p)$

If $X_1, \dots, X_n \overset{iid}{\sim} \text{Bernoulli}(p)$, then $$Y = \sum_{i=1}^n X_i \sim \text{Binomial}(n,p).$$

**Generating each Bernoulli via Inverse Transform:**

- Draw $U_i \sim \mathcal{U}(0,1)$.
- If $U_i \le p$, set $X_i = 1$; otherwise $X_i = 0$.
- Repeat for $i = 1, \dots, n$, then sum to get $Y$.

**Worked example:** $Y \sim \text{Bin}(3, 0.4)$, with $U_1 = 0.63,\ U_2 = 0.17,\ U_3 = 0.81$: $$Y = 0 + 1 + 0 = 1$$ (since only $U_2 = 0.17 \le 0.4$).

---

## Example 2 — Triangular$(0,1,2)$

If $U_1, U_2 \overset{iid}{\sim} \mathcal{U}(0,1)$, then $$U_1 + U_2 \sim \text{Triangular}(0,1,2).$$

Two independent "boxes" (uniform densities) sum to form a **triangular** density — visually, two rectangles convolve into a triangle:

```
  [0,1] Uniform  +  [0,1] Uniform  =  Triangular(0,1,2)
     ▭                  ▭                  ▲
```

> [!tip] This is **easier** than deriving/inverting the Triangular CDF directly — but generating two uniforms per draw may not be _faster_ than the Inverse Transform method.

---

## Example 3 — Erlang$_n(\lambda)$

If $X_1, \dots, X_n \overset{iid}{\sim} \text{Exponential}(\lambda)$, then $$Y = \sum_{i=1}^n X_i \sim \text{Erlang}_n(\lambda).$$

Using the Inverse Transform for each Exponential, $X_i = \dfrac{-1}{\lambda}\ln(U_i)$, so:

$$ Y = \sum_{i=1}^n X_i = \sum_{i=1}^n \left[\frac{-1}{\lambda}\ln(U_i)\right] = \frac{-1}{\lambda}\ln\left(\prod_{i=1}^n U_i\right) $$

> [!success] Efficiency Because the sum of logs collapses into the **log of a product**, this only requires computing **one** natural log per realization of $Y$ (after multiplying the $n$ uniforms together) — making it quite efficient computationally, despite still needing $n$ uniform draws.

## Crude "Desert Island" Normal Generator (Convolution + CLT)

> [!abstract] Core Idea This is a **convolution-based approximate generator** for $\text{Nor}(0,1)$, built entirely from **Uniform(0,1)** draws and the **Central Limit Theorem (CLT)** — no inverse CDF, no special functions. Useful if you're stranded with nothing but a uniform RNG (hence "desert island").

---

### The Construction

Suppose $U_1, \dots, U_n \overset{iid}{\sim} \mathcal{U}(0,1)$, and let $$Y = \sum_{i=1}^n U_i.$$

Since each $U_i$ has mean $\tfrac12$ and variance $\tfrac{1}{12}$, for **large $n$** (see [[Expectation and Variance of a 01 Uniform RV]]) the CLT gives $$Y \approx \text{Normal}\left(\frac{n}{2}, \frac{n}{12}\right).$$

#### The $n=12$ trick

Choosing $n = 12$ makes the variance term collapse nicely ($n/12 = 1$), so — _pretending 12 counts as "large"_ —

$$Y - 6 = \sum_{i=1}^{12} U_i - 6 \approx \text{Nor}(0,1).$$

**Algorithm:**

1. Draw 12 i.i.d. $\mathcal{U}(0,1)$ values.
2. Sum them.
3. Subtract 6.
4. Treat the result as an approximate standard Normal draw.

---

### Why This Is _Crude_ (and Generally a Bad Idea)

> [!warning] Not recommended in practice The slide explicitly flags this as a method the author **wouldn't use**. Several real problems justify that:

- **$n=12$ isn't actually "large."** The CLT is an asymptotic result; invoking it at $n=12$ is a convenience hack (nice variance = 1), not a statistically justified sample size. The approximation quality is mediocre.
- **Bounded support vs. unbounded target.** $Y = \sum_{i=1}^{12} U_i \in [0, 12]$, so $Y - 6 \in [-6, 6]$. A true $\text{Nor}(0,1)$ has **infinite support** — this generator can **never produce values beyond $\pm 6$**, and in practice its tails are much thinner than a real Normals. Any application sensitive to tail behavior (risk analysis, rare-event simulation, extreme value studies) will be **systematically wrong**.
- **Inefficient.** It takes **12 uniform draws to produce one approximate Normal draw** — far more computation than needed compared to exact/fast alternatives.
- **Only approximate, never exact.** Unlike the Erlang or Binomial convolution examples, there's no exact distributional identity here — just an asymptotic approximation, so bias remains no matter how you tune it.

### Better Alternatives

- **Inverse Transform** using a good approximation to $\Phi^{-1}$ (e.g., Beasley-Springer-Moro algorithm).
- **Box–Muller transform**: turns 2 uniforms into 2 _exact_ independent standard Normals.
- **Ziggurat algorithm**: fast, exact, widely used in practice (e.g., in many software RNG libraries).

> [!tip] Takeaway This example is pedagogically useful for illustrating **convolution + CLT**, but it's a cautionary tale: an approximation that's easy to derive by hand is not necessarily good enough — especially in the tails — for real simulation work.

---
## Other Convolution-Related Tidbits

### 1. Geometric → Negative Binomial

If $X_1, \dots, X_n \overset{iid}{\sim} \text{Geometric}(p)$, then
$$\sum_{i=1}^n X_i \sim \text{NegBin}(n,p).$$

Makes sense intuitively: a Geometric counts trials/failures until the **1st** success; summing $n$ of them counts trials until the **$n$-th** success — which is exactly the Negative Binomial.

---

### 2. Squared Standard Normals → Chi-Square

If $Z_1, \dots, Z_n \overset{iid}{\sim} \text{Nor}(0,1)$, then
$$\sum_{i=1}^n Z_i^2 \sim \chi^2(n).$$

This is essentially the **definition** of the chi-square distribution with $n$ degrees of freedom — a sum of $n$ squared independent standard Normals.

---

### 3. Cauchy → Cauchy (the weird one)

If $X_1, \dots, X_n \overset{iid}{\sim} \text{Cauchy}$, then
$$\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i \sim \text{Cauchy}.$$

> [!warning] "Getting nowhere fast"
> Normally, averaging **reduces variance** and the sample mean concentrates around the true mean (Law of Large Numbers). The Cauchy distribution breaks this:
> - It has **no defined mean or variance** (heavy tails, undefined moments).
> - Averaging $n$ i.i.d. Cauchy variables gives you **another Cauchy** with the *same* distribution as a single draw — no concentration, no improvement, no matter how large $n$ gets.
> - Hence the joke: averaging more data gets you **nowhere fast** — the LLN and CLT simply don't apply here.


---
## Quick Reference: Convolution Distributions in Plain English

| Distribution                     | Used For (Plain English)                                                                                                                                                             | Parameters                                                                                                | Generation Rule (Convolution)                                                                                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bernoulli$(p)$**               | A single yes/no trial — e.g. one coin flip, one customer converting or not.                                                                                                          | $p$ = probability of "success" (1) on that one trial.                                                     | *Building block* — not itself a convolution; generated directly via Inverse Transform: $X = \mathbb{1}(U \le p)$.                                                       |
| **Binomial$(n,p)$**              | Counting how many successes happen out of a fixed number of independent yes/no trials — e.g. # of heads in 20 coin flips, # of defective parts in a batch.                           | $n$ = number of trials; $p$ = success probability per trial.                                              | Sum of $n$ i.i.d. Bernoulli$(p)$: $Y = \sum_{i=1}^n X_i$, where $X_i = \mathbb{1}(U_i \le p)$.                                                                          |
| **Uniform$(0,1)$**               | Modeling complete randomness with no preference across a range — e.g. a random number generator, a random point on an interval.                                                      | Bounds $0$ and $1$ (or general $a,b$) = the range all outcomes are equally likely within.                 | *Building block* — generated directly by the RNG; not itself built from convolution.                                                                                    |
| **Triangular$(0,1,2)$**          | A rough, easy-to-reason-about model when you only know a min, a most-likely value, and a max — common in risk/project estimation ("best case, most likely, worst case").             | Here: min $=0$, mode $=1$, max $=2$ (general Triangular has parameters $a$ = min, $c$ = mode, $b$ = max). | Sum of 2 i.i.d. $\mathcal{U}(0,1)$: $Y = U_1 + U_2$.                                                                                                                    |
| **Exponential$(\lambda)$**       | Time until the next random event happens — e.g. time between customer arrivals, time until a machine fails (memoryless: doesn't matter how long you've already waited).              | $\lambda$ = rate of events per unit time (higher $\lambda$ = events happen sooner/more often).            | *Building block* — generated directly via Inverse Transform: $X = \dfrac{-1}{\lambda}\ln(U)$; not itself built from convolution.                                        |
| **Erlang$_n(\lambda)$**          | Time until the **$n$-th** event happens in a Poisson-type process — e.g. time until the 5th customer arrives, time until the 3rd failure.                                            | $n$ = number of events being waited for; $\lambda$ = rate of each individual event.                       | Sum of $n$ i.i.d. Exp$(\lambda)$: $Y = \dfrac{-1}{\lambda}\ln\!\Big(\prod_{i=1}^n U_i\Big)$.                                                                            |
| **Geometric$(p)$**               | Number of trials needed to get the **first** success — e.g. number of sales calls until the first sale.                                                                              | $p$ = probability of success on each trial.                                                               | *Building block* — generated directly via Inverse Transform; not itself built from convolution.                                                                         |
| **NegBin$(n,p)$**                | Number of trials needed to get the **$n$-th** success — e.g. number of sales calls until the 5th sale.                                                                               | $n$ = number of successes being waited for; $p$ = success probability per trial.                          | Sum of $n$ i.i.d. Geometric$(p)$: $Y = \sum_{i=1}^n X_i$.                                                                                                               |
| **Normal / Nor$(\mu,\sigma^2)$** | The classic "bell curve" — models things that cluster around an average with symmetric random variation — e.g. heights, measurement error.                                           | $\mu$ = mean (center); $\sigma^2$ = variance (spread).                                                    | *Approximate only* (CLT "desert island" trick): $Y = \sum_{i=1}^{12} U_i - 6 \approx \text{Nor}(0,1)$ — not an exact method, generally not recommended.                 |
| **Chi-Square $\chi^2(n)$**       | Measures squared deviation / variability — heavily used in hypothesis testing (e.g. goodness-of-fit tests, variance tests).                                                          | $n$ = degrees of freedom (roughly, how many independent squared Normals were summed).                     | Sum of $n$ squared i.i.d. Nor$(0,1)$: $Y = \sum_{i=1}^n Z_i^2$.                                                                                                         |
| **Cauchy**                       | A "pathological" heavy-tailed distribution — used as a stress-test/counterexample in theory, and models phenomena with extreme, unpredictable outliers (e.g. ratios of two Normals). | Has a location parameter (center/peak) and scale parameter (spread), but **no defined mean or variance**. | Degenerate/curious case: averaging $n$ i.i.d. Cauchy gives $\bar{X} = \frac{1}{n}\sum X_i \sim \text{Cauchy}$ again — convolution doesn't concentrate it toward a mean. |

> [!note] Reading the table
> Rows marked "*building block*" are the simple distributions typically generated directly (often via Inverse Transform) and then **summed** to build the more complex distributions below them via convolution.

---

## Related

- [[Inverse Transform Theorem]]
- [[Random Variate Generation - Overview]]
#simulation #variance-reduction #statistics

# Control Variates

Related: [[Antithetic Random Numbers]] — the other major variance reduction technique (VRT) discussed prior to this. Control variates is the third and final VRT in this sequence, and is closely tied to **regression** ideas from introductory statistics.

---

## Setup

Goal: estimate the mean $\mu$ of a steady-state simulation output process $X_1, X_2, \dots, X_n$.

Suppose there is some other random variable $Y$ such that:

- $\mathrm{E}[Y]$ is **known**
- $\mathrm{Cov}(\bar X, Y) > 0$ (or more generally, nonzero)

$Y$ is called the **control variate** — a quantity correlated with what we're estimating, whose true mean we already know.

The usual estimator is $\bar X$. The **control-variate estimator** is instead:

$$ C = \bar X - \beta(Y - \mathrm{E}[Y]), \quad \text{for some constant } \beta. $$

**Intuition:** if $Y$ happens to come out above its known mean in a given run, that's a signal $\bar X$ is probably also running high (since they're correlated) — so we correct $\bar X$ downward, and vice versa. This "washes out" some of the noise.

---

## Why it works

**Unbiasedness:**

$$ \mathrm{E}[C] = \mathrm{E}[\bar X] - \beta(\mathrm{E}[Y] - \mathrm{E}[Y]) = \mathrm{E}[\bar X] = \mu $$

So $C$ is unbiased for $\mu$ regardless of the choice of $\beta$ — nice, since it means we're free to choose $\beta$ purely to minimize variance.

**Variance:**

$$ \mathrm{Var}(C) = \mathrm{Var}(\bar X) + \beta^2 \mathrm{Var}(Y) - 2\beta,\mathrm{Cov}(\bar X, Y) $$

This is a quadratic in $\beta$, so we can minimize it directly. Differentiating w.r.t. $\beta$ and setting to zero gives the optimal choice:

$$ \beta^* = \frac{\mathrm{Cov}(\bar X, Y)}{\mathrm{Var}(Y)} $$

> [!note] This is exactly the OLS regression slope coefficient — hence the "closely related to regression" framing. $\beta^*$ is the same formula you'd get regressing $\bar X$ on $Y$.

Plugging $\beta^*$ back in:

$$ \mathrm{Var}(C) = \mathrm{Var}(\bar X) - \frac{\mathrm{Cov}^2(\bar X, Y)}{\mathrm{Var}(Y)} ;<; \mathrm{Var}(\bar X) \qquad \blacksquare $$

**Key result:** as long as $\mathrm{Cov}(\bar X, Y) \neq 0$, the control-variate estimator strictly reduces variance compared to plain $\bar X$. The stronger the correlation between $X$ and $Y$, the bigger the variance reduction.

---

## Examples

- **Weight estimation:** estimate a population's mean weight $\mu$ using observed weights $X_1, X_2, \dots$, with corresponding heights $Y_1, Y_2, \dots$ as the control (assuming $\mathrm{E}[Y]$, e.g. mean height, is known). Weight and height are correlated, so height data helps "correct" the weight estimate.
- **American option pricing:** estimating the price of an American stock option is hard, but the price of the corresponding **European** option is easy (often known in closed form). Use the European option price as a control variate to reduce variance when estimating the American option price via simulation.

---

## Practical note

Many simulation texts give guidance on how to design and run simulations of _competing systems_ so that you can jointly exploit:

- **CRN** (Common Random Numbers)
- **ARN** (Antithetic Random Numbers)
- **Control variates**

...together, rather than using just one VRT in isolation.

---

## Summary

|Concept|Formula|
|---|---|
|Control-variate estimator|$C = \bar X - \beta(Y - \mathrm{E}[Y])$|
|Unbiasedness|$\mathrm{E}[C] = \mu$ for any $\beta$|
|Variance|$\mathrm{Var}(C) = \mathrm{Var}(\bar X) + \beta^2\mathrm{Var}(Y) - 2\beta,\mathrm{Cov}(\bar X,Y)$|
|Optimal $\beta$|$\beta^* = \dfrac{\mathrm{Cov}(\bar X, Y)}{\mathrm{Var}(Y)}$|
|Resulting variance|$\mathrm{Var}(C) = \mathrm{Var}(\bar X) - \dfrac{\mathrm{Cov}^2(\bar X,Y)}{\mathrm{Var}(Y)} < \mathrm{Var}(\bar X)$|

**Requirement:** need a variable $Y$ with known $\mathrm{E}[Y]$ and nonzero correlation with $\bar X$.
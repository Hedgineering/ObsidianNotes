# Time Series: ARMA and Autoregressive Pareto (ARP)

## 1. Moving Average Process — MA(1)

A first-order moving average process is

$$ Y_i = \epsilon_i + \theta \epsilon_{i-1} $$

where:

- $\theta$ is a constant
- $\epsilon_i \overset{iid}{\sim} N(0,1)$ are independent random shocks

### Intuition

Each observation depends on:

1. the **current random shock** $\epsilon_i$
2. the **previous random shock** $\epsilon_{i-1}$

Thus, adjacent observations are correlated because they share a common shock:

$$ Y_i = \epsilon_i + \theta\epsilon_{i-1} $$

$$ Y_{i+1} = \epsilon_{i+1} + \theta\epsilon_i $$

Both contain $\epsilon_i$.

However,

$$ Y_{i+2} = \epsilon_{i+2}+\theta\epsilon_{i+1} $$

shares no random shocks with $Y_i$, so they are independent and therefore uncorrelated.

### Variance

Since the errors are independent with variance $1$:

$$ \operatorname{Var}(Y_i) = \operatorname{Var}(\epsilon_i+\theta\epsilon_{i-1}) = \operatorname{Var}(\epsilon_i) +\theta^2\operatorname{Var}(\epsilon_{i-1}) = 1+\theta^2 $$

### Covariance Function

At lag $1$:

$$ \operatorname{Cov}(Y_i,Y_{i+1}) = \operatorname{Cov}(\epsilon_i+\theta\epsilon_{i-1}, \epsilon_{i+1}+\theta\epsilon_i) = \theta\operatorname{Var}(\epsilon_i) = \theta $$

For larger lags:

$$ \operatorname{Cov}(Y_i,Y_{i+k})=0, \qquad k\geq2 $$

Therefore,

$$ \gamma(k) = \begin{cases} 1+\theta^2, & k=0 \ \theta, & |k|=1 \ 0, & |k|\geq2 \end{cases} $$

> [!tip] Key Intuition An MA process has **finite memory in its shocks**. For MA(1), a shock $\epsilon_i$ affects only $Y_i$ and $Y_{i+1}$. After that, its effect disappears completely.

### Simulation Recipe

1. Generate $\epsilon_0\sim N(0,1)$.
2. Generate $\epsilon_1\sim N(0,1)$ and calculate $Y_1=\epsilon_1+\theta\epsilon_0$.
3. Generate $\epsilon_2$ and calculate $Y_2=\epsilon_2+\theta\epsilon_1$.
4. Continue recursively.

---

## 2. Autoregressive Process — AR(1)

A first-order autoregressive process is

$$ Y_i=\phi Y_{i-1}+\epsilon_i, \qquad -1<\phi<1 $$

In the parameterization used here,

$$ Y_0\sim N(0,1) \qquad \text{and} \qquad \epsilon_i\overset{iid}{\sim}N(0,1-\phi^2) $$

This choice makes $\operatorname{Var}(Y_i)=1$.

### Intuition

Unlike MA(1), AR(1) directly depends on the **previous observation**:

$$ Y_i \leftarrow Y_{i-1} \leftarrow Y_{i-2} \leftarrow Y_{i-3} \leftarrow\cdots $$

Expanding recursively:

$$ Y_i =\phi Y_{i-1}+\epsilon_i =\phi(\phi Y_{i-2}+\epsilon_{i-1})+\epsilon_i =\phi^2Y_{i-2} +\phi\epsilon_{i-1} +\epsilon_i $$

Continuing,

$$ Y_i = \epsilon_i +\phi\epsilon_{i-1} +\phi^2\epsilon_{i-2} +\phi^3\epsilon_{i-3} +\cdots $$

Thus, old shocks can affect the process indefinitely, but their influence decays geometrically.

### Covariance Function

With $\operatorname{Var}(Y_i)=1$:

$$ \operatorname{Cov}(Y_i,Y_{i+k}) = \phi^{|k|} $$

Therefore: $\gamma(0)=1$, $\gamma(1)=\phi$, $\gamma(2)=\phi^2$, $\gamma(3)=\phi^3$, and so on.

### Effect of $\phi$

- $\phi\approx1$: strong positive persistence
- $\phi\approx0$: observations are nearly independent
- $\phi<0$: observations tend to alternate direction
- $|\phi|\geq1$: the standard stationary AR(1) process is no longer stable

### Why Is the Innovation Variance $1-\phi^2$?

We want $\operatorname{Var}(Y_i)=1$. Since $Y_i=\phi Y_{i-1}+\epsilon_i$, and $Y_{i-1}$ and $\epsilon_i$ are independent:

$$ \operatorname{Var}(Y_i) = \phi^2\operatorname{Var}(Y_{i-1}) + \operatorname{Var}(\epsilon_i) $$

Setting both $Y_i$ and $Y_{i-1}$ to variance $1$:

$$ 1=\phi^2+\operatorname{Var}(\epsilon_i) $$

so

$$ \operatorname{Var}(\epsilon_i)=1-\phi^2 $$

Equivalently, if $Z_i\sim N(0,1)$,

$$ \epsilon_i=\sqrt{1-\phi^2}, Z_i $$

---

## 3. MA vs. AR: The Main Intuition

|Property|MA(1)|AR(1)|
|---|---|---|
|Definition|$Y_i=\epsilon_i+\theta\epsilon_{i-1}$|$Y_i=\phi Y_{i-1}+\epsilon_i$|
|Depends directly on|Current and past shocks|Past observations|
|Memory|Finite|Infinite but decaying|
|Covariance|Cuts off after lag 1|Decays geometrically|
|Effect of one shock|Disappears after one lag|Persists indefinitely|

> [!important] Central distinction **MA models propagate shocks for a fixed number of periods. AR models propagate the state itself, causing past shocks to indirectly persist into the future.**

---

## 4. ARMA(p,q) Process

An ARMA process combines:

- an **autoregressive (AR)** component
- a **moving average (MA)** component

The general ARMA(p,q) process is

$$ Y_i = \sum_{j=1}^{p}\phi_jY_{i-j} + \epsilon_i + \sum_{j=1}^{q}\theta_j\epsilon_{i-j} $$

or explicitly,

$$ Y_i = \phi_1Y_{i-1} +\cdots+ \phi_pY_{i-p} + \epsilon_i + \theta_1\epsilon_{i-1} +\cdots+ \theta_q\epsilon_{i-q} $$

### Meaning of p and q

- $p$: number of previous **observations** used
- $q$: number of previous **shocks/errors** used

For example, an ARMA(2,1) process is

$$ Y_i = \phi_1Y_{i-1} + \phi_2Y_{i-2} + \epsilon_i + \theta_1\epsilon_{i-1} $$

### Intuition

ARMA combines two types of memory — memory of previous states, plus memory of shocks:

$$ Y_i = \underbrace{\sum_{j=1}^{p}\phi_jY_{i-j}}_{\text{memory of previous states}} + \underbrace{\epsilon_i + \sum_{j=1}^{q}\theta_j\epsilon_{i-j}}_{\text{memory of shocks}} $$

The coefficients must satisfy appropriate stability conditions so that the process does not explode.

---

## 5. Exponential Autoregressive Process — EAR(1)

The EAR(1) process is useful for generating **correlated exponential random variables**.

Let

$$ Y_0\sim\operatorname{Exp}(1) \qquad \text{and} \qquad \epsilon_i\overset{iid}{\sim}\operatorname{Exp}(1) $$

Then

$$ Y_i = \begin{cases} \phi Y_{i-1}, & \text{with probability } \phi \\ \phi Y_{i-1}+\epsilon_i, & \text{with probability } 1-\phi \end{cases} $$

where $0\leq\phi<1$.

### Intuition

Every new observation retains the fraction $\phi Y_{i-1}$ of the previous observation. Sometimes a new exponential innovation $\epsilon_i$ is added.

The construction is specifically designed so that:

1. each $Y_i$ still has an $\operatorname{Exp}(1)$ marginal distribution
2. observations are serially correlated

Its covariance structure is

$$ \operatorname{Cov}(Y_i,Y_{i+k}) = \phi^{|k|} $$

Thus, it has the same geometric covariance decay as the standardized AR(1) — $1,\phi,\phi^2,\phi^3,\ldots$ — but its marginal distribution is exponential rather than normal.

---

## 6. Autoregressive Pareto Process — ARP

The goal is to generate a sequence of **correlated Pareto random variables**.

A Pareto random variable $X$ with parameters $\lambda>0$ and $\beta>0$ has CDF

$$ F_X(x) = 1-\left(\frac{\lambda}{x}\right)^\beta, \qquad x\geq\lambda $$

The Pareto distribution is **heavy-tailed**, meaning extreme values occur with greater probability than under distributions such as the normal.

### 6.1 The Probability Integral Transform

The ARP construction relies on the fundamental result:

> [!important] Probability Integral Transform If $Y$ has continuous CDF $F_Y$, then $U=F_Y(Y)\sim\operatorname{Uniform}(0,1)$.
> 
> Conversely, if $U\sim\operatorname{Uniform}(0,1)$, then $X=F_X^{-1}(U)$ has CDF $F_X$.

Therefore, we can transform:

$$ \text{Correlated Normal} \rightarrow \text{Correlated Uniform} \rightarrow \text{Correlated Pareto} $$

---

## 7. Constructing Correlated Pareto Random Variables

Start with a stationary Gaussian AR(1):

$$ Y_i = \phi Y_{i-1} + \sqrt{1-\phi^2},Z_i, \qquad Z_i\overset{iid}{\sim}N(0,1) $$

Because each $Y_i\sim N(0,1)$, apply the standard normal CDF $\Phi$:

$$ U_i=\Phi(Y_i) $$

Then each marginally satisfies $U_i\sim\operatorname{Uniform}(0,1)$.

Because the original $Y_i$ values are dependent, the transformed $U_i$ values remain dependent.

> [!note] A deterministic monotonic transformation changes the marginal distributions but does not destroy the underlying dependence between observations.

### 7.1 Transform Uniforms into Pareto Variables

The Pareto CDF is $u = 1-\left(\dfrac{\lambda}{x}\right)^\beta$. Solve for $x$:

$$ 1-u = \left(\frac{\lambda}{x}\right)^\beta \quad\Longrightarrow\quad (1-u)^{1/\beta} = \frac{\lambda}{x} $$

so

$$ F_X^{-1}(u) = \frac{\lambda}{(1-u)^{1/\beta}} $$

Therefore,

$$ X_i = F_X^{-1}(\Phi(Y_i)) = \frac{\lambda}{\left[1-\Phi(Y_i)\right]^{1/\beta}} $$

produces a sequence of correlated Pareto random variables.

---

## 8. ARP Transformation Pipeline

The complete idea is:

$$ Y_i \sim \text{correlated } N(0,1) \quad\xrightarrow{\ U_i=\Phi(Y_i)\ }\quad U_i \sim \text{correlated Uniform}(0,1) \quad\xrightarrow{\ X_i=F_{\text{Pareto}}^{-1}(U_i)\ }\quad X_i \sim \text{correlated Pareto} $$

The marginal distributions change:

$$ N(0,1) \rightarrow U(0,1) \rightarrow \operatorname{Pareto}(\lambda,\beta) $$

while dependence is carried through the transformations.

### Important Caveat About Correlation

The transformations preserve **dependence**, but generally not the exact **Pearson correlation coefficient**.

Therefore, if $\operatorname{Corr}(Y_i,Y_{i+k})=\phi^{|k|}$, it does **not** generally follow that $\operatorname{Corr}(X_i,X_{i+k})=\phi^{|k|}$.

> [!warning] The nonlinear transformation changes Pearson correlation. What is preserved is the underlying dependence structure induced by the Gaussian process — this construction is closely related to a **Gaussian copula**.

---

## 9. Big Picture

### ARMA

ARMA models dependence directly through past states, past shocks, and a new shock:

$$ \text{ARMA} = \text{past states} + \text{past shocks} + \text{new shock} $$

### ARP

ARP addresses a different problem:

> [!question] How can we generate a **time-dependent sequence with Pareto marginal distributions**?

The strategy is:

1. generate a correlated process with convenient normal marginals
2. map each normal observation to a uniform using its CDF
3. map each uniform observation to the desired Pareto distribution using the inverse CDF

$$ \text{Correlated Normal} \xrightarrow{\Phi} \text{Correlated Uniform} \xrightarrow{F_{\text{Pareto}}^{-1}} \text{Correlated Pareto} $$

This general idea extends beyond the Pareto distribution:

$$ X_i=F_{\text{target}}^{-1}\left(\Phi(Y_i)\right) $$

can be used to construct dependent random variables with many desired marginal distributions.

---

## Python Implementation

```python
import math
import random
import numpy as np


def simulate_ma1(n, theta, rng=None):
    """
    Simulate n observations of an MA(1) process:
        Y_i = eps_i + theta * eps_(i-1)
    """
    rng = rng or np.random.default_rng()
    eps = rng.standard_normal(n + 1)  # eps_0, eps_1, ..., eps_n
    Y = eps[1:] + theta * eps[:-1]
    return Y


def simulate_ar1(n, phi, rng=None):
    """
    Simulate n observations of a stationary AR(1) process:
        Y_0 ~ N(0,1)
        Y_i = phi * Y_(i-1) + eps_i,  eps_i ~ N(0, 1 - phi^2)
    """
    rng = rng or np.random.default_rng()
    Y = np.zeros(n)
    Y[0] = rng.standard_normal()
    innovation_sd = math.sqrt(1 - phi ** 2)
    for i in range(1, n):
        Y[i] = phi * Y[i - 1] + innovation_sd * rng.standard_normal()
    return Y


def simulate_ear1(n, phi, rng=None):
    """
    Simulate n observations of an EAR(1) process (correlated Exp(1) marginals):
        Y_0 ~ Exp(1)
        Y_i = phi * Y_(i-1),                w.p. phi
            = phi * Y_(i-1) + eps_i,        w.p. 1 - phi,  eps_i ~ Exp(1)
    """
    rng = rng or np.random.default_rng()
    Y = np.zeros(n)
    Y[0] = rng.exponential(1.0)
    for i in range(1, n):
        if rng.random() < phi:
            Y[i] = phi * Y[i - 1]
        else:
            Y[i] = phi * Y[i - 1] + rng.exponential(1.0)
    return Y


def simulate_arp(n, phi, lam, beta, rng=None):
    """
    Simulate n observations of a correlated Pareto sequence (ARP)
    via the Gaussian-copula-style pipeline:
        Y_i (correlated N(0,1))  -->  U_i = Phi(Y_i)  -->  X_i = F_Pareto^{-1}(U_i)

    lam, beta: Pareto parameters (F(x) = 1 - (lam/x)^beta, x >= lam)
    """
    from scipy.stats import norm  # only needed for Phi (standard normal CDF)

    rng = rng or np.random.default_rng()
    Y = simulate_ar1(n, phi, rng=rng)          # correlated standard normals
    U = norm.cdf(Y)                            # correlated uniforms
    X = lam / (1 - U) ** (1 / beta)            # correlated Pareto
    return X


if __name__ == "__main__":
    rng = np.random.default_rng(42)

    ma_series = simulate_ma1(10, theta=0.6, rng=rng)
    print("MA(1) sample:", np.round(ma_series, 3))

    ar_series = simulate_ar1(10, phi=0.8, rng=rng)
    print("AR(1) sample:", np.round(ar_series, 3))

    ear_series = simulate_ear1(10, phi=0.5, rng=rng)
    print("EAR(1) sample:", np.round(ear_series, 3))

    arp_series = simulate_arp(10, phi=0.8, lam=1.0, beta=3.0, rng=rng)
    print("ARP sample:  ", np.round(arp_series, 3))

    # Sanity check: empirical lag-1 correlation of a long AR(1) run should be ~ phi
    long_ar = simulate_ar1(200_000, phi=0.8, rng=rng)
    empirical_corr = np.corrcoef(long_ar[:-1], long_ar[1:])[0, 1]
    print(f"\nEmpirical AR(1) lag-1 correlation: {empirical_corr:.4f} (theoretical: 0.8)")
```

## Related

- [[Order Statistics and Other Stuff]]
- [[Multivariate Normal Random Variable Generation]]
- [[Baby Stochastic Process (Markov Chains and Poisson Arrivals)]]
# Brownian Motion

Discovered by Brown; analyzed rigorously by Einstein; mathematical rigor established by Wiener (also called the _Wiener process_).

Widely used in everything from financial analysis to queueing theory to statistics to other OR/IE application areas.

## Definition

The stochastic process ${\mathcal{W}(t), t \ge 0}$ is **standard Brownian motion** if:

1. $\mathcal{W}(0) = 0$
2. $\mathcal{W}(t) \sim \text{Nor}(0,t)$
3. ${\mathcal{W}(t), t \ge 0}$ has stationary and independent increments

### Increments

**Increments:** anything of the form $\mathcal{W}(b) - \mathcal{W}(a)$.

**Stationary increments:** the distribution of $\mathcal{W}(t+h) - \mathcal{W}(t)$ only depends on $h$ (not on $t$ itself).

**Independent increments:** if $a < b < c < d$, then $\mathcal{W}(d) - \mathcal{W}(c)$ is independent of $\mathcal{W}(b) - \mathcal{W}(a)$ — non-overlapping windows of the path are independent of each other.

---

## How Do You Get BM? (Donsker's Theorem)

Let $Y_1, Y_2, \ldots$ be any sequence of i.i.d. RVs with mean zero and variance $1$. (To some extent, the $Y_i$'s don't even have to be independent!)

> [!important] Donsker's Central Limit Theorem $$ \frac{1}{\sqrt{n}} \sum_{i=1}^{\lfloor nt \rfloor} Y_i \xrightarrow{d} \mathcal{W}(t) \qquad \text{as } n \to \infty $$ where $\xrightarrow{d}$ denotes convergence in distribution as $n$ gets big, and $\lfloor \cdot \rfloor$ is the floor function, e.g., $\lfloor 3.7 \rfloor = 3$.

> [!tip] The regular CLT is a very special case of this Big Boy! (Just fix $t=1$ and you recover the ordinary Central Limit Theorem for $\frac{1}{\sqrt{n}}\sum_{i=1}^n Y_i$.)

### Constructing BM in Practice

One choice that works well is to take $Y_i = \pm1$, each with probability $1/2$. Take $n$ at least 100, $t = 1/n, 2/n, \ldots, n/n$, and calculate $\mathcal{W}(1/n), \mathcal{W}(2/n), \ldots, \mathcal{W}(n/n)$.

Another choice is simply to take $Y_i \sim \text{Nor}(0,1)$.

**Recursive construction:** pick some "large" value of $n$ and start with $\mathcal{W}(0) = 0$. Then:

$$ \mathcal{W}!\left(\frac{i}{n}\right) = \mathcal{W}!\left(\frac{i-1}{n}\right) + \frac{Y_i}{\sqrt{n}} $$

> [!note] Why this works This is exactly Donsker's theorem applied one step at a time — each new increment is a properly-scaled i.i.d. shock $Y_i/\sqrt{n}$ added to the running path, so that as $n$ grows large the resulting path converges to genuine Brownian motion.

---

## Miscellaneous Properties of Brownian Motion

- BM is continuous everywhere, but has **no derivatives**! (Deep!)
- $\operatorname{Cov}(\mathcal{W}(s), \mathcal{W}(t)) = \min(s,t)$
- The area under $\mathcal{W}(t)$ is normal: $\displaystyle\int_0^1 \mathcal{W}(t),dt \sim \text{Nor}!\left(0, \tfrac{1}{3}\right)$
- A **Brownian bridge**, $\mathcal{B}(t)$, is conditioned BM such that $\mathcal{W}(0) = \mathcal{W}(1) = 0$.
- $\operatorname{Cov}(\mathcal{B}(s), \mathcal{B}(t)) = \min(s,t) - st$
- $\displaystyle\int_0^1 \mathcal{B}(t),dt \sim \text{Nor}!\left(0, \tfrac{1}{12}\right)$

---

# Geometric Brownian Motion (GBM)

The process
$$
S(t) = S(0)\exp\left\{\left(\mu - \frac{\sigma^2}{2}\right)t + \sigma\mathcal{W}(t)\right\}, \qquad t \ge 0
$$
is often used to model stock prices, where $\mu$ is related to the "drift" of the stock price, $\sigma$ is its volatility, and $S(0)$ is the initial price.

## Option Pricing with GBM

We can use GBM to estimate option prices. For example, a **European call option** permits its owner, who pays an up-front fee for the privilege, to purchase the stock at a pre-agreed strike price $k$, at a pre-determined expiry date $T$. Its "value" is

$$ e^{-rT}, E!\left[(S(T) - k)^+\right] $$

where $x^+ = \max{0, x}$ and $\mu \leftarrow r$, the "risk-free" interest rate.

### Estimating the Expected Value via Simulation

To estimate this expected value, we can run multiple simulation replications of $\mathcal{W}(T)$ and $(S(T)-k)^+$, and then take the sample average of the $e^{-rT}(S(T)-k)^+$ values.

> [!question] Exercise Estimate the value of a stock option. Pick your favorite values of $r, \sigma, T, k$, and off you go!

There are lots of ways to actually do this:

- Directly simulate the BM path many times (as described above), then compute $S(T)$ from it.
- Simulate the distribution of $S(T)$ directly — it's **lognormal**, so you can sample it in one shot without ever building the whole path.
- Look up the exact **Black–Scholes** closed-form answer (see below), and skip simulation entirely.

---

## How to Win a Nobel Prize

Let $\phi(\cdot)$ and $\Phi(\cdot)$ denote the usual $\text{Nor}(0,1)$ p.d.f. and c.d.f. Moreover, define the horrible-looking

$$ b \equiv \frac{rT - \dfrac{\sigma^2 T}{2} - \ln(k/S(0))}{\sigma\sqrt{T}} $$

### The Black–Scholes European Call Option Value
$$
e^{-rT}E[S(T) - k]^+
$$
 
$$
= e^{-rT}\, E\left[S(0)\exp\left\{\left(r-\frac{\sigma^2}{2}\right)T + \sigma\mathcal{W}(T)\right\} - k\right]^+
$$
 
$$
= e^{-rT} \int_{-\infty}^{\infty} \left[S(0)\exp\left\{\left(r-\frac{\sigma^2}{2}\right)T + \sigma\sqrt{T}z\right\} - k\right]^+ \phi(z)\,dz
$$
 
$$
= S(0)\,\Phi(b + \sigma\sqrt{T}) - ke^{-rT}\Phi(b) \qquad \text{(after lots of algebra)} \qquad \blacksquare
$$
 
> [!success] Now get your tickets to Norway or Sweden or wherever they give out the Nobel Prize...

---

## Python Implementation

```python
import math
import numpy as np
from scipy.stats import norm


def simulate_bm_path(T, n, rng=None):
    """
    Simulate a single Brownian motion path on [0, T] using n steps,
    via the recursive construction:
        W(i/n) = W((i-1)/n) + Y_i / sqrt(n)
    (rescaled here to a general horizon T rather than [0,1]).

    Returns:
        t_grid: array of time points, length n+1
        W: array of W(t) values at those times, length n+1 (W[0] = 0)
    """
    rng = rng or np.random.default_rng()
    dt = T / n
    increments = rng.standard_normal(n) * math.sqrt(dt)
    W = np.concatenate(([0.0], np.cumsum(increments)))
    t_grid = np.linspace(0, T, n + 1)
    return t_grid, W


def simulate_gbm_terminal(S0, mu, sigma, T, size=1, rng=None):
    """
    Simulate S(T) directly from its (lognormal) terminal distribution,
    without needing to build the whole BM path:
        S(T) = S0 * exp((mu - sigma^2/2) * T + sigma * W(T)),
        W(T) ~ Nor(0, T)
    """
    rng = rng or np.random.default_rng()
    W_T = rng.standard_normal(size) * math.sqrt(T)
    S_T = S0 * np.exp((mu - sigma ** 2 / 2) * T + sigma * W_T)
    return S_T


def black_scholes_call(S0, k, r, sigma, T):
    """
    Exact Black-Scholes value of a European call option.
    """
    b = (r * T - (sigma ** 2 * T) / 2 - math.log(k / S0)) / (sigma * math.sqrt(T))
    return S0 * norm.cdf(b + sigma * math.sqrt(T)) - k * math.exp(-r * T) * norm.cdf(b)


def monte_carlo_call_price(S0, k, r, sigma, T, n_reps=200_000, rng=None):
    """
    Estimate the European call option price via Monte Carlo simulation
    of the terminal stock price S(T), using r as the risk-free drift.
    """
    rng = rng or np.random.default_rng()
    S_T = simulate_gbm_terminal(S0, mu=r, sigma=sigma, T=T, size=n_reps, rng=rng)
    payoffs = np.maximum(S_T - k, 0.0)
    discounted_payoffs = math.exp(-r * T) * payoffs
    price_estimate = discounted_payoffs.mean()
    std_error = discounted_payoffs.std(ddof=1) / math.sqrt(n_reps)
    return price_estimate, std_error


if __name__ == "__main__":
    rng = np.random.default_rng(42)

    # Example BM path
    t_grid, W = simulate_bm_path(T=1.0, n=1000, rng=rng)
    print("Sample BM path, W(1):", round(W[-1], 4))

    # Example: European call option pricing
    S0, k, r, sigma, T = 100.0, 105.0, 0.03, 0.25, 1.0

    mc_price, mc_se = monte_carlo_call_price(S0, k, r, sigma, T, n_reps=500_000, rng=rng)
    bs_price = black_scholes_call(S0, k, r, sigma, T)

    print(f"\nMonte Carlo price:  {mc_price:.4f}  (std error: {mc_se:.4f})")
    print(f"Black-Scholes price: {bs_price:.4f}")
```

> [!tip] Sanity check The Monte Carlo estimate should land within roughly 1-2 standard errors of the exact Black-Scholes price — this is a great way to confirm both the simulation code and your understanding of the closed-form formula are correct.

## Related

- [[Time Series ARMA and ARP]]
- [[Multivariate Normal Random Variable Generation]]
- [[Random Variable Generation for Queueing Processes - MM1 and Lindley Recursion]]
# Non-Homogeneous Poisson Processes (NHPPs)

## Nonstationary Arrivals

Same assumptions as a regular Poisson process, except the arrival rate $\lambda$ isn't a constant — so **stationary increments** doesn't apply.

Let:

$$ \lambda(t) = \text{rate (intensity) function at time } t $$

$$ N(t) = \text{number of arrivals during } [0,t] $$

Then:

$$ N(s+t) - N(s) \sim \text{Poisson}\left(\int_s^{s+t} \lambda(u),du\right) $$

> [!note] Intuition The number of arrivals over any window is Poisson, but the _mean_ of that Poisson isn't $\lambda \times (\text{length of window})$ like in the ordinary case — it's the **integral of the rate function** over the window, since the rate itself is changing over time.

## Example: Waffle House Arrivals

Suppose the arrival pattern to the Waffle House over a certain time period is a NHPP with $\lambda(t) = t^2$. Find the probability that there will be exactly 4 arrivals between times $t=1$ and $t=2$.

First, the number of arrivals in that time interval is:

$$ N(2) - N(1) \sim \text{Pois}\left(\int_1^2 t^2,dt\right) \sim \text{Pois}(7/3) $$

Thus:

$$ P(N(2) - N(1) = 4) = \frac{e^{-7/3}(7/3)^4}{4!} = 0.120 \qquad \blacksquare $$

---

## Generating NHPP Arrivals

### Incorrect NHPP Algorithm ⚠️

A tempting but **wrong** approach: treat the process like a regular Poisson process, but plug in the _current_ rate $\lambda(T_i)$ at each step.

```
T_0 ← 0; i ← 0
Repeat
    Generate U from U(0,1)
    T_(i+1) ← T_i − (1/λ(T_i)) ln(U)
    i ← i + 1
```

> [!danger] Don't use this algorithm! It can **"skip" intervals** with large $\lambda(t)$. Because the rate used for the interarrival draw is frozen at $\lambda(T_i)$ (the rate at the _previous_ arrival), the algorithm has no way to "notice" a spike in the rate function that occurs between $T_i$ and the next arrival — it sails right over it, understating the true intensity in that region.

### The Thinning Algorithm ✅

The correct approach: the **Thinning Algorithm**.

1. Assumes that $\lambda^* = \max_t \lambda(t)$ is finite.
2. Generates **potential** arrivals at the max rate $\lambda^*$ (an ordinary, easy-to-generate homogeneous Poisson process).
3. Accepts a potential arrival at time $t$ with probability $\lambda(t)/\lambda^*$ — i.e., "thins out" the potential arrivals, keeping more of them where $\lambda(t)$ is close to the peak $\lambda^*$, and discarding more where $\lambda(t)$ is much smaller than $\lambda^*$.

> [!info] Visual intuition Draw the true rate curve $\lambda(t)$ and a flat line at its max height $\lambda^*$. Potential arrival times $t_1, t_2, \ldots$ are generated as if arrivals occurred at the constant high rate $\lambda^*$. Each potential arrival is then kept with probability equal to how close the true curve $\lambda(t)$ is to the ceiling $\lambda^*$ at that point — points where the curve dips low get rejected more often.

### Thinning Algorithm (Pseudocode)

```
T_0 ← 0; i ← 0
Repeat
    t ← T_i
    Repeat
        Generate U, V from U(0,1)
        t ← t − (1/λ*) ln(U)      ← each t update represents a potential arrival (at rate λ*)
    until V ≤ λ(t)/λ*             ← but we only keep the potential arrival w.p. λ(t)/λ*
    i ← i + 1
    T_i ← t                       ← these T_i's are the arrivals we end up keeping
```

---

## Python Implementation

```python
import math
import random

def generate_nhpp_thinning(lam_func, lam_star, t_end, rng=random):
    """
    Generate arrival times of a non-homogeneous Poisson process
    on [0, t_end] using the Thinning Algorithm.

    lam_func: function lambda(t) giving the rate at time t
    lam_star: upper bound on lambda(t), i.e. max_t lambda(t)
    t_end: stop generating once the arrival time exceeds this
    rng: random module or Random() instance (for reproducibility)

    Returns: list of accepted arrival times (floats), all <= t_end.
    """
    arrivals = []
    t = 0.0

    while True:
        # Generate a potential arrival at the constant rate lam_star
        u = rng.random()
        t = t - (1.0 / lam_star) * math.log(u)

        if t > t_end:
            break

        # Accept the potential arrival with probability lambda(t) / lam_star
        v = rng.random()
        if v <= lam_func(t) / lam_star:
            arrivals.append(t)

    return arrivals


if __name__ == "__main__":
    # Example: lambda(t) = t^2 on [0, 2], so lam_star = max_t lambda(t) = 4
    lam_func = lambda t: t ** 2
    lam_star = 4.0  # = lambda(2), the max on [0, 2]

    random.seed(42)
    arrival_times = generate_nhpp_thinning(lam_func, lam_star, t_end=2.0)
    print(f"Number of arrivals on [0,2]: {len(arrival_times)}")
    print("Arrival times:", [round(t, 3) for t in arrival_times])

    # Sanity check: simulate many replications and compare average
    # count in [1, 2] against the theoretical Pois(7/3) mean.
    random.seed(0)
    counts = []
    for _ in range(20_000):
        times = generate_nhpp_thinning(lam_func, lam_star, t_end=2.0)
        count_in_1_2 = sum(1 for t in times if t > 1.0)
        counts.append(count_in_1_2)

    empirical_mean = sum(counts) / len(counts)
    theoretical_mean = 7 / 3  # integral of t^2 from 1 to 2
    print(f"\nEmpirical mean count in [1,2]: {empirical_mean:.4f}")
    print(f"Theoretical mean (7/3):        {theoretical_mean:.4f}")
```

> [!tip] Choosing $\lambda^*$ In practice, $\lambda^*$ should be the true maximum of $\lambda(t)$ over the interval you care about — too small an upper bound breaks the algorithm's correctness (you can't accept with probability greater than 1), while too large a bound just wastes potential-arrival draws that get rejected.

## Related

- [[Baby Stochastic Process (Markov Chains and Poisson Arrivals)]]
- [[Order Statistics and Other Stuff]]
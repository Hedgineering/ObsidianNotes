# Queueing Processes: M/M/1 and the Lindley Recursion

## M/M/1 Queue

Consider a single-server queue with customers arriving according to a $\text{Poisson}(\lambda)$ process, standing in line with a **FIFO** discipline, and then getting served in an $\text{Exp}(\mu)$ amount of time.

> [!info] Notation "M/M/1" The three slots describe: **M**arkovian (Poisson) arrivals, **M**arkovian (exponential) service times, and **1** server. This is the classic simplest queueing model.

### Notation

Let:

- $I_{i+1}$ = the interarrival time between the $i$th and $(i+1)$st customers
- $S_i$ = the $i$th customer's service time
- $W_i^Q$ = the $i$th customer's wait **before** service (queueing delay, not counting service itself)

---

## The Lindley Recursion

Lindley gives a very nice way to generate a series of waiting times for this simple example — and remarkably, **you don't even need the exponential assumptions** for this to hold. It works for _any_ single-server FIFO queue, regardless of the arrival or service distributions.

### Wait Before Service

$$ W_{i+1}^Q = \max{\{W_i^Q + S_i - I_{i+1}, 0}\} $$

> [!note] Intuition Think of it from customer $i+1$'s perspective. Customer $i$ arrived, waited $W_i^Q$ in queue, then took $S_i$ time to be served — so customer $i$ "clears the system" at a certain point. Customer $i+1$ arrives $I_{i+1}$ time after customer $i$ did. If customer $i+1$ arrives _before_ customer $i$'s total time in system ($W_i^Q + S_i$) is up, they must wait for the remainder: $W_i^Q + S_i - I_{i+1}$. If customer $i+1$ arrives _after_ that point, the server is already free, so they wait $0$ — hence the $\max{\cdot, 0}$.

### Total Time in System

Similarly, the total time in system is $W_i = W_i^Q + S_i$ (queueing delay plus the customer's own service time), and it satisfies its own clean recursion:

$$ W_{i+1} = \max\{{W_i - I_{i+1},\ 0\}} + S_{i+1} $$

> [!tip] Why this is so convenient for simulation Both recursions only require you to track **one running number** ($W_i^Q$ or $W_i$) plus fresh draws of $S_i$ and $I_{i+1}$ at each step — there's no need to explicitly simulate arrival and departure event lists, or maintain queue-length state machines. Just generate the interarrival and service times, and the waiting times fall out of the recursion directly.

---

## Python Implementation

```python
import math
import random


def simulate_mm1_waits(n, lam, mu, rng=None):
    """
    Simulate n customers through an M/M/1 queue using the Lindley recursion.

    lam: arrival rate (Poisson(lam) process -> Exp(lam) interarrival times)
    mu:  service rate (Exp(mu) service times)

    Returns:
        wait_Q: list of wait-before-service times, W^Q_1, ..., W^Q_n
        wait_total: list of total time-in-system, W_1, ..., W_n
    """
    rng = rng or random

    wait_Q = [0.0] * n       # W^Q_i
    wait_total = [0.0] * n   # W_i = W^Q_i + S_i

    # First customer arrives to an empty system: no wait before service.
    S = [-math.log(rng.random()) / mu for _ in range(n)]      # service times
    I = [-math.log(rng.random()) / lam for _ in range(n)]     # interarrival times (I_2, I_3, ... conceptually)

    wait_Q[0] = 0.0
    wait_total[0] = wait_Q[0] + S[0]

    for i in range(n - 1):
        # W^Q_(i+1) = max(W^Q_i + S_i - I_(i+1), 0)
        wait_Q[i + 1] = max(wait_Q[i] + S[i] - I[i + 1], 0.0)
        wait_total[i + 1] = wait_Q[i + 1] + S[i + 1]

    return wait_Q, wait_total


if __name__ == "__main__":
    rng = random.Random(42)
    lam, mu = 3.0, 5.0  # arrival rate 3/hr, service rate 5/hr -> utilization rho = lam/mu = 0.6

    n = 20
    wait_Q, wait_total = simulate_mm1_waits(n, lam, mu, rng=rng)

    print(f"{'Customer':>8} {'W^Q_i (queue wait)':>20} {'W_i (total time)':>18}")
    for i in range(n):
        print(f"{i+1:8d} {wait_Q[i]:20.4f} {wait_total[i]:18.4f}")

    # Sanity check: for M/M/1, theoretical mean wait in queue is
    # E[W^Q] = rho / (mu - lam) = rho^2 / (lam * (1 - rho))   [various equivalent forms]
    rho = lam / mu
    theoretical_mean_wait_Q = rho / (mu - lam)

    rng2 = random.Random(0)
    n_big = 200_000
    wait_Q_big, _ = simulate_mm1_waits(n_big, lam, mu, rng=rng2)
    # discard an initial "warm-up" portion since the queue starts empty
    warm_up = 5000
    empirical_mean_wait_Q = sum(wait_Q_big[warm_up:]) / (n_big - warm_up)

    print(f"\nTheoretical mean W^Q: {theoretical_mean_wait_Q:.4f}")
    print(f"Empirical mean W^Q:   {empirical_mean_wait_Q:.4f}")
```

> [!warning] Note on simulation output Because the queue starts **empty** (the first customer never waits), the early waiting times are systematically too small compared to steady state. When comparing simulated averages to steady-state theoretical formulas, discard an initial "warm-up" period, as done above.

## Related

- [[Baby Stochastic Process (Markov Chains and Poisson Arrivals)]]
- [[Order Statistics and Other Stuff]]
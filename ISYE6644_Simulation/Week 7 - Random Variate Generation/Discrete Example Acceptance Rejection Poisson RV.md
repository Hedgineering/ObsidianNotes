# A-R Method for a Discrete Distribution: Poisson Example

> [!abstract] Setup
> Generate a realization of $X \sim \text{Poisson}(\lambda)$, with p.m.f.
> $$P(X=n) = e^{-\lambda}\frac{\lambda^n}{n!}, \quad n = 0, 1, 2, \dots$$
> This uses a **variation of A-R** — instead of an envelope/majorizer in the usual continuous sense, we exploit the connection between the Poisson process and its interarrival times to derive a clean stopping rule.

---

## Deriving the Rule

**Key fact:** $X = n$ means we observe **exactly $n$ arrivals** from a Poisson$(\lambda)$ process in one time unit.

Let $A_i$ be the $i$th interarrival time of a Poisson$(\lambda)$ process (so $A_i \overset{iid}{\sim} \text{Exp}(\lambda)$).

$$
X = n \iff \text{exactly } n \text{ Pois}(\lambda) \text{ arrivals occur by } t=1
$$

This happens exactly when the $n$th arrival occurs by time 1, **and** the $(n+1)$st arrival occurs after time 1:

$$
X = n \iff \sum_{i=1}^n A_i \le 1 < \sum_{i=1}^{n+1} A_i
$$

Now substitute the Inverse-Transform generator for each Exponential interarrival time, $A_i = \dfrac{-1}{\lambda}\ln(U_i)$:

$$
X = n \iff \sum_{i=1}^n \left[\frac{-1}{\lambda}\ln(U_i)\right] \le 1 < \sum_{i=1}^{n+1} \left[\frac{-1}{\lambda}\ln(U_i)\right]
$$

Combine the sum of logs into a log of a product:

$$
X = n \iff \frac{-1}{\lambda}\ln\!\left(\prod_{i=1}^n U_i\right) \le 1 < \frac{-1}{\lambda}\ln\!\left(\prod_{i=1}^{n+1} U_i\right)
$$

Multiply through by $-\lambda$ (flipping inequalities) and exponentiate:

$$
X = n \iff \prod_{i=1}^n U_i \ge e^{-\lambda} > \prod_{i=1}^{n+1} U_i \tag{5}
$$

> [!success] Takeaway
> To generate $X$, keep multiplying fresh $\mathcal{U}(0,1)$ draws together until the running product first drops **below** $e^{-\lambda}$. The number of multiplications performed (minus 1) is $X$.

---

## Algorithm

Samples $\mathcal{U}(0,1)$'s until (5) becomes true — i.e., until the first time $n$ such that $e^{-\lambda} > \prod_{i=1}^{n+1} U_i$.

```
a ← e^(-λ); p ← 1; X ← -1

Until p < a  
Generate U from U(0,1)  
p ← p·U  
X ← X + 1

Return X
```

**How it works:** `p` tracks the running product of uniforms. Each pass through the loop multiplies in one more $U$ and increments $X$. The loop stops as soon as the running product dips below the threshold $a = e^{-\lambda}$, and the current value of $X$ is returned.

---

## Worked Example (BCNN): Generate a Pois(2) RV

Sample until $e^{-\lambda} = 0.1353 > \prod_{i=1}^{n+1} U_i$.

| $n$ | $U_{n+1}$ | $\prod_{i=1}^{n+1} U_i$ | Stop? |
|---|---|---|---|
| 0 | 0.3911 | 0.3911 | No |
| 1 | 0.9451 | 0.3696 | No |
| 2 | 0.5033 | 0.1860 | No |
| 3 | 0.7003 | 0.1303 | **Yes** |

Since $0.1303 < 0.1353$ at $n=3$, we stop and take $X = 3$.

---

## Remarks

> [!note] Expected number of uniforms needed
> An easy argument shows the expected number of $U$'s required to generate one realization of $X$ is
> $$E[X+1] = \lambda + 1$$
> So for large $\lambda$, this algorithm requires (on average) many uniform draws per realization — it gets slower as $\lambda$ grows.

### Normal Approximation for Large $\lambda$ ($\lambda \ge 20$)

If $\lambda \ge 20$, we can instead use the Normal approximation:
$$\frac{X - \lambda}{\sqrt{\lambda}} \approx \text{Nor}(0,1)$$

**Algorithm (for $\lambda \ge 20$):**
```
Generate Z from Nor(0,1)  
Return X = max(0, ⌊λ + √λ·Z + 0.5⌋) ("continuity correction")
```

**Example:** If $\lambda = 30$ and $Z = 1.46$:
$$X = \lfloor 30.5 + \sqrt{30}(1.46) \rfloor = 38$$

> [!tip] Why the "+0.5"?
> This is a **continuity correction** — it adjusts a continuous Normal approximation to better match a discrete target distribution by rounding to the nearest integer rather than truncating.

---

## Final Remarks

> [!info] Alternative: Discrete Inverse Transform
> Another way to generate Pois$(\lambda)$ is to simply **table the c.d.f. values**, exactly like in the discrete Inverse Transform method. This may be **more efficient and accurate** than the A-R-based approach above — which isn't to say the A-R method isn't clever and elegant, just that it isn't always the fastest tool for the job.

> [!quote] Closing note
> A-R is used for many other random variables, and even entire **stochastic processes** — this is just one illustrative discrete example among many possible applications.

---

## Related
- [[Acceptance Rejection Method for Random Variable Generation]]
- [[Convolution Method For Random Variable Generation]]
- [[Inverse Transform Theorem]]
#simulation #statistics #ranking-and-selection

# Bernoulli Selection: Extensions

Continues from [[Bernoulli Selection]]. The single-stage procedure $\mathcal{B}_{SH}$ (Sobel and Huyett 1957) works, but uses a fixed sample size $n$ regardless of how the data actually look along the way. Here we look at two better procedures that save observations. Welcome to the "Bern Unit"!

---

## A Curtailment Trick (Bechhofer and Kulkarni)

**Idea:** Do the single-stage procedure exactly as before, except **stop sampling early** as soon as the population in second place can, at best, only **tie** for first.

This is called **curtailment** — you might as well stop, because it won't be possible for the outcome to change (except possibly a tie, which doesn't end up mattering since the current leader still wins or ties).

> [!tip] Key result Curtailment gives the **same** $P(\mathrm{CS})$ as the single-stage procedure, but a **lower expected number of observations** ($\le n$).

So curtailment is a free lunch: identical statistical guarantee, but you often stop sampling before reaching the full $n$ prescribed by the table.

### Example (continuing the $\mathcal{B}_{SH}$ example)

Recall from [[Bernoulli Selection]]: for $k=4$, $P^\star = 0.95$, $\Delta^\star = 0.10$, the single-stage procedure required $n = 212$ observations.

Suppose that, at the end of just **180** samples from each population, we have the intermediate result: $$ Y_{1,180} = 50,\quad Y_{2,180} = 130,\quad Y_{3,180} = 74,\quad Y_{4,180} = 97. $$

We **stop sampling right now** and select population 2 as the best, because it's not possible for population 4 (the current runner-up) to catch up in the remaining $212 - 180 = 32$ observations — even if population 4 won every remaining trial ($97 + 32 = 129 < 130$), it couldn't beat population 2. Big savings — 32 fewer observations per population than the full single-stage procedure required.

---

## A Sequential Procedure $\mathcal{B}_{BKS}$ (Bechhofer, Kiefer, and Sobel 1968)

Curtailment saves _some_ observations, but it's still tied to the fixed sample size $n$ from the single-stage table. The sequential procedure $\mathcal{B}_{BKS}$ is **even more efficient**, and uses a fundamentally different probability requirement.

### New Probability Requirement

For specified constants $(P^\star, \theta^\star)$ with $1/k < P^\star < 1$ and $\theta^\star > 1$, we require $P(\mathrm{CS}) \geq P^\star$ whenever the **odds ratio**

$$ \frac{p_{[k]}/(1-p_{[k]})}{p_{[k-1]}/(1-p_{[k-1]})} \geq \theta^\star. $$

> [!note] Contrast with $\mathcal{B}_{SH}$ The single-stage procedure's indifference-zone requirement was stated in terms of the raw **difference** $p_{[k]} - p_{[k-1]} \geq \Delta^\star$ (see [[Bernoulli Selection]]). Here, the requirement is stated in terms of the **odds ratio** instead — a different, often more natural way to quantify "how separated" the top two success probabilities are.

The procedure proceeds in **stages**, taking **one** Bernoulli observation from each of the $k$ populations at each stage.

### Procedure Details

At the $m$-th stage of experimentation ($m \geq 1$):

1. Observe the random Bernoulli vector $(X_{1m}, \dots, X_{km})$ — one new 0/1 outcome per population.
2. Compute the cumulative sums $Y_{im} = \sum_{j=1}^m X_{ij}$ for $1 \le i \le k$, and denote the ordered sums by $Y_{[1]m} \le \cdots \le Y_{[k]m}$.
3. **Stop** if $$ Z_m \equiv \sum_{i=1}^{k-1} (1/\theta^\star)^{,Y_{[k]m} - Y_{[i]m}} ;\leq; \frac{1-P^\star}{P^\star}. $$

Let $N$ be the (random) stage $m$ at which the procedure stops. **Select** the population yielding $Y_{[k]N}$ (the current leader) as the one associated with $p_{[k]}$.

> [!tip] Efficiency $\mathcal{B}_{BKS}$ is even more efficient than curtailment, since it can stop the moment the _statistical evidence_ (via $Z_m$) is strong enough — rather than waiting for a table-based $n$ to be reached, or for the second-place population to be mathematically incapable of catching up.

---

## Example

For $k = 3$ and $(P^\star, \theta^\star) = (0.75, 2)$, suppose the following sequence of vector-observations is obtained using $\mathcal{B}_{BKS}$:

|$m$|$X_{1m}$|$X_{2m}$|$X_{3m}$|$Y_{1m}$|$Y_{2m}$|$Y_{3m}$|$Z_m$|
|---|---|---|---|---|---|---|---|
|1|1|0|1|1|0|1|1.5|
|2|0|1|1|1|1|2|1.0|
|3|0|1|1|1|2|3|0.75|
|4|0|0|1|1|2|4|0.375|
|5|1|1|1|2|3|5|0.375|
|6|1|0|1|3|3|6|0.25|

The stopping threshold is $(1-P^\star)/P^\star = (1-0.75)/0.75 = 1/3 \approx 0.333$.

Since $Z_6 = 0.25 \le 1/3$, sampling **stops at stage $N = 6$**, and **population 3 is selected as best** (it has the largest cumulative sum $Y_{3,6} = 6$).

> [!note] Watch the trend $Z_m$ is monotonically shrinking as population 3 pulls ahead — once it crosses below the threshold $(1-P^\star)/P^\star$, we have enough evidence to stop and declare a winner.

---

## Summary

|Procedure|Stopping rule|Efficiency|
|---|---|---|
|$\mathcal{B}_{SH}$ (single-stage)|Fixed $n$ from table|Baseline|
|Curtailment (Bechhofer & Kulkarni)|Stop once 2nd place can't catch up within remaining $n - m$ trials|Same $P(\mathrm{CS})$, $\mathbb{E}[\text{obs}] \le n$|
|$\mathcal{B}_{BKS}$ (sequential, 1968)|Stop once $Z_m \le (1-P^\star)/P^\star$|Most efficient; adapts stage-by-stage using odds-ratio evidence|

- Curtailment is a simple "don't keep sampling once the answer can't change" improvement on top of the single-stage procedure.
- $\mathcal{B}_{BKS}$ reformulates the guarantee in terms of an **odds ratio** $\theta^\star$ rather than a raw probability difference $\Delta^\star$, and fully sequentially updates a statistic $Z_m$ after each round of one-observation-per-population sampling, stopping as soon as there's sufficient evidence.
- Both extensions preserve the desired $P(\mathrm{CS}) \geq P^\star$ guarantee while reducing the expected sampling effort compared to the fixed-$n$ single-stage procedure.
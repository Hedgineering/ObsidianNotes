#statistics #simulation #output-analysis #finite-horizon

# Finite-Horizon Output Analysis

> **Goal:** Simulate a system of interest over a _finite_ time horizon and analyze the output.

---

## 1. Types of Simulation Output

### Discrete output

Observations $Y_1, Y_2, \dots, Y_m$, where the number of observations $m$ can be:

- a **constant**, or
- **random**

**Example:** Waiting times $Y_1,\dots,Y_{100}$ of the first 100 customers at a store.

### $m$ tied to a time window $[0,T]$

$m$ = random number of customers observed during $[0,T]$, where $T$ itself can be known or random.

|$T$|$m$|Example|
|---|---|---|
|known/fixed|random|customers from 10am–2pm|
|random|random|customers from 10am until cashier leaves for a school pickup call|

### Continuous output

${Y(t) \mid 0 \le t \le T}$ observed over $[0,T]$, with $T$ known or random.

**Example:** $Y(t)$ = number of customers in queue over 8am–5pm ($T$ constant), or until the cashier has to leave ($T$ random).

---

## 2. Easiest Goal: Estimate the Mean

$$\theta \equiv \mathrm{E}[\bar{Y}_m], \qquad \bar{Y}_m \equiv \frac{1}{m}\sum_{i=1}^m Y_i$$

- The $Y_i$'s distribution **may change over the day/run** — no problem, $\theta$ is just the expected value of the average of _whatever_ these $Y_i$'s are.

**Continuous analog:** $$\theta \equiv \mathrm{E}[\bar{Y}(T)], \qquad \bar{Y}(T) \equiv \frac{1}{T}\int_0^T Y(t),dt$$

**Examples:**

- Estimate average waiting time of first 100 customers.
- Estimate time-averaged number of customers in line between 8am and 5pm.

---

## 3. The Core Problem ⚠️

$\bar{Y}_m$ is unbiased for $\theta$ _by definition_ — that part's easy.

But a proper statistical analysis also needs an estimate of $\mathrm{Var}(\bar{Y}_m)$. Two things break the usual iid tools:

1. The $Y_i$'s are **not necessarily i.i.d.** (dependent, as established previously)
2. The $Y_i$'s **may not even be identically distributed** (e.g., waiting times tend to be worse at lunch rush than at 10am)

Consequence — from the earlier variance-of-sample-mean result (Eq. 2): $$\mathrm{Var}(\bar{Y}_m) \neq \mathrm{Var}(Y_1)/m$$

And as shown before: **don't use $S_Y^2/m$ to estimate $\mathrm{Var}(\bar{Y}_m)$** — it's biased (usually too low under positive correlation).

> "Whatever shall we do??"

---

## 4. The Fix: Method of Independent Replications (IR)

**Idea:** Run the _entire simulation_ $r$ separate times, each with a different random number seed, so that the runs are genuinely independent of each other.

- Conduct $r$ independent replications of the system.
- Each replication consists of $m$ observations.
- Independence across replications is easy to guarantee — just **re-initialize with a different pseudo-random number seed** for each run.

### Notation

Sample mean of replication $i$: $$Z_i \equiv \frac{1}{m}\sum_{j=1}^m Y_{i,j}$$ where $Y_{i,j}$ = observation $j=1,\dots,m$ from replication $i=1,\dots,r$. (E.g., $Y_{i,j}$ = waiting time of customer $j$ in replication $i$.)

**Key fact:** If every replication is started under the _same operating conditions_ (e.g., all queues empty and idle), then $$Z_1, Z_2, \dots, Z_r \ \text{are i.i.d.}$$

This is the whole trick — we've converted a hard dependent-data problem into an easy iid problem, just one level up (at the level of replicate means instead of individual observations).

---

## 5. Estimating $\mathrm{Var}(\bar{Y}_m)$ via IR

Grand sample mean across replications: $$\bar{Z}_r \equiv \frac{1}{r}\sum_{i=1}^r Z_i$$

Since $\mathrm{Var}(\bar{Y}_m) = \mathrm{Var}(Z_i)$, the natural point estimator is the sample variance of the $Z_i$'s: $$S_Z^2 \equiv \frac{1}{r-1}\sum_{i=1}^r (Z_i - \bar{Z}_r)^2$$

**Why this works where $S_Y^2/m$ didn't:** $S_Z^2$ and $S_Y^2/m$ _look_ similar in form, but because the $Z_i$'s are genuinely i.i.d. (unlike the raw $Y_{i,j}$'s), $S_Z^2$ is **usually much less biased** for $\mathrm{Var}(\bar Y_m) = \mathrm{Var}(Z_i)$.

➡️ $S_Z^2 / r$ is a reasonable estimator for $\mathrm{Var}(\bar{Z}_r)$.

---

## 6. Confidence Interval for $\theta$

If $m$ (observations per replication) is large enough, a **CLT** kicks in: the replicate means are approximately $$Z_1,\dots,Z_r \overset{approx}{\sim} \text{i.i.d. } \mathrm{Nor}(\theta, \mathrm{Var}(Z_1))$$

which gives $$S_Z^2 \approx \frac{\mathrm{Var}(Z_1),\chi^2(r-1)}{r-1}$$

Standard manipulations then yield the **approximate IR $100(1-\alpha)%$ two-sided CI for $\theta$**:

$$\boxed{\theta \in \bar{Z}_r \pm t_{\alpha/2,,r-1}\sqrt{S_Z^2/r}} \tag{3}$$

This has the _same form_ as the classical iid CI, but now legitimately applies because the CLT justifies (approximate) normality and independence of the $Z_i$'s — **not** of the raw $Y_{i,j}$'s.

---

## 7. Worked Example

**Setup:** Estimate expected average waiting time for the first $m=5000$ customers at a bank. Run $r=5$ independent replications, each initialized empty & idle, each with 5000 waiting times.

|$i$|1|2|3|4|5|
|---|---|---|---|---|---|
|$Z_i$|3.2|4.3|5.1|4.2|4.6|

- $\bar{Z}_5 = 4.28$
- $S_Z^2 = 0.487$
- $\alpha = 0.05 \Rightarrow t_{0.025,4} = 2.78$

**95% CI:** $$\theta \in 4.28 \pm (2.78)\sqrt{0.487/5} = [3.41,\ 5.15]$$

---

## Summary / Mental Model

|Step|What happens|
|---|---|
|Raw output $Y_{i,j}$ within a replication|dependent, maybe non-identically distributed — hard to analyze directly|
|Replicate means $Z_i = \bar{Y}_{m}$ per run|i.i.d. across replications (same start conditions + independent seeds)|
|$S_Z^2$|valid (much less biased) estimator of $\mathrm{Var}(Z_i)$|
|CI (3)|valid approximate CI for $\theta$, relying on CLT across replications, **not** within a single run|

**Big idea:** IR sidesteps the dependence problem entirely by pushing the iid assumption up to the level of whole-replication averages rather than individual observations.

## Open Threads / To Follow Up

- [ ] How to choose $r$ (number of replications) vs. $m$ (length of each replication)
- [ ] Trade-offs of IR vs. other methods (batch means, etc.) for steady-state (vs. finite-horizon) analysis
- [ ] Variance reduction techniques to make IR more efficient (common random numbers, etc.)
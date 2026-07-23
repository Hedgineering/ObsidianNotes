#statistics #input-analysis #simulation #problem-data

# Problem Children: Input Analysis in the Real World

Companion note to [[Method of Moments]], [[Goodness-of-Fit Tests]], and friends. Those notes assume you can find a nice, clean, i.i.d. sample and fit a textbook distribution to it. Real data is often not so cooperative — this note covers the common "problem children" you'll run into during **input analysis** (e.g., for simulation modeling) and what to do about them.

---

## Overview

> Nobody likes to talk about them, but every family has them. You'd think that after all the theory we've done, we could always find good distributions to fit our data. Not exactly.

Four categories of trouble:

1. **No / little data**
2. **Data that doesn't look like one of the usual distributions** ("goofy" data)
3. **Nonstationary data** (from distributions that change over time)
4. **Multivariate / correlated data**

---

## 1. No / Little Data

This issue turns up more often than you'd expect: there could be literally **no data available**, or the data you have is awful (goofy values, not cleaned properly, etc.). There are **no great options** here — but some suggestions:

### Interview "experts"

- Try to at least get **min**, **max**, and **"most likely"** values out of them — then you can guess a **Uniform** or **Triangular** distribution.
- Getting **quantiles** from the expert is even better (more information than just min/max/mode).
- At minimum, discuss the **nature of the observations** with them.

### Reason about the nature of the random variable

If you have some idea about the nature of the RVs, you may be able to make a good guess as to the distribution:

|Question|Suggests|
|---|---|
|Discrete or continuous?|Narrows the family broadly|
|Are observations successes/failures?|Bernoulli, Binomial, ...|
|Do observations adhere to Poisson assumptions?|Poisson (counting arrivals) or Exponential (interarrival times)|
|Are observations averages or sums?|Normal (CLT-type reasoning)|
|Are observations bounded?|Beta|
|Reliability or job times?|Gamma, Weibull, Lognormal, etc.|
|Anything from the physical characteristics of the RV?|Use domain knowledge directly|

> **Bottom line:** when data is scarce, lean on subject-matter expertise and the _structural_ properties of the process (bounded? memoryless? a sum of things?) to pick a plausible distribution family.

---

## 2. Goofy Distributions

Sometimes your histogram just doesn't look like anything in the textbook. Classic example: a **forced marriage of two normals** — most software packages can't pick this up or fit it properly.

> **Example:** a poorly designed exam has two modes (easy questions cluster one way, hard questions another) — the resulting score histogram is clearly **bimodal**, and fitting a single Normal to it (as many packages default to) gives a terrible fit, visibly missing the two humps.

### Options

- **Model as a mixture** of reasonable distributions (e.g., a mixture of two Normals). More faithful to the underlying process, but harder to fit and simulate from.
- **Easier: sample from the empirical distribution** (or a _smoothed_ version of the empirical distribution) instead of fitting a named parametric family at all. This is a form of **bootstrapping**.

> **Trade-off:** mixtures require you to estimate more parameters (mixing weights + each component's parameters) but generalize to new sample sizes; empirical/bootstrap resampling is simple and assumption-free but is tied to the exact data you observed (limited extrapolation beyond the observed range).

---

## 3. Nonstationary Data

Arrival rates (or other process parameters) can **change over time** — think restaurants (lunch/dinner rush), highway traffic (rush hour), call center activity (time-of-day patterns), or seasonal product demand.

> **You must take this variability into account, or else: GIGO** (garbage in, garbage out).

### Suggestion: Nonhomogeneous Poisson Process (NHPP)

- Recall from the RV Generation module — instead of a constant arrival rate $\lambda$, use a **rate function** $\lambda(t)$ that varies with time.
- You need to **model the rate function properly** (not just eyeball it).
- **Arena** (and similar simulation software) typically uses a **piecewise-constant rate function** — i.e., you specify a constant arrival rate for each separate time period (e.g., a different $\lambda$ for each hour of the day).

---

## 4. Multivariate / Correlated Data

Data don't have to be i.i.d.! Two related ways this shows up:

### Multivariate random variables

Multiple variables measured on the same subject/unit may be correlated with each other.

> **Example:** a person's height and weight are correlated.

### Serially correlated data

Observations that are correlated with each other **across time**:

- Monthly unemployment rates.
- Arrivals to a social media site — may be correlated if an interesting item appears and the public "gets wind of it" (viral clustering).
- A badly damaged part may require more service than usual at a **series** of stations (correlated service times across the line).
- If a server gets tired, their service times may be longer than usual as a shift goes on (correlated over time).

### What do you need to do?

1. **Identify** multivariate / serial correlation situations — don't just assume independence.
2. **Propose appropriate models**, e.g.:
    - **Multivariate Normal** for jointly-distributed, correlated variables like heights and weights.
    - **Time series models** for serially correlated observations, e.g., autoregressive-moving average $\mathrm{ARMA}(p,q)$, $\mathrm{EAR}(1)$, ARTOP, etc.
3. **Estimate the relevant parameters** (easier said than done):
    - Multivariate Normal: marginal means and variances are no big deal; **covariances** may not be so easy.
    - Time series: for simple models like $\mathrm{AR}(1)$, estimating the coefficient $\phi$ is easy (similar to estimating a covariance). But coefficients for more complicated models need to be estimated using dedicated software, e.g., **Box–Jenkins** methodology.
4. **Validate** to see if your estimated model is actually any good.
5. **Alternative**: bootstrap samples from an empirical (joint/sequential) distribution, if you have enough data.

---

## Summary / Key Takeaways

|Problem|Core fix|
|---|---|
|No / little data|Lean on expert judgment (min/max/mode or quantiles) and structural reasoning about the RV's nature|
|Goofy (e.g. multimodal) data|Model as a mixture, or bootstrap from the (smoothed) empirical distribution|
|Nonstationary data|Model with a nonhomogeneous Poisson process; often piecewise-constant rate function|
|Multivariate / correlated data|Use multivariate distributions (e.g. Multivariate Normal) or time series models (ARMA, AR(1), etc.); estimate covariances/coefficients carefully; validate|

> **General theme:** when the standard i.i.d.-single-named-distribution toolkit doesn't fit reality, fall back on (a) domain knowledge / expert elicitation, (b) more flexible models (mixtures, time series, multivariate distributions), or (c) resampling directly from the data itself (bootstrapping) — often the most robust fallback when data is scarce or theory doesn't cooperate.


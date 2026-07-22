
#simulation #statistics #input-modeling

# Identifying Distributions for Simulation Inputs

Before you can simulate a system, you need to model its stochastic inputs (arrival times, service times, demand, etc.) with a probability distribution. This note covers the basic exploratory tools for **looking at data** and a decision framework for **guessing a reasonable family** of distributions before formally testing goodness-of-fit.

> [!abstract] Big picture workflow
> 
> 1. Explore the data visually (histogram, stem-and-leaf).
> 2. Ask structural questions about the data (discrete/continuous, univariate/multivariate, how much data, expert input available?).
> 3. Choose a "reasonable" candidate distribution based on the mechanism generating the data.
> 4. Fit parameters and **test** the hypothesis (probability plots, goodness-of-fit tests).
> 5. If it fails, revise and repeat.

---

## 1. Exploratory Tools

### 1.1 Histograms

A histogram bins the data and plots frequency (or relative frequency/density) per bin. It's the standard first look at the **shape** of a distribution: symmetry, skew, modality, tails, outliers.

- Bin width matters a lot — too few bins oversmooths and hides structure; too many bins is noisy and just re-displays raw data.
- Common rules of thumb for bin count: Sturges' rule ($k = \lceil \log_2 n \rceil + 1$), Scott's rule, Freedman–Diaconis rule (uses IQR, more robust to outliers).
- A histogram is really an estimate of the **PDF** (continuous) or **PMF** (discrete) of the underlying random variable.

### 1.2 Stem-and-Leaf Plots

A stem-and-leaf plot is essentially **a histogram turned on its side that keeps the actual numerical values**, rather than throwing them away into bins. Each data point is split into a "stem" (leading digit(s)) and a "leaf" (trailing digit), e.g.:

```
10 | 000
 9 | 998764422110
 8 | 9764544321
 7 | 965432100
 6 | 7532
 5 | 433
 4 | 8
```

- Advantages over a plain histogram: you can reconstruct the exact data values, and it doubles as a rough sorted list.
- Good for small-to-moderate datasets by hand; less practical for huge datasets (use a histogram or KDE instead).
- Like a histogram, it gives you shape info: here the data is left-skewed with a long lower tail down to 48.

### 1.3 Other quick exploratory tools worth knowing

- **Box plot**: median, quartiles, whiskers, outliers — good for skew/outlier detection and comparing groups.
- **Kernel density estimate (KDE)**: smoothed, bin-free alternative to a histogram.
- **Scatter plot / lag plot**: check independence between consecutive observations (important — many distribution-fitting techniques assume i.i.d. data).
- **Time series plot**: check for stationarity/trend before assuming the data is even i.i.d.

---

## 2. Why Histograms "Work": Glivenko–Cantelli

> [!theorem] Glivenko–Cantelli Theorem (Fundamental Theorem of Statistics) states that with enough samples, a histogram will approach the true PDF or PMF of the underlying stationary Random Variable's Distribution.
> Let $X_1, X_2, \dots, X_n$ be i.i.d. samples from a distribution with true CDF $F(x)$. 
> Let $F_n(x)$ be the **empirical CDF**: 
> $$F_n(x) = \frac{1}{n}\sum_{i=1}^n \mathbb{1}({X_i \le x})$$ 
> Then $F_n(x)$ converges to $F(x)$ **uniformly** and **almost surely** as $n \to \infty$: 
> $$ \sup_x |F_n(x) - F(x)| \xrightarrow{a.s.} 0 > $$

**Intuition / relevance to simulation input modeling:**

- This is the theoretical justification for using empirical data (histograms, empirical CDFs) as a stand-in for the true underlying distribution.
- Given enough i.i.d. observations of a **stationary** random variable, and a histogram with an appropriately chosen number of bins, the histogram will converge to the true PDF/PMF.
- This is also the basis for the **Kolmogorov–Smirnov (K-S) goodness-of-fit test**, which measures the max distance $\sup_x |F_n(x) - F(x)|$ between the empirical CDF and a hypothesized CDF.
- Caveat: convergence requires (a) enough data, (b) i.i.d./stationary observations — if the process is non-stationary (e.g., time-varying arrival rates), a single histogram over all the data will mislead you.

---

## 3. Which Distribution? — A Decision Framework

### 3.1 Questions to ask before anything else

- **Discrete vs. Continuous?** Are outcomes counts/categories, or measurements on a continuum?
- **Univariate vs. Multivariate?** Is there dependence between multiple inputs (e.g., processing time correlated with part size)?
- **How much data is available?** Lots of data → can fit a specific parametric family and test it rigorously. Little/no data → may need to rely on subjective/expert estimates (e.g., min/mode/max → triangular).
- **Are subject-matter experts available** to describe the physical/logical mechanism generating the data?
- **What if you have no data at all?** Can you at least bound it (min/max) or guess a "most likely" value? This still lets you pick something better than nothing (e.g., Uniform or Triangular).

### 3.2 Discrete Distributions

|Distribution|Typical use case|Key parameter(s)|
|---|---|---|
|**Bernoulli(p)**|Single yes/no trial (success prob. $p$) — e.g., is a part defective?|$p$|
|**Binomial(n, p)**|Number of successes in $n$ independent Bernoulli($p$) trials — e.g., # defective parts out of a batch of $n$|$n, p$|
|**Geometric(p)**|Number of Bernoulli($p$) trials until the **first** success — e.g., number of attempts until a machine successfully starts|$p$|
|**Negative Binomial**|Number of trials until the $r$-th success (generalizes Geometric) — e.g., attempts needed to get $r$ successful sales calls|$r, p$|
|**Poisson(λ)**|Counts of arrivals/events over a fixed interval of time or space, when events happen independently at a constant average rate — e.g., customer arrivals per hour, calls per minute|$\lambda$ (rate)|
|**Empirical ("sample") distribution**|Just resample directly from the observed data itself — used when no standard parametric family fits well, or you want to preserve data quirks exactly|none (data-driven)|

**Discrete decision hints:**

- Fixed number of independent trials, want # of successes → **Binomial**.
- Want to know _how long until_ the first success → **Geometric**.
- Events happening continuously in time at a constant rate, counting how many occur → **Poisson**.
- Nothing fits and you have plenty of data → **Empirical**.

### 3.3 Continuous Distributions

|Distribution|Typical use case|Key info needed|
|---|---|---|
|**Uniform**|Almost no information about the shape — only know the min and max possible values|min, max|
|**Triangular**|Little/no data, but you (or an expert) can estimate min, max, and the "most likely" (mode) value — very common when data is scarce|min, mode, max|
|**Exponential(λ)**|Time **between** arrivals in a Poisson process (memoryless) — e.g., interarrival times of customers, time between machine failures|rate $\lambda$ (or mean $1/\lambda$)|
|**Normal**|Good default model for quantities that result from many small additive effects — heights, weights, IQs, measurement error, and (via CLT) sample means|mean $\mu$, std dev $\sigma$|
|**Beta**|Flexible distribution on a bounded interval $[0,1]$ (or rescaled) — good when you know the data is naturally bounded, e.g., proportions, fractions, probabilities themselves|shape params $\alpha, \beta$|
|**Gamma**|Sum of several exponential random variables; time until the $k$-th event in a Poisson process; general-purpose skewed positive data / reliability & lifetime data|shape $k$, rate $\theta$|
|**Weibull**|Reliability/lifetime/failure-time data, especially when failure rate changes over time (increasing = wear-out, decreasing = infant mortality)|shape, scale|
|**Lognormal**|Positive data that is the _product_ of many independent effects (multiplicative processes) — e.g., income distributions, particle sizes, some service times|$\mu, \sigma$ of underlying normal|
|**Gumbel**|Extreme value data — modeling the max or min of many random variables, e.g., flood levels, extreme wind speeds|location, scale|
|**Empirical ("sample") distribution**|Same idea as discrete case: resample from the data directly when no clean parametric fit exists|none (data-driven)|

**Continuous decision hints:**

- No data, just bounds → **Uniform**.
- No data, but bounds + a "most likely" guess → **Triangular**.
- Time between independent, constant-rate events → **Exponential**.
- Bounded on [0,1], want flexible shape (skew, U-shaped, etc.) → **Beta**.
- Time until several independent events happen (sum of exponentials) → **Gamma**.
- Reliability/failure-time data where hazard rate isn't constant → **Weibull**.
- Positive, right-skewed, multiplicative-growth process → **Lognormal**.
- Care about extremes (max/min of a process) → **Gumbel** (or other extreme value distributions).
- Lots of raw data, symmetric bell shape → **Normal**.
- Nothing standard fits, and you have a lot of data → **Empirical**.

---

## 4. Testing the Hypothesis ("Game Plan")

Once you've picked a _candidate_ distribution, don't just trust your gut — test it.

1. **Fit parameters** to the data (method of moments, MLE, etc.).
2. **Probability plots** (e.g., normal probability plot / Q-Q plot): if the hypothesized distribution is correct, the sorted data plotted against the theoretical quantiles should fall approximately on a straight line. Systematic curvature = bad fit (e.g., indicates skew the candidate distribution doesn't capture).
3. **Goodness-of-fit tests**:
    - **Chi-square test** — compares observed vs. expected bin counts (works for both discrete and binned continuous data).
    - **Kolmogorov–Smirnov (K-S) test** — compares empirical CDF to hypothesized CDF (continuous data, and connects directly back to Glivenko–Cantelli).
    - **Anderson–Darling test** — like K-S but weights the tails more heavily; often more powerful.
4. If the candidate fails these tests, revise: try a different family, transform the data, or consider that the process might be a **mixture** of distributions or **non-stationary**.

> [!tip] Rule of thumb Choose a "reasonable" distribution based on the physical mechanism generating the data, then test it — don't just try to force-fit whatever distribution has the best-looking histogram match. Understanding _why_ the data should follow a certain distribution (e.g., "this is a count of rare independent events, so Poisson makes sense") is more robust than pure curve-fitting.

---

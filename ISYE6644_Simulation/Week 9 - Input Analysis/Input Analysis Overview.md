
Helps identify which RVs we use as inputs to a simulation (we want to avoid Garbage-in-Garbage-out). Goal is to identify and use RVs that adequately approximate what is going on in the real world. Examples include
- Interarrival times
- Service times
- Breakdown times

The process loop looks like this:
1. Assume a distribution and simulate expected values / data
2. Observe real data and gather empirical evidence
3. Conduct some Goodness-of-Fit or other statistical test to see if the assumed distribution is adequate
4. If inadequate, find another distribution or modeling setup that hopefully more accurately models reality using intuition, MLE, Method of Moments, etc.
5. Iterate until you have a good model, and simulate for further analysis
## Identifying Distributions

[[Basic Methods of Identifying Distributions for Simulation Inputs]]

Covers steps you can take to discover the underlying distribution of a stationary random variable using things like Histograms, Stem-and-Leaf plots, and intuition based on what various distributions are often used for.
## Unbiased Point Estimators

[[Unbiased Point Estimation]]

A **point estimator** is a statistic used to estimate a true distribution parameter (e.g., $\bar{X}$ is a statistic and a point estimator for the population mean $\mu$). > > The sample mean $\bar{X}$ is unbiased for $\mu$, and the sample variance $S^2$ is unbiased for $\sigma^2$ — for **any** underlying distribution.

However, unbiasedness does **not** carry through nonlinear transformations: the sample standard deviation $S = \sqrt{S^2}$ is **biased** for $\sigma$ (it tends to *underestimate* it), even though $S^2$ itself is unbiased for $\sigma^2$. 

> [!info]- Why does taking a square root break unbiasedness? 
>This follows from **Jensen's inequality**. For a concave function $g$ (like $g(x) = \sqrt{x}$) applied to a random variable $Y$: 
> $$\mathrm{E}[g(Y)] \le g(\mathrm{E}[Y])$$ 
> Applying this with $Y = S^2$ and $g(x) = \sqrt{x}$: > $$\mathrm{E}[S] = \mathrm{E}[\sqrt{S^2}] \le \sqrt{\mathrm{E}[S^2]} = \sqrt{\sigma^2} = \sigma$$ 
>  So $S$ is biased **downward** for $\sigma$ — it's not that "variance is biased for standard deviation," but specifically that the nonlinear square-root transformation of an unbiased estimator ($S^2 \to S$) doesn't preserve unbiasedness.
>   Same phenomenon shows up elsewhere: $\bar{X}$ is unbiased for $1/\lambda$ when $X_i \sim \mathrm{Exp}(\lambda)$, but $1/\bar{X}$ is **biased** for $\lambda$, since $g(x) = 1/x$ is convex and Jensen's inequality flips the other way: 
>   $$\mathrm{E}[1/\bar{X}] \ge 1/\mathrm{E}[\bar{X}] = \lambda$$
## Mean Squared Error

Bias = difference in expected value of a statistic from the actual parameter it is supposed to be estimating
Variance = variance of the estimator across many realizations of the statistic being estimated

An estimator is basically a function that uses several realizations of a random variable to estimate some population parameter. We want to choose an estimator (function) that is good by choosing the estimator with the least bias and variance. 

Mean Squared Error is basically a metric that combines the bias and variance of a given estimator to help you choose the best estimator (the one with the least Mean Squared Error)

[[Mean Squared Error]]
## Maximum Likelihood Estimators

[[Maximum Likelihood Estimators]]

The likelihood of seeing some collection of data can be seen as the product of the independent p.d.f at each realization of each datapoint. This definition of likelihood can be used to search for the parameters that maximize this likelihood probability. Taking the natural log of both sides and setting to zero is a useful trick in direct analytical optimization using MLE.
## Method of Moments

[[Method of Moments]]

The $k$-th moment of a random variable is defined as the expectation of a random variable raised to the $k$-th power. You can use this to estimate parameters of the underlying p.d.f. of a sample of data.
## Goodness-of-Fit Tests

[[Goodness of Fit Tests]]
[[Goodness of Fit Tests Exponential Example]]
[[Goodness of Fit Tests Weibull Example]]
[[Other Goodness of Fit Tests]]

## Problematic Scenarios

[[Problem Children in Input Analysis]]

>[!important] You can use Tools > Input Analyzer in Arena to analyze data to figure out what data generating distribution with what parameters a dataset likely comes from. You can use "Fit All" to test all distribution and choose the ones with the best $\chi^2$ or K-S test statistics.


# Random Variate Generation Methods
## Inverse Transform Method

**Theorem**: If $X$ is a continuous random variable with CDF $F(x)$, then the random variable $F(X) \sim Unif(0, 1)$ . 

If you plug the random variable itself into its own CDF function, the resulting distribution is uniform between 0 and 1.

[[Random Numbers#Generating Continuous Random Variables (Inverse Transform)]]
[[Random Numbers#Generating Arbitrary Discrete Random Variables (Inverse Transform Method)]]

[[Inverse Transform Theorem]]

## Convolution Method

You can generate Random Variables by summing other Random Variables from other distributions. See [[Convolution Method For Random Variable Generation]]
## Acceptance-Rejection Method
[[Acceptance Rejection Method for Random Variable Generation]]
## Composition Method

[[Composition of Random Variables]]

You can break down a random variable into a linear combination of other, easier to generate random variables.

## Box-Muller Normals

[[Box-Muller Standard Normal Random Variable Generation]]

You can use $ln$ with $sin$ or $cos$ to generate a standard random normal.
There's also a slightly computationally faster way (Polar method) that avoids trig calculations .

It can also be observed that due to the Box-Muller approach being a valid way of generating standard normal RVs computationally, some corollaries also hold true:
- A RV that is a sum of two chi-square 1 RV's gives a chi-square 2 RV, which also happens to be the same as an exponential RV with parameter $\lambda = \frac{1}{2}$.
- A Cauchy RV is simply the ratio of two standard normal RVs
## Order Statistics

[[Order Statistics and Other Stuff]]

An $n$-order statistic in the Probability/Simulation context is the $n$th largest or smallest value of a collection of iid RV realizations collected.

`Keyword: (min of n iid RVs); Generating min's and max's of iid RV's, e.g. min of iid Exponentials`
Basically, instead of a process that takes $n$ steps to compute something like the minimum or maximum of $n$ RVs of some given distribution, you can use some clever manipulation to shortcut it to 1 step of an equivalent alternative RV using some algebra and probability theory. 

## Multivariate Normal Distribution
[[Multivariate Normal Random Variable Generation]]

Algorithm and python implementation of multi-variate (n-dimensional) normal random variable generation with correlation/interaction terms across dimensions.
## Baby Stochastic Processes
[[Baby Stochastic Process (Markov Chains and Poisson Arrivals)]]

Markov Chains represent multiple states, and transitions to other states. Underlying assumption being that transition to next state depends only on current state (not states before it -- called memory-less or stateless property)

Poisson Arrivals can be homogenous -- in which case they can be modeled simply using a poisson (inverse exponential) random variable -- or non-homogenous/non-stationary in which case we need to use a thinning algorithm (think acceptance-rejection type RV generation) to create realizations when the Poisson Parameter $\lambda$ varies over time with some known function $\lambda(t)$.
## Non-homogenous Poisson
[[Non-homogenous Poisson Processes]]

When the Poisson Parameter $\lambda$ varies over time with some known function $\lambda(t)$. Naive implementation of this doesn't work because it doesn't properly handle cases of large lambda within an interval (it looks at interval start points rather than the full p.d.f of the given interval), which is why we need to use an acceptance-rejection thinning algorithm to correct for it.
## Time Series
[[Time Series ARMA and ARP]]

Auto-regressive Exponential and Auto-Regressive Pareto are somewhat new

The EAR(1) process is useful for generating **correlated exponential random variables**.
The ARP(1) goal is to generate a sequence of **correlated Pareto random variables** -- which are heavy tailed random variables, useful for simulating tail-risk scenarios.
## Queueing Processes
[[Random Variable Generation for Queueing Processes - MM1 and Lindley Recursion]]

A quick and efficient way to generate waiting and service times for a simple queuing process.
## Brownian Motion


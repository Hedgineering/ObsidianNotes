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

Basically, instead of a process that takes $n$ steps to compute something like the minimum or maximum of $n$ RVs of some given distribution, you can use some clever manipulation to shortcut it to 1 step of an equivalent alternative RV using some algebra and probability theory. 

## Multivariate Normal Distribution
[[Multivariate Normal Random Variable Generation]]

Algorithm and python implementation of multi-variate (n-dimensional) normal random variable generation with correlation/interaction terms across dimensions.
## Baby Stochastic Processes

## Non-homogenous Poisson

## Time Series

## Queueing Processes

## Brownian Motion
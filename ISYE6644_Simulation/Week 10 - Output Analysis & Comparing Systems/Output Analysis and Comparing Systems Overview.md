## Steps in a Simulation Study
1. Preliminary Analysis of System
2. Model Building (input analysis, testing, iteration)
3. Verification and Validation (loop between steps 2 and 3)
4. Experimental Design & Simulation Runs
5. Statistical Analysis of Output <-- We are covering this topic in this module
6. Implementation


Input processes are Random Variables (RVs) e.g interarrival times, service times, breakdown times.

Therefore, simulation outputs also contain some randomness. 

Simulation results yield **estimates** of measures of system performance. Since these estimators themselves are random variables, simulation results are subject to sampling error.

Things we care about measuring:
- Means
- Variances
- Quantiles (e.g. what is the 99% quantile of the line length in a given queue)
- Success probabilities

We want **point estimators** and **confidence intervals** of the above measures.

>[!error] Simulations almost NEVER produce raw output that is independent and identically distributed (i.i.d.)

- **Not Independent** - Often see serial correlation (if one customer waits for a long time, the next is likely to do so)
- **Not Identically Distributed** - Morning customers may have much shorter wait than those around closing or night time
- **Not Normally Distributed** - often skewed to the right, and almost never less than zero

## Finite-Horizon Analysis (Terminating ; short term performance)

E.g. 
- Mass transit systems during rush hour
- Distribution system over one month period
- Production system until a set of machines break down
- Start-up phase of any system -- stationary or non-stationary
## Finite-Horizon Extensions


## Simulation Initialization Issues


## Steady-State Analysis (Long term analysis)
Often some parameter of the long term equilibrium distribution of an output process

E.g.
- Mean wait or delay of continuously operating comms system
- Distribution system over LONG period of time
- Many Markov chains

## Properties of Batch Means


## Other Steady-State (S-S) Methods


---
Stats and simulation experiments are typically performed to analyze or compare a "small" number of systems, say < 200

Method depends on the type of comparison and properties of the data

One System --> Use traditional confidence intervals (CIs) based on the normal or t-distribution from classical statistics

Two Systems --> Could again use CIs from baby stats, maybe even clever ones based on paired observations

More than Two Systems --> Need Ranking and Selection techniques

Confidence Intervals can be used around:
- means
- variances
- quantiles
- one-sample
- two-sample (e.g. differences in means)
- >2 samples
- "classical" baby stats environment
- simulation environment (observations are not i.i.d. normal)
## Confidence Interval for the Mean


## CIs for Difference in Two Means


## Paired CI for Difference in Two Means


## CIs for Mean Differences in Simulations


## Variance Reduction Techniques
### Common Random Numbers


### Antithetic Random Numbers


### Control Variates


## Ranking and Selection Methods


## Normal Means Selection Problem


## Single-Stage Procedure


## Normal Extensions


## Bernoulli Probability Selection


## Bernoulli Extensions


## Multinomial cell selection


## Single-stage procedure + extensions
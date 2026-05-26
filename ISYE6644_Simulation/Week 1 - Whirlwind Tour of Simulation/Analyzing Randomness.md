## Terminating Simulations
Short term behavior
- Avg customer waiting time in a bank
- Avg # of infected victims during a pandemic

Usually analyzed via independent replications
1. Make independent runs (replications) of the simulation, each under identical conditions
2. Save sample means from each replication, assume the sample mans are i.i.d normal (Central Limit Theorem)
3. Use classical stats techniques on the **sample means** (avg.s of several replications), not the original observations

## Steady-State Simulations
Simulations that converge on some consistent state of the system over time -- the long run behavior of some system.

First deal with initialization (start-up) bias (state in beginning of sim is not necessarily what long term steady state will be like)
- usually some "warm up" simulation before collecting data

### Ways to deal with steady state data
- Batch means
- Overlapping Batch Means / Spectral analysis
- Standardized Time Series Analysis
- Regeneration

#### What is Batch Means?
1. Make one long run (as opposed to many shorter reps)
2. Warm up simulation before collecting data
3. Chop remaining observations into contiguous batches
4. Take sample means from each batch, assume they are i.i.d. normal (Central Limit Theorem)
5. Do Classical Stats on these batch means


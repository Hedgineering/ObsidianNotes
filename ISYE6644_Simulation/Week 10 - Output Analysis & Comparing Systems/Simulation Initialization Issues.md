#statistics #simulation #output-analysis #initialization-bias #steady-state

# Simulation Initialization Issues

> **Last time:** extensions of the IR output analysis method for terminating simulations. **This time:** Initialization — how to start a simulation and when to keep the data. **Idea:** this is a combination of **art + science**.

---

## 1. The Problem

Before a simulation can run, one must supply **initial values for all state variables**.

- The experimenter may not know the "right" initial values, so they're often chosen **arbitrarily**.
- Classic example: it's "convenient" to initialize a queue as **empty and idle**.
- This choice can have a **significant but unrecognized impact** on the simulation run's outcome.

This is called **initialization bias** — and it's especially problematic for **steady-state** output analysis (less of an issue if you're deliberately studying a terminating/transient system that really does start empty and idle).

---

## 2. Some Initialization Issues

- Visual detection of initialization effects is **sometimes difficult** — e.g., queueing systems with high intrinsic variance can visually swamp the bias signal.
- How _should_ a simulation be initialized? Depends on context. Example:
    - A machine shop closes at a fixed time each day.
    - Each new day should start with demand depending on the **jobs remaining from the previous day** — not from some arbitrary/default state.
- **Consequences of initialization bias:**
    - Point estimators for steady-state parameters can have **high MSE**
    - Confidence intervals can have **poor coverage**

---

## 3. Detecting Initialization Bias

### (a) Visual detection

Scan the simulation output for a "burn-in" pattern (an initial trend before settling down).

- Downsides: tedious, and can easily **miss** subtle bias.
- To make visual detection more effective:
    - Transform the data (e.g., take logs or square roots)
    - **Smooth** the data
    - **Average across several replications** (see the Welch method below 👇)

### (b) Statistical tests

Formal tests asking: does the **mean** or **variance** of the process change over time?

---

## 4. Dealing with Bias — Method (a): Truncation

**Idea:** let the simulation "warm up," and only start retaining data for analysis after some **truncation point**.

- The experimenter hopes the _remaining_ (post-truncation) data are representative of the true steady-state system.
- Output truncation is the **most popular** method for handling initialization bias — all major simulation languages have built-in truncation functions.

### The hard part: choosing a good truncation point

- Truncate **too early** → significant bias may still remain in the retained data.
- Truncate **too late** → good (usable) observations are wasted.
- **No simple rule performs well in general.**

### Welch's Method (1983) — a reasonable practice

1. Run **several independent replications** of the simulation.
2. At each time step, **average the observations across replications** — this cancels out a lot of the noise that would otherwise obscure the initial transient in a single run.
3. Take a **moving average** of that across-replication average (smooths further).
4. **Visually inspect** the smoothed curve and pick a truncation point around where it appears to level off into steady state.

> **Why this combo works:** averaging across reps kills the _across-replication_ random noise, while the moving average kills _within-run_ short-term noise — leaving the underlying transient trend much more visible than in raw output.

**What the resulting plot looks like, conceptually:** individual raw replication series are noisy and don't clearly reveal any transient. The smoothed, across-replication-averaged series, by contrast, shows a clear rising trend early on that then **flattens out** once the system reaches steady state. That flattening point is read as "the process has reached steady state," and everything before it is truncated/discarded.

- New, more sophisticated **sequential change-point detection algorithms** may also help automate this choice.

---

## 5. Dealing with Bias — Method (b): Very Long Runs

**Idea:** just make the run so long that the initialization effect becomes negligible relative to the total run length ("overwhelm" it rather than removing it).

- Conceptually simple to carry out.
- May yield point estimators with **lower MSE** than the truncation method (Fishman 1978).
- **Downside:** can be wasteful with observations — might need an **excessively long run** before initialization effects become negligible.

---

## 6. Bottom Line

> Once initialization effects are dealt with (via truncation, long runs, or both), you can proceed to do proper **steady-state** analysis.

## Summary Table

|Method|How it works|Pros|Cons|
|---|---|---|---|
|Truncation|Discard early "warm-up" data|Most popular; built into simulation software|Hard to pick a good cutoff — too early leaves bias, too late wastes data|
|Welch's method|Average across reps → moving average → visually pick cutoff|Makes the transient visible above the noise|Still a visual/subjective judgment call|
|Very long run|Let the run "dilute" the initial bias|Simple; can have lower MSE than truncation|Potentially very wasteful of computation/observations|

## Open Threads / To Follow Up

- [ ] Formal statistical tests for detecting initialization bias (mean/variance change-point tests)
- [ ] Sequential/automated change-point detection algorithms for choosing truncation points
- [ ] How initialization bias interacts with the independent replications (IR) method covered previously
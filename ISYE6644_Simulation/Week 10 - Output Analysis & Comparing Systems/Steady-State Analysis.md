#statistics #simulation #output-analysis #steady-state #batch-means

# Steady-State Simulation Analysis

> **Last time:** simulation initialization issues, as a lead-up to... **This time:** Steady-state simulation analysis. Remaining lessons in this module:
> 
> - Intro to steady-state (s-s) analysis
> - Properties of Batch Means

---

## 1. Setup and Goal

Assume we have on hand **stationary (steady-state)** simulation output $Y_1, Y_2, \dots, Y_n$ (i.e., initialization bias has already been dealt with — see previous notes on initialization).

**Goal:** Estimate some parameter of interest, e.g.:

- mean customer waiting time
- expected profit produced by a certain factory configuration

Specifically, suppose the mean of this output is the unknown quantity $\mu$. We'll use the sample mean $\bar{Y}_n$ to estimate $\mu$.

Just as with terminating/finite-horizon simulations (where we used the method of Independent Replications, or IR), **any point estimator must be accompanied by a measure of its variance.**

---

## 2. The Variance Parameter $\sigma^2$

Rather than working with $\mathrm{Var}(\bar{Y}_n)$ directly (which depends on $n$ and is awkward to estimate), we work with the **variance parameter**:

$$\sigma^2 \equiv \lim_{n\to\infty} n,\mathrm{Var}(\bar{Y}_n)$$

Using the earlier result (Eq. 2 from prior notes) relating $\mathrm{Var}(\bar Y_n)$ to the covariance function $R_k$:

$$\sigma^2 = \lim_{n\to\infty}\left[R_0 + 2\sum_{k=1}^{n-1}\left(1-\frac{k}{n}\right)R_k\right] = \sum_{k=-\infty}^{\infty} R_k$$

(this last equality holds if the $R_k$'s decrease to 0 quickly enough as $k\to\infty$).

$\sigma^2$ shows up **everywhere** — simulation output analysis, Brownian motion, financial engineering applications, etc.

### Example: MA(1) process

$$Y_{i+1} = \theta\epsilon_i + \epsilon_{i+1}, \qquad \epsilon_i \overset{iid}{\sim}\mathrm{Nor}(0,1)$$

- $R_0 = 1+\theta^2$, $R_{\pm 1} = \theta$, $R_k = 0$ otherwise.
- $n\mathrm{Var}(\bar Y_n) = R_0 + 2\sum_{k=1}^{n-1}\left(1-\frac{k}{n}\right)R_k = (1+\theta)^2 - \frac{2\theta}{n}$
- So $\sigma^2 = \lim_{n\to\infty} n\mathrm{Var}(\bar Y_n) = (1+\theta)^2$.

### Example: AR(1) process

$$Y_{i+1} = \phi Y_i + \epsilon_{i+1}, \qquad \epsilon_i \overset{iid}{\sim}\mathrm{Nor}(0,1-\phi^2), \quad -1<\phi<1, \quad Y_0\sim\mathrm{Nor}(0,1)$$

- $R_k = \phi^{|k|}$ for all $k$.
- After algebra: $\sigma^2 = \dfrac{1+\phi}{1-\phi}$.
- **Note:** $\sigma^2$ **explodes** as $\phi \to 1$ — i.e., as correlation approaches perfect positive dependence, the effective "information content" of the data collapses (consistent with earlier notes: strong positive correlation makes $\sigma^2 \gg \mathrm{Var}(Y_1)$).

### The general toolbox

Many methods exist for estimating $\sigma^2$ and for conducting steady-state output analysis in general:

- **Batch means** (this document's main focus)
- **Independent replications (IR)**
- Standardized time series
- Spectral analysis
- Regeneration
- ARMA time series modeling

---

## 3. The Method of Batch Means (BM)

**Purpose:** estimate $\sigma^2$, and calculate confidence intervals for $\mu$.

### Idea

Instead of running many independent replications (IR), **divide one long simulation run into a number of contiguous batches**, then appeal to a **central limit theorem** to assume the resulting **batch sample means** are approximately i.i.d. normal.

### Setup

Partition $Y_1, Y_2, \dots, Y_n$ into $b$ nonoverlapping, contiguous batches, each with $m$ observations (assume $n = bm$):

$$\underbrace{Y_1,\dots,Y_m}_{\text{batch }1},\ \underbrace{Y_{m+1},\dots,Y_{2m}}_{\text{batch }2},\ \dots,\ \underbrace{Y_{(b-1)m+1},\dots,Y_{bm}}_{\text{batch }b}$$

### Batch means

The $i$th **batch mean** is the sample mean of the $m$ observations in batch $i=1,\dots,b$: $$\bar{Y}_{i,m} \equiv \frac{1}{m}\sum_{j=1}^m Y_{(i-1)m+j}$$

- Batch means are **correlated for small $m$** (adjacent batches "leak" correlation into each other near their boundary).
- But for **large $m$**: $$\bar{Y}_{1,m}, \dots, \bar{Y}_{b,m} \approx \text{i.i.d. } \mathrm{Nor}\big(\mu,\ \mathrm{Var}(\bar{Y}_{i,m})\big) \approx \mathrm{Nor}(\mu,\ \sigma^2/m)$$

> **The key trick, same spirit as IR:** batching converts one long dependent sequence into a small number of (approximately) i.i.d. summary statistics, which we can then treat with classical iid methods — just like IR converted dependence-within-a-run into iid-ness-across-runs.

---

## 4. The Batch Means Estimator of $\sigma^2$

Recall $\sigma^2 = \lim_{n\to\infty} n,\mathrm{Var}(\bar{Y}_n) = \lim_{m\to\infty} m,\mathrm{Var}(\bar{Y}_{1,m})$.

Define the **batch means estimator**: $$\hat{V}_B \equiv \frac{m}{b-1}\sum_{i=1}^b \big(\bar{Y}_{i,m} - \bar{Y}_n\big)^2 \ \approx\ \frac{\sigma^2 \chi^2(b-1)}{b-1}$$

(Note: this has essentially the same form as the IR variance estimator $S_Z^2$, just with batches in place of replications — hence "more similar to IR" than it might first appear, modulo notation.)

### Bias of $\hat{V}_B$

$$\mathrm{E}[\hat{V}_B] \doteq \frac{\sigma^2}{b-1}\mathrm{E}[\chi^2(b-1)] = \sigma^2$$

So $\hat{V}_B$ is **asymptotically unbiased** for $\sigma^2$ as the batch size $m \to \infty$.

---

## Summary / Mental Model

|Concept|Role|
|---|---|
|$\sigma^2 = \sum_{k=-\infty}^\infty R_k$|The fundamental variance parameter capturing long-run correlation structure|
|Batch means $\bar{Y}_{i,m}$|Chop one long run into $b$ chunks of $m$ obs each; for large $m$, chunks behave like iid normal draws|
|$\hat{V}_B$|Sample-variance-style estimator of $\sigma^2$ built from the batch means; asymptotically unbiased as $m\to\infty$|
|Contrast with IR|IR gets iid-ness by running many independent replications; BM gets (approximate) iid-ness by batching a single long run instead|

**Big picture:** Both AR(1) and MA(1) examples illustrate that $\sigma^2$ depends heavily on the correlation structure of the process — and can blow up as correlation strengthens. Batch means gives one practical way to actually estimate $\sigma^2$ from a single steady-state run, which then feeds into confidence intervals for $\mu$ (to be covered next).

## Open Threads / To Follow Up

- [ ] How to choose $b$ (number of batches) vs. $m$ (batch size) in practice
- [ ] Constructing the actual CI for $\mu$ using $\hat V_B$ (analogous to the IR CI, eq. 3 in earlier notes)
- [ ] Tests for whether batches are "large enough" to be treated as approximately independent
- [ ] Comparison of batch means vs. IR vs. other steady-state methods (spectral, regenerative, etc.)
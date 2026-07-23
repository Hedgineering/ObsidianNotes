#simulation #statistics #ranking-and-selection

# Ranking and Selection (R&S)

Previous module: [[Control Variates]] and other [[Variance Reduction Techniques]] wrapped up the VRT material. This begins a new topic: **ranking and selection methods**.

**Goal:** Give procedures for finding the best of several competing systems, with a **statistical guarantee** of being correct.

---

## Overview

**Ranking, selection, and multiple comparisons** methods are a class of statistical techniques used to compare alternative systems.

- The experimenter wants to select the **best** of a number ($\geq 2$) of competing processes.
- We specify a **desired probability of correctly selecting the best process** — especially important if the best process is significantly better than its competitors.
- These methods are simple to use, fairly general, and intuitively appealing (see Bechhofer, Santner, and Goldsman 1995).

---

## Why not just use ANOVA / simultaneous CIs?

For $>2$ systems, one option is simultaneous confidence intervals or ANOVA. But:

> These methods don't tell us much except that "at least one of the systems is different than the others" — which is no surprise.

They don't tell us **which** system is best, or give a clean statistical guarantee of correct selection. R&S methods are designed to fill that gap.

### What "best" means depends on the question

When comparing systems, different criteria may define "best":

- Which has the **biggest mean**?
- The **smallest variance**?
- The **highest probability** of yielding a success?
- The **lowest risk**?

---

## Scope of this module

The remainder of this module presents R&S procedures to find the best system **with respect to one parameter** (e.g., mean).

### Examples

|Nickname|Question|Underlying distribution|
|---|---|---|
|Great Expectations|Which of 10 fertilizers produces the largest mean crop yield?|Normal|
|Great Expectorants|Find the pain reliever with highest probability of relieving a cough|Binomial|
|Great Ex-Patriots|Who is the most popular former New England football player?|Multinomial|

---

## What R&S procedures do

**R&S** selects either:

- the single best system, **or**
- a subset of systems that is guaranteed to include the best (an "indifference-zone" or subset-selection approach)

Key features:

- **Guarantee a probability of correct selection** (a pre-specified confidence level, e.g. $P(CS) \geq P^*$).
- **Multiple Comparisons Procedures (MCPs)** add in certain confidence intervals for comparing systems pairwise or against the best.

---

## Relevance to simulation

R&S is especially useful in simulation contexts because simulation output often violates the "nice" iid normal assumptions of classical R&S theory — but we can engineer around this:

- **Normally distributed data** → achieved via **batching** (batch means to approximate normality).
- **Independence** → achieved by **controlling random numbers** (e.g., using separate/independent random number streams per system).
- **Multiple-stage sampling** → achieved by **retaining the seeds** across stages, so additional observations can be generated consistently if more sampling is needed.

---

## Summary

- R&S methods statistically guarantee correct identification of the best system (or a subset containing it), unlike ANOVA which only signals _that_ systems differ.
- The criterion for "best" (mean, variance, probability of success, etc.) must be chosen based on the application.
- In simulation, R&S relies on batching (for normality), controlled random numbers (for independence), and retained seeds (for multi-stage sampling).
- Next steps in this module: specific R&S procedures for finding the best system w.r.t. one parameter.
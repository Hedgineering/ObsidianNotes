## Key Takeaways

1. Experimental design is the process of creating a study that can identify causal relationships while minimizing bias, noise, and confounding effects.

2. The primary goal is to answer:

   "Did variable $X$ actually cause outcome $Y$?"

3. Good experimental design focuses on:

   - Randomization
   - Control groups
   - Replication
   - Statistical power
   - Avoiding bias
   - Proper measurement of outcomes

4. In quant research, poor experimental design often creates:

   - False alpha
   - Overfitting
   - Data snooping
   - Lookahead bias
   - Survivorship bias
   - Spurious correlations

5. Before running any test, always define:

   - Hypothesis
   - Population
   - Treatment
   - Outcome metric
   - Success criteria
   - Sample size
   - Analysis plan

6. A perfectly sophisticated statistical model cannot rescue a poorly designed experiment.

---

# Quick Reference Table

| Concept | Purpose | Key Idea |
|----------|----------|----------|
| Experimental Unit | What receives treatment | Person, stock, strategy, portfolio, etc. |
| Treatment | Intervention being tested | New strategy, signal, pricing model |
| Control Group | Baseline comparison | Existing strategy or no treatment |
| Randomization | Prevent bias | Assign treatments randomly |
| Replication | Reduce noise | Repeat observations |
| Blocking | Reduce variance | Compare similar groups |
| Confounding Variable | Hidden influence | Creates misleading conclusions |
| Statistical Power | Detect real effects | Probability of finding true signal |
| Type I Error | False positive | Detect effect that doesn't exist |
| Type II Error | False negative | Miss effect that exists |
| A/B Test | Compare two treatments | Industry standard experiment |
| Holdout Sample | Out-of-sample validation | Protect against overfitting |

---

# Intuition

Suppose someone claims:

> "Adding feature $X$ improves prediction accuracy."

How do we know?

Maybe:

- Feature $X$ truly helps.
- Market conditions changed.
- The training period was unusually favorable.
- Random chance produced a better result.

Experimental design attempts to separate:

$$
\text{Signal}
=
\text{True Effect}
$$

from

$$
\text{Noise}
=
\text{Random Variation}
$$

and

$$
\text{Bias}
=
\text{Systematic Error}
$$

---

# Why Experimental Design Matters

Without proper design:

- Statistical tests become unreliable.
- Conclusions become misleading.
- Capital may be allocated incorrectly.
- Research becomes non-reproducible.

A common saying:

> Garbage in, garbage out.

Even perfect statistical inference cannot fix a flawed experiment.

---

# Core Components of an Experiment

## Experimental Unit

The smallest entity receiving treatment.

Examples:

| Domain | Experimental Unit |
|----------|----------|
| Medicine | Patient |
| Marketing | Customer |
| Manufacturing | Product |
| Quant Finance | Trade, stock, portfolio, strategy |

---

## Treatment

The intervention being tested.

Examples:

- New trading signal
- New execution algorithm
- Alternative portfolio construction method
- New machine learning feature

Denote:

$$
T =
\begin{cases}
1 & \text{Treatment} \\
0 & \text{Control}
\end{cases}
$$

---

## Response Variable

The outcome being measured.

Examples:

- Accuracy
- Revenue
- Sharpe ratio
- Return
- Volatility
- Drawdown

Usually denoted

$$
Y
$$

---

## Control Group

Provides a baseline.

Instead of asking:

> "Did returns increase?"

we ask:

> "Did returns increase relative to what would have happened otherwise?"

Example:

| Group | Strategy |
|---------|---------|
| Control | Existing model |
| Treatment | New model |

---

# Randomization

## Intuition

Randomization prevents hidden variables from systematically favoring one treatment.

Instead of:

- Giving treatment to strongest stocks

we randomly assign treatment.

This helps ensure:

$$
\text{Treatment Group}
\approx
\text{Control Group}
$$

before treatment occurs.

---

## Why Randomization Works

Randomization balances:

- Known variables
- Unknown variables

across groups.

This dramatically reduces confounding.

---

# Replication

## Intuition

One observation proves almost nothing.

Repeated observations reduce noise.

Example:

Bad:

- Test strategy on one month

Good:

- Test across hundreds of months

---

## Effect on Variance

As sample size increases,

$$
\operatorname{Var}(\bar X)
=
\frac{\sigma^2}{n}
$$

Thus larger samples produce more stable estimates.

---

# Blocking

## Intuition

Sometimes groups naturally differ.

Instead of comparing everyone together, compare similar entities.

Example:

Compare stocks within:

- Sector
- Market cap bucket
- Region

rather than mixing everything together.

---

## Example

Bad:

Compare:

- Utilities
- Tech stocks

directly.

Better:

Evaluate treatment separately within sectors.

This removes unnecessary variability.

---

# Confounding Variables

## Definition

A confounder affects both:

$$
X
$$

and

$$
Y
$$

creating a misleading relationship.

---

## Example

Suppose:

$$
\text{Ice Cream Sales}
\uparrow
$$

and

$$
\text{Drowning Incidents}
\uparrow
$$

Does ice cream cause drowning?

No.

Hidden variable:

$$
\text{Temperature}
$$

causes both.

---

## Quant Example

Suppose:

$$
\text{Momentum Signal}
\rightarrow
\text{Higher Returns}
$$

But momentum exposure may simply coincide with:

- Bull markets
- Growth factor exposure
- Small-cap exposure

These are potential confounders.

---

# Biases to Avoid

## Selection Bias

Data is not representative.

Example:

Only testing stocks that survived.

---

## Survivorship Bias

Failed assets disappear from dataset.

Example:

Using only current S&P 500 members when backtesting 20 years.

This creates inflated performance.

---

## Lookahead Bias

Using future information.

Example:

Using earnings data before it was publicly released.

---

## Data Snooping Bias

Trying many ideas and only reporting winners.

Example:

Testing 10,000 signals and publishing the best one.

Some will appear significant by chance.

---

## Confirmation Bias

Seeking evidence supporting your hypothesis while ignoring contrary evidence.

---

# Hypothesis Formulation

A good experiment begins with a precise hypothesis.

---

## Null Hypothesis

Usually:

$$
H_0:
\text{No Effect}
$$

Example:

$$
H_0:
\mu_T = \mu_C
$$

---

## Alternative Hypothesis

Example:

$$
H_A:
\mu_T > \mu_C
$$

or

$$
H_A:
\mu_T \neq \mu_C
$$

---

# Statistical Power

## Definition

Probability of detecting a true effect.

$$
\text{Power}
=
1-\beta
$$

where

$$
\beta
=
P(\text{Type II Error})
$$

---

## Interpretation

Higher power means:

- Lower chance of missing real effects.
- More reliable experiments.

Typical target:

$$
80\%
\text{ to }
90\%
$$

power.

---

# Sample Size Considerations

Larger samples:

- Reduce variance
- Increase power
- Improve reliability

But cost more.

A key tradeoff:

$$
\text{Precision}
\leftrightarrow
\text{Cost}
$$

---

# A/B Testing

## Definition

Compare two treatments.

Group A:

$$
T=0
$$

Group B:

$$
T=1
$$

Measure:

$$
Y_A
$$

and

$$
Y_B
$$

Then test whether:

$$
Y_A
=
Y_B
$$

or

$$
Y_A
\neq
Y_B
$$

---

## Real-World Examples

### Tech

- Website redesign
- Recommendation algorithm
- Pricing page

### Quant Finance

- New execution algorithm
- New signal
- New risk model
- Alternative order routing

---

# Experimental Design in Quant Research

## The Central Problem

Quant researchers rarely perform true randomized experiments.

Markets are not laboratory environments.

Instead, we rely on:

- Historical backtests
- Simulations
- Natural experiments
- Paper trading
- Controlled deployments

---

# Research Workflow

## Step 1

Generate hypothesis.

Example:

> Earnings revisions predict future returns.

---

## Step 2

Define outcome.

Example:

$$
\text{Next 20-Day Return}
$$

---

## Step 3

Construct dataset.

Avoid:

- Lookahead bias
- Survivorship bias

---

## Step 4

Split Data

Typical:

Training:

$$
60\%-80\%
$$

Validation:

$$
10\%-20\%
$$

Test:

$$
10\%-20\%
$$

---

## Step 5

Perform Analysis

Examples:

- t-test
- regression
- bootstrap
- permutation test

---

## Step 6

Out-of-Sample Validation

Most important step.

A signal that only works in-sample is worthless.

---

# Holdout Sets

## Intuition

Keep part of data hidden until final evaluation.

This approximates future performance.

---

## Example

Training:

$$
2000-2018
$$

Validation:

$$
2019-2021
$$

Test:

$$
2022-2024
$$

Never repeatedly tune on the test set.

---

# Walk-Forward Testing

Especially important in finance.

Instead of:

$$
\text{Train}
\rightarrow
\text{Single Test}
$$

perform:

$$
\text{Train}
\rightarrow
\text{Test}
\rightarrow
\text{Roll Forward}
\rightarrow
\text{Repeat}
$$

This better mimics live trading.

---

# Example: Testing a New Alpha Signal

## Research Question

Does analyst revision score predict returns?

---

## Hypothesis

$$
H_0:
\mu = 0
$$

No predictive value.

$$
H_A:
\mu > 0
$$

Positive predictive value.

---

## Design

1. Rank stocks by revision score.

2. Form:

   - Top decile portfolio
   - Bottom decile portfolio

3. Compute:

$$
R_t
=
R_{Top}
-
R_{Bottom}
$$

4. Measure:

- Mean return
- Sharpe ratio
- Information ratio

5. Test significance.

---

## Potential Pitfalls

- Data revisions unavailable historically
- Survivorship bias
- Sector concentration
- Small sample size
- Overfitting parameters

---

# Causal Inference vs Prediction

Important distinction.

---

## Predictive Modeling

Goal:

$$
X
\rightarrow
Y
$$

accurately.

Causality not required.

Example:

Predict next-day return.

---

## Causal Inference

Goal:

$$
X
\text{ causes }
Y
$$

Much harder.

Requires stronger experimental design.

---

# Best Practices

## Before Running the Experiment

- Define hypothesis first.
- Define success metrics.
- Define significance threshold.
- Define sample size.
- Predefine analysis plan.

---

## During Analysis

- Keep holdout data untouched.
- Track all experiments.
- Avoid data snooping.
- Test robustness.

---

## After Results

- Replicate findings.
- Stress test assumptions.
- Evaluate out-of-sample.
- Evaluate under different regimes.

---

# Common Mistakes

1. Testing hundreds of ideas and reporting one winner.

2. Using future information.

3. Ignoring transaction costs.

4. Ignoring regime changes.

5. Looking only at Sharpe ratio.

6. Repeatedly reusing the test set.

7. Mistaking correlation for causation.

8. Small sample sizes.

9. Changing hypotheses after seeing results.

10. Ignoring economic intuition.

---

# Relationship to Other Topics

Experimental design works closely with:

- [[Hypothesis Testing]]
- [[Confidence Intervals]]
- [[Regression Analysis]]
- [[Law of Large Numbers]]
- [[Central Limit Theorem (CLT)]]
- [[Bayes Theorem]]
- [[Probability Axioms]]
- [[Law of the Unconscious Statistician (LOTUS)]]
- [[Bootstrap Methods]]
- [[Cross Validation]]

---

# Interview-Level Summary

Experimental design is the process of structuring a study so that observed effects can be attributed to a treatment rather than random chance, bias, or confounding variables. Good design relies on randomization, control groups, replication, sufficient statistical power, and rigorous out-of-sample validation. In quant finance, experimental design is particularly important because researchers are vulnerable to overfitting, lookahead bias, survivorship bias, and data snooping. The ultimate goal is to distinguish genuine alpha from noise and ensure results generalize to future market conditions.
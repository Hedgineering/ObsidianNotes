## Quick Reference Table

| Concept | Question Answered | Key Idea |
|----------|----------|----------|
| Correlation | Are two variables related? | Association only |
| Causation | Does changing $X$ change $Y$? | Intervention effect |
| Confounder | Common cause of treatment and outcome | Creates spurious relationships |
| Treatment | Variable being manipulated | Usually denoted $T$ |
| Outcome | Variable of interest | Usually denoted $Y$ |
| Counterfactual | What would have happened otherwise? | Fundamental causal concept |
| ATE | Average Treatment Effect | Population-level causal effect |
| ATT | Average Treatment Effect on Treated | Effect among treated individuals |
| Randomized Experiment | Gold standard for causality | Removes confounding |
| Observational Study | No random assignment | Requires adjustment |
| Instrumental Variables | Handles hidden confounding | Uses exogenous variation |
| Difference-in-Differences | Before/after comparison | Policy evaluation |
| Regression Discontinuity | Exploits cutoff rules | Quasi-experiment |
| Propensity Scores | Balances treatment groups | Mimics randomization |
| DAG | Directed Acyclic Graph | Visual causal model |

---

# Intuition

## Why Causal Inference Exists

Most machine learning answers:

> What happens together?

Causal inference tries to answer:

> What happens if we intervene?

Example:

Suppose we observe:

$$
\text{Ice Cream Sales}
\uparrow
$$

and

$$
\text{Drowning Incidents}
\uparrow
$$

Correlation exists.

But buying ice cream does not cause drowning.

The true causal factor is:

$$
\text{Temperature}
$$

which affects both.

This is the central challenge of causal inference.

---

# Correlation vs Causation

## Correlation

Measures association.

$$
\rho(X,Y)
=
\frac{\operatorname{Cov}(X,Y)}
{\sigma_X \sigma_Y}
$$

A large correlation does not imply causation.

---

## Causation

Changing $X$ causes a change in $Y$.

Symbolically:

$$
X
\rightarrow
Y
$$

If we intervene and force:

$$
X=x
$$

then:

$$
Y
$$

changes as a consequence.

---

# The Fundamental Problem of Causal Inference

For a single individual:

We can only observe one reality.

Suppose a patient receives treatment.

We observe:

$$
Y(1)
$$

where:

- $1$ means treated

We cannot simultaneously observe:

$$
Y(0)
$$

which represents the untreated outcome.

---

## Counterfactual Framework

Potential outcomes:

$$
Y(1)
=
\text{Outcome if treated}
$$

$$
Y(0)
=
\text{Outcome if untreated}
$$

True treatment effect:

$$
\tau
=
Y(1)-Y(0)
$$

Problem:

We never observe both.

---

# Average Treatment Effect (ATE)

Population-level causal effect:

$$
ATE
=
E[Y(1)-Y(0)]
$$

Equivalent:

$$
ATE
=
E[Y(1)]
-
E[Y(0)]
$$

---

## Example

Suppose:

Treatment group average:

$$
80
$$

Control group average:

$$
70
$$

Then:

$$
ATE
=
80-70
=
10
$$

Interpretation:

Treatment increases outcome by 10 units on average.

---

# Average Treatment Effect on the Treated (ATT)

Sometimes we only care about treated individuals.

$$
ATT
=
E[Y(1)-Y(0)\mid T=1]
$$

Used heavily in policy evaluation.

---

# Randomized Controlled Trials (RCTs)

## Gold Standard

Assign treatment randomly.

$$
T
\perp
(Y(0),Y(1))
$$

Meaning:

Treatment assignment is independent of potential outcomes.

---

## Why It Works

Randomization balances:

- Age
- Income
- Education
- Hidden factors

on average.

Therefore:

$$
ATE
=
E[Y\mid T=1]
-
E[Y\mid T=0]
$$

can be estimated directly.

---

# Confounding

## Definition

A confounder affects both:

- Treatment
- Outcome

---

## Example

Smoking and lung cancer.

Variables:

$$
T
=
\text{Smoking}
$$

$$
Y
=
\text{Cancer}
$$

Potential confounder:

$$
Z
=
\text{Genetic predisposition}
$$

---

## DAG

```text
      Z
     / \
    /   \
   v     v
   T --> Y
```

Ignoring $Z$ creates biased estimates.

---

# Directed Acyclic Graphs (DAGs)

## Purpose

Visualize causal assumptions.

Example:

```text
Education
     ↓
Income
     ↓
Spending
```

---

## Common Structures

### Chain

```text
X → Z → Y
```

Mediation.

---

### Fork

```text
    Z
   / \
  X   Y
```

Confounding.

---

### Collider

```text
X → Z ← Y
```

Important rule:

Do NOT condition on colliders.

Doing so creates bias.

---

# Backdoor Criterion

Goal:

Estimate causal effect:

$$
T
\rightarrow
Y
$$

Block all non-causal paths.

Example:

```text
Z → T → Y
 \      ^
  \    /
   → →
```

Adjust for:

$$
Z
$$

to eliminate confounding.

---

# Adjustment Using Regression

Common approach:

$$
Y
=
\beta_0
+
\beta_1T
+
\beta_2Z
+
\varepsilon
$$

where:

- $T$ = treatment
- $Z$ = confounder

Interpretation:

$$
\beta_1
$$

approximates causal effect.

Only valid if all confounders are included.

---

## Python Example

```python
import statsmodels.api as sm

X = df[["treatment","age","income"]]

X = sm.add_constant(X)

model = sm.OLS(
    df["outcome"],
    X
).fit()

print(model.summary())
```

---

# Selection Bias

Occurs when treatment assignment is not random.

Example:

People choosing to attend college differ from people who do not.

Observed difference:

$$
E[Y|T=1]
-
E[Y|T=0]
$$

contains:

- Treatment effect
- Selection effect

---

# Propensity Scores

## Motivation

Make treatment and control groups comparable.

---

## Definition

Probability of receiving treatment:

$$
e(X)
=
P(T=1|X)
$$

---

## Step 1

Estimate:

$$
P(T=1|X)
$$

usually using logistic regression.

---

## Step 2

Match similar individuals.

Example:

```text
Treated:
Age = 30

Control:
Age = 30
```

Compare outcomes.

---

## Python Example

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(X, treatment)

propensity = model.predict_proba(X)[:,1]
```

---

# Propensity Score Matching

Match units with similar scores.

Example:

```text
0.83 ↔ 0.82

0.45 ↔ 0.46

0.22 ↔ 0.23
```

Goal:

Create balanced groups.

---

# Inverse Probability Weighting (IPW)

Weight observations by:

$$
\frac{1}{e(X)}
$$

or

$$
\frac{1}{1-e(X)}
$$

depending on treatment status.

Creates a pseudo-population that resembles a randomized trial.

---

# Instrumental Variables (IV)

## Motivation

Unobserved confounding exists.

---

## Instrument Requirements

Instrument $Z$ must:

1. Affect treatment
2. Not directly affect outcome
3. Affect outcome only through treatment

---

## Example

Education studies:

$$
Z
=
\text{Distance to College}
$$

Influences:

$$
\text{Education}
$$

but ideally not earnings directly.

---

## DAG

```text
Z → T → Y
```

---

# Two-Stage Least Squares (2SLS)

Stage 1:

$$
T
=
\alpha_0
+
\alpha_1 Z
+
u
$$

Predict:

$$
\hat T
$$

---

Stage 2:

$$
Y
=
\beta_0
+
\beta_1 \hat T
+
\varepsilon
$$

---

# Difference-in-Differences (DiD)

## Motivation

Evaluate policies.

---

## Example

A state increases minimum wage.

Compare:

- Treated state
- Untreated state

Before and after.

---

## Formula

$$
DiD
=
(Y_{T,after}-Y_{T,before})
-
(Y_{C,after}-Y_{C,before})
$$

---

## Key Assumption

Parallel trends.

Without treatment:

Both groups would have evolved similarly.

---

# Regression Discontinuity Design (RDD)

## Motivation

Exploit cutoff rules.

---

## Example

Scholarship awarded if:

$$
\text{Score}
\ge 90
$$

Compare:

```text
89.9
vs
90.1
```

Students are nearly identical.

Difference estimates causal effect.

---

## Graph

```text
Outcome
   ^
   |
   |      /
   |     /
   |----|
   |   /
   |  /
   +------------>
         90
```

Jump at cutoff indicates treatment effect.

---

# Mediation

Sometimes treatment acts through an intermediate variable.

Example:

```text
Education
    ↓
Skills
    ↓
Income
```

Skills mediate the effect.

---

# Causal Machine Learning

Traditional causal methods estimate:

$$
ATE
$$

Modern methods estimate:

$$
\tau(X)
$$

where treatment effect varies by individual.

---

## Causal Forests

Extension of Random Forests.

Estimate:

$$
E[Y(1)-Y(0)|X]
$$

---

## Uplift Models

Estimate:

```text
Who benefits from treatment?
```

Applications:

- Marketing
- Medicine
- Recommendation systems

---

## Double Machine Learning (DML)

Uses ML to estimate nuisance functions.

Then estimates causal effect.

Useful when:

```text
Many covariates
High-dimensional data
```

---

# Common Assumptions

## Consistency

Observed outcome equals corresponding potential outcome.

---

## Exchangeability

No hidden confounding.

$$
(Y(0),Y(1))
\perp
T
\mid
X
$$

---

## Positivity

Every individual could potentially receive either treatment.

$$
0
<
P(T=1|X)
<
1
$$

---

## SUTVA

Stable Unit Treatment Value Assumption.

One person's treatment does not affect another's outcome.

---

# Common Pitfalls

## Correlation ≠ Causation

Most important rule.

---

## Missing Confounders

Regression cannot fix omitted variables.

---

## Conditioning on Colliders

Can introduce bias.

---

## Using ML Prediction Models as Causal Models

High predictive accuracy does NOT imply causal validity.

Example:

A model predicting:

$$
Y
=
f(X)
$$

may be excellent for prediction while being completely wrong about interventions.

---

## Extrapolating Beyond Observed Data

Causal estimates are often local.

Avoid assuming they apply universally.

---

# Real-World Applications

## Economics

- Education returns
- Tax policy
- Labor economics

---

## Medicine

- Drug effectiveness
- Clinical trials
- Treatment recommendation

---

## Marketing

- Ad effectiveness
- Customer targeting
- Campaign optimization

---

## Technology

- A/B testing
- Recommendation systems
- Product experiments

---

## Quantitative Finance

Examples:

```text
Does a signal cause future returns?

Does an execution strategy improve fills?

Does news sentiment cause price movement?
```

Requires careful separation of causality from correlation.

---

# Relationship to Machine Learning

| Machine Learning | Causal Inference |
|----------|----------|
| Predict $Y$ | Estimate effect of intervention |
| Focus on accuracy | Focus on causal validity |
| Correlations sufficient | Requires causal assumptions |
| Uses train/test error | Uses identification strategies |
| Answers "What will happen?" | Answers "What if we change something?" |

# Key Takeaways

- Causal inference studies the effect of interventions, not merely associations.
- The fundamental challenge is that counterfactual outcomes are unobservable.
- Randomized experiments are the gold standard for causal estimation.
- Observational studies require adjustment for confounding.
- DAGs help reason about causal assumptions and bias.
- Propensity scores, IVs, DiD, and RDD are the most commonly used observational causal methods.
- Modern causal ML extends classical methods to estimate heterogeneous treatment effects.
- A highly predictive model is not necessarily a causal model.
- Every causal conclusion depends on assumptions; understanding those assumptions is often more important than the estimation method itself.
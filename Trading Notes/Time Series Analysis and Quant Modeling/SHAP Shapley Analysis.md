# SHAP Analysis for Quant Finance Models and Strategies

## Quick Reference Table

| Concept | Definition | Why It Matters |
|---|---|---|
| SHAP | Shapley Additive Explanations | Explain model predictions |
| Shapley Value | Fair contribution of a feature to prediction | Attribution mechanism |
| Global SHAP | Average feature importance across dataset | Understand strategy drivers |
| Local SHAP | Feature contribution for one prediction | Explain individual trades |
| SHAP Interaction | Contribution from feature interactions | Detect nonlinear relationships |
| Feature Attribution | How much each feature contributes | Model debugging |
| Base Value | Average prediction before features | Reference point |
| SHAP Value | Marginal contribution of a feature | Incremental effect |
| Model Explainability | Understanding why model behaves as it does | Validation and trust |
| Feature Leakage | Future information accidentally used | Common failure mode |
| Concept Drift | Feature relationship changes over time | Alpha decay detection |
| Regime Dependency | Features behave differently in different environments | Strategy robustness |
| Stability Analysis | Check SHAP consistency over time | Production monitoring |

---

# Big Picture

SHAP is one of the most important model debugging tools used in modern quantitative finance.

The central question:

> Why did the model make this prediction?

Suppose a model predicts:

$$
E[r_{t+1}]
=
0.75\%
$$

for a stock.

SHAP attempts to answer:

- Which features contributed?
- By how much?
- Did the prediction come from sensible factors?
- Is the model relying on noise?
- Is the model using a leaking feature?

---

# Why SHAP Exists

Many modern models are "black boxes".

Examples:

- XGBoost
- LightGBM
- Random Forests
- Neural Networks

These models often outperform linear models but can be difficult to interpret.

SHAP provides:

$$
\text{Prediction}
=
\text{Baseline}
+
\sum_i \text{Feature Contributions}
$$

---

# Origin: Cooperative Game Theory

SHAP comes from the concept of Shapley Values.

Imagine:

5 people cooperate to earn:

$$
\$100
$$

Question:

How should the:

$$
\$100
$$

be divided fairly?

Each person contributed differently.

Shapley values allocate the reward based on marginal contribution.

---

# Translating to Machine Learning

Instead of players:

We have features.

Example:

| Feature |
|----------|
| Momentum |
| Volatility |
| Market Beta |
| Earnings Revision |
| Volume |

Instead of splitting money:

We split model prediction.

---

# Core SHAP Equation

Prediction:

$$
f(x)
=
\phi_0
+
\sum_{i=1}^{n}
\phi_i
$$

where:

- $\phi_0$ = baseline prediction
- $\phi_i$ = SHAP contribution of feature $i$

---

# Example

Suppose:

Baseline return prediction:

$$
0.10\%
$$

Contributions:

Momentum:

$$
+0.25\%
$$

Volatility:

$$
-0.05\%
$$

Earnings Revision:

$$
+0.15\%
$$

Prediction:

$$
0.10
+
0.25
-
0.05
+
0.15
$$

$$
=
0.45\%
$$

---

# Local SHAP

Local SHAP explains one prediction.

Example:

Why did model buy:

AAPL

today?

SHAP might show:

| Feature | Contribution |
|-----------|-------------|
| Momentum | +0.30 |
| Relative Volume | +0.15 |
| Earnings Revision | +0.12 |
| Volatility Spike | -0.08 |

Final prediction:

$$
+0.49
$$

---

# Global SHAP

Global SHAP explains the entire model.

Question:

Which factors drive predictions overall?

Example:

| Feature | Avg Absolute SHAP |
|----------|-------------------|
| Momentum | 0.35 |
| Volatility | 0.22 |
| Earnings Revision | 0.18 |
| Beta | 0.10 |

This indicates momentum is the dominant signal.

---

# Why Quant Researchers Love SHAP

Backtests tell you:

$$
\text{What happened}
$$

SHAP tells you:

$$
\text{Why it happened}
$$

This distinction is extremely important.

---

# Example: Model Validation

Suppose model uses:

- Momentum
- Value
- Volatility
- Earnings Revisions

Research hypothesis:

Momentum should dominate.

SHAP shows:

| Feature | SHAP Importance |
|----------|----------------|
| Random Feature | 0.45 |
| Momentum | 0.12 |
| Value | 0.08 |

Something is wrong.

The model is likely overfitting.

---

# SHAP vs Feature Importance

Tree models often provide feature importance.

Example:

```python
model.feature_importances_
```

Problem:

Feature importance does not explain:

- Direction
- Magnitude
- Local predictions

SHAP does.

---

# Example

Feature importance:

| Feature | Importance |
|-----------|-----------|
| Momentum | 50 |
| Volatility | 30 |

Does not tell us:

- Does momentum increase returns?
- Decrease returns?
- Under what conditions?

SHAP does.

---

# SHAP Directionality

SHAP shows:

- Importance
- Sign

Example:

Momentum SHAP:

Positive momentum:

$$
\Rightarrow
$$

positive predictions.

Negative momentum:

$$
\Rightarrow
$$

negative predictions.

This confirms economic intuition.

---

# Quant Strategy Development Workflow

Typical process:

$$
\text{Hypothesis}
\rightarrow
\text{Feature Engineering}
\rightarrow
\text{Model}
\rightarrow
\text{Backtest}
\rightarrow
\text{SHAP Analysis}
\rightarrow
\text{Iteration}
$$

---

# Feature Discovery

SHAP often reveals unexpected alpha.

Example:

Researcher expects:

Momentum.

SHAP reveals:

Short-term volatility carries most predictive power.

This generates a new hypothesis.

---

# Leakage Detection

One of the most valuable uses.

Suppose:

Model achieves:

$$
R^2 = 0.60
$$

on financial returns.

This is suspiciously high.

SHAP reveals:

Feature:

```
next_day_close
```

dominates.

Future information leaked into training.

---

# Example: Earnings Leakage

Feature:

```
earnings_surprise_t+1
```

appears highly important.

Impossible in production.

SHAP quickly exposes the issue.

---

# Regime Analysis

A feature may work differently across market regimes.

Example:

Momentum SHAP:

| Regime | Average SHAP |
|----------|------------|
| Bull Market | +0.35 |
| Bear Market | -0.10 |

Now we know:

Strategy behavior changes with regime.

---

# SHAP Through Time

A common quant analysis:

Compute monthly SHAP importance.

Track:

$$
SHAP_t
$$

over time.

---

# Example

Momentum importance:

| Year | SHAP |
|--------|------|
| 2020 | 0.40 |
| 2021 | 0.32 |
| 2022 | 0.18 |
| 2023 | 0.10 |

Alpha appears to be decaying.

---

# Alpha Decay Detection

A signal may remain in the model but contribute less over time.

SHAP often identifies:

- Crowding
- Regime changes
- Feature degradation

before performance visibly deteriorates.

---

# Cross-Sectional Equity Models

Common features:

- Momentum
- Value
- Quality
- Volatility
- Earnings revisions
- Analyst estimates

SHAP can determine:

Which factor actually drives predictions.

---

# Example

Prediction:

$$
+1.2\%
$$

SHAP decomposition:

| Factor | SHAP |
|----------|------|
| Momentum | +0.50 |
| Quality | +0.30 |
| Value | +0.25 |
| Beta | -0.05 |
| Volatility | +0.20 |

Now prediction becomes interpretable.

---

# SHAP for XGBoost and LightGBM

Most common use case in quant research.

Reason:

Tree-based models have fast exact SHAP algorithms.

Supported directly.

Example:

```python
import shap

explainer = shap.TreeExplainer(model)

shap_values = explainer.shap_values(X)
```

---

# SHAP Summary Plot

Most common visualization.

Each point:

One observation.

Color:

Feature value.

X-axis:

SHAP contribution.

Shows:

- Importance
- Direction
- Nonlinearity

simultaneously.

---

# Interpreting SHAP Summary Plots

Example:

Momentum feature.

Red points:

High momentum.

Located on positive side.

Interpretation:

Higher momentum increases predictions.

Exactly what we expect.

---

# Partial Dependence vs SHAP

Partial Dependence:

Shows average model response.

SHAP:

Shows actual contributions.

SHAP usually provides more detailed explanations.

---

# SHAP Interaction Values

Very important in finance.

Question:

Do features interact?

Example:

Momentum and volatility.

Maybe:

Momentum only works when volatility is low.

SHAP interactions reveal this.

---

# Example

Model behavior:

High Momentum

AND

Low Volatility

creates strongest alpha.

Interaction value becomes large.

---

# Nonlinear Discovery

Suppose:

Returns increase with momentum up to:

$$
z=2
$$

Then flatten.

SHAP reveals this nonlinear relationship automatically.

---

# Risk Model Validation

Suppose risk model predicts:

$$
\sigma_{t+1}
$$

SHAP may show:

| Feature | SHAP |
|----------|------|
| Realized Vol | Large |
| VIX | Medium |
| Market Return | Small |

This confirms intuitive behavior.

---

# Credit Models

Credit prediction:

$$
P(\text{Default})
$$

SHAP often explains:

- Debt ratio
- Interest coverage
- Cash flow
- Credit spread

contributions.

Useful for model governance.

---

# Portfolio Construction

SHAP can explain:

Why optimizer prefers certain assets.

Example:

Expected return model:

$$
\hat r_i
$$

drives portfolio weights.

SHAP explains:

Why:

$$
\hat r_i
$$

is high.

---

# Factor Investing

SHAP often used to compare:

Traditional factors:

- Value
- Momentum
- Quality

against ML features.

Question:

Is ML discovering genuinely new information?

Or merely replicating known factors?

---

# SHAP Stability Analysis

Critical production check.

Question:

Do feature contributions remain stable?

Example:

Compute:

$$
E[|\phi_i|]
$$

monthly.

Large shifts indicate:

- Drift
- Regime change
- Data issue

---

# Production Monitoring

Many firms monitor:

- Feature distributions
- SHAP distributions
- Prediction distributions

simultaneously.

---

# Example

Suddenly:

Volume SHAP importance:

$$
0.05
\rightarrow
0.80
$$

Likely indicates:

- Data bug
- Regime shift
- Pipeline issue

---

# SHAP and Model Simplification

Suppose:

100 features.

SHAP shows:

Only:

10

matter.

Researchers may remove:

90

features.

Benefits:

- Faster training
- Better interpretability
- Lower overfitting

---

# Common Quant Workflow

## Step 1

Train model.

---

## Step 2

Compute SHAP.

---

## Step 3

Validate feature contributions.

---

## Step 4

Remove suspicious features.

---

## Step 5

Retrain.

---

## Step 6

Compare performance and stability.

---

# Common Python Workflow

## Compute SHAP

```python
import shap

explainer = shap.TreeExplainer(model)

shap_values = explainer.shap_values(X)
```

---

## Summary Plot

```python
shap.summary_plot(
    shap_values,
    X
)
```

---

## Dependence Plot

```python
shap.dependence_plot(
    "momentum",
    shap_values,
    X
)
```

---

## Force Plot

Explains individual predictions.

```python
shap.force_plot(
    explainer.expected_value,
    shap_values[0],
    X.iloc[0]
)
```

---

# Worked Example 1

Model prediction:

$$
0.70\%
$$

Baseline:

$$
0.10\%
$$

SHAP:

Momentum:

$$
+0.30
$$

Value:

$$
+0.15
$$

Volatility:

$$
-0.05
$$

Quality:

$$
+0.20
$$

Check:

$$
0.10
+
0.30
+
0.15
-
0.05
+
0.20
$$

$$
=
0.70
$$

Prediction explained.

---

# Worked Example 2

Monthly SHAP importance:

| Month | Momentum |
|---------|----------|
| Jan | 0.35 |
| Feb | 0.34 |
| Mar | 0.33 |
| Apr | 0.32 |
| May | 0.10 |

Interpretation:

Potential:

- Regime shift
- Data issue
- Alpha decay

Requires investigation.

---

# Worked Example 3

Model:

$$
R^2=0.50
$$

SHAP:

| Feature | Importance |
|----------|------------|
| Future Return | 0.80 |
| Momentum | 0.05 |
| Value | 0.04 |

Interpretation:

Feature leakage.

Model is invalid.

---

# Common Interview Questions

## Why use SHAP instead of feature importance?

SHAP provides:

- Magnitude
- Direction
- Local explanations
- Global explanations

while traditional importance often only measures usage frequency.

---

## How would SHAP help debug a strategy?

It can identify:

- Overfitting
- Leakage
- Regime dependence
- Unexpected drivers
- Alpha decay

---

## Why might SHAP values change over time?

Possible causes:

- Regime shifts
- Crowding
- Data drift
- Structural market changes
- Broken pipelines

---

## What does a negative SHAP value mean?

The feature decreases the prediction relative to baseline.

---

## What does a large SHAP value mean?

The feature strongly influences the prediction.

---

# Common Mistakes

## Mistake 1: Treating SHAP as Causality

SHAP explains model behavior.

It does not prove causation.

---

## Mistake 2: Ignoring Regimes

Feature importance often changes across market environments.

---

## Mistake 3: Looking Only at Global SHAP

Global importance can hide important local effects.

---

## Mistake 4: Ignoring Interactions

Many finance relationships are nonlinear.

Interaction effects matter.

---

## Mistake 5: Assuming Stable Importance

Alpha factors often decay.

SHAP should be monitored through time.

---

# Key Takeaways

- SHAP is one of the most important tools for understanding modern ML models in quantitative finance.
- It decomposes predictions into feature contributions using Shapley values from game theory.
- Global SHAP identifies the most important drivers of a strategy.
- Local SHAP explains individual predictions and trades.
- SHAP is extremely useful for detecting leakage, overfitting, alpha decay, regime dependence, and data quality problems.
- Time-series monitoring of SHAP values is often as important as monitoring returns.
- SHAP interaction values reveal nonlinear relationships and factor interactions.
- In production quant research, SHAP is commonly used after every major model iteration to validate that the model is learning economically sensible relationships.
- A model that performs well but has nonsensical SHAP explanations should be viewed with suspicion.
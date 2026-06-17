## Quick Reference Table

| Situation | Recommended Starting Models | Notes |
|------------|------------|------------|
| Small Regression Problem | Linear Regression, Ridge, Lasso | Fast, interpretable |
| Small Classification Problem | Logistic Regression | Strong baseline |
| Tabular Regression | LightGBM, XGBoost, Random Forest | Industry standard |
| Tabular Classification | LightGBM, XGBoost, Random Forest | Usually strongest baseline |
| High Interpretability Required | Linear Models, Decision Trees | Easier to explain |
| High-Dimensional Sparse Data | Lasso, Elastic Net, Linear Models | Text, NLP |
| Image Data | CNN, Vision Transformer | Deep learning dominates |
| Text Data | Transformer Models | BERT, GPT, T5 |
| Time Series Forecasting | ARIMA, SARIMA, LightGBM, LSTM | Depends on data size |
| Clustering | K-Means, DBSCAN, GMM | Depends on cluster shape |
| Anomaly Detection | Isolation Forest, One-Class SVM | Rare-event detection |
| Small Dataset | Simpler models | Avoid deep learning |
| Massive Dataset | Boosting, Deep Learning | More complex models become feasible |

---

# What is Model Selection?

Model selection is the process of choosing the most appropriate model for a problem.

Goal:

$$
\text{Best Generalization Performance}
$$

not

$$
\text{Lowest Training Error}
$$

A more complex model is not necessarily better.

---

# Intuition

Suppose we want to predict house prices.

Possible models:

```text
Linear Regression
Random Forest
XGBoost
Neural Network
```

Each model makes different assumptions.

Model selection determines:

```text
Which model best balances:

Bias
Variance
Interpretability
Complexity
Compute Cost
Business Requirements
```

---

# The Model Selection Process

```text
Understand Problem
        ↓
Build Baseline
        ↓
Evaluate
        ↓
Compare Models
        ↓
Cross Validation
        ↓
Hyperparameter Tuning
        ↓
Final Model
```

---

# Step 1: Understand the Problem

Before choosing a model, answer:

## What is the target?

Regression:

$$
y \in \mathbb{R}
$$

Examples:

- Price
- Revenue
- Temperature

---

Classification:

$$
y \in \{0,1\}
$$

Examples:

- Fraud
- Disease
- Churn

---

Multi-class:

$$
y \in \{1,2,\ldots,K\}
$$

Examples:

- Image categories
- Product categories

---

## What is the data type?

| Data Type | Typical Models |
|------------|------------|
| Tabular | XGBoost, RF, Linear Models |
| Images | CNN, ViT |
| Text | Transformers |
| Time Series | ARIMA, LightGBM, LSTM |
| Graphs | GNNs |
| Audio | CNNs, Transformers |

---

# Step 2: Start with a Baseline

Always start simple.

Examples:

Regression:

```python
from sklearn.linear_model import LinearRegression
```

Classification:

```python
from sklearn.linear_model import LogisticRegression
```

---

Why?

Provides a benchmark.

Future models must outperform it.

---

# The Bias-Variance Tradeoff

Recall:

$$
\text{Error}
=
\text{Bias}^2
+
\text{Variance}
+
\text{Noise}
$$

---

# High Bias Models

Examples:

- Linear Regression
- Logistic Regression

Characteristics:

```text
Simple
Stable
May underfit
```

---

# High Variance Models

Examples:

- Deep Trees
- Neural Networks

Characteristics:

```text
Flexible
Can overfit
```

---

# Goal

Find balance.

```text
Not Too Simple
Not Too Complex
```

---

# Common Model Families

# Linear Models

Examples:

- Linear Regression
- Ridge
- Lasso
- Elastic Net

---

## Strengths

- Fast
- Interpretable
- Stable

---

## Weaknesses

- Linear assumptions
- Limited flexibility

---

## Best When

```text
Small datasets
Interpretability important
Relationships mostly linear
```

---

# Tree-Based Models

Examples:

- Decision Trees
- Random Forest
- Extra Trees

---

## Strengths

- Capture nonlinearities
- Handle interactions automatically
- Minimal preprocessing

---

## Weaknesses

- Less interpretable
- Can overfit

---

## Best When

```text
Tabular data
Mixed feature types
```

---

# Gradient Boosted Trees

Examples:

- XGBoost
- LightGBM
- CatBoost

---

## Strengths

Often best tabular-data performance.

Handles:

```text
Missing values
Nonlinear relationships
Feature interactions
```

---

## Weaknesses

- More tuning required
- Slower than linear models

---

## Best When

```text
Structured data
Competitions
Production ML
```

---

# Support Vector Machines

Objective:

Maximize margin.

---

## Strengths

Works well for:

```text
Medium-sized datasets
High-dimensional spaces
```

---

## Weaknesses

- Doesn't scale well
- Computationally expensive

---

## Best When

```text
Thousands of rows
Not millions
```

---

# Neural Networks

General function approximators.

---

## Strengths

Capture complex nonlinear relationships.

---

## Weaknesses

Require:

```text
Large datasets
More compute
More tuning
```

---

## Best When

```text
Images
Text
Speech
Massive datasets
```

---

# Time Series Models

Examples:

- ARIMA
- SARIMA
- VAR
- Prophet
- LightGBM
- LSTM

---

## Best When

Order matters.

$$
X_t
\neq
X_{\text{random}}
$$

---

# Choosing Based on Dataset Size

## Small Data

Example:

```text
500 rows
```

Start with:

- Linear Models
- Trees

Avoid:

- Deep Neural Networks

---

## Medium Data

Example:

```text
10,000 - 100,000 rows
```

Good choices:

- Random Forest
- XGBoost
- LightGBM

---

## Large Data

Example:

```text
Millions of rows
```

Consider:

- LightGBM
- Deep Learning
- Ensembles

---

# Choosing Based on Interpretability

## High Interpretability Needed

Examples:

- Healthcare
- Finance
- Regulation

Choose:

```text
Linear Models
Small Trees
Generalized Additive Models
```

---

## Low Interpretability Needed

Choose:

```text
XGBoost
Deep Learning
Ensembles
```

---

# Choosing Based on Speed

## Fast Training

Examples:

```text
Linear Regression
Logistic Regression
Naive Bayes
```

---

## Moderate

Examples:

```text
Random Forest
LightGBM
XGBoost
```

---

## Slow

Examples:

```text
Deep Neural Networks
Large Transformers
```

---

# Evaluation Metrics

Model selection depends on the metric.

---

# Regression

## MSE

$$
MSE
=
\frac1n
\sum
(y_i-\hat y_i)^2
$$

---

## RMSE

$$
RMSE
=
\sqrt{MSE}
$$

Most common.

---

## MAE

$$
MAE
=
\frac1n
\sum
|y_i-\hat y_i|
$$

For Obsidian tables:

$$
|x|
=
\lvert x \rvert
$$

---

## R-Squared

$$
R^2
=
1
-
\frac{RSS}{TSS}
$$

---

# Classification

## Accuracy

$$
\frac{TP+TN}
{N}
$$

---

## Precision

$$
\frac{TP}
{TP+FP}
$$

---

## Recall

$$
\frac{TP}
{TP+FN}
$$

---

## F1 Score

$$
F1
=
2
\cdot
\frac{
\text{Precision}
\cdot
\text{Recall}
}
{
\text{Precision}
+
\text{Recall}
}
$$

---

## ROC-AUC

Measures ranking quality.

Popular for:

```text
Imbalanced classification
```

---

# Cross Validation for Model Selection

## Why?

A single train-test split may be misleading.

---

## K-Fold

Evaluate:

$$
M_1
$$

$$
M_2
$$

$$
M_3
$$

across multiple folds.

---

## Python Example

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    model,
    X,
    y,
    cv=5
)
```

---

# Comparing Models Properly

Incorrect:

```text
Model A:
Split #1

Model B:
Split #2
```

Different data.

Unfair comparison.

---

Correct:

```text
Same train split
Same validation split
Same metric
```

for every model.

---

# Statistical Comparison

Suppose:

| Model | RMSE |
|---------|---------|
| XGBoost | 10.2 |
| LightGBM | 10.1 |

Difference:

```text
0.1
```

May not be meaningful.

Cross-validation helps determine stability.

---

# Model Selection Using Learning Curves

Plot:

$$
\text{Training Error}
$$

and

$$
\text{Validation Error}
$$

---

## High Bias

```text
Train Error High
Validation Error High
```

Need more flexible model.

---

## High Variance

```text
Train Error Low
Validation Error High
```

Need regularization.

---

# Ensemble Model Selection

Sometimes best model is:

```text
Not One Model
```

but:

```text
Combination of Models
```

---

# Bagging

Example:

```text
Random Forest
```

Reduces variance.

---

# Boosting

Examples:

```text
XGBoost
LightGBM
CatBoost
```

Reduces bias.

---

# Stacking

Example:

```text
Linear Model
Random Forest
XGBoost
        ↓
Meta Model
```

Often used in competitions.

---

# Time Series Model Selection

# Small Univariate Series

Try:

```text
ARIMA
SARIMA
ETS
```

---

# Tabular Forecasting

Try:

```text
LightGBM
XGBoost
```

with:

- Lag features
- Rolling statistics
- Calendar features

---

# Long Sequences

Try:

```text
LSTM
GRU
Transformers
```

---

# Industry Best Practices

# Start Simple

Always establish baseline:

```text
Linear Regression
Logistic Regression
```

---

# Strong Tabular Baseline

Most production teams test:

```text
Random Forest
LightGBM
XGBoost
```

early.

---

# Optimize Features Before Models

Often:

```text
Better Features
>
Better Models
```

---

# Compare Multiple Families

Don't compare:

```text
XGBoost
vs
LightGBM
```

only.

Also compare:

```text
Linear
Tree
Boosting
```

families.

---

# Use Cross Validation

Especially:

```text
Dataset < 100,000 rows
```

---

# Use Time-Aware Validation

For forecasting:

```python
TimeSeriesSplit
```

not random K-Fold.

---

# Track Experiments

Record:

```text
Model
Hyperparameters
Features
Metric
Dataset Version
```

Tools:

- MLflow
- Weights & Biases
- Neptune

---

# Avoid Test Set Leakage

Never repeatedly evaluate on:

```text
Test Set
```

during selection.

---

# Common Mistakes

## Choosing the Most Complex Model

Complexity ≠ performance.

---

## Ignoring Business Constraints

Fast model may be preferable.

---

## Ignoring Interpretability

Important in:

- Healthcare
- Banking
- Insurance

---

## Comparing Models on Different Splits

Creates misleading conclusions.

---

## Optimizing Wrong Metric

Example:

```text
Maximize Accuracy
```

when:

```text
Recall
```

actually matters.

---

# Real-World Model Selection Workflow

```text
Understand Problem
        ↓
Create Baseline
        ↓
Feature Engineering
        ↓
Train Several Model Families
        ↓
Cross Validation
        ↓
Hyperparameter Tuning
        ↓
Compare Metrics
        ↓
Check Interpretability
        ↓
Evaluate on Holdout Test Set
        ↓
Deploy
```

# Industry Model Selection Hierarchy

For tabular data, many teams effectively follow:

```text
Linear Model
      ↓
Random Forest
      ↓
LightGBM / XGBoost
      ↓
Ensemble
      ↓
Deep Learning
```

Only move to the next step if meaningful improvement exists.

# Key Takeaways

- Model selection is the process of choosing the model that generalizes best.
- Start with simple baselines before complex models.
- Compare model families, not just hyperparameters within one family.
- Use cross-validation for reliable comparisons.
- Select models using the metric that matches business objectives.
- Feature engineering often matters more than model choice.
- The best model balances accuracy, interpretability, latency, maintainability, and cost.
- For tabular data, gradient boosted trees are often the strongest baseline.
- For images and text, deep learning usually dominates.
- Never use the test set to choose a model.
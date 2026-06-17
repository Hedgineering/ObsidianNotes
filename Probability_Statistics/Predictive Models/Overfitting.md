# Overfitting

## Quick Reference Table

| Concept | Description | Typical Symptoms | Common Fixes |
|----------|----------|----------|----------|
| Overfitting | Model learns noise instead of signal | Excellent train performance, poor validation performance | Regularization, more data, simpler model |
| Underfitting | Model too simple to learn signal | Poor train and validation performance | More complex model, better features |
| High Variance | Sensitive to training data | Large train-validation gap | Regularization, bagging |
| High Bias | Model assumptions too restrictive | High error everywhere | More flexible model |
| Data Leakage | Model sees future information | Unrealistically good validation results | Fix pipeline |
| Early Stopping | Stop training before memorization | Prevents validation deterioration | Common in boosting and deep learning |
| Cross Validation | Better estimate of generalization | More stable model selection | Industry standard |

---

# Intuition

## What is Overfitting?

Overfitting occurs when a model learns:

$$
\text{Signal}
+
\text{Noise}
$$

instead of learning only:

$$
\text{Signal}
$$

The model performs well on data it has already seen but poorly on new data.

---

## Example

Suppose the true relationship is:

$$
y = x^2 + \varepsilon
$$

where:

$$
\varepsilon
=
\text{random noise}
$$

A good model learns:

$$
y \approx x^2
$$

An overfit model learns:

$$
y
=
x^2
+
\text{every random fluctuation}
$$

which will not generalize.

---

# Visual Intuition

## Underfit

```text
Data: Curved

Model: Straight Line
```

Misses important structure.

---

## Good Fit

```text
Captures true pattern
```

---

## Overfit

```text
Wiggles through every point
```

Memorizes noise.

---

# Formal Perspective

Recall the [[Bias Variance Tradeoff]]:

$$
\text{Expected Error}
=
\text{Bias}^2
+
\text{Variance}
+
\text{Noise}
$$

Overfitting corresponds to:

$$
\text{Variance}
\uparrow
$$

The model becomes highly sensitive to small changes in training data.

---

# The Most Common Symptom

Training performance:

```text
Excellent
```

Validation performance:

```text
Poor
```

---

## Example

| Metric | Train | Validation |
|----------|----------|----------|
| Accuracy | 99% | 70% |
| RMSE | 1.2 | 8.5 |
| R² | 0.99 | 0.45 |

Classic overfitting.

---

# Learning Curves

## Ideal

```text
Training Error ↓

Validation Error ↓

Small Gap
```

---

## Overfitting

```text
Training Error ↓↓↓

Validation Error ↓ then ↑
```

---

## Underfitting

```text
Training Error High

Validation Error High
```

---

# Why Overfitting Happens

# Model Too Flexible

Examples:

```text
Very Deep Tree
Large Neural Network
Huge Number of Features
```

Model capacity exceeds available information.

---

# Too Little Data

Example:

```text
100 samples
10,000 features
```

Model can memorize observations.

---

# Noisy Features

Many variables contain:

```text
Random fluctuations
```

instead of useful signal.

---

# Data Leakage

One of the most common causes.

Model accidentally sees future information.

Example:

```text
Predicting customer churn
```

using:

```text
Cancellation Date
```

Produces unrealistic performance.

---

# Excessive Hyperparameter Tuning

Repeatedly optimizing against the same validation set eventually overfits the validation set.

---

# Diagnosing Overfitting

# Step 1: Compare Train and Validation Metrics

Example:

```python
train_rmse
valid_rmse
```

Large gap:

```text
Potential Overfitting
```

---

# Step 2: Plot Learning Curves

```python
from sklearn.model_selection import learning_curve
```

Look for:

```text
Train ↓↓↓

Validation Flat
```

or

```text
Train ↓↓↓

Validation ↑
```

---

# Step 3: Inspect Feature Importance

Ask:

```text
Is the model relying on weird features?
```

Use:

```python
SHAP
Feature Importance
Permutation Importance
```

---

# Step 4: Check for Leakage

Always ask:

```text
Would this feature exist at prediction time?
```

If not:

```text
Leakage
```

---

# What To Do When You Detect Overfitting

# Step 1: Verify Data Leakage

Before changing the model:

Check:

- Future information
- Label leakage
- Target leakage
- Train-test contamination

Many apparent overfitting problems are actually leakage problems.

---

# Step 2: Examine Feature Engineering

Often:

```text
Bad Features
=
Overfitting
```

Ask:

```text
Are features meaningful?
```

Remove:

- Redundant features
- Suspicious features
- Leakage features

---

# Step 3: Simplify the Model

Example:

Instead of:

```text
Depth = 50
```

try:

```text
Depth = 5
```

---

# Step 4: Add Regularization

Examples:

Linear Models:

- Ridge
- Lasso
- Elastic Net

Neural Networks:

- Weight decay
- Dropout

Boosting:

- Stronger regularization
- Smaller trees

---

# Step 5: Gather More Data

One of the most effective solutions.

More observations make memorization harder.

---

# Step 6: Improve Validation Procedure

Sometimes the model is fine.

Validation procedure is flawed.

Examples:

- Random split on time series
- Data leakage between folds

---

# Overfitting Prevention by Model Type

# Linear Models

## Regularization

Ridge:

$$
RSS
+
\lambda
\sum_j \beta_j^2
$$

Lasso:

$$
RSS
+
\lambda
\sum_j \lvert \beta_j \rvert
$$

---

## Python Example

```python
from sklearn.linear_model import Ridge

model = Ridge(
    alpha=10
)
```

---

# Decision Trees

Most common cause:

```text
Tree Too Deep
```

---

## Controls

```python
DecisionTreeRegressor(
    max_depth=5,
    min_samples_leaf=20
)
```

---

# Random Forest

Usually less prone to overfitting.

Control:

```python
RandomForestRegressor(
    max_depth=10,
    min_samples_leaf=10
)
```

---

# Gradient Boosting

Most important controls:

```text
learning_rate
max_depth
subsample
```

---

## Example

```python
XGBRegressor(
    learning_rate=0.05,
    max_depth=4,
    subsample=0.8
)
```

---

# Neural Networks

Most susceptible to overfitting.

---

## Weight Decay

Equivalent to Ridge.

$$
L
=
L_{data}
+
\lambda \lVert W \rVert^2
$$

---

## Dropout

Randomly disables neurons.

```python
nn.Dropout(0.3)
```

---

## Early Stopping

Monitor validation loss.

Stop when it stops improving.

```python
EarlyStopping(
    patience=10
)
```

---

# Time Series Overfitting

Special challenges:

```text
Temporal Dependence
Nonstationarity
Regime Changes
```

---

# Common Mistake

Using random train-test split.

Incorrect:

```text
2025 data
predicts
2024 data
```

---

# Correct

```python
TimeSeriesSplit
```

or

```text
Walk Forward Validation
```

---

# Feature Engineering and Overfitting

# Too Many Features

Example:

```text
10,000 Features
500 Samples
```

High overfitting risk.

---

# Feature Selection

Remove weak predictors.

Methods:

- Lasso
- Mutual Information
- Recursive Feature Elimination

---

# Domain Knowledge

Strong domain-driven features often reduce overfitting.

---

# Data Augmentation

Increase effective sample size.

Examples:

Images:

- Rotation
- Flipping

Text:

- Synonym replacement

Audio:

- Noise injection

---

# Cross Validation and Overfitting

## Why Cross Validation Helps

Single validation split:

```text
High Variance
```

Cross validation:

```text
More Reliable
```

---

## Python Example

```python
cross_val_score(
    model,
    X,
    y,
    cv=5
)
```

---

# Early Stopping

One of the most powerful practical techniques.

---

## Example

| Epoch | Train Loss | Validation Loss |
|---------|---------|---------|
| 10 | 0.50 | 0.55 |
| 20 | 0.30 | 0.35 |
| 30 | 0.15 | 0.25 |
| 40 | 0.05 | 0.29 |

Validation worsens.

Stop near epoch 30.

---

# A Practical Overfitting Debugging Workflow

Suppose:

```text
Train Accuracy = 99%
Validation Accuracy = 75%
```

---

## Step 1

Check leakage.

```text
Most important check
```

---

## Step 2

Check train-validation split.

```text
Is validation representative?
```

---

## Step 3

Inspect learning curves.

```text
Overfit or data issue?
```

---

## Step 4

Reduce complexity.

Examples:

```text
Lower Tree Depth
Fewer Features
Smaller Network
```

---

## Step 5

Increase regularization.

Examples:

```text
Larger Alpha
Dropout
Weight Decay
```

---

## Step 6

Try feature selection.

Remove:

```text
Noisy Features
```

---

## Step 7

Collect more data if possible.

---

# Industry Best Practices

# Always Build a Baseline

Start with:

```text
Linear Regression
Logistic Regression
```

before complex models.

---

# Use Proper Validation

For tabular:

```text
K-Fold
```

For time series:

```text
TimeSeriesSplit
```

---

# Track Train vs Validation Metrics

Every experiment should log:

```text
Train Metric
Validation Metric
Gap
```

---

# Use Early Stopping Everywhere

Industry default for:

- XGBoost
- LightGBM
- CatBoost
- Neural Networks

---

# Prefer Simpler Models Initially

Many production systems never need deep learning.

---

# Regularize Aggressively

Especially when:

```text
Dataset is small
```

---

# Feature Engineering Before Model Complexity

Usually:

```text
Better Features
>
More Complex Model
```

---

# Maintain a True Holdout Set

Never use:

```text
Test Set
```

for tuning.

---

# Monitor Production Performance

Even if validation looks good.

Data distributions change.

This is called:

```text
Concept Drift
```

or

```text
Data Drift
```

---

# Common Warning Signs

| Symptom | Likely Cause |
|----------|----------|
| Train Excellent, Validation Poor | Overfitting |
| Validation Better Than Train | Leakage or unusual split |
| Validation Changes Wildly Across Folds | Dataset too small |
| Train and Validation Both Poor | Underfitting |
| Test Much Worse Than Validation | Validation set overfit |

---

# Real-World Iteration Workflow

```text
Train Model
      ↓
Evaluate Validation Performance
      ↓
Large Train/Validation Gap?
      ↓
Yes
      ↓
Check Leakage
      ↓
Reduce Complexity
      ↓
Increase Regularization
      ↓
Feature Selection
      ↓
More Data
      ↓
Retrain
      ↓
Re-Evaluate
```

# Key Takeaways

- Overfitting occurs when a model learns noise instead of signal.
- The hallmark symptom is excellent training performance and significantly worse validation performance.
- Always investigate leakage before changing the model.
- Regularization, feature selection, simpler models, and more data are the most effective remedies.
- Cross-validation helps identify true generalization performance.
- Early stopping is one of the highest-return techniques in modern ML.
- Time series problems require time-aware validation to avoid hidden overfitting.
- Industry teams monitor train-validation gaps continuously during model development.
- Feature engineering and data quality improvements often reduce overfitting more effectively than hyperparameter tuning.
- A model that generalizes well is almost always preferable to a model that perfectly fits historical data.
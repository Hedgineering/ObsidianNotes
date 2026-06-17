# Hyperparameter Tuning and Optimization

## Quick Reference Table

| Concept | Purpose | Examples |
|----------|----------|----------|
| Hyperparameter | Controls model behavior before training | learning_rate, max_depth, n_estimators |
| Parameter | Learned from data during training | Regression coefficients, tree splits, neural network weights |
| Grid Search | Exhaustively test combinations | Small search spaces |
| Random Search | Randomly sample combinations | Large search spaces |
| Bayesian Optimization | Learn where good regions are | Expensive models |
| Cross Validation | Estimate out-of-sample performance | K-Fold, TimeSeriesSplit |
| Early Stopping | Stop training before overfitting | Neural Nets, XGBoost |
| Regularization | Reduce model complexity | Ridge, Lasso, Dropout |
| Validation Set | Used for tuning | Separate from test set |
| Test Set | Final unbiased evaluation | Never tune on it |

---

# Intuition

Hyperparameter tuning is the process of finding settings that allow a model to generalize well to unseen data.

The goal is NOT:

$$
\text{Maximize Training Accuracy}
$$

The goal IS:

$$
\text{Maximize Generalization Performance}
$$

Many beginners accidentally optimize for:

$$
\text{Training Error}
$$

which often causes severe overfitting.

---

# Parameters vs Hyperparameters

## Parameters

Learned automatically during training.

Examples:

Linear Regression:

$$
\beta_0,\beta_1,\ldots,\beta_p
$$

Neural Network:

$$
W,b
$$

Tree:

- Split locations
- Leaf predictions

---

## Hyperparameters

Specified before training.

Examples:

```python
RandomForestRegressor(
    max_depth=10,
    n_estimators=500
)
```

Here:

- max_depth
- n_estimators

are hyperparameters.

---

# Why Hyperparameters Matter

Consider Random Forest.

Very shallow trees:

```text
max_depth = 2
```

may underfit.

Very deep trees:

```text
max_depth = 100
```

may overfit.

Hyperparameter tuning attempts to find:

```text
max_depth = 10
```

or whatever produces the best validation performance.

---

# The Correct Data Splitting Procedure

## Gold Standard

```text
Raw Data
    ↓
Train Set
    ↓
Validation Set
    ↓
Test Set
```

Purpose:

| Dataset | Purpose |
|----------|----------|
| Train | Learn model parameters |
| Validation | Tune hyperparameters |
| Test | Final unbiased evaluation |

---

# Common Mistake

Incorrect:

```text
Train
↓
Tune on Test Set
↓
Report Test Performance
```

This leaks information.

The test set becomes another validation set.

Reported performance becomes optimistic.

---

# Cross Validation

## Why Use It?

A single validation split can be noisy.

Instead:

```text
Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

Train multiple times.

Average performance.

---

## K-Fold Cross Validation

Example:

$$
K = 5
$$

Procedure:

```text
Train Train Train Train Validate
Train Train Train Validate Train
Train Train Validate Train Train
Train Validate Train Train Train
Validate Train Train Train Train
```

Average score.

---

## Python Example

```python
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor()

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="neg_mean_squared_error"
)

print(scores.mean())
```

---

# Time Series Cross Validation

Regular K-Fold is invalid.

Future data must not leak into the past.

Incorrect:

```text
2024 used to predict 2023
```

---

## TimeSeriesSplit

Correct:

```text
Train: Jan-Feb
Validate: Mar

Train: Jan-Mar
Validate: Apr

Train: Jan-Apr
Validate: May
```

---

## Python Example

```python
from sklearn.model_selection import TimeSeriesSplit

tscv = TimeSeriesSplit(
    n_splits=5
)
```

---

# Hyperparameter Search Methods

# Grid Search

## Idea

Try every combination.

Example:

```python
{
    "max_depth": [3,5,7],
    "n_estimators": [100,200]
}
```

Tests:

```text
3 × 2 = 6 combinations
```

---

## Pros

- Simple
- Exhaustive

## Cons

- Expensive
- Scales poorly

---

## Python Example

```python
from sklearn.model_selection import GridSearchCV

grid = GridSearchCV(
    estimator=model,
    param_grid={
        "max_depth":[3,5,7],
        "n_estimators":[100,200]
    },
    cv=5
)

grid.fit(X,y)

print(grid.best_params_)
```

---

# Random Search

## Idea

Randomly sample combinations.

Instead of:

```text
Try everything
```

do:

```text
Try 100 random combinations
```

---

## Why It Works

Many hyperparameters matter very little.

Random search spends more time exploring useful regions.

Often finds equally good solutions with far less computation.

---

## Python Example

```python
from sklearn.model_selection import RandomizedSearchCV

search = RandomizedSearchCV(
    estimator=model,
    param_distributions={
        "max_depth":[3,5,7,10,15],
        "n_estimators":[100,200,500]
    },
    n_iter=20,
    cv=5
)

search.fit(X,y)
```

---

# Bayesian Optimization

## Intuition

Instead of guessing randomly:

Learn where good hyperparameters seem to exist.

Then search nearby.

Process:

```text
Try parameters
      ↓
Observe score
      ↓
Build surrogate model
      ↓
Choose promising region
      ↓
Repeat
```

---

## When to Use

Useful when:

- Training expensive models
- Large search spaces
- Neural networks
- Production ML

---

## Common Libraries

```python
optuna
hyperopt
scikit-optimize
ax
ray tune
```

---

## Optuna Example

```python
import optuna

def objective(trial):

    max_depth = trial.suggest_int(
        "max_depth",
        2,
        20
    )

    model = RandomForestRegressor(
        max_depth=max_depth
    )

    score = ...

    return score

study = optuna.create_study(
    direction="minimize"
)

study.optimize(
    objective,
    n_trials=100
)
```

---

# Early Stopping

## Intuition

Validation loss eventually stops improving.

Example:

| Epoch | Train Loss | Validation Loss |
|---------|---------|---------|
| 10 | 0.50 | 0.55 |
| 20 | 0.35 | 0.40 |
| 30 | 0.20 | 0.32 |
| 40 | 0.10 | 0.35 |

At epoch 40:

Validation loss worsens.

Model begins memorizing noise.

Stop near epoch 30.

---

## XGBoost Example

```python
model.fit(
    X_train,
    y_train,
    eval_set=[(X_val,y_val)],
    early_stopping_rounds=50
)
```

---

# Monitoring Overfitting

## Learning Curves

Plot:

$$
\text{Training Error}
$$

and

$$
\text{Validation Error}
$$

vs training size or epochs.

---

## Overfitting Pattern

```text
Train Error ↓↓↓

Validation Error ↓ then ↑
```

---

## Underfitting Pattern

```text
Train Error High

Validation Error High
```

---

## Good Fit

```text
Train Error Low

Validation Error Low

Small Gap
```

---

# Hyperparameter Tuning by Model Type

# Linear Models

## Important Hyperparameters

Ridge:

```python
alpha
```

Lasso:

```python
alpha
```

Elastic Net:

```python
alpha
l1_ratio
```

---

## Example

```python
from sklearn.linear_model import RidgeCV

model = RidgeCV(
    alphas=np.logspace(-4,4,100)
)
```

---

# Random Forest

## Important Hyperparameters

```text
n_estimators
max_depth
min_samples_leaf
max_features
```

---

## Typical Search Space

```python
{
    "max_depth":[3,5,10,20,None],
    "min_samples_leaf":[1,5,10],
    "n_estimators":[100,300,500]
}
```

---

# XGBoost / LightGBM

## Most Important Hyperparameters

### Learning Rate

$$
\eta
$$

Controls step size.

---

### Number of Trees

```python
n_estimators
```

---

### Maximum Depth

```python
max_depth
```

---

### Row Sampling

```python
subsample
```

---

### Column Sampling

```python
colsample_bytree
```

---

### Regularization

```python
reg_alpha
reg_lambda
```

---

## Industry Rule

Most performance improvements come from:

```text
learning_rate
max_depth
subsample
```

before anything else.

---

# Neural Networks

## Most Important Hyperparameters

### Learning Rate

Usually most important.

Typical:

```python
1e-2
1e-3
1e-4
```

---

### Batch Size

Typical:

```python
32
64
128
256
```

---

### Weight Decay

Controls regularization.

---

### Dropout Rate

Typical:

```python
0.1 - 0.5
```

---

### Architecture

- Layers
- Hidden units
- Attention heads
- Embedding size

---

# Common Search Spaces

## Learning Rate

Never search linearly.

Bad:

```python
[0.001,0.002,0.003]
```

Good:

```python
[1e-5,1e-4,1e-3,1e-2]
```

Use logarithmic scales.

---

## Example

```python
trial.suggest_float(
    "learning_rate",
    1e-5,
    1e-1,
    log=True
)
```

---

# Industry Best Practices

# Start With Baselines

Always begin with:

```text
Linear Model
Random Forest
XGBoost
```

before trying complex deep learning.

---

# Tune Few Parameters First

Most gains come from:

```text
3-5 important hyperparameters
```

not 50.

---

# Use Random Search First

Industry often uses:

```text
Random Search
    ↓
Bayesian Optimization
```

Rarely:

```text
Huge Grid Search
```

---

# Automate Everything

Use:

```text
MLflow
Weights & Biases
Neptune
Comet
```

for experiment tracking.

Record:

- Hyperparameters
- Metrics
- Code version
- Data version

---

# Keep a True Holdout Set

Never touch:

```text
Test Set
```

until the end.

---

# Use Cross Validation

Especially when:

```text
Dataset < 100,000 rows
```

---

# Time Series Requires Time-Aware Validation

Always use:

```python
TimeSeriesSplit
```

or walk-forward validation.

Never shuffle.

---

# Use Early Stopping

Industry default for:

- XGBoost
- LightGBM
- CatBoost
- Neural Networks

---

# Monitor Multiple Metrics

Classification:

```text
Accuracy
Precision
Recall
F1
ROC-AUC
```

Regression:

```text
RMSE
MAE
R²
MAPE
```

---

# Common Pitfalls

## Tuning on Test Data

Most common mistake.

---

## Searching Too Many Hyperparameters

Large search spaces create:

- Huge computation costs
- Difficult interpretation

---

## Ignoring Feature Engineering

Often:

```text
Better Features
>
Better Hyperparameters
```

---

## Comparing Models on Different Splits

Always compare on identical folds.

---

## Optimizing a Single Metric

Example:

```text
Maximize Accuracy
```

while ignoring:

```text
Precision
Recall
Business Cost
```

---

# Real-World Hyperparameter Optimization Workflow

```text
Raw Data
      ↓
Feature Engineering
      ↓
Train / Validation / Test Split
      ↓
Baseline Model
      ↓
Cross Validation
      ↓
Random Search
      ↓
Bayesian Optimization
      ↓
Early Stopping
      ↓
Evaluate on Holdout Test Set
      ↓
Deploy
      ↓
Monitor Production Metrics
```

# Key Takeaways

- Hyperparameter tuning is an optimization problem over model settings.
- Validation data guides tuning; test data does not.
- Cross-validation provides more reliable estimates than a single split.
- Random search usually outperforms grid search for the same compute budget.
- Bayesian optimization is the preferred approach for expensive models.
- Early stopping is one of the most effective regularization techniques.
- Feature engineering often produces larger gains than hyperparameter tuning.
- Industry workflows emphasize reproducibility, experiment tracking, and strict separation of train, validation, and test data.
- The goal is not the best validation score possible; the goal is the best future out-of-sample performance.
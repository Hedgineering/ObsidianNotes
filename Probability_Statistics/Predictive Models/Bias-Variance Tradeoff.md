## Key Takeaways

1. The bias-variance tradeoff describes the tension between:

   - Models that are too simple and underfit
   - Models that are too complex and overfit

2. Bias is error from overly simplistic assumptions.

3. Variance is error from being too sensitive to the training data.

4. The goal is not to minimize bias or variance individually.

   The goal is to minimize total out-of-sample error.

5. A good model balances:

$$
\text{Bias}
+
\text{Variance}
$$

so that it generalizes well to unseen data.

6. In practice, we diagnose bias and variance by comparing:

$$
\text{Training Error}
$$

against

$$
\text{Validation Error}
$$

7. Hyperparameter tuning is often about controlling model complexity.

8. Cross validation is one of the main tools for finding the best bias-variance balance.

---

# Quick Reference Table

| Situation | Training Error | Validation Error | Diagnosis | Common Fix |
|---|---:|---:|---|---|
| High bias | High | High | Underfitting | More complex model, better features, less regularization |
| High variance | Low | High | Overfitting | More regularization, simpler model, more data |
| Good fit | Low/moderate | Low/moderate | Balanced | Keep model, validate robustness |
| Data leakage | Very low | Very low unrealistically | Invalid evaluation | Audit features and splits |
| Noisy target | High | High | Irreducible noise | Better labels, better target, accept limit |

---

# Quick Reference: Model Complexity

| Model / Hyperparameter | Increasing It Usually Does |
|---|---|
| Polynomial degree | Decreases bias, increases variance |
| Tree depth | Decreases bias, increases variance |
| Number of neighbors in KNN | Increases bias, decreases variance |
| Regularization strength $\lambda$ | Increases bias, decreases variance |
| Neural network size | Decreases bias, increases variance |
| Number of boosting rounds | Decreases bias, increases variance |
| Learning rate | Affects optimization and overfitting |
| Dropout rate | Increases bias, decreases variance |
| Minimum samples per leaf | Increases bias, decreases variance |

---

# Core Intuition

Suppose we are trying to predict:

$$
Y
$$

from:

$$
X
$$

A very simple model may miss the real relationship.

Example:

$$
Y
=
X^2
+
\varepsilon
$$

but we fit:

$$
\hat Y
=
\beta_0+\beta_1X
$$

This model is too simple.

It has high bias.

---

A very complex model may fit every random fluctuation in the training data.

It learns:

$$
\text{Signal}
+
\text{Noise}
$$

instead of just:

$$
\text{Signal}
$$

This model has high variance.

---

# Bias

Bias measures error from wrong assumptions or oversimplification.

Informally:

$$
\text{Bias}
=
\text{Systematic Error}
$$

A high-bias model tends to be wrong in the same way across many datasets.

---

## Examples of High Bias

- Fitting a straight line to nonlinear data
- Using too few features
- Strong regularization
- Very shallow decision tree
- Too-small neural network
- Undertrained model
- Ignoring important interactions

---

# Variance

Variance measures sensitivity to the particular training sample.

Informally:

$$
\text{Variance}
=
\text{Instability Across Samples}
$$

A high-variance model may perform very well on training data but poorly on new data.

---

## Examples of High Variance

- Very deep decision tree
- High-degree polynomial regression
- Large neural network with little data
- Too many features relative to observations
- Weak regularization
- Repeatedly tuning on the validation set

---

# Irreducible Error

Some error cannot be removed by any model.

This is caused by:

- Random noise
- Measurement error
- Missing information
- Inherently unpredictable outcomes

---

# Bias-Variance Decomposition

For squared error loss, expected prediction error can be decomposed as:

$$
\mathbb{E}
\left[
(Y-\hat f(X))^2
\right]
=
\operatorname{Bias}(\hat f(X))^2
+
\operatorname{Var}(\hat f(X))
+
\sigma^2
$$

where:

- $\operatorname{Bias}(\hat f(X))^2$ is squared bias
- $\operatorname{Var}(\hat f(X))$ is variance
- $\sigma^2$ is irreducible error

---

# Meaning of Each Term

## Squared Bias

$$
\operatorname{Bias}(\hat f(x))^2
=
\left(
\mathbb{E}[\hat f(x)] - f(x)
\right)^2
$$

This measures how far the average model prediction is from the true function.

---

## Variance

$$
\operatorname{Var}(\hat f(x))
=
\mathbb{E}
\left[
\left(
\hat f(x)-\mathbb{E}[\hat f(x)]
\right)^2
\right]
$$

This measures how much predictions change across different training samples.

---

## Irreducible Noise

If:

$$
Y
=
f(X)+\varepsilon
$$

with:

$$
\operatorname{Var}(\varepsilon)=\sigma^2
$$

then $\sigma^2$ is noise no model can eliminate.

---

# Visual Intuition

Imagine trying to hit a bullseye.

## High Bias, Low Variance

Shots are tightly grouped but far from the center.

The model is consistently wrong.

---

## Low Bias, High Variance

Shots are spread widely around the center.

The model is correct on average but unstable.

---

## Low Bias, Low Variance

Shots are tightly grouped near the center.

This is ideal.

---

# Underfitting

Underfitting occurs when the model is too simple.

Training error:

$$
\text{High}
$$

Validation error:

$$
\text{High}
$$

The model cannot capture the true pattern.

---

## Signs of Underfitting

- Poor training performance
- Poor validation performance
- Residuals show obvious structure
- Model misses important nonlinearities
- Adding training data does not help much
- Both train and test error are similar but bad

---

## Fixes for Underfitting

- Add better features
- Add nonlinear transformations
- Add interaction terms
- Use a more flexible model
- Reduce regularization
- Train longer
- Improve target definition

---

# Overfitting

Overfitting occurs when the model is too complex.

Training error:

$$
\text{Low}
$$

Validation error:

$$
\text{High}
$$

The model memorizes training noise.

---

## Signs of Overfitting

- Very low training error
- Much worse validation error
- Performance changes heavily across folds
- Model relies on suspicious features
- Feature importances are unstable
- Small hyperparameter changes cause large performance changes

---

## Fixes for Overfitting

- Add more data
- Simplify the model
- Increase regularization
- Reduce feature count
- Use cross validation
- Use early stopping
- Use dropout
- Use ensembling
- Remove leakage
- Use stronger validation

---

# Training Error vs Validation Error

Let:

$$
E_{train}
$$

be training error and:

$$
E_{val}
$$

be validation error.

---

## High Bias Pattern

$$
E_{train}
\approx
E_{val}
$$

but both are high.

Interpretation:

The model is too simple.

---

## High Variance Pattern

$$
E_{train}
\ll
E_{val}
$$

Interpretation:

The model overfits.

---

## Good Fit Pattern

$$
E_{train}
\le
E_{val}
$$

and both are reasonably low.

A small gap is normal.

---

# Model Complexity Curve

As model complexity increases:

- Bias usually decreases
- Variance usually increases
- Training error usually decreases
- Validation error usually decreases at first, then increases

The optimal model lies near the minimum validation error.

---

# Example: Polynomial Regression

Suppose the true relationship is curved.

Degree 1 polynomial:

$$
\hat y
=
\beta_0+\beta_1x
$$

Likely high bias.

---

Degree 3 polynomial:

$$
\hat y
=
\beta_0+\beta_1x+\beta_2x^2+\beta_3x^3
$$

May fit well.

---

Degree 30 polynomial:

$$
\hat y
=
\beta_0+\beta_1x+\cdots+\beta_{30}x^{30}
$$

Likely high variance.

---

# Regularization and the Bias-Variance Tradeoff

Regularization penalizes model complexity.

General regularized objective:

$$
\text{Loss}
+
\lambda
\cdot
\text{Penalty}
$$

As $\lambda$ increases:

$$
\text{Bias}
\uparrow
$$

$$
\text{Variance}
\downarrow
$$

---

# Ridge Regression

Ridge uses an $L_2$ penalty:

$$
\min_\beta
\sum_{i=1}^{n}
(y_i-x_i^\top\beta)^2
+
\lambda
\sum_{j=1}^{p}
\beta_j^2
$$

Ridge shrinks coefficients toward zero.

Higher $\lambda$:

- More shrinkage
- Higher bias
- Lower variance

---

# Lasso Regression

Lasso uses an $L_1$ penalty:

$$
\min_\beta
\sum_{i=1}^{n}
(y_i-x_i^\top\beta)^2
+
\lambda
\sum_{j=1}^{p}
|\beta_j|
$$

Lasso can set coefficients exactly to zero.

Useful for feature selection.

---

# Decision Trees

Decision trees have strong bias-variance behavior.

---

## Shallow Tree

Small depth:

$$
\text{High Bias}
$$

$$
\text{Low Variance}
$$

---

## Deep Tree

Large depth:

$$
\text{Low Bias}
$$

$$
\text{High Variance}
$$

---

## Important Tree Hyperparameters

| Hyperparameter | Higher Value Usually Does |
|---|---|
| `max_depth` | Decreases bias, increases variance |
| `min_samples_leaf` | Increases bias, decreases variance |
| `min_samples_split` | Increases bias, decreases variance |
| `max_features` | Decreases correlation among trees |
| `ccp_alpha` | Prunes tree; increases bias, decreases variance |

---

# Random Forests

Random forests reduce variance by averaging many trees.

Single deep tree:

$$
\text{Low Bias, High Variance}
$$

Random forest:

$$
\text{Lower Variance Through Averaging}
$$

Prediction:

$$
\hat f_{RF}(x)
=
\frac1B
\sum_{b=1}^{B}
\hat f_b(x)
$$

---

## Random Forest Tuning

Useful hyperparameters:

- `n_estimators`
- `max_depth`
- `min_samples_leaf`
- `max_features`

Increasing `n_estimators` usually reduces variance until performance stabilizes.

---

# Boosting

Boosting builds models sequentially.

Each new learner tries to fix previous errors.

Examples:

- Gradient Boosting
- XGBoost
- LightGBM
- CatBoost

Boosting can reduce bias strongly but may overfit if pushed too far.

---

## Boosting Hyperparameters

| Hyperparameter | Effect |
|---|---|
| `n_estimators` | More rounds reduce bias but may increase variance |
| `learning_rate` | Smaller values slow learning and regularize |
| `max_depth` | Controls tree complexity |
| `subsample` | Reduces variance |
| `colsample_bytree` | Reduces variance |
| `reg_alpha` | L1 regularization |
| `reg_lambda` | L2 regularization |

---

# Neural Networks

Neural networks are highly flexible.

Large networks can have low bias but high variance.

---

## Common Bias-Variance Controls

| Technique | Main Effect |
|---|---|
| More layers/units | Decreases bias, may increase variance |
| Dropout | Decreases variance |
| Weight decay | Decreases variance |
| Early stopping | Decreases variance |
| Data augmentation | Decreases variance |
| Batch normalization | Improves optimization/stability |
| More data | Decreases variance |

---

# Hyperparameter Tuning Goal

The goal is not to find the model with the best training score.

The goal is to find the model with the best expected future performance.

So we tune based on validation or cross-validation performance.

---

# Standard Hyperparameter Tuning Workflow

1. Split data into train, validation, and test.

2. Fit models on training set.

3. Tune hyperparameters on validation set or cross validation.

4. Select the best model.

5. Evaluate once on the untouched test set.

6. Report test performance.

---

# Cross Validation for Model Selection

For candidate hyperparameters:

$$
\lambda_1,\lambda_2,\dots,\lambda_m
$$

compute cross-validation error:

$$
CV(\lambda_j)
=
\frac1K
\sum_{k=1}^{K}
E_k(\lambda_j)
$$

Choose:

$$
\lambda^*
=
\arg\min_{\lambda_j}
CV(\lambda_j)
$$

---

# One Standard Error Rule

Sometimes the simplest model within one standard error of the best validation score is preferred.

Let:

$$
CV_{min}
$$

be the best cross-validation error.

Choose the simplest model satisfying:

$$
CV(\lambda)
\le
CV_{min}+SE(CV_{min})
$$

This often improves robustness.

---

# Practical Model Selection Strategy

## Step 1: Start with Baselines

Examples:

- Mean prediction
- Linear regression
- Logistic regression
- Naive time series forecast
- Simple moving average
- Existing production model

If a complex model barely beats the baseline, be skeptical.

---

## Step 2: Try Simple Interpretable Models

Examples:

- Linear regression
- Ridge
- Logistic regression
- Decision tree
- ARIMA

These reveal whether there is obvious signal.

---

## Step 3: Try More Flexible Models

Examples:

- Random forest
- Gradient boosting
- Neural network
- State-space models
- Ensembles

Only keep complexity if validation performance improves.

---

## Step 4: Stress Test

Check:

- Different folds
- Different time periods
- Different subgroups
- Different random seeds
- Outliers removed
- Feature subsets

Robust models are more trustworthy.

---

# Python: Train/Validation/Test Split

```python
from sklearn.model_selection import train_test_split

X = df[features]
y = df[target]

X_train, X_temp, y_train, y_temp = train_test_split(
    X,
    y,
    test_size=0.30,
    random_state=42
)

X_val, X_test, y_val, y_test = train_test_split(
    X_temp,
    y_temp,
    test_size=0.50,
    random_state=42
)
```

---

# Python: Diagnosing Bias and Variance

```python
from sklearn.metrics import mean_squared_error

def rmse(y_true, y_pred):
    return mean_squared_error(y_true, y_pred, squared=False)

train_pred = model.predict(X_train)
val_pred = model.predict(X_val)

train_rmse = rmse(y_train, train_pred)
val_rmse = rmse(y_val, val_pred)

print("Train RMSE:", train_rmse)
print("Validation RMSE:", val_rmse)

if train_rmse > 0.8 * val_rmse and val_rmse > desired_threshold:
    print("Likely high bias / underfitting")
elif val_rmse > 1.5 * train_rmse:
    print("Likely high variance / overfitting")
else:
    print("Reasonable bias-variance balance")
```

---

# Python: Ridge Regression Hyperparameter Tuning

```python
import numpy as np

from sklearn.linear_model import Ridge
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import GridSearchCV

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", Ridge())
])

param_grid = {
    "model__alpha": np.logspace(-4, 4, 20)
}

grid = GridSearchCV(
    estimator=pipe,
    param_grid=param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X_train, y_train)

print("Best alpha:", grid.best_params_["model__alpha"])
print("Best CV RMSE:", -grid.best_score_)
```

---

# Python: Lasso Hyperparameter Tuning

```python
import numpy as np

from sklearn.linear_model import Lasso
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import GridSearchCV

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", Lasso(max_iter=10000))
])

param_grid = {
    "model__alpha": np.logspace(-4, 2, 20)
}

grid = GridSearchCV(
    pipe,
    param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X_train, y_train)

best_model = grid.best_estimator_

print("Best alpha:", grid.best_params_["model__alpha"])
print("Best CV RMSE:", -grid.best_score_)
print("Nonzero coefficients:", (best_model.named_steps["model"].coef_ != 0).sum())
```

---

# Python: Decision Tree Complexity Tuning

```python
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import GridSearchCV

param_grid = {
    "max_depth": [2, 3, 5, 10, None],
    "min_samples_leaf": [1, 5, 10, 25, 50]
}

grid = GridSearchCV(
    DecisionTreeRegressor(random_state=42),
    param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X_train, y_train)

print("Best params:", grid.best_params_)
print("Best CV RMSE:", -grid.best_score_)
```

---

# Python: Random Forest Tuning

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import RandomizedSearchCV

param_dist = {
    "n_estimators": [200, 500, 1000],
    "max_depth": [3, 5, 10, None],
    "min_samples_leaf": [1, 5, 10, 25],
    "max_features": ["sqrt", "log2", 0.5, 1.0]
}

search = RandomizedSearchCV(
    RandomForestRegressor(random_state=42, n_jobs=-1),
    param_distributions=param_dist,
    n_iter=25,
    cv=5,
    scoring="neg_root_mean_squared_error",
    random_state=42,
    n_jobs=-1
)

search.fit(X_train, y_train)

print("Best params:", search.best_params_)
print("Best CV RMSE:", -search.best_score_)
```

---

# Python: Gradient Boosting with Early Stopping

```python
from sklearn.ensemble import HistGradientBoostingRegressor
from sklearn.metrics import mean_squared_error

model = HistGradientBoostingRegressor(
    learning_rate=0.05,
    max_iter=1000,
    max_leaf_nodes=31,
    l2_regularization=1.0,
    early_stopping=True,
    validation_fraction=0.2,
    random_state=42
)

model.fit(X_train, y_train)

val_pred = model.predict(X_val)
val_rmse = mean_squared_error(y_val, val_pred, squared=False)

print("Validation RMSE:", val_rmse)
```

---

# Python: Logistic Regression Regularization

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(max_iter=1000))
])

param_grid = {
    "model__C": [0.001, 0.01, 0.1, 1, 10, 100],
    "model__penalty": ["l2"]
}

grid = GridSearchCV(
    pipe,
    param_grid,
    cv=5,
    scoring="roc_auc"
)

grid.fit(X_train, y_train)

print("Best C:", grid.best_params_["model__C"])
print("Best CV AUC:", grid.best_score_)
```

Note:

$$
C
=
\frac{1}{\lambda}
$$

So smaller `C` means stronger regularization.

---

# Python: Learning Curves

Learning curves help diagnose whether more data may help.

```python
import numpy as np
import matplotlib.pyplot as plt

from sklearn.model_selection import learning_curve
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor(random_state=42)

train_sizes, train_scores, val_scores = learning_curve(
    model,
    X,
    y,
    cv=5,
    scoring="neg_root_mean_squared_error",
    train_sizes=np.linspace(0.1, 1.0, 10),
    n_jobs=-1
)

train_rmse = -train_scores.mean(axis=1)
val_rmse = -val_scores.mean(axis=1)

plt.plot(train_sizes, train_rmse, label="Train RMSE")
plt.plot(train_sizes, val_rmse, label="Validation RMSE")
plt.xlabel("Training set size")
plt.ylabel("RMSE")
plt.legend()
plt.show()
```

---

# Interpreting Learning Curves

## High Bias

If train and validation errors converge at a high error:

$$
E_{train}
\approx
E_{val}
$$

and both are high.

More data probably will not help much.

Use a better model or features.

---

## High Variance

If training error is low but validation error is much higher:

$$
E_{train}
\ll
E_{val}
$$

More data may help.

Regularization may also help.

---

# Python: Validation Curve

Validation curves show how one hyperparameter affects bias and variance.

```python
import numpy as np
import matplotlib.pyplot as plt

from sklearn.model_selection import validation_curve
from sklearn.tree import DecisionTreeRegressor

param_range = [1, 2, 3, 5, 10, 20, None]

train_scores, val_scores = validation_curve(
    DecisionTreeRegressor(random_state=42),
    X,
    y,
    param_name="max_depth",
    param_range=param_range,
    cv=5,
    scoring="neg_root_mean_squared_error",
    n_jobs=-1
)

train_rmse = -train_scores.mean(axis=1)
val_rmse = -val_scores.mean(axis=1)

plt.plot([str(x) for x in param_range], train_rmse, label="Train RMSE")
plt.plot([str(x) for x in param_range], val_rmse, label="Validation RMSE")
plt.xlabel("max_depth")
plt.ylabel("RMSE")
plt.legend()
plt.show()
```

---

# Time Series Bias-Variance Tradeoff

For time series, validation must preserve time order.

Random splits can create leakage.

Use:

- Expanding window validation
- Rolling window validation
- Walk-forward validation

---

# Python: Time Series Split

```python
from sklearn.model_selection import TimeSeriesSplit
from sklearn.linear_model import Ridge
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_squared_error

tscv = TimeSeriesSplit(n_splits=5)

rmses = []

for train_idx, test_idx in tscv.split(X):
    X_train_fold = X.iloc[train_idx]
    X_test_fold = X.iloc[test_idx]

    y_train_fold = y.iloc[train_idx]
    y_test_fold = y.iloc[test_idx]

    pipe = Pipeline([
        ("scaler", StandardScaler()),
        ("model", Ridge(alpha=1.0))
    ])

    pipe.fit(X_train_fold, y_train_fold)

    pred = pipe.predict(X_test_fold)

    fold_rmse = mean_squared_error(
        y_test_fold,
        pred,
        squared=False
    )

    rmses.append(fold_rmse)

print("Fold RMSEs:", rmses)
print("Average RMSE:", sum(rmses) / len(rmses))
```

---

# Quant Finance Perspective

In quant finance, bias-variance tradeoff appears as:

| Problem | Bias Issue | Variance Issue |
|---|---|---|
| Alpha model | Misses real signal | Finds false alpha |
| Portfolio optimizer | Too constrained | Overreacts to noisy estimates |
| Risk model | Too simple | Too unstable |
| Volatility forecast | Slow to adapt | Too jumpy |
| Execution model | Ignores market microstructure | Overfits to historical fills |

---

# Quant Example: Portfolio Optimization

Mean-variance optimization is very sensitive to expected return estimates.

Optimization objective:

$$
\max_w
\left(
w^\top \mu
-
\frac{\gamma}{2}
w^\top \Sigma w
\right)
$$

where:

- $w$ is portfolio weights
- $\mu$ is expected returns
- $\Sigma$ is covariance matrix
- $\gamma$ is risk aversion

High variance in $\hat\mu$ can create unstable portfolios.

---

## Practical Fixes

- Shrink expected returns
- Use constraints
- Use regularization
- Use factor models
- Penalize turnover
- Use robust optimization
- Validate out-of-sample

---

# Hyperparameter Tuning Best Practices

1. Always separate training, validation, and test data.

2. Never tune on the final test set.

3. Use cross validation for stable estimates.

4. Use time-aware splits for time series.

5. Tune broad ranges first, then refine.

6. Prefer simpler models when performance is similar.

7. Track all experiments.

8. Evaluate both mean and variance of CV scores.

9. Check robustness across seeds and data splits.

10. Watch for leakage before trusting any score.

---

# Practical Decision Rules

## If Underfitting

Symptoms:

$$
E_{train}
\text{ high}
$$

$$
E_{val}
\text{ high}
$$

Actions:

- Add features
- Add interactions
- Add nonlinear transformations
- Use more flexible models
- Lower regularization
- Train longer
- Improve target quality

---

## If Overfitting

Symptoms:

$$
E_{train}
\text{ low}
$$

$$
E_{val}
\text{ high}
$$

Actions:

- Add data
- Increase regularization
- Reduce model complexity
- Remove noisy features
- Use early stopping
- Use dropout
- Average models
- Improve validation design

---

## If Validation Score is Noisy

Symptoms:

- Fold scores vary widely
- Results change across seeds
- Model ranking is unstable

Actions:

- Use more folds
- Use repeated cross validation
- Use larger validation set
- Bootstrap performance estimates
- Simplify model
- Collect more data

---

# Worked Example: Diagnosing a Model

Suppose a regression model has:

$$
RMSE_{train}
=
2.1
$$

$$
RMSE_{val}
=
2.3
$$

and baseline validation RMSE:

$$
RMSE_{baseline}
=
8.0
$$

Diagnosis:

- Training and validation errors are close.
- Both beat baseline.
- Model likely generalizes reasonably.

---

Now suppose:

$$
RMSE_{train}
=
0.5
$$

$$
RMSE_{val}
=
7.5
$$

Diagnosis:

- Training error is much lower than validation error.
- Model likely overfits.

Fix:

- Increase regularization.
- Reduce complexity.
- Check leakage.
- Add more data.

---

Now suppose:

$$
RMSE_{train}
=
7.8
$$

$$
RMSE_{val}
=
8.0
$$

Diagnosis:

- Training and validation errors are both high.
- Model likely underfits.

Fix:

- Add features.
- Use nonlinear model.
- Reduce regularization.
- Improve target definition.

---

# Common Mistakes

1. Choosing the model with the lowest training error.

2. Tuning repeatedly on the test set.

3. Using random split for time series.

4. Ignoring baseline models.

5. Assuming a bigger model is always better.

6. Assuming regularization always improves performance.

7. Using too narrow a hyperparameter search space.

8. Optimizing one metric while caring about another.

9. Ignoring uncertainty in validation scores.

10. Forgetting that irreducible noise may dominate.

---

# Relationship to Other Topics

Bias-variance tradeoff is closely related to:

- [[Model Evaluation Debugging and Iteration]]
- [[Cross Validation]]
- [[Bootstrap Methods]]
- [[Regression Analysis]]
- [[Regularization]]
- [[Overfitting]]
- [[Experimental Design]]
- [[Hyperparameter Tuning]]
- [[Machine Learning]]
- [[Time Series Analysis]]

---

# Interview-Level Summary

The bias-variance tradeoff explains why models can fail by being either too simple or too complex. High-bias models underfit and perform poorly on both training and validation data. High-variance models overfit and perform much better on training data than validation data. The goal is to choose model complexity and hyperparameters that minimize validation or cross-validation error. In practice, this means starting with baselines, using realistic validation splits, tuning regularization and complexity, checking learning curves, and selecting the simplest model that performs well out-of-sample.
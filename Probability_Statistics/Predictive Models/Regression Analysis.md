## Key Takeaways

1. Regression analysis models the relationship between an outcome variable $Y$ and one or more predictors $X_1,\dots,X_p$.

2. Regression can be used for:
   - Prediction
   - Explanation
   - Estimating relationships
   - Causal analysis, with strong assumptions
   - Forecasting, with time-aware validation

3. Always start with:
   - Clear target definition
   - Data cleaning
   - Exploratory data analysis
   - Simple baseline model
   - Train/validation/test split
   - Residual analysis
   - Out-of-sample evaluation

4. Linear regression is not just a prediction tool. It is also one of the most important tools in classical statistics.

5. A regression coefficient answers:

   > Holding other variables constant, how does $Y$ change when $X_j$ increases by one unit?

6. Good regression analysis is not just fitting a model. It includes:
   - Checking assumptions
   - Diagnosing residuals
   - Testing robustness
   - Avoiding leakage
   - Interpreting coefficients carefully

7. In real-world data science, regression models are often used as baselines because they are simple, fast, interpretable, and surprisingly strong.

---

# Quick Reference: Regression Workflow

| Step | Action | Purpose |
|---|---|---|
| 1 | Define target $Y$ | Clarify what is being predicted or explained |
| 2 | Define predictors $X$ | Choose variables available at prediction time |
| 3 | Clean data | Fix missing values, outliers, duplicates, bad units |
| 4 | Explore data | Understand distributions and relationships |
| 5 | Split data | Prevent overfitting and leakage |
| 6 | Fit baseline | Establish minimum acceptable performance |
| 7 | Fit regression model | Estimate relationships |
| 8 | Evaluate performance | RMSE, MAE, $R^2$, AUC, log loss, etc. |
| 9 | Check residuals | Diagnose model failures |
| 10 | Iterate | Transform features, regularize, add interactions |
| 11 | Validate out-of-sample | Confirm generalization |
| 12 | Interpret carefully | Separate association from causation |

---

# Quick Reference: Data Cleaning Checklist

| Issue | What to Check | Common Fix |
|---|---|---|
| Missing values | NaNs, blanks, nulls | Imputation, missing indicators, deletion |
| Duplicates | Duplicate rows or IDs | Drop or aggregate |
| Outliers | Extreme values | Winsorize, transform, investigate |
| Bad units | Mixed units | Standardize units |
| Categorical variables | Strings, labels | One-hot encode |
| Skewed variables | Long-tailed distributions | Log transform |
| Leakage | Future information in features | Remove leaked variables |
| Collinearity | Predictors too correlated | Drop variables, regularize |
| Time ordering | Future used to predict past | Use time-based split |
| Scaling | Different feature magnitudes | Standardize for regularized models |

---

# Quick Reference: Common Transformations

| Transformation | Formula | Use Case |
|---|---|---|
| Standardization | $z = \frac{x-\bar x}{s}$ | Regularization, gradient methods |
| Min-max scaling | $x'=\frac{x-x_{\min}}{x_{\max}-x_{\min}}$ | Neural nets, bounded inputs |
| Log transform | $x'=\log(x)$ | Positive skewed variables |
| Log plus one | $x'=\log(1+x)$ | Variables with zeros |
| Polynomial terms | $x^2,x^3$ | Nonlinear relationships |
| Interactions | $x_1x_2$ | Variable effects depend on each other |
| One-hot encoding | Category $\rightarrow$ binary columns | Categorical predictors |
| Lag features | $x_{t-1},x_{t-2}$ | Time series |
| Rolling mean | $\frac1k\sum_{j=0}^{k-1}x_{t-j}$ | Time series smoothing |
| Winsorization | Cap extremes | Reduce outlier influence |

---

# Quick Reference: Python Libraries

| Task | Library |
|---|---|
| Data manipulation | `pandas`, `numpy` |
| Visualization | `matplotlib`, `seaborn` |
| Statistical regression | `statsmodels` |
| Machine learning regression | `scikit-learn` |
| Regularized regression | `scikit-learn` |
| Logistic regression | `statsmodels`, `scikit-learn` |
| Count regression | `statsmodels` |
| Time series regression | `statsmodels`, `sklearn` |
| Gradient boosting | `xgboost`, `lightgbm`, `catboost` |

---

# What Regression Does

Regression models a relationship:

$$
Y
=
f(X_1,X_2,\dots,X_p)
+
\varepsilon
$$

where:

- $Y$ is the outcome
- $X_1,\dots,X_p$ are predictors
- $f$ is the systematic relationship
- $\varepsilon$ is random error

The goal is to estimate:

$$
f
$$

from data.

---

# Simple Linear Regression

Simple linear regression uses one predictor.

Model:

$$
Y_i
=
\beta_0
+
\beta_1X_i
+
\varepsilon_i
$$

where:

- $\beta_0$ is the intercept
- $\beta_1$ is the slope
- $\varepsilon_i$ is the error term

---

## Interpretation

$$
\beta_1
=
\text{expected change in }Y\text{ for a one-unit increase in }X
$$

holding everything else fixed.

In simple regression, there is only one $X$.

---

# Multiple Linear Regression

Multiple regression uses several predictors.

$$
Y_i
=
\beta_0
+
\beta_1X_{i1}
+
\beta_2X_{i2}
+
\cdots
+
\beta_pX_{ip}
+
\varepsilon_i
$$

Compact matrix form:

$$
Y
=
X\beta
+
\varepsilon
$$

---

# Ordinary Least Squares (OLS)

OLS chooses coefficients that minimize squared residuals.

Residual:

$$
e_i
=
y_i-\hat y_i
$$

Objective:

$$
\min_{\beta}
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
$$

Since:

$$
\hat y_i
=
\beta_0+\beta_1x_{i1}+\cdots+\beta_px_{ip}
$$

OLS solves:

$$
\min_{\beta}
\sum_{i=1}^{n}
\left(
y_i
-
x_i^\top\beta
\right)^2
$$

---

# OLS Closed-Form Solution

In matrix form:

$$
\hat\beta
=
(X^\top X)^{-1}X^\top y
$$

when $X^\top X$ is invertible.

If predictors are highly collinear, $X^\top X$ may be unstable or singular.

---

# Regression Assumptions

Classical linear regression often assumes:

$$
Y
=
X\beta
+
\varepsilon
$$

with:

$$
E[\varepsilon \mid X]=0
$$

$$
\operatorname{Var}(\varepsilon \mid X)=\sigma^2
$$

$$
\operatorname{Corr}(\varepsilon_i,\varepsilon_j)=0
$$

Sometimes also:

$$
\varepsilon_i \sim N(0,\sigma^2)
$$

especially for small-sample inference.

---

# Assumption Summary

| Assumption | Meaning | Problem if Violated |
|---|---|---|
| Linearity | Relationship is linear in parameters | Biased predictions |
| Independent errors | Errors are not correlated | Bad standard errors |
| Constant variance | Error variance is stable | Inefficient estimates |
| No perfect multicollinearity | Predictors not exact copies | Cannot estimate coefficients |
| Exogeneity | Errors unrelated to predictors | Biased coefficients |
| Normal errors | Errors approximately normal | Small-sample inference issues |

---

# Fitted Values and Residuals

Prediction:

$$
\hat y_i
=
x_i^\top\hat\beta
$$

Residual:

$$
e_i
=
y_i-\hat y_i
$$

Good regression models should have residuals with no obvious pattern.

---

# Regression Metrics

## Mean Squared Error

$$
MSE
=
\frac1n
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
$$

---

## Root Mean Squared Error

$$
RMSE
=
\sqrt{
\frac1n
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
}
$$

---

## Mean Absolute Error

$$
MAE
=
\frac1n
\sum_{i=1}^{n}
|y_i-\hat y_i|
$$

---

## $R^2$

$$
R^2
=
1
-
\frac{
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
}
{
\sum_{i=1}^{n}
(y_i-\bar y)^2
}
$$

Interpretation:

$$
R^2
=
\text{fraction of variation in }Y\text{ explained by the model}
$$

---

## Adjusted $R^2$

$$
R^2_{adj}
=
1
-
(1-R^2)
\frac{n-1}{n-p-1}
$$

Adjusted $R^2$ penalizes adding unnecessary predictors.

---

# Coefficient Interpretation

Suppose:

$$
\hat\beta_1=2.5
$$

Then a one-unit increase in $X_1$ is associated with an average increase of:

$$
2.5
$$

units in $Y$, holding other predictors constant.

---

# Important Warning

Regression coefficients usually show association, not causation.

To interpret $\beta_j$ causally, we need stronger assumptions such as:

- No omitted confounders
- Correct model specification
- No reverse causality
- No selection bias

See [[Experimental Design]] and [[Causal Inference]].

---

# Hypothesis Testing in Regression

For coefficient $\beta_j$:

$$
H_0:
\beta_j=0
$$

$$
H_A:
\beta_j\neq 0
$$

Test statistic:

$$
t
=
\frac{\hat\beta_j}{SE(\hat\beta_j)}
$$

A large absolute $t$-statistic suggests evidence that $\beta_j$ is different from zero.

---

# Confidence Interval for Coefficient

A confidence interval for $\beta_j$:

$$
\hat\beta_j
\pm
t_{\alpha/2,n-p-1}
SE(\hat\beta_j)
$$

If the interval excludes zero, the coefficient is statistically significant at level $\alpha$.

---

# Multicollinearity

Multicollinearity occurs when predictors are highly correlated.

Example:

$$
X_2
\approx
2X_1
$$

Problems:

- Coefficients become unstable.
- Standard errors become large.
- Signs may flip unexpectedly.
- Interpretation becomes difficult.

---

## Variance Inflation Factor

For predictor $X_j$:

$$
VIF_j
=
\frac{1}{1-R_j^2}
$$

where $R_j^2$ comes from regressing $X_j$ on the other predictors.

Large values suggest multicollinearity.

Common rough rule:

$$
VIF > 5
$$

or

$$
VIF > 10
$$

may be concerning.

---

# Heteroskedasticity

Heteroskedasticity means error variance changes with predictors.

Formally:

$$
\operatorname{Var}(\varepsilon_i \mid X_i)
\neq
\sigma^2
$$

Example:

Prediction errors are larger for expensive houses than cheap houses.

---

## Fixes

- Transform $Y$
- Use robust standard errors
- Use weighted least squares
- Use a different model

---

# Autocorrelation

Autocorrelation means errors are correlated over time.

Example:

$$
\operatorname{Corr}(\varepsilon_t,\varepsilon_{t-1})
\neq
0
$$

Common in time series.

Fixes:

- Add lag features
- Use ARIMA/ARIMAX
- Use Newey-West standard errors
- Use state-space models

---

# Outliers and Influential Points

An outlier has unusual $Y$ or $X$ values.

An influential point strongly changes the fitted regression line.

Useful diagnostics:

- Residuals
- Leverage
- Cook's distance

---

## Cook's Distance

Cook's distance measures how much the regression fit changes if observation $i$ is removed.

Large Cook's distance means the observation is influential.

---

# Common Regression Types

| Type | Target | Example |
|---|---|---|
| Linear Regression | Continuous $Y$ | Predict price |
| Ridge Regression | Continuous $Y$ | Many correlated predictors |
| Lasso Regression | Continuous $Y$ | Feature selection |
| Elastic Net | Continuous $Y$ | Sparse correlated predictors |
| Logistic Regression | Binary $Y$ | Predict default |
| Multinomial Logistic Regression | Multi-class $Y$ | Predict category |
| Poisson Regression | Count $Y$ | Predict number of events |
| Quantile Regression | Conditional quantile | Predict median or tail |
| ARIMAX / Dynamic Regression | Time series $Y_t$ | Forecast demand |
| Mixed Effects Regression | Grouped data | Students within schools |

---

# Ridge Regression

Ridge regression adds an $L_2$ penalty.

Objective:

$$
\min_{\beta}
\sum_{i=1}^{n}
(y_i-x_i^\top\beta)^2
+
\lambda
\sum_{j=1}^{p}
\beta_j^2
$$

Ridge shrinks coefficients toward zero but usually does not set them exactly to zero.

Useful when:

- Predictors are correlated
- There are many predictors
- You want lower variance

---

# Lasso Regression

Lasso adds an $L_1$ penalty.

Objective:

$$
\min_{\beta}
\sum_{i=1}^{n}
(y_i-x_i^\top\beta)^2
+
\lambda
\sum_{j=1}^{p}
|\beta_j|
$$

Lasso can set coefficients exactly to zero.

Useful for:

- Feature selection
- Sparse models
- High-dimensional data

---

# Elastic Net

Elastic Net combines Ridge and Lasso.

Objective:

$$
\min_{\beta}
\sum_{i=1}^{n}
(y_i-x_i^\top\beta)^2
+
\lambda_1
\sum_{j=1}^{p}
|\beta_j|
+
\lambda_2
\sum_{j=1}^{p}
\beta_j^2
$$

Useful when:

- There are many predictors
- Predictors are correlated
- You want feature selection plus stability

---

# Logistic Regression

Logistic regression is used for binary outcomes.

Target:

$$
Y \in \{0,1\}
$$

Model:

$$
P(Y=1 \mid X)
=
p(X)
$$

Logit model:

$$
\log
\left(
\frac{p(X)}{1-p(X)}
\right)
=
\beta_0+\beta_1X_1+\cdots+\beta_pX_p
$$

Equivalent:

$$
p(X)
=
\frac{1}
{1+\exp(-X^\top\beta)}
$$

---

## Logistic Regression Interpretation

A coefficient $\beta_j$ changes the log-odds.

Odds:

$$
\frac{p}{1-p}
$$

Log-odds:

$$
\log
\left(
\frac{p}{1-p}
\right)
$$

A one-unit increase in $X_j$ multiplies the odds by:

$$
e^{\beta_j}
$$

holding other variables fixed.

---

# Poisson Regression

Used for count data.

Target:

$$
Y \in \{0,1,2,\dots\}
$$

Model:

$$
Y_i
\sim
\operatorname{Poisson}(\lambda_i)
$$

Log link:

$$
\log(\lambda_i)
=
x_i^\top\beta
$$

Equivalent:

$$
\lambda_i
=
\exp(x_i^\top\beta)
$$

Useful for:

- Number of claims
- Number of trades
- Website clicks
- Count of events

---

# Quantile Regression

OLS estimates conditional mean:

$$
E[Y \mid X]
$$

Quantile regression estimates conditional quantiles:

$$
Q_\tau(Y \mid X)
$$

Examples:

- Median regression: $\tau=0.5$
- Lower tail: $\tau=0.05$
- Upper tail: $\tau=0.95$

Useful when:

- Outliers matter
- Tails matter
- Mean is not enough

---

# Time Series Regression

Time series regression often includes lagged variables.

Example:

$$
Y_t
=
\beta_0
+
\beta_1X_t
+
\beta_2Y_{t-1}
+
\varepsilon_t
$$

For forecasting, features must be available at time $t$.

A valid forecast target:

$$
Y_{t+h}
$$

using features known at time $t$.

---

# Regression in Quant Finance

Common uses:

- Factor models
- Alpha signal testing
- Risk modeling
- Return forecasting
- Volatility forecasting
- Transaction cost modeling
- Portfolio attribution

---

## Factor Model Example

A common linear factor model:

$$
R_i - R_f
=
\alpha
+
\beta_1F_1
+
\beta_2F_2
+
\cdots
+
\beta_kF_k
+
\varepsilon_i
$$

where:

- $R_i$ is asset return
- $R_f$ is risk-free return
- $F_1,\dots,F_k$ are factor returns
- $\alpha$ is unexplained return

---

## Alpha Interpretation

If:

$$
\alpha > 0
$$

and statistically significant, the strategy may have return unexplained by known factors.

But always check:

- Transaction costs
- Data snooping
- Out-of-sample stability
- Regime dependence
- Multiple testing

---

# Python: Core Imports

```python
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split, KFold, cross_val_score, TimeSeriesSplit
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
```

---

# Python: Linear Regression with scikit-learn

Use `scikit-learn` when prediction is the main goal.

```python
import numpy as np
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# X = feature matrix, y = target
X = df[["x1", "x2", "x3"]]
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)

pred = model.predict(X_test)

rmse = mean_squared_error(y_test, pred, squared=False)
mae = mean_absolute_error(y_test, pred)
r2 = r2_score(y_test, pred)

print("RMSE:", rmse)
print("MAE:", mae)
print("R^2:", r2)
print("Coefficients:", model.coef_)
print("Intercept:", model.intercept_)
```

---

# Python: Linear Regression with statsmodels

Use `statsmodels` when inference and coefficient interpretation matter.

```python
import statsmodels.api as sm

X = df[["x1", "x2", "x3"]]
y = df["target"]

X = sm.add_constant(X)

model = sm.OLS(y, X).fit()

print(model.summary())
```

Useful output:

- Coefficients
- Standard errors
- t-statistics
- p-values
- confidence intervals
- $R^2$
- adjusted $R^2$

---

# Python: Robust Standard Errors

Use robust standard errors when heteroskedasticity may be present.

```python
import statsmodels.api as sm

X = df[["x1", "x2", "x3"]]
y = df["target"]

X = sm.add_constant(X)

model = sm.OLS(y, X).fit(cov_type="HC3")

print(model.summary())
```

---

# Python: Ridge Regression

```python
from sklearn.linear_model import Ridge
from sklearn.model_selection import GridSearchCV
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

X = df[["x1", "x2", "x3"]]
y = df["target"]

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", Ridge())
])

param_grid = {
    "model__alpha": [0.01, 0.1, 1, 10, 100]
}

grid = GridSearchCV(
    pipe,
    param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X, y)

print("Best alpha:", grid.best_params_)
print("Best CV RMSE:", -grid.best_score_)
```

---

# Python: Lasso Regression

```python
from sklearn.linear_model import Lasso
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import GridSearchCV

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", Lasso(max_iter=10000))
])

param_grid = {
    "model__alpha": [0.001, 0.01, 0.1, 1, 10]
}

grid = GridSearchCV(
    pipe,
    param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X, y)

print("Best alpha:", grid.best_params_)
print("Best CV RMSE:", -grid.best_score_)
print("Coefficients:", grid.best_estimator_.named_steps["model"].coef_)
```

---

# Python: Elastic Net

```python
from sklearn.linear_model import ElasticNet
from sklearn.model_selection import GridSearchCV
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", ElasticNet(max_iter=10000))
])

param_grid = {
    "model__alpha": [0.001, 0.01, 0.1, 1],
    "model__l1_ratio": [0.1, 0.5, 0.9]
}

grid = GridSearchCV(
    pipe,
    param_grid,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

grid.fit(X, y)

print("Best params:", grid.best_params_)
print("Best CV RMSE:", -grid.best_score_)
```

---

# Python: Logistic Regression with scikit-learn

```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, roc_auc_score, log_loss
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

X = df[["x1", "x2", "x3"]]
y = df["target_binary"]

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(max_iter=1000))
])

pipe.fit(X_train, y_train)

pred_class = pipe.predict(X_test)
pred_prob = pipe.predict_proba(X_test)[:, 1]

print("Accuracy:", accuracy_score(y_test, pred_class))
print("Precision:", precision_score(y_test, pred_class))
print("Recall:", recall_score(y_test, pred_class))
print("F1:", f1_score(y_test, pred_class))
print("ROC-AUC:", roc_auc_score(y_test, pred_prob))
print("Log Loss:", log_loss(y_test, pred_prob))
```

---

# Python: Logistic Regression with statsmodels

```python
import statsmodels.api as sm

X = df[["x1", "x2", "x3"]]
y = df["target_binary"]

X = sm.add_constant(X)

model = sm.Logit(y, X).fit()

print(model.summary())

odds_ratios = np.exp(model.params)
print(odds_ratios)
```

---

# Python: Poisson Regression

```python
import statsmodels.api as sm

X = df[["x1", "x2", "x3"]]
y = df["count_target"]

X = sm.add_constant(X)

model = sm.GLM(
    y,
    X,
    family=sm.families.Poisson()
).fit()

print(model.summary())
```

---

# Python: Quantile Regression

```python
import statsmodels.formula.api as smf

model = smf.quantreg(
    "target ~ x1 + x2 + x3",
    data=df
)

result = model.fit(q=0.5)

print(result.summary())
```

For the 90th percentile:

```python
result_90 = model.fit(q=0.9)
print(result_90.summary())
```

---

# Python: Regression with Categorical Variables

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.linear_model import Ridge
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

numeric_features = ["age", "income"]
categorical_features = ["region", "industry"]

X = df[numeric_features + categorical_features]
y = df["target"]

preprocess = ColumnTransformer([
    ("num", StandardScaler(), numeric_features),
    ("cat", OneHotEncoder(handle_unknown="ignore"), categorical_features)
])

pipe = Pipeline([
    ("preprocess", preprocess),
    ("model", Ridge(alpha=1.0))
])

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

pipe.fit(X_train, y_train)

pred = pipe.predict(X_test)

rmse = mean_squared_error(y_test, pred, squared=False)

print("RMSE:", rmse)
```

---

# Python: Time Series Regression Split

Do not randomly shuffle time series data.

```python
from sklearn.model_selection import TimeSeriesSplit
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

X = df[["lag_1", "lag_2", "rolling_mean_5"]]
y = df["target"]

tscv = TimeSeriesSplit(n_splits=5)

rmses = []

for train_idx, test_idx in tscv.split(X):
    X_train, X_test = X.iloc[train_idx], X.iloc[test_idx]
    y_train, y_test = y.iloc[train_idx], y.iloc[test_idx]

    pipe = Pipeline([
        ("scaler", StandardScaler()),
        ("model", Ridge(alpha=1.0))
    ])

    pipe.fit(X_train, y_train)

    pred = pipe.predict(X_test)

    rmse = mean_squared_error(y_test, pred, squared=False)
    rmses.append(rmse)

print("Fold RMSEs:", rmses)
print("Average RMSE:", np.mean(rmses))
```

---

# Python: Residual Plot

```python
import matplotlib.pyplot as plt

residuals = y_test - pred

plt.scatter(pred, residuals)
plt.axhline(0, linestyle="--")
plt.xlabel("Predicted values")
plt.ylabel("Residuals")
plt.title("Residual Plot")
plt.show()
```

Look for:

- Random scatter around zero
- No funnel shape
- No curved pattern
- No extreme outliers

---

# Python: Checking Multicollinearity with VIF

```python
import pandas as pd
import statsmodels.api as sm

from statsmodels.stats.outliers_influence import variance_inflation_factor

X = df[["x1", "x2", "x3"]]
X = sm.add_constant(X)

vif = pd.DataFrame()
vif["feature"] = X.columns
vif["VIF"] = [
    variance_inflation_factor(X.values, i)
    for i in range(X.shape[1])
]

print(vif)
```

---

# Python: Train/Test Evaluation Function

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

def evaluate_regression(y_true, y_pred):
    rmse = mean_squared_error(y_true, y_pred, squared=False)
    mae = mean_absolute_error(y_true, y_pred)
    r2 = r2_score(y_true, y_pred)

    return {
        "RMSE": rmse,
        "MAE": mae,
        "R2": r2
    }
```

---

# Worked Example: Simple Linear Regression

Suppose we have:

| Hours Studied | Score |
|---:|---:|
| 1 | 50 |
| 2 | 55 |
| 3 | 65 |
| 4 | 70 |
| 5 | 75 |

Let:

$$
X
=
[1,2,3,4,5]
$$

$$
Y
=
[50,55,65,70,75]
$$

We fit:

$$
Y_i
=
\beta_0+\beta_1X_i+\varepsilon_i
$$

---

## Step 1: Compute Means

$$
\bar X
=
\frac{1+2+3+4+5}{5}
=
3
$$

$$
\bar Y
=
\frac{50+55+65+70+75}{5}
=
63
$$

---

## Step 2: Compute Slope

Formula:

$$
\hat\beta_1
=
\frac{
\sum_{i=1}^{n}
(x_i-\bar x)(y_i-\bar y)
}
{
\sum_{i=1}^{n}
(x_i-\bar x)^2
}
$$

Numerator:

$$
(1-3)(50-63)
+
(2-3)(55-63)
+
(3-3)(65-63)
+
(4-3)(70-63)
+
(5-3)(75-63)
$$

$$
=
(-2)(-13)
+
(-1)(-8)
+
(0)(2)
+
(1)(7)
+
(2)(12)
$$

$$
=
26+8+0+7+24
=
65
$$

Denominator:

$$
(1-3)^2+(2-3)^2+(3-3)^2+(4-3)^2+(5-3)^2
$$

$$
=
4+1+0+1+4
=
10
$$

Therefore:

$$
\hat\beta_1
=
\frac{65}{10}
=
6.5
$$

---

## Step 3: Compute Intercept

$$
\hat\beta_0
=
\bar y-\hat\beta_1\bar x
$$

$$
=
63-6.5(3)
$$

$$
=
43.5
$$

---

## Final Model

$$
\hat Y
=
43.5+6.5X
$$

Interpretation:

Each extra hour studied is associated with about:

$$
6.5
$$

additional points.

---

# Worked Example: Prediction

Predict score for:

$$
X=6
$$

Using:

$$
\hat Y
=
43.5+6.5X
$$

we get:

$$
\hat Y
=
43.5+6.5(6)
$$

$$
=
43.5+39
$$

$$
=
82.5
$$

Predicted score:

$$
82.5
$$

---

# Worked Example: Logistic Regression Interpretation

Suppose logistic regression gives:

$$
\hat\beta_1=0.7
$$

Then a one-unit increase in $X_1$ multiplies odds by:

$$
e^{0.7}
$$

$$
\approx
2.01
$$

Interpretation:

Holding other variables fixed, a one-unit increase in $X_1$ approximately doubles the odds of the event.

---

# Regression Debugging Checklist

## If Training Error is High and Test Error is High

Likely underfitting.

Try:

- Add features
- Add nonlinear terms
- Use interactions
- Use more flexible model
- Improve target definition
- Reduce regularization

---

## If Training Error is Low and Test Error is High

Likely overfitting.

Try:

- Add regularization
- Remove noisy features
- Use more data
- Simplify model
- Cross validate
- Check leakage
- Reduce model complexity

---

## If Coefficients Look Wrong

Check:

- Multicollinearity
- Scaling
- Outliers
- Missing values
- Incorrect signs from confounding
- Bad target definition
- Data leakage

---

## If Residuals Show a Curve

Model may be missing nonlinearity.

Try:

- Polynomial terms
- Log transform
- Splines
- Tree-based models
- Interaction terms

---

## If Residuals Fan Out

Likely heteroskedasticity.

Try:

- Log-transform $Y$
- Weighted least squares
- Robust standard errors
- Different loss function

---

## If Time Series Residuals Are Autocorrelated

Try:

- Add lagged target
- Add lagged predictors
- Use ARIMA errors
- Use Newey-West standard errors
- Use state-space model

---

# Common Mistakes

1. Interpreting correlation as causation.
2. Ignoring data leakage.
3. Using random splits for time series.
4. Forgetting to compare against a baseline.
5. Trusting $R^2$ alone.
6. Ignoring residual plots.
7. Ignoring outliers.
8. Ignoring multicollinearity.
9. Over-interpreting p-values.
10. Using linear regression for binary outcomes.
11. Scaling before train/test split.
12. Tuning repeatedly on the test set.
13. Ignoring whether predictors are available at prediction time.

---

# Best Practices

1. Start simple.

2. Compare against a baseline.

3. Use `statsmodels` for inference.

4. Use `scikit-learn` for prediction pipelines.

5. Use pipelines to avoid preprocessing leakage.

6. Use cross validation when appropriate.

7. Use time-aware validation for time series.

8. Check residuals.

9. Report uncertainty when interpreting coefficients.

10. Interpret coefficients in context.

11. Prefer simpler models when performance is similar.

12. Validate on truly unseen data.

---

# Relationship to Other Topics

Regression analysis is closely related to:

- [[Hypothesis Testing]]
- [[Confidence Intervals]]
- [[Cross Validation]]
- [[Experimental Design]]
- [[Bootstrap Methods]]
- [[Bias-Variance Tradeoff]]
- [[Model Evaluation Debugging and Iteration]]
- [[Time Series Analysis]]
- [[Feature Engineering]]
- [[Causal Inference]]

---

# Interview-Level Summary

Regression analysis models the relationship between an outcome variable and predictors. Linear regression estimates continuous outcomes using least squares, logistic regression models binary outcomes using log-odds, and regularized methods like Ridge, Lasso, and Elastic Net improve stability and reduce overfitting. A strong regression workflow includes data cleaning, leakage checks, exploratory analysis, baseline comparison, realistic validation splits, residual diagnostics, coefficient interpretation, and out-of-sample evaluation. In practice, regression is valuable because it is simple, interpretable, fast, and often forms the baseline against which more complex models are judged.
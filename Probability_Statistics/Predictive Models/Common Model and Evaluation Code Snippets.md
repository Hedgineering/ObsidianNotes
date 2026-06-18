Assume:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

# 1. Train/Test Split

Use for ordinary ML when rows are independent.

```python
from sklearn.model_selection import train_test_split

X = df.drop(columns=["target"])
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

For classification with imbalanced classes:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    stratify=y,
    random_state=42
)
```

---

# 2. Time Series Split

Use when order matters. Never randomly shuffle time series.

```python
split_date = "2024-01-01"

train = df[df["date"] < split_date]
test = df[df["date"] >= split_date]

y_train = train["target"]
y_test = test["target"]
```

With sklearn:

```python
from sklearn.model_selection import TimeSeriesSplit

tscv = TimeSeriesSplit(n_splits=5)

for train_idx, test_idx in tscv.split(X):
    X_train, X_test = X.iloc[train_idx], X.iloc[test_idx]
    y_train, y_test = y.iloc[train_idx], y.iloc[test_idx]
```

---

# 3. Regression Metrics

Use for continuous targets.

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

pred = model.predict(X_test)

mae = mean_absolute_error(y_test, pred)
rmse = mean_squared_error(y_test, pred, squared=False)
r2 = r2_score(y_test, pred)

print("MAE:", mae)
print("RMSE:", rmse)
print("R2:", r2)
```

Interpretation:

```text
MAE  = average absolute error
RMSE = penalizes large errors more
R2   = variance explained
```

---

# 4. Classification Metrics

Use for binary or multiclass targets.

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, classification_report

pred = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, pred))
print("Precision:", precision_score(y_test, pred))
print("Recall:", recall_score(y_test, pred))
print("F1:", f1_score(y_test, pred))

print(classification_report(y_test, pred))
```

For probabilities:

```python
from sklearn.metrics import roc_auc_score, average_precision_score

proba = model.predict_proba(X_test)[:, 1]

print("ROC AUC:", roc_auc_score(y_test, proba))
print("PR AUC:", average_precision_score(y_test, proba))
```

Use PR AUC when classes are imbalanced.

---

# 5. Confusion Matrix

```python
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay

pred = model.predict(X_test)

cm = confusion_matrix(y_test, pred)

ConfusionMatrixDisplay(cm).plot()
plt.show()
```

---

# 6. Linear Regression

Use for simple continuous target prediction.

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)

pred = model.predict(X_test)
```

---

# 7. Ridge / Lasso Regression

Use when features are correlated or high-dimensional.

```python
from sklearn.linear_model import Ridge, Lasso

ridge = Ridge(alpha=1.0)
ridge.fit(X_train, y_train)

lasso = Lasso(alpha=0.01)
lasso.fit(X_train, y_train)
```

```text
Ridge = shrinks coefficients
Lasso = can set coefficients to zero
```

---

# 8. Logistic Regression

Use for binary classification.

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

pred = model.predict(X_test)
proba = model.predict_proba(X_test)[:, 1]
```

With class imbalance:

```python
model = LogisticRegression(
    max_iter=1000,
    class_weight="balanced"
)
```

---

# 9. Random Forest

Use for nonlinear relationships and feature interactions.

```python
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier

model = RandomForestRegressor(
    n_estimators=300,
    max_depth=None,
    random_state=42,
    n_jobs=-1
)

model.fit(X_train, y_train)
pred = model.predict(X_test)
```

Classifier:

```python
model = RandomForestClassifier(
    n_estimators=300,
    class_weight="balanced",
    random_state=42,
    n_jobs=-1
)
```

---

# 10. Gradient Boosting / XGBoost

Use for strong tabular-data performance.

```python
from xgboost import XGBRegressor

model = XGBRegressor(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=4,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42
)

model.fit(X_train, y_train)
pred = model.predict(X_test)
```

Classifier:

```python
from xgboost import XGBClassifier

model = XGBClassifier(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=4,
    eval_metric="logloss",
    random_state=42
)
```

---

# 11. LightGBM

Use for fast, high-performing tabular models.

```python
from lightgbm import LGBMRegressor

model = LGBMRegressor(
    n_estimators=500,
    learning_rate=0.05,
    num_leaves=31,
    random_state=42
)

model.fit(X_train, y_train)
pred = model.predict(X_test)
```

---

# 12. Neural Net for Tabular Regression

Use when data is large and nonlinear.

```python
import torch
import torch.nn as nn

class Net(nn.Module):
    def __init__(self, n_features):
        super().__init__()

        self.net = nn.Sequential(
            nn.Linear(n_features, 64),
            nn.ReLU(),
            nn.Linear(64, 32),
            nn.ReLU(),
            nn.Linear(32, 1)
        )

    def forward(self, x):
        return self.net(x)

model = Net(n_features=X_train.shape[1])

loss_fn = nn.MSELoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

X_t = torch.tensor(X_train.values, dtype=torch.float32)
y_t = torch.tensor(y_train.values.reshape(-1, 1), dtype=torch.float32)

for epoch in range(100):
    pred = model(X_t)
    loss = loss_fn(pred, y_t)

    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

    if epoch % 10 == 0:
        print(epoch, loss.item())
```

---

# 13. ACF Plot

Use to check autocorrelation in a time series.

```python
from statsmodels.graphics.tsaplots import plot_acf

y = df.set_index("date")["value"]

plot_acf(y.dropna(), lags=40)
plt.title("ACF Plot")
plt.show()
```

Interpretation:

```text
Slow decay
→ trend / nonstationarity

Spike at lag k
→ autocorrelation at lag k

Seasonal spikes
→ seasonality
```

---

# 14. PACF Plot

Use to help select AR order.

```python
from statsmodels.graphics.tsaplots import plot_pacf

plot_pacf(y.dropna(), lags=40)
plt.title("PACF Plot")
plt.show()
```

Interpretation:

```text
PACF cutoff after lag p
→ possible AR(p)
```

---

# 15. Train/Test Forecast Plot

```python
plt.figure(figsize=(12, 5))

plt.plot(train.index, y_train, label="Train")
plt.plot(test.index, y_test, label="Test")
plt.plot(test.index, forecast, label="Forecast")

plt.legend()
plt.title("Forecast vs Actual")
plt.show()
```

---

# 16. Confidence Bands for Forecasts

Use when model returns forecast intervals.

```python
plt.figure(figsize=(12, 5))

plt.plot(test.index, y_test, label="Actual")
plt.plot(test.index, forecast_mean, label="Forecast")

plt.fill_between(
    test.index,
    conf_int.iloc[:, 0],
    conf_int.iloc[:, 1],
    alpha=0.2,
    label="95% Confidence Interval"
)

plt.legend()
plt.title("Forecast with Confidence Band")
plt.show()
```

---

# 17. ARIMA

Use for univariate time series with trend/autocorrelation.

```python
from statsmodels.tsa.arima.model import ARIMA

y = df.set_index("date")["value"]

model = ARIMA(y_train, order=(1, 1, 1))
fit = model.fit()

forecast_res = fit.get_forecast(steps=len(y_test))

forecast_mean = forecast_res.predicted_mean
conf_int = forecast_res.conf_int()
```

```text
order=(p,d,q)

p = autoregressive lags
d = differencing
q = moving-average lags
```

---

# 18. SARIMA

Use when the series has seasonality.

```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

model = SARIMAX(
    y_train,
    order=(1, 1, 1),
    seasonal_order=(1, 1, 1, 12)
)

fit = model.fit()

forecast_res = fit.get_forecast(steps=len(y_test))

forecast_mean = forecast_res.predicted_mean
conf_int = forecast_res.conf_int()
```

```text
seasonal_order=(P,D,Q,s)

s = seasonal period
12 for monthly yearly seasonality
7 for daily weekly seasonality
```

---

# 19. SARIMAX with Exogenous Variables

Use when outside variables help forecast.

```python
exog_train = train[["gas_price", "marketing_spend"]]
exog_test = test[["gas_price", "marketing_spend"]]

model = SARIMAX(
    y_train,
    exog=exog_train,
    order=(1, 1, 1),
    seasonal_order=(1, 1, 1, 12)
)

fit = model.fit()

forecast_res = fit.get_forecast(
    steps=len(y_test),
    exog=exog_test
)

forecast_mean = forecast_res.predicted_mean
```

---

# 20. ARCH / GARCH

Use for volatility modeling, especially financial returns.

```python
from arch import arch_model

returns = df["returns"].dropna() * 100

model = arch_model(
    returns,
    mean="Constant",
    vol="GARCH",
    p=1,
    q=1
)

fit = model.fit()

forecast = fit.forecast(horizon=5)

variance_forecast = forecast.variance.iloc[-1]
vol_forecast = np.sqrt(variance_forecast)
```

```text
ARCH/GARCH models changing variance over time.
Useful for volatility clustering.
```

---

# 21. VAR

Use for multiple time series that influence each other.

```python
from statsmodels.tsa.api import VAR

data = df.set_index("date")[["demand", "supply", "price"]].dropna()

train = data.iloc[:-12]
test = data.iloc[-12:]

model = VAR(train)

fit = model.fit(maxlags=5, ic="aic")

forecast = fit.forecast(
    train.values[-fit.k_ar:],
    steps=len(test)
)

forecast_df = pd.DataFrame(
    forecast,
    index=test.index,
    columns=data.columns
)
```

```text
VAR = vector autoregression.
Each variable is modeled using lags of itself and others.
```

---

# 22. VARMA

Use for multivariate time series with AR and MA components.

```python
from statsmodels.tsa.statespace.varmax import VARMAX

data = df.set_index("date")[["y1", "y2"]].dropna()

train = data.iloc[:-12]
test = data.iloc[-12:]

model = VARMAX(
    train,
    order=(1, 1)
)

fit = model.fit(disp=False)

forecast = fit.forecast(steps=len(test))
```

```text
VARMA is more flexible than VAR,
but harder to estimate and less commonly used.
```

---

# 23. Stationarity Test

Use before ARIMA/VAR models.

```python
from statsmodels.tsa.stattools import adfuller

result = adfuller(y.dropna())

print("ADF Statistic:", result[0])
print("p-value:", result[1])
```

Interpretation:

```text
p < 0.05
→ likely stationary

p >= 0.05
→ likely nonstationary
```

---

# 24. Differencing

Use to remove trend / make series stationary.

```python
df["value_diff"] = df["value"].diff()

df["value_diff"].plot()
plt.title("Differenced Series")
plt.show()
```

Seasonal differencing:

```python
df["value_seasonal_diff"] = df["value"].diff(12)
```

---

# 25. Rolling Mean / Volatility

Use to inspect trend and changing variance.

```python
y = df.set_index("date")["value"]

rolling_mean = y.rolling(30).mean()
rolling_std = y.rolling(30).std()

plt.figure(figsize=(12, 5))
plt.plot(y, label="Original")
plt.plot(rolling_mean, label="Rolling Mean")
plt.plot(rolling_std, label="Rolling Std")
plt.legend()
plt.show()
```

---

# 26. Residual Diagnostics

Use after fitting a time series model.

```python
residuals = fit.resid

plt.figure(figsize=(10, 4))
plt.plot(residuals)
plt.title("Residuals")
plt.show()

plot_acf(residuals.dropna(), lags=40)
plt.title("Residual ACF")
plt.show()
```

Good residuals should look like noise.

---

# 27. Ljung-Box Test

Checks whether residual autocorrelation remains.

```python
from statsmodels.stats.diagnostic import acorr_ljungbox

lb = acorr_ljungbox(
    residuals.dropna(),
    lags=[10, 20],
    return_df=True
)

print(lb)
```

Interpretation:

```text
p < 0.05
→ residual autocorrelation remains

p >= 0.05
→ residuals look closer to white noise
```

---

# 28. Feature Importance

Tree-based models.

```python
importances = pd.Series(
    model.feature_importances_,
    index=X_train.columns
).sort_values(ascending=False)

importances.head(20).plot(kind="barh")
plt.title("Feature Importance")
plt.show()
```

---

# 29. Permutation Importance

Model-agnostic feature importance.

```python
from sklearn.inspection import permutation_importance

result = permutation_importance(
    model,
    X_test,
    y_test,
    n_repeats=10,
    random_state=42
)

importance = pd.Series(
    result.importances_mean,
    index=X_test.columns
).sort_values(ascending=False)

importance.head(20).plot(kind="barh")
plt.title("Permutation Importance")
plt.show()
```

---

# 30. Cross Validation

Use for robust evaluation on IID data.

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="neg_root_mean_squared_error"
)

print("CV RMSE:", -scores.mean())
```

Classification:

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="roc_auc"
)

print("CV ROC AUC:", scores.mean())
```

---

# 31. Model Pipeline

Use to avoid leakage.

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(max_iter=1000))
])

pipe.fit(X_train, y_train)

pred = pipe.predict(X_test)
```

---

# 32. ColumnTransformer

Use for mixed numerical + categorical features.

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.ensemble import RandomForestClassifier

num_cols = ["age", "income", "balance"]
cat_cols = ["segment", "state"]

preprocess = ColumnTransformer([
    ("num", StandardScaler(), num_cols),
    ("cat", OneHotEncoder(handle_unknown="ignore"), cat_cols)
])

pipe = Pipeline([
    ("preprocess", preprocess),
    ("model", RandomForestClassifier(random_state=42))
])

pipe.fit(X_train, y_train)
```

---

# 33. Quick Model Selection Guide

| Problem | Common Starting Model |
|---|---|
| Regression baseline | Linear Regression |
| Binary classification baseline | Logistic Regression |
| Nonlinear tabular data | Random Forest / XGBoost |
| Large tabular data | LightGBM |
| Image/text/deep patterns | Neural Networks |
| Univariate forecast | ARIMA / SARIMA |
| Seasonal forecast | SARIMA / SARIMAX |
| Forecast with external drivers | SARIMAX |
| Volatility forecast | ARCH / GARCH |
| Multiple related time series | VAR |
| Multivariate AR + MA | VARMA |
| Strong class imbalance | Logistic/XGBoost + PR AUC |
| Interpretability needed | Linear models / shallow trees |
| Maximum accuracy on tabular | Gradient boosting |
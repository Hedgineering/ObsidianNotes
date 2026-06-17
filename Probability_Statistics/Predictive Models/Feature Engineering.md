
## Quick Reference Table

| Technique | Purpose | Common Use Cases |
|------------|------------|------------|
| Missing Value Imputation | Handle missing data | Almost all ML projects |
| Scaling / Standardization | Normalize feature magnitudes | Linear models, Neural Nets |
| Encoding Categorical Variables | Convert categories to numbers | Business and tabular data |
| Log Transform | Reduce skewness | Financial and count data |
| Polynomial Features | Model nonlinear relationships | Linear models |
| Interaction Features | Capture feature interactions | Regression, boosting |
| Aggregation Features | Summarize groups | User behavior, transactions |
| Time Features | Extract temporal patterns | Forecasting |
| Lag Features | Capture temporal dependence | Time series |
| Rolling Statistics | Capture trends and momentum | Time series |
| Frequency Encoding | Encode category popularity | High-cardinality categoricals |
| Target Encoding | Encode categories using target information | Tabular competitions |
| PCA | Reduce dimensionality | High-dimensional data |
| Feature Selection | Remove irrelevant variables | All ML models |

---

# What is Feature Engineering?

Feature engineering is the process of transforming raw data into useful inputs for machine learning models.

Goal:

$$
\text{Raw Data}
\rightarrow
\text{Better Features}
\rightarrow
\text{Better Predictions}
$$

In many real-world problems:

```text
Feature Engineering
>
Model Selection
```

A simple model with strong features often outperforms a sophisticated model with poor features.

---

# Intuition

Suppose we want to predict house prices.

Raw feature:

```text
Date Purchased
```

Not very useful.

Engineered features:

```text
House Age
Years Since Renovation
Month
Season
```

Now the model can learn more meaningful patterns.

---

# The General Feature Engineering Workflow

```text
Raw Data
      ↓
Cleaning
      ↓
Missing Values
      ↓
Encoding
      ↓
Transformations
      ↓
Feature Creation
      ↓
Feature Selection
      ↓
Model Training
```

---

# Missing Value Handling

## Why Missing Values Matter

Many models cannot handle:

```text
NaN
```

directly.

Missing values may also contain information.

Example:

```text
Income Missing
```

might correlate with:

```text
Unemployed
```

---

# Common Strategies

## Mean Imputation

$$
x_{missing}
=
\bar{x}
$$

---

## Median Imputation

Preferred when skewed.

$$
x_{missing}
=
\operatorname{Median}(x)
$$

---

## Mode Imputation

For categorical variables.

---

## Missing Indicator

Create:

$$
M=
\begin{cases}
1 & \text{if missing}\\
0 & \text{otherwise}
\end{cases}
$$

Often surprisingly effective.

---

## Python Example

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(
    strategy="median"
)

X = imputer.fit_transform(X)
```

---

# Numerical Feature Scaling

## Why Scaling Matters

Features may exist on different scales.

Example:

```text
Income = 100000

Age = 30
```

Income dominates distance calculations.

---

# Standardization (Z-Score)

Most common.

$$
z
=
\frac{x-\mu}{\sigma}
$$

Produces:

$$
\mu=0
$$

$$
\sigma=1
$$

---

## Python Example

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

---

# Min-Max Scaling

Transforms:

$$
x'
=
\frac{x-x_{min}}
{x_{max}-x_{min}}
$$

Produces:

$$
0 \le x' \le 1
$$

---

## Python Example

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
```

---

# When Scaling Matters

| Model | Scaling Important? |
|---------|---------|
| Linear Regression | Yes |
| Logistic Regression | Yes |
| KNN | Yes |
| SVM | Yes |
| Neural Networks | Yes |
| PCA | Yes |
| Random Forest | Usually No |
| XGBoost | Usually No |
| LightGBM | Usually No |

---

# Categorical Encoding

Most ML models require numerical input.

---

# One-Hot Encoding

Example:

```text
Color

Red
Blue
Green
```

becomes

| Red | Blue | Green |
|-------|-------|-------|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

---

## Python Example

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(
    handle_unknown="ignore"
)
```

---

# Label Encoding

Example:

```text
Red   -> 0
Blue  -> 1
Green -> 2
```

Danger:

Model may incorrectly assume ordering.

---

# Frequency Encoding

Replace category with frequency.

Example:

```text
NYC = 1200

LA = 800

Chicago = 500
```

Useful for high-cardinality variables.

---

# Target Encoding

Replace category with average target.

Example:

```text
NYC -> Average Sale Price
```

Powerful but dangerous.

Can create leakage.

Must use out-of-fold encoding.

---

# Log Transformations

## Why?

Many variables are heavily skewed.

Example:

```text
Income
Population
Sales
Transaction Size
```

---

## Transformation

$$
x'
=
\log(x)
$$

Often:

$$
x'
=
\log(1+x)
$$

---

## Benefits

- Reduces skewness
- Stabilizes variance
- Makes linear relationships easier

---

## Python Example

```python
df["income_log"] = np.log1p(
    df["income"]
)
```

---

# Polynomial Features

Linear models cannot naturally learn:

$$
y=x^2
$$

Add:

$$
x^2
$$

as a feature.

---

## Example

Original:

$$
x
$$

Engineered:

$$
x
$$

$$
x^2
$$

$$
x^3
$$

---

## Python Example

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(
    degree=2
)

X_poly = poly.fit_transform(X)
```

---

# Interaction Features

Sometimes two variables together matter.

Original:

$$
Age
$$

$$
Income
$$

Interaction:

$$
Age \times Income
$$

---

## Example

```python
df["age_income"] = (
    df["age"] *
    df["income"]
)
```

---

# Binning

Convert continuous variables into buckets.

Example:

```text
Age

0-18
18-35
35-50
50+
```

Useful when relationships are nonlinear.

---

# Aggregation Features

Extremely important in production ML.

---

## Example

Transactions Table

```text
Customer
Amount
```

Create:

```text
Average Transaction
Maximum Transaction
Total Spend
Transaction Count
```

---

## Python Example

```python
customer_stats = (
    df.groupby("customer")
      .agg({
          "amount":[
              "mean",
              "sum",
              "max",
              "count"
          ]
      })
)
```

---

# Date and Time Features

Raw timestamps are rarely useful.

---

## Extract Components

From:

```text
2025-01-15 13:45:00
```

Create:

```text
Year
Month
Day
Weekday
Hour
Quarter
```

---

## Python Example

```python
df["month"] = df.date.dt.month
df["weekday"] = df.date.dt.weekday
df["hour"] = df.date.dt.hour
```

---

# Cyclical Features

Months wrap around.

December and January are close.

Normal encoding fails.

---

## Sine/Cosine Encoding

Month:

$$
\sin
\left(
\frac{2\pi m}{12}
\right)
$$

$$
\cos
\left(
\frac{2\pi m}{12}
\right)
$$

---

## Python Example

```python
df["month_sin"] = np.sin(
    2*np.pi*df.month/12
)

df["month_cos"] = np.cos(
    2*np.pi*df.month/12
)
```

---

# Time Series Feature Engineering

# Lag Features

Most important forecasting feature.

Create:

$$
X_{t-1}
$$

$$
X_{t-2}
$$

$$
X_{t-7}
$$

$$
X_{t-30}
$$

---

## Python Example

```python
df["lag_1"] = y.shift(1)
df["lag_7"] = y.shift(7)
```

---

# Rolling Statistics

Capture trend and momentum.

---

## Rolling Mean

$$
\frac1k
\sum_{i=1}^{k}
X_{t-i}
$$

---

## Rolling Standard Deviation

Measures volatility.

---

## Python Example

```python
df["roll_mean_7"] = (
    y.rolling(7)
     .mean()
)

df["roll_std_7"] = (
    y.rolling(7)
     .std()
)
```

---

# Expanding Features

Use all previous observations.

Example:

$$
\bar X_t
=
\frac1t
\sum_{i=1}^{t}
X_i
$$

---

## Python Example

```python
df["exp_mean"] = (
    y.expanding()
     .mean()
)
```

---

# Domain-Specific Features

Often the most valuable features.

Examples:

Finance:

```text
Moving Averages
Momentum
Volatility
Volume Ratios
```

---

Marketing:

```text
Customer Lifetime Value
Days Since Last Purchase
Purchase Frequency
```

---

Sports:

```text
Recent Win Rate
Average Points
Injury Indicators
```

---

# Dimensionality Reduction

# Principal Component Analysis (PCA)

Projects features onto:

$$
\text{Maximum Variance Directions}
$$

Useful when:

```text
Hundreds or thousands of features
```

---

## Python Example

```python
from sklearn.decomposition import PCA

pca = PCA(
    n_components=10
)

X_pca = pca.fit_transform(X)
```

---

# Feature Selection

Removing useless features often improves performance.

---

# Filter Methods

Rank features individually.

Examples:

- Correlation
- Mutual Information
- ANOVA

---

## Python Example

```python
from sklearn.feature_selection import mutual_info_regression

scores = mutual_info_regression(
    X,
    y
)
```

---

# Wrapper Methods

Evaluate subsets of features.

Examples:

- Forward Selection
- Backward Elimination
- Recursive Feature Elimination (RFE)

---

## Python Example

```python
from sklearn.feature_selection import RFE

selector = RFE(
    estimator=model,
    n_features_to_select=20
)
```

---

# Embedded Methods

Feature selection built into training.

Examples:

- Lasso
- Random Forest
- XGBoost

---

# Feature Importance

## Tree-Based Models

Can estimate importance.

Example:

```python
model.feature_importances_
```

---

## SHAP

Industry standard.

Measures contribution of each feature.

```python
import shap

explainer = shap.TreeExplainer(model)

shap_values = explainer(X)
```

---

# Leakage (Most Important Topic)

## Definition

Using information unavailable at prediction time.

---

## Example

Predicting:

```text
Will customer churn?
```

Using:

```text
Cancellation Date
```

creates leakage.

Model becomes unrealistic.

---

# Time Series Leakage

Common mistake:

```python
rolling_mean = (
    y.rolling(7)
     .mean()
)
```

includes current value.

Correct:

```python
rolling_mean = (
    y.shift(1)
     .rolling(7)
     .mean()
)
```

Only past information.

---

# Industry Best Practices

# Start Simple

Always begin with:

- Raw features
- Basic transformations

before creating hundreds of features.

---

# Understand the Business Problem

Best features usually come from domain knowledge.

---

# Build Features Using Only Available Information

Ask:

```text
Would this feature exist
at prediction time?
```

If not:

```text
Leakage
```

---

# Feature Engineering Before Hyperparameter Tuning

Often:

```text
Better Features
>
Better Model
```

---

# Track Every Feature

Maintain documentation:

```text
Feature Name
Definition
Data Source
Update Frequency
```

Production teams rely heavily on feature catalogs.

---

# Reuse Features

Modern organizations build:

```text
Feature Store
```

Examples:

- Feast
- Tecton
- Databricks Feature Store

---

# Common Pipeline Example

```python
from sklearn.pipeline import Pipeline

pipeline = Pipeline([
    ("imputer", SimpleImputer()),
    ("scaler", StandardScaler()),
    ("model", RandomForestRegressor())
])
```

Prevents train/test inconsistencies.

---

# Real-World Feature Engineering Workflow

```text
Raw Data
      ↓
Data Cleaning
      ↓
Missing Value Handling
      ↓
Encoding
      ↓
Scaling / Transformations
      ↓
Aggregation Features
      ↓
Time Features
      ↓
Lag / Rolling Features
      ↓
Feature Selection
      ↓
Model Training
      ↓
Feature Importance Analysis
      ↓
Iterate
```

# Common Mistakes

## Creating Leakage

Most dangerous mistake.

---

## Scaling Before Train/Test Split

Causes information leakage.

Fit scaler only on training data.

---

## Blindly Creating Thousands of Features

Creates:

- Overfitting
- Longer training
- Harder interpretation

---

## Ignoring Domain Knowledge

Often the highest-value features come from understanding the problem.

---

## Overusing PCA

PCA improves compression but reduces interpretability.

Use only when necessary.

# Key Takeaways

- Feature engineering is often the highest-leverage part of machine learning.
- Better features frequently outperform better models.
- Handle missing values, categorical variables, and scaling appropriately.
- Time series models depend heavily on lag and rolling features.
- Leakage prevention is critical.
- Domain-specific features often produce the largest performance gains.
- Feature selection reduces noise and improves generalization.
- Production ML systems typically spend more effort on feature engineering than model tuning.
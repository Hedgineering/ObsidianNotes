# Data Cleaning & Exploratory Data Analysis (EDA) Quick Reference

# Overall Workflow

1. Understand business problem
2. Inspect schema & data types
3. Check missing values
4. Check duplicates
5. Check distributions
6. Check target variable
7. Check relationships
8. Check leakage
9. Engineer features
10. Select model

---

# First Things To Check

## Dataset Shape

```python
df.shape
df.info()
df.describe()
```

Questions:

- How many rows?
- How many columns?
- Numerical vs categorical?
- Missing values?
- Target imbalance?

---

## Data Quality Checks

Check:

- Missing values
- Duplicates
- Invalid values
- Impossible values
- Outliers
- Unit inconsistencies
- Data leakage

Examples:

```text
Age = -5
Price = $0
Future information available today
```

---

# Missing Data

# Types of Missingness

## MCAR

Missing Completely At Random

Examples:

- Sensor randomly failed
- Survey page crashed

Safe approaches:

- Drop rows
- Impute

Least dangerous.

---

## MAR

Missing At Random

Missing depends on observed variables.

Example:

```text
Income missing more often
for younger users
```

Given age, missingness is random.

Use:

- Imputation
- Modeling

Common case.

---

## MNAR

Missing Not At Random

Missingness itself contains information.

Examples:

```text
Income omitted by wealthy people
```

```text
Patients skip embarrassing questions
```

Dropping rows creates bias.

Often create:

```python
missing_income_flag
```

as a feature.

---

# Missing Numerical Values

## Mean Imputation

```python
df[col].fillna(df[col].mean())
```

Use when:

- Roughly symmetric
- Few missing values

Avoid when:

- Skewed data

---

## Median Imputation

```python
fillna(median)
```

Most common.

Robust to outliers.

Preferred for:

- Income
- Price
- Revenue

---

## Constant Value

```python
fillna(0)
fillna(-1)
```

Use only if value has meaning.

Common:

```text
0 purchases
0 balances
```

Danger:

Missing ≠ zero.

---

## Forward Fill

```python
ffill()
```

Use when:

- Time series
- Slowly changing signals

Examples:

- Temperature
- Inventory
- Stock positions

Never use if values can change rapidly.

---

## Backfill

```python
bfill()
```

Uses future information.

Safe only if:

- Offline analysis
- Non-causal setting

Dangerous for forecasting.

---

## Interpolation

```python
interpolate()
```

Use when:

- Smooth process
- Physical measurements

Examples:

- Temperature
- Sensor readings

Avoid for:

- Categorical data
- Event data

---

## Model-Based Imputation

Examples:

- KNN Imputer
- MICE
- Regression

Use when:

- Large datasets
- Missingness substantial

---

# Missing Categorical Values

## Most Frequent Category

```python
fillna(mode)
```

Common baseline.

---

## Create "Missing" Category

```python
fillna("Unknown")
```

Often preferred.

Preserves information.

Useful for:

- User demographics
- Surveys
- Forms

---

## Missing Indicator

```python
is_missing
```

Useful when:

Missingness may be predictive.

---

# Missing Boolean Values

Options:

```python
True
False
Unknown
```

Often safest:

```python
fillna("Unknown")
```

rather than forcing True/False.

---

# Missing Date Values

Options:

## Drop

When few rows affected.

---

## Forward Fill

Common for:

- Account status
- Inventory state

---

## Use Sentinel Date

Example:

```python
1900-01-01
```

or

```python
2099-12-31
```

with indicator flag.

---

# When To Drop Rows

Safe when:

```text
<5% missing
MCAR
Large dataset
```

Dangerous when:

```text
MNAR
Small dataset
```

---

# Outlier Detection

# Numerical Features

## Z-Score

```python
|z| > 3
```

Assumes roughly normal.

---

## IQR Rule

```python
Q1 - 1.5*IQR
Q3 + 1.5*IQR
```

Most common.

Robust.

---

## Percentile Clipping

```python
1st percentile
99th percentile
```

Very common in finance.

---

# Should You Remove Outliers?

Ask:

Is it:

- Error?
- Rare but real event?

---

Remove:

```text
Age = 300
```

Keep:

```text
Million-dollar transaction
```

if real.

---

# Winsorization

Instead of removing:

```python
clip()
```

Example:

```text
Replace top 1%
with 99th percentile
```

Useful for:

- Linear models
- Finance

---

# Handling Zeros

Ask:

Does zero mean:

1. True zero?
2. Missing?
3. Impossible?

---

Examples

Valid:

```text
0 purchases
0 children
```

Invalid:

```text
0 income
```

depending on domain.

---

# Transformations

# Log Transform

## Use When

Strong right skew.

Examples:

- Income
- Revenue
- Volume

```python
np.log1p(x)
```

---

Benefits:

- Reduces outliers
- Stabilizes variance
- Makes linear models happier

---

# Box-Cox

Positive values only.

Makes distributions more normal.

---

# Yeo-Johnson

Like Box-Cox but handles negatives.

---

# Feature Distribution Guide

## Normal-ish

Good for:

- Linear Regression
- Logistic Regression
- LDA

---

## Heavy-Tailed

Consider:

- Log transform
- Tree models

---

## Highly Nonlinear

Consider:

- XGBoost
- Random Forest
- Neural Nets

---

# Categorical Variables

# Cardinality

## Low Cardinality

Examples:

```text
Gender
State
Device
```

Use:

- One Hot Encoding

---

## High Cardinality

Examples:

```text
Zip Code
Customer ID
```

Use:

- Target encoding
- Embeddings
- Frequency encoding

Avoid giant OHE matrices.

---

# Target Variable EDA

# Classification

Check:

```python
y.value_counts()
```

---

## Class Imbalance

Examples:

```text
99% negative
1% positive
```

Use:

- Class weights
- Resampling
- PR-AUC

Not accuracy.

---

# Regression

Inspect:

```python
sns.histplot(y)
```

Check:

- Skewness
- Outliers
- Heavy tails

---

# Visualization Cheat Sheet with Example Input Shapes

Assume:

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

Example dataset:

| age | income | balance | credit_score | segment | defaulted | date       | sales |
|-----|---------|---------|-------------|---------|-----------|------------|-------|
| 25  | 50000   | 2000    | 720         | A       | 0         | 2025-01-01 | 100   |
| 32  | 75000   | 4500    | 680         | B       | 1         | 2025-01-02 | 120   |
| 41  | 90000   | 7000    | 760         | A       | 0         | 2025-01-03 | 140   |
| 28  | 55000   | 1000    | 650         | C       | 1         | 2025-01-04 | 95    |

Typical meanings:

```text
age, income, balance, credit_score = numerical

segment = categorical

defaulted = binary target

date = datetime

sales = time series value
```

---

# Histogram

Use for:

- Distribution
- Skewness
- Outliers

Input:

```python
df["income"]

0    50000
1    75000
2    90000
3    55000
```

Code:

```python
sns.histplot(data=df, x="income", bins=30)
plt.show()
```

Interpretation:

```text
Symmetric
→ Linear models happy

Right-skewed
→ Consider log transform

Multiple peaks
→ Multiple populations

Long tails
→ Investigate outliers
```

---

# KDE

Use for:

- Smooth distribution estimate

Input:

```python
df["income"]
```

Code:

```python
sns.kdeplot(data=df, x="income")
plt.show()
```

Multiple groups:

```python
sns.kdeplot(
    data=df,
    x="income",
    hue="defaulted"
)
```

Interpretation:

```text
Separated curves
→ Feature may predict target

Overlapping curves
→ Weak predictive power
```

---

# Boxplot

Use for:

- Outlier detection
- Group comparison

Input:

```python
x = segment

A A B B C C

y = income

50000
60000
70000
75000
40000
45000
```

Code:

```python
sns.boxplot(
    data=df,
    x="segment",
    y="income"
)
```

Interpretation:

```text
Large boxes
→ High variance

Extreme dots
→ Outliers

Different medians
→ Potential signal
```

---

# Violin Plot

Use for:

- Distribution shape by group

Input:

Same as boxplot.

Code:

```python
sns.violinplot(
    data=df,
    x="segment",
    y="income"
)
```

Interpretation:

```text
Wide sections
→ High density

Multiple bulges
→ Multimodal distribution
```

---

# Scatterplot

Use for:

- Numerical vs numerical

Input:

```python
age      income

25       50000
32       75000
41       90000
28       55000
```

Code:

```python
sns.scatterplot(
    data=df,
    x="age",
    y="income"
)
```

With class labels:

```python
sns.scatterplot(
    data=df,
    x="age",
    y="income",
    hue="defaulted"
)
```

Interpretation:

```text
Straight trend
→ Linear model

Curved trend
→ Polynomial / Trees

Clusters
→ Segmentation opportunity
```

---

# Regression Plot

Scatter + trend line.

Input:

```python
age vs income
```

Code:

```python
sns.regplot(
    data=df,
    x="age",
    y="income"
)
```

Interpretation:

```text
Strong line
→ Good linear relationship

Large residual spread
→ Weak predictor
```

---

# Pairplot

Use for:

- Multiple numerical variables

Input:

```python
df[
    [
        "age",
        "income",
        "balance",
        "credit_score"
    ]
]
```

Shape:

```python
(n_rows, n_features)
```

Example:

```python
(1000, 4)
```

Code:

```python
sns.pairplot(
    df[
        [
            "age",
            "income",
            "balance",
            "credit_score"
        ]
    ]
)
```

With target:

```python
sns.pairplot(
    df[
        [
            "age",
            "income",
            "balance",
            "credit_score",
            "defaulted"
        ]
    ],
    hue="defaulted"
)
```

Interpretation:

```text
Strong separation
→ Predictive features

Strong linearity
→ Linear models

Complex boundaries
→ Trees / XGBoost
```

---

# Correlation Heatmap

Use for:

- Multicollinearity
- Redundant features

Input:

```python
df.select_dtypes("number")
```

Example:

```python
age
income
balance
credit_score
sales
```

Code:

```python
corr = df.select_dtypes(
    include="number"
).corr()

sns.heatmap(
    corr,
    annot=True,
    cmap="coolwarm"
)
```

Interpretation:

```text
0.9+
→ Near duplicate features

0.7+
→ Potential multicollinearity

0
→ No linear relationship
```

Common action:

```text
Drop one feature
Use Ridge
Use PCA
```

---

# Count Plot

Use for:

- Category frequencies

Input:

```python
segment

A
A
B
C
C
C
```

Code:

```python
sns.countplot(
    data=df,
    x="segment"
)
```

Interpretation:

```text
Imbalanced categories

Rare categories

Data quality issues
```

---

# Bar Plot

Use for:

- Aggregated statistics

Input:

```python
segment

A
B
C

average income

60000
80000
45000
```

Code:

```python
sns.barplot(
    data=df,
    x="segment",
    y="income"
)
```

Interpretation:

```text
Compare means

Compare target rates

Identify strong predictors
```

For classification:

```python
sns.barplot(
    data=df,
    x="segment",
    y="defaulted"
)
```

Output:

```text
Default rate by segment
```

---

# Time Series Plot

Use for:

- Trend
- Seasonality
- Regime changes

Input:

```python
date         sales

2025-01-01   100
2025-01-02   120
2025-01-03   140
2025-01-04   95
```

Code:

```python
sns.lineplot(
    data=df,
    x="date",
    y="sales"
)
```

Interpretation:

```text
Upward trend

Seasonality

Structural breaks

Volatility clustering
```

---

# Rolling Average Plot

Use for:

- Noise reduction

Input:

```python
date
sales
```

Code:

```python
df["sales_ma"] = (
    df["sales"]
    .rolling(7)
    .mean()
)

sns.lineplot(
    data=df,
    x="date",
    y="sales"
)

sns.lineplot(
    data=df,
    x="date",
    y="sales_ma"
)
```

Interpretation:

```text
Underlying trend

Seasonality

Regime shifts
```

---

# Model Selection Clues From EDA

| Observation | Likely Model |
|------------|-------------|
| Linear trend | Linear Regression |
| Binary target + linear separation | Logistic Regression |
| Strong nonlinear relationships | XGBoost / Random Forest |
| Many correlated features | Ridge / PCA |
| High-dimensional sparse categories | Trees / Embeddings |
| Time trend | ARIMA / Prophet / Forecasting Models |
| Complex interactions | Gradient Boosting |
| Clear clusters | Clustering (KMeans, GMM) |

---

# Quick EDA Checklist

For every feature ask:

```text
1. Missing values?

2. Distribution shape?

3. Outliers?

4. Skewed?

5. Correlated?

6. Predictive?

7. Stable through time?

8. Leakage risk?

9. Requires scaling?

10. Requires transformation?
```

A strong EDA session should allow you to answer:

```text
What should I clean?

What should I transform?

What should I encode?

What should I drop?

What model family is most appropriate?
```

---

# Correlation vs Modeling

# Strong Linear Relationship

Consider:

- Linear Regression
- GLM

---

# Weak Linear, Strong Nonlinear

Consider:

- Trees
- Boosting
- Neural Networks

---

# Multicollinearity

Symptoms:

```python
corr > 0.9
```

or

High VIF.

Problems:

- Unstable coefficients
- Inflated variance

---

Solutions:

- Remove variables
- PCA
- Ridge

---

# Missingness as Signal

Sometimes:

```text
Missing ≠ Missing
```

Examples:

- Income not reported
- Medical test skipped
- Optional profile fields

Always ask:

```text
Does the fact that it is missing
contain information?
```

If yes:

Create:

```python
missing_flag
```

---

# Logistic vs Probit

Both model:

```text
P(Y=1)
```

---

## Logistic

Most common.

Outputs:

```text
Odds ratios
```

Easy interpretation.

Preferred default.

---

## Probit

Assumes latent Gaussian process.

Common in:

- Economics
- Finance
- Credit risk

---

# Missingness + Logistic Models

Danger:

Complete-case analysis:

```python
dropna()
```

can introduce bias.

Especially under:

```text
MAR
MNAR
```

Consider:

- Imputation
- Missing indicators
- Sensitivity analysis

---

# Time Series Missing Data

# Safe

Forward fill:

```text
Inventory
Account balance
Position size
```

---

# Usually Unsafe

Forward fill:

```text
Price
Returns
Volume
```

Can create fake signals.

---

# Interpolation Safe

Examples:

- Temperature
- Sensor readings
- Physical processes

---

# Interpolation Dangerous

Examples:

- Trades
- Orders
- User events
- Clicks

---

# EDA Questions To Always Answer

1. What does each feature represent?
2. Why is data missing?
3. Is missingness informative?
4. Are there outliers?
5. Are outliers errors?
6. Is target balanced?
7. Are variables skewed?
8. Are relationships linear?
9. Is there multicollinearity?
10. Is there leakage?
11. Is data stationary (time series)?
12. What assumptions will my model make?
13. Does preprocessing violate causality?
14. Can future information leak into training?

---

# Golden Rule

Before choosing a model:

Understand

- Missingness
- Distribution shape
- Outliers
- Time structure
- Leakage risks

These usually matter more than the choice between XGBoost, Random Forest, Logistic Regression, or Neural Networks.
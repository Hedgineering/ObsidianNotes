
## Quick Reference Table

  
| Method | Penalty Added to Loss | Effect | Common Use Cases |
| ------ | --------------------- | ------ | ---------------- |
| Ridge (L2) | $\lambda \sum_j \beta_j^2$ | Shrinks coefficients toward zero | Multicollinearity, many correlated features |  
| Lasso (L1) | $\lambda \sum_j \vert \beta_j \vert$ | Shrinks coefficients, can set some exactly to zero | Feature selection |  
| Elastic Net | $\lambda_1 \sum_j \vert \beta_j \vert + \lambda_2 \sum_j \beta_j^2$ | Combines Ridge and Lasso | Many correlated predictors |  
| Weight Decay | $\lambda \Vert W \Vert^2$ | Neural network equivalent of Ridge | Deep learning |  
| Early Stopping | Stop training before convergence | Prevents overfitting | Neural networks, boosting |  
| Dropout | Randomly disable neurons during training | Prevents co-adaptation | Deep learning |  
| Data Augmentation | Artificially expand dataset | Improves generalization | Vision, NLP, audio |  
| Tree Depth Limits | Restrict tree complexity | Reduces overfitting | Decision trees, Random Forests |  
| Pruning | Remove weak tree branches | Simplifies model | Decision trees |  
| Bagging | Average many models | Reduces variance | Random Forest |  
| Boosting Regularization | Learning rate, subsampling | Controls complexity | XGBoost, LightGBM |

---

# Intuition

## Why Regularization Exists

Machine learning models try to minimize training error.

Without constraints, a sufficiently flexible model can memorize noise.

Example:

Suppose the true relationship is

$$
y = x + \varepsilon
$$

where $\varepsilon$ is random noise.

A highly flexible model might learn

$$
y
=
x
+
\text{noise pattern}
$$

instead of the true signal.

This produces:

- Extremely low training error
- Poor test performance

Regularization intentionally discourages overly complex models.

The goal is:

$$
\text{Generalization}
>
\text{Training Accuracy}
$$

---

# Relationship to Bias-Variance Tradeoff

Recall:

$$
\text{Prediction Error}
=
\text{Bias}^2
+
\text{Variance}
+
\text{Noise}
$$

Regularization:

- Increases bias slightly
- Decreases variance substantially

Often reducing overall error.

Visual intuition:

```text
No Regularization

Train Error  ↓↓↓
Test Error   ↑↑↑

Strong Regularization

Train Error  ↑
Test Error   ↓
```

---

# General Form

Most ML models minimize:

$$
\text{Loss}
=
\text{Data Loss}
+
\text{Penalty}
$$

or

$$
L(\theta)
=
L_{\text{data}}
+
\lambda R(\theta)
$$

where:

- $L_{\text{data}}$ = prediction error
- $R(\theta)$ = regularization term
- $\lambda$ = regularization strength

---

# Ridge Regression (L2)

## Definition

Ordinary Least Squares minimizes

$$
RSS
=
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
$$

Ridge minimizes

$$
RSS
+
\lambda
\sum_{j=1}^{p}
\beta_j^2
$$

---

## Intuition

Large coefficients are penalized.

Instead of:

$$
\beta
=
[100,-80,95]
$$

Ridge encourages:

$$
\beta
=
[2.1,-1.8,1.7]
$$

The model still uses all features but with smaller weights.

---

## Geometric Interpretation

Constraint:

$$
\sum_j \beta_j^2
\le c
$$

forms a sphere.

Optimization searches for:

$$
\min RSS
$$

inside this sphere.

---

## When to Use Ridge

Use when:

- Many predictors
- Predictors are correlated
- Feature selection is not required
- Prediction accuracy is primary goal

---

## Python Example

```python
from sklearn.linear_model import Ridge

model = Ridge(alpha=1.0)

model.fit(X_train, y_train)

preds = model.predict(X_test)
```

Note:

```python
alpha = lambda
```

in scikit-learn terminology.

---

# Lasso Regression (L1)

## Definition

Lasso minimizes

$$
RSS
+
\lambda
\sum_j |\beta_j|
$$

---

## Intuition

Unlike Ridge:

Lasso can force coefficients exactly to zero.

Example:

Before:

$$
[4.2, 0.8, 2.3, 0.5, 1.1]
$$

After:

$$
[4.0, 0, 2.0, 0, 0]
$$

This performs automatic feature selection.

---

## Geometric Interpretation

Constraint:

$$
\sum_j |\beta_j|
\le c
$$

creates a diamond-shaped region.

Corners increase the probability that coefficients become exactly zero.

---

## When to Use Lasso

Use when:

- Many features
- Sparse solution desired
- Feature selection important

---

## Python Example

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=0.01)

model.fit(X_train, y_train)

preds = model.predict(X_test)
```

---

# Elastic Net

## Motivation

Ridge:

- Handles correlated variables well

Lasso:

- Performs feature selection

Elastic Net combines both.

---

## Objective

$$
RSS
+
\lambda_1
\sum_j |\beta_j|
+
\lambda_2
\sum_j \beta_j^2
$$

---

## When to Use

Especially useful when:

- Thousands of predictors
- Highly correlated features
- Need feature selection

---

## Python Example

```python
from sklearn.linear_model import ElasticNet

model = ElasticNet(
    alpha=0.1,
    l1_ratio=0.5
)

model.fit(X_train, y_train)
```

where:

$$
l1\_ratio
=
\frac{\text{L1 contribution}}
{\text{Total penalty}}
$$

---

# Choosing Lambda

## Small Lambda

$$
\lambda \approx 0
$$

Behavior:

- Almost no regularization
- High variance
- Overfitting risk

---

## Large Lambda

$$
\lambda \rightarrow \infty
$$

Behavior:

- Very simple model
- High bias
- Underfitting risk

---

## Selecting Lambda with Cross Validation

Most common approach:

```python
from sklearn.linear_model import RidgeCV

model = RidgeCV(
    alphas=[0.01,0.1,1,10,100],
    cv=5
)

model.fit(X_train, y_train)

print(model.alpha_)
```

---

# Regularization in Logistic Regression

Objective:

$$
-\log L
+
\lambda R(\beta)
$$

where:

$$
R(\beta)
=
\sum_j \beta_j^2
$$

or

$$
R(\beta)
=
\sum_j |\beta_j|
$$

---

## Python Example

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    penalty="l2"
)

model.fit(X_train, y_train)
```

For L1:

```python
model = LogisticRegression(
    penalty="l1",
    solver="liblinear"
)
```

---

# Regularization in Decision Trees

Trees do not use coefficient penalties.

Instead complexity is controlled by:

- Maximum depth
- Minimum samples per split
- Minimum samples per leaf
- Cost complexity pruning

---

## Example

Without restrictions:

```text
Depth = 50
```

Tree memorizes training data.

With restrictions:

```text
Depth = 5
```

Tree learns broader patterns.

---

## Python Example

```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier(
    max_depth=5,
    min_samples_leaf=20
)
```

---

# Random Forest Regularization

Random Forest reduces overfitting through:

## Bagging

Train many trees:

$$
T_1,T_2,\ldots,T_B
$$

Average:

$$
\hat y
=
\frac1B
\sum_{b=1}^{B}
T_b
$$

Variance decreases.

---

## Random Feature Selection

Each split sees only a subset of variables.

This decorrelates trees.

---

## Example

```python
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor(
    max_depth=10,
    min_samples_leaf=10,
    n_estimators=500
)
```

---

# Gradient Boosting Regularization

Gradient boosting can easily overfit.

Common controls:

## Learning Rate

Update:

$$
F_m
=
F_{m-1}
+
\eta h_m
$$

Small $\eta$:

- Slower learning
- Better generalization

---

## Tree Depth

Typical values:

```text
3 to 10
```

---

## Subsampling

Use only part of the dataset.

Example:

```text
80% rows
```

per boosting round.

---

## XGBoost Example

```python
import xgboost as xgb

model = xgb.XGBRegressor(
    learning_rate=0.05,
    max_depth=4,
    subsample=0.8,
    colsample_bytree=0.8
)
```

---

# Neural Network Regularization

Neural networks can contain millions of parameters.

Regularization becomes critical.

---

# Weight Decay (L2)

Loss:

$$
L
=
L_{\text{data}}
+
\lambda \|W\|^2
$$

Equivalent to Ridge.

---

## PyTorch Example

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=1e-3,
    weight_decay=1e-4
)
```

---

# Dropout

Randomly remove neurons during training.

Example:

```text
100 neurons
↓
keep only 80
```

during each iteration.

---

## Intuition

Prevents neurons from becoming overly dependent on one another.

Acts like training many subnetworks simultaneously.

---

## PyTorch Example

```python
nn.Dropout(p=0.2)
```

Meaning:

$$
20\%
$$

of neurons disabled each batch.

---

# Early Stopping

Stop training when validation error stops improving.

---

## Example

```text
Epoch 20:
Validation Loss = 0.15

Epoch 30:
Validation Loss = 0.16
```

Stop around epoch 20.

---

## Keras Example

```python
from tensorflow.keras.callbacks import EarlyStopping

callback = EarlyStopping(
    patience=10,
    restore_best_weights=True
)
```

---

# Batch Normalization

Normalizes activations.

For batch:

$$
x
\rightarrow
\frac{x-\mu}
{\sigma}
$$

Benefits:

- Faster training
- Slight regularization effect

---

# Data Augmentation

Create synthetic observations.

Examples:

Images:

- Rotate
- Flip
- Crop

Audio:

- Noise injection
- Time shifting

Text:

- Synonym replacement

Effectively increases sample size.

---

# Time Series Regularization

Additional challenges:

- Temporal dependence
- Nonstationarity

---

## Common Approaches

### Smaller Lag Sets

Instead of:

$$
X_{t-1}
,\ldots,
X_{t-500}
$$

Use:

$$
X_{t-1}
,\ldots,
X_{t-20}
$$

---

### Ridge / Lasso on Lag Features

Very common.

Example:

```python
from sklearn.linear_model import Ridge

ridge.fit(X_lags, y)
```

---

### Early Stopping for Boosting

Common in forecasting.

```python
model.fit(
    X_train,
    y_train,
    eval_set=[(X_val,y_val)],
    early_stopping_rounds=50
)
```

---

# Practical Workflow

## Linear Models

Try:

1. OLS
2. Ridge
3. Lasso
4. Elastic Net

Compare with cross validation.

---

## Tree Models

Tune:

```text
max_depth
min_samples_leaf
min_samples_split
```

---

## Boosting

Tune:

```text
learning_rate
max_depth
subsample
colsample_bytree
```

---

## Neural Networks

Tune:

```text
weight_decay
dropout
early_stopping
batch_size
```

---

# Common Mistakes

## Forgetting Feature Scaling

Ridge and Lasso are sensitive to scale.

Always standardize:

```python
from sklearn.preprocessing import StandardScaler
```

before fitting.

---

## Using Lasso with Highly Correlated Features

Lasso may arbitrarily keep one variable and remove another.

Elastic Net is often safer.

---

## Selecting Lambda on Test Data

Incorrect:

```text
Train
→ Test
→ Pick Best Lambda
```

Correct:

```text
Train
→ Validation
→ Select Lambda
→ Final Test
```

or use cross validation.

---

## Excessive Regularization

Too much regularization produces:

$$
\text{High Bias}
$$

and underfitting.

Always inspect:

- Training error
- Validation error
- Test error

simultaneously.

# Key Takeaways

- Regularization controls model complexity.
- The goal is improved generalization, not perfect training accuracy.
- Ridge shrinks coefficients.
- Lasso shrinks and performs feature selection.
- Elastic Net combines both.
- Tree-based models regularize through structural constraints.
- Neural networks rely heavily on weight decay, dropout, and early stopping.
- Cross-validation is the standard method for choosing regularization strength.
- Most production ML systems use some form of regularization, even if it is not explicitly called "regularization."
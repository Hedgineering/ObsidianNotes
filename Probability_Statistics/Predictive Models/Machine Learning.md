# Machine Learning (ML) Overview

## Quick Reference Table

| Model / Method | Category | Best Used For | Strengths | Weaknesses |
|----------|----------|----------|----------|----------|
| Linear Regression | Supervised | Continuous prediction | Fast, interpretable | Linear relationships only |
| Ridge Regression | Supervised | Regression with multicollinearity | Stable coefficients | Still linear |
| Lasso Regression | Supervised | Feature selection | Sparse models | Can underfit |
| Elastic Net | Supervised | Many correlated predictors | Combines Ridge + Lasso | Hyperparameter tuning |
| Logistic Regression | Supervised | Binary classification | Fast, interpretable | Linear decision boundary |
| Naive Bayes | Supervised | Text classification, spam | Extremely fast | Strong independence assumptions |
| KNN | Supervised | Small datasets | Simple, non-parametric | Slow at prediction |
| Decision Tree | Supervised | Classification/regression | Interpretable | Overfitting |
| Random Forest | Ensemble | General purpose tabular ML | Strong baseline | Less interpretable |
| Extra Trees | Ensemble | High-dimensional tabular data | Fast, robust | Less interpretable |
| Gradient Boosting | Ensemble | Tabular data | Strong predictive power | Slower training |
| XGBoost | Ensemble | Structured/tabular data | Industry standard | Many hyperparameters |
| LightGBM | Ensemble | Large tabular datasets | Very fast | Can overfit |
| CatBoost | Ensemble | Categorical-heavy data | Minimal preprocessing | Slower than LightGBM |
| SVM | Supervised | Medium-sized classification problems | Effective in high dimensions | Doesn't scale well |
| Neural Networks | Supervised | Complex nonlinear patterns | Flexible | Data hungry |
| CNN | Deep Learning | Images, vision | State-of-the-art vision | Large datasets needed |
| RNN | Deep Learning | Sequential data | Captures sequence information | Training difficulties |
| LSTM | Deep Learning | Time series, NLP | Long-term memory | Slower |
| GRU | Deep Learning | Time series, NLP | Faster than LSTM | Slightly less expressive |
| Transformer | Deep Learning | Language, sequence modeling | SOTA for many domains | Computationally expensive |
| K-Means | Unsupervised | Clustering | Fast | Assumes spherical clusters |
| Hierarchical Clustering | Unsupervised | Cluster exploration | Dendrogram visualization | Expensive |
| DBSCAN | Unsupervised | Arbitrary-shaped clusters | Detects outliers | Sensitive to parameters |
| Gaussian Mixture Models | Unsupervised | Soft clustering | Probabilistic clusters | Assumes Gaussianity |
| PCA | Dimensionality Reduction | Feature compression | Simple, effective | Linear only |
| UMAP | Dimensionality Reduction | Visualization | Preserves local structure | Not interpretable |
| t-SNE | Dimensionality Reduction | Visualization | Excellent clusters | Slow |
| Autoencoders | Deep Learning | Representation learning | Nonlinear compression | Harder to train |
| Isolation Forest | Anomaly Detection | Outlier detection | Fast | May miss subtle anomalies |
| One-Class SVM | Anomaly Detection | Novelty detection | Flexible | Scaling issues |

---

# What is Machine Learning?

Machine Learning is the process of learning patterns from data in order to make predictions, classifications, decisions, or discover structure without explicitly programming every rule.

Instead of writing:

$$
\text{Price}
=
\text{Rule 1}
+
\text{Rule 2}
+
\text{Rule 3}
$$

we provide examples

$$
(X_i, y_i)
$$

and allow the algorithm to learn

$$
f(X)
\approx
y
$$

from the data.

---

# Main Categories of Machine Learning

## Supervised Learning

Input data contains both features and labels.

$$
(X, y)
$$

Goal:

$$
f(X)
\rightarrow
y
$$

Examples:

- House price prediction
- Credit risk
- Disease diagnosis
- Stock return prediction
- Demand forecasting

### Tasks

#### Regression

Predict a continuous value.

Examples:

$$
y
=
\text{House Price}
$$

$$
y
=
\text{Future Sales}
$$

#### Classification

Predict a category.

Examples:

$$
y \in \{0,1\}
$$

or

$$
y \in \{\text{Dog}, \text{Cat}, \text{Bird}\}
$$

---

## Unsupervised Learning

Only inputs are available.

$$
X
$$

No labels.

Goal:

- Find structure
- Find clusters
- Compress information
- Detect anomalies

Examples:

- Customer segmentation
- Market regime detection
- Topic modeling

---

## Reinforcement Learning

Agent learns through rewards.

At time $t$:

$$
s_t
\rightarrow
a_t
\rightarrow
r_t
$$

where:

- $s_t$ = state
- $a_t$ = action
- $r_t$ = reward

Applications:

- Robotics
- Games
- Trading execution
- Recommendation systems

---

# Common Supervised Learning Models

## Linear Regression

Assumes:

$$
y
=
\beta_0
+
\beta_1x_1
+
\cdots
+
\beta_px_p
+
\varepsilon
$$

Use when:

- Relationship roughly linear
- Need interpretability
- Baseline model

---

## Ridge Regression

Adds penalty:

$$
RSS
+
\lambda
\sum \beta_j^2
$$

Use when:

- Features highly correlated
- Want coefficient stability

---

## Lasso Regression

Adds:

$$
RSS
+
\lambda
\sum |\beta_j|
$$

Use when:

- Need automatic feature selection
- Many predictors

---

## Logistic Regression

Binary classification:

$$
P(Y=1)
=
\frac{1}
{1+e^{-z}}
$$

where

$$
z
=
\beta_0
+
\beta^T X
$$

Use when:

- Interpretable classification
- Probabilities needed

---

## Decision Trees

Recursive splitting:

$$
X_j < c
$$

Advantages:

- Handles nonlinearities
- Handles interactions
- Easy to visualize

---

## Random Forest

Many trees:

$$
\hat y
=
\frac{1}{B}
\sum_{b=1}^{B}
T_b(x)
$$

Use when:

- Tabular data
- Strong baseline needed
- Limited tuning time

---

## Gradient Boosting

Sequentially learns errors:

$$
F_m(x)
=
F_{m-1}(x)
+
\eta h_m(x)
$$

Popular implementations:

- XGBoost
- LightGBM
- CatBoost

Most commonly used for:

- Kaggle competitions
- Structured business data
- Quantitative finance signals

---

## Support Vector Machines

Maximize margin:

$$
\max \text{Margin}
$$

Use when:

- Moderate dataset size
- High-dimensional features

---

# Deep Learning Models

## Feed Forward Neural Networks

General function approximator:

$$
y
=
f(W_2 f(W_1x+b_1)+b_2)
$$

Use when:

- Complex nonlinear relationships

---

## Convolutional Neural Networks (CNN)

Primarily:

- Images
- Video
- Spatial data

Applications:

- Medical imaging
- Object detection
- Face recognition

---

## Recurrent Neural Networks (RNN)

Designed for sequences.

Input:

$$
x_1,x_2,\ldots,x_T
$$

Applications:

- Time series
- NLP

---

## LSTM

Adds memory cells.

Useful for:

- Long sequences
- Forecasting
- NLP

---

## GRU

Simplified LSTM.

Usually:

- Faster training
- Similar performance

---

## Transformers

Modern sequence architecture.

Core mechanism:

$$
\text{Attention}(Q,K,V)
=
\text{softmax}
\left(
\frac{QK^T}
{\sqrt d}
\right)V
$$

Applications:

- GPT
- BERT
- Modern forecasting
- Vision Transformers

---

# Unsupervised Learning Models

## K-Means

Objective:

$$
\min
\sum_{i=1}^{n}
\sum_{k=1}^{K}
r_{ik}
\|x_i-\mu_k\|^2
$$

Use when:

- Customer segmentation
- Regime detection
- Simple clustering

---

## Hierarchical Clustering

Builds cluster tree.

Useful when:

- Number of clusters unknown

---

## DBSCAN

Clusters dense regions.

Good for:

- Arbitrary cluster shapes
- Noise detection

---

## Gaussian Mixture Models

Assumes:

$$
f(x)
=
\sum_{k=1}^{K}
\pi_k
N(\mu_k,\Sigma_k)
$$

Useful for:

- Soft clustering
- Probabilistic assignments

---

# Dimensionality Reduction

## PCA

Projects data onto directions of maximum variance.

Find eigenvectors of:

$$
\Sigma
=
\frac1n X^TX
$$

Use when:

- Too many features
- Visualization
- Noise reduction

---

## t-SNE

Visualization only.

Good for:

- High-dimensional embeddings

Avoid for:

- Feature engineering

---

## UMAP

Modern alternative to t-SNE.

Usually:

- Faster
- Better global structure

---

## Autoencoders

Neural network compression:

$$
X
\rightarrow
Z
\rightarrow
\hat X
$$

Applications:

- Representation learning
- Compression
- Anomaly detection

---

# Anomaly Detection Models

## Isolation Forest

Outliers are easier to isolate.

Applications:

- Fraud detection
- Sensor monitoring
- Trading surveillance

---

## One-Class SVM

Learns normal behavior.

Detects:

$$
\text{New Data}
\notin
\text{Normal Region}
$$

---

# Ensemble Methods

Ensembles combine multiple models.

## Bagging

Parallel models.

Example:

- Random Forest

Reduces:

$$
\text{Variance}
$$

---

## Boosting

Sequential models.

Examples:

- XGBoost
- LightGBM
- CatBoost

Reduces:

$$
\text{Bias}
$$

---

## Stacking

Train:

Level 1 models

$$
M_1,M_2,M_3
$$

Then:

Meta-model

$$
M_{meta}
$$

learns from predictions.

Common in competitions.

---

# Common Production ML Pipelines

## Tabular Business Prediction

```text
Raw Data
    ↓
Cleaning
    ↓
Feature Engineering
    ↓
Train/Test Split
    ↓
XGBoost / LightGBM
    ↓
Calibration
    ↓
Deployment
```

---

## Credit Risk

```text
Raw Data
    ↓
Feature Engineering
    ↓
Logistic Regression
        +
Gradient Boosting
    ↓
Scorecard
```

---

## Recommendation Systems

```text
Collaborative Filtering
        +
Deep Learning
        +
Ranking Model
```

---

## Search Engines

```text
BM25
   +
Embeddings
   +
Transformer Re-Ranker
```

---

## Computer Vision

```text
Images
   ↓
CNN / Vision Transformer
   ↓
Object Detection
   ↓
Business Rules
```

---

# Machine Learning for Time Series

Unlike standard ML:

$$
X_t
$$

depends on

$$
X_{t-1}, X_{t-2}, \ldots
$$

Temporal order matters.

---

## Classical Time Series Models

### AR

$$
X_t
=
c
+
\sum_{i=1}^{p}
\phi_i X_{t-i}
+
\varepsilon_t
$$

---

### MA

$$
X_t
=
\varepsilon_t
+
\sum_{i=1}^{q}
\theta_i \varepsilon_{t-i}
$$

---

### ARIMA

Combines:

- AR
- Differencing
- MA

Good for:

- Univariate forecasting

---

### SARIMA

Adds seasonality.

---

### VAR

Multivariate forecasting.

$$
X_t
=
A_1X_{t-1}
+
\cdots
+
A_pX_{t-p}
+
\varepsilon_t
$$

Useful when multiple series influence each other.

---

## ML-Based Time Series

### Random Forest

Create lag features:

$$
X_{t-1}
,
X_{t-2}
,
X_{t-3}
$$

then train normally.

---

### XGBoost / LightGBM

Most common modern approach.

Features:

- Lags
- Rolling averages
- Calendar features
- Holidays
- External variables

Often beats ARIMA.

---

### LSTM / GRU

Useful when:

- Long sequences
- Complex nonlinear dynamics

---

### Transformers

Current state-of-the-art in many forecasting tasks.

Examples:

- Informer
- Autoformer
- PatchTST
- TimeGPT

---

# Common Production Time Series Pipelines

## Forecasting

```text
Raw Time Series
       ↓
Feature Engineering
       ↓
Lag Features
       ↓
Rolling Statistics
       ↓
Calendar Features
       ↓
LightGBM
       ↓
Forecast
```

---

## Quant Trading

```text
Market Data
       ↓
Feature Engineering
       ↓
Signal Models
            ↓
    XGBoost
    Random Forest
    Neural Net
            ↓
Risk Model
            ↓
Portfolio Optimizer
            ↓
Execution Engine
```

---

# Python Libraries

```python
# Classical ML
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor
from sklearn.svm import SVC

# Gradient Boosting
import xgboost as xgb
import lightgbm as lgb
from catboost import CatBoostRegressor

# Deep Learning
import torch
import tensorflow as tf

# Time Series
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.tsa.api import VAR

# Clustering
from sklearn.cluster import KMeans
from sklearn.cluster import DBSCAN

# Dimensionality Reduction
from sklearn.decomposition import PCA

# Anomaly Detection
from sklearn.ensemble import IsolationForest
```

# Typical Model Selection Heuristics

| Situation | Usually Start With |
|------------|------------|
| Tabular classification | LightGBM / XGBoost |
| Tabular regression | LightGBM / XGBoost |
| Small interpretable model | Linear / Logistic Regression |
| Many correlated predictors | Ridge |
| Feature selection needed | Lasso |
| Images | CNN / Vision Transformer |
| NLP | Transformer |
| Clustering | K-Means |
| Outlier detection | Isolation Forest |
| Univariate forecasting | ARIMA |
| Multivariate forecasting | VAR |
| Large-scale forecasting | LightGBM |
| Sequence forecasting | LSTM / Transformer |

# Key Takeaways

- Gradient boosted trees dominate structured/tabular data.
- Deep learning dominates images, language, audio, and large-scale sequence problems.
- Linear models remain valuable for interpretability and baselines.
- Ensembles usually outperform individual models.
- Time series forecasting often combines classical statistics and ML.
- In production systems, multiple models are frequently combined into a larger pipeline rather than relying on a single algorithm.
- Always start with simple baselines before moving to complex models.
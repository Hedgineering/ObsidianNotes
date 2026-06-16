# Cross Validation

## Key Takeaways

1. Cross validation is a technique for estimating how well a model will perform on unseen data.

2. The primary goal is to obtain a realistic estimate of out-of-sample performance.

3. Cross validation helps detect:
   - Overfitting
   - Underfitting
   - Model instability
   - Data leakage

4. Never evaluate a model solely on training performance.

5. Cross validation is one of the most important tools in:
   - Machine Learning
   - Statistical Modeling
   - Quantitative Research
   - Forecasting

6. In finance and time series, standard random cross validation is often invalid because it destroys temporal ordering.

7. For time series data, use:
   - Walk-forward validation
   - Rolling windows
   - Expanding windows
   - TimeSeriesSplit

8. Cross validation estimates future performance but does not guarantee future performance.

---

# Quick Reference Table

| Method | Description | Common Use Case |
|----------|----------|----------|
| Train/Test Split | Single split into train and test sets | Quick baseline |
| Holdout Validation | Separate validation and test sets | Model tuning |
| K-Fold CV | Split into K folds and rotate test fold | General ML |
| Stratified K-Fold | Preserves class proportions | Classification |
| Leave-One-Out (LOOCV) | One observation tested at a time | Small datasets |
| Repeated K-Fold | Multiple random K-fold runs | Stable estimates |
| Time Series CV | Preserve temporal order | Forecasting |
| Walk-Forward Validation | Sequential retraining/testing | Quant finance |

---

# Intuition

Suppose you train a model on historical data and obtain:

$$
R^2 = 0.95
$$

Is that good?

Maybe.

But perhaps the model memorized the training data.

Cross validation asks:

> "How well does the model perform on data it has never seen before?"

This is much closer to the real-world problem.

---

# Why Training Error is Misleading

Suppose we fit a highly flexible model.

Training error may be:

$$
0
$$

But future prediction error may be huge.

This is called:

$$
\text{Overfitting}
$$

The model learned noise instead of signal.

---

# General Idea

Split data into:

- Training data
- Validation data

Train on one portion.

Evaluate on another.

If performance remains good:

$$
\text{Model likely generalizes}
$$

---

# Train/Test Split

The simplest form of validation.

---

## Procedure

1. Split data.

Example:

$$
80\%
\text{ Train}
$$

$$
20\%
\text{ Test}
$$

2. Train model on training set.

3. Evaluate on test set.

---

## Example

Dataset:

$$
1000
$$

observations.

Training:

$$
800
$$

Testing:

$$
200
$$

Train model on 800 observations.

Evaluate RMSE on remaining 200.

---

## Limitation

Results depend heavily on the particular split.

A lucky split may produce misleading conclusions.

---

# Validation Set Approach

Often split into:

| Dataset | Purpose |
|----------|----------|
| Train | Fit model |
| Validation | Tune hyperparameters |
| Test | Final unbiased evaluation |

---

## Example

Dataset size:

$$
10,000
$$

Split:

$$
70\%
\text{ Train}
$$

$$
15\%
\text{ Validation}
$$

$$
15\%
\text{ Test}
$$

Workflow:

1. Train on Train
2. Tune on Validation
3. Evaluate once on Test

---

# K-Fold Cross Validation

## Intuition

Instead of relying on a single split, repeatedly rotate the validation set.

This reduces randomness.

---

## Procedure

Choose:

$$
K
$$

folds.

Common values:

$$
K=5
$$

or

$$
K=10
$$

---

Example:

$$
K=5
$$

Data:

| Fold | Data |
|--------|--------|
| 1 | 20% |
| 2 | 20% |
| 3 | 20% |
| 4 | 20% |
| 5 | 20% |

---

Iteration 1:

Train:

$$
2,3,4,5
$$

Test:

$$
1
$$

---

Iteration 2:

Train:

$$
1,3,4,5
$$

Test:

$$
2
$$

---

Continue until each fold has served as test data once.

---

# K-Fold Formula

Suppose fold errors are:

$$
E_1,E_2,\ldots,E_K
$$

Cross validation estimate:

$$
CV
=
\frac1K
\sum_{i=1}^{K}
E_i
$$

---

# Example

Five-fold CV yields:

$$
RMSE=
[3.1,2.8,3.3,3.0,2.9]
$$

Average:

$$
CV
=
\frac{3.1+2.8+3.3+3.0+2.9}{5}
$$

$$
=
3.02
$$

Estimated out-of-sample RMSE:

$$
3.02
$$

---

# Visualizing K-Fold

Suppose:

$$
K=5
$$

Fold assignments:

| Fold | Role |
|--------|--------|
| 1 | Test |
| 2-5 | Train |

Then:

| Fold | Role |
|--------|--------|
| 2 | Test |
| 1,3,4,5 | Train |

Then repeat until all folds are tested.

---

# Stratified K-Fold

## Problem

Classification datasets may be imbalanced.

Example:

| Class | Proportion |
|---------|---------|
| Positive | 5% |
| Negative | 95% |

Random splitting can create distorted folds.

---

## Solution

Maintain class proportions in every fold.

Each fold approximately preserves:

$$
5\%
$$

positive observations.

---

## Use Cases

- Fraud detection
- Medical diagnosis
- Credit default prediction

---

# Leave-One-Out Cross Validation (LOOCV)

## Procedure

Suppose:

$$
n
$$

observations.

Train on:

$$
n-1
$$

Test on:

$$
1
$$

Repeat for every observation.

---

## Example

Dataset:

$$
n=100
$$

Requires:

$$
100
$$

model fits.

---

## Estimate

$$
CV
=
\frac1n
\sum_{i=1}^{n}
E_i
$$

---

## Advantages

Uses nearly all data for training.

---

## Disadvantages

Very computationally expensive.

Can have high variance.

---

# Repeated K-Fold

Perform K-Fold multiple times.

Example:

$$
10
$$

different random 5-fold splits.

Compute:

$$
50
$$

validation scores.

---

Benefits:

- More stable estimates
- Less dependence on one split

---

# Bias-Variance Tradeoff

Cross validation estimates themselves have variance.

---

## Small K

Example:

$$
K=2
$$

Advantages:

- Fast

Disadvantages:

- Higher bias

---

## Large K

Example:

$$
K=10
$$

Advantages:

- Lower bias

Disadvantages:

- More computation

---

Common practice:

$$
K=5
$$

or

$$
K=10
$$

---

# Cross Validation for Model Selection

Suppose we compare:

- Linear Regression
- Random Forest
- XGBoost

Using 5-fold CV.

Results:

| Model | CV RMSE |
|---------|---------|
| Linear Regression | 12.5 |
| Random Forest | 10.4 |
| XGBoost | 9.8 |

Choose:

$$
\text{XGBoost}
$$

because it achieves lowest CV error.

---

# Hyperparameter Tuning

Cross validation is commonly used to select:

- Tree depth
- Learning rate
- Number of estimators
- Regularization strength

---

## Example

Candidate depths:

$$
[2,4,6,8]
$$

Cross-validation RMSE:

| Depth | RMSE |
|---------|---------|
| 2 | 15 |
| 4 | 11 |
| 6 | 9 |
| 8 | 11 |

Choose:

$$
6
$$

---

# Nested Cross Validation

## Problem

If we tune hyperparameters using CV and report the same CV score:

$$
\text{Optimistic Bias}
$$

may occur.

---

## Solution

Use:

- Outer CV loop
- Inner CV loop

Outer loop estimates performance.

Inner loop tunes hyperparameters.

---

# Data Leakage

## Definition

Information from the future accidentally enters training.

---

## Example

Predicting stock returns.

Features include:

$$
\text{Next Quarter Earnings}
$$

This information was unavailable at prediction time.

Results become meaningless.

---

## Cross Validation Cannot Fix Leakage

A leaked feature may produce:

$$
R^2 = 0.99
$$

even under perfect CV.

Always verify features are available at prediction time.

---

# Time Series Cross Validation

## Why Ordinary K-Fold Fails

Random shuffling destroys time ordering.

Example:

Training:

$$
2025
$$

Testing:

$$
2023
$$

This is impossible in real life.

---

# Time Series Principle

Always train on the past.

Always predict the future.

---

# Expanding Window Validation

Iteration 1:

Train:

$$
[1]
$$

Test:

$$
[2]
$$

---

Iteration 2:

Train:

$$
[1,2]
$$

Test:

$$
[3]
$$

---

Iteration 3:

Train:

$$
[1,2,3]
$$

Test:

$$
[4]
$$

Continue expanding.

---

# Rolling Window Validation

Use a fixed-size training window.

Example:

Train:

$$
2000-2005
$$

Test:

$$
2006
$$

---

Then:

Train:

$$
2001-2006
$$

Test:

$$
2007
$$

---

Then:

Train:

$$
2002-2007
$$

Test:

$$
2008
$$

---

Useful when old observations become less relevant.

---

# Walk-Forward Validation

Most common approach in quant finance.

---

## Example

Train:

$$
2000-2015
$$

Test:

$$
2016
$$

---

Retrain:

$$
2000-2016
$$

Test:

$$
2017
$$

---

Retrain:

$$
2000-2017
$$

Test:

$$
2018
$$

---

Repeat until present.

This closely mimics live trading.

---

# Quant Finance Applications

Cross validation is used for:

- Alpha signal testing
- Factor models
- Portfolio optimization
- Risk forecasting
- Volatility forecasting
- Return prediction
- Alternative data validation

---

# Example: Alpha Signal Validation

Hypothesis:

> Earnings revisions predict future returns.

Procedure:

1. Build signal.
2. Train on historical period.
3. Evaluate on future period.
4. Roll forward.
5. Aggregate results.

Metrics:

- Sharpe Ratio
- Information Ratio
- Hit Rate
- Return
- Drawdown

---

# Common Mistakes

## Mistake 1

Evaluating on training data.

---

## Mistake 2

Tuning using the test set.

The test set should only be used once.

---

## Mistake 3

Using random K-Fold on time series.

---

## Mistake 4

Ignoring data leakage.

---

## Mistake 5

Choosing the best model after testing hundreds of models.

This introduces selection bias.

---

## Mistake 6

Reporting only average CV score.

Also examine:

$$
\text{Std Dev of Fold Scores}
$$

Large variation may indicate instability.

---

# Relationship to Other Topics

Cross validation is closely related to:

- [[Experimental Design]]
- [[Hypothesis Testing]]
- [[Bootstrap Methods]]
- [[Confidence Intervals]]

---

# Interview-Level Summary

Cross validation is a resampling technique used to estimate out-of-sample model performance. The most common approach is K-Fold Cross Validation, where data is repeatedly split into training and validation folds and performance is averaged across folds. Cross validation helps detect overfitting, compare competing models, and tune hyperparameters. In quant finance and time series forecasting, ordinary random K-Fold is usually inappropriate because it violates temporal ordering. Instead, walk-forward, rolling-window, or expanding-window validation should be used to mimic real-world prediction and trading conditions.
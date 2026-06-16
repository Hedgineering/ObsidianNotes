# Model Evaluation, Debugging, and Iteration

## Key Takeaways

1. Model evaluation asks:

   "How well does this model solve the real problem on unseen data?"

2. Model debugging asks:

   "Why is the model failing, and where is the failure coming from?"

3. Model iteration asks:

   "What should we change next to improve performance safely?"

4. A good model is not just accurate. It should also be:

   - Stable
   - Robust
   - Interpretable enough for the use case
   - Well-calibrated when probabilities matter
   - Reliable out-of-sample
   - Useful after costs, constraints, and deployment issues

5. Never trust training performance alone.

6. Always compare against simple baselines.

7. Most model improvement comes from:

   - Better data
   - Better target definition
   - Better features
   - Better validation design
   - Fixing leakage
   - Better error analysis

8. In real-world data science and quant research, models are improved through a loop:

$$
\text{Evaluate}
\rightarrow
\text{Diagnose}
\rightarrow
\text{Modify}
\rightarrow
\text{Validate}
\rightarrow
\text{Monitor}
$$

---

# Quick Reference Table

| Area | Main Question | Common Tools |
|---|---|---|
| Regression | How close are predictions to actual values? | RMSE, MAE, $R^2$, residual plots |
| Classification | Are classes predicted correctly? | Accuracy, precision, recall, F1, ROC-AUC, PR-AUC |
| Probabilistic Models | Are probabilities meaningful? | Log loss, Brier score, calibration curves |
| Time Series | Does the model forecast future values well? | Walk-forward validation, forecast errors, residual autocorrelation |
| Neural Networks | Is the model learning or overfitting? | Train/val curves, loss curves, ablations |
| State-Space Models | Are latent states and observations consistent? | Innovation residuals, Kalman gain, filter stability |
| Ensemble Models | Do multiple models improve robustness? | Out-of-sample comparison, stacking validation |
| Quant Models | Does the model survive costs and regime changes? | Sharpe, drawdown, turnover, transaction costs |

---

# The Model Development Loop

A professional workflow usually looks like:

$$
\text{Define Objective}
\rightarrow
\text{Build Baseline}
\rightarrow
\text{Train Model}
\rightarrow
\text{Evaluate}
\rightarrow
\text{Debug Errors}
\rightarrow
\text{Improve}
\rightarrow
\text{Validate}
\rightarrow
\text{Deploy}
\rightarrow
\text{Monitor}
$$

The loop repeats until the model is good enough or the idea is rejected.

---

# Step 1: Define the Objective

Before modeling, define the exact goal.

Bad objective:

> Build a good model.

Better objective:

> Predict next-day stock return direction with out-of-sample AUC above $0.53$ after transaction costs.

Or:

> Forecast monthly demand with RMSE lower than the seasonal naive baseline.

---

# Step 2: Define the Target Variable

The target is what the model tries to predict.

Examples:

| Problem | Target |
|---|---|
| Regression | Continuous value $Y$ |
| Classification | Class label $Y \in \{0,1\}$ |
| Time Series | Future value $Y_{t+h}$ |
| Quant Alpha | Future return $R_{t:t+h}$ |
| Volatility Forecasting | Future realized volatility |
| Kalman Filter | Latent state $x_t$ |

---

# Target Leakage

Target leakage occurs when features contain information that would not be available at prediction time.

Example:

Predicting tomorrow's return using tomorrow's closing price.

This creates fake performance.

In time-indexed problems, every feature must satisfy:

$$
\text{Feature Timestamp}
\le
\text{Prediction Timestamp}
$$

---

# Step 3: Establish Baselines

Always compare against simple models.

## Regression Baselines

Mean prediction:

$$
\hat y_i = \bar y
$$

Median prediction:

$$
\hat y_i = \operatorname{median}(y)
$$

---

## Classification Baselines

Majority class classifier:

$$
\hat y_i
=
\text{Most Common Class}
$$

Random classifier.

Prior-probability classifier:

$$
\hat p_i
=
P(Y=1)
$$

---

## Time Series Baselines

Naive forecast:

$$
\hat y_{t+1}
=
y_t
$$

Seasonal naive forecast:

$$
\hat y_{t+s}
=
y_t
$$

Moving average:

$$
\hat y_{t+1}
=
\frac1k
\sum_{j=0}^{k-1}
y_{t-j}
$$

---

## Quant Baselines

Compare against:

- Buy and hold
- Equal-weight portfolio
- Existing production model
- Random signal
- Sector-neutral baseline
- Simple factor model

A complex model must beat simple baselines out-of-sample.

---

# Step 4: Choose the Right Evaluation Split

Evaluation must match the real-world use case.

---

## IID Data

For independent and identically distributed data, common approaches include:

- Train/test split
- K-fold [[Cross Validation]]
- Stratified K-fold for imbalanced classification

---

## Time Series Data

For time series, random splitting is usually invalid.

Use:

- Train on the past
- Validate on the future
- Test on later future data

Example:

$$
\text{Train: } 2010-2018
$$

$$
\text{Validation: } 2019-2021
$$

$$
\text{Test: } 2022-2024
$$

---

## Walk-Forward Validation

Walk-forward validation mimics live forecasting.

Iteration 1:

$$
\text{Train: } 2010-2015
$$

$$
\text{Test: } 2016
$$

Iteration 2:

$$
\text{Train: } 2010-2016
$$

$$
\text{Test: } 2017
$$

Iteration 3:

$$
\text{Train: } 2010-2017
$$

$$
\text{Test: } 2018
$$

---

# Regression Evaluation Metrics

Suppose actual values are:

$$
y_1,\ldots,y_n
$$

and predictions are:

$$
\hat y_1,\ldots,\hat y_n
$$

Define residuals:

$$
e_i
=
y_i-\hat y_i
$$

---

## Mean Squared Error

$$
MSE
=
\frac1n
\sum_{i=1}^{n}
(y_i-\hat y_i)^2
$$

MSE heavily penalizes large errors.

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

RMSE is in the same units as $Y$.

---

## Mean Absolute Error

$$
MAE
=
\frac1n
\sum_{i=1}^{n}
|y_i-\hat y_i|
$$

MAE is more robust to outliers than RMSE.

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
\text{Fraction of variance explained by the model}
$$

---

## Common Regression Debugging Questions

- Are residuals centered near zero?
- Are residuals larger for certain ranges of $X$?
- Are there outliers?
- Does the model systematically underpredict or overpredict?
- Is error worse for a specific subgroup?

---

# Classification Evaluation Metrics

## Confusion Matrix

| | Predicted Positive | Predicted Negative |
|---|---:|---:|
| Actual Positive | TP | FN |
| Actual Negative | FP | TN |

---

## Accuracy

$$
\text{Accuracy}
=
\frac{TP+TN}{TP+TN+FP+FN}
$$

Accuracy can be misleading when classes are imbalanced.

---

## Precision

$$
\text{Precision}
=
\frac{TP}{TP+FP}
$$

Precision answers:

> Of predicted positives, how many were actually positive?

---

## Recall

$$
\text{Recall}
=
\frac{TP}{TP+FN}
$$

Recall answers:

> Of actual positives, how many did we find?

---

## F1 Score

$$
F_1
=
2
\cdot
\frac{
\text{Precision}
\cdot
\text{Recall}
}
{
\text{Precision}
+
\text{Recall}
}
$$

Useful when both false positives and false negatives matter.

---

## ROC-AUC

ROC-AUC measures ranking ability across thresholds.

AUC interpretation:

$$
AUC
=
P(\text{Random Positive Ranked Above Random Negative})
$$

---

## PR-AUC

Precision-recall AUC is often better for rare-event classification.

Examples:

- Fraud detection
- Disease detection
- Default prediction
- Rare trading opportunities

---

# Probability Model Evaluation

Sometimes the goal is not only correct labels, but good probabilities.

Example:

A model predicts:

$$
P(Y=1 \mid X)=0.80
$$

Among many such cases, roughly $80\%$ should actually be positive.

---

## Log Loss

For binary classification:

$$
\text{Log Loss}
=
-\frac1n
\sum_{i=1}^{n}
\left[
y_i\log(\hat p_i)
+
(1-y_i)\log(1-\hat p_i)
\right]
$$

Log loss strongly penalizes confident wrong predictions.

---

## Brier Score

$$
\text{Brier Score}
=
\frac1n
\sum_{i=1}^{n}
(\hat p_i-y_i)^2
$$

Lower is better.

---

## Calibration

A model is calibrated if predicted probabilities match observed frequencies.

Example:

Among predictions near:

$$
0.70
$$

the event should happen about:

$$
70\%
$$

of the time.

---

# Time Series Model Evaluation

Time series models must be evaluated differently because observations are ordered and dependent.

---

## Forecast Error

For forecast horizon $h$:

$$
e_{t+h}
=
y_{t+h}
-
\hat y_{t+h \mid t}
$$

---

## One-Step-Ahead Forecast

$$
\hat y_{t+1 \mid t}
$$

uses information up to time $t$.

---

## Multi-Step-Ahead Forecast

$$
\hat y_{t+h \mid t}
$$

predicts $h$ steps into the future.

Multi-step forecasts are usually harder.

---

## Residual Autocorrelation

Good time series residuals should not contain obvious remaining structure.

Check whether:

$$
\operatorname{Corr}(e_t,e_{t-k})
\approx
0
$$

for meaningful lags $k$.

If residuals are autocorrelated, the model missed time structure.

---

## Ljung-Box Test

Used to test whether residual autocorrelations are jointly zero.

Null hypothesis:

$$
H_0:
\rho_1=\rho_2=\cdots=\rho_k=0
$$

If rejected, residuals likely contain remaining serial dependence.

---

## Time Series Debugging Questions

- Are residuals autocorrelated?
- Are residuals heteroskedastic?
- Does performance vary by regime?
- Does the model fail during volatility spikes?
- Does performance decay over forecast horizon?
- Are features available at forecast time?
- Is the target stationary?

---

# Neural Network Evaluation and Debugging

Neural networks are flexible, so they can easily overfit.

---

## Training and Validation Loss

Healthy learning:

$$
\text{Training Loss}
\downarrow
$$

$$
\text{Validation Loss}
\downarrow
$$

---

Overfitting:

$$
\text{Training Loss}
\downarrow
$$

$$
\text{Validation Loss}
\uparrow
$$

---

Underfitting:

$$
\text{Training Loss High}
$$

$$
\text{Validation Loss High}
$$

---

## Common Neural Network Debugging Checks

- Can the model overfit a tiny batch?
- Are gradients exploding?
- Are gradients vanishing?
- Is the learning rate too high?
- Is the learning rate too low?
- Is data normalized?
- Is the loss function appropriate?
- Are labels correctly encoded?
- Is regularization too strong?
- Is the architecture unnecessarily complex?

---

## Tiny Batch Overfit Test

A neural network should be able to memorize a tiny dataset.

If it cannot overfit:

$$
10
$$

or

$$
100
$$

examples, there may be a bug.

Possible causes:

- Wrong labels
- Broken loss function
- Incorrect output activation
- Learning rate issue
- Data preprocessing bug

---

# Classical Statistical Model Evaluation

Classical models emphasize assumptions, interpretability, and uncertainty.

Examples:

- Linear regression
- Logistic regression
- GLMs
- ARIMA
- VAR
- State-space models

---

## Linear Regression Diagnostics

Model:

$$
Y
=
X\beta+\varepsilon
$$

Assumptions often include:

$$
E[\varepsilon \mid X]=0
$$

$$
\operatorname{Var}(\varepsilon \mid X)=\sigma^2
$$

$$
\operatorname{Corr}(\varepsilon_i,\varepsilon_j)=0
$$

---

## Important Checks

- Residuals centered around zero
- No strong residual pattern
- No severe heteroskedasticity
- No influential outliers
- Coefficients have plausible signs
- Confidence intervals are reasonable

---

## Coefficient Significance

For coefficient $\beta_j$:

$$
H_0:
\beta_j=0
$$

Test statistic:

$$
t
=
\frac{\hat\beta_j}{SE(\hat\beta_j)}
$$

Large absolute values suggest stronger evidence.

---

# State-Space Models and Kalman Filters

A common setup:

$$
x_t
=
A x_{t-1}
+
w_t
$$

$$
y_t
=
H x_t
+
v_t
$$

where:

- $x_t$ is the hidden state
- $y_t$ is the observation
- $w_t$ is process noise
- $v_t$ is observation noise

---

## Prediction Step

$$
\hat x_{t \mid t-1}
=
A\hat x_{t-1 \mid t-1}
$$

---

## Innovation

$$
\nu_t
=
y_t
-
H\hat x_{t \mid t-1}
$$

The innovation is the surprise in the new observation.

---

## Innovation Covariance

$$
S_t
=
H P_{t \mid t-1}H^\top
+
R
$$

---

## Kalman Gain

$$
K_t
=
P_{t \mid t-1}H^\top S_t^{-1}
$$

---

## Update Step

$$
\hat x_{t \mid t}
=
\hat x_{t \mid t-1}
+
K_t\nu_t
$$

---

## Kalman Filter Debugging

Check:

- Are innovations centered near zero?
- Are innovations autocorrelated?
- Is estimated uncertainty too small?
- Is the filter too slow to adapt?
- Is the filter too noisy?
- Are $Q$ and $R$ reasonable?
- Does the state estimate explode?
- Does the covariance matrix remain positive semidefinite?

---

# Combining Models

Many real systems combine multiple models.

Examples:

- Neural network + Kalman filter
- Regression + rules-based filter
- Forecast model + risk model
- Alpha model + portfolio optimizer
- Classifier + anomaly detector

---

# Example: Neural Network + Kalman Filter

Possible design:

1. Neural network estimates nonlinear observation model.
2. Kalman filter smooths noisy state estimates.

Example:

$$
z_t
=
f_\theta(X_t)
$$

where $f_\theta$ is a neural network output.

Then:

$$
x_t
=
A x_{t-1}+w_t
$$

$$
z_t
=
H x_t+v_t
$$

The neural network extracts a signal, and the Kalman filter smooths it over time.

---

## Debugging Hybrid Models

When a combined model fails, isolate components.

Ask:

1. Does the neural network work alone?
2. Does the Kalman filter work with clean simulated data?
3. Does the combined system improve over either component?
4. Is error introduced by bad features, bad state assumptions, or bad integration?
5. Are time indices aligned correctly?

---

# Ensemble Model Evaluation

Ensembles combine multiple predictions.

Simple average:

$$
\hat y
=
\frac1M
\sum_{m=1}^{M}
\hat y^{(m)}
$$

Weighted average:

$$
\hat y
=
\sum_{m=1}^{M}
w_m\hat y^{(m)}
$$

where:

$$
\sum_{m=1}^{M}
w_m
=
1
$$

---

## When Ensembles Help

Ensembles help when models make different errors.

If errors are weakly correlated, averaging can reduce variance.

---

## Ensemble Debugging

Check:

- Does each model beat baseline?
- Are model errors correlated?
- Does the ensemble improve out-of-sample?
- Are weights learned only on validation data?
- Is there leakage in stacking?

---

# Quant Finance Model Evaluation

In quant finance, predictive accuracy is not enough.

A model must survive:

- Transaction costs
- Slippage
- Turnover
- Capacity constraints
- Shorting constraints
- Risk limits
- Regime changes

---

## Common Quant Metrics

| Metric | Meaning |
|---|---|
| Mean Return | Average return |
| Volatility | Risk or variability |
| Sharpe Ratio | Return per unit risk |
| Sortino Ratio | Downside-risk-adjusted return |
| Max Drawdown | Worst peak-to-trough loss |
| Hit Rate | Fraction of profitable trades |
| Turnover | How often positions change |
| Information Ratio | Active return per active risk |
| Capacity | How much capital strategy can absorb |

---

## Sharpe Ratio

$$
\text{Sharpe}
=
\frac{
E[R-R_f]
}
{
\sigma(R-R_f)
}
$$

Annualized daily Sharpe:

$$
\text{Sharpe}_{ann}
=
\frac{\bar r}{s_r}
\sqrt{252}
$$

---

## Maximum Drawdown

Let cumulative wealth be:

$$
W_t
$$

Running peak:

$$
M_t
=
\max_{s \le t} W_s
$$

Drawdown:

$$
D_t
=
\frac{W_t-M_t}{M_t}
$$

Maximum drawdown:

$$
\max_t |D_t|
$$

---

## Transaction Cost Adjustment

Gross return:

$$
R_t^{gross}
$$

Turnover:

$$
TO_t
$$

Cost per unit turnover:

$$
c
$$

Net return:

$$
R_t^{net}
=
R_t^{gross}
-
c \cdot TO_t
$$

A model that works before costs may fail after costs.

---

# Error Analysis

Error analysis is one of the most important real-world debugging tools.

---

## Procedure

1. Collect cases where the model performs badly.
2. Group errors by type.
3. Identify recurring patterns.
4. Prioritize fixes based on impact.

---

## Example Error Buckets

| Error Type | Possible Fix |
|---|---|
| Outliers | Robust loss, winsorization |
| Missing values | Better imputation |
| Regime shift | Regime-aware model |
| Bad labels | Label cleaning |
| Rare classes | Rebalancing or better metric |
| Miscalibration | Probability calibration |
| Feature leakage | Feature audit |

---

# Bias-Variance Diagnosis

Training error:

$$
E_{train}
$$

Validation error:

$$
E_{val}
$$

---

## High Bias

$$
E_{train}
\text{ high}
$$

$$
E_{val}
\text{ high}
$$

Model is underfitting.

Possible fixes:

- Add features
- Use more flexible model
- Reduce regularization
- Train longer
- Improve target definition

---

## High Variance

$$
E_{train}
\text{ low}
$$

$$
E_{val}
\text{ high}
$$

Model is overfitting.

Possible fixes:

- Add data
- Increase regularization
- Simplify model
- Use dropout
- Use early stopping
- Reduce feature count

---

# Ablation Studies

Ablation means removing one component at a time.

Example:

Full model uses:

$$
A+B+C
$$

Test:

$$
A+B
$$

$$
A+C
$$

$$
B+C
$$

$$
A
$$

$$
B
$$

$$
C
$$

This identifies which components actually help.

---

# Sensitivity Analysis

Change assumptions slightly and observe performance.

Examples:

- Change lookback windows
- Change thresholds
- Change random seeds
- Change training period
- Remove outliers
- Use different validation folds

Robust models should not collapse under small changes.

---

# Residual Analysis

Residual:

$$
e_i
=
y_i-\hat y_i
$$

Useful checks:

- Mean close to zero
- No obvious pattern
- Constant variance
- No autocorrelation
- No major subgroup bias

---

## Residual Mean

$$
\bar e
=
\frac1n
\sum_{i=1}^{n}
e_i
$$

Ideally:

$$
\bar e
\approx
0
$$

---

## Residual Variance

$$
s_e^2
=
\frac1{n-1}
\sum_{i=1}^{n}
(e_i-\bar e)^2
$$

---

# Model Calibration

Calibration matters when probabilities drive decisions.

Examples:

- Risk models
- Credit default probabilities
- Medical diagnosis
- Trade probability models

---

## Calibration Rule

If a model predicts:

$$
\hat p=0.80
$$

then the event should occur about:

$$
80\%
$$

of the time among similar predictions.

---

# Monitoring After Deployment

A model can be good historically and fail later.

Monitor:

- Input drift
- Target drift
- Prediction drift
- Error drift
- Calibration drift
- Latency
- Missing data
- Feature distribution changes

---

## Data Drift

Feature distribution changes:

$$
P_{train}(X)
\neq
P_{live}(X)
$$

---

## Concept Drift

Relationship changes:

$$
P_{train}(Y \mid X)
\neq
P_{live}(Y \mid X)
$$

Concept drift is harder because the underlying mapping changes.

---

# Real-World Iteration Strategy

A practical improvement order:

1. Verify data quality.
2. Verify target definition.
3. Check leakage.
4. Compare against baselines.
5. Perform error analysis.
6. Improve features.
7. Tune model complexity.
8. Tune hyperparameters.
9. Try alternative model families.
10. Ensemble if individual models are strong.
11. Validate on fresh data.
12. Monitor after deployment.

---

# Common Model Improvement Levers

## Data Improvements

- More observations
- Cleaner labels
- Better feature coverage
- Better timestamp alignment
- Better missing value handling
- Removing duplicates
- Removing leakage

---

## Feature Improvements

- Interactions
- Lagged variables
- Rolling statistics
- Domain-specific transformations
- Normalization
- Dimensionality reduction
- Better categorical encoding

---

## Model Improvements

- Simpler model
- More flexible model
- Regularization
- Hyperparameter tuning
- Better loss function
- Better architecture
- Ensemble methods

---

## Evaluation Improvements

- Better validation split
- Better metrics
- Subgroup analysis
- Backtesting
- Walk-forward testing
- Stress testing
- Confidence intervals

---

# Debugging Checklist

## Data

- Are rows duplicated?
- Are labels correct?
- Are missing values handled properly?
- Are timestamps aligned?
- Are units consistent?
- Are train and test distributions similar?
- Is there leakage?

---

## Features

- Are features available at prediction time?
- Are categorical encodings consistent?
- Are numerical scales reasonable?
- Are outliers handled?
- Are transformations applied consistently?
- Are features too sparse?

---

## Model

- Is the loss decreasing?
- Is the model too simple?
- Is the model too complex?
- Are hyperparameters reasonable?
- Are coefficients or feature importances plausible?
- Does the model beat baseline?

---

## Evaluation

- Is the metric aligned with the business goal?
- Is the validation split realistic?
- Is the test set untouched?
- Are results statistically significant?
- Are confidence intervals wide?
- Does performance survive robustness checks?

---

## Deployment

- Are live features identical to training features?
- Are there missing live inputs?
- Is latency acceptable?
- Is monitoring in place?
- Does performance decay over time?
- Is retraining needed?

---

# Example 1: Regression Debugging

Problem:

Predict house prices.

Model:

Linear regression.

Training RMSE:

$$
20,000
$$

Validation RMSE:

$$
80,000
$$

Diagnosis:

$$
E_{train} \ll E_{val}
$$

This suggests overfitting or leakage issues.

Possible fixes:

- Check feature leakage.
- Reduce feature count.
- Add regularization.
- Use cross validation.
- Check outliers.
- Compare to simple baseline.

---

# Example 2: Classification Debugging

Problem:

Fraud detection.

Model accuracy:

$$
99\%
$$

But fraud rate:

$$
1\%
$$

A model predicting "not fraud" every time also gets:

$$
99\%
$$

accuracy.

Accuracy is misleading.

Better metrics:

- Recall
- Precision
- F1
- PR-AUC

---

# Example 3: Time Series Debugging

Problem:

Forecast demand.

Model beats baseline on random train/test split.

But fails in production.

Likely issue:

Random split leaked future seasonal information.

Better design:

$$
\text{Train on past}
\rightarrow
\text{Test on future}
$$

Use walk-forward validation.

---

# Example 4: Quant Model Debugging

Problem:

Signal has high backtest Sharpe.

After transaction costs:

$$
\text{Sharpe}
\approx
0
$$

Diagnosis:

Signal may rely on excessive turnover.

Fixes:

- Penalize turnover.
- Smooth positions.
- Trade less frequently.
- Add transaction cost model.
- Check capacity.

---

# Example 5: Neural Network Debugging

Problem:

Training loss does not decrease.

Possible causes:

- Learning rate too high
- Learning rate too low
- Broken labels
- Incorrect activation function
- Loss function mismatch
- Bad normalization
- Gradient problem

First test:

Can the network overfit a tiny batch?

If no:

The training pipeline likely has a bug.

---

# Common Mistakes

1. Evaluating only on training data.
2. Using a random split for time series.
3. Ignoring baseline models.
4. Optimizing the wrong metric.
5. Tuning repeatedly on the test set.
6. Ignoring data leakage.
7. Ignoring calibration.
8. Ignoring subgroup performance.
9. Assuming complex models are automatically better.
10. Deploying without monitoring drift.
11. Ignoring transaction costs in quant models.
12. Treating backtest performance as guaranteed future performance.

---

# Relationship to Other Topics

Model evaluation and debugging are closely related to:

- [[Experimental Design]]
- [[Cross Validation]]
- [[Confidence Intervals]]
- [[Bootstrap Methods]]
- [[Hypothesis Testing]]
- [[Bias-Variance Tradeoff]]
- [[Overfitting]]
- [[Regression Analysis]]
- [[Time Series Analysis]]
- [[Machine Learning]]
- [[Kalman Filter]]
- [[Model Selection]]
- [[Feature Engineering]]

---

# Interview-Level Summary

Model evaluation measures how well a model solves the real problem, model debugging identifies why it fails, and model iteration improves it through controlled changes. A strong workflow starts with simple baselines, realistic validation splits, appropriate metrics, error analysis, leakage checks, robustness testing, and monitoring after deployment. Classical models require residual and assumption checks, machine learning models require out-of-sample validation and bias-variance diagnosis, neural networks require training dynamics and ablation analysis, time series models require walk-forward validation and residual autocorrelation checks, and hybrid systems such as neural networks with Kalman filters require each component to be tested independently before evaluating the combined system.
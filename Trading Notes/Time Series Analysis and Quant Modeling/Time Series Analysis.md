# Time Series Analysis: Quant Finance-Oriented Overview

## Quick Reference Table

| Topic | Purpose | Quant Finance Importance |
|---------|----------|-------------------------|
| Stationarity | Stable statistical properties over time | Extremely Important |
| Trend | Long-term movement | Important |
| Seasonality | Repeating periodic patterns | Moderate |
| Autocorrelation (ACF) | Relationship with lagged values | Extremely Important |
| Partial Autocorrelation (PACF) | Direct lag relationships | Extremely Important |
| White Noise | Pure randomness | Foundation |
| AR Models | Predict using past values | Important |
| MA Models | Predict using past shocks | Important |
| ARMA | AR + MA | Important |
| ARIMA | Differenced ARMA | Important |
| SARIMA | Seasonal ARIMA | Moderate |
| Exponential Smoothing | Forecasting trends/seasonality | Moderate |
| VAR | Multivariate time series | Extremely Important |
| VARX | VAR with exogenous variables | Extremely Important |
| Cointegration | Long-run equilibrium relationships | Extremely Important |
| VECM | Cointegrated system modeling | Important |
| Kalman Filter | Dynamic state estimation | Extremely Important |
| State Space Models | Hidden state modeling | Extremely Important |
| GARCH | Volatility forecasting | Extremely Important |
| EGARCH/GJR-GARCH | Asymmetric volatility | Important |
| HMM | Regime detection | Important |
| PCA | Factor extraction | Extremely Important |
| Fourier Methods | Frequency decomposition | Useful |
| Wavelets | Multi-scale decomposition | Niche |
| Change Point Detection | Regime shifts | Important |
| Bayesian Time Series | Probabilistic forecasting | Growing Importance |
| Deep Learning TS Models | Complex nonlinear forecasting | Increasing Importance |

---

# Big Picture

Time series analysis studies data observed sequentially through time.

Examples:

| Domain | Time Series |
|----------|------------|
| Equities | Daily returns |
| FX | Exchange rates |
| Rates | Treasury yields |
| Commodities | Futures prices |
| Macro | Inflation |
| Operations | Website traffic |
| Sensors | Temperature readings |

Unlike ordinary machine learning:

Observations are not independent.

Time matters.

Ordering matters.

History matters.

---

# Core Mental Model

Most time series analysis revolves around:

$$
X_t
=
\text{Signal}
+
\text{Noise}
$$

The challenge is separating:

- Predictable structure
- Random variation

---

# Typical Quant Workflow

$$
\text{Raw Series}
\rightarrow
\text{Stationarity}
\rightarrow
\text{Feature Extraction}
\rightarrow
\text{Model}
\rightarrow
\text{Forecast}
\rightarrow
\text{Risk Analysis}
$$

---

# The Most Important Concept: Stationarity

A stationary process has stable properties over time.

Examples:

Mean:

$$
E[X_t]
=
\mu
$$

Variance:

$$
Var(X_t)
=
\sigma^2
$$

remain constant.

---

# Why Stationarity Matters

Many classical models assume:

$$
\text{Stationarity}
$$

Without it:

- Forecasts become unreliable
- Parameters drift
- Statistical tests fail

---

# Types of Stationarity

## Strict Stationarity

Entire distribution unchanged.

Rarely required.

---

## Weak Stationarity

Only requires:

- Constant mean
- Constant variance
- Constant autocovariance

Most common assumption.

---

# Random Walk

A central finance concept.

$$
X_t
=
X_{t-1}
+
\epsilon_t
$$

where:

$$
\epsilon_t
\sim
WN(0,\sigma^2)
$$

Random walks are:

- Nonstationary
- Common for asset prices

---

# Differencing

Convert nonstationary series into stationary series.

First difference:

$$
\Delta X_t
=
X_t-X_{t-1}
$$

Example:

Prices:

$$
\rightarrow
$$

Returns.

---

# Returns vs Prices

Quant finance typically models:

$$
r_t
=
\log(P_t)-\log(P_{t-1})
$$

rather than prices.

Returns are usually closer to stationary.

---

# White Noise

Foundation of all forecasting.

White noise:

$$
\epsilon_t
\sim
WN(0,\sigma^2)
$$

Properties:

- Mean zero
- Constant variance
- No autocorrelation

Pure randomness.

---

# Autocorrelation (ACF)

Measures relationship between:

$$
X_t
$$

and

$$
X_{t-k}
$$

Formula:

$$
\rho_k
=
Corr(X_t,X_{t-k})
$$

---

# Why ACF Matters

Many forecasting models rely on:

Predictability from past values.

---

# Partial Autocorrelation (PACF)

Measures direct effect of lag:

$$
k
$$

after removing intermediate lags.

Critical for identifying AR models.

---

# AR Models (Autoregressive)

Predict future values using past values.

AR(1):

$$
X_t
=
c
+
\phi X_{t-1}
+
\epsilon_t
$$

---

# Interpretation

If:

$$
\phi = 0.8
$$

then today's value strongly depends on yesterday's value.

---

# AR(p)

General form:

$$
X_t
=
c
+
\sum_{i=1}^{p}
\phi_i X_{t-i}
+
\epsilon_t
$$

---

# MA Models (Moving Average)

Depend on past shocks.

MA(1):

$$
X_t
=
\mu
+
\epsilon_t
+
\theta \epsilon_{t-1}
$$

---

# ARMA Models

Combine:

- AR
- MA

ARMA(p,q):

$$
AR(p)
+
MA(q)
$$

---

# ARIMA Models

Most famous classical forecasting model.

ARIMA:

$$
(p,d,q)
$$

where:

- $p$ = AR order
- $d$ = differencing order
- $q$ = MA order

---

# Example

ARIMA:

$$
(1,1,1)
$$

means:

- First difference once
- AR(1)
- MA(1)

---

# SARIMA

Adds seasonality.

Useful for:

- Retail demand
- Energy consumption
- Economic data

Less common in finance.

---

# Exponential Smoothing

Forecasts using weighted averages.

Recent observations receive higher weight.

---

# EWMA

Very important in finance.

Recursive form:

$$
\sigma_t^2
=
\lambda\sigma_{t-1}^2
+
(1-\lambda)r_t^2
$$

Used by:

- RiskMetrics
- Volatility forecasting

---

# Volatility Clustering

One of the most important financial stylized facts.

Large moves tend to follow:

Large moves.

Small moves tend to follow:

Small moves.

---

# GARCH

The most important volatility model.

GARCH(1,1):

$$
\sigma_t^2
=
\omega
+
\alpha r_{t-1}^2
+
\beta \sigma_{t-1}^2
$$

---

# Why GARCH Matters

Models:

- Volatility persistence
- Volatility forecasting
- Risk estimation

Widely used in:

- Risk management
- Options
- Portfolio construction

---

# EGARCH

Captures asymmetry.

Bad news often increases volatility more than good news.

Very important in equities.

---

# Multivariate Time Series

Most quant finance problems involve multiple series.

Examples:

- Stocks
- Rates
- FX
- Commodities

interacting simultaneously.

---

# VAR (Vector Autoregression)

One of the most important quant models.

General form:

$$
X_t
=
A_1X_{t-1}
+
\cdots
+
A_pX_{t-p}
+
\epsilon_t
$$

where:

$$
X_t
$$

is a vector.

---

# Applications of VAR

- Macro forecasting
- Yield curve modeling
- Multi-asset prediction
- Economic analysis

---

# VARX

VAR with exogenous variables.

Example:

Predict:

- Auto sales

using:

- Past sales
- Gas prices
- Transit ridership

Very common in industry.

---

# Cointegration

Extremely important in quantitative finance.

Two nonstationary series may move together long term.

Example:

- ETF vs NAV
- Stock pairs
- Yield curve instruments

---

# Cointegration Definition

If:

$$
X_t
$$

and

$$
Y_t
$$

are nonstationary

but:

$$
X_t-\beta Y_t
$$

is stationary,

then they are cointegrated.

---

# Why Cointegration Matters

Foundation of:

- Statistical arbitrage
- Pairs trading
- Relative value trading

---

# Engle-Granger Test

Most common cointegration test.

Used heavily in pairs trading research.

---

# VECM

Vector Error Correction Model.

Combines:

- Short-term dynamics
- Long-term equilibrium

for cointegrated series.

---

# State Space Models

Very important modern framework.

General form:

State equation:

$$
x_t
=
Ax_{t-1}
+w_t
$$

Observation equation:

$$
y_t
=
Hx_t
+v_t
$$

---

# Kalman Filter

Perhaps the most important modern time-series tool in quantitative finance.

Used for:

- Trend estimation
- Spread estimation
- Dynamic hedge ratios
- Signal extraction

---

# Common Quant Uses of Kalman Filters

## Dynamic Beta

Estimate:

$$
\beta_t
$$

through time.

---

## Pairs Trading

Estimate changing spread relationship.

---

## Trend Following

Extract hidden trend from noisy prices.

---

# Hidden Markov Models (HMM)

Assume market switches between hidden regimes.

Example:

States:

- Bull
- Bear
- Sideways

---

# Uses

- Regime detection
- Volatility states
- Strategy switching

---

# PCA

Extremely important in finance.

Used for:

- Factor extraction
- Yield curves
- Risk models

---

# Yield Curve Example

Most curve movement explained by:

1. Level
2. Slope
3. Curvature

obtained via PCA.

---

# Spectral Analysis

Studies frequencies in time series.

Fourier decomposition:

$$
X_t
=
\sum
A_i
\sin(\omega_i t)
+
B_i
\cos(\omega_i t)
$$

Useful for:

- Cycles
- Seasonal behavior

---

# Wavelets

Multi-scale decomposition.

Useful when frequency structure changes through time.

More niche in finance.

---

# Change Point Detection

Identify structural breaks.

Examples:

- COVID
- 2008 Crisis
- Regime shifts

Important for:

- Risk management
- Alpha monitoring

---

# Bayesian Time Series

Instead of:

Point forecasts.

Produces:

Probability distributions.

Useful for:

- Uncertainty estimation
- Dynamic updating

---

# Deep Learning Time Series

Modern approaches:

- LSTM
- GRU
- Temporal CNN
- Transformer
- TFT
- N-BEATS

---

# Important Reality Check

In finance:

Deep learning often produces modest gains over simpler methods.

Many successful firms still heavily use:

- Linear models
- Factor models
- Kalman filters
- PCA
- GARCH
- VAR

---

# Stylized Facts of Financial Time Series

These are extremely important.

---

## Prices Approximate Random Walks

Returns more predictable than prices.

---

## Returns Have Near-Zero Autocorrelation

Especially daily returns.

---

## Volatility Clusters

Large moves follow large moves.

---

## Heavy Tails

Extreme events occur more frequently than Gaussian theory predicts.

---

## Asymmetric Volatility

Bad news often increases volatility more.

---

## Regime Dependence

Market behavior changes through time.

---

# Quant Finance Time Series Toolbox

## Return Forecasting

- Linear regression
- AR
- VAR
- State-space models
- ML models

---

## Volatility Forecasting

- EWMA
- GARCH
- EGARCH
- Realized volatility models

---

## Risk Modeling

- PCA
- Factor models
- Covariance estimation
- Dynamic correlation models

---

## Statistical Arbitrage

- Cointegration
- VECM
- Kalman filters
- Mean reversion models

---

## Trend Following

- Moving averages
- State-space models
- Kalman filters
- Regime models

---

## Macro Forecasting

- VAR
- VARX
- Bayesian VAR
- State-space models

---

# Common Python Libraries

## Core

```python
pandas
numpy
scipy
```

---

## Classical Time Series

```python
statsmodels
```

Provides:

- ARIMA
- SARIMA
- VAR
- VECM
- State-space models

---

## Volatility Models

```python
arch
```

Provides:

- GARCH
- EGARCH
- GJR-GARCH


---

## State Space / Kalman

```python
pykalman
filterpy
statsmodels
```

---

## Machine Learning

```python
scikit-learn
xgboost
lightgbm
```

---

## Deep Learning

```python
pytorch
tensorflow
```

---

# Recommended Learning Order for Quant Finance

## Phase 1 (Absolute Essentials)

1. Stationarity
2. ACF/PACF
3. AR
4. MA
5. ARIMA
6. Forecast evaluation

---

## Phase 2 (Quant Core)

7. GARCH
8. VAR
9. Cointegration
10. VECM
11. PCA
12. Kalman Filters

---

## Phase 3 (Professional Quant)

13. State-space models
14. HMMs
15. Dynamic factor models
16. Bayesian time series
17. Regime-switching models

---

## Phase 4 (Advanced)

18. Deep learning time series
19. Transformers
20. Probabilistic forecasting
21. Reinforcement-learning-based forecasting

---

# Common Interview Questions

## Why model returns instead of prices?

Prices are usually nonstationary.

Returns are often closer to stationary.

---

## Why is stationarity important?

Most statistical inference assumes stable distributions.

---

## What is the difference between AR and MA?

AR uses past values.

MA uses past shocks.

---

## Why does GARCH work?

Volatility clusters.

Past volatility predicts future volatility.

---

## Why is cointegration important?

It enables mean-reversion strategies despite nonstationary prices.

---

## Why use Kalman Filters?

Markets evolve dynamically.

Kalman filters update estimates continuously as new information arrives.

---

# Key Takeaways

- Time series analysis studies data observed through time where observations are dependent.
- Stationarity is the foundational concept behind most classical methods.
- ARIMA remains one of the most important forecasting frameworks.
- GARCH and its variants dominate volatility forecasting.
- VAR is one of the most important multivariate models in economics and finance.
- Cointegration and VECM form the foundation of many statistical arbitrage strategies.
- Kalman filters and state-space models are among the most useful professional quant tools.
- PCA is heavily used in factor models, yield curves, and risk systems.
- Modern quant finance combines classical methods with machine learning and deep learning, but classical methods remain surprisingly dominant in production.
- If your goal is quantitative research, the highest-priority topics are: Stationarity, ARIMA, GARCH, VAR, Cointegration, PCA, Kalman Filters, and State-Space Models.
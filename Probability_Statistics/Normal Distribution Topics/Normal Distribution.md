# Normal Distribution (Gaussian Distribution)

## Quick Reference

| Property | Formula |
|----------|---------|
| Notation | $X \sim \mathcal{N}(\mu,\sigma^2)$ |
| Parameters | $\mu$ = mean, $\sigma^2$ = variance, $\sigma$ = standard deviation |
| Support | $-\infty < x < \infty$ |
| PDF | $f_X(x)=\dfrac{1}{\sigma\sqrt{2\pi}}\exp\left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)$ |
| CDF | $F_X(x)=P(X\le x)=\displaystyle\int_{-\infty}^{x}f_X(t)\,dt$ |
| Mean | $E[X]=\mu$ |
| Variance | $\operatorname{Var}(X)=\sigma^2$ |
| Standard Deviation | $\sigma$ |
| Mode | $\mu$ |
| Median | $\mu$ |
| Skewness | $0$ |
| Kurtosis | $3$ (Excess Kurtosis = $0$) |
| Symmetric? | Yes |
| Bell-shaped? | Yes |

---

# What Does

$$
X\sim\mathcal N(\mu,\sigma^2)
$$

Mean?

This notation means

> The random variable $X$ follows a **Normal (Gaussian) distribution** with mean $\mu$ and variance $\sigma^2$.

Knowing these two parameters completely determines the distribution.

For example,

$$
X\sim\mathcal N(10,4)
$$

means

- Mean:

  $$
  \mu=10
  $$

- Variance:

  $$
  \sigma^2=4
  $$

- Standard deviation:

  $$
  \sigma=2.
  $$

---

# Intuition

The Normal distribution models quantities that arise from the **combined effect of many small, independent sources of randomness.**

Examples include:

- Human height
- IQ scores
- Measurement error
- Manufacturing tolerances
- Sensor noise
- Test scores
- Asset returns (approximately, over short periods)
- Estimation errors
- Sample means (via the [[Central Limit Theorem]])

Rather than one large random event determining the outcome,

many tiny random influences add together.

The resulting distribution is often approximately Normal.

---

## Why Does It Appear So Often?

Suppose your height depends on

- genetics,
- nutrition,
- sleep,
- hormones,
- environment,
- random developmental variation,

and dozens of other factors.

Each contributes only a small amount.

Adding many small independent effects naturally produces a distribution that is approximately Normal.

This is one of the main consequences of the [[Central Limit Theorem]].

---

# Shape of the Normal Distribution

The Normal distribution is

- continuous,
- symmetric,
- bell-shaped,
- unimodal (one peak).

The highest point occurs at the mean.

As you move away from the mean,

the density decreases smoothly.

```text
                 ^
                 |
             .-''''-.
           .'        '.
         .'            '.
-------.'------μ--------'.-------->
```

---

# Parameters

Every Normal distribution is determined by two numbers.

## Mean ($\mu$)

The mean controls the **location** of the distribution.

Increasing $\mu$

moves the entire bell curve to the right.

Decreasing $\mu$

moves it to the left.

Changing $\mu$

**does not change the shape**.

---

## Standard Deviation ($\sigma$)

The standard deviation controls the **spread**.

Large $\sigma$

↓

Wider distribution

↓

More variability

---

Small $\sigma$

↓

Narrower distribution

↓

Less variability

---

Example

```text
Small σ

        /\
       /  \
------/----\------

Large σ

     /--------\
----/----------\----
```

---

# Probability Density Function (PDF)

The PDF is

$$
f_X(x)
=
\frac{1}{\sigma\sqrt{2\pi}}
\exp\left(
-\frac{(x-\mu)^2}{2\sigma^2}
\right).
$$

The PDF tells us the **probability density**, not probability.

Remember:

$$
P(X=x)=0
$$

for every continuous random variable.

Probabilities are computed by integrating the PDF.

Example:

$$
P(a\le X\le b)
=
\int_a^bf_X(x)\,dx.
$$

---

## Understanding the PDF

The PDF has three important parts.

### 1. The Mean

The term

$$
(x-\mu)^2
$$

measures how far $x$ is from the center.

Closer to the mean

↓

Larger density.

Farther away

↓

Smaller density.

---

### 2. The Variance

The denominator

$$
2\sigma^2
$$

controls how quickly the curve decreases.

Large variance

↓

Slow decrease

↓

Wide bell curve.

Small variance

↓

Rapid decrease

↓

Narrow bell curve.

---

### 3. The Normalization Constant

The factor

$$
\frac1{\sigma\sqrt{2\pi}}
$$

ensures that

$$
\int_{-\infty}^{\infty}f(x)\,dx=1.
$$

Without this constant,

the PDF would not represent a valid probability distribution.

---

# Cumulative Distribution Function (CDF)

The CDF is

$$
F_X(x)
=
P(X\le x).
$$

Unlike the PDF,

the CDF gives **actual probabilities**.

It is defined as

$$
F_X(x)
=
\int_{-\infty}^{x}
f_X(t)\,dt.
$$

Properties

- Starts at

  $$
  0.
  $$

- Ends at

  $$
  1.
  $$

- Never decreases.

---

## Important Fact

There is **no elementary closed-form expression** for the Normal CDF.

Instead,

we use

- statistical tables,
- calculators,
- software (NumPy, SciPy, R, etc.).

For example, in Python:

```python
from scipy.stats import norm

norm.cdf(1.5)
norm.pdf(1.5)
norm.ppf(0.975)
```

---

# Standard Normal Distribution

The most important Normal distribution is

$$
Z\sim\mathcal N(0,1).
$$

This is called the **Standard Normal Distribution**.

Its parameters are

Mean:

$$
0
$$

Variance:

$$
1.
$$

Its PDF becomes

$$
f(z)
=
\frac1{\sqrt{2\pi}}
e^{-z^2/2}.
$$

Nearly all Normal probability calculations are converted to this distribution.

---

# Z-Scores

A z-score tells us

> **How many standard deviations an observation is from the mean.**

The formula is

$$
z
=
\frac{x-\mu}{\sigma}.
$$

Interpretation

| Z-score | Meaning |
|---------|---------|
| $0$ | Exactly at the mean |
| $1$ | One standard deviation above the mean |
| $-2$ | Two standard deviations below the mean |
| $3$ | Three standard deviations above the mean |

---

## Example

Suppose

$$
X\sim\mathcal N(70,10^2).
$$

A student scores

$$
85.
$$

The z-score is

$$
z
=
\frac{85-70}{10}
=
1.5.
$$

Interpretation:

The student scored

**1.5 standard deviations above the mean.**

---

# Common Probabilities

The Normal distribution follows the famous

**68–95–99.7 Rule**

Approximately

- **68%** of observations lie within 1 standard deviation of the mean

  $$
  \mu\pm1\sigma.
  $$

- **95%** lie within 2 standard deviations of the mean

  $$
  \mu\pm2\sigma.
  $$

- **99.7%** lie within 3 standard deviations of the mean

  $$
  \mu\pm3\sigma.
  $$

Visualization

```text
             68%

        |-----------|

             95%

    |---------------------|

             99.7%

|-----------------------------|
```

---

## Common Z-Values to Memorize

| Probability | Z-score |
|-------------|---------|
| 90% CI | $\pm1.645$ |
| 95% CI | $\pm1.96$ |
| 99% CI | $\pm2.576$ |

These appear constantly in

- confidence intervals,
- hypothesis testing,
- statistical inference,
- quantitative finance.

---

# Why Is the Standard Normal So Useful?

Suppose

$$
X\sim\mathcal N(\mu,\sigma^2).
$$

Instead of building separate probability tables for every possible Normal distribution,

we standardize every problem using

$$
Z
=
\frac{X-\mu}{\sigma}.
$$

Then

$$
Z\sim\mathcal N(0,1).
$$

This allows every Normal probability problem to be solved using a single table or software function.

---

# Summary

- The Normal distribution is one of the most important probability distributions in statistics.
- It models quantities influenced by many small independent random effects.
- It is completely determined by its mean $\mu$ and variance $\sigma^2$.
- The PDF describes probability density.
- The CDF gives cumulative probabilities.
- The Standard Normal Distribution is $\mathcal N(0,1)$.
- Z-scores measure how many standard deviations an observation is from the mean.
- Nearly all statistical inference—including confidence intervals, hypothesis tests, and the [[Central Limit Theorem]]—is built on the Normal distribution.
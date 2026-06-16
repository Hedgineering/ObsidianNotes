# Expectation, Variance, Covariance, and Correlation

## Quick Reference Table

| Concept | Measures | Formula |
|----------|----------|----------|
| Expectation | Center / long-run average | $E[X] = \sum xP(X=x)$ |
| Variance | Spread around the mean | $\operatorname{Var}(X)=E[(X-\mu)^2]$ |
| Shortcut Variance Formula | Easier variance computation | $\operatorname{Var}(X)=E[X^2]-E[X]^2$ |
| Standard Deviation | Typical distance from the mean | $\sigma = \sqrt{\operatorname{Var}(X)}$ |
| Covariance | Direction of linear relationship | $\operatorname{Cov}(X,Y)=E[(X-\mu_X)(Y-\mu_Y)]$ |
| Covariance Shortcut | Easier covariance computation | $\operatorname{Cov}(X,Y)=E[XY]-E[X]E[Y]$ |
| Correlation | Strength and direction of linear relationship | $\rho_{X,Y}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}$ |

---

# Big Picture

These four concepts answer different questions:

| Question | Statistic |
|-----------|-----------|
| Where is the distribution centered? | Expectation |
| How spread out is it? | Variance |
| Do two variables move together? | Covariance |
| How strongly do they move together? | Correlation |

Think of them as:

- Expectation → average location
- Variance → amount of uncertainty
- Covariance → whether variables move together
- Correlation → standardized covariance

---

# Expectation

## Intuition

Expectation is the long-run average outcome if an experiment is repeated many times.

It is the "center of gravity" of a probability distribution.

Importantly:

> Expectation does NOT have to be a possible outcome.

For example:

$$
E[\text{Fair Die}]
=
3.5
$$

Even though you can never roll a 3.5.

---

## Discrete Formula

For a discrete random variable,

$$
E[X]
=
\sum_x xP(X=x)
$$

---

## Continuous Formula

For a continuous random variable,

$$
E[X]
=
\int_{-\infty}^{\infty}
xf(x)
dx
$$

---

## Properties of Expectation

### Constant Rule

$$
E[c]
=
c
$$

---

### Linearity

$$
E[aX+b]
=
aE[X]+b
$$

---

### Sum Rule

$$
E[X+Y]
=
E[X]+E[Y]
$$

This is true even if $X$ and $Y$ are dependent.

---

## Example 1

A random variable has distribution:

| x | P(X=x) |
|---|---|
| 0 | 0.2 |
| 1 | 0.5 |
| 2 | 0.3 |

Find

$$
E[X]
$$

### Solution

$$
E[X]
=
\sum xP(X=x)
$$

$$
=
0(0.2)+1(0.5)+2(0.3)
$$

$$
=
0+0.5+0.6
$$

$$
=
1.1
$$

---

## Example 2

Suppose

$$
E[X]=10
$$

Find

$$
E[3X+5]
$$

### Solution

Using linearity,

$$
E[3X+5]
=
3E[X]+5
$$

$$
=
3(10)+5
$$

$$
=
35
$$

---

# Variance

## Intuition

Expectation tells us the center.

Variance tells us how much observations fluctuate around that center.

Large variance:

- More uncertainty
- More spread

Small variance:

- Less uncertainty
- Values cluster near the mean

---

## Definition

Let

$$
\mu = E[X]
$$

Then

$$
\operatorname{Var}(X)
=
E[(X-\mu)^2]
$$

---

## Why Square?

If we simply averaged

$$
X-\mu
$$

positive and negative deviations would cancel.

Squaring ensures:

- All deviations become positive
- Large deviations are penalized more heavily

---

## Computational Formula

The most useful formula:

$$
\operatorname{Var}(X)
=
E[X^2]
-
(E[X])^2
$$

---

## Standard Deviation

Variance is measured in squared units.

To return to the original units:

$$
\sigma
=
\sqrt{\operatorname{Var}(X)}
$$

---

## Properties

### Constant

$$
\operatorname{Var}(c)=0
$$

---

### Scaling

$$
\operatorname{Var}(aX)
=
a^2\operatorname{Var}(X)
$$

---

### Shift

$$
\operatorname{Var}(X+b)
=
\operatorname{Var}(X)
$$

Adding a constant changes the center but not the spread.

---

## Example 1

Roll a fair die.

Find the variance.

### Solution

First compute

$$
E[X]
=
3.5
$$

Next,

$$
E[X^2]
=
\frac{
1^2+2^2+3^2+4^2+5^2+6^2
}{6}
$$

$$
=
\frac{91}{6}
$$

Therefore,

$$
\operatorname{Var}(X)
=
E[X^2]-E[X]^2
$$

$$
=
\frac{91}{6}
-
3.5^2
$$

$$
=
\frac{35}{12}
$$

---

## Example 2

Suppose

$$
\operatorname{Var}(X)=4
$$

Find

$$
\operatorname{Var}(3X+10)
$$

### Solution

$$
\operatorname{Var}(3X+10)
=
3^2\operatorname{Var}(X)
$$

$$
=
9(4)
$$

$$
=
36
$$

---

# Covariance

## Intuition

Covariance measures whether two variables move together.

Positive covariance:

- Large values of $X$ tend to occur with large values of $Y$
- Small values of $X$ tend to occur with small values of $Y$

Negative covariance:

- Large values of $X$ tend to occur with small values of $Y$

Near zero covariance:

- No linear relationship

---

## Definition

$$
\operatorname{Cov}(X,Y)
=
E[(X-\mu_X)(Y-\mu_Y)]
$$

---

## Computational Formula

Usually computed using

$$
\operatorname{Cov}(X,Y)
=
E[XY]
-
E[X]E[Y]
$$

---

## Independence

If $X$ and $Y$ are independent,

$$
E[XY]
=
E[X]E[Y]
$$

Therefore,

$$
\operatorname{Cov}(X,Y)
=
0
$$

Important:

$$
\operatorname{Cov}(X,Y)=0
$$

does NOT necessarily imply independence.

The reverse statement is generally false.

---

## Covariance and Variance

Variance is a special case:

$$
\operatorname{Var}(X)
=
\operatorname{Cov}(X,X)
$$

---

## Example 1

Suppose

$$
E[X]=2
$$

$$
E[Y]=5
$$

$$
E[XY]=13
$$

Find

$$
\operatorname{Cov}(X,Y)
$$

### Solution

$$
\operatorname{Cov}(X,Y)
=
E[XY]-E[X]E[Y]
$$

$$
=
13-(2)(5)
$$

$$
=
3
$$

---

## Example 2

Suppose

$$
Y=2X+1
$$

and

$$
\operatorname{Var}(X)=9
$$

Find

$$
\operatorname{Cov}(X,Y)
$$

### Solution

Using

$$
\operatorname{Cov}(X,aX+b)
=
a\operatorname{Var}(X)
$$

we get

$$
\operatorname{Cov}(X,Y)
=
2(9)
$$

$$
=
18
$$

---

# Correlation

## Intuition

Covariance tells us direction but not strength.

The problem is that covariance depends on units.

For example:

- Dollars vs cents
- Inches vs feet

can drastically change covariance.

Correlation fixes this by standardizing covariance.

---

## Definition

$$
\rho_{X,Y}
=
\frac{
\operatorname{Cov}(X,Y)
}{
\sigma_X\sigma_Y
}
$$

---

## Range

Correlation always satisfies

$$
-1
\le
\rho
\le
1
$$

---

## Interpretation

### Perfect Positive Relationship

$$
\rho=1
$$

As one variable increases, the other increases exactly.

---

### Perfect Negative Relationship

$$
\rho=-1
$$

As one variable increases, the other decreases exactly.

---

### No Linear Relationship

$$
\rho=0
$$

No linear relationship exists.

A nonlinear relationship may still exist.

---

## Common Strength Guidelines

| Correlation | Interpretation |
|------------|------------|
| 0.0 - 0.2 | Very weak |
| 0.2 - 0.4 | Weak |
| 0.4 - 0.6 | Moderate |
| 0.6 - 0.8 | Strong |
| 0.8 - 1.0 | Very strong |

These are rough guidelines only.

---

## Example 1

Suppose

$$
\operatorname{Cov}(X,Y)=12
$$

$$
\sigma_X=3
$$

$$
\sigma_Y=4
$$

Find the correlation.

### Solution

$$
\rho
=
\frac{
12
}{
(3)(4)
}
$$

$$
=
1
$$

Perfect positive linear relationship.

---

## Example 2

Suppose

$$
\operatorname{Cov}(X,Y)=-25
$$

$$
\sigma_X=10
$$

$$
\sigma_Y=5
$$

Find the correlation.

### Solution

$$
\rho
=
\frac{-25}
{(10)(5)}
$$

$$
=
-0.5
$$

Moderate negative linear relationship.

---

# Relationship Between the Four Concepts

Expectation describes the center:

$$
E[X]
$$

Variance describes spread:

$$
\operatorname{Var}(X)
=
E[(X-\mu)^2]
$$

Covariance describes joint movement:

$$
\operatorname{Cov}(X,Y)
=
E[(X-\mu_X)(Y-\mu_Y)]
$$

Correlation standardizes covariance:

$$
\rho
=
\frac{
\operatorname{Cov}(X,Y)
}{
\sigma_X\sigma_Y
}
$$

---

# Common Interview Questions

## Question 1

If

$$
Y=aX+b
$$

what is

$$
E[Y]
$$

### Answer

$$
E[Y]
=
aE[X]+b
$$

---

## Question 2

If

$$
Y=aX+b
$$

what is

$$
\operatorname{Var}(Y)
$$

### Answer

$$
\operatorname{Var}(Y)
=
a^2\operatorname{Var}(X)
$$

---

## Question 3

When can covariance be zero?

### Answer

Whenever

$$
E[XY]
=
E[X]E[Y]
$$

This always occurs for independent variables.

---

## Question 4

Can correlation be larger than 1?

### Answer

No.

Correlation always satisfies

$$
-1
\le
\rho
\le
1
$$

---

## Question 5

What is the relationship between variance and covariance?

### Answer

$$
\operatorname{Var}(X)
=
\operatorname{Cov}(X,X)
$$

Variance is simply covariance with itself.

---

# Key Formulas to Memorize

Expectation:

$$
E[X]
=
\sum_x xP(X=x)
$$

Variance:

$$
\operatorname{Var}(X)
=
E[(X-\mu)^2]
$$

Shortcut Variance:

$$
\operatorname{Var}(X)
=
E[X^2]
-
(E[X])^2
$$

Covariance:

$$
\operatorname{Cov}(X,Y)
=
E[(X-\mu_X)(Y-\mu_Y)]
$$

Shortcut Covariance:

$$
\operatorname{Cov}(X,Y)
=
E[XY]
-
E[X]E[Y]
$$

Correlation:

$$
\rho_{X,Y}
=
\frac{
\operatorname{Cov}(X,Y)
}{
\sigma_X\sigma_Y
}
$$
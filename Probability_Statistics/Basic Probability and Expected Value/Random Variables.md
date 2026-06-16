# Random Variables

## Quick Reference Table

| Topic | Meaning | Key Formula |
|---|---|---|
| Random Variable | Function that maps outcomes to numbers | $X:S \to \mathbb{R}$ |
| Discrete PMF | Probability at each value | $p_X(x)=P(X=x)$ |
| Continuous PDF | Density over an interval | $P(a \le X \le b)=\int_a^b f_X(x)\,dx$ |
| CDF | Probability variable is at most $x$ | $F_X(x)=P(X \le x)$ |
| Expectation | Long-run average / center | $E[X]=\sum_x xp_X(x)$ or $E[X]=\int_{-\infty}^{\infty}xf_X(x)\,dx$ |
| Variance | Spread around the mean | $\operatorname{Var}(X)=E[(X-\mu)^2]$ |
| Standard Deviation | Spread in original units | $\sigma_X=\sqrt{\operatorname{Var}(X)}$ |
| Scalar Shift | Add constant | $E[X+c]=E[X]+c$, $\operatorname{Var}(X+c)=\operatorname{Var}(X)$ |
| Scalar Scaling | Multiply by constant | $E[aX]=aE[X]$, $\operatorname{Var}(aX)=a^2\operatorname{Var}(X)$ |
| Sum of Random Variables | Combine variables | $E[X+Y]=E[X]+E[Y]$ |
| Variance of Sum | Spread of combined variables | $\operatorname{Var}(X+Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)+2\operatorname{Cov}(X,Y)$ |
| Independent Sum Variance | If $X,Y$ independent | $\operatorname{Var}(X+Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)$ |
| Function of Random Variable | Transform $X$ into $g(X)$ | $E[g(X)]$ via [[Law of the Unconscious Statistician (LOTUS)]] |

---

# What Is a Random Variable?

A random variable is a function that assigns a number to each outcome in a sample space.

Formally,

$$
X:S \to \mathbb{R}
$$

where:

- $S$ is the sample space
- $X$ is the random variable
- $\mathbb{R}$ is the set of real numbers

A random variable is not the random outcome itself.

It is a numerical summary of the outcome.

---

# Example Intuition

Suppose we flip two coins.

The sample space is

$$
S
=
\{HH,HT,TH,TT\}
$$

Define

$$
X
=
\text{number of heads}
$$

Then

$$
X(HH)=2
$$

$$
X(HT)=1
$$

$$
X(TH)=1
$$

$$
X(TT)=0
$$

So the random variable turns outcomes into numbers.

---

# Types of Random Variables

## Discrete Random Variables

A discrete random variable takes countable values.

Examples:

- Number of heads
- Number of customers
- Number of defective items
- Result of a die roll

Typical values:

$$
0,1,2,3,\ldots
$$

or a finite set such as

$$
1,2,3,4,5,6
$$

---

## Continuous Random Variables

A continuous random variable takes values over intervals.

Examples:

- Height
- Weight
- Time
- Temperature
- Stock return

A continuous random variable can take infinitely many possible values in an interval.

---

# Probability Mass Function (PMF)

For a discrete random variable, the probability mass function gives the probability that $X$ equals a specific value.

$$
p_X(x)
=
P(X=x)
$$

The PMF must satisfy two properties.

First,

$$
p_X(x)
\ge
0
$$

Second,

$$
\sum_x p_X(x)
=
1
$$

---

# Probability Density Function (PDF)

For a continuous random variable, the probability density function describes how probability is distributed over intervals.

The PDF is written as

$$
f_X(x)
$$

For continuous variables,

$$
P(a \le X \le b)
=
\int_a^b f_X(x)\,dx
$$

The PDF must satisfy two properties.

First,

$$
f_X(x)
\ge
0
$$

Second,

$$
\int_{-\infty}^{\infty} f_X(x)\,dx
=
1
$$

Important:

For continuous random variables,

$$
P(X=x)
=
0
$$

for any exact value $x$.

Probabilities come from intervals, not individual points.

---

# Cumulative Distribution Function (CDF)

The cumulative distribution function gives the probability that $X$ is less than or equal to a value.

$$
F_X(x)
=
P(X \le x)
$$

The CDF works for both discrete and continuous random variables.

---

## CDF for a Discrete Random Variable

If $X$ is discrete,

$$
F_X(x)
=
\sum_{t \le x} p_X(t)
$$

---

## CDF for a Continuous Random Variable

If $X$ is continuous,

$$
F_X(x)
=
\int_{-\infty}^{x} f_X(t)\,dt
$$

If $F_X$ is differentiable, then

$$
f_X(x)
=
\frac{d}{dx}F_X(x)
$$

---

# Important CDF Properties

Every CDF satisfies:

$$
0
\le
F_X(x)
\le
1
$$

The CDF is nondecreasing:

$$
x_1 < x_2
\implies
F_X(x_1)
\le
F_X(x_2)
$$

The left limit is

$$
\lim_{x \to -\infty}F_X(x)
=
0
$$

The right limit is

$$
\lim_{x \to \infty}F_X(x)
=
1
$$

For any interval,

$$
P(a < X \le b)
=
F_X(b)-F_X(a)
$$

For continuous random variables,

$$
P(a \le X \le b)
=
F_X(b)-F_X(a)
$$

because exact endpoints have probability zero.

---

# Descriptive Statistics of a Random Variable

The most important descriptive statistics are:

- Mean
- Variance
- Standard deviation
- Median
- Quantiles
- Mode
- Skewness
- Kurtosis

The most commonly used ones are expectation and variance.

---

# Expectation

Expectation measures the center or long-run average of a random variable.

For a discrete random variable,

$$
E[X]
=
\sum_x xp_X(x)
$$

For a continuous random variable,

$$
E[X]
=
\int_{-\infty}^{\infty}
xf_X(x)
\,dx
$$

Expectation is also called the mean.

Usually,

$$
\mu_X
=
E[X]
$$

---

# Expectation of a Function of a Random Variable

If we transform $X$ using a function $g$, then $g(X)$ is also a random variable.

Using [[Law of the Unconscious Statistician (LOTUS)]],

for discrete $X$:

$$
E[g(X)]
=
\sum_x g(x)p_X(x)
$$

For continuous $X$:

$$
E[g(X)]
=
\int_{-\infty}^{\infty}
g(x)f_X(x)
\,dx
$$

This lets us compute expectations of transformed random variables without needing to derive the full distribution of $g(X)$ first.

---

# Variance

Variance measures spread around the mean.

Let

$$
\mu_X
=
E[X]
$$

Then

$$
\operatorname{Var}(X)
=
E[(X-\mu_X)^2]
$$

The shortcut formula is

$$
\operatorname{Var}(X)
=
E[X^2]
-
(E[X])^2
$$

---

# Standard Deviation

Standard deviation is the square root of variance.

$$
\sigma_X
=
\sqrt{\operatorname{Var}(X)}
$$

Variance is in squared units.

Standard deviation is in the original units of $X$.

---

# Median

A median is a value $m$ such that at least half the probability is at or below $m$, and at least half is at or above $m$.

For continuous random variables, the median often satisfies

$$
F_X(m)
=
0.5
$$

The median describes the middle of the distribution.

Unlike the mean, it is less sensitive to extreme values.

---

# Quantiles

The $p$-quantile is a value $q_p$ satisfying

$$
F_X(q_p)
=
p
$$

For example:

- $q_{0.25}$ is the first quartile
- $q_{0.50}$ is the median
- $q_{0.75}$ is the third quartile

---

# Mode

The mode is the most likely value.

For a discrete random variable,

$$
\text{mode}
=
\arg\max_x p_X(x)
$$

For a continuous random variable,

$$
\text{mode}
=
\arg\max_x f_X(x)
$$

For continuous variables, the mode is the value where the density is highest, not necessarily where probability mass is concentrated at a single point.

---

# Shifting a Random Variable

Suppose

$$
Y=X+c
$$

Then the expectation shifts by $c$:

$$
E[Y]
=
E[X+c]
=
E[X]+c
$$

The variance does not change:

$$
\operatorname{Var}(Y)
=
\operatorname{Var}(X+c)
=
\operatorname{Var}(X)
$$

Intuition:

Adding a constant moves the whole distribution left or right, but does not change its spread.

---

# Scaling a Random Variable

Suppose

$$
Y=aX
$$

Then the expectation scales by $a$:

$$
E[Y]
=
E[aX]
=
aE[X]
$$

The variance scales by $a^2$:

$$
\operatorname{Var}(Y)
=
\operatorname{Var}(aX)
=
a^2\operatorname{Var}(X)
$$

The standard deviation scales by $\lvert a \rvert$:

$$
\sigma_Y
=
\lvert a \rvert \sigma_X
$$

Intuition:

Multiplying by $a$ stretches or compresses the distribution.

If $a$ is negative, the distribution is also reflected.

---

# Affine Transformation

An affine transformation has the form

$$
Y=aX+b
$$

Expectation:

$$
E[Y]
=
E[aX+b]
=
aE[X]+b
$$

Variance:

$$
\operatorname{Var}(Y)
=
\operatorname{Var}(aX+b)
=
a^2\operatorname{Var}(X)
$$

Standard deviation:

$$
\sigma_Y
=
\lvert a \rvert \sigma_X
$$

---

# Adding Random Variables

Suppose

$$
Z=X+Y
$$

Expectation is always linear:

$$
E[Z]
=
E[X+Y]
=
E[X]+E[Y]
$$

This is true whether or not $X$ and $Y$ are independent.

Variance is different.

In general,

$$
\operatorname{Var}(X+Y)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
+
2\operatorname{Cov}(X,Y)
$$

If $X$ and $Y$ are independent, then

$$
\operatorname{Cov}(X,Y)
=
0
$$

so

$$
\operatorname{Var}(X+Y)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
$$

---

# Subtracting Random Variables

Suppose

$$
Z=X-Y
$$

Expectation:

$$
E[X-Y]
=
E[X]-E[Y]
$$

Variance:

$$
\operatorname{Var}(X-Y)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
-
2\operatorname{Cov}(X,Y)
$$

If $X$ and $Y$ are independent,

$$
\operatorname{Var}(X-Y)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
$$

Important:

Variance adds for independent variables even when the variables are subtracted.

---

# Linear Combination of Random Variables

For

$$
Z
=
aX+bY+c
$$

Expectation:

$$
E[Z]
=
aE[X]+bE[Y]+c
$$

Variance:

$$
\operatorname{Var}(Z)
=
a^2\operatorname{Var}(X)
+
b^2\operatorname{Var}(Y)
+
2ab\operatorname{Cov}(X,Y)
$$

If $X$ and $Y$ are independent,

$$
\operatorname{Var}(Z)
=
a^2\operatorname{Var}(X)
+
b^2\operatorname{Var}(Y)
$$

---

# Sum of Many Random Variables

Suppose

$$
S_n
=
X_1+X_2+\cdots+X_n
$$

Then

$$
E[S_n]
=
E[X_1]+E[X_2]+\cdots+E[X_n]
$$

If all variables have the same mean $\mu$,

$$
E[S_n]
=
n\mu
$$

In general,

$$
\operatorname{Var}(S_n)
=
\sum_{i=1}^{n}
\operatorname{Var}(X_i)
+
2
\sum_{i<j}
\operatorname{Cov}(X_i,X_j)
$$

If the variables are independent,

$$
\operatorname{Var}(S_n)
=
\sum_{i=1}^{n}
\operatorname{Var}(X_i)
$$

If they are independent and identically distributed with variance $\sigma^2$,

$$
\operatorname{Var}(S_n)
=
n\sigma^2
$$

---

# Sample Mean of Random Variables

Suppose

$$
\bar{X}_n
=
\frac{1}{n}
\sum_{i=1}^{n}X_i
$$

If $X_1,\ldots,X_n$ are i.i.d. with

$$
E[X_i]=\mu
$$

and

$$
\operatorname{Var}(X_i)=\sigma^2
$$

then

$$
E[\bar{X}_n]
=
\mu
$$

and

$$
\operatorname{Var}(\bar{X}_n)
=
\frac{\sigma^2}{n}
$$

This is one reason the sample mean becomes more stable as $n$ grows.

This connects directly to the [[Law of Large Numbers]].

---

# Product of Random Variables

In general,

$$
E[XY]
\ne
E[X]E[Y]
$$

However, if $X$ and $Y$ are independent,

$$
E[XY]
=
E[X]E[Y]
$$

This fact is often used when computing covariance:

$$
\operatorname{Cov}(X,Y)
=
E[XY]-E[X]E[Y]
$$

If $X$ and $Y$ are independent, then

$$
\operatorname{Cov}(X,Y)
=
0
$$

---

# Nonlinear Transformations

If

$$
Y=g(X)
$$

then expectation usually does not pass through the function.

In general,

$$
E[g(X)]
\ne
g(E[X])
$$

For example,

$$
E[X^2]
\ne
(E[X])^2
$$

unless $X$ has zero variance.

Use [[Law of the Unconscious Statistician (LOTUS)]] to compute

$$
E[g(X)]
$$

---

# Common Nonlinear Transformation Examples

If

$$
Y=X^2
$$

then

$$
E[Y]
=
E[X^2]
$$

If

$$
Y=e^X
$$

then

$$
E[Y]
=
E[e^X]
$$

If

$$
Y=\log(X)
$$

then

$$
E[Y]
=
E[\log(X)]
$$

These are not generally equal to

$$
(E[X])^2,
$$

$$
e^{E[X]},
$$

or

$$
\log(E[X]).
$$

---

# Distribution of a Transformed Random Variable

Sometimes we need the full distribution of

$$
Y=g(X)
$$

not just its expectation.

There are different approaches depending on whether $X$ is discrete or continuous.

---

## Transformation of a Discrete Random Variable

If $X$ is discrete and

$$
Y=g(X),
$$

then for each possible value $y$,

$$
P(Y=y)
=
\sum_{x:g(x)=y}
P(X=x)
$$

Intuition:

Add up the probabilities of all $x$ values that map to the same $y$.

---

## Transformation of a Continuous Random Variable: CDF Method

If

$$
Y=g(X),
$$

then start from the CDF:

$$
F_Y(y)
=
P(Y \le y)
$$

Substitute:

$$
F_Y(y)
=
P(g(X) \le y)
$$

Then solve the inequality in terms of $X$.

Once you find $F_Y(y)$, differentiate if needed:

$$
f_Y(y)
=
\frac{d}{dy}F_Y(y)
$$

---

## Transformation of a Continuous Random Variable: Monotone Formula

If $Y=g(X)$ and $g$ is one-to-one and differentiable, then

$$
f_Y(y)
=
f_X(g^{-1}(y))
\left|
\frac{d}{dy}g^{-1}(y)
\right|
$$

where $g^{-1}$ is the inverse transformation.

---

# Joint Random Variables

Two random variables $X$ and $Y$ can be studied together.

Their joint distribution describes probabilities involving both variables.

---

## Joint PMF

For discrete variables,

$$
p_{X,Y}(x,y)
=
P(X=x,Y=y)
$$

The joint PMF must satisfy

$$
p_{X,Y}(x,y)
\ge
0
$$

and

$$
\sum_x\sum_y p_{X,Y}(x,y)
=
1
$$

---

## Joint PDF

For continuous variables,

$$
f_{X,Y}(x,y)
$$

satisfies

$$
f_{X,Y}(x,y)
\ge
0
$$

and

$$
\int_{-\infty}^{\infty}
\int_{-\infty}^{\infty}
f_{X,Y}(x,y)
\,dx\,dy
=
1
$$

For a region $A$,

$$
P((X,Y)\in A)
=
\iint_A f_{X,Y}(x,y)
\,dx\,dy
$$

---

# Marginal Distributions

A marginal distribution describes one variable by itself after ignoring the other variable.

For discrete variables,

$$
p_X(x)
=
\sum_y p_{X,Y}(x,y)
$$

and

$$
p_Y(y)
=
\sum_x p_{X,Y}(x,y)
$$

For continuous variables,

$$
f_X(x)
=
\int_{-\infty}^{\infty}
f_{X,Y}(x,y)
\,dy
$$

and

$$
f_Y(y)
=
\int_{-\infty}^{\infty}
f_{X,Y}(x,y)
\,dx
$$

---

# Conditional Distributions

Conditional distributions describe one random variable given information about another.

For discrete variables,

$$
p_{X \mid Y}(x \mid y)
=
\frac{p_{X,Y}(x,y)}
{p_Y(y)}
$$

provided

$$
p_Y(y)>0
$$

For continuous variables,

$$
f_{X \mid Y}(x \mid y)
=
\frac{f_{X,Y}(x,y)}
{f_Y(y)}
$$

provided

$$
f_Y(y)>0
$$

---

# Independence of Random Variables

Two random variables $X$ and $Y$ are independent if knowing one gives no information about the other.

For discrete variables,

$$
p_{X,Y}(x,y)
=
p_X(x)p_Y(y)
$$

for all $x,y$.

For continuous variables,

$$
f_{X,Y}(x,y)
=
f_X(x)f_Y(y)
$$

for all $x,y$.

Equivalently,

$$
F_{X,Y}(x,y)
=
F_X(x)F_Y(y)
$$

for all $x,y$.

---

# Covariance

Covariance measures whether two variables tend to move together.

Definition:

$$
\operatorname{Cov}(X,Y)
=
E[(X-E[X])(Y-E[Y])]
$$

Shortcut formula:

$$
\operatorname{Cov}(X,Y)
=
E[XY]-E[X]E[Y]
$$

Positive covariance means $X$ and $Y$ tend to move in the same direction.

Negative covariance means they tend to move in opposite directions.

Zero covariance means no linear relationship, but not necessarily independence.

---

# Correlation

Correlation standardizes covariance.

$$
\rho_{X,Y}
=
\frac{
\operatorname{Cov}(X,Y)
}{
\sigma_X\sigma_Y
}
$$

Correlation always satisfies

$$
-1
\le
\rho_{X,Y}
\le
1
$$

Correlation is unitless.

It measures strength and direction of linear association.

---

# Moment Generating Functions

The moment generating function of $X$ is

$$
M_X(t)
=
E[e^{tX}]
$$

when this expectation exists near $t=0$.

Moments can be found by differentiating:

$$
E[X]
=
M_X'(0)
$$

$$
E[X^2]
=
M_X''(0)
$$

More generally,

$$
E[X^n]
=
M_X^{(n)}(0)
$$

Moment generating functions can also help identify distributions and compute sums of independent random variables.

---

# Summary of Transformation Rules

## Shift

If

$$
Y=X+c,
$$

then

$$
E[Y]
=
E[X]+c
$$

$$
\operatorname{Var}(Y)
=
\operatorname{Var}(X)
$$

---

## Scale

If

$$
Y=aX,
$$

then

$$
E[Y]
=
aE[X]
$$

$$
\operatorname{Var}(Y)
=
a^2\operatorname{Var}(X)
$$

---

## Scale and Shift

If

$$
Y=aX+b,
$$

then

$$
E[Y]
=
aE[X]+b
$$

$$
\operatorname{Var}(Y)
=
a^2\operatorname{Var}(X)
$$

---

## Sum

If

$$
Z=X+Y,
$$

then

$$
E[Z]
=
E[X]+E[Y]
$$

$$
\operatorname{Var}(Z)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
+
2\operatorname{Cov}(X,Y)
$$

---

## Difference

If

$$
Z=X-Y,
$$

then

$$
E[Z]
=
E[X]-E[Y]
$$

$$
\operatorname{Var}(Z)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
-
2\operatorname{Cov}(X,Y)
$$

---

## Independent Sum

If $X$ and $Y$ are independent, then

$$
\operatorname{Var}(X+Y)
=
\operatorname{Var}(X)
+
\operatorname{Var}(Y)
$$

---

## Function

If

$$
Y=g(X),
$$

then generally

$$
E[Y]
=
E[g(X)]
$$

Use [[Law of the Unconscious Statistician (LOTUS)]]:

Discrete case:

$$
E[g(X)]
=
\sum_x g(x)p_X(x)
$$

Continuous case:

$$
E[g(X)]
=
\int_{-\infty}^{\infty}
g(x)f_X(x)
\,dx
$$

---

# Common Mistakes

## Mistake 1: Treating a PDF Like a Probability

For continuous random variables,

$$
f_X(x)
$$

is not a probability.

Instead,

$$
P(a \le X \le b)
=
\int_a^b f_X(x)\,dx
$$

---

## Mistake 2: Thinking $P(X=x)$ Matters for Continuous Variables

For continuous $X$,

$$
P(X=x)
=
0
$$

Probability comes from intervals.

---

## Mistake 3: Assuming Variance Is Linear

Expectation is linear:

$$
E[X+Y]
=
E[X]+E[Y]
$$

Variance is not generally linear:

$$
\operatorname{Var}(X+Y)
\ne
\operatorname{Var}(X)+\operatorname{Var}(Y)
$$

unless $X$ and $Y$ are uncorrelated.

---

## Mistake 4: Forgetting the Square When Scaling Variance

If

$$
Y=aX,
$$

then

$$
\operatorname{Var}(Y)
=
a^2\operatorname{Var}(X)
$$

not

$$
a\operatorname{Var}(X)
$$

---

## Mistake 5: Assuming $E[g(X)]=g(E[X])$

In general,

$$
E[g(X)]
\ne
g(E[X])
$$

For nonlinear functions, use [[Law of the Unconscious Statistician (LOTUS)]].

---

# Key Takeaways

A random variable maps outcomes to numbers:

$$
X:S \to \mathbb{R}
$$

Discrete random variables use PMFs:

$$
p_X(x)
=
P(X=x)
$$

Continuous random variables use PDFs:

$$
P(a \le X \le b)
=
\int_a^b f_X(x)\,dx
$$

All random variables have CDFs:

$$
F_X(x)
=
P(X \le x)
$$

Expectation describes center:

$$
E[X]
$$

Variance describes spread:

$$
\operatorname{Var}(X)
$$

Covariance describes joint movement:

$$
\operatorname{Cov}(X,Y)
$$

Correlation standardizes covariance:

$$
\rho_{X,Y}
$$

Linearity of expectation always holds:

$$
E[aX+bY+c]
=
aE[X]+bE[Y]+c
$$

Variance depends on covariance:

$$
\operatorname{Var}(aX+bY)
=
a^2\operatorname{Var}(X)
+
b^2\operatorname{Var}(Y)
+
2ab\operatorname{Cov}(X,Y)
$$
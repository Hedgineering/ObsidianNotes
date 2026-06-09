# Central Limit Theorem (CLT)

## Motivation

Many real-world quantities are the result of **many small independent effects being added together**.

Examples:

- A person's height is influenced by many genetic and environmental factors.
- Measurement error is the sum of many tiny sources of noise.
- The average return of a portfolio is the combined effect of many individual asset movements.
- The average of repeated experiments accumulates many small random fluctuations.

Even when the individual random variables are **not normally distributed**, their average often behaves approximately like a Normal random variable.

The Central Limit Theorem explains why the Normal distribution appears so frequently in nature, science, and statistics.

---

## Statement of the Central Limit Theorem

Let

$$
X_1, X_2, \dots, X_n
\overset{\text{i.i.d.}}{\sim}
\text{any distribution}
$$

with

$$
E[X_i] = \mu
$$

and

$$
\operatorname{Var}(X_i) = \sigma^2 < \infty.
$$

Define the sample mean

$$
\bar X_n
=
\frac{1}{n}
\sum_{i=1}^{n}
X_i.
$$

Then

$$
\frac{\bar X_n - \mu}
{\sigma/\sqrt{n}}
\overset{d}{\longrightarrow}
N(0,1)
\qquad
\text{as }
n \to \infty.
$$

Equivalently,

$$
\bar X_n
\approx
N\!\left(
\mu,
\frac{\sigma^2}{n}
\right)
\qquad
\text{for large } n.
$$

---

## Intuition

Suppose we repeatedly average more and more independent observations:

$$
\bar X_n
=
\frac{X_1 + X_2 + \cdots + X_n}{n}.
$$

Each observation contributes a little randomness.

The positive and negative fluctuations tend to cancel out.

As more observations are added:

- The center stays near $\mu$.
- The spread shrinks.
- The shape becomes increasingly bell-shaped.

Remarkably, this happens regardless of the original distribution.

For example:

- Uniform distributions become Normal.
- Exponential distributions become Normal.
- Bernoulli distributions become Normal.
- Dice rolls become Normal.

The Normal distribution acts like a universal attractor for averages.

---

## Why the Standardization?

The variance of the sample mean is

$$
\operatorname{Var}(\bar X_n)
=
\frac{\sigma^2}{n}.
$$

As $n$ grows,

$$
\operatorname{Var}(\bar X_n)
\to 0.
$$

Without rescaling, the distribution collapses to a single point at $\mu$.

To keep a non-degenerate distribution, we standardize:

$$
Z_n
=
\frac{\bar X_n-\mu}
{\sigma/\sqrt{n}}.
$$

Now

$$
E[Z_n]=0
$$

and

$$
\operatorname{Var}(Z_n)=1.
$$

The CLT says that these standardized averages converge to

$$
N(0,1).
$$

---

## Derivation Using Moment Generating Functions

Let

$$
Y_i
=
\frac{X_i-\mu}{\sigma}.
$$

Then

$$
E[Y_i]=0,
\qquad
\operatorname{Var}(Y_i)=1.
$$

Define

$$
Z_n
=
\frac{1}{\sqrt n}
\sum_{i=1}^{n}
Y_i.
$$

We compute the MGF of $Z_n$.

Because the $Y_i$ are independent,

$$
M_{Z_n}(t)
=
\left(
M_Y\!\left(
\frac{t}{\sqrt n}
\right)
\right)^n.
$$

Using a Taylor expansion around $0$,

$$
M_Y(s)
=
1
+
\frac{s^2}{2}
+
o(s^2),
$$

since

$$
E[Y]=0,
\qquad
E[Y^2]=1.
$$

Substituting

$$
s=\frac{t}{\sqrt n},
$$

gives

$$
M_Y\!\left(
\frac{t}{\sqrt n}
\right)
=
1
+
\frac{t^2}{2n}
+
o\!\left(\frac1n\right).
$$

Therefore,

$$
M_{Z_n}(t)
=
\left(
1
+
\frac{t^2}{2n}
+
o\!\left(\frac1n\right)
\right)^n.
$$

Using

$$
\left(
1+\frac{x}{n}
\right)^n
\to
e^x,
$$

we obtain

$$
M_{Z_n}(t)
\to
e^{t^2/2}.
$$

But

$$
e^{t^2/2}
$$

is exactly the MGF of

$$
N(0,1).
$$

Hence

$$
Z_n
\overset{d}{\longrightarrow}
N(0,1).
$$

This proves the CLT.

---

## Example 1: Coin Flips

Let

$$
X_i
=
\begin{cases}
1,&\text{Heads}\\
0,&\text{Tails}
\end{cases}
$$

with

$$
P(X_i=1)=0.5.
$$

Then

$$
\mu=0.5,
\qquad
\sigma^2=0.25.
$$

The sample proportion of heads is

$$
\bar X_n.
$$

For large $n$,

$$
\bar X_n
\approx
N\!\left(
0.5,
\frac{0.25}{n}
\right).
$$

For example, if

$$
n=100,
$$

then

$$
\bar X_{100}
\approx
N(0.5,0.0025).
$$

The probability of obtaining between 45 and 55 heads can be approximated using the Normal distribution.

---

## Example 2: Rolling a Die

Suppose

$$
X_i
\sim
\text{Uniform}\{1,2,3,4,5,6\}.
$$

Then

$$
\mu
=
3.5
$$

and

$$
\sigma^2
=
\frac{35}{12}.
$$

For

$$
n=100,
$$

the average roll is approximately

$$
\bar X_{100}
\approx
N\!\left(
3.5,
\frac{35}{1200}
\right).
$$

Even though a single die roll is discrete and far from Normal, the average of many rolls is nearly Normal.

---

## Example 3: Uniform Distribution

Suppose

$$
X_i
\sim
\mathrm{Unif}(0,1).
$$

Then

$$
\mu
=
\frac12
$$

and

$$
\sigma^2
=
\frac1{12}.
$$

For large $n$,

$$
\bar X_n
\approx
N\!\left(
\frac12,
\frac1{12n}
\right).
$$

The average of Uniform random variables becomes bell-shaped even though the original variables are flat.

---

## Relationship to the Law of Large Numbers

The Law of Large Numbers says

$$
\bar X_n
\to
\mu.
$$

The CLT goes further.

It tells us:

1. How fast convergence occurs.
2. How much uncertainty remains.
3. What distribution describes that uncertainty.

The LLN says

$$
\bar X_n \to \mu.
$$

The CLT says

$$
\bar X_n
\approx
N\!\left(
\mu,
\frac{\sigma^2}{n}
\right).
$$

for large $n$.

---

## Standard Error

The standard deviation of the sample mean is

$$
\operatorname{SE}(\bar X_n)
=
\sqrt{
\operatorname{Var}(\bar X_n)
}
=
\frac{\sigma}{\sqrt n}.
$$

This is called the **standard error**.

Doubling the sample size does not cut uncertainty in half.

Instead,

$$
\operatorname{SE}
\propto
\frac{1}{\sqrt n}.
$$

To reduce uncertainty by a factor of 2, we need roughly 4 times as many observations.

---

## Practical Rule of Thumb

A common rule is:

$$
n \ge 30
$$

is often enough for a decent Normal approximation.

However:

- Highly skewed distributions require larger $n$.
- Heavy-tailed distributions require larger $n$.
- Infinite-variance distributions may violate the CLT assumptions entirely.

---

## Key Takeaway

The Central Limit Theorem states that if

$$
X_1,\dots,X_n
$$

are i.i.d. with finite mean and variance, then

$$
\frac{\bar X_n-\mu}
{\sigma/\sqrt n}
\overset{d}{\longrightarrow}
N(0,1).
$$

Equivalently,

$$
\bar X_n
\approx
N\!\left(
\mu,
\frac{\sigma^2}{n}
\right)
\quad
\text{for large } n.
$$

No matter what distribution generated the data, averages tend toward a Normal distribution. This fact is the foundation of confidence intervals, hypothesis tests, Monte Carlo methods, and much of modern statistics.

The Law of the Unconscious Statistician (LOTUS) is a shortcut for computing expectations of functions of random variables.

Instead of:

1. Finding the distribution of $Y=g(X)$,
2. Then computing $\mathbb{E}[Y]$,

we can compute the expectation directly from the distribution of $X$.

---

## Discrete Version

If $X$ is a discrete random variable with pmf $p(x)$, then

$$
\mathbb{E}[g(X)]
=
\sum_x g(x)p(x).
$$

---

## Continuous Version

If $X$ is a continuous random variable with pdf $f(x)$, then

$$
\mathbb{E}[g(X)]
=
\int_{-\infty}^{\infty}
g(x)f(x)\,dx.
$$

---

## Intuition

Normally, if

$$
Y=g(X),
$$

you might think:

$$
\mathbb{E}[Y]
=
\mathbb{E}[g(X)]
$$

requires finding the distribution of $Y$ first.

LOTUS says:

> Don't bother finding the distribution of $Y$.
> Just plug $g(X)$ directly into the expectation formula for $X$.

That is why statisticians jokingly call it the **Law of the Unconscious Statistician** — people often use it without realizing they are changing variables.

---

## Example 1: Compute $\mathbb{E}[X^2]$

Let

$$
X \sim \mathrm{Unif}(0,1).
$$

We want

$$
\mathbb{E}[X^2].
$$

Using LOTUS:

$$
\mathbb{E}[X^2]
=
\int_0^1 x^2\,dx
=
\left[\frac{x^3}{3}\right]_0^1
=
\frac13.
$$

No need to find the distribution of $X^2$.

---

## Example 2: Compute $\mathbb{E}[e^X]$

Let

$$
X \sim \mathrm{Unif}(0,1).
$$

Then

$$
\mathbb{E}[e^X]
=
\int_0^1 e^x\,dx
=
\left[e^x\right]_0^1
=
e-1.
$$

Again, we never compute the distribution of $e^X$.

---

## Example 3: Recover the Mean of a Uniform

Let

$$
X \sim \mathrm{Unif}(0,1).
$$

Take

$$
g(X)=X.
$$

Then

$$
\mathbb{E}[X]
=
\int_0^1 x\,dx
=
\frac12.
$$

LOTUS contains the ordinary expectation formula as a special case.

---

## Example 4: Monte Carlo Integration

From your notes:

$$
I_i
=
(b-a)
g(a+(b-a)U_i)
$$

where

$$
U_i \sim \mathrm{Unif}(0,1).
$$

Using LOTUS:

$$
\mathbb{E}[I_i]
=
(b-a)
\mathbb{E}
\!\left[
g(a+(b-a)U_i)
\right]
$$

$$
=
(b-a)
\int_0^1
g(a+(b-a)u)\,du
$$

$$
=
I.
$$

This shows the Monte Carlo estimator is unbiased.

---

## Example 5: Variance via LOTUS

Suppose

$$
X \sim \mathrm{Unif}(0,1).
$$

To compute

$$
\mathrm{Var}(X),
$$

first compute

$$
\mathbb{E}[X^2]
=
\int_0^1 x^2\,dx
=
\frac13.
$$

and

$$
\mathbb{E}[X]
=
\frac12.
$$

Then

$$
\mathrm{Var}(X)
=
\mathbb{E}[X^2]
-
(\mathbb{E}[X])^2
=
\frac13-\frac14
=
\frac1{12}.
$$

---

## The One-Line Rule

If you remember only one thing:

$$
\boxed{
\mathbb{E}[g(X)]
=
\int g(x)f(x)\,dx
}
$$

(or a sum for discrete random variables).

Just plug the function you care about into the expectation formula for the original random variable.
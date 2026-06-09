# Integration

**Definition**  
The function $F(x)$ having derivative $f(x)$ is called the *antiderivative*.

The antiderivative is denoted

$$
F(x) = \int f(x)\,dx
$$

and this is also called the *indefinite integral* of $f(x)$.

## Fundamental Theorem of Calculus

If $f(x)$ is continuous, then the area under the curve for $x \in [a,b]$ is denoted and given by the *definite integral*

$$
\int_a^b f(x)\,dx
=
F(x)\Big|_a^b
=
F(b)-F(a).
$$
## Integration Steps
Let's integrate

$$
I
=
\int_a^b g(x)\,dx
=
(b-a)
\int_0^1
g\!\left(a+(b-a)u\right)\,du
$$

where we get the last equality by substituting

$$
u = \frac{x-a}{b-a}.
$$

We can do this with something like analytical calculus or Gauss-Laguerre integration, but if these are not possible, we can use **Monte Carlo Integration**.

---
Suppose $U_1, U_2, \ldots$ are Independent and Identically Distributed uniform random variables from 0 to 1 $\mathrm{Unif}(0,1)$, and define

$$
I_i
\equiv
(b-a)\,
g\!\left(a+(b-a)U_i\right),
\qquad
i=1,2,\ldots,n.
$$

This is essentially generating random values along the x axis (domain) of integration.

We can use the sample average as an estimator for $I$:

$$
\bar{I}_n
\equiv
\frac{1}{n}
\sum_{i=1}^{n} I_i
=
\frac{b-a}{n}
\sum_{i=1}^{n}
g\!\left(a+(b-a)U_i\right).
$$
This is essentially getting corresponding y values for the random x values we generated and finding the mean of those y values.

Why is this okay? Let's appeal to our old friend, the Law of Large Numbers: If an estimator is asymptotically unbiased and its variance goes to zero, then things are good.

### Expectation of the Integral via Monte Carlo

First, by the [[Law of the Unconscious Statistician (LOTUS)]], notice that

$$
\mathbb{E}[\bar{I}_n]
=
(b-a)\,
\mathbb{E}
\!\left[
g\!\left(a+(b-a)U_i\right)
\right]
$$

$$
=
(b-a)
\int_{\mathbb{R}}
g\!\left(a+(b-a)u\right)
f(u)\,du
$$
$f(u)=1$ only when the domain is between 0 and 1, as a property of the PDF of $Unif(0, 1)$ so where $f(u)$ is the $\mathrm{Unif}(0,1)$ pdf this simplifies to the definition of the original Integral value $I$ we defined above:

$$
=
(b-a)
\int_0^1
g\!\left(a+(b-a)u\right)\,du
=
I.
$$
So $\bar{I}_n$ is unbiased for $I$  -- that is to say, the expectation of the result of this integration method using random variables is consistent with the true analytical definition of the integral.

### Variance of the Integral via Monte Carlo

Suppose

$$
\bar{I}_n
=
\frac{1}{n}
\sum_{i=1}^{n}
Y_i,
$$

where the $Y_i$ are i.i.d. random variables with finite variance

$$
\operatorname{Var}(Y_i)=\sigma^2 < \infty.
$$

Using the variance of a sum of independent random variables,

$$
\operatorname{Var}\!\left(
\sum_{i=1}^{n} Y_i
\right)
=
\sum_{i=1}^{n}
\operatorname{Var}(Y_i)
=
n\sigma^2.
$$

Since variance scales quadratically with constants,

$$
\operatorname{Var}(\bar{I}_n)
=
\operatorname{Var}\!\left(
\frac{1}{n}
\sum_{i=1}^{n}Y_i
\right)
=
\frac{1}{n^2}
\operatorname{Var}\!\left(
\sum_{i=1}^{n}Y_i
\right).
$$

Substituting the previous result gives

$$
\operatorname{Var}(\bar{I}_n)
=
\frac{1}{n^2}
(n\sigma^2)
=
\frac{\sigma^2}{n}.
$$

Therefore in terms of Big O notation, this is asymptotically equivalent to $\frac{1}{n}$, which is numerically stable as $n$ grows very large:

$$
\operatorname{Var}(\bar{I}_n)
=
\frac{\sigma^2}{n}
=
O\!\left(\frac{1}{n}\right).
$$

The intuition is that averaging $n$ independent samples reduces the variance by a factor of $n$. Equivalently, the standard deviation (standard error) decreases like

$$
\sqrt{\operatorname{Var}(\bar{I}_n)}
=
\frac{\sigma}{\sqrt{n}}.
$$

Therefore, since it can also be shown that

$$
\operatorname{Var}(\bar{I}_n)
=
O\!\left(\frac{1}{n}\right),
$$

the Law of Large Numbers implies

$$
\bar{I}_n
\to
I
\qquad
\text{as }
n \to \infty.
$$

## Approximate Confidence Interval for $I$

By the [[Central Limit Theorem (CLT)]], we have

$$
\bar{I}_n
\approx
\operatorname{Nor}
\!\left(
\mathbb{E}[\bar{I}_n],
\operatorname{Var}(\bar{I}_n)
\right)
\sim
\operatorname{Nor}
\!\left(
I,
\frac{\operatorname{Var}(I_i)}{n}
\right).
$$

This suggests that a reasonable $100(1-\alpha)\%$ confidence interval for $I$ is

$$
I
\in
\bar{I}_n
\pm
z_{\alpha/2}
\sqrt{\frac{S_I^2}{n}},
$$

where $z_{\alpha/2}$ is the usual standard normal quantile, and

$$
S_I^2
\equiv
\frac{1}{n-1}
\sum_{i=1}^{n}
\left(I_i-\bar{I}_n\right)^2
$$

is the sample variance of the $I_i$'s.

### Example: Monte Carlo Integration with a Confidence Interval

Suppose we want to estimate

$$
I
=
\int_0^1 \sin(\pi x)\,dx,
$$

and pretend we do **not** know the true answer

$$
I
=
\frac{2}{\pi}
\approx
0.6366.
$$

Generate

$$
n=4
$$

independent samples from

$$
U_i \sim \mathrm{Unif}(0,1):
$$

$$
U_1 = 0.79,
\qquad
U_2 = 0.11,
\qquad
U_3 = 0.68,
\qquad
U_4 = 0.31.
$$

Since

$$
I
=
\int_0^1 g(x)\,dx
\qquad
\text{with}
\qquad
g(x)=\sin(\pi x),
$$

our Monte Carlo observations are

$$
I_i
=
g(U_i)
=
\sin(\pi U_i).
$$

Evaluating:

$$
I_1
=
\sin(\pi\cdot0.79)
=
0.613,
$$

$$
I_2
=
\sin(\pi\cdot0.11)
=
0.339,
$$

$$
I_3
=
\sin(\pi\cdot0.68)
=
0.844,
$$

$$
I_4
=
\sin(\pi\cdot0.31)
=
0.827.
$$

The Monte Carlo estimator is

$$
\bar I_n
=
\frac{1}{4}
\sum_{i=1}^{4} I_i
=
\frac{0.613+0.339+0.844+0.827}{4}
=
0.656.
$$

Thus our estimate of the integral is

$$
\hat I
=
0.656.
$$

---

### Computing the Sample Variance

First compute

$$
S_I^2
=
\frac{1}{n-1}
\sum_{i=1}^{n}
(I_i-\bar I_n)^2.
$$

Using

$$
\bar I_n = 0.656,
$$

we obtain

$$
S_I^2
=
\frac{
(0.613-0.656)^2
+
(0.339-0.656)^2
+
(0.844-0.656)^2
+
(0.827-0.656)^2
}{3}
\approx
0.056.
$$

Therefore

$$
S_I
=
\sqrt{0.056}
\approx
0.237.
$$

---

### Standard Error

The estimated standard error of the Monte Carlo estimator is

$$
\operatorname{SE}(\bar I_n)
=
\sqrt{\frac{S_I^2}{n}}
=
\sqrt{\frac{0.056}{4}}
=
0.118.
$$

---

### 95\% Confidence Interval

Using

$$
z_{0.025}
=
1.96,
$$

the approximate 95\% confidence interval is

$$
\bar I_n
\pm
1.96
\sqrt{\frac{S_I^2}{n}}.
$$

Substituting values gives

$$
0.656
\pm
1.96(0.118)
=
0.656
\pm
0.232.
$$

Therefore,

$$
I
\in
(0.424,\;0.888).
$$

---

### Interpretation

Our Monte Carlo estimate is

$$
\hat I = 0.656,
$$

with approximate 95\% confidence interval

$$
(0.424,\;0.888).
$$

The true value

$$
\frac{2}{\pi}
\approx
0.6366
$$

lies inside the interval.

Notice that the confidence interval is very wide because

$$
n=4
$$

is extremely small. In practice, Monte Carlo simulations often use thousands or millions of samples, causing the standard error to shrink at the rate

$$
\frac{1}{\sqrt n}.
$$
## Quick Reference Table

| Distribution | Use When... | Support | PMF / PDF |
|------------|------------|----------|----------|
| Bernoulli | Single success/failure experiment | $x \in \{0,1\}$ | $P(X=x)=p^x(1-p)^{1-x}$ |
| Binomial | Count successes in $n$ independent Bernoulli trials | $x=0,1,\dots,n$ | $P(X=k)=\binom{n}{k}p^k(1-p)^{n-k}$ |
| Geometric | Number of trials until first success | $x=1,2,\dots$ | $P(X=k)=(1-p)^{k-1}p$ |
| Negative Binomial | Number of trials until the $r$-th success | $x=r,r+1,\dots$ | $P(X=k)=\binom{k-1}{r-1}p^r(1-p)^{k-r}$ |
| Hypergeometric | Sampling without replacement | discrete | $P(X=k)=\frac{\binom{K}{k}\binom{N-K}{n-k}}{\binom{N}{n}}$ |
| Poisson | Count events occurring in time/space | $x=0,1,2,\dots$ | $P(X=k)=\frac{\lambda^k e^{-\lambda}}{k!}$ |
| Uniform (Discrete) | All outcomes equally likely | finite set | $P(X=x)=\frac{1}{n}$ |
| Uniform (Continuous) | Any value in an interval equally likely | $a \le x \le b$ | $f(x)=\frac{1}{b-a}$ |
| Exponential | Waiting time until next event | $x \ge 0$ | $f(x)=\lambda e^{-\lambda x}$ |
| Normal | Natural variation around a mean | $-\infty < x < \infty$ | $f(x)=\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ |

---

# Bernoulli Distribution

## What It Is

A Bernoulli random variable models a single experiment with only two possible outcomes:

- Success
- Failure

Common examples:

- Coin flip
- Pass/fail exam
- Defective/non-defective item

---

## Parameters

$$
p = P(\text{success})
$$

---

## PMF

$$
P(X=x)
=
p^x(1-p)^{1-x}
$$

for

$$
x \in \{0,1\}
$$

---

## Mean

$$
E[X]
=
p
$$

---

## Variance

$$
\operatorname{Var}(X)
=
p(1-p)
$$

---

## Example

A biased coin lands heads with probability

$$
p=0.7
$$

Find the probability of getting heads.

### Solution

Let

$$
X=1
$$

represent heads.

Then

$$
P(X=1)
=
0.7
$$

---

# Binomial Distribution

## What It Is

The Binomial distribution counts the number of successes in a fixed number of independent Bernoulli trials.

Common examples:

- Number of heads in 20 flips
- Number of customers who purchase a product
- Number of students passing an exam

---

## Parameters

$$
n = \text{number of trials}
$$

$$
p = \text{success probability}
$$

---

## PMF

$$
P(X=k)
=
\binom{n}{k}
p^k
(1-p)^{n-k}
$$

---

## Mean

$$
E[X]
=
np
$$

---

## Variance

$$
\operatorname{Var}(X)
=
np(1-p)
$$

---

## Example

A fair coin is flipped 10 times.

Find the probability of exactly 3 heads.

### Solution

$$
P(X=3)
=
\binom{10}{3}
(0.5)^3
(0.5)^7
$$

$$
=
\frac{\binom{10}{3}}{2^{10}}
$$

$$
=
\frac{120}{1024}
$$

$$
\approx
0.1172
$$

---

# Geometric Distribution

## What It Is

Models the number of trials required to obtain the first success.

Common examples:

- Number of coin flips until first head
- Number of sales calls until first sale

---

## PMF

$$
P(X=k)
=
(1-p)^{k-1}p
$$

---

## Mean

$$
E[X]
=
\frac{1}{p}
$$

---

## Variance

$$
\operatorname{Var}(X)
=
\frac{1-p}{p^2}
$$

---

## Example

A coin lands heads with probability

$$
0.2
$$

Find the probability that the first head occurs on flip 4.

### Solution

$$
P(X=4)
=
(0.8)^3(0.2)
$$

$$
=
0.1024
$$

---

# Negative Binomial Distribution

## What It Is

Models the number of trials required to achieve the $r$-th success.

Common examples:

- Number of shots until 5 baskets
- Number of calls until 3 sales

---

## PMF

$$
P(X=k)
=
\binom{k-1}{r-1}
p^r
(1-p)^{k-r}
$$

---

## Mean

$$
E[X]
=
\frac{r}{p}
$$

---

## Example

A basketball player makes shots with probability

$$
0.8
$$

Find the probability that the third made shot occurs on attempt 5.

### Solution

$$
P(X=5)
=
\binom{4}{2}
(0.8)^3
(0.2)^2
$$

$$
=
6(0.512)(0.04)
$$

$$
=
0.12288
$$

---

# Hypergeometric Distribution

## What It Is

Used when sampling without replacement.

Common examples:

- Drawing cards from a deck
- Selecting defective items from a batch

---

## Parameters

$$
N = \text{population size}
$$

$$
K = \text{successes in population}
$$

$$
n = \text{sample size}
$$

---

## PMF

$$
P(X=k)
=
\frac{
\binom{K}{k}
\binom{N-K}{n-k}
}{
\binom{N}{n}
}
$$

---

## Example

A box contains

- 7 red balls
- 3 blue balls

Two balls are drawn without replacement.

Find the probability of drawing exactly one blue ball.

### Solution

$$
P(X=1)
=
\frac{
\binom{3}{1}
\binom{7}{1}
}{
\binom{10}{2}
}
$$

$$
=
\frac{21}{45}
$$

$$
=
0.4667
$$

---

# Poisson Distribution

## What It Is

Models counts of events occurring randomly over time, space, or volume.

Common examples:

- Customers arriving per hour
- Phone calls received per minute
- Defects per meter of wire

---

## Parameter

$$
\lambda
=
\text{average event rate}
$$

---

## PMF

$$
P(X=k)
=
\frac{\lambda^k e^{-\lambda}}
{k!}
$$

---

## Mean

$$
E[X]
=
\lambda
$$

---

## Variance

$$
\operatorname{Var}(X)
=
\lambda
$$

---

## Example

A store receives an average of

$$
4
$$

customers per hour.

Find the probability of exactly 2 customers arriving in an hour.

### Solution

$$
P(X=2)
=
\frac{4^2e^{-4}}
{2!}
$$

$$
=
8e^{-4}
$$

$$
\approx
0.1465
$$

---

# Uniform Distribution (Discrete)

## What It Is

Every outcome has equal probability.

Common examples:

- Rolling a fair die
- Drawing a random card index
- Selecting a random student

---

## PMF

$$
P(X=x)
=
\frac{1}{n}
$$

---

## Example

Roll a fair six-sided die.

Find

$$
P(X=5)
$$

### Solution

$$
P(X=5)
=
\frac{1}{6}
$$

---

# Uniform Distribution (Continuous)

## What It Is

Every value in an interval is equally likely.

Common examples:

- Random number generators
- Random positions along a line segment

---

## PDF

$$
f(x)
=
\frac{1}{b-a}
$$

for

$$
a \le x \le b
$$

---

## Mean

$$
E[X]
=
\frac{a+b}{2}
$$

---

## Variance

$$
\operatorname{Var}(X)
=
\frac{(b-a)^2}{12}
$$

---

## Example

Suppose

$$
X \sim \operatorname{Unif}(0,10)
$$

Find

$$
P(X < 3)
$$

### Solution

Area under the density:

$$
P(X<3)
=
\frac{3-0}{10-0}
$$

$$
=
0.3
$$

---

# Exponential Distribution

## What It Is

Models waiting time until the next event in a Poisson process.

Common examples:

- Time until next customer arrives
- Time until equipment failure
- Time between earthquakes

---

## PDF

$$
f(x)
=
\lambda e^{-\lambda x}
$$

for

$$
x \ge 0
$$

---

## Mean

$$
E[X]
=
\frac{1}{\lambda}
$$

---

## Variance

$$
\operatorname{Var}(X)
=
\frac{1}{\lambda^2}
$$

---

## Memoryless Property

$$
P(X>s+t \mid X>s)
=
P(X>t)
$$

---

## Example

Suppose

$$
\lambda=2
$$

Find

$$
P(X>1)
$$

### Solution

For an exponential random variable,

$$
P(X>t)
=
e^{-\lambda t}
$$

Therefore

$$
P(X>1)
=
e^{-2}
$$

$$
\approx
0.1353
$$

---

# Normal Distribution

## What It Is

The most important continuous distribution in statistics.

It models natural variation around an average value.

Common examples:

- Heights
- Test scores
- Measurement errors
- Asset returns (approximately)

---

## PDF

$$
f(x)
=
\frac{1}
{\sqrt{2\pi\sigma^2}}
e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$

---

## Parameters

$$
\mu
=
\text{mean}
$$

$$
\sigma^2
=
\text{variance}
$$

---

## Standardization

Convert any normal random variable to a standard normal:

$$
Z
=
\frac{X-\mu}
{\sigma}
$$

---

## Empirical Rule

Approximately:

$$
68\%
$$

within

$$
1\sigma
$$

$$
95\%
$$

within

$$
2\sigma
$$

$$
99.7\%
$$

within

$$
3\sigma
$$

---

## Example

Suppose

$$
X \sim N(100,10^2)
$$

Find

$$
P(X<120)
$$

### Solution

Standardize:

$$
Z
=
\frac{120-100}
{10}
=
2
$$

Therefore

$$
P(X<120)
=
P(Z<2)
$$

Using a standard normal table,

$$
P(Z<2)
\approx
0.9772
$$

---

# Choosing the Right Distribution

Ask yourself:

### Am I counting successes?

- One trial → Bernoulli
- Fixed number of trials → Binomial
- Until first success → Geometric
- Until $r$ successes → Negative Binomial

### Am I sampling without replacement?

- Hypergeometric

### Am I counting random events in time or space?

- Poisson

### Am I measuring waiting time between events?

- Exponential

### Are all outcomes equally likely?

- Uniform

### Is the quantity continuous and bell-shaped?

- Normal
# PMFs vs PDFs: What Do They Actually Represent?

## Quick Reference

| Property | PMF (Discrete) | PDF (Continuous) |
|-----------|----------------|------------------|
| Stands for | Probability Mass Function | Probability Density Function |
| Used for | Discrete random variables | Continuous random variables |
| Gives | Probability of a specific value | Probability **density** (not probability) |
| Probability at a single value | Can be positive | Always 0 |
| Probabilities are computed by | Summation | Integration |

---

# The Most Important Idea

Many students first learning continuous random variables make the following mistake:

> "If the PDF equals 1 at some point, doesn't that mean there's a 100% chance of getting that value?"

**No.**

The key idea is:

> **A PDF is *not* a probability.**

Instead:

> **A PDF tells you how densely probability is packed around a point.**

Actual probabilities are obtained by computing **areas under the PDF**, not by looking at the height of the curve.

---

# Discrete Random Variables (PMFs)

Suppose we roll a fair six-sided die.

| \(x\) | \(P(X=x)\) |
|-------|------------|
|1|1/6|
|2|1/6|
|3|1/6|
|4|1/6|
|5|1/6|
|6|1/6|

Here,

$$
P(X=3)=\frac16.
$$

This means there is exactly a

$$
\frac16
$$

chance of rolling a 3.

For discrete random variables:

> The PMF directly gives the probability of each possible outcome.

---

# Continuous Random Variables (PDFs)

Now suppose

$$
X\sim\text{Uniform}(0,1).
$$

Its PDF is

$$
f_X(x)=
\begin{cases}
1,&0\le x\le1\\
0,&\text{otherwise}.
\end{cases}
$$

Many people incorrectly interpret this as

> "Every value has probability 1."

That is **not** what the PDF means.

In reality,

$$
P(X=0.23)=0
$$

and

$$
P(X=0.44)=0.
$$

In fact,

$$
P(X=a)=0
$$

for **every** real number \(a\).

---

# Then What Does a PDF Actually Mean?

A PDF measures **probability density**.

Think of it as

> **Probability per unit length.**

It is analogous to physical density.

---

# Analogy: Density of a Metal Rod

Suppose a metal rod has density

$$
5\text{ kg/m}.
$$

This does **not** mean every meter weighs 5 kg.

Instead,

A piece of length

$$
L
$$

has mass

$$
5L.
$$

For example,

Length

$$
0.2\text{ m}
$$

has mass

$$
5(0.2)=1\text{ kg}.
$$

The density itself is **not** a mass.

You must multiply by a length.

---

# PDFs Work Exactly the Same Way

The PDF

$$
f(x)=1
$$

means

> There is **1 unit of probability per unit length.**

To obtain an actual probability, multiply by an interval (via integration).

For example,

$$
P(0.2\le X\le0.5)
=
\int_{0.2}^{0.5}1\,dx
=
0.3.
$$

Notice that

Length of interval:

$$
0.5-0.2=0.3.
$$

Probability:

$$
0.3.
$$

---

# Why Is the Probability of a Single Point Zero?

Consider

$$
P(X=0.23).
$$

This is

$$
\int_{0.23}^{0.23}1\,dx.
$$

The interval has width

$$
0.
$$

Therefore,

$$
P(X=0.23)=0.
$$

A single point has **zero width**, so it contributes **zero area** under the PDF.

---

# Visualizing a Uniform PDF

Imagine the PDF as the height of a rectangle.

```text
 ^
 |      ┌──────────────────────┐
1|      │                      │
 |      │                      │
 |______│______________________│____> x
        0                      1
```

The **height** is 1.

The probability is the **area**.

For example,

between

$$
0.4
$$

and

$$
0.6,
$$

```text
 ^
 |      ┌──────────────────────┐
1|      │    ██████████        │
 |      │    ██████████        │
 |______│____██████████________│____
        0    .4      .6        1
```

Area

$$
=
1\times0.2
=
0.2.
$$

Therefore,

$$
P(0.4\le X\le0.6)=0.2.
$$

---

# Why Must the Height Be 1?

Every valid probability distribution must satisfy

$$
\int_{-\infty}^{\infty}f(x)\,dx=1.
$$

For a Uniform(0,1),

the rectangle has

Height:

$$
1
$$

Width:

$$
1
$$

Area:

$$
1\times1=1.
$$

If the height were 2,

the total area would be

$$
2,
$$

which cannot represent a probability distribution.

---

# PMFs vs PDFs: Intuition

## PMF (Discrete)

Think:

> "What is the probability of landing **exactly on this value**?"

Example:

$$
P(X=3)=0.17.
$$

---

## PDF (Continuous)

Think:

> "How densely packed is probability around this value?"

To obtain probability,

take an interval and compute the area:

$$
P(a\le X\le b)
=
\int_a^bf(x)\,dx.
$$

---

# Common Misconceptions

### ❌ "The PDF at a point is the probability."

False.

Only the **area under the PDF** represents probability.

---

### ❌ "If the PDF equals 5, the probability is greater than 1."

False.

PDF values are **not probabilities**.

A PDF may even be larger than 1, provided its total area equals 1.

Example:

A Uniform distribution on

$$
[0,0.2]
$$

has

$$
f(x)=5.
$$

Since

$$
5\times0.2=1,
$$

it is a perfectly valid PDF.

---

# Mental Model to Remember

## PMF

> **Probability at a point**

Read

$$
P(X=x)
$$

as

> "What is the chance of getting exactly \(x\)?"

---

## PDF

> **Probability density around a point**

Read

$$
f(x)
$$

as

> "How densely packed is probability near \(x\)?"

To obtain an actual probability,

compute an area:

$$
P(a\le X\le b)
=
\int_a^bf(x)\,dx.
$$

---

# Summary

- PMFs assign **probabilities** to individual values.
- PDFs assign **probability density**, not probability.
- For continuous random variables,

  $$
  P(X=x)=0
  $$

  for every single value.

- Probabilities are always computed by **integrating the PDF over an interval**.

> **Remember:**
>
> - **PMF → probability**
> - **PDF → probability density**
> - **Area under a PDF = probability**
Prereqs:
- [[Probability Axioms]]
- [[Counting Principles]]
- [[Common Permutation Combination Identities]]

Related:
[[Hypergeometric Distribution]]

# Binomial Probability Distribution

The **Binomial Distribution** models probabilities when performing repeated independent trials with only two possible outcomes.

Unlike the Hypergeometric Distribution, sampling is effectively done **with replacement** (or equivalently with independent trials).

---

# Setup

Suppose each trial has:

- probability $p$ of success
- probability $1-p$ of failure

We perform:

$$
n
$$

independent trials.

Let:

$$
X = \text{number of successes}
$$

---

# Goal

Find the probability of getting exactly

$$
k
$$

successes in $n$ trials.

---

# Binomial Probability Formula

$$
P(X=k)
=
{n \choose k}
p^k
(1-p)^{n-k}
$$

---

# Intuition Behind the Formula

The formula comes from two ideas:

1. Count how many different ways the successes can occur
2. Compute the probability of any one specific arrangement

---

# Step 1: Probability of One Specific Arrangement

Suppose:

- success probability = $p$
- failure probability = $1-p$

Consider one particular sequence:

$$
SSFSF
$$

with:

- 3 successes
- 2 failures

The probability is:

$$
p \cdot p \cdot (1-p) \cdot p \cdot (1-p)
$$

which simplifies to:

$$
p^3(1-p)^2
$$

In general, any sequence with:

- $k$ successes
- $n-k$ failures

has probability:

$$
p^k(1-p)^{n-k}
$$

---

# Step 2: Count How Many Such Sequences Exist

Now count how many different arrangements contain exactly:

- $k$ successes
- $n-k$ failures

This is:

$$
{n \choose k}
$$

because we are choosing which $k$ positions are successes.

## Intuition for the ${n \choose k}$ Term

Suppose we perform:

$$
n
$$

trials and want exactly:

$$
k
$$

successes.

Think of the trials as indexed positions:

$$
\{1,2,3,\dots,n\}
$$

A sequence is completely determined once we choose which indices contain successes.

For example, if:

$$
n=5, \quad k=2
$$

and we choose success indices:

$$
\{2,5\}
$$

then the sequence must be:

$$
F \quad S \quad F \quad F \quad S
$$

So instead of thinking about arranging successes and failures directly, we can think of the problem as:

> Choosing $k$ positions from $n$ total positions to place the successes.

The number of ways to choose those success positions is:

$$
{n \choose k}
$$

which counts the number of subsets of size $k$ from the set of trial indices.

---

# Combine Both Parts

Therefore:

$$
P(X=k)
=
(\text{\# arrangements})
\times
(\text{probability of one arrangement})
$$

giving:

$$
P(X=k)
=
{n \choose k}
p^k
(1-p)^{n-k}
$$

---

# Example

Suppose:

- a coin is fair
- probability of heads:

$$
p=\frac12
$$

Flip the coin:

$$
n=5
$$

times.

Find the probability of getting exactly:

$$
k=3
$$

heads.

---

Using the formula:

$$
P(X=3)
=
{5 \choose 3}
\left(\frac12\right)^3
\left(\frac12\right)^2
$$

Simplify:

$$
=
10 \cdot \left(\frac12\right)^5
=
\frac{10}{32}
=
\frac{5}{16}
$$

So:

$$
P(X=3)=0.3125
$$

---
# Example of a Binomial Distribution Using Type 1 and Type 2 Objects

Suppose each draw can result in:

- a Type 1 object with probability

$$
\frac{a}{a+b}
$$

- a Type 2 object with probability

$$
\frac{b}{a+b}
$$

Assume draws are made **with replacement**, so these probabilities stay constant on every draw.

We perform:

$$
n
$$

independent draws.

---

# Goal

Find the probability of obtaining exactly

$$
k
$$

Type 1 objects.

---

# Step 1: Probability of One Specific Arrangement

Suppose one particular sequence is:

$$
1122211
$$

where:

- 1 = Type 1
- 2 = Type 2

If this sequence contains:

- $k$ Type 1 objects
- $n-k$ Type 2 objects

then its probability is:

$$
\left(\frac{a}{a+b}\right)^k
\left(\frac{b}{a+b}\right)^{n-k}
$$

because probabilities multiply across independent draws.

---

# Step 2: Count the Number of Such Arrangements

Now count how many sequences contain exactly:

- $k$ Type 1 objects
- $n-k$ Type 2 objects

This is:

$$
{n \choose k}
$$

because we choose which $k$ positions are occupied by Type 1 objects.

---

# Combine Both Parts

Therefore:

$$
P(\text{exactly } k \text{ Type 1 objects})
=
{n \choose k}
\left(\frac{a}{a+b}\right)^k
\left(\frac{b}{a+b}\right)^{n-k}
$$

---

# Concrete Example

Suppose:

- 7 objects are Type 1
- 3 objects are Type 2

So:

$$
a=7, \quad b=3
$$

Then:

$$
P(\text{Type 1})=\frac{7}{10}
$$

and

$$
P(\text{Type 2})=\frac{3}{10}
$$

Suppose we draw:

$$
n=5
$$

times with replacement.

Find the probability of getting exactly:

$$
k=4
$$

Type 1 objects.

---

Using the Binomial Formula:

$$
P(X=4)
=
{5 \choose 4}
\left(\frac{7}{10}\right)^4
\left(\frac{3}{10}\right)^1
$$

Compute:

$$
=
5 \cdot (0.7)^4 \cdot (0.3)
$$

$$
=
5 \cdot 0.2401 \cdot 0.3
$$

$$
=
0.36015
$$

So the probability is approximately:

$$
36.0\%
$$

---

# Relationship to the [[Binomial Theorem]]

Recall:

$$
(x+y)^n
=
\sum_{k=0}^n
{n \choose k}
x^k y^{n-k}
$$

The Binomial Distribution uses the same coefficients:

$$
{n \choose k}
$$

because both involve counting arrangements of two types of outcomes.

---

# Difference Between Binomial and Hypergeometric

## Binomial Distribution


- independent trials
- effectively sampling with replacement
- probability stays constant

$$
p
$$

- with replacement
- probabilities stay constant
- independent trials

$$
P(\text{Type 1})=\frac{a}{a+b}
$$

always.

---

## Hypergeometric Distribution

- sampling without replacement
- probabilities change after each draw
- dependent trials
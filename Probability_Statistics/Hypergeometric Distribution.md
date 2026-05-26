Prereqs:
- [[Probability Axioms]]
- [[Counting Principles]]
- [[Common Permutation Combination Identities]]

Related:
[[Binomial Distribution]]
# Hypergeometric Probability Distribution (Pick w/o Replacement)

The **Hypergeometric Distribution** models probabilities when sampling **without replacement** from a finite population.

---

# Setup

Suppose a population contains:

- $a$ objects of Type 1
- $b$ objects of Type 2

So the total population size is

$$
a+b
$$

We sample

$$
n \le a+b
$$

objects **without replacement**.

---

# Goal

Find the probability of selecting exactly

$$
k
$$

Type 1 objects in the sample.

---

# Hypergeometric Probability Formula

$$
P(X=k)
=
\frac{
{a \choose k}
{b \choose n-k}
}{
{a+b \choose n}
}
$$

---

# Interpretation of the Formula

## Numerator

### Choose $k$ Type 1 objects

$$
{a \choose k}
$$

### Choose the remaining $n-k$ objects from Type 2

$$
{b \choose n-k}
$$

Multiplying gives the number of favorable selections:

$$
{a \choose k}{b \choose n-k}
$$

---

## Denominator

The total number of ways to choose any sample of size $n$ from all $a+b$ objects:

$$
{a+b \choose n}
$$

---

# Final Probability

Therefore,

$$
P(X=k)
=
\frac{
\text{\# favorable selections}
}{
\text{\# total selections}
}
$$

which becomes

$$
P(X=k)
=
\frac{
{a \choose k}
{b \choose n-k}
}{
{a+b \choose n}
}
$$

---

# Example

Suppose:

- 7 red balls
- 5 blue balls

Total:

$$
12
$$

We randomly select:

$$
n=4
$$

balls without replacement.

Find the probability of getting exactly:

$$
k=3
$$

red balls.

---

Using the formula:

$$
P(X=3)
=
\frac{
{7 \choose 3}
{5 \choose 1}
}{
{12 \choose 4}
}
$$

**Side Note**: You can validate that your formula was written correctly by ensuring the $a$ and $b$ in the numerator sum up to the $a + b$ in the denominator ($7+5 = 12$) and the $k$ and $n-k$ in the numerator sum up to the sample size $n$ in the denominator ($3+1=4$).

Compute:

$$
=
\frac{
35 \cdot 5
}{
495
}
=
\frac{175}{495}
\approx 0.354
$$

So the probability is approximately

$$
0.354
$$

or

$$
35.4\%
$$
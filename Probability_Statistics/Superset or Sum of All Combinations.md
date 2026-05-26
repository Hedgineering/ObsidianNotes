Prereqs:
- [[Common Permutation Combination Identities]]
# Intuition for Sum of All Combinations

Recall:

$$
\sum_{r=0}^n {n \choose r} = 2^n
$$

## Example with 3 Objects

Suppose we have the set

$$
\{A,B,C\}
$$

How many total subsets exist?

---

### Choose 0 Elements

$$
{3 \choose 0}=1
$$

Subset:

$$
\emptyset
$$

---

### Choose 1 Element

$$
{3 \choose 1}=3
$$

Subsets:

$$
\{A\}, \{B\}, \{C\}
$$

---

### Choose 2 Elements

$$
{3 \choose 2}=3
$$

Subsets:

$$
\{A,B\}, \{A,C\}, \{B,C\}
$$

---

### Choose 3 Elements

$$
{3 \choose 3}=1
$$

Subset:

$$
\{A,B,C\}
$$

---

Adding all possibilities:

$$
{3 \choose 0}
+
{3 \choose 1}
+
{3 \choose 2}
+
{3 \choose 3}
=
1+3+3+1
=
8
$$

But:

$$
2^3=8
$$

Why?

Because each element independently has 2 choices:

- included
- not included

So the total number of subsets is

$$
2 \times 2 \times 2 = 2^3
$$

## Proof

This can also be proven using [[Binomial Theorem]]

The **Binomial Theorem** gives a formula for expanding powers of a binomial:

$$
(x+y)^n
$$

The expansion is:

$$
(x+y)^n
=
\sum_{i=0}^{n}
{n \choose i}
x^i y^{n-i}
$$
---


So we begin with $2^n$ and the fact that $1 + 1 = 2$.
$$
2^n = (1 + 1)^n 
$$
Apply Binomial Theorem, and the fact that $1^x = 1$:
$$(1 + 1)^n = \sum_{i=0}^{n}
{n \choose i}
1^i \times 1^{n-i} = \sum_{i=0}^{n}
{n \choose i}
$$
Notice that we can substitute any variable for $i$, so
$$
2^n =  \sum_{r=0}^{n}
{n \choose r} 
$$
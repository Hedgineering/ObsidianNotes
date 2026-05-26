# Counting Principles

---

# Addition Rule

Suppose:

- Method $A$ can be performed in $n_A$ ways
- Method $B$ can be performed in $n_B$ ways

If you can use **only one** method, then the total number of possible outcomes is

$$
n_A + n_B
$$

## Example

At a cafe, you can choose:

- a muffin in 2 ways
- or a bagel in 3 ways

Then the total number of choices is

$$
2 + 3 = 5
$$

The Addition Rule is used when choices are connected by an **OR**.

---

# Multiplication Rule

Suppose a process consists of multiple operations performed one after another.

If:

- the first operation can be done in $n_1$ ways
- the second operation can be done in $n_2$ ways
- ...
- the $k$th operation can be done in $n_k$ ways

then the total number of ways to perform the entire process is

$$
n_1 n_2 \cdots n_k
$$

or equivalently

$$
\prod_{i=1}^k n_i
$$

The Multiplication Rule is used when choices are connected by an **AND**.

---

## Example

Suppose:

- you have 3 shirts
- and 4 pairs of pants

Then the number of outfits is

$$
3 \times 4 = 12
$$

because each shirt can be paired with each pair of pants.

---

# Permutations

A **permutation** is an arrangement where **order matters**.

## Number of Permutations of $n$ Distinct Objects

$$
n!
$$

where

$$
n! = n(n-1)(n-2)\cdots(2)(1)
$$

---

## Permutations of $r$ Objects Chosen from $n$

The number of ordered arrangements of $r$ objects selected from $n$ distinct objects is

$$
P(n,r)=\frac{n!}{(n-r)!}
$$

---

## Example

How many ways can 3 students be selected and arranged from 5 students?

$$
P(5,3)=\frac{5!}{(5-3)!}
=
\frac{5!}{2!}
=
60
$$

Might be easier to remember as if you have to fill 3 slots when order matters,

`_, _, _`

You have 5 options for the first position, 
You have 4 options for the second (since you already put one in the first slot)
You have 3 options for the third (since you already put someone in slots 1 and 2)

`5 choices, 4 choices, 3 choices`

So $5 * 4 * 3 = 60$ ways for them to be arranged.

---

# Combinations

A **combination** is a selection where **order does not matter**.

## Number of Combinations of $r$ Objects Chosen from $n$

$$
{n \choose r}
=
\frac{n!}{r!(n-r)!}
$$

This is also written as

$$
C(n,r)
$$

---

## Example

How many ways can 3 students be selected from 5 students if order does not matter?

$$
{5 \choose 3}
=
\frac{5!}{3!2!}
=
10
$$

Think of combinations as sets. The set $\{A, B, C\}$ is the exact same as $\{B, A, C\}$ .

We need to divide away the repetition. How many ways can you get the same combination of 3?

For the same combination of A, B, and C - the permutations include the following
You have 3 choices for the first slot (A, B or C)
You have 2 choices for the second slot (you already put one in the first slot)
You have 1 choice for the third slot

So by the same logic, we need to divide away the additional $r!$ permutations of the same combination being repeated, which is why there is that extra term in the denominator.

---

# Relationship Between Permutations and Combinations

Permutations count ordered arrangements.

Combinations count unordered selections.

They are related by:

$$
P(n,r)=r!{n \choose r}
$$

because each group of $r$ selected objects can be arranged in $r!$ different orders.
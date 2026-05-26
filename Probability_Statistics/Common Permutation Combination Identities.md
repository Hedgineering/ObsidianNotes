Prereqs / Related to:
[[Counting Principles]]
# Common Permutation and Combination Identities

---

# Combinations

## Choosing Nothing

There is exactly one way to choose nothing:

$$
{n \choose 0} = 1
$$

---

## Choosing Everything

There is exactly one way to choose all objects:

$$
{n \choose n} = 1
$$

---

## Choosing One Object

Choosing 1 object from $n$ objects:

$$
{n \choose 1} = n
$$

---

## Choosing All But One

Choosing $n-1$ objects is equivalent to choosing which single object to leave out:

$$
{n \choose n-1} = n
$$

---

## Symmetry Identity

Choosing $r$ objects is equivalent to choosing which $n-r$ objects to exclude:

$$
{n \choose r} = {n \choose n-r}
$$

---

## Pascal's Identity

A fundamental recursive relationship:

$$
{n \choose r}
=
{n-1 \choose r}
+
{n-1 \choose r-1}
$$

This generates Pascal's Triangle.
For intuition see [[Pascal's Identity]]

---

## Sum of All Combinations

The total number of subsets of an $n$-element set is

$$
\sum_{r=0}^n {n \choose r} = 2^n
$$
For intuition see [[Superset or Sum of All Combinations]]

---

# Permutations

## Permuting All Objects

The number of arrangements of $n$ distinct objects:

$$
P(n,n)=n!
$$

---

## Permuting One Object

Choosing and arranging 1 object from $n$:

$$
P(n,1)=n
$$

---

## Permuting Zero Objects

There is exactly one way to arrange nothing:

$$
P(n,0)=1
$$

---

## General Permutation Formula

$$
P(n,r)=\frac{n!}{(n-r)!}
$$

---

# Factorial Identities

## Definition of Factorial

$$
n! = n(n-1)(n-2)\cdots(2)(1)
$$

---

## Recursive Definition

$$
n! = n(n-1)!
$$

---

## Zero Factorial

By definition:

$$
0! = 1
$$

This makes many formulas work cleanly.

Example:

$$
{n \choose n}
=
\frac{n!}{n!0!}
=
1
$$

---

# Relationship Between Permutations and Combinations

Permutations count ordered arrangements.

Combinations count unordered selections.

They are related by:

$$
P(n,r)=r!{n \choose r}
$$

and equivalently

$$
{n \choose r}
=
\frac{P(n,r)}{r!}
$$
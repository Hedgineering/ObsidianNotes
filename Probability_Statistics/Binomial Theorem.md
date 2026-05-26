Prereqs:
- [[Counting Principles]]
# Binomial Theorem

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

# Intuition

Each term in the expansion comes from:

- choosing some number of $x$'s
- and the remaining factors as $y$'s

when multiplying

$$
(x+y)(x+y)\cdots(x+y)
$$

($n$ total factors)

The coefficient

$$
{n \choose i}
$$

counts how many ways we can choose exactly $i$ copies of $x$.

---

# Example: Expanding $(x+y)^2$

Using the theorem:

$$
(x+y)^2
=
{2 \choose 0}x^0y^2
+
{2 \choose 1}x^1y^1
+
{2 \choose 2}x^2y^0
$$

Simplifying:

$$
= y^2 + 2xy + x^2
$$

Usually written as:

$$
x^2 + 2xy + y^2
$$

---

# Example: Expanding $(x+y)^3$

$$
(x+y)^3
=
{3 \choose 0}y^3
+
{3 \choose 1}xy^2
+
{3 \choose 2}x^2y
+
{3 \choose 3}x^3
$$

Substituting coefficients:

$$
=
y^3 + 3xy^2 + 3x^2y + x^3
$$

Usually written as:

$$
x^3 + 3x^2y + 3xy^2 + y^3
$$

---

# Pascal's Triangle

The coefficients in the Binomial Theorem come directly from **Pascal's Triangle**.

## Rows of Pascal's Triangle

$$
\begin{aligned}
n=0 &: \quad 1 \\
n=1 &: \quad 1 \quad 1 \\
n=2 &: \quad 1 \quad 2 \quad 1 \\
n=3 &: \quad 1 \quad 3 \quad 3 \quad 1 \\
n=4 &: \quad 1 \quad 4 \quad 6 \quad 4 \quad 1
\end{aligned}
$$

These numbers are exactly:

$$
{n \choose 0},
{n \choose 1},
\dots,
{n \choose n}
$$

---
$$
\begin{array}{cccccccccccccc}

&&&&&& {0 \choose 0} &&&&&& \\[8pt]

&&&&& {1 \choose 0} && {1 \choose 1} &&&&& \\[8pt]

&&&& {2 \choose 0} && {2 \choose 1} && {2 \choose 2} &&&& \\[8pt]

&&& {3 \choose 0} && {3 \choose 1} && {3 \choose 2} && {3 \choose 3} &&& \\[8pt]

&& {4 \choose 0} && {4 \choose 1} && {4 \choose 2} && {4 \choose 3} && {4 \choose 4} &&

\end{array}
$$

  
$$  
\begin{array}{cccccccccc}  
&&&&1&&&& \\[6pt]  
  
&&&1&&1&&& \\[6pt]  
  
&&1&&2&&1&& \\[6pt]  
  
&1&&3&&3&&1& \\[6pt]  
  
1&&4&&6&&4&&1
\end{array}  
$$  
  
Each row corresponds to the coefficients of:  
  
$$  
(x+y)^n  
$$  
  
For example:  
  
- Row $n=2$ gives:  
  
$$  
1 \quad 2 \quad 1  
$$  
  
so  
  
$$  
(x+y)^2  
=  
x^2 + 2xy + y^2  
$$
# Example Using Pascal's Triangle

The coefficients for

$$
(x+y)^4
$$

come from row $n=4$:

$$
1 \quad 4 \quad 6 \quad 4 \quad 1
$$

So:

$$
(x+y)^4
=
x^4
+
4x^3y
+
6x^2y^2
+
4xy^3
+
y^4
$$

---

# Important Connection

Pascal's Identity:

$$
{n \choose r}
=
{n-1 \choose r}
+
{n-1 \choose r-1}
$$

is exactly the rule used to generate Pascal's Triangle.

Each entry equals the sum of the two entries above it.
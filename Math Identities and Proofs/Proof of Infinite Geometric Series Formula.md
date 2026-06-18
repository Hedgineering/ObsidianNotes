We want to prove

$$
\sum_{k=0}^{\infty} r^k
=
\frac{1}{1-r}
$$

for

$$
|r|<1.
$$

---

# Step 1: Start With a Finite Sum

Define the finite partial sum

$$
S_n
=
\sum_{k=0}^{n} r^k
$$

So

$$
S_n
=
1+r+r^2+\cdots+r^n
$$

---

# Step 2: Multiply by r

Multiply both sides by

$$
r.
$$

Then

$$
rS_n
=
r+r^2+r^3+\cdots+r^{n+1}
$$

---

# Step 3: Subtract

Now subtract:

$$
S_n-rS_n
=
(1+r+r^2+\cdots+r^n)
-
(r+r^2+\cdots+r^n+r^{n+1})
$$

Everything cancels except the first and last terms:

$$
S_n-rS_n
=
1-r^{n+1}
$$

Factor the left side:

$$
S_n(1-r)
=
1-r^{n+1}
$$

Therefore

$$
S_n
=
\frac{1-r^{n+1}}{1-r}
$$

for

$$
r\ne1.
$$

---

# Step 4: Take the Infinite Limit

The infinite series is the limit of the partial sums:

$$
\sum_{k=0}^{\infty}r^k
=
\lim_{n\to\infty}S_n
$$

Substitute the formula:

$$
\sum_{k=0}^{\infty}r^k
=
\lim_{n\to\infty}
\frac{1-r^{n+1}}{1-r}
$$

If

$$
|r|<1,
$$

then the numerator goes to zero because exponentiating a number between 0 and 1 makes it increasingly smaller, thus

$$
\lim_{n\to\infty}r^{n+1}=0.
$$

So

$$
\sum_{k=0}^{\infty}r^k
=
\frac{1-0}{1-r}
$$

Therefore

$$
\boxed{
\sum_{k=0}^{\infty} r^k
=
\frac{1}{1-r}
}
$$

---

# Why We Need |r| < 1

The condition

$$
|r|<1
$$

means the powers of

$$
r
$$

shrink toward zero.

Examples:

If

$$
r=\frac12,
$$

then

$$
1,\frac12,\frac14,\frac18,\ldots
$$

shrinks to zero.

If

$$
r=-\frac12,
$$

then

$$
1,-\frac12,\frac14,-\frac18,\ldots
$$

also shrinks to zero.

But if

$$
|r|\ge1,
$$

then

$$
r^n
$$

does not shrink to zero.

Examples:

If

$$
r=1,
$$

then

$$
1+1+1+\cdots
$$

diverges.

If

$$
r=-1,
$$

then

$$
1-1+1-1+\cdots
$$

does not settle to one value.

If

$$
r=2,
$$

then

$$
1+2+4+8+\cdots
$$

diverges.

So the infinite geometric series formula only works when

$$
|r|<1.
$$

---

# Intuition

The finite sum formula is

$$
S_n
=
\frac{1-r^{n+1}}{1-r}.
$$

The only difference between the finite and infinite case is the leftover tail term:

$$
r^{n+1}.
$$

If

$$
|r|<1,
$$

then this tail becomes smaller and smaller until it disappears in the limit.

That leaves

$$
\frac{1}{1-r}.
$$

---

# Example

Let

$$
r=\frac12.
$$

Then

$$
\sum_{k=0}^{\infty}
\left(\frac12\right)^k
=
1+\frac12+\frac14+\frac18+\cdots
$$

Using the formula:

$$
=
\frac{1}{1-\frac12}
=
\frac{1}{\frac12}
=
2.
$$

So

$$
1+\frac12+\frac14+\frac18+\cdots
=
2.
$$

---

# Key Takeaway

To prove

$$
\sum_{k=0}^{\infty} r^k
=
\frac{1}{1-r},
$$

first prove the finite version:

$$
\sum_{k=0}^{n}r^k
=
\frac{1-r^{n+1}}{1-r}.
$$

Then take the limit as

$$
n\to\infty.
$$

When

$$
|r|<1,
$$

the leftover term

$$
r^{n+1}
$$

goes to zero, giving

$$
\boxed{
\sum_{k=0}^{\infty} r^k
=
\frac1{1-r}
}
$$
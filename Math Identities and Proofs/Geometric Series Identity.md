# Deriving the Identity

We want to derive

$$
\sum_{k=1}^{\infty}
k^2r^{k-1}
=
\frac{1+r}{(1-r)^3},
\qquad |r|<1
$$

Rather than memorizing it, we'll build it from the geometric series.

---

# Step 1: Start with the Geometric Series

For

$$
|r|<1,
$$

we know from [[Proof of Infinite Geometric Series Formula]]

$$
\sum_{k=0}^{\infty}
r^k
=
\frac1{1-r}
$$

This is the foundation for nearly every power-series identity you'll encounter.

---

# Step 2: Differentiate Both Sides

Differentiate with respect to

$$
r.
$$

Left side

$$
\frac{d}{dr}
\left(
\sum_{k=0}^{\infty}r^k
\right)
=
\sum_{k=1}^{\infty}
kr^{k-1}
$$

(The first term disappears because the derivative of a constant is zero.)

Right side

$$
\frac{d}{dr}
\left(
\frac1{1-r}
\right)
=
\frac1{(1-r)^2}
$$

Therefore

$$
\boxed{
\sum_{k=1}^{\infty}
kr^{k-1}
=
\frac1{(1-r)^2}
}
$$

This is the identity used when deriving the expectation of the geometric distribution.

---

# Step 3: Differentiate Again

Now differentiate

$$
\sum_{k=1}^{\infty}
kr^{k-1}
=
\frac1{(1-r)^2}
$$

Differentiate term-by-term.

Left side

$$
\sum_{k=2}^{\infty}
k(k-1)r^{k-2}
$$

Right side

$$
\frac{d}{dr}
\left(
\frac1{(1-r)^2}
\right)
=
\frac{2}{(1-r)^3}
$$

Therefore

$$
\boxed{
\sum_{k=2}^{\infty}
k(k-1)r^{k-2}
=
\frac2{(1-r)^3}
}
$$

Notice this is **not** quite what we want.

We need

$$
k^2r^{k-1}.
$$

---

# Step 4: Rewrite k²

Observe

$$
k^2
=
k(k-1)+k
$$

This is the key algebra trick.

Therefore

$$
\sum
k^2r^{k-1}
=
\sum
k(k-1)r^{k-1}
+
\sum
kr^{k-1}
$$

---

# Step 5: Convert the First Sum

We already know

$$
\sum
k(k-1)r^{k-2}
=
\frac2{(1-r)^3}
$$

Multiply both sides by

$$
r.
$$

Then

$$
\sum
k(k-1)r^{k-1}
=
\frac{2r}{(1-r)^3}
$$

---

# Step 6: Add the Two Pieces

Now substitute.

$$
\sum
k^2r^{k-1}
=
\frac{2r}{(1-r)^3}
+
\frac1{(1-r)^2}
$$

Write over a common denominator.

$$
=
\frac{2r+(1-r)}
{(1-r)^3}
$$

Simplify.

$$
2r+1-r
=
1+r
$$

Therefore

$$
\boxed{
\sum_{k=1}^{\infty}
k^2r^{k-1}
=
\frac{1+r}{(1-r)^3}
}
$$

Exactly what we wanted.

---

# Why This Works

Differentiating a power series "pulls down" the exponent.

For example

$$
r^5
\rightarrow
5r^4
$$

Differentiate again

$$
5r^4
\rightarrow
20r^3
=
5(5-1)r^3
$$

Every differentiation introduces another factor involving

$$
k.
$$

This is why differentiating power series is such a common technique for computing expectations and moments.

---

# Pattern to Remember

Starting from

$$
\sum r^k
=
\frac1{1-r}
$$

Differentiate once

$$
\sum kr^{k-1}
=
\frac1{(1-r)^2}
$$

Differentiate twice

$$
\sum k(k-1)r^{k-2}
=
\frac2{(1-r)^3}
$$

Then use

$$
k^2
=
k(k-1)+k
$$

to obtain

$$
\sum
k^2r^{k-1}
=
\frac{1+r}{(1-r)^3}.
$$

---

# General Insight

This technique extends much further.

By repeatedly differentiating the geometric series, you can derive identities for

- $$\sum k^3r^k$$
- $$\sum k^4r^k$$
- Higher moments
- Moment generating functions (MGFs)
- Probability generating functions (PGFs)
- Z-transforms
- Taylor series expansions

This is why the geometric series is often called the "mother of all power series."
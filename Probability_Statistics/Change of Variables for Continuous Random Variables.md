## Quick Reference

Suppose

$$
Y=g(X),
$$

where $g$ is **one-to-one** (strictly increasing or decreasing).

To find the PDF of $Y$:

1. Find the support of $Y$.
2. Solve for $X$ in terms of $Y$.
3. Differentiate to find

$$
\frac{dx}{dy}.
$$

4. Compute

$$
\boxed{
f_Y(y)
=
f_X(x(y))
\left|
\frac{dx}{dy}
\right|
}
$$

The absolute value ensures the PDF is always nonnegative.

---

# Intuition

Think of a PDF as **probability density per unit length**.

If the transformation stretches the x-axis, the density must decrease.

If it compresses the x-axis, the density must increase.

Example:

Suppose a tiny interval of width

$$
dx
$$

gets stretched into

$$
dy.
$$

Since probability is preserved,

$$
P(X\in dx)
=
P(Y\in dy).
$$

Therefore,

$$
f_X(x)\,dx
=
f_Y(y)\,dy.
$$

Dividing by $dy$ gives

$$
f_Y(y)
=
f_X(x)
\frac{dx}{dy}.
$$

Since derivatives may be negative,

$$
\boxed{
f_Y(y)
=
f_X(x)
\left|
\frac{dx}{dy}
\right|
}
$$

---

# General Procedure

Given

$$
Y=g(X),
$$

Step 1.

Find the support of $Y$.

Substitute the endpoints of the support of $X$ into the transformation.

---

Step 2.

Solve for $X$.

Write

$$
X=g^{-1}(Y).
$$

---

Step 3.

Differentiate.

Compute

$$
\frac{dx}{dy}.
$$

---

Step 4.

Plug into

$$
f_Y(y)
=
f_X(g^{-1}(y))
\left|
\frac{dx}{dy}
\right|.
$$

---

Step 5.

Verify.

Check that

$$
\int f_Y(y)\,dy=1.
$$

---

# Common Transformations

## Linear Transformation

Suppose

$$
Y=aX+b.
$$

Then

$$
X=\frac{Y-b}{a}
$$

and

$$
\frac{dx}{dy}
=
\frac1a.
$$

Taking the absolute value,

$$
\boxed{
f_Y(y)
=
f_X\!\left(\frac{y-b}{a}\right)
\frac1{|a|}
}
$$

---

## Scaling Only

If

$$
Y=aX,
$$

then

$$
\boxed{
f_Y(y)
=
f_X\!\left(\frac ya\right)
\frac1{|a|}
}
$$

---

## Shift Only

If

$$
Y=X+b,
$$

then

$$
\boxed{
f_Y(y)
=
f_X(y-b)
}
$$

Notice that shifting does **not** change the shape of the PDF.

---

# Example 1

Suppose

$$
X\sim\text{Uniform}(0,2)
$$

and

$$
Y=3X+1.
$$

### Step 1: Find the support

When

$$
X=0,
$$

$$
Y=1.
$$

When

$$
X=2,
$$

$$
Y=7.
$$

Therefore,

$$
Y\in[1,7].
$$

---

### Step 2: Solve for X

$$
X=\frac{Y-1}{3}.
$$

---

### Step 3: Differentiate

$$
\frac{dx}{dy}
=
\frac13.
$$

---

### Step 4: Substitute

Since

$$
f_X(x)=\frac12,
$$

we obtain

$$
f_Y(y)
=
\frac12
\cdot
\frac13
=
\frac16.
$$

Therefore

$$
\boxed{
f_Y(y)=
\begin{cases}
\frac16,&1\le y\le7,\\
0,&\text{otherwise.}
\end{cases}
}
$$

---

# Example 2

Suppose

$$
X\sim\text{Uniform}(0,1)
$$

and

$$
Y=2X.
$$

Support:

$$
0\le Y\le2.
$$

Inverse:

$$
X=\frac Y2.
$$

Derivative:

$$
\frac{dx}{dy}
=
\frac12.
$$

Since

$$
f_X(x)=1,
$$

the new PDF is

$$
f_Y(y)
=
1\cdot\frac12
=
\frac12.
$$

Thus

$$
\boxed{
f_Y(y)=
\begin{cases}
\frac12,&0\le y\le2,\\
0,&\text{otherwise.}
\end{cases}
}
$$

---

# Example 3 (Decreasing Transformation)

Suppose

$$
X\sim\text{Uniform}(0,1)
$$

and

$$
Y=1-X.
$$

Support:

$$
0\le Y\le1.
$$

Inverse:

$$
X=1-Y.
$$

Derivative:

$$
\frac{dx}{dy}
=
-1.
$$

Absolute value:

$$
\left|
\frac{dx}{dy}
\right|
=
1.
$$

Therefore

$$
f_Y(y)
=
1\cdot1
=
1.
$$

The PDF is unchanged.

This illustrates why the absolute value is necessary.

---

# Worked Solution: Exponential Transformation

Suppose

$$
X\sim\text{Uniform}(0,1)
$$

and

$$
Y=X^2.
$$

### Step 1

Support:

$$
0\le Y\le1.
$$

---

### Step 2

Inverse:

$$
X=\sqrt{Y}.
$$

---

### Step 3

Differentiate:

$$
\frac{dx}{dy}
=
\frac1{2\sqrt{Y}}.
$$

---

### Step 4

Substitute:

Since

$$
f_X(x)=1,
$$

we obtain

$$
\boxed{
f_Y(y)
=
\frac1{2\sqrt{y}},
\qquad
0<y<1.
}
$$

Notice the density is much larger near zero because many values of $X$ are compressed into a small interval of $Y$.

---

# Common Mistakes

❌ Forgetting to change the support.

❌ Forgetting to compute the inverse transformation.

❌ Forgetting the absolute value around

$$
\frac{dx}{dy}.
$$

❌ Substituting $x$ instead of $g^{-1}(y)$ into the original PDF.

❌ Forgetting to verify that the PDF integrates to 1.

---

# Practice Problems

## Problem 1

Suppose

$$
X\sim\text{Uniform}(2,5),
$$

and

$$
Y=4X-3.
$$

Find:

- Support of $Y$
- PDF of $Y$
- $E[Y]$

### Solution

Support:

$$
5\le Y\le17.
$$

Inverse:

$$
X=\frac{Y+3}{4}.
$$

Derivative:

$$
\frac{dx}{dy}
=
\frac14.
$$

Original PDF:

$$
f_X(x)=\frac13.
$$

Therefore

$$
\boxed{
f_Y(y)=
\frac1{12},
\qquad
5\le y\le17.
}
$$

Mean:

$$
E[Y]
=
4E[X]-3
=
4\left(\frac{2+5}{2}\right)-3
=
11.
$$

---

## Problem 2

Suppose

$$
X\sim\text{Uniform}(0,4),
$$

and

$$
Y=8-2X.
$$

Find the PDF of $Y$.

### Solution

Support:

$$
0\le Y\le8.
$$

Inverse:

$$
X=\frac{8-Y}{2}.
$$

Derivative:

$$
\frac{dx}{dy}
=
-\frac12.
$$

Absolute value:

$$
\frac12.
$$

Original PDF:

$$
f_X(x)=\frac14.
$$

Therefore

$$
\boxed{
f_Y(y)=
\frac18,
\qquad
0\le y\le8.
}
$$

---

## Problem 3

Suppose

$$
X\sim\text{Uniform}(0,3),
$$

and

$$
Y=X+5.
$$

Find the support and PDF of $Y$.

### Solution

Support:

$$
5\le Y\le8.
$$

Inverse:

$$
X=Y-5.
$$

Derivative:

$$
\frac{dx}{dy}=1.
$$

Therefore

$$
\boxed{
f_Y(y)
=
\frac13,
\qquad
5\le y\le8.
}
$$

Notice that adding a constant simply shifts the support—the shape of the PDF does not change.
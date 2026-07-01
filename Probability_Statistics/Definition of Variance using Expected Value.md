# Variance Identity

## Goal

Prove the fundamental identity

$$
\operatorname{Var}(X)
=
E[X^2]-\left(E[X]\right)^2
$$

This formula is often easier to compute than the definition because it only requires finding two expectations.

---

## Step 1: Start with the definition

Variance measures the average squared distance from the mean.

By definition,

$$
\operatorname{Var}(X)
=
E\left[(X-\mu)^2\right]
$$

where

$$
\mu = E[X].
$$

---

## Step 2: Expand the square

Use the algebra identity

$$
(a-b)^2=a^2-2ab+b^2.
$$

Substitute

$$
a=X,\qquad b=\mu.
$$

Then

$$
(X-\mu)^2
=
X^2-2\mu X+\mu^2.
$$

Therefore,

$$
\operatorname{Var}(X)
=
E\left[X^2-2\mu X+\mu^2\right].
$$

---

## Step 3: Use linearity of expectation

Expectation distributes across sums:

$$
E[A+B]
=
E[A]+E[B].
$$

Also, constants can be factored out:

$$
E[cX]
=
cE[X].
$$

Applying both,

$$
\operatorname{Var}(X)
=
E[X^2]
-
2\mu E[X]
+
E[\mu^2].
$$

---

## Step 4: Simplify each term

Recall

$$
\mu=E[X].
$$

So

### First term

Nothing changes:

$$
E[X^2].
$$

---

### Second term

Since

$$
E[X]=\mu,
$$

we get

$$
2\mu E[X]
=
2\mu^2.
$$

---

### Third term

Since

$$
\mu^2
$$

is just a constant,

$$
E[\mu^2]
=
\mu^2.
$$

---

## Step 5: Combine everything

Substitute these back:

$$
\operatorname{Var}(X)
=
E[X^2]
-
2\mu^2
+
\mu^2.
$$

Combine like terms:

$$
\operatorname{Var}(X)
=
E[X^2]-\mu^2.
$$

Finally, replace

$$
\mu=E[X]
$$

to obtain

$$
\boxed{
\operatorname{Var}(X)
=
E[X^2]
-
\left(E[X]\right)^2
}
$$

---

# Why This Works (Intuition)

Variance is defined as

> "the average squared distance from the mean."

The expression

$$
(X-\mu)^2
$$

contains three pieces:

$$
X^2
-
2\mu X
+
\mu^2.
$$

Taking expectations gives

$$
E[X^2]
-
2\mu E[X]
+
\mu^2.
$$

Since

$$
E[X]=\mu,
$$

the middle term becomes

$$
2\mu^2,
$$

which cancels one copy of the final

$$
+\mu^2,
$$

leaving

$$
E[X^2]-\mu^2.
$$

The "cross term" disappears because the deviations above and below the mean balance perfectly in expectation.

---

# Memory Trick

Always remember the order:

$$
\boxed{
\operatorname{Var}(X)
=
E[X^2]
-
(E[X])^2
}
$$

Notice carefully:

- Square **inside** expectation:

$$
E[X^2]
$$

- Square **outside** expectation:

$$
(E[X])^2
$$

These are **not** the same quantity.

---

# Example

Suppose

$$
P(X=1)=0.5,
\qquad
P(X=3)=0.5.
$$

Compute

### Step 1: Find the mean

$$
E[X]
=
1(0.5)+3(0.5)
=
2.
$$

---

### Step 2: Find the second moment

$$
E[X^2]
=
1^2(0.5)+3^2(0.5)
=
0.5+4.5
=
5.
$$

---

### Step 3: Apply the identity

$$
\operatorname{Var}(X)
=
5-2^2
=
5-4
=
1.
$$

---

# Summary

| Quantity | Formula |
|-----------|----------|
| Mean | $$E[X]$$ |
| Variance (definition) | $$E[(X-E[X])^2]$$ |
| Expanded variance formula | $$\boxed{E[X^2]-(E[X])^2}$$ |

---

## Key Ideas Used in the Proof

1. Definition of variance:

$$
\operatorname{Var}(X)=E[(X-\mu)^2]
$$

2. Algebra identity:

$$
(a-b)^2=a^2-2ab+b^2
$$

3. Linearity of expectation:

$$
E[A+B]=E[A]+E[B]
$$

4. Constants factor out:

$$
E[cX]=cE[X]
$$

5. Expectation of a constant is the constant:

$$
E[c]=c.
$$

Together, these are all you need to derive one of the most frequently used identities in probability and statistics.
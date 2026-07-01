# Mean of a Uniform Distribution

## Quick Reference

If

$$
X \sim \text{Uniform}(a,b),
$$

then

$$
\boxed{
E[X]=\frac{a+b}{2}
}
$$

This is simply the midpoint of the interval.

---

# Intuition

A uniform distribution assigns equal probability density to every value in its support.

Since the distribution is perfectly symmetric, the "balance point" or average must lie exactly halfway between the endpoints.

Visualize the support:

```text
a -------------------------- b
              ^
           midpoint
```

The midpoint is

$$
\frac{a+b}{2},
$$

so this is the expected value.

For example,

$$
X\sim\text{Uniform}(1,7)
$$

has

$$
E[X]
=
\frac{1+7}{2}
=
4.
$$

---

# Derivation

Start from the definition of expectation.

The PDF of a Uniform distribution is

$$
f_X(x)
=
\frac{1}{b-a},
\qquad
a\le x\le b.
$$

Then

$$
E[X]
=
\int_a^b x f_X(x)\,dx.
$$

Substitute the PDF:

$$
=
\int_a^b
x\cdot\frac1{b-a}\,dx.
$$

Factor out the constant:

$$
=
\frac1{b-a}
\int_a^b x\,dx.
$$

Integrate:

$$
=
\frac1{b-a}
\left[
\frac{x^2}{2}
\right]_a^b.
$$

Evaluate the limits:

$$
=
\frac1{b-a}
\cdot
\frac{b^2-a^2}{2}.
$$

Factor the difference of squares:

$$
b^2-a^2
=
(b-a)(a+b).
$$

Cancel the common factor:

$$
E[X]
=
\frac{(b-a)(a+b)}{2(b-a)}
=
\boxed{
\frac{a+b}{2}
}.
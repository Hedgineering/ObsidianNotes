# RANDU (The Infamous Linear Congruential Generator)

## Quick Reference

| Property | Value |
|---|---|
| Type | Multiplicative [[Linear Congruential Generator (LCG)]] |
| Recurrence | $X_{i+1}=65539X_i \bmod 2^{31}$ |
| Multiplier | $a=65539=2^{16}+3$ |
| Increment | $c=0$ |
| Modulus | $m=2^{31}$ |
| Output | $R_i=\dfrac{X_i}{2^{31}}$ |
| Era | Widely used on IBM systems in the 1960s–1970s |
| Famous for | One of the worst PRNGs ever deployed |

---

## Definition

RANDU is a multiplicative LCG defined by

$$
X_{i+1}
=
65539X_i
\bmod
2^{31}.
$$

The normalized pseudo-random numbers are

$$
R_i
=
\frac{X_i}{2^{31}}.
$$

Unlike a mixed LCG,

$$
X_{i+1}
=
(aX_i+c)
\bmod m,
$$

RANDU has

$$
c=0.
$$

---

## Why Was RANDU Chosen?

The multiplier

$$
65539
=
2^{16}+3
$$

was chosen because multiplication could be computed efficiently on IBM hardware.

Instead of performing a full multiplication,

$$
65539X
=
(2^{16}+3)X
=
(X\ll16)+3X,
$$

where

- $\ll16$ means a left bit shift by 16 positions,
- shifting and addition were much faster than multiplication on 1960s hardware.

Unfortunately, this optimization produced terrible statistical properties.

---

## The Fundamental Problem

At first glance, the numbers

$$
R_1,R_2,R_3,\ldots
$$

appear approximately uniform on

$$
[0,1).
$$

However, consecutive values are **not independent**.

When viewed in multiple dimensions, extremely strong linear relationships appear.

---

## The Famous 15 Hyperplanes

Suppose we form triples

$$
(R_i,R_{i+1},R_{i+2}).
$$

If the generator were truly random,

these points should approximately fill the cube

$$
[0,1]^3.
$$

Instead,

every point lies on only

$$
15
$$

parallel planes.

This is dramatically worse than a well-designed LCG.

RANDU famously fails the **spectral test**, making it unsuitable for Monte Carlo simulations and scientific computing. 

---

## Why Does This Happen?

Notice

$$
65539
=
2^{16}+3.
$$

Squaring the multiplier,

$$
65539^2
=
(2^{16}+3)^2
$$

gives

$$
2^{32}
+
6\cdot2^{16}
+
9.
$$

Since

$$
2^{32}
\equiv
0
\pmod{2^{31}},
$$

we obtain

$$
65539^2
\equiv
6(65539)-9
\pmod{2^{31}}.
$$

Using the recurrence,

$$
X_{i+1}
=
65539X_i,
$$

we derive

$$
X_{i+2}
=
65539^2X_i
\equiv
6X_{i+1}
-
9X_i
\pmod{2^{31}}.
$$

This means every three consecutive values satisfy

$$
\boxed{
X_{i+2}
=
6X_{i+1}
-
9X_i
\pmod{2^{31}}
}
$$

which is an exact linear relationship.

Instead of behaving independently,

every third value is almost completely determined by the previous two. 

---

## Geometric Interpretation

Consider plotting

$$
(R_i,R_{i+1},R_{i+2})
$$

in three-dimensional space.

A good PRNG produces points filling the cube.

RANDU produces

```
Good Generator

+---------+
| .  . .  |
| . . . . |
|  . . .  |
| . .  .  |
+---------+

Points fill the cube.
```

RANDU instead produces

```
RANDU

///////////
///////////
///////////
///////////

Only 15 parallel planes contain every point.
```

The points never occupy the empty regions between the planes.

---

## Example

Suppose

$$
X_0=1.
$$

The first few values are

$$
1,
65539,
393225,
1769499,
7077969,
26542323,
\ldots
$$

Normalizing,

$$
R_i
=
\frac{X_i}{2^{31}}.
$$

Although these values appear random individually,

their triples satisfy the linear relation above.

---

## Why This Is Bad

Many algorithms assume independent random samples.

Examples include

- Monte Carlo integration,
- numerical optimization,
- statistical simulation,
- physics simulations,
- financial modeling.

If all samples lie on only 15 planes,

large portions of the sample space are never explored.

This can introduce severe bias into computed results.

---

## Spectral Test Failure

The **spectral test** evaluates the lattice structure of an LCG.

A good generator has

- many hyperplanes,
- closely spaced hyperplanes.

RANDU has

- only **15** planes in three dimensions,

making it one of the worst examples ever studied.

---

## Historical Importance

RANDU became the default random number generator on many IBM systems during the 1960s and 1970s.

Because of its popularity,

many published scientific simulations unknowingly used poor-quality random numbers.

After George Marsaglia published **"Random Numbers Fall Mainly in the Planes" (1968)**, RANDU became the classic example of how **a long period and an apparently uniform histogram do not guarantee good randomness.**

---

## Python Implementation

```python
def randu(seed, n):
    """
    RANDU random number generator.
    """
    a = 65539
    m = 2 ** 31

    x = seed
    values = []

    for _ in range(n):
        x = (a * x) % m
        values.append(x / m)

    return values


numbers = randu(seed=1, n=10)

print(numbers)
```

---

## Advantages

- Extremely simple.
- Very fast on older hardware.
- Easy to implement.

---

## Disadvantages

- Catastrophic correlations.
- Fails the spectral test.
- Points lie on only 15 planes in 3D.
- Poor Monte Carlo performance.
- Considered one of the worst PRNGs ever widely deployed.

---

## Common Interview Question

**Why does RANDU appear random in one dimension but fail in higher dimensions?**

Because the histogram of individual values is approximately uniform,

but consecutive outputs satisfy

$$
X_{i+2}
=
6X_{i+1}
-
9X_i
\pmod{2^{31}},
$$

creating strong geometric structure that only becomes visible when plotting tuples such as

$$
(R_i,R_{i+1},R_{i+2}).
$$

---

## Common Mistakes

### Mistake 1

Thinking a uniform histogram implies good randomness.

Uniformity alone does **not** imply independence.

---

### Mistake 2

Ignoring higher-dimensional tests.

Many weak generators look acceptable in one dimension but fail badly when pairs or triples are examined.

---

### Mistake 3

Assuming a long period guarantees quality.

RANDU demonstrates that a generator can have a long period while still producing highly structured, predictable outputs.

---

## Key Takeaways

- RANDU is a multiplicative LCG:

$$
X_{i+1}
=
65539X_i
\bmod2^{31}.
$$

- It was designed for computational efficiency on early IBM hardware.

- Every three consecutive values satisfy

$$
X_{i+2}
=
6X_{i+1}
-
9X_i
\pmod{2^{31}}.
$$

- Consequently,

$$
(R_i,R_{i+1},R_{i+2})
$$

lie on only

$$
15
$$

parallel planes.

- RANDU is now regarded as one of the most famous examples of a poorly designed pseudo-random number generator and serves as a cautionary tale in PRNG design.
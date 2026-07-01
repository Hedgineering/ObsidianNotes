# Full-Period Linear Congruential Generators (LCGs)

## Quick Reference

| Concept | Definition | Importance |
|---|---|---|
| LCG | $X_{n+1} = (aX_n + c) \bmod m$ | Generates a sequence of pseudo-random integers |
| Period | Number of values produced before the sequence repeats | Longer periods generally produce better generators |
| Full Period | Period equals $m$ | Visits every possible state exactly once before repeating |
| Maximum Period | $m$ | There are only $m$ possible states ($0,\ldots,m-1$) |
| Hull-Dobell Theorem | Conditions for an LCG to have full period | Used to choose good parameters $(a,c,m)$ |

---

## Intuition

A pseudo-random number generator eventually repeats because it has only a finite number of possible states.

For an LCG,

$$
X_{n+1}
=
(aX_n+c)
\bmod m,
$$

each state satisfies

$$
0
\le
X_n
<
m.
$$

Therefore there are only

$$
m
$$

possible values.

A **full-period** LCG is one that visits **every possible state exactly once** before repeating.

Think of it as walking through every room in a building exactly one time before returning to the starting room.

---

## Linear Congruential Generator

An LCG is defined by

$$
X_{n+1}
=
(aX_n+c)
\bmod m,
$$

where

- $a$ = multiplier,
- $c$ = increment,
- $m$ = modulus,
- $X_0$ = seed.

The pseudo-random number is usually

$$
R_n
=
\frac{X_n}{m}.
$$

---

## What Is the Period?

The **period** is the number of generated values before the sequence begins repeating.

For example,

$$
1,\;
4,\;
7,\;
2,\;
5,\;
1,\;
4,\;
7,\;
2,\;
5,\;
\ldots
$$

has period

$$
5.
$$

---

## What Does Full Period Mean?

Since there are only

$$
m
$$

possible states,

the longest possible period is

$$
m.
$$

An LCG has **full period** if

$$
\text{Period}
=
m.
$$

This means every integer

$$
0,\;
1,\;
2,\;
\ldots,\;
m-1
$$

appears exactly once before the sequence repeats.

---

## Worked Example: Full-Period Generator

Consider

$$
X_{n+1}
=
(5X_n+1)
\bmod 8,
$$

with

$$
X_0=0.
$$

Generate the sequence.

### Step 1

$$
X_1
=
(5\cdot0+1)
\bmod8
=
1.
$$

### Step 2

$$
X_2
=
(5\cdot1+1)
\bmod8
=
6.
$$

### Step 3

$$
X_3
=
(5\cdot6+1)
\bmod8
=
31
\bmod8
=
7.
$$

Continuing,

| $n$ | $X_n$ |
|---:|---:|
|0|0|
|1|1|
|2|6|
|3|7|
|4|4|
|5|5|
|6|2|
|7|3|
|8|0|

Notice every state

$$
0,1,2,3,4,5,6,7
$$

appears exactly once.

Therefore,

$$
\boxed{\text{Period}=8=m.}
$$

This is a **full-period LCG**.

---

## Worked Example: Not Full Period

Now consider

$$
X_{n+1}
=
(2X_n+1)
\bmod8,
$$

with

$$
X_0=0.
$$

Sequence:

$$
0
\rightarrow
1
\rightarrow
3
\rightarrow
7
\rightarrow
7
\rightarrow
7
\rightarrow
\cdots
$$

Only the values

$$
\{0,1,3,7\}
$$

are ever produced.

States

$$
2,\;
4,\;
5,\;
6
$$

are never visited.

This generator **does not** have full period.

---

## Why Is Full Period Important?

Suppose

$$
m
=
2^{32}.
$$

Then there are

$$
2^{32}
=
4,\!294,\!967,\!296
$$

possible states.

A full-period generator can produce over four billion numbers before repeating.

If the period were only

$$
2^{16}
=
65,\!536,
$$

the sequence would start repeating much sooner, producing obvious patterns.

For Monte Carlo simulations and randomized algorithms, a longer period is generally preferred.

---

## Hull-Dobell Theorem

An LCG

$$
X_{n+1}
=
(aX_n+c)
\bmod m
$$

has full period **if and only if** all three conditions hold.

### Condition 1

The increment and modulus are relatively prime.

$$
\gcd(c,m)=1.
$$

---

### Condition 2

Every prime factor of $m$ divides

$$
a-1.
$$

If

$$
m
=
p_1^{e_1}
p_2^{e_2}
\cdots,
$$

then

$$
p_i
\mid
(a-1)
$$

for every prime factor.

---

### Condition 3

If

$$
4
\mid
m,
$$

then

$$
4
\mid
(a-1).
$$

This additional condition is only needed when the modulus is divisible by $4$.

---

## Why These Conditions Matter

These conditions ensure the generator can eventually reach **every possible state**.

If even one condition fails,

- some states become unreachable,
- the generator falls into shorter cycles,
- the period is less than $m$.

---

## Full Period Does NOT Mean Good Randomness

A common misconception is:

> Full period implies high-quality randomness.

This is **false**.

A generator may have

- maximum period,
- yet still exhibit
  - serial correlation,
  - lattice structure,
  - predictable patterns,
  - failures on statistical randomness tests.

Modern generators such as [[Mersenne Twister]], [[PCG]], [[Xoshiro]], and [[Philox]] are designed to have both **long periods** and **strong statistical properties**.

---

## Python Example

```python
def lcg(seed, a, c, m, n):
    """
    Linear Congruential Generator
    """
    x = seed
    sequence = [x]

    for _ in range(n - 1):
        x = (a * x + c) % m
        sequence.append(x)

    return sequence


sequence = lcg(
    seed=0,
    a=5,
    c=1,
    m=8,
    n=10
)

print(sequence)
```

Output:

```text
[0, 1, 6, 7, 4, 5, 2, 3, 0, 1]
```

---

## Common Mistakes

### Mistake 1

Thinking a full-period generator never repeats.

Every finite-state generator eventually repeats.

A full-period generator simply repeats **as late as mathematically possible**.

---

### Mistake 2

Assuming full period means random.

Period measures **sequence length**, not **statistical quality**.

---

### Mistake 3

Ignoring parameter selection.

Changing

- $a$,
- $c$,
- or $m$

can dramatically shorten the period.

---

## Key Takeaways

- An LCG is

$$
X_{n+1}
=
(aX_n+c)
\bmod m.
$$

- The maximum possible period is

$$
m.
$$

- A **full-period LCG** visits every possible state exactly once before repeating.

- The Hull-Dobell Theorem provides the necessary and sufficient conditions for full period.

- Full period is desirable but **does not guarantee good randomness**. Statistical quality requires additional properties beyond simply having a long cycle.
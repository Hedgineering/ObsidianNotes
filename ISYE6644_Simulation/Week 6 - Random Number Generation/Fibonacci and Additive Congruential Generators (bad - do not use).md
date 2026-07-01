# Fibonacci and Additive Congruential Generators

## Quick Reference

| Concept | Formula | Meaning |
|---|---|---|
| Recurrence | $X_i = (X_{i-1} + X_{i-2}) \bmod m$ | Next state is the sum of the previous two states modulo $m$ |
| Output | $R_i = \dfrac{X_i}{m}$ | Normalizes values into $[0,1)$ |
| Seeds | $X_{-1}, X_0$ | Initial values used to start the sequence |
| Modulus | $m$ | Maximum range of integer states |
| Main Issues | Strong dependence, poor randomness | Neighboring values are highly correlated |

## Intuition

This generator is inspired by the Fibonacci sequence.

Instead of computing

$$
F_i = F_{i-1} + F_{i-2},
$$

it performs the addition modulo some integer $m$:

$$
X_i = (X_{i-1} + X_{i-2}) \bmod m.
$$

The modulo operation prevents the numbers from growing indefinitely while keeping them in the range

$$
0 \le X_i < m.
$$

The resulting integers are then converted into pseudo-random numbers by dividing by $m$.

Although this is more sophisticated than the [[Mid-Square Method for Random Numbers (bad-do not use)]], it still produces strong patterns and correlations, making it unsuitable for high-quality random number generation.

---

## Definition

The generator is defined recursively as

$$
X_i =
(X_{i-1} + X_{i-2})
\bmod m,
\qquad
i = 1,2,\ldots
$$

where

- $X_{-1}$ and $X_0$ are the seeds,
- $m$ is the modulus,
- each state satisfies

$$
0 \le X_i < m.
$$

The pseudo-random number is

$$
R_i
=
\frac{X_i}{m}.
$$

---

## What Does "mod" Mean?

The notation

$$
a = b \bmod m
$$

means that $a$ is the remainder when dividing $b$ by $m$.

For example,

$$
13 \div 7
=
1
\text{ remainder }
6.
$$

Therefore,

$$
13 \bmod 7 = 6.
$$

Equivalently,

$$
6
=
13
\bmod 7.
$$

Another example:

$$
18 \bmod 5 = 3.
$$

---

## Worked Example

Suppose

$$
m = 10,
$$

with seeds

$$
X_{-1}=3,
\qquad
X_0=7.
$$

Compute the sequence.

### Step 1

$$
X_1
=
(7+3)
\bmod 10
=
10
\bmod 10
=
0.
$$

### Step 2

$$
X_2
=
(0+7)
\bmod 10
=
7.
$$

### Step 3

$$
X_3
=
(7+0)
\bmod 10
=
7.
$$

### Step 4

$$
X_4
=
(7+7)
\bmod 10
=
4.
$$

Sequence:

$$
3,\;
7,\;
0,\;
7,\;
7,\;
4,\;
\ldots
$$

Corresponding pseudo-random values:

$$
0.3,\;
0.7,\;
0,\;
0.7,\;
0.7,\;
0.4,\;
\ldots
$$

---

## Why Use Modulo?

Without modulo,

$$
F_i = F_{i-1}+F_{i-2}
$$

grows exponentially.

The modulo operation wraps numbers back into the range

$$
0,\;
1,\;
\ldots,\;
m-1.
$$

This guarantees the generator eventually repeats because there are only finitely many possible states.

---

## Problems with This Generator

### 1. Small Numbers Tend to Follow Small Numbers

Suppose

$$
X_{i-2}=2,
\qquad
X_{i-1}=3.
$$

Then

$$
X_i
=
(2+3)
\bmod m
=
5.
$$

Small values naturally produce another relatively small value.

This introduces positive serial correlation, meaning nearby outputs are statistically dependent.

A good random number generator should not have this behavior.

---

### 2. Impossible Ordering Patterns

Consider three consecutive values

$$
X_{i-1},
\quad
X_i,
\quad
X_{i+1}.
$$

For three independent random numbers, every ordering should be equally likely.

There are

$$
3! = 6
$$

possible orderings, each occurring with probability

$$
\frac16.
$$

The middle value should therefore lie between the other two with probability

$$
\frac26
=
\frac13.
$$

Specifically, the following should each occur naturally:

$$
X_{i-1}
<
X_{i+1}
<
X_i
$$

or

$$
X_i
<
X_{i+1}
<
X_{i-1}.
$$

However, because

$$
X_{i+1}
=
(X_i+X_{i-1})
\bmod m,
$$

these orderings can never occur.

This reveals a deterministic structure that should not exist in truly random sequences.

---

### 3. Strong Dependence

Every value is completely determined by the previous two values.

Mathematically,

$$
X_i
=
f(X_{i-1},X_{i-2}).
$$

Knowing two consecutive states tells you every future state.

True randomness should not allow such prediction.

---

### 4. Finite State Space

Each state satisfies

$$
0
\le
X_i
<
m.
$$

There are only

$$
m^2
$$

possible pairs

$$
(X_{i-1},X_i).
$$

Eventually a pair must repeat.

Once this happens, the entire sequence repeats forever.

Thus every additive congruential generator eventually enters a cycle.

---

## Python Implementation

```python
def additive_congruential(seed1, seed2, modulus, n):
    """
    Fibonacci/Additive Congruential Generator.
    """
    x_prev = seed1
    x_curr = seed2

    values = [x_prev, x_curr]

    for _ in range(n - 2):
        x_next = (x_prev + x_curr) % modulus
        values.append(x_next)
        x_prev, x_curr = x_curr, x_next

    random_numbers = [x / modulus for x in values]

    return values, random_numbers


states, random_values = additive_congruential(
    seed1=3,
    seed2=7,
    modulus=10,
    n=10
)

print(states)
print(random_values)
```

---

## Comparison with the Fibonacci Sequence

Ordinary Fibonacci numbers satisfy

$$
F_i
=
F_{i-1}
+
F_{i-2}.
$$

Additive congruential generators modify this to

$$
X_i
=
(F_{i-1}
+
F_{i-2})
\bmod m.
$$

The modulo operation prevents unbounded growth but introduces cycles.

---

## Common Mistakes

### Mistake 1

Thinking the modulo operation makes the sequence random.

Modulo only keeps numbers bounded—it does not remove deterministic patterns.

---

### Mistake 2

Assuming nearby values are independent.

Every value depends directly on the previous two values.

---

### Mistake 3

Ignoring cycles.

Since only finitely many states exist, the sequence must eventually repeat.

---

## Key Takeaways

The additive congruential generator uses

$$
X_i
=
(X_{i-1}+X_{i-2})
\bmod m.
$$

It outputs

$$
R_i
=
\frac{X_i}{m}.
$$

Although historically important, it has significant flaws:

- Strong serial correlation.
- Small numbers tend to produce small numbers.
- Certain orderings of consecutive values are impossible.
- The sequence is deterministic.
- The generator eventually repeats in cycles.

Modern pseudo-random number generators use far more sophisticated algorithms to avoid these weaknesses.
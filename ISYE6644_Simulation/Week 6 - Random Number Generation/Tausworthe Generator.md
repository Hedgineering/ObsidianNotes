# Tausworthe Generator

## Quick Reference

| Concept | Formula | Meaning |
|---|---|---|
| State | Binary sequence $B_1,B_2,\ldots$ | Generator operates on bits rather than integers |
| General recurrence | $B_i=\left(\sum_{j=1}^{q} c_jB_{i-j}\right)\bmod2$ | Linear recurrence over binary values |
| Practical recurrence | $B_i=(B_{i-r}+B_{i-q})\bmod2$ | Uses only two previous bits |
| XOR form | $B_i=B_{i-r}\oplus B_{i-q}$ | Addition modulo 2 is equivalent to XOR |
| Initialization | Specify $B_1,\ldots,B_q$ | Initial seed bits |
| Maximum period | $2^q-1$ | Achieved for suitable tap positions $(r,q)$ |
| Uniform numbers | Group $\ell$ bits into an integer and divide by $2^\ell$ | Produces values in $[0,1)$ |

---

# Intuition

The Tausworthe generator is a **binary pseudo-random number generator**.

Unlike a Linear Congruential Generator (LCG), which produces integers,

$$
X_0,X_1,X_2,\ldots,
$$

the Tausworthe generator directly produces **bits**

$$
B_1,B_2,B_3,\ldots
$$

where each bit is either

$$
0
\quad\text{or}\quad
1.
$$

Each new bit is computed by taking the XOR of two previous bits.

Because XOR is extremely fast in hardware and software, Tausworthe generators are efficient while still achieving long periods.

---

# General Definition

A Tausworthe generator defines

$$
B_i
=
\left(
\sum_{j=1}^{q}
c_jB_{i-j}
\right)
\bmod2,
$$

where

- $B_i\in\{0,1\}$,
- $c_j\in\{0,1\}$,
- $q$ determines how many previous bits are used.

The coefficients

$$
c_j
=
0
\quad\text{or}\quad
1
$$

determine which previous bits participate in the recurrence.

---

# Practical Implementation

Most implementations simplify the recurrence to

$$
B_i
=
(B_{i-r}+B_{i-q})
\bmod2,
\qquad
0<r<q.
$$

Since addition modulo 2 is exactly XOR,

$$
B_i
=
B_{i-r}
\oplus
B_{i-q}.
$$

This makes each new bit require only one XOR operation.

---

# Why Does XOR Work?

The XOR truth table is

| $A$ | $B$ | $A\oplus B$ |
|---:|---:|---:|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|0|

Notice this is identical to addition modulo 2.

$$
0+0
=
0
\bmod2
$$

$$
0+1
=
1
\bmod2
$$

$$
1+0
=
1
\bmod2
$$

$$
1+1
=
2
\equiv
0
\bmod2
$$

Therefore,

$$
A\oplus B
=
(A+B)
\bmod2.
$$

---

# Initialization

The first

$$
q
$$

bits must be supplied.

That is,

$$
B_1,
B_2,
\ldots,
B_q
$$

are chosen as the initial seed.

After these values are specified, every future bit is determined by the recurrence.

---

# Worked Example

Suppose

$$
r=3,
\qquad
q=5,
$$

with initial bits

$$
B_1=B_2=B_3=B_4=B_5=1.
$$

The recurrence is

$$
B_i
=
B_{i-3}
\oplus
B_{i-5}.
$$

---

## Step 1

Compute

$$
B_6.
$$

Using

$$
B_3
\oplus
B_1,
$$

we obtain

$$
1
\oplus
1
=
0.
$$

Therefore,

$$
B_6=0.
$$

---

## Step 2

Compute

$$
B_7.
$$

Using

$$
B_4
\oplus
B_2,
$$

we obtain

$$
1
\oplus
1
=
0.
$$

Thus,

$$
B_7=0.
$$

---

## Step 3

Compute

$$
B_8.
$$

Using

$$
B_5
\oplus
B_3,
$$

we obtain

$$
1
\oplus
1
=
0.
$$

Therefore,

$$
B_8=0.
$$

---

## Step 4

Compute

$$
B_9.
$$

Using

$$
B_6
\oplus
B_4,
$$

we obtain

$$
0
\oplus
1
=
1.
$$

Thus,

$$
B_9=1.
$$

---

Continuing this process gives

$$
11111\;
000\;
1101\;
1101\;
0100\;
0010\;
0101\;
1001\;
1111\;
\cdots
$$

which matches the lecture example.

---

# Period

For a properly chosen recurrence,

the maximum period is

$$
2^q-1.
$$

Notice this is **not**

$$
2^q.
$$

The all-zero state

$$
000\cdots000
$$

is excluded because

$$
0\oplus0=0,
$$

meaning once all bits become zero,

the generator remains stuck there forever.

Therefore only

$$
2^q-1
$$

nonzero states are usable.

---

## Example

For

$$
q=5,
$$

the maximum period is

$$
2^5-1
=
31.
$$

The lecture example therefore cycles after

$$
31
$$

generated bits.

---

# Converting Bits to Uniform Random Numbers

The generator produces bits.

To obtain numbers in

$$
[0,1),
$$

group together

$$
\ell
$$

consecutive bits.

Interpret them as a binary integer.

Then divide by

$$
2^\ell.
$$

Formally,

if

$$
b_1b_2\cdots b_\ell
$$

represents an integer

$$
I,
$$

then

$$
U
=
\frac{I}{2^\ell}.
$$

---

# Example

Suppose

$$
\ell=4.
$$

The generated groups are

$$
1111_2,
\qquad
1000_2,
\qquad
1101_2,
\qquad
1101_2,
\ldots
$$

Convert each binary number to decimal.

### First group

$$
1111_2
=
15.
$$

Output

$$
\frac{15}{16}
=
0.9375.
$$

---

### Second group

$$
1000_2
=
8.
$$

Output

$$
\frac{8}{16}
=
0.5.
$$

---

### Third group

$$
1101_2
=
13.
$$

Output

$$
\frac{13}{16}
=
0.8125.
$$

---

### Fourth group

$$
1101_2
=
13.
$$

Output

$$
\frac{13}{16}
=
0.8125.
$$

---

The resulting pseudo-random sequence is

$$
0.9375,\;
0.5,\;
0.8125,\;
0.8125,\;
\ldots
$$

---

# Why Is It Better Than an LCG?

Compared to traditional [[Linear Congruential Generator (LCG)]]s,

Tausworthe generators

- use simple XOR operations,
- are extremely fast,
- have very long periods,
- exhibit better statistical properties,
- are well suited to computer hardware.

Many modern generators are based on similar ideas, including [[Linear Feedback Shift Register (LFSR)]] generators and several combined random number generators.

---

# Python Implementation

```python
def tausworthe(seed_bits, r, q, n):
    """
    Simple Tausworthe generator.

    Parameters
    ----------
    seed_bits : list[int]
        Initial q bits.
    r, q : int
        Tap positions.
    n : int
        Total number of bits to generate.
    """
    bits = seed_bits.copy()

    while len(bits) < n:
        new_bit = bits[-r] ^ bits[-q]   # XOR
        bits.append(new_bit)

    return bits


seed = [1, 1, 1, 1, 1]

bits = tausworthe(seed, r=3, q=5, n=32)

print(bits)
```

### Convert Bits to Uniform Numbers

```python
def bits_to_uniform(bits, l=4):
    values = []

    for i in range(0, len(bits), l):
        block = bits[i:i+l]

        if len(block) < l:
            break

        binary = "".join(str(b) for b in block)

        decimal = int(binary, 2)

        values.append(decimal / (2 ** l))

    return values


uniforms = bits_to_uniform(bits, l=4)

print(uniforms)
```

---

# Advantages

- Very fast (only XOR operations)
- Long periods
- Small memory requirements
- Efficient hardware implementation
- Better statistical properties than simple LCGs

---

# Disadvantages

- Poor choice of tap positions can produce short periods.
- A bad seed (all zeros) causes the generator to remain zero forever.
- Modern applications typically use improved generators such as [[Mersenne Twister]], [[PCG]], [[Xoshiro]], or [[Philox]].

---

# Common Mistakes

### Mistake 1

Thinking XOR is different from modulo-2 addition.

They are mathematically identical.

---

### Mistake 2

Using an all-zero seed.

The generator immediately becomes

$$
000000000\cdots
$$

forever.

---

### Mistake 3

Grouping bits incorrectly.

To obtain uniform numbers,

always interpret the bits as a binary integer **before** dividing by

$$
2^\ell.
$$

---

# Key Takeaways

- Tausworthe generators operate on **bits**, not integers.
- Each new bit is computed using XOR of previous bits.
- XOR is equivalent to addition modulo 2.
- Properly chosen parameters give a period of

$$
2^q-1.
$$

- Consecutive groups of bits can be converted into uniform random numbers by dividing the corresponding binary integer by

$$
2^\ell.
$$

- Tausworthe generators are fast, have long periods, and form the basis of many modern random number generators.
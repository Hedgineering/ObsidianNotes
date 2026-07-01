Created by J. von Neumann

# Mid-Square Method

## Quick Reference

| Idea         | Formula / Rule                            | Notes                                               |
| ------------ | ----------------------------------------- | --------------------------------------------------- |
| Goal         | Generate pseudo-random numbers            | Early method by John von Neumann                    |
| State        | $X_i$                                     | Integer with a fixed number of digits               |
| Output       | $R_i = \dfrac{X_i}{10000}$                | Converts the integer state into a number in $[0,1)$ |
| Update rule  | Square $X_i$, then take the middle digits | The middle part becomes $X_{i+1}$                   |
| Main issue   | Positive serial correlation in $R_i$'s    | Consecutive values are not independent              |
| Degeneration | Small seeds can collapse to $0$           | Example: $X_i = 0003$                               |

## Intuition

The mid-square method tries to create randomness by repeatedly squaring the current number and taking the middle digits of the result.

The hope is that squaring “mixes up” the digits enough that the middle digits look random.

However, the method is poor because the sequence can quickly become predictable, correlated, or collapse into bad cycles.

## Setup

Let $X_i$ be a 4-digit integer state.

Define the pseudo-random number:

$$
R_i = \frac{X_i}{10000}
$$

Since $X_i < 10000$, this gives:

$$
0 \le R_i < 1
$$

The update rule is:

$$
X_{i+1}
=
\text{middle 4 digits of } X_i^2
$$

## Worked Example

Suppose the seed is:

$$
X_0 = 6632
$$

Square it:

$$
6632^2 = 43983424
$$

Take the middle 4 digits:

$$
43983424
\rightarrow
9834
$$

So:

$$
X_1 = 9834
$$

Then square again:

$$
9834^2 = 96707556
$$

Take the middle 4 digits:

$$
96707556
\rightarrow
7075
$$

So:

$$
X_2 = 7075
$$

Therefore the sequence begins:

$$
X_0 = 6632,\quad X_1 = 9834,\quad X_2 = 7075,\quad \ldots
$$

The corresponding pseudo-random values are:

$$
R_0 = \frac{6632}{10000} = 0.6632
$$

$$
R_1 = \frac{9834}{10000} = 0.9834
$$

$$
R_2 = \frac{7075}{10000} = 0.7075
$$

## Degeneration Example

Consider:

$$
X_i = 0003
$$

Then:

$$
3^2 = 9
$$

Written as an 8-digit number:

$$
00000009
$$

The middle 4 digits are:

$$
00000009
\rightarrow
0000
$$

So:

$$
X_{i+1} = 0000
$$

Now:

$$
0^2 = 0
$$

and the sequence stays at zero forever:

$$
X_{i+1} = 0,\quad X_{i+2} = 0,\quad X_{i+3} = 0,\quad \ldots
$$

This is called degeneration.

## Python Implementation

```python
def mid_square(seed: int, n: int, digits: int = 4) -> list[int]:
    """
    Generate n states using the mid-square method.
    
    Parameters:
        seed: initial integer state
        n: number of states to generate
        digits: number of digits kept each step
    
    Returns:
        list of generated integer states
    """
    states = []
    x = seed
    width = 2 * digits

    for _ in range(n):
        states.append(x)

        squared = x ** 2
        squared_str = str(squared).zfill(width)

        start = digits // 2
        end = start + digits

        x = int(squared_str[start:end])

    return states


states = mid_square(seed=6632, n=10)

random_values = [x / 10000 for x in states]

print(states)
print(random_values)

```

### Worked Example: Mid-Square Method

Given:

$$
X_0 = 6632
$$

We want:

$$
R_3 = \frac{X_3}{10000}
$$

#### Step 1: Find $X_1$

Square $X_0$:

$$
6632^2 = 43983424
$$

Take the middle 4 digits:

$$
43983424 \rightarrow 9834
$$

So:

$$
X_1 = 9834
$$

#### Step 2: Find $X_2$

Square $X_1$:

$$
9834^2 = 96707556
$$

Take the middle 4 digits:

$$
96707556 \rightarrow 7075
$$

So:

$$
X_2 = 7075
$$

#### Step 3: Find $X_3$

Square $X_2$:

$$
7075^2 = 50055625
$$

Take the middle 4 digits:

$$
50055625 \rightarrow 0556
$$

So:

$$
X_3 = 556
$$

Since we treat the state as 4 digits:

$$
X_3 = 0556
$$

Therefore:

$$
R_3
=
\frac{X_3}{10000}
=
\frac{556}{10000}
=
0.0556
$$

#### Final Answer

$$
\boxed{R_3 = 0.0556}
$$
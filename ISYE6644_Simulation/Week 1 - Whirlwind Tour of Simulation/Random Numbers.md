# How do you generate a random number?

## Linear Congruential Generator

This is a deterministic way of generating a random number that seems to come from a uniform distribution $\text{Uniform}(0, 1)$

This has a large "Cycle Time" -- it doesn't repeat in sequence for a very long time if it's hyperparameters are chosen well.

1. Decide some seed (often an integer) $X_0$
2. Select your next number using $X_{i} = (a * X_{i-1} + b) \mod m$  
	- $m$ is usually some large prime number
	- $a$ is often some large number
	- $b$ is some carefully chosen offset
	- $a$ and $b$ are carefully chosen constants to ensure the sequence generated has a large cardinality before repeating
	- (remainder of some linear combination of the previous number as dividend, and some large prime number $m$ as the divisor)
3.  The random number (from a Uniform distribution) $U_i = X_i / m$
	- $X_i$ is the number generated from step 2
	- $m$ is the modulus number you chose in step 2 (usually some large prime)

Here's one example:

$$
\begin{align}
X_i &= 16807 X_{i-1} \mod{(2^{31} - 1)} \\
R_i &= \frac{X_i}{2^{31} - 1}
\end{align}
$$

Where you must choose some initial seed $X_0$ and $R_i$ is your $i$'th random number.

By the way, $16807 = 7^5$ and $2^{31} - 1$ is a massive prime number.
## Exponential Distribution

You can take the outputs of a $\text{Uniform}(0, 1)$ random variable and turn it into an exponential random variable by taking

$$ \text{Exp}(\lambda) = -\frac{1}{\lambda} ln(U_i) $$
## Discrete Uniform Random Numbers

Suppose your domain is $\{1,2,3,...\}$ and you want to generate the $i$'th number ($X = i$) with probability $\frac{1}{n}$ where $n$ is the cardinality of your domain (like an $n$ sided die toss from Dungeons & Dragons).

If $U \sim Unif(0,1)$, that means $U$ is a random number within $[0,1)$ we can obtain such an $X$ using a scale factor of $n$ and rounding up (ceiling function).

$$
X = \lceil nU \rceil
$$

For example, if you want to generate a random number between 1 and 8 inclusive, we can choose $n$ to be 8, multiply by $U$ and round up the result.

## Generating Arbitrary Discrete Random Variables (Inverse Transform Method)

Suppose

$$
P(X=x)=
\begin{cases}
0.25, & x=-2, \\
0.10, & x=3, \\
0.65, & x=4.2, \\
0, & \text{otherwise}.
\end{cases}
$$

Since the probabilities are not convenient multiples of $\frac{1}{6}$, we cannot easily simulate this random variable with a fair die. Instead, we use the **inverse transform method**.

1. For each possible value of $x$ we calculate the probability density function $f$ (PDF) and cumulative density function $F$ (CDF)
2. Define each **Uniform Interval** based on the CDF values
3. Generate a discrete uniform $U \sim Unif(0, 1)$
4. Use a series of conditional statements to determine the value of the discrete random variable generated
	- For example, if $U = 0.13$ then it falls in the **Uniform Interval** $[0,\,0.25]$ so the value of $X$ will evaluate to $-2$ as per the table below

| $x$ | $f(x)=P(X=x)$ | $F(x)=P(X\le x)$ | Uniform Interval |
|------|------|------|------|
| $-2$ | $0.25$ | $0.25$ | $[0,\,0.25]$ |
| $3$ | $0.10$ | $0.35$ | $(0.25,\,0.35]$ |
| $4.2$ | $0.65$ | $1.00$ | $(0.35,\,1.00)$ |

In other words:

Let

$$
U \sim \mathrm{Unif}(0,1).
$$

Then define

$$
X=
\begin{cases}
-2, & 0 \le U \le 0.25, \\
3, & 0.25 < U \le 0.35, \\
4.2, & 0.35 < U < 1.
\end{cases}
$$

This transformation produces exactly the desired distribution:

$$
P(X=-2)=0.25,
\qquad
P(X=3)=0.10,
\qquad
P(X=4.2)=0.65.
$$

## Generating Continuous Random Variables (Inverse Transform)

**Theorem**: If $X$ is a continuous random variable with CDF $F(x)$, then the random variable $F(X) \sim Unif(0, 1)$ . If you plug the random variable itself into its own CDF function, the resulting distribution is uniform between 0 and 1.

So if you want to generate values of the random variable $X$, simply set $F(X) = U \sim Unif(0, 1)$ and solve for $X = F^{-1}(U)$.

**Example**:
Suppose $X \sim Exp(\lambda)$

Then $F(x) = 1-e^{-\lambda x}$ for $x > 0$.

Set $F(X) = 1- e^{-\lambda X} = U$ and solve for $X$.

$$
\begin{align}
F(X) = U &= 1- e^{-\lambda X} \\
e^{-\lambda X} &= 1 - U  \\
-\lambda X &= ln(1 - U)  \\
X &= \frac{-1}{\lambda} ln(1 - U)  \\
X &= \frac{-1}{\lambda} ln(U) \text{ since } 1 - \text{uniform RV} \text{ is also a uniform RV} \\
X &= \frac{-ln(U)}{\lambda} \sim Exp(\lambda)
\end{align}
$$

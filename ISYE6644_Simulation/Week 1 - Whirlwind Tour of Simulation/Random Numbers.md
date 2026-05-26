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

## Exponential Distribution

You can take the outputs of a $\text{Uniform}(0, 1)$ random variable and turn it into an exponential random variable by taking

$$ \text{Exp}(\lambda) = -\frac{1}{\lambda} ln(U_i) $$

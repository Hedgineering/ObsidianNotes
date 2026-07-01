We want to find an algorithm that will generate pseudo-random numbers that are reproducible, appear to be i.i.d to statistical tests, and generate a $Unif(0,1)$ distribution.
### Bad Generators
There will be discussions of some lousy (bad) generators.
- Random device output
	- toss a coin, roll a die, least significant digits of atomic clock, particle count by Grieger counter, etc.
	- Storing and repeating experiment is difficult
	- Not easily reproducible

- Table of random numbers
	- e.g. *A Million Random Digits with 100,000 Normal Deviates* book has a table of many samplings of a random distribution from some scientific instrument, just in a table
	- Cumbersome, slow, tables are too small for large statistical experiments
	- Not really random once tabled, it's one realization sequence
- Midsquare - [[Mid-Square Method for Random Numbers (bad-do not use)]]
	- Pick a seed
	- Square it
	- Take the middle $n$ digits of the resulting squared number
	- Divide by $10^n$ where $n$ = number of digits in your mid-square
- Fibonacci - [[Fibonacci and Additive Congruential Generators (bad - do not use)]]
	- Like LCG but just adds two seed numbers, takes modulus (remainder) with $m$ and then divides by $m$
	- Adjacent numbers are highly correlated so this is suboptimal

### Linear Congruential Generators (LCGs)
We've already somewhat discussed Linear Congruential Generators [[Random Numbers#How do you generate a random number?#Linear Congruential Generator]] but we will dive deeper on this.

LCGs are the most widely used generators, when implemented properly (coefficients and modulus constants $a, c, m$ are chosen well) these have decent statistical properties and long *period* or *cycle length* i.e. time until LCG starts repeating.

$X_i = (aX_{i-1} + c) \mod{m}$ , where $X_0$ is the seed
$R_i = X_i / m, \hspace{0.25in} i = 1,2,...$ 

Side note: if $c = 0$  this is called a *multiplicative* generator.

You can find a python implementation in [[When is an LCG full period#Python Example]]

You can visually test if a generator is good by plotting pairs or triples or n-tuples of points in a scatterplot to visualize autocorrelation. For example, you could plot pairs of $(R_{i-1}, R_i)$ on $\mathbb{R}^2$ or triples of  $(R_{i-2}, R_{i-1}, R_i)$ on $\mathbb{R}^3$ and see if you see a consistent pattern (many lines or hyperplanes). If the generator or choice of constants are good, it should look overall random / noisy / uncorrelated.
### Tausworthe Generators

This is another good generator that tends to be more popular with comp-sci folks.

The idea is basically
1. Initialize some set of $b$ bits
2. Define some pair of constants $x$ and $y$ such that $x < y < b$
3. To generate the next bit, perform the **XOR** operation on $x$ bits ago with $y$ bits ago
4. Repeat step 3 to get many bits
5. Form groups of $n$ bits (put parenthesis around each set of $n$ bits along the stream you generated)
6. Convert each group of $n$ bits to decimal and divide by $2^n$ to normalize to $[0,1)$ 

### Generalizations of LCGs

In practical applications, we often need several billion pseudo random numbers, but as of the year 2026 we almost never need more than $2^{100}$ pseudo random numbers for our statistical applications.


You can do different combinations to create better LCGs. See [[Combination Application of Linear Congruential Generator]]

A simple generalization is the following where $a_i$ is a series of constants
$$
X_i = (\sum_{j=1}^{q} a_i X_{i-j}) \mod{m}
$$
This is essentially generating weighted sums of prior LCG numbers before doing another modulus, and using this, extremely large periods are possible (up to $m^q-1$ if parameters are chosen properly), but watch out for cases like the Fibonacci special case from above!

You can also combine results of two different generators, shuffle the results or alternate selections of one generator versus another.

See "Mersenne Twister" generator created by Matsumoto and Nishimura that has a period of $2^{19937} - 1$ 

### Choosing a Good Generator

#### Theory

**Theorem:** The generator $X_i = aX_{i-1} \mod{2^n}, \hspace{0.25in} (n > 3)$ can have cycle length of at most $2^{n-2}$ which can be achieved when $X_0$ is odd and $a = 8k+3$ or $a=8k+5$ for some $k$.

**Theorem:** The generator $X_i = (aX_{i-1} + c) \mod{m}, \hspace{0.25in} (c > 3)$ has full cycle if
- $c$ and $m$ are relatively prime (no common prime factors)
- $a-1$ is a multiple of every prime which divides $m$ (find all prime factors of $m$, ensure some integer constant $k_i$ exists for each prime factor $p_i$ such that $p_i k_i = a-1$)
- $a-1$ is a multiple of 4 if 4 divides $m$ (if $m \mod{4} \equiv 0$) 

**Corollary of above theorem:** The generator $X_i = (aX_{i-1} + c) \mod{2^n}, \hspace{0.25in} (c,n > 1)$ has full cycle if $c$ is odd and $a = 4k+1$ for some $k$

**Theorem:** The *multiplicative* generator  $X_i = (aX_{i-1}) \mod{m}$ with prime $m$ has full period $(m-1)$ if and only if
- $m$ divides $a^{m-1}-1$ 
- for all integers $i < m - 1$, $m$ does not divide $a^i - 1$ 

**Remark**: for $m = 2^{31} - 1$ it can be shown that 534,600,000 multipliers yield full period, the "best" of which is $a=950,706,376$ (see Fishman and Moore 1986).

#### Geometric Considerations

**Theorem:** The $k$-tuples

$$
(R_i,\ldots,R_{i+k-1}),
\qquad
i \ge 1,
$$

generated by multiplicative generators lie on parallel hyperplanes in

$$
[0,1]^k.
$$

The following geometric quantities are of interest:

- **Minimum number of hyperplanes** (in all directions). Find the multiplier that **maximizes** this number.

- **Maximum distance between parallel hyperplanes.** Find the multiplier that **minimizes** this distance.

- **Minimum Euclidean distance between adjacent $k$-tuples.** Find the multiplier that **maximizes** this distance.

**Remark:** The **RANDU** generator is particularly poor because its generated points lie on only **15 hyperplanes**.

Can also look at **one-step serial correlation**.

The serial correlation of an LCG satisfies (Greenberger, 1961):

$$
\operatorname{Corr}(R_1,R_2)
\le
\frac{1}{a}
\left(
1
-
\frac{6c}{m}
+
6\left(\frac{c}{m}\right)^2
\right)
+
\frac{a+6}{m}.
$$

This upper bound is very small (low correlation is a good thing, means it is more random-like) for

$$
m
\approx
2\times10^9
$$

and, for example,

$$
a=16807.
$$

There are many other theoretical criteria that can be used to evaluate the performance of a particular pseudo-random number (PRN) generator.
#### Statistical Tests

We can look at 2 classes of tests:
- **Goodness-of-fit tests**: Are the PRNs approximately $\sim Unif(0,1)$?
- **Independence tests:** Are the PRNs approximately independent of one another?

If a generator passes both types of tests (in addition to a few others we will discuss later) then it is a good candidate for usage for overall generation of PRNs.

Generally we test some null hypothesis $H_0$ versus some alternative hypothesis $H_1$ . 

We regard $H_0$ as the status quo, and only reject it if we have "ample" evidence against it (innocent until proven guilty) -- 

i.e. "There is such a miniscule probability of $H_0$ being true (below some threshold) that we will reject it in favor of $H_1$ which is much more likely to be the case".

Usually we really want to avoid incorrect rejections of $H_0$ (avoid Type I error ; avoid false positives)
- **Type I error**: False positive 
	- reject $H_0$ even though it's true -- $\alpha = P(\text{Reject } H_0 | H_0 \text{ true})$
- **Type II error**: False negative 
	- fail to reject $H_0$ even though it's false -- $\beta = P(\text{Accept } H_0 | H_0 \text{ false})$

In the design of the test, we set the **level of significance** which is essentially the probability of making a **Type I error**

See [[Hypothesis Testing]] for more info.


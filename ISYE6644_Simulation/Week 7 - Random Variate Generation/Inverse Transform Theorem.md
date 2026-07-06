## Inverse Transform Theorem

> [!theorem] Inverse Transform Theorem
> Let $X$ be a continuous random variable with c.d.f. $F(x)$. Then:
> $$F(X) \sim \mathcal{U}(0,1)$$

---

### Proof

Let $Y = F(X)$, and suppose $Y$ has c.d.f. $G(y)$.

**Step 1:** Write $G(y)$ in terms of $X$:

$$
G(y) = P(Y \leq y) = P(F(X) \leq y)
$$

**Step 2:** Since $F$ is invertible (strictly increasing c.d.f.), apply $F^{-1}$ to both sides of the inequality:

$$
P(F(X) \leq y) = P(X \leq F^{-1}(y))
$$

**Step 3:** But $P(X \leq a) = F(a)$ by definition of the c.d.f., so:

$$
P(X \leq F^{-1}(y)) = F(F^{-1}(y)) = y
$$

**Conclusion:**

$$
G(y) = y \quad \Longrightarrow \quad Y = F(X) \sim \mathcal{U}(0,1) \qquad \blacksquare
$$

---

> [!note] Intuition
> This is the theoretical basis for **inverse transform sampling**: if you can compute $F^{-1}$, you can generate samples from any distribution by first drawing $U \sim \mathcal{U}(0,1)$ and setting $X = F^{-1}(U)$.

## Applications of the Inverse Transform Theorem (Continuous Examples)

### Example 1: Uniform Distribution $\mathcal{U}(a,b)$

The c.d.f. is:

$$
F(x) = \frac{x-a}{b-a}, \qquad a \leq x \leq b
$$

**Solving for $X$:** Set $F(X) = U$ and solve:

$$
\frac{X-a}{b-a} = U
$$

$$
X = a + (b-a)U \qquad \blacksquare
$$

---

### Example 2: Exponential Distribution $\text{Exp}(\lambda)$

The c.d.f. is:

$$
F(x) = 1 - e^{-\lambda x}, \qquad x > 0
$$

**Solving for $X$:** Set $F(X) = U$:

$$
1 - e^{-\lambda X} = U
$$

$$
e^{-\lambda X} = 1 - U
$$

$$
-\lambda X = \ln(1-U)
$$

$$
X = -\frac{1}{\lambda}\ln(1-U)
$$

> [!note]
> Since $U \sim \mathcal{U}(0,1)$ implies $1-U \sim \mathcal{U}(0,1)$ as well, we can equivalently write:
> $$X = -\frac{1}{\lambda}\ln(U) \qquad \blacksquare$$

### Example 3: Weibull Distribution

The c.d.f. is:

$$
F(x) = 1 - e^{-(\lambda x)^\beta}, \qquad x > 0
$$

**Solving for $X$:** Set $F(X) = U$:

$$
1 - e^{-(\lambda X)^\beta} = U
$$

$$
e^{-(\lambda X)^\beta} = 1 - U
$$

$$
-(\lambda X)^\beta = \ln(1-U)
$$

$$
(\lambda X)^\beta = -\ln(1-U)
$$

$$
\lambda X = [-\ln(1-U)]^{1/\beta}
$$

$$
X = \frac{1}{\lambda}\left[-\ln(1-U)\right]^{1/\beta}
$$

> [!note]
> Using the equivalence $1-U \sim \mathcal{U}(0,1)$ when $U \sim \mathcal{U}(0,1)$, we can also write:
> $$X = \frac{1}{\lambda}\left[-\ln(U)\right]^{1/\beta} \qquad \blacksquare$$

### Example 4: Triangular$(0,1,2)$ Distribution

**p.d.f.:**

$$
f(x) =
\begin{cases}
x & \text{if } 0 \leq x < 1 \\
2-x & \text{if } 1 \leq x \leq 2
\end{cases}
$$

**c.d.f.:**

$$
F(x) =
\begin{cases}
x^2/2 & \text{if } 0 \leq x < 1 \\
1 - (x-2)^2/2 & \text{if } 1 \leq x \leq 2
\end{cases}
$$

> [!warning]
> Since $F$ is **piecewise**, we must invert each piece **separately**, and figure out which piece applies based on the value of $U$.

---

#### Case 1: $U < 1/2$

This corresponds to $X \in [0,1)$, where $F(X) = X^2/2$. Solve:

$$
\frac{X^2}{2} = U \quad \Longrightarrow \quad X = \sqrt{2U}
$$

#### Case 2: $U \geq 1/2$

This corresponds to $X \in [1,2]$, where $F(X) = 1 - (X-2)^2/2$. Solving on this interval, the only root landing in $[1,2]$ is:

$$
X = 2 - \sqrt{2(1-U)}
$$

**Putting it together:**

$$
X =
\begin{cases}
\sqrt{2U} & \text{if } U < 1/2 \\
2 - \sqrt{2(1-U)} & \text{if } U \geq 1/2
\end{cases}
$$

**Numerical check:** If $U = 0.4$ (so $U<1/2$, use Case 1):

$$
X = \sqrt{2(0.4)} = \sqrt{0.8} \qquad \blacksquare
$$

---

> [!danger] Remark: Do NOT replace $U$ by $1-U$ here!
> In the exponential/Weibull examples, swapping $U \to 1-U$ was valid because $F$ was a **single global formula** valid over the whole support — since $U \sim \mathcal{U}(0,1) \Rightarrow 1-U \sim \mathcal{U}(0,1)$, plugging either into the *same* inverse formula gives an equally correct sampler (same distribution, different pairing of random values).
>
> Here, $F$ is **piecewise**, and the branch you use *depends on the actual value of $U$* relative to $1/2$ — the two pieces are not interchangeable copies of the same formula. If you naively substituted $1-U$ into the Case 1 formula ($X = \sqrt{2(1-U)}$), you'd be:
> - Evaluating the *wrong branch condition* (an event with $U < 1/2$ would get mapped as if $U \geq 1/2$), and
> - Producing an $X$ that does **not** have the correct triangular distribution overall.
>
> **Takeaway:** the $U \leftrightarrow 1-U$ shortcut only works when $F^{-1}$ is a single closed-form expression valid on the entire range. When $F$ is piecewise, each branch's inverse is tied to a specific sub-range of $U$, and you must check which condition ($U<1/2$ vs. $U \geq 1/2$) the *drawn* $U$ actually satisfies.

---

> [!tip] General Lesson: Piecewise CDFs
> Whenever $F(x)$ is defined by cases:
> 1. Identify the **range of $U = F(x)$ values** corresponding to each piece (here: $[0, 1/2)$ and $[1/2, 1]$).
> 2. Invert **each piece on its own sub-domain**.
> 3. Assemble the final inverse as a piecewise function of $U$, and always route each drawn $U$ through the branch condition — never assume symmetry shortcuts apply.

## Discrete Examples

For discrete distributions, the inverse transform method doesn't give a clean closed-form $F^{-1}$ — instead, it's often best to **construct a table** mapping ranges of $U$ to values of $X$.

**General idea:** partition $(0,1)$ into intervals matching the jumps of $F(x)$, then assign $X = x$ whenever $U$ falls in the corresponding interval.

---

### Baby Discrete Example: Bernoulli$(p)$

| $x$ | $P(X=x)$ | $F(x)$ | $\mathcal{U}(0,1)$'s |
|:---:|:--------:|:------:|:---------------------:|
| $0$ | $1-p$ | $1-p$ | $[0,\ 1-p]$ |
| $1$ | $p$ | $1$ | $(1-p,\ 1]$ |

**Rule:**

$$
\text{If } U \leq 1-p, \text{ take } X = 0; \text{ otherwise, take } X = 1
$$

**Numerical check:** If $p = 0.75$ and we generate $U = 0.13$:

$$
U = 0.13 \leq 1-p = 0.25 \quad \Longrightarrow \quad X = 0 \qquad \blacksquare
$$

#### Alternate "Backwards" Table for Bernoulli$(p)$

> [!info]
> This alternate ordering isn't strictly the inverse transform method (since it doesn't follow $F(x)$'s natural increasing order), but it's often the more convenient/commonly used version in practice.

| $x$ | $P(X=x)$ | $\mathcal{U}(0,1)$'s |
|:---:|:--------:|:---------------------:|
| $1$ | $p$ | $[0,\ p]$ |
| $0$ | $1-p$ | $(p,\ 1]$ |

**Rule:**

$$
\text{If } U \leq p, \text{ take } X = 1; \text{ otherwise, take } X = 0 \qquad \blacksquare
$$

---

> [!tip] Why this works
> The intervals $[0, 1-p]$ and $(1-p, 1]$ have lengths $1-p$ and $p$ respectively. Since $U \sim \mathcal{U}(0,1)$, the probability $U$ lands in an interval equals that interval's length — matching $P(X=0) = 1-p$ and $P(X=1) = p$ exactly. 
> 
> This is the discrete analogue of inverting $F$: we're using the **jump points of $F$** as cutoffs for $U$.

> [!tip] Comparing the two tables
> - **Standard (inverse transform) order:** list outcomes in increasing order of $x$, cutoffs built from $F(x)$.
> - **Backwards order:** list outcomes in whatever order is convenient (here, "success" first), with cutoffs built directly from the probabilities themselves.
>
> Both are valid ways to simulate from the distribution — what matters is that the **interval lengths match the probabilities**, not the order in which outcomes are listed. This flexibility becomes especially handy for larger discrete distributions where you might want to order outcomes by likelihood (most probable first) for efficiency.
---

### Example: General Discrete p.m.f.

Suppose we have a slightly less-trivial discrete p.m.f.:

| $x$ | $P(X=x)$ | $F(x)$ | $\mathcal{U}(0,1)$'s |
|:----:|:--------:|:------:|:---------------------:|
| $-1$ | $0.6$ | $0.6$ | $[0.0,\ 0.6]$ |
| $2.5$ | $0.3$ | $0.9$ | $(0.6,\ 0.9]$ |
| $4$ | $0.1$ | $1.0$ | $(0.9,\ 1.0]$ |

**Numerical check:** If $U = 0.63$:

$$
0.6 < U = 0.63 \leq 0.9 \quad \Longrightarrow \quad X = 2.5 \qquad \blacksquare
$$

---

> [!tip] General Procedure for Discrete Inverse Transform
> 1. List all possible values of $x$ (in increasing order, for the "true" inverse transform version).
> 2. Compute the cumulative probabilities $F(x) = \sum_{x_i \leq x} P(X=x_i)$.
> 3. Partition $(0,1]$ into intervals $\big(F(x_{i-1}),\ F(x_i)\big]$, one per outcome.
> 4. Generate $U \sim \mathcal{U}(0,1)$, find which interval it falls into, and return the corresponding $x$.
>
> This works because the length of each interval equals $P(X = x_i)$, so $P(U \in \text{interval}_i) = P(X = x_i)$ exactly — matching the target distribution.

### Example: Discrete Uniform Distribution (No Table Needed)

> [!info]
> Sometimes there's an easy way to avoid constructing a table entirely — when the p.m.f. has enough structure, we can write $F^{-1}$ directly.

The discrete uniform distribution on $\{1, 2, \ldots, n\}$:

$$
P(X=k) = \frac{1}{n}, \qquad k = 1, 2, \ldots, n
$$

**Formula:**

$$
X = \lceil nU \rceil
$$

where $\lceil \cdot \rceil$ is the **ceiling function**.

This is like an $n$-sided die toss/roll like from Dungeons & Dragons.

**Numerical check:** If $n = 10$ and $U = 0.376$:

$$
X = \lceil 10(0.376) \rceil = \lceil 3.76 \rceil = 4 \qquad \blacksquare
$$

---

> [!tip] Why this works
> Each integer $k \in \{1, \ldots, n\}$ corresponds to the interval $U \in \left(\frac{k-1}{n},\ \frac{k}{n}\right]$, which has length $\frac{1}{n}$ — matching $P(X=k) = \frac{1}{n}$. Multiplying $U$ by $n$ rescales $(0,1]$ to $(0,n]$, and taking the ceiling snaps $nU$ up to the correct integer bucket. This avoids ever writing out a table since the intervals are evenly spaced and easy to compute directly.

### Example: Geometric Distribution

**p.m.f. and c.d.f.:**

$$
f(k) = q^{k-1}p \qquad \text{and} \qquad F(k) = 1 - q^k, \qquad k = 1, 2, \ldots
$$

where $q = 1-p$.

---

**Derivation:** We want the smallest integer $k$ such that $F(k) \geq U$:

$$
X = \min\left[k : 1 - q^k \geq U\right]
$$

Solving $1 - q^k = U$ for $k$ (treating $k$ as continuous first):

$$
q^k = 1-U \quad \Longrightarrow \quad k\ln(q) = \ln(1-U) \quad \Longrightarrow \quad k = \frac{\ln(1-U)}{\ln(q)} = \frac{\ln(1-U)}{\ln(1-p)}
$$

Since we need the **smallest integer** $k$ satisfying the inequality (not an exact real root), we round up:

$$
X = \left\lceil \frac{\ln(1-U)}{\ln(1-p)} \right\rceil \sim \left\lceil \frac{\ln(U)}{\ln(1-p)} \right\rceil
$$

> [!note]
> As before, since $U \sim \mathcal{U}(0,1) \Rightarrow 1-U \sim \mathcal{U}(0,1)$, both forms are equivalent in distribution — **and this substitution is valid here** because $F^{-1}$ (before rounding) is a single global formula over the whole support, unlike the piecewise triangular example.

---

**Numerical check:** If $p = 0.3$ (so $q = 0.7$) and $U = 0.72$:

$$
X = \left\lceil \frac{\ln(1-0.72)}{\ln(1-0.3)} \right\rceil = \left\lceil \frac{\ln(0.28)}{\ln(0.7)} \right\rceil = \lceil 3.58\ldots \rceil = 4 \qquad \blacksquare
$$

---

> [!tip] Why the ceiling function appears
> Unlike continuous distributions where $F^{-1}$ gives an exact real-valued $X$, the geometric distribution is discrete and its c.d.f. only takes specific "jump" values. We need the **first** integer $k$ at which the cumulative probability $F(k)$ reaches or exceeds $U$ — hence $\lceil \cdot \rceil$ rather than an exact solve. This mirrors the general discrete inverse transform principle (find smallest $x$ with $F(x) \geq U$), just expressed in closed form instead of a table.

### Remark: Alternate Way to Generate Geom$(p)$

> [!info] Key Idea
> We can also generate $\text{Geom}(p)$ by **counting Bernoulli$(p)$ trials until the first success** — this follows directly from the definition of the geometric distribution as the trial number of the first success in a sequence of i.i.d. Bernoulli trials.

---

#### Easy Example: Dice Tosses

Generate $X \sim \text{Geom}(1/6)$.

This is the same as counting the number of dice tosses until a $3$ (or any fixed target number) appears, where each toss is an i.i.d. $\text{Bern}(1/6)$ trial (success = target number appears).

**Example rolls:** $6, 2, 1, 4, 3$

The first success (rolling a $3$) occurs on the **5th** trial:

$$
X = 5 \qquad \blacksquare
$$

---

#### General Method (Not Just Dice)

> [!tip]
> Life isn't always dice tosses — a **general way** to generate $\text{Geom}(p)$ is to count the number of trials until $U_i \leq p$, generating fresh i.i.d. $U_i \sim \mathcal{U}(0,1)$'s one at a time.

**Numerical check:** If $p = 0.3$, and we generate:

$$
U_1 = 0.71, \quad U_2 = 0.96, \quad U_3 = 0.12
$$

Since $U_1 > p$ and $U_2 > p$, but $U_3 = 0.12 \leq p = 0.3$ (first success on trial 3):

$$
X = 3 \qquad \blacksquare
$$

---

> [!note] Connection to the Closed-Form Method
> This trial-counting approach is conceptually simpler but requires generating **multiple** $U_i$'s per simulated $X$ (one per trial), whereas the earlier closed-form inverse transform
> $$X = \left\lceil \frac{\ln(U)}{\ln(1-p)} \right\rceil$$
> generates the same $\text{Geom}(p)$ variable using only a **single** $U$. Both are valid — the trial-counting method is more intuitive/direct from the definition, while the inverse transform method is more efficient computationally.

### Remark: Discrete Distributions with Infinitely Many Values

> [!warning] The Problem
> If a discrete distribution (like $\text{Pois}(\lambda)$) has an **infinite** number of possible values, you can't write out a complete table. Instead:
> 1. Write out table entries **until the c.d.f. is nearly 1** (captures essentially all the probability mass).
> 2. Generate exactly **one** $U$.
> 3. **Search** the table until you find $x_i$ such that:
> $$U \in \big(F(x_{i-1}),\ F(x_i)\big]$$
> 4. Return $X = F^{-1}(U) = x_i$.

**General table structure:**

| $x$ | $P(X=x)$ | $F(x)$ | $\mathcal{U}(0,1)$'s |
|:---:|:--------:|:------:|:---------------------:|
| $x_1$ | $f(x_1)$ | $F(x_1)$ | $[0,\ F(x_1)]$ |
| $x_2$ | $f(x_2)$ | $F(x_2)$ | $(F(x_1),\ F(x_2)]$ |
| $x_3$ | $f(x_3)$ | $F(x_3)$ | $(F(x_2),\ F(x_3)]$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ |

---

### Example: Poisson$(2)$ Distribution

A **Poisson distribution with parameter λ=2** models the number of times some event occurs in a fixed interval (of time, space, distance, etc.) when:

- Events occur **independently** of one another.
- The average rate is **2 events per interval**.
- Events occur randomly and individually (not in batches).

Suppose $X \sim \text{Pois}(2)$, so that:

$$
f(x) = \frac{e^{-2}2^x}{x!}, \qquad x = 0, 1, 2, \ldots
$$

**Table (truncated):**

| $x$ | $f(x)$ | $F(x)$ | $\mathcal{U}(0,1)$'s |
|:---:|:------:|:------:|:---------------------:|
| $0$ | $0.1353$ | $0.1353$ | $[0,\ 0.1353]$ |
| $1$ | $0.2706$ | $0.4059$ | $(0.1353,\ 0.4059]$ |
| $2$ | $0.2706$ | $0.6765$ | $(0.4059,\ 0.6765]$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ |

**Numerical check:** If $U = 0.313$:

$$
0.1353 < U = 0.313 \leq 0.4059 \quad \Longrightarrow \quad X = 1 \qquad \blacksquare
$$

---

> [!tip] Practical Note
> Since we can't enumerate infinitely many rows, in practice you'd:
> - Truncate the table once $F(x)$ is close enough to $1$ (e.g., within some tolerance like $10^{-10}$) for the application's precision needs, **or**
> - Use a **search algorithm** (linear or binary search) that computes $F(x)$ **on the fly** term-by-term (adding $f(x)$ increments) rather than pre-storing an enormous table, stopping as soon as the cumulative sum exceeds $U$.
>
> This is exactly how many software implementations (e.g., `rpois` in R) work under the hood for unbounded discrete distributions.

## Empirical Distribution Example



---

> [!summary] General Recipe (Inverse Transform Sampling)
> 1. Draw $U \sim \mathcal{U}(0,1)$
> 2. Solve $F(X) = U$ for $X$ in terms of $U$
> 3. The resulting $X = F^{-1}(U)$ has c.d.f. $F$
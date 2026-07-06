## Continuous Empirical Distributions

> [!info] Motivation
> If you can't find a good theoretical distribution to model a certain random variable, you may want to use the **empirical c.d.f.** of the observed data $X_1, X_2, \ldots, X_n$ instead.

---

### Definition: Empirical c.d.f.

$$
\hat{F}_n(x) \equiv \frac{\text{number of } X_i\text{'s} \leq x}{n}
$$

> [!note]
> $\hat{F}_n(x)$ is a **step function** with jumps of height $\frac{1}{n}$ — one jump occurs at each observed data point $X_i$.

---

### Good News: Glivenko–Cantelli Lemma

> [!theorem] Glivenko–Cantelli Lemma
> Even though $X$ is continuous, as $n \to \infty$:
> $$\hat{F}_n(x) \to F(x) \quad \text{for all } x$$
>
> So $\hat{F}_n(x)$ is a **good approximation** to the true (unknown) c.d.f. $F(x)$, no matter what the underlying continuous distribution actually is.

---

> [!tip] Why this matters
> This result justifies **simulating from the data itself** when the true distribution is unknown or hard to fit theoretically:
> - Build $\hat{F}_n(x)$ from the observed sample.
> - Apply inverse transform sampling to $\hat{F}_n$ (since it's just a discrete-like step function) to generate new synthetic data resembling the original sample.
> - This is the theoretical basis for methods like the **bootstrap**, which resamples from $\hat{F}_n$ instead of assuming a parametric model.

## Simulating from a Continuous Empirical Distribution

> [!info]
> Software like **ARENA** provides built-in functions `DISC` and `CONT` to generate random variables from the empirical c.d.f.'s of discrete and continuous distributions, respectively. Here we derive the continuous version ourselves.

---

### Step 0: Order the Data

Define the **ordered points** (order statistics):

$$
X_{(1)} \leq X_{(2)} \leq \cdots \leq X_{(n)}
$$

**Example:** If $X_1 = 4$, $X_2 = 1$, $X_3 = 6$, then:

$$
X_{(1)} = 1, \quad X_{(2)} = 4, \quad X_{(3)} = 6
$$

> [!note] Picture: $\hat{F}_n(x)$ vs. true $F(x)$
> The empirical c.d.f. $\hat{F}_n(x)$ is a **step function** (jumps of $1/n$ at each $X_{(i)}$), while the true, unknown c.d.f. $F(x)$ is some smooth increasing curve running roughly through the "middle" of those steps. The idea below is to **connect the dots** of $\hat{F}_n$ with straight lines to get a continuous approximation to $F$.

---

### Step 1: Linear Interpolation Between Order Statistics

Since we only have a **finite** number $n$ of data points, we can turn the empirical c.d.f. into a genuinely **continuous** random variable by linearly interpolating between the $X_{(i)}$'s:

$$
F(x) =
\begin{cases}
0 & \text{if } x < X_{(1)} \\[6pt]
\dfrac{i-1}{n-1} + \dfrac{x - X_{(i)}}{(n-1)\left(X_{(i+1)} - X_{(i)}\right)} & \text{if } X_{(i)} \leq x < X_{(i+1)}, \ \forall i \\[10pt]
1 & \text{if } x \geq X_{(n)}
\end{cases}
$$

---

### Step 2: Inverting $F$ — the Algorithm

Think of this like a **dice toss over $1, 2, \ldots, n-1$** to pick which segment to land in, then a $\mathcal{U}(0,1)$ draw for *where* within that segment.

**① Set $F(X) = U \sim \mathcal{U}(0,1)$.** Let:

$$
P = (n-1)U \qquad \text{and} \qquad I = \lceil P \rceil
$$

> $I$ is the **random starting point** — i.e., which interval $\big[X_{(I)}, X_{(I+1)}\big)$ we land in.

**② Solve to get:**

$$
X = X_{(I)} + \underbrace{(P - I + 1)}_{\text{turns out} \sim \mathcal{U}(0,1)} \big(X_{(I+1)} - X_{(I)}\big)
$$

> [!tip] Interpreting the pieces
> - $X_{(I)}$: the **random starting point**, an order statistic chosen uniformly at random from $\{X_{(1)}, \ldots, X_{(n-1)}\}$.
> - $(P - I + 1)$: turns out to itself be $\sim \mathcal{U}(0,1)$ — it tells you **how far towards the next point** $X_{(I+1)}$ to travel.
> - Together: $X$ is a **uniformly random point along the segment** connecting $X_{(I)}$ and $X_{(I+1)}$.

---

### Worked Example

Suppose $X_{(1)} = 1$, $X_{(2)} = 4$, $X_{(3)} = 6$ (so $n=3$). Let $U = 0.73$.

**Compute $P$ and $I$:**

$$
P = (n-1)U = 2(0.73) = 1.46, \qquad I = \lceil P \rceil = \lceil 1.46 \rceil = 2
$$

**Compute $X$:**

$$
X = X_{(I)} + (P - I + 1)\big(X_{(I+1)} - X_{(I)}\big)
$$

$$
= X_{(2)} + (1.46 - 2 + 1)\big(X_{(3)} - X_{(2)}\big)
$$

$$
= 4 + (0.46)(6-4)
$$

$$
= 4.92 \qquad \blacksquare
$$

---

> [!summary] Big Picture
> This method lets us simulate a **continuous-valued** $X$ directly from raw data $X_1, \ldots, X_n$, without assuming any particular parametric distribution (Normal, Exponential, etc.). It's essentially inverse transform sampling applied to a **piecewise-linear approximation** of the unknown true c.d.f. $F(x)$, built by connecting the empirical c.d.f.'s jump points with straight lines.


### Check: Verifying the Same Example a Different Way

For the concrete case $n=3$ with $X_{(1)}=1$, $X_{(2)}=4$, $X_{(3)}=6$, we can write out $F(x)$ **explicitly** piece-by-piece and invert directly, as a check on the general formula from before.

---

**Explicit piecewise c.d.f.** (plugging $n=3$ into the general linear-interpolation formula):

$$
F(x) =
\begin{cases}
0 + \dfrac{x-1}{2(4-1)} & \text{if } 1 \leq x < 4 \quad (i=1 \text{ case}) \\[10pt]
\dfrac{1}{2} + \dfrac{x-4}{2(6-4)} & \text{if } 4 \leq x < 6 \quad (i=2 \text{ case})
\end{cases}
$$

---

**Solving $F(X) = U$ on each piece:**

*Case $i=1$ ($1 \leq X < 4$, corresponding to $U < 1/2$):*

$$
\frac{X-1}{6} = U \quad \Longrightarrow \quad X = 1 + 6U
$$

*Case $i=2$ ($4 \leq X < 6$, corresponding to $U \geq 1/2$):*

$$
\frac{1}{2} + \frac{X-4}{4} = U \quad \Longrightarrow \quad X = 2 + 4U
$$

**Putting it together:**

$$
X =
\begin{cases}
1 + 6U & \text{if } U < 1/2 \\
2 + 4U & \text{if } U \geq 1/2
\end{cases}
$$

---

**Numerical check:** If $U = 0.73$ (so $U \geq 1/2$, use second case):

$$
X = 2 + 4(0.73) = 4.92 \qquad \blacksquare
$$

---

> [!success] Consistency Check
> This matches **exactly** the result $X = 4.92$ obtained earlier using the general formula $X = X_{(I)} + (P-I+1)(X_{(I+1)}-X_{(I)})$. This confirms both approaches — the abstract "dice toss + uniform remainder" formula and the direct piecewise inversion — describe the **same underlying construction**, just packaged differently.

> [!tip] When to use which form
> - The **piecewise-by-hand** version (this note) is intuitive for small, fixed $n$ and good for double-checking by hand.
> - The **general formula** (previous note) is what you'd actually *code up*, since it works for arbitrary $n$ without having to rewrite cases each time.


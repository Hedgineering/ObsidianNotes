# Acceptance-Rejection (A-R) Method

> [!abstract] Core Idea **Motivation:** Most c.d.f.'s can't be inverted efficiently (or at all in closed form), so Inverse Transform isn't always an option. A-R instead **samples from a distribution that's "almost" the one we want**, and then **accepts only a certain proportion** of those samples — the ones that survive a random accept/reject test — so that what's left over has _exactly_ the target distribution.

---

## Baby Example: Generating $\mathcal{U}(\tfrac{2}{3}, 1)$

> You'd normally just use Inverse Transform for this — but it's a clean way to see A-R in action.

**Algorithm:**

1. Generate $U \sim \mathcal{U}(0,1)$.
2. If $U \ge \tfrac{2}{3}$, **ACCEPT** $X \leftarrow U$. Otherwise, **REJECT** and go back to Step 1.

**Intuition:** We're drawing from an "easy" distribution (Uniform(0,1)) that _covers_ our target range, and only keeping the draws that fall in the target region $[\tfrac23, 1]$. Conditioned on acceptance, the kept values are exactly $\mathcal{U}(\tfrac23,1)$.

---

## General Setup & Notation

Suppose we want to simulate a continuous RV $X$ with p.d.f. $f(x)$, but $f$ is **hard to sample directly**.

Suppose instead we can **easily generate** a RV with p.d.f. $$h(x) \equiv \frac{t(x)}{c}$$ where $t(x)$ **majorizes** $f(x)$, ($t$ is always bigger than $f$ for all the same inputs $x$) meaning: $$t(x) \ge f(x), \quad \forall x \in \mathbb{R}$$

and normalizing constant $c$ is the area under $t$ (which may sum to greater than 1):
$$c \equiv \int_{-\infty}^{\infty} t(x),dx ;\ge; \int_{-\infty}^{\infty} f(x),dx = 1,$$ with the requirement that $c < \infty$.

You can think of this as generating some random variable $Y$ uniformly under $t(x)$ and accept that point with probability $f(Y) / t(Y) = f(Y) / ( c \cdot h(Y) )$ -- **so it's more likely to accept a generated point if the PDF value is similar to the actual PDF $f$ .**

> [!info] Interpretation
> 
> - $f(x)$ = target density (hard to sample)
> - $t(x)$ = a "majorizing"/envelope function that sits on or above $f(x)$ everywhere
> - $h(x) = t(x)/c$ = a proper density (easy to sample from) proportional to $t(x)$
> - $c$ = the "inflation factor" — how much bigger the envelope's total area is than 1. **Smaller $c$ = more efficient algorithm** (fewer rejections).

---

## Theorem (von Neumann, 1951)

Define $$g(x) \equiv \frac{f(x)}{t(x)}, \qquad 0 \le g(x) \le 1 \ \ \text{for all } x.$$

Let $U \sim \mathcal{U}(0,1)$, and let $Y$ be a RV (independent of $U$) with p.d.f. $h(y) = t(y)/c$.

> [!success] Key Result **If $U \le g(Y)$, then $Y$ has (conditional) p.d.f. $f(y)$.** In other words: draw an "easy" candidate $Y$ from $h$, then accept it with probability $g(Y)$ — and the accepted values are distributed exactly as the hard target $f$.

---

## Algorithm A-R

```
Repeat
    Generate U from U(0,1)
    Generate Y from h(y)              (independent of U)
until U ≤ g(Y) = f(Y)/t(Y) = f(Y)/(c·h(Y))     ← Acceptance event

Return X ← Y
```

- **Candidate step:** draw $Y \sim h$ (easy distribution).
- **Test step:** draw independent $U \sim \mathcal{U}(0,1)$; compute $g(Y) = f(Y)/t(Y)$.
- **Accept** if $U \le g(Y)$ → return $X \leftarrow Y$.
- **Reject** otherwise → discard and repeat both draws.

---

## Why It Works (Intuition)

- $t(x)$ forms an "umbrella" over $f(x)$; sampling $Y$ from $h(y) = t(y)/c$ is like throwing points at the region under the umbrella.
- The acceptance test $U \le f(Y)/t(Y)$ effectively **thins out** the points so that, at each $x$, the surviving density is proportional to $f(x)$ rather than $t(x)$.
- Geometrically: imagine generating uniform random points **under the curve $t(x)$**; keeping only those that also fall **under the curve $f(x)$** gives points uniformly distributed under $f(x)$ — exactly what you need to have density $f$.

---
### Example (Law 2015): A-R for a Beta-like Density

> [!abstract] Setup
> Generate a RV with p.d.f.
> $$f(x) = 60x^3(1-x)^2, \quad 0 \le x \le 1$$
> This is a Beta$(4,3)$-type density. Its c.d.f. **can't be inverted analytically**, so Inverse Transform is off the table — a perfect candidate for Acceptance-Rejection.

---

#### Step 1 — Find the Max of $f(x)$

The maximum occurs at $x = 0.6$, giving
$$f(0.6) = 2.0736$$

#### Step 2 — Choose a Majorizing Function $t(x)$

Simplest possible choice: a **flat, constant majorizer** equal to the peak height:
$$t(x) = 2.0736 \quad \text{for } 0 \le x \le 1$$

> [!warning] Inefficient by design
> This is explicitly called out as an **inefficient** choice — a flat line at the max height "wastes" a lot of area between $t(x)$ and $f(x)$ near the tails (where $f$ is small). It's chosen here for simplicity/illustration, not for speed.

#### Step 3 — Compute $c$

$$c = \int_0^1 t(x)\, dx = 2.0736$$

(since $t(x)$ is constant over $[0,1]$, the integral is just the constant itself).

#### Step 4 — Derive $h(x)$

$$h(x) = \frac{t(x)}{c} = \frac{2.0736}{2.0736} = 1$$

So $h(x) = 1$ on $[0,1]$ — i.e., $h$ is just the **Uniform(0,1)** p.d.f. This is the payoff of picking a constant $t(x)$: sampling from $h$ becomes trivially easy (just draw $Y \sim \mathcal{U}(0,1)$).

#### Step 5 — Derive $g(x)$

$$g(x) = \frac{f(x)}{t(x)} = \frac{60x^3(1-x)^2}{2.0736}$$

This is the acceptance probability function, evaluated at each candidate $Y$.

---

#### Step 6 — Run the Algorithm

1. Generate $U \sim \mathcal{U}(0,1)$.
2. Generate $Y \sim h(y) = \mathcal{U}(0,1)$ (independent of $U$).
3. Accept if $U \le g(Y)$; else reject and repeat.

**Worked example:** Suppose $U = 0.13$ and $Y = 0.25$.

$$g(0.25) = \frac{60(0.25)^3(1-0.25)^2}{2.0736} = \frac{60(0.015625)(0.5625)}{2.0736} \approx \frac{0.5273}{2.0736} \approx 0.2543$$

Since $U = 0.13 \le g(Y) \approx 0.2543$ → **ACCEPT**, so $X \leftarrow 0.25$.

---

#### Visual Summary

- $f(x) = 60x^3(1-x)^2$ — the target density (green curve), peaking at $x=0.6$.
- $t(x) = 2.0736$ — flat red line sitting above $f(x)$ everywhere on $[0,1]$ (the majorizer).
- $h(x) = 1$ — flat purple line, the Uniform(0,1) density used to generate candidates $Y$.

> [!tip] Why this example matters
> It's a clean illustration of the **general A-R machinery** applied to a case where:
> - The target density's CDF has no closed-form inverse.
> - A trivial constant majorizer makes candidate generation as easy as possible (plain Uniform draws).
> - The trade-off is efficiency: this particular $t(x)$ is *not* tight against $f(x)$, so many candidates will be rejected — but it's simple to understand and implement.


## Efficiency Notes

> [!tip] Choosing a good $t(x)$
> 
> - The **acceptance probability** on each iteration is $1/c$ (since $c = \int t(x),dx$ and the "wasted area" above $f$ but under $t$ determines the rejection rate).
> - A **tight envelope** ($t(x)$ close to $f(x)$, i.e. $c$ close to 1) means fewer rejected draws and a faster algorithm.
> - A **loose envelope** ($c \gg 1$) wastes many draws — efficient A-R design is about picking $h$ (easy to sample) and $t$ (tight bound) simultaneously.
> - Expected number of iterations needed to get one accepted draw is $c$.

---
## Proof of the Acceptance-Rejection Method

> [!abstract] What We're Proving Given the Acceptance-Rejection Method setup — 
> 
> Target density $f$, envelope $t(x) \ge f(x)$, 
> easy-to-sample density $h(y) = t(y)/c$, 
> and $g(x) \equiv f(x)/t(x)$ 
> 
> — we prove that the procedure 
> _"generate $Y\sim h$, generate independent $U \sim \mathcal{U}(0,1)$, accept if $U \le g(Y)$, set $X = Y$ and stop"_ 
> produces an $X$ whose p.d.f. is **exactly** $f(x)$.

---

### Setup Recap

- $U \sim \mathcal{U}(0,1)$, independent of $Y$.
- $t(y)$ is some upper bound "majorizing" function that's always greater than the p.d.f. $f(x)$ of the RV we want to generate
- $Y$ has p.d.f. $h(y) = t(y)/c$.
	- We need to divide $t$ by the integral of $t$ to normalize it into something that has integral of 1 and can be treated as a p.d.f
- $g(x) \equiv f(x)/t(x)$, with $0 \le g(x) \le 1$.
	- Since $t$ is always greater than $f$, we can treat $g$ as the "probability of accepting RV $Y$ generated using $h(y) = t(y)/c$"
- **Acceptance event** $A$: ${U \le g(Y)}$.
	- As per above, you Accept with probability $g(y)$
- If $A$ occurs: set $X \leftarrow Y$ and stop.

**Goal:** show $P(X \le x) = \displaystyle\int_{-\infty}^x f(y) \quad dy$. given that we're using this acceptance-rejection method.

---

### Step 1 — Express the CDF of $X$ as a Conditional Probability

Since $X$ is only ever set equal to $Y$ **when acceptance happens**, the distribution of $X$ is exactly the _conditional_ distribution of $Y$ given acceptance:

$$ P(X \le x) = P(Y \le x \mid A) = \frac{P(A, Y \le x)}{P(A)} \tag{1} $$

This is just the definition of conditional probability — we need to work out both the numerator $P(A, Y\le x)$ and the denominator $P(A)$.

---

### Step 2 — Acceptance Probability Given $Y = y$

Condition on $Y = y$ and compute the chance of acceptance:

$$ \begin{aligned} 
P(A \mid Y = y) &= P(U \le g(Y) \mid Y=y) \\
                &= P(U \le g(y) \mid Y=y) \qquad \text{(substitute } Y=y\text{)}\\
                &= P(U \le g(y)) \qquad\qquad\quad \text{(} U \text{ and } Y \text{ are independent)} \\ 
			    &= g(y) \qquad\qquad\qquad\qquad
			    \text{(} U \text{ is Uniform(0,1), so } P(U\le u)=u\text{)} 
\end{aligned} \tag{2} 
$$

> [!note] Why this matters This says: _given a candidate value $y$, the chance we accept it is exactly $g(y) = f(y)/t(y)$_ — bigger when $f(y)$ is close to $t(y)$ (envelope is tight there), smaller when $t(y)$ overshoots $f(y)$ by a lot.

---

### Step 3 — Numerator: $P(A,\ Y \le x)$

Use the Law of Total Probability, integrating over all possible values of $Y$ (weighted by its density $h(y)$), restricted to $y \le x$:

$$ \begin{aligned} 

P(A,\ Y \le x) &= \int_{-\infty}^{\infty} P(A,\ Y \le x \mid Y = y) h(y)\quad dy \\ 
               &= \int_{-\infty}^{x} P(A \mid Y = y) h(y)\quad dy \\ 
               &= \frac{1}{c}\int_{-\infty}^{x} P(A \mid Y = y) t(y)\quad dy \qquad \left(h(y) = t(y)/c\right) \\ 
               &= \frac{1}{c}\int_{-\infty}^{x} g(y) t(y)\quad dy \qquad\qquad\ \ \text{(by (2))}\\ 
               &= \frac{1}{c}\int_{-\infty}^{x} f(y)\quad dy \qquad\qquad\qquad  \left(g(y)t(y) = f(y)\right) 
\end{aligned} 

\tag{3} 

$$

> [!info] Key simplification $g(y)\cdot t(y) = \dfrac{f(y)}{t(y)}\cdot t(y) = f(y)$ — the envelope $t(y)$ cancels out entirely, leaving just the **target density** $f(y)$ inside the integral. This is the crux of why A-R recovers $f$ exactly.

---

### Step 4 — Denominator: $P(A)$

Let $x \to \infty$ in (3):

$$ P(A) = \frac{1}{c}\int_{-\infty}^{\infty} f(y), dy = \frac{1}{c}(1) = \frac{1}{c} \tag{4} $$

(using the fact that $f$ is a valid p.d.f., so it integrates to 1).

> [!success] Interpretation The **overall acceptance probability** on any given trial is $1/c$ — matching the earlier claim that $c$ controls efficiency (smaller $c$ → higher acceptance rate).

---

### Step 5 — Combine Everything

Plug (3) and (4) into (1):

$$ P(X \le x) = \frac{P(A,\ Y\le x)}{P(A)} = \frac{\frac{1}{c}\int_{-\infty}^x f(y)\quad dy}{\frac{1}{c}} = \int_{-\infty}^x f(y)\quad dy $$

$$ \boxed{P(X \le x) = \int_{-\infty}^x f(y)\quad dy \ \ \Longrightarrow\ \ X \text{ has p.d.f. } f(x).} \qquad \blacksquare $$

---

### Practical Implications of the Proof

> [!warning] Two main issues in practice The proof shows A-R is _exact_, but two practical requirements fall directly out of it:
> 
> 1. **Need to sample from $h(y)$ quickly.** The whole method hinges on $h$ being an "easy" distribution to draw from — if generating $Y$ is itself slow, A-R gains nothing.
> 2. **$c$ must be small** — i.e., $t(x)$ must be "close" to $f(x)$. From (4), the acceptance probability is $P(A) = 1/c$, and the **number of trials until a success** (acceptance) is $\text{Geometric}(1/c)$. This means: $$\text{Mean number of trials} = c$$ So a large $c$ (loose envelope) directly translates into wasted computation — many rejected draws for every accepted one.

---
## Related

- [[Convolution Method For Random Variable Generation]]
- [[Inverse Transform Theorem]]
- [[Random Variate Generation - Overview]]
#statistics #estimation #method-of-moments

# The Method of Moments

## Moments of a Random Variable

The **$k$th moment** of a random variable $X$ is

$$ \mathrm{E}[X^k] = \begin{cases} \sum_x x^k f(x) & \text{if } X \text{ is discrete} \[6pt] \int_{\mathbb{R}} x^k f(x),dx & \text{if } X \text{ is continuous} \end{cases} $$

## Definition: MOM Estimator

Suppose $X_1, \dots, X_n$ are i.i.d. from p.m.f./p.d.f. $f(x)$.

The **method of moments (MOM) estimator** for $\mathrm{E}[X^k]$ is the corresponding _sample_ moment:

$$ m_k \equiv \frac{1}{n}\sum_{i=1}^n X_i^k $$

> **Core idea:** replace population (theoretical) moments with sample moments, then solve for the parameter(s) of interest.

---

## Basic Examples

- MOM estimator for $\mu = \mathrm{E}[X_i]$: $$ m_1 = \bar X = \frac{1}{n}\sum_{i=1}^n X_i $$
    
- MOM estimator for $\mathrm{E}[X_i^2]$: $$ m_2 = \frac{1}{n}\sum_{i=1}^n X_i^2 $$
    
- MOM estimator for $\mathrm{Var}(X_i) = \mathrm{E}[X_i^2] - (\mathrm{E}[X_i])^2$: $$ m_2 - m_1^2 = \frac{1}{n}\sum_{i=1}^n X_i^2 - \bar X^2 = \frac{n-1}{n}S^2 $$
    

(It's also fine to just use $S^2$ instead of $\frac{n-1}{n}S^2$.)

---

## Worked Example: Poisson

Suppose $X_1, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Pois}(\lambda)$.

- Since $\lambda = \mathrm{E}[X_i]$, a MOM estimator is $\hat\lambda = \bar X$.
- But also $\lambda = \mathrm{Var}(X_i)$, so **another** valid MOM estimator is $\frac{n-1}{n}S^2$ (or plain $S^2$).

> **Rule of thumb:** when multiple MOM estimators exist for the same parameter, usually use the **easier-looking** one if you have a choice.

This illustrates that MOM estimators are **not unique** — different moment equations can lead to different (but all valid) estimators.

---

## Worked Example: Normal

Suppose $X_1, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Nor}(\mu, \sigma^2)$.

- MOM estimator for $\mu$: $\hat\mu = \bar X$
- MOM estimator for $\sigma^2$: $\frac{n-1}{n}S^2$ (or $S^2$)

> For the Normal distribution, these MOM estimators coincide with the **MLEs**.

---

## Worked Example: Beta Distribution

Suppose $X_1, \dots, X_n \overset{\text{iid}}{\sim} \mathrm{Beta}(a,b)$ with p.d.f.

$$ f(x) = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}x^{a-1}(1-x)^{b-1}, \qquad 0 < x < 1 $$

Known moment relationships:

$$ \mathrm{E}[X] = \frac{a}{a+b} \qquad\qquad \mathrm{Var}(X) = \frac{ab}{(a+b)^2(a+b+1)} = \frac{\mathrm{E}[X] b}{(a+b)(a+b+1)} $$

### Step 1 — Solve for $a$ in terms of $b$ and $\mathrm{E}[X]$

$$ \mathrm{E}[X] = \frac{a}{a+b} \Longrightarrow a = \frac{b\mathrm{E}[X]}{1 - \mathrm{E}[X]} \approx \frac{b\bar X}{1-\bar X} \tag{$*$} $$

### Step 2 — Plug into the variance equation

Substitute:

- $\bar X$ for $\mathrm{E}[X]$
- $S^2$ for $\mathrm{Var}(X)$
- $\dfrac{b\bar X}{1-\bar X}$ for $a$

into the $\mathrm{Var}(X)$ formula. After algebra, solve for $b$:

$$ b \approx \frac{(1-\bar X)^2 \bar X}{S^2} - 1 + \bar X $$

### Step 3 — Back-substitute

Plug this $b$ back into $(*)$ to get the MOM estimator for $a$.

> **Pattern:** with two parameters, use the first two sample moments ($\bar X$, $S^2$) to set up two equations, then solve the system.

---

## Numerical Example (Hayter)

Given data from a Beta distribution with:

$$ \bar X = 0.3007, \qquad S^2 = 0.01966 $$

The MOM estimators come out to:

$$ \hat a \approx 2.92, \qquad \hat b \approx 6.78 $$

---

## Summary / Key Takeaways

- **General recipe:**
    1. Write the population moments ($\mathrm{E}[X]$, $\mathrm{E}[X^2]$, etc.) in terms of the unknown parameter(s).
    2. Set population moments equal to the corresponding sample moments ($m_1, m_2, \dots$).
    3. Solve the resulting system of equations for the parameter(s).
- **Not unique**: several different moment equations can estimate the same parameter (e.g., Poisson $\lambda$ via mean or variance) — pick whichever is algebraically simplest.
- For **Normal** distributions, MOM estimators = MLEs.
- For distributions with more than one parameter (e.g. **Beta**), you generally need as many moment equations as there are parameters, and the algebra can get messy.



# Variance of a Uniform Random Variable

> [!abstract] Result 
> 
> For $U \sim \text{Uniform}(0,1)$: $\quad \text{Var}(U) = \dfrac{1}{12}$ 
> 
> For $U \sim \text{Uniform}(a,b)$: $\quad \text{Var}(U) = \dfrac{(b-a)^2}{12}$

Both follow from the identity $$\text{Var}(X) = E[X^2] - \big(E[X]\big)^2.$$

---

## Specific Case: $\mathcal{U}(0,1)$

Density: $f(x) = 1$ for $x \in [0,1]$.

**Step 1 — Mean:** $$E[U] = \int_0^1 x \cdot 1 , dx = \left.\frac{x^2}{2}\right|_0^1 = \frac{1}{2}$$

**Step 2 — Second moment:** $$E[U^2] = \int_0^1 x^2 , dx = \left.\frac{x^3}{3}\right|_0^1 = \frac{1}{3}$$

**Step 3 — Variance:** $$\text{Var}(U) = E[U^2] - \big(E[U]\big)^2 = \frac{1}{3} - \left(\frac{1}{2}\right)^2 = \frac{1}{3} - \frac{1}{4} = \frac{1}{12}$$

---

## General Case: $\mathcal{U}(a,b)$

Density: $f(x) = \dfrac{1}{b-a}$ for $x \in [a,b]$.

**Step 1 — Mean:** $$E[U] = \int_a^b x \cdot \frac{1}{b-a}, dx = \frac{1}{b-a}\left.\frac{x^2}{2}\right|_a^b = \frac{b^2 - a^2}{2(b-a)} = \frac{a+b}{2}$$

(using $b^2 - a^2 = (b-a)(b+a)$)

**Step 2 — Second moment:** $$E[U^2] = \int_a^b x^2 \cdot \frac{1}{b-a}, dx = \frac{1}{b-a}\left.\frac{x^3}{3}\right|_a^b = \frac{b^3 - a^3}{3(b-a)}$$

Using $b^3 - a^3 = (b-a)(b^2+ab+a^2)$: $$E[U^2] = \frac{a^2 + ab + b^2}{3}$$

**Step 3 — Variance:** $$ \text{Var}(U) = \frac{a^2+ab+b^2}{3} - \left(\frac{a+b}{2}\right)^2 $$

Expand $\left(\dfrac{a+b}{2}\right)^2 = \dfrac{a^2+2ab+b^2}{4}$, then combine over a common denominator of 12:

$$ \text{Var}(U) = \frac{4(a^2+ab+b^2) - 3(a^2+2ab+b^2)}{12} = \frac{a^2 - 2ab + b^2}{12} = \frac{(b-a)^2}{12} $$

$$\boxed{\text{Var}(U) = \frac{(b-a)^2}{12}}$$

Setting $a=0, b=1$ recovers the specific case: $\dfrac{(1-0)^2}{12} = \dfrac{1}{12}$. ✓

---

## Why This Matters (Connection to Convolution)

> [!tip] Application In the [[Convolution Method#Crude "Desert Island" Normal Generator (Convolution + CLT)]] example, summing $n$ i.i.d. $\mathcal{U}(0,1)$ variables gives variance $n \cdot \dfrac{1}{12} = \dfrac{n}{12}$ (variances add for independent RVs). Choosing $n = 12$ makes this variance exactly $1$ — which is _why_ $n=12$ was picked, not because 12 is "large" in any CLT sense.

---

## Related

- [[Convolution Method#Crude "Desert Island" Normal Generator (Convolution + CLT)]]
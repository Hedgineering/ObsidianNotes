# Box-Muller Method

A nice, easy way to generate standard normals — no inverse c.d.f. of the normal needed (which has no closed form).

## Theorem

If $U_1, U_2$ are i.i.d. $\mathcal{U}(0,1)$, then

$$ Z_1 = \sqrt{-2\ln(U_1)} \cos(2\pi U_2) $$

$$ Z_2 = \sqrt{-2\ln(U_1)} \sin(2\pi U_2) $$

are i.i.d. $\text{Nor}(0,1)$.

> [!warning] The trig calculations ($\cos$, $\sin$) must be done in **radians**. This is an easy source of bugs when implementing in software that defaults to degrees.

---

## Corollaries

These fall out directly from the Box-Muller transformation.

### Corollary 1: Sum of squares gives a Chi-Square(2), which is Exponential

We know $Z_1^2 + Z_2^2 \sim \chi^2(1) + \chi^2(1) \sim \chi^2(2)$.

But directly from the formulas:

$$ Z_1^2 + Z_2^2 = -2\ln(U_1)\big(\cos^2(2\pi U_2) + \sin^2(2\pi U_2)\big) $$

$$ = -2\ln(U_1) $$

$$ \sim \text{Exp}(1/2) $$

Thus we've just proven that

$$ \chi^2(2) \sim \text{Exp}(1/2). \qquad \blacksquare $$

### Corollary 2: Ratio of Standard Normals gives a Cauchy distribution

We know $Z_2/Z_1 \sim \text{Nor}(0,1)/\text{Nor}(0,1) \sim \text{Cauchy} \sim t(1)$.

Moreover, using the Box-Muller formulas directly, the $\sqrt{-2\ln(U_1)}$ terms cancel:

$$ Z_2/Z_1 = \frac{\sqrt{-2\ln(U_1)},\sin(2\pi U_2)}{\sqrt{-2\ln(U_1)},\cos(2\pi U_2)} = \tan(2\pi U_2) $$

Thus we've just proven that

$$ \tan(2\pi U) \sim \text{Cauchy} \qquad \text{(and, similarly, } \cot(2\pi U) \sim \text{Cauchy)} $$

Similarly,

$$ Z_2^2/Z_1^2 = \tan^2(2\pi U) \sim t^2(1) \sim F(1,1) $$

> [!tip] Why this matters These corollaries give you free, closed-form generators for the Cauchy and $F(1,1)$ distributions using only a single uniform $U$ — no need to separately derive their inverse c.d.f.'s from scratch.

---

## Polar Method (Marsaglia) — a faster alternative

The Polar Method avoids computing $\cos$ and $\sin$ directly (which are computationally expensive), making it a little faster than Box-Muller.

**Steps:**

1. Generate $U_1, U_2$ i.i.d. $\mathcal{U}(0,1)$.
    
    Let $V_i = 2U_i - 1$, $i = 1,2$, and $W = V_1^2 + V_2^2$.
    
2. If $W > 1$, **reject** and go back to Step 1.
    
    Otherwise, let $Y = \sqrt{-2\ln(W)/W}$, and accept $Z_i \leftarrow V_i Y$, $i = 1,2$.
    

Then $Z_1, Z_2$ are i.i.d. $\text{Nor}(0,1)$.

> [!info] Intuition $(V_1, V_2)$ is a uniformly random point in the square $[-1,1]^2$. Rejecting when $W > 1$ restricts to points uniformly distributed inside the **unit circle** — this replaces the explicit angle-and-radius construction of Box-Muller with an implicit one, avoiding trig function calls entirely.

---

## Proof That Box-Muller Works

**Goal:** show that $(Z_1, Z_2)$ as defined by the Box-Muller transformation are independent, each $\text{Nor}(0,1)$.

**Setup.** Let $R^2 = -2\ln(U_1)$ and $\Theta = 2\pi U_2$, so that

$$ Z_1 = R\cos(\Theta), \qquad Z_2 = R\sin(\Theta) $$

This is exactly the polar-coordinate representation of a point $(Z_1, Z_2)$ in the plane, with radius $R \ge 0$ and angle $\Theta \in [0, 2\pi)$.

**Step 1 — distribution of $\Theta$.** Since $U_2 \sim \mathcal{U}(0,1)$, the transformation $\Theta = 2\pi U_2$ gives $\Theta \sim \mathcal{U}(0, 2\pi)$ — a uniformly random angle around the circle.

**Step 2 — distribution of $R$.** Since $U_1 \sim \mathcal{U}(0,1)$, we have $-\ln(U_1) \sim \text{Exp}(1)$ (standard inverse-transform fact), so $R^2 = -2\ln(U_1) \sim \text{Exp}(1/2)$.

The density of $R^2$ is $f_{R^2}(r^2) = \tfrac12 e^{-r^2/2}$. Changing variables from $R^2$ to $R$ via $r^2 \to r$ (Jacobian $2r$) gives the density of $R$:

$$ f_R(r) = r,e^{-r^2/2}, \qquad r > 0 $$

This is exactly the **Rayleigh distribution** — the known distribution of the radius of a bivariate standard normal vector.

**Step 3 — joint density of $(R, \Theta)$.** Since $U_1$ and $U_2$ are independent, $R$ and $\Theta$ are independent, so their joint density is the product:

$$ f_{R,\Theta}(r,\theta) = f_R(r), f_\Theta(\theta) = r,e^{-r^2/2} \cdot \frac{1}{2\pi}, \qquad r > 0,; 0 \le \theta < 2\pi $$

**Step 4 — transform to $(Z_1, Z_2)$.** Apply the change of variables $z_1 = r\cos\theta$, $z_2 = r\sin\theta$. The Jacobian of the polar-to-Cartesian transformation has absolute value $r$, so:

$$ f_{Z_1,Z_2}(z_1,z_2) = f_{R,\Theta}(r,\theta) \cdot \frac{1}{r} = \frac{1}{2\pi},e^{-r^2/2} = \frac{1}{2\pi},e^{-(z_1^2+z_2^2)/2} $$

**Step 5 — factor into independent standard normals.** This joint density factors as:

$$ f_{Z_1,Z_2}(z_1,z_2) = \left(\frac{1}{\sqrt{2\pi}}e^{-z_1^2/2}\right)\left(\frac{1}{\sqrt{2\pi}}e^{-z_2^2/2}\right) $$

which is exactly the product of two independent standard normal densities.

Therefore $Z_1, Z_2$ are i.i.d. $\text{Nor}(0,1)$. $\qquad \blacksquare$
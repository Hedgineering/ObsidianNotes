# Multivariate Normal Random Variable Generation

## Bivariate Normal Distribution

The random vector $(X, Y)$ has the **bivariate normal distribution** with means $\mu_X = E[X]$ and $\mu_Y = E[Y]$, variances $\sigma_X^2 = \text{Var}(X)$ and $\sigma_Y^2 = \text{Var}(Y)$, and correlation $\rho = \text{Corr}(X,Y)$ if it has joint p.d.f.

$$
f(x,y) = \frac{1}{2\pi\sigma_X\sigma_Y\sqrt{1-\rho^2}} \exp\left\{ \frac{-\left[z_X^2(x) + z_Y^2(y) - 2\rho z_X(x) z_Y(y)\right]}{2(1-\rho^2)} \right\}
$$
where $z_X(x) \equiv (x-\mu_X)/\sigma_X$ and $z_Y(y) \equiv (y - \mu_Y)/\sigma_Y$.

> [!example] Heights and weights of people can be modeled as bivariate normal.

## Multivariate Normal Distribution

The random vector $\boldsymbol{X} = (X_1, \ldots, X_k)^{\mathrm{T}}$ has the **multivariate normal distribution** with mean vector $\boldsymbol{\mu} = (\mu_1, \ldots, \mu_k)^{\mathrm{T}}$ and $k \times k$ covariance matrix $\Sigma = (\sigma_{ij})$ if it has p.d.f.

$$
f(\boldsymbol{x}) = \frac{1}{(2\pi)^{k/2}|\Sigma|^{1/2}} \exp\left\{ -\frac{(\boldsymbol{x}-\boldsymbol{\mu})^{\mathrm{T}}\Sigma^{-1}(\boldsymbol{x}-\boldsymbol{\mu})}{2} \right\}, \qquad \boldsymbol{x} \in \mathbb{R}^k
$$
with

$$ E[X_i] = \mu_i, \qquad \text{Var}(X_i) = \sigma_{ii}, \qquad \text{Cov}(X_i, X_j) = \sigma_{ij} $$

**Notation:** $\boldsymbol{X} \sim \text{Nor}_k(\boldsymbol{\mu}, \Sigma)$.

---

## Generating X via Cholesky Decomposition

To generate $\boldsymbol{X}$, start with a vector $\boldsymbol{Z} = (Z_1, \ldots, Z_k)$ of i.i.d. $\text{Nor}(0,1)$ RVs. That is, suppose $\boldsymbol{Z} \sim \text{Nor}_k(\boldsymbol{0}, I)$, where $I$ is the $k \times k$ identity matrix, and $\boldsymbol{0}$ is simply a vector of $0$'s.

Suppose we can find the (lower triangular) **Cholesky matrix** $C$ such that $\Sigma = CC^{\mathrm{T}}$.

Then it can be shown that $\boldsymbol{X} = \boldsymbol{\mu} + C\boldsymbol{Z}$ is multivariate normal with mean $\boldsymbol{\mu}$ and covariance matrix

$$ \Sigma \equiv E[(C\boldsymbol{Z})(C\boldsymbol{Z})^{\mathrm{T}}] = E[C\boldsymbol{Z}\boldsymbol{Z}^{\mathrm{T}}C^{\mathrm{T}}] = C\left(E[\boldsymbol{Z}\boldsymbol{Z}^{\mathrm{T}}]\right)C^{\mathrm{T}} = CC^{\mathrm{T}} $$

> [!note] Why this works Since $E[\boldsymbol{Z}\boldsymbol{Z}^{\mathrm{T}}] = I$ (the $Z_i$'s are independent, unit-variance, zero-mean), the covariance of $\boldsymbol{X} = \boldsymbol{\mu} + C\boldsymbol{Z}$ collapses exactly to $CC^{\mathrm{T}} = \Sigma$, which is the target covariance matrix by construction.

## Case k = 2

For $k=2$, we can easily derive:

$$ C = \begin{pmatrix} \sqrt{\sigma_{11}} & 0 \\ \dfrac{\sigma_{12}}{\sqrt{\sigma_{11}}} & \sqrt{\sigma_{22} - \dfrac{\sigma_{12}^2}{\sigma_{11}}} \end{pmatrix} $$

Since $\boldsymbol{X} = \boldsymbol{\mu} + C\boldsymbol{Z}$, we have:

$$ X_1 = \mu_1 + \sqrt{\sigma_{11}}, Z_1 $$

$$ X_2 = \mu_2 + \frac{\sigma_{12}}{\sqrt{\sigma_{11}}} Z_1 + \sqrt{\sigma_{22} - \frac{\sigma_{12}^2}{\sigma_{11}}}, Z_2 $$

## General Algorithm for Computing C (dimension k)

For $i = 1, \ldots, k$:
	For $j = 1, \ldots, i-1$:

$$ c_{ij} \leftarrow \left(\sigma_{ij} - \sum_{\ell=1}^{j-1} c_{i\ell}c_{j\ell}\right) \Big/ c_{jj} $$

$$ c_{ji} \leftarrow 0 $$
For $i = 1, \ldots, k$:
$$ c_{ii} \leftarrow \left(\sigma_{ii} - \sum_{\ell=1}^{i-1} c_{i\ell}^2\right)^{1/2} $$

## Generating X Once C Is Known

Once $C$ has been computed, the multivariate normal RV $\boldsymbol{X} = \boldsymbol{\mu} + C\boldsymbol{Z}$ can easily be generated:

1. Generate $Z_1, Z_2, \ldots, Z_k \sim$ i.i.d. $\text{Nor}(0,1)$.
2. Let $X_i \leftarrow \mu_i + \sum_{j=1}^{i} c_{ij} Z_j$, for $i = 1, 2, \ldots, k$.

> [!tip] Because $C$ is lower triangular, the sum for $X_i$ only involves $Z_1, \ldots, Z_i$ — each new component only depends on standard normals up to its own index.

---

## Python Implementation

### Vanilla Python (no external libraries)

```python
import math
import random

def cholesky(sigma):
    """
    Compute the lower-triangular Cholesky factor C such that
    sigma = C @ C^T, using the algorithm from the notes.
    sigma: k x k covariance matrix, given as a list of lists.
    Returns C as a list of lists (k x k, lower triangular).
    """
    k = len(sigma)
    C = [[0.0] * k for _ in range(k)]

    for i in range(k):
        for j in range(i):  # j = 0, ..., i-1
            s = sum(C[i][l] * C[j][l] for l in range(j))
            C[i][j] = (sigma[i][j] - s) / C[j][j]
        # diagonal term c_ii
        s = sum(C[i][l] ** 2 for l in range(i))
        C[i][i] = math.sqrt(sigma[i][i] - s)

    return C


def generate_mvn(mu, sigma, rng=random):
    """
    Generate one draw from Nor_k(mu, sigma) using the Cholesky method.
    mu: list of length k (mean vector)
    sigma: k x k covariance matrix (list of lists)
    rng: random module or Random() instance (for reproducibility)
    Returns: list of length k, one multivariate normal draw.
    """
    k = len(mu)
    C = cholesky(sigma)

    # Step 1: generate k i.i.d. standard normals
    Z = [rng.gauss(0.0, 1.0) for _ in range(k)]

    # Step 2: X_i = mu_i + sum_{j=1}^{i} c_ij * Z_j
    X = []
    for i in range(k):
        xi = mu[i] + sum(C[i][j] * Z[j] for j in range(i + 1))
        X.append(xi)

    return X


if __name__ == "__main__":
    mu = [5.0, 10.0]
    sigma = [
        [4.0, 2.0],
        [2.0, 3.0],
    ]

    random.seed(42)
    sample = generate_mvn(mu, sigma)
    print("Generated X:", sample)
```

### NumPy Implementation

```python
import numpy as np

def generate_mvn_numpy(mu, sigma, size=1, rng=None):
    """
    Generate draws from Nor_k(mu, sigma) using the Cholesky method.

    mu: array-like, shape (k,)      -- mean vector
    sigma: array-like, shape (k,k)  -- covariance matrix
    size: number of samples to generate
    rng: optional np.random.Generator for reproducibility

    Returns: ndarray of shape (size, k) if size > 1,
             or shape (k,) if size == 1.
    """
    mu = np.asarray(mu, dtype=float)
    sigma = np.asarray(sigma, dtype=float)
    k = mu.shape[0]

    if rng is None:
        rng = np.random.default_rng()

    # Cholesky factor: sigma = C @ C.T
    C = np.linalg.cholesky(sigma)

    # Step 1: generate size x k i.i.d. standard normals
    Z = rng.standard_normal(size=(size, k))

    # Step 2: X = mu + C @ Z  (applied per-row)
    X = mu + Z @ C.T

    return X[0] if size == 1 else X


if __name__ == "__main__":
    mu = np.array([5.0, 10.0])
    sigma = np.array([
        [4.0, 2.0],
        [2.0, 3.0],
    ])

    rng = np.random.default_rng(42)
    sample = generate_mvn_numpy(mu, sigma, size=1, rng=rng)
    print("Generated X:", sample)

    # Sanity check: empirical covariance should approach sigma as size grows
    big_sample = generate_mvn_numpy(mu, sigma, size=100_000, rng=rng)
    print("Empirical mean:", big_sample.mean(axis=0))
    print("Empirical cov:\n", np.cov(big_sample.T))
```

> [!info] Note on `numpy` `np.linalg.cholesky` directly implements the same lower-triangular decomposition $\Sigma = CC^{\mathrm{T}}$ derived above, so the NumPy version skips the manual Cholesky algorithm and just calls it — useful for confirming the manual implementation is correct.

## Related

- [[Box-Muller Standard Normal Random Variable Generation]]
- [[Composition of Random Variables]]
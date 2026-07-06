Here's a better portable solution to generate Nor(0,1)'s: The following approximation has absolute error $\leq 0.45 \times 10^{-3}$:

$$
Z = \text{sign}(U - 1/2)\left(t - \frac{c_0 + c_1 t + c_2 t^2}{1 + d_1 t + d_2 t^2 + d_3 t^3}\right),
$$

where $\text{sign}(x) = 1, 0, -1$ if $x$ is positive, zero, or negative, respectively,

$$
t = \left\{-\ln\left[\min(U, 1-U)\right]^2\right\}^{1/2},
$$

and

$$
c_0 = 2.515517, \quad c_1 = 0.802853, \quad c_2 = 0.010328,
$$

$$
d_1 = 1.432788, \quad d_2 = 0.189269, \quad d_3 = 0.001308.
$$

#### If you want to generalize to Norm($\mu, \sigma^2$)
You can use the above approximation if you want to sample $Z \sim \text{Nor}(0,1)$ , then if you want $X \sim \text{Nor}(\mu, \sigma^2)$, just take

$$
X \leftarrow \mu + \sigma Z.
$$

**Easy Example (Inverse Transform):** Suppose you want to generate $X \sim \text{Nor}(3, 16)$, and you start with $U = 0.59$. Then

$$
X = \mu + \sigma Z = 3 + 4\Phi^{-1}(0.59) = 3 + 4(0.2275) = 3.91. \qquad \square
$$

### Sample MATLAB Usage to Generate Normal RV Samples
```matlab
m = 100000;
X = [];

for i=1:m
	% rand gives you random number in [0,1)
	% norminv gives you inverse normal cdf function 
	%     (given some cumulative probability return 
	%      corresponding z-score from standard normal)
	X(i) = norminv(rand, 0, 1); 
	i = i+1;
end
histogram(X)
```


### Sample Python Implementation

#### Standard math library
```py
import math

def norm_inv_odeh_evans(U):
    """
    Approximate the inverse standard normal CDF (quantile function)
    using the Odeh & Evans (1974) rational approximation.
    Absolute error <= 0.45e-3.

    Parameters
    ----------
    U : float
        Uniform(0,1) random variate, 0 < U < 1.

    Returns
    -------
    float
        Approximate Z ~ Nor(0,1) such that Phi(Z) = U.
    """
    c0, c1, c2 = 2.515517, 0.802853, 0.010328
    d1, d2, d3 = 1.432788, 0.189269, 0.001308

    if U <= 0 or U >= 1:
        raise ValueError("U must be strictly between 0 and 1")

    # sign(U - 1/2)
    if U > 0.5:
        s = 1
    elif U < 0.5:
        s = -1
    else:
        s = 0

    m = min(U, 1 - U)

    # Standard Odeh & Evans form: t = sqrt(-2 ln(m))
    t = math.sqrt(-2.0 * math.log(m))

    # --- Literal transcription from the image (kept for reference) ---
    # t = math.sqrt((-math.log(m)) ** 2)   # reduces to -log(m); included for parity

    numerator = c0 + c1 * t + c2 * t**2
    denominator = 1 + d1 * t + d2 * t**2 + d3 * t**3

    Z = s * (t - numerator / denominator)
    return Z


# --- quick sanity check against known quantiles ---
if __name__ == "__main__":
    test_values = [0.5, 0.975, 0.025, 0.9, 0.1, 0.99, 0.01]
    for U in test_values:
        print(f"U={U:>6}: Z = {norm_inv_odeh_evans(U): .6f}")
```

#### Numpy

```py
import numpy as np

def norm_inv_odeh_evans_np(U):
    """
    Vectorized approximation of the inverse standard normal CDF
    using the Odeh & Evans (1974) rational approximation.
    Absolute error <= 0.45e-3.

    Parameters
    ----------
    U : array_like
        Uniform(0,1) values, each strictly between 0 and 1.

    Returns
    -------
    numpy.ndarray
        Approximate Z ~ Nor(0,1) values such that Phi(Z) = U.
    """
    U = np.asarray(U, dtype=np.float64)

    if np.any((U <= 0) | (U >= 1)):
        raise ValueError("All values of U must be strictly between 0 and 1")

    c0, c1, c2 = 2.515517, 0.802853, 0.010328
    d1, d2, d3 = 1.432788, 0.189269, 0.001308

    s = np.sign(U - 0.5)                 # sign(U - 1/2), vectorized
    m = np.minimum(U, 1 - U)
    t = np.sqrt(-2.0 * np.log(m))        # standard Odeh & Evans form

    numerator = c0 + c1 * t + c2 * t**2
    denominator = 1 + d1 * t + d2 * t**2 + d3 * t**3

    Z = s * (t - numerator / denominator)
    return Z


if __name__ == "__main__":
    # Generate 1 million standard normals from uniforms
    rng = np.random.default_rng(42)
    U = rng.uniform(0, 1, size=1_000_000)
    Z = norm_inv_odeh_evans_np(U)

    print("Sample mean:", Z.mean())      # should be ~0
    print("Sample std :", Z.std())       # should be ~1

    # Spot-check known quantiles
    test_U = np.array([0.5, 0.975, 0.025, 0.9, 0.1, 0.99, 0.01])
    print(norm_inv_odeh_evans_np(test_U))
```
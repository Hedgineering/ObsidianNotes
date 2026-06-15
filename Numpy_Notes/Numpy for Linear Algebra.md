
Linear algebra is one of NumPy's biggest strengths. Almost every machine learning, statistics, simulation, graphics, optimization, and scientific computing problem eventually becomes matrix operations.

---

# 1. Core Data Structures

## Scalars (0D)

```python
x = np.array(5)

print(x)
# 5
```

Shape:

```python
x.shape
# ()
```

---

## Vectors (1D)

```python
v = np.array([1, 2, 3])
```

Shape:

```python
v.shape
# (3,)
```

Think:

$$  
v =  
\begin{bmatrix}  
1 & 2 & 3  
\end{bmatrix}  
$$

NumPy does **not** distinguish row and column vectors for 1D arrays.

---

## Matrices (2D)

```python
A = np.array([
    [1,2],
    [3,4]
])
```

Shape:

```python
A.shape
# (2,2)
```

$$  
A=  
\begin{bmatrix}  
1 & 2 \  
3 & 4  
\end{bmatrix}  
$$

---

## Higher Dimensions

```python
X = np.random.rand(10, 5, 3)
```

Shape:

```python
(10,5,3)
```

Think:

- 10 matrices
    
- each 5×3
    

---

# 2. Matrix Creation

## Identity Matrix

```python
I = np.eye(3)
```

$$  
\begin{bmatrix}  
1&0&0\  
0&1&0\  
0&0&1  
\end{bmatrix}  
$$

---

## Zeros

```python
np.zeros((3,3))
```

---

## Ones

```python
np.ones((3,3))
```

---

## Diagonal Matrix

```python
np.diag([1,2,3])
```

$$  
\begin{bmatrix}  
1&0&0\  
0&2&0\  
0&0&3  
\end{bmatrix}  
$$

---

## Random Matrix

```python
np.random.rand(3,3)
```

Uniform(0,1)

---

## Random Normal

```python
np.random.randn(3,3)
```

Standard normal

---

# 3. Shapes and Dimensions

## Shape

```python
A.shape
```

Returns:

```python
(rows, cols)
```

---

## Number of Dimensions

```python
A.ndim
```

---

## Total Elements

```python
A.size
```

---

# 4. Transpose

## Matrix Transpose

```python
A.T
```

or

```python
np.transpose(A)
```

Example:

```python
A = np.array([
    [1,2,3],
    [4,5,6]
])

A.T
```

$$  
\begin{bmatrix}  
1&4\  
2&5\  
3&6  
\end{bmatrix}  
$$

---

# 5. Reshape

Convert shapes without copying data.

```python
x = np.arange(12)

x.reshape(3,4)
```

$$  
\begin{bmatrix}  
0&1&2&3\  
4&5&6&7\  
8&9&10&11  
\end{bmatrix}  
$$

---

## Automatic Dimension

```python
x.reshape(3,-1)
```

NumPy computes missing dimension.

---

# 6. Matrix Multiplication

Most important operation.

---

## Element-wise Multiplication

```python
A * B
```

Example:

```python
A = np.array([[1,2],[3,4]])
B = np.array([[5,6],[7,8]])
```

```python
A * B
```

$$  
\begin{bmatrix}  
5&12\  
21&32  
\end{bmatrix}  
$$

---

## Matrix Multiplication

```python
A @ B
```

or

```python
np.matmul(A,B)
```

or

```python
np.dot(A,B)
```

Example:

```python
A @ B
```

$$  
\begin{bmatrix}  
19&22\  
43&50  
\end{bmatrix}  
$$

because

# $$  
\begin{bmatrix}  
1&2\  
3&4  
\end{bmatrix}  
\begin{bmatrix}  
5&6\  
7&8  
\end{bmatrix}

\begin{bmatrix}  
19&22\  
43&50  
\end{bmatrix}  
$$

---

# 7. Dot Product

## Vector Dot Product

```python
a = np.array([1,2,3])
b = np.array([4,5,6])

np.dot(a,b)
```

Result:

$$  
1(4)+2(5)+3(6)=32  
$$

---

## Using @

```python
a @ b
```

same result

---

# 8. Inner Product vs Outer Product

## Inner Product

```python
np.inner(a,b)
```

Produces scalar.

---

## Outer Product

```python
np.outer(a,b)
```

$$  
\begin{bmatrix}  
4&5&6\  
8&10&12\  
12&15&18  
\end{bmatrix}  
$$

---

# 9. Matrix Powers

## Repeated Multiplication

```python
np.linalg.matrix_power(A, 3)
```

Computes:

$$  
A^3  
$$

---

# 10. Determinants

Measures volume scaling and invertibility.

```python
np.linalg.det(A)
```

Example:

```python
A = np.array([
    [1,2],
    [3,4]
])

np.linalg.det(A)
```

Result:

$$  
1(4)-2(3)=-2  
$$

---

## Interpretation

If determinant:

- nonzero → invertible
    
- zero → singular
    

---

# 11. Matrix Inverse

Solve:

$$  
AX=B  
$$

by:

$$  
X=A^{-1}B  
$$

NumPy:

```python
A_inv = np.linalg.inv(A)
```

Example:

```python
A_inv @ A
```

returns identity matrix.

---

## Best Practice

Avoid:

```python
np.linalg.inv(A) @ b
```

Prefer:

```python
np.linalg.solve(A,b)
```

More stable and faster.

---

# 12. Solving Linear Systems

Given

$$  
Ax=b  
$$

Example:

$$  
\begin{aligned}  
2x+y &=5 \  
x+3y &=6  
\end{aligned}  
$$

```python
A = np.array([
    [2,1],
    [1,3]
])

b = np.array([5,6])

x = np.linalg.solve(A,b)
```

---

# 13. Rank

Number of independent rows/columns.

```python
np.linalg.matrix_rank(A)
```

Useful for:

- detecting redundancy
    
- checking singularity
    

---

# 14. Trace

Sum of diagonal.

```python
np.trace(A)
```

Example:

# $$  
\text{trace}(A)

a_{11}+a_{22}+...  
$$

---

# 15. Eigenvalues and Eigenvectors

Fundamental in PCA and many ML algorithms.

Solve:

$$  
Av=\lambda v  
$$

---

## Compute

```python
eigvals, eigvecs = np.linalg.eig(A)
```

Example:

```python
A = np.array([
    [2,0],
    [0,3]
])
```

Returns:

Eigenvalues:

```python
[2,3]
```

Eigenvectors:

```python
[[1,0],
 [0,1]]
```

---

## Verify

```python
A @ eigvecs[:,0]
```

equals

```python
eigvals[0] * eigvecs[:,0]
```

---

# 16. Singular Value Decomposition (SVD)

One of the most important decompositions.

$$  
A = U\Sigma V^T  
$$

Compute:

```python
U,S,Vt = np.linalg.svd(A)
```

Applications:

- PCA
    
- compression
    
- recommender systems
    
- dimensionality reduction
    

---

# 17. Norms

Measure vector size.

---

## Euclidean Norm

```python
np.linalg.norm(v)
```

Computes:

$$  
\sqrt{x_1^2+x_2^2+\cdots+x_n^2}  
$$

---

Example

```python
v = np.array([3,4])

np.linalg.norm(v)
```

returns

```python
5
```

---

## L1 Norm

```python
np.linalg.norm(v, ord=1)
```

$$  
\sum |x_i|  
$$

---

## Infinity Norm

```python
np.linalg.norm(v, ord=np.inf)
```

Largest absolute element.

---

# 18. Distances

Euclidean distance:

```python
np.linalg.norm(a-b)
```

Example:

```python
p1 = np.array([1,2])
p2 = np.array([4,6])

np.linalg.norm(p1-p2)
```

$$  
\sqrt{(4-1)^2+(6-2)^2}  
=5  
$$

---

# 19. Broadcasting in Linear Algebra

Example:

```python
A = np.random.rand(100,5)
mean = A.mean(axis=0)

A - mean
```

Mean vector automatically broadcasts to every row.

Very common for:

- centering data
    
- normalization
    
- feature scaling
    

---

# 20. Statistics as Linear Algebra

## Column Means

```python
X.mean(axis=0)
```

---

## Covariance Matrix

```python
np.cov(X.T)
```

Produces:

# $$  
\Sigma

\frac1{n-1}  
(X-\bar X)^T(X-\bar X)  
$$

---

## Correlation Matrix

```python
np.corrcoef(X.T)
```

---

# 21. Useful Axis Patterns

Suppose:

```python
X.shape == (100, 5)
```

100 observations, 5 features.

---

## Sum Each Column

```python
X.sum(axis=0)
```

Result:

```python
(5,)
```

---

## Sum Each Row

```python
X.sum(axis=1)
```

Result:

```python
(100,)
```

---

## Matrix Multiplication Shapes

|Shape A|Shape B|Result|
|---|---|---|
|(m,n)|(n,p)|(m,p)|
|(n,)|(n,)|scalar|
|(m,n)|(n,)|(m,)|

Example:

```python
X.shape
# (100,5)

w.shape
# (5,)
```

```python
X @ w
```

Result:

```python
(100,)
```

This is exactly how linear regression predictions are computed.

---

# 22. Common Machine Learning Operations

## Linear Regression Prediction

```python
y_pred = X @ w + b
```

---

## Logistic Regression

```python
z = X @ w + b

p = 1 / (1 + np.exp(-z))
```

---

## Neural Network Layer

```python
output = X @ W + b
```

Shapes:

```python
X = (batch, features)
W = (features, neurons)
output = (batch, neurons)
```

---

# 23. Most Important Functions to Memorize

```python
A.T
```

Transpose

```python
A @ B
```

Matrix multiply

```python
np.dot(a,b)
```

Dot product

```python
np.outer(a,b)
```

Outer product

```python
np.linalg.solve(A,b)
```

Solve system

```python
np.linalg.inv(A)
```

Inverse

```python
np.linalg.det(A)
```

Determinant

```python
np.linalg.eig(A)
```

Eigenvalues

```python
np.linalg.svd(A)
```

SVD

```python
np.linalg.norm(x)
```

Norm

```python
np.trace(A)
```

Trace

```python
np.linalg.matrix_rank(A)
```

Rank

---

# Mental Model

When doing NumPy linear algebra, think in terms of:

- **Vectors = points/arrows**
    
- **Matrices = transformations**
    
- **Dot product = similarity/projection**
    
- **Matrix multiplication = applying transformations**
    
- **Transpose = swapping rows and columns**
    
- **Inverse = undoing a transformation**
    
- **Determinant = volume scaling**
    
- **Eigenvectors = directions that don't rotate**
    
- **Eigenvalues = how much those directions stretch**
    
- **SVD = the universal decomposition of a matrix**
    

If you're preparing for quant, ML, statistics, or scientific computing interviews, the highest-value topics are matrix multiplication, broadcasting, solving linear systems, covariance matrices, eigenvalues/eigenvectors, and SVD. Those appear constantly in practice.
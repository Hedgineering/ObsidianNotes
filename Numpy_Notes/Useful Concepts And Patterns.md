Repo to fork for practice: [numpy-100](https://github.com/rougier/numpy-100)

"Null Vector" is another name for a zeros vector. 
```
np.zeros(10) # get array of 10 elements, each one is zero
np.full(10, np.nan) # create an array of 10 elements, fill with second arg (np.nan)
np.empty(10) # allocate array of 10 elements, defaults are garbage values
```

You can quickly use slicing to index into patterns of matrices, and reverse numpy arrays.
```
a[::-1] # reverse an array

## 2d Matrix with 0s inside and 1s bordering
size = 5
a = np.zeros((size, size))
a[0] = 1 # set first row to 1s
a[-1] = 1 # set last row to 1s
a[:, 0] = 1 # set first item of all rows to 1
a[:,-1] = 1 # set last item of all rows to 1
a

## Alternatively use np.pad to pad the borders of a matrix with some value
z = np.ones((5,5))
np.pad(z, pad_width=2, mode = 'constant', constant_values=2)
```
You can make a 0 start sequence of numbers (array) using `np.arange(start, end)`. This is often followed by `.reshape((n,m))` to quickly build matrices.

You can make a matrix of random numbers between 0 and 1 using `np.random.random((n,m))`.


If you need to tile or repeat matrices, here's and example of how you can create a checkerboard pattern
```
np.tile([[0, 1],[1,0]], (4,4)) # 8x8 checkerboard of 1s and 0s
```

If you need to normalize a matrix, you can do it like so
```
(a - a.min()) / (a.max() - a.min()) # normalize between 0 and 1
(a - a.mean()) / np.std(a) # normalize to z-scores
```

If you need to get the index in an n-dimensional array of the min or max (or just get the indexed equivalent of the i'th number in the matrix) you can do it with [[Ravel Index and Unravel Index]].

If you need to transform something into a diagonal matrix, you can use [[Diagonal Transformation Numpy]], including if you want it to be shifted from the main diagonal by $k$ spaces up or down.

If you're not using Pandas, you can represent tabular or structural data using numpy structs aka numpy custom data types -- see [[Numpy Custom Dtypes and Structured Arrays]]

You can multiply matrices in numpy for python 3.5 and above using the `@` operator:
```
A = np.ones((5, 3))
B = np.ones((3, 2))

print("A:\n", A)
print("B:\n", B)

print("A x B = ")

# here's one way
np.linalg.matmul(A, B)

# here's another way
A @ B

```

`np.where(z > 0)` is useful for getting indices  of values that are positive
`np.where(condition, value if true, value if false)` this is useful for conditional transformation

An example of rounding numbers directionally away from zero (round up if positive, down if negative)

```
np.where(z>0, np.ceil(z), np.floor(z))
```

You can determine overlaps between arrays using `np.intersect1d(a1, a2)`

You can do computations in place in numpy like so (using the `out=` parameter in numpy arithmetic functions)
```
A = np.ones(3)*1
B = np.ones(3)*2
np.add(A,B,out=B)
np.divide(A,2,out=A)
np.negative(A,out=A)
np.multiply(A,B,out=A)
```

Ways to extract integer part of numpy arrays
```
Z = np.random.uniform(0,10,10)

print(Z - Z%1) # modulus 1 to get fractional component and subtract to remove
print(Z // 1) # integer division
print(np.floor(Z)) # round down
print(Z.astype(int)) # cast to int (discard by truncate)
print(np.trunc(Z)) # truncate via numpy
```

Every ufunc has a reduce method that is marginally faster than numpy, e.g. `np.add.reduce(array)` is marginally faster than `np.sum(array)`

```
np.add.reduce(z)       # sum
np.multiply.reduce(z)  # product
np.maximum.reduce(z)   # max
np.minimum.reduce(z)   # min
```

General pattern:
```
ufunc.reduce(array)

(((a op b) op c) op d) ...
```

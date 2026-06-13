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
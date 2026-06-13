`np.unravel_index` converts a **flat (1D) index** into the corresponding **multi-dimensional coordinates** of an array.

Think of it as the opposite of flattening.

### Example 1: 2D array

Suppose you have a 3×4 array:

```python
import numpy as np

arr = np.arange(12).reshape(3, 4)

print(arr)
```

```text
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

The flattened version is:

```text
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
```

Index `6` in the flattened array corresponds to:

```python
np.unravel_index(6, arr.shape)
```

Output:

```python
(1, 2)
```

because:

```text
arr[1, 2] == 6
```

---

### Why it works

For a shape `(3, 4)`:

```text
flat index = row * 4 + col
```

So:

```text
6 = 1 * 4 + 2
```

which gives:

```text
(row, col) = (1, 2)
```

`unravel_index` does this arithmetic for you.

---

### Common use case: finding max/min location

```python
arr = np.array([
    [5, 2, 7],
    [1, 9, 3]
])

idx = np.argmax(arr)
print(idx)
```

Output:

```text
4
```

because `9` is at flat index 4.

Convert back to coordinates:

```python
row, col = np.unravel_index(idx, arr.shape)

print(row, col)
```

Output:

```text
1 1
```

Now:

```python
arr[row, col]
```

gives:

```text
9
```

---

### Example 2: 3D array

Shape:

```python
shape = (2, 3, 4)
```

Total elements:

```text
2 × 3 × 4 = 24
```

Take flat index 17:

```python
np.unravel_index(17, shape)
```

Output:

```python
(1, 1, 1)
```

Meaning:

```python
arr[1, 1, 1]
```

would be the 18th element in row-major (C-style) ordering.

---

### The inverse: `ravel_multi_index`

`unravel_index`:

```python
flat_idx -> coordinates
```

`ravel_multi_index`:

```python
coordinates -> flat_idx
```

Example:

```python
np.ravel_multi_index((1, 2), (3, 4))
```

Output:

```python
6
```

which is exactly the inverse of:

```python
np.unravel_index(6, (3, 4))
# (1, 2)
```

### Mental model

Imagine reading the array like a book:

```text
[[0 1 2 3]
 [4 5 6 7]
 [8 9 10 11]]
```

Flattening numbers them:

```text
0  1  2  3
4  5  6  7
8  9 10 11
↑
flat indices
```

`unravel_index(6, (3,4))` simply asks:

> "If I count to the 7th element while reading left-to-right, top-to-bottom, which row and column am I on?"

Answer:

```python
(1, 2)
```
This is because the **second argument means completely different things** in the two functions.

```
print(sum(range(5), -1)) # output 9

from numpy import sum as numpy_sum

print(numpy_sum(range(5), -1)) # output 10
```
### Python built-in `sum`

```python
sum(range(5), -1)
```


This is the signature, start is an optional additional value to add to the sum of the iterable
```python
sum([0 1 2 3 4], start=0)
```

So this means:

```python
-1 + 0 + 1 + 2 + 3 + 4
```

which equals:

```python
9
```

---

### NumPy `sum`

```python
np.sum(range(5), -1)
```

Signature:

```python
np.sum(a, axis=None, ...)
```

The second argument is **axis**, not a starting value.

So NumPy interprets this as:

```python
np.sum(range(5), axis=-1)
```

Since `range(5)` becomes a 1D array:

```python
array([0, 1, 2, 3, 4])
```

Axis `-1` means:

> Sum along the last dimension.

A 1D array only has one dimension, so axis `-1` is equivalent to axis `0`.

Thus:

```python
0 + 1 + 2 + 3 + 4 = 10
```

---

### Why does NumPy use negative axes?

Negative axes count from the end:

For a 3D array:

```python
a.shape == (2, 3, 4)
```

|Axis|Meaning|
|---|---|
|0|first dimension|
|1|second dimension|
|2|third dimension|
|-1|third dimension|
|-2|second dimension|
|-3|first dimension|

This is often convenient when you don't know how many leading dimensions exist.

---

### If you want a starting value with NumPy

Use the `initial` parameter:

```python
np.sum(range(5), initial=-1)
```

Result:

```python
9
```

which matches:

```python
sum(range(5), -1)
```

The key takeaway is:

|Function|Second Argument|
|---|---|
|`sum(iterable, x)`|starting value|
|`np.sum(array, x)`|axis to reduce over|

They have similar names but different APIs.
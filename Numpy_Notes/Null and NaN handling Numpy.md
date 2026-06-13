# Important NaN Facts
### Any number times nan is nan

```python
0 * np.nan
```

Output:

```python
nan
```

---

### Nan based Boolean expressions will always evaluate to false
```python
np.nan == np.nan
```

Output:

```python
False
```

---

```python
np.inf > np.nan
```

Output:

```python
False
```

---

### Arithmetic with NaN will become NaN

```python
np.nan - np.nan
```

Output:

```python
nan
```

---
### Nan can be found in sets, but you shouldn't rely on this behavior
```python
np.nan in set([np.nan])
```

Output:

```python
True
```

---

### Always use a diff threshold for floating point comparisons, Boolean expressions don't play well with floats
```python
0.3 == 3 * 0.1
```

Output:

```python
False
```

---



## 1. NaN propagates through arithmetic

Almost any arithmetic involving `NaN` produces `NaN`:

```python
np.nan + 5      # nan
np.nan * 0      # nan
np.nan - np.nan # nan
np.nan / 2      # nan
```

Think:

> "Once NaN enters a calculation, it contaminates the result."

---

## 2. NaN is not equal to anything

Not even itself:

```python
np.nan == np.nan
# False

np.nan != np.nan
# True
```

This is mandated by the IEEE-754 floating point standard.

---

## 3. Never check for NaN using `==`

Wrong:

```python
arr == np.nan
```

Correct:

```python
np.isnan(arr)
```

Example:

```python
arr = np.array([1, np.nan, 3])

np.isnan(arr)
# [False, True, False]
```

---

## 4. Comparisons with NaN are always False

```python
np.nan < 5
# False

np.nan > 5
# False

np.nan == 5
# False
```

Even:

```python
np.nan < np.nan
# False

np.nan > np.nan
# False
```

---

## 5. NaN is different from Infinity

```python
np.inf
```

means positive infinity.

```python
np.nan
```

means unknown / missing / undefined.

Examples:

```python
1 / 0.0
# inf

0 / 0.0
# nan
```

Conceptually:

```text
inf = extremely large
nan = not a number
```

---

## 6. NaNs affect aggregations

```python
arr = np.array([1, 2, np.nan])

np.sum(arr)
# nan

np.mean(arr)
# nan
```

Use the NaN-aware versions:

```python
np.nansum(arr)
# 3

np.nanmean(arr)
# 1.5
```

Other useful functions:

```python
np.nanmin
np.nanmax
np.nanstd
np.nanvar
np.nanmedian
```

---

## 7. Finding NaNs

```python
mask = np.isnan(arr)
```

Count NaNs:

```python
np.sum(np.isnan(arr))
```

Get indices:

```python
np.where(np.isnan(arr))
```

---

## 8. Replacing NaNs

Replace with zero:

```python
arr = np.nan_to_num(arr)
```

Or:

```python
arr[np.isnan(arr)] = 0
```

Replace with mean:

```python
arr[np.isnan(arr)] = np.nanmean(arr)
```

---

## 9. `None` vs `NaN`

`None`:

```python
np.array([1, None, 3])
```

becomes

```python
dtype=object
```

which is usually slow and inconvenient.

For numerical arrays use:

```python
np.nan
```

instead of:

```python
None
```

---

## 10. Why does `np.nan in set([np.nan])` return True?

This is a Python set implementation detail.

Even though:

```python
np.nan == np.nan
# False
```

the exact same object can still be found in a set because Python first checks hash buckets and object identity.

Don't rely on this behavior for NaN detection.

Use:

```python
np.isnan(x)
```

instead.

---

## Common interview / debugging gotchas

### Floating point equality

```python
0.3 == 3 * 0.1
# False
```

because floating point numbers are approximations.

Use:

```python
np.isclose(0.3, 3 * 0.1)
# True
```

---

### NaN-safe equality

```python
a = np.array([1, np.nan, 3])
b = np.array([1, np.nan, 3])

np.array_equal(a, b)
# False
```

Use:

```python
np.array_equal(a, b, equal_nan=True)
# True
```

or

```python
np.allclose(a, b, equal_nan=True)
# True
```

---

### Detect any NaNs

```python
np.isnan(arr).any()
```

### Detect all NaNs

```python
np.isnan(arr).all()
```

These are probably the two checks you'll use most often in data science and machine learning pipelines.
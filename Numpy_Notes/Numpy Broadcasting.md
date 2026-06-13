[Numpy Broadcasting by Bro Code](https://youtu.be/P67wiuTx7l0)

NumPy **broadcasting** is the set of rules that lets NumPy perform operations on arrays of different shapes **without actually copying data**.

## Intuition

Imagine you have:

```python
scores = np.array([
    [80, 90, 100],
    [70, 85, 95]
])
```

and you want to add 5 points to every score:

```python
scores + 5
```

You can think of NumPy as temporarily pretending the `5` is:

```python
[
 [5, 5, 5],
 [5, 5, 5]
]
```

and then performing element-wise addition.

The scalar isn't actually copied; NumPy just _broadcasts_ it across the array.

---

## Broadcasting Rule

When comparing two shapes:

1. Start from the **rightmost dimension**
    
2. Two dimensions are compatible if:
    
    - They are equal, OR
        
    - One of them is `1`
        
3. Missing dimensions are treated as `1`
    

---

# Example 1: Scalar + Matrix

```python
A.shape == (2, 3)
5.shape == ()
```

```python
A + 5
```

Think:

```python
(2,3)
(1,1)
```

`1` can expand to any size:

```python
(2,3)
(2,3)
```

Works.

---

# Example 2: Add a Row Vector

```python
A = np.array([
    [1,2,3],
    [4,5,6]
])

b = np.array([10,20,30])
```

Shapes:

```python
A.shape = (2,3)
b.shape = (3,)
```

NumPy treats `(3,)` as:

```python
(1,3)
```

Then:

```python
(2,3)
(1,3)
```

The first dimension expands:

```python
(2,3)
(2,3)
```

Result:

```python
[
 [11,22,33],
 [14,25,36]
]
```

Think of:

```python
[
 [10,20,30],
 [10,20,30]
]
```

being added.

---

# Example 3: Add a Column Vector

```python
A = np.array([
    [1,2,3],
    [4,5,6]
])

b = np.array([
    [10],
    [20]
])
```

Shapes:

```python
A.shape = (2,3)
b.shape = (2,1)
```

Compare:

```python
(2,3)
(2,1)
```

Dimension-by-dimension:

```python
2 vs 2  ✓
3 vs 1  ✓
```

The second dimension expands.

Result:

```python
[
 [11,12,13],
 [24,25,26]
]
```

Think of:

```python
[
 [10,10,10],
 [20,20,20]
]
```

---

# Example 4: Outer Addition

This is one of the most useful examples.

```python
row = np.array([1,2,3])
col = np.array([[10],[20],[30]])
```

Shapes:

```python
row.shape = (3,)
col.shape = (3,1)
```

Treat row as:

```python
(1,3)
```

Compare:

```python
(3,1)
(1,3)
```

Both dimensions can expand:

```python
(3,3)
```

Result:

```python
[
 [11,12,13],
 [21,22,23],
 [31,32,33]
]
```

Visualization:

```python
[
 [10],
 [20],
 [30]
]
+
[
 [1,2,3]
]
```

becomes

```python
[
 [11,12,13],
 [21,22,23],
 [31,32,33]
]
```

---

# Example 5: RGB Image Scaling

Suppose an image is:

```python
image.shape == (1000, 1000, 3)
```

where:

```python
[...,0] = Red
[...,1] = Green
[...,2] = Blue
```

You want to scale channels differently:

```python
scale = np.array([1.2, 0.8, 1.0])
```

Shapes:

```python
(1000,1000,3)
(3,)
```

NumPy sees:

```python
(1000,1000,3)
(1,1,3)
```

Result:

```python
image * scale
```

Every RGB pixel gets multiplied by:

```python
[R*1.2, G*0.8, B*1.0]
```

---

# Example 6: Standardizing Columns

Suppose:

```python
X.shape == (1000, 5)
```

1000 samples, 5 features.

Compute column means:

```python
means = X.mean(axis=0)
```

Shape:

```python
means.shape == (5,)
```

Then:

```python
X_centered = X - means
```

Broadcasting automatically subtracts the correct mean from each column.

Equivalent to:

```python
[
 [m1,m2,m3,m4,m5],
 [m1,m2,m3,m4,m5],
 ...
]
```

but without creating that giant array.

---

# Example 7: When Broadcasting Fails

```python
A.shape = (2,3)
B.shape = (2,2)
```

Compare:

```python
(2,3)
(2,2)
```

Last dimensions:

```python
3 vs 2
```

Neither is 1.

Result:

```python
ValueError:
operands could not be broadcast together
```

---

# A Useful Mental Model

Whenever you're unsure:

1. Write the shapes.
    
2. Align them on the right.
    
3. Replace missing dimensions with `1`.
    
4. Check each dimension:
    
    - same size ✓
        
    - one is `1` ✓
        
    - otherwise ❌
        

Example:

```python
(8, 1, 6)
(7, 6)
```

Align right:

```python
(8,1,6)
(1,7,6)
```

Compare:

```python
8 vs 1 ✓
1 vs 7 ✓
6 vs 6 ✓
```

Result shape:

```python
(8,7,6)
```

This "align from the right and let dimensions of size 1 stretch" rule explains essentially all NumPy broadcasting behavior.
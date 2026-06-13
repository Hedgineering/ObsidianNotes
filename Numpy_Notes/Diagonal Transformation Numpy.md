## `np.diag(..., k=?)`

The `k` parameter tells NumPy **which diagonal** to use relative to the main diagonal.

The main diagonal is:

$$  
\begin{bmatrix}  
\boxed{\cdot} & & & \\  
& \boxed{\cdot} & & \\  
& & \boxed{\cdot} & \\  
& & & \boxed{\cdot}  
\end{bmatrix}  
$$

- `k=0` → main diagonal
    
- `k=1` → one diagonal **above** the main diagonal
    
- `k=2` → two diagonals above
    
- `k=-1` → one diagonal **below** the main diagonal
    
- `k=-2` → two diagonals below
    

---

### Example: Main diagonal (`k=0`)

```python
np.diag([1,2,3,4])
```

$$  
\begin{bmatrix}  
1 & 0 & 0 & 0\  
0 & 2 & 0 & 0\  
0 & 0 & 3 & 0\  
0 & 0 & 0 & 4  
\end{bmatrix}  
$$

---

### Example: One diagonal below (`k=-1`)

```python
np.diag([1,2,3,4], k=-1)
```

$$  
\begin{bmatrix}  
0 & 0 & 0 & 0 & 0\\  
1 & 0 & 0 & 0 & 0\\  
0 & 2 & 0 & 0 & 0\\  
0 & 0 & 3 & 0 & 0\\  
0 & 0 & 0 & 4 & 0  
\end{bmatrix}  
$$

This is your exercise:

```
0 . . . .
1 0 . . .
. 2 0 . .
. . 3 0 .
. . . 4 0
```

Notice that every value is shifted **one row down** from the main diagonal.

---

### Example: One diagonal above (`k=1`)

```python
np.diag([1,2,3,4], k=1)
```

$$  
\begin{bmatrix}  
0 & 1 & 0 & 0 & 0\\  
0 & 0 & 2 & 0 & 0\\  
0 & 0 & 0 & 3 & 0\\  
0 & 0 & 0 & 0 & 4\\  
0 & 0 & 0 & 0 & 0  
\end{bmatrix}  
$$

Now everything is shifted **one column to the right**.

---

### Visual intuition

Think of `k` as an offset from the main diagonal:

```text
k = 2   x
k = 1  x
k = 0 x
k =-1  x
k =-2   x
```

Positive `k` moves **up/right**.

Negative `k` moves **down/left**.

A good way to remember it:

> `k` = "how many diagonals away from the main diagonal do you want?"  
> Positive = above. Negative = below. Zero = main diagonal.
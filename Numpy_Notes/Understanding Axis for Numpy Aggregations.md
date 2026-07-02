Consider:
```py
# Suppose this is the matrix of bernoulli trials
# Rows indicate infectious kids
# Cols indicate susceptible kids
# True indicates if infectious kid from row r infected susceptible kid from col c
attempts = np.array([
 [False, False, True,  False],
 [False, True,  False, False],
 [False, False, False, False]
])

# Find which susceptible kid was infected by any infectious kid
np.any(attempts, axis=0)

# output is array([False,  True,  True, False])
```

This is one of the most common sources of confusion in NumPy. The key insight is:

> **`axis` refers to the axis that disappears (is reduced), not the direction you visually scan.**

Let's build the intuition.

Your array has shape `(3, 4)`:

```text
        columns
          0      1      2      3
rows
0      False  False   True   False
1      False   True  False   False
2      False  False  False   False
```

Shape:

```python
attempts.shape
# (3, 4)
```

which means

- `axis=0` → the **row axis** (3 rows)
    
- `axis=1` → the **column axis** (4 columns)
    

Notice this is talking about **which dimension exists**, not what you're combining.

---

## What does reducing mean?

When you do

```python
np.any(attempts, axis=0)
```

NumPy asks:

> "Remove axis 0."

Axis 0 has length 3.

So for each column, it combines the three row values.

Column 0:

```text
False
False
False
```

↓

```python
False
```

Column 1:

```text
False
True
False
```

↓

```python
True
```

Column 2:

```text
True
False
False
```

↓

```python
True
```

Column 3:

```text
False
False
False
```

↓

```python
False
```

Result:

```python
[False, True, True, False]
```

There are still **4 columns**, because the row dimension disappeared.

---

## What if we reduce axis=1?

```python
np.any(attempts, axis=1)
```

Now we're removing the column axis.

Each row becomes one value.

Row 0:

```text
False False True False
```

↓

```python
True
```

Row 1:

```text
False True False False
```

↓

```python
True
```

Row 2:

```text
False False False False
```

↓

```python
False
```

Result:

```python
[True, True, False]
```

There are still **3 rows**, because the columns disappeared.

---

## A memory trick

Don't think

> "axis=0 means operate on rows."

Think

> **"axis=0 means REMOVE the row dimension."**

After removing rows, only columns remain.

Likewise,

> **"axis=1 means REMOVE the column dimension."**

After removing columns, only rows remain.

---

## Another visualization

Imagine squeezing the array.

### axis=0

```text
      ↓ squeeze ↓

F F T F
F T F F
F F F F
```

becomes

```text
F T T F
```

Rows collapsed.

---

### axis=1

```text
F F T F  → T
F T F F  → T
F F F F  → F
```

Columns collapsed.

---

## Why is axis 0 the row axis?

Because NumPy stores arrays as nested arrays.

Your matrix is actually

```python
[
    [False, False, True, False],   # row 0
    [False, True, False, False],   # row 1
    [False, False, False, False],  # row 2
]
```

The **outermost list** contains the rows.

Indexing works like

```python
attempts[0]      # first row
attempts[1]      # second row
```

So the first index (`axis=0`) selects **which row**.

Then inside a row,

```python
attempts[0][2]
```

or equivalently

```python
attempts[0, 2]
```

the second index selects the column.

That's why:

- first index → rows → `axis=0`
    
- second index → columns → `axis=1`
    

---

A concise rule that many experienced NumPy users rely on is:

> **The `axis` argument tells NumPy which index position to eliminate. Whatever dimensions remain determine the shape of the output.**

In your epidemic simulation, this maps nicely:

- Rows = infectious kids
    
- Columns = susceptible kids
    

So:

```python
np.any(attempts, axis=0)
```

asks:

> "For each susceptible kid (each column), did **any infectious kid** infect them?"

whereas

```python
np.any(attempts, axis=1)
```

asks:

> "For each infectious kid (each row), did they infect **any** susceptible kid?"

That's a useful mental model for reasoning about reductions in simulations.
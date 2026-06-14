# Generators: The Big Idea

A generator is a way to produce values **one at a time, on demand**, instead of creating the entire collection in memory upfront.

Compare:

```python
squares = [x*x for x in range(1_000_000)]
```

Creates **1 million numbers immediately**.

vs

```python
squares = (x*x for x in range(1_000_000))
```

Creates a generator that computes each square only when requested.

---

## Intuition

Think of:

### List

```python
[1, 2, 3, 4, 5]
```

Like a bucket containing all the values.

### Generator

```python
(x for x in range(5))
```

Like a vending machine:

```text
next() -> 0
next() -> 1
next() -> 2
...
```

Nothing exists until you ask for it.

---

## Example

```python
gen = (x*x for x in range(5))

next(gen)
```

Output:

```python
0
```

Then:

```python
next(gen)
```

Output:

```python
1
```

Then:

```python
next(gen)
```

Output:

```python
4
```

The generator remembers where it left off.

---

# Why Generators Exist

Suppose:

```python
[x*x for x in range(1_000_000_000)]
```

This would require enormous memory.

A generator:

```python
(x*x for x in range(1_000_000_000))
```

stores only:

```text
current position
code to generate next value
```

Memory usage stays tiny.

---

# Generator Functions

Instead of:

```python
def squares(n):
    return [i*i for i in range(n)]
```

you can write:

```python
def squares(n):
    for i in range(n):
        yield i*i
```

Notice:

```python
yield
```

instead of:

```python
return
```

---

When called:

```python
g = squares(5)
```

nothing executes yet.

Values are produced one-by-one:

```python
list(g)
```

Output:

```python
[0,1,4,9,16]
```

---

# Why This Matters For NumPy

NumPy likes contiguous arrays:

```python
array([1,2,3,4])
```

But sometimes data arrives sequentially:

- reading a huge file
    
- streaming sensor data
    
- parsing logs
    
- generating simulation results
    

A generator lets you produce values incrementally.

---

# np.fromiter

Purpose:

```python
np.fromiter(iterable, dtype)
```

Build a NumPy array directly from an iterator/generator.

Example:

```python
gen = (x*x for x in range(5))

np.fromiter(gen, dtype=int)
```

Output:

```python
array([0,1,4,9,16])
```

---

## Why Not Just Use np.array?

You could do:

```python
np.array(list(gen))
```

But then:

1. Generator → List
    
2. List → NumPy array
    

Two steps.

`fromiter` does:

```text
Generator → NumPy array
```

directly.

More memory efficient.

---

# Practical Example

Suppose a file contains:

```text
1
2
3
4
5
```

You can stream it:

```python
def read_numbers():
    with open("nums.txt") as f:
        for line in f:
            yield int(line)
```

Then:

```python
arr = np.fromiter(
    read_numbers(),
    dtype=np.int64
)
```

No giant intermediate list.

---

# The count Argument

Signature:

```python
np.fromiter(
    iterable,
    dtype,
    count=-1
)
```

---

## count = -1 (default)

Means:

```text
Read until iterator exhausted.
```

Example:

```python
gen = (x*x for x in range(5))

np.fromiter(gen, dtype=int)
```

NumPy keeps asking:

```python
next(gen)
next(gen)
next(gen)
...
```

until `StopIteration`.

---

## count = N

Example:

```python
gen = (x*x for x in range(100))

np.fromiter(
    gen,
    dtype=int,
    count=5
)
```

Output:

```python
array([0,1,4,9,16])
```

NumPy stops after 5 values.

---

# Why count Exists

Without count:

```text
NumPy doesn't know how large the array will be.
```

So it may need to:

```text
allocate
grow
copy
grow
copy
grow
copy
```

similar to how Python lists resize.

If count is known:

```python
np.fromiter(gen, dtype=int, count=1_000_000)
```

NumPy can:

```text
allocate exactly once
fill array
done
```

which is faster.

---

# A Realistic Use Case

Monte Carlo simulation:

```python
def simulation():
    while True:
        yield np.random.normal()
```

Collect first million samples:

```python
samples = np.fromiter(
    simulation(),
    dtype=float,
    count=1_000_000
)
```

No list ever created.

---

# When Would You Actually Use fromiter?

Rarely in everyday data science.

Most common cases:

### Reading huge files

```python
yield parsed_value
```

↓

```python
np.fromiter(...)
```

---

### Streaming simulations

```python
yield simulated_result
```

↓

```python
np.fromiter(...)
```

---

### Computational pipelines

```python
yield transform(x)
```

↓

```python
np.fromiter(...)
```

---

### Avoiding intermediate lists

Instead of:

```python
np.array([
    expensive(x)
    for x in data
])
```

use:

```python
np.fromiter(
    (expensive(x) for x in data),
    dtype=float
)
```

---

# Rule of Thumb

Use:

```python
np.array(...)
```

for normal Python lists.

Use:

```python
np.fromiter(...)
```

when:

- data is generated lazily
    
- data comes from a generator
    
- size is very large
    
- you want to avoid building an intermediate list
    

The biggest conceptual takeaway is that **generators trade memory for computation**: values are computed only when needed, and `np.fromiter` is NumPy's bridge from a lazy stream of values into a fast contiguous array.

## Why Generators Matter (Practical Use Cases)

Generators are most useful when:

- **Reading huge files** line-by-line instead of loading them entirely into memory.
    
- **Streaming data** from APIs, sensors, Kafka, sockets, etc.
    
- **Monte Carlo simulations** where samples are generated on demand.
    
- **Data pipelines** that transform data lazily.
    
- **Infinite sequences** (e.g. Fibonacci numbers, random walks).
    
- **Feeding NumPy via `np.fromiter()`** without creating an intermediate Python list.
    

### Best Practice

If you need all values immediately:

```python
data = [f(x) for x in values]
```

use a list.

If values are expensive to compute, potentially infinite, or very large:

```python
data = (f(x) for x in values)
```

use a generator.

A good rule of thumb:

> Use generators when memory usage matters or when you don't know how many values you'll need.

---

## Handling `StopIteration`

Generators signal they are exhausted by raising `StopIteration`.

### Unsafe

```python
gen = (x*x for x in range(3))

while True:
    print(next(gen))
```

Eventually:

```text
0
1
4
StopIteration
```

---

### Safe (try/except)

```python
gen = (x*x for x in range(3))

while True:
    try:
        value = next(gen)
        print(value)
    except StopIteration:
        break
```

Output:

```text
0
1
4
```

---

### Preferred Pythonic Approach

Usually you shouldn't call `next()` manually:

```python
gen = (x*x for x in range(3))

for value in gen:
    print(value)
```

Python automatically handles `StopIteration` internally.

This is how generators are used 99% of the time.

---

## Safe Manual Iteration with a Default Value

A lesser-known trick:

```python
gen = (x*x for x in range(3))

while True:
    value = next(gen, None)

    if value is None:
        break

    print(value)
```

The second argument to `next()` is a fallback value returned instead of raising `StopIteration`.

---

## Typical NumPy Pattern

```python
def read_numbers():
    with open("numbers.txt") as f:
        for line in f:
            yield float(line)
```

```python
arr = np.fromiter(
    read_numbers(),
    dtype=np.float64
)
```

Advantages:

```text
File -> Generator -> NumPy Array
```

instead of:

```text
File -> List -> NumPy Array
```

which requires an extra copy and more memory.

---

## Generator vs List Comprehension

```python
[x*x for x in range(1_000_000)]
```

creates:

```text
1,000,000 values immediately
```

Memory: O(n)

---

```python
(x*x for x in range(1_000_000))
```

creates:

```text
generator state
current position
generation logic
```

Memory: O(1)

---

## When `np.fromiter()` Is Worth Using

Most NumPy code uses:

```python
np.array(my_list)
```

Use `np.fromiter()` when:

- Source data is already a generator.
    
- Reading large files.
    
- Streaming data.
    
- Running large simulations.
    
- Avoiding an intermediate Python list.
    

Example:

```python
squares = (x*x for x in range(1_000_000))

arr = np.fromiter(
    squares,
    dtype=np.int64,
    count=1_000_000
)
```

Providing `count` is a performance optimization because NumPy can allocate the entire array once instead of repeatedly growing it.

```text
Generator -> NumPy Array
```

with one allocation instead of many.
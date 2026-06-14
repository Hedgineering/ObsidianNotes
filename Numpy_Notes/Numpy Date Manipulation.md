## Intuition

NumPy treats dates and times as **numeric values with time units attached**.

Think of:

```python
np.datetime64('2026-06-14')
```

as roughly:

> "Day number N since the Unix epoch"

and

```python
np.timedelta64(5, 'D')
```

as:

> "A duration of 5 days"

This allows dates to behave much like numbers:

```python
date + duration
date - duration
date2 - date1
```

---

## Core Types

### `datetime64`

Represents a specific point in time.

```python
date = np.datetime64('2026-06-14')
```

Output:

```python
numpy.datetime64('2026-06-14')
```

---

### `timedelta64`

Represents a duration.

```python
delta = np.timedelta64(5, 'D')
```

Output:

```python
numpy.timedelta64(5,'D')
```

Meaning:

```text
5 days
```

---

## Creating Dates

### Specific Date

```python
np.datetime64('2026-06-14')
```

```text
2026-06-14
```

---

### Date + Time

```python
np.datetime64('2026-06-14T15:30')
```

```text
2026-06-14T15:30
```

---

### Current Date

```python
np.datetime64('today')
```

Example:

```text
2026-06-14
```

---

### Current Time

```python
np.datetime64('now')
```

Example:

```text
2026-06-14T20:15:32
```

---

## Available Units

|Unit|Meaning|
|---|---|
|`Y`|Years|
|`M`|Months|
|`W`|Weeks|
|`D`|Days|
|`h`|Hours|
|`m`|Minutes|
|`s`|Seconds|
|`ms`|Milliseconds|
|`us`|Microseconds|
|`ns`|Nanoseconds|

Examples:

```python
np.timedelta64(1, 'D')
np.timedelta64(3, 'W')
np.timedelta64(2, 'h')
```

---

## Date Arithmetic

### Add Days

```python
today = np.datetime64('today')

today + np.timedelta64(1, 'D')
```

Tomorrow.

---

### Subtract Days

```python
today - np.timedelta64(7, 'D')
```

One week ago.

---

### Add Weeks

```python
today + np.timedelta64(2, 'W')
```

Two weeks later.

---

### Add Hours

```python
dt = np.datetime64('2026-06-14T10:00')

dt + np.timedelta64(3, 'h')
```

```text
2026-06-14T13:00
```

---

## Difference Between Dates

Subtracting two dates produces a `timedelta64`.

```python
end = np.datetime64('2026-06-20')
start = np.datetime64('2026-06-14')

end - start
```

Output:

```python
np.timedelta64(6,'D')
```

---

### Extract Number of Days

```python
(end - start) / np.timedelta64(1, 'D')
```

Output:

```python
6.0
```

---

## Creating Date Ranges

### Daily Dates

```python
np.arange(
    '2016-07',
    '2016-08',
    dtype='datetime64[D]'
)
```

Output:

```text
2016-07-01
2016-07-02
...
2016-07-31
```

Useful for:

- time series
    
- simulations
    
- calendars
    

---

### Weekly Dates

```python
np.arange(
    '2026-01',
    '2026-03',
    dtype='datetime64[W]'
)
```

Output:

```text
2025-12-29
2026-01-05
2026-01-12
...
```

---

### Monthly Dates

```python
np.arange(
    '2026',
    '2027',
    dtype='datetime64[M]'
)
```

Output:

```text
2026-01
2026-02
...
2026-12
```

---

## Casting Between Units

### Day → Month

```python
date = np.datetime64('2026-06-14')

date.astype('datetime64[M]')
```

Output:

```text
2026-06
```

---

### Day → Year

```python
date.astype('datetime64[Y]')
```

Output:

```text
2026
```

---

## Vectorized Date Arithmetic

Works on entire arrays.

```python
dates = np.arange(
    '2026-06-01',
    '2026-06-06',
    dtype='datetime64[D]'
)

dates
```

```text
['2026-06-01'
 '2026-06-02'
 '2026-06-03'
 '2026-06-04'
 '2026-06-05']
```

Add one week:

```python
dates + np.timedelta64(7, 'D')
```

```text
['2026-06-08'
 '2026-06-09'
 '2026-06-10'
 '2026-06-11'
 '2026-06-12']
```

---

## Useful Comparisons

```python
dates > np.datetime64('2026-06-03')
```

```text
[False False False True True]
```

---

### Filter Dates

```python
dates[dates >= np.datetime64('2026-06-03')]
```

Output:

```text
['2026-06-03'
 '2026-06-04'
 '2026-06-05']
```

---

## Business Day Functions

### Check Business Day

```python
np.is_busday('2026-06-15')
```

```text
True
```

---

### Next Business Day

```python
np.busday_offset(
    '2026-06-12',
    1
)
```

If June 12 is Friday:

```text
2026-06-15
```

(skips weekend)

---

### Count Business Days

```python
np.busday_count(
    '2026-06-01',
    '2026-07-01'
)
```

Returns number of weekdays.

---

## Common Patterns

### Days Until Trip

```python
trip = np.datetime64('2026-10-28')
today = np.datetime64('today')

(trip - today) / np.timedelta64(1, 'D')
```

---

### Generate Next 30 Days

```python
today = np.datetime64('today')

today + np.arange(30)
```

---

### Generate Every Monday

```python
np.arange(
    '2026-01',
    '2026-04',
    dtype='datetime64[W]'
)
```

---

## Mental Model

Treat NumPy dates as:

```text
datetime64  = timestamp
timedelta64 = duration
```

Just like:

```text
float64     = number
```

you can do:

```python
date + duration
date - duration
date2 - date1
```

and NumPy performs the arithmetic efficiently over entire arrays.
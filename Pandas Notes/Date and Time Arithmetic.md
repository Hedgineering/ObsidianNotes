You can convert / parse a column into a date using 

```py
import pandas as pd

df['date_col'] = pd.to_datetime(df['date_col'])

worker["joining_date"] = pd.to_datetime(worker["joining_date"])

worker['joining_year'] = worker.joining_date.dt.year
worker['joining_month'] = worker.joining_date.dt.month
worker['joining_day'] = worker.joining_date.dt.day


worker["probation_end"] = worker["joining_date"] + pd.Timedelta(days=90)
worker["anniversary"] = worker["joining_date"] + pd.DateOffset(years=1)
worker[["first_name", "joining_date", "probation_end", "anniversary"]]
```

Common `.dt` properties:

- `.dt.year` — 4-digit year
- `.dt.month` — month 1–12
- `.dt.day` — day of month 1–31
- `.dt.hour`, `.dt.minute`, `.dt.second` — time components
- `.dt.dayofweek` — Monday=0, Sunday=6 (as per ISO standard)
- `.dt.day_name()` — full name like “Monday”
- `.dt.quarter` — quarter 1–4

FYI: `pd.Timestamp.now()` is today’s datetime.

### Timedelta vs DateOffset

`pd.Timedelta(days=7)` for fixed durations (days, hours, seconds). 
`pd.DateOffset(months=1)` for calendar-aware durations (months, years) that **handle varying month lengths**.

- Subtract datetimes and use `.dt.days` for integer differences.
e.g.
```py
df['diff'] = (df['date1'] - df['date2']).dt.days
```


### Truncation for Time Series Analysis (TSA) or Grouping
#### Period vs Timestamp

`.dt.to_period("M")` returns a Period object (like "2024-03"). 
Some operations need Timestamps instead — convert back with `.dt.to_timestamp()`. 
For `groupby`, Period works fine.

```python
# Truncate to month (returns Period) -- generally better for grouping or TSA
orders["month"] = orders["order_date"].dt.to_period("M")

# Or truncate to first of month (returns Timestamp)
orders["month_start"] = (
    orders["order_date"].dt.to_period("M").dt.to_timestamp()
)
```
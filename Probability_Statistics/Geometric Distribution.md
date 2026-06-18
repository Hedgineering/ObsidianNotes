## Quick Summary

**Setup**

Repeat independent trials until the first success.

Each trial has

- Success probability: $p$
- Failure probability: $1-p$

Define the random variable

$$
X=\text{number of trials until the first success}
$$

Then

$$
X \sim \text{Geometric}(p)
$$

### Key Results

| Quantity | Formula |
|----------|---------|
| PMF | $P(X=k)=(1-p)^{k-1}p$ |
| Mean | $E[X]=\frac{1}{p}$ |
| Variance | $\frac{1-p}{p^2}$ |

---

# Intuition

The geometric distribution models **waiting time**.

Examples:

- Number of coin flips until the first Heads.
- Number of free throws until the first make.
- Number of two-dice rolls until the first sum of 11.
- Number of login attempts until success.

The key idea is:

> After every failure, you're in exactly the same situation as when you started.

Nothing has changed.

The trials are independent.

The process has **no memory**.

---

# First-Step Analysis (The Most Important Derivation)

Suppose

$$
E[X]=\mu.
$$

We don't know $\mu$ yet.

Instead of computing an infinite sum, analyze the **first trial**.

There are only two possibilities.

---

## Case 1: Success

Probability

$$
p
$$

You needed exactly

$$
1
$$

trial.

Contribution:

$$
1
$$

---

## Case 2: Failure

Probability

$$
1-p
$$

You've already used

$$
1
$$

trial.

Now you're back where you started.

Since the process is memoryless, you still expect another

$$
\mu
$$

trials.

Total:

$$
1+\mu
$$

---

# Build the Expectation Equation

Take the weighted average of the two cases.

$$
\mu
=
p(1)
+
(1-p)(1+\mu)
$$

This single equation completely determines the expected waiting time.

---

# Solve the Equation

Expand:

$$
\mu
=
p
+
(1-p)
+
(1-p)\mu
$$

Since

$$
p+(1-p)=1
$$

we obtain

$$
\mu
=
1
+
(1-p)\mu
$$

Move the expectation terms together:

$$
\mu-(1-p)\mu
=
1
$$

Factor:

$$
\mu p
=
1
$$

Therefore

$$
\boxed{
E[X]
=
\frac1p
}
$$

---

# Why This Works

Suppose you fail.

Did you get any closer?

No.

The process simply starts over.

You've wasted one trial.

Everything else is statistically identical.

Therefore

$$
\text{Expected time}
=
1
+
\text{Expected time again}
$$

weighted by the probability of failure.

This recursive idea is called **First-Step Analysis**.

---

# Visualization

```text
             Start
               │
        ┌──────┴──────┐
     Success       Failure
     (prob p)    (prob 1-p)
        │             │
      Done      Used one trial
                      │
                Back to Start
```

Notice the loop.

That loop is exactly why the expectation appears on **both sides** of the equation.

---

# Example: Rolling Two Dice Until Sum = 11

Possible sums of 11:

$$
(5,6),\;(6,5)
$$

There are

$$
36
$$

equally likely outcomes.

Therefore

$$
p
=
\frac2{36}
=
\frac1{18}
$$

Apply the formula:

$$
E[X]
=
\frac1{1/18}
=
18
$$

Interpretation:

On average,

you should expect to roll the dice about

$$
18
$$

times before seeing the first sum of 11.

This **does not** mean you'll always wait exactly 18 rolls.

Sometimes you'll get lucky on the first roll.

Sometimes it may take 50 or more.

The expectation is simply the long-run average over many repeated experiments.

---

# Alternative Derivation Using the PMF

The PMF of the geometric distribution is

$$
P(X=k)
=
(1-p)^{k-1}p
$$

By definition,

$$
E[X]
=
\sum_{k=1}^{\infty}
kP(X=k)
$$

Substitute the PMF:

$$
E[X]
=
\sum_{k=1}^{\infty}
k(1-p)^{k-1}p
$$

Factor out $p$:

$$
E[X]
=
p
\sum_{k=1}^{\infty}
k(1-p)^{k-1}
$$

Use the identity

$$
\sum_{k=1}^{\infty}
kr^{k-1}
=
\frac1{(1-r)^2}
$$

where

$$
r=1-p.
$$

Then

$$
1-r=p
$$

so

$$
E[X]
=
p
\cdot
\frac1{p^2}
=
\frac1p
$$

This derivation is elegant, but First-Step Analysis is usually much easier to understand and generalize.

---

# Why Is the Expected Waiting Time Approximately the Reciprocal?

Suppose an event happens

- 50% of the time

You should wait about

$$
\frac1{0.5}=2
$$

trials.

---

Suppose it happens

- 25% of the time

Average wait:

$$
\frac1{0.25}=4
$$

trials.

---

Suppose it happens

- 1% of the time

Average wait:

$$
\frac1{0.01}=100
$$

trials.

A useful rule of thumb is:

> If an independent event has probability $p$ each trial, expect to wait about $\frac1p$ trials before seeing it for the first time.

---

# Common Interview Trick

Many interview problems can be solved immediately by recognizing a geometric distribution.

Examples:

- Expected rolls until a die shows 6.

$$
p=\frac16
$$

Answer:

$$
E[X]=6
$$

---

- Expected flips until Heads.

$$
p=\frac12
$$

Answer:

$$
E[X]=2
$$

---

- Expected rolls of two dice until the sum is 7.

Probability:

$$
p=\frac6{36}=\frac16
$$

Answer:

$$
E[X]=6
$$

---

- Expected rolls until the sum is 11.

Probability:

$$
p=\frac2{36}=\frac1{18}
$$

Answer:

$$
E[X]=18
$$

---

# Key Intuition to Remember

The defining property of the geometric distribution is **memorylessness**.

After every failure:

- You have already spent one trial.
- The future is statistically identical to the beginning.
- Therefore,

$$
\boxed{
E[X]
=
1
+
(1-p)E[X]
}
$$

This recursive equation leads directly to

$$
\boxed{
E[X]
=
\frac1p
}
$$

without ever computing an infinite sum.

# Derivation of the Variance of the Geometric Distribution

## Quick Summary

Suppose independent Bernoulli trials have

- Success probability

$$
p
$$

- Failure probability

$$
1-p
$$

Define

$$
X=\text{number of trials until the first success}
$$

Then

$$
X\sim\text{Geometric}(p)
$$

The key formulas are

| Quantity | Formula |
|----------|----------|
| PMF | $P(X=k)=(1-p)^{k-1}p$ |
| Mean | $\displaystyle E[X]=\frac1p$ |
| Second Moment | $\displaystyle E[X^2]=\frac{2-p}{p^2}$ |
| Variance | $\displaystyle Var(X)=\frac{1-p}{p^2}$ |

---

# Review: Definition of Variance

Variance is defined as

$$
Var(X)
=
E[X^2]-E[X]^2
$$

Since we already know

$$
E[X]
=
\frac1p
$$

the only missing piece is

$$
E[X^2].
$$

---

# Method 1: First-Step Analysis (Recursive Derivation)

This is the most intuitive derivation and extends the same idea used to derive the expected value.

---

## Step 1

Let

$$
\mu
=
E[X]
=
\frac1p.
$$

Also define

$$
S
=
E[X^2].
$$

Our goal is to solve for

$$
S.
$$

---

## Step 2: Analyze the First Trial

There are only two possibilities.

---

### Case 1: Success

Probability

$$
p
$$

The first trial succeeds immediately.

Therefore

$$
X=1
$$

and

$$
X^2=1.
$$

Contribution:

$$
1
$$

---

### Case 2: Failure

Probability

$$
1-p
$$

One trial has already been used.

After that failure,

the process starts over exactly as before because each trial is independent.

Let the remaining waiting time be

$$
Y.
$$

Then

$$
X
=
1+Y.
$$

Now square both sides.

$$
X^2
=
(1+Y)^2
$$

Expand.

$$
X^2
=
1
+
2Y
+
Y^2
$$

Take expectations.

$$
E[X^2]
=
1
+
2E[Y]
+
E[Y^2]
$$

Since

$$
E[Y]=\mu
$$

and

$$
E[Y^2]=S,
$$

we obtain

$$
E[X^2]
=
1
+
2\mu
+
S.
$$

This is the expected squared waiting time **after a failure**.

---

## Step 3: Average the Two Cases

Weight each outcome by its probability.

$$
S
=
p(1)
+
(1-p)
(1+2\mu+S)
$$

Notice this is almost identical to the expectation derivation.

The only difference is that we squared

$$
1+Y.
$$

---

## Step 4: Substitute the Mean

Since

$$
\mu=\frac1p,
$$

substitute into the equation.

$$
S
=
p
+
(1-p)
\left(
1+\frac2p+S
\right)
$$

Expand.

$$
S
=
1
+
\frac{2(1-p)}p
+
(1-p)S
$$

Move the expectation terms together.

$$
S-(1-p)S
=
1+\frac{2(1-p)}p
$$

Factor.

$$
pS
=
1+\frac{2(1-p)}p
$$

Divide by

$$
p.
$$

$$
S
=
\frac1p
+
\frac{2(1-p)}{p^2}
$$

Simplify.

Common denominator:

$$
p^2
$$

Therefore

$$
S
=
\frac{p+2-2p}{p^2}
=
\frac{2-p}{p^2}
$$

Hence

$$
\boxed{
E[X^2]
=
\frac{2-p}{p^2}
}
$$

---

## Step 5: Compute the Variance

Use the definition.

$$
Var(X)
=
E[X^2]-E[X]^2
$$

Substitute.

$$
=
\frac{2-p}{p^2}
-
\frac1{p^2}
$$

Simplify.

$$
\boxed{
Var(X)
=
\frac{1-p}{p^2}
}
$$

---

# Why This Works

The expectation derivation used

$$
X=1+Y.
$$

The variance derivation uses

$$
X^2=(1+Y)^2.
$$

Expand the square.

$$
(1+Y)^2
=
1+2Y+Y^2
$$

Then apply expectations.

Everything else follows exactly the same recursive logic.

---

# Intuition

The geometric distribution is **memoryless**.

After a failure:

- You have already spent one trial.
- The future is statistically identical to the beginning.
- The remaining waiting time is another geometric random variable.

This recursive property makes the derivation possible.

---

# Method 2: Using the PMF

Start with the PMF.

$$
P(X=k)
=
(1-p)^{k-1}p
$$

By definition ([[Law of the Unconscious Statistician (LOTUS)]]),

$$
E[X^2]
=
\sum_{k=1}^{\infty}
k^2P(X=k)
$$

Substitute.

$$
E[X^2]
=
\sum_{k=1}^{\infty}
k^2(1-p)^{k-1}p
$$

Factor out

$$
p.
$$

$$
=
p
\sum_{k=1}^{\infty}
k^2r^{k-1},
\qquad
r=1-p
$$

Use the standard identity [[Geometric Series Identity]]

$$
\sum_{k=1}^{\infty}
k^2r^{k-1}
=
\frac{1+r}{(1-r)^3}
$$

Substitute

$$
r=1-p.
$$

Then

$$
1+r
=
2-p
$$

and

$$
1-r
=
p.
$$

Therefore

$$
E[X^2]
=
p
\cdot
\frac{2-p}{p^3}
=
\frac{2-p}{p^2}
$$

Finally,

$$
Var(X)
=
\frac{2-p}{p^2}
-
\frac1{p^2}
=
\frac{1-p}{p^2}
$$

---

# Example

Suppose

$$
p=\frac14.
$$

Mean

$$
E[X]
=
4
$$

Second moment

$$
E[X^2]
=
\frac{2-\frac14}{(\frac14)^2}
=
\frac{\frac74}{\frac1{16}}
=
28
$$

Variance

$$
Var(X)
=
28-4^2
=
12
$$

Alternatively,

$$
Var(X)
=
\frac{1-\frac14}{(\frac14)^2}
=
\frac{\frac34}{\frac1{16}}
=
12
$$

Both methods agree.

---

# Interpretation

As the success probability decreases,

the average waiting time grows.

However,

the waiting time also becomes much more unpredictable.

Example:

| Success Probability | Mean | Variance |
|--------------------|------|----------|
| $0.5$ | $2$ | $2$ |
| $0.25$ | $4$ | $12$ |
| $0.10$ | $10$ | $90$ |
| $0.01$ | $100$ | $9900$ |

Rare events have both

- long expected waiting times
- very large variability.

---

# Key Takeaways

- The variance measures the spread of the waiting time around its mean.
- The recursive (first-step analysis) derivation mirrors the derivation of the expected value.
- The only additional step is expanding

$$
(1+Y)^2
=
1+2Y+Y^2.
$$

- The geometric distribution becomes increasingly variable as the success probability decreases.

---

# Formula Cheat Sheet

| Quantity                    | Formula                             |
| --------------------------- | ----------------------------------- |
| PMF                         | $\displaystyle P(X=k)=(1-p)^{k-1}p$ |
| Mean $E[X]$                 | $\displaystyle \frac1p$             |
| Second Moment ($E[X^2]$)    | $\displaystyle \frac{2-p}{p^2}$     |
| Variance $=E[X^2]-(E[X])^2$ | $\displaystyle \frac{1-p}{p^2}$     |


---

# Memory Trick

The recursive derivations for the geometric distribution follow the same pattern.

Expected value:

$$
X=1+Y
$$

Second moment:

$$
X^2=(1+Y)^2
$$

Expand,

take expectations,

and solve recursively.

This "first-step analysis" technique appears frequently in probability, stochastic processes, Markov chains, random walks, and quantitative finance interviews.
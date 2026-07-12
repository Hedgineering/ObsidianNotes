# Basic Stochastic Processes

## Markov Chains

Consider a time series having a certain number of **states** (e.g., sun/rain) that can transition from day to day.

**Example:** On Monday it's sunny, on Tuesday and Wednesday it's rainy, etc.

> [!note] Informal definition If tomorrow's weather only depends on today (not on any earlier history), then you have a **Markov chain**.

### Setup: Weather Example

Let $X_i = 0$ if it rains on day $i$; otherwise $X_i = 1$. Denote the day-to-day transition probabilities by

$$ P_{jk} = P(\text{state } k \text{ on day } i+1 \mid \text{state } j \text{ on day } i), \qquad j,k = 0,1 $$

Suppose the probability state transition matrix is

$$ \mathbf{P} = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix} $$

(e.g., $P_{01} = 0.3$ is the top-right entry: probability of transitioning from rain (0) to sun (1)).

### Simulating a Sample Path

Suppose it rains on Monday. Simulate the rest of the work week by, on each day, drawing a uniform $U_i$ and comparing it against the relevant row of $\mathbf{P}$ (specifically $P_{\cdot 0}$, the probability of rain given yesterday's state) to decide whether it rains (R) or shines (S):

|Day|$P(R \mid X_{i-1})$|$U_i$|$U_i < P_{\cdot 0}$?|R/S|
|---|---|---|---|---|
|M|–|–|–|R|
|Tu|$P_{00} = 0.7$|0.62|Y|R|
|W|$P_{00} = 0.7$|0.03|Y|R|
|Th|$P_{00} = 0.7$|0.77|N|S|
|F|$P_{10} = 0.4$|0.91|N|S|

> [!tip] How to read this table Each day, look up the probability of rain **conditional on yesterday's actual outcome** (that's why the row switches from $P_{00}$ to $P_{10}$ once it turns sunny on Thursday). Draw a fresh uniform $U_i$, and if $U_i$ is less than that conditional rain probability, the day is rainy; otherwise it's sunny. This is just the inverse-transform method applied to a 2-state discrete distribution, re-applied fresh each day using whatever state you're currently in.

**Result:** starting from rain on Monday, this particular random path gives Rain, Rain, Rain, Sun, Sun for Tu–F.

---

## Poisson Arrivals

When the arrival rate is a **constant** $\lambda$, the interarrival times of a $\text{Poisson}(\lambda)$ process are i.i.d. $\text{Exp}(\lambda)$, and the arrival times are:

$$ T_0 \leftarrow 0 \qquad \text{and} \qquad T_i \leftarrow T_{i-1} - \frac{1}{\lambda}\ln(U_i), \qquad i \ge 1 $$

> [!success] Soooo easy! Each new arrival time is just the previous arrival time plus a fresh exponential interarrival gap, generated via inverse transform. No need to track anything more complicated than the running total.

### Generating a Fixed Number of Arrivals in a Fixed Interval

Now suppose instead we want to generate a **fixed number $n$** of $\text{PP}(\lambda)$ arrivals within a **fixed time interval** $[a,b]$.

> [!important] Key theorem The joint distribution of the $n$ arrival times is the same as the joint distribution of the **order statistics** of $n$ i.i.d. $\mathcal{U}(a,b)$ RVs.

This gives a very simple recipe:

1. Generate i.i.d. $U_1, \ldots, U_n$ from $\mathcal{U}(0,1)$.
2. Sort the $U_i$'s: $U_{(1)} < U_{(2)} < \cdots < U_{(n)}$.
3. Set the arrival times to $T_i \leftarrow a + (b-a)U_{(i)}$.

> [!success] Still soooo easy! Rather than simulating exponential interarrival gaps one at a time and hoping you land inside $[a,b]$ with exactly $n$ points, you just generate $n$ uniforms directly on $[a,b]$, sort them, and you're done — guaranteed exactly $n$ arrivals, correctly distributed.

## Related

- [[Order Statistics and Other Stuff]]
- [[Composition of Random Variables]]
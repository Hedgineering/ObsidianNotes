#statistics #simulation #output-analysis #steady-state #variance-estimation

# Other Steady-State Output Analysis Methods

> **Last lesson:** learned about various properties of the BM variance estimator and CI. **This time:** there are a number of other variance estimators out there that do **better than BM**! **Key idea to watch for:** overlapping batch means — literally, _something for nothing_.

---

## 1. Independent Replications (IR) Revisited for Steady-State

Of the difficulties with BM, **correlation among the batch means** is arguably the most troublesome.

- This problem is **explicitly avoided** by the method of IR (introduced earlier for terminating simulations) — the replicate means are independent **by construction**.
- **Trade-off:** since _each_ of the $r$ replications must be separately started (and warmed up), **initialization bias presents more trouble for IR** in steady-state analysis than it does for BM (which only has to warm up once, at the start of the single long run).

> **Recommendation (Alexopoulos & Goldsman 2004, "To Batch or not to Batch?"):** because of the extra initialization bias burden across all $r$ reps, it's generally better to **use batch means over independent replications** for steady-state analysis.

---

## 2. Overlapping Batch Means (OBM)

### The idea

Instead of the usual **nonoverlapping** batches, use a sliding window of overlapping batches of size $m$:

$$Y_1,\dots,Y_m \quad|\quad Y_2,\dots,Y_{m+1} \quad|\quad Y_3,\dots,Y_{m+2} \quad|\ \cdots$$

**Overlapping batch means:** $$\bar{Y}^o_{i,m} \equiv \frac{1}{m}\sum_{j=i}^{i+m-1} Y_j, \qquad i=1,2,\dots,n-m+1$$

This produces **many more** batch means than nonoverlapping BM (roughly $n-m+1$ of them, vs. only $b = n/m$ for regular BM) — using essentially the _same underlying data_.

### The surprising result

Even though the $\bar{Y}^o_{i,m}$'s are **highly correlated** with each other (since consecutive windows share almost all their data), this turns out to be **no problem**.

**OBM point estimator for $\mu$:** just $\bar{Y}_n$ (same as always).

**OBM estimator for $\sigma^2$:** $$\hat{V}_O = \frac{m}{n-m+1}\sum_{i=1}^{n-m+1}\big(\bar{Y}^o_{i,m} - \bar{Y}_n\big)^2$$

**Facts (as $n,m$ get large):** $$\frac{\mathrm{E}[\hat V_O]}{\mathrm{E}[\hat V_B]} \to 1 \qquad\text{and}\qquad \frac{\mathrm{Var}(\hat V_O)}{\mathrm{Var}(\hat V_B)} \to \frac{2}{3}$$

➡️ **Same bias as BM, but lower variance** — genuinely "something for nothing" (Meketon & Schmeiser 1984, "Overlapping Batch Means: Something for Nothing?").

- Note: **no attempt is made** to make the overlapping batch means independent — that's fine, since $\hat V_O$ turns out to be almost identical to **Bartlett's spectral estimator** for $\sigma^2$ (a connection to the frequency-domain approach described below).

### OBM confidence interval

For large $m$ and $b = n/m$: $$\hat{V}_O \approx \frac{\sigma^2 \chi^2(d)}{d}, \qquad d = \frac{3}{2}(b-1)$$

So OBM gives you **50% more degrees of freedom** than regular (nonoverlapping) batch means for the same data.

**Resulting CI:** $$\mu \in \bar{Y}_n \pm t_{\alpha/2,,d}\sqrt{\hat{V}_O/n}$$

> **Recommendation:** for large $m$ and $n/m$, **use OBM instead of regular BM** — you get a tighter CI at essentially no extra cost.

---

## 3. Spectral Estimation

Estimates $\mathrm{Var}(\bar{Y}_n)$ (and the corresponding CIs for $\mu$) in a way **completely different** from batch means.

- Operates in the **frequency domain**, whereas batch means works in the **time domain**.
- Sometimes takes a bit more effort to apply.
- Works well enough that it's worth consulting the relevant references directly, e.g., **Lada and Wilson's work on WASSP** (Wilson's Automated Simulation Sampling Procedure? — check the specific reference).

---

## 4. Regeneration

**Idea:** many simulations can be broken into **i.i.d. blocks** that probabilistically "start over" at certain **regeneration points**.

**Example:** an M/M/1 queue's waiting-time process — the i.i.d. blocks are defined by groups of customers whose **endpoints have zero waiting time** (the system "resets" to empty whenever a customer arrives to find no queue).

### Pros

- Uses genuine i.i.d. structure.
- Under certain conditions, gives **great estimators** for $\mathrm{Var}(\bar Y_n)$ and CIs for $\mu$.
- **Effectively eliminates initialization problems** — no need for truncation/warm-up, since the process naturally "restarts" itself.

### Cons

- Can be **difficult to define natural regeneration points** for many systems.
- Often need **extremely long simulation runs** just to obtain a reasonable _number_ of i.i.d. regeneration blocks.

---

## 5. Standardized Time Series

**Idea:** just as one often uses the CLT to standardize i.i.d. random variables into an (asymptotically) normal RV, **Schruben and colleagues** generalize this using a **process** central limit theorem — standardizing a stationary simulation process into a **Brownian bridge** process.

- Properties of Brownian bridges are then used to derive a number of good estimators for $\mathrm{Var}(\bar{Y}_n)$ and CIs for $\mu$.
- **Easy to apply**, and has some **asymptotic advantages over batch means**.
- **Open research issue:** combining various strategies together to get even-better variance estimators.

---

## Summary Comparison

|Method|Domain / Mechanism|Key advantage|Key drawback|
|---|---|---|---|
|**IR**|Multiple independent short runs|Replicate means truly independent|Initialization bias hits _every_ rep|
|**BM**|One long run, nonoverlapping batches|Simple, intuitive|Batch means can be correlated; only one bias hit|
|**OBM**|One long run, sliding/overlapping batches|Same bias as BM, lower variance, +50% d.f.|Slightly more computation to form all windows|
|**Spectral estimation**|Frequency domain|Fundamentally different approach; complements time-domain methods|More technical to apply|
|**Regeneration**|i.i.d. blocks between regeneration points|Eliminates initialization bias entirely|Hard to find natural regen points; needs very long runs|
|**Standardized time series**|Brownian bridge via process CLT|Easy to apply; asymptotic advantages over BM|Newer/more specialized; less "classic" than BM|

## Open Threads / To Follow Up

- [ ] Details of Bartlett's spectral estimator and its exact relationship to $\hat V_O$
- [ ] WASSP and other automated procedures referenced under spectral estimation
- [ ] How to identify regeneration points in more complex queueing networks
- [ ] Research direction: combining OBM / spectral / standardized-time-series ideas for better estimators
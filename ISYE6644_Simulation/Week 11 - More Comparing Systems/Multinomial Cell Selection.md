#simulation #statistics #ranking-and-selection

# Multinomial Selection

Continues from [[Bernoulli Extensions]]. Generalizes Bernoulli selection (2 categories: success/failure) to **multinomial** selection (any number of categories): find the **most-probable category**. Lots of applications — surveys, simulations, etc.

---

## Introduction

**Goal:** Select the multinomial category having the largest probability.

### Examples

- Who is the most popular political candidate?
- Which television show is most watched during a particular time slot?
- Which simulated warehouse configuration is most likely to maximize throughput?

As with normal means and Bernoulli selection, this again uses the **indifference-zone** approach (see [[Normal Means Selection]], [[Bernoulli Probability Selection]]).

---

## Experimental Set-Up

- $k$ possible outcomes (categories).
- $p_i$ is the probability of the $i$-th category.
- $n$ independent replications of the experiment.
- $Y_i$ is the number of outcomes falling in category $i$ after the $n$ observations have been taken.

---

## The Multinomial Distribution

**Definition:** If the $k$-variate discrete vector random variable $\boldsymbol{Y} = (Y_1, Y_2, \dots, Y_k)$ has probability mass function

$$ P{Y_1 = y_1, Y_2 = y_2, \dots, Y_k = y_k} = \frac{n!}{\prod_{i=1}^k y_i!} \prod_{i=1}^k p_i^{y_i}, $$

then $\boldsymbol{Y}$ has a **multinomial** distribution with parameters $n$ and $\boldsymbol{p} = (p_1, \dots, p_k)$, where $\sum_{i=1}^k p_i = 1$ and $p_i > 0$ for all $i$.

> [!note] Connection to Bernoulli When $k = 2$, this reduces to the familiar Binomial pmf. Multinomial selection is the natural generalization of [[Bernoulli Selection]] to more than two categories.

### Example: Colored Die

Suppose three faces of a fair die are **red**, two are **blue**, and one is **violet**, i.e., $\boldsymbol{p} = (3/6, 2/6, 1/6)$.

Toss it $n = 5$ times. The probability of observing exactly three reds, no blues, and two violets is:

$$ P{\boldsymbol{Y} = (3,0,2)} = \frac{5!}{3!,0!,2!}(3/6)^3(2/6)^0(1/6)^2 = 0.03472. $$

---

## Selection Rule

Suppose we did **not** know the true probabilities for red, blue, and violet, and we want to select the most probable color.

**Selection rule:** choose the category (color) that occurs most frequently during the $n$ trials, using **randomization** to break ties.

### Worked Example: $P(\mathrm{CS})$ for Red, $n = 5$

Let $\boldsymbol{Y} = (Y_r, Y_b, Y_v)$ denote the counts of (red, blue, violet) in five trials. The probability we correctly select red is:

$$ P{\text{red wins in 5 trials}} = P{Y_r > Y_b \text{ and } Y_v} + 0.5,P{Y_r = Y_b, Y_r > Y_v} + 0.5,P{Y_r > Y_b, Y_r = Y_v} $$

$$ = P{\boldsymbol{Y} = (5,0,0), (4,1,0), (4,0,1), (3,2,0), (3,1,1), (3,0,2)} + 0.5,P{\boldsymbol{Y}=(2,2,1)} + 0.5,P{\boldsymbol{Y}=(2,1,2)} $$

**Outcomes favorable to a correct selection (CS) of red**, with associated probabilities (randomizing on ties):

|Outcome $(r,b,v)$|Contribution to $P{\text{red wins}}$|
|---|---|
|(5,0,0)|0.03125|
|(4,1,0)|0.10417|
|(4,0,1)|0.05208|
|(3,2,0)|0.13889|
|(3,1,1)|0.13889|
|(3,0,2)|0.03472|
|(2,2,1)|$(0.5)(0.13889)$|
|(2,1,2)|$(0.5)(0.06944)$|

Summing: $P(\mathrm{CS}) = 0.6042$.

> [!tip] Boosting confidence Can increase $P(\mathrm{CS})$ by increasing $n$ — more trials give the true leading category more room to separate itself from the pack, just like larger $n$ improved $P(\mathrm{CS})$ in the normal-means and Bernoulli settings.

---

## Caution: "Most Probable" ≠ "Highest Expected Value"

> [!warning] These are different goals The most probable alternative might be **preferable** to the one having the largest **expected value** — they can disagree completely.

**Example:** Consider two inventory policies, $A$ and $B$, where

$$ \text{Profit from } A = $5 \text{ with probability } 1 $$

$$ \text{Profit from } B = \begin{cases} $0 & \text{with probability } 0.99 \ $1000 & \text{with probability } 0.01 \end{cases} $$

Then: $$ \mathrm{E}[\text{Profit from } A] = $5 < \mathrm{E}[\text{Profit from } B] = $10, $$

but

$$ P{\text{Profit from } A > \text{Profit from } B} = 0.99. $$

So $\mathrm{E}[A] < \mathrm{E}[B]$, but **$A$ wins almost all of the time**.

> [!note] Takeaway Multinomial/Bernoulli-style "most probable" selection procedures answer a genuinely different question than mean-based procedures like [[Single-Stage Normal Means Procedure]]. Choosing the right criterion (biggest mean vs. highest probability of success/winning) matters — revisit the "what measures do you use to compare systems?" discussion in [[Ranking and Selection]].

---

## Summary

- Multinomial selection generalizes Bernoulli selection to $k \geq 2$ categories: pick the category with the largest true probability $p_i$.
- Multinomial pmf: $P{\boldsymbol{Y}=\boldsymbol{y}} = \frac{n!}{\prod y_i!}\prod p_i^{y_i}$.
- Simple selection rule: pick the category with the highest observed count out of $n$ trials, randomizing to break ties.
- $P(\mathrm{CS})$ can be computed exactly by enumerating favorable outcomes (as in the red/blue/violet die example) and increases with $n$.
- **Caution:** "most probable" and "largest expected value" are different criteria and can select different alternatives — choose the criterion that actually matches your decision problem.
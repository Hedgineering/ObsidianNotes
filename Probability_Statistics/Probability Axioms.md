**Prerequisite Information**
[[Sample Spaces and Simple Sample Spaces]]
# Probability Axioms

The standard probability axioms are the **Kolmogorov axioms**.

Let $\Omega$ be the sample space and let $A, B, A_1, A_2, \dots$ be events.

---

# The Three Probability Axioms (Kolmogorov's Axioms)

The three Kolmogorov axioms are the foundational assumptions of probability theory. Most of elementary probability is derived from them.
## 1. Non-negativity

For every event $A$,

$$
P(A) \ge 0
$$

Probabilities can never be negative.

---

## 2. Normalization

The probability of the entire sample space is 1:

$$
P(\Omega) = 1
$$

Something in the sample space must happen.

---

## 3. Countable Additivity ($\sigma$-additivity)

If events $A_1, A_2, A_3, \dots$ are mutually exclusive (disjoint), meaning

$$
A_i \cap A_j = \emptyset
\quad \text{for } i \ne j
$$

then

$$
P\left(\bigcup_{i=1}^{\infty} A_i\right)
=
\sum_{i=1}^{\infty} P(A_i)
$$

The probability of disjoint events occurring is the sum of their probabilities.

---

# Important Consequences Derived from the Axioms

These are not axioms themselves, but they follow directly from them.

---

## Probability of the Empty Set

$$
P(\emptyset) = 0
$$

Impossible events have probability zero.

---

## Complement Rule

$$
P(A^c) = 1 - P(A)
$$

---

## Monotonicity

If

$$
A \subseteq B
$$

then

$$
P(A) \le P(B)
$$

A subset cannot have larger probability than the set containing it.

---

## Addition Rule

For any two events:

$$
P(A \cup B)
=
P(A) + P(B) - P(A \cap B)
$$

If you have 3 events:
  
$$  
P(A \cup B \cup C)  
=  
P(A) + P(B) + P(C)  
- P(A \cap B)  
- P(A \cap C)  
- P(B \cap C)  
+ P(A \cap B \cap C)  
$$
The final addition term is because the middle of the 3 way Venn Diagram is subtracted away twice when you remove the second and third intersection terms.

This can be generalized to the following
### General Inclusion–Exclusion Principle

For events $A_1, A_2, \dots, A_n$:

$$
P\left(\bigcup_{i=1}^n A_i\right)
=
\sum_i P(A_i)
-
\sum_{i<j} P(A_i \cap A_j)
+
\sum_{i<j<k} P(A_i \cap A_j \cap A_k)
-\cdots+
(-1)^{n+1}P\left(\bigcap_{i=1}^n A_i\right)
$$

#### Pattern

- Add all single-event probabilities (+ Singles)
- Subtract all pairwise intersections (- Pairs intersection)
- Add all triple intersections (+ Triple intersections)
- Subtract all quadruple intersections (- Quadruple intersections)
- Continue alternating signs (etc.)

---

## Union Bound (Boole's Inequality)

$$
P\left(\bigcup_i A_i\right)
\le
\sum_i P(A_i)
$$

---

## Bounds on Probability

For every event $A$,

$$
0 \le P(A) \le 1
$$

---

## Conditional Probability / Bayes' Rule

When $P(B) > 0$,

$$
P(A \mid B)
=
\frac{P(A \cap B)}{P(B)}
=
\frac{P(B \mid A) P(A)}{P(B)}
$$

---

## Independence

Events $A$ and $B$ are independent if

$$
P(A \cap B)
=
P(A)P(B)
$$

---


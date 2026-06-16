## Motivation

Many probability problems are naturally stated in the "forward" direction.

Examples:

- Given that someone has a disease, what is the probability they test positive?
- Given that it is raining, what is the probability the ground is wet?

These are probabilities of the form

$$
P(B \mid A)
$$

However, we are often interested in the reverse question:

- Given a positive test result, what is the probability the person actually has the disease?
- Given that the ground is wet, what is the probability it rained?

These are probabilities of the form

$$
P(A \mid B)
$$

Bayes' Theorem allows us to reverse conditional probabilities.

---

## Prerequisite: Conditional Probability

Recall the definition of conditional probability.

For events $A$ and $B$ with $P(B) > 0$,

$$
P(A \mid B)
=
\frac{P(A \cap B)}
{P(B)}
$$

Similarly,

$$
P(B \mid A)
=
\frac{P(A \cap B)}
{P(A)}
$$

Rearranging gives

$$
P(A \cap B)
=
P(B \mid A)P(A)
$$

Substituting into the first equation produces Bayes' Theorem.

---

## Bayes' Theorem

For events $A$ and $B$ with $P(B) > 0$,

$$
P(A \mid B)
=
\frac{P(B \mid A)P(A)}
{P(B)}
$$

where

- $P(A)$ = Prior
- $P(B \mid A)$ = Likelihood
- $P(B)$ = Evidence
- $P(A \mid B)$ = Posterior

A useful memory aid:

$$
\text{Posterior}
=
\frac{\text{Likelihood} \times \text{Prior}}
{\text{Evidence}}
$$

---

## Computing the Evidence

The denominator $P(B)$ is often not given directly.

Using the Law of Total Probability,

$$
P(B)
=
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
$$

Substituting into Bayes' Theorem gives

$$
P(A \mid B)
=
\frac{
P(B \mid A)P(A)
}{
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
}
$$

This is the form most commonly used in applications.

---

## Intuition

Think of Bayes' Theorem as updating a belief after observing new evidence.

Before observing evidence:

$$
P(A)
$$

After observing evidence $B$:

$$
P(A \mid B)
$$

The stronger the relationship between $A$ and $B$, the more our belief should change.

Observing evidence that strongly supports $A$ should increase the probability of $A$.

Observing evidence that frequently occurs even without $A$ should have a smaller effect.

---

## Relationship to Kolmogorov's Axioms

Bayes' Theorem is not an axiom.

It follows from the definition of conditional probability, which itself is built on the standard probability framework.

### Axiom 1 (Non-Negativity)

$$
P(A) \ge 0
$$

### Axiom 2 (Normalization)

$$
P(S) = 1
$$

### Axiom 3 (Countable Additivity)

For disjoint events,

$$
P(A \cup B)
=
P(A)
+
P(B)
$$

Bayes' Theorem is therefore a consequence of the probability axioms rather than a separate rule.

---

## Important Related Formulas

### Conditional Probability

$$
P(A \mid B)
=
\frac{P(A \cap B)}
{P(B)}
$$

### Multiplication Rule

$$
P(A \cap B)
=
P(A \mid B)P(B)
=
P(B \mid A)P(A)
$$

### Law of Total Probability

$$
P(B)
=
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
$$

### Bayes' Theorem

$$
P(A \mid B)
=
\frac{
P(B \mid A)P(A)
}{
P(B)
}
$$

### Expanded Bayes' Theorem

$$
P(A \mid B)
=
\frac{
P(B \mid A)P(A)
}{
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
}
$$

# Bayes' Theorem Practice Problems

## Problem 1: Basic Bayes

Suppose

$$
P(A)=0.4
$$

$$
P(B \mid A)=0.7
$$

$$
P(B \mid A^c)=0.2
$$

Find

$$
P(A \mid B)
$$

---

## Solution

First compute $P(A^c)$:

$$
P(A^c)
=
1-P(A)
=
1-0.4
=
0.6
$$

Using the Law of Total Probability,

$$
P(B)
=
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
$$

$$
P(B)
=
(0.7)(0.4)
+
(0.2)(0.6)
$$

$$
P(B)
=
0.28+0.12
=
0.40
$$

Now use Bayes' Theorem:

$$
P(A \mid B)
=
\frac{P(B \mid A)P(A)}
{P(B)}
$$

$$
P(A \mid B)
=
\frac{(0.7)(0.4)}
{0.40}
$$

$$
P(A \mid B)
=
\frac{0.28}{0.40}
=
0.70
$$

Therefore,

$$
P(A \mid B)
=
0.70
$$

---

# Problem 2: Medical Test

A disease affects $2\%$ of a population.

A test has sensitivity $95\%$, meaning

$$
P(+ \mid D)=0.95
$$

The false positive rate is $10\%$, meaning

$$
P(+ \mid D^c)=0.10
$$

Find the probability that a person actually has the disease given that they tested positive.

That is, find

$$
P(D \mid +)
$$

---

## Solution

We are given:

$$
P(D)=0.02
$$

$$
P(D^c)=1-0.02=0.98
$$

$$
P(+ \mid D)=0.95
$$

$$
P(+ \mid D^c)=0.10
$$

Using the Law of Total Probability,

$$
P(+)
=
P(+ \mid D)P(D)
+
P(+ \mid D^c)P(D^c)
$$

$$
P(+)
=
(0.95)(0.02)
+
(0.10)(0.98)
$$

$$
P(+)
=
0.019+0.098
=
0.117
$$

Now apply Bayes' Theorem:

$$
P(D \mid +)
=
\frac{P(+ \mid D)P(D)}
{P(+)}
$$

$$
P(D \mid +)
=
\frac{(0.95)(0.02)}
{0.117}
$$

$$
P(D \mid +)
=
\frac{0.019}
{0.117}
$$

$$
P(D \mid +)
\approx
0.1624
$$

Therefore,

$$
P(D \mid +)
\approx
16.24\%
$$

### Intuition

Even though the test is sensitive, the disease is rare.

Because the disease affects only $2\%$ of the population, many positive tests come from false positives among healthy people.

This is an example of the base rate effect.

---

# Problem 3: Spam Filter

Suppose $30\%$ of emails are spam.

A spam filter flags $90\%$ of spam emails.

It also incorrectly flags $5\%$ of non-spam emails.

Find the probability that an email is actually spam given that it was flagged.

That is, find

$$
P(S \mid F)
$$

---

## Solution

Let

$$
S = \text{email is spam}
$$

and

$$
F = \text{email is flagged}
$$

We are given:

$$
P(S)=0.30
$$

$$
P(S^c)=0.70
$$

$$
P(F \mid S)=0.90
$$

$$
P(F \mid S^c)=0.05
$$

Compute $P(F)$:

$$
P(F)
=
P(F \mid S)P(S)
+
P(F \mid S^c)P(S^c)
$$

$$
P(F)
=
(0.90)(0.30)
+
(0.05)(0.70)
$$

$$
P(F)
=
0.270+0.035
=
0.305
$$

Apply Bayes' Theorem:

$$
P(S \mid F)
=
\frac{P(F \mid S)P(S)}
{P(F)}
$$

$$
P(S \mid F)
=
\frac{(0.90)(0.30)}
{0.305}
$$

$$
P(S \mid F)
=
\frac{0.270}
{0.305}
$$

$$
P(S \mid F)
\approx
0.8852
$$

Therefore,

$$
P(S \mid F)
\approx
88.52\%
$$

---

# Problem 4: Defective Items From Two Machines

Machine A produces $70\%$ of a factory's items.

Machine B produces $30\%$ of the items.

Machine A has a defect rate of $1\%$.

Machine B has a defect rate of $4\%$.

An item is selected randomly and found to be defective.

Find the probability that it came from Machine B.

That is, find

$$
P(B \mid D)
$$

---

## Solution

Let

$$
B = \text{item came from Machine B}
$$

and

$$
D = \text{item is defective}
$$

We are given:

$$
P(A)=0.70
$$

$$
P(B)=0.30
$$

$$
P(D \mid A)=0.01
$$

$$
P(D \mid B)=0.04
$$

Compute the total probability of a defective item:

$$
P(D)
=
P(D \mid A)P(A)
+
P(D \mid B)P(B)
$$

$$
P(D)
=
(0.01)(0.70)
+
(0.04)(0.30)
$$

$$
P(D)
=
0.007+0.012
=
0.019
$$

Apply Bayes' Theorem:

$$
P(B \mid D)
=
\frac{P(D \mid B)P(B)}
{P(D)}
$$

$$
P(B \mid D)
=
\frac{(0.04)(0.30)}
{0.019}
$$

$$
P(B \mid D)
=
\frac{0.012}
{0.019}
$$

$$
P(B \mid D)
\approx
0.6316
$$

Therefore,

$$
P(B \mid D)
\approx
63.16\%
$$

### Intuition

Even though Machine B only makes $30\%$ of the items, it has a much higher defect rate.

So once we know the item is defective, Machine B becomes the more likely source.

---

# Problem 5: Drawing From Bags

There are two bags.

Bag 1 contains:

- 4 red balls
- 6 blue balls

Bag 2 contains:

- 8 red balls
- 2 blue balls

A bag is chosen at random, then one ball is drawn.

The ball is red.

Find the probability that Bag 2 was chosen.

That is, find

$$
P(B_2 \mid R)
$$

---

## Solution

Let

$$
B_1 = \text{Bag 1 chosen}
$$

$$
B_2 = \text{Bag 2 chosen}
$$

$$
R = \text{red ball drawn}
$$

Since each bag is equally likely,

$$
P(B_1)=\frac{1}{2}
$$

$$
P(B_2)=\frac{1}{2}
$$

From Bag 1,

$$
P(R \mid B_1)
=
\frac{4}{10}
=
0.4
$$

From Bag 2,

$$
P(R \mid B_2)
=
\frac{8}{10}
=
0.8
$$

Compute $P(R)$ using total probability:

$$
P(R)
=
P(R \mid B_1)P(B_1)
+
P(R \mid B_2)P(B_2)
$$

$$
P(R)
=
(0.4)\left(\frac{1}{2}\right)
+
(0.8)\left(\frac{1}{2}\right)
$$

$$
P(R)
=
0.2+0.4
=
0.6
$$

Apply Bayes' Theorem:

$$
P(B_2 \mid R)
=
\frac{P(R \mid B_2)P(B_2)}
{P(R)}
$$

$$
P(B_2 \mid R)
=
\frac{(0.8)\left(\frac{1}{2}\right)}
{0.6}
$$

$$
P(B_2 \mid R)
=
\frac{0.4}
{0.6}
=
\frac{2}{3}
$$

Therefore,

$$
P(B_2 \mid R)
=
\frac{2}{3}
$$

---

# Problem 6: Cards

A card is drawn randomly from a standard $52$-card deck.

Let

$$
A = \text{card is an ace}
$$

and

$$
S = \text{card is a spade}
$$

Find

$$
P(A \mid S)
$$

---

## Solution

Using direct counting:

There are $13$ spades.

Only $1$ of those spades is an ace.

Therefore,

$$
P(A \mid S)
=
\frac{1}{13}
$$

We can also verify using Bayes' Theorem.

We know:

$$
P(A)=\frac{4}{52}=\frac{1}{13}
$$

$$
P(S)=\frac{13}{52}=\frac{1}{4}
$$

Given that a card is an ace, exactly one of the four aces is a spade:

$$
P(S \mid A)=\frac{1}{4}
$$

Now apply Bayes' Theorem:

$$
P(A \mid S)
=
\frac{P(S \mid A)P(A)}
{P(S)}
$$

$$
P(A \mid S)
=
\frac{
\left(\frac{1}{4}\right)
\left(\frac{1}{13}\right)
}{
\frac{1}{4}
}
$$

$$
P(A \mid S)
=
\frac{1}{13}
$$

---

# Problem 7: Multiple Causes

A company receives applications from three sources:

- Website: $50\%$
- Referral: $30\%$
- Recruiter: $20\%$

The probability of being hired from each source is:

$$
P(H \mid W)=0.04
$$

$$
P(H \mid R)=0.10
$$

$$
P(H \mid C)=0.08
$$

where

$$
W = \text{website}
$$

$$
R = \text{referral}
$$

$$
C = \text{recruiter}
$$

Given that a randomly selected applicant was hired, find the probability they came from a referral.

That is, find

$$
P(R \mid H)
$$

---

## Solution

We are given:

$$
P(W)=0.50
$$

$$
P(R)=0.30
$$

$$
P(C)=0.20
$$

$$
P(H \mid W)=0.04
$$

$$
P(H \mid R)=0.10
$$

$$
P(H \mid C)=0.08
$$

Compute $P(H)$ using the Law of Total Probability:

$$
P(H)
=
P(H \mid W)P(W)
+
P(H \mid R)P(R)
+
P(H \mid C)P(C)
$$

$$
P(H)
=
(0.04)(0.50)
+
(0.10)(0.30)
+
(0.08)(0.20)
$$

$$
P(H)
=
0.020+0.030+0.016
=
0.066
$$

Apply Bayes' Theorem:

$$
P(R \mid H)
=
\frac{P(H \mid R)P(R)}
{P(H)}
$$

$$
P(R \mid H)
=
\frac{(0.10)(0.30)}
{0.066}
$$

$$
P(R \mid H)
=
\frac{0.030}
{0.066}
$$

$$
P(R \mid H)
\approx
0.4545
$$

Therefore,

$$
P(R \mid H)
\approx
45.45\%
$$

---

# Problem 8: Drug Test False Positives

Suppose $5\%$ of people use a certain drug.

A test correctly identifies users $98\%$ of the time.

A test incorrectly identifies non-users as users $3\%$ of the time.

A person tests positive.

Find the probability that the person actually uses the drug.

That is, find

$$
P(U \mid +)
$$

---

## Solution

Let

$$
U = \text{person uses the drug}
$$

We are given:

$$
P(U)=0.05
$$

$$
P(U^c)=0.95
$$

$$
P(+ \mid U)=0.98
$$

$$
P(+ \mid U^c)=0.03
$$

Compute $P(+)$:

$$
P(+)
=
P(+ \mid U)P(U)
+
P(+ \mid U^c)P(U^c)
$$

$$
P(+)
=
(0.98)(0.05)
+
(0.03)(0.95)
$$

$$
P(+)
=
0.049+0.0285
=
0.0775
$$

Apply Bayes' Theorem:

$$
P(U \mid +)
=
\frac{P(+ \mid U)P(U)}
{P(+)}
$$

$$
P(U \mid +)
=
\frac{(0.98)(0.05)}
{0.0775}
$$

$$
P(U \mid +)
=
\frac{0.049}
{0.0775}
$$

$$
P(U \mid +)
\approx
0.6323
$$

Therefore,

$$
P(U \mid +)
\approx
63.23\%
$$

---

# Problem 9: Reverse Conditional Probability

Suppose

$$
P(A)=0.25
$$

$$
P(B)=0.50
$$

$$
P(A \mid B)=0.40
$$

Find

$$
P(B \mid A)
$$

---

## Solution

Start with the multiplication rule:

$$
P(A \cap B)
=
P(A \mid B)P(B)
$$

$$
P(A \cap B)
=
(0.40)(0.50)
=
0.20
$$

Now use conditional probability:

$$
P(B \mid A)
=
\frac{P(A \cap B)}
{P(A)}
$$

$$
P(B \mid A)
=
\frac{0.20}
{0.25}
$$

$$
P(B \mid A)
=
0.80
$$

Therefore,

$$
P(B \mid A)
=
0.80
$$

---

# Problem 10: Diagnostic Test With Negative Result

A disease affects $1\%$ of people.

A test has sensitivity $99\%$:

$$
P(+ \mid D)=0.99
$$

The false positive rate is $5\%$:

$$
P(+ \mid D^c)=0.05
$$

A person tests negative.

Find the probability that the person has the disease.

That is, find

$$
P(D \mid -)
$$

---

## Solution

We are given:

$$
P(D)=0.01
$$

$$
P(D^c)=0.99
$$

We need probabilities involving a negative test.

Since

$$
P(+ \mid D)=0.99
$$

we have

$$
P(- \mid D)
=
1-P(+ \mid D)
=
1-0.99
=
0.01
$$

Since

$$
P(+ \mid D^c)=0.05
$$

we have

$$
P(- \mid D^c)
=
1-P(+ \mid D^c)
=
1-0.05
=
0.95
$$

Compute $P(-)$:

$$
P(-)
=
P(- \mid D)P(D)
+
P(- \mid D^c)P(D^c)
$$

$$
P(-)
=
(0.01)(0.01)
+
(0.95)(0.99)
$$

$$
P(-)
=
0.0001+0.9405
=
0.9406
$$

Apply Bayes' Theorem:

$$
P(D \mid -)
=
\frac{P(- \mid D)P(D)}
{P(-)}
$$

$$
P(D \mid -)
=
\frac{(0.01)(0.01)}
{0.9406}
$$

$$
P(D \mid -)
=
\frac{0.0001}
{0.9406}
$$

$$
P(D \mid -)
\approx
0.000106
$$

Therefore,

$$
P(D \mid -)
\approx
0.0106\%
$$

---

# Summary Pattern

Most Bayes problems follow this structure:

$$
P(\text{Cause} \mid \text{Evidence})
=
\frac{
P(\text{Evidence} \mid \text{Cause})
P(\text{Cause})
}{
P(\text{Evidence})
}
$$

The denominator is usually found using total probability:

$$
P(\text{Evidence})
=
\sum_i
P(\text{Evidence} \mid \text{Cause}_i)
P(\text{Cause}_i)
$$

In the two-case version:

$$
P(B)
=
P(B \mid A)P(A)
+
P(B \mid A^c)P(A^c)
$$
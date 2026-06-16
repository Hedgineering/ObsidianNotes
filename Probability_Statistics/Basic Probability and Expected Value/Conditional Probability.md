## Intuition

Conditional probability answers:

> "Given that event (B) has already occurred, what is the probability that event (A) occurs?"

Instead of considering the entire sample space, we restrict our attention to outcomes where (B) happened.

---

## Definition


$$
P(A \mid B)
=
\frac{P(A \cap B)}{P(B)}
\qquad
(P(B) > 0)
$$


where:

- $P(A \mid B)$ = probability of (A) given (B)
    
- $P(A \cap B)$ = probability both (A) and (B) occur
    
- $P(B)$ = probability (B) occurs
    

---

## Rearranging the Formula

Often useful:

$$
P(A \cap B)
=
P(A \mid B)P(B)
$$


and similarly


$$
P(A \cap B)
=
P(B \mid A)P(A)
$$


---

## Visual Interpretation

Suppose

$$
P(B)=0.4
$$

and

$$
P(A \cap B)=0.1
$$

Then

$$
P(A \mid B)
=
\frac{0.1}{0.4}
=
0.25
$$

Interpretation:

> Among the outcomes where (B) occurred, (25%) also satisfy (A).

---

# Example 1: Drawing Cards

A card is drawn from a standard deck.

Let

- (A): card is an Ace
    
- (B): card is a Spade
    

Find (P(A \mid B)).

### Solution

There are 13 spades.

Only one is the Ace of Spades.

Thus

$$
P(A \cap B)
=
\frac{1}{52}
$$

and

$$
P(B)
=
\frac{13}{52}
$$

Therefore

$$
P(A \mid B)
=
\frac{\frac1{52}}{\frac{13}{52}}
=
\frac1{13}
$$

---
## Example 2: Rolling Dice

Roll two fair dice.

Let

- (A): sum equals 8
    
- (B): first die equals 3
    

Find (P(A \mid B)).

### Solution

If first die is 3, possible outcomes are:

$$  
(3,1),(3,2),(3,3),(3,4),(3,5),(3,6)  
$$

Only

$$  
(3,5)  
$$

gives sum 8.

Therefore

$$  
P(A \mid B)
=
\frac16  
$$

---

# Example 3: Medical Testing

Suppose

$$  
P(D)=0.01  
$$

Disease prevalence.

Test sensitivity:

$$  
P(+ \mid D)=0.95  
$$

False positive rate:

$$  
P(+ \mid D^c)=0.05  
$$

Find probability a person has the disease given a positive result.

This is a Bayes Theorem problem.

$$  
P(D \mid +)
=
\frac{P(+ \mid D)P(D)}  
{P(+)}  
$$

where

$$  
P(+)
=
P(+ \mid D)P(D)  
+  
P(+ \mid D^c)P(D^c)  
$$

Calculate:

$$  
P(+)
=
0.95(0.01)  
+  
0.05(0.99)
=
0.059  
$$

Thus

$$  
P(D \mid +)
=
\frac{0.95(0.01)}  
{0.059}  
\approx 0.161  
$$

Even after a positive test, the probability of having the disease is only about

$$  
16.1%  
$$

because the disease is rare.

---

# Common Exam Relationships

## Conditional Probability

$$ 
P(A \mid B)
=
\frac{P(A \cap B)}{P(B)}  
$$

## Multiplication Rule

$$  
P(A \cap B)
=
P(A \mid B)P(B)  
$$

## Bayes Theorem

$$  
P(A \mid B)
=
\frac{P(B \mid A)P(A)}  
{P(B)}  
$$

## Independence

If (A) and (B) are independent:

$$  
P(A \mid B)
=
P(A)  
$$

and

$$  
P(A \cap B)
=
P(A)P(B)  
$$

---

# Practice Problems

## Problem 1

A deck of cards is shuffled.

Given that the card drawn is a face card, what is the probability it is a King?

### Solution

Face cards:

$$  
12  
$$

Kings:

$$  
4  
$$

Therefore

$$  
P(\text{King} \mid \text{Face Card})

= \frac{4}{12}
=
\frac13  
$$

---

## Problem 2

Two fair dice are rolled.

Given the sum is 7, what is the probability the first die is 3?

### Solution

Sum 7 outcomes:

$$  
(1,6),(2,5),(3,4),(4,3),(5,2),(6,1)  
$$

Total:

$$  
6  
$$

Favorable:

$$  
(3,4)  
$$

Thus

$$  
P(\text{First die}=3 \mid \text{Sum}=7)
=
\frac16  
$$

---

## Problem 3

Suppose

$$  
P(A)=0.6  
$$

$$  
P(B)=0.5  
$$

$$  
P(A \cap B)=0.3  
$$

Find (P(A \mid B)).

### Solution

$$  
P(A \mid B) =

\frac{P(A \cap B)}  
{P(B)}
=
\frac{0.3}{0.5}
=
0.6  
$$

Since

$$  
P(A \mid B)
=
P(A)  
$$

the events are independent.

---

# Quick Checklist

When you see:

> "Given that..."

immediately think:

$$  
P(A \mid B)
=
\frac{P(A \cap B)}  
{P(B)}  
$$

Typical workflow:

1. Identify the condition (B).
    
2. Restrict the sample space to outcomes where (B) occurs.
    
3. Count outcomes satisfying both (A) and (B).
    
4. Divide by outcomes satisfying (B).
    

This "restrict the sample space first" viewpoint is usually the fastest way to solve conditional probability questions on interviews and exams.
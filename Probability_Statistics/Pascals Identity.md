# Intuition for Pascal's Identity

Recall:

$$
{n \choose r}
=
{n-1 \choose r}
+
{n-1 \choose r-1}
$$

## Example

Suppose we want to choose 2 people from 5 people:

$$
{5 \choose 2}
$$

Focus on one specific person, say Alice.

Any valid group of 2 people falls into one of two categories:

### Case 1: Alice is NOT chosen

Then we must choose all 2 people from the remaining 4:

$$
{4 \choose 2}
$$

---

### Case 2: Alice IS chosen

We already used 1 spot for Alice, so we only need 1 more person from the remaining 4:

$$
{4 \choose 1}
$$

---

Adding both cases:

$$
{5 \choose 2}
=
{4 \choose 2}
+
{4 \choose 1}
$$

Numerically:

$$
10 = 6 + 4
$$

This is Pascal's Identity.

---


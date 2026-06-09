Prerequisites:
- [[Counting Principles#Permutations]]
- [[Counting Principles#Combinations]]

Suppose you have 2 white marbles and 4 red marbles. If you are randomly shuffling these marbles in a line, Find
- The probability that the white marbles will be on the ends ($WRRRRW$)
- The probability that the white marbles will be side-by-side
	- For example, any of the below are valid
		- $WWRRRR$
		- $RWWRRR$
		- ...
		- $RRRRWW$

## Permutations Approach

Define sample space $S=\{\text{Every random ordering of 6 marbles}\}$

### Probability of $WRRRRW$

How many ways  are there to order 6 marbles? 

${}_6P_6 = 6! = 720$ ways

$\underline{\text{6 options}} \quad \underline{\text{5 options}} \quad \underline{\text{4 options}} \quad \underline{\text{3 options}} \quad \underline{\text{2 options}} \quad \underline{\text{1 option}} \rightarrow \quad 6! \text{ possibilities}$

Now, for a moment, lets label each marble with an identifier:

$\{W_1, W_2, R_1, R_2, R_3, R_4\}$

If we want to find the probability that the white marbles will be on the ends ($WRRRRW$), we can arrive at this selection in several ways, since the white marbles are interchangeable, and the red marbles are interchangeable. That is to say:

Interchangeable White Marbles:
$\{W_1, R_1, R_2, R_3, R_4, W_2\} = \{W_2, R_1, R_2, R_3, R_4, W_1\} = WRRRRW$

- For the two white marbles, we have 2 slots we can put them into, the beginning or the end, so the cardinality (size) of this set of permutations that make up $WRRRRW$ where the red marbles by identifier are held constant is ${}_2P_2=2!=2$ as can be seen above

Interchangeable Red Marbles:
$\{W_1, R_1, R_2, R_3, R_4, W_2\} = \{W_1, R_2, R_1, R_3, R_4, W_2\} = \ldots = WRRRRW$

- For the four red marbles, we have 4 slots we can put them into, the four middle slots, so the cardinality (size) of this set of permutations that make up $WRRRRW$ where the white marbles by identifier are held constant is ${}_4P_4=4!=4 \cdot 3 \cdot 2 \cdot 1 = 24$

Therefore, the overall size of the set of permutations that make up $WRRRRW$ is the product of the above two (since for either choice of the white marble permutations we can permute all the red marbles), so we get $2 \cdot 24 = 48$ permutations.

Thus, $P(WRRRRW)=\frac{2!4!}{6!}=\frac{2 \cdot 1 \cdot 4 \cdot 3 \cdot 2 \cdot 1 }{6 \cdot 5 \cdot 4 \cdot 3 \cdot 2 \cdot 1 } = \frac{2}{6 \cdot 5} = \frac{1}{15} \text{ or } \frac{48 \text{ valid permutations}}{720 \text{ total permutations}} = 0.0\overline{6}$

$\blacksquare$

### Probability of Whites side-by-side

Consider one valid set $WWRRRR$ - Similar to the above section, we can see that whites are interchangeable and reds are interchangeable, so out of all permutations of 6 marbles, we have $2! \cdot 4!$ ways of getting $WWRRRR$, so $\frac{2!4!}{6!} = \frac{1}{15}$. 

You can shift the two white marbles to get all 5 possibilities where whites are side by side:
- $WWRRRR$
- $RWWRRR$
- $RRWWRR$
- $RRRWWR$
- $RRRRWW$

By the same argument, each of these have a $\frac{1}{15}$ probability, so the total probability is $5 \cdot \frac{1}{15} = \frac{1}{3}$

$\blacksquare$

## Combinations Approach

Let's define the sample space $S = \{\text{All possible pairs of indices occupied by white marbles }\}$.
### Probability of $WRRRRW$

**What is the cardinality of $S$ ?**
- If we have 6 indices (1,2,3,4,5,6), one for each position a marble can occupy, and we can only choose two of them for the white marbles (order doesn't matter here) then we have ${}_6C_2=15$ total index pairs
	- E.g.
		- Indices $\{ 1, 2\} \rightarrow WWRRRR$
		- Indices $\{ 1, 3\} \rightarrow WRWRRR$
		- Indices $\{ 1, 4\} \rightarrow WRRWRR$
		- etc.

If we want whites to be in the beginning and end, the only valid combination of indices that we're interested in is $\{ 1, 6\} \rightarrow WRRRRW$.

So:
$$
\frac{1 \text{ valid combination of indices - i.e. }\{ 1, 6\}}{{}_6C_2 \text{ possible combinations of index pairs}} = \frac{1}{15} 
$$
### Probability of Whites side-by-side

Similar to the Permutations approach, we can say we have 5 possible configurations that are valid, so if each configuration has a $\frac{1}{15}$ probability, then having whites side by side is simply $5 \cdot \frac{1}{15} = \frac{1}{3}$

$\blacksquare$


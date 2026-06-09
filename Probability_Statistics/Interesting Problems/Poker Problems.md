Prerequisites:
- [[Counting Principles#Permutations]]
- [[Counting Principles#Combinations]]

Poker Hands are ranked like this (best to worst):
1. Royal Flush (10, J, Q, K, A all of the same suit)
2. Straight Flush (Sequence of the same suit)
3. Quads aka 4-of-a-kind (e.g. all four 8's or all four K's)
4. Full House (a pair and a 3-of-a-kind)
5. Flush
6. Straight
7. Triplets aka 3-of-a-kind (e.g. three 8's or three K's)
8. Two Pair (e.g. a pair of 8's or a pair of K's)
9. One Pair (e.g. 3 of hearts and 3 of spades)
10. High Card

## High Card  
  
- Pick 5 distinct ranks  
- Exclude the 10 straight sequences  
- Assign suits to each card  
- Exclude flushes (all cards of the same suit)  
  
$$  
\frac  
{\left({13 \choose 5} - 10\right)\left(4^5 - 4\right)}  
{{52 \choose 5}}  
\approx  
0.501177  
$$  
  
$$  
50.1177\%  
$$  
  
You'll get this about once every 2 hands.
## Pair - 2 cards of the same rank, rest junk

- Pick a rank (13 ranks across 4 suits) - so 13 choose 1
- Pick two suits - so 4 choose 2
- For the other junk cards, we need 3 OTHER ranks
	- from the 13 possible ranks, we already occupied 1 for our pair, so we need to pick 3 from the other 12
- Choose 1 suit per junk card (each card can be any of the 4 suits, so 4 * 4 * 4)

$$
\frac
{ {13 \choose 1} \cdot {4 \choose 2} \cdot {12 \choose 3} \cdot 4^3 }
{52 \choose 5}
\approx 0.422569 \text{ or } 42.26\%
$$
You'll get this maybe once in every 2 or 3 hands.

## 2 Pair (not full house)
- Pick 2 ranks (e.g. Ace and 2)
- Pick 2 suits for the first pair (Ace hearts and Ace spades)
- Pick 2 suits for the second pair (2 diamonds and 2 hearts)
- Pick a junk card
	- can't be the same rank as either of the ones chosen before (so the other two aces and other two 2s are out, and we already picked 4 cards, so out of 52 we removed 8 possibilities so we have 44 remaining junk cards to choose from)

$$
\frac
{ {13 \choose 2} \cdot {4 \choose 2} \cdot {4 \choose 2} \cdot {44 \choose 1} }
{52 \choose 5}
\approx
0.047539 \text{ or } 4.7539\%
$$

You'll get this maybe once in every 21 hands.

## 3 of a kind (not full house)
- Pick 1 rank out of the 13 (e.g. Ace)
- Pick 3 suits out of the four (e.g. hearts, diamonds, clubs)
- Pick (not a pair), so of the 12 remaining ranks, we pick two ranks
	- This can be of any suit, so choose any suit of the four for the first card and any of the four for the second card

$$
\frac
{ {13 \choose 1} \cdot {4 \choose 3} \cdot {12 \choose 2} \cdot {4 \choose 1} \cdot {4 \choose 1} }
{52 \choose 5}
\approx
0.02113 \text{ or } 2.113\%
$$
You'll get this once in every 47 or 48 hands.

## Straight (not Straight Flush)  
  
- There are 10 possible rank sequences
```
1. A 2 3 4 5
2.   2 3 4 5 6
3.     3 4 5 6 7
4.       4 5 6 7 8
5.         5 6 7 8 9
...

9:   9 10 J Q K
10:    10 J Q K A
```
- Each card may be any of the 4 suits  
- Subtract the 40 straight flushes  
  
$$  
\frac  
{10 \cdot 4^5 - 40}  
{{52 \choose 5}}  
\approx  
0.003925  
$$  
  
$$  
0.3925\%  
$$  
  
You'll get this once every 255 hands.

## Flush - 5 cards of the same suit

There are 4 suits and 13 cards in a suit, of which 5 make up your hand.

We want to create a fraction where the numerator represents the cardinality of the desired hands and the denominator represents the cardinality of all possible hands.

Denominator is easy -- there are 52 cards in total, and we need a hand of 5 cards (order doesn't matter) so we choose 5 from 52 cards $52 \choose 5$ in the denominator.

Numerator:
- There are 4 suits (hearts, spades, clubs, diamonds), so we need to pick a suit (4 choose 1)
- There are 13 cards in a suit (Ace through King) and we need 5 in our hand (13 choose 5)

So for any suit, we take any 5 card hand of that suit. This makes up the numerator.
$$
\frac
{{4 \choose 1} \cdot {13 \choose 5}}
{52 \choose 5}
\approx
0.00198 \text{ or } 0.198\%
$$

You'll get this once every 505-506 hands.

If we're not including the 10 straight flushes in each suit (see above)


Then we get 
$$  
\frac  
{{4 \choose 1} \cdot {13 \choose 5} - 40}  
{{52 \choose 5}}  
\approx  
0.001965  
$$  
  
$$  
0.1965\%  
$$  
  
You'll get this once every 509 hands.
## Full House (A pair and a 3-of-a-kind)
- pick a rank for the pair
- pick 2 suits for the pair
- pick a different rank for the 3-of-a-kind
- pick any 3 suits for the 3-of-a-kind

$$
\frac
{ {13 \choose 1} \cdot {4 \choose 2} \cdot {12 \choose 1} \cdot {4 \choose 3} }
{52 \choose 5}
\approx
0.00144 \text{ or } 0.144\%
$$
You'll get this once every 694-695 hands.

## Quads / 4-of-a-Kind  
  
- Pick a rank for the quads  
- Take all 4 suits  
- Pick 1 junk card from the remaining 48 cards  
  
$$  
\frac  
{{13 \choose 1} \cdot {4 \choose 4} \cdot {48 \choose 1}}  
{{52 \choose 5}}  
\approx  
0.0002401  
$$  
  
$$  
0.02401\%  
$$  
  
You'll get this once every 4,165 hands.

## Straight Flush (not Royal Flush)  
  
- There are 10 possible straight sequences  
- Exclude the royal flush sequence, leaving 9  
- Pick 1 suit  
  
$$  
\frac  
{9 \cdot {4 \choose 1}}  
{{52 \choose 5}}  
=  
\frac{36}{2,598,960}  
\approx  
0.00001385  
$$  
  
$$  
0.001385\%  
$$  
  
You'll get this once every 72,193 hands.  
  
## Royal Flush  
  
- Pick 1 suit  
- The ranks are fixed: 10, J, Q, K, A  
  
$$  
\frac  
{{4 \choose 1}}  
{{52 \choose 5}}  
=  
\frac{4}{2,598,960}  
\approx  
0.00000154  
$$  
  
$$  
0.000154\%  
$$  
  
You'll get this once every 649,740 hands.
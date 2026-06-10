Suppose

Store sells a product at $ $d$  / unit

Our inventory policy is to have at least $s$ units in stock at the start of each day.

If our inventory is less than $s$ by the end of the day (EOD), we place an order with our supplier to restore our stock to $S$ by the beginning of the next day (assume overnight delivery).

Let $I_i$ denote inventory at end of day $i$.
Let $Z_i$ denote the order we place at the end of day $i$

$$
Z_i=
\begin{cases}
S - I_i & \text{ if } I_i < s \\
0 & \text{ otherwise}
\end{cases}
$$

Cost to place the order has some constant order cost $K$ and some cost per unit ordered $c$
$$
\text{Cost of order} = K + c Z_i
$$

Cost to hold unsold inventory overnight is some constant times the number of excess units, i.e. $ $h$ / unit held overnight.

Cost of being unable to meet demand (customers want more units than you actually have) is $ $p$ / unit that you lack.

No backlogs are allowed, and demand on day $i$ is $D_i$

Therefore,
$$
\begin{aligned}
\text{Profit}
&=
\text{Sales}
-
\text{Ordering Cost}
-
\text{Holding Cost}
-
\text{Penalty Cost}
\\[6pt]
&=
d \,
\min\!\left(
D_i,
\text{inventory at beginning of day } i
\right)
\\[6pt]
&\quad
-
\begin{cases}
K + cZ_i, & \text{if } I_i < s, \\[4pt]
0, & \text{otherwise}
\end{cases}
\\[10pt]
&\quad
-
hI_i
-
p\,
\max\!\left(
0,
D_i
-
\text{inventory at beginning of day } i
\right) \\
\\
&=  
d\,\min\!\left(D_i,\; I_{i-1}+Z_{i-1}\right)  
\\[8pt]  
&\quad  
-  
\begin{cases}  
K+cZ_i, & \text{if } I_i < s, \\[4pt]  
0, & \text{otherwise}  
\end{cases}  
\\[12pt]  
&\quad  
-  
hI_i  
-  
p\,\max\!\left(  
0,\;  
D_i-\left(I_{i-1}+Z_{i-1}\right)  
\right).  
\end{aligned}
$$
# Inventory Control Example

Given parameters

$$
d = 10,
\qquad
s = 3,
\qquad
S = 10,
\qquad
K = 2,
\qquad
c = 4,
\qquad
h = 1,
\qquad
p = 2.
$$

Demand sequence:

$$
D_1 = 5,
\quad
D_2 = 2,
\quad
D_3 = 8,
\quad
D_4 = 6,
\quad
D_5 = 2,
\quad
D_6 = 1.
$$

Initial inventory:

$$
I_0 + Z_0 = 10.
$$

The inventory policy is an $(s,S)$ policy:

$$
s = 3,
\qquad
S = 10.
$$

Whenever

$$
I_i < s,
$$

place an order

$$
Z_i = S - I_i.
$$

Otherwise,

$$
Z_i = 0.
$$

---

# Simulation Results

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c}
\text{Day}
&
\text{Begin Stock}
&
D_i
&
I_i
&
Z_i
&
\text{Sales Rev}
&
\text{Order Cost}
&
\text{Hold Cost}
&
\text{Penalty Cost}
&
\text{Total Rev}
\\
\hline
1 & 10 & 5 & 5 & 0 & 50 & 0 & -5 & 0 & 45 \\
2 & 5  & 2 & 3 & 0 & 20 & 0 & -3 & 0 & 17 \\
3 & 3  & 8 & 0 & 10 & 30 & -42 & 0 & -10 & -22 \\
4 & 10 & 6 & 4 & 0 & 60 & 0 & -4 & 0 & 56 \\
5 & 4  & 2 & 2 & 8 & 20 & -34 & -2 & 0 & -16 \\
6 & 10 & 1 & 9 & 0 & 10 & 0 & -9 & 0 & 1
\end{array}
$$

---

# Day 3 Calculation

Beginning inventory:

$$
3
$$

Demand:

$$
D_3 = 8
$$

Sales:

$$
\min(8,3)=3
$$

Sales revenue:

$$
10 \times 3 = 30
$$

Unsatisfied demand:

$$
8-3=5
$$

Penalty cost:

$$
-2(5)
=
-10
$$

Ending inventory:

$$
I_3
=
3-3
=
0
$$

Since

$$
I_3 < s
=
3,
$$

place an order

$$
Z_3
=
10-0
=
10.
$$

Ordering cost:

$$
-(K+cZ_3)
=
-(2+4(10))
=
-42.
$$

Total profit:

$$
30-42-10
=
-22.
$$

---

# Day 5 Calculation

Beginning inventory:

$$
4
$$

Demand:

$$
2
$$

Sales revenue:

$$
10 \times 2
=
20
$$

Ending inventory:

$$
I_5
=
4-2
=
2
$$

Holding cost:

$$
-1(2)
=
-2
$$

Since

$$
I_5 < 3,
$$

order

$$
Z_5
=
10-2
=
8.
$$

Ordering cost:

$$
-(2+4(8))
=
-34.
$$

Total profit:

$$
20-34-2
=
-16.
$$

---

# Total Profit Over 6 Days

$$
45
+
17
+
(-22)
+
56
+
(-16)
+
1
=
81.
$$

Therefore,

$$
\boxed{
\text{Total Profit}
=
81
}
$$

---

# Summary Statistics

Total demand:

$$
5+2+8+6+2+1
=
24
$$

Total units sold:

$$
5+2+3+6+2+1
=
19
$$

Lost sales:

$$
24-19
=
5
$$

Fill rate:

$$
\frac{19}{24}
=
0.792
$$

or

$$
79.2\%.
$$

---

Total ordering cost:

$$
42+34
=
76
$$

Total holding cost:

$$
5+3+0+4+2+9
=
23
$$

Total penalty cost:

$$
10
$$

Total sales revenue:

$$
50+20+30+60+20+10
=
190.
$$

Check:

$$
190
-
76
-
23
-
10
=
81.
$$
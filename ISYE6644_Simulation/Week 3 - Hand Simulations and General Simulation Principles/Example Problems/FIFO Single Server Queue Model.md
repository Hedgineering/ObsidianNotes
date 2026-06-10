First-In-First-Out model of queue.

Definitions:
## Single-Server Queue Notation

The interarrival time between customers $i-1$ and $i$ is

$$
I_i
$$

Customer $i$'s arrival time is the sum of inter-arrival times leading up to customer $i$'s arrival.

$$
A_i
=
\sum_{j=1}^{i} I_j
$$

Customer $i$ starts service at time that is larger of either their arrival, or departure of customer $i-1$

$$
T_i
=
\max\!\left(A_i,\; D_{i-1}\right)
$$

Customer $i$'s waiting time in the queue is difference between service start time $T_i$ and arrival time $A_i$

$$
W_i^Q
=
T_i - A_i
$$

Customer $i$'s time in the system is departure time $D_i$ minus arrival time $A_i$

$$
W_i
=
D_i - A_i
$$

Customer $i$'s service time aka service duration is (how long it takes to serve this customer)

$$
S_i
$$

Customer $i$'s departure time is sum between service start time and service duration

$$
D_i
=
T_i + S_i
$$

---

### Variable Definitions

| Variable | Meaning                                           |
| -------- | ------------------------------------------------- |
| $I_i$    | Interarrival time between customers $i-1$ and $i$ |
| $A_i$    | Arrival time of customer $i$                      |
| $T_i$    | Time customer $i$ begins service                  |
| $W_i^Q$  | Waiting time in queue                             |
| $S_i$    | Service time/service duration                     |
| $D_i$    | Departure time                                    |
| $W_i$    | Total time spent in the system                    |

---

### Intuition

Customer $i$ can only begin service when:

1. They have arrived ($A_i$), and
2. The previous customer has departed ($D_{i-1}$).

Therefore,

$$
T_i
=
\max(A_i, D_{i-1})
$$

If

$$
A_i > D_{i-1}
$$

the server sits idle waiting for customer $i$.

If

$$
A_i < D_{i-1}
$$

customer $i$ must wait in the queue.

## Example: Sequence of Events

Given the following interarrival and service times:

$$
\begin{array}{c|c|c}
i & I_i & S_i \\
\hline
1 & 3 & 7 \\
2 & 1 & 6 \\
3 & 2 & 4 \\
4 & 4 & 6 \\
5 & 5 & 1 \\
6 & 5 & 2
\end{array}
$$

Using

$$
A_i = \sum_{j=1}^{i} I_j
$$

$$
T_i = \max(A_i, D_{i-1})
$$

$$
W_i^Q = T_i - A_i
$$

$$
D_i = T_i + S_i
$$

we obtain:

$$
\begin{array}{c|c c|c c c|c}
i
&
I_i
&
A_i
&
T_i
&
W_i^Q
&
S_i
&
D_i
\\
\hline
1 & 3 & 3  & 3  & 0  & 7 & 10 \\
2 & 1 & 4  & 10 & 6  & 6 & 16 \\
3 & 2 & 6  & 16 & 10 & 4 & 20 \\
4 & 4 & 10 & 20 & 10 & 6 & 26 \\
5 & 5 & 15 & 26 & 11 & 1 & 27 \\
6 & 5 & 20 & 27 & 7  & 2 & 29
\end{array}
$$

### Sample Calculation

Customer 1:

$$
A_1 = I_1 = 3
$$

$$
T_1 = A_1 = 3
$$

$$
W_1^Q = 3 - 3 = 0
$$

$$
D_1 = 3 + 7 = 10
$$

---

Customer 2:

$$
A_2 = 3 + 1 = 4
$$

$$
T_2 = \max(4,10) = 10
$$

$$
W_2^Q = 10 - 4 = 6
$$

$$
D_2 = 10 + 6 = 16
$$

---

Customer 3:

$$
A_3 = 3 + 1 + 2 = 6
$$

$$
T_3 = \max(6,16) = 16
$$

$$
W_3^Q = 16 - 6 = 10
$$

$$
D_3 = 16 + 4 = 20
$$

---
# Example Statistics for the Single-Server Queue

From the previous table:

$$
\begin{array}{c|c|c|c}
i & A_i & D_i & W_i^Q \\
\hline
1 & 3 & 10 & 0 \\
2 & 4 & 16 & 6 \\
3 & 6 & 20 & 10 \\
4 & 10 & 26 & 10 \\
5 & 15 & 27 & 11 \\
6 & 20 & 29 & 7
\end{array}
$$

---

# Average Waiting Time in Queue

The waiting times are

$$
0,\;6,\;10,\;10,\;11,\;7
$$

Therefore

$$
\bar W_Q
=
\frac{1}{6}
\sum_{i=1}^{6}
W_i^Q
=
\frac{0+6+10+10+11+7}{6}
=
\frac{44}{6}
=
7.33
$$

---

# Number in the System

Let

$$
L(t)
=
\text{number of customers in the system}
$$

where "in the system" means

$$
\text{queue} + \text{service}.
$$

The only times that $L(t)$ can change are:

- arrivals
- departures

These times are called **events**.

---

## Event Table

$$
\begin{array}{c|l|c}
t & \text{event} & L(t) \\
\hline
0  & \text{simulation begins} & 0 \\
3  & \text{customer 1 arrives} & 1 \\
4  & \text{customer 2 arrives} & 2 \\
6  & \text{customer 3 arrives} & 3 \\
10 & \text{customer 1 departs, customer 4 arrives} & 3 \\
15 & \text{customer 5 arrives} & 4 \\
16 & \text{customer 2 departs} & 3 \\
20 & \text{customer 3 departs, customer 6 arrives} & 3 \\
26 & \text{customer 4 departs} & 2 \\
27 & \text{customer 5 departs} & 1 \\
29 & \text{customer 6 departs} & 0
\end{array}
$$

---

# Time-Average Number in the System

The average number in the system is the area under the $L(t)$ curve divided by the total simulation time.

$$
\bar L
=
\frac{1}{29}
\int_0^{29}
L(t)\,dt
$$

Compute the rectangular areas:

$$
(3-0)(0)
+
(4-3)(1)
+
(6-4)(2)
+
(15-6)(3)
+
(16-15)(4)
+
(26-16)(3)
+
(27-26)(2)
+
(29-27)(1)
$$

$$
=
0
+
1
+
4
+
27
+
4
+
30
+
2
+
2
=
70
$$

Therefore

$$
\bar L
=
\frac{70}{29}
=
2.414
$$

---

# Alternative Computation (Person-Time Method)

Notice that

$$
W_i
=
D_i - A_i
$$

is the total time customer $i$ spends in the system.

The total person-time in the system is

$$
\sum_{i=1}^{6}
(D_i-A_i)
$$

which equals

$$
(10-3)
+
(16-4)
+
(20-6)
+
(26-10)
+
(27-15)
+
(29-20)
$$

$$
=
7+12+14+16+12+9
=
70.
$$

Thus

$$
\bar L
=
\frac{\text{total person-time}}{\text{simulation length}}
=
\frac{70}{29}.
$$

This matches the area-under-the-curve calculation.

---

# Average Time in System

Using

$$
W_i = D_i - A_i
$$

we have

$$
W_1=7,\quad
W_2=12,\quad
W_3=14,\quad
W_4=16,\quad
W_5=12,\quad
W_6=9.
$$

Therefore

$$
\bar W
=
\frac{7+12+14+16+12+9}{6}
=
\frac{70}{6}
=
11.67.
$$

---

# Average Service Time

$$
\bar S
=
\frac{7+6+4+6+1+2}{6}
=
\frac{26}{6}
=
4.33.
$$

---

# Average Interarrival Time

$$
\bar I
=
\frac{3+1+2+4+5+5}{6}
=
\frac{20}{6}
=
3.33.
$$

---

# Estimated Arrival Rate

Using

$$
\lambda
=
\frac{\text{customers completed}}
{\text{simulation time}}
$$

gives

$$
\hat\lambda
=
\frac{6}{29}
=
0.207
\text{ customers/unit time}.
$$

---

# Server Utilization

The server is busy continuously from

$$
t=3
$$

until

$$
t=29.
$$

Total busy time:

$$
29-3=26.
$$

Thus

$$
\hat\rho
=
\frac{26}{29}
=
0.897.
$$

So the server is busy approximately

$$
89.7\%
$$

of the simulation.

---

# Little's Law Check

Little's Law states

$$
L = \lambda W.
$$

Using the estimates:

$$
\hat\lambda \hat W
=
\frac{6}{29}
\cdot
\frac{70}{6}
=
\frac{70}{29}
=
\hat L.
$$

Therefore

$$
\boxed{
\hat L
=
\hat\lambda \hat W
=
\frac{70}{29}
}
$$

which confirms Little's Law exactly for this sample path.
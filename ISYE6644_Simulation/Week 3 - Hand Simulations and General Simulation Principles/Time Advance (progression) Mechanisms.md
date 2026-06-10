How does the simulation clock move or progress?

Usually always moves forward, two common ways:
- **Fixed-Increment Time Advance** - update state at fixed times $nh, n = 0,1,2, ...$ with some appropriately selected timestep $h$. 
	- Often used in continuous time models like differential equations, and in models where data is only available at fixed times (e.g. end of day or end of month)
	- Wasteful if events are sparse in time (i.e. if nothing happens for a long duration, you're wasting compute simulating unnecessary idle gaps)
	
- **Next-Event Time Advance** - Clock is initialized to some $t_0$, all known future event times are determined in some **Future Events List (FEL)**, ordered by time and at each event the system state and FEL are updated.

Generally it's a good idea to use linked lists or skip lists for the FEL to efficiently use pointers in cases where you need to insert between FEL efficiently or add/remove sections of the list.

## Next-Event Time Advance Example

Consider a usual single-server FIFO queue that will process exactly $10$ customers.

$$
\begin{array}{c|cccccccccc}
\text{customer} & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 \\
\hline
\text{arrival time} & 1 & 3 & 4 & 10 & 17 & 18 & 19 & 20 & 27 & 29 \\
\text{service time} & 5 & 4 & 1 & 3 & 2 & 1 & 4 & 7 & 3 & 1
\end{array}
$$

where

$$
L_Q(t) = \text{number of customers waiting in queue at time } t
$$

$$
B(t) =
\begin{cases}
1, & \text{server is busy at time } t \\
0, & \text{server is idle at time } t
\end{cases}
$$

$$
\begin{array}{c|cc|c|c|cc}
\text{Clock } t
&
L_Q(t)
&
B(t)
&
\text{Queue }(\text{cust}, \text{arr time})
&
\text{FEL }(\text{event time}, \text{event type})
&
\text{busy time}
&
\text{time in sys}
\\
\hline
0 & 0 & 0 & \varnothing & (1, 1A) & 0 & 0 \\
1 & 0 & 1 & \varnothing & (3, 2A), (6, 1D) & 0 & 0 \\
3 & 1 & 1 & (2, 3) & (4, 3A), (6, 1D) & 2 & 2 \\
4 & 2 & 1 & (2, 3), (3, 4) & (6, 1D), (10, 4A) & 3 & 4 \\
6 & 1 & 1 & (3, 4) & (10, 2D), (10, 4A) & 5 & 10 \\
10 & 0 & 1 & \varnothing & (10, 4A), (11, 3D) & 9 & 18
\end{array}
$$
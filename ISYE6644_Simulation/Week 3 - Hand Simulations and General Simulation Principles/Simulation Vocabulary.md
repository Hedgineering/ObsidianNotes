**System:** collection of entities (people, machines, things, etc.) that interact together to accomplish some goal.

**Model:** abstract representation of a system, usually using math or some logical relationships that describe the system in terms of states, entities, sets, events, etc.

**System State:** a set of variables that contain enough information to describe the system at any point in time; like a "snapshot" of the system

e.g. in a single-server queue, all you may need to describe the state at some point in time $t$ are:
$$
\begin{align}
L_Q(t) &= \text{ \# of people in the queue at time }t \\
B(t) &= \begin{cases} 
		1, & \text{ if server is busy at time }t \\
		0, & \text{ if server is idle at time }t \\
		\end{cases}
\end{align}
$$
**Entities:** can be permanent (e.g. machine) or temporary (e.g. customers), and have various attributes (e.g. priority of a customer, avg. speed of a server, etc.)

**List (aka queue):** Some ordered list of associated entities (e.g. linked list, or line of people)

**Event:** *point in time* at which the system state changes (and which can't be predicted with certainty beforehand), and can also loosely refer to "what" happened

e.g. arrival event, departure event, machine breakdown, etc.

**Activity:** duration of time of *specified length* (e.g. unconditional wait)

e.g.
- exponential customer inter-arrival times
- constant service times

We can explicitly generate these events, so these are "specified" (we know the distribution from which these events are drawn)

**Conditional Wait**: A duration of time of *unspecified length*

e.g. a customer waiting time -- we don't know this directly, but can be determined with expectation or descriptive statistics using the simulation of activities and events.

!!!!!!!!!!!!!!
**Always collect arrival times and service times. We can always reverse-engineer waiting times based on arrival & service times.**
!!!!!!!!!!!!!!

**Simulation Clock**: Variable whose value represents simulated time (doesn't necessarily equal real time in real life)



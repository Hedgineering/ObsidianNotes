[[Monte Carlo]]

1. Take some function $f(x)$
2. Sample $n$ rectangles ($n$ is some LARGE number)
	- Each rectangle is centered randomly on $[0, 1]$
	- Each rectangle has width $\frac{1}{n}$
	- Each rectangle has height $f(x)$
3. Add up the areas

$$\int f(x) = \lim_{n \rightarrow \infty}\sum_{i=0}^n \frac{1}{n}f(x_i) \text{ , Where } x_i \sim \text{Uniform}(0, 1) $$
See more in [[RemoteSync/ISYE6644_Simulation/Week 3 - Hand Simulations and General Simulation Principles/Example Problems/Monte Carlo Integration|Monte Carlo Integration]]

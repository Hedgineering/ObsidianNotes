[[Monte Carlo]]

1. Draw a unit square (side length of 1)
2. Inscribe a circle within that square (radius of 1, centered in the square)
3. Randomly spawn points (or throw darts into that board)

$$\frac{\text{points in the circle}}{\text{total points}} = \frac{\pi}{4}$$
This is because

$$\text{Area of Circle} = \pi r^2$$
$$\text{Area of Square} = s^2$$

When $2r = s$ , e.g. when square side length is 1 and Circle inside it has radius of 0.5:
$$P(\text{random point in circle } | \text{ random point in square})=\frac{\text{Area of Circle}}{\text{Area of Square}} = \frac{\pi r^2}{s^2}=\frac{\pi r^2}{(2r)^2}=\frac{\pi r^2}{4r^2}=\frac{\pi}{4}$$

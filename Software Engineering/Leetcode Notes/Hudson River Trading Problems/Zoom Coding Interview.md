
# Red-Light Green-Light

You are given

L = final index that players want to reach e.g. `12`
W = watcher position e.g. `5`
p = player positions `[2, 4, 8]`
d = times when watcher changes direction e.g. `[3, 4]`
tf = final timestamp when simulation ends e.g. `10`

You are expected to output an integer that represents how many players can reach index L by the end time

All entities (players and watcher) move at 1 step/index per time step
Watcher begins the simulation facing Left (towards index 0)
If watcher is facing players (e.g. player pos < watcher pos and watcher facing left) then players under line of sight of watcher cannot take steps
If watcher is not facing a player, they can move
players are allowed to move if they are on the same index / position as the watcher

### Naive (fixed timestep) solution - $O(tf \cdot n)$ 
```py
def simulate_naive(L, W, p, d, tf, clamp_watcher=True):
    d_set = set(d)
    w = W
    direction = -1          # facing left
    players = list(p)

    for t in range(tf):
        if t in d_set:
            direction *= -1

        new_players = []
        for x in players:
            if x == w:
                nx = x + 1                       # on watcher's tile -> free
            elif direction == -1 and x < w:
                nx = x                            # frozen (watcher facing left, in shadow)
            elif direction == 1 and x > w:
                nx = x                            # frozen (watcher facing right, in shadow)
            else:
                nx = x + 1                        # not watched -> advance
            new_players.append(nx)
        players = new_players

        w += direction
        if clamp_watcher and w < 0:
            w = 0
    return sum(1 for x in players if x >= L)
```

### Efficient (event-driven / interval) solution $O(|d|\cdot n)$
```py
def simulate_efficient(L, W, p, d, tf):
    flips = sorted(set(t for t in d if 0 <= t < tf))
    boundaries = flips + [tf]

    w, direction, t_cur = W, -1, 0
    players = list(p)

    for b in boundaries:
        seg_len = b - t_cur
        if seg_len > 0:
            new_players = []
            for x in players:
                if direction == -1:
                    if x >= w:
                        nx = x + seg_len                       # free whole segment
                    else:
                        k0 = w - x                              # steps until watcher reaches x
                        nx = x if k0 >= seg_len else x + (seg_len - k0)
                else:  # direction == +1
                    if x <= w:
                        nx = x + seg_len
                    else:
                        k0 = x - w
                        nx = x if k0 >= seg_len else x + (seg_len - k0)
                new_players.append(nx)
            players = new_players
            w += direction * seg_len

        t_cur = b
        if b < tf:
            direction *= -1     # apply the flip scheduled at this boundary

    return sum(1 for x in players if x >= L)
```
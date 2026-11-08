
# Lab 2: BFS vs. DFS 

## 7. Compare BFS and DFS

Using the original maze and the original direction order:

| Algorithm | Path Length | Maximum Frontier |
|---|---:|---:|
| BFS | 8 | 3 |
| DFS | 10 | 5 |

### Answers

### 1. Which algorithm found the shorter path?
   - BFS found the shorter path.

### 2. Which algorithm had the larger maximum frontier?
   - DFS had the larger maximum frontier in this run (5 vs. 3).

### 3. Why did the two algorithms produce different paths on the same maze?
   - Because they manage the frontier differently:
     - BFS uses a queue (FIFO): it expands nodes in order of discovery by distance from the start.
     - DFS uses a stack (LIFO): it keeps going down one branch before backtracking.
   - This changes which route is explored first.

### 4. Does DFS always produce the shortest path? Why or why not?
   - No. DFS is not guaranteed to find the shortest path because it commits to one branch and may go deep before checking other branches.
   - It may find a longer route before a shorter one.

### 5. Does BFS always produce the shortest path when every movement costs 1? Why?
   - Yes. BFS explores nodes by increasing distance from the start.
   - The first time BFS reaches the goal, it is at the minimum number of moves from the start, so it guarantees the shortest path in an unweighted maze.

---

## 8. Experiment 1 — Change the Neighbor Order

Original DFS order:
- up, down, left, right

Changed order:
- right, down, left, up

### Answer

### Did the DFS path change?
- Yes, the DFS path usually changes when the neighbor order changes.

### Why?
- DFS explores the next neighbor in the order it appears in the list.
- Different orders cause the algorithm to follow different branches first, so it can find a different route to the goal.

### Did BFS’s path depend on this ordering the same way?
- Not in the same way.
- BFS is still layer-by-layer and still guarantees a shortest path length.
- Neighbor ordering can change which shortest path is chosen if there are multiple equally short paths, but BFS still explores by distance, not by branch depth.
- DFS is much more sensitive to neighbor ordering because it chooses one branch and keeps going.

***Note***: In many mazes, BFS may still return the same path length even if the exact route changes, as long as there are multiple shortest routes.

---

## 9. Experiment 2 — Change `start` and `goal`

Example results for several maze pairs:

| Start | Goal | BFS Length | BFS Max Frontier | DFS Length | DFS Max Frontier |
|---|---|---:|---:|---:|---:|
| (0, 0) | (2, 2) | 4 | 3 | 7 | 5 |
| (0, 0) | (4, 4) | 8 | 3 | 10 | 5 |
| (0, 4) | (4, 4) | 4 | 3 | 4 | 3 |

### Answers

### 6. Does BFS’s path length ever change if you re-run it with the same `start`/`goal`? Does DFS’s?
   - With the same start and goal and the same code, both algorithms are deterministic, so the path length will not change on repeated runs.
   - However, if the neighbor order changes, DFS may produce a different path length because its branch choice changes.
   - BFS does not usually change the path length for a fixed start/goal, because it always expands by distance.

### 7. As the distance between `start` and `goal` grows, what happens to the maximum frontier size for each algorithm?
   - BFS frontier tends to grow much more quickly as distance increases, because it expands all nodes at the current depth before moving deeper.
   - DFS frontier is often smaller at a given moment because it follows a single branch and backtracks, but in some mazes it can still become large.
   - In general:
     - BFS tends to have a larger frontier when the maze is wide and branching.
     - DFS usually keeps a smaller frontier, but can still grow substantially in complex graphs.

### 8. Can you find a `(start, goal)` pair where BFS and DFS return the same path? What does that tell you about the maze structure between those two points?
   - Yes. A simple example is:
     - start = (0, 4)
     - goal = (4, 4)
   - Both algorithms can follow the same open path along the right side of the maze.
   - This tells us that the maze structure is simple and there is not much branching between those two positions, so both search methods happen to choose the same route.

### 9. What happens if `goal` is unreachable from `start` (e.g. sealed off by walls)? Try it — what do `bfs()` and `dfs()` return?
   - They both return:
     - `None` for the path
     - plus the final frontier size reached before the search exhausted all possible nodes
   - In other words:
     - `bfs()` returns `(None, max_frontier)`
     - `dfs()` returns `(None, max_frontier)`
   - This means the algorithm searched the whole reachable region and never found the goal.

---

## Final takeaway

- BFS is best when you want the shortest path.
- DFS is useful for exploring deeply but may find a longer route.
- The real difference is this one line:
  - BFS: `frontier.popleft()`
  - DFS: `frontier.pop()`

That one change turns the same generic search into two very different strategies.

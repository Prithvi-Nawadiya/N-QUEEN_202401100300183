# N-QUEEN_202401100300183
# N-Queens Problem - Hill Climbing Algorithm

## Overview
This project solves the classic **N-Queens problem** using the **Hill Climbing algorithm** with sideways moves. The N-Queens problem is about placing N chess queens on an N×N chessboard such that no two queens threaten each other.

## Problem Explanation
- Each queen must be placed so that no two queens share the same **row**, **column**, or **diagonal**.
- For example:
  - **N = 4:** One solution is placing queens at positions (0,1), (1,3), (2,0), and (3,2).
  - **N = 8:** There are 92 distinct solutions for this classic problem.

## Approach - Hill Climbing Algorithm

### Steps Followed:
1. **Initial State:** Start with a random arrangement of queens (one in each row with a random column).  
2. **Heuristic Function:** Calculate the number of pairs of queens that are attacking each other.
3. **Neighbor Generation:** Move each queen to every other column in its row to generate neighbor states.
4. **Selection:** Choose the neighbor with the least number of conflicts. If multiple neighbors have the same score, pick one randomly.
5. **Sideways Move:** Allow a limited number of sideways moves (where the conflict score does not improve) to escape flat areas (plateaus).
6. **Termination:** The process stops if:
   - A solution is found (zero conflicts).
   - No better neighbor can be found.
   - Maximum allowed sideways moves are exhausted.

## Code
```python
import numpy as np
import random

def calculate_conflicts(state):
    conflicts = 0
    n = len(state)
    for i in range(n):
        for j in range(i + 1, n):
            if state[i] == state[j] or abs(state[i] - state[j]) == abs(i - j):
                conflicts += 1
    return conflicts

def get_neighbors(state):
    neighbors = []
    n = len(state)
    for row in range(n):
        for col in range(n):
            if col != state[row]:
                new_state = list(state)
                new_state[row] = col
                neighbors.append(new_state)
    return neighbors

def hill_climbing(N, max_sideways=100):
    state = list(np.random.permutation(N))
    current_conflicts = calculate_conflicts(state)
    sideways_moves = 0

    while True:
        neighbors = get_neighbors(state)
        neighbor_conflicts = [calculate_conflicts(neighbor) for neighbor in neighbors]
        min_conflict = min(neighbor_conflicts)
        best_neighbors = [neighbors[i] for i in range(len(neighbors)) if neighbor_conflicts[i] == min_conflict]
        next_state = random.choice(best_neighbors)

        if min_conflict < current_conflicts:
            state = next_state
            current_conflicts = min_conflict
            sideways_moves = 0
        elif min_conflict == current_conflicts:
            if sideways_moves < max_sideways:
                state = next_state
                sideways_moves += 1
            else:
                break
        else:
            break

        if current_conflicts == 0:
            return state

    return None

def print_board(state):
    N = len(state)
    board = np.full((N, N), '.', dtype=str)
    for row in range(N):
        board[row, state[row]] = 'Q'
    for line in board:
        print(" ".join(line))

def solve_n_queen(N):
    solution = hill_climbing(N)
    if solution:
        print(f"\nSolution for N={N}: {solution}")
        print_board(solution)
    else:
        print(f"\nNo solution found for N={N}")

# Testing for multiple N values
test_values = [4, 8, 12]
for N in test_values:
    print(f"\n=== Solving for N={N} ===")
    solve_n_queen(N)
```

## Sample Outputs

*Note: Replace this section with screenshots of your program output.*

- **N = 4:**
```
. Q . .
. . . Q
Q . . .
. . Q .
```

- **N = 8:**
```
Q . . . . . . .
. . . . Q . . .
. . . . . . Q .
. . . . . . . Q
. . Q . . . . .
. . . Q . . . .
. . . . . Q . .
. Q . . . . . .
```

- **N = 12:**
```
(Sample output will vary due to random starting points)
```

## Requirements
- Python 3.x
- Numpy library

## How to Run
1. Install Python and ensure `numpy` is available:
   ```bash
   pip install numpy
   ```
2. Run the Python script:
   ```bash
   python n_queen.py
   ```

## Limitations
- The algorithm can get stuck in local maxima (a good but not perfect solution).
- For larger values of N, it might take more time to find a solution.

## Possible Improvements
- Use random restarts to escape local maxima.
- Implement simulated annealing to explore the search space better.
- Optimize the neighbor selection process for faster results.



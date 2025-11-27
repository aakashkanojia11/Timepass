# Code for 8 puzzle problem using bradth first search 
from collections import deque

def print_state(state):
    for i in range(0, 9, 3):
        print(state[i:i+3])
    print()

def is_solvable(state):
    inv = 0
    arr = [x for x in state if x != 0]
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):
            if arr[i] > arr[j]:
                inv += 1
    return inv % 2 == 0

def get_neighbors(state):
    neighbors = []
    idx = state.index(0)
    x, y = divmod(idx, 3)
    moves = [(-1,0),(1,0),(0,-1),(0,1)]

    for dx, dy in moves:
        nx, ny = x+dx, y+dy
        if 0<=nx<3 and 0<=ny<3:
            nidx = nx*3 + ny
            new_state = list(state)
            new_state[idx], new_state[nidx] = new_state[nidx], new_state[idx]
            neighbors.append(tuple(new_state))
    return neighbors

def bfs(start, goal):
    if not is_solvable(start):
        return None

    queue = deque([(start, [])])
    visited = {start}

    while queue:
        state, path = queue.popleft()

        if state == goal:
            return path + [state]

        for nb in get_neighbors(state):
            if nb not in visited:
                visited.add(nb)
                queue.append((nb, path + [state]))
    return None

# Example
start = (1,2,3,
         4,0,5,
         6,7,8)

goal  = (1,2,3,
         4,5,6,
         7,8,0)

sol = bfs(start, goal)
if sol:
    print("BFS Solution found:")
    print("Solution found in", len(sol)-1, "moves\n")   # <-- Added line
    for s in sol:
        print_state(s)
else:
    print("No solution")


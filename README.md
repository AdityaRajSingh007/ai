# ai
# ==========================================
# 1. BFS using Fixed Graph
# ==========================================
def bfs_fixed():
    graph = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F'], 'D': [], 'E': [], 'F': []}
    visited, queue = set(), ['A']
    print("BFS Traversal:", end=" ")
    while queue:
        node = queue.pop(0)
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            queue.extend(graph[node])
    print()

# ==========================================
# 2. BFS using User Input
# ==========================================
def bfs_user():
    graph = {}
    n = int(input("Enter number of nodes: "))
    for _ in range(n):
        node, *neighbors = input("Enter node and neighbors (e.g., A B C): ").split()
        graph[node] = neighbors
    start = input("Enter start node: ")
    visited, queue = set(), [start]
    print("BFS Traversal:", end=" ")
    while queue:
        node = queue.pop(0)
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            queue.extend(graph.get(node, []))
    print()

# ==========================================
# 3. DFS using Fixed Graph
# ==========================================
def dfs_fixed():
    graph = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F'], 'D': [], 'E': [], 'F': []}
    visited, stack = set(), ['A']
    print("DFS Traversal:", end=" ")
    while stack:
        node = stack.pop()
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            stack.extend(reversed(graph[node]))
    print()

# ==========================================
# 4. DFS using User Input
# ==========================================
def dfs_user():
    graph = {}
    n = int(input("Enter number of nodes: "))
    for _ in range(n):
        node, *neighbors = input("Enter node and neighbors (e.g., A B C): ").split()
        graph[node] = neighbors
    start = input("Enter start node: ")
    visited, stack = set(), [start]
    print("DFS Traversal:", end=" ")
    while stack:
        node = stack.pop()
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            stack.extend(reversed(graph.get(node, [])))
    print()

# ==========================================
# 5. Tic Tac Toe
# ==========================================
def tic_tac_toe():
    board = [[' ' for _ in range(3)] for _ in range(3)]
    
    def print_board():
        for i, row in enumerate(board):
            print('|'.join(row))
            if i < 2: print('-'*5)
    
    def check_win(player):
        for i in range(3):
            if all(board[i][j] == player for j in range(3)) or all(board[j][i] == player for j in range(3)):
                return True
        return all(board[i][i] == player for i in range(3)) or all(board[i][2-i] == player for i in range(3))
    
    player = 'X'
    for turn in range(9):
        print_board()
        print(f"\nPlayer {player}'s turn")
        r, c = map(int, input("Enter row and column (0-2): ").split())
        if board[r][c] == ' ':
            board[r][c] = player
            if check_win(player):
                print_board()
                print(f"Player {player} wins!")
                return
            player = 'O' if player == 'X' else 'X'
        else:
            print("Invalid move!")
            turn -= 1
    print_board()
    print("It's a draw!")

# ==========================================
# 6. 8-Puzzle Problem
# ==========================================
def puzzle_8():
    from collections import deque
    
    def get_neighbors(state):
        neighbors, zero = [], state.index(0)
        moves = {'U': -3, 'D': 3, 'L': -1, 'R': 1}
        for move, delta in moves.items():
            new_pos = zero + delta
            if ((move == 'L' and zero % 3 != 0) or (move == 'R' and zero % 3 != 2) or 
                (move in ['U', 'D'] and 0 <= new_pos < 9)):
                new_state = list(state)
                new_state[zero], new_state[new_pos] = new_state[new_pos], new_state[zero]
                neighbors.append(tuple(new_state))
        return neighbors
    
    initial = tuple(map(int, input("Enter initial state (9 numbers, 0 for blank): ").split()))
    goal = tuple(map(int, input("Enter goal state (9 numbers, 0 for blank): ").split()))
    
    queue = deque([(initial, [initial])])
    visited = {initial}
    
    while queue:
        state, path = queue.popleft()
        if state == goal:
            print(f"\nSolution found in {len(path)-1} moves!")
            for step in path:
                print([step[i:i+3] for i in range(0, 9, 3)])
                print()
            return
        for neighbor in get_neighbors(state):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    print("No solution found!")

# ==========================================
# 7. Water Jug Problem
# ==========================================
def water_jug():
    from collections import deque
    
    jug1_cap = int(input("Enter Jug 1 capacity: "))
    jug2_cap = int(input("Enter Jug 2 capacity: "))
    target = int(input("Enter target amount: "))
    
    queue = deque([((0, 0), [])])
    visited = {(0, 0)}
    
    while queue:
        (j1, j2), path = queue.popleft()
        if j1 == target or j2 == target:
            print("\nSolution found!")
            for step in path + [(j1, j2)]:
                print(f"Jug1: {step[0]}, Jug2: {step[1]}")
            return
        
        states = [
            (jug1_cap, j2), (j1, jug2_cap), (0, j2), (j1, 0),
            (j1 - min(j1, jug2_cap - j2), j2 + min(j1, jug2_cap - j2)),
            (j1 + min(j2, jug1_cap - j1), j2 - min(j2, jug1_cap - j1))
        ]
        
        for state in states:
            if state not in visited:
                visited.add(state)
                queue.append((state, path + [(j1, j2)]))
    print("No solution found!")

# ==========================================
# Main Menu
# ==========================================
def main():
    while True:
        print("\n=== AI Lab Experiments ===")
        print("1. BFS using Fixed Graph")
        print("2. BFS using User Input")
        print("3. DFS using Fixed Graph")
        print("4. DFS using User Input")
        print("5. Tic Tac Toe")
        print("6. 8-Puzzle Problem")
        print("7. Water Jug Problem")
        print("0. Exit")
        
        choice = input("\nEnter your choice: ")
        
        if choice == '1': bfs_fixed()
        elif choice == '2': bfs_user()
        elif choice == '3': dfs_fixed()
        elif choice == '4': dfs_user()
        elif choice == '5': tic_tac_toe()
        elif choice == '6': puzzle_8()
        elif choice == '7': water_jug()
        elif choice == '0': break
        else: print("Invalid choice!")

if __name__ == "__main__":
    main()
# ==========================================
# Alpha Pruning
# ==========================================
8. Alphs Beta Puring
MAX, MIN = 1000, -1000
def minimax(depth, nodeIndex, maximizingPlayer, values, alpha, beta):
    if depth == 3:
        return values[nodeIndex]
    if maximizingPlayer:
        best = MIN
        for i in range(0, 2):
            val = minimax(depth + 1, nodeIndex * 2 + i, False, values, alpha, beta)
            best = max(best, val)
            alpha = max(alpha, best)
            if beta <= alpha:
                break
        return best
    else:
        best = MAX
        for i in range(0, 2):
            val = minimax(depth + 1, nodeIndex * 2 + i, True, values, alpha, beta)
            best = min(best, val)
            beta = min(beta, best)
            if beta <= alpha:
                break
        return best
if __name__ == "__main__":
    values = [3, 5, 6, 9, 1, 2, 0, -1]
    print("The optimal value is:", minimax(0, 0, True, values, MIN, MAX))

# ==========================================
# Monkey Banana
# ==========================================
i = 0
def Monkey_go_box(x, y):
    global i
    i = i + 1
    print('Step', i, ': Monkey at', x, 'goes to', y)
def Monkey_move_box(x, y):
    global i
    i = i + 1
    print('Step', i, ': Monkey takes the box from', x, 'and moves it to', y)
def Monkey_on_box():
    global i
    i = i + 1
    print('Step', i, ': Monkey climbs up the box')
def Monkey_get_banana():
    global i
    i = i + 1
    print('Step', i, ': Monkey picks the banana')
print("Enter monkey location:")
monkey = input().strip()
print("Enter banana location:")
banana = input().strip()
print("Enter box location:")
box = input().strip()
print('\nThe steps are as follows:')
Monkey_go_box(monkey, box)
Monkey_move_box(box, banana)
Monkey_on_box()
Monkey_get_banana()
print('\nSuccess! Monkey got the banana!')


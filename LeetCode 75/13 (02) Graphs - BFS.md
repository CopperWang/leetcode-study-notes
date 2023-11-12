# 0000 Graphs - BFS

---

在 LeetCode 或其他算法平台中，当题型分类中提到 "Graphs - BFS" 时，它指的是在图数据结构上应用宽度优先搜索（Breadth-First Search）算法。

**宽度优先搜索（BFS）的定义：**

- BFS 是一种用于图和树的遍历算法，它按层级顺序遍历节点。
- 这种算法使用队列来跟踪待访问的节点，确保每次从队列中取出节点进行探索时，都是按照它们被添加到队列中的顺序。

**BFS 适用的场景：**

1. **层级遍历**：BFS 可以用于一层层遍历图或树的节点，例如在树的层次遍历或者在图中按层次搜索节点。
2. **最短路径**：在无权图中，BFS 可以用来找到从起点到任意节点的最短路径（最少边数）。
3. **连通性**：BFS 可以检查两个节点是否连通，或者图中的所有节点是否都是连通的。
4. **组件搜索**：在图中使用 BFS 可以识别所有连通组件。
5. **图的广度探索**：如在网络爬虫或社交网络分析中，BFS 可以用来广度优先探索网络。

**BFS 算法的特点：**

- **时间复杂度**：对于图来说，BFS 的时间复杂度是 O(V + E)，其中 V 是节点数，E 是边数。
- **空间复杂度**：BFS 的空间复杂度可以达到 O(V)，因为在最坏的情况下，算法需要存储所有节点。

**Python 中使用 BFS 遍历图的一个基本示例：**

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    
    while queue:
        vertex = queue.popleft()
        if vertex not in visited:
            visited.add(vertex)
            # 将所有邻接但未访问过的节点加入队列
            queue.extend(set(graph[vertex]) - visited)
    return visited

# 假设 graph 是一个图的邻接表表示
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

# 从节点 'A' 开始进行 BFS 遍历
print(bfs(graph, 'A'))
```

在这个例子中，我们定义了一个简单的无向图，并从节点 'A' 开始进行 BFS 遍历。我们使用 `deque` 来实现队列，并使用 `visited` 集合来跟踪已经访问过的节点。

在实际的 LeetCode 问题中，BFS 常常用于解决上述提到的类型的问题，并且可能需要一些额外的逻辑来处理特定的问题场景。

# 1926 Nearest Exit from Entrance in Maze

---

**题目翻译：**

给你一个` m x n` 的矩阵迷宫（0索引），迷宫中有空格（用'.'表示）和墙（用'+'表示）。你还得到了迷宫的入口，其中 entrance = [entrancerow, entrancecol] 表示你最初站立的单元格的行和列。

一步之内，你可以向上、下、左、右移动一格。你不能走进墙所在的单元格，也不能走出迷宫。你的目标是找到从入口到最近出口的路径。出口被定义为迷宫边界上的空格。入口不算作出口。

返回从入口到最近出口的最短路径中的步数，如果没有这样的路径，返回 -1。

示例 1:
输入：maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
输出：1
解释：这个迷宫里有3个出口在 [1,0], [0,2], 和 [2,3]。
最初，你在入口单元格 [1,2]。
- 你可以通过向左移动2步到达 [1,0]。
- 你可以通过向上移动1步到达 [0,2]。
从入口到达 [2,3] 是不可能的。
因此，最近的出口是 [0,2]，距离是1步之遥。

示例 2:
输入：maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
输出：2
解释：这个迷宫里有1个出口在 [1,2]。
[1,0] 因为是入口单元格而不被算作出口。
最初，你在入口单元格 [1,0]。
- 你可以通过向右移动2步到达 [1,2]。
因此，最近的出口是 [1,2]，距离是2步之遥。

示例 3:
输入：maze = [[".","+"]], entrance = [0,0]
输出：-1
解释：这个迷宫中没有出口。

约束条件：
- maze.length == m
- maze[i].length == n
- 1 <= m, n <= 100
- maze[i][j] 要么是 '.' 要么是 '+'。
- entrance.length == 2
- 0 <= entrancerow < m
- 0 <= entrancecol < n
- entrance 总是一个空格。

解题思路和代码实现：
这个问题可以通过广度优先搜索（BFS）来解决。我们可以从入口开始，搜索所有可能的路径，直到找到一个出口或遍历完所有可能的路径。

以下是具体的Python代码实现，包括详细的中文注释：

```python
from collections import deque

class Solution:
    def nearestExit(self, maze: List[List[str]], entrance: List[int]) -> int:
        m, n = len(maze), len(maze[0])
        queue = deque([(entrance[0], entrance[1], 0)])  # 使用队列存储当前位置和步数
        maze[entrance[0]][entrance[1]] = '+'  # 将入口标记为已访问
        while queue:
            x, y, steps = queue.popleft()
            # 检查是否到达出口，出口不是入口且在迷宫边界上
            if (x != entrance[0] or y != entrance[1]) and (x == 0 or y == 0 or x == m-1 or y == n-1):
                return steps
            # 四个方向移动
            for dx, dy in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
                new_x, new_y = x + dx, y + dy
                # 检查新位置是否在迷宫内且不是墙
                if 0 <= new_x < m and 0 <= new_y < n and maze[new_x][new_y] == '.':
										queue.append((new_x, new_y, steps + 1))
                    maze[new_x][new_y] = '+'  # 标记新位置为已访问
        return -1  # 如果没有找到出口，则返回-1

```

在这段代码中，我们首先使用`deque`来创建一个队列，并将入口和初始步数0加入队列。然后进行广度优先搜索，每次从队列中取出一个位置并尝试四个方向移动。如果到达边界并且不是入口，则找到了出口，返回步数。在移动时，如果新位置是空格，则将其加入队列并标记为已访问。

该方法的时间复杂度为O(m*n)，其中m和n分别是迷宫的行数和列数，因为每个空格最多访问一次。空间复杂度为O(m*n)，在最坏情况下，队列中可能需要存储整个迷宫中所有的空格。

# 0994 Rotting Oranges

---

**题目翻译：**

给你一个 `m x n` 的网格，每个单元格可以有三个值之一：

- `0` 代表一个空单元格，
- `1` 代表一个新鲜橙子，
- `2` 代表一个腐烂的橙子。

每分钟，任何与腐烂橙子四方相邻的新鲜橙子都会变腐烂。

返回直到没有单元格有新鲜橙子为止所必须经过的最小分钟数。如果这是不可能的，返回 -1。

示例 1:
输入: grid = [[2,1,1],[1,1,0],[0,1,1]]
输出: 4

示例 2:
输入: grid = [[2,1,1],[0,1,1],[1,0,1]]
输出: -1
解释: 左下角的橙子（第2行，第0列）永远不会腐烂，因为腐烂只是四方向发生的。

示例 3:
输入: grid = [[0,2]]
输出: 0
解释: 由于在第 0 分钟时已经没有新鲜橙子了，答案就是 0。

约束条件：
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 10
- grid[i][j] 是 0、1 或 2。

**解题思路和代码实现：**

这个问题可以通过广度优先搜索（BFS）来解决。我们可以遍历网格，找到所有腐烂的橙子，并将它们作为BFS的起始点。然后每一轮BFS中，我们将与腐烂橙子相邻的新鲜橙子变成腐烂的，并记录轮数。如果最后还有新鲜橙子剩余，那么返回-1；否则返回轮数。

以下是具体的Python代码实现，包括详细的中文注释：

```python
from collections import deque

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        ROWS, COLS = len(grid), len(grid[0])
        fresh = 0
        rotten = deque()
        # 找到所有腐烂的橙子，并计算新鲜橙子的数量
        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == 1:
                    fresh += 1
                elif grid[r][c] == 2:
                    rotten.append((r, c))
        
        minutes_passed = 0
        # 四个方向的移动
        directions = [(1,0), (-1,0), (0,1), (0,-1)]
        # BFS
        while rotten and fresh > 0:
            minutes_passed += 1
            for _ in range(len(rotten)):
                x, y = rotten.popleft()
                for dr, dc in directions:
                    row, col = x + dr, y + dc
                    if (row < 0 or row == ROWS or 
                        col < 0 or col == COLS or 
                        grid[row][col] != 1):
                        continue
                    fresh -= 1
                    grid[row][col] = 2
                    rotten.append((row, col))
        
        # 如果还有新鲜橙子剩余，返回-1
        return minutes_passed if fresh == 0 else -1
```

在这段代码中，我们首先遍历整个网格，找到所有腐烂的橙子并将其坐标加入到队列`rotten`中，同时计算新鲜橙子的总数`fresh`。接下来，我们开始执行BFS，每一轮中，我们处理当前队列中所有的腐烂橙子，并将其四方向上的新鲜橙子变腐烂，然后将这些新变腐烂的橙子加入到队列中。每执行完一轮BFS，时间就加1。最后，如果没有新鲜橙子剩余，返回经过的分钟数；否则返回-1。

该方法的时间复杂度为O(m*n)，其中m和n分别是网格的行数和列数，因为我们需要遍历网格中的每个单元格。空间复杂度为O(m*n)，在最坏情况下，我们需要存储网格中所有的橙子。
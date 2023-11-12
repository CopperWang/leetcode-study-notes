# 0000 Graphs - DFS

---

在 LeetCode 或其他算法问题中提到的 "Graphs - DFS" 指的是在图结构上应用深度优先搜索（Depth-First Search）算法。

**深度优先搜索（DFS）定义：**

- 深度优先搜索是一种用于遍历或搜索树或图的算法。在遍历图时，DFS 会尽可能深地沿着分支前进，直到它到达了一个叶子节点或者没有其他节点可以访问，然后它回溯并探索其他分支。

**DFS 通常用于以下场景：**

1. **路径查找**：在图中找到从一个节点到另一个节点的路径。
2. **连通性检测**：检查图中两个节点是否连通，或者图是否完全连通。
3. **拓扑排序**：对有向无环图（DAG）进行排序，这在解决依赖问题（如任务调度）时非常有用。
4. **环检测**：检查图中是否存在环。
5. **组件发现**：找出图中所有连通组件。
6. **解决迷宫和拼图问题**：在需要探索多个可能的分支时使用 DFS。

**DFS 的实现：**

- DFS 可以通过递归方法或使用栈数据结构实现。
- 在递归实现中，算法会不断深入每个分支，直到不能再深入为止，然后回溯。
- 在栈实现中，算法会将节点放入栈中，然后不断地从栈中取出节点并探索。

**DFS 算法的特点：**
- **空间复杂度**：在最坏情况下，DFS 的空间复杂度可以达到 O(V)，其中 V 是图中节点的数量。
- **时间复杂度**：在无权图中，DFS 的时间复杂度为 O(V + E)，其中 E 是图中边的数量。

以下是使用 Python 实现图的 DFS 遍历的一个基本例子：

```python
# 假设我们有一个图，用邻接列表表示
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}

visited = set()  # 用于跟踪已经访问过的节点

def dfs(visited, graph, node):  # 递归函数
    if node not in visited:
        print(node)
        visited.add(node)
        for neighbour in graph[node]:
            dfs(visited, graph, neighbour)

# 调用函数并传入起始节点
dfs(visited, graph, 'A')
```

在这个例子中，我们从节点 'A' 开始对图进行深度优先遍历。这个特定的实现使用了递归方法，它是实现 DFS 的常见方式之一。在实际的 LeetCode 问题中，你可能需要处理更复杂的图，并且可能需要对算法进行额外的调整以解决具体的问题。

# 0841 Keys and Rooms

---

**题目翻译：**

有从 `0` 到 `n - 1` 编号的 `n` 个房间，除了房间 `0`，所有的房间都上锁了。你的目标是参观所有的房间。然而，如果没有钥匙，你不能进入一个上锁的房间。

当你参观一个房间时，你可能会在里面找到一组不同的钥匙。每个钥匙上都有一个数字，表示它可以解锁哪个房间，你可以带走它们中的所有钥匙来解锁其他房间。

给定一个数组`rooms`，其中`rooms[i]`是你如果参观了房间`i`可以获得的一组钥匙，如果你能参观所有房间，返回`true`，否则返回`false`。

示例 1:
输入: rooms = [[1],[2],[3],[]]
输出: true
解释: 
我们访问房间 0 并拿到钥匙 1。
然后我们访问房间 1 并拿到钥匙 2。
然后我们访问房间 2 并拿到钥匙 3。
然后我们访问房间 3。
由于我们能够访问每个房间，我们返回 true。

示例 2:
输入: rooms = [[1,3],[3,0,1],[2],[0]]
输出: false
解释: 我们不能进入房间号 2，因为解锁它的唯一钥匙在那个房间里。

约束条件：
- n == rooms.length
- 2 <= n <= 1000
- 0 <= rooms[i].length <= 1000
- 1 <= sum(rooms[i].length) <= 3000
- 0 <= rooms[i][j] < n
- rooms[i] 的所有值都是唯一的。

**解题思路和代码实现：**

这个问题可以通过深度优先搜索（DFS）或广度优先搜索（BFS）来解决。我们可以使用一个集合来记录已经访问过的房间，并从房间 0 开始探索，尝试使用每个房间里的钥匙去访问新的房间。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        # 使用一个集合来记录已经访问过的房间
        visited = set()
        
        def dfs(room):
            # 如果房间已经访问过，则返回
            if room in visited:
                return
            # 标记当前房间为已访问
            visited.add(room)
            # 使用当前房间中的钥匙去访问其他房间
            for key in rooms[room]:
                dfs(key)
        
        # 从房间 0 开始探索
        dfs(0)
        # 如果访问过的房间数量等于总房间数，则说明可以访问所有房间
        return len(visited) == len(rooms)
```

在这段代码中，我们定义了一个名为`canVisitAllRooms`的方法，并在其中定义了一个辅助的递归函数`dfs`来进行深度优先搜索。在`dfs`中，我们首先检查当前房间是否已经被访问过，如果没有，则将其添加到`visited`集合中，并使用当前房间里的所有钥匙尝试访问其他房间。最后，我们比较`visited`集合的大小和房间总数，如果相等，则表示可以访问所有房间。

该方法的时间复杂度为O(n + k)，其中n是房间数，k是钥匙的总数。空间复杂度为O(n)，因为需要一个集合来记录访问过的房间。

# 0547 Number of Provinces

---

**题目翻译：**

有 n 个城市，其中一些城市直接相连，另一些则不相连。如果城市 a 与城市 b 直接相连，城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

一个省份是一组直接或间接相连的城市，组外没有其他城市。

给定一个 n x n 的矩阵 isConnected，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，isConnected[i][j] = 0 表示不直接相连。

返回省份的总数。

示例 1:
输入: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出: 2

示例 2:
输入: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出: 3

约束条件：
- 1 <= n <= 200
- n == isConnected.length
- n == isConnected[i].length
- isConnected[i][j] 是 1 或 0。
- isConnected[i][i] == 1
- isConnected[i][j] == isConnected[j][i]

**解题思路和代码实现：**

这个问题可以通过深度优先搜索（DFS）或并查集（Union Find）来解决。我们可以通过遍历矩阵，使用 DFS 来深入每个城市，标记所有已经访问过的城市，以找出所有的省份。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(city):
            # 标记城市为已访问
            visited[city] = True
            # 访问所有与当前城市直接相连的城市
            for j in range(len(isConnected)):
                if isConnected[city][j] == 1 and not visited[j]:
                    dfs(j)
        
        n = len(isConnected)
        visited = [False] * n
        provinces = 0
        # 遍历所有城市
        for i in range(n):
            # 如果城市尚未访问，说明找到了一个新的省份
            if not visited[i]:
                dfs(i)
                provinces += 1
        return provinces
```

在这段代码中，我们定义了一个名为`findCircleNum`的方法，并在其中定义了一个辅助的递归函数`dfs`来进行深度优先搜索。在`dfs`中，我们标记当前城市为已访问，并对所有与之直接相连的城市递归地调用`dfs`。我们使用一个`visited`列表来记录已经访问过的城市。对于每个尚未访问的城市，我们调用`dfs`，每次调用都将省份计数`provinces`加一。

该方法的时间复杂度为O(n^2)，其中n是城市的数量，因为我们需要遍历整个矩阵来找出所有省份。空间复杂度为O(n)，由于需要一个与城市数量相同大小的`visited`列表来记录访问状态。

# 1466 Reorder Routes to Make All Paths Lead to the City Zero

---

**题目翻译：**

有从 0 到 n - 1 编号的 n 个城市和 n - 1 条道路，这些道路保证了任意两个不同城市间只有一条路径可以互通（这些道路构成了一棵树）。去年，交通部决定将这些道路定向为单行，因为它们太窄了。

道路由 `connections` 表示，其中`connections[i] = [ai, bi]`表示一条从城市 `ai` 到城市 `bi` 的道路。

今年，首都（城市 `0`）将举办一场大型活动，许多人想要前往这个城市。

你的任务是重新定向一些道路，以便每个城市都可以到达城市 0。返回改变方向的最少边数。

保证在重新定向后，每个城市都能到达城市 `0`。

示例 1:
输入: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
输出: 3
解释: 改变红色箭头所示的边的方向，使得每个节点都能到达节点 0（首都）。

示例 2:
输入: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
输出: 2
解释: 改变红色箭头所示的边的方向，使得每个节点都能到达节点 0（首都）。

示例 3:
输入: n = 3, connections = [[1,0],[2,0]]
输出: 0

约束条件：
- 2 <= n <= 5 * 10^4
- connections.length == n - 1
- connections[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi

**解题思路和代码实现：**

这个问题可以通过深度优先搜索（DFS）来解决。我们可以构建一个图，表示每个城市可达的城市和需要改变方向的道路。然后从首都开始，递归地访问每个城市，如果需要改变方向才能到达，就增加计数器。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def minReorder(self, n: int, connections: List[List[int]]) -> int:
        roads = set()  # 保存需要改变方向的道路
        graph = defaultdict(list)  # 建立图的邻接表
        for a, b in connections:
            roads.add((a, b))
            graph[a].append(b)
            graph[b].append(a)
        
        def dfs(city, parent):
            count = 0
            for neighbor in graph[city]:
                if neighbor == parent:
                    continue  # 避免反向访问父节点
                if (city, neighbor) in roads:
                    count += 1  # 需要改变方向
                count += dfs(neighbor, city)  # 递归访问邻居
            return count
        
        return dfs(0, -1)  # 从首都开始访问，首都没有父节点，所以父节点设为-1
```

在这段代码中，我们首先创建一个`roads`集合来保存所有需要改变方向的道路，然后创建一个`graph`字典来建立每个城市与其邻居的邻接表。在`dfs`函数中，我们递归地访问每个城市的邻居，如果当前道路需要改变方向，我们就将计数器增加1，并且累加递归访问邻居的结果。

该方法的时间复杂度为O(n)，其中n是城市的数量，因为我们需要遍历所有的城市。空间复杂度为O(n)，用于存储图的邻接表和需要改变方向的道路集合。

# 0399 Evaluate Division

---

**题目翻译：**

给你一个变量对数组`equations`和一个实数数组`values`，其中`equations[i] = [Ai, Bi]`和`values[i]`共同表示等式`Ai / Bi = values[i]`。每个`Ai`或`Bi`是一个表示单个变量的字符串。

你还会得到一些查询，其中`queries[j] = [Cj, Dj]`代表第`j`个查询，在这个查询中你必须找到`Cj / Dj = ?`的答案。

返回所有查询的答案。如果无法确定单个答案，请返回`-1.0`。

注意：输入总是有效的。你可以假设评估查询不会导致除以零，并且没有矛盾。

注意：不在等式列表中出现的变量是未定义的，因此无法为它们确定答案。

示例 1:
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
给定：a / b = 2.0, b / c = 3.0
查询是：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
返回：[6.0, 0.5, -1.0, 1.0, -1.0 ]
注意：x 是未定义的 => -1.0

示例 2:
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]

示例 3:
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]

约束条件：
- 1 <= equations.length <= 20
- equations[i].length == 2
- 1 <= Ai.length, Bi.length <= 5
- values.length == equations.length
- 0.0 < values[i] <= 20.0
- 1 <= queries.length <= 20
- queries[i].length == 2
- 1 <= Cj.length, Dj.length <= 5
- Ai, Bi, Cj, Dj 由小写英文字母和数字组成。

**解题思路和代码实现：**

这个问题可以通过构建图和深度优先搜索（DFS）来解决。我们可以构建一个图，图的节点是变量，边是变量之间的比率。然后对于每个查询，我们在图中搜索从`Cj`到`Dj`的路径，并计算路径上的权值乘积。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        graph = defaultdict(dict)
        # 构建图
        for (x, y), value in zip(equations, values):
            graph[x][y] = value
            graph[y][x] = 1.0 / value
        
        # 深度优先搜索函数
        def dfs(x, y, visited):
            if x not in graph or y not in graph:
                return -1.0
            if y in graph[x]:
                return graph[x][y]
            for i in graph[x]:
                if i not in visited:
                    visited.add(i)
                    temp = dfs(i, y, visited)
                    if temp == -1.0:
                        continue
                    return graph[x][i] * temp
            return -1.0
        
        # 对每个查询执行深度优先搜索
        results = []
        for query in queries:
            results.append(dfs(query[0], query[1], set()))
        return results
```

在这段代码中，我们首先构建了一个图`graph`，图中的每个节点是一个变量，每个节点的邻接节点是与之有直接比率关系的变量，边的权值是比率。`dfs`函数用于深度优先搜索从变量`x`到变量`y`的路径，如果找到这样的路径，则返回路径上的权值乘积；如果没有找到，返回`-1.0`。对于每个查询，我们使用`dfs`函数搜索路径，并将结果添加到`results`列表中。

该方法的时间复杂度为O(Q * (V + E))，其中Q是查询的数量，V是变量的数量，E是等式的数量。空间复杂度为O(V + E)，用于存储图。
# 0000 Binary Tree - BFS

---

在 LeetCode 或其他算法相关的讨论中，当提到二叉树（Binary Tree）的 BFS，它指的是宽度优先搜索（Breadth-First Search）算法的应用。BFS 是一种遍历或搜索树和图的算法，它从根节点开始，一层层向下遍历，也就是说，它首先访问根节点的所有直接子节点，然后访问所有子节点的子节点，依此类推。

BFS 在二叉树中的应用通常涉及到使用队列来保持跟踪下一个要访问的节点。对于二叉树，这意味着按层次顺序访问节点。

**二叉树的 BFS 适用场景：**

1. **层次遍历**：如果你想逐层打印或处理二叉树中的节点，BFS 是一个理想的选择。

2. **查找最短路径**：在树形结构中，BFS 可以用来找到从根节点到目标节点的最短路径，因为它总是优先访问距离根节点近的节点。

3. **序列化和反序列化二叉树**：BFS 可以用来按层次顺序序列化一个二叉树，这样可以方便地反序列化重建该二叉树。

4. **最小深度**：BFS 能够高效地找到二叉树的最小深度，因为它会在找到第一个叶子节点时停止搜索。

5. **检查完全性**：检查一个二叉树是否是完全二叉树时，BFS 可以用来确保所有层（除了最后一层）都是满的，并且最后一层的节点都尽可能地靠左排列。

**Python 中使用 BFS 遍历二叉树的例子：**

```python
from collections import deque

class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

def bfs_traversal(root):
    if not root:
        return []
    queue = deque([root])
    result = []
    while queue:
        level_size = len(queue)
        current_level = []
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.value)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(current_level)
    return result
```

在这个例子中，我们使用 `deque` 从 Python 的 `collections` 模块创建一个双端队列作为我们的队列。我们从根节点开始，并将其子节点按顺序添加到队列中，以确保我们能够按层次顺序访问每个节点。

# 0199 Binary Tree Right Side View

---

**题目翻译：**

给定一个二叉树的根节点`root`，想象你站在它的**右侧**，返回你从上到下能看到的节点的值。

示例 1:
输入: root = [1,2,3,null,5,null,4]
输出: [1,3,4]

示例 2:
输入: root = [1,null,3]
输出: [1,3]

示例 3:
输入: root = []
输出: []

约束条件：
- 树中节点的数量范围是 [0, 100]。
- -100 <= Node.val <= 100

**解题思路和代码实现：**

这个问题可以通过层序遍历（BFS）来解决，我们可以使用队列来实现层序遍历，每次遍历一层的节点，并记录每层的最右侧节点的值。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        queue = [root]  # 使用列表实现队列，存储每层的节点
        rightside = []  # 存储每层最右侧节点的值
        
        while queue:
            level_length = len(queue)
            for i in range(level_length):
                node = queue.pop(0)  # 弹出队列的第一个节点
                
                # 如果是当前层的最后一个节点，将其值添加到rightside列表中
                if i == level_length - 1:
                    rightside.append(node.val)
                
                # 将当前节点的左右子节点加入队列
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return rightside
```

在这段代码中，我们首先检查根节点是否为空。然后，我们使用一个队列来存储树的每一层的节点，并进行层序遍历。在遍历每一层时，我们检查每个节点是否是当前层的最后一个节点（即右侧视图能看到的节点），如果是，则将其值添加到结果列表中。对于每个节点的左右子节点，如果它们不为空，则将它们加入队列中，以便下一次迭代遍历。

该方法的时间复杂度为O(n)，其中n是树中的节点数量，因为我们需要访问树中的每个节点。空间复杂度为O(d)，其中d是树的直径，这是因为队列中最多同时存储两层节点的数量。在最坏的情况下，这个值接近树中的节点数。

# 1161 Maximum Level Sum of a Binary Tree

---

**题目翻译：**

给定一个二叉树的根节点，根的级别为`1`，它的孩子的级别为`2`，依此类推。

返回**最小**的级别`x`，使得该级别`x`上所有节点值的和**最大**。

示例 1:
输入：root = [1,7,0,7,-8,null,null]
输出：2
解释：
第1级的和 = 1。
第2级的和 = 7 + 0 = 7。
第3级的和 = 7 + -8 = -1。
因此我们返回最大和的级别，即第2级。

示例 2:
输入：root = [989,null,10250,98693,-89388,null,null,null,-32127]
输出：2

约束条件：
- 树中的节点数在范围 [1, 10^4] 内。
- -10^5 <= Node.val <= 10^5

**解题思路和代码实现：**

这个问题可以通过层序遍历（BFS）来解决。我们可以使用队列来实现层序遍历，每次遍历一层的节点，并记录每层节点的值的总和，然后找出总和最大的层。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        queue = [(root, 1)]  # 队列中存储节点及其所在层级
        max_sum = float('-inf')  # 初始化最大和为负无穷
        min_level = 0  # 最大和所在的层级
        
        while queue:
            level_size = len(queue)  # 当前层的节点数量
            level_sum = 0  # 当前层的节点值总和
            for _ in range(level_size):
                node, level = queue.pop(0)
                level_sum += node.val
                # 将下一层的节点加入队列
                if node.left:
                    queue.append((node.left, level+1))
                if node.right:
                    queue.append((node.right, level+1))
            
            # 如果当前层的总和大于最大和，更新最大和及其层级
            if level_sum > max_sum:
                max_sum = level_sum
                min_level = level
        
        return min_level
```

在这段代码中，我们使用了一个队列来存储每个节点及其对应的层级，并进行层序遍历。对于每一层的节点，我们计算这一层所有节点的值的和，并与之前记录的最大和进行比较，如果更大，则更新最大和及其对应的层级。遍历完成后，我们返回具有最大和的层级。

该方法的时间复杂度为O(n)，其中n是树中的节点数量，因为我们需要遍历树中的每个节点。空间复杂度为O(w)，其中w是树的最大宽度，这是因为队列中最多同时存储一层节点的数量。在最坏的情况下，这个值接近树中的节点数。


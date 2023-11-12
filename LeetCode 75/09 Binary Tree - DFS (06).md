# 0000 Binary Tree - DFS

---

在 LeetCode 或其他算法平台中提到的 "Binary Tree" 指的是一种树形数据结构，其中每个节点最多有两个子节点，分别称为左子节点和右子节点。二叉树是数据结构中的一种基础概念，经常用于表示具有层次结构的数据。

"DFS" 是深度优先搜索（Depth-First Search）的缩写，是一种用于遍历或搜索树或图的算法。在二叉树的上下文中，DFS 可以进一步分为三种遍历方式：

1. **前序遍历（Pre-order Traversal）**：首先访问根节点，然后递归地进行左子树的前序遍历，最后递归地进行右子树的前序遍历。

2. **中序遍历（In-order Traversal）**：首先递归地进行左子树的中序遍历，然后访问根节点，最后递归地进行右子树的中序遍历。对于二叉搜索树（BST），中序遍历将以升序访问所有节点。

3. **后序遍历（Post-order Traversal）**：首先递归地进行左子树的后序遍历，然后递归地进行右子树的后序遍历，最后访问根节点。

DFS 适用于许多情况，包括：

- 检查二叉树中是否存在某个值。
- 查找二叉树中的最大或最小值。
- 计算二叉树的高度。
- 序列化和反序列化二叉树。
- 克隆或复制二叉树。
- 找到二叉树中的某个路径或总和。

由于深度优先搜索可以访问树的所有节点，并按照特定的顺序进行，它是解决树相关问题的一个非常有用的工具。DFS 算法可以使用递归或栈来实现。

下面是一个用 Python 实现二叉树的前序遍历的示例：

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def preorderTraversal(root):
    if root is None:
        return []
    stack, output = [root, ], []
    while stack:
        node = stack.pop()
        if node is not None:
            output.append(node.val)
            if node.right is not None:
                stack.append(node.right)
            if node.left is not None:
                stack.append(node.left)
    return output
```

在这个例子中，我们定义了一个 `TreeNode` 类来表示二叉树的节点，并使用一个栈来实现非递归的前序遍历。

# 0104 Maximum Depth of Binary Tree

---

**题目翻译：**

给定一个二叉树的根节点 root，返回其最大深度。

二叉树的最大深度是从根节点到最远叶子节点的最长路径上的节点数量。

示例 1:
输入：root = [3,9,20,null,null,15,7]
输出：3

示例 2:
输入：root = [1,null,2]
输出：2

约束条件：
- 树中的节点数的范围是 [0, 10^4]。
- -100 <= Node.val <= 100

**解题思路和代码实现：**

这是一个典型的递归问题。二叉树的最大深度可以通过递归地求解左右子树的最大深度然后取最大值再加1来得到。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # 如果节点为空，则返回深度为0
        if not root:
            return 0
        
        # 递归计算左子树的最大深度
        leftSubtreeDepth = self.maxDepth(root.left)
        # 递归计算右子树的最大深度
        rightSubtreeDepth = self.maxDepth(root.right)
        # 取左右子树深度的最大值，并加上当前节点的深度1
        return max(leftSubtreeDepth, rightSubtreeDepth) + 1
```

在这段代码中，我们定义了一个`TreeNode`类来表示二叉树的节点，然后在`Solution`类中定义了一个`maxDepth`方法来递归地求解最大深度。对于每一个非空节点，我们计算其左右子树的最大深度，然后取两者之间的最大值并加1来得到以当前节点为根的子树的最大深度。

该方法的时间复杂度为O(n)，其中n是树中的节点数，因为每个节点都需要访问一次。空间复杂度为O(height)，其中height是树的高度，这是因为递归时栈的深度取决于树的高度。在最坏的情况下，树是线性的，递归的深度为树的高度。

# 0872 Leaf-Similar Trees

---

**题目翻译：**

考虑一个二叉树的所有叶子节点，从左到右的顺序，这些叶子的值形成一个叶值序列。

例如，在上面给定的树中，叶值序列是`（6, 7, 4, 9, 8）`。

如果两个二叉树的叶值序列相同，那么这两个二叉树被认为是叶相似的。

如果两个给定的树的头节点分别是`root1`和`root2`，并且它们是叶相似的，返回`true`。

示例 1:
输入: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
输出: true

示例 2:
输入: root1 = [1,2,3], root2 = [1,3,2]
输出: false

约束条件：
- 每棵树中的节点数将在范围 [1, 200] 内。
- 给定的两棵树的值将在范围 [0, 200] 内。

**解题思路和代码实现：**

我们可以对两棵树分别进行深度优先搜索（DFS），收集叶子节点的值。如果两个序列完全相同，则返回`true`；否则返回`false`。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        def dfs(node):
            # 如果当前节点是叶子节点，返回节点值构成的列表
            if not node.left and not node.right:
                return [node.val]
            # 否则递归左右子树，合并结果
            leaves = []
            if node.left:
                leaves += dfs(node.left)
            if node.right:
                leaves += dfs(node.right)
            return leaves
        
        # 分别收集两棵树的叶值序列
        leaves1 = dfs(root1)
        leaves2 = dfs(root2)
        
        # 如果两棵树的叶值序列相同，则返回true；否则返回false
        return leaves1 == leaves2
```

在这段代码中，我们定义了一个辅助函数`dfs`来递归地进行深度优先搜索，并收集叶子节点的值。然后，我们分别对`root1`和`root2`执行这个函数，并比较得到的叶值序列是否相同。

该方法的时间复杂度为O(n)，其中n是树中节点的数量，因为每个节点最多被访问一次。空间复杂度为O(h)，其中h是树的高度，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为链状，空间复杂度为O(n)。

# 1448 Count Good Nodes in Binary Tree

---

**题目翻译：**

给定一个二叉树的根节点`root`，如果从根节点到`X`的路径上没有节点的值大于`X`的值，那么树中的节点`X`就被称为 **好节点**。

返回二叉树中 **好节点** 的数量。

示例 1:
输入: root = [3,1,4,3,null,1,5]
输出: 4
解释: 蓝色节点是好节点。
根节点（3）总是一个好节点。
节点 4 -> (3,4) 是从根开始路径上的最大值。
节点 5 -> (3,4,5) 是路径上的最大值。
节点 3 -> (3,1,3) 是路径上的最大值。

示例 2:
输入: root = [3,3,null,4,2]
输出: 3
解释: 节点 2 -> (3, 3, 2) 不是好节点，因为它的路径上有一个值“3”比它大。

示例 3:
输入: root = [1]
输出: 1
解释: 根被认为是好节点。

约束条件：
- 二叉树中的节点数在[1, 10^5]范围内。
- 每个节点的值在[-10^4, 10^4]之间。

**解题思路和代码实现：**

这道题可以通过深度优先搜索（DFS）来解决。我们需要在搜索过程中记录从根节点到当前节点路径上的最大值，然后与当前节点的值进行比较。如果当前节点的值不小于该路径上的最大值，则它是一个好节点。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        def dfs(node, max_val):
            # 如果节点为空，则返回0
            if not node:
                return 0
            
            # 如果当前节点的值大于等于路径上的最大值，则是好节点
            res = 1 if node.val >= max_val else 0
            
            # 更新路径上的最大值
            max_val = max(max_val, node.val)
            
            # 递归左子树和右子树，并累加好节点的数量
            res += dfs(node.left, max_val)
            res += dfs(node.right, max_val)
            return res
        
        # 从根节点开始递归，初始最大值为根节点的值
        return dfs(root, root.val)
```

在这段代码中，我们定义了一个辅助函数`dfs`，它接受当前节点和当前路径上的最大值作为参数。对于每个节点，我们都检查它是否是一个好节点，然后递归地调用其左子节点和右子节点，并累加结果。

该方法的时间复杂度为O(n)，其中n是树中的节点数量，因为每个节点都需要访问一次。空间复杂度为O(h)，其中h是树的高度，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为线性的，空间复杂度为O(n)。

# 0437 Path Sum III

---

**题目翻译：**

给定一个二叉树的根节点`root`和一个整数`targetSum`，返回沿路径上值之和等于`targetSum`的路径数量。

路径不需要从根开始或在叶子结束，但必须向下（即，只能从父节点到子节点）。

示例 1:
输入: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出: 3
解释: 和为8的路径如图所示。

示例 2:
输入: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出: 3

约束条件：
- 树中的节点数在[0, 1000]的范围内。
- -10^9 <= Node.val <= 10^9
- -1000 <= targetSum <= 1000

**解题思路和代码实现：**

这个问题可以通过深度优先搜索（DFS）来解决。我们需要遍历树中的每个节点，并对每个节点，我们都从该节点开始尝试所有可能的向下路径，计算这些路径上节点值的和是否等于`targetSum`。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        def dfs(node, curr_sum):
            if not node:
                return 0

            # 计算包含当前节点的路径和
            curr_sum += node.val

            # 如果当前路径和等于目标和，则计数加1，否则为0
            path_count = 1 if curr_sum == targetSum else 0

            # 递归左右子树，并累加路径计数
            path_count += dfs(node.left, curr_sum)
            path_count += dfs(node.right, curr_sum)

            return path_count

        if not root:
            return 0

        # 从每个节点出发，计算路径和
        return (dfs(root, 0) +
                self.pathSum(root.left, targetSum) +
                self.pathSum(root.right, targetSum))
```

在这段代码中，我们定义了一个辅助函数`dfs`来递归地检查从当前节点开始的所有向下路径，看这些路径上节点值的和是否等于`targetSum`。然后，对于每个节点，我们都调用`dfs`来计算以该节点为起点的路径，同时也对该节点的左右子节点递归调用`pathSum`函数。

该方法的时间复杂度为O(n^2)在最坏的情况下，因为对于每个节点，我们都可能需要遍历整棵树。空间复杂度为O(h)，其中h是树的高度，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为链状，空间复杂度为O(n)。

# 1372 Longest ZigZag Path in a Binary Tree

---

**题目翻译：**

给定一个二叉树的根节点`root`。

二叉树的ZigZag路径定义如下：

1. 在二叉树中选择任意节点和一个方向（右或左）。
2. 如果当前方向是右，则移动到当前节点的右子节点；否则，移动到左子节点。
3. 从右到左或从左到右改变方向。
4. 重复第二和第三步，直到你不能在树中移动为止。

ZigZag长度定义为访问的节点数 - 1。（单个节点的长度为0）。

返回该树中包含的最长ZigZag路径。

示例 1:
输入：root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1]
输出：3
解释：蓝色节点中的最长ZigZag路径（右 -> 左 -> 右）。

示例 2:
输入：root = [1,1,1,null,1,null,null,1,1,null,1]
输出：4
解释：蓝色节点中的最长ZigZag路径（左 -> 右 -> 左 -> 右）。

示例 3:
输入：root = [1]
输出：0

约束条件：
- 树中的节点数的范围是 [1, 5 * 10^4]。
- 1 <= Node.val <= 100

**解题思路和代码实现：**

这个问题可以通过递归的方法来解决。我们需要在递归过程中记录每个节点开始的左右ZigZag路径长度，并在全局维护一个最长路径长度。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def longestZigZag(self, root: Optional[TreeNode]) -> int:
        # 初始化最长ZigZag路径长度为0
        self.longest = 0

        def dfs(node, direction, length):
            # 如果节点为空，则返回
            if not node:
                return
            
            # 更新全局最长ZigZag路径长度
            self.longest = max(self.longest, length)
            
            # 如果当前方向是左，则递归计算从右子节点开始的ZigZag路径长度
            if direction == 'left':
                dfs(node.left, 'right', length + 1)
                dfs(node.right, 'left', 1)
            # 如果当前方向是右，则递归计算从左子节点开始的ZigZag路径长度
            else:
                dfs(node.right, 'left', length + 1)
                dfs(node.left, 'right', 1)
        
        # 从根节点的左右子节点开始递归，初始方向分别为左和右，初始长度为1
        dfs(root.left, 'right', 1)
        dfs(root.right, 'left', 1)
        
        return self.longest
```

在这段代码中，我们定义了一个辅助函数`dfs`来递归地遍历树，并计算从每个节点开始的ZigZag路径长度。`dfs`函数接受当前节点、当前方向和当前路径长度作为参数。当我们移动到子节点时，我们改变方向并增加路径长度。如果当前节点为空，我们返回并不再递归。每次递归时，我们都会检查并更新全局最长ZigZag路径长度。

该方法的时间复杂度为O(n)，其中n是树中的节点数量，因为我们需要访问树中的每个节点。空间复杂度为O(h)，其中h是树的高度，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为链状，空间复杂度为O(n)。

# 0236 Lowest Common Ancestor of a Binary Tree

---

**题目翻译：**

给定一个二叉树，找到树中两个给定节点的最低公共祖先（LCA）。

根据维基百科上对LCA的定义：“两个节点p和q的最低公共祖先是T中同时具有p和q作为后代的最低节点（我们允许一个节点是其自身的后代）”。

示例 1:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点5和节点1的LCA是3。

示例 2:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点5和节点4的LCA是5，因为根据LCA定义，一个节点可以是其自身的后代。

示例 3:
输入: root = [1,2], p = 1, q = 2
输出: 1

约束条件：
- 树中的节点数的范围是[2, 10^5]。
- -10^9 <= Node.val <= 10^9
- 所有Node.val都是独一无二的。
- p != q
- p和q将存在于树中。

**解题思路和代码实现：**

这个问题可以通过递归方法来解决。递归地在二叉树中查找节点p和q，如果在某个节点的左右子树中分别找到了节点p和q，那么这个节点就是p和q的最低公共祖先。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 如果到达叶子节点或找到p或q，则返回当前节点
        if not root or root == p or root == q:
            return root
        
        # 递归左子树和右子树
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        
        # 如果左右子树递归结果都不为空，说明当前节点是p和q的LCA
        if left and right:
            return root
        
        # 如果左子树为空，则LCA在右子树；如果右子树为空，则LCA在左子树
        return left if left else right
```

在这段代码中，我们定义了一个名为`lowestCommonAncestor`的递归函数。函数首先检查当前节点是否为空或等于p或q中的一个。如果是，则返回当前节点。然后递归地在左右子树中寻找LCA。如果一个节点的左右子树递归调用都返回了非空结果，那么这个节点就是LCA。如果一个子树的递归调用为空，那么LCA必然在另一子树中。

该方法的时间复杂度为O(n)，其中n是树中的节点数，因为我们需要访问树中的每个节点。空间复杂度为O(h)，其中h是树的高度，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为链状，空间复杂度为O(n)。
# 0000 Binary Search Tree

---

在 LeetCode 或其他算法平台中提到的 "Binary Search Tree"（BST），即二叉搜索树，是一种特殊的二叉树。在这种树中，每个节点都满足以下性质：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 每个左右子树也必须是一个二叉搜索树。

**二叉搜索树的优点包括：**

- **有序性**：BST 的中序遍历结果是按升序排序的，这使得查找、插入和删除操作可以以对数时间复杂度进行，这是因为每一次比较都能排除一半的搜索空间。
- **动态数据集合**：BST 能够动态地增加和删除节点，可以适应不断变化的数据集合。

**二叉搜索树适用的场景：**

- **数据的有序遍历**：当需要按顺序访问元素时，BST 提供了非常有效的方法。
- **查找操作**：在 BST 中查找一个值是非常高效的，平均时间复杂度为 O(log n)。
- **插入和删除操作**：BST 使得数据的插入和删除操作也能以对数时间复杂度进行，这对于需要频繁更新的数据集合来说非常有用。

**二叉搜索树的缺点：**

- **不保证平衡**：普通的 BST 不保证树的平衡，最坏情况下（例如，插入已排序的数据）会退化成一个链表，此时所有操作的时间复杂度退化为 O(n)。
- **需要额外操作以保持平衡**：为了保持操作的对数时间复杂度，可能需要使用平衡二叉搜索树（如 AVL 树或红黑树）。

在实际应用中，二叉搜索树经常被用于实现高效的查找和排序操作，如数据库索引。在 LeetCode 上，与 BST 相关的题目通常会考察对这种数据结构的理解以及对其操作的实现。

# 0700 Search in a Binary Search Tree

---

**题目翻译：**

给定一个二叉搜索树（BST）的根节`root`点和一个整数`val`。

找到BST中节点值等于`val`的节点，并返回以该节点为根的子树。如果不存在这样的节点，返回`null`。

示例 1:
输入：root = [4,2,7,1,3], val = 2
输出：[2,1,3]

示例 2:
输入：root = [4,2,7,1,3], val = 5
输出：[]

约束条件：
- 树中的节点数在范围 [1, 5000] 内。
- 1 <= Node.val <= 10^7
- root 是二叉搜索树。
- 1 <= val <= 10^7

**解题思路和代码实现：**

由于给定的是一棵二叉搜索树，我们可以利用其性质（左子树的所有节点值小于根节点值，右子树的所有节点值大于根节点值）来进行高效搜索。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # 如果节点为空或找到值为val的节点，返回当前节点
        if not root or root.val == val:
            return root
        
        # 如果val小于当前节点值，搜索左子树
        if val < root.val:
            return self.searchBST(root.left, val)
        
        # 如果val大于当前节点值，搜索右子树
        return self.searchBST(root.right, val)
```

在这段代码中，我们定义了一个名为`searchBST`的递归函数。如果当前节点为空或节点的值等于要搜索的值`val`，则返回当前节点。否则，根据`val`与当前节点值的比较结果决定是继续在左子树中搜索还是在右子树中搜索。

该方法的时间复杂度为O(h)，其中h是树的高度，因为我们需要沿着树的高度最多访问一次。空间复杂度为O(h)，这是由于递归调用栈的深度取决于树的高度。在最坏的情况下，树的形态为链状，空间复杂度为O(n)，其中n是树中的节点数。

# 0450 Delete Node in a BST

---

**题目翻译：**

给定一个二叉搜索树（BST）的根节点引用和一个键值`key`，删除BST中键值为`key`的节点。返回BST的根节点引用（可能已更新）。

基本上，删除可以分为两个阶段：

1. 搜索要删除的节点。
2. 如果找到该节点，则删除该节点。

示例 1:
输入: root = [5,3,6,2,4,null,7], key = 3
输出: [5,4,6,2,null,null,7]
解释: 给定要删除的键值是3。所以我们找到值为3的节点并删除它。
一个有效的答案是[5,4,6,2,null,null,7]，如上图所示的BST。
请注意，另一个有效答案是[5,2,6,null,4,null,7]，也是可以接受的。

示例 2:
输入: root = [5,3,6,2,4,null,7], key = 0
输出: [5,3,6,2,4,null,7]
解释: 树中没有值为0的节点。

示例 3:
输入: root = [], key = 0
输出: []

约束条件：
- 树中的节点数在范围 [0, 10^4] 内。
- -10^5 <= Node.val <= 10^5
- 每个节点都有唯一的值。
- root 是一个有效的二叉搜索树。
- -10^5 <= key <= 10^5

进阶：你能以O(树高度)的时间复杂度解决它吗？

**解题思路和代码实现：**

要删除BST中的一个节点，我们可以通过递归的方式找到这个节点，然后根据这个节点的子节点的情况来处理删除操作。

1. 如果节点没有子节点，我们可以直接删除它。
2. 如果节点只有一个子节点，我们可以用其子节点替代它。
3. 如果节点有两个子节点，我们可以用它的后继节点（右子树中的最小节点）替代它，然后在右子树中删除后继节点。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None

        # 如果key小于根节点值，去左子树删除
        if key < root.val:
            root.left = self.deleteNode(root.left, key)
        # 如果key大于根节点值，去右子树删除
        elif key > root.val:
            root.right = self.deleteNode(root.right, key)
        else:
            # 如果当前节点是要删除的节点
            # 如果只有一个子节点或没有子节点
            if not root.left:
                return root.right
            if not root.right:
                return root.left
            # 如果有两个子节点，找到右子树中的最小节点，即后继节点
            min_larger_node = self.findMin(root.right)
            root.val = min_larger_node.val  # 用后继节点的值替换当前节点值
            root.right = self.deleteNode(root.right, root.val)  # 删除后继节点
        return root

    def findMin(self, node):
        while node.left:
            node = node.left
        return node
```

在这段代码中，`deleteNode`方法通过递归查找要删除的节点。如果找到了，就根据它的子节点情况来处理删除操作。如果节点有两个子节点，我们使用`findMin`辅助方法找到其右子树中的最小节点（后继节点），并用后继节点的值替换当前节点的值，然后递归地删除后继节点。

该方法的平均时间复杂度为O(log n)，其中n是树中的节点数，这是因为BST的性质导致我们每次都能排除一半的节点。在最坏的情况下，如果树呈线性结构，时间复杂度会退化为O(n)。空间复杂度为O(h)，h是树的高度，主要是递归调用栈的开销。在最坏的情况下，空间复杂度也是O(n)。
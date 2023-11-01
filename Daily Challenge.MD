# 458. Poor Pigs

题干：

有 `buckets` 个液体的桶，其中恰好有一个桶是有毒的。为了找出哪个是有毒的，你可以喂一些（可怜的）猪这个液体，以观察它们是否会死亡。不幸的是，你只有 `minutesToTest` 分钟的时间来确定哪个桶是有毒的。

你可以按照以下步骤喂猪：

1. 选择一些还活着的猪进行喂食。
2. 对于每一头猪，选择要喂它的桶。猪会同时消耗所有选定的桶，这不需要任何时间。每只猪都可以从任意数量的桶中饮食，而每个桶都可以被任意数量的猪饮食。
3. 等待 `minutesToDie` 分钟。在此期间，你不可以喂其他的猪。
4. 在 `minutesToDie` 分钟过后，任何喝了有毒桶里的液体的猪都会死亡，其他的都会存活下来。
5. 重复此过程，直到你用完了所有时间。

给定 `buckets`、`minutesToDie` 和 `minutesToTest`，返回在规定时间内确定哪个桶有毒所需的最小猪的数量。

分析：

这个问题实际上是一个关于信息编码的问题。考虑以下场景：如果我们只有1只猪和15分钟的时间，而且猪在吃毒草后15分钟会死，那么我们只能测试2桶。这是因为我们可以让猪吃第一桶，然后等待15分钟。如果它死了，第一桶是有毒的，否则第二桶是有毒的。

现在，考虑我们有2只猪和30分钟的时间。我们可以测试4桶。首先，让第一只猪吃第1和第2桶，让第二只猪吃第2和第3桶。如果只有第一只猪死了，那么第1桶是有毒的；如果只有第二只猪死了，那么第3桶是有毒的；如果两只猪都死了，那么第2桶是有毒的；如果都没死，那么第4桶是有毒的。

当我们可以测试的次数增加时，我们可以测试更多的桶。对于给定的`minutesToTest`和`minutesToDie`，我们可以测试的次数是`minutesToTest // minutesToDie`。

策略是将桶编码为基数为`(minutesToTest // minutesToDie + 1)`的数字。例如，如果`minutesToTest // minutesToDie`是2（意味着我们可以测试3次），那么我们可以使用3作为基数来编码桶。

每只猪可以用来确定编码的一个数字。因此，我们的目标是找到最少的猪，使得给定的基数下的编码可以涵盖所有的桶。

例如，考虑`buckets = 1000`。如果我们只可以测试一次，那么我们需要10只猪，因为2^10 = 1024。但是，如果我们可以测试两次，那么我们只需要5只猪，因为3^5 = 243（而这不够）。我们需要6只猪，因为3^6 = 729。如果我们可以测试三次，那么4^3 = 64，这还不够。但是，4^4 = 256，所以我们需要4只猪。

以下是相应的Python代码：

```python
class Solution:
    def poorPigs(self, buckets: int, minutesToDie: int, minutesToTest: int) -> int:
        pigs = 0
        base = minutesToTest // minutesToDie + 1
        
        while (base ** pigs) < buckets:
            pigs += 1
            
        return pigs
```

时间复杂度：O(log buckets) —— 实际上是确定需要多少只猪的时间。

空间复杂度：O(1)。

# 1356. Sort Integers by The Number of 1 Bits

### 题干
你被给予一个整数数组`arr`。按照数组中整数的二进制表示中`1`的数量进行升序排序，如果有两个或更多的整数具有相同数量的`1`，则需要按它们的升序进行排序。

排序后返回该数组。

### 分析
为了解决这个问题，我们首先需要为数组中的每个元素计算其二进制表示中的1的个数。然后，我们可以使用Python的内置排序功能，并提供一个自定义的排序键。这个键应该首先基于1的个数进行排序，然后再基于元素的实际值进行排序。

### 以下是相应的Python代码

```python
class Solution:
    def sortByBits(self, arr: List[int]) -> List[int]:
        # 定义一个内部函数来计算一个数的二进制表示中1的个数
        def count_bits(n):
            count = 0
            while n:
                count += n & 1
                n >>= 1
            return count
        
        # 对数组进行排序，首先根据1的个数，然后根据数值
        return sorted(arr, key=lambda x: (count_bits(x), x))
```

时间复杂度：$O(n \log n)$ —— n是数组的长度。这是由于我们使用了排序，而计算二进制中1的数量是O(log n)的操作。

空间复杂度：O(1) —— 如果我们不考虑输出所需的空间。

# 2433. Find The Original Array of Prefix Xor
### 题干
你被给予一个大小为n的整数数组pref。找到并返回满足以下条件的大小为n的数组arr：

pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]。
注意，^ 表示按位异或操作。

可以证明答案是唯一的。

### 分析
为了解决这个问题，我们首先要理解一个重要的XOR（异或）属性：

```
A ^ B = C
```

那么

```
A = B ^ C
```

和

```
B = A ^ C
```

这意味着如果我们有一个给定的XOR结果和其中的一个数，我们可以得到另一个数。

现在，对于给定的问题，我们有：

```
pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]
```

为了找到`arr[i]`，我们可以使用以下逻辑：

```
arr[i] = pref[i] ^ pref[i-1]
```

对于`arr[0]`，我们有：

```
arr[0] = pref[0]
```

使用上述逻辑，我们可以为给定的`pref`数组找到`arr`数组。

以下是相应的Python代码：

```python
class Solution:
    def findArray(self, pref: List[int]) -> List[int]:
        n = len(pref)
        arr = [0] * n
        arr[0] = pref[0]
        
        for i in range(1, n):
            arr[i] = pref[i] ^ pref[i-1]
        
        return arr
```

时间复杂度：O(n) —— n是数组的长度。我们只对数组进行了一次循环。

空间复杂度：O(1) —— 如果我们不考虑输出所需的空间。

# 501. Find Mode in Binary Search Tree

### 题目翻译：

题目：501. 在二叉搜索树中找众数

给定一个包含重复值的二叉搜索树（BST）的根节点，返回其中所有的众数（即，出现次数最多的元素）。

如果树中有多个众数，可以按任意顺序返回它们。

假设BST的定义如下：

- 一个节点的左子树只包含键值小于或等于该节点的键值的节点。
- 一个节点的右子树只包含键值大于或等于该节点的键值的节点。
- 左右子树也必须是二叉搜索树。

示例：

1. 输入：root = [1,null,2,2] 输出：[2]
2. 输入：root = [0] 输出：[0]

约束条件：

- 树中的节点数量在范围 [1, 10^4] 内。
- 节点的值在 -10^5 到 10^5 之间。

后续挑战：你能在不使用额外空间的情况下完成这个任务吗？（假设由于递归产生的隐式栈空间不计入总空间）

### 知识点：

这道题测试了以下知识点：
1. 二叉搜索树（BST）的特性。
2. 树的中序遍历。
3. 对遍历的结果进行分析，找到出现次数最多的元素。

### 解题方法：

我们可以使用中序遍历，因为BST的中序遍历是升序的。当我们进行中序遍历时，相同的值会连续出现。因此，我们可以在遍历过程中记录当前值的出现次数，并与之前的最大出现次数进行比较。

详细步骤：
1. 使用中序遍历树，获取到所有的节点值。
2. 在遍历过程中，计算每个值的出现次数。
3. 找出出现次数最多的值，并记录下来。

Python代码解答：
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        # 如果根为空，直接返回空列表
        if not root:
            return []

        # 当前值，当前值出现的次数，最大出现次数
        self.curVal, self.curCount, self.maxCount = None, 0, 0
        # 用于存储众数
        self.modes = []

        # 中序遍历函数
        def inorder(node):
            if not node:
                return
            # 遍历左子树
            inorder(node.left)
            # 如果当前值和上一个值相同，则计数增加
            if node.val == self.curVal:
                self.curCount += 1
            else:
                # 否则更新当前值和计数
                self.curVal = node.val
                self.curCount = 1
            # 更新最大出现次数，并根据需要更新众数列表
            if self.curCount > self.maxCount:
                self.maxCount = self.curCount
                self.modes = [self.curVal]
            elif self.curCount == self.maxCount:
                self.modes.append(self.curVal)
            # 遍历右子树
            inorder(node.right)

        # 调用中序遍历
        inorder(root)
        return self.modes
```

时间复杂度：O(n) - n 是树的节点数量。我们遍历了整棵树。

空间复杂度：O(1) - 不考虑递归所使用的隐式栈空间和结果列表。
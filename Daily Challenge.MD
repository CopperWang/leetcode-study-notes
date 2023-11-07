# 0458 Poor Pigs

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

# 1356 Sort Integers by The Number of 1 Bits

**题干**

你被给予一个整数数组`arr`。按照数组中整数的二进制表示中`1`的数量进行升序排序，如果有两个或更多的整数具有相同数量的`1`，则需要按它们的升序进行排序。

排序后返回该数组。

**分析**

为了解决这个问题，我们首先需要为数组中的每个元素计算其二进制表示中的1的个数。然后，我们可以使用Python的内置排序功能，并提供一个自定义的排序键。这个键应该首先基于1的个数进行排序，然后再基于元素的实际值进行排序。

**以下是相应的Python代码**

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

# 2433 Find The Original Array of Prefix Xor
**题干**

你被给予一个大小为n的整数数组pref。找到并返回满足以下条件的大小为n的数组arr：

pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]。
注意，^ 表示按位异或操作。

可以证明答案是唯一的。

**分析**

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

# 0501 Find Mode in Binary Search Tree

**题目翻译：**

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

**知识点：**

这道题测试了以下知识点：
1. 二叉搜索树（BST）的特性。
2. 树的中序遍历。
3. 对遍历的结果进行分析，找到出现次数最多的元素。

**解题方法：**

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

# 1503 Last Moment Before All Ants Fall Out of a Plank

**题目翻译：**

我们有一块长度为n单位的木板。一些蚂蚁在木板上行走，每只蚂蚁的速度为每秒1个单位。一些蚂蚁向左移动，其他蚂蚁向右移动。

当两只蚂蚁在某个点相遇时，它们会改变方向并继续移动。假设改变方向不需要额外的时间。

当蚂蚁在时间`t`到达木板的一端时，它会立即从木板上掉下来。

给定一个整数`n`和两个整数数组`left`和`right`，分别代表向左和向右移动的蚂蚁的位置，请返回最后一只蚂蚁从木板上掉下来的时刻。

**知识点与解题思路：**

这个问题的关键是理解当蚂蚁在木板上相遇并改变方向时，其实和它们互相穿过去没有区别（因为每只蚂蚁都是以相同的速度移动）。所以，要找到最后一只蚂蚁掉下木板的时刻，我们只需要考虑最远距离的蚂蚁需要多久才能到达木板的边缘。

具体步骤如下：

1. 遍历`left`数组，找到距离左边缘最远的蚂蚁，也就是`left`数组中的最大值，因为它们都在向左移动。
2. 遍历`right`数组，找到距离右边缘最远的蚂蚁，计算方法是`n`减去`right`数组中的最小值，因为它们都在向右移动。
3. 比较这两个值，更大的一个就是所有蚂蚁掉下木板的最后时刻。

Python代码解答：
```python
class Solution:
    def getLastMoment(self, n: int, left: List[int], right: List[int]) -> int:
        # 找到向左移动的蚂蚁中距离左边缘最远的蚂蚁
        max_left = max(left) if left else 0
        # 找到向右移动的蚂蚁中距离右边缘最远的蚂蚁，这里用n减去right数组中的最小值
        max_right = n - min(right) if right else 0
        # 返回两者之间的最大值，即为最后一只蚂蚁掉下木板的时刻
        return max(max_left, max_right)
```

时间复杂度：O(L+R) - 其中L是left数组的长度，R是right数组的长度。我们需要分别遍历left和right数组各一次来找到最大值和最小值。
空间复杂度：O(1) - 我们只使用了几个变量来存储结果和中间计算的值，与输入数组的长度无关。

# 1535 Find the Winner of an Array Game

---

**题目翻译：**

给定一个由不同整数构成的整数数组`arr`和一个整数`k`。

游戏将在数组的前两个元素（即`arr[0]`和`arr[1]`）之间进行。在游戏的每一轮中，我们比较`arr[0]`和`arr[1]`，较大的整数获胜并保持在位置0，较小的整数移动到数组的末尾。当一个整数连续赢得`k`轮游戏时，游戏结束。

返回将赢得游戏的整数。

保证游戏会有一个赢家。

示例 1:
输入: arr = [2,1,3,5,4,6,7], k = 2
输出: 5
解释: 让我们看一下游戏的各轮情况：
轮次 |       arr       | 赢家 | 连胜次数
  1   | [2,1,3,5,4,6,7] | 2      | 1
  2   | [2,3,5,4,6,7,1] | 3      | 1
  3   | [3,5,4,6,7,1,2] | 5      | 1
  4   | [5,4,6,7,1,2,3] | 5      | 2
因此我们可以看出将进行4轮游戏，5是赢家，因为它连续赢了2场游戏。

示例 2:
输入: arr = [3,2,1], k = 10
输出: 3
解释: 3将连续赢得前10轮。

约束条件:
- 2 <= arr.length <= 10^5
- 1 <= arr[i] <= 10^6
- `arr` 包含不同的整数。
- 1 <= k <= 10^9

**解题思路和代码实现：**

这个问题可以通过模拟比赛的过程来解决。我们可以维护两个变量：当前的获胜者`current_winner`和当前获胜者的连胜次数`current_win_count`。遍历数组，比较`current_winner`和数组中的下一个元素，更新获胜者和连胜次数。如果连胜次数达到`k`，或者`current_winner`赢得了所有对手，则游戏结束。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def getWinner(self, arr: List[int], k: int) -> int:
        current_winner = arr[0]  # 当前的获胜者初始化为第一个元素
        current_win_count = 0  # 当前获胜者的连胜次数初始化为0
        
        for i in range(1, len(arr)):
            if arr[i] > current_winner:
                # 如果遇到比当前获胜者更大的数，更新获胜者并重置连胜次数
                current_winner = arr[i]
                current_win_count = 1
            else:
                # 否则，当前获胜者连胜次数加1
                current_win_count += 1
            
            # 如果连胜次数达到k，或者已经赢得了所有对手，游戏结束
            if current_win_count == k or i == len(arr) - 1:
                return current_winner
        
        # 如果循环结束都没有返回，意味着当前获胜者赢得了所有对手
        return current_winner
```

在这段代码中，我们从数组的第二个元素开始遍历，每次都用`current_winner`和当前元素比较，根据比较结果更新`current_winner

# 1845 Seat Reservation Manager

---

**题目翻译：**

设计一个系统，管理从1到n编号的n个座位的预定状态。

实现 SeatManager 类：

- `SeatManager(int n)` 初始化一个将管理从1到n编号的n个座位的SeatManager对象。所有座位初始状态都是可用的。
- `int reserve()` 查找最小编号的未预定座位，预定它，并返回它的编号。
- `void unreserve(int seatNumber)` 释放给定编号的座位的预定状态。

示例 1:

输入
["SeatManager", "reserve", "reserve", "unreserve", "reserve", "reserve", "reserve", "reserve", "unreserve"]
[[5], [], [], [2], [], [], [], [], [5]]
输出
[null, 1, 2, null, 2, 3, 4, 5, null]

解释
SeatManager seatManager = new SeatManager(5); // 初始化一个有5个座位的SeatManager。
seatManager.reserve();    // 所有座位都可用，所以返回编号最小的座位，即1号座位。
seatManager.reserve();    // 可用座位有[2,3,4,5]，因此返回编号最小的座位，即2号座位。
seatManager.unreserve(2); // 释放2号座位，现在可用座位有[2,3,4,5]。
seatManager.reserve();    // 可用座位有[2,3,4,5]，因此返回编号最小的座位，即2号座位。
seatManager.reserve();    // 可用座位有[3,4,5]，因此返回编号最小的座位，即3号座位。
seatManager.reserve();    // 可用座位有[4,5]，因此返回编号最小的座位，即4号座位。
seatManager.reserve();    // 唯一可用的座位是5号座位，所以返回5。
seatManager.unreserve(5); // 释放5号座位，现在可用座位有[5]。

约束条件:

- 1 <= n <= 10^5
- 1 <= seatNumber <= n
- 每次调用 reserve 时，保证至少有一个未预定的座位。
- 每次调用 unreserve 时，保证 seatNumber 是已预定的座位。
- 总共最多会有 10^5 次对 reserve 和 unreserve 的调用。

**解题思路和代码实现：**

这个问题可以通过维护一个最小堆来实现。最小堆可以帮助我们以O(logn)的时间复杂度快速找到最小的未预定座位，并且当座位释放时，我们可以将其再次添加到最小堆中。

以下是具体的Python代码实现，包括详细的中文注释：

```python
import heapq  # 导入heapq库，以使用最小堆

class SeatManager:
    def __init__(self, n: int):
        # 初始化时，所有座位都是可用的，因此把它们全部添加到最小堆中
        self.available_seats = list(range(1, n + 1))
        heapq.heapify(self.available_seats)  # 将可用座位列表转换为最小堆

    def reserve(self) -> int:
        # 弹出并返回最小堆中的最小元素，即最小编号的未预定座位
        return heapq.heappop(self.available_seats)

    def unreserve(self, seatNumber: int) -> None:
        # 将释放的座位号添加回最小堆中
        heapq.heappush(self.available_seats, seatNumber)
```

在这段代码

中，我们创建了一个`SeatManager`类，其中有一个最小堆`available_seats`来存储所有可用的座位编号。`reserve`方法每次从最小堆中弹出并返回最小的元素，即最小编号的未预定座位。`unreserve`方法则将释放的座位编号再次推入最小堆中。

该方法的时间复杂度为O(logn)，主要是由于最小堆操作的时间复杂度。空间复杂度为O(n)，因为我们需要存储所有座位的可用状态。

# 1921 Eliminate Maximum Number of Monsters

---

**题目翻译：**

你在玩一个视频游戏，你需要保卫你的城市免受一群怪物的攻击，有`n`个怪物。你得到了一个大小为`n`的0索引整数数组`dist`，其中`dist[i]`是第`i`个怪物距离城市的初始距离（以千米为单位）。

这些怪物以恒定的速度向城市行进。每个怪物的速度在一个大小为`n`的整数数组`speed`中给出，其中`speed[i]`是第`i`个怪物的速度（以千米/分钟为单位）。

你有一种武器，一旦充满电，就可以消灭一个怪物。然而，这种武器需要一分钟的时间来充电。游戏开始时，武器已经充满电。

当任何怪物到达你的城市时，你就输了。如果一个怪物在武器完全充电的确切时刻到达城市，它算作失败，游戏结束，你不能使用你的武器。

返回在你输掉游戏前你可以消灭的最大怪物数量，如果你可以在他们到达城市前消灭所有怪物，则返回`n`。

示例 1:
输入：dist = [1,3,4], speed = [1,1,1]
输出：3
解释：
开始时，怪物的距离是[1,3,4]。你消灭了第一个怪物。
一分钟后，怪物的距离是[X,2,3]。你消灭了第二个怪物。
一分钟后，怪物的距离是[X,X,2]。你消灭了第三个怪物。
所有3个怪物都可以被消灭。

示例 2:
输入：dist = [1,1,2,3], speed = [1,1,1,1]
输出：1
解释：
开始时，怪物的距离是[1,1,2,3]。你消灭了第一个怪物。
一分钟后，怪物的距离是[X,0,1,2]，所以你输了。
你只能消灭1个怪物。

示例 3:
输入：dist = [3,2,4], speed = [5,3,2]
输出：1
解释：
开始时，怪物的距离是[3,2,4]。你消灭了第一个怪物。
一分钟后，怪物的距离是[X,0,2]，所以你输了。
你只能消灭1个怪物。

约束条件：

- n == dist.length == speed.length
- 1 <= n <= 10^5
- 1 <= dist[i], speed[i] <= 10^5

**解题思路和代码实现：**

这道题目可以通过计算每个怪物到达城市所需的时间，并将这些时间排序来解决。然后依次消灭每个怪物，直到没有时间消灭下一个怪物为止。

以下是具体的Python代码实现，包括详细的中文注释：

```python
class Solution:
    def eliminateMaximum(self, dist: List[int], speed: List[int]) -> int:
        # 计算每个怪物到达城市的时间，结果为浮点数，需要取上整
        time_to_city = [(dist[i] - 1) // speed[i] for i in range(len(dist))]
        
        # 对到达时间进行排序
        time_to_city.sort()
        
        # 初始化消灭怪物的数量
        eliminated = 0
        
        # 遍历每个怪物
        for i in range(len(time_to_city)):
            # 如果当前时间大于等于怪物到达城市的时间，则无法继续消灭
            if i > time_to_city[i]:
                break
            # 否则消灭怪物
            eliminated += 1
            
        return eliminated
```

在这段代码中，我们首先计算了每个怪物到达城市的时间，然后将这些时间排序。我们按照时间顺序遍历怪物，并在还有时间消灭怪物时消灭它们。一旦遇到无法在怪物到达之前消灭的情况，就停止消灭。

该方法的时间复杂度为O(nlogn)，主要由排序引起。空间复杂度为O(n)，用于存储怪物到达城市的时间数组。
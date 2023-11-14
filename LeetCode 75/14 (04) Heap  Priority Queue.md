# 0000 Heap / Priority Queue

---

在 LeetCode 或其他算法问题中，"Heap" 或 "Priority Queue"（优先队列）通常指的是一种特殊的数据结构，它允许以任意顺序插入元素，但删除元素的操作则按照元素的优先级进行，通常是按照元素的值大小。

**堆（Heap）的定义：**

- 堆是一种特殊的完全二叉树，所有节点都满足堆性质。
- 在一个最大堆中，每个节点的值都大于或等于其子节点的值；而在一个最小堆中，每个节点的值都小于或等于其子节点的值。
- 这种性质使得堆的根节点总是整个树中的最大值（最大堆）或最小值（最小堆）。

**优先队列（Priority Queue）的定义：**

- 优先队列是一个抽象数据类型，它类似于常规的队列或栈数据结构，但每个元素都有一个“优先级”。
- 优先队列中的元素按优先级出队，最高优先级的元素最先出队。
- 优先队列通常使用堆来实现，以便能够快速访问到优先级最高的元素。

**Heap / Priority Queue 适用的场景：**
1. **动态数据处理**：当你需要动态地获取数据集中的最大或最小元素时，堆/优先队列非常有用。
2. **Dijkstra算法**：在 Dijkstra 的单源最短路径算法中，优先队列用来选择下一个要访问的节点。
3. **Huffman编码**：在 Huffman 编码树的构建过程中，优先队列用来按频率排序节点。
4. **贪心算法**：许多贪心算法，如 Prim 算法求最小生成树，也会使用优先队列。
5. **事件模拟**：在模拟具有多个事件，每个事件都有一个特定的触发时间的系统时，优先队列可用来选择下一个将发生的事件。

**Python 中的 Heap / Priority Queue 实现：**
Python 中的 `heapq` 模块提供了堆操作的函数，用于实现优先队列。以下是使用 `heapq` 的一个示例：

```python
import heapq

# 创建一个空的最小堆
min_heap = []

# 向堆中添加元素
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 1)
heapq.heappush(min_heap, 2)

# 从堆中弹出最小元素
print(heapq.heappop(min_heap))  # 输出: 1
```

使用堆或优先队列可以在 O(log n) 时间内进行插入和删除最值操作，这使得它们在解决需要快速访问或更新集合中最值的问题时非常高效。

# 0215 Kth Largest Element in an Array

---

**题目翻译：**

给定一个整数数组 `nums` 和一个整数 `k`，返回数组中第 `k` 大的元素。

注意，它是按排序顺序中的第 `k` 大元素，而不是第 `k` 个不同的元素。

你能在不排序的情况下解决它吗？

示例 1:
输入: nums = [3,2,1,5,6,4], k = 2
输出: 5

示例 2:
输入: nums = [3,2,3,1,2,4,5,5,6], k = 4
输出: 4

约束条件：
- 1 <= k <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4

**解题思路和代码实现：**

这个问题可以通过使用堆或快速选择算法来解决。使用堆是一种简单的方式，但不是最优的方法。快速选择算法（基于快速排序的选择方法）可以在平均情况下提供更好的性能。以下是使用堆的解决方案：

```python
import heapq

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # 构建一个最小堆
        min_heap = []
        for num in nums:
            heapq.heappush(min_heap, num)
            # 保持堆的大小为k
            if len(min_heap) > k:
                heapq.heappop(min_heap)
        
        # 堆顶元素就是第k大的元素
        return min_heap[0]
```

在这段代码中，我们使用了Python内置的最小堆（通过`heapq`模块实现）来解决这个问题。我们遍历数组中的每个元素，将其添加到堆中，并保持堆的大小为 `k`。这样，当我们完成遍历时，堆顶元素（即最小元素）就是整个数组中第 `k` 大的元素。

这个方法的时间复杂度为O(n log k)，其中n是数组的大小。空间复杂度为O(k)，用于存储堆。 

# 2336 Smallest Number in Infinite Set

---

**题目翻译：**

你有一个包含所有正整数的集合 [1, 2, 3, 4, 5, ...]。

实现 SmallestInfiniteSet 类：

- SmallestInfiniteSet() 初始化 SmallestInfiniteSet 对象，使其包含所有正整数。
- int popSmallest() 移除并返回无限集合中最小的整数。
- void addBack(int num) 将一个正整数 num 添加回无限集合中，如果它还不在无限集合中的话。

示例 1:
输入
["SmallestInfiniteSet", "addBack", "popSmallest", "popSmallest", "popSmallest", "addBack", "popSmallest", "popSmallest", "popSmallest"]
[[], [2], [], [], [], [1], [], [], []]
输出
[null, null, 1, 2, 3, null, 1, 4, 5]

解释
SmallestInfiniteSet smallestInfiniteSet = new SmallestInfiniteSet();
smallestInfiniteSet.addBack(2);    // 2 已经在集合中，所以没有变化。
smallestInfiniteSet.popSmallest(); // 返回 1，因为 1 是最小的数字，并从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 2，并从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 3，并从集合中移除。
smallestInfiniteSet.addBack(1);    // 1 被添加回集合。
smallestInfiniteSet.popSmallest(); // 返回 1，因为 1 被添加回集合并且是最小的数字，并从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 4，并从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 5，并从集合中移除。

约束条件：

- 1 <= num <= 1000
- 最多总共会对 popSmallest 和 addBack 进行 1000 次调用。

**解题思路：**

可以使用一个变量来跟踪当前集合中最小的数字，和一个集合（或者列表）来跟踪被移除又添加回来的数字。

代码实现：
```python
class SmallestInfiniteSet:
    def __init__(self):
        self.min_val = 1  # 跟踪当前集合中最小的数字
        self.added_back = set()  # 跟踪被移除又添加回来的数字

    def popSmallest(self) -> int:
        # 如果有添加回来的数字，返回最小的那个，并从集合中移除
        if self.added_back:
            smallest = min(self.added_back)
            self.added_back.remove(smallest)
            return smallest
        # 否则，返回当前最小值并更新最小值
        smallest = self.min_val
        self.min_val += 1
        return smallest

    def addBack(self, num: int) -> None:
        # 只有当数字小于当前最小值且不在添加回来的集合中时，才添加回来
        if num < self.min_val and num not in self.added_back:
            self.added_back.add(num)
```

在这段代码中，我们维护一个变量 `min_val` 来跟踪集合中的最小值，以及一个集合 `added_back` 来跟踪被移除又添加回来的数字。`popSmallest` 方法首先检查 `added_back` 集合中是否有值，如果有则弹出最小的值，否则弹出 `min_val` 并更新它。`addBack` 方法则检查要添加回来的数字是否小于 `min_val` 且不在 `added_back` 中，如果是则将其添加到 `added_back` 中。

时间复杂度分析：
- `popSmallest`：O(1) 或者 O(logN)，取决于 `added_back` 的数据结构，如果是排序列表，则为 O(logN)。
- `addBack`：O(1)，因为添加到集合的操作是常数时间复杂度。

空间复杂度分析：
- O(N)，N 是添加回来的数字的数量，这是 `added_back` 的最大大小。

# 2542 Maximum Subsequence Score

---





Total Cost to Hire K Workers


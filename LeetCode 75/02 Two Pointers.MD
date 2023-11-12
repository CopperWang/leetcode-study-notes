# 0000 Two Pointers

---

在 LeetCode 或其他算法问题讨论中，"Two Pointers"（双指针技巧）是一种常用的解题方法，特别是在处理数组或链表时。

**双指针技巧的定义：**
- 双指针技巧涉及同时使用两个指针（索引或迭代器）来遍历数据结构（通常是线性结构如数组或链表）。
- 这两个指针可以以相同的方向移动（称为“快慢指针”），也可以向相反方向移动（称为“对撞指针”）。

**双指针技巧适用的场景：**
1. **数组或字符串中的对撞指针**：在有序数组中寻找两个数的和等于特定值，或者在字符串中寻找回文。
2. **链表中的快慢指针**：检测链表中的循环，找到链表的中间节点。
3. **滑动窗口**：在数组或字符串中查找连续子序列的特定属性（如总和、最大值或最小值）。
4. **移除重复元素**：从有序数组中移除重复项。
5. **合并两个有序数组或链表**：在归并排序中或将两个有序链表合并为一个有序链表时使用。

双指针技巧的优点是可以减少时间复杂度（从 O(n^2) 减少到 O(n)），并且可以在不需要额外空间的情况下解决问题，从而达到空间复杂度为 O(1)。

**双指针技巧的示例（在有序数组中寻找两数之和）：**
```python
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    while left < right:
        s = numbers[left] + numbers[right]
        if s == target:
            return [left + 1, right + 1]  # 返回的是索引+1，因为LeetCode要求的是从1开始的索引
        elif s < target:
            left += 1  # 和太小，移动左指针
        else:
            right -= 1  # 和太大，移动右指针
    return [-1, -1]  # 如果没有找到结果
```

在这个例子中，`left` 是左指针，`right` 是右指针，它们分别从数组的两端开始向中间移动，直到找到一对数的和等于 `target`，或者指针相遇。这种技巧常用于解决类似的有序数组问题。在 LeetCode 上，许多关于数组和链表的问题都可以用双指针技巧来优化解法。

# 0283 Move Zeroes

---

**题目翻译：**

给定一个整数数组 `nums`，将所有的 `0` 移动到它的末尾，同时保持非零元素的相对顺序。

注意，你必须在原地做到这一点，不要复制数组。

**知识点与解题思路：**

这道题考察的是数组的原地操作。解决方案是使用双指针技术来同时遍历数组并进行元素的交换。

1. 使用两个指针：一个指针（例如，`non_zero_pos`）用于跟踪最新找到的非零元素应该存放的位置，另一个指针（例如，`curr`）用于遍历数组。
2. 当在`curr`位置找到一个非零元素时，将其与`non_zero_pos`位置的元素交换，然后增加`non_zero_pos`。
3. 最终，所有的0都会被推到数组的末尾，而非零元素则保持它们的相对顺序。

Python代码解答：
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        non_zero_pos = 0  # 用于记录下一个非零元素应该放的位置
        
        # 遍历数组
        for curr in range(len(nums)):
            if nums[curr] != 0:  # 如果当前元素不是0
                # 交换当前元素和non_zero_pos位置的元素
                nums[non_zero_pos], nums[curr] = nums[curr], nums[non_zero_pos]
                non_zero_pos += 1  # 移动到下一个位置
```

时间复杂度：O(n) - 我们遍历了数组一次。
空间复杂度：O(1) - 只使用了常数级别的额外空间。

# 0392 Is Subsequence

---

**题目翻译：**

给定两个字符串 `s` 和 `t`，如果 `s` 是 `t` 的子序列，返回 `true`，否则返回 `false`。

字符串的子序列是从原始字符串形成的新字符串，可以删除一些（可以没有）字符，而不改变剩余字符的相对位置。 (例如，`"ace"`是`"abcde"`的子序列，而`"aec"`不是)。

**知识点与解题思路：**

这道题目考察的是双指针的技巧。我们可以使用两个指针同时遍历s和t，确保s中的每个字符按顺序在t中出现。

1. 使用两个指针：一个指针`i`用于遍历字符串`s`，另一个指针`j`用于遍历字符串`t`。
2. 当`s[i]`等于`t[j]`时，移动指针`i`到下一个位置。
3. 如果`s`中的每个字符都按顺序在`t`中出现，那么最终`i`将等于`s`的长度，返回`True`。否则返回`False`。

Python代码解答：
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i, j = 0, 0
        
        while i < len(s) and j < len(t):
            if s[i] == t[j]:  # 如果两个字符相等，移动指针i到下一个位置
                i += 1
            j += 1  # 无论如何，都要移动指针j

        # 如果i等于s的长度，说明s中的每个字符都按顺序在t中出现
        return i == len(s)
```

时间复杂度：O(n) - 其中n是字符串`t`的长度。
空间复杂度：O(1) - 只使用了常数级别的额外空间。

后续问题解答：
如果有很多输入字符串s，例如s1，s2，...，sk，并且你想逐一检查t是否有它的子序列。在这种情况下，为了提高效率，你可能需要预处理字符串`t`，构建一个位置索引，记录每个字符在`t`中出现的所有位置。然后，对于每个输入的`s`，使用二分查找方法检查s中的每个字符是否按顺序在t中出现。

# 0011 Container With Most Water

---

**题目翻译：**

给定一个整数数组`height`，长度为`n`。有`n`条垂直线，使得第`i`条线的两个端点是`(i, 0)`和`(i, height[i])`。

找到两条线，它们与`x`轴一起形成一个容器，使得该容器可以容纳最多的水。

返回容器可以存储的最大水量。

注意你不能倾斜容器。

**知识点与解题思路：**

这道题目是典型的双指针题目。为了找到可以容纳最多水的容器，我们可以使用以下策略：

1. 初始化两个指针`left`和`right`，分别指向数组的开始和结束。
2. 初始化一个变量`max_area`来存储最大的面积。
3. 在`left`小于`right`的情况下，进行以下操作：
   - 计算当前容器的面积：`min(height[left], height[right]) * (right - left)`。
   - 更新`max_area`。
   - 如果`height[left]`小于`height[right]`，则移动`left`指针。否则，移动`right`指针。

这种方法的思想是：始终尝试移动较短的线条，希望找到一个更高的线条，从而增加容器的面积。

Python代码解答：
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        max_area = 0

        while left < right:
            # 计算当前容器的面积
            area = min(height[left], height[right]) * (right - left)
            max_area = max(max_area, area)
            
            # 移动指针
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

时间复杂度：O(n) - 只遍历数组一次。
空间复杂度：O(1) - 只使用了常数级别的额外空间。

# 1679 Max Number of K-Sum Pairs

---

**题目翻译：**

你得到一个整数数组 nums 和一个整数 k。

在一次操作中，你可以从数组中选择两个数，它们的和等于 k，并从数组中移除它们。

返回你可以在数组上执行的最大操作数。

**知识点与解题思路：**

我们可以使用哈希表来帮助解决这个问题。思路如下：

1. 初始化一个哈希表来存储每个数字出现的次数。
2. 初始化一个变量 `operations` 为 0，用于存储总的操作数。
3. 遍历数组中的每个数字 `num`：
   - 计算 `target = k - num`。
   - 如果 `target` 在哈希表中出现过，那么：
      - 增加操作数。
      - 减少哈希表中 `target` 的数量。
   - 否则，将 `num` 添加到哈希表中，并增加其数量。

Python代码解答：
```python
class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        # 初始化哈希表和操作数
        hash_map = {}
        operations = 0
        
        # 遍历数组中的每个数字
        for num in nums:
            target = k - num
            
            # 如果target在哈希表中，执行一次操作并更新哈希表
            if hash_map.get(target, 0) > 0:
                operations += 1
                hash_map[target] -= 1
            else:
                # 否则，将当前数字添加到哈希表中
                if num not in hash_map:
                    hash_map[num] = 0
                hash_map[num] += 1
        
        return operations
```

时间复杂度：O(n) - 我们只遍历数组一次。
空间复杂度：O(n) - 在最坏的情况下，哈希表可能需要存储数组中的所有数字。